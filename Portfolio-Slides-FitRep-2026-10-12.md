# FitRep — Naskah Deck Portofolio (16:9)
**Target:** Apple Developer Academy @ UC — AI/ML Institute Track · **Versi:** rev. 7.2 (2026-10-14) · **File:** `FitRep-Portfolio.pptx` (31 slide)

> Naskah ini **menggantikan** `Portfolio-Slides-FitRep.md` (rev. 5, 5 Okt 2026). File lama sengaja disimpan sebagai arsip.
> Isi di bawah disalin dari deck per 14 Okt 2026 (setelah suntingan pemilik pukul 06.29). Teks slide berbahasa Inggris; panduan dan catatan berbahasa Indonesia.
> Nama file masih bertanggal 12 Okt agar tautan lama tidak putus; isinya sudah rev. 7.

> **Legenda status data:**
> - ✅ terverifikasi dari kode, model, Create ML, atau dokumen asli
> - 🟨 `[ISI]` / `[CEK]` fakta yang hanya kamu ketahui
> - 🟦 `[UKUR]` wajib diukur di iPhone fisik

> **Sitasi:** gaya **IEEE**, nomor `[n]` global untuk seluruh deck (FitRep + SIDIA), urut kemunculan pertama.
> - Daftar [1]–[23] ada di slide 31 dan Lampiran D.
> - Satu nomor = satu sumber.
> - Aturan kurasi: publikasi berupa **artikel jurnal terbitan setelah 12 Okt 2023**, sumber lain hanya **dokumentasi resmi Apple**.
> - **Pengecualian sadar:** [2] Nae dkk. (2022), diambil dari keynote riset Academy 2024 (lihat Lampiran D).

> **Placeholder `[ISI]`** tampil oranye tua `#C93400` di PowerPoint. Cari dengan `Cmd+F "[ISI"`. Setelah diisi, ganti warnanya ke warna teks biasa.

---

## 0 · Alur Presentasi

Satu pesan per slide. Alurnya: masalah → solusi → proses & teknologi → hasil → evaluasi → kesimpulan → dampak → coba sendiri. Durasi ±18–20 menit untuk slide utama FitRep; lampiran disiapkan untuk tanya-jawab.

| # Deck | Kode | Label pill | Judul slide | Pesan utama |
|---|---|---|---|---|
| 1 | — | — | Cover *(teks dapat diedit, dibuat pemilik)* | Nama, peran, kontak (tautan aktif) |
| 2 | — | — | About Me *(teks dapat diedit, dibuat pemilik)* | Pendidikan, pengalaman ML, sertifikasi |
| 3 | — | — | Certifications & Training *(dibuat pemilik)* — **baru rev. 7.1** | Sertifikat Coursera, Dicoding, Binar/FGA |
| 4 | 01 | Project Snapshot | FitRep: On-Device Bicep Curl Coach | Apa yang dibangun, peran, hasil kunci |
| 5 | 02 | The Problem | Beginners Train Without a Feedback Loop | Bukti pengguna → kenapa teknik penting → kebutuhan |
| 6 | 03 | The Solution | Touchless Coaching, One Rep at a Time | Pengalaman tanpa sentuh + 3 design goal + video demo |
| 7 | 04 | Engineering Trade-offs | Three Architectures, One Deciding Question | Mengapa hybrid |
| 8 | 05 | Data Pipeline | Data: Balanced, Single-Rep, Side-View Clips | Data seimbang, satu rep per klip |
| 9 | 06 | ML Methodology | Why Skeletons Beat Pixels Here | Pose = input ringkas dan privat |
| 10 | 07 | The Hybrid Decision | Angle Rules Decide; AI Can Only Veto | Logika vonis 4 gate |
| 11 | 08 | iOS System Architecture | Swift 6 Pipeline, Zero Per-Frame Hops | Rekayasa iOS tingkat produksi |
| 12 | 09 | Robustness · Privacy · Inclusivity | Built for Messy Real-World Conditions | Robust, privat, inklusif |
| 13 | 10 | Model Results | Strong Scores, One-Sided Errors | 94%, tetapi galat condong ke satu arah |
| 14–15 | 10b | Create ML Evidence | Training & Validation in Create ML · Testing & Model Output | Bukti asli angka slide 13 — **dipindah dari lampiran (rev. 7.2)** |
| 16 | 11 | Error Analysis | Three Failures That Shaped the System | Diagnosis → perbaikan |
| 17 | 12 | Multi-Level Evaluation | Model Accuracy Is Not Product Accuracy | L1 selesai, L2–L3 terencana |
| 18 | 13 | Reflection & Roadmap | What I Learned, What Comes Next | Pelajaran + roadmap |
| 19 | 14 | App Exposure | Covered by Two Surabaya Newspapers | Liputan Jawa Pos dan Harian Bhirawa |
| 20 | 15 | Watch & Try | See It Work, Then Try It Yourself | Video demo + beta TestFlight |
| 21–30 | — | SIDIA 01–10 | Proyek lain *(naskah: `SiDia UNESA/portfolio/Portfolio-Slides-SIDIA-2026-10-12.md`)* | — |
| 31 | — | Appendix References | Daftar pustaka IEEE global [1]–[23] (1 halaman) | Pustaka |

> Sejak rev. 7, pill FitRep hanya berisi **nama bagian** (tanpa "FitRep 0X"). Kolom "Kode" di atas hanya untuk naskah ini. Pojok kanan atas slide FitRep kini berisi "Portfolio 1 of 3 · FitRep" (SIDIA: "Portfolio 3 of 3 · SiDia").

---

## 1 · Design System (PowerPoint)

Palet diambil dari cover dan About Me (slide 1–2) supaya seluruh portofolio satu gaya. Desainnya datar dan sederhana, mengikuti prinsip HIG: kejelasan, kontras cukup, warna dipakai hemat, dan makna tidak hanya disampaikan lewat warna.

| Elemen | Spesifikasi |
|---|---|
| Kanvas | 13,33 × 7,5 in (16:9) · margin 0,6 in · zona konten y 2,3–6,6 in · baris sumber y 6,85 in |
| Warna dasar | Latar putih `#FFFFFF` · kartu `#F2F2F2` · dekorasi `#ECEAEC` · teks `#000000` / `#3A3A3C` · teks sekunder `#6E6E73` |
| Aksen | Oranye `#FF751F` (theme accent1), sama persis dengan cover. Dipakai untuk pill seksi, sorotan, grafik, kolom "SELECTED", status NEXT |
| Oranye tua | `#C93400` (accent5 dan hyperlink). Dipakai untuk status salah/kesalahan, tautan, placeholder `[ISI]`, dan teks oranye kecil di latar terang. Kontras di atas putih 5,3:1 |
| Status | Selesai (DONE, VERIFIED, Correct, ✓) = hitam (accent4) · salah (Incorrect, ✕, Symptom) = `#C93400` · rencana (PLANNED, PARTLY VERIFIED, TO MEASURE) = `#6E6E73` (accent6) · berikutnya/terpilih = `#FF751F` |
| Font | Judul **Montserrat ExtraBold 30 pt** · judul kartu Montserrat Bold 13–15 pt (kartu aksi slide 20: 20 pt) · isi **Poppins 12–14 pt** · caption 10–11 pt · sumber 9 pt · References 8 pt |
| Motif | Pill oranye (`Nama Bagian`) · ikon putih dalam lingkaran hitam · ikon kutipan oranye · grid titik kiri atas · seperempat lingkaran abu kanan bawah · kartu abu polos; garis tepi oranye hanya untuk sorotan bukti (kutipan ahli slide 4) |
| Tombol (rev. 7) | Kapsul gaya iOS. **Utama:** isi gelap `#1C1C1E`, teks putih tebal. **Sekunder:** putih bergaris tepi gelap. **Tautan video:** kapsul putih bergaris `#D1D1D6` dengan ikon YouTube merah `#FF0000`. Satu aksi utama per kartu |
| Aturan | Status selalu dengan teks atau ikon (✓/✕), tidak hanya warna · maksimal ±40 kata isi per slide · sitasi IEEE `[n]` di teks slide, daftar lengkap di slide 31 · catatan pembicara di Notes |
| Kontras | Teks kecil minimal 4,5:1. **Pengecualian sadar:** pill teks putih di atas `#FF751F` (2,7:1) dipertahankan agar sama dengan cover. Tombol aksi baru (slide 20) sudah memakai kapsul gelap agar lolos kontras |
| Aset | Layar aplikasi = frame video demo FitRep dan screenshot aplikasi (iPhone fisik) · logo dari App Icon · tanpa logo Apple · ikon YouTube digambar ulang sebagai vektor (bisa diganti aset resmi YouTube Brand Resources) · ikon cermin digambar dengan gaya garis yang sama · screenshot Create ML dibiarkan apa adanya (bukti) |

---

## 2 · Slide

### 01 · Project Snapshot  *(deck 4)*
**Tujuan:** dalam 10 detik reviewer tahu apa yang dibangun, peran saya, dan bahwa hasilnya terukur.
- **Judul:** FitRep: On-Device Bicep Curl Coach
- **Subjudul:** Counts only clean reps and flags body swing — with just the iPhone camera, fully on-device [1].
- **Meta:** Group project · 6 members · 4 weeks · Portfolio 1 of 3
- **Chip:** Swift 6 · SwiftUI · Vision · Create ML · Core ML · AVFoundation · iOS 17.2+
- **Poin:**
  - **My Role — ML & iOS Engineer** — Built the dataset, trained the Create ML classifier [1], and engineered the rep state machine and Swift 6 pipeline.
  - **Hybrid by Design** — Geometry decides when a rep happens; a pose classifier decides whether its form breaks.
  - **Private by Default** — No networking code: frames are analyzed in memory, never stored or sent.
- **KPI:** **94%** testing accuracy · **4.1 MB** Core ML model, on-device · **0** network calls
- **Visual:** logo FitRep · layar beranda aplikasi · baris sumber "Screen: FitRep Homescreen".
- **Notes:** *"FitRep is a bicep curl coach that runs entirely on the iPhone. I owned the ML and the core engine: the dataset, the classifier, and the state machine that turns joint angles into rep verdicts."*
- **Sitasi:** [1] WWDC20 "Build an action classifier with Create ML" (subjudul dan My Role)
- **Data:**
  - ✅ KPI dari kode dan model: tidak ada kode jaringan, `BicepCurl.mlmodel` 4,1 MB, testing 94% (Testing-2, 173/185), sama dengan tampilan Create ML di slide 15.
  - ✅ KPI utama diganti dari *validation* ke **testing accuracy**: data test berasal dari folder terpisah, sedangkan validasi dipisah otomatis dari data training yang berisi salinan klip.
  - ✅ Semua slide memakai **94%**, sesuai angka yang ditampilkan Create ML (bukti di slide 15). Nilai persisnya 173/185 = 93,5%; sebutkan bila ditanya.
  - 🟨 Keynote Academy 2024 ("Meet the Team") menampilkan **5 anggota + 1 mentor**, dan peran pemilik tertulis "ML Modelling". Samakan meta "6 members" dan kalimat My Role dengan kenyataan.
  - 🟨 Periode proyek: keynote riset 1 Agu 2024, liputan koran Okt 2024, video demo Juni 2026, model aktif 8 Sep 2026. Siapkan penjelasan evolusinya.

### 02 · The Problem  *(deck 5)* — **disusun ulang rev. 7**
**Tujuan:** masalahnya nyata dan berbukti: suara pengguna → kenapa teknik penting → apa yang mereka coba sekarang → apa yang benar-benar dibutuhkan.
- **Judul:** Beginners Train Without a Feedback Loop
- **Subjudul:** Training alone, beginners can't tell if their form is right — and no one corrects them.
- **WHAT BEGINNERS TOLD US** (baris atas, kartu kutipan dengan ikon kutipan oranye):
  - "I struggle to tell whether my form is right or not…" — Kevin, gym beginner
  - "…If I move the wrong way, it's bound to hurt." — Weka, gym beginner
  - "At the gym, people usually don't know where to start." — Luist, gym beginner *(asli: "Di gym, biasanya orang itu dia bingung mulai dari mana."; diganti 14 Okt dari kutipan "setahun terbuang")*
- **WHAT THEY TRY TODAY** (kiri bawah, ikon dalam lingkaran hitam):
  - **Personal Trainer** — Live correction, but expensive — paid per session.
  - **Mirror** — Shows your form, but never tells you what's wrong.
  - **Online Videos & Articles** — Free, but can be misleading and hard to apply alone.
- **WHY TECHNIQUE MATTERS** (kanan bawah, kartu bergaris tepi oranye): "An incorrect technique can lead to hurting your tendons, ligaments and muscles. … A wrong technique will not help you achieve your goals." — Nae et al. [2]
- **COMMON MISTAKES:**
  1. **Swinging the Body** — The swing lifts the weight, not your arm. ✅ Salah ketik "he swing…" sudah diperbaiki (14 Okt 2026).
  2. **Incomplete ROM** — Arm never fully straightens [3]. (Opsi lebih awam: "Half Reps".)
