## Module 06 - Concurrency

<details><summary><b>Milestone 1: Single threaded web server</b></summary>

Method `handle_connection` membaca data dari `TcpStream` menggunakan `BufReader`. Method `lines()` digunakan untuk membaca request line per line, dan `take_while(|line| !line.is_empty())` digunakan untuk berhenti membaca ketika menemukan empty line, karena HTTP request diakhiri dengan dua newline berturut-turut. Output yang muncul di terminal menunjukkan struktur HTTP request seperti `GET / HTTP/1.1`, diikuti berbagai headers seperti `Host`, `User-Agent`, dan sebagainya.

Saat pertama kali menjalankan server (sebelum ada method `handle_connection`), muncul beberapa message "Connection established!" sekaligus, itu terjadi karena browser secara otomatis mencoba mengirim ulang request ketika tidak mendapat response yang proper dalam waktu tertentu.

</details>
