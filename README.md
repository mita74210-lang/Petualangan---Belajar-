# Petualangan Belajar — Kuis Interaktif SD

Prototipe web kuis untuk murid SD kelas 1–6 yang memuat Bahasa Indonesia, Matematika, IPAS, Pendidikan Pancasila, PAI dan Budi Pekerti, PJOK, Bahasa Inggris, serta Seni Rupa.

## Menjalankan

1. Ekstrak folder proyek.
2. Buka `index.html` dengan browser modern (Chrome, Edge, Firefox, Safari). Untuk fitur suara lebih konsisten, gunakan Chrome/Edge dan izinkan audio.
3. Form murid meminta nama, kelas, dan mata pelajaran. Pilihan kelas otomatis menentukan fase: kelas 1–2 = A, kelas 3–4 = B, kelas 5–6 = C.
4. Untuk mencoba dasbor guru tanpa cloud, tekan **Ruang Guru** lalu gunakan PIN demo `1234`.
5. Hasil pada mode lokal tersimpan di `localStorage` browser tersebut. Mode lokal tidak menyinkronkan perangkat berbeda.

## Isi dan cakupan pembelajaran

Bank soal contoh dipetakan menurut fase A/B/C, bukan mengklaim setiap soal sebagai ATP sekolah tertentu. Tingkat kesulitan soal awal mengikuti fase dan bergerak naik/turun secara sederhana mengikuti jawaban murid. Guru tetap perlu meninjau soal, bahasa, konteks, dan tujuan pembelajaran sebelum digunakan untuk asesmen kelas.

Catatan penting:
- IPAS pada panduan CP SD mulai Fase B (kelas 3–4); pilihan IPAS kelas 1–2 dalam prototipe adalah pengenalan/pengayaan lingkungan, bukan klaim sebagai mapel IPAS terpisah pada Fase A.
- Panduan Bahasa Inggris BSKAP 2025 membahas Fase B–F. Opsi Bahasa Inggris kelas 1–2 disediakan sebagai pengayaan sekolah.
- PAI disusun untuk konteks Pendidikan Agama Islam dan Budi Pekerti. Sekolah harus memeriksa kesesuaian dengan CP/ketentuan Kementerian Agama dan modul ajar yang digunakan.
- Muatan lokal, contoh daerah, urutan materi, dan kalender pembelajaran harus disesuaikan dengan KOSP sekolah.

## Mengaktifkan sinkronisasi cloud lintas perangkat (Firebase)

Cloud **belum aktif** sampai langkah-langkah ini dilakukan dengan proyek Firebase milik sekolah.

