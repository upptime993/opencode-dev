---
name: idea-intake
description: Tangkap ide kasar/mentah dari user dan ubah jadi brief terstruktur dengan asumsi yang tervalidasi, sebelum dokumen apapun dibuat. Ini GATE PERTAMA di pipeline — mencegah AI berasumsi diam-diam. Use when: user bilang "saya mau buat aplikasi...", "saya punya ide...", "gw pengen bikin...", user kasih deskripsi produk yang singkat/ambigu, atau user minta mulai proyek baru tanpa detail jelas.
---

# SKILL: IDEA INTAKE (Anti-Halu Gate)

## 📋 Metadata
- **Nama:** Idea Intake
- **Versi:** 1.0
- **Output:** Brief tervalidasi
- **Lokasi File:** `.agents/0-BRIEF.md`
- **Dependensi:** Tidak ada — ini titik masuk pipeline (langkah 0, sebelum PRD)

## 🎯 Trigger Keywords
Otomatis aktif saat user menyebut:
- "Saya mau buat aplikasi..."
- "Saya punya ide..."
- "Gw pengen bikin..."
- "Mau mulai proyek baru"
- "Baca brief saya" / "Load brief"

**Auto-redirect:** Jika skill `mini-prd` dipanggil tapi `.agents/0-BRIEF.md` belum ada, AI HARUS redirect ke skill ini dulu sebelum lanjut PRD. Idea Intake selalu jadi langkah pertama — tidak boleh dilewati.

## 📝 Filosofi
Sumber halusinasi AI di tahap planning bukan karena AI "ngarang jahat" — tapi karena ide user selalu datang ringkas/ambigu, dan AI dipaksa harus "lengkap" dalam satu jalan sehingga diam-diam mengisi celah dengan tebakan sendiri. Skill ini membalik urutannya: **ambiguitas dibuat eksplisit dan divalidasi user DULU**, sebelum satu kalimat pun PRD ditulis.

**Aturan besi:** AI DILARANG menuliskan tebakan sebagai fakta di mana pun dalam dokumen ini. Setiap tebakan wajib ditawarkan sebagai pilihan ke user, atau ditandai `[DEFAULT — belum dikonfirmasi]` jika user memilih skip.

## 📂 Baca/Menyimpan File
- **Baca:** `@.agents/0-BRIEF.md`
- **Simpan:** `.agents/0-BRIEF.md`

## ⚙️ Cara Kerja Skill

### FASE 1: Tangkap Ide Mentah (Verbatim)
Simpan kalimat ide user **persis apa adanya**, tanpa diparafrase atau "dirapikan" — ini jadi acuan ground-truth yang tidak berubah sepanjang pipeline.

### FASE 2: Ekstrak Unknowns (Bukan Pertanyaan Template)
AI membaca ide user lalu mengekstrak hal-hal spesifik yang ambigu — **hasil parsing ide itu sendiri**, bukan daftar pertanyaan generik yang sama untuk semua proyek ("siapa target user" dst tidak boleh jadi template tetap).

**Aturan format pertanyaan (wajib):**
1. **Closed-choice** (opsi A/B/C) — jangan open-ended. User jaman sekarang males ngetik panjang; closed-choice mengurangi ambiguitas jawaban juga.
2. **Setiap opsi kasih default + alasan singkat** jika user tidak menjawab atau bilang "terserah"/"bebas".
3. **Maksimal 5-7 unknowns per sesi**, diurutkan dari yang paling berdampak ke arsitektur (data model, siapa penggunanya, online/offline, auth) ke yang paling kosmetik (nama, warna).
4. Jangan tanya hal yang **sudah** implisit jelas dari ide user — hanya tanya yang benar-benar ambigu.

**Contoh:**
```
User: "gw mau bikin app buat nyatet pengeluaran harian"

AI: 🔍 Beberapa hal yang belum jelas dari idemu:

1. Dipakai sendiri, atau mau di-share ke orang lain (misal pasangan)?
   a) Cuma saya sendiri
   b) Bisa di-share / multi-user
   → default kalau skip: (a) — gak disebut kebutuhan sharing

2. Perlu internet terus, atau harus bisa dipakai offline?
   a) Online aja
   b) Harus jalan offline juga
   → default: (a)

3. Kategori pengeluaran itu preset (makan/transport/dst), atau user bikin sendiri?
   a) Preset
   b) Custom
   → default: (b) — app finansial personal biasanya butuh fleksibel

Jawab pakai format singkat, misal: "1a 2b 3b" — atau bilang "pakai default semua".
```

### FASE 3: Konfirmasi
User boleh jawab sekaligus (`1a 2b 3b`) atau satu-satu, atau bilang "pakai default semua". Untuk poin yang tidak dijawab eksplisit, AI pakai default **tapi tetap ditandai** `[DEFAULT — belum dikonfirmasi]` — tidak pernah ditulis seolah itu fakta yang user minta.

### FASE 4: Deteksi Constraint Keras
Tanyakan singkat HANYA jika relevan dari konteks ide (jangan tanya semua constraint template ke semua ide):
- Ada deadline?
- Ada batasan platform (wajib web/mobile/desktop)?
- Ada stack/tools yang wajib dipakai (misal karena sudah ada tim/infra)?

Jika user tidak menyebut apapun soal ini → **JANGAN diisi asumsi apapun**. Tulis "Belum ditentukan", biar tech spec nanti tahu ini masih ruang terbuka, bukan sudah diputuskan.

### FASE 5: Simpan Brief

### FASE 6: Finalisasi
1. Tampilkan ringkasan brief lengkap ke user untuk direview sekali lagi.
2. Instruksikan: *"Kalau sudah sesuai, ketik 'buat PRD dari brief ini' untuk lanjut."*

## 📄 Format `.agents/0-BRIEF.md`

```markdown
# BRIEF: [Nama Proyek Sementara]

## Ide Asli (Verbatim dari User)
> "[copy persis kalimat/paragraf user, jangan diedit atau dirapikan]"

## ✅ Terkonfirmasi User
- [Poin yang user jawab eksplisit, dengan nomor rujukan U-01, U-02, dst]

## ⚠️ Default Belum Dikonfirmasi
- [DEFAULT] [poin] — alasan: [kenapa AI pilih ini]

## 🚧 Constraint Keras
- **Deadline:** [ada, sebutkan / Belum ditentukan]
- **Platform wajib:** [ada, sebutkan / Belum ditentukan]
- **Stack wajib:** [ada, sebutkan / Belum ditentukan]

## Catatan Tambahan
[hal lain yang relevan dari percakapan tapi belum masuk kategori di atas]
```

## 🔄 Aturan Lintas-Skill (WAJIB dipatuhi skill lain)
- Skill **mini-prd** WAJIB membaca `0-BRIEF.md` sebelum mulai. Tidak boleh mulai PRD dari nol tanpa brief ini.
- Setiap poin `[DEFAULT — belum dikonfirmasi]` di brief HARUS diturunkan sebagai `[ASUMSI]` di PRD — labelnya tidak boleh "hilang" atau berubah jadi pernyataan fakta biasa saat naik ke dokumen berikutnya.
- Ide Asli (verbatim) HARUS tetap bisa dirujuk balik dari PRD/Tech Spec/Tasks kapan pun diperlukan untuk cek "apakah ini benar diminta user".
