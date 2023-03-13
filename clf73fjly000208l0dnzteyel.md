---
title: "Upload Single File (Optional) dengan Multer di NestJS"
seoTitle: "Upload Single File (Optional) dengan Multer di NestJS"
seoDescription: "Upload File hari ini adalah sebuah kebutuhan dalam pengembangan web, seperti misalnya upload gambar untuk sebuah web toko online atau portal berita"
datePublished: Mon Mar 13 2023 17:23:28 GMT+0000 (Coordinated Universal Time)
cuid: clf73fjly000208l0dnzteyel
slug: upload-single-file-optional-dengan-multer-di-nestjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678727415820/e5132e45-51d7-4ac2-a18c-325f57c0554b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1678728142632/dc15bfca-6ce6-4933-af8e-60a7a368df36.png
tags: express, backend, multer, file-upload, nestjs

---

**Upload File** hari ini adalah sebuah kebutuhan dalam pengembangan web, seperti misalnya *upload* gambar untuk sebuah web toko online atau web portal berita, atau video untuk web *landing page* perusahaan dan file lainnya.

Tulisan kali ini kita akan mencoba membuat fungsi *upload single file image* dengan menggunakan library `Multer` dan *framework* `NestJS`.

Disini saya anggap bahwa pembaca sudah memiliki sebuah project `NestJS`.

### Upload File

Untuk kebutuhan fungsi upload file sendiri, **NestJS** sudah menyediakan sebuah module bawaan yang menggunakan **Multer** di dalamnya. **Multer** sendiri adalah sebuah *middleware* untuk melakukan file upload di **Express**.

**Multer** mengolah data yang dikirim melalui format `multipart/form-data` yang memang umum digunakan ketika kita melakukan *upload file* dengan melalui request `HTTP POST` method.

Karena **NestJS** menggunakan **Typescript** secara bawaan dan juga untuk *type safety*, kita install terlebih dahulu package `Multer typings`.

```bash
npm i -D @types/multer
```

### Contoh Sederhana

Contohnya disini saya akan mencoba menerapkannya untuk `menambah product`. Sesuai dengan yang dicontohkan pada dokumentasi, kita akan menyimpan fungsi *upload file* ini di `controller`. Pada kasus saya, maka fungsi tersebut akan disimpan di file `product.controller.ts` dan pada method `createProduct`.

```typescript
import { Controller, Post, UploadedFile, UseInterceptors } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';

@Controller('product')
export class ProductController {
  constructor(private readonly productService: ProductService) {} 

  @Post('createProduct')
  @UseInterceptors(FileInterceptor('image'))
  async createProduct(
    @UploadedFile() file: Express.Multer.File,
  ) {
    console.log(file);
  }
}
```

Untuk mengupload *single file* kita bisa menggunakan *interceptors* `FileInterceptors` dan untuk mengambil *file* dari *request* yang dikirim kita bisa menggunakan *decorator* `@UploadedFile`.

Lalu, setelah itu kita coba melalui `Postman`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678467667513/57701d77-b195-4faa-b134-c5d396273855.png align="center")

Nama `key` pada `Postman` kita sesuaikan dengan parameter yang ada pada *decorator* `FileInterceptor(...)`. Pada contoh diatas saya menggunakan parameter **'image'**, maka dari **Postman** pun kita harus mengisi `key` dengan nama `'image'.`

> Untuk decorator `FileInterceptor` sendiri dapat menerima dua argument, yaitu:
> 
> * `fieldName` : nama field sesuai dengan yang dikirim dari client baik melalui Postman atau form HTML.
>     
> * `optional`: objek-objek opsional yang disediakan oleh Multer melalui MulterOptions, seperti misalnya penamaan file, direktory dan lainnya.
>     

Jangan lupa untuk mengirimkan `body` dengan menggunakan tipe `form-data`, karena jika selain tipe tersebut kita tidak bisa mengupload file.

Nah, apabila kita klik `send` maka tidak akan memunculkan hasil apa-apa di `Postman`.

Namun jika kita lihat pada *console text editor* pada project kita, maka akan muncul seperti ini.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678467858343/73ec6535-77e5-4de7-9294-143f975a40af.png align="center")

Maka artinya, `controller` kita atau file `product.controller.ts` sudah bisa menerima `request file` yang dikirim dari `client/user`.

### Menambahkan DiskStorage Multer

