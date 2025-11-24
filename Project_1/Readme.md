# **Aplikasi Perpustakaan Universitas Bahagia**

## **1. Struktur Folder Proyek**

```
Perpustakaan/
├── index.html          
├── detail.html         
├── login.html          
├── register.html       
├── menu.html   
├── koneksi
│   └── connection.php
├── css/
│   └── style.css       
└── js/
    └── scripts.js
```

---

## **2. Halaman Utama**

**File:** `index.php`

```php
<?php
include 'includes/header.php';
?>

<header class="row p-4 shadow-sm" style="background: #d0dbe8; color: black;">
    <div class="col-md-12 text-center mb-1">
        <h1 class="display-5 fw-bold">Selamat Datang di Perpustakaan Universitas Bahagia</h1>
        <p class="fs-4 text-muted">Pusat sumber daya ilmu pengetahuan.</p>
    </div>
</header>

<div class="row mb-5 mt-5">
    <div class="col-md-6 mb-5">
        <h3>Universitas Bahagia</h3>
        <p>Perpustakaan kami berlokasi strategis di dalam kompleks Kampus Utama Universitas Bahagia, tepatnya di Gedung Rektorat Lantai 2. Akses mudah bagi seluruh civitas akademika dan masyarakat umum yang terdaftar.</p>
        <p>Kami bangga melayani kebutuhan informasi dan literasi di kompleks Kampus Bahagia.</p>
    </div>
    <div class="col-md-6">
        <h3>Visi dan Misi</h3>
        <dl class="row">
            <dt class="col-sm-1">Visi</dt>
            <dd class="col-sm-11">Menjadi perpustakaan digital terdepan yang mendukung riset dan inovasi di tingkat global.</dd>         
            <dt class="col-sm-1">Misi</dt>
            <dd class="col-sm-11">
                <ul class="list-unstyled">
                    <li>Menyediakan koleksi terlengkap.</li>
                    <li>Memberikan pelayanan berbasis teknologi.</li>
                    <li>Mendukung proses belajar mengajar yang efektif.</li>
                </ul>
            </dd>
        </dl>
    </div>
</div>

<h1 class="mb-4">Koleksi Novel Terbaru</h1>

<div class="row row-cols-1 row-cols-md-3 g-4">
    
    <div class="col"> 
        <div class="card h-100 shadow-sm">
            <img src="images/bumi_manusia.jpeg" class="card-img-top" alt="Cover Bumi Manusia" style="height: 250px; object-fit: cover;">
            <div class="card-body">
                <h5 class="card-title">Bumi Manusia</h5>
                <p class="card-text text-muted">Penulis: Pramoedya Ananta Toer</p>
                <p class="card-text">Bumi Manusia adalah salah satu karya besar dalam ranah sastra Indonesia...</p>
                <a href="detail.php?id=1" class="btn btn-primary">Lihat Detail</a>
            </div>
        </div>
    </div>
    
    <div class="col">
        <div class="card h-100 shadow-sm">
            <img src="images/An_Enola_Holmes_Mystery.jpeg" class="card-img-top" alt="Cover An Enola Holmes Mystery" style="height: 250px; object-fit: cover;">
            <div class="card-body">
                <h5 class="card-title">An Enola Holmes Mystery The Case of The Missing Marquess...</h5>
                <p class="card-text text-muted">Penulis: Nancy Springer</p>
                <p class="card-text">Novel The Case of the Missing Marquess an Enola Holmes Mystery bercerita tentang Enola Holmes...</p>
                <a href="detail.php?id=2" class="btn btn-primary">Lihat Detail</a>
            </div>
        </div>
    </div>
    
    <div class="col">
        <div class="card h-100 shadow-sm"> 
             <img src="images/Seribu_Wajah_Ayah.jpeg" class="card-img-top" alt="Cover Seribu Wajah Ayah" style="height: 250px; object-fit: cover;">
            <div class="card-body">
                <h5 class="card-title">Seribu Wajah Ayah</h5>
                <p class="card-text text-muted">Penulis: Nurun Ala</p>
                <p class="card-text">Kasih orang tua merupakan kasih sepanjang masa, kasih orang tua terhadap anaknya memang tidak ada batas...</p>
                <a href="detail.php?id=3" class="btn btn-primary">Lihat Detail</a>
            </div>
        </div>
    </div>

    <div class="col-12 text-center mt-5">
        <a href="menu.php" class="btn btn-lg btn-success">Lihat Katalog Lengkap ➡️</a>
    </div>
    
</div> 


<?php 
include 'includes/footer.php'; 
?>
```

---

## **3. Halaman Register**

**File:** `register.php`

