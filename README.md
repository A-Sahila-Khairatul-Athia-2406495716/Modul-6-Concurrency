## Module 06 - Concurrency

<details><summary><b>Milestone 1: Single threaded web server</b></summary>

Method `handle_connection` membaca data dari `TcpStream` menggunakan `BufReader`. Method `lines()` digunakan untuk membaca request line per line, dan `take_while(|line| !line.is_empty())` digunakan untuk berhenti membaca ketika menemukan empty line, karena HTTP request diakhiri dengan dua newline berturut-turut. Output yang muncul di terminal menunjukkan struktur HTTP request seperti `GET / HTTP/1.1`, diikuti berbagai headers seperti `Host`, `User-Agent`, dan sebagainya.

Saat pertama kali menjalankan server (sebelum ada method `handle_connection`), muncul beberapa message "Connection established!" sekaligus, itu terjadi karena browser secara otomatis mencoba mengirim ulang request ketika tidak mendapat response yang proper dalam waktu tertentu.

</details>

<details><summary><b>Milestone 2: Returning HTML</b></summary>

![Commit 2 screen capture](/assets/images/commit2.png)

Metode `handle_connection` dimodifikasi agar server bisa mengirimkan response HTML yang proper ke browser, bukan hanya menerima request saja. Struktur dari HTTP response terdiri dari status line (`HTTP/1.1 200 OK`), diikuti headers (`Content-Length`), lalu dua `\r\n` sebagai separator, baru kemudian body (HTML). `Content-Length` penting karena browser menggunakannya untuk tahu berapa byte yang harus dibaca sebagai body. Di sini `fs::read_to_string("hello.html")` digunakan untuk membaca file HTML, lalu `format!()` untuk menyusun response string, dan `stream.write_all(response.as_bytes())` untuk mengirimkannya ke browser. Sekarang browser bisa menampilkan halaman HTML yang kita buat.

</details>

