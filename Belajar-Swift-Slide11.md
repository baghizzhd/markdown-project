# Swift dari Nol untuk FitRep (Slide 11)

> Disusun 16 Okt 2026 sebagai pendamping slide 11 deck FitRep, **"30 Frames a Second, Without Freezing the Screen"** (bagian 08 · iOS System Architecture di naskah deck).
> Untuk pemula yang belum pernah belajar Swift. Setiap konsep langsung dikaitkan ke bagian slide 11 yang memakainya.
> Versi interaktif (simulasi data race, perjalanan satu frame, progres belajar): https://claude.ai/artifact/Vknp2hWbHLmqVsz1kVgiEc (private, hanya bisa dibuka pemilik).

> **Tentang contoh kode.** Semua kode di sini adalah versi sederhana untuk belajar, bukan salinan kode FitRep (kode aslinya ada di MacBook). Nama asli di kode FitRep ditulis di bagian **Di FitRep**. Contoh kode belum di-compile; cocokkan dengan kode asli sebelum mengutip detailnya, terutama kerangka di C5 dan C6.
>
> **Coba sendiri.** Kode bagian A bisa dijalankan di Xcode (File → New → Playground) atau aplikasi Swift Playgrounds. Tempel, lalu tekan Run.

---

## Daftar Isi

