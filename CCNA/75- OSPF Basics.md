# شرح مفصّل — **OSPF Basics** (أساسيات OSPF)

هنا شرح عربي عملي ومفصّل مع أمثلة أوامر ونقاط تشخيص — مناسب للمبتدئين والممارسين.

---

## 1) ما هو OSPF باختصار؟

**OSPF (Open Shortest Path First)** هو بروتوكول توجيه داخلي (IGP) من نوع **link-state** يُستخدم داخل نفس الـ Autonomous System. يجمَع معلومات حالة الروابط من كل الراوترات ويبني **قاعدة حالة الروابط (LSDB)** ثم يحسب أشجار أقصر مسار (SPF) باستخدام خوارزمية Dijkstra ليُكوّن جدول التوجيه. ([rfc-editor.org](https://www.rfc-editor.org/rfc/rfc2328.html?utm_source=chatgpt.com "RFC 2328: OSPF Version 2"))

---

## 2) المكونات والمفاهيم الأساسية

- **Router ID (RID):** رقم 32-bit يميّز كل راوتر (تُستخدم أعلى عنوان IP للـ loopback إن وُجد، أو أعلى واجهة). ([Cisco](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/xe-16/iro-xe-16-book/iro-cfg.html?utm_source=chatgpt.com "IP Routing: OSPF Configuration Guide"))
    
- **Area (المنطقة):** تقسيم هرمي للمجال لتقليل حجم قاعدة الحالة (Area 0 = backbone). الرواتر التي تربط مناطق تسمى **ABR**. ([Cisco](https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/13703-8.html?utm_source=chatgpt.com "Understand OSPF Areas and Virtual Links"))
    
- **LSA (Link-State Advertisement):** رسائل تُعلن عن الروابط والشبكات — لها أنواع مختلفة (Type-1..Type-5, وغيرها) وتُستخدم لبناء LSDB. ([NetworkLessons.com](https://networklessons.com/ospf/ospf-lsa-types-explained?utm_source=chatgpt.com "OSPF LSA Types Explained"))
    
- **Neighbor & Adjacency:** الجيران (neighbors) يتكوَّن بينهم adjacency اعتماداً على نوع الشبكة (مثلاً على broadcast تُنتخب DR/BDR).
    
- **SPF Calculation:** بعد تلقي LSAs، كل راوتر ينفذ Dijkstra ليبني شجرة أقصر طرق ويملأ جدول التوجيه. ([rfc-editor.org](https://www.rfc-editor.org/rfc/rfc2328.html?utm_source=chatgpt.com "RFC 2328: OSPF Version 2"))
    

---

## 3) أنواع الشبكات وسلوك OSPF على كل نوع

- **Broadcast (Ethernet):** يُنتخب DR وBDR لتقليل الفيضانات.
    
- **Point-to-Point:** لا حاجة لانتخاب DR؛ adjacency مباشر.
    
- **NBMA (مثل Frame-Relay):** قد تحتاج تكوين يدوياً أو استخدام الـ neighbors/login; قد تُحدَد DR/BDR.
    
- **Point-to-Multipoint:** تُعامَل كعدة وصلات نقطة-لـنقطة (لا DR عادة). ([Wikipedia](https://en.wikipedia.org/wiki/Open_Shortest_Path_First?utm_source=chatgpt.com "Open Shortest Path First"))
    

---

## 4) LSA — أنواع مهمة (مبسط)

- **Type 1 (Router LSA):** يصف واجهات الراوتر داخل نفس الـ area — يُبث فقط داخل الـ area.
    
- **Type 2 (Network LSA):** يعلن عنها الـ DR لوصف الشبكات الـ broadcast داخل الـ area.
    
- **Type 3 (Summary LSA):** يعلنها الـ ABR لنقل معلومات عن شبكة من area أخرى (Area→Backbone→Other Areas).
    
- **Type 4 (ASBR Summary):** إعلان عن موقع الـ ASBR إلى بقية الـ AS.
    
- **Type 5 (External LSA):** تصدرها الـ ASBR لتعريف مسارات خارجية (مثل شبكات من BGP).  
    (هناك أيضاً Type-7 للنُـوّع NSSA). ([NetworkLessons.com](https://networklessons.com/ospf/ospf-lsa-types-explained?utm_source=chatgpt.com "OSPF LSA Types Explained"))
    

---

## 5) مناطق OSPF وأنواعها

- **Backbone (Area 0):** جميع الـ areas الأخرى يجب أن تتصل بالـ Area 0 (أو عبر virtual link).
    
- **Standard Area (Normal)** — ينشر Type-1/2/3/4/5 LSAs.
    
- **Stub Area:** لا يقبل Type-5 externals؛ يستخدم default summary من الـ ABR لتقليل الجداول.
    
- **Totally Stubby Area:** لا يقبل Type-3 ولا Type-5 (فقط default).
    
- **NSSA (Not-So-Stubby Area):** يسمح بإدخال بعض المسارات الخارجية محلياً كـ Type-7 ثم تُحوّل إلى Type-5 عند الـ ABR. ([Cisco](https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/13703-8.html?utm_source=chatgpt.com "Understand OSPF Areas and Virtual Links"))
    

---

## 6) كيفية عمل الجيران (Neighbor Formation) بسرعة

1. تبادل Hello packets على الواجهة (تضم Router ID، hello/dead timers، area).
    
2. إذا توافقت الإعدادات (area, authentication, timers) → يتكوَّن الجار (neighbor).
    
3. على broadcast، بعد election يكوّن الـ DR adjacency مع كل الراوترات الأخرى عبر الـ DR.
    
4. بمجرد الجار متصل → يتم تبادل Database Description (DD), Link State Requests, ثم Link State Updates. ([rfc-editor.org](https://www.rfc-editor.org/rfc/rfc2328.html?utm_source=chatgpt.com "RFC 2328: OSPF Version 2"))
    

---

## 7) أمثلة تكوين أساسي (Cisco IOS) — OSPFv2 (IPv4)

```cisco
! 1) تفعيل OSPF مع Process ID = 1
router ospf 1
 router-id 1.1.1.1       ! يفضل ضبط loopback وتعيينه هنا
 network 10.0.0.0 0.0.0.255 area 0
 network 192.168.1.0 0.0.0.255 area 1

! 2) مثال ضبط واجهة (بدل استخدام network)
interface GigabitEthernet0/0
 ip address 10.0.0.1 255.255.255.0
 ip ospf 1 area 0

! 3) جعل الواجهة passive (لا تُنشئ neighborship)
router ospf 1
 passive-interface GigabitEthernet0/1
```

> ملاحظة: `network` في الـ router-ospf يربط الـ interfaces المطابقة مع المنطقة (area). ([NetworkLessons.com](https://networklessons.com/ospf/basic-ospf-configuration?utm_source=chatgpt.com "Basic OSPF Configuration"))

---

## 8) Metrics / Cost وطرق ضبطها

- **Cost** = مقياس يستخدمه OSPF لاختيار المسار (افتراضياً 100 Mbps = cost 1 على أجهزة Cisco القديمة). يُحسب عادةً: `cost = reference bandwidth / interface bandwidth`. يمكنك ضبط `ip ospf cost <value>` على الواجهة أو تغيير `auto-cost reference-bandwidth` ليتناسب مع روابط 10G/40G/100G. ([Cisco](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/xe-16/iro-xe-16-book/iro-cfg.html?utm_source=chatgpt.com "IP Routing: OSPF Configuration Guide"))
    

---

## 9) عملية SPF وConvergence

- عند تغيّر الحالة (مثل سقوط رابط)، راوترٌ ما يولّد LSA محدثة ويُرسِلها إلى الجيران → تُبث عبر الـ area → كل راوتر يُعيد تشغيل SPF لحساب المسارات الجديدة.
    
- OSPF يستخدم تقنيات مثل **LSA throttling** و**incremental SPF** لتقليل عبء الـ CPU عند تغيّرات متكررة. ([NetworkLessons.com](https://notes.networklessons.com/ospf?utm_source=chatgpt.com "OSPF - Notes"))
    

---

## 10) OSPF External Routes وType-5

- الراوتر الذي يربط الـ AS بعالم خارجي (مثل BGP) يُصبح **ASBR** ويصدر Type-5 LSAs لتعريف الشبكات الخارجية. يمكن تصنيفها كـ **E1** أو **E2** — الفرق في احتساب الـ cost (E1 تجمع cost داخل الـ AS + external cost، E2 تستخدم cost خارجي ثابت عادة). ([rfc-editor.org](https://www.rfc-editor.org/rfc/rfc2328.html?utm_source=chatgpt.com "RFC 2328: OSPF Version 2"))
    

---

## 11) OSPFv3 (IPv6) — لمحة

- OSPFv3 مهيأ لـ IPv6 (RFC 5340) — يستخدم link-local لانسِداد الجيران، ويفصل بين الـ LSA والمعلومات المتعلقة بـ IPv6 prefixes. معظم مفاهيم OSPFv2 نفسها تظل موجودة لكن مع فروق تنفيذية. ([documentation.nokia.com](https://documentation.nokia.com/nsp/24-8/User_Guide/ai8ha35pr5.html?utm_source=chatgpt.com "OSPFv2 and OSPv3"))
    

---

## 12) أوامر تشخيصية مهمة (Cisco)

- `show ip ospf neighbor` — تحقق حالة الجيران (Full, 2WAY, etc.).
    
- `show ip ospf` — معلومات عامة عن عملية OSPF (router id, area).
    
- `show ip ospf database` — يعرض LSDB وLSA types.
    
- `show ip route ospf` — مسارات المتعلمة عبر OSPF في جدول التوجيه.
    
- `debug ip ospf events` / `debug ip ospf adj` — للتفاصيل الحية (استخدمها بحذر في بيئة إنتاج). ([Cisco](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/xe-16/iro-xe-16-book/iro-cfg.html?utm_source=chatgpt.com "IP Routing: OSPF Configuration Guide"))
    

---

## 13) نصائح عملية / Best Practices

- ضع **Area 0** مركزياً واجعل كل ABR متصل به (أو استخدم virtual links كحل أخير). ([Cisco](https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/13703-8.html?utm_source=chatgpt.com "Understand OSPF Areas and Virtual Links"))
    
- استخدم **loopback interface** كـ Router-ID (ثابت وموثوق).
    
- ضبط `passive-interface` على الـ interfaces التي لا تحتاج لجيران (أمن وكفاءة).
    
- راقب **LSA growth** وحدد سياسات (LSA filtering, summarization) خصوصاً على الـ ABRs لتقليل حجم الـ LSDB. ([NetworkLessons.com](https://notes.networklessons.com/ospf?utm_source=chatgpt.com "OSPF - Notes"))
    

---

## 14) موارد موثوقة للمراجعة والتعمق

- RFC 2328 — OSPFv2 (المرجع الرسمي). ([rfc-editor.org](https://www.rfc-editor.org/rfc/rfc2328.html?utm_source=chatgpt.com "RFC 2328: OSPF Version 2"))
    
- Cisco OSPF Configuration Guide (أمثلة أوامر وإعدادات). ([Cisco](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/xe-16/iro-xe-16-book/iro-cfg.html?utm_source=chatgpt.com "IP Routing: OSPF Configuration Guide"))
    
- شروحات تفصيلية عن أنواع LSAs وعملياتهم (NetworkLessons / FRRouting docs). ([NetworkLessons.com](https://networklessons.com/ospf/ospf-lsa-types-explained?utm_source=chatgpt.com "OSPF LSA Types Explained"))
    

---

### تحب أعمل لك الآن؟

- سيناريو عملي (شبكة مكوّنة من 3 راوترات + مناطق) مع أوامر جاهزة للتطبيق والاختبار؟
    
- أو رسم توضيحي يبيّن مرحلة Neighbor → DB Exchange → SPF → Route install؟
    

قل أي خيار تريده وأنا أطبّقه لك فوراً. 🚀