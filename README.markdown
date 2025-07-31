# Sistem Deteksi Wilayah Rentan di Jawa Timur: Integrasi Clustering K-Means dan LightGBM untuk Mengukur Ketimpangan Akses Pendidikan dan Ekonomi

## Proyek
**Judul**: Sistem Deteksi Wilayah Rentan di Jawa Timur: Integrasi Clustering K-Means dan LightGBM untuk Mengukur Ketimpangan Akses Pendidikan dan Ekonomi
  
**Topik**: Analisis spasial dan prediktif tingkat kerentanan wilayah Jawa Timur berbasis data sosial ekonomi dan pendidikan untuk mendukung perencanaan pembangunan berbasis bukti dan intervensi kebijakan yang terarah.
**Tujuan**: Proyek ini dirancang untuk mengembangkan sistem deteksi wilayah rentan di Provinsi Jawa Timur dengan memanfaatkan data sosial ekonomi dan pendidikan kabupaten/kota Jawa Timur (2020–2024) untuk mengelompokkan wilayah dengan K-Means dan memprediksi tingkat kerentanan menggunakan LightGBM, disertai visualisasi dan rekomendasi kebijakan berbasis data untuk proyeksi tahun 2025.

**Anggota Tim (TIM PEMULA)**:  
1. Zulma Nayla Ifaada (23031554063)
2. Nazril Ravi Pratama (24031554129)
3. Muhammad Habib Nur Aiman (24031554152)

## Tujuan
1. Mengembangkan sistem deteksi wilayah rentan berbasis data tabular periode 2020–2024 dari kabupaten/kota di Provinsi Jawa Timur.
2. Menerapkan metode Clustering K-Means untuk mengelompokkan wilayah berdasarkan kemiripan kondisi sosial ekonomi dan akses pendidikan.
3. Menggunakan algoritma LightGBM sebagai model klasifikasi untuk memprediksi tingkat kerentanan wilayah, termasuk proyeksi kondisi tahun 2025.
4. Menyediakan analisis dan rekomendasi kebijakan berbasis data dan visualisasi.


## Struktur Folder
```
📁 [Nama Repo]/
├── README.md               # Dokumentasi proyek (file ini)
├── [].ipynb                # Pengaturan konfigurasi Celery
├── [].ipynb                # Pengaturan konfigurasi Celery           
├── db_config.py            # Konfigurasi koneksi database
├── celery_app.py           # Menginisialisasi aplikasi Celery
├── run_task.py             # Skrip untuk memicu tugas Celery
├── df_order_clean.csv      # Data pesanan yang telah dibersihkan
├── df_order_item_clean.csv # Detail item pesanan yang telah dibersihkan
├── df_order_payment_bersih.csv # Data pembayaran yang telah dibersihkan
├── product_clean.csv       # Data produk yang telah dibersihkan
├── ecom_analytics_task1.xlsx # Keluaran: Rata-rata pendapatan per kuartal
├── ecom_analytics_task2.xlsx # Keluaran: Produk paling sering terjual per bulan
├── ecom_analytics_task3.xlsx # Keluaran: Performa penjualan bulanan
├── ecom_analytics_task4.xlsx # Keluaran: Tingkat stok berdasarkan produk terjual
├── ecom_analytics_task5.xlsx # Keluaran: Prediksi restok berdasarkan ambang minimum penjualan
├── Data.py                 # Load data ke database
├── celery_task.log                 # Celery_tasks.log
├── queries.log                 # Quueries.log
```

## Task
1. **Menghitung Rata-Rata Per Kuartal**  
   - Menghitung rata-rata pendapatan per kuartal menggunakan data pembayaran.  
   - Keluaran: `ecom_analytics_task1.xlsx`
2. **Produk Terlaris dari Modus Pendapatan**  
   - Mengidentifikasi produk yang paling sering terjual setiap bulan berdasarkan frekuensi penjualan.  
   - Keluaran: `ecom_analytics_task2.xlsx`
3. **Performa Penjualan Bulanan**  
   - Menganalisis performa penjualan bulanan (misalnya, total pendapatan, jumlah pesanan).  
   - Keluaran: `ecom_analytics_task3.xlsx`
4. **Stok Barang Berdasarkan Produk yang Terjual**  
   - Memantau tingkat stok dengan menghitung unit terjual per produk.  
   - Keluaran: `ecom_analytics_task4.xlsx`
