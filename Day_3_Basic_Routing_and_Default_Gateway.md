# Cisco Networking Learning — Day 3

> Learning Cisco Networking from 0  
> Day 3 — Basic Routing and Default Gateway

---

## 1. Tujuan Day 3

Pada Day 3 kita mulai mengenal **Router**.

Day 2 menggunakan satu network:

```text
PC 1 -------- Switch -------- PC 2
```

Hari ini kita menggunakan dua network berbeda:

```text
PC 1 --- Switch 1 --- Router --- Switch 2 --- PC 2
```

Target:
- Mengenal Router dan interface
- Memberikan IP Address pada interface Router
- Memahami Default Gateway
- Menggunakan Router CLI
- Mengaktifkan interface
- Menghubungkan dua network
- Melakukan ping antar-network
- Memahami routing dasar

---

## 2. Mengapa Membutuhkan Router?

Misalnya terdapat:

```text
Network A
192.168.1.0/24
```

dan:

```text
Network B
192.168.2.0/24
```

Switch saja tidak digunakan untuk melakukan routing antar dua network tersebut.

Kita membutuhkan router:

```text
Network A
192.168.1.0/24
       |
    Router
       |
Network B
192.168.2.0/24
```

Router meneruskan packet dari satu network ke network lainnya.

---

## 3. Topology

```text
PC 1 -------- Switch 1 -------- Router -------- Switch 2 -------- PC 2
```

Alamat yang digunakan:

```text
PC 1:              192.168.1.10
Router interface 1: 192.168.1.1

Router interface 2: 192.168.2.1
PC 2:              192.168.2.10
```

---

## 4. Perangkat

Gunakan:
- 2 PC
- 2 Switch
- 1 Router

Contoh router:

```text
1941
```

Pastikan router memiliki minimal dua interface Ethernet.

Gunakan **Copper Straight-Through** untuk koneksi:

```text
PC 1 → Switch 1
Switch 1 → Router
Router → Switch 2
Switch 2 → PC 2
```

---

## 5. Network yang Digunakan

### Network 1

```text
192.168.1.0/24
```

PC 1:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
```

### Network 2

```text
192.168.2.0/24
```

PC 2:

```text
IP Address: 192.168.2.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

---

## 6. Apa itu Default Gateway?

Default Gateway adalah alamat perangkat yang digunakan ketika host ingin mengirim traffic ke luar network lokalnya.

Contoh PC 1:

```text
PC 1
192.168.1.10
     |
     | Default Gateway
     ↓
192.168.1.1
     |
   Router
```

PC 2:

```text
PC 2
192.168.2.10
     |
     | Default Gateway
     ↓
192.168.2.1
     |
   Router
```

Sederhananya:

```text
Network lokal
     ↓
Default Gateway
     ↓
Router
     ↓
Network lain
```

---

## 7. Konfigurasi Router

Buka:

```text
Router → CLI
```

Masuk ke privileged mode:

```text
enable
```

Kemudian:

```text
configure terminal
```

Prompt menjadi:

```text
Router(config)#
```

---

## 8. Melihat Interface

Gunakan:

```text
show ip interface brief
```

Contoh:

```text
Interface              IP-Address      Status
GigabitEthernet0/0     unassigned      administratively down
GigabitEthernet0/1     unassigned      administratively down
```

> Nama interface dapat berbeda tergantung model router.

---

## 9. Konfigurasi Interface Pertama

Gunakan interface pertama untuk Network 1:

```text
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

`no shutdown` digunakan untuk mengaktifkan interface.

---

## 10. Konfigurasi Interface Kedua

Gunakan interface kedua untuk Network 2:

```text
interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
```

Router sekarang memiliki:

```text
GigabitEthernet0/0
192.168.1.1

GigabitEthernet0/1
192.168.2.1
```

---

## 11. Mengecek Interface

Gunakan:

```text
show ip interface brief
```

Idealnya interface menunjukkan:

```text
Status: up
Protocol: up
```

Jika muncul:

```text
administratively down
```

periksa apakah sudah menjalankan:

```text
no shutdown
```

---

## 12. Apakah Perlu Static Route?

Untuk topology sederhana ini, **belum perlu static route**.

Alasannya, kedua network langsung terhubung ke router:

```text
192.168.1.0/24
       |
192.168.1.1
       |
     Router
       |
192.168.2.1
       |
192.168.2.0/24
```

Router otomatis mengetahui network yang terhubung langsung.

Ini disebut **Connected Route**.

---

## 13. Melihat Routing Table

Gunakan:

```text
show ip route
```

Network yang terhubung langsung biasanya ditandai:

```text
C
```

`C` berarti:

```text
Connected
```

Contoh:

```text
C 192.168.1.0/24 is directly connected
C 192.168.2.0/24 is directly connected
```

---

## 14. Menguji Koneksi

Dari PC 1:

```bash
ping 192.168.1.1
```

Menguji:

```text
PC 1 → Router
```

Dari PC 2:

```bash
ping 192.168.2.1
```

Menguji:

```text
PC 2 → Router
```

---

## 15. Ping Antar-Network

Sekarang dari PC 1:

```bash
ping 192.168.2.10
```

Alurnya:

```text
PC 1
192.168.1.10
      |
      ↓
Switch 1
      |
      ↓
Router
      |
      ↓
Switch 2
      |
      ↓
