Panduan lengkap Bahasa Indonesia untuk menggunakan repository **[`mattpocock/skills`](https://github.com/mattpocock/skills)** dalam workflow engineering, terutama untuk **Unity/C#** dan **ASP.NET Core / .NET Backend**, dengan **Codex**, **Google Antigravity**, dan **JetBrains Rider**.

> **Terakhir diverifikasi:** 30 Agustus 2026 terhadap branch `main` repository `mattpocock/skills`.
>
> Fokus dokumen ini adalah **cara memakai skill secara praktis**. Nama skill tetap dalam Bahasa Inggris karena nama folder/identifier harus sama dengan upstream, tetapi seluruh prompt dan penjelasan dapat menggunakan **Bahasa Indonesia**.

---

# 1. Apa Itu `mattpocock/skills`?

`mattpocock/skills` adalah kumpulan **Agent Skills**: folder berisi `SKILL.md` dan, bila perlu, script/reference tambahan yang memberi coding agent sebuah workflow khusus.

Skill bukan library C#, bukan Unity package, dan bukan plugin Rider.

Secara sederhana:

```text
Anda
  ↓
Coding Agent
(Codex / Antigravity / Claude Code / agent kompatibel)
  ↓
mattpocock/skills
  ↓
workflow + engineering discipline
  ↓
repository Anda
```

Tanpa skill, agent sering bergerak seperti ini:

```text
Prompt
  ↓
Baca beberapa file
  ↓
Langsung coding
```

Dengan flow utama Matt Pocock:

```text
Idea / Requirement
       ↓
grill-with-docs
       ↓
(optional: prototype / research)
       ↓
to-spec
       ↓
to-tickets
       ↓
implement ticket
       ↓
  implement memanfaatkan
   TDD + code review
       ↓
commit
       ↓
next ticket
```

Perbedaan penting dibanding versi sederhana workflow ini: **`implement` upstream memang dirancang untuk mendorong `tdd` pada seam yang sudah disepakati dan menutup pekerjaan dengan `code-review` sebelum commit.** Jadi `implement → tdd → code-review` bukan tiga tahap yang selalu harus Anda trigger manual satu per satu.

---

# 2. Apakah Bisa Menggunakan Bahasa Indonesia?

**Ya.** Nama skill tetap misalnya `grill-with-docs`, tetapi isi prompt Anda boleh Bahasa Indonesia.

Contoh portable:

```text
Gunakan skill grill-with-docs.

Saya ingin membuat sistem inventory pada project Unity.
Interview saya sampai requirement dan edge case jelas.
Jangan implementasi dulu.
```

Atau pada harness yang menyediakan slash-command:

```text
/grill-with-docs
```

Syntax trigger dapat berbeda antar coding agent. Cara paling portable adalah **menyebut nama skill secara eksplisit** di prompt.

---

# 3. User-Invoked vs Model-Invoked

Repository upstream membedakan skill berdasarkan **siapa yang boleh mengaktifkannya**.

## User-invoked

Hanya Anda yang mengaktifkan skill tersebut secara eksplisit.

Contoh:

```text
setup-matt-pocock-skills
grill-with-docs
to-spec
to-tickets
implement
wayfinder
```

Di Codex, skill semacam ini memiliki metadata yang mencegah implicit invocation. Artinya jangan berharap agent otomatis menjalankan `to-spec` hanya karena percakapan terlihat matang; Anda yang meminta.

## Model-invoked

Agent boleh memilih skill secara otomatis ketika task cocok, tetapi Anda juga boleh menyebutnya langsung.

Contoh:

```text
tdd
diagnosing-bugs
code-review
research
codebase-design
domain-modeling
```

Prinsip penting upstream:

```text
User-invoked skill
    boleh mengorkestrasi
Model-invoked skill

User-invoked skill
    TIDAK boleh memanggil
User-invoked skill lain secara otomatis
```

---

# 4. Kompatibilitas Tool yang Anda Pakai

| Tool | Bisa memakai Agent Skills? | Cara penggunaan yang disarankan |
|---|---:|---|
| **Codex CLI** | ✅ | Install melalui `npx skills@latest add mattpocock/skills`, lalu jalankan dari root repo. |
| **Google Antigravity** | ✅ | Antigravity mendukung standar `SKILL.md`; workspace skill berada di `.agents/skills/<skill>/`. |
| **JetBrains Rider** | ⚠️ Bukan host skill | Gunakan Rider sebagai IDE, lalu jalankan Codex dari terminal di root repo atau gunakan agent lain yang membaca repo yang sama. |
| **Claude Code** | ✅ | Upstream juga tersedia sebagai plugin Claude Code. |

## Rider + Codex

Workflow yang sangat natural:

```text
Rider
 ├─ edit / inspect kode
 ├─ debugger
 ├─ Unity/.NET tooling
 └─ Terminal
      ↓
     codex
      ↓
 mattpocock/skills
```

Skill tidak peduli Anda membuka repository melalui Rider, VS Code, atau editor lain. Yang penting coding agent dijalankan pada repository yang sama.

## Antigravity

Antigravity mendukung Agent Skills berbasis `SKILL.md` dan mengenali workspace skills dari:

```text
<repo>/.agents/skills/<skill-name>/SKILL.md
```

Antigravity dapat memilih skill dari description secara otomatis atau Anda dapat menyebut nama skill secara eksplisit.

Beberapa skill **tidak benar-benar portable** karena secara sengaja khusus harness tertentu:

```text
claude-handoff
  → Claude-specific

git-guardrails-claude-code
  → Claude Code-specific
```

Untuk perpindahan Codex ↔ Antigravity, gunakan skill stabil:

```text
handoff
```

---

# 5. Instalasi untuk Codex

Dari root repository:

```bash
npx skills@latest add mattpocock/skills
```

Installer akan meminta Anda memilih skill dan coding agent yang ingin dipasangi skill.

Pastikan minimal memasang:

```text
setup-matt-pocock-skills
grill-with-docs
to-spec
to-tickets
implement
tdd
diagnosing-bugs
code-review
```

Untuk workflow Anda, saya juga sangat menyarankan:

```text
ask-matt
handoff
domain-modeling
codebase-design
improve-codebase-architecture
research
resolving-merge-conflicts
```

Update skill yang sudah dicopy ke project:

```bash
npx skills update
```

## Skill In-Progress/Beta

Skill dalam `skills/in-progress` tidak ikut bundle stabil. Install secara eksplisit:

```bash
npx skills@latest add mattpocock/skills --skill=<nama-skill>
```

Contoh:

```bash
npx skills@latest add mattpocock/skills --skill=implement-spec
```

---

# 6. Setup Repository Sekali Saja

Setelah install:

```bash
codex
```

Lalu:

```text
Gunakan skill setup-matt-pocock-skills untuk mengonfigurasi repository ini.
```

Skill setup memeriksa repository terlebih dahulu, lalu mengatur tiga hal utama.

## A. Issue Tracker

Pilihan upstream:

```text
GitHub
GitLab
Local Markdown
Other (Jira, Linear, dll. sebagai workflow custom)
```

Untuk GitHub, workflow bergantung pada `gh` CLI.

Cek:

```bash
gh auth status
git remote -v
```

Untuk project solo yang belum dipublish:

```text
Gunakan local markdown sebagai issue tracker.
```

Local issue biasanya disimpan di area `.scratch/` sesuai convention skill.

## B. Triage Labels

Jika `triage` terpasang, default labels:

```text
needs-triage
needs-info
ready-for-agent
ready-for-human
wontfix
```

Untuk kebanyakan project pribadi:

```text
Pertahankan default triage labels.
```

## C. Domain Docs

Default:

```text
CONTEXT.md
docs/adr/
docs/agents/
```

`CONTEXT.md` berfungsi sebagai bahasa bersama antara Anda, agent, dan codebase.

Contoh vocabulary Unity:

```text
Run
  = satu sesi permainan dari start sampai game over

Blind
  = target score yang harus dikalahkan

Joker Effect
  = rule yang memodifikasi scoring/economy/hand behavior
```

Contoh vocabulary backend:

```text
Transaction
  = pencatatan income atau expense

Posted Transaction
  = transaction yang sudah final dan memengaruhi balance
```

---

# 7. Main Flow Resmi: Idea → Ship

Flow yang paling sering dipakai:

```text
grill-with-docs
      ↓
  ada pertanyaan yang
  harus diuji dengan kode/UI?
      ├─ ya → handoff → prototype → handoff kembali
      └─ tidak
      ↓
  build lebih dari satu sesi?
      ├─ ya → to-spec → to-tickets
      │                    ↓
      │               implement per ticket
      │
      └─ tidak → implement langsung
```

`implement` kemudian dapat menggunakan:

```text
tdd
 ↓
code-review
 ↓
commit
```

## Context Hygiene

Upstream menyarankan:

```text
grill-with-docs
   ↓
to-spec
   ↓
to-tickets
```

sebisa mungkin berada dalam satu context yang masih sehat agar keputusan tidak tercecer.

Setelah ticket dibuat:

```text
Ticket #1 → fresh context → implement
Ticket #2 → fresh context → implement
Ticket #3 → fresh context → implement
```

Ini sangat cocok untuk Codex karena setiap ticket menjadi self-contained.

---

# 8. Katalog Semua Skill

## Stable Engineering

| Skill | Invocation | Fungsi singkat | Relevansi Anda |
|---|---|---|---|
| `ask-matt` | User | Router memilih skill/flow | ★★★★★ |
| `setup-matt-pocock-skills` | User | Setup tracker, labels, domain docs | ★★★★★ |
| `grill-with-docs` | User | Matangkan requirement + domain docs | ★★★★★ |
| `triage` | User | Triage incoming issues | ★★★★☆ |
| `improve-codebase-architecture` | User | Survey peluang memperdalam arsitektur | ★★★★★ |
| `to-spec` | User | Percakapan → spec | ★★★★★ |
| `to-tickets` | User | Spec → tracer-bullet tickets | ★★★★★ |
| `implement` | User | Implement spec/ticket | ★★★★★ |
| `wayfinder` | User | Peta decision tickets untuk kerja sangat besar | ★★★★☆ |
| `prototype` | Model/User | Prototype throwaway menjawab design question | ★★★☆☆ Unity / ★★★★☆ logic |
| `diagnosing-bugs` | Model/User | Diagnosis bug disiplin | ★★★★★ |
| `research` | Model/User | Riset sumber primer → cited Markdown | ★★★★☆ |
| `tdd` | Model/User | Red-green-refactor | ★★★★★ .NET / ★★★★☆ Unity |
| `domain-modeling` | Model/User | Perbaiki vocabulary/domain model | ★★★★★ |
| `codebase-design` | Model/User | Desain deep module & seam | ★★★★★ |
| `code-review` | Model/User | Review Standards + Spec | ★★★★★ |
| `resolving-merge-conflicts` | Model/User | Resolve merge/rebase by intent | ★★★★☆ |
| `wizard` | Model/User | Bash wizard untuk human-only steps | ★★★☆☆ |

## Stable Productivity

| Skill | Invocation | Fungsi singkat | Relevansi Anda |
|---|---|---|---|
| `grill-me` | User | Grilling stateless di luar repo | ★★★★☆ |
| `handoff` | User | Handoff context ke agent/session lain | ★★★★★ |
| `teach` | User | Belajar skill secara stateful lintas sesi | ★★★★☆ |
| `to-questionnaire` | User | Buat questionnaire untuk stakeholder | ★★★☆☆ |
| `wait-what` | User | Jelaskan ulang pesan yang tidak dipahami | ★★★★☆ |
| `grilling` | Model/User | Primitive interview | ★★★☆☆ |
| `writing-for-agents` | Model/User | Menulis instruction docs untuk agent | ★★★★☆ |

## In Progress / Beta

| Skill | Status | Fungsi singkat |
|---|---|---|
| `loop-me` | Beta | Membentuk workflow spec lintas sesi |
| `writing-beats` | Beta | Menulis artikel beat-by-beat |
| `writing-fragments` | Beta | Mengumpulkan raw writing fragments |
| `writing-shape` | Beta | Membentuk fragments menjadi artikel |
| `claude-handoff` | Beta, Claude-specific | Handoff ke Claude background agent |
| `setup-ts-deep-modules` | Beta, TypeScript | Dependency-cruiser + deep-module boundaries |
| `implement-spec` | Beta | Implement seluruh spec sebagai task graph pada satu branch |
| `retro` | STUB | Rencana retrospective environment agent; belum functional |

## Misc

| Skill | Catatan |
|---|---|
| `git-guardrails-claude-code` | Claude Code-specific |
| `migrate-to-shoehorn` | TypeScript-specific |
| `scaffold-exercises` | General learning utility |
| `setup-pre-commit` | Node/Husky-oriented |

## Deprecated

Bucket `skills/deprecated` saat verifikasi ini tidak memiliki active `SKILL.md`; hanya terdapat README bucket.

---

# 9. Stable Engineering Skills — Detail + Prompt Indonesia


## `ask-matt`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Router untuk memilih skill atau flow yang paling cocok ketika Anda tahu masalahnya tetapi belum tahu harus mulai dari skill mana.

**Kapan dipakai:**
- Saat lupa fungsi masing-masing skill.
- Saat bingung apakah harus mulai dari bug diagnosis, grill, architecture review, atau wayfinder.
- Saat ingin rekomendasi flow tanpa langsung mengubah kode.

**Catatan penting:**
- Ini adalah pintu masuk paling aman ketika ragu.
- Skill ini tidak menggantikan skill lain; tugasnya memilih jalur yang tepat.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill ask-matt.

Situasi saya:
- Project: [Unity 6 / ASP.NET Core .NET 8]
- Tujuan atau masalah: [jelaskan]
- Kondisi repo saat ini: [baru / sudah berjalan / legacy]

Tentukan skill atau flow mattpocock/skills yang paling cocok.
Jelaskan urutannya secara singkat.
Jangan implementasi apa pun dulu.
```

---

## `setup-matt-pocock-skills`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Setup sekali per repository: issue tracker, label triage, dan layout dokumentasi domain yang dipakai skill engineering lain.

**Kapan dipakai:**
- Pertama kali memasang mattpocock/skills pada repo.
- Saat mengganti issue tracker atau ingin mereset konfigurasi skill.

**Catatan penting:**
- GitHub adalah default; GitLab dan local Markdown didukung, tracker lain dapat dideskripsikan sebagai workflow custom.
- Jika `triage` terpasang, default labelnya: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`.
- Default domain docs adalah single-context: `CONTEXT.md` + `docs/adr/`.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill setup-matt-pocock-skills untuk mengonfigurasi repository ini.

Preferensi saya:
- Issue tracker: GitHub Issues
- Pertahankan default triage labels
- Gunakan layout domain docs yang direkomendasikan

Periksa repo terlebih dahulu dan tunjukkan rencana perubahan sebelum menulis file.
```

---

## `grill-with-docs`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Interview requirement secara mendalam sambil membangun bahasa domain bersama melalui `CONTEXT.md` dan ADR.

**Kapan dipakai:**
- Fitur baru masih berupa ide.
- Requirement terasa jelas di kepala tetapi belum tertulis.
- Ada istilah domain yang ambigu.
- Sebelum membuat spec untuk perubahan yang cukup penting.

**Catatan penting:**
- Gunakan ini ketika sedang berada di working directory/repository.
- Berbeda dengan `grill-me`, skill ini stateful dan meninggalkan paper trail di repo.
- Jangan minta implementasi dalam tahap ini.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill grill-with-docs.

Saya ingin membuat fitur [NAMA FITUR].

Konteks:
- Stack: [Unity 6 C# / ASP.NET Core .NET 8]
- Tujuan: [tujuan]
- Requirement awal:
  - ...
  - ...

Interview saya sampai requirement, edge case, istilah domain, dan keputusan penting cukup jelas.
Perbarui CONTEXT.md/ADR bila memang diperlukan.
Jangan implementasi kode dulu.
```

---

## `triage`

**Invocation:** User-invoked  
**Relevansi:** ★★★★☆ GitHub workflow  
**Fungsi:** Memindahkan issue yang masuk melalui state machine triage sampai jelas apakah butuh info, siap untuk agent, siap untuk manusia, atau ditolak.

**Kapan dipakai:**
- Ada bug report atau feature request dari luar yang masih mentah.
- Banyak issue menumpuk dan perlu diklasifikasikan sebelum dikerjakan.

**Catatan penting:**
- Gunakan untuk issue yang datang dari luar/raw request.
- Jangan triage ticket yang baru dibuat oleh `to-tickets`; ticket tersebut sudah dianggap agent-ready.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill triage.

Triage issue yang masih berstatus needs-triage pada repository ini.
Untuk setiap issue:
- tentukan apakah informasi cukup
- minta informasi tambahan bila perlu
- tandai ready-for-agent jika dapat dikerjakan agent
- tandai ready-for-human jika memerlukan keputusan manusia
- jangan mengimplementasikan issue apa pun.
```

---

## `improve-codebase-architecture`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Memindai codebase untuk peluang membuat modul lebih “deep”, mengurangi coupling/complexity, lalu menyajikan kandidat perbaikan untuk dipilih.

**Kapan dipakai:**
- Project mulai sulit dinavigasi agent maupun manusia.
- Service/manager tumbuh terlalu besar.
- Unity memiliki banyak singleton/manager saling tergantung.
- .NET layer menjadi saling bocor atau terlalu banyak interface tipis.

**Catatan penting:**
- Ini survey untuk menemukan kandidat, bukan refactor otomatis seluruh codebase.
- Setelah memilih kandidat, lanjutkan ke diskusi/desain lalu spec/ticket.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill improve-codebase-architecture.

Audit codebase ini untuk mencari deepening opportunities.
Fokus pada:
- coupling antar modul
- interface yang terlalu lebar atau terlalu tipis
- tanggung jawab yang bocor antar layer
- area yang sulit dites
- duplikasi policy/business rule

Jangan refactor dulu.
Tampilkan kandidat dan alasan prioritasnya terlebih dahulu.
```

---

## `to-spec`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Mengubah percakapan/keputusan yang sudah matang menjadi specification yang buildable dan dipublikasikan ke issue tracker sesuai konfigurasi repo.

**Kapan dipakai:**
- Diskusi requirement sudah selesai.
- Anda sudah punya keputusan yang cukup dan ingin menjadikannya sumber kebenaran implementasi.

**Catatan penting:**
- Skill ini terutama mensintesis apa yang sudah dibahas; bukan pengganti grilling.
- Jika requirement masih banyak lubang, kembali ke `grill-with-docs`.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill to-spec.

Ubah requirement dan keputusan yang sudah kita sepakati menjadi specification implementasi.
Pastikan mencakup:
- tujuan
- scope dan out-of-scope
- perilaku utama
- edge cases
- modul/seam yang terlibat
- acceptance criteria tingkat fitur

Jangan mulai implementasi.
```

---

## `to-tickets`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Memecah spec/plan menjadi tracer-bullet tickets kecil yang memiliki blocking edges/dependencies yang jelas.

**Kapan dipakai:**
- Spec terlalu besar untuk satu sesi.
- Ingin mengerjakan satu ticket per sesi/commit.
- Ingin ticket yang dapat berjalan blockers-first.

**Catatan penting:**
- Prioritaskan vertical/tracer bullet dibanding memecah murni berdasarkan layer bila memungkinkan.
- Pada tracker nyata, dependency dapat direpresentasikan sebagai blocking links; pada local tracker menjadi file Markdown.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill to-tickets.

Pecah specification ini menjadi ticket implementasi kecil.
Setiap ticket harus:
- punya scope yang jelas
- punya acceptance criteria
- menyatakan blocking/dependency edges
- dapat diverifikasi secara independen jika memungkinkan
- menghindari ticket yang terlalu besar

Jangan implementasi apa pun.
```

---

## `implement`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Mengerjakan spec atau ticket. Flow upstream mendorong TDD pada seam yang disepakati dan menutup pekerjaan dengan code review sebelum commit.

**Kapan dipakai:**
- Ticket/spec sudah cukup jelas dan siap dikerjakan.
- Anda ingin agent fokus pada satu unit pekerjaan.

**Catatan penting:**
- Untuk workflow multi-ticket, mulai sesi/context baru per ticket agar konteks tetap bersih.
- Tidak perlu selalu memanggil `tdd` dan `code-review` terpisah karena `implement` memang dirancang mengorkestrasi keduanya.
- Tetap sebutkan “ticket ini saja” bila Anda ingin scope sangat ketat.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill implement.

Implementasikan ticket #[NUMBER] saja.

Ikuti:
- specification terkait
- acceptance criteria ticket
- convention repository yang sudah ada

Gunakan TDD pada seam yang sesuai.
Jalankan test yang relevan.
Lakukan code review sebelum menyatakan selesai.
Jangan mengerjakan ticket lain.
```

---

## `wayfinder`

**Invocation:** User-invoked  
**Relevansi:** ★★★★☆ Project sangat besar  
**Fungsi:** Memetakan pekerjaan yang terlalu besar/berkabut untuk satu sesi menjadi decision tickets, lalu menyelesaikan keputusan satu per satu sampai jalur implementasi terlihat.

**Kapan dipakai:**
- Greenfield project besar.
- Migrasi arsitektur besar.
- Fitur lintas banyak subsystem yang arah implementasinya belum jelas.

**Catatan penting:**
- Wayfinder menghasilkan keputusan, bukan deliverable kode.
- Setelah peta jelas, biasanya lanjut `to-spec` → `to-tickets` → `implement`.
- Jangan gunakan untuk fitur kecil yang sudah well-scoped.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill wayfinder.

Tujuan besar saya:
[DESKRIPSI]

Project: [Unity / .NET / gabungan]
Batasan utama:
- ...
- ...

Buat peta decision tickets untuk mengurangi ketidakpastian.
Selesaikan keputusan blockers-first.
Jangan mulai implementasi produksi sampai jalurnya cukup jelas untuk diubah menjadi spec.
```

---

## `prototype`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★☆☆ Unity / ★★★★☆ Web/logic  
**Fungsi:** Membuat prototype throwaway untuk menjawab satu pertanyaan desain, terutama state/logic atau beberapa variasi UI yang dapat dibandingkan.

**Kapan dipakai:**
- Diskusi saja tidak cukup untuk menentukan bentuk state model.
- Perlu melihat/merasakan beberapa alternatif UI sebelum memilih.

**Catatan penting:**
- Prototype upstream berorientasi output HTML yang mudah dibagikan.
- Untuk Unity, cocok untuk memvalidasi flow/state/UI concept, tetapi bukan pengganti prototype nyata jika pertanyaannya bergantung pada Physics, Animator, Input System, URP, scene lifecycle, atau performa engine.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill prototype untuk menjawab satu pertanyaan desain berikut:

Pertanyaan:
[CONTOH: state machine shop Balatro-like mana yang paling sederhana?]

Buat prototype throwaway yang hanya cukup untuk membandingkan alternatif.
Jangan mengubah production implementation.
Tuliskan kesimpulan yang dapat dibawa kembali ke spec.
```

---

## `diagnosing-bugs`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Loop diagnosis disiplin untuk bug sulit/performance regression: reproduksi → minimisasi → hipotesis → instrumentasi → root cause → fix → regression test.

**Kapan dipakai:**
- Bug tidak jelas penyebabnya.
- Bug intermittent/flaky.
- Regression muncul setelah perubahan tertentu.
- Performance drop yang perlu dibuktikan.

**Catatan penting:**
- Kunci utama: bangun feedback loop yang benar-benar merah pada bug ini sebelum berteori terlalu jauh.
- Jika root cause sudah benar-benar diketahui, Anda dapat langsung memakai `tdd` untuk regression test + fix.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill diagnosing-bugs.

Bug:
[DESKRIPSI]

Expected:
[EXPECTED]

Actual:
[ACTUAL]

Pertama buat reproduksi/feedback loop yang dapat dipercaya.
Lalu minimalkan kasus, bentuk hipotesis, instrumentasikan bila perlu, dan identifikasi root cause.
Jangan melakukan perubahan spekulatif.
Setelah root cause terbukti, tambahkan regression test dan perbaiki penyebabnya.
```

---

## `research`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★☆ Unity / .NET  
**Fungsi:** Riset teknis berbasis sumber primer/high-trust lalu menyimpan hasil sebagai Markdown bercitation di repo.

**Kapan dipakai:**
- Perlu memastikan behavior API/library/engine sebelum desain.
- Membandingkan pilihan teknologi dari dokumentasi resmi.
- Perlu catatan riset yang bisa dibaca agent lain.

**Catatan penting:**
- Upstream merancang skill ini untuk background agent bila harness mendukung; dukungan concurrency/background bergantung agent yang Anda pakai.
- Riset memberi bahan untuk `grill-with-docs`; riset tidak menggantikan keputusan desain.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill research.

Riset pertanyaan berikut:
[PERTANYAAN]

Prioritaskan sumber primer/resmi.
Contoh untuk project saya:
- Unity Manual / Unity Scripting API
- Microsoft Learn / ASP.NET Core / EF Core docs
- dokumentasi library resmi

Simpan hasil sebagai Markdown dengan citation/link sumber.
Pisahkan fakta, trade-off, dan rekomendasi.
```

---

## `tdd`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★★ .NET / ★★★★☆ Unity  
**Fungsi:** Test-driven development red → green → refactor, satu vertical slice kecil pada satu waktu.

**Kapan dipakai:**
- Membangun behavior yang dapat dites dari interface/seam yang jelas.
- Memperbaiki bug dengan regression test.

**Catatan penting:**
- Di .NET sangat cocok untuk domain/service/API behavior dengan xUnit/NUnit/MSTest sesuai project.
- Di Unity, prioritaskan pure C# logic/EditMode test; gunakan PlayMode test saat behavior memang bergantung GameObject/scene/lifecycle.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill tdd.

Implementasikan behavior berikut dengan red-green-refactor:
[BEHAVIOR]

Aturan:
1. Tulis test yang gagal karena behavior belum ada.
2. Buat perubahan minimum agar test lolos.
3. Refactor tanpa mengubah behavior.
4. Jalankan test relevan setelah setiap slice.

Jangan menambahkan abstraction yang belum dibutuhkan.
```

---

## `domain-modeling`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Memperjelas bahasa domain, menantang istilah fuzzy/overloaded, menguji istilah dengan edge-case scenarios, serta memperbarui `CONTEXT.md`/ADR.

**Kapan dipakai:**
- Nama entity/service/state membingungkan.
- Satu kata dipakai untuk beberapa konsep.
- Business/game rule kompleks perlu vocabulary yang konsisten.

**Catatan penting:**
- Membaca `CONTEXT.md` saja bukan domain-modeling; skill ini untuk kerja aktif memperbaiki model/vocabulary.
- Sangat berguna pada game dengan banyak istilah rule/state dan backend dengan domain rules.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill domain-modeling.

Audit istilah domain berikut:
[DAFTAR ISTILAH]

Cari:
- istilah yang ambigu
- satu istilah dengan beberapa arti
- dua istilah untuk konsep yang sama
- edge case yang merusak definisi saat ini

Usulkan vocabulary yang lebih konsisten dan perbarui CONTEXT.md/ADR bila keputusan penting sudah disepakati.
```

---

## `codebase-design`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Vocabulary/disiplin desain deep modules: banyak behavior di balik interface kecil, seam bersih, locality tinggi, dan dapat dites melalui interface.

**Kapan dipakai:**
- Mendesain batas module/service/system baru.
- Refactor manager besar menjadi module yang lebih sehat.
- Mencari seam untuk testing.

**Catatan penting:**
- Ini bukan style guide C#; fokusnya bentuk modul dan boundary.
- Gunakan bersama `improve-codebase-architecture` ketika kandidat masalah arsitektur sudah ditemukan.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill codebase-design.

Bantu desain ulang area berikut sebagai deep module:
[AREA]

Evaluasi:
- public interface
- hidden implementation
- seam/dependency boundary
- locality
- testability melalui interface
- informasi yang seharusnya disembunyikan

Berikan 2 alternatif desain dan trade-off sebelum memilih.
```

---

## `code-review`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★★ Unity / .NET  
**Fungsi:** Review diff dari dua sumbu: Standards (convention + code smells) dan Spec (kesetiaan terhadap issue/spec).

**Kapan dipakai:**
- Satu ticket selesai.
- Sebelum commit/PR.
- Ingin review branch terhadap base/fixed point.

**Catatan penting:**
- Upstream dapat menggunakan parallel subagents bila harness mendukung.
- Minta findings dulu sebelum fix jika Anda ingin memisahkan review dan perubahan.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill code-review.

Review perubahan untuk ticket #[NUMBER] terhadap fixed point/base yang sesuai.

Periksa dua sumbu:
1. Standards: convention repo, bug risk, smell, complexity, test quality.
2. Spec: apakah implementasi memenuhi spec dan acceptance criteria.

Laporkan findings dengan severity dan lokasi.
Jangan ubah kode dulu.
```

---

## `resolving-merge-conflicts`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★☆ Unity / .NET  
**Fungsi:** Menyelesaikan merge/rebase conflict hunk demi hunk berdasarkan intent dari kedua sisi, lalu menyelesaikan operasi Git.

**Kapan dipakai:**
- Repo sedang berada di tengah merge/rebase conflict.
- Conflict tidak aman diselesaikan hanya dengan memilih ours/theirs.

**Catatan penting:**
- Skill upstream sengaja tidak melakukan `--abort`.
- Untuk file Unity YAML (`.unity`, `.prefab`, `.asset`), berhati-hati ekstra; konflik serialized asset sering memerlukan pemahaman scene/prefab, bukan sekadar teks.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill resolving-merge-conflicts.

Repository sedang berada dalam merge/rebase conflict.
Selesaikan conflict satu hunk pada satu waktu berdasarkan intent dari kedua perubahan.
Telusuri commit/source yang relevan sebelum memilih hasil.
Jalankan test/validation setelah conflict selesai.
Jangan abort operasi Git.
```

---

## `wizard`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★☆☆ Ops / setup eksternal  
**Fungsi:** Membuat interactive Bash wizard untuk langkah yang memang harus dilakukan manusia, seperti credential, secret, dashboard pihak ketiga, provisioning, migration, atau cutover.

**Kapan dipakai:**
- Agent terhalang langkah manual di dashboard.
- Perlu prosedur setup berulang yang aman dan terdokumentasi.

**Catatan penting:**
- Bukan untuk pekerjaan yang sebenarnya bisa dilakukan agent langsung.
- Cocok misalnya setup GitHub Secrets, service account, CI credential, atau deployment checklist.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill wizard.

Buat interactive Bash wizard untuk proses berikut:
[PROSES MANUAL]

Wizard harus:
- menjelaskan langkah manusia satu per satu
- membuka/menunjukkan URL yang benar bila perlu
- meminta nilai yang diperlukan dengan jelas
- tidak mencetak secret secara sembrono
- melakukan validasi setelah setiap langkah bila memungkinkan.
```

---

# 10. Stable Productivity Skills — Detail + Prompt Indonesia


## `grill-me`

**Invocation:** User-invoked  
**Relevansi:** ★★★★☆ Umum  
**Fungsi:** Interview mendalam yang stateless untuk mematangkan rencana/desain ketika Anda tidak sedang bekerja di repository.

**Kapan dipakai:**
- Brainstorm di luar repo.
- Merancang ide sebelum menentukan project directory.

**Catatan penting:**
- Jika sedang berada di repo, `grill-with-docs` biasanya lebih baik karena menyimpan vocabulary/decision ke docs.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill grill-me.

Saya punya ide berikut:
[IDE]

Interview saya sampai semua keputusan penting cukup jelas.
Jangan membuat implementasi atau file project dulu.
```

---

## `handoff`

**Invocation:** User-invoked  
**Relevansi:** ★★★★★ Multi-agent / ganti IDE  
**Fungsi:** Mengompakkan context percakapan menjadi dokumen handoff portable agar agent/session/directory lain dapat melanjutkan pekerjaan.

**Kapan dipakai:**
- Pindah dari Codex ke Antigravity atau sebaliknya.
- Membuka sesi baru agar context bersih.
- Memindahkan side task ke directory/harness lain.

**Catatan penting:**
- Handoff sangat berguna untuk kombinasi Rider + Codex + Antigravity karena state penting tidak bergantung chat lama.
- Tetap commit/checkpoint perubahan kode; handoff bukan pengganti Git.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill handoff.

Buat handoff dari pekerjaan saat ini agar agent baru bisa melanjutkan tanpa membaca seluruh chat.
Masukkan:
- tujuan
- keputusan yang sudah dibuat
- file/modul relevan
- apa yang sudah selesai
- apa yang belum selesai
- test/status Git
- next action yang paling aman.
```

---

## `teach`

**Invocation:** User-invoked  
**Relevansi:** ★★★★☆ Belajar Unity / .NET  
**Fungsi:** Mengajar konsep/skill secara bertahap lintas beberapa sesi dengan current directory sebagai teaching workspace stateful.

**Kapan dipakai:**
- Belajar DDD, testing, EF Core, async, Unity architecture, dsb.
- Ingin latihan bertahap, bukan jawaban satu kali.

**Catatan penting:**
- Berguna jika Anda ingin agent menyimpan progress belajar, glossary, mission, dan resources di workspace.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill teach.

Ajari saya topik: [TOPIK].
Level saya saat ini: [LEVEL].
Target: [TARGET].

Gunakan C# dan contoh yang relevan dengan [Unity / ASP.NET Core].
Buat pembelajaran bertahap dengan latihan kecil dan checkpoint pemahaman.
```

---

## `to-questionnaire`

**Invocation:** User-invoked  
**Relevansi:** ★★★☆☆ Team / stakeholder  
**Fungsi:** Mengubah keputusan yang tidak bisa Anda jawab sendiri menjadi questionnaire Markdown untuk orang yang memiliki jawabannya.

**Kapan dipakai:**
- Butuh keputusan product owner/designer/domain expert.
- Requirement terblokir informasi stakeholder.

**Catatan penting:**
- Skill menginterview Anda tentang siapa penerima dan informasi apa yang dibutuhkan, lalu pertanyaan diarahkan ke gap tersebut.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill to-questionnaire.

Saya perlu mendapatkan keputusan dari [ROLE/ORANG] mengenai:
[TOPIK]

Saya membutuhkan jawaban agar dapat menyelesaikan:
[KEPUTUSAN/SPEC]

Buat questionnaire Markdown yang ringkas tetapi cukup untuk menghilangkan ambiguity.
```

---

## `wait-what`

**Invocation:** User-invoked  
**Relevansi:** ★★★★☆ Umum  
**Fungsi:** Meminta agent mengulang penjelasan yang baru saja tidak “masuk”, dengan konteks yang hilang dan bahasa yang lebih sederhana memakai vocabulary project.

**Kapan dipakai:**
- Penjelasan agent terlalu jargon.
- Anda memahami kode tetapi tidak memahami alasan desainnya.

**Catatan penting:**
- Dipakai setelah penjelasan yang membingungkan; untuk mencegah jargon sejak awal, domain vocabulary yang baik dari `grill-with-docs` lebih efektif.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill wait-what.

Saya belum paham penjelasan terakhir.
Jelaskan ulang dengan Bahasa Indonesia sederhana, gunakan istilah yang sudah ada di CONTEXT.md, dan berikan satu contoh konkret dari codebase ini.
```

---

## `grilling`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★☆☆ Primitive  
**Fungsi:** Primitive interview yang dipakai oleh berbagai skill untuk mengeksplorasi keputusan sampai semua cabang desain terselesaikan.

**Kapan dipakai:**
- Anda memang ingin primitive interview tanpa wrapper/penyimpanan khusus.

**Catatan penting:**
- Biasanya lebih baik menggunakan `grill-with-docs` atau `grill-me` daripada memanggil primitive ini langsung.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill grilling untuk menginterview saya tentang keputusan berikut:
[KEPUTUSAN]

Pisahkan fakta yang bisa Anda cari sendiri dari keputusan yang memang harus saya buat.
```

---

## `writing-for-agents`

**Invocation:** Model-invoked / bisa dipanggil user  
**Relevansi:** ★★★★☆ Codex / Antigravity  
**Fungsi:** Pedoman menulis dokumen yang dikonsumsi agent: `SKILL.md`, `AGENTS.md`, `CLAUDE.md`, dan dokumen yang dirujuk agent.

**Kapan dipakai:**
- Membuat AGENTS.md untuk .NET/Unity repo.
- Membuat skill custom sendiri.
- Merapikan instruction docs agar hemat context dan tidak ambigu.

**Catatan penting:**
- Sangat relevan jika Anda ingin mengadaptasi mattpocock/skills untuk convention C# atau Unity sendiri.

**Contoh prompt Bahasa Indonesia:**

```text
Gunakan skill writing-for-agents.

Review dan perbaiki dokumen agent berikut:
[AGENTS.md / SKILL.md / docs agent]

Target:
- instruksi eksplisit
- mudah ditemukan agent
- tidak boros context
- tidak menduplikasi aturan
- contoh command yang benar untuk project C# ini.
```

---

# 11. In-Progress / Beta Skills

> Upstream secara eksplisit menyebut bucket ini **Beta**. Skill dapat berubah atau hilang tanpa warning, tidak masuk bundle/plugin stabil, dan harus dipasang secara direct.


## `loop-me`

**Invocation:** User-invoked  
**Status:** Beta  
**Fungsi:** Mengembangkan workflow spec yang implementable secara bertahap lintas beberapa sesi dengan directory sebagai stateful workspace.

**Untuk workflow Anda:** Berguna untuk merancang proses kerja yang kompleks; bukan pilihan utama untuk ticket coding biasa.

**Contoh:**

```text
Gunakan skill loop-me.
Saya ingin merancang workflow berulang untuk [TUJUAN].
Bantu saya mematangkannya lintas sesi sampai cukup konkret untuk dijalankan.
```

---


## `writing-beats`

**Invocation:** User-invoked  
**Status:** Beta  
**Fungsi:** Membentuk artikel sebagai perjalanan beat demi beat, memilih beat berikutnya secara iteratif.

**Untuk workflow Anda:** Tidak relevan langsung ke Unity/.NET coding; berguna untuk artikel/devlog teknis.

**Contoh:**

```text
Gunakan skill writing-beats untuk menyusun artikel teknis tentang [TOPIK].
Bangun artikel beat demi beat dan jangan menulis semuanya sekaligus.
```

---


## `writing-fragments`

**Invocation:** User-invoked  
**Status:** Beta  
**Fungsi:** Menggali potongan ide/nugget tulisan dan menyimpannya sebagai raw material sebuah artikel.

**Untuk workflow Anda:** Cocok untuk mengumpulkan catatan devlog/tutorial; bukan engineering flow.

**Contoh:**

```text
Gunakan skill writing-fragments.
Interview saya untuk mengumpulkan fragmen tulisan tentang [TOPIK] dan simpan sebagai bahan mentah.
```

---


## `writing-shape`

**Invocation:** User-invoked  
**Status:** Beta  
**Fungsi:** Mengubah Markdown berisi raw material menjadi artikel, paragraf demi paragraf sambil menilai pilihan format.

**Untuk workflow Anda:** Cocok untuk menyusun artikel dari catatan; bukan untuk implementasi kode.

**Contoh:**

```text
Gunakan skill writing-shape pada file [FILE.md].
Bentuk raw material menjadi artikel secara bertahap dan jelaskan keputusan struktur yang penting.
```

---


## `claude-handoff`

**Invocation:** User-invoked  
**Status:** Beta / Claude-specific  
**Fungsi:** Menyerahkan context ke background agent Claude baru melalui `claude --bg`.

**Untuk workflow Anda:** Tidak cocok untuk Codex/Antigravity secara langsung; gunakan `handoff` yang stabil dan harness-neutral.

**Contoh:**

```text
Skill ini Claude-specific. Untuk Codex/Antigravity, gunakan skill handoff sebagai gantinya.
```

---


## `setup-ts-deep-modules`

**Invocation:** User-invoked  
**Status:** Beta / TypeScript  
**Fungsi:** Memasang dependency-cruiser agar package TypeScript berperilaku sebagai deep modules dengan entry-point yang jelas.

**Untuk workflow Anda:** Tidak untuk Unity/.NET backend. Dapat relevan bila repo Anda juga memiliki React/TypeScript frontend yang besar.

**Contoh:**

```text
Gunakan skill setup-ts-deep-modules pada workspace TypeScript ini.
Jangan menyentuh project .NET/Unity.
Konfigurasikan boundary package dan validasi dependency sesuai skill.
```

---


## `implement-spec`

**Invocation:** User-invoked  
**Status:** Beta  
**Fungsi:** Mengimplementasikan seluruh spec pada satu branch sebagai task graph dan dapat menjalankan implementer subagents pada ready frontier, lalu menghasilkan satu PR.

**Untuk workflow Anda:** Powerful tetapi lebih agresif daripada workflow satu-ticket-per-sesi. Untuk project pribadi Anda, gunakan setelah workflow stabil dan spec/ticket benar-benar kuat.

**Contoh:**

```text
Gunakan skill implement-spec untuk specification [SPEC].
Kerjakan task graph sesuai blocking edges.
Jaga semua pekerjaan pada branch ini dan jangan mengubah scope spec.
Catatan: skill ini beta; laporkan setiap asumsi atau konflik.
```

---


## `retro`

**Invocation:** User-invoked  
**Status:** Beta / STUB  
**Fungsi:** Dirancang untuk menyarankan perbaikan environment coding agent setelah sesi: steering files, standards, checks, tooling.

**Untuk workflow Anda:** README upstream menyatakan masih STUB/design notes dan belum functional; jangan jadikan workflow produksi.

**Contoh:**

```text
Jangan bergantung pada retro untuk workflow produksi saat ini; status upstream masih STUB.
```

---


# 12. Misc Skills

Skill berikut disimpan upstream sebagai tool yang jarang dipakai dan tidak dipromosikan dalam plugin utama.


## `git-guardrails-claude-code`

**Jenis:** Claude Code-specific  
**Fungsi:** Memasang hook Claude Code untuk memblokir command Git berbahaya seperti push/reset --hard/clean sebelum dieksekusi.

**Catatan untuk Anda:** Tidak portable ke Codex/Antigravity tanpa adaptasi. Konsepnya bagus, implementasinya khusus Claude Code.

---


## `migrate-to-shoehorn`

**Jenis:** TypeScript-specific  
**Fungsi:** Migrasi test TypeScript dari `as` type assertions ke `@total-typescript/shoehorn`.

**Catatan untuk Anda:** Tidak relevan untuk Unity/.NET.

---


## `scaffold-exercises`

**Jenis:** General learning  
**Fungsi:** Membuat struktur exercise dengan sections, problems, solutions, dan explainers.

**Catatan untuk Anda:** Bisa dipakai untuk membuat latihan C#/Unity/.NET jika Anda sedang belajar/mengajar.

---


## `setup-pre-commit`

**Jenis:** Node/Husky-specific  
**Fungsi:** Setup Husky pre-commit dengan lint-staged, Prettier, type checking, dan tests.

**Catatan untuk Anda:** Cocok untuk frontend React/TS. Untuk .NET/Unity, konsep pre-commit bisa diadaptasi tetapi skill ini sendiri berorientasi Node/Husky.

---


# 13. Workflow Rekomendasi untuk ASP.NET Core / .NET Backend

## A. Fitur Baru Normal

Misalnya:

```text
Add recurring transaction
Add authentication
Add transaction filtering
Add pagination
Add budget rules
```

Flow:

```text
setup (sekali)
   ↓
grill-with-docs
   ↓
to-spec
   ↓
to-tickets
   ↓
fresh context
   ↓
implement ticket #1
   ↓
commit
   ↓
fresh context
   ↓
implement ticket #2
```

Prompt awal:

```text
Gunakan skill grill-with-docs.

Saya ingin menambahkan fitur recurring transaction pada ASP.NET Core .NET 8.

Requirement awal:
- user dapat membuat recurrence harian/mingguan/bulanan
- recurrence memiliki start date
- recurrence dapat dinonaktifkan
- generated transaction tidak boleh duplicate

Interview saya sampai domain rule dan edge case cukup jelas.
Jangan implementasi dulu.
```

Setelah matang:

```text
Gunakan skill to-spec.
Ubah diskusi kita menjadi specification.
Jangan implementasi.
```

Lalu:

```text
Gunakan skill to-tickets.
Pecah specification menjadi tracer-bullet tickets dengan blocking edges.
Jangan implementasi.
```

Implementasi:

```text
Gunakan skill implement.
Implementasikan ticket #4 saja.
Jalankan dotnet test yang relevan.
Jangan mengerjakan ticket lain.
```

## B. Bug .NET yang Sulit

Contoh:

```text
EF Core menghasilkan duplicate row
Total balance salah setelah update
Race condition pada stock reservation
Integration test flaky
```

Gunakan:

```text
diagnosing-bugs
```

Prompt:

```text
Gunakan skill diagnosing-bugs.

Bug:
Balance bulanan salah setelah transaction diedit dua kali.

Expected:
Balance mencerminkan nilai terakhir tepat satu kali.

Actual:
Nilai lama tampaknya masih ikut dihitung.

Bangun reproduksi yang dapat dijalankan dengan dotnet test.
Jangan membuat fix spekulatif.
Temukan root cause, tambahkan regression test, lalu fix.
```

Jika root cause sudah diketahui jelas:

```text
Gunakan skill tdd.
Buat regression test untuk bug ini terlebih dahulu, lalu lakukan minimum fix.
```

## C. Refactor Layered Architecture

Jangan langsung:

```text
Refactor project ini agar clean architecture.
```

Lebih baik:

```text
improve-codebase-architecture
      ↓
codebase-design
      ↓
grill-with-docs
      ↓
to-spec
      ↓
to-tickets
      ↓
implement
```

Alasannya: “Clean Architecture” sering membuat agent menambah banyak interface/folder tanpa memperbaiki information hiding. `codebase-design` lebih fokus pada seam, depth, dan interface yang benar-benar memberi leverage.

---

# 14. Workflow Rekomendasi untuk Unity

## A. Gameplay Feature Baru

Contoh:

```text
Joker effect system
Inventory
Quest system
Typing mechanic
Boss debuff
Save/load
Shop state
Card scoring
```

Flow:

```text
grill-with-docs
      ↓
(domain-modeling bila vocabulary kompleks)
      ↓
to-spec
      ↓
to-tickets
      ↓
implement satu ticket
```

Prompt:

```text
Gunakan skill grill-with-docs.

Saya ingin menambahkan Joker Effect System pada Unity 6.

Konteks:
- C#
- beberapa Joker mengubah scoring
- beberapa Joker mengubah economy
- beberapa Joker bereaksi terhadap event berbeda

Saya ingin menghindari satu class besar berisi switch untuk semua joker.
Interview saya tentang rule, lifecycle, stacking, ordering, dan edge cases.
Jangan implementasi dulu.
```

Jika istilah seperti berikut mulai tumpang tindih:

```text
Effect
Trigger
Modifier
Scoring Modifier
Round Modifier
Passive Effect
```

panggil:

```text
Gunakan skill domain-modeling.
```

## B. Testing pada Unity

TDD sangat efektif jika seam Anda berupa pure C#.

Prioritas umum:

```text
Domain/game logic murni
  → EditMode tests

GameObject / MonoBehaviour lifecycle
  → PlayMode tests bila benar-benar perlu

Visual/animation feel
  → jangan memaksa semua hal menjadi unit test
```

Prompt:

```text
Gunakan skill tdd.

Behavior:
The Psychic boss mewajibkan tepat 5 kartu dimainkan.

Buat test behavior pada layer game logic yang tidak bergantung scene jika memungkinkan.
Jangan membuat test yang hanya memverifikasi implementation detail.
```

## C. Bug Unity

Contoh:

```text
Joker berubah setelah membuka booster
Tarot yang dipilih berbeda dari yang masuk container
Boss debuff tetap aktif setelah round selesai
Save/load menggandakan item
```

Prompt:

```text
Gunakan skill diagnosing-bugs.

Bug:
Setelah booster pack dibuka, Joker di container tiba-tiba berubah.

Expected:
Membuka booster tidak mengubah daftar Joker yang sudah dimiliki.

Actual:
Satu atau lebih Joker berubah setelah state booster dibuka.

Bangun reproduksi sekecil mungkin.
Telusuri state ownership, shared references, ScriptableObject mutation, dan generation path berdasarkan bukti.
Jangan langsung mengubah kode.
```

`diagnosing-bugs` sangat berguna pada Unity karena bug sering berasal dari state yang tidak terlihat jelas:

```text
ScriptableObject shared state
static state
singleton lifetime
scene reload
serialized reference
event subscription
object pooling
random seed/state
async/coroutine ordering
```

Agent tetap harus membuktikan root cause, bukan mengasumsikan salah satu daftar di atas.

## D. Architecture Unity yang Mulai “Manager Everywhere”

Flow:

```text
improve-codebase-architecture
       ↓
 pilih kandidat
       ↓
codebase-design
       ↓
grill-with-docs
       ↓
to-spec
```

Prompt:

```text
Gunakan skill improve-codebase-architecture.

Audit Assets/Scripts untuk mencari area yang terlalu bergantung pada singleton/manager,
interface yang bocor, state ownership yang tidak jelas, dan module yang sulit dites.

Jangan refactor dulu.
Tampilkan kandidat paling bernilai beserta alasannya.
```

## E. Prototype untuk Unity

Perlu memahami batas skill upstream:

```text
prototype
```

cocok untuk:

```text
state flow
menu flow
rule logic
UI concept
alternative layouts
```

Tetapi **bukan validator yang cukup** untuk:

```text
Unity Physics
Animator transitions
Input System timing
rendering/URP
actual frame performance
scene lifecycle
Addressables behavior
```

Jika pertanyaan bergantung engine, buat sandbox nyata di Unity secara terpisah dan gunakan hasilnya sebagai primary source untuk kembali ke spec.

---

# 15. Workflow Rider ↔ Codex ↔ Antigravity

Anda dapat memakai ketiganya tanpa konflik selama Git tetap menjadi sumber checkpoint.

Contoh:

```text
Rider
  ↓
manual inspect / debugger
  ↓
Codex + implement
  ↓
git diff / tests
  ↓
handoff
  ↓
Antigravity untuk task lain / review
  ↓
Rider untuk inspect hasil
```

## Aturan Aman

Jangan biarkan dua agent menulis file yang sama pada waktu bersamaan tanpa branch/worktree terpisah.

Sebelum pindah agent:

```bash
git status
git diff
```

Jika pekerjaan cukup matang:

```bash
git add <file-terkait>
git commit -m "feat: ..."
```

Jika belum siap commit, buat handoff:

```text
Gunakan skill handoff.
Buat handoff pekerjaan saat ini agar saya dapat melanjutkannya di Antigravity.
```

Lalu agent baru harus mulai dengan:

```text
Baca handoff ini dan inspect repository state sebelum membuat perubahan.
Periksa git status dan git diff.
Jangan mengulang implementasi yang sudah ada.
```

---

# 16. GitHub Issues Workflow

Jika setup memilih GitHub:

```text
Requirement
   ↓
to-spec
   ↓
GitHub issue/spec
   ↓
to-tickets
   ↓
GitHub issues + blocking edges
   ↓
implement issue
   ↓
review + tests
   ↓
close
```

## Incoming Issue vs Ticket Buatan Anda

Penting:

```text
Issue dari user / bug report eksternal
    → triage

Ticket hasil to-tickets
    → JANGAN triage lagi
    → sudah agent-ready
```

## Prompt Melihat Pekerjaan Berikutnya

```text
Inspect issue tracker dan repository state.

Tunjukkan:
- ticket completed
- ticket in progress
- ticket yang blocked
- ticket ready-for-agent
- dependency/blocking edges
- current git status

Jangan ubah apa pun.
```

## Setelah Ticket Selesai

```text
Verifikasi acceptance criteria ticket ini dan jalankan test relevan.
Jika implementasi memang lengkap, tutup ticket tersebut.
Lalu tampilkan ticket berikutnya yang tidak blocked.
Jangan mulai ticket berikutnya.
```

---

# 17. `CONTEXT.md` dan ADR untuk Unity/.NET

Salah satu fitur paling bernilai dari flow ini bukan sekadar ticket, tetapi **shared vocabulary**.

## CONTEXT.md

Isi yang bagus:

```markdown
# Domain Language

## Blind
Target score untuk satu round.

## Boss Blind
Blind dengan rule modifier tambahan.

## Debuff
Rule sementara yang memodifikasi behavior pemain selama Blind aktif.
```

Jangan jadikan `CONTEXT.md` sebagai dump semua detail implementation.

Tujuannya agar:

```text
User
Agent
Class names
Method names
Ticket
Spec
```

menggunakan istilah yang sama.

## ADR

ADR cocok untuk keputusan yang mahal untuk dibalik.

Contoh .NET:

```text
ADR: Transaction amount disimpan sebagai decimal, bukan double.
```

Contoh Unity:

```text
ADR: Joker definitions immutable sebagai ScriptableObject data,
sedangkan runtime mutable state berada pada runtime instance terpisah.
```

Ini mencegah agent di sesi berikutnya “menemukan ulang” keputusan yang sama.

---

# 18. Cara Memilih Skill dengan Cepat

```text
Saya bingung skill apa
    ↓
ask-matt
```

```text
Ide belum matang di dalam repo
    ↓
grill-with-docs
```

```text
Ide belum matang dan belum ada repo
    ↓
grill-me
```

```text
Diskusi sudah matang
    ↓
to-spec
```

```text
Spec terlalu besar
    ↓
to-tickets
```

```text
Ticket sudah siap
    ↓
implement
```

```text
Behavior kecil dan jelas, ingin test-first
    ↓
tdd
```

```text
Bug sulit / root cause belum jelas
    ↓
diagnosing-bugs
```

```text
Root cause bug sudah terbukti
    ↓
tdd + regression test
```

```text
Ingin review implementasi
    ↓
code-review
```

```text
Codebase mulai berantakan
    ↓
improve-codebase-architecture
```

```text
Sudah tahu module mana yang perlu didesain
    ↓
codebase-design
```

```text
Istilah domain rancu
    ↓
domain-modeling
```

```text
Project/feature terlalu besar untuk satu sesi
    ↓
wayfinder
```

```text
Perlu validasi design question dengan prototype
    ↓
prototype
```

```text
Perlu membaca docs/sumber resmi dulu
    ↓
research
```

```text
Pindah Codex ↔ Antigravity / sesi baru
    ↓
handoff
```

```text
Sedang merge conflict
    ↓
resolving-merge-conflicts
```

```text
Agent berhenti karena langkah yang hanya manusia bisa lakukan
    ↓
wizard
```

```text
Penjelasan agent bikin bingung
    ↓
wait-what
```

---

# 19. Model dan Reasoning untuk Codex

Model yang tersedia dapat berubah menurut plan/rollout/versi Codex. Gunakan model picker Codex untuk melihat pilihan aktual pada akun Anda.

Keluarga GPT-5.6 saat dokumen ini diverifikasi:

| Model | Posisi umum | Cocok untuk |
|---|---|---|
| **GPT-5.6 Sol** | Flagship | architecture, debugging sulit, planning besar, review penting |
| **GPT-5.6 Terra** | Balance intelligence/cost | coding harian, ticket normal, TDD normal |
| **GPT-5.6 Luna** | Cost-sensitive/high-volume | task mekanis, rename, DTO, enum, perubahan kecil |

GPT-5.6 mendukung reasoning effort:

```text
none
low
medium
high
xhigh
max
```

Ketersediaan opsi pada Codex UI dapat berbeda dari API.

## Rekomendasi Praktis

| Pekerjaan | Model | Reasoning |
|---|---|---|
| `ask-matt` normal | Terra / Sol | Medium |
| `grill-with-docs` | Sol | Medium |
| `to-spec` | Sol | Medium |
| `to-tickets` | Terra | Medium |
| Ticket sangat mekanis | Luna | Low / Medium |
| Ticket normal | Terra | Medium |
| Ticket lintas layer | Terra / Sol | Medium |
| `tdd` normal | Terra | Medium |
| Bug sederhana | Terra | Medium |
| `diagnosing-bugs` sulit | Sol | High |
| `domain-modeling` kompleks | Sol | Medium / High |
| `codebase-design` | Sol | Medium / High |
| `improve-codebase-architecture` | Sol | High |
| `wayfinder` | Sol | High |
| `code-review` normal | Terra | Medium |
| `code-review` critical | Sol | High |
| `research` | Terra / Sol | Medium |

Aturan sederhana:

```text
Mekanis dan scope kecil
  → Luna + Low/Medium

Coding sehari-hari
  → Terra + Medium

Ambigu / architecture / debugging sulit
  → Sol + Medium/High

Masih benar-benar sulit
  → naikkan reasoning setelah baseline gagal
```

Jangan memakai `max` hanya karena ingin “hasil paling bagus”; feedback loop, test, dan scope yang bersih biasanya lebih bernilai daripada sekadar reasoning lebih tinggi.

---

# 20. Prompt Template Cepat

## A. Start Sesi

```text
Inspect repository sebelum membuat perubahan.

Periksa:
- git status
- git diff
- CONTEXT.md / domain docs yang relevan
- issue tracker / ticket aktif
- test yang relevan

Ringkas current state.
Jangan ubah apa pun.
```

## B. Feature Baru

```text
Gunakan skill grill-with-docs.

Saya ingin membuat [FITUR].

Stack:
- [Unity 6 / .NET 8]

Requirement awal:
- ...
- ...

Interview saya sampai scope, terminology, edge cases, dan keputusan penting cukup jelas.
Jangan implementasi dulu.
```

## C. Spec

```text
Gunakan skill to-spec.

Ubah requirement yang sudah kita sepakati menjadi specification.
Jangan implementasi.
```

## D. Tickets

```text
Gunakan skill to-tickets.

Pecah specification menjadi tracer-bullet tickets.
Setiap ticket harus punya acceptance criteria dan blocking edges yang jelas.
Jangan implementasi.
```

## E. Implement Satu Ticket

```text
Gunakan skill implement.

Implementasikan ticket #[NUMBER] saja.
Ikuti specification, acceptance criteria, dan project conventions.
Jalankan test yang relevan.
Jangan mengerjakan ticket lain.
```

## F. Bug Diagnosis

```text
Gunakan skill diagnosing-bugs.

Bug:
[BUG]

Expected:
[EXPECTED]

Actual:
[ACTUAL]

Bangun feedback loop yang mereproduksi bug ini.
Minimalkan kasus dan buktikan root cause sebelum fix.
Tambahkan regression test.
Jangan membuat perubahan spekulatif.
```

## G. Review

```text
Gunakan skill code-review.

Review diff ticket #[NUMBER].
Nilai Standards dan Spec compliance secara terpisah.
Laporkan findings dulu.
Jangan ubah kode.
```

## H. Pindah Agent

```text
Gunakan skill handoff.

Buat handoff lengkap agar saya bisa melanjutkan task ini di agent/session lain.
Sertakan keputusan, state repo, test status, unfinished work, dan next action.
```

## I. Audit Architecture

```text
Gunakan skill improve-codebase-architecture.

Cari deepening opportunities pada repository ini.
Jangan refactor.
Tampilkan kandidat dan prioritasnya dulu.
```

---

# 21. Setelah Laptop Crash / Agent Terputus

Perubahan yang sudah ditulis ke disk tetap berada di repo.

Setelah membuka sesi baru:

```text
Inspect repository sebelum membuat perubahan.

Periksa:
- git status
- git diff
- current ticket
- relevant spec
- CONTEXT.md
- test status
- partially implemented code

Tentukan di mana pekerjaan sebelumnya berhenti.
Jangan restart dari awal.
```

Setelah state jelas:

```text
Lanjutkan ticket yang sedang aktif dari state repository saat ini.
Jangan mengulang implementasi yang sudah ada.
```

---

# 22. Git Sebagai Checkpoint

Sebelum coding:

```bash
git status
```

Setelah satu ticket benar-benar selesai:

```bash
git add <file-yang-terkait>
git commit -m "feat: implement ..."
```

Ideal:

```text
Ticket #1
  ↓
tests
  ↓
review
  ↓
commit

Ticket #2
  ↓
tests
  ↓
review
  ↓
commit
```

Hindari menunggu banyak ticket sebelum membuat checkpoint.

Untuk project Unity, jangan asal `git add .` jika repository memiliki generated/cache files yang belum di-ignore. Pastikan `.gitignore` Unity benar.

---

# 23. Skill yang Paling Bernilai untuk Stack Anda

Jika Anda tidak ingin memasang semuanya, shortlist untuk **Unity + .NET Backend + Codex/Antigravity**:

## Tier 1 — Wajib

```text
setup-matt-pocock-skills
ask-matt
grill-with-docs
to-spec
to-tickets
implement
tdd
diagnosing-bugs
code-review
handoff
```

## Tier 2 — Sangat Berguna

```text
domain-modeling
codebase-design
improve-codebase-architecture
research
resolving-merge-conflicts
```

## Tier 3 — Situasional

```text
triage
wayfinder
prototype
wizard
teach
to-questionnaire
wait-what
writing-for-agents
```

## Jangan Diprioritaskan untuk Unity/.NET

```text
setup-ts-deep-modules
migrate-to-shoehorn
setup-pre-commit
```

kecuali bagian React/TypeScript pada repo Anda memang membutuhkannya.

Claude-specific:

```text
claude-handoff
git-guardrails-claude-code
```

---

# 24. Rekomendasi Workflow Default Anda

Untuk mayoritas pekerjaan:

```text
           ┌──────────────────┐
           │ Ada requirement? │
           └────────┬─────────┘
                    ↓
           grill-with-docs
                    ↓
              to-spec
                    ↓
             to-tickets
                    ↓
          implement ONE ticket
             ↙          ↘
           tdd       code-review
             \          /
               tests
                 ↓
               commit
                 ↓
            next ticket
```

Untuk bug:

```text
Bug
 ↓
diagnosing-bugs
 ↓
root cause terbukti
 ↓
regression test / tdd
 ↓
fix
 ↓
code-review
 ↓
commit
```

Untuk arsitektur:

```text
Codebase mulai sulit
 ↓
improve-codebase-architecture
 ↓
pilih kandidat
 ↓
codebase-design
 ↓
grill-with-docs
 ↓
to-spec
 ↓
to-tickets
 ↓
implement
```

Untuk pekerjaan yang sangat besar:

```text
Foggy / huge effort
 ↓
wayfinder
 ↓
decisions jelas
 ↓
to-spec
 ↓
to-tickets
 ↓
implement
```

---

# 25. Hal yang Sebaiknya Tidak Dilakukan

## Jangan langsung coding dari requirement kabur

Kurang baik:

```text
Buat inventory system yang bagus.
```

Lebih baik:

```text
Gunakan skill grill-with-docs.
Saya ingin membuat inventory system...
```

## Jangan memakai `triage` pada ticket hasil `to-tickets`

Ticket tersebut sudah dibuat secara sengaja untuk agent.

## Jangan memakai `wayfinder` untuk CRUD kecil

Overhead-nya terlalu besar.

## Jangan meminta `prototype` menjawab hal yang hanya dapat divalidasi engine Unity

HTML prototype tidak membuktikan Physics/Animator/Input/URP behavior.

## Jangan menganggap TDD berarti semua code harus unit-tested

Yang penting adalah feedback loop pada seam yang memberi nilai.

## Jangan membiarkan dua agent mengedit working tree yang sama secara bersamaan

Gunakan:

```text
branch
worktree
commit
handoff
```

untuk memisahkan pekerjaan.

## Jangan mengandalkan skill Beta untuk workflow kritis tanpa inspeksi

Terutama:

```text
implement-spec
retro
```

`retro` saat ini masih STUB menurut upstream.

---

# 26. Referensi

## Matt Pocock Skills

- Repository: https://github.com/mattpocock/skills
- Stable engineering catalog: https://github.com/mattpocock/skills/tree/main/skills/engineering
- Stable productivity catalog: https://github.com/mattpocock/skills/tree/main/skills/productivity
- In-progress/Beta: https://github.com/mattpocock/skills/tree/main/skills/in-progress
- Misc: https://github.com/mattpocock/skills/tree/main/skills/misc

## Antigravity

- Agent Skills documentation: https://www.antigravity.google/docs/ide/skills/

## OpenAI

- Models: https://developers.openai.com/api/docs/models
- Codex: https://developers.openai.com/codex

---

# 27. Cheat Sheet Satu Layar

| Situasi | Skill |
|---|---|
| Tidak tahu skill mana | `ask-matt` |
| Setup repo | `setup-matt-pocock-skills` |
| Requirement belum matang di repo | `grill-with-docs` |
| Requirement belum matang di luar repo | `grill-me` |
| Istilah domain rancu | `domain-modeling` |
| Diskusi → spec | `to-spec` |
| Spec → ticket | `to-tickets` |
| Implement ticket | `implement` |
| Test-first | `tdd` |
| Bug sulit | `diagnosing-bugs` |
| Review | `code-review` |
| Survey arsitektur | `improve-codebase-architecture` |
| Desain module/seam | `codebase-design` |
| Project sangat besar/berkabut | `wayfinder` |
| Design question perlu prototype | `prototype` |
| Riset docs resmi | `research` |
| Incoming issue | `triage` |
| Merge conflict | `resolving-merge-conflicts` |
| Human-only setup | `wizard` |
| Pindah agent/session | `handoff` |
| Tidak paham penjelasan | `wait-what` |
| Belajar terstruktur | `teach` |
| Butuh jawaban stakeholder | `to-questionnaire` |
| Menulis AGENTS.md/SKILL.md | `writing-for-agents` |

**Default utama untuk Anda:**

```text
Unity / .NET feature
→ grill-with-docs
→ to-spec
→ to-tickets
→ implement satu ticket
→ commit
```

**Default bug:**

```text
diagnosing-bugs
→ regression test
→ fix
→ review
```

**Default pindah Codex ↔ Antigravity:**

```text
handoff
→ git status/diff
→ lanjut dari state repo
```