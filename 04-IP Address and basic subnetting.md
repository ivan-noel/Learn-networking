# Cisco Networking Learning — Day 4

> Learning Cisco Networking from 0  
> Day 4 — IP Address and Basic Subnetting

## 1. Tujuan Day 4

Hari ini kita akan mempelajari:
- IPv4
- Subnet mask
- CIDR / prefix
- Network address
- Broadcast address
- Host address
- Jumlah host
- Block size
- Subnetting dasar `/24` sampai `/28`
- Penerapan subnetting sederhana di Cisco Packet Tracer

---

## 2. Apa Itu IPv4?

IPv4 adalah sistem pengalamatan untuk memberi alamat pada perangkat jaringan.

Contoh:

```text
192.168.1.10
```

IPv4 memiliki panjang **32 bit** dan terdiri dari 4 octet:

```text
192 . 168 . 1 . 10
```

Setiap octet bernilai `0–255`.

---

## 3. Apa Itu Subnet Mask?

Subnet mask menentukan bagian **network** dan **host** dari sebuah IP.

Contoh:

```text
IP Address : 192.168.1.10
Subnet Mask: 255.255.255.0
```

Bentuk CIDR-nya:

```text
192.168.1.10/24
```

---

## 4. Apa Itu CIDR?

CIDR adalah penulisan singkat subnet mask.

```text
/24 = 255.255.255.0
/25 = 255.255.255.128
/26 = 255.255.255.192
/27 = 255.255.255.224
/28 = 255.255.255.240
```

Angka setelah `/` menunjukkan jumlah bit network.

Contoh `/24`:

```text
24 bit = Network
 8 bit = Host
```

Karena IPv4 memiliki 32 bit:

```text
32 - 24 = 8
```

---

## 5. Network Address

Network address adalah alamat yang menunjukkan identitas sebuah jaringan.

Contoh:

```text
192.168.1.10/24
```

Network address:

```text
192.168.1.0
```

Host lain seperti:

```text
192.168.1.20
192.168.1.30
192.168.1.100
```

masih berada di network:

```text
192.168.1.0/24
```

---

## 6. Broadcast Address

Broadcast address digunakan untuk mengirim data ke seluruh host dalam satu subnet.

Untuk:

```text
192.168.1.0/24
```

hasilnya:

```text
Network   : 192.168.1.0
Host      : 192.168.1.1 - 192.168.1.254
Broadcast : 192.168.1.255
```

Network dan broadcast tidak digunakan sebagai alamat host biasa.

---

## 7. Jumlah Host pada /24

Untuk:

```text
192.168.1.0/24
```

Host bits:

```text
32 - 24 = 8
```

Total address:

```text
2^8 = 256
```

Usable host:

```text
256 - 2 = 254
```

Jadi `/24` memiliki **254 usable hosts**.

---

## 8. Subnet /25

```text
Subnet Mask : 255.255.255.128
CIDR        : /25
Host Bits   : 7
Total       : 128
Usable Host : 126
```

Contoh pertama:

```text
Network   : 192.168.1.0
Host      : 192.168.1.1 - 192.168.1.126
Broadcast : 192.168.1.127
```

Subnet berikutnya:

```text
Network   : 192.168.1.128
Host      : 192.168.1.129 - 192.168.1.254
Broadcast : 192.168.1.255
```

---

## 9. Subnet /26

```text
Subnet Mask : 255.255.255.192
CIDR        : /26
Host Bits   : 6
Total       : 64
Usable Host : 62
```

Block size:

```text
256 - 192 = 64
```

Network:

```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

Contoh:

```text
Network   : 192.168.1.64
Host      : 192.168.1.65 - 192.168.1.126
Broadcast : 192.168.1.127
```

---

## 10. Subnet /27

```text
Subnet Mask : 255.255.255.224
CIDR        : /27
Host Bits   : 5
Total       : 32
Usable Host : 30
```

Block size:

```text
256 - 224 = 32
```

Network:

```text
192.168.1.0/27
192.168.1.32/27
192.168.1.64/27
192.168.1.96/27
192.168.1.128/27
192.168.1.160/27
192.168.1.192/27
192.168.1.224/27
```

---

## 11. Subnet /28

```text
Subnet Mask : 255.255.255.240
CIDR        : /28
Host Bits   : 4
Total       : 16
Usable Host : 14
```

Block size:

```text
256 - 240 = 16
```

Network:

```text
192.168.1.0/28
192.168.1.16/28
192.168.1.32/28
192.168.1.48/28
192.168.1.64/28
...
```

---

## 12. Tabel CIDR Dasar

| CIDR | Subnet Mask | Usable Host |
|---|---|---:|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /29 | 255.255.255.248 | 6 |
| /30 | 255.255.255.252 | 2 |

Rumus:

```text
Host Bits = 32 - Prefix
Total Address = 2^Host Bits
Usable Host = 2^Host Bits - 2
```

---

## 13. Block Size

Block size menentukan jarak antar network.

Rumus:

```text
Block Size = 256 - nilai subnet mask pada octet yang berubah
```

Contoh `/26`:

```text
256 - 192 = 64
```

Maka network:

```text
0, 64, 128, 192
```

Contoh `/27`:

```text
256 - 224 = 32
```

Maka network:

```text
0, 32, 64, 96, 128, 160, 192, 224
```

---

## 14. Contoh Menentukan Network Address

Diberikan:

```text
192.168.1.70/26
```

Subnet mask:

```text
255.255.255.192
```

Block size:

```text
256 - 192 = 64
```

Network:

```text
0
64
128
192
```

IP `70` berada pada range `64–127`.

Maka:

```text
Network   : 192.168.1.64
Host      : 192.168.1.65 - 192.168.1.126
Broadcast : 192.168.1.127
```

---

## 15. Contoh Lain

Diberikan:

```text
172.16.10.200/27
```

Subnet mask:

```text
255.255.255.224
```

Block size:

```text
256 - 224 = 32
```

IP `200` berada pada range `192–223`.

Maka:

```text
Network   : 172.16.10.192
Host      : 172.16.10.193 - 172.16.10.222
Broadcast : 172.16.10.223
```

---

## 16. Mengapa Subnetting Dibutuhkan?

Misalnya jaringan:

```text
192.168.1.0/24
```

memiliki 254 usable hosts.

Jika sebuah ruangan hanya membutuhkan 30 komputer, kita dapat menggunakan:

```text
192.168.1.0/27
```

yang menyediakan 30 usable hosts.

Subnetting membantu:
- Membagi jaringan
- Mengurangi pemborosan alamat IP
- Memisahkan kelompok perangkat
- Membuat desain jaringan lebih terstruktur

---

## 17. Penerapan di Cisco Packet Tracer

Topology:

```text
PC0 ---- Switch ---- Router ---- Switch ---- PC1
```

### Network 1

```text
Network : 192.168.10.0/26
Router  : 192.168.10.1

