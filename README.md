# FundFlow

**FundFlow** adalah platform berbasis web pencatatan keuangan dan pengawasan kinerja terpadu untuk organisasi kemahasiswaan (BEM, DPM, Himpunan, dan UKM). Platform ini memfasilitasi pengurus organisasi untuk mengunggah bukti nota transaksi secara terbuka dan *real-time*, sekaligus memberikan akses publik bagi mahasiswa umum untuk memantau saldo, melihat transparansi alokasi dana, serta menilai akuntabilitas tata kelola ormawa.

---

## 1. Anggota Kelompok

| No | Nama    | NPM    |
|----|--------|--------|
| 1  | Nafarrel | 250013 |
| 2  | Salomo   | 250034 |
| 3  | Rasyid   | 250076 |

---

## 2. Fungsi

Website ini menjadi wadah publik untuk **memantau efektivitas, transparansi, dan akuntabilitas keuangan organisasi kemahasiswaan**. Pengurus organisasi (bendahara) wajib mencatat transaksi pemasukan dan pengeluaran secara terbuka yang dilengkapi dengan unggahan bukti fisik nota/kuitansi. Setiap laporan pengeluaran diproses dan diverifikasi oleh tim pengawas/auditor (DPM) sebelum diterbitkan di direktori publik.

Mahasiswa umum dapat memantau saldo aktif secara *real-time*, melihat visualisasi grafik alokasi anggaran, serta memberikan *review* dan penilaian (*rating*) terhadap akuntabilitas transparansi masing-masing ormawa.

Fitur utama:
- Autentikasi & Authorization multi-role (`admin/auditor`, `treasurer`, `student`).
- Form pencatatan transaksi pemasukan dan pengeluaran kas dilengkapi fitur *upload* foto nota/kuitansi fisik.
- Dashboard pengawasan & verifikasi (*Approve/Reject*) laporan keuangan oleh DPM/Admin.
- Direktori laporan keuangan publik beserta visualisasi grafik (*chart*) alokasi dana per kategori.
- Fitur Ulasan & Rating Akuntabilitas bagi mahasiswa untuk menilai tingkat keterbukaan keuangan ormawa.

---

## 3. Tujuan (SDG)

**SDG 16.6 — Mengembangkan Lembaga yang Efektif, Akuntabel, dan Transparan di Semua Tingkat**

Proyek ini mendukung pencapaian target SDG 16.6 dengan cara:
- **Transparansi Informasi:** Membuka akses penuh bagi seluruh mahasiswa untuk melihat arus kas dan bukti fisik transaksi organisasi secara *real-time*.
- **Akuntabilitas Kelembagaan:** Mendorong pengurus ormawa untuk mengelola anggaran secara tertib dan dapat dipertanggungjawabkan melalui mekanisme verifikasi laporan.
- **Partisipasi Pengawasan Publik:** Memberikan wadah evaluasi berbasis ulasan (*review*) dari mahasiswa guna mengukur tingkat efektivitas dan keterbukaan lembaga kemahasiswaan.

---

## 4. Target Pengguna

| Pengguna | Kebutuhan |
|---|---|
| **Bendahara Organisasi (`treasurer`)** | Menginput data transaksi kas, mengunggah foto kuitansi/nota, dan mengelola alokasi anggaran internal ormawa. |
| **DPM / Admin Pengawas (`admin`)** | Meninjau & memverifikasi validitas kuitansi laporan keuangan, mengelola data ormawa, serta memantau skor akuntabilitas. |
| **Mahasiswa Umum (`student`)** | Memantau saldo aktif, melihat rincian nota transaksi, dan memberikan rating & ulasan transparansi ormawa. |

---

## 5. Skema Database (ERD)

Minimal 4 tabel: `users`, `organizations`, `financial_reports`, `accountability_reviews`.

```mermaid
erDiagram
    USERS ||--o{ ORGANIZATIONS : mengelola
    USERS ||--o{ FINANCIAL_REPORTS : mengunggah
    ORGANIZATIONS ||--o{ FINANCIAL_REPORTS : memiliki
    ORGANIZATIONS ||--o{ ACCOUNTABILITY_REVIEWS : menerima

    USERS {
        bigint id PK
        string name
        string email
        string password
        enum role "admin, treasurer, student"
        timestamp created_at
        timestamp updated_at
    }

    ORGANIZATIONS {
        bigint id PK
        bigint user_id FK "Bendahara Utama"
        string org_name "Contoh: HMIF, BEM, DPM"
        string code "Contoh: HMIF"
        decimal current_balance
        timestamp created_at
        timestamp updated_at
    }

    FINANCIAL_REPORTS {
        bigint id PK
        bigint organization_id FK
        bigint user_id FK
        string title "Contoh: Cetak Spanduk Lomba"
        enum type "income, expense"
        string category "Perlengkapan, Konsumsi, Kas"
        decimal amount
        string receipt_image "Path foto nota"
        enum status "draft, verified, rejected"
        timestamp created_at
        timestamp updated_at
    }

    ACCOUNTABILITY_REVIEWS {
        bigint id PK
        bigint organization_id FK
        bigint user_id FK
        integer rating "Skor 1-5 Bintang"
        text review_comment "Ulasan transparansi"
        enum status "pending,