```php
<?php
include 'koneksi/connection.php'; // Pastikan path ini benar

$success_message = "";
$error_message = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $namaLengkap = $_POST['nama_lengkap'];
    $nimMahasiswa = $_POST['nim_mahasiswa'];
    $password = $_POST['password_reg'];

    $passwordHash = password_hash($password, PASSWORD_DEFAULT);

    // Cek apakah NIM sudah terdaftar
    $stmt_check = $conn->prepare("SELECT NIM FROM users WHERE NIM = ?");
    $stmt_check->execute([$nimMahasiswa]);
    $nim_exist = $stmt_check->rowCount();

    if ($nim_exist > 0) {
        $error_message = "NIM/Username sudah terdaftar.";
    } else {
        // Masukkan data ke database
        $stmt_insert = $conn->prepare("INSERT INTO users (Nama, NIM, Password) VALUES (?, ?, ?)");
        
        if ($stmt_insert->execute([$namaLengkap, $nimMahasiswa, $passwordHash])) {
            $success_message = "Pendaftaran berhasil! Silakan login.";
        } else {
            $error_message = "Pendaftaran gagal: " . $stmt_insert->errorInfo()[2];
        }
    }
}

include 'includes/header.php'; // Pastikan path ini benar
?>

<div class="container my-5">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <div class="card shadow-lg p-5">
                <h3 class="card-title text-center mb-4">Daftar Anggota Baru</h3>
                
                <?php if ($success_message): ?>
                    <div class="alert alert-success" role="alert"><?php echo $success_message; ?> <a href="login.php">Login di sini</a></div>
                <?php endif; ?>
                <?php if ($error_message): ?>
                    <div class="alert alert-danger" role="alert"><?php echo $error_message; ?></div>
                <?php endif; ?>

                <form method="POST" action="register.php">
                    <div class="mb-3">
                        <label for="nama_lengkap" class="form-label">Nama Lengkap</label>
                        <input type="text" class="form-control" id="nama_lengkap" name="nama_lengkap" required>
                    </div>
                    <div class="mb-3">
                        <label for="nim_mahasiswa" class="form-label">NIM Mahasiswa / Username</label>
                        <input type="text" class="form-control" id="nim_mahasiswa" name="nim_mahasiswa" required>
                    </div>
                    <div class="mb-3">
                        <label for="password_reg" class="form-label">Password</label>
                        <div class="input-group">
                            <input type="password" class="form-control" id="password_reg" name="password_reg" required>
                            <button class="btn btn-outline-secondary" type="button" id="togglePasswordReg"> 
                                <i class="fas fa-eye" id="eyeIconReg"></i>
                            </button>
                        </div>
                    </div>
                    <button type="submit" class="btn btn-success w-100 mb-3">Daftar Sekarang</button>
                </form>
                <p class="text-center">Sudah punya akun? <a href="login.php">Login di sini</a></p>
            </div>
        </div>
    </div>
</div>

<?php
include 'includes/footer.php';
?>

<script>
    document.getElementById('togglePasswordReg').addEventListener('click', function (e) {
        const passwordField = document.getElementById('password_reg');
        const icon = document.getElementById('eyeIconReg');
        
        const type = passwordField.getAttribute('type') === 'password' ? 'text' : 'password';
        passwordField.setAttribute('type', type);
        
        icon.classList.toggle('fa-eye');
        icon.classList.toggle('fa-eye-slash');
    });
</script>
```

---

## **4. Halaman Login**

**File:** `login.php`

```php
<?php
include 'koneksi/connection.php';

$error_message = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $nim = $_POST['username'];
    $password = $_POST['password'];

    $stmt = $conn->prepare("SELECT NIM, Password, Nama FROM users WHERE NIM = ?"); 
    $stmt->execute([$nim]);
    $row = $stmt->fetch(PDO::FETCH_ASSOC);

    if ($row) {
        if (password_verify($password, $row['Password'])) { 
            session_start();
            $_SESSION['loggedin'] = true;
            $_SESSION['nim'] = $row['NIM'];
            $_SESSION['nama'] = $row['Nama'];
            
            header("Location: menu.php");
            exit;
        } else {
            $error_message = "Password salah.";
        }
    } else {
        $error_message = "Username atau Nomor Anggota tidak ditemukan.";
    }
}

include 'includes/header.php'; 
?>

<div class="container my-5"> 
    <div class="row justify-content-center">
        <div class="col-md-6">
            <div class="card shadow-lg p-5">
                <h3 class="card-title text-center mb-4">Login Anggota Perpustakaan</h3>
                
                <?php if ($error_message): ?>
                    <div class="alert alert-danger" role="alert"><?php echo $error_message; ?></div>
                <?php endif; ?>

                <form method="POST" action="login.php">
                    <div class="mb-3">
                        <label for="username" class="form-label">Username / Nomor Anggota</label>
                        <input type="text" class="form-control" id="username" name="username" required>
                    </div>
                    <div class="mb-3">
                        <label for="password" class="form-label">Password</label>
                        <div class="input-group">
                            <input type="password" class="form-control" id="password" name="password" required>
                            <button class="btn btn-outline-secondary" type="button" id="togglePassword">
                                <i class="fas fa-eye" id="eyeIcon"></i>
                            </button>
                        </div>
                    </div>
                    <button type="submit" class="btn btn-primary w-100 mb-3">Login</button>
                </form>
                <p class="text-center">Belum punya akun? <a href="register.php">Daftar di sini</a></p>
            </div>
        </div>
    </div>
</div>

<?php
include 'includes/footer.php'; 
?>

<script>
    document.getElementById('togglePassword').addEventListener('click', function (e) {
        const passwordField = document.getElementById('password');
        const icon = document.getElementById('eyeIcon');
        
        const type = passwordField.getAttribute('type') === 'password' ? 'text' : 'password';
        passwordField.setAttribute('type', type);
        
        icon.classList.toggle('fa-eye');
        icon.classList.toggle('fa-eye-slash');
    });
</script>
```