Tetapi file yang kita *upload* belum masuk ke folder file di dalam *project* kita, karena secara default `Multer` tidak akan membuat folder ketika kita tidak mendefinisikan di folder mana file kita akan disimpan. Oleh karena itu, agar file yang diupload dapat masuk ke folder *project* kita, kita bisa menggunakan `options` yang sudah disediakan oleh `Multer`. Kita bisa baca selengkapnya di [dokumentasi Multer](https://github.com/expressjs/multer#multeropts).

```typescript
storage: diskStorage({
  destination: './uploads',
  filename(req, file, callback) {
    const uniqueSuffix = Date.now() + Math.round(Math.random() * 1e9);
      callback(
        null,
          uniqueSuffix + '.' + file.originalname.split('.').pop(),
      );
   },
}),
```

Di atas kita menggunakan `diskStorage` milik `Multer`. Dengan menggunakan `diskStorage` kita bisa mengkontrol untuk penyimpanan file di `disk project` kita.

`diskStorage` sendiri menerima `dua options`, yaitu:

* `destination`: untuk mengatur di folder mana file yang dikirim oleh client akan disimpan di dalam *project* kita.
    
* `filename`: sebuah *function* yang berfungsi untuk mengcustom penamaan file kita, seperti kode diatas kita memberikan nama *file* yang dikirim oleh *client* dengan penamaan unik dengan memanfaatkan tanggal `Date.now()` dan nomor acak `Math.random() * 1e9`
    

> Note:
> 
> * Apabila `destination` tidak kita buat maka, file tidak akan tersimpan di dalam project kita
>     
> * Ketika kita mengisikan string di dalamnya maka otomatis `Multer` akan membuatkan folder sesuai `path/string` yang kita isi. Misalnya pada kasus diatas adalah `./uploads`, maka `Multer` akan otomatis membuat `folder uploads` di root directory project kita.
>     
> * Kita harus mengatur nama extension dari file yang diupload karena secara default Multer tidak akan menambahkan extension untuk file yang diupload. Pada kode diatas kita menggunakan perintah `file.originalname.split('.').pop()` di dalam function `filename` untuk mengambil dan menambah extension untuk file yang diupload.
>     
> * Jika kita tidak mendefinisikan nama file di dalam `function filename`, secara default `Multer` akan memberikan nama yang acak untuk file yang diupload tanpa disertai dengan extension file.
>     

Setelah kita menambahkan kode diatas, maka *method* `createProduct` yang ada di `controller` akan menjadi seperti ini.

```typescript
import { Controller, Post, UploadedFile, UseInterceptors } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { diskStorage } from 'multer';

@Post('createProduct')
  @UseInterceptors(
    FileInterceptor('image', {
      storage: diskStorage({
        destination: './uploads',
        filename(req, file, callback) {
          const uniqueSuffix = Date.now() + Math.round(Math.random() * 1e9);
          callback(
            null,
            uniqueSuffix + '.' + file.originalname.split('.').pop(),
          );
        },
      }),
    }),
  )
  async createProduct(@UploadedFile() file: Express.Multer.File) {
    console.log(file);
  }
}
```

Jika kita melakukan *request* kembali di Postman, maka seharusnya folder dengan nama `uploads` sudah terbuat dengan disertai file yang kita kirim didalamnya.

### Menambahkan Validasi File Upload

Untuk kebutuhan validasi *file upload* kita bisa menggunakan validasi dari `Multer` ataupun dari `NestJS` dengan menggunakan `Pipe`. `Pipe` yang digunakan untuk validasi *file upload* adalah `ParseFilePipe`.

Apabila kita menggunakan validasi dari `Multer` kita bisa menggunakan *options* `fileFilter` dan `limits`. `fileFilter` berfungsi untuk validasi tipe file apa saja yang boleh dikirim dan `limits` berfungsi untuk melakukan limit besaran file (dalam *byte*) yang boleh diupload .

Pertama kita akan mencoba menggunakan validasi yang sudah ada di `NestJS` yaitu menggunakan `ParseFilePipe` atau `FileValidator`. `NestJS` memiliki bawaan dua `FileValidator` yang bisa kita implementasikan, yaitu `MaxFileSizeValidator` dan `FileTypeValidator`.

* `MaxFileSizeValidator` berfungsi untuk mengkontrol berapa besaran file (dalam *byte*) yang boleh diikirim/diupload
    
* `FileTypeValidator` berfungsi untuk memvalidasi tipe file apa yang boleh dikirim/diupload.
    

Untuk penggunaanya kita bisa menambahkan *argument* kedalam *decorator* `@UploadedFile`.

```typescript
new ParseFilePipe({
  validators: [
    new MaxFileSizeValidator({ maxSize: 2000000 }),
    new FileTypeValidator({ fileType: /(jpg|jpeg|png|gif)$/ }),
  ],
}),
```

Diatas kita menerapkan validasi untuk size file sebesar 2Mb (**2000000 *byte***) dan tipe file berupa `jpg,jpeg,png,gif` (menggunakan `regex`).

Selain itu kita juga bisa menerapkan agar file yang diupload bersifat *optional* dengan menggunakan *options*:

```typescript
fileIsRequired: false,
```

di dalam `ParseFilePipe`. Jika kita tidak menerapkan kode diatas maka setiap kita akan melakukan *create product* maka kita wajib menyertakan juga file (gambar produk). Mungkin saja kebutuhan kita terkadang tidak memerlukan untuk *upload file* ketika menambahkan produk karena keadaan tertentu.

Jika kita terapkan kode diatas maka method `createProduct` pada file `product.controller.ts` kita akan menjadi seperti dibawah ini.

```typescript
import { Controller, FileTypeValidator, MaxFileSizeValidator, ParseFilePipe, Post, UploadedFile, UseInterceptors } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { diskStorage } from 'multer';

@Post('createProduct')
  @UseInterceptors(
    FileInterceptor('image', {
      storage: diskStorage({
        destination: './uploads',
        filename(req, file, callback) {
          const uniqueSuffix = Date.now() + Math.round(Math.random() * 1e9);
          callback(
            null,
            uniqueSuffix + '.' + file.originalname.split('.').pop(),
          );
        },
      }),
    }),
  )
  async createdProduct(
    @UploadedFile(
      new ParseFilePipe({
        validators: [
          new MaxFileSizeValidator({ maxSize: 2000000 }),
          new FileTypeValidator({ fileType: /(jpg|jpeg|png|gif)$/ }),
        ],
        fileIsRequired: false,
      }),
    )
    file: Express.Multer.File,
  ) {
    console.log(file);
  }
```

Jika kita mencoba mengirim request file dengan format selain gambar maka hasilnya akan error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678636296094/d7c57cbf-efcc-4e3a-9282-9b60e274a334.png align="center")

