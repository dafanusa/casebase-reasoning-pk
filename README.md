# Sistem Case-Based Reasoning untuk Prediksi Putusan Hukum

Repositori ini memuat pipeline lengkap pengembangan sistem Case-Based Reasoning (CBR) yang mencakup tahapan akuisisi data, representasi kasus, retrieval kasus, reuse solusi, dan evaluasi model. Sistem dirancang untuk mendukung analisis putusan hukum dengan memanfaatkan kemiripan kasus terdahulu sebagai dasar rekomendasi penyelesaian kasus baru. Dataset yang digunakan berasal dari perkara pidana Penipuan dan Penggelapan pada Pengadilan Negeri Sidoarjo.

# Case-Based Reasoning untuk Prediksi Putusan Mahkamah Agung

Repositori ini berisi implementasi sistem **Case-Based Reasoning (CBR)** yang digunakan untuk menemukan kasus hukum terdahulu yang memiliki kemiripan dengan kasus baru, kemudian memanfaatkan informasi tersebut untuk membantu memprediksi solusi atau putusan yang relevan. Studi kasus yang digunakan adalah perkara **pidana penipuan dan penggelapan** berdasarkan kumpulan putusan Pengadilan Negeri Sidoarjo.

---

## Persiapan Instalasi

### Mengunduh Repository

repositori dapat diunduh menggunakan perintah berikut:

```bash
git clone https://github.com/nama-kamu/cbr-mahkamah.git
cd cbr-mahkamah
```

### Instalasi Dependensi

Disarankan menggunakan virtual environment agar kebutuhan library proyek terisolasi dengan baik (opsional).

```bash
pip install -r requirements.txt
```

### Instalasi Pendukung OCR

Karena sistem memanfaatkan ekstraksi teks dari dokumen PDF, diperlukan beberapa utilitas tambahan:

```bash
sudo apt install poppler-utils tesseract-ocr tesseract-ocr-ind -y
```

---

## Struktur Direktori

```text
project-root/
├── tahap_1_5.py
├── requirements.txt
├── Data/
│   ├── raw/              # Hasil ekstraksi teks dari PDF
│   ├── processed/        # Data kasus yang telah diproses
│   ├── results/          # Hasil retrieval dan prediksi
│   └── eval/             # Data evaluasi sistem
├── CSV/                  # Dataset hasil scraping
└── PDF/                  # Dokumen putusan dalam format PDF
```

---

## Tahapan Sistem

Sistem dikembangkan dengan pendekatan **Case-Based Reasoning (CBR)** yang terdiri atas beberapa tahapan utama.

### 1. Akuisisi Data dan OCR

Tahap awal dilakukan untuk memperoleh data putusan melalui proses scraping, mengunduh dokumen PDF, kemudian mengekstraksi isi dokumen menjadi teks menggunakan OCR.

Menjalankan proses scraping:

```python
run_scraper()
```

Hasil ekstraksi teks akan disimpan pada direktori:

```text
Data/raw/
```

---

### 2. Representasi Kasus

Data hasil scraping dan OCR selanjutnya diolah menjadi representasi kasus yang terstruktur sehingga dapat digunakan dalam proses pencarian kemiripan kasus.

Output utama:

```text
cases.csv
```

---

### 3. Pencarian Kasus Serupa (Case Retrieval)

Sistem menyediakan dua pendekatan retrieval untuk menemukan kasus yang paling relevan terhadap query pengguna.

#### Metode TF-IDF

```python
retrieve(query, k=3)
```

#### Metode BERT Embedding

```python
retrieve_bert(query, k=3)
```

Parameter `k` menunjukkan jumlah kasus dengan tingkat kemiripan tertinggi yang akan ditampilkan.

---

### 4. Prediksi Solusi (Solution Reuse)

Setelah kasus serupa ditemukan, sistem memanfaatkan informasi dari kasus tersebut untuk menghasilkan prediksi putusan terhadap kasus baru.

```python
predict_outcome(query)
```

Output:

```text
predictions.csv
```

---

### 5. Evaluasi dan Visualisasi

Tahap evaluasi dilakukan untuk mengukur performa sistem menggunakan beberapa metrik, antara lain:

- Accuracy
- Precision
- Recall
- F1-Score

Selain itu, sistem juga menghasilkan visualisasi perbandingan performa antar metode retrieval.

---

## Contoh Query Pengujian

```json
[
  {
    "query_id": "q1",
    "query_text": "Penipuan jual beli rumah secara online"
  },
  {
    "query_id": "q2",
    "query_text": "Investasi bodong oleh terdakwa"
  },
  {
    "query_id": "q3",
    "query_text": "Penipuan kendaraan bermotor"
  },
  {
    "query_id": "q4",
    "query_text": "Mengaku pegawai bank dan menipu korban"
  },
  {
    "query_id": "q5",
    "query_text": "Transaksi fiktif pengadaan barang"
  }
]
```

---

## Hasil yang Dihasilkan Sistem

| Nama File | Keterangan |
|------------|------------|
| `cases.csv` | Dataset kasus yang telah diproses |
| `queries.json` | Kumpulan query untuk pengujian |
| `predictions.csv` | Hasil prediksi putusan |
| `retrieval_metrics.csv` | Evaluasi performa metode TF-IDF |
| `retrieval_metrics_bert.csv` | Evaluasi performa metode BERT |
| `performance_comparison.png` | Grafik perbandingan performa model |

---

## Alur Singkat Sistem

```text
Scraping Data
      │
      ▼
 Download PDF
      │
      ▼
 OCR Dokumen
      │
      ▼
Representasi Kasus
      │
      ▼
 Case Retrieval
(TF-IDF / BERT)
      │
      ▼
 Prediksi Solusi
      │
      ▼
Evaluasi Sistem
```

---

## Tujuan Pengembangan

Proyek ini bertujuan untuk membantu proses pencarian referensi putusan yang relevan dengan kasus baru melalui pendekatan **Case-Based Reasoning**, sehingga dapat mendukung analisis hukum berbasis data secara lebih cepat dan terstruktur.