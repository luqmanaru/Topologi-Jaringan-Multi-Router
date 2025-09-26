# Topologi-Jaringan-Multi-Router

Implementasi infrastruktur jaringan hybrid dengan konektivitas end-to-end

![Cisco](https://img.shields.io/badge/Cisco-Infrastructure-blue?style=for-the-badge&logo=cisco&logoColor=white)
![Network](https://img.shields.io/badge/Network-Hybrid-green?style=for-the-badge)
![OSPF](https://img.shields.io/badge/OSPF-Routing-orange?style=for-the-badge)

---

## 📝 Deskripsi
Proyek ini mengimplementasikan **topologi jaringan multi-router** dengan infrastruktur hybrid wired dan wireless. Implementasi mencakup:
- Konfigurasi 4 router dengan routing dinamis OSPF
- Implementasi access point untuk jaringan nirkabel
- Verifikasi konektivitas end-to-end antar segmen jaringan
- Analisis tabel routing dan neighbor adjacency

---

## 🏗️ Topologi Jaringan
| Perangkat | Interface | IP Address | Subnet | Koneksi |
|-----------|-----------|------------|--------|---------|
| **R1** | Gi0/0 | 192.168.1.1 | /30 | Ke R2 |
| | Gi0/1 | 192.168.2.1 | /30 | Ke R3 |
| **R2** | Gi0/0 | 192.168.1.2 | /30 | Ke R1 |
| | Gi0/1 | 192.168.3.1 | /30 | Ke R4 |
| **R3** | Gi0/0 | 192.168.3.2 | /30 | Ke R2 |
| | Gi0/1 | 192.168.4.1 | /30 | Ke R4 |
| **R4** | Gi0/0 | 192.168.4.2 | /30 | Ke R3 |
| | Gi0/1 | 192.168.5.1 | /24 | Ke Access Point |
| **Access Point** | Gi0/0 | 192.168.5.10 | /24 | Ke R4 |
| | Dot11Radio0 | - | - | Ke Laptop |
| **Laptop1** | Wireless | DHCP | /24 | Ke Access Point |
| **Laptop2** | Wireless | DHCP | /24 | Ke Access Point |

---

## 🖼️ Gambar Topologi
```
    [R1]--------[R2]
     |            |
     |            |
    [R3]--------[R4]
                  |
                  |
             [Access Point]
                  |
            [Laptop1, Laptop2]
```
<img width="921" height="324" alt="image" src="https://github.com/user-attachments/assets/2a378aa5-89f6-422c-98d1-61b474a38ada" />

---

## 📊 Output Verifikasi
### Tabel Routing R1
```
R1# show ip route

Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     192.168.1.0/30 is subnetted, 1 subnets
C       192.168.1.0 is directly connected, GigabitEthernet0/0
     192.168.2.0/30 is subnetted, 1 subnets
C       192.168.2.0 is directly connected, GigabitEthernet0/1
     192.168.3.0/30 is subnetted, 1 subnets
O       192.168.3.0 [110/2] via 192.168.1.2, 00:00:05, GigabitEthernet0/0
     192.168.4.0/30 is subnetted, 1 subnets
O       192.168.4.0 [110/3] via 192.168.1.2, 00:00:05, GigabitEthernet0/0
     192.168.5.0/24 is subnetted, 1 subnets
O       192.168.5.0 [110/4] via 192.168.1.2, 00:00:05, GigabitEthernet0/0
```

### OSPF Neighbor R4
```
R4# show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
192.168.3.2       1   FULL/BDR        00:00:39    192.168.4.1     GigabitEthernet0/0
```

### Konektivitas Access Point
```
AP-Office# ping 192.168.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/8 ms
```

### Konektivitas Laptop ke Router
```
Laptop1> ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:
Reply from 192.168.1.1: bytes=32 time=15ms TTL=254
Reply from 192.168.1.1: bytes=32 time=10ms TTL=254
Reply from 192.168.1.1: bytes=32 time=12ms TTL=254
Reply from 192.168.1.1: bytes=32 time=11ms TTL=254

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 10ms, Maximum = 15ms, Average = 12ms
```

---

**luqmanaru**  
