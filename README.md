# SMILE SPSS 2026 — Klasifikasi Sentimen Ekonomi

## Permasalahan

Tugas kompetisi ini adalah mengklasifikasikan setiap kalimat berbahasa Indonesia bertema ekonomi, bisnis, dan keuangan sebagai **NEGATIF**, **NETRAL**, atau **POSITIF**. Kalimat bisa memuat istilah khusus, informasi campuran, atau ekspresi yang bergantung pada konteks. Metrik evaluasinya **Macro F1**, sehingga ketiga kelas dinilai dengan bobot yang sama.

## Dataset

Dataset resmi tersedia di [`Dataset/`](Dataset/):

| Berkas | Isi |
| --- | --- |
| `train.csv` | 1.526 kalimat berlabel; kolom `id`, `kalimat`, dan `sentiment`. |
| `test.csv` | 654 kalimat tanpa label; kolom `id` dan `kalimat`. |
| `sample_submission.csv` | Contoh keluaran dengan kolom `id` dan `sentiment_label`. |

Distribusi label pada data latih adalah **458 POSITIF**, **610 NETRAL**, dan **458 NEGATIF**. `id` hanya dipakai untuk identifikasi baris, bukan sebagai fitur semantik. Penjelasan data yang lebih terperinci tersedia di [`Dataset/Dataset.md`](Dataset/Dataset.md); dokumen tersebut berasal dari folder sumber dan masih menyebut lokasi lama `data/`.

## Pendekatan yang diimplementasikan

[`GARUDA_Executed_final.ipynb`](GARUDA_Executed_final.ipynb) menggabungkan **empat encoder berbasis bobot pralatih** (IndoBERT Large p2, Twitter XLM-R Base Sentiment, XLM-R Large, dan Twitter XLM-R Large untuk model *student*) dengan satu pengklasifikasi leksikal **TF-IDF kata/karakter + SVC**. Tiga encoder pertama membentuk *teacher*. Prediksi data uji hanya menjadi *pseudo-label* untuk pelatihan *student* apabila keyakinan *teacher* mencapai **0,95**, disetujui SVC, dan lolos pemeriksaan kemiripan. Validasi memakai pembagian terstratifikasi; prediksi lima anggota kemudian dirata-ratakan dengan bobot yang sama. Notebook mencatat *hash* input dan bobot untuk audit serta menulis hasil ke `outputs/garuda_final/submission_garuda.csv`.

**Catatan:** notebook menggunakan model pralatih dan memerlukan dependensi ML serta checkpoint yang tidak disalin. Konfigurasi cache lokal di notebook menunjuk ke drive `D:`; bila checkpoint tidak tersedia secara lokal, notebook mencoba mengunduhnya. Ketentuan babak final tersedia di [`guidebook/Teknis Lomba SMILE Babak Final.md`](guidebook/Teknis%20Lomba%20SMILE%20Babak%20Final.md).
