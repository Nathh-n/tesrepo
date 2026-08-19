# Materi Implementasi
## Product Catalog Dashboard - Flutter

Panduan implementasi untuk membangun satu halaman dashboard katalog produk dengan CRUD lokal di memori dan layout responsif.

## 1. Tujuan Pembelajaran

Setelah menyelesaikan tugas, intern diharapkan mampu:

- Menyusun widget tree Flutter yang rapi dan mudah dibaca.
- Membedakan penggunaan `StatelessWidget` dan `StatefulWidget`.
- Memahami peran `BuildContext` pada navigasi, dialog, bottom sheet, dan theme.
- Menggunakan widget layout dasar dari Material Design.
- Membuat tampilan yang beradaptasi terhadap ukuran layar.
- Mengelola state sederhana dengan `setState`.
- Mengimplementasikan alur `Read`, `Create`, `Update`, dan `Delete` secara lokal.

## 2. Konteks Produk

Buat halaman bernama **Product Catalog Dashboard** untuk admin. Admin dapat melihat ringkasan produk, memfilter kategori, menambah produk, mengubah produk, dan menghapus produk.

Data tidak perlu disimpan ke database atau API. Gunakan `List<Product>` selama aplikasi berjalan. Setelah aplikasi ditutup, perubahan boleh hilang.

## 3. Kontrak Data Minimum

Gunakan model sederhana berikut sebagai acuan. Intern boleh menyesuaikan tipe atau nama field selama perilakunya sama.

```dart
class Product {
	Product({
		required this.id,
		required this.name,
		required this.price,
		required this.category,
		required this.imageUrl,
		required this.isActive,
	});

	final int id;
	String name;
	double price;
	String category;
	String imageUrl;
	bool isActive;
}
```

Sediakan minimal 6 data dummy dengan 2 atau lebih kategori. Tampilkan harga dalam format mata uang yang konsisten.

## 4. Konsep Widget yang Wajib Terlihat

| Konsep/widget | Penerapan yang diharapkan |
| --- | --- |
| `Widget` | Pahami widget sebagai building block Flutter. Susun tampilan dengan menggabungkan widget kecil menjadi komponen yang lebih besar. |
| `StatelessWidget` | Gunakan untuk komponen yang tampilannya bergantung pada input dan tidak memiliki state yang berubah, seperti kartu statistik atau kartu produk. |
| `StatefulWidget` | Gunakan untuk halaman dashboard karena daftar produk dan kategori terpilih dapat berubah selama aplikasi berjalan. |
| `BuildContext` | Gunakan context untuk mengakses theme dan ukuran layar, serta sebagai acuan ketika membuka dialog atau bottom sheet. |
| Widget tree | Susun hubungan parent-child dari `MaterialApp` ke `Scaffold`, `SafeArea`, body, hingga komponen produk. Pastikan setiap widget berada pada parent yang tepat. |
| Material Design | Gunakan komponen dan prinsip Material seperti `ThemeData`, typography, warna, icon, elevation, dan feedback interaksi secara konsisten. |
| `Scaffold` | Gunakan sebagai struktur utama halaman untuk menyediakan area `AppBar`, body, dan `FloatingActionButton`. |
| `AppBar` | Gunakan sebagai header halaman untuk menampilkan judul Product Catalog Dashboard dan aksi tingkat halaman bila diperlukan. |
| `Container` | Gunakan sebagai pembungkus dengan ukuran, padding, warna, border radius, atau shadow, terutama untuk stats banner dan empty state. |
| `Row` | Gunakan untuk menyusun beberapa widget secara horizontal, misalnya statistik pada banner dan tombol aksi Edit/Delete pada kartu produk. Atur `mainAxisAlignment` atau `mainAxisSize` agar jarak dan lebar elemen tetap sesuai. |
| `Column` | Gunakan untuk menyusun beberapa widget secara vertikal, misalnya konten halaman, field pada form, informasi kartu produk, dan empty state. Atur `mainAxisAlignment` serta `crossAxisAlignment` agar susunan konten rapi dan sesuai kebutuhan. |
| `Stack` | Gunakan untuk menumpuk widget pada area yang sama, misalnya menempatkan badge di atas stats banner atau overlay pada gambar produk. |
| `ListView` | Gunakan `ListView.builder` untuk membuat filter kategori secara efisien. Atur `scrollDirection: Axis.horizontal` agar kategori dapat digeser ke samping. |
| `GridView` | Gunakan `GridView.builder` untuk menampilkan kartu produk dalam susunan grid yang dibuat sesuai kebutuhan dan jumlah data. |
| `SingleChildScrollView` | Gunakan untuk membuat satu rangkaian konten dapat scroll ketika ukurannya melebihi ruang yang tersedia, misalnya pada form bottom sheet, halaman detail, atau konten panjang. Pada form yang memunculkan keyboard, tambahkan padding bawah berdasarkan `MediaQuery.of(context).viewInsets.bottom`. |
| `SafeArea` | Bungkus konten utama dengan `SafeArea` agar tidak tertutup notch, status bar, atau area sistem pada perangkat tertentu. |
| `MediaQuery` | Gunakan untuk membaca ukuran layar saat menentukan rasio kartu dan `viewInsets.bottom` untuk memberi ruang ketika keyboard muncul. |
| `LayoutBuilder` | Gunakan untuk membaca lebar area yang tersedia dan menentukan jumlah kolom grid: 2 kolom di bawah 600dp dan 4 kolom mulai 600dp. |

