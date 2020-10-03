# Alpine.js

![npm bundle size](https://img.shields.io/bundlephobia/minzip/alpinejs)
![npm version](https://img.shields.io/npm/v/alpinejs)
[![Chat](https://img.shields.io/badge/chat-on%20discord-7289da.svg?sanitize=true)](https://alpinejs.codewithhugo.com/chat/)

Alpine.js menawarkan kepada Anda sifat reaktif dan deklaratif dari kerangka kerja 
besar seperti Vue atau React dengan biaya yang jauh lebih rendah.

Anda bisa mempertahankan DOM Anda, dan memercik dalam perilaku sesuai keinginan Anda.

Anggap saja seperti [Tailwind](https://tailwindcss.com/) untuk Javascript.

> Catatan: Sintaks framework ini hampir seluruhnya dipinjam dari [Vue](https://vuejs.org/) (dan dengan ekstensi [Angular](https://angularjs.org/)). Saya selamanya berterima kasih atas hadiah mereka kepada web.

## Terjemahan Dokumentasi

| Bahasa | Link dokumentasi |
| --- | --- |
| Bahasa Indonesia | [**Dokumentation in Bahasa Indonesia**](./README.id.md) |
| Cina Tradisional | [**繁體中文說明文件**](./README.zh-TW.md) |
| Jerman | [**Dokumentation in Deutsch**](./README.de.md) |
| Jepang | [**日本語ドキュメント**](./README.ja.md) |
| Rusia | [**Документация на русском**](./README.ru.md) |
| Portugis | [**Documentação em Português**](./README.pt.md) |
| Spanyol | [**Documentación en Español**](./README.es.md) |

## Instalasi

**Dari CDN:** Tambahkan script di bawah ini pada bagian akhir dari tag `<head>`.

```html
<script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.min.js" defer></script>
```

Hanya itu, dan ini akan melakukan persiapan (initialize) sendiri.

Untuk lingkunan produksi, disarankan untuk menentukan nomer versi tertentu pada link untuk menghindari kerusakan yang tidak terduga dari versi terbaru. Misalnya untuk menggunakan versi `2.7.0` (latest):

```html
<script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.7.0/dist/alpine.min.js" defer></script>
```

**Dari NPM:** Instalasi paket dari NPM.

```js
npm i alpinejs
```

Setelah itu, impor di dalam script anda.

```js
import 'alpinejs'
```

**Untuk menambahkan dukungan IE11** gunakan skrip berikut.

```html
<script type="module" src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.min.js"></script>
<script nomodule src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine-ie11.min.js" defer></script>
```

Pola di atas adalah [module/nomodule pattern](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/)
yang akan mengakibatkan bundel modern dimuat secara otomatis di peramban modern, dan bundel IE11 dimuat secara otomatis di IE11 dan peramban lawas lainnya.

## Penggunaan

*Dropdown/Modal*
```html
<div x-data="{ open: false }">
    <button @click="open = true">Open Dropdown</button>

    <ul
        x-show="open"
        @click.away="open = false"
    >
        Dropdown Body
    </ul>
</div>
```

*Tabs*
```html
<div x-data="{ tab: 'foo' }">
    <button :class="{ 'active': tab === 'foo' }" @click="tab = 'foo'">Foo</button>
    <button :class="{ 'active': tab === 'bar' }" @click="tab = 'bar'">Bar</button>

    <div x-show="tab === 'foo'">Tab Foo</div>
    <div x-show="tab === 'bar'">Tab Bar</div>
</div>
```

Anda bahkan dapat menggunakannya untuk hal-hal yang kompleks:
*Mengambil konten dropdown saat disentuh pointer (hover).*
```html
<div x-data="{ open: false }">
    <button
        @mouseenter.once="
            fetch('/dropdown-partial.html')
                .then(response => response.text())
                .then(html => { $refs.dropdown.innerHTML = html })
        "
        @click="open = true"
    >Show Dropdown</button>

    <div x-ref="dropdown" x-show="open" @click.away="open = false">
        Loading Spinner...
    </div>
</div>
```

## Belajar

Tedapat 14 direktif yang tersedia untuk anda:


| Direktif | Deskripsi |
| --- | --- |
| [`x-data`](#x-data) | Deklarasi sebuah batasan komponen baru. |
| [`x-init`](#x-init) | Jalankan sebuah ekspresi saat komponen dipersiapkan. |
| [`x-show`](#x-show) | Ganti `display: none;` pada elemen berdasarkan expresi (true atau false). |
| [`x-bind`](#x-bind) | Menetapkan nilai pada sebuah atribut dengan hasil ekspresi Javascript|
| [`x-on`](#x-on) | Memberikan sebuah event listener pada elemen. Menjalankan expresi javascript saat terjadi sebuah event.|
| [`x-model`](#x-model) | Menambahkan "data binding dua-arah" pada sebuah elemen. Menjaga agar elemen input tetap tersinkron dengan data komponen. |
| [`x-text`](#x-text) | Cara kerjannya mirip seperti `x-bind`, tapi akan mengubah teks (`innerText`) di dalam elemen. |
| [`x-html`](#x-html) | Cara kerjannya mirip seperti `x-bind`, tapi akan mengubah kode HTML (`innerHTML`) dari elemen. |
| [`x-ref`](#x-ref) | Cara mudah untuk mengambil elemen DOM yang berada di luar komponen. |
| [`x-if`](#x-if) | Hapus elemen sepenuhnya dari DOM. Harus digunakan pada tag `<template>`. |
| [`x-for`](#x-for) | Buat simpul DOM baru untuk setiap item dalam larik. Harus digunakan pada tag `<template>`. |
| [`x-transition`](#x-transition) | Direktif untuk menerapkan kelas ke berbagai tahapan transisi elemen |
| [`x-spread`](#x-spread) | Memungkinkan Anda mengikat objek direktif Alpine ke elemen agar dapat digunakan kembali dengan lebih baik |
| [`x-cloak`](#x-cloak) | Atribut ini dihapus saat Alpine menginisialisasi. Berguna untuk menyembunyikan DOM yang telah diinisialisasi sebelumnya. |

Dan ada 6 properti ajaib:

| Properti ajaib | Deskripsi |
| --- | --- |
| [`$el`](#el) |  Ambil simpul DOM komponen akar. |
| [`$refs`](#refs) | Ambil elemen DOM yang ditandai dengan `x-ref` di dalam komponen. |
| [`$event`](#event) | Mengambil objek "Event" native browser dalam event listener.  |
| [`$dispatch`](#dispatch) | Buat `CustomEvent` dan kirim menggunakan `.dispatchEvent() `secara internal. |
| [`$nextTick`](#nexttick) | Jalankan ekspresi tertentu SETELAH Alpine membuat pembaruan DOM reaktifnya. |
| [`$watch`](#watch) | Akan mengaktifkan callback yang diberikan saat properti komponen yang Anda "awasi" berubah. |


## Sponsor

<img width="33%" src="https://refactoringui.nyc3.cdn.digitaloceanspaces.com/tailwind-logo.svg" alt="Tailwind CSS">

**Ingin logo anda di sini? [Hubungi via Twitter](https://twitter.com/calebporzio)**

## Proyek Komunitas

* [AlpineJS Weekly Newsletter](https://alpinejs.codewithhugo.com/newsletter/)
* [Spruce (State Management)](https://github.com/ryangjchandler/spruce)
* [Turbolinks Adapter](https://github.com/SimoTod/alpine-turbolinks-adapter)
* [Alpine Magic Helpers](https://github.com/KevinBatdorf/alpine-magic-helpers)
* [Awesome Alpine](https://github.com/ryangjchandler/awesome-alpine)

### Direktif

---

### `x-data`

**Contoh:** `<div x-data="{ foo: 'bar' }">...</div>`

**Struktur:** `<div x-data="[object literal]">...</div>`

`x-data` untuk mendeklarasikan sebuah batasan komponen baru. Ini akan memberitahu framework untuk menyiapkan sebuah komponen baru dengan objek data yang diberikan.

Anggap saja seperti properti `data` dari komponen Vue.

**Ekstrak Logika Komponen**

Anda bisa mengekstrak data (dan perilaku) ke dalam fungsi yang bisa digunakan kembali:

```html
<div x-data="dropdown()">
    <button x-on:click="open">Open</button>

    <div x-show="isOpen()" x-on:click.away="close">
        // Dropdown
    </div>
</div>

<script>
    function dropdown() {
        return {
            show: false,
            open() { this.show = true },
            close() { this.show = false },
            isOpen() { return this.show === true },
        }
    }
</script>
```



> **Untuk pengguna bundler**, catat bahwa Alpine.js mengakses fungsi-fungsi yang berada di dalam global scope (`window`), Anda harus secara eksplisit menetapkan fungsi Anda ke `window` untuk menggunakannya dengan` x-data` misalnya `window.dropdown = function () {}` (ini karena dengan Webpack, Rollup, Parcel dll. ` function` yang Anda tetapkan akan default ke scope modul bukan `window`).

Anda juga dapat menggabungkan beberapa objek data menggunakan penghancuran objek (destructuring):

```html
<div x-data="{...dropdown(), ...tabs()}">
```

---

### `x-init`
**Contoh:** `<div x-data="{ foo: 'bar' }" x-init="foo = 'baz'"></div>`

**Struktur:** `<div x-data="..." x-init="[expression]"></div>`

`x-init` akan menjalankan sebuah ekspresi javascript saat komponen dipersiapkan.

Jika Anda ingin menjalankan kode SETELAH Alpine selesai membuat pembaruan awal ke DOM (sesuatu seperti hook `mount ()` di VueJS), Anda dapat mengembalikan callback dari `x-init`, dan itu akan dijalankan setelah:

`x-init="() => { // we have access to the post-dom-initialization state here // }"`

---

### `x-show`
**Contoh:** `<div x-show="open"></div>`

**Struktur:** `<div x-show="[expression]"></div>`

`x-show` mengubah style `display: none; ` pada elemen bergantung pada apakah ekspresi ditetapkan ke `true` atau `false`.

**x-show.transition**

`x-show.transition` adalah API praktis untuk membuat `x-show` Anda lebih menyenangkan menggunakan transisi CSS.

```html
<div x-show.transition="open">
    These contents will be transitioned in and out.
</div>
```

| Direktif | Deskripsi |
| --- | --- |
| `x-show.transition` | Fade dan skala simultan. (opasitas, skala: 0,95, fungsi waktu: kubik-bezier (0,4, 0,0, 0,2, 1), durasi masuk: 150 md, durasi habis: 75 md)
| `x-show.transition.in` | Hanya transisi masuk. |
| `x-show.transition.out` | Hanya transisi keluar |
| `x-show.transition.opacity` | Gunakan fade saja. |
| `x-show.transition.scale` | Gunakan skala saja |
| `x-show.transition.scale.75` | Kustomisasi transformasi skala CSS `transform: scale(.75)`. |
| `x-show.transition.duration.200ms` | Setel transisi "masuk" ke 200ms. Keluaran akan disetel menjadi setengahnya (100 md). |
| `x-show.transition.origin.top.right` | Sesuaikan asal transformasi CSS `transform-origin: top right`. |
| `x-show.transition.in.duration.200ms.out.duration.50ms` | Durasi berbeda untuk "masuk" dan "keluar". |

> Catatan: Semua pengubah transisi ini dapat digunakan bersama satu sama lain. Ini dimungkinkan (meskipun konyol lol): `x-show.transition.in.duration.100ms.origin.top.right.opacity.scale.85.out.duration.200ms.origin.bottom.left.opacity.scale. 95`

> Catatan: `x-show` akan menunggu sampai setiap anak menyelesaikan transisi keluar. Jika Anda ingin mengabaikan perilaku ini, tambahkan modifer `.immediate`:
```html
<div x-show.immediate="open">
    <div x-show.transition="open">
</div>
```
---

### `x-bind`

> Note: You are free to use the shorter ":" syntax: `:type="..."`

> Catatan: Anda bebas menggunakan sintaks ":" yang lebih pendek: `:type=" ... "`

**Contoh:** `<input x-bind:type="inputType">`

**Struktur:** `<input x-bind:[attribute]="[expression]">`

`x-bind` menetapkan nilai atribut ke hasil ekspresi JavaScript. Ekspresi memiliki akses ke semua kunci dari objek data komponen, dan akan diperbarui setiap kali datanya diperbarui.

> Catatan: Binding atribut HANYA diperbarui ketika dependensinya diperbarui. Framework ini cukup pintar untuk mengamati perubahan data dan mendeteksi binding mana yang mempedulikannya.

**`x-bind` untuk atribut class**

`x-bind` punya perilaku sedikit berbeda saaat melakukan binding dengan atribut `class`.

Untuk kelas, Anda meneruskan objek yang kuncinya adalah nama kelas, dan nilai adalah ekspresi boolean untuk menentukan apakah nama kelas tersebut diterapkan atau tidak.

Untuk nama class yang anda berikan sebagai key dari object dan nilainya berupa ekspresi boolean, maka ini akan menentukan class ini digunakan atau tidak.

Sebagai contoh:
`<div x-bind:class="{ 'hidden': foo }"></div>`

Pada contoh ini, class `"hidden"` hanya akan dipakai saat nilai dari data atribut `foo` adalah `true`

**`x-bind` untuk atribut boolean**

`x-bind` mendukung atribut boolean dengan cara yang sama seperti atribut nilai, yakni dengan menggunakan sebuah varibel sebagai kondisi ekspresi javascript dimana nilainya akan `true` dan `false.

`x-bind` supports boolean attributes in the same way as value attributes, using a variable as the condition or any JavaScript expression that resolves to `true` or `false`.

Sebagai contoh:
```html
<!-- diberkan: -->
<button x-bind:disabled="myVar">Click me</button>

<!-- Saat myVar == true: -->
<button disabled="disabled">Click me</button>

<!-- Saat myVar == false: -->
<button>Click me</button>
```

Ini akan menambahkan atribut `disabled` ketika `myVar` bernilai `true` dan sebaliknya akan menghapus atribut `disabled` ketika `myVar` bernilai `false`.

Atribut boolean yang didukung sama seperti [HTML specification](https://html.spec.whatwg.org/multipage/indices.html#attributes-3:boolean-attribute), contohnya `disabled`, `readonly`, `required`, `checked`, `hidden`, `selected`, `open`, dll.


> Catatan: Jika Anda memerlukan status false untuk ditampilkan untuk atribut Anda, seperti `aria-*`, chain` .toString()` ke nilai saat mengikat ke atribut. Misalnya: `:aria-expanded="isOpen.toString()"` akan tetap ada apakah `isOpen` adalah `true` atau` false`.

**modifier `.camel`**
**Contoh:** `<svg x-bind:view-box.camel="viewBox">`

Modifier `camel` akan mengikat ke huruf besar/kecil yang setara dengan nama atribut. Pada contoh di atas, nilai `viewBox` akan terikat pada atribut` viewBox` sebagai lawan dari atribut `view-box`.

---

### `x-on`

> Catatan: Anda bebas menggunakan singkatan "@" untuk sintak yang lebih singkat: `@click="..."`

**Contoh:** `<button x-on:click="foo = 'bar'"></button>`

**Struktur:** `<button x-on:[event]="[expression]"></button>`

`x-on` memberikan event listener ke elemen tempat dideklarasikan. Saat terjadi sebuah event, maka nilainya akan diberikan dari ekspresi javascript yang dieksekusi.

Jika ada data yang dimodifikasi pada ekspresi, meka elemen yang sudah "mengikat" terhadap data tersebut akan diupdate.

> Catatan: Anda juga bisa memberikan dengan nama fungsi javascript

**Contoh:** `<button x-on:click="myFunction"></button>`

Ini sama seperti: `<button x-on:click="myFunction($event)"></button>`

**modifier `keydown`**

**Contoh:** `<input type="text" x-on:keydown.escape="open = false">`

Anda bisa menentukan tombol tertentu untuk dipantau dengan menggunakan modifier keydown dengan cara menambahkan direktif tombol di belakang `x-on:keydown`. Perlu dicatat, modifier ini menggunakan kebab-case yang merupakan versi dari nilai `Event.key`.

Contoh: `enter`, `escape`, `arrow-up`, `arrow-down`

> Catatan: Anda juga bisa memantau tombol kombinasi dari modifier-sistem seperti: `x-on:keydown.cmd.enter="foo"`

**Modifier `.away`**

**Contoh:** `<div x-on:click.away="showModal = false"></div>`

Ketika diberikan modifier `.away`, maka event handler hanya akan diekseskusi saat event berasal dari selain dirinya sendiri (elemen itu sendiri) dan turunannya (elemen anaknya).

Ini sangat berguna untuk menghilangkan dropdown dan modal saat user melakukan klik di luar dari elemen.

**modifier `.prevent`**
**Contoh:** `<input type="checkbox" x-on:click.prevent>`

Menambahakn modifier `.prevent` pada event listener akan memanggil `preventDefault` saat terjadi sebuah event. Pada contoh di atas, checkbox tidak akan dicek saat user melakukan klik.

**modifier `.stop`**
**Contoh:** `<div x-on:click="foo = 'bar'"><button x-on:click.stop></button></div>`

Menambahkan modifier `.stop` pada event listener akan memanggil `stopPropagation` saat terjadi sebuah event. Pada contoh di atas, artinya event "click" tidak akan mengembang dari tombol ke `<div>`, atau dengan katalain saat user klik tombol, maka `foo` tidak akan diset nilainya menjadi `bar`.


**modifier `.self`**
**Contoh:** `<div x-on:click.self="foo = 'bar'"><button></button></div>`

Menambahkan modifier `.self` pada sebuah event listener maka hanya akan memicu event handler jika `$event.target` adalah elemen itu sendiri. Pada contoh di atas, event "click" yang dari elemen tombol tidak akan menjalankan event handler pada `<div>`.

**modifier `.window`**
**Contoh:** `<div x-on:resize.window="isOpen = window.outerWidth > 768 ? false : open"></div>`

Menambahkan modifier `.window` pada event listener akan memasang event listener pada objek global window bukan pada elemen DOM yang menjadi tempat event tersebut dideklarasikan. Ini sangat berguna saat anda ingin memodifikasi state dari komponen ketika ada perubahan pada jendela browser, misalnya seperti event resize. Pada contoh di atas, saat jendela browser diperbesar menjadi 768 piksel, maka kita akan menutup modal/dropdown, selain dari itu akan tetap dibuka.


>Catatan: Anda juga bisa menggunakan modifier `.document` untuk menambahkan event listener pada dokumen


**modifier `.once`**
**Contoh:** `<button x-on:mouseenter.once="fetchSomething()"></button>`

Menambahkan modifier `.once` pada event listener akan membuat event listerner hanya akan memantau event satu kali saja. Ini sangat berguna pada sesuatu yang dilakukan sekali saja, seperti meload partial (bagian) dari HTML, dan sebagainya.

**modifier `.passive`**
**Contoh:** `<button x-on:mousedown.passive="interactive = true"></button>`

Menambahkan modifier `.passive` pada event listener akan membuat event literner menjadi pasif, yang artinya `preventDefault()` tidak akan bekerja pada event yang sedang diproses. Ini tentunya bisa membantu meningkatkan performa, contohnya pada scoll di perangkat layar sentuh.


**Modifier `.debounce`**
**Contoh:** `<input x-on:input.debounce="fetchSomething()">`

Modifier `.debounce` memungkinkan anda untuk "debounce" sebuah event handler. Dengan kata lain, event handler TIDAK akan dijalankan sampai batas waktu tertentu.

Nilai default debounce untuk "menunggu" adalah 250 milidetik.

Jika anda ingin mengubahnya, anda hyga bisa menentukan waktu tunggu secara spesifik:

```html
<input x-on:input.debounce.750="fetchSomething()">
<input x-on:input.debounce.750ms="fetchSomething()">
```

**modifier `.camel`**
**Contoh:** `<input x-on:event-name.camel="doSomething()">`

Modifier `camel` akan melampirkan pemroses event untuk nama event yang setara dengan camelCase. Dalam contoh di atas, ekspresi akan dievaluasi ketika event `eventName` diaktifkan pada elemen.


---

### `x-model`
**Contoh:** `<input type="text" x-model="foo">`

**Struktur:** `<input type="text" x-model="[data item]">`

`x-model` akan menambahkan "binding data dua arah" (two-way binding) pada sebauh elemen. Dengan kata lain, nilai dari elemen input akan tetap disinkronkan dengan nilai dari data item pada komponen.

> Catatan: `x-model` merupakan cara yang cukup cerdas untuk mendeteksi perubahan pada input text, checkbox, radio button, textarea, select, dan multiple select. Perilakunya sama seperti [cara yang dilakukan Vue](https://vuejs.org/v2/guide/forms.html) dalam skenario ini.



**Modifier `.number`**
**Contoh:** `<input x-model.number="age">`

Modifier `.number` akan mengubah nilai input menjadi angka. Jika nilai tidak bisa diubah menjadi angka yang valid, maka nilainya akan dibiarkan dengan nilai yang diinputkan.


**Modifier `.debounce`**
**Contoh:** `<input x-model.debounce="search">`

Modifier `debounce` memungkinkan anda untuk menambahkan "debounce" pada update nilai. Dengan kata lain, event handler TIDAK akan dijalankan sampai batas waktu yang ditentukan.

Nilai default pada waktu "tunggu" debounce adalah 250 milidetik.

Jika anda ingin mengubahnya, anda juga bisa menentukan waktu tunggu secara spresifik:


```html
<input x-model.debounce.750="search">
<input x-model.debounce.750ms="search">
```

---

### `x-text`
**Contoh:** `<span x-text="foo"></span>`

**Struktur:** `<span x-text="[expression]"`

`x-text` cara kerjannya mirip seperti `x-bind`, bedanya `x-text` akan melakukan update teks (`innerText`) pada sebuah elemen. Sedangkan `x-bind` hanya mengupdate nilai atribut.

---

### `x-html`
**Contoh:** `<span x-html="foo"></span>`

**Struktur:** `<span x-html="[expression]"`

`x-html` cara kerjannya mirip seperti `x-bind`, bedanya `x-html` akan mengupdate kode HTML pada elemen sedangkan `x-bind` mengupdate nilai atribut.


> :warning: **Jangan pernah gunakan pada konten yang berasal dari user.** :warning:
>
> Karena rendering HTML secara dinamis dari pihak ketiga berpotensi rentan terhadap serangan [XSS](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting).


---

### `x-ref`
**Contoh:** `<div x-ref="foo"></div><button x-on:click="$refs.foo.innerText = 'bar'"></button>`

**Struktur:** `<div x-ref="[ref name]"></div><button x-on:click="$refs.[ref name].innerText = 'bar'"></button>`

`x-ref` menyediakan cara yang mudah untuk mendapatkan elemen DOM dari luar komponen. Dengan memberikan atribut `x-ref` pada elemen, maka ini akan membuatnya bisa diakses dari semua event handler dari objek `$refs`.

Ini sangat berguna sebagai alternative penggunaan id dan `document.querySelector` pada semua tempat.


> Catatan: anda juga bisa memberikan nilai dinamis pada x-ref: `<span :x-ref="item.id"></span>` jika anda menginginkannya.


---

### `x-if`
**Contoh:** `<template x-if="true"><div>Some Element</div></template>`

**Struktur:** `<template x-if="[expression]"><div>Some Element</div></template>`

Untuk kasus di mana `x-show` tidak cukup (`x-show` menyetel elemen ke `display: none` jika salah),` x-if` dapat digunakan untuk benar-benar menghapus elemen sepenuhnya dari DOM.

Penting bahwa `x-if` digunakan pada tag` <template></template>` karena Alpine tidak menggunakan DOM virtual. Implementasi ini memungkinkan Alpine untuk tetap bertahan dan menggunakan DOM asli untuk melakukan keajaibannya.

> Catatan: `x-if` harus punya sebuah elemen root di dalam tag `<template></template>`.

> Catatan: Saat menggunakan `template` di tag `svg`, anda harus menambahkan sebuah [polyfill](https://github.com/alpinejs/alpine/issues/637#issuecomment-654856538) yang akan dijalankan sebelum Alpine.js dipersiapkan.


---

### `x-for`
**Contoh:**
```html
<template x-for="item in items" :key="item">
    <div x-text="item"></div>
</template>
```

> Catatan: binding `:key` bersifat opsional, tapi SANGAT disarankan menggunakannya.

`x-for` bisa digunakan untuk kasus ketika anda ingin membuat elemen DOM baru dari setiap item pada array. Ini mirip seperti `v-for` pada Vue, namun dengan satu perbedaan yakni menggunakan tag `template`, bukan elemen DOM biasa.

Jika anda ingin mengakses current index dari iterasi, gunakan sintaks berikut:


```html
<template x-for="(item, index) in items" :key="index">
    <!-- You can also reference "index" inside the iteration if you need. -->
    <div x-text="index"></div>
</template>
```

Jika anda ingin mengakses objek dari array (collection) dari iterasi, gunakan sintaks berikut:

```html
<template x-for="(item, index, collection) in items" :key="index">
    <!-- You can also reference "collection" inside the iteration if you need. -->
    <!-- Current item. -->
    <div x-text="item"></div>
    <!-- Same as above. -->
    <div x-text="collection[index]"></div>
    <!-- Previous item. -->
    <div x-text="collection[index - 1]"></div>
</template>
```

> Catatan: `x-for` harus punya sebauh elemen root di dalam tag `<template></template>`.

> Catatan: Saat menggunakan `template` di tag `svg`, anda harus menambahkan sebuah [polyfill](https://github.com/alpinejs/alpine/issues/637#issuecomment-654856538) yang akan dijalankan sebelum Alpine.js dipersiapkan.

#### Perulangan bersarang dengan `x-for`

Anda bisa membuat perulangan bersarang dengan `x-for`, namun anda HARUS membungkus tiap perulangan dengan elemen. Sebagai contoh:


```html
<template x-for="item in items">
    <div>
        <template x-for="subItem in item.subItems">
            <div x-text="subItem"></div>
        </template>
    </div>
</template>
```

#### Iterasi dengan range

Alpine mendukung sintak `i in n`, dimana `n` adalah sebuah integer yang akan memungkinkan anda untuk melakukan iterasi pada elemen berdasarkan range yang sudah ditentukan.

```html
<template x-for="i in 10">
    <span x-text="i"></span>
</template>
```

---

### `x-transition`
**Contoh:**
```html
<div
    x-show="open"
    x-transition:enter="transition ease-out duration-300"
    x-transition:enter-start="opacity-0 transform scale-90"
    x-transition:enter-end="opacity-100 transform scale-100"
    x-transition:leave="transition ease-in duration-300"
    x-transition:leave-start="opacity-100 transform scale-100"
    x-transition:leave-end="opacity-0 transform scale-90"
>...</div>
```

```html
<template x-if="open">
    <div
        x-transition:enter="transition ease-out duration-300"
        x-transition:enter-start="opacity-0 transform scale-90"
        x-transition:enter-end="opacity-100 transform scale-100"
        x-transition:leave="transition ease-in duration-300"
        x-transition:leave-start="opacity-100 transform scale-100"
        x-transition:leave-end="opacity-0 transform scale-90"
    >...</div>
</template>
```

> Contoh di atas menggunakan class CSS dari [Tailwind CSS](https://tailwindcss.com)

Alpine memberikan 6 macam direktif transisi untuk menggunakan class tertentu pada tiap tahapan dalam transisi elemen antara state "hidden" dan "shown". Direktif ini dapat bekerja dengan `x-show` dan `x-if`.

Perilakunya benar-benar sama seperti transisi pada VueJS, namun punya beberapa perbedaan dengan nama yang lebih masuk akal:

| Direktif | Deksripsi |
| --- | --- |
| `:enter` | Digunakan selama dalam pase entering. |
| `:enter-start` | Ditambahkan sebelum elemen dimasukkan, dihapus satu frame setelah elemen dimasukkan. |
| `:enter-end` | Menambahkan satu frame setelah elemen dimasukkan (pada saat yang sama `enter-start` dihapus), dihapus ketika transisi/animasi selesai.
| `:leave` | Digunakan sllama dalam pase leaving. |
| `:leave-start` | Ditambahkan segera saat transisi leaving dipicu, hapus satu frame setelahnya |
| `:leave-end` | Menambahkan satu frame setelah transisi leave dipicu (pada saat yang sama `leave-start` dihapus), dihapus ketika transisi/animasi selesai.

---

### `x-spread`
**Contoh:**
```html
<div x-data="dropdown()">
    <button x-spread="trigger">Open Dropdown</button>

    <span x-spread="dialogue">Dropdown Contents</span>
</div>

<script>
    function dropdown() {
        return {
            open: false,
            trigger: {
                ['@click']() {
                    this.open = true
                },
            },
            dialogue: {
                ['x-show']() {
                    return this.open
                },
                ['@click.away']() {
                    this.open = false
                },
            }
        }
    }
</script>
```


`x-spread` memungkinkan anda untuk mengekstrak sebuah elmen dari binding Alpine menjadi objek yang bisa digunakan kembali.

Key dari object adalah direktif (bisa direktif dan juga modifier) dan nilainya merupakan callback yang akan dievaluasi oleh Alpine.

> Catatan: Ada beberapa peringatan untuk x-spread:
> - Saat "spread" dilakukan pada direktif `x-for`, anda harus mengembalikan string dari callback. Sebagai contoh: `['x-for']() { return 'item in items' }`.
> - `x-data` dan `x-init` tidak bisa digunakan di dalam objek "spread"

---

### `x-cloak`
**Contoh:** `<div x-data="{}" x-cloak></div>`

Atribut `x-cloak` adalah elemen yang dihapus saat Aplpine dipersiapkan. Ini berguna untuk menyembunikan DOM yang sudah dipersiapkan sebelumnya. Biasanya untuk menambahkan style global seperti ini:


```html
<style>
    [x-cloak] { display: none; }
</style>
```

### Properti Ajaib

> Properti ajaib **tidak tersedia di dalam** `x-data` jika komponen belum di-initialize, kecuali `$el`.


---

### `$el`
**Contoh:**
```html
<div x-data>
    <button @click="$el.innerHTML = 'foo'">Replace me with "foo"</button>
</div>
```

`$el` adalah properti ajaib yang bisa digunakan untuk mendapatkan root komponen dari node DOM.

### `$refs`
**Contoh:**
```html
<span x-ref="foo"></span>

<button x-on:click="$refs.foo.innerText = 'bar'"></button>
```

`$refs` adalah properti ajaib yang bisa digunakan untuk mendapatkan elemen DOM yang memiliki atribut `x-ref` di dalam komponen. Ini sangat berguna ketika anda ingin memanupulasi DOM secara manual.

---

### `$event`
**Contoh:**
```html
<input x-on:input="alert($event.target.value)">
```

`$event` adalah properti ajaib yang bisa digunakan dengan event listener untuk mendapatkan objek "Event" dari browser.

> Catatan: Properti $event hanya tersedia pada eksrepsi DOM.

Jika anda ingin mengakses `$event` di dalam fungsi Javascript, maka anda harus memasukannya sebagai parameter:

`<button x-on:click="myFunction($event)"></button>`

---

### `$dispatch`
**Contoh:**
```html
<div @custom-event="console.log($event.detail.foo)">
    <button @click="$dispatch('custom-event', { foo: 'bar' })">
    <!-- When clicked, will console.log "bar" -->
</div>
```

**Catatan saat propagasi event**

Perhatikan, karena ada [event bubbling](https://en.wikipedia.org/wiki/Event_bubbling) dan saat anda ingin mendapatkan event yang bersumber dari node dalam satu sarang (same nesting hirarchy) maka sebaiknya anda gunakan modifier [`.window`](https://github.com/alpinejs/alpine#x-on).


**Contoh:**

```html
<div x-data>
    <span @custom-event="console.log($event.detail.foo)"></span>
    <button @click="$dispatch('custom-event', { foo: 'bar' })">
<div>
```

> Ini tidak akan bekerja karena saat `custom-event` terjadi, maka propagasi akan dikirim ke `div`.

**Pengiriman ke Komponen**

Anda juga bisa memanfaatkan teknik sebelumnya unuk membuat kompoen bisa saling berbicara.

**Contoh:**

```html
<div x-data @custom-event.window="console.log($event.detail)"></div>

<button x-data @click="$dispatch('custom-event', 'Hello World!')">
<!-- When clicked, will console.log "Hello World!". -->
```

`$dispatch` adalah sebuah singkatan untuk membuat `CustomEvent` dan mengirimkannya dengan `dispatchEvent()` secara internal. Ada banyak studi kasus penggunaan passing data antara komponen dengan custom event. [Silahkan baca di sini](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events) untuk informasi lebih lanjut tentang cara kerja `CustomEvent` di browser.

Anda akan melihat bahwa setiap data yang diteruskan sebagai parameter kedua pada `$ dispatch('some-event', {some: 'data'})`, akan tersedia pada properti "detail" di event baru: `$event.detail.some`. Melampirkan data event khusus ke properti `.detail` adalah praktik standar untuk `CustomEvents` di browser. Baca [di sini](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/detail) untuk info lebih lanjut.

Anda juga bisa menggunakan `$dispatch()` untuk memicu update data untuk binding `x-model`. Contohnya:

```html
<div x-data="{ foo: 'bar' }">
    <span x-model="foo">
        <button @click="$dispatch('input', 'baz')">
        <!-- After the button is clicked, `x-model` will catch the bubbling "input" event, and update foo to "baz". -->
    </span>
</div>
```

> Catatan: Properti $dispatch hanya tersedia pada eksrepsi DOM.

Jika anda ingin mengakses `$dispatch` di dalam fungsi javascript, anda bisa meneruskannya secara langsung melalui parameter:

`<button x-on:click="myFunction($dispatch)"></button>`

---

### `$nextTick`
**Contoh:**
```html
<div x-data="{ fruit: 'apple' }">
    <button
        x-on:click="
            fruit = 'pear';
            $nextTick(() => { console.log($event.target.innerText) });
        "
        x-text="fruit"
    ></button>
</div>
```

`$nextTick` adalah properti ajaib yang memungkinkan anda untuk mengeksekusi hanya ekpresi yang diberikan SETELAH Alpine melakukan DOM update secara reaktif. Ini berguna pada saat Anda ingin berinteraksi dengan status DOM SETELAH itu tercermin setiap pembaruan data yang Anda buat.

---

### `$watch`
**Contoh:**
```html
<div x-data="{ open: false }" x-init="$watch('open', value => console.log(value))">
    <button @click="open = ! open">Toggle Open</button>
</div>
```

Anda bisa "mengawasi" sebuah properti dari komponen dengan method ajaib `$watch`. Pada contoh di atas, saat tombol diklik dan nilai `open` berubah, maka callback akan dijalankan dengan mengeksekusi `console.log` dengan nilai yang baru.

## Keamanan

Jika anda menemukan celah keamanan, mohon untuk mengirim email ke [calebporzio@gmail.com]()

Alpine mengandalkan implementasi kustom yang menggunakan objek `Function` untuk mengevaluasi arahannya. Meskipun lebih aman daripada `eval()`, penggunaannya dilarang di beberapa lingkungan, seperti Aplikasi Google Chrome, menggunakan Kebijakan Keamanan Konten (CSP) yang terbatas.

Jika Anda menggunakan Alpine di situs web yang berurusan dengan data sensitif dan membutuhkan [CSP](https://csp.withgoogle.com/docs/strict-csp.html), Anda perlu memasukkan `unsafe-eval` dalam kebijakan Anda. Kebijakan kuat yang dikonfigurasi dengan benar akan membantu melindungi pengguna Anda saat menggunakan data pribadi atau keuangan.

Karena kebijakan berlaku untuk semua skrip di halaman Anda, penting agar pustaka eksternal lain yang termasuk dalam situs web ditinjau dengan cermat untuk memastikan bahwa mereka dapat dipercaya dan tidak akan menimbulkan kerentanan Cross Site Scripting baik menggunakan fungsi `eval()` atau memanipulasi DOM untuk memasukkan kode berbahaya ke halaman Anda.

## Roadmap V3
* Move from `x-ref` to `ref` for Vue parity?
* Add `Alpine.directive()`
* Add `Alpine.component('foo', {...})` (With magic `__init()` method)
* Dispatch Alpine events for "loaded", "transition-start", etc... ([#299](https://github.com/alpinejs/alpine/pull/299)) ?
* Remove "object" (and array) syntax from `x-bind:class="{ 'foo': true }"` ([#236](https://github.com/alpinejs/alpine/pull/236) to add support for object syntax for the `style` attribute)
* Improve `x-for` mutation reactivity ([#165](https://github.com/alpinejs/alpine/pull/165))
* Add "deep watching" support in V3 ([#294](https://github.com/alpinejs/alpine/pull/294))
* Add `$el` shortcut
* Change `@click.away` to `@click.outside`?

## Lisensi

Copyright © 2019-2020 Caleb Porzio and contributors

Licensed under the MIT license, see [LICENSE.md](LICENSE.md) for details.