---

## **5. Halaman Menu Utama**

**File:** `menu.php`

```php
<?php
include 'includes/header.php';
?>

<style>
    .card-img-top {
        height: 250px; 
        object-fit: cover;
    }
    .alert-custom-info {
        background-color: #e0f7fa; 
        border-left: 5px solid #00bcd4; 
        color: #006064;
        padding: 1.5rem;
        border-radius: .5rem;
    }
</style>

<div class="container my-5">

    <div class="container my-5">

        <div class="alert alert-custom-info shadow-sm" role="alert">
            <h4 class="alert-heading"><i class="fas fa-hand-wave me-2"></i> Selamat Datang, Anggota Perpustakaan!</h4>
            <p>Anda telah berhasil masuk ke Dashboard Anggota. Di sini Anda bisa melihat buku yang sedang Anda pinjam dan menjelajahi katalog lengkap kami.</p>
        </div>

        <h4 class="mt-5 mb-3 fw-bold text-dark">Daftar Buku yang Sedang Dipinjam</h4>
        <div class="table-responsive shadow-sm rounded-3">
            <table class="table table-striped table-hover align-middle bg-white">
                <thead class="table-dark">
                    <tr>
                        <th>Judul Buku</th>
                        <th>Tanggal Pinjam</th>
                        <th>Tanggal Pengembalian</th>
                        <th>Status</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Bumi Manusia</td>
                        <td>2025-11-01</td>
                        <td>2025-11-15</td>
                        <td><span class="badge text-bg-danger"><i class="fas fa-clock"></i> TERLAMBAT</span></td>
                    </tr>
                    <tr>
                        <td>Laskar Pelangi</td>
                        <td>2025-11-15</td>
                        <td>2025-11-29</td>
                        <td><span class="badge text-bg-success"><i class="fas fa-check-circle"></i> Aktif</span></td>
                    </tr>
                </tbody>
            </table>
        </div>

        <h4 class="mt-5 mb-4 fw-bold text-dark">Katalog Lengkap Perpustakaan</h4>
        
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 g-4">
            
            <div class="col"> 
                <div class="card h-100 shadow-lg border-0 rounded-4 overflow-hidden transition-shadow">
                    <img src="images/bumi_manusia.jpeg" class="card-img-top" alt="Cover Bumi Manusia">
                    <div class="card-body d-flex flex-column">
                        <h5 class="card-title text-primary">Bumi Manusia</h5>
                        <p class="card-text small text-muted">Penulis: Pramoedya Ananta Toer</p>
                        <p class="card-text flex-grow-1 small">Bumi Manusia adalah salah satu karya besar dalam ranah sastra Indonesia...</p>
                        <a href="detail.php?id=1" class="btn btn-outline-primary mt-3">Lihat Detail</a>
                    </div>
                </div>
            </div>
            
            <div class="col">
                <div class="card h-100 shadow-lg border-0 rounded-4 overflow-hidden">
                    <img src="images/An_Enola_Holmes_Mystery.jpeg" class="card-img-top" alt="Cover An Enola Holmes Mystery">
                    <div class="card-body d-flex flex-column">
                        <h5 class="card-title text-success">An Enola Holmes Mystery</h5>
                        <p class="card-text small text-muted">Penulis: Nancy Springer</p>
                        <p class="card-text flex-grow-1 small">Novel The Case of the Missing Marquess bercerita tentang Enola Holmes, adik bungsu dari kakak-beradik detektif terkenal...</p>
                        <a href="detail.php?id=2" class="btn btn-outline-success mt-3">Lihat Detail</a>
                    </div>
                </div>
            </div>
            
            <div class="col">
                <div class="card h-100 shadow-lg border-0 rounded-4 overflow-hidden"> 
                    <img src="images/Seribu_Wajah_Ayah.jpeg" class="card-img-top" alt="Cover Seribu Wajah Ayah">
                    <div class="card-body d-flex flex-column">
                        <h5 class="card-title text-warning">Seribu Wajah Ayah</h5>
                        <p class="card-text small text-muted">Penulis: Nurun Ala</p>
                        <p class="card-text flex-grow-1 small">Kasih orang tua merupakan kasih sepanjang masa, kasih orang tua terhadap anaknya memang tidak ada batas...</p>
                        <a href="detail.php?id=3" class="btn btn-outline-warning mt-3">Lihat Detail</a>
                    </div>
                </div>
            </div>

            <div class="col">
                <div class="card h-100 shadow-lg border-0 rounded-4 overflow-hidden"> 
                    <img src="images/Atomic_Habit.jpeg" class="card-img-top" alt="Cover Atomic Habits">
                    <div class="card-body d-flex flex-column">
                        <h5 class="card-title text-danger">Atomic Habits</h5>
                        <p class="card-text small text-muted">Penulis: James Clear</p>
                        <p class="card-text flex-grow-1 small">Membahas tentang cara membentuk kebiasaan yang baik dan menghilangkan kebiasaan buruk dengan perubahan kecil.</p>
                        <a href="detail.php?id=4" class="btn btn-outline-danger mt-3">Lihat Detail</a>
                    </div>
                </div>
            </div>

        </div> 
    </div>
</div>

<?php
include 'includes/footer.php';
?>
```

