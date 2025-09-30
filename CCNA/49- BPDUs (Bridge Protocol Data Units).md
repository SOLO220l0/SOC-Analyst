تمام 👌، خلينا ندخل بعمق أكثر في موضوع **BPDUs (Bridge Protocol Data Units)**، لأنها العمود الفقري لبروتوكول **STP / RSTP**.

---

## 📌 ما هي BPDUs؟

- رسائل Layer 2 (تنتقل داخل الشبكة كـ **Frames**).
    
- يتبادلها الـ **Switches** فيما بينهم لبناء شجرة (Spanning Tree) تمنع حدوث **Loops**.
    
- تحتوي على معلومات عن:
    
    - من هو **Root Bridge**
        
    - تكلفة الطريق (Path Cost)
        
    - أولوية (Priority) + عنوان MAC
        
    - معرف المنفذ (Port ID)
        

---

## 📌 أنواع الـ BPDUs

1. **Configuration BPDU**
    
    - النوع الأساسي.
        
    - يُستخدم في انتخابات الـ Root Bridge وتوزيع المعلومات.
        
    - في STP: فقط الـ **Root Bridge** يرسلها كل 2 ثانية (Hello Time).
        
    - في RSTP: كل Switch يرسلها (Handshake سريع).
        
2. **Topology Change Notification (TCN) BPDU**
    
    - تُرسل لما يحدث تغيير (مثل: منفذ انتقل لـ Forwarding أو Blocked).
        
    - الهدف: إخبار جميع الـ Switches أن يعيدوا تحديث جداول الـ MAC.
        

---

## 📌 الحقول الأساسية في Configuration BPDU

|الحقل|الوظيفة|
|---|---|
|**Root ID**|معرّف الـ Root Bridge (Priority + MAC)|
|**Cost to Root**|تكلفة الطريق من المرسل حتى Root|
|**Bridge ID**|معرّف السويتش اللي أرسل BPDU|
|**Port ID**|المنفذ الذي أرسل BPDU|
|**Timers**|Hello, Forward Delay, Max Age|

---

## 📌 دور الـ BPDU في STP

1. **Election:** كل Switch يرسل BPDU → الأصغر Root ID يفوز ويصير Root Bridge.
    
2. **Path Selection:** الـ Switches يحسبوا أقل تكلفة طريق للـ Root استنادًا إلى BPDUs.
    
3. **Port Roles:** يحددوا Root Port, Designated Port, Blocked Port.
    
4. **Topology Changes:** إذا صار تغيير (كابل فصل، سويتش جديد) → يُرسل TCN BPDU عشان تتحدث الشبكة بسرعة.
    

---

## 📌 في STP vs RSTP

- **STP (802.1D):**
    
    - فقط Root Bridge يرسل BPDUs.
        
    - التوزيع بطيء (Convergence 30–50 ثانية).
        
- **RSTP (802.1W):**
    
    - كل Switch يرسل BPDUs (Handshake سريع).
        
    - التوزيع سريع (Convergence 1–10 ثوانٍ).
        

---

## 📌 العنوان MAC المستخدم

- كل BPDUs تُرسل إلى العنوان **Multicast**:
    
    ```
    01:80:C2:00:00:00
    ```
    

---

🔹 **الخلاصة:**  
**BPDUs = رسائل التحكم الأساسية في STP/RSTP**، وهي اللي تسمح للـ Switches يعرفوا مين Root Bridge، ويحددوا المسارات الأفضل، ويمنعوا الـ Loops.


[[50- Per-VLAN STP]]



