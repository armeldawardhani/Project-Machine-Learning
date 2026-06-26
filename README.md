# PROJECT-Machine-Learning
Prediksi Life Expectancy Menggunakan Pendekatan Machine Learning (Regresi)
# Deskripsi Project
Dataset yang digunakan dalam penelitian ini adalah Life Expectancy Data yang dikompilasi oleh WHO (World Health Organization) dan United Nations. Dataset ini berisi data statistik kesehatan, ekonomi, pendidikan, dan sosial dari 193 negara yang tercatat selama periode 16 tahun, yaitu dari tahun 2000 hingga 2015.
Dataset ini dipilih karena kelengkapan variabelnya yang mencakup berbagai aspek kehidupan yang secara teoritis berpengaruh terhadap angka harapan hidup. Selain itu, dataset ini telah banyak digunakan dalam penelitian bidang kesehatan dan machine learning, sehingga validitas dan relevansinya sudah teruji. Dataset memenuhi seluruh persyaratan minimal yang ditetapkan dalam tugas project ini, yaitu memiliki lebih dari 1000 observasi, lebih dari 5 kolom fitur, memuat campuran tipe data numerik dan kategorik, serta memiliki satu kolom target yang jelas.
# Anggota Tim
1. Ardika Wahyu Fariz  (K1D024013)
2. Meylina Nur Sasmita  (K1D024017)
3. Fatih Maulana Luthf Rabbaanii  (K1D024022)
4. Eunike Prasetyaning Yuniawan  (K1D024026)
5. Armelda Shafa Wardhani  (K1D024029)
# Sumber Dataset
 Life Expectancy (WHO) : https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who
Dataset yang digunakan dalam penelitian ini adalah Life Expectancy Data yang dikompilasi oleh WHO (World Health Organization) dan United Nations. Dataset ini berisi data statistik kesehatan, ekonomi, pendidikan, dan sosial dari 193 negara yang tercatat selama periode 16 tahun, yaitu dari tahun 2000 hingga 2015.
Dataset ini dipilih karena kelengkapan variabelnya yang mencakup berbagai aspek kehidupan yang secara teoritis berpengaruh terhadap angka harapan hidup. Selain itu, dataset ini telah banyak digunakan dalam penelitian bidang kesehatan dan machine learning, sehingga validitas dan relevansinya sudah teruji. Dataset memenuhi seluruh persyaratan minimal yang ditetapkan dalam tugas project ini, yaitu memiliki lebih dari 1000 observasi, lebih dari 5 kolom fitur, memuat campuran tipe data numerik dan kategorik, serta memiliki satu kolom target yang jelas.

# Cara Menjalankan
Google Colab (Direkomendasikan)
1. Buka Google Colab
2. Upload file Prediksi_Life_Expectancy.ipynb
3. Upload dataset Life Expectancy Data (1).csv ke Google Drive atau langsung ke sesi Colab
4. Sesuaikan path file di cell pemanggilan data:
file_path = '/content/Life Expectancy Data (1).csv'
5. Jalankan semua cell secara berurutan: Runtime → Run all
   