- **Bar bawah:** **They need** Guidance on what to do, and live feedback on every rep. · **Challenge** How might we give coach-level feedback on every rep with only an iPhone?
- **Caption:** Quotes: our user interviews with gym beginners, translated from Indonesian.
- **Notes (3 paragraf):** pembuka persona Chris (berhenti PT karena biaya → latihan sendiri → masalah), lalu Nae dkk. + dua kesalahan, lalu PT/cermin/video → kebutuhan + Challenge.
- **Sitasi:** [2] Nae dkk. 2022, *Marathon* · [3] Wolf dkk. 2025, *PeerJ*
- **Data:**
  - ✅ Kutipan pengguna dicocokkan kata per kata dengan keynote asli `~/Downloads/Storytelling with Data.pdf` (1 Agu 2024). Teks asli bahasa Indonesia ada di Notes. Cadangan: Dylan, Jessica, Salsa.
  - ✅ Kutipan Nae dicek ke PDF asli (MARATHON vol. 14 no. 1, 2022, pp. 40–46, bagian 4.1 "Badly executed exercises"). Satu kalimat di tengahnya dilewati (…). Atribusi keynote "Cristina, et al." keliru; nama keluarga penulis **Nae**. DOI yang tercetak di paper tidak terdaftar, jadi References memakai URL resmi.
  - ✅ "Arm never fully straightens [3]": siku tidak lurus = posisi bisep teregang terlewat; [3] menunjukkan *lengthened partials* setara gerakan penuh. Nuansa: yang bermasalah bukan gerakan parsialnya, tapi posisi teregang yang terlewat.
  - ✅ Momentum sengaja tanpa sitasi (mekanika dasar). Ambang 12°/35°/75°/145° sengaja hanya di slide 10 (The Hybrid Decision). Istilah teknis "momentum cheating" cukup disebut lisan.
  - 🟨 Chris kemungkinan rekan tim Christian yang memerankan persona; konfirmasi. Konfirmasi juga jumlah responden dan izin menampilkan nama depan; bila ragu, ganti menjadi "Beginner A/B/C".
  - Jangan klaim FitRep mencegah cedera. Risiko cedera hanya alasan teknik itu penting; klaim FitRep adalah umpan balik teknik.

### 03 · The Solution  *(deck 6)*
**Tujuan:** pengalaman produk + tiga design goal yang menjadi tolok ukur slide berikutnya.
- **Judul:** Touchless Coaching, One Rep at a Time
- **Subjudul:** From positioning to the final set, the user never touches the screen.
- **Design goals:**
  - **G1 · Per-Rep Verdict** — Feedback after each rep, not a constant stream of cues [4].
  - **G2 · Form-Fault Detection** — Torso swing, elbow drift, and partial range on every rep.
  - **G3 · 100% On-Device** — Video is analyzed in memory; never saved, never sent.
- **Alur 4 langkah (dengan layar asli):**
  1. **Set the goal** — Pick 3–6 sets × 6–15 reps, then start.
  2. **Step into frame** — Hold side-on for 1 s → 5-second auto countdown.
  3. **Clean rep** — Green pulse + chime: counted [5].
  4. **Faulty rep** — Red pulse + tone: not counted. Each set ends with a skippable 30-second rest.
- **Baris sumber:** Screens: frames from the FitRep demo recording on iPhone
- **Tombol (kanan bawah, rev. 7):** ikon YouTube + **Watch the full demo on YouTube** → `youtube.com/watch?v=soQHorpnEWM`
- **Notes:** *"Every later slide is measured against these three goals. For G1, I chose per-rep instead of continuous feedback because motor-learning research shows constant cues create dependency."* Detail: kamera *zoom* halus ke 1,15× saat latihan dimulai.
- **Sitasi:** [4] Akizuki dkk. 2025, *Front. Psychol.* · [5] Apple HIG "Feedback"
- **Data:**
  - ✅ **[4] Akizuki dkk. 2025** (abstrak sudah dicek) — dasar memilih umpan balik **per rep**. Penelitian ini mengukur dua waktu yang berbeda: **saat latihan** (umpan balik masih diberikan) dan **tes seminggu kemudian** (tanpa umpan balik, untuk melihat apa yang benar-benar dipelajari).

    | Kelompok | Saat latihan | Tes seminggu kemudian |
    |---|---|---|
    | Umpan balik **selama** gerakan (info benar/salah tampil terus saat bergerak) | Paling sedikit salah | **Paling banyak salah** |
    | Umpan balik **setelah** gerakan selesai | Paling banyak salah | **Lebih sedikit salah** |

    > Catatan istilah: "umpan balik" = informasi seberapa benar gerakan pengguna, **bukan** gerakan contoh yang harus diikuti. Contoh di FitRep: "selama gerakan" = koreksi terus-menerus seperti "turunkan siku… tahan badan…" saat mengangkat; "setelah gerakan" = satu vonis hijau/merah setelah rep selesai (yang dipakai FitRep).

    - **Kenapa terbalik?** Saat latihan, kelompok "selama gerakan" terlihat bagus karena terus dibantu umpan balik. Begitu bantuan itu dihilangkan, mereka kesulitan karena sudah bergantung padanya. Kelompok "setelah gerakan" harus menilai gerakannya sendiri, jadi lebih banyak salah saat latihan, tapi justru lebih banyak belajar.
    - **FitRep** memberi vonis setelah setiap rep selesai, sesuai kelompok kedua: tujuannya agar pengguna benar-benar belajar gerakannya, bukan bergantung pada aplikasi.
    - Kelompok yang **memilih sendiri** kapan menerima umpan balik juga hasilnya baik di tes seminggu kemudian. Fitur ini belum ada di FitRep (ide pengembangan).
  - ✅ [5] (halaman HIG dicek): berikan umpan balik lewat beberapa cara (warna, teks, suara, haptic) agar tetap diterima walau pengguna memalingkan pandangan, mematikan suara, atau memakai VoiceOver. FitRep memasangkan warna + bunyi; haptic tidak dipakai karena ponsel di penyangga.
  - ✅ Dari kode: posisi ditahan 30 frame, countdown 5 s, zoom 1,15×, istirahat 30 s, warna tampil 1,2 s, chime melewati 0,18 s hening.
  - ⚠️ Rentang "3–6 sets × 6–15 reps" berbeda dengan kode `WorkoutPlan.swift` (set 1–6, rep 6–20, bawaan 3 × 12). Samakan salah satunya.

### 04 · Engineering Trade-offs  *(deck 7)*
**Tujuan:** arsitektur dipilih dengan membandingkan alternatif secara eksplisit.
- **Judul:** Three Architectures, One Deciding Question
- **Subjudul:** Who counts the reps — and who judges the form?
- **Tabel perbandingan** (bahasa awam sejak rev. 7.1):

| ✓ good · △ partly · ✕ poor | Angle Rules Only | AI Video Model Only | **Rules + AI · SELECTED** |
|---|---|---|---|
| How it works | Simple and free, but only catches mistakes we program as angle limits. | Judges the whole movement, but needs big data, heavy computing [6], and a separate counter [7]. | Angle rules count each rep; a small 4.1 MB AI model checks the form. |
| Counts each rep | ✓ | ✕ | ✓ |
| Judges overall form | △ | ✓ | ✓ |
| Training data needed | none | a lot | 140 clips per type |
| Explains mistakes | ✓ | ✕ | ✓ |

- **Keputusan (bar bawah):** Angles count the reps; AI checks the form.
  - Langsung menjawab subjudul ("Who counts the reps — and who judges the form?"). Alasannya disampaikan lisan: *"Counting a rep has a clear answer, so simple angle rules handle it. Judging whether the form is good needs more judgment, so that's where the AI comes in."*
- **Legenda (sel kiri atas tabel, sejak rev. 7.2):** ✓ good · △ partly · ✕ poor — warna simbol sama dengan isi tabel (✓ hitam, △ abu, ✕ oranye tua).
- **Caption:** A comparison based on published research, not our own test.
- **Notes:** *"I didn't let the AI model count reps, because it gives overlapping guesses, not a clear start and end for each rep. So the angle rules count the reps, where the answer is certain, and the AI only judges the form, where judgment is needed."*
- **Padanan istilah lama → baru:** Pure Geometry → Angle Rules Only · End-to-End Video Model → AI Video Model Only · Hybrid → Rules + AI · Approach → How it works · Discrete rep count → Counts each rep · Holistic form → Judges overall form · Explainable fault → Explains mistakes. Istilah teknis tetap boleh dipakai saat menjawab pertanyaan reviewer ML.
- **Sitasi:** dua klaim berbeda dalam satu sel, jadi dua sumber: [6] Alomar dkk. 2025, *Artif. Intell. Rev.* (model video butuh data dan komputasi besar) · [7] Ullah dkk. 2025, *Sci. Rep.* (contoh sistem penilai gerakan dengan penghitung repetisi berbasis sudut sendi; dukungan tidak langsung, inti klaim "butuh penghitung terpisah" adalah penalaran tim)
- **Data:** ✅ 4,1 MB dan 140 klip/kelas. Untuk bukti kuantitatif: *ablation* geometry-only vs hybrid pada set rep yang sama (Lampiran B).

### 05 · Data Pipeline  *(deck 8)*
**Tujuan:** proses data disiplin dan transparan, dengan angka asli.
- **Judul:** Data: Balanced, Single-Rep, Side-View Clips
- **Subjudul:** Each clip is one side-view rep for Create ML's Action Classifier, which learns movement from body pose [1].
  - Label Correct / Incorrect / Other tetap terlihat di grafik.
- **Pipeline:** Record — side-view iPhone clips → Trim — one rep per clip · 2.3–2.8 s → Label [1] — 3 target classes: Correct · Incorrect · Other → Create ML [8] — Vision pose · 30 fps · 90-frame window → Split [9] — common 70/30 split · train · validate · test
- **Grafik:** clips per class, 140 train + 60 test untuk Correct (Benar), Incorrect (Salah), Other (Others), total **420 train · 180 test**.
- **Poin:**
  - **An Other Class Is a Must** — The model must also learn when you're not exercising or idle [1], so non-curl moves go to Other, not a forced verdict.
  - **Balanced Classes, Augmented Clips** — 140 training and 60 test clips per class, including speed-varied and oversampled ones; Apple suggests about 50 [1].
  - **Same Extractor, Train and Runtime** — Create ML and the app read the same 18 Vision body landmarks at the same 30 fps [1], [10].
- **Notes:** *"Recording time was the bottleneck, so I balanced the classes with speed-varied and oversampled clips. I used Create ML's Action Classifier because it learns movement from body pose, and Apple's own guidance is about fifty clips per action. I also kept an Other class, because Apple says the model must also learn when you're not exercising — otherwise every movement is forced into Correct or Incorrect. Create ML and the app read the same 18 body landmarks at the same 30 frames per second, so there's no mismatch between training and use. Each class is split 70/30 between training and test, a common ratio."*
- **Sitasi:** [1] WWDC20 "Build an action classifier with Create ML" · [8] Creating an action classifier model · [9] Sivakumar dkk. 2024, *PeerJ Comput. Sci.* (70:30 dan 80:20 rasio train-test paling umum) · [10] Apple sample code "Detecting human actions in a live video feed" (alur `keypointsMultiArray()` → action classifier)
- **Bukti dari transkrip [1]** (dicek 14 Okt 2026, tombol *Transcript* di halaman sesi):

  | Kutipan transkrip | Mendukung |
  |---|---|
  | "each action has around 50 videos. That's what you should aim for" · "around 50 videos for each exercise… and you should, too" | Panduan ~50 klip per kelas (poin 1) |
  | "This year, we built Action Classification to learn from human body poses" · "powered by Vision's human body pose estimation… works best with human body actions" · prediksi pada "a time window that consists of a sequence of frames" | Alasan memakai Action Classifier (subjudul) |
  | "encode the position of 18 landmarks on the body… x and y coordinates, plus a confidence" | 18 titik tubuh, format 3 × 18 (poin 3) |
  | "keep the average frame rate consistent between your training and testing videos" | 30 fps yang sama saat training dan di aplikasi (poin 3) |
  | "try to make all actions roughly at the same duration" | Satu rep per klip, ±2,75 s (langkah Trim; disebut lisan) |
  | "The model does need to understand exercises, but it also needs to understand when we're not exercising, or not moving at all" · "You could create two extra folders: one for walking around and stretching, and one for sitting or doing nothing quietly" | Kelas Other wajib ada (kartu 1 "An Other Class Is a Must") |
  | "If you collected some examples of resting, stretching, and sitting down, you could put all these together in a single folder called 'other'" · "five exercises plus two negative classes: 'other' and 'none'" | Panduan kelas negatif/Other [1] (langkah Label [1]; kata "Other" diberi warna aksen) |
