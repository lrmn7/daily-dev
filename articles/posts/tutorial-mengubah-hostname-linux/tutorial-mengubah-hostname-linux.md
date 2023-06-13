**Last updated**: April 6, 2020


### Apa itu Hostname ?

Hostname adalah sebuah nama unik yang diberikan kepada sebuah perangkat di jaringan, seperti komputer atau server, sehingga perangkat tersebut dapat dikenali dan diakses oleh perangkat lain di jaringan. Hostname sering digunakan sebagai bagian dari alamat IP dan membantu pengguna dan sistem untuk mengidentifikasi perangkat di jaringan. 

> Dalam konteks jaringan, hostname juga berfungsi sebagai pengganti dari alamat IP, yang sulit untuk diingat oleh manusia. Dengan menggunakan hostname, pengguna dapat dengan mudah mengingat nama perangkat dan menggunakannya untuk terhubung ke jaringan. Penting untuk memahami konsep dasar hostname sebelum membahas cara mengubahnya.

### Alasan untuk mengubah Hostname 

Ada beberapa alasan mengapa seseorang ingin mengubah hostname pada perangkat di jaringan. Salah satu alasan utama adalah untuk meningkatkan keamanan. Dalam lingkungan bisnis, misalnya, perangkat mungkin perlu diberi nama yang sesuai dengan kebijakan keamanan organisasi. 

Selain itu, menghindari tabrakan nama pada jaringan juga merupakan alasan umum untuk mengubah hostname. Jika dua perangkat di jaringan memiliki nama yang sama, hal ini dapat menyebabkan konflik dan masalah koneksi jaringan. 

Terakhir, masalah kinerja jaringan dapat menjadi alasan untuk mengubah hostname. Sebuah hostname yang terlalu panjang atau kompleks dapat memperlambat kinerja jaringan, sehingga mengubah hostname menjadi lebih sederhana dapat membantu memperbaiki kinerja jaringan. 

Sebelum mengubah hostname, penting untuk mempertimbangkan alasan di balik perubahan ini dan mengikuti langkah-langkah yang tepat untuk memastikan keamanan dan kelancaran operasi perangkat di jaringan.

#### Persiapan sebelum mengubah Hostname
Sebelum mengubah hostname pada perangkat di jaringan, ada beberapa persiapan yang perlu dilakukan.

* Pertama, backup data yang ada pada perangkat. Meskipun mengubah hostname biasanya tidak akan menyebabkan kehilangan data, namun selalu ada kemungkinan kesalahan dalam proses ini. 
* Kedua, catat nama hostname lama. Hal ini berguna jika ada masalah selama proses perubahan, sehingga dapat dengan mudah mengembalikan ke nama hostname sebelumnya. 
* Ketiga, persiapkan nama hostname baru dan pastikan nama yang dipilih unik dan sesuai dengan kebijakan jaringan. 
* Terakhir, pastikan bahwa perubahan hostname tidak akan memengaruhi konfigurasi jaringan atau aplikasi yang berjalan pada perangkat. Jika perlu, lakukan tes terlebih dahulu untuk memastikan bahwa perangkat masih berfungsi dengan baik setelah perubahan hostname. 

Dengan melakukan persiapan yang tepat sebelum mengubah hostname, kita dapat meminimalkan risiko dan memastikan bahwa perangkat akan beroperasi dengan lancar setelah perubahan.

### Cara Mengubah Hostname pada Linux Tanpa Restart

Untuk mengganti hostname di Linux tanpa restart, Anda dapat mengikuti langkah-langkah berikut:

1. Pertama, buka terminal atau console shell di Linux Anda.

2. Untuk melihat nama hostname saat ini, jalankan perintah berikut:
```bash
hostname
```

3. Untuk mengubah nama hostname, jalankan perintah berikut (menggantikan "new-hostname" dengan nama hostname yang diinginkan):
```bash
sudo hostnamectl set-hostname new-hostname
```
Anda akan diminta untuk memasukkan kata sandi root atau sudo untuk menyelesaikan perintah.

4. Setelah perintah di atas dijalankan, perbarui file /etc/hosts dengan menambahkan nama hostname baru dan IP address yang terkait dengan hostname tersebut.
```bash
sudo nano /etc/hosts
```

5. Tambahkan baris berikut di bawah baris dengan "127.0.0.1 localhost":
```bash
127.0.0.1 new-hostname
```
6. Setelah Anda menyimpan perubahan pada file /etc/hosts, Anda dapat memeriksa apakah nama hostname baru sudah diaktifkan dengan menjalankan perintah:
```bash
hostname
```

> Perhatikan bahwa perubahan ini mungkin tidak langsung berlaku untuk sesi shell yang sudah ada sebelum perubahan di atas, jadi Anda mungkin perlu membuka sesi shell baru untuk melihat perubahan yang baru saja dibuat.
