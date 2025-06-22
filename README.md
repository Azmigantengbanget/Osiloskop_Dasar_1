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


  { "en": "Alat ukur visualisasi sinyal listrik?", "id": "Osiloskop." },
  { "en": "Apa yang ditampilkan di osiloskop?", "id": "Bentuk gelombang sinyal." },
  { "en": "Sumbu vertikal (Y) merepresentasikan?", "id": "Tegangan (Voltage)." },
  { "en": "Sumbu horizontal (X) merepresentasikan?", "id": "Waktu (Time)." },
  { "en": "Layar osiloskop disebut?", "id": "Graticule." },
  { "en": "Garis-garis kotak pada layar?", "id": "Divisi (Division)." },
  { "en": "Bentuk gelombang yang umum diuji?", "id": "Kotak, sinus, segitiga." },
  { "en": "Fungsi utama osiloskop?", "id": "Analisis sinyal elektronik." },
  { "en": "Osiloskop tipe lama?", "id": "Osiloskop Analog." },
  { "en": "Osiloskop tipe modern?", "id": "Osiloskop Digital (DSO)." },
  { "en": "Kepanjangan DSO?", "id": "Digital Storage Oscilloscope." },
  { "en": "Keuntungan utama osiloskop digital?", "id": "Bisa menyimpan bentuk gelombang." },
  { "en": "Bagian kontrol untuk sumbu vertikal?", "id": "Vertical Control." },
  { "en": "Tombol pengatur skala tegangan?", "id": "Volts/Div." },
  { "en": "Satuan pada Volts/Div?", "id": "Volt per Divisi." },
  { "en": "Fungsi tombol Volts/Div?", "id": "Mengatur sensitivitas vertikal." },
  { "en": "Memutar Volts/Div searah jarum jam?", "id": "Memperkecil tampilan gelombang." },
  { "en": "Memutar Volts/Div berlawanan arah jarum jam?", "id": "Memperbesar tampilan gelombang." },
  { "en": "Tombol penggeser gelombang naik/turun?", "id": "Vertical Position." },
  { "en": "Mode kopling (coupling)?", "id": "AC, DC, Ground." },
  { "en": "Mode kopling DC menampilkan?", "id": "Komponen AC dan DC." },
  { "en": "Mode kopling AC menampilkan?", "id": "Hanya komponen AC." },
  { "en": "Mode kopling Ground (GND)?", "id": "Menampilkan garis referensi nol." },
  { "en": "Kanal input osiloskop?", "id": "CH1, CH2, dst." },
  { "en": "Bagian kontrol untuk sumbu horizontal?", "id": "Horizontal Control." },
  { "en": "Tombol pengatur skala waktu?", "id": "Time/Div atau Secs/Div." },
  { "en": "Satuan pada Time/Div?", "id": "Waktu per Divisi." },
  { "en": "Fungsi tombol Time/Div?", "id": "Mengatur kecepatan sapuan horizontal." },
  { "en": "Memutar Time/Div searah jarum jam?", "id": "Memperbesar (zoom in) waktu." },
  { "en": "Memutar Time/Div berlawanan arah jarum jam?", "id": "Memperkecil (zoom out) waktu." },
  { "en": "Dasar waktu osiloskop?", "id": "Timebase." },
  { "en": "Tombol penggeser gelombang kiri/kanan?", "id": "Horizontal Position." },
  { "en": "Bagian untuk menstabilkan tampilan?", "id": "Trigger Control." },
  { "en": "Fungsi utama trigger?", "id": "Sinkronisasi sapuan, menstabilkan gambar." },
  { "en": "Tampilan gelombang tidak stabil artinya?", "id": "Trigger tidak diatur benar." },
  { "en": "Tombol pengatur level tegangan trigger?", "id": "Trigger Level." },
  { "en": "Trigger saat sinyal naik?", "id": "Rising Edge." },
  { "en": "Trigger saat sinyal turun?", "id": "Falling Edge." },
  { "en": "Tombol pemilih tepi trigger?", "id": "Trigger Slope." },
  { "en": "Sumber sinyal untuk trigger?", "id": "Trigger Source." },
  { "en": "Contoh trigger source?", "id": "CH1, CH2, Line, EXT." },
  { "en": "Mode trigger yang selalu menampilkan jejak?", "id": "Mode Auto." },
  { "en": "Mode trigger yang menunggu event?", "id": "Mode Normal." },
  { "en": "Mode trigger untuk menangkap satu event?", "id": "Mode Single." },
  { "en": "Kabel penghubung ke osiloskop?", "id": "Probe." },
  { "en": "Konektor input di osiloskop?", "id": "Konektor BNC." },
  { "en": "Fungsi utama probe?", "id": "Menghubungkan sirkuit ke osiloskop." },
  { "en": "Probe standar memiliki pelemahan?", "id": "1X dan 10X." },
  { "en": "Fungsi atenuasi 10X pada probe?", "id": "Mengurangi pembebanan sirkuit." },
  { "en": "Jika pakai probe 10X, pembacaan tegangan?", "id": "Dikalikan sepuluh." },
  { "en": "Proses penyesuaian probe?", "id": "Kompensasi probe." },
  { "en": "Sinyal untuk kompensasi probe?", "id": "Sinyal kotak (Cal)." },
  { "en": "Kompensasi probe yang benar menampilkan?", "id": "Bentuk kotak sempurna." },
  { "en": "Kompensasi kurang (undercompensated)?", "id": "Sudut kotak membulat." },
  { "en": "Kompensasi lebih (overcompensated)?", "id": "Sudut kotak lancip (overshoot)." },
  { "en": "Tegangan dari puncak ke puncak?", "id": "Voltage Peak-to-Peak (Vpp)." },
  { "en": "Tegangan dari nol ke puncak?", "id": "Voltage Peak (Vp) / Amplitudo." },
  { "en": "Cara mengukur Vpp di layar?", "id": "Jumlah divisi vertikal x Volts/Div." },
  { "en": "Waktu untuk satu siklus gelombang?", "id": "Periode (T)." },
  { "en": "Cara mengukur periode di layar?", "id": "Jumlah divisi horizontal x Time/Div." },
  { "en": "Jumlah siklus per detik?", "id": "Frekuensi (f)." },
  { "en": "Satuan frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Rumus frekuensi dari periode?", "id": "f = 1 / T." },
  { "en": "Siklus kerja gelombang kotak?", "id": "Duty Cycle." },
  { "en": "Duty cycle 50% berarti?", "id": "Waktu HIGH dan LOW sama." },
  { "en": "Tombol pengatur fokus jejak?", "id": "Focus." },
  { "en": "Tombol pengatur intensitas cahaya jejak?", "id": "Intensity." },
  { "en": "Tombol untuk pengukuran otomatis?", "id": "Measure atau Cursors." },
  { "en": "Garis bantu untuk pengukuran manual?", "id": "Cursors." },
  { "en": "Kanal input untuk sinyal eksternal trigger?", "id": "EXT (External)." },
  { "en": "Menambahkan dua sinyal secara matematis?", "id": "Fungsi Math (ADD)." },
  { "en": "Melihat spektrum frekuensi sinyal?", "id": "Fungsi FFT." },
  { "en": "Kepanjangan FFT?", "id": "Fast Fourier Transform." },
  { "en": "Bandwidth osiloskop artinya?", "id": "Rentang frekuensi yang diukur." },
  { "en": "Sample rate osiloskop digital?", "id": "Seberapa cepat ADC mengambil sampel." },
  { "en": "Satuan sample rate?", "id": "Samples per second (S/s)." },
  { "en": "Kapasitas memori osiloskop digital?", "id": "Record Length." },
  { "en": "Jumlah bit resolusi vertikal ADC?", "id": "Vertical Resolution." },
  { "en": "Resolusi vertikal umum?", "id": "8 bit." },
  { "en": "Tombol untuk menyimpan gambar layar?", "id": "Save/Print." },
  { "en": "Tombol pengaturan default pabrik?", "id": "Default Setup." },
  { "en": "Tombol untuk bantuan?", "id": "Help." },
  { "en": "Mode tampilan XY?", "id": "Menampilkan sinyal CH1 vs CH2." },
  { "en": "Aplikasi mode XY?", "id": "Melihat figur Lissajous." },
  { "en": "Figur Lissajous untuk apa?", "id": "Analisis beda fasa/frekuensi." },
  { "en": "Tampilan titik-titik pada DSO?", "id": "Mode Dot." },
  { "en": "Tampilan garis sambung pada DSO?", "id": "Mode Vector." },
  { "en": "Impedansi input standar osiloskop?", "id": "1 Megaohm (1 MΩ)." },
  { "en": "Kapasitansi input osiloskop?", "id": "Beberapa puluh Picofarad (pF)." },
  { "en": "Efek pembebanan (loading effect)?", "id": "Probe mengubah sinyal sirkuit." },
  { "en": "Probe 10X mengurangi loading effect?", "id": "Ya, karena impedansi tinggi." },
  { "en": "Tombol untuk membalik fasa sinyal?", "id": "Invert." },
  { "en": "Apa itu 'ground loop'?", "id": "Gangguan karena beda potensial ground." },
  { "en": "Ujung ground pada probe?", "id": "Klip buaya (Alligator clip)." },
  { "en": "Ujung positif pada probe?", "id": "Ujung tajam (Probe tip)." },
  { "en": "Aksesoris probe untuk menyantol?", "id": "Hook tip." },
  { "en": "Pita berwarna pada probe untuk?", "id": "Identifikasi kanal." },
  { "en": "Menampilkan nilai rata-rata sinyal?", "id": "Pengukuran V_avg." },
  { "en": "Menampilkan nilai RMS sinyal?", "id": "Pengukuran V_rms." },
  { "en": "Mode akuisisi standar?", "id": "Sample Mode." },
  { "en": "Mode akuisisi untuk menangkap glitch?", "id": "Peak Detect Mode." },
  { "en": "Mode akuisisi untuk mengurangi noise?", "id": "Average Mode." },
  { "en": "Mode akuisisi untuk resolusi lebih tinggi?", "id": "High-Resolution Mode." },
  { "en": "Jeda waktu sebelum trigger diaktifkan lagi?", "id": "Trigger Holdoff." },
  { "en": "Tujuan trigger holdoff?", "id": "Menstabilkan gelombang kompleks." },
  { "en": "Trigger pada lebar pulsa tertentu?", "id": "Pulse Width Trigger." },
  { "en": "Trigger pada sinyal video analog?", "id": "Video Trigger." },
  { "en": "Trigger pada pola logika beberapa kanal?", "id": "Logic / Pattern Trigger." },
  { "en": "Trigger pada pulsa yang lebih kecil (runt)?", "id": "Runt Trigger." },
  { "en": "Trigger saat sinyal memasuki/keluar jendela?", "id": "Window Trigger." },
  { "en": "Kopling trigger untuk menolak frekuensi rendah?", "id": "HF Reject." },
  { "en": "Kopling trigger untuk menolak frekuensi tinggi?", "id": "LF Reject." },
  { "en": "Kopling trigger untuk mengurangi noise?", "id": "Noise Reject." },
  { "en": "Sumbu X pada mode FFT?", "id": "Frekuensi." },
  { "en": "Sumbu Y pada mode FFT?", "id": "Amplitudo (Magnitude)." },
  { "en": "Satuan magnitudo FFT?", "id": "Decibel (dB)." },
  { "en": "Fungsi 'windowing' pada FFT?", "id": "Mengurangi kebocoran spektral." },
  { "en": "Contoh fungsi windowing FFT?", "id": "Hanning, Flattop, Blackman." },
  { "en": "Mengukur beda waktu dengan kursor?", "id": "Delta Time (Δt)." },
  { "en": "Mengukur beda tegangan dengan kursor?", "id": "Delta Voltage (ΔV)." },
  { "en": "Waktu naik sinyal (10% ke 90%)?", "id": "Rise Time." },
  { "en": "Waktu turun sinyal (90% ke 10%)?", "id": "Fall Time." },
  { "en": "Pengukuran V_max?", "id": "Tegangan maksimum absolut." },
  { "en": "Pengukuran V_min?", "id": "Tegangan minimum absolut." },
  { "en": "Statistik pengukuran otomatis?", "id": "Mean, Std Dev, Min, Max." },
  { "en": "Probe yang butuh catu daya eksternal?", "id": "Probe Aktif (Active Probe)." },
  { "en": "Probe standar tanpa catu daya?", "id": "Probe Pasif (Passive Probe)." },
  { "en": "Keuntungan probe aktif?", "id": "Bandwidth tinggi, kapasitansi rendah." },
  { "en": "Probe untuk mengukur beda tegangan?", "id": "Differential Probe." },
  { "en": "Probe untuk mengukur arus?", "id": "Current Probe." },
  { "en": "Probe untuk mengukur tegangan sangat tinggi?", "id": "High-Voltage Probe." },
  { "en": "Efek sinyal palsu karena sampling lambat?", "id": "Aliasing." },
  { "en": "Syarat sampling untuk menghindari aliasing?", "id": "Teorema Nyquist." },
  { "en": "Laju sampling minimal menurut Nyquist?", "id": "Dua kali frekuensi tertinggi." },
  { "en": "Proses menggambar garis antar titik sampel?", "id": "Interpolasi." },
  { "en": "Jenis interpolasi paling umum?", "id": "Sin(x)/x." },
  { "en": "Osiloskop dengan input analog dan digital?", "id": "MSO (Mixed Signal Oscilloscope)." },
  { "en": "Kanal digital pada MSO disebut?", "id": "Kanal logika (Logic channels)." },
  { "en": "Tampilan jejak gelombang di layar?", "id": "Trace." },
  { "en": "Menyimpan pengaturan osiloskop saat ini?", "id": "Save Setup." },
  { "en": "Memuat pengaturan yang sudah disimpan?", "id": "Recall Setup." },
  { "en": "Menyimpan data bentuk gelombang?", "id": "Save Waveform." },
  { "en": "Format data gelombang yang umum?", "id": ".wfm, .csv, .bin." },
  { "en": "Mode akuisisi 'roll'?", "id": "Untuk sinyal berfrekuensi sangat rendah." },
  { "en": "Fungsi matematika integrasi?", "id": "Menghitung luas di bawah kurva." },
  { "en": "Fungsi matematika diferensiasi?", "id": "Menghitung laju perubahan (slope)." },
  { "en": "Pengujian otomatis 'lulus/gagal'?", "id": "Mask Testing." },
  { "en": "Topeng (mask) pada mask testing adalah?", "id": "Batas toleransi bentuk gelombang." },
  { "en": "Analisis sinyal protokol serial?", "id": "Serial Protocol Decoding." },
  { "en": "Contoh protokol yang bisa di-decode?", "id": "I2C, SPI, UART, CAN." },
  { "en": "Konektor USB di osiloskop untuk?", "id": "Menyimpan data, kontrol jarak jauh." },
  { "en": "Konektor LAN di osiloskop untuk?", "id": "Kontrol jarak jauh via jaringan." },
  { "en": "Standar perintah untuk instrumen?", "id": "SCPI." },
  { "en": "Kepanjangan SCPI?", "id": "Standard Commands for Programmable Instruments." },
  { "en": "Perangkat lunak untuk mengontrol osiloskop?", "id": "NI LabVIEW, MATLAB." },
  { "en": "Apa itu 'roll-off' filter?", "id": "Karakteristik atenuasi filter." },
  { "en": "Bandwidth osiloskop diukur pada?", "id": "Titik -3dB." },
  { "en": "Titik -3dB berarti sinyal teratenuasi?", "id": "Menjadi sekitar 70.7%." },
  { "en": "Aturan praktis bandwidth vs frekuensi?", "id": "Bandwidth 5x frekuensi sinyal." },
  { "en": "Menampilkan persistensi jejak gelombang?", "id": "Persistence mode." },
  { "en": "Tujuan mode persistensi?", "id": "Melihat glitch atau jitter." },
  { "en": "Tampilan 'color grade' pada persistensi?", "id": "Menunjukkan frekuensi kejadian." },
  { "en": "Kapasitansi parasitik probe?", "id": "Probe loading." },
  { "en": "Efek probe loading pada sirkuit?", "id": "Mengubah perilaku sirkuit." },
  { "en": "Cara mengurangi probe loading?", "id": "Gunakan probe 10X atau aktif." },
  { "en": "Tombol 'Autoset' atau 'Autoscale'?", "id": "Mengatur skala secara otomatis." },
  { "en": "Kapan 'Autoset' berguna?", "id": "Saat pertama kali melihat sinyal." },
  { "en": "Kalibrasi diri osiloskop?", "id": "Self-Calibration." },
  { "en": "Tujuan self-calibration?", "id": "Menjaga akurasi pengukuran." },
  { "en": "Grounding yang baik penting untuk?", "id": "Pengukuran akurat dan aman." },
  { "en": "Panjang kabel ground probe sebaiknya?", "id": "Se-pendek mungkin." },
  { "en": "Mengukur noise pada catu daya?", "id": "Power supply ripple measurement." },
  { "en": "Pembatasan bandwidth (bandwidth limit)?", "id": "Mengurangi noise frekuensi tinggi." },
  { "en": "Analisis daya (power analysis)?", "id": "Fitur khusus untuk analisis daya." },
  { "en": "Layar sentuh pada osiloskop modern?", "id": "Touchscreen display." },
  { "en": "Penyimpanan 'segmented memory'?", "id": "Menangkap banyak event trigger." },
  { "en": "Pencarian event pada gelombang tersimpan?", "id": "Waveform Search." },
  { "en": "Mengukur beda fasa dua gelombang?", "id": "Phase measurement." },
  { "en": "Gelombang referensi yang disimpan?", "id": "Reference waveform." },
  { "en": "Membandingkan sinyal langsung dengan referensi?", "id": "Ya, bisa dilakukan." },
  { "en": "Keluaran sinyal dari osiloskop?", "id": "AFG/Function Generator output." },
  { "en": "Fungsi AFG pada osiloskop?", "id": "Membangkitkan sinyal uji." },
  { "en": "Osiloskop berbasis PC?", "id": "PC-Based Oscilloscope." },
  { "en": "Keuntungan osiloskop berbasis PC?", "id": "Lebih murah, layar besar." },
  { "en": "Kekurangan osiloskop berbasis PC?", "id": "Butuh komputer, performa terbatas." },
  { "en": "Diagram konstelasi pada osiloskop?", "id": "Untuk analisis sinyal modulasi." },
  { "en": "Analisis jitter?", "id": "Mengukur variasi waktu sinyal." },
  { "en": "Histogram pada pengukuran?", "id": "Menunjukkan distribusi nilai." },
  { "en": "Mode 'XY' disebut juga?", "id": "Mode Lissajous." },
  { "en": "Jejak gelombang pada layar?", "id": "Waveform trace." },
  { "en": "Pelemahan sinyal disebut?", "id": "Attenuation." },
  { "en": "Penguatan sinyal disebut?", "id": "Amplification." },
  { "en": "Input impedansi tinggi (Hi-Z)?", "id": "Impedansi 1 Megaohm." },
  { "en": "Input impedansi rendah (50 Ohm)?", "id": "Untuk sinyal frekuensi tinggi." },
  { "en": "Kapan menggunakan input 50 Ohm?", "id": "Saat sirkuit berimpedansi 50 Ohm." },
  { "en": "Tampilan sinyal DC murni?", "id": "Garis lurus horizontal." },
  { "en": "Tampilan sinyal DC dengan noise?", "id": "Garis lurus 'berbulu' (fuzzy)." },
  { "en": "Tampilan sinyal modulasi amplitudo (AM)?", "id": "Sinus dengan amplop bervariasi." },
  { "en": "Tampilan sinyal modulasi frekuensi (FM)?", "id": "Sinus dengan kerapatan bervariasi." },
  { "en": "Tampilan sinyal PWM?", "id": "Gelombang kotak lebar bervariasi." },
  { "en": "Mengukur ripple catu daya?", "id": "Gunakan kopling AC." },
  { "en": "Pengaturan untuk mengukur ripple?", "id": "Kopling AC, bandwidth limit." },
  { "en": "Puncak yang melampaui level HIGH?", "id": "Overshoot." },
  { "en": "Osilasi setelah tepi sinyal?", "id": "Ringing." },
  { "en": "Mengukur overshoot menggunakan?", "id": "Kursor atau pengukuran otomatis." },
  { "en": "Tampilan tidak stabil, periksa apa?", "id": "Pengaturan trigger (level, slope)." },
  { "en": "Tidak ada tampilan, periksa apa?", "id": "Kopling, posisi vertikal, trigger." },
  { "en": "Bentuk sinyal terdistorsi, periksa apa?", "id": "Kompensasi probe." },
  { "en": "Pengukuran tegangan salah, periksa apa?", "id": "Atenuasi probe (1X/10X)." },
  { "en": "Tampilan berderau, coba apa?", "id": "Mode Average, periksa grounding." },
  { "en": "Mengapa kabel ground probe harus pendek?", "id": "Mengurangi induktansi dan noise." },
  { "en": "Aksesoris ground probe terbaik?", "id": "Pegas ground (spring ground)." },
  { "en": "Mengukur beda fasa dua sinyal?", "id": "Gunakan kursor atau pengukuran fasa." },
  { "en": "Mengukur waktu tunda dua sinyal?", "id": "Gunakan kursor waktu." },
  { "en": "Apa itu 'pre-trigger'?", "id": "Melihat sinyal sebelum trigger." },
  { "en": "Fitur pre-trigger hanya ada di?", "id": "Osiloskop digital (DSO)." },
  { "en": "Menyimpan dan memutar ulang akuisisi?", "id": "Mode History / Replay." },
  { "en": "Memicu trigger pada area di layar?", "id": "Zone Trigger." },
  { "en": "Analisis kualitas sinyal komunikasi data?", "id": "Eye Diagram Analysis." },
  { "en": "Bukaan 'mata' pada eye diagram lebar?", "id": "Kualitas sinyal bagus." },
  { "en": "Bukaan 'mata' pada eye diagram sempit?", "id": "Jitter atau noise tinggi." },
  { "en": "Sampling real-time adalah?", "id": "Menangkap sinyal dalam satu sapuan." },
  { "en": "Sampling 'equivalent-time' (ETS)?", "id": "Membangun sinyal dari banyak siklus." },
  { "en": "ETS hanya untuk sinyal?", "id": "Periodik atau berulang." },
  { "en": "Jika bandwidth osiloskop terlalu rendah?", "id": "Bentuk sinyal tidak akurat." },
  { "en": "Sinyal kotak akan terlihat seperti?", "id": "Sinyal sinus (jika bandwidth rendah)." },
  { "en": "Keuntungan resolusi vertikal 12-bit?", "id": "Detail tegangan lebih baik." },
  { "en": "Kelemahan probe pasif?", "id": "Kapasitansi lebih tinggi." },
  { "en": "Kelemahan probe aktif?", "id": "Mahal, butuh daya, rentan." },
  { "en": "Mode persistensi tak terbatas?", "id": "Infinite Persistence." },
  { "en": "Tujuan infinite persistence?", "id": "Menangkap event yang jarang terjadi." },
  { "en": "Diagram Bode?", "id": "Plot respons frekuensi." },
  { "en": "Osiloskop dengan analisis diagram Bode?", "id": "Frequency Response Analyzer (FRA)." },
  { "en": "Mengukur impedansi sirkuit?", "id": "Bisa dengan generator sinyal." },
  { "en": "Tampilan waterfall pada FFT?", "id": "Menunjukkan perubahan spektrum waktu." },
  { "en": "Mengukur arus dengan osiloskop?", "id": "Gunakan probe arus." },
  { "en": "Mengukur resistansi shunt dengan probe?", "id": "Bisa untuk mengukur arus." },
  { "en": "Menekan komponen DC pada sinyal?", "id": "Menggunakan kopling AC." },
  { "en": "Mengapa menekan komponen DC?", "id": "Untuk melihat sinyal AC kecil." },
  { "en": "Melihat detail kecil sinyal besar?", "id": "Gunakan zoom atau perbesar skala." },
  { "en": "Fungsi 'zoom' pada osiloskop?", "id": "Memperbesar bagian dari gelombang." },
  { "en": "Analisis logika pada MSO?", "id": "Melihat timing sinyal digital." },
  { "en": "Kanal logika MSO biasanya punya?", "id": "Threshold tegangan yang bisa diatur." },
  { "en": "Men-decode bus paralel?", "id": "Bisa dengan kanal logika MSO." },
  { "en": "Mencari event spesifik dalam data panjang?", "id": "Fungsi Search & Mark." },
  { "en": "Akurasi pengukuran osiloskop tergantung?", "id": "Kalibrasi dan pengaturan." },
  { "en": "Periode kalibrasi yang disarankan?", "id": "Biasanya satu tahun sekali." },
  { "en": "Apa itu 'linearity' osiloskop?", "id": "Akurasi di seluruh rentang skala." },
  { "en": "Sinyal 'glitch' adalah?", "id": "Pulsa pendek yang tidak normal." },
  { "en": "Menangkap glitch dengan?", "id": "Peak Detect atau Pulse Trigger." },
  { "en": "Menampilkan tegangan sebagai warna?", "id": "Mode Digital Phosphor / Intensity Grade." },
  { "en": "Warna paling terang pada DPO?", "id": "Bagian sinyal paling sering terjadi." },
  { "en": "Kepanjangan DPO?", "id": "Digital Phosphor Oscilloscope." },
  { "en": "Menghitung daya (V x I)?", "id": "Menggunakan fungsi Math." },
  { "en": "Menampilkan hasil ukur sebagai grafik?", "id": "Trend plot." },
  { "en": "Plot 'XY' untuk dua sinyal sinus?", "id": "Figur Lissajous." },
  { "en": "Lingkaran pada figur Lissajous?", "id": "Beda fasa 90 derajat." },
  { "en": "Garis lurus pada figur Lissajous?", "id": "Beda fasa 0 atau 180 derajat." },
  { "en": "Menyimpan screenshot layar ke USB?", "id": "Ya, fitur umum DSO." },
  { "en": "Menghubungkan osiloskop ke proyektor?", "id": "Melalui port video out/LAN." },
  { "en": "Peringatan 'Bandwidth Limit' di layar?", "id": "Fitur pembatas bandwidth aktif." },
  { "en": "Impedansi probe 10X?", "id": "10 Megaohm." },
  { "en": "Mengapa kompensasi probe penting?", "id": "Memastikan respons frekuensi datar." },
  { "en": "Sinyal kalibrasi probe biasanya?", "id": "1 kHz, gelombang kotak." },
  { "en": "Mengukur sinyal tanpa referensi ground?", "id": "Gunakan differential probe." },
  { "en": "Pengukuran 'floating' berbahaya jika?", "id": "Menggunakan probe pasif biasa." },
  { "en": "Mengapa pengukuran 'floating' berbahaya?", "id": "Bisa menyebabkan korsleting." },
  { "en": "Osiloskop portabel bertenaga baterai?", "id": "Handheld Oscilloscope." },
  { "en": "Keuntungan osiloskop handheld?", "id": "Portabel, terisolasi dari ground." },
  { "en": "Fungsi 'fine' pada tombol skala?", "id": "Pengaturan skala lebih presisi." },
  { "en": "Menampilkan beberapa siklus gelombang?", "id": "Kurangi skala Time/Div." },
  { "en": "Melihat satu pulsa lebih detail?", "id": "Tambah skala Time/Div." },
  { "en": "Melihat amplitudo lebih detail?", "id": "Kurangi skala Volts/Div." },
  { "en": "Jejak sinyal terlalu tinggi/rendah?", "id": "Atur dengan posisi vertikal." },
  { "en": "Titik trigger tidak terlihat?", "id": "Atur dengan posisi horizontal." },
  { "en": "Tombol 'Force Trigger'?", "id": "Memaksa trigger sekali." },
  { "en": "Indikator 'Trig'd' di layar?", "id": "Osiloskop berhasil ter-trigger." },
  { "en": "Ikon '?' di osiloskop?", "id": "Menu bantuan (help)." },
  { "en": "Input 'probe comp' juga bisa jadi?", "id": "Output sinyal kotak." },
  { "en": "Waktu dari trigger hingga awal layar?", "id": "Trigger delay." },
  { "en": "Melihat data setelah trigger?", "id": "Post-trigger data." },
  { "en": "Melihat data sebelum trigger?", "id": "Pre-trigger data." },
  { "en": "Kursor yang menempel pada gelombang?", "id": "Waveform cursors." },
  { "en": "Kursor yang bebas di layar?", "id": "Screen cursors." },
  { "en": "Resolusi waktu osiloskop ditentukan?", "id": "Oleh sample rate." },
  { "en": "Mengapa penting mengetahui bandwidth probe?", "id": "Agar tidak membatasi osiloskop." },
  { "en": "Kapan menggunakan kopling DC?", "id": "Saat melihat total tegangan." },
  { "en": "Kapan menggunakan kopling AC?", "id": "Saat melihat sinyal AC kecil." },
  { "en": "Kapan menggunakan mode 'Normal Trigger'?", "id": "Saat sinyal tidak periodik." },
  { "en": "Kapan menggunakan mode 'Auto Trigger'?", "id": "Saat sinyal periodik stabil." },
  { "en": "Osiloskop dengan memori sangat dalam?", "id": "Deep Memory Oscilloscope." },
  { "en": "Plot distribusi nilai pengukuran?", "id": "Histogram." },
  { "en": "Histogram tegangan menunjukkan?", "id": "Distribusi level tegangan." },
  { "en": "Grafik perubahan pengukuran dari waktu ke waktu?", "id": "Trend Plot." },
  { "en": "Analisis kepatuhan sinyal terhadap standar?", "id": "Compliance Testing." },
  { "en": "Pengujian otomatis dengan 'topeng' virtual?", "id": "Mask Testing." },
  { "en": "Variasi waktu pada tepi sinyal?", "id": "Jitter." },
  { "en": "Jitter yang bersifat acak?", "id": "Random Jitter (RJ)." },
  { "en": "Jitter yang bersifat dapat diprediksi?", "id": "Deterministic Jitter (DJ)." },
  { "en": "Analisis rugi-rugi daya pada saklar?", "id": "Switching Loss Analysis." },
  { "en": "Analisis area kerja aman transistor?", "id": "Safe Operating Area (SOA) Test." },
  { "en": "Analisis distorsi harmonik?", "id": "Harmonics Analysis." },
  { "en": "Probe untuk kanal logika MSO?", "id": "Logic Probe." },
  { "en": "Probe untuk deteksi medan elektromagnetik?", "id": "Near-field probe." },
  { "en": "Tujuan near-field probe?", "id": "Debugging EMI (interferensi)." },
  { "en": "Aksesoris untuk menyerap pantulan sinyal?", "id": "Terminator 50 Ohm." },
  { "en": "Bagian input osiloskop penguat/pelemah sinyal?", "id": "Front-end amplifier/attenuator." },
  { "en": "Komponen utama DSO pengubah sinyal?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Memori penyimpan titik sampel?", "id": "Acquisition Memory." },
  { "en": "Hubungan record length, sample rate, waktu?", "id": "Waktu = Record Length / Sample Rate." },
  { "en": "Waktu saat osiloskop tidak mengakuisisi?", "id": "Dead time atau Blind time." },
  { "en": "Ukuran performa dinamis ADC?", "id": "ENOB." },
  { "en": "Kepanjangan ENOB?", "id": "Effective Number of Bits." },
  { "en": "Menerjemahkan data bus I2C?", "id": "I2C protocol decoding." },
  { "en": "Trigger pada alamat I2C tertentu?", "id": "Ya, bisa dilakukan." },
  { "en": "Menerjemahkan data bus SPI?", "id": "SPI protocol decoding." },
  { "en": "Trigger pada data SPI tertentu?", "id": "Ya, bisa dilakukan." },
  { "en": "Menerjemahkan data bus CAN?", "id": "CAN protocol decoding." },
  { "en": "Standar kontrol instrumen via LAN?", "id": "LXI (LAN eXtensions for Instrumentation)." },
  { "en": "Mengontrol osiloskop dari browser?", "id": "Web interface." },
  { "en": "Pembaruan perangkat lunak osiloskop?", "id": "Firmware update." },
  { "en": "Mengapa firmware perlu diupdate?", "id": "Perbaikan bug, fitur baru." },
  { "en": "Kedekatan hasil ukur dengan nilai sebenarnya?", "id": "Akurasi (Accuracy)." },
  { "en": "Kemampuan mengulang hasil ukur yang sama?", "id": "Presisi (Precision)." },
  { "en": "Derau yang dihasilkan oleh osiloskop sendiri?", "id": "Vertical noise atau noise floor." },
  { "en": "Pengaruh suhu pada pengukuran?", "id": "Bisa menyebabkan pergeseran (drift)." },
  { "en": "Fungsi output 'Cal' atau 'Probe Comp'?", "id": "Kompensasi probe, referensi sinyal." },
  { "en": "Menyimpan data pengukuran ke file teks?", "id": "Simpan sebagai file CSV." },
  { "en": "Menyimpan pengaturan lengkap osiloskop?", "id": "Simpan sebagai file setup." },
  { "en": "Osiloskop dengan resolusi vertikal tinggi?", "id": "High-Definition Oscilloscope." },
  { "en": "Resolusi umum High-Definition Oscilloscope?", "id": "10-bit atau 12-bit." },
  { "en": "Anotasi pada bentuk gelombang?", "id": "Menambahkan label atau catatan." },
  { "en": "Mode 'infinite persistence' menampilkan?", "id": "Semua jejak gelombang." },
  { "en": "Kabel BNC digunakan untuk?", "id": "Menghubungkan sinyal frekuensi tinggi." },
  { "en": "Impedansi kabel BNC yang umum?", "id": "50 Ohm atau 75 Ohm." },
  { "en": "Osiloskop sampling?", "id": "Untuk sinyal periodik frekuensi sangat tinggi." },
  { "en": "Teknik sampling pada osiloskop sampling?", "id": "Equivalent-time sampling (ETS)." },
  { "en": "Pelindung dari medan magnet?", "id": "Mu-metal shielding." },
  { "en": "Antarmuka untuk kontrol instrumen lama?", "id": "GPIB." },
  { "en": "Kepanjangan GPIB?", "id": "General Purpose Interface Bus." },
  { "en": "Mengukur resistansi dengan osiloskop?", "id": "Tidak bisa secara langsung." },
  { "en": "Mengukur kapasitansi dengan osiloskop?", "id": "Tidak bisa secara langsung." },
  { "en": "Menampilkan waktu tunda antar kanal?", "id": "Channel-to-channel skew." },
  { "en": "Tujuan de-skew pada probe?", "id": "Menyamakan waktu tunda." },
  { "en": "Pembesaran vertikal dan horizontal?", "id": "Zoom." },
  { "en": "Menampilkan sinyal dalam domain Z?", "id": "Tidak bisa dilakukan." },
  { "en": "Pengukuran 'pulse oximetry' menggunakan?", "id": "Prinsip optik." },
  { "en": "Osiloskop untuk debugging otomotif?", "id": "Automotive oscilloscope." },
  { "en": "Fitur khusus osiloskop otomotif?", "id": "Tes sensor, decoding bus CAN." },
  { "en": "Mengukur 'slew rate' sebuah op-amp?", "id": "Ya, dengan sinyal kotak." },
  { "en": "Slew rate adalah?", "id": "Laju perubahan tegangan maksimum." },
  { "en": "Osiloskop dengan baterai internal?", "id": "Portabel atau handheld." },
  { "en": "Standar keamanan untuk instrumen ukur?", "id": "CAT ratings (I, II, III, IV)." },
  { "en": "CAT rating untuk pengukuran elektronik?", "id": "CAT I." },
  { "en": "CAT rating untuk pengukuran stop kontak?", "id": "CAT II." },
  { "en": "Ground sasis osiloskop terhubung ke?", "id": "Ground listrik (bumi)." },
  { "en": "Mengukur sinyal 'AC line' langsung?", "id": "Gunakan differential probe." },
  { "en": "Bahaya mengukur 'AC line' dengan probe pasif?", "id": "Risiko korsleting dan kejut listrik." },
  { "en": "Mode tampilan 'YT'?", "id": "Mode standar (Tegangan vs Waktu)." },
  { "en": "Menganalisis diagram 'eye' untuk?", "id": "Kualitas sinyal data digital." },
  { "en": "Parameter yang diukur dari 'eye diagram'?", "id": "Jitter, noise margin, eye height." },
  { "en": "Aplikasi osiloskop dalam audio?", "id": "Melihat bentuk gelombang suara." },
  { "en": "Aplikasi osiloskop dalam radio?", "id": "Melihat sinyal termodulasi." },
  { "en": "Aplikasi osiloskop dalam perbaikan TV?", "id": "Melihat sinyal video dan sinkronisasi." },
  { "en": "Menampilkan sinyal digital sebagai bus?", "id": "Ya, pada MSO." },
  { "en": "Nilai bus ditampilkan dalam format?", "id": "Heksadesimal, biner, atau desimal." },
  { "en": "Trigger pada kondisi 'setup and hold'?", "id": "Setup/Hold Trigger." },
  { "en": "Tujuan setup/hold trigger?", "id": "Mencari pelanggaran timing." },
  { "en": "Efek 'aliasing' pada spektrum FFT?", "id": "Frekuensi tinggi tampak sebagai rendah." },
  { "en": "Mencari sinyal 'runt'?", "id": "Gunakan runt trigger." },
  { "en": "Osiloskop virtual?", "id": "Software yang mensimulasikan osiloskop." },
  { "en": "Osiloskop untuk edukasi?", "id": "Biasanya lebih sederhana dan murah." },
  { "en": "Konektor 'trigger out'?", "id": "Output sinyal trigger internal." },
  { "en": "Tujuan 'trigger out'?", "id": "Sinkronisasi dengan instrumen lain." },
  { "en": "Waktu pemanasan osiloskop?", "id": "Untuk mencapai akurasi spesifikasi." },
  { "en": "Lama waktu pemanasan yang disarankan?", "id": "Sekitar 20-30 menit." },
  { "en": "Penyimpanan data non-volatil?", "id": "Data tetap ada saat mati." },
  { "en": "Pengukuran frekuensi dengan presisi tinggi?", "id": "Gunakan frequency counter." },
  { "en": "Osiloskop lebih baik untuk analisis?", "id": "Bentuk gelombang domain waktu." },
  { "en": "Spectrum analyzer lebih baik untuk?", "id": "Analisis domain frekuensi." },
  { "en": "Menampilkan rata-rata dari banyak akuisisi?", "id": "Mode Average." },
  { "en": "Menampilkan 'amplop' dari banyak akuisisi?", "id": "Mode Envelope atau Peak Detect." },
  { "en": "Kursor yang melacak pengukuran otomatis?", "id": "Measurement cursors." },
  { "en": "Tampilan 'gated' pada pengukuran?", "id": "Mengukur hanya pada area tertentu." },
  { "en": "Apa itu 'linearity error'?", "id": "Kesalahan pada skala vertikal." },
  { "en": "Apa itu 'offset error'?", "id": "Pergeseran DC pada sinyal." },
  { "en": "Apa itu 'gain error'?", "id": "Kesalahan pada faktor penguatan." },
  { "en": "Hubungan rise time dan bandwidth?", "id": "Bandwidth ≈ 0.35 / Rise Time." },
  { "en": "Fungsi utama tombol Volts/Div?", "id": "Mengatur skala tegangan." },
  { "en": "Fungsi utama tombol Time/Div?", "id": "Mengatur skala waktu." },
  { "en": "Fungsi utama tombol Trigger Level?", "id": "Menentukan titik pemicu." },
  { "en": "Fungsi utama tombol Position?", "id": "Menggeser posisi jejak." },
  { "en": "Kopling DC melewatkan sinyal?", "id": "AC dan DC." },
  { "en": "Kopling AC memblokir sinyal?", "id": "Komponen DC." },
  { "en": "Tujuan utama kompensasi probe?", "id": "Memastikan pengukuran akurat." },
  { "en": "Hubungan frekuensi dan periode?", "id": "Frekuensi = 1 / Periode." },
  { "en": "Kelebihan osiloskop dari multimeter?", "id": "Menampilkan bentuk gelombang." },
  { "en": "Multimeter mengukur apa?", "id": "Nilai RMS atau rata-rata." },
  { "en": "Osiloskop mengukur domain?", "id": "Domain waktu (time domain)." },
  { "en": "Spectrum Analyzer mengukur domain?", "id": "Domain frekuensi (frequency domain)." },
  { "en": "Osiloskop vs Logic Analyzer?", "id": "Detail analog vs banyak kanal digital." },
  { "en": "Efek probe membebani sirkuit?", "id": "Circuit loading." },
  { "en": "Bagaimana probe membebani sirkuit?", "id": "Resistansi, kapasitansi, induktansi." },
  { "en": "Ukuran penolakan sinyal 'common-mode'?", "id": "CMRR." },
  { "en": "Kepanjangan CMRR?", "id": "Common Mode Rejection Ratio." },
  { "en": "CMRR penting untuk probe jenis?", "id": "Differential probe." },
  { "en": "Sensitivitas vertikal adalah?", "id": "Pengaturan Volts/Div terkecil." },
  { "en": "Akurasi dasar waktu horizontal?", "id": "Timebase accuracy." },
  { "en": "Pengukuran hanya di area tertentu?", "id": "Gated measurement." },
  { "en": "Menguji dioda dengan mode XY?", "id": "Bisa, untuk melihat kurva I-V." },
  { "en": "Jitter pada sinyal trigger?", "id": "Trigger jitter." },
  { "en": "Penyimpanan event trigger berurutan cepat?", "id": "Segmented memory." },
  { "en": "Fungsi matematika akar kuadrat?", "id": "SQRT." },
  { "en": "Aturan praktis ground probe?", "id": "Jaga kabel ground sependek mungkin." },
  { "en": "Cara menemukan jejak yang hilang?", "id": "Gunakan Autoset atau Default Setup." },
  { "en": "Kapan menggunakan input impedansi 50Ω?", "id": "Untuk sinyal RF atau high-speed." },
  { "en": "Penemu tabung sinar katoda?", "id": "Ferdinand Braun." },
  { "en": "Kepanjangan CRT?", "id": "Cathode Ray Tube." },
  { "en": "Jejak sinyal di layar?", "id": "Trace." },
  { "en": "Representasi grafis dari sinyal?", "id": "Waveform." },
  { "en": "Fenomena fisik yang diukur?", "id": "Signal." },
  { "en": "Mengapa trigger Normal lebih baik untuk 'single shot'?", "id": "Hanya memicu jika ada event." },
  { "en": "Mengapa trigger Auto tidak ideal untuk 'single shot'?", "id": "Akan terus menyapu layar." },
  { "en": "Tampilan alias karena sample rate rendah?", "id": "Aliasing." },
  { "en": "Bagaimana cara menghindari aliasing?", "id": "Naikkan sample rate." },
  { "en": "Koneksi osiloskop ke PC?", "id": "USB atau LAN." },
  { "en": "Gelombang sinus yang terdistorsi memiliki?", "id": "Harmonik." },
  { "en": "Analisis harmonik menggunakan?", "id": "Fungsi FFT." },
  { "en": "Bandwidth didefinisikan pada titik?", "id": "-3dB." },
  { "en": "Tegangan pada titik -3dB?", "id": "70.7% dari tegangan asli." },
  { "en": "Daya pada titik -3dB?", "id": "Setengah dari daya asli." },
  { "en": "Rumus praktis bandwidth dan rise time?", "id": "BW ≈ 0.35 / t_rise." },
  { "en": "Resolusi 8-bit berarti ada berapa level?", "id": "256 level." },
  { "en": "Resolusi 12-bit berarti ada berapa level?", "id": "4096 level." },
  { "en": "Tampilan 'stair-stepping' pada gelombang?", "id": "Disebabkan oleh resolusi terbatas." },
  { "en": "Fungsi 'averaging' mengurangi noise acak?", "id": "Ya." },
  { "en": "Fungsi 'averaging' tidak bisa mengurangi?", "id": "Noise periodik." },
  { "en": "Mode 'envelope' menangkap?", "id": "Nilai min dan max gelombang." },
  { "en": "Tombol 'single' untuk apa?", "id": "Menangkap satu event trigger." },
  { "en": "Tombol 'run/stop' untuk apa?", "id": "Menjalankan/menghentikan akuisisi." },
  { "en": "Warna hijau pada BNC osiloskop?", "id": "Biasanya ground sasis." },
  { "en": "Ground sasis terhubung ke?", "id": "Pin ground pada steker listrik." },
  { "en": "Mengukur sinyal tanpa ground sirkuit?", "id": "Berbahaya, butuh probe differential." },
  { "en": "Probe dengan ujung sangat kecil?", "id": "Probe jarum (needle-point)." },
  { "en": "Pengukuran 'glitch' menggunakan?", "id": "Mode Peak Detect." },
  { "en": "Pengukuran lebar pulsa positif?", "id": "Positive pulse width." },
  { "en": "Pengukuran lebar pulsa negatif?", "id": "Negative pulse width." },
  { "en": "Pengukuran V_rms untuk gelombang apa?", "id": "Gelombang AC." },
  { "en": "V_rms gelombang sinus?", "id": "V_peak / akar kuadrat 2." },
  { "en": "Tampilan intensitas-gradasi disebut juga?", "id": "Digital Phosphor (DPO)." },
  { "en": "Fungsi matematika 'absolute'?", "id": "Membuat semua nilai positif." },
  { "en": "Fungsi 'invert' pada kanal?", "id": "Membalik fasa sinyal 180 derajat." },
  { "en": "Kapan 'invert' berguna?", "id": "Saat melihat sinyal diferensial." },
  { "en": "Bagaimana melihat sinyal diferensial?", "id": "CH1 - CH2 (setelah invert CH2)." },
  { "en": "Derau yang amplitudonya proporsional frekuensi?", "id": "Pink noise." },
  { "en": "Derau yang spektrumnya datar?", "id": "White noise." },
  { "en": "Mengukur 'slew rate' di osiloskop?", "id": "Mengukur kemiringan (slope) tepi." },
  { "en": "Firmware osiloskop disimpan di?", "id": "Memori non-volatil." },
  { "en": "Pengujian kepatuhan standar USB?", "id": "USB compliance testing." },
  { "en": "Pengujian kepatuhan standar Ethernet?", "id": "Ethernet compliance testing." },
  { "en": "Protokol otomotif umum?", "id": "CAN, LIN, FlexRay." },
  { "en": "Osiloskop dengan generator fungsi?", "id": "Memiliki output AFG." },
  { "en": "Fungsi 'gating' pada counter?", "id": "Menghitung pulsa dalam periode waktu." },
  { "en": "Akurasi frekuensi osiloskop tergantung?", "id": "Kristal osilator internal." },
  { "en": "Kestabilan kristal osilator diukur dalam?", "id": "ppm (parts per million)." },
  { "en": "Tampilan 'persistence' mirip dengan?", "id": "Layar fosfor osiloskop analog." },
  { "en": "Slot kunci di belakang osiloskop?", "id": "Kensington lock." },
  { "en": "Mengapa osiloskop punya kipas?", "id": "Untuk pendinginan komponen internal." },
  { "en": "Mengukur waktu antara dua tepi naik?", "id": "Bisa untuk mengukur periode." },
  { "en": "Mengukur 'preshoot'?", "id": "Anomali sebelum tepi sinyal." },
  { "en": "Bentuk gelombang ideal?", "id": "Tidak memiliki distorsi." },
  { "en": "Bentuk gelombang di dunia nyata?", "id": "Selalu memiliki noise/distorsi." },
  { "en": "Tujuan utama seorang insinyur dengan osiloskop?", "id": "Debugging dan verifikasi desain." },
  { "en": "Keterampilan paling penting menggunakan osiloskop?", "id": "Mengatur trigger dengan benar." },
  { "en": "Sinyal referensi 10 MHz?", "id": "Untuk sinkronisasi antar instrumen." },
  { "en": "Port 'Ref In' pada osiloskop?", "id": "Input untuk referensi clock eksternal." },
  { "en": "Port 'Ref Out' pada osiloskop?", "id": "Output referensi clock internal." },
  { "en": "Tampilan 'magnify'?", "id": "Sama seperti fungsi zoom." },
  { "en": "Interpolasi 'linear'?", "id": "Menghubungkan titik dengan garis lurus." },
  { "en": "Interpolasi 'Sin(x)/x' lebih baik untuk?", "id": "Sinyal sinus dan periodik." },
  { "en": "Resolusi waktu adalah?", "id": "Interval waktu antar sampel." },
  { "en": "Kebalikan dari sample rate?", "id": "Resolusi waktu." },
  { "en": "Mode 'XY' membutuhkan berapa kanal?", "id": "Dua kanal." },
  { "en": "Sinyal input CH1 menggerakkan?", "id": "Sumbu X (pada mode XY)." },
  { "en": "Sinyal input CH2 menggerakkan?", "id": "Sumbu Y (pada mode XY)." },
  { "en": "Tiga fungsi utama osiloskop?", "id": "Visualisasi, ukur, analisis sinyal." },
  { "en": "Tampilan layar osiloskop analog?", "id": "Fosfor (phosphor screen)." },
  { "en": "Tampilan layar osiloskop digital?", "id": "LCD atau LED." },
  { "en": "Kecepatan osiloskop menggambar ulang gelombang?", "id": "Waveform Update Rate." },
  { "en": "Satuan waveform update rate?", "id": "wfms/s." },
  { "en": "Update rate tinggi berguna untuk?", "id": "Menangkap event jarang (glitch)." },
  { "en": "Trigger yang dibuat dari sirkuit analog?", "id": "Analog trigger." },
  { "en": "Trigger yang dibuat dari data digital?", "id": "Digital trigger." },
  { "en": "Keuntungan digital trigger?", "id": "Lebih banyak opsi trigger kompleks." },
  { "en": "Probe arus AC fleksibel?", "id": "Rogowski coil." },
  { "en": "Alat untuk injeksi pulsa logika?", "id": "Logic pulser." },
  { "en": "Alat untuk melacak jalur arus?", "id": "Current tracer." },
  { "en": "Konektor BNC bentuk 'T'?", "id": "BNC T-connector." },
  { "en": "Fungsi BNC T-connector?", "id": "Membagi atau menggabung sinyal." },
  { "en": "Ketidakpastian dalam setiap pengukuran?", "id": "Measurement uncertainty." },
  { "en": "Prinsip penting sebelum pengukuran akurat?", "id": "Lakukan kalibrasi." },
  { "en": "Keterbatasan utama osiloskop?", "id": "Bandwidth dan sample rate." },
  { "en": "Peringkat keamanan untuk tegangan listrik?", "id": "CAT rating." },
  { "en": "Plot hasil pengukuran vs waktu?", "id": "Trend plot." },
  { "en": "Plot distribusi statistik pengukuran?", "id": "Histogram." },
  { "en": "Menggunakan kursor pada tampilan FFT?", "id": "Untuk mengukur frekuensi & magnitudo." },
  { "en": "Sinyal clock untuk ADC?", "id": "Sample clock." },
  { "en": "Men-debug bus SPI?", "id": "Lihat sinyal SCLK, MOSI, MISO, CS." },
  { "en": "Men-debug bus I2C?", "id": "Lihat sinyal SDA dan SCL." },
  { "en": "Melihat urutan 'power-up' catu daya?", "id": "Gunakan beberapa kanal osiloskop." },
  { "en": "Mengukur waktu 'startup' osilator kristal?", "id": "Gunakan mode single trigger." },
  { "en": "Menemukan sumber noise pada PCB?", "id": "Gunakan near-field probe." },
  { "en": "Kepanjangan V_avg?", "id": "Voltage Average (rata-rata)." },
  { "en": "Kepanjangan V_rms?", "id": "Voltage Root Mean Square." },
  { "en": "Kepanjangan MSO?", "id": "Mixed Signal Oscilloscope." },
  { "en": "Kepanjangan AFG?", "id": "Arbitrary Function Generator." },
  { "en": "Osiloskop dengan AFG bisa?", "id": "Membangkitkan bentuk gelombang." },
  { "en": "Memori internal osiloskop disebut?", "id": "Acquisition memory." },
  { "en": "Semakin dalam memori, semakin?", "id": "Lama waktu akuisisi." },
  { "en": "Teknologi layar sentuh memudahkan?", "id": "Navigasi dan zoom." },
  { "en": "Firmware osiloskop adalah?", "id": "Sistem operasi internalnya." },
  { "en": "Pemicu 'dropout'?", "id": "Memicu saat sinyal hilang." },
  { "en": "Pemicu 'runt'?", "id": "Memicu pada pulsa abnormal." },
  { "en": "Gelombang sinus murni pada FFT?", "id": "Satu puncak (spike) tunggal." },
  { "en": "Gelombang kotak pada FFT?", "id": "Puncak di frekuensi ganjil." },
  { "en": "Jejak pada layar CRT dibuat oleh?", "id": "Tembakan elektron." },
  { "en": "Pembelokan vertikal CRT oleh?", "id": "Tegangan sinyal input." },
  { "en": "Pembelokan horizontal CRT oleh?", "id": "Tegangan gigi gergaji (sawtooth)." },
  { "en": "Frekuensi tegangan gigi gergaji menentukan?", "id": "Skala Time/Div." },
  { "en": "Mode 'chop' pada osiloskop analog?", "id": "Mengganti antar kanal dengan cepat." },
  { "en": "Mode 'alt' (alternate) pada osiloskop analog?", "id": "Menggambar tiap kanal bergantian." },
  { "en": "Masalah paralaks pada layar CRT?", "id": "Kesalahan baca karena sudut pandang." },
  { "en": "Kelebihan osiloskop analog?", "id": "Tampilan real-time, update rate instan." },
  { "en": "Kelemahan osiloskop analog?", "id": "Tidak bisa menyimpan, fitur terbatas." },
  { "en": "Impedansi probe lebih tinggi dari sirkuit?", "id": "Untuk mengurangi efek pembebanan." },
  { "en": "Kapasitor variabel kecil di probe?", "id": "Untuk kompensasi." },
  { "en": "Menghubungkan output AFG ke input CH1?", "id": "Untuk melihat sinyal yang dibangkitkan." },
  { "en": "Penyimpanan pengaturan kustom?", "id": "User-defined setup." },
  { "en": "Sinyal 'trigger out' berbentuk?", "id": "Pulsa digital." },
  { "en": "Menggunakan multimeter untuk mengukur Vpp?", "id": "Tidak bisa secara langsung." },
  { "en": "Pengukuran otomatis 'overshoot' dalam persen?", "id": "Ya, fitur umum." },
  { "en": "Pengukuran otomatis 'preshoot'?", "id": "Ya, pada beberapa osiloskop." },
  { "en": "Bandwidth probe harus?", "id": "Sama atau lebih tinggi dari osiloskop." },
  { "en": "Mengapa membersihkan ujung probe penting?", "id": "Untuk kontak listrik yang baik." },
  { "en": "Menghindari 'ground loop' dengan?", "id": "Satu titik ground bersama." },
  { "en": "Pengukuran diferensial menolak?", "id": "Sinyal common-mode." },
  { "en": "Contoh sinyal common-mode?", "id": "Noise listrik 50/60 Hz." },
  { "en": "Jejak yang sangat tebal mungkin karena?", "id": "Noise atau jitter." },
  { "en": "Osiloskop dengan voltmeter terintegrasi?", "id": "Beberapa model memilikinya (DVM)." },
  { "en": "Menyimpan data ke jaringan?", "id": "Melalui koneksi LAN." },
  { "en": "Perbedaan 'acquisition' dan 'waveform'?", "id": "Proses vs hasil." },
  { "en": "Kalibrasi sinyal kotak probe disebut?", "id": "Probe compensation." },
  { "en": "Pemicu berdasarkan waktu naik/turun?", "id": "Slew Rate Trigger." },
  { "en": "Tujuan 'slew rate trigger'?", "id": "Menangkap transisi yang terlalu lambat/cepat." },
  { "en": "Aplikasi osiloskop di bidang medis?", "id": "Melihat sinyal EKG, EEG." },
  { "en": "Melihat sinyal dari sensor getaran?", "id": "Ya, bisa dilakukan." },
  { "en": "Tampilan sumbu-Z pada osiloskop?", "id": "Mengontrol intensitas (pada model analog)." },
  { "en": "Mengukur tegangan AC dan DC bersamaan?", "id": "Gunakan kopling DC." },
  { "en": "Melihat riak pada tegangan DC?", "id": "Gunakan kopling AC." },
  { "en": "Tegangan 'offset' pada osiloskop?", "id": "Menambahkan/mengurangi level DC." },
  { "en": "Tujuan 'offset'?", "id": "Melihat sinyal AC di atas DC besar." },
  { "en": "Menampilkan hasil ukur di layar?", "id": "Measurement display." },
  { "en": "Apa itu 'record length'?", "id": "Jumlah titik sampel yang disimpan." },
  { "en": "Record length panjang berguna untuk?", "id": "Menangkap sinyal durasi panjang." },
  { "en": "Menampilkan gelombang dalam tiga dimensi?", "id": "Tidak bisa." },
  { "en": "Mengukur frekuensi sinyal AM?", "id": "Mengukur frekuensi sinyal pembawa." },
  { "en": "Mengukur frekuensi sinyal modulasi?", "id": "Bisa dengan FFT." },
  { "en": "Osiloskop genggam disebut juga?", "id": "Scopemeter." },
  { "en": "Keuntungan scopemeter?", "id": "Portabel dan terisolasi secara elektrik." },
  { "en": "Resolusi layar osiloskop?", "id": "Jumlah piksel pada layar." },
  { "en": "Pentingnya resolusi layar?", "id": "Untuk detail visual yang lebih baik." },
  { "en": "Fitur 'pass/fail' testing?", "id": "Sama seperti mask testing." },
  { "en": "Mode 'gated' pada FFT?", "id": "Menganalisis spektrum bagian tertentu." },
  { "en": "Pemicu pada event protokol serial?", "id": "Serial trigger." },
  { "en": "Contoh serial trigger?", "id": "Trigger pada 'start bit' UART." },
  { "en": "Menampilkan data sebagai diagram transisi?", "id": "State display (pada MSO)." },
  { "en": "Menghubungkan probe ke titik uji?", "id": "Probing." },
  { "en": "Titik uji pada PCB?", "id": "Test point." },
  { "en": "Akurasi penguatan DC?", "id": "DC gain accuracy." },
  { "en": "Fungsi 'fine' pada tombol?", "id": "Pengaturan presisi." },
  { "en": "Mengunci panel depan osiloskop?", "id": "Panel lock." },
  { "en": "Tujuan 'panel lock'?", "id": "Mencegah perubahan pengaturan tak sengaja." },
  { "en": "Osiloskop adalah alat fundamental untuk?", "id": "Insinyur dan teknisi elektronika." }



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
            }, 7500);
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
