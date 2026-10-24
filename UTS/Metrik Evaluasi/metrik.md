# Metrik Evaluasi

Metrik utama penelitian adalah **macro-F1** dan **MAE dalam satuan kelas**. Macro-F1 adil terhadap kelas minoritas (data tidak seimbang, rasio 3,6), sedangkan MAE mengukur seberapa jauh prediksi meleset pada skala kematangan yang berurutan.

---

## Daftar metrik

| Metrik | Mengukur | Alasan dipakai | Rujukan |
| --- | --- | --- | --- |
| **Macro-F1** | Rata-rata F1 lima kelas, bobot sama | Data tidak seimbang; akurasi bias ke kelas mayoritas | Saputra (2026) |
| **MAE (kelas)** | Rata-rata jarak tingkat prediksi ke label | Metrik utama regresi ordinal | Shi dkk. (2023) |
| **QWK** | Kesepakatan prediksi–label, hukuman kuadrat untuk kesalahan jauh | Ukuran kesepakatan untuk klasifikasi ordinal | Yilmaz & Demirhan (2023) |
| **% salah 1 tingkat vs ≥ 2 tingkat** | Jenis kesalahan | Kesalahan jauh (mis. Immature ↔ Decayed) paling merugikan | Shi dkk. (2023) |
| Akurasi, precision, recall per kelas | Performa per kelas | Pembanding dengan penelitian lain | Saputra (2026) |
| Confusion matrix | Pasangan kelas yang tertukar | Fokus PartiallyRipe ↔ FullyRipe dan OverRipe | Saputra (2026) |

