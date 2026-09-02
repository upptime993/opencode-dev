---
name: verify
description: Double-check tasks using Automated (Unit/Integration) testing, Manual testing, dan Scope Fidelity check — memastikan implementasi tidak melenceng atau menambah hal yang tidak diminta. Use when: user says "verifikasi", "cek fitur", or "selesai coding".
---

# SKILL: VERIFY (Anti-Halu, Vibe Coding Ready)

---

## 📋 Metadata
- **Nama:** Verify
- **Versi:** 2.0
- **Output:** Laporan verifikasi
- **Dependensi:** `3-TASKS.md` (wajib), dokumen lain opsional

---

## 🎯 Trigger Keywords
- "Verifikasi" / "Cek fitur" / "Selesai coding"
- "Review hasil" / "Test task" / "Cek implementasi"

## 📝 Deskripsi
Memverifikasi task yang sudah diimplementasikan dengan mengecek kode, menjalankan test, memastikan sesuai acuan — **dan sekarang juga memastikan tidak ada scope creep**: implementasi tidak diam-diam melakukan lebih (atau berbeda) dari yang diminta di task/FR aslinya.

## 📂 Baca/Menyimpan File
- **Baca Task (WAJIB):** `@.agents/3-TASKS.md`
- **Baca Tech Spec:** `@.agents/2-TECH-SPEC.md` (opsional)
- **Baca PRD:** `@.agents/1-PRD.md` (opsional, penting untuk cek Scope Fidelity)
- **Simpan Laporan:** `.agents/REVIEW.md`

## ⚙️ Cara Kerja Skill

### FASE 1: Deteksi Trigger & Baca Dokumen
1. Cek `.agents/3-TASKS.md` — cari task status "Done".
2. Baca PRD/Tech Spec jika ada, sebagai acuan untuk cek Scope Fidelity.
3. Tidak ada task Done → minta user buat/kerjakan task dulu.

### FASE 2: Pilih Task
Sama seperti sebelumnya — tampilkan daftar task Done, user pilih, atau default ke task pertama.

### FASE 3: Proses Verifikasi (4 Aspek)

1. **Kesesuaian dengan Acuan**
   - Apakah implementasi sesuai deskripsi task?
   - Apakah business rules terpenuhi?

2. **Kualitas & Keamanan Kode**
   - Code smell, celah keamanan, input validation kurang?
   - Struktur & penamaan sesuai best practice framework?

3. **Fungsionalitas**
   - Kode bisa dijalankan? (sintaks, import, dependensi)
   - Ada test yang bisa dijalankan?

4. **Scope Fidelity** *(baru — inti anti-halu)*
   - Bandingkan kode aktual dengan deskripsi task **dan** FR asalnya (kalau ada, via label `← FR-XX`).
   - Apakah ada behavior/fitur/endpoint/field yang muncul di kode tapi **tidak** ada di task/FR? Jika ya → catat sebagai **scope creep**, bukan otomatis dianggap bagus meski kelihatannya berguna.
   - Apakah sebaliknya ada bagian dari task yang **tidak** diimplementasikan (task diklaim Done tapi sebenarnya parsial)?
   - Jika task berlabel `[EXTRA]` yang seharusnya butuh approval — cek apakah approval user memang tercatat sebelum implementasi dilakukan.

### FASE 4: Hasil Verifikasi

```
📋 Hasil Verifikasi T-03: Create User Model (← FR-01)

Status: ✅ Lolos / ⚠️ Catatan / ❌ Gagal

1. Kesesuaian Acuan: [ringkas]
2. Kualitas & Keamanan: [ringkas]
3. Fungsionalitas: [ringkas]
4. Scope Fidelity: [ringkas — sebutkan eksplisit kalau ada scope creep]

Temuan:
- [Issue] — [lokasi] — [severity]
- [SCOPE CREEP] [apa yang ditambahkan di luar task] — [rekomendasi: hapus / jadikan task terpisah dengan approval]

Saran:
- [Saran perbaikan]
```

### Update Status
- Lolos semua 4 aspek → status tetap "Done".
- Ada catatan (termasuk scope creep) → tulis ke `.agents/REVIEW.md`, user bisa putuskan: hapus bagian scope creep, atau approve retroaktif jadi task resmi baru. Status diubah sementara jadi "Perlu Review" sampai user putuskan.
