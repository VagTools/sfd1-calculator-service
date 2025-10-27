[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Documentação da API do Serviço de Cálculo SFD1
&gt; Versão: v1.0  
&gt; Atualizado: 26/10/2025  
&gt; URL Base: `https://xxxxxx.com`  

---

## 1. Autenticação

Para obter um `X-API-Token`, por favor, inscreva-se através do nosso formulário de inscrição: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Todas as solicitações devem incluir o seguinte no cabeçalho:

| Chave do cabeçalho | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| X-API-Token | string | ✅ | O API-Token solicitado ao provedor de serviços (Observação: defina este token como uma variável, pois ele se tornará permanentemente inválido após um número fixo de usos). |
| X-Response-Format | string | ✅ | Valor fixo `wrapped` |
| Cookie | string | ✅ | Usado para especificar o idioma da resposta. Por exemplo, para obter respostas em chinês, defina `language=pt_PT`. Os idiomas suportados estão listados no topo do documento. |

---

## 2. Visão geral da API

| Função | Método | Caminho | Descrição |
|---|---|---|---|
| Enviar tarefa de cálculo | POST | `/api/sfd/calc` | Envie um número de estrutura e retorne um token de tarefa. |
| Consultar tempos restantes | GET | `/api/sfd/token-remaining-times`| Consulte os tempos de chamada restantes para a chave de API atual. |
| Exportar Registros de Cálculo | GET | `/api/sfd/calc-logs` | Exportar todos os registros de cálculo para a chave API atual como um arquivo. |

---

## 3. APIs detalhadas

### 3.1 Enviar tarefa de cálculo
**POST** `/api/sfd/calc`

#### Exemplo de solicitação
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--cookie 'language=pt_PT' \
--data '{
    "structure": "6536xxxxx"
}'
```

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| structure | string | ✅ | Código de solicitação de 112 ou 158 bits |

#### Resposta
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

| Campo | Tipo | Descrição |
|---|---|---|
| token | string | Código de desbloqueio SFD1, usado para desbloquear o módulo SFD1. |
| remaining | number | O número restante de vezes que esta chave de API pode ser chamada. |


### 3.2 Consultar tempos restantes
**GET** `/api/sfd/token-remaining-times`

#### Exemplo de solicitação
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=pt_PT'
```

#### Resposta
```json
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| Campo | Tipo | Descrição |
|---|---|---|
| remaining | number | O número restante de vezes que esta chave de API pode ser chamada. |


### 3.3 Exportar Registros de Cálculo

**GET** `/api/sfd/calc-logs`



Este endpoint exporta todos os registros de cálculo associados ao `X-API-Token` fornecido. Ele aciona um download de arquivo, tipicamente em formato de planilha (por exemplo, Excel/CSV). 



#### Exemplo de solicitação

```bash
curl --location '{{BaseURL}}/api/sfd/calc-logs' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=pt_PT'
```



#### Resposta

A resposta será um download de arquivo, não um objeto JSON. O navegador ou cliente lidará com o processo de download. 



---



## 4. Códigos de Erro 



O serviço retorna os seguintes códigos de erro de negócio no campo `code` do corpo da resposta quando uma solicitação falha. 



| Código de Negócio | Status HTTP | Descrição (de messages_pt_PT.properties) |
|---|---|---|
| 40300 | 403 | O token da API foi desativado. |
| 40301 | 403 | O token da API expirou. |
| 40001 | 400 | A quantidade restante do token da API é insuficiente. |
| 40003 | 400 | Nenhuma informação GRP vinculada ao token da API. |
| 40004 | 400 | Não é possível converter para a estrutura SOAP. |
| 40002 | 400 | Exceção de comprimento da estrutura. O comprimento deve ser {0} ou {1}. |
| 40002 | 400 | Exceção de argumento de solicitação ilegal. |
| 40003 | 403 | Acesso proibido. |
| 50001 | 500 | Exceção de sistema desconhecida. |
