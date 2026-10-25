# Day 11 — ACL Basics and Traffic Filtering

## 1. Tujuan Pembelajaran
- Menjelaskan fungsi Access Control List (ACL).
- Membedakan standard ACL dan extended ACL.
- Mengonfigurasi standard ACL sederhana pada router Cisco.
- Menguji hasil ACL dan melakukan troubleshooting.

## 2. Recall Day 10
Day 10 membahas DHCP untuk memberikan konfigurasi IP secara otomatis. Day 11 membahas ACL, yaitu aturan untuk memfilter paket jaringan menggunakan `permit` dan `deny`.

## 3. Materi Inti

### Apa itu ACL?
**Access Control List (ACL)** adalah kumpulan aturan yang diperiksa router untuk menentukan apakah paket diizinkan (`permit`) atau ditolak (`deny`). ACL bisa membatasi traffic berdasarkan alamat IP dan, untuk jenis tertentu, protocol atau port.

### Standard ACL vs Extended ACL

| Jenis | Yang diperiksa | Kegunaan |
|---|---|---|
| Standard ACL | IP sumber | Mengizinkan/menolak berdasarkan alamat sumber |
| Extended ACL | IP sumber, IP tujuan, protocol, dan port | Memfilter traffic lebih spesifik |

Materi ini menggunakan standard IPv4 ACL bernomor.

### Aturan penting
1. ACL diperiksa berurutan dari atas ke bawah.
2. Aturan pertama yang cocok menentukan tindakan.
3. Letakkan aturan lebih spesifik sebelum aturan umum.
4. Ada **implicit deny** di akhir: paket yang tidak cocok dengan aturan permit akan ditolak.
5. Arah ACL dilihat dari perspektif router: `in` berarti masuk ke interface; `out` berarti keluar dari interface.

## 4. Topologi Packet Tracer

```text
PC0 ──┐
      ├── SW0 ─── R1 ─── SW1 ── PC1
PC2 ──┘
```

PC0 dan PC2 berada di LAN kiri, sedangkan PC1 berada di LAN kanan.

## 5. IP Address Plan

| Perangkat | Interface | IP Address | Subnet Mask |
|---|---|---|---|
| PC0 | NIC | `192.168.10.10` | `255.255.255.0` |
| PC2 | NIC | `192.168.10.20` | `255.255.255.0` |
| R1 | G0/0 (LAN kiri) | `192.168.10.1` | `255.255.255.0` |
| R1 | G0/1 (LAN kanan) | `192.168.20.1` | `255.255.255.0` |
| PC1 | NIC | `192.168.20.10` | `255.255.255.0` |

Gateway PC0 dan PC2: `192.168.10.1`. Gateway PC1: `192.168.20.1`.

## 6. Konfigurasi Router

Sesuaikan nama interface jika model router berbeda.

```text
enable
configure terminal
interface gigabitEthernet 0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
end
```

Atur alamat IP dan gateway PC sesuai tabel. Periksa interface:

```text
show ip interface brief
```

## 7. Uji Sebelum ACL
Dari PC0, buka **Desktop → Command Prompt**:

```text
ping 192.168.20.10
```

Pastikan ping berhasil sebelum memasang ACL. Jika gagal, periksa IP, gateway, kabel, dan status interface.

## 8. Konfigurasi Standard ACL

Tujuan: blokir PC0 (`192.168.10.10`) menuju LAN kanan, tetapi izinkan sumber lain dari LAN kiri.

```text
enable
configure terminal
access-list 10 deny host 192.168.10.10
access-list 10 permit 192.168.10.0 0.0.0.255
interface gigabitEthernet 0/1
 ip access-group 10 out
exit
end
```

Penjelasan:
- `access-list 10` membuat standard ACL bernomor 10.
- `deny host 192.168.10.10` menolak traffic dengan alamat sumber PC0.
- `permit 192.168.10.0 0.0.0.255` mengizinkan sumber lain dari LAN kiri.
- `ip access-group 10 out` menerapkan ACL saat paket keluar dari G0/1 menuju LAN kanan.

