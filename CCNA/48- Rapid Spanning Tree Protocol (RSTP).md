
---

## 📌 ما هو RSTP؟

- **RSTP = Rapid Spanning Tree Protocol**
    
- المعيار: **IEEE 802.1W**
    
- نسخة مطورة من **STP (802.1D)**.
    
- الهدف: نفس الهدف الأساسي → منع **Loops** في الشبكة، لكن بسرعة أكبر بكثير.
    

---

## 📌 الفرق بين STP و RSTP

|العنصر|STP (802.1D) ⏳|RSTP (802.1W) ⚡|
|---|---|---|
|**الـ Convergence** (زمن الاستقرار بعد تغيير)|30–50 ثانية|1–10 ثوانٍ|
|**حالات المنافذ**|Blocking → Listening → Learning → Forwarding|Discarding → Learning → Forwarding|
|**الـ BPDU**|يرسل فقط Root Bridge|كل Switch يرسل BPDU|
|**التعافي بعد فشل**|بطيء|سريع (Immediate Recovery)|

---

## 📌 حالات المنافذ (RSTP States)

RSTP يبسط الحالات إلى 3 فقط:

1. **Discarding** = Blocking + Listening (لا يمرر أي بيانات)
    
2. **Learning** = يتعلم الـ MAC addresses لكن لا يمرر بيانات
    
3. **Forwarding** = يمرر البيانات بشكل طبيعي
    

---

## 📌 أدوار المنافذ (Port Roles) في RSTP

1. **Root Port (RP)** → المنفذ الأقرب إلى Root Bridge.
    
2. **Designated Port (DP)** → المنفذ المسؤول عن تمرير الترافيك لشبكة معينة.
    
3. **Alternate Port** → منفذ احتياطي (Backup) يُستخدم لو فشل الـ Root Port.
    
4. **Backup Port** → نسخة احتياطية للـ Designated Port داخل نفس الشبكة الفرعية (Segment).
    

🔹 الجديد هنا: RSTP أضاف **Alternate + Backup Ports** لسرعة الاستبدال عند حدوث فشل.

---

## 📌 كيف يعمل RSTP بسرعة؟

- بدل ما ينتظر مؤقتات طويلة (Timers) زي STP، يستخدم **Handshake سريع (Proposal/Agreement)** بين الـ Switches:
    
    1. Switch يرسل **Proposal BPDU** → يقترح يكون هو Designated Port.
        
    2. Switch الآخر يرد بـ **Agreement** إذا وافق.
        
- النتيجة: المنافذ تدخل Forwarding بسرعة كبيرة.
    

---

## 📌 الفائدة العملية

- في الشبكات الكبيرة (Enterprise Networks)، استخدام **RSTP** يقلل زمن توقف الشبكة من نصف دقيقة تقريبًا إلى ثوانٍ قليلة فقط.
    
- هو الافتراضي تقريبًا في أجهزة **Cisco الحديثة**.
    

---

🔹 **الخلاصة:**  
**RSTP = STP لكن أسرع بكثير**

- يحمي من Loops
    
- يتعافى بسرعة عند حدوث أعطال
    
- أسهل وأبسط في الحالات والأدوار
    

---

[[49- BPDUs (Bridge Protocol Data Units)]]