PC0:
IP Address : 192.168.10.10
Subnet     : 255.255.255.192
Gateway    : 192.168.10.1
```

### Network 2

```text
Network : 192.168.10.64/26
Router  : 192.168.10.65

PC1:
IP Address : 192.168.10.70
Subnet     : 255.255.255.192
Gateway    : 192.168.10.65
```

Konfigurasi router:

```text
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.192
no shutdown
exit

interface gigabitEthernet 0/1
ip address 192.168.10.65 255.255.255.192
no shutdown
exit
```

Periksa:

```text
show ip interface brief
```

Interface yang menunjukkan:

```text
up    up
```

berarti aktif.

Tes dari PC0:

```text
ping 192.168.10.70
```

Jika berhasil, PC0 dapat berkomunikasi dengan PC1 melalui router.

---

## 18. Hal yang Sering Membingungkan

### Network vs Host

```text
192.168.1.0/24
```

`192.168.1.0` adalah network address.

```text
192.168.1.10
```

adalah host address.

### Host vs Broadcast

Pada:

```text
192.168.1.0/24
```

host:

```text
192.168.1.1 - 192.168.1.254
```

broadcast:

```text
192.168.1.255
```

### /24 vs /26

```text
/24 = 254 usable hosts
/26 = 62 usable hosts
```

Semakin besar angka prefix, semakin kecil ukuran subnet dan semakin sedikit host per subnet.

---

## 19. Mini Quiz

1. Berapa bit panjang IPv4?
2. Berapa nilai maksimal sebuah octet?
3. Apa subnet mask dari `/24`?
4. Berapa usable host pada `/24`?
5. Berapa usable host pada `/26`?
6. Apa network address dari `192.168.1.70/26`?
7. Apa broadcast address dari `192.168.1.70/26`?
8. Berapa block size `/27`?
9. Apa network address dari `172.16.10.200/27`?
10. Apa broadcast address dari `172.16.10.200/27`?

---

## 20. Latihan Subnetting

Untuk setiap IP, cari:

1. Network Address
2. Broadcast Address
3. Usable Host Range
4. Jumlah Usable Host

### Soal 1

```text
192.168.10.50/24
```

### Soal 2

```text
192.168.10.100/25
```

### Soal 3

```text
192.168.10.150/26
```

### Soal 4

```text
192.168.10.200/27
```

### Soal 5

```text
192.168.10.50/28
```

---

## 21. Ringkasan Day 4

Hari ini kita mempelajari:

- IPv4 memiliki 32 bit.
- IPv4 terdiri dari 4 octet.
- Setiap octet bernilai 0–255.
- Subnet mask menentukan bagian network dan host.
- CIDR adalah bentuk singkat subnet mask.
- Network address adalah identitas subnet.
- Broadcast address digunakan untuk broadcast dalam subnet.
- Host address digunakan oleh perangkat.
- `/24` = 254 usable hosts.
- `/26` = 62 usable hosts.
- `/27` = 30 usable hosts.
- `/28` = 14 usable hosts.
- Block size membantu menentukan network address.
- Subnetting membagi jaringan menjadi subnet yang lebih kecil.

Konsep utama:

```text
IP Address
    ↓
Subnet Mask
    ↓
CIDR / Prefix
    ↓
Network Address
    ↓
Host Range
    ↓
Broadcast Address
```

---

## 22. Next Step — Day 5

Day 5 akan menggabungkan subnetting dengan praktik Cisco Packet Tracer:

- Membuat beberapa subnet
- Menentukan IP setiap perangkat
- Menentukan default gateway
- Menghubungkan beberapa network
- Konfigurasi router
- Pengujian dengan `ping`
- Troubleshooting kesalahan IP
- Membuat desain jaringan sederhana

> **Target Day 5:** mulai bisa melihat kebutuhan jaringan, menentukan subnet, lalu menerapkannya ke topology Cisco Packet Tracer.
