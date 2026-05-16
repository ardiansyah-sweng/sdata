# 🌳 Bab 4: Tree — Ketika Data Tidak Cukup Hanya Berbaris Lurus

> **Prasyarat:** Agar dapat menguasai materi struktur data Tree, paling tidak kamu sudah menguasai **rekursif**, **pointer** dan **struct**.

---

## 📋 Daftar Isi

- [Bayangkan Silsilah Keluarga](#bayangkan-silsilah-keluarga)
- [Kenapa Tidak Pakai Array atau Linked List Saja?](#kenapa-tidak-pakai-array-atau-linked-list-saja)
- [Bagian-Bagian Tree](#bagian-bagian-tree)
- [Tinggi dan Kedalaman](#tinggi-dan-kedalaman)
- [Kenapa Kita Perlu Mempelajari Tree?](#kenapa-kita-perlu-mempelajari-tree)
- [Jenis-Jenis Tree](#jenis-jenis-tree)
- [Ringkasan](#ringkasan)

---

## Bayangkan Silsilah Keluarga

Bayangkan kamu sedang membuat silsilah keluarga.

Kamu mulai dari kakekmu di bagian paling atas. Di bawahnya, ada dua anaknya — ayahmu dan pamanmu. Di bawah ayahmu, ada kamu dan adikmu. Di bawah pamanmu, ada dua sepupumu.

Coba kamu gambar di kertas. Hasilnya akan terlihat seperti ini:

```
            Kakek
           /     \
        Ayah     Paman
        /  \      /  \
      Kamu Adik  Sep1 Sep2
```

Tanpa sadar, kamu baru saja menggambar sebuah **Tree (Pohon)** dalam struktur data.

---

## Kenapa Tidak Pakai Array atau Linked List Saja?

Pertanyaan bagus. Selama ini kamu sudah belajar menyimpan data secara **linear** — artinya data disusun satu per satu dalam satu baris, seperti antrian orang di kasir.

```
Array: [Kakek, Ayah, Paman, Kamu, Adik, Sep1, Sep2]
```

Tapi coba kamu bayangkan menyimpan silsilah keluarga dalam bentuk array seperti itu. Bagaimana kamu tahu siapa anak siapa? Bagaimana kamu tahu Paman adalah saudara Ayah, bukan anak Ayah?

Kamu tidak bisa. Informasi tentang **hubungan antar data** hilang begitu saja.

Di sinilah **Tree** hadir sebagai solusi. Tree bukan hanya menyimpan data — ia juga menyimpan **relasi hierarkis** antar data tersebut. Siapa yang di atas, siapa yang di bawah, siapa yang sejajar.

---

## Bagian-Bagian Tree

Sebelum melangkah lebih jauh, ada beberapa istilah yang perlu kamu kenali. Semuanya akan dijelaskan dengan analogi yang sudah kamu kenal.

Kita pakai contoh **pohon mangga sungguhan** sebagai panduan.

```
              [Batang Utama]         ← ROOT
                /         \
          [Dahan A]     [Dahan B]    ← INTERNAL NODE
           /    \            \
        [Ranting] [Ranting]  [Ranting]
           |                    |
         [Daun]              [Daun]  ← LEAF NODE
```

---

### 1. 🌱 Root (Akar)

Root adalah simpul paling atas dalam tree — titik awal dari segalanya. Seperti batang utama pohon mangga yang menopang semua cabang di atasnya. Dalam sebuah tree, hanya ada **satu root**, tidak lebih.

> Pada contoh silsilah keluarga tadi, **Kakek** adalah root-nya.

---

### 2. 🔵 Node (Simpul)

Setiap titik dalam tree disebut **node**. Kakek adalah node. Ayah adalah node. Kamu adalah node. Setiap node menyimpan satu data dan memiliki koneksi ke node lain di bawahnya.

---

### 3. ↔️ Edge (Sisi/Cabang)

Garis yang menghubungkan satu node dengan node lainnya disebut **edge**. Seperti cabang yang menghubungkan batang pohon ke dahannya.

> **Fakta menarik:** Jika ada **7 node** dalam sebuah tree, maka ada tepat **6 edge** — selalu satu lebih sedikit dari jumlah node.

---

### 4. 👨‍👧 Parent dan Child (Orang Tua dan Anak)

| Istilah | Definisi | Contoh |
|---|---|---|
| **Parent** | Node yang berada di atas dan memiliki cabang ke bawah | Ayah |
| **Child** | Node yang berada di bawah parent-nya | Kamu, Adik |

Konsep ini sama persis dengan hubungan keluarga sungguhan — karena memang dari situlah analoginya diambil.

---

### 5. 🍃 Leaf Node (Simpul Daun)

Node yang **tidak punya child sama sekali** disebut leaf node — seperti daun di ujung ranting yang tidak tumbuh cabang lagi.

> Pada contoh silsilah, **Kamu, Adik, Sep1, dan Sep2** adalah leaf node.

---

### 6. 🌿 Subtree

Ambil sembarang node dalam tree, lalu lihat ia beserta semua keturunannya — itu disebut **subtree**. Pohon kecil di dalam pohon yang lebih besar.

```
        Ayah          ← ini adalah subtree tersendiri
        /  \
      Kamu  Adik
```

---

## Tinggi dan Kedalaman

Bayangkan kamu berdiri di depan pohon mangga dan ingin mengukurnya. Kamu bisa mengukur dari dua sudut pandang berbeda.

### 📏 Kedalaman (Depth) — diukur dari atas ke bawah

Seberapa jauh sebuah node dari root.

```
Kakek     → Depth 0
Ayah      → Depth 1
Kamu      → Depth 2
```

> Root selalu berada di **Depth 0**.

---

### 📐 Tinggi (Height) — diukur dari bawah ke atas

Seberapa jauh jarak terpanjang dari sebuah node ke leaf di bawahnya.

```
Kamu (leaf)   → Height 0
Ayah          → Height 1
Kakek (root)  → Height 2
```

> Leaf node selalu memiliki **Height 0**.

---

### Perbandingan Depth vs Height

```
              Kakek        (Depth 0, Height 2)
             /     \
          Ayah     Paman   (Depth 1, Height 1)
          /  \
        Kamu Adik          (Depth 2, Height 0)
```

Jika ada yang bertanya *"berapa tinggi tree ini?"* — jawabannya adalah tinggi dari root-nya. Pada contoh silsilah tadi, **tinggi tree-nya adalah 2**.

---

## Kenapa Kita Perlu Mempelajari Tree?

Kamu mungkin berpikir: *"Oke, ini menarik. Tapi kapan saya akan pakai ini di dunia nyata?"*

Jawabannya: **hampir setiap saat** — hanya saja kamu tidak menyadarinya.

---

### 💻 Struktur Folder Komputer

Saat kamu membuka folder di komputermu, struktur folder itu adalah tree.

```
Documents/
├── Kuliah/
│   ├── Semester1/
│   │   └── struktur-data.pdf
│   └── Semester2/
└── Pribadi/
    └── foto.jpg
```

Ada folder induk, di dalamnya ada subfolder, di dalam subfolder ada file-file. Itu persis hierarki parent-child yang baru saja kita bahas.

---

### 🌐 Halaman Web (DOM)

Saat kamu membuka halaman web, browser-mu membaca kode HTML dan mengubahnya menjadi tree yang disebut **DOM (Document Object Model)**. Setiap tag HTML adalah node dalam tree itu.

```html
<html>              ← Root
  <head>            ← Child dari html
    <title>...</title>
  </head>
  <body>            ← Child dari html
    <h1>...</h1>
    <p>...</p>
  </body>
</html>
```

---

### 🔍 Fitur Autocomplete

Saat kamu mengetik `"str"` dan muncul saran `"struktur data"`, `"string"`, `"streaming"` — di balik layar ada sebuah tree yang sedang bekerja keras memilihkan saran terbaik untukmu.

---

Tree bukan sekadar teori akademis. Ia adalah **fondasi dari banyak sistem nyata** yang kamu gunakan sehari-hari.

---

## Jenis-Jenis Tree

Tree hadir dalam banyak bentuk, masing-masing dirancang untuk memecahkan masalah yang berbeda.

---

### 🌳 General Tree

Tree paling bebas — setiap node boleh punya **berapa pun child**, tidak ada batasan.

```
        A
      / | \
     B  C  D
    /|     |
   E  F    G
```

Seperti silsilah keluarga, di mana seseorang bisa punya satu, dua, tiga anak, atau bahkan tidak sama sekali.

---

### 🌲 Binary Tree

Tree yang lebih disiplin — setiap node maksimal hanya boleh punya **dua child**: satu di kiri, satu di kanan.

```
        A
       / \
      B   C
     / \   \
    D   E   F
```

Pembatasan ini terdengar sederhana, tapi ternyata membuka banyak kemungkinan yang sangat berguna — seperti yang akan kita lihat di bab berikutnya tentang **Binary Search Tree**.

---

### 🌴 N-ary Tree

Tree di mana setiap node boleh punya maksimal **N child**. Lebih fleksibel dari Binary Tree, tapi lebih terstruktur dari General Tree.

```
Contoh 3-ary Tree (maks. 3 child per node):

        A
      / | \
     B  C  D
    /|     |
   E  F    G
```

---

### Perbandingan Singkat

| Jenis | Maks. Child per Node | Kegunaan Umum |
|---|---|---|
| **General Tree** | Tidak terbatas | Silsilah, hierarki organisasi |
| **Binary Tree** | 2 | BST, Heap, Expression Tree |
| **N-ary Tree** | N (ditentukan) | File system, Trie |

---

## Ringkasan

Sampai di sini, kamu sudah memahami tiga hal penting:

**Pertama**, mengapa Tree ada — karena Array dan Linked List tidak bisa menyimpan hubungan hierarkis antar data.

**Kedua**, apa saja bagian-bagian Tree:

| Istilah | Makna Sederhana |
|---|---|
| **Root** | Simpul paling atas, titik awal |
| **Node** | Setiap titik dalam tree |
| **Edge** | Garis penghubung antar node |
| **Parent** | Node yang memiliki child |
| **Child** | Node di bawah parent |
| **Leaf** | Node tanpa child |
| **Subtree** | Pohon kecil di dalam pohon besar |
| **Depth** | Jarak node dari root (atas ke bawah) |
| **Height** | Jarak node ke leaf terbawah (bawah ke atas) |

**Ketiga**, di mana Tree digunakan — dari struktur folder komputer, halaman web, hingga fitur autocomplete yang kamu pakai setiap hari.

---

## ➡️ Selanjutnya: General Tree (BST)

Di bab berikutnya, kita akan masuk ke jenis tree yang paling umum: [**General Tree**](general-tree-struktur-data.md) — pohon yang bisa kita jumpati di kehidupan sehari-hari.

Tapi sebelum itu, pastikan konsep di bab ini sudah benar-benar kamu pahami, karena General Tree dibangun di atas fondasi yang baru saja kita pelajari bersama.

---
## ➡️ Selanjutnya: Binary Search Tree (BST)

Di bab berikutnya, kita akan masuk ke jenis tree yang paling banyak digunakan dalam pemrograman: **Binary Search Tree** — pohon yang bisa mencari data dengan kecepatan luar biasa.

Tapi sebelum itu, pastikan konsep di bab ini sudah benar-benar kamu pahami, karena Binary Search Tree dibangun di atas fondasi yang baru saja kita pelajari bersama.

---
> 📝 **Catatan Penulis:** Materi ini digerate pakai AI claude.
