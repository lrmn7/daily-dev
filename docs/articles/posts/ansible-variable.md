**Last updated**: April 21, 2020


### Apa itu Ansible Playbook ?

Ansible variable adalah nilai yang diberikan kepada variabel yang digunakan dalam playbook Ansible. Variabel ini dapat digunakan untuk mengatur nilai dari modul Ansible, seperti modul `package`, `copy`, atau `template`. Penggunaan variabel memungkinkan playbook untuk menjadi lebih fleksibel dan dapat digunakan di berbagai lingkungan dengan mudah.

#### Jenis Ansibel Variable

Ada beberapa jenis variabel yang dapat digunakan dalam playbook Ansible, antara lain:

1. **Variabel Built-in**: Variabel ini disediakan oleh Ansible secara default dan dapat digunakan tanpa harus didefinisikan secara eksplisit. Contohnya adalah variabel `inventory_hostname`, yang berisi nama host yang diatur dalam inventory Ansible.

2. **Variabel Dinamis**: Variabel ini didefinisikan secara dinamis dalam playbook Ansible dan diisi dengan nilai yang dihasilkan oleh tugas sebelumnya. Contohnya adalah variabel `ansible_facts`, yang berisi fakta-fakta tentang host yang diatur dalam playbook.

3. **Variabel Pengguna**: Variabel ini didefinisikan oleh pengguna dan digunakan dalam playbook Ansible. Contohnya adalah variabel `apache_port` atau `apache_root` pada contoh playbook yang telah diberikan sebelumnya.

Penggunaan variabel yang tepat dapat membuat playbook Ansible menjadi lebih efektif dan efisien, serta dapat mempermudah pengaturan nilai pada modul-modul Ansible.

#### Penamaan Variabel

Untuk menggunakan variabel dalam playbook Ansible, pengguna dapat memanggil variabel menggunakan tanda kurung kurawal. 

#### Waktu yang tepat menggunakan Variable

Waktu yang tepat untuk menggunakan variabel dalam Ansible adalah ketika pengguna ingin mengatur konfigurasi infrastruktur mereka secara fleksibel dan dinamis. Beberapa situasi yang umumnya memerlukan penggunaan variabel dalam Ansible adalah:

1. Konfigurasi yang berbeda untuk host yang berbeda: Ketika pengguna ingin memberikan konfigurasi yang berbeda untuk setiap host dalam inventory file, pengguna dapat menggunakan variabel untuk menyimpan nilai konfigurasi yang spesifik untuk setiap host.

2. Penggunaan playbook yang sama untuk lingkungan yang berbeda: Ketika pengguna ingin menggunakan playbook yang sama untuk lingkungan produksi dan pengembangan, pengguna dapat menggunakan variabel untuk menentukan nilai konfigurasi yang berbeda untuk setiap lingkungan.

3. Penggunaan playbook yang kompleks: Ketika pengguna menggunakan playbook yang kompleks, variabel dapat membantu memudahkan pengelolaan playbook dan menjaga kode playbook tetap terorganisir.

4. Menentukan nilai berdasarkan kondisi tertentu: Ketika pengguna ingin menentukan nilai variabel berdasarkan kondisi tertentu, seperti jenis sistem operasi atau versi perangkat lunak yang diinstal pada host, variabel dapat digunakan untuk menentukan nilai yang sesuai untuk setiap kondisi.

5. Menghindari pengulangan kode yang tidak perlu: Ketika pengguna ingin menghindari pengulangan kode yang tidak perlu dalam playbook, variabel dapat digunakan untuk menyimpan nilai yang sama dan digunakan di beberapa bagian playbook.

Dalam situasi-situasi ini, penggunaan variabel dapat membantu pengguna mengatur dan mengelola infrastruktur mereka dengan lebih mudah dan efisien. Namun, penting untuk memahami cara kerja variabel dan memastikan bahwa variabel yang digunakan sudah didefinisikan dengan benar untuk menghindari kesalahan dalam konfigurasi sistem.

