**Last updated**: June 22, 2020


### Apa itu Adhoc Command ?

  > Adhoc command di Ansible adalah perintah yang dapat Anda jalankan dari command line interface (CLI) untuk melakukan tugas-tugas tertentu pada host atau grup host dalam inventory. Adhoc command terdiri dari modul Ansible yang dijalankan untuk melakukan tugas-tugas seperti menjalankan perintah shell, menginstal paket, memeriksa status service, menyalin file, dan banyak lagi.

Adhoc command sangat berguna dalam situasi ketika Anda perlu melakukan tugas yang cepat dan sederhana pada beberapa host tanpa harus membuat playbook Ansible. 

Misalnya, Anda dapat menggunakan adhoc command untuk memeriksa status service pada beberapa host dalam inventory, melakukan update dan upgrade pada host, atau menjalankan perintah shell pada satu host atau semua host dalam inventory.

#### Kapan Adhoc Command digunakan ?

Adhoc command di Ansible diperlukan dalam beberapa situasi, di antaranya:

1. Ketika Anda perlu melakukan tugas cepat dan sederhana pada beberapa host dalam inventory, tetapi tidak memerlukan playbook Ansible yang lengkap.

2. Ketika Anda ingin menguji tugas tertentu pada satu host atau beberapa host dalam inventory untuk memastikan bahwa semuanya berfungsi dengan baik sebelum Anda menggunakannya dalam playbook Ansible.

3. Ketika Anda ingin memeriksa keadaan suatu host atau beberapa host dalam inventory untuk debugging atau eksperimen.

4. Ketika ada perubahan yang harus dilakukan pada lingkungan Anda dan Anda memerlukan tindakan cepat untuk menangani masalah atau keadaan darurat.

5. Ketika Anda perlu melakukan tugas-tugas yang tidak dapat dilakukan dengan playbook Ansible, seperti menjalankan perintah shell, menginstal paket, memeriksa status service, menyalin file, dan lain sebagainya.

Adhoc command sangat berguna dalam situasi-situasi di atas, karena memungkinkan Anda untuk melakukan tugas-tugas sederhana dengan cepat dan efisien, tanpa harus membuat playbook Ansible yang lengkap dan terstruktur. 

Dengan menggunakan adhoc command, Anda dapat menghemat waktu dan upaya, dan memungkinkan Anda untuk dengan cepat mengambil tindakan ketika ada masalah atau perubahan yang harus dilakukan pada lingkungan Anda.

#### Persiapan Sever Controller Ansible & 2 Managed Server

Disini saya sudah menyiapkan 3 buah server, 1 server sebagi controller dan 2 sebagai managed server yang nanti beberapa job/task dijalankan dari controller. Server saya buat dengan Vagrant, ada di tutorial saya sebelumnya. Berikut konfigurasi Vagrant yang saya buat. 

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

#### Konfigurasi Ansible Inventory

  > Ansible Inventory adalah file konfigurasi yang digunakan untuk mengelompokkan host yang akan dikelola oleh Ansible. File ini berisi daftar host yang dikelola, grup host, dan variabel yang terkait dengan host atau grup host. Ansible Inventory juga digunakan untuk menentukan bagaimana Ansible akan menghubungi host yang dikelola, seperti melalui SSH atau WinRM.

Kemudian tiap server sudah saya tambahkan ke inventory ansiblenya, berikut detailnya:
```bash
ubuntu-managed1

[belajaransible]
ubuntu-managed2
```

### Contoh Penggunaan Adhoc Command Ansible

Berikut dibawah ini kami berikan beberapa contoh adhoc command di ansible yang simple:

#### 1. Menjalankan perintah shell pada satu atau beberapa host
Anda perlu mengganti <nama_host> atau <nama_grup_host> dengan nama host atau grup host yang ingin Anda gunakan, dan ls -al dengan perintah shell yang sesuai.
```bash
ansible <nama_host> -a 'ls -al'
ansible <nama_grup_host> -a 'ls -al'
```

