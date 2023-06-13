**Last updated**: April 14, 2020


### Apa itu Ansible Inventory ?

  > Ansible inventory adalah file yang berisi daftar host atau node yang akan dikelola oleh Ansible. Inventory berisi informasi seperti nama host, alamat IP, dan koneksi SSH.

Inventory dapat ditulis dalam berbagai format, seperti file teks sederhana, file YAML, file JSON, atau file dinamis yang dihasilkan secara otomatis oleh program atau layanan konfigurasi lainnya.

Setiap host dalam inventory memiliki satu atau beberapa grup yang mengelompokkan host sesuai dengan fungsinya. Ansible menggunakan grup ini untuk mengelola host secara bersamaan, memilih host yang akan dioperasikan, dan menentukan tindakan apa yang harus dilakukan pada host tersebut.

  > Inventory juga dapat menyertakan variabel tambahan yang akan digunakan oleh playbook Ansible. Variabel ini dapat didefinisikan pada level host atau grup, dan dapat diakses oleh playbook saat menjalankan tugas.

Secara umum, inventory adalah elemen penting dalam pengelolaan konfigurasi dan otomatisasi dengan Ansible. Dengan menyimpan informasi host dan grup dalam inventory, pengguna Ansible dapat mengelola dan mengkonfigurasi server dan aplikasi secara efektif dan efisien.

### Kapan penggunaan Grup dalam Ansible Inventory diperlukan ?

Grup dalam Ansible inventory diperlukan ketika kita ingin mengelompokkan host sesuai dengan fungsinya atau atribut tertentu, sehingga kita dapat mengelola dan mengeksekusi playbook pada kelompok host yang sama secara bersamaan. Beberapa contoh penggunaan grup dalam inventory adalah:

1. **Memisahkan host production dan staging**: Dalam lingkungan produksi, kita mungkin ingin memisahkan host yang digunakan untuk produksi dan host yang digunakan untuk pengujian atau staging. Dalam hal ini, kita dapat membuat dua grup berbeda dalam inventory: group production dan group staging.

2. **Mengelompokkan host berdasarkan peran**: Dalam beberapa kasus, host mungkin memiliki peran yang berbeda-beda. Sebagai contoh, kita mungkin memiliki beberapa host yang berperan sebagai web server dan beberapa host lain yang berperan sebagai database server. Dalam hal ini, kita dapat membuat dua grup berbeda dalam inventory: group webserver dan group database.

3. **Mengatur variabel**: Grup dapat digunakan untuk menentukan variabel yang hanya berlaku pada satu atau beberapa host tertentu. Sebagai contoh, kita mungkin ingin menetapkan variabel yang berbeda untuk host yang digunakan di lingkungan produksi dan pengujian. Dalam hal ini, kita dapat membuat grup production dan staging yang masing-masing memiliki variabel yang berbeda.

Dengan menggunakan grup dalam inventory, kita dapat mengelola dan mengeksekusi playbook dengan lebih mudah dan efisien. Kita dapat memilih grup tertentu atau seluruh host dalam inventory untuk dieksekusi playbook, serta menentukan tindakan yang berbeda untuk setiap grup atau host yang berbeda.

### Aturan penamaan Ansible Inventory

Aturan penamaan grup dalam Ansible inventory tidaklah baku, namun ada beberapa pedoman yang umumnya diikuti oleh komunitas Ansible, yaitu:

1. **Gunakan nama grup yang mudah dipahami**: Nama grup harus mudah dipahami dan deskriptif, sehingga mudah diidentifikasi dan dipahami oleh seluruh anggota tim. Sebagai contoh, grup yang digunakan untuk server web dapat diberi nama "webserver".

2. **Hindari spasi atau karakter khusus**: Nama grup sebaiknya tidak mengandung spasi atau karakter khusus, karena dapat menyebabkan masalah pada saat pengolahan inventory. Sebagai alternatif, kita dapat menggunakan underscore (_) atau dash (-) sebagai pengganti spasi.

3. **Gunakan huruf kecil**: Nama grup sebaiknya menggunakan huruf kecil semua, karena penggunaan huruf besar dapat menyebabkan kesalahan penulisan dan sulit untuk dibaca.