- **Data:**
  - ⚠️ **Jujur soal [9]:** paper itu hanya menyatakan 70:30 dan 80:20 *"the most common train-test splits in the literature"*; ia **tidak** menyebut 70:30 optimal, dan akurasinya cenderung naik bila porsi training lebih besar. Jangan klaim "ideal". Studinya memakai citra MRI, bukan urutan pose.
  - ✅ Sumber data: `~/Documents/New Model Cleaned - 140-60/`, Create ML 6.2, 80 iterasi, augmentasi bawaan nonaktif, klip 2,3–2,8 s.
  - ✅ **Asal angka 90 frame:** dihitung otomatis oleh Create ML berdasarkan rumus resmi Apple [8]:
  **Prediction Window Size = Frame Rate × Action Duration**
    Dengan pengaturan kamera iPhone sebesar 30 fps dan parameter **Action Duration** di Create ML sebesar 3 detik (sedikit di atas durasi rep 2,3–2,8 s, agar satu rep utuh muat), perhitungannya adalah:

    **30 frame/detik × 3 detik = 90 frame**

    Dengan demikian, ukuran *prediction window* yang digunakan di Create ML adalah **90 frame**.
  - ✅ **Mengapa 90 frame (3,0 s) dipilih padahal klip repetisi 2,3–2,8 s?**
    - Satu repetisi di klip training kami (fase konsentrik naik + eksentrik turun) berdurasi 2,3–2,8 detik (≈ 70–84 frame). Ini durasi rep di data kami, bukan durasi rep manusia pada umumnya; tempo pengguna nyata bisa berbeda (gerbang 1 menerima 1–6 s).
    - Window 90 frame (3,0 detik) sengaja dipilih sedikit lebih besar dari durasi repetisi agar **seluruh siklus repetisi dari awal lengan mulai menekuk hingga lurus sempurna tercakup utuh tanpa terpotong dalam satu jendela analisis**.
    - Jika window terlalu pendek (misal 60 frame / 2,0 s), gerakan terpotong di tengah dan model kehilangan konteks lintasan temporal secara penuh. Jika terlalu panjang (misal 150 frame / 5,0 s), latensi membengkak dan window akan didominasi noise frame antar-repetisi.
    - Selisih kecil frame diam/transisi di awal atau akhir repetisi (±6–20 frame) ditangani secara aman oleh sistem logika vonis repetisi lewat syarat ambang overlap minimal 30 frame (slide 10 & 16).
  - 🟨 Jumlah subjek: tulis di notes. Bila ≤ 3 orang, akui sebagai keterbatasan.
  - ✅ Isi kelas *Others* sudah didefinisikan: klip diam + gerakan bukan curl (lihat poin "Isi kelas Other" di bawah).
  - ✅ **Soal 18 vs 19 titik terjawab:** Vision mendeteksi hingga 19 titik, tetapi Action Classifier memakai **18 landmark** (x, y, confidence) — disebut langsung di transkrip [1]. Kalimat "18 Vision body landmarks" sudah tepat.
  - ⚠️ **Jujur soal 140 klip:** sudah termasuk klip augmentasi (variasi kecepatan + oversampling). Jangan klaim "3× panduan Apple". Apple juga menyarankan "a variety of different people", sedangkan jumlah subjek FitRep masih terbatas (roadmap: rekam lebih banyak orang).
  - ✅ **Isi kelas Other:** klip diam **dan** gerakan bukan curl, digabung dalam satu folder. Saran Apple dua folder memakai kata *"could"* (pilihan), dan Apple juga menyebut contoh diam/peregangan boleh disatukan *"in a single folder called 'other'"* [1]. Masalah yang tersisa hanya window **campuran** diam + awal rep (slide 16), ditangani aturan overlap 30 frame.

### 06 · ML Methodology  *(deck 9)*
**Tujuan:** Menjustifikasi secara ilmiah dan teknis mengapa klasifikasi berbasis kerangka tubuh (skeleton pose) jauh lebih unggul daripada memproses piksel gambar mentah, serta menyajikan klaim arsitektur model secara jujur dan berbasis bukti yang dapat diinspeksi.

- **Judul:** Why Skeletons Beat Pixels Here
- **Subjudul:** Pose sequences are compact, appearance-invariant, and private by construction.
- **Visual:** Graf spasial-temporal berisi 3 kerangka tubuh berurutan ($t - 1, t, t + 1$).
  - *Vertices* (simpul) = 18 titik sendi tubuh.
  - *Spatial edges* (garis tegas) = tulang yang menghubungkan sendi dalam 1 frame.
  - *Temporal edges* (garis putus-putus) = lintasan gerak sendi yang sama antar-frame dari waktu ke waktu.
- **KPI:**
  - **2,764,800 → 54** values per frame (≈ 51,200× smaller input)
  - **4,860** values per window (90 frames × 54 values ≈ 19 KB, Float32)
- **Poin Utama di Slide:**
  - **Graph-Shaped Signal** — Sendi bertindak sebagai *vertices*; tulang dan lintasan temporal frame-ke-frame bertindak sebagai *edges* — fondasi ilmiah dari riset skeleton Spatio-Temporal Graph Convolutional Networks (ST-GCN) [11].
  - **What We Can Verify** `VERIFIED` — Spesifikasi Core ML fisik: 27 konvolusi, 20 BatchNorm, 14 ReLU, komputasi Float16. Apple tidak mendokumentasikan arsitektur resminya.
  - **Hardware Placement** `TO MEASURE` — Core ML membagi eksekusi lapisan ke CPU, GPU, dan Apple Neural Engine (ANE) [12]; alokasi riil diukur lewat Xcode Performance Report.
- **Notes Lisan di Slide:** *"Pose sequences strip away clothing, lighting, and background, and they're a 51,000-fold smaller input. I only claim what I can inspect in the Core ML spec, because Apple doesn't document the classifier's internals."*
- **Sitasi:**
  - **[11] Ren dkk. 2024**, *Cyborg and Bionic Systems* — Survei komprehensif pengenalan aksi berbasis kerangka tubuh dan arsitektur spatio-temporal graph convolution.
  - **[12] WWDC24 Session 10161**, *"Deploy machine learning and AI models on-device with Core ML"* — Mekanisme runtime Core ML dalam mempartisi model ke CPU, GPU, dan Apple Neural Engine.

---

#### 💡 Panduan Mudah Memahami Slide 9 (Penjelasan Lengkap)

Slide ini menjawab pertanyaan paling mendasar dari reviewer/mentor AI/ML: **"Kenapa kalian tidak melatih model langsung dari video atau piksel kamera (seperti CNN/Vision biasa), melainkan lewat titik kerangka tubuh Vision?"**

##### 1. Tiga Alasan Utama: Mengapa Kerangka Jauh Lebih Unggul (*Why Skeletons Beat Pixels*)
1. **Input Jauh Lebih Ringkas (~51.200× lebih sedikit angka):**
   - Video mentah 720p berisi jutaan angka per frame, atau puluhan megabyte per detik bila tidak dikompresi.
   - Dengan mengekstrak koordinat sendi, input classifier menyusut menjadi hanya **19 KB per window 3 detik**.
   - Nuansa jujur: dampaknya ke memori dan baterai iPhone **belum diukur** (evaluasi L3), dan Vision tetap membaca gambar setiap frame. Jadi klaimnya "input lebih ringkas", bukan "hemat baterai".
2. **Jauh Kurang Terpengaruh Penampilan (*Appearance-Invariant*):**
   - Model berbasis piksel mudah tertipu oleh warna baju, motif pakaian, warna kulit, intensitas cahaya ruangan, bayangan, atau barang-barang di latar belakang kamar/gym.
   - Urutan kerangka tubuh **tidak membawa informasi visual tersebut** ke classifier; yang dinilai hanya pola gerak sendi.
   - Nuansa jujur: kualitas deteksi pose Vision sendiri tetap bisa dipengaruhi cahaya, pakaian longgar, atau latar yang ramai. Jadi klaimnya "jauh kurang sensitif", bukan "kebal".
3. **Privasi Alami Sejak Awal (*Private by Construction*):**
   - Aplikasi fitness sering digunakan di rumah atau kamar pribadi.
   - Dengan model skeleton, tidak ada rekaman wajah, bentuk tubuh, atau suasana ruangan pengguna yang disimpan atau diproses oleh model klasifikasi. Yang diolah murni hanya deretan angka koordinat $(x, y)$.

---

##### 2. Bedah Angka dan Rumus KPI (Dari Mana Angka Ini Berasal?)

| Metrik | Perhitungan Matematis | Hasil | Makna Teknis |
|---|---|---|---|
| **Piksel per Frame** | 1280 × 720 × 3 kanal RGB | **2.764.800 nilai kanal** | Jumlah nilai kanal warna pada satu frame gambar beresolusi HD 720p. |
| **Kerangka per Frame** | 18 titik sendi × 3 nilai (x, y, confidence) | **54 nilai** | Jumlah nilai numerik yang merepresentasikan 18 titik sendi hasil ekstraksi Apple Vision. |
| **Rasio Pengurangan Data** | 2.764.800 ÷ 54 | **≈ 51.200:1** | Representasi kerangka memiliki sekitar 51.200 kali lebih sedikit nilai numerik dibandingkan data piksel mentah. Ini merupakan perbandingan jumlah nilai, bukan pengukuran langsung pengurangan beban komputasi. |
| **Ukuran 1 Window (90 Frame)** | 90 frame × 54 nilai per frame | **4.860 nilai numerik** | Jumlah nilai masukan untuk satu *prediction window* berdurasi 3 detik pada 30 fps, dengan asumsi setiap frame menghasilkan 18 titik sendi dan 3 nilai per titik. |
| **Ukuran Memori Window** | 4.860 × 4 byte (Float32) | **19.440 byte (≈ 19 KB)** | Perkiraan ukuran data masukan untuk satu window jika seluruh nilai disimpan sebagai Float32, belum termasuk metadata, buffer, dan overhead pemrosesan. |

---

##### 3. Cara Membaca Visual Graf Spasial-Temporal ($t - 1, t, t + 1$)
Di slide terdapat visualisasi 3 pose tubuh berurutan pada waktu $t - 1$, $t$, dan $t + 1$:
- **Simpul (*Vertices*):** 18 titik sendi tubuh (bahu, siku, pergelangan tangan, pinggul, lutut, dll.).
- **Sisi Spasial (*Spatial Edges* / Garis Padat):** Tulang-tulang anatomi yang menghubungkan sendi dalam **frame yang sama** (misal menghubungkan bahu ke siku, siku ke pergelangan tangan). Ini merepresentasikan **postur tubuh** pada detik tersebut.
- **Sisi Temporal (*Temporal Edges* / Garis Putus-putus):** Menghubungkan titik sendi yang sama **dari frame sebelumnya ke frame berikutnya** (misal posisi siku di $t - 1 \to t \to t + 1$). Ini merepresentasikan **kecepatan, akselerasi, dan lintasan gerak** seiring berjalannya waktu.
- *Konteks Riset:* Representasi data berbentuk graf spasial-temporal inilah yang menjadi dasar riset modern aksi tubuh manusia (ST-GCN - Spatial-Temporal Graph Convolutional Networks, Ren dkk. 2024 [11]).

---

##### 4. Memahami 2 Label Status: `VERIFIED` vs `TO MEASURE`

Slide 9 menggunakan dua tag status evaluasi yang menunjukkan integritas ilmiah tim:

1. **Tag `VERIFIED` (Fakta Terbukti dari File Model Fisik):**
   - Saat file `BicepCurl.mlmodel` dibuka dan diinspeksi spesifikasinya (`coremltools` dan `xcrun coremlcompiler`), kita menemukan struktur lapisan internal: **27 lapisan Convolution, 20 Batch Normalization, 14 Activation ReLU**, 1 pooling, 1 softmax, dengan presisi komputasi **Float16**.
   - *Kenapa tidak diberi kutipan paper atau dokumen Apple?* Karena ini adalah **temuan data primer langsung** dari model kita sendiri. Apple tidak mendokumentasikan arsitektur internal Create ML Action Classifier. Kita tidak boleh mengarang nama arsitektur (jangan klaim "ini pasti ST-GCN"), melainkan hanya menyatakan fakta lapisan yang terbukti bisa diverifikasi secara fisik.
2. **Tag `TO MEASURE` (Jujur Soal Alokasi Perangkat Keras):**
   - Framework Core ML secara cerdas membagi beban komputasi model ke tiga unit prosesor di iPhone: **CPU, GPU, dan Apple Neural Engine (ANE)** [12].
   - *Kenapa statusnya TO MEASURE?* Karena alokasi persentase beban nyata di iPhone fisik (misal berapa persen di ANE vs GPU) harus diukur langsung melalui **Xcode Performance Report** saat iPhone dicolokkan ke Mac. Status `TO MEASURE` menunjukkan kejujuran bahwa pengukuran hardware profiling pada perangkat fisik adalah langkah lanjutan yang belum selesai dilakukan. **Jangan pernah mengklaim "100% berjalan di Neural Engine" tanpa bukti pengukuran nyata.**

---

##### 5. Script Siap Ucap (Untuk Presentasi & Tanya Jawab)

- **Kalimat Utama Pitching (Presentasi Slide 9):**
  > *"Di slide ini, kami menunjukkan mengapa kami memilih melatih model berbasis kerangka tubuh (skeleton pose) daripada piksel gambar kamera. Dengan memakai 18 titik pose Vision, input classifier menyusut sekitar 51.000 kali lipat—dari 2,7 juta angka piksel menjadi hanya 54 angka per frame, atau sekitar 19 Kilobyte per window 3 detik. Ini perbandingan jumlah angka, bukan hasil ukur beban komputasi: Vision tetap membaca gambar kamera, dan beban di iPhone baru akan diukur di evaluasi L3. Selain menjaga privasi pengguna, model ini jauh kurang sensitif terhadap warna pakaian, warna kulit, maupun pencahayaan ruangan. Terkait arsitektur internalnya, karena Apple tidak mendokumentasikan jeroan Create ML, kami memegang integritas ilmiah dengan hanya mengklaim apa yang kami verifikasi langsung dari spesifikasi file Core ML-nya: yaitu 27 konvolusi, 20 batchnorm, dan komputasi Float16."*

