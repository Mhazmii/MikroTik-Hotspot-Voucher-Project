# Topologi Jaringan

## 1. Gambaran Umum

Jaringan pada proyek ini menggunakan desain dua lapisan perangkat, yaitu **Core Router** dan **Dedicated Hotspot Router**.

Arsitektur ini dibuat untuk memisahkan fungsi jaringan utama dengan layanan pengguna hotspot.

Pembagian fungsi:

- RB760iGS bertanggung jawab terhadap koneksi internet dan jaringan inti.
- RB951Ui-2HnD bertanggung jawab terhadap jaringan pengguna hotspot dan layanan WiFi.

---

# 2. Topologi Fisik (Physical Topology)

Topologi fisik menunjukkan bagaimana perangkat saling terhubung menggunakan kabel jaringan.

```
                        Internet
                            |
                            |
                   Modem / ONT ISP
                     192.168.100.1
                            |
                            |
                    Kabel LAN UTP
                            |
                            |
                    ether1 (WAN)
                     MikroTik RB760iGS
                     Core Router
                            |
                ether5 (PoE Out + Data)
                            |
                            |
                    ether1 (UPLINK)
                  MikroTik RB951Ui-2HnD
                   Hotspot Router
                            |
            ---------------------------------
            |               |               |
          ether2          ether3          WLAN
        Client LAN      Client LAN    KEBON-HOTSPOT
```

---

## 3. Mapping Interface RB760iGS

RB760 berfungsi sebagai Core Router.

| Interface | Nama Konfigurasi | Fungsi |
|---|---|---|
| ether1 | WAN | Koneksi menuju modem ISP |
| ether2 | LAN1 | Management dan konfigurasi |
| ether3 | LAN2 | Cadangan / pengembangan |
| ether4 | LAN3 | Cadangan / pengembangan |
| ether5 | RB951 | Uplink dan PoE ke RB951 |
| sfp1 | SFP | Belum digunakan |

---

## 4. Mapping Interface RB951Ui-2HnD

RB951 berfungsi sebagai Hotspot Router dan Access Point.

| Interface | Nama Konfigurasi | Fungsi |
|---|---|---|
| ether1 | UPLINK | Koneksi ke RB760 |
| ether2 | LAN1 | Jaringan lokal hotspot |
| ether3 | LAN2 | Jaringan lokal hotspot |
| ether4 | LAN3 | Jaringan lokal hotspot |
| ether5 | LAN4 | Jaringan lokal hotspot |
| wlan1 | KEBON-HOTSPOT | Akses WiFi pengguna |

---

# 5. Topologi Logis (Logical Topology)

Topologi logis menggambarkan pembagian jaringan berdasarkan alamat IP dan fungsi masing-masing subnet.

```
                      Internet
                          |
                    192.168.100.0/24
                          |
                 RB760 WAN (DHCP Client)
                 192.168.100.xxx
                          |
                          |
                  Management Network
                    172.20.1.0/24
                          |
            --------------------------------
            |                              |
        RB760                           RB951
     172.20.1.1                      172.20.1.2
                                          |
                                          |
                                  Hotspot Network
                                  172.20.10.0/24
                                          |
                                          |
                                    Hotspot User
                                  172.20.10.xxx
```

---

# 6. Alur Perjalanan Data (Traffic Flow)

Berikut adalah alur paket data dari pengguna hotspot menuju internet:

```
Client WiFi
    |
    | 172.20.10.xxx
    |
RB951 Hotspot Router
Gateway: 172.20.10.1
    |
    | NAT ke jaringan management
    |
RB760 Core Router
Gateway: 172.20.1.1
    |
    | NAT ke jaringan ISP
    |
Modem ISP
192.168.100.1
    |
    |
Internet
```

Proses NAT terjadi pada dua titik:

### NAT Pertama

Dilakukan oleh RB951:

```
172.20.10.x
↓
172.20.1.2
```

Fungsi:

- Memisahkan jaringan hotspot dengan jaringan management.
- Menjadikan RB951 sebagai gateway khusus pengguna hotspot.

---

### NAT Kedua

Dilakukan oleh RB760:

```
172.20.1.x
↓
192.168.100.x
```

Fungsi:

- Menghubungkan jaringan lokal ke modem ISP.
- Mengizinkan seluruh jaringan internal mengakses internet.

---

# 7. Segmentasi Jaringan

Jaringan dibagi menjadi beberapa bagian untuk memudahkan pengelolaan dan pengembangan.

## WAN Network

Digunakan untuk komunikasi dengan ISP.

```
192.168.100.0/24
```

---

## Management Network

Digunakan untuk mengakses perangkat jaringan.

```
172.20.1.0/24
```

Perangkat:

- RB760: 172.20.1.1
- RB951: 172.20.1.2

---

## Hotspot User Network

Digunakan khusus untuk pengguna WiFi.

```
172.20.10.0/24
```

Gateway:

```
172.20.10.1
```

DHCP Pool:

```
172.20.10.100 - 172.20.10.254
```

---

# 8. Alasan Pemisahan Jaringan

Pemisahan jaringan dilakukan untuk mendapatkan beberapa keuntungan:

- Konfigurasi lebih terstruktur.
- Memudahkan proses troubleshooting.
- Memisahkan akses administrator dengan pengguna hotspot.
- Memudahkan penerapan firewall.
- Mempermudah pengembangan VLAN di masa depan.
- Mengurangi risiko gangguan dari jaringan pengguna.

---

# 9. Rencana Pengembangan Topologi

Topologi saat ini masih dapat dikembangkan menjadi:

- Penambahan Access Point tambahan.
- Implementasi VLAN Management.
- VLAN Hotspot.
- VLAN CCTV.
- Server lokal.
- Sistem monitoring jaringan.
- Integrasi dengan Mikhmon.
- Penerapan firewall berlapis.

---

# Kesimpulan

Topologi yang digunakan dalam proyek ini menerapkan konsep pemisahan antara Core Router dan Hotspot Router.

RB760iGS difokuskan sebagai pusat pengelolaan jaringan dan akses internet, sedangkan RB951Ui-2HnD menangani layanan pengguna seperti WiFi, Captive Portal, dan sistem voucher.

Desain ini memberikan struktur jaringan yang lebih mudah dikelola dan menjadi pondasi untuk pengembangan jaringan pada skala yang lebih besar.
