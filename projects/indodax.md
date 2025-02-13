# Dokumentasi Fungsi API Indodax dalam Google Apps Script 

Skrip ini menyediakan fungsionalitas untuk berinteraksi dengan platform trading Indodax menggunakan API-nya. Berikut penjelasan detail tentang fungsi `indodax` dan contoh penggunaannya untuk berbagai tindakan.

`indodax(action, type, pair, amount, price, market) { ... }`

1. Menentukan operasi yang akan dilakukan:
   `('getBalance', 'order', 'cancel')`

3. Type menentukan jenis order atau aset:
   `('buy', 'sell')`

3. Pair pasangan trading `('btc_idr', 'eth_idr')`
   
5. Aamount Jumlah untuk order (opsional untuk beberapa tindakan, type `Number`):
   
    `100000`
7. Price untuk order (hanya digunakan untuk limit order):
   `market` (Boolean): Menunjukkan apakah order adalah market order (true) atau limit order (false).

8. Perbarui variabel berikut dengan kredensial API Anda:
   `apiKey = 'KUNCI_API_ANDA'; const secretKey = 'KUNCI_RAHASIA_ANDA';`
---
## Tindakan dan Contoh

1. Mendapatkan Saldo:
   
   - **Action:**: `indodax('getBalance')`
   - **Respons:** Array objek berisi saldo tidak nol: `[  { symbol: 'btc', amount: 0.01 },  { symbol: 'eth', amount: 0.5 }]`
---
2. Membuat Order
   - **Action:** `order`
   - **Parameter Wajib:** `type, pair, amount, price, market`
   -  Market Order:  `indodax('order', 'buy', 'btc_idr', 100000, '', true); `
   -  Limit Order:  `indodax('order', 'sell', 'eth_idr', 0.01, 7500000, false);`
---
3. Membatalkan Order
   - **Action:** `cancel` **Parameter Wajib:** `type` (ID order): `indodax('cancel', '80872690');`
---
4. Mendapatkan Riwayat Order:
   - **Action:** `orderHistory` **Parameter Wajib:** `type` (pasangan trading).
   - `indodax('orderHistory', 'btc_idr');`
---
5. Mendapatkan Riwayat Trading:
   - **Action:** `tradeHistory`- **Parameter Wajib:** `type` (pasangan trading), `pair` (ID order opsional).
   - `indodax('tradeHistory', 'btc_idr', '123456');`
---
6. Mendapatkan Order Aktif:
   - **Action:** `openOrders`- **Parameter Wajib:** `type` (pasangan trading)
   - - `indodax('openOrders', 'btc_idr');`
---
7. Mendapatkan Detail Order Tertentu:
   - **Action:** `getOrder`- **Parameter Wajib:** `type` (pasangan trading), `pair` (ID order)
   -  `indodax('getOrder', 'btc_idr', '123456');console.log(detailOrder);`
---
## Fungsi Pembantu
1. `request()`Menangani permintaan API dan memproses respons.
2. `hmac_sha512(payloadString)`Menghasilkan tanda tangan HMAC-SHA-512 yang diperlukan untuk autentikasi API.
3. `toQueryString(obj)`Mengkonversi objek menjadi string query untuk permintaan API.
---
## Catatan
1. Ganti `KUNCI_API_ANDA` dan `KUNCI_RAHASIA_ANDA` dengan kredensial API Anda yang sebenarnya.
2. Pastikan kunci API memiliki izin yang sesuai untuk tindakan yang ingin Anda lakukan.
3. `requestBody` mencakup parameter default seperti `recvWindow` untuk mencegah kedaluwarsa permintaan.
   
Dengan menggunakan skrip ini, Anda dapat mengotomatisasi tugas trading dan pemantauan di platform Indodax secara efektif. Selalu tangani kunci API dengan aman dan hindari hardcoding di lingkungan produksi.
