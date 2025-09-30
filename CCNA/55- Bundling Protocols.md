تمام، دعنا نشرح **Bundling Protocols** بالتفصيل في سياق الشبكات، مع أمثلة عملية:

---

## **1️⃣ تعريف Bundling Protocols**

- **Bundling Protocols** هي البروتوكولات التي تُستخدم لدمج أكثر من رابط في **Link Aggregation Group (LAG)** واحدة.
    
- الهدف: زيادة عرض النطاق (Bandwidth)، وتحسين **التكرار (Redundancy)**، وتسهيل إدارة الروابط.
    
- تعمل على مستوى **Layer 2** أو **Layer 3** حسب البروتوكول.
    

---

## **2️⃣ أشهر البروتوكولات المستخدمة**

|البروتوكول|النوع|الوصف|
|---|---|---|
|**LACP (Link Aggregation Control Protocol)**|Dynamic|معيار IEEE 802.3ad / 802.1AX، يقوم بدمج الروابط ديناميكيًا ويتأكد من أن الطرفين يدعمان LACP.|
|**PAgP (Port Aggregation Protocol)**|Dynamic (Cisco)|بروتوكول سيسكو الخاص لعمل LAG ديناميكي بين سويتشات سيسكو.|
|**Static/Manual Bundling**|Static|ربط الروابط بدون بروتوكول، يعتمد على التهيئة اليدوية على كلا الطرفين.|

---

## **3️⃣ مقارنة بين البروتوكولات**

|الخاصية|LACP|PAgP|Static|
|---|---|---|---|
|معيار|IEEE 802.3ad|Cisco proprietary|لا يوجد|
|ديناميكية|نعم|نعم|لا|
|التوافق|أجهزة متعددة|أجهزة Cisco فقط|أي جهاز يدعم aggregation|
|الفحص التلقائي|نعم|نعم|لا|

---

## **4️⃣ طريقة عمل البروتوكولات**

1. **LACP**: الطرفان يتبادلان رسائل LACP ليعرف كل طرف أي الروابط صالحة للدمج.
    
    - الوضع **active** → يبادر بإرسال رسائل LACP.
        
    - الوضع **passive** → ينتظر الطرف الآخر.
        
2. **PAgP**: يعمل بشكل مشابه لـ LACP لكنه خاص بسيسكو فقط.
    
3. **Static Bundling**: كل الروابط تضاف يدويًا إلى **Port-Channel**، بدون رسائل بروتوكولية.
    

---

## **5️⃣ التطبيق العملي في Packet Tracer**

- **Scenario**: سويتشين متصلين بعدة روابط.
    
- **Dynamic LAG (LACP) Example**:
    

```bash
Switch> enable
Switch# conf t
Switch(config)# interface range fa0/1 - 2
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# exit
Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk
```

- تحقق من حالة البروتوكول:
    

```bash
Switch# show etherchannel summary
```

سترى أن الروابط تعمل ضمن **Port-Channel 1** باستخدام **LACP**.

- **Static Bundling Example**:
    

```bash
Switch(config-if-range)# channel-group 1 mode on
```

هنا لا يحتاج الطرف الآخر لبروتوكول، يتم الدمج مباشرة.

---

إذا أحببت، أقدر أجهز لك **جدول مقارنة عملي في Packet Tracer** بين **LACP وPAgP وStatic Bundling** مع تجربة فعليّة على **2 سويتش و3 روابط** لتوضيح تأثير كل بروتوكول على عرض النطاق وحالة الروابط.

[[56- LACP (Link Aggregation Control Protocol)]]