تمام، **LAG** أو **Link Aggregation Group** هو مفهوم مهم جدًا في الشبكات، خصوصًا عند الحاجة لزيادة سرعة الاتصال بين أجهزة الشبكة أو توفير **Redundancy**. سأشرحه لك بالتفصيل مع التطبيق العملي على Packet Tracer:

---

## **1️⃣ التعريف الأساسي**

- **LAG (Link Aggregation Group)**: هو مجموعة من الروابط الفيزيائية تُجمع معًا لتتصرف كوصلة واحدة من حيث **النطاق الترددي (Bandwidth)** و **الاعتمادية (Redundancy)**.
    
- الفكرة: بدلاً من وجود رابط واحد بين جهازين، نستخدم عدة روابط لزيادة السرعة وتقليل احتمالية الانقطاع.
    

---

## **2️⃣ الفوائد**

1. **زيادة السرعة**: الجمع بين عدة روابط يعني زيادة مجموع عرض النطاق (مثلاً 4 روابط × 1Gbps = 4Gbps).
    
2. **الاعتمادية والتكرار**: إذا انقطعت إحدى الروابط، تبقى الشبكة تعمل عبر الروابط الأخرى.
    
3. **تحسين استخدام الشبكة**: توزيع البيانات عبر عدة وصلات يقلل من الاختناقات.
    
4. **سهولة الإدارة**: تُعامل جميع الروابط المجمعة كوصلة واحدة.
    

---

## **3️⃣ أنواع LAG**

- **Static LAG**: الربط يتم يدويًا على كلا الجهازين، بدون بروتوكول إضافي.
    
- **Dynamic LAG (LACP - Link Aggregation Control Protocol)**: الربط يستخدم بروتوكول LACP لتنسيق روابط المجموعة تلقائيًا.
    
    - معيار IEEE: **802.3ad / 802.1AX**
        

---

## **4️⃣ مكونات LAG**

|العنصر|الوصف|
|---|---|
|Physical Links|الروابط الفيزيائية بين الأجهزة|
|Aggregation|عملية دمج الروابط في واجهة واحدة|
|Protocol (اختياري)|LACP لإدارة الروابط ديناميكيًا|

---

## **5️⃣ التطبيق العملي على Packet Tracer**

**المطلوب**: جهازين Switch متصلين بعدة وصلات.

### **خطوات إنشاء LAG (Static)**

1. افتح Packet Tracer وضع **Switch1 وSwitch2**.
    
2. اربطهما بـ 2 أو أكثر من **copper straight-through cables**.
    
3. على CLI لكل Switch:
    

```bash
Switch> enable
Switch# conf t
Switch(config)# interface range fa0/1 - 2
Switch(config-if-range)# channel-group 1 mode on
Switch(config-if-range)# exit
Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk   # إذا تريد نقل VLANs
```

4. كرر على **Switch2** بنفس الإعدادات.
    
5. تحقق من الحالة:
    

```bash
Switch# show etherchannel summary
```

سترى أن كل الروابط أصبحت جزءًا من **Port-Channel 1**.

### **خطوات إنشاء LAG (Dynamic - LACP)**

- نفس الخطوات السابقة، لكن بدل `mode on` استخدم:
    

```bash
channel-group 1 mode active
```

- `active` → LACP يعمل تلقائيًا.
    
- `passive` → ينتظر الطرف الآخر لتفعيل LACP.
    

---

إذا أحببت، أقدر أجهز لك **مختبر Packet Tracer جاهز** يحتوي على **LAG بين سويتشين + VLAN + Trunk** لتجربته مباشرة ومشاهدة تأثيره على الشبكة.

[[55- Bundling Protocols]]