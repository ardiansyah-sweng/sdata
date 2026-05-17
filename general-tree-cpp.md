# Mini File Explorer
Kita akan membuat program  yang berfungsi sebagai file explorer sederhana. Di program ini kita bisa membuat folder, subfolder, file di dalam folder atau subfolder. 
Sebagai implementasi dari **General Tree**, berarti kita tidak tahu berapa jumlah ***child*** yang akan dimiliki tiap **parent**.

---
## Spesifikasi Program
1. Membuat folder atau file baru
2. Mencetak struktur direktori beserta isinya
3. Mencari folder atau file
4. Mengedit nama folder atau file
5. Menghapus file atau folder
6. Menambah folder atau file

---
## Langkah 1. Membuat fungsi main()

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    return 0;
}
```
---
## Langkah 2. Membuat tipe data `struct` untuk membuat `Node`
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

int main() {
    return 0;
}
```
---
## Langkah 3. Membuat fungsi `createNode`
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

Node* createNode(string name, bool isFile, Node* parent){
    Node* node = new Node();
    node->name = name;
    node->isFile = isFile;
    node->parent = parent;
    
    return node;
}

int main() {
    return 0;
}
```
---
## Langkah 4. Membuat direktori `root`
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

Node* createNode(string name, bool isFile, Node* parent){
    Node* node = new Node();
    node->name = name;
    node->isFile = isFile;
    node->parent = parent;
    
    return node;
}

int main() {
    Node* root = createNode("/root", false, nullptr);
    cout << root << endl << root->name << endl << root->isFile << endl << root->children.size();
    
    return 0;
}
```
### Output
`0x118db2b0`
`/root`
`0`
`0`

Artinya simpul `root` di alamat memori `0x118db2b0` dengan nama direktori `/root`, bukan sebuah `file` dan ukuran `children` adalah `0` yang artinya masih belum punya simpul anak.

---
## Langkah 5. Menambah fungsi `mkdir` untuk membuat subfolder
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

Node* createNode(string name, bool isFile, Node* parent){
    Node* node = new Node();
    node->name = name;
    node->isFile = isFile;
    node->parent = parent;
    
    return node;
}

void mkdir(Node* parent, string name){
    Node* folder = createNode(name, false, parent);
    parent->children.push_back(folder);
}

int main() {
    Node* root = createNode("/root", false, nullptr);

    mkdir(root, "Documents");
    
    cout << root << endl << root->name << endl << root->isFile << endl << root->children.size();
    
    return 0;
}
```
### Output
`0x2c8172b0`
`/root`
`0`
`1`

Pada tahap ini `root` sudah memiliki satu child yaitu folder `Documents` terlihat dari ukuran *array* `children` sekarang adalah `1`. Namun karena sudah berbentuk `tree` berarti kita harus melakukan traversal agar bisa mencetak subfolder `Documents` tersebut. Untuk itu kita perlu membuat sebuah prosedur untuk mencetaknya yaitu `printTree()`.

---
## Langkah 6. Membuat prosedur `printTree()` untuk mencetak seluruh simpul (folder atau file)
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

Node* createNode(string name, bool isFile, Node* parent){
    Node* node = new Node();
    node->name = name;
    node->isFile = isFile;
    node->parent = parent;
    
    return node;
}

void mkdir(Node* parent, string name){
    Node* folder = createNode(name, false, parent);
    parent->children.push_back(folder);
}

void printTree(Node* node, string indent=""){
    cout << indent;
    if (node->isFile)
        cout << "📄 ";
    else
        cout << "📁 ";
    
    cout << node->name <<" "<<node->children.size() << " " << node << endl;

    for (Node* child: node->children){
        printTree(child, indent + "  ");
    }
}

int main() {
    Node* root = createNode("/root", false, nullptr);

    mkdir(root, "Documents");

    printTree(root);
    
    return 0;
}
```
### Output
```
📁 /root 1 0x2c0222b0
  📁 Documents 0 0x2c022300
```

Artinya saat ini, di `root` yang beralamat di `0x2c0222b0` memiliki `1` subfolder `Documents`. Subfolder `Documents` yang beralamat di `0x2c022300` memiliki `0` child.
Sampai di sini kita sudah berhasil membuat subfolder. Sekarang saatnya kita coba membuat file.

---

## Langkah 7. Membuat prosedur `touch()` untuk menambah file
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

Node* createNode(string name, bool isFile, Node* parent){
    Node* node = new Node();
    node->name = name;
    node->isFile = isFile;
    node->parent = parent;
    
    return node;
}

