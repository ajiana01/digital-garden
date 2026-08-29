# Workflow Matt Pocock Skills + Codex CLI

Panduan praktis menggunakan repository **mattpocock/skills** bersama **Codex CLI** untuk membuat fitur, mengerjakan ticket, debugging, testing, dan code review.

> Fokus panduan ini adalah penggunaan sehari-hari, bukan penjelasan teori panjang.

---

## 1. Konsep Singkat

Tanpa skills, workflow biasanya seperti:

```text
Prompt
  ↓
Codex membaca kode
  ↓
Codex langsung implement
```

Dengan `mattpocock/skills`:

```text
Requirement
    ↓
grill-with-docs
    ↓
to-spec
    ↓
to-tickets
    ↓
implement
    ↓
tdd
    ↓
code-review
```

Tujuannya supaya Codex tidak langsung coding sebelum requirement dan scope pekerjaan jelas.

---

# 2. Setup Pertama Kali

Install skills dari root repository:

```bash
npx skills@latest add mattpocock/skills
```

Masuk ke Codex:

```bash
codex
```

Lalu setup repository sekali saja:

```text
Use the setup-matt-pocock-skills skill to configure this repository.
```

Pada setup, tentukan issue tracker yang digunakan.

Contoh:

```text
GitHub.

I plan to publish this repository on GitHub.
Use GitHub Issues for issue tracking.
```

Atau kalau masih project lokal:

```text
Use local markdown for issue tracking.
```

---

# 3. Cara Memanggil Skill di Codex CLI

Pada setup Codex saya, skill tidak harus muncul sebagai slash command.

Gunakan natural-language prompt:

```text
Use the grill-with-docs skill.
```

```text
Use the to-spec skill.
```

```text
Use the to-tickets skill.
```

```text
Use the implement skill.
```

Untuk skill lain:

```text
Use the diagnosing-bugs skill.
```

```text
Use the code-review skill.
```

```text
Use the tdd skill.
```

Jika versi Codex Anda menampilkan skill sebagai slash command, slash command juga dapat digunakan. Namun natural-language invocation adalah pilihan aman.

---

# 4. Workflow Membuat Project / Fitur Baru

Contoh project:

```text
PocketLedger

React + TypeScript
.NET 8 Web API
PostgreSQL
Entity Framework Core
```

Misalnya ingin membuat fitur:

```text
Transaction Management
```

Workflow:

```text
Ide
 ↓
grill-with-docs
 ↓
to-spec
 ↓
to-tickets
 ↓
implement ticket #1
 ↓
test
 ↓
code-review
 ↓
commit
 ↓
ticket #2
```

---

## STEP 1 — Matangkan Requirement

Gunakan:

```text
Use the grill-with-docs skill.

I want to build a Transaction feature for PocketLedger.

Initial requirements:
- User can add income.
- User can add expense.
- Transaction has amount, category, date, and description.
- Amount must be greater than zero.
- User can edit and delete transactions.

Tech stack:
- ASP.NET Core .NET 8
- React + TypeScript
- PostgreSQL
- Entity Framework Core

Help me clarify the requirements before implementation.
Do not implement anything yet.
```

Codex akan menanyakan hal-hal yang belum jelas.

Contoh:

```text
Should a transaction category be mandatory?

Can transactions from previous months be edited?

Should deleting a category also delete transactions?
```

Jawab sampai requirement sudah jelas.

### Model yang disarankan

```text
GPT-5.6 Sol
Reasoning: Medium
```

Jika domain/arsitektur kompleks:

```text
GPT-5.6 Sol
Reasoning: High
```

---

## STEP 2 — Buat Specification

Setelah diskusi requirement selesai:

```text
Use the to-spec skill.

Turn the requirements we agreed on into a specification.
Do not start implementation.
```

Spec menjadi sumber kebenaran untuk implementasi.

### Model

```text
GPT-5.6 Sol
Reasoning: Medium
```

---

## STEP 3 — Pecah Menjadi Tickets

Setelah spec selesai:

```text
Use the to-tickets skill.

Break the specification into small implementation tickets.

Each ticket should:
- have a clear scope
- have acceptance criteria
- declare dependencies
- be independently testable where possible

Do not implement the tickets yet.
```

Contoh hasil:

```text
#1 Setup Transaction domain model

#2 Add Transaction validation

#3 Implement Create Transaction service

#4 Add POST /api/transactions

#5 Implement transaction unit tests

#6 Create React transaction form

#7 Integrate React form with API
```

