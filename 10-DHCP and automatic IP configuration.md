# Day 10 — DHCP and Automatic IP Configuration

## 1. Tujuan Pembelajaran
Setelah materi ini, kamu mampu menjelaskan DHCP, memahami proses DORA, mengonfigurasi router Cisco sebagai DHCP server di Packet Tracer, serta menguji dan melakukan troubleshooting dasar.

## 2. Recall Day 9
Day 9 membahas RIPv2, yaitu routing protocol agar router mempelajari rute ke jaringan lain. DHCP berbeda: DHCP memberikan konfigurasi IP kepada client secara otomatis.

## 3. Materi Inti

### 3.1 Apa itu DHCP?
**DHCP (Dynamic Host Configuration Protocol)** memberikan konfigurasi jaringan secara otomatis, seperti:
- IP address
- Subnet mask
- Default gateway
- DNS server

Tanpa DHCP, pengaturan tersebut perlu dimasukkan manual ke setiap perangkat.

### 3.2 IP Statis vs DHCP

| Aspek | IP Statis | DHCP |
|---|---|---|
| Konfigurasi | Manual | Otomatis |
| Banyak client | Lebih banyak pekerjaan | Lebih praktis |
| Penggunaan umum | Server atau perangkat tertentu | PC, laptop, ponsel, dan client lain |

Alamat DHCP biasanya memiliki **lease time**, yaitu masa peminjaman alamat yang dapat diperbarui.

### 3.3 Proses DORA
1. **Discover** — client mencari DHCP server.
2. **Offer** — server menawarkan konfigurasi.
3. **Request** — client meminta alamat yang ditawarkan.
4. **Acknowledge** — server mengonfirmasi konfigurasi.

### 3.4 DHCP Server dan Relay
DHCP server menyediakan konfigurasi IP. Jika server berada di subnet berbeda, router dapat menggunakan DHCP relay, yang pada Cisco IOS sering dikonfigurasi dengan `ip helper-address`. Pada latihan ini, router bertindak sebagai DHCP server untuk LAN yang terhubung langsung.

## 4. Topologi Packet Tracer

```text
PC0 ─┐
     ├── SW0 ─── R1
PC1 ─┘
```

Hubungkan dua PC ke switch, lalu switch ke interface LAN router.

## 5. IP Address Plan

| Perangkat | Interface | Pengaturan |
|---|---|---|
| R1 | G0/0 (LAN) | `192.168.10.1/24` |
| PC0 | NIC | DHCP |
| PC1 | NIC | DHCP |
| DHCP pool | LAN | `192.168.10.0/24` |
| Excluded range | — | `192.168.10.1`–`192.168.10.20` |

Subnet mask `/24` adalah `255.255.255.0`. Alamat router dikecualikan agar tidak diberikan kepada client.

## 6. Konfigurasi Router

Nama interface bisa berbeda menurut model router. Sesuaikan jika perangkatmu tidak memiliki `GigabitEthernet0/0`.

### 6.1 Konfigurasi interface LAN

```text
enable
configure terminal
interface gigabitEthernet 0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
```

### 6.2 Kecualikan alamat yang tidak boleh dibagikan

```text
ip dhcp excluded-address 192.168.10.1 192.168.10.20
```

### 6.3 Buat DHCP pool

```text
ip dhcp pool LAN-STAFF
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
exit
end
```

Penjelasan:
- `ip dhcp pool LAN-STAFF` membuat pool DHCP.
- `network` menentukan subnet yang dilayani.
- `default-router` memberikan default gateway ke client.
- `dns-server` memberikan alamat DNS contoh `8.8.8.8`. Akses internet tidak diperlukan untuk membuktikan DHCP bekerja di lab lokal.

## 7. Atur PC Menggunakan DHCP

Lakukan pada PC0 dan PC1:
1. Klik PC.
2. Pilih **Desktop**.
3. Buka **IP Configuration**.
4. Klik **DHCP**.
5. Tunggu sampai pengaturan terisi otomatis.

Kedua PC seharusnya menerima IP dari subnet `192.168.10.0/24`, biasanya di atas `192.168.10.20`, dengan gateway `192.168.10.1`. Alamat persisnya dapat berbeda.

## 8. Verifikasi

Periksa status interface router:

```text
show ip interface brief
```

Periksa pool:

```text
show ip dhcp pool
```

Periksa alamat yang telah diberikan:

```text
show ip dhcp binding
```

Dari PC0, buka **Desktop → Command Prompt**:

```text
ipconfig
ping 192.168.10.1
```

Ulangi pada PC1. Jika alamat dan gateway valid serta ping gateway berhasil, konfigurasi dasar kemungkinan sudah benar.

## 9. Troubleshooting

| Masalah | Yang perlu diperiksa |
|---|---|
| PC tidak mendapat IP yang diharapkan atau mendapat `169.254.x.x` | Pastikan DHCP dipilih, kabel aktif, dan pool benar. |
| PC tidak bisa ping gateway | Periksa IP interface router, subnet mask, kabel, dan `no shutdown`. |
| Binding belum muncul | Pastikan client meminta alamat dengan memilih DHCP dan tunggu proses selesai. |
| Client mendapat alamat salah | Periksa `network` dan excluded range. |
| Perintah tidak dikenali | Pastikan berada pada mode konfigurasi yang benar dan IOS mendukung perintah tersebut. |
| Pool kehabisan alamat | Periksa jumlah client, excluded addresses, dan kapasitas subnet. |

## 10. Latihan Mandiri
1. Buat topologi satu router, satu switch, dan dua PC.
2. Konfigurasikan interface LAN router `192.168.10.1/24`.
3. Kecualikan `192.168.10.1` sampai `192.168.10.20`.
4. Buat pool `LAN-STAFF`.
5. Atur kedua PC ke DHCP.
6. Catat IP address, subnet mask, dan gateway yang diterima.
7. Jalankan `show ip dhcp binding`.
8. Ping `192.168.10.1` dari kedua PC.
9. Tantangan: tambahkan PC ketiga dan lihat apakah alamat baru diberikan dari pool yang sama.

## 11. Mini Quiz

**1. Apa fungsi DHCP?**  
Jawaban: Memberikan konfigurasi jaringan seperti IP, subnet mask, gateway, dan DNS secara otomatis.

**2. Apa kepanjangan DORA?**  
Jawaban: Discover, Offer, Request, Acknowledge.

**3. Apa fungsi `ip dhcp excluded-address`?**  
Jawaban: Mengecualikan alamat agar tidak dibagikan oleh DHCP.

**4. Apa fungsi `default-router` dalam DHCP pool?**  
Jawaban: Memberikan default gateway kepada client.

**5. Perintah untuk melihat alamat yang sudah diberikan DHCP?**  
Jawaban: `show ip dhcp binding`.

## 12. Ringkasan
- DHCP memberikan konfigurasi IP secara otomatis.
- DORA berarti Discover, Offer, Request, Acknowledge.
- Router Cisco dapat bertindak sebagai DHCP server.
- `ip dhcp excluded-address` mengecualikan alamat tertentu.
- Gunakan `ip dhcp pool`, `network`, `default-router`, dan `dns-server` untuk konfigurasi.
- Gunakan `show ip dhcp pool`, `show ip dhcp binding`, `ipconfig`, dan `ping` untuk verifikasi.

## 13. Preview Day 11
Day 11 akan membahas **Access Control List (ACL) Basics**, yaitu cara router memfilter lalu lintas jaringan menggunakan aturan `permit` dan `deny`.
