**Last updated**: June 5, 2020


### Apa itu Jinja 2

Jinja2 adalah sebuah library template engine untuk Python yang digunakan untuk mempermudah pembuatan template file dengan menggabungkan struktur file dengan data yang akan digunakan pada file tersebut.

> Dalam konteks Ansible, Jinja2 digunakan sebagai mekanisme template engine untuk menghasilkan file konfigurasi yang dibutuhkan pada host yang sedang dielola. Hal ini memungkinkan pengguna Ansible untuk membuat file konfigurasi yang lebih dinamis, yang dapat berubah sesuai dengan variabel yang didefinisikan dalam playbook Ansible.

Jinja2 sangat berguna dalam membuat file konfigurasi yang kompleks dan dapat digunakan dengan banyak format file seperti .yml, .json, .ini, .xml, dan lain-lain. Pada dasarnya, Jinja2 memungkinkan kita untuk memisahkan konfigurasi dari kode, sehingga dapat memudahkan dalam pemeliharaan dan pengaturan konfigurasi yang lebih dinamis pada infrastruktur IT.

### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server yang akan di install Apache. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible dan konfigurasi apache.

```bash
lrmn7@ubuntu-controller:~$ mkdir ansible-jinja2
lrmn7@ubuntu-controller:~$ cd ansible-jinja2/
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

#### Membuat inventory ansible

Kemudian buat inventory ansible

```bash
vim inventory
```

```bash
[webservers]
ubuntu-managed2
```

#### File index.html untuk Apache2

Jika sudah selanjutnya buat file yang akan digunakan untuk apache2 sebagai bukti kalau kita sudah berhasil menginstall apache2

```bash
mkdir files
cd files
echo "Hello world! This is from Ansible x Jinja2 Template." > index.html.j2
```

### Membuat ansible playbook

Langkah terkahir adalah dengan membuat ansible playbook, disini kita menggunaka 3 module yaitu apt, copy dan service.

```bash
vim site.yml
```

```yml
---
- name: install and start apache2
  hosts: webservers
  become: true

  tasks:
    - name: ensure apache2 package is present
      apt:
        name: apache2
        state: present
        update_cache: yes
        force_apt_get: yes

    - name: restart apache2 service
      service: name=apache2 state=restarted enabled=yes

    - name: copy index.html
      template: src=files/index.html.j2 dest=/var/www/html/index.html
```

#### Mengecek ansible playbook

Ansible playbook yang sudah kalian tulis lebih baik di cek terlebih dahulu apakah terdapat error atau tidak dengan menggunakan command dibawah ini:

```bash
ansible-playbook --syntax-check site.yml
```

#### Menjalankan ansible playbook

Jika playbook tidak ada yang error, langkah selanjutnya adalah menjalankannya

```bash
ansible-playbook -i inventory site.yml
```

Jika berhasil maka akan tampil seperti dibawah ini

```bash
lrmn7@ubuntu-controller:~/ansible-jinja2$ ansible-playbook -i inventory site.yml

PLAY [Install & Start apache2 menggunakan Jinja2 Template] *********************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [ubuntu-managed2]

TASK [ensure apache2 package is installed or present] **************************************************************************************************
changed: [ubuntu-managed2]

TASK [restart apache2 service] *************************************************************************************************************************
changed: [ubuntu-managed2]

TASK [copy index.html.j2 template] *********************************************************************************************************************
changed: [ubuntu-managed2]

PLAY RECAP *********************************************************************************************************************************************
ubuntu-managed2            : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Mengecek Apache2 di Managed Server

Jika ansible sudah dijalankan maka tertulis changed di ubuntu managed2 untuk task install dan copy, itu berarti bisa saja apache sudah diinstall dan file index.html.j2 sudah di copy ke folder configurasi apache2. Langkah selanjutnya cek managed servernya.

```bash
curl ubuntu-managed2
```

Jika tampil seperti dibawah, maka kita telah berhasil menginstall apache2 menggunakan ansible.

```bash
lrmn7@ubuntu-controller:~/ansible-jinja2$ curl ubuntu-managed2
Hello world! This is from Ansible x Jinja2 Template.
```

Kita juga dapat melakukan port forwarding ubuntu-managed2 untuk melihatnya di laptop host kita dengan membukanya di browser, caranya sudah dituliskan di artikel sebelumnya.
