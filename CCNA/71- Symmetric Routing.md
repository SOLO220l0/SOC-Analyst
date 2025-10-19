# شرح مفصّل — **Symmetric Routing** (التوجيه المتماثل)

سأشرح لك الموضوع بشكل كامل: المفهوم، الفرق مع **Asymmetric Routing**، الحالات العملية، لماذا مهم، وملاحظات في الشبكات الحقيقية.

---

## 1) ما هو **Symmetric Routing**؟

**Symmetric Routing** يعني أن **مسار الذهاب والعودة لحزمة البيانات عبر الشبكة يكون نفسه**.  
بعبارة أخرى: إذا حزمة أُرسلت من الجهاز A إلى الجهاز B عبر مسار معين، فإن الحزمة الردّية من B إلى A تستخدم **نفس المسار تماماً**.

- مهم للـ firewalls، NAT devices، VPN tunnels، أو أي جهاز يعتمد stateful inspection (مثل ASA firewall أو FortiGate).
    
- إذا لم يحدث هذا → تصبح الحزمة الردّية تتبع مسار مختلف → مشاكل في الاتصالات أو فشل في stateful devices.
    

---

## 2) الفرق بين Symmetric و Asymmetric Routing

|الخاصية|Symmetric|Asymmetric|
|---|---|---|
|مسار الذهاب والعودة|نفس المسار|مسار مختلف|
|Stateful Devices|يعمل بشكل صحيح|قد يفشل (packet dropped)|
|Troubleshooting|أسهل|أصعب|
|مثال عملي|A → R1 → R2 → B, B → R2 → R1 → A|A → R1 → R2 → B, B → R3 → R1 → A|

> غالباً على الإنترنت العامة يحدث **Asymmetric Routing** بسبب تعدد المسارات والـ BGP policy، لكنه مقبول إذا لا توجد أجهزة Stateful على الطريق.

---

## 3) مثال عملي على Symmetric Routing

### شبكة بسيطة:

```
PC-A --- Router1 --- Router2 --- PC-B
```

1. PC-A يرسل إلى PC-B.
    
2. الحزمة تمر عبر R1 → R2 → PC-B.
    
3. الردّ من PC-B يجب أن يعود عبر R2 → R1 → PC-A (نفس المسار).
    

- إذا هناك Router3 يمر عليه الردّ بدل R2 → R1 → PC-A → يصبح asymmetric → قد يسبب drop إذا كان هناك firewall stateful.
    

---

## 4) لماذا مهم؟

1. **Stateful Firewalls / NAT**
    
    - أجهزة مثل firewalls تحفظ حالة كل اتصال (state table).
        
    - إذا ردّ البيانات لم يعد عبر نفس الواجهة، يتم رفض الحزمة لأنها غير متوقعة.
        
2. **VPN Tunnels**
    
    - IPSec يحتاج للحزم العودة عبر نفس النفق، خلاف ذلك فشل الاتصال.
        
3. **Troubleshooting أسهل**
    
    - Symmetric Routing يجعل تتبع المسار وفهم المشاكل أسهل.
        

---

## 5) كيف نحقق Symmetric Routing؟

1. **Static Routes**:
    
    - ضبط static routes على الراوترات بحيث توجيه الذهاب والعودة عبر نفس المسار.
        
2. **Dynamic Routing Protocols مع سياسة**:
    
    - بعض البروتوكولات (مثل OSPF, EIGRP) تضمن Symmetric Routing إذا كان كل الراوترات متطابقة.
        
    - BGP قد ينتج عنه Asymmetric Routing بسبب اختلاف policy أو prepending.
        
3. **VRF / Policy-based Routing**:
    
    - تحديد مسار خاص لبعض الحزم لضمان الذهاب والعودة عبر نفس المسار.
        
4. **تجنب ECMP غير متساوي بدون ضبط**:
    
    - في بعض الشبكات، ECMP (Equal-Cost Multi-Path) يوزع الحزم بشكل round-robin → قد ينتج asymmetric على مستوى الحزمة. استخدم **per-flow hashing** لتجنب ذلك.
        

---

## 6) مثال على مشكلة Asymmetric Routing

- PC-A → Router1 → Router2 → PC-B → Router3 → Router1 → PC-A
    
- إذا Router1 لديه firewall stateful، الرد من PC-B قد يتم رفضه لأنه لم يأتِ من الواجهة المتوقعة.
    

---

## 7) أوامر تشخيص على Cisco

1. **تحقق من مسارات الذهاب والعودة**:
    

```
show ip route
traceroute <destination>
```

- يمكن مقارنة المسارات ذهاباً وعودة للتأكد من Symmetric.
    

2. **تحقق من ECMP أو multipath**:
    

```
show ip route | include <network>
show ip cef
```

3. **تحقق من stateful firewall**:
    

- عادة على جهاز الـ firewall:
    

```
show conn
show session
```

- تأكد أن كل اتصال لديه state entry.
    

---

## 8) نصائح وملاحظات

- في الشبكات الداخلية، Symmetric Routing **مفضل** خاصة عند وجود NAT/Firewall/VPN.
    
- على الإنترنت العامة، Asymmetric Routing شائع، وحل المشكلات يحتاج تتبع دقيق.
    
- لتصحيح Symmetric Routing: ضبط static routes أو سياسة BGP/OSPF أو استخدام tunnel مخصص.
    
- دائما راقب **next-hop consistency** لضمان العودة عبر نفس المسار.
    

---

إذا تحب، أقدر أرسم لك **مخطط توضيحي** يظهر الفرق بين **Symmetric vs Asymmetric Routing** مع مسارات الذهاب والعودة، بحيث يكون سهل الفهم.

هل تريد أن أرسمه لك؟
[[72- Asymmetric Routing]]