1. Buat proyek di [Firebase Console](https://console.firebase.google.com/).
2. Tambahkan aplikasi Web dan salin konfigurasi Firebase Web App.
3. Edit `firebase-config.js`; ganti `ISI_API_KEY_FIREBASE`, `ISI_PROJECT`, `ISI_PROJECT_ID`, dan `ISI_APP_ID` menggunakan konfigurasi dari Firebase Console.
4. Firebase Console → Authentication → Sign-in method: aktifkan **Anonymous** dan **Email/Password**.
5. Buat akun email/password untuk guru di Authentication.
6. Firebase Console → Firestore Database: buat database.
7. Salin aturan dari `firestore.rules` ke tab Rules. Ganti `guru@sekolah.sch.id` dengan email guru yang diizinkan. Publikasikan rules.
8. Hosting aplikasi melalui Firebase Hosting, Netlify, Vercel, atau server HTTPS. Sebarkan URL hosting yang sama ke perangkat murid dan guru.
9. Guru masuk lewat Ruang Guru dengan email/password yang sudah dibuat. Murid mengirim hasil sebagai pengguna anonim. Dasbor guru akan membaca koleksi `submissions` secara real-time.

**Penting:** konfigurasi web Firebase bukan rahasia seperti private key, tetapi aturan Firestore adalah pengaman utama. Jangan pernah memasukkan service-account JSON/private key di browser. Buat akun guru terpisah, batasi email pada Rules, aktifkan proteksi akun, dan jangan membagikan kredensial guru kepada murid.

### Batasan keamanan prototipe

- Skor dihitung di browser, sehingga cocok untuk latihan/asesmen formatif, bukan ujian berisiko tinggi. Pengguna teknis dapat memodifikasi skor dari browser.
- Rules contoh membatasi akun anonim hanya untuk membuat dokumen baru, dan hanya email guru yang dapat membaca. Untuk produksi sekolah, idealnya pindahkan validasi hasil ke Cloud Functions/server terpercaya, tambahkan App Check/rate limiting, pembatasan domain, kebijakan retensi/penghapusan data, backup, serta audit akses.
- Identitas yang dikumpulkan hanya nama dan kelas. Tetap perlakukan sebagai data pribadi anak: gunakan nama seperlunya (hindari NISN/alamat/nomor HP), beri pemberitahuan kepada orang tua/sekolah, batasi akses guru, dan tentukan masa simpan data.
- Firebase harus dikonfigurasi dan Rules diuji sebelum link dibagikan. Tanpa konfigurasi, status yang tampil adalah **Mode lokal**.

## Fitur prototipe

- Form nama, kelas, dan mapel.
- Fase/difficulty menyesuaikan kelas 1–6 (A/B/C) dan bergerak sederhana mengikuti performa.
- Umpan balik positif atau koreksi beserta alasan setelah jawaban.
- Narasi soal/teks menggunakan Web Speech API, dengan toggle ON/OFF dan tombol dengarkan.
- Efek suara sederhana dengan Web Audio API, toggle ON/OFF.
- Soal “dengarkan lalu jawab” pada beberapa contoh soal.
- Dashboard nilai, pencarian, dan ekspor Word (`.doc`), Excel (`.xlsx`), serta PDF (`.pdf`). Pustaka ekspor dimuat dari CDN, sehingga fitur ekspor perlu koneksi internet.
- Penyimpanan lokal sebagai fallback; Firebase Firestore sebagai opsi sinkronisasi cloud.

## Referensi kurikulum utama

Referensi berikut merupakan rujukan resmi pemerintah yang diperiksa pada 10 November 2026. Materi aplikasi merupakan contoh soal turunan, bukan salinan lengkap dokumen.

- [Permendikdasmen Nomor 13 Tahun 2025 — perubahan atas Permendikbudristek Nomor 12 Tahun 2024 tentang Kurikulum](https://jdih.kemendikdasmen.go.id/sjdih/siperpu/dokumen/salinan/Permendikdasmen_No_13_Tahun_2025_ttg_Perubahan_atas_Permendikbudristek_No_12_Tahun_2024_ttg_Kurikulum_pada_Pendidikan_Anak_Usia_Dini_Jenjang_Dikdasmen.pdf)
- [Rumah Pendidikan — Pengertian Capaian Pembelajaran dan pembagian fase](https://pusatinformasi.rumahpendidikan.kemendikdasmen.go.id/hc/id/articles/52513313951257-Pengertian-Capaian-Pembelajaran-CP)
- [Panduan Pembelajaran dan Asesmen, edisi revisi 2025 (BSKAP)](https://repositori.kemendikdasmen.go.id/33290/)
- [Panduan Bahasa Indonesia, 2025 (BSKAP)](https://repositori.kemendikdasmen.go.id/33598/)
- [Panduan Matematika, 2025 (BSKAP)](https://repositori.kemendikdasmen.go.id/33608/)
- [Panduan IPAS Fase B dan C, 2025 (BSKAP)](https://repositori.kemendikdasmen.go.id/33599/)
- [Panduan Pendidikan Pancasila, 2025 (BSKAP)](https://repositori.kemendikdasmen.go.id/33626/)
- [Panduan PJOK, 2025 (BSKAP)](https://repositori.kemendikdasmen.go.id/33612/)
- [Panduan Bahasa Inggris, 2025 (BSKAP)](https://repositori.kemendikdasmen.go.id/33628/)
- [Capaian Pembelajaran Seni Rupa Fase A–F (BSKAP)](https://kurikulum.kemdikbud.go.id/file/cp/dasmen/26.%20CP%20Seni%20Rupa.pdf)
- [MDN — Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)

## Catatan lisensi dan pemakaian

Gunakan bank soal sebagai titik awal. Periksa kembali materi agama, bahasa, dan istilah daerah dengan guru mata pelajaran. Dokumen panduan pemerintah tetap menjadi rujukan utama; prototipe ini tidak menggantikan ATP, modul ajar, atau asesmen yang disusun satuan pendidikan.


## Bank soal versi diperluas
Setiap kombinasi fase (A/B/C) dan mata pelajaran kini memiliki 33 butir (3 soal awal + 30 butir pengayaan yang disusun dari kompetensi inti). Setiap sesi mengambil 10 soal secara acak, dengan pemilihan tingkat kesulitan yang mengikuti fase dan penyesuaian bertahap berdasarkan jawaban murid. Karena kelas 1–2 berada di Fase A, kelas 3–4 Fase B, dan kelas 5–6 Fase C, konten mengikuti pengelompokan fase Kurikulum Merdeka.

Catatan pedagogis: bank ini adalah bank latihan awal yang perlu ditelaah guru/sekolah sebelum asesmen resmi. PAI perlu diselaraskan dengan buku ajar dan kebijakan satuan pendidikan; IPAS dan Bahasa Inggris pada Fase A diposisikan sebagai pengayaan/pengenalan, bukan klaim mata pelajaran wajib fase tersebut.