5. **Prediksi Restok Berdasarkan Ambang Minimum Penjualan**  
   - Memprediksi kebutuhan restok dengan membandingkan stok dengan ambang minimum penjualan.  
   - Keluaran: `ecom_analytics_task5.xlsx`

## Syarat
- **PythonលPython**: Versi 10.8 atau lebih tinggi
- **Dependensi**:
  - `pandas`: Untuk manipulasi dan analisis data
  - `celery`: Untuk manajemen tugas queue-worker
  - `psycopg2`: Untuk driver database pada Python
  - `openpyxl` atau `xlsxwriter`: Untuk keluaran file Excel
- **Opsional**: Database (misalnya, PostgreSQL, MySQL) jika tidak menggunakan file CSV secara langsung

## Instruksi Pengaturan
1. **Kloning Repositori**  
   - Pastikan semua file berada di direktori `Datawerehouse/`.
2. **Instal Dependensi**  
   - Jalankan perintah berikut:  
     ```
     pip install pandas celery openpyxl
     ```
3. **Konfigurasi Database**  
   - Edit `db_config.py` untuk mengatur kredensial database (misalnya, host, port, pengguna, kata sandi).  
   - Jika menggunakan file CSV, konfigurasi database tidak diperlukan.
4. **Konfigurasi Celery**  
   - Perbarui `celeryconfig.py` dengan URL broker RabbitMQ (misalnya, `amqp://guest:guest@localhost:5672//`).  
   - Pastikan layanan RabbitMQ berjalan.
5. **Jalankan RabbitMQ** 
   - Instal Erlang OTP (https://github.com/erlang/otp/releases/download/OTP-25.3/otp_win64_25.3.exe) 
   - Instal RabbitMQ (unduh dari https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.11.11/rabbitmq-server-3.11.11.exe).  
   - Jalankan server RabbitMQ:  
     ```
     rabbitmq-server
     ```
6. **Jalankan Worker Celery**  
   - Jalankan perintah berikut dari direktori `Datawerehouse/`:  
     ```
     celery -A tasks flower --port=5555
     ```
7. **Jalankan Utama**  
   - Jalankan tugas secara individu:  
     ```
     celery -A tasks worker -Q queue1 --loglevel=info --pool=solo --hostname=worker1@%h
     celery -A tasks worker -Q queue2 --loglevel=info --pool=solo --hostname=worker1@%h
     celery -A tasks worker -Q queue3 --loglevel=info --pool=solo --hostname=worker2@%h
     celery -A tasks worker -Q queue4 --loglevel=info --pool=solo --hostname=worker2@%h
     celery -A tasks worker -Q queue5 --loglevel=info --pool=solo --hostname=worker3@%h
     ```
   - Atau jalankan skrip utama untuk mengatur semua tugas:  
     ```
     python main.py
     ```

## Penggunaan
- **Menjalankan Tugas**: Gunakan `celery -A tasks worker` untuk memicu tugas Celery tertentu yang didefinisikan di `tasks.py`.
- **Menguji Kueri**: Jalankan `test_query.py` untuk memvalidasi kueri di `queries.py` terhadap data Anda.
- **Keluaran**: Periksa file `ecom_analytics_task1.xlsx` hingga `ecom_analytics_task5.xlsx` untuk hasil.
- **Sumber Data**: Sistem menggunakan file CSV yang telah dibersihkan (`df_order_clean.csv`, dll.) untuk analisis.

## Catatan
- Pastikan file CSV konsisten (misalnya, ID pesanan dan produk yang cocok).
- Uji dengan data kecil terlebih dahulu untuk memverifikasi fungsionalitas.
- Jika menggunakan database, amankan kredensial di `db_config.py` (misalnya, gunakan variabel lingkungan).
- Hasil disimpan sebagai file Excel untuk memudahkan peninjauan dan pelaporan.

## Pemecahan Masalah
- **Celery Tidak Terhubung**: Pastikan RabbitMQ berjalan dan URL di `celeryconfig.py` benar (misalnya, `amqp://guest:guest@localhost:5672//`).
- **Data Hilang**: Periksa file CSV untuk nilai yang hilang atau tidak konsisten.
- **Kesalahan di Keluaran**: Jalankan `test_query.py` untuk mendiagnosis kueri dan logika tugas.

## Lisensi
Proyek ini disusun sebagai kontribusi akademik dalam rangka keikutsertaan pada kompetisi Statistics Essay Competition (SEC), salah satu cabang lomba dalam ajang Satria Data.