Assalamuallaikum wr.wb kembali lagi ke github saya mastio1836 kali ini saya akan melanjutkan sebuah program dari lab11 aplikasi sederhana menggunakan framework codeigniter dan hari ini saya akan menambahkan deklarasi CRUD mau tau bagaimana simak penjelasanya siapkan xampp kalian ...kemudia buka website"google,choreme (localhost) lalu php my admin" kalian akan dituju ke sebuah pembuatan table database dan berikut penjelasanya

# Persiapan 
  
   Siapkan database kalian lalu ikuti ini buat database dengan nama lab_ci4 lalu 

   CREATE TABLE artikel (
  id INT(11) auto_increment,
  judul VARCHAR(200) NOT NULL,
  isi TEXT,
  gambar VARCHAR(200),
  status TINYINT(1) DEFAULT 0,
  slug VARCHAR(200),
  PRIMARY KEY(id)
  );

![membuat tabel pada database](https://user-images.githubusercontent.com/56244106/122919637-a7da9600-d38a-11eb-9984-2753ba074b56.JPG)

dan ketika kalian sudah mendeklarasikan kode tersebut hasilnya akan seperti ini

![Struktur database](https://user-images.githubusercontent.com/56244106/122920102-351dea80-d38b-11eb-83f6-6cc2861b83d6.JPG)


# Konfigurasi koneksi database
Selanjutnya membuat konfigurasi untuk menghubungkan dengan database server.
Konfigurasi dapat dilakukan dengan du acara, yaitu pada file app/config/database.php
atau menggunakan file .env. Pada praktikum ini kita gunakan konfigurasi pada file .env.
Konfigurasi dapat dilakukan dengan cara mengubah beberapa kode pada file htdocs\lab11_php_ci\ci4\.env.
- Cari pada line DATABASE
- Ubah data seperti berikut

#database.default.hostname = localhost
#database.default.database = lab_ci4
#database.default.username = root
#database.default.password = 
#database.default.DBDriver = MySQLi
#database.default.DBPrefix =

Hilangkan tanda pagar # didepan. Maka jadi seperti dibawah ini.

database.default.hostname = localhost
database.default.database = lab_ci4
database.default.username = root
database.default.password = 
database.default.DBDriver = MySQLi
database.default.DBPrefix =

![konfigurasi koneksi database](https://user-images.githubusercontent.com/56244106/122920734-e6bd1b80-d38b-11eb-9e0a-217aa6bc0678.JPG)

# Step ke 2
 Membuat Model
Selanjutnya adalah membuat Model untuk memproses data Artikel. Buat file baru pada
direktori app/Models dengan nama ArtikelModel.php 

ikuti langkah sperti pada gambar ini

![input artikel model](https://user-images.githubusercontent.com/56244106/122921148-5b905580-d38c-11eb-9ba5-bab4962d0aa8.JPG)

# Step ke 3
Membuat Controller
Buat Controller baru dengan nama Artikel.php pada direktori app/Controllers.
 
![artikel kontroler baru index](https://user-images.githubusercontent.com/56244106/122935421-85e90f80-d39a-11eb-8355-91063cff685d.JPG)

# Step ke 4
Membuat View
Buat direktori baru dengan nama artikel pada direktori app/views, kemudian buat file
baru dengan nama index.php. 

![input index php](https://user-images.githubusercontent.com/56244106/122936185-2808f780-d39b-11eb-878c-b4498b524981.JPG)

Selanjutnya buka browser kembali, dengan mengakses url http://localhost:8080/artikel

![output artikel belum ada data](https://user-images.githubusercontent.com/56244106/122936516-6999a280-d39b-11eb-9d63-bc51e59ae43e.JPG)

lalu ini adalah pendeklarasian input di database tujuannya agar data tersebut dapat di tampilkan pada layout

![data artikel data base](https://user-images.githubusercontent.com/56244106/122936832-ab2a4d80-d39b-11eb-92ab-fedc42f738c6.JPG)

lalu kalian refresh dan ini adalah hasil outputn dari pendeklarasianya tersbut

![output artikel ketika sudah di tambah di database](https://user-images.githubusercontent.com/56244106/122936985-ce54fd00-d39b-11eb-9c6d-c076da87c75e.JPG)

# step ke 5 
Membuat tampilan detail artikel

public function view($slug)
    {
        $model = new ArtikelModel();
        $artikel = $model->where(['slug' => $slug])->first();
        // Menampilkan error apabila data tidak ada.
        if (!$artikel)
        {
            throw PageNotFoundException::forPageNotFound();
        }
        $title = $artikel['judul'];
        return view('artikel/detail', compact('artikel', 'title'));
    }

![input detail artikel controller](https://user-images.githubusercontent.com/56244106/122939737-1543f200-d39e-11eb-9f3a-0bb74ed49ff3.JPG)

# Step ke 6
Buat view baru untuk halaman detail dengan nama app/views/artikel/detail.php.

![input detail](https://user-images.githubusercontent.com/56244106/122940043-520fe900-d39e-11eb-9d7d-d669f55a1f08.JPG)

# Step ke 7
 Routing baru untuk artikel detail

$routes->get('/artikel/(:any)', 'Artikel::view/$1');

![routes artikel](https://user-images.githubusercontent.com/56244106/122941001-2ccfaa80-d39f-11eb-8a3e-86f70848b99a.JPG)

![output artikel detail pertama](https://user-images.githubusercontent.com/56244106/122941167-5688d180-d39f-11eb-8bf1-b36d2e932976.JPG)


# Step ke 8
Membuat menu admin
Menu admin adalah untuk proses CRUD data artikel. Buat method baru pada
"Controller Artikel dengan nama admin_index(). "

![controller admin index](https://user-images.githubusercontent.com/56244106/122945028-78d01e80-d3a2-11eb-8317-024c603ee4dc.JPG)

Kemudian buat view untuk tampilan admin dengan nama admin_index.php.

![admin index input](https://user-images.githubusercontent.com/56244106/122945808-090e6380-d3a3-11eb-82c7-f9908fc02ae7.JPG)

Tambahkan routing untuk menu admin seperti berikut:

$routes->group('admin', function($routes) {
  $routes->get('artikel', 'Artikel::admin_index');
  $routes->add('artikel/add', 'Artikel::add');
  $routes->add('artikel/edit/(:any)', 'Artikel::edit/$1');
  $routes->get('artikel/delete/(:any)', 'Artikel::delete/$1');
});

Setelah itu buat template header dan footer baru untuk Halaman Admin. Buat file baru dengan nama admin_header.php pada direktori app/view/template

![input admin header](https://user-images.githubusercontent.com/56244106/122949762-1e38c180-d3a6-11eb-8218-90b4f1b1e260.JPG)

Dan Buat file baru lagi dengan nama admin_footer.php pada direktori app/view/template

![input admin footer](https://user-images.githubusercontent.com/56244106/122949865-33adeb80-d3a6-11eb-96f9-2ca4dc60e2c3.JPG)

Kemudian buat file baru lagi dengan nama admin.css pada direktori ci4/public untuk mempercantik tampilan Halaman Admin


/* Reset CSS */
* {
    margin: 0;
    padding: 0;
}
body {
    line-height:1;
    font-size:100%;
    font-family:'Open Sans', sans-serif;
    color:#5a5a5a;
}
#container {
    width: 980px;
    margin: 0 auto;
    box-shadow: 0 0 1em #cccccc;
}

/* header */
header {
    padding: 20px;
}
header h1 {
    margin: 20px 10px;
    color: #b5b5b5;
}

/* navigasi */
nav {
    display: block;
    background-color: #1f5faa;
}
nav a {
    padding: 15px 30px;
    display: inline-block;
    color: #ffffff;
    font-size: 14px;
    text-decoration: none;
    font-weight: bold;
}
nav a.active,
nav a:hover {
    background-color: #2b83ea;
}

/* footer */
footer {
    clear: both;
    background-color: #1d1d1d;
    padding: 20px;
    color:  #eee
}

/* ADMIN TABEL */
body{
    font-family: sans-serif;
}
table{
    border-collapse: collapse;
    margin: 20px;
    width: 95%;
}
table td{
    border: 1px solid #c9c9c9;
    font-size: 19px;
    padding: 10px 8px;
}
table th {
    background:#4d8cd4;
    color:#ffffff;
    font-size: 17px;
    text-align: left;
    border: 1px solid #c9c9c9;
    padding: 13px 15px;
}
table tr {
    background:#ffffff;
    text-align: left;
}
tr:hover{
    background: #e7e7e7;
}

/* ADMIN TOMBOL */
.btn{
    font-size: 14px;
    background-color: #afafaf;
    color: #444444;
    border-radius: 5px;
    padding: 10px 20px;
    margin-top: 8x;
    text-decoration: none;
}

.btn-danger{
    font-size: 14px;
    background-color: rgb(223, 35, 35);
    color: white;
    border-radius: 5px;
    padding: 10px 20px;
    margin-top: 8x;
    text-decoration: none;
}

a:active,
a:hover{
    opacity: 80%;
}

/* TAMBAH ARTIKEL */
textarea{
    width: 94%;
    padding: 10px;
    border: 2px solid gray;
    box-sizing: border-box;
    font-size: 15px;
    margin-left: 20px;
}

input[type=text]{
    width: 94%;
    padding: 10px;
    border: 2px solid gray;
    box-sizing: border-box;
    font-size: 15px;
    margin: 20px;   
}

input[type=submit]{
    padding: 10px;
    background-color: rgb(30, 117, 216);
    color: white;
    box-sizing: border-box;
    font-size: 15px;
    margin: 20px;   
}

input[type=submit]:active,
input[type=submit]:hover{
    opacity: 80%;
}

h2{
    margin-top: 20px;
    margin-left: 20px;
}

Akses menu admin dengan url http://localhost:8080/admin/artikel

![output admin artikel CRUD dan tampilan css](https://user-images.githubusercontent.com/56244106/122950133-69eb6b00-d3a6-11eb-8926-e8d24d550dcf.JPG)

# Step ke 9
 Menambah data artikel
Tambahkan fungsi/method baru pada Controller Artikel dengan nama add()

![controller add](https://user-images.githubusercontent.com/56244106/122950492-b171f700-d3a6-11eb-83e9-9228a90b056b.JPG)

Kemudian buat view untuk form tambah dengan nama form_add.php

![input form add](https://user-images.githubusercontent.com/56244106/122950661-d5cdd380-d3a6-11eb-99b9-6c37f7a6b661.JPG)

Klik menu Tambah Artikel dan inilah hasilnya.  http://localhost:8080/admin/artikel/add

![output add tambah artikel](https://user-images.githubusercontent.com/56244106/122951003-234a4080-d3a7-11eb-88f0-a20b73559185.JPG)

# Step ke 10
Mengubah data
Tambahkan fungsi/method baru pada Controller Artikel dengan nama edit().

    public function edit($id)
    {
        $artikel = new ArtikelModel();

        // validasi data.
        $validation = \Config\Services::validation();
        $validation->setRules(['judul' => 'required']);
        $isDataValid = $validation->withRequest($this->request)->run();
        
        if ($isDataValid)
        {
            $artikel->update($id, [
                'judul' => $this->request->getPost('judul'),
                'isi' => $this->request->getPost('isi'),]);
            return redirect('admin/artikel');
        }

        // ambil data lama
        $data = $artikel->where('id', $id)->first();
        $title = "Edit Artikel";
        return view('artikel/form_edit', compact('title', 'data'));
    }

![controller edit](https://user-images.githubusercontent.com/56244106/122951703-ae2b3b00-d3a7-11eb-91e6-03e3325c7f24.JPG)

Kemudian buat view untuk form tambah dengan nama form_edit.php

![input form edit](https://user-images.githubusercontent.com/56244106/122951911-d87cf880-d3a7-11eb-9877-efc0906a49bd.JPG)

dan ini adalah hasil outputnya
![output halaman edit](https://user-images.githubusercontent.com/56244106/122952073-f9454e00-d3a7-11eb-86d0-90349b966399.JPG)

# Step ke 11
Tambahkan fungsi/method baru pada Controller Artikel dengan nama delete()

    public function delete($id)
    {
        $artikel = new ArtikelModel();
        $artikel->delete($id);
        return redirect('admin/artikel');
    }

![controller dellete](https://user-images.githubusercontent.com/56244106/122952553-44f7f780-d3a8-11eb-823d-350ec3936e85.JPG)


