#### 2. Melakukan ping ke satu atau beberapa host untuk memeriksa apakah host terhubung atau tidak
Anda perlu mengganti <nama_host> atau <nama_grup_host> dengan nama host atau grup host yang ingin Anda gunakan.
```bash
ansible <nama_host> -m ping
ansible <nama_grup_host> -m ping
```

#### 3. Menyalin file dari local ke remote host:
Anda perlu mengganti <nama_host> atau <nama_grup_host>, <path_to_local_file>, dan <path_to_remote_file> dengan nilai yang sesuai untuk lingkungan Anda.
```bash
ansible <nama_host> -m copy -a 'src=<path_to_local_file> dest=<path_to_remote_file>'
ansible <nama_grup_host> -m copy -a 'src=<path_to_local_file> dest=<path_to_remote_file>'
```
  > Pada adhoc command Ansible, src dan dest adalah dua parameter yang digunakan pada modul copy untuk menentukan sumber (source) dan tujuan (destination) dari file yang akan dicopy.

* Secara rinci, **src** adalah lokasi file yang ingin dicopy, bisa berupa path absolut atau path relatif terhadap direktori kerja saat ini. Parameter src pada modul copy dapat digunakan untuk menentukan file yang akan dicopy dari host yang menjalankan Ansible (control node) ke host target, atau sebaliknya.

* Sedangkan **dest** adalah lokasi tujuan tempat file akan disalin. Lokasi dest dapat berupa path absolut atau path relatif pada host target. Dalam contoh adhoc command yang telah diberikan sebelumnya, src diisi dengan lokasi file yang ingin dicopy, sedangkan dest diisi dengan lokasi tempat file akan dicopy.

#### 4. Menginstal paket pada satu atau beberapa host:
Anda perlu mengganti <nama_host> atau <nama_grup_host> dan <nama_paket> dengan nilai yang sesuai untuk lingkungan Anda.
```bash
ansible <nama_host> -m apt -a 'name=<nama_paket> state=latest'
ansible <nama_grup_host> -m apt -a 'name=<nama_paket> state=latest'
```

#### 5. Memeriksa status service pada satu atau beberapa host:
Anda perlu mengganti <nama_host> atau <nama_grup_host> dan <nama_service> dengan nilai yang sesuai untuk lingkungan Anda.
```bash
ansible <nama_host> -m systemd -a 'name=<nama_service> state=started'
ansible <nama_grup_host> -m systemd -a 'name=<nama_service> state=started'
```

  > Pada contoh-contoh adhoc command Ansible di atas, *state* adalah salah satu parameter yang digunakan untuk menentukan keadaan atau kondisi yang ingin dicapai oleh modul Ansible yang digunakan.

Misalnya, pada contoh keempat di atas, `state=latest` pada modul apt digunakan untuk memastikan bahwa paket yang diinstal pada host adalah versi terbaru yang tersedia. 

Sedangkan pada contoh kelima, `state=started` pada modul systemd digunakan untuk memastikan bahwa service yang ditentukan dalam kondisi `started` atau berjalan.

Ada beberapa nilai state yang dapat digunakan dalam modul-modul Ansible, antara lain:
* **present**: Menunjukkan bahwa suatu objek (misalnya file, direktori, atau paket) harus ada pada host yang ditargetkan.
* **absent**: Menunjukkan bahwa suatu objek harus tidak ada pada host yang ditargetkan.
* **latest**: Menunjukkan bahwa suatu paket harus diperbarui ke versi terbaru yang tersedia.
* **started**: Menunjukkan bahwa suatu service harus berjalan atau dijalankan.
* **stopped**: Menunjukkan bahwa suatu service harus dihentikan atau berhenti.
Pilihan state yang tepat akan tergantung pada tugas atau tujuan yang ingin dicapai oleh adhoc command Ansible yang sedang dilakukan.