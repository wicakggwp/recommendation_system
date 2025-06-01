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

- Mengembangkan sistem yang dapat memberikan rekomendasi relevan sesuai deskripsi dari suatu produk.

- Membangun sistem yang dapat memberikan rekomendasi produk dengan memanfaatkan data rating dan preferensi pengguna untuk memberikan rekomendasi yang sesuai dengan keinginan individual.

### Solution Statements

1. Content-Based Filtering: 
   - Melihat dan menganalisis kesamaan karateristik daribebrapa fitur yang terkait, seperti nama produk, deskripsi produk, dan rating.
   - Memberikan rekomendasi produk yang memiliki karakteristik serupa.

2. Collaborative Filtering:
   - Menganalisis pola dari rating dan preferensi pengguna.
   - Memberikan rekomendasi produk berdasarkan preferensi personal maupun dari riwayat rating pengguna.

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


## Data Preparation

Tahapanan ini dilakukan untuk mempersiapkan data agar siap digunakan dalam pengembangan model. Beberapa teknik yang diterapkan meliputi:

### Data Preprocessing

Melakukan pemilihan beberapa kolom atau fitur yang memiliki relevansi dengan kesesuaian proyek ini, karena terlalu banyak kolom dari dataset maka dipilih beberapa saja agar memudahkan dalam memproses dataset tersebut.
```python
df = df[['user_id','product_id', 'rating', 'rating_count', 'category',
 'product_name', 'about_product', 'review_id', 'review_content']]
```

### Data Cleaning

Tujuan dari tahap adalah untuk membersihkan dan mengekstrak kata-kata penting (tags) dari kolom kategori dalam DataFrame df, agar data menjadi lebih terstruktur dan siap digunakan dalam proses analisis atau pemodelan seperti:
   - Content-based filtering dalam sistem rekomendasi, dengan membandingkan kesamaan antar produk berdasarkan tag.
   - Analisis teks seperti TF-IDF atau word embedding.
   - Penanganan missing values ini penting untuk memastikan semua data dapat diproses dengan baik dalam algoritma machine learning.
   - Mengganti karakter pemisah | dengan koma , pada kolom category agar lebih konsisten dan mudah diproses sebagai daftar tag atau kata kunci.
   - Menyatukan informasi penting dalam satu kolom untuk digunakan dalam content-based filtering.
   - Mengonversi nilai pada kolom rating ke format numerik (float/int), dan jika ada nilai yang tidak bisa dikonversi (seperti teks atau simbol), akan diubah menjadi NaN.

### Data Preparation Content-Based Filtering

**TF-IDF Vectorization**:

Tujuan utamanya adalah menyiapkan data teks agar bisa dianalisis oleh model machine learning atau digunakan dalam perhitungan kesamaan dokumen (seperti cosine similarity). Cosine similiarity bertujuan untuk mencari kesamaan dalam fitur.

Parameter yang dipilih:
   - max_features=1000: Membatasi fitur
   - ngram_range=(1, 2): Menggunakan unigram dan bigram untuk konteks yang lebih baik
   - min_df=1: Mempertahankan semua term yang muncul minimal sekali

### 3. Data Preparation Collaborative Filtering

Pada dataset asli tidak ada rating user, maka perlu untuk membuat synthetic user-item interactions

Synthetic data ini diperlukan karena:
   - Data asli tidak memuat informasi interaksi antara pengguna dan item.
   - Hal ini memungkinkan penerapan serta evaluasi metode collaborative filtering.

**Pembuatan DataFrame Rating**:
```python
ratings_df = pd.DataFrame({
    'user_id': user_ids,
    'product_id': item_ids,
    'rating': ratings
})
```

### Konversi dan Splitting Data

Penggunaan library surprise mengharuskan konversi data dari pandas ke surprise, agar dapat diproses oleh libarary surprise.

Langkah yang dilakukan pada tahap ini meliputi:
   - Mengkonversi DataFrame pandas ke format yang dapat diproses oleh library Surprise.
   - Menetapkan skala rating yang sesuai (1-5).
   - Mempersiapkan data untuk algoritma collaborative filtering.
   - Membagi dataset dengan rasio data train 80% dan data test 20%, pembagian ini berlaku untuk kedua model.
   
## Modeling