4. **Hindari nama yang sama dengan variabel bawaan**: Sebaiknya hindari nama grup yang sama dengan variabel bawaan Ansible seperti all, ungrouped, atau localhost, karena ini dapat menyebabkan konflik dan masalah pada inventory.

5. **Gunakan format yang konsisten**: Sebaiknya gunakan format yang konsisten dalam penamaan grup, misalnya menggunakan format "role-nama" atau "app-nama" untuk memudahkan pengelompokan.

Dengan mengikuti aturan penamaan grup yang baik, kita dapat membuat inventory yang mudah dikelola dan diorganisasi dengan baik.

### Contoh Ansible Inventory

Berikut dibawah ini adalah beberapa contoh penulisan ansible inventory baik yang menuliskan menggunakan nama host, ip , grouping dan lainnya.

#### Inventory dengan grup menggunakan nama host
Penjelasan:

* Dalam inventory ini, kita hanya menggunakan nama host untuk mengidentifikasi host yang akan diatur dan dikelola.
* Ada dua grup yaitu webserver dan database, dimana webserver memiliki 3 host yaitu web-01, web-02, dan web-03, sedangkan database memiliki 2 host yaitu db-01 dan db-02.

```bash
[webserver]
web-01
web-02
web-03

[database]
db-01
db-02
```

#### Inventory dengan grup menggunakan IP address
Penjelasan:

* Dalam inventory ini, kita menggunakan alamat IP untuk mengidentifikasi host yang akan diatur dan dikelola.
* Ada dua grup yaitu webserver dan database, dimana webserver memiliki 3 host dengan alamat IP berturut-turut 192.168.0.10, 192.168.0.11, dan 192.168.0.12 
* Sedangkan database memiliki 2 host dengan alamat IP berturut-turut 192.168.0.20 dan 192.168.0.21

```bash
[webserver]
192.168.0.10
192.168.0.11
192.168.0.12

[database]
192.168.0.20
192.168.0.21
```

#### Inventory dengan grup dan variabel menggunakan nama host
Penjelasan:

* Dalam inventory ini, kita menggunakan nama host untuk mengidentifikasi host yang akan diatur dan dikelola, serta menentukan variabel http_port dan db_port untuk masing-masing host.
* Ada dua grup yaitu webserver dan database, dimana webserver memiliki 3 host yaitu web-01, web-02, dan web-03, sedangkan database memiliki 2 host yaitu db-01 dan db-02.
* Pada grup webserver, variabel http_port didefinisikan dengan nilai 80 untuk host web-01 dan web-02, sedangkan untuk host web-03 variabel http_port didefinisikan dengan nilai 8080.
* Pada grup database, variabel db_port didefinisikan dengan nilai 3306 untuk host db-01 dan nilai 5432 untuk host db-02.

```bash
[webserver]
web-01 http_port=80
web-02 http_port=80
web-03 http_port=8080

[database]
db-01 db_port=3306
db-02 db_port=5432
```

#### Ansible inventory tanpa grup
Penjelasan:

* Ini adalah contoh Ansible inventory sederhana yang hanya terdiri dari tiga host, yaitu "webserver1", "webserver2", dan "webserver3".
* Setiap host didefinisikan dengan menggunakan parameter "ansible_host" yang menentukan alamat IP host yang digunakan oleh Ansible untuk terhubung ke host tersebut.
* Tidak ada grup yang didefinisikan dalam file inventory ini. Oleh karena itu, semua host yang didefinisikan dalam file inventory akan diperlakukan secara individual oleh Ansible.
* Dalam playbook atau perintah Ansible lainnya, kita dapat menggunakan nama host "webserver1", "webserver2", atau "webserver3" untuk merujuk pada host yang terkait dalam file inventory ini.

```bash
webserver1 ansible_host=192.168.1.101
webserver2 ansible_host=192.168.1.102
webserver3 ansible_host=192.168.1.103
```

Atau bisa juga dengan ansible inventory seperti materi sebelumnya

```bash
ubuntu-managed1

[belajaransible]
ubuntu-managed2
```