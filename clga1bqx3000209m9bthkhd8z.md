---
title: "Mengirim Email di NestJS"
seoTitle: "Mengirim Email di NestJS menggunakan Nodemailer"
seoDescription: "Membuat dan mengirim email sederhana di NestJS dengan menggunakan Nodemailer dan Fake Service SMPT Ethereal"
datePublished: Sun Apr 09 2023 23:27:32 GMT+0000 (Coordinated Universal Time)
cuid: clga1bqx3000209m9bthkhd8z
slug: mengirim-email-di-nestjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681082672214/326944dd-bdca-47d3-9b2d-80f96f600a13.png
tags: nodemailer, nestjs

---

Pada artikel kali ini kita akan membuat fitur untuk mengirim email sederhana di project `NestJS` dengan menggunakan [Nodemailer](https://nodemailer.com/about/) atau [@nestjs-modules/mailer](https://www.npmjs.com/package/@nestjs-modules/mailer) (NestJS). Untuk keperluan testing kali ini kita akan menggunakan `fake SMTP service` yaitu [ethereal.email](https://ethereal.email/) .

## Persiapan Project

Pertama kita install terlebih dahulu `nesjs-cli` jika belum ada, dengan perintah ini

```bash
npm i -g @nestjs/cli
```

Setelah terinstall, kita buat sebuah project dengan nama email menggunakan perintah

```bash
nest new email
```

Setelah project kita buat, lalu kita install `depedency` atau `library` yang dibutuhkan, seperti `Nodemailer` dan `@nestjs-modules/mailer`

```bash
npm i --save @nestjs-modules/mailer nodemailer
npm install --save-dev @types/nodemailer
```

---

## Membuat Akun Fake SMTP Service Ethereal

Disini karena kita hanya untuk mencoba, kita akan menggunakan `Fake SMTP Service` milik [ethereal.email](https://ethereal.email/).

Pertama kita pergi ke halaman web [https://ethereal.email/](https://ethereal.email/).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681035652871/e4c6adf3-1a30-452b-9b8b-d6cd4046a1a9.png align="center")

Lalu kita klik pada button **Create Ethereal Account**. Setelah itu akan muncul tampilan seperti ini.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681035728121/f2c32626-cf57-45cf-9a58-426dd2557e87.png align="center")

Simpan baik-baik account credentials diatas, karena hal tersebut akan kita gunakan untuk konfigurasi pada project kita.

## Membuat Mail Module

Setelah selesai menginstall `depedency` yang dibutuhkan, selanjutnya kita bisa membuat file-file kebutuhan untuk mail, seperti `Mail Module`, `Mail Service` dan `Mail Controller`.

Di NestJS, kita bisa menggunakan perintah

```bash
nest g module mail/mail
nest g service mail/mail
nest g controller mail/mail
```

untuk membuat sebuah `module, service dan controller`. Dengan menggunakan perintah di atas NestJS akan otomatis akan membuatkan kita folder dan juga file `module, service atau controller` kita di dalam folder `/src`.

Atau jika kita ingin membuat sebuah `module, service, controller, entity dan dto` secara langsung kita bisa menggunakan perintah.

```bash
nest g resource mail/mail
```

Setelah kita membuat file `MailModule`, selanjutnya kita akan mengimport `Mailer Module` milik `@nestjs-modules/mailer` dan juga `Nodemailer` di dalam `MailModule` atau file `mail.module.ts` yang telah kita buat/generate tadi.

Di dalam `MailModule` ini kita akan konfigurasi untuk mail server melalui `SMTP`.

```typescript
import { Module } from '@nestjs/common';
import { MailService } from './mail.service';
import { MailController } from './mail.controller';
import { MailerModule } from '@nestjs-modules/mailer';

@Module({
  imports: [
    MailerModule.forRoot({
      transport: {
        host: 'smtp.ethereal.email',
        port: 587,
        auth: {
          user: 'dock.haley@ethereal.email',
          pass: 'SPEHmbUXgDZENCQGfW',
        },
        tls: {
          rejectUnauthorized: false,
        },
      },
    }),
  ],
  providers: [MailService],
  controllers: [MailController],
  exports: [MailService], // Export Mail Service untuk digunakan sebagai Depedency Injection
})
export class MailModule {}
```

Untuk konfigurasi seperti `host, port, auth dan tls` dapat disesuaikan dengan yang kita dapat ketika create account `ethereal` yang telah kita buat.

## Email Service

Nah untuk fungsi mengirim email kita akan buat di file `mail.service.ts`

```typescript
import { MailerService } from '@nestjs-modules/mailer';
import { Injectable } from '@nestjs/common';

@Injectable()
export class MailService {
  constructor(private mailerService: MailerService) {}

  async sendEmail(email: string) {
    const bodyEmail = 'Haloo, ini contoh kirim email';

    await this.mailerService.sendMail({
      from: 'rifqipratama9@gmail.com',
      to: email,
      subject: 'Contoh Kirim Email NestJS',
      text: bodyEmail,
      html: `<p>${bodyEmail}</p>`,
    });
  }
}
```

Pada file `mail.service.ts` di atas kita menggunakan `MailerService` agar kita bisa menggunakan method method yang ada pada `MailerService` milik `@nestjs-modules/mailer.`

Contohnya pada kode di atas untuk mengirim email kita bisa menggunakan method `sendMail` milik `MailerService` yang didalamnya kita bisa mengatur seperti

* **from**: nama atau asal pengirim email
    
* **to**: tujuan email dikirimkan
    
* **subject**: nama subject pada email yang dikirim
    
* **text**: isi dari body email
    
* **html**: isi dari body email dalam bentuk html
    

Untuk template email sendiri kita bisa menggunakan library untuk template adapter seperti [Pug](https://pugjs.org/api/getting-started.html) atau EJS. Namun, pada tulisan kali ini kita hanya mencoba untuk mengirim email sederhana saja. Apabila ada kesempatan di lain waktu kita akan mencoba menggunakan email template.

## Memanggil Mail Service

`MailService` yang telah kita buat bisa kita digunakan baik di service lain atau pun di `controller`. Kali ini kita akan memanggilnya langsung di `mail.controller.ts`.

```typescript
import { Body, Controller, Post } from '@nestjs/common';
import { MailService } from './mail.service';

@Controller('mail')
export class MailController {
  constructor(private mailService: MailService) {}

  @Post('sendEmail')
  sendEmail(@Body() body: { email: string }) {
    try {
      this.mailService.sendEmail(body.email);

      return { message: 'Email berhasil dikirim' };
    } catch (e) {
      return e;
    }
  }
}
```

Pertama kita memanggil `MailService` dengan menggunakan `constructor`. Selanjutnya kita menggunakan method `POST` untuk menggunakan atau mengirimkan email.

Pada method `sendEmail` diatas kita menerima `parameter` berupa `body` dengan isi yaitu `email`. Karena pada `MailService` yang telah kita buat sebelumnya, pada method `sendEmail` kita menerima parameter yaitu email. Email ini adalah nama alamat email yang akan kita kirim.

Di dalam `MailController` diatas kita menggunakan `try catch`, agar ketika terjadi `error` program yang kita jalankan tidak akan `crash` dan juga agar lebih mudah menangkap error yang terjadi.

Jika email berhasil dikirim, maka akan mereturn `message` dengan isi **"Email berhasil dikirim"**, apaila gagal maka akan mengirimkan pesan error.

## Test Kirim Email

Sekarang jika kita jalankan server `NestJS` kita

```typescript
npm run start:dev
```

Lalu, kita panggil dengan menggunakan `Postman`. Jika berhasil, maka akan muncul seperti tampilan dibawah.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681081074177/70a0d8ac-8ab3-42e5-83e0-ad0443b933a8.png align="center")

Lalu, kita cek pada bagian message di website ethereal.email. Seharusnya email yang telah kita kirim tadi sudah muncul.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681081201493/c76dbd48-8234-4911-b98b-dc509e0bec3f.png align="center")

Apabila kita buka, maka isinya adalah sesuai dengan yang kita kirim pada `MailService` atau `mail.service.ts`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681081266187/b2c21b96-4096-4afe-9aa7-ed0fee9a26aa.png align="center")

---

Sekarang kita sudah berhasil mengirimkan email sederhana di project NestJS. Selanjutnya apabila ada kesempatan kita akan menggunakan template email baik itu menggunakan Pug ataupun EJS. Cukup sekian, Terima Kasih.