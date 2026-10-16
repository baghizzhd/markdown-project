# Dokumentasi Belajar FitRep — Persiapan Tanya-Jawab

> Disusun 12 Okt 2026 dari **kode di folder ini**, file model `BicepCurl.mlmodel`, PRD v1.2, dan deck portofolio (`FitRep-Portfolio.pptx`).
> Semua angka sudah dicek langsung ke kode atau metadata model. Bagian bertanda **[CEK]** atau **[ISI]** hanya Anda yang tahu jawabannya — lengkapi sebelum wawancara.
> Nomor sitasi **[n]** mengikuti daftar pustaka deck versi terbaru (23 entri). Per 14 Okt 2026 deck berisi 31 slide: FitRep di slide 4–20 (The Problem = 5, The Solution = 6, Model Results = 13, bukti Create ML = 14–15, Watch & Try = 20), SIDIA di 21–30, References di slide 31.

---

## Daftar Isi

1. [Cara memakai dokumen ini](#1-cara-memakai-dokumen-ini)
2. [Pitch: 30 detik dan 2 menit](#2-pitch-30-detik-dan-2-menit)
3. [Kartu angka (wajib hafal)](#3-kartu-angka-wajib-hafal)
4. [Produk: masalah, solusi, alur pengguna](#4-produk-masalah-solusi-alur-pengguna)
5. [Keputusan arsitektur: kenapa hybrid](#5-keputusan-arsitektur-kenapa-hybrid)
6. [Data dan training model](#6-data-dan-training-model)
7. [Hasil dan evaluasi](#7-hasil-dan-evaluasi)
8. [Logika vonis repetisi (inti sistem)](#8-logika-vonis-repetisi-inti-sistem)
9. [Rekayasa iOS](#9-rekayasa-ios)
10. [Privasi, ketahanan, inklusivitas](#10-privasi-ketahanan-inklusivitas)
11. [Tiga kegagalan dan perbaikannya](#11-tiga-kegagalan-dan-perbaikannya)
12. [Keterbatasan yang harus dijawab jujur](#12-keterbatasan-yang-harus-dijawab-jujur)
13. [Bank pertanyaan dan jawaban](#13-bank-pertanyaan-dan-jawaban)
14. [Glosarium](#14-glosarium)
15. [Peta file kode](#15-peta-file-kode)
16. [Referensi kunci](#16-referensi-kunci)
17. [Checklist sebelum wawancara](#17-checklist-sebelum-wawancara)

---

## 1. Cara memakai dokumen ini

| Waktu belajar | Baca |
|---|---|
| 15 menit sebelum wawancara | §2 (pitch), §3 (angka), §12 (keterbatasan) |
| Belajar inti (±1 jam) | §4–§11, berurutan |
| Latihan lisan | §13 — tutup jawabannya, coba jawab sendiri, lalu bandingkan |

Tiga prinsip menjawab:
1. **Sebut komponen dan angka spesifik**, bukan kata sifat ("inovatif", "canggih").
2. **Bedakan yang sudah diukur dan yang baru direncanakan.** Contoh: akurasi model sudah diukur; latensi di iPhone belum.
3. **Akui keterbatasan lebih dulu sebelum ditanya.** Itu terlihat matang, bukan lemah.

---

## 2. Pitch: 30 detik dan 2 menit

### Versi 30 detik
> FitRep adalah pelatih bicep curl yang berjalan sepenuhnya di iPhone. Kamera depan membaca kerangka tubuh dengan Vision. Aplikasi lalu menghitung hanya repetisi yang bersih, dan menandai repetisi yang memakai ayunan badan atau gerakan tidak penuh. Sistemnya hybrid: aturan geometri sudut sendi memutuskan kapan satu repetisi terjadi, sedangkan model Create ML berukuran 4,1 MB menilai kualitas gerakannya. Akurasi testing model 94%, dan tidak ada satu pun data yang keluar dari perangkat.

### Versi 2 menit (masalah → solusi → cara → hasil → pelajaran)
1. **Masalah.** Pemula yang latihan sendiri tidak punya umpan balik objektif per repetisi. Dua kesalahan paling umum sulit dirasakan sendiri: mengayun badan (momentum) dan tidak menurunkan lengan sampai lurus. Pelatih pribadi mahal, sedangkan video tutorial hanya satu arah.
2. **Solusi.** Cukup iPhone dan dumbbell. Pengguna berdiri menyamping, lalu aplikasi berjalan tanpa disentuh: hitung mundur, menilai setiap repetisi, istirahat antar set. Rep yang benar mendapat kilatan hijau dan bunyi chime, lalu dihitung. Rep yang salah mendapat kilatan merah dan bunyi berbeda, dan tidak dihitung.
3. **Cara kerja.** Vision mengekstrak 18 titik sendi per frame. Dari titik itu dihitung sudut siku dan kemiringan badan. State machine memutuskan awal dan akhir repetisi. Paralel dengan itu, model Action Classifier menilai jendela 90 frame (3 detik), tetapi hanya diberi hak **veto**.
4. **Hasil.** Akurasi testing 94% (173 dari 185), dengan training 96,9% dan validasi 94,3%. Kesalahannya satu arah: sekitar 1 dari 7 window yang salah terbaca benar. Karena itu ayunan badan dan range of motion dicek secara geometris, tidak bergantung pada model.
5. **Pelajaran.** Akurasi model bukan akurasi produk. Langkah berikutnya bukan model yang lebih besar, melainkan evaluasi per repetisi dengan orang baru dan pengukuran di perangkat.

---

## 3. Kartu angka (wajib hafal)

### Model dan hasil
| Angka | Arti | Sumber |
|---|---|---|
| **96,9%** | Akurasi training, iterasi 80 (34.432/35.533 window) | Create ML |
| **94,3%** | Akurasi validasi, split otomatis (1.717/1.820 window) | Create ML |
| **94%** | **Akurasi testing** Testing-2 (173/185) — KPI utama di deck, sama dengan tampilan Create ML (slide 15). Nilai persisnya 93,5% | Create ML |
| **85,7%** | Recall kelas Salah (validasi) → ±1 dari 7 window salah terbaca benar | Create ML |
| **100%** | Recall kelas Benar (validasi dan test) → tidak ada rep benar yang ditolak model | Create ML |
| **4,1 MB** | Ukuran model (penyimpanan Float32, komputasi Float16) | file model |
| **27 · 20 · 14** | Jumlah lapisan Convolution · BatchNorm · ReLU | metadata Core ML |
| **90 × 3 × 18** | Input: 90 frame × (x, y, confidence) × 18 sendi | metadata Core ML |
| **3 kelas** | `Benar`, `Salah`, `Others` (tanpa kelas Idle terpisah; klip diam termasuk Others) | metadata Core ML |
| **80** | Iterasi training, Create ML 6.2, 8 Sep 2026 (±32 menit) | metadata Core ML |

### Data
| Angka | Arti |
|---|---|
| **140 / 60** | Klip training / test per kelas → **420 train · 180 test** |
| **2,3–2,8 s** | Durasi satu klip = satu repetisi (rata-rata ≈ 2,75 s) |
| **2.764.800 → 54** | Nilai per frame: piksel 1280×720×3 vs 18 sendi × 3 → ±51.200× lebih kecil |
| **4.860** | Nilai per window (90 × 54) ≈ 19 KB |

### Ambang logika vonis (`RepetitionTracker.swift`)
| Angka | Arti |
|---|---|
| **< 140°** | Siku mulai menekuk → repetisi dimulai |
| **< 75°** | Fleksi penuh (puncak curl) |
| **> 145°** | Ekstensi kembali → repetisi selesai |
| **1,0–6,0 s** | Durasi repetisi yang sah (batas bawah dinaikkan dari 0,8 s pada 15 Okt 2026) |
| **> 12°** | Perubahan kemiringan badan sejak awal rep → ayunan badan |
| **> 35°** | Lengan atas miring dari vertikal → siku bergeser/terayun |
| **≥ 0,60** | Confidence minimal prediksi model agar dihitung sebagai bukti |
| **≥ 30 frame** | Overlap minimal window dengan repetisi yang sedang dinilai (1,0 detik) |
| **≥ 2** | Window "Salah" yang cukup untuk vonis salah |
| **5 frame** | Moving average untuk meredam noise sudut (jeda ±67 ms) |
| **0,4** | Confidence minimal sendi bahu/siku/pergelangan |

### Pipeline dan sesi
| Angka | Arti |
|---|---|
| **30 fps · 720p** | Kamera depan, dibatasi 30 fps sesuai frame rate data training |
| **stride 10** | Prediksi baru setiap 10 frame (±0,33 s) setelah window penuh |
| **≥ 60%** | Minimal frame dengan pose asli dalam satu window agar diprediksi |
| **30 frame** | Posisi menyamping harus ditahan ±1 detik |
| **5 s · 30 s** | Hitung mundur · istirahat antar set (bisa dilewati) |
| **1,2 s** | Lama warna hijau/merah tampil |
| **1,15×** | Zoom halus saat latihan dimulai |
| **3 × 12** | Rencana bawaan (set × rep). Rentang di kode: set 1–6, rep 6–20 (lihat §12) |

### Rekayasa
| Angka | Arti |
|---|---|
| **0** | Kode jaringan (tidak ada `URLSession`, dicek ulang) |
| **91** | String terlokalisasi (bahasa sumber Indonesia + English) |
| **6** | Kasus kegagalan yang ditangani di kode |
| **1** | Satu-satunya `@unchecked Sendable` (pembungkus model Core ML) |
| **iOS 17.2+ · Swift 6** | Target minimum · language mode |

### Tim **[CEK]**
6 anggota · 4 minggu (sesuai versi Canva). Konfirmasi juga periode proyek: video demo bertanggal 17 Jun 2026, sedangkan model dilatih 8 Sep 2026.

---

## 4. Produk: masalah, solusi, alur pengguna

### Masalah (slide FitRep 02, disusun ulang 13 Okt 2026)

Slide ini dibaca sebagai satu alur cerita: **suara pengguna → kenapa teknik penting → apa yang mereka coba sekarang → apa yang benar-benar mereka butuhkan.**

**Pembuka lisan, tidak tertulis di slide: persona Chris** (dari riset Apple Developer Academy). Chris berlatih dengan personal trainer sampai merasa cukup paham, lalu berhenti karena tidak mau terus membayar dan mulai berlatih sendiri. Saat itulah masalahnya muncul. **[CEK]** Pastikan apakah Chris persona fiktif atau orang nyata.

**1. Suara pengguna (baris atas slide)** — kutipan wawancara dengan pemula gym, diterjemahkan dari bahasa Indonesia:
| Narasumber | Di slide | Kutipan asli |
|---|---|---|
| Kevin | "I struggle to tell whether my form is right or not…" | "Kesulitan apakah form gerakan yang dilakukan sudah benar atau belum, …" |
| Weka | "…If I move the wrong way, it's bound to hurt." | "..ngga ngerti cara penggunaan alat-alatnya yang benar. Kalau salah gerak pasti sakit." |
| Luist | "At the gym, people usually don't know where to start." | "Di gym, biasanya orang itu dia bingung mulai dari mana." |

Cadangan kalau ditanya: Luist juga pernah bilang ia merasa membuang waktu setahun di gym karena tidak tahu targetnya ("Dan aku akhirnya ngabisin waktu 1 tahunan gitu sia-sia gymku…"). Jessica, "Malu, ngga ngerti pake alat-alatnya takut diliatin kalau salah" (cocok untuk argumen latihan privat di rumah). Dylan, "Awal gym ngga ngerti cara pakai alat gimana." **[CEK]** Jumlah total responden dan izin menampilkan nama depan.

**2. Kenapa teknik penting (kiri bawah)**
- Kutipan ahli, dicek kata per kata ke PDF asli [2]: *"An incorrect technique can lead to hurting your tendons, ligaments and muscles. … A wrong technique will not help you achieve your goals."*
  - Sumber: Nae dkk. (2022), panduan teoretis latihan gym untuk mahasiswa di jurnal *Marathon*, bagian "Badly executed exercises".
  - Ini **panduan, bukan studi cedera.** Jadi pakai sebagai pendukung, bukan bukti klinis.
  - Elipsis (…) menandai satu kalimat yang dilewati.
  - Atribusi keynote lama "Cristina, et al." keliru: Cristina adalah nama depan, nama keluarganya **Nae**.
- Dua kesalahan bicep curl yang sulit dirasakan sendiri:
  - **Momentum cheating:** ayunan badan yang mengangkat beban. Ini mekanika dasar, sengaja tanpa sitasi.
  - **Incomplete ROM:** lengan tidak sampai posisi teregang (siku lurus). Fase panjang-otot terkait adaptasi otot [3].

  **Penjelasan detail dua baris di slide:**

  | Baris di slide | Artinya | Contoh nyata | Di FitRep |
  |---|---|---|---|
  | **Swinging the Body** — "The swing lifts the weight, not your arm." *(istilah teknis: momentum cheating)* | Badan yang mengayun yang mengangkat beban, bukan otot bisep | Beban terasa berat → badan diayun ke belakang supaya dumbbell "terlempar" naik → bisep bekerja lebih ringan | Badan berubah miring > **12°** sejak awal rep, atau lengan atas > **35°** dari vertikal → vonis **salah** (merah, tidak dihitung) |
  | **Incomplete ROM** — "Arm never fully straightens [3]." | ROM = *Range of Motion*. Lengan tidak diturunkan sampai lurus, jadi posisi saat bisep paling **teregang** terlewat | Gerakan setengah jalan: dari siku setengah menekuk langsung naik lagi tanpa meluruskan lengan | Siku harus < **75°** lalu > **145°**, dalam 1–6 s; kalau tidak → rep **ditolak** (tidak dihitung, tanpa suara) |

  - **Kenapa momentum tanpa sitasi?** Ini mekanika dasar: gaya dari ayunan menggantikan kerja otot.
  - **Kenapa posisi teregang penting?** Wolf dkk. (2025) [3] menemukan latihan di bagian rentang yang teregang (*lengthened partials*) menghasilkan adaptasi otot setara dengan gerakan penuh. Kalau bagian itu dilewati, manfaatnya hilang.
  - **Kenapa angka ambang tidak ada di slide 5 (The Problem)?** Itu slide masalah, jadi cukup bahasa pengguna. Angka 12°, 35°, 75°, dan 145° dijelaskan di slide 10 (The Hybrid Decision).
  - **Kalimat siap ucap:** "Ada dua kesalahan yang sulit dirasakan sendiri saat bicep curl. Pertama, momentum cheating: badan diayun sehingga bukan otot yang mengangkat beban. Kedua, rentang gerak tidak penuh: lengan tidak diluruskan, sehingga posisi otot teregang yang penting untuk pertumbuhan otot terlewat."
  - **Apakah "Arm never fully straightens" masih didukung [3]?** Ya. Posisi otot bisep paling teregang terjadi saat siku hampir lurus. Jadi "lengan tidak pernah lurus penuh" = posisi teregang terlewat, dan itulah fase yang menurut Wolf dkk. [3] penting untuk adaptasi otot.
  - **Nuansa penting kalau ditanya lebih dalam:** [3] **tidak** mengatakan semua gerakan parsial buruk. Parsial di bagian teregang (*lengthened partials*) justru hasilnya setara dengan gerakan penuh. Yang bermasalah adalah parsial yang **melewatkan** bagian teregang, yaitu lengan yang tidak pernah diluruskan. Jawaban aman: *"Masalahnya bukan gerakan parsialnya, tapi posisi teregang yang terlewat."*
  - Ambang 12° dan 145° sengaja dipindah ke slide FitRep 07; slide masalah cukup menjelaskan kesalahannya dengan bahasa pengguna.

**3. Apa yang mereka coba sekarang (kanan bawah)** — sesuai keynote "Existing Behavior":
| Cara | Kekurangan |
|---|---|
| Personal trainer | Koreksi langsung, tapi mahal (bayar per sesi) |
| Cermin | Bisa melihat diri sendiri, tapi tidak pernah memberi tahu apa yang salah |
| Video & artikel online | Gratis, tapi bisa menyesatkan dan sulit diterapkan sendiri |

**4. Kebutuhan dan Challenge (bar bawah)**
- **They need:** *guidance* (tahu apa yang harus dilakukan) + *live feedback* (tahu apakah setiap rep benar).
- **Challenge:** "How might we give coach-level feedback on every rep with only an iPhone?"
- Slide berikutnya (FitRep 03) menjawab keduanya:
  - *Live feedback* = G1 vonis per rep + G2 deteksi kesalahan form.
  - *Guidance* = layar panduan, panduan keamanan, dan panduan suara saat penentuan posisi.

**Kenapa tidak mengklaim "mencegah cedera"?** Klaim itu butuh validasi klinis. Slide menyebut risiko cedera hanya sebagai **alasan teknik itu penting** (suara pengguna + Nae dkk.). Yang FitRep klaim adalah **umpan balik teknik**, bukan pencegahan cedera. Kata "injury prevention" sengaja dihapus sejak lamaran 2025.

### Tiga design goal (dipakai sebagai tolok ukur di slide evaluasi)
- **G1 · Vonis per repetisi:** umpan balik setelah tiap rep, bukan aliran koreksi terus-menerus. Riset motor learning menunjukkan bahwa jadwal dan waktu pemberian umpan balik memengaruhi hasil belajar gerakan [4].
  - **Isi penelitian [4]** (Akizuki dkk., 2025, *Front. Psychol.*, abstrak dicek): tiga kelompok diberi umpan balik visual dengan waktu berbeda.
    - **Concurrent** = umpan balik **selama** gerakan, terus-menerus.
    - **Terminal** = umpan balik **setelah** gerakan selesai.
    - **Self-controlled** = peserta memilih sendiri kapan menerima umpan balik.
  - **Hasil:** saat latihan, kelompok *concurrent* paling sedikit salah. Tapi pada tes retensi seminggu kemudian, tanpa umpan balik, kelompok *terminal* dan *self-controlled* justru **lebih baik**. Kesimpulan penulis: umpan balik terus-menerus membuat peserta **bergantung** padanya.
  - **Hubungan ke FitRep:** vonis diberikan **setelah setiap rep selesai** (gaya *terminal*), bukan koreksi terus-menerus selama gerakan (gaya *concurrent*). Tujuannya agar pengguna benar-benar belajar gerakannya, bukan bergantung pada aplikasi.
  - **Nuansa jujur:** yang terbaik di penelitian itu adalah *self-controlled*. FitRep belum memberi pengguna pilihan kapan menerima umpan balik; ini bisa jadi ide pengembangan.
  - **Catatan:** kalimat ini ada di **slide 6 (The Solution)**, kartu G1, bukan di slide masalah.
- **G2 · Deteksi kesalahan form:** ayunan badan, siku bergeser, ROM parsial.
- **G3 · 100% on-device:** video dianalisis di memori; tidak disimpan, tidak dikirim.

### Alur pengguna (sesuai kode)
1. **Daftar latihan:** Bicep Curl aktif; Push Up dan Squat tampil terkunci ("Segera hadir"). Di latar belakang, Vision dan model Core ML mulai di-*warm up*.
2. **Layar panduan:** instruksi, hal yang perlu dihindari, dan pilihan set × rep (bawaan 3 × 12).
3. **Panduan keamanan 4 halaman:** berat beban, beban terlalu ringan, beban terlalu berat, posisi kamera.
4. **Penentuan posisi:** berdiri menyamping, wajah terlihat, ditahan 30 frame (±1 detik). Setelah itu hitung mundur 5 detik.
5. **Latihan:**
   - Rep benar → kilatan **hijau** dan chime, dihitung.
   - Rep salah → kilatan **merah** dan bunyi "Wrong", tidak dihitung.
   - Rep tidak sah (ROM tidak lengkap atau durasi di luar 1–6 s) → tanpa suara, tidak dihitung.
6. **Set selesai** saat jumlah rep **benar** mencapai target. Lalu istirahat 30 detik dengan kamera dijeda; istirahat bisa dilewati.
7. **Selesai** setelah set terakhir: kamera mati, layar selesai, audio penutup.

Aplikasi ini **tanpa sentuh** selama latihan: pengguna tidak perlu menyentuh layar dari penentuan posisi sampai set terakhir.

---

## 5. Keputusan arsitektur: kenapa hybrid

### Pertanyaan penentunya: siapa yang menghitung rep, dan siapa yang menilai kualitasnya?

| Kriteria | Geometri murni | Model video end-to-end | **Hybrid (dipilih)** |
|---|---|---|---|
| Hitung rep diskret | ✓ | ✕ (butuh penghitung terpisah [7]) | ✓ |
| Menilai form secara holistik | △ (hanya aturan yang dikodekan) | ✓ | ✓ |
| Data yang dibutuhkan | ✓ tidak ada | ✕ besar | △ 140 klip/kelas |
| Kesalahan bisa dijelaskan | ✓ | ✕ | ✓ |

Perbandingan ini **kualitatif, berbasis literatur**, bukan benchmark internal. Di slide (bahasa awam): kolom menjadi **Angle Rules Only · AI Video Model Only · Rules + AI**, dan baris menjadi *How it works · Counts each rep · Judges overall form · Training data needed (teks saja, tanpa ikon) · Explains mistakes* (baris "jalan di ponsel" dihapus karena kedua pendekatan bisa berjalan di ponsel; bedanya hanya beban komputasi, yang sudah disebut di "How it works")**. Keputusan di bar bawah: **"Angles count the reps; AI checks the form."** Bukti kuantitatifnya (ablation) baru direncanakan.

**Prinsip keputusan:** aturan dipakai di bagian yang pasti; ML dipakai hanya di bagian yang butuh penilaian.
Di slide ditulis singkat: **"Angles count the reps; AI checks the form."** Kalimat lisan: *"Counting a rep has a clear answer, so simple angle rules handle it. Judging whether the form is good needs more judgment, so that's where the AI comes in."*

### Kenapa tidak membiarkan ML yang menghitung? (Slide 10: kotak "Why not let AI count reps?")
Banyak orang mengira aplikasi fitness berbasis AI menyuruh ML melakukan segalanya (mulai dari menghitung repetisi sampai menilai gerakan). Namun, di FitRep tugas itu dipisah secara tegas: **"Angles count the reps; AI checks the form."**

#### Mengapa sifat tumpang tindih (*sliding window*) membuat ML gagal menghitung?
Model Action Classifier Create ML tidak bisa menebak dari 1 frame foto saja. Model membutuhkan rentang waktu **90 frame (3 detik)** untuk mengenali pola gerakan. Agar aplikasi responsif, model menebak setiap **10 frame (stride)** sekali.

Perhatikan bagaimana jendela (*window*) 90 frame itu bergeser:
- **Tebakan ke-1:** membaca frame **1 s.d. 90**
- **Tebakan ke-2:** membaca frame **11 s.d. 100** *(frame 11–90 dibaca ulang lagi!)*
- **Tebakan ke-3:** membaca frame **21 s.d. 110** *(frame 21–100 dibaca ulang lagi!)*
- **Tebakan ke-4:** membaca frame **31 s.d. 120**

Karena jendela-jendela tersebut **saling tumpang tindih (*overlapping* hampir 90%)**, isi video di tebakan ke-1, ke-2, dan ke-3 hampir sama persis.

#### Masalah Fatal Jika ML yang Menghitung:
Misalnya Anda melakukan **1 kali gerakan bicep curl** selama 2,5 detik (misal berlangsung dari frame 30 sampai 105):
1. **Di tebakan ke-2 (frame 11–100):** Model melihat Anda melakukan curl → Model menebak *"Benar"* (confidence 85%).
2. **Di tebakan ke-3 (frame 21–110):** Model masih melihat gerakan curl yang sama → Model kembali menebak *"Benar"* (confidence 92%).
3. **Di tebakan ke-4 (frame 31–120):** Model masih melihat kelanjutan curl tersebut → Model menebak *"Benar"* (confidence 88%).

Jika sistem mengandalkan tebakan ML untuk menambah hitungan repetisi:
- Kapan counter harus bertambah? Di tebakan ke-2, ke-3, atau ke-4?
- Karena ketiga tebakan tersebut sama-sama berteriak *"Benar!"*, sistem berbasis ML murni akan bingung dan berisiko menghitung **3 kali repetisi (*double/triple-counting*)**, padahal Anda hanya melakukan **1 kali gerakan fisik**.
- Model ML hanya tahu: *"Di dalam durasi 3 detik ini, ada aktivitas bicep curl."* Namun, model **tidak tahu kapan tepatnya gerakan fisik itu mulai di bawah dan selesai di bawah**.

#### Solusi Kinematika Geometri (Bagaikan Saklar Lampu):
Sebaliknya, kinematika sudut siku memiliki status fisik yang diskret (nyata dan tegas):
1. **Titik Mulai:** dari posisi lengan lurus (`lowered`), siku mulai menekuk melewati $140^\circ$ (sudut $< 140^\circ$).
2. **Titik Puncak:** Menekuk naik sampai kontraksi penuh (`raised`, $< 75^\circ$).
3. **Titik Selesai:** Lengan kembali lurus ke bawah (`lowered`, $> 145^\circ$).

Tepat saat kondisi ke-3 terpenuhi, barulah repetisi dihitung **+1**. Seperti saklar lampu yang ditekan ke atas lalu ke bawah (*klik!*), hitungannya mutlak dan tidak mungkin terhitung ganda.

> **Kesimpulan:** Sudut siku bertindak sebagai **pencatat hitungan (1, 2, 3)**, sedangkan ML bertindak sebagai **pemeriksa teknik yang hanya boleh membatalkan** (veto): ia bisa membuat rep dinyatakan salah, tetapi tidak bisa mengesahkan atau menambah hitungan. Analogi: rumus sudut = **wasit** yang menghitung; AI = **asisten video** yang hanya boleh berkata "batalkan".

### Kenapa input berupa kerangka (pose), bukan piksel? — slide 9 "Why Skeletons Beat Pixels Here"
- **Ringkas:** 54 nilai per frame dibanding 2,7 juta (±51.200× lebih kecil).
  - Piksel: 1280 × 720 × 3 warna = 2.764.800 angka per frame.
  - Kerangka: 18 sendi × (x, y, keyakinan) = 54 angka per frame.
  - Satu window 3 detik: 90 × 54 = 4.860 angka ≈ 19 KB (Float32).
- **Jauh kurang terpengaruh penampilan:** pakaian, pencahayaan, dan latar belakang tidak ikut masuk ke classifier. Kualitas deteksi pose Vision sendiri tetap bisa dipengaruhi cahaya atau pakaian, jadi jangan bilang "kebal".
- **Privat sejak awal:** yang diproses hanya koordinat sendi.
- **Berbentuk graf:** sendi sebagai simpul, tulang dan lintasan antar-frame sebagai sisi. Ini dasar riset GCN untuk pengenalan aksi berbasis kerangka [11].
  - Cara membaca diagram di slide: tiga kerangka (t − 1, t, t + 1) adalah frame berurutan. Garis biasa = tulang dalam satu frame. Garis putus-putus = sendi yang sama dari satu frame ke frame berikutnya, sehingga **gerakan** terbaca, bukan hanya pose diam.
- **Dua kartu status di slide 9:**
  - **VERIFIED:** isi model yang bisa diperiksa langsung dari file Core ML (lihat "Model apa yang dipakai?" di bawah).
  - **TO MEASURE:** pembagian kerja CPU, GPU, dan Neural Engine [12] belum diukur. Ukur lewat Xcode → `BicepCurl.mlmodel` → Performance, dengan iPhone tersambung. Jangan bilang "100% Neural Engine".

> Kalimat siap ucap: "Saya memakai kerangka tubuh, bukan piksel. Datanya sekitar 51 ribu kali lebih kecil, jauh kurang terpengaruh baju atau cahaya, dan lebih menjaga privasi. Soal isi model, saya hanya mengklaim yang bisa saya periksa sendiri dari file Core ML-nya."

### Model apa yang dipakai? (jawab hati-hati)
- **Create ML Action Classifier** (berbasis body pose), diekspor sebagai Core ML.
- **Yang bisa diverifikasi** dari spesifikasi Core ML: 27 Convolution, 20 BatchNorm, 14 ReLU, 1 average pooling, softmax, komputasi Float16.
- **Kenapa kartu VERIFIED tidak diberi sitasi?**
  - Angka 27 · 20 · 14 · Float16 adalah **bukti primer**: dibaca sendiri dari file `BicepCurl.mlmodel`, sama seperti tangkapan layar Create ML (slide 14–15). Slide sudah menyebut sumbernya ("Core ML spec").
  - Dicek ulang 14 Okt 2026: 117 lapisan, di antaranya **27 convolution, 20 batchnorm, 14 activation (semua ReLU)**, 1 pooling, 1 softmax; metadata `computePrecision: Float16`, `storagePrecision: Float32`.
  - "Apple doesn't **document** the architecture" adalah klaim ketiadaan, jadi tidak bisa dikutip. Pegangannya: transkrip WWDC20 [1] dan dokumentasi [8] hanya membahas data dan parameter (frame rate, durasi, augmentasi, iterasi); kata *architecture*, *convolution*, atau *neural network* tidak muncul di transkrip [1].
  - **Jebakan:** versi **terkompilasi** (`.mlmodelc`, mis. hasil `xcrun coremlcompiler compile`) menunjukkan 8 BatchNorm dan 13 aktivasi, karena kompiler menggabungkan sebagian lapisan. Angka 27 · 20 · 14 berlaku untuk file spesifikasi `.mlmodel` sebelum dikompilasi.
  - Cara cek: `coremltools` → `spec.neuralNetworkClassifier.layers` (hitung jenis lapisan); presisi terlihat di `metadata.json` hasil kompilasi.

> Kalimat siap ucap: "Angka itu saya baca sendiri dari file Core ML-nya, jadi tidak perlu kutipan. Arsitekturnya memang tidak didokumentasikan Apple; WWDC dan dokumentasi Create ML hanya membahas data dan parameter training."
- **Apple tidak mendokumentasikan arsitektur internalnya.** Jangan mengklaim pasti "ST-GCN". Jawaban aman: *"Jaringan konvolusi atas urutan kerangka; konsepnya sejalan dengan riset GCN spasial-temporal, tetapi detail arsitekturnya tidak didokumentasikan Apple, jadi saya hanya mengklaim yang bisa saya inspeksi."*

---

## 6. Data dan training model

### Pipeline data
1. **Rekam:** video iPhone dari samping.
2. **Potong:** satu klip = satu repetisi (2,3–2,8 s).
   - **Kenapa 2,3–2,8 s cocok dengan 90-frame window (3,0 s)?**
     - Pada 30 fps, satu repetisi di klip training kami (fase konsentrik naik + eksentrik turun) berdurasi 2,3–2,8 detik (≈ 70–84 frame). Ini durasi rep di data kami, bukan durasi rep manusia pada umumnya; tempo pengguna nyata bisa berbeda (gerbang 1 menerima 1–6 s).
     - Window Create ML 90 frame (3,0 detik) sengaja dipilih sedikit lebih besar dari rata-rata repetisi agar **seluruh siklus gerakan dari awal tekukan hingga lurus kembali tercakup utuh dalam satu jendela observasi**.
     - Jika window terlalu pendek (misal 60 frame / 2,0 s), gerakan terpotong di tengah dan model kehilangan konteks lintasan temporal secara utuh. Jika terlalu panjang (misal 150 frame / 5,0 s), latensi membengkak dan window akan didominasi noise frame antar-repetisi.
     - Selisih kecil (±0,2–0,7 s atau 6–20 frame) diisi oleh transisi gerakan dan ditangani oleh saringan overlap minimal 30 frame saat evaluasi repetisi (§8 & §11).
3. **Label:** dikelompokkan ke dalam 3 kelas target (`Benar`, `Salah`, `Others`) dan diseimbangkan (140 train · 60 test per kelas) menggunakan variasi kecepatan dan *oversampling* untuk menutupi keterbatasan waktu rekaman. Augmentasi bawaan Create ML **tidak** diaktifkan.
4. **Create ML [8]:** Vision mengekstrak pose kerangka tubuh (*landmarks*), target 30 fps, window prediksi 90 frame.
   - **Dari mana asal angka 90 frame?**
     - Sesuai rumus resmi Apple di dokumentasi [8]:
       $$\text{Prediction Window Size (frames)} = \text{Frame Rate (fps)} \times \text{Action Duration (detik)}$$
     - Karena kamera iPhone diatur pada **30 fps** dan parameter **Action Duration** di Create ML diatur **3 detik** (sedikit di atas durasi rep 2,3–2,8 s, agar satu rep utuh muat), maka Create ML otomatis menghitung ukuran window:
       $$\text{30 fps (gambar per detik)} \times \text{3 detik (durasi)} = \mathbf{90\text{ frame}}$$
5. **Split [9]:** pembagian 70:30 yang umum dipakai: folder training (140/kelas), validasi dipisahkan otomatis oleh Create ML dari data training, folder test terpisah (60/kelas).

### Kelas
| Kelas | Arti |
|---|---|
| `Benar` | Repetisi dengan form benar |
| `Salah` | Repetisi dengan kesalahan form |
| `Others` | Semua yang bukan curl (*negative class* sesuai panduan WWDC20 [1]): **klip diam** dan **gerakan lain** (aktivitas acak/bukan latihan), agar tidak dipaksa terbaca Benar/Salah |

Sesuai sesi WWDC20 [1], Apple secara spesifik menyarankan menyertakan *negative class* (seperti folder `"other"`) untuk mengumpulkan contoh gerakan non-target (peregangan, istirahat, penyesuaian tubuh). Tanpa kelas ini, model klasifikasi aksi akan dipaksa memetakan setiap gerakan ke dalam kelas aksi target.

**Kenapa kelas Other wajib ada (ditekankan di slide 8, kartu "An Other Class Is a Must")?**
- Kutipan transkrip [1]: *"The model does need to understand exercises, but it also needs to understand when we're not exercising, or not moving at all."*
- Classifier selalu memilih salah satu kelas yang ia kenal. Tanpa Other, gerakan yang bukan curl (misalnya membetulkan posisi atau meregangkan badan) tetap akan dipaksa menjadi Benar atau Salah.
- Bila ditanya soal saran dua folder (*"one for walking around and stretching, and one for sitting or doing nothing quietly"*): Apple memakai kata *"could"*, jadi itu pilihan. Others menggabungkan klip diam dan gerakan bukan curl dalam satu folder, cara yang juga disebut Apple: *"you could put all these together in a single folder called 'other'"* [1].

> Kalimat siap ucap: "Apple bilang model juga harus belajar kapan kita tidak sedang berolahraga. Karena itu ada kelas Other, supaya gerakan bukan curl tidak dipaksa dinilai benar atau salah."

Tidak ada kelas **Idle** (diam) **terpisah**; klip diam sudah termasuk di `Others`. Model lama (`BicepCurl_3000`, Create ML 5.0, 3.000 iterasi) punya kelas Idle dan OtherAction. Model aktif hanya 3 kelas. Akibatnya bisa dilihat di §11 (masalah rep pertama).

### "Same extractor" — kenapa penting
Create ML dan aplikasi sama-sama membaca **18 titik tubuh** dari Vision (x, y, confidence) pada **30 fps** yang sama [1], [10]. Jadi tidak ada *feature skew* (perbedaan cara ekstraksi fitur antara training dan pemakaian).
- Sesi WWDC20 [1] menyebut langsung bahwa Action Classifier memakai **18 landmark**, sehingga pertanyaan "Vision kan 19 titik?" terjawab: 19 titik dideteksi Vision, 18 dipakai classifier.
- Sesi yang sama juga menyarankan frame rate yang konsisten antara training dan pemakaian; kamera FitRep dikunci 30 fps, sama dengan data latih.

### Kenapa 70:30 (140 train · 60 test per kelas)? — kotak "Split [9]" di slide 8
Slide 8 menulis **"Split [9] · Common 70/30 split · Train · validate · test"**.
- **Sumber [9]:** Sivakumar dkk. (2024), *PeerJ Computer Science*: *"the most common train-test splits in the literature are 70:30 and 80:20"*. Jadi klaim di slide hanya "**umum dipakai**", bukan "ideal".
- **Jujur bila ditanya "apakah 70:30 paling bagus?":** tidak ada bukti bahwa 70:30 ideal untuk data kecil.
  - Paper [9] sendiri menemukan akurasi cenderung naik bila porsi training lebih besar (diuji 50:50 sampai 95:05, data MRI tumor otak).
  - Bichri dkk. (2024, *IJACSA* 15(2)), dengan dataset kecil 1.000 gambar, menyimpulkan training perlu **lebih dari 70%** (80:20 dan 90:10 paling baik). Tidak dicantumkan di deck.
  - Dobbin & Simon (2011, *BMC Med. Genomics*): porsi 2/3 training mendekati optimal bila data ≥ 100 sampel dan akurasi ≥ 85%. FitRep (600 klip, 94%) memenuhi syarat itu, tapi paper ini lebih dari 3 tahun dan bidangnya genomik. Tidak dicantumkan di deck.
- **Ingat:** test set FitRep berisi klip augmentasi, jadi jangan sebut *unseen*. Paper-paper di atas mengasumsikan test set yang benar-benar terpisah.
- **Validasi** tidak termasuk 30%: Create ML mengambilnya otomatis dari folder training.
- **Alasan pribadi memilih 140/60:** **[ISI]**

> Kalimat siap ucap: "Saya memakai 70:30, salah satu rasio yang paling umum. Studi terbaru menunjukkan porsi training yang lebih besar sering sedikit lebih baik, jadi langkah berikutnya menguji 80:20 atau cross-validation dengan subjek baru."

### Kenapa slide 8 memakai [8] dan [10] di tempat berbeda?
Keduanya mendukung klaim yang berbeda. Dalam IEEE, setiap klaim dirujuk ke sumber yang benar-benar membuktikannya.

| Teks di slide 8 | Klaimnya | Sumber |
|---|---|---|
| Kotak **Create ML [8]**: Vision pose · 30 fps · 90-frame window | Cara **melatih** model: pengaturan Frame Rate dan prediction window | **[8]** dokumentasi *Creating an Action Classifier Model*: frame rate model disamakan dengan aplikasi, dan window = Frame Rate × Action Duration (30 × 3 = 90) |
| Kartu **Same Extractor** … [1], [10] | **Aplikasi saat berjalan** membaca titik tubuh yang sama dengan saat training | **[1]** WWDC20: 18 landmark dan frame rate yang konsisten · **[10]** sample code *Detecting Human Actions in a Live Video Feed*: aplikasi live mengambil pose dari Vision lalu mengirimnya ke classifier |

- [10] tidak membahas pengaturan training atau asal angka 90 frame, jadi tidak cocok untuk kotak Create ML.
- [8] tidak menyebut 18 landmark dan tidak menunjukkan cara aplikasi membaca pose saat berjalan, jadi tidak cukup sendirian untuk kartu Same Extractor.

> Kalimat siap ucap: "[8] untuk pengaturan training, [10] untuk cara aplikasi membaca pose saat berjalan."

### Apa kata Apple (WWDC20 [1], transkrip dicek)
| Kutipan | Dipakai di slide 8 untuk |
|---|---|
| "each action has around 50 videos. That's what you should aim for" | Panduan ~50 klip per kelas |
| "built Action Classification to learn from human body poses… works best with human body actions" | Alasan memakai Action Classifier |
| "encode the position of 18 landmarks… x and y coordinates, plus a confidence" | 18 titik tubuh |
| "keep the average frame rate consistent between your training and testing videos" | 30 fps yang sama |
| "try to make all actions roughly at the same duration" | Satu rep per klip (±2,75 s) |
| "The model does need to understand exercises, but it also needs to understand when we're not exercising, or not moving at all" | Kelas Other wajib ada (kartu "An Other Class Is a Must") |
| "If you collected some examples of resting, stretching, and sitting down, you could put all these together in a single folder called 'other'" · "plus two negative classes: 'other' and 'none'" | Panduan kelas negatif / Others (langkah Label [1]) |

**Jawaban jujur soal jumlah data:** 140 klip per kelas **sudah termasuk** klip augmentasi. Jadi jangan bilang "3× lipat dari saran Apple". Apple juga menyarankan "a variety of different people", dan jumlah subjek FitRep masih terbatas; itu sebabnya "record more people" ada di roadmap.

### Kurva validasi per iterasi
10 → 36,2% · 20 → 75,1% · 30 → 60,8% · 40 → 89,3% · 50 → 84,0% · 60 → 98,7% · 70 → 93,8% · 80 → 94,3%.
Kurvanya naik-turun karena set validasi kecil dan berasal dari data training yang sama. Angka 98,7% di iterasi 60 **bukan** bukti model lebih baik (lihat T-B7 di §13).

### Tiga file model di proyek
| File | Status |
|---|---|
| `BicepCurl.mlmodel` | **Dipakai** — FitRep(Best Model), md5 `871158…`, 8 Sep 2026 |
| `BicepCurl_100%.mlmodel` | Kandidat 12 Sep 2026, tidak dipakai. **[CEK]** Siapkan alasan tidak dipakai. |
| `BicepCurl_3000.mlmodel` | Model lama 4 kelas (dengan Idle), tidak dipakai |

Ketiganya ikut ter-*bundle* ke aplikasi (±8 MB tambahan). Ini bisa dibersihkan nanti.

---

## 7. Hasil dan evaluasi

### Validasi per kelas (Create ML)
| Kelas | Precision | Recall | F1 |
|---|---|---|---|
| Benar | 85,7% | **100%** | 0,92 |
| Others | 100% | 96,4% | 0,98 |
| Salah | 100% | **85,7%** | 0,92 |

### Testing-2 (confusion matrix; baris = label asli, kolom = prediksi)
| Asli \ Prediksi | Benar | Others | Salah |
|---|---|---|---|
| Benar | 60 | 0 | 0 |
| Others | 5 | 60 | 0 |
| Salah | 7 | 0 | 53 |

Cara membacanya:
- **Semua 12 kesalahan mengarah ke "Benar".** Tidak ada rep benar yang ditolak, tapi 7 rep salah dan 5 Others lolos sebagai benar.
- Precision `Benar` = 60/72 = **83%** → dari semua yang diprediksi benar, 17% sebenarnya bukan.
- Inilah kesalahan yang **mahal**: menyebut rep buruk sebagai baik. Karena itu ayunan dan ROM divonis oleh **geometri, yang tidak bisa dibantah model**.
- Header Testing-2 mencatat 180 item, tetapi tabel evaluasi berisi 185 (Others 65). Kemungkinan isi folder berubah setelah evaluasi.

### Evaluasi tiga level (ide kunci: akurasi model ≠ akurasi produk)
| Level | Apa yang diukur | Status |
|---|---|---|
| **L1 · Model** (window) | Akurasi Create ML → G2 | **SELESAI.** Berikutnya: uji dengan orang yang belum pernah direkam |
| **L2 · Sistem** (rep) | Vonis per repetisi: 3–5 subjek × 2 set × 10 rep (5 bersih, 5 salah), dilabeli orang kedua dari video. Metrik: precision (benar), recall (salah), MCC [17], akurasi hitung → G2 | **DIRENCANAKAN** |
| **L3 · Perangkat** | Inferensi median (Xcode Performance Report [12]), latensi end-to-end (rekam 240 fps), thermal 6 set × 15 rep, App Privacy Report → G1, G3 | **SEBAGIAN.** Yang sudah terverifikasi: tidak ada kode jaringan |

Rencana tambahan:
- **Ablation:** vonis geometri saja vs hybrid pada rep yang sama. Selisih recall "salah" = kontribusi nyata ML.
- **Batas latensi:** moving average 5 frame menambah jeda (5−1)/2 = 2 frame ≈ **67 ms** di 30 fps. Ini batas bawah, belum termasuk Vision, perpindahan ke main thread, dan audio.

---

## 8. Logika vonis repetisi (inti sistem)

File: `Domain/RepetitionTracker.swift`. Pelajari bagian ini sampai bisa menggambarnya di papan tulis.

### Per frame
1. **Pilih sisi badan:** bahu kiri atau kanan, mana yang confidence-nya lebih tinggi (yaitu sisi yang menghadap kamera). Jadi geometrinya tidak terikat pada satu sisi.
2. **Wajib terdeteksi:** bahu, siku, pergelangan dengan confidence > 0,4. Kalau tidak, frame dilewati.
3. **Hitung di koordinat piksel** (bukan ternormalisasi) supaya rasio aspek benar:
   - **Sudut siku** = sudut bahu–siku–pergelangan.
   - **Kemiringan lengan atas** = garis bahu→siku terhadap vertikal.
   - **Kemiringan badan** = garis pinggul→bahu terhadap vertikal (bila pinggul terdeteksi).
4. Ketiganya dihaluskan dengan **moving average 5 frame**.

### "Average the last 5 frames" (slide 10) — penjelasan lengkap

**Untuk apa?** Deteksi pose Vision tidak sempurna. Walau lengan diam, titik siku bisa sedikit melompat antar-frame, sehingga sudutnya ikut naik-turun sendiri. Tanpa penghalusan, satu frame yang meleset bisa dikira rep mulai atau badan berayun. Yang dihaluskan adalah **hasil deteksi pose**, bukan getaran kamera.

**Cara kerjanya: kotak berisi 5 angka terakhir**
- Setiap frame baru, sudut terbaru **dimasukkan** ke kotak.
- Bila isi kotak lebih dari 5, angka **paling lama dibuang**.
- Semua angka di kotak dirata-ratakan → sudut inilah yang dipakai aplikasi.
- Kotak ini "bergeser" mengikuti waktu (istilahnya *moving average*): rata-rata baru dihitung di **setiap frame**, bukan per blok 5 frame. Jadi 75 frame = **75 rata-rata**, bukan 15.

**Contoh tepat setelah set dimulai (kotak masih kosong):**

| Frame | Isi kotak | Rata-rata |
|---|---|---|
| 1 | 160 | 160° |
| 2 | 160, 158 | 159° |
| 3 | 160, 158, 162 | 160° |
| 4 | 160, 158, 162, 159 | 159,75° |
| 5 | 160, 158, 162, 159, 161 | 160° ← kotak penuh |
| 6 | ~~160~~ 158, 162, 159, 161, 157 | 159,4° ← angka paling lama dibuang |

Frame 1–4 tetap dirata-ratakan dari angka yang ada saja. Mulai frame 5, selalu dari 5 angka terakhir.

**Kenapa ini mencegah rep palsu?** Lengan diam ±150°, tapi frame 3 meleset jadi 138°. Tanpa rata-rata, 138° < 140° → rep dikira mulai. Dengan rata-rata (150, 152, 138, 151, 149) → **148°**, tetap di atas 140° → tidak ada rep palsu.

**Kapan dihitung dalam satu rep 2,5 detik (75 frame)?** Di **setiap frame**, 75 kali. Kejadian rep hanya dicatat saat sudut rata-rata melewati batas:

| Waktu | Frame | Sudut rata-rata | Kejadian |
|---|---|---|---|
| 0,0 s | 1 | ±160° | lengan lurus |
| ±0,2 s | ±6 | < 140° | rep **mulai** |
| ±1,1 s | ±33 | < 75° | **puncak** |
| ±2,5 s | ±75 | > 145° | rep **selesai**, vonis langsung |

Contoh di dekat puncak: sudut asli frame 29–33 = 82, 78, **74**, 70, 66. Di frame 31 sudut asli sudah < 75°, tapi rata-ratanya belum. Di frame 33 rata-ratanya (82+78+74+70+66) ÷ 5 = **74°** → puncak dicatat. Selisih 2 frame inilah jeda **±67 ms** (rumusnya (5 − 1) ÷ 2 = 2 frame di 30 fps).

**Kapan kotaknya dikosongkan?** Hanya saat sesi direset (set baru dimulai atau setelah jeda), supaya sudut dari sebelum jeda tidak memicu rep palsu. Di antara rep **tidak** dikosongkan, jadi frame pertama sebuah rep sudah memakai rata-rata 5 frame.

**Bukan hanya sudut siku:** kemiringan badan dan lengan atas juga dirata-ratakan 5 frame, lalu dicek di setiap frame terhadap batas ayunan (12° dan 35°). Karena itu gerbang "Body Swing?" cukup terpicu **sekali**, tanpa takut satu frame meleset.

**Jangan tertukar dengan window AI** — keduanya terpisah, hanya sama-sama memakai "beberapa frame terakhir":

| | Rata-rata sudut (lajur Angles) | Window AI (lajur AI) |
|---|---|---|
| Tugas | Menghaluskan angka sudut | AI menilai form |
| Isi | 5 sudut terakhir | 90 frame terakhir (3 detik) |
| Dihitung | Setiap frame (30×/detik) | Setiap 10 frame (±3×/detik) |
| Dalam rep 75 frame | 75 rata-rata | ±7 tebakan |
| Hasil | Satu sudut halus → dibandingkan 140°/75°/145° | Benar / Salah / Others + keyakinan |

Analogi: rata-rata sudut seperti melirik **speedometer** terus-menerus tapi tidak tertipu jarum yang bergetar; window AI seperti **instruktur** yang tiap sepertiga detik menonton ulang rekaman 3 detik terakhir. Keduanya baru bertemu di kotak **Result**.

Di kode: `MovingAverage.swift` (tambah angka, buang yang terlama bila > 5, bagi dengan jumlah isi) dan `smoothingWindowSize = 5` di `RepetitionTracker.swift`.

> Kalimat siap ucap: "Pose detection is a bit noisy frame to frame, so I average the last 5 frames before using the angle. One bad reading can't trigger a false rep, and it only adds about 67 milliseconds of delay."

### State machine
```
lowered ──siku < 140°──▶ (rep dimulai) ──siku < 75°──▶ raised ──siku > 145°──▶ lowered (rep selesai)
                              │
                              └── siku > 145° tanpa pernah < 75°  → rep dibatalkan diam-diam
```
- Rep selesai harus sudah mencapai fleksi penuh **dan** durasinya 1,0–6,0 s. Kalau tidak → `repetitionRejected` (tanpa suara).
- Rep yang berjalan > 6 s (misal sendi hilang) → `repetitionTimedOut`.

### Kenapa mulai di 140° tapi selesai di 145°?
**Masalahnya:** kamera tidak membaca sudut siku dengan sempurna. Walaupun lengan diam, angkanya bisa naik-turun 1–2°.

**Kalau mulai dan selesai memakai angka yang sama (misal 140°)**, lengan yang diam di sekitar 140° akan terbaca seperti ini:

| Sudut terbaca | Yang terjadi |
|---|---|
| 139° | Rep **mulai** |
| 141° | Rep **batal** (lengan dianggap lurus lagi) |
| 139° | Rep **mulai** lagi |
| 141° | **Batal** lagi… |

Aplikasi bingung terus, padahal lengan tidak bergerak. Waktu mulai rep (untuk durasi 1–6 s) juga jadi kacau.

**Solusinya: dibuat jarak 5°.**
- Rep **mulai** saat siku turun **di bawah 140°**.
- Rep **selesai** saat siku naik **di atas 145°**.
- Di antara 140°–145° **tidak terjadi apa-apa**; goyangan kecil di daerah ini diabaikan.

**Contoh:** lengan diam terbaca 142°, 141°, 143° → tidak ada yang terjadi. Baru saat Anda benar-benar menekuk sampai 139° → rep mulai. Setelah rep selesai di atas 145°, rep berikutnya baru bisa mulai kalau siku turun lagi di bawah 140°.

**Kenapa angka selesai yang lebih tinggi?**
- **Selesai di 145°:** lengan harus benar-benar lurus dulu sebelum rep dianggap lengkap (gerakan penuh, sejalan dengan kartu "Full Movement? [3]").
- **Mulai di 140°:** begitu siku sedikit menekuk, rep langsung tercatat, sehingga waktu rep dihitung sejak awal gerakan.

**Jujur:** 140°, 145°, dan jarak 5° **dipilih dari uji coba**, bukan dari literatur, dan belum dikalibrasi dengan data berlabel. Di kode: `repetitionStartAngle = 140`, `fullExtensionAngle = 145` (`RepetitionTracker.swift`). Istilah teknisnya *histeresis*.

> Kalimat siap ucap: "I use two thresholds, 140° to start and 145° to finish. The 5° gap means small wobbles in the angle reading can't make a rep start and stop over and over."

### Hubungannya dengan "Average the last 5 frames"
Keduanya menangani masalah yang sama (angka sudut dari kamera sedikit bergoyang), tapi di **dua tahap berbeda**:
- **Tahap 1 · rata-rata 5 frame → menenangkan angkanya.** Menghilangkan loncatan besar sesaat. Contoh: 150, 152, **138**, 151, 149 → rata-rata **148°**.
- **Tahap 2 · jarak 5° (140°/145°) → menenangkan keputusannya.** Menangani goyangan kecil yang tersisa, misalnya sudut rata-rata 140,4° → 139,6° → 140,3° tepat di sekitar batas.

| Kalau hanya memakai… | Yang masih bisa salah |
|---|---|
| Rata-rata 5 frame saja | Sudut di sekitar 140° tetap bisa membuat rep mulai–batal berulang |
| Jarak 5° saja | Satu frame yang meleset jauh (misal 130°) langsung melewati batas → rep palsu |
| **Keduanya** | Angkanya halus dulu, lalu keputusan diambil dengan jarak aman |

Hal yang sama berlaku untuk ayunan: kemiringan dirata-ratakan dulu, baru dibandingkan dengan 12° dan 35°.

> Kalimat siap ucap: "Two layers handle noisy pose readings: a 5-frame average smooths the angle, and a 5° gap between the start and finish thresholds keeps the decision from flipping."

### Kenapa tidak 140° dan 150°?
Bisa saja, tapi tidak ada angka yang pasti "ideal". **145° dipilih sebagai titik tengah.**

| | 145° (sekarang) | 150° |
|---|---|---|
| Jarak dari batas mulai | 5° | 10°, lebih tahan goyangan |
| Seberapa lurus lengan harus kembali | Hampir lurus | Harus lebih lurus lagi |
| Risiko utama | Rep yang belum benar-benar lurus mungkin sudah dianggap selesai | Rep yang sebenarnya sudah lurus bisa **tidak pernah dianggap selesai** |
| Kapan vonis keluar | Sedikit lebih awal | Sedikit lebih lambat |

**Kenapa 150° berisiko?**
- Lengan lurus **tidak selalu terbaca 180°**. Dari kamera samping, lengan yang terlihat lurus bisa terbaca ±145–165°, tergantung bentuk lengan, posisi kamera, dan letak titik siku dari Vision.
- Banyak orang sengaja **tidak mengunci siku** sampai lurus penuh di posisi bawah (otot tetap tegang, sendi aman).
- Kalau lengan seseorang "mentok" terbaca 148°, batas 150° tidak pernah tercapai → rep menggantung lalu dibatalkan setelah 6 detik → gerakan yang benar tidak dihitung.

**Apakah jarak 5° cukup?** Setelah rata-rata 5 frame, sisa goyangan angka sekitar 1–2°, jadi jarak 5° sudah lebih besar dari goyangan itu. Jarak 10° menambah keamanan yang tidak terlalu dibutuhkan, dengan risiko menolak rep yang benar.

**Catatan:** PRD versi lama mencatat 150° (selesai) dan 60° (puncak), sedangkan kode sekarang 145° dan 75°. Kemungkinan angka itu pernah dicoba lalu dilonggarkan setelah uji coba (dugaan; konfirmasi ke tim).

**Cara menentukan angka terbaik (bagian evaluasi L2):** rekam beberapa orang melakukan curl yang benar, catat sudut siku **terbesar** saat lengan di bawah. Bila hampir semua > 155°, batas 150° aman; bila banyak yang hanya 146–150°, 145° lebih tepat.

> Kalimat siap ucap: "145° is a compromise. A higher finish threshold would be more robust to noise, but some people's straight arm only reads around 145 to 150° from the side, so their clean reps would never finish. I'll calibrate it with labeled recordings."

### Empat gerbang vonis (urutan penting)
| # | Gerbang | Aturan | Hasil |
|---|---|---|---|
| 1 | **Range of Motion** [3] | Mulai < 140°, puncak < 75°, selesai > 145°, dalam 1,0–6,0 s | Gagal → ditolak, tanpa suara |
| 2 | **Veto ayunan** | Kemiringan badan berubah > 12° sejak awal rep, **atau** lengan atas > 35° dari vertikal | Salah (`swinging`) |
| 3 | **Veto ML** | Dari window yang yakin (≥ 0,60) dan overlap ≥ 30 frame rep ini: ≥ 2 window Salah, **atau** Salah > Benar | Salah (`poorForm`) |
| 4 | **Terima bawaan** | Kinematika bersih dan tidak ada bukti "Salah" yang yakin | Benar |

Nama di slide 10 (versi bahasa awam sejak 14 Okt 2026). Judul slide: **"Angle Rules Decide; AI Can Only Veto"** (dulu "Kinematics Decide; ML Holds a Veto").

| Di slide 10 | Artinya | Istilah teknis / di kode |
|---|---|---|
| **Angles**: Measure elbow and body angles → Average the last 5 frames → Track the rep: down → up → down | Hitung sudut siku dan badan, haluskan getaran, lacak lengan turun–naik–turun | Kinematika: `PoseGeometry` → rata-rata 5 frame (`MovingAverage`) → *state machine* lowered → raised → lowered |
| **AI**: Watch the last 3 seconds → On-device AI rates the form → Keep only confident guesses | Tiap 10 frame, AI melihat 3 detik terakhir dan menebak; hanya tebakan yakin yang dipakai | `PoseWindow` 90 frame → `BicepCurl.mlmodel` (Core ML) → confidence ≥ 0,60 dan overlap ≥ 30 frame |
| **1 · Full Movement? [3]** | Rep **mulai** saat siku < 140°, lalu menekuk penuh (< 75°) dan lurus lagi (> 145°) dalam 1–6 s. Durasi dihitung dari 140° sampai 145°; rep 2+ hanya bisa mulai setelah rep sebelumnya selesai (> 145°). Selisih 140°/145° = zona penyangga agar sudut yang bergoyang di sekitar batas tidak memicu mulai–selesai berulang | Range of Motion → gagal = *rejected* |
| **2 · Body Swing?** | Badan miring > 12° atau lengan atas > 35° | Veto ayunan → `incorrect(.swinging)` |
| **3 · AI Says Wrong?** | Hanya tebakan yakin (≥ 60%) yang melihat ≥ 1 detik rep ini. Salah jika **2+ "wrong"** atau **lebih banyak "wrong" daripada "right"** (dua poin terpisah di slide) | Veto ML → `incorrect(.poorForm)` |
| **4 · Otherwise: Good** | Lolos semua → dihitung | Terima bawaan (*default accept*) → `correct` |
| **Good rep · counted** / **Bad form · not counted** / **Not a valid rep · ignored** | Benar & dihitung / salah & tidak dihitung / bukan rep sah, tanpa bunyi | correct / incorrect / rejected |
| **Why not let AI count reps?** | Tebakan AI tumpang-tindih, tidak punya awal-akhir yang jelas | Sliding window 90 frame, stride 10 [7] |

Detail bukti ML:
- Kelas `Others` diabaikan sebagai bukti.
- Window dengan pose asli < 60% tidak diprediksi; frame tanpa orang diisi nol.
- Vonis langsung diberikan saat siku melewati 145° (`verdictDelayFrames = 0`).

### Fakta penting: status "inconclusive" tidak pernah tercapai
Kode punya `.inconclusive`, tapi dengan aturan di atas tidak ada kombinasi yang menghasilkannya:
- Kalau Salah = 0 → Benar (gerbang 4).
- Kalau Salah ≥ 1 dan Benar = 0 → Salah > Benar → Salah.
- Kalau Salah = 1 dan Benar ≥ 1 → Benar.

Akibatnya sistem **tidak pernah bilang "tidak yakin"**. Ini sudah diakui di deck sebagai *technical debt*.

### Kenapa ML hanya diberi hak veto?
- Model tidak punya kelas Idle terpisah (klip diam ada di Others). Window yang berisi **campuran** frame diam dan awal gerakan tidak mirip klip mana pun, sehingga jarang terbaca "Benar", sehingga ML bisa menolak rep bersih.
- Kesalahan model satu arah: ±1 dari 7 window salah terbaca benar.
- Window yang tumpang-tindih tidak memberi kejadian diskret.

Jadi ML hanya boleh **menolak**, dan hanya dengan bukti yakin yang berulang.

Inilah arti judul slide 10, **"Angle Rules Decide; AI Can Only Veto"**: rumus sudut yang memutuskan, AI hanya bisa membatalkan.

> Kalimat siap ucap (slide 10): "Rumus sudut yang memutuskan: kapan rep mulai, mencapai puncak, dan selesai, serta apakah badan berayun. AI hanya punya hak veto, dan itu pun hanya kalau ia berulang kali yakin gerakannya salah. Saya tidak membiarkan AI menghitung rep, karena tebakannya tumpang-tindih dan tidak punya awal-akhir yang jelas."

### Penjelasan langkah demi langkah (bagian yang paling sering membingungkan)

#### a. "Tebakan model" ≠ "vonis aplikasi"
| | Tebakan model | Vonis aplikasi |
|---|---|---|
| Berapa kali per rep | Banyak (±9 kali untuk rep 3 detik) | **Satu kali** |
| Kapan | Setiap 10 frame (±0,33 detik) | **Saat rep selesai** (siku lurus lagi > 145°) |
| Isinya | "Benar", "Salah", atau "Others" + confidence | BENAR (dihitung) atau SALAH (tidak dihitung) |
| Siapa yang memutuskan | Model ML | Aturan di `RepetitionTracker`, memakai tebakan yang lolos saringan + cek geometri |

Vonis **tidak menunggu** sejumlah frame tertentu. Waktu vonis ditentukan **geometri** (rep selesai). Tebakan model hanya dikumpulkan sebagai bukti selama rep berlangsung.

#### b. Tiga angka yang mudah tertukar
Semuanya dihitung dari window 90 frame (3 detik):

| Angka | Pertanyaan yang dijawab | Kalau tidak terpenuhi |
|---|---|---|
| **10 frame** (stride) | **Seberapa sering** model menebak? | — (jadwal tetap, ±3 kali per detik) |
| **54 frame** (60% dari 90) | Apakah model **boleh menebak**? Minimal 54 frame harus berisi orang yang terdeteksi | Model tidak menebak sama sekali |
| **30 frame** (overlap) | Apakah tebakan ini **melihat rep yang sedang dinilai**? Minimal 30 frame (1,0 detik) dari window berasal dari rep ini | Tebakan dibuang |

Ditambah satu saringan lagi: **confidence ≥ 0,60** dan label **Benar/Salah** (Others dibuang). Tebakan yang lolos semua saringan disebut **bukti**.

Jangan tertukar: "60%" (54 frame berisi orang) ≠ "0,60" (tingkat keyakinan tebakan).

#### c. Timeline satu rep 3 detik (90 frame)
```
Tebakan ke-:   1    2    3    4    5    6    7    8    9
Frame rep ini: 5   15   25   35   45   55   65   75   85   (dari 90 frame window)
Lolos 30?      ✕    ✕    ✕    ✓    ✓    ✓    ✓    ✓    ✓
```
- Tebakan 1–3 terbuang karena window-nya belum memuat minimal 30 frame gerakan aktif (masih didominasi frame diam/countdown).
- Tebakan ke-4 ke atas (frame 35 dst.) lolos saringan dan menjadi bukti jika confidence ≥ 0,60 dan bukan Others.
- Posisi pastinya bisa bergeser beberapa frame tergantung kapan rep dimulai; frame yang dibuang kamera juga bisa mengurangi jumlah tebakan.
- Rep 70 frame (2,3 detik) menghasilkan 7 tebakan, dengan 4–5 tebakan lolos saringan overlap 30 frame.

#### d. Kenapa 30 frame?
**30 frame ≈ 1,0 detik.** Window 90 frame lebih panjang daripada satu rep (±70–84 frame), sehingga window di awal rep masih berisi frame diam. Window **campuran** diam + awal gerakan seperti itu tidak mirip klip mana pun (klip Others yang diam penuh maupun klip Benar yang berisi satu rep utuh), jadi hampir selalu terbaca salah bila dibiarkan masuk.

| Ambang | Akibat |
|---|---|
| Terlalu kecil (misal 5–15) | Window masih didominasi frame diam dari hitung mundur → rep bersih awal berisiko disalahkan |
| Terlalu besar (misal 60) | Hanya 1–3 window lolos sebelum rep selesai. Rep tempo cepat (1,8–2,0 s) tidak punya bukti cukup → ML kehilangan hak vetonya |
| **30** | Ada 1,0 detik (sepertiga window) gerakan aktif nyata, dan masih menyisakan 4–6 window lolos per repetisi normal |

Kasus khusus di kode: kalau rep-nya sendiri lebih pendek dari 30 frame, syaratnya cukup seluruh rep masuk window. Sejak durasi minimum rep dinaikkan ke **1 detik** (= 30 frame), kasus ini hampir hanya terjadi bila kamera membuang frame yang terlambat.

Jujur: angka 30 dipilih **empiris**, memastikan setidaknya sepertiga jendela observasi adalah gerakan aktif pengguna. Kalibrasinya bagian dari evaluasi L2.

#### e. Kenapa 2 bukti Salah, tapi bukti Benar tidak wajib?
- **Bukti "Salah" ≥ 2:** satu tebakan "Salah" bisa saja kekeliruan model. Dua tebakan yang yakin dianggap konsisten → rep SALAH.
- **Bukti "Benar" ≥ 1:** aturan ini dicek **setelah** aturan Salah. Bahkan tanpa bukti Benar pun rep tetap BENAR kalau tidak ada bukti Salah sama sekali (terima bawaan).
- Alasannya: sistem sengaja **hati-hati sebelum menyalahkan pengguna**. Rep bersih yang disalahkan membuat pengguna tidak percaya aplikasi. Kesalahan paling penting (ayunan, ROM) sudah ditangkap geometri.

#### f. Contoh perhitungan vonis
Semua contoh mengandaikan ROM lengkap, durasi 1–6 s, dan tanpa ayunan:

| Bukti Benar | Bukti Salah | Vonis | Alasan |
|---|---|---|---|
| 3 | 0 | **BENAR** | Tidak ada bukti Salah |
| 5 | 1 | **BENAR** | Salah baru 1, tidak lebih banyak dari Benar |
| 4 | 2 | **SALAH** | Salah ≥ 2, walaupun Benar lebih banyak |
| 0 | 1 | **SALAH** | Salah (1) > Benar (0) |
| 0 | 0 | **BENAR** | Tidak ada bukti → terima bawaan |

Contoh lengkap satu rep, 6 tebakan lolos saringan 30 frame (tebakan ke-4 s.d. 9):

| Tebakan ke- | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|
| Label | Salah | Benar | Others | Benar | Salah | Benar |
| Confidence | 0,70 | 0,55 | 0,80 | 0,88 | 0,65 | 0,91 |
| Jadi bukti? | Salah ✓ | ✕ (< 0,60) | ✕ (Others) | Benar ✓ | Salah ✓ | Benar ✓ |

Bukti Benar = 2, Salah = 2 → Salah ≥ 2 → **SALAH**.

Rep juga **langsung SALAH tanpa melihat tebakan model** bila geometri mendeteksi ayunan (badan > 12° atau lengan atas > 35°).

#### g. Kalimat siap ucap
> "Vonis diberikan sekali, saat rep selesai. Selama rep berlangsung, model menebak setiap sepertiga detik. Tebakan dipakai sebagai bukti hanya kalau window-nya cukup berisi orang, cukup berisi rep ini (minimal 30 frame atau 1 detik), dan model yakin minimal 0,60. Rep dinyatakan salah kalau badan berayun, ada minimal dua bukti salah, atau bukti salah lebih banyak dari benar. Selain itu rep dinyatakan benar."

---

## 9. Rekayasa iOS

### Slide 11 dalam bahasa mudah ("30 Frames a Second, Without Freezing the Screen")
Slide 11 menjawab: **bagaimana 30 gambar per detik diproses tanpa macet, dan kodenya tetap aman?** Sejak 16 Okt 2026 teks slidenya memakai bahasa awam; judul lama "Swift 6 Pipeline, Zero Per-Frame Hops". Tabel padanan istilah lengkap ada di naskah bagian 08.

- **Swift 6 (subjudul: "Swift 6 rejects code where two tasks could change the same data at once"):** compiler memeriksa agar dua bagian program tidak mengubah data yang sama bersamaan (*data race*, penyebab crash acak). Diperiksa **sebelum** aplikasi jalan [13].
- **"Analyzed where the camera delivers it" / No Handoffs per Frame** (dulu *zero per-frame hops*): frame tidak dioper ke antrean lain lalu menunggu giliran; frame langsung diproses di antrean tempat kamera mengirimnya. Analogi: paket langsung diproses di meja tempat kurir meletakkannya.
- **Lapisan = restoran:** Screens (SwiftUI views) = ruang makan · Runner (ViewModel) = **pelayan** (hanya menjalankan perintah: suara, timer, kamera) · Decider (WorkoutSession) = **kepala dapur** (menerima kejadian → keadaan baru + daftar tugas) · Rules (Domain) = **buku resep** (rumus sudut, pelacak rep) · Tools (Services) = **peralatan dapur** (kamera, Vision, Core ML, audio).
- **One Place Decides (pure reducer):** kejadian yang sama selalu menghasilkan keputusan yang sama → bisa diuji tanpa iPhone. Jujur: test otomatisnya **belum ada**.
- **Tiga jalur kerja:**
  - **Camera lane** (`sessionQueue`): menyalakan kamera; `startRunning()` menunggu kamera siap, jadi dipisah supaya layar tidak membeku.
  - **Analysis lane** (`frameQueue`): Vision mencari sendi di setiap frame dan model Core ML menebak tiap 10 frame, langsung di sini.
  - **Screen lane** (`MainActor`): jalur layar; hasil tiap frame dikirim lewat AsyncStream (seperti ban berjalan).
- **Actor = ruangan berpintu satu:** hanya satu pekerjaan di dalamnya pada satu waktu → aman dari data race. Ruangan kamera dan pipeline ditaruh **di antrean kamera itu sendiri**, jadi frame masuk tanpa menunggu giliran (`assumeIsolated`).
- **Redraw Only on Change:** preview kamera langsung ke layar (bukan SwiftUI), dan keadaan layar hanya diganti kalau berbeda (`if session.state != state`). Tanpa ini, layar bisa digambar ulang 30 kali per detik → boros baterai.
- **1 documented exception** (`@unchecked Sendable`): satu-satunya "percaya saja, ini aman" jenis ini ke compiler, yaitu `ExerciseClassifier` (kelas model buatan Xcode). Aman karena propertinya tidak pernah berubah dan hanya dipanggil dari satu actor. Ada juga 2 `nonisolated(unsafe)` yang terdokumentasi (lihat di bawah), jadi label chip ini masih perlu diputuskan.
- **Bila ditanya "is there really no handoff?":** tidak ada perpindahan antrean saat frame **masuk** analisis. Hasil tiap frame tetap dikirim sekali ke MainActor lewat AsyncStream, karena hanya MainActor yang boleh mengubah tampilan.
- **Bila ditanya "apakah ke-30 frame selalu dianalisis?":** kamera mengirim hingga 30 fps dan membuang frame yang terlambat. Waktu Vision per frame (target < 33 ms) **belum diukur** (L3), jadi jangan klaim semua frame pasti dianalisis.

> Kalimat siap ucap: "Tiga puluh frame masuk setiap detik. Setiap frame dianalisis langsung di jalur tempat kamera mengirimnya, jadi tidak menumpuk, dan hanya hasil ringkasnya yang dikirim ke layar. Semua keputusan latihan ada di satu tempat yang bisa diuji tanpa iPhone, dan Swift 6 menolak meng-compile kode yang memungkinkan dua tugas mengubah data yang sama bersamaan."

Detail teknisnya ada di bawah.

### Lapisan (MVVM + Services + domain murni)
```
Features (SwiftUI views)
   ↓
WorkoutSessionViewModel   @MainActor @Observable — hanya MENJALANKAN efek
   ↓ send(Action)
WorkoutSession            reducer murni: Action → State + [Effect]  (tanpa timer/audio/kamera)
   ↓
Domain                    RepetitionTracker · PoseGeometry · StartPositionDetector · MovingAverage
Services                  CameraService · FramePipeline · PoseEstimator · ExerciseClassifier(+Loader)
                          WorkoutAnalysisService · AudioService
```
- **Reducer murni:** setiap `Action` (frame, timer tick, skip rest, dll.) mengubah state dan **mengembalikan daftar efek**: putar suara, mulai/stop timer, zoom, jeda kamera, log. ViewModel yang menjalankan efek itu.
  - Keuntungan: logika bisa diuji tanpa perangkat, dan efek samping eksplisit.
  - Status jujur: **belum ada test target otomatis.** Menurut PRD, saat refactor perilaku state machine dicek setara dengan versi lama lewat uji acak.

### Alur data per frame
```
Kamera depan (AVCaptureSession, antrean sessionQueue)
  → frame dikirim ke frameQueue
  → FramePipeline (actor yang executor-nya = frameQueue), masuk sinkron via assumeIsolated
      → PoseEstimator: Vision VNDetectHumanBodyPoseRequest → pose orang terbesar
      → PoseWindow: geser 90 frame, stride 10
      → ExerciseClassifier: prediksi Core ML (label + confidence)
  → FrameAnalysis (timestamp, pose, prediksi?)
  → AsyncStream → MainActor → ViewModel → WorkoutSession.send(.frameAnalyzed)
```

### Swift 6 concurrency — poin yang bisa dibanggakan
- **Actor dengan executor = dispatch queue:**
  - `CameraService` berjalan di `sessionQueue`; pola ini mengikuti sampel AVCam Apple. Hasilnya, `startRunning()` yang memblokir tidak pernah berjalan di main thread.
  - `FramePipeline` berjalan di `frameQueue`, antrean yang sama dengan delegate kamera.
- **Tanpa lompatan async per frame:** callback kamera masuk ke isolasi actor secara sinkron (`assumeIsolated`), tetapi compiler Swift 6 tetap memeriksa data race [13].
- **Hanya 1 `@unchecked Sendable`:** `ExerciseClassifier`, karena kelas model hasil generate Xcode tidak Sendable.
  - Kenapa aman: semua propertinya immutable, dan `predict` hanya dipanggil dari actor pipeline.
  - Ada juga 2 `nonisolated(unsafe)` yang terdokumentasi: `CMSampleBuffer` yang dipakai sinkron di antrean yang sama, dan preview layer.
- **`ExerciseClassifierLoader` (actor):**
  - Memuat model sekali untuk seluruh aplikasi; pemanggil berikutnya menunggu `Task` yang sama.
  - Kalau gagal, cache dibuang supaya sesi berikutnya bisa mencoba ulang.
- **Warm-up saat launch:** Vision dan model Core ML dikompilasi saat aplikasi dibuka, bukan saat pengguna menekan "Mulai". Pada run pertama, kompilasi bisa makan beberapa detik.

### Performa
- 720p (cukup untuk Vision dan lebih ringan dari 1080p), dibatasi 30 fps, frame terlambat dibuang (`alwaysDiscardsLateVideoFrames`).
- Preview kamera memakai layer langsung, tidak lewat SwiftUI.
- State hanya di-*assign* bila berubah, jadi 30 fps tidak memaksa SwiftUI render ulang. ViewModel memakai `@Observable`; properti internal ditandai `@ObservationIgnored`.

### Detail kamera dan audio yang sering ditanya
- **Kamera depan** dan *mirrored*: pengguna melihat layar sambil latihan.
- **`RotationCoordinator`:** frame untuk model selalu tegak terhadap gravitasi, dan preview mengikuti orientasi layar.
- Kamera **dijeda** saat istirahat dan dinyalakan **1 detik sebelum** istirahat selesai.
- **Interupsi** (aplikasi lain memakai kamera) ditangani: muncul pesan, lalu latihan lanjut otomatis.
- **Media services reset** ditangani dengan restart sesi.
- **Audio dua kanal:**
  - Kanal suara (panduan) dan kanal efek (chime/countdown) terpisah, jadi "Correct" tidak memotong panduan suara.
  - Player di-cache.
  - 0,18 s hening di awal file chime dilewati supaya bunyinya instan.
- Layar tidak meredup selama latihan (`isIdleTimerDisabled`).
- Panduan keamanan dibuat sebagai **overlay**, bukan `fullScreenCover`. Alasannya: presentasi modal bisa memicu `onDisappear`, yang akan menghentikan kamera dan timer.

---

## 10. Privasi, ketahanan, inklusivitas

### Privasi
- **Tidak ada kode jaringan sama sekali** (sudah dicek: tidak ada `URLSession`/`URLRequest`).
- Frame hanya ada di memori dan dibuang setelah pose diekstrak; tidak ada yang disimpan.
- Izin yang diminta hanya **kamera**. Deskripsi izinnya tersedia dalam bahasa Indonesia dan Inggris.

### Enam kasus kegagalan yang ditangani
| Kasus | Penanganan |
|---|---|
| Izin kamera ditolak | Pesan + tombol "Buka Pengaturan" |
| Interupsi kamera | Pesan, lanjut otomatis setelah kamera tersedia |
| Media server reset | Sesi kamera di-restart otomatis |
| Model gagal dimuat | Kamera tetap jalan, pesan ditampilkan, pemuatan bisa dicoba lagi di sesi berikutnya |
| Banyak orang di frame | Dipilih pose dengan kotak pembatas terbesar (orang terdekat) |
| Pose hilang | Guard confidence per sendi, timeout rep 6 s, window dengan pose < 60% tidak diprediksi |

Tambahan: kalau kamera tidak tersedia, juga muncul pesan.

### Inklusivitas
- String Catalog [14], bahasa sumber **Indonesia** + English, **91 string**.
- Panduan suara mengikuti bahasa sistem (file audio `_en`).
- Umpan balik selalu **memasangkan warna dengan suara**, jadi tidak bergantung pada warna saja [5].
- Geometri tidak terikat pada satu sisi badan.
- **Jangan klaim:** label VoiceOver dan haptic **belum ada**. Keduanya ada di roadmap "LATER".

---

## 11. Tiga kegagalan dan perbaikannya

### 1. Rep salah lolos sebagai benar
- **Gejala:** sebagian rep salah dinilai benar.
- **Akar masalah:** kelas terlemah model adalah Salah (recall ≈ 86%).
- **Respons:** gerbang ayunan dan ROM berbasis geometri memveto **secara independen** dari model [15].

### 2. Rep pertama hampir tidak pernah "Benar"
- **Gejala:** rep pertama setiap set jarang terbaca benar.
- **Akar masalah:** klip training dipotong pas satu rep (2,3–2,8 s), tapi window live 3 s di awal set masih berisi frame diam dari hitung mundur. Klip diam memang ada di Others, tapi window **campuran** diam + awal rep tidak mirip klip training mana pun.
- **Perbaikan:** window hanya dihitung sebagai bukti bila memuat **≥ 30 frame rep yang sedang dinilai** (1,0 detik). Posisi dihitung dalam frame, bukan detik, karena kamera membuang frame terlambat.

### 3. Rep bersih tanpa bukti
- **Gejala:** rep cepat atau rep pertama tidak mendapat satu pun window yang yakin.
- **Akar masalah:** terlalu sedikit window yakin yang overlap dengan rep pendek.
- **Perbaikan:** ROM penuh + tanpa ayunan + nol window Salah → **dinilai benar** (gerbang 4, "terima bawaan").

**Keterbatasan yang tersisa:** status "inconclusive" tidak tercapai, jadi sistem tidak pernah bilang "tidak yakin". Ini dicatat sebagai technical debt.

**Pesan utamanya:** setiap perbaikan dimulai dari akar masalah yang didiagnosis, bukan dari mengutak-atik ambang.

---

## 12. Keterbatasan yang harus dijawab jujur

1. **Data test bukan data "unseen".** Folder test berasal dari sumber rekaman yang sama dengan training. Jangan pakai kata *unseen* atau *held-out*. Uji dengan orang baru adalah langkah berikutnya [16].
2. **Data kecil dan diseimbangkan dengan augmentasi.** Variasi kecepatan dan salinan klip dipakai karena waktu rekaman terbatas. Jumlah subjek: **[ISI]**. Kalau ≤ 3 orang, akui sebagai keterbatasan.
3. **Semua metrik masih di level window.** Akurasi per repetisi (L2) dan metrik perangkat (L3) belum diukur.
4. **Ambang (12°, 35°, 0,60, 30 frame, durasi 1–6 s) dipilih secara empiris** saat uji coba. Ambang ini belum dikalibrasi dengan data berlabel; kalibrasinya bagian dari L2 dan ablation.
   - **Durasi minimum 1 detik** (dulu 0,8 s, dinaikkan 15 Okt 2026): keputusan desain, bukan dari literatur. Rep normal di data training 2,3–2,8 s; rep di bawah 1 detik hampir pasti mengandalkan entakan/momentum dan berisiko, jadi ditolak tanpa bunyi. Sitasi [3] di kartu "Full Movement?" mendukung **rentang gerak penuh**, bukan angka durasi ini.
   - **Batas atas 6 detik:** rep yang tidak selesai dalam 6 detik (misalnya sendi hilang dari kamera) dibatalkan supaya tidak menggantung.
   - Kalimat siap ucap: *"Satu detik adalah batas kewajaran yang saya pilih: rep normal saya 2,3–2,8 detik, dan rep yang lebih cepat dari 1 detik terlalu berisiko, jadi tidak dihitung. Angka ini belum dikalibrasi dengan data berlabel."*
   - **Jangan tertukar:** 1–6 detik = **lama satu rep** (gerbang 1). ≥ 30 frame (1 detik) = **berapa banyak rep ini yang terlihat dalam satu tebakan AI** (gerbang 3). Angkanya kini sama-sama 1 detik, tetapi fungsinya berbeda.
5. **"Inconclusive" tidak tercapai** (lihat §8).
6. **Hanya bicep curl.** Geometri sudah tidak terikat sisi, tapi model untuk lengan kiri **belum divalidasi**.
7. **Pembagian kerja CPU/GPU/Neural Engine belum diukur.** Jangan bilang "jalan 100% di Neural Engine". Sumbernya Xcode Performance Report, dan statusnya "TO MEASURE".
8. **Belum ada test target otomatis.** Desain reducer membuatnya mudah diuji, tapi tesnya belum ditulis.
9. **Nuansa saat model gagal dimuat:**
   - Pesan di layar berbunyi "repetisi tidak dapat dinilai".
   - Kenyataannya, sesi tetap menghitung rep dengan **geometri saja** (gerbang 1, 2, 4 tetap jalan; veto ML tidak ada).
   - Kalau ditanya, jelaskan apa adanya.
10. **Inkonsistensi dokumen yang perlu dibereskan sebelum presentasi:**
    - Deck FitRep 03 menulis "Pick **3–6 sets × 6–15 reps**". Kode (`WorkoutPlan.swift`) memakai **set 1–6 dan rep 6–20**, bawaan 3 × 12. Samakan salah satunya.
    - PRD v1.2 sudah usang di beberapa hal:
      - Swift 5 (kode: Swift 6).
      - Hanya lengan kanan (kode: sisi mana pun).
      - Posisi ditahan ±3 s (kode: 30 frame ≈ 1 s).
      - Ambang confidence 0,70 (kode: 0,60).
    - Angka thermal L3 "6 set × 15 rep" (§7) mengikuti rentang lama deck. Bila deck disamakan dengan kode (rep 6–20), kasus terberatnya menjadi 6 × 20.
    - Jika ditanya, jawab sesuai **kode**.

---

## 13. Bank pertanyaan dan jawaban

> Format: **T** = pertanyaan, **J** = jawaban ringkas yang bisa diucapkan. Latih dengan menutup bagian J.

### A. Produk dan masalah

**T-A1. Apa itu FitRep, dalam satu kalimat?**
J: Pelatih bicep curl di iPhone yang hanya menghitung repetisi bersih dan menandai ayunan badan atau gerakan tidak penuh, sepenuhnya on-device.

**T-A2. Untuk siapa aplikasi ini?**
J: Pemula yang latihan beban sendiri di rumah atau gym tanpa pelatih pribadi, dan belum yakin form-nya benar.

**T-A2b. Apa bukti bahwa masalah ini nyata?**
J: Wawancara kami dengan pemula gym. Mereka bilang sulit tahu apakah form-nya sudah benar, salah gerak bisa sakit, dan banyak yang bahkan bingung harus mulai dari mana. Pentingnya teknik juga ditegaskan literatur: teknik yang salah bisa melukai tendon dan otot, dan tidak membantu mencapai tujuan [2].

**T-A2c. Kenapa tidak cukup dengan cermin atau video?**
J: Cermin memperlihatkan gerakan, tapi tidak memberi tahu apa yang salah. Video satu arah dan bisa menyesatkan atau sulit diterapkan sendiri. Personal trainer memberi koreksi langsung, tapi mahal. Yang dibutuhkan pemula adalah panduan dan umpan balik langsung di setiap rep.

**T-A3. Kenapa bicep curl?**
J: Gerakannya terjadi di satu bidang, jadi terlihat jelas dari samping, dan kesalahannya bisa diukur dengan sudut sendi: sudut siku untuk ROM, kemiringan badan untuk ayunan. Itu membuatnya cocok sebagai latihan pertama sebelum push-up dan squat.

**T-A4. Apa bedanya dengan menonton video tutorial?**
J: Video bersifat satu arah dan tidak tahu apa yang Anda lakukan. FitRep memberi vonis setelah **setiap** repetisi Anda sendiri, dan hanya menghitung rep yang benar.

**T-A5. Kenapa umpan baliknya per repetisi, bukan terus-menerus?**
J: Akizuki dkk. [4] membandingkan umpan balik selama gerakan (terus-menerus) dengan umpan balik setelah gerakan. Umpan balik terus-menerus memang membuat latihan terlihat lebih bagus, tapi seminggu kemudian, tanpa umpan balik, hasilnya justru lebih buruk karena peserta jadi bergantung. Jadi FitRep memberi vonis setelah setiap rep selesai, dan itu juga tidak mengganggu fokus saat mengangkat beban.

**T-A5b. Kenapa umpan baliknya "Green pulse + chime" (warna + suara) [5]?**
J: Saat mengangkat beban, pengguna sering tidak melihat layar. Apple HIG bagian *Feedback* [5] menyarankan umpan balik lewat beberapa cara sekaligus (warna, teks, suara, haptic), agar tetap diterima ketika orang memalingkan pandangan dari layar, mematikan suara, atau memakai VoiceOver. Karena itu setiap vonis memasangkan warna dengan bunyi: hijau + chime = dihitung, merah + nada "Wrong" = tidak dihitung. Ini juga membuat informasinya tidak bergantung pada warna saja, yang penting bagi pengguna buta warna.
- Detail kode: kilatan warna tampil 1,2 detik. Chime diputar dengan melewati 0,18 detik hening di awal file, jadi terdengar seketika. Suara efek punya kanal sendiri, jadi tidak memotong panduan suara.
- Jujur: haptic tidak dipakai, karena ponsel diletakkan di penyangga, bukan dipegang. Label VoiceOver juga belum ada; keduanya di roadmap.

**T-A6. Kenapa kamera depan?**
J: Pengguna perlu melihat layar untuk umpan balik visual dan hitungan rep. Gambarnya di-*mirror* supaya terasa seperti cermin.

**T-A7. Kenapa harus berdiri menyamping?**
J: Gerakan curl terjadi di bidang samping tubuh, jadi sudut siku dan kemiringan badan paling akurat dilihat dari samping. Dari depan, lengan bawah bergerak maju mendekati kamera, sehingga sudut siku di gambar 2D hampir tidak berubah (*foreshortening*). Detektor posisi juga menolak posisi frontal: jarak horizontal kedua bahu harus ≤ 0,12 lebar frame.

**T-A8. Apakah aplikasi ini mencegah cedera?**
J: Saya tidak mengklaim itu, karena butuh validasi klinis. Yang saya klaim hanya hal yang bisa diukur: ROM penuh dan tidak ada ayunan.

### B. Machine learning dan data

**T-B1. Model apa yang dipakai?**
J: Create ML Action Classifier yang diekspor ke Core ML, 4,1 MB, 3 kelas. Inputnya 90 frame × 18 sendi × (x, y, confidence) dari Vision.

**T-B2. Apa arsitektur jaringannya?**
J: Apple tidak mendokumentasikannya. Yang bisa saya verifikasi dari spesifikasi Core ML: 27 lapisan konvolusi, 20 BatchNorm, 14 ReLU, komputasi Float16. Konsepnya sejalan dengan riset GCN atas kerangka [11], tapi saya tidak mengklaim detail yang tidak bisa saya cek.

**T-B3. Kenapa Create ML, bukan PyTorch atau TensorFlow?**
J: Datanya kecil dan waktunya terbatas. Kebutuhan utamanya juga on-device, dengan ekstraktor fitur yang **sama** antara training dan aplikasi (Vision). Create ML menyediakan itu langsung. Trade-off-nya: saya tidak bisa mengontrol arsitektur. Karena itu kelemahan model saya tutup di level sistem dengan veto geometri.
Tambahan dari WWDC20 [1]: Action Classifier memang dibuat untuk belajar gerakan manusia dari body pose, dan memprediksi per jendela waktu (urutan frame), cocok untuk repetisi latihan.

**T-B4. Bagaimana datanya dibuat?**
J:
- Rekaman video iPhone dari samping, dipotong satu repetisi per klip (2,3–2,8 s).
- Diberi label Benar, Salah, atau Others.
- Diseimbangkan dengan variasi kecepatan dan oversampling.
- Hasilnya 140 klip training dan 60 klip test per kelas.
- Apple menyarankan sekitar 50 klip per kelas [1]. Jumlah 140 klip FitRep sudah termasuk klip augmentasi, jadi sampaikan apa adanya.

**T-B5. Apa itu kelas Others?**
J: Kelas negatif (*negative class*) sesuai rekomendasi resmi Apple di WWDC20 [1] ("Build an action classifier with Create ML"). Apple menyarankan membuat folder `"other"` untuk menampung contoh gerakan seperti istirahat, peregangan, atau aktivitas non-target lainnya. Tujuannya adalah mencegah model salah mengklasifikasikan gerakan acak sebagai "Benar" atau "Salah". Di aplikasi FitRep, prediksi `Others` sengaja diabaikan sebagai bukti form bicep curl.

**T-B6. Kenapa satu rep per klip?**
J: Supaya label bersih: satu klip = satu vonis. Konsekuensinya, model tidak pernah melihat window **campuran** diam + awal rep (klip diam sendiri ada di Others), dan itu jadi akar masalah rep pertama (§11).

**T-B6b. Mengapa durasi klip 2,3–2,8 detik cocok dengan window 90 frame (3,0 detik)?**
J: Pada 30 fps, satu repetisi di klip training kami berdurasi 2,3–2,8 detik (≈ 70–84 frame). Window 90 frame (3,0 detik) dipilih agar **seluruh siklus repetisi (fase konsentrik dan eksentrik) tercakup utuh tanpa terpotong**. Jika window terlalu kecil (< 60 frame / 2 s), model kehilangan konteks lintasan gerak penuh; jika terlalu besar (> 120 frame / 4 s), responsivitas lambat dan window tercampur noise dari gerakan antar-repetisi. Selisih kecil frame diam di awal/akhir repetisi ditangani secara elegan oleh logika saringan overlap 30 frame (1,0 detik) di sistem hybrid kami.

**T-B7. Validasi di iterasi 60 mencapai 98,7%. Kenapa model akhirnya 94,3%?**
J: Model yang dipakai adalah hasil akhir training di iterasi 80. Set validasinya kecil dan dipisahkan otomatis dari data training yang sama, jadi kurvanya bergejolak (60,8% → 89,3% → 84,0% → 98,7%). Satu titik tinggi di data seberisik itu bukan bukti model lebih baik. Bukti yang lebih kuat adalah uji dengan orang baru. **[CEK]** Kalau Anda sengaja memilih model ini dibanding checkpoint lain, sebutkan alasan Anda.

**T-B8. Apakah model overfitting?**
J: Jarak training 96,9% dan validasi 94,3% kecil. Tapi saya tidak menganggap itu bukti generalisasi: validasi dan test berasal dari sumber rekaman yang sama, dan ada augmentasi berupa salinan klip. Angkanya kemungkinan optimistis. Karena itu langkah pertama di roadmap adalah uji dengan subjek yang belum pernah direkam [16].

**T-B9. Kenapa tidak pakai augmentasi bawaan Create ML?**
J: **[CEK alasan Anda.]** Alternatif jawaban: saya ingin mengontrol augmentasinya sendiri, yaitu variasi kecepatan yang sesuai dengan variasi tempo pengguna nyata.

**T-B10. Apa itu window dan stride?**
J: Window = 90 frame terakhir (3 detik di 30 fps) yang dinilai model sekaligus. Angka 90 frame dihitung otomatis oleh Create ML dari rumus resmi Apple [8]:
$$\text{30 fps (kecepatan frame kamera)} \times \text{3 detik (durasi aksi)} = \mathbf{90\text{ frame}}$$
Stride = window bergeser 10 frame, jadi ada prediksi baru tiap ±0,33 detik.

**T-B11. Kenapa confidence 0,60?**
J: Ambang yang sama untuk Benar dan Salah, supaya satu window Salah yang lemah tidak bisa membatalkan window Benar yang yakin. Nilainya dipilih empiris saat uji coba, dan akan dikalibrasi lewat evaluasi per repetisi.

**T-B12. Kenapa model tidak punya kelas Idle terpisah?**
J: Model lama punya kelas Idle dan OtherAction terpisah. Model aktif menggabungkan keduanya ke `Others` (klip diam + gerakan bukan curl), cara yang juga disebut Apple [1]. Yang masih menjadi masalah adalah window **campuran** diam + awal rep (lihat §11). Kondisi diam ditangani **geometri** (state machine tidak memulai rep sebelum siku < 140°) dan aturan overlap 30 frame. Rencana perbaikannya: latih ulang dengan klip yang memuat jeda diam.

**T-B13. Kenapa input kerangka lebih baik dari piksel di kasus ini?**
J: 54 nilai per frame dibanding 2,7 juta. Pakaian, cahaya, dan latar tidak ikut masuk ke classifier (walau deteksi pose Vision sendiri tetap bisa terpengaruh). Privat sejak awal. Dan formatnya sama persis antara training dan aplikasi.

### C. Evaluasi

**T-C1. Berapa akurasinya?**
J: Akurasi testing **94%** (173 dari 185, persisnya 93,5%; Create ML menampilkannya 94%). Sebagai pelengkap: training 96,9% dan validasi 94,3%. Semua itu akurasi **model** di level window, belum akurasi vonis per repetisi di aplikasi.

**T-C1b. Kenapa menonjolkan akurasi testing, bukan validasi?**
J:
- Data test berasal dari **folder terpisah** (Testing-2) yang tidak dipakai saat training.
- Set validasi dipisah otomatis secara acak dari folder training, yang berisi salinan klip dan versi klip yang dipercepat atau diperlambat. Klip yang hampir identik bisa masuk ke training dan validasi sekaligus, jadi angka validasi lebih optimistis.
- Akurasi training (96,9%) tidak dipakai karena diukur pada data yang dipakai untuk belajar.
- Test pun masih berasal dari sumber rekaman yang sama, jadi saya tidak menyebutnya *unseen*. Uji dengan orang baru adalah langkah berikutnya.

Catatan: header Testing-2 mencatat 180 item, tetapi tabel evaluasi berisi 185. Idealnya jalankan ulang **Evaluate** di Create ML.

**T-C2. Apa kelemahan terbesar model?**
J: Kesalahannya satu arah. Di test, ke-12 kesalahan semuanya memprediksi "Benar". Recall Salah di validasi 85,7%, artinya ±1 dari 7 window salah terbaca benar. Itu kesalahan yang mahal, makanya ayunan dan ROM divonis geometri.

**T-C2b. Kenapa angka utama dari testing, tapi grafik recall per kelas di slide 13 dari validasi?**
J: Untuk kelas Salah, recall validasi (85,7%) adalah angka yang lebih rendah, jadi lebih konservatif. Testing menunjukkan pola yang sama: Salah tetap kelas terlemah dengan recall 88,3% (53 dari 60), dan semua kesalahan mengarah ke Benar.
- Catatan: recall Others di testing 92,3% (60 dari 65) memakai tabel 185 item, sedangkan header Testing-2 mencatat 180 (lihat §7).

**T-C3. Apa beda precision dan recall di sini?**
J:
- **Recall Salah:** dari semua window yang benar-benar salah, berapa yang tertangkap (validasi 85,7%).
- **Precision Benar:** dari semua window yang diprediksi benar, berapa yang memang benar (test 83%).
- Untuk aplikasi pelatih, yang paling penting adalah **recall kelas salah**.

**T-C4. Kenapa merencanakan MCC?**
J: MCC memakai keempat sel confusion matrix dan tetap bermakna walau kelasnya tidak seimbang. Di level rep, jumlah rep bersih dan salah tidak akan seimbang [17].

**T-C5. Bagaimana mengukur akurasi produk?**
J: Protokol L2:
- 3–5 subjek × 2 set × 10 rep (5 bersih, 5 dengan kesalahan).
- Direkam, lalu dilabeli manual oleh orang kedua.
- Vonis aplikasi dibandingkan dengan label: akurasi hitung, precision Benar, recall Salah, MCC.

**T-C6. Berapa latensinya?**
J: Belum diukur. Rencananya merekam slow-motion 240 fps lalu menghitung frame dari siku lurus sampai kilatan hijau. Batas bawah teoretisnya ±67 ms dari moving average 5 frame, belum termasuk Vision dan audio. Saya tidak mau menyebut angka yang belum diukur.

**T-C7. Model jalan di Neural Engine?**
J: Core ML yang memutuskan pembagian lapisan ke CPU, GPU, dan Neural Engine [12]. Pembagian aktualnya bisa dilihat di Xcode Performance Report. Itu masih berstatus *to measure*, jadi saya tidak mengklaimnya.

**T-C8. Bagaimana membuktikan ML benar-benar berguna dibanding geometri saja?**
J: Lewat ablation: jalankan vonis geometri saja dan hybrid pada set rep yang sama. Selisih recall "salah" adalah kontribusi nyata ML. Ini sudah direncanakan, belum dijalankan.

### D. Rekayasa iOS

**T-D1. Jelaskan arsitektur aplikasinya.**
J: MVVM + Services di sekitar reducer domain yang murni.
- `WorkoutSession` menerima `Action` dan mengembalikan state baru plus daftar `Effect`.
- ViewModel `@MainActor` hanya menjalankan efek: audio, timer, kamera.
- Services membungkus AVFoundation, Vision, dan Core ML.

**T-D2. Kenapa memakai reducer murni?**
J: Semua transisi sesi bisa diuji tanpa kamera atau perangkat, dan efek samping jadi eksplisit. Jujur saja, test target-nya belum dibuat; desainnya sudah siap untuk itu.

**T-D3. Bagaimana frame diproses tanpa memblokir UI?**
J:
- Kamera berjalan di `sessionQueue`.
- Frame masuk ke `frameQueue`, yang sekaligus menjadi executor actor `FramePipeline`. Vision dan Core ML berjalan sinkron di sana.
- Main actor hanya menerima hasil ringkas (`FrameAnalysis`) lewat `AsyncStream`.

**T-D4. Apa maksud "analyzed where the camera delivers it" / "No Handoffs per Frame" (dulu "zero per-frame hops") di slide 11?**
J: Callback kamera sudah berada di antrean milik actor pipeline, jadi masuk ke isolasi actor bisa dilakukan sinkron dengan `assumeIsolated`. Tidak ada `await` atau perpindahan thread untuk setiap frame saat masuk analisis, tapi compiler Swift 6 tetap memeriksa data race. Hasil tiap frame tetap dikirim sekali ke MainActor lewat AsyncStream.

**T-D5. Kenapa ada `@unchecked Sendable`?**
J: Hanya satu, di pembungkus model, karena kelas yang di-generate Xcode tidak Sendable. Aman karena semua propertinya immutable dan `predict` hanya dipanggil dari actor pipeline. Alasannya didokumentasikan di kode.

**T-D6. Bagaimana mencegah UI render ulang 30 kali per detik?**
J: Preview kamera memakai layer langsung, tidak lewat SwiftUI. State hanya di-assign kalau nilainya berubah, karena meng-assign nilai yang sama pun tetap memicu render ulang. Properti internal ViewModel ditandai `@ObservationIgnored`.

**T-D7. Bagaimana menangani waktu muat model yang lama?**
J: Saat aplikasi dibuka, Vision dan model Core ML sudah di-warm-up, sehingga kompilasi pertama tidak terjadi saat pengguna menekan "Mulai". Loader-nya actor yang menyimpan satu `Task`, jadi model dimuat sekali dan pemanggil lain menunggu task yang sama.

**T-D8. Bagaimana kalau ada dua orang di depan kamera?**
J: `PoseEstimator` memilih pose dengan kotak pembatas sendi terbesar, biasanya orang paling dekat ke kamera.

**T-D9. Kenapa 720p dan 30 fps?**
J: Vision memperkecil frame sendiri dan sudut sendi tidak bergantung resolusi, jadi 720p cukup dan lebih ringan. 30 fps disamakan dengan frame rate data training, karena window 90 frame harus mewakili 3 detik.

**T-D10. Kenapa panduan keamanan bukan `fullScreenCover`?**
J: Presentasi modal bisa memicu `onDisappear` di layar latihan, dan itu akan menghentikan kamera serta timer. Overlay di dalam view yang sama menghindari masalah itu.

### E. Privasi, etika, aksesibilitas

**T-E0. Apa arti dan kegunaan "0 network calls" di slide FitRep 01?**
J: Artinya aplikasi **tidak punya kode jaringan sama sekali** (tidak ada `URLSession`/`URLRequest`). Seluruh proses (kamera → Vision → Core ML → vonis) berjalan di iPhone. Kegunaannya:
1. **Privasi:** video tubuh adalah data sensitif dan tidak mungkin terkirim ke server (design goal G3).
2. **Bisa dipakai tanpa internet:** tetap jalan di gym dengan sinyal buruk atau mode pesawat.
3. **Respons lebih cepat:** tidak ada jeda bolak-balik ke server, sehingga vonis bisa muncul setiap rep.
4. **Tanpa biaya server dan risiko kebocoran data:** tidak ada backend yang harus dirawat atau diamankan.
5. **Bisa dibuktikan:** lewat pemeriksaan kode, dan nantinya App Privacy Report di iPhone (FitRep seharusnya tidak punya domain jaringan).

**T-E1. Bagaimana privasi dijamin?**
J: Tidak ada kode jaringan sama sekali. Frame hanya ada di memori dan dibuang setelah pose diekstrak; tidak ada yang disimpan. Rencana verifikasinya lewat App Privacy Report: FitRep seharusnya tidak punya domain jaringan.

**T-E2. Aksesibilitas apa yang sudah ada?**
J: Umpan balik selalu memasangkan warna dengan suara, ada bahasa Indonesia dan Inggris untuk UI dan suara, dan aplikasi tanpa sentuh selama latihan. Label VoiceOver dan haptic **belum** ada; keduanya di roadmap.

**T-E3. Bias data?**
J: Datanya dari sedikit subjek (**[ISI]**), sehingga bisa bias terhadap postur, pakaian, atau jenis tubuh tertentu. Mitigasinya: input berupa kerangka (bukan piksel) dan uji dengan orang baru sebagai langkah berikutnya.

### F. Peran, tim, proses

**T-F1. Apa peran Anda?** **[CEK — sesuaikan dengan kenyataan]**
J (versi deck): ML & iOS Engineer. Saya membangun dataset, melatih classifier Create ML, dan merekayasa state machine repetisi serta pipeline Swift 6.

**T-F2. Berapa orang dan berapa lama?** **[CEK]**
J: 6 anggota, 4 minggu.

**T-F3. Keputusan teknis apa yang paling Anda banggakan?**
J: Memberi ML hanya hak veto. Model saya punya kesalahan satu arah, jadi saya tidak mengejar akurasi lebih tinggi; saya memastikan kesalahan paling mahal ditangkap aturan yang tidak bisa dibantah model.

**T-F4. Masalah paling sulit?**
J: Rep pertama yang hampir tidak pernah terbaca benar. Akarnya ketidakcocokan antara data training (klip pas satu rep) dan window live (masih berisi frame diam), ditambah window yang berisi campuran diam + awal rep (tidak mirip klip training mana pun). Solusinya aturan overlap 30 frame (1,0 detik) dan terima bawaan.

**T-F5. Apa yang berubah dari lamaran sebelumnya?**
J: Dulu saya menjelaskan tujuan; sekarang saya mendokumentasikan keputusan, kegagalan, dan cara mengukurnya. Kata seperti "inovatif" dan "injury prevention" saya ganti dengan komponen bernama dan angka terverifikasi.

**T-F6. Kalau punya 3 bulan lagi?**
J:
1. Uji dengan orang baru dan evaluasi per repetisi di perangkat.
2. Rekam lebih banyak orang supaya tidak bergantung pada augmentasi.
3. Latih ulang dengan klip yang memuat jeda diam.
4. Tampilkan alasan kesalahan ("badan terayun" vs "form kurang"); datanya sudah ada di `FormVerdict`.
5. Pulihkan status "tidak yakin".

Setelah itu: lengan kiri, push-up, squat, VoiceOver, dan haptic.

### G. Pertanyaan menjebak

**T-G1. "94% itu sudah bagus, kenapa masih butuh aturan geometri?"**
J: Karena 94% itu akurasi window, dan kesalahannya condong ke arah yang paling merugikan pengguna, yaitu rep salah dianggap benar. Aturan geometri menutup tepat kelemahan itu.

**T-G2. "Data test Anda unseen?"**
J: Tidak. Folder test berasal dari sumber rekaman yang sama, jadi saya tidak menyebutnya unseen. Uji generalisasi dengan orang baru adalah langkah pertama berikutnya.

**T-G3. "Kalau aturan geometri yang memutuskan, ML-nya sebenarnya tidak perlu?"**
J: Geometri hanya menangkap kesalahan yang bisa saya kodekan sebagai sudut, yaitu ayunan dan ROM. ML menilai kualitas gerakan secara holistik, termasuk pola yang tidak saya kodekan. Seberapa besar kontribusinya akan dijawab ablation, dan saya tidak mau mengklaim angka sebelum itu.

**T-G4. "Apa yang terjadi kalau model gagal dimuat?"**
J: Kamera tetap jalan dan pengguna diberi tahu. Secara teknis, sesi masih menghitung rep dengan geometri (ROM + ayunan) tanpa veto ML. Pesan di layar saat ini berbunyi "tidak dapat dinilai", dan itu perlu saya perjelas.

**T-G5. "Kenapa sistem tidak pernah bilang 'tidak yakin'?"**
J: Dengan aturan saat ini, status inconclusive secara logika tidak tercapai: tanpa bukti salah, rep yang ROM-nya lengkap diterima. Itu pilihan sadar demi rep pertama, tapi tercatat sebagai technical debt yang akan diperbaiki.

**T-G6. "Apakah ini ST-GCN?"**
J: Apple tidak mendokumentasikan arsitektur Action Classifier. Yang saya tahu pasti dari spesifikasinya: 27 konvolusi, 20 BatchNorm, 14 ReLU. Saya hanya mengklaim yang bisa saya inspeksi.

**T-G7. "Kenapa ambang ayunan 12°?"**
J: Dipilih empiris dari uji coba, agar gerakan napas atau goyangan kecil tidak dianggap ayunan. Belum dikalibrasi dengan data berlabel; kalibrasinya bagian dari protokol L2.

**T-G8. "Apakah sudah diuji di iPhone?"**
J: **[CEK]** Demo di iPhone sudah direkam (video 17 Jun 2026). Pengukuran terstruktur di perangkat (latensi, thermal, inferensi) belum dijalankan.

---

## 14. Glosarium

| Istilah | Penjelasan singkat |
|---|---|
| **ROM (range of motion)** | Rentang gerak sendi. Di sini: siku dari lurus (> 145°) ke menekuk penuh (< 75°) lalu lurus lagi |
| **Fleksi / ekstensi** | Menekuk / meluruskan sendi |
| **Momentum cheating** | Mengayun badan untuk membantu mengangkat beban, sehingga otot target bekerja lebih ringan |
| **Keypoint / landmark** | Titik sendi hasil Vision (x, y, confidence) |
| **Window** | Potongan 90 frame berurutan yang dinilai model sekaligus |
| **Stride** | Jarak geser antar-window (10 frame) |
| **Confidence** | Probabilitas kelas yang diprediksi model (0–1) |
| **Precision** | Dari yang diprediksi kelas X, berapa yang benar-benar X |
| **Recall** | Dari yang benar-benar kelas X, berapa yang tertangkap |
| **F1** | Rata-rata harmonik precision dan recall |
| **MCC** | Korelasi Matthews; memakai keempat sel confusion matrix, cocok untuk data tidak seimbang |
| **Confusion matrix** | Tabel label asli vs prediksi |
| **Data leakage** | Informasi dari data test "bocor" ke training (misal subjek/rekaman sama), sehingga skor terlalu optimistis |
| **Oversampling** | Memperbanyak contoh kelas dengan menyalin data |
| **Augmentasi** | Membuat variasi data (misal tempo lebih cepat/lambat) |
| **Ablation** | Mematikan satu komponen untuk mengukur kontribusinya |
| **State machine** | Model berupa status dan transisi (lowered → raised → lowered) |
| **Moving average** | Rata-rata beberapa nilai terakhir untuk meredam noise |
| **Reducer** | Fungsi murni: (state, action) → state baru + efek |
| **Effect** | Permintaan efek samping (suara, timer, kamera) yang dijalankan di luar reducer |
| **Actor** | Tipe Swift yang menjamin state-nya hanya diakses satu tugas pada satu waktu |
| **Executor** | Tempat kode actor dijalankan; di sini sebuah dispatch queue |
| **MainActor** | Actor untuk thread UI |
| **Sendable** | Penanda bahwa nilai aman dikirim antar-konteks concurrency |
| **AsyncStream** | Aliran nilai asinkron, di sini mengalirkan hasil frame ke UI |
| **Core ML / Vision / Create ML** | Menjalankan model on-device / deteksi pose dan gambar / melatih model tanpa kode |
| **Float16** | Presisi 16-bit untuk komputasi; lebih hemat memori dan energi |
| **Neural Engine (ANE)** | Akselerator ML di chip Apple |
| **Feature skew** | Fitur saat training berbeda dengan saat inferensi |

---

## 15. Peta file kode

| Kalau ditanya tentang… | Buka file |
|---|---|
| Ambang dan state machine rep | `MC3-FitRep/Domain/RepetitionTracker.swift` |
| Rumus sudut | `MC3-FitRep/Domain/PoseGeometry.swift` |
| Deteksi posisi menyamping | `MC3-FitRep/Domain/StartPositionDetector.swift` |
| Alur sesi (countdown, istirahat, set) | `MC3-FitRep/Domain/WorkoutSession.swift` |
| Pipeline per frame + `assumeIsolated` | `MC3-FitRep/Services/Analysis/FramePipeline.swift`, `WorkoutAnalysisService.swift` |
| Vision dan pemilihan orang terbesar | `MC3-FitRep/Services/Vision/PoseEstimator.swift` |
| Model, window, ambang 60% pose | `MC3-FitRep/Services/Classification/ExerciseClassifier.swift`, `PoseWindow.swift`, `ExerciseClassifierLoader.swift` |
| Kamera (720p, 30 fps, rotasi, interupsi) | `MC3-FitRep/Services/Camera/CameraService.swift` |
| Efek → audio/timer/kamera | `MC3-FitRep/Features/WorkoutSession/WorkoutSessionViewModel.swift` |
| Rentang set/rep | `MC3-FitRep/Domain/WorkoutPlan.swift` |
| Pesan kegagalan | `MC3-FitRep/Features/WorkoutSession/Components/CameraProblemView.swift` |
| Teks dwibahasa | `MC3-FitRep/Resources/Localizable.xcstrings` |

---

## 16. Referensi kunci

Hafalkan **apa yang didukung** tiap referensi, bukan detail bibliografinya.

| No. | Sumber | Mendukung klaim |
|---|---|---|
| [1] | WWDC20 · Build an action classifier with Create ML | Action Classifier on-device dari kamera; belajar dari body pose; ~50 klip per kelas; 18 landmark; frame rate konsisten (slide 4 dan 8) |
| [2] | Nae dkk., 2022 (*Marathon*) | Teknik yang salah bisa melukai tendon/otot dan tidak membantu mencapai tujuan (slide masalah) |
| [3] | Wolf dkk., 2025 (*PeerJ*) | Fase panjang-otot (lengthened partials) terkait adaptasi otot → alasan cek ROM |
| [4] | Akizuki dkk., 2025 | Waktu pemberian umpan balik memengaruhi motor learning → alasan vonis per rep |
| [5] | HIG · Feedback | Umpan balik memasangkan warna dan suara |
| [6] | Alomar dkk., 2025 (survei) | Model video end-to-end butuh data dan komputasi besar |
| [7] | Ullah dkk., 2025 | Penilaian gerakan real-time butuh penghitung/penanda rep terpisah |
| [8] | Creating an action classifier model (dokumentasi) | Detail teknis melatih classifier di Create ML (window, fps) |
| [9] | Sivakumar dkk., 2024 (*PeerJ Comput. Sci.*) | 70:30 dan 80:20 rasio train-test paling umum (kotak Split slide 8) |
| [10] | Detecting human actions in a live video feed (sampel Apple) | Keypoint Vision yang sama untuk training dan aplikasi |
| [11] | Ren dkk., 2024 (survei kerangka) | Dasar riset GCN untuk aksi berbasis kerangka |
| [12] | WWDC24 · Core ML on-device | Pembagian lapisan CPU/GPU/ANE; Performance Report |
| [13] | WWDC24 · Migrate to Swift 6 | Pemeriksaan data race oleh compiler |
| [14] | WWDC23 · String Catalogs | Lokalisasi ID/EN |
| [15] | Indykov dkk., 2025 | Taktik arsitektur sistem ML (aturan sebagai pengaman di sekitar model) |
| [16] | Rehman dkk., 2024 | Metode validasi HAR → perlu uji subjek baru |
| [17] | Stoica & Babu, 2024 | MCC untuk klasifikasi biner/multikelas |
| [18]–[23] | Sumber SIDIA | Benchmark Flutter, concurrency, cookie, Instruments, widget, SUS |

---

## 17. Checklist sebelum wawancara

- [ ] Isi semua **[CEK]/[ISI]** di dokumen ini: jumlah subjek, peran pribadi, jumlah anggota dan durasi, alasan tidak memakai `BicepCurl_100%`, alasan tanpa augmentasi bawaan.
- [ ] Samakan rentang set/rep antara deck ("3–6 × 6–15") dan kode (1–6 × 6–20).
- [ ] Latih pitch 30 detik dan 2 menit dengan suara keras (target: 30 detik ± 5).
- [ ] Bisa menggambar state machine dan 4 gerbang vonis tanpa melihat catatan.
- [ ] Siapkan demo: iPhone fisik, cahaya cukup, berdiri menyamping. Kamera dan Core ML tidak bisa diuji di simulator.
- [ ] Slide **20 · Watch & Try**: isi link TestFlight publik di tombol "Join the TestFlight beta" (klik kanan → Edit Link) dan footer `[ISI: public link]`, hapus penanda `[ISI: link]`, lalu ganti kotak "QR TestFlight [ISI]" dengan gambar QR. Buat QR dengan `swift make_qr.swift "<link>" qr-testflight.png`.
- [ ] Pastikan video demo YouTube (*FitRep: Real-Time Exercise Form Feedback*) berstatus Public/Unlisted, dan tautannya bisa diklik di PDF.
- [ ] Cocokkan tim dengan keynote 2024: "Meet the Team" menampilkan **5 anggota + 1 mentor** (deck menulis 6 members), dan peran Anda di sana tertulis **ML Modelling**. Sesuaikan kalimat My Role dengan bagian yang benar-benar Anda kerjakan.
- [ ] Hafalkan kalimat jujur untuk tiga hal: test bukan unseen, metrik masih level window, latensi belum diukur.
