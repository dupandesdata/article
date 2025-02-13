OCR (Optical Character Recognition) adalah teknologi yang digunakan untuk mengubah berbagai jenis dokumen, seperti dokumen kertas yang dipindai, foto dokumen, atau gambar teks, menjadi data yang dapat diedit dan dicari. Dengan menggunakan OCR, teks dalam gambar dapat dikenali dan diekstrak sehingga dapat disimpan dalam format digital. 

### Fungsi dan Manfaat OCR

1. **Pengolahan Dokumen**: Memudahkan pengolahan dokumen fisik menjadi format digital, seperti PDF atau dokumen Word.
2. **Pencarian Teks**: Memungkinkan pencarian teks dalam dokumen yang awalnya hanya dalam bentuk gambar.
3. **Automasi**: Mengurangi pekerjaan manual dalam memasukkan data dari dokumen fisik.
4. **Aksesibilitas**: Membantu membuat konten lebih mudah diakses oleh orang dengan kebutuhan khusus, seperti tunanetra.

### Contoh Penggunaan

- **Digitalisasi Buku**: Mengubah buku fisik menjadi format digital.
- **Pengolahan Formulir**: Menggunakan OCR untuk mengekstrak data dari formulir yang diisi tangan.
- **Pindai Kartu Identitas**: Mengenali dan mengekstrak informasi dari kartu identitas atau dokumen resmi lainnya.

### Cara Kerja OCR

1. **Pindai Gambar**: Gambar dokumen dipindai atau diambil menggunakan kamera.
2. **Pra-pemrosesan**: Gambar diolah untuk meningkatkan kualitas, seperti penghapusan noise atau penyesuaian kontras.
3. **Pengenalan Karakter**: Algoritma OCR menganalisis gambar untuk mengenali karakter dan teks.
4. **Post-pemrosesan**: Teks yang dikenali diperiksa dan diperbaiki untuk kesalahan pengenalan.

Berikut adalah penjelasan mengenai logika skrip dalam kode:

### Logika Skrip

1. **Inisialisasi Vue.js**:
   ```javascript
   new Vue({
       el: '#app',
       data: {
           imageUrl: null,
           croppedImageUrl: null,
           convertedText: '',
           isConverting: false,
           isCropping: false,
           cropper: null
       },
       methods: { ... }
   });
   ```
   - **`el`**: Menentukan elemen HTML yang akan dikelola oleh Vue, yaitu `#app`.
   - **`data`**: Menyimpan data yang digunakan dalam aplikasi:
     - `imageUrl`: URL gambar yang diunggah.
     - `croppedImageUrl`: URL gambar yang dipotong.
     - `convertedText`: Teks yang dihasilkan dari OCR.
     - `isConverting`: Status untuk menunjukkan apakah proses konversi sedang berlangsung.
     - `isCropping`: Status untuk menunjukkan apakah pemotongan gambar aktif.
     - `cropper`: Instance dari Cropper.js.

2. **Metode**:
   - **`handleImageUpload(event)`**: 
     - Mengambil file gambar yang diunggah.
     - Membaca file menggunakan `FileReader` dan mengatur `imageUrl` dengan hasil pembacaan.
     - Menghapus instance cropper jika ada dan mengatur status pemotongan ke `false`.

   - **`initCropper()`**:
     - Menginisialisasi Cropper.js jika belum ada instance (`this.cropper`).
     - Menggunakan `this.$nextTick()` untuk memastikan DOM diperbarui sebelum membuat cropper.
     - Mengatur status pemotongan ke `true`.

   - **`cropImage()`**:
     - Menggunakan cropper untuk mendapatkan URL gambar yang dipotong dan menyimpannya di `croppedImageUrl`.
     - Menghentikan pemotongan dan menghancurkan instance cropper.

   - **`cancelCrop()`**:
     - Menghancurkan instance cropper jika ada dan mengatur status pemotongan dan gambar yang dipotong ke `null`.

   - **`convertToText()`**:
     - Memeriksa apakah ada gambar untuk dikonversi (gambarnya bisa hasil potongan atau gambar asli).
     - Mengatur `isConverting` ke `true` dan mengosongkan `convertedText`.
     - Menggunakan Tesseract.js untuk mengenali teks dari gambar.
     - Menangani hasil konversi, jika berhasil akan mengupdate `convertedText`, jika gagal akan menampilkan pesan kesalahan.
     - Mengatur `isConverting` ke `false` setelah proses selesai.

### Interaksi Pengguna

- **Mengunggah Gambar**: Pengguna memilih gambar menggunakan input file. Setelah diunggah, gambar ditampilkan dan siap untuk dipotong.
  
- **Memotong Gambar**: Pengguna mengklik gambar untuk memulai proses pemotongan. Setelah itu, mereka dapat memilih untuk memotong gambar atau membatalkan pemotongan.

- **Konversi Gambar Menjadi Teks**: Setelah gambar dipotong (atau jika pengguna memilih gambar asli), pengguna dapat mengklik tombol untuk mengonversi gambar menjadi teks. Status tombol berubah berdasarkan proses konversi.

### Ringkasan

Skrip ini menggunakan Vue.js untuk mengelola keadaan aplikasi dan interaksi pengguna. Tesseract.js digunakan untuk proses OCR, sedangkan Cropper.js digunakan untuk memberikan fitur pemotongan gambar. Aplikasi ini memungkinkan pengguna untuk mengunggah, memotong, dan mengonversi gambar menjadi teks dengan antarmuka yang responsif dan interaktif.
