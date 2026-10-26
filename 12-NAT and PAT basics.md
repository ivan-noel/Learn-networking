# Day 12 — NAT and PAT Basics

## 1. Tujuan Pembelajaran
Setelah mempelajari materi ini, kamu diharapkan mampu:
- Menjelaskan NAT (*Network Address Translation*) dan PAT (*Port Address Translation*).
- Membedakan Static NAT, Dynamic NAT, dan PAT/NAT Overload.
- Membuat topologi NAT sederhana di Cisco Packet Tracer.
- Mengonfigurasi dan menguji PAT melalui Cisco IOS CLI.
- Melakukan troubleshooting dasar.

## 2. Recall Day 11
Pada Day 11, kamu mempelajari ACL (*Access Control List*) untuk mengizinkan atau memblokir traffic. Pada materi ini, ACL juga digunakan untuk memilih alamat sumber yang akan diterjemahkan oleh NAT.

## 3. Apa Itu NAT?
**NAT** menerjemahkan alamat IP pada paket saat paket melewati router. NAT sering digunakan agar perangkat dengan IPv4 privat dapat berkomunikasi melalui alamat IPv4 lain.

Rentang IPv4 privat:
| Rentang | CIDR |
|---|---|
| `10.0.0.0` – `10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0` – `172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0` – `192.168.255.255` | `192.168.0.0/16` |

Istilah penting:
- **Inside local:** alamat perangkat internal yang digunakan di jaringan internal.
- **Inside global:** alamat yang mewakili perangkat internal di sisi luar setelah diterjemahkan.
- **Inside interface:** interface router yang menghadap jaringan internal.
- **Outside interface:** interface router yang menghadap jaringan luar.

NAT bukan pengganti firewall. NAT menerjemahkan alamat, sedangkan firewall/ACL menerapkan kebijakan traffic.

## 4. Jenis NAT
### Static NAT
Pemetaan satu alamat internal ke satu alamat luar secara tetap.
```text
ip nat inside source static 192.168.10.10 203.0.113.10
```

### Dynamic NAT
Memetakan alamat internal ke alamat yang tersedia dari sebuah pool alamat luar.

### PAT (NAT Overload)
Memungkinkan banyak perangkat internal berbagi satu alamat IP luar. Router membedakan sesi menggunakan nomor port dan informasi koneksi. PAT lazim digunakan pada jaringan rumah dan kantor kecil.

## 5. Topologi Packet Tracer
```text
PC0 ──┐
      ├── SW0 ─── R1 ─── R2 ─── Server
PC1 ──┘        inside   outside
```
Perangkat: 2 PC, 1 switch, 2 router, dan 1 server.

`203.0.113.0/24` dan `198.51.100.0/24` merupakan rentang dokumentasi untuk lab, bukan alamat internet publik untuk penggunaan nyata. Nama interface dapat berbeda menurut model router; sesuaikan jika perangkatmu menggunakan `GigabitEthernet0/0/0`.

## 6. IP Address Plan
| Perangkat | Interface | IP address | Subnet mask | Gateway |
|---|---|---|---|---|
| PC0 | NIC | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| PC1 | NIC | `192.168.10.20` | `255.255.255.0` | `192.168.10.1` |
| R1 | G0/0 (inside) | `192.168.10.1` | `255.255.255.0` | — |
| R1 | G0/1 (outside) | `203.0.113.2` | `255.255.255.252` | — |
| R2 | G0/0 | `203.0.113.1` | `255.255.255.252` | — |
| R2 | G0/1 | `198.51.100.1` | `255.255.255.0` | — |
| Server | NIC | `198.51.100.10` | `255.255.255.0` | `198.51.100.1` |

Atur IP PC dan Server melalui **Desktop → IP Configuration**.

## 7. Konfigurasi R1
```text
enable
configure terminal
interface gigabitEthernet 0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/1
 ip address 203.0.113.2 255.255.255.252
 no shutdown
exit
ip route 0.0.0.0 0.0.0.0 203.0.113.1
end
show ip interface brief
show ip route
```
Default route memberi R1 jalur menuju jaringan yang tidak tercantum secara lebih spesifik pada tabel routing.

## 8. Konfigurasi R2
```text
enable
configure terminal
interface gigabitEthernet 0/0
 ip address 203.0.113.1 255.255.255.252
 no shutdown
exit
interface gigabitEthernet 0/1
 ip address 198.51.100.1 255.255.255.0
 no shutdown
exit
ip route 192.168.10.0 255.255.255.0 203.0.113.2
end
show ip interface brief
show ip route
```
Static route di R2 menyediakan jalur balik ke LAN internal agar balasan dari Server dapat mencapai PC.

