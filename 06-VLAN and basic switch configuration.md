# Cisco Networking Learning — Day 6

> Learning Cisco Networking from 0  
> Day 6 — VLAN & Basic Switch Configuration

## 1. Tujuan Day 6

Setelah menyelesaikan Day 6, kamu diharapkan memahami:

- Apa itu VLAN
- Kenapa VLAN digunakan
- Apa itu VLAN ID
- Perbedaan access port dan trunk port
- Cara membuat VLAN pada switch Cisco
- Cara memasukkan port ke VLAN tertentu
- Cara memeriksa VLAN dengan CLI
- Cara menguji komunikasi antar-PC
- Kenapa PC dari VLAN berbeda tidak bisa berkomunikasi secara langsung
- Gambaran awal tentang trunk

---

## 2. Mengingat Kembali Day 5

Pada Day 5 kita membuat beberapa network menggunakan router.

Contohnya:

```text
PC0 ---- Switch0 ---- Router ---- Switch1 ---- PC1
```

Router digunakan untuk menghubungkan network yang berbeda.

Sekarang kita akan fokus lebih dalam pada **switch**.

Sebelum menggunakan router untuk menghubungkan network, kita perlu memahami bagaimana switch dapat membagi sebuah jaringan menjadi beberapa kelompok menggunakan VLAN.

---

## 3. Apa Itu VLAN?

**VLAN** adalah singkatan dari:

> Virtual Local Area Network

VLAN memungkinkan kita membagi satu switch secara logis menjadi beberapa kelompok.

Contoh:

```text
VLAN 10
PC0
PC1
PC2

VLAN 20
PC3
PC4
PC5
```

Secara sederhana:

> VLAN digunakan untuk memisahkan perangkat secara logis meskipun perangkat tersebut menggunakan switch fisik yang sama.

---

## 4. Kenapa VLAN Digunakan?

### 4.1 Memisahkan jaringan

Contoh:

```text
VLAN 10 → Guru
VLAN 20 → Siswa
VLAN 30 → Administrasi
```

Walaupun menggunakan switch yang sama, setiap kelompok dapat dipisahkan.

### 4.2 Mengurangi Broadcast

Broadcast dari VLAN 10 tidak otomatis menyebar ke VLAN 20.

### 4.3 Mempermudah Pengelolaan

Administrator dapat membuat kelompok berdasarkan fungsi:

```text
VLAN 10 = Staff
VLAN 20 = Student
VLAN 30 = Guest
```

---

## 5. VLAN ID

Setiap VLAN memiliki ID.

Contoh:

```text
VLAN 10
VLAN 20
VLAN 30
```

Contoh penggunaan:

```text
VLAN 10 = STAFF
VLAN 20 = STUDENT
```

VLAN ID digunakan untuk mengidentifikasi VLAN.

---

## 6. Access Port

**Access port** biasanya digunakan untuk menghubungkan perangkat akhir seperti:

- PC
- Printer
- Server
- IP Phone

Contoh:

```text
PC0
 |
Fa0/1
 |
Switch
```

Jika Fa0/1 dimasukkan ke VLAN 10:

```text
Fa0/1 → VLAN 10
```

---

## 7. Trunk Port

**Trunk port** digunakan untuk membawa beberapa VLAN melalui satu koneksi.

Contoh:

```text
Switch0 ===== Switch1
          Trunk
```

Satu link trunk dapat membawa:

```text
VLAN 10
VLAN 20
VLAN 30
```

Secara sederhana:

```text
Access Port
PC → Switch

Trunk Port
Switch → Switch
```

Catatan: konfigurasi trunk lebih lanjut akan dipelajari pada materi berikutnya.

---

## 8. Topology Day 6

Kita akan membuat topology sederhana:

```text
PC0 --------             PC1 ---------- Switch0
             /
PC2 --------/

PC3 --------             PC4 ---------- Switch0
             /
PC5 --------/
```

Pembagian:

```text
VLAN 10
PC0
PC1
PC2

VLAN 20
PC3
PC4
PC5
```

---

## 9. IP Address

### VLAN 10

Network:

```text
192.168.10.0/24
```

