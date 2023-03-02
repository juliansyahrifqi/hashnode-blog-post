---
title: "Membuat Custom Validation dengan Database di NestJS"
seoTitle: "Membuat Custom Validation dengan Database di Nest JS"
seoDescription: "Berdasarkan halaman dokumentasi NestJS, NestJS adalah sebuah framework untuk membangun aplikasi backend/server-side yang scalable dan efisien."
datePublished: Thu Mar 02 2023 13:16:39 GMT+0000 (Coordinated Universal Time)
cuid: cler4rrio000j09le2qsb5gx0
slug: membuat-custom-validation-dengan-database-di-nestjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677762716042/2fdffc61-229a-4f29-949c-ebee0099c8d1.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1677762948428/d47e0f0b-658e-4e44-bb45-18ec38958f2d.png
tags: backend, typescript, nestjs, decorators

---

Pada artikel kali ini, saya akan mencoba menjelaskan/membuat *custom validation* dengan *database* di NestJS

## Pendahuluan

Berdasarkan halaman dokumentasi NestJS, NestJS adalah sebuah *framework* untuk membangun aplikasi *backend/server-side* yang *scalable* dan efisien. NestJS juga dibangun dan sepenuhnya mendukung Typescript.

*Framework* NestJS sendiri sudah banyak menyediakan cara untuk melakukan validasi *request* data, seperti menggunakan `Pipes` . Tetapi kita juga bisa menggunakan library `class-validator` melalui `ValidationPipe.`

`ValidationPipe` menyediakan pendekatan yang nyaman dan mudah untuk menerapkan aturan-aturan validasi dari request data, dimana nantinya aturan-aturan validasi ini nantinya dapat diterapkan di dalam `class` atau `DTO` dari setiap *module*.

Validasi dengan menggunakan library `class-validator` ini disarankan juga oleh NestJS, seperti yang ada pada [dokumentasi NestJS](https://docs.nestjs.com/techniques/validation#validation).

## Membuat Custom Validation

`class-validator` sendiri adalah *library* yang *powerful* dan sudah banyak digunakan. `class-validator` menyediakan banyak *decorator* untuk validasi. Namun, `class-validator` hanya menyediakan untuk validasi dasar saja seperti mengecek apakah string, apakah input kosong dan lainnya. Untuk validasi seperti mengecek data ke database `class-validator` tidak menyediakan secara default, tetapi `class-validator` juga menyediakan untuk kita dapat membuat sebuah *custom validation*.

Untuk menggunakan `class-validator` kita bisa menginstallnya terlebih dahulu dengan menggunakan perintah.

```bash
npm i --save class-validator class-transformer
```

Setelah itu, kita bisa menggunakan fungsi `useContainer` dari `class-validator` yang fungsinya adalah untuk melakukan *setting container* agar dapat digunakan oleh *library* `class-validator` dan juga untuk melakukan NestJS *depedency injection*.

Kita bisa menggunakannya di dalam file `main.ts.`

```typescript
import 'dotenv/config';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common';
import { useContainer } from 'class-validator';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const port = process.env.PORT || 3001;

  app.useGlobalPipes(new ValidationPipe({ transform: true }));

  // Tambahkan ini di file main.ts
  useContainer(app.select(AppModule), { fallbackOnErrors: true });

  await app.listen(port, () => {
    console.log(`Listen on port ${port}`);
  });
}
bootstrap();
```

Selanjutnya kita bisa membuat `ValidatorConstraint` dan juga `class` yang isinya adalah *logic* untuk validasi yang akan kita buat.

Kita bisa buat sebuah folder dengan nama validation dan buat file dengan nama (disesuaikan) `UserExists.ts` Untuk penamaan file bisa disesuaikan dengan kebutuhan, karena saya disini akan membuat validasi untuk mengecek apakah user terdaftar atau tidak maka saya namai dengan `UserExists`.

```typescript
import { Injectable } from '@nestjs/common';
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationOptions,
  registerDecorator,
} from 'class-validator';
import { UsersService } from 'src/users/users.service';

@ValidatorConstraint({ name: 'UserExists', async: true })
@Injectable()
export class UserExistsValidation implements ValidatorConstraintInterface {
  constructor(private usersService: UsersService) {}

  async validate(value: number): Promise<boolean> {
    return this.usersService.getUsersById(value).then((data) => {
      if (data !== null) {
        return true;
      } else {
        return false;
      }
    });
  }

  defaultMessage() {
    return `users doesn't exists in table users`;
  }
}
```

*Class* diatas berfungsi untuk mencari data *user* ke *database* sesuai dengan id yang diinputkan (melalui method `getUsersById` yang ada pada class `userService`). *Method validate* harus mereturn sebuah tipe data boolean.

Disini kita menggunakan decorator `@Injectable()` untuk kita bisa melakukan `depedency injection`, seperti yang kita lihat kita melakukan *injection depedency* ke dalam `constructor` dari *class* `UserExistsValidation`.

Setelah kita membuat *class* tersebut, kita bisa mendeklarasikan/menggunakan pada *providers* di *module* yang sesuai. Karena disini saya akan menggunakannya di *users*, maka saya akan mendeklarasikannya di *module users* atau di *file* `users.module.ts`

```typescript
import { Module } from '@nestjs/common';
import { SequelizeModule } from '@nestjs/sequelize';
import { UsersService } from './users.service';
import { users } from 'models';
import { UsersController } from './users.controller';
import { UserExistsValidation } from './validation/userExists';
import { AuthModule } from 'src/auth/auth.module';

