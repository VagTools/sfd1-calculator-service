[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Документация по API службы расчетов SFD1
> Версия: v1.0
> Обновлено: 26.10.2025
> Базовый URL: `https://xxxxxx.com`

---

## 1. Аутентификация

Чтобы получить `X-API-Token`, подайте заявку через нашу форму заявки: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Все запросы должны содержать в заголовке следующее:

| Ключ заголовка | Тип | Обязательно | Описание |
|---|---|---|---|
| X-API-Token | string | ✅ | API-токен, полученный от поставщика услуг (Примечание: установите этот токен как переменную, так как он станет недействительным после фиксированного количества использований). |
| X-Response-Format | string | ✅ | Фиксированное значение `wrapped` |

---

## 2. Обзор API

| Функция | Метод | Путь | Описание |
|---|---|---|---|
| Отправить задачу на расчет | POST | `/api/sfd/calc` | Отправьте номер структуры и получите токен задачи. |
| Запросить оставшееся количество раз | GET | `/api/sfd/token-remaining-times`| Запросить оставшееся количество вызовов для текущего API-ключа. |

---

## 3. Подробные API

### 3.1 Отправить задачу на расчет
**POST** `/api/sfd/calc`

#### Пример запроса
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--data '{
    "structure": "6536xxxxx"
}'
```

| Поле | Тип | Обязательно | Описание |
|---|---|---|---|
| structure | string | ✅ | 112-битный или 158-битный код запроса |

#### Ответ
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

| Поле | Тип | Описание |
|---|---|---|
| token | string | Код разблокировки SFD1, используемый для разблокировки модуля SFD1. |
| remaining | number | Оставшееся количество раз, которое можно вызвать этот API-ключ. |


### 3.2 Запросить оставшееся количество раз
**GET** `/api/sfd/token-remaining-times`

#### Пример запроса
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### Ответ
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| Поле | Тип | Описание |
|---|---|---|
| remaining | number | Оставшееся количество раз, которое можно вызвать этот API-ключ. |