//membuat direktori
void mkdir(Node* parent, string name){
    Node* folder = createNode(name, false, parent);
    parent->children.push_back(folder);
}

//membuat file
void touch(Node* parent, string name){
    Node* file = createNode(name, true, parent);
    parent->children.push_back(file);
}

void printTree(Node* node, string indent=""){
    cout << indent;
    if (node->isFile)
        cout << "📄 ";
    else
        cout << "📁 ";
    
    cout << node->name <<" "<<node->children.size() << " " << node << endl;

    for (Node* child: node->children){
        printTree(child, indent + "  ");
    }
}

int main() {
    Node* root = createNode("/root", false, nullptr);

    mkdir(root, "Documents");
    touch(root, "system.exe");

    printTree(root);
    
    return 0;
}
```
### Output
```
📁 /root 2 0x2c3592b0
  📁 Documents 0 0x2c359300
  📄 system.exe 0 0x2c359370
```

Kita lihat sekarang setelah membuat subfolder `Documents` kita membuat file bernama `system.exe`. Keduanya di dalam folder utama `/root`. Nah sekarang kita coba membuat folder baru di dalam `Documents`.

Namun, karena `Documents` ada di dalam `/root`, berarti kita butuh masuk atau traverse ke dalam simpul `/root` ini agar bisa mencapai simpul `Documents`. Oleh karena itu kita butuh satu fungsi yang membawa kita menjelajahi folder yang dituju, yaitu `findChild()`.

---
## Langkah 8. Membuat fungsi `findChild()` untuk mengunjungi simpul dalam tree
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

Node* createNode(string name, bool isFile, Node* parent){
    Node* node = new Node();
    node->name = name;
    node->isFile = isFile;
    node->parent = parent;
    
    return node;
}

//membuat direktori
void mkdir(Node* parent, string name){
    Node* folder = createNode(name, false, parent);
    parent->children.push_back(folder);
}

//membuat file
void touch(Node* parent, string name){
    Node* file = createNode(name, true, parent);
    parent->children.push_back(file);
}

void printTree(Node* node, string indent=""){
    cout << indent;
    if (node->isFile)
        cout << "📄 ";
    else
        cout << "📁 ";
    
    cout << node->name <<" "<<node->children.size() << " " << node << endl;

    for (Node* child: node->children){
        printTree(child, indent + "  ");
    }
}

//masuk ke simpul yang dituju
Node* findChild(Node* parent, string name){
    for (Node* child: parent->children){
        if (child->name == name)
            return child;
    }
    return nullptr;
}

int main() {
    Node* root = createNode("/root", false, nullptr);

    mkdir(root, "Documents");
    touch(root, "system.exe");
    
    //masuk ke folder Documents
    Node* documents = findChild(root, "Documents");
    //buat folder Kuliah SData
    mkdir(documents, "Kuliah SData");
    //buat folder Bank Soal SData
    mkdir(documents, "Bank Soal SData");
    
    //masuk ke folder Kuliah SData
    Node* kuliahSData = findChild(documents, "Kuliah SData");
    touch(kuliahSData, "general-tree.cpp");
    touch(kuliahSData, "general-tree.exe");
    mkdir(kuliahSData, "Images");

    printTree(root);
    
    return 0;
}
```
### Output
```
📁 /root 2 0x2cf872b0
  📁 Documents 2 0x2cf87300
    📁 Kuliah SData 3 0x2cf873e0
      📄 general-tree.cpp 0 0x2cf874c0
      📄 general-tree.exe 0 0x2cf87560
      📁 Images 0 0x2cf875e0
    📁 Bank Soal SData 0 0x2cf87430
  📄 system.exe 0 0x2cf87370


=== Code Execution Successful ===
```

Dengan bantuan `findChild()` kita bisa membuat folder atau file dalam subfolder. Caranya kita harus mendeklarasikan dulu variabel folder yang dituju, misalnya `documents` untuk masuk ke folder `Documents`.
Perhatikan sninppet:
```
//masuk ke folder Documents
Node* documents = findChild(root, "Documents");
```
Selanjutnya baru kita buat folder barunya dengan memanggil `mkdir()`.
Perhatikan:
```
mkdir(documents, "Kuliah SData");
```

Hal serupa harus kita lakukan bila untuk membuat file. 

