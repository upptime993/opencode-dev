# 🚀 OpenCode Dev — My OpenCode Skills v2

Kumpulan **skill** untuk **OpenCode** dan **Claude Code** yang mengikuti alur kerja engineering lengkap: dari ide mentah → PRD → Tech Spec → task → implementasi → verifikasi.

## 🧩 Daftar Skill

| Skill | Fungsi | Langkah |
|-------|--------|---------|
| `idea-intake` | Tangkap ide mentah → brief tervalidasi (Anti-Halu Gate) | 0️⃣ |
| `mini-prd` | Bikin PRD ringkas dari brief | 1️⃣ |
| `write-tech-spec` | Bikin Tech Spec teknis (arsitektur, API, struktur) | 2️⃣ |
| `create-issues` | Pecah jadi task list (TASKS.md) | 3️⃣ |
| `implement-task` | Implementasi task satu-satu | 4️⃣ |
| `verify` | Verifikasi hasil (import/compile/smoke test) | 5️⃣ |
| `learnit` | Belajar pola & struktur dari repo lain | bonus |
| `legacy-decoder` | Baca & dokumentasikan kode legacy | bonus |

## 🔄 Alur Kerja (Pipeline)

```
idea-intake → mini-prd → write-tech-spec → create-issues → implement-task → verify
```

## 📦 Install

### OpenCode
```bash
# Salin skill ke folder config OpenCode
cp -r skills/* ~/.config/opencode/skills/
```

### Claude Code
```bash
# Salin skill ke folder Claude Code
cp -r skills/* ~/.claude/skills/
```

## 📁 Struktur

Setiap skill berformat **Agent Skills** (frontmatter `name` + `description`), jadi kompatibel dengan OpenCode & Claude Code.

```
opencode-dev/
├── idea-intake/         # SKILL.md + template
├── mini-prd/
├── write-tech-spec/
├── create-issues/
├── implement-task/
├── verify/
├── learnit/
├── legacy-decoder/
└── README.md
```

---
© upptime993
