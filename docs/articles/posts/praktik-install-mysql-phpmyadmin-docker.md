**Last updated**: June 15, 2020


#### Menginstall MySQL dan phpMyAdmin
Untuk menginstall MySQL dan phpMyAdmin menggunakan Docker, Anda bisa menjalankan perintah berikut:
```bash
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=rootpassword -d mysql:5.7
$ docker run --name myadmin -d --link mysql:db -p 8383:80 phpmyadmin/phpmyadmin
```

Perintah pertama akan menjalankan container MySQL dengan nama `mysql` dan menetapkan password root sebagai `rootpassword`. Container akan dijalankan dalam mode background (detached mode) dengan menggunakan image MySQL versi 5.7.

Perintah kedua akan menjalankan container phpMyAdmin dengan nama `myadmin`, menghubungkan ke container MySQL yang sudah dijalankan sebelumnya, dan menjalankan webserver pada port 8383. Image yang digunakan adalah `phpmyadmin/phpmyadmin`.

Setelah menjalankan perintah di atas, Anda bisa mengakses phpMyAdmin di http://localhost:8383/. Anda bisa login dengan menggunakan username `root` dan password `rootpassword`.

#### Penjelasan mengenai "--link mysql:db"

Opsi `--link` pada perintah docker run digunakan untuk menghubungkan antar container Docker. Dalam contoh di atas, opsi `--link mysql:db` akan menghubungkan container `myadmin` ke container `mysql`.

Setelah terhubung, container `myadmin` bisa mengakses container `mysql` dengan menggunakan hostname `db`. Ini berguna ketika container `myadmin` (phpMyAdmin) perlu mengakses database di container `mysql`.

Contoh penggunaan opsi `--link` adalah ketika Anda ingin menjalankan aplikasi yang memerlukan database, dan Anda ingin menjalankan database terpisah dari aplikasi tersebut dalam container yang berbeda. Dengan menggunakan opsi --link, aplikasi bisa terhubung ke database dengan menggunakan hostname yang telah ditentukan.

Sebagai tambahan, opsi --link sudah tidak direkomendasikan lagi untuk digunakan dalam versi terbaru dari Docker. Penggunaan opsi --link bisa menyebabkan masalah keamanan dan ketergantungan antar container yang tidak perlu. Sebagai gantinya, Anda bisa menggunakan fitur Docker network untuk menghubungkan container, atau menggunakan fitur Docker Compose untuk mengelola beberapa container sekaligus.

#### Menghubungkan MySQL dan phpMyAdmin dengan Docker Network

Untuk menggunakan Docker network untuk menghubungkan container MySQL dan phpMyAdmin, Anda bisa menjalankan perintah berikut:
1. Buat Docker network baru dengan perintah 
```bash
docker network create mynetwork
```
2. Jalankan container MySQL dengan menambahkan opsi `--network mynetwork`.
```bash
docker run --name mysql -e MYSQL_ROOT_PASSWORD=rootpassword --network mynetwork -d mysql:5.7
```
3. Jalankan container phpMyAdmin dengan menambahkan opsi `--network mynetwork`.
```bash
docker run --name myadmin -d --network mynetwork -p 8383:80 phpmyadmin/phpmyadmin
```
Setelah menjalankan perintah di atas, kedua container akan terhubung dalam Docker network `mynetwork`. Container `myadmin` bisa mengakses container `mysql` dengan menggunakan hostname `mysql`.

#### Penggunaan Docker Volume agar data tetap aman

Sebagai tambahan, jika Anda ingin menambahkan volume untuk menyimpan data MySQL di luar container, Anda bisa menambahkan opsi `-v /my/own/datadir:/var/lib/mysql` pada perintah docker run untuk container MySQL. Ini akan menyimpan data MySQL di dalam folder `/my/own/datadir` di host Anda, sehingga data akan tetap tersimpan meskipun container dihapus atau dibongkar.
```bash
docker run --name mysql -e MYSQL_ROOT_PASSWORD=rootpassword --network mynetwork -v /my/own/datadir:/var/lib/mysql -d mysql:5.7
```
Setelah menjalankan perintah di atas, Anda bisa mengakses phpMyAdmin di http://localhost:8383/. Anda bisa login dengan menggunakan username `root` dan password `rootpassword`.

