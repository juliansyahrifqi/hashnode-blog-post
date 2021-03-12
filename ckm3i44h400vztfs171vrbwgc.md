## Menggunakan Hashnode GraphQL API pada Aplikasi React

Hashnode adalah salah satu platform gratis untuk menulis blog yang dikhususkan untuk para developer di seluruh dunia. Banyak sekali fitur yang dapat kita gunakan pada platform ini, seperti:
- Mendukung  [Markdown](https://www.markdownguide.org/getting-started/) 
- Backup dengan Github.
- Edit ( Custom ) CSS sendiri.
- Syntax Highlighting.
- Bisa menghosting blog kita dengan menggunakan domain kita sendiri.

Selain dari fitur-fitur tersebut, Hashnode juga menyediakan layanan API yaitu **Hashnode's GraphQL API.** Kita bisa memanfaatkan layanan API tersebut salah satunya untuk **menampilkan data dari blog Hashnode  kita di halaman website portfolio kita**.

Kita bisa membaca dokumentasi dan bermain-main dengan Hashnode GraphQL API  [disini](https://api.hashnode.com/).

## Mencoba Hashnode API

Hashnode API ini menggunakan GraphQL, jadi untuk dapat mendapatkan data kita harus mengirimkan query.
> GraphQL adalah bahasa manipulasi untuk API. Cara kerjanya adalah dengan cara mengirimkan GraphQL query untuk mendapatkan data yang kita inginkan dari API.

Kita bisa lihat melalui dokumentasinya, untuk mendapatkan data postingan blog kita dapat mengirimkan query dari **user -> publication -> posts**.

![hashnode-api-dokumentasi.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615361294771/J4nHyeAe6.png)

Query yang dapat kita gunakan adalah seperti ini.
```
query{
  user(username: "juliansyahrifqi") {
    publication {
      posts{
        dateAdded
        slug
        title
        brief
        coverImage
      }
    }
  }
}
```
Jika kita jalankan query di atas di halaman web  [Hashnode API](https://api.hashnode.com/), maka kita akan mendapatkan hasil seperti ini.
![hashnode-api-query.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615362057082/fQU9ZhDW0.png)

### Penjelasan
Pada query diatas, kita mengirimkan query ke halaman website [https://api.hashnode.com/](https://api.hashnode.com/) untuk merequest data postingan blog, seperti tanggal ditambahkannya blog ( **dateAdded** ), URL atau permalink  [( slug )](https://www.niagahoster.co.id/blog/apa-itu-slug/), judul postingan blog ( **title** ), keterangan postingan ( **brief** ) dan cover gambar ( **coverImage** ).

Selain data diatas, masih banyak data yang bisa direquest oleh kita, silahkan lihat pada dokumentasi.

## Menggunakan Hashnode API untuk Aplikasi React 

Seperti dijelaskan sebelumnya, kita akan menampilkan data postingan blog hashnode pada website kita dengan menggunakan React.

### Menyiapkan Project React

Kita pasang project aplikasi React dengan menggunakan ``create-react-app``.
```
$ npx create-react-app hashnode-api
```
Buka project React tersebut di text editor. Disini saya menggunakan [Visual Studio Code.](https://code.visualstudio.com/)

```
$ cd hashnode-api
$ code .
```
Jalankan project react kita.
```
$ npm start
```
Project React kita akan berjalan pada ``localhost:3000``

![react-app.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615366282226/Gin4v85nC.png)

Pada bagian folder ``src``, kita hapus beberapa file yang tidak dibutuhkan, seperti ``App.css``, ``App.test.js``, ``logo.svg``, ``reportWebVitals.js``, dan ``setupTests.js``.

Kita buat folder baru dengan nama ``components``. Folder ini nantinya akan diisi oleh komponen-komponen yang akan kita buat.

Struktur folder projek kita akan menjadi seperti ini:
```
├── node_modules
├── public
├── src
│   ├── components
│   └── index.css
│   └── index.js
├── .gitignore
├── package-lock.json
├── package.json
└── README.md
```
Pada file ``App.js`` kita hapus kode ``import logo`` dan ``import App.css``, kemudian ganti dengan ``import React from 'react'``

Pada file ``index.js`` kita hapus kode ``import reportWebVitals`` dan kode ``reportWebVitals()``.

### Mengambil Data atau Fetching Data Postingan Blog

Buat satu file di dalam folder ``components`` dengan nama ``BlogPost.js``. Di dalam file tersebut nantinya kita akan melakukan pemanggilan API dan juga menampilkan semua data postingan blog hashnode kita. 

Masukkan kode di bawah ke dalam file ``BlogPost.js`` tersebut.
```
import React, { useEffect, useState } from 'react';
import Post from './Post';
import '../index.css';

const BlogPost = () => {
    const [posts, setPosts] = useState(null);

    const query = `
        {   
            user(username: "juliansyahrifqi") {
            publication {
                posts {
                    dateAdded
                    slug
                    title
                    brief
                    coverImage
                    }
                }
            }
        }
    `;

    useEffect(() => {
        const fetchAPI = async () => {
            const response = await fetch('https://api.hashnode.com', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ query })
            })

            const responseData = await response.json();
            setPosts(responseData.data.user.publication.posts);
        } 

        fetchAPI();
    })

    return (
        <div className="post">
            {posts && (
                <div className="card-wrapper">
                    {posts.map((post, index) => (
                        <Post key={index} post={post} />
                    ))}
                </div>
            )}
        </div>
    )
}

export default BlogPost;
```

Pada kode diatas saya menggunakan  [React Hooks](https://reactjs.org/docs/hooks-intro.html). Pada kode diatas juga kita melakukan [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) array dan melemparkan elemen ``post`` melalui [props](https://reactjs.org/docs/components-and-props.html) ke komponen ``Post.js``

Selanjutnya pada folder ``components``, kita buat satu lagi file dengan nama ``Post.js``. Pada file komponen ``Post.js`` tersebut akan menangkap sebuah props ``post`` yang dilempar oleh ``BlogPost.js`` tadi dan komponen ini juga berfungsi untuk menampilkan masing-masing postingan dari blog hashnode kita.

Kode dari ``Post.js`` sendiri adalah sebagai berikut:
```
import React from 'react';
import '../index.css';

const Post = ({ post }) => {
    const options = {weekday: 'long', year: 'numeric', month: 'long', day: 'numeric'};

    return (
        <div className="card">
            <img src={post.coverImage} alt={post.title}></img>
            <a href={`juliansyahrifqi.hashnode.dev/${post.slug}`}>
                <h2>{post.title}</h2>
            </a>
            <p className="date">{new Date(post.dateAdded).toLocaleDateString('id', options)}</p> 
            <hr></hr>
            <p>{post.brief}</p>
        </div>
    )
}

export default Post;
```

Lalu pada file ``App.js`` kita panggil komponen ``BlogPost.js`` kita.
```
import React from 'react';
import BlogPost from './components/BlogPost';

function App() {
  return (
    <div className="App">
      <header>
        <h2>Hashnode API</h2>
      </header>
      <div className="post">
        <BlogPost />
      </div>
    </div>
  );
}

export default App;

```

### Menambahkan Styling

Agar tampilan websitenya lebih cantik dan rapih kita tambahkan styling. Tambahkan kode css dibawah ini ke dalam file ``index.css``.

```
header {
  text-align: center;
}

/* card styling */
.card-wrapper {
  width: 90%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 60px auto;
}

.card {
  width: 350px;
  height: auto;
  border: 1px solid #bbbbbb;
  border-radius: 10px;
  padding: 15px;
  margin-bottom: 50px;
}

img {
  width: inherit;
  height: inherit;
  border-radius: 10px;
}

a {
  text-decoration: none;
  color: black;
}

.date {
  font-size: 12px;
  font-weight: bold;
}
```

Dan website yang kita buat sudah selesai.
![hashnode-api.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615381603527/DIyhZubSf.png)

***
Kalian bisa melihat live website nya  [disini](https://hashnode-api-reactjs.vercel.app/)  atau juga source code nya di  [github](https://github.com/juliansyahrifqi/hashnode-api-reactjs).

Terima kasih sudah membaca tulisan ini, jika ada masukkan dan pertanyaan bisa melalui kolom komentar. **See you next time!**
***

**Sumber:**
-  [https://engineering.hashnode.com/introducing-hashnode-graphql-api-public-beta-cjydzvp59001q2gs1b5zxaeaf](https://engineering.hashnode.com/introducing-hashnode-graphql-api-public-beta-cjydzvp59001q2gs1b5zxaeaf) 
-  [https://sandeep.dev/how-to-show-blog-posts-from-your-hashnode-blog-on-your-personal-site](https://sandeep.dev/how-to-show-blog-posts-from-your-hashnode-blog-on-your-personal-site) 
-  [https://api.hashnode.com/](https://api.hashnode.com/) 










