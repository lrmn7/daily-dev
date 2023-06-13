**Last updated**: April 16, 2020


### Apa itu Ansible Role ?

Di Ansible, "roles" merujuk pada unit organisasi yang digunakan untuk mengelompokkan dan mengatur konfigurasi, tugas, dan file yang terkait dalam lingkungan Ansible. Roles digunakan untuk memecah konfigurasi menjadi komponen yang lebih kecil dan dapat digunakan kembali, sehingga mempermudah pengelolaan konfigurasi sistem yang kompleks.

#### Kegunaan Ansible Role

Kegunaan Roles di Ansible adalah sebagai berikut:

1. **Modularitas**: Roles memungkinkan Anda untuk membagi konfigurasi sistem menjadi bagian-bagian yang lebih kecil dan terorganisir. Setiap role dapat mewakili fungsionalitas atau peran yang berbeda dalam sistem, seperti web server, database server, atau load balancer. Hal ini membuat konfigurasi lebih mudah diorganisir, dipelihara, dan dimodifikasi.

2. **Reusabilitas**: Roles memungkinkan penggunaan kembali konfigurasi yang telah ditentukan. Dengan memisahkan peran dan tanggung jawab masing-masing role, Anda dapat dengan mudah menggabungkan dan menerapkan role yang sama ke host yang berbeda dalam infrastruktur Anda. Ini membantu menghemat waktu dan upaya dalam pengelolaan konfigurasi.

3. **Pembagian tanggung jawab**: Roles memungkinkan tim yang bekerja sama membagi tanggung jawab dalam pengembangan konfigurasi. Setiap anggota tim dapat bertanggung jawab atas pengembangan, pemeliharaan, dan pengujian role tertentu. Dengan demikian, memudahkan kolaborasi dan mempercepat pengembangan infrastruktur.

4. **Pengorganisasian File**: Roles membantu mengorganisasi file dan direktori yang terkait dengan konfigurasi sistem. Setiap role memiliki struktur direktori sendiri, termasuk file-file seperti tugas (tasks), template, file konfigurasi, modul, dan sebagainya. Hal ini membantu mengatur dan membedakan berbagai komponen konfigurasi dalam proyek Anda.

### Studi kasus ansible role

#### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server yang akan di install Apache2. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible.

```bash
lrmn7@ubuntu-controller:~$ mkdir ansible-role
lrmn7@ubuntu-controller:~$ cd ansible-role
```

#### Membuat inventory ansible

Kemudian buat inventory ansible

```bash
vim inventory
```

```bash
[managed]
ubuntu-managed
```

#### Membuat konfigurasi ansible

Silahkan buat konfigurasi ansible sesuai dibawah ini, pastikan usernya menyesuaikan user kalian yang sudah diberi hak akses sudo baik di controller atau managed server:

```bash
vim ansible.cfg
```

```bash
[defaults]
inventory = ./inventory
remote_user = lrmn7
```

#### Membuat roles

Silahkan buat folder roles dengan struktur file seperti dibawah ini

- 📁roles
  - 📁files
    - 📁html
      - 📄index.html
- 📁handlers
  - 📄main.yml
- 📁tasks
  - 📄main.yml
- 📁templates
  - 📄roles.conf.j2

Berikut adalah isi file `roles/file/html/index.html`

```html
Belajar ansible roles
```

Kemudian, ini adalah isi file `roles/handlers/main.yml`

```yml
---
# handlers file for roles
- name: restart apache2
  service:
    name: apache2
    state: restarted
```

Selanjutnya, ini adalah isi file `roles/tasks/main.yml`

```yml
- name: Ensure apache2 is installed
  apt:
    name: apache2
    state: latest

- name: Ensure apache2 is started and enabled
  service:
    name: apache2
    state: started
    enabled: true

- name: Vhost file is installed
  template:
    src: roles/templates/roles.conf.j2
    dest: /etc/apache2/sites-available/roles.conf
    owner: root
    group: root
    mode: 0644

- name: Enable vhost
  command: a2ensite roles.conf
  notify:
    - restart apache2

- name: HTML content is installed in managed hosts
  copy:
    src: roles/files/html/index.html
    dest: "/var/www/html/index.html"
```

Langkah selanjutnya isi file `roles/templates/roles.conf.j2`

```xml
<VirtualHost *:80>
    ServerAdmin webmaster@roles.lrmn7
    ServerName roles.lrmn7
    ErrorLog ${APACHE_LOG_DIR}/roles.lrmn7-error.log
    CustomLog ${APACHE_LOG_DIR}/roles.lrmn7-access.log combined
    DocumentRoot /var/www/html/roles.lrmn7/

    <Directory /var/www/html/roles.lrmn7/>
        Options -Indexes +FollowSymLinks +Includes
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

#### Membuat playbook roles.yml

Silahkan buat file `roles.yml` dan isi seperti dibawah ini

```yml
- name: Use role playbook
  hosts: managed
  become: true
  pre_tasks:
    - name: pre_tasks message
      debug:
        msg: "Ensure web server configuration."

  roles:
    - roles

  post_tasks:
    - name: post_tasks message
      debug:
        msg: "Web server is configured."
```

Jika sudah semua, maka struktur direktorinya akan seperti ini

- 📁roles
  - 📁files
    - 📁html
      - 📄index.html
  - 📁handlers
    - 📄main.yml
  - 📁tasks
    - 📄main.yml
  - 📁templates
    - 📄roles.conf.j2
- 📄ansible.cfg
- 📄inventory
- 📄roles.yml

#### Jalankan ansible playbook

Silahkan jalankan ansible playbook dengan mengetikan perintah

```bash
ansible-playbook roles.yml
```

Maka ansible akan menjalankannya

```bash
lrmn7@ubuntu-controller:~/ansible-role$ ansible-playbook roles.yml

PLAY [Use role playbook] ************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************
ok: [ubuntu-managed]

TASK [pre_tasks message] ************************************************************************************************************************
ok: [ubuntu-managed] => {
    "msg": "Ensure web server configuration."
}

TASK [roles : Ensure apache2 is installed] ******************************************************************************************************
ok: [ubuntu-managed]

TASK [roles : Ensure apache2 is started and enabled] ********************************************************************************************
ok: [ubuntu-managed]

TASK [roles : Vhost file is installed] **********************************************************************************************************
ok: [ubuntu-managed]

TASK [roles : Enable vhost] *********************************************************************************************************************
changed: [ubuntu-managed]

TASK [roles : HTML content is installed in managed hosts] ***************************************************************************************
ok: [ubuntu-managed]

RUNNING HANDLER [roles : restart apache2] *******************************************************************************************************
changed: [ubuntu-managed]

TASK [post_tasks message] ***********************************************************************************************************************
ok: [ubuntu-managed] => {
    "msg": "Web server is configured."
}

PLAY RECAP **************************************************************************************************************************************
ubuntu-managed             : ok=9    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

#### Cek server ubuntu-managed

Langkah terakhir silahkan ketikan perintah

```bash
curl ubuntu-managed
```

Jika tampil seperti dibawah maka tutorial kali ini berhasil

```bash
lrmn7@ubuntu-controller:~/ansible-role$ curl ubuntu-managed
Belajar ansible roles
```