| Device | IP Address | VLAN |
|---|---|---|
| PC0 | 192.168.10.10 | VLAN 10 |
| PC1 | 192.168.10.11 | VLAN 10 |
| PC2 | 192.168.10.12 | VLAN 10 |

Subnet Mask:

```text
255.255.255.0
```

### VLAN 20

Network:

```text
192.168.20.0/24
```

| Device | IP Address | VLAN |
|---|---|---|
| PC3 | 192.168.20.10 | VLAN 20 |
| PC4 | 192.168.20.11 | VLAN 20 |
| PC5 | 192.168.20.12 | VLAN 20 |

Subnet Mask:

```text
255.255.255.0
```

Untuk latihan ini kita belum menggunakan router, jadi:

```text
Default Gateway = dikosongkan
```

---

## 10. Membuat Topology di Packet Tracer

Tambahkan:

```text
6 × PC
1 × Switch
```

Contoh switch:

```text
2960
```

Hubungkan:

```text
PC0 → Switch Fa0/1
PC1 → Switch Fa0/2
PC2 → Switch Fa0/3

PC3 → Switch Fa0/4
PC4 → Switch Fa0/5
PC5 → Switch Fa0/6
```

Gunakan:

```text
Copper Straight-Through
```

---

## 11. Konfigurasi IP PC

Klik:

```text
PC0
→ Desktop
→ IP Configuration
```

Masukkan:

```text
IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
```

PC1:

```text
192.168.10.11
255.255.255.0
```

PC2:

```text
192.168.10.12
255.255.255.0
```

PC3:

```text
192.168.20.10
255.255.255.0
```

PC4:

```text
192.168.20.11
255.255.255.0
```

PC5:

```text
192.168.20.12
255.255.255.0
```

---

## 12. Masuk ke CLI Switch

Klik switch → **CLI**.

Ketik:

```text
enable
configure terminal
```

Prompt biasanya menjadi:

```text
Switch(config)#
```

---

## 13. Membuat VLAN 10

```text
vlan 10
name STAFF
exit
```

---

## 14. Membuat VLAN 20

```text
vlan 20
name STUDENT
exit
```

Konfigurasi:

```text
enable
configure terminal

vlan 10
name STAFF
exit

vlan 20
name STUDENT
exit
```

---

## 15. Memeriksa VLAN

Gunakan:

```text
show vlan brief
```

Kamu seharusnya melihat:

```text
10    STAFF
20    STUDENT
```

Namun port masih perlu dimasukkan ke VLAN yang sesuai.

---

## 16. Memasukkan Port ke VLAN 10

PC0–PC2 menggunakan Fa0/1–Fa0/3.

Gunakan:

```text
interface range fastEthernet 0/1 - 3
switchport mode access
switchport access vlan 10
exit
```

---

## 17. Memasukkan Port ke VLAN 20

PC3–PC5 menggunakan Fa0/4–Fa0/6.

Gunakan:

```text
interface range fastEthernet 0/4 - 6
switchport mode access
switchport access vlan 20
exit
```

---

## 18. Konfigurasi Lengkap Switch

```text
enable
configure terminal

vlan 10
name STAFF
exit

vlan 20
name STUDENT
exit

interface range fastEthernet 0/1 - 3
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/4 - 6
switchport mode access
switchport access vlan 20
exit

end
```

---

## 19. Periksa VLAN Lagi

Gunakan:

```text
show vlan brief
```

Hasilnya kira-kira:

```text
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active
10   STAFF                            active    Fa0/1, Fa0/2, Fa0/3
20   STUDENT                          active    Fa0/4, Fa0/5, Fa0/6
```

Artinya:

```text
Fa0/1 → VLAN 10
Fa0/2 → VLAN 10
Fa0/3 → VLAN 10

Fa0/4 → VLAN 20
Fa0/5 → VLAN 20
Fa0/6 → VLAN 20
```

---

## 20. Pengujian VLAN 10

Dari PC0:

```text
ping 192.168.10.11
```

Kemudian:

```text
ping 192.168.10.12
```

Seharusnya berhasil karena PC0, PC1, dan PC2 berada dalam VLAN 10.

---

## 21. Pengujian VLAN 20

Dari PC3:

