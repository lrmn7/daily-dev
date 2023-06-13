**Last updated**: April 20, 2020


### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server yang akan di install Nginx. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible.
```bash
lrmn7@ubuntu-controller:~$ mkdir latihan-ansible-3
lrmn7@ubuntu-controller:~$ cd latihan-ansible-3
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
host_key_checking = False
```

#### Membuat inventory ansible

Kemudian buat inventory ansible
```bash
vim inventory
```
```bash
[latihan3ansible]
ubuntu-managed1
```
#### Membuat Jinja2 Template

Langkah selanjutnya adalah membuat Jinja2 Template untuk Nginx dan MariaDB
```bash
vim mariadb.list.j2
```
Kemdudian di `mariadb.list.j2` silahkan masukkan text dibawah ini
```bash
deb https://download.nus.edu.sg/mirror/mariadb/repo/10.9/ubuntu jammy main
```
Setelah itu lanjut ke nginx
```bash
vim nginx.list.j2
```
Isi `nginx.list.j2` seperti dibawah ini
```bash
deb http://nginx.org/packages/mainline/ubuntu/ jammy nginx
```

### Membuat ansible playbook

Langkah terkahir adalah dengan membuat ansible playbook, disini kita menggunaka 3 module yaitu apt, copy dan service.
```bash
vim latihan3.yml
```
```yml
---
- name: Praktik Install Nginx dan Mariadb Menggunakan Jinja2 Template di Playbook
  hosts: latihan3ansible
  become: true
  tasks:
    - name: Add nginx repo
      template:
        src: nginx.list.j2
        dest: /etc/apt/sources.list.d/nginx.list

    - name: Add maridb 10.9 repo
      template:
        src: mariadb.list.j2
        dest: /etc/apt/sources.list.d/mariadb.list

    - name: Apt key nginx
      ansible.builtin.apt_key:
        url: https://nginx.org/keys/nginx_signing.key
        state: present
    - name: Apt key mariadb
      ansible.builtin.apt_key:
        url: https://mariadb.org/mariadb_release_signing_key.asc
        state: present

    - name: Update repo
      apt:
        update_cache: true
        cache_valid_time: 3600
        force_apt_get: true

    - name: Install nginx
      apt:
        name: nginx=1.23.1-1~jammy
        update_cache: yes
        force_apt_get: yes

    - name: Starting the nginx service
      service: name=nginx state=started enabled=yes

    - name: Install mariadb
      apt:
        name:
           - mariadb-server-10.9
           - mariadb-client-10.9
        update_cache: yes
        force_apt_get: yes

    - name: Starting the mariadb server
      service: name=mariadb state=started enabled=yes
```

#### Mengecek ansible playbook

Ansible playbook yang sudah kalian tulis lebih baik di cek terlebih dahulu apakah terdapat error atau tidak dengan menggunakan command dibawah ini:
```bash
ansible-playbook --syntax-check latihan3.yml
```

#### Menjalankan ansible playbook

Jika playbook tidak ada yang error, langkah selanjutnya adalah menjalankannya
```bash
ansible-playbook -i inventory latihan3.yml
```

Jika berhasil maka akan tampil seperti dibawah ini
```bash
lrmn7@ubuntu-controller:~/latihan-ansible-3$ ansible-playbook -i inventory latihan3.yml

PLAY [Praktik Install Nginx dan Mariadb Menggunakan Jinja2 Template di Playbook] ***********************************

TASK [Gathering Facts] *********************************************************************************************
ok: [ubuntu-managed1]

TASK [Add nginx repo] **********************************************************************************************
ok: [ubuntu-managed1]

TASK [Add maridb 10.9 repo] ****************************************************************************************
ok: [ubuntu-managed1]

TASK [Apt key nginx] ***********************************************************************************************
ok: [ubuntu-managed1]

TASK [Apt key mariadb] *********************************************************************************************
ok: [ubuntu-managed1]

TASK [Update repo] *************************************************************************************************
changed: [ubuntu-managed1]

TASK [Install nginx] ***********************************************************************************************
ok: [ubuntu-managed1]

TASK [Start the nginx service] *************************************************************************************
ok: [ubuntu-managed1]

TASK [Install mariadb] *********************************************************************************************
ok: [ubuntu-managed1]

TASK [Start the mariadb server] ************************************************************************************
ok: [ubuntu-managed1]

PLAY RECAP *********************************************************************************************************
ubuntu-managed1            : ok=10   changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  

```

> Karena saya sudah beberapa kali running playbook ini, maka beberapa task yang seharusnya sekali jalan seperti menambahkan repository akan berubah menjadi changed. Namun karena ada beberapa config ansible playbook yang harus saya perbaiki maka task tersebut yang sebelumnya changed ketika di running kembali akan berubah menjadi ok.

### Mengecek Nginx di Managed Server

Jika ansible sudah dijalankan maka tertulis changed atau ok di ubuntu-managed1 untuk task install dan copy. Langkah selanjutnya cek managed servernya. Bisa diikuti dengan mengetikan command dibawah ini.
```bash
curl http://ubuntu-managed1/
```

Jika tampil seperti dibawah, maka kita telah berhasil menginstall nginx menggunakan ansible.
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
Kita juga dapat melakukan port forwarding ubuntu-managed1 untuk melihatnya di laptop host kita dengan membukanya di browser, caranya sudah dituliskan di artikel sebelumnya.

<img src="docs/assets/images/Nginx-MariaDB-Jinja2.png" alt="Praktik: Install Nginx dan Mariadb Menggunakan Jinja2 Template di Playbook">