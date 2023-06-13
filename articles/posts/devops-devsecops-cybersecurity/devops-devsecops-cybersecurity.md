**Last updated**: May 20, 2020


### Apa yang dimaksud dengan DevSecOps?
DevSecOps adalah pendekatan pengembangan perangkat lunak yang bertujuan untuk menyatukan tim pengembangan, keamanan, dan operasi untuk membangun dan memelihara aplikasi perangkat lunak yang aman. Pendekatan ini didasarkan pada prinsip-prinsip integrasi berkelanjutan, pengiriman berkelanjutan, dan penerapan berkelanjutan, yang bertujuan untuk memberikan pembaruan dan fitur perangkat lunak dengan lebih cepat dan sering. 

  > Dalam DevSecOps, keamanan merupakan bagian penting dari proses pengembangan perangkat lunak. Artinya, pengujian keamanan, pemantauan, dan langkah-langkah keamanan lainnya dibangun ke dalam siklus hidup pengembangan perangkat lunak (SDLC) sejak awal, bukan ditambahkan kemudian. DevSecOps bertujuan untuk meningkatkan kolaborasi dan komunikasi antara tim pengembangan, keamanan, dan operasi, untuk menciptakan proses pengembangan perangkat lunak yang lebih efisien dan efektif.

### DevSecOps vs DevOps
Saya menggunakan kata "vs" dengan ringan di sini, tetapi jika kita berpikir kembali ke tahun 2022 dan tujuan DevOps adalah untuk meningkatkan kecepatan, keandalan, dan kualitas rilis perangkat lunak.

DevSecOps adalah perpanjangan dari filosofi DevOps yang menekankan pada integrasi praktik keamanan ke dalam proses pengembangan perangkat lunak. **Tujuan dari DevSecOps adalah untuk membangun langkah-langkah keamanan ke dalam proses pengembangan perangkat lunak sehingga keamanan menjadi bagian penting dari perangkat lunak sejak awal**. Hal ini membantu mengurangi risiko kerentanan keamanan yang dimasukkan ke dalam perangkat lunak dan membuatnya lebih mudah untuk mengidentifikasi dan memperbaiki masalah apa pun yang muncul.

  > **DevOps** berfokus pada peningkatan kolaborasi dan komunikasi antara pengembang dan staf operasi untuk meningkatkan kecepatan, keandalan, dan kualitas rilis perangkat lunak, sedangkan **DevSecOps** berfokus pada pengintegrasian praktik keamanan ke dalam proses pengembangan perangkat lunak untuk mengurangi risiko kerentanan keamanan dan meningkatkan keamanan perangkat lunak secara keseluruhan.

#### Automated Security
Keamanan otomatis mengacu pada penggunaan teknologi untuk melakukan tugas-tugas keamanan tanpa perlu campur tangan manusia. Hal ini dapat mencakup hal-hal seperti perangkat lunak keamanan yang memantau jaringan dari ancaman dan mengambil tindakan untuk memblokirnya, atau sistem yang menggunakan kecerdasan buatan untuk menganalisis rekaman keamanan dan mengidentifikasi aktivitas yang tidak biasa. Sistem keamanan otomatis dirancang untuk membuat proses keamanan menjadi lebih efisien dan efektif, serta membantu mengurangi beban kerja personel keamanan.

Komponen utama dari semua hal yang berkaitan dengan DevSecOps adalah kemampuan untuk mengotomatiskan banyak tugas yang ada saat membuat dan mengirimkan perangkat lunak, ketika kita menambahkan keamanan sejak awal, itu berarti kita juga perlu mempertimbangkan aspek otomatisasi keamanan.

#### Security at Scale (Containers and Microservices)
Kita tahu bahwa skala dan infrastruktur dinamis yang dimungkinkan oleh `Containers` dan `Microservices` telah mengubah cara sebagian besar organisasi menjalankan bisnis.