```text
ping 192.168.20.11
```

Kemudian:

```text
ping 192.168.20.12
```

Seharusnya berhasil karena ketiganya berada dalam VLAN 20.

---

## 22. Pengujian Antar-VLAN

Dari PC0 coba:

```text
ping 192.168.20.10
```

PC0 berada di:

```text
VLAN 10
```

PC3 berada di:

```text
VLAN 20
```

Pada topology ini, ping seharusnya gagal.

Mengapa?

Karena VLAN 10 dan VLAN 20 adalah broadcast domain yang berbeda dan kita belum memiliki perangkat Layer 3 untuk melakukan routing.

---

## 23. Kenapa VLAN Berbeda Tidak Bisa Berkomunikasi?

Switch Layer 2 dapat memindahkan frame di dalam VLAN.

Tetapi untuk berpindah:

```text
VLAN 10
   ↓
VLAN 20
```

dibutuhkan routing Layer 3.

Contoh:

```text
VLAN 10
   |
   v
Router / Layer 3 Switch
   |
   v
VLAN 20
```

Konsep ini disebut:

> Inter-VLAN Routing

---

## 24. Analogi Sederhana VLAN

Bayangkan satu gedung memiliki satu switch besar.

Tanpa VLAN:

```text
Satu gedung
└── Semua orang berada dalam satu kelompok
```

Dengan VLAN:

```text
Gedung
├── VLAN 10 → Staff
├── VLAN 20 → Student
└── VLAN 30 → Guest
```

Perangkat fisiknya sama, tetapi kelompoknya dipisahkan secara logis.

---

## 25. Access Port vs Trunk Port

### Access

```text
PC
 |
 | Access
 |
Switch
```

Biasanya digunakan untuk perangkat akhir.

Contoh:

```text
Fa0/1 → VLAN 10
```

### Trunk

```text
Switch0
   ||
   || Trunk
   ||
Switch1
```

Trunk digunakan untuk membawa traffic beberapa VLAN.

Contoh:

```text
VLAN 10
VLAN 20
VLAN 30
```

melalui satu link.

---

## 26. Pengantar Konfigurasi Trunk

Misalnya:

```text
Switch0 ===== Switch1
```

Port penghubung dapat diatur sebagai trunk:

```text
interface fastEthernet 0/24
switchport mode trunk
```

Periksa dengan:

```text
show interfaces trunk
```

Catatan: ini baru pengantar. Praktik trunk antar-switch akan dibahas lebih lanjut.

---

## 27. Troubleshooting VLAN

Jika ping gagal, periksa satu per satu.

### 27.1 Periksa IP Address

Contoh:

```text
PC0 = 192.168.10.10
PC1 = 192.168.10.11
```

### 27.2 Periksa Subnet Mask

```text
255.255.255.0
```

### 27.3 Periksa VLAN

```text
show vlan brief
```

Pastikan port berada di VLAN yang benar.

### 27.4 Periksa Kabel

Pastikan link PC dan switch aktif.

### 27.5 Periksa Konfigurasi

```text
show running-config
```

---

## 28. Kesalahan yang Sering Terjadi

### Kesalahan 1 — Membuat VLAN tetapi lupa memasukkan port

Membuat:

```text
vlan 10
name STAFF
```

belum otomatis memindahkan Fa0/1 ke VLAN 10.

Solusi:

```text
interface fa0/1
switchport mode access
switchport access vlan 10
```

### Kesalahan 2 — IP Address salah

Contoh:

```text
PC0 = 192.168.10.10
PC1 = 192.168.20.10
```

Padahal keduanya seharusnya berada di VLAN 10.

### Kesalahan 3 — Mengharapkan VLAN berbeda langsung bisa ping

```text
VLAN 10 → VLAN 20
```

Tanpa router atau Layer 3 switch.

Ini memang tidak akan bekerja pada topology latihan kita.

---

## 29. Latihan 1 — Identifikasi VLAN

Diberikan:

```text
Fa0/1 → VLAN 10
Fa0/2 → VLAN 10
Fa0/3 → VLAN 20
Fa0/4 → VLAN 20
```

Jawab:

