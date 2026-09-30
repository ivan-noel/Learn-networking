# Cisco Networking Learning — Day 2

> Learning Cisco Networking from 0  
> Day 2 — First Network with Cisco Packet Tracer

---

## 1. Tujuan Day 2

Pada Day 2 kita mulai praktik langsung menggunakan **Cisco Packet Tracer**.

Target hari ini:

- Mengenal interface Packet Tracer
- Membuat topology sederhana
- Menambahkan PC dan Switch
- Menghubungkan perangkat dengan kabel
- Memberikan IP Address
- Menguji koneksi dengan `ping`
- Mulai mengenal CLI Cisco

Topology:

```text
PC 1 -------- Switch -------- PC 2
```

---

## 2. Apa itu Topology?

Topology adalah gambaran bagaimana perangkat dalam jaringan saling terhubung.

```text
PC 1 -------- Switch -------- PC 2
```

Artinya PC 1 dan PC 2 terhubung melalui switch dan berada dalam satu jaringan.

---

## 3. Membuat Project

Buka **Cisco Packet Tracer** dan buat project baru.

Simpan dengan nama:

```text
Day_2_First_Network
```

---

## 4. Menambahkan PC

1. Pilih **End Devices**
2. Pilih **PC**
3. Letakkan dua PC pada workspace

Berikan nama:

```text
PC 1
PC 2
```

---

## 5. Menambahkan Switch

1. Pilih **Network Devices**
2. Pilih **Switches**
3. Pilih **2960**
4. Letakkan switch di antara kedua PC

Topology:

```text
PC 1          Switch          PC 2
```

---

## 6. Menghubungkan Perangkat

Pilih **Connections** lalu gunakan:

```text
Copper Straight-Through
```

Hubungkan:

```text
PC 1 → Switch
PC 2 → Switch
```

Contoh port:

```text
PC 1 FastEthernet0 → Switch FastEthernet0/1
PC 2 FastEthernet0 → Switch FastEthernet0/2
```

Topology:

```text
PC 1 -------- Switch -------- PC 2
```

Tunggu beberapa saat sampai indikator koneksi menjadi aktif.

---

## 7. Memberikan IP Address

Gunakan jaringan:

```text
192.168.1.0/24
```

PC 1:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
```

PC 2:

```text
IP Address: 192.168.1.11
Subnet Mask: 255.255.255.0
```

Untuk latihan ini **Default Gateway dikosongkan** karena kedua PC berada dalam network yang sama.

---

## 8. Konfigurasi IP PC 1

Klik:

```text
PC 1
↓
Desktop
↓
IP Configuration
```

Masukkan:

```text
IP Address:
192.168.1.10

Subnet Mask:
255.255.255.0
```

---

## 9. Konfigurasi IP PC 2

Klik:

```text
PC 2
↓
Desktop
↓
IP Configuration
```

Masukkan:

```text
IP Address:
192.168.1.11

Subnet Mask:
255.255.255.0
```

---

## 10. Menguji Koneksi dengan Ping

Pada PC 1:

```text
PC 1
↓
Desktop
↓
Command Prompt
```

Ketik:

```bash
ping 192.168.1.11
```

Jika berhasil, akan muncul reply dari PC 2.

Contoh:

```text
Reply from 192.168.1.11
```

Artinya:

```text
PC 1 → Switch → PC 2
```

berhasil berkomunikasi.

---

## 11. Apa yang Terjadi Saat Ping?

PC 1 mengirim **ICMP Echo Request** menuju PC 2.

```text
PC 1
192.168.1.10
    |
    | Echo Request
    ↓
 Switch
    |
    ↓
PC 2
192.168.1.11
```

PC 2 membalas dengan **ICMP Echo Reply**:

```text
PC 2
    |
    | Echo Reply
    ↓
 Switch
    |
    ↓
PC 1
```

Jika reply diterima, koneksi berhasil.

---

## 12. Fungsi Switch

Switch menjadi penghubung antara PC 1 dan PC 2.

```text
PC 1 -------- Switch -------- PC 2
```

Switch menggunakan **MAC Address** untuk menentukan ke mana frame Ethernet harus diteruskan.

Untuk sekarang cukup pahami:

```text
PC 1
  ↓
Switch
  ↓
PC 2
```

---

## 13. Melihat MAC Address

Pada PC:

```text
PC
↓
Desktop
↓
Command Prompt
```

Gunakan:

```bash
ipconfig /all
```

Informasi yang dapat terlihat antara lain:

- IP Address
- Subnet Mask
- Default Gateway
- MAC Address

MAC Address biasanya ditampilkan sebagai **Physical Address**.

---

## 14. IP Address vs MAC Address

### IP Address

Contoh:

```text
192.168.1.10
```

Digunakan sebagai alamat pada jaringan IP.

### MAC Address

Contoh:

```text
00E0.8F12.3456
```

Digunakan pada komunikasi Ethernet dan berhubungan dengan network interface.

Secara sederhana:

```text
IP Address
    ↓
Alamat logis

MAC Address
    ↓
