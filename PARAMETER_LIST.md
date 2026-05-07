## 1. Sistem Komunikasi, Radio Control, & Hardware
 
  Parameter ini merupakan jantung penghubung antara RadioMaster, Pixhawk, dan Mission Planner:
  
  * SERIALx_PROTOCOL = 23: Digunakan untuk mengubah port Serial menjadi protokol RCIN/CRSF agar mendukung receiver CRSF/ELRS. Catatan untuk dokumentasi: Ganti huruf 'x' dengan port tempat receiver ER6 dicolok, misalnya SERIAL1_PROTOCOL atau SERIAL2_PROTOCOL.
  
  * RC_OPTIONS = 256: Berfungsi untuk mengaktifkan fitur telemetri pada jalur RC agar datanya sinkron.
  
  * BRD_OPTIONS = 3: Mengoptimalkan performa IOMCU agar sistem lebih stabil saat memproses komando, dan mengatur opsi khusus pada flight controller seperti pengaktifan fitur MAVftp.
  
  *  RCMAP_PITCH, RCMAP_ROLL, RCMAP_THROTTLE: Mengubah mapping urutan stik agar sesuai dengan gaya terbang (misalnya AETR atau TAER), sangat berguna jika fungsi channel sempat tertukar.

## 2. Sistem Video & OSD (RunCam WiFiLink / OpenIPC)

Digunakan agar Pixhawk mau mengirim data telemetri/OSD lewat jalur Serial ke VTX:

* OSD_TYPE = 1: Untuk mengaktifkan mesin OSD internal berbasis MAX7456.  


* SERIAL4_PROTOCOL = 42: Mengubah bahasa port Serial 4 menjadi DisplayPort. Jika port yang digunakan bukan Serial 4, ubah angka 4 pada nama parameter ini sesuai port yang dipakai.  


* SERIAL4_BAUD = 115: Menetapkan kecepatan transfer data pada 115200 baudrate.  


* MSP_OPTIONS = 0: Memastikan pengiriman data MSP menggunakan standar tanpa modifikasi khusus.

## 3. Kecepatan Udara & Aerodinamika (Airspeed)

Sangat krusial untuk menjaga aerodinamika pesawat agar tidak mengalami stall di udara:

* AIRSPEED_MIN (atau ARSPD_FBW_MIN) = 15 m/s: Ditetapkan sebagai batas kecepatan udara minimal saat terbang otomatis. Sistem ArduPilot (TECS) akan "melawan" perintah apa pun jika menyuruh pesawat terbang di bawah kecepatan ini, sehingga memberi batas aman (safety margin) dari titik jatuh/stall.  

* AIRSPEED_MAX (atau ARSPD_FBW_MAX) = 22 m/s: Batas kecepatan tertinggi yang didapatkan dari perhitungan Pitch Speed propeller di eCalc agar motor tidak overload dan sayap tidak mengalami getaran struktural (flutter).

* AIRSPEED_CRUISE (atau TRIM_ARSPD_CM) = 17 m/s (1700 cm/s): Merupakan kecepatan target normal pesawat saat terbang antar-waypoint di mode AUTO. Angka ini didapat dari grafik efisiensi eCalc untuk menghemat pemakaian baterai.

* AIRSPEED_STALL = 11 m/s: Titik kecepatan kritis referensi di mana pesawat mulai kehilangan daya angkat. Angka ini dihitung menggunakan Stall Calculator berdasarkan berat dan luas sayap (bukan dari eCalc), dan target kecepatan tidak boleh disetel mendekati angka ini.

## 4. Kalibrasi Mekanik & Servo
Parameter penyesuaian fungsi mekanik dan derajat simpangan sayap:


* SERVOx_FUNCTION: Menentukan alokasi servo mana yang menjadi Aileron (1), Elevator (2), atau Rudder (4).  


* SERVOx_TRIM: Parameter tempat menyimpan titik 1500µs "palsu" guna meluruskan aileron yang miring secara fisik, menggantikan fungsi trim di remot.  


* SERVOx_MIN & SERVOx_MAX: Digunakan untuk membatasi gerak ekstrem aileron agar tidak bablas hingga 60 derajat yang bisa menyebabkan stall.  


* SERVO_AUTO_TRIM = 1: Mengizinkan Pixhawk untuk belajar mencari posisi terbang lurus secara mandiri saat mengudara.

## 5. Navigasi, Misi, & Pendaratan Otomatis (Auto-Landing)
Kumpulan logika untuk menjalankan misi otonom dan pendaratan presisi:

* LAND_PF_ALT = 10 m: Menentukan ketinggian di mana fase pendaratan mulai dieksekusi. Ini adalah titik transisi pesawat dari "turun landai" masuk ke mode "Pre-Flare" bersiap menyentuh tanah.

