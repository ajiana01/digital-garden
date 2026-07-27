# 📄 GAME DESIGN DOCUMENT: [Judul Game - Misal: "Mod Simulator: The 7-Day Ban"]

**Studio:** Dopamine Rush
**Engine:** Unity 6
**Platform:** PC (Web/Standalone)
**Tools Visual:** Figma (Mockup UI), Clip Studio Paint (Aset 2D & Karakter)

### 1. Elevator Pitch

Sebuah game _roguelite UI-based point-and-click_ yang bertempo cepat, di mana pemain berperan sebagai moderator _live stream_. Tugas utama: menjaga "Stream Health" dari _chat_ toksik, donasi suara _earrape_, dan _media share_ vulgar selama 7 hari berturut-turut agar _streamer_ tidak di-_banned_.

### 2. Core Gameplay Loop

1. **Preparation Phase:** Pilih tipe _stream_ (menentukan _modifier_ hari itu).
    
2. **Action Phase (60 Detik/Hari):** _Stream_ berjalan. Lakukan moderasi (Klik/Drag/Slider) secepat mungkin.
    
3. **Upgrade Phase:** Gunakan uang donasi (_Donos_) yang terkumpul untuk membeli _upgrade_ di toko.
    
4. **Repeat:** Ulangi hingga Hari ke-7 (Menang) atau Stream Health habis (Game Over -> Ulang dari Hari 1).
    

### 3. Core Mechanics & Controls (The Action Phase)

Fokus pada mekanik yang sederhana secara _coding_, namun membutuhkan refleks dari pemain.

|**Ancaman / Target**|**Visualisasi di UI**|**Aksi Pemain (Controls)**|**Penalti jika Gagal**|
|---|---|---|---|
|**Komentar Toksik / Spam**|Teks _chat_ berwarna merah atau berikon peringatan.|**Left Click** pada _chat_ untuk menghapusnya.|Stream Health turun perlahan.|
|**Media Share Vulgar**|Layar _pop-up video_ di tengah UI (gambar placeholder/abstrak).|**Drag & Hold** kotak sensor hitam ke atas _pop-up_ selama 3 detik.|Stream Health turun drastis dalam 1 hit.|
|**Earrape / Scream Audio**|Notifikasi donasi besar diiringi efek visual UI bergetar (Audio kencang).|**Click & Drag** _Slider Volume UI_ ke arah kiri/bawah (Mute).|Streamer marah, penonton (skor uang) kabur.|
|**Chaos / Raid (Emergency)**|Layar dipenuhi _chat_ sangat cepat.|**Tekan Tombol Spasi** (Slow-Mode / Sub-Only). Layar melambat 5 detik.|-|

### 4. Roguelite Elements

#### A. Draft Modifier (Pilihan Tipe Stream)

Sebelum mulai, pemain disajikan 2 opsi _stream_ secara acak (RNG).

- **Horror Game Playthrough:** Ancaman _Earrape_ meningkat 50%. _Reward_ uang donasi 1.5x lebih banyak.
    
- **Just Chatting:** Kecepatan _Chat_ toksik meningkat 200%. _Reward_ uang standar.
    
- **Reaction YouTube:** Ancaman _Media Share_ vulgar muncul lebih sering. _Reward_ uang 2x lipat.
    

#### B. Toko Upgrade (Meta-Progression)

Mata uang: Uang Donasi (didapat berdasarkan sisa Stream Health di akhir hari).

- **Auto-Mod Bot:** Otomatis menghapus 1 _chat_ merah setiap 5 detik (Pasif).
    
- **Wide Censor Board:** Ukuran _collider/sprite_ kotak sensor menjadi 2x lebih besar.
    
- **Chill Pills:** Kapasitas maksimal Stream Health bertambah 20%.
    
- **Mod Keyboard:** _Cooldown_ tombol darurat (Spasi) berkurang 10 detik.
    

### 5. UI & Art Direction

Karena 90% _gameplay_ terjadi di kanvas UI, tata letak adalah segalanya.

- **Kiri Atas:** Avatar _Streamer_ (Bereaksi: Senyum jika aman, Panik/Menangis jika banyak pelanggaran).
    
- **Atas Tengah:** _Stream Health Bar_ (Warna Hijau ke Merah) dan Indikator Hari (Day 1/7).
    
- **Tengah:** Layar utama _stream_ (tempat _Media Share_ muncul).
    
- **Kanan:** Panel _Live Chat_ yang terus bergulir (_Scroll Rect_).
    
- **Bawah Kanan:** _Slider Volume_ UI.
    

### 6. Development Roadmap (48 Jam)

**Hari 1: Core Systems & Greyboxing (Fokus di Unity 6)**

- _Setup Canvas UI_, _Scroll Rect_ untuk _chat_.
    
- Buat _Object Pooling_ untuk _spawn_ teks _chat_ biasa dan _chat_ ancaman.
    
- Implementasi fungsi interaksi dasar: Klik untuk hapus (_destroy/disable_), _Drag_ kotak untuk sensor (_OnDrag_, _OnDrop_).
    
- Buat sistem _Health Bar_ dan Timer (60 detik per sesi).
    

**Hari 2: Roguelite Systems, Art & Polish (Fokus Game Feel)**

- Implementasi sistem _Day/Run_, _Modifier_ Sederhana, dan UI Toko _Upgrade_.
    
- Ganti _greybox_ dengan aset 2D/UI final dari Clip Studio Paint.
    
- **Juice it up:** Tambahkan _Screen Shake_ ringan saat Stream Health turun, suara ketikan _keyboard_, suara "BEEP" sensor, dan partikel uang saat dapat donasi.
    
- _Playtest_, sesuaikan kecepatan _spawn_ ancaman, dan _Build_.