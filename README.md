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


  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Jumlah Arus Masuk Sama Dengan Arus Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Dalam Loop Tertutup Sama Dengan Nol." },
  { "en": "Apa Itu Sumber Tegangan Ideal?", "id": "Sumber Tegangan (Resistansi Internal Nol)." },
  { "en": "Apa Itu Sumber Arus Ideal?", "id": "Sumber Arus (Resistansi Internal Tak Terhingga)." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Penyederhanaan Rangkaian (Sumber Tegangan Seri)." },
  { "en": "Apa Itu Teorema Norton?", "id": "Penyederhanaan Rangkaian (Sumber Arus Paralel)." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Analisis Rangkaian (Satu Sumber Aktif Dinyalakan)." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Penghambat Arus Listrik." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Penyimpan Energi Medan Magnet." },
  { "en": "Apa Itu Reaktansi (X)?", "id": "Hambatan Komponen Pasif Terhadap AC." },
  { "en": "Apa Itu Reaktansi Induktif (XL)?", "id": "Hambatan Yang Ditimbulkan Induktor (AC)." },
  { "en": "Apa Itu Reaktansi Kapasitif (XC)?", "id": "Hambatan Yang Ditimbulkan Kapasitor (AC)." },
  { "en": "Apa Itu Impedansi (Z)?", "id": "Hambatan Total Rangkaian AC (Kompleks)." },
  { "en": "Apa Itu Fasor (Phasor)?", "id": "Representasi Sinyal AC (Bilangan Kompleks)." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya Yang Dikonsumsi Beban Resistif (Watt)." },
  { "en": "Apa Itu Daya Reaktif (Q)?", "id": "Daya Yang Diserap Dikembalikan (VAR)." },
  { "en": "Apa Itu Daya Semu (S)?", "id": "Total Daya (Gabungan P Dan Q) (VA)." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata Dan Daya Semu (P/S)." },
  { "en": "Apa Itu Koreksi Faktor Daya?", "id": "Memperbaiki PF (Menambah Kapasitor Paralel)." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Penyearah Arus Listrik." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Aktif (Saklar Dan Penguat)." },
  { "en": "Apa Itu BJT (Bipolar Junction Transistor)?", "id": "Transistor (Kontrol Arus Basis)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Transistor (Kontrol Tegangan Gerbang)." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Untuk Fungsi Pewaktu)." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Komponen (Menstabilkan Tegangan Output)." },
  { "en": "Apa Itu Sensor?", "id": "Komponen Pengubah Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Komponen Pengubah Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital (Logika Dapat Diprogram)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Dasar Rangkaian Digital." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Digital (Penghitung Pulsa Clock)." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog." },
  { "en": "Apa Itu Sinyal Clock (Detak)?", "id": "Sinyal Denyut Sinkronisasi Digital." },
  { "en": "Apa Itu Kristal Osilator?", "id": "Komponen (Penghasil Sinyal Clock Akurat)." },
  { "en": "Contoh Bus Serial?", "id": "UART, I2C, SPI." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Cetak." },
  { "en": "Apa Itu Komponen SMD (Surface Mount)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Jalur PCB (Hijau)." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu File Gerber?", "id": "File Standar Manufaktur PCB." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur Listrik Dasar." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Sinyal." },
  { "en": "Apa Itu Solder Uap (Hot Air)?", "id": "Alat Solder SMD (Udara Panas)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Grounding (Pentanahan)?", "id": "Koneksi Ke Bumi (Proteksi Keamanan)." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik (Noise)." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Penghantar Listrik." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Penghambat Listrik." },
  { "en": "Apa Itu Semikonduktor?", "id": "Bahan (Silikon, Antara Konduktor Isolator)." },
  { "en": "Apa Itu Doping (Semikonduktor)?", "id": "Penambahan Ketidakmurnian (Tipe N/P)." },
  { "en": "Apa Itu Transduser?", "id": "Pengubah Satu Bentuk Energi Ke Lainnya." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Resistor (Menghubungkan Pin Ke VCC)." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Resistor (Menghubungkan Pin Ke GND)." },
  { "en": "Apa Itu Op-Amp Inverting?", "id": "Penguat (Output Membalik Fasa)." },
  { "en": "Apa Itu Op-Amp Non-Inverting?", "id": "Penguat (Output Fasa Sama)." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat (Gain = 1, Buffer Impedansi)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Op-Amp Menolak Sinyal Sama." },
  { "en": "Apa Itu Slew Rate (Op-Amp)?", "id": "Kecepatan Maksimal Perubahan Output." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Penguat (Linearitas Tinggi, Efisiensi Rendah)." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Resonansi RLC?", "id": "Kondisi (XL Sama Dengan XC)." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Meloloskan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Meloloskan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter (Hanya R, L, C)." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter (Menggunakan Op-Amp)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Jaringan RC)." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator (Frekuensi Sangat Stabil)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Pengunci Fasa Sinyal." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Rangkaian Pengubah AC Ke DC Efisien." },
  { "en": "Apa Itu Regulator Linear?", "id": "IC Penstabil Tegangan (Contoh: 78xx)." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu IC (Integrated Circuit) TTL?", "id": "Keluarga IC Digital (5V, Seri 74xx)." },
  { "en": "Apa Itu IC (Integrated Circuit) CMOS?", "id": "Keluarga IC Digital (Daya Rendah)." },
  { "en": "Apa Itu Flip-Flop D?", "id": "Memori (Penyimpan Data D Saat Clock)." },
  { "en": "Apa Itu Register Geser SIPO?", "id": "Input Serial, Output Paralel (74HC595)." },
  { "en": "Apa Itu Tampilan Seven Segment?", "id": "Penampil Angka (Tujuh LED)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair (Hemat Daya)." },
  { "en": "Apa Itu OLED (Organic LED)?", "id": "Tampilan (Piksel Memancarkan Cahaya Sendiri)." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Master)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Cepat, Empat Kabel)." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Di Dalam Chip." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital (Logika Dapat Diprogram Ulang)." },
  { "en": "Apa Itu Sensor PIR?", "id": "Sensor Gerak (Radiasi Panas Inframerah)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat (Strain Gauge)." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Per Langkah Presisi)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Motor DC." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator (Gerak Linear Elektromagnet)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Kumparan)." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya Yang Berubah Menjadi Kerja (Watt)." },
  { "en": "Apa Itu Daya Reaktif (Q)?", "id": "Daya Yang Dipertukarkan (Medan Magnet/Listrik)." },
  { "en": "Apa Itu Daya Semu (S)?", "id": "Daya Total Yang Ditarik Beban (VA)." },
  { "en": "Apa Itu Faktor Daya (PF)?", "id": "Rasio Daya Nyata Dan Daya Semu." },
  { "en": "Apa Itu Koreksi Faktor Daya?", "id": "Upaya Membuat Faktor Daya Mendekati Satu." },
  { "en": "Bagaimana Koreksi Faktor Daya Dilakukan?", "id": "Menambah Kapasitor Paralel Beban Induktif." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif Penghambat Arus." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif Penyimpan Muatan." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif Penyimpan Medan Magnet." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Aktif Penyearah Arus." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Aktif Saklar Dan Penguat." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Rangkaian Elektronik Terpadu Dalam Chip." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Sensor?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Pengubah Listrik Ke Gerakan/Aksi Fisik." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog Dengan Pulsa." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Pengukur Posisi Sudut." },
  { "en": "Apa Itu Kapasitor Elektrolit?", "id": "Kapasitor Polar (Nilai Kapasitansi Besar)." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Non-Polar (Nilai Kecil, Frekuensi Tinggi)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Penstabil Tegangan Output." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Vf Rendah (Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Arus Kontrol Kecil)." },
  { "en": "Apa Itu MOSFET Daya?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Mode Astable Dan Monostable)." },
  { "en": "Apa Itu Regulator Linear?", "id": "IC Penstabil Tegangan (Efisiensi Rendah)." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Beban)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Tampilan Seven Segment?", "id": "Penampil Angka (Tujuh LED Baris)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair (Hemat Daya)." },
  { "en": "Apa Itu OLED (Organic LED)?", "id": "Tampilan (Kontras Tinggi, Piksel Cahaya Sendiri)." },
  { "en": "Apa Itu Bus Paralel?", "id": "Jalur Data (Banyak Bit Sekaligus)." },
  { "en": "Apa Itu Bus Serial?", "id": "Jalur Data (Satu Bit Per Waktu)." },
  { "en": "Apa Itu Sinyal Clock (Detak)?", "id": "Sinyal Denyut (Sinkronisasi Rangkaian)." },
  { "en": "Apa Itu Kristal Osilator?", "id": "Komponen (Penghasil Clock Frekuensi Akurat)." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Digital (Penghitung Pulsa)." },
  { "en": "Apa Itu Register Geser?", "id": "Rangkaian Digital (Penggeser Data Bit)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Dasar Rangkaian Digital." },
  { "en": "Apa Itu Aljabar Boolean?", "id": "Matematika Logika Digital." },
  { "en": "Apa Itu Rangkaian Kombinasional?", "id": "Rangkaian (Output Tergantung Input Saat Ini)." },
  { "en": "Apa Itu Rangkaian Sekuensial?", "id": "Rangkaian (Output Tergantung Input Dan Memori)." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian (Mengukur Perubahan Resistansi Kecil)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Noise Yang Sama." },
  { "en": "Apa Itu Slew Rate (Op-Amp)?", "id": "Kecepatan Maksimal Perubahan Output." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Penguat (Linearitas Tinggi, Efisiensi Rendah)." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Kapasitor)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Pengunci Fasa Sinyal." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu (Nilai Berkelanjutan)." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Nilai 0 Dan 1)." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Demodulasi?", "id": "Memisahkan Informasi Dari Sinyal Pembawa." },
  { "en": "Apa Itu AM (Amplitude Modulation)?", "id": "Modulasi (Amplitudo Pembawa Berubah)." },
  { "en": "Apa Itu FM (Frequency Modulation)?", "id": "Modulasi (Frekuensi Pembawa Berubah)." },
  { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu Antena?", "id": "Pengubah Listrik Ke Gelombang Elektromagnetik." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Jalur PCB (Hijau)." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD (Pemanasan Oven)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat Pelindung ESD (Terhubung Ke Ground)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Bentuk Sinyal." },
  { "en": "Apa Itu Logger Data (Data Logger)?", "id": "Alat Perekam Data (Jangka Waktu Panjang)." },
  { "en": "Apa Itu Baterai Li-Ion?", "id": "Baterai Isi Ulang (Kepadatan Energi Tinggi)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu Pengisian Cepat (Fast Charging)?", "id": "Mengisi Baterai Dengan Arus Tinggi." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Trafo Step-Up?", "id": "Trafo (Menaikkan Tegangan AC)." },
  { "en": "Apa Itu Trafo Step-Down?", "id": "Trafo (Menurunkan Tegangan AC)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Komponen Saklar Daya (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Komponen Elektronika?", "id": "Bagian Dasar Pembentuk Rangkaian Listrik." },
  { "en": "Apa Itu Komponen Pasif?", "id": "Komponen (Tidak Memerlukan Sumber Daya Luar)." },
  { "en": "Contoh Komponen Pasif?", "id": "Resistor, Kapasitor, Induktor." },
  { "en": "Apa Itu Komponen Aktif?", "id": "Komponen (Memerlukan Sumber Daya Luar, Menguatkan)." },
  { "en": "Contoh Komponen Aktif?", "id": "Dioda, Transistor, IC." },
  { "en": "Apa Itu Resistansi?", "id": "Sifat Bahan (Menghambat Aliran Arus)." },
  { "en": "Apa Itu Konduktansi?", "id": "Sifat Bahan (Kemampuan Menghantarkan Arus)." },
  { "en": "Apa Satuan Konduktansi?", "id": "Siemens (S) Atau Mho (℧)." },
  { "en": "Apa Itu Dioda Silikon?", "id": "Dioda (Tegangan Jatuh Maju Sekitar 0.7V)." },
  { "en": "Apa Itu Dioda Germanium?", "id": "Dioda (Tegangan Jatuh Maju Sekitar 0.3V)." },
  { "en": "Apa Itu Transistor Bipolar (BJT)?", "id": "Transistor (Menggunakan Elektron Dan Hole)." },
  { "en": "Apa Itu Transistor Unipolar (FET)?", "id": "Transistor (Hanya Menggunakan Satu Muatan Saja)." },
  { "en": "Apa Itu Common Emitter (CE)?", "id": "Konfigurasi BJT (Penguatan Tegangan Tinggi)." },
  { "en": "Apa Itu Common Collector (CC)?", "id": "Konfigurasi BJT (Penguatan Arus, Voltage Follower)." },
  { "en": "Apa Itu Common Base (CB)?", "id": "Konfigurasi BJT (Penguatan Tegangan, Frekuensi Tinggi)." },
  { "en": "Apa Itu Mode Cut-Off (BJT)?", "id": "Transistor Mati (Saklar Terbuka)." },
  { "en": "Apa Itu Mode Saturasi (BJT)?", "id": "Transistor Menyala Penuh (Saklar Tertutup)." },
  { "en": "Apa Itu Mode Aktif (BJT)?", "id": "Transistor (Mode Penguat Linear)." },
  { "en": "Apa Itu Transkonduktansi (gm)?", "id": "Ukuran (Efisiensi Konversi Tegangan Input Ke Arus Output)." },
  { "en": "Apa Itu Transimpedansi?", "id": "Ukuran (Rasio Tegangan Output Terhadap Arus Input)." },
  { "en": "Apa Itu Op-Amp Inverting?", "id": "Penguat Op-Amp (Sinyal Output Berlawanan Fasa)." },
  { "en": "Apa Itu Op-Amp Non-Inverting?", "id": "Penguat Op-Amp (Sinyal Output Sefasa)." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Op-Amp (Membandingkan Dua Tegangan)." },
  { "en": "Apa Itu Multivibrator Astable?", "id": "Rangkaian (Menghasilkan Pulsa Kontinu)." },
  { "en": "Apa Itu Multivibrator Monostable?", "id": "Rangkaian (Menghasilkan Satu Pulsa (Trigger))." },
  { "en": "Apa Itu Multivibrator Bistable?", "id": "Rangkaian (Dua Keadaan Stabil, Flip-Flop)." },
  { "en": "Apa Itu Rangkaian RC?", "id": "Rangkaian (Resistor Dan Kapasitor)." },
  { "en": "Apa Itu Konstanta Waktu (τ) RC?", "id": "Waktu (Kapasitor Mengisi/Mengosongkan 63.2%)." },
  { "en": "Apa Itu Impedansi Kompleks?", "id": "Representasi Hambatan AC (Magnitudo Dan Fasa)." },
  { "en": "Apa Itu Diagram Bode?", "id": "Grafik (Respons Frekuensi Rangkaian (Gain Dan Fasa))." },
  { "en": "Apa Itu Frekuensi Cut-Off?", "id": "Frekuensi (Gain Turun 3 dB)." },
  { "en": "Apa Itu Filter Bandpass?", "id": "Filter (Meloloskan Pita Frekuensi Tertentu)." },
  { "en": "Apa Itu Filter Bandstop (Notch)?", "id": "Filter (Menghambat Pita Frekuensi Tertentu)." },
  { "en": "Apa Itu Q-Factor (Faktor Kualitas) Filter?", "id": "Ukuran (Selektivitas Filter)." },
  { "en": "Apa Itu Osilator Hartley?", "id": "Osilator (Jaringan Umpan Balik Induktor Tapped)." },
  { "en": "Apa Itu Osilator Colpitts?", "id": "Osilator (Jaringan Umpan Balik Kapasitor Tapped)." },
  { "en": "Apa Itu Gerbang Logika AND?", "id": "Output '1' Jika Semua Input '1'." },
  { "en": "Apa Itu Gerbang Logika OR?", "id": "Output '1' Jika Salah Satu Input '1'." },
  { "en": "Apa Itu Gerbang Logika NOT?", "id": "Output (Kebalikan Input)." },
  { "en": "Apa Itu Decoder?", "id": "Rangkaian Digital (Mengubah Kode Ke Output Unik)." },
  { "en": "Apa Itu Encoder?", "id": "Rangkaian Digital (Mengubah Input Unik Ke Kode)." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Rangkaian Digital (Pemilih Input Data)." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Rangkaian Digital (Pengarah Output Data)." },
  { "en": "Apa Itu Half Adder?", "id": "Penjumlah Digital (Dua Input, Sum Carry)." },
  { "en": "Apa Itu Full Adder?", "id": "Penjumlah Digital (Tiga Input (Termasuk Carry In))." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Bagian CPU (Melakukan Operasi Aritmatika Logika)." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Baca Tulis Volatil (Sementara)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Baca Saja Non-Volatil (Permanen)." },
  { "en": "Apa Itu Flash Memory?", "id": "Memori Non-Volatil (Bisa Dihapus Diprogram Ulang)." },
  { "en": "Apa Itu EEPROM?", "id": "Memori (Dapat Diprogram Dihapus Secara Elektrik)." },
  { "en": "Apa Itu IC (Integrated Circuit) TTL?", "id": "Keluarga Digital (Transistor-Transistor Logic, Cepat)." },
  { "en": "Apa Itu IC (Integrated Circuit) CMOS?", "id": "Keluarga Digital (Daya Rendah, Rentan ESD)." },
  { "en": "Apa Itu IC (Integrated Circuit) Linear?", "id": "IC Pengolah Sinyal Analog (Op-Amp, Regulator)." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal (Nilai Berkelanjutan Sepanjang Waktu)." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal (Nilai Diskrit (0 Atau 1))." },
  { "en": "Apa Itu Sampling (Pencuplikan)?", "id": "Proses Pengambilan Nilai Sinyal Analog (ADC)." },
  { "en": "Apa Itu Quantization (Kuantisasi)?", "id": "Proses Pembulatan Nilai Sample Ke Tingkat Diskrit." },
  { "en": "Apa Itu LSB (Least Significant Bit)?", "id": "Bit (Memiliki Bobot Nilai Paling Kecil)." },
  { "en": "Apa Itu MSB (Most Significant Bit)?", "id": "Bit (Memiliki Bobot Nilai Paling Besar)." },
  { "en": "Apa Itu Resolusi ADC/DAC?", "id": "Jumlah Bit (Menentukan Ketelitian)." },
  { "en": "Apa Itu Kecepatan Bit (Bit Rate)?", "id": "Jumlah Bit Data (Ditransmisikan Per Detik)." },
  { "en": "Apa Itu Baud Rate?", "id": "Jumlah Simbol (Ditransmisikan Per Detik)." },
  { "en": "Apa Itu Protokol Komunikasi?", "id": "Aturan Standar Pertukaran Data." },
  { "en": "Apa Itu USB (Universal Serial Bus)?", "id": "Standar Koneksi Serial (Data Daya)." },
  { "en": "Apa Itu Ethernet?", "id": "Standar Jaringan Komputer (Kabel Twisted Pair)." },
  { "en": "Apa Itu Bluetooth?", "id": "Standar Komunikasi Nirkabel Jarak Dekat." },
  { "en": "Apa Itu Zigbee?", "id": "Standar Nirkabel (Daya Rendah, Jaringan Mesh)." },
  { "en": "Apa Itu Trafo (Transformer)?", "id": "Komponen (Mengubah Tegangan AC (Induksi))." },
  { "en": "Apa Itu Lilitan Primer?", "id": "Lilitan Trafo (Terhubung Ke Sumber Input)." },
  { "en": "Apa Itu Lilitan Sekunder?", "id": "Lilitan Trafo (Terhubung Ke Beban Output)." },
  { "en": "Apa Itu Inti Ferit?", "id": "Inti Magnetik (Digunakan Di Trafo Frekuensi Tinggi)." },
  { "en": "Apa Itu Inti Besi Laminasi?", "id": "Inti Magnetik (Digunakan Di Trafo Frekuensi Rendah)." },
  { "en": "Apa Itu Rugi-Rugi Tembaga (Copper Loss)?", "id": "Kerugian Daya (Akibat Resistansi Lilitan)." },
  { "en": "Apa Itu Rugi-Rugi Inti (Core Loss)?", "id": "Kerugian Daya (Histeresis Dan Arus Eddy)." },
  { "en": "Apa Itu Arus Eddy (Arus Pusar)?", "id": "Arus Induksi (Di Dalam Inti, Menghasilkan Panas)." },
  { "en": "Apa Itu Penyearah Setengah Gelombang?", "id": "Rangkaian (Satu Dioda, Menghilangkan Setengah Siklus)." },
  { "en": "Apa Itu Penyearah Jembatan?", "id": "Rangkaian (Empat Dioda, Memanfaatkan Kedua Siklus)." },
  { "en": "Apa Itu Filter Kapasitor (Filter C)?", "id": "Kapasitor (Mengurangi Ripple Tegangan DC)." },
  { "en": "Apa Itu Faktor Ripple?", "id": "Ukuran (Fluktuasi Sinyal AC Tersisa Pada DC Output)." },
  { "en": "Apa Itu Regulator Tegangan Linear?", "id": "IC (Menurunkan Tegangan (Disipasi Panas))." },
  { "en": "Apa Itu Regulator Tegangan Switching?", "id": "IC (Menggunakan Saklar Dan Induktor (Efisiensi Tinggi))." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Menurunkan Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Menaikkan Tegangan DC)." },
  { "en": "Apa Itu Sensor Suhu Termokopel?", "id": "Sensor (Mengukur Suhu (Prinsip Seebeck))." },
  { "en": "Apa Itu Sensor Suhu RTD?", "id": "Sensor (Mengukur Suhu (Resistansi Kawat))." },
  { "en": "Apa Itu Sensor Suhu Termistor?", "id": "Sensor (Mengukur Suhu (Resistansi Semikonduktor))." },
  { "en": "Apa Itu Sensor Jarak Ultrasonik?", "id": "Sensor (Mengukur Jarak (Gelombang Suara))." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor (Mengukur Berat/Gaya)." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor (Mengukur Regangan Bahan)." },
  { "en": "Apa Itu Aktuator Solenoid?", "id": "Aktuator (Gerak Linear (Elektromagnet))." },
  { "en": "Apa Itu Motor DC Brushless (Tanpa Sikat)?", "id": "Motor (Menggunakan Komutasi Elektronik)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Putar Presisi Per Langkah)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian (Mengontrol Arah Putar Motor DC)." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog (Mengubah Duty Cycle)." },
  { "en": "Apa Itu Duty Cycle (Siklus Kerja)?", "id": "Rasio (Waktu ON Terhadap Total Periode Pulsa)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Memisahkan Rangkaian Bertegangan Beda)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay Elektronik (Menggunakan Optocoupler)." },
  { "en": "Apa Itu Komponen Elektronika?", "id": "Bagian Dasar Pembentuk Rangkaian Listrik." },
  { "en": "Apa Itu Komponen Pasif?", "id": "Komponen (Tidak Memerlukan Daya Untuk Beroperasi)." },
  { "en": "Apa Itu Komponen Aktif?", "id": "Komponen (Memerlukan Daya, Bisa Menguatkan Sinyal)." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif (Menghambat Arus)." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif (Menyimpan Muatan)." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif (Menyimpan Medan Magnet)." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Aktif (Penyearah Arus)." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Aktif (Saklar Dan Penguat)." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Rangkaian Elektronik Terpadu Dalam Chip." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Sensor?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Pengubah Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog Dengan Pulsa." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Tiga Terminal)." },
  { "en": "Apa Itu Kapasitor Elektrolit?", "id": "Kapasitor Polar (Nilai Kapasitansi Besar)." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Non-Polar (Frekuensi Tinggi)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda (Vf Rendah, Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Kontrol Arus)." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Mode Astable Dan Monostable)." },
  { "en": "Apa Itu Regulator Linear?", "id": "IC Penstabil Tegangan (Efisiensi Rendah)." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Beban)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Tampilan Seven Segment?", "id": "Penampil Angka (Tujuh LED Baris)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair (Hemat Daya)." },
  { "en": "Apa Itu OLED (Organic LED)?", "id": "Tampilan (Piksel Cahaya Sendiri, Kontras)." },
  { "en": "Apa Itu Bus Paralel?", "id": "Jalur Data (Banyak Bit Sekaligus)." },
  { "en": "Apa Itu Bus Serial?", "id": "Jalur Data (Satu Bit Per Waktu)." },
  { "en": "Apa Itu Sinyal Clock (Detak)?", "id": "Sinyal Denyut (Sinkronisasi Rangkaian)." },
  { "en": "Apa Itu Kristal Osilator?", "id": "Komponen (Penghasil Clock Frekuensi Akurat)." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Digital (Penghitung Pulsa)." },
  { "en": "Apa Itu Register Geser?", "id": "Rangkaian Digital (Penggeser Data Bit)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Dasar Rangkaian Digital." },
  { "en": "Apa Itu Aljabar Boolean?", "id": "Matematika Logika Digital." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian (Mengukur Perubahan Resistansi Kecil)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Noise Yang Sama." },
  { "en": "Apa Itu Slew Rate (Op-Amp)?", "id": "Kecepatan Maksimal Perubahan Output." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Penguat (Linearitas Tinggi, Efisiensi Rendah)." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Kapasitor)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Pengunci Fasa Sinyal." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu (Nilai Berkelanjutan)." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Nilai 0 Dan 1)." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Demodulasi?", "id": "Memisahkan Informasi Dari Sinyal Pembawa." },
  { "en": "Apa Itu AM (Amplitude Modulation)?", "id": "Modulasi (Amplitudo Pembawa Berubah)." },
  { "en": "Apa Itu FM (Frequency Modulation)?", "id": "Modulasi (Frekuensi Pembawa Berubah)." },
  { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu Antena?", "id": "Pengubah Listrik Ke Gelombang Elektromagnetik." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Jalur PCB (Hijau)." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD (Pemanasan Oven)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat (Menghubungkan Tubuh Ke Ground)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Bentuk Sinyal." },
  { "en": "Apa Itu Logger Data (Data Logger)?", "id": "Alat Perekam Data (Jangka Waktu Panjang)." },
  { "en": "Apa Itu Baterai Li-Ion?", "id": "Baterai Isi Ulang (Kepadatan Energi Tinggi)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu Pengisian Cepat (Fast Charging)?", "id": "Mengisi Baterai Dengan Arus Tinggi." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Trafo Step-Up?", "id": "Trafo (Menaikkan Tegangan AC)." },
  { "en": "Apa Itu Trafo Step-Down?", "id": "Trafo (Menurunkan Tegangan AC)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Komponen Saklar Daya (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Photodiode (Dioda Foto)?", "id": "Dioda (Sensor Cahaya)." },
  { "en": "Apa Itu Phototransistor?", "id": "Transistor (Peka Cahaya, Lebih Sensitif)." },
  { "en": "Apa Itu Sensor PIR?", "id": "Sensor Gerak (Radiasi Panas Inframerah)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Per Langkah Presisi)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Motor DC." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator (Gerak Linear Elektromagnet)." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Mikroprosesor (MPU)?", "id": "CPU Dalam Satu Chip IC." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Otak Pemroses Instruksi Utama." },
  { "en": "Apa Itu Memori (Penyimpanan)?", "id": "Perangkat Penyimpan Data Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Baca Tulis (Volatile)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca (Non-Volatile)." },
  { "en": "Apa Itu Flash Memory?", "id": "Memori Non-Volatile (EEPROM, Cepat)." },
  { "en": "Apa Itu Port I/O (Input/Output)?", "id": "Pin Komunikasi MCU Dengan Dunia Luar." },
  { "en": "Apa Itu GPIO (General Purpose I/O)?", "id": "Pin I/O (Fungsi Fleksibel)." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Timer (Pewaktu) MCU?", "id": "Peripheral Penghitung Waktu Internal." },
  { "en": "Apa Itu Interupsi (Interrupt)?", "id": "Sinyal (Meminta Perhatian CPU Segera)." },
  { "en": "Apa Itu ISR (Interrupt Service Routine)?", "id": "Fungsi Khusus Penangan Interupsi." },
  { "en": "Apa Itu Bootloader?", "id": "Program Kecil Pemuat Kode Utama." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tertanam Di Hardware." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "OS (Respon Tugas Terjamin Batas Waktu)." },
  { "en": "Apa Itu Papan Pengembangan (Dev Board)?", "id": "Papan Siap Pakai (Arduino, ESP32)." },
  { "en": "Apa Itu Arduino?", "id": "Platform MCU (Open Source, ATmega)." },
  { "en": "Apa Itu Raspberry Pi Pico?", "id": "MCU (Mikrokontroler) Berbasis RP2040." },
  { "en": "Apa Itu ESP32?", "id": "MCU (Wi-Fi Dan Bluetooth)." },
  { "en": "Apa Itu Komponen Pasif?", "id": "Komponen (Tidak Memerlukan Daya)." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif (Menghambat Arus)." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif (Menyimpan Muatan)." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif (Menyimpan Medan Magnet)." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Aktif (Penyearah Arus)." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Aktif (Saklar Dan Penguat)." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Komponen (Menstabilkan Tegangan Output)." },
  { "en": "Apa Itu Trafo (Transformer)?", "id": "Komponen (Mengubah Tegangan AC)." },
  { "en": "Apa Itu Sensor?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Pengubah Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Tiga Terminal)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Vf Rendah (Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Kontrol Arus)." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Beban)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Tampilan Seven Segment?", "id": "Penampil Angka (Tujuh LED Baris)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair (Hemat Daya)." },
  { "en": "Apa Itu OLED (Organic LED)?", "id": "Tampilan (Piksel Cahaya Sendiri, Kontras)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Dasar Rangkaian Digital." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Digital (Penghitung Pulsa)." },
  { "en": "Apa Itu Register Geser?", "id": "Rangkaian Digital (Penggeser Data Bit)." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian (Mengukur Perubahan Resistansi Kecil)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Noise Yang Sama." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Komponen (Penghasil Clock Frekuensi Akurat)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Pengunci Fasa Sinyal." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD (Pemanasan Oven)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Bentuk Sinyal." },
  { "en": "Apa Itu Baterai Li-Ion?", "id": "Baterai Isi Ulang (Kepadatan Energi Tinggi)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Komponen Saklar Daya (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Photodiode (Dioda Foto)?", "id": "Dioda (Sensor Cahaya)." },
  { "en": "Apa Itu Phototransistor?", "id": "Transistor (Peka Cahaya, Lebih Sensitif)." },
  { "en": "Apa Itu Sensor PIR?", "id": "Sensor Gerak (Radiasi Panas Inframerah)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Logika TTL (Transistor-Transistor Logic)?", "id": "Keluarga IC Digital (5V, Cepat)." },
  { "en": "Apa Itu Logika CMOS (Complementary MOS)?", "id": "Keluarga IC Digital (Daya Rendah)." },
  { "en": "Apa Itu Op-Amp Inverting?", "id": "Penguat (Output Membalik Fasa)." },
  { "en": "Apa Itu Op-Amp Non-Inverting?", "id": "Penguat (Output Fasa Sama)." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat (Gain = 1, Buffer Impedansi)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Kapasitor)." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Rangkaian Pengubah AC Ke DC Efisien." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Meloloskan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Meloloskan Frekuensi Tinggi." },
  { "en": "Apa Itu Modulasi AM?", "id": "Modulasi (Amplitudo Pembawa Berubah)." },
  { "en": "Apa Itu Modulasi FM?", "id": "Modulasi (Frekuensi Pembawa Berubah)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu Impedansi?", "id": "Hambatan Total Rangkaian AC (Termasuk Resistansi Dan Reaktansi)." },
  { "en": "Apa Itu Reaktansi?", "id": "Hambatan Yang Ditimbulkan Kapasitor Atau Induktor (AC)." },
  { "en": "Apa Itu Kapasitansi?", "id": "Kemampuan Komponen Menyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktansi?", "id": "Kemampuan Komponen Menghasilkan Medan Magnet." },
  { "en": "Apa Itu Dioda Varaktor?", "id": "Dioda (Kapasitansi Berubah Tergantung Tegangan Balik)." },
  { "en": "Apa Itu Dioda Tunnel (Esaki)?", "id": "Dioda (Resistansi Negatif, Switching Sangat Cepat)." },
  { "en": "Apa Itu TRIAC?", "id": "Komponen Saklar Daya (Dua Arah AC)." },
  { "en": "Apa Itu DIAC?", "id": "Komponen Pemicu TRIAC (Dua Arah AC)." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Transistor Daya (Gabungan MOSFET Dan BJT)." },
  { "en": "Apa Itu Thyristor?", "id": "Keluarga Semikonduktor Daya (SCR, TRIAC)." },
  { "en": "Apa Itu LDMOS (Laterally Diffused MOSFET)?", "id": "MOSFET (Untuk Aplikasi Daya Frekuensi Radio)." },
  { "en": "Apa Itu Komponen Piezoelektrik?", "id": "Komponen (Mengubah Tekanan Mekanik Ke Listrik)." },
  { "en": "Apa Itu Resonator Keramik?", "id": "Komponen (Pengganti Kristal, Kurang Akurat)." },
  { "en": "Apa Itu Dekoder (Decoder)?", "id": "Rangkaian Digital (Mengubah Kode Biner Ke Output Unik)." },
  { "en": "Apa Itu Encoder (Penyandi)?", "id": "Rangkaian Digital (Mengubah Input Unik Ke Kode Biner)." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Rangkaian Digital (Memilih Satu Dari Banyak Input)." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Rangkaian Digital (Mengirim Satu Input Ke Banyak Output)." },
  { "en": "Apa Itu Komparator Digital?", "id": "Rangkaian Digital (Membandingkan Dua Bilangan Biner)." },
  { "en": "Apa Itu Half Adder?", "id": "Penjumlah Digital (Dua Input, Sum Dan Carry)." },
  { "en": "Apa Itu Full Adder?", "id": "Penjumlah Digital (Tiga Input (Termasuk Carry In))." },
  { "en": "Apa Itu Rangkaian Sekuensial?", "id": "Rangkaian Digital (Output Tergantung Input Dan Memori)." },
  { "en": "Apa Itu Rangkaian Kombinasional?", "id": "Rangkaian Digital (Output Hanya Tergantung Input Saat Ini)." },
  { "en": "Apa Itu Modulus Counter?", "id": "Jumlah Keadaan Unik (Dihitung Oleh Pencacah)." },
  { "en": "Apa Itu Latch (Sel Penahan)?", "id": "Elemen Memori Dasar (Tidak Sinkron Clock)." },
  { "en": "Apa Itu Flip-Flop D?", "id": "Elemen Memori (Menyimpan Data D Pada Tepi Clock)." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Rangkaian (Membersihkan Sinyal Noise Menjadi Pulsa Bersih)." },
  { "en": "Apa Itu Noise Margin?", "id": "Toleransi Sinyal Digital (Terhadap Noise Tegangan)." },
  { "en": "Apa Itu Fan-Out (Gerbang Logika)?", "id": "Jumlah Maksimum Beban (Yang Dapat Digerakkan Gerbang)." },
  { "en": "Apa Itu Propagation Delay?", "id": "Waktu (Perubahan Input Sampai Output Stabil)." },
  { "en": "Apa Itu Sinyal Sinkron?", "id": "Sinyal (Semua Perubahan Diatur Oleh Clock Sama)." },
  { "en": "Apa Itu Sinyal Asinkron?", "id": "Sinyal (Perubahan Tidak Tergantung Clock Sama)." },
  { "en": "Apa Itu Bus Serial?", "id": "Jalur Data (Satu Bit Per Waktu, I2C, SPI)." },
  { "en": "Apa Itu Bus Paralel?", "id": "Jalur Data (Banyak Bit Sekaligus, Cepat)." },
  { "en": "Apa Itu Protokol RS-232?", "id": "Standar Komunikasi Serial (Tegangan Tinggi, Jarak Jauh)." },
  { "en": "Apa Itu Protokol CAN (Controller Area Network)?", "id": "Bus Komunikasi (Di Kendaraan Mobil, Industri)." },
  { "en": "Apa Itu Protokol Ethernet?", "id": "Standar Jaringan Data (Kabel Twisted Pair/Fiber Optic)." },
  { "en": "Apa Itu Konverter Buck-Boost?", "id": "Regulator Switching (Bisa Menaikkan Atau Menurunkan Tegangan)." },
  { "en": "Apa Itu Konverter Flyback?", "id": "Regulator Switching Terisolasi (Trafo Khusus)." },
  { "en": "Apa Itu Konverter Sepic?", "id": "Regulator Switching (Output Selalu Positif, Naik/Turun)." },
  { "en": "Apa Itu LDO (Low Dropout Regulator)?", "id": "Regulator Linear (Tegangan Input Sangat Dekat Output)." },
  { "en": "Apa Itu Regulator Linear?", "id": "Regulator (Menggunakan Transistor Mode Aktif (Panas))." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Menggunakan Saklar (Efisiensi Tinggi))." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pengaman (Komponen Melebur Saat Arus Berlebih)." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Pengaman (Saklar Otomatis, Bisa Di-reset)." },
  { "en": "Apa Itu Varistor (MOV)?", "id": "Pelindung (Menyerap Lonjakan Tegangan Tinggi)." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppression)?", "id": "Pelindung (Menyerap Lonjakan Tegangan Cepat)." },
  { "en": "Apa Itu Grounding (Pentanahan)?", "id": "Koneksi Ke Bumi (Jalur Pelepasan Arus Aman)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat Pelindung ESD (Menghubungkan Tubuh Ke Ground)." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik (Noise)." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kemampuan Rangkaian (Berfungsi Baik Di Lingkungan EMI)." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Filter (Melewatkan Frekuensi Di Bawah Cut-Off)." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Filter (Melewatkan Frekuensi Di Atas Cut-Off)." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter (Hanya Menggunakan R, L, C)." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter (Menggunakan Komponen Aktif, Op-Amp)." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Penguat (Kombinasi Kelas A Dan Kelas B)." },
  { "en": "Apa Itu Crossover Distortion?", "id": "Distorsi (Terjadi Saat Transistor Kelas B Berpindah Fasa)." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya (Diubah Menjadi Kerja Nyata, Watt)." },
  { "en": "Apa Itu Daya Reaktif (Q)?", "id": "Daya (Dipertukarkan Medan Magnet/Listrik, VAR)." },
  { "en": "Apa Itu Daya Semu (S)?", "id": "Daya Total (Vrms x Arms, VA)." },
  { "en": "Apa Itu Faktor Daya (PF)?", "id": "Rasio Daya Nyata Dan Daya Semu (P/S)." },
  { "en": "Apa Itu Koreksi Faktor Daya?", "id": "Upaya Membuat PF Mendekati Satu (Meminimalkan Q)." },
  { "en": "Apa Itu Harmonik (Harmonic)?", "id": "Frekuensi (Kelipatan Bilangan Bulat Dari Frekuensi Dasar)." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran (Distorsi Harmonik Total Sinyal)." },
  { "en": "Apa Itu Resonansi Seri?", "id": "Kondisi (Impedansi Minimum, XL = XC)." },
  { "en": "Apa Itu Resonansi Paralel?", "id": "Kondisi (Impedansi Maksimum, XL = XC)." },
  { "en": "Apa Itu Dioda LED?", "id": "Dioda (Memancarkan Cahaya Saat Bias Maju)." },
  { "en": "Apa Itu Dioda Laser?", "id": "Dioda (Memancarkan Cahaya Koheren (Satu Fasa))." },
  { "en": "Apa Itu Photodiode?", "id": "Dioda (Menghasilkan Arus Saat Terkena Cahaya)." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor (Mengukur Medan Magnet)." },
  { "en": "Apa Itu Sensor LVDT (Linear Variable Differential Transformer)?", "id": "Sensor (Mengukur Perpindahan Linear)." },
  { "en": "Apa Itu Sensor Kapasitif?", "id": "Sensor (Mengukur Jarak/Kehadiran (Perubahan Kapasitansi))." },
  { "en": "Apa Itu Motor Sinkron?", "id": "Motor AC (Kecepatan Sama Dengan Frekuensi Sumber)." },
  { "en": "Apa Itu Motor Asinkron (Induksi)?", "id": "Motor AC (Kecepatan Kurang Dari Frekuensi Sumber)." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Pengontrol Kecepatan Motor AC (Mengubah Frekuensi)." },
  { "en": "Apa Itu Inverter?", "id": "Rangkaian (Mengubah DC Menjadi AC)." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian (Mengubah AC Menjadi DC)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Sistem Catu Daya (Menyediakan Daya Darurat Saat Listrik Padam)." },
  { "en": "Apa Itu Baterai SLA (Sealed Lead Acid)?", "id": "Baterai (Biasa Digunakan Untuk UPS)." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Cetak (Jalur Tembaga)." },
  { "en": "Apa Itu V-Cut (PCB)?", "id": "Garis Potongan V (Mempermudah Pemisahan PCB)." },
  { "en": "Apa Itu Stencil Solder Paste?", "id": "Template (Untuk Mencetak Pasta Solder Di PCB SMD)." },
  { "en": "Apa Itu Komponen THT (Through-Hole Technology)?", "id": "Komponen (Kaki Menembus Lubang PCB)." },
  { "en": "Apa Itu Komponen SMT (Surface Mount Technology)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Reflow Oven?", "id": "Oven (Digunakan Untuk Menyolder Komponen SMT)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat Pelindung ESD (Menghubungkan Tubuh Ke Ground)." },
  { "en": "Apa Itu Probe Diferensial (Osiloskop)?", "id": "Probe (Mengukur Perbedaan Dua Titik (Tanpa Referensi Ground))." },
  { "en": "Apa Itu Frekuensi Bandwidth (Osiloskop)?", "id": "Rentang Frekuensi (Yang Dapat Diukur Akurat)." },
  { "en": "Apa Itu Probe Logika?", "id": "Alat Uji (Mendeteksi Keadaan Logika (HIGH/LOW))." },
  { "en": "Apa Itu Logger Data (Data Logger)?", "id": "Alat (Perekam Data Dalam Jangka Waktu Panjang)." },
  { "en": "Apa Itu Kalibrasi (Kalibrasi)?", "id": "Proses (Memastikan Akurasi Alat Ukur)." },
  { "en": "Apa Itu Traceability (Ketertelusuran)?", "id": "Kemampuan (Melacak Standar Kalibrasi Ke Standar Nasional)." },
  { "en": "Apa Itu JFET (Junction Field-Effect Transistor)?", "id": "FET (Gerbang Tipe PN Junction)." },
  { "en": "Apa Itu Dual Gate MOSFET?", "id": "MOSFET (Dua Gerbang, Aplikasi Mixer RF)." },
  { "en": "Apa Itu Current Mirror?", "id": "Rangkaian (Menyalin Arus Secara Akurat)." },
  { "en": "Apa Itu Buffer (Penyangga)?", "id": "Rangkaian (Memisahkan Rangkaian (Mencegah Loading))." },
  { "en": "Apa Itu Gain Loop Terbuka (Open Loop Gain)?", "id": "Penguatan Maksimal (Tanpa Umpan Balik)." },
  { "en": "Apa Itu Gain Loop Tertutup (Closed Loop Gain)?", "id": "Penguatan (Dengan Umpan Balik)." },
  { "en": "Apa Itu Penguatan Loop Terbuka (Open Loop Gain)?", "id": "Penguatan Maksimal Op-Amp (Tanpa Umpan Balik)." },
  { "en": "Apa Itu Penguatan Loop Tertutup (Closed Loop Gain)?", "id": "Penguatan Op-Amp (Dengan Umpan Balik)." },
  { "en": "Apa Itu Biasing (Bias) Op-Amp?", "id": "Pemberian Tegangan DC (Agar Op-Amp Berfungsi)." },
  { "en": "Apa Itu Tegangan Offset (Offset Voltage)?", "id": "Perbedaan Tegangan Input (Agar Output Nol)." },
  { "en": "Apa Itu Arus Bias Input (Input Bias Current)?", "id": "Arus DC Kecil (Mengalir Ke Pin Input)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Op-Amp (Menolak Sinyal Sama)." },
  { "en": "Apa Itu Slew Rate?", "id": "Kecepatan Maksimal Perubahan Tegangan Output." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Op-Amp (Pembanding Dua Tegangan)." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator (Dengan Histeresis (Anti Noise))." },
  { "en": "Apa Itu Histeresis (Hysteresis)?", "id": "Perbedaan Level Pemicu (Naik Dan Turun)." },
  { "en": "Apa Itu Generator Gelombang Segitiga?", "id": "Rangkaian (Menghasilkan Pulsa Segitiga)." },
  { "en": "Apa Itu Generator Gelombang Sinus?", "id": "Rangkaian (Menghasilkan Pulsa Sinus)." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator (Op-Amp, Menghasilkan Sinus)." },
  { "en": "Apa Itu Osilator Kuadratur?", "id": "Osilator (Menghasilkan Sinus Dan Kosinus)." },
  { "en": "Apa Itu Multi-Sim (Software)?", "id": "Perangkat Lunak (Simulasi Rangkaian Elektronika)." },
  { "en": "Apa Itu LTSpice (Software)?", "id": "Perangkat Lunak (Simulasi Rangkaian Analog Gratis)." },
  { "en": "Apa Itu PSpice (Software)?", "id": "Perangkat Lunak (Simulasi Rangkaian Spice)." },
  { "en": "Apa Itu Spice (Simulation Program with IC Emphasis)?", "id": "Bahasa (Deskripsi Sirkuit Untuk Simulasi)." },
  { "en": "Apa Itu Spectre (Software)?", "id": "Simulator (Analisis Rangkaian IC Skala Besar)." },
  { "en": "Apa Itu Verilog-A?", "id": "Bahasa (Pemodelan Rangkaian Sinyal Campuran)." },
  { "en": "Apa Itu Verilog-AMS?", "id": "Bahasa (Gabungan Verilog-A Dan Verilog Digital)." },
  { "en": "Apa Itu MATLAB (Matrix Laboratory)?", "id": "Perangkat Lunak (Komputasi Numerik, Simulasi)." },
  { "en": "Apa Itu Simulink?", "id": "Lingkungan (Simulasi Grafis Dinamis (MATLAB))." },
  { "en": "Apa Itu LabVIEW?", "id": "Perangkat Lunak (Pemrograman Grafis Instrumentasi)." },
  { "en": "Apa Itu Firmware Development?", "id": "Proses (Penulisan Kode Untuk Sistem Embedded)." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Perangkat Lunak (Editor, Compiler, Debugger)." },
  { "en": "Apa Itu Compiler (Kompilator)?", "id": "Penerjemah (Kode Sumber Ke Kode Mesin)." },
  { "en": "Apa Itu Linker?", "id": "Penghubung (Kode Objek Ke File Eksekusi)." },
  { "en": "Apa Itu Loader?", "id": "Pemuat (Program Dari Disk Ke Memori Utama)." },
  { "en": "Apa Itu Debugger?", "id": "Alat (Pencari Dan Pemecah Masalah Kode)." },
  { "en": "Apa Itu Breakpoint?", "id": "Titik (Menghentikan Program Untuk Debugging)." },
  { "en": "Apa Itu Watch Window?", "id": "Jendela (Memantau Nilai Variabel Saat Debug)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka (Debugging Dan Pemrograman IC)." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka (Debug Dengan Dua Pin Data Clock)." },
  { "en": "Apa Itu Bootloader?", "id": "Program Kecil (Memuat Program Utama Saat Power ON)." },
  { "en": "Apa Itu Flash Programming?", "id": "Proses (Menulis Kode Ke Memori Flash MCU)." },
  { "en": "Apa Itu Over-The-Air (OTA) Update?", "id": "Pembaruan (Firmware Nirkabel (Wi-Fi/Bluetooth))." },
  { "en": "Apa Itu Sistem Operasi Real-Time (RTOS)?", "id": "OS (Menjamin Respon Dalam Batas Waktu Tertentu)." },
  { "en": "Apa Itu Kernel (OS)?", "id": "Inti (Pengelola Sumber Daya Hardware)." },
  { "en": "Apa Itu Task (Tugas) (RTOS)?", "id": "Unit Program (Independen Yang Dijadwalkan)." },
  { "en": "Apa Itu Scheduler (RTOS)?", "id": "Pengatur (Giliran Eksekusi Task)." },
  { "en": "Apa Itu Preemption?", "id": "Kondisi (Task Prioritas Tinggi Mengambil Alih CPU)." },
  { "en": "Apa Itu Priority Inversion?", "id": "Masalah (Task Rendah Blokir Task Tinggi)." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Pengunci (Mencegah Akses Bersamaan Ke Sumber Daya)." },
  { "en": "Apa Itu Semaphore?", "id": "Mekanisme Sinkronisasi (Mengontrol Akses)." },
  { "en": "Apa Itu Message Queue (Antrian Pesan)?", "id": "Saluran (Komunikasi Data Antar Task)." },
  { "en": "Apa Itu Circular Buffer (Buffer Melingkar)?", "id": "Buffer (Data Ditulis Timpa Setelah Penuh)." },
  { "en": "Apa Itu Sensor Piezoresistif?", "id": "Sensor (Hambatan Berubah Akibat Tekanan/Regangan)." },
  { "en": "Apa Itu Sensor Ultrasonik (Jarak)?", "id": "Sensor (Mengukur Waktu Tempuh Pulsa Suara)." },
  { "en": "Apa Itu Sensor Proximity Induktif?", "id": "Sensor (Mendeteksi Logam (Medan Magnet))." },
  { "en": "Apa Itu Sensor Proximity Kapasitif?", "id": "Sensor (Mendeteksi Logam Atau Non-Logam)." },
  { "en": "Apa Itu Sensor Fotoelektrik?", "id": "Sensor (Mendeteksi Cahaya (Through-Beam, Retro-Reflective))." },
  { "en": "Apa Itu Sensor Suhu Inframerah?", "id": "Sensor (Mengukur Radiasi Panas (Tanpa Kontak))." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor)?", "id": "Sensor (Mengukur Kecepatan Atau Volume Fluida)." },
  { "en": "Apa Itu Sensor Level (Level Sensor)?", "id": "Sensor (Mengukur Ketinggian Cairan/Padatan)." },
  { "en": "Apa Itu Sensor pH?", "id": "Sensor (Mengukur Keasaman/Kebasaan Cairan)." },
  { "en": "Apa Itu Sensor Konduktivitas?", "id": "Sensor (Mengukur Daya Hantar Listrik Cairan)." },
  { "en": "Apa Itu Sensor Warna?", "id": "Sensor (Menganalisis Komponen Cahaya (RGB))." },
  { "en": "Apa Itu Sensor Garis (Line Sensor)?", "id": "Sensor (Mendeteksi Garis (Robot Line Follower))." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor (Mengukur Medan Magnet (Kecepatan, Posisi))." },
  { "en": "Apa Itu Encoder Absolut?", "id": "Encoder (Memberi Kode Posisi Mutlak)." },
  { "en": "Apa Itu Encoder Inkremental?", "id": "Encoder (Memberi Pulsa (Perubahan Posisi))." },
  { "en": "Apa Itu Torsi (Torque)?", "id": "Gaya Putar (Diperlukan Aktuator Motor)." },
  { "en": "Apa Itu Motor Sinkron?", "id": "Motor AC (Berputar Pada Kecepatan Sinkron)." },
  { "en": "Apa Itu Motor Asinkron (Induksi)?", "id": "Motor AC (Berputar Di Bawah Kecepatan Sinkron)." },
  { "en": "Apa Itu Rotor (Motor)?", "id": "Bagian Motor Yang Berputar." },
  { "en": "Apa Itu Stator (Motor)?", "id": "Bagian Motor Yang Diam." },
  { "en": "Apa Itu Motor DC Brushless (BLDC)?", "id": "Motor DC (Menggunakan Komutasi Elektronik)." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengontrol Motor AC (Mengatur Frekuensi)." },
  { "en": "Apa Itu Kopling (Coupling)?", "id": "Komponen (Menghubungkan Dua Poros Mekanis)." },
  { "en": "Apa Itu Rem Elektromagnetik?", "id": "Mekanisme (Pengereman Menggunakan Gaya Magnet)." },
  { "en": "Apa Itu Solenoid Valve (Katup Solenoid)?", "id": "Aktuator (Mengontrol Aliran Fluida Dengan Listrik)." },
  { "en": "Apa Itu Kontaktor (Contactor)?", "id": "Saklar Elektromekanis (Untuk Beban Daya Besar)." },
  { "en": "Apa Itu Relay Overload Termal?", "id": "Pengaman (Motor Dari Arus Berlebih, Panas)." },
  { "en": "Apa Itu ELCB (Earth Leakage Circuit Breaker)?", "id": "Pengaman (Mendeteksi Arus Bocor Ke Tanah)." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pengaman (Hubung Singkat Dan Beban Lebih)." },
  { "en": "Apa Itu MCCB (Molded Case Circuit Breaker)?", "id": "MCB (Versi Daya Arus Tinggi)." },
  { "en": "Apa Itu Inverter?", "id": "Rangkaian (Mengubah DC Menjadi AC)." },
  { "en": "Apa Itu Rectifier (Penyearah)?", "id": "Rangkaian (Mengubah AC Menjadi DC)." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Rangkaian (Mengubah Level Tegangan DC)." },
  { "en": "Apa Itu SMPS (Switch Mode Power Supply)?", "id": "Catu Daya (Regulasi Switching, Efisiensi Tinggi)." },
  { "en": "Apa Itu Regulator Linear?", "id": "Catu Daya (Regulasi Transistor, Disipasi Panas)." },
  { "en": "Apa Itu Heat Sink (Pendingin)?", "id": "Logam (Penyerap Panas Komponen Daya)." },
  { "en": "Apa Itu Pasta Termal (Thermal Paste)?", "id": "Bahan (Peningkat Hantaran Panas Ke Heatsink)." },
  { "en": "Apa Itu Dioda Bridge (Jembatan)?", "id": "Rangkaian (Empat Dioda, Penyearah Gelombang Penuh)." },
  { "en": "Apa Itu Filter Kapasitor?", "id": "Kapasitor (Menghaluskan Tegangan Ripple DC)." },
  { "en": "Apa Itu Faktor Ripple?", "id": "Ukuran (Fluktuasi AC Yang Tersisa Pada DC Output)." },
  { "en": "Apa Itu Penstabil Zener?", "id": "Rangkaian (Zener Dioda, Output Tegangan Stabil)." },
  { "en": "Apa Itu Transformator Isolasi?", "id": "Trafo (Rasio 1:1, Memisahkan Ground Galvanik)." },
  { "en": "Apa Itu Autotransformer?", "id": "Trafo (Hanya Satu Lilitan, Tidak Ada Isolasi)." },
  { "en": "Apa Itu IC (Integrated Circuit) TTL?", "id": "Keluarga Digital (Transistor-Transistor Logic)." },
  { "en": "Apa Itu IC (Integrated Circuit) CMOS?", "id": "Keluarga Digital (Daya Rendah, MOSFET)." },
  { "en": "Apa Itu IC (Integrated Circuit) Op-Amp?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Astable, Monostable)." },
  { "en": "Apa Itu Flip-Flop Tipe D?", "id": "Memori (Menyimpan Input D Saat Tepi Clock Naik)." },
  { "en": "Apa Itu Register Geser SIPO?", "id": "Input Serial, Output Paralel (74HC595)." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian (Mengukur Resistansi Tak Diketahui)." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Filter (Melewatkan Frekuensi Rendah)." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Filter (Melewatkan Frekuensi Tinggi)." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Komponen (Penghasil Clock Frekuensi Akurat)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem (Mengunci Fasa Osilator Dengan Sinyal Referensi)." },
  { "en": "Apa Itu Komponen Elektronika?", "id": "Bagian Dasar Pembentuk Rangkaian Listrik." },
  { "en": "Apa Itu Komponen Pasif?", "id": "Komponen (Tidak Memerlukan Sumber Daya Luar)." },
  { "en": "Apa Itu Komponen Aktif?", "id": "Komponen (Memerlukan Sumber Daya, Bisa Menguatkan)." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif (Menghambat Arus Listrik)." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif (Menyimpan Muatan Listrik)." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif (Menyimpan Medan Magnet)." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Aktif (Penyearah Arus)." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Aktif (Saklar Dan Penguat)." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Rangkaian Elektronik Terpadu Dalam Chip." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Sensor?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Pengubah Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog Dengan Pulsa." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Tiga Terminal)." },
  { "en": "Apa Itu Kapasitor Elektrolit?", "id": "Kapasitor Polar (Nilai Kapasitansi Besar)." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Non-Polar (Frekuensi Tinggi)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Vf Rendah (Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Kontrol Arus)." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Mode Astable Dan Monostable)." },
  { "en": "Apa Itu Regulator Linear?", "id": "IC Penstabil Tegangan (Efisiensi Rendah)." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Beban)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Tampilan Seven Segment?", "id": "Penampil Angka (Tujuh LED Baris)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair (Hemat Daya)." },
  { "en": "Apa Itu OLED (Organic LED)?", "id": "Tampilan (Piksel Cahaya Sendiri, Kontras)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Dasar Rangkaian Digital." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Digital (Penghitung Pulsa)." },
  { "en": "Apa Itu Register Geser?", "id": "Rangkaian Digital (Penggeser Data Bit)." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian (Mengukur Perubahan Resistansi Kecil)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Noise Yang Sama." },
  { "en": "Apa Itu Slew Rate (Op-Amp)?", "id": "Kecepatan Maksimal Perubahan Output." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Penguat (Linearitas Tinggi, Efisiensi Rendah)." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Kapasitor)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Pengunci Fasa Sinyal." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu (Nilai Berkelanjutan)." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Nilai 0 Dan 1)." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Demodulasi?", "id": "Memisahkan Informasi Dari Sinyal Pembawa." },
  { "en": "Apa Itu AM (Amplitude Modulation)?", "id": "Modulasi (Amplitudo Pembawa Berubah)." },
  { "en": "Apa Itu FM (Frequency Modulation)?", "id": "Modulasi (Frekuensi Pembawa Berubah)." },
  { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu Antena?", "id": "Pengubah Listrik Ke Gelombang Elektromagnetik." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Jalur PCB (Hijau)." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD (Pemanasan Oven)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat (Menghubungkan Tubuh Ke Ground)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Bentuk Sinyal." },
  { "en": "Apa Itu Logger Data (Data Logger)?", "id": "Alat Perekam Data (Jangka Waktu Panjang)." },
  { "en": "Apa Itu Baterai Li-Ion?", "id": "Baterai Isi Ulang (Kepadatan Energi Tinggi)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu Pengisian Cepat (Fast Charging)?", "id": "Mengisi Baterai Dengan Arus Tinggi." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Komponen Saklar Daya (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Photodiode (Dioda Foto)?", "id": "Dioda (Sensor Cahaya)." },
  { "en": "Apa Itu Phototransistor?", "id": "Transistor (Peka Cahaya, Lebih Sensitif)." },
  { "en": "Apa Itu Sensor PIR?", "id": "Sensor Gerak (Radiasi Panas Inframerah)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Per Langkah Presisi)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Motor DC." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator (Gerak Linear Elektromagnet)." },
  { "en": "Apa Itu Logika TTL (Transistor-Transistor Logic)?", "id": "Keluarga IC Digital (5V, Cepat)." },
  { "en": "Apa Itu Logika CMOS (Complementary MOS)?", "id": "Keluarga IC Digital (Daya Rendah)." },
  { "en": "Apa Itu Op-Amp Inverting?", "id": "Penguat (Output Membalik Fasa)." },
  { "en": "Apa Itu Op-Amp Non-Inverting?", "id": "Penguat (Output Fasa Sama)." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat (Gain = 1, Buffer Impedansi)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Kapasitor)." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Rangkaian Pengubah AC Ke DC Efisien." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Meloloskan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Meloloskan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Lolos Pita (Band Pass Filter)?", "id": "Filter (Melewatkan Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Filter Tolak Pita (Band Stop Filter)?", "id": "Filter (Meredam Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Frekuensi Cut-Off?", "id": "Frekuensi Batas (Gain Turun 3dB)." },
  { "en": "Apa Itu Faktor Kualitas (Q-Factor)?", "id": "Ukuran Kualitas (Rangkaian Resonansi Filter)." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Perilaku Rangkaian (Terhadap Perubahan Frekuensi)." },
  { "en": "Apa Itu Diagram Bode?", "id": "Grafik (Respon Frekuensi (Gain Dan Fasa))." },
  { "en": "Apa Itu Orde Filter?", "id": "Jumlah Komponen Penyimpan Energi (Menentukan Slope)." },
  { "en": "Apa Itu Slope Filter?", "id": "Laju Penurunan Gain (dB Per Dekade/Oktaf)." },
  { "en": "Apa Itu Desibel (dB)?", "id": "Satuan Logaritmik (Mengukur Rasio Gain Daya)." },
  { "en": "Apa Itu Redaman (Attenuation)?", "id": "Penurunan Kekuatan Sinyal (Rugi-Rugi Daya)." },
  { "en": "Apa Itu Bandwidth (Lebar Pita)?", "id": "Rentang Frekuensi (Yang Dapat Dilewatkan)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian (Penghasil Sinyal Periodik)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Dan Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Dan Kapasitor)." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator (Frekuensi Sangat Stabil)." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem (Mengunci Fasa Sinyal)." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian (Mengubah AC Menjadi DC)." },
  { "en": "Apa Itu Inverter?", "id": "Rangkaian (Mengubah DC Menjadi AC)." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Rangkaian (Mengubah Level Tegangan DC)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan)." },
  { "en": "Apa Itu SMPS (Switch Mode Power Supply)?", "id": "Catu Daya (Regulator Switching, Efisiensi Tinggi)." },
  { "en": "Apa Itu Regulator Linear?", "id": "Regulator (Menggunakan Transistor (Boros Daya))." },
  { "en": "Apa Itu Trafo Step-Up?", "id": "Transformator (Menaikkan Tegangan AC)." },
  { "en": "Apa Itu Trafo Step-Down?", "id": "Transformator (Menurunkan Tegangan AC)." },
  { "en": "Apa Itu Dioda Bridge (Jembatan)?", "id": "Rangkaian (Empat Dioda, Penyearah Gelombang Penuh)." },
  { "en": "Apa Itu Faktor Ripple (Riak)?", "id": "Ukuran (Fluktuasi AC Yang Tersisa Pada DC Output)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Varistor (MOV)?", "id": "Pelindung (Menyerap Lonjakan Tegangan)." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pengaman (Melebur Saat Arus Berlebih)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount)?", "id": "Komponen (Ditempel Di Permukaan PCB)." },
  { "en": "Apa Itu Komponen THT (Through-Hole)?", "id": "Komponen (Kaki Menembus Lubang PCB)." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Hijau PCB." },
  { "en": "Apa Itu Silkscreen?", "id": "Teks Putih Penanda Komponen Di PCB." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD (Pemanasan Oven)." },
  { "en": "Apa Itu Solder Wick?", "id": "Serabut Tembaga (Pembersih Sisa Timah)." },
  { "en": "Apa Itu Flux (Fluks)?", "id": "Cairan Kimia (Pembersih Oksidasi Solderan)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat Pelindung ESD (Terhubung Ke Ground)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Bentuk Sinyal." },
  { "en": "Apa Itu Probe (Osiloskop)?", "id": "Kabel Input (Menghubungkan Ke Rangkaian)." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Alat (Menganalisis Sinyal Digital Multi-Kanal)." },
  { "en": "Apa Itu Penganalisa Spektrum?", "id": "Alat (Menampilkan Sinyal Domain Frekuensi)." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Antena Dipole?", "id": "Antena Dasar (Dua Elemen Lurus)." },
  { "en": "Apa Itu Antena Monopole?", "id": "Antena (Satu Elemen, Ground Plane)." },
  { "en": "Apa Itu Baterai Li-Ion?", "id": "Baterai Isi Ulang (Kepadatan Energi Tinggi)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu Sensor PIR?", "id": "Sensor Gerak (Radiasi Panas Inframerah)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat (Strain Gauge)." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Per Langkah Presisi)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Motor DC." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator (Gerak Linear Elektromagnet)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Beban)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Photodiode?", "id": "Dioda (Sensor Cahaya)." },
  { "en": "Apa Itu Phototransistor?", "id": "Transistor (Peka Cahaya, Lebih Sensitif)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Komponen Saklar Daya (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Logika TTL?", "id": "Keluarga IC Digital (5V, Cepat)." },
  { "en": "Apa Itu Logika CMOS?", "id": "Keluarga IC Digital (Daya Rendah)." },
  { "en": "Apa Itu Op-Amp Inverting?", "id": "Penguat (Output Membalik Fasa)." },
  { "en": "Apa Itu Op-Amp Non-Inverting?", "id": "Penguat (Output Fasa Sama)." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat (Gain = 1, Buffer Impedansi)." },
  { "en": "Apa Itu Flip-Flop D?", "id": "Memori (Menyimpan Data D Saat Clock)." },
  { "en": "Apa Itu Register Geser SIPO?", "id": "Input Serial, Output Paralel (74HC595)." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Di Dalam Chip." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital (Logika Dapat Diprogram Ulang)." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Tiga Terminal)." },
  { "en": "Apa Itu Kapasitor Elektrolit?", "id": "Kapasitor Polar (Nilai Kapasitansi Besar)." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Non-Polar (Frekuensi Tinggi)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Vf Rendah (Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Kontrol Arus)." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Mode Astable Dan Monostable)." },
  { "en": "Apa Itu Regulator Linear?", "id": "IC Penstabil Tegangan (Efisiensi Rendah)." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Peta Karnaugh (K-Map)?", "id": "Metode Grafis (Menyederhanakan Persamaan Boolean)." },
  { "en": "Apa Itu Minimisasi (Digital)?", "id": "Proses (Mengurangi Jumlah Gerbang Logika)." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu Minimum (Data Stabil Sebelum Clock Edge)." },
  { "en": "Apa Itu Waktu Hold (Hold Time)?", "id": "Waktu Minimum (Data Stabil Setelah Clock Edge)." },
  { "en": "Apa Itu Metastability (Digital)?", "id": "Kondisi (Output Flip-Flop Tidak Jelas (0 Atau 1))." },
  { "en": "Apa Itu FIFO (First-In, First-Out)?", "id": "Struktur Data (Antrian, Pertama Masuk Pertama Keluar)." },
  { "en": "Apa Itu LIFO (Last-In, First-Out)?", "id": "Struktur Data (Stack, Terakhir Masuk Pertama Keluar)." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Bagian CPU (Melakukan Operasi Aritmatika Logika)." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computing)?", "id": "Arsitektur CPU (Instruksi Kompleks)." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computing)?", "id": "Arsitektur CPU (Instruksi Sederhana, Cepat)." },
  { "en": "Apa Itu Pipelining (CPU)?", "id": "Teknik (Memproses Instruksi Secara Bersamaan)." },
  { "en": "Apa Itu Cache Memory?", "id": "Memori Cepat (Menyimpan Data Sering Diakses)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer (Mengatur Ulang MCU Jika Program Macet)." },
  { "en": "Apa Itu Brown-Out Detection (BOD)?", "id": "Fitur MCU (Mereset Jika Tegangan Turun Drastis)." },
  { "en": "Apa Itu Konverter Half-Bridge?", "id": "Rangkaian Daya (Dua Saklar, Switching DC-DC/AC)." },
  { "en": "Apa Itu Konverter Full-Bridge?", "id": "Rangkaian Daya (Empat Saklar, H-Bridge Lengkap)." },
  { "en": "Apa Itu Driver Gerbang (Gate Driver)?", "id": "Rangkaian (Penyedia Arus Cepat Ke Gerbang MOSFET/IGBT)." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Pelindung (Meredam Lonjakan Tegangan Switching)." },
  { "en": "Apa Itu Freewheeling Diode?", "id": "Dioda (Mencegah Lonjakan Saat Induktor Mati)." },
  { "en": "Apa Itu Diode Recuperation?", "id": "Dioda (Mengarahkan Arus Kembali Ke Sumber Di Inverter)." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter RF (Berdasarkan Gelombang Akustik Permukaan)." },
  { "en": "Apa Itu Balun (Balanced-Unbalanced)?", "id": "Komponen RF (Menghubungkan Jalur Seimbang Dan Tidak Seimbang)." },
  { "en": "Apa Itu Mixer (RF)?", "id": "Rangkaian (Mengubah Frekuensi Sinyal)." },
  { "en": "Apa Itu Heterodyne Receiver?", "id": "Penerima (Menggunakan Mixer Untuk IF (Intermediate Frequency))." },
  { "en": "Apa Itu IF (Intermediate Frequency)?", "id": "Frekuensi Tengah (Digunakan Di Penerima Radio)." },
  { "en": "Apa Itu Modulasi QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi Digital (Mengubah Amplitudo Dan Fasa)." },
  { "en": "Apa Itu Modulasi OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Modulasi (Menggunakan Banyak Pembawa Sempit)." },
  { "en": "Apa Itu Demodulator?", "id": "Rangkaian (Memisahkan Informasi Dari Sinyal Pembawa)." },
  { "en": "Apa Itu Noise Figure (Angka Noise)?", "id": "Ukuran (Degradasi SNR Oleh Komponen)." },
  { "en": "Apa Itu SNR (Signal-to-Noise Ratio)?", "id": "Rasio (Kekuatan Sinyal Terhadap Kekuatan Noise)." },
  { "en": "Apa Itu DMM (Digital Multimeter)?", "id": "Alat Ukur (Tampilkan Hasil Secara Digital)." },
  { "en": "Apa Itu Resolusi DMM?", "id": "Jumlah Digit (Terkecil Yang Dapat Ditampilkan)." },
  { "en": "Apa Itu Akurasi DMM?", "id": "Perbedaan (Nilai Yang Ditampilkan Dan Nilai Sebenarnya)." },
  { "en": "Apa Itu Aliasing (Sinyal)?", "id": "Kesalahan (Sampling Terlalu Rendah, Frekuensi Salah)." },
  { "en": "Apa Itu Teorema Nyquist?", "id": "Frekuensi Sampling (Minimal Dua Kali Frekuensi Tertinggi Sinyal)." },
  { "en": "Apa Itu Komponen Piezoelektrik?", "id": "Komponen (Menghasilkan Tegangan Dari Tekanan Mekanik)." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor (Mengukur Regangan (Perubahan Resistansi))." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor (Mengukur Berat/Gaya)." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu (Resistansi Logam Murni (Presisi))." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu (Menggunakan Sambungan Dua Logam Beda)." },
  { "en": "Apa Itu Transduser?", "id": "Perangkat (Mengubah Satu Bentuk Energi Ke Bentuk Lain)." },
  { "en": "Apa Itu Aktuator?", "id": "Perangkat (Mengubah Sinyal Listrik Menjadi Gerakan)." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor (Gerak Langsung Tanpa Rotasi)." },
  { "en": "Apa Itu Komutator (Motor DC)?", "id": "Cincin (Mengubah Arah Arus Di Kumparan Rotor)." },
  { "en": "Apa Itu Brush (Sikat) Motor?", "id": "Penghantar (Menghubungkan Stator Ke Komutator)." },
  { "en": "Apa Itu Back EMF (Counter EMF)?", "id": "Tegangan Balik (Dihasilkan Motor Saat Berputar)." },
  { "en": "Apa Itu Sensor Hall Effect?", "id": "Sensor (Mengukur Medan Magnet)." },
  { "en": "Apa Itu Penguat Arus (Current Amplifier)?", "id": "Penguat (Menaikkan Arus, Gain Tegangan Rendah)." },
  { "en": "Apa Itu Penguat Tegangan (Voltage Amplifier)?", "id": "Penguat (Menaikkan Tegangan, Gain Arus Rendah)." },
  { "en": "Apa Itu Gain (Penguatan)?", "id": "Rasio (Output Terhadap Input Sinyal)." },
  { "en": "Apa Itu Noise Floor?", "id": "Level Daya Noise (Di Bawah Sinyal)." },
  { "en": "Apa Itu JFET (Junction Field-Effect Transistor)?", "id": "FET (Gerbang Tipe PN Junction)." },
  { "en": "Apa Itu MOSFET Tipe Enhancement?", "id": "MOSFET (Dibutuhkan Tegangan Positif Untuk ON (N-Channel))." },
  { "en": "Apa Itu MOSFET Tipe Depletion?", "id": "MOSFET (ON Tanpa Tegangan Gerbang)." },
  { "en": "Apa Itu Darlington Pair?", "id": "Konfigurasi BJT (Penguatan Arus Sangat Tinggi)." },
  { "en": "Apa Itu Thyristor?", "id": "Keluarga Semikonduktor Daya (SCR, TRIAC)." },
  { "en": "Apa Itu UJT (Unijunction Transistor)?", "id": "Transistor (Digunakan Sebagai Osilator Relaksasi)." },
  { "en": "Apa Itu Komponen Surface Mount Technology (SMT)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Komponen Through-Hole Technology (THT)?", "id": "Komponen (Kaki Menembus Lubang PCB)." },
  { "en": "Apa Itu Vias (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung (PCB (Biasanya Hijau))." },
  { "en": "Apa Itu Silkscreen?", "id": "Lapisan (Teks Penanda Komponen Di PCB)." },
  { "en": "Apa Itu Ground Plane?", "id": "Lapisan Tembaga Luas (Untuk Ground Di PCB)." },
  { "en": "Apa Itu Power Plane?", "id": "Lapisan Tembaga Luas (Untuk Jalur Tegangan Di PCB)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMT (Pemanasan Oven)." },
  { "en": "Apa Itu Solder Paste?", "id": "Pasta (Campuran Timah Dan Fluks Untuk SMT)." },
  { "en": "Apa Itu Sekring (Fuse) Cepat (Fast Blow)?", "id": "Sekring (Putus Seketika Saat Arus Berlebih)." },
  { "en": "Apa Itu Sekring Lambat (Slow Blow)?", "id": "Sekring (Memiliki Waktu Tunda Sebelum Putus)." },
  { "en": "Apa Itu Varistor (MOV)?", "id": "Pelindung (Menyerap Lonjakan Tegangan Tinggi)." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppression)?", "id": "Pelindung (Menyerap Lonjakan Tegangan Cepat)." },
  { "en": "Apa Itu Proteksi ESD?", "id": "Metode (Mencegah Kerusakan Akibat Pelepasan Listrik Statis)." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik (Noise)." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kemampuan (Rangkaian Berfungsi Baik Di Lingkungan EMI)." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif (Meredam Noise Frekuensi Tinggi)." },
  { "en": "Apa Itu Kapasitor Decoupling?", "id": "Kapasitor (Meredam Fluktuasi Tegangan (Dekat IC))." },
  { "en": "Apa Itu Current Loop (4-20 mA)?", "id": "Standar (Transmisi Sinyal Analog Jarak Jauh)." },
  { "en": "Apa Itu Isolator Galvanik?", "id": "Pemisah (Jalur Listrik Untuk Mencegah Arus Bocor)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC (Isolasi Galvanik Menggunakan Cahaya)." },
  { "en": "Apa Itu Trafo Isolasi?", "id": "Trafo (Rasio 1:1, Memberi Isolasi Galvanik)." },
  { "en": "Apa Itu Autotransformer?", "id": "Trafo (Hanya Satu Lilitan, Tidak Ada Isolasi Galvanik)." },
  { "en": "Apa Itu Inverter Gelombang Kotak (Square Wave)?", "id": "Inverter (Output Sinyal Kotak Sederhana)." },
  { "en": "Apa Itu Inverter Gelombang Sinus Modifikasi (Modified Sine Wave)?", "id": "Inverter (Output Beberapa Tingkat Tegangan)." },
  { "en": "Apa Itu Inverter Gelombang Sinus Murni (Pure Sine Wave)?", "id": "Inverter (Output Sinyal Sinus Murni)." },
  { "en": "Apa Itu Topologi Power Supply Half-Bridge?", "id": "Topologi SMPS (Dua Saklar Seri, Kapasitor Split)." },
  { "en": "Apa Itu Topologi Power Supply Full-Bridge?", "id": "Topologi SMPS (Empat Saklar, Efisiensi Tinggi)." },
  { "en": "Apa Itu Topologi Power Supply Flyback?", "id": "Topologi SMPS (Sederhana, Daya Rendah)." },
  { "en": "Apa Itu Topologi Power Supply Forward?", "id": "Topologi SMPS (Lebih Kompleks Dari Flyback, Daya Menengah)." },
  { "en": "Apa Itu Regulasi Jalur (Line Regulation)?", "id": "Ukuran (Seberapa Baik Output Stabil Terhadap Input)." },
  { "en": "Apa Itu Regulasi Beban (Load Regulation)?", "id": "Ukuran (Seberapa Baik Output Stabil Terhadap Beban)." },
  { "en": "Apa Itu Ripple Rejection?", "id": "Kemampuan (Regulator Menolak Ripple Input)." },
  { "en": "Apa Itu Drop-Out Voltage (LDO)?", "id": "Tegangan Minimal (Input Di Atas Output Agar LDO Stabil)." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Penguat (Menguatkan Perbedaan Antara Dua Input)." },
  { "en": "Apa Itu Op-Amp Chopper-Stabilized?", "id": "Op-Amp (Noise Rendah, Offset Tegangan Sangat Kecil)." },
  { "en": "Apa Itu Op-Amp Rail-to-Rail?", "id": "Op-Amp (Output Mencapai Tegangan VCC Dan GND)." },
  { "en": "Apa Itu Sensor Kapasitif?", "id": "Sensor (Mendeteksi Benda (Perubahan Kapasitansi))." },
  { "en": "Apa Itu Sensor Induktif?", "id": "Sensor (Mendeteksi Benda Logam (Perubahan Induktansi))." },
  { "en": "Apa Itu Sensor Suhu Digital?", "id": "Sensor Suhu (Output Berupa Data Biner (I2C/SPI))." },
  { "en": "Apa Itu Sensor Suhu Analog?", "id": "Sensor Suhu (Output Berupa Tegangan/Arus)." },
  { "en": "Apa Itu Thermistor NTC?", "id": "Termistor (Resistansi Turun Saat Suhu Naik)." },
  { "en": "Apa Itu Thermistor PTC?", "id": "Termistor (Resistansi Naik Saat Suhu Naik)." },
  { "en": "Apa Itu Load Cell Bridge?", "id": "Rangkaian (Menggunakan Load Cell (Jembatan Wheatstone))." },
  { "en": "Apa Itu Amplifier Instrumentasi?", "id": "Penguat Diferensial (Presisi Tinggi (Sensor))." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter LPF (Ditempatkan Sebelum ADC Untuk Mencegah Aliasing)." },
  { "en": "Apa Itu Nyquist Rate?", "id": "Laju Sampling (Minimum Dua Kali Frekuensi Tertinggi Sinyal)." },
  { "en": "Apa Itu Sampling Rate?", "id": "Laju (Berapa Banyak Sampel Diambil Per Detik)." },
  { "en": "Apa Itu Kuat Medan Listrik (Electric Field Strength)?", "id": "Intensitas (Medan Listrik Di Suatu Titik)." },
  { "en": "Apa Itu Kerapatan Fluks Magnet (Magnetic Flux Density)?", "id": "Jumlah Garis Fluks (Per Satuan Luas)." },
  { "en": "Apa Itu Komponen Elektronika?", "id": "Bagian Dasar Pembentuk Rangkaian Listrik." },
  { "en": "Apa Itu Komponen Pasif?", "id": "Tidak Memerlukan Sumber Daya Luar." },
  { "en": "Apa Itu Komponen Aktif?", "id": "Memerlukan Sumber Daya, Bisa Menguatkan." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif Penghambat Arus Listrik." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif Penyimpan Medan Magnet." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Aktif Penyearah Arus." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Aktif Saklar Dan Penguat." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Rangkaian Elektronik Terpadu Dalam Chip." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Sensor?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Pengubah Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog Dengan Pulsa." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Tiga Terminal)." },
  { "en": "Apa Itu Kapasitor Elektrolit?", "id": "Kapasitor Polar (Nilai Kapasitansi Besar)." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Non-Polar (Frekuensi Tinggi)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Vf Rendah (Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Kontrol Arus)." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Mode Astable Dan Monostable)." },
  { "en": "Apa Itu Regulator Linear?", "id": "IC Penstabil Tegangan (Efisiensi Rendah)." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Beban)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Tampilan Seven Segment?", "id": "Penampil Angka (Tujuh LED Baris)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair (Hemat Daya)." },
  { "en": "Apa Itu OLED (Organic LED)?", "id": "Tampilan (Piksel Cahaya Sendiri, Kontras)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Dasar Rangkaian Digital." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Digital (Penghitung Pulsa)." },
  { "en": "Apa Itu Register Geser?", "id": "Rangkaian Digital (Penggeser Data Bit)." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian (Mengukur Perubahan Resistansi Kecil)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Noise Yang Sama." },
  { "en": "Apa Itu Slew Rate (Op-Amp)?", "id": "Kecepatan Maksimal Perubahan Output." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Penguat (Linearitas Tinggi, Efisiensi Rendah)." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Kapasitor)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Pengunci Fasa Sinyal." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Rangkaian Pengubah AC Ke DC Efisien." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Meloloskan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Meloloskan Frekuensi Tinggi." },
  { "en": "Apa Itu Trafo Step-Up?", "id": "Transformator (Menaikkan Tegangan AC)." },
  { "en": "Apa Itu Trafo Step-Down?", "id": "Transformator (Menurunkan Tegangan AC)." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Antena?", "id": "Pengubah Listrik Ke Gelombang Elektromagnetik." },
  { "en": "Apa Itu Antena Dipole?", "id": "Antena Dasar (Dua Elemen Lurus)." },
  { "en": "Apa Itu Antena Monopole?", "id": "Antena (Satu Elemen, Ground Plane)." },
  { "en": "Apa Itu Baterai Li-Ion?", "id": "Baterai Isi Ulang (Kepadatan Energi Tinggi)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu Sensor PIR?", "id": "Sensor Gerak (Radiasi Panas Inframerah)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Per Langkah Presisi)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Motor DC." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator (Gerak Linear Elektromagnet)." },
  { "en": "Apa Itu Logika TTL?", "id": "Keluarga IC Digital (5V, Cepat)." },
  { "en": "Apa Itu Logika CMOS?", "id": "Keluarga IC Digital (Daya Rendah)." },
  { "en": "Apa Itu Op-Amp Inverting?", "id": "Penguat (Output Membalik Fasa)." },
  { "en": "Apa Itu Op-Amp Non-Inverting?", "id": "Penguat (Output Fasa Sama)." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat (Gain = 1, Buffer Impedansi)." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu (Nilai Berkelanjutan)." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Nilai 0 Dan 1)." },
  { "en": "Apa Itu Modulasi AM?", "id": "Modulasi (Amplitudo Pembawa Berubah)." },
  { "en": "Apa Itu Modulasi FM?", "id": "Modulasi (Frekuensi Pembawa Berubah)." },
  { "en": "Apa Itu Flip-Flop D?", "id": "Memori (Menyimpan Data D Saat Clock)." },
  { "en": "Apa Itu Register Geser SIPO?", "id": "Input Serial, Output Paralel (74HC595)." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Di Dalam Chip." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital (Logika Dapat Diprogram Ulang)." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Tiga Terminal)." },
  { "en": "Apa Itu Kapasitor Elektrolit?", "id": "Kapasitor Polar (Nilai Kapasitansi Besar)." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Non-Polar (Frekuensi Tinggi)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Vf Rendah (Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Kontrol Arus)." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Komponen Saklar Daya (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Kapasitor Tantalum?", "id": "Kapasitor Polar (Kecil, Kapasitansi Tinggi, Stabilitas Baik)." },
  { "en": "Apa Itu Induktor Toroidal?", "id": "Induktor (Lilitan Pada Inti Berbentuk Donat (Fluks Tertutup))." },
  { "en": "Apa Itu Komponen Ferrite Bead?", "id": "Komponen Pasif (Meredam Noise Frekuensi Tinggi)." },
  { "en": "Apa Itu Thermistor NTC?", "id": "Termistor (Resistansi Turun Saat Suhu Naik)." },
  { "en": "Apa Itu Thermistor PTC?", "id": "Termistor (Resistansi Naik Saat Suhu Naik)." },
  { "en": "Apa Itu Varistor (MOV)?", "id": "Pelindung (Menyerap Lonjakan Tegangan Tinggi)." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppression)?", "id": "Pelindung (Menyerap Lonjakan Tegangan Sangat Cepat)." },
  { "en": "Apa Itu JFET (Junction Field-Effect Transistor)?", "id": "FET (Gerbang Tipe PN Junction, Kontrol Tegangan)." },
  { "en": "Apa Itu MOSFET Tipe Enhancement?", "id": "MOSFET (Dibutuhkan Tegangan Gerbang Untuk ON)." },
  { "en": "Apa Itu MOSFET Tipe Depletion?", "id": "MOSFET (ON Tanpa Tegangan Gerbang)." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Transistor Daya (Gabungan MOSFET Dan BJT)." },
  { "en": "Apa Itu Darlington Pair?", "id": "Konfigurasi BJT (Penguatan Arus Sangat Tinggi)." },
  { "en": "Apa Itu UJT (Unijunction Transistor)?", "id": "Transistor (Digunakan Sebagai Osilator Relaksasi)." },
  { "en": "Apa Itu Dioda Varaktor?", "id": "Dioda (Kapasitansi Berubah Tergantung Tegangan Balik)." },
  { "en": "Apa Itu Dioda Tunnel?", "id": "Dioda (Resistansi Negatif, Switching Sangat Cepat)." },
  { "en": "Apa Itu TRIAC?", "id": "Komponen Saklar Daya (Dua Arah AC)." },
  { "en": "Apa Itu DIAC?", "id": "Komponen Pemicu TRIAC (Dua Arah AC)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya (Searah DC)." },
  { "en": "Apa Itu Voltage Regulator Linear?", "id": "IC Penstabil Tegangan (Efisiensi Rendah)." },
  { "en": "Apa Itu Regulator Switching?", "id": "IC Penstabil Tegangan (Efisiensi Tinggi)." },
  { "en": "Apa Itu LDO (Low Dropout Regulator)?", "id": "Regulator Linear (Input Dekat Output)." },
  { "en": "Apa Itu Konverter Buck-Boost?", "id": "Regulator Switching (Bisa Naik Atau Turun)." },
  { "en": "Apa Itu Konverter Flyback?", "id": "Regulator Switching Terisolasi (Trafo Khusus)." },
  { "en": "Apa Itu Driver Gerbang (Gate Driver)?", "id": "Rangkaian (Penyedia Arus Cepat Ke Gerbang MOSFET)." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Pelindung (Meredam Lonjakan Tegangan Switching)." },
  { "en": "Apa Itu Freewheeling Diode?", "id": "Dioda (Mencegah Lonjakan Saat Induktor Mati)." },
  { "en": "Apa Itu Arus Inrush (Inrush Current)?", "id": "Lonjakan Arus (Saat Alat Dinyalakan Pertama Kali)." },
  { "en": "Apa Itu Ripple Factor?", "id": "Ukuran (Fluktuasi AC Yang Tersisa Pada DC Output)." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya (Diubah Menjadi Kerja Nyata, Watt)." },
  { "en": "Apa Itu Daya Reaktif (Q)?", "id": "Daya (Dipertukarkan Medan Magnet/Listrik, VAR)." },
  { "en": "Apa Itu Daya Semu (S)?", "id": "Daya Total (Vrms x Arms, VA)." },
  { "en": "Apa Itu Faktor Daya (PF)?", "id": "Rasio Daya Nyata Dan Daya Semu (P/S)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Upaya Membuat PF Mendekati Satu." },
  { "en": "Apa Itu Harmonik (Harmonic)?", "id": "Frekuensi (Kelipatan Bilangan Bulat Frekuensi Dasar)." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran (Distorsi Harmonik Total Sinyal)." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Penguat (Menguatkan Perbedaan Antara Dua Input)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR?", "id": "Kemampuan Op-Amp (Menolak Sinyal Sama)." },
  { "en": "Apa Itu Slew Rate?", "id": "Kecepatan Maksimal Perubahan Tegangan Output." },
  { "en": "Apa Itu Gain Loop Terbuka?", "id": "Penguatan Maksimal Op-Amp (Tanpa Umpan Balik)." },
  { "en": "Apa Itu Gain Loop Tertutup?", "id": "Penguatan Op-Amp (Dengan Umpan Balik)." },
  { "en": "Apa Itu Op-Amp Rail-to-Rail?", "id": "Op-Amp (Output Mencapai Tegangan VCC Dan GND)." },
  { "en": "Apa Itu Komparator?", "id": "Op-Amp (Pembanding Dua Tegangan)." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator (Dengan Histeresis (Anti Noise))." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator (Op-Amp, Menghasilkan Sinus)." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter (Hanya Menggunakan R, L, C)." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter (Menggunakan Komponen Aktif, Op-Amp)." },
  { "en": "Apa Itu Filter Lolos Pita (Band Pass Filter)?", "id": "Filter (Melewatkan Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Filter Tolak Pita (Band Stop Filter)?", "id": "Filter (Meredam Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Frekuensi Cut-Off?", "id": "Frekuensi Batas (Gain Turun 3dB)." },
  { "en": "Apa Itu Faktor Kualitas (Q-Factor)?", "id": "Ukuran Kualitas (Rangkaian Resonansi Filter)." },
  { "en": "Apa Itu Orde Filter?", "id": "Jumlah Komponen Penyimpan Energi (Menentukan Slope)." },
  { "en": "Apa Itu Slope Filter?", "id": "Laju Penurunan Gain (dB Per Dekade/Oktaf)." },
  { "en": "Apa Itu Diagram Bode?", "id": "Grafik (Respon Frekuensi (Gain Dan Fasa))." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Penguat (Kombinasi Kelas A Dan Kelas B)." },
  { "en": "Apa Itu Crossover Distortion?", "id": "Distorsi (Terjadi Saat Transistor Kelas B Berpindah)." },
  { "en": "Apa Itu Jitter (Sinyal)?", "id": "Variasi Waktu (Ketidakstabilan Pulsa Clock)." },
  { "en": "Apa Itu Phase Noise?", "id": "Noise (Berhubungan Dengan Ketidakstabilan Fasa Osilator)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem (Mengunci Fasa Sinyal)." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Sinyal Sinkron?", "id": "Sinyal (Semua Perubahan Diatur Oleh Clock Sama)." },
  { "en": "Apa Itu Sinyal Asinkron?", "id": "Sinyal (Perubahan Tidak Tergantung Clock Sama)." },
  { "en": "Apa Itu Peta Karnaugh (K-Map)?", "id": "Metode Grafis (Menyederhanakan Persamaan Boolean)." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu Minimum (Data Stabil Sebelum Clock Edge)." },
  { "en": "Apa Itu Waktu Hold (Hold Time)?", "id": "Waktu Minimum (Data Stabil Setelah Clock Edge)." },
  { "en": "Apa Itu Metastability (Digital)?", "id": "Kondisi (Output Flip-Flop Tidak Jelas)." },
  { "en": "Apa Itu FIFO (First-In, First-Out)?", "id": "Struktur Data (Antrian, Pertama Masuk Pertama Keluar)." },
  { "en": "Apa Itu LIFO (Last-In, First-Out)?", "id": "Struktur Data (Stack, Terakhir Masuk Pertama Keluar)." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Bagian CPU (Melakukan Operasi Aritmatika Logika)." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computing)?", "id": "Arsitektur CPU (Instruksi Kompleks)." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computing)?", "id": "Arsitektur CPU (Instruksi Sederhana, Cepat)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer (Mengatur Ulang MCU Jika Program Macet)." },
  { "en": "Apa Itu Brown-Out Detection (BOD)?", "id": "Fitur MCU (Mereset Jika Tegangan Turun Drastis)." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "OS (Respon Tugas Terjamin Batas Waktu)." },
  { "en": "Apa Itu Task (Tugas) (RTOS)?", "id": "Unit Program (Independen Yang Dijadwalkan)." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Pengunci (Mencegah Akses Bersamaan Ke Sumber Daya)." },
  { "en": "Apa Itu Semaphore?", "id": "Mekanisme Sinkronisasi (Mengontrol Akses)." },
  { "en": "Apa Itu Message Queue (Antrian Pesan)?", "id": "Saluran (Komunikasi Data Antar Task)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka (Debugging Dan Pemrograman IC)." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka (Debug Dengan Dua Pin Data Clock)." },
  { "en": "Apa Itu Over-The-Air (OTA) Update?", "id": "Pembaruan (Firmware Nirkabel (Wi-Fi/Bluetooth))." },
  { "en": "Apa Itu Sensor Piezoresistif?", "id": "Sensor (Hambatan Berubah Akibat Tekanan/Regangan)." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor (Mengukur Regangan (Perubahan Resistansi))." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat (Strain Gauge)." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu (Resistansi Logam Murni (Presisi))." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu (Sambungan Dua Logam Beda)." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor (Mengukur Medan Magnet)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Sensor Proximity Induktif?", "id": "Sensor (Mendeteksi Logam (Medan Magnet))." },
  { "en": "Apa Itu Sensor Proximity Kapasitif?", "id": "Sensor (Mendeteksi Logam Atau Non-Logam)." },
  { "en": "Apa Itu Sensor Fotoelektrik?", "id": "Sensor (Mendeteksi Cahaya)." },
  { "en": "Apa Itu Sensor Suhu Inframerah?", "id": "Sensor (Mengukur Radiasi Panas (Tanpa Kontak))." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor (Gerak Langsung Tanpa Rotasi)." },
  { "en": "Apa Itu Komutator (Motor DC)?", "id": "Cincin (Mengubah Arah Arus Di Kumparan Rotor)." },
  { "en": "Apa Itu Motor DC Brushless (BLDC)?", "id": "Motor DC (Menggunakan Komutasi Elektronik)." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengontrol Motor AC (Mengatur Frekuensi)." },
  { "en": "Apa Itu Relay Overload Termal?", "id": "Pengaman (Motor Dari Arus Berlebih, Panas)." },
  { "en": "Apa Itu ELCB (Earth Leakage Circuit Breaker)?", "id": "Pengaman (Mendeteksi Arus Bocor Ke Tanah)." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pengaman (Hubung Singkat Dan Beban Lebih)." },
  { "en": "Apa Itu MCCB (Molded Case Circuit Breaker)?", "id": "MCB (Versi Daya Arus Tinggi)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Sistem Catu Daya (Menyediakan Daya Darurat)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat Pelindung ESD (Terhubung Ke Ground)." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Komunikasi (Dua Kawat Berpasangan, Meredam Noise)." },
  { "en": "Apa Itu Fiber Optic?", "id": "Kabel Komunikasi (Sinyal Cahaya, Bandwidth Sangat Tinggi)." },
  { "en": "Apa Itu Protokol RS-485?", "id": "Standar Komunikasi Serial (Jarak Jauh, Multi-Drop)." },
  { "en": "Apa Itu Protokol CAN (Controller Area Network)?", "id": "Bus Komunikasi (Mobil, Otomasi Industri)." },
  { "en": "Apa Itu Protokol Ethernet?", "id": "Standar Jaringan Data (LAN)." },
  { "en": "Apa Itu Wi-Fi?", "id": "Teknologi Nirkabel (Jaringan Lokal)." },
  { "en": "Apa Itu Bluetooth?", "id": "Teknologi Nirkabel (Jarak Pendek, Daya Rendah)." },
  { "en": "Apa Itu Zigbee?", "id": "Protokol Nirkabel (Jaringan Mesh, Daya Sangat Rendah)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi Nirkabel (Jarak Sangat Dekat)." },
  { "en": "Apa Itu RFID (Radio Frequency Identification)?", "id": "Teknologi (Menggunakan Gelombang Radio (Tag, Reader))." },
  { "en": "Apa Itu Modem (Modulator-Demodulator)?", "id": "Perangkat (Mengubah Sinyal Digital Ke Analog (Transmisi))." },
  { "en": "Apa Itu Multiplexing?", "id": "Teknik (Menggabungkan Banyak Sinyal Ke Satu Saluran)." },
  { "en": "Apa Itu Demultiplexing?", "id": "Teknik (Memisahkan Kembali Sinyal Gabungan)." },
  { "en": "Apa Itu Modulasi AM?", "id": "Modulasi (Amplitudo Pembawa Berubah)." },
  { "en": "Apa Itu Modulasi FM?", "id": "Modulasi (Frekuensi Pembawa Berubah)." },
  { "en": "Apa Itu Modulasi FSK (Frequency Shift Keying)?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu Modulasi PSK (Phase Shift Keying)?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena (Pengarah Gain Tinggi)." },
  { "en": "Apa Itu Antena Patch?", "id": "Antena Datar (Untuk Frekuensi Microwave)." },
  { "en": "Apa Itu Gain Antena?", "id": "Ukuran (Efisiensi Antena Dalam Mengarahkan Daya)." },
  { "en": "Apa Itu Polarisasi Antena?", "id": "Arah (Medan Listrik Gelombang Radio)." },
  { "en": "Apa Itu Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu EMI Shielding?", "id": "Perisai Logam (Melindungi Rangkaian Dari Noise Elektromagnetik)." },
  { "en": "Apa Itu Kapasitor Decoupling?", "id": "Kapasitor (Meredam Fluktuasi Tegangan (Dekat IC))." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif (Meredam Noise Frekuensi Tinggi)." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Filter (Melewatkan Frekuensi Rendah)." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Filter (Melewatkan Frekuensi Tinggi)." },
  { "en": "Apa Itu Filter Lolos Pita (Band Pass Filter)?", "id": "Filter (Melewatkan Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Filter Tolak Pita (Band Stop Filter)?", "id": "Filter (Meredam Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter LPF (Ditempatkan Sebelum ADC Untuk Mencegah Aliasing)." },
  { "en": "Apa Itu Nyquist Rate?", "id": "Laju Sampling (Minimum Dua Kali Frekuensi Tertinggi Sinyal)." },
  { "en": "Apa Itu Sampling Rate?", "id": "Laju (Berapa Banyak Sampel Diambil Per Detik)." },
  { "en": "Apa Itu ADC Pipa (Pipeline ADC)?", "id": "ADC (Resolusi Tinggi, Kecepatan Tinggi)." },
  { "en": "Apa Itu ADC SAR (Successive Approximation Register)?", "id": "ADC (Seimbang Antara Kecepatan Dan Akurasi)." },
  { "en": "Apa Itu ADC Delta-Sigma?", "id": "ADC (Resolusi Sangat Tinggi, Kecepatan Rendah)." },
  { "en": "Apa Itu DAC R-2R Ladder?", "id": "DAC (Menggunakan Jaringan Resistor R Dan 2R)." },
  { "en": "Apa Itu Glitch (DAC)?", "id": "Lonjakan Sinyal (Terjadi Saat Output DAC Berubah)." },
  { "en": "Apa Itu Monotonicity (DAC)?", "id": "Sifat DAC (Output Selalu Naik Saat Input Naik)." },
  { "en": "Apa Itu LDO (Low Dropout Regulator)?", "id": "Regulator Linear (Input Dekat Output)." },
  { "en": "Apa Itu Dropout Voltage?", "id": "Tegangan Minimal (Input Di Atas Output Agar LDO Stabil)." },
  { "en": "Apa Itu Regulator Switching?", "id": "IC Penstabil Tegangan (Efisiensi Tinggi)." },
  { "en": "Apa Itu Konverter Buck-Boost?", "id": "Regulator Switching (Bisa Naik Atau Turun)." },
  { "en": "Apa Itu Konverter Flyback?", "id": "Regulator Switching Terisolasi (Trafo Khusus)." },
  { "en": "Apa Itu Konverter Forward?", "id": "Regulator Switching Terisolasi (Transformator)." },
  { "en": "Apa Itu Arus Inrush?", "id": "Lonjakan Arus (Saat Alat Dinyalakan Pertama Kali)." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya (Diubah Menjadi Kerja Nyata, Watt)." },
  { "en": "Apa Itu Faktor Daya (PF)?", "id": "Rasio Daya Nyata Dan Daya Semu (P/S)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Upaya Membuat PF Mendekati Satu." },
  { "en": "Apa Itu Thyristor?", "id": "Keluarga Semikonduktor Daya (SCR, TRIAC)." },
  { "en": "Apa Itu JFET (Junction Field-Effect Transistor)?", "id": "FET (Gerbang Tipe PN Junction, Kontrol Tegangan)." },
  { "en": "Apa Itu MOSFET Tipe Enhancement?", "id": "MOSFET (Dibutuhkan Tegangan Gerbang Untuk ON)." },
  { "en": "Apa Itu MOSFET Tipe Depletion?", "id": "MOSFET (ON Tanpa Tegangan Gerbang)." },
  { "en": "Apa Itu Darlington Pair?", "id": "Konfigurasi BJT (Penguatan Arus Sangat Tinggi)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya (Searah DC)." },
  { "en": "Apa Itu TRIAC?", "id": "Komponen Saklar Daya (Dua Arah AC)." },
  { "en": "Apa Itu DIAC?", "id": "Komponen Pemicu TRIAC (Dua Arah AC)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Sensor Piezoresistif?", "id": "Sensor (Hambatan Berubah Akibat Tekanan/Regangan)." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor (Mengukur Regangan (Perubahan Resistansi))." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat (Strain Gauge)." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu (Resistansi Logam Murni (Presisi))." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu (Sambungan Dua Logam Beda)." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor (Mengukur Medan Magnet)." },
  { "en": "Apa Itu Sensor LVDT (Linear Variable Differential Transformer)?", "id": "Sensor (Mengukur Perpindahan Linear)." },
  { "en": "Apa Itu Torsi (Torque)?", "id": "Gaya Putar (Diperlukan Aktuator Motor)." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor (Gerak Langsung Tanpa Rotasi)." },
  { "en": "Apa Itu Motor DC Brushless (BLDC)?", "id": "Motor DC (Menggunakan Komutasi Elektronik)." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengontrol Motor AC (Mengatur Frekuensi)." },
  { "en": "Apa Itu Encoder Absolut?", "id": "Encoder (Memberi Kode Posisi Mutlak)." },
  { "en": "Apa Itu Encoder Inkremental?", "id": "Encoder (Memberi Pulsa (Perubahan Posisi))." },
  { "en": "Apa Itu Komutator (Motor DC)?", "id": "Cincin (Mengubah Arah Arus Di Kumparan Rotor)." },
  { "en": "Apa Itu Brush (Sikat) Motor?", "id": "Penghantar (Menghubungkan Stator Ke Komutator)." },
  { "en": "Apa Itu Back EMF (Counter EMF)?", "id": "Tegangan Balik (Dihasilkan Motor Saat Berputar)." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Industri (Kontrol Proses Otomasi)." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem (Mengontrol Dan Memantau Proses Industri Besar)." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka (Interaksi Manusia Dengan Mesin Industri)." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "OS (Respon Tugas Terjamin Batas Waktu)." },
  { "en": "Apa Itu Task (Tugas) (RTOS)?", "id": "Unit Program (Independen Yang Dijadwalkan)." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Pengunci (Mencegah Akses Bersamaan Ke Sumber Daya)." },
  { "en": "Apa Itu Semaphore?", "id": "Mekanisme Sinkronisasi (Mengontrol Akses)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka (Debugging Dan Pemrograman IC)." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka (Debug Dengan Dua Pin Data Clock)." },
  { "en": "Apa Itu Peta Karnaugh (K-Map)?", "id": "Metode Grafis (Menyederhanakan Persamaan Boolean)." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu Minimum (Data Stabil Sebelum Clock Edge)." },
  { "en": "Apa Itu Waktu Hold (Hold Time)?", "id": "Waktu Minimum (Data Stabil Setelah Clock Edge)." },
  { "en": "Apa Itu Metastability (Digital)?", "id": "Kondisi (Output Flip-Flop Tidak Jelas)." },
  { "en": "Apa Itu FIFO (First-In, First-Out)?", "id": "Struktur Data (Antrian, Pertama Masuk Pertama Keluar)." },
  { "en": "Apa Itu LIFO (Last-In, First-Out)?", "id": "Struktur Data (Stack, Terakhir Masuk Pertama Keluar)." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Bagian CPU (Melakukan Operasi Aritmatika Logika)." },
  { "en": "Apa Itu CISC?", "id": "Arsitektur CPU (Instruksi Kompleks)." },
  { "en": "Apa Itu RISC?", "id": "Arsitektur CPU (Instruksi Sederhana, Cepat)." },
  { "en": "Apa Itu Cache Memory?", "id": "Memori Cepat (Menyimpan Data Sering Diakses)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer (Mengatur Ulang MCU Jika Program Macet)." },
  { "en": "Apa Itu Brown-Out Detection (BOD)?", "id": "Fitur MCU (Mereset Jika Tegangan Turun Drastis)." },
  { "en": "Apa Itu Jitter (Sinyal)?", "id": "Variasi Waktu (Ketidakstabilan Pulsa Clock)." },
  { "en": "Apa Itu Phase Noise?", "id": "Noise (Berhubungan Dengan Ketidakstabilan Fasa Osilator)." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Penguat (Kombinasi Kelas A Dan Kelas B)." },
  { "en": "Apa Itu Crossover Distortion?", "id": "Distorsi (Terjadi Saat Transistor Kelas B Berpindah)." },
  { "en": "Apa Itu Slew Rate?", "id": "Kecepatan Maksimal Perubahan Tegangan Output." },
  { "en": "Apa Itu CMRR?", "id": "Kemampuan Op-Amp (Menolak Sinyal Sama)." },
  { "en": "Apa Itu Op-Amp Rail-to-Rail?", "id": "Op-Amp (Output Mencapai Tegangan VCC Dan GND)." },
  { "en": "Apa Itu Kapasitor Variabel (Varco)?", "id": "Kapasitor (Nilai Kapasitansi Dapat Diubah)." },
  { "en": "Di Mana Kapasitor Variabel Digunakan?", "id": "Rangkaian Tuning Radio (Pemilih Frekuensi)." },
  { "en": "Apa Itu Kapasitor Trim (Trimmer)?", "id": "Kapasitor Variabel (Untuk Penyetelan Presisi)." },
  { "en": "Apa Itu Dioda Varaktor (Varicap)?", "id": "Dioda (Kapasitansi Diatur Tegangan Balik)." },
  { "en": "Apa Itu Kapasitansi Parasitik?", "id": "Kapasitansi (Tidak Diinginkan, Antar Jalur)." },
  { "en": "Apa Itu Induktansi Parasitik?", "id": "Induktansi (Tidak Diinginkan, Jalur PCB)." },
  { "en": "Apa Itu Resistor Film Karbon?", "id": "Resistor (Film Karbon Di Substrat Keramik, Murah)." },
  { "en": "Apa Itu Resistor Film Logam?", "id": "Resistor (Film Logam Tipis, Presisi Tinggi)." },
  { "en": "Apa Itu Resistor Wirewound?", "id": "Resistor (Kawat Logam Dililit, Daya Tinggi)." },
  { "en": "Apa Itu Resistor Shunt?", "id": "Resistor Nilai Sangat Rendah (Pengukur Arus Besar)." },
  { "en": "Apa Itu Toleransi Resistor?", "id": "Batas Penyimpangan Nilai Aktual Dari Nilai Nominal." },
  { "en": "Apa Itu Koefisien Suhu Resistor (TCR)?", "id": "Perubahan Resistansi (Terhadap Perubahan Suhu)." },
  { "en": "Apa Itu Dioda LED (Light Emitting Diode) Inframerah?", "id": "LED (Memancarkan Cahaya Inframerah (Tak Terlihat))." },
  { "en": "Apa Itu Dioda LED (Light Emitting Diode) Ultraviolet?", "id": "LED (Memancarkan Cahaya UV)." },
  { "en": "Apa Itu LED (Light Emitting Diode) RGB?", "id": "LED (Mengandung Tiga Warna (Merah, Hijau, Biru))." },
  { "en": "Apa Itu LED (Light Emitting Diode) Addressable?", "id": "LED (Dapat Diatur Warna Individual Dengan Data Serial)." },
  { "en": "Apa Itu Photodiode Avalanche?", "id": "Photodiode (Penguatan Internal (Efek Longsoran))." },
  { "en": "Apa Itu PIN Diode?", "id": "Dioda (Lapisan Intrinsik (I), Saklar RF)." },
  { "en": "Apa Itu Rectifier (Penyearah)?", "id": "Rangkaian (Mengubah AC Menjadi DC)." },
  { "en": "Apa Itu Inverter?", "id": "Rangkaian (Mengubah DC Menjadi AC)." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Rangkaian (Mengubah Level Tegangan DC)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Konverter Buck-Boost?", "id": "Regulator Switching (Bisa Naik Atau Turun)." },
  { "en": "Apa Itu SMPS (Switch Mode Power Supply)?", "id": "Catu Daya (Regulasi Switching, Efisiensi Tinggi)." },
  { "en": "Apa Itu Flyback Converter?", "id": "Topologi SMPS (Sederhana, Terisolasi, Daya Rendah)." },
  { "en": "Apa Itu Forward Converter?", "id": "Topologi SMPS (Trafo Dengan De-magnetizing Winding)." },
  { "en": "Apa Itu Half-Bridge Converter?", "id": "Topologi SMPS (Dua Saklar Seri, Kapasitor Split)." },
  { "en": "Apa Itu Full-Bridge Converter?", "id": "Topologi SMPS (Empat Saklar, Efisiensi Daya Tinggi)." },
  { "en": "Apa Itu Regulator Linear?", "id": "Regulator (Transistor Di Daerah Aktif, Panas)." },
  { "en": "Apa Itu LDO (Low Dropout Regulator)?", "id": "Regulator Linear (Input Dekat Output)." },
  { "en": "Apa Itu Dropout Voltage?", "id": "Tegangan Input Minimal (Di Atas Output Agar Stabil)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya (Searah DC)." },
  { "en": "Apa Itu TRIAC?", "id": "Komponen Saklar Daya (Dua Arah AC)." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Transistor Daya (Gabungan MOSFET Dan BJT)." },
  { "en": "Apa Itu MOSFET Daya?", "id": "MOSFET (Struktur Vertikal, Arus Besar)." },
  { "en": "Apa Itu Driver Gerbang (Gate Driver)?", "id": "IC (Penyedia Arus Cepat Ke Gerbang MOSFET/IGBT)." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Pelindung (Meredam Lonjakan Tegangan Switching)." },
  { "en": "Apa Itu Freewheeling Diode?", "id": "Dioda (Mencegah Lonjakan Saat Induktor Mati)." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya (Diubah Menjadi Kerja Nyata, Watt)." },
  { "en": "Apa Itu Daya Reaktif (Q)?", "id": "Daya (Dipertukarkan Medan Magnet/Listrik, VAR)." },
  { "en": "Apa Itu Daya Semu (S)?", "id": "Daya Total (Vrms x Arms, VA)." },
  { "en": "Apa Itu Faktor Daya (PF)?", "id": "Rasio Daya Nyata Dan Daya Semu (P/S)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Upaya Membuat PF Mendekati Satu." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran (Distorsi Harmonik Total Sinyal)." },
  { "en": "Apa Itu Harmonik?", "id": "Frekuensi (Kelipatan Bilangan Bulat Frekuensi Dasar)." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Penguat (Menguatkan Perbedaan Antara Dua Input)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Op-Amp (Menolak Sinyal Sama)." },
  { "en": "Apa Itu Op-Amp Rail-to-Rail?", "id": "Op-Amp (Output Mencapai Tegangan VCC Dan GND)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Dan Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Dan Kapasitor)." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator (Frekuensi Sangat Stabil)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem (Mengunci Fasa Sinyal)." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Filter Lolos Pita?", "id": "Filter (Melewatkan Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Filter Tolak Pita?", "id": "Filter (Meredam Rentang Frekuensi Tertentu)." },
  { "en": "Apa Itu Orde Filter?", "id": "Jumlah Komponen Penyimpan Energi (Menentukan Slope)." },
  { "en": "Apa Itu Sinyal Sinkron?", "id": "Sinyal (Semua Perubahan Diatur Oleh Clock Sama)." },
  { "en": "Apa Itu Sinyal Asinkron?", "id": "Sinyal (Perubahan Tidak Tergantung Clock Sama)." },
  { "en": "Apa Itu Peta Karnaugh (K-Map)?", "id": "Metode Grafis (Menyederhanakan Persamaan Boolean)." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu Minimum (Data Stabil Sebelum Clock Edge)." },
  { "en": "Apa Itu Waktu Hold (Hold Time)?", "id": "Waktu Minimum (Data Stabil Setelah Clock Edge)." },
  { "en": "Apa Itu Metastability (Digital)?", "id": "Kondisi (Output Flip-Flop Tidak Jelas)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer (Mengatur Ulang MCU Jika Program Macet)." },
  { "en": "Apa Itu Brown-Out Detection (BOD)?", "id": "Fitur MCU (Mereset Jika Tegangan Turun Drastis)." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "OS (Respon Tugas Terjamin Batas Waktu)." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Pengunci (Mencegah Akses Bersamaan Ke Sumber Daya)." },
  { "en": "Apa Itu Semaphore?", "id": "Mekanisme Sinkronisasi (Mengontrol Akses)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka (Debugging Dan Pemrograman IC)." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka (Debug Dengan Dua Pin Data Clock)." },
  { "en": "Apa Itu Sensor Piezoresistif?", "id": "Sensor (Hambatan Berubah Akibat Tekanan/Regangan)." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor (Mengukur Regangan (Perubahan Resistansi))." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat (Strain Gauge)." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu (Resistansi Logam Murni (Presisi))." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu (Sambungan Dua Logam Beda)." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor (Mengukur Medan Magnet)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor (Gerak Langsung Tanpa Rotasi)." },
  { "en": "Apa Itu Motor DC Brushless (BLDC)?", "id": "Motor DC (Menggunakan Komutasi Elektronik)." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengontrol Motor AC (Mengatur Frekuensi)." },
  { "en": "Apa Itu Relay Overload Termal?", "id": "Pengaman (Motor Dari Arus Berlebih, Panas)." },
  { "en": "Apa Itu ELCB (Earth Leakage Circuit Breaker)?", "id": "Pengaman (Mendeteksi Arus Bocor Ke Tanah)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Sistem Catu Daya (Menyediakan Daya Darurat)." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Komunikasi (Dua Kawat Berpasangan, Meredam Noise)." },
  { "en": "Apa Itu Fiber Optic?", "id": "Kabel Komunikasi (Sinyal Cahaya, Bandwidth Sangat Tinggi)." },
  { "en": "Apa Itu Protokol RS-485?", "id": "Standar Komunikasi Serial (Jarak Jauh, Multi-Drop)." },
  { "en": "Apa Itu Protokol CAN (Controller Area Network)?", "id": "Bus Komunikasi (Mobil, Otomasi Industri)." },
  { "en": "Apa Itu Protokol Zigbee?", "id": "Protokol Nirkabel (Jaringan Mesh, Daya Sangat Rendah)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi Nirkabel (Jarak Sangat Dekat)." },
  { "en": "Apa Itu RFID (Radio Frequency Identification)?", "id": "Teknologi (Menggunakan Gelombang Radio (Tag, Reader))." },
  { "en": "Apa Itu Modulasi FSK?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu Modulasi PSK?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena (Pengarah Gain Tinggi)." },
  { "en": "Apa Itu Antena Patch?", "id": "Antena Datar (Untuk Frekuensi Microwave)." },
  { "en": "Apa Itu Gain Antena?", "id": "Ukuran (Efisiensi Antena Dalam Mengarahkan Daya)." },
  { "en": "Apa Itu Polarisasi Antena?", "id": "Arah (Medan Listrik Gelombang Radio)." },
  { "en": "Apa Itu Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu EMI Shielding?", "id": "Perisai Logam (Melindungi Rangkaian Dari Noise Elektromagnetik)." },
  { "en": "Apa Itu Kapasitor Decoupling?", "id": "Kapasitor (Meredam Fluktuasi Tegangan (Dekat IC))." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif (Meredam Noise Frekuensi Tinggi)." },
  { "en": "Apa Itu Komponen Elektronika?", "id": "Bagian Dasar Pembentuk Rangkaian Listrik." },
  { "en": "Apa Itu Komponen Pasif?", "id": "Tidak Memerlukan Sumber Daya Luar." },
  { "en": "Apa Itu Komponen Aktif?", "id": "Memerlukan Sumber Daya, Bisa Menguatkan." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif Penghambat Arus Listrik." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif Penyimpan Medan Magnet." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Aktif Penyearah Arus." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Aktif Saklar Dan Penguat." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Rangkaian Elektronik Terpadu Dalam Chip." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "IC Penguat Diferensial Gain Tinggi." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Sensor?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Pengubah Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Daya Analog Dengan Pulsa." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu UART (Universal Asynchronous R/T)?", "id": "Komunikasi Serial (TX, RX)." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat)." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Bus Serial (Empat Kabel, Cepat)." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Menghubungkan Pin Input Ke VCC." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Menghubungkan Pin Input Ke GND." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Intensitas Cahaya." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Peka Suhu (NTC/PTC)." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Tiga Terminal)." },
  { "en": "Apa Itu Kapasitor Elektrolit?", "id": "Kapasitor Polar (Nilai Kapasitansi Besar)." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Non-Polar (Frekuensi Tinggi)." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda (Penstabil Tegangan)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Vf Rendah (Switching Cepat)." },
  { "en": "Apa Itu Transistor BJT NPN?", "id": "BJT (Basis Positif, Kontrol Arus)." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET (Saklar Daya Arus Besar)." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Serbaguna (Mode Astable Dan Monostable)." },
  { "en": "Apa Itu Regulator Linear?", "id": "IC Penstabil Tegangan (Efisiensi Rendah)." },
  { "en": "Apa Itu Regulator Switching?", "id": "Regulator (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Konverter Buck?", "id": "Regulator Switching (Penurun Tegangan DC)." },
  { "en": "Apa Itu Konverter Boost?", "id": "Regulator Switching (Penaik Tegangan DC)." },
  { "en": "Apa Itu Relay (Relai)?", "id": "Saklar Elektromekanis (Kontrol Beban)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Saklar Elektronik, Tanpa Gerakan)." },
  { "en": "Apa Itu Optocoupler?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan)." },
  { "en": "Apa Itu Tampilan Seven Segment?", "id": "Penampil Angka (Tujuh LED Baris)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair (Hemat Daya)." },
  { "en": "Apa Itu OLED (Organic LED)?", "id": "Tampilan (Piksel Cahaya Sendiri, Kontras)." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Dasar Rangkaian Digital." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Digital (Penghitung Pulsa)." },
  { "en": "Apa Itu Register Geser?", "id": "Rangkaian Digital (Penggeser Data Bit)." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian (Mengukur Perubahan Resistansi Kecil)." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi (Sensor)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Noise Yang Sama." },
  { "en": "Apa Itu Slew Rate (Op-Amp)?", "id": "Kecepatan Maksimal Perubahan Output." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Penguat (Linearitas Tinggi, Efisiensi Rendah)." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator (Menggunakan Resistor Kapasitor)." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator (Menggunakan Induktor Kapasitor)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Pengunci Fasa Sinyal." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi Dikontrol Tegangan)." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Rangkaian Pengubah AC Ke DC Efisien." },
  { "en": "Apa Itu Filter Lolos Bawah (LPF)?", "id": "Meloloskan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Atas (HPF)?", "id": "Meloloskan Frekuensi Tinggi." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount)?", "id": "Komponen (Disolder Di Permukaan PCB)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD (Pemanasan Oven)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat (Menghubungkan Tubuh Ke Ground)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Bentuk Sinyal." },
  { "en": "Apa Itu Baterai Li-Ion?", "id": "Baterai Isi Ulang (Kepadatan Energi Tinggi)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Komponen Saklar Daya (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu Photodiode (Dioda Foto)?", "id": "Dioda (Sensor Cahaya)." },
  { "en": "Apa Itu Phototransistor?", "id": "Transistor (Peka Cahaya, Lebih Sensitif)." },
  { "en": "Apa Itu Sensor PIR?", "id": "Sensor Gerak (Radiasi Panas Inframerah)." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Jarak (Gelombang Suara)." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Posisi Sudut Putaran." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Per Langkah Presisi)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Motor DC." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator (Gerak Linear Elektromagnet)." },
  { "en": "Apa Itu Logika TTL?", "id": "Keluarga IC Digital (5V, Cepat)." },
  { "en": "Apa Itu Logika CMOS?", "id": "Keluarga IC Digital (Daya Rendah)." },
  { "en": "Apa Itu Op-Amp Inverting?", "id": "Penguat (Output Membalik Fasa)." },
  { "en": "Apa Itu Op-Amp Non-Inverting?", "id": "Penguat (Output Fasa Sama)." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat (Gain = 1, Buffer Impedansi)." },
  { "en": "Apa Itu Flip-Flop D?", "id": "Memori (Menyimpan Data D Saat Clock)." },
  { "en": "Apa Itu Register Geser SIPO?", "id": "Input Serial, Output Paralel (74HC595)." },
  { "en": "Apa Itu Mikrokontroler (MCU)?", "id": "Komputer Kecil Di Dalam Chip." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital (Logika Dapat Diprogram Ulang)." },
  { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni (Tanpa Bahan Pengotor (Doping))." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor (Telah Diberi Bahan Pengotor (Doping))." },
  { "en": "Apa Itu Bahan Doping?", "id": "Zat Pengotor (Ditambahkan Untuk Mengubah Konduktivitas)." },
  { "en": "Apa Itu Semikonduktor Tipe P?", "id": "Semikonduktor (Kelebihan Hole (Pembawa Muatan Positif))." },
  { "en": "Apa Itu Semikonduktor Tipe N?", "id": "Semikonduktor (Kelebihan Elektron (Pembawa Muatan Negatif))." },
  { "en": "Apa Itu P-N Junction?", "id": "Batasan (Antara Tipe P Dan Tipe N (Dasar Dioda))." },
  { "en": "Apa Itu Depletion Region?", "id": "Daerah Kosong (Tepat Di Sambungan P-N (Tanpa Pembawa Bebas))." },
  { "en": "Apa Itu Tegangan Breakdown Zener?", "id": "Tegangan Balik (Dioda Zener Mulai Menghantar Stabil)." },
  { "en": "Apa Itu Efek Hall?", "id": "Tegangan (Muncul Tegak Lurus Arus Dan Medan Magnet)." },
  { "en": "Apa Itu Peltier Effect?", "id": "Pendinginan/Pemanasan (Terjadi Saat Arus Lewat Sambungan Dua Konduktor)." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor (Gerak Langsung Tanpa Rotasi)." },
  { "en": "Apa Itu Back EMF (Counter EMF)?", "id": "Tegangan Balik (Dihasilkan Motor Saat Berputar)." },
  { "en": "Apa Itu Komutator (Motor DC)?", "id": "Cincin (Mengubah Arah Arus Di Kumparan Rotor)." },
  { "en": "Apa Itu Motor DC Brushless (BLDC)?", "id": "Motor DC (Menggunakan Komutasi Elektronik)." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengontrol Motor AC (Mengatur Frekuensi)." },
  { "en": "Apa Itu Torsi (Torque)?", "id": "Gaya Putar (Diperlukan Aktuator Motor)." },
  { "en": "Apa Itu Servo Motor?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu Sensor LVDT?", "id": "Sensor (Mengukur Perpindahan Linear)." },
  { "en": "Apa Itu Sensor Proximity Induktif?", "id": "Sensor (Mendeteksi Logam (Medan Magnet))." },
  { "en": "Apa Itu Sensor Proximity Kapasitif?", "id": "Sensor (Mendeteksi Logam Atau Non-Logam)." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor (Mengukur Regangan (Perubahan Resistansi))." },
  { "en": "Apa Itu Penganalisa Spektrum?", "id": "Alat (Menampilkan Sinyal Domain Frekuensi)." },
  { "en": "Apa Itu DMM True RMS?", "id": "Multimeter (Mengukur Nilai RMS Sebenarnya)." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Alat (Menganalisis Sinyal Digital Multi-Kanal)." },
  { "en": "Apa Itu Penganalisa Jaringan VNA?", "id": "Alat (Mengukur Respon Jaringan RF)." },
  { "en": "Apa Itu Time Domain Reflectometer (TDR)?", "id": "Alat (Mencari Kerusakan Kabel (Pantulan Sinyal))." },
  { "en": "Apa Itu Osiloskop Digital Storage (DSO)?", "id": "Osiloskop (Menyimpan Dan Menganalisis Sinyal Digital)." },
  { "en": "Apa Itu Antena Dipole Setengah Gelombang?", "id": "Antena (Panjang Sekitar Setengah Panjang Gelombang)." },
  { "en": "Apa Itu Antena Log-Periodic?", "id": "Antena (Bandwidth Lebar, Gain Sedang)." },
  { "en": "Apa Itu Polarisasi Sirkular?", "id": "Polarisasi Antena (Medan Berputar (Satelit))." },
  { "en": "Apa Itu Balun (Balanced-Unbalanced)?", "id": "Komponen RF (Menghubungkan Jalur Seimbang Dan Tidak Seimbang)." },
  { "en": "Apa Itu Return Loss (Kerugian Balik)?", "id": "Ukuran (Daya Yang Dipantulkan Kembali Oleh Beban)." },
  { "en": "Apa Itu Isolator Galvanik?", "id": "Pemisah (Jalur Listrik Untuk Mencegah Arus Bocor/Ground Loop)." },
  { "en": "Apa Itu Trafo Isolasi?", "id": "Trafo (Rasio 1:1, Memberi Isolasi Galvanik)." },
  { "en": "Apa Itu Optoisolator?", "id": "IC Isolasi (Cahaya, Memisahkan Tegangan Tinggi)." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter LPF (Ditempatkan Sebelum ADC Untuk Mencegah Aliasing)." },
  { "en": "Apa Itu Nyquist Rate?", "id": "Laju Sampling (Minimum Dua Kali Frekuensi Tertinggi Sinyal)." },
  { "en": "Apa Itu ADC Delta-Sigma?", "id": "ADC (Resolusi Sangat Tinggi, Kecepatan Rendah)." },
  { "en": "Apa Itu DAC R-2R Ladder?", "id": "DAC (Menggunakan Jaringan Resistor R Dan 2R)." },
  { "en": "Apa Itu Monotonicity (DAC)?", "id": "Sifat DAC (Output Selalu Naik Saat Input Naik)." },
  { "en": "Apa Itu Regulator LDO?", "id": "Regulator Linear (Input Dekat Output)." },
  { "en": "Apa Itu Drop-Out Voltage (LDO)?", "id": "Tegangan Minimal (Input Di Atas Output Agar LDO Stabil)." },
  { "en": "Apa Itu Power Good Pin?", "id": "Pin Sinyal (Menunjukkan Output Power Supply Stabil)." },
  { "en": "Apa Itu Mode Kontrol Arus (Current Mode Control)?", "id": "Metode Kontrol Regulator Switching (Umpan Balik Arus)." },
  { "en": "Apa Itu Mode Kontrol Tegangan (Voltage Mode Control)?", "id": "Metode Kontrol Regulator Switching (Umpan Balik Tegangan)." },
  { "en": "Apa Itu Inrush Current?", "id": "Lonjakan Arus (Saat Alat Dinyalakan Pertama Kali)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Upaya Membuat PF Mendekati Satu." },
  { "en": "Apa Itu Jitter (Sinyal)?", "id": "Variasi Waktu (Ketidakstabilan Pulsa Clock)." },
  { "en": "Apa Itu Phase Noise?", "id": "Noise (Berhubungan Dengan Ketidakstabilan Fasa Osilator)." },
  { "en": "Apa Itu Virtual Ground (Tanah Maya)?", "id": "Titik (Op-Amp Dengan Umpan Balik (Tegangan Nol))." },
  { "en": "Apa Itu Penguat Integrator?", "id": "Op-Amp (Output Adalah Integral Input)." },
  { "en": "Apa Itu Penguat Differentiator?", "id": "Op-Amp (Output Adalah Turunan Input)." },
  { "en": "Apa Itu Penguat Komparator?", "id": "Op-Amp (Pembanding Dua Tegangan)." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator (Dengan Histeresis (Anti Noise))." },
  { "en": "Apa Itu Op-Amp Chopper-Stabilized?", "id": "Op-Amp (Noise Rendah, Offset Tegangan Sangat Kecil)." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat (Efisiensi Tinggi, Switching)." },
  { "en": "Apa Itu Crossover Distortion?", "id": "Distorsi (Terjadi Saat Transistor Kelas B Berpindah)." },
  { "en": "Apa Itu MUX (Multiplexer)?", "id": "Perangkat Digital (Memilih Satu Dari Banyak Input)." },
  { "en": "Apa Itu DEMUX (Demultiplexer)?", "id": "Perangkat Digital (Mengarahkan Satu Input Ke Banyak Output)." },
  { "en": "Apa Itu Decoder?", "id": "Rangkaian Digital (Mengubah Kode Ke Output Unik)." },
  { "en": "Apa Itu Encoder?", "id": "Rangkaian Digital (Mengubah Input Unik Ke Kode)." },
  { "en": "Apa Itu Parity Bit (Bit Paritas)?", "id": "Bit Tambahan (Mendeteksi Kesalahan Transmisi Data)." },
  { "en": "Apa Itu Hamming Code?", "id": "Kode (Mendeteksi Dan Memperbaiki Kesalahan Data)." },
  { "en": "Apa Itu Bus Contention?", "id": "Konflik (Dua Perangkat Berusaha Mengendalikan Bus Bersamaan)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka (Debugging Dan Pemrograman IC)." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka (Debug Dengan Dua Pin Data Clock)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer (Mengatur Ulang MCU Jika Program Macet)." },
  { "en": "Apa Itu Brown-Out Detection (BOD)?", "id": "Fitur MCU (Mereset Jika Tegangan Turun Drastis)." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "OS (Respon Tugas Terjamin Batas Waktu)." },
  { "en": "Apa Itu Mutex?", "id": "Pengunci (Mencegah Akses Bersamaan Ke Sumber Daya)." },
  { "en": "Apa Itu Semaphore?", "id": "Mekanisme Sinkronisasi (Mengontrol Akses)." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Komunikasi (Dua Kawat Berpasangan, Meredam Noise)." },
  { "en": "Apa Itu Fiber Optic?", "id": "Kabel Komunikasi (Sinyal Cahaya, Bandwidth Sangat Tinggi)." },
  { "en": "Apa Itu Protokol RS-485?", "id": "Standar Komunikasi Serial (Jarak Jauh, Multi-Drop)." },
  { "en": "Apa Itu Protokol CAN?", "id": "Bus Komunikasi (Mobil, Otomasi Industri)." },
  { "en": "Apa Itu Zigbee?", "id": "Protokol Nirkabel (Jaringan Mesh, Daya Sangat Rendah)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi Nirkabel (Jarak Sangat Dekat)." },
  { "en": "Apa Itu RFID?", "id": "Teknologi (Menggunakan Gelombang Radio (Tag, Reader))." },
  { "en": "Apa Itu Modem?", "id": "Perangkat (Mengubah Sinyal Digital Ke Analog (Transmisi))." },
  { "en": "Apa Itu Multiplexing?", "id": "Teknik (Menggabungkan Banyak Sinyal Ke Satu Saluran)." },
  { "en": "Apa Itu Modulasi FSK?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu Modulasi PSK?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu Filter Keramik?", "id": "Filter RF (Menggunakan Bahan Keramik Resonansi)." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter RF (Berdasarkan Gelombang Akustik Permukaan)." },
  { "en": "Apa Itu Dielectric Constant (Konstanta Dielektrik)?", "id": "Ukuran (Kemampuan Bahan Menyimpan Energi Listrik)." },
  { "en": "Apa Itu Microstrip Line?", "id": "Jalur Transmisi (Sinyal RF Di Permukaan PCB)." },
  { "en": "Apa Itu Stripline?", "id": "Jalur Transmisi (Sinyal RF Terkubur Di Dalam PCB)." },
  { "en": "Apa Itu Trace Width (Lebar Jalur)?", "id": "Lebar Jalur Tembaga Di PCB (Pengaruh Impedansi)." },
  { "en": "Apa Itu Annular Ring?", "id": "Cincin Tembaga (Mengelilingi Lubang Komponen THT Di PCB)." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Hijau PCB." },
  { "en": "Apa Itu Silkscreen?", "id": "Teks Putih Penanda Komponen Di PCB." },
  { "en": "Apa Itu Bill of Materials (BOM)?", "id": "Daftar (Semua Komponen Di Rangkaian)." },
  { "en": "Apa Itu Flying Probe Test?", "id": "Metode Pengujian PCB (Probe Bergerak Cepat)." },
  { "en": "Apa Itu ESD Shielding?", "id": "Perisai (Melindungi Rangkaian Dari Pelepasan Listrik Statis)." },
  { "en": "Apa Itu EMI Shielding?", "id": "Perisai Logam (Melindungi Rangkaian Dari Noise Elektromagnetik)." },
  { "en": "Apa Itu Kapasitor Decoupling?", "id": "Kapasitor (Meredam Fluktuasi Tegangan (Dekat IC))." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif (Meredam Noise Frekuensi Tinggi)." },
  { "en": "Apa Itu Resistor Film Karbon?", "id": "Resistor (Film Karbon Di Substrat Keramik, Murah)." },
  { "en": "Apa Itu Resistor Film Logam?", "id": "Resistor (Film Logam Tipis, Presisi Tinggi)." },
  { "en": "Apa Itu Resistor Wirewound?", "id": "Resistor (Kawat Logam Dililit, Daya Tinggi)." },
  { "en": "Apa Itu Kapasitor Tantalum?", "id": "Kapasitor Polar (Kecil, Kapasitansi Tinggi, Stabilitas Baik)." },
  { "en": "Apa Itu Induktor Toroidal?", "id": "Induktor (Lilitan Pada Inti Berbentuk Donat (Fluks Tertutup))." },
  { "en": "Apa Itu Thyristor?", "id": "Keluarga Semikonduktor Daya (SCR, TRIAC)." },
  { "en": "Apa Itu DIAC?", "id": "Komponen Pemicu TRIAC (Dua Arah AC)." },
  { "en": "Apa Itu MOSFET SiC (Silicon Carbide)?", "id": "MOSFET (Bahan Semikonduktor Daya Tinggi, Switching Sangat Cepat)." },
  { "en": "Apa Itu Transistor GaN (Gallium Nitride)?", "id": "Transistor Daya (Bahan Baru, Efisiensi Dan Frekuensi Sangat Tinggi)." },
  { "en": "Apa Itu Current Sense Resistor?", "id": "Resistor Presisi (Nilai Sangat Rendah Untuk Mengukur Arus Besar)." },
  { "en": "Apa Itu Resistor Jaringan (Resistor Network)?", "id": "Beberapa Resistor (Dalam Satu Kemasan IC)." },
  { "en": "Apa Itu Resistor Anti-Surge?", "id": "Resistor (Dirancang Mampu Menerima Lonjakan Daya Tinggi Jangka Pendek)." },
  { "en": "Apa Itu Kapasitor Film Polipropilena?", "id": "Kapasitor Non-Polar (Stabilitas Tinggi, Audio/Resonansi)." },
  { "en": "Apa Itu Kapasitor Mylar?", "id": "Kapasitor Film (Dielektrik Polyester, Serbaguna)." },
  { "en": "Apa Itu Kapasitor Trimmer?", "id": "Kapasitor Variabel (Untuk Penyetelan Presisi, Sekali Atur)." },
  { "en": "Apa Itu Induktor Mode Umum (Common Mode Choke)?", "id": "Induktor (Meredam Noise Yang Sama Di Kedua Jalur)." },
  { "en": "Apa Itu Saturasi Inti Induktor?", "id": "Kondisi (Inti Induktor Tidak Dapat Menyimpan Medan Magnet Lagi)." },
  { "en": "Apa Itu Induktor Air Core?", "id": "Induktor (Tanpa Inti Ferrite/Logam, Digunakan Untuk RF)." },
  { "en": "Apa Itu Dioda Pin?", "id": "Dioda (Lapisan Intrinsik (I), Digunakan Sebagai Saklar RF)." },
  { "en": "Apa Itu Dioda Esaki (Tunnel Diode)?", "id": "Dioda (Resistansi Negatif, Switching Sangat Cepat)." },
  { "en": "Apa Itu Photodarlington?", "id": "Transistor Foto (Sensitivitas Sangat Tinggi (Gabungan Photodiode Dan Darlington))." },
  { "en": "Apa Itu Transistor UJT (Unijunction Transistor)?", "id": "Transistor (Digunakan Sebagai Osilator Relaksasi)." },
  { "en": "Apa Itu Shunt Regulator?", "id": "Regulator (Mengalihkan Arus Ke Ground Untuk Menstabilkan Tegangan)." },
  { "en": "Apa Itu IC Supervisory (Supervisor IC)?", "id": "IC (Memantau Tegangan Input/Clock, Menghasilkan Sinyal Reset)." },
  { "en": "Apa Itu Isolator Galvanik?", "id": "Pemisah (Jalur Listrik Untuk Mencegah Arus Bocor/Ground Loop)." },
  { "en": "Apa Itu Trafo Isolasi?", "id": "Trafo (Rasio 1:1, Memberi Isolasi Galvanik)." },
  { "en": "Apa Itu Catu Daya Mode Linear?", "id": "Catu Daya (Regulasi Berdasarkan Disipasi Daya, Efisiensi Rendah)." },
  { "en": "Apa Itu Mode Batas Kontinu (CCM)?", "id": "Mode Operasi Konverter Switching (Arus Induktor Tidak Pernah Nol)." },
  { "en": "Apa Itu Mode Batas Diskontinu (DCM)?", "id": "Mode Operasi Konverter Switching (Arus Induktor Nol Di Setiap Siklus)." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya (Diubah Menjadi Kerja Nyata, Watt)." },
  { "en": "Apa Itu Daya Reaktif (Q)?", "id": "Daya (Dipertukarkan Medan Magnet/Listrik, VAR)." },
  { "en": "Apa Itu Daya Semu (S)?", "id": "Daya Total (Vrms x Arms, VA)." },
  { "en": "Apa Itu Power Factor (Faktor Daya)?", "id": "Rasio Daya Nyata Dan Daya Semu (P/S)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Upaya Membuat PF Mendekati Satu." },
  { "en": "Apa Itu Penguat Transkonduktansi (OTA)?", "id": "Op-Amp (Output Adalah Arus Yang Dikontrol Tegangan Input)." },
  { "en": "Apa Itu Gain Bandwidth Product (GBWP)?", "id": "Ukuran (Op-Amp (Gain Dikali Bandwidth))." },
  { "en": "Apa Itu Settling Time (Op-Amp)?", "id": "Waktu (Dibutuhkan Output Untuk Mencapai Nilai Akhir Stabil)." },
  { "en": "Apa Itu Tegangan Offset Input?", "id": "Tegangan DC (Diperlukan Antar Input Agar Output Nol)." },
  { "en": "Apa Itu Penguat Integrator?", "id": "Op-Amp (Output Adalah Integral Input)." },
  { "en": "Apa Itu Penguat Differentiator?", "id": "Op-Amp (Output Adalah Turunan Input)." },
  { "en": "Apa Itu Op-Amp Chopper-Stabilized?", "id": "Op-Amp (Noise Rendah, Offset Tegangan Sangat Kecil)." },
  { "en": "Apa Itu Filter Chebyshev?", "id": "Filter (Respon Frekuensi Dengan Ripple Di Band Pass/Stop)." },
  { "en": "Apa Itu Filter Butterworth?", "id": "Filter (Respon Frekuensi Datar Di Band Pass)." },
  { "en": "Apa Itu Filter Bessel?", "id": "Filter (Respon Fasa Paling Linear (Untuk Pulsa))." },
  { "en": "Apa Itu Logika Asinkron?", "id": "Rangkaian Digital (Tidak Ada Clock Global Yang Mengatur Semua Perubahan)." },
  { "en": "Apa Itu Logika Sinkron?", "id": "Rangkaian Digital (Semua Perubahan Diatur Oleh Clock Sama)." },
  { "en": "Apa Itu Kode Gray?", "id": "Kode Digital (Hanya Satu Bit Berubah Antar Nilai Berurutan)." },
  { "en": "Apa Itu VRAM (Video RAM)?", "id": "Jenis Memori (Digunakan Untuk Menyimpan Data Gambar)." },
  { "en": "Apa Itu EEPROM?", "id": "Memori Non-Volatile (Dapat Dihapus Dan Diprogram Secara Listrik)." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "IC Digital (Logika Dapat Diprogram Ulang, Lebih Sederhana Dari FPGA)." },
  { "en": "Apa Itu Netlist?", "id": "Daftar (Semua Komponen Dan Koneksi Mereka Di Rangkaian)." },
  { "en": "Apa Itu Panelization (PCB)?", "id": "Proses (Menggabungkan Beberapa PCB Kecil Ke Panel Besar)." },
  { "en": "Apa Itu Flying Probe Test?", "id": "Metode Pengujian PCB (Probe Bergerak Cepat, Tanpa Jig Khusus)." },
  { "en": "Apa Itu In-Circuit Test (ICT)?", "id": "Metode Pengujian PCB (Menggunakan Jig Untuk Mengakses Semua Titik Uji)." },
  { "en": "Apa Itu Finish Permukaan PCB HASL?", "id": "Proses Permukaan PCB (Hot Air Solder Leveling, Timah Panas)." },
  { "en": "Apa Itu Finish Permukaan PCB ENIG?", "id": "Proses Permukaan PCB (Electroless Nickel Immersion Gold, Kualitas Tinggi)." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Hijau PCB." },
  { "en": "Apa Itu Silkscreen?", "id": "Teks Putih Penanda Komponen Di PCB." },
  { "en": "Apa Itu Layer Stacking (Tumpukan Lapisan PCB)?", "id": "Urutan (Lapisan Tembaga Dan Dielektrik Di PCB Multi-Layer)." },
  { "en": "Apa Itu Waveguide?", "id": "Struktur (Menyalurkan Gelombang Elektromagnetik (RF Tinggi))." },
  { "en": "Apa Itu Stripline?", "id": "Jalur Transmisi (Sinyal RF Terkubur Di Dalam PCB)." },
  { "en": "Apa Itu Microstrip Line?", "id": "Jalur Transmisi (Sinyal RF Di Permukaan PCB)." },
  { "en": "Apa Itu Smith Chart?", "id": "Alat Grafis (Pencocokan Impedansi Dan Analisis RF)." },
  { "en": "Apa Itu RF Mixer?", "id": "Komponen RF (Menggabungkan Dua Frekuensi Menghasilkan Output Baru)." },
  { "en": "Apa Itu Frekuensi Intermediate (IF)?", "id": "Frekuensi Tengah (Sinyal Dikonversi Untuk Pemrosesan Lebih Lanjut)." },
  { "en": "Apa Itu LNA (Low Noise Amplifier)?", "id": "Penguat (Tahap Awal Sinyal RF, Noise Sangat Rendah)." },
  { "en": "Apa Itu Antena Log-Periodic?", "id": "Antena (Bandwidth Lebar, Gain Sedang)." },
  { "en": "Apa Itu Polarisasi Sirkular?", "id": "Polarisasi Antena (Medan Berputar (Satelit))." },
  { "en": "Apa Itu Balun (Balanced-Unbalanced)?", "id": "Komponen RF (Menghubungkan Jalur Seimbang Dan Tidak Seimbang)." },
  { "en": "Apa Itu Penganalisa Spektrum?", "id": "Alat (Menampilkan Sinyal Domain Frekuensi)." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Alat (Menganalisis Sinyal Digital Multi-Kanal)." },
  { "en": "Apa Itu Signal Generator?", "id": "Alat (Menghasilkan Sinyal Uji (Sinus, Pulsa, Dll))." },
  { "en": "Apa Itu DMM True RMS?", "id": "Multimeter (Mengukur Nilai RMS Sebenarnya)." },
  { "en": "Apa Itu Time Domain Reflectometer (TDR)?", "id": "Alat (Mencari Kerusakan Kabel (Pantulan Sinyal))." },
  { "en": "Apa Itu Sensor Piezokeramik?", "id": "Sensor (Menghasilkan Tegangan Akibat Tekanan/Gaya)." },
  { "en": "Apa Itu MEMS (Micro-Electro-Mechanical System)?", "id": "Perangkat Mikroskopis (Sensor/Aktuator Mekanis Kecil)." },
  { "en": "Apa Itu Sensor Gyroscope?", "id": "Sensor (Mengukur Kecepatan Rotasi/Putaran)." },
  { "en": "Apa Itu Magnetometer?", "id": "Sensor (Mengukur Kekuatan Dan Arah Medan Magnet)." },
  { "en": "Apa Itu Sensor Barometrik?", "id": "Sensor (Mengukur Tekanan Udara (Ketinggian))." },
  { "en": "Apa Itu Sensor LVDT?", "id": "Sensor (Mengukur Perpindahan Linear)." },
  { "en": "Apa Itu Encoder Absolut?", "id": "Encoder (Memberi Kode Posisi Mutlak)." },
  { "en": "Apa Itu Encoder Inkremental?", "id": "Encoder (Memberi Pulsa (Perubahan Posisi))." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator (Gerak Linear Elektromagnet)." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor (Gerak Langsung Tanpa Rotasi)." },
  { "en": "Apa Itu Back EMF (Counter EMF)?", "id": "Tegangan Balik (Dihasilkan Motor Saat Berputar)." },
  { "en": "Apa Itu Komutator (Motor DC)?", "id": "Cincin (Mengubah Arah Arus Di Kumparan Rotor)." },
  { "en": "Apa Itu Motor DC Brushless (BLDC)?", "id": "Motor DC (Menggunakan Komutasi Elektronik)." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengontrol Motor AC (Mengatur Frekuensi)." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Industri (Kontrol Proses Otomasi)." },
  { "en": "Apa Itu SCADA?", "id": "Sistem (Mengontrol Dan Memantau Proses Industri Besar)." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka (Interaksi Manusia Dengan Mesin Industri)." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "OS (Respon Tugas Terjamin Batas Waktu)." },
  { "en": "Apa Itu Mutex?", "id": "Pengunci (Mencegah Akses Bersamaan Ke Sumber Daya)." },
  { "en": "Apa Itu Semaphore?", "id": "Mekanisme Sinkronisasi (Mengontrol Akses)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka (Debugging Dan Pemrograman IC)." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka (Debug Dengan Dua Pin Data Clock)." },
  { "en": "Apa Itu Protokol CAN?", "id": "Bus Komunikasi (Mobil, Otomasi Industri)." },
  { "en": "Apa Itu Protokol RS-485?", "id": "Standar Komunikasi Serial (Jarak Jauh, Multi-Drop)." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Komunikasi (Dua Kawat Berpasangan, Meredam Noise)." },
  { "en": "Apa Itu Fiber Optic?", "id": "Kabel Komunikasi (Sinyal Cahaya, Bandwidth Sangat Tinggi)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi Nirkabel (Jarak Sangat Dekat)." },
  { "en": "Apa Itu RFID?", "id": "Teknologi (Menggunakan Gelombang Radio (Tag, Reader))." },
  { "en": "Apa Itu Modulasi FSK?", "id": "Modulasi Digital (Beda Frekuensi)." },
  { "en": "Apa Itu Modulasi PSK?", "id": "Modulasi Digital (Beda Fasa)." },
  { "en": "Apa Itu Modem?", "id": "Perangkat (Mengubah Sinyal Digital Ke Analog (Transmisi))." },
  { "en": "Apa Itu Multiplexing?", "id": "Teknik (Menggabungkan Banyak Sinyal Ke Satu Saluran)." },
  { "en": "Apa Itu Filter Keramik?", "id": "Filter RF (Menggunakan Bahan Keramik Resonansi)." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter RF (Berdasarkan Gelombang Akustik Permukaan)." },
  { "en": "Apa Itu Dielectric Constant?", "id": "Ukuran (Kemampuan Bahan Menyimpan Energi Listrik)." },
  { "en": "Apa Itu ESD Shielding?", "id": "Perisai (Melindungi Rangkaian Dari Pelepasan Listrik Statis)." },
  { "en": "Apa Itu EMI Shielding?", "id": "Perisai Logam (Melindungi Rangkaian Dari Noise Elektromagnetik)." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif (Meredam Noise Frekuensi Tinggi)." },
  { "en": "Apa Itu Kapasitor Seri Ekivalen (ESR)?", "id": "Resistansi (Terdapat Secara Internal Di Kapasitor)." },
  { "en": "Apa Itu Induktansi Seri Ekivalen (ESL)?", "id": "Induktansi (Terdapat Secara Internal Di Kapasitor)." },
  { "en": "Apa Itu Faktor Disipasi Kapasitor?", "id": "Ukuran (Efisiensi Kapasitor (Rasio ESR Terhadap Reaktansi))." },
  { "en": "Apa Itu Kerugian Dielektrik?", "id": "Energi (Hilang Sebagai Panas Dalam Bahan Isolasi Kapasitor)." },
  { "en": "Apa Itu Resonansi Diri Induktor?", "id": "Frekuensi (Induktor Menjadi Kapasitor Akibat Kapasitansi Parasitik)." },
  { "en": "Apa Itu Inti Ferrite?", "id": "Bahan Keramik (Digunakan Sebagai Inti Induktor/Trafo Frekuensi Tinggi)." },
  { "en": "Apa Itu Resistor Jaringan (Resistor Network)?", "id": "Beberapa Resistor (Dalam Satu Kemasan IC)." },
  { "en": "Apa Itu Dioda Bidirectional?", "id": "Dioda (Dapat Menghantarkan Arus Dalam Dua Arah (DIAC))." },
  { "en": "Apa Itu Current Mirror?", "id": "Rangkaian Transistor (Menyalin Arus Dari Satu Cabang Ke Cabang Lain)." },
  { "en": "Apa Itu Active Load (Beban Aktif)?", "id": "Pengganti Resistor (Dengan Transistor Untuk Gain Lebih Tinggi)." },
  { "en": "Apa Itu IC Supervisory?", "id": "IC (Memantau Tegangan/Clock, Menghasilkan Sinyal Reset)." },
  { "en": "Apa Itu Latch-Up (CMOS)?", "id": "Kondisi (IC CMOS Menarik Arus Berlebihan Secara Destruktif)." },
  { "en": "Apa Itu Proteksi Reverse Polarity?", "id": "Rangkaian (Melindungi Dari Pemasangan Catu Daya Terbalik)." },
  { "en": "Apa Itu Proteksi Arus Berlebih (Overcurrent Protection)?", "id": "Rangkaian (Membatasi/Memutus Arus Jika Melebihi Batas)." },
  { "en": "Apa Itu Penyangga (Buffer)?", "id": "Penguat (Gain = 1, Isolasi Impedansi)." },
  { "en": "Apa Itu Comparator (Komparator)?", "id": "IC (Membandingkan Dua Tegangan, Output Digital)." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator (Dengan Histeresis (Anti Noise))." },
  { "en": "Apa Itu Penguat Transimpedansi?", "id": "Op-Amp (Mengubah Arus Input Menjadi Tegangan Output)." },
  { "en": "Apa Itu Slew Rate?", "id": "Kecepatan Maksimal Perubahan Output (Op-Amp/IC)." },
  { "en": "Apa Itu Jitter (Sinyal)?", "id": "Variasi Waktu (Ketidakstabilan Pulsa Clock)." },
  { "en": "Apa Itu Phase Noise?", "id": "Noise (Berhubungan Dengan Ketidakstabilan Fasa Osilator)." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator (Frekuensi Sangat Stabil)." },
  { "en": "Apa Itu Osilator Keramik?", "id": "Osilator (Lebih Murah Dari Kristal, Akurasi Kurang)." },
  { "en": "Apa Itu Osilator Relaksasi?", "id": "Osilator (Menggunakan Pengisian/Pengosongan Kapasitor)." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Penguat (Kompromi Antara Kelas A Dan B)." },
  { "en": "Apa Itu Crossover Distortion?", "id": "Distorsi (Terjadi Saat Transistor Kelas B Berpindah)." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Upaya Membuat PF Mendekati Satu." },
  { "en": "Apa Itu THD (Total Harmonic Distortion)?", "id": "Ukuran (Distorsi Harmonik Total Sinyal)." },
  { "en": "Apa Itu Zero Crossing Detector?", "id": "Rangkaian (Mendeteksi Saat Sinyal Melewati Nol Volt)." },
  { "en": "Apa Itu Nyquist Rate?", "id": "Laju Sampling (Minimum Dua Kali Frekuensi Tertinggi Sinyal)." },
  { "en": "Apa Itu Aliasing (Sinyal)?", "id": "Fenomena (Frekuensi Tinggi Terbaca Sebagai Frekuensi Rendah)." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter LPF (Ditempatkan Sebelum ADC Untuk Mencegah Aliasing)." },
  { "en": "Apa Itu ADC Successive Approximation Register (SAR)?", "id": "ADC (Kecepatan Sedang, Akurasi Baik)." },
  { "en": "Apa Itu ADC Flash?", "id": "ADC (Sangat Cepat, Resolusi Rendah)." },
  { "en": "Apa Itu DAC R-2R Ladder?", "id": "DAC (Menggunakan Jaringan Resistor R Dan 2R)." },
  { "en": "Apa Itu Monotonicity (DAC)?", "id": "Sifat DAC (Output Selalu Naik Saat Input Naik)." },
  { "en": "Apa Itu MUX (Multiplexer)?", "id": "Perangkat Digital (Memilih Satu Dari Banyak Input)." },
  { "en": "Apa Itu DEMUX (Demultiplexer)?", "id": "Perangkat Digital (Mengarahkan Satu Input Ke Banyak Output)." },
  { "en": "Apa Itu Decoder?", "id": "Rangkaian Digital (Mengubah Kode Ke Output Unik)." },
  { "en": "Apa Itu Encoder?", "id": "Rangkaian Digital (Mengubah Input Unik Ke Kode)." },
  { "en": "Apa Itu Bus Contention?", "id": "Konflik (Dua Perangkat Berusaha Mengendalikan Bus Bersamaan)." },
  { "en": "Apa Itu Parity Bit?", "id": "Bit Tambahan (Mendeteksi Kesalahan Transmisi Data)." },
  { "en": "Apa Itu Hamming Code?", "id": "Kode (Mendeteksi Dan Memperbaiki Kesalahan Data)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer (Mengatur Ulang MCU Jika Program Macet)." },
  { "en": "Apa Itu Brown-Out Detection (BOD)?", "id": "Fitur MCU (Mereset Jika Tegangan Turun Drastis)." },
  { "en": "Apa Itu Interupsi (Interrupt)?", "id": "Sinyal (Menghentikan Program Utama Untuk Menjalankan Tugas Mendesak)." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Fitur MCU (Transfer Data Tanpa Melibatkan CPU)." },
  { "en": "Apa Itu Periphery (MCU)?", "id": "Modul Internal (Timer, ADC, UART, Dll)." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak (Diprogram Ke Dalam Memori MCU)." },
  { "en": "Apa Itu JTAG?", "id": "Antarmuka (Debugging Dan Pemrograman IC)." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka (Debug Dengan Dua Pin Data Clock)." },
  { "en": "Apa Itu Protokol SPI?", "id": "Bus Serial (Empat Kabel, Cepat (Master-Slave))." },
  { "en": "Apa Itu Protokol I2C?", "id": "Bus Serial (Dua Kabel, Multi-Perangkat (Master-Slave/Multi-Master))." },
  { "en": "Apa Itu Protokol UART?", "id": "Komunikasi Serial (TX, RX (Asinkron))." },
  { "en": "Apa Itu Protokol CAN?", "id": "Bus Komunikasi (Mobil, Otomasi Industri)." },
  { "en": "Apa Itu Protokol RS-485?", "id": "Standar Komunikasi Serial (Jarak Jauh, Multi-Drop)." },
  { "en": "Apa Itu Modbus?", "id": "Protokol Komunikasi (Otomasi Industri)." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Komunikasi (Dua Kawat Berpasangan, Meredam Noise)." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Hambatan Khas Kabel Transmisi (50/75 Ohm)." },
  { "en": "Apa Itu Return Loss?", "id": "Ukuran (Daya Yang Dipantulkan Kembali Oleh Beban)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Antena Kabel)." },
  { "en": "Apa Itu Balun?", "id": "Komponen RF (Menghubungkan Jalur Seimbang Dan Tidak Seimbang)." },
  { "en": "Apa Itu Antena Dipole Setengah Gelombang?", "id": "Antena (Panjang Sekitar Setengah Panjang Gelombang)." },
  { "en": "Apa Itu Antena Patch?", "id": "Antena Datar (Untuk Frekuensi Microwave)." },
  { "en": "Apa Itu Gain Antena?", "id": "Ukuran (Efisiensi Antena Dalam Mengarahkan Daya)." },
  { "en": "Apa Itu Polarisasi Antena?", "id": "Arah (Medan Listrik Gelombang Radio)." },
  { "en": "Apa Itu Ground Plane?", "id": "Lapisan Tembaga Luas Untuk Ground PCB." },
  { "en": "Apa Itu Via (PCB)?", "id": "Lubang Tembaga (Penghubung Antar Lapisan)." },
  { "en": "Apa Itu Annular Ring?", "id": "Cincin Tembaga (Mengelilingi Lubang Komponen THT Di PCB)." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Hijau PCB." },
  { "en": "Apa Itu Silkscreen?", "id": "Teks Putih Penanda Komponen Di PCB." },
  { "en": "Apa Itu Component Footprint?", "id": "Pola Tembaga (Tempat Komponen Akan Disolder Di PCB)." },
  { "en": "Apa Itu Bill of Materials (BOM)?", "id": "Daftar (Semua Komponen Di Rangkaian)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD (Pemanasan Oven)." },
  { "en": "Apa Itu Wave Soldering?", "id": "Metode Solder THT (Komponen Dilewatkan Di Atas Gelombang Timah Cair)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Noise (Mengganggu Fungsi Rangkaian)." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif (Meredam Noise Frekuensi Tinggi)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur (Volt, Ampere, Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Visual Bentuk Sinyal." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Alat (Menganalisis Sinyal Digital Multi-Kanal)." },
  { "en": "Apa Itu Penganalisa Spektrum?", "id": "Alat (Menampilkan Sinyal Domain Frekuensi)." },
  { "en": "Apa Itu Signal Generator?", "id": "Alat (Menghasilkan Sinyal Uji (Sinus, Pulsa, Dll))." },
  { "en": "Apa Itu Sensor Piezokeramik?", "id": "Sensor (Menghasilkan Tegangan Akibat Tekanan/Gaya)." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor (Mengukur Regangan (Perubahan Resistansi))." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat (Strain Gauge)." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu (Sambungan Dua Logam Beda)." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor (Mengukur Medan Magnet)." },
  { "en": "Apa Itu Sensor Barometrik?", "id": "Sensor (Mengukur Tekanan Udara (Ketinggian))." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor (Gerak Langsung Tanpa Rotasi)." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor (Gerak Per Langkah Presisi)." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor (Posisi Terkontrol Umpan Balik)." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengontrol Motor AC (Mengatur Frekuensi)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Motor DC." },
  { "en": "Apa Itu Relay Overload Termal?", "id": "Pengaman (Motor Dari Arus Berlebih, Panas)." },
  { "en": "Apa Itu ELCB (Earth Leakage Circuit Breaker)?", "id": "Pengaman (Mendeteksi Arus Bocor Ke Tanah)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Sistem Catu Daya (Menyediakan Daya Darurat)." },
  { "en": "Apa Itu Baterai LiFePO4?", "id": "Baterai Isi Ulang (Besi Fosfat Lithium, Lebih Aman)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (Melindungi Mengelola Baterai Lithium)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Komponen Saklar Daya Searah." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Komponen Saklar Daya Bolak-Balik." },
  { "en": "Apa Itu DIAC?", "id": "Komponen Pemicu TRIAC (Dua Arah AC)." }



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
