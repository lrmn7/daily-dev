**Last updated**: August 19, 2021


> Tutorial pemasangan dan pengaturan python ini ditujukan untuk di lingkungan kerja FIAKO Engineering. Sehingga, harap disesuaikan lagi untuk kebutuhan masing-masing. 
{: .prompt-info }

Tutorial ini ditujukan untuk **user** bukan untuk **development**. Bisa dibilang proses pemasangan dan pengaturan untuk ruang kerja python bisa menyulitkan dan ribet awal-awalnya, tapi jika sistem sudah terpasang dengan baik, penggunaannya akan lebih mudah kedepannya. Sehingga, jika ada pertanyaan harap langsung hubungi saya.

# Pendahuluan

Secara umum berikut aplikasi yang akan digunakan dan dipasang:

- [Miniconda]
- [Visual Studio Code](https://code.visualstudio.com/)
- [Google Colab](https://colab.research.google.com/)

## Anaconda atau Miniconda

Banyak tutorial ataupun pelatihan yang menyarankan penggunaan [Anaconda], akan tetapi, saya ingin menyarankan hal yang berbeda yaitu [Miniconda]. Alasan pribadi saya adalah pengguna setidaknya "dipaksa" untuk navigasi dalam _virtual environment_ dan membiasakan diri dengan tampilan _command prompt_. Dan kalau ada kerusakan / _error_ cukup membuat _environment_ baru saja.

## Visual Studio Code

VSCode ini digunakan sebagai alternatif Notepad++ atau Ultraedit. Saya juga masih awam soal penggunaan VSCode karena saya menghabiskan waktu pengembangnnya menggunakan Google Colab. Tapi VSCode ini bisa dijadikan alternatif kalau tidak mau menggunakan Google Colab. Alternatif lain bisa menggunakan Jupyter Notebook atau Jupyter Lab. Tapi, saya juga lebih jarang menyentuh itu dibandingkan VSCode. Saya menggunakan VSCode lebih sering untuk pengembangan situs (Jekyll).

## Google Colab

Saya merekomendasikan untuk menggunakan Google Colab untuk pekerjaan Python. Karena dengan Google Colab, kolaborasi akan lebih mudah. Hanya saja, saya sarankan juga untuk menginstall [Drive for Desktop](https://www.google.com/drive/download/). Jadi, jika ketika offline, masih bisa melakukan pekerjaannya menggunakan VSCode atau Jupyter Notebook/Lab. Dan ketika online, sistem tersinkronisasi secara otomatis. Sehingga pekerjaan pythonnya disimpan di _cloud_. 

------

# Pemasangan

Berikut tahapan-tahapan untuk melakukan pemasangan dan pengaturan python. Harap dibaca terlebih dahulu keseluruhan sebelum mengikuti panduan dibawah ini. 

## Miniconda

1. Download Miniconda di halaman [ini](https://docs.conda.io/en/latest/miniconda.html).
  - Pada bagian "Latest Miniconda Installer Links", piliah `Miniconda3 Windows 64-bit`.
  - Akan terdownload file `Miniconda3-latest-Windows-x86_64.exe`.
2. Jalankan file tersebut dengan akses Administrator.
  - Klik kanan pada file install, kemudian pilih "Run as Administrator".
3. Di bagian "Welcome to Miniconda3 ..." klik **Next**.
4. Setujui _License Agreement_ dengan klik **I Agree**.
5. Pada Bagian "Install for:" pilih "All Users (required admin privileges)". 
6. **PENTING!**, Pada bagian "Destination Folder" ubah alamat instalasi menjadi **"C:\Miniconda3"**
7. Pada "Advanced Options", pastikan hanya "Register Miniconda3 as the system Python 3.9" yang dicentang ✔️.
![](https://docs.anaconda.com/_images/win-install-options.png){: w="500"}
8. Kemudian **Install**. 
9. Selesai sudah tahap instalasi Miniconda. (Bisa dibaca README dan halaman pengenalan Anaconda kalau penasaran. 🤭)

Selanjutnya dilanjutkan dengan pemasangan _virtual environment_ di conda:

1. Buka "Anaconda Prompt (Miniconda3)" melalui start menu (🪟).
2. Pastikan ada tulisan `(base)` sebelum alamat _working directory_.
3. Ketik perintah `conda update conda` untuk memastikan conda di versi terbaru. Atau bisa disalin dari kode dibawah ini (klik kanan untuk paste di _Anaconda Prompt_).  
```batchfile
conda update conda
```
  - Jika ada update terbaru, ketik Yes atau y, untuk melanjutkan proses update.
4. Membuat Environment `userhk37`:
  - Pada _Anaconda Prompt_ ketik perintah `conda env create --file "https://github.com/lrmn7/setup/raw/main/conda/userhk37.yml"`.
```batchfile
conda env create --file "https://github.com/lrmn7/setup/raw/main/conda/userhk37.yml"
```
  - Tunggu untuk conda melakukan _solving environment_.
5. Tunggu prosesnya sampai selesai. Proses selesai ketika muncul di _Anaconda Prompt_ `(base) C:\Users\...>`
6. Setelah itu ketik perintah `jupyter serverextension enable --py jupyter_http_over_ws`.
7. Oke. Tahap pemasangan environment `userhk37` telah selesai.

Jika ternyata sudah melakukan pembuatan environment `userhk37`, Anda bisa memperbaruinya di _Anaconda Prompt_ dengan perintah berikut:
```batchfile
conda env update --file "https://github.com/lrmn7/setup/raw/main/conda/userhk37.yml" --prune
```

## Visual Studio Code

Untuk instalasi Visual Studio Code bisa langsung didownload di [sini](https://code.visualstudio.com/#alt-downloads). Download versi "System Installer 64-bit". 

![](/docs/assets/images/20220421_pp_01.png){: w="250"}

Instalasi ini disesuaikan saja dengan kebutuhan pribadi tidak perlu pakai tutorial rasanya. 

# Pengaturan

Pengaturan ini diutamakan untuk VSCode dan Google Colab. Untuk miniconda sudah selesai dan bisa langsung digunakan. 

## Visual Studio Code

1. Buka Visual Studio Code.
  - Jika muncul dialog untuk melakukan instalasi extension, pilih **Install**. Biasanya muncul untuk python. 
2. Buka _Command Palette_ dengan `Ctrl + Shift + P`.
  - ketik "terminal select" sampai muncul pilihan "Terminal: Select Default Profile".
  - pilih "Terminal: Select Default Profile" dengan keyboard kemudian Enter.
  - pilih "Command Prompt".
3. Anda bisa membuka terminal dengan ``Ctrl + ` ``. Sebelum membuka, bisa mematikan seluruh terminal dengan membuka _Command Palette_ dan mencari opsi "Terminal: Kill All Terminal".
4. Install extension yang akan dibutuhkan. Berikut extension yang saya gunakan (yang utama hanya Python dan Jupyter sebenarnya).<br>
![](/docs/assets/images/20220421_pp_02.png)
5. Ketika ingin melakukan scripting menggunakan python (atau file `.py`) jangan lupa untuk mengatur interpreter ke `conda: userhk37`. Caranya dengan membuka _Command Palette_ dan mencari opsi "Python: Select Interpreter". Pilih ``Python 3.7 ('userhk37')``.

Karena nanti penggunaan akan lebih fokus ke Notebook, saya biasanya menggunakan Google Colab untuk interfacenya. Tapi hal ini dimungkinkan juga pakai VSCode, ini hanya masalah preferensi saja. 

## Google Colab

Jika belum pernah sama sekali membuka [Google Colab](https://colab.research.google.com/), tahapan ini perlu dilakukan agar dibuat folder "Colab Notebooks" di Google Drive.

1. Login ke akun Google.
2. Buka halaman [Google Colab](https://colab.research.google.com/)
3. Pilih "New Notebook"
4. Setelah terbuka notebook dengan judul "Untitled ...", simpan notebook dengan menekan `Ctrl + S`. 
5. Tutup halamannya.

## Google Drive

Bagian ini bisa dilakukan jika ingin menggunakan _notebook_ saat tidak dapat mengakses Google Colab (internet mati/offline).  

1. Install [Google Drive for Desktop](https://www.google.com/drive/download/). Untuk pengaturan gunakan seperti default.
2. Jika berhasil maka sistem akan membuat _Drive_ baru di sistem yaitu `G:\` yang dikhususkan untuk Google Drive.
3. Melalui windows explorer, Buka `G:\` > `My Drive`.
4. Klik kanan pada folder `Colab Notebooks` kemudian pilih "Offline Access" > "Available Offline" (Jika menggunakan Windows 11 pilih "Show more options") <br>
![](/docs/assets/images/20220421_pp_03.png)
5. Selesai sudah, notebook yang telah dibuat di Google Colab akan selalu tersedia di mesin Anda. 

Ketika terpasangnya Google Drive for Desktop, hal ini memungkinkan untuk melakukan pekerjaan baik di mesin lokal ataupun cloud. Tergantung kepentingannya. Pengalaman pribadi saya saat melakukan pemodelan _Deep Learning_, saat melatih model, saya menggunakan _cloud server_, tapi saat mengevaluasi hasil model, saya menggunakan mesin lokal. Tapi, rasanya, ini untuk tutorial sebagai _advanced user_ saja, karena perlu pengetahuan koding di pythonnya.  

------

Sudah selesai pemasangan dan pengaturan sistem python, untuk memulai bereksperimen dengan python akan dilanjutkan di tulisan berikutnya. Jika ada pertanyaan, hubungi saya langsung, atau melalui `lrmn.dev@gmail.com`.

<!-- LINKS -->
[Anaconda]: https://www.anaconda.com/
[Miniconda]: https://docs.conda.io/en/latest/miniconda.html