## 5. Urutan Implementasi

### Tahap A - Struktur halaman

1. Buat `MaterialApp` dan theme Material.
2. Buat `ProductDashboardPage` sebagai `StatefulWidget`.
3. Tambahkan `Scaffold`, `AppBar`, `SafeArea`, dan `FloatingActionButton`.
4. Pecah UI menjadi komponen kecil, contohnya `StatsBanner`, `CategoryFilter`, `ProductCard`, dan `ProductFormSheet`.

### Tahap B - Tampilan dan data Read

1. Buat `List<Product>` dummy di state halaman.
2. Buat stats banner menggunakan `Container`, `Stack`, dan `Row`.
3. Buat filter kategori dengan `ListView.builder` horizontal.
4. Buat daftar produk dengan `LayoutBuilder` dan `GridView.builder`.
5. Gunakan aturan berikut:
	 - lebar `< 600dp`: 2 kolom;
	 - lebar `>= 600dp`: 4 kolom;
	 - gunakan `MediaQuery` untuk menghitung `childAspectRatio` yang tetap nyaman dibaca.

### Tahap C - Create dan Update

1. Ketika FAB ditekan, buka `showModalBottomSheet`.
2. Buat form dengan input nama, harga, dan kategori.
3. Isi nilai awal ketika mode Edit.
4. Tambahkan `SingleChildScrollView` dan padding dari `MediaQuery.of(context).viewInsets.bottom`.
5. Validasi field wajib dan harga harus berupa angka positif.
6. Mode Create menambahkan produk melalui `setState`.
7. Mode Edit memperbarui produk yang dipilih melalui `setState`.
8. Tutup sheet hanya setelah data valid berhasil disimpan.

### Tahap D - Delete dan empty state

1. Tombol Delete membuka `showDialog` dengan `AlertDialog`.
2. Hapus data hanya setelah user menekan tombol konfirmasi.
3. Pastikan jumlah produk dan tampilan grid langsung berubah.
4. Saat daftar kosong, tampilkan `Container` dan `Column` di tengah halaman dengan teks **Belum Ada Produk** serta ilustrasi/icon yang relevan.

## 6. Aturan Perilaku

- Filter kategori hanya menampilkan produk dari kategori terpilih.
- Sediakan kategori `Semua` untuk mengembalikan seluruh produk.
- Produk aktif dihitung dari `isActive == true`.
- Delete harus memiliki konfirmasi dan tidak boleh terjadi saat dialog dibatalkan.
- Input yang tidak valid tidak boleh menambah atau mengubah data.
- Layout tidak boleh overflow saat keyboard muncul atau saat diuji pada layar sempit.
- Setiap kartu harus menampilkan gambar, nama, harga, kategori/status, Edit, dan Delete.
- Gunakan key atau id produk yang stabil bila diperlukan untuk menjaga state item.

## 7. Acceptance Criteria

Implementasi dinyatakan selesai jika:

- Semua 18 widget/konsep pada tabel dapat ditemukan dan dijelaskan penggunaannya.
- Stats banner, filter horizontal, grid produk, FAB, form, dialog konfirmasi, dan empty state tersedia.
- Create, Read, Update, dan Delete berjalan tanpa restart aplikasi.
- Form Create dan Edit menggunakan sheet yang sama atau abstraksi yang setara.
- Tidak ada overflow pada keyboard, layar mobile, atau grid.
- `flutter analyze` tidak menghasilkan error.
- Aplikasi dapat dijalankan dengan `flutter run`.

## 8. Output

1. Source code Flutter yang dapat dijalankan.
2. Screenshot tampilan.
3. Catatan singkat yang memetakan 18 widget/konsep ke file atau komponen implementasinya.
