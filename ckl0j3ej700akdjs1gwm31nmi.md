## Mengenal LINE Frontend  Framework ( LIFF ) Part 3

Di part 1 dan part 2 kita sudah berhasil membuat akun LINE Developer Console, membuat provider dan membuat LINE Login Channel. Lalu kapan kita buat aplikasi LIFF nya? Nah, sekarang kita akan buat  aplikasi LIFF nya. Untuk kalian yang tidak mengikuti atau belum membaca part-part sebelumnya, kalian membacanya terlebih dahulu bisa klik [disini](https://juliansyahrifqi.hashnode.dev/mengenal-line-frontend-framework-liff-part-1?guid=7d8bdcd5-d4af-49a8-bc2d-66cc98a414f8&deviceId=ff62e3ef-a185-4f8c-85fe-877ceabe0020) untuk part 1 dan klik  [disini](https://juliansyahrifqi.hashnode.dev/mengenal-line-frontend-framework-liff-part-2) untuk part 2. 

*** 
## LIFF API dan Client API

Oke, sebelum kita memulai pembuatan aplikasi LIFF alangkah baiknya kita ketahui dulu tentang **LIFF API** dan **Client API**. Jadi untuk membuat aplikasi LIFF kita memanfaatkan yang namanya LIFF API, nah LIFF API ini dapat kita akses dengan yang namanya Client API. Client API ini berisi method-method yang dapat kita gunakan pada LINE LIFF. Method-method yang ada dalam Client API adalah:

1. **``liff.sendMessages()``**, yang fungsinya untuk mengirimkan pesan atas nama pengguna ke dalam ruang obrolan dimana aplikasi LIFF dijalankan. Tipe pesan yang dapat dikirimkan itu dapat berupa teks, foto, video, lokasi, sticker, lokasi. 
2. **``liff.getProfile()``**, yang fungsinya mendapatkan informasi pengguna seperti id pengguna, nama pengguna, status, dan foto profil.
3. **``liff.openWindow()``**, yang fungsinya untuk membuka aplikasi LIFF di browser eksternal. 

Untuk informasi lengkap mengenai Client API ini kalian bisa membacanya [disini](https://developers.line.biz/en/reference/liff/#client-api) 
***
## LIFF SDK

Untuk inisialisasi aplikasi LIFF juga kita harus mengintegrasikan **LIFF SDK** ke dalam aplikasi LIFF yang kita buat. LIFF SDK ini dibangun dengan menggunakan bahasa Javascript. Apa sebenarnya LIFF SDK itu? 
 **LIFF SDK** adalah UI Framework yang fungsinya untuk membuka aplikasi LIFF ini di dalam aplikasi LINE sendiri tanpa harus menginstal web karena bisa langsung di akses hanya melalui URL di dalam LINE. Dari URL tersebut jika kita buka, maka Client API yang terintegrasi dengan LIFF SDK ini akan membuka aplikasi kita tanpa menutup aplikasi LINE. 

Nah, untuk mengintegrasikan LIFF SDK ini caranya cukup mudah, kita cukup menambahkan URL atau CDN path LIFF SDK ke dalam berkas file html kita

```
<script charset="utf-8" src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
```

atau kita dapat menggunakan npm package
```
$ npm install --save @line/liff
```

ataupun dengan menggunakan yarn
```
$ yarn add @line/liff
```

> ** Tetapi perlu diperhatikan bahwa npm package masih dalam tahap percobaan, jadi bisa saja terdapat perubahan atau mungkin dihapus di kemudian hari. Jadi, saya sarankan agar menggunakan CDN saja agar lebih pasti **.

***

## Fitur Aplikasi Todo List
Langsung saja kita masuk ke tahap-tahap pembuatan aplikasinya. Aplikasi yang akan kita buat ini adalah aplikasi web todo list sederhana. Untuk pembuatan aplikasi todo list ini kita hanya akan menggunakan HTML, CSS dan Vanilla Javascript ( Tanpa framework ). Di dalam aplikasi todo list ini nantinya akan memiliki fitur-fitur seperti, diantaranya: 
- Menambahkan todo 
- Menghapus todo
- Memberi tanda selesai pada todo
- Melihat todo
- Implementasi LIFF ( Login, logout, mengirim pesan, membuka di browser eksternal )

Nanti, jika aplikasinya sudah jadi maka tampilannya akan seperti dibawah ini.

**Aplikasi Line:**

![todo-app.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1613029034107/ShnL4ibat.png)

**Buka di Browser Eksternal:**

![todo-app-eksternal.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1613028999995/xjyT5xcgb.png)

**Kirim Pesan:**

![kirim-pesan.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1613028610807/QGS6m9CtG.png)
***

## Pembuatan Aplikasi LIFF Todo List

### Konfigurasi LIFF di LINE Developers

Langkah pertama untuk melakukan konfigurasi LIFF adalah dengan menambahkan aplikasi LIFF kita, caranya adalah masuk ke channel yang sebelumnya sudah dibuat. Pilih tab **LIFF** lalu klik tombol **Add**.

![konfigurasi-liff-1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612959892623/Nk7-Rk-OI.png)

Setelah itu, isi kolom **LIFF app name**, **Size**, **Endpoint URL**, **Scope**, **Bot Link Feature **dan **Options**.


![konfigurasi-liff-2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612960394636/jmYL2tAni.png)

- **LIFF app name ** : nama projek atau aplikasi LIFF kita
- **Size**: ukuran view aplikasi LIFF.
- **Endpoint URL**: alamat web dari aplikasi kita, sementara kita isi dengan **``https://contoh.heroku.app.com``** terlebih dahulu, link tersebut akan kita ganti setelah kita menghosting web kita atau saat web kita sudah online.
- **Profile**: mendapatkan informasi profil pengguna.
- **Openid**: diperlukan ketika kita ingin mendapatkan informasi mengenai token ID
- **chat_message.write**: mengizinkan pengguna untuk mengirim pesan.
- **Bot link feature On ( Normal )**: menampilkan permission access untuk menambahkan Line Official Account yang kita buat tanpa membuka halaman baru
- **On ( Aggressive ) **: menampikan layar konfirmasi jika pengguna ingin menambahkan LINE Official Account sebagai teman dengan membuka halaman baru.
- **Off**: tidak menampilkan layar konfirmasi ( permission access ) ketika menambahkan LINE Official Account sebagai teman.
- **Options Scan QR**: kita bisa menambahkan pengaturan ini ketika aplikasi kita ingin atau butuh untuk membaca QR Code
-  **Module mode**: ketika mode ini aktif, maka tombol share dalam aplikasi LIFF akan disembunyikan.

Setelah semua kolom sudah diisi, klik **Add**. Jika berhasil, kalian akan diberikan informasi mengenai LIFF ID dan LIFF URL untuk aplikasi yang akan dibuat.


![liff-detail.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612972215999/v3dTcRxlS.png)

***

### Langkah Pembuatan Aplikasi Todo LIFF
1. Buat direktori atau folder dengan nama yang bebas sesuai pilihan kalian. Disini nama folder saya adalah **todo-app-liff**
```
$ mkdir todo-app-liff
$ cd todo-app-liff
```
2. Buat folder dengan nama css, folder js dan file index.html di dalam direktori atau folder yang sudah dibuat tadi
```
$ mkdir css js
$ touch index.html
```
3. Buka direktori atau folder **todo-app-liff** tadi kedalam teks editor kalian. Disini saya menggunakan Visual Studio Code.
```
$ code .
``` 
4. Buka file **index.html** yang tadi sudah dibuat dan tambahkan script atau kode dibawah ini ke dalamnya.
%[https://gist.github.com/juliansyahrifqi/0a1a5097a8aa2d13bd0cc25e7399e2f6]
5. Selanjutnya kita buat script javascript dengan nama **script.js ** atau apapun di dalam folder **js ** yang sudah dibuat.
%[https://gist.github.com/juliansyahrifqi/d3d0dc554518d8b3dd850f19d67d43e0]
6. Buat file CSS dengan nama **style.css** pada folder** css**. Kemudian masukkan kode css di bawah ini ke dalam file **style.css** tersebut
%[https://gist.github.com/juliansyahrifqi/0b524e8adf81fc2d24e36b7aa8c7f1ef]
Sampai disini, kita sudah berhasil membuat aplikasi todo listnya, kita sudah bisa menjalankannya di browser. Apabila aplikasi todo list kita berjalan dengan benar maka tampilan dari aplikasinya akan seperti ini:
![aplikasi-todo.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612543027348/VYOlBUgwe.png)

Selanjutnya adalah langkah implementasi fitur-fitur LIFF pada aplikasi todo list kita.

***

### Implementasi Fitur LIFF

Langkah-langkah implementasi fitur LIFF ke dalam aplikasi todo list adalah sebagai berikut:
1. Integrasikan aplikasi kita dengan **LIFF SDK** dengan cara menambahkan kode atau script dibawah ini di file **index.html** dan tempatkan di bagian bawah **<body> **setelah pemanggilan **script.js
**
```
<script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
```
2. Buat file javascript baru di dalam folder **js** dengan nama **liff-starter.js**, tambahkan script atau kode dibawah ini ke dalam file **liff-starter.js** kita.

%[https://gist.github.com/juliansyahrifqi/17097f5b984e49f960dd78cc431a06e1]
Jangan lupa kita panggil script **liff-starter.js** kita ke dalam file **index.html**. Sisipkan kode atau script dibawah ini ke dalam file **index.html** setelah pemanggilan script** LIFF SDK**.
```
<script defer src="js/liff-starter.js"></script>
```

Pada bagian script ini, pastikan bahwa variabel defaultLiffId diisi dengan **LIFF ID** yang sudah didapatkan sebelumnya.
```
const defaultLiffId = "";   // isi dengan liff id kalian
 ```

Sampai langkah ini, seharusnya aplikasi Todo List kita sudah terintegrasi dengan LIFF, untuk penjelasan kode diatas dan cara hosting aplikasi todo list kita akan dibahas di part selanjutnya. Dan jangan lupa, jika ada masukkan atau saran bisa melalui kolom komentar. **See you next time!**

***
Untuk lebih lengkapnya mengenai **liff-starter** kalian bisa lihat di repositori milik LINE di link  [https://github.com/line/line-liff-v2-starter](https://github.com/line/line-liff-v2-starter) 









 





