تمام، خلينا نشرح الفرق بين **Static** و **Dynamic** في سياق **Link Aggregation (LAG / EtherChannel)**، بشكل علمي وعمل على Packet Tracer:

---

## **1️⃣ Static LAG (Static EtherChannel)**

- **التعريف**: تجميع الروابط يتم **يدويًا** بدون بروتوكول تفاوض.
    
- كل رابط ضمن LAG يجب تهيئته يدويًا على كلا الطرفين.
    
- **مزايا**:
    
    - بسيط وسريع.
        
    - أقل تعقيدًا، لا يعتمد على بروتوكولات إضافية.
        
- **عيوب**:
    
    - لا يكتشف فشل الروابط ديناميكيًا إلا على مستوى Layer 1.
        
    - إذا تم تكوين رابط بشكل خاطئ على أحد الطرفين → loop أو خطأ في الاتصال.
        

**Packet Tracer مثال عملي**:

```bash
Switch> enable
Switch# configure terminal
Switch(config)# interface range fa0/1 - 2
Switch(config-if-range)# channel-group 1 mode on
Switch(config-if-range)# exit
Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

- الوضع هنا **mode on** يعني Static، لا يوجد LACP أو PAgP.
    

---

## **2️⃣ Dynamic LAG**

- **التعريف**: تجميع الروابط **تفاوضيًا** باستخدام بروتوكولات مثل:
    
    - **LACP (Link Aggregation Control Protocol)** → معيار IEEE 802.3ad
        
    - **PAgP (Port Aggregation Protocol)** → Cisco proprietary
        
- يقوم الطرفان **بتبادل رسائل** لتحديد الروابط الصالحة للدمج.
    
- **مزايا**:
    
    - يكتشف تلقائيًا الروابط المتوافقة.
        
    - يمنع الأخطاء loops أو دمج روابط غير متطابقة.
        
    - يدعم التكرار وإعادة التفاوض عند فشل رابط.
        
- **عيوب**:
    
    - يحتاج بروتوكول مدعوم على كلا الطرفين.
        
    - أكثر تعقيدًا من Static.
        

**Packet Tracer مثال عملي على LACP**:

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

- الوضع **mode active** → Dynamic باستخدام LACP.
    
- **Passive** → الطرف ينتظر التفاوض فقط.
    

---

### **📌 الفرق الأساسي بين Static و Dynamic**

|العنصر|Static|Dynamic (LACP/PAgP)|
|---|---|---|
|التفاوض|لا يوجد|يتم التفاوض تلقائيًا|
|التعقيد|منخفض|متوسط|
|الاعتمادية|منخفضة عند خطأ الإعداد|عالية|
|المرونة|منخفضة|عالية|
|مثال Packet Tracer|mode on|mode active / passive|

---

لو تحب، أقدر أرسم لك **سيناريو Packet Tracer عملي** يوضح Static vs Dynamic LAG، وتشوف الفرق بينهما عند قطع أحد الروابط أو إعادة توصيلها.

[[58- AP (Access Point Modes)]]