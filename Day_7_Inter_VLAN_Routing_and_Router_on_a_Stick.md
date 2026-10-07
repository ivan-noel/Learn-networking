# Cisco Networking Learning — Day 7

> Learning Cisco Networking from 0  
> Day 7 — Inter-VLAN Routing & Router-on-a-Stick

---

## 1. Tujuan Day 7

Setelah menyelesaikan Day 7, kamu diharapkan memahami:

- Kenapa VLAN yang berbeda tidak dapat berkomunikasi langsung
- Apa itu Inter-VLAN Routing
- Fungsi router dalam menghubungkan VLAN
- Apa itu Router-on-a-Stick
- Apa itu subinterface
- Apa itu 802.1Q
- Cara membuat trunk antara switch dan router
- Cara memberikan gateway untuk setiap VLAN
- Cara menguji komunikasi antar-VLAN
- Cara melakukan troubleshooting dasar

---

## 2. Mengingat Kembali Day 6

Pada Day 6 kita membuat:

```text
VLAN 10 → STAFF
VLAN 20 → STUDENT
```

Contohnya:

```text
VLAN 10
PC0
PC1
PC2
```

dan:

```text
VLAN 20
PC3
PC4
PC5
```

PC dalam VLAN yang sama dapat berkomunikasi.

Tetapi:

```text
VLAN 10 → VLAN 20
```

tidak dapat berkomunikasi hanya dengan switch Layer 2.

Kenapa?

Karena dibutuhkan proses **routing**.

---

## 3. Apa Itu Inter-VLAN Routing?

**Inter-VLAN Routing** adalah proses yang memungkinkan perangkat pada VLAN yang berbeda untuk berkomunikasi.

Contoh:

```text
PC0
VLAN 10
   |
   v
Router
   |
   v
VLAN 20
PC3
```

Router menentukan bagaimana traffic dari satu network menuju network lainnya.

---

## 4. Kenapa Router Dibutuhkan?

Contoh:

```text
VLAN 10
192.168.10.0/24
```

dan:

```text
VLAN 20
192.168.20.0/24
```

Keduanya merupakan network yang berbeda.

Router bekerja pada Layer 3 dan dapat menghubungkan network yang berbeda.

```text
VLAN 10
192.168.10.0/24
      |
      v
   Router
      |
      v
VLAN 20
192.168.20.0/24
```

---

## 5. Konsep Default Gateway

Setiap VLAN membutuhkan gateway agar perangkat dapat mengirim traffic menuju network lain.

Contoh:

```text
VLAN 10
Gateway = 192.168.10.1
```

dan:

```text
VLAN 20
Gateway = 192.168.20.1
```

PC0:

```text
IP Address: 192.168.10.10
Gateway: 192.168.10.1
```

PC1:

```text
IP Address: 192.168.20.10
Gateway: 192.168.20.1
```

Gateway tersebut akan berada pada router.

---

## 6. Apa Itu Router-on-a-Stick?

**Router-on-a-Stick** adalah metode Inter-VLAN Routing yang menggunakan satu interface fisik router dan beberapa subinterface.

Contoh:

```text
                 Router
                  G0/0
                    |
                  Trunk
                    |
                  Switch
                /                  VLAN 10      VLAN 20
              |            |
             PC0          PC1
```

Satu interface fisik:

```text
G0/0
```

dapat memiliki:

```text
G0/0.10 → VLAN 10
G0/0.20 → VLAN 20
```

---

## 7. Apa Itu Subinterface?

Subinterface adalah interface virtual yang dibuat di dalam interface fisik router.

Contoh:

```text
interface gigabitEthernet 0/0.10
```

dan:

```text
interface gigabitEthernet 0/0.20
```

Artinya router memiliki:

```text
G0/0.10
G0/0.20
```

di bawah interface fisik:

```text
G0/0
```

---

## 8. Apa Itu 802.1Q?

Router perlu mengetahui traffic tersebut berasal dari VLAN mana.

Untuk itu kita menggunakan:

```text
encapsulation dot1Q
```

Contoh:

```text
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
```

Artinya:

```text
G0/0.10 → VLAN 10
```

Untuk VLAN 20:

```text
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
```

Artinya:

```text
G0/0.20 → VLAN 20
```

---

## 9. Topology Day 7

Gunakan topology:

