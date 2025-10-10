# Model Deployment Netflix Project

Proyek ini bertujuan untuk membangun dan menerapkan sistem rekomendasi film dan acara TV berdasarkan konten yang ada di Netflix. Proyek ini dikerjakan oleh Group 4:

2702223084 - Jonathan Christopher Gani

2702274495 - Kevin Gabriel Wiharja

2702258333 - Hartono Yaputra

## 1. Pembersihan Data & Analisis Data Eksplorasi (EDA)

Tahap awal dari proyek ini adalah pembersihan dan analisis data untuk memastikan data yang digunakan berkualitas baik dan siap untuk pemodelan. Proses ini mencakup beberapa langkah:

Memuat Data: Dataset yang digunakan adalah netflix_titles.csv yang berisi informasi tentang film dan acara TV di Netflix.

Konversi Format Data: Kolom date_added diubah formatnya menjadi tipe data tanggal (datetime) untuk memudahkan pemrosesan.

Menghapus Kolom Identifier: Kolom show_id dihapus karena tidak relevan untuk proses pemodelan dan dapat menyulitkan deteksi data duplikat.

Pemeriksaan dan Perbaikan Nilai Kolom: Beberapa nilai yang tidak sesuai pada kolom rating (misalnya, '74 min', '84 min') diidentifikasi dan dihapus dari dataset.

Menghapus Kolom yang Tidak Digunakan: Kolom seperti director, country, date_added, dan duration dihapus karena sistem rekomendasi ini akan fokus pada atribut konten seperti cast, listed_in, dan description.

Penanganan Nilai yang Hilang: Baris data yang memiliki nilai kosong pada kolom cast dan rating dihapus karena sulit untuk diimputasi dan dapat menyebabkan bias pada model.

Analisis Distribusi Data: Dilakukan analisis distribusi pada kolom numerik (release_year) dan kategorikal (type, rating) untuk memahami karakteristik data.

Setelah melalui proses pembersihan, dataset siap untuk tahap selanjutnya, yaitu pembangunan sistem rekomendasi.

## 2. Sistem Rekomendasi Berbasis Konten

Sistem rekomendasi ini dibangun dengan pendekatan Content-Based Filtering, yang merekomendasikan item berdasarkan kemiripan atributnya. Langkah-langkahnya adalah sebagai berikut:

Pemilihan Fitur: Fitur-fitur yang relevan seperti listed_in (genre), title (judul), rating, dan description (deskripsi) digabungkan menjadi satu teks tunggal untuk setiap item.

Vektorisasi Teks dengan TF-IDF: TfidfVectorizer digunakan untuk mengubah data teks gabungan menjadi representasi numerik (vektor). Proses ini juga mengabaikan kata-kata umum dalam bahasa Inggris (stop words) yang tidak memberikan banyak informasi.

Perhitungan Kemiripan Kosinus: cosine_similarity dihitung dari matriks TF-IDF untuk mengukur seberapa mirip satu film/acara TV dengan yang lainnya. Skor kemiripan ini berkisar antara 0 (tidak mirip) hingga 1 (sangat mirip).

## 3. Implementasi Sistem Rekomendasi
 
Sebuah fungsi bernama recommend_system dibuat untuk menghasilkan rekomendasi. Fungsi ini menerima judul film/acara TV sebagai masukan dan memberikan daftar 5 item yang paling mirip berdasarkan skor kemiripan kosinus.

Contoh penggunaan:

```bash

Python

# Memberikan rekomendasi untuk "InuYasha the Movie 4: Fire on the Mystic Island"
recommend_system('InuYasha the Movie 4: Fire on the Mystic Island')

# Memberikan rekomendasi untuk "Naruto Shippuden the Movie: Blood Prison"
recommend_system('Naruto Shippuden the Movie: Blood Prison')
Hasil dari fungsi ini adalah sebuah DataFrame yang berisi judul, tipe, pemeran, rating, genre, dan skor kemiripan dari item yang direkomendasikan.

```

## 4. Persiapan Deployment Model

Untuk mempersiapkan model agar dapat digunakan di aplikasi lain (misalnya, aplikasi web), semua komponen penting dari sistem rekomendasi disimpan ke dalam sebuah file netflix_recommender.pkl menggunakan library pickle. Komponen yang disimpan meliputi:

tfidf: Objek TfidfVectorizer yang sudah di-fit.

cosine_sim: Matriks kemiripan kosinus.

dataset: DataFrame yang sudah dibersihkan.

indices: Pemetaan antara judul dan indeks dalam dataset.

Dengan menyimpan komponen-komponen ini, model dapat dimuat kembali dan digunakan untuk memberikan rekomendasi tanpa perlu melatih ulang dari awal.
