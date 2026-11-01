# GeoSVG

Pustaka JavaScript ringan untuk menggambar **bangun geometri** sebagai SVG di browser: titik, garis, segitiga, lingkaran, sudut, grafik fungsi, sumbu koordinat, dan lainnya. Tanpa dependensi, satu file, dan hasilnya bisa diunduh sebagai **SVG** atau **PNG**.

> **Versi saat ini: `0.1.0`** (tahap awal). API masih bisa berubah sebelum `1.0.0`.

---

## Daftar Isi

- [Fitur](#fitur)
- [Instalasi](#instalasi)
- [Mulai Cepat](#mulai-cepat)
- [Membuat Kanvas](#membuat-kanvas)
- [Sistem Koordinat (`yUp`)](#sistem-koordinat-yup)
- [Cara Menulis Titik](#cara-menulis-titik)
- [Opsi Gaya Umum](#opsi-gaya-umum)
- [Daftar Method](#daftar-method)
- [Label](#label)
- [LaTeX](#latex)
- [Ekspor SVG dan PNG](#ekspor-svg-dan-png)
- [Lapisan, Grup, dan Batch](#lapisan-grup-dan-batch)
- [Penanganan Error](#penanganan-error)
- [Aksesibilitas](#aksesibilitas)
- [Alias Opsi (Indonesia dan Inggris)](#alias-opsi-indonesia-dan-inggris)
- [Kompatibilitas](#kompatibilitas)

---

## Fitur

- Satu file, **tanpa dependensi**
- Bentuk dasar sampai lanjutan: titik, garis, sinar, segmen, vektor, poligon, lingkaran, elips, busur, juring, tembereng, kurva Bézier, dan grafik fungsi
- **Konstruksi geometri**: garis sejajar, garis tegak lurus, garis bagi, titik potong, lingkaran luar dan dalam segitiga, garis singgung, refleksi
- Sumbu koordinat, grid, garis dimensi, tanda sama panjang, sudut, dan sudut siku-siku
- Mode **`yUp`**: sumbu Y mengarah ke atas, seperti buku matematika
- Label teks dan rumus **LaTeX** (butuh KaTeX atau MathJax)
- Ekspor ke **SVG** dan **PNG**
- Dukungan aksesibilitas (judul, deskripsi, dan penanda gambar hiasan)
- Nama method dan opsi berbahasa Indonesia, dengan alias bahasa Inggris untuk sebagian opsi
- Bisa dipakai lewat `<script>`, AMD (RequireJS), maupun `require` di bundler

---

## Instalasi

### Lewat CDN (jsDelivr)

Versi biasa:

```html
<script src="https://cdn.jsdelivr.net/gh/otakbiru/GeoSVG/GeoSVG.js"></script>
```

Versi minimize (lebih kecil, cocok untuk produksi):

```html
<script src="https://cdn.jsdelivr.net/gh/otakbiru/GeoSVG/GeoSVG.min.js"></script>
```

> **Tip:** alamat di atas selalu mengambil versi terbaru dari repositori. Untuk hasil yang stabil, kunci ke satu versi dengan menambahkan nama tag Git setelah nama repositori, misalnya
> `https://cdn.jsdelivr.net/gh/otakbiru/GeoSVG@0.1.0/GeoSVG.min.js`
> (sesuaikan dengan nama tag rilis yang ada di repositori).

### Unduh manual

Unduh `GeoSVG.js` (atau `GeoSVG.min.js`) dari repositori, lalu muat dengan `<script src="GeoSVG.js"></script>`.

### Bundler atau Node

```js
const GeoSVG = require('./GeoSVG.js');
```

Library ini butuh **DOM** (browser). Di framework dengan render sisi server (Next.js, Nuxt, dan sejenisnya), buat instance di dalam `useEffect` atau `onMounted`, bukan saat render di server. Pakai `GeoSVG.didukung()` untuk memeriksa apakah lingkungannya cocok.

---

## Mulai Cepat

```html
<div id="kanvas"></div>

<script src="https://cdn.jsdelivr.net/gh/otakbiru/GeoSVG/GeoSVG.js"></script>
<script>
  // Kanvas 400 x 300, sumbu Y ke atas, dengan margin 20
  const geo = new GeoSVG('kanvas', 400, 300, { margin: 20, yUp: true });

  const A = [60, 40], B = [340, 40], C = [180, 240];

  // Segitiga
  geo.buatSegitiga(A, B, C, {
    stroke: '#1a56db',
    strokeWidth: 2,
    fill: 'rgba(26, 86, 219, 0.08)'
  });

  // Titik dengan label
  geo.buatTitik(A, { label: 'A', labelPosition: 'bottom-left' });
  geo.buatTitik(B, { label: 'B', labelPosition: 'bottom-right' });
  geo.buatTitik(C, { label: 'C', labelPosition: 'top-center' });

  // Sudut di titik A (titik sudut adalah argumen tengah)
  geo.buatSudut(B, A, C, { label: 'α', radius: 28 });

  // Lingkaran luar segitiga
  geo.buatLingkaranLuar(A, B, C, { stroke: '#888', dashed: true });
</script>
```

Container boleh berupa **id** (teks) atau **elemen HTML** langsung.

---

## Membuat Kanvas

```js
new GeoSVG(container, width, height, options)
new GeoSVG(container, options)          // width dan height boleh ditulis di dalam options
```

| Argumen | Keterangan |
|---|---|
| `container` | id elemen (teks) atau elemen HTML |
| `width`, `height` | ukuran gambar. Boleh angka, atau teks seperti `'100%'`. Kalau dikosongkan, dipakai `100%` |
| `options` | lihat tabel di bawah |

### Opsi konstruktor

| Opsi | Alias | Keterangan |
|---|---|---|
| `margin` | `padding` | ruang kosong di sekeliling area gambar (butuh `width` dan `height` berupa angka) |
| `viewBox` | | teks, misalnya `'0 0 800 600'`, atau `true` untuk dihitung otomatis dari ukuran dan margin |
| `preserveAspectRatio` | | diteruskan ke atribut SVG |
| `background` | `backgroundColor` | warna latar. Bawaan: transparan |
| `overflow` | | bawaan `'visible'`, supaya garis di tepi tidak terpotong |
| `defaultStroke` | | warna garis bawaan (bawaan `'black'`) |
| `defaultStrokeWidth` | | tebal garis bawaan |
| `fontFamily`, `fontSize` | | font bawaan untuk semua teks |
| `yUp` | | `true` = sumbu Y mengarah ke atas ([lihat di bawah](#sistem-koordinat-yup)) |
| `strict` | | `true` = input salah langsung melempar error ([lihat di bawah](#penanganan-error)) |
| `clearContainer` | | `true` = gambar lama di container itu diganti, bukan ditumpuk |
| `title` | `judul` | judul gambar untuk pembaca layar |
| `description` | `desc`, `deskripsi` | deskripsi gambar untuk pembaca layar |
| `dekoratif` | | `true` = gambar dianggap hiasan, disembunyikan dari pembaca layar |

Kalau container sudah berisi gambar GeoSVG lain dan `clearContainer` tidak diisi, gambar baru akan **bertumpuk** dan muncul peringatan di Console.

---

## Sistem Koordinat (`yUp`)

Secara bawaan, koordinat mengikuti SVG: titik `(0, 0)` ada di **kiri atas** dan sumbu Y mengarah **ke bawah**.

Dengan `yUp: true`, sumbu Y dibalik sehingga mengarah **ke atas**, seperti di buku matematika. Angka yang Anda tulis sama dengan angka di viewBox, dan teks serta label tetap tegak (tidak ikut terbalik). Opsi ini butuh `width` dan `height` berupa angka (atau `viewBox` berupa teks).

```js
const geo = new GeoSVG('kanvas', 400, 300, { yUp: true });
geo.buatTitik(50, 50, { label: 'P' });   // 50 dari kiri, 50 dari bawah
```

---

## Cara Menulis Titik

Hampir semua method menerima titik dalam dua bentuk:

```js
geo.buatSegmen(10, 20, 100, 80);              // bentuk angka
geo.buatSegmen([10, 20], [100, 80]);          // bentuk array [x, y]
geo.buatSegmen({ x: 10, y: 20 }, { x: 100, y: 80 });   // bentuk objek {x, y}
```

Begitu juga untuk lingkaran, busur, elips, dan bentuk lain yang punya pusat:

```js
geo.buatLingkaran(100, 100, 50, { stroke: 'red' });
geo.buatLingkaran([100, 100], 50, { stroke: 'red' });
```

---

## Opsi Gaya Umum

Opsi berikut berlaku untuk hampir semua bentuk:

| Opsi | Keterangan |
|---|---|
| `stroke` | warna garis |
| `fill` | warna isi |
| `strokeWidth` | tebal garis |
| `fillOpacity`, `strokeOpacity` | transparansi isi dan garis |
| `dashed` | garis putus-putus: `true` (pola `6 4`), angka (misalnya `5`), atau teks pola (misalnya `'8 3 2 3'`) |
| `strokeDasharray` | pola putus-putus tertulis; menang atas `dashed` |
| `opacity`, `strokeLinecap`, `strokeLinejoin`, `strokeMiterlimit`, `strokeDashoffset`, `fillRule`, `clipRule`, `vectorEffect`, `pointerEvents`, `visibility`, `cursor`, `transform` | diteruskan ke atribut SVG (penulisan camelCase) |
| `id` | id elemen (harus unik di halaman) |
| `className` atau `class` | nama kelas CSS |
| `dataset` | objek untuk atribut `data-*`, misalnya `{ namaBentuk: 'a' }` menjadi `data-nama-bentuk="a"` |

Atribut hanya ditulis kalau Anda mengisinya. Sisanya diwarisi dari gaya bawaan kanvas.

---

## Daftar Method

Method berawalan **`buat`** menggambar sesuatu dan mengembalikan elemen SVG-nya. Method berawalan **`hitung`** hanya menghitung dan mengembalikan angka atau koordinat, tanpa menggambar.

### Titik

| Method | Keterangan |
|---|---|
| `buatTitik(x, y, opsi)` | titik. `marker`: `'full-circle'` (bawaan), `'empty-circle'`, `'diamond'`, `'plus'`. Opsi lain: `size`, `color`, `label`, `labelPosition`, `labelDistance`, `labelColor`, `dx`, `dy` |
| `hitungTitikTengah(p1, p2)` | mengembalikan `[x, y]` |
| `buatTitikTengah(p1, p2, opsi)` | menggambar titik di tengah dua titik |
| `buatTitikPotong(objA, objB, opsi)` | titik di perpotongan garis atau lingkaran |
| `hitungTitikPotong(objA, objB, opsi)` | sama, tanpa menggambar. `{ segmen: true }` membatasi pada ruas garis |
| `buatTitikPotongGaris(a1, a2, b1, b2, opsi)` | titik potong dua garis dari empat titik |

Objek untuk titik potong: garis ditulis `[p1, p2]`, lingkaran ditulis `{ pusat: [x, y], r }`.

### Garis

| Method | Keterangan |
|---|---|
| `buatGaris(p1, p2, opsi)` | garis tak hingga, dipotong di tepi gambar (`clipPad` mengatur jarak tambahan) |
| `buatSinar(pangkal, arah, opsi)` | sinar dari titik pangkal melewati titik arah |
| `buatSegmen(p1, p2, opsi)` | ruas garis |
| `buatVektor(p1, p2, opsi)` | ruas garis berpanah |
| `buatPolyline(titik, opsi)` | garis patah melewati banyak titik |
| `buatGarisSejajar(p1, p2, lewat, opsi)` | garis sejajar `p1-p2`; `lewat` berupa titik yang dilalui atau angka jarak geser |
| `buatSegmenSejajar(p1, p2, lewat, opsi)` | versi segmen dari garis sejajar |
| `buatGarisTegakLurus(p1, p2, lewat, opsi)` | garis tegak lurus; `lewat` bawaan `'tengah'` (sumbu ruas) |
| `buatGarisBagi(p1, titikSudut, p3, opsi)` | garis bagi sudut |
| `hitungKakiTegakLurus(p, a, b)` | kaki garis tegak lurus dari titik `p` ke garis `a-b` |
| `hitungGarisBagi(p1, titikSudut, p3)` | menghitung garis bagi |
| `buatTandaSamaPanjang(p1, p2, opsi)` | tanda hash pada ruas garis. Opsi: `count`, `size`, `spacing`, `stroke` |
| `buatDimensi(p1, p2, offset, label, opsi)` | garis dimensi dengan label ukuran |

### Poligon dan Segitiga

| Method | Keterangan |
|---|---|
| `buatPoligon(titik, opsi)` | poligon dari daftar titik. `sideLabels: ['a', 'b', 'c']` memberi label tiap sisi |
| `buatSegitiga(p1, p2, p3, opsi)` | segitiga |
| `buatPersegiPanjang(x, y, lebar, tinggi, opsi)` | persegi panjang; `radius` (angka atau `'penuh'`) untuk sudut membulat |
| `buatPersegi(x, y, sisi, opsi)` | persegi |
| `buatPoligonBeraturan(pusat, radius, jumlahSisi, opsi)` | poligon beraturan; `startAngle` dalam derajat |
| `buatLabelSisi(p1, p2, teks, opsi)` | label di tengah sisi; teks kosong atau `'auto'` menampilkan panjang sisi |
| `buatLabelSisiPoligon(titik, daftarTeks, opsi)` | label untuk semua sisi poligon |

### Lingkaran dan Lengkungan

| Method | Keterangan |
|---|---|
| `buatLingkaran(cx, cy, r, opsi)` | lingkaran |
| `buatElips(cx, cy, rx, ry, opsi)` | elips |
| `buatBusur(pusat, r, sudutAwal, sudutAkhir, opsi)` | busur; sudut dalam **derajat**; `sweep`: `1` searah jarum jam (bawaan), `0` berlawanan |
| `buatJuring(pusat, r, sudutAwal, sudutAkhir, opsi)` | juring |
| `buatTembereng(pusat, r, sudutAwal, sudutAkhir, opsi)` | tembereng |
| `buatSetengahLingkaran(p1, p2, opsi)` | setengah lingkaran dengan diameter `p1-p2`. Opsi: `tutup`, `sisi` (`'kiri'` atau `'kanan'`) |
| `hitungLingkaranLuar(p1, p2, p3)` | mengembalikan `{ pusat, r }` |
| `hitungLingkaranDalam(p1, p2, p3)` | mengembalikan `{ pusat, r }` |
| `buatLingkaranLuar(p1, p2, p3, opsi)` | menggambar lingkaran luar segitiga |
| `buatLingkaranDalam(p1, p2, p3, opsi)` | menggambar lingkaran dalam segitiga |

### Garis Singgung

| Method | Keterangan |
|---|---|
| `hitungTitikSinggung(lingkaran, titik)` | titik singgung dari sebuah titik ke lingkaran |
| `buatGarisSinggung(lingkaran, titik, opsi)` | garis singgung dari sebuah titik |
| `hitungSinggungPersekutuan(lingkaran1, lingkaran2, jenis)` | garis singgung persekutuan; `jenis`: `'luar'` (bawaan) atau `'dalam'` |
| `buatGarisSinggungPersekutuan(lingkaran1, lingkaran2, opsi)` | menggambar garis singgung persekutuan |

### Kurva dan Grafik

| Method | Keterangan |
|---|---|
| `buatFungsi(f, xMin, xMax, opsi)` | grafik `y = f(x)`. Opsi: `langkah` (bawaan 200), `skala`, `skalaX`, `skalaY`, `maksLompatan` (garis diputus di titik tak kontinu) |
| `buatKurvaBezier(titik, opsi)` | kurva Bézier |
| `buatKurvaHalus(titik, opsi)` | kurva halus melewati titik-titik |

```js
geo.buatFungsi(x => 100 + 80 * Math.sin(x / 30), 0, 400, { stroke: 'crimson', strokeWidth: 2 });
```

### Sudut

| Method | Keterangan |
|---|---|
| `hitungSudut(p1, titikSudut, p3, opsi)` | besar sudut dalam derajat. Opsi: `refleks`, `desimal` |
| `buatSudut(p1, titikSudut, p3, opsi)` | lengkungan sudut. Opsi: `radius`, `label`, `labelRadius`, `labelColor`, `refleks` |
| `buatSiku(p1, titikSudut, p3, opsi)` | tanda sudut siku-siku. Opsi: `size`, `dot`, `labelColor` |

Titik sudut (vertex) selalu menjadi argumen **tengah**.

### Transformasi

| Method | Keterangan |
|---|---|
| `hitungRefleksi(objek, terhadap)` | hasil pencerminan objek |
| `buatRefleksi(objek, terhadap, opsi)` | menggambar hasil pencerminan |

### Sumbu, Grid, dan Pola

| Method | Keterangan |
|---|---|
| `buatSumbuKoordinat(xMin, xMax, yMin, yMax, step, opsi)` | sumbu X dan Y berpanah dengan tanda angka. Opsi: `tickSize`, `skipZero` |
| `buatGrid(stepX, stepY, opsi)` | jaring-jaring; seluruh garis digabung menjadi satu elemen `<path>` |
| `buatPolaArsir(id, tipe, opsi)` | pola arsir. `tipe`: `'miring'`, `'vertikal'`, `'horizontal'`, `'silang'`. Pakai sebagai `fill: 'url(#id)'` |

### Teks

| Method | Keterangan |
|---|---|
| `buatTeks(x, y, teks, opsi)` | teks. Opsi: `position`, `fill` (alias `color`), `fontSize`, `fontFamily`, `fontWeight`, `fontStyle` |
| `buatLatex(x, y, latex, opsi)` | rumus LaTeX ([lihat di bawah](#latex)) |

**Nilai posisi** (`position` / `labelPosition`):

```
top-left      top-center      top-right
middle-left   middle-center   middle-right
bottom-left   bottom-center   bottom-right
```

Penulisan bahasa Indonesia juga diterima: `'kiri atas'`, `'tengah atas'`, `'kanan atas'`, `'kiri tengah'`, `'tengah'`, `'kanan tengah'`, `'kiri bawah'`, `'tengah bawah'`, `'kanan bawah'`.

### Pengelolaan Elemen

| Method | Keterangan |
|---|---|
| `buatGrup(daftarElemen, opsi)` | membungkus elemen dalam satu grup |
| `hapus(elemen)` | menghapus elemen |
| `pindahKeAtas(elemen)`, `pindahKeBawah(elemen)` | mengubah urutan tumpukan |
| `pindahKeLapisan(elemen, nama)` | memindahkan ke lapisan lain |
| `setJudul(teks)`, `setDeskripsi(teks)` | mengubah judul dan deskripsi aksesibilitas |
| `setGambarLatar(sumber, opsi)` | memasang gambar latar (lihat di bawah) |
| `hapusGambarLatar()` | menghapus gambar latar |
| `clear()` | mengosongkan gambar (judul, deskripsi, dan gambar latar dipertahankan) |
| `destroy()` | melepas gambar dari halaman dan membuang isinya |

### Gambar Latar

```js
await geo.setGambarLatar('foto.png', { fit: 'contain' });   // 'contain' | 'cover' | 'stretch'
```

`sumber` boleh berupa `File` atau `Blob`, alamat gambar (URL), atau data URL. Format: JPG, PNG, WebP, GIF. Gambar disimpan sebagai data URL sehingga **ikut tersimpan di file SVG** saat diunduh. Untuk URL dari situs lain, server gambar harus mengizinkan akses (CORS).

### Static

| Anggota | Keterangan |
|---|---|
| `GeoSVG.versi` | nomor versi, misalnya `'0.1.0'` |
| `GeoSVG.didukung()` | `true` jika lingkungan ini punya DOM |

---

## Label

Hampir semua garis (`buatSegmen`, `buatGaris`, `buatSinar`, `buatVektor`, dan turunannya) menerima opsi label:

| Opsi | Keterangan |
|---|---|
| `label` | teks label |
| `labelPosisi` | posisi di sepanjang garis: angka `0` sampai `1`, atau `'awal'`, `'tengah'` (bawaan), `'akhir'` |
| `labelSisi` | `'kiri'` (bawaan) atau `'kanan'`, dilihat dari arah garis |
| `labelJarak` | jarak label dari garis (bawaan 6) |
| `labelColor` | warna label |

```js
geo.buatSegmen([50, 50], [250, 50], { label: '5 cm', labelSisi: 'kanan' });
```

Jika garis dibuat dengan label, hasil method adalah sebuah **grup** (`<g>`) berisi garis dan teksnya.

---

## LaTeX

`buatLatex` membutuhkan **KaTeX** atau **MathJax** yang sudah dimuat di halaman. GeoSVG tidak menyertakannya sendiri.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex/dist/katex.min.css">
<script src="https://cdn.jsdelivr.net/npm/katex/dist/katex.min.js"></script>
```

```js
const rumus = geo.buatLatex(200, 150, '\\frac{a}{b} + \\sqrt{x}', {
  position: 'middle-center',
  fontSize: '18px',
  displayMode: false
});

await rumus.ready;   // Promise: selesai setelah rumus dirender dan diposisikan
```

Opsi: `position`, `color`, `fontSize`, `opacity`, `displayMode`, `width`, `height`.

Kalau KaTeX atau MathJax tidak ditemukan, muncul peringatan di Console dan rumus tampil sebagai teks mentah.

---

## Ekspor SVG dan PNG

```js
geo.keStringSVG();                 // SVG sebagai teks (lengkap dengan deklarasi XML)
geo.downloadSVG('segitiga.svg');   // unduh sebagai file SVG

await geo.toPNGBlob({ scale: 2, background: 'white' });   // Promise<Blob>, tanpa mengunduh
await geo.downloadPNG('segitiga.png', 3);                 // unduh PNG dengan pembesaran 3x
await geo.downloadPNG('segitiga.png', { scale: 3, background: 'white' });
```

| Opsi PNG | Keterangan |
|---|---|
| `scale` | pembesaran (bawaan 2). Ukuran 300×150 dengan `scale: 2` menjadi 600×300 piksel |
| `background` | warna latar. Bawaan: mengikuti latar SVG. `'transparent'` = tanpa latar |
| `tanpaLatex` | `true` = rumus LaTeX tidak disertakan (berguna kalau PNG gagal dibuat) |

---

## Lapisan, Grup, dan Batch

Gambar disusun dalam tiga lapisan, dari bawah ke atas: **`grid`**, **`shapes`**, **`labels`**. Bentuk masuk ke lapisan sesuai jenisnya: grid di bawah, bentuk di tengah, teks dan label di atas. Dengan begitu, teks tidak tertutup bentuk.

### Batch

Kalau menggambar banyak elemen sekaligus, bungkus dalam `batch` supaya elemen dimasukkan ke halaman **satu kali saja**, bukan satu per satu:

```js
geo.batch(g => {
  for (let i = 0; i < 500; i++) {
    g.buatLingkaran(Math.random() * 400, Math.random() * 300, 3);
  }
});
```

`batch` otomatis selesai walaupun ada error di tengah jalan. Tersedia juga `mulaiBatch()` dan `selesaiBatch()` kalau ingin mengatur sendiri (batch di dalam batch aman).

---

## Penanganan Error

Secara bawaan, input yang salah **tidak menghentikan program**: GeoSVG menampilkan peringatan di Console dan mengembalikan `null`.

```js
const lingkaran = geo.buatLingkaran(100, 100, -5);   // r negatif
// Console: "GeoSVG: buatLingkaran: pusat dan r harus angka, dan r tidak boleh negatif"
// lingkaran === null
```

Untuk pengembangan, aktifkan mode **strict** supaya kesalahan langsung melempar `Error`:

```js
const geo = new GeoSVG('kanvas', 400, 300, { strict: true });
```

---

## Aksesibilitas

Beri gambar judul dan deskripsi supaya pembaca layar bisa menjelaskannya:

```js
const geo = new GeoSVG('kanvas', 400, 300, {
  title: 'Segitiga ABC',
  description: 'Segitiga dengan titik A di kiri bawah, B di kanan bawah, dan C di atas.'
});
```

Kalau gambar hanya hiasan, sembunyikan dari pembaca layar:

```js
const geo = new GeoSVG('kanvas', 400, 300, { dekoratif: true });
```

Mengisi `description` tanpa `title` akan memunculkan peringatan.

---

## Alias Opsi (Indonesia dan Inggris)

Beberapa opsi bisa ditulis dengan lebih dari satu nama. Kalau nama bakunya tidak diisi, dipakai alias pertama yang diisi.

| Konteks | Nama baku | Alias |
|---|---|---|
| Konstruktor | `margin` | `padding` |
| Konstruktor | `background` | `backgroundColor` |
| Konstruktor | `title` | `judul` |
| Konstruktor | `description` | `desc`, `deskripsi` |
| `buatTitik` | `labelDistance` | `labelPadding`, `offset` |
| `buatTeks` | `fill` | `color` |
| `buatTandaSamaPanjang` | `count` | `ticks`, `jumlah` |
| `buatTandaSamaPanjang` | `size` | `length`, `panjang` |
| `buatTandaSamaPanjang` | `spacing` | `gap`, `jarak` |
| `buatTandaSamaPanjang` | `stroke` | `color` |
| `buatSudut` | `labelColor` | `color` |
| `buatSudut` | `refleks` | `reflex` |
| `buatSiku` | `size` | `radius` |
| `buatSiku` | `dot` | `showDot` |
| `buatSiku` | `labelColor` | `color` |
| `buatPolaArsir` | `stroke` | `color` |

---

## Kompatibilitas

- Butuh browser modern dengan dukungan SVG dan `document.createElementNS`.
- Ekspor PNG memakai canvas dan `Blob`.
- Pemuatan: `<script>` biasa (variabel global `GeoSVG`), AMD/RequireJS, atau `require` di bundler.
