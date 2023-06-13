**Last updated**: November 23, 2020


### Mengenal lebih dekat apa itu Node.js ?

Node.js adalah sebuah Javascript platform untuk general-purpose programming yang mengizinkan pengguna untuk membuat sebuah network applications secara cepat. Secara garis besar Javascript dapat digunakan pada sisi `front-end` (_Angular,React,Vue_) dan juga `back-end`, proses development dapat dilakukan dengan lebih konsisten dan didesain dengan mudah karena dibuat dengan sistem yang sama.

### Persyaratan install Node.js

Pada tutorial atau cara kali ini saya menggunakan Linux `Zorin Lite 12.4`, tidak usah khawatir jika kalian menggunakan Ubuntu karena Zorin berbasis Ubuntu dan cara instalasinya pun sama. Kemudian yang diperlukan adalah koneksi internet dan pemahaman dasar seputar linux.

#### Kelebihan menginstall Node.js dengan NVM

Ada alternatif lain sebenarnya dalam menginstall Node.js selain menggunakan `apt` yaitu dengan menggunakan `nvm`, singkatan dari "_Node.js version manager_". 
> Kelebihan menginstall menggunakan nvm adalah kamu bisa menginstall lebih dari satu (multiple), dan juga self-contained version dari Node.js yang tidak akan mempengaruhi sistem mu.

Nvm mengizinkan kamu untuk mengakses versi terbaru dari Node.js dan mengatur  versi rilis sebelumnya. Berbeda dengan apt-get yang hanya menginstal versi stabil, disini kamu bisa memilih versi apa saja yang kamu mau termasuk versi stabil dari Node.js.

### Tutorial cara install Node.js di linux dengan menggunakan NVM (LTS Version)

Sebelum memulai instalasi terlebih dari update packages terbaru dari Ubuntu kalian, silahkan ikuti perintah dibawah ini:

```bash
 $  sudo apt-get update
 $  sudo apt-get install build-essential libssl-dev
```

Langkah selanjutnya adalah dengan menginstall nvm, silakan ikuti caranya dibawah ini:

```bash
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
```

Sebelum masuk ke folder nvm, kita lihat terlebih dahulu apakah terdapat foldernya, kita dapat melihatnya dengan cara dibawah:

```bash
$ ls -la
```

```bash
Hasilnya akan terlihat seperti dibawah :

-rw-rw-r--  1 lrmn lrmn   77 Peb  8 19:31 .gitconfig
-rw-------  1 lrmn lrmn 1240 Peb 17 10:43 .ICEauthority
drwxrwxr-x  3 lrmn lrmn 4096 Peb  8 18:32 .local
drwxr-xr-x  2 lrmn lrmn 4096 Peb  8 18:32 Music
drwxrwxr-x  2 lrmn lrmn 4096 Peb 17 03:55 .nano
-rw-------  1 lrmn lrmn  210 Peb  8 19:41 .netrc
-rw-------  1 lrmn lrmn    0 Peb 16 21:47 .node_repl_history
drwxrwxr-x  8 lrmn lrmn 4096 Peb 17 03:59 .nvm
```

Apakah kalian melihat folder dengan nama **.nvm**, jika ia silahkan masuk ke folder tersebut, silahkan ketikan perintah

```bash
$ cd .nvm
```

<sub>**Note:** Pada posis ini kita dianggap sudah masuk ke folder nvm, ditandai dengan berubahnya **$** menjadi **~/.nvm$**. Silahkan ketikan perintah setelah tanda **$**</sub>

Kemudian lihat apakah ada script instalasinya dengan perintah *ls -la*, mungkin bisa sama bisa berbeda.

```bash
~/.nvm$ ls -la
```

```bash
Terdapat file instalasinya dengan nama install.sh 

-rw-rw-r--  1 lrmn lrmn    140 Peb 17 03:53 .dockerignore
-rw-rw-r--  1 lrmn lrmn    221 Peb 17 03:53 .editorconfig
drwxrwxr-x  8 lrmn lrmn   4096 Peb 17 03:56 .git
-rw-rw-r--  1 lrmn lrmn      9 Peb 17 03:53 .gitattributes
drwxrwxr-x  2 lrmn lrmn   4096 Peb 17 03:53 .github
-rw-rw-r--  1 lrmn lrmn    252 Peb 17 03:53 .gitignore
-rwxrwxr-x  1 lrmn lrmn  13226 Peb 17 03:53 install.sh
-rw-rw-r--  1 lrmn lrmn   1078 Peb 17 03:53 LICENSE.md
```

Pada tutorial kali ini terdapat *install.sh* silakan install dengan cara dibawah, **ingat!!!** silahkan ketikan perintah setelah tanda *$*

```bash
~/.nvm$ bash install.sh
```

<sub>**Note:** Mulai dari sini kebawah sebenarnya kalian tidak harus berada pada folder nvm, kita bisa lakukan dimana saja. Agar sama dengan tutorial silahkan ketikan **_cd .._** untuk kembali ke folder Home dan berubah menjadi **$**.</sub>

Proses instalasi akan berlangsung,setelah selesai langkah selanjutnya silahkan ikuti perintah

```bash
$ source ~/.profile
```

Sekarang nvm berhasil di install, nah untuk menginstall `Node.js` silakan kalian ketikan:

```bash
$ nvm ls-remote
```

```bash
Akan tampil seperti dibawah ini:

           v10.10.0
       v10.11.0
       v10.12.0
       v10.13.0   (LTS: Dubnium)
       v10.14.0   (LTS: Dubnium)
       v10.14.1   (LTS: Dubnium)
       v10.14.2   (LTS: Dubnium)
       v10.15.0   (LTS: Dubnium)
->     v10.15.1   (Latest LTS: Dubnium)
```

Dapat kamu lihat diatas, terdapat banyak versi baik versi stabil maupun versi terbaru dari LTS (Long Time Support). Pada tutorial kali ini saya akan menginstall versi v10.15.1 LTS. Silahkan ketikan:

```bash
$ nvm install v.10.15.1
```

Umumnya, nvm akan mengganti versi yang digunakan dengan yang baru saja di install. Untuk itu silahkan ketikan perintah dibawah agar kita ganti ke versi LTS.

```bash
$ nvm use v.10.15.1
```

#### Mengecek instalasi Node.js 
Kemudian untuk melihat apakah Node.js berhasil di install silahkan ketikan perintah:

```bash
$ node -v
```

```bash
Akan keluar:

v.10.15.1
```
#### Mengatur versi default Node.js yang akan digunakan 
Jika kalian memiliki atau menginstall lebih dari 1 versi Node.js, kalian bisa melihatnya dengan cara dibawah:

```bash
$ nvm ls
```

Silahkan ikuti perintah dibawah agar menjadikan versi LTS tadi menjadi versi default yang akan digunakan untuk project kedepannya:

```bash
$ nvm alias default v.10.15.1
```