# DEBUG LOG
## Proyek Katalog Karya – Debugging P10

Dokumen ini berisi catatan proses identifikasi, analisis, perbaikan, dan pengujian ulang terhadap enam gejala (G1–G6) pada proyek Katalog Karya.

---

## G1 – Logo Tidak Muncul

**Gejala:**
Logo pada halaman utama tidak tampil meskipun elemen gambar sudah ditambahkan ke dalam HTML.

**Langkah Reproduksi:**
1. Jalankan halaman `index.html` melalui browser.
2. Perhatikan bagian logo pada halaman utama.
3. Amati apakah gambar logo berhasil ditampilkan.

**Hipotesis Penyebab:**
- Path gambar pada atribut `src` tidak sesuai dengan lokasi file sebenarnya.
- File logo tidak ditemukan oleh browser.
- Format gambar tidak didukung browser.

**Hasil Analisis:**

Setelah dilakukan pemeriksaan, ditemukan bahwa path pada HTML mengarah ke `img/logo.svg`, sedangkan file logo sebenarnya berada di folder `assets/`.

**Bukti DevTools:**
- Buka Developer Tools menggunakan tombol F12.
- Pilih tab Network.
- Periksa permintaan file `logo.svg`.
- Catat status HTTP dan path file yang diminta.
- Simpan screenshot sebagai bukti sebelum dan sesudah perbaikan.

**Perbaikan:**

Mengubah path gambar pada `index.html`:

Sebelum:
`src="img/logo.svg"`

Sesudah:
`src="assets/logo.svg"`

**Pengujian Ulang:**

Muat ulang halaman dan pastikan logo tampil dengan benar. Periksa kembali tab Network untuk memastikan file logo berhasil dimuat.

**Commit:**
- `dfd15f9` – fix: perbaiki path logo
- `96a5ad2` – fix: perbaiki path logo

---

## G2 – Layout Galeri Tidak Tersusun dengan Benar

**Gejala:**

Kartu karya pada bagian galeri tidak tersusun sesuai tata letak grid yang diharapkan.

**Langkah Reproduksi:**
1. Buka halaman Katalog Karya.
2. Gulir menuju bagian galeri.
3. Amati susunan kartu karya.
4. Periksa apakah kartu tampil dalam beberapa kolom sesuai ukuran layar.

**Hipotesis Penyebab:**
- Elemen pembungkus galeri belum menggunakan kelas Bootstrap `row`.
- Kelas kolom pada elemen kartu tidak diterapkan dengan benar.
- Struktur HTML antara elemen baris dan kolom tidak sesuai.

**Hasil Analisis:**

Elemen pembungkus galeri perlu menggunakan kelas `row` agar sistem grid Bootstrap dapat bekerja dengan baik.

**Bukti DevTools:**
- Buka tab Elements.
- Pilih elemen dengan ID `galeri`.
- Periksa class pada elemen pembungkus dan elemen kartu.
- Periksa tampilan pada ukuran layar laptop dan mobile.

**Perbaikan:**

Mengubah elemen galeri pada `index.html` agar menggunakan kelas Bootstrap:

`<div id="galeri" class="row g-3"></div>`

**Pengujian Ulang:**

Muat ulang halaman dan periksa susunan kartu pada layar laptop dan mobile. Pastikan kartu tersusun rapi dan tidak saling bertumpuk.

**Commit:**
- `43959cf` – fix: perbaiki layout galeri
- `4193537` – fix: layout

---

## G3 – Filter Kategori Tidak Berfungsi

**Gejala:**

Pilihan kategori pada menu filter tidak dapat digunakan untuk menyaring karya berdasarkan kategori yang dipilih.

**Langkah Reproduksi:**
1. Buka halaman Katalog Karya.
2. Pilih salah satu kategori pada menu filter.
3. Amati perubahan daftar karya.
4. Periksa apakah karya yang ditampilkan sesuai dengan kategori pilihan.

**Hipotesis Penyebab:**
- ID elemen filter pada HTML tidak sama dengan selector yang digunakan JavaScript.
- Event listener tidak berhasil dipasang.
- Fungsi penyaringan kategori tidak berjalan dengan benar.

**Hasil Analisis:**

Ditemukan ketidaksesuaian antara ID elemen pada HTML, yaitu `filterKategori`, dengan selector JavaScript yang menggunakan `#filter-kategori`. Ketidaksesuaian ini menyebabkan elemen tidak ditemukan oleh JavaScript.

**Bukti DevTools:**
- Buka tab Console.
- Periksa apakah terdapat error JavaScript.
- Periksa elemen filter pada tab Elements.
- Pastikan selector JavaScript sesuai dengan ID elemen HTML.

**Perbaikan:**

Menyesuaikan selector pada `main.js` agar sama dengan ID HTML:

`document.querySelector('#filterKategori')`

**Pengujian Ulang:**

Pilih beberapa kategori secara bergantian dan pastikan galeri hanya menampilkan karya sesuai kategori yang dipilih.

**Commit:**
- `825c9da` – fix:filter kategori

---

## G4 – Jumlah Karya Tidak Sesuai Hasil Filter

**Gejala:**

Informasi jumlah karya yang ditampilkan tidak berubah meskipun pengguna melakukan pencarian atau penyaringan kategori.

