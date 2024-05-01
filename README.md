# Reflection
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable? <br>
**1.1. Unary:** client memberikan satu  request dan server memberikan satu respon kembali ke client. Unary cocok digunakan untuk interaksi client-server yang hanya tunggal, misalnya seperti fetch data sebuah user. <br>
**1.2. Server Streaming:** server memiliki tujuan untuk secara kontinu mengirimkan data ke client dalam ukuran yang besar, misalnya ketika client ingin mengetahui live statistics dari pertandingan sepak bola. <br>
**1.3. Bi-directional Streaming:** client dan server saling mengirimkan data antar satu sama lain dengan jalurnya yang saling independen. Cocok digunakan untuk komunikasi client-server yang dinamis. Contoh penggunannya seperti dalam tools kolaborasi seperti Figma.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
**2.1. Authentication:** Kita dapat mengimplementasikan token-based authentication, contohnya menggunakan token JWT pada setiap terjadinya gRPC call. <br>
**2.2. Authorization:** Dalam authorization, kita dapat mengimplementasikan Role-Based Access Control untuk mengatur hal-hal apa saja yang boleh dilakukan oleh tiap client. <br>
**2.3. Data Encryption:** Kita dapat menerapkan Transport Layer Security (TLS) untuk menenkripsi data saat proses pengirimannya. Data yang disimpan juga harus dienksripsi untuk menjamin keamanannya. <br>

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications? <br>
Isu yang muncul dapat berkaitan dengan performance dan juga security. Untuk masalah performance, terdapat potensi masalah ketika server ingin mengirimkan satu pesan ke banyak client. Hal tersebut dapat ditangani dengan menggunakan serde pada Rust untuk melakukan serialization dan deserialization saat transmisi data. Untuk masalah security, terdapat potensi penyadapan pada transmisi data. Solusinya dapat menggunakan TLS untuk menjamin keamanan data saat transmisi.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services? <br>
**4.1. Advantages:** ReceiverStream dapat mempermudah integrasi message passing tokio dengan stream sehingga dapat juga mempermudah integrasi dengan library lainnya yang digunakan untuk asynchrous messsage passing. <br>
**4.2. Disadvantages:** Tidak terlalu fleksibel karena bergantung pada interface Receiver.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time? <br>
Kita sudah menerapkan pemisahan pengaturan gRPC dalam `services.proto` yang menjamin reusability. Selain itu, kita juga dapat menerapkan dependency injection untuk memberikan komponen yang dibutuhkan sehingga menjamin fleksibilitas kode. 

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic? <br>
Banyak langkah yang bisa dilakukan, seperti validasi data pembayaran, interaksi dengan database untuk memeriksa dan men-update saldo, monitoring response time, dan konfirmasi pengiriman dana.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms? <br>
Dalam aspek interoperability, gRPC mendukung banyak bahasa pemograman sehingga integrasi antar services dapat dilakukan dengan lancar. Dalam aspek lainnya, gRPC mendukung HTTP/2 yang bisa mengurangi latency karena mendukung multiplexing, yang merupakan kemampuan untuk mengirimkan banyak request pada suatu koneksi.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs? <br>
**8.1. Advantages:** HTTP/2 mendukung multiplexing, kemampuan untuk mengirimkan banyak request pada suatu koneksi TCP yang dapat meminimalkan latency. HTTP/2 juga mendukung header compression dan server push yang mampu mengurangi besar data yang dikirimkan dan juga meminimalkan latency. <br>
**8.2. Disadvantages:** Tidak semua perangkat mendukung HTTP/2, meskipun mayoritas sudah mendukungnya. Implementasinya juga lebih kompleks dengan adanya fitur multiplexing dan stream. <br>
**8.3. WebSocket** mendukung full duplex communication, yang memungkinkan terjadinya transmisi data antara client dan server pada saat waktu yang bersamaan, tetapi kelebihan ini tidak sebanding dengan versi terbarunya (HTTP/2)

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness? <br>
**9.1. Real Time Communication:** REST API termasuk tipe unary RPC method karena dia bekerja dengan cara client mengirimkan satu request ke server, client menunggu respon sebelum bisa melanjutkan tugas lainnya, lalu server mengembalikan satu respon. Jadi, tidak memungkinkan untuk melakukan real time communication dengan REST API. Sementara itu, gRPC termasuk bidirectional sehingga client dan server dapat saling mengirimkan data kapan pun. Maka dari itu, gRPC cocok diterapkan untuk real time communication. <br>
**9.2. Responsiveness:** REST API tidak responsif karena latency-nya dalam membuat koneksi dan overhead dari header HTTP/1.1. gRPC mampu lebih responsif karena menggunakan fitur-fitur HTTP/2 seperti stream multiplexing dan header compression yang mampu meminimalkan latency.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads? <br>
**10.1. Protocol Buffers di gRPC:** wajib menggunakan file `.proto` (`services.proto` pada tutorial ini) untuk mendefinisikan skema struktur data dan interface services. Membuat aplikasi menjadi konsisten, tetapi tidak cukup fleksibel dalam memodifikasi struktur data. Protocol Buffers juga menggunakan bandwith yang lebih kecil daripada JSON karena adanya serliazation dan deserialization, tetapi membuat proses debugging menjadi lebih sulit karena datanya binary. <br>
**10.2. JSON di REST API:** JSON tidak membutuhkan schema yang ditentukan sehingga membuat struktur data bisa lebih dinamis, tetapi berpotensi menimbulkan inkonsistensi dalam pengaturan data. Format JSON juga human-readable, yang mempermudah debugging, tetapi memiliki memakai bandwith yang lebih besar.