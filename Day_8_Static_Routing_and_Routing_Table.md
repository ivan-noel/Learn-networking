# Day 8 — Static Routing and Routing Table

## 1. Tujuan
- Memahami routing dan routing table.
- Membedakan connected route dan static route.
- Memahami destination network dan next-hop.
- Mengonfigurasi static route dan menguji koneksi.

## 2. Recall Day 7
Day 7 membahas Inter-VLAN Routing. Kali ini kita menghubungkan dua LAN melalui dua router:

```text
PC0 — SW0 — R1 — R2 — SW1 — PC1
```

## 3. Routing dan Routing Table
Routing adalah proses menentukan jalur packet menuju network tujuan. Routing table menyimpan informasi jalur yang diketahui router.

Lihat routing table:
```text
show ip route
```

Kode penting:
- `C` = Connected route (network terhubung langsung)
- `L` = Local route (IP interface router sendiri)
- `S` = Static route (dikonfigurasi manual)

## 4. Static Route
Static route dimasukkan manual oleh administrator.

Format:
```text
ip route <destination-network> <subnet-mask> <next-hop>
```

Contoh:
```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

Artinya: untuk menuju `192.168.20.0/24`, teruskan packet ke next-hop `10.0.0.2`.

- **Destination network**: network yang ingin dituju.
- **Subnet mask**: mask network tujuan.
- **Next-hop**: alamat IP router berikutnya.

## 5. Topologi Packet Tracer
Tempatkan dua PC, dua switch, dan dua router. Hubungkan sesuai diagram:

```text
PC0 — SW0 — R1 G0/0
               R1 G0/1 — R2 G0/1
                            R2 G0/0 — SW1 — PC1
```

Nama interface dapat berbeda menurut model router. Sesuaikan dengan interface yang tersedia.

## 6. IP Plan

| Perangkat | Interface | IP address | Subnet mask | Gateway |
|---|---|---|---|---|
| PC0 | NIC | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| R1 | G0/0 | `192.168.10.1` | `255.255.255.0` | — |
| R1 | G0/1 | `10.0.0.1` | `255.255.255.252` | — |
| R2 | G0/1 | `10.0.0.2` | `255.255.255.252` | — |
| R2 | G0/0 | `192.168.20.1` | `255.255.255.0` | — |
| PC1 | NIC | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` |

LAN A: `192.168.10.0/24`  
Link R1–R2: `10.0.0.0/30`  
LAN B: `192.168.20.0/24`

## 7. Konfigurasi PC
Buka **PC → Desktop → IP Configuration**.

PC0:
```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

PC1:
```text
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

## 8. Konfigurasi R1
```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
interface gigabitEthernet 0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
end
```

## 9. Konfigurasi R2
```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
interface gigabitEthernet 0/1
ip address 10.0.0.2 255.255.255.252
no shutdown
exit
end
```

## 10. Periksa Interface dan Routing Table
Pada kedua router:
```text
show ip interface brief
show ip route
```

Interface yang dipakai harus `up/up`. Awalnya R1 mengetahui LAN A dan link R1–R2, tetapi belum mengetahui LAN B. R2 mengetahui LAN B dan link R1–R2, tetapi belum mengetahui LAN A.

## 11. Static Route pada R1
Agar R1 dapat mencapai LAN B:
```text
enable
configure terminal
ip route 192.168.20.0 255.255.255.0 10.0.0.2
end
```

## 12. Static Route pada R2
Agar R2 dapat mencapai LAN A dan mengirim packet balasan:
```text
enable
configure terminal
ip route 192.168.10.0 255.255.255.0 10.0.0.1
end
```

## 13. Verifikasi
Pada R1:
```text
show ip route
```
Cari static route menuju `192.168.20.0/24` melalui `10.0.0.2`.

Pada R2, cari static route menuju `192.168.10.0/24` melalui `10.0.0.1`. Static route biasanya ditandai `S`.

## 14. Uji Koneksi
Dari PC0:
```text
ping 192.168.10.1
ping 10.0.0.2
ping 192.168.20.10
```

Dari PC1:
```text
ping 192.168.20.1
ping 10.0.0.1
ping 192.168.10.10
```

Untuk melihat jalur dari PC0:
```text
tracert 192.168.20.10
```

Jalur logisnya: PC0 → R1 → R2 → PC1. Ping pertama kadang gagal saat ARP baru dipelajari; coba ulangi.

## 15. Troubleshooting
Jika ping gagal, periksa:
1. IP address, subnet mask, dan default gateway PC.
2. `show ip interface brief`; interface harus `up/up`.
3. Koneksi R1–R2: dari R1 coba `ping 10.0.0.2`, dan dari R2 coba `ping 10.0.0.1`.
4. `show ip route` pada kedua router.
5. Destination, mask, dan next-hop pada static route.
6. Kabel dan port yang digunakan.

## 16. Menghapus Static Route
Pada R1:
```text
no ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

Pada R2:
```text
no ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

## 17. Perintah Penting

| Command | Fungsi |
|---|---|
| `show ip route` | Melihat routing table |
| `show ip interface brief` | Melihat status interface |
| `ip route` | Membuat static route |
| `no ip route` | Menghapus static route |
| `ping` | Menguji konektivitas |
| `tracert` | Melihat jalur packet dari PC |

## 18. Latihan
1. Tuliskan command static route pada R1 agar menuju `192.168.20.0/24` melalui `10.0.0.2`.
2. Tuliskan route yang dibutuhkan R2 untuk mencapai `192.168.10.0/24`.
3. Jelaskan arti command `ip route 192.168.10.0 255.255.255.0 10.0.0.1`.
4. Jika R1 dan R2 bisa saling ping tetapi PC0 gagal ping PC1, sebutkan dua hal yang harus diperiksa.

## 19. Mini Quiz

**1. Apa fungsi routing?**  
A. Membuat VLAN · B. Menentukan jalur packet · C. Mengubah MAC address · D. Mengganti kabel  
**Jawaban: B**

**2. Command untuk melihat routing table adalah:**  
A. `show vlan brief` · B. `show ip route` · C. `show running vlan` · D. `show mac address`  
**Jawaban: B**

**3. Kode static route adalah:**  
A. `C` · B. `L` · C. `S` · D. `V`  
**Jawaban: C**

**4. Apa itu next-hop?**  
A. PC tujuan · B. Router berikutnya untuk meneruskan packet · C. Nama VLAN · D. Subnet mask  
**Jawaban: B**

**5. Mengapa kedua router membutuhkan route?**  
**Jawaban:** Agar packet menuju tujuan dan packet balasan memiliki jalur kembali.

## 20. Ringkasan
Materi hari ini:
- Routing dan routing table.
- Connected route dan static route.
- Destination network dan next-hop.
- `ip route` dan `show ip route`.
- Pengujian menggunakan `ping` dan `tracert`.
- Troubleshooting static routing.

Pola utama:
```text
Destination network → Next-hop
192.168.20.0/24 → 10.0.0.2
```

Command:
```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## 21. Preview Day 9
Day 9 akan memperkenalkan **Dynamic Routing**, routing protocol, perbedaan static dan dynamic routing, pengenalan RIP, serta pengujian dan troubleshooting.
