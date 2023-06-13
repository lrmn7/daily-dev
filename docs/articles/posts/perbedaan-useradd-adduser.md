**Last updated**: May 14, 2021


### Perbedaan useradd & adduser
`useradd` dan `adduser` adalah perintah di sistem operasi Linux yang digunakan untuk membuat akun pengguna baru. Meskipun keduanya dapat digunakan untuk tujuan yang sama, ada beberapa perbedaan yang harus dipertimbangkan.

1. Konfigurasi Default
`useradd` hanya menambahkan pengguna baru ke sistem dan tidak membuat direktori rumah atau mengatur konfigurasi default lainnya. Dalam banyak kasus, pengguna baru harus diatur dengan direktori rumah dan file konfigurasi default tertentu. Dalam hal ini, `adduser` lebih fleksibel, karena perintah ini akan mengatur konfigurasi default yang tepat saat pengguna baru dibuat.

2. Interaksi dengan Pengguna
Ketika menjalankan `adduser`, pengguna akan diminta untuk memasukkan beberapa informasi tambahan, seperti nama lengkap, nomor telepon, dan alamat email. Informasi ini dapat membantu administrator sistem untuk mengelola akun pengguna secara lebih efektif dan dapat digunakan untuk menghasilkan daftar pengguna yang lebih terorganisir.

3. Konfigurasi yang Lebih Otomatis
Seperti yang telah disebutkan sebelumnya, `adduser` menyediakan konfigurasi yang lebih otomatis daripada `useradd`. Dalam banyak kasus, hal ini dapat menghemat waktu dan usaha dalam pengaturan akun pengguna.

4. Ketergantungan Paket
Beberapa distribusi Linux hanya menyediakan `adduser`, sementara yang lain menyertakan kedua perintah. Dalam hal ini, penggunaan perintah mana yang lebih tepat tergantung pada preferensi dan kebutuhan pengguna, serta ketergantungan paket.

5. Opsi yang Lebih Mudah Dipahami
`adduser` memiliki opsi yang lebih mudah dipahami daripada `useradd`, terutama bagi pengguna yang baru mengenal sistem Linux. Ini karena perintah tersebut menyediakan opsi-opsi yang lebih jelas dan dapat dipahami, seperti -gecos untuk memasukkan informasi tambahan pengguna.

Dalam kesimpulan, `adduser` lebih disukai oleh administrator sistem yang ingin membuat akun pengguna dengan konfigurasi default yang lengkap dan informasi tambahan, sementara `useradd` lebih cocok untuk pengguna yang ingin membuat akun dengan opsi yang lebih spesifik. Namun, keduanya dapat digunakan untuk membuat akun pengguna baru dan tidak ada yang benar atau salah dalam memilih salah satu perintah tersebut.

#### Menambahkan akses sudo ke user baru
Untuk memberikan akses sudo kepada user baru setelah melakukan adduser, Anda dapat mengikuti langkah-langkah berikut:

1. Masuk sebagai root atau sebagai pengguna yang memiliki hak akses sudo.

2. Buka berkas sudoers menggunakan perintah visudo. Pastikan untuk menggunakan perintah ini karena berkas sudoers harus diedit dengan hati-hati untuk menghindari masalah keamanan.
```bash
sudo visudo
```

3. Tambahkan baris berikut pada akhir berkas sudoers untuk memberikan akses sudo ke user baru. Ganti nama_user_baru dengan nama pengguna yang ingin Anda berikan akses sudo.
```bash
nama_user_baru ALL=(ALL:ALL) NOPASSWD:ALL
```

4. Simpan dan keluar dari berkas sudoers. Pastikan untuk menyimpan perubahan dengan benar.

5. Sekarang user baru dapat menggunakan perintah sudo dengan cara mengetikkan perintah sudo diikuti oleh perintah yang ingin mereka jalankan, lalu memasukkan kata sandi mereka saat diminta.

Dengan mengikuti langkah-langkah di atas, user baru sekarang memiliki akses sudo dan dapat menjalankan perintah dengan hak akses root. Namun, pastikan untuk memberikan akses sudo hanya kepada pengguna yang membutuhkannya dan tidak memberikan akses sudo ke semua pengguna tanpa pertimbangan yang matang karena dapat membahayakan keamanan sistem.