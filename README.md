# Ringkasan Perintah Jaringan (Modul 1)

Berikut adalah ringkasan dari perintah-perintah jaringan dasar di Linux yang digunakan dalam Praktikum Jaringan Komputer Modul 1.

### 1. Konfigurasi IP & Interface Dasar
- `ip addr add [IP]/[Subnet] dev [interface]`
  - **Fungsi:** Mengatur/menambahkan alamat IP secara statis ke sebuah interface jaringan (misal: `eth0`).
- `ip link set [interface] up`
  - **Fungsi:** Mengaktifkan (menyalakan) interface jaringan agar bisa digunakan untuk mengirim dan menerima data.
- `ip -br addr`
  - **Fungsi:** Menampilkan ringkasan singkat dari seluruh interface jaringan di PC beserta alamat IP yang terpasang.

### 2. Pengecekan Konektivitas (Troubleshooting)
- `ping [IP Tujuan]`
  - **Fungsi:** Mengecek apakah suatu perangkat dapat dijangkau di jaringan menggunakan protokol ICMP (mengirim Echo Request dan menunggu Echo Reply).
- `mtr [IP Tujuan]`
  - **Fungsi:** Singkatan dari *My Traceroute*. Alat diagnostik yang menggabungkan fungsi `ping` dan `traceroute` secara real-time untuk melihat jalur (hop) dan persentase packet loss secara berkesinambungan.

### 3. Konfigurasi Ethernet Bridge
- `brctl addbr [nama_bridge]`
  - **Fungsi:** Membuat sebuah virtual bridge baru (contoh: `br0`). Bridge berfungsi mirip seperti switch virtual untuk menggabungkan beberapa interface.
- `brctl addif [nama_bridge] [interface]`
  - **Fungsi:** Memasukkan sebuah interface (misal `eth0`) ke dalam bridge yang sudah dibuat.

### 4. Simulasi Gangguan Jaringan (Traffic Control)
- `tc qdisc replace dev [interface] root netem loss [X]%`
  - **Fungsi:** Mensimulasikan kondisi packet loss (paket hilang di jalan) pada antarmuka jaringan sebesar persentase tertentu.
- `tc qdisc replace dev [interface] root tbf rate [X]mbit burst 64k limit 64k`
  - **Fungsi:** Membatasi kecepatan maksimum (throughput limit) dari sebuah antarmuka menggunakan algoritma Token Bucket Filter (TBF).
- `tc qdisc del dev [interface] root`
  - **Fungsi:** Menghapus seluruh aturan limitasi atau packet loss yang telah dipasang pada interface tersebut.

### 5. Pengukuran Kapasitas & Throughput
- `ethtool [interface]`
  - **Fungsi:** Melihat informasi teknis dari kartu jaringan, termasuk kapasitas maksimal teoritis (Speed) dari antarmuka tersebut (misal: 10000Mb/s).
- `iperf3 -s`
  - **Fungsi:** Menjalankan aplikasi iperf3 dalam mode Server (menunggu koneksi masuk untuk dites).
- `iperf3 -c [IP Server] -u -b 10G`
  - **Fungsi:** Menjalankan iperf3 dalam mode Client untuk melakukan *stress test* transfer data ke server. Argumen `-u` berarti menggunakan protokol UDP (lebih brutal dari TCP), dan `-b 10G` memaksa pengiriman dengan target kecepatan 10 Gbps.

### 6. Analisis Paket Data
- `termshark -i [interface]`
  - **Fungsi:** Menjalankan aplikasi packet sniffer/analyzer berbasis terminal (versi CLI dari Wireshark). Berguna untuk menangkap dan menganalisis traffic jaringan yang lewat (seperti melihat perbedaan antara Hub dan Switch).

### 7. DHCP (Materi Teori Tambahan)
- `udhcpd -f [file_konfigurasi]`
  - **Fungsi:** Menjalankan layanan DHCP Server untuk memberikan IP address secara otomatis kepada client.
- `udhcpc -i [interface]`
  - **Fungsi:** Menjalankan layanan DHCP Client secara manual untuk meminta IP address dari DHCP Server terdekat.
