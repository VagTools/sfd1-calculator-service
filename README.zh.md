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
| Cookie | string | ✅ | 用于指定响应的语言。例如，要获取中文响应，请设置 `language=zh_CN`。文档顶部列出了支持的语言。 |

---

## 2. 接口总览

| 接口功能         | Method | 路径                            | 描述                           |
|------------------|--------|---------------------------------|--------------------------------|
| 提交计算任务     | POST   | `/api/sfd/calc`                 | 传入结构号，返回任务 token     |
| 查询剩余次数     | GET    | `/api/sfd/token-remaining-times`| 查询当前 API-Key 剩余调用次数  |
| 导出计算日志 | GET | `/api/sfd/calc-logs` | 以文件形式导出当前 API-Key 的所有计算日志。 |

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
--cookie 'language=zh_CN' \
--data 
{
    "structure": "6536xxxxx"
}
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
--header 'X-Response-Format: wrapped' \
--cookie 'language=zh_CN'
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

### 3.3 导出计算日志

**GET** `/api/sfd/calc-logs`



此接口导出与所提供的 `X-API-Token` 关联的所有计算日志。它会触发文件下载，通常是电子表格格式（例如 Excel/CSV）。



#### 请求示例

```bash

curl --location '{{BaseURL}}/api/sfd/calc-logs' \

--header 'X-API-Token: {{YOUR_API_TOKEN}}' \

--header 'X-Response-Format: wrapped' \

--cookie 'language=zh_CN'

```



#### 响应

响应将是一个文件下载，而不是一个 JSON 对象。浏览器或客户端将处理下载过程。



---



## 4. 错误码描述



当请求失败时，服务会在响应体的 `code` 字段中返回以下业务错误码。


| 业务代码 | HTTP 状态 | 描述 (来自 messages_zh_CN.properties) |
| --------- | ------ | ---------------------- |
| 40300 | 403 | API令牌已禁用。 |
| 40301 | 403 | API令牌已过期。 |
| 40001 | 400 | API令牌剩余数量不足。 |
| 40003 | 400 | API令牌未绑定GRP信息。 |
| 40004 | 400 | 无法转换为SOAP结构。 |
| 40002 | 400 | 结构长度异常。长度必须为 {0} 或 {1}。 |
| 40002 | 400 | 不合法的请求参数异常。 |
| 40003 | 403 | 禁止访问。 |
| 50001 | 500 | 未知系统异常。 |