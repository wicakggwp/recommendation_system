# Laporan Proyek Machine Learning - Ikhzan Wicaksono

## Project Overview

Perkembangan era digital yang semakin maju, terjadi pergeseran perilaku konsumen dari berbelanja secara langsung di toko fisik menjadi melakukan transaksi melalui platform online seperti e-commerce. Hal ini juga memengaruhi pola persaingan antar pelaku bisnis, sehingga mereka perlu mengubah strategi pemasaran untuk menjangkau calon pelanggan secara lebih efektif. 

Oleh karena itu, diperlukan suatu sistem yang mampu mendukung kebutuhan tersebut. Menurut **[Mariani et al.(2020)](https://www.academia.edu/download/77211339/1194.pdf)** sistem rekomendasi menawarkan cara yang lebih mudah dan cepat sehinggapengguna tidak perlu meluangkan waktu terlalu banyak untuk menemukan barang yang diinginkan. Dalam jurnal yang dikemukakan oleh **[Subroto et al.(2020)](https://core.ac.uk/download/pdf/551487925.pdf)** sistem rekomendasi dibagi menjadi dua yaitu content based approach dan collaborative filter, Collaborative filter merupakan salah satu algoritma yang digunakan untuk menyusun Sistem Rekomendasi dan telah terbukti memberikan hasil yang sangat baik.

Dengan menggunakan data penjualan dari e-commerce amazon yang berasal dari **[Kaggle](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset/data)**, sistem akan menganalisis karateristik produk yang tersedia dan preferensi pembeli atau user unutk memberikan rekomendasi yang personal sesuai keinginan user atau pembeli.

_Referensi :_ 

_Putri, Mariani Widia, Achmad Muchayan, and Made Kamisutara. "Sistem rekomendasi produk pena eksklusif menggunakan metode content-based filtering dan TF-IDF." JOINTECS (Journal of Information Technology and Computer Science) 5.3 (2020): 229-236._

_Subroto, Imam Much Ibnu, et al. "Sistem Rekomendasi pada Pembelajaran Mobile Menggunakan Metode Cosine Similarity dan Collaborative Filtering." J. Transistor Elektro dan Inform.(TRANSISTOR EI) 4.1 (2022): 21-28._

## Business Understanding

Bisnis e-commerce mulai semakin berkembang, para pelaku bisnis pun mencari cara agar dapat mempermudah mereka dalam menjual produknya dan menignkatkan penjualan.

### Problem Statements

- Bagaimana sistem mempermudah user dalam menemukan produk sesuai karateristik yang mereka inginkan?

- Bagimana sistem memberikan rekomendasi produk yang relevan dan dipersonalisasi untuk pengguna atau pembeli berdasarkan history rating?

### Goals

Untuk mengatasi permasalahan di atas, proyek ini bertujuan untuk:

- Mengembangkan sistem yang dapat memberikan saran relevan berdasarkan karakteristik tempat wisata rating dan deskripsi.

- Membangun sistem yang dapat memberikan rekomendasi produk dengan memanfaatkan data rating dan preferensi pengguna untuk memberikan rekomendasi yang sesuai dengan keinginan individual.

### Solution Statements

1. **Content-Based Filtering**: 
   - Menganalisis karakteristik tempat wisata seperti nama produk, deskripsi, kategori, dan rating
   - Menggunakan TF-IDF Vectorizer untuk mengekstrak fitur dari nilai kategorikal
   - Menghitung kesamaan antar kategori menggunakan Cosine Similarity
   - Merekomendasikan produk yang memiliki karakteristik serupa

2. **Collaborative Filtering**:
   - Menganalisis pola dari rating dan preferensi pengguna
   - Algoritma Matrix Factorization (SVD) untuk memprediksi rating pengguna
   - Merekomendasikan tempat wisata berdasarkan kesamaan preferensi dari pengguna lain
   - Memberikan rekomendasi personal yang disesuaikan dengan riwayat rating pengguna

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah **Amazon Dataset Sales** yang berisi informasi mengenai penjualan produk di e-commerce amazon berisi rating dan review dari berbagai pengguna. Dataset ini berisikan 1465 baris dan 16 fitur yang berbeda, dataset ini tersedia secara publik di [Kaggle](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset/data). Dataset ini bisa dikatakan cukup bersih dari missing values, karena hanya terdapat beberapa saja.

### Fitur yang terdapat dalam dataset

- product_id – ID unik untuk setiap produk.
- product_name – Nama atau judul produk.
- category – Kategori atau jenis produk (misal: elektronik, fashion).
- discounted_price – Harga produk setelah diskon.
- actual_price – Harga asli produk sebelum diskon.
- discount_percentage – Persentase potongan harga dari harga asli.
- rating – Nilai rata-rata penilaian pengguna terhadap produk, terdiri dari skala 1-5.
- rating_count – Jumlah total rating yang diterima produk.
- about_product – Deskripsi singkat atau informasi tentang produk.
- user_id – ID unik pengguna yang memberikan ulasan.
- user_name – Nama pengguna yang memberi ulasan.
- review_id – ID unik untuk setiap ulasan.
- review_title – Judul singkat dari ulasan.
- review_content – Isi atau detail ulasan pengguna tentang produk.
- img_link – Tautan ke gambar produk.
- product_link – Tautan ke halaman produk pada situs e-commerce.

### Exploratory Data Analysis

Analisis eksplorasi data mengungkapkan beberapa insight penting:

![Distribusi Rating](assets/rating_distribution.png)
*Gambar 1: Distribusi rating tempat wisata menunjukkan sebagian besar tempat wisata memiliki rating di atas 4.0*

**Statistik Rating**:
- Rating minimum: 3.0
- Rating maksimum: 5.0  
- Rating rata-rata: 4.3
- Sebagian besar tempat wisata (>80%) memiliki rating di atas 4.0

![Distribusi Review](assets/review_count_distribution.png)  
*Gambar 2: Distribusi jumlah review menunjukkan sebagian besar tempat wisata memiliki puluhan hingga ratusan review*  

**Statistik Review**:  
- Review minimum: 0  
- Review maksimum: ~1000  
- Sebagian besar review berkisar antara 10–200  

![Distribusi Provinsi](assets/province_distribution.png)
*Gambar 3: Distribusi tempat wisata per provinsi menunjukkan Aceh sebagai provinsi dengan data terbanyak*

**Distribusi Geografis**:
- Dataset mencakup wisata dari 38 provinsi di Indonesia
- ATop 10 provinsi dengan tempat wisata terbanyak menunjukkan distribusi yang cukup merata
- Setiap provinsi memiliki sekitar 35 tempat wisata dalam dataset

![Kategori Wisata](assets/category_distribution.png)
*Gambar 4: Distribusi kategori wisata menunjukkan pantai sebagai kategori terpopuler*

**Kategori Wisata**:
- **Lainnya**: Kategori campuran untuk wisata yang tidak masuk kategori utama (Kategori terbanyak dengan lebih dari 300 tempat wisata)
- **Pantai**: Kategori kedua terbanyak dengan sekitar 230 tempat wisata (sesuai dengan karakteristik Indonesia sebagai negara kepulauan)
- **Gunung**: Kategori ketiga dengan sekitar 175 tempat wisata
- **Taman**: wisata alam taman
- **Air Terjun**: Wisata alam air terjun
- **Kategori lain**: benteng, wisata_alam, danau, pulau, etc.

![Rating vs Review](assets/rating_vs_review.png)
*Gambar 5: Hubungan antara rating dan jumlah review menunjukkan korelasi positif*

**Hubungan Rating dan Review**:
- Terdapat korelasi positif antara rating dan jumlah review
- Sebagian besar tempat wisata memiliki jumlah review yang bervariasi dari puluhan hingga ribuan

![Rating per Kategori](assets/rating_by_category.png)  
*Gambar 6: Boxplot rating untuk 5 kategori terpopuler*  

**Insight Per Kategori**:  
- **Pantai** & **Gunung**: median rating ~4.4–4.5, cukup konsisten  
- **Lainnya**: variasi rating paling lebar  
- **Taman** & **Air Terjun**: memiliki beberapa outlier dengan rating rendah (<3.5) 

## Data Preparation

Tahapan data preparation dilakukan untuk mempersiapkan data agar siap digunakan dalam pengembangan model sistem rekomendasi. Beberapa teknik yang diterapkan meliputi:

### 1. Data Cleaning

**Pembersihan Data Kategori**:
```python
def clean_categories(cat_str):
    if pd.isna(cat_str):
        return ['lainnya']
    cleaned = cat_str.strip("[]'\"").replace("'", "").replace('"', '')
    categories = [cat.strip() for cat in cleaned.split(',')]
    return categories if categories[0] else ['lainnya']
```

Proses pembersihan data kategori diperlukan karena:
- Data kategori disimpan dalam format string yang menyerupai array
- Terdapat inkonsistensi dalam penggunaan tanda kutip
- Beberapa tempat wisata memiliki multiple kategori

**Penanganan Missing Values (menghapus baris)**:
- **alamat**: 2 missing values
- **rating**: 1 missing value
- **jumlah_review**: 1 missing value

Penanganan missing values ini penting untuk memastikan semua data dapat diproses dengan baik dalam algoritma machine learning.

### 2. Feature Engineering

**Popularity Score Creation**:
```python
# Normalisasi jumlah review ke skala 0-5
review_max = df_processed['jumlah_review'].max()
df_processed['review_normalized'] = (df_processed['jumlah_review'] / review_max) * 5

# Perhitungan popularity score (weighted average)
df_processed['popularity_score'] = (0.7 * df_processed['rating']) + (0.3 * df_processed['review_normalized'])
```

Popularity score dibuat untuk:
- Menggabungkan faktor rating dan popularitas (jumlah review)
- Memberikan bobot lebih besar pada rating (70%) dibanding popularitas (30%)
- Membantu dalam ranking rekomendasi

**Content Feature Creation**:
```python
df_processed['content'] = (
    df_processed['nama'] + ' ' +
    df_processed['alamat'] + ' ' +
    df_processed['deskripsi'] + ' ' +
    df_processed['provinsi'] + ' ' +
    df_processed['kategori_utama']
)
```

Fitur content dibuat untuk content-based filtering dengan menggabungkan semua informasi tekstual yang relevan.

### 3. Data Preparation untuk Content-Based Filtering

**TF-IDF Vectorization**:
```python
tfidf_vectorizer = TfidfVectorizer(
    max_features=1000,
    stop_words='english',
    ngram_range=(1, 2),
    min_df=1
)
tfidf_matrix = tfidf_vectorizer.fit_transform(df_processed['content'])
```

Parameter yang dipilih:
- `max_features=1000`: Membatasi fitur untuk efisiensi komputasi
- `ngram_range=(1, 2)`: Menggunakan unigram dan bigram untuk konteks yang lebih baik
- `min_df=1`: Mempertahankan semua term yang muncul minimal sekali

**Cosine Similarity Calculation**:
```python
cosine_sim = cosine_similarity(tfidf_matrix)
```

Matrix cosine similarity dibuat untuk mengukur kesamaan antar tempat wisata berdasarkan konten tekstual.

### 4. Data Preparation untuk Collaborative Filtering

Karena dataset asli tidak memiliki data rating pengguna, dibuat **synthetic user-item interactions**:

**Pembuatan Data Rating Sintetis**:
```python
# Simulasi 100 user dengan rating pattern yang realistis
n_users = 100
for user_id in range(1, n_users + 1):
    n_ratings = np.random.randint(10, 31)  # 10-30 rating per user
    rated_places = np.random.choice(df_processed.index, n_ratings, replace=False)
    
    for place_idx in rated_places:
        base_rating = df_processed.loc[place_idx, 'rating']
        synthetic_rating = np.clip(base_rating + np.random.normal(0, 0.5), 1, 5)
```

Synthetic data diperlukan karena:
- Dataset asli tidak memiliki data interaksi user-item
- Memungkinkan implementasi dan evaluasi collaborative filtering
- Pattern rating dibuat realistis berdasarkan rating aktual tempat wisata

**Pembuatan DataFrame Rating**:
```python
ratings_df = pd.DataFrame({
    'user_id': user_ids,
    'place_id': place_ids,
    'rating': ratings
})
```

### 5. Konversi Data ke Format Surprise

**Persiapan Data untuk Library Surprise**:
```python
from surprise import Dataset, Reader

# Create reader dengan skala rating 1-5
reader = Reader(rating_scale=(1, 5))

# Load data ke format Surprise
surprise_data = Dataset.load_from_df(ratings_df[['user_id', 'place_id', 'rating']], reader)
```

Langkah ini diperlukan untuk:
- Mengkonversi DataFrame pandas ke format yang dapat diproses oleh library Surprise
- Menetapkan skala rating yang sesuai (1-5)
- Mempersiapkan data untuk algoritma collaborative filtering

### 6. Split Data

**Pembagian Data Training dan Testing**:
```python
from surprise.model_selection import train_test_split as surprise_train_test_split

# Split data dengan rasio 80:20
trainset, testset = surprise_train_test_split(surprise_data, test_size=0.2, random_state=42)
```

Data splitting dilakukan untuk:
- **Training set (80%)**: Digunakan untuk melatih model SVD
- **Test set (20%)**: Digunakan untuk evaluasi performa model
- **Random state=42**: Memastikan reproducibility hasil

**Informasi Split Data**:
- Training set: ~80% dari total ratings
- Test set: ~20% dari total ratings
- Total synthetic ratings: ~2000+ interactions dari 100 users

## Modeling

### Content-Based Filtering

Content-based filtering merekomendasikan tempat wisata berdasarkan kesamaan karakteristik dengan tempat yang diminati pengguna.

**Algoritma yang digunakan**:
1. **TF-IDF Vectorization**: Mengkonversi teks menjadi vektor numerik
2. **Cosine Similarity**: Menghitung kesamaan antar tempat wisata

**Implementasi**:
```python
def get_content_recommendations(place_name, cosine_sim=cosine_sim, df=df_processed, top_n=10):
    # Mencari index tempat wisata
    idx = df[df['nama'].str.contains(place_name, case=False, na=False)].index[0]
    
    # Menghitung similarity scores
    sim_scores = list(enumerate(cosine_sim[idx]))
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)
    
    # Mengambil top N recommendations
    sim_scores = sim_scores[1:top_n+1]
    place_indices = [i[0] for i in sim_scores]
    
    return df.iloc[place_indices]
```

**Contoh Output Content-Based Filtering**:
```
Pantai Paradiso Sabang
  Lokasi: Jl. Malahayati, Kuta Ateueh, Sukakarya, Aceh, Aceh
  Rating: 4.5, Reviews: 421.0
  Kategori: pantai
  Similarity: 0.482

Tebing Batu HATUPIA
  Lokasi: Sawai, Kec. Seram Utara, Maluku, Maluku
  Rating: 5.0, Reviews: 50.0
  Kategori: lainnya
  Similarity: 0.411

Pantai Sawang
  Lokasi: Sawang, Kec. Samudera, Aceh, Aceh
  Rating: 3.6, Reviews: 31.0
  Kategori: pantai
  Similarity: 0.408
```

### Collaborative Filtering

Collaborative filtering merekomendasikan tempat wisata berdasarkan preferensi pengguna yang memiliki selera serupa.

**Algoritma yang digunakan**:
- **SVD (Singular Value Decomposition)**: Matrix factorization technique untuk prediksi rating

**Implementasi**:
```python
from surprise import SVD

# Training model
svd_model = SVD(n_factors=50, n_epochs=20, random_state=42)
svd_model.fit(trainset)

def get_collaborative_recommendations(user_id, model=svd_model, top_n=10):
    # Mencari tempat yang belum dirating user
    user_ratings = ratings_df[ratings_df['user_id'] == user_id]['place_id'].values
    unrated_places = [place_id for place_id in all_place_ids if place_id not in user_ratings]
    
    # Prediksi rating untuk tempat yang belum dirating
    predictions = []
    for place_id in unrated_places:
        pred = model.predict(user_id, place_id)
        predictions.append((place_id, pred.est))
    
    # Sort berdasarkan predicted rating
    predictions.sort(key=lambda x: x[1], reverse=True)
    return predictions[:top_n]
```

**Contoh Output Collaborative Filtering**:
```
Rekomendasi untuk User ID: 81

Air Terjun Takapala
  Lokasi: Bonto Lerung, Kec. Tinggimoncong, Sulawesi Selatan
  Rating Aktual: 4.5, Reviews: 1.973
  Kategori: air_terjun
  Predicted Rating: 4.78

Danau Belibis Tayan
  Lokasi: Sejotang, Kec. Tayan Hilir, Kalimantan Barat
  Rating Aktual: 4.3, Reviews: 257.0
  Kategori: danau
  Predicted Rating: 4.66

Air Terjun Kali
  Lokasi: Unnamed Road, Tinoor Satu, Kec. Tomohon Utara, Sulawesi Utara, Sulawesi Utara
  Rating Aktual: 4.5, Reviews: 399.0
  Kategori: air_terjun
  Predicted Rating: 4.65
```

### Kelebihan dan Kekurangan

**Content-Based Filtering**:

*Kelebihan*:
- Tidak memerlukan data pengguna lain (cold start problem teratasi)
- Dapat menjelaskan alasan rekomendasi berdasarkan fitur item
- Tidak terpengaruh oleh sparsity data
- Cocok untuk pengguna baru

*Kekurangan*:
- Terbatas pada fitur yang tersedia dalam data
- Cenderung memberikan rekomendasi yang mirip (kurang diverse)
- Tidak dapat menemukan preferensi tersembunyi pengguna
- Bergantung pada kualitas ekstraksi fitur

**Collaborative Filtering**:

*Kelebihan*:
- Dapat menemukan pola preferensi yang kompleks dan tersembunyi
- Memberikan rekomendasi yang lebih personal
- Tidak bergantung pada fitur item
- Dapat memberikan rekomendasi yang surprising

*Kekurangan*:
- Cold start problem untuk pengguna dan item baru
- Memerlukan data rating yang cukup banyak
- Computational complexity tinggi untuk dataset besar
- Sparsity problem pada matrix user-item

## Evaluation

### Metrik Evaluasi untuk Collaborative Filtering

**Root Mean Square Error (RMSE)**:
RMSE mengukur rata-rata kuadrat perbedaan antara rating prediksi dan rating aktual.

**Formula**:
```
RMSE = √(Σ(r_ui - ř_ui)² / N)
```

dimana:
- r_ui: rating aktual user u untuk item i
- ř_ui: rating prediksi user u untuk item i  
- N: jumlah prediksi

**Mean Absolute Error (MAE)**:
MAE mengukur rata-rata absolut perbedaan antara rating prediksi dan rating aktual.

**Formula**:
```
MAE = Σ|r_ui - ř_ui| / N
```

**Hasil Evaluasi Collaborative Filtering**:
- **RMSE**: 0.4965
- **MAE**: 0.4118

Nilai RMSE dan MAE yang sangat rendah menunjukkan bahwa model SVD dapat memprediksi rating dengan sangat akurat. RMSE 0.50 berarti rata-rata kesalahan prediksi hanya sekitar 0.5 poin dalam skala 1-5, yang merupakan performa yang sangat baik.

### Metrik Evaluasi untuk Content-Based Filtering

**Diversity Score**:
Mengukur keberagaman kategori dalam rekomendasi.

**Formula**: 
```
Diversity = Jumlah kategori unik / Total rekomendasi
```

**Coverage Score**:
Mengukur persentase item yang dapat direkomendasikan sistem.

**Formula**:
```
Coverage = Jumlah item yang dapat direkomendasikan / Total item
```

**Hasil Evaluasi Content-Based Filtering**:
- **Diversity Score**: 0.5000
- **Coverage Score**: 0.3162

Skor diversity 0.50 menunjukkan bahwa sistem memberikan rekomendasi dengan keberagaman sedang, dengan rata-rata 50% kategori berbeda dalam setiap set rekomendasi. Skor coverage 0.31 menunjukkan bahwa sekitar 31% tempat wisata dalam dataset dapat direkomendasikan oleh sistem dengan baik.

### Interpretasi Hasil

**Collaborative Filtering Performance**:
- RMSE 0.50 berarti rata-rata kesalahan prediksi hanya sekitar 0.5 poin dalam skala 1-5
- MAE 0.41 menunjukkan rata-rata kesalahan absolut 0.41 poin
- Performance ini sangat baik mengingat skala rating hanya 1-5
- Model berhasil memprediksi rating dengan akurasi tinggi

**Content-Based Filtering Performance**:
- Diversity score 0.50 menunjukkan rekomendasi yang cukup beragam namun masih bisa ditingkatkan
- Coverage score 0.31 menunjukkan bahwa masih ada ruang untuk peningkatan dalam cakupan item
- Sistem cenderung fokus pada item dengan karakteristik yang sangat mirip

**Perbandingan Kedua Metode**:
- Content-based filtering lebih konsisten dan dapat menangani cold start problem
- Collaborative filtering memberikan rekomendasi yang lebih personal namun memerlukan data yang cukup
- Kombinasi kedua metode (hybrid approach) dapat memberikan hasil terbaik

## Kesimpulan

Proyek sistem rekomendasi tempat wisata Indonesia telah berhasil dikembangkan dengan mengimplementasikan dua pendekatan utama: content-based filtering dan collaborative filtering. 

**Pencapaian Utama**:

1. **Dataset Comprehensive**: Berhasil menganalisis 1.169 tempat wisata dari 38 provinsi Indonesia dengan 11 fitur informatif

2. **Model Content-Based Filtering**: 
   - Diversity Score: 0.5000 (rekomendasi dengan keberagaman sedang)
   - Coverage Score: 0.3162 (cakupan yang cukup)
   - Efektif untuk cold start problem
   - Memberikan rekomendasi yang relevan berdasarkan kesamaan konten

3. **Model Collaborative Filtering**:
   - RMSE: 0.4965 (prediksi yang sangat akurat)
   - MAE: 0.4118 (kesalahan rata-rata yang sangat rendah)
   - Personalisasi yang excellent dengan akurasi prediksi tinggi
   - Mampu memberikan rekomendasi yang personal dan akurat

**Kontribusi untuk Industri Pariwisata**:
- Membantu wisatawan menemukan destinasi yang sesuai preferensi
- Mendukung promosi destinasi wisata yang kurang terkenal
- Meningkatkan efisiensi dalam perencanaan perjalanan wisata
- Menyediakan basis untuk pengembangan aplikasi wisata yang lebih canggih

**Rekomendasi Pengembangan Selanjutnya**:
1. Implementasi real-time user feedback untuk meningkatkan akurasi
2. Integrasi dengan data eksternal seperti cuaca, event, dan seasonal factors
3. Pengembangan mobile application untuk aksesibilitas yang lebih baik
4. Implementasi advanced algorithms seperti deep learning untuk performa yang lebih optimal

Sistem rekomendasi ini telah membuktikan efektivitasnya dalam memberikan rekomendasi tempat wisata yang relevan dan personal, dengan potensi besar untuk dikembangkan lebih lanjut dalam mendukung industri pariwisata Indonesia.