### Content-Based Filtering

Content Based Filtering bekerja dengan merekomendasikan item kepada pengguna berdasarkan kemiripan atribut (fitur) item dengan item yang disukai sebelumnya.
```python
def get_content_recommendations(place_name, cosine_sim=cosine_sim, df=df, top_n=10):
    try:
        idx = df[df['product_name'].str.lower() == place_name.lower()].index[0]
    except IndexError:
        print(f"Place '{place_name}' not found!")
        return None

    sim_scores = list(enumerate(cosine_sim[idx]))
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)
    sim_scores = sim_scores[1:top_n+1]

    place_indices = [i[0] for i in sim_scores]

    recommendations = df.iloc[place_indices][['product_name', 'category', 'rating', 'rating_count', 'about_product']].copy()
    recommendations['similarity_score'] = [score[1] for score in sim_scores]

    return recommendations
```
Berikut contoh output dari algorima tersebut
```
Wayona Nylon Braided USB to Lightning Fast Charging and Data Sync Cable Compatible for iPhone 13, 12,11, X, 8, 7, 6, 5, iPad Air, Pro, Mini (3 FT Pack of 1, Grey)
  Rating: 4.2, Rating Count: nan
  About: High Compatibility : Compatible With iPhone 12, 11, X/XsMax/Xr ,iPhone 8/8 Plus,iPhone 7/7 Plus,iPhone 6s/6s Plus,iPhone 6/6 Plus,iPhone 5/5s/5c/se,iPad Pro,iPad Air 1/2,iPad mini 1/2/3,iPod nano7,iPod touch and more apple devices.|Fast Charge&Data Sync : It can charge and sync simultaneously at a rapid speed, Compatible with any charging adaptor, multi-port charging station or power bank.|Durability : Durable nylon braided design with premium aluminum housing and toughened nylon fiber wound tightly around the cord lending it superior durability and adding a bit to its flexibility.|High Security Level : It is designed to fully protect your device from damaging excessive current.Copper core thick+Multilayer shielding, Anti-interference, Protective circuit equipment.|WARRANTY: 12 months warranty and friendly customer services, ensures the long-time enjoyment of your purchase. If you meet any question or problem, please don't hesitate to contact us.
  Similarity: 1.000

Wayona Nylon Braided USB to Lightning Fast Charging and Data Sync Cable Compatible for iPhone 13, 12,11, X, 8, 7, 6, 5, iPad Air, Pro, Mini (3 FT Pack of 1, Grey)
  Rating: 4.2, Rating Count: nan
  About: High Compatibility : Compatible With iPhone 12, 11, X/XsMax/Xr ,iPhone 8/8 Plus,iPhone 7/7 Plus,iPhone 6s/6s Plus,iPhone 6/6 Plus,iPhone 5/5s/5c/se,iPad Pro,iPad Air 1/2,iPad mini 1/2/3,iPod nano7,iPod touch and more apple devices.|Fast Charge&Data Sync : It can charge and sync simultaneously at a rapid speed, Compatible with any charging adaptor, multi-port charging station or power bank.|Durability : Durable nylon braided design with premium aluminum housing and toughened nylon fiber wound tightly around the cord lending it superior durability and adding a bit to its flexibility.|High Security Level : It is designed to fully protect your device from damaging excessive current.Copper core thick+Multilayer shielding, Anti-interference, Protective circuit equipment.|WARRANTY: 12 months warranty and friendly customer services, ensures the long-time enjoyment of your purchase. If you meet any question or problem, please don't hesitate to contact us.
  Similarity: 1.000
```

### Collaborative Filtering

Collaborative Filtering (CF) adalah metode sistem rekomendasi yang menyarankan item kepada pengguna berdasarkan pola interaksi atau preferensi pengguna lain. Model ini menggunakan algoritma SVD dengan matrix factorization untuk memprediksi rating.