PC 2
192.168.2.10
```

Jika berhasil, router sudah meneruskan traffic dari:

```text
192.168.1.0/24
```

ke:

```text
192.168.2.0/24
```

---

## 16. Bagaimana Packet Berjalan?

Jika PC 1 ingin mengakses PC 2:

```text
PC 1
192.168.1.10
     |
     | Destination: 192.168.2.10
     ↓
Switch 1
     |
     ↓
Router
     |
     | Routing
     ↓
Switch 2
     |
     ↓
PC 2
192.168.2.10
```

Karena PC 2 berada di network berbeda, PC 1 mengirim traffic melalui **Default Gateway**:

```text
192.168.1.1
```

Router kemudian meneruskan packet ke Network 2.

---

## 17. Perbedaan Day 2 dan Day 3

### Day 2

```text
192.168.1.0/24

PC 1 ---- Switch ---- PC 2
```

Satu network, sehingga tidak membutuhkan router.

### Day 3

```text
192.168.1.0/24       192.168.2.0/24
       |                     |
       +------ Router -------+
```

Dua network berbeda, sehingga router digunakan untuk menghubungkannya.

---

## 18. Konsep Default Gateway

Ingat:

```text
Tujuan satu network
        ↓
Komunikasi langsung melalui LAN

Tujuan network berbeda
        ↓
Default Gateway
        ↓
Router
        ↓
Network lain
```

Contoh PC 1:

```text
IP:
192.168.1.10

Gateway:
192.168.1.1
```

Tujuan:

```text
192.168.1.20
```

masih satu network.

Tetapi:

```text
192.168.2.10
```

berada di network berbeda, sehingga traffic menggunakan gateway:

```text
192.168.1.1
```

---

## 19. Command yang Dipelajari

| Command | Fungsi |
|---|---|
| `enable` | Masuk ke Privileged EXEC Mode |
| `configure terminal` | Masuk ke Global Configuration Mode |
| `interface gigabitEthernet 0/0` | Memilih interface |
| `ip address ...` | Memberikan IP Address |
| `no shutdown` | Mengaktifkan interface |
| `show ip interface brief` | Melihat status interface |
| `show ip route` | Melihat routing table |
| `ping` | Menguji konektivitas |

---

## 20. Troubleshooting

Jika PC 1 tidak dapat ping PC 2, periksa:

### IP Address

PC 1:

```text
192.168.1.10
```

PC 2:

```text
192.168.2.10
```

### Subnet Mask

Keduanya:

```text
255.255.255.0
```

### Default Gateway

PC 1:

```text
192.168.1.1
```

PC 2:

```text
192.168.2.1
```

### Interface Router

Gunakan:

```text
show ip interface brief
```

Pastikan interface:

```text
up
up
```

### Routing Table

Gunakan:

```text
show ip route
```

Pastikan terdapat:

```text
192.168.1.0/24
192.168.2.0/24
```

sebagai connected network.

---

## 21. Latihan Mandiri

Coba gunakan dua network baru:

```text
Network 1:
10.10.10.0/24

Network 2:
10.20.20.0/24
```

Gunakan:

```text
PC 1:
10.10.10.10

Router Interface 1:
10.10.10.1

Router Interface 2:
10.20.20.1

PC 2:
10.20.20.10
```

Default Gateway:

```text
PC 1 → 10.10.10.1
PC 2 → 10.20.20.1
```

Kemudian dari PC 1:

```bash
ping 10.20.20.10
```

---

## 22. Mini Quiz

1. Apa fungsi utama router?
2. Mengapa Day 3 membutuhkan router sedangkan Day 2 tidak?
3. Apa yang dimaksud Default Gateway?
4. Apa Default Gateway PC 1?
5. Apa Default Gateway PC 2?
6. Apa fungsi `no shutdown`?
7. Apa fungsi `show ip interface brief`?
8. Apa fungsi `show ip route`?
9. Apa arti kode `C` pada routing table?
10. Apa yang terjadi ketika PC 1 melakukan `ping 192.168.2.10`?

---

## 23. Day 3 Summary

Materi yang dipelajari:

- Router
- Dua network berbeda
- Interface Router
- IP Address pada interface
- Default Gateway
- Cisco Router CLI
- `enable`
- `configure terminal`
- `interface`
- `ip address`
- `no shutdown`
- `show ip interface brief`
- `show ip route`
- Connected Route
- Ping antar-network
- Troubleshooting dasar

Konsep utama:

```text
Network 1
192.168.1.0/24
      |
192.168.1.1
      |
    Router
      |
192.168.2.1
      |
Network 2
192.168.2.0/24
```

```text
PC → Switch → Router → Switch → PC
```

> **Goal:** Memahami bahwa router menghubungkan network yang berbeda, sedangkan Default Gateway menjadi jalan keluar host ketika tujuan berada di luar network lokal.

---

## Next Step — Day 4

Day 4 akan membahas **IP Address dan Subnetting Dasar**.

Target:

1. Memahami network address
2. Memahami host address
3. Memahami subnet mask
4. Memahami `/24`, `/25`, `/26`, dan lainnya
5. Menghitung jumlah host
6. Menentukan network address
7. Menentukan broadcast address
8. Menerapkan subnetting pada topology Packet Tracer

> **Goal:** Bisa melihat IP seperti `192.168.1.10/24` dan memahami bagian network serta host-nya.
