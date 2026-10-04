# Cisco Networking Learning — Day 5

> Learning Cisco Networking from 0  
> Day 5 — Building a Multi-Network Topology with Cisco Packet Tracer

## 1. Tujuan Day 5

Pada Day 4 kita sudah mempelajari subnetting. Sekarang kita menggabungkan:

- IP Address
- Subnet Mask
- Network Address
- Default Gateway
- Switch
- Router
- Beberapa network
- Konfigurasi interface router
- `ping`
- Troubleshooting dasar

**Target:** membuat dua network berbeda dan membuat keduanya saling berkomunikasi melalui router.

---

## 2. Mengingat Kembali Day 4

```text
192.168.10.0/26
Network   : 192.168.10.0
Host      : 192.168.10.1 - 192.168.10.62
Broadcast : 192.168.10.63
```

```text
192.168.10.64/26
Network   : 192.168.10.64
Host      : 192.168.10.65 - 192.168.10.126
Broadcast : 192.168.10.127
```

Kedua subnet tersebut berbeda network, sehingga membutuhkan router untuk saling berkomunikasi.

---

## 3. Apa Itu Default Gateway?

Default gateway adalah alamat IP perangkat yang menjadi jalan keluar dari sebuah network.

Biasanya gateway adalah interface router.

Contoh:

```text
PC0
IP      : 192.168.10.10
Gateway : 192.168.10.1
```

Jika PC0 ingin berkomunikasi dengan network lain, PC0 mengirim paket ke gateway tersebut.

---

## 4. Switch vs Router

### Switch

Menghubungkan perangkat dalam network yang sama.

```text
PC0 ---- Switch ---- PC1
```

### Router

Menghubungkan network yang berbeda.

```text
Network 1 ---- Router ---- Network 2
```

Secara sederhana:

```text
Switch → menghubungkan perangkat
Router → menghubungkan network
```

---

## 5. Topology Day 5

Buat:

```text
PC0 ---- Switch0 ---- Router ---- Switch1 ---- PC1
```

Router:

```text
G0/0 → Network 1
G0/1 → Network 2
```

---

## 6. Menentukan IP Address

### Network 1

```text
Network : 192.168.10.0/26
Mask    : 255.255.255.192
Router  : 192.168.10.1
PC0     : 192.168.10.10
Gateway : 192.168.10.1
```

### Network 2

```text
Network : 192.168.10.64/26
Mask    : 255.255.255.192
Router  : 192.168.10.65
PC1     : 192.168.10.70
Gateway : 192.168.10.65
```

---

## 7. Tabel IP Address

| Device | Interface | IP Address | Subnet Mask | Gateway |
|---|---|---|---|---|
| PC0 | FastEthernet | 192.168.10.10 | 255.255.255.192 | 192.168.10.1 |
| Router | G0/0 | 192.168.10.1 | 255.255.255.192 | - |
| Router | G0/1 | 192.168.10.65 | 255.255.255.192 | - |
| PC1 | FastEthernet | 192.168.10.70 | 255.255.255.192 | 192.168.10.65 |

---

## 8. Membuat Topology di Packet Tracer

Tambahkan:

```text
2 PC
2 Switch
1 Router
```

Susun:

```text
PC0 ---- Switch0 ---- Router ---- Switch1 ---- PC1
```

Hubungkan dengan kabel Ethernet.

---

## 9. Konfigurasi PC0

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
255.255.255.192

Default Gateway:
192.168.10.1
```

---

## 10. Konfigurasi PC1

Klik:

```text
PC1
→ Desktop
→ IP Configuration
```

Masukkan:

```text
IP Address:
192.168.10.70

Subnet Mask:
255.255.255.192

