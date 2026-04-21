## Module 06 - Concurrency

<details><summary><b>Milestone 1: Single threaded web server</b></summary>

Method `handle_connection` membaca data dari `TcpStream` menggunakan `BufReader`. Method `lines()` digunakan untuk membaca request line per line, dan `take_while(|line| !line.is_empty())` digunakan untuk berhenti membaca ketika menemukan empty line, karena HTTP request diakhiri dengan dua newline berturut-turut. Output yang muncul di terminal menunjukkan struktur HTTP request seperti `GET / HTTP/1.1`, diikuti berbagai headers seperti `Host`, `User-Agent`, dan sebagainya.

Saat pertama kali menjalankan server (sebelum ada method `handle_connection`), muncul beberapa message "Connection established!" sekaligus, itu terjadi karena browser secara otomatis mencoba mengirim ulang request ketika tidak mendapat response yang proper dalam waktu tertentu.

</details>

<details><summary><b>Milestone 2: Returning HTML</b></summary>

![Commit 2 screen capture](/assets/images/commit2.png)

Metode `handle_connection` dimodifikasi agar server bisa mengirimkan response HTML yang proper ke browser, bukan hanya menerima request saja. Struktur dari HTTP response terdiri dari status line (`HTTP/1.1 200 OK`), diikuti headers (`Content-Length`), lalu dua `\r\n` sebagai separator, baru kemudian body (HTML). `Content-Length` penting karena browser menggunakannya untuk tahu berapa byte yang harus dibaca sebagai body. Di sini `fs::read_to_string("hello.html")` digunakan untuk membaca file HTML, lalu `format!()` untuk menyusun response string, dan `stream.write_all(response.as_bytes())` untuk mengirimkannya ke browser. Sekarang browser bisa menampilkan halaman HTML yang kita buat.

</details>

<details><summary><b>Milestone 3: Validating request and selectively responding</b></summary>

![Commit 3 screen capture](/assets/images/commit3.png)

Server yang tadinya selalu mengembalikan `hello.html` untuk request apapun, sekarang bisa membedakan request mana yang valid dan mana yang tidak. Method `handle_connection` dimodifikasi dengan menambahkan `let request_line = &http_request[0]` untuk mengambil baris pertama HTTP request (`http_request`). Dari baris ini kita bisa tahu method apa yang dipakai (`GET`), path mana yang diminta (`/`), dan versi HTTPnya (`1.1`). Kalau requestnya ke path `/`, akan dikembalikan `hello.html` dengan status `200 OK`. Selain itu (misalnya misalnya `GET /bad HTTP/1.1`), akan dikembalikan `404.html` dengan status `404 NOT FOUND`.

Dalam memodifikasi method ini, dilakukan refactoring karena kode untuk membaca file, menghitung length, menyusun response, dan menulis ke stream untuk kedua case sama persis. Daripada ditulis dua kali, bagian yang berbeda (`status_line` dan `filename`) dijadikan conditional, sementara bagian yang sama cukup ditulis sekali di luar conditional block.

</details>

<details><summary><b>Milestone 4: Simulation slow response</b></summary>

Di sini ditambahkan simulasi slow request dengan menambahkan endpoint `/sleep` yang menggunakan `thread::sleep(Duration::from_secs(10))` di mana akan membuat server "tidur" selama 10 detik sebelum mengirim response.
Dapat terlihat jelas masalah dari single-threaded server, yaitu ketika satu tab browser membuka `/sleep`, tab lain yang mencoba membuka `/` harus menunggu sampai request `/sleep` selesai dulu, padahal request `/` seharusnya bisa diproses dengan sangat cepat. Ini karena server hanya bisa menangani satu request dalam satu waktu secara sequential. Jika ada lebih banyak user lagi yang mengakses secara bersamaan, akan lebih kacau lagi. Inilah mengapa kita perlu multithreaded server.

</details>


