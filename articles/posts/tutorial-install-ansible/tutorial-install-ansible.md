**Last updated**: September 27, 2020


### Pengenalan Ansible

> Ansible adalah alat open source yang digunakan untuk mengotomatisasi tugas-tugas IT seperti manajemen konfigurasi, penyebaran aplikasi, dan otomatisasi tugas-tugas rutin. Ansible memungkinkan Anda untuk mengotomatisasi tugas-tugas IT di seluruh lingkungan, dari server lokal hingga server cloud dan lingkungan hybrid.

Dalam istilah yang lebih sederhana, Ansible adalah alat yang dapat membantu Anda mengelola banyak server secara efisien, sehingga Anda tidak perlu menghabiskan banyak waktu dan upaya dalam melakukan tugas-tugas manual seperti menginstal perangkat lunak, mengonfigurasi sistem, dan melakukan tugas-tugas serupa pada banyak server yang berbeda.

### Persyaratan instalasi sistem

Berikut adalah persyaratan sistem minimum yang diperlukan untuk menginstal Ansible:

1. **Sistem Operasi**: Ansible dapat diinstal pada sistem operasi berbasis Linux, macOS, atau Windows. Namun, sebagian besar pengguna menggunakan sistem operasi Linux, seperti Ubuntu, Debian, CentOS, atau Red Hat Enterprise Linux (RHEL).

2. **Python**: Ansible ditulis dalam bahasa Python, sehingga Python harus terinstal pada sistem sebelum menginstal Ansible. Versi Python yang disarankan adalah Python 3.x. Namun, beberapa versi Ansible masih memerlukan Python 2.x.

3. **Memory**: Ansible cukup ringan dan tidak memerlukan banyak sumber daya sistem. Namun, pastikan sistem memiliki setidaknya 1 GB RAM atau lebih.

4. **Storage**: Ansible tidak memerlukan banyak penyimpanan, tetapi pastikan ada cukup ruang kosong pada sistem untuk menginstal dan menjalankan playbook.

5. **Akses SSH**: Ansible menggunakan protokol SSH untuk berkomunikasi dengan host. Pastikan host yang akan dielola sudah memiliki akses SSH yang diperbolehkan.

6. **Akun dengan akses root atau sudo**: Ansible memerlukan akses ke root atau sudo untuk menjalankan tugas-tugas manajemen konfigurasi pada host. Pastikan akun yang digunakan untuk mengelola host memiliki akses root atau sudo yang diperbolehkan.

Demikianlah persyaratan sistem minimum yang perlu dipenuhi sebelum menginstal Ansible pada sistem. Pastikan untuk memenuhi persyaratan ini agar dapat menginstal dan menjalankan Ansible dengan baik pada sistem.

Selain itu disini saya sudah menyiapkan 3 buah server, 1 server sebagi controller dan 2 sebagai managed server yang nanti beberapa job/task dijalankan dari controller. Server saya buat dengan Vagrant, ada di tutorial saya sebelumnya. Berikut konfigurasi Vagrant yang saya buat. 

```ruby
Vagrant.configure("2") do |config|
 config.vm.box = "ubuntu/jammy64"
 config.vm.boot_timeout = 900

 config.vm.provider "virtualbox" do |vb|
     vb.gui = false
     vb.memory = "1024"
     vb.cpus = 1
     vb.linked_clone = true
 end

 config.vm.define "ubuntu-controller" do |controller|
     controller.vm.network :private_network, ip: "10.10.10.10"
 end
 config.vm.define "ubuntu-managed1" do |managed1|
     managed1.vm.network :private_network, ip: "10.10.10.20"
 end
  config.vm.define "ubuntu-managed2" do |managed2|
     managed2.vm.network :private_network, ip: "10.10.10.30"
 end

end
```

Kemudian dari tiap-tiap user, sudah diberikan hak akses root dan dapat login antar server secara passwordless. Tutorial juga ada di artikel sebelumnya.

### Tutorial Install Ansible

Masuk ke server controller dan ikuti tutorial dibawah ini.

#### Install required package yang diperlukan
```bash
sudo apt update
sudo apt install -y software-properties-common
```

#### Tambahkan PPA Ansible ke controller server
```bash
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible=7*
ansible --version
```

#### Buat default konfigurasi ansiblenya
```bash
sudo mkdir -p /etc/ansible
sudo vim /etc/ansible/hosts
```

didalam file hosts, silahkan masukan 2 server host yang akan di managed dari server controller. Tulis dibagian paling bawah saja agar kita bisa melihat konfigurasi yang sudah diberikan oleh ansible sebagai contoh.
```bash
ubuntu-managed1

[belajaransible]
ubuntu-managed2
```
Diatas, saya masukan 2 server atau bisa diganti juga dengan menggunakan ip server. 
* ubuntu-managed1 : diluar grup
* ubuntu-managed2 : didalam grup
Jadi ansible dapat menjalankan job/tasks berdasarkan grup.

#### Menampilkan keseluruhan inventory host
Berikut perintahnya:
```bash
ansible all --list-hosts
```
Maka akan menampilkan keseluruhannya seperti dibawah ini:
```bash
febrian@ubuntu-controller:~$ ansible all --list-hosts
  hosts (2):
    ubuntu-managed1
    ubuntu-managed2
```
#### Menampilkan host yang tidak masuk dalam grup
Berikut perintahnya:
```bash
ansible ungrouped --list-hosts
```
Maka hanya akan tampil, ubuntu-managed1 seperti dibawah ini:
```bash
febrian@ubuntu-controller:~$ ansible ungrouped --list-hosts
  hosts (1):
    ubuntu-managed1
```
#### Menampilkan host yang masuk ke dalam grup
Berikut perintahnya:
```bash
ansible belajaransible --list-hosts
```
Maka akan menampilkan ubuntu-managed2 seperti dibawah ini:
```bash
febrian@ubuntu-controller:~$ ansible belajaransible --list-hosts
  hosts (1):
    ubuntu-managed2
```
#### Mengecek keseluruhan host di ansible
Berikut perintahnya:
```
ansible all -m ping
```
Dengan menjalankan command `ansible all -m ping`, Ansible akan melakukan koneksi ke semua host yang terdaftar dalam inventory dan mengembalikan pong jika koneksi berhasil. 

> Command ini sering digunakan untuk memeriksa apakah host yang terdaftar dalam inventory dapat diakses dan terhubung dengan baik menggunakan protokol SSH.

Jika berhasil maka akan menampilkan seperti dibawah ini:
```bash
febrian@ubuntu-controller:~$ ansible all -m ping
ubuntu-managed2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
ubuntu-managed1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```



