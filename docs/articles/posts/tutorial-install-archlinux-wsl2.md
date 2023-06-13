**Last updated**: October 15, 2020


### Mengetahui lebih dahulu apa itu Systemd

Sebelum membahas bagaimana cara install arch linux di WSL2 Windows, mari kita bahas apa itu Systemd. Menurut laman systemd.io.

> Systemd is a suite of basic building blocks for a Linux system. It provides a system and service manager that runs as PID 1 and starts the rest of the system.

#### Kenapa harus Systemd ?

Beberapa distro Linux paling populer di luar sana sudah menggunakan systemd secara default pada instalasi bare metal. Beberapa di antaranya, seperti Ubuntu dan Debian tersedia di WSL.

Penyertaan systemd pada WSL membawa tools ini lebih dekat dengan pengalaman menjalankan Linux secara asli. Systemd juga diperlukan untuk beberapa tool yang sekarang dapat digunakan dengan mudah di WSL, seperti snap, microk8s, hostnamectl, systemctl, dan lainnya.

1. snap
  - Biner praktis yang memungkinkan Anda untuk menginstal dan mengelola perangkat lunak di dalam Ubuntu.
  - Coba jalankan: `snap install spotify` atau `snap install postman`
2. microk8s
  - Dapatk menjalankan Kubernetes secara lokal di sistem Anda dengan cepat.
3. systemctl
  - Alat yang merupakan bagian dari systemd, berinteraksi dengan layanan di mesin Linux.
  - Coba jalankan: `systemctl list-units --type=service` untuk melihat layanan mana yang tersedia dan statusnya

**Update**: Setelah saya menulis tutorial ini Systemd sudah dapat dijalankan di WSL. 
Sumber: 
<https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl>

#### Apa itu Distrod dan apa hubungannya dengan Systemd

Distrod adalah meta-distro berbasis systemd untuk WSL2 yang memungkinkan Anda untuk menginstal Ubuntu, Arch Linux, Gentoo dan banyak distro lainnya dengan systemd dalam satu menit, atau membuat distro Anda saat ini menjalankan systemd.

Distrod juga menyediakan fitur `auto-start` `built-in` dan layanan `port forwarding`. Ini memungkinkan Anda untuk memulai layanan yang dikelola systemd, seperti `ssh`, pada startup Windows dan membuatnya dapat diakses dari luar Windows.

Ini juga menjadikan Distrod sebagai solusi bagi kalian yang ingin menginstall berbagai macam distro linux lainnya untuk dapat dijalankan di WSL2 Windows 10/11.

#### Cara mengetahui distro linux di WSL jalan dengan Systemd atau tidak.

Jika sebelumnya kalian sudah pernah install WSL dengan distro yang ada di Windows Store, maka systemd tidak akan muncul dan akan menampilkan pesan seperti dibawah ini, kalian dapat lakukan dengan mengetikan perintah `systemctl`.
```bash
febrian@Thinkpad-X230:~$ systemctl
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```

### Tutorial install distro linux lain di WSL2 Windows dengan Distrod

Untuk saat ini Distrod sudah dapat menjalankan beberapa varian distribusi linux, beberapa yang sudah dapat dijalankan yaitu: Ubuntu, Debian, Arch Linux, Fedora, CentOS, AlmaLinux, Rocky Linux, Kali Linux, Linux Mint, openSUSE, Amazon Linux, Oracle Linux, Gentoo Linux.

#### Option 1: Instal distro baru dengan systemd berjalan

