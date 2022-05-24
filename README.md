# Capstone Modul 3
**Machine Learning Project**

**Database: Ecommerce Customer Churn**

<br />

Business Problem Understanding
------------------------------
**Context :**

Dataset yang dipakai dimiliki oleh sebuah perusahaan E-commerce terdepan. Perusahaan tersebut ingin memprediksi apakah seorang customer akan churn (berhenti menggunakan layanan yang diberikan), sehingga mereka dapat mengantisipasi hal tersebut dengan memberikan promo yang sesuai kepada customer yang ada.

**Problem Statement :**

Perusahaan E-commerce tentunya sangat bergantung pada customer yang melakukan transaksi atau menggunakan layanan yang ditawarkan. Seringkali perusahaan hanya berfokus pada mendapat customer baru, padahal berdasarkan penelitian didapati bahwa returning customer dapat membawa profit lebih banyak, dan biaya yang dibutuhkan untuk mendapat customer baru jauh lebih besar dibandingkan dengan biaya mempertahankan customer.

**Goals :**

Melihat permasalahan tersebut, perusahaan ingin memiliki kemampuan untuk memprediksi kemungkinan seorang customer akan churn atau tidak, sehingga perusahaan dapat mengantisipasi nya dengan memberikan insentif seperti promo, cashback, dan lainnya.

**Analytic Approach :**

Yang akan kita lakukan adalah menganalisis data untuk menemukan pola yang membedakan customer yang akan churn atau tidak, dimana kemudian kita akan membangun model **klasifikasi** yang akan membantu perusahaan untuk dapat memprediksi probabilitas seorang customer akan churn atau tidak.

**Metric Evaluation :**

Berdasarkan konsekuensinya, maka kita akan membuat model yang dapat meminimalisir 2 hal:
1. Jumlah promo yang diberikan kepada customer *(False Positive)* sehingga cost yang keluar lebih efisien.
2. Jumlah customer yang dianggap akan bertahan tetapi justru keluar *(False Negative)*, karena konsekuensi ini akan merugikan perusahaan.

Oleh karena itu, kita akan menggunakan f1-score sebagai measurement.

<br />

Data Understanding
------------------
Dataset source: https://www.kaggle.com/datasets/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction

Note : 
- Dataset tidak seimbang
- Sebagian besar fitur bersifat numerikal

### Attribute Information

| Attribute | Data Type, Length | Description |
| --- | --- | --- |
| Tenure | Float | Tenure of customer in organization [months] |
| WarehouseToHome | Float | Distance in between warehouse to home of customer |
| NumberOfDeviceRegistered | Int | Total number of deceives is registered on particular customer |
| PreferedOrderCat | Object | Preferred order category of customer in last month |
| SatisfactionScore | Int | Satisfactory score of customer on service |
| MaritalStatus | Object | Marital status of customer |
| NumberOfAddress | Int | Total number of address added on particular customer |
| Complain | Int | Any complaint has been raised in last month |
| DaySinceLastOrder | Float | Day Since last order by customer [days] |
| CashbackAmount | Float | Average cashback in last month |
| Churn | Int | 0 – Customer not churn, 1 – Customer churn |

[input Imbalance]

<br />

Data Analysis
-------------
[input Hist]
[input Count]
[input Heatmap]

**Analysis :**

