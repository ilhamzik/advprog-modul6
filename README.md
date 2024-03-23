Muhammad Ilham Zikri
Advanced Programming A
NPM 2206083533

# refleksi

## 1 : Single Threaded Server
Pada tahap awal, berhasil dibuat Sebuah Server Berbasis Satu Thread yang mendengarkan di alamat 127.0.0.1 dan port 7878. Setiap koneksi yang masuk akan ditangani oleh fungsi handle_connection. Fungsi ini menerima TcpStream object dan menggunakannya untuk membuat BufReader object. Dengan BufReader object, setiap permintaan HTTP dari koneksi dibaca, kemudian hasilnya dicetak. Proses ini memungkinkan server untuk menangani permintaan dengan efisien.

## 2 : Returning HTML
Sebelumnya, saat mengakses situs web, tidak ada respons yang dikembalikan, hanya permintaan HTTP yang diterima dan dicetak di terminal. Sekarang, kita ingin situs web mengirimkan tampilan HTML tertentu. Langkah pertama adalah menambahkan modul fs agar kode Rust dapat membaca file dari sistem, khususnya file HTML yang akan ditampilkan nantinya. Setelah itu, dibuat file HTML bernama hello.html dan isinya dibaca menggunakan fungsi fs::read_to_string, kemudian disimpan dalam variabel contents. HTML tersebut akan dikirim sebagai respons HTTP dalam variabel response yang juga mencakup status respons 200 OK dan header yang menunjukkan panjang konten.
![1111](https://github.com/ilhamzik/advprog-modul6/assets/124953758/ed49f50f-7016-4c23-952b-6b773c687bdb)

## 3 : Validating Request and Selectively Responding
Saat ini, situs web kita hanya memberikan satu respons tanpa memperhatikan jenis permintaan yang diterima, yaitu mengirimkan halaman hello.html. Kita ingin membuat dua respons yang berbeda tergantung pada permintaan dari pengguna. Jika status line permintaan adalah HTTP/1.1 200 OK, situs web akan memberikan respons HTML hello.html dengan status 200 OK. Jika status line permintaan bukan seperti itu, situs web akan memberikan respons HTML 404.html dengan status 404 NOT FOUND. Refactoring dapat dilakukan pada blok if-else tersebut. Karena keduanya mengembalikan respons dengan format yang serupa, yaitu status line dan konten HTML dari file yang akan dikirim, kita dapat menyederhanakan blok if-else tersebut untuk mengatur status_line dan nama file yang akan dibaca oleh fs. Refactoring ini penting untuk mengurangi redundansi dalam kode kita.
![2222](https://github.com/ilhamzik/advprog-modul6/assets/124953758/35e573a1-f238-420a-8d42-0b1b6b7ab9cf)

## 4 : Simulation Slow Response
Dengan menjalankan server pada satu thread, kita ingin mengamati respons saat banyak pengguna mengakses situs web. Kami menambahkan fitur /sleep pada permintaan, yang menyebabkan penundaan selama 10 detik sebelum mengakses situs web. Hal ini dapat diibaratkan dengan situasi di mana server sedang melakukan operasi yang membutuhkan waktu. Ketika banyak pengguna mengakses /sleep secara bersamaan, server harus menangani permintaan satu per satu, menyebabkan antrian yang memperlambat respons dari situs web.

## 5 : Multithreaded Server
Saat ini, kita telah mengamati bagaimana situasi yang mungkin terjadi ketika banyak pengguna mencoba mengakses situs web kita secara simultan. Salah satu solusi untuk menangani jumlah respons yang banyak secara bersamaan adalah dengan menggunakan server yang menggunakan multiple threads. Implementasi dari pendekatan ini dapat dilakukan dengan menggunakan thread pool. Thread pool merupakan sebuah kumpulan thread yang telah dibuat sebelumnya dan siap untuk menyelesaikan tugas. Jumlah thread dalam pool ini dibatasi untuk mencegah serangan Denial of Service (DoS). Ketika ada permintaan baru masuk, permintaan tersebut dimasukkan ke dalam antrian, dan kemudian satu thread dari pool yang sedang tidak digunakan akan mengambil permintaan dari antrian tersebut. Thread yang telah mengambil permintaan akan menangani permintaan tersebut. Setelah menyelesaikan permintaan, thread akan kembali ke pool untuk digunakan kembali dalam menangani permintaan berikutnya. Dengan adanya thread pool, server dapat mengelola hingga N permintaan secara konkuren, di mana N adalah jumlah thread dalam pool tersebut.
