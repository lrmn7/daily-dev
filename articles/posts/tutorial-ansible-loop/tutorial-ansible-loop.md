**Last updated**: July 10, 2020


### Ansible Loop

Looping dalam Ansible memungkinkan Anda untuk mengulangi serangkaian tugas atau perintah terhadap satu atau lebih host dalam inventory Anda. Ini memungkinkan Anda untuk melakukan operasi yang sama pada banyak host sekaligus dengan mudah.

Kegunaan dari looping dalam Ansible sangat beragam. Beberapa contoh penggunaan yang umum termasuk:

- **Konfigurasi massal**: Anda dapat menggunakan looping untuk mengonfigurasi banyak host sekaligus dengan konfigurasi yang sama.

```yml
---
- name: Konfigurasi Massal
  hosts: all
  tasks:
    - name: Konfigurasi file konfigurasi
      template:
        src: template.conf.j2
        dest: /etc/myapp/config.conf
      loop:
        - host1
        - host2
        - host3
```

- **Penyebaran aplikasi**: Anda dapat menggunakan looping untuk mendistribusikan dan menginstal aplikasi pada beberapa host dalam waktu yang singkat.

```yml
---
- name: Penyebaran Aplikasi
  hosts: app_servers
  tasks:
    - name: Unduh aplikasi dari repositori
      git:
        repo: https://github.com/myapp.git
        dest: /opt/myapp
      loop:
        - app_server1
        - app_server2
        - app_server3
```

- **Pemeliharaan infrastruktur**: Anda dapat menggunakan looping untuk melakukan tugas pemeliharaan rutin, seperti pembaruan perangkat lunak atau pemantauan status host.

<img src="docs/assets/images/ansible-playbook-pemeliharaan-infra.png" alt="Ansible playbook Pemeliharaan infrastruktur">

Dengan fitur looping yang kuat ini, Ansible memungkinkan Anda untuk mengotomatisasi tugas yang kompleks dan mengelola infrastruktur dengan efisiensi yang tinggi.

### Studi kasus ansible loop

#### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible.

```bash
lrmn7@ubuntu-controller:~$ mkdir ansible-loop
lrmn7@ubuntu-controller:~$ cd ansible-loop
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
```

#### Membuat daftar username

Pertama kita buat terlebih dahulu foldernya `group_vars` kemudian silahkan buat file `user-devops.yml` dan isi filenya seperti dibawah ini

- 📁group_vars
  - 📄user-devops.yml

```yml
list_user_dev_managed:
  - dev1
  - dev2
  - dev3
  - dev4
  - dev5

list_user_ops_managed:
  - ops1
  - ops2
  - ops3
  - ops4
  - ops5
```

#### Membuat secret file

Silahkan buat file `secret.yml` dan isi username dan password kalian

```yml
pw: satu2tiga
```

#### Encrypt file secret

Selanjutnya adalah melakukan encrypt file secret yang sebelumnya dibuat, silahkan ketikan command dibawah ini

```bash
lrmn7@ubuntu-controller:~/ansible-loop$ ansible-vault encrypt secret.yml
New Vault password: satu2tiga
Confirm New Vault password:  satu2tiga
Encryption successful
```

#### Membuat ansible playbook

Yang terakhir adalah membuat playbooknya, bisa ikuti scriptnya seperti dibawah ini

```bash
vi devops-loop.yml
```

<img src="docs/assets/images/ansible-playbook-devops-loop.png" alt="Ansible playbook devops-loop.yml">

#### Jalankan playbook

Sebelum menjalankan playbooknya, kalian juga bisa membuat vault password seperti dibawah ini

```bash
lrmn7@ubuntu-controller:~/ansible-loop$ ansible-playbook --syntax-check --ask-vault-pass devops-loop.yml
Vault password: satu2tiga

playbook: devops-loop.yml
lrmn7@ubuntu-controller:~/ansible-loop$ echo "satu2tiga" > rahasia
lrmn7@ubuntu-controller:~/ansible-loop$ chmod 600 rahasia
```

Masukan `rahasia` sebagai vault passwordnya, sehingga kita tidak perlu memasukan kata sandi ketika menjalankan ansible-nya

```bash
ansible-playbook --vault-password-file=rahasia devops-loop.yml
```

Jika berhasil maka ansible akan menampilkan pesan seperti dibawah ini

```bash
lrmn7@ubuntu-controller:~/ansible-loop$ ansible-playbook --vault-password-file=rahasia devops-loop.yml

PLAY [Create user account ubuntu-managed] *******************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************
ok: [ubuntu-managed]

TASK [Creating user dev1-5] *********************************************************************************************************************
[DEPRECATION WARNING]: Encryption using the Python crypt module is deprecated. The Python crypt module is deprecated and will be removed from
Python 3.13. Install the passlib library for continued encryption functionality. This feature will be removed in version 2.17. Deprecation
warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
changed: [ubuntu-managed] => (item=dev1)
changed: [ubuntu-managed] => (item=dev2)
changed: [ubuntu-managed] => (item=dev3)
changed: [ubuntu-managed] => (item=dev4)
changed: [ubuntu-managed] => (item=dev5)

TASK [Creating user ops1-5] *********************************************************************************************************************
[DEPRECATION WARNING]: Encryption using the Python crypt module is deprecated. The Python crypt module is deprecated and will be removed from
Python 3.13. Install the passlib library for continued encryption functionality. This feature will be removed in version 2.17. Deprecation
warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
changed: [ubuntu-managed] => (item=ops1)
changed: [ubuntu-managed] => (item=ops2)
changed: [ubuntu-managed] => (item=ops3)
changed: [ubuntu-managed] => (item=ops4)
changed: [ubuntu-managed] => (item=ops5)

PLAY RECAP **************************************************************************************************************************************
ubuntu-managed             : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

#### Mengecek semua login username dengan sshpass

Pertama install terlebih dahulu sshpass `sudo apt install sshpass`

Kemudian buat script dibawah ini

```bash
vi test-user.sh
```

Kemudian masukan filenya dibawah ini

```bash
#!/bin/bash

users=("dev1" "dev2" "dev3" "dev4" "dev5" "ops1" "ops2" "ops3" "ops4" "ops5")
hostname="ubuntu-managed"
password="satu2tiga"

for user in "${users[@]}"
do
    sshpass -p "$password" ssh "$user@$hostname" whoami
done
```

Setelah itu berikan izin untuk mengeksekusi script dengan command dibawah ini:

```bash
chmod +x test-user.sh
```

Terakhir jalankan script tersebut, jika berhasil akan tampil seperti dibawah

```bash
lrmn7@ubuntu-controller:~/ansible-loop$ ./test-user.sh
dev1
dev2
dev3
dev4
dev5
ops1
ops2
ops3
ops4
ops5
```
