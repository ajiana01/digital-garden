---
date: 2026-08-07
tags:
  - bootcamp
  - software-engineering
  - formulatrix
  - csharp
  - advance-csharp
  - data-structure
---
> [!summary] TL;DR
> Menutup minggu pertama bootcamp dengan beralih ke materi *Advanced C#*. Sesi diawali dengan pemaparan dari Mas Akmal sebelum beralih ke *self-paced learning*. Tantangan logika di hari terakhir minggu ini adalah mengimplementasikan struktur data *Circular Queue*.

## 🧠 Konsep Utama: *Advanced C#*

Setelah memperkuat fundamental dan *Creating Types* di hari-hari sebelumnya, hari ini level kompleksitas mulai dinaikkan. Mas Akmal memberikan gambaran besar terlebih dahulu mengenai fitur-fitur tingkat lanjut di C#, sebelum akhirnya kami kembali mengatur ritme sendiri melalui *self-paced learning*.

Materi *Advanced C#* yang dibedah hari ini meliputi:
- **Delegates & Event Handler:** Memahami fondasi dari *event-driven programming* di ekosistem .NET.
- **Try Statements and Exceptions:** Praktik terbaik dalam menangani *error* dan menjaga aplikasi agar tidak *crash* (*error handling*).
- **Enumerator and Iterators:** Membedah cara kerja di balik layar dari *looping* (seperti `foreach`) dan penggunaan `yield`.
- **Nullable Value Types:** Mengelola data yang bisa bernilai *null* untuk menghindari *NullReferenceException* yang fatal.
- **Operator Overloading:** Mengubah perilaku operator standar (`+`, `-`, `==`, dll.) agar bisa digunakan pada tipe data atau *class* buatan sendiri.

## 🚧 Tantangan & Resolusi: *Logic Exercise* Akhir Pekan

> [!info] The Challenge: *Circular Queue*
> Sebagai tugas penutup minggu ini, *logic exercise* yang diberikan di Google Classroom adalah **Circular Queue** (Antrean Melingkar).

**Eksekusi:**
Berbeda dengan *Queue* linear yang saya kerjakan di hari kedua, *Circular Queue* menuntut manipulasi *pointer/index* (biasanya menggunakan operator *modulo*) agar ruang memori yang kosong di depan antrean bisa digunakan kembali. 

Saya menerapkan logika *pointer* `head` dan `tail` yang saling mengejar dalam batas kapasitas tertentu. Setelah algoritma berjalan sempurna dan melewati *test case*, kode tersebut langsung saya *commit* dan *push* ke repositori GitHub Formulatrix seperti biasa untuk diserahkan.

## 💡 Refleksi Pribadi

Menyelesaikan minggu pertama ini rasanya sangat memuaskan. Jika ditarik benang merahnya, kurikulum minggu ini disusun dengan alur yang sangat brilian. 

Dari sisi logika struktur data, saya diajak berprogres dari **Queue** (Day 2), lalu **Stack** (Day 3), Linked List (Day 4), dan sekarang ditutup dengan **Circular Queue** (Day 5). Sementara dari sisi bahasa pemrograman, perjalanannya mengalir mulus dari fundamental sintaks, pembuatan *class/types*, hingga kini menyentuh fitur-fitur *advanced* C# seperti *Delegates* dan *Events*. 

Masa *self-paced learning* setelah instruksi Mas Akmal juga melatih kemandirian dalam membaca dokumentasi. Ini adalah fondasi yang sangat kokoh sebelum menghadapi tantangan yang lebih besar di minggu kedua nanti. Waktunya istirahat sejenak dan melakukan *recharge*!

## 🔗 Referensi & Links