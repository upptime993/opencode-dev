---
name: mini-prd
description: Buat Product Requirement Document singkat dan padat dari brief yang sudah tervalidasi, dengan pemisahan tegas antara fakta terkonfirmasi user dan asumsi AI. Gunakan ketika: user minta buat PRD, user bilang "buat PRD dari brief ini", user minta ringkasan produk, atau user minta spesifikasi produk. WAJIB ada 0-BRIEF.md dulu (skill idea-intake) — jika belum ada, redirect ke idea-intake.
---

# SKILL: PRD GENERATOR (Anti-Halu, Vibe Coding Ready)

## 📋 Metadata
- **Nama:** PRD Generator
- **Versi:** 2.0
- **Output:** PRD 6 bagian + lampiran Fakta/Asumsi
- **Lokasi File:** `.agents/1-PRD.md`
- **Dependensi:** `0-BRIEF.md` (**WAJIB** — lihat skill `idea-intake`)

## 🎯 Trigger Keywords
- "Buat PRD dari brief ini"
- "Buat PRD untuk..."
- "Buat product requirement..."
- "Baca PRD saya" / "Load PRD"

## ⚠️ Guard Wajib (Cek Sebelum Mulai)
1. Cek `.agents/0-BRIEF.md`.
   - **Tidak ada** → STOP. Katakan ke user: *"Belum ada brief. Ceritakan dulu idenya, saya bantu susun brief-nya sebelum masuk PRD."* → jalankan skill `idea-intake`.
   - **Ada** → baca seluruhnya, termasuk section "Default Belum Dikonfirmasi" — ini akan dibawa turun sebagai asumsi berlabel ke PRD.
2. Jangan pernah mulai menulis PRD tanpa brief ini. Ini bukan langkah opsional.

## 📝 Deskripsi
Skill ini membaca `0-BRIEF.md` dan menghasilkan PRD ringkas tapi detail. Bedanya dari versi sebelumnya: **setiap pernyataan di PRD wajib bisa dilacak sumbernya** — apakah dari fakta yang user konfirmasi eksplisit di brief, atau asumsi AI yang dibuat untuk melengkapi dokumen. Dua hal ini TIDAK BOLEH tercampur tanpa label.

## 📂 Baca/Menyimpan File
- **Baca (WAJIB):** `@.agents/0-BRIEF.md`
- **Baca:** `@.agents/1-PRD.md`
- **Simpan:** `.agents/1-PRD.md`

## ⚙️ Cara Kerja Skill

### FASE 1: Deteksi Trigger & Baca Brief
- "baca"/"load" → baca `1-PRD.md`, beri ringkasan.
- "buat" → cek guard di atas, lalu lanjut FASE 2.

### FASE 2: Klarifikasi Tambahan (Hanya yang BELUM Terjawab di Brief)
Brief sudah menjawab unknowns level ide. Di sini AI hanya tanya hal spesifik level PRD yang brief belum cover — **closed-choice, maksimal 3 pertanyaan**, contoh:
1. **Indikator sukses utama?** (mis: a) jumlah user aktif, b) retensi mingguan, c) belum tahu — tentukan nanti)
2. **Versi ini (v1) sebatas apa?** (a) MVP super minim, b) lengkap tapi tanpa fitur sosial, c) sesuai brief apa adanya)

Jika brief sudah cukup lengkap, skip fase ini dan bilang ke user secara singkat kenapa di-skip.

### FASE 3: Produksi PRD (6 Bagian + Lampiran)
Hasilkan **per bagian**. User ketik `lanjut` untuk lanjut ke bagian berikutnya — ini bukan formalitas, AI harus benar-benar berhenti dan tunggu, supaya user sempat koreksi sebelum bagian berikutnya dibangun di atas kesalahan yang sama.

**Aturan penandaan wajib di SETIAP bagian:**
- Kalimat/poin yang berasal dari fakta eksplisit brief → tulis biasa, tanpa tanda (default-nya adalah fakta).
- Kalimat/poin yang berasal dari `[DEFAULT — belum dikonfirmasi]` di brief, atau asumsi baru yang AI buat untuk melengkapi PRD → **wajib** diberi tag `[A-XX]` inline, dan didaftar di Lampiran Asumsi dengan alasan singkat.

### FASE 4: Finalisasi
Setelah bagian 6 dan Lampiran selesai:
1. Tampilkan **ringkasan Lampiran Asumsi** ke user secara terpisah — ini yang paling penting untuk dicek, bukan seluruh dokumen.
2. Tanyakan: "Ada asumsi di atas yang salah/mau diubah?" — jika ya, revisi sebelum lanjut.
3. Instruksikan simpan ke `.agents/1-PRD.md`.
4. Rekomendasikan lanjut ke **Tech Spec Generator**.

