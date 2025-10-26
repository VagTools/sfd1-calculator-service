[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1 计算服务 API 文档  
&gt; 版本：v1.0  
&gt; 更新：2025-10-26  
&gt; BaseURL：`https://xxxxxx.com`

---

## 1. 鉴权方式

如需获取 `X-API-Token`，请通过我们的申请表进行申请：[https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

所有请求均需在 Header 中传入：

| Header 键         | 类型   | 必填 | 说明                          |
|-------------------|--------|------|------------------------------|
| X-API-Token       | string | ✅   | 向服务商申请的 API-Token (注意,这个Token请设置成变量，因为Token固定的数量用完之后就永久失效了)    |
| X-Response-Format | string | ✅   | 固定值 `wrapped` |

---

## 2. 接口总览

| 接口功能         | Method | 路径                            | 描述                           |
|------------------|--------|---------------------------------|--------------------------------|
| 提交计算任务     | POST   | `/api/sfd/calc`                 | 传入结构号，返回任务 token     |
| 查询剩余次数     | GET    | `/api/sfd/token-remaining-times`| 查询当前 API-Key 剩余调用次数  |

---

## 3. 详细接口

### 3.1 提交计算任务
**POST** `/api/sfd/calc`

#### 请求示例
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--data '{
    "structure": "6536xxxxx"
}'
```

| 字段        | 类型     | 必填 | 说明           |
| --------- | ------ | -- | ------------ |
| structure | string | ✅  | 112位 或 158位 请求码 |  

响应
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

| 字段        | 类型     | 说明                     |
| --------- | ------ | ---------------------- |
| token     | string | SFD1解锁码，用来解锁模块SFD1        |
| remaining | number | 该 API-Key 剩余可调用次数） |


### 3.2 查询剩余次数
**GET** `/api/sfd/token-remaining-times`

#### 请求示例
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

响应
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| 字段        | 类型     | 说明                     |
| --------- | ------ | ---------------------- |
| remaining | number | 该 API-Key 剩余可调用次数） |