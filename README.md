# Segmentasi Polip pada Citra Kolonoskopi dengan U-Net

**Tugas P10 — Kecerdasan Buatan (BM25-41204), Teknik Biomedis ITERA**

## Anggota kelompok

| Nama | NIM | Username GitHub | Kontribusi utama |
|---|---|---|---|
| Nahdatul Azizah Putri | 123430006 | dkwlzh | Bertanggung jawab dalam melakukan konfigurasi lingkungan kerja, mulai dari persiapan Python dan Jupyter Notebook. Selain itu, melakukan pengunduhan dan pemeriksaan dataset Kvasir-SEG, membandingkan hasil eksperimen berdasarkan nilai Dice Score dan IoU pada setiap skenario augmentasi, serta membantu menyusun dan merapikan README.md. |
| Indah Nabila Putri. T | 123430047 | indh992 | Bertanggung jawab dalam pelaksanaan dan pengamatan hasil eksperimen segmentasi menggunakan model U-Net. Selain itu, melakukan analisis terhadap hasil setiap skenario augmentasi, mengamati perbedaan hasil segmentasi melalui peta kesalahan, serta membantu menyusun bagian pembahasan dan kesimpulan pada README.md. |
| Erni Afriyanti Hulu | 123430067 | erni123430067-afk | Bertanggung jawab dalam membantu pengaturan skenario eksperimen dan proses pengujian model menggunakan Google Colab. Selain itu, membantu memantau proses pelatihan, mendokumentasikan hasil dari setiap skenario augmentasi, serta mengumpulkan hasil pengujian sebagai bahan evaluasi dan penyusunan laporan. |

## 1. Pertanyaan

Augmentasi apa yang membantu U-Net menemukan polip pada citra kolonoskopi, dan augmentasi apa yang tidak? Kenapa?

Pada eksperimen ini, augmentasi yang digunakan meliputi horizontal flip, vertical flip, rotasi hingga 30°, serta perubahan kecerahan dan kontras. Selain itu, dilakukan pengujian kombinasi dari beberapa augmentasi tersebut. Skenario tanpa augmentasi juga digunakan sebagai pembanding.

Augmentasi seperti cropping, zoom, blur, noise, dan elastic deformation tidak digunakan dalam eksperimen ini. Jenis augmentasi tersebut tidak dimasukkan karena pengujian dibatasi pada perubahan orientasi dan tampilan warna atau pencahayaan citra. Pembatasan ini juga bertujuan menjaga bentuk dan batas polip agar tidak berubah secara berlebihan sehingga tetap sesuai dengan mask segmentasinya.

Catatan: Augmentasi yang tidak digunakan bukan berarti terbukti tidak membantu. Jenis tersebut memang tidak diuji dalam eksperimen ini. Sementara itu, augmentasi mana yang benar-benar membantu U-Net baru dapat ditentukan setelah membandingkan hasil Dice Score dan IoU.

## 2. Data & pengaturan

- Dataset: Kvasir-SEG, terdiri dari 1.000 citra kolonoskopi beserta mask polip.
- Ukuran citra: 128 × 128 piksel.
- Data uji: 200 citra.
- Kumpulan data latih: 800 citra.
- Epoch: 50.
- Ulangan: 3 kali per skenario.
- Dijalankan di:  Google Colab T4.

<img width="1587" height="671" alt="image" src="https://github.com/user-attachments/assets/97d6013f-07bf-4fb3-aec7-9fca5b68d22a" />


## 3. Skenario augmentasi & hipotesis

| Skenario | Augmentasi (kode) | Alasan klinis | Hipotesis: Dice naik/turun/sama? |
|---|---|---|---|
| Tanpa augmentasi | — | Pembanding | — |
| Flip | RandomHorizontalFlip, RandomVerticalFlip |	Posisi dan orientasi polip dapat berbeda pada setiap pengambilan citra kolonoskopi. |	Naik |
| Rotasi |	RandomRotation(degrees=30) |	Posisi kamera dan arah pengambilan citra dapat menyebabkan perbedaan orientasi polip. |	Naik |
| Kecerahan dan kontras |	ColorJitter(brightness=0.3, contrast=0.3)	| Pencahayaan dan kontras citra dapat berubah akibat kondisi pengambilan gambar. |	Naik |
| Kombinasi	| Flip, rotasi, kecerahan, dan kontras |	Menggabungkan beberapa variasi citra untuk meningkatkan keberagaman data pelatihan. |	Naik |

