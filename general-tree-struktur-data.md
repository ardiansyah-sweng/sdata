# 🌳 General Tree — Pohon yang Tidak Memaksa Semua Cabang Harus Sama

> **Pendekatan:** Materi ini ditulis menggunakan **Teknik Feynman** — menjelaskan konsep kompleks dengan bahasa sederhana, analogi nyata, dan membangun pemahaman dari masalah sehari-hari sebelum masuk ke definisi formal.

---

## 📋 Daftar Isi

- [🌳 General Tree — Pohon yang Tidak Memaksa Semua Cabang Harus Sama](#-general-tree--pohon-yang-tidak-memaksa-semua-cabang-harus-sama)
  - [📋 Daftar Isi](#-daftar-isi)
  - [Bayangkan Struktur Organisasi Perusahaan](#bayangkan-struktur-organisasi-perusahaan)
  - [Apa Bedanya dengan Tree Biasa?](#apa-bedanya-dengan-tree-biasa)
  - [Kenapa Kebebasan Ini Penting?](#kenapa-kebebasan-ini-penting)
  - [Cara Menyimpan General Tree di dalam Kode](#cara-menyimpan-general-tree-di-dalam-kode)
    - [📋 Cara 1 — Simpan Child dalam Sebuah List](#-cara-1--simpan-child-dalam-sebuah-list)
    - [🔄 Cara 2 — Left-Child Right-Sibling (Teknik Konversi)](#-cara-2--left-child-right-sibling-teknik-konversi)
  - [Operasi Dasar pada General Tree](#operasi-dasar-pada-general-tree)
    - [🚶 Traversal — Cara Mengunjungi Semua Node](#-traversal--cara-mengunjungi-semua-node)
      - [1. Pre-order Traversal](#1-pre-order-traversal)
      - [2. Post-order Traversal](#2-post-order-traversal)
      - [3. Level-order Traversal](#3-level-order-traversal)
    - [Perbandingan Ketiga Traversal](#perbandingan-ketiga-traversal)
  - [Di Mana General Tree Digunakan?](#di-mana-general-tree-digunakan)
    - [💻 Sistem File Komputer](#-sistem-file-komputer)
    - [🌐 DOM pada Halaman Web](#-dom-pada-halaman-web)
    - [💬 Komentar Bersarang di Media Sosial](#-komentar-bersarang-di-media-sosial)
    - [📦 Format Data XML dan JSON](#-format-data-xml-dan-json)
  - [Perbedaan General Tree, Binary Tree, dan BST](#perbedaan-general-tree-binary-tree-dan-bst)
  - [Ringkasan](#ringkasan)
  - [➡️ Selanjutnya: Binary Search Tree (BST)](#️-selanjutnya-binary-search-tree-bst)
  - [⬅️ Sebelumnya: Tree](#️-sebelumnya-tree)

---

## Bayangkan Struktur Organisasi Perusahaan

Bayangkan kamu sedang melihat struktur organisasi sebuah perusahaan.

Di paling atas ada seorang **Direktur Utama**. Di bawahnya ada tiga manajer — Manajer Keuangan, Manajer Produksi, dan Manajer Pemasaran. Manajer Keuangan punya dua staf. Manajer Produksi punya empat staf. Manajer Pemasaran tidak punya staf sama sekali — ia bekerja sendiri.

Coba kamu gambar:

```
                Direktur Utama
               /       |        \
          Keuangan  Produksi  Pemasaran
           /  \     / | \ \
         S1   S2  S3 S4 S5 S6
```

Perhatikan sesuatu yang menarik di sini. Setiap manajer punya **jumlah staf yang berbeda-beda**. Keuangan punya 2, Produksi punya 4, Pemasaran punya 0.

Tidak ada yang memaksa mereka harus sama. Setiap node bebas punya child sebanyak yang dibutuhkan.

> Itulah inti dari **General Tree**.

---

## Apa Bedanya dengan Tree Biasa?

Di [bab sebelumnya](tree-struktur-data.md), kamu sudah berkenalan dengan konsep Tree secara umum — root, node, edge, parent, child, dan leaf. Semua itu tetap berlaku di General Tree.

Yang membedakan General Tree dari jenis tree lainnya hanya **satu hal**:

> **Tidak ada batasan berapa banyak child yang boleh dimiliki oleh sebuah node.**

Itu saja. Sesederhana itu.

Bandingkan dengan **Binary Tree** yang akan kita pelajari setelah ini:

| | General Tree | Binary Tree |
|---|---|---|
| Maks. child per node | **Tidak terbatas** | Maksimal 2 |
| Kebebasan struktur | Sangat bebas | Terbatas kiri & kanan |

Di General Tree, dua child boleh. Sepuluh child boleh. Nol pun boleh.

---

## Kenapa Kebebasan Ini Penting?

Karena **dunia nyata tidak selalu simetris**.

Bayangkan kamu diminta memodelkan **daftar isi sebuah buku**:

```
Buku Struktur Data
├── Bab 1: Array
│   ├── 1.1 Pengertian Array
│   ├── 1.2 Operasi Dasar
│   └── 1.3 Array Multidimensi
├── Bab 2: Linked List
│   ├── 2.1 Singly Linked List
│   └── 2.2 Doubly Linked List
└── Bab 3: Stack
    └── 3.1 Implementasi Stack
```

Bab 1 punya 3 subbab. Bab 2 punya 2 subbab. Bab 3 hanya punya 1 subbab.

Jika kamu dipaksa memakai Binary Tree, kamu hanya boleh punya maksimal **2 subbab per bab**. Kamu tidak bisa merepresentasikan Bab 1 dengan benar.

Dengan General Tree, kamu bebas. Setiap bab boleh punya subbab sebanyak yang dibutuhkan — persis seperti buku sungguhan.

---

## Cara Menyimpan General Tree di dalam Kode

Karena setiap node bisa punya jumlah child yang berbeda-beda, kita tidak bisa menyiapkan "slot" child yang tetap. Lalu bagaimana cara menyimpannya?

Ada **dua cara** yang paling umum dipakai.

---

### 📋 Cara 1 — Simpan Child dalam Sebuah List

Bayangkan setiap node membawa sebuah **daftar belanja**. Daftar itu berisi semua child-nya. Kalau punya 3 child, daftarnya berisi 3 nama. Kalau tidak punya child, daftarnya kosong.

```
Node {
    data     : "Direktur Utama"
    children : [Keuangan, Produksi, Pemasaran]
}

Node {
    data     : "Keuangan"
    children : [S1, S2]
}

Node {
    data     : "Pemasaran"
    children : []     ← daftar kosong, ini adalah leaf node
}
```

✅ **Kelebihan:** Mudah dipahami dan paling natural.  
⚠️ **Kekurangan:** Ukuran list setiap node bisa berbeda-beda, sehingga memori yang digunakan tidak seragam.

---

### 🔄 Cara 2 — Left-Child Right-Sibling (Teknik Konversi)

Ini cara yang lebih cerdas — dan sedikit lebih butuh imajinasi.

Idenya: *"Bagaimana jika kita ubah General Tree menjadi Binary Tree, tanpa kehilangan informasi apapun?"*

Caranya menggunakan dua aturan sederhana:

- Pointer **kiri** → selalu menunjuk ke **child pertama**
- Pointer **kanan** → selalu menunjuk ke **saudara kandung berikutnya (sibling)**

Mari kita lihat contohnya:

```
General Tree (asli):

                Direktur
               /    |    \
          Keuangan Produksi Pemasaran
           /  \
          S1  S2


Setelah konversi ke Left-Child Right-Sibling:

        Direktur
        /
   Keuangan ──→ Produksi ──→ Pemasaran
   /
  S1 ──→ S2
```

Setiap node sekarang hanya punya **dua pointer** — persis seperti Binary Tree. Tapi seluruh informasi hierarki aslinya tetap tersimpan dengan utuh.

> 💡 Teknik ini sangat berguna ketika kamu ingin menyimpan General Tree menggunakan struktur Binary Tree yang sudah ada, tanpa harus membuat implementasi baru dari nol.

---

## Operasi Dasar pada General Tree

### 🚶 Traversal — Cara Mengunjungi Semua Node

Traversal artinya kamu berjalan menelusuri seluruh tree, mengunjungi setiap node satu per satu. Ada tiga cara utama melakukan ini.

---

#### 1. Pre-order Traversal

Kunjungi node saat ini **sebelum** mengunjungi child-nya.

> Seperti membaca daftar isi buku dari atas ke bawah — bab dulu, baru subbab.

```
Urutan kunjungan:
Direktur → Keuangan → S1 → S2 → Produksi → S3 → S4 → S5 → S6 → Pemasaran
```

```
Langkah:
1. Kunjungi Direktur          ✓
2. Masuk ke child pertama: Keuangan  ✓
3. Kunjungi S1, S2            ✓
4. Kembali, masuk ke Produksi ✓
5. Kunjungi S3, S4, S5, S6   ✓
6. Kembali, masuk ke Pemasaran ✓
```

---

#### 2. Post-order Traversal

Kunjungi semua child **terlebih dahulu**, baru kunjungi node saat ini.

> Seperti menghitung total gaji — hitung dulu semua staf, baru laporkan ke manajernya.

```
Urutan kunjungan:
S1 → S2 → Keuangan → S3 → S4 → S5 → S6 → Produksi → Pemasaran → Direktur
```

---

#### 3. Level-order Traversal

Kunjungi node **level demi level** dari atas ke bawah, dari kiri ke kanan.

> Seperti membaca bagan organisasi baris per baris.

```
Level 0: Direktur
Level 1: Keuangan → Produksi → Pemasaran
Level 2: S1 → S2 → S3 → S4 → S5 → S6
```

---

### Perbandingan Ketiga Traversal

| Traversal | Urutan Kunjungan | Analogi |
|---|---|---|
| **Pre-order** | Node → Children | Membaca daftar isi |
| **Post-order** | Children → Node | Menghitung total dari bawah |
| **Level-order** | Level per level | Membaca bagan dari atas |

---

## Di Mana General Tree Digunakan?

Kamu sudah sering bersentuhan dengan General Tree tanpa menyadarinya.

---

### 💻 Sistem File Komputer

Setiap folder bisa berisi berapa pun subfolder dan file di dalamnya — tidak ada batasan. Folder `Documents` bisa berisi 2 subfolder, sementara folder `Downloads` bisa berisi 50 file sekaligus.

```
📁 Documents/
├── 📁 Kuliah/
│   ├── 📁 Semester1/
│   │   └── 📄 struktur-data.pdf
│   └── 📁 Semester2/
│       └── 📄 algoritma.pdf
└── 📁 Pribadi/
    ├── 📄 foto.jpg
    └── 📄 catatan.txt
```

---

### 🌐 DOM pada Halaman Web

Tag `<div>` bisa berisi satu elemen, bisa juga berisi dua puluh elemen sekaligus. Tidak ada pembatasan seperti di Binary Tree.

```html
<html>                        ← Root
  <head>                      ← Child dari html
    <title>Halaman Web</title>
    <meta charset="utf-8">
    <link rel="stylesheet">   ← 3 child di sini
  </head>
  <body>                      ← Child dari html
    <h1>Judul</h1>
    <p>Paragraf 1</p>
    <p>Paragraf 2</p>
    <footer>...</footer>      ← 4 child di sini
  </body>
</html>
```

---

### 💬 Komentar Bersarang di Media Sosial

Ketika kamu membalas sebuah komentar, lalu orang lain membalas balasanmu, terbentuk hierarki yang persis seperti General Tree. Satu komentar bisa dibalas oleh banyak orang, dan setiap balasan bisa dibalas lagi.

```
💬 Postingan
├── 💬 Komentar A
│   ├── 💬 Balas dari User1
│   ├── 💬 Balas dari User2
│   └── 💬 Balas dari User3
└── 💬 Komentar B
    └── 💬 Balas dari User4
        └── 💬 Balas dari User5
```

---

### 📦 Format Data XML dan JSON

Format data yang digunakan untuk menyimpan dan mengirim data di internet juga memiliki struktur General Tree. Setiap elemen bisa memiliki anak elemen sebanyak yang diperlukan.

```xml
<perusahaan>
  <departemen nama="Keuangan">
    <staf>S1</staf>
    <staf>S2</staf>
  </departemen>
  <departemen nama="Produksi">
    <staf>S3</staf>
    <staf>S4</staf>
    <staf>S5</staf>
    <staf>S6</staf>
  </departemen>
  <departemen nama="Pemasaran"/>
</perusahaan>
```

---

## Perbedaan General Tree, Binary Tree, dan BST

Sebelum menutup bab ini, penting untuk meluruskan perbedaan tiga istilah yang sering membingungkan:

| | General Tree | Binary Tree | Binary Search Tree |
|---|---|---|---|
| **Maks. child** | Tidak terbatas | 2 | 2 |
| **Urutan child** | Bebas | Ada kiri & kanan | Kiri < Root < Kanan |
| **Pencarian** | Tidak terstruktur | Tidak terstruktur | Terstruktur & cepat |
| **Contoh pakai** | File system, DOM, XML | Heap, Expression Tree | Pencarian data terurut |

```
General Tree        Binary Tree         Binary Search Tree
─────────────       ─────────────       ──────────────────
      A                   A                     8
    / | \                / \                   / \
   B  C  D              B   C                 3   10
  /|      \            / \   \               / \    \
 E  F      G          D   E   F             1   6    14
```

> General Tree adalah yang paling bebas. Binary Tree mulai menambahkan aturan. Binary Search Tree menambahkan aturan lagi di atas Binary Tree. Setiap tambahan aturan membuka kemampuan baru yang lebih spesifik.

---

## Ringkasan

Sampai di sini, kamu sudah memahami tiga hal penting dari bab ini.

**Pertama**, mengapa General Tree ada — karena dunia nyata penuh dengan hierarki yang tidak simetris, dan kita butuh struktur yang cukup fleksibel untuk merepresentasikannya.

**Kedua**, dua cara menyimpan General Tree:

| Cara | Ide Utama | Cocok untuk |
|---|---|---|
| **List of Children** | Setiap node simpan daftar child-nya | Implementasi sederhana |
| **Left-Child Right-Sibling** | Konversi ke Binary Tree | Hemat memori, reuse struktur |

**Ketiga**, di mana General Tree hidup:

| Aplikasi | Contoh |
|---|---|
| Sistem File | Folder & subfolder di komputer |
| Halaman Web | DOM (Document Object Model) |
| Media Sosial | Komentar bersarang |
| Format Data | XML, JSON |

---

## ➡️ Selanjutnya: Binary Search Tree (BST)

Di bab berikutnya, kita akan masuk ke **Binary Search Tree** — tree dengan dua aturan sederhana yang ternyata membuatnya mampu mencari data dengan kecepatan yang luar biasa.

Sebelum itu, pastikan kamu sudah memahami perbedaan antara General Tree dan Binary Tree — karena perbedaan itulah yang menjadi titik awal dari bab berikutnya.

---

## ⬅️ Sebelumnya: Tree

Masih *stuck* atau bingung dengan **General Tree**? Jangan sedih, kembali aja lagi **Tree** — kamu akan *refresh* kembali dengan pemahaman tentang Tree dari konsep paling mendasar.

---

> 📝 **Catatan Penulis:** Materi ini ditulis dengan pendekatan [Teknik Feynman](https://fs.blog/feynman-technique/) — jika kamu bisa menjelaskan sebuah konsep dengan bahasa sederhana kepada orang lain, itu tandanya kamu benar-benar memahaminya.