- Data imbalance, dimana jumlah customer yang tidak `Churn` sebanyak 83% dan yang `Churn` 17%
- Berdasarkan feature `Tenure`, potensi customer churn akan berkurang dengan bertambah lamanya customer stay. Potensi churn tertinggi berada di 2 bulan pertama (artinya customer baru)
- Berdasarkan feature `WarehouseToHome`, potensi customer churn akan berkurang dengan bertambah dekatnya jarak rumah ke warehouse, namun tidak signifikan.
- Berdasarkan feature `DaySinceLastOrder`, potensi customer churn akan berkurang dengan bertambah lamanya hari terakhir mereka berbelanja.
- Berdasarkan feature `CashbackAmount`, potensi customer churn akan berkurang dengan bertambah tinggi nya cashback yang diterima customer.
- Berdasarkan feature `PreferedOrderCat`, customer yang berbelanja kategori barang yang relatif lebih mahal memiliki potensi churn lebih tinggi.
- Berdasarkan feature `SatisfactionScore`, tingkat satisfaction tinggi tidak menjamin customer untuk tidak churn
- Berdasarkan feature `MaritalStatus`, customer yang single memiliki potensi churn tertinggi
- Berdasarkan feature `Complain`, customer yang melakukan complain memiliki potensi churn lebih tinggi
- Berdasarkan matrix korelasi, `Tenure` dan `CashbackAmount` memiliki korelasi tertinggi dengan nilai 0.46 (artinya semakin lama customer stay, semakin tinggi jumlah cashback yang diterima)
- Features yang tidak disebutkan berarti tidak memiliki pengaruh yang jelas terhadap potensi `Churn`

*Note: Untuk kelengkapan tabel bisa dilihat di folder Images*

<br />

Modelling & Evaluation
----------------------
Setelah melakukan cross validation, model yang terbaik digunakan adalah XGBoost.

**Feature Importances :**
[input impotances]

Terlihat bahwa ternyata untuk model XGBoost kita, fitur/kolom `Tenure` adalah yang paling penting, kemudian diikuti dengan `Complain`, `PreferedOrderCat: Laptop & Accessory`, dan selanjutnya.

<br />

Conclusion & Recommendation
---------------------------
[input classification]
[input confusion matrix]

**Conclusion :**

Hal-hal yang dapat dikonklusikan berdasarkan hasil classification report:
- Berdasarkan `Recall`, terdapat 98% customer yang dibatasi promonya dan memang tidak akan churn, dan terdapat 82% customer yang diberika promo dan memang akan churn.
- Berdasarkan `Precision`, kita dapat memprediksi 96% customer yang tidak churn dengan tepat, dan memprediksi 92% customer yang churn dengan tepat.

**Recommendation :**

Hal-hal yang bisa dilakukan untuk mempertahankan customer:
- Tenure seorang customer cukup berhubungan dengan cashback amount yang diterima customer, sehingga untuk customer yang lebih baru (khususnya 2 bulan pertama karena memiliki potensi churn tertinggi), dapat diberikan promo berupa cashback yang cukup besar. Tentunya promo bisa disesuaikan kedepannya sehingga tidak mengeluarkan cost yang berlebih.
- Bagi customer yang memiliki jarak cukup jauh dari warehouse dapat lebih sering diberikan promo berupa potongan ongkos kirim sehingga mengurangi kemungkinan customer untuk churn.
- Complain merupakan salah satu feature yang cukup berpengaruh terhadap churn, untuk itu sistem dan pelayanan customer service perlu diperhatikan dan dikembangkan.
- Customer yang lebih sering berbelanja barang di kategori yang relatif lebih mahal dapat diberikan promo yang lebih baik juga sehingga mengurangi potensi customer untuk churn.
- Tingkat satisfaction customer tidak menjamin customer untuk tidak churn, sehingga tetap perlu memperhatikan pola belanja dari customer tersebut.

Hal-hal yang bisa dilakukan untuk mengembangkan project dan modelnya lebih baik lagi :
- Menghandle missing values dengan pengecekan lebih detil sehingga dapat diisi dengan lebih tepat.
- Mengeksplorasi lebih banyak hubungan antar feature numerikalnya, bukan hanya satu feature terhadap feature churn.
- Menambah beberapa data/feature lainnya, seperti metode pembayaran yang lebih sering dipakai, lama nya customer membuka aplikasi, atau kuantitas dalam sekali transaksi, sehingga performa model dapat lebih ditingkatkan, dan lainnya.
- Mencoba algoritma ML dan model lainnya, ataupun handling imbalance dengan metode lain, dan kemudian di hyperparameter tuning dengan lebih baik.

<br />

Additional Links
----------------
Link Video Penjelasan & Screenshots Visualisasi: https://drive.google.com/drive/folders/1vsK4Hns_iEUk88Aq1jnYmbD8sHaxUHiq?usp=sharing