1. Apakah PC pada Fa0/1 dapat berkomunikasi langsung dengan PC pada Fa0/2?
2. Apakah PC pada Fa0/1 dapat berkomunikasi langsung dengan PC pada Fa0/3?
3. VLAN berapa yang digunakan Fa0/4?

### Jawaban

1. Ya, karena keduanya VLAN 10.
2. Tidak, karena berbeda VLAN dan belum ada routing.
3. VLAN 20.

---

## 30. Latihan 2 — Buat VLAN Sendiri

Buat topology:

```text
PC0
PC1
PC2
PC3
      |
    Switch
```

Buat:

```text
VLAN 10 = STAFF
VLAN 20 = GUEST
```

Atur:

```text
PC0 → VLAN 10
PC1 → VLAN 10

PC2 → VLAN 20
PC3 → VLAN 20
```

Gunakan:

```text
VLAN 10:
192.168.10.0/24

VLAN 20:
192.168.20.0/24
```

Uji:

```text
PC0 → PC1
PC2 → PC3
PC0 → PC2
```

Catat mana yang berhasil dan mana yang gagal.

---

## 31. Mini Quiz

### 1. Apa kepanjangan VLAN?

A. Virtual Local Area Network  
B. Variable Local Area Network  
C. Virtual Link Access Network  
D. Virtual LAN Address Network

### 2. Apa fungsi utama VLAN?

A. Mengganti kabel jaringan  
B. Membagi jaringan secara logis  
C. Menghapus IP address  
D. Mengubah switch menjadi router

### 3. Port untuk perangkat seperti PC biasanya menggunakan?

A. Trunk  
B. Access  
C. Routing  
D. WAN

### 4. Perintah untuk melihat daftar VLAN adalah?

A.

```text
show vlan brief
```

B.

```text
show ip route
```

C.

```text
show interfaces trunk
```

D.

```text
show mac address
```

### 5. Apakah VLAN 10 dan VLAN 20 dapat berkomunikasi langsung melalui switch Layer 2?

A. Ya  
B. Tidak

Jawaban:

```text
1. A
2. B
3. B
4. A
5. B
```

---

## 32. Ringkasan Day 6

Hari ini kita mempelajari:

- VLAN = Virtual Local Area Network
- VLAN membagi jaringan secara logis
- VLAN memiliki VLAN ID
- Access port digunakan untuk perangkat akhir
- Trunk digunakan untuk membawa beberapa VLAN
- VLAN dapat digunakan untuk memisahkan kelompok perangkat
- `vlan 10` membuat VLAN 10
- `name STAFF` memberi nama VLAN
- `switchport mode access` membuat port menjadi access
- `switchport access vlan 10` memasukkan port ke VLAN 10
- `show vlan brief` digunakan untuk melihat VLAN
- Perangkat dalam VLAN yang sama dapat berkomunikasi jika konfigurasi IP benar
- VLAN berbeda membutuhkan routing untuk berkomunikasi
- Inter-VLAN Routing digunakan untuk komunikasi antar-VLAN

---

## 33. Command Penting Day 6

### Membuat VLAN

```text
vlan 10
name STAFF
```

### Membuat VLAN 20

```text
vlan 20
name STUDENT
```

### Mengatur access port

```text
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
```

### Menggunakan interface range

```text
interface range fastEthernet 0/1 - 3
switchport mode access
switchport access vlan 10
```

### Melihat VLAN

```text
show vlan brief
```

### Melihat konfigurasi

```text
show running-config
```

### Pengantar trunk

```text
interface fastEthernet 0/24
switchport mode trunk
```

### Melihat trunk

```text
show interfaces trunk
```

---

## 34. Next Step — Day 7

Pada Day 7 kita dapat mulai mempelajari:

- Inter-VLAN Routing
- Mengapa router dibutuhkan untuk komunikasi antar-VLAN
- Router-on-a-Stick
- Subinterface router
- 802.1Q
- Konfigurasi trunk
- Gateway untuk masing-masing VLAN
- Pengujian komunikasi antar-VLAN
- Troubleshooting Inter-VLAN Routing

> Day 6 selesai. Jangan hanya membaca — coba topology VLAN ini langsung di Cisco Packet Tracer.
