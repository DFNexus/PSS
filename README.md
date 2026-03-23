Danendra Farrel Haryo Wibowo 
A11.2023.15025
A11.4618

Tugas1 PSS

# Dokumentasi Tugas: WordPress, MariaDB, dan Redis Stack (Docker Compose)

Proyek ini bertujuan untuk mengimplementasikan infrastruktur Content Management System (CMS) yang scalable dan memiliki performa tinggi menggunakan Docker Compose di lingkungan Debian 13.


# Langkah-langkah Menjalankan Stack (Deployment)
  Ikuti urutan perintah berikut untuk menjalankan infra di mesin lokal mu:

1. **Masuk ke Direktori Proyek**:
   pada terminal mu, Pastikan kamu ada di folder tempat file `docker-compose.yml` berada.

2. Inisialisasi Infrastruktur:
  Jalankan perintah berikut untuk mengunduh image dan menjalankan kontainer dalam mode background :
  "sudo docker compose up -d"

3. Verifikasi Kontainer:
  Pastikan ketiga layanan (WordPress, DB, Redis) sudah berstatus 'Up' : 
  "sudo docker ps" 

4. Akses Web:
  Buka browser dan akses alamat "http://localhost:8080" untuk memulai proses instalasi WordPress.


# Gamber Dokumentasi Hasil 
1. WordPress Installation Page
  Menunjukkan bahwa server web WordPress sudah berhasil merespons permintaan pada port 8000 :
<img width="504" height="524" alt="image" src="https://github.com/user-attachments/assets/40c9ea2e-3cf4-4eff-a257-b094b89f46f1" />

2. WordPress Dashboard
  Menunjukkan bahwa instalasi telah selesai dan database sudah terhubung dengan benar :
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/14e7a3ed-4173-4d8f-a1fb-9876d3c19fd2" />

3. Docker PS (3 Containers Running)
  Menunjukkan infrastruktur lengkap yang terdiri dari 3 service aktif :
<img width="1343" height="293" alt="Screenshot_20260323_211848" src="https://github.com/user-attachments/assets/dd46365e-f9f8-476d-9dda-a176174f5820" />

4. Redis CLI Ping Test
  Bukti teknis bahwa service Redis merespons permintaan di dalam jaringan Docker dan jika keluarnya "PONG", berarti sudah berhasil:
<img width="420" height="77" alt="image" src="https://github.com/user-attachments/assets/6e52360e-2392-4ba4-8a30-89259b0acd42" />

5. Test add new Post
  jika sudah melakukan langkah sebelumnya, selanjutnya coba melakukan add post, masuk menu post lalu klik tombol bertuliskan "add post",
  jika belum menambahkan apa apa, hanya akan ada 1 post saja dan jika sudah berhasil akan muncul post satu lagi :
  <img width="1920" height="1080" alt="Screenshot_20260323_211823" src="https://github.com/user-attachments/assets/9594addc-1dbd-44e7-900b-d1c3194730f8" />

6. Menambah Redis Object Cache
   Masuk ke menu plugin -> add plugin -> ketik di search "Redis Object cache" -> install Now -> active, dan jika sudah berhasil tampilannya akan seperti ini  :
   <img width="1920" height="1080" alt="Screenshot_20260323_211724" src="https://github.com/user-attachments/assets/bbb118f8-486f-427b-8987-b804262f08d5" />

   - biasanya akan ada kendala redis tidak terbaca, jika masalah itu mucul cukup ketikkan di terminal mu di dalam folder yang sama dengan .yaml nya sebagai berikut : 
    "sudo docker exec -i pss-wordpress-1 sh -c "sed -i \"/stop editing/i define('WP_REDIS_HOST', 'redis');\" wp-config.php"

# Jawaban dari pertanyaan dari Bapak Fahri

1. Kenapa perlu volume untuk MySQL/MariaDB?
  Untuk Data Persistence. Secara default, kontainer bersifat ephemeral (data hilang jika dihapus).
  Volume menyimpan data database secara permanen di penyimpanan fisik host (laptop) agar data tetap aman meskipun kontainer di-reset atau dihentikan.

2. Apa fungsi depends_on di Docker Compose?
  Mengatur urutan startup layanan. Perintah ini memastikan kontainer db dan redis berjalan lebih dulu sebelum wordpress aktif,
  guna menghindari error "Connection Refused" saat aplikasi mencoba menyambung ke database yang belum siap.

3. Bagaimana cara WordPress container connect ke MySQL?
  Melalui Docker Bridge Network menggunakan DNS internal. WordPress memanggil nama layanan (db) sebagai hostname dan melakukan autentikasi
  secara otomatis menggunakan kredensial yang didefinisikan pada bagian Environment Variables di file YAML.

4. Apa keuntungan menggunakan Redis untuk WordPress?
  Sebagai Object Cache berbasis RAM untuk meningkatkan performa sistem melalui:

 - Kecepatan: Mempercepat loading halaman dengan menyimpan hasil query di RAM.

 - Efisiensi: Mengurangi beban kerja proses pada database MariaDB.

 - Scalability: Memungkinkan situs menangani lebih banyak trafik dengan penggunaan CPU yang minimal.
