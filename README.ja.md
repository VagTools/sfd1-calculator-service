[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1計算サービスAPIドキュメント
&gt; バージョン: v1.0  
&gt; 更新日: 2025-10-26  
&gt; ベースURL: `https://xxxxxx.com`

---

## 1. 認証方法

`X-API-Token` を取得するには、申請フォームから申請してください：[https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

すべてのリクエストには、ヘッダーに以下を含める必要があります:

| ヘッダーキー | タイプ | 必須 | 説明 |
|---|---|---|---|
| X-API-Token | string | ✅ | サービスプロバイダーから申請したAPI-Token（注意：このトークンは、固定回数使用後に永久に無効になるため、変数として設定してください）。 |
| X-Response-Format | string | ✅ | 固定値 `wrapped` |
| Cookie | string | ✅ | 応答言語を指定するために使用します。例えば、中国語の応答を得るには `language=zh_CN` を設定します。サポートされている言語はドキュメントの冒頭に記載されています。 |

---

## 2. API概要

| 機能 | メソッド | パス | 説明 |
|---|---|---|---|
| 計算タスクの送信 | POST | `/api/sfd/calc` | 構造番号を送信し、タスクトークンを返します。 |
| 残り回数の照会 | GET | `/api/sfd/token-remaining-times`| 現在のAPI-Keyの残り呼び出し回数を照会します。 |
| 計算ログのエクスポート | GET | `/api/sfd/calc-logs` | 現在のAPI-Keyのすべての計算ログをファイルとしてエクスポートします。 |

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
--cookie 'language=ja_JP' \
--data 
    "{
        \"structure\": \"6536xxxxx\"\n
     }"
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
--header 'X-Response-Format: wrapped' \
--cookie 'language=ja_JP'
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

### 3.3 計算ログのエクスポート

**GET** `/api/sfd/calc-logs`



このエンドポイントは、提供された `X-API-Token` に関連するすべての計算ログをエクスポートします。通常はスプレッドシート形式（例：Excel/CSV）でファイルダウンロードをトリガーします。



#### リクエスト例

```bash

curl --location '{{BaseURL}}/api/sfd/calc-logs' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=ja_JP'
```



#### レスポンス

応答はJSONオブジェクトではなく、ファイルのダウンロードになります。ブラウザまたはクライアントがダウンロードプロセスを処理します。



---



## 4. エラーコード



リクエストが失敗した場合、サービスは応答ボディの `code` フィールドに以下のビジネスエラーコードを返します。



| ビジネスコード | HTTPステータス | 説明 (messages_ja_JP.propertiesより) |
|---|---|---|
| 40300 | 403 | APIトークンは無効化されています。 |
| 40301 | 403 | APIトークンは期限切れです。 |
| 40001 | 400 | APIトークンの残量が不足しています。 |
| 40003 | 400 | APIトークンにGRP情報がバインドされていません。 |
| 40004 | 400 | SOAP構造に変換できません。 |
| 40002 | 400 | 構造の長さが不正です。長さは {0} または {1} である必要があります。 |
| 40002 | 400 | 不正なリクエストパラメータ例外です。 |
| 40003 | 403 | アクセスが拒否されました。 |
| 50001 | 500 | 不明なシステム例外です。 |