```python
def get_collaborative_recommendations(user_id, model=svd_model, df=df, ratings_df=ratings_df, top_n=5):
    user_ratings = ratings_df[ratings_df['user_id'] == user_id]['product_id'].values

    all_item_ids = df['product_id'].unique()
    unrated_items = [item_id for item_id in all_item_ids if item_id not in user_ratings]

    predictions = []
    for item_id in unrated_items:
        try:
            pred = model.predict(user_id, item_id)
            predictions.append((item_id, pred.est))
        except ValueError:
             pass


    predictions.sort(key=lambda x: x[1], reverse=True)

    top_predictions = predictions[:top_n]

    rec_item_ids = [pred[0] for pred in top_predictions]
    rec_ratings = [pred[1] for pred in top_predictions]
    valid_rec_item_ids = [item_id for item_id in rec_item_ids if item_id in df['product_id'].values]

    recommendations = df[df['product_id'].isin(valid_rec_item_ids)][['product_name', 'category', 'rating', 'rating_count', 'about_product', 'product_id']].copy() # Include product_id for merging

    valid_predictions_df = pd.DataFrame({
        'product_id': [pred[0] for pred in top_predictions if pred[0] in valid_rec_item_ids],
        'predicted_rating': [pred[1] for pred in top_predictions if pred[0] in valid_rec_item_ids]
    })
    if recommendations.empty:
        return pd.DataFrame()
    if 'product_id' not in recommendations.columns:
         return pd.DataFrame()
    if 'product_id' not in valid_predictions_df.columns:
         return pd.DataFrame()

    recommendations = recommendations.merge(valid_predictions_df, on='product_id', how='left')

    if 'predicted_rating' in recommendations.columns:
        recommendations = recommendations.sort_values(by='predicted_rating', ascending=False).reset_index(drop=True)
        recommendations = recommendations.head(top_n)

    return recommendations
```

Berikut contoh output dari algorima tersebut
```
Mencari rekomendasi untuk User ID: 71

Riwayat rating user (sample 5):
                                        product_name category  rating
0  Amazonbasics Micro Usb Fast Charging Cable For...              4.0
1  Amazonbasics Micro Usb Fast Charging Cable For...              4.0
2  Fire-Boltt Ninja Call Pro Plus 1.83" Smart Wat...              3.9
3  Bajaj Majesty DX-11 1000W Dry Iron with Advanc...              4.1
4  OnePlus 126 cm (50 inches) Y Series 4K Ultra H...              4.3

Top 5 Rekomendasi (Collaborative Filtering):
----------------------------------------
1. Wayona Nylon Braided USB to Lightning Fast Charging and Data Sync Cable Compatible for iPhone 13, 12,11, X, 8, 7, 6, 5, iPad Air, Pro, Mini (3 FT Pack of 1, Grey)
   Category        : 
   Rating Aktual   : 4.2 (nan rating)
   About Product   : High Compatibility : Compatible With iPhone 12, 11, X/XsMax/Xr ,iPhone 8/8 Plus,iPhone 7/7 Plus,iPhone 6s/6s Plus,iPhone 6/6 Plus,iPhone 5/5s/5c/se,iPad Pro,iPad Air 1/2,iPad mini 1/2/3,iPod nano7,iPod touch and more apple devices.|Fast Charge&Data Sync : It can charge and sync simultaneously at a rapid speed, Compatible with any charging adaptor, multi-port charging station or power bank.|Durability : Durable nylon braided design with premium aluminum housing and toughened nylon fiber wound tightly around the cord lending it superior durability and adding a bit to its flexibility.|High Security Level : It is designed to fully protect your device from damaging excessive current.Copper core thick+Multilayer shielding, Anti-interference, Protective circuit equipment.|WARRANTY: 12 months warranty and friendly customer services, ensures the long-time enjoyment of your purchase. If you meet any question or problem, please don't hesitate to contact us.
   Predicted Rating: 5.00

2. Ambrane Unbreakable 60W / 3A Fast Charging 1.5m Braided Type C Cable for Smartphones, Tablets, Laptops & other Type C devices, PD Technology, 480Mbps Data Sync, Quick Charge 3.0 (RCT15A, Black)
   Category        : 
   Rating Aktual   : 4.0 (nan rating)
   About Product   : Compatible with all Type C enabled devices, be it an android smartphone (Mi, Samsung, Oppo, Vivo, Realme, OnePlus, etc), tablet, laptop (Macbook, Chromebook, etc)|Supports Quick Charging (2.0/3.0)|Unbreakable – Made of special braided outer with rugged interior bindings, it is ultra-durable cable that won’t be affected by daily rough usage|Ideal Length – It has ideal length of 1.5 meters which is neither too short like your typical 1meter cable or too long like a 2meters cable|Supports maximum 3A fast charging and 480 Mbps data transfer speed|6 months manufacturer warranty from the date of purchase
   Predicted Rating: 5.00
```