---

## **6. Halaman Detail**

**File:** `detail.php`

```php
<?php
$npm = "23552011318";
$nama = "Anisa Wulandari";
$kelas = "TIF RP-23 CNS A";
?>
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title id="bookTitleMeta">Perpustakaan Universitas Bahagia | Memuat...</title> 
    <link rel="stylesheet" href="bootstrap-5.3.8-dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        body {
            background-color: #f8f9fa;
        }
        .book-cover {
            max-height: 450px;
            object-fit: cover;
            width: 100%;
            border-radius: 0.5rem;
            box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease-in-out;
        }
        .book-cover:hover {
            transform: scale(1.02);
        }
        .synopsis-content p {
            margin-bottom: 1rem;
            line-height: 1.6;
        }
    </style>
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark shadow-sm sticky-top">
        <div class="container">
            <a class="navbar-brand" href="index.php">📚 Perpustakaan Universitas Bahagia</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <a class="nav-link" href="index.php">Beranda</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="menu.php">Katalog Buku</a>
                    </li>
                    <li class="nav-item d-flex align-items-center ms-md-2 mt-2 mt-md-0">
                        <a class="nav-link btn btn-sm btn-outline-light" href="login.php" title="Login">
                            <i class="fas fa-user-circle fa-lg"></i>
                            <span class="d-md-none ms-2">Login</span>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <div class="container my-5">
        <div id="book-details-container" class="row">
            <div class="col-12 text-center p-5">
                <div class="spinner-border text-primary" role="status">
                    <span class="visually-hidden">Memuat...</span>
                </div>
                <p class="mt-3 text-muted">Memuat detail buku...</p>
            </div>
        </div>
    </div>

    <div class="bg-dark text-white">
        <div class="text-center py-2">
            <p class="mb-0 small">@Copyright by <?php echo "{$npm}_{$nama}_{$kelas}"; ?></p>
        </div>
    </div>

    <script src="bootstrap-5.3.8-dist/js/bootstrap.bundle.min.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            
            const books = [
                {
                    id: 1, 
                    title: "Bumi Manusia",
                    author: "Pramoedya Ananta Toer",
                    publisher: "Lentera Dipantara",
                    year: 2005,
                    isbn: "978-979-97334-1-8",
                    pages: 535,
                    category: "Fiksi Sejarah",
                    synopsis: "Bumi Manusia adalah volume pertama dari Tetralogi Buru yang fenomenal.\nNovel ini mengambil latar belakang masa kolonial Belanda di Surabaya pada akhir abad ke-19. \nKisah ini berpusat pada Minke, seorang pribumi Jawa yang beruntung dapat bersekolah di HBS (sekolah menengah Belanda) dan jatuh cinta pada Annelies, putri seorang nyai Belanda. Melalui konflik Minke, Pramoedya menggambarkan perjuangan kaum pribumi mencari identitas dan keadilan di tengah diskriminasi rasial yang kejam.",
                    stock: 5,
                    image: "images/bumi_manusia.jpeg" 
                },
                {
                    id: 2, 
                    title: "An Enola Holmes Mystery: The Case of The Missing Marquess",
                    author: "Nancy Springer",
                    publisher: "Noura Publishing",
                    year: 2020,
                    isbn: "978-602-38520-2-1",
                    pages: 236,
                    category: "Fiksi Misteri Remaja",
                    synopsis: "Novel ini memperkenalkan Enola Holmes, adik bungsu dari kakak beradik detektif terkenal, Mycroft dan Sherlock Holmes. Di ulang tahunnya yang keenam belas, ibu Enola menghilang secara misterius, hanya meninggalkan hadiah aneh dan petunjuk samar. \nSaat kakak-kakaknya berencana mengirimnya ke sekolah asrama, Enola memutuskan untuk melarikan diri ke London. Dalam perjalanannya, ia menemukan dirinya terlibat dalam kasus hilangnya seorang marquess muda. Menggunakan kecerdikan dan kemampuannya, Enola harus menghindari kakak-kakaknya yang ingin mengembalikannya sambil memecahkan kasus pertamanya yang berbahaya.",
                    stock: 3,
                    image: "images/An_Enola_Holmes_Mystery.jpeg" 
                },
                {
                    id: 3, 
                    title: "Seribu Wajah Ayah",
                    author: "Nurun Ala",
                    publisher: "Mizan",
                    year: 2022,
                    isbn: "978-602-441-030-2",
                    pages: 250,
                    category: "Fiksi Kontemporer",
                    synopsis: "Sebuah koleksi cerita yang menyentuh hati tentang berbagai peran dan pengorbanan seorang ayah dalam hidup anak-anaknya.\nDari kisah inspiratif hingga yang mengharukan, buku ini merayakan kasih sayang orang tua yang tak terhingga dan berbagai wajah yang ditampilkan seorang ayah untuk memastikan kebahagiaan keluarganya.",
                    stock: 10,
                    image: "images/Seribu_Wajah_Ayah.jpeg" 
                },
                {
                    id: 4, 
                    title: "Atomic Habits: Perubahan Kecil dengan Hasil Luar Biasa",
                    author: "James Clear",
                    publisher: "Gramedia Pustaka Utama",
                    year: 2019,
                    isbn: "978-602-06331-7-1",
                    pages: 352,
                    category: "Pengembangan Diri",
                    synopsis: "Atomic Habits menawarkan kerangka kerja yang telah terbukti untuk meningkatkan diri setiap hari. James Clear, salah satu pakar terkemuka dunia tentang pembentukan kebiasaan, mengungkapkan strategi praktis yang akan mengajarkan Anda cara membentuk kebiasaan yang baik, menghilangkan yang buruk, dan menguasai perilaku kecil yang mengarah pada hasil yang luar biasa. \nBuku ini menjelaskan cara sederhana untuk membuat perubahan kecil yang akan mengubah hidup Anda.",
                    stock: 7,
                    image: "images/Atomic_Habit.jpeg" 
                }
            ];

            function displayBookDetails() {
                const container = document.getElementById('book-details-container');
                const titleMeta = document.getElementById('bookTitleMeta');
                
                if (!container) {
                    return;
                }

                try {
                    const urlParams = new URLSearchParams(window.location.search);
                    const bookId = parseInt(urlParams.get('id')) || 1; 

                    const book = books.find(b => b.id === bookId);
                    
                    if (!book) {
                        container.innerHTML = `
                            <div class="col-12 text-center my-5">
                                <div class="alert alert-danger shadow-sm" role="alert">
                                    <h4 class="alert-heading">Buku Tidak Ditemukan!</h4>
                                    <p>ID buku yang diminta tidak valid atau tidak ada dalam katalog. (ID Dicari: ${bookId})</p>
                                    <hr>
                                    <a href="menu.php" class="btn btn-danger btn-lg">Kembali ke Katalog</a>
                                </div>
                            </div>
                        `;
                        if (titleMeta) titleMeta.innerText = "Perpustakaan Universitas Bahagia | Tidak Ditemukan";
                        return;
                    }

                    if (titleMeta) {
                        titleMeta.innerText = `Perpustakaan Universitas Bahagia | ${book.title}`;
                    }

                    const statusClass = book.stock > 0 ? 'alert-success' : 'alert-danger';
                    const statusText = book.stock > 0 ? `TERSEDIA (Stok: ${book.stock} eksemplar)` : `TIDAK TERSEDIA (Stok: 0)`;
                    const buttonDisabled = book.stock > 0 ? '' : 'disabled';

                    const htmlContent = `
                        <div class="col-md-4 text-center mb-4">
                            <img src="${book.image}" class="img-fluid rounded-lg book-cover" alt="Cover ${book.title}" 
                                onerror="this.onerror=null; this.src='images/placeholder.jpeg'" loading="lazy">
                        </div>

                        <div class="col-md-8">
                            <h1 class="display-5 fw-bold text-primary">${book.title}</h1>
                            <p class="lead text-secondary">Penulis: ${book.author}</p>
                            <hr class="mb-4">
                            
                            <div class="row mb-4 small bg-light p-3 rounded-lg shadow-sm">
                                <div class="col-sm-6 mb-2"><strong>Penerbit:</strong> ${book.publisher}</div>
                                <div class="col-sm-6 mb-2"><strong>Tahun Terbit:</strong> ${book.year}</div>
                                <div class="col-sm-6 mb-2"><strong>ISBN:</strong> ${book.isbn}</div>
                                <div class="col-sm-6 mb-2"><strong>Halaman:</strong> ${book.pages} halaman</div>
                                <div class="col-sm-12"><strong>Kategori:</strong> <span class="badge bg-info text-dark">${book.category}</span></div>
                            </div>

                            <h3 class="mt-4">Sinopsis</h3>
                            <div class="synopsis-content">
                                ${book.synopsis.split('\n').map(p => p.trim() ? `<p class="text-dark">${p.trim()}</p>` : '').join('')}
                            </div>
                            
                            <div class="alert ${statusClass} mt-4 shadow-sm">
                                <i class="fas fa-cubes me-2"></i>
                                **Status Ketersediaan:** ${statusText}
                            </div>

                            <div class="mt-4">
                                <a href="login.php" class="btn btn-success btn-lg ${buttonDisabled} me-2 shadow">
                                    <i class="fas fa-book-reader me-2"></i>Pinjam Sekarang
                                </a>
                                <a href="menu.php" class="btn btn-outline-secondary btn-lg">
                                    <i class="fas fa-list me-2"></i>Kembali ke Katalog
                                </a>
                            </div>
                        </div>
                    `;
                    
                    container.innerHTML = htmlContent;

                } catch (error) {
                    container.innerHTML = `
                        <div class="col-12 text-center my-5">
                            <div class="alert alert-danger shadow-sm" role="alert">
                                <h4 class="alert-heading">KESALAHAN KRITIS!</h4>
                                <p>Terjadi kesalahan saat memproses data: ${error.message}. Halaman tidak dapat dimuat.</p>
                                <hr>
                                <a href="menu.php" class="btn btn-danger btn-lg">Kembali ke Katalog</a>
                            </div>
                        </div>
                    `;
                }
            }

            displayBookDetails();
        });
    </script>
</body>
</html>
```

