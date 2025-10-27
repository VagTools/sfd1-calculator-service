[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1 계산 서비스 API 문서
&gt; 버전: v1.0  
&gt; 업데이트: 2025-10-26  
&gt; BaseURL: `https://xxxxxx.com`

---

## 1. 인증 방식

`X-API-Token`을 받으려면 신청서를 통해 신청하십시오: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

모든 요청은 헤더에 다음을 포함해야 합니다:

| 헤더 키 | 타입 | 필수 | 설명 |
|---|---|---|---|
| X-API-Token | string | ✅ | 서비스 제공업체에서 신청한 API-Token (참고: 이 토큰은 고정된 횟수 사용 후 영구적으로 무효화되므로 변수로 설정하십시오). |
| X-Response-Format | string | ✅ | 고정값 `wrapped` |
| Cookie | string | ✅ | 응답 언어를 지정하는 데 사용됩니다. 예를 들어, 중국어 응답을 얻으려면 `language=ko_KR`을 설정하십시오. 지원되는 언어는 문서 상단에 나열되어 있습니다. |

---

## 2. API 개요

| 기능 | 메소드 | 경로 | 설명 |
|---|---|---|---|
| 계산 작업 제출 | POST | `/api/sfd/calc` | 구조 번호를 제출하고 작업 토큰을 반환합니다. |
| 남은 횟수 조회 | GET | `/api/sfd/token-remaining-times`| 현재 API-Key의 남은 호출 횟수를 조회합니다. |
| 계산 로그 내보내기 | GET | `/api/sfd/calc-logs` | 현재 API-Key의 모든 계산 로그를 파일로 내보냅니다. |

---

## 3. 상세 API

### 3.1 계산 작업 제출
**POST** `/api/sfd/calc`

#### 요청 예시
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--cookie 'language=ko_KR' \
--data 
{
    "structure": "6536xxxxx"
}
```

| 필드 | 타입 | 필수 | 설명 |
|---|---|---|---|
| structure | string | ✅ | 112비트 또는 158비트 요청 코드 |

#### 응답
```json
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "token": "7Fxxxxxxxxx",
    "remaining": 80
  }
}
```

| 필드 | 타입 | 설명 |
|---|---|---|
| token | string | SFD1 잠금 해제 코드, 모듈 SFD1을 잠금 해제하는 데 사용됩니다. |
| remaining | number | 이 API-Key가 호출할 수 있는 남은 횟수. |


### 3.2 남은 횟수 조회
**GET** `/api/sfd/token-remaining-times`

#### 요청 예시
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=ko_KR'
```

#### 응답
```json
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| 필드 | 타입 | 설명 |
|---|---|---|
| remaining | number | 이 API-Key가 호출할 수 있는 남은 횟수. |


### 3.3 계산 로그 내보내기


**GET** `/api/sfd/calc-logs`



이 엔드포인트는 제공된 `X-API-Token`과 관련된 모든 계산 로그를 내보냅니다. 일반적으로 스프레드시트 형식(예: Excel/CSV)으로 파일 다운로드를 트리거합니다.



#### 요청 예시


```bash
curl --location '{{BaseURL}}/api/sfd/calc-logs' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=ko_KR'
```



#### 응답


응답은 JSON 객체가 아닌 파일 다운로드입니다. 브라우저 또는 클라이언트가 다운로드 프로세스를 처리합니다.



---




## 4. 오류 코드



요청 실패 시 서비스는 응답 본문의 `code` 필드에 다음 비즈니스 오류 코드를 반환합니다.



| 비즈니스 코드 | HTTP 상태 | 설명 (messages_ko_KR.properties에서) |
|---|---|---|
| 40300 | 403 | API 토큰이 비활성화되었습니다. |
| 40301 | 403 | API 토큰이 만료되었습니다. |
| 40001 | 400 | API 토큰의 남은 수량이 부족합니다. |
| 40003 | 400 | API 토큰에 바인딩된 GRP 정보가 없습니다. |
| 40004 | 400 | SOAP 구조로 변환할 수 없습니다. |
| 40002 | 400 | 구조 길이가 잘못되었습니다. 길이는 {0} 또는 {1}이어야 합니다. |
| 40002 | 400 | 잘못된 요청 인수 예외입니다. |
| 40003 | 403 | 접근이 금지되었습니다. |
| 50001 | 500 | 알 수 없는 시스템 예외입니다. |
