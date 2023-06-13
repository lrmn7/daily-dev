**Last updated**: April 6, 2020


### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server yang akan di install Apache. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible dan konfigurasi apache.
```bash
mkdir -p ansible-apache2/files
cd ansible-apache2
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
```

#### Membuat inventory ansible

Kemudian buat inventory ansible
```bash
vim inventory
```
```bash
[web]
ubuntu-managed1
```

#### File index.html untuk Apache2

Jika sudah selanjutnya buat file yang akan digunakan untuk apache2 sebagai bukti kalau kita sudah berhasil menginstall apache2
```bash
echo "Hello world! This is from Ansible x Apache2." > files/index.html
```

### Membuat ansible playbook

Langkah terkahir adalah dengan membuat ansible playbook, disini kita menggunaka 3 module yaitu apt, copy dan service.
```bash
vim site.yml
```
```yml
---
- name: Install and start Apache2
  hosts: web
  become: true
  tasks:
    - name: Check/Install apache2 package is present
      apt: name=apache2 state=present

    - name: Copy index.html file / correct index.html is present
      copy:
        src: ./files/index.html
        dest: /var/www/html/index.html

    - name: Ensure Apache 2 is started
      service:
        name: apache2
        state: started
        enabled: true
```

#### Mengecek ansible playbook

Ansible playbook yang sudah kalian tulis lebih baik di cek terlebih dahulu apakah terdapat error atau tidak dengan menggunakan command dibawah ini:
```bash
ansible-playbook --syntax-check site.yml
```

#### Menjalankan ansible playbook

Jika playbook tidak ada yang error, langkah selanjutnya adalah menjalankannya
```bash
ansible-playbook site.yml
```

Jika berhasil maka akan tampil seperti dibawah ini
```bash
lrmn@ubuntu-controller:~/ansible-apache2$ ansible-playbook site.yml

PLAY [Install and start Apache2] ********************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************
ok: [ubuntu-managed1]

TASK [Check/Install apache2 package is present] *****************************************************************************
changed: [ubuntu-managed1]

TASK [Copy index.html file / correct index.html is present] *****************************************************************
changed: [ubuntu-managed1]

TASK [Ensure Apache 2 is started] *******************************************************************************************
ok: [ubuntu-managed1]

PLAY RECAP ******************************************************************************************************************
ubuntu-managed1            : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

### Mengecek Apache2 di Managed Server

Jika ansible sudah dijalankan maka tertulis changed di ubuntu managed1 untuk task install dan copy, itu berarti bisa saja apache sudah diinstall dan file index.html sudah di copy ke folder configurasi apache2. Langkah selanjutnya cek managed servernya.
```bash
curl ubuntu-managed1
```

Jika tampil seperti dibawah, maka kita telah berhasil menginstall apache2 menggunakan ansible.
```bash
lrmn@ubuntu-controller:~/ansible-apache2$ curl ubuntu-managed1
Hello world! This is from Ansible x Apache2.
```
Kita juga dapat melakukan port forwarding ubuntu-managed1 untuk melihatnya di laptop host kita dengan membukanya di browser, caranya sudah dituliskan di artikel sebelumnya.