```text
              Router
                |
               G0/0
                |
              Trunk
                |
             Switch
            /               VLAN 10   VLAN 20
           |          |
          PC0        PC1
```

Lebih detail:

```text
PC0 ----                   Switch0 ===== Router
         /
PC1 ----/
```

Gunakan:

```text
Switch Fa0/24 → Router G0/0
```

Fa0/24 akan menjadi trunk.

---

## 10. IP Address Plan

### VLAN 10

Network:

```text
192.168.10.0/24
```

Gateway:

```text
192.168.10.1
```

PC0:

```text
IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
```

### VLAN 20

Network:

```text
192.168.20.0/24
```

Gateway:

```text
192.168.20.1
```

PC1:

```text
IP Address: 192.168.20.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.20.1
```

---

## 11. Membuat Topology di Packet Tracer

Tambahkan:

```text
1 Router
1 Switch
2 PC
```

Contoh:

```text
Router 1941 atau 2911
Switch 2960
```

Hubungkan:

```text
PC0 → Switch Fa0/1
PC1 → Switch Fa0/2
Switch Fa0/24 → Router G0/0
```

Gunakan:

```text
Copper Straight-Through
```

---

## 12. Konfigurasi IP PC0

Klik:

```text
PC0
→ Desktop
→ IP Configuration
```

Masukkan:

```text
IP Address:
192.168.10.10

Subnet Mask:
255.255.255.0

Default Gateway:
192.168.10.1
```

---

## 13. Konfigurasi IP PC1

Klik:

```text
PC1
→ Desktop
→ IP Configuration
```

Masukkan:

```text
IP Address:
192.168.20.10

Subnet Mask:
255.255.255.0

Default Gateway:
192.168.20.1
```

---

## 14. Membuat VLAN pada Switch

Masuk ke CLI switch:

```text
enable
configure terminal
```

Buat VLAN 10:

```text
vlan 10
name STAFF
exit
```

Buat VLAN 20:

```text
vlan 20
name STUDENT
exit
```

---

## 15. Memasukkan PC0 ke VLAN 10

PC0 berada pada Fa0/1.

```text
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
exit
```

---

## 16. Memasukkan PC1 ke VLAN 20

PC1 berada pada Fa0/2.

```text
interface fastEthernet 0/2
switchport mode access
switchport access vlan 20
exit
```

---

## 17. Mengubah Port Switch Menjadi Trunk

Port yang menuju router adalah:

```text
Fa0/24
```

Gunakan:

```text
interface fastEthernet 0/24
switchport mode trunk
exit
```

Kemudian:

```text
end
```

---

## 18. Memeriksa VLAN dan Trunk

Gunakan:

```text
show vlan brief
```

Pastikan:

```text
Fa0/1 → VLAN 10
Fa0/2 → VLAN 20
```

Kemudian:

```text
show interfaces trunk
```

Pastikan:

```text
Fa0/24
```

terdeteksi sebagai trunk.

---

## 19. Konfigurasi Router

Masuk ke router:

```text
enable
configure terminal
```

Kita menggunakan:

```text
G0/0
```

Pada Router-on-a-Stick, kita tidak memberikan IP langsung pada G0/0.

Kita akan membuat subinterface.

---

## 20. Membuat Subinterface VLAN 10

```text
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
```

Artinya:

```text
G0/0.10
→ VLAN 10
→ 192.168.10.1
```

---

## 21. Membuat Subinterface VLAN 20

```text
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit
```

Artinya:

```text
G0/0.20
→ VLAN 20
→ 192.168.20.1
```

---

## 22. Mengaktifkan Interface Router

Pastikan interface fisik aktif:

```text
interface gigabitEthernet 0/0
no shutdown
exit
```

Konfigurasi lengkap router:

```text
enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
exit

interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

end
```

---

## 23. Memeriksa Interface Router

Gunakan:

```text
show ip interface brief
```

Perhatikan:

```text
G0/0
G0/0.10
G0/0.20
```

Yang penting:

```text
G0/0.10 → 192.168.10.1
G0/0.20 → 192.168.20.1
```

---

## 24. Memeriksa Routing Table

Gunakan:

```text
show ip route
```

Router seharusnya mengetahui:

```text
192.168.10.0/24
192.168.20.0/24
```

