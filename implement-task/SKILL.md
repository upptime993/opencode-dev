---
name: implement-task
description: Execute and automate coding tasks from the .agents/3-TASKS.md, hanya mengerjakan task yang sudah punya sumber jelas (FR/input user) atau sudah diapprove eksplisit jika berlabel EXTRA — mencegah AI membangun fitur di luar permintaan tanpa sepengetahuan user. Use when: user says "kerjakan task", "lanjutkan implementasi", "selesaikan tugas", or "start working on tasks".
---
# SKILL: IMPLEMENT TASK (Anti-Halu, Vibe Coding Ready)

---

## 📋 Metadata
- **Nama:** Implement Task
- **Versi:** 2.0
- **Output:** Kode implementasi dari task yang dipilih
- **Dependensi:** `3-TASKS.md` (wajib dibaca)

## 🎯 Trigger Keywords
- "Implement task" / "Kerjakan task" / "Mulai implementasi"
- "Coding task" / "Buat kode untuk task" / "Jalankan task"

## 📝 Deskripsi
Membaca daftar task dari `.agents/3-TASKS.md` dan mengimplementasikannya satu per satu. **Aturan baru:** AI tidak boleh mengerjakan task apapun yang statusnya "Menunggu persetujuan" (label `[EXTRA]`) — itu artinya task itu belum disetujui user dan mengerjakannya sama saja membangun sesuatu yang tidak diminta.

## 📂 Baca/Menyimpan File
- **Baca Task (WAJIB):** `@.agents/3-TASKS.md`
- **Baca Tech Spec:** `@.agents/2-TECH-SPEC.md` (opsional, referensi)
- **Update Task:** `.agents/3-TASKS.md`

## ⚙️ Cara Kerja Skill

### FASE 1: Deteksi Trigger & Baca Task
1. Cek `.agents/3-TASKS.md`. Tidak ada → minta user buat Task dulu.
2. Pisahkan task menjadi 2 kelompok: **Task Utama** (siap dikerjakan) dan **Task Tambahan/EXTRA** (perlu persetujuan — tidak boleh disentuh).
3. Auto-detect boilerplate (sama seperti sebelumnya).

### FASE 2: Pilih Task
1. Tampilkan daftar task **Todo dari Task Utama saja**.
2. Jika ada task EXTRA yang belum diapprove, tampilkan sebagai catatan terpisah:
   ```
   ⚠️ Ada 2 task tambahan yang diusulkan AI, belum kamu approve:
   - T-EXTRA-01: [judul] — [alasan diusulkan]
   Mau di-approve, ditolak, atau dibahas dulu?
   ```
   Jangan lanjut mengerjakannya sampai user merespons eksplisit.
3. Beri nomor pada task Todo, tanyakan: "Task mana yang ingin diimplementasikan?"

Jika user tidak memilih, ambil task Utama prioritas **High** pertama — **tidak pernah** task EXTRA yang belum diapprove, bahkan jika prioritasnya ditulis High oleh AI sendiri.

### FASE 3: Implementasi Task
1. Baca detail task (judul, deskripsi, sumber `← FR-XX`, dependensi, file yang diubah).
2. **Cek kesesuaian scope:** sebelum menulis kode, cocokkan deskripsi task dengan FR aslinya (jika ada) — pastikan implementasi tidak "berkembang" jadi lebih luas dari yang tertulis di task (misal task-nya "tampilkan daftar", jangan diam-diam tambah fitur edit/hapus tanpa task terpisah).
3. Cek dependensi: task sebelumnya sudah selesai? Jika belum, beri peringatan dan tawarkan kerjakan dependensinya dulu.
4. Baca file yang akan diubah (jika sudah ada).
5. Tulis kode implementasi — sesuaikan stack dan konteks project. Jika di tengah implementasi AI menyadari perlu menambah sesuatu di luar scope task (misal helper function tambahan, validasi ekstra) yang cukup signifikan → **jangan langsung tulis diam-diam**; sebutkan ke user dulu sebagai catatan kecil, sisanya tetap boleh jalan untuk hal-hal remeh teknis (bukan keputusan produk).
6. Update status task menjadi "Done".

### FASE 4: Finalisasi & Next Step
1. Beri tahu file apa saja yang berubah.
2. Sebutkan singkat apakah implementasi 100% sesuai scope task, atau ada penyesuaian kecil yang dilakukan (dan kenapa).
3. Tanyakan: "Lanjut ke task berikutnya? (y/n)"

**Contoh respons:**
```
✅ Task T-03: Create User Model selesai! (← FR-01)

File yang diubah:
- [path sesuai stack]

Catatan: sesuai scope task, tidak ada penambahan di luar deskripsi.

Lanjut ke task berikutnya? (y/n)
```

## 📝 Format Output Implementasi
Setiap task selesai, AI HARUS menampilkan:
1. ✅ **Konfirmasi selesai** — Task T-XX: [Nama Task] ✅ *(dengan sumber `← FR-XX` jika ada)*
2. **File yang diubah**
3. **Catatan kesesuaian scope** — sesuai / ada penyesuaian kecil (jelaskan)
4. **Testing** — cara menguji hasil implementasi
5. **Ajakan lanjut**

Kode ditulis langsung ke file — tidak perlu ditampilkan penuh di respons.
