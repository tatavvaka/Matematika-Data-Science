# Panduan fungsi Soal 1

## Cara menggunakan dokumen ini

Buka template awal di Colab. Gunakan judul bagian dan nama fungsi sebagai penunjuk lokasi. Untuk setiap bagian, baca tujuan, pahami variabel masukan dan hasil, coba implementasinya, lalu jalankan sel uji tepat setelah fungsi. Kode di bawah merupakan contoh implementasi berbantuan AI, bukan bukti kepatuhan terhadap aturan AI submission.

Saat memakai contoh isian, salin hanya kode di dalam pagar `python` ke area antara `MULAI KODE DI SINI` dan `AKHIRI KODE DI SINI`. Indentasi adalah bagian dari sintaks Python. Jangan menambahkan impor, mengganti nama fungsi, mengubah seed, atau mengganti sel pengujian.

## Peta konsep

| Konsep | Makna dalam tugas |
|---|---|
| Uniform | Kepadatan konstan pada interval; rentang sama panjang memiliki peluang yang sama. |
| Gaussian | Distribusi kontinu berbentuk lonceng dengan rata-rata dan standar deviasi. |
| Binomial | Banyak keberhasilan dari sejumlah percobaan tetap dengan peluang keberhasilan tetap dan percobaan independen. |
| PDF | Kepadatan untuk data kontinu. Nilai PDF bukan probabilitas pada satu titik dan dapat melebihi satu. |
| PMF | Probabilitas untuk nilai diskret tertentu. |
| CDF | Peluang bahwa variabel acak bernilai kurang dari atau sama dengan suatu batas. |
| Inverse CDF / PPF | Mengubah kuantil/probabilitas menjadi nilai variabel. Untuk distribusi diskret gunakan invers umum. |
| Prior | Proporsi kelas pada data training. |
| Likelihood | Nilai model fitur jika kelas tertentu diasumsikan benar. |
| Posterior | Peluang kelas setelah mengamati fitur; untuk memilih kelas cukup membandingkan prior × likelihood. |

Naive Bayes menggunakan asumsi independensi fitur bersyarat pada kelas. Data sintetis dalam template mengulang seed pada setiap pemanggilan generator; hasil evaluasinya merupakan latihan dengan karakteristik sintetis dan tidak boleh dianggap sebagai bukti performa dunia nyata.

## Catatan template yang harus dipahami

1. **Nama `uniform_inverse_cdf` dipertahankan**, meskipun fungsi tersebut menghitung inverse CDF Gaussian. Nama merupakan kontrak pemanggilan template.
2. **Komentar `binom_inverse_cdf` di Tugas 3.2 berbeda dengan fungsi yang tersedia.** Fungsi yang benar-benar didefinisikan bernama `binomial_inverse_cdf`.
3. **Pengujian `third_binomial` mencetak `third_gaussian`.** Sel ini berada di luar area isian dan tetap dipertahankan. Pada pemeriksaan terpisah, tiga nilai pertama `third_binomial` yang sebenarnya adalah `[22., 18., 17.]`. Jadi keluaran desimal Gaussian pada baris uji itu bukan alasan mengubah generator binomial.
4. **Standar deviasi berbeda antarbagian.** Output estimasi awal cocok dengan `ddof=0` (pembagi N, sigma sekitar 1.810). Bagian training secara eksplisit meminta standar deviasi sampel, yaitu `ddof=1` (pembagi N−1). Perbedaan ini harus dicatat agar tidak tertukar.
5. **Parameter binomial pada contoh training tidak konsisten secara statistik.** Contoh output menyimpan `n_trials` sebagai maksimum yang teramati per kelas (29, 23, 17), sementara nilai p cocok dengan rata-rata dibagi 30. Contoh implementasi ini mereproduksi konvensi output tersebut. Untuk model binomial dengan 30 percobaan yang diketahui, pasangan yang konsisten secara statistik adalah n=30 dan p=mean/30. Konvensi template ini perlu diklarifikasi kepada pengajar/forum; jangan menganggapnya rumus umum estimasi binomial. Tidak ada akses ke pengujian tersembunyi Dicoding untuk memastikan ekspektasinya.
6. **`inf` dan `-1` dapat merupakan keluaran uji yang memang diminta.** Quantile Gaussian pada probabilitas 1 adalah tak hingga. `binom.ppf(0,...)` mengikuti konvensi batas SciPy dan menghasilkan −1; itu bukan jumlah hari berkicau dari data sintetis yang harus diganti menjadi nol.
7. Judul template menyebut `P(X|C)` sebagai posterior di satu tempat. Secara konsep, `P(X|C)` adalah likelihood; posterior adalah `P(C|X)`. Pada fitur kontinu, fungsi yang dihitung adalah kepadatan.
8. Template sudah mengimpor `sklearn.metrics.accuracy_score` pada sel evaluasi. Impor bawaan ini dipertahankan; classifier tetap dihitung melalui fungsi-fungsi yang diminta.

