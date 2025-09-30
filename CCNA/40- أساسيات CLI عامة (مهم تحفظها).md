
# بدءاً من الصفر — إعداد البيئة في Packet Tracer

1. افتح Packet Tracer → اسحب أجهزة (Routers, Switches, PCs).
    
2. وصل الكابلات:
    
    - بين Router ↔ Switch: `Copper Straight-Through` أو `Auto` (PT يتعرف تلقائياً).
        
    - بين Switch ↔ PC: `Copper Straight-Through`.
        
    - Router ↔ Router: `Copper Crossover` أو `Serial` لو تسوي Serial.
        
3. اضغط على جهاز → CLI لفتح واجهة الاعداد. استخدم وضع **Simulation** لتتبع الـ packets و **Realtime** للتشغيل الطبيعي.
    

---

# أساسيات CLI عامة (مهم تحفظها)

```text
-- الدخول إلى المودز
Router> enable                # من user EXEC الى privileged EXEC
Router# configure terminal    # من privileged إلى Global Config

-- حفظ
Router# write memory          # أو
Router# copy running-config startup-config

-- الخروج
Router(config)# exit
Router# disable

-- أوامر فحص أساسية
show running-config
show startup-config
show ip interface brief
show interfaces
show ip route
show mac address-table       # على السويتش
```

---

# 1) إعداد راوتر بسيط (Interface IP + NAT example)

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1

# إعداد الواجهة
R1(config)# interface gigabitEthernet0/0
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

# إذا فيه واجهة للخارج (مثال للإنترنت)
R1(config)# interface gigabitEthernet0/1
R1(config-if)# ip address 200.10.10.2 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

# تمكين NAT (مثال بسيط PAT)
R1(config)# access-list 1 permit 192.168.10.0 0.0.0.255
R1(config)# ip nat inside source list 1 interface gigabitEthernet0/1 overload
R1(config)# interface gigabitEthernet0/0
R1(config-if)# ip nat inside
R1(config-if)# exit
R1(config)# interface gigabitEthernet0/1
R1(config-if)# ip nat outside
R1(config-if)# exit

# حفظ
R1# copy running-config startup-config
```

---

# 2) إعداد سويتش أساسي (VLANs + SVI + Trunk)

### تكوين سويتش: إنشاء VLANs، وضع المنافذ Access، إعداد Trunk بين Switch & Router (Router-on-a-stick)

```text
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1

# إنشاء VLANs
SW1(config)# vlan 10
SW1(config-vlan)# name Sales
SW1(config-vlan)# exit
SW1(config)# vlan 20
SW1(config-vlan)# name IT
SW1(config-vlan)# exit

# تعيين منافذ Access
SW1(config)# interface range g0/1 - 2
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 10
SW1(config-if-range)# exit

SW1(config)# interface range g0/3 - 4
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 20
SW1(config-if-range)# exit

# تعيين منفذ للـ Trunk (إلى الراوتر أو سويتش آخر)
SW1(config)# interface g0/24
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk encapsulation dot1q    # في بعض السويتشات
SW1(config-if)# exit

# إعداد SVI (إذا السويتش Layer3 يدعم أو للـ inter-VLAN)
SW1(config)# interface vlan 10
SW1(config-if)# ip address 192.168.10.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
```

### Router-on-a-stick (على الراوتر واجهة فرعية لكل VLAN)

```text
R1(config)# interface g0/1.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1(config-subif)# exit

R1(config)# interface g0/1.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
R1(config-subif)# exit
```

---

# 3) DHCP Server (Router as DHCP)

```text
# إعداد pool
R1(config)# ip dhcp pool LAN10
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# exit

# استثناء عناوين ثابتة
R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
```

---

# 4) NAT Static و Dynamic (أساسيات)

```text
# Static NAT (مثال: public IP 200.10.10.10 maps to host 192.168.10.50)
R1(config)# ip nat inside source static 192.168.10.50 200.10.10.10

# Dynamic PAT (سبق مثال أعلى) ip nat inside source list 1 interface g0/1 overload
```

---

# 5) ACL (Access-Lists) أمثلة شائعة

### Standard ACL (حسب source only)

```text
R1(config)# access-list 10 permit 192.168.10.0 0.0.0.255
R1(config)# interface g0/0
R1(config-if)# ip access-group 10 in
```

### Extended ACL (source + dest + proto + port)

```text
# منع HTTP من شبكة 192.168.10.0 إلى 8.8.8.8
R1(config)# access-list 100 deny tcp 192.168.10.0 0.0.0.255 host 8.8.8.8 eq 80
R1(config)# access-list 100 permit ip any any
R1(config)# interface g0/0
R1(config-if)# ip access-group 100 out
```

---

# 6) STP (Spanning Tree) — أوامر أساسية على السويتش

```text
# عرض حالة STP
SW1# show spanning-tree

# تعيين أولويات root (لتحديد من سيكون الـ Root Bridge)
SW1(config)# spanning-tree vlan 1 priority 4096
```

---

# 7) EtherChannel (LACP) — تجميع المنافذ

```text
# على Switch A:
SW1(config)# interface range g0/1 - 2
SW1(config-if-range)# channel-group 1 mode active   # LACP (active/ passive)
SW1(config-if-range)# exit
SW1(config)# interface port-channel 1
SW1(config-if)# switchport mode trunk   # أو access حسب التصميم

