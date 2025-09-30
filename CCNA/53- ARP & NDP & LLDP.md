تمام! خلينا نوضح **ARP و NDP و LLDP** بالتفصيل، مع التطبيق العملي في **Packet Tracer**، عشان يكون عندك صورة واضحة عن كل واحد ووظيفته وطريقة رؤيته عمليًا.

---

## **1️⃣ ARP – Address Resolution Protocol**

### **وظيفته:**

- يستخدم في شبكات **IPv4** لترجمة **IP Address → MAC Address**.
    
- كل جهاز يريد إرسال حزمة في نفس الشبكة المحلية (LAN) يحتاج معرفة عنوان MAC للجهاز الهدف.
    

### **آلية العمل:**

1. الجهاز يرسل **ARP Request (Broadcast)** يسأل: "مين عنده هذا الـ IP؟"
    
2. الجهاز صاحب العنوان يرد بـ **ARP Reply (Unicast)** يحتوي على عنوان MAC.
    
3. يتم تخزين العنوان في **ARP Table** لفترة مؤقتة.
    

### **أوامر مهمة:**

```bash
show arp           # عرض جدول ARP
arp -a             # على Windows
```

### **تطبيق عملي في Packet Tracer:**

- ضع **PC1 و PC2** على نفس Switch.
    
- أعطِ كل PC عنوان IPv4.
    
- Ping من PC1 إلى PC2 → لاحظ في **Simulation Mode** ظهور ARP Request وReply.
    
- تحقق من جدول ARP على PC1.
    

---

## **2️⃣ NDP – Neighbor Discovery Protocol**

### **وظيفته:**

- يستخدم في شبكات **IPv6** بدلاً من ARP.
    
- يقوم بعدة وظائف:
    
    1. **Address Resolution** → ترجمة IPv6 إلى MAC (مثل ARP).
        
    2. **Neighbor Unreachability Detection (NUD)** → التحقق من أن الجار متاح.
        
    3. **Router Discovery & RA** → توزيع عناوين IPv6 تلقائياً (SLAAC).
        
    4. **Duplicate Address Detection (DAD)** → التأكد من أن العنوان غير مكرر.
        

### **آلية العمل:**

- **NS (Neighbor Solicitation)** → طلب MAC للجهاز الهدف.
    
- **NA (Neighbor Advertisement)** → الرد بعنوان MAC.
    
- **RA (Router Advertisement)** → الراوتر يرسل معلومات الشبكة لتكوين العنوان تلقائيًا.
    

### **أوامر مهمة:**

```bash
show ipv6 neighbors   # عرض جدول الجيران
ping <IPv6>           # يتحقق من NDP
```

### **تطبيق عملي في Packet Tracer:**

- ضع **Router وPC1 وPC2** على نفس LAN.
    
- Ping IPv6 من PC1 إلى PC2 → راقب NS وNA.
    
- اختبر SLAAC وRA عن طريق ترك PC على **Automatic IPv6**.
    

---

## **3️⃣ LLDP – Link Layer Discovery Protocol**

### **وظيفته:**

- بروتوكول **Layer 2** (Data Link).
    
- يسمح للأجهزة بالتعرف على الجيران المتصلين مباشرة.
    
- يرسل معلومات مثل:
    
    - اسم الجهاز
        
    - نوع الجهاز
        
    - المنفذ
        
    - VLAN
        
    - Capabilities (Switch/Router/Phone)
        

> مفيد في **Network Topology Discovery** وإدارة الشبكة.

### **أوامر مهمة على Cisco Devices:**

```bash
show lldp neighbors
show lldp neighbors detail
```

### **تطبيق عملي في Packet Tracer:**

1. شغل **LLDP** على **Switches وRouters**:
    

```bash
conf t
lldp run
```

2. شغل الأمر:
    

```bash
show lldp neighbors
```

3. سترى كل جهاز متصل بالمنفذ، نوعه، واسمه.
    
4. مفيد جدًا لتحديد العلاقات بين الأجهزة بدون معرفة مسبقة.
    

---

## **4️⃣ مقارنة سريعة**

|البروتوكول|الإصدار|الغرض الرئيسي|الطبقة|Broadcast/Unicast|
|---|---|---|---|---|
|ARP|IPv4|IP → MAC|L2|Broadcast Request, Unicast Reply|
|NDP|IPv6|IP → MAC + Neighbor Discovery + SLAAC + DAD|L3 + ICMPv6|Multicast NS, Unicast NA|
|LLDP|أي IP|كشف الأجهزة المجاورة|L2|Multicast (EtherType 0x88cc)|

---

إذا أحببت، أقدر أصنع لك **مختبر Packet Tracer جاهز** يشمل **ARP + NDP + LLDP** مع جميع الأجهزة والـ Simulation Mode جاهز لتشوف الحزم والتفاعل مباشرة.

[[54- LAG = Link Aggregation Group]]