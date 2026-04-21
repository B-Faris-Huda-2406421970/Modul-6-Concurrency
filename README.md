# Modul 6 Concurrency

## Reflection 1
### Apa yang `handle_connection` lakukan?
Fungsi `handle_connection` bertugas untuk membaca dan memproses HTTP request yang dikirim oleh client seperti browser melalui TCP. Implementasi dari `handle_connection` adalah:
1. Membungkus stream dengan buffer
2. Membaca request per baris
3. Mengekstrak teks untuk menangani jika ada error
4. Mencari batas akhir header HTTP
5. Mengumpulkan seluruh String yang sesuai ke dalam suatu Vector String
6. Mencetak hasilnya (yakni isi dari request tersebut)

## Reflection 2
### Apa yang baru dari `handle_connection`
`handle_connection` sekarang sudah bisa memberikan HTTP response ke client (browser). Sebelumnya, fungsi hanya mencetak request yang didapatkan ke terminal. Hal-hal yang baru ditambahkan adalah:
1. Membuat Response Status HTTP
2. Membaca file HTML `hello.html`
3. Menghitung ukuran konten
4. Membuat String HTTP Response
5. Mengirimkan response yang sudah dibuat ke client

### Screenshot browser setelah menambahkan file HTML
![Commit 2 Screenshot Browser](/assets/images/Commit%202_Browser.jpg)

### Screenshot terminal setelah menambahkan file HTML
![Commit 2 Screenshot Terminal](/assets/images/Commit%202_Terminal.jpg)

## Reflection 3
### Kenapa response harus dipisah dan kenapa refactor tersebut harus dilakukan?
Refactoring ini dilakukan untuk menerapkan prinsip Don't Repeat Yourself (DRY). Pada kode awal sebelum dilakukan refactoring, dapat dilihat bahwa ada beberapa baris kode yang sama yang berada di bagian if-else berbeda. Perbedaannya hanya terletak pada response status dan file HTML yang dikembalikan. Dengan menerapkan prinsip DRY, kita meningkatkan maintainability dari kode tersebut. Kita memisahkan logika dengan data yang akan kita kembalikan ke client nantinya.

### Screenshot browser untuk bad request (endpoint yang tidak ada)
![Commit 3 Screenshot Browser untuk Bad Request](/assets/images/Commit%203_Bad%20Request.jpg)

### Screenshot terminal
![Commit 3 Screenshot Terminal](/assets/images/Commit%203_Terminal.jpg)

## Reflection 4
### Bagaimana dan mengapa kode tersebut bekerja seperti itu
Jika kita mencoba mengakses endpoint /sleep, maka akan ada delay selama 5 detik sebelum halaman (response) dikembalikan oleh server. Cara kerja kode tersebut adalah:
1. Match mencocokkan bagian kode mana yang akan menjadi status code dan file dari response
2. Jika ada yang mengakses endpoint /sleep, OS akan menghentikan eksekusi program selama 5 detik (thread sleep) dan tidak memproses baris apapun
3. Program kembali bangun
4. Response dikembalikan berupa status code dan file HTML yang akan ditampilkan

Jika kita mengakses endpoint /sleep terlebih dahulu, lalu mengakses endpoint lain di tab baru (misal endpoint root /), maka tab baru tersebut juga ikut terkena imbasnya. Ini bisa menjadi masalah karena menyebabkan tab lain yang tidak mengakses endpoint tersebut ikut terkena delay yang tidak diinginkan. Hal ini disebabkan oleh server kita masih bersifat single-threaded. Sehingga jika thread tersebut dimatikan untuk sementara, maka request yang dihandle oleh client tersebut juga ikut tertunda.

### Screenshot browser untuk endpoint root (/)
![Commit 4 Screenshot Browser untuk Endpoint Root](/assets/images/Commit%204_Endpoint%20Root.jpg)

### Screenshot browser ketika menunggu response pada endpoint /sleep
![Commit 4 Screenshot Browser saat Menunggu Response pada Endpoint Sleep](/assets/images/Commit%204_Waiting%20di%20Endpoint%20Sleep.jpg)

### Screenshot browser setelah mendapatkan response pada endpoint /sleep
![Commit 4 Screenshot Browser saat Mendapatkan Response pada Endpoint Sleep](/assets/images/Commit%204_Endpoint%20Sleep.jpg)