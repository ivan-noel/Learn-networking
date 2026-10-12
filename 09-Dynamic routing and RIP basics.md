# Day 9 — Dynamic Routing and RIP Basics

## 1. Tujuan Pembelajaran
Setelah mempelajari materi ini, kamu mampu:
- Menjelaskan static routing dan dynamic routing.
- Memahami fungsi routing protocol.
- Mengonfigurasi RIP version 2 (RIPv2) pada router Cisco.
- Memeriksa routing table dan menguji konektivitas.

## 2. Recall Day 8
Pada Day 8, kita menghubungkan dua LAN menggunakan dua router dan memasukkan static route secara manual, misalnya:

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

Static route berguna, tetapi jika jaringan bertambah atau jalur berubah, rute mungkin perlu diperbarui secara manual.

## 3. Materi Inti

### 3.1 Dynamic Routing
Dynamic routing memungkinkan router bertukar informasi jaringan melalui **routing protocol**. Router kemudian dapat mempelajari jaringan remote dan memperbarui routing table berdasarkan informasi yang diterima.

Contoh routing protocol:
- **RIP:** sederhana, menggunakan hop count.
- **OSPF:** link-state protocol yang umum dipakai pada jaringan internal.
- **EIGRP:** routing protocol yang banyak dikaitkan dengan lingkungan Cisco.
- **BGP:** digunakan untuk pertukaran rute antar-autonomous system, termasuk di internet.

RIP cocok untuk latihan pemula, tetapi batas hop dan karakteristik konvergensinya membuatnya kurang cocok untuk jaringan modern yang besar.

### 3.2 Static Routing vs Dynamic Routing

| Aspek | Static Routing | Dynamic Routing |
|---|---|---|
| Konfigurasi | Rute dimasukkan manual | Router belajar lewat protocol |
| Perubahan topologi | Perlu penyesuaian manual | Dapat beradaptasi sesuai protocol |
| Penggunaan | Jaringan kecil atau rute khusus | Jaringan dengan banyak router/jaringan |
| Kode rute Cisco | `S` | Bergantung protocol, misalnya `R` untuk RIP |

### 3.3 RIP dan RIPv2
**Routing Information Protocol (RIP)** memilih rute berdasarkan **hop count**, yaitu jumlah router yang dilewati untuk mencapai jaringan tujuan.

- RIPv2 mendukung subnet mask/VLSM dan menggunakan multicast `224.0.0.9`.
- Maksimum 15 hop dianggap dapat dijangkau; 16 hop dianggap tidak terjangkau.
- RIP mengirim pembaruan routing secara berkala.
- RIP sederhana untuk dipelajari, tetapi punya keterbatasan untuk jaringan besar.

### 3.4 Kode Routing Table
Jalankan `show ip route`. Kode umum:
- `C` — Connected, jaringan terhubung langsung.
- `L` — Local, alamat IP interface router.
- `S` — Static, rute manual.
- `R` — RIP, rute dipelajari melalui RIP.

## 4. Topologi Packet Tracer
Buat topologi tiga router:

```text
PC0 — SW0 — R1 — R2 — R3 — SW1 — PC1
```

Gunakan port Ethernet yang tersedia pada model routermu. Nama interface bisa berbeda-beda; sesuaikan perintah dengan perangkat yang dipakai.

## 5. IP Address Plan

| Perangkat | Interface | IP Address | Prefix |
|---|---|---|---|
| PC0 | NIC | `192.168.10.10` | `/24` |
| R1 | G0/0 (LAN) | `192.168.10.1` | `/24` |
| R1 | G0/1 (ke R2) | `10.0.12.1` | `/30` |
| R2 | G0/0 (ke R1) | `10.0.12.2` | `/30` |
| R2 | G0/1 (ke R3) | `10.0.23.1` | `/30` |
| R3 | G0/0 (ke R2) | `10.0.23.2` | `/30` |
| R3 | G0/1 (LAN) | `192.168.30.1` | `/24` |
| PC1 | NIC | `192.168.30.10` | `/24` |

Subnet mask `/24` adalah `255.255.255.0`. Subnet mask `/30` adalah `255.255.255.252`.

Default gateway PC0: `192.168.10.1`. Default gateway PC1: `192.168.30.1`.

## 6. Konfigurasi Interface Router
Pastikan interface sudah diberi IP address dan diaktifkan dengan `no shutdown`.

### R1
```text
enable
configure terminal
interface gigabitEthernet 0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/1
 ip address 10.0.12.1 255.255.255.252
 no shutdown
exit
end
```

### R2
```text
enable
configure terminal
interface gigabitEthernet 0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown
exit
interface gigabitEthernet 0/1
 ip address 10.0.23.1 255.255.255.252
 no shutdown
exit
end
```

### R3
```text
enable
configure terminal
interface gigabitEthernet 0/0
 ip address 10.0.23.2 255.255.255.252
 no shutdown
exit
interface gigabitEthernet 0/1
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit
end
```

