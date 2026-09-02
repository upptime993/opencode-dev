---
name: legacy-decoder
description: Reverse-engineer legacy code into structured documentation (Tech Spec + Tasks), dengan pemisahan tegas antara apa yang benar-benar ada di kode (fakta) dan interpretasi AI soal maksud/business logic-nya (inferensi). Use when: user says "pahami kode", "analisis legacy", "reverse engineer", "mapping kode", "dokumentasi kode existing".
---

# SKILL: LEGACY DECODER (Anti-Halu)

## 📋 Metadata
- **Nama:** Legacy Decoder
- **Versi:** 2.0
- **Output:** Analisis arsitektur + Tech Spec (opsional) + Task List
- **Lokasi File:** `.agents/4-LEGACY-DECODER.md`
- **Dependensi:** Tidak ada (bekerja dari kode existing)

## 🎯 Trigger Keywords
- "Pahami kode" / "Analisis legacy" / "Reverse engineer"
- "Mapping kode" / "Dokumentasi kode existing" / "Review codebase"
- "Baca hasil analisis" / "Load legacy analysis"

## 📝 Catatan Anti-Halu Khusus Skill Ini
Untuk kode existing, sumber halu yang paling sering terjadi bukan "AI mengarang fitur", tapi **AI menebak *maksud* (intent) dari kode** dan menuliskannya seolah itu fakta. Contoh: melihat function `calculateDiscount()` lalu menyimpulkan "ini untuk program loyalty member" padahal itu cuma tebakan dari nama function, bukan sesuatu yang benar-benar terverifikasi di kode.

**Aturan wajib:** semua temuan dikategorikan ke salah satu dari dua kelas berikut, dan tidak boleh dicampur tanpa label:
- **[FAKTA]** — benar-benar terbaca di kode: nama function, struktur tabel, endpoint yang terdaftar di router, tipe data, dependency yang ter-import.
- **[INFERENSI]** — kesimpulan/tebakan AI soal *tujuan* atau *business rule* di balik kode tersebut, disimpulkan dari nama variabel/komentar/pola kode, tapi tidak 100% pasti.

## 📂 Baca/Menyimpan File
- **Baca:** `@.agents/4-LEGACY-DECODER.md`
- **Simpan:** `.agents/4-LEGACY-DECODER.md`
- **Output Tech Spec:** `.agents/2-TECH-SPEC.md` (opsional)
- **Output Tasks:** `.agents/3-TASKS.md`

## ⚙️ Cara Kerja Skill

### FASE 0: Deteksi Trigger
- "baca"/"load" → baca `4-LEGACY-DECODER.md`, beri ringkasan.
- "pahami"/"analisis"/"reverse"/"mapping"/"review" → lanjut FASE 1.

### FASE 1: Discovery
1. Scan struktur folder project. **[FAKTA]**
2. Baca file konfigurasi (`package.json`, `composer.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `Gemfile`, `pubspec.yaml`, `CMakeLists.txt`, `pom.xml`, dll).
3. Identifikasi framework, runtime, database, dependency utama — ini **[FAKTA]** langsung dari file config, bukan tebakan.
4. Simpan ke `.agents/4-LEGACY-DECODER.md` (append), berlabel `[FAKTA]`.

### FASE 2: Codebase Mapping
1. Petakan struktur direktori (controller/model/service/route/view) — **[FAKTA]**.
2. Identifikasi entry point dan alur request — **[FAKTA]** jika bisa ditelusuri langsung dari kode; **[INFERENSI]** jika harus menyimpulkan dari pola/konvensi tanpa jejak eksplisit.
3. Simpan dependency graph / arsitektur ke `.agents/4-LEGACY-DECODER.md` (append).

### FASE 3: Business Logic Extraction
1. Ekstrak daftar routes/endpoints/actions — **[FAKTA]**.
2. Identifikasi entities/models dan relasinya — **[FAKTA]** jika ada di schema/model definition.
3. Catat business rules yang **terlihat** dari kode (misal ada `if` yang eksplisit menyebut suatu kondisi) → **[FAKTA]**.
4. Catat business rules yang **disimpulkan** dari nama function/pola kode tanpa comment penjelas → **[INFERENSI]**, wajib ditulis "kemungkinan maksudnya adalah..." bukan dinyatakan pasti.
5. Simpan ke `.agents/4-LEGACY-DECODER.md` (append), keduanya berlabel jelas.

### FASE 4: Database Reverse (jika ada DB)
1. Baca migration files / schema definition — **[FAKTA]**.
2. Petakan entity-relationship — **[FAKTA]** dari foreign key/schema; **[INFERENSI]** untuk relasi implisit yang tidak dideklarasikan di level DB.
3. Catat index, constraint, trigger — **[FAKTA]**.
4. Simpan database overview ke `.agents/4-LEGACY-DECODER.md` (append).

### FASE 5: Output Generation
**Opsional — Tech Spec (`2-TECH-SPEC.md`):**
1. Tanyakan: "Buat Tech Spec dari hasil analisis? (y/n)"
2. Jika ya → generate 5 bagian, dengan bagian yang berasal dari **[INFERENSI]** tetap ditandai `[A-XX]` mengikuti format skill `write-tech-spec`.
3. Simpan ke `.agents/2-TECH-SPEC.md`.

**Wajib — Tasks (`3-TASKS.md`):**
1. Generate task list (refactor, bug fix, optimasi, dokumentasi), mengikuti format skill `create-issues` — task yang berasal dari **[INFERENSI]** soal business logic (bukan dari fakta kode langsung) ditandai `[EXTRA — berdasarkan inferensi, perlu konfirmasi]`.
2. Simpan ke `.agents/3-TASKS.md`.

### FASE 6: Finalisasi
1. Tampilkan ringkasan hasil analisis, **dengan ringkasan terpisah untuk semua [INFERENSI]** yang perlu dikonfirmasi user — supaya user tahu bagian mana dari "pemahaman AI soal kode ini" yang sebenarnya masih tebakan.
2. Rekomendasikan lanjut ke **implement-task** atau **verify**.
