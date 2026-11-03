# Laporan pemeriksaan Soal 1

Tanggal pemeriksaan pendamping: 2026-11-03.

## Hasil

| Pemeriksaan | Hasil |
|---|---|
| Sel kode selesai | 37/37 |
| Error eksekusi | 0 |
| Grafik tertanam | 3 |
| Kelompok angka contoh template yang sesuai | 11/11 |
| Pemeriksaan perilaku tambahan | 20/20 |
| Data training | 2415 |
| Data testing | 1035 |
| Akurasi sintetis sebelum pembulatan | 0.999033816425 |

Kode hanya diubah dalam 18 area isian pada 16 sel. Sel Markdown dan kode di luar area isian cocok dengan template asli. `.py` diekspor dari notebook yang sama dan lolos pemeriksaan sintaks Python.

## Cara eksekusi dan batasnya

Sel dijalankan berurutan melalui IPython dalam satu proses dengan penangkapan keluaran teks, tabel, dan grafik yang sebenarnya. Mesin pemeriksaan membatasi socket sehingga proses kernel Jupyter terpisah tidak digunakan. Output dalam notebook bukan angka acuan yang ditempel sebagai hasil.

Ini adalah pengujian lokal. Belum ada pengujian di Colab pengguna maupun sistem otomatis Dicoding. Uji tersembunyi, kepatuhan penggunaan AI, dan keputusan nilai tidak dapat disahkan oleh laporan ini.

Perbandingan angka contoh memakai toleransi relatif 1e-10 dan absolut 0; pemformatan tampilan objek dapat berbeda antarversi pustaka. Tidak ada pembulatan nilai internal untuk memaksa keluaran.

## Lingkungan pemeriksaan

| Komponen | Versi |
|---|---|
| python | 3.12.14 |
| numpy | 2.3.5 |
| pandas | 2.2.3 |
| scipy | 1.17.0 |
| matplotlib | 3.10.8 |
| seaborn | 0.13.2 |
| scikit-learn | 1.8.0 |

## Rincian pemeriksaan tambahan

| Pemeriksaan | Hasil |
|---|---|
| uniform_shape_and_range | True |
| uniform_seed_reproducible | True |
| gaussian_quantile_reference | True |
| gaussian_endpoint_inf | True |
| binomial_ppf_endpoint_minus_one | True |
| binomial_support | True |
| uniform_pdf_boundary | True |
| gaussian_pdf_reference_array | True |
| binomial_mass_sums_to_one | True |
| binomial_outside_support_zero | True |
| synthetic_rows_and_columns | True |
| synthetic_class_counts | True |
| standalone_gaussian_ddof0 | True |
| training_gaussian_ddof1 | True |
| training_binomial_template_convention | True |
| priors_sum_one | True |
| likelihood_example_reference | True |
| feature_order_respected | True |
| prediction_matches_argmax | True |
| split_sizes_total | True |

## Catatan sebelum submission

Konvensi training binomial dan salah rujuk variabel uji dijelaskan di `panduan-fungsi-soal-1.md`. Soal 2, Soal 3, dan Knowledge Check belum dikerjakan. Contoh implementasi dibuat dengan bantuan AI; mengikuti angka contoh bukan bukti memenuhi larangan AI dalam ketentuan kelas.
