---
title: Apa itu VueJS?
date: 2019-01-06 20:45:51
tags:
---

# Prasyarat
1. Pengetahuan dan pengalaman tentang [dasar-dasar HTML](https://www.w3schools.com/html/html_basic.asp)
2. Pengetahuan dan pengalaman tentang [Web Server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server)
3. Pengetahuan dan pengalaman tentang [dasar-dasar Javascript](http://rachelnabors.com/javascript-for-designers/)

# Tools
1. [Visual Studio Code (ver 1.30.1)](https://code.visualstudio.com/)
2. [Fenix Web Server (ver 2.0)](http://fenixwebserver.com/) or [live-server](https://www.npmjs.com/package/live-server)

# Apa itu VueJS?

VueJS posisinya seperti jQuery. Berfungsi sebagai DOM Manipulator.
Berbeda dengan jQuery, dimana jQuery merupakan library sedangkan VueJS adalah framework.

Bertujuan untuk menyediakan fitur-fitur yang belum disediakan oleh vanilla javascript.
Vanilla Javascript adalah istilah untuk javascript bawaan browser.

# Mengapa VueJS?
VueJS lahir karena sebuah kebutuhan. Kebutuhan untuk membuat custom User Interface (UI). Untuk bisa memahami kebutuhan yang dimaksud, kembali pada bentuk dasar web.

# Bagaimana bentuk dasar web?
Bentuk dasar web adalah berupa sebuah file dan sebuah aplikasi.
File yang dimaksud adalah file yang berakhiran .html.
Aplikasi yang dimaksud adalah browser.

berikut adalah contoh dasar file .html

```html index.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Hello</title>
</head>
<body>
  <div>
    Hello World
  </div>
</body>
</html>
```

Ini adalah bentuk dasar dari file .html. Salah satu ciri khasnya adalah tidak ada perubahan dari isi selama file ini ditampilkan di browser. Meskipun di reload bolak-balik.

Sifat ini disebut dengan halaman statis.

Dengan adanya javascript, sebuah file html bisa berubah menjadi dinamis. Contohnya kode berikut
```html current-time.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Hello</title>
</head>
<body>
  <div>
    Hello World<br>
    Current Time is <span id="current-time"></span>
  </div>
  <script>
    function getCurrentTime() {
      return (new Date()).toUTCString()
    }

    document.getElementById('current-time')
        .innerHTML = getCurrentTime()
  </script>
</body>
</html>
```

Di kode ini, digunakan elemen html `span` untuk menampilkan "current time". berpasangan dengan perintah html `innerHTML`. Jadi di sini, Javascript digunakan untuk meng-update html element, biasanya disebut juga dengan DOM element.

setiap kali di-reload, file ini akan menampilkan perubahan konten sesuai perubahan waktu.

dengan javascript, tampilan waktu bisa di-update sesuai interval tertentu tanpa harus reload browser.
berikut contoh kodenya
```html current-time-auto-update.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Hello</title>
</head>
<body>
  <div>
    Hello World
    Current Time is <span id="current-time"></span>
  </div>
  <script>
    function getCurrentTime() {
      return (new Date()).toUTCString()
    }

    setInterval(() => {
      document.getElementById('current-time')
        .innerHTML = getCurrentTime()
    }, 1000)
  </script>
</body>
</html>
```

Di kondisi kode seperti inilah, VueJS bisa memberikan bentuk kode dari perspektif yang berbeda, seperti kode berikut
```html current-time-vuejs.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Hello</title>
</head>
<body>
  <div id="app">
    Hello World
    Current Time is <span>{{ currentTime }}</span>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    function getCurrentTime() {
      return (new Date()).toUTCString()
    }

    const app = new Vue({
      el: '#app',
      data: {
        currentTime: getCurrentTime()
      },
      methods: {
        updateTime: function() {
          this.currentTime = getCurrentTime()
        } 
      }
    })

    setInterval(() => {
      app.updateTime()
    }, 1000)
  </script>
</body>
</html>
```

secara hasil atau tampilan di browser, keduanya menghasilkan output yang sama.
perbedaannya terdapat pada kode yang dibuat. perhatikan saat kedua kode tersebut dibandingkan.

![komparasi js dengan vuejs](js-vs-vuejs.png)
Secara jumlah baris menjadi lebih banyak. Tapi secara struktur pemograman, bagian-bagian dari kode tersebut terlihat lebih jelas. Kodenya menjadi lebih mudah dipahami.

Dengan struktur yang baru ini, terdapat cara baca dari kode tersebut seperti gambar berikut.
![cara baca vuejs](cara-baca-vuejs.png)
Cara bacanya sebagai berikut:
1. VueJS diminta untuk mengelola DOM element dengan id `app`.
  pemilihan DOM element dilakukan menggunakan CSS Selector, dalam hal ini `#app`. yang artinya, apapun yang ada di dalam element tersebut diperlakukan sebagai template.
2. Salah satu elemen dari `#app` berisi elemen `span` yang memiliki konten berupa teks dengan dua kurawal keriting (curly braces). Ini artinya, Vue diminta untuk menampilkan isi dari data `currentTime`
3. Terdapat perintah `app.updateTime()`. `app` di sini merujuk pada elemen yang dikelola oleh Vue. Dan melalui `app`, fungsi apapun yang tertulis di `methods` bisa dipanggil.
4. Fungsi yang didaftarkan di `methods` bisa digunakan untuk mengutak-atik data dengan menggunakan perintah `this`. Dalam hal ini `this.currentTime` merujuk pada variable `currentTime` pada bagian `data`.

4 hal tersebut mewakili konsep [enkapsulasi][encapsulation], yaitu mengumpulkan beberapa hal yang berbeda tapi berkaitan dalam satu tempat. Ada 3 hal utama yang dikumpulkan oleh VueJS, yaitu `el`, `data`, dan `methods`. Hal ini mewakili 3 bagian utama dari halaman web ini. `el` mewakili DOM element dalam hal ini ada element dengan id `app` yang diwakili oleh css selector `#app`. Data mewakili data yang bisa ditampilkan di html. Diwakili oleh `{% raw %}{{ currentTime }}{% endraw %}` pada html. Bentuk `{% raw %}{{ }}{% endraw %}` merupakan perintah untuk menampilkan data. Adanya bentuk ini merupakan ciri khas dari [HTML template][html-templating] Kemudian ada `methods` dalam hal ini adalah fungsi `updateTime`.

[html-templating]: https://colorlib.com/wp/top-templating-engines-for-javascript/
[encapsulation]: https://www.thoughtco.com/data-encapsulation-2034263