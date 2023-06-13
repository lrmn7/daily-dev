**Last updated**: August 19, 2020


### Apa itu Git dan kenapa developer harus menggunakan Git ?

Git bisa diibaratkan sebagai sebuah tombol `SAVE` yang sangat Epic untuk menyimpan file termasuk juga folder/direktori didalamnya. Secara resmi, _**Git** adalah sebuah version control system_.

Jika kita menggunakan **Teks Editor** maka akan menyimpan atau mencatat semua kata yang ada dalam dokumen sebagai sebuah file tunggal, anda hanya akan diberikan satu saja catatan file, contohnya jika menggunakan **Microsoft Word** seperti Essay.docx, kecuali jika kalian membuat salinan seperti dalam mengerjakan skripsi:

> Bab1-revisi1.docx, Bab2-revisi2.docx, Bab1-final.docx

Jika menyimpan dengan menggunakan Git maka akan menyimpan atau mencatat semua files dan folder dan juga menjaga mencatat history penyimpanan dalam setiap penyimpanan.

<sub>**Note:** Git bisa dikatakan/diibaratkan seperti **Checkpoint** dalam Game.</sub>

Sebagai seorang individual developer, Git dapat kamu gunakan untuk mereview bagaimana project berkembang dan dapat dengan mudah mengembalikan status file yang sudah tersimpan sebelumnya. Jika terhubung dengan internet, Git mengijinkan penggunanya untuk melakukan sharing dan kolaborasi dengan sesama developer seperti **Github**.

#### Perbedaan antara Git dan Teks Editor
  * Teks editor hanya bisa membuat dan menyimpan perubahan dalam satu buah file.
  * Git melacak dan mecatat semua pergantian pada file dari waktu ke waktu.

#### Perbedaan antara Git dan Github
  * Git bekerja pada level local atau kamu bisa menggunakannya tanpa perlu menggunakan internet. Semua perubahhan yang kamu buat tersimpan secara local (disimpan di komputer milikmu) dengan Git.

  * Sedangkan Github bekerja pada level remote. Kalian harus melakukan push (upload) semua perubahan yang ada pada level local (dengan menggunakan Git) ke Github.

> Jadi untuk bisa menggunakan Git harus menginstall terlebih dahulu, Git dapat kalian download di gitscm.com, sedangkan untuk bisa membuat repository ataupaun push ke Github kalian harus mendaftar pada websitenya di github.com

### Cara menginstall Git pada Linux & Windows
Jika menggunakan sistem operasi Linux, kalian dapat menjalankan perintah:
```bash
lrmn7@acer:~$ apt-get install git
```

Sedangkan, jika menggunakan Windows, caranya sangat mudah cukup download filenya dan jalankan installernya,klik next dan next sampai finish.

Untuk memastikan apakah git sudah terinstall, silahkan ketikan `git --version` pada terminal ataupun cmd
```bash
lrmn7@acer:~$ git --version
git version 2.7.4
```
### Konfigurasi Git dan Github pada Linux & Windows

#### Setup Git
Agar Git berfungsi dan berjalan dengan baik, kita perlu memberi tahu siapa kita sehingga dapat menautkan pengguna Git lokal (Anda) ke GitHub. Saat mengerjakan projek dengan sebuah tim, ini berguna untuk memberitahukan info pada oranglain/anggota tim untuk melihat apa saja yang telah Anda _commit_ dan perubahann setiap baris kode.

Perintah di bawah ini akan mengkonfigurasi Git. Pastikan untuk memasukkan informasi Anda sendiri di dalam tanda kutip (tetap sertakan tanda kutip)! 
```bash
lrmn7@acer:~$ git config --global user.name "Your Name"
lrmn7@acer:~$ git config --global user.email "yourname@example.com"
``` 
Untuk mengecek konfigurasi yang sudah dilakukan tadi bisa dengan mengetikan perintah ini pada terminal.
```bash
lrmn7@acer:~$ git config --list
user.name=pseud0nyms
user.email=solihinlrmn70@gmail.com
color.ui=auto
``` 
#### Membuat akun Github atau Login ke Github
Buka <http://github.com> dan silahkan buat akun disana. Jika kamu sudah memiliki akun, silahkan login ke Github. Kamu tidak perlu menggunakan alamat email yang sama dengan yang kamu gunakan sebelumnya, tetapi mungkin ini adalah ide yang baik untuk menggunakan akun yang sama agar tutorial ini semuanya berjalan dengan mudah.

