**Last updated**: April 1, 2020


### Apa itu Ansible?
> Ansible adalah alat open source yang digunakan untuk mengotomatisasi tugas-tugas IT seperti manajemen konfigurasi, penyebaran aplikasi, dan otomatisasi tugas-tugas rutin. Ansible memungkinkan Anda untuk mengotomatisasi tugas-tugas IT di seluruh lingkungan, dari server lokal hingga server cloud dan lingkungan hybrid.

Dalam istilah yang lebih sederhana, Ansible adalah alat yang dapat membantu Anda mengelola banyak server secara efisien, sehingga Anda tidak perlu menghabiskan banyak waktu dan upaya dalam melakukan tugas-tugas manual seperti menginstal perangkat lunak, mengonfigurasi sistem, dan melakukan tugas-tugas serupa pada banyak server yang berbeda.

#### Kenapa perlu Ansible?

Sebagai seorang DevOps, Ansible dapat sangat bermanfaat bagi Anda dalam beberapa cara:

1. **Otomatisasi**: Dalam dunia DevOps, otomatisasi sangat penting untuk meningkatkan efisiensi dan mengurangi kesalahan manusia. Ansible memungkinkan Anda untuk mengotomatisasi banyak tugas rutin seperti manajemen konfigurasi, penyebaran aplikasi, dan otomatisasi tugas-tugas operasional sehingga Anda dapat menghabiskan lebih sedikit waktu dan upaya dalam melakukan tugas-tugas manual.

2. **Konsistensi**: Dalam sebuah lingkungan IT, konsistensi sangat penting untuk memastikan bahwa semua server dan sistem bekerja dengan benar. Ansible memungkinkan Anda untuk melakukan tugas-tugas yang sama pada banyak server dengan cara yang konsisten, sehingga memastikan bahwa semua server bekerja pada tingkat yang sama.

3. **Skalabilitas**: Ansible dapat membantu Anda mengelola lingkungan IT yang kompleks dan berkembang dengan cepat. Dengan Ansible, Anda dapat mengelola banyak server dan sistem dengan cara yang efisien dan mudah diperluas.

4. **Integrasi**: Ansible dapat diintegrasikan dengan banyak alat dan platform lain seperti Git, Jenkins, Docker, dan sebagainya. Ini memungkinkan Anda untuk mengotomatisasi seluruh siklus hidup aplikasi dari pengembangan hingga produksi dan membantu Anda mencapai tujuan DevOps Anda dengan lebih efisien.

5. **Manajemen infrastruktur**: Ansible membantu Anda mengelola infrastruktur dan lingkungan IT secara keseluruhan. Dalam peran DevOps, Anda harus memastikan bahwa semua sistem, server, dan aplikasi berjalan dengan benar dan Ansible dapat membantu Anda mencapai tujuan itu.

### Apa itu Ansible Module?

> Ansible Module adalah unit kerja dasar dalam Ansible yang digunakan untuk menentukan tindakan yang harus dilakukan pada host yang dikelola. Module Ansible dapat digunakan untuk melakukan tugas-tugas seperti manajemen paket, manajemen file, pengaturan konfigurasi, manajemen pengguna, dan banyak lagi.

Module Ansible berisi kode yang dapat dijalankan pada host yang dikelola untuk menyelesaikan tugas-tugas tertentu. Module ini dapat dijalankan secara independen atau sebagai bagian dari playbook Ansible. Setiap module Ansible memiliki parameter khusus yang dapat dikonfigurasi untuk menyesuaikan perilaku module.

Ada ratusan module Ansible yang tersedia untuk digunakan, dan module-module ini terus berkembang seiring dengan perkembangan teknologi dan permintaan komunitas. Beberapa contoh module Ansible yang umum digunakan adalah:

