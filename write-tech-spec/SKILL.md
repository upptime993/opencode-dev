---
name: write-tech-spec
description: Write technical specifications for features, changes, or bug fixes, dengan setiap keputusan teknis wajib dilacak balik ke FR di PRD atau ditandai sebagai asumsi AI. Produces a markdown file in .agents/2-TECH-SPEC.md. Gunakan ketika: user minta tech spec, spesifikasi teknis, implementation plan, atau mendeskripsikan fitur yang butuh perencanaan sebelum coding.
---

# SKILL: TECH SPEC GENERATOR (Anti-Halu, Vibe Coding Ready)

## 📋 Metadata
- **Nama:** Tech Spec Generator
- **Versi:** 2.0
- **Output:** Tech Spec 5 bagian + Lampiran Asumsi
- **Lokasi File:** `.agents/2-TECH-SPEC.md`
- **Dependensi:** `1-PRD.md` (**WAJIB** dibaca, termasuk Lampiran Fakta/Asumsi-nya)

## 🎯 Trigger Keywords
- "Buat Tech Spec"
- "Buat spesifikasi teknis"
- "Buat spec dari PRD"
- "Baca TECH-SPEC saya" / "Load Tech Spec"

## ⚠️ Guard Wajib
1. Cek `.agents/1-PRD.md`. Tidak ada → minta user buat PRD dulu (skill `mini-prd`).
2. **Baca Lampiran Asumsi PRD.** Jika ada asumsi berstatus "Perlu dikonfirmasi" yang berdampak langsung ke keputusan arsitektur (mis. jumlah user, kebutuhan offline, kompleksitas data) → **tanyakan dulu ke user sebelum mulai Tech Spec**, jangan diam-diam anggap sudah settled.

## 📝 Deskripsi
Membaca PRD dan menghasilkan Tech Spec ringkas tapi detail. Setiap keputusan teknis (stack, struktur DB, endpoint) yang tidak punya dasar eksplisit dari PRD wajib ditandai sebagai asumsi teknis, bukan ditulis seolah "satu-satunya pilihan yang benar".

## 📂 Baca/Menyimpan File
- **Baca PRD (WAJIB):** `@.agents/1-PRD.md`
- **Baca Tech Spec:** `@.agents/2-TECH-SPEC.md`
- **Simpan Tech Spec:** `.agents/2-TECH-SPEC.md`

## ⚙️ Cara Kerja Skill

### FASE 1: Deteksi Trigger, Baca PRD & Lampiran Asumsi
Lihat Guard Wajib di atas.

### FASE 2: Klarifikasi (3 Pertanyaan, Closed-Choice)
1. **Tech Stack pilihan?** (beri 3-4 opsi konkret sesuai jenis proyek dari PRD, bukan daftar generik semua framework yang ada)
2. **Database?** (opsi sesuai kebutuhan data dari PRD — relational jika data terstruktur/relasional, document jika fleksibel)
3. **Hosting?**

Jika user jawab "terserah kamu" → AI boleh pilih, **tapi wajib tandai pilihan itu `[A-XX]`** di Lampiran dengan alasan teknis singkat (bukan alasan template).

### FASE 3: Produksi Tech Spec (5 Bagian)
Per bagian, user ketik `lanjut`.

**Aturan traceability wajib:**
- Setiap keputusan desain (tabel DB, endpoint, komponen) yang punya asal jelas dari FR tertentu di PRD → cantumkan `(← FR-XX)`.
- Setiap keputusan yang TIDAK berasal dari FR manapun (AI menambahkan demi best practice, misal rate limiting, logging, caching layer) → tandai `[A-XX — infra/best-practice, bukan dari FR]`.
- **Dilarang** menambahkan fitur/entity/endpoint baru yang tidak ada di PRD tanpa tag ini. Kalau AI merasa itu perlu, tulis sebagai rekomendasi terpisah di akhir, bukan disisipkan diam-diam ke tabel utama.

### FASE 4: Finalisasi
1. Tampilkan Lampiran Asumsi Teknis secara terpisah ke user untuk direview.
2. Instruksikan simpan ke `.agents/2-TECH-SPEC.md`.
3. Rekomendasikan lanjut ke **Task Generator**.

## 📑 5 Bagian Tech Spec + Lampiran
1. Tech Stack & Arsitektur
2. Database Design
3. Interface Design
4. Alur Logika & Business Rules
5. Keamanan, Performa, & Deployment
6. **Lampiran: Asumsi Teknis** *(baru)*

## 📄 BAGIAN 1: Tech Stack & Arsitektur

### Tech Stack
| Layer | Technology | Version | Sumber |
|-------|------------|---------|--------|
| Frontend | [Framework] | [Versi] | [pilihan user / `[A-01]`] |
| Backend | [Framework] | [Versi] | |
| Database | [DB] | [Versi] | |

### Arsitektur Sistem
```
Frontend → Backend/API → Database
```

### Struktur Folder
AI generate sesuai best practice resmi framework yang dipilih (lihat catatan referensi di bawah, tidak berubah dari versi sebelumnya — ini murni teknis, bukan sumber halu karena mengikuti konvensi resmi framework).

### Justifikasi
- **[Framework]:** [alasan — jika bukan dari constraint PRD, tandai `[A-XX]`]

## 📄 BAGIAN 2: Database Design

### Entity Overview
| Entity | Key Fields | Relasi | Sumber |
|--------|-----------|--------|--------|
| [Entity 1] | id, ... | → Entity 2 (1:N) | ← FR-01 |
| [Entity X] | id, ... | | `[A-02]` — entity tambahan untuk [alasan teknis] |

> Setiap entity WAJIB bisa dijawab: "ini menyimpan data untuk FR yang mana?" Kalau tidak bisa dijawab dari PRD, itu asumsi — tandai, jangan sembunyikan.

## 📄 BAGIAN 3: Interface Design

Bentuk interface menyesuaikan stack (REST/GraphQL/Server Actions/dll — logika pemilihan sama seperti versi sebelumnya).

| Method | Path/Action | Deskripsi | Sumber |
|--------|-------------|-----------|--------|
| [GET/POST] | [path] | [deskripsi] | ← FR-XX |

## 📄 BAGIAN 4: Alur Logika & Business Rules

**Alur [Fitur dari FR-XX]:**
1. [Langkah sesuai FR]
2. [Langkah sesuai FR]

### Business Rules
- [Rule] ← FR-XX
- [Rule] `[A-03]` — tidak eksplisit di FR, ditambahkan demi konsistensi data

## 📄 BAGIAN 5: Keamanan, Performa, & Deployment

Konten menyesuaikan stack + hosting (sama seperti versi sebelumnya). Item yang bukan requirement eksplisit dari PRD NFR → tandai `[A-XX]`.

### Development Setup
Perintah setup sesuai framework yang dipilih.

## 📄 LAMPIRAN: Asumsi Teknis

```markdown
| ID | Lokasi | Asumsi | Alasan Teknis | Status |
|----|--------|--------|----------------|--------|
| A-01 | Tech Stack | [isi] | [alasan] | Perlu dikonfirmasi |
| A-02 | DB: Entity X | [isi] | [alasan] | Perlu dikonfirmasi |
```

## 🔄 Finalisasi

1. **Simpan file:** `.agents/2-TECH-SPEC.md`
2. **Lanjut ke Task Generator:**
   Ketik: `"Buat Task berdasarkan Tech Spec yang sudah dibuat"`