- **Bila Ditanya: "Arsitektur apa sebenarnya yang dipakai Create ML Action Classifier ini?"**
  > *"Apple tidak mendokumentasikan arsitektur resminya, baik di dokumentasi maupun di sesi WWDC. Namun, dari inspeksi langsung file `.mlmodel` yang kami ekstrak, model ini terdiri dari 27 lapisan konvolusi, 20 batchnorm, dan 14 ReLU yang memproses data urutan koordinat sendi. Secara konseptual, struktur data spatio-temporal graph ini sejalan dengan penelitian Spatio-Temporal Graph Convolutional Networks (ST-GCN) seperti pada paper Ren dkk. 2024, tetapi kami membatasi klaim kami hanya pada lapisan yang terverifikasi di file Core ML."*

- **Bila Ditanya: "Apakah model ini sudah 100% berjalan di Apple Neural Engine (ANE)?"**
  > *"Core ML secara otomatis membagi eksekusi lapisan ke CPU, GPU, dan Neural Engine. Di slide kami tandai 'TO MEASURE' karena alokasi pastinya harus diukur secara empiris di iPhone fisik melalui Xcode Performance Report saat profiling. Kami tidak ingin mengklaim '100% ANE' sebelum memiliki data pengukuran fisiknya."*

### 07 · The Hybrid Decision  *(deck 10)*
**Tujuan:** logika vonis yang sebenarnya, termasuk alasan ML hanya diberi hak veto.
- **Judul:** Angle Rules Decide; AI Can Only Veto
- **Subjudul:** Every rep passes four checks the moment the arm straightens again.
- **Alur dua lajur:**
  - **Angles:** Measure elbow and body angles → Average the last 5 frames → Track the rep: down → up → down
  - **AI:** Watch the last 3 seconds → On-device AI rates the form → Keep only confident guesses
  - **Result:** Good rep · counted (hitam) / Bad form · not counted (oranye tua) / Not a valid rep · ignored (abu)
- **Gate:**
  1. **Full Movement? [3]** — A rep starts when the elbow bends below 140°. It must bend fully (under 75°) and straighten again (over 145°) within 1–6 s. If not, the rep is ignored.
  2. **Body Swing?** — If the torso tilts more than 12° or the upper arm drifts over 35°, the rep is marked wrong.
  3. **AI Says Wrong?** — tiga poin:
     - Only confident AI guesses **(≥ 60%)** that **saw ≥ 1** second of this rep count.
     - **2+ “wrong”** → wrong.
     - **More “wrong” than “right”** → wrong.
  4. **Otherwise: Good** — Full movement, no swing, and no strong “wrong” signal from the AI → counted as a good rep.
- **Callout:** **Why not let AI count reps?** Its views overlap with no clear start or end [7] → reps counted twice or missed.
- **Notes:** *"Angle rules make the decision: when a rep starts, peaks and ends, and whether the body swings. The AI only gets a veto, and only when it is repeatedly confident the form is wrong. It can't approve reps on its own, because a 3-second view at the start of a set still contains standing-still frames."*
- **Istilah teknis di balik kata awam** (ada juga di notes slide, untuk tanya-jawab):

  | Di slide (awam) | Istilah teknis | Di kode |
  |---|---|---|
  | Angle rules / Angles | Kinematika: sudut sendi | `PoseGeometry` |
  | Average the last 5 frames | Rata-rata bergerak 5 frame (titik sendi hasil deteksi pose sedikit melompat antar-frame) | `MovingAverage` |
  | Track the rep: down → up → down | *State machine* lowered → raised → lowered (mulai < 140°) | `RepetitionTracker` |
  | Watch the last 3 seconds | PoseWindow 90 frame, ditebak tiap 10 frame, ≥ 54 frame berisi pose | `PoseWindow` |
  | On-device AI rates the form | ExerciseClassifier (Core ML) | `BicepCurl.mlmodel` |
  | Confident guesses · saw ≥ 1 s of this rep | Confidence ≥ 0,60 · overlap ≥ 30 frame | `minimumPredictionConfidence`, `minimumEvidenceOverlapFrames` |
  | Not a valid rep · ignored | *Rejected*: tidak dihitung, tanpa bunyi | `repetitionRejected` |
  | Full Movement? / Body Swing? / AI Says Wrong? / Otherwise: Good | Range of Motion / Swing Veto / ML Veto / Default Accept | `verdict(for:)` |
- **Sitasi:** [3] Wolf dkk. 2025 · [7] Ullah dkk. 2025. [7] menopang penghitungan repetisi berbasis kinematika; pernyataan bahwa jendela yang tumpang-tindih tidak punya awal/akhir adalah penalaran tim sendiri.
- **Data:** ✅ Semua ambang dari `Domain/RepetitionTracker.swift`; `verdictDelayFrames = 0`. "Rejected" = tidak dihitung, tanpa bunyi. Model menebak setiap 10 frame; window hanya diprediksi bila ≥ 54 dari 90 frame berisi pose. Penjelasan langkah demi langkah ada di `Belajar-FitRep.md` §8.

---

#### 💡 Panduan Mudah Memahami Slide 10

Slide ini menjawab: **"Siapa yang memutuskan sebuah rep dihitung atau tidak: rumus sudut atau AI?"**

##### 1. Arti judul: "Angle Rules Decide; AI Can Only Veto"
- **Angle rules** = rumus sudut dan posisi sendi (siku, badan, lengan atas). Istilah teknisnya *kinematika* (ilmu gerak); judul lama slide ini "Kinematics Decide; ML Holds a Veto".
- **Rumus sudut yang memutuskan**: mulai, puncak, dan selesainya rep, serta apakah badan berayun.
- **AI hanya punya hak veto**: AI boleh **membatalkan** rep yang terlihat salah, tetapi **tidak bisa mengesahkan** rep sendirian.
- Analogi: rumus sudut adalah **wasit** yang menghitung, sedangkan AI adalah **asisten video** yang hanya boleh berkata "batalkan", dan hanya kalau buktinya kuat.

##### 2. Dua jalur yang berjalan bersamaan
| Jalur | Isi | Bahasa mudahnya |
|---|---|---|
| **Angles** | Sudut sendi → rata-rata 5 frame → *state machine* turun → naik → turun | Hitung sudut siku setiap frame, haluskan agar tidak bergetar, lalu lacak lengan: turun, naik, turun lagi = satu rep |
| **AI** | Window 90 frame → model Core ML → bukti form | Setiap 10 frame, AI melihat 3 detik terakhir dan menebak Benar / Salah / Others beserta tingkat yakinnya |

Hasil akhirnya hanya tiga:
- **Good rep · counted**: benar, dihitung, pulse hijau dan bunyi.
- **Bad form · not counted**: salah, tidak dihitung, pulse merah dan bunyi.
- **Not a valid rep · ignored**: bukan rep yang sah, diabaikan tanpa bunyi.

**Kotak "Average the last 5 frames" — apa maksudnya?**
- **Untuk apa:** deteksi pose sedikit melompat antar-frame walau lengan diam. Satu frame yang meleset bisa dikira rep mulai atau badan berayun. Yang dihaluskan adalah hasil deteksi pose, bukan getaran kamera.
- **Cara kerja:** bayangkan kotak berisi **5 sudut terakhir**. Setiap frame, sudut baru masuk, yang paling lama dibuang, lalu isinya dirata-ratakan. Rata-rata dihitung di **setiap frame** (bergeser 1 frame), jadi rep 75 frame = **75 rata-rata**, bukan 15.
- **Contoh:** lengan diam ±150°, satu frame meleset 138°. Tanpa rata-rata → 138° < 140° → rep palsu. Dengan rata-rata (150, 152, 138, 151, 149) → **148°** → aman.
- **Dalam rep 2,5 s (75 frame):** mulai (rata-rata < 140°) ±frame 6 · puncak (< 75°) ±frame 33 · selesai (> 145°) ±frame 75. Rata-rata membuat kejadian tercatat ±2 frame lebih lambat (**±67 ms**).
- **Kapan kotak dikosongkan:** hanya saat set baru dimulai atau setelah jeda (di frame 1–4 setelahnya, rata-rata memakai angka yang ada saja). Di antara rep tidak dikosongkan.
- **Juga untuk kemiringan badan dan lengan atas**, sehingga gerbang "Body Swing?" cukup terpicu sekali.
- **Jangan tertukar dengan window AI:** rata-rata sudut = 5 frame, dihitung tiap frame (75× per rep), menghasilkan sudut halus. Window AI = 90 frame (3 s), dijalankan tiap 10 frame (±7× per rep), menghasilkan tebakan benar/salah. Keduanya terpisah dan baru bertemu di kotak **Result**.
- **Di kode:** `MovingAverage.swift`, `smoothingWindowSize = 5`. Penjelasan lengkap dengan tabel: `Belajar-FitRep.md` §8.
- **Bila ditanya:** *"Pose detection is a bit noisy frame to frame, so I average the last 5 frames before using the angle. One bad reading can't trigger a false rep, and it only adds about 67 milliseconds of delay."*

##### 3. Empat gerbang, dicek berurutan saat lengan kembali lurus
| Gerbang | Pertanyaannya | Kalau gagal |
|---|---|---|
| **1 · Full Movement? [3]** | Apakah gerakannya lengkap? Siku mulai menekuk (< 140°), naik penuh (< 75°), turun sampai lurus (> 145°), dalam 1–6 detik | **Ditolak** tanpa bunyi. Kalau lengan lurus lagi sebelum sampai puncak, rep dibatalkan diam-diam |
| **2 · Body Swing?** | Apakah badan berayun? Kemiringan badan berubah > 12°, atau lengan atas miring > 35° dari tegak | **Salah** (tanpa melihat AI) |
| **3 · AI Says Wrong?** | Apakah AI yakin rep ini salah? Hanya tebakan yang yakin (≥ 0,60) dan cukup melihat rep ini (≥ 30 frame) yang dihitung. Salah ≥ 2 kali, atau Salah lebih banyak dari Benar | **Salah** |
| **4 · Otherwise: Good** | Lolos semua gerbang dan tidak ada bukti Salah yang kuat? | **Benar**, dihitung |

Contoh: rep 2,5 detik, siku 160° → 60° → 160°, badan tegak. AI memberi 4 tebakan Benar dan 1 Salah. Salah belum 2 kali dan tidak lebih banyak dari Benar → **Benar, dihitung**.

*"The moment the arm straightens again"* artinya vonis keluar **saat itu juga** begitu siku > 145°, tanpa jeda (`verdictDelayFrames = 0`).

**Kenapa mulai di 140° tapi selesai di 145°?**
- Kamera tidak membaca sudut siku dengan sempurna. Walaupun lengan diam, angkanya bisa naik-turun 1–2°.
- Kalau mulai dan selesai memakai angka yang sama (misal 140°), lengan yang diam di sekitar 140° akan terbaca **"mulai, batal, mulai, batal"** terus-menerus.
- Karena itu dibuat **jarak 5°**:
  - Rep **mulai** saat siku turun **di bawah 140°**.
  - Rep **selesai** saat siku naik **di atas 145°**.
  - Di antara 140°–145° **tidak terjadi apa-apa**; goyangan kecil di daerah ini diabaikan.
- **Contoh:** lengan diam terbaca 142°, 141°, 143° → tidak ada yang terjadi. Baru saat Anda benar-benar menekuk sampai 139° → rep mulai.
- **Kenapa angka selesai lebih tinggi?** Supaya lengan benar-benar lurus dulu sebelum rep dianggap selesai (gerakan penuh, sejalan dengan [3]).
- **Jujur:** 140° dan 145° dipilih dari uji coba, belum dikalibrasi. Di kode: `repetitionStartAngle = 140`, `fullExtensionAngle = 145`. Istilah teknisnya *histeresis*. Penjelasan dengan tabel: `Belajar-FitRep.md` §8.
- **Bila ditanya:** *"I use two thresholds, 140° to start and 145° to finish. The 5° gap means small wobbles in the angle reading can't make a rep start and stop over and over."*

**Hubungannya dengan "Average the last 5 frames"**
- Keduanya menangani angka sudut yang bergoyang, di dua tahap: **rata-rata 5 frame menenangkan angkanya** (loncatan sesaat 138° → 148°), lalu **jarak 5° menenangkan keputusannya** (goyangan kecil di sekitar 140° diabaikan).
- Rata-rata saja → sudut di sekitar 140° masih bisa mulai–batal berulang. Jarak saja → satu frame yang meleset jauh bisa memicu rep palsu. Keduanya dipakai bersama.
- **Bila ditanya:** *"Two layers handle noisy pose readings: a 5-frame average smooths the angle, and a 5° gap between the start and finish thresholds keeps the decision from flipping."*

**Kenapa tidak 140° dan 150°?**
- Bisa saja, tapi tidak ada angka "ideal". **145° = titik tengah.**
- 150° memberi jarak 10° (lebih tahan goyangan), tetapi lengan yang sudah lurus **bisa tidak pernah dianggap selesai**: dari kamera samping, lengan lurus bisa terbaca hanya ±145–150°, dan banyak orang sengaja tidak mengunci siku. Repnya menggantung lalu dibatalkan setelah 6 detik.
- Setelah rata-rata 5 frame, sisa goyangan ±1–2°, jadi jarak 5° sudah cukup.
- PRD versi lama mencatat 150°/60°; kode sekarang 145°/75° (kemungkinan dilonggarkan setelah uji coba; konfirmasi ke tim).
- **Cara memastikan (L2):** rekam beberapa orang, catat sudut siku terbesar saat lengan di bawah. Bila hampir semua > 155°, 150° aman; bila banyak yang 146–150°, tetap 145°.
- **Bila ditanya:** *"145° is a compromise. A higher finish threshold would be more robust to noise, but some people's straight arm only reads around 145 to 150° from the side, so their clean reps would never finish. I'll calibrate it with labeled recordings."*