---

## **7. scripts.js**

**File:** `scripts.js`

```js
function validateLogin() {
    const inputNomorAnggota = document.getElementById('username').value;
    const inputPassword = document.getElementById('password').value;
    
    // --- Lakukan Validasi Sederhana (Contoh) ---
    // Ganti logika ini dengan pemeriksaan Local Storage/API di Project 2
    
    // Contoh Akun Statis untuk demonstrasi:
    const VALID_NOMOR = "23552011318";
    const VALID_PASS = "anisa123";

    if (inputNomorAnggota === VALID_NOMOR && inputPassword === VALID_PASS) {
        // 1. Jika valid, simpan status login (misalnya ke sessionStorage)
        sessionStorage.setItem('isLoggedIn', 'true');
        
        // 2. Arahkan ke halaman menu
        window.location.href = "menu.html"; 
    } else {
        alert("Nomor Anggota atau Password salah. Coba lagi!");
    }
}

function handleRegistration() {
    const nama = document.getElementById('nama_lengkap').value;
    const nomor = document.getElementById('nim_mahasiswa').value; // Ambil nilai ini
    const pass = document.getElementById('password_reg').value;

    // ... (Logika menyimpan data, menggunakan 'nomor' sebagai kunci) ...

    alert("Registrasi berhasil! Silakan login menggunakan Nomor Anggota.");
    window.location.href = "login.html"; 
}


// Fungsi umum untuk mengatur Tampilkan/Sembunyikan Password
function setupPasswordToggle(inputId, toggleId) {
    const passwordInput = document.getElementById(inputId);
    const toggleButton = document.getElementById(toggleId);

    if (passwordInput && toggleButton) {
        toggleButton.addEventListener('click', function () {
            // Mengubah tipe input antara 'password' dan 'text'
            const type = passwordInput.getAttribute('type') === 'password' ? 'text' : 'password';
            passwordInput.setAttribute('type', type);
            
            // Mengubah ikon mata (mata terbuka <-> mata tertutup)
            this.querySelector('i').classList.toggle('fa-eye');
            this.querySelector('i').classList.toggle('fa-eye-slash');
        });
    }
}

// Panggil fungsi setelah dokumen selesai dimuat (untuk menghindari error "is not defined")
document.addEventListener('DOMContentLoaded', function() {
    // 1. Setup untuk halaman LOGIN
    // Memanggil setupPasswordToggle dengan ID input dan tombol toggle dari login.html
    setupPasswordToggle('password', 'togglePassword');

    // 2. Setup untuk halaman REGISTRASI (Asumsikan ID input password adalah 'password_reg')
    // Jika Anda memiliki field password lain di register, pastikan ID-nya unik:
    setupPasswordToggle('password_reg', 'togglePasswordReg'); 
    
});

document.addEventListener('DOMContentLoaded', function() {
            
    const books = [
        {
            id: 1, 
            title: "Bumi Manusia",
            author: "Pramoedya Ananta Toer",
            publisher: "Lentera Dipantara",
            year: 2005,
            isbn: "978-979-97334-1-8",
            pages: 535,
            category: "Fiksi Sejarah",
            synopsis: "Bumi Manusia adalah volume pertama dari Tetralogi Buru yang fenomenal.\nNovel ini mengambil latar belakang masa kolonial Belanda di Surabaya pada akhir abad ke-19. \nKisah ini berpusat pada Minke, seorang pribumi Jawa yang beruntung dapat bersekolah di HBS (sekolah menengah Belanda) dan jatuh cinta pada Annelies, putri seorang nyai Belanda. Melalui konflik Minke, Pramoedya menggambarkan perjuangan kaum pribumi mencari identitas dan keadilan di tengah diskriminasi rasial yang kejam.",
            stock: 5,
            image: "images/bumi_manusia.jpeg" 
        },
        {
            id: 2, 
            title: "An Enola Holmes Mystery: The Case of The Missing Marquess",
            author: "Nancy Springer",
            publisher: "Noura Publishing",
            year: 2020,
            isbn: "978-602-38520-2-1",
            pages: 236,
            category: "Fiksi Misteri Remaja",
            synopsis: "Novel ini memperkenalkan Enola Holmes, adik bungsu dari kakak beradik detektif terkenal, Mycroft dan Sherlock Holmes. Di ulang tahunnya yang keenam belas, ibu Enola menghilang secara misterius, hanya meninggalkan hadiah aneh dan petunjuk samar. \nSaat kakak-kakaknya berencana mengirimnya ke sekolah asrama, Enola memutuskan untuk melarikan diri ke London. Dalam perjalanannya, ia menemukan dirinya terlibat dalam kasus hilangnya seorang marquess muda. Menggunakan kecerdikan dan kemampuannya, Enola harus menghindari kakak-kakaknya yang ingin mengembalikannya sambil memecahkan kasus pertamanya yang berbahaya.",
            stock: 3,
            image: "images/An_Enola_Holmes_Mystery.jpeg" 
        },
        {
            id: 3, 
            title: "Seribu Wajah Ayah",
            author: "Nurun Ala",
            publisher: "Mizan",
            year: 2022,
            isbn: "978-602-441-030-2",
            pages: 250,
            category: "Fiksi Kontemporer",
            synopsis: "Sebuah koleksi cerita yang menyentuh hati tentang berbagai peran dan pengorbanan seorang ayah dalam hidup anak-anaknya.\nDari kisah inspiratif hingga yang mengharukan, buku ini merayakan kasih sayang orang tua yang tak terhingga dan berbagai wajah yang ditampilkan seorang ayah untuk memastikan kebahagiaan keluarganya.",
            stock: 10,
            image: "images/Seribu_Wajah_Ayah.jpeg" 
        },
        {
            id: 4, 
            title: "Atomic Habits: Perubahan Kecil dengan Hasil Luar Biasa",
            author: "James Clear",
            publisher: "Gramedia Pustaka Utama",
            year: 2019,
            isbn: "978-602-06331-7-1",
            pages: 352,
            category: "Pengembangan Diri",
            synopsis: "Atomic Habits menawarkan kerangka kerja yang telah terbukti untuk meningkatkan diri setiap hari. James Clear, salah satu pakar terkemuka dunia tentang pembentukan kebiasaan, mengungkapkan strategi praktis yang akan mengajarkan Anda cara membentuk kebiasaan yang baik, menghilangkan yang buruk, dan menguasai perilaku kecil yang mengarah pada hasil yang luar biasa. \nBuku ini menjelaskan cara sederhana untuk membuat perubahan kecil yang akan mengubah hidup Anda.",
            stock: 7,
            image: "images/Atomic_Habit.jpeg" 
        }
    ];

    function displayBookDetails() {
        const container = document.getElementById('book-details-container');
        const titleMeta = document.getElementById('bookTitleMeta');
        
        if (!container) {
            return;
        }

        try {
            const urlParams = new URLSearchParams(window.location.search);
            const bookId = parseInt(urlParams.get('id')) || 1; 

            const book = books.find(b => b.id === bookId);
            
            if (!book) {
                container.innerHTML = `
                    <div class="col-12 text-center my-5">
                        <div class="alert alert-danger shadow-sm" role="alert">
                            <h4 class="alert-heading">Buku Tidak Ditemukan!</h4>
                            <p>ID buku yang diminta tidak valid atau tidak ada dalam katalog. (ID Dicari: ${bookId})</p>
                            <hr>
                            <a href="menu_utama.php" class="btn btn-danger btn-lg">Kembali ke Katalog</a>
                        </div>
                    </div>
                `;
                if (titleMeta) titleMeta.innerText = "Perpustakaan Universitas Bahagia | Tidak Ditemukan";
                return;
            }

            if (titleMeta) {
                titleMeta.innerText = `Perpustakaan Universitas Bahagia | ${book.title}`;
            }

            const statusClass = book.stock > 0 ? 'alert-success' : 'alert-danger';
            const statusText = book.stock > 0 ? `TERSEDIA (Stok: ${book.stock} eksemplar)` : `TIDAK TERSEDIA (Stok: 0)`;
            const buttonDisabled = book.stock > 0 ? '' : 'disabled';

            const htmlContent = `
                <div class="col-md-4 text-center mb-4">
                    <img src="${book.image}" class="img-fluid rounded-lg book-cover" alt="Cover ${book.title}" 
                        onerror="this.onerror=null; this.src='images/placeholder.jpeg'" loading="lazy">
                </div>

                <div class="col-md-8">
                    <h1 class="display-5 fw-bold text-primary">${book.title}</h1>
                    <p class="lead text-secondary">Penulis: ${book.author}</p>
                    <hr class="mb-4">
                    
                    <div class="row mb-4 small bg-light p-3 rounded-lg shadow-sm">
                        <div class="col-sm-6 mb-2"><strong>Penerbit:</strong> ${book.publisher}</div>
                        <div class="col-sm-6 mb-2"><strong>Tahun Terbit:</strong> ${book.year}</div>
                        <div class="col-sm-6 mb-2"><strong>ISBN:</strong> ${book.isbn}</div>
                        <div class="col-sm-6 mb-2"><strong>Halaman:</strong> ${book.pages} halaman</div>
                        <div class="col-sm-12"><strong>Kategori:</strong> <span class="badge bg-info text-dark">${book.category}</span></div>
                    </div>

                    <h3 class="mt-4">Sinopsis</h3>
                    <div class="synopsis-content">
                        ${book.synopsis.split('\n').map(p => p.trim() ? `<p class="text-dark">${p.trim()}</p>` : '').join('')}
                    </div>
                    
                    <div class="alert ${statusClass} mt-4 shadow-sm">
                        <i class="fas fa-cubes me-2"></i>
                        **Status Ketersediaan:** ${statusText}
                    </div>

                    <div class="mt-4">
                        <a href="login.php" class="btn btn-success btn-lg ${buttonDisabled} me-2 shadow">
                            <i class="fas fa-book-reader me-2"></i>Pinjam Sekarang
                        </a>
                        <a href="menu_utama.php" class="btn btn-outline-secondary btn-lg">
                            <i class="fas fa-list me-2"></i>Kembali ke Katalog
                        </a>
                    </div>
                </div>
            `;
            
            container.innerHTML = htmlContent;

        } catch (error) {
            container.innerHTML = `
                <div class="col-12 text-center my-5">
                    <div class="alert alert-danger shadow-sm" role="alert">
                        <h4 class="alert-heading">KESALAHAN KRITIS!</h4>
                        <p>Terjadi kesalahan saat memproses data: ${error.message}. Halaman tidak dapat dimuat.</p>
                        <hr>
                        <a href="menu_utama.php" class="btn btn-danger btn-lg">Kembali ke Katalog</a>
                    </div>
                </div>
            `;
        }
    }

    displayBookDetails();
});
```

---

## **8. Style**

/* File: css/style.css */

/* Mencegah scrollbar horizontal */
body {
    overflow-x: hidden; 
}

/* Memperbaiki masalah margin negatif pada ROW */
.row {
    margin-left: 0 !important;
    margin-right: 0 !important;
}
```
---

## **9. Koneksi**

/* File: koneksi/connection.php */

<?php
$database_hostname = "localhost";
$database_username = "root";
$database_password = "";
$database_name = "db_perpustakaan_bahagia";
$database_port = "3306";

try{
    $conn = new PDO ("mysql:host=$database_hostname;
    port=$database_port; dbname=$database_name",
    $database_username,
    $database_password
);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION); 
}catch (PDOException $e){
    die("Koneksi database gagal: " . $e->getMessage());
}
?>
```
