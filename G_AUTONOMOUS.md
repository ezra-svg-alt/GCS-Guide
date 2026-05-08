## 1. Roadmap Persiapan Autonomous

* Fase Darat (Wajib Lulus di Bengkel):

A. Center of Gravity (CG)

B. Kalibrasi Sensor Dasar: Accelerometer (agar Pixhawk tahu posisi datar), Compass (agar tahu arah Utara/Selatan, jauhkan dari logam saat kalibrasi!), dan Radio (mengenalkan batas mentok stik remotemu ke Pixhawk).

C. Kalibrasi Airspeed Sensor (Pitot Tube): Tiup pelan selang Pitot dan pastikan angkanya naik di Mission Planner.

D. Set Saklar Flight Mode: Siapkan saklar di remotemu untuk 3 mode wajib: FBWA (Fly-By-Wire A, mode stabilisasi), AUTOTUNE, dan AUTO.

* Fase Udara (Eksekusi Lapangan):

JANGAN PERNAH langsung terbang mode AUTO di penerbangan perdana!

A. Langkah 1: Lepas landas pakai mode FBWA. Rasakan apakah pesawat sudah stabil atau masih liar.

B. Langkah 2 (Mode Autotune): Bawa pesawat agak tinggi, lalu aktifkan saklar AUTOTUNE. Gerakkan stik Roll (kiri-kanan) dan Pitch (naik-turun) sampai mentok berkali-kali. Biarkan Pixhawk mempelajari kelenturan sayap gabusmu dan menghitung parameter PID-nya sendiri.

C. Langkah 3: Setelah pendaratan selesai, uji terbang lagi. Kalau stabilnya sudah seperti melayang di atas rel, barulah kamu boleh menggambar rute dan mengaktifkan mode AUTO.

## 2. Dropping Paket
Secara teknis software, BISA BANGET diakali tanpa kamera!

* Jalur Curang (GPS/Loiter): Di Mission Planner, kamu tinggal menaruh waypoint tepat di atas titik koordinat terpal target. Di titik tersebut, kamu tambahkan perintah DO_SET_SERVO (atau DO_DIGICAM_CONTROL) untuk memutar servo yang menjepit paket. Selesai. Pesawat akan membuang paket berdasarkan koordinat GPS buta.

* Realita Lapangan (Kenapa Butuh Citra?): Akurasi GPS bawaan pesawat itu bisa meleset 2 hingga 5 meter. Belum lagi paket yang jatuh akan tertiup angin (efek parabola). Kalau target jatuhnya harus sangat presisi (masuk ke dalam lingkaran kecil terpal), membuang secara buta pakai GPS pasti meleset jauh.

* Jalan Ninja Juara: Menggunakan Computer Vision (seperti mendeteksi objek dengan model YOLOv8). Kamera akan mengunci warna/bentuk terpal secara real-time di bawah pesawat, lalu menghitung kecepatan angin, dan memerintahkan servo menjatuhkan paket di milidetik yang paling sempurna. Ini jauh lebih sulit, tapi ini yang dinilai tinggi oleh juri.