#### Membuat SSH Key
SSH Key adalah `cryptographically secure identifier`. Ini seperti kata sandi yang sangat panjang yang digunakan untuk mengidentifikasi mesin/pc kalian. 

> GitHub menggunakan SSH Key untuk memungkinkan kalian mengunggah ke repositori tanpa harus mengetikkan nama pengguna dan kata sandi Anda setiap saat.

<sub>**Note:** Untuk SSH Key bersifat optional, tapi **sangat direkomendasikan**. Jika kalian tidak melakukan konfigurasi SSH Key kalian akan direpotkan untuk memasukan username dan password saat akan melakukan push pada repository.</sub>

Pertama, kita perlu melihat apakah Anda sudah membuat SSH Key. Ketikkan ini ke terminal:
```bash
lrmn7@acer:~$ ls ~/.ssh/id_rsa.pub
No such file or directory
```
Jika pesan yang muncul berupa `No such file or directory`, berarti kalian belum membuat SSH Key. Jika pesan yang tampil seperti dibawah ini, berarti kalian sudah pernah membuat SSH Key sebelumnya, dan ikuti langkah selanjutnya.
```bash
lrmn7@acer:~$ ls ~/.ssh/id_rsa.pub
/home/lrmn7/.ssh/id_rsa.pub
```
Untuk membuat SSH Key, jalankan perintah berikut di dalam terminal Anda. Perintah `-C` diikuti oleh alamat email digunakan untuk memastikan bahwa GitHub tahu siapa kamu.
```bash
lrmn7@acer:~$ ssh-keygen -C yourname@example.com
```
  * Terminal akan meminta kamu untuk menyimpan Key yang akan dibuat, cukup tekan <kbd>↩ Enter</kbd>.
  * Selanjutnya dia akan memintamu untuk memasukan kata sandi, silakan masukan jika mau, tetapi itu tidak wajib.

#### Menghuhbungkan SSH Key dengan Github
Sekarang, kamu perlu memberi tahu GitHub SSH Key kamu sehingga bisa melakukan push (upload kode) tanpa harus mengetikkan kata sandi setiap saat.

  1. Pertama, masuk ke GitHub dan klik gambar profil Anda di sudut kanan atas. Kemudian, klik Pengaturan di menu drop-down.

  2. Selanjutnya, di sisi kiri, klik `SSH and GPG Keys`. Kemudian, klik tombol hijau di sudut kanan atas yang bertuliskan `New SSH Key`. Beri nama SSH Key mu sesuatu yang cukup deskriptif untuk Anda ingat dari mana asalnya. Biarkan windows ini terbuka saat Anda melakukan langkah selanjutnya.

  3. Sekarang kamu perlu menyalin SSH Key. Untuk melakukan ini, kita akan menggunakan perintah yang disebut `cat` untuk membaca file ke konsol/terminal.
  ```bash
  lrmn7@acer:~$ cat ~/.ssh/id_rsa.pub
  ```
  4. Copy teks yang dimulai dengan ssh-rsa dan berakhir dengan alamat email milik mu.

  5. Sekarang, kembali ke GitHub dan Paste SSH Key pada kolom. Kemudian, klik `Add SSH Key`. Sekarang kamu sudah selesai! Kamu telah berhasil menambahkan SSH Key!

### Membuat Repository Github

Sebelum membahas bagaimana cara mebuat repository, kita cari terlebih dahulu pengertian atau fungsi repository pada github ? Dibawah saya berikan penjelasannya yang saya kutip dari laman resmi github.

> Repositori bisa dikatakan/diibaratkan sebuah folder pada projek Anda. Repositori berisi semua file pada projek dan juga menyimpan setiap revisi/perubahan yang terjadi pada setiap file. Kalian pun dapat mendiskusikan dan mengelola projek secara bersama-sama di dalam repositori.

  1. Silahkan login terlebih dahulu ke <http://github.com>, perhatikan menu atas sebelah kanan. Terdapat icon lonceng, tanda plus (+) & avatar github. Klik tanda + dan pilih `New repository`.

  2. Pada bagian `Repository Name`, silahkan masukan nama repository yang akan kalian buat.

  3. Dibagian `Description`, silahkan deskripsikan projek yang akan kalian buat pada repository ini.

  4. Terdapat 2 jenis repository yang ada pada github, yaitu Public & Private. Silahkan kalian pilih Public saja.
      * `Public`: Siapapun dapat melihat repository ini, dan kamu dapat memilih siapa yang boleh melakukan commit.
      * `Private`: Kamu bisa memilih siapa saja yang dapat melihat dan melakukan commit pada 
      repository ini. (Private banyak digunakan oleh startup atau perusahaan IT dalam mengerjakan sebuah projek.)

  5. Sampai sini repository berhasil kalian buat, selanjutnya saya akan bahas bagaimana dan langkah cara menambahkan file projek ke repository.

