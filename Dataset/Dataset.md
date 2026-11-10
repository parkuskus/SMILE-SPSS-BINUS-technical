# SMILE Competition 2026: Decode the Market Sentiment

> **Profiting from Chaos: Finding Signal in a Reality.**

Di tengah derasnya informasi ekonomi, keuangan, dan bisnis, setiap kalimat dapat mengandung sinyal yang mencerminkan suatu kondisi atau sentimen. Namun, tidak semua informasi mudah dibaca.

Dalam kompetisi ini, peserta ditantang untuk membangun model *machine learning* yang mampu mengklasifikasikan sentimen teks ke dalam:

- 🟢 **POSITIF**
- ⚪ **NETRAL**
- 🔴 **NEGATIF**

Peserta bebas mengeksplorasi berbagai pendekatan *machine learning*, NLP, *deep learning*, maupun *pre-trained language model* untuk mendapatkan performa terbaik.

> *Can you find the signal in the noise?*

## Ringkasan

| Aspek | Keterangan |
| --- | --- |
| Data latih | 1.526 data berlabel (`data/train.csv`) |
| Data uji | 654 data (`data/test.csv`) |
| Contoh submisi | `data/sample_submission.csv` |
| Target | `sentiment`: POSITIF, NETRAL, NEGATIF |
| Metrik evaluasi | *Macro F1 Score* |
| Penilaian preliminary | 70% *Kaggle Leaderboard*, 30% kualitas *notebook* |

## Latar Belakang

Informasi ekonomi, keuangan, dan bisnis terus bertambah dalam jumlah besar. Di tengah arus informasi tersebut, setiap kalimat dapat mengandung berbagai sinyal yang mencerminkan kondisi, opini, maupun sentimen tertentu.

Tantangannya adalah menemukan sinyal sentimen yang bermakna di tengah *noise*.

## Dataset

Kompetisi ini berfokus pada klasifikasi sentimen teks berbahasa Indonesia dalam konteks ekonomi, keuangan, bisnis, dan informasi terkait lainnya.

| Berkas | Jumlah | Kolom |
| --- | --- | --- |
| `data/train.csv` | 1.526 baris | `id`, `kalimat`, `sentiment` |
| `data/test.csv` | 654 baris | `id`, `kalimat` |
| `data/sample_submission.csv` | 654 baris | `id`, `sentiment_label` |

Data latih memiliki label sentimen sebagai *ground truth*, sedangkan data uji digunakan untuk evaluasi akhir.

### Variabel Target

Variabel target yang harus diprediksi adalah `sentiment`. Nilainya harus persis salah satu dari:

| Label | Deskripsi |
| --- | --- |
| POSITIF | Kalimat diklasifikasikan sebagai positif. |
| NETRAL | Kalimat diklasifikasikan sebagai netral. |
| NEGATIF | Kalimat diklasifikasikan sebagai negatif. |

> Catatan: `sample_submission.csv` di repositori ini memakai nama kolom `sentiment_label`. Samakan dengan format yang diminta Kaggle saat mengunggah.

### Distribusi Data Latih

| Label | Jumlah |
| --- | --- |
| POSITIF | 458 |
| NETRAL | 610 |
| NEGATIF | 458 |
| **Total** | **1.526** |

Kelas NETRAL lebih banyak daripada POSITIF dan NEGATIF. Perhatikan ketidakseimbangan ini saat validasi dan pemodelan.

### Deskripsi Fitur

- **`id`**: identitas unik setiap baris. Dipakai untuk identifikasi dan pencocokan hasil prediksi dengan format submisi. Jangan dipakai sebagai fitur semantik karena hanya berupa penanda.
- **`kalimat`**: teks yang menjadi sumber utama informasi sentimen. Dapat berisi istilah ekonomi dan keuangan, kondisi bisnis, informasi perusahaan, kondisi pasar, pernyataan atau opini, serta konteks positif, negatif, atau netral.

## Tantangan

Peserta bebas menggunakan berbagai pendekatan, antara lain:

