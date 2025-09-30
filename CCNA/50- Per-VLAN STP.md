تمام 👌 خلينا نشرح موضوع **Per-VLAN STP (PVST / PVST+)** لأنه مهم جدًا في بيئة الـ **LAN مع VLANs**:

---

## 📌 المشكلة مع STP العادي

- **STP (802.1D أو 802.1W)** يشتغل على مستوى **الشبكة كاملة**.
    
- يعني: كل الشبكة يكون لها **شجرة Spanning Tree واحدة فقط**.
    
- المشكلة: لو عندك **VLANs متعددة**، راح يستخدموا نفس الـ Tree → بعض الـ Links ممكن تبقى غير مستغلة (غير فعالة) في كل الـ VLANs.
    

---

## 📌 الحل → Per-VLAN Spanning Tree (PVST)

- قدمته Cisco كإضافة.
    
- الفكرة:
    
    - بدل ما يكون عندك شجرة واحدة للشبكة كلها، يكون عندك **شجرة منفصلة لكل VLAN**.
        
    - يعني: **كل VLAN لها Root Bridge خاص بها، وحساب STP خاص بها**.
        

---

## 📌 المزايا

1. **تحميل متوازن (Load Balancing):**
    
    - ممكن تخلي VLAN 10 تستخدم Switch A كـ Root،
        
    - و VLAN 20 تستخدم Switch B كـ Root.
        
    - النتيجة: استغلال أفضل للروابط (Links).
        
2. **مرونة أكثر:**
    
    - كل VLAN تقدر يكون لها إعدادات STP مستقلة (Priority، Root Bridge).
        
3. **أمان واستقرار:**
    
    - لو صار Loop في VLAN واحدة، ما يأثر على باقي الـ VLANs.
        

---

## 📌 الأنواع

1. **PVST (Per-VLAN STP):**
    
    - يعمل مع **STP (802.1D)**.
        
    - قديم نوعًا ما.
        
2. **PVST+ (Per-VLAN STP Plus):**
    
    - يعمل مع **RSTP (802.1W)**.
        
    - أسرع بكثير.
        
    - هو الأكثر استخدامًا في أجهزة Cisco الحديثة.
        

---

## 📌 مثال عملي

تخيل عندك 2 Switches متصلين برابطين (Redundant Links):

- **بدون PVST:**
    
    - STP يختار مسار واحد فقط (ويغلق الآخر) لكل VLAN → رابط واحد بس مستغل.
        
- **مع PVST:**
    
    - ممكن تخلي VLAN 10 تستخدم الرابط الأول،
        
    - و VLAN 20 تستخدم الرابط الثاني.
        
    - كذا تستغل الرابطين وتمنع الازدحام.
        

---

## 📌 أوامر أساسية (Cisco IOS)

```bash
# تفعيل PVST+
Switch(config)# spanning-tree mode pvst

# تغيير Priority لعمل Switch كـ Root في VLAN معينة
Switch(config)# spanning-tree vlan 10 priority 4096

# عرض حالة STP لكل VLAN
Switch# show spanning-tree
```

---

🔹 **الخلاصة:**  
**Per-VLAN STP = نسخة من STP لكل VLAN**

- تحل مشكلة استغلال رابط واحد فقط.
    
- تسمح بتوزيع الحمل بين الـ VLANs.
    
- الأفضل حاليًا: **PVST+ (مع RSTP)**.
    

---

[[51- STP Design]]