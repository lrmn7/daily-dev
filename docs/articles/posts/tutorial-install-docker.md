**Last updated**: December 10, 2020


### Pengertian Docker

Docker adalah platform perangkat lunak yang dapat digunakan untuk mengembangkan, menyimpan, dan menjalankan aplikasi dalam lingkungan yang dapat ditetapkan secara independen. Docker menggunakan teknologi container, yang memungkinkan aplikasi untuk berjalan dalam lingkungan yang terisolasi dari sistem operasi host yang digunakan. Ini membuatnya mudah untuk mengembangkan, mengirim, dan menjalankan aplikasi di berbagai platform, termasuk di server, desktop, dan cloud.

#### Apa itu Container ?

Docker container adalah sebuah wadah atau kontainer yang menjadi tempat untuk menjalankan aplikasi. Docker container dapat dianggap sebagai sebuah lingkungan atau runtime untuk menjalankan aplikasi yang terisolasi dari lingkungan yang lain. Dengan menggunakan docker container, Anda dapat dengan mudah mengembangkan, menjalankan, dan mengelola aplikasi Anda dengan cara yang lebih efisien dan terstruktur.

#### Perbedaan antara Docker Container dan Virtual Machine

<img src="docs/assets/images/Container-VS-Virtual-Machine.PNG" alt="Docker VS Virtual Machine">

Docker container dan virtual machine adalah dua teknologi yang berbeda yang digunakan untuk mengelola aplikasi dan infrastruktur. _Secara sederhana, perbedaan utama antara kedua teknologi ini adalah bagaimana mereka mengelola sumber daya komputer_. Docker container menggunakan cara yang lebih efisien untuk mengelola sumber daya komputer daripada virtual machine.

 >Docker container adalah sebuah lingkungan yang terisolasi di dalam host yang menjalankan sistem operasi. Container ini berisi aplikasi dan semua komponen yang dibutuhkan untuk menjalankan aplikasi tersebut, termasuk sistem operasi, library, dan file konfigurasi. 
 
 Container ini dapat dengan mudah dijalankan di berbagai host yang menjalankan sistem operasi yang sama, sehingga memungkinkan untuk deployment yang cepat dan mudah.

> Virtual machine, di sisi lain, adalah sebuah lingkungan yang terisolasi yang berjalan di atas host yang menjalankan sistem operasi. Virtual machine berisi sebuah sistem operasi lengkap yang berjalan di atas host, sehingga membutuhkan lebih banyak sumber daya komputer daripada container. 

Virtual machine juga membutuhkan waktu yang lebih lama untuk di-deploy dan lebih sulit untuk dikelola daripada container.

#### Kelebihan Docker Container

Secara umum, Docker container lebih efisien daripada virtual machine karena menggunakan sumber daya komputer dengan lebih efisien dan memudahkan deployment dan pengelolaan aplikasi. Namun, virtual machine masih bisa berguna dalam kasus-kasus tertentu, seperti ketika Anda ingin menjalankan sistem operasi yang berbeda di host yang sama atau ketika Anda membutuhkan lingkungan yang benar-benar terisolasi dari host.

### Langkah / Tutorial Install Docker di Linux Ubuntu

1. Mulailah dengan memperbarui paket ke versi terbaru yang tersedia.
```bash
lrmn7@ubuntu:~$ sudo apt update
lrmn7@ubuntu:~$ sudo apt upgrade
```

2. Instal beberapa paket yang memungkinkan Anda untuk menggunakan paket melalui HTTPS.
```bash
lrmn7@ubuntu:~$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

3. Tambahkan GPG key Docker repository.
```bash
lrmn7@ubuntu:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

4. Sekarang tambahkan Docker repository dari Ubuntu 22.04 (jammy) ke apt sources.
```bash
lrmn7@ubuntu:~$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5. Perbarui indeks paket dan siapkan server Anda untuk menginstal Docker dari repo Docker resmi.
```bash
lrmn7@ubuntu:~$ sudo apt update
lrmn7@ubuntu:~$ sudo apt-cache policy docker-ce
```

6. Anda akan menerima output yang mirip dengan ini.
```bash
Output
docker-ce:
  Installed: (none)
  Candidate: 5:20.10.14~3-0~ubuntu-jammy
  Version table:
     5:20.10.14~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
     5:20.10.13~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
