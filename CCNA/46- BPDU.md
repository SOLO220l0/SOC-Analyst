 موضوع **BPDU** لأنه أساس عمل **STP**:

---

## 📌 ما هو BPDU؟

- اختصار: **Bridge Protocol Data Unit**
    
- هو نوع خاص من الرسائل (Frames) يتبادلها الـ **Switches** فيما بينهم في الشبكة.
    
- الهدف: تبادل معلومات تساعد بروتوكول **STP (Spanning Tree Protocol)** على:
    
    1. **انتخاب الـ Root Bridge**
        
    2. **تحديد المنافذ (Ports) الفعّالة والمغلقة**
        
    3. **الحفاظ على استقرار الشبكة بدون Loops**
        

---

## 📌 أنواع BPDU

1. **Configuration BPDU**
    
    - تُستخدم في عمليات الانتخابات (Election).
        
    - تحتوي على معلومات مثل:
        
        - **Root Bridge ID** (Priority + MAC)
            
        - **Root Path Cost** (تكلفة المسار إلى Root Bridge)
            
        - **Sender Bridge ID** (معرّف السويتش اللي أرسل الـ BPDU)
            
        - **Port ID**
            
2. **Topology Change Notification (TCN) BPDU**
    
    - تُرسل لما يصير تغيير في الشبكة (مثلاً منفذ يفتح أو ينغلق).
        
    - الهدف: تنبيه كل الـ Switches أن الشبكة لازم تعيد الحسابات (Recalculate).
        

---

## 📌 خصائص مهمة

- **من يرسل BPDUs؟**
    
    - كل Switch يرسل BPDUs، لكن الـ **Root Bridge** هو المصدر الأساسي للـ **Configuration BPDUs**.
        
- **كم مرة تُرسل؟**
    
    - كل **2 ثانية** بشكل افتراضي (Hello Time).
        
- **إلى أي عنوان MAC تُرسل؟**
    
    - تُرسل إلى **Multicast MAC: 01:80:C2:00:00:00**
        

---

## 📌 دور BPDU في STP

1. **البداية:** كل Switch يفترض نفسه Root Bridge ويرسل BPDU.
    
2. **المقارنة:** الـ Switches يقارنوا الـ Bridge ID.
    
3. **الانتخاب:** الأصغر يفوز ويصير Root Bridge.
    
4. **التوزيع:** Root Bridge يستمر في إرسال BPDUs لتأكيد وضعه.
    
5. **التغيير:** لو تغيرت الشبكة، يرسل Switch رسالة **TCN BPDU** لباقي السويتشات.
    

---

## 📌 مثال مبسط

- عندك 3 Switches: S1, S2, S3
    
- كل واحد يرسل BPDU بمعلوماته.
    
- لو S1 عنده **Priority أقل + MAC أصغر** → يفوز ويصير Root Bridge.
    
- S2 و S3 يقبلوا BPDUs منه، ويحسبوا مساراتهم بناءً على بياناته.
    

---

🔹 **الخلاصة:**  
**BPDU = رسائل STP الأساسية**، بدونها ما يقدر البروتوكول يحدد Root Bridge أو يمنع Loops.

[[47- Optimal Path]]