### Kelebihan dan Kekurangan

| Aspek                         | Content-Based Filtering | Collaborative Filtering |
| ----------------------------- | ----------------------- | ----------------------- |
| Ketergantungan pengguna       | Tidak                   | Ya                      |
| Ketergantungan deskripsi item | Ya                      | Tidak                   |
| Cold start user               | Lebih baik              | Bermasalah              |
| Cold start item               | Bermasalah              | Bermasalah              |
| Eksplorasi                    | Kurang (terbatas)       | Baik (lebih bervariasi) |
| Interpretasi hasil            | Mudah dijelaskan        | Sulit dijelaskan        |


## Evaluation

### Metrik Evaluasi Content Based Filtering
Metrik yang digunakan untuk evaluasi pada model ini meliputi:
- Diversity Score mengukur seberapa beragam item yang direkomendasikan kepada satu pengguna
- Coverage Score mengukur seberapa besar proporsi dari total item di sistem yang pernah direkomendasikan ke user (baik satu user atau semua user).

| Metrik              | Nilai | Analisa                                                                |
| ------------------- | ----- | ---------------------------------------------------------------------- |
| **Diversity Score** | 0.11  | Rekomendasi terlalu mirip, kurang variasi                              |
| **Coverage Score**  | 0.28  | Cakupan item masih terbatas, belum menyentuh banyak konten di database |


### Metrik Evaluasi Collaborative Filtering
Metrik yang digunakan untuk evaluasi pada model ini meliputi:
- Root Mean Square Error (RMSE) mengukur seberapa besar rata-rata kesalahan prediksi, tapi lebih sensitif terhadap kesalahan besar karena nilai error dikuadratkan dulu sebelum dirata-ratakan.
- Mean Absolute Error (MAE) adalah rata-rata kesalahan absolut antara rating asli dan prediksi. Berbeda RMSE, MAE tidak memberikan pinalti error besar secara ekstra.

| Metrik         | Nilai | Analisa                                                                                              |
| ------------------- | --------- | ------------------------------------------------------------------------------------------- |
| **RMSE**            | 1.11      | Prediksi rating masih meleset rata-rata sekitar 1 poin, cukup akurat tapi bisa ditingkatkan |
| **MAE**             | 0.96      | Rata-rata kesalahan prediksi mendekati 1 poin, cukup dekat dengan rating asli               |


**Perbandingan Kedua Metode**:

| Metrik                            | Nilai     | Analisa                                                             | Evaluasi                                      |
| --------------------------------- | --------- | ------------------------------------------------------------------- | --------------------------------------------- |
| **Content-Based Diversity**       | 0.11      | Mengukur keberagaman item dalam daftar rekomendasi untuk satu user. | **Rendah** → Item terlalu mirip.              |
| **Content-Based Coverage**        | 0.28      | Mengukur proporsi item di database yang pernah direkomendasikan.    | **Rendah** → Cakupan item sempit.             |
| **RMSE (Root Mean Square Error)** | 1.11      | Rata-rata deviasi kuadrat antara rating prediksi dan aktual.        | **Sedang** → Prediksi masih meleset \~1 poin. |
| **MAE (Mean Absolute Error)**     | 0.96      | Rata-rata kesalahan absolut antara rating prediksi dan aktual.      | **Sedang** → Cukup dekat ke nilai asli.       |

Berdasarkan hasil evaluasi di atas, sistem rekomendasi saat ini menunjukkan akurasi prediksi yang cukup baik (RMSE 1.11 dan MAE 0.96), namun masih memiliki keterbatasan dalam hal keberagaman dan cakupan rekomendasi (Diversity 0.11 dan Coverage 0.28). Artinya, meskipun sistem mampu memprediksi rating dengan cukup akurat, item yang direkomendasikan cenderung terlalu mirip dan hanya mencakup sebagian kecil dari seluruh database. Perbaikan disarankan untuk meningkatkan eksplorasi dan variasi dalam hasil rekomendasi agar lebih menarik dan bermanfaat bagi pengguna.