Default Gateway:
192.168.10.65
```

---

## 11. Konfigurasi Router

Buka router → **CLI**.

Jika muncul initial configuration dialog:

```text
no
```

Kemudian:

```text
enable
configure terminal
```

### G0/0

```text
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.192
no shutdown
exit
```

### G0/1

```text
interface gigabitEthernet 0/1
ip address 192.168.10.65 255.255.255.192
no shutdown
exit
```

---

## 12. Memeriksa Interface

Gunakan:

```text
show ip interface brief
```

Yang diharapkan:

```text
GigabitEthernet0/0     192.168.10.1    up    up
GigabitEthernet0/1     192.168.10.65   up    up
```

`up/up` berarti interface aktif dan protokolnya aktif.

---

## 13. Memeriksa Routing Table

Gunakan:

```text
show ip route
```

Karena kedua network langsung terhubung, router seharusnya mengetahui:

```text
C    192.168.10.0/26
C    192.168.10.64/26
```

`C` berarti **Connected**.

Artinya network tersebut langsung terhubung ke router.

---

## 14. Pengujian Bertahap

Jangan langsung menguji PC0 ke PC1. Uji dari yang paling sederhana.

### PC0 → Gateway

Dari PC0:

```text
ping 192.168.10.1
```

### PC1 → Gateway

Dari PC1:

```text
ping 192.168.10.65
```

### PC0 → PC1

Dari PC0:

```text
ping 192.168.10.70
```

Jika berhasil, kedua network sudah dapat berkomunikasi.

---

## 15. Bagaimana Router Menentukan Jalur?

Routing table sederhana:

```text
Network                  Interface

192.168.10.0/26          G0/0
192.168.10.64/26         G0/1
```

PC0 mengirim data ke:

```text
192.168.10.70
```

Router melihat bahwa IP tersebut berada di:

```text
192.168.10.64/26
```

Maka router mengirim paket melalui:

```text
G0/1
```

---

## 16. Alur Komunikasi

Ketika PC0 melakukan:

```text
ping 192.168.10.70
```

alur sederhananya:

```text
PC0
 ↓
Switch0
 ↓
Router G0/0
 ↓
Router G0/1
 ↓
Switch1
 ↓
