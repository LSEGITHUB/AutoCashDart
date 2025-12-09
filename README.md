# AutoCash Dart SDK

مكتبة `AutoCash` هي SDK مكتوبة بلغة **Dart** تتيح لك استلام المدفوعات والتحقق منها تلقائيًا عبر  
**VF-Cash – OKX – Binance** باستخدام نظام AutoCash ولوحة التحكم الخاصة بك.

تم تصميم المكتبة لتكون:
- Type-safe
- متوافقة مع Flutter (Android / iOS / Web)
- جاهزة للاستخدام في Production

---

## المتطلبات

- Dart 3.10 أو أحدث
- Flutter (مدعومة)
- مكتبة http

في ملف pubspec.yaml:

```yaml
dependencies:
  autocash:
```
---

## التثبيت

قم بتنفيذ الامر التالى:

```bash
dart pub add autocash
```
---

## التهيئة
```dart
import 'package:autocash/autocash.dart';

final autoCash = AutoCash(
  userId: 'YOUR_USER_ID',
  panelId: 'YOUR_PANEL_ID',
);
```
---

## الحصول على معلومات لوحة التحكم

باستخدام async/await:
```dart
final info = await autoCash.getPanelInfo();

print(info.number);
print(info.rate);
print(info.currency);
print(info.method);
```
أو باستخدام then:
```dart
autoCash.getPanelInfo().then((info) {
  print(info);
});
```
---

## إنشاء روابط الدفع

### VF-Cash
```dart
final paymentLink = autoCash.vfcashLink(
  extra: 'username',
);
```
---

### OKX
```dart
final okxLink = autoCash.okxLink(
  100,
  extra: 'username',
);
```
---

### Binance
```dart
final binanceLink = autoCash.binanceLink(
  100,
  extra: 'username',
);
```
---

## استخدام extra

القيمة extra يتم إرسالها إلى السيرفر كما هي  
وتعود مرة أخرى مع نتيجة التحقق أو callback.

تُستخدم لربط عملية الدفع بـ:
- اسم مستخدم
- رقم طلب
- معرف داخلي

---

## التحقق من عملية VF-Cash
```dart
final result = await autoCash.checkVFCash(
  phone: '01012345678',
  amount: 100,
  extra: 'username',
);
```
أو باستخدام then:
```dart
autoCash.checkVFCash(
  phone: '01012345678',
  amount: 100,
  extra: 'username',
).then((result) {
  print(result);
});
```
---

## التحقق من عملية OKX
```dart
autoCash.checkOKX(
  txid: 'TX_ID',
  amount: 100,
  extra: 'username',
).then((result) {
  print(result);
});
```
---

## التحقق من عملية Binance
```dart
autoCash.checkBinance(
  txid: 'TX_ID',
  amount: 100,
  extra: 'username',
).then((result) {
  print(result);
});
```
---

## نتيجة التحقق

كائن PaymentResult يحتوي على:
```dart
result.status  
result.message  
result.key  
```
مثال:
```json
{
  "status": "success",
  "message": "تم إكمال عملية الدفع بنجاح",
  "key": "TRANSACTION_KEY"
}
```
---

## إعادة التوجيه (Redirect)

لإخفاء رابط الدفع الحقيقي:
```dart
final redirectLink = autoCash.redirect(
  paymentLink,
);
```
---

## معالجة الأخطاء
```dart
try {
  await autoCash.getPanelInfo();
} catch (e) {
  print(e);
}
```
---

## ملاحظات مهمة

- تأكد من صحة userId و panelId
- لا تعتمد على التوقيت المحلي
- المفتاح key هو المرجع الأساسي لأي عملية
- يفضل استخدام قيم دقيقة للمبالغ

---

## الترخيص

MIT