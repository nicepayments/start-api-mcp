## 🤖 NICEPAY MCP Tool 프롬프트 가이드 (@nicepay/start-api-mcp)

- NICEPAY의 `FOR START API`를 보다 쉽게 연동할 수 있도록 **[Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction)** 기반 프롬프트 사용 가이드를 제공합니다.
- MCP Tool을 활용하면 LLM과 대화하듯 자연어로 결제 요청, 승인, 취소, 현금영수증 발급까지 처리할 수 있습니다.  
- LLM이 적절하게 Tool을 호출할 수 있도록, 아래의 [개발언어별 프롬프트 예시](#-개발언어별-프롬프트-예시)를 반드시 확인해주세요.

💬 [개발언어별 프롬프트 예시](#-개발언어별-프롬프트-예시) 보기
- 🟩 [Node.js](prompts/nodejs.md)
- 🐍 [Python](prompts/python.md)
- ☕ [Java Spring Boot](prompts/java-spring.md)
---

### ⚙️ MCP 서버 등록 방법
> 진행 전 [Node.js](https://nodejs.org/) **20.0.0 이상** 설치가 필요합니다.

![MCP 등록 예시](./assets/nicepay-mcp-register.gif)

- 아래 설정을 `mcpServers` 항목에 추가한 뒤, 개발 도구를 재시작합니다. (Cursor etc..)
- 재시작 후 MCP Tool이 정상적으로 노출되는지 확인합니다.

   ```json
    "mcpServers": {
      "nicepay-start-api-mcp": {
        "command": "npx",
        "args": [
          "-y",
          "@nicepay/start-api-mcp@latest"
        ]
      }
    }
   ```

> Tool loading이 되지 않는 경우 npx -y @nicepay/start-api-mcp@latest 를 CLI에서 실행 후 문제가 있는지 체크 해주세요.

---

### 📂 개발언어별 프롬프트 예시

- 🟩 [Node.js](prompts/nodejs.md)
- 🐍 [Python](prompts/python.md)
- ☕ [Java Spring Boot](prompts/java-spring.md)


---

### 🔑 인증 정보(Credentials) 확인 절차

1. NICEPAY 관리자 사이트 접속 → [https://start.nicepay.co.kr](https://start.nicepay.co.kr)  
2. 로그인 후 좌측 메뉴에서 **`개발정보`** 클릭  
3. **KEY 정보** 항목에서 아래 정보를 확인하거나 신규 발급 가능:
   - **Client Key / Secret Key**: API 인증용 고유 키  
   - 신규 발급 시 두 키가 **함께 생성**되며, **Base64 인코딩**하여 사용

> **운영용 키**는 외부에 노출되지 않도록 주의하세요.  

---

### 🌐 API 서버 도메인 구분

NICEPAY는 테스트와 운영 환경을 **도메인 기준으로 완전히 분리**하여 제공합니다.

| 구분       | API 도메인 URL                         | 설명                      |
|------------|-----------------------------------------|---------------------------|
| 샌드박스   | `https://sandbox-api.nicepay.co.kr`     | 개발 및 테스트 전용 환경 |
| 운영(Live) | `https://api.nicepay.co.kr`             | 실제 결제 처리 환경      |

---

### 📝 NICEPAY MCP Tool 목록

| Tool 이름                          | API 이름                         | 설명 |
|-----------------------------------|----------------------------------|------|
| create_payment_window             | 결제창 호출                      | NICEPAY JS SDK를 사용해 브라우저에서 결제창을 호출합니다. |
| approve_payment                   | 결제 승인                        | 결제 인증 후 거래를 승인 처리합니다. |
| cancel_payment                    | 결제 취소                        | 전체 또는 부분 결제를 취소합니다. |
| create_billing_key               | 빌링키 발급                      | 암호화된 카드 정보를 이용해 정기결제용 빌링키를 발급합니다. |
| approve_billing_payment          | 빌링키 결제 승인                 | 발급된 빌링키를 통해 정기결제를 승인합니다. |
| expire_billing_key               | 빌링키 만료                      | 더 이상 사용하지 않는 빌링키를 만료(삭제) 처리합니다. |
| create_cash_receipt              | 현금영수증 발급                  | 현금 결제 건에 대한 현금영수증을 발급합니다. |
| cancel_cash_receipt              | 현금영수증 취소                  | 이미 발급된 현금영수증을 취소합니다. |
| get_cash_receipt_status          | 현금영수증 상태 조회             | 현금영수증의 처리 상태를 조회합니다. |
| find_payment_by_order_id         | 거래 내역 조회                   | 주문번호를 기준으로 결제 내역을 조회합니다. |
| get_terms                        | 약관 조회                        | 전자금융거래, 개인정보 수집 등 관련 약관 내용을 조회합니다. |
| get_card_promotions              | 카드 이벤트 조회                 | 카드사의 포인트 적립 및 무이자 혜택 정보를 조회합니다. |
| list_interest_free_installments | 무이자 할부 정보 조회           | 카드사별 무이자 할부 조건을 조회합니다. |
| search_nicepay_qna | QnA 유사질문 검색 | NICEPAY 연동과 관련된 FAQ 데이터셋을 기반으로 조회하여, 유사한 QnA 응답을 반환합니다. |
> `create_payment_window` Tool은 브라우저 기반으로 실행되는 클라이언트용 Tool입니다. 서버에서는 사용할 수 없습니다.


---

### 📦 설치 및 정보

- 📦 NPM: [@nicepay/start-api-mcp](https://www.npmjs.com/package/@nicepay/start-api-mcp)
- 🧑‍💻 GitHub: [https://github.com/nicepayments](https://github.com/nicepayments)

---

### ⚖️ 면책 조항 (Disclaimer)
`@nicepay/start-api-mcp`는 AI가 자동 생성한 콘텐츠를 제공합니다. 해당 정보는 부정확하거나 불완전할 수 있으며, 참고용 으로만 사용해야 합니다. 사용자는 본 도구를 통해 제공되는 정보를 신뢰하기 전에 반드시 독립적으로 검증해야 하며, 이 정보를 기반으로 한 모든 결정 및 행동의 책임은 전적으로 사용자 본인에게 있습니다. NICEPAY는 생성된 정보의 정확성 또는 완전성을 보장하지 않으며, 이를 사용함으로 인해 발생한 직간접적인 손해나 책임에 대해 어떠한 법적 책임도 지지 않습니다.