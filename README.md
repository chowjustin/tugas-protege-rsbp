# Dokumentasi Pembuatan Ontologi Restoran di Protege

Nama: Justin Chow <br>
NRP: 5025231087

## 1\. Struktur Ontologi

### 1.1. Kelas (Classes)

- **`MenuItem`**: Merepresentasikan semua item yang dapat dipesan dari menu.
  - **Contoh Individu**: `:Cappuccino`, `ChickenSteak`, `NasiGoreng`, `LemonTea`.
- **`Order`**: Merepresentasikan pesanan yang dibuat oleh pelanggan. Setiap pesanan terdiri dari satu atau lebih `MenuItem`.
  - **Contoh Individu**: `:ORD001`, `:ORD002`.
- **`Staff`**: Merepresentasikan staf yang bekerja di restoran (chef dan waiter).
  - **Contoh Individu**: `:GordonRamsay`, `JohnDoe`, `UncleRoger`.
- **`Customer`**: Merepresentasikan pelanggan yang membuat pesanan.
  - **Contoh Individu**: `:Hubner`, `:JamieOliver`.
- **`Ingredient`**: Merepresentasikan bahan-bahan yang digunakan untuk membuat `MenuItem`.
  - **Contoh Individu**: `Chicken`, `CoffeeBeans`, `Milk`,`Garlic`.

### 1.2. Hierarki Kelas (Class Hierarchy)

Contoh hierarki dalam ontologi ini adalah:

- **Hierarki Staf**: Kelas `Staff` memiliki dua subclass, yaitu `Chef` dan `Waiter`, yang membedakan jenis pekerjaan staf.
  ```
  owl:Thing
  └── Staff
      ├── Chef
      └── Waiter
  ```
- **Hierarki Menu Item**: Kelas `MenuItem` memiliki subclass `Food` dan `Beverage`. Kelas `Food` kemudian dibagi lagi menjadi `Appetizer`, `MainCourse`, dan `Dessert`, sementara `Beverage` dibagi menjadi `Coffee` dan `NonCoffee`.
  ```
  owl:Thing
  └── MenuItem
      ├── Food
      │   ├── Appetizer
      │   ├── MainCourse
      │   └── Dessert
      └── Beverage
          ├── Coffee
          └── NonCoffee
  ```

### 1.3. Properti Objek (Object Properties)

- **`hasItem`**: Menghubungkan sebuah `Order` dengan `MenuItem` yang dipesan.
  - **Domain**: `Order`
  - **Range**: `MenuItem`
- **`placedBy`**: Menunjukkan `Customer` yang membuat sebuah `Order`.
  - **Domain**: `Order`
  - **Range**: `Customer`
- **`preparedBy`**: Menunjukkan `Chef` yang menyiapkan sebuah `Order`.
  - **Domain**: `Order`
  - **Range**: `Chef`
- **`servedBy`**: Menunjukkan `Waiter` yang melayani sebuah `Order`.
  - **Domain**: `Order`
  - **Range**: `Waiter`
- **`containsIngredient`**: Menghubungkan sebuah `MenuItem` dengan `Ingredient` yang dikandungnya.
  - **Domain**: `MenuItem`
  - **Range**: `Ingredient`

### 1.4. Properti Data (Data Properties)

- **`hasPrice`**: Memberikan nilai harga (float) untuk setiap `MenuItem`.
  - **Domain**: `MenuItem`
  - **Range**: `xsd:float`
- **`staffID`**: Memberikan ID unik (string) untuk setiap anggota `Staff`.
  - **Domain**: `Staff`
  - **Range**: `xsd:string`
- **`hasAllergenInfo`**: Menyediakan informasi alergen (string) untuk sebuah `Ingredient`.
  - **Domain**: `Ingredient`
  - **Range**: `xsd:string`
- **`orderTimestamp`**: Mencatat waktu dan tanggal (dateTime) saat `Order` dibuat.
  - **Domain**: `Order`
  - **Range**: `xsd:dateTime`

---

## 2\. Axioma

Contoh axioma dalam ontologi restoran ini adalah:

- **Batasan Kardinalitas (Cardinality Restriction)**: Kelas `Order` didefinisikan menggunakan batasan kardinalitas minimum pada properti `hasItem`.

  - **Definisi**: `Order owl:equivalentClass [ owl:onProperty :hasItem ; owl:minQualifiedCardinality "1" ; owl:onClass :MenuItem ]`
  - **Penjelasan**: Axioma ini menyatakan bahwa sebuah entitas dianggap sebagai sebuah `Order` jika dan hanya jika ia memiliki setidaknya satu (`min 1`) properti `hasItem` yang menunjuk ke sebuah `MenuItem`. Ini memastikan bahwa pesanan kosong tidak dapat ada.

---

## 3\. Hasil Reasoning (Penalaran)

Setelah menjalankan reasoner, ontologi ini dinyatakan konsisten karena ada kontradiksi logis yang ditemukan dalam definisi kelas, properti, atau individu. Ontologi ini diuji menggunakan reasoner HermiT yang terintegrasi di Protege.

Reasoner mampu menyimpulkan fakta baru berdasarkan struktur dan axioma yang ada. Saya mencoba Inferensi Berbasis Definisi Kelas (Aksioma):

- Pertama-tama saya membuat sebuah individu bernama `TestOrderValid` dan hanya diberi satu properti, yaitu `hasItem :NasiGoreng`. Individu ini tidak secara eksplisit dinyatakan sebagai anggota kelas `Order`.
- Lalu selanjutnya, saya menjalankan reasoner dan berdasarkan aksioma batasan kardinalitas pada kelas `Order` (sebuah `Order` harus memiliki minimal satu `MenuItem`), reasoner menyimpulkan bahwa `TestOrderValid` adalah anggota dari kelas `Order`.
