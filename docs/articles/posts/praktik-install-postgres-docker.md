**Last updated**: June 19, 2020


### Prasyarat
Sebelum masuk ke materi, ada beberapa hal yang perlu dilakukan yaitu menginstall docker & docker compose terlebih dahulu serta menginstall DBeaver sebagai DBMS-nya. Tutorial dapat dilihat di internet atau artikel sebelumnya.

### Apa itu Docker ?
Docker adalah perangkat lunak yang memungkinkan Anda untuk membangun, memulai, dan menjalankan aplikasi dalam lingkungan virtual yang dikenal sebagai _"container"_. Ini membantu mengatasi masalah kompatibilitas yang sering terjadi saat menjalankan aplikasi pada berbagai sistem operasi. 

  > Docker memungkinkan Anda untuk membungkus aplikasi dan dependensinya menjadi satu paket yang dapat dengan mudah dipindahkan dan dijalankan di mana saja. Hal ini membuat aplikasi lebih mudah dikembangkan dan di deployment. Docker juga memungkinkan Anda untuk memanagement sumber daya dan memastikan konsistensi lingkungan selama proses pengembangan dan produksi.

### Kenapa menggunakan Docker ?

Berikut adalah beberapa kelebihan menginstall aplikasi di docker dibandingkan menginstall aplikasi langsung pada windows:

1. **Portabilitas**: Aplikasi yang diinstall di dalam docker dapat dengan mudah dipindahkan dari satu sistem ke sistem lain tanpa masalah kompatibilitas.

2. **Isolasi lingkungan**: Docker membantu memisahkan aplikasi dan dependensinya dari sistem operasi host, sehingga memastikan bahwa aplikasi bekerja dengan benar meskipun sistem operasi host berubah.

3. **Kemudahan management**: Docker memungkinkan Anda untuk memanage sumber daya dan memastikan konsistensi lingkungan selama proses pengembangan dan produksi.

4. **Keamanan**: Docker membantu memastikan keamanan aplikasi dengan membatasi akses aplikasi ke sistem operasi host.

5. **Efisiensi**: Docker memungkinkan Anda untuk menjalankan lebih banyak aplikasi pada satu mesin tanpa mempengaruhi kinerja satu sama lain.

Dengan demikian, menginstall aplikasi di docker dapat memberikan banyak keuntungan dibandingkan menginstall aplikasi secara langsung pada windows.

#### Install PostgreSQL menggunakan Docker Compose

Untuk langkah pertama disini saya membuat folder dan docker-compose nya:

```bash
lrmn7@Thinkpad-T460:~$ mkdir belajar-postgres
lrmn7@Thinkpad-T460:~$ cd belajar-postgres
lrmn7@Thinkpad-T460:~/belajar-postgres$ touch docker-compose.yml
lrmn7@Thinkpad-T460:~/belajar-postgres$ sudo vim docker-compose.yml
``` 

Kemudian silahkan copy docker-compose dibawah ini
```yml
version: '3'
services:
  db:
    image: postgres:latest
    container_name: postgresql-compose
    ports:
      - "5432:5432"
    volumes:
      - postgresql-volume:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=lrmn7
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=postgresql-database
    networks:
      - postgresql-network
volumes:
  postgresql-volume:
networks:
  postgresql-network:
```

Berikut penjelasan dari docker-compose diatas.

File ini menjalankan sebuah container PostgreSQL yang memiliki nama "postgresql-compose", membuka port 5432, dan menyimpan data pada volume "postgresql-volume". Container ini juga memiliki beberapa environment variable seperti nama pengguna, password, dan nama database. Container ini tergabung dalam sebuah jaringan bernama "postgresql-network".

```bash
lrmn7@Thinkpad-T460:~/belajar-postgres$ docker-compose up
```

#### Koneksi PostgreSQL dengan DBeaver

Setelah berhasil menjalankan Postgre, langkah selanjutnya adalah melakukan tes koneksi dari database ke DMBS yang digunakan yaitu DBeaver. Berikut langkahnya:

1. Klik Create New Database Connection
2. Kemudian pilih PostgreSQL dan lik next
3. Kemudian isi beberapa kolom berikut ini:
  * Host: 127.0.0.1
  * Port: 5432
  * Database: postgresql-database
  * Username: lrmn7
  * Password: secret
4. Klik Test Connection, jika sukses silahkan klik finish.
  <img src="docs/assets/images/Koneksi-PostgreSQL.PNG" alt="Koneksi PostgreSQL dengan DBeaver">  
5. Jika sudah bisa dilakukan import sample database yang ada pada tutorial di link ini: [Sample database](https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/)
6. Untuk stop container bisa dilakukan CTRL + C di 'docker-compose up' sebelumnya.

<sub> **Catatan** : _Penggunaan docker volume akan menyimpan database yang sudah di eksekusi atau import ketika docker container di stop, sehingga ketika ingin menjalankan kembali PostgreSQL bisa dilakukan `docker-compose up` kembali tanpa takut data hilang._ </sub>



