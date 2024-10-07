# Prometheus and Grafana

Prometheus dan Grafana adalah alat yang sering digunakan bersama untuk monitoring dan visualisasi data dalam infrastruktur IT dan aplikasi. Prometheus adalah sistem monitoring dan alerting yang mengumpulkan data metrik dari aplikasi dan server dalam format time-series, menyimpannya, dan memungkinkan pengguna untuk membuat aturan alerting untuk mendeteksi kondisi tertentu, seperti penggunaan CPU yang tinggi. Di sisi lain, Grafana adalah platform visualisasi data yang memungkinkan pengguna untuk membuat dashboard interaktif yang menampilkan data dari berbagai sumber, termasuk Prometheus, dalam bentuk grafik dan tabel yang mudah dipahami. Dengan demikian, Prometheus fokus pada pengumpulan dan penyimpanan data metrik, sedangkan Grafana fokus pada visualisasi data tersebut, sehingga keduanya membentuk sistem monitoring yang komprehensif.

## Tech Stack
- [Prometheus](https://prometheus.io/) - Main Packages for monitoring system and time series database
- [Grafana](https://grafana.com/) - Main Packages for data visualization
- [Debian](https://www.debian.org/releases/bullseye/) - Main Operating System that are used in this project
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - For running virtual machine

## Installation Guide
This is installation guide for installing Prometheus and Grafana on Debian 11 (Bullseye)

### Prerequisite
update repository dengan perintah berikut
```
apt update
```
install beberapa paket berikut 
```
apt install -y apt-transport-https software-properties-common wget
```
*jika belum ada paket ini silahkan diinstall terlebih dahulu*
> ```
> apt install -y gnupg
> ```
import GPG key
```
mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | tee /etc/apt/keyrings/grafana.gpg > /dev/null
```
tambahkan repositori untuk stable version grafana
```
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | tee -a /etc/apt/sources.list.d/grafana.list
```
dan update repository lagi dengan perintah berikut
```
apt update
```

### Install and SetUp Grafana
install grafana dengan melakukan perintah berikut
```
apt install -y grafana
```

jalankan grafana server dengan melakukan perintah berikut
```
systemctl start grafana-server
```
verifikasi jika server berhsil jalan dengan melakukan perintah berikut
```
systemctl status grafana-server
```

untuk melihat dashboard grafana kalian bisa buka dashboardnya di
> ```
> http://localhost:3000
> ```
with this credential
>```
> Credential Grafana
> ...
> Username: admin
> Password: admin
>```

### Install and SetUp Prometheus
install prometheus dengan melakukan perintah berikut
```
apt install -y prometheus
```
enable prometheus service agar autostart saat system dinyalakan atau dijalankan
```
systemctl enable prometheus
```
jalankan prometheus dengan melakukan perintah berikut
```
systemctl start prometheus
```
verifikasi jika prometheus berhsil jalan dengan melakukan perintah berikut
```
systemctl status prometheus
```

untuk melihat dashboard prometheus kalian bisa buka dashboardnya di
> ```
> http://localhost:9090
> ```

### Install and SetUp Node-Exporter
install node-exporter dengan melakukan perintah berikut
```
apt install -y prometheus-node-exporter
```
enable node-exporter service agar autostart saat system dinyalakan atau dijalankan
```
systemctl enable prometheus-node-exporter
```
jalankan node-exporter dengan melakukan perintah berikut
```
systemctl start prometheus-node-exporter
```
verifikasi jika node-exporter berhsil jalan dengan melakukan perintah berikut
```
systemctl status prometheus-node-exporter
```

untuk melihat dashboard prometheus kalian bisa buka dashboardnya di
> ```
> http://localhost:9100/metrics
> ```

### Config Prometheus to Monitoring Node Clients
edit file berikut ***/etc/prometheus/prometheus.yml***
```
nano /etc/prometheus/prometheus.yml
```
tambahkan job baru di *scrape_configs*
> ```
> scrape_configs:
>   ...
>   - job_name: node
>     scrape_interval: 10s
>     static_configs:
>       - targets: ["localhost:9100"]
> ```
restart prometheus service
```
systemctl restart prometheus
```

<!-- ### Metode Lain
untuk mempermudah instalasi saya juga membuat beberapa cara untuk membuat phpipam dapat berjalan, antara lain adalah dengan menggunakan shell script dan juga menggunakan dockerfile yang bisa di unduh pada folder `method/` pada repository ini

---
untuk menggunakan metode shell script lakukan perintah berikut
```
chmod +x phpipam.sh
./phpipam.sh
```
dan untuk menggunakan metode docker lakukan perintah berikut
```
docker-compose up --build
``` -->
