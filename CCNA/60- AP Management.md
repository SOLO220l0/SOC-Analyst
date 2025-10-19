
---

## **1. المنافذ في نقاط الوصول (AP Ports)**

- بعض نقاط الوصول (**APs**) تحتوي على:
    
    - **PoE Port**: لتزويد الطاقة عبر الكابل (Power over Ethernet)
        
    - **AUX Port**: منفذ إضافي يمكن استخدامه لنقل البيانات
        
- **دمج المنافذ (Port Bundling / Aggregation):**
    
    - يمكن دمج هذين المنفذين لتشكيل **واجهة بيانات بعرض نطاق أكبر** (Higher Bandwidth).
        

---

## **2. منافذ إدارة وحدة التحكم (WLC Management Port)**

- وحدات التحكم (**WLC**) تحتوي على **منفذ إدارة/خدمة (Service/Management Port)**
    
- يمكن تعيين **عنوان IP** لهذا المنفذ للوصول إلى واجهة الإدارة الرسومية (**GUI**) من المتصفح.
    

---

## **3. دمج المنافذ (Port Aggregation / Channel Group)**

### على السويتش:

- استخدم الأمر:
    

```
channel-group mode on 
```

- ملاحظة: بعض WLCs **لا تدعم LACP أو PAgP**، لذلك الوضع "on" مطلوب.
    

### على نقطة الوصول (AP):

- الوضع يمكن أن يكون:
    
    - **ON** → تفعيل التجميع مباشرة
        
    - **LACP** → بروتوكول تجميع تلقائي، لكن فقط على نقاط الوصول **المحلية (Local APs)**
        
- **لا يمكن** استخدام التجميع على نقاط الوصول **المستقلة (Autonomous APs)**.
    

---

## **4. إدارة APs و WLCs**

- مثل أي جهاز شبكي آخر، يمكن إدارتهم عبر:
    
    1. **CLI (Command Line Interface):**
        
        - Console
            
        - Telnet
            
        - SSH
            
    2. **GUI (Graphical User Interface):**
        
        - HTTP
            
        - HTTPS
            
- **إدارة الوصول والتصديق (Authorization):**
    
    - يمكن تطبيق **AAA (Authentication, Authorization, Accounting)** للتحكم في من يمكنه إدارة الجهاز.
        

---

إذا تحب، أقدر أرسم لك **مخطط بالعربي يوضح AP مع منافذ PoE و AUX ودمجها، وكيف WLC يتصل لإدارة APs**، لتكون الصورة واضحة.

[[61- (LAPs - Lightweight Access Points)]]