Jalankan cell-cell berikut secara berurutan dari atas ke bawah di colab:
1. Perumusan Tujuan & Persiapan Data
Import semua library yang dibutuhkan
Load dataset dari path yang sesuai
Drop kolom Country dan Year (tidak relevan untuk prediksi)
2. Penelaahan Data (EDA)
Cek tipe data dengan df.info()
Statistik deskriptif dengan df.describe()
Visualisasi distribusi: bar chart, histogram, box plot, dan scatter plot
Heatmap matriks korelasi antar variabel
3. Validasi Data
Pemeriksaan data duplikat
Identifikasi missing values per kolom
Pengecekan nilai negatif
4. Pembersihan Data
Imputasi missing values menggunakan nilai median untuk semua kolom numerik
Konfirmasi ulang tidak ada duplikat setelah pembersihan
Deteksi outlier menggunakan boxplot dan metode IQR
5. Penentuan Objek Data
Identifikasi ulang tipe data setiap fitur
Label Encoding pada kolom Status (Developing → 0, Developed → 1)
Perhitungan korelasi linear antar variabel
6. Konstruksi Data & Feature Engineering
Menghapus kolom yang berindikasi multikolinearitas:
percentage expenditure, thinness 5-9 years, infant deaths
Analisis Feature Importance dengan XGBoost
Analisis SHAP values untuk interpretabilitas model
Drop kolom HIV/AIDS (terlalu dominan, menutupi fitur lain)
Re-analisis Feature Importance dan SHAP tanpa HIV/AIDS
7. Standarisasi Data
Standardisasi fitur numerik menggunakan StandardScaler
Variabel target Life expectancy tidak distandardisasi
8. Penentuan Label & Pemodelan
Definisikan X (14 fitur) dan y (Life expectancy)
Split data: 80% training / 20% testing (random_state=42)
Latih dan evaluasi 4 model:
ModelMAERMSER²Linear Regression3.18664.50210.7661Decision Tree Regressor1.67843.03890.8934Random Forest Regressor1.14431.81080.9622XGBoost Regressor1.19591.73650.9652
9. Evaluasi & Hyperparameter Tuning
Bandingkan kinerja semua model berdasarkan metrik R²
Lakukan GridSearchCV pada XGBoost dengan parameter grid:
n_estimators: [100, 200, 300]
max_depth: [3, 5, 7]
learning_rate: [0.01, 0.1, 0.2]
subsample: [0.7, 0.8, 1.0]
colsample_bytree: [0.7, 0.8, 1.0]
Model final terbaik: XGBoost Tuned dengan R² = 0.9692
# Ringkasan Hasil
Berdasarkan analisis yang telah dilakukan terhadap dataset Life Expectancy yang bersumber dari WHO dan United Nations dengan total 2.938 observasi serta 20 variabel, berikut adalah ringkasan hasil analisis.
1. Berdasarkan analisis Feature Importance dari model XGBoost dan SHAP, ditemukan bahwa faktor yang paling dominan mempengaruhi angka harapan hidup adalah Income Composition of Resources dengan kontribusi sebesar 64,66% yang berarti indeks komposisi pendapatan dan sumber daya suatu negara merupakan indikator terpenting dalam menentukan umur panjang penduduk. Semakin tinggi indeks ini, maka semakin tinggi pula angka harapan hidup yang diprediksi oleh model. Faktor kedua yang paling berpengaruh adalah Adult Mortality atau tingkat kematian dewasa dengan kontribusi sebesar 21,34%. Berdasarkan analisis SHAP, fitur ini menunjukkan pengaruh negatif yang sangat kuat, di mana nilai Adult Mortality yang tinggi cenderung menurunkan prediksi angka harapan hidup secara signifikan karena ketika banyak penduduk meninggal pada usia produktif, maka rata-rata harapan hidup secara keseluruhan akan menurun drastis. Faktor ketiga adalah Under-five Deaths atau angka kematian balita dengan kontribusi sebesar 4,31% yang juga menunjukkan pengaruh negatif, mencerminkan bahwa kegagalan dalam melindungi anak-anak dari kematian dini berdampak buruk pada statistik harapan hidup secara keseluruhan. Selain ketiga faktor utama tersebut, terdapat faktor lain seperti Diphtheria dan Thinness 1-19 Years masing-masing sebesar 1,85%, serta Schooling sebesar 1,34% yang menunjukkan pengaruh positif, di mana pendidikan berperan dalam meningkatkan kesadaran akan kesehatan dan kemampuan ekonomi yang berkontribusi pada umur yang lebih panjang.
2. Model regresi yang dihasilkan memiliki tingkat akurasi yang sangat baik dalam memprediksi angka harapan hidup. Model terbaik yaitu XGBoost Regressor yang telah melalui hyperparameter tuning mampu menjelaskan sekitar 96,92% variasi data Life Expectancy yang ditunjukkan oleh nilai R-squared sebesar 0,9692, yang berarti hanya sekitar 3,08% variasi dalam data yang tidak dapat dijelaskan oleh model. Dari sisi akurasi prediksi, model ini menghasilkan RMSE sebesar 1,6342 yang berarti rata-rata kesalahan prediksi model adalah sekitar 1,63 tahun dari nilai aktual angka harapan hidup. MAE yang dihasilkan sebesar 1,0905 berarti secara rata-rata absolut prediksi model hanya meleset sekitar 1,09 tahun dari nilai sebenarnya. Dengan seluruh metrik evaluasi yang menunjukkan hasil sangat baik, dapat disimpulkan bahwa model regresi yang dibangun memiliki kemampuan prediksi yang sangat tinggi dan dapat diandalkan untuk memprediksi angka harapan hidup secara akurat berdasarkan variabel-variabel yang tersedia dalam dataset.
3. Faktor ekonomi (Income Composition of Resources) dan faktor kesehatan (Adult Mortality) merupakan dua faktor paling dominan yang mempengaruhi angka harapan hidup. Model XGBoost Regressor yang telah di-tuning terbukti memiliki kemampuan prediksi sangat baik dengan akurasi 96,92% dan rata-rata kesalahan prediksi hanya sekitar 1,63 tahun. Di antara keempat algoritma yang diuji, XGBoost Regressor menunjukkan performa terbaik, terutama setelah melalui optimasi hyperparameter. Hasil ini memberikan wawasan bagi pembuat kebijakan dalam merancang intervensi efektif untuk meningkatkan angka harapan hidup melalui peningkatan kualitas ekonomi, penurunan angka kematian dewasa dan balita, serta peningkatan akses terhadap pendidikan dan layanan kesehatan dasar.
