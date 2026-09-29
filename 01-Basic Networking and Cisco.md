# Cisco Networking Learning — Day 1

> Learning Cisco Networking from 0  
> Day 1 — Basic Networking & Cisco Introduction

---

## 1. Apa itu Cisco?

Cisco adalah perusahaan teknologi yang banyak dikenal karena produk dan solusi jaringan komputer.

Dalam pembelajaran networking, Cisco sering digunakan sebagai dasar untuk memahami:

- Network device
- Switching
- Routing
- IP Address
- VLAN
- Network security
- Network configuration
- Network troubleshooting

Contoh perangkat Cisco:

- Router
- Switch
- Access Point
- Firewall
- Network Controller

---

## 2. Apa itu Network?

Network atau jaringan komputer adalah sekumpulan perangkat yang saling terhubung sehingga dapat berkomunikasi dan bertukar data.

Contoh sederhana:

```text
PC 1 -------- Switch -------- PC 2
```

PC 1 dan PC 2 dapat berkomunikasi melalui switch.

Contoh jaringan yang lebih besar:

```text
PC
 |
Switch
 |
Router
 |
Internet
```

---

## 3. Perangkat Jaringan Dasar

### A. End Device

End device adalah perangkat yang digunakan oleh pengguna atau perangkat yang menjadi tujuan/source komunikasi.

Contohnya:

- PC
- Laptop
- Smartphone
- Server
- Printer

---

### B. Switch

Switch digunakan untuk menghubungkan beberapa perangkat dalam satu jaringan lokal (LAN).

Contoh:

```text
PC 1 ----\
PC 2 ----- Switch
PC 3 ----/
```

Switch bekerja terutama pada **OSI Layer 2 (Data Link Layer)**.

Switch menggunakan **MAC Address** untuk membantu meneruskan frame ke perangkat yang tepat.

---

### C. Router

Router digunakan untuk menghubungkan jaringan yang berbeda.

Contoh:

```text
Network A
    |
  Router
    |
Network B
```

Router bekerja terutama pada **OSI Layer 3 (Network Layer)**.

Router menggunakan **IP Address** untuk menentukan ke mana paket harus dikirim.

---

## 4. Perbedaan Switch dan Router

| Switch | Router |
|---|---|
| Menghubungkan perangkat dalam LAN | Menghubungkan jaringan yang berbeda |
| Layer 2 | Layer 3 |
| Menggunakan MAC Address | Menggunakan IP Address |
| Meneruskan frame | Meneruskan packet |
| Contoh: PC ke PC dalam LAN | Contoh: LAN ke Internet |

Contoh:

```text
PC 1 ----\
PC 2 ----- Switch ---- Router ---- Internet
PC 3 ----/
```

Switch menghubungkan perangkat di dalam jaringan lokal.

Router menghubungkan jaringan lokal dengan jaringan lain.

---

## 5. Apa itu LAN?

LAN atau **Local Area Network** adalah jaringan yang mencakup area yang relatif kecil.

Contohnya:

- Rumah
- Laboratorium komputer
- Sekolah
- Kampus
- Kantor

Contoh:

```text
        Switch
       /   |   \
     PC1  PC2  PC3
```

Ketiga PC tersebut berada dalam satu LAN.

---

## 6. Apa itu WAN?

WAN atau **Wide Area Network** adalah jaringan yang mencakup area yang lebih luas.

Contohnya:

- Antar kota
- Antar wilayah
- Antar negara

Internet merupakan contoh jaringan yang sangat besar dan menggunakan konsep WAN.

Contoh sederhana:

```text
LAN A                  LAN B
  |                      |
Router A ------------- Router B
        WAN Connection
```

---

## 7. Apa itu IP Address?

IP Address adalah alamat yang digunakan untuk mengidentifikasi perangkat dalam jaringan IP.

Contoh IPv4:

```text
192.168.1.10
```

