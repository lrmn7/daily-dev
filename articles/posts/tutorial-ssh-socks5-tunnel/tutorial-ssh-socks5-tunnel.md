**Last updated**: December 8, 2020


Ada kalanya kalian ingin menjelajah Internet secara pribadi, mengakses konten yang dibatasi secara geografis, atau mem-bypass firewall.

Salah satu pilihannya adalah menggunakan VPN, tetapi itu memerlukan instalasi dan menyiapkan server VPN kalian sendiri atau berlangganan layanan VPN.

Alternatif yang lebih sederhana adalah merutekan lalu lintas jaringan lokal kalian dengan Socks proxy tunnel terenkripsi. Dengan cara ini, semua aplikasi kalian yang menggunakan proxy akan tersambung ke server SSH dan server akan meneruskan semua trafik ke tujuan sebenarnya. ISP (penyedia layanan internet) dan pihak ketiga lainnya tidak akan dapat memeriksa lalu lintas kalian dan memblokir akses ke situs web.

Tutorial ini akan memandu kalian mempersiapkan bagaimana cara SSH SOCKS Tunnel terenkripsi dengan menggunakan browser web Firefox.

### Prasyarat
Sebelumnya silahkan persiapkan terlebih dahulu beberapa hal yang harus dilakukan:
 * Server yang berjalan dengan Sistem Operasi Linux
 * Web browser (Firefox)
 * SSH Client (MobaXterm) 

Di case ini saya akan melakukan tunneling web server apache2 yang sudah di install di server linux ubuntu saya untuk nantinya dapat kita akses secara lokal di browser Firefox. Disini server saya buat dengan menggunakan Vagrant, tutorial dapat dilihat sebelumnya. Kemudian disini saya menggunakan SSH Client MobaXterm, karena sudah terdapat fitur ssh socks tunneling didalamnya.

#### Mempersiapkan web server apache2

1. Silahkan install terlebih dahulu, apache2 web server:
```bash
vagrant@ubuntu-jammy:~$ sudo apt install apache2
```
2. Silahkan cek, apakah apache2 web server sudah terinstall dan running. Ketikan perintah dibawah ini:
```bash
vagrant@ubuntu-jammy:~$ sudo systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2022-11-27 05:49:12 UTC; 4min 54s ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 2196 (apache2)
      Tasks: 55 (limit: 513)
     Memory: 5.4M
        CPU: 122ms
     CGroup: /system.slice/apache2.service
             ├─2196 /usr/sbin/apache2 -k start
             ├─2198 /usr/sbin/apache2 -k start
             └─2199 /usr/sbin/apache2 -k start
Nov 27 05:49:11 ubuntu-jammy systemd[1]: Starting The Apache HTTP Server...
Nov 27 05:49:12 ubuntu-jammy apachectl[2195]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'S>
Nov 27 05:49:12 ubuntu-jammy systemd[1]: Started The Apache HTTP Server.
lines 1-16/16 (END)
```
3. Disini, apache2 running di port 80.

#### Mempersiapkan SSH SOCKS Tunneling di MobaXterm

1. Silahkan buka MobaXterm, klik `Tunneling` di Menubar atas.
2. Silahkan klik `New SSH tunnel`
3. Pilih `Dynamic port forwarding (SOCKS proxy)`, silahkan isi seperti dibawah ini dan klik save.
<img src="/docs/assets/images/MobaXterm-Tunneling-Server-satu-Vagrant.PNG" alt="Dynamic port forwarding (SOCKS proxy)">
4. Kemudian klik tombol start.

#### Mempersiapkan proxy configuration di Firefox

1. Masuk setting Firefox.
2. Silahkan masuk ke `Network Settings`
3. Kemudian isi seperti dibawah ini:
<img src="/docs/assets/images/Proxy-Configuration-Firefox.PNG" alt="Proxy configuration Firefox">

### Akses apache2 Web server di Firefox

1. Silahkan akses ip web server kalian.
2. Karena apache2 web server running di port 80, silahkan akses ip_server:80. Di tutorial ini saya menggunakan Vagrant, dan ip server saya di `10.10.10.21`.
3. Dan apache2 sekarang sudah bisa di akses di local network kalian.
<img src="/docs/assets/images/Apache2-Webserver-Firefox.PNG" alt="Apache2 Web server Firefox">