### Model

Pilihan hemat:

```text
GPT-5.6 Terra
Reasoning: Medium
```

Untuk project kompleks:

```text
GPT-5.6 Sol
Reasoning: Medium
```

---

# 5. Melihat Ticket yang Belum / Sudah Dikerjakan

Gunakan prompt:

```text
Show me all current tickets and their implementation status.

Group them into:
- Completed
- In Progress
- Blocked
- Not Started

Do not modify anything.
```

Contoh:

```text
Completed
✓ #1 Setup Transaction domain model
✓ #2 Add Transaction validation

In Progress
→ #3 Implement Create Transaction service

Not Started
○ #4 POST /api/transactions
○ #5 Unit tests
○ #6 React form
○ #7 API integration
```

Jika menggunakan GitHub Issues:

```text
Open   = belum selesai
Closed = sudah selesai
```

---

# 6. Implementasi Ticket

Disarankan mengerjakan **satu ticket dalam satu waktu**.

Contoh:

```text
Use the implement skill.

Implement ticket #3 only.

Follow:
- the specification
- the ticket acceptance criteria
- existing project conventions

Use TDD where appropriate.

Run the relevant tests after implementation.

Do not work on other tickets.
```

Setelah selesai:

```text
Verify that ticket #3 satisfies all acceptance criteria.

If everything passes:
- mark the ticket completed
- show me the remaining tickets

Do not start the next ticket yet.
```

---

# 7. Model untuk Implementasi Ticket

## Ticket Sangat Sederhana

Contoh:

```text
Create DTO
Create enum
Rename property
Add simple validation
Create basic CRUD endpoint
```

Gunakan:

```text
GPT-5.6 Luna
Reasoning: Low
```

atau kalau ingin sedikit lebih aman:

```text
GPT-5.6 Luna
Reasoning: Medium
```

---

## Ticket Normal

Contoh:

```text
Implement POST /transactions
Implement service
Add EF Core repository
Create React form
Integrate frontend with API
```

Rekomendasi utama:

```text
GPT-5.6 Terra
Reasoning: Medium
```

Ini adalah pilihan default yang bagus untuk coding harian.

---

## Ticket Sulit

Contoh:

```text
Complex EF Core relationships
Refactoring several modules
Complex state management
Concurrency
Transaction boundaries
Complex domain rules
```

Gunakan:

```text
GPT-5.6 Sol
Reasoning: Medium
```

Jika masih sulit:

```text
GPT-5.6 Sol
Reasoning: High
```

---

# 8. Workflow Jika Menemukan Bug

Jangan langsung:

```text
Fix this bug.
```

Gunakan `diagnosing-bugs`.

Contoh kasus:

```text
Monthly budget becomes incorrect after editing a transaction.
```

Prompt:

```text
Use the diagnosing-bugs skill.

Bug:
Monthly budget becomes incorrect after editing an expense transaction.

Expected:
The monthly total should reflect the updated transaction amount exactly once.

Actual:
The old amount appears to remain included in the total.

First reproduce and identify the root cause.

Do not make speculative changes.
Add a regression test before or as part of the fix where appropriate.
```

Workflow:

```text
Bug
 ↓
Reproduce
 ↓
Minimise
 ↓
Hypothesis
 ↓
Inspect / Instrument
 ↓
Root cause
 ↓
Regression test
 ↓
Fix
 ↓
Test
```

### Model

Bug sederhana:

```text
GPT-5.6 Terra
Reasoning: Medium
```

Bug sulit:

```text
GPT-5.6 Sol
Reasoning: High
```

---

# 9. Jika Bug Sudah Jelas dan Hanya Perlu Fix

Kalau root cause sudah diketahui:

```text
Use the tdd skill.

Fix this bug using a regression test first.

Root cause:
TransactionService adds the updated amount but does not remove
the previous amount from the monthly total.

Expected:
Editing a transaction must recalculate the monthly total correctly.
```

Kemudian:

```text
RED
 ↓
Test gagal
 ↓
GREEN
 ↓
Implement minimum fix
 ↓
REFACTOR
 ↓
Test tetap pass
```

---

# 10. Code Review

Setelah fitur selesai:

```text
Use the code-review skill.

Review the implementation of ticket #3.

Check:
- specification compliance
- acceptance criteria
- potential bugs
- code quality
- unnecessary complexity
- test coverage
- project conventions

Do not modify code yet.
Report findings first.
```

Jika review menemukan masalah:

