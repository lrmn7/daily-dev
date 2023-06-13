**Last updated**: October 8, 2020


### Apa itu SSH ?
SSH adalah singkatan dari Secure Shell, yaitu protokol jaringan yang digunakan untuk mengamankan koneksi jaringan antara dua perangkat yang terhubung melalui jaringan yang tidak aman, seperti internet. SSH memungkinkan pengguna untuk mengakses dan mengendalikan perangkat jarak jauh secara aman dengan melakukan enkripsi data yang dikirimkan antara kedua perangkat.

SSH banyak digunakan dalam dunia IT, terutama dalam mengakses dan mengelola server jarak jauh. Dalam konteks ini, SSH memungkinkan administrator server untuk melakukan tugas-tugas administratif pada server tanpa harus berada di lokasi fisik server tersebut. Dalam beberapa kasus, SSH juga digunakan untuk mengakses dan mengontrol perangkat lain seperti router dan switch jaringan.

### Bagaimana SSH bekerja ?

SSH bekerja dengan menggunakan teknologi kriptografi untuk melindungi komunikasi antara kedua perangkat yang terhubung melalui jaringan. Kriptografi digunakan untuk menyandikan pesan yang dikirim antara kedua perangkat sehingga hanya penerima yang dituju yang dapat membaca dan memahami isi pesan tersebut. Ini membuat komunikasi antara kedua perangkat menjadi lebih aman, karena data yang ditransmisikan tidak dapat diakses oleh pihak lain yang tidak berwenang.

Secara umum, proses kerja SSH adalah sebagai berikut:

  1. Klien SSH (biasanya program seperti PuTTY atau OpenSSH) meminta koneksi ke server SSH dengan menggunakan nama host dan port.

  2. Server SSH memberikan respon dengan mengirimkan sertifikat digital, yang berisi kunci publik dan informasi lainnya yang diperlukan untuk melakukan enkripsi dan dekripsi data.

  3. Klien SSH memverifikasi sertifikat digital tersebut untuk memastikan bahwa koneksi yang dibuat benar-benar ke server yang dimaksudkan.

  4. Setelah sertifikat digital telah diverifikasi, klien SSH dan server SSH saling menukar kunci enkripsi, yang akan digunakan untuk mengamankan koneksi antara kedua perangkat.

  5. Setelah kunci enkripsi berhasil ditukar, koneksi antara klien SSH dan server SSH dianggap aman, dan keduanya dapat memulai pertukaran data yang dienkripsi.

Dengan cara ini, SSH memungkinkan pengguna untuk mengakses dan mengendalikan perangkat jarak jauh dengan cara yang aman dan terenkripsi, sehingga meminimalkan risiko penyadapan atau pemalsuan data oleh pihak yang tidak berwenang.

### Port yang digunakan untuk SSH

Port default yang digunakan untuk koneksi SSH adalah Port 22. Namun, port SSH dapat dikonfigurasi untuk menggunakan port yang berbeda jika diinginkan. Konfigurasi ini dilakukan di server SSH dan/atau di klien SSH yang digunakan untuk mengakses server tersebut.

#### Informasi antar server

Untuk bisa terhubung ke server, kita bisa menggunakan beberapa aplikasi pihak ketiga contohnya seperti Putty, MobaXterm, Termius dan lain sebagainya. Untuk terhubung ke server menggunakan SSH, server harus terlebih dahulu dizinkan aksesnya ke port 22, karena SSH dapat berjalan menggunakan port tersebut.

Disini saya sudah menyiapkan 2 buah server yang saya buat dengan menggunakan Vagrant, tutorial bisa dicari diartikel sebelumnya. Berikut detail terkait 2 server tersebut:
  * Server 1:
    * username  : admin-node-1
    * hostname  : ubuntu-node-1
    * ip addres : 10.10.10.10
    * password  : node1
  * Server 2:
    * username  : admin-node-2
    * hostname  : ubuntu-node-2
    * ip addres : 10.10.10.20
    * password  : node2

> Perlu diingat, di dua sever diatas autentikasi public saya aktifkan agar bisa login antar server menggunakan password di `/etc/ssh/sshd_config` sebagai contoh pembelajaran.

#### Membuat SSH Keygen

Berikut adalah langkah-langkah untuk membuat SSH keygen agar bisa mengakses ke server:

1. Buka terminal atau command prompt di komputer Anda.

2. Ketik perintah berikut untuk memulai pembuatan kunci SSH keygen:
```bash
ssh-keygen -t rsa
```
Perintah ini akan membuat kunci SSH dengan algoritma enkripsi RSA.

3. Anda akan diminta untuk menentukan lokasi penyimpanan kunci SSH. Anda dapat menekan tombol Enter untuk menggunakan lokasi default atau memilih lokasi penyimpanan yang berbeda dengan mengetikkan lokasi yang diinginkan.

4. Kemudian, Anda akan diminta untuk memasukkan passphrase untuk kunci SSH. Passphrase adalah kata sandi yang digunakan untuk melindungi kunci SSH. Anda dapat memasukkan passphrase atau menekan tombol Enter untuk tidak menggunakan passphrase.

5. Kunci SSH publik dan pribadi akan dibuat di lokasi yang Anda pilih pada langkah ke-3. Kunci publik akan disimpan di file dengan ekstensi .pub dan kunci pribadi akan disimpan di file tanpa ekstensi.

#### Copy key ke server tujuan
1. Selanjutnya, kunci publik yang baru dibuat perlu disalin ke server SSH yang akan diakses. Anda dapat menggunakan perintah berikut untuk menyalin kunci publik ke server:
```bash
ssh-copy-id username@alamat_ip_server
```
2. Ganti username dengan nama pengguna Anda pada server dan alamat_ip_server dengan alamat IP server yang akan diakses.

3. Anda akan diminta untuk memasukkan kata sandi untuk akun pada server SSH. Setelah berhasil memasukkan kata sandi, kunci publik Anda akan disalin ke file authorized_keys pada server.

4. Jika berhasil maka akan tampil seperti ini:

```bash
admin-node-1@ubuntu-node-1:~$ ssh-copy-id admin-node-2@10.10.10.20
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/admin-node-1/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
admin-node-2@10.10.10.20's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'admin-node-2@10.10.10.20'"
and check to make sure that only the key(s) you wanted were added.

admin-node-1@ubuntu-node-1:~$
```

```bash
admin-node-1@ubuntu-node-1:~$ ssh admin-node-2@10.10.10.20
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-57-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Feb 15 22:39:19 UTC 2023

  System load:  0.0029296875      Processes:               111
  Usage of /:   4.8% of 38.70GB   Users logged in:         0
  Memory usage: 19%               IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%                IPv4 address for enp0s8: 10.10.10.20


44 updates can be applied immediately.
33 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Wed Feb 15 22:33:09 2023 from 10.10.10.10
admin-node-2@ubuntu-node-2:~$
```
#### Mengecek akses server tujuan
Untuk mengeceknya bisa dengan command dibawah ini:
```bash
admin-node-1@ubuntu-node-1:~$ ssh admin-node-2@10.10.10.20 "whoami;hostname"
admin-node-2
ubuntu-node-2
admin-node-1@ubuntu-node-1:~$
```
> Jika menampilkan user dan hostname, maka sebenarnya kita sudah bisa akses ssh ke server tanpa perlu password lagi atau passwordless.