##### 4. Kenapa AI tidak boleh mengesahkan atau menghitung rep?
- **Tidak boleh mengesahkan:** di awal set, window 3 detik masih berisi campuran berdiri diam dan awal gerakan. Window campuran seperti itu sering terbaca Salah. Kalau AI wajib mengesahkan, rep bersih pertama bisa ikut ditolak.
- **Tidak boleh menghitung (kotak "Why not let AI count reps?"):** window AI saling tumpang-tindih, karena satu window baru muncul setiap 10 frame. Tidak ada titik mulai dan selesai yang jelas, sehingga satu rep bisa terhitung dua kali atau terlewat. Rumus sudut punya kejadian yang jelas: mulai → puncak → selesai.
- Kesalahan paling penting, yaitu ayunan dan gerakan tidak lengkap, sudah ditangkap rumus sudut **tanpa bergantung pada AI**. Ini menutup kelemahan AI di kelas Salah (recall ±86%).

##### 5. Sitasi di slide ini
- **[3] Wolf dkk. 2025:** mendukung pentingnya rentang gerak penuh. Bagian lengan lurus adalah posisi otot paling teregang (lihat `Belajar-FitRep.md` bagian Incomplete ROM untuk nuansanya).
- **[7] Ullah dkk. 2025:** mendukung penghitungan rep berbasis kinematika. Kalimat "window tumpang-tindih tidak punya awal/akhir" adalah penalaran tim sendiri.

##### 6. Kalimat siap ucap
> "Rumus sudut yang memutuskan: kapan rep mulai, mencapai puncak, dan selesai, serta apakah badan berayun. AI hanya punya hak veto, dan itu pun hanya kalau ia berulang kali yakin gerakannya salah. Saya tidak membiarkan AI menghitung rep, karena tebakannya tumpang-tindih dan tidak punya awal-akhir yang jelas."

### 08 · iOS System Architecture  *(deck 11)*
**Tujuan:** rekayasa iOS tingkat produksi: concurrency, isolasi, disiplin rendering.
- **Judul:** Swift 6 Pipeline, Zero Per-Frame Hops
- **Subjudul:** MVVM + Services around a pure domain reducer, data-race-checked by the compiler [13].
- **Lapisan:** Features (SwiftUI views) → WorkoutSessionViewModel (@MainActor · executes effects) → WorkoutSession (pure reducer: Action → State + Effects) → Domain (RepetitionTracker · PoseGeometry) · Services: CameraService · FramePipeline · PoseEstimator · ExerciseClassifier · AudioService
- **Lajur thread:** `sessionQueue` AVCaptureSession · blocking startRunning() · `frameQueue` Vision + Core ML, synchronous (assumeIsolated) · `MainActor` AsyncStream → state → SwiftUI
- **Poin:**
  1. **Pure Reducer Domain** — WorkoutSession maps Action → State + Effects; the ViewModel only executes effects. Testable without a device.
  2. **Actors on Serial Queues** — Camera and frame pipeline are actors whose executors are dispatch queues; Vision and Core ML run synchronously.
  3. **Render Only on Change** — The preview bypasses SwiftUI, and state is reassigned only when it differs, so 30 fps never forces redraws.
- **Chip:** Swift 6 language mode · 1 documented @unchecked Sendable · iOS 17.2+
- **Notes:** *"The frame pipeline is an actor that runs on the very queue the camera delivers frames to, so each frame enters isolation synchronously — no async hop per frame — while Swift 6 still checks for data races."*
- **Sitasi:** [13] WWDC24 "Migrate your app to Swift 6"
- **Data:** ✅ `SWIFT_VERSION = 6.0`, iOS 17.2. Belum ada test target, jadi jangan klaim "unit-tested".

---

#### 💡 Panduan Mudah Memahami Slide 11

Slide ini menjawab: **"Bagaimana aplikasi memproses 30 gambar per detik tanpa macet, dan kodenya tetap rapi serta aman?"**

##### 1. Arti judul: "Swift 6 Pipeline, Zero Per-Frame Hops"
- **Swift 6** = versi bahasa Swift yang compiler-nya memeriksa ketat agar dua bagian program tidak mengubah data yang sama bersamaan (*data race*, penyebab crash acak). Pemeriksaannya terjadi **sebelum** aplikasi dijalankan [13].
- **Pipeline** = jalur pemrosesan setiap frame: kamera → Vision (cari sendi) → Core ML (nilai form) → hasil.
- **Zero per-frame hops** = frame **tidak dioper-oper** antar-antrean. Analogi: paket tidak perlu dipindah ke loket lain lalu menunggu giliran; paket langsung diproses di meja tempat kurir meletakkannya. Frame diproses di antrean yang sama tempat kamera mengirimnya.

##### 2. Lapisan aplikasi (analogi restoran)
| Lapisan | Di slide | Analogi | Tugasnya |
|---|---|---|---|
| Features | SwiftUI views | Ruang makan | Yang dilihat pengguna |
| ViewModel | WorkoutSessionViewModel · @MainActor | **Pelayan** | Hanya **menjalankan** perintah: putar suara, nyalakan timer, jeda kamera |
| Reducer | WorkoutSession · Action → State + Effects | **Kepala dapur** | Menerima kejadian (frame baru, timer, tombol), lalu menulis keadaan baru + daftar tugas. Tidak menyentuh kamera atau suara sendiri |
| Domain | RepetitionTracker · PoseGeometry | **Buku resep** | Rumus sudut dan pelacak rep (slide 10) |
| Services | CameraService · FramePipeline · PoseEstimator · ExerciseClassifier · AudioService | **Peralatan dapur** | Kamera, Vision, Core ML, audio |

**"Pure reducer"** artinya hasilnya hanya bergantung pada masukan: kejadian yang sama selalu menghasilkan keputusan yang sama. Karena itu logikanya bisa diuji **tanpa iPhone** (*testable without a device*). Jujur: test otomatisnya **belum ada**, jadi katakan "bisa diuji", bukan "sudah diuji".

##### 3. Tiga jalur kerja (di slide: sessionQueue · frameQueue · MainActor)
| Jalur | Isinya | Bahasa mudahnya |
|---|---|---|
| **sessionQueue** | AVCaptureSession · blocking `startRunning()` | Menyalakan kamera. `startRunning()` **menunggu** sampai kamera siap, jadi dijalankan di jalur sendiri supaya layar tidak membeku |
| **frameQueue** | Vision + Core ML, synchronous (`assumeIsolated`) | Setiap frame dianalisis **langsung di sini**, tanpa dioper |
| **MainActor** | AsyncStream → state → SwiftUI | Jalur utama layar. Hasil analisis dikirim lewat **AsyncStream** (seperti ban berjalan), lalu layar diperbarui |

##### 4. Tiga poin di slide
1. **Pure Reducer Domain:** keputusan dipusatkan di satu tempat yang murni (kepala dapur); ViewModel hanya pelaksana. Mudah dilacak dan bisa diuji tanpa perangkat.
2. **Actors on Serial Queues:** *actor* = ruangan berpintu satu; hanya satu pekerjaan di dalamnya pada satu waktu, jadi aman dari *data race*. Ruangan kamera dan ruangan pipeline ditempatkan **di antrean kamera itu sendiri** (`sessionQueue` dan `frameQueue`). Akibatnya frame masuk ke ruangan secara langsung, tanpa menunggu giliran.
3. **Render Only on Change:** preview kamera langsung ditempel ke layar (`AVCaptureVideoPreviewLayer`), tidak lewat SwiftUI. Keadaan layar hanya diganti **kalau memang berbeda** (`if session.state != state`). Tanpa ini, 30 frame per detik bisa memaksa layar digambar ulang 30 kali per detik → boros baterai dan patah-patah.

##### 5. Tiga label kecil (chip)
- **Swift 6 language mode:** pemeriksaan data race penuh aktif (`SWIFT_VERSION = 6.0`).
- **1 documented @unchecked Sendable:** satu-satunya tempat kita bilang ke compiler "percaya saja, ini aman", yaitu `ExerciseClassifier`. Kelas model buatan Xcode belum ditandai aman, padahal propertinya tidak pernah berubah dan hanya dipanggil dari satu actor. Alasannya ditulis di kode.
- **iOS 17.2+:** versi iOS minimum.

##### 6. Bila ditanya lebih dalam
- **"Benar-benar nol lompatan?"** Nol lompatan saat frame **masuk** ke pipeline. Hasil analisis tiap frame tetap dikirim satu kali ke layar (MainActor) lewat AsyncStream. Itu yang memang perlu, karena hanya MainActor yang boleh mengubah tampilan.
- **[13] WWDC24 "Migrate your app to Swift 6":** sumber bahwa mode bahasa Swift 6 memeriksa data race saat compile.

> Kalimat siap ucap: "Setiap frame diproses langsung di antrean yang sama tempat kamera mengirimnya, jadi tidak ada antrean tambahan per frame, dan Swift 6 tetap memeriksa data race. Keputusan latihan dipusatkan di satu reducer murni, dan layar hanya digambar ulang kalau keadaannya benar-benar berubah."

### 09 · Robustness, Privacy & Inclusivity  *(deck 12)*
**Tujuan:** siap menghadapi kondisi dan pengguna nyata, tanpa klaim uji pengguna.
- **Judul:** Built for Messy Real-World Conditions
- **Subjudul:** Failure paths are designed in — not discovered by users.
- **Kartu:**
  - **Robustness** — Handles denied camera access, interruptions, media-server resets, model-load failure, multiple people, and lost poses.
  - **Privacy** — No networking code exists; frames live only in memory and are discarded after pose extraction.
  - **Inclusivity** — Indonesian-first String Catalog [14] with English; voice guidance follows the system language; feedback pairs color with sound [5].
- **KPI:** **6** failure cases handled in code · **91** localized strings in the String Catalog · **ID + EN** UI text, camera permission, voice guidance
- **Notes:** *"I treated failure paths as features: if the model fails to load, the camera still runs and the user is told why. And nothing leaves the phone — there is literally no networking code."*
- **Sitasi:** [14] WWDC23 "Discover String Catalogs" · [5] Apple HIG "Feedback"
- **Data:** ✅ 91 key di `Localizable.xcstrings` (sumber `id`). Enam kasus kegagalan dari CameraService, ExerciseClassifier, RepetitionTracker. Belum ada label VoiceOver maupun haptic, jadi jangan diklaim. Nuansa: bila model gagal dimuat, pesan di layar menyebut repetisi "tidak dapat dinilai", padahal sesi tetap menghitung dengan geometri saja.

### 10 · Model Results  *(deck 13)*
**Tujuan:** menampilkan hasil yang sudah terukur, lalu langsung menunjukkan kelemahannya.
- **Judul:** Strong Scores, One-Sided Errors
- **Subjudul:** Create ML results for FitRep(Best Model) — window-level, after 80 iterations.
- **KPI:** **96.9%** training (iteration 80) · **94.3%** validation (automatic split) · **94%** testing (Testing-2, 173 / 185)
- **Grafik:** recall per class (validation) — Correct 100% · Other 96.4% · **Incorrect 85.7%** (batang oranye)
- **Insight:** About 1 in 7 incorrect windows reads as correct — so swing and range of motion are vetoed geometrically, independent of the model.
- **Notes:** *"At window level the model reaches 94% accuracy on the Testing-2 set, but its typical mistake is letting an incorrect rep pass as correct — roughly one in seven. That's why swing and range of motion are checked geometrically, independent of the model."*
- **Sumber (baris slide):** Create ML (Xcode) — FitRep_Model.mlproj, model FitRep(Best Model), 8 Sep 2026 · window-level metrics · screenshots on the next two slides
- **Data:** ✅ Training 34.432/35.533 window · validasi 1.717/1.820 · Testing-2 94% (173/185 = 93,5%, dibulatkan Create ML) · recall Salah 85,7% (validasi). Jangan menyebut angka testing sebagai *unseen/held-out*.
- 🟨 **Sumber campuran:** KPI utama memakai **testing**, tetapi grafik recall dan insight "1 in 7" memakai **validasi**. Bila ditanya: *"For the Incorrect class, validation recall is the lower, more conservative figure. Testing shows the same pattern: Incorrect is still the weakest class at 88.3% recall, 53 of 60, and every error goes toward Correct."* Opsi slide (dicatat, deck belum diubah): ganti grafik ke recall testing — Correct 100% (60/60) · Other 92,3% (60/65) · Incorrect 88,3% (53/60) — dan insight menjadi "7 of 60 incorrect windows read as correct". Bila memakai angka testing, ingat selisih Others 65 vs header 180 (slide 15).

### 10b · Create ML Evidence  *(deck 14–15)* — **dipindah dari lampiran, rev. 7.2**
**Tujuan:** bukti asli angka di slide 13 langsung menyusul angkanya. Label pill "Create ML Evidence" (sebelumnya "Appendix · Create ML"); caption slide 13 kini "screenshots on the next two slides".
- **Training & Validation in Create ML** *(deck 14, dulu A1)*
  - Subjudul: FitRep(Best Model) · 80 iterations · 30 fps · 90-frame prediction window.
  - Screenshot kurva akurasi per iterasi, caption: *Accuracy per iteration — training (black) vs validation (gray).*
  - READOUT: **96.9%** training accuracy · iteration 80 · **94.3%** validation accuracy · automatic split · **94%** testing accuracy · Testing-2 (173 / 185)
  - Baris sumber: Source: Create ML (Xcode) — project FitRep_Model.mlproj, model FitRep(Best Model); screenshots captured 8 Sep 2026.
  - Notes: *"These are the raw Create ML results: 97% training, 94% validation, and 94% on the Testing-2 set after 80 iterations."* Screenshot asli di `~/Downloads` (Screenshot 2026-09-08 at 13.54.32).