```text
Fix only the issues identified in the code review.

Run the relevant tests afterward.
```

### Model

Review normal:

```text
GPT-5.6 Terra
Reasoning: Medium
```

Review fitur penting:

```text
GPT-5.6 Sol
Reasoning: Medium / High
```

---

# 11. Model & Reasoning Cheat Sheet

| Pekerjaan | Model | Reasoning |
|---|---|---|
| Setup sederhana | Terra | Low / Medium |
| `grill-with-docs` | Sol | Medium |
| Domain kompleks | Sol | High |
| `to-spec` | Sol | Medium |
| `to-tickets` | Terra | Medium |
| DTO / enum / rename | Luna | Low |
| CRUD sederhana | Luna / Terra | Medium |
| Ticket normal | **Terra** | **Medium** |
| React component normal | Terra | Medium |
| React ↔ API integration | Terra | Medium |
| EF Core normal | Terra | Medium |
| TDD sederhana | Terra | Medium |
| Bug sederhana | Terra | Medium |
| Bug kompleks | Sol | High |
| Architecture | Sol | High |
| Code review normal | Terra | Medium |
| Code review penting | Sol | Medium / High |
| Refactor besar | Sol | High |

---

# 12. Cara Memilih Model Secara Sederhana

Gunakan aturan berikut:

```text
Apakah task sangat jelas dan mekanis?
          │
        YES
          ↓
    Luna + Low
```

```text
Apakah task coding sehari-hari?
          │
        YES
          ↓
   Terra + Medium
```

```text
Apakah task ambigu / kompleks / berisiko?
          │
        YES
          ↓
    Sol + Medium
          │
      masih sulit?
          ↓
      Sol + High
```

Rekomendasi default:

```text
Coding sehari-hari
→ GPT-5.6 Terra + Medium
```

---

# 13. Reasoning Level

## Low

Cocok untuk:

```text
DTO
enum
rename
simple CRUD
formatting
small test
```

Kelebihan:

```text
lebih cepat
lebih hemat usage
```

---

## Medium

Cocok untuk sebagian besar coding.

```text
service implementation
API endpoint
React component
API integration
EF Core
unit tests
code review
```

Ini adalah default yang disarankan.

---

## High

Gunakan jika memang dibutuhkan:

```text
complex debugging
architecture
large refactor
concurrency
complex business rules
cross-layer bugs
```

Higher reasoning menggunakan lebih banyak token dan biasanya lebih lambat.

Jangan menggunakan `High` hanya karena ingin hasil "lebih bagus".

---

## Extra High / Max / Ultra

Biasanya **tidak diperlukan untuk ticket coding biasa**.

Gunakan hanya untuk masalah yang benar-benar sulit atau pekerjaan besar yang membutuhkan reasoning sangat dalam / pembagian pekerjaan ke subagents.

---

# 14. Mengganti Model di Codex CLI

Di dalam Codex:

```text
/model
```

Kemudian pilih model dan reasoning.

Atau jalankan Codex langsung dengan model tertentu:

```bash
codex -m gpt-5.6-terra
```

Untuk Sol:

```bash
codex -m gpt-5.6-sol
```

Untuk Luna:

```bash
codex -m gpt-5.6-luna
```

---

# 15. Workflow Harian yang Direkomendasikan

Setelah project sudah memiliki spec dan tickets:

```text
Start Codex
   ↓
Check tickets
   ↓
Pick ONE ticket
   ↓
Implement
   ↓
TDD / tests
   ↓
Code review
   ↓
Mark complete
   ↓
Commit
   ↓
Next ticket
```

Prompt awal sesi:

```text
Inspect the current repository state.

Show:
- current specification
- completed tickets
- in-progress ticket
- remaining tickets
- current git status

Do not modify anything.
```

Setelah memilih ticket:

```text
Use the implement skill.

Implement ticket #4 only.

Follow its specification and acceptance criteria.
Use TDD where appropriate.
Run relevant tests.
Do not work on any other ticket.
```

---

# 16. Setelah Laptop Crash / Internet Putus

File yang sudah ditulis ke disk tetap berada di repository.

Setelah membuka Codex kembali:

```text
Inspect the repository before making changes.

Check:
- git status
- git diff
- current specification
- ticket status
- relevant tests
- partially implemented code

Determine where the previous work stopped.

Do not modify anything until you understand the current state.
```

Setelah Codex menjelaskan kondisinya:

```text
Continue the current ticket from its existing state.

Do not restart the implementation from scratch.
```

---

