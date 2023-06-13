**Last updated**: March 14, 2020


### Bekenalan dengan Vagrant

#### Apa itu Vagrant ?

Sebelum mengetahui lebih lanjut bagaimana cara menginstall Vagrant, kita bahas terlebih dahulu apa kegunaan Vagrant.
Menurut website resminya, Vagrant adalah
> Vagrant is a tool for building and managing virtual machine environments in a single workflow. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases production parity, and makes the “works on my machine” excuse a relic of the past.

Singkatnya Vagrant adalah alat open-source yang membantu kita untuk mengotomatisasi pembuatan dan pengelolaan Mesin Virtual. Singkatnya, kita dapat menentukan konfigurasi mesin virtual dalam file konfigurasi sederhana, dan Vagrant menciptakan mesin Virtual yang sama hanya dengan menggunakan satu perintah sederhana. Vagrant menyediakan antarmuka baris perintah untuk mengotomatisasi tugas-tugas tersebut. 

#### Kenapa harus menggunakan Vagrant ?

Sebuah aplikasi terdiri dari beberapa komponen yang perlu dikonfigurasi dengan dapat berjalan dengan baik. Sebagai contoh, sebuah aplikasi web modern mungkin memiliki beberapa komponen-komponen seperti Java, JavaScript, Python, dll sebagai bahasa, MySQL, Oracle, MongoDB, dll sebagai Database, komponen-komponen lain seperti webserver, load-balancer, API Gateway, Message Queue, dll berdasarkan kebutuhan.  

Sebelum Vagrant, semua komponen diatas perlu diatur secara manual. Selama proses setup, banyak masalah yang dihadapi diantaranya:

 * Di setiap mesin, setup perlu dilakukan secara terpisah, yang memakan banyak waktu.
 * Konfigurasi manual mungkin salah, yang perlu di-debug dan diperbaiki setiap saat.
 * Lingkungan pengembangan, pengujian, dan produksi harus identik. Tetapi karena instalasi manual dan setup komponen ini, mungkin ada sedikit perbedaan yang membuat kita sangat kesusahan, karena, dalam skenario seperti itu, aplikasi mungkin berjalan di lingkungan pengembangan, tetapi menghadapi masalah di lingkungan produksi.

> Selain beberapa hal diatas, Tools ini sering saya gunakan untuk latihan beberapa tutorial mengenai DevOps yang mana kadang kita membutuhkan sebuah VPS, namun dengan Vagrant kita bisa mengikuti tutorial tersebut tanpa mengeluarkan banyak biaya. Selain itu virtualisanya dapat kita lakukan ssh dan tunneling di browser host. 

#### Prasyarat menggunakan Vagrant

1. Install aplikasi virtualisasi seperti; VirtualBox, VMware, atau Hyper-V. Disini karena saya menggunakan Windows 10 saya menginstall VirtualBox karena mudah dan juga gratis.

2. Jika sudah, silahkan download Vagrant di website resmi, linknya ada disini. <https://developer.hashicorp.com/vagrant/downloads>

3. Setelah itu, verikasi dengan cara mengetikan command dibawah ini pada CMD. Jika berhasil maka akan tampil versi Vagrant yang sudah kita Install.
```bash
C:\Users\User>vagrant -v
Vagrant 2.3.1
```

### Tutorial Membuat Instance/VM di Vagrant

#### Konfigurasi Vagrant file
Di studi kasus ini saya membutuhkan sebuah server dengan distribusi Ubuntu 22.04 dengan spesifikasi rendah misal memory hanya 512Mb dan 1 Core, maka berikut tutorial singkatnya.

