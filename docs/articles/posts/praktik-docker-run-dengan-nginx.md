**Last updated**: June 7, 2020


### Perbedaan Docker Run dan Docker Create Container

Perintah `docker run` digunakan untuk menjalankan sebuah container. Perintah ini akan memeriksa apakah gambar yang ditentukan tersedia secara lokal, dan jika tidak, akan menarik gambar dari registry yang ditentukan. Kemudian akan membuat sebuah container baru dari gambar tersebut dan menjalankannya.

Perintah `docker create` digunakan untuk membuat sebuah container baru, tetapi tidak menjalankannya. Perintah ini akan membuat sebuah container tetapi meninggalkannya dalam keadaan berhenti. Untuk menjalankan container tersebut, Anda dapat menggunakan perintah `docker start`.

Jadi, perbedaan utama antara `docker run` dan `docker create` adalah bahwa <u>docker run membuat dan menjalankan sebuah container dengan satu perintah</u>, sementara `docker create` <u>hanya membuat sebuah container</u>. Perintah docker start digunakan untuk menjalankan kembali sebuah container yang telah dihentikan.

#### Mencari image nginx
```bash
docker search nginx
```

#### Menjalankan image nginx dengan nama container nginx1 dan expose ke port 8181
```bash
docker run -d --name nginx1 -p 8181:80 nginx:latest
```
Dengan perintah diatas, docker akan membuat container dengan nama nginx1 dengan melakukan download / pull image dari Docker hub untuk di expose ke port 8181.

#### Menjalankan image nginx dengan nama container nginx2 dan expose ke port 8282
```bash
docker run -d --name nginx2 -p 8282:80 nginx:latest
```

#### Melihat container yang sedang berjalan
```bash
docker ps -a
```

#### Cek apakah nginx sudah bisa diakses dengan perintah curl
```bash
curl localhost:8181
```
<sub><b>Note</b>: Lakukan hal yang sama, curl nginx2 ke port 8282.</sub><br><br>
Maka akan tampil seperti dibawah ini baik itu nginx1 ataupun nginx2
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

#### Masuk ke masing-masing container, ubah halaman default nginx
```bash
docker exec -it nginx1 /bin/bash
touch index.html && echo "hello nginx1" >> index.html && mv index.html /usr/share/nginx/html
```
Kemudian restart container nginx1
```bash
docker restart nginx1
```
<sub><b>Note:</b> Lakukan hal yang sama untuk nginx2 namun di index.html ubah menjadi "hello nginx2"</sub>

#### Cek kembali masing-masing Nginx
```bash
curl localhost:8181
curl localhost:8282
```
Maka default page Nginx sudah diganti ke halaman `hello nginx1` dan `hello nginx2`

