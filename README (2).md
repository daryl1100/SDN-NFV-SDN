# Implementasi Infrastruktur NFV + SDN Sederhana  
## Menggunakan Mininet, Open vSwitch, dan POX Controller

## Deskripsi

Project ini merupakan implementasi sederhana dari konsep **Network Function Virtualization (NFV)** yang diintegrasikan dengan **Software Defined Network (SDN)**.

Pada implementasi ini digunakan:

- **Mininet** sebagai simulator jaringan
- **Open vSwitch (OVS)** sebagai virtual switch
- **POX Controller** sebagai SDN Controller
- **iptables** sebagai Virtual Firewall (VNF)
- **Linux IP Forwarding** sebagai Virtual Router

Tujuan dari project ini adalah membangun jaringan virtual yang fleksibel, mudah dikontrol, dan dapat menunjukkan integrasi antara NFV dan SDN.

---

# Langkah-Langkah Pengerjaan

## 1. Update Sistem

```bash
sudo apt update
sudo apt upgrade -y
```

---

## 2. Install Mininet dan Open vSwitch

```bash
sudo apt install mininet openvswitch-switch -y
```

Cek instalasi:

```bash
sudo mn --test pingall
```

Jika berhasil berarti Mininet siap digunakan.

---

## 3. Install Git

```bash
sudo apt install git -y
```

---

## 4. Download POX Controller

```bash
cd ~
git clone https://github.com/noxrepo/pox.git
```

---

## 5. Jalankan POX Controller

Masuk ke folder POX:

```bash
cd ~/pox
```

Jalankan controller:

```bash
./pox.py forwarding.l2_learning
```

Jika berhasil akan muncul:

```text
POX 0.7.0 is up.
```

Artinya SDN Controller aktif.

---

## 6. Jalankan Mininet

Buka terminal baru lalu jalankan:

```bash
sudo mn --topo single,3 --controller remote
```

Artinya:

- 1 switch
- 3 host
- terhubung ke controller POX

---

## 7. Test Koneksi

Di Mininet:

```bash
pingall
```

Jika berhasil:

```text
0% dropped
```

Artinya komunikasi antar host sukses.

---

## 8. Implementasi NFV - Virtual Firewall

Gunakan iptables:

```bash
sudo iptables -A INPUT -p icmp -j DROP
```

Fungsi:

memblokir trafik ICMP (ping)

Ini merupakan contoh Virtual Network Function (VNF).

---

## 9. Implementasi NFV - Virtual Router

Aktifkan IP Forwarding:

```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

Fungsi:

mengaktifkan routing pada Linux.

---

## 10. Monitoring SDN

### Melihat Open vSwitch

```bash
sudo ovs-vsctl show
```

### Melihat Flow Table

```bash
sudo ovs-ofctl dump-flows s1
```

Contoh output:

```text
icmp,in_port=1,nw_src=10.0.0.1,nw_dst=10.0.0.2 actions=output:2
```

Artinya paket dari Host1 ke Host2 diarahkan ke port 2 oleh controller.

Ini adalah bukti utama bahwa SDN berjalan.

---

# Pembuktian SDN

SDN berhasil jika terdapat:

## 1. Controller Aktif

```text
POX 0.7.0 is up
```

## 2. Ping Antar Host Berhasil

```text
0% dropped
```

## 3. Flow Table Muncul

```bash
sudo ovs-ofctl dump-flows s1
```

Muncul rule OpenFlow dari controller ke switch.

---


# Kesimpulan

Project ini berhasil mengintegrasikan:

## SDN

menggunakan:

- POX Controller
- Open vSwitch
- OpenFlow

dan

## NFV

menggunakan:

- Virtual Firewall
- Virtual Router

Hasilnya adalah infrastruktur jaringan virtual yang fleksibel, scalable, murah, dan mudah dikelola tanpa membutuhkan perangkat jaringan fisik.

---