1. Silahkan buat folder di direktori mu, dan masuk via CMD.
```bash
D:\Belajar-Vagrant>
```
2. Kemudian silahkan buat 1 Vagrant file. Berikut isi configurasinya.
```ruby
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/jammy64"

    config.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.memory = "512"
        vb.cpus = 1
        vb.linked_clone = true
    end

    config.vm.define "server-satu" do |server1|
        server1.vm.network :private_network, ip: "10.10.10.11"
    end

    config.vm.provision "shell", inline: <<-SHELL
        apt-get update
    SHELL
end
```
3. Jika sudah disilahkan ketikan `vagrant up`, maka vagrant akan create secara otomatis instance/vm di virtualbox kalian dengan mendownload Boxes yang tersedia di Vagrant. Boxes ini bisa dikatakan seperti Docker images.
```bash
D:\Belajar-Vagrant>vagrant up
Bringing machine 'server-satu' up with 'virtualbox' provider...
==> server-satu: Cloning VM...
==> server-satu: Matching MAC address for NAT networking...
==> server-satu: Checking if box 'ubuntu/jammy64' version '20221110.0.0' is up to date...
==> server-satu: Setting the name of the VM: Belajar-Vagrant_server-satu_1669043874779_68105
==> server-satu: Clearing any previously set network interfaces...
==> server-satu: Preparing network interfaces based on configuration...
    server-satu: Adapter 1: nat
    server-satu: Adapter 2: hostonly
==> server-satu: Forwarding ports...
    server-satu: 22 (guest) => 2222 (host) (adapter 1)
==> server-satu: Running 'pre-boot' VM customizations...
==> server-satu: Booting VM...
==> server-satu: Waiting for machine to boot. This may take a few minutes...
    server-satu: SSH address: 127.0.0.1:2222
    server-satu: SSH username: vagrant
    server-satu: SSH auth method: private key
    server-satu: Warning: Connection reset. Retrying...
    server-satu:
    server-satu: Vagrant insecure key detected. Vagrant will automatically replace
    server-satu: this with a newly generated keypair for better security.
    server-satu:
    server-satu: Inserting generated public key within guest...
    server-satu: Removing insecure key from the guest if it's present...
    server-satu: Key inserted! Disconnecting and reconnecting using new SSH key...
==> server-satu: Machine booted and ready!
==> server-satu: Checking for guest additions in VM...
    server-satu: The guest additions on this VM do not match the installed version of
    server-satu: VirtualBox! In most cases this is fine, but in rare cases it can
    server-satu: prevent things such as shared folders from working properly. If you see
    server-satu: shared folder errors, please make sure the guest additions within the
    server-satu: virtual machine match the version of VirtualBox you have installed on
    server-satu: your host and reload your VM.
    server-satu:
    server-satu: Guest Additions Version: 6.0.0 r127566
    server-satu: VirtualBox Version: 6.1
==> server-satu: Configuring and enabling network interfaces...
==> server-satu: Mounting shared folders...
    server-satu: /vagrant => D:/Belajar-Vagrant
```
4. Instance sudah berhasil kita buat. Kalian bisa cek di virtualbox maka ubuntu 22.04 sedang running.
5. Berikut beberapa command penting setelah install instance-nya.
    * Untuk menghentikan mesin dan menyimpan statusnya saat ini, silahkan ketikan:
        *`vagrant suspend`*
    * Untuk mematikan mesin virtual, gunakan perintah:
        *`vagrant halt`*
    * Untuk menghidupkan kembali mesin, ikutin perintah ini:
        *`vagrant up`*
    * Untuk menghapus semua jejak mesin virtual dari sistem Anda, ketik perintah berikut:
        *`vagrant destroy`*

### Akses Instance/VM yang sudah dibuat

Untuk mengakses Instance/VM yang sudah kita buat, silahkan jalankan terlebih dahulu dengan mengetikan perintah `vagrant up`. Jika sudah, siapkan beberapa tools favorite kalian untuk akses ssh. Bisa menggunakan Putty, MobaXterm atau bisa juga dengan WSL2. Disini saya akan coba dengan menggunakan MobaXterm.

1. Silahkan download terlebih dahulu MobaXterm.
2. Pastikan VM sudah jalan, ketikan perintah `vagrant ssh-config` untuk melihat hostname, username dan port yang akan kita ssh.
3. Silahkan buka MobaXterm, Kemudian buat session baru SSH.
 * Remote host: silahkan isi 127.0.0.1
 * Specify username: silahkan isi vagrant
 * Port: silahkan isi 2222
 * Dibagian Tab *Advanced SSH settings*, silahkan ceklis `Use private key`, browse dimana private key disimpan. Disini private key disimpan di `D:/Belajar-Vagrant/.vagrant/machines/server-satu/virtualbox/private_key`
4. Jika sudah maka akan tampil dan login ke vm ubuntu yang baru kita buat, disana kalian bebas melakukan apapaun sama halnya seperti create instance di AWS/GCP.

    ```bash
    Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-52-generic x86_64)

    System information as of Mon Nov 21 15:44:26 UTC 2022

    System load:  0.05615234375     Processes:               95
    Usage of /:   4.0% of 38.70GB   Users logged in:         0
    Memory usage: 44%               IPv4 address for enp0s3: 10.0.2.15
    Swap usage:   0%                IPv4 address for enp0s8: 10.10.10.21

    14 updates can be applied immediately.
    12 of these updates are standard security updates.
    To see these additional updates run: apt list --upgradable

    Last login: Mon Nov 21 15:38:22 2022 from 10.0.2.2
    vagrant@ubuntu-jammy:~$
    ```