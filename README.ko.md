[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1 계산 서비스 API 문서
> 버전: v1.0
> 업데이트: 2025-10-26
> BaseURL: `https://xxxxxx.com`

---

## 1. 인증 방식

`X-API-Token`을 받으려면 신청서를 통해 신청하십시오: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

모든 요청은 헤더에 다음을 포함해야 합니다:

| 헤더 키 | 타입 | 필수 | 설명 |
|---|---|---|---|
| X-API-Token | string | ✅ | 서비스 제공업체에서 신청한 API-Token (참고: 이 토큰은 고정된 횟수 사용 후 영구적으로 무효화되므로 변수로 설정하십시오). |
| X-Response-Format | string | ✅ | 고정값 `wrapped` |

---

## 2. API 개요

| 기능 | 메소드 | 경로 | 설명 |
|---|---|---|---|
| 계산 작업 제출 | POST | `/api/sfd/calc` | 구조 번호를 제출하고 작업 토큰을 반환합니다. |
| 남은 횟수 조회 | GET | `/api/sfd/token-remaining-times`| 현재 API-Key의 남은 호출 횟수를 조회합니다. |

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
--data 
{
    "structure": "6536xxxxx"
}
```

| 필드 | 타입 | 필수 | 설명 |
|---|---|---|---|
| structure | string | ✅ | 112비트 또는 158비트 요청 코드 |

#### 응답
```
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
|---|---|---|---|
| token | string | SFD1 잠금 해제 코드, 모듈 SFD1을 잠금 해제하는 데 사용됩니다. |
| remaining | number | 이 API-Key가 호출할 수 있는 남은 횟수. |


### 3.2 남은 횟수 조회
**GET** `/api/sfd/token-remaining-times`

#### 요청 예시
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### 응답
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| 필드 | 타입 | 설명 |
|---|---|---|---|
| remaining | number | 이 API-Key가 호출할 수 있는 남은 횟수. |