- **Testing & Model Output** *(deck 15, dulu A2)*
  - Subjudul: Testing-2 evaluation, class balance, and the exported Core ML model.
  - Panel Testing-2 · Training set · 140 per class · Testing set · 60 per class · Output: Action Classifier · 4.1 MB · classes Benar / Others / Salah
  - Baris sumber: sama dengan slide 14.
  - Notes: *"Precision on Correct is 83% while recall is 100% — the model's errors all go one way, which is why form faults are also checked geometrically."*
  - Tabel evaluasi menghitung 185 item (Others 65) karena isi folder Testing-2 berubah setelah evaluasi; header data source sekarang 180. Screenshot berwarna biru karena asli dari Create ML; sengaja tidak diubah.

### 11 · Error Analysis  *(deck 16)*
**Tujuan:** diagnosis kegagalan sebagai sinyal kematangan ML.
- **Judul:** Three Failures That Shaped the System
- **Subjudul:** Each response started from a diagnosed root cause, not threshold tuning.
- **Kartu (Symptom → Root cause → Response/Fix):**
  1. **Incorrect Reps Passing as Correct** — Some incorrect reps are classified as correct. → The model's weakest class is Incorrect — ≈ 86% recall in Create ML. → *Response:* Geometric swing and ROM gates veto independently of the model [15].
  2. **First Rep Never "Correct"** — The first rep of a set rarely reads as correct. → Training clips are trimmed single reps; live 3-s windows still hold idle frames. → *Fix:* Count a window only if it covers ≥ 30 frames of the current rep.
  3. **Clean Reps Without Evidence** — Fast or first reps get no confident window. → Too few confident windows overlap a short rep. → *Fix:* Full ROM + no swing + zero incorrect windows → correct.
- **Known limitation:** the "inconclusive" state is currently unreachable, so the system never says "unsure." Logged as tech debt.
- **Notes:** *"The model's typical mistake is the costly one — calling a bad rep good. Instead of chasing a higher score, I made sure the faults that matter most are caught by rules the model can't override."*
- **Sitasi:** [15] Indykov dkk. 2025, *J. Syst. Softw.* (taktik "Rule-based Models" sebagai pelengkap model ML)
- **Data:** ✅ `RepetitionTracker.swift` (gate geometri, overlap 30 frame, default accept = gerbang 4 "Otherwise: Good" di slide 10). Durasi klip 2,3–2,8 s lebih pendek dari window 3,0 s. Kata "Response" di kartu 1 sengaja tidak mengklaim urutan waktu.

### 12 · Multi-Level Evaluation  *(deck 17)*
**Tujuan:** memisahkan angka yang sudah ada dari yang masih harus diukur, dan memetakan metrik ke design goal.
- **Judul:** Model Accuracy Is Not Product Accuracy
- **Subjudul:** Three levels of evidence, each tied to a design goal — one done, two planned.
- **Legenda kiri atas:** Design goals (slide 03) — G1 per-rep verdict · G2 form-fault detection · G3 100% on-device.
- **L1 · Model** `DONE` — Create ML, window-level → G2 · **94%** testing (Testing-2) · 94.3% validation · Weakest class: Incorrect (≈ 86% recall) · Next: new-subject test with people not yet recorded [16].
- **L2 · System** `PLANNED` — rep-level verdicts → G2 · 3–5 subjects × 2 sets × 10 reps (5 clean, 5 with faults) · Labels from video, by a second person · Precision (correct), recall (incorrect), MCC [17], counting accuracy.
- **L3 · Device** `PARTLY VERIFIED` — iPhone, real sessions → G1, G3 · ✓ No networking code (verified in code) · Median inference — Xcode Performance Report [12] · End-to-end latency — filmed at 240 fps; smoothing floor ≈ 67 ms · Thermal state over 6 sets × 15 reps · App Privacy Report — no network domains.
- **Notes:** *"Window accuracy is only the first level. The verdict users feel is rep-level, and the experience they feel is latency and heat on a real phone — so those are the next measurements, with the protocol fixed before collecting data."*
- **Sitasi:** [16] Rehman dkk. 2024, *Algorithms* (k-fold 89% vs LOSO 76%) · [17] Stoica & Babu 2024, *Signal Process.* (MCC biner dan multikelas) · [12] WWDC24 Core ML
- **Data:** 🟦 L2 dan L3 belum diukur; protokolnya di Lampiran B. **Tidak ada target angka** di slide, karena PRD hanya memuat contoh `[isi, misal ≥ 85%]`. Angka ≈ 67 ms hanya jeda filter, bukan latensi end-to-end. Catatan: slide ini belum punya teks pojok kanan atas "Portfolio 1 of 3 · FitRep" seperti slide FitRep lain.
- ⚠️ **Thermal "6 sets × 15 reps"** mengikuti rentang lama slide 6 (3–6 × 6–15). Bila slide 6 disamakan dengan kode (rep 6–20), kasus terberatnya **6 × 20** (±8–9 menit): ubah kartu L3 dan Lampiran B. Bila kode yang diubah ke 6–15, biarkan.

### 13 · Reflection & Roadmap  *(deck 18)*
**Tujuan:** *growth mindset* + langkah berikutnya yang terprioritas.
- **Judul:** What I Learned, What Comes Next
- **Subjudul:** The hardest problems were measurement and orchestration, not model size.
- **Pelajaran:**
  - **Orchestration Over Size** — Edge intelligence came from pairing deterministic rules with a small model, not from a bigger network.
  - **Measure the Product, Not the Model** — 94% window accuracy says little about a rep verdict; evaluation must happen where users feel it [15].
  - **From Claims to Evidence** — My earlier pitch described goals; this version documents decisions, failures, and how each is measured.
- **Roadmap:**
  - **DONE** (hitam) — Swift 6 · pure reducer · ID/EN localization · side-agnostic geometry
  - **NEXT** (oranye) — New-subject test + rep-level evaluation on device · Record more people to reduce reliance on augmentation · Retrain with idle-padded clips · Show the fault reason (swing vs. poor form) · Restore the "inconclusive" state
  - **LATER** (abu) — Left-arm model validation · push-up & squat · VoiceOver labels + haptics
- **Notes:** *"The biggest lesson was that a model score is not a product score. My next step isn't a bigger model; it's more real recordings and a rep-level evaluation with real users."*
- **Sitasi:** [15] Indykov dkk. 2025
- **Data:** ✅ Roadmap cocok dengan kode: `FormVerdict` sudah punya `swinging`/`poorForm` tetapi belum ditampilkan; `inconclusive` tak tercapai. Ide tambahan dari [4]: biarkan pengguna memilih kapan menerima umpan balik (*self-controlled feedback*).

### 14 · App Exposure  *(deck 19)*
**Tujuan:** menunjukkan bahwa FitRep sampai ke publik umum, dengan bukti primer.
- **Judul:** Covered by Two Surabaya Newspapers
- **Subjudul:** Jawa Pos (print) and Harian Bhirawa (online) reported on FitRep in October 2024.
- **Visual:** potongan artikel Jawa Pos (hlm. 15) + tangkapan layar artikel Harian Bhirawa. Kedua gambar bisa diklik ke sumbernya.
- **Kartu COVERAGE:**
  - **Jawa Pos** — Print · Metropolis (Modern West), page 15 · Fri 18 Oct 2024 · "Mahasiswa Manfaatkan AI Buat Aplikasi Olahraga" — students use AI to build a fitness app · *Open the edition on FlipHTML5*
  - **Harian Bhirawa** — Online · Pendidikan · Tue 15 Oct 2024 · "Mahasiswa UC Berinovasi Ciptakan Aplikasi Deteksi Olahraga Berbasis AI" — UC students build an AI exercise-detection app · *Read the article on harianbhirawa.co.id*
- **Caption gambar:** Jawa Pos · print · Harian Bhirawa · online
- **Baris sumber (tanpa nomor IEEE):**
  - Sources (accessed 12 Oct 2026): Jawa Pos, 18 Oct 2024, p. 15 (photo: Riana Setiawan), https://fliphtml5.com/jrime/gszr/Jawa_Pos_18_Oktober_2024/
  - Harian Bhirawa, 15 Oct 2024 (photo: Rendy Agung P), https://harianbhirawa.co.id/mahasiswa-uc-berinovasi-ciptakan-aplikasi-deteksi-olahraga-berbasis-ai/
- **Notes:** *"FitRep also reached a general audience: in October 2024 Harian Bhirawa and Jawa Pos covered it as a student-built AI app that checks exercise form for beginners who can't afford a personal trainer."*
- **Data:**
  - ✅ Jawa Pos: Jumat 18 Okt 2024, hlm. 15, rubrik Riset & Inovasi; narasumber Gabriel M.D. dan Christian; artikel menyebut 3–6 set, 6–12 repetisi, layar hijau bila gerakan benar.
  - ✅ Tautan FlipHTML5 adalah unggahan akun "Perguruan Tinggi" (19 Okt 2024), bukan kanal resmi Jawa Pos. Bila ditanya, sebut edisi cetaknya.
  - ✅ Harian Bhirawa: Selasa 15 Okt 2024, rubrik Pendidikan, oleh Redaksi Bhirawa; narasumber Christian (Sistem Informasi Bisnis UC); demo di Dian Auditorium lt. 7 UC; artikel menyebut 30 gambar per rekaman, rencana jumping jack/push-up/leg lift/sit-up, dan pengembangan ±1 bulan.
  - Kedua artikel menggambarkan versi Okt 2024. Bila ditanya bedanya dengan versi di deck ini, jelaskan evolusinya.
  - Kliping sengaja tidak diberi nomor IEEE (bukti primer, seperti slide Create ML). Bila ingin dimasukkan ke References, nomor [18]–[23] harus digeser.