```

7. Install docker dengan mengetikan perintah dibawah ini.
```bash
lrmn7@ubuntu:~$ sudo apt install docker-ce
```

8. Cek status apakah docker sudah running atau belum 
```bash
lrmn7@ubuntu:~$ sudo systemctl status docker
```

#### Cara agar Docker dapat dijalankan tanpa perlu Sudo

Perintah docker hanya dapat dijalankan sebagai pengguna `root` secara default. Jika Anda perlu menjalankan perintah docker tanpa sudo, Anda perlu menambahkan nama pengguna Anda ke grup `docker`.
```bash
lrmn7@ubuntu:~$ sudo usermod -aG docker nama_username_kamu
```

Sekarang kalian dapat melakukan perintah docker tanpa perlu mengetikan `sudo` di awal baris perintah.

#### Docker Cheat Sheet

Berikut ini beberapa command yang sering digunakan dalam penggunana docker.

##### 1. Melihat docker image<br>
```bash
docker image ls
```

##### 2. Download docker image<br>
(Jika tanpa :tag maka akan download image yang terbaru.)
```bash
docker image pull namaimage:tag 
```

##### 3. Menghapus docker image<br>
(Docker image tidak bisa dihapus jika masih ada docker container yg masih berjalan.)
```bash
docker image rm namaimage:tag
```

##### 4. Docker container<br>
Jika docker image sebagai installer, maka docker container sebagai aplikasi hasil installernya.
1 docker image bisa digunakan menjadi beberapa docker container, asalkan namanya berbeda.

##### 5. Status container<br>
Saat membuat container, secara default container tidak akan berjalan.
Jika tidak dijalankan maka container tidak akan jalan.

##### 6. Melihat container<br>
Melihat container yg sedang berjalan / tidak pakai:
```bash
docker container ls -a
```
Jika melihat container yg berjalan saja pakai:
```bash
docker container ls
```

##### 7. Membuat container<br>
```bash
docker container create --name namacontainer namaimage:tag
```

##### 8. Menjalankan container
Dapat menjalankan/mebuat conta<br>iner dengan port yang sama karena tiap container terisolasi satu dan yg lainnya.
```bash
docker container start containerid/namacontainer
```
 
##### 9. Menghentikan container<br>
Sebelum menghapus container, container harus dihentikan dahulu, caranya:
```bash
docker container stop containerid/namacontainer
```

##### 10. Menghapus container<br>
Kita tidak bisa menghapus image, jika containernya masih ada/sedang berjalan.
```bash
docker container rm containerid/namacontainer
```

##### 11. Melihat container logs<br>
Melihat log aplikasi dicontainer pakai ini:
```bash
docker container logs containerid/namacontainer
```
Melihat log secara realtime/menunggu log baru,  pakai:
```bash
docker container logs -f containerid/namacontainer
```

##### 12. Container exec<br>
Saat membuat container,aplikasi yg ada di container hanya bisa diakses dari dalam container.
Oleh karena itu kita perlu masuk ke dalamcontainer itu sendiri.
Untuk masuk ke dalam container,kita menggunakan fitur container exec,
dimana digunakan untuk mengeksekusi kode program yg ada di dalam container.

##### 13. Masuk ke container<br>
Untuk masuk ke dalam container, kita bisa mencoba mengeksekusi program bash script yang ada di dalam container
dengan bantuan container exec. Caranya:
```bash
docker container exec -i -t containerid/namacontainer /bin/bash
```
  * -i : argument interaktif, menjaga input tetap aktif
  * -t : argument untuk alokasi terminal akses

##### 14. Container port<br>
Saat menjalankan container, container terisolasi didalam docker.
Artinya sistem host (laptop kita) tidak bisa mengakses aplikasi didalam container secara langsung,
salah catu caranya adalah dengan menggunakan exec.
Biasanya aplikasi berjalan diport tertentu, contohnya:
saat menjalankan redis dia berjalan di port 6379.

##### 15. Port forwarding<br>
Berfungsi untuk meneruskan port yang ada disistem host (laptop kita)
ke dalam docker container. 
Cara ini cocok jika ingin mengekspos port yang ada di container ke luar
sistem hostnya.

##### 16. Melakukan port forwarding<br>
Untuk melakukan port forwarding bisa menggunakan perintah dibawah ini.
(jika sebelumnya container sudah pernah dibuat, hapus dulu buat baru dengan cara dibawah).
```bash
docker container create --name namacontainer --publish porthost:portcontainer image:tag
```
Jika ingin melakukan port forwarding lebih dari 1 bisa menambahkan 2x parameter --publish

##### 17. Container environment variabel<br>
Saat membuat aplikasi, menggunakan environment variabel adalah salah satu cara agar
konfigurasi aplikasi bisa diubah secara dinamis.
Dengan menggunakan environment variabel kita bisa mengubah-ubah konfig aplikasi
tanpa harus mengubah kode aplikasinya lagi.
Docker container memiliki parameter yg bisa kita gunakan untuk mengirim
environment variabel ke aplikasi yang terdapat di dalam container.

##### 18. Menambah environment variabel<br>
Untuk menambah environment variabel, bisa dengan perintah --env atau e
```bash
docker container create --name namacontainer --env KEY="value" --env KEY2="value" image:tag
```
--env: bisa digunakan banyak sesuai kebutuhan

##### 19. Container stats<br>
Untuk melihat detail penggunaan resource tiap container, container harus jalan untuk dilihat statsnya
```bash
docker container stats
```

##### 20. Container resource limit<br>
Secara default docker akan menggunakan semua CPU dan Memory yang diberikan ke Docker.
Dan akan menggunakan semua CPU dan Memory yang tesedia di sistem host (Linux).
Jika penggunaan terlalu banyak memakan CPU dan Memory maka bisa berdampak ke container lainnya.

##### 21. Memory<br>
Kita bisa menentukan memory yang bisa digunakan container.
Perintahnya --memory diikuti angka memory yang diperbolehkan untuk digunakan.
Bisa menggunakan b (bytes),k (kilo bytes),m (mega bytes) atau g (giga bytes).
Misal 100m artinya 100 mega bytes.

##### 22. CPU<br>
Kita bisa menentukan jumlah cpu yang digunakan container.
Perintahnya --cpus
Misal kita set 1.5 artinya bisa menggunakan satu dan setengah cpu core.

##### 23. Bind mounts<br>
Kemampuan melakukan sharing file ataupun folder yang terdapat di sistem host ke container.
Fitur ini sangat berguna ketika ingin mengirimkan kkonfigurasi dari luar container.
Misal menyimpan data yg dibuat di aplikasi ke dalam container ke dalam folder di sisten host.
Jika file/folder tidak ada di sistem host secara otomatis akan dibuatkan oleh docker.
Perintahnya menggunakan parameter --mount ketika membuat container, dan ada parameternya.
type: tipe mount, bind atau volume
source: lokasi file/folder di sistem host
destination: lokasi file/folder di container
readonly: jika ada maka file/folder hanya bisa di baca d container,tidak bisa ditulis



