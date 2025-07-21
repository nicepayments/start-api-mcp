# JAVA NICEPAY MCP 프롬프트 가이드
- [JAVA NICEPAY MCP 프롬프트 가이드](#java-nicepay-mcp-프롬프트-가이드)
    - [API 서버 도메인 구분](#api-서버-도메인-구분)
    - [샌드박스 결제창 호출 + 결제 승인](#샌드박스-결제창-호출--결제-승인)

---

### API 서버 도메인 구분

> 운영환경이 필요한 경우 프롬프트에서 API End-point를 변경하세요.

| 구분       | API 도메인 URL                         | 설명                      |
|------------|-----------------------------------------|---------------------------|
| 샌드박스   | `https://sandbox-api.nicepay.co.kr`     | 개발 및 테스트 전용 환경 |
| 운영(Live) | `https://api.nicepay.co.kr`             | 실제 결제 처리 환경      |

---

### 샌드박스 결제창 호출 + 결제 승인

```text
[결제창 요청 정보]
create_payment_window 툴을 반드시 사용
approve_payment 툴을 반드시 사용

[클라이언트 정보]
clientId: "발급받은 clientId를 입력하세요"
secretKey: "발급받은 secretKey를 입력하세요"

[결제 정보]
method: "card"
orderId: 랜덤 UUID
amount: 1004
goodsName: "결제 TEST"
buyerName: "홍길동"
buyerTel: "01000000000"
buyerEmail: "test@test.com"
returnUrl: "http://localhost:8080/payment/result"

[요청사항]
1. Spring Boot + Gradle 기반 전체 서버 코드를 생성해줘 (하나의 Gradle 프로젝트 구조로)
    - `src/main/java/...`에 `Application.java`, `Controller` 포함
    - `src/main/resources/templates` 에 결제창 페이지 구성
2. `/` 경로에서는 결제 버튼이 있는 HTML + JS 페이지를 렌더링해줘
    - create_payment_window MCP Tool을 반드시 사용해서 AUTHNICE JS SDK (`https://pay.nicepay.co.kr/v1/js/`) 포함
    - 버튼 클릭 시 `AUTHNICE.requestPay()` 호출
    - 결제 실패 시 `fnError` 콜백에서 alert 호출
3. returnUrl은 `http://localhost:8080/payment/result` 로 설정하고,
    - 서버는 `POST /payment/result` 요청을 받을 수 있어야 함
    - Java에서는 `@PostMapping("/payment/result")` + `@RequestParam("tid")` 또는 `@ModelAttribute`로 `tid` 추출
4. 승인 API 호출:
    - approve_payment 툴을 반드시 사용하고 필수값은 반드시 셋팅해줘 (amount 필수)
    - Basic Auth = base64(clientId:secretKey)
    - 승인 API는 반드시 `POST https://sandbox-api.nicepay.co.kr/v1/payments/{tid}` 로 호출할 것 (절대 `/approve` 붙이면 안됨)
    - 승인 성공 시 브라우저에 JSON 전체 응답을 `<pre>` 태그로 출력
5. 프로젝트 최상위에 `build.gradle` 파일을 포함해서 다음 의존성 설정:
    - `spring-boot-starter-web`
    - `spring-boot-starter-thymeleaf` (또는 HTML 직접 반환)
    - `org.apache.httpcomponents:httpclient` 또는 `Spring RestTemplate`
```