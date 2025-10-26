[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1計算サービスAPIドキュメント
> バージョン: v1.0
> 更新日: 2025-10-26
> ベースURL: `https://xxxxxx.com`

---

## 1. 認証方法

`X-API-Token` を取得するには、申請フォームから申請してください：[https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

すべてのリクエストには、ヘッダーに以下を含める必要があります:

| ヘッダーキー | タイプ | 必須 | 説明 |
|---|---|---|---|
| X-API-Token | string | ✅ | サービスプロバイダーから申請したAPI-Token（注意：このトークンは、固定回数使用後に永久に無効になるため、変数として設定してください）。 |
| X-Response-Format | string | ✅ | 固定値 `wrapped` |

---

## 2. API概要

| 機能 | メソッド | パス | 説明 |
|---|---|---|---|
| 計算タスクの送信 | POST | `/api/sfd/calc` | 構造番号を送信し、タスクトークンを返します。 |
| 残り回数の照会 | GET | `/api/sfd/token-remaining-times`| 現在のAPI-Keyの残り呼び出し回数を照会します。 |

---

## 3. 詳細API

### 3.1 計算タスクの送信
**POST** `/api/sfd/calc`

#### リクエスト例
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--data '{
    "structure": "6536xxxxx"
}'
```

| フィールド | タイプ | 必須 | 説明 |
|---|---|---|---|
| structure | string | ✅ | 112ビットまたは158ビットのリクエストコード |

#### レスポンス
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

| フィールド | タイプ | 説明 |
|---|---|---|
| token | string | SFD1ロック解除コード、モジュールSFD1のロックを解除するために使用されます。 |
| remaining | number | このAPI-Keyが呼び出し可能な残り回数。 |


### 3.2 残り回数の照会
**GET** `/api/sfd/token-remaining-times`

#### リクエスト例
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### レスポンス
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| フィールド | タイプ | 説明 |
|---|---|---|
| remaining | number | このAPI-Keyが呼び出し可能な残り回数。 |