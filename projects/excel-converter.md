## 1. Gambaran Umum Aplikasi

Excel Converter adalah aplikasi web yang memungkinkan pengguna untuk mengkonversi file Excel ke berbagai format:
- JSON (JavaScript Object Notation)
- HTML Table (Tabel HTML)
- PDF (Portable Document Format)
- PNG (Portable Network Graphics)

## 2. Struktur Aplikasi

### 2.1 Dependensi Eksternal

<!-- Vue.js untuk framework frontend -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/3.3.4/vue.global.min.js"></script>

<!-- XLSX.js untuk membaca file Excel -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<!-- html2pdf.js untuk konversi ke PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

<!-- html2canvas untuk konversi ke PNG -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>

<!-- Tailwind CSS untuk styling -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.css" rel="stylesheet">

### 2.2 Komponen Utama UI
1. File Upload Area
2. Format Selection Buttons
3. Header/Footer Controls
4. Preview Area
5. Export Button
6. Code Display Area

## 3. Penjelasan Detail Kode

### 3.1 State Management (Vue.js Setup)
`
setup() {
    const tableData = ref([])          // Menyimpan data tabel dari Excel
    const tableHeaders = ref([])       // Menyimpan header tabel
    const selectedFormat = ref('JSON') // Format yang dipilih
    const showHeaderFooter = ref(false)// Toggle header/footer
    const header = ref('')            // Teks header
    const footer = ref('')            // Teks footer
    const previewArea = ref(null)     // Referensi area preview
    // ...
}
`

### 3.2 File Handling
`
const handleFile = (event) => {
    const file = event.target.files[0]
    const reader = new FileReader()

    reader.onload = (e) => {
        // Konversi file Excel ke array buffer
        const data = new Uint8Array(e.target.result)
        // Baca workbook Excel
        const workbook = XLSX.read(data, { type: 'array' })
        // Ambil sheet pertama
        const firstSheet = workbook.Sheets[workbook.SheetNames[0]]
        // Konversi ke JSON dengan header
        const jsonData = XLSX.utils.sheet_to_json(firstSheet, { header: 1 })
        
        // Pisahkan header dan data
        tableHeaders.value = jsonData[0]
        tableData.value = jsonData.slice(1)
    }

    reader.readAsArrayBuffer(file)
}
`

### 3.3 Export Functionality

#### 3.3.1 JSON Export
`
// Konversi data tabel ke format JSON
const jsonData = tableData.value.map(row => {
    const obj = {}
    tableHeaders.value.forEach((header, index) => {
        obj[header] = row[index]
    })
    return obj
})
return JSON.stringify(jsonData, null, 2)
`

#### 3.3.2 HTML Export
`
// Generate kode HTML untuk tabel
return `<table>
  <thead>
    <tr>
      ${tableHeaders.value.map(header => `<th>${header}</th>`).join('\n      ')}
    </tr>
  </thead>
  <tbody>
    ${tableData.value.map(row => `
    <tr>
      ${row.map(cell => `<td>${cell}</td>`).join('\n      ')}
    </tr>`).join('\n    ')}
  </tbody>
</table>`
`

#### 3.3.3 PDF Export
`
const pdfOpts = {
    margin: 1,
    filename: 'export.pdf',
    image: { type: 'jpeg', quality: 0.98 },
    html2canvas: { scale: 2 },
    jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
}
await html2pdf().from(previewArea.value).set(pdfOpts).save()
`

#### 3.3.4 PNG Export
`
const canvas = await html2canvas(previewArea.value)
const pngUrl = canvas.toDataURL('image/png')
const pngLink = document.createElement('a')
pngLink.href = pngUrl
pngLink.download = 'export.png'
pngLink.click()
`

## 4. Alur Kerja Aplikasi

1. **Upload File Excel**
   - User men-drag atau memilih file Excel
   - File dibaca menggunakan FileReader
   - Data dikonversi ke format JSON menggunakan XLSX.js

2. **Pemilihan Format**
   - User memilih format output yang diinginkan
   - UI memperbarui preview sesuai format yang dipilih

3. **Kustomisasi (untuk PDF/PNG)**
   - Opsional: User dapat menambahkan header/footer
   - Preview diperbarui secara real-time

4. **Export**
   - User menekan tombol export
   - File diproses sesuai format yang dipilih
   - File hasil konversi di-download ke perangkat user

## 5. Error Handling

- Validasi tipe file saat upload
- Pengecekan data kosong
- Handling untuk tabel besar
- Fallback untuk browser yang tidak mendukung fitur tertentu

## 6. Best Practices yang Diterapkan

1. **Performance**
   - Lazy loading untuk preview
   - Optimasi render menggunakan Vue's reactivity
   - Efficient DOM manipulation

2. **Accessibility**
   - Semantic HTML
   - ARIA labels
   - Keyboard navigation support

3. **Code Organization**
   - Modular function structure
   - Clear variable naming
   - Consistent code style

4. **Security**
   - Client-side processing
   - File type validation
   - Safe data handling

## 7. Penggunaan

1. Buka aplikasi di browser
2. Drag and drop file Excel atau klik area upload untuk memilih file
3. Pilih format output yang diinginkan (JSON, HTML, PDF, atau PNG)
4. Untuk PDF/PNG:
   - Toggle opsi header/footer jika diinginkan
   - Isi teks header/footer
5. Klik tombol Export untuk men-download file hasil konversi

## 8. Limitasi

- Maksimum ukuran file tergantung pada browser
- Format Excel yang didukung: .xlsx dan .xls
- Styling tabel terbatas pada format default
- PDF/PNG export tergantung pada tampilan preview