### File Tetap Terupload Walau Validasi Berhasil dan Response Mengembalikan Error

Namun, jika kita melihat ke folder uploads di project kita, file yang kita kirim tetap masuk ke dalam folder `uploads` walaupun validasi berhasil dan `NestJS` merespons dengan *error*. Nah untuk mengatasinya kita perlu juga untuk memvalidasi di `Multer` agar ketika validasi berhasil maka file yang kita kirim tidak akan masuk ke dalam folder `uploads` pada project kita.

### Mengatasi File Tetap Terupload dengan Validasi Multer

Kita juga bisa menambahkan validasi `Multer` seperti yang telah dijelaskan sebelumnya. Kita akan menggunakan `fileFilter` dan `limits` yang disediakan Multer untuk melakukan validasi.

Kita bisa menambahkannya ke dalam `FileInterceptor`.

```typescript
fileFilter(req, file, callback) {
  if (!file.originalname.match(/\.(jpg|jpeg|png|gif)$/)) {
     return callback(
       new UnsupportedMediaTypeException('File is not an image'),
         false,
       );
     }

     callback(null, true);
  },
  limits: { fileSize: 2000000 },
```

Diatas kita menerapkan validasi untuk `fileFilter` khusus untuk image dengan menerapkan regex `/\.(jpg|jpeg|png|gif)$/` . Apabila file yang dikirim bukan image atau tidak berbentuk format `(jpg|jpeg|png|gif)` maka akan mereturn error `UnsupportedMediaTypeException` . Selain itu juga kita menerapkan validasi untuk besaran file di dalam `limits` yaitu sebesar 2Mb (`2000000 byte`).

Jika kita menambahkan kode diatas maka method `createProduct` di file `product.controller.ts`

```typescript
import { Body, Controller, FileTypeValidator, MaxFileSizeValidator, ParseFilePipe, ParseIntPipe, Post, UploadedFile, UseInterceptors, UnsupportedMediaTypeException } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { Request } from 'express';
import { diskStorage } from 'multer';

@Post('createProduct')
  @UseInterceptors(
    FileInterceptor('image', {
      storage: diskStorage({
        destination: './uploads',
        filename(req, file, callback) {
          const uniqueSuffix = Date.now() + Math.round(Math.random() * 1e9);
          callback(
            null,
            uniqueSuffix + '.' + file.originalname.split('.').pop(),
          );
        },
      }),
    }),
  )
  async createdProduct(
    @Req() req: Request, // Menambahkan Request dari express
    @Body() createProductDto: CreateProductDto, // Create DTO Product
    @UploadedFile(
      new ParseFilePipe({
        validators: [
          new MaxFileSizeValidator({ maxSize: 2000000 }),
          new FileTypeValidator({ fileType: /(jpg|jpeg|png|gif)$/ }),
        ],
        fileIsRequired: false,
      }),
    )
    file: Express.Multer.File,
  ) {
    console.log(file);
  }
```

Nah jika kita sekarang mencoba mengirimkan file yang bukan gambar maka akan mereturn *error* dan file yang dikirim tidak akan disimpan ke dalam folder `uploads.`