**Langkah Reproduksi:**
1. Buka halaman Katalog Karya.
2. Catat jumlah karya yang ditampilkan.
3. Gunakan fitur pencarian atau filter kategori.
4. Perhatikan informasi jumlah karya.
5. Bandingkan jumlah informasi dengan jumlah kartu yang terlihat.

**Hipotesis Penyebab:**
- Jumlah karya dihitung menggunakan total data awal.
- Fungsi penghitung tidak menggunakan hasil penyaringan.
- Variabel hasil filter tidak diterapkan pada informasi jumlah karya.

**Hasil Analisis:**

Informasi jumlah karya menggunakan `KARYA.length`, sehingga jumlah yang ditampilkan selalu mengacu pada seluruh data awal, bukan jumlah data setelah proses penyaringan.

**Bukti DevTools:**
- Periksa fungsi `tampil()` pada `main.js`.
- Periksa bagian kode yang mengatur informasi jumlah karya.
- Bandingkan nilai jumlah karya dengan jumlah kartu yang muncul setelah filter.

**Perbaikan:**

Mengubah perhitungan jumlah karya agar menggunakan panjang array hasil penyaringan, yaitu `hasil.length`, bukan `KARYA.length`.

**Pengujian Ulang:**

Lakukan pencarian dan filter kategori. Pastikan informasi jumlah karya berubah sesuai jumlah kartu yang ditampilkan.

**Commit:**
- `5e96ada` – fix: fungsi tampil

---

## G5 – Tombol Detail Tidak Berfungsi Setelah Galeri Dirender Ulang

**Gejala:**

Tombol Detail tidak memberikan respons ketika diklik setelah pengguna melakukan pencarian atau filter pada galeri.

**Langkah Reproduksi:**
1. Buka halaman Katalog Karya.
2. Klik salah satu tombol Detail.
3. Tutup tampilan detail.
4. Lakukan pencarian atau filter kategori.
5. Klik tombol Detail pada kartu hasil penyaringan.
6. Amati apakah detail karya dapat dibuka.

**Hipotesis Penyebab:**
- Event listener hanya dipasang satu kali saat halaman pertama kali dimuat.
- Elemen tombol Detail dibuat ulang ketika fungsi render dijalankan.
- Tombol yang baru dibuat tidak memiliki event listener.

**Hasil Analisis:**

Fungsi `pasangDetail()` sebelumnya hanya dijalankan satu kali. Ketika galeri dirender ulang, tombol Detail yang baru dibuat tidak memperoleh event listener.

**Bukti DevTools:**
- Periksa fungsi `tampil()` dan `pasangDetail()` pada `main.js`.
- Amati perubahan elemen tombol Detail pada tab Elements setelah filter digunakan.
- Periksa tab Console ketika tombol diklik.
- Dokumentasikan perilaku tombol sebelum dan sesudah perbaikan.

**Perbaikan:**

Memperbaiki mekanisme pemasangan event listener agar tombol Detail tetap dapat digunakan setelah galeri diperbarui. Perbaikan dilakukan dengan menerapkan event delegation atau memasang kembali event listener setelah proses render.

**Pengujian Ulang:**

Klik tombol Detail sebelum dan sesudah pencarian maupun filter kategori. Pastikan detail karya tetap dapat dibuka.

**Commit:**
- `840bc18` – fix: g5 detail

---

## G6 – Pencarian Membedakan Huruf Besar dan Kecil

**Gejala:**

Hasil pencarian berbeda ketika pengguna memasukkan kata kunci dengan huruf besar atau huruf kecil.

**Langkah Reproduksi:**
1. Buka halaman Katalog Karya.
2. Masukkan kata kunci menggunakan huruf kecil.
3. Catat hasil pencarian.
4. Masukkan kata kunci yang sama menggunakan huruf kapital.
5. Bandingkan hasil pencarian.

**Hipotesis Penyebab:**
- Proses pencarian menggunakan perbandingan teks yang bersifat case-sensitive.
- Kata kunci dan judul karya tidak dinormalisasi sebelum dibandingkan.

**Hasil Analisis:**

Penggunaan `includes()` secara langsung pada teks yang belum dinormalisasi menyebabkan pencarian membedakan huruf kapital dan huruf kecil.

**Bukti DevTools:**
- Periksa fungsi pencarian pada `main.js`.
- Periksa nilai kata kunci dan judul karya yang digunakan dalam proses pencarian.
- Bandingkan hasil pencarian menggunakan huruf kapital dan huruf kecil.

**Perbaikan:**

Mengubah proses pencarian agar kata kunci dan judul karya diubah menjadi huruf kecil sebelum dibandingkan, misalnya menggunakan `toLowerCase()`.

**Pengujian Ulang:**

Masukkan kata kunci yang sama dengan variasi huruf kapital dan huruf kecil. Pastikan keduanya menghasilkan daftar karya yang sama.

**Commit:**
- `653058b` – fix g6 filter kategori

---

## Kesimpulan

Berdasarkan proses debugging, ditemukan enam gejala pada proyek Katalog Karya yang berkaitan dengan pemanggilan aset gambar, penerapan layout Bootstrap, selector JavaScript, perhitungan jumlah data, pemasangan event listener, dan proses pencarian.

Setiap gejala dianalisis melalui pemeriksaan kode dan pengujian pada browser. Perbaikan kemudian disimpan menggunakan Git agar perubahan dapat ditelusuri melalui riwayat commit.

Catatan: Bukti DevTools dan hasil pengujian ulang dilengkapi menggunakan screenshot aktual dari proses pengujian.