## 📑 6 Bagian PRD + Lampiran
1. Visi & Tujuan
2. User Persona (2 persona)
3. User Stories (10-15, berlabel `US-XX`)
4. Functional Requirements (15-20 FR, berlabel `FR-XX`, wajib rujuk balik ke `US-XX`)
5. Non-Functional Requirements
6. Out of Scope & Dependensi
7. **Lampiran: Fakta vs Asumsi** *(baru)*

## 📄 BAGIAN 1: Visi & Tujuan Produk

### Visi Produk
1 paragraf visi besar produk ini — ditulis berdasarkan Ide Asli (verbatim) di brief, bukan dikarang ulang.

### Tujuan Utama (3-5)
1. [Tujuan 1] - [Indikator keberhasilan]
2. [Tujuan 2] - [Indikator keberhasilan]

### Value Proposition
- [Nilai unik 1]
- [Nilai unik 2]

## 📄 BAGIAN 2: User Persona

### Persona 1: [Nama]
- **Usia/Pekerjaan:** [Usia], [Pekerjaan]
- **Level Teknis:** [Pemula/Menengah/Mahir]
- **Tujuan:** [Apa yang ingin dicapai?]
- **Pain Points:** [Masalah yang dihadapi]

> Jika brief tidak menyebut siapa penggunanya secara spesifik, persona ini WAJIB ditandai `[A-XX]` — jangan mengarang detail demografis seolah user sudah bilang.

## 📄 BAGIAN 3: User Stories

### Format
`US-XX: Sebagai [peran], saya ingin [tindakan], agar [manfaat].`

### Modul 1: [Sesuaikan dari Brief]
- **US-01:** Sebagai [peran], saya ingin [tindakan], agar [manfaat]. *(sumber: Ide Asli / U-02 brief)*
- **US-02:** ... `[A-01]` *(asumsi — brief tidak sebutkan eksplisit)*

*(Total 10-15 user stories, semua berlabel US-XX)*

## 📄 BAGIAN 4: Functional Requirements

### Format
**FR-XX: [Nama Fitur]** *(← dari US-YY, US-ZZ)*
- **Input:** [Data masuk]
- **Proses:** [Apa yang dilakukan sistem]
- **Output:** [Hasil yang diberikan]
- **Aturan Bisnis:** [Rules]

> **Aturan wajib:** setiap FR harus mencantumkan `← dari US-XX`. Jika ada FR yang tidak berasal dari User Story manapun (AI menganggapnya perlu demi kelengkapan teknis, misal "lupa password"), FR itu wajib ditandai `[A-XX — EXTRA, tidak diminta langsung, disarankan AI]` dan dijelaskan alasannya di Lampiran.

*(Total 15-20 FR)*

## 📄 BAGIAN 5: Non-Functional Requirements

### Performa
- [item, tandai `[A-XX]` jika angka ini AI yang tentukan sendiri, bukan dari brief]

### Keamanan
- [item]

### Skalabilitas
- [item]

### Usability
- [item]

## 📄 BAGIAN 6: Out of Scope & Dependensi

### Out of Scope (Tidak Dikerjakan di V1)
- [Fitur] - ditunda ke v2

### Dependensi
- [Library/API] - untuk [fungsi]

### Asumsi
- [Asumsi teknis lain, semua tetap berlabel jika bukan fakta eksplisit]

## 📄 LAMPIRAN: Fakta vs Asumsi *(bagian wajib baru)*

```markdown
## ✅ Fakta Terkonfirmasi (dari 0-BRIEF.md)
- U-01: [ringkas]
- U-02: [ringkas]

## ⚠️ Daftar Asumsi AI
| ID | Lokasi | Asumsi | Alasan | Status |
|----|--------|--------|--------|--------|
| A-01 | US-02 | [isi asumsi] | [alasan AI pilih ini] | Perlu dikonfirmasi |
| A-02 | FR-05 (EXTRA) | [isi asumsi] | [alasan] | Perlu dikonfirmasi |
```

Lampiran ini adalah bagian **paling penting untuk direview user** — bukan basa-basi penutup. Semua yang berstatus "Perlu dikonfirmasi" idealnya diputuskan user sebelum lanjut ke Tech Spec, karena begitu masuk Tech Spec dan Task, asumsi yang salah akan makin mahal untuk dikoreksi.

## 🔄 Finalisasi

1. **Simpan file:** `.agents/1-PRD.md`
2. **Lanjut ke Tech Spec:**
   Ketik: `"Buat Tech Spec berdasarkan PRD yang sudah dibuat"`
