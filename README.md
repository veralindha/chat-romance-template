# Apa ini?

> Referensi dari https://github.com/wongjohn/for-my-love dengan beberapa modifikasi
>

Ungkapkan apa yang ingin Anda katakan kepada pacar Anda dalam bentuk interaksi obrolan, dukung konfigurasi konten yang nyaman, ganti dengan cepat dengan konten yang Anda inginkan, klik tautan di bawah untuk melihat pratinjau efeknya.

- Akses pengguna domestik: [https://sg.chenjianhui.site/girlfriend-gift-collection/chat-want-to-say/](https://sg.chenjianhui.site/girlfriend-gift-collection/chat- ingin bilang/)
- Halaman Github: [https://calebman.github.io/girlfriend-gift-collection/chat-want-to-say/](https://calebman.github.io/girlfriend-gift-collection/chat-want -untuk-mengatakan/)
# Cara memodifikasi ke konten Anda sendiri

### 1. Ubah file konfigurasi

> .env.* di direktori root proyek chat-want-to-say adalah file konfigurasi lingkungan, yang dikemas dengan konfigurasi .env.production secara default, dan konten konfigurasinya adalah sebagai berikut:
>

| Item konfigurasi | Arti | Keterangan |
| ---------------------- | -------------------- | ----- -------------------------------------------------- |
| VITE_APP_TITLE | Judul kotak obrolan | Dapat diisi konten seperti "Mengobrol dengan XXX" |
| VITE_BASE_URL | Jalur basis halaman web | Jika digunakan di jalur sekunder server, itu harus diisi |
| VITE_CHAT_OPTIONS_PATH | Konfigurasi konten obrolan (penting) | Isi alamat file json yang sesuai dengan konten, dan temukan file tersebut di direktori publik |

Mengacu pada .env.development file konfigurasi lengkap terlihat seperti ini:

``` properti
VITE_APP_TITLE=Mengobrol dengan XXX
VITE_BASE_URL=obrolan-interaktif-demo/
VITE_CHAT_OPTIONS_PATH=opsi/demo/chat.json
```

### 2. Tingkatkan konten obrolan

Konfigurasi konten obrolan terdiri dari larik json, setiap item dalam larik sesuai dengan konten obrolan, dan informasi konfigurasi yang didukung adalah sebagai berikut:

| Item konfigurasi | Arti | Jenis | Keterangan |
| ---------------------- | -------------------- | ----- -------------------------------------------------- | -------------------------------------------------- - ---- |
| pesan | Konten obrolan | Larik | Mendukung **teks/gambar/huruf/VLog** dan bentuk konten lainnya. Dalam kasus teks, beberapa konten dapat diteruskan ke dalam larik untuk membentuk efek mundur. |
| msgInputSpeed ​​\u200b\u200b| kecepatan input teks | angka | Standarnya adalah 150, ketika isinya adalah teks, itu mewakili larik input, dan satuannya adalah ms. |
| triggerNextAction | Konfigurasi perilaku untuk memicu obrolan berikutnya | Objek | Standarnya adalah kosong. Ketika ada konten, itu akan dijeda dan menunggu interaksi pengguna untuk memicu konten obrolan berikutnya. |
| penulis | Pemilik konten obrolan | String | Standarnya kosong. Saat Anda perlu mensimulasikan bahwa pihak lain memiliki input konten, Anda dapat mengisi "saya" |

Informasi konfigurasi triggerNextAction yang dipicu oleh interaksi pengguna adalah sebagai berikut:

| Item konfigurasi | Arti | Jenis | Keterangan |
| ---------------------- | -------------------- | ----- -------------------------------------------------- | -------------------------------------------------- - ---- |
| tipe | Jenis perilaku pemicu | String | Saat ini, dua jenis nilai didukung untuk komponen konten yang berbeda, userInput/componentClose. Silakan lihat tabel di bawah ini untuk penjelasan spesifik |
| opsi | konfigurasi perilaku pemicu | objek | | konfigurasi memiliki arti yang berbeda tergantung pada jenisnya, saat ini hanya userInput yang mendukung konfigurasi opsi, lihat di bawah tabel untuk penjelasan khusus

> triggerNextAction.type dijelaskan
> - userInput: mewakili menunggu pengguna memasukkan konten tertentu untuk menyelesaikan putaran interaksi, dan mendukung konfigurasi kata kunci
> - componentClose: Artinya menunggu pengguna menutup komponen untuk menyelesaikan putaran interaksi. Ini sering digunakan dalam komponen seperti huruf/VLog. Pengguna mengklik tutup untuk kembali ke halaman obrolan guna memicu pemutaran konten selanjutnya
>

> penjelasan triggerNextAction.options, saat triggerNextAction.type = userInput, konfigurasi valid
> - resolveKeyTexts: Jenisnya adalah larik, dan kata kunci yang memicu perilaku selanjutnya hanya perlu disertakan, seperti ["OK", "Ya", "Setuju"], selama pengguna memasukkan seperti "OK ", itu akan memicu perilaku selanjutnya
> - rejectKeyTexts: Jenisnya adalah array, kata kunci daftar hitam yang memicu perilaku berikutnya, hanya perlu disertakan, prioritasnya tinggi, seperti ["Saya tidak mau"], pengguna sudah terjebak dalam ini mengobrol selama dia memasukkan "Saya tidak mau" di konten
> - rejectHitTexts: Jenisnya adalah array, konten prompt interaktif, ketika input pengguna tidak memenuhi harapan, prompt akan diberikan, prompt akan mengikuti urutan array, jika prompt tidak cukup, konten obrolan berikutnya akan dipicu secara paksa
>

Berikut adalah beberapa deskripsi dari berbagai bentuk konten

#### 1. Teks biasa

- Berikut ini adalah konten teks dengan kecepatan input 80ms

``` json
{ "msgs": ["Pesan normal dengan animasi masukan"], "msgInputSpeed": 80 }
```

- Berikut ini adalah isi teks dengan fallback effect

``` json
{
     "msgs": ["Pesan normal akan dibatalkan", "Hasil dari pengembalian pesan normal juga dapat menyesuaikan kecepatan animasi"],
     "msgInputSpeed": 120
}
```

- Berikut ini adalah isi teks dengan efek trigger

``` json
{
     "msgs": ["Saya memiliki sesuatu di hati saya, apakah Anda ingin mendengarnya?"],
     "msgInputSpeed": 40,
     "triggerNextAction": {
         "ketik": "Input pengguna",
         "pilihan": {
             "resolveKeyTexts": ["OK", "Ya", "Setuju", "Akan", "Ingin", "Ingin mendengar", "Ya" ],
             "rejectKeyTexts": ["tidak mau"],
             "rejectHitTexts": [
                 "Tidak mendengarkan? Percaya atau tidak, saya akan menunjukkan bug setiap menit?",
                 "Yah, karena kamu benar-benar tidak ingin mendengarnya, aku akan memberitahumu ~"
             ]
         }
     }
}
```

- Berikut ini adalah konten dari warna teks kustom

``` json
{
     "msgs": ["<span style=\"color: red;\">font tag rentang merah</span>"],
     "msgInputSpeed": 80
}
```

#### 2. Gambar

Contoh konten gambar ditunjukkan di bawah ini File gambar ditempatkan di direktori publik dan direferensikan menggunakan jalur relatif

``` json
{ "pesan": ["<img src='options/demo/imgs/test.png'/>"] }
```

#### 3. Surat

Contoh isi surat ditunjukkan di bawah ini. File isi surat ditempatkan di direktori publik dan direferensikan menggunakan jalur relatif. Saat surat ditutup oleh pengguna, konten obrolan berikutnya dipicu

``` json
{
     "pesan": ["<letter src='options/demo/letters/test.json'/>"],
     "triggerNextAction": { "type": "componentClose" }
}
```

Item konfigurasi surat adalah sebagai berikut:

| Item konfigurasi | Arti | Jenis | Keterangan |
| ---------------------- | -------------------- | ----- -------------------------------------------------- | -------------------------------------------------- - ---- |
| openTip | tip pembuka pesan | string | |
| judul | judul pesan | string | |
| kecepatan | kecepatan input teks | angka | kecepatan input teks huruf |
| paragraf | informasi paragraf surat | array | setiap item dalam array adalah string, mendukung teks/gambar, penjelasan spesifiknya adalah sebagai berikut |

Misalnya, arti dari paragraf konfigurasi huruf berikut adalah, setelah input "beberapa hujan prem" selesai, jeda selama 1000ms, lalu masukkan "beberapa volume angin teratai"..., dan tempatkan gambar setelah paragraf selesai.

``` json
{
     "openTip": "Kekasih",
     "title": "Surat untuk xx",
     "kecepatan": 10,
     "paragraf": [
         "Beberapa hujan prem^1000, beberapa volume angin teratai^1000, bagian selatan Sungai Yangtze sudah kabur.",
         "<img src='options/demo/imgs/test.png'/>",
         "Beberapa hujan prem^1000, beberapa volume angin teratai^1000, bagian selatan Sungai Yangtze sudah kabur.",
     ]
}

```

#### 4. VLog

Contoh konten VLog ditampilkan di bawah. File video ditempatkan di direktori publik dan direferensikan menggunakan jalur relatif. Saat video ditutup oleh pengguna, konten obrolan berikutnya dipicu.

``` json
{
     "pesan": ["<vlog src='options/demo/vlogs/test.mp4'/>"],
     "triggerNextAction": { "type": "componentClose" }
}
```

### 3. Debug & kompilasi jaringan statis