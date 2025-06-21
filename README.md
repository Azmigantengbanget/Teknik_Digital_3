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


  { "en": "Elemen memori tanpa clock?", "id": "Latch." },
  { "en": "Perbedaan Latch dan Flip-flop?", "id": "Latch tidak butuh clock." },
  { "en": "Pencacah menghitung naik?", "id": "Up Counter." },
  { "en": "Pencacah menghitung turun?", "id": "Down Counter." },
  { "en": "Register geser dengan output terhubung input?", "id": "Ring Counter." },
  { "en": "Varian dari ring counter?", "id": "Johnson Counter." },
  { "en": "Diagram representasi mesin keadaan?", "id": "State Diagram." },
  { "en": "Mesin keadaan (output tergantung state)?", "id": "Moore Machine." },
  { "en": "Mesin keadaan (output tergantung state & input)?", "id": "Mealy Machine." },
  { "en": "Memori yang bisa dibaca dan ditulis?", "id": "RAM (Random Access Memory)." },
  { "en": "Memori yang hanya bisa dibaca?", "id": "ROM (Read-Only Memory)." },
  { "en": "RAM yang datanya bisa hilang?", "id": "Volatile Memory." },
  { "en": "RAM yang datanya tetap ada?", "id": "Non-volatile Memory." },
  { "en": "Tipe RAM sangat cepat?", "id": "SRAM (Static RAM)." },
  { "en": "SRAM menyimpan data dalam?", "id": "Flip-flop." },
  { "en": "Tipe RAM umum pada komputer?", "id": "DRAM (Dynamic RAM)." },
  { "en": "DRAM menyimpan data dalam?", "id": "Kapasitor." },
  { "en": "Proses menjaga data pada DRAM?", "id": "Refreshing." },
  { "en": "ROM yang bisa diprogram sekali?", "id": "PROM." },
  { "en": "ROM yang bisa dihapus sinar UV?", "id": "EPROM." },
  { "en": "ROM yang bisa dihapus secara elektrik?", "id": "EEPROM." },
  { "en": "Tipe EEPROM modern pada USB drive?", "id": "Flash Memory." },
  { "en": "Perangkat logika yang bisa diprogram?", "id": "PLD (Programmable Logic Device)." },
  { "en": "Contoh PLD sederhana?", "id": "PAL, GAL." },
  { "en": "PLD yang lebih kompleks?", "id": "CPLD." },
  { "en": "PLD paling fleksibel dan kompleks?", "id": "FPGA." },
  { "en": "Blok dasar pada FPGA?", "id": "LUT (Look-Up Table)." },
  { "en": "Bahasa deskripsi perangkat keras?", "id": "HDL (Hardware Description Language)." },
  { "en": "Contoh bahasa HDL?", "id": "VHDL, Verilog." },
  { "en": "Pengubah sinyal analog ke digital?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Pengubah sinyal digital ke analog?", "id": "DAC (Digital-to-Analog Converter)." },
  { "en": "Proses mengambil sampel sinyal analog?", "id": "Sampling." },
  { "en": "Proses memetakan sampel ke level digital?", "id": "Quantization." },
  { "en": "Kesalahan akibat kuantisasi?", "id": "Quantization Error." },
  { "en": "Jumlah bit output ADC?", "id": "Resolution (Resolusi)." },
  { "en": "Kecepatan ADC mengambil sampel?", "id": "Sampling Rate." },
  { "en": "Teorema laju sampling minimum?", "id": "Teorema Nyquist." },
  { "en": "Jenis DAC paling umum?", "id": "R-2R Ladder DAC." },
  { "en": "Waktu tunda propagasi sinyal?", "id": "Propagation Delay." },
  { "en": "Jumlah input yang bisa dihubungkan ke output?", "id": "Fan-out." },
  { "en": "Jumlah input pada sebuah gerbang?", "id": "Fan-in." },
  { "en": "Kekebalan sirkuit terhadap derau?", "id": "Noise Margin." },
  { "en": "Daya yang digunakan oleh gerbang?", "id": "Power Dissipation." },
  { "en": "Logika tiga keadaan?", "id": "Tri-state logic." },
  { "en": "Tiga keadaan pada tri-state logic?", "id": "HIGH, LOW, High-Impedance." },
  { "en": "Resistor untuk memastikan level HIGH?", "id": "Pull-up resistor." },
  { "en": "Resistor untuk memastikan level LOW?", "id": "Pull-down resistor." },
  { "en": "Rangkaian pemicu dengan histeresis?", "id": "Schmitt Trigger." },
  { "en": "Tujuan Schmitt Trigger?", "id": "Membersihkan sinyal derau." },
  { "en": "Rangkaian penjumlah banyak bit paralel?", "id": "Parallel Adder." },
  { "en": "Representasi bilangan negatif (komplemen 1)?", "id": "1's Complement." },
  { "en": "Representasi bilangan negatif (komplemen 2)?", "id": "2's Complement." },
  { "en": "Cara mendapatkan 1's complement?", "id": "Balik semua bit." },
  { "en": "Cara mendapatkan 2's complement?", "id": "1's complement ditambah 1." },
  { "en": "Operasi pengurangan menggunakan?", "id": "Penjumlahan dengan 2's complement." },
  { "en": "Unit yang melakukan operasi aritmatika & logika?", "id": "ALU." },
  { "en": "Kepanjangan ALU?", "id": "Arithmetic Logic Unit." },
  { "en": "Rangkaian pendeteksi urutan bit?", "id": "Sequence Detector." },
  { "en": "Peta Karnaugh digunakan untuk?", "id": "Menyederhanakan ekspresi Boolean." },
  { "en": "Pengelompokan 1 pada Peta Karnaugh?", "id": "Grouping." },
  { "en": "Kondisi 'tidak peduli' pada K-Map?", "id": "Don't care condition." },
  { "en": "Sinyal clock dihasilkan oleh?", "id": "Osilator." },
  { "en": "Osilator menggunakan kristal?", "id": "Crystal oscillator." },
  { "en": "Siklus kerja sinyal clock?", "id": "Duty Cycle." },
  { "en": "Duty cycle 50% berarti?", "id": "Waktu HIGH dan LOW sama." },
  { "en": "Variasi waktu pada sinyal clock?", "id": "Jitter." },
  { "en": "Perbedaan waktu tiba clock?", "id": "Clock skew." },
  { "en": "Rangkaian pembagi frekuensi clock?", "id": "Frequency divider." },
  { "en": "T Flip-flop dapat berfungsi sebagai?", "id": "Pembagi frekuensi 2." },
  { "en": "Sinyal yang mengaktifkan/menonaktifkan sirkuit?", "id": "Enable signal." },
  { "en": "Rangkaian untuk menghasilkan pulsa tunggal?", "id": "Monostable multivibrator." },
  { "en": "Rangkaian osilator sederhana?", "id": "Astable multivibrator." },
  { "en": "Rangkaian dengan dua keadaan stabil?", "id": "Bistable multivibrator (Flip-flop)." },
  { "en": "Masalah pada asynchronous counter?", "id": "Propagation delay (ripple effect)." },
  { "en": "Pencacah hingga 10 (0-9)?", "id": "Decade Counter." },
  { "en": "Pencacah hingga 16 (0-F)?", "id": "Hexadecimal Counter / Mod-16." },
  { "en": "Memori cache pada CPU?", "id": "SRAM." },
  { "en": "Memori utama pada komputer?", "id": "DRAM." },
  { "en": "BIOS disimpan di?", "id": "ROM atau Flash Memory." },
  { "en": "Memori asosiatif?", "id": "Content-Addressable Memory (CAM)." },
  { "en": "Memori yang diakses secara serial?", "id": "Shift register." },
  { "en": "Sistem pada sebuah chip?", "id": "SoC (System on a Chip)." },
  { "en": "Bahaya balapan sinyal?", "id": "Race condition." },
  { "en": "Pulsa pendek tak diinginkan?", "id": "Glitch." },
  { "en": "Kode pendeteksi kesalahan?", "id": "Error detection code." },
  { "en": "Contoh kode pendeteksi kesalahan?", "id": "Parity bit." },
  { "en": "Bit paritas untuk jumlah 1 ganjil?", "id": "Odd parity." },
  { "en": "Bit paritas untuk jumlah 1 genap?", "id": "Even parity." },
  { "en": "Kode pengoreksi kesalahan?", "id": "Error correction code (ECC)." },
  { "en": "Contoh kode pengoreksi kesalahan?", "id": "Hamming code." },
  { "en": "Kode biner reflektif?", "id": "Gray code." },
  { "en": "Keuntungan Gray code?", "id": "Hanya satu bit berubah." },
  { "en": "Representasi angka dengan tanda?", "id": "Signed number representation." },
  { "en": "Kondisi luapan pada penjumlahan biner?", "id": "Overflow." },
  { "en": "Antarmuka bus paralel?", "id": "Parallel interface." },
  { "en": "Antarmuka bus serial?", "id": "Serial interface." },
  { "en": "Contoh antarmuka serial?", "id": "UART, SPI, I2C." },
  { "en": "Proses sinkronisasi data serial?", "id": "Start bit dan Stop bit." },
  { "en": "Tabel untuk desain state machine?", "id": "Excitation Table." },
  { "en": "Tujuan reduksi state?", "id": "Menyederhanakan rangkaian." },
  { "en": "Mesin keadaan yang clock-nya tidak terpusat?", "id": "Asynchronous State Machine." },
  { "en": "Otak dari sebuah sistem komputer?", "id": "Microprocessor (MPU)." },
  { "en": "Sistem komputer dalam satu chip?", "id": "Microcontroller (MCU)." },
  { "en": "Perbedaan utama MPU dan MCU?", "id": "MCU punya RAM/ROM internal." },
  { "en": "Bagian dari CPU?", "id": "ALU, Control Unit, Register." },
  { "en": "Jalur untuk alamat memori?", "id": "Address Bus." },
  { "en": "Jalur untuk transfer data?", "id": "Data Bus." },
  { "en": "Jalur untuk sinyal kontrol?", "id": "Control Bus." },
  { "en": "Siklus instruksi dasar CPU?", "id": "Fetch, Decode, Execute." },
  { "en": "Tahap mengambil instruksi dari memori?", "id": "Fetch." },
  { "en": "Tahap menerjemahkan instruksi?", "id": "Decode." },
  { "en": "Tahap menjalankan instruksi?", "id": "Execute." },
  { "en": "Sinyal interupsi dari perangkat keras?", "id": "Interrupt." },
  { "en": "Program yang melayani interupsi?", "id": "ISR (Interrupt Service Routine)." },
  { "en": "I/O berbagi alamat dengan memori?", "id": "Memory-mapped I/O." },
  { "en": "I/O punya alamat terpisah?", "id": "Port-mapped I/O." },
  { "en": "Komunikasi serial tanpa clock?", "id": "Asynchronous (misal: UART)." },
  { "en": "Komunikasi serial dengan clock?", "id": "Synchronous (misal: SPI, I2C)." },
  { "en": "Kepanjangan UART?", "id": "Universal Asynchronous Receiver-Transmitter." },
  { "en": "Bit penanda awal data UART?", "id": "Start bit." },
  { "en": "Bit penanda akhir data UART?", "id": "Stop bit." },
  { "en": "Kecepatan transmisi UART?", "id": "Baud rate." },
  { "en": "Kepanjangan SPI?", "id": "Serial Peripheral Interface." },
  { "en": "Perangkat pengendali bus SPI?", "id": "Master." },
  { "en": "Perangkat yang dikendalikan di bus SPI?", "id": "Slave." },
  { "en": "Jalur data dari Master ke Slave?", "id": "MOSI." },
  { "en": "Jalur data dari Slave ke Master?", "id": "MISO." },
  { "en": "Jalur clock pada SPI?", "id": "SCLK." },
  { "en": "Jalur pemilih slave pada SPI?", "id": "SS (Slave Select)." },
  { "en": "Kepanjangan I2C?", "id": "Inter-Integrated Circuit." },
  { "en": "Jumlah kabel pada bus I2C?", "id": "Dua (SDA dan SCL)." },
  { "en": "Jalur data pada I2C?", "id": "SDA (Serial Data)." },
  { "en": "Jalur clock pada I2C?", "id": "SCL (Serial Clock)." },
  { "en": "Sinyal dengan dua kabel (positif/negatif)?", "id": "Differential signaling." },
  { "en": "Keuntungan differential signaling?", "id": "Tahan terhadap derau." },
  { "en": "Contoh differential signaling?", "id": "LVDS, RS-485, USB." },
  { "en": "Waktu data harus stabil sebelum clock?", "id": "Setup time." },
  { "en": "Waktu data harus stabil setelah clock?", "id": "Hold time." },
  { "en": "Pelanggaran setup atau hold time menyebabkan?", "id": "Metastability." },
  { "en": "Keadaan flip-flop yang tidak menentu?", "id": "Metastable state." },
  { "en": "Masalah sinyal antar domain clock beda?", "id": "Clock Domain Crossing (CDC)." },
  { "en": "Solusi untuk CDC?", "id": "Synchronizer (penyinkron)." },
  { "en": "Teknik eksekusi instruksi tumpang tindih?", "id": "Pipelining." },
  { "en": "Tujuan pipelining?", "id": "Meningkatkan throughput instruksi." },
  { "en": "Bahaya dalam pipeline?", "id": "Pipeline hazard." },
  { "en": "Keluarga logika sangat cepat boros daya?", "id": "ECL (Emitter-Coupled Logic)." },
  { "en": "Output yang bisa dihubungkan bersama?", "id": "Open-collector / Open-drain." },
  { "en": "Logika yang terbentuk dari output open-collector?", "id": "Wired-AND." },
  { "en": "Rangkaian pengubah level tegangan logika?", "id": "Logic Level Shifter." },
  { "en": "Alat ukur sinyal digital multi-kanal?", "id": "Logic Analyzer." },
  { "en": "Fungsi utama logic analyzer?", "id": "Analisis waktu (timing analysis)." },
  { "en": "Alat melihat bentuk sinyal vs waktu?", "id": "Oscilloscope." },
  { "en": "Standar untuk pengujian papan sirkuit?", "id": "JTAG." },
  { "en": "Kepanjangan JTAG?", "id": "Joint Test Action Group." },
  { "en": "Mekanisme pengujian sirkuit internal?", "id": "BIST (Built-In Self-Test)." },
  { "en": "Memori buffer 'pertama masuk, pertama keluar'?", "id": "FIFO (First-In, First-Out)." },
  { "en": "Memori buffer 'terakhir masuk, pertama keluar'?", "id": "LIFO (Last-In, First-Out)." },
  { "en": "Nama lain LIFO?", "id": "Stack." },
  { "en": "Operasi memasukkan data ke stack?", "id": "Push." },
  { "en": "Operasi mengambil data dari stack?", "id": "Pop." },
  { "en": "Kondisi FIFO saat kosong dan dibaca?", "id": "Underflow." },
  { "en": "Kondisi FIFO saat penuh dan ditulis?", "id": "Overflow." },
  { "en": "Penunjuk lokasi tulis di FIFO?", "id": "Write pointer." },
  { "en": "Penunjuk lokasi baca di FIFO?", "id": "Read pointer." },
  { "en": "Arsitektur CPU set instruksi sedikit?", "id": "RISC (Reduced Instruction Set Computer)." },
  { "en": "Arsitektur CPU set instruksi banyak?", "id": "CISC (Complex Instruction Set Computer)." },
  { "en": "Contoh arsitektur RISC?", "id": "ARM, MIPS." },
  { "en": "Contoh arsitektur CISC?", "id": "x86 (Intel, AMD)." },
  { "en": "Memori cache tercepat di CPU?", "id": "L1 Cache." },
  { "en": "Unit manajemen memori?", "id": "MMU (Memory Management Unit)." },
  { "en": "Fungsi MMU?", "id": "Menerjemahkan alamat virtual ke fisik." },
  { "en": "Operasi atomik dalam digital?", "id": "Operasi tak bisa diinterupsi." },
  { "en": "Pengunci untuk akses sumber daya?", "id": "Semaphore atau Mutex." },
  { "en": "Kondisi dua proses saling menunggu?", "id": "Deadlock." },
  { "en": "Pengkodean karakter 7-bit?", "id": "ASCII." },
  { "en": "Pengkodean karakter universal?", "id": "Unicode (misal: UTF-8)." },
  { "en": "Rangkaian yang menghasilkan sinyal clock?", "id": "Clock generator." },
  { "en": "Distribusi sinyal clock ke seluruh chip?", "id": "Clock tree." },
  { "en": "Penambahan bit untuk menjaga transisi?", "id": "Bit stuffing." },
  { "en": "Bilangan pecahan dalam format biner?", "id": "Fixed-point atau Floating-point." },
  { "en": "Standar untuk format floating-point?", "id": "IEEE 754." },
  { "en": "Bagian dari angka floating-point?", "id": "Sign, Exponent, Mantissa." },
  { "en": "Prosesor khusus pengolahan sinyal?", "id": "DSP (Digital Signal Processor)." },
  { "en": "Operasi fundamental pada DSP?", "id": "MAC (Multiply-Accumulate)." },
  { "en": "Perangkat lunak untuk simulasi logika?", "id": "Logic simulator." },
  { "en": "Verifikasi fungsionalitas desain hardware?", "id": "Functional verification." },
  { "en": "Desain chip dari awal sampai akhir?", "id": "ASIC design flow." },
  { "en": "Kepanjangan ASIC?", "id": "Application-Specific Integrated Circuit." },
  { "en": "Perbedaan FPGA dan ASIC?", "id": "FPGA bisa diprogram ulang." },
  { "en": "Bahasa level transfer register?", "id": "RTL (Register-Transfer Level)." },
  { "en": "HDL mendeskripsikan apa?", "id": "Struktur dan perilaku sirkuit." },
  { "en": "Proses sintesis HDL?", "id": "Mengubah kode HDL jadi gerbang." },
  { "en": "Penempatan dan perutean pada FPGA?", "id": "Place and Route." },
  { "en": "File konfigurasi untuk FPGA?", "id": "Bitstream." },
  { "en": "Data ditemukan di cache?", "id": "Cache hit." },
  { "en": "Data tidak ditemukan di cache?", "id": "Cache miss." },
  { "en": "Masalah konsistensi data antar cache?", "id": "Cache coherence problem." },
  { "en": "Arsitektur CPU eksekusi banyak instruksi?", "id": "Superscalar." },
  { "en": "Teknik CPU menebak hasil percabangan?", "id": "Branch prediction." },
  { "en": "Eksekusi instruksi sebelum waktunya?", "id": "Speculative execution." },
  { "en": "Satu instruksi, banyak data?", "id": "SIMD (Single Instruction, Multiple Data)." },
  { "en": "Banyak instruksi, banyak data?", "id": "MIMD (Multiple Instruction, Multiple Data)." },
  { "en": "Hirarki kecepatan memori?", "id": "Register, Cache, RAM, Disk." },
  { "en": "Kepanjangan DDR pada RAM?", "id": "Double Data Rate." },
  { "en": "Apa arti 'Double Data Rate'?", "id": "Transfer data di dua tepi clock." },
  { "en": "Ukuran jeda latensi RAM?", "id": "CAS Latency (CL)." },
  { "en": "Memori dengan koreksi kesalahan?", "id": "ECC Memory." },
  { "en": "Transfer data tanpa intervensi CPU?", "id": "DMA (Direct Memory Access)." },
  { "en": "Keuntungan menggunakan DMA?", "id": "Mengurangi beban kerja CPU." },
  { "en": "Kualitas sinyal digital?", "id": "Signal Integrity." },
  { "en": "Pantulan sinyal di ujung saluran?", "id": "Reflection." },
  { "en": "Osilasi sinyal setelah transisi?", "id": "Ringing." },
  { "en": "Gangguan sinyal antar jalur berdekatan?", "id": "Crosstalk." },
  { "en": "Penyesuaian impedansi untuk cegah pantulan?", "id": "Impedance matching." },
  { "en": "Resistor di ujung saluran transmisi?", "id": "Termination resistor." },
  { "en": "Diagram untuk visualisasi kualitas sinyal?", "id": "Eye diagram." },
  { "en": "Bukaan mata 'eye diagram' lebar?", "id": "Sinyal berkualitas baik." },
  { "en": "Filter digital dengan respons impuls terbatas?", "id": "FIR (Finite Impulse Response)." },
  { "en": "Filter digital dengan respons impuls tak terbatas?", "id": "IIR (Infinite Impulse Response)." },
  { "en": "Transformasi sinyal domain waktu ke frekuensi?", "id": "Fourier Transform." },
  { "en": "Algoritma cepat untuk Fourier Transform?", "id": "FFT (Fast Fourier Transform)." },
  { "en": "Operasi matematika penggabungan dua sinyal?", "id": "Convolution." },
  { "en": "Blok desain siap pakai untuk SoC?", "id": "IP Core (Intellectual Property Core)." },
  { "en": "IP Core dalam bentuk fisik?", "id": "Hard IP." },
  { "en": "IP Core dalam bentuk kode HDL?", "id": "Soft IP." },
  { "en": "Standar bus internal pada SoC ARM?", "id": "AMBA (AXI, AHB)." },
  { "en": "Teknik hemat daya mematikan clock?", "id": "Clock gating." },
  { "en": "Teknik hemat daya mematikan bagian sirkuit?", "id": "Power gating." },
  { "en": "Prosesor dengan lebih dari satu inti?", "id": "Multi-core processor." },
  { "en": "Analisis waktu statis pada desain?", "id": "STA (Static Timing Analysis)." },
  { "en": "Verifikasi desain menggunakan pembuktian matematis?", "id": "Formal verification." },
  { "en": "Ukuran seberapa banyak kode diuji?", "id": "Code coverage." },
  { "en": "Komputasi terinspirasi dari otak manusia?", "id": "Neuromorphic computing." },
  { "en": "Unit dasar komputasi kuantum?", "id": "Qubit." },
  { "en": "Prinsip superposisi pada qubit?", "id": "Bisa bernilai 0 dan 1 sekaligus." },
  { "en": "Sirkuit logika tanpa sinyal clock terpusat?", "id": "Asynchronous circuit." },
  { "en": "Gerbang logika yang bisa dibalikkan?", "id": "Reversible logic gate." },
  { "en": "Jenis memori berdasarkan lokasi fisik?", "id": "On-chip / Off-chip memory." },
  { "en": "Waktu akses memori?", "id": "Memory access time." },
  { "en": "Siklus waktu memori?", "id": "Memory cycle time." },
  { "en": "Bandwidth memori adalah?", "id": "Laju transfer data memori." },
  { "en": "Memori buffer antara CPU dan RAM?", "id": "CPU Cache." },
  { "en": "Level cache yang paling dekat CPU?", "id": "L1 Cache." },
  { "en": "Cache untuk instruksi dan data terpisah?", "id": "Harvard cache architecture." },
  { "en": "Penundaan karena data tidak di cache?", "id": "Miss penalty." },
  { "en": "Kebijakan penulisan cache (write-through)?", "id": "Tulis ke cache dan memori." },
  { "en": "Kebijakan penulisan cache (write-back)?", "id": "Tulis ke cache, nanti ke memori." },
  { "en": "Metode pemetaan cache (direct-mapped)?", "id": "Setiap blok memori ke satu baris." },
  { "en": "Metode pemetaan cache (fully associative)?", "id": "Blok memori ke baris mana saja." },
  { "en": "Set instruksi (ISA) adalah?", "id": "Antarmuka antara software dan hardware." },
  { "en": "Pemrosesan paralel di level instruksi?", "id": "ILP (Instruction-Level Parallelism)." },
  { "en": "Unit fungsional dalam CPU?", "id": "Execution unit." },
  { "en": "Operasi mikro dalam CPU?", "id": "Micro-operations." },
  { "en": "Pengurutan micro-operations?", "id": "Control unit." },
  { "en": "Desain control unit (hardwired)?", "id": "Menggunakan gerbang logika." },
  { "en": "Desain control unit (microprogrammed)?", "id": "Menggunakan memori (control store)." },
  { "en": "Firmware pada sebuah sistem?", "id": "Software permanen di hardware." },
  { "en": "Proses booting sistem?", "id": "Memuat OS saat startup." },
  { "en": "Program pertama yang berjalan saat boot?", "id": "Bootstrap loader." },
  { "en": "Tes mandiri saat startup?", "id": "POST (Power-On Self-Test)." },
  { "en": "Pengujian memori saat POST?", "id": "Memory test." },
  { "en": "Antarmuka firmware modern?", "id": "UEFI." },
  { "en": "Antarmuka firmware lama?", "id": "BIOS." },
  { "en": "Tabel partisi pada UEFI?", "id": "GPT (GUID Partition Table)." },
  { "en": "Tabel partisi pada BIOS?", "id": "MBR (Master Boot Record)." },
  { "en": "Prosesor sinyal untuk gambar?", "id": "Image Signal Processor (ISP)." },
  { "en": "Akselerator perangkat keras?", "id": "Hardware accelerator." },
  { "en": "Contoh hardware accelerator?", "id": "GPU, DSP, FPGA." },
  { "en": "Unit pemrosesan grafis?", "id": "GPU (Graphics Processing Unit)." },
  { "en": "Kelebihan utama GPU?", "id": "Pemrosesan paralel masif." },
  { "en": "Platform komputasi paralel dari NVIDIA?", "id": "CUDA." },
  { "en": "Arus bocor pada transistor?", "id": "Leakage current." },
  { "en": "Pengaruh leakage current?", "id": "Meningkatkan konsumsi daya statis." },
  { "en": "Hukum Moore menyatakan?", "id": "Jumlah transistor dobel setiap 2 tahun." },
  { "en": "Batas fisik pengecilan transistor?", "id": "Efek kuantum." },
  { "en": "Tegangan ambang batas transistor?", "id": "Threshold voltage." },
  { "en": "Proses fabrikasi semikonduktor?", "id": "Photolithography." },
  { "en": "Bahan dasar pembuatan chip?", "id": "Silikon." },
  { "en": "Wafer silikon adalah?", "id": "Potongan tipis silikon." },
  { "en": "Penambahan impuritas ke silikon?", "id": "Doping." },
  { "en": "Semikonduktor tipe-N memiliki kelebihan?", "id": "Elektron." },
  { "en": "Semikonduktor tipe-P memiliki kelebihan?", "id": "Hole." },
  { "en": "Sambungan antara tipe-P dan tipe-N?", "id": "PN Junction (Dioda)." },
  { "en": "Komponen dasar CMOS?", "id": "Transistor NMOS dan PMOS." },
  { "en": "Inverter CMOS terdiri dari?", "id": "Satu NMOS dan satu PMOS." },
  { "en": "Skala integrasi chip (SSI)?", "id": "Kurang dari 10 gerbang." },
  { "en": "Skala integrasi chip (MSI)?", "id": "10 hingga 100 gerbang." },
  { "en": "Skala integrasi chip (LSI)?", "id": "100 hingga 1000 gerbang." },
  { "en": "Skala integrasi chip (VLSI)?", "id": "Lebih dari 1000 gerbang." },
  { "en": "Desain chip modern menggunakan skala?", "id": "VLSI/ULSI." },
  { "en": "Dokumen spesifikasi komponen elektronik?", "id": "Datasheet." },
  { "en": "Informasi penting dalam datasheet?", "id": "Parameter listrik, timing, pinout." },
  { "en": "Sirkuit untuk mereset sistem saat dinyalakan?", "id": "Power-On Reset (POR)." },
  { "en": "Masalah pantulan pada saklar mekanik?", "id": "Bouncing." },
  { "en": "Rangkaian untuk menghilangkan bouncing?", "id": "Debouncer." },
  { "en": "Teknik menyalakan banyak display bergantian?", "id": "Multiplexing display." },
  { "en": "Konflik saat banyak output aktif di bus?", "id": "Bus contention." },
  { "en": "Pencegah bus contention?", "id": "Menggunakan buffer tri-state." },
  { "en": "Penjumlah dengan carry yang lebih cepat?", "id": "Carry Lookahead Adder." },
  { "en": "Keuntungan Carry Lookahead Adder?", "id": "Mengurangi delay propagasi." },
  { "en": "Rangkaian penggeser bit cepat?", "id": "Barrel Shifter." },
  { "en": "Unit pemroses bilangan pecahan?", "id": "FPU (Floating-Point Unit)." },
  { "en": "Penemu aljabar logika?", "id": "George Boole." },
  { "en": "Mikroprosesor komersial pertama?", "id": "Intel 4004." },
  { "en": "Perbedaan firmware dan software?", "id": "Firmware terikat pada hardware." },
  { "en": "Kode HDL untuk verifikasi desain?", "id": "Testbench." },
  { "en": "Representasi desain sebagai koneksi gerbang?", "id": "Netlist." },
  { "en": "Batasan desain dalam sintesis HDL?", "id": "Constraints (waktu, area)." },
  { "en": "Assignment serempak di Verilog?", "id": "Non-blocking (<=)." },
  { "en": "Assignment berurutan di Verilog?", "id": "Blocking (=)." },
  { "en": "Kerusakan konduktor karena aliran arus?", "id": "Electromigration." },
  { "en": "Kondisi 'short circuit' pada CMOS?", "id": "Latch-up." },
  { "en": "Kerusakan IC karena listrik statis?", "id": "ESD (Electrostatic Discharge)." },
  { "en": "Gelang anti listrik statis?", "id": "ESD wrist strap." },
  { "en": "Tipe kemasan IC dengan dua baris pin?", "id": "DIP (Dual In-line Package)." },
  { "en": "Tipe kemasan IC untuk surface mount?", "id": "SOIC, QFP." },
  { "en": "Kemasan IC dengan bola solder di bawah?", "id": "BGA (Ball Grid Array)." },
  { "en": "Rangkaian yang outputnya tergantung history?", "id": "Rangkaian sekuensial." },
  { "en": "Aplikasi utama D Flip-flop?", "id": "Register, buffer data." },
  { "en": "Aplikasi utama T Flip-flop?", "id": "Pembagi frekuensi, counter." },
  { "en": "Aplikasi utama Multiplexer?", "id": "Seleksi data, routing." },
  { "en": "Aplikasi utama Decoder?", "id": "Seleksi memori, driver display." },
  { "en": "Diagram yang menunjukkan timing sinyal?", "id": "Timing diagram." },
  { "en": "Aktivasi sinyal dengan level LOW?", "id": "Active-low." },
  { "en": "Aktivasi sinyal dengan level HIGH?", "id": "Active-high." },
  { "en": "Notasi active-low pada nama sinyal?", "id": "Garis di atas atau n di akhir." },
  { "en": "Sinyal untuk sinkronisasi transfer data?", "id": "Handshake signal." },
  { "en": "Contoh sinyal handshake?", "id": "ACK (Acknowledge)." },
  { "en": "Kode ASCII menggunakan berapa bit?", "id": "7-bit atau 8-bit." },
  { "en": "Jumlah karakter dalam ASCII 7-bit?", "id": "128 karakter." },
  { "en": "Memori akses acak statis?", "id": "SRAM." },
  { "en": "Memori akses acak dinamis?", "id": "DRAM." },
  { "en": "Penyederhanaan persamaan logika disebut?", "id": "Minimization." },
  { "en": "Bentuk kanonik dari ekspresi Boolean?", "id": "Sum-of-Products, Product-of-Sums." },
  { "en": "Istilah 'minterm' digunakan dalam?", "id": "Sum-of-Products (SOP)." },
  { "en": "Istilah 'maxterm' digunakan dalam?", "id": "Product-of-Sums (POS)." },
  { "en": "Gerbang logika dari dioda dan resistor?", "id": "Diode Logic (DL)." },
  { "en": "Gerbang logika dari resistor dan transistor?", "id": "RTL (Resistor-Transistor Logic)." },
  { "en": "Gerbang logika dari dioda dan transistor?", "id": "DTL (Diode-Transistor Logic)." },
  { "en": "Keluarga logika HTL untuk lingkungan?", "id": "Berderau tinggi (High Noise)." },
  { "en": "Sirkuit terintegrasi skala kecil?", "id": "SSI (Small-Scale Integration)." },
  { "en": "Sirkuit terintegrasi skala menengah?", "id": "MSI (Medium-Scale Integration)." },
  { "en": "Sirkuit terintegrasi skala besar?", "id": "LSI (Large-Scale Integration)." },
  { "en": "Sirkuit terintegrasi skala sangat besar?", "id": "VLSI (Very Large-Scale Integration)." },
  { "en": "Rangkaian untuk mengubah laju data?", "id": "Rate converter." },
  { "en": "FIFO digunakan sebagai?", "id": "Buffer, rate converter." },
  { "en": "Konversi paralel ke serial?", "id": "PISO Shift Register." },
  { "en": "Konversi serial ke paralel?", "id": "SIPO Shift Register." },
  { "en": "Alat untuk memprogram PROM/EPROM?", "id": "Device programmer." },
  { "en": "Soket untuk memprogram chip?", "id": "ZIF (Zero Insertion Force) socket." },
  { "en": "Papan sirkuit cetak?", "id": "PCB (Printed Circuit Board)." },
  { "en": "Desain layout PCB?", "id": "PCB Layout." },
  { "en": "Proses menyolder komponen surface-mount?", "id": "Reflow soldering." },
  { "en": "Proses menyolder komponen through-hole?", "id": "Wave soldering." },
  { "en": "Alat bantu debugging di dalam chip?", "id": "In-Circuit Debugger (ICD)." },
  { "en": "Arsitektur prosesor Harvard memisahkan?", "id": "Memori data dan instruksi." },
  { "en": "Arsitektur prosesor Von Neumann?", "id": "Memori data dan instruksi menyatu." },
  { "en": "Kelebihan arsitektur Harvard?", "id": "Akses data/instruksi bersamaan." },
  { "en": "Kelebihan arsitektur Von Neumann?", "id": "Desain lebih sederhana." },
  { "en": "Register penyimpan alamat instruksi berikutnya?", "id": "Program Counter (PC)." },
  { "en": "Register penyimpan instruksi saat ini?", "id": "Instruction Register (IR)." },
  { "en": "Register penyimpan hasil operasi ALU?", "id": "Accumulator." },
  { "en": "Register serbaguna?", "id": "General Purpose Register (GPR)." },
  { "en": "Register penyimpan status CPU?", "id": "Status Register / Flag Register." },
  { "en": "Contoh flag pada status register?", "id": "Zero, Carry, Overflow flag." },
  { "en": "Kumpulan instruksi yang dimengerti CPU?", "id": "Instruction Set." },
  { "en": "Mode pengalamatan (addressing mode)?", "id": "Cara CPU mengakses operan." },
  { "en": "Pengalamatan menggunakan nilai langsung?", "id": "Immediate addressing." },
  { "en": "Pengalamatan menggunakan alamat memori?", "id": "Direct addressing." },
  { "en": "Pengalamatan menggunakan register?", "id": "Register addressing." },
  { "en": "Pengalamatan tak langsung (indirect)?", "id": "Alamat menunjuk ke alamat lain." },
  { "en": "Sistem interkoneksi di dalam chip?", "id": "Network-on-Chip (NoC)." },
  { "en": "Pemrosesan sinyal digital secara real-time?", "id": "Real-time DSP." },
  { "en": "Tanggapan sistem terhadap input impuls?", "id": "Impulse response." },
  { "en": "Sinyal clock dari sumber eksternal?", "id": "External clock." },
  { "en": "Penggunaan Phase-Locked Loop (PLL)?", "id": "Sintesis/pemulihan frekuensi clock." },
  { "en": "Rangkaian untuk menahan nilai tegangan?", "id": "Sample and Hold." },
  { "en": "ADC tercepat?", "id": "Flash ADC." },
  { "en": "Kekurangan Flash ADC?", "id": "Sangat kompleks dan mahal." },
  { "en": "ADC dengan keseimbangan kecepatan/resolusi?", "id": "Successive Approximation ADC." },
  { "en": "ADC resolusi tinggi kecepatan rendah?", "id": "Integrating ADC (Dual-slope)." },
  { "en": "DAC paling sederhana?", "id": "Binary-weighted resistor DAC." },
  { "en": "Kekurangan DAC binary-weighted?", "id": "Butuh resistor presisi tinggi." },
  { "en": "Elektronika daya?", "id": "Aplikasi elektronika daya tinggi." },
  { "en": "Komponen saklar daya?", "id": "MOSFET, IGBT." },
  { "en": "Rangkaian tanpa memori?", "id": "Rangkaian kombinasional." },
  { "en": "Rangkaian dengan elemen memori?", "id": "Rangkaian sekuensial." },
  { "en": "Model abstrak untuk rangkaian sekuensial?", "id": "FSM (Finite State Machine)." },
  { "en": "Gerbang yang bisa membuat semua gerbang lain?", "id": "Gerbang universal (NAND/NOR)." },
  { "en": "Gerbang logika yang tidak mengubah logika?", "id": "Buffer." },
  { "en": "Tujuan utama buffer?", "id": "Menguatkan sinyal/arus." },
  { "en": "Rangkaian aritmatika untuk penjumlahan?", "id": "Adder." },
  { "en": "Rangkaian untuk memilih satu dari banyak data?", "id": "Multiplexer (MUX)." },
  { "en": "Rangkaian untuk mengarahkan data ke satu tujuan?", "id": "Demultiplexer (DEMUX)." },
  { "en": "Elemen dasar penyimpan satu bit data?", "id": "Flip-flop." },
  { "en": "Representasi bilangan negatif dengan bit tanda?", "id": "Signed magnitude." },
  { "en": "Presisi floating-point 32-bit?", "id": "Single precision." },
  { "en": "Presisi floating-point 64-bit?", "id": "Double precision." },
  { "en": "Urutan byte: MSB di alamat terendah?", "id": "Big-endian." },
  { "en": "Urutan byte: LSB di alamat terendah?", "id": "Little-endian." },
  { "en": "Jaringan internet menggunakan urutan byte?", "id": "Big-endian." },
  { "en": "Prosesor Intel menggunakan urutan byte?", "id": "Little-endian." },
  { "en": "Kode karakter dari IBM?", "id": "EBCDIC." },
  { "en": "Tahap pipeline: ambil instruksi?", "id": "Instruction Fetch (IF)." },
  { "en": "Tahap pipeline: terjemahkan instruksi?", "id": "Instruction Decode (ID)." },
  { "en": "Tahap pipeline: eksekusi operasi?", "id": "Execute (EX)." },
  { "en": "Tahap pipeline: akses memori?", "id": "Memory Access (MEM)." },
  { "en": "Tahap pipeline: tulis hasil kembali?", "id": "Write Back (WB)." },
  { "en": "Klasifikasi arsitektur komputer Flynn?", "id": "Flynn's Taxonomy." },
  { "en": "Komputer sekuensial tradisional (Flynn)?", "id": "SISD." },
  { "en": "Prosesor vektor atau GPU (Flynn)?", "id": "SIMD." },
  { "en": "Model kesalahan sirkuit digital?", "id": "Fault model." },
  { "en": "Kesalahan paling umum: 'terjebak di 0'?", "id": "Stuck-at-0 fault." },
  { "en": "Kesalahan paling umum: 'terjebak di 1'?", "id": "Stuck-at-1 fault." },
  { "en": "Desain yang mudah diuji?", "id": "DFT (Design for Testability)." },
  { "en": "Teknik pengujian dengan menghubungkan flip-flop?", "id": "Scan chain." },
  { "en": "Standar bus paralel kecepatan tinggi?", "id": "PCI Express (PCIe)." },
  { "en": "Distribusi sinyal clock dengan delay sama?", "id": "Clock tree (misal: H-tree)." },
  { "en": "Kapasitor untuk menstabilkan tegangan catu daya?", "id": "Decoupling capacitor." },
  { "en": "Lokasi decoupling capacitor?", "id": "Sangat dekat pin daya IC." },
  { "en": "Derau akibat perubahan arus di ground?", "id": "Ground bounce." },
  { "en": "Jaringan penyedia daya pada chip?", "id": "PDN (Power Distribution Network)." },
  { "en": "Rentang tegangan input untuk histeresis?", "id": "Hysteresis band." },
  { "en": "Model komputasi teoretis?", "id": "Mesin Turing (Turing Machine)." },
  { "en": "Tujuan mesin Turing?", "id": "Mendefinisikan komputasi secara matematis." },
  { "en": "Level abstraksi desain (perilaku)?", "id": "Behavioral level." },
  { "en": "Level abstraksi desain (transfer register)?", "id": "RTL (Register-Transfer Level)." },
  { "en": "Level abstraksi desain (gerbang logika)?", "id": "Gate level." },
  { "en": "Simulasi vs Sintesis?", "id": "Verifikasi vs pembuatan sirkuit." },
  { "en": "Koneksi atau 'kabel' dalam HDL?", "id": "Net atau Wire." },
  { "en": "Variabel penyimpan nilai dalam HDL?", "id": "Register (reg)." },
  { "en": "Sinyal yang tidak terhubung (mengambang)?", "id": "Floating signal." },
  { "en": "Pintu masuk/keluar pada sirkuit?", "id": "Port." },
  { "en": "Deklarasi port dalam modul HDL?", "id": "Input, output, inout." },
  { "en": "Penggunaan kembali sebuah modul desain?", "id": "Instantiation." },
  { "en": "Desain dengan parameter yang bisa diubah?", "id": "Parameterized design." },
  { "en": "Nilai tetap dalam kode?", "id": "Konstanta (Constant)." },
  { "en": "Loop pada deskripsi hardware?", "id": "Generate loop." },
  { "en": "Pernyataan kondisional pada hardware?", "id": "if-else, case." },
  { "en": "Multiplexer 4-ke-1 butuh berapa select line?", "id": "2 select lines." },
  { "en": "Decoder 3-ke-8 butuh berapa input?", "id": "3 input." },
  { "en": "Counter 4-bit bisa menghitung sampai?", "id": "15 (desimal)." },
  { "en": "Memori 1K x 8 butuh berapa address line?", "id": "10 address lines (2^10=1024)." },
  { "en": "Kapasitas memori 1K x 8?", "id": "8 Kilobit atau 1 Kilobyte." },
  { "en": "Siklus mesin (machine cycle)?", "id": "Waktu eksekusi instruksi dasar." },
  { "en": "Siklus clock (clock cycle)?", "id": "Periode sinyal clock." },
  { "en": "Instruksi per siklus (IPC)?", "id": "Ukuran efisiensi prosesor." },
  { "en": "IPC lebih dari 1 dimungkinkan oleh?", "id": "Arsitektur superscalar." },
  { "en": "Peningkatan frekuensi clock disebut?", "id": "Overclocking." },
  { "en": "Risiko dari overclocking?", "id": "Panas berlebih, ketidakstabilan." },
  { "en": "Pengurangan frekuensi/tegangan hemat daya?", "id": "Underclocking/Undervolting." },
  { "en": "Sistem angka komplemen basis (r-1)?", "id": "Diminished radix complement." },
  { "en": "Sistem angka komplemen basis (r)?", "id": "Radix complement." },
  { "en": "1's complement adalah contoh dari?", "id": "Diminished radix complement." },
  { "en": "2's complement adalah contoh dari?", "id": "Radix complement." },
  { "en": "Kode ASCII extended menggunakan berapa bit?", "id": "8-bit." },
  { "en": "Jumlah karakter ASCII extended?", "id": "256 karakter." },
  { "en": "Sinyal yang memulai komunikasi?", "id": "Request." },
  { "en": "Sinyal yang mengonfirmasi penerimaan?", "id": "Acknowledge." },
  { "en": "Protokol polling?", "id": "Master memeriksa status slave." },
  { "en": "Protokol berbasis interupsi?", "id": "Slave memberitahu master." },
  { "en": "Pengontrol interupsi yang bisa diprogram?", "id": "PIC (Programmable Interrupt Controller)." },
  { "en": "Peta memori (memory map)?", "id": "Alokasi alamat untuk hardware." },
  { "en": "Mode proteksi pada CPU?", "id": "Protected mode." },
  { "en": "Mode awal pada prosesor x86?", "id": "Real mode." },
  { "en": "Catu daya yang tidak bisa diinterupsi?", "id": "UPS (Uninterruptible Power Supply)." },
  { "en": "Penstabil tegangan?", "id": "Voltage regulator." },
  { "en": "Sistem digital toleran kesalahan?", "id": "Fault-tolerant system." },
  { "en": "Redundansi pada sistem digital?", "id": "Menggunakan komponen cadangan." },
  { "en": "Redundansi modular tiga rangkap?", "id": "Triple Modular Redundancy (TMR)." },
  { "en": "Logika pemungutan suara (voting logic)?", "id": "Memilih output mayoritas." },
  { "en": "Sistem digital untuk aplikasi kritis?", "id": "Safety-critical system." },
  { "en": "Contoh aplikasi safety-critical?", "id": "Avionik, medis, otomotif." },
  { "en": "Verifikasi formal menggunakan model checking?", "id": "Memeriksa semua state sistem." },
  { "en": "Sistem waktu-nyata (real-time system)?", "id": "Sistem dengan batasan waktu." },
  { "en": "Batas waktu eksekusi tugas?", "id": "Deadline." },
  { "en": "Sistem 'hard real-time'?", "id": "Deadline tidak boleh terlewat." },
  { "en": "Sistem 'soft real-time'?", "id": "Deadline boleh terlewat sesekali." },
  { "en": "Sistem operasi waktu-nyata?", "id": "RTOS (Real-Time Operating System)." },
  { "en": "Penjadwalan tugas pada RTOS?", "id": "Task scheduling." },
  { "en": "Tugas dengan prioritas lebih tinggi?", "id": "Didahulukan oleh scheduler." },
  { "en": "Efek 'lipatan' frekuensi saat sampling?", "id": "Aliasing." },
  { "en": "Solusi untuk mencegah aliasing?", "id": "Filter anti-aliasing." },
  { "en": "Filter sebelum ADC disebut?", "id": "Filter anti-aliasing." },
  { "en": "Fungsi 'windowing' dalam DSP?", "id": "Mengurangi kebocoran spektral." },
  { "en": "Contoh fungsi windowing?", "id": "Hamming, Hanning, Blackman." },
  { "en": "Pita frekuensi yang dilewatkan filter?", "id": "Passband." },
  { "en": "Pita frekuensi yang diredam filter?", "id": "Stopband." },
  { "en": "Riak kecil di passband filter?", "id": "Passband ripple." },
  { "en": "Modulasi fasa dengan 4 keadaan?", "id": "QPSK." },
  { "en": "Modulasi amplitudo dan fasa?", "id": "QAM." },
  { "en": "Interferensi antar simbol berurutan?", "id": "ISI (Inter-Symbol Interference)." },
  { "en": "Kode untuk deteksi & koreksi eror?", "id": "Channel coding." },
  { "en": "Contoh channel coding?", "id": "Convolutional codes, Turbo codes." },
  { "en": "Wilayah kerja transistor sebagai saklar?", "id": "Cutoff dan saturation." },
  { "en": "Wilayah kerja transistor sebagai penguat?", "id": "Linear / Active region." },
  { "en": "Teknologi transistor 3D?", "id": "FinFET." },
  { "en": "Keuntungan utama FinFET?", "id": "Mengurangi arus bocor." },
  { "en": "Arus yang bocor melalui gerbang transistor?", "id": "Gate leakage." },
  { "en": "Perubahan threshold voltage karena substrat?", "id": "Body effect." },
  { "en": "Batasan waktu dalam desain FPGA?", "id": "Timing constraints." },
  { "en": "Perintah HDL untuk batasan clock?", "id": "create_clock." },
  { "en": "Perencanaan penempatan blok pada FPGA?", "id": "Floorplanning." },
  { "en": "Daya yang dikonsumsi saat sirkuit diam?", "id": "Daya statis." },
  { "en": "Daya yang dikonsumsi saat sirkuit beralih?", "id": "Daya dinamis." },
  { "en": "Rangkaian pendeteksi perbedaan fasa?", "id": "Phase detector." },
  { "en": "Rangkaian penghasil arus dari beda fasa?", "id": "Charge pump." },
  { "en": "Osilator yang frekuensinya diatur tegangan?", "id": "VCO (Voltage-Controlled Oscillator)." },
  { "en": "Tiga komponen utama PLL?", "id": "Phase detector, Filter, VCO." },
  { "en": "Kebijakan penggantian cache (paling lama tak dipakai)?", "id": "LRU (Least Recently Used)." },
  { "en": "Kebijakan penggantian cache (pertama masuk)?", "id": "FIFO (First-In, First-Out)." },
  { "en": "Deteksi overflow pada penjumlahan 2's complement?", "id": "Carry in dan carry out berbeda." },
  { "en": "Tujuan utama CRC?", "id": "Deteksi eror lebih andal." },
  { "en": "Bidang matematika untuk kode ECC?", "id": "Galois Field." },
  { "en": "Pengujian chip saat masih di wafer?", "id": "Wafer-level testing." },
  { "en": "Proses memilah chip bagus dan jelek?", "id": "Die sorting." },
  { "en": "Koneksi chip ke kemasan?", "id": "Wire bonding." },
  { "en": "Papan sirkuit fleksibel?", "id": "Flexible PCB (FPC)." },
  { "en": "Angka 'gain' dari sebuah buffer?", "id": "Satu (tidak menguatkan tegangan)." },
  { "en": "Gerbang logika dengan output open-drain?", "id": "Membutuhkan pull-up eksternal." },
  { "en": "Contoh bus dengan logika open-drain?", "id": "Bus I2C." },
  { "en": "Perbedaan logika dan aritmatika?", "id": "Logika (benar/salah), Aritmatika (angka)." },
  { "en": "Operasi 'shift left' setara dengan?", "id": "Perkalian dengan 2." },
  { "en": "Operasi 'shift right' setara dengan?", "id": "Pembagian dengan 2." },
  { "en": "Register penggeser dua arah?", "id": "Bidirectional shift register." },
  { "en": "Register geser dengan input dari output?", "id": "Feedback shift register." },
  { "en": "Register untuk membangkitkan sekuens biner?", "id": "LFSR." },
  { "en": "Kepanjangan LFSR?", "id": "Linear Feedback Shift Register." },
  { "en": "Aplikasi LFSR?", "id": "Pembangkit angka acak, CRC." },
  { "en": "Memori dua port?", "id": "Dual-port memory." },
  { "en": "Keuntungan dual-port memory?", "id": "Bisa diakses dari dua sumber." },
  { "en": "Memori yang isinya dipetakan ke file?", "id": "Memory-mapped file." },
  { "en": "Proses inisialisasi memori saat startup?", "id": "Memory initialization." },
  { "en": "Pengontrol memori?", "id": "Memory controller." },
  { "en": "Fungsi memory controller?", "id": "Mengatur akses ke DRAM." },
  { "en": "Bank memori adalah?", "id": "Bagian independen dari memori." },
  { "en": "Interleaving memori?", "id": "Mengakses beberapa bank bersamaan." },
  { "en": "Tujuan interleaving?", "id": "Meningkatkan throughput memori." },
  { "en": "Sinyal 'chip select' atau 'chip enable'?", "id": "Mengaktifkan sebuah chip." },
  { "en": "Sinyal 'output enable'?", "id": "Mengaktifkan output buffer." },
  { "en": "Arbiter bus?", "id": "Mengatur siapa yang boleh pakai bus." },
  { "en": "Skema arbitrasi bus?", "id": "Daisy chain, Polling, Fixed-priority." },
  { "en": "Proses sinkronisasi data antar clock domain?", "id": "Handshaking." },
  { "en": "FIFO untuk menyeberangi domain clock?", "id": "Asynchronous FIFO." },
  { "en": "Sirkuit logika yang ditenagai cahaya?", "id": "Optical logic." },
  { "en": "Sirkuit yang menggunakan spin elektron?", "id": "Spintronics." },
  { "en": "Komputasi menggunakan molekul DNA?", "id": "DNA computing." },
  { "en": "Peta Karnaugh untuk 5 variabel?", "id": "Dua peta 4 variabel." },
  { "en": "Metode penyederhanaan tabular?", "id": "Metode Quine-McCluskey." },
  { "en": "Ekspresi Boolean paling sederhana?", "id": "Minimal expression." },
  { "en": "Implikan prima esensial?", "id": "Harus ada dalam hasil akhir." },
  { "en": "Gerbang logika dengan histeresis?", "id": "Schmitt trigger." },
  { "en": "Tujuan histeresis pada input?", "id": "Mencegah transisi palsu." },
  { "en": "Waktu naik sinyal (rise time)?", "id": "Waktu dari 10% ke 90%." },
  { "en": "Waktu turun sinyal (fall time)?", "id": "Waktu dari 90% ke 10%." },
  { "en": "Efek 'ground bounce' disebabkan oleh?", "id": "Induktansi pada pin ground." },
  { "en": "Efek 'power bounce' (VCC bounce)?", "id": "Induktansi pada pin power." },
  { "en": "Kapasitor bypass sama dengan?", "id": "Decoupling capacitor." },
  { "en": "Jalur transmisi pada PCB?", "id": "Microstrip atau Stripline." },
  { "en": "Impedansi karakteristik saluran transmisi?", "id": "Impedansi yang dilihat sinyal." },
  { "en": "Alat untuk mengukur impedansi?", "id": "TDR (Time-Domain Reflectometer)." },
  { "en": "Analisis integritas daya?", "id": "Power Integrity (PI) analysis." },
  { "en": "Analisis kompatibilitas elektromagnetik?", "id": "EMC analysis." },
  { "en": "Pancaran noise elektromagnetik?", "id": "EMI (Electromagnetic Interference)." },
  { "en": "Desain perangkat keras digital?", "id": "Hardware design." },
  { "en": "Verifikasi perangkat keras?", "id": "Hardware verification." },
  { "en": "Proses mengubah spesifikasi ke desain?", "id": "Sintesis." },
  { "en": "Emulator sirkuit dalam software?", "id": "Circuit simulator (misal: SPICE)." },
  { "en": "Model transistor untuk SPICE?", "id": "SPICE model." },
  { "en": "Bahasa C/C++ untuk deskripsi hardware?", "id": "SystemC." },
  { "en": "Sintesis level tinggi?", "id": "High-Level Synthesis (HLS)." },
  { "en": "Keuntungan HLS?", "id": "Desain lebih cepat." },
  { "en": "Verifikasi berbasis pernyataan (assertion)?", "id": "Assertion-Based Verification." },
  { "en": "Pernyataan yang mendefinisikan perilaku?", "id": "Assertion." },
  { "en": "Keluarga logika IIL atau I2L?", "id": "Integrated Injection Logic." }


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