Keduanya merupakan network yang terhubung langsung melalui subinterface.

---

## 25. Pengujian Gateway VLAN 10

Dari PC0:

```text
ping 192.168.10.1
```

Jika berhasil, berarti:

```text
PC0 → Switch → Trunk → Router G0/0.10
```

sudah terhubung.

---

## 26. Pengujian Gateway VLAN 20

Dari PC1:

```text
ping 192.168.20.1
```

Jika berhasil, berarti:

```text
PC1 → Switch → Trunk → Router G0/0.20
```

sudah terhubung.

---

## 27. Pengujian Antar-VLAN

Sekarang coba dari PC0:

```text
ping 192.168.20.10
```

Jika konfigurasi benar, ping akan berhasil.

Alurnya:

```text
PC0
192.168.10.10
     |
     v
Gateway 192.168.10.1
     |
     v
Router
     |
     v
192.168.20.1
     |
     v
PC1
192.168.20.10
```

---

## 28. Bagaimana Router Melakukan Routing?

PC0 ingin menghubungi:

```text
192.168.20.10
```

PC0 mengetahui bahwa tujuan tersebut bukan bagian dari:

```text
192.168.10.0/24
```

Maka PC0 mengirim traffic ke:

```text
Default Gateway
192.168.10.1
```

Router menerima traffic melalui:

```text
G0/0.10
```

Router melihat tujuan:

```text
192.168.20.10
```

Router mengetahui network tersebut berada pada:

```text
G0/0.20
```

Kemudian router meneruskan traffic menuju VLAN 20.

---

## 29. Diagram Alur Lengkap

```text
PC0
192.168.10.10
   |
   | VLAN 10
   v
Switch
   |
   | Trunk
   v
Router
G0/0.10
192.168.10.1
   |
   | Routing
   v
G0/0.20
192.168.20.1
   |
   | VLAN 20
   v
Switch
   |
   v
PC1
192.168.20.10
```

---

## 30. Kenapa Link Switch-Router Harus Trunk?

Satu link tersebut membawa beberapa VLAN:

```text
VLAN 10
VLAN 20
```

Jika access port digunakan, satu port hanya berada pada satu VLAN.

Dengan trunk:

```text
VLAN 10 ─┐
         ├── Trunk ── Router
VLAN 20 ─┘
```

Router dapat menerima traffic dari beberapa VLAN melalui satu interface fisik.

---

## 31. Troubleshooting

Jika PC0 tidak dapat ping PC1, periksa secara berurutan.

### 31.1 Periksa IP PC

PC0:

```text
192.168.10.10
255.255.255.0
192.168.10.1
```

PC1:

```text
192.168.20.10
255.255.255.0
192.168.20.1
```

### 31.2 Periksa VLAN

```text
show vlan brief
```

Pastikan:

```text
Fa0/1 → VLAN 10
Fa0/2 → VLAN 20
```

### 31.3 Periksa Trunk

```text
show interfaces trunk
```

Pastikan:

```text
Fa0/24
```

merupakan trunk.

### 31.4 Periksa Subinterface Router

```text
show ip interface brief
```

Pastikan:

```text
G0/0.10 → 192.168.10.1
G0/0.20 → 192.168.20.1
```

### 31.5 Periksa Encapsulation

```text
show running-config
```

Pastikan ada:

```text
encapsulation dot1Q 10
encapsulation dot1Q 20
```

### 31.6 Periksa Routing Table

```text
show ip route
```

Pastikan router mengetahui:

```text
192.168.10.0/24
192.168.20.0/24
```

---

## 32. Kesalahan yang Sering Terjadi

### Kesalahan 1 — Lupa `no shutdown`

```text
interface gigabitEthernet 0/0
no shutdown
```

### Kesalahan 2 — Salah VLAN ID

Harus sesuai:

```text
VLAN 10
encapsulation dot1Q 10
```

dan:

```text
VLAN 20
encapsulation dot1Q 20
```

### Kesalahan 3 — Salah Default Gateway

PC0:

```text
192.168.10.1
```

PC1:

```text
192.168.20.1
```

### Kesalahan 4 — Port Switch bukan trunk

Gunakan:

```text
switchport mode trunk
```

pada port yang menuju router.

### Kesalahan 5 — Nama interface berbeda

Perangkat Packet Tracer dapat menggunakan interface yang berbeda.