## Aturan membaca hasil

- Sel definisi fungsi dapat selesai tanpa mencetak apa pun; itu normal.
- Angka pada `Output yang diharapkan` adalah teks acuan bawaan, bukan bukti sel sudah dijalankan.
- Jangan membulatkan nilai internal; format tiga desimal dipakai hanya saat mencetak.
- Gambar acuan eksternal di Markdown mungkin memerlukan internet. Grafik hasil eksekusi ada pada output sel.
- Bila muncul error, tangani sel pertama yang gagal sebelum melanjutkan, karena fungsi berikutnya bergantung padanya.

## 1. Uniform — generate_rand_uniform

**Tujuan dan alasan:** Menghasilkan `num_samples` angka pada interval [lower_bound, upper_bound). `np.random.seed(15)` sudah ditetapkan sebelum area isian.

**Contoh isian area yang disediakan:**

```python
  # Menggunakan generator yang diwajibkan template; seed tetap di atas.
  array = np.random.uniform(
      low=lower_bound, high=upper_bound, size=num_samples
  )
```

**Cara memeriksa:** Jalankan definisi, lalu sel uji berikutnya. Tujuh angka pertama yang dicetak: 0.849, 0.179, 0.054, 0.362, 0.275, 0.530, 0.306.

## 2. Visualisasi uniform — viz_uniform_teoretis

**Tujuan dan alasan:** Gunakan generator yang baru dibuat. Histogram memakai 30 bins dan density=True agar dapat dibandingkan dengan PDF teoretis.

**Contoh isian area yang disediakan:**

```python
  array = generate_rand_uniform(lower_bound, upper_bound, num_samples)
  plt.hist(array, bins=30, density=True,
           color="lightgrey", edgecolor="black")
```

**Cara memeriksa:** Jalankan sel uji 10.000.000 sampel sesuai template. Tunggu sampai selesai. Histogram pada 0–10 mendekati kepadatan 0,1; batang tidak harus persis sama tinggi.

## 3. Inverse CDF Gaussian — uniform_inverse_cdf

**Tujuan dan alasan:** Terapkan rumus sigma × sqrt(2) × erfinv(2p−1) + miu. `np.asarray` memungkinkan masukan angka maupun array.

**Contoh isian area yang disediakan:**

```python
  x = sigma * np.sqrt(2) * erfinv(2 * np.asarray(probability) - 1) + miu
```

**Cara memeriksa:** Uji bawaan mencetak −10.997 dan inf. Jangan mengganti inf dengan angka buatan.

## 4. Generator Gaussian — generate_gaussian

**Tujuan dan alasan:** Bentuk bilangan uniform 0–1 menggunakan fungsi sebelumnya, lalu transformasikan dengan inverse CDF.

**Contoh isian area yang disediakan:**

