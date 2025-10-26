[ 简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Documentación de la API del servicio de cálculo SFD1
> Versión: v1.0
> Actualizado: 26-10-2025
> URL base: `https://xxxxxx.com`

---

## 1. Autenticación

Para obtener un `X-API-Token`, solicítelo a través de nuestro formulario de solicitud: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Todas las solicitudes deben incluir lo siguiente en el encabezado:

| Clave de encabezado | Tipo | Requerido | Descripción |
|---|---|---|---|
| X-API-Token | string | ✅ | El token de API solicitado al proveedor de servicios (Nota: establezca este token como una variable, ya que dejará de ser válido permanentemente después de un número fijo de usos). |
| X-Response-Format | string | ✅ | Valor fijo `wrapped` |

---

## 2. Descripción general de la API

| Función | Método | Ruta | Descripción |
|---|---|---|---|
| Enviar tarea de cálculo | POST | `/api/sfd/calc` | Envíe un número de estructura y devuelva un token de tarea. |
| Consultar tiempos restantes | GET | `/api/sfd/token-remaining-times`| Consulte los tiempos de llamada restantes para la clave de API actual. |

---

## 3. API detalladas

### 3.1 Enviar tarea de cálculo
**POST** `/api/sfd/calc`

#### Ejemplo de solicitud
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--data '{
    "structure": "6536xxxxx"
}'
```

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| structure | string | ✅ | Código de solicitud de 112 o 158 bits |

#### Respuesta
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

| Campo | Tipo | Descripción |
|---|---|---|
| token | string | Código de desbloqueo SFD1, utilizado para desbloquear el módulo SFD1. |
| remaining | number | El número restante de veces que se puede llamar a esta clave de API. |


### 3.2 Consultar tiempos restantes
**GET** `/api/sfd/token-remaining-times`

#### Ejemplo de solicitud
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### Respuesta
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| Campo | Tipo | Descripción |
|---|---|---|
| remaining | number | El número restante de veces que se puede llamar a esta clave de API. |