# UTS: Riset Klasifikasi Kematangan TBS Kelapa Sawit

Folder ini berisi dokumen riset UTS untuk penelitian **klasifikasi tingkat kematangan tandan buah segar (TBS) kelapa sawit menggunakan EfficientNet-B0 dengan regresi ordinal dan evaluasi berbasis kelompok data**.

## Isi folder

| Folder | Berkas | Isi |
| --- | --- | --- |
| [`dataset riset`](dataset%20riset/) | [`dataset.md`](dataset%20riset/dataset.md) | Karakteristik dataset, temuan analisis data, dan split berbasis kelompok |
| [`metrik evaluasi`](metrik%20evaluasi/) | [`metrik.md`](metrik%20evaluasi/metrik.md) | Definisi metrik, kode perhitungan, hasil acuan, dan protokol evaluasi |
| [`proposal penelitian`](proposal%20penelitian/) | [`proposal.md`](proposal%20penelitian/proposal.md) | Latar belakang, rumusan masalah, metodologi, skenario eksperimen, hipotesis |
| [`riset gap`](riset%20gap/) | [`gap.md`](riset%20gap/gap.md) | Pemetaan penelitian terdahulu dan celah penelitian G1–G5 |
| [`riset mind map`](riset%20mind%20map/) | [`mind map.md`](riset%20mind%20map/mind%20map.md), [`Peta Riset Kematangan TBS Sawit.png`](riset%20mind%20map/Peta%20Riset%20Kematangan%20TBS%20Sawit.png) | Peta konsep penelitian |

## Urutan membaca

1. [`riset mind map`](riset%20mind%20map/mind%20map.md) untuk gambaran besar.
2. [`riset gap`](riset%20gap/gap.md) untuk alasan penelitian ini perlu dilakukan.
3. [`dataset riset`](dataset%20riset/dataset.md) untuk karakteristik dan masalah data.
4. [`metrik evaluasi`](metrik%20evaluasi/metrik.md) untuk cara mengukur hasil.
5. [`proposal penelitian`](proposal%20penelitian/proposal.md) untuk rancangan lengkap.

## Temuan utama analisis data

| Temuan | Angka |
| --- | --- |
| Citra unik setelah membuang duplikat persis | 4.728 (dari 4.740) |
| Citra dalam kelompok yang bocor di split bawaan | **2.230 (47%)** |
| Macro-F1 dari fitur warna saja | 0,489 |
| Macro-F1 fitur EfficientNet-B0 tanpa fine-tuning | 0,811 |
| Kesalahan model awal yang hanya meleset 1 tingkat | 79% |
| Kandidat label meragukan | 740 (15,7%) |

## Status

- [x] Studi literatur dan analisis data (langkah 1–15)
- [x] Rancangan pipeline dan proposal
- [ ] Implementasi dan training (E1, E2, E1-B, E2-B)
- [ ] Evaluasi dan penulisan hasil
