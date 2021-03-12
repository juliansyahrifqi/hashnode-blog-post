## Mengenal LINE Frontend Framework ( LIFF ) Part 2

# Pembahasan LINE Login dan pembuatan LINE Login Channel.

Kalau sebelumnya kita melakukan pendaftaran LINE Developer Console dan membuat provider, pada part ini kita akan membahas tentang apa itu LINE Login dan cara pembuatan LINE Login Channel. Pastikan kalian sudah mengikuti dari part 1 ya agar pembahasan kita kali ini nyambung. Jika kalian belum membacanya, kalian bisa  [membacanya disini](https://juliansyahrifqi.hashnode.dev/mengenal-line-frontend-framework-liff-part-1) 

## Apa itu LINE Login?

**LINE Login** adalah sebuah API dari LINE yang memungkinkan seorang pengguna dapat dengan mudah melakukan login ke dalam sebuah aplikasi website ataupun mobile dengan menggunakan akun LINE yang sudah mereka punya. Selain itu, seorang pengguna juga dapat dengan mudah membuat akun untuk sebuah aplikasi web ataupun mobile hanya dengan menggunakan akun LINE yang sudah mereka punya.

Dan perlu diketahui, **LINE Login** ini tidak hanya berfungsi pada aplikasi mobile seperti Android dan iOS saja loh, tetapi juga dapat berfungsi pada aplikasi website dan game Unity.

Salah satu contoh penggunaan **LINE Login** ini adalah pada game Line Rangers, yang dimana kita dapat membuat akun untuk game tersebut dengan menggunakan akun LINE yang sudah kita punya. Atau contoh lain juga adalah game Line Let's Get Rich, bagi yang pernah memainkannya mungkin kita masih ingat, di dalam game tersebut kita bisa login dengan menggunakan akun LINE yang sudah kita punya.


![line-login-rangers.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612176388904/BfBqd2RlB.png)
Sumber:  [https://developers.line.biz/en/docs/line-login/overview/#native-app
](https://developers.line.biz/en/docs/line-login/overview/#native-app) 

Cukup kebayang kan dengan penjelasan diatas? Kalau sudah, kita masuk ke bagian berikutnya yaitu pembuatan LINE Login Channel. Kalau kalian masih belum kebayang coba dibaca lagi berkali-kali atau kalian bisa cari sumber-sumber lainnya.

## Pembuatan LINE Login Channel

Sebelum kita membuat **LINE Login Channel** ini, kita pahami dulu apa yang dimaksud dengan channel. Jadi kalau kita merujuk pada dokumentasi resmi dari LINE, channel itu adalah sebuah jembatan komunikasi yang menghubungkan antara Line Platform yang akan kita buat dengan provider yang sebelumnya sudah kita buat. Nah begitu kira-kira penjelasannya.

Langsung saja kita buat LINE Login Channelnya.


1. Pastikan kalian sudah login di LINE Developer Console dan sudah membuat provider. Kalau kalian belum membuatnya, kalian bisa membacanya [disini](https://juliansyahrifqi.hashnode.dev/mengenal-line-frontend-framework-liff-part-1) 

2. Masuk ke bagian Provider yang sudah dibuat sebelumnya, kemudian pilih pada bagian **Create a LINE Login Channel **. ![create-a-line-login-channel.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612178775287/02zXRX6Ql.png)

3. Pada bagian **Create a channel** ini kalian wajib mengisi kolom *Channel name*, *Channel description*, *App types* dan *Email address*. 
![create-a-channel.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612179284209/xLcSOkfc2.png) 
> **Untuk catatan**, sesuai anjuran pada dokumentasi resminya, jangan menyertakan kata "LINE" pada nama channel atau kolom *Channel name*. 
<br><br>
Dan untuk kolom *App types*, kalian bisa memilih *Web app*, *Mobile app* ataupun keduanya. Sesuaikan dengan kebutuhan aplikasi yang ingin kalian buat nanti. Disini saya menggunakan yang *Web app*.

4. Setelah semuanya lengkap terisi. Centang bagian *agreement* dan klik tombol **Create**. 
5. Jika kita berhasil membuat LINE Login Channel, akan ada notifikasi yang muncul pada bagian kanan atas halaman website dan kalian akan otomatis diarahkan pada bagian informasi Channel yang sudah dibuat tadi.
![channel-info.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612180023415/NzVmrR6TW.png)
6. Dan jika kita lihat pada bagian provider, channel yang sudah dibuat tadi akan muncul.
![provider-list-channel.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612180132510/RH6VulfQg.png)

Hooray! Selamat kita sudah berhasil membuat LINE Login Channel. Langkah selanjutnya kita akan memulai proses membuat aplikasi web LIFF nya, tapi di part selanjutnya ya!. Jika ada masukan dan pertanyaan bisa lewat kolom komentar. **See you next time!**

Kalau kalian ingin tau lebih jelasnya, kalian bisa baca dokumentasi resminya di link ini  
- [https://developers.line.biz/en/docs/line-login/overview/#integrating-with-web-apps](https://developers.line.biz/en/docs/line-login/overview/#integrating-with-web-apps) 
-  [https://developers.line.biz/en/docs/liff/overview/](https://developers.line.biz/en/docs/liff/overview/) 

