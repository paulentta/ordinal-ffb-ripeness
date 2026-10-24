# Dataset Riset: Kematangan TBS Kelapa Sawit

Dataset berisi **4.728 citra unik** tandan buah segar (TBS) kelapa sawit dalam **5 kelas kematangan berurutan (ordinal)**. Analisis data menemukan bahwa **47% citra berada dalam kelompok foto yang bocor lintas split** pada pembagian bawaan, sehingga penelitian ini memakai **split berbasis kelompok foto per tandan/sesi** sebagai split utama.

> **Sumber dataset:** An Ordinal Dataset for Ripeness Level Classification in Oil Palm Fruit Quality Grading (https://data.mendeley.com/datasets/424y96m6sw/1)
> Deskripsi dataset (4.728 citra, 5 tahap Immature–Decayed) sama dengan dataset yang dipakai pada penelitian CFO-RetinaNet, tetapi sumber resminya perlu dikonfirmasi.

---

## 1. Struktur folder asli

```
Dataset_asli/
├── Images/
│   ├── 0Immature/        (758 file)
│   ├── 1PartiallyRipe/   (1.406 file)
│   ├── 2FullyRipe/       (1.499 file)
│   ├── 3OverRipe/        (666 file)
│   └── 4Decayed/         (411 file)
└── Train_val_test_split/
    ├── Training.txt      (3.306 nama file)
    ├── Validation.txt    (704 nama file)
    └── Testing.txt       (718 nama file)
```

Total 4.740 file `.jpg`. Sebanyak 12 file bernama `... (1).jpg` adalah **duplikat persis** (MD5 identik dengan file aslinya) dan tidak tercantum di split mana pun, sehingga dikeluarkan. Dataset kerja = **4.728 citra**.

## 2. Distribusi kelas

| Kelas (urutan) | Jumlah | % | Ciri visual utama |
| --- | ---: | ---: | --- |
| 0 Immature | 758 | 16,0 | Brondolan ungu kehitaman; banyak difoto masih di pohon |
| 1 PartiallyRipe | 1.405 | 29,7 | Merah gelap dengan sedikit oranye |
| 2 FullyRipe | 1.489 | 31,5 | Merah-oranye dominan, mulai ada brondolan lepas |
| 3 OverRipe | 665 | 14,1 | Brondolan berkurang, soket kosong dan duri dominan |
| 4 Decayed | 411 | 8,7 | Hampir tanpa buah, duri kering cokelat-krem |
| **Total** | **4.728** | **100** | |

Rasio ketidakseimbangan (terbanyak / tersedikit) = 1.489 / 411 = **3,6** (kategori sedang).

## 3. Properti teknis

| Properti | Nilai |
| --- | --- |
| Ukuran citra | Semua **256 × 256** piksel |
| Mode warna | Semua RGB |
| Format | Semua JPEG |
| File rusak | 0 |
| Ukuran file (median per kelas) | 53–60 KB |

Citra sudah diperkecil dari foto aslinya, sehingga detail halus brondolan sebagian hilang. Karena itu input model **tidak diperkecil lebih jauh** dari 224 × 224.

## 4. Split bawaan pembuat dataset

| Split | Jumlah | % |
| --- | ---: | ---: |
| Training | 3.306 | 69,9 |
| Validation | 704 | 14,9 |
| Testing | 718 | 15,2 |

Split bawaan **stratified per kelas** (proporsi kelas hampir identik di ketiga split), tetapi dibagi **acak per gambar**. Akibatnya foto-foto beruntun dari tandan yang sama tersebar ke Training dan Testing (lihat bagian 6).

## 5. Ringkasan temuan analisis data

| Langkah | Aspek | Temuan | Tindakan |
| --- | --- | --- | --- |
| 3–4 | Duplikat persis | 12 file "(1)", MD5 identik | Dikeluarkan |
| 5 | Properti teknis | Seragam, tidak ada file rusak | Resize ke 224 × 224 |
| 5 | Ketidakseimbangan | Rasio 3,6 | Metrik utama macro-F1 |
| 7, 13.4 | Shortcut latar | Warna latar saja → macro-F1 0,374 (acak 0,20); Immature berlatar hijau, OverRipe berlatar terang | Augmentasi brightness/contrast + random crop; Grad-CAM saat evaluasi |
| 11 | Kualitas gambar | Motion blur nyata pada ±1% citra; gelap/terang ekstrem umumnya karena isi gambar | Tidak dibuang; augmentasi blur acak |
| 8–12 | Near-duplicate & kebocoran | 3.010 citra (64%) punya kembaran; 479 kelompok bocor di split bawaan (2.230 citra, 47%) | Split utama diganti split berbasis kelompok |
| 12.4 | Duplikat rotasi/flip | 29 pasangan baru, 13 lintas split; terbanyak rotasi 90° | Ikut dikelompokkan |
| 13 | Warna per kelas | Warna saja → macro-F1 0,489; PartiallyRipe vs FullyRipe hampir tak terpisahkan (AUC terbaik 0,585) | Pakai CNN (tekstur + struktur); hue tidak diaugmentasi |
| 15 | Label meragukan | 740 kandidat (15,7%), terbanyak di pasangan PartiallyRipe ↔ FullyRipe | Label tidak dikoreksi; dicatat sebagai keterbatasan |

Rincian metrik dan hasil acuan ada di [`metrik evaluasi/metrik.md`](../metrik%20evaluasi/metrik.md).

## 6. Split berbasis kelompok (split utama)

Citra dikelompokkan per **tandan/sesi pemotretan**, lalu **satu kelompok utuh masuk ke satu split**. Dua citra **sekelas** digabung ke satu kelompok bila memenuhi salah satu aturan:

1. Jarak pHash ≤ 8 pada salah satu dari 8 orientasi (asli, flip, rotasi 90/180/270° dengan dan tanpa flip).
2. Termasuk kelompok duplikat langkah 10 (pHash orientasi asli).
3. Kemiripan kosinus fitur EfficientNet-B0 pretrained ≥ 0,87.
4. Kemiripan kosinus ≥ 0,80 **dan** selisih nomor file ≤ 3.

Ambang 0,87 dikalibrasi dari pola nomor file: pada kosinus ≥ 0,87, 60–93% pasangan bernomor berdekatan (selisih ≤ 5), sedangkan pada pasangan acak hanya ±1,3%.

**Hasil pengelompokan**

| Ukuran | Nilai |
| --- | ---: |
| Kelompok berisi > 1 citra | 794 |
| Citra yang punya kembaran | 3.010 |
| Kelompok terbesar | 92 citra |
| Kelompok bocor pada split bawaan | 479 |
| Kelompok bocor pada split baru | **0** |

## 8. Keterbatasan dataset

- **Tidak ada ID tandan.** Pengelompokan memakai heuristik kemiripan visual dan nomor file, sehingga kebocoran terukur adalah batas bawah.
- **Label tidak diverifikasi ahli.** Beberapa label tampak jelas keliru (mis. `PartiallyRipe921/928/704`, `Immature520–522`).
- **Kelas Decayed** diperlakukan sebagai tahap setelah OverRipe sesuai susunan pembuat dataset, padahal di industri sering dianggap kelas mutu.
- **Bias latar** antar kelas tidak bisa dihilangkan sepenuhnya.
