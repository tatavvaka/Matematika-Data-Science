# Catatan progres

## Pemeriksaan contoh oleh pendamping — 2026-11-03

- Contoh implementasi Soal 1 dibuat dengan bantuan AI.
- Perubahan kode berada dalam 18 area isian pada 16 sel.
- Sel Markdown, seed, impor, sel uji, dan kode di luar penanda tidak diubah.
- 37 sel kode dieksekusi dengan IPython dalam satu proses; 0 error.
- 11 kelompok angka contoh sesuai; 20 pemeriksaan tambahan bernilai True.
- Akurasi sintetis: 99.9034%.
- Ketidakkonsistenan template dicatat dalam panduan fungsi.
- Hasil ini bukan hasil penilaian sistem Dicoding.

## Pengerjaan oleh pengguna
## Soal 1 — Distribusi dan Algoritma Naive-Bayes

- Tanggal pengerjaan: 03 November 2026.
- [X] Membuat repository pribadi.
- [X] Mempelajari setiap fungsi.
- [X] Menjalankan notebook dari awal di Colab.
- [X] Memeriksa grafik, tabel, serta contoh output.
- [X] Mengunduh `.ipynb` dan `.py` terbaru.
- [X] Melakukan commit berdasarkan hasil yang benar-benar diperiksa.
- Kendala dan pemahaman baru:
- Link Colab: https://colab.research.google.com/drive/1wtQG0WN1F58OWHS1U1gCP0XCqs9TOAVr?usp=sharing
- Langkah berikutnya: Soal 2, 3, dan Knowledge Check.

## Soal 2 — A/B Testing Sistem Rekomendasi

- Tanggal pengerjaan: 03 November 2026.
- Implementasi: pembuatan data sintetis, estimasi metrik sampel,
  derajat kebebasan Satterthwaite, statistik t, dan keputusan uji dua sisi.
- Standar deviasi menggunakan ddof=1.
- Hasil pembanding: origin 2058 pengguna dan varian 2120 pengguna.
- Rata-rata artikel: origin 4,72 dan varian 5,38.
- DoF: 4168,28.
- Statistik t: -9,48.
- Keputusan pada alpha 0,05: menolak hipotesis nol.
- Pemeriksaan restart dan run all:
- Kendala dan pemahaman baru:
- Link Colab: https://colab.research.google.com/drive/13LQ30E5clJa4u1z9ve2W7_PpJXIFfyUZ?usp=sharing
- Langkah berikutnya: Soal 3 dan Knowledge Check.

## Soal 3 — Software Development A/B Testing

### Perubahan
- Tanggal pengerjaan: 03 November 2026.
- Melengkapi tujuh area isian pada template Soal 3.
- Menghitung kebutuhan sampel dan durasi pengujian.
- Membuat dataset simulasi untuk kelompok origin dan varian.
- Menghitung metrik proporsi, proporsi gabungan, z-statistik, dan keputusan uji hipotesis dua sisi.
- Link Colab: https://colab.research.google.com/drive/1e8iAc9oXTGCHKQon5bnftORZxfMvFBik?usp=sharing
- Langkah berikutnya: Knowledge Check.

### Hasil pemeriksaan
- Notebook dijalankan ulang dari awal tanpa error.
- Kebutuhan sampel minimum: 3.627 pengguna per kelompok.
- Durasi pengujian: 8 hari.
- Origin: 3.959 pengguna dengan 2.734 keberhasilan.
- Varian: 4.041 pengguna dengan 2.906 keberhasilan.
- Proporsi gabungan: 0,7050.
- Z-statistik: -2,7996.
- Hipotesis nol ditolak pada alpha 0,05.

### Pemahaman
- Proporsi keberhasilan kelompok varian lebih tinggi sekitar 2,86 poin persentase daripada origin pada data simulasi.
- Pengujian dua sisi memberikan bukti adanya perbedaan proporsi yang signifikan secara statistik.

### Kendala


### Langkah berikutnya
Mengerjakan Knowledge Check dan memeriksa kelengkapan seluruh berkas submission.
