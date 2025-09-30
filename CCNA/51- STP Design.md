
---


- **STP = Spanning Tree Protocol**
    
- الهدف: منع **Loop** (الحلقات) داخل الشبكات ذات التكرار Redundancy.
    
- بدون STP، إذا كان هناك أكثر من مسار بين السويتشات، الحزم (Frames) ستدور بلا توقف → انهيار الشبكة (Broadcast Storm).
    

لهذا السبب، "تصميم STP" يعني: **كيف تبني شبكة ذات طبقات مع مسارات احتياطية، لكن بدون حلقات نشطة**.

---

## 🏗️ مكونات التصميم الأساسية

1. **Root Bridge Selection**
    
    - كل الشبكة تختار "جسر جذري" (Root Bridge).
        
    - الافضل: تعيين **Core Switch** كـ Root.
        
    - يتم الاختيار بناءً على:
        
        - أولوية Bridge Priority (الأقل أفضل).
            
        - عنوان MAC (الأصغر يفوز عند التساوي).
            

---

2. **Port Roles (أدوار المنافذ)**
    
    - **Root Port (RP):** المنفذ الأقرب إلى الـ Root Bridge.
        
    - **Designated Port (DP):** المنفذ المسموح له بإرسال الترافيك في الشبكة.
        
    - **Blocked Port:** المنافذ المعطلة مؤقتًا لمنع الحلقات.
        

---

3. **Topology Design**
    
    - طبقات واضحة (Core – Distribution – Access).
        
    - **Core Layer:** عادة هو Root Bridge.
        
    - **Distribution Layer:** يكون Backup Root أو Secondary Root.
        
    - **Access Layer:** يحتوي على المنافذ التي تصل إلى المستخدمين، غالبًا بدون منافذ Blocked.
        

---

4. **Load Balancing باستخدام STP**
    
    - يمكن توزيع الحمل بتغيير الـ **Priority** بين VLANs:
        
        - VLAN 10 → Root Bridge = Switch A
            
        - VLAN 20 → Root Bridge = Switch B
            
    - هذا يسمح باستغلال الروابط الاحتياطية بدل تركها خاملة تمامًا.
        

---

5. **نسخ STP**
    
    - **802.1D (STP):** النسخة الأصلية (بطيئة – 30-50 ثانية convergence).
        
    - **802.1w (RSTP):** أسرع (أقل من 6 ثواني).
        
    - **PVST+/MSTP:** دعم تعدد الـ VLANs كلٌ له شجرة مستقلة.
        

---

## ⚙️ مثال عملي (تصميم مصغر)

- 2 Core Switches (Core1, Core2).
    
- 2 Distribution Switches (Dist1, Dist2).
    
- Access Switches متصلة بهم.
    

### خطوات التصميم:

1. اضبط Core1 كـ Root Bridge:
    

```bash
Core1(config)# spanning-tree vlan 1 priority 4096
```

2. اضبط Core2 كـ Secondary Root:
    

```bash
Core2(config)# spanning-tree vlan 1 priority 8192
```

3. راقب الأدوار:
    

```bash
Switch# show spanning-tree
```

→ سترى Root Ports, Designated Ports, و Blocked Ports.

---

## 🧭 الخلاصة

**STP Design** يعني:

- اختيار الـ Root Bridge بشكل ذكي (عادةً في الـ Core).
    
- ضبط أولويات لتوزيع الحمل عبر VLANs.
    
- ضمان وجود روابط احتياطية لكن بدون Loops.
    
- استخدام النسخة المناسبة (RSTP/MSTP) لتسريع التعافي.
    

---

[[52- Neighbor Discovery Protocol (NDP)]]