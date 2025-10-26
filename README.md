[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1 Calculation Service API Documentation
> Version: v1.0
> Updated: 2025-10-26
> BaseURL: `https://xxxxxx.com`

---

## 1. Authentication

To obtain an `X-API-Token`, please apply through our application form: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

All requests must include the following in the Header:

| Header Key | Type | Required | Description |
|---|---|---|---|
| X-API-Token | string | ✅ | The API-Token applied for from the service provider (Note: Please set this Token as a variable, as it will become permanently invalid after a fixed number of uses). |
| X-Response-Format | string | ✅ | Fixed value `wrapped` |

---

## 2. API Overview

| Function | Method | Path | Description |
|---|---|---|---|
| Submit Calculation Task | POST | `/api/sfd/calc` | Submit a structure number and return a task token. |
| Query Remaining Times | GET | `/api/sfd/token-remaining-times`| Query the remaining call times for the current API-Key. |

---

## 3. Detailed APIs

### 3.1 Submit Calculation Task
**POST** `/api/sfd/calc`

#### Request Example
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--data '{
    "structure": "6536xxxxx"
}'
```

| Field | Type | Required | Description |
|---|---|---|---|
| structure | string | ✅ | 112-bit or 158-bit request code |

#### Response
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

| Field | Type | Description |
|---|---|---|
| token | string | SFD1 unlock code, used to unlock module SFD1. |
| remaining | number | The remaining number of times this API-Key can be called. |


### 3.2 Query Remaining Times
**GET** `/api/sfd/token-remaining-times`

#### Request Example
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### Response
```json
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| Field | Type | Description |
|---|---|---|
| remaining | number | The remaining number of times this API-Key can be called. |