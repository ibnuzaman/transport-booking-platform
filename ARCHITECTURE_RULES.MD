# Architecture Rules - Ticketing System

Dokumen ini adalah panduan aturan struktur dan dependensi agar codebase tetap scalable.

## 1) Prinsip Utama

- Pisahkan kode berdasarkan `business module`, bukan berdasarkan jenis teknis global.
- Arah dependensi harus mengarah ke inti bisnis (`entity`/`usecase`), bukan sebaliknya.
- `internal/shared` hanya untuk komponen benar-benar lintas module dan bebas logika domain spesifik.
- `internal/platform` berisi integrasi teknis (DB, cache, queue, logger), bukan business rule.

## 2) Aturan Layer dan Arah Dependensi

### Layer per module

Contoh: `internal/module/booking`

- `entity`: model domain inti + invariants domain.
- `usecase`: business logic / orchestration.
- `repository`: kontrak persistence + implementasi penyimpanan data.
- `delivery/http`: adapter HTTP (handler, request/response DTO, binding).

### Arah import yang diperbolehkan

- `delivery/http` -> `usecase`, `shared`
- `usecase` -> `entity`, `repository` (interface), `shared`
- `repository` -> `entity`, `platform`
- `entity` -> (tidak bergantung ke layer lain)
- `bootstrap` -> semua layer untuk wiring dependency

### Import yang dilarang

- `entity` mengimpor `delivery`, `repository`, `platform`, atau `shared/response`.
- `usecase` mengimpor package HTTP (`net/http`, framework handler) secara langsung.
- Module A mengakses implementasi internal module B secara langsung (harus via interface/contract).

## 3) Aturan Folder

- Satu business capability = satu folder module di `internal/module/<nama-module>`.
- Setiap module minimal memiliki: `entity`, `usecase`, `repository`, `delivery/http`.
- File yang reusable lintas module:
  - error umum -> `internal/shared/errors`
  - format response API -> `internal/shared/response`
  - middleware lintas endpoint -> `internal/shared/middleware`
- Integrasi teknis global:
  - config -> `internal/platform/config`
  - DB -> `internal/platform/database`
  - cache -> `internal/platform/cache`
  - queue -> `internal/platform/queue`
  - logger -> `internal/platform/logger`

## 4) Naming Convention

- Nama folder module pakai lowercase singular: `booking`, `payment`, `ticket`.
- Interface gunakan nama berbasis peran: `BookingRepository`, `BookingUsecase`.
- Struct implementasi beri suffix konkret: `bookingRepository`, `bookingUsecase`, `bookingHandler`.
- File disarankan konsisten:
  - `entity/*.go`
  - `usecase/*.go`
  - `repository/*.go`
  - `delivery/http/*.go`

## 5) Checklist Saat Menambah Module Baru

1. Buat folder `internal/module/<module-baru>/`.
2. Tambahkan subfolder: `entity`, `usecase`, `repository`, `delivery/http`.
3. Definisikan entity domain terlebih dahulu.
4. Buat interface repository yang dibutuhkan usecase.
5. Implementasikan usecase tanpa ketergantungan HTTP framework.
6. Implementasikan delivery HTTP sebagai adapter ke usecase.
7. Wiring dependency di `internal/bootstrap`.
8. Pastikan tidak ada pelanggaran arah dependensi.

## 6) Boundary Rules (Wajib)

- Tidak boleh ada query SQL langsung di `usecase` atau `delivery`.
- Tidak boleh ada logika validasi bisnis utama di handler HTTP.
- DTO request/response API tidak digunakan sebagai entity domain.
- `shared` tidak boleh menyimpan logic spesifik `booking` (atau module tertentu).

## 7) Rule of Change

Saat ada perubahan requirement:

- Jika perubahan murni bisnis -> ubah di `usecase`/`entity`.
- Jika perubahan format API -> ubah di `delivery/http`.
- Jika perubahan storage/infra -> ubah di `repository`/`platform`.
- Hindari perubahan lintas banyak layer untuk satu concern kecil.