Contoh:

```text
GigabitEthernet0/0
```

atau:

```text
FastEthernet0/0
```

Sesuaikan dengan perangkat yang digunakan.

---

## 33. Latihan 1 — Tambahkan VLAN 30

Tambahkan:

```text
VLAN 30 = GUEST
```

Network:

```text
192.168.30.0/24
```

Gateway:

```text
192.168.30.1
```

Buat:

```text
G0/0.30
```

Konfigurasi:

```text
interface gigabitEthernet 0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit
```

Kemudian masukkan satu PC ke VLAN 30.

---

## 34. Latihan 2 — Uji Semua VLAN

Jika sudah memiliki:

```text
VLAN 10
VLAN 20
VLAN 30
```

Uji:

```text
VLAN 10 → VLAN 20
VLAN 10 → VLAN 30
VLAN 20 → VLAN 30
```

Semua seharusnya dapat berkomunikasi jika konfigurasi benar.

---

## 35. Mini Quiz

### 1. Apa fungsi Inter-VLAN Routing?

A. Menghubungkan PC ke switch  
B. Memungkinkan komunikasi antar-VLAN  
C. Menghapus VLAN  
D. Mengganti subnet mask

### 2. Apa yang digunakan Router-on-a-Stick?

A. Banyak router fisik  
B. Satu interface fisik dengan beberapa subinterface  
C. Hanya switch  
D. Hub

### 3. Perintah untuk menghubungkan subinterface dengan VLAN adalah?

A.

```text
ip address
```

B.

```text
no shutdown
```

C.

```text
encapsulation dot1Q
```

D.

```text
show vlan
```

### 4. Jika VLAN 10 menggunakan gateway 192.168.10.1, gateway tersebut berada pada?

A. PC  
B. Router subinterface VLAN 10  
C. Switch access port  
D. Kabel

### 5. Link switch ke router pada Router-on-a-Stick biasanya menggunakan?

A. Access  
B. Trunk  
C. Console  
D. Loopback

Jawaban:

```text
1. B
2. B
3. C
4. B
5. B
```

---

## 36. Ringkasan Day 7

Hari ini kita mempelajari:

- Inter-VLAN Routing memungkinkan VLAN berbeda berkomunikasi.
- Router dapat digunakan untuk melakukan routing antar-VLAN.
- Default gateway digunakan oleh PC untuk mengirim traffic ke network lain.
- Router-on-a-Stick menggunakan satu interface fisik router.
- Interface tersebut dibagi menjadi beberapa subinterface.
- Contoh:
  - `G0/0.10` → VLAN 10
  - `G0/0.20` → VLAN 20
- `encapsulation dot1Q` menghubungkan subinterface dengan VLAN ID.
- Link switch-router harus menjadi trunk agar beberapa VLAN dapat melewati satu link.
- `show ip interface brief` digunakan untuk memeriksa interface.
- `show ip route` digunakan untuk melihat routing table.
- `show interfaces trunk` digunakan untuk memeriksa trunk.
- Default gateway VLAN 10 dapat berupa `192.168.10.1`.
- Default gateway VLAN 20 dapat berupa `192.168.20.1`.

---

## 37. Command Penting Day 7

### Membuat subinterface

```text
interface gigabitEthernet 0/0.10
```

### Menentukan VLAN

```text
encapsulation dot1Q 10
```

### Memberikan IP

```text
ip address 192.168.10.1 255.255.255.0
```

### Mengaktifkan interface

```text
no shutdown
```

### Membuat trunk pada switch

```text
interface fastEthernet 0/24
switchport mode trunk
```

### Melihat interface router

```text
show ip interface brief
```

### Melihat routing table

```text
show ip route
```

### Melihat trunk

```text
show interfaces trunk
```

### Melihat konfigurasi

```text
show running-config
```

---

## 38. Next Step — Day 8

Pada Day 8 kita dapat melanjutkan ke:

- Static Routing
- Network tujuan
- Next-hop
- Routing table
- Konfigurasi `ip route`
- Menghubungkan beberapa router
- Menguji komunikasi antar-network
- Troubleshooting static route

> Day 7 selesai. Pastikan kamu mencoba Router-on-a-Stick langsung di Cisco Packet Tracer sebelum lanjut ke Day 8.
