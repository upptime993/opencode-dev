---
name: create-issues
description: Create local task lists from a Tech Spec file or direct input (bugs, small tasks), dengan setiap task wajib dilacak balik ke FR/asumsi asalnya — mencegah AI menyisipkan task yang tidak diminta tanpa terdeteksi. Use when: user says "buat task", "ada bug", "tambah task", "eksekusi rencana task", "pecah task", "breakdown pekerjaan", or "detailkan task".
---

# SKILL: TASK GENERATOR (Anti-Halu, Vibe Coding Ready)

## 📋 Metadata
- **Nama:** Task Generator
- **Versi:** 2.0
- **Output:** Task list terperinci + label sumber
- **Lokasi File:** `.agents/3-TASKS.md`
- **Dependensi:** `2-TECH-SPEC.md` (opsional — bisa langsung input user)

## 🎯 Trigger Keywords
- "Buat Task" / "Buat tugas" / "Buat task list"
- "Buat implementation task"
- "Baca TASKS saya" / "Load Task"

## 📝 Deskripsi
Membaca Tech Spec (atau input langsung user) dan menghasilkan task terperinci. Bedanya dari versi sebelumnya: **setiap task wajib mencantumkan asal-usulnya** — dari FR mana, atau apakah ini task "EXTRA" yang AI usulkan sendiri. Ini titik kritis anti-halu, karena di sinilah dokumen berubah jadi kode — kalau ada task yang tidak diminta lolos tanpa tanda di sini, hasilnya jadi fitur yang user tidak pernah minta.

## 📂 Baca/Menyimpan File
- **Baca Tech Spec:** `@.agents/2-TECH-SPEC.md` (jika ada)
- **Baca PRD:** `@.agents/1-PRD.md` (untuk cross-check FR asli, jika Tech Spec ada)
- **Baca Task:** `@.agents/3-TASKS.md`
- **Simpan Task:** `.agents/3-TASKS.md`

## ⚙️ Cara Kerja Skill

### FASE 1: Deteksi Trigger & Analisa Input
**Langkah 1 — Cek input user:** deskripsi langsung (bug/fitur kecil) atau hanya trigger tanpa deskripsi?

**Langkah 2 — Cek sumber acuan:**
- Ada `2-TECH-SPEC.md` → baca dan jadikan acuan utama.
- Input langsung user → proses tanpa Tech Spec, pakai input user sebagai acuan (tetap tag asalnya — lihat FASE 3).
- Tidak ada keduanya → minta user buat Tech Spec dulu, atau berikan deskripsi langsung.

**Langkah 3 — Auto-detect boilerplate** (sama seperti sebelumnya, cek `package.json`/`composer.json`/dll).

### FASE 2: Konfirmasi Scope
Tanyakan 1 hal: **Prioritas utama?** (Fitur inti dulu / semua sekaligus?)

### FASE 3: Produksi Task — dengan Wajib Label Sumber

Setiap task WAJIB mencantumkan field **Sumber**, dengan 3 kemungkinan nilai:
- `← FR-XX` — task ini murni turunan langsung dari Functional Requirement di Tech Spec/PRD.
- `← Input langsung user` — untuk mode bug/task tanpa Tech Spec.
- `[EXTRA — diusulkan AI, bukan dari acuan]` — task yang AI anggap perlu (misal: setup CI, error handling tambahan, task refactor) tapi TIDAK ada dasarnya dari FR/input user manapun.

**Aturan keras:** Task berlabel `[EXTRA]` **tidak boleh langsung masuk task list utama**. Kumpulkan semua task EXTRA di section terpisah "Task Tambahan (Perlu Persetujuan)" di akhir dokumen, dan minta user approve satu-satu sebelum dipindah ke task list utama / sebelum dikerjakan di `implement-task`.

Dua mode produksi (sama seperti sebelumnya):
- **Tech Spec** → task per modul, user ketik `lanjut` per modul.
- **Input langsung** → semua task langsung, tanpa pembagian modul.

### FASE 4: Finalisasi
1. Tampilkan task list utama (semua sudah berlabel sumber) untuk review.
2. Tampilkan terpisah section **Task Tambahan (Perlu Persetujuan)** jika ada — user harus jawab approve/tolak per item.
3. Instruksikan simpan ke `.agents/3-TASKS.md`.
4. Rekomendasikan lanjut ke **Implementation**.

## 📑 Struktur Task
- **ID:** T-XX
- **Judul:** [Nama task]
- **Deskripsi:** [Penjelasan singkat]
- **Modul:** [Nama modul]
- **Sumber:** `← FR-XX` / `← Input langsung user` / `[EXTRA]` *(field baru, wajib)*
- **Prioritas:** High/Mid/Low
- **Status:** Todo/In Progress/Done
- **Dependensi:** [ID task yang harus selesai dulu]
- **Estimasi:** [Jam/hari]
- **File yang diubah:** [Path file]

## 📄 TASK LIST

> AI generate task, konkret dan actionable. Task yang muncul tergantung konten + hasil auto-detect — tidak ada template tetap.
>
> **Setiap task wajib punya field Sumber terisi. Tidak boleh kosong.**
> Jika AI ragu apakah suatu task punya dasar dari FR atau tidak — default-kan ke `[EXTRA]`, karena lebih aman ketahuan dan diapprove user daripada diam-diam masuk sebagai "task biasa".

```markdown
### Task Utama (dari acuan)
**T-01: [Judul]**
- Sumber: ← FR-02
- Prioritas: High
- Status: Todo
...

### Task Tambahan (Perlu Persetujuan) — TIDAK dikerjakan sampai user approve
**T-EXTRA-01: [Judul]**
- Sumber: [EXTRA — diusulkan AI]
- Alasan diusulkan: [kenapa AI pikir ini perlu]
- Status: Menunggu persetujuan
```

AI urutkan task utama berdasarkan dependensi. Jangan buat task yang tidak relevan (skip database jika tidak pakai DB, skip auth jika tidak disebut acuan).

## 🔄 Aturan Lintas-Skill
- Skill **implement-task** WAJIB menolak mengerjakan task berstatus "Menunggu persetujuan" sampai user eksplisit approve dan memindahkannya ke task utama.