Setiap perangkat dalam jaringan membutuhkan alamat IP yang sesuai agar dapat berkomunikasi menggunakan IP.

Contoh:

```text
PC 1
IP: 192.168.1.10

PC 2
IP: 192.168.1.11
```

---

## 8. Apa itu MAC Address?

MAC Address adalah alamat hardware yang digunakan pada jaringan Ethernet.

Contoh format:

```text
00:1A:2B:3C:4D:5E
```

MAC Address berhubungan dengan **Network Interface Card (NIC)**.

Perbedaan sederhana:

```text
MAC Address
    ↓
Identitas hardware/network interface

IP Address
    ↓
Alamat perangkat dalam jaringan IP
```

---

## 9. Apa itu Packet?

Packet adalah unit data yang dikirim melalui jaringan pada Network Layer.

Contoh sederhana:

```text
PC 1
  |
  | Packet
  ↓
Router
  |
  ↓
PC 2
```

Packet dapat berisi informasi seperti:

- Source IP Address
- Destination IP Address
- Data

---

## 10. Apa itu Frame?

Frame merupakan unit data pada **Data Link Layer**.

Pada jaringan Ethernet, frame menggunakan MAC Address untuk komunikasi pada jaringan lokal.

Secara sederhana:

```text
Application
    ↓
Transport
    ↓
Packet
    ↓
Frame
    ↓
Bits
```

---

## 11. OSI Model

OSI Model digunakan untuk membantu memahami bagaimana komunikasi jaringan bekerja.

Terdapat 7 layer:

| Layer | Name |
|---|---|
| 7 | Application |
| 6 | Presentation |
| 5 | Session |
| 4 | Transport |
| 3 | Network |
| 2 | Data Link |
| 1 | Physical |

Yang perlu mulai diingat:

```text
Layer 3 → Network → IP Address → Router

Layer 2 → Data Link → MAC Address → Switch

Layer 1 → Physical → Cable / Signal
```

Tidak perlu menghafalkan semuanya sekaligus.

Yang penting untuk Day 1 adalah mulai memahami hubungan:

```text
Router → Layer 3 → IP Address

Switch → Layer 2 → MAC Address
```

---

## 12. Cisco Packet Tracer

Cisco Packet Tracer adalah simulator jaringan yang dapat digunakan untuk belajar networking tanpa harus memiliki perangkat Cisco secara fisik.

Kita dapat membuat topology seperti:

```text
PC 1 ---- Switch ---- PC 2
```

Kemudian melakukan konfigurasi dan pengujian koneksi.

Packet Tracer sangat berguna untuk pemula karena kita dapat:

- Membuat topology
- Menghubungkan perangkat
- Memberikan IP Address
- Mengkonfigurasi switch
- Mengkonfigurasi router
- Menggunakan CLI
- Melakukan troubleshooting
- Menguji koneksi dengan `ping`

---

## 13. Contoh Topology Pertama

Topology sederhana yang akan menjadi dasar latihan:

```text
PC 1 --------\
              \
               Switch
              /
PC 2 --------/
```

Kemudian kita dapat memberikan IP:

```text
PC 1
IP Address: 192.168.1.10

PC 2
IP Address: 192.168.1.11
```

Jika konfigurasi benar, kedua PC seharusnya dapat berkomunikasi.

Kita dapat mengujinya menggunakan:

```bash
ping 192.168.1.11
```

dari PC 1.

---

## 14. Apa itu CLI?

CLI adalah singkatan dari **Command-Line Interface**.

Cisco menyediakan CLI untuk melakukan konfigurasi perangkat.

Contoh tampilan:

```text
Router>
```

atau:

```text
Switch>
```

Cisco menggunakan command tertentu untuk melakukan konfigurasi.

Contoh:

```text
enable
```

Command tersebut digunakan untuk berpindah ke privileged EXEC mode.

Contoh:

```text
Router> enable
Router#
```

Tanda prompt berubah dari:

```text
>
```

menjadi:

```text
#
```

---