```python
  uniform_values = generate_rand_uniform(0, 1, num_samples)
  array = uniform_inverse_cdf(uniform_values, miu, sigma)
```

**Cara memeriksa:** Tiga nilai awal Gaussian pertama: [1.03137584, −0.91958083, −1.60394386].

## 5. Visualisasi Gaussian — viz_distribusi_gaussian

**Tujuan dan alasan:** Buat tiga histogram dengan 32 bins, alpha 0,5, dan label yang berbeda.

**Contoh isian area yang disediakan:**

```python
  ax.hist(first_gaussian, bins=32, alpha=0.5, label="Gaussian 1")
  ax.hist(second_gaussian, bins=32, alpha=0.5, label="Gaussian 2")
  ax.hist(third_gaussian, bins=32, alpha=0.5, label="Gaussian 3")
```

**Cara memeriksa:** Jalankan sel pemanggil; ketiga kelompok histogram dan legend harus muncul.

## 6. Inverse CDF binomial — binomial_inverse_cdf

**Tujuan dan alasan:** Gunakan `binom.ppf` sebagaimana petunjuk; n adalah jumlah percobaan dan p adalah peluang sukses setiap percobaan.

**Contoh isian area yang disediakan:**

```python
  array = binom.ppf(probability, n=n_trials, p=success_prob)
```

**Cara memeriksa:** Uji bawaan menghasilkan 7.000, −1.000, dan 10.000.

## 7. Generator binomial — generate_binomial

**Tujuan dan alasan:** Gunakan generator uniform lalu inverse CDF binomial. Jangan menggantinya dengan generator berbeda karena output deterministiknya akan berubah.

**Contoh isian area yang disediakan:**

```python
  uniform_values = generate_rand_uniform(0, 1, num_samples)
  array = binomial_inverse_cdf(uniform_values, n_trials, success_prob)
```

**Cara memeriksa:** Dua baris awal uji: [7.,3.,2.] dan [9.,6.,4.]. Baris ketiga memiliki salah rujuk variabel bawaan; lihat catatan di atas.

## 8. Visualisasi binomial — viz_distribusi_binomial

**Tujuan dan alasan:** Visualisasikan ketiga array binomial dengan label masing-masing.

**Contoh isian area yang disediakan:**

```python
  ax.hist(first_binomial, alpha=0.5, label="Binomial 1")
  ax.hist(second_binomial, alpha=0.5, label="Binomial 2")
  ax.hist(third_binomial, alpha=0.5, label="Binomial 3")
```

**Cara memeriksa:** Ketiga histogram tampil; data binomial berisi nilai bilangan bulat meskipun representasi array memakai float.

## 9. Data sintetis — generate_data_synthetic

**Tujuan dan alasan:** Gunakan argumen `features` dan `params`, lalu pilih distribusi berdasarkan nama fitur. Dua fitur memakai Gaussian, sing_days memakai binomial, dan rasio paruh/kepala memakai uniform.

**Contoh isian area yang disediakan:**

```python
  for feature in features:
      feature_params = params[breed][feature]
      if feature in ("wingspan_cm", "weight_g"):
          df[feature] = generate_gaussian(
              feature_params.miu, feature_params.sigma, n_samples
          )
      elif feature == "sing_days":
          df[feature] = generate_binomial(
              feature_params.n_trials, feature_params.probability, n_samples
          )
      elif feature == "beak_head_ratio":
          df[feature] = generate_rand_uniform(
              feature_params.lower_bound, feature_params.upper_bound, n_samples
          )
```

**Cara memeriksa:** Jalankan sel ini, tabel head, serta sel split bawaan setelahnya. Total 3.450 baris; kelas 0/1/2 berjumlah 1.200/1.350/900; training 2.415 dan testing 1.035. Tidak perlu mengunggah CSV tambahan.

## 10. PDF uniform — pdf_for_uniform