Ini juga alasan mengapa kita harus membawa keamanan otomatis tersebut ke dalam prinsip-prinsip DevOps untuk memastikan bahwa `container security guidelines` tertentu terpenuhi.

Yang saya maksud dengan ini adalah dengan teknologi cloud-native, kita tidak bisa hanya memiliki kebijakan dan postur keamanan yang statis; model keamanan kita juga harus dinamis dengan beban kerja yang ada dan bagaimana hal itu berjalan.

Tim DevOps perlu menyertakan keamanan otomatis untuk melindungi lingkungan dan data secara keseluruhan, serta integrasi berkelanjutan dan proses pengiriman yang berkelanjutan.

Daftar di bawah ini diambil dari postingan blog `RedHat`
  1. **Standardise and automate the environment**: Setiap layanan harus memiliki hak istimewa sesedikit mungkin untuk meminimalkan koneksi dan akses yang tidak sah.
  2. **Centralise user identity and access control capabilities**: Kontrol akses yang ketat dan mekanisme otentikasi terpusat sangat penting untuk mengamankan layanan mikro karena otentikasi dilakukan di beberapa titik.
  3. **Isolate containers running microservices from each other and the network**: Ini mencakup data dalam perjalanan dan data saat istirahat karena keduanya dapat menjadi target bernilai tinggi bagi penyerang.
  4. **Encrypt data between apps and services**: Platform orkestrasi kontainer dengan fitur keamanan terintegrasi membantu meminimalkan kemungkinan akses yang tidak sah.
  5. **Introduce secure API gateways**: API yang aman meningkatkan otorisasi dan visibilitas perutean. Dengan mengurangi API yang terbuka, organisasi dapat mengurangi permukaan serangan.

### Cybersecurity vs DevSecOps

Sesuai dengan judulnya, ini sebenarnya bukan vs, tetapi lebih kepada perbedaan antara kedua topik tersebut. Tapi saya pikir penting untuk mengangkat hal ini karena hal ini akan menjelaskan mengapa Keamanan harus menjadi bagian dari proses, prinsip, dan metodologi DevOps.

  > **Cybersecurity** adalah praktik melindungi sistem dan jaringan komputer dari serangan digital, pencurian, dan kerusakan. Hal ini melibatkan identifikasi dan penanganan kerentanan, penerapan langkah-langkah keamanan, dan pemantauan sistem terhadap ancaman.

  > **DevSecOps**, di sisi lain, adalah kombinasi dari praktik pengembangan, keamanan, dan operasi. Ini adalah filosofi yang bertujuan untuk mengintegrasikan keamanan ke dalam proses pengembangan, daripada memperlakukannya sebagai langkah yang terpisah. Hal ini melibatkan kolaborasi antara tim pengembangan, keamanan, dan operasi di seluruh siklus hidup pengembangan perangkat lunak (SDLC).

#### Beberapa perbedaan utama antara Cybersecurity dan DevSecOps antara lain:

  * **Focus**: Cybersecurity terutama difokuskan untuk melindungi sistem dari ancaman eksternal, sedangkan DevSecOps berfokus pada pengintegrasian keamanan ke dalam proses pengembangan.

  * **Scope**: Cybersecurity mencakup topik yang lebih luas, termasuk keamanan jaringan, keamanan data, keamanan aplikasi, dan banyak lagi. DevSecOps, di sisi lain, secara khusus berfokus pada peningkatan keamanan pengembangan dan penerapan perangkat lunak.

  * **Approach**: Cybersecurity biasanya melibatkan penerapan langkah-langkah keamanan setelah proses pengembangan selesai, sedangkan DevSecOps melibatkan pengintegrasian keamanan ke dalam proses pengembangan sejak awal.

  * **Collaboration**: Cybersecurity sering kali melibatkan kolaborasi antara tim TI dan tim keamanan, sedangkan DevSecOps melibatkan kolaborasi antara tim pengembangan, keamanan, dan operasi.
