**Last updated**: June 12, 2020


### Apa itu Ansible Secret ?

Ansible Secret adalah sebuah fitur yang memungkinkan Anda untuk mengelola dan menggunakan data rahasia secara aman dalam konfigurasi dan tugas Ansible. Dalam konteks Ansible, data rahasia biasanya mencakup kata sandi, kunci SSH, sertifikat, token API, atau data sensitif lainnya yang tidak boleh terlihat oleh semua orang yang memiliki akses ke konfigurasi atau playbook Ansible Anda.

### Fungsi/Kegunaan Ansible Secret

Kegunaan utama dari Ansible Secret adalah untuk menjaga kerahasiaan data sensitif saat melakukan otomatisasi dengan Ansible. Dengan menggunakan Ansible Secret, Anda dapat menyimpan data rahasia secara terenkripsi dan menggunakannya dalam playbook Anda tanpa harus mengeksposnya secara langsung di dalam file konfigurasi. Ini membantu melindungi informasi sensitif dari akses yang tidak sah atau penyalahgunaan.

Ansible Secret memanfaatkan enkripsi simetris menggunakan kunci enkripsi yang dikelola oleh Ansible Vault. Ansible Vault adalah utilitas bawaan dalam Ansible yang memungkinkan Anda membuat file terenkripsi yang berisi data rahasia. Anda dapat menggunakan Ansible Vault untuk membuat, mengedit, dan membuka file-file ini menggunakan kata sandi atau kunci enkripsi.

### Studi kasus ansible secret

#### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible.

```bash
lrmn7@ubuntu-controller:~$ mkdir ansible-secret
lrmn7@ubuntu-controller:~$ cd ansible-secret
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

#### Membuat secret file

Silahkan buat file `secret.yml` dan isi username dan password kalian

```yml
username: administrator
pw: satu2tiga
```

#### Encrypt file secret

Selanjutnya adalah melakukan encrypt file secret yang sebelumnya dibuat, silahkan ketikan command dibawah ini

```bash
lrmn7@ubuntu-controller:~/ansible-secret$ ansible-vault encrypt secret.yml
New Vault password: satu2tiga
Confirm New Vault password:  satu2tiga
Encryption successful
```

#### Membuat ansible playbook

Yang terakhir adalah membuat playbooknya, bisa ikuti scriptnya seperti dibawah ini

```bash
vi secret-book.yml
```

<img src="docs/assets/images/ansible-playbook-secret-book.png" alt="Ansible playbook secret-book.yml">

#### Jalankan playbook

Sebelum menjalankan playbooknya, kalian juga bisa membuat vault password seperti dibawah ini

```bash
lrmn7@ubuntu-controller:~/ansible-secret$ ansible-playbook --syntax-check --ask-vault-pass secret-book.yml
Vault password: satu2tiga

playbook: secret-book.yml
lrmn7@ubuntu-controller:~/ansible-secret$ echo "satu2tiga" > rahasia
lrmn7@ubuntu-controller:~/ansible-secret$ chmod 600 rahasia
```

Masukan `rahasia` sebagai vault passwordnya, sehingga kita tidak perlu memasukan kata sandi ketika menjalankan ansible-nya

```bash
ansible-playbook --vault-password-file=rahasia secret-book.yml
```

Jika berhasil akan menampilkan seperti dibawah ini

```bash
lrmn7@ubuntu-controller:~/ansible-secret$ ansible-playbook --vault-password-file=rahasia secret-book.yml

PLAY [create user accounts for all our servers] *****************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************
ok: [ubuntu-managed]

TASK [Creating user from secret.yml] ****************************************************************************************************************
[DEPRECATION WARNING]: Encryption using the Python crypt module is deprecated. The Python crypt module is deprecated and will be removed from Python
 3.13. Install the passlib library for continued encryption functionality. This feature will be removed in version 2.17. Deprecation warnings can be
 disabled by setting deprecation_warnings=False in ansible.cfg.
changed: [ubuntu-managed]

PLAY RECAP ******************************************************************************************************************************************
ubuntu-managed             : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

lrmn7@ubuntu-controller:~/ansible-secret$
```

#### Login user yang sudah dibuat

Untuk mengecek apakah kita berhasil membuat username dan password, kalian bisa mengeceknya dengan login user yang sudah dibuat tadi

```bash
lrmn7@ubuntu-controller:~/ansible-secret$ ssh administrator@ubuntu-managed
administrator@ubuntu-managed's password: satu2tiga
```

Jika berhasil, maka kalian akan masuk sebagai administrator

```bash
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-69-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Jun 10 11:12:31 UTC 2023

  System load:  0.0390625         Processes:               94
  Usage of /:   4.6% of 38.70GB   Users logged in:         0
  Memory usage: 19%               IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%                IPv4 address for enp0s8: 10.10.10.22

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

 * Introducing Expanded Security Maintenance for Applications.
   Receive updates to over 25,000 software packages with your
   Ubuntu Pro subscription. Free for personal use.

     https://ubuntu.com/pro

Expanded Security Maintenance for Applications is not enabled.

33 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

$ whoami
administrator
$ hostname
ubuntu-managed
$ exit
```
