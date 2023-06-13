**Last updated**: April 1, 2020


### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server yang akan di install Apache. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible dan konfigurasi apache.
```bash
lrmn@ubuntu-controller:~$ mkdir latihan-ansible
lrmn@ubuntu-controller:~$ cd latihan-ansible/
```

#### Membuat konfigurasi ansible

Silahkan buat konfigurasi ansible sesuai dibawah ini, pastikan usernya menyesuaikan user kalian yang sudah diberi hak akses sudo baik di controller atau managed server:
```bash
vim ansible.cfg
```
```bash
[defaults]
inventory = ./inventory
remote_user = lrmn
host_key_checking = False
```

#### Membuat inventory ansible

Kemudian buat inventory ansible
```bash
vim inventory
```
```bash
[latihan1ansible]
ubuntu-managed1
```

### Membuat ansible playbook

Langkah terkahir adalah dengan membuat ansible playbook, disini kita menggunaka 3 module yaitu apt, copy dan service.
```bash
vim latihan1.yml
```
```yml
---
- name: Install Apache2, MariaDB, PHP dengan Ansible Playbook
  hosts: latihan1ansible
  become: true
  become_user: root
  remote_user: lrmn
  tasks:
    - name: Install apache2
      apt:
        name: apache2
        update_cache: yes
        state: latest
        force_apt_get: yes
    - name: Install mariadb-server
      apt:
        name: mariadb-server
        update_cache: yes
        state: latest
        force_apt_get: yes
    - name: Install php
      apt:
        name: php
        update_cache: yes
        state: latest
        force_apt_get: yes
    - name: Install php-mysql
      apt:
        name: php-mysql
        update_cache: yes
        state: latest
        force_apt_get: yes
    - name: Ensure apache2 is running
      service:
        name: apache2
        state: started
        enabled: true
    - name: Ensure mariadb is running
      service:
        name: mariadb
        state: started
        enabled: true
    - name: Generate web content
      copy:
        content: "Praktik: Install Apache2, MariaDB, PHP dengan Ansible Playbook"
        dest: /var/www/html/index.php

- name: Verify apache
  hosts: localhost
  tasks:
    - name: Tests the web service is running
      uri:
        url: http://ubuntu-managed1/index.php
        status_code: 200
        return_content: yes
```

#### Mengecek ansible playbook

Ansible playbook yang sudah kalian tulis lebih baik di cek terlebih dahulu apakah terdapat error atau tidak dengan menggunakan command dibawah ini:
```bash
ansible-playbook --syntax-check latihan1.yml
```

#### Menjalankan ansible playbook

Jika playbook tidak ada yang error, langkah selanjutnya adalah menjalankannya
```bash
ansible-playbook -i inventory latihan1.yml
```

Jika berhasil maka akan tampil seperti dibawah ini
```bash
lrmn@ubuntu-controller:~/latihan-ansible$ ansible-playbook -i inventory latihan1.yml

PLAY [Install Apache2, MariaDB, PHP dengan Ansible Playbook] *******************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [ubuntu-managed1]

TASK [Install apache2] *********************************************************************************************
ok: [ubuntu-managed1]

TASK [Install mariadb-server] **************************************************************************************
changed: [ubuntu-managed1]

TASK [Install php] *************************************************************************************************
changed: [ubuntu-managed1]

TASK [Install php-mysql] *******************************************************************************************
changed: [ubuntu-managed1]

TASK [Ensure apache2 is running] ***********************************************************************************
ok: [ubuntu-managed1]

TASK [Ensure mariadb is running] ***********************************************************************************
ok: [ubuntu-managed1]

TASK [Generate web content] ****************************************************************************************
changed: [ubuntu-managed1]

PLAY [Verify apache] ***********************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [localhost]

TASK [Tests the web service is running] ****************************************************************************
ok: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
ubuntu-managed1            : ok=8    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

Karena sebelumnya di ubuntu-managed1 Apache sudah di install, maka Task Install Apache jika sudah pernah akan menjadi `ok` bukan `changed`.

### Mengecek Apache2 di Managed Server

Jika ansible sudah dijalankan maka tertulis changed di ubuntu managed1 untuk task install dan copy, itu berarti bisa saja apache sudah diinstall dan file index.html.j2 sudah di copy ke folder configurasi apache2. Langkah selanjutnya cek managed servernya.
```bash
curl http://ubuntu-managed1/index.php
```

Jika tampil seperti dibawah, maka kita telah berhasil menginstall apache2 menggunakan ansible.
```bash
lrmn@ubuntu-controller:~/latihan-ansible$ curl http://ubuntu-managed1/index.php
Praktik: Install Apache2, MariaDB, PHP dengan Ansible Playbook
```
Kita juga dapat melakukan port forwarding ubuntu-managed1 untuk melihatnya di laptop host kita dengan membukanya di browser, caranya sudah dituliskan di artikel sebelumnya.

<img src="/docs/assets/images/Port-Forwarding-Ansible-Apache-Variable.png" alt="Praktik: Install Apache2, MariaDB, PHP dengan Ansible Playbook">