Bila kita perhatikan, program ini tidak mencegah kita bila ingin menambah folder atau file dengan nama yang sama. Padahal hal ini tidak boleh terjadi. Oleh karena itu kita perlu memodifikasi `mkdir()` dan `touch()` agar dapat mencegah penamaan yang sama. 

---
## Langkah 9. Modifikasi `mkdir()` dan `touch()` agar mencegah duplikasi nama folder/file 
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    string name;
    bool isFile;
    Node* parent;
    vector<Node*> children;
};

Node* createNode(string name, bool isFile, Node* parent){
    Node* node = new Node();
    node->name = name;
    node->isFile = isFile;
    node->parent = parent;
    
    return node;
}

Node* findChild(Node* parent, string name){
    for (Node* child: parent->children){
        if (child->name == name)
            return child;
    }
    return nullptr;
}

//membuat direktori
void mkdir(Node* parent, string name){
    
    // CEK DUPLIKAT
    if (findChild(parent, name) != nullptr){
        cout << "ERROR: Nama folder/file sudah ada: "
             << name << endl;

        return;
    }
    
    Node* folder = createNode(name, false, parent);
    parent->children.push_back(folder);
    
    cout << "Folder berhasil dibuat: "
         << name << endl;    
}

//membuat file
void touch(Node* parent, string name){
    
    // CEK DUPLIKAT
    if (findChild(parent, name) != nullptr){
        cout << "ERROR: Nama folder/file sudah ada: "
             << name << endl;

        return;
    }
    
    Node* file = createNode(name, true, parent);
    parent->children.push_back(file);
    
    cout <<"File berhasil dibuat: "<<name<<endl;
}

void printTree(Node* node, string indent=""){
    cout << indent;
    if (node->isFile)
        cout << "📄 ";
    else
        cout << "📁 ";
    
    cout << node->name <<" "<<node->children.size() << " " << node << endl;

    for (Node* child: node->children){
        printTree(child, indent + "  ");
    }
}

int main() {
    Node* root = createNode("/root", false, nullptr);

    mkdir(root, "Documents");
    touch(root, "system.exe");
    
    //masuk ke folder Documents
    Node* documents = findChild(root, "Documents");
    //buat folder Kuliah SData
    mkdir(documents, "Kuliah SData");
    //buat folder Bank Soal SData
    mkdir(documents, "Bank Soal SData");
    
    //masuk ke folder Kuliah SData
    Node* kuliahSData = findChild(documents, "Kuliah SData");
    touch(kuliahSData, "general-tree.cpp");
    touch(kuliahSData, "general-tree.exe");
    mkdir(kuliahSData, "Images");
    printTree(root);
    
    return 0;
}
```
### Output
```
Folder berhasil dibuat: Documents
File berhasil dibuat: system.exe
Folder berhasil dibuat: Kuliah SData
Folder berhasil dibuat: Bank Soal SData
File berhasil dibuat: general-tree.cpp
File berhasil dibuat: general-tree.exe
Folder berhasil dibuat: Images
ERROR: Nama folder/file sudah ada: general-tree.exe
📁 /root 2 0xcc342b0
  📁 Documents 2 0xcc34300
    📁 Kuliah SData 3 0xcc347f0
      📄 general-tree.cpp 0 0xcc348d0
      📄 general-tree.exe 0 0xcc34970
      📁 Images 0 0xcc349f0
    📁 Bank Soal SData 0 0xcc34840
  📄 system.exe 0 0xcc34780


=== Code Execution Successful ===
```

### Catatan
Pastikan `findChild()` dipindah setelah `createNode()` agar bisa dikenali dan dipanggil oleh `mkdir()` dan `touch()`.

---
## Langkah 10. Mengubah nama folder/file

---
## Langkah 11. Menghapus folder/file
---
## Diskusi & Latihan
1. Misalnya kita sudah sampai ke dalam folder `Images`. Bagaimana caranya agar posisi kita balik ke folder satu tingkat ke atasnya yaitu `Kuliah` atau langsung ke `root`?
2. Kode `Cek Duplikat` muncul dua kali di `mkdir()` dan `touch()`. Bagaimana caranya agar jangan muncul dua kali?
3. Kembangkanlah kode ini agar menggunakan antarmuka CLI. User bisa memilih menu tambah folder, tambah file, cari folder/file, edit nama folder/file, hingga hapus file/folder.
   ```
   IF-UAD Mini File Explorer
   1. Create new folder
   2. Create new file
   3. Browse folder/file
   4. Edit folder/file name
   5. Delete folder/file
   6. Find files
   7. Exit

   Choose your menus: [0-6]
   ```