**Tujuan dan alasan:** Kepadatan 1/(b−a) jika nilai berada dalam [a,b], dan nol di luar interval.

**Contoh isian area yang disediakan:**

```python
  if lower_bound <= val <= upper_bound:
      pdf = 1 / (upper_bound - lower_bound)
  else:
      pdf = 0.0
```

**Cara memeriksa:** Uji menghasilkan 0.500 dan 0.000.

## 11. PDF Gaussian — pdf_for_gaussian

**Tujuan dan alasan:** Pisahkan faktor normalisasi 1/(sigma sqrt(2π)) dan eksponensial. Masukan list/array dikonversi menjadi array agar operasi elemen berjalan.

**Contoh isian area yang disediakan:**

```python
  val = np.asarray(val)
  pdf = (1 / (sigma * np.sqrt(2 * np.pi))) * np.exp(
      -0.5 * ((val - miu) / sigma) ** 2
  )
```

**Cara memeriksa:** Uji menghasilkan 0.001, 0.000, 0.002. Angka 0.000 dapat berasal dari pembulatan nilai sangat kecil; jangan mengubah nilai internal menjadi nol.

## 12. PMF binomial — pmf_for_binomial

**Tujuan dan alasan:** Gunakan `binom.pmf`, yang direkomendasikan dalam petunjuk template dan sudah diimpor.

**Contoh isian area yang disediakan:**

```python
  pmf = binom.pmf(val, n=n_trials, p=probability)
```

**Cara memeriksa:** Uji menghasilkan 0.206, 0.144, dan 0.000. Nilai di atas jumlah percobaan memiliki massa probabilitas nol.

## 13. Estimasi Gaussian — gaussian_params_estimation

**Tujuan dan alasan:** Hitung rata-rata serta standar deviasi dengan ddof=0 agar sesuai output contoh pada fungsi ini.

**Contoh isian area yang disediakan:**

```python
  miu = np.mean(sample)
  # ddof=0 mereproduksi output contoh fungsi ini (sigma 1.810).
  sigma = np.std(sample, ddof=0)
```

**Cara memeriksa:** Contoh menghasilkan miu 47.400 dan sigma 1.810. Tiga fungsi estimasi berada dalam satu sel; isi semua bagian sebelum menjalankan sel uji berikutnya.

## 14. Estimasi binomial — binomial_params_estimation

**Tujuan dan alasan:** n_trials=30 sudah ditetapkan di luar area isian. Rata-rata jumlah sukses dibagi 30 mengestimasi peluang sukses.

**Contoh isian area yang disediakan:**

```python
  success_probability = np.mean(sample) / n_trials
```

**Cara memeriksa:** Uji menghasilkan n=30 dan p=0.600.

## 15. Estimasi uniform — uniform_params_estimation

**Tujuan dan alasan:** Gunakan minimum dan maksimum sampel untuk memperkirakan batas interval.

**Contoh isian area yang disediakan:**

```python
  lower_bound = np.min(sample)
  upper_bound = np.max(sample)
```

**Cara memeriksa:** Uji menghasilkan batas bawah 1.200 dan batas atas 4.000.

## 16. Training — training_params_estimation

**Tujuan dan alasan:** Untuk setiap kelas, pilih baris kelas tersebut, hitung prior, lalu estimasikan parameter setiap fitur. Gunakan data training saja. Pahami catatan ddof dan konvensi binomial sebelum memakai contoh ini.

**Contoh isian area yang disediakan:**