- **Bagian A · Dasar bahasa Swift** — [A1](#a1-swift-xcode-dan-compiler) · [A2](#a2-let-dan-var) · [A3](#a3-tipe-data-dan-fungsi) · [A4](#a4-struct-dan-class) · [A5](#a5-enum-switch-dan-optional) · [A6](#a6-protocol)
- **Bagian B · Layar dan arsitektur** — [B1](#b1-swiftui-layar-yang-digambar-dari-data) · [B2](#b2-lima-lapisan-dan-reducer)
- **Bagian C · Concurrency** — [C1](#c1-thread-dan-antrean) · [C2](#c2-data-race) · [C3](#c3-asyncawait-dan-actor) · [C4](#c4-mainactor-dan-asyncstream) · [C5](#c5-swift-6-sendable-dan-3-pengecualian) · [C6](#c6-executor-dan-assumeisolated)
- **Bagian D · Slide 11 utuh** — [D1](#d1-perjalanan-satu-frame) · [D2](#d2-peta-slide-11-dan-naskah-siap-ucap) · [D3](#d3-latihan-tanya-jawab)
- [Glosarium](#glosarium) · [Belajar lanjut](#belajar-lanjut)

## Cara memakai

Setelah selesai, Anda bisa:
- membaca potongan kode Swift sederhana tanpa bingung,
- menjelaskan lima lapisan dan tiga jalur di slide 11 dengan kata-kata sendiri,
- menjawab pertanyaan reviewer tentang Swift 6, actor, dan "No Handoffs per Frame" dengan jujur.

| Sesi | Bagian | Waktu |
|---|---|---|
| 1 | A · Dasar bahasa Swift | ±45 menit |
| 2 | B · Layar dan arsitektur + C1–C3 | ±50 menit |
| 3 | C4–C6 + D · Slide 11 utuh | ±60 menit |

---

## Bagian A · Dasar bahasa Swift

Enam pelajaran pendek tentang bahan dasar yang muncul di semua kode Swift. Tanpa ini, istilah seperti "immutable", "class", atau "Sendable" di slide 11 akan terasa asing.

### A1. Swift, Xcode, dan compiler
*±5 menit · Di slide 11: "Swift 6 rejects code…"*

**Swift** adalah bahasa pemrograman buatan Apple untuk aplikasi iPhone, iPad, dan Mac. Seluruh FitRep ditulis dengan Swift. **Xcode** adalah aplikasi di Mac tempat kode ditulis, dijalankan di simulator, lalu dipasang ke iPhone.

Sebelum aplikasi bisa dipasang, **compiler** (penerjemah) membaca semua kode, memeriksa aturannya, lalu menerjemahkannya ke bahasa mesin iPhone. Karena itu ada dua jenis masalah yang sangat berbeda:

| Jenis | Kapan ketahuan | Siapa yang kena |
|---|---|---|
| **Error compile** | Saat Xcode membuat aplikasi. Aplikasi tidak bisa dibuat sampai kodenya diperbaiki. | Hanya programmer |
| **Bug runtime** | Saat aplikasi sudah berjalan: salah hitung, atau crash. | Pengguna |

Subjudul slide 11 menyebut Swift 6 *menolak* kode tertentu. Artinya satu jenis bug berbahaya (data race, pelajaran C2) dipindahkan dari baris kedua ke baris pertama tabel ini: ketahuan sebelum aplikasi sampai ke tangan pengguna.

**Di FitRep:** proyek memakai `SWIFT_VERSION = 6.0` (Swift 6 language mode) dan target minimum iOS 17.2.

**Cek pemahaman.** *Kalau compiler menolak sebuah kode, apakah pengguna pernah melihat bug itu?*
→ Tidak. Aplikasinya bahkan tidak bisa dibuat sampai kode itu diperbaiki.

### A2. let dan var
*±5 menit · Di slide 11: alasan pengecualian `ExerciseClassifier` aman*

Setiap data di Swift disimpan dengan nama. `let` untuk nilai yang **tidak boleh berubah** (konstanta), `var` untuk nilai yang **boleh berubah** (variabel). Teks setelah `//` adalah komentar: catatan untuk manusia, diabaikan compiler.

```swift
let startAngle = 140.0   // konstanta: tidak bisa diubah
var goodReps = 0         // variabel: boleh berubah

goodReps = goodReps + 1  // ✓ boleh
goodReps += 1            // ✓ cara singkat, artinya sama

startAngle = 150         // ✕ error compile:
                         //   startAngle adalah let
```

**Kenapa penting untuk slide 11?** Data yang tidak pernah berubah aman dibaca oleh banyak bagian program sekaligus, karena tidak ada yang bisa mengubahnya di tengah jalan. Ini muncul lagi di C5: pembungkus model Core ML di FitRep dianggap aman karena isinya tidak pernah berubah.

**Di FitRep:** ambang di slide 10 disimpan sebagai pengaturan bernama di `RepetitionTracker`, misalnya `repetitionStartAngle = 140` dan `fullExtensionAngle = 145`.

**Cek pemahaman.** *Jumlah rep yang terus bertambah sebaiknya disimpan dengan let atau var?*
→ `var`, karena nilainya berubah setiap rep.

### A3. Tipe data dan fungsi
*±10 menit · Di slide 11: "One Place Decides"*

Setiap nilai punya **tipe**. Empat yang paling sering dipakai:

```swift
let fps: Int = 30               // bilangan bulat
let elbowAngle: Double = 152.4  // bilangan desimal
let isFullRep: Bool = true      // benar atau salah
let label: String = "Benar"     // teks

let window = 90                 // tanpa ditulis pun Swift
                                // tahu ini Int
```

**Fungsi** adalah resep: menerima masukan, lalu mengembalikan hasil. `-> Bool` berarti "fungsi ini mengembalikan Bool". `angle:` adalah label masukan, ditulis juga saat fungsi dipanggil.

```swift
// Aturan mulai rep dari slide 10
func isRepStarting(angle: Double) -> Bool {
    return angle < 140
}

isRepStarting(angle: 138)   // true
isRepStarting(angle: 150)   // false
```

Satu contoh lagi, dengan **array** (`[Double]` = daftar angka desimal) dan perulangan `for`. Ini rata-rata 5 frame dari slide 10 ("Average the last 5 frames"):

```swift
func average(of values: [Double]) -> Double {
    var total = 0.0
    for value in values {
        total += value
    }
    return total / Double(values.count)
}

average(of: [150, 152, 138, 151, 149])   // 148.0
```

**Fungsi murni.** Kedua fungsi di atas hasilnya hanya bergantung pada masukan, dan tidak menyentuh kamera, suara, atau layar. Masukan yang sama selalu memberi hasil yang sama, jadi mudah diuji tanpa iPhone. Inilah ide di balik poin **One Place Decides** (B2).

**Di FitRep:**
- `PoseGeometry` menghitung sudut siku dari tiga titik (bahu, siku, pergelangan).
- `MovingAverage` menghitung rata-rata 5 nilai terakhir.

**Cek pemahaman.** *Bisakah `isRepStarting(angle: 138)` memberi hasil berbeda jika dipanggil dua kali?*
→ Tidak. Fungsi murni selalu memberi hasil yang sama untuk masukan yang sama.

### A4. struct dan class
*±10 menit · Di slide 11: akar masalah data race*

Swift punya dua cara utama membuat tipe sendiri. Bedanya ada pada apa yang terjadi saat nilai "dibagikan".

**struct** seperti kertas yang difotokopi: setiap orang memegang salinannya sendiri.

```swift
struct Plan {
    var sets: Int
    var reps: Int
}

var a = Plan(sets: 3, reps: 12)
var b = a          // b = fotokopi dari a
b.reps = 15
print(a.reps)      // 12 → a tidak ikut berubah
```

**class** seperti satu papan tulis bersama: siapa pun yang memegangnya menulis di papan yang sama.

```swift
final class Scoreboard {
    var goodReps = 0
}

let x = Scoreboard()
let y = x          // y menunjuk papan yang SAMA
y.goodReps = 5
print(x.goodReps)  // 5 → x ikut berubah
```

`final` berarti class ini tidak bisa diturunkan; abaikan dulu. `let` pada class hanya mengunci *papan mana* yang dipegang, bukan isi papannya.

| | struct | class |
|---|---|---|
| Saat dibagikan | Disalin | Dipakai bersama |
| Analogi | Fotokopi | Papan tulis bersama |
| Risiko dua jalur mengubah bersamaan | Tidak ada | **Ada**, bila isinya `var` |

Baris terakhir adalah inti seluruh bagian C: **data race hanya mungkin terjadi pada data bersama yang bisa berubah**, dan class adalah tempat data seperti itu biasanya tinggal.

**Di FitRep:** kelas model Core ML yang dibuat otomatis oleh Xcode dari `BicepCurl.mlmodel` adalah class. Karena itu ia butuh perlakuan khusus (C5).

**Cek pemahaman.** *`var a = Plan(sets: 3, reps: 12)`, lalu `var b = a` dan `b.reps = 15`. Berapa `a.reps`?*
→ Tetap 12, karena struct disalin.

### A5. enum, switch, dan Optional
*±10 menit · Di slide 11: "to-do list" dan tebakan AI tiap 10 frame*

**enum** adalah daftar pilihan yang pasti; setiap pilihan bisa membawa data tambahan. **switch** memeriksa pilihan mana yang sedang berlaku, dan Swift **mewajibkan** semua pilihan ditangani.

```swift
// Tiga hasil rep dari slide 10
enum Verdict {
    case correct
    case incorrect(reason: String)
    case rejected
}

let v = Verdict.incorrect(reason: "swinging")

switch v {
case .correct:
    print("Hijau + chime, dihitung")
case .incorrect(let reason):
    print("Merah, alasan: \(reason)")
case .rejected:
    print("Diam, tidak dihitung")
}
```

`\(reason)` di dalam teks berarti "sisipkan nilai `reason` di sini".

**Optional: nilai yang mungkin kosong.** Tanda `?` setelah tipe berarti "boleh kosong". Kosong ditulis `nil`. Sebelum dipakai, isinya dibuka dengan aman memakai `if let`.

```swift
var prediction: String? = nil   // frame ini belum ada tebakan

if let p = prediction {
    print("AI menebak \(p)")
} else {
    print("Tidak ada tebakan di frame ini")
}

prediction = "Benar"            // sekarang ada isinya
```

Ada juga cara paksa, `prediction!`, yang langsung membuka isinya. Kalau ternyata kosong, aplikasi crash. Ingat ini untuk D3: Swift 6 tidak mencegah crash jenis ini.

**Di FitRep:**
- Daftar aksi untuk reducer (frame baru, timer, lewati istirahat) dan daftar efek (putar suara, jeda kamera) adalah enum.
- `FormVerdict` menyimpan alasan salah seperti `swinging` dan `poorForm`.
- AI hanya menebak tiap 10 frame, jadi hasil per frame menyimpan tebakan sebagai Optional: kebanyakan frame tidak punya tebakan.

**Cek pemahaman.** *Kenapa Swift mewajibkan switch menangani semua case?*
→ Supaya tidak ada kasus yang terlupa. Kalau kelak ada case baru, compiler menunjukkan setiap switch yang perlu diperbarui.

### A6. protocol
*±5 menit · Di slide 11: "Sendable", "View"*

**protocol** adalah daftar janji. Tipe yang "ikut" sebuah protocol wajib menepati semua janjinya, dan compiler memeriksanya.

```swift
protocol Describable {
    func describe() -> String
}

struct Rep: Describable {
    let seconds: Double

    func describe() -> String {
        "Rep selama \(seconds) detik"
    }
}
```

Tiga protocol yang akan Anda temui:
- `View`: "saya bisa digambar di layar" (B1).
- `Equatable`: "saya bisa dibandingkan dengan `==` dan `!=`".
- `Sendable`: "saya **aman dikirim** dari satu jalur kerja ke jalur lain" (C5). Int, Double, String, Bool, dan struct yang isinya Sendable otomatis menepati janji ini. Class biasa dengan `var` tidak.

**Cek pemahaman.** *Apakah `struct Plan` dari A4 aman dikirim antar-jalur?*
→ Ya. Isinya hanya Int, dan struct disalin saat dikirim, jadi tidak ada papan bersama.

---

## Bagian B · Layar dan arsitektur

Cara SwiftUI menggambar layar, lalu lima lapisan di slide 11: Screens, Runner, Decider, Rules, Tools.

### B1. SwiftUI: layar yang digambar dari data
*±10 menit · Di slide 11: Screens · "Redraw Only on Change"*

**SwiftUI** adalah cara Apple membuat tampilan. Anda tidak menggambar layar langkah demi langkah; Anda menulis **deskripsi**: "layar ini menampilkan angka rep". Saat datanya berubah, SwiftUI menggambar ulang bagian yang memakai data itu.

```swift
import SwiftUI

// Lapisan Screens
struct RepCounterView: View {
    let goodReps: Int

    var body: some View {
        Text("Rep benar: \(goodReps)")
            .font(.largeTitle)
    }
}
```

`body` adalah deskripsi tampilannya. `some View` berarti "sesuatu yang bisa digambar"; detail tipenya diurus Swift.

**ViewModel yang diamati.** Data yang dipakai layar biasanya tinggal di sebuah class bertanda `@Observable`. Setiap kali propertinya **diisi**, layar yang membacanya digambar ulang, **bahkan bila nilainya sama**. Karena itu FitRep hanya mengisi state kalau nilainya benar-benar berbeda:

```swift
// "Redraw Only on Change"
@Observable
final class WorkoutViewModel {
    var state = WorkoutState()   // WorkoutState: Equatable

    func apply(_ newState: WorkoutState) {
        if newState != state {   // hanya kalau berbeda
            state = newState     // → layar digambar ulang
        }
    }
}
```

Tanpa cek ini, 30 frame per detik berarti layar bisa digambar ulang 30 kali per detik walau tidak ada yang berubah. Itu membuang tenaga dan bisa membuat tampilan patah-patah.

Video kamera sendiri tidak lewat SwiftUI sama sekali. Preview kamera ditempel langsung ke layar sebagai *layer* khusus, jadi 30 frame video tidak memaksa SwiftUI bekerja.

**Di FitRep:**
- Pola cek: `if session.state != state`.
- Preview kamera: `AVCaptureVideoPreviewLayer`, tidak lewat SwiftUI.
- Properti internal ViewModel diberi `@ObservationIgnored` supaya perubahannya tidak memicu gambar ulang.

**Cek pemahaman.** *Kenapa `if newState != state` diperlukan?*
→ Karena mengisi nilai yang sama pun memicu gambar ulang. Dengan cek ini, layar hanya digambar saat ada perubahan nyata.

### B2. Lima lapisan dan reducer
*±10 menit · Di slide 11: Screens → Runner → Decider → Rules · Tools · "One Place Decides"*

| Di slide | Analogi | Tugasnya | Di kode FitRep |
|---|---|---|---|
| **Screens** | Ruang makan | Yang dilihat pengguna | SwiftUI views (`Features/`) |
| **Runner** | Pelayan | Hanya **menjalankan** perintah: putar suara, nyalakan timer, jeda kamera | `WorkoutSessionViewModel` · @MainActor |
| **Decider** | Kepala dapur | Menerima kejadian, menulis keadaan baru + daftar tugas. Tidak menyentuh kamera atau suara sendiri | `WorkoutSession` (reducer) |
| **Rules** | Buku resep | Rumus sudut dan pelacak rep (slide 10) | `RepetitionTracker` · `PoseGeometry` |
| **Tools** | Peralatan dapur | Kamera, deteksi pose, model AI, audio | `CameraService` · `FramePipeline` · `PoseEstimator` · `ExerciseClassifier` · `AudioService` |

**Reducer: Decider dalam kode.** Reducer adalah fungsi murni (A3) yang menerima keadaan sekarang dan satu kejadian, lalu menghasilkan keadaan baru dan daftar tugas. `inout` berarti fungsi boleh mengubah keadaan yang diberikan kepadanya.

```swift
// Lapisan Decider
enum Action {
    case repFinished(clean: Bool)
    case restSkipped
}

enum Effect {
    case playChime
    case playWrongTone
    case startCamera
}

struct WorkoutState: Equatable {
    var goodReps = 0
}

func reduce(_ state: inout WorkoutState,
            _ action: Action) -> [Effect] {
    switch action {
    case .repFinished(clean: true):
        state.goodReps += 1
        return [.playChime]
    case .repFinished(clean: false):
        return [.playWrongTone]
    case .restSkipped:
        return [.startCamera]
    }
}
```

Reducer hanya **menulis** `.playChime` di daftar tugas. Yang benar-benar memutar suara adalah Runner:

```swift
// Lapisan Runner
for effect in effects {
    switch effect {
    case .playChime:     audio.play("chime")
    case .playWrongTone: audio.play("wrong")
    case .startCamera:   camera.start()
    }
}
```

Hasilnya: semua keputusan latihan ada di satu tempat, dan tempat itu bisa diuji tanpa kamera atau iPhone.

```swift
var state = WorkoutState()
let effects = reduce(&state, .repFinished(clean: true))

// Tanpa kamera, tanpa iPhone, bisa dicek:
// state.goodReps == 1
// effects == [.playChime]
```

**Di FitRep:**
- `WorkoutSession` menerima `Action` (frame baru, timer, lewati istirahat) dan mengembalikan state baru + daftar `Effect` (suara, timer, zoom, jeda kamera, log).
- **Jujur:** test otomatisnya belum ditulis. Katakan "can be tested", jangan "tested".

**Cek pemahaman.** *Siapa yang memutar bunyi chime: Decider atau Runner?*
→ Runner. Decider hanya menulis `.playChime` di daftar tugas.

---

## Bagian C · Concurrency

*Concurrency* berarti beberapa pekerjaan berjalan bersamaan. Kamera, analisis, dan layar FitRep bekerja bersamaan di tiga jalur. Enam pelajaran ini menjelaskan kenapa itu perlu, bahayanya, dan cara Swift 6 menjaganya.

### C1. Thread dan antrean
*±8 menit · Di slide 11: judul · Camera lane*

**Thread** adalah jalur kerja. Bayangkan kasir: satu kasir melayani satu pembeli dalam satu waktu. iPhone punya banyak kasir, tetapi hanya **satu** yang boleh menggambar layar, yaitu **main thread** (jalur layar).

Kalau jalur layar disuruh menunggu sesuatu yang lambat, misalnya kamera yang butuh waktu untuk menyala, layar ikut membeku selama menunggu. Itulah arti "Without Freezing the Screen" di judul slide.

Pekerjaan lambat dipindah ke **antrean** (*queue*) lain. Antrean serial mengerjakan tugasnya satu per satu, berurutan.

```swift
// Camera lane
let sessionQueue = DispatchQueue(label: "camera.session")

sessionQueue.async {
    captureSession.startRunning()  // menunggu kamera siap,
                                   // tapi bukan di jalur layar
}
```

**Anggaran waktu 30 fps.** Pada 30 frame per detik, satu frame datang setiap ±33 milidetik. Kalau analisis satu frame lebih lama dari itu, frame berikutnya sudah datang. FitRep menyuruh kamera **membuang frame yang terlambat** supaya tidak menumpuk.

**Di FitRep:**
- Kamera depan 720p, 30 fps, `alwaysDiscardsLateVideoFrames`.
- Kamera dinyalakan di `sessionQueue`, mengikuti pola sampel AVCam Apple.
- Waktu analisis per frame di iPhone **belum diukur** (rencana L3, target < 33 ms).

**Cek pemahaman.** *Kenapa `startRunning()` tidak dipanggil di jalur layar?*
→ Karena ia menunggu sampai kamera siap. Kalau dipanggil di jalur layar, layar membeku selama menunggu.

### C2. Data race
*±8 menit · Di slide 11: "two tasks could change the same data at once"*

Perintah `goodReps += 1` terlihat seperti satu langkah, padahal komputer mengerjakannya dalam tiga langkah: **baca** nilai, **tambah** satu, **tulis** kembali. Kalau dua jalur mengerjakannya pada papan yang sama (class dari A4) di waktu yang hampir bersamaan, langkah-langkahnya bisa bertabrakan.

Contoh: dua rep bersih selesai hampir bersamaan. Jalur A dan jalur B sama-sama menjalankan `goodReps += 1`. Nilai awal 0.

**Tanpa pengaman (class biasa):**

| # | Jalur | Yang terjadi | goodReps |
|---|---|---|---|
| 1 | A | Membaca goodReps | 0 |
| 2 | B | Membaca goodReps | 0 |
| 3 | A | Menulis 0 + 1 | 1 |
| 4 | B | Menulis 0 + 1 | 1 |

✕ Hasil akhir 1, seharusnya 2. Satu rep bersih hilang.

**Dengan actor (C3):**

| # | Jalur | Yang terjadi | goodReps |
|---|---|---|---|
| 1 | A | Masuk ruangan, membaca goodReps | 0 |
| 2 | B | Menunggu di pintu | · |
| 3 | A | Menulis 0 + 1, lalu keluar | 1 |
| 4 | B | Masuk, membaca goodReps | 1 |
| 5 | B | Menulis 1 + 1, lalu keluar | 2 |

✓ Hasil akhir 2. Kedua rep tercatat.

Bahayanya, tabrakan ini **tidak selalu** terjadi. Ia hanya muncul kalau dua jalur kebetulan bertemu di waktu yang tepat. Bisa lolos seratus kali saat dicoba, lalu muncul di iPhone pengguna. Karena itu data race disebut bug acak dan sangat sulit dicari. Swift 6 tidak menunggu bug ini muncul; ia menolak kode yang *memungkinkan* tabrakan, saat compile (A1).

**Cek pemahaman.** *Kenapa data race disebut bug "acak"?*
→ Karena hanya terjadi kalau dua jalur kebetulan mengubah data yang sama di waktu yang hampir bersamaan, jadi tidak selalu muncul saat dicoba.

### C3. async/await dan actor
*±10 menit · Di slide 11: setiap jalur punya "ruangan" sendiri*

**actor** adalah tipe seperti class, tetapi isinya dijaga seperti **ruangan berpintu satu**: hanya satu pekerjaan boleh berada di dalam pada satu waktu. Yang lain menunggu di pintu. Inilah tabel "Dengan actor" di C2.

```swift
actor RepCounter {
    private var goodReps = 0

    func add() {
        goodReps += 1
    }

    func current() -> Int {
        goodReps
    }
}

let counter = RepCounter()

// Di dalam fungsi async:
await counter.add()                 // antre masuk ruangan
let total = await counter.current()
```

- `private`: isi ini hanya bisa disentuh dari dalam ruangan.
- `await`: "di titik ini saya mungkin harus menunggu giliran". Dari luar actor, setiap masuk ruangan ditandai `await`.
- `async`: fungsi yang boleh berisi `await`, yaitu boleh menunggu tanpa membekukan jalurnya.

**Di FitRep:**
- `CameraService` dan `FramePipeline` adalah actor.
- `ExerciseClassifierLoader` juga actor: memuat model sekali untuk seluruh aplikasi. Pemanggil berikutnya menunggu tugas pemuatan yang sama. Kalau gagal, cache dibuang supaya sesi berikutnya bisa mencoba lagi.

**Cek pemahaman.** *Apa arti `await` di depan `counter.add()`?*
→ Titik di mana kode boleh menunggu giliran masuk ruangan actor.

### C4. MainActor dan AsyncStream
*±8 menit · Di slide 11: Screen lane · "one small result per frame"*

**MainActor** adalah ruangan khusus milik jalur layar. Kode bertanda `@MainActor` selalu berjalan di sana, sehingga aman mengubah tampilan.

```swift
// Lapisan Runner
@MainActor
@Observable
final class WorkoutViewModel {
    var goodReps = 0   // hanya disentuh di jalur layar
}
```

Hasil analisis dari jalur lain sampai ke ruang layar lewat **AsyncStream**, seperti ban berjalan: satu sisi menaruh barang, sisi lain mengambilnya satu per satu.

```swift
// Analysis lane → Screen lane
struct FrameAnalysis: Sendable {
    let time: Double
    let prediction: String?   // tebakan AI, tidak selalu ada
}

let (stream, continuation) =
    AsyncStream.makeStream(of: FrameAnalysis.self)

// Jalur analisis: taruh hasil di ban berjalan
continuation.yield(FrameAnalysis(time: 1.2, prediction: nil))

// Jalur layar: ambil satu per satu
for await analysis in stream {
    print(analysis.time)
}
```

`FrameAnalysis` adalah struct kecil yang Sendable (A6): hanya ringkasan, bukan gambar kamera. Itulah "one small result per frame" di Screen lane.

**Di FitRep:**
- `WorkoutSessionViewModel` bertanda `@MainActor`.
- Tiap frame menghasilkan satu `FrameAnalysis` (waktu, pose, tebakan bila ada) yang dikirim lewat `AsyncStream` ke jalur layar, lalu diteruskan ke `WorkoutSession` sebagai aksi `.frameAnalyzed`.

**Cek pemahaman.** *Kenapa hasil analisis harus pindah ke MainActor?*
→ Karena hanya MainActor yang boleh mengubah tampilan.

### C5. Swift 6, Sendable, dan 3 pengecualian
*±10 menit · Di slide 11: "Swift 6 safety checks on" · "3 documented exceptions"*

Di mode bahasa Swift 5, sebagian besar pemeriksaan data race hanya berupa peringatan, atau mati. Di **Swift 6 language mode**, pemeriksaan itu menjadi **error**: kode yang bisa menyebabkan data race tidak akan bisa di-compile [13].

Aturan utamanya: data yang dikirim antar-ruangan harus **Sendable** (A6), atau compiler harus bisa membuktikan tidak ada yang memakainya lagi setelah dikirim.

```swift
// Yang ditolak Swift 6
final class Scoreboard {   // class biasa: bukan Sendable
    var goodReps = 0
}

actor Pipeline {
    func record(_ board: Scoreboard) {
        board.goodReps += 1
    }
}

func demo(pipeline: Pipeline) async {
    let board = Scoreboard()
    await pipeline.record(board) // board dikirim ke ruangan
    board.goodReps += 1          // ✕ error compile: board
                                 //   masih dipakai di luar
}
```

Kalau `Scoreboard` diganti struct, masalahnya hilang: yang dikirim hanya fotokopi.

**Pengecualian: "percaya saja, ini aman".** Kadang compiler tidak bisa membuktikan sesuatu aman, padahal programmer tahu itu aman. Swift menyediakan dua tanda pengecualian. Dengan tanda ini, tanggung jawab berpindah dari compiler ke programmer, jadi jumlahnya harus sedikit dan alasannya wajib ditulis.

| Tanda | Artinya | Di FitRep |
|---|---|---|
| `@unchecked Sendable` | "Tipe ini aman dikirim, percayalah." | 1×: `ExerciseClassifier` |
| `nonisolated(unsafe)` | "Variabel ini saya jamin dipakai dengan aman." | 2×: `CMSampleBuffer` (frame kamera) dan preview layer |

```swift
// Pengecualian 1
final class Classifier: @unchecked Sendable {
    let model: BicepCurl   // kelas buatan Xcode, bukan Sendable

    // ALASAN: model tidak pernah diganti (let), dan
    // hanya dipanggil dari satu actor (FramePipeline).

    init(model: BicepCurl) {
        self.model = model
    }
}
```

Pengecualian 2 ada di contoh C6.

**Di FitRep:**
- Chip slide 11: **Swift 6 safety checks on** (`SWIFT_VERSION = 6.0`) dan **3 documented exceptions** (1 `@unchecked Sendable` + 2 `nonisolated(unsafe)`), masing-masing dengan alasan tertulis di kode.
- `ExerciseClassifier` aman karena semua propertinya tidak pernah berubah dan `predict` hanya dipanggil dari actor pipeline.

**Cek pemahaman.** *Apa hubungan "Swift 6 safety checks on" dengan "3 documented exceptions"?*
→ Pemeriksaan aktif untuk seluruh kode. Di tiga titik, programmer mengambil alih tanggung jawab dari compiler dan menuliskan alasannya.

### C6. Executor dan assumeIsolated
*±10 menit · Di slide 11: subjudul · Analysis lane · poin 2 "No Handoffs per Frame"*

Setiap actor punya **executor**: jalur tempat kode di dalam ruangannya dijalankan. Biasanya Swift memilihkannya. FitRep memilih sendiri: ruangan `FramePipeline` ditaruh **di antrean yang sama tempat kamera mengirim frame**.

```swift
// Kerangka sederhana; kode asli bisa berbeda
actor FramePipeline {
    private let queue: DispatchSerialQueue

    init(queue: DispatchSerialQueue) {
        self.queue = queue
    }

    // "Ruangan" actor ini = antrean frame kamera
    nonisolated var unownedExecutor: UnownedSerialExecutor {
        queue.asUnownedSerialExecutor()
    }

    func analyze(_ buffer: CMSampleBuffer) {
        // Vision tiap frame, Core ML tiap 10 frame
    }
}
```

Kamera menyerahkan setiap frame lewat fungsi *callback* yang berjalan di antrean itu. Karena kode sudah berada di jalur ruangan, ia boleh langsung masuk tanpa antre memakai `assumeIsolated` ("anggap saya sudah di dalam"):

```swift
// Kerangka sederhana · callback kamera, sudah berjalan di antrean frame
func captureOutput(_ output: AVCaptureOutput,
                   didOutput sampleBuffer: CMSampleBuffer,
                   from connection: AVCaptureConnection) {
    // Pengecualian 2: frame bukan Sendable, tapi dipakai
    // langsung di antrean yang sama, jadi aman.
    nonisolated(unsafe) let buffer = sampleBuffer

    pipeline.assumeIsolated { p in
        p.analyze(buffer)   // langsung, tanpa await
    }
}
```

Bandingkan dengan cara biasa, yang memindahkan setiap frame ke loket lain lalu menunggu giliran, 30 kali per detik:

```swift
// Cara yang dihindari
Task {
    await pipeline.analyze(buffer)   // dioper dan antre lagi
}
```

**Analogi:** paket tidak dipindah ke loket lain lalu menunggu giliran; paket langsung diproses di meja tempat kurir meletakkannya.

**Pengamannya:** kalau `assumeIsolated` ternyata dipanggil dari jalur yang salah, aplikasi sengaja berhenti di baris itu. Kesalahannya langsung ketahuan saat dicoba, bukan diam-diam menjadi data race.

**Di FitRep:**
- `FramePipeline` memakai `frameQueue` sebagai executor; frame masuk secara sinkron lewat `assumeIsolated`. Antrean sebagai executor tersedia sejak iOS 17.
- Jujur: tidak ada perpindahan saat frame **masuk** analisis, tetapi hasil tiap frame tetap dikirim satu kali ke jalur layar (C4). Karena itu judul slide tidak mengklaim "zero".

**Cek pemahaman.** *Apa yang terjadi kalau `assumeIsolated` dipanggil dari jalur yang salah?*
→ Aplikasi sengaja berhenti di baris itu, sehingga kesalahan ketahuan saat dicoba dan tidak menjadi data race diam-diam.

---

## Bagian D · Slide 11 utuh

### D1. Perjalanan satu frame
*±10 menit · Di slide 11: tiga jalur*

| # | Jalur | Yang terjadi | Di kode | Konsep |
|---|---|---|---|---|
| 1 | Camera lane | **Kamera menangkap frame.** Kamera depan merekam 720p pada 30 fps. Sesinya dinyalakan di `sessionQueue`, jadi `startRunning()` yang menunggu tidak membekukan layar. Frame yang terlambat dibuang supaya tidak menumpuk. | `CameraService` (actor) · `sessionQueue` · `alwaysDiscardsLateVideoFrames` | C1, C3 |
| 2 | Analysis lane | **Frame tiba.** Kamera menyerahkan frame lewat callback yang berjalan di `frameQueue`. Frame ini (`CMSampleBuffer`) bukan Sendable, tetapi dipakai langsung di antrean yang sama: salah satu dari 3 pengecualian terdokumentasi. | `frameQueue` · `CMSampleBuffer` · `nonisolated(unsafe)` | C5, C6 |
| 3 | Analysis lane | **Masuk FramePipeline tanpa antre.** Ruangan actor `FramePipeline` berada di `frameQueue` itu sendiri, jadi frame masuk secara sinkron lewat `assumeIsolated`, tanpa await. Inilah "No Handoffs per Frame". | `FramePipeline` (actor, executor = `frameQueue`) · `assumeIsolated` | C6 |
| 4 | Analysis lane | **Vision mencari titik sendi** di setiap frame. Bila ada beberapa orang, dipilih pose dengan kotak pembatas terbesar, biasanya orang terdekat. | `PoseEstimator` · `VNDetectHumanBodyPoseRequest` | A3 |
| 5 | Analysis lane | **Window 90 frame dan tebakan AI.** Tiap 10 frame, bila minimal 54 frame berisi pose, model Core ML menebak Benar, Salah, atau Others. Frame lain tidak punya tebakan (Optional kosong). | `PoseWindow` · `ExerciseClassifier` (`@unchecked Sendable`) · `BicepCurl.mlmodel` | A5, C5 |
| 6 | Analysis lane | **Hasil ringkas ke ban berjalan.** Waktu, pose, dan tebakan (bila ada) dibungkus menjadi `FrameAnalysis`, lalu ditaruh di AsyncStream. Ini satu-satunya perpindahan per frame, dan memang perlu karena hanya MainActor yang boleh mengubah layar. | `FrameAnalysis` · `AsyncStream` | C4 |
| 7 | Screen lane | **Decider dan Rules memutuskan.** ViewModel meneruskan hasil sebagai aksi `.frameAnalyzed` ke `WorkoutSession`. `RepetitionTracker` menghitung sudut, melacak rep, dan memberi vonis saat lengan lurus lagi. Hasilnya: state baru + daftar efek. | `WorkoutSessionViewModel` (@MainActor) → `WorkoutSession.send(.frameAnalyzed)` → `RepetitionTracker` | B2 |
| 8 | Screen lane | **Runner menjalankan efek, layar diperbarui.** Misalnya chime dan pulse hijau. State hanya diisi bila berbeda, jadi SwiftUI menggambar ulang hanya saat ada perubahan. Preview kamera berjalan sendiri di layer-nya. | `WorkoutSessionViewModel` · `AudioService` · `AVCaptureVideoPreviewLayer` | B1 |

### D2. Peta slide 11 dan naskah siap ucap
*±10 menit · Seluruh slide 11*

| Teks di slide 11 | Artinya | Pelajaran |
|---|---|---|
| **30 Frames a Second, Without Freezing the Screen** | 30 frame per detik diproses di luar jalur layar | C1 |
| Each frame is analyzed where the camera delivers it | Frame masuk actor tanpa await | C6 |
| …Swift 6 rejects code where two tasks could change the same data at once [13] | Data race diperiksa saat compile | A1, C2, C5 |
| Screens · Runner · Decider · Rules · Tools | SwiftUI · ViewModel · reducer · Domain · Services | B1, B2 |
| Camera lane | `sessionQueue`: kamera menyala tanpa membekukan layar | C1 |
| Analysis lane | `frameQueue`: Vision tiap frame, AI tiap 10 frame | C6, D1 |
| Screen lane | `MainActor` + AsyncStream, satu hasil ringkas per frame | C4 |
| One Place Decides | Reducer murni, bisa diuji tanpa iPhone (test belum ditulis) | A3, B2 |
| No Handoffs per Frame | Executor actor = antrean kamera + `assumeIsolated` | C6 |
| Redraw Only on Change | Preview layer langsung + state diisi hanya bila berbeda | B1 |
| Swift 6 safety checks on | `SWIFT_VERSION = 6.0` | C5 |
| 3 documented exceptions | 1 `@unchecked Sendable` + 2 `nonisolated(unsafe)` | C5, C6 |
| iOS 17.2+ | Versi iOS minimum | — |

> **Notes slide 11 · siap ucap (EN):** "Thirty frames arrive every second. Each one is analyzed right where the camera delivers it, so frames don't pile up, and only a small result goes to the screen. All workout decisions live in one place that can be tested without a phone, and Swift 6 refuses to compile code where two tasks could change the same data at once."

> **Versi Indonesia:** "Tiga puluh frame masuk setiap detik. Setiap frame dianalisis langsung di jalur tempat kamera mengirimnya, jadi tidak menumpuk, dan hanya hasil ringkasnya yang dikirim ke layar. Semua keputusan latihan ada di satu tempat yang bisa diuji tanpa iPhone, dan Swift 6 menolak meng-compile kode yang memungkinkan dua tugas mengubah data yang sama bersamaan."

**Latihan:** ucapkan versi Inggris tiga kali sambil menunjuk jalur di slide. Targetnya ±25 detik.

### D3. Latihan tanya-jawab
*±15 menit · Pertanyaan reviewer tentang slide 11*

> Format: **T** = pertanyaan (dalam bahasa Inggris, karena reviewer kemungkinan bertanya begitu), **J** = jawaban, **Siap ucap** = kalimat bahasa Inggris. Tutup jawabannya, jawab dengan suara keras, lalu bandingkan.

**T-S1. "Walk me through the architecture."** *(B2)*
J: Lima lapisan: Screens menampilkan state, Runner menjalankan efek, Decider (reducer murni) mengubah setiap kejadian menjadi state baru + daftar tugas, Rules menghitung sudut dan rep, Tools membungkus kamera, Vision, Core ML, dan audio.
Siap ucap: *"Screens show state, a Runner executes effects, and one pure Decider turns each event into the next state plus a to-do list. Camera, Vision, Core ML and audio sit behind services."*

**T-S2. "Why a pure reducer? Is it unit-tested?"** *(A3, B2 · jawab jujur)*
J: Kejadian yang sama selalu menghasilkan keputusan yang sama, jadi logikanya bisa diuji tanpa perangkat dan efek sampingnya terlihat jelas. Test otomatisnya **belum ada**.
Siap ucap: *"It makes every decision reproducible: same event, same result, so it can be tested without a device. I haven't written the test target yet. It's next on my list."*

**T-S3. "How do frames avoid blocking the UI?"** *(C1, C4, C6)*
J: Kamera berjalan di `sessionQueue`. Frame dianalisis di `frameQueue`, yang sekaligus executor actor `FramePipeline`. Jalur layar hanya menerima hasil ringkas lewat AsyncStream.
Siap ucap: *"The camera runs on its own queue, analysis runs on the queue the camera delivers to, and the main actor only receives a small result per frame."*

**T-S4. "Is there really no handoff per frame?"** *(C4, C6)*
J: Tidak ada perpindahan saat frame masuk analisis. Hasil tiap frame tetap dikirim sekali ke MainActor, karena hanya MainActor yang boleh mengubah tampilan.
Siap ucap: *"No hop when a frame enters analysis. Each frame's result still crosses once to the main actor, because only the main actor may update the UI."*

**T-S5. "Are all 30 frames analyzed?"** *(C1 · jawab jujur)*
J: Kamera mengirim hingga 30 fps dan membuang frame yang terlambat. Waktu Vision per frame di iPhone belum diukur, jadi jangan klaim semua frame pasti dianalisis.
Siap ucap: *"The camera delivers up to 30 fps and drops late frames. I haven't measured Vision's per-frame time on device yet, so I don't claim every frame is analyzed."*

**T-S6. "Why are there unsafe exceptions in Swift 6 code?"** *(C5, C6)*
J: Ketiganya ada di titik pertemuan dengan tipe buatan Apple atau Xcode yang belum ditandai Sendable: kelas model Core ML, frame kamera, dan preview layer. Setiap pengecualian punya alasan tertulis. **[CEK]** Cocokkan dengan kode di Mac sebelum memakai jawaban ini.
Siap ucap: *"All three sit where my code meets Apple types that aren't marked Sendable. Each one has a written reason in the code, and everything else is checked by the compiler."*

**T-S7. "So Swift 6 means the app can't crash?"** *(A5, C5)*
J: Tidak. Swift 6 mencegah data race pada kode yang diperiksa. Crash jenis lain tetap mungkin, misalnya membuka Optional yang kosong dengan `!`.
Siap ucap: *"No. Swift 6 rules out data races in checked code. Other crashes, like force-unwrapping a missing value, are still possible."*

**T-S8. "How do you avoid redrawing 30 times a second?"** *(B1)*
J: Preview kamera memakai layer langsung, tidak lewat SwiftUI. State hanya diisi kalau nilainya berubah, karena mengisi nilai yang sama pun memicu gambar ulang.
Siap ucap: *"The camera preview is a layer that bypasses SwiftUI, and state is only reassigned when it actually changes."*

**T-S9. "How do you handle the model's load time?"** *(C3)*
J: Vision dan model Core ML di-warm-up saat aplikasi dibuka, bukan saat pengguna menekan Mulai. `ExerciseClassifierLoader` adalah actor yang memuat model sekali; pemanggil lain menunggu tugas yang sama.
Siap ucap: *"Vision and the Core ML model are warmed up at launch, and a loader actor loads the model once and shares it."*

**T-S10. "Why put actors on dispatch queues instead of plain actors?"** *(C6)*
J: AVFoundation mengirim frame lewat dispatch queue yang kita pilih. Kalau antrean itu dijadikan executor actor, setiap frame bisa masuk actor secara sinkron, tanpa antre lagi.
Siap ucap: *"AVFoundation delivers frames on a dispatch queue. Making that queue the actor's executor lets each frame enter the actor synchronously."*

---

## Glosarium

| Istilah | Penjelasan singkat |
|---|---|
| **actor** | Tipe yang isinya dijaga seperti ruangan berpintu satu; hanya satu pekerjaan di dalam pada satu waktu |
| **assumeIsolated** | Masuk ruangan actor secara langsung, tanpa await, karena kode sudah berjalan di jalur ruangan itu. Berhenti sengaja bila ternyata salah jalur |
| **async / await** | async: fungsi yang boleh menunggu. await: titik di mana kode mungkin menunggu giliran |
| **AsyncStream** | Ban berjalan: satu sisi menaruh nilai, sisi lain mengambil satu per satu |
| **class** | Tipe yang dipakai bersama saat dibagikan (papan tulis bersama) |
| **compiler** | Penerjemah kode ke bahasa mesin, sekaligus pemeriksa aturan sebelum aplikasi dibuat |
| **data race** | Dua jalur mengubah data yang sama di waktu yang hampir bersamaan, sehingga hasilnya kacau. Bug acak |
| **DispatchQueue** | Antrean tugas. Antrean serial mengerjakan tugas satu per satu |
| **Effect** | Tugas yang ditulis reducer untuk dijalankan Runner: putar suara, mulai timer, jeda kamera |
| **enum** | Daftar pilihan yang pasti; tiap pilihan bisa membawa data |
| **executor** | Jalur tempat kode di dalam ruangan actor dijalankan |
| **@MainActor** | Ruangan khusus jalur layar; satu-satunya yang boleh mengubah tampilan |
| **nonisolated(unsafe)** | Pengecualian: "variabel ini saya jamin dipakai dengan aman". Tanggung jawab pindah ke programmer |
| **@Observable** | Tanda pada class agar layar yang membacanya digambar ulang saat propertinya diisi |
| **Optional (`?`)** | Nilai yang boleh kosong (`nil`). Dibuka aman dengan `if let`; dibuka paksa dengan `!` bisa crash |
| **protocol** | Daftar janji kemampuan yang wajib ditepati sebuah tipe |
| **reducer** | Fungsi murni: (state, kejadian) → state baru + daftar efek. Di slide 11: Decider |
| **Sendable** | Janji "aman dikirim antar-jalur" |
| **@unchecked Sendable** | Pengecualian: "tipe ini aman dikirim, percayalah". Di FitRep dipakai sekali |
| **struct** | Tipe yang disalin saat dibagikan (fotokopi) |
| **Swift 6 language mode** | Mode bahasa di mana pemeriksaan data race menjadi error compile |
| **SwiftUI · View** | Cara Apple membuat tampilan dari deskripsi; View adalah sesuatu yang bisa digambar |
| **thread** | Jalur kerja. Main thread adalah jalur layar |
| **ViewModel** | Penghubung data dan layar. Di slide 11: Runner |

---

## Belajar lanjut

- [Swift Playgrounds](https://developer.apple.com/swift-playgrounds/): aplikasi Apple untuk mencoba kode Swift di iPad atau Mac.
- [The Swift Programming Language](https://docs.swift.org/swift-book/): buku resmi Swift. Mulai dari bab "A Swift Tour", lalu "Concurrency".
- [SwiftUI Tutorials](https://developer.apple.com/tutorials/swiftui): tutorial resmi Apple untuk membuat tampilan.
- [WWDC24 "Migrate your app to Swift 6"](https://developer.apple.com/videos/play/wwdc2024/10169/): sumber [13] di deck, tentang pemeriksaan data race di Swift 6.

---

*Fakta FitRep diambil dari `Portfolio-Slides-FitRep-2026-10-12.md` (rev. 7.2, bagian 08) dan `Belajar-FitRep.md` §9 per 16 Okt 2026. Nomor sitasi [13] mengikuti daftar pustaka deck.*
