<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Arti Kata Gabut?", "id": "Singkatan Gaji Buta, Tidak Melakukan Apapun." },
  { "en": "Apa Arti Kata Baper?", "id": "Singkatan Bawa Perasaan, Terlalu Sensitif." },
  { "en": "Apa Arti Kata Bucin?", "id": "Singkatan Budak Cinta, Sangat Mencintai." },
  { "en": "Apa Arti Kata Mager?", "id": "Singkatan Malas Gerak, Enggan Melakukan Apapun." },
  { "en": "Apa Arti Kata Japri?", "id": "Singkatan Jalur Pribadi, Pesan Personal." },
  { "en": "Apa Arti Kata Woles?", "id": "Kebalikan Dari 'Slow', Artinya Santai." },
  { "en": "Apa Arti Kata Santuy?", "id": "Bentuk Lain Dari Kata Santai." },
  { "en": "Apa Arti Kata Mantul?", "id": "Singkatan Mantap Betul, Sangat Bagus." },
  { "en": "Apa Arti Kata Pansos?", "id": "Singkatan Panjat Sosial, Mencari Perhatian." },
  { "en": "Apa Arti Singkatan ASAP (As Soon As Possible)?", "id": "Secepat Mungkin Atau Sesegera Mungkin." },
  { "en": "Apa Arti Singkatan FYI (For Your Information)?", "id": "Sebagai Informasi Untuk Anda." },
  { "en": "Apa Arti Singkatan IMHO (In My Humble Opinion)?", "id": "Menurut Pendapat Saya Pribadi." },
  { "en": "Apa Arti Singkatan LOL (Laughing Out Loud)?", "id": "Tertawa Terbahak-Bahak Sangat Keras." },
  { "en": "Apa Arti Singkatan OTW (On The Way)?", "id": "Sedang Dalam Perjalanan Menuju Lokasi." },
  { "en": "Apa Arti Singkatan WDYM (What Do You Mean)?", "id": "Apa Maksudmu Berkata Seperti Itu?" },
  { "en": "Apa Arti Singkatan IDK (I Don't Know)?", "id": "Saya Tidak Tahu Jawabannya." },
  { "en": "Apa Arti Singkatan CMIIW (Correct Me If I'm Wrong)?", "id": "Koreksi Saya Jika Saya Salah." },
  { "en": "Apa Arti Singkatan OOT (Out Of Topic)?", "id": "Di Luar Topik Pembicaraan Utama." },
  { "en": "Apa Arti Singkatan BRB (Be Right Back)?", "id": "Akan Segera Kembali Lagi Sebentar." },
  { "en": "Apa Arti Singkatan BTW (By The Way)?", "id": "Ngomong-Ngomong, Mengubah Topik." },
  { "en": "Apa Maksud Kata Literally?", "id": "Sesuatu Yang Terjadi Secara Harfiah." },
  { "en": "Apa Maksud Kata Basically?", "id": "Pada Dasarnya Atau Inti Dasarnya." },
  { "en": "Apa Maksud Kata Prefer?", "id": "Lebih Suka Satu Pilihan Tertentu." },
  { "en": "Apa Maksud Kata Handle?", "id": "Menangani Atau Mengelola Suatu Situasi." },
  { "en": "Apa Maksud Kata Worth It?", "id": "Sesuatu Yang Sepadan Dengan Usahanya." },
  { "en": "Apa Maksud Kata Healing?", "id": "Proses Penyembuhan Diri, Liburan." },
  { "en": "Apa Maksud Kata Clingy?", "id": "Sifat Seseorang Yang Terlalu Manja." },
  { "en": "Apa Maksud Kata Random?", "id": "Sesuatu Yang Acak, Tidak Terduga." },
  { "en": "Apa Maksud Kata Vibes?", "id": "Suasana Atau Aura Yang Dirasakan." },
  { "en": "Apa Maksud Kata Urgent?", "id": "Sesuatu Yang Sangat Mendesak Dilakukan." },
  { "en": "Apa Arti Dari Deadline?", "id": "Batas Waktu Akhir Suatu Tugas." },
  { "en": "Apa Arti Dari Meeting?", "id": "Rapat Atau Pertemuan Untuk Diskusi." },
  { "en": "Apa Arti Dari Follow Up?", "id": "Menindaklanjuti Sesuatu Yang Sudah Dibahas." },
  { "en": "Apa Arti Dari Briefing?", "id": "Pengarahan Singkat Sebelum Memulai Tugas." },
  { "en": "Apa Arti Dari Overtime?", "id": "Bekerja Melebihi Jam Kerja Normal, Lembur." },
  { "en": "Apa Arti Singkatan KPI (Key Performance Indicator)?", "id": "Indikator Kunci Penilaian Kinerja." },
  { "en": "Apa Arti Singkatan SOP (Standard Operating Procedure)?", "id": "Prosedur Standar Operasi Kerja." },
  { "en": "Apa Maksud Kata Hoax?", "id": "Berita Bohong Atau Informasi Palsu." },
  { "en": "Apa Maksud Kata Valid?", "id": "Sesuatu Yang Sah, Benar, Akurat." },
  { "en": "Apa Maksud Kata Absurd?", "id": "Sesuatu Yang Konyol, Tidak Masuk Akal." },
  { "en": "Apa Maksud Kata Ambigu?", "id": "Memiliki Makna Ganda, Tidak Jelas." },
  { "en": "Apa Maksud Kata Skeptis?", "id": "Sikap Meragukan Sesuatu, Tidak Mudah Percaya." },
  { "en": "Apa Maksud Kata Ironi?", "id": "Sindiran Halus, Berkata Sebaliknya." },
  { "en": "Apa Maksud Kata Esensi?", "id": "Inti Atau Hakikat Dari Sesuatu." },
  { "en": "Apa Maksud Kata Krusial?", "id": "Sangat Penting, Menentukan, Gawat." },
  { "en": "Apa Maksud Kata Signifikan?", "id": "Sesuatu Yang Penting Dan Berarti." },
  { "en": "Apa Maksud Kata Komitmen?", "id": "Janji Atau Keterikatan Pada Sesuatu." },
  { "en": "Apa Maksud Kata Konteks?", "id": "Situasi Yang Berhubungan Dengan Kejadian." },
  { "en": "Apa Maksud Kata Fleksibel?", "id": "Sifat Mudah Menyesuaikan Diri, Luwes." },
  { "en": "Apa Maksud Kata Efisien?", "id": "Tepat Guna, Tidak Membuang Sumber Daya." },
  { "en": "Apa Maksud Kata Efektif?", "id": "Berhasil Guna, Mencapai Tujuan." },
  { "en": "Apa Arti Kata Gercep?", "id": "Singkatan Gerak Cepat, Bertindak Cepat." },
  { "en": "Apa Arti Kata Modus?", "id": "Modal Dusta, Pura-Pura Baik." },
  { "en": "Apa Arti Kata Galau?", "id": "Perasaan Bimbang, Sedih, Atau Resah." },
  { "en": "Apa Arti Kata Alay?", "id": "Gaya Berlebihan, Norak, Suka Pamer." },
  { "en": "Apa Arti Kata Lebay?", "id": "Sikap Berlebihan Dalam Merespon Sesuatu." },
  { "en": "Apa Arti Kata Kepo?", "id": "Sikap Ingin Tahu Urusan Orang Lain." },
  { "en": "Apa Arti Kata Curhat?", "id": "Singkatan Curahan Hati, Bercerita Masalah." },
  { "en": "Apa Arti Kata Baperan?", "id": "Orang Yang Mudah Terbawa Perasaan." },
  { "en": "Apa Arti Singkatan TFL (Thanks For Like)?", "id": "Terima Kasih Sudah Memberi Suka." },
  { "en": "Apa Arti Singkatan AFK (Away From Keyboard)?", "id": "Sedang Tidak Berada Di Depan Komputer." },
  { "en": "Apa Arti Singkatan GWS (Get Well Soon)?", "id": "Semoga Cepat Sembuh Dari Sakit." },
  { "en": "Apa Arti Singkatan TGIF (Thanks God It's Friday)?", "id": "Syukur Karena Hari Jumat Tiba." },
  { "en": "Apa Arti Singkatan IMO (In My Opinion)?", "id": "Menurut Pendapat Atau Opini Saya." },
  { "en": "Apa Arti Singkatan NGL (Not Gonna Lie)?", "id": "Jujur Saja, Tidak Akan Berbohong." },
  { "en": "Apa Arti Singkatan TMI (Too Much Information)?", "id": "Terlalu Banyak Informasi Yang Diberikan." },
  { "en": "Apa Arti Singkatan POV (Point Of View)?", "id": "Sudut Pandang Seseorang Melihat Sesuatu." },
  { "en": "Apa Arti Singkatan TBH (To Be Honest)?", "id": "Sejujurnya, Berkata Yang Sebenarnya." },
  { "en": "Apa Arti Singkatan RN (Right Now)?", "id": "Saat Ini Juga, Sekarang Juga." },
  { "en": "Apa Maksud Kata Cringe?", "id": "Merasa Geli Atau Jijik Melihat Sesuatu." },
  { "en": "Apa Maksud Kata Relate?", "id": "Merasa Terhubung Dengan Suatu Cerita." },
  { "en": "Apa Maksud Kata Ghosting?", "id": "Menghilang Tiba-Tiba Tanpa Kabar." },
  { "en": "Apa Maksud Kata Spill?", "id": "Membocorkan Rahasia Atau Cerita." },
  { "en": "Apa Maksud Kata Toxic?", "id": "Sesuatu Yang Beracun, Buruk, Negatif." },
  { "en": "Apa Maksud Kata Salty?", "id": "Merasa Kesal Atau Tersinggung (Gerah)." },
  { "en": "Apa Maksud Kata Savage?", "id": "Tindakan Keren, Berani, Atau Brutal." },
  { "en": "Apa Maksud Kata Swag?", "id": "Gaya Keren Atau Penampilan Menarik." },
  { "en": "Apa Maksud Kata Halu?", "id": "Singkatan Halusinasi, Mengkhayal Terlalu Tinggi." },
  { "en": "Apa Maksud Kata Insecure?", "id": "Merasa Tidak Aman, Kurang Percaya Diri." },
  { "en": "Apa Maksud Kata Overthinking?", "id": "Berpikir Berlebihan Tentang Sesuatu." },
  { "en": "Apa Maksud Kata Effort?", "id": "Usaha Keras Yang Dilakukan Seseorang." },
  { "en": "Apa Maksud Kata Privilege?", "id": "Keuntungan Atau Hak Istimewa Khusus." },
  { "en": "Apa Maksud Kata Circle?", "id": "Lingkaran Pertemanan Atau Pergaulan." },
  { "en": "Apa Maksud Kata Virtual?", "id": "Sesuatu Yang Tidak Nyata, Maya." },
  { "en": "Apa Maksud Kata Relevan?", "id": "Sesuatu Yang Berkaitan Atau Berhubungan." },
  { "en": "Apa Maksud Kata Konsisten?", "id": "Sikap Teguh, Tetap, Tidak Berubah-Ubah." },
  { "en": "Apa Maksud Kata Spontan?", "id": "Melakukan Sesuatu Tanpa Rencana." },
  { "en": "Validasi Itu Artinya Apa?", "id": "Proses Pembuktian Kebenaran Sesuatu." },
  { "en": "Verifikasi Itu Artinya Apa?", "id": "Proses Pemeriksaan Kebenaran Data." },
  { "en": "Apa Arti Kata Hype?", "id": "Sesuatu Yang Sedang Ramai Dibicarakan." },
  { "en": "Apa Arti Kata Lowkey?", "id": "Melakukan Sesuatu Secara Diam-Diam." },
  { "en": "Apa Arti Kata Highkey?", "id": "Melakukan Sesuatu Secara Terbuka." },
  { "en": "Apa Arti Kata Simp?", "id": "Orang Yang Terlalu Tunduk Pada Pasangan." },
  { "en": "Apa Arti Kata FOMO (Fear Of Missing Out)?", "id": "Takut Ketinggalan Tren Atau Momen." },
  { "en": "Apa Arti Kata JOMO (Joy Of Missing Out)?", "id": "Menikmati Momen Ketinggalan Tren." },
  { "en": "Apa Arti Kata FWB (Friends With Benefits)?", "id": "Teman Tapi Memiliki Hubungan Fisik." },
  { "en": "Apa Arti Kata TTM (Teman Tapi Mesra)?", "id": "Teman Biasa Tapi Bersikap Mesra." },
  { "en": "Apa Arti Kata HTS (Hubungan Tanpa Status)?", "id": "Hubungan Dekat Tanpa Status Pacaran." },
  { "en": "Apa Arti Kata Resign?", "id": "Berhenti Atau Mengundurkan Diri Dari Pekerjaan." },
  { "en": "Apa Arti Kata Freelance?", "id": "Bekerja Lepas, Tidak Terikat Kontrak Tetap." },
  { "en": "Apa Arti Kata Hybrid?", "id": "Metode Kerja Campuran (Kantor Dan Rumah)." },
  { "en": "Apa Arti Singkatan WFH (Work From Home)?", "id": "Bekerja Dari Rumah Secara Penuh." },
  { "en": "Apa Arti Singkatan WFO (Work From Office)?", "id": "Bekerja Dari Kantor Seperti Biasa." },
  { "en": "Apa Arti Kata Typing?", "id": "Sedang Mengetik Pesan Balasan." },
  { "en": "Apa Arti Kata Typo?", "id": "Kesalahan Mengetik Satu Huruf Atau Kata." },
  { "en": "Apa Arti Singkatan VC (Video Call)?", "id": "Panggilan Video, Tatap Muka Virtual." },
  { "en": "Apa Arti Singkatan VN (Voice Note)?", "id": "Pesan Suara Yang Dikirim Via Chat." },
  { "en": "Apa Arti Singkatan PC (Personal Chat)?", "id": "Pesan Pribadi, Bukan Di Grup." },
  { "en": "Apa Arti Kata PAP (Post A Picture)?", "id": "Permintaan Mengirim Foto Saat Itu Juga." },
  { "en": "Apa Arti Kata Gaje?", "id": "Singkatan Tidak Jelas, Aneh." },
  { "en": "Apa Arti Kata Komuk?", "id": "Singkatan Kondisi Muka Atau Ekspresi Wajah." },
  { "en": "Apa Arti Kata Gabut?", "id": "Kondisi Bosan Tidak Ada Kerjaan." },
  { "en": "Apa Arti Kata Gemoy?", "id": "Bentuk Lain Dari Kata Gemas Sekali." },
  { "en": "Apa Arti Kata Nego?", "id": "Negosiasi, Proses Tawar Menawar Harga." },
  { "en": "Apa Arti Kata Deal?", "id": "Kesepakatan Tercapai, Setuju." },
  { "en": "Apa Arti Kata COD (Cash On Delivery)?", "id": "Bayar Tunai Saat Barang Tiba." },
  { "en": "Apa Arti Kata Preloved?", "id": "Barang Bekas Berkualitas Yang Dijual Kembali." },
  { "en": "Apa Arti Kata Thrift?", "id": "Membeli Barang Bekas, Hemat." },
  { "en": "Apa Maksud Kata Estetik?", "id": "Sesuatu Yang Indah Dilihat, Artistik." },
  { "en": "Apa Maksud Kata Hidden Gem?", "id": "Tempat Bagus Yang Belum Diketahui Orang." },
  { "en": "Apa Maksud Kata Jujurly?", "id": "Bentuk Lain Kata 'Jujur Saja'." },
  { "en": "Apa Maksud Kata Ngab?", "id": "Panggilan 'Bang' Yang Dibalik." },
  { "en": "Apa Maksud Kata Sabi?", "id": "Kata 'Bisa' Yang Dibalik." },
  { "en": "Apa Maksud Kata Kuy?", "id": "Kata 'Yuk' Yang Dibalik, Ajakan." },
  { "en": "Apa Maksud Kata Takis?", "id": "Kata 'Sikat' Yang Dibalik, Ambil." },
  { "en": "Apa Maksud Kata Red Flag?", "id": "Tanda Bahaya Dalam Suatu Hubungan." },
  { "en": "Apa Maksud Kata Green Flag?", "id": "Tanda Baik Dalam Suatu Hubungan." },
  { "en": "Apa Maksud Kata Gaslighting?", "id": "Manipulasi Psikologis Membuat Orang Ragu." },
  { "en": "Apa Maksud Kata Trust Issue?", "id": "Masalah Kepercayaan Terhadap Orang Lain." },
  { "en": "Apa Maksud Kata Support System?", "id": "Lingkungan Yang Memberi Dukungan Mental." },
  { "en": "Apa Maksud Kata Burnout?", "id": "Kelelahan Fisik Dan Mental Akibat Kerja." },
  { "en": "Apa Maksud Kata Mental Breakdown?", "id": "Kondisi Stres Akut Dan Emosional." },
  { "en": "Apa Maksud Kata Quarter Life Crisis?", "id": "Krisis Emosional Di Usia 20-An." },
  { "en": "Apa Maksud Kata Self Love?", "id": "Mencintai Diri Sendiri Sepenuhnya." },
  { "en": "Apa Maksud Kata Self Reward?", "id": "Memberi Penghargaan Pada Diri Sendiri." },
  { "en": "Apa Maksud Kata Mindset?", "id": "Pola Pikir Seseorang Dalam Melihat Sesuatu." },
  { "en": "Apa Maksud Kata Attitude?", "id": "Sikap Atau Perilaku Seseorang." },
  { "en": "Apa Maksud Kata Respect?", "id": "Rasa Hormat Terhadap Orang Lain." },
  { "en": "Apa Maksud Kata Anxiety?", "id": "Perasaan Cemas Atau Khawatir Berlebihan." },
  { "en": "Apa Maksud Kata Mood?", "id": "Suasana Hati Seseorang Saat Ini." },
  { "en": "Apa Maksud Kata Confident?", "id": "Merasa Percaya Diri." },
  { "en": "Apa Maksud Kata Humble?", "id": "Sikap Rendah Hati, Tidak Sombong." },
  { "en": "Apa Maksud Kata Loyal?", "id": "Sikap Setia Pada Sesuatu." },
  { "en": "Apa Maksud Kata Netizen?", "id": "Warga Internet, Pengguna Dunia Maya." },
  { "en": "Apa Maksud Kata Influencer?", "id": "Orang Yang Mempengaruhi Opini Publik." },
  { "en": "Apa Maksud Kata Endorse?", "id": "Mempromosikan Produk Atau Jasa (Berbayar)." },
  { "en": "Apa Maksud Kata Giveaway?", "id": "Memberikan Hadiah Gratis Kepada Pengikut." },
  { "en": "Apa Maksud Kata Unboxing?", "id": "Video Membuka Paket Atau Produk Baru." },
  { "en": "Apa Maksud Kata Review?", "id": "Ulasan Atau Penilaian Terhadap Produk." },
  { "en": "Apa Maksud Kata Disclaimer?", "id": "Pernyataan Penyangkalan Tanggung Jawab." },
  { "en": "Apa Maksud Kata Workshop?", "id": "Pelatihan Atau Lokakarya Singkat." },
  { "en": "Apa Maksud Kata Webinar?", "id": "Seminar Yang Dilakukan Secara Online." },
  { "en": "Apa Maksud Kata Podcast?", "id": "Siaran Audio Digital Mirip Radio." },
  { "en": "Apa Maksud Kata Playlist?", "id": "Daftar Putar Lagu Atau Video." },
  { "en": "Apa Maksud Kata Update?", "id": "Memperbarui Sesuatu Ke Versi Terbaru." },
  { "en": "Apa Maksud Kata Upgrade?", "id": "Meningkatkan Sesuatu Ke Kualitas Lebih Baik." },
  { "en": "Apa Maksud Kata Download?", "id": "Proses Mengunduh Data Dari Internet." },
  { "en": "Apa Maksud Kata Upload?", "id": "Proses Mengunggah Data Ke Internet." },
  { "en": "Apa Arti Singkatan GG (Good Game)?", "id": "Permainan Yang Bagus (Pujian)." },
  { "en": "Apa Arti Singkatan Noob?", "id": "Sebutan Untuk Pemain Baru, Amatir." },
  { "en": "Apa Arti Kata Pro?", "id": "Sebutan Untuk Pemain Profesional, Ahli." },
  { "en": "Apa Arti Kata Mabar?", "id": "Singkatan Main Bareng (Game Online)." },
  { "en": "Apa Arti Kata Ngelag?", "id": "Kondisi Jaringan Internet Lambat, Patah-Patah." },
  { "en": "Apa Arti Kata AFK (Away From Keyboard)?", "id": "Meninggalkan Permainan Untuk Sementara." },
  { "en": "Apa Arti Kata Damage?", "id": "Tingkat Kerusakan Serangan Dalam Game." },
  { "en": "Apa Arti Kata Imba?", "id": "Singkatan 'Imbalance', Terlalu Kuat." },
  { "en": "Apa Arti Singkatan MVP (Most Valuable Player)?", "id": "Pemain Paling Berharga Dalam Tim." },
  { "en": "Apa Arti Kata Spam?", "id": "Tindakan Berulang Yang Mengganggu." },
  { "en": "Apa Arti Kata Rizz?", "id": "Karismatik, Kemampuan Memikat Lawan Jenis." },
  { "en": "Apa Arti Kata Sus?", "id": "Singkatan 'Suspicious', Mencurigakan." },
  { "en": "Apa Arti Kata Flexing?", "id": "Memamerkan Harta Atau Gaya Hidup Mewah." },
  { "en": "Apa Arti Kata Glow Up?", "id": "Perubahan Penampilan Menjadi Jauh Lebih Baik." },
  { "en": "Apa Arti Kata Bestie?", "id": "Panggilan Akrab Untuk Sahabat Dekat." },
  { "en": "Apa Arti Kata Crush?", "id": "Orang Yang Sedang Kita Suka Diam-Diam." },
  { "en": "Apa Arti Kata PDKT?", "id": "Singkatan Pendekatan Sebelum Pacaran." },
  { "en": "Apa Arti Kata PHP (Pemberi Harapan Palsu)?", "id": "Memberi Harapan Tapi Tidak Menepati." },
  { "en": "Apa Arti Kata CLBK (Cinta Lama Bersemi Kembali)?", "id": "Balikan Dengan Mantan Pacar." },
  { "en": "Apa Arti Kata Move On?", "id": "Melanjutkan Hidup, Melupakan Masa Lalu." },
  { "en": "Apa Arti Kata Gamon?", "id": "Singkatan Gagal Move On, Sulit Melupakan." },
  { "en": "Apa Arti Kata Salting?", "id": "Singkatan Salah Tingkah, Menjadi Gugup." },
  { "en": "Apa Arti Kata Friendzone?", "id": "Zona Pertemanan, Cinta Tak Terbalas." },
  { "en": "Apa Arti Kata Brotherzone?", "id": "Dianggap Kakak, Bukan Sebagai Pasangan." },
  { "en": "Apa Arti Kata Denial?", "id": "Menyangkal Atau Tidak Menerima Kenyataan." },
  { "en": "Apa Arti Kata Ilfeel?", "id": "Singkatan Hilang Perasaan (Feeling)." },
  { "en": "Apa Arti Kata Me Time?", "id": "Waktu Untuk Menikmati Kesendirian." },
  { "en": "Apa Arti Kata Quality Time?", "id": "Waktu Berkualitas Bersama Orang Terdekat." },
  { "en": "Apa Arti Kata Staycation?", "id": "Liburan Dengan Menginap Di Hotel Lokal." },
  { "en": "Apa Arti Kata Workaholic?", "id": "Orang Yang Kecanduan Bekerja Keras." },
  { "en": "Apa Arti Kata Introvert?", "id": "Nyaman Sendiri, Energi Habis Saat Ramai." },
  { "en": "Apa Arti Kata Ekstrovert?", "id": "Nyaman Di Keramaian, Dapat Energi." },
  { "en": "Apa Arti Kata Ambivert?", "id": "Gabungan Sifat Introvert Dan Ekstrovert." },
  { "en": "Apa Arti Kata Judgmental?", "id": "Sikap Suka Menghakimi Orang Lain." },
  { "en": "Apa Arti Kata TBL (Takut Banget Loh)?", "id": "Ungkapan Menyatakan Rasa Sangat Takut." },
  { "en": "Apa Arti Kata SBL (Sebel Banget Loh)?", "id": "Ungkapan Menyatakan Rasa Sangat Sebal." },
  { "en": "Apa Arti Kata CBL (Capek Banget Loh)?", "id": "Ungkapan Menyatakan Rasa Sangat Lelah." },
  { "en": "Apa Arti Kata NBL (Ngakak Banget Loh)?", "id": "Ungkapan Menyatakan Tertawa Sangat Keras." },
  { "en": "Apa Arti Kata Plot Twist?", "id": "Alur Cerita Yang Mengejutkan, Penuh Kejutan." },
  { "en": "Apa Arti Kata POV (Point Of View) Di Medsos?", "id": "Sudut Pandang Pengguna Dalam Konten." },
  { "en": "Apa Arti Kata Pick Me?", "id": "Orang Yang Berusaha Keras Tampil Beda." },
  { "en": "Apa Arti Kata Cegil?", "id": "Singkatan Dari 'Cewek Gila', Intens." },
  { "en": "Apa Arti Kata Cokul?", "id": "Singkatan Dari 'Cowok Cool', Keren." },
  { "en": "Apa Arti Kata Flop?", "id": "Sesuatu Yang Gagal Total, Tidak Laku." },
  { "en": "Apa Arti Kata Era?", "id": "Masa Atau Periode Tertentu (Misal: Era Bucin)." },
  { "en": "Apa Arti Kata Body Count?", "id": "Jumlah Pasangan Seksual Yang Dimiliki." },
  { "en": "Apa Arti Kata Delulu?", "id": "Singkatan 'Delusional', Percaya Khayalan Sendiri." },
  { "en": "Apa Arti Kata Solulu?", "id": "Singkatan 'Solusi', Jawaban Masalah." },
  { "en": "Apa Arti Kata Nolep?", "id": "Singkatan 'No Life', Tidak Punya Kehidupan Sosial." },
  { "en": "Apa Arti Kata Sotoy?", "id": "Singkatan 'Sok Tahu', Merasa Paling Tahu." },
  { "en": "Apa Arti Kata Songong?", "id": "Sikap Sombong, Angkuh, Atau Belagu." },
  { "en": "Apa Arti Kata Julid?", "id": "Suka Berkomentar Negatif, Iri, Sinis." },
  { "en": "Apa Arti Kata Nyinyir?", "id": "Suka Mengomentari Pedas Dan Meremehkan." },
  { "en": "Apa Arti Kata Binal?", "id": "Sikap Agresif Secara Seksual (Biasanya Wanita)." },
  { "en": "Apa Arti Kata SJW (Social Justice Warrior)?", "id": "Pejuang Keadilan Sosial (Seringkali Negatif)." },
  { "en": "Apa Arti Kata NPC (Non-Player Character)?", "id": "Orang Yang Bertindak Seperti Robot, Pasif." },
  { "en": "Apa Arti Kata Based?", "id": "Sikap Jujur, Berani, Tanpa Peduli Opini." },
  { "en": "Apa Arti Kata Main Character?", "id": "Merasa Menjadi Tokoh Utama Kehidupan." },
  { "en": "Apa Arti Kata Side Character?", "id": "Merasa Menjadi Tokoh Sampingan." },
  { "en": "Apa Arti Kata Backingan?", "id": "Orang Kuat Yang Melindungi Seseorang." },
  { "en": "Apa Arti Kata Cuan?", "id": "Keuntungan Finansial, Uang." },
  { "en": "Apa Arti Kata Boncos?", "id": "Mengalami Kerugian Finansial, Rugi." },
  { "en": "Apa Arti Kata Hoki?", "id": "Keberuntungan Yang Tidak Terduga." },
  { "en": "Apa Arti Kata Sial?", "id": "Nasib Buruk Atau Malang." },
  { "en": "Apa Arti Kata Apes?", "id": "Bentuk Lain Dari Sial, Kurang Beruntung." },
  { "en": "Apa Arti Kata Empati?", "id": "Ikut Merasakan Perasaan Orang Lain." },
  { "en": "Apa Arti Kata Simpati?", "id": "Menunjukkan Kepedulian, Tidak Ikut Merasakan." },
  { "en": "Apa Arti Kata Apatis?", "id": "Sikap Acuh Tak Acuh, Tidak Peduli." },
  { "en": "Apa Arti Kata Euforia?", "id": "Perasaan Gembira Yang Sangat Berlebihan." },
  { "en": "Apa Arti Kata Nostalgia?", "id": "Kenangan Manis Pada Masa Lalu." },
  { "en": "Apa Arti Kata Melankolis?", "id": "Sifat Murung, Sedih, Penuh Perenungan." },
  { "en": "Apa Arti Kata Stigma?", "id": "Label Negatif Dari Masyarakat." },
  { "en": "Apa Arti Kata Narasi?", "id": "Cerita Atau Sudut Pandang Tertentu." },
  { "en": "Apa Arti Kata Literasi?", "id": "Kemampuan Membaca Dan Memahami Informasi." },
  { "en": "Apa Arti Kata Urgensi?", "id": "Tingkat Kepentingan Sesuatu, Mendesak." },
  { "en": "Apa Arti Kata Esensial?", "id": "Sesuatu Yang Sangat Penting, Mendasar." },
  { "en": "Apa Arti Kata Stagnan?", "id": "Berhenti Di Tempat, Tidak Berkembang." },
  { "en": "Apa Arti Kata Progresif?", "id": "Berkembang Maju, Ada Kemajuan." },
  { "en": "Apa Arti Kata Implisit?", "id": "Sesuatu Yang Tersirat, Tidak Langsung." },
  { "en": "Apa Arti Kata Eksplisit?", "id": "Sesuatu Yang Tegas, Jelas, Langsung." },
  { "en": "Apa Arti Kata Konspirasi?", "id": "Teori Rahasia Dibalik Suatu Peristiwa." },
  { "en": "Apa Arti Kata Skeptis?", "id": "Sikap Meragukan Sesuatu, Tidak Percaya." },
  { "en": "Apa Arti Kata Oportunis?", "id": "Orang Yang Mengambil Untung Dari Kesempatan." },
  { "en": "Apa Arti Kata Realistis?", "id": "Melihat Sesuatu Sesuai Kenyataan." },
  { "en": "Apa Arti Kata Idealis?", "id": "Melihat Sesuatu Sesuai Harapan Sempurna." },
  { "en": "Apa Arti Kata Pragmatis?", "id": "Berpikir Praktis Dan Berorientasi Hasil." },
  { "en": "Apa Arti Kata Pasif?", "id": "Sikap Menerima, Tidak Proaktif." },
  { "en": "Apa Arti Kata Agresif?", "id": "Sikap Menyerang, Cenderung Memaksa." },
  { "en": "Apa Arti Kata Pasif-Agresif?", "id": "Menunjukkan Agresi Secara Tidak Langsung." },
  { "en": "Apa Arti Kata Asertif?", "id": "Tegas Menyatakan Pendapat Tanpa Agresif." },
  { "en": "Apa Arti Kata Konsumtif?", "id": "Sifat Suka Berbelanja Berlebihan, Boros." },
  { "en": "Apa Arti Kata Hedonisme?", "id": "Paham Yang Mengejar Kesenangan Duniawi." },
  { "en": "Apa Arti Kata Minimalis?", "id": "Gaya Hidup Dengan Barang Secukupnya." },
  { "en": "Apa Arti Kata Frugal Living?", "id": "Gaya Hidup Hemat Dan Cermat Finansial." },
  { "en": "Apa Arti Kata Disclaimer?", "id": "Pernyataan Penyangkalan Atau Batasan." },
  { "en": "Apa Arti Kata Integritas?", "id": "Kejujuran Dan Konsistensi Moral." },
  { "en": "Apa Arti Kata Kredibilitas?", "id": "Tingkat Kepercayaan Publik Terhadap Seseorang." },
  { "en": "Apa Arti Kata Loyalitas?", "id": "Kepatuhan Atau Kesetiaan Seseorang." },
  { "en": "Signifikan Itu Artinya Apa?", "id": "Berpengaruh Besar, Penting, Berarti." },
  { "en": "Fundamental Itu Artinya Apa?", "id": "Sesuatu Yang Sangat Mendasar, Pokok." },
  { "en": "Krusial Itu Artinya Apa?", "id": "Sangat Penting, Gawat, Menentukan." },
  { "en": "Relatif Itu Artinya Apa?", "id": "Tidak Mutlak, Tergantung Pembandingnya." },
  { "en": "Objektif Itu Artinya Apa?", "id": "Menilai Berdasarkan Fakta, Tidak Subjektif." },
  { "en": "Subjektif Itu Artinya Apa?", "id": "Menilai Berdasarkan Perasaan Pribadi." },
  { "en": "Apa Arti Kata Worthy?", "id": "Merasa Layak Atau Pantas Mendapatkan." },
  { "en": "Apa Arti Kata Clueless?", "id": "Sama Sekali Tidak Tahu Apapun." },
  { "en": "Apa Arti Kata Cuddling?", "id": "Berpelukan Mesra Untuk Kenyamanan." },
  { "en": "Apa Arti Kata Triggered?", "id": "Tersinggung Atau Terpancing Emosinya." },
  { "en": "Apa Arti Kata Awkward?", "id": "Perasaan Canggung Atau Kikuk." },
  { "en": "Apa Arti Kata Legit?", "id": "Singkatan 'Legitimate', Asli, Sah, Keren." },
  { "en": "Apa Arti Kata Slebew?", "id": "Kata Slang Untuk Sesuatu Yang Sensual." },
  { "en": "Apa Arti Kata YGY (Ya Guys Ya)?", "id": "Penegasan Kalimat, 'Ya Kan'." },
  { "en": "Apa Arti Kata NT (Nice Try)?", "id": "Pujian Atas Usaha Yang Gagal." },
  { "en": "Apa Arti Kata Damage (Konteks Gaul)?", "id": "Sesuatu Yang Sangat Mempesona." },
  { "en": "Apa Arti Kata Prenjon?", "id": "Pelesetan Dari 'Friendzone'." },
  { "en": "Apa Arti Kata Mleyot?", "id": "Sangat Lemas Karena Terlalu Suka." },
  { "en": "Apa Arti Kata Ngadi-Ngadi?", "id": "Berkelakuan Aneh, Mengada-Ada." },
  { "en": "Apa Arti Kata Rungkad?", "id": "Hancur Lebur, Bangkrut, Kalah Telak." },
  { "en": "Apa Arti Kata Tepar?", "id": "Kondisi Sangat Lelah Hingga Tak Berdaya." },
  { "en": "Apa Arti Kata Healing (Konteks Gaul)?", "id": "Jalan-Jalan Atau Liburan Singkat." },
  { "en": "Apa Arti Kata Deep Talk?", "id": "Obrolan Mendalam Tentang Hal Pribadi." },
  { "en": "Apa Arti Kata Small Talk?", "id": "Obrolan Ringan, Basa-Basi." },
  { "en": "Apa Arti Kata Cancel Culture?", "id": "Budaya Memboikot Tokoh Publik Bermasalah." },
  { "en": "Apa Arti Kata WTB (Want To Buy)?", "id": "Pesan Yang Menandakan Ingin Membeli." },
  { "en": "Apa Arti Kata WTS (Want To Sell)?", "id": "Pesan Yang Menandakan Ingin Menjual." },
  { "en": "Apa Arti Kata WTT (Want To Trade)?", "id": "Pesan Yang Menandakan Ingin Tukar Barang." },
  { "en": "Apa Arti Kata Niche?", "id": "Segmen Pasar Yang Sangat Spesifik." },
  { "en": "Apa Arti Kata Mainstream?", "id": "Sesuatu Yang Umum, Populer, Biasa." },
  { "en": "Apa Arti Kata Underground?", "id": "Sesuatu Yang Tidak Populer, Di Luar Arus." },
  { "en": "Apa Arti Kata Skena?", "id": "Suasana Atau Gaya Kolektif (Musik, Fashion)." },
  { "en": "Apa Arti Kata Polisi Skena?", "id": "Orang Yang Merasa Paling Tahu Skena." },
  { "en": "Apa Arti Kata Circle Kopi?", "id": "Lingkaran Pertemanan Pecinta Kopi." },
  { "en": "Apa Arti Kata OVT (Overthinking)?", "id": "Berpikir Berlebihan Akan Sesuatu Hal." },
  { "en": "Apa Arti Kata Closure?", "id": "Penerimaan, Penutupan Babak Hidup." },
  { "en": "Apa Arti Kata Relationship Goals?", "id": "Standar Ideal Sebuah Hubungan Asmara." },
  { "en": "Apa Arti Kata Family Goals?", "id": "Standar Ideal Sebuah Keluarga Harmonis." },
  { "en": "Apa Arti Kata Situationship?", "id": "Hubungan Tanpa Status, Lebih Dari Teman." },
  { "en": "Apa Arti Kata Dry Text?", "id": "Pesan Teks Singkat, Datar, Tidak Tertarik." },
  { "en": "Apa Arti Kata Love Bombing?", "id": "Pemberian Cinta Berlebihan Di Awal." },
  { "en": "Apa Arti Kata Silent Treatment?", "id": "Hukuman Mendiamkan Seseorang." },
  { "en": "Apa Arti Kata Breadcrumbing?", "id": "Memberi Harapan Kecil Tanpa Niat Serius." },
  { "en": "Apa Arti Kata Benching?", "id": "Menjadikan Seseorang Pilihan Cadangan." },
  { "en": "Apa Arti Kata Orbiting?", "id": "Mengamati Medsos Mantan Tanpa Interaksi." },
  { "en": "Apa Arti Kata Cuffing Season?", "id": "Musim Mencari Pasangan Jangka Pendek." },
  { "en": "Apa Arti Kata Slow Burn?", "id": "Hubungan Yang Berkembang Perlahan." },
  { "en": "Apa Arti Kata Ick?", "id": "Perasaan Jijik Tiba-Tiba Pada Pasangan." },
  { "en": "Apa Arti Kata Beige Flag?", "id": "Tanda Aneh Tapi Tidak Berbahaya." },
  { "en": "Apa Arti Kata Clickbait?", "id": "Judul Menjebak Untuk Mendapat Klik." },
  { "en": "Apa Arti Kata Netizen Budiman?", "id": "Warganet Yang Berkomentar Baik." },
  { "en": "Apa Arti Kata Netizen Maha Benar?", "id": "Warganet Yang Suka Menghakimi." },
  { "en": "Apa Arti Kata Buzzer?", "id": "Orang Dibayar Mendengungkan Isu." },
  { "en": "Apa Arti Kata Pargoy?", "id": "Goyangan Pesta (Populer Di TikTok)." },
  { "en": "Apa Arti Kata Yaudah?", "id": "Singkatan 'Ya Sudah', Pasrah." },
  { "en": "Apa Arti Kata Paripurna?", "id": "Sempurna, Lengkap, Tanpa Cacat." },
  { "en": "Apa Arti Kata HQQ (Hakiki)?", "id": "Sesuatu Yang Paling Sebenarnya." },
  { "en": "Apa Arti Kata Fomo (Fear Of Missing Out)?", "id": "Takut Ketinggalan Berita Atau Tren." },
  { "en": "Apa Arti Kata Jomo (Joy Of Missing Out)?", "id": "Bahagia Ketinggalan Tren." },
  { "en": "Apa Arti Kata OOMF (One Of My Followers)?", "id": "Salah Satu Pengikut Saya (Media Sosial)." },
  { "en": "Apa Arti Kata Mutualan?", "id": "Saling Mengikuti Di Media Sosial." },
  { "en": "Apa Arti Kata Repost?", "id": "Mengunggah Ulang Konten Orang Lain." },
  { "en": "Apa Arti Kata Mention?", "id": "Menyebut Nama Akun Lain (Tagging)." },
  { "en": "Apa Arti Kata Thread?", "id": "Rangkaian Cuitan (Tweet) Yang Bersambung." },
  { "en": "Apa Arti Kata Spill The Tea?", "id": "Membongkar Gosip Atau Rahasia Panas." },
  { "en": "Apa Arti Kata Salty (Konteks Gaul)?", "id": "Merasa Kesal, Iri, Atau Sinis." },
  { "en": "Apa Arti Kata Shook?", "id": "Sangat Terkejut, Kaget, Heran." },
  { "en": "Apa Arti Kata Stan?", "id": "Penggemar Garis Keras (Stalker Fan)." },
  { "en": "Apa Arti Kata Ship?", "id": "Mendukung Hubungan Dua Orang (Relationship)." },
  { "en": "Apa Arti Kata OTP (One True Pairing)?", "id": "Pasangan Yang Dianggap Paling Cocok." },
  { "en": "Apa Arti Kata Fandom?", "id": "Komunitas Penggemar Sesuatu (Film, Grup)." },
  { "en": "Apa Arti Kata Bias?", "id": "Anggota Grup Favorit Yang Paling Disuka." },
  { "en": "Apa Arti Kata Wrecked?", "id": "Merasa Terpesona Idola Lain (Selain Bias)." },
  { "en": "Apa Arti Kata Hiatus?", "id": "Masa Vakum Atau Istirahat Sementara." },
  { "en": "Apa Arti Kata Comeback?", "id": "Kembali Lagi Merilis Karya Baru." },
  { "en": "Apa Arti Kata Rookie?", "id": "Artis Pendatang Baru, Masih Awal." },
  { "en": "Apa Arti Kata Senior?", "id": "Artis Yang Sudah Lama Berkarier." },
  { "en": "Apa Arti Kata Hwaiting?", "id": "Kata Semangat (Dari Bahasa Korea)." },
  { "en": "Apa Arti Kata Mianhae?", "id": "Kata Maaf (Dari Bahasa Korea)." },
  { "en": "Apa Arti Kata Gomawo?", "id": "Kata Terima Kasih (Dari Bahasa Korea)." },
  { "en": "Apa Arti Kata Saranghae?", "id": "Aku Cinta Kamu (Dari Bahasa Korea)." },
  { "en": "Apa Arti Kata Aigo?", "id": "Ekspresi Kaget Atau Mengeluh (Korea)." },
  { "en": "Apa Arti Kata Daebak?", "id": "Ekspresi Luar Biasa Atau Hebat (Korea)." },
  { "en": "Apa Arti Kata Oppa?", "id": "Panggilan Wanita Ke Pria Lebih Tua (Korea)." },
  { "en": "Apa Arti Kata Unnie?", "id": "Panggilan Wanita Ke Wanita Lebih Tua (Korea)." },
  { "en": "Apa Arti Kata Hyung?", "id": "Panggilan Pria Ke Pria Lebih Tua (Korea)." },
  { "en": "Apa Arti Kata Noona?", "id": "Panggilan Pria Ke Wanita Lebih Tua (Korea)." },
  { "en": "Apa Arti Kata Maknae?", "id": "Anggota Termuda Dalam Grup (Korea)." },
  { "en": "Apa Arti Kata Agency?", "id": "Perusahaan Yang Menaungi Artis." },
  { "en": "Apa Arti Kata Trainee?", "id": "Orang Yang Berlatih Menjadi Idola." },
  { "en": "Apa Arti Kata Debut?", "id": "Penampilan Resmi Pertama Kali." },
  { "en": "Apa Arti Kata FYP (For Your Page)?", "id": "Halaman Utama Rekomendasi TikTok." },
  { "en": "Apa Arti Kata Check?", "id": "Tren Video Memeriksa Sesuatu (TikTok)." },
  { "en": "Apa Arti Kata IB (Inspired By)?", "id": "Terinspirasi Oleh (Kredit Konten)." },
  { "en": "Apa Arti Kata DC (Dance Credit)?", "id": "Kredit Untuk Pembuat Gerakan Tari." },
  { "en": "Apa Arti Kata Stitch?", "id": "Menggabungkan Video Orang Lain Dengan Video Kita." },
  { "en": "Apa Arti Kata Duet?", "id": "Membuat Video Berdampingan Di TikTok." },
  { "en": "Apa Arti Kata CO (Check Out)?", "id": "Menyelesaikan Proses Pembelian Online." },
  { "en": "Apa Arti Kata Keranjang Kuning?", "id": "Fitur Berjualan Di TikTok Shop." },
  { "en": "Apa Arti Kata Trust Issue?", "id": "Kesulitan Mempercayai Orang Lain." },
  { "en": "Apa Arti Kata Daddy Issue?", "id": "Masalah Psikologis Akibat Figur Ayah." },
  { "en": "Apa Arti Kata Mommy Issue?", "id": "Masalah Psikologis Akibat Figur Ibu." },
  { "en": "Apa Arti Kata Inner Child?", "id": "Sisi Kekanak-Kanakan Dalam Diri." },
  { "en": "Apa Arti Kata Toxic Positivity?", "id": "Sikap Positif Berlebihan Yang Salah." },
  { "en": "Apa Arti Kata Self-Awareness?", "id": "Kesadaran Penuh Atas Diri Sendiri." },
  { "en": "Apa Arti Kata Impulsive?", "id": "Bertindak Tiba-Tiba Tanpa Berpikir." },
  { "en": "Apa Arti Kata Procrastination?", "id": "Kebiasaan Menunda-Nunda Pekerjaan." },
  { "en": "Apa Arti Kata Produktif?", "id": "Menghasilkan Sesuatu, Banyak Hasil." },
  { "en": "Apa Arti Kata Kontraproduktif?", "id": "Tindakan Yang Menghambat Hasil." },
  { "en": "Apa Arti Kata Ambisius?", "id": "Punya Keinginan Kuat Meraih Sukses." },
  { "en": "Apa Arti Kata Realistis?", "id": "Sesuai Dengan Kenyataan, Masuk Akal." },
  { "en": "Apa Arti Kata Optimis?", "id": "Selalu Berpikiran Baik, Penuh Harapan." },
  { "en": "Apa Arti Kata Pesimis?", "id": "Selalu Berpikiran Buruk, Hilang Harapan." },
  { "en": "Apa Arti Kata Passion?", "id": "Minat Besar, Gairah Terhadap Sesuatu." },
  { "en": "Apa Arti Kata Visi?", "id": "Tujuan Jangka Panjang, Pandangan Masa Depan." },
  { "en": "Apa Arti Kata Misi?", "id": "Langkah-Langkah Untuk Mencapai Visi." },
  { "en": "Apa Arti Kata Core Value?", "id": "Nilai-Nilai Inti (Perusahaan Atau Pribadi)." },
  { "en": "Apa Arti Kata Brainstorming?", "id": "Diskusi Mencari Ide Bersama-Sama." },
  { "en": "Apa Arti Kata Feedback?", "id": "Umpan Balik, Masukan, Atau Kritik." },
  { "en": "Apa Arti Kata Deadline?", "id": "Batas Waktu Pengumpulan Tugas." },
  { "en": "Apa Arti Kata Urgent?", "id": "Sangat Mendesak, Harus Segera Dilakukan." },
  { "en": "Apa Arti Kata ASAP (As Soon As Possible)?", "id": "Sesegera Mungkin, Secepatnya." },
  { "en": "Apa Arti Kata FYI (For Your Information)?", "id": "Sekadar Informasi, Untuk Anda Ketahui." },
  { "en": "Apa Arti Kata TBD (To Be Decided)?", "id": "Akan Ditentukan Nanti." },
  { "en": "Apa Arti Kata TBC (To Be Confirmed)?", "id": "Akan Dikonfirmasi Nanti." },
  { "en": "Apa Arti Kata N/A (Not Available)?", "id": "Tidak Tersedia Atau Tidak Berlaku." },
  { "en": "Apa Arti Kata Tip-Ex?", "id": "Cairan Putih Untuk Menghapus Tinta." },
  { "en": "Apa Arti Kata Odol?", "id": "Sebutan Lain Untuk Pasta Gigi." },
  { "en": "Apa Arti Kata Setrika?", "id": "Alat Untuk Melicinkan Pakaian." },
  { "en": "Apa Arti Kata BUC (Butuh Uang Cepat)?", "id": "Menjual Barang Karena Mendesak." },
  { "en": "Apa Arti Kata Curcol?", "id": "Singkatan Curhat Colongan." },
  { "en": "Apa Arti Kata Mupeng?", "id": "Singkatan Muka Pengen, Sangat Ingin." },
  { "en": "Apa Arti Kata Sekut?", "id": "Bagus, Keren, Santai (Bahasa Gaul)." },
  { "en": "Apa Arti Kata Goks?", "id": "Pelesetan Dari 'Gokil', Sangat Keren." },
  { "en": "Apa Arti Kata Unyu?", "id": "Imut, Lucu, Menggemaskan." },
  { "en": "Apa Arti Kata Mellow?", "id": "Perasaan Sedih, Sendu, Melankolis." },
  { "en": "Apa Arti Kata Bonyok?", "id": "Sebutan Lain Untuk Ayah Dan Ibu." },
  { "en": "Apa Arti Kata Bokap?", "id": "Sebutan Gaul Untuk Ayah." },
  { "en": "Apa Arti Kata Nyokap?", "id": "Sebutan Gaul Untuk Ibu." },
  { "en": "Apa Arti Kata Bro?", "id": "Panggilan Akrab Untuk Laki-Laki." },
  { "en": "Apa Arti Kata Sis?", "id": "Panggilan Akrab Untuk Perempuan." },
  { "en": "Apa Arti Kata Gan?", "id": "Panggilan Akrab 'Juragan' Di Internet." },
  { "en": "Apa Arti Kata Aganwati?", "id": "Panggilan 'Juragan' Wanita Di Kaskus." },
  { "en": "Apa Arti Kata Kaskus?", "id": "Forum Online Terbesar Di Indonesia." },
  { "en": "Apa Arti Kata Pertamax?", "id": "Komentar Pertama Di Sebuah Thread." },
  { "en": "Apa Arti Kata Cendol?", "id": "Reputasi Baik Di Forum Kaskus." },
  { "en": "Apa Arti Kata Bata?", "id": "Reputasi Buruk Di Forum Kaskus." },
  { "en": "Apa Arti Kata Sundul?", "id": "Menaikkan Thread Agar Muncul Lagi." },
  { "en": "Apa Arti Kata Silent Reader?", "id": "Pembaca Pasif, Tidak Berkomentar." },
  { "en": "Apa Arti Kata SR (Silent Reader)?", "id": "Hanya Membaca, Tidak Ikut Diskusi." },
  { "en": "Apa Arti Kata Kopdar (Kopi Darat)?", "id": "Pertemuan Tatap Muka Komunitas Online." },
  { "en": "Apa Arti Kata Admin?", "id": "Administrator, Pengelola Grup Atau Halaman." },
  { "en": "Apa Arti Kata Mimin?", "id": "Sebutan Akrab Untuk Admin." },
  { "en": "Apa Arti Kata Momod?", "id": "Sebutan Akrab Untuk Moderator Forum." },
  { "en": "Apa Arti Kata Moderator?", "id": "Pengawas Jalannya Diskusi Di Forum." },
  { "en": "Apa Arti Kata Banned?", "id": "Diblokir, Dilarang Mengakses Sesuatu." },
  { "en": "Apa Arti Kata Akun Bodong?", "id": "Akun Palsu Atau Tidak Asli." },
  { "en": "Apa Arti Kata Akun Clone?", "id": "Akun Kedua, Duplikat Akun Utama." },
  { "en": "Apa Arti Kata Alter Ego?", "id": "Kepribadian Kedua Seseorang (Sering Di Medsos)." },
  { "en": "Apa Arti Kata Hate Speech?", "id": "Ujaran Kebencian Kepada Pihak Lain." },
  { "en": "Apa Arti Kata Cyberbullying?", "id": "Perundungan Yang Terjadi Di Dunia Maya." },
  { "en": "Apa Arti Kata Doxing?", "id": "Menyebarkan Data Pribadi Orang Lain." },
  { "en": "Apa Arti Kata Phishing?", "id": "Penipuan Mencuri Data (Password, Pin)." },
  { "en": "Apa Arti Kata Scam?", "id": "Penipuan Online Untuk Mendapat Uang." },
  { "en": "Apa Arti Kata Trustworthy?", "id": "Seseorang Yang Dapat Dipercaya Penuh." },
  { "en": "Apa Arti Kata Trusted?", "id": "Terpercaya (Biasanya Untuk Penjual)." },
  { "en": "Apa Arti Kata Testi (Testimonial)?", "id": "Ulasan Jujur Dari Pembeli." },
  { "en": "Apa Arti Kata Ori (Original)?", "id": "Barang Asli, Bukan Tiruan." },
  { "en": "Apa Arti Kata KW (Kualitas)?", "id": "Barang Tiruan, Palsu, Replika." },
  { "en": "Apa Arti Kata Reject?", "id": "Barang Cacat Produksi Pabrik." },
  { "en": "Apa Arti Kata BNIB (Brand New In Box)?", "id": "Barang Baru Masih Segel Dalam Kotak." },
  { "en": "Apa Arti Kata BNOB (Brand New Open Box)?", "id": "Barang Baru, Kotak Sudah Dibuka." },
  { "en": "Apa Arti Kata Mint Condition?", "id": "Kondisi Barang Bekas Seperti Baru." },
  { "en": "Apa Arti Kata NPU (Nett Price Updated)?", "id": "Harga Nett, Tidak Bisa Ditawar." },
  { "en": "Apa Arti Kata Nego Tipis?", "id": "Tawaran Boleh, Tapi Sedikit Saja." },
  { "en": "Apa Arti Kata Afgan?", "id": "Menawar Harga Sangat Sadis (Lagu 'Sadis')." },
  { "en": "Apa Arti Kata DP (Down Payment)?", "id": "Uang Muka, Tanda Jadi Pembelian." },
  { "en": "Apa Arti Kata PO (Pre-Order)?", "id": "Pesan Dan Bayar Dulu, Barang Nanti." },
  { "en": "Apa Arti Kata Ready Stock?", "id": "Barang Tersedia, Siap Dikirim Langsung." },
  { "en": "Apa Arti Kata Sold Out?", "id": "Barang Sudah Habis Terjual Semua." },
  { "en": "Apa Arti Kata Restock?", "id": "Mengisi Ulang Stok Barang Yang Habis." },
  { "en": "Apa Arti Kata Limited Edition?", "id": "Edisi Terbatas, Diproduksi Sedikit." },
  { "en": "Apa Arti Kata Rare Item?", "id": "Barang Langka Yang Sulit Dicari." },
  { "en": "Apa Arti Kata Wishlist?", "id": "Daftar Barang Yang Sangat Diinginkan." },
  { "en": "Apa Arti Kata Impulsive Buying?", "id": "Membeli Barang Tiba-Tiba Tanpa Rencana." },
  { "en": "Apa Arti Kata FOMO (Fear Of Missing Out) Marketing?", "id": "Takut Ketinggalan Promo Atau Tren." },
  { "en": "Apa Arti Kata Cashback?", "id": "Uang Kembali Setelah Melakukan Pembelian." },
  { "en": "Apa Arti Kata Flash Sale?", "id": "Diskon Besar Dalam Waktu Sangat Singkat." },
  { "en": "Apa Arti Kata Giveaway?", "id": "Bagi-Bagi Hadiah Gratis." },
  { "en": "Apa Arti Kata Endorsement?", "id": "Promosi Produk Oleh Tokoh Terkenal." },
  { "en": "Apa Arti Kata Paid Promote?", "id": "Bayar Akun Lain Untuk Promosi." },
  { "en": "Apa Arti Kata Rate Card?", "id": "Daftar Harga Jasa (Influencer)." },
  { "en": "Apa Arti Kata Brief?", "id": "Arahan Singkat Mengenai Detail Pekerjaan." },
  { "en": "Apa Arti Kata Revisi?", "id": "Perbaikan Atau Pengulangan Hasil Kerja." },
  { "en": "Apa Arti Kata ACC?", "id": "Disetujui (Diterima) Oleh Pihak Lain." },
  { "en": "Apa Arti Kata Pending?", "id": "Status Tertunda, Menunggu Keputusan." },
  { "en": "Apa Arti Kata Cancel?", "id": "Dibatalkan, Tidak Jadi Dilakukan." },
  { "en": "Apa Arti Kata Reschedule?", "id": "Menjadwalkan Ulang Waktu Acara." },
  { "en": "Apa Arti Kata Reminder?", "id": "Pesan Pengingat Akan Sesuatu." },
  { "en": "Apa Arti Kata To Do List?", "id": "Daftar Pekerjaan Yang Harus Dilakukan." },
  { "en": "Apa Arti Kata Multitasking?", "id": "Mengerjakan Banyak Tugas Sekaligus." },
  { "en": "Apa Arti Kata KBL (Kaget Banget Loh)?", "id": "Ungkapan Menyatakan Rasa Sangat Kaget." },
  { "en": "Apa Arti Kata Sefruit?", "id": "Pelesetan 'Sepotong Buah', Artinya 'Sebuah'." },
  { "en": "Apa Arti Kata Bestie?", "id": "Sebutan Untuk Sahabat Sangat Dekat." },
  { "en": "Apa Arti Kata Brokie?", "id": "Sebutan Untuk Orang Yang Sedang Bangkrut." },
  { "en": "Apa Arti Kata Sus (Suspicious)?", "id": "Sesuatu Yang Terlihat Mencurigakan." },
  { "en": "Apa Arti Kata Bet?", "id": "Pelesetan 'Banget', Menegaskan Sesuatu." },
  { "en": "Apa Arti Kata PM (Personal Message)?", "id": "Mengirim Pesan Secara Pribadi." },
  { "en": "Apa Arti Kata DM (Direct Message)?", "id": "Sama Seperti PM, Pesan Pribadi." },
  { "en": "Apa Arti Kata BM (Banyak Mau)?", "id": "Sedang Sangat Menginginkan Sesuatu." },
  { "en": "Apa Arti Kata Old Money?", "id": "Kekayaan Yang Diwariskan Turun-Temurun." },
  { "en": "Apa Arti Kata New Money?", "id": "Kekayaan Yang Didapat Dari Usaha Sendiri." },
  { "en": "Apa Arti Kata Sugar Daddy?", "id": "Pria Tua Kaya Yang Membiayai Pasangan Muda." },
  { "en": "Apa Arti Kata Sugar Baby?", "id": "Pasangan Muda Yang Dibiayai Sugar Daddy." },
  { "en": "Apa Arti Kata Gold Digger?", "id": "Orang Yang Mengejar Harta Pasangannya." },
  { "en": "Apa Arti Kata Social Climber?", "id": "Orang Yang Suka Panjat Sosial." },
  { "en": "Apa Arti Kata Drama Queen?", "id": "Orang Yang Melebih-Lebihkan Masalah." },
  { "en": "Apa Arti Kata Attention Seeker?", "id": "Orang Yang Selalu Mencari Perhatian." },
  { "en": "Apa Arti Kata Humble Bragging?", "id": "Pamer Secara Terselubung, Pura-Pura Merendah." },
  { "en": "Apa Arti Kata Benchmark?", "id": "Standar Tolok Ukur Pembanding Kinerja." },
  { "en": "Apa Arti Kata Insight?", "id": "Wawasan Baru Yang Mendalam." },
  { "en": "Apa Arti Kata Stakeholder?", "id": "Pihak Yang Berkepentingan Dalam Proyek." },
  { "en": "Apa Arti Kata Pitching?", "id": "Presentasi Ide Singkat Untuk Meyakinkan." },
  { "en": "Apa Arti Kata Deck?", "id": "Materi Presentasi, Biasanya Format Slide." },
  { "en": "Apa Arti Kata Brainstorming?", "id": "Diskusi Kelompok Untuk Mendapatkan Ide." },
  { "en": "Apa Arti Kata Case Study?", "id": "Studi Kasus, Analisis Masalah Spesifik." },
  { "en": "Apa Arti Kata Data Driven?", "id": "Mengambil Keputusan Berdasarkan Data." },
  { "en": "Apa Arti Kata User Experience (UX)?", "id": "Pengalaman Pengguna Saat Memakai Produk." },
  { "en": "Apa Arti Kata User Interface (UI)?", "id": "Tampilan Visual Produk Digital." },
  { "en": "Apa Arti Kata Glitch?", "id": "Kesalahan Teknis Kecil Pada Sistem." },
  { "en": "Apa Arti Kata Bug?", "id": "Kesalahan Serius Pada Kode Program." },
  { "en": "Apa Arti Kata Debugging?", "id": "Proses Mencari Dan Memperbaiki Bug." },
  { "en": "Apa Arti Kata Bandwidth?", "id": "Kapasitas Maksimal Transfer Data Internet." },
  { "en": "Apa Arti Kata Server Down?", "id": "Server Pusat Sedang Mati, Tidak Bisa Diakses." },
  { "en": "Apa Arti Kata Maintenance?", "id": "Perawatan Sistem, Seringkali Offline." },
  { "en": "Apa Arti Kata Update?", "id": "Memperbarui Perangkat Lunak Ke Versi Baru." },
  { "en": "Apa Arti Kata Upgrade?", "id": "Meningkatkan Kualitas (Hardware Atau Layanan)." },
  { "en": "Apa Arti Kata Downgrade?", "id": "Menurunkan Kualitas (Hardware Atau Layanan)." },
  { "en": "Apa Arti Kata Backup?", "id": "Mencadangkan Data Agar Tidak Hilang." },
  { "en": "Apa Arti Kata Restore?", "id": "Mengembalikan Data Cadangan Seperti Semula." },
  { "en": "Apa Arti Kata Default?", "id": "Pengaturan Awal Atau Bawaan Pabrik." },
  { "en": "Apa Arti Kata Template?", "id": "Pola Atau Cetakan Dasar Desain." },
  { "en": "Apa Arti Kata Manual?", "id": "Dilakukan Dengan Tangan, Tidak Otomatis." },
  { "en": "Apa Arti Kata Otomatis?", "id": "Bekerja Sendiri Tanpa Bantuan Manusia." },
  { "en": "Apa Arti Kata Premis?", "id": "Asumsi Dasar Atau Awal Sebuah Argumen." },
  { "en": "Apa Arti Kata Hipotesis?", "id": "Dugaan Awal Yang Perlu Diuji Kebenarannya." },
  { "en": "Apa Arti Kata Paradoks?", "id": "Pernyataan Yang Terlihat Bertentangan Logika." },
  { "en": "Apa Arti Kata Anomali?", "id": "Keanehan, Penyimpangan Dari Keadaan Normal." },
  { "en": "Apa Arti Kata Dilema?", "id": "Situasi Sulit, Memilih Di Antara Dua Pilihan Buruk." },
  { "en": "Apa Arti Kata Stagnan?", "id": "Berhenti Di Tempat, Tidak Ada Kemajuan." },
  { "en": "Apa Arti Kata Dinamis?", "id": "Selalu Bergerak, Penuh Perubahan." },
  { "en": "Apa Arti Kata Statis?", "id": "Diam, Tetap, Tidak Berubah Keadaannya." },
  { "en": "Apa Arti Kata Pro Rata?", "id": "Dihitung Secara Proporsional Sesuai Porsi." },
  { "en": "Apa Arti Kata Lump Sum?", "id": "Pembayaran Penuh Dalam Satu Kali." },
  { "en": "Apa Arti Kata Reimburse?", "id": "Penggantian Uang Yang Sudah Ditalangi." },
  { "en": "Apa Arti Kata Invoice?", "id": "Tagihan Resmi Rincian Pembayaran." },
  { "en": "Apa Arti Kata Quotation?", "id": "Penawaran Harga Resmi Sebelum Transaksi." },
  { "en": "Apa Arti Kata PO (Purchase Order)?", "id": "Dokumen Resmi Pemesanan Barang." },
  { "en": "Apa Arti Kata DO (Delivery Order)?", "id": "Dokumen Resmi Pengiriman Barang." },
  { "en": "Apa Arti Kata Resi?", "id": "Bukti Pengiriman Barang Dari Ekspedisi." },
  { "en": "Apa Arti Kata Ekspedisi?", "id": "Jasa Pengiriman Barang Atau Paket." },
  { "en": "Apa Arti Kata Kurir?", "id": "Orang Yang Mengantarkan Paket." },
  { "en": "Apa Arti Kata Internal?", "id": "Sesuatu Yang Berasal Dari Dalam." },
  { "en": "Apa Arti Kata Eksternal?", "id": "Sesuatu Yang Berasal Dari Luar." },
  { "en": "Apa Arti Kata Mayoritas?", "id": "Bagian Terbesar Atau Kebanyakan." },
  { "en": "Apa Arti Kata Minoritas?", "id": "Bagian Terkecil Atau Sebagian Kecil." },
  { "en": "Apa Arti Kata Prioritas?", "id": "Sesuatu Yang Diutamakan, Paling Penting." },
  { "en": "Apa Arti Kata Sekunder?", "id": "Urutan Kedua, Tidak Terlalu Penting." },
  { "en": "Apa Arti Kata Tersier?", "id": "Urutan Ketiga, Kebutuhan Mewah." },
  { "en": "Apa Arti Kata Wir?", "id": "Panggilan 'Jawir', Merujuk Orang Jawa." },
  { "en": "Apa Arti Kata Slay?", "id": "Melakukan Sesuatu Dengan Sangat Keren." },
  { "en": "Apa Arti Kata Pookie?", "id": "Panggilan Sayang Yang Sangat Manja." },
  { "en": "Apa Arti Kata Gatekeeping?", "id": "Mempersulit Akses Informasi Suatu Hobi." },
  { "en": "Apa Arti Kata Glazing?", "id": "Memuji Seseorang Secara Berlebihan." },
  { "en": "Apa Arti Kata Yap?", "id": "Bentuk Lain Dari Kata 'Ya'." },
  { "en": "Apa Arti Kata Nope?", "id": "Bentuk Lain Dari Kata 'Tidak'." },
  { "en": "Apa Arti Kata Yass?", "id": "Ekspresi 'Ya' Yang Sangat Bersemangat." },
  { "en": "Apa Arti Kata Literally (Gaul)?", "id": "Penekanan Kata 'Banget', 'Sangat'." },
  { "en": "Apa Arti Kata Basically (Gaul)?", "id": "Pada Dasarnya, Intinya Sih." },
  { "en": "Apa Arti Kata Act Of Service?", "id": "Bahasa Cinta Berupa Tindakan Melayani." },
  { "en": "Apa Arti Kata Physical Touch?", "id": "Bahasa Cinta Berupa Sentuhan Fisik." },
  { "en": "Apa Arti Kata Words Of Affirmation?", "id": "Bahasa Cinta Berupa Kata-Kata Pujian." },
  { "en": "Apa Arti Kata Receiving Gifts?", "id": "Bahasa Cinta Berupa Menerima Hadiah." },
  { "en": "Apa Arti Kata Quality Time?", "id": "Bahasa Cinta Berupa Waktu Berkualitas." },
  { "en": "Apa Arti Kata HTS (Hubungan Tanpa Status)?", "id": "Dekat Seperti Pacar, Tanpa Komitmen." },
  { "en": "Apa Arti Kata Backburner?", "id": "Menjadikan Seseorang Pilihan Cadangan." },
  { "en": "Apa Arti Kata SMB (Social Media Break)?", "id": "Istirahat Total Dari Media Sosial." },
  { "en": "Apa Arti Kata Detox Digital?", "id": "Mengurangi Penggunaan Gawai." },
  { "en": "Apa Arti Kata Mental Health?", "id": "Kesehatan Jiwa Atau Mental Seseorang." },
  { "en": "Apa Arti Kata Self-Care?", "id": "Aktivitas Merawat Diri Sendiri." },
  { "en": "Apa Arti Kata Self-Esteem?", "id": "Tingkat Harga Diri Seseorang." },
  { "en": "Apa Arti Kata Self-Confident?", "id": "Kepercayaan Terhadap Kemampuan Diri." },
  { "en": "Apa Arti Kata Self-Doubt?", "id": "Keraguan Terhadap Kemampuan Diri." },
  { "en": "Apa Arti Kata Imposter Syndrome?", "id": "Merasa Sukses Karena Beruntung, Tak Pantas." },
  { "en": "Apa Arti Kata Grinding (Game)?", "id": "Mengulang Aksi Untuk Menaikkan Level." },
  { "en": "Apa Arti Kata Farming (Game)?", "id": "Mengulang Aksi Untuk Mengumpulkan Item." },
  { "en": "Apa Arti Kata Cooldown (Game)?", "id": "Waktu Jeda Sebelum Skill Bisa Dipakai." },
  { "en": "Apa Arti Kata AoE (Area Of Effect)?", "id": "Serangan Yang Mengenai Satu Area." },
  { "en": "Apa Arti Kata Buff (Game)?", "id": "Peningkatan Status Atau Kekuatan." },
  { "en": "Apa Arti Kata Nerf (Game)?", "id": "Penurunan Status Atau Kekuatan." },
  { "en": "Apa Arti Kata Lag?", "id": "Keterlambatan Respon Karena Jaringan Lambat." },
  { "en": "Apa Arti Kata Frame Drop?", "id": "Gambar Patah-Patah Karena Grafis Berat." },
  { "en": "Apa Arti Kata Cheater?", "id": "Pemain Yang Menggunakan Program Curang." },
  { "en": "Apa Arti Kata Hode?", "id": "Pria Yang Berpura-Pura Menjadi Wanita." },
  { "en": "Apa Arti Kata Ngabuburit?", "id": "Menunggu Waktu Berbuka Puasa." },
  { "en": "Apa Arti Kata Bukber?", "id": "Singkatan Buka Puasa Bersama." },
  { "en": "Apa Arti Kata Takjil?", "id": "Makanan Ringan Untuk Berbuka Puasa." },
  { "en": "Apa Arti Kata Mudik?", "id": "Pulang Ke Kampung Halaman Saat Lebaran." },
  { "en": "Apa Arti Kata Halal Bihalal?", "id": "Acara Silaturahmi Saling Memaafkan." },
  { "en": "Apa Arti Singkatan THR (Tunjangan Hari Raya)?", "id": "Bonus Uang Yang Diterima Saat Lebaran." },
  { "en": "Apa Arti Kata Arus Balik?", "id": "Perjalanan Kembali Ke Kota Perantauan." },
  { "en": "Apa Arti Kata Parsel?", "id": "Bingkisan Hadiah Yang Dikirimkan." },
  { "en": "Apa Arti Kata Kating?", "id": "Singkatan Kakak Tingkat Di Kampus." },
  { "en": "Apa Arti Kata Dedek Gemes?", "id": "Panggilan Untuk Junior Yang Menarik." },
  { "en": "Apa Arti Kata Ospek?", "id": "Orientasi Pengenalan Kampus Mahasiswa Baru." },
  { "en": "Apa Arti Singkatan UKM (Unit Kegiatan Mahasiswa)?", "id": "Ekstrakurikuler Versi Mahasiswa Di Kampus." },
  { "en": "Apa Arti Singkatan BEM (Badan Eksekutif Mahasiswa)?", "id": "Organisasi Eksekutif Tertinggi Mahasiswa." },
  { "en": "Apa Arti Singkatan DPM (Dewan Perwakilan Mahasiswa)?", "id": "Organisasi Legislatif Pengawas BEM." },
  { "en": "Apa Arti Singkatan HIMA (Himpunan Mahasiswa)?", "id": "Organisasi Mahasiswa Tingkat Jurusan." },
  { "en": "Apa Arti Singkatan KRS (Kartu Rencana Studi)?", "id": "Lembar Pengambilan Mata Kuliah Per Semester." },
  { "en": "Apa Arti Singkatan KHS (Kartu Hasil Studi)?", "id": "Lembar Laporan Nilai Mata Kuliah." },
  { "en": "Apa Arti Singkatan IPK (Indeks Prestasi Kumulatif)?", "id": "Nilai Rata-Rata Gabungan Selama Kuliah." },
  { "en": "Apa Arti Singkatan SKS (Satuan Kredit Semester)?", "id": "Satuan Bobot Mata Kuliah." },
  { "en": "Apa Arti Kata Cumlaude?", "id": "Gelar Kehormatan Lulus Dengan Nilai Tinggi." },
  { "en": "Apa Arti Kata Skripsi?", "id": "Tugas Akhir Ilmiah Mahasiswa S1." },
  { "en": "Apa Arti Kata Tesis?", "id": "Tugas Akhir Ilmiah Mahasiswa S2." },
  { "en": "Apa Arti Kata Disertasi?", "id": "Tugas Akhir Ilmiah Mahasiswa S3." },
  { "en": "Apa Arti Kata Dosen?", "id": "Pengajar Atau Guru Di Perguruan Tinggi." },
  { "en": "Apa Arti Kata Rektor?", "id": "Pemimpin Tertinggi Perguruan Tinggi." },
  { "en": "Apa Arti Kata Dekan?", "id": "Pemimpin Tertinggi Fakultas Di Universitas." },
  { "en": "Apa Arti Kata Matkul?", "id": "Singkatan Dari Mata Kuliah." },
  { "en": "Apa Arti Kata Maba?", "id": "Singkatan Dari Mahasiswa Baru." },
  { "en": "Apa Arti Kata KKN (Kuliah Kerja Nyata)?", "id": "Pengabdian Mahasiswa Di Masyarakat Desa." },
  { "en": "Apa Arti Kata Magang?", "id": "Praktik Kerja Di Perusahaan (Internship)." },
  { "en": "Apa Arti Kata Wisuda?", "id": "Upacara Pelantikan Kelulusan Mahasiswa." },
  { "en": "Apa Arti Kata Alumni?", "id": "Orang Yang Telah Lulus Dari Sekolah." },
  { "en": "Apa Arti Kata Reuni?", "id": "Pertemuan Kembali Para Alumni Sekolah." },
  { "en": "Apa Arti Kata BPJS (Badan Penyelenggara Jaminan Sosial)?", "id": "Lembaga Asuransi Kesehatan Negara." },
  { "en": "Apa Arti Singkatan SIM (Surat Izin Mengemudi)?", "id": "Bukti Izin Resmi Mengendarai Kendaraan." },
  { "en": "Apa Arti Singkatan STNK (Surat Tanda Nomor Kendaraan)?", "id": "Bukti Kepemilikan Sah Kendaraan Bermotor." },
  { "en": "Apa Arti Singkatan BPKB (Buku Pemilik Kendaraan Bermotor)?", "id": "Buku Bukti Sah Kepemilikan Kendaraan." },
  { "en": "Apa Arti Singkatan KTP (Kartu Tanda Penduduk)?", "id": "Kartu Identitas Resmi Warga Negara." },
  { "en": "Apa Arti Singkatan KK (Kartu Keluarga)?", "id": "Kartu Identitas Data Anggota Keluarga." },
  { "en": "Apa Arti Singkatan NPWP (Nomor Pokok Wajib Pajak)?", "id": "Nomor Identitas Untuk Keperluan Pajak." },
  { "en": "Apa Arti Kata Tilang?", "id": "Bukti Pelanggaran Lalu Lintas." },
  { "en": "Apa Arti Kata Razia?", "id": "Pemeriksaan Mendadak Oleh Pihak Berwajib." },
  { "en": "Apa Arti Kata Macet?", "id": "Kondisi Lalu Lintas Yang Terhenti." },
  { "en": "Apa Arti Kata Banjir?", "id": "Genangan Air Yang Meluap." },
  { "en": "Apa Arti Kata Gempa?", "id": "Getaran Pada Permukaan Bumi." },
  { "en": "Apa Arti Kata Tsunami?", "id": "Gelombang Laut Besar Akibat Gempa." },
  { "en": "Apa Arti Kata Hoax?", "id": "Berita Bohong, Informasi Palsu." },
  { "en": "Apa Arti Kata Viral?", "id": "Sesuatu Yang Menyebar Cepat Di Internet." },
  { "en": "Apa Arti Kata Trending?", "id": "Topik Yang Sedang Hangat Dibicarakan." },
  { "en": "Apa Arti Kata Konten?", "id": "Isi Atau Materi (Video, Tulisan, Gambar)." },
  { "en": "Apa Arti Kata Kreator?", "id": "Orang Yang Membuat Konten." },
  { "en": "Apa Arti Kata Subscriber?", "id": "Pelanggan Saluran (Contoh: Youtube)." },
  { "en": "Apa Arti Kata Follower?", "id": "Pengikut Akun Media Sosial." },
  { "en": "Apa Arti Kata Username?", "id": "Nama Akun Pengguna Di Platform." },
  { "en": "Apa Arti Kata Password?", "id": "Kata Sandi Rahasia Untuk Masuk Akun." },
  { "en": "Apa Arti Kata Login?", "id": "Proses Masuk Ke Dalam Akun." },
  { "en": "Apa Arti Kata Logout?", "id": "Proses Keluar Dari Akun." },
  { "en": "Apa Arti Kata Signup?", "id": "Proses Mendaftar Akun Baru." },
  { "en": "Apa Arti Kata Error?", "id": "Terjadi Kesalahan Pada Sistem." },
  { "en": "Apa Arti Kata Loading?", "id": "Proses Memuat Data, Menunggu." },
  { "en": "Apa Arti Kata Buffering?", "id": "Proses Memuat Video (Streaming)." },
  { "en": "Apa Arti Kata Streaming?", "id": "Menonton Konten Langsung Via Internet." },
  { "en": "Apa Arti Kata Browsing?", "id": "Aktivitas Menjelajahi Situs Web." },
  { "en": "Apa Arti Kata Browser?", "id": "Aplikasi Untuk Membuka Situs Web." },
  { "en": "Apa Arti Kata Link?", "id": "Tautan, Alamat Situs Web." },
  { "en": "Apa Arti Kata URL (Uniform Resource Locator)?", "id": "Alamat Lengkap Sebuah Halaman Web." },
  { "en": "Apa Arti Kata Download?", "id": "Mengunduh Data Dari Internet Ke Perangkat." },
  { "en": "Apa Arti Kata Upload?", "id": "Mengunggah Data Dari Perangkat Ke Internet." },
  { "en": "Apa Arti Kata Online?", "id": "Terhubung Dengan Jaringan Internet." },
  { "en": "Apa Arti Kata Offline?", "id": "Tidak Terhubung Dengan Jaringan Internet." },
  { "en": "Apa Arti Kata Wi-Fi?", "id": "Jaringan Nirkabel Untuk Koneksi Internet." },
  { "en": "Apa Arti Kata Kuota?", "id": "Batas Maksimal Penggunaan Data Internet." },
  { "en": "Apa Arti Kata Provider?", "id": "Perusahaan Penyedia Layanan Internet." },
  { "en": "Apa Arti Kata Sim Card?", "id": "Kartu Identitas Pelanggan Seluler." },
  { "en": "Apa Arti Kata Pulsa?", "id": "Saldo Untuk Layanan Telepon Seluler." },
  { "en": "Apa Arti Kata Smartphone?", "id": "Telepon Pintar Dengan Fitur Canggih." },
  { "en": "Apa Arti Kata Gadget?", "id": "Perangkat Elektronik Kecil, Gawai." },
  { "en": "Apa Arti Kata Laptop?", "id": "Komputer Jinjing Yang Portabel." },
  { "en": "Apa Arti Kata PC (Personal Computer)?", "id": "Komputer Pribadi Untuk Di Meja." },
  { "en": "Apa Arti Kata Keyboard?", "id": "Papan Ketik Untuk Memasukkan Teks." },
  { "en": "Apa Arti Kata Mouse?", "id": "Perangkat Penunjuk Kursor Di Layar." },
  { "en": "Apa Arti Kata Monitor?", "id": "Layar Penampil Gambar Komputer." },
  { "en": "Apa Arti Kata Speaker?", "id": "Perangkat Pengeras Suara." },
  { "en": "Apa Arti Kata Headphone?", "id": "Alat Dengar Audio Di Kepala." },
  { "en": "Apa Arti Kata Earphone?", "id": "Alat Dengar Audio Di Telinga." },
  { "en": "Apa Arti Kata Charger?", "id": "Alat Untuk Mengisi Ulang Baterai." },
  { "en": "Apa Arti Kata Power Bank?", "id": "Penyimpan Daya Baterai Portabel." },
  { "en": "Apa Arti Kata Kabel Data?", "id": "Kabel Untuk Transfer Data Atau Charger." },
  { "en": "Apa Arti Kata Overheat?", "id": "Kondisi Perangkat Terlalu Panas." },
  { "en": "Apa Arti Kata Hang?", "id": "Kondisi Perangkat Macet, Tidak Merespon." },
  { "en": "Apa Arti Kata Restart?", "id": "Mematikan Lalu Menghidupkan Ulang Perangkat." },
  { "en": "Apa Arti Kata Install?", "id": "Memasang Aplikasi Atau Program." },
  { "en": "Apa Arti Kata Uninstall?", "id": "Mencopot Aplikasi Atau Program." },
  { "en": "Apa Arti Kata Akun?", "id": "Identitas Pengguna Di Sebuah Layanan." },
  { "en": "Apa Arti Kata Notifikasi?", "id": "Pemberitahuan Atau Peringatan Baru." },
  { "en": "Apa Arti Kata Spam?", "id": "Pesan Sampah Yang Tidak Diinginkan." },
  { "en": "Apa Arti Kata Phishing?", "id": "Upaya Penipuan Untuk Mencuri Data." },
  { "en": "Apa Arti Kata Hacker?", "id": "Orang Yang Meretas Sistem Komputer." },
  { "en": "Apa Arti Kata Cracker?", "id": "Peretas Dengan Niat Buruk." },
  { "en": "Apa Arti Kata Virus?", "id": "Program Jahat Perusak Sistem." },
  { "en": "Apa Arti Kata Antivirus?", "id": "Program Pelindung Dari Serangan Virus." },
  { "en": "Apa Arti Kata Firewall?", "id": "Sistem Keamanan Jaringan Komputer." },
  { "en": "Apa Arti Kata Backup?", "id": "Mencadangkan Data Penting." },
  { "en": "Apa Arti Kata Overrated?", "id": "Sesuatu Yang Dinilai Terlalu Tinggi." },
  { "en": "Apa Arti Kata Underrated?", "id": "Sesuatu Yang Dinilai Terlalu Rendah." },
  { "en": "Apa Arti Kata Rizz?", "id": "Daya Tarik Karismatik Memikat Lawan Jenis." },
  { "en": "Apa Arti Kata Simp?", "id": "Orang Yang Terlalu Tunduk Pada Pasangannya." },
  { "en": "Apa Arti Kata Sus?", "id": "Singkatan 'Suspicious', Artinya Mencurigakan." },
  { "en": "Apa Arti Kata Glazing (Gaul)?", "id": "Memuji Seseorang Secara Berlebihan." },
  { "en": "Apa Arti Kata Yap?", "id": "Kata Lain Untuk 'Ya', Terdengar Santai." },
  { "en": "Apa Arti Kata Nope?", "id": "Kata Lain Untuk 'Tidak', Terdengar Santai." },
  { "en": "Apa Arti Kata Gatekeeping?", "id": "Mempersulit Orang Lain Mengakses Hobi." },
  { "en": "Apa Arti Kata Sefruit?", "id": "Pelesetan Kata 'Sebuah', 'Sepotong'." },
  { "en": "Apa Arti Kata Sebat?", "id": "Istilah Gaul Untuk Merokok Sebentar." },
  { "en": "Apa Arti Kata Sekut?", "id": "Kata Gaul Yang Berarti Keren, Asyik." },
  { "en": "Apa Arti Kata Sans?", "id": "Kependekan Dari 'Santai', Tidak Tegang." },
  { "en": "Apa Arti Kata Garing?", "id": "Lelucon Yang Tidak Lucu, Hambar." },
  { "en": "Apa Arti Kata Jayus?", "id": "Sama Seperti Garing, Lelucon Tidak Lucu." },
  { "en": "Apa Arti Kata Receh?", "id": "Lelucon Sederhana Tapi Sangat Lucu." },
  { "en": "Apa Arti Kata Ngakak?", "id": "Tertawa Sangat Keras Terbahak-Bahak." },
  { "en": "Apa Arti Kata Ngabrut?", "id": "Singkatan 'Ngakak Brutal', Sangat Lucu." },
  { "en": "Apa Arti Kata Ngokey?", "id": "Bentuk Lain Dari Kata 'Oke'." },
  { "en": "Apa Arti Kata OTW (On The Way)?", "id": "Sedang Dalam Perjalanan Menuju Lokasi." },
  { "en": "Apa Arti Kata Anw (Anyway)?", "id": "Sama Seperti 'Ngomong-Ngomong'." },
  { "en": "Apa Arti Kata GBU (God Bless You)?", "id": "Semoga Tuhan Memberkati Anda." },
  { "en": "Apa Arti Kata JBU (Jesus Bless You)?", "id": "Semoga Yesus Memberkati Anda (Kristen)." },
  { "en": "Apa Arti Kata Aamiin?", "id": "Semoga Allah Mengabulkan Doa (Islam)." },
  { "en": "Apa Arti Kata Insya Allah?", "id": "Jika Allah Mengizinkan (Islam)." },
  { "en": "Apa Arti Kata Masya Allah?", "id": "Ekspresi Kagum Atas Ciptaan Allah." },
  { "en": "Apa Arti Kata Astagfirullah?", "id": "Memohon Ampun Kepada Allah." },
  { "en": "Apa Arti Kata Alhamdulillah?", "id": "Ucapan Syukur Kepada Allah." },
  { "en": "Apa Arti Kata Damai Sejahtera?", "id": "Salam Khas Umat Kristiani." },
  { "en": "Apa Arti Kata Om Swastiastu?", "id": "Salam Pembuka Umat Hindu." },
  { "en": "Apa Arti Kata Namo Buddhaya?", "id": "Salam Pembuka Umat Buddha." },
  { "en": "Apa Arti Kata KBL (Kaget Banget Loh)?", "id": "Ekspresi Saat Merasa Sangat Kaget." },
  { "en": "Apa Arti Kata NBL (Ngakak Banget Loh)?", "id": "Ekspresi Saat Tertawa Sangat Keras." },
  { "en": "Apa Arti Kata CBL (Capek Banget Loh)?", "id": "Ekspresi Saat Merasa Sangat Lelah." },
  { "en": "Apa Arti Kata SBL (Sebel Banget Loh)?", "id": "Ekspresi Saat Merasa Sangat Kesal." },
  { "en": "Apa Arti Kata Relate?", "id": "Merasa Terhubung Dengan Cerita Orang Lain." },
  { "en": "Apa Arti Kata Relatable?", "id": "Sesuatu Yang Mudah Terhubung Dengan Kita." },
  { "en": "Apa Arti Kata Valid?", "id": "Benar, Sah, Tidak Bisa Dibantah." },
  { "en": "Apa Arti Kata Invalid?", "id": "Tidak Benar, Tidak Sah, Salah." },
  { "en": "Apa Arti Kata Trust Issue?", "id": "Masalah Sulit Percaya Pada Orang Lain." },
  { "en": "Apa Arti Kata Anxiety?", "id": "Gangguan Kecemasan Berlebih." },
  { "en": "Literally Itu Maksudnya Apa?", "id": "Secara Harfiah, Benar-Benar Terjadi." },
  { "en": "Basically Itu Maksudnya Apa?", "id": "Pada Dasarnya, Inti Masalahnya." },
  { "en": "Prefer Itu Maksudnya Apa?", "id": "Lebih Suka Satu Hal Tertentu." },
  { "en": "Handle Itu Maksudnya Apa?", "id": "Menangani Atau Mengatasi Suatu Masalah." },
  { "en": "Worth It Itu Maksudnya Apa?", "id": "Sepadan Dengan Usaha, Waktu, Biaya." },
  { "en": "Not Worth It Maksudnya Apa?", "id": "Tidak Sepadan Dengan Usaha, Rugi." },
  { "en": "Healing Itu Maksudnya Apa?", "id": "Proses Penyembuhan Diri (Fisik, Mental)." },
  { "en": "Healing (Gaul) Itu Maksudnya Apa?", "id": "Jalan-Jalan, Liburan, Menenangkan Diri." },
  { "en": "Clingy Itu Maksudnya Apa?", "id": "Sifat Terlalu Manja, Lengket, Bergantung." },
  { "en": "Random Itu Maksudnya Apa?", "id": "Acak, Tidak Terduga, Tidak Berpola." },
  { "en": "Vibes Itu Maksudnya Apa?", "id": "Aura, Suasana, Energi Yang Dirasakan." },
  { "en": "Urgent Itu Maksudnya Apa?", "id": "Sangat Mendesak, Gawat, Harus Segera." },
  { "en": "Deadline Itu Maksudnya Apa?", "id": "Batas Waktu Terakhir Penyelesaian Tugas." },
  { "en": "Meeting Itu Maksudnya Apa?", "id": "Rapat, Pertemuan Diskusi Formal." },
  { "en": "Follow Up Itu Maksudnya Apa?", "id": "Menindaklanjuti Sesuatu Yang Sudah Dibahas." },
  { "en": "Briefing Itu Maksudnya Apa?", "id": "Pengarahan Singkat Sebelum Memulai Pekerjaan." },
  { "en": "Overtime Itu Maksudnya Apa?", "id": "Kerja Lembur, Bekerja Melebihi Jam Normal." },
  { "en": "KPI (Key Performance Indicator) Itu Apa?", "id": "Indikator Utama Penilaian Kinerja." },
  { "en": "SOP (Standard Operating Procedure) Itu Apa?", "id": "Prosedur Standar Operasi Kerja." },
  { "en": "Hoax Itu Maksudnya Apa?", "id": "Berita Palsu, Informasi Bohong." },
  { "en": "Absurd Itu Maksudnya Apa?", "id": "Konyol, Aneh, Tidak Masuk Akal." },
  { "en": "Ambigu Itu Maksudnya Apa?", "id": "Memiliki Makna Ganda, Tidak Jelas." },
  { "en": "Ironi Itu Maksudnya Apa?", "id": "Sindiran, Berkata Sebaliknya Dari Kenyataan." },
  { "en": "Esensi Itu Maksudnya Apa?", "id": "Inti, Hakikat, Pokok Dari Sesuatu." },
  { "en": "Krusial Itu Maksudnya Apa?", "id": "Sangat Penting, Menentukan, Gawat." },
  { "en": "Signifikan Itu Maksudnya Apa?", "id": "Penting, Berarti, Berdampak Besar." },
  { "en": "Komitmen Itu Maksudnya Apa?", "id": "Keterikatan Janji Pada Sesuatu." },
  { "en": "Konteks Itu Maksudnya Apa?", "id": "Situasi Yang Melatarbelakangi Sesuatu." },
  { "en": "Fleksibel Itu Maksudnya Apa?", "id": "Luwes, Mudah Menyesuaikan Diri." },
  { "en": "Efisien Itu Maksudnya Apa?", "id": "Tepat Guna, Hemat Waktu, Hemat Biaya." },
  { "en": "Efektif Itu Maksudnya Apa?", "id": "Berhasil Guna, Mencapai Tujuan." },
  { "en": "Gercep Itu Maksudnya Apa?", "id": "Singkatan Gerak Cepat, Tanggap." },
  { "en": "Modus Itu Maksudnya Apa?", "id": "Singkatan Modal Dusta, Niat Terselubung." },
  { "en": "Galau Itu Maksudnya Apa?", "id": "Perasaan Resah, Bimbang, Sedih." },
  { "en": "Alay Itu Maksudnya Apa?", "id": "Gaya Berlebihan, Norak, Suka Pamer." },
  { "en": "Lebay Itu Maksudnya Apa?", "id": "Sikap Berlebihan Merespon Sesuatu." },
  { "en": "Kepo Itu Maksudnya Apa?", "id": "Selalu Ingin Tahu Urusan Orang Lain." },
  { "en": "Curhat Itu Maksudnya Apa?", "id": "Singkatan Curahan Hati, Bercerita." },
  { "en": "Baperan Itu Maksudnya Apa?", "id": "Orang Yang Mudah Terbawa Perasaan." },
  { "en": "TFL (Thanks For Like) Itu Apa?", "id": "Terima Kasih Telah Memberi Suka (Like)." },
  { "en": "AFK (Away From Keyboard) Itu Apa?", "id": "Sedang Tidak Di Depan Komputer." },
  { "en": "GWS (Get Well Soon) Itu Apa?", "id": "Semoga Lekas Sembuh Dari Sakit." },
  { "en": "TGIF (Thanks God It's Friday) Itu Apa?", "id": "Syukur Akhirnya Hari Jumat Tiba." },
  { "en": "IMO (In My Opinion) Itu Apa?", "id": "Menurut Pendapat Saya Pribadi." },
  { "en": "NGL (Not Gonna Lie) Itu Apa?", "id": "Jujur Saja, Sebenarnya." },
  { "en": "TMI (Too Much Information) Itu Apa?", "id": "Informasi Terlalu Detail, Tidak Perlu." },
  { "en": "POV (Point Of View) Itu Apa?", "id": "Sudut Pandang Seseorang Melihat Sesuatu." },
  { "en": "TBH (To Be Honest) Itu Apa?", "id": "Sejujurnya, Terus Terang Saja." },
  { "en": "RN (Right Now) Itu Apa?", "id": "Saat Ini Juga, Sekarang Juga." },
  { "en": "Cringe Itu Maksudnya Apa?", "id": "Merasa Geli, Jijik, Malu Melihatnya." },
  { "en": "Ghosting Itu Maksudnya Apa?", "id": "Menghilang Tiba-Tiba Tanpa Kabar." },
  { "en": "Spill Itu Maksudnya Apa?", "id": "Membocorkan Rahasia, Gosip, Atau Cerita." },
  { "en": "Toxic Itu Maksudnya Apa?", "id": "Beracun, Memberi Pengaruh Buruk." },
  { "en": "Savage Itu Maksudnya Apa?", "id": "Keren, Brutal, Berani, Tidak Peduli." },
  { "en": "Swag Itu Maksudnya Apa?", "id": "Gaya Keren, Penampilan Yang Menarik." },
  { "en": "Halu Itu Maksudnya Apa?", "id": "Singkatan Halusinasi, Mengkhayal." },
  { "en": "Insecure Itu Maksudnya Apa?", "id": "Merasa Tidak Aman, Kurang Percaya Diri." },
  { "en": "Effort Itu Maksudnya Apa?", "id": "Usaha Keras Yang Dikeluarkan." },
  { "en": "Privilege Itu Maksudnya Apa?", "id": "Hak Istimewa, Keuntungan Khusus." },
  { "en": "Circle Itu Maksudnya Apa?", "id": "Lingkaran Pertemanan, Pergaulan." },
  { "en": "Virtual Itu Maksudnya Apa?", "id": "Tidak Nyata, Maya, Via Internet." },
  { "en": "Relevan Itu Maksudnya Apa?", "id": "Berkaitan, Berhubungan, Sesuai Topik." },
  { "en": "Konsisten Itu Maksudnya Apa?", "id": "Tetap, Teguh, Tidak Berubah-Ubah." },
  { "en": "Apa Arti Kata Spontan?", "id": "Melakukan Sesuatu Tanpa Rencana." },
  { "en": "Validasi Itu Artinya Apa?", "id": "Proses Pembuktian Kebenaran Sesuatu." },
  { "en": "Verifikasi Itu Artinya Apa?", "id": "Proses Pemeriksaan Kesesuaian Data." },
  { "en": "Apa Arti Kata Hype?", "id": "Sesuatu Yang Sedang Ramai Dibicarakan." },
  { "en": "Apa Arti Kata Lowkey?", "id": "Secara Diam-Diam, Tidak Terbuka." },
  { "en": "Apa Arti Kata Highkey?", "id": "Secara Terang-Terangan, Terbuka." },
  { "en": "Apa Arti Kata FOMO (Fear Of Missing Out)?", "id": "Takut Ketinggalan Tren Atau Momen." },
  { "en": "Apa Arti Kata FWB (Friends With Benefits)?", "id": "Teman Tapi Memiliki Hubungan Fisik." },
  { "en": "Apa Arti Kata TTM (Teman Tapi Mesra)?", "id": "Teman Biasa Tapi Bersikap Mesra." },
  { "en": "Apa Arti Kata Resign?", "id": "Mengundurkan Diri Dari Pekerjaan." },
  { "en": "Apa Arti Kata Freelance?", "id": "Pekerja Lepas, Tidak Terikat Perusahaan." },
  { "en": "Apa Arti Kata Hybrid (Kerja)?", "id": "Metode Kerja Campuran (WFO Dan WFH)." },
  { "en": "Apa Arti Singkatan WFH (Work From Home)?", "id": "Bekerja Penuh Dari Rumah." },
  { "en": "Apa Arti Singkatan WFO (Work From Office)?", "id": "Bekerja Penuh Dari Kantor." },
  { "en": "Apa Arti Kata Typing?", "id": "Penanda Seseorang Sedang Mengetik Pesan." },
  { "en": "Apa Arti Kata Typo?", "id": "Kesalahan Dalam Mengetik Kata." },
  { "en": "Apa Arti Singkatan VC (Video Call)?", "id": "Panggilan Video Tatap Muka Virtual." },
  { "en": "Apa Arti Singkatan VN (Voice Note)?", "id": "Pesan Yang Dikirim Berbentuk Suara." },
  { "en": "Apa Arti Singkatan PC (Personal Chat)?", "id": "Pesan Pribadi, Bukan Diskusi Grup." },
  { "en": "Apa Arti Kata PAP (Post A Picture)?", "id": "Permintaan Untuk Mengirim Foto." },
  { "en": "Apa Arti Kata Gaje?", "id": "Singkatan 'Gak Jelas', Aneh." },
  { "en": "Apa Arti Kata Komuk?", "id": "Singkatan 'Kondisi Muka' Atau Ekspresi." },
  { "en": "Apa Arti Kata Gemoy?", "id": "Pelesetan Dari Kata 'Gemas', Lucu." },
  { "en": "Apa Arti Kata Nego?", "id": "Negosiasi, Proses Tawar Menawar." },
  { "en": "Apa Arti Kata Deal?", "id": "Sepakat, Persetujuan Telah Tercapai." },
  { "en": "Apa Arti Kata COD (Cash On Delivery)?", "id": "Metode Bayar Tunai Saat Barang Sampai." },
  { "en": "Apa Arti Kata Preloved?", "id": "Barang Bekas Berkualitas Dijual Lagi." },
  { "en": "Apa Arti Kata Thrift?", "id": "Aktivitas Membeli Barang Bekas." },
  { "en": "Apa Maksud Kata Estetik?", "id": "Memiliki Nilai Keindahan, Artistik." },
  { "en": "Apa Maksud Kata Hidden Gem?", "id": "Tempat Bagus Yang Belum Terkenal." },
  { "en": "Apa Maksud Kata Jujurly?", "id": "Bentuk Gaul Dari 'Jujur Saja'." },
  { "en": "Apa Maksud Kata Ngab?", "id": "Panggilan 'Bang' Yang Dibalik." },
  { "en": "Apa Maksud Kata Sabi?", "id": "Kata 'Bisa' Yang Dibalik." },
  { "en": "Apa Maksud Kata Kuy?", "id": "Kata 'Yuk' Yang Dibalik, Ajakan." },
  { "en": "Apa Maksud Kata Takis?", "id": "Kata 'Sikat' Yang Dibalik, Ambil." },
  { "en": "Apa Maksud Kata Red Flag?", "id": "Tanda Bahaya Dalam Sebuah Hubungan." },
  { "en": "Apa Maksud Kata Green Flag?", "id": "Tanda Baik Dalam Sebuah Hubungan." },
  { "en": "Apa Maksud Kata Gaslighting?", "id": "Manipulasi Membuat Korban Ragu." },
  { "en": "Apa Maksud Kata Support System?", "id": "Lingkungan Yang Memberi Dukungan." },
  { "en": "Apa Maksud Kata Burnout?", "id": "Kelelahan Ekstrem Akibat Kerja." },
  { "en": "Apa Maksud Kata Mental Breakdown?", "id": "Kondisi Emosional Yang Sangat Terpuruk." },
  { "en": "Apa Maksud Kata Quarter Life Crisis?", "id": "Krisis Identitas Di Usia 20-An." },
  { "en": "Apa Maksud Kata Self Love?", "id": "Upaya Mencintai Diri Sendiri." },
  { "en": "Apa Maksud Kata Self Reward?", "id": "Hadiah Untuk Diri Sendiri." },
  { "en": "Apa Maksud Kata Mindset?", "id": "Pola Pikir Yang Membentuk Sifat." },
  { "en": "Apa Maksud Kata Attitude?", "id": "Sikap, Perilaku, Atau Budi Pekerti." },
  { "en": "Apa Maksud Kata Respect?", "id": "Rasa Hormat Kepada Orang Lain." },
  { "en": "Apa Maksud Kata Confident?", "id": "Perasaan Percaya Diri Yang Tinggi." },
  { "en": "Apa Maksud Kata Humble?", "id": "Sikap Rendah Hati, Tidak Sombong." },
  { "en": "Apa Maksud Kata Loyal?", "id": "Sikap Setia, Patuh, Taat." },
  { "en": "Apa Maksud Kata Netizen?", "id": "Pengguna Aktif Internet, Warganet." },
  { "en": "Apa Maksud Kata Influencer?", "id": "Orang Yang Mempengaruhi Opini Publik." },
  { "en": "Apa Maksud Kata Endorse?", "id": "Promosi Berbayar Oleh Influencer." },
  { "en": "Apa Maksud Kata Unboxing?", "id": "Video Proses Membuka Paket." },
  { "en": "Apa Maksud Kata Review?", "id": "Ulasan Penilaian Terhadap Produk." },
  { "en": "Apa Maksud Kata Workshop?", "id": "Lokakarya Atau Pelatihan Singkat." },
  { "en": "Apa Maksud Kata Webinar?", "id": "Seminar Yang Dilakukan Online." },
  { "en": "Apa Maksud Kata Podcast?", "id": "Siaran Audio Digital Berbasis Topik." },
  { "en": "Apa Maksud Kata Playlist?", "id": "Daftar Putar Lagu Atau Video." },
  { "en": "Apa Arti Singkatan GG (Good Game)?", "id": "Permainan Yang Bagus, Pujian." },
  { "en": "Apa Arti Singkatan Noob?", "id": "Sebutan Untuk Pemain Amatir." },
  { "en": "Apa Arti Kata Pro?", "id": "Sebutan Untuk Pemain Profesional." },
  { "en": "Apa Arti Kata Mabar?", "id": "Singkatan 'Main Bareng' Game." },
  { "en": "Apa Arti Kata Ngelag?", "id": "Kondisi Koneksi Lambat, Patah-Patah." },
  { "en": "Apa Arti Kata Imba?", "id": "Singkatan 'Imbalance', Terlalu Kuat." },
  { "en": "Apa Arti Singkatan MVP (Most Valuable Player)?", "id": "Pemain Paling Berharga Dalam Tim." },
  { "en": "Apa Arti Kata Flexing?", "id": "Aktivitas Memamerkan Kekayaan." },
  { "en": "Apa Arti Kata Glow Up?", "id": "Perubahan Drastis Menjadi Lebih Baik." },
  { "en": "Apa Arti Kata Bestie?", "id": "Panggilan Untuk Sahabat Dekat." },
  { "en": "Apa Arti Kata Crush?", "id": "Orang Yang Disukai Secara Diam-Diam." },
  { "en": "Apa Arti Kata PDKT?", "id": "Singkatan 'Pendekatan' Sebelum Pacaran." },
  { "en": "Apa Arti Kata PHP (Pemberi Harapan Palsu)?", "id": "Memberi Harapan Cinta Tapi Bohong." },
  { "en": "Apa Arti Kata CLBK (Cinta Lama Bersemi Kembali)?", "id": "Balikan Dengan Mantan Pacar." },
  { "en": "Apa Arti Kata Move On?", "id": "Melupakan Masa Lalu, Melanjutkan Hidup." },
  { "en": "Apa Arti Kata Gamon?", "id": "Singkatan 'Gagal Move On'." },
  { "en": "Apa Arti Kata Salting?", "id": "Singkatan 'Salah Tingkah', Gugup." },
  { "en": "Apa Arti Kata Friendzone?", "id": "Hubungan Sebatas Teman, Cinta Bertepuk Sebelah Tangan." },
  { "en": "Apa Arti Kata Brotherzone?", "id": "Dianggap Sebagai Kakak Laki-Laki." },
  { "en": "Apa Arti Kata Denial?", "id": "Fase Penyangkalan, Tidak Menerima Fakta." },
  { "en": "Apa Arti Kata Ilfeel?", "id": "Singkatan 'Hilang Feeling', Jijik." },
  { "en": "Apa Arti Kata Me Time?", "id": "Waktu Pribadi Untuk Diri Sendiri." },
  { "en": "Apa Arti Kata Staycation?", "id": "Liburan Di Hotel Dalam Kota." },
  { "en": "Apa Arti Kata Workaholic?", "id": "Orang Yang Kecanduan Bekerja." },
  { "en": "Apa Arti Kata Introvert?", "id": "Pribadi Yang Nyaman Dalam Kesendirian." },
  { "en": "Apa Arti Kata Ekstrovert?", "id": "Pribadi Yang Nyaman Di Keramaian." },
  { "en": "Apa Arti Kata Ambivert?", "id": "Pribadi Gabungan Introvert Dan Ekstrovert." },
  { "en": "Apa Arti Kata Judgmental?", "id": "Sikap Suka Menghakimi Orang Lain." },
  { "en": "Apa Arti Kata TBL (Takut Banget Loh)?", "id": "Ekspresi Yang Menunjukkan Rasa Takut." },
  { "en": "Apa Arti Kata Plot Twist?", "id": "Alur Cerita Yang Mengejutkan." },
  { "en": "Apa Arti Kata Pick Me?", "id": "Orang Yang Berusaha Keras Tampil Beda." },
  { "en": "Apa Arti Kata Cegil?", "id": "Singkatan 'Cewek Gila', Intens." },
  { "en": "Apa Arti Kata Cokul?", "id": "Singkatan 'Cowok Cool', Keren." },
  { "en": "Apa Arti Kata Flop?", "id": "Sesuatu Yang Gagal Total." },
  { "en": "Apa Arti Kata Era?", "id": "Sebuah Masa Atau Periode Tertentu." },
  { "en": "Apa Arti Kata Delulu?", "id": "Singkatan 'Delusional', Terlalu Berkhayal." },
  { "en": "Apa Arti Kata Solulu?", "id": "Singkatan 'Solusi', Jalan Keluar." },
  { "en": "Apa Arti Kata Nolep?", "id": "Singkatan 'No Life', Anti Sosial." },
  { "en": "Apa Arti Kata Sotoy?", "id": "Singkatan 'Sok Tahu', Merasa Paling Paham." },
  { "en": "Apa Arti Kata Songong?", "id": "Sikap Sombong, Angkuh, Belagu." },
  { "en": "Apa Arti Kata Julid?", "id": "Sinis, Iri, Suka Mengomentari Negatif." },
  { "en": "Apa Arti Kata Nyinyir?", "id": "Komentar Pedas Sambil Meremehkan." },
  { "en": "Apa Arti Kata SJW (Social Justice Warrior)?", "id": "Pejuang Keadilan Sosial (Seringkali Sinis)." },
  { "en": "Apa Arti Kata NPC (Non-Player Character)?", "id": "Orang Yang Bertindak Seperti Robot, Pasif." },
  { "en": "Apa Arti Kata Based?", "id": "Berani Berpendapat Jujur, Tanpa Takut." },
  { "en": "Apa Arti Kata Main Character?", "id": "Merasa Jadi Tokoh Utama Kehidupan." },
  { "en": "Apa Arti Kata Side Character?", "id": "Merasa Jadi Tokoh Sampingan." },
  { "en": "Apa Arti Kata Backingan?", "id": "Orang Kuat Yang Melindungi Seseorang." },
  { "en": "Apa Arti Kata Cuan?", "id": "Keuntungan Finansial, Uang." },
  { "en": "Apa Arti Kata Boncos?", "id": "Mengalami Kerugian Finansial." },
  { "en": "Apa Arti Kata Hoki?", "id": "Keberuntungan Yang Tidak Terduga." },
  { "en": "Apa Arti Kata Apes?", "id": "Nasib Sial Atau Malang." },
  { "en": "Apa Arti Kata Empati?", "id": "Mampu Ikut Merasakan Perasaan Orang." },
  { "en": "Apa Arti Kata Simpati?", "id": "Menunjukkan Kepedulian (Tidak Ikut Merasa)." },
  { "en": "Apa Arti Kata Apatis?", "id": "Sikap Acuh Tak Acuh, Tidak Peduli." },
  { "en": "Apa Arti Kata Euforia?", "id": "Perasaan Senang Berlebihan." },
  { "en": "Apa Arti Kata Nostalgia?", "id": "Rindu Kenangan Manis Masa Lalu." },
  { "en": "Apa Arti Kata Melankolis?", "id": "Sifat Murung, Sedih, Penuh Perenungan." },
  { "en": "Apa Arti Kata Stigma?", "id": "Pandangan Negatif Masyarakat." },
  { "en": "Apa Arti Kata Narasi?", "id": "Cerita Atau Sudut Pandang." },
  { "en": "Apa Arti Kata Literasi?", "id": "Kemampuan Membaca Dan Memahami." },
  { "en": "Apa Arti Kata Urgensi?", "id": "Tingkat Kepentingan, Mendesak." },
  { "en": "Apa Arti Kata Esensial?", "id": "Sangat Penting, Mendasar, Pokok." },
  { "en": "Apa Arti Kata Progresif?", "id": "Berkembang Maju, Ada Kemajuan." },
  { "en": "Apa Arti Kata Implisit?", "id": "Makna Tersirat, Tidak Langsung." },
  { "en": "Apa Arti Kata Eksplisit?", "id": "Makna Jelas, Tegas, Langsung." },
  { "en": "Apa Arti Kata Konspirasi?", "id": "Teori Rahasia Dibalik Peristiwa." },
  { "en": "Apa Arti Kata Oportunis?", "id": "Mengambil Keuntungan Dari Situasi." },
  { "en": "Apa Arti Kata Idealis?", "id": "Berpikir Sesuai Harapan Sempurna." },
  { "en": "Apa Arti Kata Pragmatis?", "id": "Berpikir Praktis Berorientasi Hasil." },
  { "en": "Apa Arti Kata Pasif?", "id": "Tidak Aktif, Hanya Menerima." },
  { "en": "Apa Arti Kata Agresif?", "id": "Sikap Menyerang, Cenderung Memaksa." },
  { "en": "Apa Arti Kata Pasif-Agresif?", "id": "Agresi Yang Ditunjukkan Tidak Langsung." },
  { "en": "Apa Arti Kata Asertif?", "id": "Tegas Menyatakan Pendapat Tanpa Menyerang." },
  { "en": "Apa Arti Kata Konsumtif?", "id": "Sifat Boros, Suka Berbelanja." },
  { "en": "Apa Arti Kata Hedonisme?", "id": "Paham Yang Mengejar Kesenangan." },
  { "en": "Apa Arti Kata Minimalis?", "id": "Gaya Hidup Sederhana, Secukupnya." },
  { "en": "Apa Arti Kata Frugal Living?", "id": "Gaya Hidup Hemat Dan Cermat." },
  { "en": "Apa Arti Kata Integritas?", "id": "Kejujuran Dan Konsistensi Moral." },
  { "en": "Apa Arti Kata Kredibilitas?", "id": "Tingkat Kepercayaan Terhadap Seseorang." },
  { "en": "Apa Arti Kata Loyalitas?", "id": "Kesetiaan Atau Kepatuhan Penuh." },
  { "en": "Fundamental Itu Artinya Apa?", "id": "Sangat Mendasar, Pokok, Prinsip." },
  { "en": "Relatif Itu Artinya Apa?", "id": "Tidak Mutlak, Tergantung Pembanding." },
  { "en": "Objektif Itu Artinya Apa?", "id": "Berdasarkan Fakta, Tidak Memihak." },
  { "en": "Subjektif Itu Artinya Apa?", "id": "Berdasarkan Perasaan Pribadi." },
  { "en": "Apa Arti Kata Worthy?", "id": "Merasa Layak Atau Pantas." },
  { "en": "Apa Arti Kata Clueless?", "id": "Sama Sekali Tidak Tahu." },
  { "en": "Apa Arti Kata Cuddling?", "id": "Berpelukan Mesra Mencari Kenyamanan." },
  { "en": "Apa Arti Kata Triggered?", "id": "Terpancing Emosi Negatifnya." },
  { "en": "Apa Arti Kata Awkward?", "id": "Situasi Canggung Atau Kikuk." },
  { "en": "Apa Arti Kata Legit?", "id": "Singkatan 'Legitimate', Asli, Sah." },
  { "en": "Apa Arti Kata Slebew?", "id": "Kata Slang Sensual (Populer TikTok)." },
  { "en": "Apa Arti Kata YGY (Ya Guys Ya)?", "id": "Penegasan Kalimat, 'Iya Kan'." },
  { "en": "Apa Arti Kata NT (Nice Try)?", "id": "Pujian Atas Usaha Walau Gagal." },
  { "en": "Apa Arti Kata Damage (Gaul)?", "id": "Daya Tarik Yang Sangat Mempesona." },
  { "en": "Apa Arti Kata Prenjon?", "id": "Pelesetan Dari Kata 'Friendzone'." },
  { "en": "Apa Arti Kata Mleyot?", "id": "Sangat Lemas Karena Terlalu Suka." },
  { "en": "Apa Arti Kata Ngadi-Ngadi?", "id": "Mengada-Ada, Berkelakuan Aneh." },
  { "en": "Apa Arti Kata Rungkad?", "id": "Hancur Lebur, Kalah Telak." },
  { "en": "Apa Arti Kata Tepar?", "id": "Sangat Lelah Hingga Tak Berdaya." },
  { "en": "Apa Arti Kata Deep Talk?", "id": "Obrolan Mendalam Dan Serius." },
  { "en": "Apa Arti Kata Small Talk?", "id": "Obrolan Ringan Atau Basa-Basi." },
  { "en": "Apa Arti Kata Cancel Culture?", "id": "Budaya Memboikot Tokoh Publik." },
  { "en": "Apa Arti Kata WTB (Want To Buy)?", "id": "Penanda Serius Ingin Membeli Barang." },
  { "en": "Apa Arti Kata WTS (Want To Sell)?", "id": "Penanda Serius Ingin Menjual Barang." },
  { "en": "Apa Arti Kata WTT (Want To Trade)?", "id": "Penanda Serius Ingin Tukar Barang." },
  { "en": "Apa Arti Kata Niche?", "id": "Pasar Yang Sangat Spesifik, Ceruk." },
  { "en": "Apa Arti Kata Mainstream?", "id": "Sesuatu Yang Sangat Umum, Populer." },
  { "en": "Apa Arti Kata Underground?", "id": "Sesuatu Yang Tidak Populer." },
  { "en": "Apa Arti Kata Skena?", "id": "Lingkaran Kolektif (Musik, Gaya Hidup)." },
  { "en": "Apa Arti Kata Polisi Skena?", "id": "Orang Yang Merasa Paling Paham Skena." },
  { "en": "Apa Arti Kata OVT (Overthinking)?", "id": "Berpikir Berlebihan Akan Sesuatu." },
  { "en": "Apa Arti Kata Closure?", "id": "Penerimaan, Penutupan Babak Hidup." },
  { "en": "Apa Arti Kata Relationship Goals?", "id": "Standar Ideal Hubungan Asmara." },
  { "en": "Apa Arti Kata Situationship?", "id": "Hubungan Tanpa Status, Lebih Dari Teman." },
  { "en": "Apa Arti Kata Dry Text?", "id": "Pesan Teks Singkat Dan Datar." },
  { "en": "Apa Arti Kata Love Bombing?", "id": "Pemberian Cinta Berlebihan Di Awal." },
  { "en": "Apa Arti Kata Silent Treatment?", "id": "Hukuman Mendiamkan Pasangan." },
  { "en": "Apa Arti Kata Breadcrumbing?", "id": "Memberi Harapan Kecil Tanpa Niat." },
  { "en": "Apa Arti Kata Benching?", "id": "Menjadikan Seseorang Pilihan Cadangan." },
  { "en": "Apa Arti Kata Orbiting?", "id": "Mengamati Medsos Mantan Tanpa Interaksi." },
  { "en": "Apa Arti Kata Cuffing Season?", "id": "Musim Mencari Pasangan Jangka Pendek." },
  { "en": "Apa Arti Kata Slow Burn?", "id": "Hubungan Yang Berkembang Perlahan." },
  { "en": "Apa Arti Kata Ick?", "id": "Perasaan Jijik Tiba-Tiba Pada Pasangan." },
  { "en": "Apa Arti Kata Beige Flag?", "id": "Sifat Aneh Pasangan (Tidak Buruk)." },
  { "en": "Apa Arti Kata Clickbait?", "id": "Judul Menjebak Untuk Mendapat Klik." },
  { "en": "Apa Arti Kata Buzzer?", "id": "Orang Dibayar Mendengungkan Isu." },
  { "en": "Apa Arti Kata Pargoy?", "id": "Goyangan Pesta (Populer TikTok)." },
  { "en": "Apa Arti Kata Paripurna?", "id": "Sempurna, Lengkap, Tanpa Cacat." },
  { "en": "Apa Arti Kata HQQ (Hakiki)?", "id": "Sesuatu Yang Paling Sebenarnya." },
  { "en": "Apa Arti Kata OOMF (One Of My Followers)?", "id": "Salah Satu Pengikut Saya." },
  { "en": "Apa Arti Kata Mutualan?", "id": "Aktivitas Saling Mengikuti Di Medsos." },
  { "en": "Apa Arti Kata Repost?", "id": "Mengunggah Ulang Konten Orang Lain." },
  { "en": "Apa Arti Kata Mention?", "id": "Menyebut Nama Akun Lain (Tag)." },
  { "en": "Apa Arti Kata Thread?", "id": "Rangkaian Cuitan (Tweet) Bersambung." },
  { "en": "Apa Arti Kata Spill The Tea?", "id": "Membongkar Gosip Atau Rahasia." },
  { "en": "Salty Itu Maksudnya Apa?", "id": "Merasa Kesal, Iri, Atau Sinis." },
  { "en": "Shook Itu Maksudnya Apa?", "id": "Sangat Kaget, Terkejut, Heran." },
  { "en": "Stan Itu Maksudnya Apa?", "id": "Penggemar Garis Keras (Stalker Fan)." },
  { "en": "Ship Itu Maksudnya Apa?", "id": "Mendukung Hubungan Antara Dua Orang." },
  { "en": "OTP (One True Pairing) Itu Apa?", "id": "Pasangan Yang Dianggap Paling Cocok." },
  { "en": "Fandom Itu Maksudnya Apa?", "id": "Komunitas Penggemar Sesuatu (Grup, Film)." },
  { "en": "Bias Itu Maksudnya Apa?", "id": "Anggota Grup Idola Yang Paling Disukai." },
  { "en": "Wrecked Itu Maksudnya Apa?", "id": "Terpesona Idola Lain Selain Bias Utama." },
  { "en": "Hiatus Itu Maksudnya Apa?", "id": "Masa Istirahat Atau Vakum Sementara." },
  { "en": "Comeback Itu Maksudnya Apa?", "id": "Kembali Merilis Karya Baru (Musik)." },
  { "en": "Rookie Itu Maksudnya Apa?", "id": "Pendatang Baru, Artis Generasi Baru." },
  { "en": "Senior Itu Maksudnya Apa?", "id": "Artis Yang Sudah Lama Berkarier." },
  { "en": "Hwaiting Itu Maksudnya Apa?", "id": "Kata Semangat (Dari Bahasa Korea)." },
  { "en": "Mianhae Itu Maksudnya Apa?", "id": "Kata Maaf (Dari Bahasa Korea)." },
  { "en": "Gomawo Itu Maksudnya Apa?", "id": "Kata Terima Kasih (Dari Bahasa Korea)." },
  { "en": "Saranghae Itu Maksudnya Apa?", "id": "Aku Cinta Kamu (Dari Bahasa Korea)." },
  { "en": "Aigo Itu Maksudnya Apa?", "id": "Ekspresi Kaget Atau Mengeluh (Korea)." },
  { "en": "Daebak Itu Maksudnya Apa?", "id": "Ekspresi Luar Biasa, Hebat (Korea)." },
  { "en": "Oppa Itu Panggilan Untuk Siapa?", "id": "Wanita Ke Pria Lebih Tua (Korea)." },
  { "en": "Unnie Itu Panggilan Untuk Siapa?", "id": "Wanita Ke Wanita Lebih Tua (Korea)." },
  { "en": "Hyung Itu Panggilan Untuk Siapa?", "id": "Pria Ke Pria Lebih Tua (Korea)." },
  { "en": "Noona Itu Panggilan Untuk Siapa?", "id": "Pria Ke Wanita Lebih Tua (Korea)." },
  { "en": "Maknae Itu Panggilan Untuk Siapa?", "id": "Anggota Termuda Dalam Grup (Korea)." },
  { "en": "Agency Itu Maksudnya Apa?", "id": "Perusahaan Yang Menaungi Artis." },
  { "en": "Trainee Itu Maksudnya Apa?", "id": "Orang Yang Berlatih Menjadi Idola." },
  { "en": "Debut Itu Maksudnya Apa?", "id": "Penampilan Resmi Pertama Kali." },
  { "en": "FYP (For Your Page) Itu Apa?", "id": "Halaman Rekomendasi Utama TikTok." },
  { "en": "Check Itu Maksudnya Apa (TikTok)?", "id": "Tren Video Memeriksa Sesuatu." },
  { "en": "IB (Inspired By) Itu Apa?", "id": "Penanda Kredit, Terinspirasi Oleh." },
  { "en": "DC (Dance Credit) Itu Apa?", "id": "Penanda Kredit Pembuat Gerakan Tari." },
  { "en": "Stitch Itu Maksudnya Apa (TikTok)?", "id": "Menggabungkan Video Orang Lain." },
  { "en": "Duet Itu Maksudnya Apa (TikTok)?", "id": "Video Berdampingan Dengan Kreator Lain." },
  { "en": "CO Itu Maksudnya Apa (Belanja)?", "id": "Singkatan 'Check Out', Menyelesaikan Belanja." },
  { "en": "Keranjang Kuning Itu Apa?", "id": "Fitur Berjualan Di TikTok Shop." },
  { "en": "Inner Child Itu Maksudnya Apa?", "id": "Sisi Kekanak-Kanakan Dalam Diri." },
  { "en": "Toxic Positivity Itu Apa?", "id": "Sikap Positif Berlebihan Yang Salah." },
  { "en": "Self-Awareness Itu Maksudnya Apa?", "id": "Kesadaran Penuh Atas Diri Sendiri." },
  { "en": "Impulsive Itu Maksudnya Apa?", "id": "Bertindak Tiba-Tiba Tanpa Berpikir Panjang." },
  { "en": "Procrastination Itu Maksudnya Apa?", "id": "Kebiasaan Menunda-Nunda Pekerjaan." },
  { "en": "Produktif Itu Maksudnya Apa?", "id": "Menghasilkan Banyak Sesuatu, Efisien." },
  { "en": "Kontraproduktif Itu Maksudnya Apa?", "id": "Tindakan Yang Menghambat Hasil." },
  { "en": "Ambisius Itu Maksudnya Apa?", "id": "Penuh Ambisi, Keinginan Kuat Meraih Sukses." },
  { "en": "Optimis Itu Maksudnya Apa?", "id": "Selalu Berpikiran Baik, Penuh Harapan." },
  { "en": "Pesimis Itu Maksudnya Apa?", "id": "Selalu Berpikiran Buruk, Hilang Harapan." },
  { "en": "Passion Itu Maksudnya Apa?", "id": "Minat Besar, Gairah Terhadap Sesuatu." },
  { "en": "Visi Itu Maksudnya Apa?", "id": "Tujuan Jangka Panjang, Pandangan Masa Depan." },
  { "en": "Misi Itu Maksudnya Apa?", "id": "Langkah-Langkah Spesifik Mencapai Visi." },
  { "en": "Core Value Itu Maksudnya Apa?", "id": "Nilai-Nilai Inti Yang Dipegang." },
  { "en": "Feedback Itu Maksudnya Apa?", "id": "Umpan Balik, Masukan, Atau Kritik." },
  { "en": "TBD (To Be Decided) Itu Apa?", "id": "Akan Ditentukan Kemudian Harinya." },
  { "en": "TBC (To Be Confirmed) Itu Apa?", "id": "Akan Dikonfirmasi Nanti Kepastiannya." },
  { "en": "N/A (Not Available) Itu Apa?", "id": "Tidak Tersedia Atau Tidak Berlaku." },
  { "en": "Tip-Ex Itu Maksudnya Apa?", "id": "Cairan Putih Untuk Menghapus Tinta Pulpen." },
  { "en": "Odol Itu Maksudnya Apa?", "id": "Sebutan Umum Untuk Pasta Gigi." },
  { "en": "Setrika Itu Maksudnya Apa?", "id": "Alat Listrik Pelicin Pakaian Kusut." },
  { "en": "BUC (Butuh Uang Cepat) Itu Apa?", "id": "Menjual Barang Karena Kebutuhan Mendesak." },
  { "en": "Curcol Itu Maksudnya Apa?", "id": "Singkatan 'Curhat Colongan', Cerita Tiba-Tiba." },
  { "en": "Mupeng Itu Maksudnya Apa?", "id": "Singkatan 'Muka Pengen', Sangat Ingin." },
  { "en": "Goks Itu Maksudnya Apa?", "id": "Pelesetan Dari 'Gokil', Sangat Keren." },
  { "en": "Unyu Itu Maksudnya Apa?", "id": "Imut, Lucu, Dan Menggemaskan." },
  { "en": "Mellow Itu Maksudnya Apa?", "id": "Perasaan Sedih, Sendu, Rindu." },
  { "en": "Bonyok Itu Maksudnya Apa?", "id": "Sebutan Gaul Untuk Ayah Dan Ibu." },
  { "en": "Bokap Itu Maksudnya Apa?", "id": "Sebutan Gaul Untuk Ayah." },
  { "en": "Nyokap Itu Maksudnya Apa?", "id": "Sebutan Gaul Untuk Ibu." },
  { "en": "Bro Itu Panggilan Untuk Siapa?", "id": "Singkatan 'Brother', Panggilan Akrab Pria." },
  { "en": "Sis Itu Panggilan Untuk Siapa?", "id": "Singkatan 'Sister', Panggilan Akrab Wanita." },
  { "en": "Gan Itu Panggilan Untuk Siapa?", "id": "Singkatan 'Juragan', Sapaan Di Internet." },
  { "en": "Aganwati Itu Panggilan Untuk Siapa?", "id": "Sebutan 'Juragan' Wanita Di Kaskus." },
  { "en": "Pertamax Itu Maksudnya Apa (Forum)?", "id": "Komentar Pertama Di Sebuah Postingan." },
  { "en": "Cendol Itu Maksudnya Apa (Kaskus)?", "id": "Reputasi Baik Atau Pujian Di Kaskus." },
  { "en": "Bata Itu Maksudnya Apa (Kaskus)?", "id": "Reputasi Buruk Atau Hinaan Di Kaskus." },
  { "en": "Sundul Itu Maksudnya Apa (Forum)?", "id": "Menaikkan Postingan Lama Agar Terlihat." },
  { "en": "Silent Reader Itu Maksudnya Apa?", "id": "Pembaca Pasif Yang Tidak Berkomentar." },
  { "en": "Kopdar (Kopi Darat) Itu Apa?", "id": "Pertemuan Tatap Muka Komunitas Online." },
  { "en": "Admin Itu Maksudnya Apa?", "id": "Administrator, Pengelola Grup Atau Halaman." },
  { "en": "Mimin Itu Maksudnya Apa?", "id": "Panggilan Akrab Untuk Admin." },
  { "en": "Momod Itu Maksudnya Apa?", "id": "Panggilan Akrab Untuk Moderator Forum." },
  { "en": "Moderator Itu Maksudnya Apa?", "id": "Pengawas Jalannya Diskusi Atau Forum." },
  { "en": "Banned Itu Maksudnya Apa?", "id": "Diblokir, Dilarang Mengakses Layanan." },
  { "en": "Akun Bodong Itu Maksudnya Apa?", "id": "Akun Palsu, Akun Tidak Asli." },
  { "en": "Akun Clone Itu Maksudnya Apa?", "id": "Akun Kedua, Duplikat Akun Utama." },
  { "en": "Alter Ego Itu Maksudnya Apa?", "id": "Kepribadian Kedua Seseorang (Sering Di Medsos)." },
  { "en": "Hate Speech Itu Maksudnya Apa?", "id": "Ujaran Kebencian Yang Menyerang." },
  { "en": "Cyberbullying Itu Maksudnya Apa?", "id": "Perundungan Atau Penindasan Di Dunia Maya." },
  { "en": "Doxing Itu Maksudnya Apa?", "id": "Menyebarkan Data Pribadi Orang Lain." },
  { "en": "Phishing Itu Maksudnya Apa?", "id": "Penipuan Mencuri Data Sensitif (Password)." },
  { "en": "Scam Itu Maksudnya Apa?", "id": "Penipuan Online Untuk Mendapatkan Uang." },
  { "en": "Trustworthy Itu Maksudnya Apa?", "id": "Seseorang Yang Dapat Dipercaya Penuh." },
  { "en": "Trusted Itu Maksudnya Apa?", "id": "Terpercaya (Biasanya Untuk Penjual Online)." },
  { "en": "Testi (Testimonial) Itu Apa?", "id": "Ulasan Jujur Dari Pembeli Produk." },
  { "en": "Ori (Original) Itu Maksudnya Apa?", "id": "Barang Asli, Bukan Barang Tiruan." },
  { "en": "KW (Kualitas) Itu Maksudnya Apa?", "id": "Barang Tiruan, Palsu, Atau Replika." },
  { "en": "Reject Itu Maksudnya Apa (Produk)?", "id": "Barang Cacat Produksi Dari Pabrik." },
  { "en": "BNIB (Brand New In Box) Itu Apa?", "id": "Barang Baru, Masih Segel Dalam Kotak." },
  { "en": "BNOB (Brand New Open Box) Itu Apa?", "id": "Barang Baru, Kotak Sudah Dibuka." },
  { "en": "Mint Condition Itu Maksudnya Apa?", "id": "Kondisi Barang Bekas Yang Seperti Baru." },
  { "en": "NPU (Nett Price Updated) Itu Apa?", "id": "Harga Nett, Tidak Bisa Ditawar Lagi." },
  { "en": "Nego Tipis Itu Maksudnya Apa?", "id": "Tawaran Boleh, Tapi Hanya Sedikit." },
  { "en": "Afgan Itu Maksudnya Apa (Nego)?", "id": "Menawar Harga Terlalu Sadis (Lagu 'Sadis')." },
  { "en": "DP (Down Payment) Itu Apa?", "id": "Uang Muka, Tanda Jadi Pembelian." },
  { "en": "Apa Arti PO (Pre-Order)?", "id": "Pesan Dan Bayar Dulu, Barang Nanti." },
  { "en": "Apa Arti Ready Stock?", "id": "Barang Tersedia, Siap Dikirim Langsung." },
  { "en": "Apa Arti Sold Out?", "id": "Barang Sudah Habis Terjual Semua." },
  { "en": "Apa Arti Restock?", "id": "Mengisi Ulang Stok Barang Yang Habis." },
  { "en": "Apa Arti Limited Edition?", "id": "Edisi Terbatas, Diproduksi Sedikit." },
  { "en": "Apa Arti Rare Item?", "id": "Barang Langka Yang Sulit Dicari." },
  { "en": "Apa Arti Wishlist?", "id": "Daftar Barang Yang Sangat Diinginkan." },
  { "en": "Apa Arti Impulsive Buying?", "id": "Membeli Barang Tiba-Tiba Tanpa Rencana." },
  { "en": "Apa Arti Cashback?", "id": "Uang Kembali Setelah Melakukan Pembelian." },
  { "en": "Apa Arti Flash Sale?", "id": "Diskon Besar Dalam Waktu Sangat Singkat." },
  { "en": "Apa Arti Paid Promote?", "id": "Bayar Akun Lain Untuk Promosi." },
  { "en": "Apa Arti Rate Card?", "id": "Daftar Harga Jasa (Misal Influencer)." },
  { "en": "Apa Arti Brief?", "id": "Arahan Singkat Mengenai Detail Pekerjaan." },
  { "en": "Apa Arti Revisi?", "id": "Perbaikan Atau Pengulangan Hasil Kerja." },
  { "en": "Apa Arti ACC?", "id": "Disetujui Atau Diterima Oleh Atasan." },
  { "en": "Apa Arti Pending?", "id": "Status Tertunda, Menunggu Keputusan." },
  { "en": "Apa Arti Cancel?", "id": "Dibatalkan, Tidak Jadi Dilakukan." },
  { "en": "Apa Arti Reschedule?", "id": "Menjadwalkan Ulang Waktu Acara." },
  { "en": "Apa Arti Reminder?", "id": "Pesan Pengingat Akan Sesuatu." },
  { "en": "Apa Arti To Do List?", "id": "Daftar Pekerjaan Yang Harus Dilakukan." },
  { "en": "Apa Arti Multitasking?", "id": "Mengerjakan Banyak Tugas Sekaligus." },
  { "en": "Apa Arti Brokie?", "id": "Sebutan Untuk Orang Yang Sedang Bangkrut." },
  { "en": "Apa Arti PM (Personal Message)?", "id": "Mengirim Pesan Secara Pribadi." },
  { "en": "Apa Arti DM (Direct Message)?", "id": "Pesan Pribadi (Sama Seperti PM)." },
  { "en": "Apa Arti BM (Banyak Mau)?", "id": "Sedang Sangat Menginginkan Sesuatu." },
  { "en": "Apa Arti Old Money?", "id": "Kekayaan Yang Diwariskan Turun-Temurun." },
  { "en": "Apa Arti New Money?", "id": "Kekayaan Hasil Usaha Generasi Pertama." },
  { "en": "Apa Arti Sugar Daddy?", "id": "Pria Tua Kaya Membiayai Pasangan Muda." },
  { "en": "Apa Arti Sugar Baby?", "id": "Pasangan Muda Dibiayai Sugar Daddy." },
  { "en": "Apa Arti Gold Digger?", "id": "Orang Yang Mengejar Harta Pasangannya." },
  { "en": "Apa Arti Social Climber?", "id": "Orang Yang Suka Panjat Sosial." },
  { "en": "Apa Arti Drama Queen?", "id": "Orang Yang Suka Melebih-Lebihkan Masalah." },
  { "en": "Apa Arti Attention Seeker?", "id": "Orang Yang Selalu Mencari Perhatian." },
  { "en": "Apa Arti Humble Bragging?", "id": "Pamer Terselubung, Pura-Pura Merendah." },
  { "en": "Apa Arti Benchmark?", "id": "Standar Tolok Ukur Pembanding Kinerja." },
  { "en": "Apa Arti Insight?", "id": "Wawasan Baru Yang Mendalam." },
  { "en": "Apa Arti Stakeholder?", "id": "Pihak Yang Berkepentingan Dalam Proyek." },
  { "en": "Apa Arti Pitching?", "id": "Presentasi Ide Singkat Untuk Meyakinkan." },
  { "en": "Apa Arti Deck?", "id": "Materi Presentasi, Biasanya Format Slide." },
  { "en": "Apa Arti Case Study?", "id": "Studi Kasus, Analisis Masalah Spesifik." },
  { "en": "Apa Arti Data Driven?", "id": "Mengambil Keputusan Berdasarkan Data." },
  { "en": "Apa Arti User Experience (UX)?", "id": "Pengalaman Pengguna Saat Memakai Produk." },
  { "en": "Apa Arti User Interface (UI)?", "id": "Tampilan Visual Produk Digital." },
  { "en": "Apa Arti Glitch?", "id": "Kesalahan Teknis Kecil Pada Sistem." },
  { "en": "Apa Arti Bug?", "id": "Kesalahan Serius Pada Kode Program." },
  { "en": "Apa Arti Debugging?", "id": "Proses Mencari Dan Memperbaiki Bug." },
  { "en": "Apa Arti Bandwidth?", "id": "Kapasitas Maksimal Transfer Data Internet." },
  { "en": "Apa Arti Server Down?", "id": "Server Pusat Mati, Tidak Bisa Diakses." },
  { "en": "Apa Arti Maintenance?", "id": "Perawatan Sistem, Seringkali Offline." },
  { "en": "Apa Arti Backup?", "id": "Mencadangkan Data Agar Tidak Hilang." },
  { "en": "Apa Arti Restore?", "id": "Mengembalikan Data Cadangan Seperti Semula." },
  { "en": "Apa Arti Default?", "id": "Pengaturan Awal Atau Bawaan Pabrik." },
  { "en": "Apa Arti Template?", "id": "Pola Atau Cetakan Dasar Desain." },
  { "en": "Apa Arti Manual?", "id": "Dilakukan Dengan Tangan, Tidak Otomatis." },
  { "en": "Apa Arti Otomatis?", "id": "Bekerja Sendiri Tanpa Bantuan Manusia." },
  { "en": "Apa Arti Premis?", "id": "Asumsi Dasar Atau Awal Sebuah Argumen." },
  { "en": "Apa Arti Hipotesis?", "id": "Dugaan Awal Yang Perlu Diuji Kebenarannya." },
  { "en": "Apa Arti Paradoks?", "id": "Pernyataan Yang Terlihat Bertentangan Logika." },
  { "en": "Apa Arti Anomali?", "id": "Keanehan, Penyimpangan Dari Keadaan Normal." },
  { "en": "Apa Arti Dilema?", "id": "Situasi Sulit, Memilih Di Antara Dua Pilihan Buruk." },
  { "en": "Apa Arti Dinamis?", "id": "Selalu Bergerak, Penuh Perubahan." },
  { "en": "Apa Arti Statis?", "id": "Diam, Tetap, Tidak Berubah Keadaannya." },
  { "en": "Apa Arti Pro Rata?", "id": "Dihitung Secara Proporsional Sesuai Porsi." },
  { "en": "Apa Arti Lump Sum?", "id": "Pembayaran Penuh Dalam Satu Kali." },
  { "en": "Apa Arti Reimburse?", "id": "Penggantian Uang Yang Sudah Ditalangi." },
  { "en": "Apa Arti Invoice?", "id": "Tagihan Resmi Rincian Pembayaran." },
  { "en": "Apa Arti Quotation?", "id": "Penawaran Harga Resmi Sebelum Transaksi." },
  { "en": "Apa Arti PO (Purchase Order)?", "id": "Dokumen Resmi Pemesanan Barang." },
  { "en": "Apa Arti DO (Delivery Order)?", "id": "Dokumen Resmi Pengiriman Barang." },
  { "en": "Apa Arti Resi?", "id": "Bukti Pengiriman Barang Dari Ekspedisi." },
  { "en": "Apa Arti Ekspedisi?", "id": "Jasa Pengiriman Barang Atau Paket." },
  { "en": "Apa Arti Kurir?", "id": "Orang Yang Mengantarkan Paket." },
  { "en": "Apa Arti Internal?", "id": "Sesuatu Yang Berasal Dari Dalam." },
  { "en": "Apa Arti Eksternal?", "id": "Sesuatu Yang Berasal Dari Luar." },
  { "en": "Apa Arti Mayoritas?", "id": "Bagian Terbesar Atau Kebanyakan." },
  { "en": "Apa Arti Minoritas?", "id": "Bagian Terkecil Atau Sebagian Kecil." },
  { "en": "Apa Arti Prioritas?", "id": "Sesuatu Yang Diutamakan, Paling Penting." },
  { "en": "Apa Arti Sekunder?", "id": "Urutan Kedua, Tidak Terlalu Penting." },
  { "en": "Apa Arti Tersier?", "id": "Urutan Ketiga, Kebutuhan Mewah." },
  { "en": "Apa Arti Wir?", "id": "Panggilan 'Jawir', Merujuk Orang Jawa." },
  { "en": "Apa Arti Slay?", "id": "Melakukan Sesuatu Dengan Sangat Keren." },
  { "en": "Apa Arti Pookie?", "id": "Panggilan Sayang Yang Sangat Manja." },
  { "en": "Apa Arti Grinding (Game)?", "id": "Mengulang Aksi Untuk Menaikkan Level." },
  { "en": "Apa Arti Farming (Game)?", "id": "Mengulang Aksi Untuk Mengumpulkan Item." },
  { "en": "Apa Arti Cooldown (Game)?", "id": "Waktu Jeda Sebelum Skill Bisa Dipakai." },
  { "en": "Apa Arti AoE (Area Of Effect)?", "id": "Serangan Yang Mengenai Satu Area." },
  { "en": "Apa Arti Buff (Game)?", "id": "Peningkatan Status Atau Kekuatan." },
  { "en": "Apa Arti Nerf (Game)?", "id": "Penurunan Status Atau Kekuatan." },
  { "en": "Apa Arti Lag?", "id": "Keterlambatan Respon Karena Jaringan Lambat." },
  { "en": "Apa Arti Frame Drop?", "id": "Gambar Patah-Patah Karena Grafis Berat." },
  { "en": "Apa Arti Cheater?", "id": "Pemain Yang Menggunakan Program Curang." },
  { "en": "Apa Arti Hode?", "id": "Pria Yang Berpura-Pura Menjadi Wanita." },
  { "en": "Apa Arti Ngabuburit?", "id": "Menunggu Waktu Berbuka Puasa." },
  { "en": "Apa Arti Bukber?", "id": "Singkatan Buka Puasa Bersama." },
  { "en": "Apa Arti Takjil?", "id": "Makanan Ringan Untuk Berbuka Puasa." },
  { "en": "Apa Arti Mudik?", "id": "Pulang Ke Kampung Halaman Saat Lebaran." },
  { "en": "Apa Arti Halal Bihalal?", "id": "Acara Silaturahmi Saling Memaafkan." },
  { "en": "Apa Arti Singkatan THR (Tunjangan Hari Raya)?", "id": "Bonus Uang Yang Diterima Saat Lebaran." },
  { "en": "Apa Arti Arus Balik?", "id": "Perjalanan Kembali Ke Kota Perantauan." },
  { "en": "Apa Arti Parsel?", "id": "Bingkisan Hadiah Yang Dikirimkan." },
  { "en": "Apa Arti Kating?", "id": "Singkatan Kakak Tingkat Di Kampus." },
  { "en": "Apa Arti Dedek Gemes?", "id": "Panggilan Untuk Junior Yang Menarik." },
  { "en": "Apa Arti Kata Ospek?", "id": "Orientasi Pengenalan Kampus Mahasiswa Baru." },
  { "en": "Apa Arti Singkatan UKM (Unit Kegiatan Mahasiswa)?", "id": "Ekstrakurikuler Versi Mahasiswa Di Kampus." },
  { "en": "Apa Arti Singkatan BEM (Badan Eksekutif Mahasiswa)?", "id": "Organisasi Eksekutif Tertinggi Mahasiswa." },
  { "en": "Apa Arti Singkatan DPM (Dewan Perwakilan Mahasiswa)?", "id": "Organisasi Legislatif Pengawas BEM." },
  { "en": "Apa Arti Singkatan HIMA (Himpunan Mahasiswa)?", "id": "Organisasi Mahasiswa Tingkat Jurusan." },
  { "en": "Apa Arti Singkatan KRS (Kartu Rencana Studi)?", "id": "Lembar Pengambilan Mata Kuliah Per Semester." },
  { "en": "Apa Arti Singkatan KHS (Kartu Hasil Studi)?", "id": "Lembar Laporan Nilai Mata Kuliah." },
  { "en": "Apa Arti Singkatan IPK (Indeks Prestasi Kumulatif)?", "id": "Nilai Rata-Rata Gabungan Selama Kuliah." },
  { "en": "Apa Arti Singkatan SKS (Satuan Kredit Semester)?", "id": "Satuan Bobot Mata Kuliah." },
  { "en": "Apa Arti Kata Cumlaude?", "id": "Gelar Kehormatan Lulus Dengan Nilai Tinggi." },
  { "en": "Apa Arti Kata Skripsi?", "id": "Tugas Akhir Ilmiah Mahasiswa S1." },
  { "en": "Apa Arti Kata Tesis?", "id": "Tugas Akhir Ilmiah Mahasiswa S2." },
  { "en": "Apa Arti Kata Disertasi?", "id": "Tugas Akhir Ilmiah Mahasiswa S3." },
  { "en": "Apa Arti Kata Dosen?", "id": "Pengajar Atau Guru Di Perguruan Tinggi." },
  { "en": "Apa Arti Kata Rektor?", "id": "Pemimpin Tertinggi Perguruan Tinggi." },
  { "en": "Apa Arti Kata Dekan?", "id": "Pemimpin Tertinggi Fakultas Di Universitas." },
  { "en": "Apa Arti Kata Matkul?", "id": "Singkatan Dari Mata Kuliah." },
  { "en": "Apa Arti Kata Maba?", "id": "Singkatan Dari Mahasiswa Baru." },
  { "en": "Apa Arti Kata KKN (Kuliah Kerja Nyata)?", "id": "Pengabdian Mahasiswa Di Masyarakat Desa." },
  { "en": "Apa Arti Kata Magang?", "id": "Praktik Kerja Di Perusahaan (Internship)." },
  { "en": "Apa Arti Kata Wisuda?", "id": "Upacara Pelantikan Kelulusan Mahasiswa." },
  { "en": "Apa Arti Kata Alumni?", "id": "Orang Yang Telah Lulus Dari Sekolah." },
  { "en": "Apa Arti Kata Reuni?", "id": "Pertemuan Kembali Para Alumni Sekolah." },
  { "en": "Apa Arti Kata BPJS (Badan Penyelenggara Jaminan Sosial)?", "id": "Lembaga Asuransi Kesehatan Negara." },
  { "en": "Apa Arti Singkatan SIM (Surat Izin Mengemudi)?", "id": "Bukti Izin Resmi Mengendarai Kendaraan." },
  { "en": "Apa Arti Singkatan STNK (Surat Tanda Nomor Kendaraan)?", "id": "Bukti Kepemilikan Sah Kendaraan Bermotor." },
  { "en": "Apa Arti Singkatan BPKB (Buku Pemilik Kendaraan Bermotor)?", "id": "Buku Bukti Sah Kepemilikan Kendaraan." },
  { "en": "Apa Arti Singkatan KTP (Kartu Tanda Penduduk)?", "id": "Kartu Identitas Resmi Warga Negara." },
  { "en": "Apa Arti Singkatan KK (Kartu Keluarga)?", "id": "Kartu Identitas Data Anggota Keluarga." },
  { "en": "Apa Arti Singkatan NPWP (Nomor Pokok Wajib Pajak)?", "id": "Nomor Identitas Untuk Keperluan Pajak." },
  { "en": "Apa Arti Kata Tilang?", "id": "Bukti Pelanggaran Lalu Lintas." },
  { "en": "Apa Arti Kata Razia?", "id": "Pemeriksaan Mendadak Oleh Pihak Berwajib." },
  { "en": "Apa Arti Kata Macet?", "id": "Kondisi Lalu Lintas Yang Terhenti." },
  { "en": "Apa Arti Kata Banjir?", "id": "Genangan Air Yang Meluap." },
  { "en": "Apa Arti Kata Gempa?", "id": "Getaran Pada Permukaan Bumi." },
  { "en": "Apa Arti Kata Tsunami?", "id": "Gelombang Laut Besar Akibat Gempa." },
  { "en": "Apa Arti Kata Viral?", "id": "Sesuatu Yang Menyebar Cepat Di Internet." },
  { "en": "Apa Arti Kata Trending?", "id": "Topik Yang Sedang Hangat Dibicarakan." },
  { "en": "Apa Arti Kata Konten?", "id": "Isi Atau Materi (Video, Tulisan, Gambar)." },
  { "en": "Apa Arti Kata Kreator?", "id": "Orang Yang Membuat Konten." },
  { "en": "Apa Arti Kata Subscriber?", "id": "Pelanggan Saluran (Contoh: Youtube)." },
  { "en": "Apa Arti Kata Follower?", "id": "Pengikut Akun Media Sosial." },
  { "en": "Apa Arti Kata Username?", "id": "Nama Akun Pengguna Di Platform." },
  { "en": "Apa Arti Kata Password?", "id": "Kata Sandi Rahasia Untuk Masuk Akun." },
  { "en": "Apa Arti Kata Login?", "id": "Proses Masuk Ke Dalam Akun." },
  { "en": "Apa Arti Kata Logout?", "id": "Proses Keluar Dari Akun." },
  { "en": "Apa Arti Kata Signup?", "id": "Proses Mendaftar Akun Baru." },
  { "en": "Apa Arti Kata Error?", "id": "Terjadi Kesalahan Pada Sistem." },
  { "en": "Apa Arti Kata Loading?", "id": "Proses Memuat Data, Menunggu." },
  { "en": "Apa Arti Kata Buffering?", "id": "Proses Memuat Video (Streaming)." },
  { "en": "Apa Arti Kata Streaming?", "id": "Menonton Konten Langsung Via Internet." },
  { "en": "Apa Arti Kata Browsing?", "id": "Aktivitas Menjelajahi Situs Web." },
  { "en": "Apa Arti Kata Browser?", "id": "Aplikasi Untuk Membuka Situs Web." },
  { "en": "Apa Arti Kata Link?", "id": "Tautan, Alamat Situs Web." },
  { "en": "Apa Arti Kata URL (Uniform Resource Locator)?", "id": "Alamat Lengkap Sebuah Halaman Web." },
  { "en": "Apa Arti Kata Online?", "id": "Terhubung Dengan Jaringan Internet." },
  { "en": "Apa Arti Kata Offline?", "id": "Tidak Terhubung Dengan Jaringan Internet." },
  { "en": "Apa Arti Kata Wi-Fi?", "id": "Jaringan Nirkabel Untuk Koneksi Internet." },
  { "en": "Apa Arti Kata Kuota?", "id": "Batas Maksimal Penggunaan Data Internet." },
  { "en": "Apa Arti Kata Provider?", "id": "Perusahaan Penyedia Layanan Internet." },
  { "en": "Apa Arti Kata Sim Card?", "id": "Kartu Identitas Pelanggan Seluler." },
  { "en": "Apa Arti Kata Pulsa?", "id": "Saldo Untuk Layanan Telepon Seluler." },
  { "en": "Apa Arti Kata Smartphone?", "id": "Telepon Pintar Dengan Fitur Canggih." },
  { "en": "Apa Arti Kata Gadget?", "id": "Perangkat Elektronik Kecil, Gawai." },
  { "en": "Apa Arti Kata Laptop?", "id": "Komputer Jinjing Yang Portabel." },
  { "en": "Apa Arti Kata PC (Personal Computer)?", "id": "Komputer Pribadi Untuk Di Meja." },
  { "en": "Apa Arti Kata Keyboard?", "id": "Papan Ketik Untuk Memasukkan Teks." },
  { "en": "Apa Arti Kata Mouse?", "id": "Perangkat Penunjuk Kursor Di Layar." },
  { "en": "Apa Arti Kata Monitor?", "id": "Layar Penampil Gambar Komputer." },
  { "en": "Apa Arti Kata Speaker?", "id": "Perangkat Pengeras Suara." },
  { "en": "Apa Arti Kata Headphone?", "id": "Alat Dengar Audio Di Kepala." },
  { "en": "Apa Arti Kata Earphone?", "id": "Alat Dengar Audio Di Telinga." },
  { "en": "Apa Arti Kata Charger?", "id": "Alat Untuk Mengisi Ulang Baterai." },
  { "en": "Apa Arti Kata Power Bank?", "id": "Penyimpan Daya Baterai Portabel." },
  { "en": "Apa Arti Kata Kabel Data?", "id": "Kabel Untuk Transfer Data Atau Charger." },
  { "en": "Apa Arti Kata Overheat?", "id": "Kondisi Perangkat Terlalu Panas." },
  { "en": "Apa Arti Kata Hang?", "id": "Kondisi Perangkat Macet, Tidak Merespon." },
  { "en": "Apa Arti Kata Restart?", "id": "Mematikan Lalu Menghidupkan Ulang Perangkat." },
  { "en": "Apa Arti Kata Install?", "id": "Memasang Aplikasi Atau Program." },
  { "en": "Apa Arti Kata Uninstall?", "id": "Mencopot Aplikasi Atau Program." },
  { "en": "Apa Arti Kata Akun?", "id": "Identitas Pengguna Di Sebuah Layanan." },
  { "en": "Apa Arti Kata Notifikasi?", "id": "Pemberitahuan Atau Peringatan Baru." },
  { "en": "Apa Arti Kata Virus?", "id": "Program Jahat Perusak Sistem." },
  { "en": "Apa Arti Kata Antivirus?", "id": "Program Pelindung Dari Serangan Virus." },
  { "en": "Apa Arti Kata Firewall?", "id": "Sistem Keamanan Jaringan Komputer." },
  { "en": "Apa Arti Kata Overrated?", "id": "Sesuatu Yang Dinilai Terlalu Tinggi." },
  { "en": "Apa Arti Kata Underrated?", "id": "Sesuatu Yang Dinilai Terlalu Rendah." },
  { "en": "Apa Arti Kata Tip-Ex?", "id": "Cairan Putih Penghapus Tinta." },
  { "en": "Apa Arti Kata Odol?", "id": "Sebutan Lain Untuk Pasta Gigi." },
  { "en": "Apa Arti Kata Setrika?", "id": "Alat Melicinkan Pakaian." },
  { "en": "Apa Arti Kata Curcol?", "id": "Singkatan Curhat Colongan." },
  { "en": "Apa Arti Kata Mupeng?", "id": "Singkatan Muka Pengen, Sangat Ingin." },
  { "en": "Apa Arti Kata Unyu?", "id": "Imut, Lucu, Menggemaskan." },
  { "en": "Apa Arti Kata Mellow?", "id": "Perasaan Sedih, Sendu, Melankolis." },
  { "en": "Apa Arti Kata Bonyok?", "id": "Sebutan Lain Untuk Ayah Dan Ibu." },
  { "en": "Apa Arti Kata Bokap?", "id": "Sebutan Gaul Untuk Ayah." },
  { "en": "Apa Arti Kata Nyokap?", "id": "Sebutan Gaul Untuk Ibu." },
  { "en": "Apa Arti Kata Aganwati?", "id": "Panggilan 'Juragan' Wanita Di Kaskus." },
  { "en": "Apa Arti Kata Pertamax (Forum)?", "id": "Komentar Pertama Di Sebuah Postingan." },
  { "en": "Apa Arti Kata Cendol (Kaskus)?", "id": "Reputasi Baik Atau Pujian Di Kaskus." },
  { "en": "Apa Arti Kata Bata (Kaskus)?", "id": "Reputasi Buruk Atau Hinaan Di Kaskus." },
  { "en": "Apa Arti Kata Sundul (Forum)?", "id": "Menaikkan Postingan Lama Agar Terlihat." },
  { "en": "Apa Arti Kata Silent Reader?", "id": "Pembaca Pasif Yang Tidak Berkomentar." },
  { "en": "Apa Arti Kata Kopdar (Kopi Darat)?", "id": "Pertemuan Tatap Muka Komunitas Online." },
  { "en": "Apa Arti Kata Mimin?", "id": "Panggilan Akrab Untuk Admin." },
  { "en": "Apa Arti Kata Momod?", "id": "Panggilan Akrab Untuk Moderator Forum." },
  { "en": "Apa Arti Kata Banned?", "id": "Diblokir, Dilarang Mengakses Layanan." },
  { "en": "Apa Arti Kata Akun Bodong?", "id": "Akun Palsu, Akun Tidak Asli." },
  { "en": "Apa Arti Kata Akun Clone?", "id": "Akun Kedua, Duplikat Akun Utama." },
  { "en": "Apa Arti Kata Alter Ego?", "id": "Kepribadian Kedua Seseorang (Sering Di Medsos)." },
  { "en": "Apa Arti Kata Hate Speech?", "id": "Ujaran Kebencian Yang Menyerang." },
  { "en": "Apa Arti Kata Cyberbullying?", "id": "Perundungan Atau Penindasan Di Dunia Maya." },
  { "en": "Apa Arti Kata Doxing?", "id": "Menyebarkan Data Pribadi Orang Lain." },
  { "en": "Apa Arti Kata Phishing?", "id": "Penipuan Mencuri Data Sensitif (Password)." },
  { "en": "Apa Arti Kata Scam?", "id": "Penipuan Online Untuk Mendapatkan Uang." },
  { "en": "Apa Arti Kata Trustworthy?", "id": "Seseorang Yang Dapat Dipercaya Penuh." },
  { "en": "Apa Arti Kata Trusted?", "id": "Terpercaya (Biasanya Untuk Penjual Online)." },
  { "en": "Apa Arti Kata Testi (Testimonial)?", "id": "Ulasan Jujur Dari Pembeli Produk." },
  { "en": "Apa Arti Kata Ori (Original)?", "id": "Barang Asli, Bukan Barang Tiruan." },
  { "en": "Apa Arti Kata KW (Kualitas)?", "id": "Barang Tiruan, Palsu, Atau Replika." },
  { "en": "Apa Arti Kata Reject (Produk)?", "id": "Barang Cacat Produksi Dari Pabrik." },
  { "en": "Apa Arti Kata BNIB (Brand New In Box)?", "id": "Barang Baru, Masih Segel Dalam Kotak." },
  { "en": "Apa Arti Kata BNOB (Brand New Open Box)?", "id": "Barang Baru, Kotak Sudah Dibuka." },
  { "en": "Apa Arti Kata Mint Condition?", "id": "Kondisi Barang Bekas Yang Seperti Baru." },
  { "en": "Apa Arti Kata NPU (Nett Price Updated)?", "id": "Harga Nett, Tidak Bisa Ditawar Lagi." },
  { "en": "Apa Arti Kata Nego Tipis?", "id": "Tawaran Boleh, Tapi Hanya Sedikit." },
  { "en": "Apa Arti Kata Afgan (Nego)?", "id": "Menawar Harga Terlalu Sadis (Lagu 'Sadis')." },
  { "en": "Apa Arti Kata DP (Down Payment)?", "id": "Uang Muka, Tanda Jadi Pembelian." },
  { "en": "Apa Arti Kata PO (Pre-Order)?", "id": "Pesan Dan Bayar Dulu, Barang Nanti." },
  { "en": "Apa Arti Kata Ready Stock?", "id": "Barang Tersedia, Siap Dikirim Langsung." },
  { "en": "Apa Arti Kata Sold Out?", "id": "Barang Sudah Habis Terjual Semua." },
  { "en": "Apa Arti Kata Restock?", "id": "Mengisi Ulang Stok Barang Yang Habis." },
  { "en": "Apa Arti Kata Limited Edition?", "id": "Edisi Terbatas, Diproduksi Sedikit." },
  { "en": "Apa Arti Kata Rare Item?", "id": "Barang Langka Yang Sulit Dicari." },
  { "en": "Apa Arti Kata Wishlist?", "id": "Daftar Barang Yang Sangat Diinginkan." },
  { "en": "Apa Arti Kata Impulsive Buying?", "id": "Membeli Barang Tiba-Tiba Tanpa Rencana." },
  { "en": "Apa Arti Kata Cashback?", "id": "Uang Kembali Setelah Melakukan Pembelian." },
  { "en": "Apa Arti Kata Flash Sale?", "id": "Diskon Besar Dalam Waktu Sangat Singkat." },
  { "en": "Apa Arti Kata Paid Promote?", "id": "Bayar Akun Lain Untuk Promosi." },
  { "en": "Apa Arti Kata Rate Card?", "id": "Daftar Harga Jasa (Misal Influencer)." },
  { "en": "Apa Arti Kata Brief?", "id": "Arahan Singkat Mengenai Detail Pekerjaan." },
  { "en": "Apa Arti Kata Revisi?", "id": "Perbaikan Atau Pengulangan Hasil Kerja." },
  { "en": "Apa Arti Kata ACC?", "id": "Disetujui Atau Diterima Oleh Atasan." },
  { "en": "Apa Arti Kata Pending?", "id": "Status Tertunda, Menunggu Keputusan." },
  { "en": "Apa Arti Kata Cancel?", "id": "Dibatalkan, Tidak Jadi Dilakukan." },
  { "en": "Apa Arti Kata Reschedule?", "id": "Menjadwalkan Ulang Waktu Acara." },
  { "en": "Apa Arti Kata Reminder?", "id": "Pesan Pengingat Akan Sesuatu." },
  { "en": "Apa Arti Kata To Do List?", "id": "Daftar Pekerjaan Yang Harus Dilakukan." },
  { "en": "Apa Arti Kata Multitasking?", "id": "Mengerjakan Banyak Tugas Sekaligus." },
  { "en": "Apa Arti Kata Brokie?", "id": "Sebutan Untuk Orang Yang Sedang Bangkrut." },
  { "en": "Apa Arti Kata PM (Personal Message)?", "id": "Mengirim Pesan Secara Pribadi." },
  { "en": "Apa Arti Kata DM (Direct Message)?", "id": "Pesan Pribadi (Sama Seperti PM)." },
  { "en": "Apa Arti Kata BM (Banyak Mau)?", "id": "Sedang Sangat Menginginkan Sesuatu." },
  { "en": "Apa Arti Kata Old Money?", "id": "Kekayaan Yang Diwariskan Turun-Temurun." },
  { "en": "Apa Arti Kata New Money?", "id": "Kekayaan Hasil Usaha Generasi Pertama." },
  { "en": "Apa Arti Kata Sugar Daddy?", "id": "Pria Tua Kaya Membiayai Pasangan Muda." },
  { "en": "Apa Arti Kata Sugar Baby?", "id": "Pasangan Muda Dibiayai Sugar Daddy." },
  { "en": "Apa Arti Kata Gold Digger?", "id": "Orang Yang Mengejar Harta Pasangannya." },
  { "en": "Apa Arti Kata Social Climber?", "id": "Orang Yang Suka Panjat Sosial." },
  { "en": "Apa Arti Kata Drama Queen?", "id": "Orang Yang Suka Melebih-Lebihkan Masalah." },
  { "en": "Apa Arti Kata Attention Seeker?", "id": "Orang Yang Selalu Mencari Perhatian." },
  { "en": "Apa Arti Kata Humble Bragging?", "id": "Pamer Terselubung, Pura-Pura Merendah." },
  { "en": "Apa Arti Kata Benchmark?", "id": "Standar Tolok Ukur Pembanding Kinerja." },
  { "en": "Apa Arti Kata Insight?", "id": "Wawasan Baru Yang Mendalam." },
  { "en": "Apa Arti Kata Stakeholder?", "id": "Pihak Yang Berkepentingan Dalam Proyek." },
  { "en": "Apa Arti Kata Pitching?", "id": "Presentasi Ide Singkat Untuk Meyakinkan." },
  { "en": "Apa Arti Kata Deck?", "id": "Materi Presentasi, Biasanya Format Slide." },
  { "en": "Apa Arti Kata Case Study?", "id": "Studi Kasus, Analisis Masalah Spesifik." },
  { "en": "Apa Arti Kata Data Driven?", "id": "Mengambil Keputusan Berdasarkan Data." },
  { "en": "Apa Arti Kata User Experience (UX)?", "id": "Pengalaman Pengguna Saat Memakai Produk." },
  { "en": "Apa Arti Kata User Interface (UI)?", "id": "Tampilan Visual Produk Digital." },
  { "en": "Apa Arti Kata Glitch?", "id": "Kesalahan Teknis Kecil Pada Sistem." },
  { "en": "Apa Arti Kata Bug?", "id": "Kesalahan Serius Pada Kode Program." },
  { "en": "Apa Arti Kata Debugging?", "id": "Proses Mencari Dan Memperbaiki Bug." },
  { "en": "Apa Arti Kata Bandwidth?", "id": "Kapasitas Maksimal Transfer Data Internet." },
  { "en": "Apa Arti Kata Server Down?", "id": "Server Pusat Mati, Tidak Bisa Diakses." },
  { "en": "Apa Arti Kata Maintenance?", "id": "Perawatan Sistem, Seringkali Offline." },
  { "en": "Apa Arti Kata Backup?", "id": "Mencadangkan Data Agar Tidak Hilang." },
  { "en": "Apa Arti Kata Restore?", "id": "Mengembalikan Data Cadangan Seperti Semula." },
  { "en": "Apa Arti Kata Default?", "id": "Pengaturan Awal Atau Bawaan Pabrik." },
  { "en": "Apa Arti Kata Template?", "id": "Pola Atau Cetakan Dasar Desain." },
  { "en": "Apa Arti Kata Manual?", "id": "Dilakukan Dengan Tangan, Tidak Otomatis." },
  { "en": "Apa Arti Kata Otomatis?", "id": "Bekerja Sendiri Tanpa Bantuan Manusia." },
  { "en": "Apa Arti Kata Premis?", "id": "Asumsi Dasar Atau Awal Sebuah Argumen." },
  { "en": "Apa Arti Kata Hipotesis?", "id": "Dugaan Awal Yang Perlu Diuji Kebenarannya." },
  { "en": "Apa Arti Kata Paradoks?", "id": "Pernyataan Yang Terlihat Bertentangan Logika." },
  { "en": "Apa Arti Kata Anomali?", "id": "Keanehan, Penyimpangan Dari Keadaan Normal." },
  { "en": "Apa Arti Kata Dilema?", "id": "Situasi Sulit, Memilih Di Antara Dua Pilihan Buruk." },
  { "en": "Apa Arti Kata Dinamis?", "id": "Selalu Bergerak, Penuh Perubahan." },
  { "en": "Apa Arti Kata Statis?", "id": "Diam, Tetap, Tidak Berubah Keadaannya." },
  { "en": "Apa Arti Kata Pro Rata?", "id": "Dihitung Secara Proporsional Sesuai Porsi." },
  { "en": "Apa Arti Kata Lump Sum?", "id": "Pembayaran Penuh Dalam Satu Kali." },
  { "en": "Apa Arti Kata Reimburse?", "id": "Penggantian Uang Yang Sudah Ditalangi." },
  { "en": "Apa Arti Kata Invoice?", "id": "Tagihan Resmi Rincian Pembayaran." },
  { "en": "Apa Arti Kata Quotation?", "id": "Penawaran Harga Resmi Sebelum Transaksi." },
  { "en": "Apa Arti Kata PO (Purchase Order)?", "id": "Dokumen Resmi Bukti Pemesanan Barang." },
  { "en": "Apa Arti Kata DO (Delivery Order)?", "id": "Dokumen Resmi Bukti Pengiriman Barang." },
  { "en": "Apa Arti Kata Resi?", "id": "Nomor Bukti Pengiriman Dari Ekspedisi." },
  { "en": "Apa Arti Kata Ekspedisi?", "id": "Perusahaan Jasa Pengiriman Barang." },
  { "en": "Apa Arti Kata Kurir?", "id": "Orang Yang Bertugas Mengantar Paket." },
  { "en": "Apa Arti Kata Internal?", "id": "Sesuatu Yang Berasal Dari Dalam." },
  { "en": "Apa Arti Kata Eksternal?", "id": "Sesuatu Yang Berasal Dari Luar." },
  { "en": "Apa Arti Kata Mayoritas?", "id": "Bagian Terbesar, Jumlah Terbanyak." },
  { "en": "Apa Arti Kata Minoritas?", "id": "Bagian Terkecil, Jumlah Paling Sedikit." },
  { "en": "Apa Arti Kata Prioritas?", "id": "Sesuatu Yang Harus Diutamakan." },
  { "en": "Apa Arti Kata Sekunder?", "id": "Kebutuhan Peringkat Kedua (Setelah Primer)." },
  { "en": "Apa Arti Kata Tersier?", "id": "Kebutuhan Peringkat Ketiga (Mewah)." },
  { "en": "Apa Arti Kata Wir?", "id": "Panggilan Gaul 'Jawir' (Merujuk Orang Jawa)." },
  { "en": "Apa Arti Kata Slay?", "id": "Tampil Sangat Keren Dan Memukau." },
  { "en": "Apa Arti Kata Pookie?", "id": "Panggilan Sayang Yang Sangat Manja." },
  { "en": "Apa Arti Kata Grinding (Game)?", "id": "Mengulang Aksi Untuk Menaikkan Level." },
  { "en": "Apa Arti Kata Farming (Game)?", "id": "Mengulang Aksi Untuk Mengumpulkan Item." },
  { "en": "Apa Arti Kata Cooldown (Game)?", "id": "Waktu Jeda Penggunaan Kekuatan (Skill)." },
  { "en": "Apa Arti Kata AoE (Area Of Effect)?", "id": "Serangan Yang Mengenai Satu Area Luas." },
  { "en": "Apa Arti Kata Buff (Game)?", "id": "Peningkatan Status Atau Kekuatan Sementara." },
  { "en": "Apa Arti Kata Nerf (Game)?", "id": "Penurunan Status Atau Kekuatan (Dilemahkan)." },
  { "en": "Apa Arti Kata Lag?", "id": "Keterlambatan Respon Karena Jaringan Lambat." },
  { "en": "Apa Arti Kata Frame Drop?", "id": "Gambar Patah-Patah Karena Grafis Berat." },
  { "en": "Apa Arti Kata Cheater?", "id": "Pemain Yang Menggunakan Program Curang." },
  { "en": "Apa Arti Kata Hode?", "id": "Pria Yang Berpura-Pura Menjadi Wanita." },
  { "en": "Apa Arti Kata Ngabuburit?", "id": "Kegiatan Menunggu Waktu Berbuka Puasa." },
  { "en": "Apa Arti Kata Bukber?", "id": "Singkatan Buka Puasa Bersama." },
  { "en": "Apa Arti Kata Takjil?", "id": "Makanan Ringan Pembuka Puasa." },
  { "en": "Apa Arti Kata Mudik?", "id": "Pulang Ke Kampung Halaman Saat Lebaran." },
  { "en": "Apa Arti Kata Halal Bihalal?", "id": "Acara Silaturahmi Saling Memaafkan." },
  { "en": "Apa Arti Singkatan THR (Tunjangan Hari Raya)?", "id": "Bonus Uang Yang Diterima Saat Lebaran." },
  { "en": "Apa Arti Kata Arus Balik?", "id": "Perjalanan Kembali Ke Kota Perantauan." },
  { "en": "Apa Arti Kata Parsel?", "id": "Bingkisan Hadiah Yang Dikirimkan." },
  { "en": "Apa Arti Kata Kating?", "id": "Singkatan Kakak Tingkat Di Kampus." },
  { "en": "Apa Arti Kata Dedek Gemes?", "id": "Panggilan Untuk Junior Yang Menarik." },
  { "en": "Apa Arti Kata Ospek?", "id": "Orientasi Pengenalan Kampus Mahasiswa Baru." },
  { "en": "Apa Arti Singkatan UKM (Unit Kegiatan Mahasiswa)?", "id": "Ekstrakurikuler Versi Mahasiswa Di Kampus." },
  { "en": "Apa Arti Singkatan BEM (Badan Eksekutif Mahasiswa)?", "id": "Organisasi Eksekutif Tertinggi Mahasiswa." },
  { "en": "Apa Arti Singkatan DPM (Dewan Perwakilan Mahasiswa)?", "id": "Organisasi Legislatif Pengawas BEM." },
  { "en": "Apa Arti Singkatan HIMA (Himpunan Mahasiswa)?", "id": "Organisasi Mahasiswa Tingkat Jurusan." },
  { "en": "Apa Arti Singkatan KRS (Kartu Rencana Studi)?", "id": "Lembar Pengambilan Mata Kuliah Per Semester." },
  { "en": "Apa Arti Singkatan KHS (Kartu Hasil Studi)?", "id": "Lembar Laporan Nilai Mata Kuliah." },
  { "en": "Apa Arti Singkatan IPK (Indeks Prestasi Kumulatif)?", "id": "Nilai Rata-Rata Gabungan Selama Kuliah." },
  { "en": "Apa Arti Singkatan SKS (Satuan Kredit Semester)?", "id": "Satuan Bobot Mata Kuliah." },
  { "en": "Apa Arti Kata Cumlaude?", "id": "Gelar Kehormatan Lulus Dengan Nilai Tinggi." },
  { "en": "Apa Arti Kata Skripsi?", "id": "Tugas Akhir Ilmiah Mahasiswa S1." },
  { "en": "Apa Arti Kata Tesis?", "id": "Tugas Akhir Ilmiah Mahasiswa S2." },
  { "en": "Apa Arti Kata Disertasi?", "id": "Tugas Akhir Ilmiah Mahasiswa S3." },
  { "en": "Apa Arti Kata Dosen?", "id": "Pengajar Atau Guru Di Perguruan Tinggi." },
  { "en": "Apa Arti Kata Rektor?", "id": "Pemimpin Tertinggi Perguruan Tinggi." },
  { "en": "Apa Arti Kata Dekan?", "id": "Pemimpin Tertinggi Fakultas Di Universitas." },
  { "en": "Apa Arti Kata Matkul?", "id": "Singkatan Dari Mata Kuliah." },
  { "en": "Apa Arti Kata Maba?", "id": "Singkatan Dari Mahasiswa Baru." },
  { "en": "Apa Arti Kata KKN (Kuliah Kerja Nyata)?", "id": "Pengabdian Mahasiswa Di Masyarakat Desa." },
  { "en": "Apa Arti Kata Magang?", "id": "Praktik Kerja Di Perusahaan (Internship)." },
  { "en": "Apa Arti Kata Wisuda?", "id": "Upacara Pelantikan Kelulusan Mahasiswa." },
  { "en": "Apa Arti Kata Alumni?", "id": "Orang Yang Telah Lulus Dari Sekolah." },
  { "en": "Apa Arti Kata Reuni?", "id": "Pertemuan Kembali Para Alumni Sekolah." },
  { "en": "Apa Arti Kata BPJS (Badan Penyelenggara Jaminan Sosial)?", "id": "Lembaga Asuransi Kesehatan Negara." },
  { "en": "Apa Arti Singkatan SIM (Surat Izin Mengemudi)?", "id": "Bukti Izin Resmi Mengendarai Kendaraan." },
  { "en": "Apa Arti Singkatan STNK (Surat Tanda Nomor Kendaraan)?", "id": "Bukti Kepemilikan Sah Kendaraan Bermotor." },
  { "en": "Apa Arti Singkatan BPKB (Buku Pemilik Kendaraan Bermotor)?", "id": "Buku Bukti Sah Kepemilikan Kendaraan." },
  { "en": "Apa Arti Singkatan KTP (Kartu Tanda Penduduk)?", "id": "Kartu Identitas Resmi Warga Negara." },
  { "en": "Apa Arti Singkatan KK (Kartu Keluarga)?", "id": "Kartu Identitas Data Anggota Keluarga." },
  { "en": "Apa Arti Singkatan NPWP (Nomor Pokok Wajib Pajak)?", "id": "Nomor Identitas Untuk Keperluan Pajak." },
  { "en": "Apa Arti Kata Tilang?", "id": "Bukti Pelanggaran Lalu Lintas." },
  { "en": "Apa Arti Kata Razia?", "id": "Pemeriksaan Mendadak Oleh Pihak Berwajib." },
  { "en": "Apa Arti Kata Macet?", "id": "Kondisi Lalu Lintas Yang Terhenti." },
  { "en": "Apa Arti Kata Banjir?", "id": "Genangan Air Yang Meluap." },
  { "en": "Apa Arti Kata Gempa?", "id": "Getaran Pada Permukaan Bumi." },
  { "en": "Apa Arti Kata Tsunami?", "id": "Gelombang Laut Besar Akibat Gempa." },
  { "en": "Apa Arti Kata Viral?", "id": "Sesuatu Yang Menyebar Cepat Di Internet." },
  { "en": "Apa Arti Kata Trending?", "id": "Topik Yang Sedang Hangat Dibicarakan." },
  { "en": "Apa Arti Kata Konten?", "id": "Isi Atau Materi (Video, Tulisan, Gambar)." },
  { "en": "Apa Arti Kata Kreator?", "id": "Orang Yang Membuat Konten." },
  { "en": "Apa Arti Kata Subscriber?", "id": "Pelanggan Saluran (Contoh: Youtube)." },
  { "en": "Apa Arti Kata Follower?", "id": "Pengikut Akun Media Sosial." },
  { "en": "Apa Arti Kata Username?", "id": "Nama Akun Pengguna Di Platform." },
  { "en": "Apa Arti Kata Password?", "id": "Kata Sandi Rahasia Untuk Masuk Akun." },
  { "en": "Apa Arti Kata Login?", "id": "Proses Masuk Ke Dalam Akun." },
  { "en": "Apa Arti Kata Logout?", "id": "Proses Keluar Dari Akun." },
  { "en": "Apa Arti Kata Signup?", "id": "Proses Mendaftar Akun Baru." },
  { "en": "Apa Arti Kata Error?", "id": "Terjadi Kesalahan Pada Sistem." },
  { "en": "Apa Arti Kata Loading?", "id": "Proses Memuat Data, Menunggu." },
  { "en": "Apa Arti Kata Buffering?", "id": "Proses Memuat Video (Streaming)." },
  { "en": "Apa Arti Kata Streaming?", "id": "Menonton Konten Langsung Via Internet." },
  { "en": "Apa Arti Kata Browsing?", "id": "Aktivitas Menjelajahi Situs Web." },
  { "en": "Apa Arti Kata Browser?", "id": "Aplikasi Untuk Membuka Situs Web." },
  { "en": "Apa Arti Kata Link?", "id": "Tautan, Alamat Situs Web." },
  { "en": "Apa Arti Kata URL (Uniform Resource Locator)?", "id": "Alamat Lengkap Sebuah Halaman Web." },
  { "en": "Apa Arti Kata Online?", "id": "Terhubung Dengan Jaringan Internet." },
  { "en": "Apa Arti Kata Offline?", "id": "Tidak Terhubung Dengan Jaringan Internet." },
  { "en": "Apa Arti Kata Wi-Fi?", "id": "Jaringan Nirkabel Untuk Koneksi Internet." },
  { "en": "Apa Arti Kata Kuota?", "id": "Batas Maksimal Penggunaan Data Internet." },
  { "en": "Apa Arti Kata Provider?", "id": "Perusahaan Penyedia Layanan Internet." },
  { "en": "Apa Arti Kata Sim Card?", "id": "Kartu Identitas Pelanggan Seluler." },
  { "en": "Apa Arti Kata Pulsa?", "id": "Saldo Untuk Layanan Telepon Seluler." },
  { "en": "Apa Arti Kata Smartphone?", "id": "Telepon Pintar Dengan Fitur Canggih." },
  { "en": "Apa Arti Kata Gadget?", "id": "Perangkat Elektronik Kecil, Gawai." },
  { "en": "Apa Arti Agenda (Rapat)?", "id": "Daftar Topik Yang Akan Dibahas." },
  { "en": "Apa Arti Singkatan MoM (Minutes Of Meeting)?", "id": "Catatan Resmi Hasil Rapat." },
  { "en": "Apa Arti Kata Proposal?", "id": "Dokumen Rencana Pengajuan Proyek." },
  { "en": "Apa Arti Kata Tender?", "id": "Proses Pengadaan Barang Atau Jasa." },
  { "en": "Apa Arti Kata Vendor?", "id": "Pihak Pemasok Barang Atau Jasa." },
  { "en": "Apa Arti Kata Klien?", "id": "Pihak Yang Menggunakan Jasa." },
  { "en": "Apa Arti Kata Approval?", "id": "Persetujuan Resmi, ACC." },
  { "en": "Apa Arti Kata Kualitatif?", "id": "Penelitian Berbasis Deskripsi." },
  { "en": "Apa Arti Kata Kuantitatif?", "id": "Penelitian Berbasis Angka." },
  { "en": "Apa Arti Kata Riset?", "id": "Penelitian Mendalam Terhadap Sesuatu." },
  { "en": "Apa Arti Kata Survei?", "id": "Pengumpulan Data Dari Responden." },
  { "en": "Apa Arti Kata Responden?", "id": "Orang Yang Menjawab Survei." },
  { "en": "Apa Arti Kata Demografi?", "id": "Data Kependudukan (Usia, Kelamin)." },
  { "en": "Apa Arti Kata Psikografi?", "id": "Data Gaya Hidup Atau Kepribadian." },
  { "en": "Apa Arti Kata Branding?", "id": "Proses Membangun Citra Merek." },
  { "en": "Apa Arti Kata Marketing?", "id": "Aktivitas Pemasaran Produk." },
  { "en": "Apa Arti Kata Sales?", "id": "Aktivitas Penjualan Produk." },
  { "en": "Apa Arti Kata Target Audiens?", "id": "Sasaran Spesifik Konsumen." },
  { "en": "Apa Arti Kata Engagement?", "id": "Tingkat Interaksi Audiens." },
  { "en": "Apa Arti Kata Reach?", "id": "Jumlah Akun Unik Yang Melihat Konten." },
  { "en": "Apa Arti Kata Impressions?", "id": "Total Berapa Kali Konten Dilihat." },
  { "en": "Apa Arti Singkatan CTR (Click-Through Rate)?", "id": "Persentase Klik Pada Iklan." },
  { "en": "Apa Arti Singkatan SEO (Search Engine Optimization)?", "id": "Optimasi Agar Muncul Di Google." },
  { "en": "Apa Arti Singkatan SEM (Search Engine Marketing)?", "id": "Pemasaran Mesin Pencari Berbayar." },
  { "en": "Apa Arti Kata Algoritma?", "id": "Sistem Kurasi Konten Di Medsos." },
  { "en": "Apa Arti Kata Backlink?", "id": "Tautan Dari Situs Lain Ke Situs Kita." },
  { "en": "Apa Arti Kata Keyword?", "id": "Kata Kunci Yang Dicari Pengguna." },
  { "en": "Apa Arti Kata Traffic?", "id": "Jumlah Kunjungan Ke Situs Web." },
  { "en": "Apa Arti Kata Konversi?", "id": "Perubahan Pengunjung Menjadi Pembeli." },
  { "en": "Apa Arti Singkatan ROI (Return On Investment)?", "id": "Laba Atas Biaya Investasi." },
  { "en": "Apa Arti Kata Budget?", "id": "Anggaran Dana Yang Disiapkan." },
  { "en": "Apa Arti Kata Estimasi?", "id": "Perkiraan Waktu Atau Biaya." },
  { "en": "Apa Arti Kata Timeline?", "id": "Garis Waktu Pengerjaan Proyek." },
  { "en": "Apa Arti Kata Project Manager?", "id": "Orang Yang Mengelola Proyek." },
  { "en": "Apa Arti Kata Desain Grafis?", "id": "Proses Komunikasi Visual." },
  { "en": "Apa Arti Kata Tipografi?", "id": "Seni Menata Huruf Agar Mudah Dibaca." },
  { "en": "Apa Arti Kata Palet Warna?", "id": "Kombinasi Warna Yang Digunakan." },
  { "en": "Apa Arti Kata Mockup?", "id": "Visualisasi Desain Pada Produk Jadi." },
  { "en": "Apa Arti Kata Prototipe?", "id": "Model Awal Produk Untuk Uji Coba." },
  { "en": "Apa Arti Kata Wireframe?", "id": "Kerangka Dasar Tampilan Aplikasi." },
  { "en": "Apa Arti Kata Beta Version?", "id": "Versi Uji Coba Sebelum Rilis." },
  { "en": "Apa Arti Kata Rilis?", "id": "Peluncuran Resmi Produk Ke Publik." },
  { "en": "Apa Arti Kata Patch?", "id": "Perbaikan Kecil Untuk Bug Software." },
  { "en": "Apa Arti Singkatan API (Application Programming Interface)?", "id": "Jembatan Penghubung Antar Aplikasi." },
  { "en": "Apa Arti Kata Front-End?", "id": "Bagian Tampilan Depan Situs Web." },
  { "en": "Apa Arti Kata Back-End?", "id": "Bagian Dapur (Server, Database) Situs Web." },
  { "en": "Apa Arti Kata Full-Stack?", "id": "Menguasai Front-End Dan Back-End." },
  { "en": "Apa Arti Kata Database?", "id": "Tempat Kumpulan Data Disimpan." },
  { "en": "Apa Arti Kata Cloud Computing?", "id": "Penyimpanan Data Berbasis Internet." },
  { "en": "Apa Arti Kata Domain?", "id": "Alamat Unik Sebuah Situs Web." },
  { "en": "Apa Arti Kata Hosting?", "id": "Jasa Penyimpanan Data Situs Web." },
  { "en": "Apa Arti Singkatan SSL (Secure Sockets Layer)?", "id": "Protokol Keamanan (Https)." },
  { "en": "Apa Arti Kata E-commerce?", "id": "Perdagangan Jual Beli Online." },
  { "en": "Apa Arti Kata Marketplace?", "id": "Platform Jual Beli (Contoh: Shopee)." },
  { "en": "Apa Arti Kata Payment Gateway?", "id": "Pihak Ketiga Pemroses Pembayaran." },
  { "en": "Apa Arti Kata E-wallet?", "id": "Dompet Digital (Contoh: GoPay)." },
  { "en": "Apa Arti Singkatan QRIS (Quick Response Code Indonesian Standard)?", "id": "Standar Kode QR Pembayaran Indonesia." },
  { "en": "Apa Arti Kata Top Up?", "id": "Proses Pengisian Ulang Saldo." },
  { "en": "Apa Arti Kata Saldo?", "id": "Jumlah Uang Tersisa Di Rekening." },
  { "en": "Apa Arti Kata Mutasi?", "id": "Catatan Riwayat Transaksi Rekening." },
  { "en": "Apa Arti Kata Rekening?", "id": "Akun Keuangan Di Bank." },
  { "en": "Apa Arti Kata Transfer?", "id": "Proses Memindahkan Uang Antar Rekening." },
  { "en": "Apa Arti Singkatan ATM (Automated Teller Machine)?", "id": "Mesin Anjungan Tunai Mandiri." },
  { "en": "Apa Arti Kata Debit?", "id": "Transaksi Menggunakan Uang Di Rekening." },
  { "en": "Apa Arti Kata Kredit?", "id": "Transaksi Menggunakan Uang Pinjaman." },
  { "en": "Apa Arti Kata Bunga (Bank)?", "id": "Imbal Jasa Atas Simpanan Atau Pinjaman." },
  { "en": "Apa Arti Kata Inflasi?", "id": "Kenaikan Harga Barang Secara Umum." },
  { "en": "Apa Arti Kata Deflasi?", "id": "Penurunan Harga Barang Secara Umum." },
  { "en": "Apa Arti Kata Resesi?", "id": "Kondisi Ekonomi Yang Menurun Drastis." },
  { "en": "Apa Arti Kata Saham?", "id": "Bukti Kepemilikan Suatu Perusahaan." },
  { "en": "Apa Arti Kata Reksadana?", "id": "Wadah Investasi Kolektif." },
  { "en": "Apa Arti Kata Investasi?", "id": "Menanam Modal Untuk Mendapat Untung." },
  { "en": "Apa Arti Kata Dividen?", "id": "Bagi Hasil Keuntungan Saham." },
  { "en": "Apa Arti Kata Portofolio?", "id": "Kumpulan Aset Investasi Yang Dimiliki." },
  { "en": "Apa Arti Kata Profit?", "id": "Keuntungan Bersih, Cuan." },
  { "en": "Apa Arti Kata Loss?", "id": "Kerugian Dalam Investasi." },
  { "en": "Apa Arti Kata Cut Loss?", "id": "Menjual Aset Rugi Mencegah Kerugian." },
  { "en": "Apa Arti Kata Take Profit?", "id": "Menjual Aset Saat Sudah Untung." },
  { "en": "Apa Arti Kata Analisis Teknikal?", "id": "Analisis Harga Berdasarkan Grafik." },
  { "en": "Apa Arti Kata Analisis Fundamental?", "id": "Analisis Berdasarkan Kinerja Perusahaan." },
  { "en": "Apa Arti Kata Sekuritas?", "id": "Perusahaan Perantara Jual Beli Saham." },
  { "en": "Apa Arti Singkatan IPO (Initial Public Offering)?", "id": "Penawaran Saham Perdana Ke Publik." },
  { "en": "Apa Arti Kata Startup?", "id": "Perusahaan Rintisan Berbasis Teknologi." },
  { "en": "Apa Arti Kata Unicorn?", "id": "Startup Dengan Valuasi Di Atas 1 Miliar USD." },
  { "en": "Apa Arti Kata Decacorn?", "id": "Startup Dengan Valuasi Di Atas 10 Miliar USD." },
  { "en": "Apa Arti Kata Venture Capital?", "id": "Modal Ventura, Pemodal Startup." },
  { "en": "Apa Arti Kata Angel Investor?", "id": "Investor Individu Yang Mendanai Startup." },
  { "en": "Apa Arti Singkatan CEO (Chief Executive Officer)?", "id": "Direktur Utama Perusahaan." },
  { "en": "Apa Arti Singkatan CFO (Chief Financial Officer)?", "id": "Direktur Keuangan Perusahaan." },
  { "en": "Apa Arti Singkatan CTO (Chief Technology Officer)?", "id": "Direktur Teknologi Perusahaan." },
  { "en": "Apa Arti Singkatan COO (Chief Operating Officer)?", "id": "Direktur Operasional Perusahaan." },
  { "en": "Apa Arti Singkatan HRD (Human Resource Development)?", "id": "Bagian Sumber Daya Manusia." },
  { "en": "Apa Arti Kata Rekrutmen?", "id": "Proses Pencarian Karyawan Baru." },
  { "en": "Apa Arti Kata Interview?", "id": "Proses Wawancara Kerja." },
  { "en": "Apa Arti Singkatan CV (Curriculum Vitae)?", "id": "Daftar Riwayat Hidup." },
  { "en": "Apa Arti Kata Resume?", "id": "Ringkasan Pengalaman Kerja." },
  { "en": "Apa Arti Kata Cover Letter?", "id": "Surat Pengantar Lamaran Kerja." },
  { "en": "Apa Arti Kata Probation?", "id": "Masa Percobaan Karyawan Baru." },
  { "en": "Apa Arti Kata Benefit?", "id": "Tunjangan Tambahan Selain Gaji." },
  { "en": "Apa Arti Kata Kompensasi?", "id": "Total Gaji Dan Benefit Yang Diterima." },
  { "en": "Apa Arti Kata Job Desk?", "id": "Rincian Tugas Dan Tanggung Jawab." },
  { "en": "Apa Arti Kata Freelancer?", "id": "Pekerja Lepas Yang Tidak Terikat." },
  { "en": "Apa Arti Kata Part-Time?", "id": "Bekerja Paruh Waktu, Jam Terbatas." },
  { "en": "Apa Arti Kata Full-Time?", "id": "Bekerja Penuh Waktu, Jam Standar." },
  { "en": "Apa Arti Kata Remote (Kerja)?", "id": "Bekerja Dari Lokasi Mana Saja." },
  { "en": "Apa Arti Kata On-Site?", "id": "Bekerja Langsung Di Lokasi Kantor." },
  { "en": "Apa Arti Kata Resign?", "id": "Proses Mengundurkan Diri Dari Pekerjaan." },
  { "en": "Apa Arti Kata PHK (Pemutusan Hubungan Kerja)?", "id": "Pemberhentian Karyawan Oleh Perusahaan." },
  { "en": "Apa Arti Kata Layoff?", "id": "PHK Massal Karena Alasan Perusahaan." },
  { "en": "Apa Arti Kata Referensi (Kerja)?", "id": "Rekomendasi Dari Atasan Sebelumnya." },
  { "en": "Apa Arti Kata Networking?", "id": "Membangun Jaringan Relasi Profesional." },
  { "en": "Apa Arti Kata Mentor?", "id": "Pembimbing Profesional Yang Berpengalaman." },
  { "en": "Apa Arti Kata Mentee?", "id": "Orang Yang Dibimbing Oleh Mentor." },
  { "en": "Apa Arti Kata Skill?", "id": "Keterampilan Atau Keahlian Spesifik." },
  { "en": "Apa Arti Kata Hard Skill?", "id": "Keterampilan Teknis (Contoh: Koding)." },
  { "en": "Apa Arti Kata Soft Skill?", "id": "Keterampilan Non-Teknis (Contoh: Komunikasi)." },
  { "en": "Apa Arti Kata Leadership?", "id": "Keterampilan Kepemimpinan." },
  { "en": "Apa Arti Kata Teamwork?", "id": "Kemampuan Bekerja Sama Dalam Tim." },
  { "en": "Apa Arti Kata Problem Solving?", "id": "Kemampuan Memecahkan Masalah." },
  { "en": "Apa Arti Kata Adaptif?", "id": "Kemampuan Mudah Beradaptasi." },
  { "en": "Apa Arti Kata Inovatif?", "id": "Kemampuan Menciptakan Sesuatu Yang Baru." },
  { "en": "Apa Arti Kata Kreatif?", "id": "Kemampuan Berpikir Di Luar Kotak." },
  { "en": "Apa Arti Kata Analitis?", "id": "Kemampuan Menganalisis Data." },
  { "en": "Apa Arti Kata Deadline Fighter?", "id": "Orang Yang Suka Mengerjakan Dekat Deadline." },
  { "en": "Apa Arti Kata Hustle Culture?", "id": "Budaya Gila Kerja Keras." },
  { "en": "Apa Arti Kata Sandwich Generation?", "id": "Generasi Penanggung Biaya Orang Tua Dan Anak." },
  { "en": "Apa Arti Kata Boomer?", "id": "Generasi Yang Lahir (1946-1964)." },
  { "en": "Apa Arti Kata Gen X?", "id": "Generasi Yang Lahir (1965-1980)." },
  { "en": "Apa Arti Kata Milenial (Gen Y)?", "id": "Generasi Yang Lahir (1981-1996)." },
  { "en": "Apa Arti Kata Gen Z?", "id": "Generasi Yang Lahir (1997-2012)." },
  { "en": "Apa Arti Kata Alpha?", "id": "Generasi Yang Lahir (2013-Sekarang)." },
  { "en": "Apa Arti Kata Sigma Male?", "id": "Pria Mandiri, Serigala Penyendiri." },
  { "en": "Apa Arti Kata Alpha Male?", "id": "Pria Dominan, Pemimpin Kelompok." },
  { "en": "Apa Arti Kata Beta Male?", "id": "Pria Pengikut, Bukan Pemimpin." },
  { "en": "Apa Arti Kata Backpacker?", "id": "Wisatawan Hemat Dengan Ransel." },
  { "en": "Apa Arti Kata Itinerary?", "id": "Rencana Detail Perjalanan." },
  { "en": "Apa Arti Kata Check-In (Hotel)?", "id": "Proses Konfirmasi Kedatangan Di Hotel." },
  { "en": "Apa Arti Kata Check-Out (Hotel)?", "id": "Proses Konfirmasi Keberangkatan Dari Hotel." },
  { "en": "Apa Arti Kata Transit?", "id": "Berhenti Sementara Dalam Perjalanan." },
  { "en": "Apa Arti Kata Delay (Penerbangan)?", "id": "Penundaan Jadwal Keberangkatan." },
  { "en": "Apa Arti Kata Boarding?", "id": "Proses Memasuki Pesawat Terbang." },
  { "en": "Apa Arti Kata Take-Off?", "id": "Proses Pesawat Mulai Lepas Landas." },
  { "en": "Apa Arti Kata Landing?", "id": "Proses Pesawat Mendarat Di Tujuan." },
  { "en": "Apa Arti Kata Paspor?", "id": "Buku Identitas Resmi Untuk Luar Negeri." },
  { "en": "Apa Arti Kata Visa?", "id": "Izin Masuk Resmi Ke Negara Lain." },
  { "en": "Apa Arti Kata Imigrasi?", "id": "Pemeriksaan Dokumen Masuk Negara." },
  { "en": "Apa Arti Kata Bea Cukai?", "id": "Pemeriksaan Barang Bawaan Lintas Negara." },
  { "en": "Apa Arti Kata Defisit Kalori?", "id": "Makan Lebih Sedikit Kalori Dari Kebutuhan." },
  { "en": "Apa Arti Kata Surplus Kalori?", "id": "Makan Lebih Banyak Kalori Dari Kebutuhan." },
  { "en": "Apa Arti Kata Protein?", "id": "Zat Gizi Pembangun Otot." },
  { "en": "Apa Arti Kata Karbohidrat?", "id": "Sumber Energi Utama Tubuh." },
  { "en": "Apa Arti Kata Lemak?", "id": "Zat Gizi Cadangan Energi." },
  { "en": "Apa Arti Kata Kardio?", "id": "Latihan Untuk Kesehatan Jantung." },
  { "en": "Apa Arti Kata Latihan Beban?", "id": "Latihan Untuk Membangun Kekuatan Otot." },
  { "en": "Apa Arti Kata Pemanasan?", "id": "Gerakan Awal Sebelum Olahraga Inti." },
  { "en": "Apa Arti Kata Pendinginan?", "id": "Gerakan Akhir Setelah Olahraga Inti." },
  { "en": "Apa Arti Kata Tumis?", "id": "Memasak Cepat Dengan Sedikit Minyak." },
  { "en": "Apa Arti Kata Rebus?", "id": "Memasak Dalam Air Mendidih." },
  { "en": "Apa Arti Kata Kukus?", "id": "Memasak Dengan Uap Air Panas." },
  { "en": "Apa Arti Kata Goreng?", "id": "Memasak Dalam Minyak Panas." },
  { "en": "Apa Arti Kata Panggang?", "id": "Memasak Dengan Panas Kering (Oven)." },
  { "en": "Apa Arti Kata Bakar?", "id": "Memasak Langsung Di Atas Api." },
  { "en": "Apa Arti Kata Marinasi?", "id": "Proses Merendam Bahan Makanan Dalam Bumbu." },
  { "en": "Apa Arti Kata Bumbu?", "id": "Rempah-Rempah Penambah Rasa." },
  { "en": "Apa Arti Kata Resep?", "id": "Panduan Langkah-Langkah Memasak." },
  { "en": "Apa Arti Kata Abstrak (Karya Tulis)?", "id": "Ringkasan Singkat Isi Karya Tulis." },
  { "en": "Apa Arti Kata Jurnal (Akademik)?", "id": "Publikasi Ilmiah Berkala." },
  { "en": "Apa Arti Kata Sitasi?", "id": "Mengutip Sumber Referensi Tulisan." },
  { "en": "Apa Arti Kata Daftar Pustaka?", "id": "Daftar Semua Sumber Referensi." },
  { "en": "Apa Arti Kata Plagiarisme?", "id": "Mencuri Karya Tulis Orang Lain." },
  { "en": "Apa Arti Kata Revisi UU?", "id": "Proses Mengubah Undang-Undang." },
  { "en": "Apa Arti Singkatan RUU (Rancangan Undang-Undang)?", "id": "Draf Undang-Undang Yang Belum Sah." },
  { "en": "Apa Arti Singkatan KUHP (Kitab Undang-Undang Hukum Pidana)?", "id": "Aturan Hukum Pidana Di Indonesia." },
  { "en": "Apa Arti Kata Amendemen?", "id": "Perubahan Resmi Dokumen (Contoh: UUD)." },
  { "en": "Apa Arti Kata Oposisi?", "id": "Pihak Yang Menentang Pemerintah." },
  { "en": "Apa Arti Kata Koalisi?", "id": "Gabungan Partai Pendukung Pemerintah." },
  { "en": "Apa Arti Kata Legislatif?", "id": "Lembaga Pembuat Undang-Undang (DPR)." },
  { "en": "Apa Arti Kata Eksekutif?", "id": "Lembaga Pelaksana Undang-Undang (Presiden)." },
  { "en": "Apa Arti Kata Yudikatif?", "id": "Lembaga Pengawas Hukum (MA, MK)." },
  { "en": "Apa Arti Kata Demokrasi?", "id": "Pemerintahan Dari Rakyat, Oleh Rakyat." },
  { "en": "Apa Arti Kata Monarki?", "id": "Pemerintahan Yang Dipimpin Raja." },
  { "en": "Apa Arti Kata Konstitusi?", "id": "Hukum Dasar Tertulis Negara, UUD." },
  { "en": "Apa Arti Kata Diplomasi?", "id": "Hubungan Antar Negara." },
  { "en": "Apa Arti Kata Embargo?", "id": "Larangan Perdagangan Dengan Negara Lain." },
  { "en": "Apa Arti Kata Sanksi?", "id": "Hukuman Terhadap Negara Pelanggar." },
  { "en": "Apa Arti Kata Veto?", "id": "Hak Membatalkan Keputusan." },
  { "en": "Apa Arti Kata Pemilu (Pemilihan Umum)?", "id": "Proses Memilih Wakil Rakyat." },
  { "en": "Apa Arti Kata Pilkada (Pemilihan Kepala Daerah)?", "id": "Proses Memilih Gubernur, Bupati." },
  { "en": "Apa Arti Kata Kampanye?", "id": "Masa Promosi Calon Pemilu." },
  { "en": "Apa Arti Kata TPS (Tempat Pemungutan Suara)?", "id": "Lokasi Tempat Mencoblos." },
  { "en": "Apa Arti Kata Quick Count?", "id": "Hitung Cepat Hasil Pemilu (Sampel)." },
  { "en": "Apa Arti Kata Real Count?", "id": "Hitung Resmi Hasil Pemilu (KPU)." },
  { "en": "Apa Arti Kata Golput?", "id": "Golongan Putih, Tidak Memilih." },
  { "en": "Apa Arti Kata Materai?", "id": "Segel Pajak Tanda Sah Dokumen." },
  { "en": "Apa Arti Kata Notaris?", "id": "Pejabat Umum Pembuat Akta Otentik." },
  { "en": "Apa Arti Kata Akta Otentik?", "id": "Dokumen Resmi Dibuat Notaris." },
  { "en": "Apa Arti Singkatan MOU (Memorandum Of Understanding)?", "id": "Nota Kesepahaman Awal (Belum Mengikat)." },
  { "en": "Apa Arti Singkatan PKS (Perjanjian Kerja Sama)?", "id": "Kontrak Kerja Sama Resmi (Mengikat)." },
  { "en": "Apa Arti Kata Komuter?", "id": "Orang Yang Bepergian Jauh Ke Tempat Kerja." },
  { "en": "Apa Arti Kata Urban?", "id": "Berkaitan Dengan Wilayah Perkotaan." },
  { "en": "Apa Arti Kata Rural?", "id": "Berkaitan Dengan Wilayah Pedesaan." },
  { "en": "Apa Arti Kata Subsidi?", "id": "Bantuan Pemerintah Agar Harga Murah." },
  { "en": "Apa Arti Kata Matic (Mobil)?", "id": "Mobil Bertransmisi Otomatis." },
  { "en": "Apa Arti Kata Manual (Mobil)?", "id": "Mobil Bertransmisi Gigi Manual." },
  { "en": "Apa Arti Kata Oli (Mesin)?", "id": "Cairan Pelumas Mesin Kendaraan." },
  { "en": "Apa Arti Kata Servis (Motor)?", "id": "Perawatan Rutin Kendaraan." },
  { "en": "Apa Arti Kata Spare Part?", "id": "Suku Cadang Pengganti Komponen." },
  { "en": "Apa Arti Kata Aki (Kendaraan)?", "id": "Baterai Sumber Listrik Kendaraan." },
  { "en": "Apa Arti Kata Busi?", "id": "Komponen Pemantik Api Mesin Bensin." },
  { "en": "Apa Arti Kata Velg?", "id": "Kerangka Besi Roda Kendaraan." },
  { "en": "Apa Arti Kata Knalpot?", "id": "Saluran Pembuangan Gas Mesin." },
  { "en": "Apa Arti Kata Spion?", "id": "Kaca Untuk Melihat Bagian Belakang." },
  { "en": "Apa Arti Kata Ambis?", "id": "Singkatan Ambisius, Sangat Giat Belajar." },
  { "en": "Apa Arti Kata Pewe?", "id": "Singkatan 'Posisi Wenak', Sangat Nyaman." },
  { "en": "Apa Arti Kata Bonyok (Kondisi)?", "id": "Luka Lebam Atau Memar Parah." },
  { "en": "Apa Arti Kata Cringe Parah?", "id": "Sangat Menjijikkan Atau Memalukan." },
  { "en": "Apa Arti Kata No Debat?", "id": "Benar, Tidak Bisa Dibantah Lagi." },
  { "en": "Apa Arti Kata The Real...?", "id": "Benar-Benar Asli (Contoh: The Real Sultan)." },
  { "en": "Apa Arti Kata Spek Bidadari?", "id": "Spesifikasi Sempurna Seperti Bidadari." },
  { "en": "Apa Arti Kata Elit?", "id": "Sesuatu Yang Mewah, Kelas Atas." },
  { "en": "Apa Arti Kata Sulit?", "id": "Susah, Tidak Mudah Dilakukan." },
  { "en": "Apa Arti Kata Gampang?", "id": "Mudah, Tidak Sulit Dilakukan." },
  { "en": "Apa Arti Kata Hoodie?", "id": "Jaket Dengan Penutup Kepala." },
  { "en": "Apa Arti Kata Sneakers?", "id": "Sepatu Kets Untuk Gaya Santai." },
  { "en": "Apa Arti Singkatan OOTD (Outfit Of The Day)?", "id": "Pakaian Yang Dikenakan Hari Ini." },
  { "en": "Apa Arti Kata Outfit?", "id": "Satu Set Pakaian, Gaya Pakaian." },
  { "en": "Apa Arti Kata Vintage?", "id": "Gaya Lama Asli (20 Tahun Lebih)." },
  { "en": "Apa Arti Kata Retro?", "id": "Gaya Baru Yang Meniru Gaya Lama." },
  { "en": "Apa Arti Kata Denim?", "id": "Bahan Kain Kuat (Bahan Jeans)." },
  { "en": "Apa Arti Kata Jeans?", "id": "Celana Yang Terbuat Dari Denim." },
  { "en": "Apa Arti Kata Oversize?", "id": "Pakaian Berukuran Sangat Besar." },
  { "en": "Apa Arti Kata Fitting Room?", "id": "Ruang Ganti Baju Di Toko." },
  { "en": "Apa Arti Kata Ratio (Medsos)?", "id": "Komentar Yang Lebih Banyak Suka." },
  { "en": "Apa Arti Kata Mid?", "id": "Sesuatu Yang Biasa Saja, Standar." },
  { "en": "Apa Arti Kata Cap (Slang)?", "id": "Bohong, Omong Kosong." },
  { "en": "Apa Arti Kata No Cap?", "id": "Jujur, Tidak Bohong, Serius." },
  { "en": "Apa Arti Kata Let Him Cook?", "id": "Biar Dia Lakukan, Jangan Diganggu." },
  { "en": "Apa Arti Kata Cooked (Slang)?", "id": "Selesai, Gagal Total, Habis." },
  { "en": "Apa Arti Kata Touch Grass?", "id": "Keluar Rumah, Jangan Main Game Terus." },
  { "en": "Apa Arti Kata Gyatt?", "id": "Ekspresi Untuk Bokong Besar." },
  { "en": "Apa Arti Kata Skibidi?", "id": "Istilah Populer Dari Video Aneh." },
  { "en": "Apa Arti Kata Rizzler?", "id": "Orang Yang Pandai Merayu (Rizz)." },
  { "en": "Apa Arti Kata Agile (Kerja)?", "id": "Metode Kerja Cepat Dan Fleksibel." },
  { "en": "Apa Arti Kata Scrum (Kerja)?", "id": "Kerangka Kerja Metode Agile." },
  { "en": "Apa Arti Kata Sprint (Kerja)?", "id": "Siklus Kerja Singkat Dalam Scrum." },
  { "en": "Apa Arti Singkatan OKR (Objective Key Results)?", "id": "Metode Penetapan Target Perusahaan." },
  { "en": "Apa Arti Kata Confidential?", "id": "Sesuatu Yang Bersifat Sangat Rahasia." },
  { "en": "Apa Arti Singkatan NDA (Non-Disclosure Agreement)?", "id": "Perjanjian Jaga Kerahasiaan." },
  { "en": "Apa Arti Singkatan B2B (Business To Business)?", "id": "Bisnis Yang Menjual Ke Bisnis Lain." },
  { "en": "Apa Arti Singkatan B2C (Business To Consumer)?", "id": "Bisnis Yang Menjual Ke Konsumen." },
  { "en": "Apa Arti Kata Brain Dump?", "id": "Mencurahkan Semua Ide Tanpa Filter." },
  { "en": "Apa Arti Kata Notulensi?", "id": "Catatan Rinci Jalannya Rapat." },
  { "en": "Apa Arti Kata Arabica (Kopi)?", "id": "Jenis Kopi Rasa Asam Dan Manis." },
  { "en": "Apa Arti Kata Robusta (Kopi)?", "id": "Jenis Kopi Rasa Pahit Dan Kuat." },
  { "en": "Apa Arti Kata V60 (Kopi)?", "id": "Metode Seduh Kopi Manual (Pour Over)." },
  { "en": "Apa Arti Kata Espresso?", "id": "Ekstrak Biji Kopi Pekat." },
  { "en": "Apa Arti Kata Latte?", "id": "Espresso Dicampur Susu Steam." },
  { "en": "Apa Arti Kata Americano?", "id": "Espresso Dicampur Air Panas." },
  { "en": "Apa Arti Singkatan ISO (Kamera)?", "id": "Tingkat Sensitivitas Sensor Cahaya." },
  { "en": "Apa Arti Kata Aperture (Kamera)?", "id": "Bukaan Lensa Mengatur Cahaya Masuk." },
  { "en": "Apa Arti Kata Shutter Speed?", "id": "Kecepatan Jendela Lensa Membuka." },
  { "en": "Apa Arti Kata Bokeh?", "id": "Efek Latar Belakang Kabur (Blur)." },
  { "en": "Apa Arti Penyakit GERD?", "id": "Penyakit Asam Lambung Naik." },
  { "en": "Apa Arti Penyakit Autoimun?", "id": "Sistem Imun Menyerang Tubuh Sendiri." },
  { "en": "Apa Arti Kata Kolesterol?", "id": "Lemak Jahat Dalam Darah." },
  { "en": "Apa Arti Penyakit Diabetes?", "id": "Penyakit Kelebihan Gula Darah." },
  { "en": "Apa Arti Kata Resep (Dokter)?", "id": "Catatan Dokter Untuk Penebusan Obat." },
  { "en": "Apa Arti Obat Generik?", "id": "Obat Tanpa Merek Dagang." },
  { "en": "Apa Arti Obat Paten?", "id": "Obat Dengan Merek Dagang Asli." },
  { "en": "Apa Arti Penyakit Hipertensi?", "id": "Penyakit Tekanan Darah Tinggi." },
  { "en": "Apa Arti Kata Vaksin?", "id": "Zat Pembangun Kekebalan Tubuh." },
  { "en": "Apa Arti Kata Imunisasi?", "id": "Proses Pemberian Vaksin Tubuh." },
  { "en": "Apa Arti Kata Gentrifikasi?", "id": "Perubahan Area Miskin Menjadi Mahal." },
  { "en": "Apa Arti Singkatan MoM (Minutes Of Meeting)?", "id": "Catatan Resmi Hasil Rapat." },
  { "en": "Apa Arti Kata Proposal?", "id": "Dokumen Rencana Pengajuan Proyek." },
  { "en": "Apa Arti Kata Tender?", "id": "Proses Pengadaan Barang Atau Jasa." },
  { "en": "Apa Arti Kata Vendor?", "id": "Pihak Pemasok Barang Atau Jasa." },
  { "en": "Apa Arti Kata Klien?", "id": "Pihak Yang Menggunakan Jasa." },
  { "en": "Apa Arti Kata Approval?", "id": "Persetujuan Resmi, ACC." },
  { "en": "Apa Arti Kata Kualitatif?", "id": "Penelitian Berbasis Deskripsi." },
  { "en": "Apa Arti Kata Kuantitatif?", "id": "Penelitian Berbasis Angka." },
  { "en": "Apa Arti Kata Riset?", "id": "Penelitian Mendalam Terhadap Sesuatu." },
  { "en": "Apa Arti Kata Survei?", "id": "Pengumpulan Data Dari Responden." },
  { "en": "Apa Arti Kata Responden?", "id": "Orang Yang Menjawab Survei." },
  { "en": "Apa Arti Kata Demografi?", "id": "Data Kependudukan (Usia, Kelamin)." },
  { "en": "Apa Arti Kata Psikografi?", "id": "Data Gaya Hidup Atau Kepribadian." },
  { "en": "Apa Arti Kata Branding?", "id": "Proses Membangun Citra Merek." },
  { "en": "Apa Arti Kata Marketing?", "id": "Aktivitas Pemasaran Produk." },
  { "en": "Apa Arti Kata Sales?", "id": "Aktivitas Penjualan Produk." },
  { "en": "Apa Arti Kata Target Audiens?", "id": "Sasaran Spesifik Konsumen." },
  { "en": "Apa Arti Kata Engagement (Medsos)?", "id": "Tingkat Interaksi Audiens Pada Konten." },
  { "en": "Apa Arti Kata Reach (Medsos)?", "id": "Jumlah Akun Unik Yang Melihat Konten." },
  { "en": "Apa Arti Kata Impressions (Medsos)?", "id": "Total Frekuensi Konten Dilihat." },
  { "en": "Apa Arti Singkatan CTR (Click-Through Rate)?", "id": "Persentase Jumlah Klik Pada Iklan." },
  { "en": "Apa Arti Singkatan SEO (Search Engine Optimization)?", "id": "Optimasi Mesin Pencari (Contoh: Google)." },
  { "en": "Apa Arti Singkatan SEM (Search Engine Marketing)?", "id": "Pemasaran Mesin Pencari Berbayar." },
  { "en": "Apa Arti Kata Algoritma (Medsos)?", "id": "Sistem Yang Mengatur Tampilan Konten." },
  { "en": "Apa Arti Kata Backlink?", "id": "Tautan Dari Situs Lain Ke Situs Kita." },
  { "en": "Apa Arti Kata Keyword?", "id": "Kata Kunci Yang Dicari Pengguna." },
  { "en": "Apa Arti Kata Traffic (Website)?", "id": "Jumlah Kunjungan Ke Situs Web." },
  { "en": "Apa Arti Kata Konversi (Bisnis)?", "id": "Perubahan Pengunjung Menjadi Pembeli." },
  { "en": "Apa Arti Singkatan ROI (Return On Investment)?", "id": "Tingkat Pengembalian Modal Investasi." },
  { "en": "Apa Arti Kata Budget?", "id": "Anggaran Dana Yang Telah Disiapkan." },
  { "en": "Apa Arti Kata Estimasi?", "id": "Perkiraan Waktu Atau Biaya Proyek." },
  { "en": "Apa Arti Kata Timeline?", "id": "Garis Waktu Pengerjaan Sebuah Proyek." },
  { "en": "Apa Arti Kata Tipografi?", "id": "Seni Menata Huruf Agar Mudah Dibaca." },
  { "en": "Apa Arti Kata Palet Warna?", "id": "Kombinasi Warna Yang Digunakan Desain." },
  { "en": "Apa Arti Kata Mockup?", "id": "Visualisasi Desain Pada Produk Jadi." },
  { "en": "Apa Arti Kata Prototipe?", "id": "Model Awal Produk Untuk Uji Coba." },
  { "en": "Apa Arti Kata Wireframe?", "id": "Kerangka Dasar Tampilan Aplikasi." },
  { "en": "Apa Arti Kata Beta Version?", "id": "Versi Uji Coba Produk Sebelum Rilis." },
  { "en": "Apa Arti Kata Rilis?", "id": "Peluncuran Resmi Produk Ke Publik." },
  { "en": "Apa Arti Kata Patch (Game)?", "id": "Perbaikan Kecil Untuk Bug Software." },
  { "en": "Apa Arti Singkatan API (Application Programming Interface)?", "id": "Jembatan Penghubung Antar Aplikasi." },
  { "en": "Apa Arti Kata Front-End?", "id": "Bagian Tampilan Depan Sebuah Situs Web." },
  { "en": "Apa Arti Kata Back-End?", "id": "Bagian Dapur (Server, Database) Situs Web." },
  { "en": "Apa Arti Kata Full-Stack?", "id": "Programmer Penguasa Front-End Dan Back-End." },
  { "en": "Apa Arti Kata Database?", "id": "Basis Data, Kumpulan Data Tersimpan." },
  { "en": "Apa Arti Kata Cloud Computing?", "id": "Penyimpanan Data Berbasis Internet (Awan)." },
  { "en": "Apa Arti Kata Domain?", "id": "Alamat Unik Sebuah Situs Web." },
  { "en": "Apa Arti Kata Hosting?", "id": "Jasa Penyimpanan Data Situs Web." },
  { "en": "Apa Arti Singkatan SSL (Secure Sockets Layer)?", "id": "Protokol Keamanan (Penanda Https)." },
  { "en": "Apa Arti Kata E-commerce?", "id": "Aktivitas Perdagangan Jual Beli Online." },
  { "en": "Apa Arti Kata Marketplace?", "id": "Platform Jual Beli (Contoh: Tokopedia)." },
  { "en": "Apa Arti Kata Payment Gateway?", "id": "Pihak Pemroses Transaksi Pembayaran." },
  { "en": "Apa Arti Kata E-wallet?", "id": "Dompet Digital (Contoh: Dana, OVO)." },
  { "en": "Apa Arti Singkatan QRIS (Quick Response Code Indonesian Standard)?", "id": "Standar Kode QR Pembayaran Indonesia." },
  { "en": "Apa Arti Kata Top Up?", "id": "Proses Pengisian Ulang Saldo Digital." },
  { "en": "Apa Arti Kata Mutasi (Bank)?", "id": "Catatan Riwayat Transaksi Rekening." },
  { "en": "Apa Arti Kata Transfer (Bank)?", "id": "Proses Memindahkan Uang Antar Rekening." },
  { "en": "Apa Arti Singkatan ATM (Automated Teller Machine)?", "id": "Mesin Anjungan Tunai Mandiri." },
  { "en": "Apa Arti Kata Debit?", "id": "Transaksi Menggunakan Uang Di Rekening." },
  { "en": "Apa Arti Kata Kredit?", "id": "Transaksi Menggunakan Uang Pinjaman Bank." },
  { "en": "Apa Arti Kata Bunga (Bank)?", "id": "Imbal Jasa Simpanan Atau Pinjaman." },
  { "en": "Apa Arti Kata Inflasi?", "id": "Kenaikan Harga Barang Secara Umum." },
  { "en": "Apa Arti Kata Deflasi?", "id": "Penurunan Harga Barang Secara Umum." },
  { "en": "Apa Arti Kata Resesi?", "id": "Kondisi Ekonomi Yang Menurun Drastis." },
  { "en": "Apa Arti Kata Saham?", "id": "Bukti Kepemilikan Suatu Perusahaan." },
  { "en": "Apa Arti Kata Reksadana?", "id": "Wadah Investasi Kolektif Dikelola Manajer." },
  { "en": "Apa Arti Kata Investasi?", "id": "Menanam Modal Untuk Mendapat Untung." },
  { "en": "Apa Arti Kata Dividen?", "id": "Bagi Hasil Keuntungan Pemegang Saham." },
  { "en": "Apa Arti Kata Portofolio (Investasi)?", "id": "Kumpulan Aset Investasi Yang Dimiliki." },
  { "en": "Apa Arti Kata Profit?", "id": "Keuntungan Bersih, Laba, Cuan." },
  { "en": "Apa Arti Kata Loss (Investasi)?", "id": "Kerugian Dalam Investasi, Boncos." },
  { "en": "Apa Arti Kata Cut Loss?", "id": "Menjual Aset Rugi Mencegah Kerugian." },
  { "en": "Apa Arti Kata Take Profit?", "id": "Menjual Aset Saat Sudah Untung." },
  { "en": "Apa Arti Analisis Teknikal?", "id": "Analisis Harga Saham Berdasarkan Grafik." },
  { "en": "Apa Arti Analisis Fundamental?", "id": "Analisis Berdasarkan Kinerja Perusahaan." },
  { "en": "Apa Arti Singkatan IPO (Initial Public Offering)?", "id": "Penawaran Saham Perdana Ke Publik." },
  { "en": "Apa Arti Kata Startup?", "id": "Perusahaan Rintisan Berbasis Teknologi." },
  { "en": "Apa Arti Kata Unicorn?", "id": "Startup Dengan Valuasi 1 Miliar USD." },
  { "en": "Apa Arti Kata Decacorn?", "id": "Startup Dengan Valuasi 10 Miliar USD." },
  { "en": "Apa Arti Venture Capital?", "id": "Perusahaan Modal Ventura (Pemodal Startup)." },
  { "en": "Apa Arti Angel Investor?", "id": "Investor Individu Yang Mendanai Startup." },
  { "en": "Apa Arti Singkatan CEO (Chief Executive Officer)?", "id": "Jabatan Direktur Utama Perusahaan." },
  { "en": "Apa Arti Singkatan CFO (Chief Financial Officer)?", "id": "Jabatan Direktur Keuangan Perusahaan." },
  { "en": "Apa Arti Singkatan CTO (Chief Technology Officer)?", "id": "Jabatan Direktur Teknologi Perusahaan." },
  { "en": "Apa Arti Singkatan COO (Chief Operating Officer)?", "id": "Jabatan Direktur Operasional Perusahaan." },
  { "en": "Apa Arti Singkatan HRD (Human Resource Development)?", "id": "Departemen Sumber Daya Manusia." },
  { "en": "Apa Arti Kata Rekrutmen?", "id": "Proses Pencarian Dan Penerimaan Karyawan." },
  { "en": "Apa Arti Kata Interview?", "id": "Proses Wawancara Calon Karyawan." },
  { "en": "Apa Arti Singkatan CV (Curriculum Vitae)?", "id": "Dokumen Daftar Riwayat Hidup." },
  { "en": "Apa Arti Kata Resume?", "id": "Ringkasan Pengalaman Kerja Profesional." },
  { "en": "Apa Arti Kata Cover Letter?", "id": "Surat Pengantar Lamaran Kerja." },
  { "en": "Apa Arti Kata Probation?", "id": "Masa Percobaan Karyawan Baru." },
  { "en": "Apa Arti Kata Benefit (Kerja)?", "id": "Tunjangan Tambahan Di Luar Gaji Pokok." },
  { "en": "Apa Arti Kata Job Desk?", "id": "Rincian Tugas Dan Tanggung Jawab." },
  { "en": "Apa Arti Kata Remote (Kerja)?", "id": "Bekerja Dari Lokasi Mana Saja." },
  { "en": "Apa Arti Kata On-Site?", "id": "Bekerja Langsung Di Lokasi Kantor." },
  { "en": "Apa Arti Kata PHK (Pemutusan Hubungan Kerja)?", "id": "Pemberhentian Karyawan Oleh Perusahaan." },
  { "en": "Apa Arti Kata Layoff?", "id": "PHK Massal Karena Kondisi Perusahaan." },
  { "en": "Apa Arti Kata Networking (Karier)?", "id": "Membangun Jaringan Relasi Profesional." },
  { "en": "Apa Arti Kata Hard Skill?", "id": "Keterampilan Teknis (Contoh: Koding, Desain)." },
  { "en": "Apa Arti Kata Soft Skill?", "id": "Keterampilan Non-Teknis (Contoh: Komunikasi)." },
  { "en": "Apa Arti Kata Leadership?", "id": "Keterampilan Memimpin Sebuah Tim." },
  { "en": "Apa Arti Kata Teamwork?", "id": "Kemampuan Bekerja Sama Dalam Tim." },
  { "en": "Apa Arti Kata Problem Solving?", "id": "Kemampuan Mencari Solusi Masalah." },
  { "en": "Apa Arti Kata Adaptif?", "id": "Sifat Mudah Menyesuaikan Diri." },
  { "en": "Apa Arti Kata Inovatif?", "id": "Sifat Menciptakan Sesuatu Yang Baru." },
  { "en": "Apa Arti Kata Kreatif?", "id": "Sifat Menghasilkan Ide-Ide Baru." },
  { "en": "Apa Arti Kata Analitis?", "id": "Sifat Menganalisis Data Secara Mendalam." },
  { "en": "Apa Arti Hustle Culture?", "id": "Budaya Gila Kerja Keras Berlebihan." },
  { "en": "Apa Arti Sandwich Generation?", "id": "Generasi Penanggung Biaya Orang Tua Dan Anak." },
  { "en": "Apa Arti Kata Sigma Male?", "id": "Pria Mandiri, Serigala Penyendiri." },
  { "en": "Apa Arti Kata Alpha Male?", "id": "Pria Dominan, Pemimpin Kelompok." },
  { "en": "Apa Arti Kata Backpacker?", "id": "Wisatawan Hemat Dengan Ransel." },
  { "en": "Apa Arti Kata Itinerary?", "id": "Rencana Detail Jadwal Perjalanan." },
  { "en": "Apa Arti Kata Transit (Penerbangan)?", "id": "Berhenti Sementara Sebelum Melanjutkan." },
  { "en": "Apa Arti Kata Delay (Penerbangan)?", "id": "Penundaan Jadwal Keberangkatan Pesawat." },
  { "en": "Apa Arti Kata Boarding?", "id": "Proses Memasuki Pesawat Terbang." },
  { "en": "Apa Arti Kata Take-Off?", "id": "Proses Pesawat Mulai Lepas Landas." },
  { "en": "Apa Arti Kata Landing?", "id": "Proses Pesawat Mendarat Di Tujuan." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
