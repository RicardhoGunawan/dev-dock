# Lingkungan Server Lokal

Repositori ini berisi konfigurasi Docker Compose untuk lingkungan pengembangan web lokal yang menggunakan PHP 8.1 (dengan Apache), MySQL 8.0, dan phpMyAdmin.

## Prasyarat

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/) (sudah termasuk dalam Docker Desktop)

## Ringkasan Layanan

Berikut adalah layanan-layanan yang dikonfigurasi dalam lingkungan ini:

| Layanan | Versi Image | Port | URL Lokal | Catatan |
|---------|---------------|------|-----------|---------|
| **Web Server** | PHP 8.1 & Apache | `80` | [http://localhost](http://localhost) | Document root terhubung ke direktori utama (`../`). |
| **Database** | MySQL 8.0 | `3306` | `localhost:3306` | Password root adalah `root`. |
| **phpMyAdmin** | Latest | `8080` | [http://localhost:8080](http://localhost:8080) | Terhubung ke database MySQL secara otomatis. |

## Memulai

1. **Masuk ke dalam direktori** di mana file ini berada:
   ```bash
   cd local-server
   ```

2. **Jalankan container** di latar belakang (background):
   ```bash
   docker-compose up -d
   ```

3. **Akses layanan**:
   - Aplikasi web: [http://localhost](http://localhost)
   - Manajer database (phpMyAdmin): [http://localhost:8080](http://localhost:8080)
     - **Username:** `root`
     - **Password:** `root`

## Struktur Proyek & Volume

- **Web Server Document Root**: Container `web_server` menghubungkan direktori induk atau folder di atasnya (`../`) ke dalam `/var/www/html`. Ini berarti kode aplikasi web Anda (file PHP/HTML) harus berada di satu folder *di atas* direktori `local-server` ini agar dapat diakses melalui localhost.
- **Data MySQL**: File database disimpan dalam Docker volume persisten bernama `mysql_data`. Data database Anda akan tetap aman dan tidak terhapus, meskipun container dihentikan atau dihapus.

## Konfigurasi PHP Bawaan

Container web server Apache telah diatur untuk mendukung framework aplikasi secara langsung:
- Mengaktifkan modul Apache `mod_rewrite` secara otomatis.
- Menginstal ekstensi PHP `pdo` dan `pdo_mysql` otomatis untuk kelancaran koneksi database.

## Perintah-Perintah yang Umum Digunakan

**Melihat status container**
```bash
docker-compose ps
```

**Melihat log**
Untuk melihat apa yang terjadi (log) di dalam container:
```bash
docker-compose logs -f
```

**Menghentikan lingkungan**
Perintah ini akan menghentikan container tanpa menghancurkan data di dalam volume (database tetap aman):
```bash
docker-compose stop
```

**Menghapus dan membongkar lingkungan**
Ini menghentikan container dan membongkar network, tetapi **mempertahankan** volume yang telah dibuat (data database Anda akan tetap aman):
```bash
docker-compose down
```

**Menghancurkan semuanya (termasuk data database)**
⚠️ *Peringatan: Perintah ini akan menghapus seluruh data database Anda secara permanen!*
```bash
docker-compose down -v
```

## Berkontribusi

Kami menerima kontribusi! Untuk berkontribusi:
1. Fork repositori ini.
2. Buat branch fitur baru (`git checkout -b fitur/fitur-saya`).
3. Commit perubahan Anda (`git commit -am 'Menambahkan sebuah fitur'`).
4. Push ke branch (`git push origin fitur/fitur-saya`).
5. Buka Pull Request.
