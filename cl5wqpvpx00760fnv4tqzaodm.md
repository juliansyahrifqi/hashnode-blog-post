## Prinsip-prinsip integritas dataset

Tulisan ini berisi tentang rangkuman saya selama mengikuti course **Spesialisasi From Data to Insights with Google Cloud ** yang diadakan oleh Google melalui kampus saya. Detail tentang course ini bisa kita lihat di [Link ini (Coursera)](https://www.coursera.org/specializations/from-data-to-insights-google-cloud-platform).

Sebelum memulai, ada sebuah kalimat yaitu **"Garbage in... garbage out"**. Artinya dari kalimat tersebut adalah untuk membuat sebuah model machine learning yang baik, kita tidak bisa menggunakan/mengolah dari "sampah", kita harus menggunakan data yang baik (bukan sampah). 

Karena dalam ML sendiri bergantung pada pola data yang konsisten dan tentunya data yang baik. Jika kita menggunakan data sampah, maka hasil yang dikeluarkan juga akan berupa sampah.
* * *

Dataset dengan kualitas yang tinggi akan mengikuti 5 aturan yang ketat, yaitu **Validity**, **Accuracy**, **Completeness**, **Consistency ** dan **Uniformity**. Kita akan jelaskan satu-persatu dibawah ini.

### Validity (Valid atau Absah)
Data memiliki batasan atau keunikannya masing-masing. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658494896270/vc__ThjlR.png align="center") 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658494953362/bgFiGX_5x.png align="center")

Misal seperti gambar di atas, setiap benda tersebut memiliki tanda pengenal yang unik. Setiap telepon mempunyai nomor yang unik (setiap orang berbeda-beda) jika ada orang yang memiliki nomor yang sama dengan kita maka itu akan menjadi masalah, sama halnya seperti plat nomor pada mobil, plat nomor setiap kendaraan/mobil memiliki keunikan masing-masing, apabila ada yang sama maka akan menjadi masalah besar.

Berhubungan dengan data, setiap data harus memiliki pengenal yang unik. Jika tidak, maka hal tersebut akan menjadi masalah ketika kita akan melakukan **join**.

Kemudian, data yang valid juga memiliki batas jarak. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658495565170/N9yq3I0R3.png align="center")

Misal dari gambar diatas terdapat **Roll** dengan value yang berbeda-beda. Kita lihat pada roll ke-6 value yang keluar adalah 7, hal tersebut tidak akan mungkin mengingat dadu hanya memiliki nilai maks yaitu 6.

Hal tersebut membuat data yang kita punya tidak valid ataupun kotor, sehingga kita harus mentransformasikan atau "membersihkannya" terlebih dahulu sebelum kita proses atau jadikan sebagai model machine learning.

### Accuracy

Kemudian, data juga harus sesuai dengan sumber kebenaran atau realita yang ada hal tersebut lah yang membuat data akurat.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658496331373/ZcxTjU7tc.png align="left")

Misal, di gambar di atas adalah negara bagian yang ada di U.S. Tetapi di dalamnya terdapat negara bagian dengan nama **Hot Dog**. Maka hal tersebut tidak sesuai dengan sumber kebenaran atau realita yang ada sehingga data yang ada tidak akurat.

### Completeness

Data juga harus lengkap tidak bisa setengah-setengah, hal tersebut akan menjadikan kita kekurangan informasi. Jika misal kita ingin membuat gambaran tentang nilai akhir mahasiswa, tetapi hanya ada data mahasiswa dan nilai, kita akan berfikiran mungkin bahwa untuk mendapatkan nilai akhir cukup hanya dengan data itu, tetapi kemudian untuk nilai akhir juga bisa dilihat dari jumlah kehadiran mahasiswa yang dimana kita tidak mempunyai data tersebut, akhirnya gambaran tentang nilai akhir mahasiswa kita bisa saja tidak valid ataupun salah dikarenakan ada kekurangan data.

Hal yang wajar apabila kita terus menanyakan "Apakah data yang kita punya sudah lengkap", "Apakah saya punya semuanya" dan lainnya.

### Consistency

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658497091388/v5uzvmhmH.png align="left")

Kita lihat pada gambar di atas, pada tabel yang atas rumah yang terdapat pada alamat **123 ABC St** adalah kepemilikan dari **Owner ID 12**, tetapi kemudian pada tabel yang bawah **Owner ID 12** memiliki alamat di **53rd Ave.**. Hal tersebut akan menimbulkan pertanyaan **Jadi, siapa yang memiliki rumah tersebut?**. 

Hal tersebut membuat data yang ada tidak konsisten dan sulit dimengerti oleh manusia yang membacanya.

### Uniformity

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658497353013/vX9y6ubRX.png align="left")

Ada sebuah kasus seperti yang ada pada gambar di atas, NASA kehilangan robot pengorbit iklim di Mars karena tim peneliti menggunakan sistem pengukuran yang berbeda, di tim satu menggunakan sistem Inggris yaitu **feet dan yards** dan tim lainnya menggunakan metric system. Sehingga perhitungan atau pengukuran yang harusnya bisa sama malah memiliki hasil yang berbeda, entah yang mana yang benar.

Oleh karena itu, apabila kita akan membangun sebuah sistem yang kompleks disarankan agar menggunakan sistem perhitungan atau pengukuran yang sama  atau satu jenis (uniform) sehingga terhindar dari kesalahan-kesalahan yang tidak diinginkan