```python
    for breed in df["breed"].unique():
        df_breed = df[df["breed"] == breed][features]
        probs_dict[breed] = len(df_breed) / len(df)

        inner_dict = {} #JANGAN DIHAPUS
        for feature in df_breed.columns:
            sample = df_breed[feature]
            if feature in ("wingspan_cm", "weight_g"):
                # Pada tahap training template meminta standar deviasi sampel.
                params = gaussian_params(miu=sample.mean(), sigma=sample.std(ddof=1))
            elif feature == "sing_days":
                # Template menampilkan n=max sampel, tetapi p=mean/30.
                # Pertahankan konvensi contoh ini; lihat catatan konseptual di panduan.
                _, success_probability = binomial_params_estimation(sample)
                params = binomial_params(
                    n_trials=int(sample.max()), probability=success_probability
                )
            elif feature == "beak_head_ratio":
                lower_bound, upper_bound = uniform_params_estimation(sample)
                params = uniform_params(lower_bound, upper_bound)
            inner_dict[feature] = params
        params_dict[breed] = inner_dict
```

**Cara memeriksa:** Prior kelas 0/1/2 sekitar 0.345756, 0.395445, 0.258799 dan jumlahnya satu. Cocokkan juga tabel parameter dengan output yang diharapkan.

## 17. Likelihood — probability_given_class

**Tujuan dan alasan:** Pasangkan nilai dan fitur dengan zip, ambil parameter kelas, hitung PDF atau PMF yang sesuai, kemudian kalikan semua kontribusi.

**Contoh isian area yang disediakan:**

```python
    for x, feature in zip(X, features):
        params = params_dict[breed][feature]
        if feature in ("wingspan_cm", "weight_g"):
            probability_f = pdf_for_gaussian(x, params.miu, params.sigma)
        elif feature == "sing_days":
            probability_f = pmf_for_binomial(x, params.n_trials, params.probability)
        elif feature == "beak_head_ratio":
            probability_f = pdf_for_uniform(x, params.lower_bound, params.upper_bound)
        probability *= probability_f
```

**Cara memeriksa:** Pada burung contoh, likelihood kelas 0/1/2 sekitar 1.9252029362842843e−86, 1.863471980693772e−20, dan 9.672973185945612e−09. Perbedaan digit paling akhir floating point dapat terjadi.

## 18. Prediksi — breed_bird_prediction

**Tujuan dan alasan:** Kalikan likelihood tiap kelas dengan prior kelas tersebut. Susun sesuai urutan kelas 0,1,2 lalu pilih indeks skor maksimum. Skor itu belum dinormalisasi menjadi probabilitas posterior.

**Contoh isian area yang disediakan:**

```python
    posterior_breed_0 = probability_given_class(X, features, 0, params_dict) * probs_dict[0]
    posterior_breed_1 = probability_given_class(X, features, 1, params_dict) * probs_dict[1]
    posterior_breed_2 = probability_given_class(X, features, 2, params_dict) * probs_dict[2]
    posterior = np.array([posterior_breed_0, posterior_breed_1, posterior_breed_2])
    prediction = np.argmax(posterior)
```

**Cara memeriksa:** Contoh burung kelas 2 diprediksi sebagai 2. Jalankan sel evaluasi setelahnya; contoh ini mencetak 1.00 setelah dibulatkan dua desimal, sedangkan nilai sebenarnya sekitar 0.9990338164 (99.9034%).

## Pemahaman akhir yang perlu kamu jelaskan sendiri

1. Mengapa generator Gaussian menggunakan data uniform dan inverse CDF?
2. Apa perbedaan PDF, PMF, dan CDF?
3. Mengapa prior dihitung dari data training?
4. Mengapa likelihood beberapa fitur dikalikan?
5. Mengapa skor prior × likelihood cukup untuk memilih kelas tanpa membagi P(X)?
6. Mengapa akurasi tinggi pada data sintetis belum menjamin akurasi pada data nyata?

## Referensi

- Template Soal 1 yang disediakan pengguna; instruksi dan keluaran acuannya dipertahankan.
- NumPy std: https://numpy.org/doc/stable/reference/generated/numpy.std.html
- SciPy binom: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.binom.html
- SciPy erfinv: https://docs.scipy.org/doc/scipy/reference/generated/scipy.special.erfinv.html