### Create Product ke Database

Sekarang kita sudah dapat mengupload file ke dalam folder kita, selanjutnya kita akan menambahkannya ke dalam `database`.

Untuk `create product` ke `database`, kita tidak mengupload filenya secara langsung ke `database`, melainkan hanya `string url` untuk akses file yang ada di folder `uploads` kita saja.

```typescript
import { Body, Controller, FileTypeValidator, MaxFileSizeValidator, ParseFilePipe, ParseIntPipe, Post, UploadedFile, UseInterceptors } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { Request } from 'express';
import { diskStorage } from 'multer';

@Post('createProduct')
  @UseInterceptors(
    FileInterceptor('image', {
      storage: diskStorage({
        destination: './uploads',
        filename(req, file, callback) {
          const uniqueSuffix = Date.now() + Math.round(Math.random() * 1e9);
          callback(
            null,
            uniqueSuffix + '.' + file.originalname.split('.').pop(),
          );
        },
      }),
    }),
  )
  async createdProduct(
    @Req() req: Request, // Menambahkan Request dari express
    @Body() createProductDto: CreateProductDto, // Create DTO Product
    @UploadedFile(
      new ParseFilePipe({
        validators: [
          new MaxFileSizeValidator({ maxSize: 2000000 }),
          new FileTypeValidator({ fileType: /(jpg|jpeg|png|gif)$/ }),
        ],
        fileIsRequired: false,
      }),
    )
    file: Express.Multer.File,
  ) {
    try {
      let finalImageUrl: string;

      if (file) {
        finalImageUrl =
          req.protocol +
          '://' +
          req.get('host') +
          '/uploads/' +
          file.filename;
      } else {
         finalImageUrl =
          req.protocol +
          '://' +
          req.get('host') +
          '/uploads/' +
          'product-not-available.jpg';
      }

      await this.productService.createProduct({
        ...createProductDto,
        image: finalImageUrl,
      });

      return { status: 'Success', message: 'Product successfully created' };
    } catch (e) {
      return e;
    }
  }
```

Pada kode diatas kita menggunakan `try catch` agar ketika terjadi error maka program yang kita jalankan tidak berhenti dan menangkap `error` yang terjadi.

Adapun logika kode diatas:

* Kita mendeklarasikan variabel dengan nama `finalImageUrl` dengan tipe data `string`
    
* Pada blok `if(file)` kita mengecek apakah ada file yang dikirim/diupload oleh client, jika ya maka variabel `finalImageUrl` diisi dengan url menuju ke folder `uploads` pada project kita
    
* Jika tidak ada file yang dikirim, maka `finalImageUrl` akan diisi dengan url menuju file `product-not-found.jpg`
    
* Setelah itu kita mengirimkan *request* dari *user* menuju ke `service` untuk ditambahkan ke tabel *product*.
    
* Lalu, *return* pesan berhasil.
    

`CreateProductDTO` sendiri adalah sebuah class yang berisi tipe data atau field yang kita butuhkan untuk create product ke database. Isi dari class `CreateProductDTO` adalah:

```typescript
export class CreateProductDto {
  name: string;
  description: string;
  category_id: number;
  price: string;
  image: string;
}
```

Jika kita melakukan *request* kembali dari `Postman`, maka hasilnya akan seperti gambar dibawah ini

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678638036762/e1a036c8-5b32-4525-94c0-e4dd650b0e7e.png align="center")

Jika kita get all product, maka *product* berhasil disimpan ke dalam database.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678638413668/f7656e51-064c-4b9c-ab9d-c99a3a2a06fd.png align="center")

Namun, jika kita mengklik *url* dari image kita tadi dan melakukan request melalui `Postman`. Maka file image tidak akan tampil. Agar image bisa kita tampil ketika kita get maka kita bisa menjadikan folder menjadi `static folder` menggunakan `NestJS` ataupun dengan membuat sebuah *method* untuk mengambil *image* dengan menggunakan *method* dari `Express` yaitu `sendFile`.

Dokumentasi Serve Static File/Folder NestJS: [https://docs.nestjs.com/recipes/serve-static](https://docs.nestjs.com/recipes/serve-static)

Dokumentasi Express SendFile: [https://expressjs.com/en/5x/api.html#res.sendFile](https://expressjs.com/en/5x/api.html#res.sendFile)

---

Cukup Sekian tulisan kali ini, semoga bermanfaat.

Referensi:

* File Upload NestJS: [https://docs.nestjs.com/techniques/file-upload#file-upload](https://docs.nestjs.com/techniques/file-upload#file-upload)
    
* Multer: [https://www.npmjs.com/package/multer](https://www.npmjs.com/package/multer)