 **انتخابات STP (STP Elections)** :

---

## 📌 ما معنى STP Elections؟

- في بروتوكول **Spanning Tree Protocol (STP)**، قبل ما يبدأ يحدد أي المنافذ تغلق وأيها تظل شغالة، لازم أولًا يختار **Switch رئيسي (Root Bridge)**.
    
- عملية الاختيار هذه اسمها **STP Election**.
    

---

## 📌 مراحل الانتخابات

### 1. انتخاب **Root Bridge**

- كل Switch في الشبكة يرسل رسالة اسمها **BPDU (Bridge Protocol Data Unit)**.
    
- الـ **BPDU** تحتوي على:
    
    - **Bridge ID** = (Priority + MAC Address).
        
- الـ Switch اللي عنده **أصغر Bridge ID** يصبح **Root Bridge**.
    

🔹 ملاحظة:

- القيمة الافتراضية للـ Priority = 32768.
    
- إذا كانت نفس الـ Priority → المقارنة تصير بالـ **MAC Address** (الأصغر يفوز).
    

---

### 2. اختيار **Root Port (RP)** لكل Switch غير رئيسي

- كل Switch يحدد أقصر منفذ يوصل للـ Root Bridge.
    
- هذا المنفذ يصير **Root Port**.
    

---

### 3. اختيار **Designated Port (DP)**

- لكل شبكة فرعية (Segment) لازم يكون فيه منفذ واحد مسؤول عن تمرير الترافيك.
    
- هذا المنفذ يُسمى **Designated Port** (يُختار بناءً على أقل Path Cost).
    

---

### 4. المنافذ الأخرى تصير **Blocked Ports**

- إذا المنفذ ما هو Root ولا Designated → يصير **Blocked** عشان يمنع الـ Loop.
    

---

## 📌 مثال عملي

لو عندك 3 Switches (S1, S2, S3):

1. كل واحد يرسل BPDU بالـ Bridge ID.
    
2. مثلاً:
    
    - S1 Priority 32768 + MAC أصغر → S1 يصير Root Bridge.
        
3. S2 و S3 يختاروا أفضل منفذ يوصل لـ S1 كـ **Root Port**.
    
4. الباقي: منفذ واحد يظل **Blocked** لتفادي الـ Loop.
    

---

## 📌 الخلاصة

- **Election = اختيار Root Bridge** (بأصغر Bridge ID).
    
- بعدين يتم تحديد Root Ports و Designated Ports.
    
- الهدف = تكوين شجرة (Spanning Tree) تمنع الحلقات (Loops).
    

---


[[46- BPDU]]