# على Switch B:
SW2(config)# interface range g0/1 - 2
SW2(config-if-range)# channel-group 1 mode active
```

---

# 8) Port Security (حماية منافذ Access)

```text
SW1(config)# interface g0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 1
SW1(config-if)# switchport port-security violation shutdown
SW1(config-if)# switchport port-security mac-address sticky 0200.AAAA.AAAA
```

---

# 9) Routing Protocols — إعدادات أساسية

### OSPF (مثال)

```text
R1(config)# router ospf 1
R1(config-router)# network 192.168.10.0 0.0.0.255 area 0
R1(config-router)# network 10.0.0.0 0.0.0.255 area 0
```

### EIGRP (مثال)

```text
R1(config)# router eigrp 100
R1(config-router)# network 192.168.10.0
R1(config-router)# network 10.0.0.0
R1(config-router)# no auto-summary
```

---

# 10) VLAN Routing — SVI (Layer 3 Switch)

```text
SW1(config)# interface vlan 10
SW1(config-if)# ip address 192.168.10.1 255.255.255.0
SW1(config-if)# no shutdown
# فعل ip routing على السويتش لعمل routing بين SVIs
SW1(config)# ip routing
```

---

# 11) DHCP Snooping, ARP Inspection, and Security features (مقدمة)

```text
# تمكين DHCP Snooping
SW1(config)# ip dhcp snooping
SW1(config)# ip dhcp snooping vlan 10,20
# تعيين منافذ موثوقة (Trunk/upstream)
SW1(config)# interface g0/24
SW1(config-if)# ip dhcp snooping trust

# Dynamic ARP Inspection يعتمد على DHCP snooping binding
SW1(config)# ip arp inspection vlan 10,20
```

---

# 12) Troubleshooting Commands — الأهم

```text
show ip interface brief
show ip route
show running-config
show interfaces status
show arp
show mac address-table
show cdp neighbors          # أو lldp neighbors
ping 192.168.10.1
traceroute 8.8.8.8
debug ip icmp               # (احذر في بيئات حية؛ في PT آمن)
```

---

# 13) Packet Tracer Tips & Tricks

- استخدم **Simulation mode**: اضغط Play → ضع فلتر على البروتوكولات (ARP, ICMP, DHCP, STP, OSPF...) لمشاهدة الـ packet flow خطوة بخطوة.
    
- احفظ ملف `.pkt` دائماً.
    
- افتح نافذة **Logical Workspace** و **Physical Workspace** لفهم التوصيلات فعليًا.
    
- استخدم **Inspect Tool** على أي رابط لعرض التفاصيل (معدل النقل، duplex, error...).
    
- تستطيع سحب أجهزة Virtual Machines وServers من قائمة End Devices (Server) وتشغيل DHCP/TFTP/HTTP داخلها.
    

---

# 14) مسارات تدريبية (من الصفر للاحتراف)

1. **أساسيات الشبكات**: IP addressing, subnetting, cables, basic switch/router. (تمارين: بناء شبكة بسيطة، ping بين PCs)
    
2. **Switching**: VLANs, Trunking, STP, PortSecurity. (تمارين: فصل VLANs، router-on-a-stick)
    
3. **Routing Static + Default**: static routes, default gateway. (تمارين: ربط شبكتين عبر راوترين)
    
4. **Dynamic Routing**: OSPF/EIGRP basics → redistribution → metrics. (تمارين: 3 راوترات في area)
    
5. **Services**: DHCP, NAT, DNS, NTP. (تمارين: إعداد DHCP + NAT للإنترنت)
    
6. **Security**: ACLs, Port Security, DHCP Snooping, DAI. (تمارين: منع HTTP من شبكة معينة)
    
7. **Advanced**: EtherChannel, HSRP/VRRP, QoS basics.
    
8. **Design & Troubleshoot**: جمع كل التقنيات في سيناريو حقيقي (مكتب، DMZ، remote access).
    

---

# أمثلة مختصرة (ثلاث سيناريوهات جاهزة تنسخها في Packet Tracer)

### سيناريو 1: شبكة بسيطة (PC ↔ Switch ↔ Router ↔ Internet)

- ضع PC1 على VLAN10، PC2 على VLAN20، Router-on-a-stick لإتاحة اتصال بين الـ VLANs والإنترنت عبر NAT. (الأوامر أعلاه)
    

### سيناريو 2: OSPF بين 3 راوترات

```text
# Router1
router ospf 1
 network 10.0.12.0 0.0.0.255 area 0
 network 192.168.1.0 0.0.0.255 area 0

# Router2
router ospf 1
 network 10.0.12.0 0.0.0.255 area 0
 network 10.0.23.0 0.0.0.255 area 0

# Router3
router ospf 1
 network 10.0.23.0 0.0.0.255 area 0
```

### سيناريو 3: Port Security blocking unknown MAC

```text
interface g0/1
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky
```

---

# موارد إضافية

- كورسات CCNA labs (Packet Tracer labs).
    
- Cisco Docs و IOS Command Reference.
    
- موقع GNS3 وEVE-NG إذا تبي بيئات أكثر قربًا للـ Production.
    

---

إذا تحب أنفذ لك الآن:

- أرسل لي **سيناريو محدد** (مثلاً: عدد PCs, VLANs, هل تريد NAT, OSPF أو ACL) وأنا أكتب لك ملف خطوات كامل (.pkt أو قائمة أوامر) جاهز للتطبيق في Packet Tracer.
    

أي سيناريو تريد نبدأ فيه عمليًا؟