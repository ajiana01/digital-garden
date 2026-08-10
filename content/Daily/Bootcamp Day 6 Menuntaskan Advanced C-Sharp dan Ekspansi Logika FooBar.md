---
date: 2026-08-10
tags:
  - bootcamp
  - software-engineering
  - csharp
  - advance-csharp
  - algorithm
---
> [!summary] TL;DR
> Membuka minggu kedua dengan melanjutkan *self-paced learning* untuk *Advanced C#*. Progres pemahaman semakin terlihat dengan perolehan skor kuis 42/50. Hari ini juga ditutup dengan *logic exercise* berupa ekspansi dari "FooBar" menjadi "FooBarJazz". Besok bersiap untuk masuk ke fundamental *framework* dan pengumuman *individual project*.

## 🧠 Konsep Utama: Mematangkan *Advanced C#*

Hari pertama di minggu kedua ini masih didedikasikan untuk menguasai **Advanced C#** secara *self-paced*. Saya menggunakan waktu ini untuk mengulang dan memperdalam kembali materi-materi kompleks dari akhir pekan lalu, memastikan pemahaman mengenai fitur-fitur seperti *Delegates*, *Events*, dan memori (*Nullable types*) benar-benar melekat.

Memahami fitur *advanced* ini secara mandiri sangat penting karena besok kami akan mulai diperkenalkan pada fundamental *framework*, di mana konsep-konsep *advanced* bahasa C# ini akan banyak digunakan di balik layar.

## 🚧 Tantangan & Resolusi: Kuis Evaluasi dan *FooBarJazz*

> [!success] Evaluasi Pemahaman
> Di satu jam terakhir, diadakan kuis evaluasi untuk materi *Advanced C#*. Saya berhasil mendapatkan skor **42/50**. Ini adalah sebuah peningkatan yang sangat memuaskan dibandingkan skor-skor kuis di minggu pertama (39 dan 35), menandakan bahwa cara belajar dan adaptasi saya sudah berada di jalur yang benar.

**Daily Logic Exercise: *The FooBarJazz***
Tantangan logika hari ini adalah melanjutkan program "FooBar" dari hari pertama dengan aturan tambahan:
- Jika habis dibagi 3, cetak "foo"
- Jika habis dibagi 5, cetak "bar"
- **Aturan baru:** Jika habis dibagi 7, cetak "jazz"
- Jika memenuhi beberapa kondisi, *output* harus digabungkan (*concatenate*).

**Eksekusi:**
Logikanya menuntut pengelolaan *string* yang dinamis. Saya menggunakan pendekatan *string concatenation* berurutan dengan mengevaluasi kondisi *modulo* (`%`).
- `x = 21` (habis dibagi 3 dan 7) menghasilkan `foojazz`
- `x = 35` (habis dibagi 5 dan 7) menghasilkan `barjazz`
- `x = 105` (habis dibagi 3, 5, dan 7) menghasilkan `foobarjazz`

Setelah *logic* berjalan sesuai dengan seluruh *test case*, kode langsung saya *push* ke GitHub perusahaan.

## 💡 Refleksi Pribadi

Mendapatkan skor 42/50 hari ini memberikan dorongan kepercayaan diri yang luar biasa untuk memulai minggu kedua. Terlihat jelas bahwa membiasakan diri membaca dokumentasi dan mencoba kode secara mandiri (*self-paced*) memberikan dampak langsung pada pemahaman saya terhadap sintaks dan *behaviour* bahasa C#.

Di sisi lain, ada rasa antisipasi yang tinggi untuk esok hari. Kami akan mulai memasuki materi *Framework Fundamental* dan akan mendapatkan *briefing* mengenai **Tugas Proyek Individu**. Masa transisi dari sekadar mengerjakan kuis dan logika algoritma menuju pembuatan proyek nyata akan menjadi pembuktian sesungguhnya dari semua fundamental yang dipelajari selama 6 hari ini.

## 🔗 Referensi & Links