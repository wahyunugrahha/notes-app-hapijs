# Notes App Backend (Hapi.js)

Repositori ini menyimpan backend untuk aplikasi **Notes**, yang dibangun dengan **Hapi.js** dan **PostgreSQL**. Repositori ini menyediakan **RESTful API** untuk mengelola akun pengguna, autentikasi pengguna, dan menangani catatan pribadi dengan otorisasi yang tepat.

---

## 📑 Daftar Isi

- [Fitur Utama](#-fitur-utama)
- [Teknologi yang Digunakan](#-teknologi-yang-digunakan)
- [Memulai](#-memulai)
  - [Prasyarat](#-prasyarat)
  - [Instalasi](#-instalasi)
  - [Pengaturan Database](#-pengaturan-database)
  - [Variabel Lingkungan](#-variabel-lingkungan)
  - [Menjalankan Aplikasi](#-menjalankan-aplikasi)
- [Endpoint API](#-endpoint-api)
  - [API Pengguna](#api-pengguna)
  - [API Autentikasi](#api-autentikasi)
  - [API Catatan](#api-catan)
  - [API Kolaborasi](#api-kolaborasi)
  - [API Unggahan](#api-unggahan)
  - [API Ekspor](#api-ekspor)
- [Pengujian API](#-pengujian-api)

---

## 🚀 Fitur Utama

- **Manajemen Pengguna**: Registrasi, detail pengguna, verifikasi kredensial dengan username unik.
- **Autentikasi & Otorisasi**: Login/logout menggunakan JWT & refresh token. Rute terlindungi memverifikasi kepemilikan catatan.
- **Manajemen Catatan**: CRUD catatan, hanya dapat diakses oleh pemilik atau kolaborator.
- **Kolaborasi**: Menambahkan kolaborator pada catatan tertentu.
- **Unggahan File**: Upload gambar ke **AWS S3**.
- **Ekspor Data**: Ekspor catatan via email menggunakan **RabbitMQ**.
- **Caching**: Optimasi query catatan dengan **Redis**.
- **Penanganan Error**: Menggunakan error kustom (ClientError, InvariantError, dll).
- **Validasi Data**: Menggunakan **Joi**.
- **Migrasi Database**: Dengan **node-pg-migrate**.
- **Kualitas Kode**: ESLint dengan standar **airbnb-base**.

---

## 🛠 Teknologi yang Digunakan

- Node.js
- Hapi.js
- PostgreSQL
- Redis
- RabbitMQ
- AWS SDK (S3)
- Joi
- nanoid
- node-pg-migrate
- bcrypt
- @hapi/jwt
- @hapi/inert
- dotenv
- nodemon
- ESLint

---

## ⚡ Memulai

### 📌 Prasyarat

- Node.js (>= 18)
- PostgreSQL
- Redis
- RabbitMQ
- AWS Account (S3 bucket & credentials)

### 📥 Instalasi

```bash
git clone https://github.com/wahyunugrahha/notes-app-hapijs.git
cd notes-app-hapijs
npm install
```

### 🗄 Pengaturan Database

Buat database:

```sql
CREATE DATABASE notesapp;
```

Jalankan migrasi:

```bash
npm run migrate up
```

### ⚙️ Variabel Lingkungan

Buat file `.env` di root project:

```env
PORT=5000
HOST=localhost
PGUSER=your_pg_username
PGHOST=your_pg_host
PGDATABASE=notesapp
PGPASSWORD=your_pg_password
PGPORT=5432
ACCESS_TOKEN_KEY=your_access_token_secret
REFRESH_TOKEN_KEY=your_refresh_token_secret
ACCESS_TOKEN_AGE=1800
AWS_REGION=your_aws_region
AWS_ACCESS_KEY_ID=your_aws_access_key_id
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key
AWS_BUCKET_NAME=your_aws_bucket_name
REDIS_SERVER=your_redis_server
RABBITMQ_SERVER=your_rabbitmq_server
```

### ▶️ Menjalankan Aplikasi

Development:

```bash
npm run start:dev
```

Production:

```bash
npm run start:prod
```

Server akan berjalan di: [http://localhost:5000](http://localhost:5000)

---

## 📡 Endpoint API

### API Pengguna

| Metode | Jalur       | Deskripsi                          | Autentikasi |
| ------ | ----------- | ---------------------------------- | ----------- |
| POST   | /users      | Registrasi pengguna baru           | ❌ Tidak    |
| GET    | /users/{id} | Detail pengguna berdasarkan ID     | ❌ Tidak    |
| GET    | /users      | Cari pengguna berdasarkan username | ❌ Tidak    |

### API Autentikasi

| Metode | Jalur            | Deskripsi                             | Autentikasi |
| ------ | ---------------- | ------------------------------------- | ----------- |
| POST   | /authentications | Login & dapatkan Access/Refresh Token | ❌ Tidak    |
| PUT    | /authentications | Refresh Access Token                  | ❌ Tidak    |
| DELETE | /authentications | Logout (hapus Refresh Token)          | ❌ Tidak    |

### API Catatan

| Metode | Jalur       | Deskripsi                                    | Autentikasi |
| ------ | ----------- | -------------------------------------------- | ----------- |
| POST   | /notes      | Membuat catatan baru                         | ✅ Ya       |
| GET    | /notes      | Ambil semua catatan pengguna                 | ✅ Ya       |
| GET    | /notes/{id} | Ambil catatan spesifik (pemilik/kolaborator) | ✅ Ya       |
| PUT    | /notes/{id} | Update catatan (pemilik/kolaborator)         | ✅ Ya       |
| DELETE | /notes/{id} | Hapus catatan (hanya pemilik)                | ✅ Ya       |

### API Kolaborasi

| Metode | Jalur           | Deskripsi                          | Autentikasi |
| ------ | --------------- | ---------------------------------- | ----------- |
| POST   | /collaborations | Tambah kolaborator (hanya pemilik) | ✅ Ya       |
| DELETE | /collaborations | Hapus kolaborator (hanya pemilik)  | ✅ Ya       |

### API Unggahan

| Metode | Jalur             | Deskripsi                        | Autentikasi |
| ------ | ----------------- | -------------------------------- | ----------- |
| POST   | /upload/images    | Upload gambar ke server/S3       | ❌ Tidak    |
| GET    | /upload/{param\*} | Akses gambar yang sudah diupload | ❌ Tidak    |

### API Ekspor

| Metode | Jalur         | Deskripsi                            | Autentikasi |
| ------ | ------------- | ------------------------------------ | ----------- |
| POST   | /export/notes | Ekspor catatan ke email via RabbitMQ | ✅ Ya       |

---

## 🧪 Pengujian API

Repositori ini menyertakan **Postman Collection** dan **Environment** untuk pengujian:

- `Notes API Test.postman_collection.json`
- `Notes API Test.postman_environment.json`

> Import file di Postman, jalankan server, lalu tes endpoint sesuai variabel environment.

---