Aturan deny diletakkan lebih dahulu. Jika aturan permit untuk seluruh LAN kiri berada di atas, paket PC0 akan cocok dengan permit itu sebelum mencapai deny.

### Wildcard mask
Untuk subnet `/24`, subnet mask adalah `255.255.255.0` dan wildcard mask adalah `0.0.0.255`. Pada wildcard mask, bit 0 harus cocok dan bit 1 diabaikan.

## 9. Verifikasi ACL

```text
show access-lists
show ip interface gigabitEthernet 0/1
show running-config
```

## 10. Pengujian Setelah ACL

Dari PC0:

```text
ping 192.168.20.10
```

Ping seharusnya gagal karena sumber PC0 ditolak.

Dari PC2:

```text
ping 192.168.20.10
```

Ping seharusnya berhasil karena PC2 cocok dengan aturan permit. Jika ping pertama gagal karena proses awal ARP atau simulasi, coba ulangi dan periksa hit count ACL.

## 11. Troubleshooting

| Masalah | Pemeriksaan |
|---|---|
| Semua PC gagal mengakses LAN kanan | Periksa urutan ACL dan aturan permit. |
| PC0 masih bisa ping PC1 | Pastikan ACL diterapkan pada G0/1 dengan arah `out`. |
| PC2 ikut terblokir | Periksa IP PC2, wildcard mask, dan urutan aturan. |
| ACL tidak tampak pada interface | Periksa `show ip interface` dan `show running-config`. |
| Ping gagal sebelum ACL dipasang | Perbaiki koneksi dasar, IP, gateway, dan interface dahulu. |

Standard ACL hanya memeriksa IP sumber, bukan IP tujuan. Karena itu, penempatannya perlu dipikirkan. Pada latihan ini ACL dipasang dekat LAN tujuan agar sesuai dengan tujuan latihan.

## 12. Latihan Mandiri
1. Buat topologi dua LAN dengan satu router.
2. Pastikan semua PC dapat ping sebelum ACL.
3. Terapkan ACL 10 seperti contoh.
4. Catat hasil ping PC0 dan PC2 ke PC1.
5. Jalankan `show access-lists` dan amati hit count.
6. Ubah sumber yang diblokir menjadi PC2 dan uji kembali. Pastikan aturan lama diganti dengan benar agar tidak membingungkan.

## 13. Mini Quiz

**1. Apa fungsi ACL?**  
Jawaban: Memeriksa traffic dan mengizinkan atau menolak paket berdasarkan aturan.

**2. Perbedaan standard dan extended ACL?**  
Jawaban: Standard ACL memeriksa IP sumber; extended ACL dapat memeriksa sumber, tujuan, protocol, dan port.

**3. Apa arti `deny`?**  
Jawaban: Menolak paket yang cocok dengan aturan.

**4. Apa itu implicit deny?**  
Jawaban: Paket yang tidak cocok dengan aturan permit mana pun ditolak secara default di akhir ACL.

**5. Apa arti `out` pada `ip access-group 10 out`?**  
Jawaban: ACL memeriksa paket ketika keluar dari interface router itu.

## 14. Ringkasan
- ACL memfilter traffic menggunakan aturan `permit` dan `deny`.
- Standard ACL memeriksa IP sumber; extended ACL bisa memeriksa lebih banyak kriteria.
- ACL dibaca dari atas ke bawah dan memiliki implicit deny.
- ACL diterapkan pada interface dengan arah `in` atau `out`.
- Gunakan `show access-lists` dan `show ip interface` untuk verifikasi.
- Uji koneksi sebelum dan sesudah ACL.

## 15. Preview Day 12
Day 12 akan membahas **NAT (Network Address Translation)**, termasuk konsep NAT overload/PAT.