## 15. Mode Dasar Cisco CLI

Untuk tahap awal, kenali beberapa mode berikut.

### User EXEC Mode

```text
Router>
```

Mode dasar ketika pertama kali masuk ke CLI.

### Privileged EXEC Mode

```text
Router#
```

Masuk menggunakan:

```text
enable
```

Mode ini memberikan akses ke command yang lebih banyak.

### Global Configuration Mode

```text
Router(config)#
```

Masuk menggunakan:

```text
configure terminal
```

atau:

```text
conf t
```

Mode ini digunakan untuk melakukan konfigurasi perangkat.

---

## 16. Command Dasar yang Dipelajari Hari Ini

### Melihat informasi dasar perangkat

```text
show version
```

### Masuk ke privileged mode

```text
enable
```

### Masuk ke configuration mode

```text
configure terminal
```

atau:

```text
conf t
```

### Kembali satu level

```text
exit
```

### Kembali ke privileged mode

```text
end
```

atau:

```text
Ctrl + Z
```

### Melihat konfigurasi

```text
show running-config
```

---

## 17. Konsep Penting Day 1

Hari ini kita mulai mengenal hubungan:

```text
End Device
     ↓
   Switch
     ↓
  Router
     ↓
   Network
```

Dan konsep:

```text
Switch
  ↓
Layer 2
  ↓
MAC Address
  ↓
Frame
```

Sedangkan:

```text
Router
  ↓
Layer 3
  ↓
IP Address
  ↓
Packet
```

---

## 18. Hal yang Harus Diingat

### 1. Switch ≠ Router

Switch menghubungkan perangkat dalam jaringan lokal.

Router menghubungkan jaringan yang berbeda.

### 2. MAC Address ≠ IP Address

MAC Address berhubungan dengan network interface/hardware.

IP Address digunakan sebagai alamat pada jaringan IP.

### 3. Frame ≠ Packet

Frame berada pada Data Link Layer.

Packet berada pada Network Layer.

### 4. CLI adalah cara utama konfigurasi Cisco

Cisco dapat dikonfigurasi melalui command pada CLI.

---

## 19. Mini Quiz

Sebelum lanjut ke Day 2, coba jawab tanpa melihat catatan.

### Question 1

Apa fungsi utama switch?

### Question 2

Apa fungsi utama router?

### Question 3

Switch bekerja pada layer berapa?

### Question 4

Router bekerja pada layer berapa?

### Question 5

Apa perbedaan MAC Address dan IP Address?

### Question 6

Apa kepanjangan dari LAN?

### Question 7

Apa kepanjangan dari WAN?

### Question 8

Command apa yang digunakan untuk masuk ke privileged EXEC mode?

### Question 9

Apa fungsi `ping`?

### Question 10

Apa fungsi Cisco Packet Tracer?

---

## 20. Day 1 Summary

Pada Day 1, kita mempelajari dasar-dasar networking dan pengenalan Cisco.

Materi utama:

- Cisco
- Computer Network
- End Device
- Switch
- Router
- LAN
- WAN
- IP Address
- MAC Address
- Packet
- Frame
- OSI Model
- Cisco Packet Tracer
- CLI
- Cisco CLI modes
- Basic Cisco commands

Konsep utama yang harus mulai dipahami:

```text
Switch → Layer 2 → MAC Address → Frame

Router → Layer 3 → IP Address → Packet
```

---

## Next Step — Day 2

Pada Day 2 kita akan mulai praktik langsung menggunakan Cisco Packet Tracer.

Target:

1. Membuat topology pertama
2. Menambahkan PC dan Switch
3. Menghubungkan perangkat dengan kabel
4. Memberikan IP Address
5. Melakukan `ping`
6. Mengenal Cisco CLI lebih jauh
7. Memahami bagaimana data berpindah dari PC → Switch → PC

> **Goal:** Jangan hanya menghafal command. Pahami apa yang terjadi di jaringan ketika setiap command digunakan.
