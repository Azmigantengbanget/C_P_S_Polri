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


  { "en": "Kepanjangan dari CCTV?", "id": "Closed-Circuit Television." },
  { "en": "Fungsi utama CCTV?", "id": "Mengawasi dan merekam suatu area." },
  { "en": "Komponen utama sistem CCTV?", "id": "Kamera, perekam, monitor, dan kabel." },
  { "en": "Alat perekam CCTV analog?", "id": "DVR (Digital Video Recorder)." },
  { "en": "Alat perekam CCTV IP?", "id": "NVR (Network Video Recorder)." },
  { "en": "Beda utama DVR dan NVR?", "id": "DVR proses, NVR rekam sinyal jadi." },
  { "en": "Tipe kamera bentuk kubah?", "id": "Kamera Dome." },
  { "en": "Tipe kamera bentuk peluru?", "id": "Kamera Bullet." },
  { "en": "Kepanjangan kamera PTZ?", "id": "Pan-Tilt-Zoom." },
  { "en": "Kemampuan utama kamera PTZ?", "id": "Bisa bergerak dan melakukan zoom." },
  { "en": "Beda kamera analog dan IP?", "id": "Kualitas gambar dan media transmisinya." },
  { "en": "Mana yang kualitasnya lebih tinggi?", "id": "Kamera IP (Internet Protocol)." },
  { "en": "Apa itu resolusi video?", "id": "Ukuran ketajaman detail gambar." },
  { "en": "Resolusi 1080p juga disebut?", "id": "Full HD." },
  { "en": "Resolusi 4K juga disebut?", "id": "Ultra HD (UHD)." },
  { "en": "Definisi sinyal analog?", "id": "Sinyal kontinu berbentuk gelombang." },
  { "en": "Definisi sinyal digital?", "id": "Sinyal diskrit berbentuk biner." },
  { "en": "Proses ubah analog ke digital?", "id": "Konversi ADC." },
  { "en": "Proses ubah digital ke analog?", "id": "Konversi DAC." },
  { "en": "Apa itu 'sampling rate'?", "id": "Frekuensi pengambilan sampel sinyal." },
  { "en": "Apa itu 'frame rate'?", "id": "Jumlah gambar per detik (fps)." },
  { "en": "Manfaat 'frame rate' tinggi?", "id": "Merekam gerakan cepat lebih jelas." },
  { "en": "Apa itu kompresi video?", "id": "Proses memperkecil ukuran file video." },
  { "en": "Tujuan utama kompresi video?", "id": "Menghemat ruang penyimpanan dan bandwidth." },
  { "en": "Standar kompresi video umum?", "id": "H.264 dan H.265 (HEVC)." },
  { "en": "Mana kompresi lebih efisien?", "id": "H.265 (HEVC)." },
  { "en": "Media penyimpanan utama NVR?", "id": "Hard Disk Drive (HDD)." },
  { "en": "Apa itu 'retention time'?", "id": "Lama waktu rekaman video disimpan." },
  { "en": "Penyimpanan rekaman di internet?", "id": "Cloud storage." },
  { "en": "Keuntungan penyimpanan 'cloud'?", "id": "Aman dari kerusakan/pencurian fisik." },
  { "en": "Fungsi CCTV untuk Polri?", "id": "Alat bukti, monitor, cegah kejahatan." },
  { "en": "CCTV untuk tilang elektronik?", "id": "Sistem ETLE." },
  { "en": "Pusat monitor CCTV terpusat?", "id": "Command Center." },
  { "en": "Apa itu 'pixel'?", "id": "Titik terkecil dalam gambar digital." },
  { "en": "Apa itu 'bitrate' video?", "id": "Jumlah data video per detik." },
  { "en": "Pengaruh 'bitrate' pada kualitas?", "id": "Bitrate tinggi, kualitas lebih baik." },
  { "en": "Definisi lensa 'varifocal'?", "id": "Lensa dengan zoom yang bisa diatur." },
  { "en": "Definisi lensa 'fixed'?", "id": "Lensa dengan sudut pandang tetap." },
  { "en": "Fungsi lampu 'infrared' (IR)?", "id": "Untuk penglihatan di kondisi gelap." },
  { "en": "Apa itu 'WDR'?", "id": "Wide Dynamic Range." },
  { "en": "Fungsi utama 'WDR'?", "id": "Menyeimbangkan area terang dan gelap." },
  { "en": "Definisi 'noise' pada video?", "id": "Bintik-bintik yang ganggu gambar." },
  { "en": "Penyebab utama 'noise'?", "id": "Kondisi pencahayaan yang sangat minim." },
  { "en": "Apa itu 'DNR'?", "id": "Digital Noise Reduction." },
  { "en": "Fungsi utama 'DNR'?", "id": "Mengurangi bintik 'noise' pada video." },
  { "en": "Kepanjangan dari PoE?", "id": "Power over Ethernet." },
  { "en": "Fungsi utama PoE?", "id": "Daya dan data lewat satu kabel." },
  { "en": "Jenis kabel kamera analog?", "id": "Kabel Coaxial." },
  { "en": "Jenis kabel kamera IP?", "id": "Kabel Ethernet (UTP)." },
  { "en": "Satuan intensitas cahaya?", "id": "Lux." },
  { "en": "Kamera 'low-light' butuh nilai?", "id": "Nilai lux yang sangat rendah." },
  { "en": "Apa itu 'field of view'?", "id": "Sudut pandang tangkapan kamera." },
  { "en": "Apa itu 'focal length'?", "id": "Menentukan sudut pandang dan zoom." },
  { "en": "Sensor pengubah cahaya ke listrik?", "id": "Image sensor (CMOS atau CCD)." },
  { "en": "Beda 'progressive' & 'interlaced'?", "id": "Progressive memindai semua baris." },
  { "en": "Mana yang gambarnya lebih stabil?", "id": "Progressive scan." },
  { "en": "Arti 'p' pada 1080p?", "id": "Progressive." },
  { "en": "Apa itu 'video analytics'?", "id": "Analisa video otomatis deteksi kejadian." },
  { "en": "Contoh 'video analytics' dasar?", "id": "Deteksi gerakan (motion detection)." },
  { "en": "Keuntungan 'motion detection'?", "id": "Menghemat ruang penyimpanan secara signifikan." },
  { "en": "Algoritma kompresi video disebut?", "id": "Codec." },
  { "en": "Apa itu 'digital zoom'?", "id": "Pembesaran gambar secara perangkat lunak." },
  { "en": "Apa itu 'optical zoom'?", "id": "Pembesaran gambar menggunakan fisik lensa." },
  { "en": "Mana kualitas zoom lebih baik?", "id": "Optical zoom." },
  { "en": "Fungsi 'auto iris'?", "id": "Otomatis sesuaikan bukaan cahaya." },
  { "en": "Apa itu 'shutter speed'?", "id": "Kecepatan rana menangkap gambar." },
  { "en": "Fungsi 'white balance'?", "id": "Menyesuaikan warna agar tampak natural." },
  { "en": "Kamera tahan perusakan disebut?", "id": "Kamera vandal-proof." },
  { "en": "Kamera tahan cuaca disebut?", "id": "Kamera weatherproof." },
  { "en": "Apa itu 'IP rating'?", "id": "Standar ketahanan terhadap debu dan air." },
  { "en": "Contoh IP rating weatherproof?", "id": "IP66 atau IP67." },
  { "en": "Jeda waktu pada video?", "id": "Latency atau kelambatan." },
  { "en": "Fungsi 'audio input' CCTV?", "id": "Merekam suara dari sekitar kamera." },
  { "en": "Fungsi 'alarm input' CCTV?", "id": "Menerima sinyal dari sensor eksternal." },
  { "en": "Apa itu 'video enhancement'?", "id": "Proses perbaikan kualitas video bukti." },
  { "en": "Contoh proses 'enhancement'?", "id": "Mencerahkan gambar, mempertajam obyek." },
  { "en": "Fungsi 'frame averaging'?", "id": "Mengurangi noise dengan gabungan frame." },
  { "en": "Fungsi 'stabilization'?", "id": "Mengurangi guncangan pada rekaman video." },
  { "en": "Fungsi 'privacy masking'?", "id": "Menutupi area privasi dalam rekaman." },
  { "en": "Riwayat penanganan bukti video?", "id": "Chain of Custody." },
  { "en": "Apa itu 'watermark' digital?", "id": "Tanda tak terlihat pada video." },
  { "en": "Fungsi 'watermark' digital?", "id": "Membuktikan keaslian rekaman video." },
  { "en": "Sinyal video komponen?", "id": "Luminance (kecerahan) dan Chrominance (warna)." },
  { "en": "Apa itu 'aspect ratio'?", "id": "Rasio perbandingan lebar dan tinggi." },
  { "en": "Aspect ratio standar HD?", "id": "16:9." },
  { "en": "Kamera yang bisa melihat panas?", "id": "Kamera thermal." },
  { "en": "Fungsi kamera thermal?", "id": "Deteksi panas tubuh, sumber api." },
  { "en": "Apa itu 'fisheye camera'?", "id": "Kamera dengan sudut pandang 360 derajat." },
  { "en": "Kamera untuk identifikasi plat nomor?", "id": "Kamera ANPR atau LPR." },
  { "en": "Kepanjangan ANPR?", "id": "Automatic Number Plate Recognition." },
  { "en": "Protokol transmisi IP Camera?", "id": "RTSP (Real-Time Streaming Protocol)." },
  { "en": "Apa itu 'multicast'?", "id": "Mengirim streaming ke banyak pengguna." },
  { "en": "Apa itu 'unicast'?", "id": "Mengirim streaming ke satu pengguna." },
  { "en": "Apa itu 'backlight compensation'?", "id": "Mengatasi obyek gelap karena cahaya belakang." },
  { "en": "Kepanjangan BLC?", "id": "Backlight Compensation." },
  { "en": "Fungsi 'face recognition'?", "id": "Mengidentifikasi orang dari fitur wajah." },
  { "en": "Database untuk 'face recognition' Polri?", "id": "Database e-KTP dan catatan kepolisian." },
  { "en": "Apa itu 'object detection'?", "id": "Mendeteksi keberadaan objek tertentu." },
  { "en": "Apa itu 'object tracking'?", "id": "Melacak pergerakan objek terdeteksi." },
  { "en": "Fungsi 'line crossing detection'?", "id": "Alarm saat objek lewati garis virtual." },
  { "en": "Fungsi 'intrusion detection'?", "id": "Alarm saat objek masuki area terlarang." },
  { "en": "Fungsi 'crowd density analysis'?", "id": "Menganalisa tingkat kepadatan kerumunan." },
  { "en": "Fungsi 'behavioral analysis'?", "id": "Deteksi perilaku aneh atau mencurigakan." },
  { "en": "Contoh perilaku aneh?", "id": "Orang berlari, berkelahi, tertinggal barang." },
  { "en": "Apa itu 'image authentication'?", "id": "Memastikan keaslian gambar atau video." },
  { "en": "Apa itu 'tamper detection'?", "id": "Mendeteksi manipulasi pada bukti video." },
  { "en": "Contoh manipulasi video?", "id": "Menghapus frame, mengubah objek." },
  { "en": "Apa itu 'photogrammetry'?", "id": "Pengukuran dari gambar (tinggi, jarak)." },
  { "en": "Apa itu 'gait analysis'?", "id": "Identifikasi orang dari cara berjalannya." },
  { "en": "Apa itu 'video clarification'?", "id": "Proses menjernihkan video bukti." },
  { "en": "Kepanjangan VMS?", "id": "Video Management System." },
  { "en": "Fungsi utama VMS?", "id": "Mengelola banyak sistem CCTV terpusat." },
  { "en": "Kepanjangan PSIM?", "id": "Physical Security Information Management." },
  { "en": "Fungsi utama PSIM?", "id": "Integrasi semua sistem keamanan fisik." },
  { "en": "Contoh integrasi CCTV?", "id": "Dengan kontrol akses dan alarm." },
  { "en": "Apa itu 'bandwidth' CCTV?", "id": "Kebutuhan transfer data streaming video." },
  { "en": "Faktor penentu 'bandwidth'?", "id": "Resolusi, frame rate, dan kompresi." },
  { "en": "Tujuan redundansi N+1?", "id": "Satu perangkat cadangan untuk beberapa perangkat." },
  { "en": "Fungsi RAID pada NVR?", "id": "Melindungi data jika satu HDD rusak." },
  { "en": "Tujuan standar ONVIF?", "id": "Integrasi perangkat CCTV beda merek." },
  { "en": "Kepanjangan ONVIF?", "id": "Open Network Video Interface Forum." },
  { "en": "Ruang warna video digital?", "id": "YUV (Luminance, Chrominance)." },
  { "en": "Ruang warna monitor komputer?", "id": "RGB (Red, Green, Blue)." },
  { "en": "Apa itu 'chroma subsampling'?", "id": "Mengurangi informasi warna untuk kompresi." },
  { "en": "Contoh notasi 'chroma subsampling'?", "id": "4:4:4, 4:2:2, 4:2:0." },
  { "en": "Konsep dasar Transformasi Fourier?", "id": "Memecah sinyal menjadi frekuensi-frekuensi." },
  { "en": "Fungsi 'low-pass filter'?", "id": "Melewatkan frekuensi rendah, meredam tinggi." },
  { "en": "Efek 'low-pass filter' gambar?", "id": "Memperhalus gambar (blurring)." },
  { "en": "Fungsi 'high-pass filter'?", "id": "Melewatkan frekuensi tinggi, meredam rendah." },
  { "en": "Efek 'high-pass filter' gambar?", "id": "Mempertajam tepi objek (sharpening)." },
  { "en": "Apa itu 'edge detection'?", "id": "Mengidentifikasi batas atau tepi objek." },
  { "en": "Algoritma 'edge detection'?", "id": "Sobel, Canny, Prewitt." },
  { "en": "Apa itu 'histogram' gambar?", "id": "Grafik distribusi intensitas piksel." },
  { "en": "Fungsi 'histogram equalization'?", "id": "Meningkatkan kontras gambar secara otomatis." },
  { "en": "Apa itu 'super resolution'?", "id": "Meningkatkan resolusi gambar pakai AI." },
  { "en": "Apa itu 'inpainting' video?", "id": "Mengisi bagian gambar yang hilang." },
  { "en": "Tujuan 'inpainting'?", "id": "Menghilangkan objek atau teks pengganggu." },
  { "en": "Apa itu 'deinterlacing'?", "id": "Mengubah video interlaced menjadi progressive." },
  { "en": "Apa itu 'demosaicing'?", "id": "Membangun gambar berwarna dari data sensor." },
  { "en": "Apa itu 'rolling shutter'?", "id": "Efek distorsi pada objek bergerak." },
  { "en": "Apa itu 'global shutter'?", "id": "Menangkap seluruh gambar secara bersamaan." },
  { "en": "Mana lebih baik untuk forensik?", "id": "Global shutter." },
  { "en": "Apa itu 'lens distortion'?", "id": "Gambar melengkung akibat karakteristik lensa." },
  { "en": "Contoh 'lens distortion'?", "id": "Barrel distortion, pincushion distortion." },
  { "en": "Fungsi 'lens correction'?", "id": "Meluruskan kembali gambar yang melengkung." },
  { "en": "Apa itu 'motion blur'?", "id": "Efek kabur akibat gerakan cepat." },
  { "en": "Cara mengurangi 'motion blur'?", "id": "Gunakan shutter speed yang lebih tinggi." },
  { "en": "Apa itu 'depth of field'?", "id": "Rentang jarak objek yang fokus." },
  { "en": "Apa itu 'bokeh'?", "id": "Efek blur pada latar belakang." },
  { "en": "Apa itu 'macro' mode?", "id": "Mode untuk pemotretan jarak sangat dekat." },
  { "en": "Apa itu 'image segmentation'?", "id": "Mempartisi gambar menjadi beberapa segmen." },
  { "en": "Apa itu 'object recognition'?", "id": "Mengklasifikasikan objek yang terdeteksi." },
  { "en": "Beda 'detection' & 'recognition'?", "id": "Detection menemukan, recognition mengidentifikasi." },
  { "en": "Apa itu 'template matching'?", "id": "Mencari bagian gambar yang cocok." },
  { "en": "Apa itu 'feature extraction'?", "id": "Mengekstrak ciri khas dari gambar." },
  { "en": "Contoh 'feature' pada wajah?", "id": "Jarak mata, bentuk hidung, bibir." },
  { "en": "Apa itu 'deep learning'?", "id": "Sub-bidang AI dengan jaringan saraf." },
  { "en": "Peran 'deep learning' di CCTV?", "id": "Meningkatkan akurasi video analytics." },
  { "en": "Apa itu 'Convolutional Neural Network'?", "id": "Arsitektur AI untuk analisa gambar." },
  { "en": "Kepanjangan CNN?", "id": "Convolutional Neural Network." },
  { "en": "Tantangan 'face recognition'?", "id": "Masker, kacamata, pencahayaan buruk." },
  { "en": "Apa itu 'liveness detection'?", "id": "Memastikan wajah di depan kamera asli." },
  { "en": "Tujuan 'liveness detection'?", "id": "Mencegah spoofing pakai foto/video." },
  { "en": "Apa itu 'license plate recognition'?", "id": "Pengenalan plat nomor kendaraan otomatis." },
  { "en": "Kepanjangan LPR?", "id": "License Plate Recognition." },
  { "en": "Apa itu 'vehicle attribute recognition'?", "id": "Mengenali merek, model, warna mobil." },
  { "en": "Apa itu 'geotagging'?", "id": "Menambahkan data lokasi ke video." },
  { "en": "Fungsi 'geotagging' untuk Polri?", "id": "Memastikan lokasi pasti TKP." },
  { "en": "Apa itu 'time synchronization'?", "id": "Sinkronisasi waktu di semua kamera." },
  { "en": "Pentingnya 'time synchronization'?", "id": "Membangun kronologi kejadian yang akurat." },
  { "en": "Protokol untuk sinkronisasi waktu?", "id": "NTP (Network Time Protocol)." },
  { "en": "Apa itu 'video synopsis'?", "id": "Ringkasan video panjang menjadi pendek." },
  { "en": "Tujuan 'video synopsis'?", "id": "Mempercepat proses review rekaman CCTV." },
  { "en": "Apa itu 'metadata' video?", "id": "Data tentang video (waktu, lokasi)." },
  { "en": "Apa itu 'smart search'?", "id": "Pencarian rekaman berdasarkan event/objek." },
  { "en": "Apa itu 'heat map'?", "id": "Visualisasi area paling banyak aktivitas." },
  { "en": "Fungsi 'heat map' untuk Polri?", "id": "Analisa pola keramaian atau lalu lintas." },
  { "en": "Apa itu 'people counting'?", "id": "Menghitung jumlah orang yang lewat." },
  { "en": "Apa itu 'queue management'?", "id": "Analisa antrian untuk manajemen keramaian." },
  { "en": "Apa itu 'audio analytics'?", "id": "Deteksi suara spesifik (teriakan, tembakan)." },
  { "en": "Apa itu 'video hashing'?", "id": "Membuat sidik jari digital video." },
  { "en": "Fungsi 'video hashing'?", "id": "Memverifikasi keaslian video bukti." },
  { "en": "Apa itu 'photo-response non-uniformity'?", "id": "Ciri khas unik sensor kamera." },
  { "en": "Kepanjangan PRNU?", "id": "Photo-Response Non-Uniformity." },
  { "en": "Fungsi analisa PRNU?", "id": "Menghubungkan video ke kamera spesifik." },
  { "en": "Apa itu 'video demultiplexing'?", "id": "Memisahkan sinyal video gabungan." },
  { "en": "Apa itu 'frame dropping'?", "id": "Kehilangan frame saat transmisi/perekaman." },
  { "en": "Dampak 'frame dropping'?", "id": "Kehilangan momen penting dalam rekaman." },
  { "en": "Apa itu 'keyframe'?", "id": "Frame utuh dalam video terkompresi." },
  { "en": "Dasar hukum CCTV publik?", "id": "UU ITE dan Peraturan terkait." },
  { "en": "Aturan pemusnahan rekaman bukti?", "id": "Sesuai standar manajemen barang bukti." },
  { "en": "Syarat video jadi alat bukti?", "id": "Asli, otentik, dan tidak dimanipulasi." },
  { "en": "Tantangan etis 'face recognition'?", "id": "Potensi bias dan pengawasan massal." },
  { "en": "Hak orang yang terekam?", "id": "Hak atas privasi sesuai aturan." },
  { "en": "Rumus dasar kebutuhan storage?", "id": "Bitrate x waktu x jumlah kamera." },
  { "en": "Rumus dasar kebutuhan bandwidth?", "id": "Bitrate per kamera x jumlah kamera." },
  { "en": "Jaringan ideal untuk CCTV?", "id": "Jaringan terpisah (dedicated network)." },
  { "en": "Tujuan jaringan terpisah?", "id": "Menjaga performa dan keamanan." },
  { "en": "Cara amankan NVR/DVR?", "id": "Ubah password default, update firmware." },
  { "en": "Kenapa enkripsi streaming penting?", "id": "Mencegah penyadapan video di jaringan." },
  { "en": "Tujuan manajemen pengguna VMS?", "id": "Memberi hak akses sesuai peran." },
  { "en": "Contoh peran pengguna VMS?", "id": "Admin, operator, investigator." },
  { "en": "SOP permintaan rekaman CCTV?", "id": "Melalui surat permintaan resmi." },
  { "en": "Kepanjangan BWC?", "id": "Body Worn Camera." },
  { "en": "Fungsi utama BWC?", "id": "Merekam interaksi petugas dengan publik." },
  { "en": "Tujuan BWC bagi Polri?", "id": "Transparansi, akuntabilitas, bukti hukum." },
  { "en": "Prosedur aktivasi BWC?", "id": "Aktifkan sebelum interaksi dengan publik." },
  { "en": "Penyimpanan data BWC?", "id": "Diunggah ke server terpusat aman." },
  { "en": "Prosedur pelabelan bukti video?", "id": "Catat waktu, tanggal, lokasi, kamera." },
  { "en": "Fungsi penyegelan barang bukti?", "id": "Menjamin bukti tidak diakses sembarangan." },
  { "en": "CCTV dalam operasi pengintaian?", "id": "Gunakan kamera tersembunyi atau PTZ." },
  { "en": "Peran CCTV di rekonstruksi?", "id": "Memvalidasi keterangan saksi dan tersangka." },
  { "en": "Fungsi 'color correction'?", "id": "Menyesuaikan keseimbangan warna video." },
  { "en": "Fungsi 'deblurring'?", "id": "Mengurangi efek blur pada gambar." },
  { "en": "Apa itu 'Gaussian noise'?", "id": "Noise acak dengan distribusi normal." },
  { "en": "Apa itu 'salt-and-pepper noise'?", "id": "Noise berupa bintik hitam putih." },
  { "en": "Kepanjangan SNR?", "id": "Signal-to-Noise Ratio." },
  { "en": "Arti SNR tinggi?", "id": "Sinyal kuat dibandingkan dengan noise." },
  { "en": "Apa itu '3D reconstruction'?", "id": "Membangun model 3D dari video." },
  { "en": "Syarat '3D reconstruction'?", "id": "Butuh rekaman dari beberapa sudut." },
  { "en": "Apa itu 'data integrity'?", "id": "Keaslian dan keutuhan data." },
  { "en": "Cara jaga 'data integrity' video?", "id": "Gunakan hashing (SHA-256)." },
  { "en": "Apa itu 'file signature analysis'?", "id": "Memverifikasi tipe file dari headernya." },
  { "en": "Apa itu 'codec analysis'?", "id": "Menganalisa jenis kompresi yang digunakan." },
  { "en": "Apa itu 'forensic video authentication'?", "id": "Proses pembuktian keaslian video." },
  { "en": "Apa itu 'video redaction'?", "id": "Menyunting bagian sensitif dari video." },
  { "en": "Contoh 'redaction'?", "id": "Memblur wajah saksi atau korban." },
  { "en": "Apa itu 'biometric data'?", "id": "Data dari ciri fisik (wajah)." },
  { "en": "Isu privasi 'biometric data'?", "id": "Risiko penyalahgunaan data identitas." },
  { "en": "Pentingnya 'audit trail' VMS?", "id": "Mencatat semua aktivitas pengguna." },
  { "en": "Apa itu 'video loss'?", "id": "Sinyal video dari kamera terputus." },
  { "en": "Penyebab 'video loss'?", "id": "Kabel putus, kamera mati, konektor." },
  { "en": "Apa itu 'network jitter'?", "id": "Variasi waktu tunda pengiriman paket." },
  { "en": "Dampak 'jitter' pada video?", "id": "Gambar patah-patah atau terdistorsi." },
  { "en": "Apa itu 'packet loss'?", "id": "Hilangnya paket data saat transmisi." },
  { "en": "Dampak 'packet loss'?", "id": "Gambar rusak atau hilang sebagian." },
  { "en": "Apa itu 'centralized storage'?", "id": "Penyimpanan data terpusat." },
  { "en": "Apa itu 'decentralized storage'?", "id": "Penyimpanan data di masing-masing NVR." },
  { "en": "Apa itu 'edge storage'?", "id": "Penyimpanan di memori kamera." },
  { "en": "Fungsi 'edge storage'?", "id": "Cadangan jika koneksi ke NVR putus." },
  { "en": "Apa itu 'failover' NVR?", "id": "NVR cadangan mengambil alih otomatis." },
  { "en": "Apa itu 'load balancing' VMS?", "id": "Membagi beban kerja ke beberapa server." },
  { "en": "Apa itu 'image flicker'?", "id": "Gambar berkedip akibat frekuensi listrik." },
  { "en": "Apa itu 'anti-flicker' mode?", "id": "Mengurangi efek kedipan pada video." },
  { "en": "Apa itu 'video wall'?", "id": "Gabungan banyak monitor jadi satu." },
  { "en": "Fungsi 'video wall'?", "id": "Tampilan monitor terpusat di Command Center." },
  { "en": "Apa itu 'joystick controller'?", "id": "Alat untuk mengontrol kamera PTZ." },
  { "en": "Apa itu 'pattern' PTZ?", "id": "Gerakan patroli otomatis kamera PTZ." },
  { "en": "Apa itu 'preset' PTZ?", "id": "Posisi Pan-Tilt-Zoom yang sudah disimpan." },
  { "en": "Apa itu 'tour' PTZ?", "id": "Urutan preset yang dijalankan otomatis." },
  { "en": "Apa itu 'auto tracking'?", "id": "Kamera PTZ otomatis mengikuti objek." },
  { "en": "Tantangan 'auto tracking'?", "id": "Kehilangan target jika terhalang." },
  { "en": "Apa itu 'digital watermark' forensik?", "id": "Menandai video bukti yang sudah dianalisa." },
  { "en": "Apa itu 'case management' VMS?", "id": "Fitur mengelola video bukti per kasus." },
  { "en": "Apa itu 'exporting' video?", "id": "Menyimpan klip video sebagai file." },
  { "en": "Format video untuk bukti?", "id": "Format asli atau format standar." },
  { "en": "Apa itu 'native format'?", "id": "Format asli dari perekam CCTV." },
  { "en": "Pentingnya 'native format'?", "id": "Menjaga metadata dan kualitas asli." },
  { "en": "Apa itu 'video player forensik'?", "id": "Player khusus untuk analisa video." },
  { "en": "Apa itu 'Standard Operating Procedure'?", "id": "Prosedur standar untuk suatu tugas." },
  { "en": "Kepanjangan SOP?", "id": "Standard Operating Procedure." },
  { "en": "SOP untuk 'live monitoring'?", "id": "Prosedur pemantauan CCTV secara langsung." },
  { "en": "Tugas operator 'live monitoring'?", "id": "Mengamati, merespon, melaporkan kejadian." },
  { "en": "Apa itu 'situational awareness'?", "id": "Pemahaman kondisi lingkungan sekitar." },
  { "en": "Peran CCTV tingkatkan 'awareness'?", "id": "Memberi 'mata' di banyak lokasi." },
  { "en": "Apa itu 'common operational picture'?", "id": "Tampilan situasi terpadu untuk semua." },
  { "en": "Apa itu 'image sensor size'?", "id": "Ukuran fisik dari sensor gambar." },
  { "en": "Pengaruh 'sensor size'?", "id": "Sensor besar, performa low-light baik." },
  { "en": "Apa itu 'F-number' atau 'F-stop'?", "id": "Ukuran bukaan diafragma lensa." },
  { "en": "Pengaruh 'F-number' kecil?", "id": "Lebih banyak cahaya masuk, baik low-light." },
  { "en": "Apa itu 'chromatic aberration'?", "id": "Distorsi warna pada tepi objek." },
  { "en": "Apa itu 'vignetting'?", "id": "Efek gambar lebih gelap di sudut." },
  { "en": "Apa itu 'convolution'?", "id": "Operasi matematis untuk filtering gambar." },
  { "en": "Apa itu 'kernel' dalam filtering?", "id": "Matriks kecil untuk operasi konvolusi." },
  { "en": "Apa itu 'median filter'?", "id": "Filter untuk hilangkan 'salt-and-pepper noise'." },
  { "en": "Apa itu 'thresholding'?", "id": "Mengubah gambar grayscale menjadi hitam-putih." },
  { "en": "Apa itu 'morphological operations'?", "id": "Operasi pada bentuk objek gambar." },
  { "en": "Contoh 'morphological operations'?", "id": "Erosion dan Dilation." },
  { "en": "Apa itu 'object permanence'?", "id": "Masalah analitik saat objek terhalang." },
  { "en": "Apa itu 'occlusion'?", "id": "Objek terhalang oleh objek lain." },
  { "en": "Apa itu 'predictive policing'?", "id": "Prediksi kejahatan berbasis analisa data." },
  { "en": "Peran data CCTV di dalamnya?", "id": "Sebagai salah satu sumber data analisa." },
  { "en": "Apa itu 'smart city'?", "id": "Kota yang gunakan teknologi terintegrasi." },
  { "en": "Peran CCTV di 'smart city'?", "id": "Monitor lalu lintas, keamanan, lingkungan." },
  { "en": "Langkah 'hardening' NVR?", "id": "Ubah port, matikan service, update." },
  { "en": "Ancaman botnet Mirai?", "id": "Menyerang perangkat IoT, termasuk CCTV." },
  { "en": "Apa itu enkripsi 'end-to-end'?", "id": "Enkripsi dari kamera hingga monitor." },
  { "en": "Tujuan VLAN untuk CCTV?", "id": "Mengisolasi trafik CCTV dari jaringan lain." },
  { "en": "Pentingnya manajemen 'firmware'?", "id": "Untuk menambal celah keamanan terbaru." },
  { "en": "Kamera 'multi-sensor panoramic'?", "id": "Gabungan beberapa kamera jadi satu." },
  { "en": "Keuntungan kamera 'panoramic'?", "id": "Cakupan area sangat luas." },
  { "en": "Teknologi 'laser night vision'?", "id": "Gunakan laser untuk penglihatan malam." },
  { "en": "Keunggulan 'laser night vision'?", "id": "Jarak pandang malam sangat jauh." },
  { "en": "Apa itu sensor LiDAR?", "id": "Deteksi jarak menggunakan pulsa laser." },
  { "en": "Beda LiDAR dan CCTV?", "id": "LiDAR ukur jarak, CCTV ambil gambar." },
  { "en": "Apa itu 'AI on the edge'?", "id": "Proses analitik AI di kamera." },
  { "en": "Keuntungan 'AI on the edge'?", "id": "Mengurangi beban server dan bandwidth." },
  { "en": "Apa itu 'kalibrasi kamera'?", "id": "Mengoreksi parameter internal lensa kamera." },
  { "en": "Tujuan 'kalibrasi' untuk forensik?", "id": "Akurasi pengukuran (photogrammetry)." },
  { "en": "Apa itu 'tiered storage'?", "id": "Penyimpanan berjenjang sesuai prioritas." },
  { "en": "Contoh 'tiered storage'?", "id": "Data baru di SSD, lama di HDD." },
  { "en": "Kepanjangan VSaaS?", "id": "Video Surveillance as a Service." },
  { "en": "Model bisnis VSaaS?", "id": "Berlangganan layanan CCTV via cloud." },
  { "en": "Definisi RAID 0?", "id": "Striping, menggabung disk tanpa redundansi." },
  { "en": "Definisi RAID 1?", "id": "Mirroring, duplikasi data ke disk lain." },
  { "en": "Definisi RAID 5?", "id": "Striping dengan satu paritas terdistribusi." },
  { "en": "Definisi RAID 6?", "id": "Striping dengan dua paritas terdistribusi." },
  { "en": "Definisi RAID 10?", "id": "Gabungan dari mirroring dan striping." },
  { "en": "Apa itu 'write endurance' SSD?", "id": "Batas siklus tulis sebuah SSD." },
  { "en": "Pentingnya 'write endurance' CCTV?", "id": "Perekaman terus-menerus butuh daya tahan." },
  { "en": "Syarat video 'enhancement' di pengadilan?", "id": "Proses terdokumentasi, tidak mengubah fakta." },
  { "en": "Dokumentasi 'enhancement' berisi?", "id": "Software, filter, dan parameter yang dipakai." },
  { "en": "Beda 'clarification' & 'manipulation'?", "id": "Clarification menjernihkan, manipulasi mengubah." },
  { "en": "Peran saksi ahli video?", "id": "Menjelaskan proses forensik di pengadilan." },
  { "en": "Korelasi data CCTV & BTS?", "id": "Mencocokkan rekaman dengan lokasi HP." },
  { "en": "Tantangan Big Data di VMS?", "id": "Volume data besar, kecepatan proses." },
  { "en": "Apa itu 'real-time processing'?", "id": "Data diolah saat itu juga." },
  { "en": "Apa itu 'batch processing'?", "id": "Data dikumpulkan dulu, baru diolah." },
  { "en": "Apa itu 'false positive' analitik?", "id": "Alarm palsu dari deteksi keliru." },
  { "en": "Apa itu 'false negative' analitik?", "id": "Kejadian penting gagal terdeteksi." },
  { "en": "Cara mengurangi 'false positive'?", "id": "Kalibrasi, atur sensitivitas, gunakan AI." },
  { "en": "Apa itu 'optical character recognition'?", "id": "Membaca teks dari dalam gambar." },
  { "en": "Kepanjangan OCR?", "id": "Optical Character Recognition." },
  { "en": "Penerapan OCR di CCTV?", "id": "Membaca plat nomor, nomor kontainer." },
  { "en": "Apa itu 'video stitching'?", "id": "Menggabungkan video dari banyak kamera." },
  { "en": "Apa itu 'defog'?", "id": "Fitur mengurangi efek kabut." },
  { "en": "Apa itu 'electronic image stabilization'?", "id": "Stabilisasi gambar secara digital." },
  { "en": "Kepanjangan EIS?", "id": "Electronic Image Stabilization." },
  { "en": "Apa itu 'optical image stabilization'?", "id": "Stabilisasi gambar menggunakan hardware lensa." },
  { "en": "Kepanjangan OIS?", "id": "Optical Image Stabilization." },
  { "en": "Mana lebih efektif, EIS/OIS?", "id": "OIS lebih efektif." },
  { "en": "Apa itu 'digital signature' video?", "id": "Memastikan video tidak diubah." },
  { "en": "Apa itu 'secure boot' NVR?", "id": "Memastikan hanya firmware asli yang jalan." },
  { "en": "Apa itu 'API' VMS?", "id": "Antarmuka untuk integrasi dengan sistem lain." },
  { "en": "Apa itu 'SDK' VMS?", "id": "Alat untuk mengembangkan fitur tambahan." },
  { "en": "Kepanjangan SDK?", "id": "Software Development Kit." },
  { "en": "Apa itu 'bit error rate'?", "id": "Tingkat kesalahan dalam transmisi data." },
  { "en": "Kepanjangan BER?", "id": "Bit Error Rate." },
  { "en": "Apa itu 'gamma correction'?", "id": "Menyesuaikan kecerahan gambar non-linear." },
  { "en": "Apa itu 'dynamic DNS'?", "id": "Layanan untuk IP address dinamis." },
  { "en": "Kepanjangan DDNS?", "id": "Dynamic Domain Name System." },
  { "en": "Fungsi DDNS untuk CCTV?", "id": "Akses remote NVR dengan IP berubah." },
  { "en": "Apa itu 'port forwarding'?", "id": "Mengarahkan trafik port ke IP lokal." },
  { "en": "Risiko 'port forwarding'?", "id": "Membuka celah keamanan jika salah." },
  { "en": "Alternatif 'port forwarding'?", "id": "Gunakan layanan P2P atau cloud." },
  { "en": "Apa itu 'P2P' CCTV?", "id": "Koneksi langsung tanpa port forwarding." },
  { "en": "Apa itu 'UPS'?", "id": "Uninterruptible Power Supply." },
  { "en": "Fungsi UPS untuk NVR?", "id": "Memberi daya saat listrik padam." },
  { "en": "Apa itu 'surge protector'?", "id": "Pelindung dari lonjakan listrik." },
  { "en": "Pentingnya 'surge protector'?", "id": "Mencegah kerusakan perangkat akibat petir." },
  { "en": "Apa itu 'data center' CCTV?", "id": "Pusat penyimpanan dan pengolahan data." },
  { "en": "Syarat 'data center' CCTV?", "id": "Aman, dingin, daya listrik stabil." },
  { "en": "Apa itu 'hot-swappable' HDD?", "id": "HDD bisa diganti tanpa matikan NVR." },
  { "en": "Apa itu 'Network Time Security'?", "id": "Versi aman dari NTP." },
  { "en": "Kepanjangan NTS?", "id": "Network Time Security." },
  { "en": "Apa itu 'data lifecycle management'?", "id": "Manajemen data dari dibuat-dihapus." },
  { "en": "Tahap 'data lifecycle'?", "id": "Create, store, use, share, archive, destroy." },
  { "en": "Apa itu 'archiving'?", "id": "Menyimpan data jangka panjang." },
  { "en": "Media untuk 'archiving'?", "id": "Tape LTO, cloud storage dingin." },
  { "en": "Apa itu 'data classification'?", "id": "Mengelompokkan data berdasarkan sensitivitas." },
  { "en": "Tujuan 'data classification'?", "id": "Menerapkan kontrol keamanan yang sesuai." },
  { "en": "Apa itu 'Gaussian blur'?", "id": "Filter untuk menghaluskan gambar." },
  { "en": "Apa itu 'Unsharp Mask'?", "id": "Filter untuk mempertajam gambar." },
  { "en": "Apa itu 'histogram of oriented gradients'?", "id": "Metode deteksi objek (misal: manusia)." },
  { "en": "Kepanjangan HOG?", "id": "Histogram of Oriented Gradients." },
  { "en": "Apa itu 'machine learning'?", "id": "Sistem yang belajar dari data." },
  { "en": "Beda 'AI' & 'Machine Learning'?", "id": "Machine Learning adalah sub-bidang AI." },
  { "en": "Apa itu 'training data'?", "id": "Data untuk melatih model AI." },
  { "en": "Apa itu 'inference' AI?", "id": "Proses AI membuat prediksi." },
  { "en": "Apa itu 'false match rate'?", "id": "Tingkat kesalahan identifikasi positif palsu." },
  { "en": "Apa itu 'false non-match rate'?", "id": "Tingkat kegagalan sistem mengenali." },
  { "en": "Apa itu 'equal error rate'?", "id": "Titik saat FMR dan FNMR sama." },
  { "en": "Kepanjangan EER?", "id": "Equal Error Rate." },
  { "en": "Apa itu 'privacy impact assessment'?", "id": "Penilaian dampak proyek terhadap privasi." },
  { "en": "Kepanjangan PIA?", "id": "Privacy Impact Assessment." },
  { "en": "Kapan PIA diperlukan?", "id": "Sebelum implementasi sistem pengawasan baru." },
  { "en": "Apa itu 'motion compensation'?", "id": "Teknik kompresi video antar frame." },
  { "en": "Apa itu 'I-frame'?", "id": "Intra-coded frame (frame utuh)." },
  { "en": "Apa itu 'P-frame'?", "id": "Predicted frame (berdasar frame sebelumnya)." },
  { "en": "Apa itu 'B-frame'?", "id": "Bi-directional predicted frame." },
  { "en": "Beda 'Wavelet' & 'Fourier Transform'?", "id": "Wavelet analisa waktu dan frekuensi." },
  { "en": "Keunggulan 'Wavelet Transform'?", "id": "Baik untuk sinyal non-stasioner." },
  { "en": "Apa itu 'frequency domain'?", "id": "Representasi sinyal berdasarkan frekuensinya." },
  { "en": "Apa itu 'Point Spread Function'?", "id": "Respon sistem optik terhadap titik." },
  { "en": "Kepanjangan PSF?", "id": "Point Spread Function." },
  { "en": "Apa itu 'deconvolution'?", "id": "Proses membalikkan distorsi (blur)." },
  { "en": "Apa itu 'blind deconvolution'?", "id": "Deconvolution tanpa mengetahui PSF." },
  { "en": "Ruang warna standar internasional?", "id": "CIE 1931 XYZ." },
  { "en": "Fungsi metadata EXIF?", "id": "Menyimpan informasi teknis sebuah gambar." },
  { "en": "Analisa artefak kompresi?", "id": "Mendeteksi manipulasi dari pola blok." },
  { "en": "Cara deteksi 'video splicing'?", "id": "Analisa noise, timestamp, dan kontinuitas." },
  { "en": "Apa itu 'frame duplication'?", "id": "Menyalin frame untuk menutupi aksi." },
  { "en": "Apa itu 'federated VMS'?", "id": "Gabungan sistem VMS independen." },
  { "en": "Tujuan 'disaster recovery drill'?", "id": "Menguji prosedur pemulihan bencana." },
  { "en": "Apa itu 'capacity planning'?", "id": "Merencanakan kebutuhan sistem di masa depan." },
  { "en": "Apa itu 'Total Cost of Ownership'?", "id": "Total biaya kepemilikan sistem." },
  { "en": "Komponen TCO CCTV?", "id": "Hardware, software, instalasi, listrik, maintenance." },
  { "en": "Masalah 'black box' pada AI?", "id": "Sulit menjelaskan cara AI mengambil keputusan." },
  { "en": "Tantangan hukum AI 'black box'?", "id": "Sulit dipertanggungjawabkan di pengadilan." },
  { "en": "Ancaman 'deepfake' sebagai bukti?", "id": "Potensi bukti palsu yang meyakinkan." },
  { "en": "Apa itu 'augmented reality' CCTV?", "id": "Menampilkan info digital di atas video." },
  { "en": "SOP permintaan 'video redaction'?", "id": "Dasar hukum jelas, proses terdokumentasi." },
  { "en": "Konsekuensi 'chain of custody' rusak?", "id": "Bukti video bisa ditolak pengadilan." },
  { "en": "Beda malfungsi & serangan siber?", "id": "Analisa log, pola, dan IOC." },
  { "en": "Apa itu 'aperture' lensa?", "id": "Bukaan diafragma yang mengontrol cahaya." },
  { "en": "Apa itu 'ISO' pada kamera?", "id": "Tingkat sensitivitas sensor terhadap cahaya." },
  { "en": "Efek ISO tinggi?", "id": "Gambar terang, tapi noise meningkat." },
  { "en": "Apa itu 'exposure triangle'?", "id": "Hubungan antara aperture, ISO, shutter speed." },
  { "en": "Apa itu 'pixel binning'?", "id": "Menggabungkan piksel untuk tingkatkan sensitivitas." },
  { "en": "Apa itu 'back-illuminated sensor'?", "id": "Sensor dengan sensitivitas cahaya lebih baik." },
  { "en": "Kepanjangan BSI sensor?", "id": "Back-Side Illuminated." },
  { "en": "Apa itu 'dynamic range'?", "id": "Rasio antara bagian terterang-tergelap." },
  { "en": "Satuan 'dynamic range'?", "id": "Decibels (dB) atau stops." },
  { "en": "Apa itu 'tonal range'?", "id": "Distribusi kecerahan dalam sebuah gambar." },
  { "en": "Apa itu 'color gamut'?", "id": "Rentang warna yang bisa ditampilkan." },
  { "en": "Standar 'color gamut' umum?", "id": "sRGB, Adobe RGB, DCI-P3." },
  { "en": "Apa itu 'color depth'?", "id": "Jumlah bit untuk merepresentasikan warna." },
  { "en": "Contoh 'color depth'?", "id": "8-bit, 10-bit, 12-bit." },
  { "en": "Efek 'color depth' tinggi?", "id": "Gradasi warna lebih halus." },
  { "en": "Apa itu 'aliasing'?", "id": "Distorsi pada sinyal frekuensi tinggi." },
  { "en": "Contoh 'aliasing' pada video?", "id": "Moire pattern pada garis rapat." },
  { "en": "Fungsi 'anti-aliasing filter'?", "id": "Mengurangi efek aliasing." },
  { "en": "Apa itu 'Nyquist theorem'?", "id": "Prinsip dasar dalam sampling sinyal." },
  { "en": "Apa itu 'interpolation'?", "id": "Memperkirakan nilai di antara data." },
  { "en": "Fungsi 'interpolation' pada gambar?", "id": "Mengubah ukuran gambar (resizing)." },
  { "en": "Apa itu 'extrapolation'?", "id": "Memperkirakan nilai di luar data." },
  { "en": "Apa itu 'convolutional kernel'?", "id": "Matriks untuk operasi filter gambar." },
  { "en": "Apa itu 'image registration'?", "id": "Menyelaraskan beberapa gambar menjadi satu." },
  { "en": "Apa itu 'optical flow'?", "id": "Pola pergerakan objek antar frame." },
  { "en": "Fungsi 'optical flow'?", "id": "Estimasi kecepatan dan arah gerakan." },
  { "en": "Apa itu 'background subtraction'?", "id": "Memisahkan objek bergerak dari latar." },
  { "en": "Tantangan 'background subtraction'?", "id": "Perubahan cahaya, latar belakang dinamis." },
  { "en": "Apa itu 'Fourier analysis'?", "id": "Analisa sinyal dalam domain frekuensi." },
  { "en": "Apa itu 'power spectrum'?", "id": "Distribusi kekuatan sinyal per frekuensi." },
  { "en": "Apa itu 'phase correlation'?", "id": "Metode untuk mengukur pergeseran gambar." },
  { "en": "Apa itu 'feature matching'?", "id": "Mencocokkan fitur unik antar gambar." },
  { "en": "Algoritma 'feature matching'?", "id": "SIFT, SURF, ORB." },
  { "en": "Apa itu 'affine transformation'?", "id": "Transformasi geometri (rotasi, skala)." },
  { "en": "Apa itu 'perspective transformation'?", "id": "Transformasi untuk memperbaiki sudut pandang." },
  { "en": "Apa itu 'epipolar geometry'?", "id": "Geometri dari sistem dua kamera." },
  { "en": "Apa itu 'stereo vision'?", "id": "Membuat persepsi kedalaman dari dua kamera." },
  { "en": "Fungsi 'stereo vision'?", "id": "Mengukur jarak objek secara akurat." },
  { "en": "Apa itu 'structured light'?", "id": "Memproyeksikan pola cahaya untuk scan 3D." },
  { "en": "Apa itu 'time-of-flight' camera?", "id": "Kamera yang mengukur jarak." },
  { "en": "Kepanjangan ToF camera?", "id": "Time-of-Flight camera." },
  { "en": "Prinsip kerja ToF?", "id": "Mengukur waktu pantul cahaya inframerah." },
  { "en": "Apa itu 'camera trapping'?", "id": "Penggunaan kamera otomatis di alam liar." },
  { "en": "Apa itu 'geospatial intelligence'?", "id": "Intelijen dari data geospasial." },
  { "en": "Kepanjangan GEOINT?", "id": "Geospatial Intelligence." },
  { "en": "Kaitan GEOINT dan CCTV?", "id": "Memetakan dan mengkorelasikan lokasi CCTV." },
  { "en": "Apa itu 'pattern recognition'?", "id": "Pengenalan pola dalam data." },
  { "en": "Apa itu 'anomaly detection'?", "id": "Deteksi kejadian yang menyimpang." },
  { "en": "Penerapan 'anomaly detection' CCTV?", "id": "Deteksi mobil lawan arus." },
  { "en": "Apa itu 'edge computing'?", "id": "Komputasi yang dilakukan dekat sumber data." },
  { "en": "Contoh 'edge computing' CCTV?", "id": "Analitik AI langsung di kamera." },
  { "en": "Apa itu 'fog computing'?", "id": "Lapisan komputasi antara edge-cloud." },
  { "en": "Apa itu 'video transcoding'?", "id": "Mengubah format atau bitrate video." },
  { "en": "Apa itu 'adaptive bitrate streaming'?", "id": "Menyesuaikan kualitas video dengan bandwidth." },
  { "en": "Apa itu 'jitter buffer'?", "id": "Buffer untuk mengatasi variasi waktu tunda." },
  { "en": "Apa itu 'peer-to-peer' streaming?", "id": "Streaming langsung antar perangkat." },
  { "en": "Apa itu 'WebRTC'?", "id": "Standar komunikasi video realtime di web." },
  { "en": "Kepanjangan WebRTC?", "id": "Web Real-Time Communication." },
  { "en": "Apa itu 'gimbal'?", "id": "Alat mekanis untuk stabilisasi kamera." },
  { "en": "Fungsi 'gimbal' pada drone?", "id": "Menjaga rekaman video tetap stabil." },
  { "en": "Apa itu 'rolling shutter compensation'?", "id": "Software untuk perbaiki efek rolling shutter." },
  { "en": "Apa itu 'generative adversarial network'?", "id": "Arsitektur AI untuk membuat data sintetis." },
  { "en": "Kepanjangan GAN?", "id": "Generative Adversarial Network." },
  { "en": "Penggunaan GAN untuk kejahatan?", "id": "Membuat deepfake video atau gambar." },
  { "en": "Penggunaan GAN untuk kebaikan?", "id": "Meningkatkan resolusi, membuat data latih." },
  { "en": "Apa itu 'Bayesian filtering'?", "id": "Filter statistik berbasis probabilitas." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Filter untuk estimasi dan prediksi." },
  { "en": "Fungsi 'Kalman filter' CCTV?", "id": "Melacak objek bergerak secara akurat." },
  { "en": "Dasar hukum penyitaan BWC?", "id": "Bagian dari peralatan melekat pada petugas." },
  { "en": "Tantangan 'deepfake' pada alibi?", "id": "Menciptakan bukti alibi palsu." },
  { "en": "Beda 'authentication' & 'identification'?", "id": "Authentication verifikasi, identification mencari." },
  { "en": "Apa itu 'video hashing' MD5/SHA?", "id": "Membuat sidik jari digital unik video." },
  { "en": "Konsep 'lossy' vs 'lossless' compression?", "id": "Lossy buang data, lossless tidak." },
  { "en": "Contoh kompresi 'lossless'?", "id": "ZIP, PNG." },
  { "en": "Contoh kompresi 'lossy'?", "id": "JPEG, H.264, MP3." },
  { "en": "Apa itu 'compression artifact'?", "id": "Distorsi gambar akibat kompresi lossy." },
  { "en": "Contoh 'compression artifact'?", "id": "Blocking, ringing, posterization." },
  { "en": "Apa itu 'color banding'?", "id": "Gradasi warna yang tidak mulus." },
  { "en": "Apa itu 'bit depth'?", "id": "Jumlah bit per piksel warna." },
  { "en": "Efek 'bit depth' rendah?", "id": "Menyebabkan 'color banding'." },
  { "en": "Apa itu 'vector quantization'?", "id": "Metode kompresi berbasis blok data." },
  { "en": "Apa itu 'Discrete Cosine Transform'?", "id": "Transformasi matematis untuk kompresi JPEG." },
  { "en": "Kepanjangan DCT?", "id": "Discrete Cosine Transform." },
  { "en": "Apa itu 'perceptual video quality'?", "id": "Kualitas video menurut persepsi manusia." },
  { "en": "Standar evaluasi kualitas video?", "id": "PSNR, SSIM." },
  { "en": "Kepanjangan PSNR?", "id": "Peak Signal-to-Noise Ratio." },
  { "en": "Kepanjangan SSIM?", "id": "Structural Similarity Index." },
  { "en": "Apa itu 'just noticeable difference'?", "id": "Perubahan terkecil yang bisa dilihat." },
  { "en": "Kepanjangan JND?", "id": "Just Noticeable Difference." },
  { "en": "Apa itu 'camera fingerprinting'?", "id": "Identifikasi kamera dari 'noise pattern'." },
  { "en": "Apa itu 'sensor pattern noise'?", "id": "Pola noise unik dari sensor." },
  { "en": "Kegunaan 'sensor pattern noise'?", "id": "Membuktikan sumber asli sebuah gambar." },
  { "en": "Apa itu 'video source identification'?", "id": "Menentukan asal perangkat perekam video." },
  { "en": "Apa itu 'format shifting'?", "id": "Mengubah format file video bukti." },
  { "en": "Risiko 'format shifting'?", "id": "Potensi kehilangan metadata dan kualitas." },
  { "en": "Apa itu 'video segmentation'?", "id": "Memecah video menjadi adegan-adegan." },
  { "en": "Apa itu 'shot boundary detection'?", "id": "Deteksi pergantian adegan atau 'cut'." },
  { "en": "Apa itu 'event detection'?", "id": "Deteksi kejadian spesifik dalam video." },
  { "en": "Beda 'event' dan 'activity'?", "id": "Event instan, activity durasi panjang." },
  { "en": "Apa itu 'multi-camera tracking'?", "id": "Melacak target melewati banyak kamera." },
  { "en": "Tantangan 'multi-camera tracking'?", "id": "Menjaga identitas target saat pindah kamera." },
  { "en": "Apa itu 'person re-identification'?", "id": "Mencocokkan orang di kamera berbeda." },
  { "en": "Dasar 'person re-identification'?", "id": "Fitur pakaian, postur, warna." },
  { "en": "Apa itu 'active learning' AI?", "id": "AI meminta input manusia." },
  { "en": "Apa itu 'unsupervised learning'?", "id": "AI belajar tanpa data berlabel." },
  { "en": "Apa itu 'supervised learning'?", "id": "AI belajar dari data berlabel." },
  { "en": "Apa itu 'reinforcement learning'?", "id": "AI belajar dari coba-coba (reward)." },
  { "en": "Apa itu 'transfer learning'?", "id": "Menggunakan model AI terlatih." },
  { "en": "Apa itu 'model drift'?", "id": "Penurunan performa model AI." },
  { "en": "Penyebab 'model drift'?", "id": "Perubahan data di dunia nyata." },
  { "en": "Apa itu 'explainable AI' (XAI)?", "id": "AI yang bisa menjelaskan keputusannya." },
  { "en": "Pentingnya XAI untuk hukum?", "id": "Dasar pertanggungjawaban keputusan AI." },
  { "en": "Apa itu 'adversarial patch'?", "id": "Stiker untuk mengelabui sistem deteksi." },
  { "en": "Apa itu 'data augmentation'?", "id": "Memperbanyak data latih secara sintetis." },
  { "en": "Tujuan 'data augmentation'?", "id": "Meningkatkan ketahanan model AI." },
  { "en": "Apa itu 'bias' dalam AI?", "id": "Kecenderungan AI tidak adil." },
  { "en": "Penyebab 'bias' dalam AI?", "id": "Data latih yang tidak representatif." },
  { "en": "Dampak 'bias' pengenalan wajah?", "id": "Akurasi rendah pada kelompok minoritas." },
  { "en": "Apa itu 'federated learning'?", "id": "Melatih AI tanpa kumpulkan data." },
  { "en": "Manfaat 'federated learning'?", "id": "Menjaga privasi data pengguna." },
  { "en": "Apa itu 'differential privacy'?", "id": "Menambahkan noise untuk melindungi privasi." },
  { "en": "Apa itu 'k-anonymity'?", "id": "Memastikan data individu tak terbedakan." },
  { "en": "Apa itu 'secure enclave'?", "id": "Area terproteksi di dalam prosesor." },
  { "en": "Fungsi 'secure enclave' CCTV?", "id": "Melindungi kunci enkripsi video." },
  { "en": "Apa itu 'trusted boot'?", "id": "Proses boot yang terverifikasi keamanannya." },
  { "en": "Apa itu 'firmware signing'?", "id": "Tanda tangan digital untuk firmware." },
  { "en": "Tujuan 'firmware signing'?", "id": "Mencegah instalasi firmware berbahaya." },
  { "en": "Apa itu 'zero-day vulnerability'?", "id": "Celah keamanan yang belum diketahui." },
  { "en": "Apa itu 'backdoor'?", "id": "Akses rahasia yang sengaja dibuat." },
  { "en": "Apa itu 'jitter' dalam sinyal?", "id": "Deviasi sinyal dari periodisitasnya." },
  { "en": "Apa itu 'eye diagram'?", "id": "Visualisasi untuk analisa kualitas sinyal." },
  { "en": "Apa itu 'constellation diagram'?", "id": "Visualisasi sinyal modulasi digital." },
  { "en": "Apa itu 'matched filter'?", "id": "Filter untuk memaksimalkan SNR." },
  { "en": "Apa itu 'Wiener filter'?", "id": "Filter untuk mengurangi noise." },
  { "en": "Apa itu 'image registration'?", "id": "Menyelaraskan dua gambar atau lebih." },
  { "en": "Apa itu 'image fusion'?", "id": "Menggabungkan informasi dari banyak gambar." },
  { "en": "Contoh 'image fusion'?", "id": "Gabungkan gambar thermal dan visual." },
  { "en": "Apa itu 'inpainting'?", "id": "Mengisi bagian gambar yang rusak." },
  { "en": "Apa itu 'outpainting'?", "id": "Memperluas gambar secara sintetis." },
  { "en": "Apa itu 'style transfer' AI?", "id": "Mengaplikasikan gaya satu gambar ke lain." },
  { "en": "Apa itu 'camera forensics'?", "id": "Forensik untuk identifikasi kamera sumber." },
  { "en": "Dasar 'camera forensics'?", "id": "Analisa PRNU, artefak lensa." },
  { "en": "Apa itu 'video game forensics'?", "id": "Forensik pada konsol atau game." },
  { "en": "Apa itu 'drone forensics'?", "id": "Forensik pada pesawat tanpa awak." },
  { "en": "Data dari 'drone forensics'?", "id": "Log penerbangan, video, data GPS." },
  { "en": "Apa itu 'vehicle forensics'?", "id": "Forensik pada sistem infotainment mobil." },
  { "en": "Apa itu 'IoT forensics'?", "id": "Forensik pada perangkat Internet of Things." },
  { "en": "Tantangan 'IoT forensics'?", "id": "Keragaman perangkat, data volatile." },
  { "en": "Apa itu 'wearable forensics'?", "id": "Forensik pada perangkat wearable." },
  { "en": "Contoh perangkat 'wearable'?", "id": "Smartwatch, fitness tracker." },
  { "en": "Apa itu 'cloud forensics'?", "id": "Forensik pada data di cloud." },
  { "en": "Tantangan 'cloud forensics'?", "id": "Yurisdiksi hukum, akses data." },
  { "en": "Apa itu 'live memory forensics'?", "id": "Analisa RAM pada sistem berjalan." },
  { "en": "Pentingnya 'live memory forensics'?", "id": "Menemukan bukti yang hilang saat mati." },
  { "en": "Apa itu 'network forensics'?", "id": "Analisa lalu lintas jaringan." },
  { "en": "Apa itu 'PCAP'?", "id": "File berisi rekaman paket jaringan." },
  { "en": "Alat untuk analisa PCAP?", "id": "Wireshark." },
  { "en": "Apa itu 'log retention policy'?", "id": "Kebijakan lama penyimpanan log." },
  { "en": "Pentingnya 'log retention policy'?", "id": "Kepatuhan hukum dan kebutuhan investigasi." },
  { "en": "Prinsip 'data minimization'?", "id": "Hanya kumpulkan data yang diperlukan." },
  { "en": "Prinsip 'purpose limitation'?", "id": "Gunakan data sesuai tujuan awal." }

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
