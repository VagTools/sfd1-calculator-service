[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Документація API служби обчислень SFD1
> Версія: v1.0
> Оновлено: 2025-10-26
> Базова URL-адреса: `https://xxxxxx.com`

---

## 1. Автентифікація

Щоб отримати `X-API-Token`, будь ласка, подайте заявку через нашу форму заявки: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Усі запити повинні містити в заголовку наступне:

| Ключ заголовка | Тип | Обов'язково | Опис |
|---|---|---|---|
| X-API-Token | string | ✅ | API-токен, отриманий від постачальника послуг (Примітка: будь ласка, встановіть цей токен як змінну, оскільки він стане недійсним після фіксованої кількості використань). |
| X-Response-Format | string | ✅ | Фіксоване значення `wrapped` |

---

## 2. Огляд API

| Функція | Метод | Шлях | Опис |
|---|---|---|---|
| Надіслати завдання на обчислення | POST | `/api/sfd/calc` | Надішліть номер структури та отримайте маркер завдання. |
| Запитати час, що залишився | GET | `/api/sfd/token-remaining-times`| Запитати час, що залишився для поточного ключа API. |

---

## 3. Детальні API

### 3.1 Надіслати завдання на обчислення
**POST** `/api/sfd/calc`

#### Приклад запиту
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

| Поле | Тип | Обов'язково | Опис |
|---|---|---|---|
| structure | string | ✅ | 112-бітний або 158-бітний код запиту |

#### Відповідь
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

| Поле | Тип | Опис |
|---|---|---|
| token | string | Код розблокування SFD1, який використовується для розблокування модуля SFD1. |
| remaining | number | Кількість разів, що залишилася, яку можна викликати цей ключ API. |


### 3.2 Запитати час, що залишився
**GET** `/api/sfd/token-remaining-times`

#### Приклад запиту
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### Відповідь
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| Поле | Тип | Опис |
|---|---|---|
| remaining | number | Кількість разів, що залишилася, яку можна викликати цей ключ API. |