# 17. Gunakan Git Sebagai Checkpoint

Sebelum mulai:

```bash
git status
```

Setelah satu ticket selesai dan lolos test:

```bash
git add .
git commit -m "feat: implement transaction creation"
```

Idealnya:

```text
Ticket #1
 ↓
test
 ↓
review
 ↓
commit

Ticket #2
 ↓
test
 ↓
review
 ↓
commit
```

Jangan menunggu 10 ticket selesai baru commit.

---

# 18. Prompt Templates

## Membuat Fitur Baru

```text
Use the grill-with-docs skill.

I want to build [FEATURE].

Initial requirements:
- ...
- ...
- ...

Tech stack:
- ...

Help me clarify the requirements.
Do not implement anything yet.
```

---

## Membuat Spec

```text
Use the to-spec skill.

Turn our agreed requirements into a specification.
Do not start implementation.
```

---

## Membuat Ticket

```text
Use the to-tickets skill.

Break the specification into small implementation tickets.

Each ticket must have:
- clear scope
- acceptance criteria
- dependencies

Do not implement anything yet.
```

---

## Implement Ticket

```text
Use the implement skill.

Implement ticket #[NUMBER] only.

Follow the specification and acceptance criteria.
Use TDD where appropriate.
Run relevant tests.

Do not implement other tickets.
```

---

## Debug Bug

```text
Use the diagnosing-bugs skill.

Bug:
[DESCRIPTION]

Expected:
[EXPECTED]

Actual:
[ACTUAL]

Reproduce and identify the root cause first.
Do not make speculative changes.

Add a regression test and fix the root cause.
```

---

## Code Review

```text
Use the code-review skill.

Review the current ticket implementation against:
- specification
- acceptance criteria
- project conventions
- tests
- potential bugs

Report findings first.
Do not modify code yet.
```

---

## Melihat Status Project

```text
Show all current tickets and their status.

Group them into:
- Completed
- In Progress
- Blocked
- Not Started

Also show the current git status.

Do not modify anything.
```

---

# 19. Workflow Singkat Berdasarkan Situasi

## Project / Fitur Baru

```text
grill-with-docs
      ↓
to-spec
      ↓
to-tickets
      ↓
implement
      ↓
tdd
      ↓
code-review
```

## Bug

```text
diagnosing-bugs
      ↓
tdd / regression test
      ↓
fix
      ↓
code-review
```

## Requirement Sudah Sangat Jelas

Tidak harus menjalankan grill lagi:

```text
to-spec
  ↓
to-tickets
  ↓
implement
```

## Ticket Sudah Ada

Langsung:

```text
implement
   ↓
tdd
   ↓
code-review
```

---

# 20. Rekomendasi Utama untuk Hemat Usage

Gunakan:

```text
Planning / specification
→ Sol + Medium

Coding sehari-hari
→ Terra + Medium

Task mekanis
→ Luna + Low

Bug sulit / architecture
→ Sol + High
```

Jangan otomatis menggunakan `High`.

Mulai dari reasoning paling rendah yang masih memberikan hasil baik.

Untuk mayoritas ticket:

```text
GPT-5.6 Terra + Medium
```

adalah titik awal yang sangat baik.

---

# 21. Ringkasan

Skill yang paling penting:

| Situasi | Skill |
|---|---|
| Setup repository | `setup-matt-pocock-skills` |
| Requirement belum jelas | `grill-with-docs` |
| Requirement → specification | `to-spec` |
| Spec → ticket | `to-tickets` |
| Mengerjakan ticket | `implement` |
| Membuat fitur dengan test | `tdd` |
| Mencari root cause bug | `diagnosing-bugs` |
| Memeriksa hasil implementasi | `code-review` |

Workflow utama:

```text
Requirement
    ↓
grill-with-docs
    ↓
to-spec
    ↓
to-tickets
    ↓
implement ONE ticket
    ↓
tests
    ↓
code-review
    ↓
commit
    ↓
next ticket
```

---

# Referensi

- Matt Pocock Skills  
  https://github.com/mattpocock/skills

- OpenAI Codex Models  
  https://developers.openai.com/codex/models

- OpenAI Codex CLI  
  https://developers.openai.com/codex/cli

- OpenAI Codex Configuration  
  https://developers.openai.com/codex/config-basic

> Catatan: model yang tersedia di Codex dapat berubah berdasarkan plan, rollout, dan versi Codex. Gunakan `/model` untuk melihat model dan reasoning level yang tersedia pada akun Anda.