### 15 · Watch & Try  *(deck 20)* — **baru rev. 7**
**Tujuan:** reviewer bisa melihat aplikasinya bekerja dan mencobanya sendiri; penutup bagian FitRep (mengikuti halaman "Try Our App!" di keynote 2024).
- **Judul:** See It Work, Then Try It Yourself
- **Subjudul:** Watch FitRep coach a set on a real iPhone, then install the beta and try one yourself.
- **Kiri (kartu hero):** dua screenshot aplikasi dari pemilik — **Guided setup** (Workout Target + Movement Instructions) dan **Session summary** (You've Done It!, total reps). Keduanya bisa diklik ke video.
- **Kartu aksi 1 · Watch the demo** (ikon video): FitRep giving real-time form feedback on a real iPhone. · YouTube · Real-Time Exercise Form Feedback · tombol utama **► Watch on YouTube** · QR "Scan to watch".
- **Kartu aksi 2 · Try it on your iPhone** (ikon ponsel): Install the beta, stand side-on, and start a set. Everything runs on-device. · iPhone · iOS 17.2+ · free with the TestFlight app · tombol sekunder **Join the TestFlight beta** + penanda `[ISI: link]` · kotak QR abu "QR TestFlight [ISI]" · "Scan to join".
- **Baris sumber:** Video: youtube.com/watch?v=soQHorpnEWM · TestFlight: `[ISI: public link]` · Screens: FitRep on iPhone
- **Notes:** *"Here's FitRep on a real iPhone: guidance before you start, live feedback on every rep, and a summary when you finish. The demo video shows a full set — and the TestFlight beta lets you try one on your own phone."*
- **Sitasi:** tidak ada (bukti primer).
- **Data:**
  - ✅ Video YouTube "FitRep: Real-Time Exercise Form Feedback" (kanal baghiz zuhdi), dicek lewat oEmbed. QR video dibuat lokal dan sudah didekode ulang ke URL yang benar. Semua tautan bisa diklik di PDF hasil ekspor PowerPoint.
  - 🟨 Saat link TestFlight publik ada, ganti di 3 tempat: tombol (klik kanan → Edit Link), `[ISI: public link]` di footer (hapus juga `[ISI: link]`), dan kotak QR. Buat QR dengan `swift make_qr.swift "<link>" qr-testflight.png`.
  - 🟨 Pastikan video berstatus Public/Unlisted.
  - Deskripsi kartu sengaja hanya mengulang judul video; isi video belum diverifikasi frame per frame.

### References  *(deck 31)*
- **References** *(deck 31)* — satu halaman, [1]–[23], dua kolom 11 + 12, 8 pt (Lampiran D).

---

## Lampiran A · Fakta Terverifikasi (dicek ulang 2026-10-12 s.d. 14)

| Fakta | Sumber |
|---|---|
| Ambang: start 140°, puncak 75°, selesai 145°, 1,0–6,0 s, torso 12°, lengan atas 35°, confidence 0,60, overlap 30 frame, `verdictDelayFrames = 0` | `Domain/RepetitionTracker.swift` |
| `.inconclusive` tidak tercapai dengan konfigurasi bawaan | `RepetitionTracker.swift` |
| Window 90, stride 10, pose asli ≥ 60% (54 frame) | `FramePipeline.swift`, `ExerciseClassifier.swift` |
| Kamera depan 720p, 30 fps, late frames dibuang, mirrored | `CameraService.swift` |
| Rencana latihan: set 1–6, rep 6–20, bawaan 3 × 12 (berbeda dengan teks slide 6) | `Domain/WorkoutPlan.swift` |
| Model aktif = **FitRep(Best Model)**: 3 kelas, 4,1 MB, input `[90,3,18]` Float32, Create ML 6.2, 80 iterasi (md5 `871158…`) | `Resources/MLModels/BicepCurl.mlmodel` |
| Swift 6, iOS 17.2; tidak ada kode jaringan | `project.pbxproj`; pencarian `URLSession` kosong |
| 91 string lokal, bahasa sumber `id` | `Localizable.xcstrings` |
| Data: 140 train + 60 test per kelas (Benar/Salah/Others); klip 2,3–2,8 s | `~/Documents/New Model Cleaned - 140-60/` |
| Metrik: train 96,9% · val 94,3% · Testing-2 94% (173/185 = 93,5%, dibulatkan Create ML) · recall val Benar 100% · Others 96,4% · Salah 85,7% | `FitRep_Model.mlproj` (8 Sep 2026) |
| Riset pengguna: kutipan Luist, Dylan, Jessica, Kevin, Salsa, Weka; persona Chris; existing behavior; need = guidance + live feedback; tim 5 anggota + 1 mentor | `~/Downloads/Storytelling with Data.pdf` (keynote 1 Agu 2024) |
| Kutipan Nae dkk. cocok kata per kata (dengan elipsis) | PDF MARATHON vol. 14 no. 1 (2022), hlm. 40–46 |
| Liputan: Jawa Pos 18 Okt 2024 hlm. 15; Harian Bhirawa 15 Okt 2024 | kliping + tautan di slide 19 (dicek 12 Okt 2026) |
| Video demo: "FitRep: Real-Time Exercise Form Feedback" | YouTube oEmbed (dicek 13 Okt 2026) |

## Lampiran B · Protokol Pengukuran (belum dijalankan)
- **New-subject test (L1):** rekam 10–15 klip per kelas dari orang yang belum pernah direkam → Create ML → FitRep(Best Model) → Testing → Evaluate. Isi `New-subject test: [UKUR]%`.
- **Rep-level (L2):** 3–5 subjek × 2 set × 10 rep (5 bersih, 5 dengan kesalahan: ayunan, ROM parsial, siku maju); label manual oleh orang kedua. Laporkan `Counting accuracy [x]/[y] · Precision(correct) [p] · Recall(incorrect) [r] · MCC [m] — n = [N] reps, [S] subjects`.
- **Ablation (slide 7):** vonis geometry-only (ML veto dimatikan) vs hybrid pada set rep yang sama. Selisih recall "incorrect" = kontribusi nyata ML.
- **Inferensi Core ML (L3):** Xcode → `BicepCurl.mlmodel` → Performance → iPhone tersambung → median prediction time + compute unit per lapisan.
- **Vision per frame (L3):** Instruments → Time Profiler / `os_signpost`; lulus bila < 33 ms (anggaran 30 fps).
- **End-to-end (L3):** rekam slow-motion 240 fps (lengan + layar satu frame); hitung frame dari siku lurus sampai pulse hijau ÷ 240. Angka ≈ 67 ms hanya jeda filter, belum termasuk Vision, hop ke MainActor, dan audio.
- **Thermal (L3):** track Thermal State di Instruments selama sesi 6 × 15 rep (±7–8 menit). ⚠️ Ikuti keputusan set/rep slide 6: bila disamakan dengan kode (rep 6–20), pakai 6 × 20 (±8–9 menit).
- **Privasi (G3):** Settings → Privacy & Security → App Privacy Report → FitRep tanpa domain jaringan.
> Kisaran tebakan hanya untuk *sanity check* perencanaan dan **tidak boleh** tampil di slide.

## Lampiran C · Pelajaran dari Lamaran 2025
- **Dihapus:** "innovative", "advanced ML algorithms", "injury prevention", "real-time posture correction".
- **Diganti:** komponen bernama, angka terverifikasi, peran pribadi, trade-off, error analysis, evaluasi 3 level.
- **Dipertahankan:** target pemula gym, argumen pelatih mahal vs video satu arah, privasi on-device.
- **Ditambahkan rev. 7:** bukti suara pengguna dari riset Academy 2024 (kutipan wawancara) dan perilaku yang sudah ada (PT, cermin, video online).

## Lampiran D · Daftar Pustaka IEEE (sama persis dengan slide 31 deck)
Nomor global untuk **seluruh deck**, urut kemunculan pertama: FitRep memakai [1]–[17]; SIDIA memakai [18]–[23] serta [14] dan [15] yang dipakai ulang. Isinya 12 artikel jurnal (11 terbitan 2024–2025 + Nae 2022) dan 11 dokumentasi Apple. DOI dicek di Crossref; halaman Apple dicek HTTP 200, judul, dan isi terhadap klaim (12–13 Okt 2026).

| No. | Dipakai di deck | Jenis |
|---|---|---|
| [1] | 4, 8 | Apple (WWDC20) |
| [2] | 5 | Jurnal 2022 (pengecualian) |
| [3] | 5, 10 | Jurnal 2025 |
| [4] | 6 | Jurnal 2025 |
| [5] | 6, 12 | Apple (HIG) |
| [6] | 7 | Jurnal 2025 |
| [7] | 7, 10 | Jurnal 2025 |
| [8] | 8 | Apple |
| [9] | 8 | Jurnal 2024 |
| [10] | 8 | Apple (sample code) |
| [11] | 9 | Jurnal 2024 |
| [12] | 9, 17 | Apple (WWDC24) |
| [13] | 11 | Apple (WWDC24) |
| [14] | 12, 23 | Apple (WWDC23) |
| [15] | 16, 18, 30 | Jurnal 2025 |
| [16] | 17 | Jurnal 2024 |
| [17] | 17 | Jurnal 2024 |
| [18] | 22, 29 | Jurnal 2025 |
| [19] | 24 | Apple (WWDC25) |
| [20] | 26 | Apple |
| [21] | 26 | Apple |
| [22] | 27 | Apple (WidgetKit) |
| [23] | 29 | Jurnal 2024 |

- [1] Apple Inc., “Build an action classifier with Create ML,” presented at WWDC20, Session 10043, Jun. 2020. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/videos/play/wwdc2020/10043/
- [2] I. C. Nae, C. Filip, R. V. Ciomag, V. M. Zamfir, and C. Pop, “Theoretical concepts regarding exercising at the gym,” *Marathon*, vol. 14, no. 1, pp. 40–46, 2022. Accessed: Oct. 13, 2026. [Online]. Available: https://marathon.ase.ro/pdf/vol14/6.%20Articol%20Marathon%20I%20Nae%2001%202022.pdf
- [3] M. Wolf et al., “Lengthened partial repetitions elicit similar muscular adaptations as full range of motion repetitions during resistance training in trained individuals,” *PeerJ*, vol. 13, Art. no. e18904, Feb. 2025, doi: 10.7717/peerj.18904.
- [4] K. Akizuki, K. Takeuchi, J. Yabuki, K. Yamaguchi, R. Yamamoto, and T. Kaneno, “Effects of self-control of feedback timing on motor learning,” *Front. Psychol.*, vol. 16, Art. no. 1638827, Sep. 2025, doi: 10.3389/fpsyg.2025.1638827.
- [5] Apple Inc., “Feedback,” Human Interface Guidelines, Apple Developer. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/design/human-interface-guidelines/feedback
- [6] K. Alomar, H. I. Aysel, and X. Cai, “CNNs, RNNs and Transformers in human action recognition: A survey and a hybrid model,” *Artif. Intell. Rev.*, vol. 58, no. 12, Art. no. 387, Oct. 2025, doi: 10.1007/s10462-025-11388-3.
- [7] R. Ullah, I. Asghar, R. Nawaz, C. Stacey, S. Akbar, and P. Bishop, “A real time action scoring system for movement analysis and feedback in physical therapy using human pose estimation,” *Sci. Rep.*, vol. 15, Art. no. 44784, Dec. 2025, doi: 10.1038/s41598-025-29062-7.
- [8] Apple Inc., “Creating an action classifier model,” Apple Developer Documentation. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/documentation/createml/creating-an-action-classifier-model
- [9] M. Sivakumar, S. Parthasarathy, and T. Padmapriya, “Trade-off between training and testing ratio in machine learning for medical image processing,” *PeerJ Comput. Sci.*, vol. 10, Art. no. e2245, Sep. 2024, doi: 10.7717/peerj-cs.2245.
- [10] Apple Inc., “Detecting human actions in a live video feed,” Apple Developer Documentation (sample code). Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/documentation/createml/detecting-human-actions-in-a-live-video-feed
- [11] B. Ren, M. Liu, R. Ding, and H. Liu, “A survey on 3D skeleton-based action recognition using learning method,” *Cyborg Bionic Syst.*, vol. 5, Art. no. 0100, 2024, doi: 10.34133/cbsystems.0100.
- [12] Apple Inc., “Deploy machine learning and AI models on-device with Core ML,” presented at WWDC24, Session 10161, Jun. 2024. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/videos/play/wwdc2024/10161/
- [13] Apple Inc., “Migrate your app to Swift 6,” presented at WWDC24, Session 10169, Jun. 2024. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/videos/play/wwdc2024/10169/
- [14] Apple Inc., “Discover String Catalogs,” presented at WWDC23, Session 10155, Jun. 2023. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/videos/play/wwdc2023/10155/
- [15] V. Indykov, D. Strüber, and R. Wohlrab, “Architectural tactics to achieve quality attributes of machine-learning-enabled systems: A systematic literature review,” *J. Syst. Softw.*, vol. 223, Art. no. 112373, May 2025, doi: 10.1016/j.jss.2025.112373.
- [16] S. U. Rehman, A. Ali, A. M. Khan, and C. Okpala, “Human activity recognition: A comparative study of validation methods and impact of feature extraction in wearable sensors,” *Algorithms*, vol. 17, no. 12, Art. no. 556, Dec. 2024, doi: 10.3390/a17120556.
- [17] P. Stoica and P. Babu, “Pearson–Matthews correlation coefficients for binary and multinary classification,” *Signal Process.*, vol. 222, Art. no. 109511, Sep. 2024, doi: 10.1016/j.sigpro.2024.109511.
- [18] H. Diansyah and Syafrinal, “Design and development of a mobile application using Android Studio and Flutter,” *J. Mobile Technol.*, vol. 3, no. 2, pp. 69–76, Sep. 2025, doi: 10.59431/jms.v3i2.646.
- [19] Apple Inc., “Embracing Swift concurrency,” presented at WWDC25, Session 268, Jun. 2025. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/videos/play/wwdc2025/268/
- [20] Apple Inc., “httpShouldSetCookies,” Apple Developer Documentation (Foundation: URLSessionConfiguration). Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/documentation/foundation/urlsessionconfiguration/httpshouldsetcookies
- [21] Apple Inc., “Analyzing HTTP traffic with Instruments,” Apple Developer Documentation. Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/documentation/foundation/analyzing-http-traffic-with-instruments
- [22] Apple Inc., “Displaying dynamic dates in widgets,” Apple Developer Documentation (WidgetKit). Accessed: Oct. 12, 2026. [Online]. Available: https://developer.apple.com/documentation/widgetkit/displaying-dynamic-dates
- [23] I. G. N. D. Paramartha, I. P. W. Prasetia, and K. K. Wardana, “Usability testing pada aplikasi Undiknas Mobile menggunakan metode System Usability Scale,” *METIK J.*, vol. 8, no. 1, pp. 24–30, Jun. 2024, doi: 10.47002/metik.v8i1.735 (in Indonesian).

**Sumber di luar daftar IEEE:** kliping Jawa Pos dan Harian Bhirawa (slide 19), screenshot Create ML (slide 13, 14, 15), kutipan wawancara pengguna (slide 5), serta video demo YouTube (slide 6 dan 20). Semuanya bukti primer yang dicantumkan di baris sumber slide.

**Riwayat kurasi:** daftar 53 entri (rev. 5) masih ada di `Portfolio-Slides-FitRep.md`. Penempatan sitasi per slide versi terbaru ada di `Revisi-Sitasi-IEEE.md`.

**Kelemahan yang perlu diketahui:**
- [2] Nae dkk. (2022) melanggar aturan kurasi "jurnal setelah Okt 2023". Ini juga panduan teoretis untuk mahasiswa di jurnal kecil, bukan studi cedera. Dipertahankan karena berasal dari riset asli tim dan kutipannya terverifikasi. Bila ingin konsisten, cari pengganti terbitan 2024–2026 dengan klaim serupa.
- [18] tetap sumber terlemah untuk SIDIA: jurnal kecil, dan studinya membandingkan Android native vs Flutter, bukan SwiftUI.
- Klik ulang setiap DOI dan URL sebelum submit.

## Lampiran E · Checklist Sebelum Presentasi
- [x] Isi meta slide 01 (anggota, durasi): "6 members · 4 weeks".
- [ ] **Cocokkan tim dengan keynote 2024:** "Meet the Team" = 5 anggota + 1 mentor; peran pemilik "ML Modelling". Kalimat My Role sudah menjadi "ML & iOS Engineer"; meta "6 members" belum dicocokkan.
- [x] **Samakan KPI testing accuracy:** semua slide kini 94% (sesuai Create ML, slide 15).
- [ ] **Samakan rentang set/rep:** slide 6 "3–6 sets × 6–15 reps" vs kode (1–6 × 6–20, bawaan 3 × 12). Ikut sesuaikan thermal di kartu L3 slide 17 dan Lampiran B (6 × 15 → 6 × 20 bila mengikuti kode).
- [ ] Isi link TestFlight dan QR di slide 20 (lihat bagian 15).
- [ ] Pastikan video YouTube berstatus Public/Unlisted.
- [ ] Konfirmasi persona Chris, jumlah responden wawancara, dan izin menampilkan nama depan di slide 5.
- [ ] Konfirmasi periode proyek (keynote Agu 2024, liputan Okt 2024, video Juni 2026, model Sep 2026) dan siapkan penjelasan evolusinya.
- [x] Ejaan teks pojok kanan atas → "Portfolio 1 of 3 · FitRep" dan "Portfolio 3 of 3 · SiDia" (27 slide, 14 Okt 2026).
- [x] Salah ketik diperbaiki (14 Okt 2026): slide 1 "intellegent" → "intelligent"; slide 3 "Ditigal" → "Digital"; slide 5 "he swing" → "The swing".
- [ ] Cek nama di sertifikat:
  - slide 3: nama di kartu sertifikat tertulis "Baghiz Zulhdi Adzin" — cek apakah memang begitu di sertifikat asli.
- [ ] Selaraskan About Me (slide 2): "Information Systems graduate" vs "Bachelor of Computer Science".
- [x] ~~Revisi "Vision's 18-joint keypoints" di slide 8~~ — kini "18 Vision body landmarks", didukung transkrip [1].
- [ ] Jalankan protokol Lampiran B; ubah status L2/L3 di slide 17 setelah ada angka.
- [x] PRD disinkronkan dengan kode (14 Okt 2026): confidence 0,60, Swift 6, sisi kiri atau kanan, posisi ditahan 30 frame, sudut 75°/145°, status inconclusive ditandai tidak tercapai.
- [ ] Putuskan pill teks putih vs hitam (lihat §1, baris Kontras).
- [ ] Ekspor ulang `FitRep-Portfolio.pdf` setelah semua perubahan dan cek keterbacaan pada zoom 100%.

---
**Changelog rev. 7.2 (2026-10-14):**
- **Perbaikan dokumen (16 Okt 2026, deck tidak berubah):** "Apple tidak mempublikasikan/merilis" → "tidak mendokumentasikan" (bagian 06 dan Belajar). "Kebal"/"tidak terpengaruh" penampilan → "jauh kurang sensitif", karena deteksi pose Vision sendiri tetap bisa terpengaruh. Durasi 2,3–2,8 s kini disebut durasi klip kami, bukan durasi rep manusia pada umumnya. Belajar T-B6 diluruskan: model melihat klip diam (di Others); yang tidak pernah dilihatnya adalah window campuran diam + awal rep. Nomor "[1], [9]" di dua entri changelog di bawah → "[1], [10]". Catatan ketergantungan thermal 6 × 15 pada rentang set/rep (slide 17, Lampiran B, Lampiran E). Jawaban lisan untuk sumber campuran testing/validasi di slide 13 (naskah bagian 10 dan Belajar T-C2b).
- **Slide 9, naskah (16 Okt 2026, deck tidak berubah):** sitasi [12] "WWDC24 Session 10159" → **10161** (sama dengan Lampiran D). Kalimat siap ucap: "ukuran data yang diproses" → "input classifier", dan "sangat hemat komputasi" dihapus, diganti catatan bahwa 51.000× adalah perbandingan jumlah angka, bukan hasil ukur beban komputasi (Vision tetap membaca gambar; beban diukur di L3). Kini sejalan dengan tabel "Bedah Angka".
- **Slide 9, panduan poin 1 (16 Okt 2026, deck tidak berubah):** "Sangat Ringan & Efisien (Hemat Data)" → **"Input Jauh Lebih Ringkas"**; klaim video mentah "membebani memori dan menguras baterai iPhone" dihapus karena belum diukur, diganti nuansa jujur (dampak memori/baterai menunggu L3; Vision tetap membaca gambar).
- **Slide 10 kartu 1:** ditambah syarat mulai "A rep starts when the elbow bends below 140°." (kotak teks ditinggikan agar 5 baris muat). Durasi 1–6 s dihitung dari < 140° (mulai) sampai > 145° (selesai).
- **Durasi rep minimum 0,8 s → 1 s (15 Okt 2026):** `minimumDuration = 1.0` di `RepetitionTracker.swift` (keputusan pemilik: rep secepat 0,8 s terlalu berisiko). Slide 10 kartu 1 kini "within 1–6 s"; notes, naskah, Belajar, PRD ikut. Rep < 1 s ditolak tanpa bunyi. Belum diuji di iPhone.
- **Slide 10:** "Smooth out camera jitter" → **"Average the last 5 frames"**: yang dihaluskan adalah titik sendi hasil deteksi pose yang melompat antar-frame, bukan getaran kamera; kata baru langsung menyebut caranya.
- **Slide 10 kartu 3:** kondisi "more wrong than right" kini poin tersendiri yang ditebalkan: **More “wrong” than “right” → wrong** (di samping **2+ “wrong” → wrong**). Keempat kartu gerbang ditinggikan 0,18 in dan kotak "Why not let AI count reps?" digeser turun agar muat.
- **Slide 10 dibuat ramah orang awam:** judul "Kinematics Decide; ML Holds a Veto" → **"Angle Rules Decide; AI Can Only Veto"**; lajur Geometry/ML → Angles/AI; gerbang → Full Movement? [3] · Body Swing? · AI Says Wrong? · Otherwise: Good; hasil → Good rep · counted / Bad form · not counted / Not a valid rep · ignored; callout → "Why not let AI count reps?". Angka ambang tidak berubah (≥ 30 frame ditulis ≥ 1 s). Tabel istilah teknis ditambahkan di bagian 07 dan notes slide.
- **Slide 9:** "Apple doesn't publish the architecture" → "doesn't **document**" (klaim ketiadaan lebih mudah dipertahankan; sama dengan notes).
- **Sumber baru [9] (14 Okt 2026):** Sivakumar dkk. 2024, *PeerJ Comput. Sci.* — dicantumkan di kotak **Split [9]** slide 8 ("Common 70/30 split · Train · validate · test") dan notes. Nomor lama [9]–[22] → **[10]–[23]** di seluruh deck, naskah (sebelum changelog ini), Belajar, dan `Revisi-Sitasi-IEEE.md`; References kini 23 entri (kolom 11 + 12). Nomor di entri changelog lama sengaja tidak diubah.
- **Struktur:** dua slide bukti Create ML dipindah dari lampiran (deck 19–20) ke **tepat setelah Model Results (deck 14–15)**. Error Analysis → deck 16, Multi-Level Evaluation → 17, Reflection → 18, App Exposure → 19, Watch & Try → 20. SIDIA (21–30) dan References (31) tetap. Nomor sitasi tidak berubah.
- **Slide 8 (Data Pipeline):** ditambah sitasi [1] (transkrip WWDC20 10043): subjudul kini menyebut alasan memakai Action Classifier (belajar gerakan dari body pose), poin 1 menyebut panduan Apple ~50 klip per kelas (dengan catatan jujur "including augmented ones"), poin 3 menjadi "the same 18 Vision body landmarks at the same 30 fps [1], [10]". Nomor sitasi tidak berubah.
- **Slide 7:** bar Decision "Use rules where the answer is clear; use AI only where judgment is needed." → **"Angles count the reps; AI checks the form."** (lebih konkret, langsung menjawab subjudul).
- **Slide 7:** baris "Training data needed" kini **tanpa ikon** (teks saja: none · a lot · 140 clips per type), karena ikon ✓/✕ di baris ini mudah terbaca sebagai "ya/tidak butuh data" dan bertentangan dengan legenda good/poor.
- **Slide 7:** baris "On-device cost / Runs on a phone" **dihapus** — kedua pendekatan bisa berjalan di ponsel, jadi baris itu tidak membedakan. Beban komputasi model video tetap disebut di sel "How it works" ("heavy computing [6]"). Tinggi baris lain disesuaikan agar tabel tetap sejajar dengan bar Decision.
- **Slide 7:** legenda ✓ good · △ partly · ✕ poor dipindah dari caption bawah ke sel kiri atas tabel.
- **Audit overlap 30 frame (2026-10-14):** ambang `minimumEvidenceOverlapFrames` = 30 frame (1,0 s) kini sama di kode, slide 10 dan 16, naskah, Belajar, dan PRD. Perbaikan kecil: kotak Trim slide 8 → "One rep per clip · 2.3–2.8 s" (tanpa "≈", en dash); poin 1 slide 8 "minimum about 50" → "about 50 per class [1]"; notes slide 10 → "dicek ulang 2026-10-14". Sitasi [8] (kotak Create ML: pengaturan training) dan [1], [10] (kartu Same Extractor: aplikasi saat berjalan) sengaja berbeda karena mendukung klaim yang berbeda.
- **Rapikan deck (14 Okt 2026):** salah ketik slide 1, 3, 5 diperbaiki; teks pojok kanan atas → "Portfolio … · …" di 27 slide; notes slide 8 ditambah sumber rumus window [8]. PRD disinkronkan dengan kode (sisi kiri/kanan, posisi 30 frame, sudut 75°/145°, inconclusive tidak tercapai). `Revisi-Sitasi-IEEE.md` diperbarui untuk slide 8 (33 baris).
- **Kelas Other = klip diam + gerakan bukan curl:** semua kalimat "tanpa kelas Idle" diluruskan menjadi "tanpa kelas Idle **terpisah**" (notes slide 8 dan 10, naskah, Belajar, PRD).
- **Slide 8 · kelas Other ditekankan [1]:** kartu 1 kini **"An Other Class Is a Must"** (kutipan transkrip: model "also needs to understand when we're not exercising"); jumlah klip, augmentasi, dan panduan ~50 digabung di kartu 2 **"Balanced Classes, Augmented Clips"**. Kata "Other" di kotak Label [1] diberi warna aksen. Notes ditambah satu kalimat tentang kelas Other.
- **Label:** "Appendix · Create ML" → "Create ML Evidence"; caption slide 13 "screenshots in Appendix A1–A2" → "screenshots on the next two slides".

**Changelog rev. 7.1 (2026-10-14, menyusul suntingan pemilik):**
- **Struktur:** slide baru **3 · Certifications & Training**; deck kini 31 slide. Semua slide FitRep bergeser +1 (FitRep 01 = deck 4, Watch & Try = deck 18), lampiran ke 19–20, SIDIA ke 21–30, References ke 31. Nomor sitasi **tidak** berubah.
- **Slide 1–2:** cover dan About Me kini berupa teks yang dapat diedit (bukan gambar); kontak di cover bisa diklik.
- **Slide 01:** My Role → "ML & iOS Engineer".
- **Slide 04 (deck 7):** tabel, subjudul, keputusan, dan caption ditulis ulang dengan bahasa awam (lihat padanan istilah di bagian slide 04).
- **Slide 02:** kutipan Luist diganti menjadi "At the gym, people usually don't know where to start." (asli: "Di gym, biasanya orang itu dia bingung mulai dari mana.").
- **Slide 02:** "Momentum Cheating — Torso swing does the lifting." → "Swinging the Body — The swing lifts the weight, not your arm." (agar mudah dipahami awam).
- **KPI testing** diseragamkan menjadi **94%** di semua slide.

**Changelog rev. 7 (2026-10-14):**
- **Struktur:** slide baru **15 · Watch & Try** (deck 17); lampiran bergeser ke deck 18–19, SIDIA ke 20–29, References ke 30. Deck kini 30 slide. Pill FitRep tanpa nomor ("Nama Bagian"); pemilik menambahkan teks pojok kanan atas.
- **Slide 02 (Problem):** disusun ulang dengan keynote riset Academy 2024 — kutipan wawancara pemula, Existing Behavior (PT, cermin, video online), kutipan ahli Nae dkk. [2], dua kesalahan umum, bar "They need + Challenge". Matriks 2×2 (dengan Wearable dan kartu FitRep) dihapus.
- **Slide 03 (Solution):** tombol "Watch the full demo on YouTube" dengan ikon YouTube.
- **Slide 01:** [1] kini WWDC20 10043 (subjudul + My Role); KPI utama diganti dari validation 94.3% ke testing accuracy 94%.
- **KPI testing 94%** di semua slide (3, 12, 14, 15, 18), sesuai tampilan Create ML di slide 19; nilai persis 93,5% hanya disebut lisan.
- **Referensi:** 20 → 22 entri. [1] WWDC20 baru; WWDC24 Core ML menjadi [11]; Nae dkk. masuk sebagai [2]; *Creating an action classifier model* menjadi [8] agar urutan IEEE tetap benar.

**Changelog rev. 6 (2026-10-12):**
- **Struktur:** cover dan About Me (slide 1–2) diganti gambar baru. Slide baru **14 · App Exposure** (deck 16) berisi liputan Jawa Pos dan Harian Bhirawa. Nomor deck bergeser +1; deck menjadi 29 slide.
- **Referensi:** dikurasi 40 → 20, References menjadi 1 slide. Semua publikasi diganti artikel jurnal terbitan setelah Okt 2023. Tiga halaman Apple diganti yang lebih spesifik: HIG Feedback, sample code action classifier, WidgetKit dynamic dates.
- **Desain:** palet disamakan dengan cover. Aksen `#FF751F`; status hijau/merah/biru diganti hitam/oranye tua/abu; teks oranye kecil menjadi `#C93400` (kontras HIG); garis tepi berwarna dihapus.

**Changelog rev. 5 dan sebelumnya:** lihat `Portfolio-Slides-FitRep.md`.