@Module({
  imports: [SequelizeModule.forFeature([users])],
  providers: [UsersService, UserExistsValidation], // Deklarasi Injectable Class
  controllers: [UsersController],
  exports: [UsersService],
})
export class UsersModule {}
```

Setelah itu kita bisa gunakan validasi yang tadi kita buat di dalam sebuah class/DTO mana saja. Disini contohnya saya akan menggunakannya di dalam `CreateCustomerDto`.

Cara menggunakannya adalah dengan menggunakan *decorator* `@Validate(nama class)` atau disini `@Validate(UserExistsValidation)`

```typescript
import { IsNotEmpty, Validate } from 'class-validator';
import { UserExistsValidation } from 'src/users/validation/userExists';

export class CreateCustomerDto {
  @IsNotEmpty()
  firstname: string;

  @IsNotEmpty()
  lastname: string;

  @IsNotEmpty()
  @Validate(UserExistsValidation) // Menggunakan validasi
  user_id: number;
}
```

Jika `user_id` yang kita input tidak ada di `database/table`users maka akan mereturn error dengan pesan: **"users doesn't exists in table users".**

Nah dari sini kita sudah bisa memakai *custom validation* yang sudah kita buat sendiri di class/DTO manapun di dalam project kita.

Kita juga bisa membuat decorator sendiri, alih-alih kita harus memanggil `@Validate(UserExistsValidation)` kita bisa membuatnya menjadi `@IsUserExists`

## Membuat Decorator Custom Sendiri

Cara membuatnya adalah hanya dengan cara menggunakan *decorator factory* atau dengan menggunakan fungsi `registerDecorator()` .

Kita buat fungsi sesuai dengan nama *decorator* yang akan kita buat di dalam *file* yang sama ketika kita membuat *class* untuk *custom validation*, disini saya akan membuat *decorator* `IsUserExist` maka saya akan membuat fungsi dengan nama `IsUserExist` .

```typescript
import { Injectable } from '@nestjs/common';
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
  ValidationOptions,
  registerDecorator,
} from 'class-validator';
import { UsersService } from 'src/users/users.service';

@ValidatorConstraint({ name: 'UserExists', async: true })
@Injectable()
export class UserExistsValidation implements ValidatorConstraintInterface {
  constructor(private usersService: UsersService) {}

  async validate(value: number): Promise<boolean> {
    return this.usersService.getUsersById(value).then((data) => {
      if (data !== null) {
        return true;
      } else {
        return false;
      }
    });
  }

  defaultMessage() {
    return `users doesn't exists in table users`;
  }
}

// Register The Decorator
export function IsUserExist(
  property: string,
  validationOptions?: ValidationOptions,
) {
  return function (object: any, propertyName: string) {
    registerDecorator({
      name: 'UserExists',
      target: object.constructor,
      propertyName: propertyName,
      constraints: [property],
      options: validationOptions,
      validator: UserExistsValidation,
    });
  };
}
```

*Decorator* `IsUserExist` sendiri menerima dua parameter, satu adalah untuk *property* dan juga *validationOptions* yang dapat kita isi seperti *custom message* dan lainnya.

Setelah itu kita bisa ganti cara pemanggilan validasi dari `@Validate(UserExistsValidation)` menjadi hanya `@IsUserExists` di dalam class/DTO kita.

```typescript
import { IsNotEmpty } from 'class-validator';
import { IsUserExist } from 'src/users/validation/userExists';

export class CreateCustomerDto {
  @IsNotEmpty()
  firstname: string;

  @IsNotEmpty()
  lastname: string;

  @IsNotEmpty()
  // Memanggil decorator, dan menambahkan custom message
  @IsUserExist('user_id', {
    message: `user tidak ditemukan di dalam database`,
  })
  user_id: number;
}
```

*Decorator* `@IsUserExist` kita mengisi argument seperti `property` yang diisi dengan 'user\_id' dan juga `validationOptions` yaitu *custom message*: "**user tidak ditemukan di dalam database**".

---

Begitulah kira-kira cara untuk membuat **Custom Validation** sekaligus juga membuat **Custom Decorator**. Untuk lebih jelasnya kita bisa lihat pada referensi dibawah.

* [Dokumentasi NestJS (Validasi)](https://docs.nestjs.com/techniques/validation#using-the-built-in-validationpipe)
    
* [Dokumentasi Library class-validator](https://www.npmjs.com/package/class-validator#custom-validation-classes)