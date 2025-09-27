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


  { "en": "Apa Itu Jaringan Listrik Cerdas (Smart Grid)?", "id": "Jaringan Listrik Dengan Komunikasi Dua Arah." },
  { "en": "Apa Tujuan Utama Smart Grid?", "id": "Meningkatkan Efisiensi, Keandalan, Dan Keamanan Energi." },
  { "en": "Apa Perbedaan Utama Dengan Jaringan Konvensional?", "id": "Komunikasi Satu Arah Melawan Dua Arah." },
  { "en": "Apa Itu Advanced Metering Infrastructure (AMI)?", "id": "Infrastruktur Komunikasi Untuk Meteran Cerdas." },
  { "en": "Apa Itu Meteran Cerdas (Smart Meter)?", "id": "Meteran Listrik Dengan Komunikasi Dua Arah." },
  { "en": "Apa Manfaat Meteran Cerdas Bagi Konsumen?", "id": "Memantau Penggunaan Energi Secara Real-Time." },
  { "en": "Apa Manfaat Meteran Cerdas Bagi Utilitas?", "id": "Pembacaan Otomatis Dan Deteksi Pemadaman." },
  { "en": "Apa Itu Demand Response?", "id": "Konsumen Mengubah Penggunaan Merespons Sinyal." },
  { "en": "Apa Sinyal Pemicu Demand Response?", "id": "Harga Listrik Tinggi Atau Kebutuhan Jaringan." },
  { "en": "Apa Itu Home Area Network (HAN)?", "id": "Jaringan Perangkat Cerdas Di Dalam Rumah." },
  { "en": "Perangkat Apa Yang Terhubung Ke HAN?", "id": "Meteran Cerdas, Termostat, Peralatan Rumah." },
  { "en": "Apa Itu Distributed Generation (DG)?", "id": "Pembangkit Listrik Skala Kecil Tersebar." },
  { "en": "Sebutkan Contoh Distributed Generation?", "id": "Panel Surya Atap, Turbin Angin Kecil." },
  { "en": "Apa Itu Sumber Daya Energi Terdistribusi (DER)?", "id": "Mencakup DG Dan Penyimpanan Energi." },
  { "en": "Apa Itu Penyimpanan Energi?", "id": "Menyimpan Energi Listrik Untuk Nanti." },
  { "en": "Sebutkan Contoh Penyimpanan Energi?", "id": "Baterai, Flywheel, Dan Penyimpanan Udara." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Yang Dapat Beroperasi." },
  { "en": "Apa Itu Mode Pulau (Islanding)?", "id": "Microgrid Beroperasi Terpisah Dari Jaringan Utama." },
  { "en": "Apa Itu Otomasi Distribusi?", "id": "Menggunakan Otomasi Untuk Mengelola Jaringan." },
  { "en": "Apa Itu Self-Healing Grid?", "id": "Jaringan Yang Dapat Mendeteksi Dan Memperbaiki." },
  { "en": "Apa Itu Fault Location, Isolation, and Service Restoration (FLISR)?", "id": "Fungsi Kunci Dari Jaringan Self-Healing." },
  { "en": "Apa Itu Recloser Cerdas?", "id": "Pemutus Sirkuit Otomatis Dengan Komunikasi." },
  { "en": "Apa Itu Saklar Cerdas?", "id": "Saklar Jaringan Yang Dapat Dikontrol Jarak." },
  { "en": "Apa Itu Wide Area Monitoring System (WAMS)?", "id": "Sistem Pemantauan Jaringan Listrik Skala Luas." },
  { "en": "Apa Itu Phasor Measurement Unit (PMU)?", "id": "Sensor Cerdas Untuk Mengukur Fasor Tegangan." },
  { "en": "Apa Itu Sinkrofasor?", "id": "Pengukuran Fasor Yang Disinkronkan Waktu." },
  { "en": "Bagaimana PMU (Phasor Measurement Unit) Disinkronkan?", "id": "Menggunakan Sinyal Waktu GPS (Global Positioning System)." },
  { "en": "Apa Manfaat WAMS (Wide Area Monitoring System)?", "id": "Meningkatkan Stabilitas Dan Visibilitas Jaringan." },
  { "en": "Apa Itu Kendaraan Listrik (Electric Vehicle)?", "id": "Mobil Yang Digerakkan Oleh Motor Listrik." },
  { "en": "Bagaimana EV (Electric Vehicle) Berinteraksi Dengan Smart Grid?", "id": "Sebagai Beban Dan Penyimpanan Energi Potensial." },
  { "en": "Apa Itu Pengisian Cerdas (Smart Charging)?", "id": "Mengontrol Waktu Dan Laju Pengisian EV." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Kendaraan Listrik Memberi Daya Kembali Ke Jaringan." },
  { "en": "Apa Itu Kualitas Daya?", "id": "Ukuran Kualitas Tegangan, Arus, Frekuensi." },
  { "en": "Bagaimana Smart Grid Meningkatkan Kualitas Daya?", "id": "Dengan Deteksi Dan Respon Cepat." },
  { "en": "Apa Itu Harmonik?", "id": "Distorsi Bentuk Gelombang Sinus." },
  { "en": "Apa Itu Voltage Sag?", "id": "Penurunan Tegangan Jangka Pendek." },
  { "en": "Apa Itu Jaringan Komunikasi?", "id": "Tulang Punggung Dari Smart Grid." },
  { "en": "Teknologi Komunikasi Apa Yang Digunakan?", "id": "Serat Optik, Nirkabel, Power Line Communication." },
  { "en": "Apa Itu Power Line Communication (PLC)?", "id": "Mengirim Data Melalui Kabel Listrik." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Melindungi Jaringan Dari Serangan Siber." },
  { "en": "Mengapa Keamanan Siber Penting Untuk Smart Grid?", "id": "Melindungi Infrastruktur Kritis Dari Serangan." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengamankan Data Selama Transmisi." },
  { "en": "Apa Itu Firewall?", "id": "Membatasi Akses Jaringan Yang Tidak Sah." },
  { "en": "Apa Itu Interoperabilitas?", "id": "Kemampuan Perangkat Berbeda Bekerja Sama." },
  { "en": "Mengapa Standar Penting Untuk Interoperabilitas?", "id": "Memastikan Komunikasi Yang Konsisten." },
  { "en": "Apa Itu IEC (International Electrotechnical Commission) 61850?", "id": "Standar Komunikasi Untuk Otomasi Gardu Induk." },
  { "en": "Apa Itu Gardu Induk Cerdas?", "id": "Gardu Induk Dengan Otomasi Dan Kontrol." },
  { "en": "Apa Itu Intelligent Electronic Device (IED)?", "id": "Perangkat Cerdas Di Gardu Induk." },
  { "en": "Sebutkan Contoh IED (Intelligent Electronic Device)?", "id": "Relai Proteksi Digital, PMU (Phasor Measurement Unit)." },
  { "en": "Apa Itu Manajemen Data Meteran (MDM)?", "id": "Sistem Perangkat Lunak Untuk Mengelola Data." },
  { "en": "Apa Itu Big Data Dalam Smart Grid?", "id": "Volume Data Besar Dari Sensor Cerdas." },
  { "en": "Apa Itu Analisis Data?", "id": "Menganalisis Data Untuk Mendapatkan Wawasan." },
  { "en": "Apa Itu Prediksi Beban?", "id": "Memperkirakan Permintaan Listrik Di Masa Depan." },
  { "en": "Apa Itu Prediksi Pembangkitan?", "id": "Memperkirakan Output Dari Sumber Terbarukan." },
  { "en": "Apa Itu Sistem Manajemen Energi (EMS)?", "id": "Perangkat Lunak Untuk Mengelola Jaringan Transmisi." },
  { "en": "Apa Itu Sistem Manajemen Distribusi (DMS)?", "id": "Perangkat Lunak Untuk Mengelola Jaringan Distribusi." },
  { "en": "Apa Itu Outage Management System (OMS)?", "id": "Perangkat Lunak Untuk Mengelola Pemadaman." },
  { "en": "Apa Itu Sistem Informasi Geografis (GIS)?", "id": "Memetakan Aset Jaringan Secara Geografis." },
  { "en": "Apa Itu Model Jaringan?", "id": "Representasi Digital Dari Jaringan Listrik." },
  { "en": "Apa Itu Estimasi Keadaan?", "id": "Memperkirakan Keadaan Jaringan Saat Ini." },
  { "en": "Apa Itu Aliran Daya Optimal?", "id": "Menemukan Kondisi Operasi Paling Efisien." },
  { "en": "Apa Itu Pembangkit Virtual (Virtual Power Plant)?", "id": "Agregasi Sumber Daya Energi Terdistribusi." },
  { "en": "Apa Itu Transactive Energy?", "id": "Pendekatan Pasar Untuk Mengelola Jaringan." },
  { "en": "Apa Itu Teknologi Blockchain?", "id": "Buku Besar Terdistribusi Untuk Transaksi." },
  { "en": "Bagaimana Blockchain Dapat Digunakan?", "id": "Untuk Perdagangan Energi Peer-to-Peer." },
  { "en": "Apa Itu Awan (Cloud Computing)?", "id": "Menyediakan Layanan Komputasi Melalui Internet." },
  { "en": "Apa Itu Edge Computing?", "id": "Memproses Data Lebih Dekat Ke Sumber." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Yang Terhubung." },
  { "en": "Bagaimana IoT (Internet of Things) Terkait Smart Grid?", "id": "Sensor Dan Perangkat Cerdas Adalah IoT." },
  { "en": "Apa Itu Sensor Cerdas?", "id": "Sensor Dengan Kemampuan Komunikasi Dan Pemrosesan." },
  { "en": "Apa Itu Otomasi Rumah?", "id": "Mengotomatiskan Kontrol Perangkat Di Rumah." },
  { "en": "Apa Itu Building Automation System (BAS)?", "id": "Sistem Otomasi Untuk Gedung Komersial." },
  { "en": "Apa Itu Industrial Automation?", "id": "Otomasi Dalam Proses Industri." },
  { "en": "Apa Itu Efisiensi Energi?", "id": "Menggunakan Lebih Sedikit Energi Untuk Tugas." },
  { "en": "Apa Itu Konservasi Energi?", "id": "Mengurangi Penggunaan Energi." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel?", "id": "Jaringan Sensor Yang Berkomunikasi Nirkabel." },
  { "en": "Apa Itu Protokol Zigbee?", "id": "Protokol Nirkabel Berdaya Rendah." },
  { "en": "Apa Itu Wi-SUN?", "id": "Standar Nirkabel Untuk Jaringan Utilitas." },
  { "en": "Apa Itu Seluler (Cellular)?", "id": "Teknologi Komunikasi Nirkabel Jarak Jauh." },
  { "en": "Apa Itu 5G?", "id": "Generasi Kelima Teknologi Seluler." },
  { "en": "Apa Itu Ketersediaan (Availability)?", "id": "Persentase Waktu Sistem Beroperasi." },
  { "en": "Apa Itu Keandalan (Reliability)?", "id": "Probabilitas Sistem Beroperasi Tanpa Kegagalan." },
  { "en": "Apa Itu Ketahanan (Resilience)?", "id": "Kemampuan Pulih Dari Gangguan." },
  { "en": "Apa Itu Ancaman Cuaca Ekstrem?", "id": "Badai, Banjir, Dan Panas Ekstrem." },
  { "en": "Bagaimana Smart Grid Meningkatkan Ketahanan?", "id": "Dengan Isolasi Gangguan Dan Rute Ulang." },
  { "en": "Apa Itu Aset Jaringan?", "id": "Transformator, Kabel, Dan Pemutus Sirkuit." },
  { "en": "Apa Itu Manajemen Aset?", "id": "Mengelola Siklus Hidup Aset Jaringan." },
  { "en": "Apa Itu Pemeliharaan Prediktif?", "id": "Memprediksi Kapan Aset Akan Gagal." },
  { "en": "Apa Itu Pemantauan Kondisi?", "id": "Memantau Kesehatan Aset Secara Real-Time." },
  { "en": "Apa Itu Digital Twin?", "id": "Model Virtual Dari Aset Fisik." },
  { "en": "Apa Itu Konsumen Proaktif (Prosumer)?", "id": "Konsumen Yang Juga Memproduksi Energi." },
  { "en": "Apa Itu Tarif Berdasarkan Waktu (Time-of-Use Tariff)?", "id": "Harga Listrik Bervariasi Berdasarkan Waktu." },
  { "en": "Apa Itu Harga Puncak Kritis (Critical Peak Pricing)?", "id": "Harga Sangat Tinggi Selama Permintaan." },
  { "en": "Apa Itu Program Pengurangan Beban?", "id": "Insentif Untuk Mengurangi Penggunaan." },
  { "en": "Apa Itu Arus Searah Tegangan Tinggi (HVDC)?", "id": "Teknologi Transmisi Daya Efisien." },
  { "en": "Apa Itu Flexible AC Transmission System (FACTS)?", "id": "Perangkat Elektronika Daya Untuk Kontrol." },
  { "en": "Apa Itu STATCOM (Static Synchronous Compensator)?", "id": "Perangkat FACTS Untuk Dukungan Tegangan." },
  { "en": "Apa Itu Neighborhood Area Network (NAN)?", "id": "Jaringan Yang Menghubungkan Meteran Cerdas." },
  { "en": "Apa Teknologi Umum Untuk NAN?", "id": "RF (Radio Frequency) Mesh Dan PLC (Power Line Communication)." },
  { "en": "Apa Itu Wide Area Network (WAN)?", "id": "Jaringan Komunikasi Jarak Jauh." },
  { "en": "Bagaimana WAN Digunakan Dalam Smart Grid?", "id": "Menghubungkan Jaringan NAN Ke Pusat Data." },
  { "en": "Apa Itu In-Home Display (IHD)?", "id": "Perangkat Untuk Menampilkan Konsumsi Energi." },
  { "en": "Bagaimana IHD (In-Home Display) Berkomunikasi?", "id": "Biasanya Melalui Jaringan HAN (Home Area Network)." },
  { "en": "Apa Itu Volt-VAR Optimization (VVO)?", "id": "Mengoptimalkan Tegangan Dan Daya Reaktif Jaringan." },
  { "en": "Apa Manfaat VVO (Volt-VAR Optimization)?", "id": "Mengurangi Kerugian Energi Dan Meningkatkan Efisiensi." },
  { "en": "Apa Itu Kontrol Kapasitor Bank?", "id": "Mengotomatiskan Switching Kapasitor Di Jaringan." },
  { "en": "Apa Itu Distributed Energy Resource Management System (DERMS)?", "id": "Perangkat Lunak Untuk Mengelola DER." },
  { "en": "Apa Tantangan Integrasi DER (Distributed Energy Resource)?", "id": "Variabilitas, Kestabilan Tegangan, Dan Proteksi." },
  { "en": "Apa Itu Voltage Rise?", "id": "Kenaikan Tegangan Akibat Injeksi Daya." },
  { "en": "Apa Itu Jaringan Mesh Nirkabel?", "id": "Setiap Node Dapat Berkomunikasi Dengan Node Lain." },
  { "en": "Apa Keuntungan Jaringan Mesh?", "id": "Keandalan Tinggi Dan Kemampuan Self-Healing." },
  { "en": "Apa Itu Public Key Infrastructure (PKI)?", "id": "Sistem Untuk Mengelola Kunci Kriptografi." },
  { "en": "Mengapa PKI (Public Key Infrastructure) Penting?", "id": "Untuk Otentikasi Aman Antar Perangkat." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Sistem Yang Mendeteksi Aktivitas Jaringan Mencurigakan." },
  { "en": "Apa Itu Otentikasi?", "id": "Proses Memverifikasi Identitas Suatu Entitas." },
  { "en": "Apa Itu Otorisasi?", "id": "Proses Memberikan Izin Akses." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Buatan Yang Ditunjukkan Oleh Mesin." },
  { "en": "Apa Itu Machine Learning (ML)?", "id": "Sub-bidang AI Yang Memungkinkan Sistem Belajar." },
  { "en": "Bagaimana AI (Artificial Intelligence) Digunakan Dalam Smart Grid?", "id": "Untuk Prediksi, Optimisasi, Dan Deteksi Anomali." },
  { "en": "Apa Itu Dynamic Pricing?", "id": "Harga Listrik Yang Berubah Secara Dinamis." },
  { "en": "Apa Itu Time-of-Use (TOU) Pricing?", "id": "Harga Bervariasi Berdasarkan Blok Waktu." },
  { "en": "Apa Itu Real-Time Pricing (RTP)?", "id": "Harga Berubah Sangat Sering (Setiap Jam)." },
  { "en": "Apa Itu Critical Peak Pricing (CPP)?", "id": "Harga Sangat Tinggi Selama Periode Kritis." },
  { "en": "Apa Itu Prosumer?", "id": "Konsumen Yang Juga Memproduksi Energi." },
  { "en": "Apa Itu Pengisian EV (Electric Vehicle) Level 1?", "id": "Pengisian Lambat Dari Stopkontak Standar." },
  { "en": "Apa Itu Pengisian EV (Electric Vehicle) Level 2?", "id": "Pengisian Cepat Menggunakan Tegangan 240V." },
  { "en": "Apa Itu DC (Direct Current) Fast Charging?", "id": "Pengisian Sangat Cepat Langsung Ke Baterai." },
  { "en": "Apa Itu Sistem Manajemen Pengisian?", "id": "Mengelola Proses Pengisian Banyak Kendaraan." },
  { "en": "Apa Itu Distributed Network Protocol (DNP3)?", "id": "Protokol Komunikasi Untuk Sistem SCADA." },
  { "en": "Apa Itu Zigbee Smart Energy Profile (SEP)?", "id": "Standar Protokol Untuk Jaringan HAN." },
  { "en": "Apa Itu Analisis Data?", "id": "Proses Memeriksa Data Untuk Menarik Kesimpulan." },
  { "en": "Apa Itu Visualisasi Data?", "id": "Menampilkan Data Secara Grafis Untuk Pemahaman." },
  { "en": "Apa Itu Dasbor?", "id": "Tampilan Visual Dari Metrik Kunci." },
  { "en": "Apa Itu Grid Modernization?", "id": "Upaya Memperbarui Jaringan Listrik." },
  { "en": "Apa Itu Sensor Cerdas?", "id": "Sensor Dengan Kemampuan Pemrosesan Dan Komunikasi." },
  { "en": "Apa Itu Fault Current Indicator (FCI)?", "id": "Sensor Yang Mendeteksi Arus Gangguan." },
  { "en": "Apa Itu Sensor Suhu Transformator?", "id": "Memantau Suhu Minyak Dan Kumparan." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Untuk Pemantauan Dan Kontrol Jarak Jauh." },
  { "en": "Apa Itu Human-Machine Interface (HMI)?", "id": "Antarmuka Grafis Untuk Operator." },
  { "en": "Apa Itu Data Historian?", "id": "Database Untuk Menyimpan Data Runtun Waktu." },
  { "en": "Apa Itu Arus Bolak-balik (AC)?", "id": "Arus Listrik Yang Arahnya Berubah." },
  { "en": "Apa Itu Arus Searah (DC)?", "id": "Arus Listrik Yang Mengalir Satu Arah." },
  { "en": "Apa Itu Konverter Daya?", "id": "Perangkat Yang Mengubah Karakteristik Daya." },
  { "en": "Apa Itu Inverter?", "id": "Mengubah DC (Direct Current) Menjadi AC (Alternating Current)." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah AC (Alternating Current) Menjadi DC (Direct Current)." },
  { "en": "Apa Itu Sistem Penyimpanan Energi Baterai (BESS)?", "id": "Menyimpan Energi Listrik Dalam Baterai." },
  { "en": "Apa Aplikasi BESS (Battery Energy Storage System)?", "id": "Dukungan Jaringan Dan Arbitrase Energi." },
  { "en": "Apa Itu State of Charge (SoC)?", "id": "Tingkat Pengisian Baterai." },
  { "en": "Apa Itu State of Health (SoH)?", "id": "Ukuran Kondisi Degradasi Baterai." },
  { "en": "Apa Itu Siklus Hidup Baterai?", "id": "Jumlah Siklus Pengisian Sebelum Kapasitas Turun." },
  { "en": "Apa Itu Efisiensi Bolak-balik (Round-trip Efficiency)?", "id": "Rasio Energi Keluar Terhadap Energi Masuk." },
  { "en": "Apa Itu Regulasi Tegangan?", "id": "Menjaga Tingkat Tegangan Dalam Batas." },
  { "en": "Apa Itu Regulasi Frekuensi?", "id": "Menjaga Frekuensi Jaringan Dalam Batas." },
  { "en": "Apa Itu Cadangan Operasi?", "id": "Kapasitas Pembangkitan Cadangan." },
  { "en": "Apa Itu Cadangan Berputar (Spinning Reserve)?", "id": "Cadangan Dari Generator Yang Sudah Sinkron." },
  { "en": "Apa Itu Black Start Capability?", "id": "Kemampuan Memulai Pembangkit Tanpa Daya Eksternal." },
  { "en": "Apa Itu Model Perkiraan Cuaca?", "id": "Digunakan Untuk Memprediksi Pembangkitan Angin/Surya." },
  { "en": "Apa Itu Variabilitas Sumber Terbarukan?", "id": "Output Yang Berubah Dan Tidak Dapat Dikendalikan." },
  { "en": "Apa Itu Intermitensi?", "id": "Sifat Tidak Terus-menerus Dari Sumber Energi." },
  { "en": "Apa Itu Curtailment?", "id": "Mengurangi Output Pembangkitan Secara Sengaja." },
  { "en": "Mengapa Curtailment Dilakukan?", "id": "Untuk Mencegah Kelebihan Produksi." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Fisik Jaringan Listrik." },
  { "en": "Apa Itu Jaringan Radial?", "id": "Memiliki Satu Jalur Ke Pelanggan." },
  { "en": "Apa Itu Jaringan Loop?", "id": "Memiliki Dua Jalur Ke Pelanggan." },
  { "en": "Apa Itu Jaringan Otomatis?", "id": "Jaringan Dengan Kemampuan Self-Healing." },
  { "en": "Apa Itu Privasi Data?", "id": "Melindungi Informasi Pribadi Konsumen." },
  { "en": "Apa Itu Data Konsumsi Energi?", "id": "Dianggap Informasi Sensitif." },
  { "en": "Apa Itu Anonimisasi Data?", "id": "Menghapus Informasi Identitas Pribadi." },
  { "en": "Apa Itu Regulasi Privasi?", "id": "Hukum Yang Mengatur Penggunaan Data." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Menyimpan Dan Memproses Data Di Server." },
  { "en": "Apa Itu Otomatisasi Gardu Induk (Substation Automation)?", "id": "Mengotomatiskan Kontrol Dan Pemantauan Gardu." },
  { "en": "Apa Itu Process Bus?", "id": "Jaringan Komunikasi Di Dalam Gardu Induk." },
  { "en": "Apa Itu GOOSE (Generic Object Oriented Substation Event)?", "id": "Pesan Cepat Untuk Proteksi." },
  { "en": "Apa Itu Sampel Nilai (Sampled Values)?", "id": "Data Arus Dan Tegangan Digital." },
  { "en": "Apa Itu Transformator Instrumen Optik?", "id": "Mengukur Arus/Tegangan Menggunakan Optik." },
  { "en": "Apa Itu Manajemen Tenaga Kerja Bergerak?", "id": "Mengelola Kru Lapangan Secara Efisien." },
  { "en": "Apa Itu Analisis Prediktif?", "id": "Menggunakan Data Untuk Memprediksi Hasil." },
  { "en": "Apa Itu Simulasi Jaringan?", "id": "Memodelkan Perilaku Jaringan Listrik." },
  { "en": "Apa Itu Real-Time Digital Simulator (RTDS)?", "id": "Simulator Perangkat Keras-dalam-Loop." },
  { "en": "Apa Itu Deregulasi Pasar?", "id": "Memisahkan Pembangkitan Dari Transmisi." },
  { "en": "Apa Itu Pasar Grosir Listrik?", "id": "Tempat Jual Beli Listrik." },
  { "en": "Apa Itu Pasar Eceran Listrik?", "id": "Konsumen Memilih Penyedia Listrik." },
  { "en": "Apa Itu Agregator?", "id": "Entitas Yang Menggabungkan DER (Distributed Energy Resource)." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Respons Jaringan Terhadap Perubahan Frekuensi." },
  { "en": "Apa Itu Inersia?", "id": "Resistensi Jaringan Terhadap Perubahan Frekuensi." },
  { "en": "Apa Itu Inersia Virtual?", "id": "Inersia Yang Disimulasikan Oleh Inverter." },
  { "en": "Apa Itu Perangkat Lunak Sebagai Layanan (SaaS)?", "id": "Model Pengiriman Perangkat Lunak." },
  { "en": "Apa Itu Platform Sebagai Layanan (PaaS)?", "id": "Menyediakan Platform Untuk Mengembangkan." },
  { "en": "Apa Itu Infrastruktur Sebagai Layanan (IaaS)?", "id": "Menyediakan Infrastruktur Komputasi." },
  { "en": "Apa Itu Transduser?", "id": "Mengubah Satu Bentuk Energi Ke Lainnya." },
  { "en": "Apa Itu Sensor?", "id": "Jenis Transduser Yang Mendeteksi." },
  { "en": "Apa Itu Aktuator?", "id": "Jenis Transduser Yang Menghasilkan Aksi." },
  { "en": "Apa Itu Komunikasi Nirkabel?", "id": "Transmisi Data Tanpa Kabel." },
  { "en": "Apa Itu Spektrum Radio?", "id": "Rentang Frekuensi Untuk Komunikasi." },
  { "en": "Apa Itu Lisensi Spektrum?", "id": "Izin Untuk Menggunakan Frekuensi." },
  { "en": "Apa Itu Pita Tanpa Lisensi?", "id": "Frekuensi Terbuka Untuk Penggunaan Umum." },
  { "en": "Apa Itu Interferensi?", "id": "Gangguan Pada Sinyal Nirkabel." },
  { "en": "Apa Itu Sistem Proteksi?", "id": "Sistem Untuk Melindungi Jaringan Dari Gangguan." },
  { "en": "Apa Itu Relai Proteksi Digital?", "id": "Relai Cerdas Dengan Kemampuan Komunikasi." },
  { "en": "Apa Itu Skema Proteksi Adaptif?", "id": "Pengaturan Proteksi Yang Dapat Berubah Otomatis." },
  { "en": "Mengapa Proteksi Adaptif Penting?", "id": "Untuk Jaringan Dengan DER (Distributed Energy Resource) Tinggi." },
  { "en": "Apa Itu Pembangkitan Terpusat?", "id": "Pembangkit Listrik Besar Jauh Dari Beban." },
  { "en": "Apa Itu Aliran Daya Satu Arah?", "id": "Ciri Khas Jaringan Listrik Konvensional." },
  { "en": "Apa Itu Aliran Daya Dua Arah?", "id": "Terjadi Dengan Adanya Pembangkit Terdistribusi." },
  { "en": "Apa Itu Konsumen?", "id": "Entitas Yang Mengkonsumsi Energi Listrik." },
  { "en": "Apa Itu Produsen?", "id": "Entitas Yang Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Prosumer?", "id": "Entitas Yang Memproduksi Dan Mengkonsumsi Energi." },
  { "en": "Apa Itu Jaringan Transmisi?", "id": "Jaringan Tegangan Tinggi Jarak Jauh." },
  { "en": "Apa Itu Jaringan Distribusi?", "id": "Jaringan Tegangan Menengah Dan Rendah." },
  { "en": "Apa Itu Gardu Induk?", "id": "Fasilitas Untuk Mengubah Level Tegangan." },
  { "en": "Apa Itu Transformator Daya?", "id": "Mengubah Tegangan Antara Level Yang Berbeda." },
  { "en": "Apa Itu Pemutus Sirkuit?", "id": "Saklar Untuk Memutus Arus Gangguan." },
  { "en": "Apa Itu Saklar Pemisah?", "id": "Saklar Untuk Mengisolasi Peralatan." },
  { "en": "Apa Itu Busbar?", "id": "Konduktor Umum Di Gardu Induk." },
  { "en": "Apa Itu Analisis Aliran Daya?", "id": "Menghitung Aliran Daya Di Jaringan." },
  { "en": "Apa Itu Analisis Kontingensi?", "id": "Menganalisis Efek Kegagalan Komponen." },
  { "en": "Apa Itu Keamanan Operasi?", "id": "Menjaga Jaringan Beroperasi Dengan Aman." },
  { "en": "Apa Itu Standardisasi?", "id": "Proses Membuat Standar Teknis." },
  { "en": "Apa Itu Open Standard?", "id": "Standar Yang Tersedia Untuk Umum." },
  { "en": "Apa Itu Proprietary Standard?", "id": "Standar Yang Dimiliki Oleh Perusahaan." },
  { "en": "Apa Itu National Institute of Standards and Technology (NIST)?", "id": "Badan Standarisasi Di Amerika Serikat." },
  { "en": "Apa Itu Kerangka Kerja Interoperabilitas Smart Grid?", "id": "Kerangka Kerja Konseptual Dari NIST." },
  { "en": "Apa Itu Model Konseptual?", "id": "Representasi Abstrak Dari Suatu Sistem." },
  { "en": "Apa Itu Aktor Dalam Model Konseptual?", "id": "Entitas Yang Berpartisipasi Dalam Sistem." },
  { "en": "Apa Itu Domain Dalam Model Konseptual?", "id": "Kelompok Fungsional Dalam Smart Grid." },
  { "en": "Sebutkan Contoh Domain Smart Grid?", "id": "Pembangkitan, Transmisi, Distribusi, Dan Pelanggan." },
  { "en": "Apa Itu Privasi?", "id": "Melindungi Informasi Pribadi Dari Akses." },
  { "en": "Apa Itu Integritas Data?", "id": "Menjaga Keakuratan Dan Konsistensi Data." },
  { "en": "Apa Itu Ketersediaan Data?", "id": "Memastikan Data Dapat Diakses Saat Dibutuhkan." },
  { "en": "Apa Itu Non-repudiation?", "id": "Memastikan Pengirim Tidak Dapat Menyangkal." },
  { "en": "Apa Itu Manajemen Identitas Dan Akses?", "id": "Mengontrol Siapa Yang Dapat Mengakses." },
  { "en": "Apa Itu Peran (Role)?", "id": "Sekumpulan Izin Yang Diberikan Kepada." },
  { "en": "Apa Itu Otentikasi Multi-Faktor?", "id": "Menggunakan Lebih Dari Satu Metode." },
  { "en": "Apa Itu Kriptografi?", "id": "Ilmu Mengamankan Komunikasi." },
  { "en": "Apa Itu Enkripsi Simetris?", "id": "Menggunakan Kunci Yang Sama Untuk Enkripsi." },
  { "en": "Apa Itu Enkripsi Asimetris?", "id": "Menggunakan Pasangan Kunci Publik Dan Privat." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memastikan Keaslian Dan Integritas Pesan." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "Mengikat Identitas Ke Kunci Publik." },
  { "en": "Apa Itu Otoritas Sertifikat (CA)?", "id": "Entitas Yang Menerbitkan Sertifikat Digital." },
  { "en": "Apa Itu Pengisian Daya Kendaraan Listrik?", "id": "Proses Mengisi Baterai EV." },
  { "en": "Apa Itu Manajemen Beban?", "id": "Mengontrol Beban Listrik Untuk Menstabilkan." },
  { "en": "Apa Itu Pengurangan Beban (Load Shedding)?", "id": "Memutuskan Beban Secara Sengaja." },
  { "en": "Apa Itu Pergeseran Beban (Load Shifting)?", "id": "Memindahkan Konsumsi Energi Ke Waktu." },
  { "en": "Apa Itu Harga Dinamis?", "id": "Harga Listrik Yang Berubah-ubah." },
  { "en": "Apa Itu Termostat Cerdas?", "id": "Termostat Yang Dapat Diprogram Dan Terhubung." },
  { "en": "Apa Itu Peralatan Cerdas?", "id": "Peralatan Rumah Yang Dapat Berkomunikasi." },
  { "en": "Apa Itu Manajemen Energi Rumah (HEMS)?", "id": "Sistem Untuk Mengelola Energi Di Rumah." },
  { "en": "Apa Itu Manajemen Energi Gedung (BEMS)?", "id": "Sistem Untuk Mengelola Energi Di Gedung." },
  { "en": "Apa Itu Data Historis?", "id": "Data Yang Telah Dikumpulkan Dari Masa." },
  { "en": "Apa Itu Model Prediktif?", "id": "Model Yang Digunakan Untuk Membuat Prediksi." },
  { "en": "Apa Itu Regresi Linier?", "id": "Metode Statistik Untuk Memodelkan Hubungan." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Pembelajaran Mesin Untuk Prediksi." },
  { "en": "Apa Itu Analisis Runtun Waktu?", "id": "Menganalisis Data Yang Diurutkan Waktu." },
  { "en": "Apa Itu Analisis Spasial?", "id": "Menganalisis Data Geografis." },
  { "en": "Apa Itu Antarmuka Pemrograman Aplikasi (API)?", "id": "Memungkinkan Aplikasi Berkomunikasi Satu Sama Lain." },
  { "en": "Apa Itu Sistem Terdistribusi?", "id": "Sistem Dengan Komponen Di Lokasi." },
  { "en": "Apa Itu Komputasi Terdistribusi?", "id": "Komputasi Yang Dilakukan Di Sistem." },
  { "en": "Apa Itu Toleransi Kesalahan?", "id": "Kemampuan Sistem Beroperasi Meski Ada." },
  { "en": "Apa Itu Redundansi?", "id": "Duplikasi Komponen Untuk Toleransi Kesalahan." },
  { "en": "Apa Itu Failover?", "id": "Beralih Ke Sistem Cadangan." },
  { "en": "Apa Itu Skalabilitas?", "id": "Kemampuan Sistem Menangani Peningkatan Beban." },
  { "en": "Apa Itu Penskalaan Vertikal?", "id": "Meningkatkan Sumber Daya Satu Mesin." },
  { "en": "Apa Itu Penskalaan Horizontal?", "id": "Menambahkan Lebih Banyak Mesin." },
  { "en": "Apa Itu Penyeimbangan Beban (Load Balancing)?", "id": "Mendistribusikan Beban Kerja Di Banyak." },
  { "en": "Apa Itu Komunikasi Optik?", "id": "Menggunakan Cahaya Untuk Mentransmisikan Data." },
  { "en": "Apa Itu Serat Optik?", "id": "Media Transmisi Berbasis Kaca." },
  { "en": "Apa Itu Komunikasi Nirkabel?", "id": "Transmisi Data Tanpa Kabel." },
  { "en": "Apa Itu Pita Spektrum?", "id": "Rentang Frekuensi Radio." },
  { "en": "Apa Itu Pita Berlisensi?", "id": "Spektrum Yang Memerlukan Izin." },
  { "en": "Apa Itu Pita Tanpa Lisensi?", "id": "Spektrum Terbuka Untuk Umum." },
  { "en": "Apa Itu Jaringan Ad Hoc?", "id": "Jaringan Nirkabel Terdesentralisasi." },
  { "en": "Apa Itu Protokol Routing?", "id": "Aturan Untuk Menentukan Jalur." },
  { "en": "Apa Itu Kualitas Layanan (QoS)?", "id": "Kemampuan Memberikan Prioritas." },
  { "en": "Apa Itu Latensi?", "id": "Waktu Tunda Dalam Transmisi." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Dalam Waktu Tunda." },
  { "en": "Apa Itu Packet Loss?", "id": "Kehilangan Paket Data Selama." },
  { "en": "Apa Itu Bandwidth?", "id": "Kapasitas Transmisi Maksimum." },
  { "en": "Apa Itu Arsitektur Berorientasi Layanan (SOA)?", "id": "Gaya Desain Perangkat Lunak." },
  { "en": "Apa Itu Layanan Web?", "id": "Layanan Yang Dapat Diakses." },
  { "en": "Apa Itu Arus Bolak-balik (AC)?", "id": "Arus Yang Arahnya Berubah." },
  { "en": "Apa Itu Arus Searah (DC)?", "id": "Arus Yang Mengalir Satu Arah." },
  { "en": "Apa Itu Grid-Forming Inverter?", "id": "Inverter Yang Dapat Menciptakan." },
  { "en": "Apa Itu Grid-Following Inverter?", "id": "Inverter Yang Mengikuti Jaringan." },
  { "en": "Apa Itu Pembangkit Sinkron?", "id": "Generator Yang Berputar Sinkron." },
  { "en": "Apa Itu Pembangkit Asinkron?", "id": "Generator Yang Tidak Berputar." },
  { "en": "Apa Itu Mesin Induksi?", "id": "Jenis Umum Motor Dan Generator." },
  { "en": "Apa Itu Slip?", "id": "Perbedaan Antara Kecepatan Sinkron." },
  { "en": "Apa Itu Inersia?", "id": "Resistensi Terhadap Perubahan Kecepatan." },
  { "en": "Apa Itu Respon Frekuensi Cepat?", "id": "Respons Cepat Terhadap Perubahan." },
  { "en": "Apa Itu Model Dinamis?", "id": "Model Yang Mewakili Perilaku." },
  { "en": "Apa Itu Simulasi Transien?", "id": "Menganalisis Perilaku Jangka Pendek." },
  { "en": "Apa Itu Analisis Keadaan Tunak?", "id": "Menganalisis Perilaku Jangka Panjang." },
  { "en": "Apa Itu Optimisasi?", "id": "Menemukan Solusi Terbaik." },
  { "en": "Apa Itu Fungsi Objektif?", "id": "Fungsi Yang Akan Diminimalkan." },
  { "en": "Apa Itu Kendala?", "id": "Batasan Yang Harus Dipenuhi." },
  { "en": "Apa Itu Pemrograman Linier?", "id": "Metode Optimisasi Untuk Masalah Linier." },
  { "en": "Apa Itu Pemrograman Non-Linier?", "id": "Metode Optimisasi Untuk Masalah Non-Linier." },
  { "en": "Apa Itu Visualisasi Jaringan?", "id": "Representasi Grafis Dari Jaringan Listrik." },
  { "en": "Apa Itu Diagram Satu Garis?", "id": "Diagram Sederhana Dari Sistem Tiga Fasa." },
  { "en": "Apa Itu Manajemen Aset Jaringan?", "id": "Mengelola Siklus Hidup Aset." },
  { "en": "Apa Itu Pemeliharaan Berbasis Kondisi?", "id": "Pemeliharaan Berdasarkan Kondisi Aktual Aset." },
  { "en": "Apa Itu Analisis Kegagalan?", "id": "Menyelidiki Penyebab Kegagalan Peralatan." },
  { "en": "Apa Itu Model Penuaan Aset?", "id": "Memprediksi Degradasi Aset Seiring Waktu." },
  { "en": "Apa Itu Digital Twin?", "id": "Model Virtual Dari Aset Fisik." },
  { "en": "Apa Manfaat Digital Twin?", "id": "Untuk Simulasi, Analisis, Dan Pemeliharaan." },
  { "en": "Apa Itu Manajemen Tenaga Kerja Lapangan?", "id": "Mengelola Kru Perbaikan Dan Pemeliharaan." },
  { "en": "Apa Itu Sistem Informasi Pelanggan (CIS)?", "id": "Database Informasi Dan Tagihan Pelanggan." },
  { "en": "Apa Itu Portal Pelanggan?", "id": "Antarmuka Web Untuk Pelanggan." },
  { "en": "Apa Itu Partisipasi Pelanggan?", "id": "Keterlibatan Aktif Pelanggan Dalam Jaringan." },
  { "en": "Apa Itu Gamifikasi?", "id": "Menggunakan Elemen Game Untuk Mendorong Perilaku." },
  { "en": "Apa Itu Pasar Kapasitas?", "id": "Pasar Untuk Menjamin Ketersediaan Pembangkitan." },
  { "en": "Apa Itu Layanan Tambahan (Ancillary Services)?", "id": "Layanan Untuk Mendukung Stabilitas Jaringan." },
  { "en": "Sebutkan Contoh Layanan Tambahan?", "id": "Regulasi Frekuensi Dan Cadangan Operasi." },
  { "en": "Apa Itu Pasar Day-Ahead?", "id": "Pasar Energi Untuk Hari Berikutnya." },
  { "en": "Apa Itu Pasar Real-Time?", "id": "Pasar Energi Untuk Pengiriman Segera." },
  { "en": "Apa Itu Harga Nodal?", "id": "Harga Listrik Di Lokasi Tertentu." },
  { "en": "Apa Itu Kemacetan Transmisi?", "id": "Batasan Fisik Pada Jaringan Transmisi." },
  { "en": "Apa Itu Hak Transmisi Finansial (FTR)?", "id": "Instrumen Keuangan Untuk Melindungi Diri." },
  { "en": "Apa Itu Operator Pasar Independen (IMO)?", "id": "Menjalankan Pasar Listrik." },
  { "en": "Apa Itu Operator Sistem Independen (ISO)?", "id": "Mengelola Keandalan Jaringan." },
  { "en": "Apa Itu Regional Transmission Organization (RTO)?", "id": "Organisasi Transmisi Regional." },
  { "en": "Apa Itu Regulasi Kinerja?", "id": "Regulasi Berdasarkan Metrik Kinerja." },
  { "en": "Apa Itu Insentif Kinerja?", "id": "Penghargaan Finansial Untuk Kinerja Baik." },
  { "en": "Apa Itu Kerangka Regulasi?", "id": "Aturan Yang Mengatur Sektor Energi." },
  { "en": "Apa Itu Kebijakan Energi?", "id": "Arah Strategis Pemerintah Untuk Energi." },
  { "en": "Apa Itu Standar Portofolio Terbarukan?", "id": "Mandat Untuk Porsi Energi Terbarukan." },
  { "en": "Apa Itu Tarif Feed-In?", "id": "Harga Premium Untuk Energi Terbarukan." },
  { "en": "Apa Itu Net Metering?", "id": "Menghitung Selisih Ekspor Dan Impor." },
  { "en": "Apa Itu Komunitas Energi?", "id": "Grup Lokal Yang Memproduksi Energi." },
  { "en": "Apa Itu Keadilan Energi?", "id": "Distribusi Manfaat Dan Beban." },
  { "en": "Apa Itu Propagasi Gelombang Radio?", "id": "Cara Gelombang Radio Merambat." },
  { "en": "Apa Itu Path Loss?", "id": "Pelemahan Sinyal Seiring Jarak." },
  { "en": "Apa Itu Shadowing?", "id": "Pelemahan Akibat Hambatan Besar." },
  { "en": "Apa Itu Fading Multipath?", "id": "Fluktuasi Sinyal Akibat Pantulan." },
  { "en": "Apa Itu Selular?", "id": "Jaringan Yang Dibagi Menjadi Sel." },
  { "en": "Apa Itu Stasiun Pangkalan (Base Station)?", "id": "Menara Seluler." },
  { "en": "Apa Itu Handoff?", "id": "Perpindahan Antara Stasiun Pangkalan." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan Nirkabel Lokal." },
  { "en": "Apa Itu Access Point?", "id": "Perangkat Yang Membuat Jaringan." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers) 802.11?", "id": "Keluarga Standar Untuk Wi-Fi." },
  { "en": "Apa Itu WiMAX (Worldwide Interoperability for Microwave Access)?", "id": "Teknologi Nirkabel Jarak Jauh." },
  { "en": "Apa Itu Jaringan Sensor Tubuh?", "id": "Sensor Yang Dikenakan Di Tubuh." },
  { "en": "Apa Itu Komunikasi Optik Nirkabel?", "id": "Menggunakan Cahaya Untuk Komunikasi." },
  { "en": "Apa Itu Li-Fi (Light Fidelity)?", "id": "Komunikasi Berbasis Cahaya Tampak." },
  { "en": "Apa Itu Arus Gangguan?", "id": "Arus Selama Hubung Singkat." },
  { "en": "Apa Itu Tingkat Gangguan?", "id": "Magnitudo Arus Gangguan." },
  { "en": "Apa Itu Analisis Hubung Singkat?", "id": "Menghitung Arus Gangguan." },
  { "en": "Apa Itu Impedansi Thevenin?", "id": "Impedansi Ekuivalen Jaringan." },
  { "en": "Apa Itu Kapasitas Pemutusan?", "id": "Arus Maksimum Yang Dapat." },
  { "en": "Apa Itu Stabilitas?", "id": "Kemampuan Sistem Kembali." },
  { "en": "Apa Itu Model Ayunan (Swing Equation)?", "id": "Model Dinamis Generator." },
  { "en": "Apa Itu Kriteria Area Sama?", "id": "Metode Grafis Untuk Analisis." },
  { "en": "Apa Itu Waktu Pemutusan Kritis?", "id": "Waktu Maksimum Untuk Memutuskan." },
  { "en": "Apa Itu Model Beban Motor?", "id": "Mewakili Perilaku Dinamis Motor." },
  { "en": "Apa Itu Penyimpanan Energi Baterai?", "id": "Nama Lain Untuk BESS." },
  { "en": "Apa Itu Kimia Baterai?", "id": "Jenis Bahan Kimia Dalam." },
  { "en": "Apa Itu Lithium-Ion?", "id": "Kimia Baterai Populer." },
  { "en": "Apa Itu Asam Timbal?", "id": "Kimia Baterai Yang Lebih." },
  { "en": "Apa Itu Baterai Aliran?", "id": "Menyimpan Energi Dalam Elektrolit." },
  { "en": "Apa Itu Konverter Daya Baterai?", "id": "Mengelola Pengisian Dan Pengosongan." },
  { "en": "Apa Itu Mode Pengisian?", "id": "Konverter Bekerja Sebagai Penyearah." },
  { "en": "Apa Itu Mode Pengosongan?", "id": "Konverter Bekerja Sebagai Inverter." },
  { "en": "Apa Itu Penuaan Baterai?", "id": "Degradasi Kapasitas Seiring Waktu." },
  { "en": "Apa Itu Kalender Penuaan?", "id": "Degradasi Akibat Waktu." },
  { "en": "Apa Itu Siklus Penuaan?", "id": "Degradasi Akibat Penggunaan." },
  { "en": "Apa Itu Manajemen Termal Baterai?", "id": "Menjaga Suhu Baterai Optimal." },
  { "en": "Apa Itu Pendinginan Udara?", "id": "Menggunakan Udara Untuk Mendinginkan." },
  { "en": "Apa Itu Pendinginan Cair?", "id": "Menggunakan Cairan Untuk Mendinginkan." },
  { "en": "Apa Itu Perubahan Fasa Material?", "id": "Menyerap Panas Selama Perubahan." },
  { "en": "Apa Itu Keselamatan Baterai?", "id": "Mencegah Kebakaran Dan Ledakan." },
  { "en": "Apa Itu Thermal Runaway?", "id": "Reaksi Berantai Eksotermik." },
  { "en": "Apa Itu Sirkuit Perlindungan?", "id": "Melindungi Baterai Dari Kondisi." },
  { "en": "Apa Itu Penyeimbangan Sel?", "id": "Menyamakan Tegangan Sel." },
  { "en": "Apa Itu Penyeimbangan Pasif?", "id": "Membuang Energi Dari Sel." },
  { "en": "Apa Itu Penyeimbangan Aktif?", "id": "Memindahkan Energi Antar Sel." },
  { "en": "Apa Itu Perangkat Lunak Tertanam?", "id": "Perangkat Lunak Di Dalam." },
  { "en": "Apa Itu Sistem Operasi Real-Time?", "id": "OS Untuk Aplikasi Real-Time." },
  { "en": "Apa Itu Bahasa Pemrograman?", "id": "Bahasa Untuk Menulis Instruksi." },
  { "en": "Apa Itu C/C++?", "id": "Bahasa Populer Untuk Sistem." },
  { "en": "Apa Itu Python?", "id": "Bahasa Tingkat Tinggi." },
  { "en": "Apa Itu MATLAB/Simulink?", "id": "Lingkungan Untuk Simulasi." },
  { "en": "Apa Itu Desain Berbasis Model?", "id": "Menggunakan Model Untuk Desain." },
  { "en": "Apa Itu Generasi Kode Otomatis?", "id": "Menghasilkan Kode Dari Model." },
  { "en": "Apa Itu Verifikasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak." },
  { "en": "Apa Itu Validasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak." },
  { "en": "Apa Itu Pengujian Unit?", "id": "Menguji Komponen Perangkat Lunak." },
  { "en": "Apa Itu Pengujian Integrasi?", "id": "Menguji Komponen Yang Digabung." },
  { "en": "Apa Itu Pengujian Sistem?", "id": "Menguji Sistem Secara Keseluruhan." },
  { "en": "Apa Itu Debugging?", "id": "Proses Menemukan Dan Memperbaiki." },
  { "en": "Apa Itu Profiling?", "id": "Menganalisis Kinerja Perangkat." },
  { "en": "Apa Itu Manajemen Memori?", "id": "Mengalokasikan Dan Membebaskan." },
  { "en": "Apa Itu Keamanan?", "id": "Melindungi Sistem Dari Ancaman." },
  { "en": "Apa Itu Penilaian Ancaman?", "id": "Mengidentifikasi Potensi Ancaman." },
  { "en": "Apa Itu Mitigasi Ancaman?", "id": "Mengurangi Dampak Ancaman." },
  { "en": "Apa Itu Model Beban Statis?", "id": "Beban Sebagai Fungsi Tegangan Dan Frekuensi." },
  { "en": "Apa Itu Model Beban Dinamis?", "id": "Beban Sebagai Fungsi Waktu Dan Variabel." },
  { "en": "Apa Itu ZIP Model?", "id": "Model Beban Gabungan (Constant Z, I, P)." },
  { "en": "Apa Itu Analisis Harmonik?", "id": "Proses Mempelajari Penyebab Dan Efek Harmonik." },
  { "en": "Apa Itu Aliran Daya Harmonik?", "id": "Menganalisis Aliran Arus Harmonik Di Jaringan." },
  { "en": "Apa Itu Impedansi Harmonik?", "id": "Impedansi Sistem Pada Frekuensi Harmonik Tertentu." },
  { "en": "Apa Itu Sumber Harmonik?", "id": "Peralatan Elektronik Yang Menghasilkan Arus Harmonik." },
  { "en": "Apa Itu Respon Frekuensi Sistem?", "id": "Bagaimana Sistem Merespons Frekuensi Yang Berbeda." },
  { "en": "Apa Itu Plot Impedansi?", "id": "Grafik Impedansi Jaringan Terhadap Frekuensi." },
  { "en": "Apa Itu Anti-Resonansi?", "id": "Kondisi Impedansi Sangat Tinggi Dekat Titik Resonansi." },
  { "en": "Apa Itu Pemodelan Transformator?", "id": "Representasi Matematis Dari Perilaku Transformator." },
  { "en": "Apa Itu Model Rangkaian Ekuivalen?", "id": "Sirkuit Sederhana Mewakili Komponen Kompleks." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Yang Dibutuhkan Untuk Menciptakan Fluks Magnetik." },
  { "en": "Apa Itu Saturasi Inti?", "id": "Inti Magnetik Tidak Dapat Menahan Fluks Lagi." },
  { "en": "Apa Efek Saturasi?", "id": "Menghasilkan Arus Harmonik Yang Sangat Signifikan." },
  { "en": "Apa Itu Histeresis?", "id": "Fenomena Ketergantungan Magnetik Pada Sejarahnya." },
  { "en": "Apa Itu Kerugian Inti?", "id": "Kerugian Histeresis Dan Kerugian Arus Eddy." },
  { "en": "Apa Itu Pemodelan Saluran Transmisi?", "id": "Mewakili Saluran Untuk Analisis Kualitas Daya." },
  { "en": "Apa Itu Model Pi?", "id": "Model Rangkaian Ekuivalen Untuk Saluran Pendek." },
  { "en": "Apa Itu Model Garis Panjang?", "id": "Model Akurat Untuk Saluran Transmisi Panjang." },
  { "en": "Apa Itu Parameter Terdistribusi?", "id": "Resistansi, Induktansi, Kapasitansi Per Satuan Panjang." },
  { "en": "Apa Itu Gelombang Berjalan?", "id": "Gelombang Yang Merambat Sepanjang Saluran Transmisi." },
  { "en": "Apa Itu Pantulan Gelombang?", "id": "Terjadi Akibat Ketidakcocokan Impedansi." },
  { "en": "Apa Itu Koefisien Pantul?", "id": "Rasio Amplitudo Gelombang Pantul Terhadap Datang." },
  { "en": "Apa Itu Lattice Diagram?", "id": "Diagram Grafis Menganalisis Pantulan Gelombang." },
  { "en": "Apa Itu Electromagnetic Transients Program (EMTP)?", "id": "Perangkat Lunak Simulasi Untuk Fenomena Transien." },
  { "en": "Apa Itu Pensinyalan Jalur Daya?", "id": "Mengirim Sinyal Kontrol Melalui Kabel Listrik." },
  { "en": "Apa Itu Ripple Control?", "id": "Contoh Pensinyalan Jalur Daya Frekuensi Rendah." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Perambatan Medan Listrik Dan Medan Magnet." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Bekerja Di Lingkungan Elektromagnetik." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Yang Merusak Kinerja." },
  { "en": "Apa Itu Emisi?", "id": "Pelepasan Energi EM (Electromagnetic) Dari Sebuah Perangkat." },
  { "en": "Apa Itu Kekebalan?", "id": "Kemampuan Perangkat Menahan Gangguan EM (Electromagnetic)." },
  { "en": "Apa Itu Kopling?", "id": "Transfer Energi Gangguan Dari Sumber Ke Korban." },
  { "en": "Apa Itu Grounding Loop?", "id": "Nama Lain Untuk Masalah Loop Tanah." },
  { "en": "Apa Itu Kualitas Sinyal?", "id": "Ukuran Kualitas Sinyal Kontrol Atau Komunikasi." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Sinyal Dari Posisi Idealnya." },
  { "en": "Apa Itu Derau Fasa?", "id": "Representasi Jitter Dalam Domain Frekuensi." },
  { "en": "Apa Itu Sistem Tenaga DC (Direct Current)?", "id": "Sistem Kelistrikan Yang Menggunakan Arus Searah." },
  { "en": "Apa Keuntungan Sistem DC (Direct Current)?", "id": "Tidak Ada Daya Reaktif Atau Masalah Frekuensi." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Mengubah Satu Level Tegangan DC Ke Level Lain." },
  { "en": "Apa Itu Konverter AC-DC?", "id": "Nama Lain Untuk Penyearah (Rectifier)." },
  { "en": "Apa Itu Konverter DC-AC?", "id": "Nama Lain Untuk Inverter." },
  { "en": "Apa Itu Konverter AC-AC?", "id": "Mengubah Tegangan Atau Frekuensi Arus Bolak-balik." },
  { "en": "Apa Itu Sistem Tenaga Hibrida?", "id": "Sistem Yang Menggabungkan Infrastruktur AC Dan DC." },
  { "en": "Apa Itu Inverter Siap Jaringan?", "id": "Inverter Dengan Fitur Pendukung Jaringan Cerdas." },
  { "en": "Apa Itu Kontrol Volt-VAR?", "id": "Mengatur Tegangan Lokal Dengan Mengontrol Daya Reaktif." },
  { "en": "Apa Itu Kontrol Frekuensi-Watt?", "id": "Mengatur Daya Aktif Untuk Menstabilkan Frekuensi." },
  { "en": "Apa Itu Ride-Through Capability?", "id": "Kemampuan Tetap Terhubung Selama Gangguan Jaringan." },
  { "en": "Apa Itu Low Voltage Ride-Through (LVRT)?", "id": "Melewati Gangguan Tegangan Rendah Tanpa Terputus." },
  { "en": "Apa Itu High Voltage Ride-Through (HVRT)?", "id": "Melewati Gangguan Tegangan Tinggi Tanpa Terputus." },
  { "en": "Apa Itu Ketidakstabilan?", "id": "Sistem Tidak Kembali Ke Titik Keseimbangan." },
  { "en": "Apa Itu Osilasi Sub-Sinkron?", "id": "Osilasi Di Bawah Frekuensi Fundamental Sistem." },
  { "en": "Apa Itu Kontrol Tambahan?", "id": "Sistem Kontrol Untuk Meredam Osilasi Jaringan." },
  { "en": "Apa Itu Teori Kontrol?", "id": "Cabang Teknik Dan Matematika Untuk Sistem Dinamis." },
  { "en": "Apa Itu Kontrol Klasik?", "id": "Metode Kontrol Berbasis Fungsi Transfer." },
  { "en": "Apa Itu Kontrol Modern?", "id": "Metode Kontrol Berbasis Ruang Keadaan." },
  { "en": "Apa Itu Ruang Keadaan?", "id": "Representasi Matematis Dari Dinamika Internal." },
  { "en": "Apa Itu Kontrol Umpan Balik?", "id": "Menggunakan Sinyal Output Untuk Mengoreksi Kesalahan." },
  { "en": "Apa Itu Kontrol Umpan Maju?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Sistem." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Yang Menyesuaikan Parameternya." },
  { "en": "Apa Itu Kontrol Kokoh?", "id": "Kontroler Yang Tahan Terhadap Ketidakpastian." },
  { "en": "Apa Itu Kontrol Optimal?", "id": "Mengoptimalkan Suatu Kriteria Kinerja." },
  { "en": "Apa Itu Estimasi Keadaan?", "id": "Memperkirakan Keadaan Internal Sistem Dari Pengukuran." },
  { "en": "Apa Itu Filter Kalman?", "id": "Estimator Optimal Untuk Sistem Linier." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Buatan Yang Ditunjukkan Oleh Mesin." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Pembelajaran Mesin Yang Terinspirasi Otak." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Logika Yang Menangani Ketidakpastian Dan Ambiguitas." },
  { "en": "Apa Itu Algoritma Genetik?", "id": "Algoritma Optimisasi Berbasis Prinsip Evolusi." },
  { "en": "Apa Itu Sistem Pakar?", "id": "Sistem Berbasis Pengetahuan Untuk Domain Khusus." },
  { "en": "Apa Itu Data Mining?", "id": "Menemukan Pola Tersembunyi Dalam Kumpulan Data." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar Dan Kompleks." },
  { "en": "Apa Itu Klasifikasi?", "id": "Tugas Mengelompokkan Data Ke Dalam Kategori." },
  { "en": "Apa Itu Regresi?", "id": "Tugas Memprediksi Nilai Numerik Kontinu." },
  { "en": "Apa Itu Klastering?", "id": "Tugas Menemukan Grup Alami Dalam Data." },
  { "en": "Apa Itu Penilaian Kualitas Daya?", "id": "Proses Mengevaluasi Tingkat Kualitas Daya." },
  { "en": "Apa Itu Diagnostik?", "id": "Proses Mengidentifikasi Penyebab Suatu Masalah." },
  { "en": "Apa Itu Prognostik?", "id": "Proses Memprediksi Masalah Di Masa Depan." },
  { "en": "Apa Itu Kesehatan Sistem?", "id": "Ukuran Kondisi Operasional Sistem Saat Ini." },
  { "en": "Apa Itu Keandalan?", "id": "Probabilitas Beroperasi Tanpa Kegagalan." },
  { "en": "Apa Itu Ketersediaan?", "id": "Proporsi Waktu Sistem Siap Untuk Beroperasi." },
  { "en": "Apa Itu Keterpeliharaan?", "id": "Kemudahan Dalam Memperbaiki Sistem Yang Gagal." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis Mode Dan Efek Kegagalan." },
  { "en": "Apa Itu FTA (Fault Tree Analysis)?", "id": "Analisis Pohon Kesalahan Secara Top-Down." },
  { "en": "Apa Itu Keamanan Siber?", "id": "Perlindungan Sistem Komputer Dari Serangan." },
  { "en": "Apa Itu Kerentanan?", "id": "Kelemahan Dalam Sistem Yang Dapat Dieksploitasi." },
  { "en": "Apa Itu Ancaman?", "id": "Sesuatu Yang Dapat Menyebabkan Kerugian." },
  { "en": "Apa Itu Risiko?", "id": "Potensi Kerugian Akibat Suatu Ancaman." },
  { "en": "Apa Itu Kontrol Keamanan?", "id": "Tindakan Untuk Mengurangi Risiko Keamanan." },
  { "en": "Apa Itu Enkripsi?", "id": "Proses Mengamankan Data Dengan Kriptografi." },
  { "en": "Apa Itu Otentikasi?", "id": "Proses Memverifikasi Identitas Pengguna." },
  { "en": "Apa Itu Otorisasi?", "id": "Proses Memberikan Izin Akses Sumber Daya." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memastikan Keaslian Dan Integritas Data." },
  { "en": "Apa Itu Standar?", "id": "Spesifikasi Yang Disepakati Bersama." },
  { "en": "Apa Itu Regulasi?", "id": "Aturan Yang Diberlakukan Oleh Otoritas." },
  { "en": "Apa Itu Kepatuhan?", "id": "Tindakan Mematuhi Standar Dan Regulasi." },
  { "en": "Apa Itu Deregulasi?", "id": "Mengurangi Atau Menghilangkan Peraturan Pemerintah." },
  { "en": "Apa Itu Pasar Listrik?", "id": "Sistem Untuk Jual Beli Energi Listrik." },
  { "en": "Apa Itu Harga Pasar Real-Time?", "id": "Harga Listrik Yang Berubah Setiap Saat." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mempengaruhi Pola Konsumsi Energi Pelanggan." },
  { "en": "Apa Itu Efisiensi Energi?", "id": "Menggunakan Lebih Sedikit Energi Untuk Pekerjaan Sama." },
  { "en": "Apa Itu Konservasi Energi?", "id": "Mengurangi Penggunaan Energi Secara Keseluruhan." },
  { "en": "Apa Itu Gelombang Tegangan?", "id": "Bentuk Gelombang Tegangan AC (Alternating Current)." },
  { "en": "Apa Itu Gelombang Arus?", "id": "Bentuk Gelombang Arus AC (Alternating Current)." },
  { "en": "Apa Itu Distorsi Notch?", "id": "Nama Lain Untuk Fenomena Notching." },
  { "en": "Apa Itu Komutasi?", "id": "Proses Transfer Arus Antar Saklar." },
  { "en": "Apa Itu Penyearah Terkendali Fasa?", "id": "Sumber Umum Distorsi Harmonik." },
  { "en": "Apa Itu Inverter Sumber Tegangan?", "id": "Sumber Harmonik Pada Sisi AC." },
  { "en": "Apa Itu Switch-Mode Power Supply (SMPS)?", "id": "Catu Daya Yang Menghasilkan Arus Harmonik." },
  { "en": "Apa Itu Arus Input SMPS (Switch-Mode Power Supply)?", "id": "Berbentuk Pulsa Dan Kaya Akan Harmonik." },
  { "en": "Apa Itu Tungku Busur Listrik?", "id": "Beban Besar Yang Menyebabkan Flicker Hebat." },
  { "en": "Apa Itu Mesin Las?", "id": "Beban Yang Menyebabkan Fluktuasi Dan Sag." },
  { "en": "Apa Itu Saturasi Magnetik?", "id": "Fenomena Non-linier Dalam Inti Transformator." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Untuk Membangkitkan Fluks Magnetik." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Menyalakan Transformator." },
  { "en": "Apa Itu Motor Induksi?", "id": "Beban Umum Di Sektor Industri." },
  { "en": "Apakah Motor Induksi Linier?", "id": "Cukup Linier Saat Berjalan Stabil." },
  { "en": "Apa Itu Arus Awal Motor?", "id": "Arus Sangat Tinggi Saat Motor Dinyalakan." },
  { "en": "Apa Itu Soft Starter?", "id": "Perangkat Mengurangi Arus Awal Motor Induksi." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Perangkat Untuk Mengontrol Kecepatan Motor." },
  { "en": "Apakah VFD (Variable Frequency Drive) Linier?", "id": "Tidak, VFD Adalah Beban Non-linier." },
  { "en": "Apa Itu Lampu Fluoresen?", "id": "Beban Non-linier Yang Menghasilkan Harmonik." },
  { "en": "Apa Itu Ballast Elektronik?", "id": "Catu Daya Untuk Lampu Fluoresen." },
  { "en": "Apa Itu Peralatan Medis?", "id": "Seringkali Sensitif Terhadap Gangguan Kualitas Daya." },
  { "en": "Apa Itu Pusat Data?", "id": "Sangat Kritis Terhadap Kualitas Dan Keandalan." },
  { "en": "Apa Itu Redundansi?", "id": "Memiliki Sistem Cadangan Untuk Keandalan." },
  { "en": "Apa Itu Sistem N+1?", "id": "Satu Unit Cadangan Untuk N Unit." },
  { "en": "Apa Itu Titik Kegagalan Tunggal?", "id": "Komponen Yang Kegagalannya Melumpuhkan Sistem." },
  { "en": "Apa Itu Analisis Statistik?", "id": "Menganalisis Data Kualitas Daya Menggunakan Statistik." },
  { "en": "Apa Itu Histogram?", "id": "Grafik Distribusi Frekuensi Suatu Parameter." },
  { "en": "Apa Itu Diagram Sebar (Scatter Plot)?", "id": "Grafik Hubungan Antara Dua Variabel." },
  { "en": "Apa Itu Kurva Durasi?", "id": "Menunjukkan Waktu Suatu Parameter Melebihi Nilai." },
  { "en": "Apa Itu ITI (CBEMA) Curve?", "id": "Kurva Toleransi Tegangan Untuk Peralatan IT." },
  { "en": "Apa Itu Kepatuhan Kualitas Daya?", "id": "Tindakan Memenuhi Standar Dan Regulasi." },
  { "en": "Apa Itu Investigasi Kualitas Daya?", "id": "Proses Menentukan Penyebab Masalah." },
  { "en": "Apa Itu Sumber Gangguan?", "id": "Peralatan Yang Menyebabkan Masalah Kualitas Daya." },
  { "en": "Apa Itu Jalur Propagasi?", "id": "Jalan Dimana Gangguan Menyebar." },
  { "en": "Apa Itu Korban Gangguan?", "id": "Peralatan Yang Terpengaruh Oleh Gangguan." },
  { "en": "Apa Itu Deteksi Arah Gangguan?", "id": "Menentukan Dari Mana Arah Gangguan Datang." },
  { "en": "Apa Itu Pemecahan Masalah?", "id": "Proses Menemukan Dan Memperbaiki Masalah." },
  { "en": "Apa Itu Osiloskop?", "id": "Instrumen Untuk Melihat Bentuk Gelombang." },
  { "en": "Apa Itu Multimeter True RMS?", "id": "Mengukur Nilai RMS Akurat Untuk Gelombang." },
  { "en": "Apa Itu Penganalisis Spektrum?", "id": "Melihat Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Logger Data?", "id": "Perangkat Merekam Pengukuran Seiring Waktu." },
  { "en": "Apa Itu Pemicuan (Triggering)?", "id": "Memulai Pengukuran Berdasarkan Suatu Peristiwa." },
  { "en": "Apa Itu Pemicu Ambang Batas?", "id": "Pemicu Saat Sinyal Melewati Level Tertentu." },
  { "en": "Apa Itu Pengambilan Sampel?", "id": "Mengubah Sinyal Analog Menjadi Data Digital." },
  { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Laju Sampling Terlalu Rendah." },
  { "en": "Apa Itu Jendela Waktu?", "id": "Durasi Pengamatan Untuk Analisis Sinyal." },
  { "en": "Apa Itu Kebocoran Spektral?", "id": "Efek Samping Jendela Waktu Terbatas." },
  { "en": "Apa Itu Windowing?", "id": "Fungsi Matematis Untuk Mengurangi Kebocoran." },
  { "en": "Apa Itu Jendela Hanning?", "id": "Jenis Jendela Fungsi Yang Umum." },
  { "en": "Apa Itu Jendela Persegi?", "id": "Tidak Menggunakan Jendela Sama Sekali." },
  { "en": "Apa Itu Resolusi Frekuensi?", "id": "Kemampuan Membedakan Komponen Frekuensi." },
  { "en": "Apa Itu Deteksi?", "id": "Proses Mengidentifikasi Kehadiran Peristiwa." },
  { "en": "Apa Itu Klasifikasi?", "id": "Proses Mengidentifikasi Jenis Peristiwa." },
  { "en": "Apa Itu Lokalisasi?", "id": "Proses Menemukan Sumber Peristiwa." },
  { "en": "Apa Itu Sistem Pakar?", "id": "Sistem Berbasis Aturan Untuk Diagnostik." },
  { "en": "Apa Itu Wavelet Transform?", "id": "Metode Analisis Waktu-Frekuensi." },
  { "en": "Apa Keuntungan Wavelet?", "id": "Sangat Baik Untuk Menganalisis Transien." },
  { "en": "Apa Itu Tegangan Common-Mode?", "id": "Tegangan Antara Konduktor Dan Ground Referensi." },
  { "en": "Apa Itu Tegangan Differential-Mode?", "id": "Tegangan Antara Dua Konduktor Jalur." },
  { "en": "Apa Itu Pemodelan Komponen?", "id": "Membuat Model Matematis Dari Komponen Listrik." },
  { "en": "Apa Itu SimPowerSystems?", "id": "Toolbox Simulink Untuk Sistem Tenaga Listrik." },
  { "en": "Apa Itu ATP-EMTP (Alternative Transients Program)?", "id": "Program Simulasi Transien Elektromagnetik." },
  { "en": "Apa Itu PSCAD/EMTDC?", "id": "Perangkat Lunak Simulasi Sistem Tenaga." },
  { "en": "Apa Itu Validasi Model?", "id": "Membandingkan Hasil Model Dengan Pengukuran Nyata." },
  { "en": "Apa Itu Verifikasi Model?", "id": "Memeriksa Apakah Model Diimplementasikan Dengan Benar." },
  { "en": "Apa Itu Pengukuran Di Lapangan?", "id": "Pengukuran Yang Dilakukan Di Lokasi Sebenarnya." },
  { "en": "Apa Itu Keselamatan Listrik?", "id": "Praktik Aman Saat Bekerja Dengan Listrik." },
  { "en": "Apa Itu Arc Flash?", "id": "Ledakan Listrik Berbahaya Akibat Busur Api." },
  { "en": "Apa Itu Personal Protective Equipment (PPE)?", "id": "Peralatan Pelindung Diri." },
  { "en": "Apa Itu Lockout-Tagout (LOTO)?", "id": "Prosedur Untuk Mematikan Peralatan Dengan Aman." },
  { "en": "Apa Itu Sistem Tidak Terganggu?", "id": "Sistem Yang Diisolasi Dari Suplai Utama." },
  { "en": "Apa Itu Kondisioner Jalur Aktif?", "id": "Nama Lain Untuk Active Power Filter." },
  { "en": "Apa Itu K-Factor Transformator?", "id": "Kemampuan Transformator Menangani Arus Harmonik." },
  { "en": "Apa Itu Derating Transformator?", "id": "Mengurangi Kapasitas Trafo Akibat Pemanasan Harmonik." },
  { "en": "Apa Itu Konverter 12-Pulsa?", "id": "Menghasilkan Lebih Sedikit Harmonik Dibanding 6-Pulsa." },
  { "en": "Apa Itu Konverter 18-Pulsa?", "id": "Menghasilkan Harmonik Yang Lebih Rendah Lagi." },
  { "en": "Apa Itu Transformator Pergeseran Fasa?", "id": "Digunakan Dalam Konverter Multi-pulsa." },
  { "en": "Apa Itu Interfensi Jalur Komunikasi?", "id": "Derau Mempengaruhi Jalur Telekomunikasi." },
  { "en": "Apa Itu Induksi?", "id": "Kopling Melalui Medan Magnet." },
  { "en": "Apa Itu Kenaikan Potensial Tanah?", "id": "Kenaikan Tegangan Tanah Selama Gangguan." },
  { "en": "Apa Itu Pelindung (Shielding)?", "id": "Menggunakan Lapisan Konduktif Untuk Memblokir Medan." },
  { "en": "Apa Itu Kabel Terpilin?", "id": "Mengurangi Kopling Magnetik Dan Crosstalk." },
  { "en": "Apa Itu Sistem Penyeimbang Beban?", "id": "Mendistribusikan Beban Secara Merata Antar Fasa." },
  { "en": "Apa Itu Koreksi Faktor Daya Aktif?", "id": "PFC (Power Factor Correction) Berbasis Konverter." },
  { "en": "Apa Itu Konverter Boost PFC?", "id": "Topologi Umum Untuk PFC (Power Factor Correction)." },
  { "en": "Apa Itu Mode Konduksi Kritis?", "id": "Mode Operasi PFC (Power Factor Correction)." },
  { "en": "Apa Itu Regulasi Beban?", "id": "Kemampuan Menjaga Tegangan Dengan Perubahan Beban." },
  { "en": "Apa Itu Regulasi Jalur?", "id": "Kemampuan Menjaga Tegangan Dengan Perubahan Input." },
  { "en": "Apa Itu Respon Transien?", "id": "Respons Terhadap Perubahan Beban Mendadak." },
  { "en": "Apa Itu Waktu Stabil?", "id": "Waktu Untuk Mencapai Keadaan Tunak." },
  { "en": "Apa Itu Overshoot?", "id": "Output Melebihi Nilai Akhir Sementara." },
  { "en": "Apa Itu Standar IEC (International Electrotechnical Commission)?", "id": "Organisasi Standar Elektroteknik Internasional." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi Profesional Dan Standar Teknik." },
  { "en": "Apa Itu Standar ANSI (American National Standards Institute)?", "id": "Organisasi Standar Di Amerika Serikat." },
  { "en": "Apa Itu CENELEC?", "id": "Komite Eropa Untuk Standarisasi Elektroteknik." },
  { "en": "Apa Hubungan EN 50160 Dengan CENELEC?", "id": "EN 50160 Adalah Standar Dari CENELEC." },
  { "en": "Apa Itu Gelombang Tegangan?", "id": "Bentuk Gelombang Tegangan Listrik AC." },
  { "en": "Apa Itu Gelombang Arus?", "id": "Bentuk Gelombang Arus Listrik AC." },
  { "en": "Apa Itu Distorsi Interharmonik?", "id": "Distorsi Yang Disebabkan Oleh Komponen Interharmonik." },
  { "en": "Apa Itu Komponen DC (Direct Current)?", "id": "Adanya Tegangan Atau Arus DC Di Sistem AC." },
  { "en": "Apa Efek Komponen DC (Direct Current)?", "id": "Dapat Menyebabkan Saturasi Inti Transformator." },
  { "en": "Apa Itu Pensinyalan Frekuensi Rendah?", "id": "Sinyal Kontrol Yang Dikirim Melalui Jaringan Listrik." },
  { "en": "Apa Itu Flickersturbance?", "id": "Gangguan Yang Menyebabkan Fenomena Flicker." },
  { "en": "Apa Itu Transformator Saturable?", "id": "Transformator Yang Dirancang Untuk Beroperasi Dalam Saturasi." },
  { "en": "Apa Itu Regulator Tegangan Induksi?", "id": "Mengatur Tegangan Dengan Mengubah Kopling Magnetik." },
  { "en": "Apa Itu Seri Kapasitor?", "id": "Kapasitor Yang Dipasang Seri Di Jalur Transmisi." },
  { "en": "Apa Itu Resonansi Sub-Sinkron?", "id": "Osilasi Antara Generator Dan Jaringan Seri." },
  { "en": "Apa Itu Batas Stabilitas?", "id": "Batas Operasi Aman Sistem Tenaga Listrik." },
  { "en": "Apa Itu Redaman?", "id": "Proses Pengurangan Amplitudo Osilasi." },
  { "en": "Apa Itu Osilasi Elektromekanis?", "id": "Osilasi Antara Rotor-rotor Generator Sinkron." },
  { "en": "Apa Itu Model Mesin Sinkron?", "id": "Representasi Matematis Dari Generator Sinkron." },
  { "en": "Apa Itu Model Beban?", "id": "Representasi Matematis Dari Perilaku Beban." },
  { "en": "Apa Itu Simulasi Domain Waktu?", "id": "Menyelesaikan Persamaan Sistem Seiring Waktu." },
  { "en": "Apa Itu Simulasi Domain Frekuensi?", "id": "Menganalisis Sistem Dalam Domain Frekuensi." },
  { "en": "Apa Itu Eigenvalue Analysis?", "id": "Digunakan Untuk Menganalisis Stabilitas Sinyal Kecil." },
  { "en": "Apa Itu Nilai Eigen?", "id": "Menunjukkan Mode Osilasi Dan Redamannya." },
  { "en": "Apa Itu Vektor Eigen?", "id": "Menunjukkan Bentuk Mode Osilasi Sistem." },
  { "en": "Apa Itu Partisipasi Faktor?", "id": "Menunjukkan Elemen Mana Yang Berpartisipasi." },
  { "en": "Apa Itu Teori Normalisasi?", "id": "Metode Analisis Sistem Non-linier." },
  { "en": "Apa Itu Bifurkasi?", "id": "Perubahan Kualitatif Dalam Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Bifurkasi Hopf?", "id": "Menuju Terbentuknya Osilasi Stabil." },
  { "en": "Apa Itu Kekacauan (Chaos)?", "id": "Perilaku Kompleks Dan Tidak Dapat Diprediksi." },
  { "en": "Apa Itu Fraktal?", "id": "Pola Geometris Yang Berulang Pada Setiap Skala." },
  { "en": "Apa Itu Sistem Tenaga Kompleks?", "id": "Sistem Dengan Banyak Komponen Yang Berinteraksi." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Koneksi Fisik Jaringan." },
  { "en": "Apa Itu Teori Graf?", "id": "Cabang Matematika Untuk Studi Graf." },
  { "en": "Apa Itu Node?", "id": "Simpul Atau Titik Dalam Graf." },
  { "en": "Apa Itu Edge?", "id": "Koneksi Antara Dua Node." },
  { "en": "Apa Itu Jaringan Skala Kecil?", "id": "Jaringan Dengan Distribusi Derajat Node." },
  { "en": "Apa Itu Jaringan Bebas Skala?", "id": "Jaringan Dengan Distribusi Derajat Hukum Pangkat." },
  { "en": "Apa Itu Ketahanan Jaringan?", "id": "Kemampuan Jaringan Menahan Gangguan." },
  { "en": "Apa Itu Serangan Bertarget?", "id": "Serangan Pada Node Paling Terhubung." },
  { "en": "Apa Itu Kegagalan Acak?", "id": "Kegagalan Acak Pada Node Jaringan." },
  { "en": "Jaringan Mana Yang Lebih Tahan?", "id": "Jaringan Bebas Skala Lebih Tahan." },
  { "en": "Apa Itu Sistem Self-Organized Criticality?", "id": "Sistem Kompleks Yang Berevolusi Ke Titik Kritis." },
  { "en": "Apa Itu Longsoran (Avalanche)?", "id": "Rangkaian Kegagalan Berantai Dalam Sistem." },
  { "en": "Apa Itu Kaskade Kegagalan?", "id": "Penyebaran Kegagalan Dalam Jaringan." },
  { "en": "Apa Itu Pemadaman Listrik Besar?", "id": "Seringkali Hasil Dari Kaskade Kegagalan." },
  { "en": "Apa Itu Sistem Proteksi Area Luas?", "id": "Skema Proteksi Jaringan Terkoordinasi." },
  { "en": "Apa Itu Pemisahan Pulau Terkendali?", "id": "Memisahkan Jaringan Secara Sengaja Saat Gangguan." },
  { "en": "Apa Itu Pencegahan?", "id": "Tindakan Untuk Mencegah Terjadinya Masalah." },
  { "en": "Apa Itu Koreksi?", "id": "Tindakan Untuk Memperbaiki Masalah Yang Terjadi." },
  { "en": "Apa Itu Keamanan Siber?", "id": "Perlindungan Sistem Komputer Dari Serangan Siber." },
  { "en": "Apa Itu Kerentanan?", "id": "Kelemahan Dalam Sistem Yang Dapat Dieksploitasi." },
  { "en": "Apa Itu Ancaman?", "id": "Sesuatu Yang Dapat Menyebabkan Kerugian." },
  { "en": "Apa Itu Risiko?", "id": "Potensi Kerugian Akibat Suatu Ancaman." },
  { "en": "Apa Itu Penilaian Risiko?", "id": "Proses Mengevaluasi Risiko Keamanan." },
  { "en": "Apa Itu Manajemen Risiko?", "id": "Mengelola Risiko Ke Tingkat Yang Dapat Diterima." },
  { "en": "Apa Itu Enkripsi?", "id": "Proses Mengamankan Data Dengan Kriptografi." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memverifikasi Keaslian Dan Integritas Pesan." },
  { "en": "Apa Itu Infrastruktur Kunci Publik (PKI)?", "id": "Sistem Untuk Mengelola Kunci Digital." },
  { "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Yang Membatasi Lalu Lintas Jaringan." },
  { "en": "Apa Itu Sistem Deteksi Intrusi (IDS)?", "id": "Sistem Yang Mendeteksi Aktivitas Jaringan Mencurigakan." },
  { "en": "Apa Itu Sistem Pencegahan Intrusi (IPS)?", "id": "Sistem Yang Mendeteksi Dan Memblokir Intrusi." },
  { "en": "Apa Itu Honeypot?", "id": "Sistem Umpan Untuk Menarik Dan Menganalisis Penyerang." },
  { "en": "Apa Itu Keamanan Fisik?", "id": "Tindakan Melindungi Akses Fisik Ke Aset." },
  { "en": "Apa Itu Pengawasan Video?", "id": "Menggunakan Kamera Untuk Memantau Area." },
  { "en": "Apa Itu Kontrol Akses?", "id": "Sistem Membatasi Masuk Ke Area Terlarang." },
  { "en": "Apa Itu Biometrik?", "id": "Menggunakan Karakteristik Fisik Unik Untuk Otentikasi." },
  { "en": "Apa Itu Forensik Digital?", "id": "Proses Menginvestigasi Kejahatan Digital." },
  { "en": "Apa Itu Rantai Pengawasan?", "id": "Mendokumentasikan Penanganan Bukti Secara Kronologis." },
  { "en": "Apa Itu Pemulihan Data?", "id": "Proses Memulihkan Data Yang Hilang." },
  { "en": "Apa Itu Data Carving?", "id": "Memulihkan File Dari Data Mentah." },
  { "en": "Apa Itu Steganografi?", "id": "Menyembunyikan Data Di Dalam Data Lain." },
  { "en": "Apa Itu Kriptanalisis?", "id": "Ilmu Dan Seni Memecahkan Kode Kriptografi." },
  { "en": "Apa Itu Rekayasa Sosial?", "id": "Memanipulasi Orang Untuk Mendapatkan Informasi." },
  { "en": "Apa Itu Phishing?", "id": "Serangan Rekayasa Sosial Melalui Email." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Berbahaya." },
  { "en": "Apa Itu Virus?", "id": "Malware Yang Menempel Pada Program Lain." },
  { "en": "Apa Itu Worm?", "id": "Malware Yang Dapat Menyebar Sendiri." },
  { "en": "Apa Itu Trojan?", "id": "Malware Yang Menyamar Sebagai Perangkat Lunak." },
  { "en": "Apa Itu Ransomware?", "id": "Malware Yang Mengenkripsi File Dan Meminta Tebusan." },
  { "en": "Apa Itu Spyware?", "id": "Malware Yang Memata-matai Aktivitas Pengguna." },
  { "en": "Apa Itu Adware?", "id": "Malware Yang Menampilkan Iklan Tidak Diinginkan." },
  { "en": "Apa Itu Botnet?", "id": "Jaringan Komputer Zombie Yang Dikendalikan." },
  { "en": "Apa Itu Serangan Denial-of-Service (DoS)?", "id": "Membuat Layanan Tidak Tersedia Untuk Pengguna." },
  { "en": "Apa Itu Serangan Distributed DoS (DDoS)?", "id": "Serangan DoS Dari Banyak Sumber." },
  { "en": "Apa Itu Zero-Day Exploit?", "id": "Mengeksploitasi Kerentanan Yang Belum Diketahui." },
  { "en": "Apa Itu Patch Keamanan?", "id": "Pembaruan Perangkat Lunak Untuk Memperbaiki Kerentanan." },
  { "en": "Apa Itu Manajemen Patch?", "id": "Proses Menerapkan Patch Keamanan." },
  { "en": "Apa Itu Penilaian Kerentanan?", "id": "Proses Mengidentifikasi Kelemahan Keamanan." },
  { "en": "Apa Itu Pengujian Penetrasi?", "id": "Aktivitas Mensimulasikan Serangan Siber." },
  { "en": "Apa Itu Peretasan Etis?", "id": "Aktivitas Peretasan Dengan Izin." },
  { "en": "Apa Itu Tim Merah?", "id": "Tim Yang Berperan Sebagai Penyerang." },
  { "en": "Apa Itu Tim Biru?", "id": "Tim Yang Berperan Sebagai Pembela." },
  { "en": "Apa Itu Tim Ungu?", "id": "Tim Yang Menggabungkan Tim Merah Dan Biru." },
  { "en": "Apa Itu Respon Insiden?", "id": "Proses Menangani Pelanggaran Keamanan." },
  { "en": "Apa Itu Rencana Respon Insiden?", "id": "Prosedur Untuk Menangani Insiden Keamanan." },
  { "en": "Apa Itu Pemulihan Bencana?", "id": "Memulihkan Operasi Teknologi Informasi Setelah Bencana." },
  { "en": "Apa Itu Rencana Kelangsungan Bisnis?", "id": "Menjaga Fungsi Bisnis Tetap Berjalan." },
  { "en": "Apa Itu Cadangan Panas (Hot Backup)?", "id": "Sistem Cadangan Yang Selalu Beroperasi." },
  { "en": "Apa Itu Cadangan Dingin (Cold Backup)?", "id": "Sistem Cadangan Yang Harus Dinyalakan Manual." },
  { "en": "Apa Itu Perjanjian Tingkat Layanan (SLA)?", "id": "Kontrak Antara Penyedia Dan Pelanggan." },
  { "en": "Apa Itu Interupsi Momen (Momentary Interruption)?", "id": "Kehilangan Daya Kurang Dari Beberapa Detik." },
  { "en": "Apa Itu Indeks Kualitas Daya?", "id": "Metrik Tunggal Untuk Mengukur Kualitas Daya." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data Pengukuran Kualitas Daya." },
  { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Dari Waktu Ke Waktu." },
  { "en": "Apa Itu Penandaan Peristiwa?", "id": "Menandai Peristiwa Kualitas Daya Dalam Data." },
  { "en": "Apa Itu Korelasi Peristiwa?", "id": "Menghubungkan Peristiwa Yang Terjadi Bersamaan." },
  { "en": "Apa Itu Kausalitas?", "id": "Hubungan Sebab Akibat Antara Peristiwa." },
  { "en": "Apa Itu Jaringan Listrik Kapal?", "id": "Sistem Tenaga Listrik Terisolasi Di Atas Kapal." },
  { "en": "Apa Itu Jaringan Listrik Pesawat?", "id": "Sistem Tenaga Listrik Di Pesawat Terbang." },
  { "en": "Apa Itu Frekuensi 400 Hz?", "id": "Frekuensi Umum Dalam Sistem Tenaga Pesawat." },
  { "en": "Apa Itu Kereta Listrik?", "id": "Kereta Yang Digerakkan Oleh Tenaga Listrik." },
  { "en": "Apa Itu Catu Daya Tepi Jalan?", "id": "Menyediakan Daya Untuk Sistem Kereta Listrik." },
  { "en": "Apa Itu Pantograf?", "id": "Kontak Geser Untuk Mengambil Daya Listrik." },
  { "en": "Apa Itu Harmonik Yang Dihasilkan Kereta?", "id": "Dihasilkan Oleh Konverter Daya Traksi." },
  { "en": "Apa Itu Sistem Industri?", "id": "Pabrik Dan Fasilitas Manufaktur." },
  { "en": "Apa Itu Beban Tungku Busur?", "id": "Menyebabkan Flicker Dan Distorsi Tegangan Hebat." },
  { "en": "Apa Itu Beban Las?", "id": "Menyebabkan Sag Dan Fluktuasi Tegangan." },
  { "en": "Apa Itu Pusat Data?", "id": "Membutuhkan Kualitas Daya Sangat Andal." },
  { "en": "Apa Itu Sistem Catu Daya Ganda?", "id": "Dua Suplai Independen Untuk Redundansi." },
  { "en": "Apa Itu Saklar Transfer Statis (STS)?", "id": "Memindahkan Beban Antar Sumber Secara Cepat." },
  { "en": "Apa Itu Waktu Transfer?", "id": "Waktu Yang Dibutuhkan Untuk Beralih Sumber." },
  { "en": "Apa Itu Peralatan Medis?", "id": "Sangat Sensitif Terhadap Gangguan Kualitas Daya." },
  { "en": "Apa Itu Standar IEC (International Electrotechnical Commission) 60601?", "id": "Standar Keamanan Internasional Untuk Peralatan Medis." },
  { "en": "Apa Itu Sistem Tenaga Terisolasi?", "id": "Digunakan Di Area Medis Kritis." },
  { "en": "Apa Itu Line Isolation Monitor (LIM)?", "id": "Memantau Integritas Sistem Tenaga Terisolasi." },
  { "en": "Apa Itu Sistem Komersial?", "id": "Gedung Perkantoran Dan Pusat Belanja." },
  { "en": "Apa Itu Beban Pencahayaan?", "id": "Lampu LED (Light-Emitting Diode) Dan Fluoresen." },
  { "en": "Apa Itu Beban HVAC (Heating, Ventilation, and Air Conditioning)?", "id": "Sistem Pemanasan, Ventilasi, Dan Pendingin Udara." },
  { "en": "Apa Itu Sistem Perumahan?", "id": "Rumah Tinggal Dan Apartemen." },
  { "en": "Apa Itu Peralatan Elektronik Konsumen?", "id": "Televisi, Komputer, Dan Perangkat Audio." },
  { "en": "Apa Itu Penyearah Jembatan?", "id": "Input Dari Banyak Peralatan Elektronik." },
  { "en": "Apa Itu Faktor Crest Arus?", "id": "Rasio Puncak Arus Terhadap Nilai RMS." },
  { "en": "Apa Itu Arus Netral?", "id": "Arus Di Konduktor Netral." },
  { "en": "Bagaimana Harmonik Mempengaruhi Arus Netral?", "id": "Harmonik Triplen Menjumlah Di Konduktor Netral." },
  { "en": "Apa Itu Netral Berukuran Lebih?", "id": "Konduktor Netral Lebih Besar Dari Konduktor Fasa." },
  { "en": "Apa Itu Ekonomi Kualitas Daya?", "id": "Analisis Biaya Masalah Dan Solusi Kualitas Daya." },
  { "en": "Apa Itu Biaya Peralatan Mitigasi?", "id": "Biaya Untuk Memasang Solusi Kualitas Daya." },
  { "en": "Apa Itu Biaya Downtime?", "id": "Kerugian Finansial Akibat Produksi Berhenti." },
  { "en": "Apa Itu Biaya Kerusakan Peralatan?", "id": "Biaya Untuk Memperbaiki Atau Mengganti Peralatan." },
  { "en": "Apa Itu Biaya Energi?", "id": "Biaya Listrik Yang Dikonsumsi." },
  { "en": "Bagaimana Kualitas Daya Mempengaruhi Biaya Energi?", "id": "Harmonik Dan Faktor Daya Buruk Meningkatkannya." },
  { "en": "Apa Itu Penalti Faktor Daya?", "id": "Denda Dari Utilitas Akibat Faktor Daya Rendah." },
  { "en": "Apa Itu Analisis Manfaat-Biaya?", "id": "Membandingkan Biaya Dan Manfaat Suatu Proyek." },
  { "en": "Apa Itu Return on Investment (ROI)?", "id": "Ukuran Profitabilitas Suatu Investasi." },
  { "en": "Apa Itu Payback Period?", "id": "Waktu Yang Dibutuhkan Untuk Mengembalikan Investasi." },
  { "en": "Apa Itu Model Stokastik?", "id": "Model Yang Melibatkan Elemen Keacakan." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Metode Menggunakan Angka Acak Untuk Simulasi." },
  { "en": "Apa Itu Prediksi Kualitas Daya?", "id": "Memperkirakan Masalah Kualitas Daya Di Masa Depan." },
  { "en": "Apa Itu Perambatan Sag?", "id": "Bagaimana Sag Menyebar Melalui Jaringan Listrik." },
  { "en": "Apa Itu Area Kerentanan?", "id": "Area Geografis Yang Terkena Dampak Gangguan." },
  { "en": "Apa Itu Sistem Otomasi Distribusi?", "id": "Mengotomatiskan Operasi Jaringan Distribusi." },
  { "en": "Apa Itu Self-Healing Grid?", "id": "Jaringan Yang Dapat Memulihkan Diri Otomatis." },
  { "en": "Apa Itu Fault Location, Isolation, and Service Restoration (FLISR)?", "id": "Fungsi Kunci Dari Jaringan Self-Healing." },
  { "en": "Apa Itu Recloser Cerdas?", "id": "Recloser Dengan Kemampuan Komunikasi Dan Kontrol." },
  { "en": "Apa Itu Saklar Cerdas?", "id": "Saklar Dengan Kontrol Jarak Jauh." },
  { "en": "Apa Itu Sensor Cerdas?", "id": "Sensor Dengan Kemampuan Pemrosesan Dan Komunikasi." },
  { "en": "Apa Itu Jaringan Sensor?", "id": "Kumpulan Sensor Yang Terhubung Untuk Pemantauan." },
  { "en": "Apa Itu Analisis Data?", "id": "Memeriksa Data Untuk Menarik Kesimpulan." },
  { "en": "Apa Itu Visualisasi Data?", "id": "Menampilkan Data Secara Grafis Untuk Pemahaman." },
  { "en": "Apa Itu Dasbor?", "id": "Tampilan Visual Informasi Kunci." },
  { "en": "Apa Itu Sistem Informasi Geografis (GIS)?", "id": "Memetakan Data Kualitas Daya Secara Geografis." },
  { "en": "Apa Itu Gelombang Berjalan?", "id": "Pulsa Yang Merambat Di Sepanjang Saluran." },
  { "en": "Apa Itu Lokasi Gangguan Gelombang Berjalan?", "id": "Menggunakan Waktu Tiba Gelombang Untuk Lokasi." },
  { "en": "Apa Itu Osilasi Frekuensi Tinggi?", "id": "Nama Lain Untuk Transien Osilatoris." },
  { "en": "Apa Itu Resonansi Inti Transformator?", "id": "Osilasi Antara Induktansi Saturable Dan Kapasitansi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Impedansi Alami Dari Jalur Transmisi." },
  { "en": "Apa Itu Refleksi?", "id": "Gelombang Memantul Kembali Dari Ketidakcocokan." },
  { "en": "Apa Itu Transmisi?", "id": "Gelombang Melewati Batas Antar Media." },
  { "en": "Apa Itu Koefisien Refleksi?", "id": "Rasio Amplitudo Gelombang Pantul Terhadap Datang." },
  { "en": "Apa Itu Koefisien Transmisi?", "id": "Rasio Amplitudo Gelombang Transmisi Terhadap Datang." },
  { "en": "Apa Itu Diagram Tangga (Lattice Diagram)?", "id": "Alat Grafis Menganalisis Refleksi Gelombang." },
  { "en": "Apa Itu Petir?", "id": "Sumber Alami Transien Elektromagnetik." },
  { "en": "Apa Itu Sambaran Langsung?", "id": "Petir Menyambar Langsung Ke Fasilitas." },
  { "en": "Apa Itu Sambaran Tidak Langsung?", "id": "Petir Menyambar Dekat Fasilitas." },
  { "en": "Apa Itu Gelombang Petir?", "id": "Bentuk Gelombang Standar Transien Petir." },
  { "en": "Apa Itu Waktu Muka (Front Time)?", "id": "Waktu Naik Dari Gelombang Petir." },
  { "en": "Apa Itu Waktu Ekor (Tail Time)?", "id": "Waktu Turun Gelombang Petir." },
  { "en": "Apa Itu Proteksi Petir?", "id": "Sistem Untuk Melindungi Dari Sambaran Petir." },
  { "en": "Apa Itu Kawat Tanah (Shield Wire)?", "id": "Menangkap Sambaran Petir Di Saluran Transmisi." },
  { "en": "Apa Itu Arrester Surja?", "id": "Nama Lain Untuk Surge Arrester." },
  { "en": "Apa Itu Tegangan Sisa?", "id": "Tegangan Di Arrester Saat Menghantar." },
  { "en": "Apa Itu Energi Surja?", "id": "Energi Yang Diserap Oleh Arrester." },
  { "en": "Apa Itu Switching Elektromagnetik?", "id": "Transien Akibat Operasi Pemutusan." },
  { "en": "Apa Itu Restrike?", "id": "Busur Api Muncul Kembali Setelah Pemutusan." },
  { "en": "Apa Itu Current Chopping?", "id": "Pemutusan Arus Secara Tiba-tiba." },
  { "en": "Apa Itu Tegangan Pemulihan Transien (TRV)?", "id": "Tegangan Di Kontak Pemutus Sirkuit." },
  { "en": "Apa Itu Capacitor Switching?", "id": "Menghasilkan Transien Osilatoris Tegangan Tinggi." },
  { "en": "Apa Itu Magnifikasi Tegangan?", "id": "Peningkatan Tegangan Akibat Resonansi." },
  { "en": "Apa Itu Inrush Arus?", "id": "Nama Lain Untuk Inrush Current." },
  { "en": "Apa Itu Ferroresonansi?", "id": "Osilasi Non-linier Kompleks." },
  { "en": "Apa Itu Mode Fundamental?", "id": "Mode Osilasi Dasar Ferroresonansi." },
  { "en": "Apa Itu Mode Subharmonik?", "id": "Osilasi Di Bawah Frekuensi Fundamental." },
  { "en": "Apa Itu Mode Kuasi-Periodik?", "id": "Osilasi Dengan Beberapa Komponen Frekuensi." },
  { "en": "Apa Itu Mode Kacau (Chaotic)?", "id": "Osilasi Tidak Teratur Dan Tak Terduga." },
  { "en": "Apa Itu Inisiasi Ferroresonansi?", "id": "Peristiwa Yang Memulai Osilasi Ferroresonansi." }



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