## 9. Konfigurasi PAT pada R1
PAT akan menggunakan alamat interface luar R1 (`203.0.113.2`) untuk traffic dari `192.168.10.0/24`.
```text
enable
configure terminal
access-list 1 permit 192.168.10.0 0.0.0.255
interface gigabitEthernet 0/0
 ip nat inside
exit
interface gigabitEthernet 0/1
 ip nat outside
exit
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
end
```
- ACL 1 memilih alamat sumber dari LAN internal.
- `ip nat inside` menandai interface internal.
- `ip nat outside` menandai interface luar.
- `overload` mengaktifkan PAT agar banyak perangkat dapat berbagi alamat interface luar.

## 10. Pengujian
Pastikan interface router berstatus `up/up`. Dari PC0, buka **Desktop → Command Prompt**:
```text
ping 192.168.10.1
ping 203.0.113.1
ping 198.51.100.10
```
Ulangi ping dari PC1 ke `198.51.100.10`. Pada R1, jalankan:
```text
show ip nat translations
show ip nat statistics
show access-lists
```
Buat traffic terlebih dahulu sebelum memeriksa tabel translasi. Entri NAT dapat berubah atau hilang setelah beberapa waktu.

## 11. Troubleshooting
| Masalah | Hal yang diperiksa |
|---|---|
| PC tidak bisa ping gateway | IP, mask, gateway, kabel, dan status interface |
| R1 tidak mencapai R2 | IP kedua ujung link, mask `/30`, dan `no shutdown` |
| Server tidak bisa dijangkau | IP/gateway Server dan route pada R1/R2 |
| Tabel NAT kosong | Buat traffic dari inside ke outside, lalu periksa lagi |
| NAT tidak menerjemahkan alamat | ACL, `ip nat inside`, `ip nat outside`, dan perintah PAT |
| Nama interface ditolak | Periksa `show ip interface brief` |

Perintah tambahan:
```text
show running-config
show ip route
show ip interface brief
show access-lists
```
Uji koneksi secara bertahap: PC ke gateway, link antar-router, lalu Server.

## 12. Latihan Mandiri
1. Jelaskan perbedaan NAT dan PAT dengan bahasamu sendiri.
2. Mengapa R1 G0/0 diberi `ip nat inside`?
3. Apa fungsi `overload`?
4. Jika IP PC1 diubah menjadi `192.168.10.30`, apakah masih termasuk jaringan yang dipilih ACL? Jelaskan.
5. Sebutkan tiga perintah untuk memeriksa NAT.
6. Apa akibatnya jika route kembali ke `192.168.10.0/24` tidak tersedia di R2?

## 13. Mini Quiz
**1. Apa kepanjangan NAT?**  
A. Network Access Table  
B. Network Address Translation  
C. Node Address Transfer  
D. Network Automatic Tunnel

**2. Teknik agar banyak perangkat berbagi satu alamat luar menggunakan port adalah ...**  
A. Static routing  
B. VLAN  
C. PAT  
D. DHCP

**3. Fungsi `ip nat outside` adalah ...**  
A. Mengaktifkan DHCP  
B. Menandai interface yang menghadap jaringan luar  
C. Membuat VLAN  
D. Memblokir semua traffic

**4. Fungsi `overload` adalah ...**  
A. Memungkinkan banyak perangkat/koneksi berbagi alamat luar melalui PAT  
B. Mematikan interface  
C. Menghapus routing table  
D. Mengganti subnet mask

### Kunci Jawaban
1. B — Network Address Translation.
2. C — PAT.
3. B — Menandai interface luar.
4. A — Berbagi alamat luar melalui PAT.

## 14. Ringkasan
- NAT menerjemahkan alamat IP saat traffic melewati router.
- Static NAT memakai pemetaan tetap satu-ke-satu.
- Dynamic NAT menggunakan pool alamat luar.
- PAT/NAT Overload memungkinkan banyak perangkat berbagi satu alamat IP luar.
- Konfigurasi PAT menggunakan ACL, `ip nat inside`, `ip nat outside`, dan `ip nat inside source ... overload`.
- Routing dua arah tetap diperlukan agar traffic dan balasannya mencapai tujuan.

## 15. Preview Day 13
Day 13 membahas **IPv6 Basics**: format alamat IPv6, penyingkatan alamat, jenis alamat dasar, dan konfigurasi IPv6 pada perangkat Cisco.