- *Machine learning*
- NLP
- *Feature engineering*
- *Deep learning*
- *Pre-trained language model*
- *Ensemble method*

Tujuannya adalah membangun model yang mampu memprediksi sentimen data yang belum pernah dilihat dengan performa terbaik.

## Evaluasi

Submisi dievaluasi memakai *macro F1 score*:

```text
F1-makro = (F1-POSITIF + F1-NETRAL + F1-NEGATIF) / 3
```

F1 dihitung terpisah untuk setiap kelas, lalu dirata-ratakan secara seimbang. Metrik ini memberi bobot yang sama untuk setiap kelas sehingga cocok untuk klasifikasi multikelas dengan kemungkinan ketidakseimbangan data.

Reproduksi dengan scikit-learn:

```python
from sklearn.metrics import f1_score

score = f1_score(y_true, y_pred, average="macro")
```

## Karakteristik Data

- Perbedaan panjang kalimat.
- Variasi penggunaan bahasa Indonesia.
- Istilah ekonomi dan keuangan.
- Kata yang bermakna berbeda tergantung konteks.
- Kalimat yang memuat informasi positif dan negatif sekaligus.
- Kata informal atau variasi penulisan.
- *Noise* pada teks.
- Ketidakseimbangan jumlah kelas.

Hal tersebut membuat pemilihan prapemrosesan, representasi teks, strategi validasi, dan model menjadi bagian penting dari kompetisi.

## Pencegahan Kebocoran Data

- Jangan memakai label data uji selama pengembangan model.
- Jangan menebak atau merekonstruksi label uji yang tersembunyi.
- Rancang validasi untuk meminimalkan risiko kebocoran data.
- Lakukan prapemrosesan dan *feature engineering* dengan cara yang tetap valid untuk data yang belum pernah dilihat.

Tujuan utama adalah model yang mampu menggeneralisasi ke data baru.

## Eksplorasi yang Disarankan

- Distribusi label sentimen.
- Panjang setiap kalimat.
- Kata dan frasa yang umum digunakan.
- Perbedaan penggunaan bahasa antar-kelas.
- *N-gram* yang berhubungan dengan setiap kelas.
- Kata yang dominan pada masing-masing sentimen.
- Kalimat yang sangat pendek atau sangat panjang.
- Data duplikat atau kalimat yang sangat mirip.
- Karakter khusus dan pola penulisan.
- Hubungan antara panjang teks dan sentimen.

Daftar tersebut hanya saran. Peserta bebas mengembangkan analisisnya sendiri.

## *Feature Engineering*

Contoh pendekatan dari kolom `kalimat`:

- *Bag-of-words*, TF-IDF, *word n-gram*, *character n-gram*.
- *Text embedding*, *sentence embedding*, representasi berbasis *transformer* atau *pre-trained language model*.
- Fitur panjang teks, jumlah kata, jumlah karakter, dan frekuensi kata.

## Sumber Data dan Referensi

Data sekunder yang telah dibersihkan untuk SMILE SPSS 2026:

- Yuttasihinggusti, *Indonesian Economic News Sentiment (Q1 2026)* — Kaggle. <https://www.kaggle.com/datasets/yuttasihinggusti/indonesian-economic-news-sentiment-q1-2026>
- Intanm, *Indonesian Financial Sentiment Analysis* — Hugging Face Datasets. <https://huggingface.co/datasets/intanm/indonesian-financial-sentiment-analysis>

Data asli tetap tunduk pada lisensi dan ketentuan dari masing-masing sumber. Dataset ini hanya untuk SMILE Competition 2026. Gunakan secara bertanggung jawab dan patuhi aturan tentang data, data eksternal, kebocoran data, serta redistribusi.

Sitiran kompetisi:

```text
Juan Erick, Kelvin_sap, and Steven Aten. SMILE | SPSS 2026.
https://www.kaggle.com/competitions/spss-2026, 2026. Kaggle.
```

---
Tugasnya sederhana untuk dijelaskan, tetapi menantang untuk diselesaikan: temukan sinyal yang tersembunyi di dalam *noise*.
