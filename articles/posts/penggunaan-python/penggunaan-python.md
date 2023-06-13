**Last updated**: August 19, 2021

> Tutorial penggunaan python ini ditujukan untuk di lingkungan kerja FIAKO Engineering. Sehingga, harap disesuaikan lagi untuk kebutuhan masing-masing. 
{: .prompt-info }

Tutorial ini merupakan lanjutan dari [tutorial pemasangan dan pengaturan python](pengaturan-python.md). Tutorial ini akan fokus bagaimana bekerja dengan python. Banyak beberapa metode untuk memulai koding dengan python. Pribadi, yang sering saya gunakan adalah [Google Colab]. Saat saya melakukan pengembangan hidrokit saya menggunakan [VSCode](https://code.visualstudio.com/). Jika diperlukan saya juga kadang menggunakan Jupyter Notebook atau Jupyter Lab, tapi kasusnya biasanya kalau keadaan offline atau perlu bereksperimen dengan jupyter notebook/lab.

# Google Colab

Penggunaan Google Colab bisa menggunakan dua mesin yaitu mesin lokal (offline) atau _cloud_ (online).

## Online

Untuk online, tidak perlu ada pengaturan sama sekali. Tinggal buka [Google Colab], "New notebook", jangan lupa _rename_ dokumen, _connect_ ke _hosted runtime_, langsung deh bereksperimen dengan python. Karena menggunakan mesin _cloud_, biasanya untuk pekerjaan yang sering dilakukan untuk _Data Science_ atau _Machine Learning_ paketnya sudah tersedia, jadi tidak perlu instalasi paket lainnya. Akan tetapi jika ingin install paket tertentu, bisa menggunakan perintah `!pip install [nama paket]`.

## Integrasi dengan Google Drive

> Integrasi ini sangat berbahaya jika Anda tidak mengetahui detail koding yang akan dijalankan. Dapat beresiko menghilangkan berkas di google drive, baik sebagian maupun keseluruhan. Konsultasikan terlebih dahulu sebelum melakukan integrasi ini. 
{: .prompt-danger}

Tahapan ini jika ingin melakukan integrasi dengan dokumen yang ada di Google Drive. Jadi, saat Anda menaruh berkas di folder `G:\My Drive\Colab Notebook\`, anda dapat mengaksesnya tanpa perlu terhubung dengan mesin lokal terlebih dahulu (diasumsikan bahwa sinkronisasi telah usai dan berhasil). Berikut _recipe_ untuk dapat mengakses dokumen di google drive. **Harap dipahami setiap barisnya sebelum dieksekusi**, jika ada pertanyaan, hubungi saya.

```python
_IS_LOCAL = False # #@param {type:"boolean"}

from pathlib import Path

if _IS_LOCAL:
    _LOCAL_DIRECTORY = '.' #@param {type:"string"}
    _DIRECTORY = Path(_LOCAL_DIRECTORY)
else:
    from google.colab import drive
    drive.mount('/content/gdrive')
    _CLOUD_DIRECTORY = '/content/gdrive/My Drive/Colab Notebooks' #@param {type:"string"}
    _DIRECTORY = Path(_CLOUD_DIRECTORY)

print(f':: Current Working Directory: {_DIRECTORY.cwd()}')
print(f':: _DIRECTORY = {_DIRECTORY}')
```

Untuk selanjutnya, Anda dapat mengakses berkas di folder Colab Notebook dengan `_DIRECTORY / 'lokasi/berkas/atau/folder'`. Perlu diingat bahwa hasil _return_ berupa object `pathlib.Path`. Untuk referensi lanjut mengenai `pathlib.Path` bisa baca [disini](https://docs.python.org/3/library/pathlib.html).

## Upload dokumen

Saat menggunakan mesin _cloud_, terkadang ada dokumen yang ingin kita akses tapi tidak mau melalui [Integrasi dengan Google Drive](#integrasi-dengan-google-drive). Solusi alternatifnya adalah mengupload file ke mesin _cloud_ **pada sesi tersebut**. Jadi, file tersebut hanya untuk sementara, selama _session_ / sesi masih aktif. Ada dua metode untuk upload dokumen, bisa menggunakan _script_ atau _drag and drop_. 

**Drag and Drop**

Buka Files disamping kiri, kemudian pilih icon untuk upload.

![](/docs/assets/images/20220422_pp_01.png)

**Script**

Gunakan _script_ dibawah ini untuk dapat dialog untuk mengupload files.

```python
from google.colab import files

upload = files.upload()
```

`upload` merupakan _dictionary_ yang berisikan file yang diupload (_key_ = nama file, _value_ = isi file). 

## Offline

Untuk menggunakan mesin lokal, bisa tergolong cukup perlu usaha dikarenakan harus membuka _Anaconda Prompt_ dan melakukan _copy-paste_ dari _prompt_ ke Google Colab. Berikut langkah-langkahnya (diasumsikan telah berhasil melakukan [pemasangan dan pengaturan python](pengaturan-python.md).

- Buka _Anaconda Prompt (userhk37)_ di Start Menu (🪟).
- Pastikan Anda di _environment_ `(userhk37)`. Jika tidak, ketik `conda activate userhk37`.
- Ketik perintah berikut ini di _Anaconda Prompt_:

```batchfile
jupyter notebook --NotebookApp.allow_origin="https://colab.research.google.com" --port=8888 --NotebookApp.port_retries=0 --no-browser --notebook-dir "G:\My Drive\Colab Notebooks"
```

- Berikut konfigurasi yang bisa digunakan dan catatan yang perlu diperhatikan:
    - `--notebook-dir "G:\My Drive\Colab Notebooks"`, Anda bisa mengganti `"G:\My Drive\Colab Notebooks"` ke alamat folder lainnya semisal `"D:\Data"`. Jika argumen ini dihilangkan, secara otomatis dia mengatur _current working directory_ berdasarkan alamat/_path_ saat perintah ini dipanggil. 
- Setelah dilakukan perintah tersebut, salin URL yang berawalan `http://localhost:8888/?token=...`.
    - Cara copynya arahkan kursor ke bagian awal url, kemudian _drag_ sampai bagian akhir url, klik kanan untuk menyalinnya.
- Setelah disalin, buka _notebook_ di Google Colab. Kemudian pilih Connect > "Connect to a local runtime".
- _Paste_ url yang telah disalin menggunakan `Ctrl + V`. Dilanjutkan dengan memilih **Connect**.
- Jika berhasil akan muncul keterangan "Connected (Local)". 

Beres sudah, untuk tutorial jika menggunakan Google Colab, baik lokal ataupun tidak.

-----

# Visual Studio Code

Untuk Visual Studio Code, hanya ada opsi untuk offline, sehingga tidak lebih rumit dibandingkan Google Colab. Penggunaan di VSCode juga tergolong sederhana karena navigasinya bisa langsung dilakukan dari _Command Palette_.

1. Buka dokumen `.py`, `.ipynb`, atau folder kerja menggunakan VSCode (klik kanan > Open with Code).
2. Buka _Command Palette_ (Ctrl + Shift + P), pilih "Python: Select Interpreter". Pilih `userhk37`.
3. Sudah selesai. Dapat langsung eksekusi file `.py` atau `.ipynb`. 

# Jupyter Notebook / Lab

Untuk menggunakan interface jupyter notebook/lab berikut langkahnya:

- Buka _Anaconda Prompt (userhk37)_ di Start Menu (🪟).
- Pastikan Anda di _environment_ `(userhk37)`. Jika tidak, ketik `conda activate userhk37`.
- Ketik perintah berikut ini di _Anaconda Prompt_:
    - `jupyter notebook` untuk membuka jupyter notebook.
    - `jupyter lab` untuk membuka jupyter lab. 
- Anda bisa menambah argumen `--notebook-dir "lokasi\yang\ingin\digunakan"` untuk mengatur _current working directory_. 

<!-- LINK -->
[Google Colab]: https://colab.research.google.com