* LAND_PF_ARSPD = 13.5 atau 14 m/s: Target kecepatan udara (flare) tepat saat roda menyentuh tanah. Angkanya harus di bawah batas minimum agar tidak memantul terbang lagi, tapi di atas batas stall agar tidak jatuh terbanting. Peringatan: Selalu gunakan parameter ini untuk Fixed Wing dan DILARANG menggunakan parameter LAND_SPEED karena sering tertukar dengan sistem Rover/Copter.

* WP_RADIUS: Diperkecil agar radius putar pesawat bisa berbelok tajam tepat di atas koordinat target deteksi YOLO.  

* RTL_ALT: Ditetapkan di ketinggian aman (misal 50-100m) agar pesawat tidak menabrak gedung atau pohon saat mode keamanan RTL/failsafe aktif.  

* TKOFF_THR_MINSPEED: Menetapkan seberapa kecepatan minimal motor sebelum pesawat dilepas saat prosedur hand-launch.
* MIS_RESTART: Mengatur apakah saat masuk ke mode Auto, misi waypoint akan dilanjutkan dari posisi terakhir atau diulang dari titik pertama.
* RELAY_PIN: Konfigurasi alokasi pin khusus aktuator mekanis untuk mengaktifkan servo penjepit saat melakukan mekanisme dropping payload ketika target terdeteksi.
## 6. Pengaturan Telemetri (Stream Rates)
Mengatur seberapa cepat dan efisien data mengalir dari Pixhawk ke Mission Planner di laptop:


* SR1_RC_CHAN = 0 (atau 1, 2): Menurunkan/menghilangkan bandwidth pengiriman data joystick RC ke layar laptop karena misi difokuskan untuk terbang otonom.  


* SR1_POSITION, SR1_EXTRA1, SR1_EXTRA2 = 2 hingga 5 (Hz): Mengatur pembaruan indikator seperti posisi, kecepatan, dan baterai agar di laptop tidak mengalami lag, tetapi radio telemetrinya tidak macet akibat beban berlebih (overload).
## 7. Manajemen Daya, Sensor, & Keamanan (Failsafe)
Parameter untuk memastikan pesawat tidak kehabisan daya di tengah misi:

* BATT_MONITOR = 4: Mewajibkan sistem Pixhawk untuk membaca Tegangan (Volt) sekaligus Arus (Ampere) sesuai jenis sensor yang digunakan.  BATT_CAPACITY: Pengaturan kapasitas baterai total (misalnya 5200 mAh) agar sistem bisa mengkalkulasi persentase sisa daya secara akurat, sekaligus syarat wajib agar fitur perlindungan Battery Failsafe berfungsi.
  
* BATT_VOLT_MULT: Sebagai Voltage Divider atau pengali untuk menyamakan angka bacaan voltase di Mission Planner dengan tegangan asli yang diukur pakai Multimeter.
  
* BATT_AMP_PERVLT: Multiplier arus baterai yang berfungsi mengkalibrasi data Amperemeter. Parameter ini wajib dikalibrasi ulang secara manual dengan mencocokkan "total mAh terbaca di log" vs "total mAh yang diisi oleh charger", DILARANG membiarkan parameter ini di angka bawaan pabrik (18.00).

* ARMING_CHECK: Parameter ini sering diubah menjadi nol saat melakukan perbaikan/ troubleshooting di bengkel/dalam ruangan untuk membypass sistem pengecekan GPS dan Kompas.
  
*   FS_GCS_ENABL = 3 (HeartbeatAndAUTO) Penjelasan: Parameter ini menentukan bagaimana Pixhawk merespons hilangnya sinyal telemetri. Opsi 3 adalah pilihan paling cerdas untuk kompetisi KRTI karena sistem akan tetap memantau sinyal laptop (Heartbeat), namun memberikan pengecualian sakti: jika pesawat sedang dalam Mode AUTO (menjalankan rute waypoint), pesawat akan mengabaikan putusnya sinyal dan tetap gigih menyelesaikan misi hingga tuntas. Failsafe pulang paksa hanya akan aktif jika sinyal putus saat pesawat diterbangkan di mode manual atau FBWA.

*   FS_LONG_ACTN = 1 (ReturnToLaunch) Penjelasan: Parameter ini menentukan tindakan nyata apa yang diambil pesawat setelah mengalami kondisi failsafe dalam durasi lama (biasanya lebih dari 5 detik). Dengan menyetel ke angka 1, pesawat diperintahkan untuk segera melakukan RTL (Return To Launch) atau pulang ke titik Home secara otomatis
