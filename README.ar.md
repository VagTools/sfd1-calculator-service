[简体中文](./README.zh.md) | [English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Deutsch](./README.de.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md) | [Українська](./README.uk.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Português](./README.pt.md)

# وثائق واجهة برمجة تطبيقات خدمة حساب SFD1
&gt; الإصدار: v1.0    
&gt; محدث: 2025-10-26  
&gt;  عنوان URL الأساسي: `https://xxxxxx.com`  

---

## 1. المصادقة

للحصول على `X-API-Token` ، يرجى التقديم من خلال نموذج الطلب الخاص بنا: [https://tally.so/r/wAo8lz](https://tally.so/r/wAo8lz)

يجب أن تتضمن جميع الطلبات ما يلي في الرأس:

| مفتاح الرأس | النوع | مطلوب | الوصف |
|---|---|---|---|
| X-API-Token | string | ✅ | رمز API-Token المطلوب من مزود الخدمة (ملاحظة: يرجى تعيين هذا الرمز كمتغير ، حيث سيصبح غير صالح بشكل دائم بعد عدد ثابت من الاستخدامات). |
| X-Response-Format | string | ✅ | قيمة ثابتة `wrapped` |
| Cookie | string | ✅ | يستخدم لتحديد لغة الاستجابة. على سبيل المثال، للحصول على استجابات باللغة الصينية، قم بتعيين `language=ar_SA`. اللغات المدعومة مدرجة في أعلى الوثيقة. |

---

## 2. نظرة عامة على واجهة برمجة التطبيقات

| الوظيفة | الطريقة | المسار | الوصف |
|---|---|---|---|
| إرسال مهمة حساب | POST | `/api/sfd/calc` | أرسل رقم هيكل وأرجع رمز مهمة. |
| الاستعلام عن الأوقات المتبقية | GET | `/api/sfd/token-remaining-times`| الاستعلام عن أوقات الاتصال المتبقية لمفتاح API الحالي. |
| تصدير سجلات الحساب | GET | `/api/sfd/calc-logs` | تصدير جميع سجلات الحساب للمفتاح API الحالي كملف. |

---

## 3. واجهات برمجة التطبيقات التفصيلية

### 3.1 إرسال مهمة حساب
**POST** `/api/sfd/calc`

#### مثال على الطلب
```bash
curl --location '{{BaseURL}}/api/sfd/calc' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--header 'Content-Type: application/json' \
--cookie 'language=ar_SA' \
--data '{
    "structure": "6536xxxxx"
}'
```

| الحقل | النوع | مطلوب | الوصف |
|---|---|---|---|
| structure | string | ✅ | رمز طلب 112 بت أو 158 بت |

#### الاستجابة
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

| الحقل | النوع | الوصف |
|---|---|---|
| token | string | رمز فتح SFD1 ، يستخدم لفتح الوحدة النمطية SFD1. |
| remaining | number | العدد المتبقي من المرات التي يمكن فيها استدعاء مفتاح API هذا. |


### 3.2 الاستعلام عن الأوقات المتبقية
**GET** `/api/sfd/token-remaining-times`

#### مثال على الطلب
```bash
curl --location '{{BaseURL}}/api/sfd/token-remaining-times' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=ar_SA'
```

#### الاستجابة
```json
{
  "code": 200,
  "message": "Calculation successful",
  "data": {
    "remaining": 80
  }
}
```

| الحقل | النوع | الوصف |
|---|---|---|
| remaining | number | العدد المتبقي من المرات التي يمكن فيها استدعاء مفتاح API هذا. |


### 3.3 تصدير سجلات الحساب


**GET** `/api/sfd/calc-logs`




تقوم نقطة النهاية هذه بتصدير جميع سجلات الحساب المرتبطة بـ `X-API-Token` المقدم. تؤدي إلى تنزيل ملف، عادةً بتنسيق جدول بيانات (مثل Excel/CSV). 



#### مثال على الطلب


```bash
curl --location '{{BaseURL}}/api/sfd/calc-logs' \
--header 'X-API-Token: {{YOUR_API_TOKEN}}' \
--header 'X-Response-Format: wrapped' \
--cookie 'language=ar_SA'
```



#### الاستجابة


ستكون الاستجابة تنزيل ملف، وليس كائن JSON. سيتعامل المتصفح أو العميل مع عملية التنزيل. 



---




## 4. رموز الخطأ 



تُرجع الخدمة رموز أخطاء العمل التالية في حقل `code` من نص الاستجابة عند فشل الطلب. 



| رمز العمل | حالة HTTP | الوصف (من messages_ar_SA.properties) |
|---|---|---|
| 40300 | 403 | تم تعطيل رمز API. |
| 40301 | 403 | انتهت صلاحية رمز API. |
| 40001 | 400 | الكمية المتبقية من رمز API غير كافية. |
| 40003 | 400 | لا توجد معلومات GRP مرتبطة برمز API. |
| 40004 | 400 | لا يمكن التحويل إلى بنية SOAP. |
| 40002 | 400 | استثناء طول البنية. يجب أن يكون الطول {0} أو {1}. |
| 40002 | 400 | استثناء وسيطة طلب غير صالحة. |
| 40003 | 403 | الوصول محظور. |
| 50001 | 500 | استثناء نظام غير معروف. |
