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

