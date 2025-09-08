

⸻

📌 التعريف • الـ Ethernet Frame هو “الظرف” أو “الحاوية” اللي تستخدمها شبكة Ethernet لنقل البيانات. • هو الشكل النهائي للمعلومة اللي تمشي داخل الشبكة عبر الطبقة الثانية (Data Link Layer). • بداخله تلاقي عنوان المرسل والمستقبل (MAC Address) + البيانات + بعض المعلومات للتحقق.

⸻

📦 مكونات الـ Ethernet Frame

يتكون من أجزاء رئيسية:

1. Preamble (7 Bytes) • سلسلة من الـ 0 و 1 (010101…) تساعد الجهاز المستقبل إنه يزامن التوقيت قبل وصول البيانات الفعلية.
2. Start Frame Delimiter (1 Byte) • يحدد بداية الإطار (Frame).
3. Destination MAC Address (6 Bytes) • عنوان الجهاز اللي راح يستقبل البيانات.
4. Source MAC Address (6 Bytes) • عنوان الجهاز اللي أرسل البيانات.
5. Type/Length (2 Bytes) • يحدد البروتوكول اللي داخل الإطار (مثلاً IPv4, IPv6, ARP).
6. Data (46–1500 Bytes) • الحمولة الفعلية (Payload) = المعلومات أو الـ IP Packet اللي نبي نرسلها.
7. Frame Check Sequence (FCS) – (4 Bytes) • كود للتحقق من الأخطاء (CRC) → يتأكد إن البيانات ما تلفت أثناء النقل.

⸻

🔎 مثال عملي

لو جهازك أرسل ملف صغير، يتحول إلى IP Packet، بعدين هذا الـ Packet ينحط داخل Ethernet Frame بهذا الشكل:

[Dst MAC][Src MAC][Type][Data(IP Packet)][FCS]

⸻

📌 الخلاصة • Ethernet Frame = الظرف • MAC Address = العنوان المكتوب على الظرف • IP Packet = الرسالة بداخل الظرف

⸻
![[Pasted image 20250908015702.png]]



[[19-  Internet Protocol Version 4 (IPv4)]]