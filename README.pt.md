[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Documentação da API do Serviço de Cálculo SFD1
> Versão: v1.0
> Atualizado: 26/10/2025
> URL Base: `https://xxxxxx.com`

---

## 1. Autenticação

Para obter um `X-API-Token`, por favor, inscreva-se através do nosso formulário de inscrição: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Todas as solicitações devem incluir o seguinte no cabeçalho:

| Chave do cabeçalho | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| X-API-Token | string | ✅ | O API-Token solicitado ao provedor de serviços (Observação: defina este token como uma variável, pois ele se tornará permanentemente inválido após um número fixo de usos). |
| X-Response-Format | string | ✅ | Valor fixo `wrapped` |

---

## 2. Visão geral da API

| Função | Método | Caminho | Descrição |
|---|---|---|---|
| Enviar tarefa de cálculo | POST | `/api/sfd/calc` | Envie um número de estrutura e retorne um token de tarefa. |
| Consultar tempos restantes | GET | `/api/sfd/token-remaining-times`| Consulte os tempos de chamada restantes para a chave de API atual. |

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
--header 'X-Response-Format: wrapped'
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