1. Silahkan download terlebih dahulu Distrod versi terbaru sini, tutorial kali ini menggunakan v.0.1.7: <https://github.com/nullpo-head/wsl-distrod/releases>
2. Ekstrak .zip file yang sudah di download, dan klik 2x pada .exe
3. Maka akan muncul pilihan 2 opsi, silahkan ketikan nomor 2
```bash
[1] Use a local tar.xz file
[2] Download an image from linuxcontainers.org
[Distrod] Choose the way to get a distro image from the list above.
[Distrod] Type the name or the index of your choice.
[Default: Download an image from linuxcontainers.org]: 2
```
4. Akan muncul distro linux lainnya, disini, Silahkan ketikan archlinux
```bash
[Distrod] Fetching from linuxcontainers.org...
[1] almalinux
[2] alpine
[3] alt
[4] amazonlinux
[5] apertis
[6] archlinux
[7] busybox
[8] centos
[9] debian
[10] devuan
[11] fedora
[12] funtoo
[13] gentoo
[14] kali
[15] mint
[16] opensuse
[17] openwrt
[18] oracle
[19] plamo
[20] pld
[21] rockylinux
[22] springdalelinux
[23] ubuntu
[24] voidlinux
[Distrod] Choose a linuxcontainers.org image from the list above.
[Distrod] Type the name or the index of your choice.
[Default: ubuntu]: archlinux
```
5. Selanjutnya silahkan pilih versinya, jika kalian diatas memilih ubuntu maka akan muncul beberapa versi ubuntunya. Sedangkan untuk archlinux tidak ada pilihan lain karena memang bersifat rolling release.
```bash
[Distrod] Fetching from linuxcontainers.org...
[1] current
[Distrod] Choose a version from the list above.
[Distrod] Type the name or the index of your choice.
[Default: current]: current
```
6. Kemudian Distrod akan mendownload distro yang kita pilih
```bash
[Distrod] Fetching from linuxcontainers.org...
[Distrod] Downloading 'https://images.linuxcontainers.org/images/archlinux/current/amd64/default/20221119_04:18/rootfs.tar.xz'...
⠤ [00:00:07] [>-------------------------------------------------] 2.88MiB/157.31MiB (558.04KiB/s, 4m)
```
7. Kemudian silahkan masukan username dan password.
```bash
[Distrod] Download done.
[Distrod] Unpacking and merging the given rootfs to the distrod rootfs. This may take a while...
[Distrod] Now Windows is installing the new distribution. This may take a while...
[Distrod] Archlinux is installed in %LocalAppData%\Archlinux
[Distrod] Done!
[Distrod] Please input the new Linux user name. This doesn't have to be the same as your Windows user name.
[Input user name]: febri4n
Retype new password:
passwd: password updated successfully
[Distrod] Querying the generated uid. This may take some time depending on your machine.
[Distrod] Initializing the new Distrod distribution. This may take a while...
[Distrod] Distrod has been enabled. Now your shell will start under systemd.
[Distrod] Setting the default user to uid: 1000
[Distrod] Installation of Distrod is now complete.
[febri4n@Thinkpad-X230 distrod_wsl_launcher-x86_64]$
```

#### Option 2: Membuat distro WSL 2 Anda saat ini menjalankan systemd

Dengan instalasi ini, systemd diaktifkan di distro WSL 2 Anda.

1. Unduh dan jalankan skrip penginstal terbaru.
```bash
curl -L -O "https://raw.githubusercontent.com/nullpo-head/wsl-distrod/main/install.sh"
chmod +x install.sh
sudo ./install.sh install
```

2. Aktifkan distrod di distro Anda
Anda memiliki dua opsi. Jika Anda ingin memulai distro Anda secara otomatis pada startup Windows, aktifkan distrod dengan perintah berikut:
```bash
/opt/distrod/bin/distrod enable --start-on-windows-boot
```
Jika tidak, silahkan ketikan perintah dibawah ini
```bash
/opt/distrod/bin/distrod enable
```
Anda dapat menjalankan `enable` dengan `--start-on-windows-boot` lagi jika Anda ingin mengaktifkan autostart nantinya.

3. Mulai ulang distro Anda
Tutup terminal WSL Anda. Buka jendela Command Prompt baru, dan jalankan perintah berikut.
```bash
wsl --terminate Distrod
```