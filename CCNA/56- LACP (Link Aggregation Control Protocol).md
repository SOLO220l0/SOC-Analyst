تمام، خلينا نشرح **LACP (Link Aggregation Control Protocol)** بالتفصيل، بطريقة عملية وعلمية:

---

## **1️⃣ تعريف LACP**

- **LACP** هو بروتوكول معيار **IEEE 802.3ad / 802.1AX**.
    
- وظيفته: دمج عدة وصلات في رابط واحد يسمى **Link Aggregation Group (LAG)**.
    
- الفوائد:
    
    1. **زيادة عرض النطاق (Bandwidth)**: مجموع سرعة الروابط المجمعة.
        
    2. **التكرار (Redundancy)**: إذا فشل رابط، البقية تستمر بالعمل.
        
    3. **إدارة أسهل**: التعامل مع LAG كوحدة واحدة.
        

---

## **2️⃣ مبدأ عمل LACP**

1. الطرفان المتصلان (مثلاً سويتشان) يتبادلان **LACPDU messages** لتحديد الروابط القابلة للدمج.
    
2. **الأوضاع (Modes)**:
    
    - **Active**: يبادر بإرسال رسائل LACP.
        
    - **Passive**: ينتظر الطرف الآخر ليبدأ التفاوض.
        
3. بعد التفاوض، يتكون **Port-Channel** واحد يحتوي على الروابط المتفق عليها.
    

---

## **3️⃣ حالات استخدام LACP**

|الوضع|وصف|
|---|---|
|Active-Active|كلا الطرفين يرسلان LACP → ديناميكي بالكامل.|
|Active-Passive|طرف واحد يبادر، الآخر يستجيب → ديناميكي متوازن.|
|Passive-Passive|كلاهما ينتظر → لا يتم الدمج حتى يبادر أحد الطرفين.|

---

## **4️⃣ مزايا LACP**

- معيار عالمي → يعمل على أجهزة متعددة المصنعين.
    
- يمنع **loops** أو الروابط غير المتوافقة من الدمج.
    
- يمكن ضبط **عدد الروابط** في LAG حسب الحاجة.
    

---

## **5️⃣ التطبيق العملي على Packet Tracer**

**Scenario**: سويتشين متصلين برابطين FastEthernet.  
**خطوات التهيئة**:

```bash
Switch> enable
Switch# configure terminal
Switch(config)# interface range fa0/1 - 2
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# exit
Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

- تحقق من الحالة:
    

```bash
Switch# show etherchannel summary
```

سترى أن الروابط أصبحت ضمن **Port-Channel 1** باستخدام **LACP**، ويظهر الوضع **Active** وحالة الروابط **up**.

---

إذا أحببت، أقدر أرسم لك **توضيح عملي على Packet Tracer** يبين كيف تعمل LACP على سويتشين مع 2 أو 3 وصلات وكيف تتصرف عند قطع رابط واحد، بحيث تشوف التكرار في الواقع.

[[57- Static & Dynamic]]