### Contoh Ansible Variable di Playbook

#### Contoh penggunaan ansible variable built-in di ansible playbook

```yml
---
- name: Contoh Playbook menggunakan Variabel Built-In Ansible
  hosts: webservers
  become: true

  tasks:
    - name: Install Apache HTTP Server
      package:
        name: httpd
        state: present

    - name: Copy Apache Config File
      copy:
        src: files/httpd.conf
        dest: /etc/httpd/conf/httpd.conf
        owner: root
        group: root
        mode: '0644'
      notify:
        - Restart Apache

  handlers:
    - name: Restart Apache
      service:
        name: httpd
        state: restarted
```
Beberapa variabel built-in Ansible yang digunakan dalam playbook di atas adalah:

* `hosts`: variabel ini menentukan host yang akan diatur oleh playbook. Pada contoh di atas, playbook akan dijalankan pada host dengan grup `webservers`.

* `become`: variabel ini menentukan apakah pengguna ingin menggunakan akses root pada host yang diatur oleh playbook. Pada contoh di atas, variabel `become` diatur sebagai `true` yang berarti playbook akan dijalankan dengan akses root.

* `name`: variabel ini menentukan nama tugas yang akan dilakukan oleh playbook. Pada contoh di atas, tugas pertama bernama "Install Apache HTTP Server" dan tugas kedua bernama "Copy Apache Config File".

* `package`: variabel ini mengaktifkan modul Ansible `package` yang digunakan untuk menginstal paket pada host yang diatur oleh playbook. Pada contoh di atas, modul `package` digunakan untuk menginstal paket `httpd` pada host.

* `copy`: variabel ini mengaktifkan modul Ansible `copy` yang digunakan untuk menyalin file ke host yang diatur oleh playbook. Pada contoh di atas, modul `copy` digunakan untuk menyalin file `httpd.conf` ke direktori `/etc/httpd/conf` pada host.

* `notify`: variabel ini menentukan handler yang akan dipanggil ketika tugas selesai. Pada contoh di atas, handler `Restart Apache` akan dipanggil setelah tugas kedua selesai.

* `handlers`: variabel ini menentukan daftar handler yang akan dipanggil oleh playbook. Pada contoh di atas, handler `Restart Apache` akan merestart layanan `httpd` pada host.

#### Contoh playbook Ansible yang menggunakan variabel yang didefinisikan oleh pengguna

<img src="docs/assets/images/contoh-ansible-variable-1.png" alt="Contoh playbook Ansible yang menggunakan variabel yang didefinisikan oleh pengguna">

Beberapa variabel yang didefinisikan oleh pengguna dalam playbook di atas adalah:

* `apache_port`: variabel ini menentukan port yang akan digunakan oleh Apache HTTP Server. Pada contoh di atas, variabel `apache_port` diatur sebagai `80`.

* `apache_root`: variabel ini menentukan direktori root yang akan digunakan oleh Apache HTTP Server. Pada contoh di atas, variabel `apache_root` diatur sebagai `/var/www/html`.

* `proxy_server`: variabel ini merupakan contoh variabel yang harus didefinisikan oleh pengguna sebelum menjalankan playbook. Variabel ini digunakan untuk mengatur nilai dari variabel `http_proxy` dan `https_proxy` di dalam environment variable.

#### Contoh ansible playbook lainnya yang menggunakan variable dengan loop

<img src="docs/assets/images/contoh-ansible-variable-2.png" alt="Contoh ansible playbook lainnya yang menggunakan variable dengan loop">

Pada contoh playbook di atas, variabel `required_packages` didefinisikan oleh pengguna dan berisi daftar nama paket yang harus diinstal pada host. Variabel tersebut kemudian digunakan untuk melakukan perulangan dengan menggunakan `loop` pada tugas Install Required Packages. Setiap iterasi pada perulangan akan menginstal satu paket yang terdapat pada variabel `required_packages`.