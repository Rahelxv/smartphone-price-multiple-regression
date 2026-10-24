# Analisis Faktor-Faktor yang Mempengaruhi Harga Smartphone Menggunakan Regresi Linear Berganda

Analisis faktor penentu harga smartphone menggunakan **regresi linear berganda (Multiple Linear Regression)**, untuk mengidentifikasi variabel hardware dan brand yang berasosiasi signifikan dengan harga, mengukur besar pengaruhnya, serta menilai seberapa baik model menjelaskan variasi harga.

> **Status:** Selesai — 24 Oktober 2026
> **Analis:** Rahel ([@Rahelxv](https://github.com/Rahelxv))

---

## 1. Latar Belakang

Smartphone telah menjadi pusat aktivitas ekonomi, sosial, dan hiburan. Sekitar 70,4% populasi dunia menggunakan ponsel, dengan 5,83 miliar pengguna unik pada April 2026 (DataReportal, 2026). Pasar didominasi Apple dan Samsung, dengan Xiaomi, OPPO, dan vivo sebagai pesaing utama di segmen menengah (Team Counterpoint, 2026).

Harga adalah faktor yang paling sering dikaji dalam keputusan pembelian ponsel, dan keterjangkauan perangkat menjadi hambatan terbesar adopsi internet seluler di negara berpenghasilan rendah dan menengah (GSMA, 2026). Karena itu, memahami faktor apa yang menentukan harga ponsel penting bagi konsumen, distributor, maupun produsen dalam mengambil keputusan membeli atau menetapkan harga.

## 2. Tugas Bisnis (Business Task)

> Mengidentifikasi faktor-faktor yang berasosiasi dengan harga smartphone serta mengukur besar hubungannya.

**Pertanyaan yang dijawab:**
- Faktor apa saja yang secara teori berpotensi mempengaruhi harga ponsel pintar?
- Faktor mana yang berasosiasi signifikan dengan harga, dan seberapa besar perubahan harganya?
- Seberapa baik faktor-faktor tersebut secara bersama-sama menjelaskan variasi harga?

**Indikator keberhasilan:**
1. Mengidentifikasi variabel yang berasosiasi signifikan dengan harga (p < 0,05) beserta besar perubahan harga tiap variabel
2. Menjelaskan besar variasi harga yang dijelaskan model (R² dan Adj. R²)
3. Memastikan hasil model dapat dipercaya melalui pemeriksaan asumsi regresi dan memahami hubungan umum yang terjadi

## 3. Metodologi

Proyek ini mengikuti kerangka **ASK → PREPARE → PROCESS → ANALYZE → SHARE**, dengan tahapan analisis berikut:

| Tahap Analisis | Deskripsi |
| --- | --- |
| **Data Cleaning** | Penghapusan duplikat berdasarkan Brand dan Model (407 → 239 baris), pembersihan tiap kolom, dan rekayasa fitur kamera dengan Regex |
| **Statistik Deskriptif** | Mean, median, standard deviation, skewness, kurtosis, dan IQR tiap variabel |
| **Data Transformation** | Log natural pada harga, pengelompokan 16 brand menjadi 8 brand utama + kategori referensi "Others" (dummy variable) |
| **Asumsi Pre-Fitting** | Linearitas, outlier, dan multikolinearitas (korelasi < \|0,8\| dan VIF < 5) |
| **Fitting Model** | Multiple Linear Regression (OLS) dengan standard error robust **HC3** |
| **Asumsi Post-Fitting** | Independensi residual (Durbin-Watson), homoskedastisitas (Breusch-Pagan), normalitas residual (Shapiro-Wilk, Jarque-Bera, Omnibus, Cook's Distance) |
| **Interpretasi** | Pengaruh hardware, pengaruh brand, dan kekuatan model secara keseluruhan |

## 4. Data

| Data | Sumber | Cakupan | Keterangan |
| --- | --- | --- | --- |
| Mobile Phone Price | [Kaggle](https://www.kaggle.com/datasets/rkiattisak/mobile-phone-price) | 407 baris × 8 kolom (mentah); 238 baris × 15 kolom (data model) | Brand, Model, Storage, RAM, Screen Size, Camera (MP), Battery Capacity, Price (USD) |

Harga dalam dolar AS dengan tingkat harga sekitar tahun 2022 (mengikuti dataset Kaggle), tidak disesuaikan inflasi dan tidak mencerminkan harga pasar saat ini.

## 5. Ringkasan Temuan Utama

- **Daya jelaskan model:** variabel independen bersama-sama menjelaskan **86,5%** variasi log(Price) (Adj. R² = 0,856). Uji F signifikan (p ≈ 4,26e-80).
- **Hardware:** RAM menjadi variabel hardware dengan asosiasi terkuat (+1 GB → harga naik ±13,8%; +1 SD ≈ +39,1%), diikuti Storage (+1 GB → ±0,2%; +1 SD ≈ +16,8%). Screen Size, Camera_Count, dan Total_Camera_MP tidak signifikan.
- **Battery Capacity:** signifikan tetapi berarah negatif (+1000 mAh → harga turun ±12,6%). Arah ini tidak sesuai logika ekonomi dan tidak boleh dibaca sebagai "baterai besar membuat ponsel lebih murah"; kemungkinan dipengaruhi hubungan dengan RAM dan Storage serta variabel yang tidak ada di data.
- **Brand:** dibandingkan kategori "Others" pada spesifikasi yang sama, **Apple ±83,4% lebih mahal**, sedangkan Vivo, Realme, Xiaomi, Oppo, Nokia, dan Motorola ±29%–45% lebih murah. Samsung tidak berbeda signifikan (p = 0,062).
- **Asumsi post-fitting:** independensi residual terpenuhi (Durbin-Watson = 2,022). Homoskedastisitas dan normalitas tidak terpenuhi secara ketat (Breusch-Pagan p < 0,001; kurtosis = 4,515), sehingga inferensi memakai **SE robust HC3** dan dampaknya dinilai terbatas pada n = 238.
- **Implikasi:** model lebih andal untuk **memahami faktor harga** daripada **memprediksi harga ponsel individual**, terutama untuk flagship, ponsel lipat, dan brand niche (mis. Galaxy S20 Ultra 5G, BlackBerry KEY2).

Detail lengkap perhitungan, visualisasi, dan interpretasi ada di notebook dan dokumen pada folder `reports/`.

## 6. Struktur Repository

```
.
├── code/      # Notebook implementasi lengkap regresi linear berganda
├── data/      # Dataset harga smartphone (Kaggle)
└── reports/   # Presentasi akhir & dokumentasi proses teknis
```

| Folder | Isi |
| --- | --- |
| `code/` | `Full_Length_implementasion_MLR_Last_Code.ipynb` (pembersihan data, transformasi, uji asumsi, fitting model, dan interpretasi) |
| `data/` | `Mobile-phone-price.csv` |
| `reports/` | *Laporan Akhir - Analisis Faktor-Faktor yang Mempengaruhi Harga Smartphone Menggunakan Regresi Linear Berganda* (presentasi), *Proses Teknis - Analisis Faktor-Faktor yang Mempengaruhi Harga Smartphone Menggunakan Regresi Linear Berganda* (catatan proses, definisi, dan penjelasan teknis) |

## 7. Tools & Tech Stack

| Kategori | Tools |
| --- | --- |
| Bahasa & Lingkungan | Python, Jupyter Notebook |
| Pengolahan Data | Pandas, NumPy |
| Pemodelan & Uji Statistik | Statsmodels, SciPy |
| Visualisasi | Matplotlib, Seaborn |
| Presentasi & Dokumentasi | Microsoft PowerPoint, Microsoft Word |
| Version Control | GitHub |

## 8. Batasan & Catatan Metodologis

- Hasil menunjukkan **asosiasi, bukan sebab-akibat**.
- Harga mengikuti dataset Kaggle (sekitar 2022), tidak disesuaikan inflasi, sehingga tidak mencerminkan harga pasar saat ini.
- Data berukuran 238 model setelah penghapusan duplikat, dan bukan mewakili seluruh pasar smartphone.
- Variabel penting seperti prosesor, kualitas sensor dan lensa kamera, material, sistem operasi, dan kelas perangkat tidak tersedia di data. Efek software (mis. iOS) tidak dapat dipisahkan dari efek brand karena Apple adalah satu-satunya brand iOS di data.
- Kategori referensi "Others" (OnePlus, Huawei, Google, Sony, LG, Asus, BlackBerry, CAT; 34 ponsel) adalah campuran heterogen, sehingga efek brand dibaca relatif terhadap campuran ini, bukan satu brand tertentu.
- Koefisien Battery Capacity berarah negatif dan dilaporkan dengan hati-hati. Peringatan Cond. No. yang tinggi (1,13e+05) menandakan kemungkinan ketidakstabilan numerik akibat perbedaan skala atau multikolinearitas.
- Asumsi homoskedastisitas dan normalitas residual tidak terpenuhi secara ketat; dampaknya diantisipasi dengan SE robust HC3 dan ukuran sampel.

## 9. Referensi Utama

- Ahmed, T. (2022). Assessing the value of 3G and 4G network modes in mobile phone pricing. *Digital Policy, Regulation and Governance, 24*(4), 380–397.
- Cui, J., Wang, X., & Zheng, Y. (2025). Analysis of Factors Influencing Smartphone Prices. *SCITEPRESS*.
- Sidak, J. G., & Skog, J. O. (2019). Hedonic Prices for Multicomponent Products. *Journal of Innovation, 4*, 301–379.

Daftar pustaka lengkap ada di dokumen pada folder `reports/`.

## 10. Author

**Rahel** — Data Analyst (portfolio project)
GitHub: [@Rahelxv](https://github.com/Rahelxv)

---

*Project ini dibuat sebagai bagian dari portofolio data analyst.*
