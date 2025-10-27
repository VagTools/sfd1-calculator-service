[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# Documentation de l'API du service de calcul SFD1
&gt; Version : v1.0  
&gt; Mis à jour : 26/10/2025  
&gt; URL de base : `https://xxxxxx.com`

---

## 1. Authentification

Pour obtenir un `X-API-Token`, veuillez en faire la demande via notre formulaire de demande : [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

Toutes les demandes doivent inclure les éléments suivants dans l'en-tête :

| Clé d'en-tête | Type | Requis | Description |
|---|---|---|---|
| X-API-Token | string | ✅ | Le jeton d'API demandé au fournisseur de services (Remarque : veuillez définir ce jeton comme une variable, car il deviendra définitivement invalide après un nombre fixe d'utilisations). |
| X-Response-Format | string | ✅ | Valeur fixe `wrapped` |
| Cookie | string | ✅ | Utilisé pour spécifier la langue de la réponse. Par exemple, pour obtenir des réponses en chinois, définissez `language=zh_CN`. Les langues prises en charge sont listées en haut du document. |

---

## 2. Présentation de l'API

| Fonction | Méthode | Chemin | Description |
|---|---|---|---|
| Soumettre une tâche de calcul | POST | `/api/sfd/calc` | Soumettez un numéro de structure et renvoyez un jeton de tâche. |
| Interroger les temps restants | GET | `/api/sfd/token-remaining-times`| Interrogez les temps d'appel restants pour la clé d'API actuelle. |
| Exporter les journaux de calcul | GET | `/api/sfd/calc-logs` | Exporter tous les journaux de calcul pour la clé API actuelle sous forme de fichier. |

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
--cookie 'language=fr_FR' \
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
--header 'X-Response-Format: wrapped' \
--cookie 'language=fr_FR'
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


### 3.3 Exporter les journaux de calcul


**GET** `/api/sfd/calc-logs`




Ce point de terminaison exporte tous les journaux de calcul associés au `X-API-Token` fourni. Il déclenche un téléchargement de fichier, généralement au format de feuille de calcul (par exemple, Excel/CSV). 




#### Exemple de requête


```bash


curl --location '{{BaseURL}}/api/sfd/calc-logs' \


--header 'X-API-Token: {{YOUR_API_TOKEN}}' \


--header 'X-Response-Format: wrapped' \


--cookie 'language=fr_FR'


```




#### Réponse


La réponse sera un téléchargement de fichier, pas un objet JSON. Le navigateur ou le client gérera le processus de téléchargement. 




---



## 4. Codes d'erreur 



Le service renvoie les codes d'erreur métier suivants dans le champ `code` du corps de la réponse lorsqu'une requête échoue. 



| Code d'entreprise | Statut HTTP | Description (de messages_fr_FR.properties) |
|---|---|---|
| 40300 | 403 | Le jeton API a été désactivé. |
| 40301 | 403 | Le jeton API a expiré. |
| 40001 | 400 | La quantité restante du jeton API est insuffisante. |
| 40003 | 400 | Aucune information GRP n'est liée au jeton API. |
| 40004 | 400 | Impossible de convertir en structure SOAP. |
| 40002 | 400 | Exception de longueur de structure. La longueur doit être {0} ou {1}. |
| 40002 | 400 | Exception d'argument de requête non valide. |
| 40003 | 403 | Accès interdit. |
| 50001 | 500 | Exception système inconnue. |