Hipotesis tersebut merupakan perkiraan sebelum eksperimen dilakukan. Hasil sebenarnya dapat berbeda karena setiap augmentasi memberikan perubahan karakteristik citra yang berbeda.

Contoh augmentasi yang dicoba:

<img width="1690" height="305" alt="image" src="https://github.com/user-attachments/assets/6c729ad1-c405-408f-a0c5-190e76e9b875" />


## 4. Hasil

| Skenario | Dice (rata-rata ± sb) | IoU (rata-rata ± sb) | Dice tiap ulangan |
|---|---|---|---|
| Tanpa augmentasi | 0.405 ± 0.000 | 0.285 ± 0.000 | 0.405 |
| Flip | 0.381 ± 0.000 | 0.263 ± 0.000 | 0.381 |
| Rotasi 30 derajat | 0.360 ± 0.000 | 0.244 ± 0.000 | 0.360 |
| Kecerahan & kontras | 0.331 ± 0.000 | 0.220 ± 0.000 | 0.331 |
| Kombinasi lengkap | 0.334 ± 0.000 | 0.222 ± 0.000 | 0.334 |

<img width="1089" height="409" alt="image" src="https://github.com/user-attachments/assets/d7c35de8-76c3-4037-ad93-ff223dcd1e1b" />


## 5. Analisis peta kesalahan

<img width="2000" height="1194" alt="image" src="https://github.com/user-attachments/assets/cf836ae8-ee74-46c3-aaea-e306658ad8d3" />

Berdasarkan peta kesalahan, hasil segmentasi U-Net menunjukkan perbedaan pada setiap sampel dan skenario augmentasi. Warna hijau menunjukkan bagian polip yang berhasil diprediksi dengan benar, merah menunjukkan false positive (bagian yang salah diprediksi sebagai polip), sedangkan biru menunjukkan false negative (bagian polip yang tidak berhasil terdeteksi).

1. Polip yang sering terlewat (False Negative/Biru)
   Pada sampel pertama dan kedua, terlihat masih terdapat bagian polip yang tidak terdeteksi oleh model, ditandai dengan warna biru. Hal ini menunjukkan bahwa model belum mampu mengenali seluruh area polip dengan baik. Sebaliknya, pada sampel ketiga dan keempat, hasil segmentasi lebih sesuai dengan mask dari dokter sehingga area biru yang muncul relatif sedikit.

2. Objek yang sering dikira polip (False Positive/Merah)
   Kesalahan false positive paling terlihat pada sampel pertama dan kedua. Area berwarna merah cukup luas dan tidak sesuai dengan area polip pada jawaban dokter. Hal ini menunjukkan bahwa model masih kesulitan membedakan polip dengan bagian lain pada citra kolonoskopi. Sementara itu, pada sampel ketiga dan keempat, area merah jauh lebih sedikit sehingga hasil segmentasinya lebih mendekati mask sebenarnya.

3. Apakah augmentasi memperbaiki kasus sulit?
   Hasil peta kesalahan menunjukkan bahwa augmentasi belum memberikan perbaikan yang konsisten. Pada sampel pertama, rotasi dan kombinasi lengkap memperoleh Dice 0,12, lebih baik dibandingkan tanpa augmentasi dan flip yang memperoleh Dice 0,00. Namun, pada sampel kedua, seluruh skenario masih menghasilkan Dice yang sangat rendah. Pada sampel ketiga, flip memperoleh Dice tertinggi sebesar 0,92, sedangkan pada sampel keempat tanpa augmentasi dan flip memperoleh Dice 0,97.

Dari hasil tersebut, flip terlihat memberikan hasil yang cukup baik pada beberapa sampel, tetapi belum mampu memperbaiki seluruh kasus sulit. Hal ini menunjukkan bahwa pengaruh augmentasi berbeda-beda tergantung karakteristik citra polip yang digunakan.

## 6. Pembahasan & kesimpulan

A. Pembahasan
    Hasil eksperimen menunjukkan bahwa penggunaan augmentasi belum tentu meningkatkan kemampuan U-Net dalam melakukan segmentasi polip. Skenario tanpa augmentasi memperoleh nilai Dice tertinggi sebesar 0,405 dan IoU sebesar 0,285. Sementara itu, skenario flip memperoleh Dice 0,381, rotasi 0,360, kecerahan dan kontras 0,331, serta kombinasi lengkap 0,334. Pada skenario flip dan rotasi, nilai Dice mengalami penurunan dibandingkan tanpa augmentasi. Padahal, kedua augmentasi tersebut diperkirakan dapat membantu model mengenali polip dari berbagai posisi dan orientasi. Hal ini menunjukkan bahwa penambahan variasi posisi citra belum tentu memberikan hasil yang lebih baik pada model yang digunakan. Augmentasi kecerahan dan kontras memperoleh nilai Dice paling rendah, yaitu 0,331. Perubahan intensitas warna dan pencahayaan kemungkinan memengaruhi karakteristik visual polip sehingga model lebih sulit membedakannya dari jaringan di sekitarnya. Sementara itu, kombinasi lengkap memperoleh Dice 0,334. Hasil ini menunjukkan bahwa penggabungan beberapa augmentasi belum tentu menghasilkan peningkatan performa.
    Secara keseluruhan, hasil pengujian tidak sesuai dengan hipotesis awal yang memperkirakan bahwa seluruh augmentasi dapat meningkatkan nilai Dice. Hal ini menunjukkan bahwa setiap jenis augmentasi memberikan pengaruh yang berbeda terhadap proses pembelajaran model. Oleh karena itu, pemilihan augmentasi perlu mempertimbangkan karakteristik citra dan hasil pengujian yang diperoleh.
    
B. Kesimpulan
    1. Skenario tanpa augmentasi memberikan hasil terbaik dengan Dice 0,405 dan IoU 0,285 dibandingkan seluruh skenario augmentasi yang diuji.
    2. Augmentasi flip dan rotasi belum mampu meningkatkan hasil segmentasi, meskipun keduanya bertujuan memberikan variasi posisi dan orientasi citra.
    3. Kecerahan dan kontras memperoleh hasil terendah dengan Dice 0,331. Kombinasi lengkap juga belum memberikan peningkatan performa.
    4. Hipotesis awal bahwa augmentasi dapat meningkatkan hasil segmentasi belum terbukti pada eksperimen ini. Dengan demikian, penggunaan augmentasi perlu              disesuaikan dengan karakteristik data dan diuji terlebih dahulu sebelum diterapkan.
   
## 7. Pembagian kerja

- Nahdatul Azizah Putri	: Konfigurasi lingkungan, pengunduhan dan pemeriksaan dataset, perbandingan nilai Dice dan IoU, serta penyusunan README.md.
- Indah Nabila Putri. T	: Pelaksanaan dan pengamatan hasil eksperimen, serta analisis hasil augmentasi, serta penyusunan README.md.
- Erni Afriyanti Hulu   :	Pengaturan eksperimen, membantu proses pengujian pada Google Colab, dan dokumentasi hasil eksperimen.

## 8. Penggunaan AI

- ChatGPT digunakan untuk membantu menyusun dan memperbaiki bahasa pada bagian analisis, pembahasan, serta kesimpulan hasil eksperimen.
- Gemini dimanfaatkan untuk membantu memperbaiki kode pada proses segmentasi citra.
- Copilot digunakan untuk membantu mengatasi kendala saat melakukan git pull serta menyinkronkan pembaruan repository terbaru ke repository lokal.

## Cara menjalankan ulang

```bash
uv sync
uv run jupyter lab segmentasi_polip.ipynb
```

Atau buka notebook di Google Colab (Runtime → Change runtime type → T4 GPU).

## Referensi

- Jha, D., dkk. (2020). Kvasir-SEG: A Segmented Polyp Dataset. *MultiMedia Modeling (MMM 2020)*, 451–462.
- Ronneberger, O., Fischer, P., & Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. *MICCAI*. arXiv:1505.04597.
- M. F. Ahamed, M. R. Islam, M. Nahiduzzaman, M. E. H. Chowdhury, A. Alqahtani, and M. Murugappan, “Automated Colorectal Polyps Detection from Endoscopic Images    using MultiResUNet Framework with Attention Guided Segmentation,” Human-Centric Intelligent Systems 2024 4:2, vol. 4, no. 2, pp. 299–315, Apr. 2024, doi:         10.1007/S44230-024-00067-1.
- X. He, “A multi-resolution unet algorithm based on data augmentation and multi-center training for polyp automatic segmentation,” Journal of Intelligent and      Fuzzy Systems, vol. 44, no. 3, pp. 4593–4604, 2023, doi: 10.3233/JIFS-223340;WGROUP:STRING:PUBLICATION.

