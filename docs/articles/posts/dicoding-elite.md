**Last updated**: May 1, 2020


#### Technical Test Result
**Status: Submission Approved**

#### Project: script_test.sh

##### Summary Project
Command: `sleep 3s | echo -e "\n"` dan `sleep 1m | echo -e "\n"`
Command ini tidak memberikan kontribusi yang signifikan terhadap output yang ditampilkan. Command `sleep 3s` digunakan untuk memberikan jeda selama 3 detik sebelum mencetak baris kosong dengan `echo -e "\n"`, begitu juga dengan command `sleep 1m`. 

> Namun, baris kosong tersebut tidak memberikan informasi tambahan yang berguna dalam konteks script ini. Jika tujuannya hanya memberikan jeda sejenak sebelum keluar dari script, maka jeda tersebut tidak diperlukan dan dapat dihapus.

Command: `free --mega | sort` dan `df -hBG | sort` juga `df -x tmpfs --output=source,pcent | sort`
Dari setiap command diatas, command `| sort` lebih baik dihapus saja agar hasil yang ditampilkan menjadi _humman readable_. 

##### Code Reviewer
Jika menambahkan pipeline `sort` hasilnya seperti ini:
```bash
febrian@linuxmint:~/Documents/Devops-submission$ df -hBG | sort
/dev/sda1             1G    1G        1G   1% /boot/efi
/dev/sda3           102G   60G       37G  62% /
Filesystem     1G-blocks  Used Available Use% Mounted on
tmpfs                 1G    1G        1G   1% /run
tmpfs                 1G    1G        1G   1% /run/lock
tmpfs                 1G    1G        1G   1% /run/user/1000
tmpfs                 4G    1G        4G   3% /dev/shm
```
Namun jika menghapus `sort` akan menjadi seperti ini (membentuk _table header_ dibagian atas):
```bash
febrian@linuxmint:~/Documents/Devops-submission$ df -hBG
Filesystem     1G-blocks  Used Available Use% Mounted on
tmpfs                 1G    1G        1G   1% /run
/dev/sda3           102G   60G       37G  62% /
tmpfs                 4G    1G        4G   3% /dev/shm
tmpfs                 1G    1G        1G   1% /run/lock
/dev/sda1             1G    1G        1G   1% /boot/efi
tmpfs                 1G    1G        1G   1% /run/user/1000
```
Kalian dapat mengetahui lebih banyak mengenai command yang digunakan dengan mengetikan: `man (nama commandnya)`
contoh: `man free`

 - Di Linux, perintah `man` merupakan singkatan dari "manual".  Perintah
   "man" digunakan untuk mengakses dokumentasi manual (manual pages)
   yang menyediakan informasi rinci tentang perintah, fungsi, dan file
   pada sistem operasi Linux.  
 - Fungsi utama `man` adalah memberikan
   panduan pengguna terperinci tentang cara menggunakan perintah dan
   memahami argumen yang diperlukan.

#### Project2: history_dicoding.txt

##### Summary Project
Diproject kedua ini, untuk membuang perintah yang ada dalam file history_dicoding.txt, kalian dapat menggunakan command `>` di linux.
- Tanda `>` digunakan untuk mengalihkan output dari perintah ke sebuah file dan menghapus konten file yang ada sebelumnya. 
- Jika file tidak ada, maka file baru akan dibuat. Jika file sudah ada, maka isinya akan digantikan dengan output perintah yang baru.

##### Code Reviewer
Kalian perlu memasukan perintah dibawah di script yang kalian buat (scipt_test.sh) atau secara langsung di terminal.
`> history_dicoding.txt` maka semua teks yang ada di file tersebut akan hilang (kosong). 

Perintah `>` sangat berguna untuk **menyimpan hasil output perintah ke dalam file, menghasilkan file baru, atau menggantikan isi file yang ada**.
