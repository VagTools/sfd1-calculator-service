[ 简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Documentación de la API del servicio de cálculo SFD1
&gt; Versión: v1.0  
&gt; Actualizado: 26-10-2025  
&gt; URL base: `https://xxxxxx.com`  

---

## 1. Autenticación

Para obtener un `X-API-Token`, solicítelo a través de nuestro formulario de solicitud: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Todas las solicitudes deben incluir lo siguiente en el encabezado:

| Clave de encabezado | Tipo | Requerido | Descripción |
|---|---|---|---|
| X-API-Token | string | ✅ | El token de API solicitado al proveedor de servicios (Nota: establezca este token como una variable, ya que dejará de ser válido permanentemente después de un número fijo de usos). |
| X-Response-Format | string | ✅ | Valor fijo `wrapped` |
| Cookie | string | ✅ | Se utiliza para especificar el idioma de la respuesta. Por ejemplo, para obtener respuestas en chino, establezca `language=zh_CN`. Los idiomas admitidos se enumeran al principio del documento. |

---

## 2. Descripción general de la API

| Función | Método | Ruta | Descripción |
|---|---|---|---|
| Enviar tarea de cálculo | POST | `/api/sfd/calc` | Envíe un número de estructura y devuelva un token de tarea. |
| Consultar tiempos restantes | GET | `/api/sfd/token-remaining-times`| Consulte los tiempos de llamada restantes para la clave de API actual. |
| Exportar registros de cálculo | GET | `/api/sfd/calc-logs` | Exportar todos los registros de cálculo para la clave API actual como un archivo. |

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
--cookie 'language=es_ES' \
--data 
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
--header 'X-Response-Format: wrapped' \
--cookie 'language=es_ES'
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


### 3.3 Exportar registros de cálculo


**GET** `/api/sfd/calc-logs`




Este endpoint exporta todos los registros de cálculo asociados con el `X-API-Token` proporcionado. Activa una descarga de archivo, típicamente en formato de hoja de cálculo (por ejemplo, Excel/CSV). 



#### Ejemplo de solicitud


```bash


curl --location '{{BaseURL}}/api/sfd/calc-logs' \

--header 'X-API-Token: {{YOUR_API_TOKEN}}' \

--header 'X-Response-Format: wrapped' \

--cookie 'language=es_ES'

```



#### Respuesta

La respuesta será una descarga de archivo, no un objeto JSON. El navegador o cliente gestionará el proceso de descarga.



---



## 4. Códigos de error 



El servicio devuelve los siguientes códigos de error de negocio en el campo `code` del cuerpo de la respuesta cuando una solicitud falla.



| Código de negocio | Estado HTTP | Descripción (de messages_es_ES.properties) |
|---|---|---|
| 40300 | 403 | El token de API ha sido deshabilitado. |
| 40301 | 403 | El token de API ha expirado. |
| 40001 | 400 | La cantidad restante del token de API es insuficiente. |
| 40003 | 400 | No hay información GRP vinculada para el token de API. |
| 40004 | 400 | No se puede convertir a la estructura SOAP. |
| 40002 | 400 | Excepción de longitud de estructura. La longitud debe ser {0} o {1}. |
| 40002 | 400 | Excepción de argumento de solicitud no válido. |
| 40003 | 403 | Acceso prohibido. |
| 50001 | 500 | Excepción de sistema desconocida. |