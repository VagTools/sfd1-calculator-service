[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1 Berechnungsdienst API-Dokumentation
> Version: v1.0
> Aktualisiert: 2025-10-26
> Basis-URL: `https://xxxxxx.com`

---

## 1. Authentifizierung

Um einen `X-API-Token` zu erhalten, beantragen Sie ihn bitte über unser Antragsformular: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Alle Anfragen müssen Folgendes im Header enthalten:

| Header-Schlüssel | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| X-API-Token | string | ✅ | Der beim Dienstanbieter beantragte API-Token (Hinweis: Bitte setzen Sie diesen Token als Variable, da er nach einer festen Anzahl von Verwendungen dauerhaft ungültig wird). |
| X-Response-Format | string | ✅ | Fester Wert `wrapped` |

---

## 2. API-Übersicht

| Funktion | Methode | Pfad | Beschreibung |
|---|---|---|---|
| Berechnungsaufgabe übermitteln | POST | `/api/sfd/calc` | Übermitteln Sie eine Strukturnummer und erhalten Sie ein Aufgaben-Token. |
| Verbleibende Zeiten abfragen | GET | `/api/sfd/token-remaining-times`| Fragen Sie die verbleibenden Aufrufzeiten für den aktuellen API-Schlüssel ab. |

---

## 3. Detaillierte APIs

### 3.1 Berechnungsaufgabe übermitteln
**POST** `/api/sfd/calc`

#### Anforderungsbeispiel
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--data '{
    "structure": "6536xxxxx"
}'
```

| Feld | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| structure | string | ✅ | 112-Bit- oder 158-Bit-Anforderungscode |

#### Antwort
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

| Feld | Typ | Beschreibung |
|---|---|---|
| token | string | SFD1-Entsperrcode, der zum Entsperren des Moduls SFD1 verwendet wird. |
| remaining | number | Die verbleibende Anzahl von Aufrufen, die dieser API-Schlüssel tätigen kann. |


### 3.2 Verbleibende Zeiten abfragen
**GET** `/api/sfd/token-remaining-times`

#### Anforderungsbeispiel
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### Antwort
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| Feld | Typ | Beschreibung |
|---|---|---|
| remaining | number | Die verbleibende Anzahl von Aufrufen, die dieser API-Schlüssel tätigen kann. |