PC1
```

Gateway PC0:

```text
192.168.10.1
```

Gateway PC1:

```text
192.168.10.65
```

---

## 17. Mengapa Router Dibutuhkan?

PC0:

```text
192.168.10.10/26
```

PC1:

```text
192.168.10.70/26
```

Dengan `/26`, subnetnya:

```text
192.168.10.0 - 63
192.168.10.64 - 127
```

PC0 berada di:

```text
192.168.10.0/26
```

PC1 berada di:

```text
192.168.10.64/26
```

Karena berbeda network, router dibutuhkan untuk menghubungkan keduanya.

---

## 18. Troubleshooting Dasar

Jika `ping` gagal, periksa satu per satu.

### 1. Periksa IP PC

PC0:

```text
192.168.10.10
```

PC1:

```text
192.168.10.70
```

### 2. Periksa Subnet Mask

Keduanya:

```text
255.255.255.192
```

### 3. Periksa Gateway

PC0:

```text
192.168.10.1
```

PC1:

```text
192.168.10.65
```

### 4. Periksa Interface Router

```text
show ip interface brief
```

Pastikan:

```text
G0/0 → up/up
G0/1 → up/up
```

### 5. Periksa Routing Table

```text
show ip route
```

Pastikan router mengetahui:

```text
192.168.10.0/26
192.168.10.64/26
```

### 6. Periksa Kabel

Pastikan koneksi:

```text
PC → Switch
Switch → Router
Router → Switch
Switch → PC
```

---

## 19. Kesalahan yang Sering Terjadi

### Lupa `no shutdown`

```text
interface gigabitEthernet 0/0
no shutdown
```

### Gateway Salah

PC0 harus menggunakan:

```text
192.168.10.1
```

bukan:

```text
192.168.10.65
```

### IP Berada di Network yang Salah

PC0:

```text
192.168.10.10/26
```

berada di network:

```text
192.168.10.0/26
```

Gateway-nya harus berada pada network yang sama.

### Subnet Mask Salah

Pastikan perangkat menggunakan mask yang sesuai:

```text
255.255.255.192
```

---

## 20. Same Network vs Different Network

Contoh:

```text
PC0 = 192.168.10.10/26
PC1 = 192.168.10.20/26
```

Keduanya berada di:

```text
192.168.10.0/26
```

Jadi berada pada network yang sama.

Contoh:

```text
PC0 = 192.168.10.10/26
PC1 = 192.168.10.70/26
```

PC0:

```text
192.168.10.0/26
```

PC1:

```text
192.168.10.64/26
```

Berarti berbeda network dan membutuhkan router.

---

## 21. Latihan 1 — Identifikasi Network

Tentukan apakah pasangan berikut berada pada network yang sama.

### A

```text
PC0 = 192.168.1.10/24
PC1 = 192.168.1.50/24
```

### B

```text
PC0 = 192.168.1.10/26
PC1 = 192.168.1.50/26
```

### C

```text
PC0 = 192.168.1.10/26
PC1 = 192.168.1.70/26
```

### D

```text
PC0 = 192.168.1.100/25
PC1 = 192.168.1.200/25
```

---

## 22. Latihan 2 — Buat Topology Sendiri

Buat:

```text
PC0 ---- Switch0 ---- Router ---- Switch1 ---- PC1
```

Gunakan:

```text
Network 1 = 192.168.20.0/27
Network 2 = 192.168.20.32/27
```

Tentukan sendiri:

- IP router G0/0
- IP router G0/1
- IP PC0
- IP PC1
- Gateway PC0
- Gateway PC1

Kemudian lakukan `ping` dari PC0 ke PC1.

---

## 23. Mini Quiz

1. Apa fungsi default gateway?
2. Apa perbedaan utama switch dan router?
3. Mengapa PC dari network berbeda membutuhkan router?
4. Apa fungsi `no shutdown`?
5. Apa fungsi `show ip interface brief`?
6. Apa fungsi `show ip route`?
7. Jika PC0 memiliki `192.168.10.10/26`, gateway mana yang benar?
   - A. `192.168.10.1`
   - B. `192.168.10.65`
   - C. `192.168.20.1`
8. Apakah `192.168.10.10/26` dan `192.168.10.70/26` berada dalam network yang sama?
9. Apa yang harus dilakukan jika interface router menunjukkan `administratively down`?
10. Command apa yang digunakan untuk menguji koneksi?

---

## 24. Ringkasan Day 5

Hari ini kita mempelajari:

- Default gateway
- Perbedaan switch dan router
- Topology dengan dua network
- Menentukan IP perangkat
- Menentukan gateway
- Konfigurasi interface router
- `no shutdown`
- `show ip interface brief`
- `show ip route`
- Pengujian menggunakan `ping`
- Routing table sederhana
- Troubleshooting dasar
- Same network vs different network

Konsep utama:

```text
Same Network
     ↓
Switch
     ↓
Different Network
     ↓
Router
     ↓
Default Gateway
```

Alur konfigurasi:

```text
1. Tentukan network
        ↓
2. Tentukan IP address
        ↓
3. Tentukan subnet mask
        ↓
4. Tentukan default gateway
        ↓
5. Konfigurasi router
        ↓
6. Aktifkan interface
        ↓
7. Test dengan ping
        ↓
8. Troubleshooting jika gagal
```

---

## 25. Next Step — Day 6

Day 6 akan mulai membahas **VLAN**:

- Apa itu VLAN
- Mengapa VLAN digunakan
- Membuat VLAN di switch
- Access port
- Mengelompokkan PC berdasarkan VLAN
- Konfigurasi VLAN melalui CLI
- Pengujian komunikasi
- Pengantar trunk
- Hubungan VLAN dengan network segmentation

> **Target Day 6:** memahami bahwa satu switch dapat dibagi secara logis menjadi beberapa kelompok jaringan menggunakan VLAN.
