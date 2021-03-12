## Mengenal LINE Frontend Framework ( LIFF ) Part 1

LINE yang kita kenal sebagai aplikasi atau platform untuk chatting, video call, voice call dan banyak lagi ternyata mempunyai teknologi lain loh. Yaitu LIFF atau LINE Frontend Framework.

### Apa itu LINE Frontend Framework ( LIFF ) ?

LIFF atau LINE Frontend Framework adalah sebuah *framework frontend *yang memungkinkan sebuah aplikasi web dapat berjalan di dalam aplikasi LINE. Aplikasi LIFF juga memungkinkan aplikasi web kita dapat mendapatkan data dari platform LINE seperti id pengguna yang dapat digunakan oleh aplikasi web kita untuk mengirim pesan atas nama pengguna.

**Hah? maksudnya gimana sih aplikasi web tapi bisa jalan di dalam aplikasi LINE? **

Mungkin biar lebih mudah dimengerti, kita bisa lihat ilustrasi gambar dibawah ini:

![Contoh Penggunaan LIFF](https://cdn.hashnode.com/res/hashnode/image/upload/v1609821048365/_AreqDvQx.png)

Nah gimana, apakah udah kebayang?

Oh iya, aplikasi LIFF juga memiliki 3 jenis ukuran tampilan layar yang berbeda loh teman-teman, diantaranya:

1. Full: Aplikasi LIFF yang ditampilkan sebesar 100% dari ukuran layar
2. Tall: Aplikasi LIFF yang ditampilkan sebesar 75% dari ukuran layar
3. Compact: Aplikasi LIFF yang ditampilkan sebesar 50% dari ukuran layar

![Jenis Ukuran Layar LIFF](https://cdn.hashnode.com/res/hashnode/image/upload/v1609822225508/DX0G7TEp4.png)

### Cara Membuat Aplikasi LIFF

Sebelum kita membuat aplikasi LIFF, kita harus mempunyai akun LINE terlebih dahulu. Jika kalian belum mempunyai akun LINE, silahkan lakukan pendaftaran terlebih dahulu dan jangan lupa sekalian melakukan verifikasi email akun kalian ya!.

Jika kalian sudah selesai melakukan pendaftaran akun atau sudah punya akun LINE yang emailnya sudah terverifikasi, langkah pertama yang harus dilakukan adalah dengan mendaftar pada **Line Developers Console**.

Masuk ke halaman  [https://developers.line.biz/en/](https://developers.line.biz/en/) untuk masuk ke halaman awal **Line Developers Console**

![Line Developers Console Website](https://cdn.hashnode.com/res/hashnode/image/upload/v1609774077549/TEqVTC8QU.png)

Klik pada bagian **Login** yang ada pada pojok kanan atas bagian website.  Setelah itu akan ada dua pilihan untuk login, pilih yang bagian **Login with LINE Account** lalu masukkan **email **dan **password** akun LINE kalian

![Login with LINE Account](https://cdn.hashnode.com/res/hashnode/image/upload/v1609774756612/817Rn_RBo.png)

Kalau pertama kali Login di komputer, LINE biasanya akan meminta kita untuk mengkonfirmasi identitas kita. Jangan khawatir caranya mudah, cukup masukkan empat digit kode yang dikirim pada aplikasi LINE di ponsel kita dan beres.

> **Note: ** LINE akan selalu meminta kita untuk melakukan verifikasi setiap kita melakukan login pada perangkat yang berbeda.

Setelah melakukan login, langkah selanjutnya adalah dengan membuat provider. Caranya klik tombol **Create** pada bagian **Providers**, lalu isi nama provider bisa dengan nama perusahaan, nama klien, nama diri, atau nama yang berkaitan dengan aplikasi yang akan kita buat nanti.

Untuk nama **provider** disini saya menggunakan nama saya sendiri.

![Create Provider](https://cdn.hashnode.com/res/hashnode/image/upload/v1610282956636/HzsrNKby5.png)

Setelah Provider berhasil dibuat maka akan tampil halaman website seperti dibawah ini.

![Halaman Providers](https://cdn.hashnode.com/res/hashnode/image/upload/v1610283566949/w6a12U6Od.png)

Setelah sampai pada tahap ini, artinya kita sudah berhasil membuat **Line Developers** dan **Providers**. Pada halaman tersebut ada beberapa buah pilihan channel, seperti:

- Line Login
- Line Messagin API
- Clova Skill
- Blockchain Service

Namun, kita tidak akan memakai semuanya, kita hanya akan menggunakan **Line Login Channel** untuk menggunakan fitur-fitur dari **LINE LIFF.**

> **Note: ** Perlu diketahui bahwa sebelumnya LIFF dapat menggunakan Line Messaging API tetapi mulai dari awal Februari 2020, LIFF sudah tidak bisa ditambahkan di Line Messaging API. Oleh karena itu LIFF saat ini hanya dapat ditambahkan pada Line Login Channel saja.

Untuk pembahasan mengenai **Line Login**, pembuatan **Line Login Channel**, dan pembuatan aplikasi **LINE LIFF** kita bahas di part selanjutnya dan jika ada masukkan atau pertanyaan bisa melalui kolom komentar. **See you next time!**



Untuk lebih jelas dan lebih lengkapnya, kalian bisa baca dokumentasi resmi di
**[https://developers.line.biz/en/docs/](https://developers.line.biz/en/docs/)**

