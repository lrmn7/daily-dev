**Last updated**: April 3, 2020


### Prasyarat

Sebelum masuk ke materi ini, pastikan sudah mengintsall ansible dan menyiapkan setidaknya 2 server 1 sebagai controller dan 1 sebagai managed server yang akan di install Apache. Saya sudah menyiapkan server dengan menggunakan Vagrant, kalian bisa ikuti di materi sebelumnya.

#### Membuat Working direktori

Sebelum memulai, pertama buat terlebih dahulu direktori yang akan digunakan sebagai tempat untuk menyimpan beberapa konfigurasi ansible dan konfigurasi apache.

```bash
lrmn@ubuntu-controller:~$ mkdir latihan-ansible-2
lrmn@ubuntu-controller:~$ cd latihan-ansible-2/
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

Kemudian buat inventory 

```bash
vim inventory
```

```bash
[latihan2ansible]
ubuntu-managed2
```

### Membuat ansible playbook

Langkah terkahir adalah dengan membuat ansible playbook, disini kita menggunaka 3 module yaitu apt, copy dan service.

```bash
vim latihan2.yml
```

```yml
- name: Praktik Install Apache2 dengan Required Package & Variable di Playbook
  hosts: latihan2ansible
  become: true
  become_user: root
  remote_user: lrmn
  vars:
    required_Pkg:
      - apache2
      - python3-urllib3
    web_Service: apache2
    content_File: "Hello World! - Praktik: Install Apache2 dengan Required Package & Variable di Playbook"
    dest_File: /var/www/html/index.html
  tasks:
    - name: Installed required packages
      apt:
        update_cache: yes
        force_apt_get: yes
        name: "{{required_Pkg}}"
        state: latest
    - name: Check {{web_Service}} service is started and enabled
      service:
         name: "{{web_Service}}"
         enabled: true
         state: started
    - name: Web content is in place
      copy:
        content: "{{content_File}}"
        dest: "{{dest_File}}"

- name: Verify the apache service
  hosts: localhost
  tasks:
    - name: Ensure the webserver is reacheable
      uri:
        url: http://ubuntu-managed2/index.html
        status_code: 200
        return_content: yes
```

#### Mengecek ansible playbook

Ansible playbook yang sudah kalian tulis lebih baik di cek terlebih dahulu apakah terdapat error atau tidak dengan menggunakan command dibawah ini:

```bash
ansible-playbook --syntax-check latihan2.yml
```

#### Menjalankan ansible playbook

Jika playbook tidak ada yang error, langkah selanjutnya adalah menjalankannya

```bash
ansible-playbook -i inventory latihan2.yml
```

Jika berhasil maka akan tampil seperti dibawah ini

```bash
lrmn@ubuntu-controller:~/latihan-ansible-2$ ansible-playbook -i inventory latihan2.yml

PLAY [Praktik Install Apache2 dengan Required Package & Variable di Playbook] **************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [ubuntu-managed2]

TASK [Installed required packages] *********************************************************************************
changed: [ubuntu-managed2]

TASK [Check apache2 service is started and enabled] ****************************************************************
ok: [ubuntu-managed2]

TASK [Web content is in place] *************************************************************************************
changed: [ubuntu-managed2]

PLAY [Verify the apache service] ***********************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [localhost]

TASK [Ensure the webserver is reacheable] **************************************************************************
ok: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
ubuntu-managed2            : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

### Mengecek Apache2 di Managed Server

Jika ansible sudah dijalankan maka tertulis changed di ubuntu managed1 untuk task install dan copy, itu berarti bisa saja apache sudah diinstall dan file index.html.j2 sudah di copy ke folder configurasi apache2. Langkah selanjutnya cek managed servernya.

```bash
curl http://ubuntu-managed2/index.html
```

Jika tampil seperti dibawah, maka kita telah berhasil menginstall apache2 menggunakan ansible.

```bash
lrmn@ubuntu-controller:~/latihan-ansible-2$ curl http://ubuntu-managed2/index.html
Hello World! - Praktik: Install Apache2 dengan Required Package & Variable di Playbook
```

Kita juga dapat melakukan port forwarding ubuntu-managed2 untuk melihatnya di laptop host kita dengan membukanya di browser, caranya sudah dituliskan di artikel sebelumnya.