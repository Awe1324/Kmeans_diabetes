##  Deskripsi

Proyek ini menerapkan algoritma **K-Means Clustering** pada dataset `diabetes.csv`. Tujuannya adalah untuk **mengelompokkan pasien** berdasarkan fitur medis seperti:

- Glucose (Kadar gula darah)
- Insulin
- BMI (Indeks Massa Tubuh)
- Age (Usia)

Tanpa melihat apakah pasien memiliki diabetes atau tidak, kita mencoba menemukan **pola tersembunyi** dengan clustering.


## Teknologi yang Digunakan

- Python 3.x
- Pandas
- Scikit-learn
- Matplotlib
- Seaborn
- Jupyter Notebook

## CELL 1
1.import pandas as pd
    Untuk membaca dan mengelola data dari file .csv dalam bentuk tabel.

2.from sklearn.preprocessing import StandardScaler
    Untuk menormalkan data (agar fitur seperti usia dan gula darah berada pada skala yang sama).

3.from sklearn.cluster import KMeans
    Menggunakan algoritma K-Means untuk mengelompokkan data ke dalam beberapa cluster.

4.import matplotlib.pyplot as plt
    Untuk membuat grafik dan visualisasi (seperti scatter plot).

5.import seaborn as sns
    Library visualisasi tambahan agar grafik lebih menarik dan berwarna.

## CELL 2
fitur = ['Glucose', 'BMI', 'Age', 'Insulin']
memilih kolom (fitur) penting dari data pasien yang akan dipakai untuk clustering:

Glucose : kadar gula darah
BMI     : indeks massa tubuh
Age     : usia pasien
Insulin : kadar insulin

data = df[fitur].copy()
Membuat salinan data baru yang hanya berisi kolom-kolom tersebut — agar lebih fokus dan tidak mengubah df asli.

## CELL 3
data.replace(0, pd.NA, inplace=True)
Mengganti semua nilai 0 di dalam data dengan NA (kosong/null). Karena dalam data medis, nilai 0 pada kolom seperti Insulin atau BMI bisa berarti data tidak tersedia.

data.fillna(data.mean(), inplace=True)
Setelah ada nilai kosong, baris ini akan mengisi kekosongan dengan rata-rata kolom masing-masing.
Ini disebut imputasi rata-rata — teknik umum untuk mengatasi data kosong.

## CELL 4
scaler = StandardScaler()
Membuat objek StandardScaler dari library scikit-learn.
Ini digunakan untuk menstandarkan data, yaitu:

Mengubah setiap kolom agar memiliki rata-rata = 0 dan standar deviasi = 1(ukuran seberapa jauh data menyebar dari nilai rata-rata)

data_scaled = scaler.fit_transform(data)
Menyesuaikan (fit) dan menerapkan transformasi (transform) pada data.
Hasilnya disimpan dalam data_scaled, bentuknya adalah array numerik yang siap digunakan untuk clustering.

Karena fitur seperti Glucose, Age, dan Insulin punya skala yang berbeda-beda.
Kalau tidak dinormalisasi, fitur yang angkanya besar bisa mendominasi hasil clustering

## CELL 5
kmeans = KMeans(n_clusters=3, random_state=0)
n_clusters=3 → artinya data akan dikelompokkan menjadi 3 kelompok.
random_state=0 → untuk hasil yang konsisten setiap kali dijalankan (supaya tidak berubah-ubah).
df['Cluster'] = kmeans.fit_predict(data_scaled)
Model dilatih pada data (fit_predict)

Setiap baris (pasien) akan diberi label Cluster (misalnya: 0, 1, atau 2)
Hasilnya disimpan di kolom baru bernama Cluster dalam df

## CELL 6
df[['Glucose', 'BMI', 'Age', 'Insulin', 'Cluster']]
Menampilkan hanya kolom-kolom penting:

Fitur medis: Glucose, BMI, Age, Insulin
Hasil clustering: Cluster (hasil dari K-Means)

.head()
Menampilkan 5 baris pertama dari data

## CELL 8
plt.figure(figsize=(8, 6))
Mengatur ukuran gambar agar tidak terlalu kecil.

sns.scatterplot(...)
Membuat scatter plot (diagram sebar):

sumbu X → nilai Glucose (gula darah)

sumbu Y → nilai BMI (indeks massa tubuh)

hue='Cluster' → memberi warna berbeda untuk tiap cluster

palette='Set2' → memilih kombinasi warna yang enak dilihat

s=100 → ukuran titik diperbesar agar jelas

plt.title(), xlabel(), ylabel()
Memberi judul dan label sumbu agar grafik mudah dipahami.

plt.grid(True)
Menampilkan garis bantu (grid) agar titik-titik lebih mudah dibaca.

Untuk melihat bagaimana pasien dikelompokkan (clustering) berdasarkan dua fitur utama: Glucose dan BMI.

seperti:

Cluster 0 → pasien dengan gula & BMI sedang
Cluster 1 → pasien dengan BMI tinggi
Cluster 2 → pasien dengan gula tinggi, dll.

Hasilnya akan menunjukkan bagaimana pasien dikelompokkan berdasarkan
kondisi medisnya.

## CELL 9

sns.countplot(...)
Membuat grafik batang (bar chart) yang menampilkan jumlah data (pasien) dalam tiap cluster.

x='Cluster' → sumbu X menampilkan label cluster (0, 1, 2)

palette='Set2' → warna batang dibuat menarik

plt.title(), xlabel(), ylabel()
Menambahkan judul dan label sumbu agar grafik mudah dibaca.

plt.grid(True, axis='y')
Menambahkan garis bantu di sumbu Y agar jumlah lebih jelas terlihat.

plt.show()
Menampilkan grafik

Untuk melihat berapa banyak pasien yang masuk ke masing-masing cluster.

## CELL 10

sns.boxplot(...)
Membuat boxplot untuk melihat sebaran nilai Glucose di setiap cluster.

x='Cluster' → menunjukkan kelompok hasil clustering (0, 1, 2)

y='Glucose' → nilai gula darah pasien

palette='Set2' → warna tiap cluster agar menarik

plt.title(), xlabel(), ylabel()
Menambahkan judul dan label untuk grafik.


Untuk melihat karakteristik gula darah pasien di setiap cluster, termasuk:
Nilai rata-rata (median → garis tengah)
Rentang nilai (dari bawah ke atas)
Apakah ada outlier (titik ekstrem)


## CELL 11

df.groupby('Cluster')[...].mean()
Mengelompokkan data berdasarkan cluster, lalu menghitung rata-rata dari setiap fitur (Glucose, BMI, Age, Insulin) dalam tiap cluster.
Hasilnya adalah tabel rata-rata per fitur untuk masing-masing cluster.

sns.heatmap(...)
Membuat heatmap (peta warna) untuk memvisualisasikan angka-angka tersebut:

annot=True = menampilkan angkanya langsung di kotak
cmap='YlGnBu' = menggunakan gradasi warna dari kuning ke biru

## CELL 12

