[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# SFD1 Berechnungsdienst API-Dokumentation
&gt; Version: v1.0  
&gt; Aktualisiert: 2025-10-26  
&gt; Basis-URL: `https://xxxxxx.com`

---

## 1. Authentifizierung

Um einen `X-API-Token` zu erhalten, beantragen Sie ihn bitte über unser Antragsformular: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Alle Anfragen müssen Folgendes im Header enthalten:

| Header-Schlüssel | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| X-API-Token | string | ✅ | Der beim Dienstanbieter beantragte API-Token (Hinweis: Bitte setzen Sie diesen Token als Variable, da er nach einer festen Anzahl von Verwendungen dauerhaft ungültig wird). |
| X-Response-Format | string | ✅ | Fester Wert `wrapped` |
| Cookie | string | ✅ | Wird verwendet, um die Antwortsprache anzugeben. Um beispielsweise chinesische Antworten zu erhalten, setzen Sie `language=de_DE`. Unterstützte Sprachen sind oben im Dokument aufgeführt. |

---

## 2. API-Übersicht

| Funktion | Methode | Pfad | Beschreibung |
|---|---|---|---|
| Berechnungsaufgabe übermitteln | POST | `/api/sfd/calc` | Übermitteln Sie eine Strukturnummer und erhalten Sie ein Aufgaben-Token. |
| Verbleibende Zeiten abfragen | GET | `/api/sfd/token-remaining-times`| Fragen Sie die verbleibenden Aufrufzeiten für den aktuellen API-Schlüssel ab. |
| Berechnungslogs exportieren | GET | `/api/sfd/calc-logs` | Alle Berechnungslogs für den aktuellen API-Schlüssel als Datei exportieren. |

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
--cookie 'language=de_DE' \
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
--header 'X-Response-Format: wrapped' \
--cookie 'language=de_DE'
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


### 3.3 Berechnungslogs exportieren


**GET** `/api/sfd/calc-logs`




Dieser Endpunkt exportiert alle Berechnungslogs, die mit dem bereitgestellten `X-API-Token` verknüpft sind. Er löst einen Dateidownload aus, typischerweise im Tabellenkalkulationsformat (z. B. Excel/CSV). 


#### Anforderungsbeispiel

```bash
curl --location '{{BaseURL}}/api/sfd/calc-logs' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=de_DE'
```


#### Antwort

Die Antwort ist ein Dateidownload, kein JSON-Objekt. Der Browser oder Client übernimmt den Downloadvorgang. 


---



## 4. Fehlercodes


Der Dienst gibt die folgenden Geschäftsfehlercodes im `code`-Feld des Antworttextes zurück, wenn eine Anfrage fehlschlägt. 


| Business Code | HTTP Status | Beschreibung (aus messages_de_DE.properties) |
|---|---|---|
| 40300 | 403 | API-Token wurde deaktiviert. |
| 40301 | 403 | API-Token ist abgelaufen. |
| 40001 | 400 | Die verbleibende Menge des API-Tokens ist unzureichend. |
| 40003 | 400 | Keine GRP-Informationen für den API-Token gebunden. |
| 40004 | 400 | Kann nicht in SOAP-Struktur konvertiert werden. |
| 40002 | 400 | Ausnahme bei der Strukturlänge. Die Länge muss {0} oder {1} sein. |
| 40002 | 400 | Ausnahme für ungültiges Anforderungsargument. |
| 40003 | 403 | Zugriff verboten. |
| 50001 | 500 | Unbekannte Systemausnahme. |