Alamat hardware/interface
```

---

## 15. Mengenal CLI Switch

Klik:

```text
Switch
↓
CLI
```

Prompt awal biasanya:

```text
Switch>
```

Ini disebut **User EXEC Mode**.

---

## 16. Masuk ke Privileged EXEC Mode

Ketik:

```text
enable
```

Prompt berubah:

```text
Switch>
```

menjadi:

```text
Switch#
```

Sekarang kita berada pada **Privileged EXEC Mode**.

---

## 17. Melihat MAC Address Table

Gunakan:

```text
show mac address-table
```

Switch dapat menampilkan MAC Address yang telah dipelajari dari perangkat yang terhubung.

Contoh konsep:

```text
MAC Address       Port
-------------------------
PC 1 MAC          Fa0/1
PC 2 MAC          Fa0/2
```

Ini membantu memahami bagaimana switch mengetahui perangkat pada port tertentu.

---

## 18. Melihat Interface Switch

Gunakan:

```text
show interfaces
```

Untuk melihat ringkasan interface:

```text
show ip interface brief
```

Pada latihan ini, interface fisik switch belum perlu diberikan IP Address secara langsung.

---

## 19. Command yang Dipelajari

| Command | Fungsi |
|---|---|
| `enable` | Masuk ke Privileged EXEC Mode |
| `ping` | Menguji konektivitas |
| `ipconfig /all` | Melihat konfigurasi jaringan pada PC |
| `show mac address-table` | Melihat MAC Address yang dipelajari switch |
| `show interfaces` | Melihat informasi interface |
| `show ip interface brief` | Melihat ringkasan interface |

---

## 20. Troubleshooting Jika Ping Gagal

Periksa satu per satu.

### Check 1 — Kabel

Pastikan:

```text
PC 1 → Switch
PC 2 → Switch
```

menggunakan kabel yang benar.

### Check 2 — IP Address

PC 1:

```text
192.168.1.10
```

PC 2:

```text
192.168.1.11
```

### Check 3 — Subnet Mask

Keduanya:

```text
255.255.255.0
```

### Check 4 — Link

Pastikan koneksi interface sudah aktif.

### Check 5 — Ping

Dari PC 1:

```bash
ping 192.168.1.11
```

Dari PC 2:

```bash
ping 192.168.1.10
```

---

## 21. Latihan Mandiri

Ubah IP Address menjadi:

```text
PC 1
192.168.10.10

PC 2
192.168.10.20
```

Subnet Mask:

```text
255.255.255.0
```

Kemudian dari PC 1:

```bash
ping 192.168.10.20
```

Jika berhasil, berarti konsep dasar IP Address dan koneksi sederhana sudah mulai dipahami.

---

## 22. Eksperimen: Membuat Ping Gagal

Coba gunakan:

PC 1:

```text
192.168.1.10
```

PC 2:

```text
192.168.2.10
```

Subnet Mask:

```text
255.255.255.0
```

Kemudian:

```bash
ping 192.168.2.10
```

Koneksi tidak akan berhasil karena kedua PC berada pada network yang berbeda:

```text
PC 1
192.168.1.10
    ↓
192.168.1.0/24


PC 2
192.168.2.10
    ↓
192.168.2.0/24
```

Untuk menghubungkan network yang berbeda, nantinya kita membutuhkan **Router**.

---

## 23. Topology Day 2

```text
                Switch
               /      \
              /        \
           PC 1        PC 2
             |            |
      192.168.1.10  192.168.1.11
```

Komunikasi:

```text
PC 1
  |
  | Frame
  ↓
Switch
  |
  | Frame
  ↓
PC 2
```

---

## 24. Mini Quiz

Coba jawab tanpa melihat materi.

### Question 1

Apa fungsi switch pada topology Day 2?

### Question 2

Apa IP Address PC 1?

### Question 3

Apa IP Address PC 2?

### Question 4

Apa fungsi `ping`?

### Question 5

Apa yang terjadi jika dua PC memiliki IP Address dari network yang berbeda?

### Question 6

Command apa yang digunakan untuk melihat MAC Address Table pada switch?

### Question 7

Apa perubahan prompt setelah menjalankan:

```text
enable
```

dari:

```text
Switch>
```

?

### Question 8

Apa fungsi `ipconfig /all`?

### Question 9

Mengapa PC 1 dan PC 2 pada latihan utama dapat berkomunikasi tanpa router?

### Question 10

Perangkat apa yang digunakan untuk menghubungkan network yang berbeda?

---

## 25. Day 2 Summary

Pada Day 2 kita melakukan praktik pertama menggunakan Cisco Packet Tracer.

Materi:

- Membuat project Packet Tracer
- Membuat topology
- Menambahkan PC
- Menambahkan Switch
- Menghubungkan perangkat
- Menggunakan Copper Straight-Through
- Mengatur IP Address
- Mengatur Subnet Mask
- Menggunakan `ping`
- Memahami ICMP Echo Request dan Echo Reply
- Melihat MAC Address
- Mengenal MAC Address Table
- Mengenal CLI Switch
- Menggunakan command dasar Cisco
- Troubleshooting sederhana

Konsep utama:

```text
PC 1
192.168.1.10
     |
     ↓
  Switch
     ↑
     |
PC 2
192.168.1.11
```

Jika kedua PC berada pada network yang sama:

```text
192.168.1.0/24
```

keduanya dapat berkomunikasi melalui switch tanpa router.

---

## Next Step — Day 3

Pada Day 3 kita akan mulai mengenal **Router** dan memahami kenapa router dibutuhkan ketika terdapat dua network yang berbeda.

Target:

1. Menambahkan Router ke topology
2. Mengenal interface router
3. Mengenal IP Address pada interface router
4. Masuk ke Router CLI
5. Mengaktifkan interface router
6. Menghubungkan dua network
7. Melakukan ping antar-network
8. Memahami konsep default gateway

> **Goal:** Memahami perbedaan komunikasi dalam satu network menggunakan switch dan komunikasi antar-network menggunakan router.
