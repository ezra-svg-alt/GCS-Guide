## 1. Persiapan dari Segi Remot (RadioMaster TX12)
* Bersihkan "Mixer" Remot. Dia ingin menerima sinyal mentah. Pastikan di remotemu TIDAK ADA settingan Elevon, V-Tail, atau Expo/Dual Rates yang aneh-aneh.
Biarkan 100% lurus (linear), biar Pixhawk yang mengkalkulasi semuanya.

* Siapkan Saklar (Switch) Flight Mode: Kamu harus mendedikasikan satu saklar fisik 3-posisi (misalnya saklar SE atau SF di TX12) dan memasukkannya ke CH5 di halaman Mixes. CH5 adalah jalur default ArduPlane untuk mengganti Flight Mode.


## 2. Persiapan dari Segi Mission Planner 
* Setup Flight Mode: Masuk ke menu SETUP > Flight Modes. Gunakan saklar CH5 yang sudah kamu buat tadi, lalu atur minimal 3 mode dasar ini:

  A. Posisi Saklar Atas: MANUAL (Wajib ada untuk take-off atau emergency bypass kalau sensor ngaco).

  B. Posisi Saklar Tengah: FBWA.

  C. Posisi Saklar Bawah: RTL (Return to Launch) atau Auto.

* Kalibrasi Level (Sangat Krusial): Masuk ke Mandatory Hardware > Accel Calibration > Calibrate Level. Pastikan pesawat Werkudara diletakkan di lantai/meja yang 100% rata air saat diklik.
  Kalau kalibrasi levelnya miring, di mode FBWA pesawatmu akan terbang miring terus!

   <img width="874" height="178" alt="image" src="https://github.com/user-attachments/assets/a4807da0-a484-4220-a56b-4f7373eac582" />
* Atur Limit Sudut (Opsional tapi penting):Cari parameter.
    * ROLL_LIMIT_DEG = 45
    * PTCH_LIM_MAX_DEG = 25
    * PTCH_LIM_MIN_DEG = -18

## 3. Uji Coba Fisik
  1.Nyalakan remot dan colok baterai pesawat (propeller/baling-baling WAJIB DIBUKA dulu demi keamanan).

  2.Cetek saklar remotmu ke mode FBWA.

  3.Pegang pesawat pakai tangan, lalu miringkan sayap kanannya ke bawah (simulasi terkena angin).

  * Reaksi Wajib: Sayap/Aileron kanan harus TURUN (seolah-olah ingin menekan angin ke bawah agar sayapnya naik lagi ke posisi rata). Kalau Aileron kanannya malah naik, pesawatmu akan roll guling-guling sampai jatuh!

  4.Menukikkan Hidung ke bawah.
  * Reaksi Wajib: Elevator (ekor belakang) harus NAIK untuk mengangkat hidungnya kembali.