* **Module File**: Digunakan untuk mengelola file dan direktori pada host yang dikelola.
* **Module Copy**: Digunakan untuk menyalin file dari mesin kontrol ke host yang dikelola.
* **Module Service**: Digunakan untuk mengelola layanan pada host yang dikelola, seperti memulai, menghentikan, dan me-restart layanan.
* **Module Package**: Digunakan untuk menginstal atau menghapus paket perangkat lunak pada host yang dikelola.
* **Module User**: Digunakan untuk mengelola pengguna dan kelompok pengguna pada host yang dikelola.

Module-module ini membantu memudahkan tugas-tugas administrasi pada host yang dikelola, dan dapat disesuaikan untuk memenuhi kebutuhan spesifik dari lingkungan yang dikelola. Dalam Ansible, Anda dapat menggunakan module-module ini untuk membangun playbook yang efisien dan akurat untuk mengotomatisasi tugas-tugas Anda.

### Format YAML di Ansible

> YAML (YAML Ain't Markup Language) adalah format serialisasi data yang umum digunakan di Ansible. Format YAML digunakan dalam playbook Ansible untuk menyediakan struktur yang mudah dibaca dan dipahami. Format ini membantu membuat file playbook Ansible menjadi lebih mudah dibaca dan dipahami oleh manusia.

Berikut adalah contoh playbook Ansible dengan format YAML:

```yaml
---
- name: Install Apache
  hosts: webservers
  become: yes
  tasks:
    - name: Install Apache
      apt: name=apache2 state=present
      notify:
        - restart apache

    - name: Ensure Apache is running
      service: name=apache2 state=started enabled=yes

  handlers:
    - name: restart apache
      service: name=apache2 state=restarted
```

Beberapa poin penting yang harus diperhatikan dalam format YAML di Ansible adalah sebagai berikut:

1. Nama file playbook harus memiliki ekstensi .yml atau .yaml.
2. Struktur playbook harus diawali dengan tiga tanda strip atau dash (---) untuk menunjukkan awal dari dokumen YAML.
3. Setiap bagian dalam playbook seperti hosts, tasks, atau handlers harus dinyatakan sebagai key-value pairs.
4. Key dalam playbook harus ditulis tanpa tanda petik atau quotes, sedangkan value dalam playbook bisa ditulis dalam tanda petik atau tanpa tanda petik tergantung pada konteksnya.
5. Indentasi sangat penting dalam format YAML. Indentasi digunakan untuk menunjukkan tingkat hierarki dan membuat playbook lebih mudah dibaca.
6. Tanda titik dua (:) digunakan untuk memisahkan key dan value dalam playbook.
7. Tanda strip atau dash (-) digunakan untuk membuat list dalam playbook.

Dalam format YAML di Ansible, penting untuk mengikuti aturan format yang benar untuk membuat playbook yang valid dan dapat dijalankan dengan benar.

### Apa itu Ansible Playbook?

> Ansible Playbook adalah file YAML yang digunakan untuk mengotomatisasi tugas-tugas pada host yang dikelola. Playbook berisi kumpulan tugas yang harus dijalankan pada host yang dikelola. Setiap tugas dalam playbook mewakili satu unit kerja dalam proses otomatisasi.

Contoh sederhana dari playbook Ansible adalah playbook untuk menginstal dan mengkonfigurasi Nginx pada host yang dikelola:

```yaml
---
- name: Install Nginx
  hosts: webservers
  become: true
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Configure Nginx
      copy:
        src: /path/to/nginx.conf
        dest: /etc/nginx/nginx.conf
      notify: restart nginx

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

Penjelasan dari playbook di atas:

* `name`: Nama playbook, yang memberikan deskripsi tentang tugas yang dilakukan.
* `hosts`: Target host yang akan dikelola.
* `become`: Digunakan untuk mengaktifkan hak akses superuser atau sudo untuk menjalankan tugas-tugas di host yang dikelola.
* `tasks`: Daftar tugas yang akan dijalankan pada host yang dikelola.
* `name`: Nama tugas, yang memberikan deskripsi tentang apa yang akan dilakukan tugas tersebut.
* `apt`: Modul Ansible yang digunakan untuk mengelola paket di sistem berbasis Debian/Ubuntu.
* `copy`: Modul Ansible yang digunakan untuk menyalin file dari mesin kontrol ke host yang dikelola.
* `notify`: Tindakan yang harus dilakukan jika ada perubahan dalam tugas. Dalam contoh ini, jika file konfigurasi Nginx berubah, maka tindakan restart nginx akan dilakukan.
* `handlers`: Daftar tindakan yang harus dilakukan jika ada notifikasi dari tugas. Dalam contoh ini, tindakan restart nginx akan dilakukan jika ada notifikasi dari tugas Configure Nginx.

Playbook Ansible seperti contoh di atas dapat membantu memudahkan tugas-tugas administrasi pada host yang dikelola. Dengan mengikuti format YAML dan module-module yang disediakan oleh Ansible, playbook dapat dibuat dengan mudah dan dipahami dengan baik.

### Apa itu Ansible Inventory

Ansible Inventory adalah file konfigurasi yang digunakan untuk mengelompokkan host yang akan dikelola oleh Ansible. File ini berisi daftar host yang dikelola, grup host, dan variabel yang terkait dengan host atau grup host. Ansible Inventory juga digunakan untuk menentukan bagaimana Ansible akan menghubungi host yang dikelola, seperti melalui SSH atau WinRM.

Contoh sederhana dari file Ansible Inventory adalah sebagai berikut:
```bash
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
db2.example.com

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=/path/to/private_key.pem
```

Penjelasan dari file Inventory di atas:

* `[webservers]` dan `[dbservers]` adalah grup host yang digunakan untuk mengelompokkan host yang akan dikelola oleh Ansible.
* `web1.example.com`, `web2.example.com`, `db1.example.com`, dan `db2.example.com` adalah host yang dikelola oleh Ansible.
* `[all:vars]` adalah bagian yang digunakan untuk menyediakan variabel yang terkait dengan host atau grup host. Dalam contoh di atas, variabel ansible_user digunakan untuk menentukan nama pengguna yang akan digunakan untuk menghubungi host, sedangkan variabel `ansible_ssh_private_key_file` digunakan untuk menentukan lokasi kunci privat SSH yang digunakan untuk autentikasi.

Dalam file Ansible Inventory, host dan grup host dapat dikelompokkan berdasarkan berbagai kriteria, seperti lingkungan, lokasi, atau peran. Dengan cara ini, tugas-tugas administrasi pada host yang dikelola dapat dikelola secara lebih terstruktur dan efisien.

### Ansible Tower

> Ansible Tower adalah platform manajemen konfigurasi yang menyediakan antarmuka pengguna berbasis web dan fitur-fitur tambahan untuk membantu mengotomatisasi tugas-tugas konfigurasi pada skala enterprise. 

Ansible Tower menyediakan antarmuka yang mudah digunakan, akses kontrol yang ketat, dan fitur-fitur pelaporan dan audit, sehingga memudahkan tim IT untuk mengelola dan mengawasi otomatisasi tugas-tugas konfigurasi di seluruh infrastruktur IT mereka. Ansible Tower juga menyediakan fitur-fitur seperti penjadwalan tugas, manajemen inventaris, dan integrasi dengan sistem manajemen konfigurasi lainnya untuk membantu mempercepat dan meningkatkan efisiensi operasi IT.

#### Perbedaan Ansible dan Ansible Tower

Perbedaan utama antara Ansible dan Ansible Tower adalah sebagai berikut:

1. **Fungsionalitas**: Ansible adalah platform open-source untuk otomatisasi konfigurasi yang digunakan untuk mengotomatisasi tugas-tugas di seluruh infrastruktur IT. Ansible Tower adalah produk komersial yang memperluas fungsionalitas Ansible dengan menambahkan fitur-fitur tambahan, seperti antarmuka pengguna berbasis web, manajemen inventaris, penjadwalan tugas, manajemen akses, dan pelaporan.

2. **Antarmuka pengguna**: Ansible menggunakan antarmuka baris perintah untuk mengelola tugas-tugas otomatisasi konfigurasinya, sedangkan Ansible Tower menyediakan antarmuka pengguna berbasis web yang memungkinkan pengguna untuk mengelola dan melacak tugas-tugas otomatisasi dengan lebih mudah.

3. **Manajemen inventaris**: Ansible Tower menyediakan fitur manajemen inventaris yang memudahkan pengguna untuk mengelola dan mengorganisasi host dan grup host dalam inventaris mereka. Fitur ini memungkinkan pengguna untuk melihat status host, menyaring host berdasarkan atribut tertentu, dan melihat riwayat tugas-tugas yang telah dijalankan pada host tertentu.

4. **Manajemen akses**: Ansible Tower menyediakan fitur manajemen akses yang memungkinkan pengguna untuk menentukan hak akses yang berbeda untuk pengguna dan tim yang berbeda. Dengan cara ini, pengguna dapat membatasi akses ke tugas-tugas otomatisasi dan host yang mungkin tidak relevan untuk pekerjaan mereka.

5. **Pelaporan**: Ansible Tower menyediakan fitur pelaporan yang memungkinkan pengguna untuk melacak dan menganalisis kinerja tugas-tugas otomatisasi mereka. Fitur ini memungkinkan pengguna untuk melihat riwayat tugas-tugas, status tugas-tugas, dan metrik kinerja yang relevan.

Secara umum, Ansible cocok untuk penggunaan di lingkungan yang lebih kecil dan tidak memerlukan fitur-fitur tambahan, sementara Ansible Tower lebih cocok untuk penggunaan di lingkungan enterprise yang memerlukan manajemen tugas-tugas otomatisasi yang lebih terpusat dan kompleks.

### Jinja2 Template di Ansible

Jinja2 adalah mesin template Python yang digunakan oleh Ansible untuk membuat file konfigurasi yang digunakan untuk mengonfigurasi server dan perangkat jaringan. 

> Ansible menggunakan Jinja2 sebagai bahasa template untuk membuat file konfigurasi yang digunakan untuk mengelola host target. Dalam file playbook Ansible, template Jinja2 dapat digunakan untuk mengganti variabel yang disediakan oleh inventory dan faktor-faktor lainnya dengan nilai yang spesifik untuk host target tertentu. 

Hal ini memungkinkan pengguna untuk membuat konfigurasi yang dapat disesuaikan secara dinamis berdasarkan keadaan masing-masing host target, yang dapat menghemat waktu dan mengurangi kesalahan konfigurasi. Dalam hal ini, Jinja2 berfungsi sebagai mekanisme untuk menghasilkan file konfigurasi yang digunakan oleh Ansible untuk melakukan tugas-tugas konfigurasi dan pengelolaan host.

Berikut adalah contoh penggunaan Jinja2 template di playbook Ansible untuk menginstal Nginx di host target:

File **nginx.list.j2**:
```bash
---
deb http://nginx.org/packages/mainline/ubuntu/ jammy nginx
```
File **playbook.yml**:
```yaml
- name: Contoh Jinja2 template install Nginx
  hosts: webservers
  become: true
  tasks:
    - name: add nginx repo
      template:
        src: nginx.list.j2
        dest: /etc/apt/sources.list.d/nginx.list

    - name: apt key nginx
      ansible.builtin.apt_key:
        url: https://nginx.org/keys/nginx_signing.key
        state: present

    - name: update repo
      apt:
        update_cache: true
        cache_valid_time: 3600
        force_apt_get: true

    - name: install nginx
      apt:
        name: nginx=1.23.1-1~jammy
        update_cache: yes
        force_apt_get: yes

    - name: starting the nginx service
      service: name=nginx state=started enabled=yes
```