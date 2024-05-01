## Nama   : Muhammad Nabiel Subhan
## NPM    : 2206081553
## Kelas  : AdPro B

### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

Perbedaan antara `unary`, `server streaming`, dan `bi-directional` adalah: <br>
1. `Unary`, client mengirimkan satu request dan menunggu satu response dari server, cocok untuk skenario seperti proses autentikasi user.<br>
2. `Server Streaming`, client mengirimkan satu request dan server mengirimkan banyak informasi yang dikirim satu per satu, cocok untuk skenario seperti pengiriman notifikasi untuk user yang telah men-subscribe notifikasi tersebut.<br>
3. `bi-directional`, client dan server dapat mengirimkan banyak informasi yang dikirim satu per satu (*stream of messages*) terhadap satu sama lain, cocok untuk skenario seperti aplikasi chat real time.<br>

### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

* Untuk authentication, dapat dipertimbangkan untuk menggunakan mutual TLS agar klien dan server dapat mengautentikasi satu sama lain. Selain itu, dapat juga menerapkan otentikasi berbasis token dengan menggunakan OAuth atau JSON Web Tokens (JWT).
* Untuk authorization, dapat menggunakan kontrol akses berdasarkan role yang dimiliki.
* Untuk data encryption, dapat menggunakan TLS untuk mengenkripsi komunikasi gRPC atau menerapkan enkripsi end-to-end untuk melindungi data yang sensitif selama proses transmisi data dan saat penyimpanan di server.

### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Masalah yang dapat timbul adalah:<br>
* Mengelola concurrency pada lingkungan multi-threaded dan sinkronisasi data dapat menjadi suatu tantangan.
* Memastikan urutan messages yang dikirimkan sesuai khususnya pada kasus jika terdapat kendala pada koneksi internet.
* Mengelola banyak koneksi secara bersamaan dapat memberikan beban kepada CPU dan memori server.

### 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

* Kelebihan
Mudah untuk mengubah Receiver menjadi stream yang dapat digunakan pada gRPC, terintegrasi dengan ekosistem tokio lainnya, dan dapat menerapkan pemrograman secara asinkronus dengan tokio.

* Kekurangan
Ada kemungkinan terjadinya penundaan pengiriman pesan ke klien, penumpukan pesan dalam antrian juga dapat menghabiskan memori jika manajemen buffer tidak diperhatikan dengan baik.

### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Yang dapat dilakukan adalah memisahkan logika bisnis menjadi beberapa modul agar lebih mudah di-maintain serta mengurangi ketergantungan antar kode, penggunaan trait (mirip interface di java) untuk menerapkan abstraksi dan polimorfisme, dan penggunaan fitur-fitur protobuf (proto) yang memudahkan serialisasi dan dukungan untuk versi.

### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

* Melakukan validasi data yang diterima dari klien.
* Melakukan otentikasi dan otorisasi pengguna.
* Melakukan unit test untuk memastikan bahwa logika fungsi bekerja dengan baik.

### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Menggunakan gRPC dapat meningkatkan efisiensi dan performa karena gRPC menggunakan serialisasi dari protobuf yang efisien serta penggunaan HTTP/2 yang memungkinkan dilakukannya streaming dan multiplexing sehingga membuat proses komunikasi yang lebih adaptif dan efisien.

### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

* Kelebihan
HTTP/2 mendukung multiplexing sehingga banyak requests dan response yang dapat dikirim secara bersamaan melalui satu koneksi TCP, dapat mengurangi ukuran metadata yang dikirim dengan menerapkan kompresi header, memungkinkan server melakukan push ke klien tanpa klien mengirimkan request terlebih dahulu, dan adanya prioritas aliran.

* Kekurangan
Penerapan HTTP/2 memiliki kompleksitas yang lebih tinggi.

### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

Perbedaan antara REST APIs dengan bidirectional streaming pada gRPC dapat dilihat pada sifat dari komunikasi yang dilakukan. REST APIs digunakan untuk komunikasi satu arah dimaan user mengirimkan request, lalu server mengirimkan response. Namun, komunikasi pada bidirectional streaming merupakan komunikasi dua arah dimana klien dan server dapat saling mengirim dan menerima data. Dengan sifat tersebut, bidirectional streaming mampu untuk merespons suatu perubahan tanpa perlu melakukan polling terlebih dahulu sehingga dapat meningkatkan responsivitas dan menghadirkan pengalaman yang real-time.

### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Dengan penggunaan protocal buffer pada gRPC, struktur data yang digunakan pada komunikasi klien dan server didefinisikan secara langsung sehingga lebih jelas struktur data yang digunakan, lebih efisien dalam hal ukuran payload dan waktu deserialisasi sehingga cocok untuk komunikasi yang memiliki throughput yang tinggi, adanya dukungan untuk validasi data yang ketat, dan memudahkan manajemen terhadap evolusi dan interoperabilitas aplikasi.