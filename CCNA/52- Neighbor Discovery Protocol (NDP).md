
---

## **1️⃣ ما هو Neighbor Discovery Protocol (NDP)**

**NDP** هو بروتوكول ضمن IPv6 يقوم بوظائف مشابهة للبروتوكولات التالية في IPv4:

- **ARP** → حل عنوان IP إلى عنوان MAC.
    
- **ICMP Redirects** → إعلام الأجهزة بأفضل مسار للوصول للشبكة.
    
- **ICMP Router Discovery** → اكتشاف الراوترات المتاحة.
    

### **وظائف NDP الأساسية**

1. **Neighbor Solicitation (NS)**
    
    - يستخدم لطلب عنوان MAC لجهاز معروف عنوانه IPv6.
        
    - ترسل الحزمة إلى عنوان **Solicited-Node Multicast** للوجهة.
        
2. **Neighbor Advertisement (NA)**
    
    - الرد على NS، يحتوي على عنوان MAC الخاص بالجهاز.
        
3. **Router Solicitation (RS)**
    
    - يرسلها المضيف لاكتشاف راوترات الشبكة.
        
4. **Router Advertisement (RA)**
    
    - يرسلها الراوتر للإعلان عن نفسه، مع توجيهات الشبكة وميزات SLAAC (تخصيص عناوين تلقائية).
        
5. **Duplicate Address Detection (DAD)**
    
    - يتحقق من عدم وجود عنوان IPv6 مكرر قبل استخدامه.
        
6. **Reachability / NUD (Neighbor Unreachability Detection)**
    
    - يتحقق مما إذا كان الجار لا يزال متاحًا على الشبكة.
        

---

## **2️⃣ آلية عمل NDP**

1. جهاز يريد الاتصال بعنوان IPv6 معين → يرسل **NS**.
    
2. الجهاز صاحب العنوان → يرسل **NA** مع عنوان MAC.
    
3. الجهاز يضيف العنوان إلى جدول الجيران (Neighbor Cache).
    
4. في حال عدم وجود رد → الجهاز يعتبر الجار غير متاح (NUD).
    

**تفاعل DAD**:

- قبل تخصيص عنوان IPv6، يرسل المضيف NS إلى العنوان نفسه.
    
- إذا تلقى NA → العنوان مكرر → يجب تغييره.
    

---

## **3️⃣ التطبيق العملي على Packet Tracer**

### **المعدات**

- 2 × PCs
    
- 1 × Switch
    
- 1 × Router (اختياري لتجربة RA/SLAAC)
    

---

### **خطوات الإعداد**

#### **أ. توصيل الأجهزة**

```
PC1 --- Switch --- PC2
```

> أو لإضافة الراوتر: PC1 --- Switch --- Router --- Switch --- PC2

---

#### **ب. إعداد عناوين IPv6 ثابتة (Static IPv6)**

**على PC1**:

- IPv6 Address: `2001:db8:1::2`
    
- Prefix: `64`
    
- Default Gateway (إن وُجد الراوتر): `2001:db8:1::1`
    

**على PC2**:

- IPv6 Address: `2001:db8:1::3`
    
- Prefix: `64`
    
- Default Gateway: `2001:db8:1::1`
    

**على Router (R1 - إن وُجد)**:

```bash
R1> enable
R1# configure terminal
R1(config)# ipv6 unicast-routing
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ipv6 address 2001:db8:1::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
R1# write memory
```

---

#### **ج. اختبار Neighbor Discovery**

1. **من PC1 نفّذ ping** إلى PC2:
    

```
ping 2001:db8:1::3
```

2. **في Packet Tracer**:
    
    - استخدم **Simulation Mode** لرؤية الحزم.
        
    - سترى:
        
        - **Neighbor Solicitation (NS)** → طلب عنوان MAC.
            
        - **Neighbor Advertisement (NA)** → الرد بعنوان MAC.
            
        - ثم ICMPv6 Echo Request / Reply.
            

---

#### **د. فحص جدول الجيران**

**على Router أو PC في CLI:**

```bash
show ipv6 neighbors
```

- يظهر IPv6، عنوان MAC، والحالة (REACHABLE، STALE، INCOMPLETE).
    

---

#### **هـ. تجربة Duplicate Address Detection (DAD)**

1. حاول إعطاء PC1 نفس عنوان IPv6 الموجود على PC2 (`2001:db8:1::3`).
    
2. ستظهر رسالة تحذير في Packet Tracer أن العنوان مكرر → يوضح عمل DAD.
    

---

### **✅ ملاحظات مهمة**

- NDP يعتمد على **ICMPv6**.
    
- العناوين الخاصة بـ **Solicited-Node Multicast** تبدأ بـ `ff02::1:ffXX:XXXX`.
    
- NDP يدعم كل وظائف IPv6 بدون الحاجة لـ ARP.
    
- يمكنك تجربة **Router Advertisement / SLAAC** بإضافة راوتر وإعطاءه RA.
    

---
[[53- ARP & NDP & LLDP]]

