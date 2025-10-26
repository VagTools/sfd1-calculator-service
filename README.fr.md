[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Documentation de l'API du service de calcul SFD1
> Version : v1.0
> Mis à jour : 26/10/2025
> URL de base : `https://xxxxxx.com`

---

## 1. Authentification

Pour obtenir un `X-API-Token`, veuillez en faire la demande via notre formulaire de demande : [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Toutes les demandes doivent inclure les éléments suivants dans l'en-tête :

| Clé d'en-tête | Type | Requis | Description |
|---|---|---|---|
| X-API-Token | string | ✅ | Le jeton d'API demandé au fournisseur de services (Remarque : veuillez définir ce jeton comme une variable, car il deviendra définitivement invalide après un nombre fixe d'utilisations). |
| X-Response-Format | string | ✅ | Valeur fixe `wrapped` |

---

## 2. Présentation de l'API

| Fonction | Méthode | Chemin | Description |
|---|---|---|---|
| Soumettre une tâche de calcul | POST | `/api/sfd/calc` | Soumettez un numéro de structure et renvoyez un jeton de tâche. |
| Interroger les temps restants | GET | `/api/sfd/token-remaining-times`| Interrogez les temps d'appel restants pour la clé d'API actuelle. |

---

## 3. API détaillées

### 3.1 Soumettre une tâche de calcul
**POST** `/api/sfd/calc`

#### Exemple de requête
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

| Champ | Type | Requis | Description |
|---|---|---|---|
| structure | string | ✅ | Code de requête de 112 ou 158 bits |

#### Réponse
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

| Champ | Type | Description |
|---|---|---|
| token | string | Code de déverrouillage SFD1, utilisé pour déverrouiller le module SFD1. |
| remaining | number | Le nombre de fois restant que cette clé d'API peut être appelée. |


### 3.2 Interroger les temps restants
**GET** `/api/sfd/token-remaining-times`

#### Exemple de requête
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped'
```

#### Réponse
```
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| Champ | Type | Description |
|---|---|---|
| remaining | number | Le nombre de fois restant que cette clé d'API peut être appelée. |