Periksa status interface pada setiap router:

```text
show ip interface brief
```

Interface yang tersambung dan aktif seharusnya menunjukkan status `up` dan protocol `up`.

## 7. Konfigurasi PC
Di Packet Tracer, klik PC → **Desktop** → **IP Configuration**.

**PC0**
- IP Address: `192.168.10.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.10.1`

**PC1**
- IP Address: `192.168.30.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.30.1`

## 8. Mengaktifkan RIPv2
Pada Cisco IOS yang umum dipakai di Packet Tracer, perintah `network` mengaktifkan RIP pada interface yang termasuk dalam major network terkait. Perintah tersebut tidak ditulis dengan prefix `/24` atau `/30`.

### R1
```text
enable
configure terminal
router rip
 version 2
 no auto-summary
 network 192.168.10.0
 network 10.0.0.0
end
```

### R2
```text
enable
configure terminal
router rip
 version 2
 no auto-summary
 network 10.0.0.0
end
```

### R3
```text
enable
configure terminal
router rip
 version 2
 no auto-summary
 network 10.0.0.0
 network 192.168.30.0
end
```

Jangan tambahkan static route ke jaringan LAN remote selama latihan ini, agar rute yang muncul bisa diamati sebagai hasil RIP.

## 9. Pengujian

### 9.1 Periksa routing protocol
Pada masing-masing router:

```text
show ip protocols
```

Pastikan RIP version 2 aktif dan jaringan yang sesuai tercantum.

### 9.2 Periksa routing table
```text
show ip route
```

Setelah beberapa saat, R1 seharusnya mempelajari `192.168.30.0/24` melalui RIP, sedangkan R3 seharusnya mempelajari `192.168.10.0/24`. Rute hasil RIP ditandai kode `R`. Jaringan yang terhubung langsung tetap berkode `C`.

### 9.3 Uji ping
Dari PC0, buka **Desktop → Command Prompt**:

```text
ping 192.168.30.10
```

Coba juga dari PC1:

```text
ping 192.168.10.10
```

Ping pertama kadang gagal karena proses awal pembelajaran alamat atau routing belum selesai. Coba ulangi setelah beberapa saat.

## 10. Troubleshooting

| Masalah | Pemeriksaan |
|---|---|
| Interface `down` | Periksa kabel, port, dan `no shutdown`. |
| Rute `R` tidak muncul | Periksa `show ip protocols`, perintah `network`, dan IP interface. |
| PC tidak bisa ping gateway | Periksa IP, subnet mask, gateway, dan koneksi PC. |
| PC0 tidak bisa ping PC1 | Periksa interface antar-router, rute RIP di setiap router, dan gateway PC. |
| Nama interface tidak cocok | Jalankan `show ip interface brief` dan sesuaikan nama port. |
| Ada static route lama | Periksa rute manual yang mungkin membuat pengujian RIP membingungkan. Hapus hanya jika yakin rute tersebut memang dibuat untuk latihan sebelumnya. |

## 11. Latihan Mandiri
1. Buat topologi tiga router seperti pada materi.
2. Konfigurasikan IP address semua interface dan PC.
3. Aktifkan RIPv2 pada R1, R2, dan R3.
4. Tampilkan routing table masing-masing router.
5. Temukan rute berkode `R` pada R1 dan R3.
6. Uji ping dari PC0 ke PC1.
7. Untuk latihan tambahan, nonaktifkan sementara satu interface antar-router dengan `shutdown`, amati perubahan, lalu pulihkan dengan `no shutdown`.

## 12. Mini Quiz

**1. Apa perbedaan utama static routing dan dynamic routing?**  
Jawaban: Static route dimasukkan manual; dynamic routing mempelajari rute melalui routing protocol.

**2. Apa arti kode `R` pada `show ip route`?**  
Jawaban: Rute dipelajari melalui RIP.

**3. Metrik apa yang digunakan RIP?**  
Jawaban: Hop count.

**4. Berapa hop maksimum yang dianggap dapat dijangkau oleh RIP?**  
Jawaban: 15 hop; 16 dianggap tidak terjangkau.

**5. Mengapa PC membutuhkan default gateway untuk mencapai jaringan lain?**  
Jawaban: Karena paket ke luar subnet lokal perlu dikirim ke router.

## 13. Ringkasan
- Dynamic routing memungkinkan router bertukar informasi tentang jaringan remote.
- RIP adalah routing protocol sederhana yang menggunakan hop count.
- RIPv2 diaktifkan dengan `router rip`, `version 2`, dan perintah `network`.
- Gunakan `show ip protocols` dan `show ip route` untuk memeriksa konfigurasi.
- Kode `R` menandai rute yang dipelajari melalui RIP.
- Selalu periksa interface dan lakukan ping saat troubleshooting.

## 14. Preview Day 10
Day 10 akan membahas **DHCP (Dynamic Host Configuration Protocol)**: cara perangkat memperoleh IP address, subnet mask, default gateway, dan DNS secara otomatis.