### Upload/Push File ke Repository (Studi Kasus Sederhana)

Disini saya akan menyiapkan sebuah file index.html yang saya simpan pada folder latihan-github, pada index.html bisa kalian isi hello world. 

<sub> **(optional)** : _silahkan kalian skip tutorial dibawah pada bagian git status, git status hanya akan menampikan status pada file._ </sub>

Yang terpenting dalam melakukan tutorial ini yaitu, git init > git add > git commit > git remote add origin link repository > git push.

> git init dan git remote hanya dilakuan satu kali saja setiap membuat repository baru.

Saya sudah masuk ke folder projek dan menyiapkan file index.html
```bash
lrmn7@acer:~/latihan-github$ ls
index.html
```

Isi index.html
```html
<!DOCTYPE html>
<html>
<head>
<title>Contoh isi index.html</title>
</head>
<body>

<h1>Hello World!</h1>
<p>Saya sedang belajar latihan git & github</p>

</body>
</html>
```

  1. Pertama yang harus kalian lakukan adalah masuk kedalam folder projek yang kalian buat diatas via terminal.

  2. Kemudian ketikan `git init`. Hasilnya akan keluar seperti dibawah.
      ```bash
      lrmn7@acer:~/latihan-github$ git init
      Initialized empty Git repository in /home/lrmn7/latihan-github/.git/
      ```

  3. Selanjutnya kalian ketikan, `git status` untuk melihat status file tersebut apakah sudah di tambahkan/ada perubahan/dihapus/sudah di commit. Biasanya akan tampil teks berwarna `merah/hijau`. Akan tampil seperti dibawah.
  
      ```bash
      lrmn7@acer:~/latihan-github$ git status
      On branch master

      No commits yet

      Untracked files:
        (use "git add <file>..." to include in what will be committed)

        index.html

      nothing added to commit but untracked files present (use "git add" to track)
      ```

  4. Kemudian ketik `git add index.html`

      ```bash
      lrmn7@acer:~/latihan-github$ git add index.html
      ```

  5. Ketikan lagi `git status` untuk melihat status yg terjadi pada file tersebut. Akan tampak diterminal file index.html berwarna hijau.

      ```bash
      lrmn7@acer:~/latihan-github$ git status
      On branch master

      No commits yet

      Changes to be committed:
        (use "git rm --cached <file>..." to unstage)

        new file:   index.html
      ```

6. Langkah selanjutnya adalah melakukan commit file (memberikan deskripsi/catatan singkat pada file yang baru ditambahkan dengan git add sebelumnya). Silahkan ketik `git commit -m "upload pertama ke github"`.

      ```bash
      lrmn7@acer:~/latihan-github$ git commit -m "upload pertama ke github"
      [master (root-commit) 508c2ba] upload pertama ke github
      1 file changed, 12 insertions(+)
      create mode 100644 index.html
      ```

7. Ketik `git remote add origin git@github.com:pseud0nyms/belajar-github.git`. Silahkan teman-teman sesuaikan dengan link repository yang sudah teman-teman buat ya.

      ```bash
      lrmn7@acer:~/latihan-github$ git remote add origin git@github.com:pseud0nyms/belajar-github.git
      ```

8. Langakah terakhir yaitu ketik `git push -u origin master`, git akan melakukan upload file. Silahkan kalian reload halaman reposiory yang ada pada browser.

      ```bash
      lrmn7@acer:~/latihan-github$ git push -u origin master
      Counting objects: 3, done.
      Delta compression using up to 4 threads.
      Compressing objects: 100% (2/2), done.
      Writing objects: 100% (3/3), 357 bytes | 357.00 KiB/s, done.
      Total 3 (delta 0), reused 0 (delta 0)
      To github.com:pseud0nyms/belajar-github.git
      * [new branch]      master -> master
      Branch 'master' set up to track remote branch 'master' from 'origin'.
      ```

Untuk tutorial lainnya terkait dengan git akan menyusul, silahkan liat beberapa perintah yang ada pada github disini: <https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf>