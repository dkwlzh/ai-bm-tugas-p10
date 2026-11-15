# Segmentasi Polip pada Citra Kolonoskopi dengan U-Net

<!--
  TEMPLATE LAPORAN — Tugas P10 Kecerdasan Buatan (BM25-41204), Teknik Biomedis ITERA.
  Petunjuk lengkap: https://mctm.web.id/ai-bm/tugas

  Cara memakai:
  - Ganti semua teks yang diawali "✍️" dengan tulisan kelompokmu, lalu hapus tanda ✍️-nya.
  - Blok komentar seperti ini adalah petunjuk; tidak tampil di GitHub. Boleh dihapus.
  - Gambar diambil dari folder hasil/ yang dibuat otomatis oleh notebook.
  - Jangan hapus judul-judul bagian (##).
-->

**Tugas P10 — Kecerdasan Buatan (BM25-41204), Teknik Biomedis ITERA**

## Anggota kelompok

| Nama | NIM | Username GitHub | Kontribusi utama |
|---|---|---|---|
| Nahdatul Azizah Putri | 123430006 | dkwlzh | persiapan proyek dan repository GitHub, konfigurasi Python dan Jupyter Notebook, serta pengunduhan dan pemeriksaan dataset Kvasir-SEG yang berisi 1.000 citra kolonoskopi beserta mask polip. Pengerjaan notebook mencakup perumusan pertanyaan penelitian, pengisian data dan pengaturan eksperimen, serta penentuan skenario augmentasi dan hipotesis pengaruhnya terhadap hasil segmentasi polip|
| ✍️ | ✍️ | ✍️ | ✍️ |
| ✍️ | ✍️ | ✍️ | ✍️ |

## 1. Pertanyaan

Augmentasi apa yang membantu U-Net menemukan polip pada citra kolonoskopi, dan augmentasi apa yang tidak? Kenapa?

## 2. Data & pengaturan

- Dataset: Kvasir-SEG, terdiri dari 1.000 citra kolonoskopi beserta mask polip.
- Ukuran citra: 128 × 128 piksel.
- Data uji: 200 citra.
- Kumpulan data latih: 800 citra.
- Data latih yang digunakan: 400 citra per ulangan.
- Model: U-Net Mini dengan sekitar 0,5 juta parameter.
- Epoch: 10.
- Ulangan: 3 kali per skenario.
- Perangkat: Laptop dengan CPU.
- Framework: PyTorch 2.15.0+cpu.

![Contoh data](hasil/contoh_data.png)

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

![Contoh augmentasi](hasil/augmentasi_flip.png)
<!-- Ganti nama file sesuai skenario kalian (lihat isi folder hasil/). -->

## 4. Hasil

<!-- Tempel isi hasil/ringkasan.md di sini. -->

✍️ tempel tabel dari `hasil/ringkasan.md`

![Perbandingan Dice](hasil/perbandingan.png)

## 5. Analisis peta kesalahan

![Peta kesalahan](hasil/peta_kesalahan.png)

<!-- Hijau = benar, merah = false positive, biru = false negative.
     Pertanyaan pemandu: Polip seperti apa yang sering terlewat (kecil? datar? warna mirip dinding usus?)
     Apa yang sering dikira polip padahal bukan? Apakah augmentasi memperbaiki kasus sulit? -->

✍️

## 6. Pembahasan & kesimpulan

<!-- Bandingkan hasil dengan hipotesis di Bagian 3. Hipotesis yang ternyata salah tetap bernilai,
     asalkan dijelaskan kenapa. Perbedaan kecil (lebih kecil dari simpangan baku) belum tentu berarti. -->

✍️

## 7. Pembagian kerja

<!-- Siapa mengerjakan apa. Penilaian proses kerja dilihat dari riwayat commit tiap anggota. -->

✍️

## 8. Penggunaan AI

<!-- Wajib diisi, walau tidak memakai AI (tulis "Tidak memakai AI").
     Contoh: "Claude Code (CLI) untuk menulis skenario augmentasi dan memperbaiki error di Bagian 4.
     Draf Bagian 6 ditulis sendiri, lalu dirapikan bahasanya dengan bantuan AI." -->

✍️

## Cara menjalankan ulang

```bash
uv sync
uv run jupyter lab segmentasi_polip.ipynb
```

Atau buka notebook di Google Colab (Runtime → Change runtime type → T4 GPU).

## Referensi

- Jha, D., dkk. (2020). Kvasir-SEG: A Segmented Polyp Dataset. *MultiMedia Modeling (MMM 2020)*, 451–462.
- Ronneberger, O., Fischer, P., & Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. *MICCAI*. arXiv:1505.04597.
- ✍️ tambahkan referensi lain yang kalian pakai
