⸻

📌 ما هو Network Architecture Model؟

هو إطار (Framework) يوضح كيفية تصميم وبناء الشبكات، بحيث يحدد الطبقات، المكونات، البروتوكولات، وكيفية تفاعلها مع بعضها. الهدف منه هو جعل الشبكات أكثر تنظيمًا، سهولة في الفهم، والإدارة.

⸻

🏛️ أنواع نماذج معمارية الشبكات الأساسية:

١. OSI Model (Open Systems Interconnection) • نموذج نظري من ٧ طبقات وضعته ISO. • الهدف منه: توحيد تصميم الشبكات والبروتوكولات.

🔹 الطبقات السبعة (من الأعلى للأسفل):

١. Application Layer – التطبيقات (HTTP, FTP, DNS).

٢. Presentation Layer – التشفير/الضغط/الترميز.

٣. Session Layer – إدارة الجلسات (Session Control).

٤. Transport Layer – النقل (TCP/UDP).

٥. Network Layer – التوجيه والعناوين (IP).

٦. Data Link Layer – التحكم في الإطار (Ethernet, MAC).

٧. Physical Layer – الإشارات والوسائط الفيزيائية (كابلات، Wi-Fi).

⸻

٢. TCP/IP Model • النموذج العملي المستخدم فعليًا في الإنترنت. • يحتوي على ٤ طبقات (مبني على OSI لكن أبسط):

١. Application Layer – التطبيقات (HTTP, SMTP, DNS).

٢. Transport Layer – TCP / UDP.

٣. Internet Layer – IP (IPv4, IPv6).

٤. Network Access Layer – Ethernet, Wi-Fi.

⸻

٣. Cisco Three-Layer Hierarchical Model • نموذج عملي تستخدمه Cisco في تصميم الشبكات الداخلية (LAN/WAN). • يتكون من ٣ طبقات:

١. Core Layer – العمود الفقري (سرعة عالية، ربط الشبكة).

٢. Distribution Layer – توزيع السياسات، الفلاتر، الـ VLANs.

٣. Access Layer – المستخدمين والأجهزة (PCs, Printers, APs).

⸻

٤. Spine-Leaf Architecture (Datacenter) • تصميم حديث يستخدم في مراكز البيانات. • يتكون من: • Spine Switches (عمود فقري سريع). • Leaf Switches (يتصل بها السيرفرات مباشرة). • يتميز بسرعة عالية، تقليل الـ Latency، ودعم الـ Cloud.

⸻

🎯 الهدف من استخدام نماذج معمارية الشبكات

١. توحيد التصميم بين الشركات والمطورين.

٢. تسهيل الفهم (التقسيم لطبقات يجعل كل وظيفة واضحة).

٣. قابلية التوسع (سهولة إضافة مكونات جديدة).

٤. سهولة الصيانة (تحديد مكان المشكلة بسرعة).

٥. الأمن والسياسات (كل طبقة يمكن التحكم فيها بشكل منفصل).

⸻

[[5- Physical Link - Physical Layer]]

[[ملاحظة مهمة لل N.ID & B.ID]]