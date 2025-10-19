# شرح مفصّل — **Asymmetric Routing** (التوجيه غير المتماثل)

سأشرح لك الموضوع بشكل شامل: التعريف، الفرق مع **Symmetric Routing**، أسباب حدوثه، أمثلة عملية، المشاكل المحتملة، وكيفية التعامل معه في الشبكات الحقيقية.

---

## 1) ما هو **Asymmetric Routing**؟

**Asymmetric Routing** يعني أن **مسار الذهاب وحزم العودة لحزمة البيانات يمر عبر مسارات مختلفة**.

- مثال: إذا حزمة أُرسلت من الجهاز A إلى الجهاز B عبر Router1 → Router2، قد تعود الحزمة من B إلى A عبر Router3 → Router1.
    
- غالبًا يحدث في الشبكات الكبيرة، الإنترنت، أو عندما تكون هناك أكثر من وصلة للإنترنت (multi-homed).
    

---

## 2) الفرق بين Symmetric و Asymmetric Routing

|الخاصية|Symmetric Routing|Asymmetric Routing|
|---|---|---|
|مسار الذهاب والعودة|نفس المسار|مسار مختلف|
|Stateful Devices|يعمل بشكل صحيح|قد يفشل (packet dropped)|
|Troubleshooting|أسهل|أصعب|
|شائع على LAN|نعم|نادر|
|شائع على الإنترنت|لا|نعم|

---

## 3) أسباب حدوث **Asymmetric Routing**

1. **Multiple ISPs / Multi-homed Networks**
    
    - وجود أكثر من مزوّد إنترنت → حزم الذهاب قد تخرج من ISP1، والعودة عبر ISP2.
        
2. **Load Balancing / ECMP (Equal-Cost Multi-Path)**
    
    - إذا تم تفعيل ECMP بدون per-flow hashing → بعض الحزم قد ترجع عبر مسار مختلف.
        
3. **Routing Policy / Route Maps / BGP Attributes**
    
    - في BGP، استخدام **AS path prepending**, Local Preference أو route filtering قد يجعل المسارات مختلفة ذهاباً وعودة.
        
4. **Static Routes غير متناسقة**
    
    - إذا وضعت static routes للذهاب فقط ولم تضبط المسار المقابل → يحدث Asymmetric Routing.
        

---

## 4) مثال عملي على Asymmetric Routing

### شبكة بسيطة:

```
PC-A --- Router1 --- Router2 --- PC-B
      \
       \--- Router3
```

- الذهاب: PC-A → Router1 → Router2 → PC-B
    
- العودة: PC-B → Router2 → Router3 → Router1 → PC-A
    

> في هذه الحالة، الحزم الردّية لم ترجع بنفس الطريق، أي routing غير متماثل.

---

## 5) المشاكل المرتبطة بـ Asymmetric Routing

1. **Stateful Firewalls / NAT Devices**
    
    - الأجهزة التي تعتمد state table قد تحظر الحزم لأن الحزمة الردّية لم تأتِ من الواجهة المتوقعة.
        
2. **Troubleshooting أصعب**
    
    - تتبع المسار بالـ traceroute يظهر مسارات مختلفة → صعوبة في فهم الشبكة.
        
3. **VPN / IPSec**
    
    - معظم الـ VPN Tunnels تتطلب Symmetric Routing. الرد عبر مسار آخر يفشل الاتصال.
        
4. **Latency أو Packet Loss**
    
    - بعض المسارات البديلة أطول أو غير مستقرة → مشاكل أداء.
        

---

## 6) كيف يمكن التعامل مع Asymmetric Routing

1. **Force Symmetric Routing**
    
    - استخدام static routes أو route maps لتوجيه الذهاب والعودة عبر نفس المسار.
        
2. **Policy-Based Routing (PBR)**
    
    - إعادة توجيه حزم محددة لمسار معين لتضمن الذهاب والعودة متطابقة.
        
3. **Firewall / NAT Configuration**
    
    - على بعض firewalls يمكن السماح بـ asymmetric routing إذا كان state table يدعم.
        
4. **ECMP per-flow hashing**
    
    - لتجنب أن ترجع حزم نفس الاتصال عبر مسار مختلف.
        
5. **BGP Policies / Route Filtering**
    
    - ضبط Local Preference وAS-path لتوجيه العائدات عبر نفس الطريق.
        

---

## 7) أوامر التشخيص (Cisco)

1. **Traceroute**
    

```
traceroute <destination>
```

- يمكن أن تظهر مسارات مختلفة للذهاب والعودة.
    

2. **Show IP Route**
    

```
show ip route
```

- تحقق المسارات المختلفة للشبكات.
    

3. **Show Conn / Show Session** (على firewalls stateful)
    

```
show conn
show session
```

- تحقق من وجود dropped sessions بسبب asymmetric routing.
    

4. **Debug IP Routing** (بحذر على الأجهزة الإنتاجية)
    

```
debug ip routing
```

---

## 8) نصائح عملية

- على الشبكات الداخلية، **حاول دائماً Symmetric Routing** لتجنب مشاكل firewall/VPN.
    
- على الإنترنت العامة، **Asymmetric Routing شائع ولا يمكن تغييره دائماً**.
    
- إذا كان لديك Multi-homed، استخدم **BGP policies** لضمان العودة عبر نفس الطريق (Symmetric).
    
- في حالات Asymmetric لا يمكن تجنبها، تأكد أن أجهزة Stateful تدعم **asymmetric flows** أو استخدم NAT/Firewall مناسب.
    

---

## 9) ملخّص سريع

- **Asymmetric Routing** = مسار الذهاب مختلف عن مسار العودة.
    
- شائع على الإنترنت، أقل على LAN الداخلية.
    
- قد يسبب مشاكل مع firewalls stateful، NAT، VPN.
    
- الحل: static routes، PBR، ضبط BGP، أو firewall configuration.
    
- التشخيص: traceroute، show ip route، show conn/session.
    

---

إذا تحب، أقدر أرسم لك **مخطط يوضح الفرق بين Symmetric و Asymmetric Routing** مع مسارات الذهاب والعودة، بحيث تقدر تشوف الفرق بصريًا بسهولة.

هل تريد أن أرسمه لك؟

[[73- A Million Prefix]]