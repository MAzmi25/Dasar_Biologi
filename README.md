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


  { "en": "Apa Unit Dasar Kehidupan?", "id": "Sel Adalah Unit Dasar Kehidupan." },
  { "en": "Apa Fungsi Utama Inti Sel (Nukleus)?", "id": "Mengontrol Semua Aktivitas Sel." },
  { "en": "Di Mana Energi Sel Diproduksi?", "id": "Energi Sel Diproduksi Di Mitokondria." },
  { "en": "Apa Fungsi Membran Sel?", "id": "Mengatur Keluar Masuknya Zat." },
  { "en": "Apa Itu Sitoplasma?", "id": "Cairan Di Dalam Sel." },
  { "en": "Apa Fungsi Ribosom?", "id": "Tempat Terjadinya Sintesis Protein." },
  { "en": "Apa Perbedaan Sel Hewan Dan Tumbuhan?", "id": "Sel Tumbuhan Memiliki Dinding Sel." },
  { "en": "Apa Itu Deoxyribonucleic Acid (DNA)?", "id": "Materi Genetik Pembawa Informasi Keturunan." },
  { "en": "Apa Bentuk Struktur Deoxyribonucleic Acid (DNA)?", "id": "Bentuknya Adalah Heliks Ganda." },
  { "en": "Apa Itu Gen?", "id": "Segmen Deoxyribonucleic Acid (DNA) Pengkode Protein." },
  { "en": "Apa Itu Kromosom?", "id": "Struktur Pembawa Deoxyribonucleic Acid (DNA)." },
  { "en": "Berapa Jumlah Kromosom Pada Manusia?", "id": "Manusia Memiliki Empat Puluh Enam Kromosom." },
  { "en": "Siapa Yang Dikenal Sebagai Bapak Genetika?", "id": "Gregor Mendel Adalah Bapak Genetika." },
  { "en": "Apa Itu Fotosintesis?", "id": "Proses Pembuatan Makanan Pada Tumbuhan." },
  { "en": "Apa Yang Dibutuhkan Untuk Fotosintesis?", "id": "Cahaya Matahari, Air, Karbon Dioksida." },
  { "en": "Apa Hasil Dari Proses Fotosintesis?", "id": "Glukosa Atau Gula Dan Oksigen." },
  { "en": "Di Bagian Mana Fotosintesis Terjadi?", "id": "Terjadi Di Dalam Kloroplas." },
  { "en": "Apa Peran Klorofil Dalam Fotosintesis?", "id": "Menyerap Energi Dari Cahaya Matahari." },
  { "en": "Mengapa Daun Umumnya Berwarna Hijau?", "id": "Karena Mengandung Banyak Zat Klorofil." },
  { "en": "Apa Itu Ekosistem?", "id": "Interaksi Makhluk Hidup Dengan Lingkungannya." },
  { "en": "Apa Saja Komponen Biotik?", "id": "Semua Makhluk Hidup Di Ekosistem." },
  { "en": "Apa Saja Komponen Abiotik?", "id": "Benda Tak Hidup Di Ekosistem." },
  { "en": "Apa Itu Rantai Makanan?", "id": "Proses Makan Dan Dimakan Organisme." },
  { "en": "Siapa Produsen Dalam Rantai Makanan?", "id": "Organisme Yang Menghasilkan Makanan Sendiri." },
  { "en": "Apa Peran Konsumen?", "id": "Memakan Organisme Lain Untuk Energi." },
  { "en": "Apa Peran Dekomposer Atau Pengurai?", "id": "Menguraikan Sisa-Sisa Makhluk Hidup." },
  { "en": "Apa Itu Simbiosis Mutualisme?", "id": "Hubungan Yang Saling Menguntungkan Keduanya." },
  { "en": "Apa Itu Taksonomi?", "id": "Ilmu Klasifikasi Makhluk Hidup." },
  { "en": "Siapa Bapak Taksonomi Modern?", "id": "Carolus Linnaeus Adalah Bapak Taksonomi." },
  { "en": "Apa Urutan Taksonomi Dari Tertinggi?", "id": "Kingdom, Filum, Kelas, Ordo, Famili." },
  { "en": "Sebutkan Lima Kingdom Makhluk Hidup?", "id": "Monera, Protista, Fungi, Plantae, Animalia." },
  { "en": "Apa Ciri Utama Kingdom Monera?", "id": "Organisme Uniseluler Tanpa Membran Inti." },
  { "en": "Apa Ciri Utama Kingdom Fungi?", "id": "Mendapatkan Makanan Dengan Mengurai." },
  { "en": "Apa Ciri Utama Kingdom Plantae?", "id": "Dapat Melakukan Proses Fotosintesis." },
  { "en": "Apa Ciri Utama Kingdom Animalia?", "id": "Dapat Bergerak Dan Bersifat Heterotrof." },
  { "en": "Apa Itu Teori Evolusi?", "id": "Perubahan Makhluk Hidup Seiring Waktu." },
  { "en": "Siapa Pencetus Teori Seleksi Alam?", "id": "Charles Darwin, Melalui Seleksi Alam." },
  { "en": "Apa Itu Seleksi Alam?", "id": "Alam Menyeleksi Individu Paling Adaptif." },
  { "en": "Apa Itu Adaptasi?", "id": "Penyesuaian Diri Terhadap Lingkungan Sekitar." },
  { "en": "Apa Fungsi Jantung Manusia?", "id": "Memompa Darah Ke Seluruh Tubuh." },
  { "en": "Apa Fungsi Utama Paru-Paru?", "id": "Tempat Pertukaran Oksigen Dan Karbon Dioksida." },
  { "en": "Apa Fungsi Ginjal?", "id": "Menyaring Darah Dan Menghasilkan Urine." },
  { "en": "Apa Fungsi Utama Hati?", "id": "Menetralkan Racun Dalam Tubuh." },
  { "en": "Apa Itu Sistem Saraf?", "id": "Mengatur Dan Mengoordinasikan Aktivitas Tubuh." },
  { "en": "Apa Itu Sistem Pencernaan?", "id": "Memproses Makanan Menjadi Nutrisi Penting." },
  { "en": "Apa Itu Sistem Pernapasan?", "id": "Sistem Organ Untuk Pertukaran Gas." },
  { "en": "Apa Itu Sistem Peredaran Darah?", "id": "Mengedarkan Darah Ke Seluruh Tubuh." },
  { "en": "Apa Itu Hormon?", "id": "Zat Kimia Pengatur Fungsi Tubuh." },
  { "en": "Apa Itu Mikroorganisme?", "id": "Organisme Yang Sangat Kecil Ukurannya." },
  { "en": "Apa Contoh Dari Mikroorganisme?", "id": "Bakteri, Virus, Jamur, Dan Protozoa." },
  { "en": "Apa Itu Bakteri?", "id": "Mikroorganisme Bersel Tunggal Tanpa Nukleus." },
  { "en": "Apakah Semua Jenis Bakteri Merugikan?", "id": "Tidak, Ada Bakteri Yang Bermanfaat." },
  { "en": "Apa Itu Virus?", "id": "Agen Infeksius Berukuran Sangat Kecil." },
  { "en": "Bagaimana Cara Virus Dapat Bereproduksi?", "id": "Dengan Menginfeksi Sel Inang Lainnya." },
  { "en": "Apa Fungsi Utama Akar Tumbuhan?", "id": "Menyerap Air Dan Nutrisi Penting." },
  { "en": "Apa Fungsi Utama Batang?", "id": "Menopang Tumbuhan Dan Mengangkut Zat." },
  { "en": "Apa Fungsi Utama Daun?", "id": "Tempat Terjadinya Proses Fotosintesis." },
  { "en": "Apa Fungsi Utama Bunga?", "id": "Sebagai Alat Reproduksi Pada Tumbuhan." },
  { "en": "Apa Perbedaan Vertebrata Dan Invertebrata?", "id": "Vertebrata Memiliki Tulang Belakang." },
  { "en": "Sebutkan Contoh Hewan Vertebrata?", "id": "Ikan, Amfibi, Reptil, Burung, Mamalia." },
  { "en": "Apa Ciri Khas Hewan Mamalia?", "id": "Memiliki Kelenjar Susu Dan Berambut." },
  { "en": "Apa Itu Hewan Amfibi?", "id": "Hewan Yang Hidup Di Dua Alam." },
  { "en": "Apa Itu Metabolisme?", "id": "Semua Reaksi Kimia Dalam Tubuh." },
  { "en": "Apa Itu Proses Katabolisme?", "id": "Proses Pemecahan Molekul Kompleks." },
  { "en": "Apa Itu Proses Anabolisme?", "id": "Proses Pembentukan Molekul Kompleks." },
  { "en": "Apa Itu Homeostasis?", "id": "Keseimbangan Kondisi Internal Tubuh." },
  { "en": "Apa Itu Enzim?", "id": "Protein Yang Mempercepat Reaksi Biokimia." },
  { "en": "Apa Fungsi Utama Enzim?", "id": "Mempercepat Laju Reaksi Kimia." },
  { "en": "Apa Itu Adenosine Triphosphate (ATP)?", "id": "Molekul Penyimpan Energi Utama Sel." },
  { "en": "Apa Itu Proses Osmosis?", "id": "Perpindahan Air Melalui Membran Semipermeabel." },
  { "en": "Apa Itu Proses Difusi?", "id": "Pergerakan Zat Dari Konsentrasi Tinggi." },
  { "en": "Apa Itu Pembelahan Sel Mitosis?", "id": "Menghasilkan Dua Sel Anak Identik." },
  { "en": "Apa Itu Pembelahan Sel Meiosis?", "id": "Menghasilkan Sel Kelamin (Gamet)." },
  { "en": "Apa Yang Dimaksud Dengan Jaringan?", "id": "Kumpulan Sel Dengan Fungsi Sama." },
  { "en": "Apa Yang Dimaksud Dengan Organ?", "id": "Kumpulan Jaringan Dengan Fungsi Tertentu." },
  { "en": "Apa Itu Sistem Organ?", "id": "Kumpulan Organ Yang Bekerja Sama." },
  { "en": "Apa Itu Respirasi Seluler?", "id": "Proses Menghasilkan Energi Dari Makanan." },
  { "en": "Di Mana Respirasi Seluler Terjadi?", "id": "Terjadi Di Dalam Sitoplasma Mitokondria." },
  { "en": "Apa Itu Alel?", "id": "Bentuk Alternatif Dari Suatu Gen." },
  { "en": "Apa Itu Genotipe?", "id": "Susunan Genetik Suatu Individu." },
  { "en": "Apa Itu Fenotipe?", "id": "Sifat Fisik Yang Tampak." },
  { "en": "Apa Itu Mutasi?", "id": "Perubahan Permanen Pada Urutan Deoxyribonucleic Acid." },
  { "en": "Apa Itu Herbivora?", "id": "Hewan Yang Hanya Memakan Tumbuhan." },
  { "en": "Apa Itu Karnivora?", "id": "Hewan Yang Hanya Memakan Daging." },
  { "en": "Apa Itu Omnivora?", "id": "Hewan Pemakan Tumbuhan Dan Daging." },
  { "en": "Apa Itu Autotrof?", "id": "Organisme Penghasil Makanan Sendiri." },
  { "en": "Apa Itu Heterotrof?", "id": "Organisme Pemakan Organisme Lain." },
  { "en": "Apa Itu Ekologi?", "id": "Ilmu Hubungan Makhluk Hidup Lingkungannya." },
  { "en": "Apa Itu Populasi?", "id": "Kumpulan Individu Sejenis Di Wilayah." },
  { "en": "Apa Itu Komunitas?", "id": "Kumpulan Populasi Berbeda Di Wilayah." },
  { "en": "Apa Itu Biosfer?", "id": "Bagian Bumi Yang Dihuni Kehidupan." },
  { "en": "Apa Fungsi Pembuluh Darah Vena?", "id": "Membawa Darah Kembali Ke Jantung." },
  { "en": "Apa Fungsi Pembuluh Darah Arteri?", "id": "Membawa Darah Dari Jantung." },
  { "en": "Apa Itu Sel Darah Merah?", "id": "Mengangkut Oksigen Ke Seluruh Tubuh." },
  { "en": "Apa Itu Sel Darah Putih?", "id": "Melawan Infeksi Dan Penyakit." },
  { "en": "Apa Itu Penyerbukan?", "id": "Proses Jatuhnya Serbuk Sari." },
  { "en": "Apa Itu Fosil?", "id": "Sisa-Sisa Makhluk Hidup Zaman Dulu." },
  { "en": "Apa Itu Karbohidrat?", "id": "Senyawa Sumber Energi Utama Tubuh." },
  { "en": "Apa Fungsi Utama Protein?", "id": "Membangun Dan Memperbaiki Jaringan Tubuh." },
  { "en": "Apa Fungsi Utama Lipid Atau Lemak?", "id": "Sebagai Cadangan Energi Jangka Panjang." },
  { "en": "Apa Itu Asam Amino?", "id": "Unit Dasar Penyusun Molekul Protein." },
  { "en": "Apa Itu Asam Nukleat?", "id": "Molekul Pembawa Informasi Genetik." },
  { "en": "Apa Contoh Asam Nukleat?", "id": "Deoxyribonucleic Acid (DNA) Dan Ribonucleic Acid (RNA)." },
  { "en": "Apa Fungsi Ribonucleic Acid (RNA)?", "id": "Membantu Dalam Proses Sintesis Protein." },
  { "en": "Apa Itu Transpirasi Pada Tumbuhan?", "id": "Proses Penguapan Air Dari Daun." },
  { "en": "Apa Fungsi Jaringan Xilem?", "id": "Mengangkut Air Dan Mineral." },
  { "en": "Apa Fungsi Jaringan Floem?", "id": "Mengangkut Hasil Fotosintesis Ke Tumbuhan." },
  { "en": "Apa Fungsi Stomata Pada Daun?", "id": "Tempat Pertukaran Gas Dan Penguapan." },
  { "en": "Apa Itu Hormon Auksin?", "id": "Hormon Yang Mengatur Pertumbuhan Tumbuhan." },
  { "en": "Apa Itu Gerak Fototropisme?", "id": "Gerak Tumbuhan Menuju Arah Cahaya." },
  { "en": "Apa Itu Hewan Berdarah Panas?", "id": "Hewan Yang Menjaga Suhu Tubuh." },
  { "en": "Apa Itu Hewan Berdarah Dingin?", "id": "Suhu Tubuh Mengikuti Lingkungan Sekitar." },
  { "en": "Apa Itu Sistem Ekskresi?", "id": "Sistem Pengeluaran Sisa Metabolisme." },
  { "en": "Apa Fungsi Utama Sistem Imun?", "id": "Melindungi Tubuh Dari Penyakit." },
  { "en": "Apa Itu Neuron Atau Sel Saraf?", "id": "Unit Fungsional Dasar Sistem Saraf." },
  { "en": "Apa Itu Reproduksi Aseksual?", "id": "Reproduksi Tanpa Peleburan Sel Kelamin." },
  { "en": "Apa Itu Reproduksi Seksual?", "id": "Reproduksi Melibatkan Peleburan Dua Gamet." },
  { "en": "Apa Itu Sel Gamet?", "id": "Sel Reproduksi Atau Sel Kelamin." },
  { "en": "Apa Itu Fertilisasi Atau Pembuahan?", "id": "Proses Peleburan Sel Sperma Ovum." },
  { "en": "Apa Hasil Dari Proses Fertilisasi?", "id": "Hasilnya Adalah Sebuah Sel Zigot." },
  { "en": "Apa Itu Jaringan Epitel?", "id": "Jaringan Pelapis Permukaan Organ Tubuh." },
  { "en": "Apa Itu Jaringan Ikat?", "id": "Jaringan Yang Mengikat Jaringan Lain." },
  { "en": "Sebutkan Tiga Jenis Jaringan Otot?", "id": "Otot Polos, Lurik, Dan Jantung." },
  { "en": "Apa Fungsi Jaringan Meristem Tumbuhan?", "id": "Jaringan Yang Selnya Aktif Membelah." },
  { "en": "Apa Fungsi Mata Manusia?", "id": "Sebagai Indra Penglihatan Untuk Melihat." },
  { "en": "Bagian Mata Mana Yang Menangkap Cahaya?", "id": "Cahaya Ditangkap Oleh Bagian Retina." },
  { "en": "Apa Fungsi Utama Telinga?", "id": "Sebagai Indra Pendengaran Dan Keseimbangan." },
  { "en": "Apa Fungsi Utama Hidung?", "id": "Sebagai Indra Penciuman Atau Pembau." },
  { "en": "Apa Fungsi Utama Lidah?", "id": "Sebagai Indra Pengecap Rasa Makanan." },
  { "en": "Apa Fungsi Kulit Manusia?", "id": "Sebagai Indra Peraba Dan Pelindung." },
  { "en": "Apa Itu Tumbuhan Berbiji Terbuka?", "id": "Bakal Biji Tidak Tertutup Daging." },
  { "en": "Apa Itu Tumbuhan Berbiji Tertutup?", "id": "Bakal Biji Tertutup Daging Buah." },
  { "en": "Apa Perbedaan Monokotil Dan Dikotil?", "id": "Jumlah Keping Biji Yang Berbeda." },
  { "en": "Apa Itu Habitat?", "id": "Tempat Tinggal Alami Suatu Organisme." },
  { "en": "Apa Itu Nisia Atau Relung?", "id": "Peran Fungsional Organisme Dalam Ekosistem." },
  { "en": "Apa Itu Piramida Makanan?", "id": "Gambaran Aliran Energi Antar Organisme." },
  { "en": "Siapa Yang Menempati Tingkat Trofik Pertama?", "id": "Organisme Produsen Seperti Tumbuhan." },
  { "en": "Apa Itu Daur Biogeokimia?", "id": "Siklus Unsur Kimia Di Alam." },
  { "en": "Bagaimana Bakteri Melakukan Proses Reproduksi?", "id": "Dengan Cara Pembelahan Biner." },
  { "en": "Apa Itu Antibiotik?", "id": "Zat Penghambat Pertumbuhan Bakteri." },
  { "en": "Siapa Penemu Antibiotik Penisilin Pertama?", "id": "Ditemukan Oleh Alexander Fleming." },
  { "en": "Apa Itu Hukum Mendel Satu?", "id": "Hukum Pemisahan Alel Secara Bebas." },
  { "en": "Apa Itu Pewarisan Sifat Dominan?", "id": "Sifat Yang Menutupi Sifat Lainnya." },
  { "en": "Apa Itu Pewarisan Sifat Resesif?", "id": "Sifat Yang Ditutupi Sifat Dominan." },
  { "en": "Apa Itu Golongan Darah Manusia?", "id": "Klasifikasi Darah Berdasarkan Antigen." },
  { "en": "Kromosom Apa Penentu Jenis Kelamin?", "id": "Kromosom X Dan Kromosom Y." },
  { "en": "Apa Itu Plankton?", "id": "Organisme Hanyut Di Perairan." },
  { "en": "Apa Perbedaan Fitoplankton Dan Zooplankton?", "id": "Fitoplankton Tumbuhan, Zooplankton Hewan." },
  { "en": "Apa Itu Bioluminesensi?", "id": "Produksi Cahaya Oleh Makhluk Hidup." },
  { "en": "Apa Itu Proses Fermentasi?", "id": "Proses Produksi Energi Tanpa Oksigen." },
  { "en": "Apa Itu Daur Hidup?", "id": "Tahapan Kehidupan Suatu Organisme." },
  { "en": "Apa Itu Metamorfosis?", "id": "Perubahan Bentuk Selama Daur Hidup." },
  { "en": "Apa Itu Metamorfosis Sempurna?", "id": "Meliputi Tahap Telur, Larva, Pupa." },
  { "en": "Apa Itu Jaringan Adiposa?", "id": "Jaringan Yang Menyimpan Lemak Tubuh." },
  { "en": "Apa Itu Kelenjar Endokrin?", "id": "Kelenjar Yang Menghasilkan Hormon." },
  { "en": "Apa Itu Usus Halus?", "id": "Tempat Penyerapan Sebagian Besar Nutrisi." },
  { "en": "Apa Fungsi Lambung?", "id": "Mencerna Makanan Dengan Asam Enzim." },
  { "en": "Apa Itu Diafragma?", "id": "Otot Utama Dalam Proses Pernapasan." },
  { "en": "Apa Fungsi Sel Darah Keping?", "id": "Membantu Dalam Proses Pembekuan Darah." },
  { "en": "Apa Itu Sistem Limfatik?", "id": "Membantu Melawan Infeksi Dalam Tubuh." },
  { "en": "Apa Itu Otak Kecil (Serebelum)?", "id": "Mengatur Keseimbangan Dan Koordinasi Gerak." },
  { "en": "Apa Itu Sumsum Tulang Belakang?", "id": "Menghubungkan Otak Dengan Sistem Saraf." },
  { "en": "Apa Itu Refleks?", "id": "Gerakan Otomatis Tanpa Kesadaran Penuh." },
  { "en": "Apa Itu Ureter?", "id": "Saluran Dari Ginjal Ke Kandung." },
  { "en": "Apa Itu Testosteron?", "id": "Hormon Seksual Utama Pada Pria." },
  { "en": "Apa Itu Estrogen?", "id": "Hormon Seksual Utama Pada Wanita." },
  { "en": "Apa Itu Ovulasi?", "id": "Pelepasan Sel Telur Dari Ovarium." },
  { "en": "Apa Itu Plasenta?", "id": "Organ Pemasok Nutrisi Kepada Janin." },
  { "en": "Apa Itu Embrio?", "id": "Tahap Awal Perkembangan Makhluk Hidup." },
  { "en": "Apa Itu Kloroplas?", "id": "Organel Tempat Terjadinya Proses Fotosintesis." },
  { "en": "Apa Itu Vakuola?", "id": "Organel Penyimpan Air Dan Zat." },
  { "en": "Apa Itu Dinding Sel?", "id": "Lapisan Kaku Pelindung Sel Tumbuhan." },
  { "en": "Apa Itu Lisosom?", "id": "Organel Pencerna Zat Asing Sel." },
  { "en": "Apa Itu Badan Golgi?", "id": "Memodifikasi Dan Mengemas Protein." },
  { "en": "Apa Itu Retikulum Endoplasma?", "id": "Jaringan Membran Produksi Protein Lemak." },
  { "en": "Apa Itu Filum?", "id": "Tingkatan Taksonomi Di Bawah Kingdom." },
  { "en": "Apa Itu Spesies?", "id": "Kelompok Organisme Yang Dapat Kawin." },
  { "en": "Apa Itu Predator?", "id": "Hewan Yang Memburu Hewan Lain." },
  { "en": "Apa Itu Mangsa?", "id": "Hewan Yang Diburu Oleh Predator." },
  { "en": "Apa Itu Parasitisme?", "id": "Satu Untung Dan Yang Lainnya Rugi." },
  { "en": "Apa Itu Komensalisme?", "id": "Satu Untung, Lainnya Tidak Terpengaruh." },
  { "en": "Apa Itu Rangka (Sistem Skeletal)?", "id": "Struktur Penopang Tubuh Makhluk Hidup." },
  { "en": "Apa Itu Sendi?", "id": "Hubungan Antara Dua Tulang Berbeda." },
  { "en": "Apa Itu Ligamen?", "id": "Jaringan Ikat Penghubung Tulang Sendi." },
  { "en": "Apa Itu Tendon?", "id": "Jaringan Ikat Penghubung Otot Tulang." },
  { "en": "Apa Itu Kartilago (Tulang Rawan)?", "id": "Jaringan Ikat Fleksibel Dan Kuat." },
  { "en": "Apa Itu Epidermis?", "id": "Lapisan Terluar Kulit Makhluk Hidup." },
  { "en": "Apa Itu Vitamin?", "id": "Nutrisi Organik Yang Dibutuhkan Tubuh." },
  { "en": "Apa Itu Mineral?", "id": "Nutrisi Anorganik Penting Bagi Tubuh." },
  { "en": "Apa Itu Dehidrasi?", "id": "Kondisi Kekurangan Cairan Dalam Tubuh." },
  { "en": "Apa Itu Vaksin?", "id": "Bahan Pemicu Sistem Kekebalan Tubuh." },
  { "en": "Apa Itu Patogen?", "id": "Agen Biologis Penyebab Suatu Penyakit." },
  { "en": "Apa Itu Kanker?", "id": "Penyakit Pertumbuhan Sel Tidak Normal." },
  { "en": "Apa Tulang Terpanjang Di Tubuh Manusia?", "id": "Tulang Paha Atau Disebut Femur." },
  { "en": "Apa Otot Terbesar Di Tubuh Manusia?", "id": "Otot Bokong Atau Gluteus Maximus." },
  { "en": "Apa Fungsi Utama Tulang Rusuk Manusia?", "id": "Melindungi Organ Jantung Dan Paru-Paru." },
  { "en": "Apa Fungsi Dari Organ Pankreas?", "id": "Menghasilkan Hormon Insulin Dan Enzim." },
  { "en": "Apa Fungsi Dari Kantung Empedu?", "id": "Menyimpan Cairan Empedu Dari Hati." },
  { "en": "Apa Fungsi Usus Besar?", "id": "Menyerap Kelebihan Air Sisa Makanan." },
  { "en": "Di Mana Letak Alveolus?", "id": "Di Ujung Saluran Udara Paru-Paru." },
  { "en": "Apa Itu Nefron?", "id": "Unit Fungsional Penyaring Pada Ginjal." },
  { "en": "Apa Fungsi Saraf Sensorik?", "id": "Mengirimkan Sinyal Rangsangan Ke Otak." },
  { "en": "Apa Fungsi Saraf Motorik?", "id": "Mengirimkan Perintah Dari Otak Otot." },
  { "en": "Apa Itu Bioteknologi?", "id": "Pemanfaatan Makhluk Hidup Untuk Produk." },
  { "en": "Apa Contoh Bioteknologi Konvensional?", "id": "Pembuatan Tempe, Yoghurt, Dan Tape." },
  { "en": "Apa Itu Proses Kloning?", "id": "Membuat Salinan Genetik Yang Identik." },
  { "en": "Apa Itu Rekayasa Genetika?", "id": "Manipulasi Langsung Gen Suatu Organisme." },
  { "en": "Apa Itu Tanaman Transgenik?", "id": "Tanaman Yang Gennya Telah Diubah." },
  { "en": "Apa Itu Terapi Gen?", "id": "Teknik Memperbaiki Gen Yang Rusak." },
  { "en": "Apa Penyebab Penyakit Diabetes?", "id": "Kekurangan Atau Resistensi Hormon Insulin." },
  { "en": "Apa Itu Reaksi Alergi?", "id": "Reaksi Berlebihan Sistem Imun Tubuh." },
  { "en": "Apa Itu Penyakit Anemia?", "id": "Kondisi Kekurangan Sel Darah Merah." },
  { "en": "Apa Itu Hipertensi?", "id": "Kondisi Tekanan Darah Terlalu Tinggi." },
  { "en": "Bagaimana Penyakit Demam Berdarah Ditularkan?", "id": "Melalui Gigitan Nyamuk Aedes Aegypti." },
  { "en": "Apa Itu Perkecambahan?", "id": "Tahap Awal Pertumbuhan Biji Tumbuhan." },
  { "en": "Apa Itu Masa Pubertas?", "id": "Masa Peralihan Anak Menjadi Dewasa." },
  { "en": "Apa Itu Proses Regenerasi?", "id": "Kemampuan Menumbuhkan Kembali Bagian Tubuh." },
  { "en": "Sebutkan Contoh Hewan Yang Beregenerasi?", "id": "Cicak, Bintang Laut, Dan Planaria." },
  { "en": "Apa Itu Partenogenesis?", "id": "Embrio Berkembang Tanpa Adanya Fertilisasi." },
  { "en": "Apa Itu Insting Atau Naluri?", "id": "Perilaku Bawaan Sejak Lahir." },
  { "en": "Apa Itu Migrasi Hewan?", "id": "Perpindahan Hewan Musiman Jarak Jauh." },
  { "en": "Apa Itu Hibernasi?", "id": "Masa Tidur Panjang Selama Musim Dingin." },
  { "en": "Apa Itu Kamuflase?", "id": "Penyamaran Agar Sesuai Dengan Lingkungan." },
  { "en": "Apa Itu Mimikri?", "id": "Meniru Penampilan Spesies Makhluk Lain." },
  { "en": "Apa Itu Kotiledon?", "id": "Daun Pertama Yang Muncul Berkecambah." },
  { "en": "Apa Fungsi Jaringan Kambium?", "id": "Penyebab Pertumbuhan Sekunder Pada Batang." },
  { "en": "Apa Itu Bunga Sempurna?", "id": "Memiliki Putik Dan Benang Sari." },
  { "en": "Apa Ciri Tumbuhan Xerofit?", "id": "Tumbuhan Adaptasi Di Lingkungan Kering." },
  { "en": "Apa Itu Tumbuhan Hidrofit?", "id": "Tumbuhan Yang Hidup Di Air." },
  { "en": "Apa Ciri Khas Filum Porifera?", "id": "Hewan Berpori Seperti Spons Laut." },
  { "en": "Apa Ciri Khas Filum Cnidaria?", "id": "Memiliki Sel Penyengat, Contohnya Ubur-Ubur." },
  { "en": "Apa Ciri Khas Filum Platyhelminthes?", "id": "Kelompok Hewan Cacing Berbentuk Pipih." },
  { "en": "Apa Ciri Khas Filum Annelida?", "id": "Kelompok Hewan Cacing Bersegmen." },
  { "en": "Apa Ciri Khas Filum Mollusca?", "id": "Kelompok Hewan Bertubuh Lunak." },
  { "en": "Apa Ciri Khas Filum Arthropoda?", "id": "Hewan Dengan Kaki Beruas Atau Berbuku." },
  { "en": "Apa Ciri Khas Filum Echinodermata?", "id": "Hewan Dengan Kulit Berduri." },
  { "en": "Apa Ciri Khas Filum Chordata?", "id": "Hewan Yang Memiliki Struktur Notokorda." },
  { "en": "Apa Fungsi Utama Mikroskop?", "id": "Alat Untuk Melihat Objek Mikroskopis." },
  { "en": "Apa Itu Kultur Jaringan?", "id": "Metode Perbanyakan Tumbuhan Dari Jaringan." },
  { "en": "Berapa Nilai pH Netral?", "id": "Tingkat Keasaman Dengan Nilai Tujuh." },
  { "en": "Apa Itu Denaturasi Protein?", "id": "Kerusakan Struktur Tiga Dimensi Protein." },
  { "en": "Apa Penyebab Denaturasi Protein?", "id": "Perubahan Suhu Atau Tingkat Keasaman." },
  { "en": "Apa Itu Sistem Binomial Nomenklatur?", "id": "Sistem Penamaan Ganda Makhluk Hidup." },
  { "en": "Siapa Penggagas Sistem Binomial Nomenklatur?", "id": "Sistem Ini Digagas Carolus Linnaeus." },
  { "en": "Apa Aturan Penulisan Nama Genus?", "id": "Kata Pertama Dan Diawali Huruf Kapital." },
  { "en": "Apa Aturan Penulisan Nama Spesies?", "id": "Kata Kedua Dan Diawali Huruf Kecil." },
  { "en": "Apa Itu Filogeni?", "id": "Studi Tentang Hubungan Evolusi Spesies." },
  { "en": "Apa Itu Pohon Filogenetik?", "id": "Diagram Hubungan Evolusi Antar Spesies." },
  { "en": "Apa Itu Struktur Vestigial?", "id": "Struktur Sisa Evolusi Tidak Berfungsi." },
  { "en": "Apa Itu Struktur Homolog?", "id": "Struktur Sama Dengan Fungsi Berbeda." },
  { "en": "Apa Itu Struktur Analog?", "id": "Struktur Berbeda Dengan Fungsi Sama." },
  { "en": "Apa Itu Keanekaragaman Hayati?", "id": "Keragaman Makhluk Hidup Di Bumi." },
  { "en": "Sebutkan Tingkatan Keanekaragaman Hayati?", "id": "Tingkat Gen, Jenis, Dan Ekosistem." },
  { "en": "Apa Itu Spesies Endemik?", "id": "Spesies Khas Suatu Wilayah Tertentu." },
  { "en": "Apa Itu Konservasi?", "id": "Upaya Perlindungan Dan Pelestarian Alam." },
  { "en": "Apa Itu Konservasi In Situ?", "id": "Pelestarian Langsung Di Habitat Aslinya." },
  { "en": "Apa Itu Konservasi Ex Situ?", "id": "Pelestarian Di Luar Habitat Aslinya." },
  { "en": "Apa Contoh Konservasi In Situ?", "id": "Cagar Alam Dan Taman Nasional." },
  { "en": "Apa Contoh Konservasi Ex Situ?", "id": "Kebun Binatang Dan Kebun Raya." },
  { "en": "Apa Itu Efek Rumah Kaca?", "id": "Pemanasan Atmosfer Akibat Gas Tertentu." },
  { "en": "Apa Itu Hujan Asam?", "id": "Hujan Dengan Tingkat Keasaman Rendah." },
  { "en": "Apa Itu Peristiwa Eutrofikasi?", "id": "Ledakan Pertumbuhan Alga Di Perairan." },
  { "en": "Apa Itu Lapisan Ozon?", "id": "Lapisan Pelindung Bumi Dari Radiasi." },
  { "en": "Apa Itu Polutan?", "id": "Zat Yang Menyebabkan Terjadinya Polusi." },
  { "en": "Apa Itu Rantai Karbon?", "id": "Dasar Dari Semua Molekul Organik." },
  { "en": "Apa Itu Monosakarida?", "id": "Unit Gula Paling Sederhana." },
  { "en": "Apa Contoh Monosakarida?", "id": "Glukosa, Fruktosa, Dan Juga Galaktosa." },
  { "en": "Apa Itu Disakarida?", "id": "Gabungan Dari Dua Unit Monosakarida." },
  { "en": "Apa Itu Polisakarida?", "id": "Karbohidrat Kompleks, Rantai Gula Panjang." },
  { "en": "Apa Contoh Polisakarida?", "id": "Pati, Glikogen, Dan Juga Selulosa." },
  { "en": "Apa Itu Glikogen?", "id": "Bentuk Simpanan Glukosa Pada Hewan." },
  { "en": "Apa Itu Selulosa?", "id": "Komponen Struktural Utama Dinding Sel." },
  { "en": "Apa Itu Ikatan Peptida?", "id": "Ikatan Kimia Antara Asam Amino." },
  { "en": "Apa Itu Kolesterol?", "id": "Jenis Lipid Penting Bagi Sel." },
  { "en": "Apa Itu Nukleotida?", "id": "Unit Dasar Penyusun Asam Nukleat." },
  { "en": "Sebutkan Tiga Komponen Nukleotida?", "id": "Gula, Fosfat, Dan Basa Nitrogen." },
  { "en": "Apa Saja Basa Nitrogen Deoxyribonucleic Acid?", "id": "Adenin, Guanin, Sitosin, Dan Timin." },
  { "en": "Basa Nitrogen Apa Yang Menggantikan Timin?", "id": "Urasil Menggantikan Timin Pada Ribonucleic Acid." },
  { "en": "Apa Itu Transkripsi Genetik?", "id": "Proses Penyalinan Deoxyribonucleic Acid Menjadi Ribonucleic Acid." },
  { "en": "Apa Itu Translasi Genetik?", "id": "Proses Penerjemahan Ribonucleic Acid Menjadi Protein." },
  { "en": "Apa Itu Kodon?", "id": "Tiga Urutan Basa Pada Ribonucleic Acid." },
  { "en": "Apa Itu Hipotesis?", "id": "Dugaan Sementara Dalam Penelitian Ilmiah." },
  { "en": "Apa Itu Variabel Bebas?", "id": "Variabel Yang Dimanipulasi Oleh Peneliti." },
  { "en": "Apa Itu Variabel Terikat?", "id": "Variabel Yang Diukur Dalam Eksperimen." },
  { "en": "Apa Itu Kelompok Kontrol?", "id": "Kelompok Pembanding Dalam Suatu Eksperimen." },
  { "en": "Apa Itu Virologi?", "id": "Cabang Ilmu Biologi Mempelajari Virus." },
  { "en": "Apa Itu Bakteriologi?", "id": "Cabang Ilmu Biologi Mempelajari Bakteri." },
  { "en": "Apa Itu Mikologi?", "id": "Cabang Ilmu Biologi Mempelajari Jamur." },
  { "en": "Apa Itu Botani?", "id": "Cabang Ilmu Biologi Mempelajari Tumbuhan." },
  { "en": "Apa Itu Zoologi?", "id": "Cabang Ilmu Biologi Mempelajari Hewan." },
  { "en": "Apa Itu Ilmu Morfologi?", "id": "Ilmu Yang Mempelajari Bentuk Luar Organisme." },
  { "en": "Apa Itu Ilmu Anatomi?", "id": "Ilmu Yang Mempelajari Struktur Dalam Organisme." },
  { "en": "Apa Itu Ilmu Fisiologi?", "id": "Ilmu Yang Mempelajari Fungsi Tubuh Organisme." },
  { "en": "Apa Itu Ilmu Histologi?", "id": "Ilmu Yang Mempelajari Jaringan Biologis." },
  { "en": "Apa Itu Ilmu Sitologi?", "id": "Ilmu Yang Mempelajari Tentang Sel." },
  { "en": "Apa Itu Ilmu Embriologi?", "id": "Ilmu Yang Mempelajari Perkembangan Embrio." },
  { "en": "Apa Itu Ilmu Patologi?", "id": "Ilmu Yang Mempelajari Tentang Penyakit." },
  { "en": "Apa Itu Ilmu Imunologi?", "id": "Ilmu Tentang Sistem Kekebalan Tubuh." },
  { "en": "Apa Itu Ilmu Ornitologi?", "id": "Cabang Ilmu Zoologi Mempelajari Burung." },
  { "en": "Apa Itu Ilmu Entomologi?", "id": "Cabang Ilmu Zoologi Mempelajari Serangga." },
  { "en": "Apa Itu Ilmu Iktiologi?", "id": "Cabang Ilmu Zoologi Mempelajari Ikan." },
  { "en": "Apa Itu Ilmu Paleontologi?", "id": "Ilmu Yang Mempelajari Tentang Fosil." },
  { "en": "Apa Itu Sistem Integumen?", "id": "Sistem Pelindung Luar Tubuh." },
  { "en": "Apa Fungsi Kelenjar Keringat?", "id": "Untuk Mengatur Suhu Tubuh Manusia." },
  { "en": "Apa Itu Keratin?", "id": "Protein Utama Penyusun Rambut Kuku." },
  { "en": "Apa Fungsi Utama Sistem Rangka?", "id": "Menopang Tubuh Dan Melindungi Organ." },
  { "en": "Bagaimana Cara Kerja Otot?", "id": "Dengan Cara Kontraksi Dan Relaksasi." },
  { "en": "Apa Itu Otot Antagonis?", "id": "Dua Otot Yang Bekerja Berlawanan." },
  { "en": "Apa Itu Sistem Endokrin?", "id": "Sistem Kelenjar Penghasil Hormon." },
  { "en": "Kelenjar Apa Yang Dijuluki Master Gland?", "id": "Kelenjar Pituitari Atau Kelenjar Hipofisis." },
  { "en": "Di Mana Letak Kelenjar Tiroid?", "id": "Terletak Di Bagian Depan Leher." },
  { "en": "Hormon Apa Yang Dihasilkan Pankreas?", "id": "Hormon Insulin Dan Juga Glukagon." },
  { "en": "Apa Itu Sel Prokariotik?", "id": "Sel Tanpa Membran Inti Sejati." },
  { "en": "Apa Itu Sel Eukariotik?", "id": "Sel Dengan Membran Inti Sejati." },
  { "en": "Apa Contoh Organisme Prokariotik?", "id": "Bakteri Dan Juga Archaea." },
  { "en": "Apa Itu Sitoskeleton?", "id": "Kerangka Internal Yang Menyokong Sel." },
  { "en": "Apa Fungsi Organel Sentriol?", "id": "Berperan Dalam Proses Pembelahan Sel." },
  { "en": "Apa Itu Fagositosis?", "id": "Proses Saat Sel Memakan Partikel." },
  { "en": "Apa Itu Pinositosis?", "id": "Proses Saat Sel Meminum Cairan." },
  { "en": "Apa Itu Endositosis?", "id": "Proses Memasukkan Zat Ke Dalam Sel." },
  { "en": "Apa Itu Eksositosis?", "id": "Proses Mengeluarkan Zat Dari Dalam Sel." },
  { "en": "Apa Itu Genom?", "id": "Keseluruhan Materi Genetik Suatu Organisme." },
  { "en": "Apa Itu Polimerase Chain Reaction (PCR)?", "id": "Teknik Untuk Memperbanyak Deoxyribonucleic Acid." },
  { "en": "Apa Itu Pindah Silang?", "id": "Pertukaran Materi Genetik Antar Kromosom." },
  { "en": "Apa Itu Hanyutan Genetik?", "id": "Perubahan Frekuensi Alel Secara Acak." },
  { "en": "Apa Itu Aliran Gen?", "id": "Transfer Materi Genetik Antar Populasi." },
  { "en": "Apa Itu Proses Spesiasi?", "id": "Proses Terbentuknya Spesies Baru." },
  { "en": "Apa Itu Isolasi Reproduksi?", "id": "Mekanisme Pencegah Perkawinan Antar Spesies." },
  { "en": "Apa Itu Radiasi Adaptif?", "id": "Evolusi Cepat Dari Satu Nenek Moyang." },
  { "en": "Apa Itu Bioma?", "id": "Komunitas Ekologis Besar Di Daratan." },
  { "en": "Sebutkan Contoh Bioma Di Dunia?", "id": "Gurun, Tundra, Taiga, Hutan Hujan." },
  { "en": "Apa Itu Suksesi Primer?", "id": "Suksesi Yang Terjadi Pada Lahan Baru." },
  { "en": "Apa Itu Suksesi Sekunder?", "id": "Suksesi Lahan Akibat Gangguan." },
  { "en": "Apa Itu Spesies Pionir?", "id": "Spesies Pertama Yang Menempati Lahan." },
  { "en": "Apa Itu Komunitas Klimaks?", "id": "Komunitas Stabil Akhir Dari Suksesi." },
  { "en": "Apa Itu Daya Dukung Lingkungan?", "id": "Kapasitas Maksimum Lingkungan Mendukung Kehidupan." },
  { "en": "Apa Itu Jaring-Jaring Makanan?", "id": "Gabungan Dari Beberapa Rantai Makanan." },
  { "en": "Apa Itu Siklus Karbon?", "id": "Siklus Pergerakan Karbon Di Alam." },
  { "en": "Apa Itu Fiksasi Nitrogen?", "id": "Pengubahan Gas Nitrogen Menjadi Amonia." },
  { "en": "Bakteri Apa Yang Memfiksasi Nitrogen?", "id": "Bakteri Rhizobium Di Akar Legum." },
  { "en": "Apa Itu Kaki Semu (Pseudopodia)?", "id": "Juluran Sitoplasma Untuk Bergerak Makan." },
  { "en": "Organisme Apa Yang Bergerak Pseudopodia?", "id": "Amoeba Dan Sel Darah Putih." },
  { "en": "Apa Itu Silia?", "id": "Struktur Mirip Rambut Untuk Bergerak." },
  { "en": "Apa Itu Flagela?", "id": "Struktur Mirip Cambuk Untuk Bergerak." },
  { "en": "Apa Itu Miselium?", "id": "Kumpulan Hifa Pada Tubuh Jamur." },
  { "en": "Apa Itu Kitin?", "id": "Polisakarida Penyusun Dinding Sel Jamur." },
  { "en": "Apa Itu Liken (Lumut Kerak)?", "id": "Simbiosis Mutualisme Jamur Dan Alga." },
  { "en": "Apa Itu Mikoriza?", "id": "Simbiosis Mutualisme Jamur Dan Akar." },
  { "en": "Apa Itu Ploid?", "id": "Jumlah Set Kromosom Di Sel." },
  { "en": "Apa Itu Sel Diploid?", "id": "Sel Dengan Dua Set Kromosom." },
  { "en": "Apa Itu Sel Haploid?", "id": "Sel Dengan Satu Set Kromosom." },
  { "en": "Apa Contoh Sel Haploid Manusia?", "id": "Sel Kelamin Seperti Sperma Ovum." },
  { "en": "Apa Itu Kariotipe?", "id": "Gambaran Susunan Kromosom Suatu Individu." },
  { "en": "Apa Itu Penyakit Genetik?", "id": "Penyakit Akibat Kelainan Materi Genetik." },
  { "en": "Apa Itu Penyakit Hemofilia?", "id": "Penyakit Sulitnya Darah Untuk Membeku." },
  { "en": "Apa Itu Buta Warna?", "id": "Ketidakmampuan Membedakan Warna Tertentu." },
  { "en": "Apa Itu Albino?", "id": "Kelainan Kekurangan Pigmen Melanin Kulit." },
  { "en": "Golongan Darah Apa Donor Universal?", "id": "Golongan Darah Tipe O Negatif." },
  { "en": "Golongan Darah Apa Resipien Universal?", "id": "Golongan Darah Tipe AB Positif." },
  { "en": "Apa Itu Faktor Rhesus (Rh)?", "id": "Protein Antigen Di Sel Darah." },
  { "en": "Apa Itu Osmoregulasi?", "id": "Pengaturan Keseimbangan Cairan Dalam Tubuh." },
  { "en": "Apa Itu Termoregulasi?", "id": "Pengaturan Suhu Internal Tubuh Organisme." },
  { "en": "Apa Itu Jaringan Darah?", "id": "Jaringan Ikat Berbentuk Cair." },
  { "en": "Apa Itu Plasma Darah?", "id": "Komponen Cair Dalam Darah Manusia." },
  { "en": "Apa Itu Hemoglobin?", "id": "Protein Pengikat Oksigen Darah Merah." },
  { "en": "Zat Apa Memberi Warna Merah Darah?", "id": "Hemoglobin Yang Mengandung Zat Besi." },
  { "en": "Berapa Lama Umur Sel Darah Merah?", "id": "Sekitar Seratus Dua Puluh Hari." },
  { "en": "Di Mana Sel Darah Merah Dihancurkan?", "id": "Di Dalam Organ Hati Limpa." },
  { "en": "Apa Itu Tekanan Darah Sistolik?", "id": "Tekanan Saat Jantung Sedang Berkontraksi." },
  { "en": "Apa Itu Tekanan Darah Diastolik?", "id": "Tekanan Saat Jantung Sedang Berelaksasi." },
  { "en": "Apa Itu Pembuluh Nadi (Arteri)?", "id": "Pembuluh Yang Membawa Darah Jantung." },
  { "en": "Apa Itu Pembuluh Balik (Vena)?", "id": "Pembuluh Yang Membawa Darah Jantung." },
  { "en": "Apa Itu Pembuluh Kapiler?", "id": "Pembuluh Darah Terkecil Di Tubuh." },
  { "en": "Apa Fungsi Katup Jantung?", "id": "Mencegah Darah Mengalir Kembali." },
  { "en": "Apa Perbedaan Pernapasan Dada Perut?", "id": "Otot Yang Digunakan Saat Bernapas." },
  { "en": "Otot Apa Untuk Pernapasan Dada?", "id": "Otot Antar Tulang Rusuk." },
  { "en": "Otot Apa Untuk Pernapasan Perut?", "id": "Otot Diafragma Di Bawah Paru-Paru." },
  { "en": "Apa Itu Pita Suara?", "id": "Jaringan Yang Bergetar Menghasilkan Suara." },
  { "en": "Di Mana Letak Pita Suara?", "id": "Terletak Di Dalam Laring." },
  { "en": "Apa Itu Enzim Ptialin (Amilase)?", "id": "Enzim Di Air Liur." },
  { "en": "Apa Fungsi Enzim Ptialin?", "id": "Mengubah Amilum Menjadi Gula Sederhana." },
  { "en": "Apa Itu Asam Klorida (HCl) Lambung?", "id": "Membunuh Kuman Dan Mengaktifkan Pepsin." },
  { "en": "Apa Fungsi Enzim Pepsin?", "id": "Memecah Protein Menjadi Pepton." },
  { "en": "Apa Fungsi Enzim Lipase?", "id": "Memecah Lemak Menjadi Asam Lemak." },
  { "en": "Apa Itu Sinapsis?", "id": "Celah Antara Dua Sel Saraf." },
  { "en": "Apa Itu Neurotransmitter?", "id": "Zat Kimia Pembawa Sinyal Saraf." },
  { "en": "Apa Bagian Utama Otak Manusia?", "id": "Otak Besar, Kecil, Batang Otak." },
  { "en": "Apa Fungsi Otak Besar (Serebrum)?", "id": "Pusat Kecerdasan, Ingatan, Dan Kesadaran." },
  { "en": "Apa Itu Pupil Mata?", "id": "Bukaan Tempat Cahaya Masuk Mata." },
  { "en": "Apa Fungsi Lensa Mata?", "id": "Memfokuskan Cahaya Yang Masuk Retina." },
  { "en": "Apa Itu Sel Kerucut (Cone Cell)?", "id": "Sel Fotoreseptor Untuk Melihat Warna." },
  { "en": "Apa Itu Sel Batang (Rod Cell)?", "id": "Sel Fotoreseptor Untuk Cahaya Redup." },
  { "en": "Apa Fungsi Gendang Telinga?", "id": "Menangkap Getaran Suara Dari Luar." },
  { "en": "Apa Itu Koklea (Rumah Siput)?", "id": "Bagian Telinga Pengubah Getaran Suara." },
  { "en": "Di Mana Sel Sperma Diproduksi?", "id": "Diproduksi Di Dalam Organ Testis." },
  { "en": "Di Mana Sel Telur Diproduksi?", "id": "Diproduksi Di Dalam Organ Ovarium." },
  { "en": "Apa Itu Tuba Fallopi?", "id": "Saluran Tempat Terjadinya Proses Pembuahan." },
  { "en": "Apa Itu Uterus (Rahim)?", "id": "Tempat Berkembangnya Janin Selama Kehamilan." },
  { "en": "Apa Itu Menstruasi?", "id": "Peluruhan Dinding Rahim Secara Periodik." },
  { "en": "Apa Itu Menopause?", "id": "Berhentinya Siklus Menstruasi Secara Alami." },
  { "en": "Apa Itu Penyerbukan Sendiri?", "id": "Serbuk Sari Dari Bunga Sama." },
  { "en": "Apa Itu Penyerbukan Silang?", "id": "Serbuk Sari Dari Bunga Berbeda." },
  { "en": "Siapa Perantara Dalam Proses Penyerbukan?", "id": "Angin, Air, Serangga, Dan Hewan." },
  { "en": "Apa Itu Rantai Makanan Perumput?", "id": "Dimulai Dari Produsen Tumbuhan Hijau." },
  { "en": "Apa Itu Rantai Makanan Detritus?", "id": "Dimulai Dari Sisa Organisme Mati." },
  { "en": "Apa Itu Pemanasan Global?", "id": "Peningkatan Suhu Rata-Rata Atmosfer Bumi." },
  { "en": "Apa Gas Rumah Kaca Utama?", "id": "Gas Karbon Dioksida Atau (CO2)." },
  { "en": "Apa Itu Proses Dekomposisi?", "id": "Proses Penguraian Bahan Organik." },
  { "en": "Apa Itu Detritivor?", "id": "Organisme Pemakan Sisa Organisme Mati." },
  { "en": "Apa Contoh Hewan Detritivor?", "id": "Cacing Tanah, Luwing, Dan Teripang." },
  { "en": "Apa Itu Kunci Dikotom?", "id": "Cara Identifikasi Berdasarkan Dua Pilihan." },
  { "en": "Apa Itu Kladistika?", "id": "Metode Klasifikasi Berdasarkan Hubungan Evolusi." },
  { "en": "Apa Nama Ilmiah Padi?", "id": "Oryza Sativa Adalah Nama Ilmiahnya." },
  { "en": "Apa Nama Ilmiah Manusia Modern?", "id": "Homo Sapiens Adalah Nama Ilmiahnya." },
  { "en": "Apa Nama Ilmiah Kucing Domestik?", "id": "Felis Catus Adalah Nama Ilmiahnya." },
  { "en": "Ke Dalam Kingdom Apa Bakteri Termasuk?", "id": "Bakteri Termasuk Dalam Kingdom Monera." },
  { "en": "Apa Itu Fotoperiodisme?", "id": "Respon Tumbuhan Terhadap Lama Penyinaran." },
  { "en": "Apa Fungsi Hormon Giberelin?", "id": "Merangsang Perkecambahan Dan Pemanjangan Batang." },
  { "en": "Apa Fungsi Hormon Sitokinin?", "id": "Merangsang Pembelahan Sel Pada Tumbuhan." },
  { "en": "Apa Fungsi Hormon Asam Absisat?", "id": "Menghambat Pertumbuhan, Menyebabkan Dormansi." },
  { "en": "Apa Fungsi Hormon Etilen?", "id": "Hormon Berbentuk Gas Pematang Buah." },
  { "en": "Apa Itu Gerak Nasti?", "id": "Gerak Tidak Dipengaruhi Arah Rangsang." },
  { "en": "Apa Contoh Gerak Seismonasti?", "id": "Menutupnya Daun Putri Malu Disentuh." },
  { "en": "Apa Itu Gerak Taksis?", "id": "Gerak Pindah Tempat Seluruh Tubuh." },
  { "en": "Apa Itu Replikasi Deoxyribonucleic Acid (DNA)?", "id": "Proses Penggandaan Molekul Deoxyribonucleic Acid." },
  { "en": "Apa Fungsi Enzim Helikase?", "id": "Enzim Pembuka Untai Ganda Deoxyribonucleic Acid." },
  { "en": "Apa Fungsi Deoxyribonucleic Acid Polimerase?", "id": "Enzim Perakit Untai Deoxyribonucleic Acid Baru." },
  { "en": "Apa Itu Messenger Ribonucleic Acid (mRNA)?", "id": "Membawa Kode Genetik Ke Ribosom." },
  { "en": "Apa Itu Transfer Ribonucleic Acid (tRNA)?", "id": "Membawa Asam Amino Ke Ribosom." },
  { "en": "Apa Itu Antikodon?", "id": "Urutan Tiga Basa Transfer Ribonucleic Acid." },
  { "en": "Apa Itu Promoter Dalam Genetika?", "id": "Wilayah Awal Proses Transkripsi Gen." },
  { "en": "Apa Itu Adaptasi Morfologi?", "id": "Penyesuaian Bentuk Tubuh Suatu Organisme." },
  { "en": "Berikan Contoh Adaptasi Morfologi?", "id": "Bentuk Paruh Burung Sesuai Makanan." },
  { "en": "Apa Itu Adaptasi Fisiologi?", "id": "Penyesuaian Fungsi Organ Dalam Tubuh." },
  { "en": "Berikan Contoh Adaptasi Fisiologi?", "id": "Ikan Air Laut Banyak Minum." },
  { "en": "Apa Itu Adaptasi Tingkah Laku?", "id": "Penyesuaian Melalui Perilaku Atau Tindakan." },
  { "en": "Berikan Contoh Adaptasi Tingkah Laku?", "id": "Paus Muncul Ke Permukaan Bernapas." },
  { "en": "Apa Itu Estivasi?", "id": "Keadaan Dormansi Selama Musim Panas." },
  { "en": "Apa Itu Perilaku Teritorial?", "id": "Perilaku Mempertahankan Suatu Wilayah Tertentu." },
  { "en": "Apa Fungsi Kalsium Bagi Tubuh?", "id": "Untuk Kesehatan Tulang Dan Gigi." },
  { "en": "Apa Fungsi Zat Besi?", "id": "Komponen Penting Pembentuk Sel Hemoglobin." },
  { "en": "Apa Fungsi Vitamin A?", "id": "Menjaga Kesehatan Organ Mata." },
  { "en": "Apa Fungsi Vitamin C?", "id": "Meningkatkan Daya Tahan Tubuh Manusia." },
  { "en": "Apa Fungsi Vitamin D?", "id": "Membantu Penyerapan Kalsium Dalam Tubuh." },
  { "en": "Apa Fungsi Vitamin K?", "id": "Membantu Dalam Proses Pembekuan Darah." },
  { "en": "Apa Itu Malnutrisi?", "id": "Ketidakseimbangan Asupan Nutrisi Gizi." },
  { "en": "Penyakit Apa Akibat Kekurangan Vitamin C?", "id": "Penyakit Skorbut Atau Gusi Berdarah." },
  { "en": "Penyakit Apa Akibat Kekurangan Yodium?", "id": "Penyakit Gondok Pada Kelenjar Tiroid." },
  { "en": "Apa Itu Jaringan Epidermis Tumbuhan?", "id": "Jaringan Pelindung Terluar Pada Tumbuhan." },
  { "en": "Apa Itu Kutikula?", "id": "Lapisan Lilin Di Permukaan Epidermis." },
  { "en": "Apa Itu Jaringan Sklerenkim?", "id": "Jaringan Penguat Dengan Dinding Tebal." },
  { "en": "Apa Itu Tulang Kompak?", "id": "Jaringan Tulang Padat Dan Keras." },
  { "en": "Apa Itu Tulang Spons?", "id": "Jaringan Tulang Berongga Di Dalam." },
  { "en": "Apa Itu Sel Goblet?", "id": "Sel Penghasil Lendir Pada Epitel." },
  { "en": "Apa Itu Osteosit?", "id": "Sel Tulang Dewasa Yang Matang." },
  { "en": "Apa Itu Kondrosit?", "id": "Sel Utama Penyusun Jaringan Tulang Rawan." },
  { "en": "Apa Nama Lain Trombosit?", "id": "Keping Darah Untuk Pembekuan Darah." },
  { "en": "Apa Itu Resistensi Antibiotik?", "id": "Kemampuan Bakteri Bertahan Dari Antibiotik." },
  { "en": "Apa Itu Inokulasi?", "id": "Proses Pemindahan Mikroba Ke Media." },
  { "en": "Apa Itu Medium Kultur?", "id": "Tempat Menumbuhkan Mikroba Di Laboratorium." },
  { "en": "Apa Itu Proses Sterilisasi?", "id": "Proses Membunuh Semua Bentuk Mikroorganisme." },
  { "en": "Apa Itu Autoklaf?", "id": "Alat Sterilisasi Menggunakan Uap Panas." },
  { "en": "Apa Itu Fermentasi Asam Laktat?", "id": "Menghasilkan Asam Laktat, Contohnya Yoghurt." },
  { "en": "Apa Itu Fosfolipid?", "id": "Komponen Utama Penyusun Membran Sel." },
  { "en": "Apa Itu Gradien Konsentrasi?", "id": "Perbedaan Konsentrasi Zat Antar Wilayah." },
  { "en": "Apa Itu Transpor Aktif?", "id": "Pergerakan Zat Melawan Gradien Konsentrasi." },
  { "en": "Apa Itu Transpor Pasif?", "id": "Pergerakan Zat Tanpa Memerlukan Energi." },
  { "en": "Apa Itu Potensial Aksi?", "id": "Impuls Listrik Pada Sel Saraf." },
  { "en": "Apa Itu Alel Ganda?", "id": "Lebih Dari Dua Alel Gen." },
  { "en": "Apa Itu Plasmid?", "id": "Struktur Deoxyribonucleic Acid Kecil Bakteri." },
  { "en": "Apa Itu Apoptosis?", "id": "Proses Kematian Sel Terprogram." },
  { "en": "Apa Fungsi Otot Polos?", "id": "Menggerakkan Organ Dalam Secara Otomatis." },
  { "en": "Apa Fungsi Otot Jantung?", "id": "Memompa Darah Ke Seluruh Tubuh." },
  { "en": "Di Mana Otot Lurik Ditemukan?", "id": "Melekat Pada Rangka Atau Tulang." },
  { "en": "Apa Itu Aktin Dan Miosin?", "id": "Protein Penting Dalam Kontraksi Otot." },
  { "en": "Apa Itu Periosteum?", "id": "Membran Luar Yang Melapisi Tulang." },
  { "en": "Apa Itu Sumsum Merah Tulang?", "id": "Tempat Produksi Sel-Sel Darah." },
  { "en": "Apa Itu Empedu?", "id": "Cairan Yang Membantu Pencernaan Lemak." },
  { "en": "Apa Itu Enzim Renin?", "id": "Menggumpalkan Protein Susu Di Lambung." },
  { "en": "Apa Itu Enzim Tripsin?", "id": "Memecah Pepton Menjadi Asam Amino." },
  { "en": "Apa Itu Gerak Peristaltik?", "id": "Gerakan Mendorong Makanan Di Kerongkongan." },
  { "en": "Apa Itu Faring?", "id": "Persimpangan Saluran Pernapasan Dan Pencernaan." },
  { "en": "Apa Itu Epiglotis?", "id": "Katup Pencegah Makanan Masuk Tenggorokan." },
  { "en": "Berapa Jumlah Ruang Jantung Manusia?", "id": "Jantung Manusia Memiliki Empat Ruang." },
  { "en": "Apa Fungsi Kulit Sebagai Organ Ekskresi?", "id": "Mengeluarkan Keringat Yang Mengandung Garam." },
  { "en": "Apa Fungsi Paru-Paru Sebagai Organ Ekskresi?", "id": "Mengeluarkan Karbon Dioksida Dan Uap Air." },
  { "en": "Apa Fungsi Hati Sebagai Organ Ekskresi?", "id": "Menghasilkan Urea Dari Proses Metabolisme." },
  { "en": "Apa Itu Urea?", "id": "Limbah Nitrogen Utama Dalam Urine." },
  { "en": "Apa Itu Urine?", "id": "Cairan Sisa Yang Diekskresikan Ginjal." },
  { "en": "Sebutkan Tiga Proses Pembentukan Urine?", "id": "Filtrasi, Reabsorpsi, Dan Juga Augmentasi." },
  { "en": "Apa Itu Proses Filtrasi?", "id": "Proses Penyaringan Darah Di Glomerulus." },
  { "en": "Apa Itu Proses Reabsorpsi?", "id": "Penyerapan Kembali Zat Yang Berguna." },
  { "en": "Apa Itu Proses Augmentasi?", "id": "Penambahan Zat Sisa Ke Urine." },
  { "en": "Apa Itu Glomerulus?", "id": "Gumpalan Kapiler Di Dalam Ginjal." },
  { "en": "Apa Perbedaan Pertumbuhan Dan Perkembangan?", "id": "Pertumbuhan Kuantitatif, Perkembangan Kualitatif." },
  { "en": "Apa Itu Pertumbuhan Primer Tumbuhan?", "id": "Pertumbuhan Yang Menyebabkan Pertambahan Tinggi." },
  { "en": "Di Mana Pertumbuhan Primer Terjadi?", "id": "Terjadi Di Ujung Akar Batang." },
  { "en": "Apa Itu Pertumbuhan Sekunder Tumbuhan?", "id": "Pertumbuhan Yang Menyebabkan Pelebaran Batang." },
  { "en": "Di Mana Pertumbuhan Sekunder Terjadi?", "id": "Terjadi Karena Aktivitas Jaringan Kambium." },
  { "en": "Apa Itu Sifat Totipotensi?", "id": "Kemampuan Sel Menjadi Individu Baru." },
  { "en": "Apa Itu Diferensiasi Sel?", "id": "Proses Sel Menjadi Lebih Terspesialisasi." },
  { "en": "Apa Itu Jaringan Embrional?", "id": "Jaringan Yang Belum Mengalami Diferensiasi." },
  { "en": "Apa Itu Gerak Sinergis?", "id": "Dua Otot Yang Bekerja Bersamaan." },
  { "en": "Apa Contoh Gerak Antagonis Otot?", "id": "Gerak Otot Bisep Dan Trisep." },
  { "en": "Apa Itu Sendi Peluru?", "id": "Sendi Yang Memungkinkan Gerak Bebas." },
  { "en": "Di Mana Letak Sendi Peluru?", "id": "Di Antara Tulang Paha Panggul." },
  { "en": "Apa Itu Sendi Engsel?", "id": "Sendi Yang Memungkinkan Gerak Satu Arah." },
  { "en": "Di Mana Letak Sendi Engsel?", "id": "Di Antara Lengan Atas Hasta." },
  { "en": "Apa Itu Sendi Putar?", "id": "Memungkinkan Gerak Rotasi Pada Poros." },
  { "en": "Di Mana Letak Sendi Putar?", "id": "Di Antara Tulang Leher Tengkorak." },
  { "en": "Apa Itu Tulang Rawan Hialin?", "id": "Banyak Ditemukan Pada Persendian Tubuh." },
  { "en": "Apa Itu Osifikasi?", "id": "Proses Pembentukan Atau Pengerasan Tulang." },
  { "en": "Apa Itu Zona Fotik?", "id": "Zona Laut Yang Menerima Cahaya." },
  { "en": "Apa Itu Zona Afotik?", "id": "Zona Laut Tidak Menerima Cahaya." },
  { "en": "Apa Itu Terumbu Karang?", "id": "Ekosistem Bawah Laut Dari Koral." },
  { "en": "Apa Itu Zooxanthellae?", "id": "Alga Simbiotik Yang Hidup Koral." },
  { "en": "Apa Itu Upwelling?", "id": "Peristiwa Naiknya Massa Air Laut." },
  { "en": "Apa Itu Nekton?", "id": "Organisme Yang Aktif Berenang Bebas." },
  { "en": "Apa Itu Bentos?", "id": "Organisme Yang Hidup Dasar Perairan." },
  { "en": "Apa Itu Imunitas Aktif?", "id": "Kekebalan Yang Dibuat Tubuh Sendiri." },
  { "en": "Apa Itu Imunitas Pasif?", "id": "Kekebalan Yang Diterima Dari Luar." },
  { "en": "Apa Contoh Imunitas Aktif Alami?", "id": "Sembuh Dari Sakit Infeksi Campak." },
  { "en": "Apa Contoh Imunitas Pasif Alami?", "id": "Antibodi Dari Ibu Ke Bayi." },
  { "en": "Apa Itu Sel T?", "id": "Jenis Sel Darah Putih Limfosit." },
  { "en": "Apa Itu Sel B?", "id": "Sel Limfosit Penghasil Antibodi Spesifik." },
  { "en": "Apa Itu Antigen?", "id": "Zat Yang Memicu Respon Imun." },
  { "en": "Apa Fungsi Utama Antibodi?", "id": "Protein Yang Melawan Antigen Spesifik." },
  { "en": "Apa Itu Fagosit?", "id": "Sel Yang Menelan Benda Asing." },
  { "en": "Apa Itu Peradangan (Inflamasi)?", "id": "Respon Tubuh Terhadap Cedera Infeksi." },
  { "en": "Apa Itu Kapsid?", "id": "Mantel Protein Pelindung Materi Genetik." },
  { "en": "Apa Itu Siklus Litik?", "id": "Siklus Reproduksi Virus Hancurkan Sel." },
  { "en": "Apa Itu Siklus Lisogenik?", "id": "Virus Berintegrasi Dengan Deoxyribonucleic Acid Inang." },
  { "en": "Apa Itu Bakteriofag?", "id": "Virus Yang Hanya Menginfeksi Bakteri." },
  { "en": "Apa Itu Pewarnaan Gram?", "id": "Metode Membedakan Jenis Dinding Bakteri." },
  { "en": "Apa Ciri Bakteri Gram-Positif?", "id": "Bakteri Dengan Dinding Peptidoglikan Tebal." },
  { "en": "Apa Ciri Bakteri Gram-Negatif?", "id": "Bakteri Dengan Dinding Peptidoglikan Tipis." },
  { "en": "Apa Itu Bakteri Aerob?", "id": "Bakteri Yang Membutuhkan Oksigen." },
  { "en": "Apa Itu Bakteri Anaerob?", "id": "Bakteri Tidak Membutuhkan Oksigen." },
  { "en": "Apa Itu Proses Glikolisis?", "id": "Pemecahan Glukosa Menjadi Asam Piruvat." },
  { "en": "Di Mana Proses Glikolisis Terjadi?", "id": "Terjadi Di Dalam Sitoplasma Sel." },
  { "en": "Apa Itu Siklus Krebs?", "id": "Tahap Respirasi Seluler Setelah Glikolisis." },
  { "en": "Di Mana Siklus Krebs Berlangsung?", "id": "Terjadi Di Matriks Dalam Mitokondria." },
  { "en": "Apa Itu Fosforilasi Oksidatif?", "id": "Tahap Akhir Penghasil Adenosine Triphosphate (ATP)." },
  { "en": "Apa Itu Koenzim?", "id": "Molekul Non-Protein Pembantu Kerja Enzim." },
  { "en": "Apa Itu Substrat Enzim?", "id": "Molekul Yang Bereaksi Dengan Enzim." },
  { "en": "Apa Itu Sisi Aktif Enzim?", "id": "Tempat Substrat Menempel Pada Enzim." },
  { "en": "Apa Faktor Yang Mempengaruhi Kerja Enzim?", "id": "Suhu, pH, Konsentrasi, Dan Inhibitor." },
  { "en": "Apa Itu Inhibitor Enzim?", "id": "Zat Yang Dapat Menghambat Kerja Enzim." },
  { "en": "Apa Itu Organisme Transgenik?", "id": "Organisme Yang Mengandung Gen Asing." },
  { "en": "Apa Itu Elektroforesis Gel?", "id": "Teknik Memisahkan Molekul Berdasarkan Ukuran." },
  { "en": "Apa Itu Sidik Jari Deoxyribonucleic Acid?", "id": "Pola Unik Deoxyribonucleic Acid Setiap Individu." },
  { "en": "Apa Itu Sel Punca (Stem Cell)?", "id": "Sel Yang Dapat Berdiferensiasi." },
  { "en": "Apa Itu Ritme Sirkadian?", "id": "Siklus Biologis Sekitar 24 Jam." },
  { "en": "Apa Itu Bioinformatika?", "id": "Penerapan Komputasi Untuk Data Biologi." },
  { "en": "Apa Itu Biomassa?", "id": "Total Massa Organisme Di Area." },
  { "en": "Apa Itu Biodiversitas?", "id": "Singkatan Dari Keanekaragaman Hayati." },
  { "en": "Apa Itu Ekoton?", "id": "Zona Transisi Antara Dua Komunitas." },
  { "en": "Apa Itu Efek Tepi?", "id": "Peningkatan Keanekaragaman Hayati Di Ekoton." },
  { "en": "Apa Itu Spesies Kunci (Keystone Species)?", "id": "Spesies Berpengaruh Besar Pada Ekosistem." },
  { "en": "Apa Itu Hewan Hermafrodit?", "id": "Individu Dengan Dua Alat Kelamin." },
  { "en": "Sebutkan Contoh Hewan Hermafrodit?", "id": "Cacing Tanah Dan Juga Siput." },
  { "en": "Apa Itu Amnion?", "id": "Selaput Pelindung Embrio Dalam Kandungan." },
  { "en": "Apa Fungsi Cairan Amnion?", "id": "Melindungi Janin Dari Guncangan Fisik." },
  { "en": "Apa Itu Aorta?", "id": "Arteri Terbesar Dalam Tubuh Manusia." },
  { "en": "Apa Itu Eritrosit?", "id": "Nama Lain Untuk Sel Darah Merah." },
  { "en": "Apa Itu Leukosit?", "id": "Nama Lain Untuk Sel Darah Putih." },
  { "en": "Apa Itu Sistem Saraf Otonom?", "id": "Mengontrol Fungsi Tubuh Tanpa Disadari." },
  { "en": "Apa Itu Sistem Saraf Somatik?", "id": "Mengontrol Gerakan Tubuh Secara Sadar." },
  { "en": "Apa Itu Ganglion?", "id": "Kumpulan Badan Sel Saraf." },
  { "en": "Apa Itu Akson?", "id": "Juluran Saraf Pembawa Impuls." },
  { "en": "Apa Itu Dendrit?", "id": "Juluran Saraf Penerima Impuls." },
  { "en": "Apa Itu Selubung Mielin?", "id": "Lapisan Lemak Pelindung Serabut Akson." },
  { "en": "Apa Fungsi Selubung Mielin?", "id": "Mempercepat Jalannya Impuls Saraf." },
  { "en": "Apa Itu Nodus Ranvier?", "id": "Celah Pada Selubung Mielin." },
  { "en": "Apa Itu Kelenjar Adrenal?", "id": "Kelenjar Di Atas Organ Ginjal." },
  { "en": "Hormon Apa Yang Dihasilkan Kelenjar Adrenal?", "id": "Hormon Adrenalin Dan Juga Kortisol." },
  { "en": "Apa Fungsi Hormon Adrenalin?", "id": "Meningkatkan Detak Jantung Tekanan Darah." },
  { "en": "Apa Itu Pilorus?", "id": "Katup Antara Lambung Usus Halus." },
  { "en": "Apa Itu Vili Usus?", "id": "Jonjot Usus Untuk Penyerapan Nutrisi." },
  { "en": "Apa Itu Bakteri Baik?", "id": "Bakteri Yang Bermanfaat Bagi Kesehatan." },
  { "en": "Apa Itu Flora Normal?", "id": "Mikroorganisme Yang Hidup Normal Tubuh." },
  { "en": "Apa Itu Cawan Petri?", "id": "Wadah Kaca Untuk Kultur Mikroba." },
  { "en": "Apa Itu Getah Bening (Limfa)?", "id": "Cairan Yang Mengandung Sel Darah Putih." },
  { "en": "Apa Fungsi Kelenjar Getah Bening?", "id": "Menyaring Limfa Dan Melawan Infeksi." },
  { "en": "Apa Organ Limfatik Terbesar Manusia?", "id": "Limpa Adalah Organ Limfatik Terbesar." },
  { "en": "Apa Fungsi Organ Limpa?", "id": "Menyaring Darah Dan Menyimpan Sel Darah." },
  { "en": "Apa Itu Pembekuan Darah (Koagulasi)?", "id": "Proses Menghentikan Pendarahan Pada Luka." },
  { "en": "Protein Apa Untuk Pembekuan Darah?", "id": "Fibrinogen Yang Menjadi Benang Fibrin." },
  { "en": "Apa Itu Serum Darah?", "id": "Plasma Darah Tanpa Faktor Pembekuan." },
  { "en": "Apa Itu Denyut Nadi?", "id": "Gelombang Tekanan Darah Di Arteri." },
  { "en": "Apa Itu Proses Fotolisis?", "id": "Proses Pemecahan Molekul Air Cahaya." },
  { "en": "Apa Hasil Dari Proses Fotolisis?", "id": "Oksigen, Proton, Dan Juga Elektron." },
  { "en": "Di Mana Reaksi Terang Fotosintesis Terjadi?", "id": "Terjadi Di Dalam Membran Tilakoid." },
  { "en": "Di Mana Reaksi Gelap Fotosintesis Terjadi?", "id": "Terjadi Di Dalam Stroma Kloroplas." },
  { "en": "Apa Nama Lain Dari Reaksi Gelap?", "id": "Siklus Calvin Atau Siklus Calvin-Benson." },
  { "en": "Apa Itu Respirasi Aerob?", "id": "Respirasi Yang Membutuhkan Oksigen Bebas." },
  { "en": "Apa Itu Respirasi Anaerob?", "id": "Respirasi Yang Tidak Membutuhkan Oksigen." },
  { "en": "Apa Produk Akhir Respirasi Aerob?", "id": "Karbon Dioksida, Air, Dan Energi." },
  { "en": "Berapa Adenosine Triphosphate Dihasilkan Respirasi Aerob?", "id": "Sekitar Tiga Puluh Enam Adenosine Triphosphate." },
  { "en": "Apa Itu Endosperma?", "id": "Jaringan Penyimpan Cadangan Makanan Biji." },
  { "en": "Apa Itu Epikotil?", "id": "Bagian Batang Embrio Di Atas Kotiledon." },
  { "en": "Apa Itu Hipokotil?", "id": "Bagian Batang Embrio Bawah Kotiledon." },
  { "en": "Apa Itu Tudung Akar?", "id": "Struktur Pelindung Ujung Akar Tumbuhan." },
  { "en": "Apa Fungsi Rambut Akar?", "id": "Memperluas Bidang Penyerapan Air Mineral." },
  { "en": "Apa Itu Lentisel?", "id": "Pori-Pori Batang Untuk Pertukaran Gas." },
  { "en": "Apa Itu Lingkaran Tahun?", "id": "Pola Pertumbuhan Sekunder Pada Batang." },
  { "en": "Apa Itu Pergiliran Keturunan (Metagenesis)?", "id": "Siklus Hidup Antara Fase Generatif." },
  { "en": "Apa Itu Lungkang Gen (Gene Pool)?", "id": "Total Kumpulan Gen Dalam Populasi." },
  { "en": "Apa Itu Frekuensi Alel?", "id": "Proporsi Alel Tertentu Dalam Populasi." },
  { "en": "Apa Itu Asas Hardy-Weinberg?", "id": "Prinsip Frekuensi Alel Tetap Konstan." },
  { "en": "Apa Itu Efek Leher Botol?", "id": "Pengurangan Drastis Ukuran Populasi." },
  { "en": "Apa Itu Efek Pendiri?", "id": "Koloni Baru Oleh Sedikit Individu." },
  { "en": "Apa Itu Feromon?", "id": "Sinyal Kimia Untuk Komunikasi Hewan." },
  { "en": "Apa Itu Altruisme Dalam Biologi?", "id": "Perilaku Merugikan Diri, Menguntungkan Lain." },
  { "en": "Apa Itu Habituasi?", "id": "Hilangnya Respon Terhadap Rangsang Berulang." },
  { "en": "Apa Itu Imprinting (Penanaman)?", "id": "Proses Belajar Tahap Awal Kehidupan." },
  { "en": "Siapa Yang Terkenal Studi Imprinting?", "id": "Konrad Lorenz Dengan Anak Angsa." },
  { "en": "Apa Itu Pengkondisian Klasik?", "id": "Belajar Melalui Asosiasi Dua Rangsangan." },
  { "en": "Siapa Pelopor Teori Pengkondisian Klasik?", "id": "Ivan Pavlov Dengan Eksperimen Anjingnya." },
  { "en": "Apa Itu Bakteri Patogen?", "id": "Bakteri Yang Dapat Menyebabkan Penyakit." },
  { "en": "Apa Itu Vektor Penyakit?", "id": "Organisme Pembawa Dan Penyebar Patogen." },
  { "en": "Apa Contoh Vektor Penyakit?", "id": "Nyamuk, Lalat, Dan Juga Tikus." },
  { "en": "Apa Itu Epidemi?", "id": "Penyebaran Penyakit Cepat Di Wilayah." },
  { "en": "Apa Itu Pandemi?", "id": "Epidemi Yang Menyebar Seluruh Dunia." },
  { "en": "Apa Itu Endemi?", "id": "Penyakit Yang Selalu Ada Wilayah." },
  { "en": "Apa Itu Masa Inkubasi?", "id": "Waktu Dari Infeksi Hingga Gejala." },
  { "en": "Apa Itu Penyakit Tuberkulosis (TBC)?", "id": "Penyakit Infeksi Bakteri Paru-Paru." },
  { "en": "Apa Bakteri Penyebab Penyakit Tuberkulosis?", "id": "Bakteri Bernama Mycobacterium Tuberculosis." },
  { "en": "Apa Itu Penyakit Malaria?", "id": "Penyakit Akibat Parasit Protozoa Plasmodium." },
  { "en": "Apa Itu Penyakit Tetanus?", "id": "Infeksi Bakteri Yang Menyerang Saraf." },
  { "en": "Apa Itu Fragmentasi Habitat?", "id": "Terpecahnya Habitat Menjadi Bagian Kecil." },
  { "en": "Apa Itu Koridor Hidupan Liar?", "id": "Jalur Penghubung Fragmen-Fragmen Habitat." },
  { "en": "Apa Itu Spesies Invasif?", "id": "Spesies Asing Yang Merusak Ekosistem." },
  { "en": "Apa Itu Titik Panas Keanekaragaman Hayati?", "id": "Wilayah Biodiversitas Tinggi Yang Terancam." },
  { "en": "Apa Itu Reintroduksi Spesies?", "id": "Upaya Mengembalikan Spesies Ke Habitatnya." },
  { "en": "Apa Itu CITES (Convention on International Trade)?", "id": "Perjanjian Internasional Perdagangan Spesies Liar." },
  { "en": "Apa Itu Hewan Ovipar?", "id": "Hewan Yang Berkembang Biak Bertelur." },
  { "en": "Apa Itu Hewan Vivipar?", "id": "Hewan Yang Berkembang Biak Melahirkan." },
  { "en": "Apa Itu Hewan Ovovivipar?", "id": "Bertelur, Telur Menetas Dalam Tubuh." },
  { "en": "Apa Contoh Hewan Ovipar?", "id": "Ayam, Ikan, Katak, Dan Penyu." },
  { "en": "Apa Contoh Hewan Vivipar?", "id": "Manusia, Kucing, Sapi, Dan Paus." },
  { "en": "Apa Contoh Hewan Ovovivipar?", "id": "Ular Kadut Dan Ikan Hiu." },
  { "en": "Apa Itu Hewan Berdarah Panas (Homoiterm)?", "id": "Suhu Tubuh Relatif Konstan." },
  { "en": "Apa Itu Hewan Berdarah Dingin (Poikiloterm)?", "id": "Suhu Tubuh Berubah Sesuai Lingkungan." },
  { "en": "Apa Itu Metamorfosis Tidak Sempurna?", "id": "Hanya Melewati Tiga Tahap Perkembangan." },
  { "en": "Apa Tahap Metamorfosis Tidak Sempurna?", "id": "Tahap Telur, Nimfa, Dan Imago." },
  { "en": "Contoh Hewan Metamorfosis Tidak Sempurna?", "id": "Belalang, Capung, Dan Juga Kecoa." },
  { "en": "Apa Itu Kambium Vaskular?", "id": "Menghasilkan Xilem Dan Floem Sekunder." },
  { "en": "Apa Itu Kambium Gabus (Felogen)?", "id": "Menghasilkan Lapisan Pelindung Yaitu Gabus." },
  { "en": "Apa Itu Sel Penjaga?", "id": "Dua Sel Yang Mengelilingi Stomata." },
  { "en": "Apa Fungsi Sel Penjaga?", "id": "Mengatur Membuka Dan Menutupnya Stomata." },
  { "en": "Apa Itu Jaringan Palisade?", "id": "Tempat Utama Terjadinya Proses Fotosintesis." },
  { "en": "Apa Itu Jaringan Spons (Bunga Karang)?", "id": "Jaringan Berongga Untuk Menyimpan Gas." },
  { "en": "Apa Itu Proses Gutasi?", "id": "Keluarnya Tetesan Air Tepi Daun." },
  { "en": "Kapan Gutasi Biasanya Terjadi?", "id": "Terjadi Pada Malam Atau Pagi." },
  { "en": "Apa Itu Hidatoda?", "id": "Struktur Tempat Terjadinya Proses Gutasi." },
  { "en": "Apa Itu Mikrotom?", "id": "Alat Untuk Mengiris Spesimen Tipis." },
  { "en": "Apa Fungsi Inkubator Laboratorium?", "id": "Alat Untuk Menjaga Suhu Optimal." },
  { "en": "Apa Fungsi Alat Sentrifus?", "id": "Alat Pemisah Partikel Berdasarkan Massa." },
  { "en": "Apa Fungsi pH Meter?", "id": "Alat Pengukur Tingkat Keasaman Larutan." },
  { "en": "Apa Itu Teknik Kromatografi?", "id": "Teknik Pemisahan Campuran Menjadi Komponen." },
  { "en": "Apa Itu Kariokinesis?", "id": "Proses Pembelahan Inti Sel (Nukleus)." },
  { "en": "Apa Itu Sitokinesis?", "id": "Proses Pembelahan Sitoplasma Suatu Sel." },
  { "en": "Apa Itu Tahap Profase?", "id": "Tahap Pertama Pembelahan Sel Mitosis." },
  { "en": "Apa Itu Tahap Metafase?", "id": "Kromosom Berjajar Di Bidang Ekuator." },
  { "en": "Apa Itu Tahap Anafase?", "id": "Kromatid Bergerak Ke Kutub Berlawanan." },
  { "en": "Apa Itu Tahap Telofase?", "id": "Tahap Akhir Dari Pembelahan Mitosis." },
  { "en": "Apa Itu Benang Spindel?", "id": "Struktur Pembelahan Sel Tarik Kromosom." },
  { "en": "Apa Itu Sentromer?", "id": "Bagian Tengah Kromosom Tempat Melekat." },
  { "en": "Apa Itu Kromatid?", "id": "Salah Satu Lengan Kromosom Hasil." },
  { "en": "Apa Beda Mitosis Dan Meiosis?", "id": "Mitosis Sel Tubuh, Meiosis Kelamin." },
  { "en": "Apa Tujuan Pembelahan Mitosis?", "id": "Untuk Pertumbuhan Dan Proses Regenerasi." },
  { "en": "Apa Tujuan Pembelahan Meiosis?", "id": "Mengurangi Jumlah Kromosom Sel Gamet." },
  { "en": "Berapa Sel Anak Dihasilkan Meiosis?", "id": "Menghasilkan Empat Sel Anak Haploid." },
  { "en": "Berapa Sel Anak Dihasilkan Mitosis?", "id": "Menghasilkan Dua Sel Anak Diploid." },
  { "en": "Di Mana Pembelahan Meiosis Terjadi?", "id": "Terjadi Di Dalam Organ Reproduksi." },
  { "en": "Apa Itu Zigot?", "id": "Sel Hasil Fertilisasi Sperma Ovum." },
  { "en": "Apa Itu Gastrulasi?", "id": "Proses Pembentukan Lapisan Germinal Embrio." },
  { "en": "Apa Tiga Lapisan Germinal Utama?", "id": "Ektoderm, Mesoderm, Dan Juga Endoderm." },
  { "en": "Apa Yang Dibentuk Oleh Lapisan Ektoderm?", "id": "Kulit, Sistem Saraf, Dan Indera." },
  { "en": "Apa Yang Dibentuk Oleh Lapisan Mesoderm?", "id": "Otot, Tulang, Sistem Peredaran Darah." },
  { "en": "Apa Yang Dibentuk Oleh Lapisan Endoderm?", "id": "Sistem Pencernaan Dan Sistem Pernapasan." },
  { "en": "Apa Itu Tahap Morula?", "id": "Tahap Awal Embrio, Bola Padat." },
  { "en": "Apa Itu Tahap Blastula?", "id": "Tahap Embrio Berongga Setelah Morula." },
  { "en": "Apa Itu Proses Organogenesis?", "id": "Proses Pembentukan Organ Pada Embrio." },
  { "en": "Apa Fungsi Tulang Tengkorak?", "id": "Tulang Yang Melindungi Organ Otak." },
  { "en": "Apa Itu Tulang Belakang (Vertebra)?", "id": "Ruas Tulang Penyusun Tulang Belakang." },
  { "en": "Apa Itu Sendi Mati (Sinartrosis)?", "id": "Hubungan Antar Tulang Tanpa Gerak." },
  { "en": "Apa Contoh Sendi Mati?", "id": "Hubungan Antar Tulang Pada Tengkorak." },
  { "en": "Apa Itu Sendi Kaku (Amfiartrosis)?", "id": "Hubungan Tulang Dengan Gerakan Terbatas." },
  { "en": "Apa Contoh Sendi Kaku?", "id": "Hubungan Antar Ruas Tulang Belakang." },
  { "en": "Apa Penyebab Kelelahan Pada Otot?", "id": "Penumpukan Asam Laktat Pada Otot." },
  { "en": "Apa Itu Atrofi Otot?", "id": "Penyusutan Massa Atau Ukuran Otot." },
  { "en": "Bagaimana Ikan Air Tawar Berosmoregulasi?", "id": "Tidak Minum, Urine Sangat Encer." },
  { "en": "Bagaimana Ikan Air Laut Berosmoregulasi?", "id": "Banyak Minum, Urine Sangat Pekat." },
  { "en": "Apa Itu Kelenjar Eksokrin?", "id": "Kelenjar Yang Mengeluarkan Produknya." },
  { "en": "Apa Contoh Kelenjar Eksokrin?", "id": "Kelenjar Keringat Dan Kelenjar Ludah." },
  { "en": "Apa Itu Deoxyribonucleic Acid Rekombinan?", "id": "Molekul Deoxyribonucleic Acid Gabungan Gen Berbeda." },
  { "en": "Apa Itu Vektor Rekayasa Genetika?", "id": "Pembawa Gen Ke Dalam Sel Inang." },
  { "en": "Apa Contoh Vektor Yang Umum Digunakan?", "id": "Plasmid Dari Bakteri Atau Virus." },
  { "en": "Apa Itu Insulin Manusia Rekombinan?", "id": "Insulin Yang Dibuat Oleh Bakteri." },
  { "en": "Apa Itu Antibodi Monoklonal?", "id": "Antibodi Identik Dari Satu Sel." },
  { "en": "Apa Itu Divisi Bryophyta?", "id": "Kelompok Tumbuhan Lumut Tidak Berpembuluh." },
  { "en": "Apa Itu Divisi Pteridophyta?", "id": "Kelompok Tumbuhan Paku Memiliki Pembuluh." },
  { "en": "Apa Itu Divisi Spermatophyta?", "id": "Kelompok Tumbuhan Berbiji Yang Dominan." },
  { "en": "Apa Ciri Utama Kelas Insecta?", "id": "Memiliki Tiga Pasang Kaki Beruas." },
  { "en": "Apa Ciri Utama Kelas Arachnida?", "id": "Memiliki Empat Pasang Kaki Beruas." },
  { "en": "Apa Ciri Utama Kelas Aves?", "id": "Kelompok Hewan Burung Yang Berbulu." },
  { "en": "Apa Ciri Utama Kelas Pisces?", "id": "Ikan Yang Bernapas Dengan Insang." },
  { "en": "Apa Ciri Utama Kelas Reptilia?", "id": "Hewan Melata Dengan Kulit Bersisik." },
  { "en": "Apa Ciri Utama Kelas Amphibia?", "id": "Hewan Yang Hidup Di Dua Alam." },
  { "en": "Apa Itu Fase Gametofit?", "id": "Fase Haploid Yang Menghasilkan Gamet." },
  { "en": "Apa Itu Fase Sporofit?", "id": "Fase Diploid Yang Menghasilkan Spora." },
  { "en": "Fase Mana Yang Dominan Pada Lumut?", "id": "Fase Gametofit Adalah Fase Dominan." },
  { "en": "Fase Mana Yang Dominan Pada Paku?", "id": "Fase Sporofit Adalah Fase Dominan." },
  { "en": "Apa Itu Spora?", "id": "Sel Reproduksi Aseksual Tumbuhan." },
  { "en": "Apa Itu Anteridium?", "id": "Struktur Penghasil Gamet Jantan." },
  { "en": "Apa Itu Arkegonium?", "id": "Struktur Penghasil Gamet Betina." },
  { "en": "Apa Itu Protonema?", "id": "Tahap Awal Gametofit Pada Lumut." },
  { "en": "Apa Itu Hifa?", "id": "Filamen Penyusun Tubuh Fungi Jamur." },
  { "en": "Apa Itu Jamur Saprofit?", "id": "Jamur Yang Mengurai Bahan Organik." },
  { "en": "Apa Itu Jamur Parasit?", "id": "Jamur Yang Hidup Menumpang Inang." },
  { "en": "Apa Itu Jamur Simbiosis?", "id": "Jamur Yang Hidup Bersama Organisme." },
  { "en": "Apa Itu Zigospora?", "id": "Spora Seksual Pada Jamur Zygomycota." },
  { "en": "Apa Itu Askospora?", "id": "Spora Seksual Pada Jamur Ascomycota." },
  { "en": "Apa Itu Basidiospora?", "id": "Spora Seksual Pada Jamur Basidiomycota." },
  { "en": "Jamur Apa Untuk Membuat Tempe?", "id": "Jamur Rhizopus Oryzae." },
  { "en": "Jamur Apa Untuk Membuat Roti?", "id": "Jamur Saccharomyces Cerevisiae Atau Ragi." },
  { "en": "Apa Itu Kingdom Protista?", "id": "Eukariota Bukan Hewan, Tumbuhan, Jamur." },
  { "en": "Bagaimana Protista Dikelompokkan?", "id": "Mirip Hewan, Tumbuhan, Dan Jamur." },
  { "en": "Apa Itu Protista Mirip Hewan (Protozoa)?", "id": "Protista Heterotrof Yang Dapat Bergerak." },
  { "en": "Apa Itu Protista Mirip Tumbuhan (Alga)?", "id": "Protista Autotrof Yang Melakukan Fotosintesis." },
  { "en": "Apa Itu Protista Mirip Jamur?", "id": "Protista Heterotrof Bertindak Sebagai Pengurai." },
  { "en": "Apa Contoh Dari Protozoa?", "id": "Amoeba, Paramecium, Dan Juga Plasmodium." },
  { "en": "Apa Alat Gerak Paramecium?", "id": "Silia Atau Disebut Rambut Getar." },
  { "en": "Apa Alat Gerak Euglena?", "id": "Flagela Atau Disebut Bulu Cambuk." },
  { "en": "Apa Penyebab Penyakit Tidur?", "id": "Trypanosoma, Sejenis Protozoa Berflagela." },
  { "en": "Apa Itu Tekanan Turgor?", "id": "Tekanan Cairan Sel Ke Dinding." },
  { "en": "Apa Itu Plasmolisis?", "id": "Lepasnya Membran Sel Dari Dinding." },
  { "en": "Kapan Plasmolisis Dapat Terjadi?", "id": "Saat Sel Di Larutan Hipertonik." },
  { "en": "Apa Itu Larutan Hipotonik?", "id": "Konsentrasi Zat Terlarut Lebih Rendah." },
  { "en": "Apa Itu Larutan Isotonik?", "id": "Konsentrasi Zat Terlarut Sama Persis." },
  { "en": "Apa Itu Larutan Hipertonik?", "id": "Konsentrasi Zat Terlarut Lebih Tinggi." },
  { "en": "Apa Itu Gen Letal?", "id": "Gen Yang Menyebabkan Kematian Individu." },
  { "en": "Apa Itu Tautan Gen (Gene Linkage)?", "id": "Kecenderungan Alel Diwariskan Secara Bersama." },
  { "en": "Apa Itu Determinasi Seks?", "id": "Mekanisme Biologis Penentuan Jenis Kelamin." },
  { "en": "Apa Itu Fase Interfase?", "id": "Fase Istirahat Dalam Siklus Sel." },
  { "en": "Apa Yang Terjadi Selama Interfase?", "id": "Pertumbuhan Sel Dan Replikasi Deoxyribonucleic Acid." },
  { "en": "Apa Itu G1, S, Dan G2?", "id": "Tiga Tahapan Dalam Siklus Interfase." },
  { "en": "Apa Yang Terjadi Pada Fase S?", "id": "Terjadi Sintesis Atau Replikasi Deoxyribonucleic Acid." },
  { "en": "Apa Itu Sitokrom?", "id": "Protein Pembawa Elektron Dalam Respirasi." },
  { "en": "Apa Itu NAD+ Dan FAD?", "id": "Koenzim Penting Pembawa Elektron Energi." },
  { "en": "Apa Peran Oksigen Dalam Respirasi Sel?", "id": "Sebagai Penerima Elektron Paling Akhir." },
  { "en": "Apa Itu Proses Kemosintesis?", "id": "Sintesis Energi Dari Reaksi Kimia." },
  { "en": "Siapa Yang Melakukan Proses Kemosintesis?", "id": "Bakteri Tertentu, Contohnya Bakteri Sulfur." },
  { "en": "Apa Itu Rantai Transpor Elektron?", "id": "Seri Kompleks Protein Pemindah Elektron." },
  { "en": "Di Mana Rantai Transpor Elektron Berada?", "id": "Di Membran Dalam Organel Mitokondria." },
  { "en": "Apa Itu Ruang Antar Membran Mitokondria?", "id": "Ruang Antara Membran Luar Dalam." },
  { "en": "Apa Itu ATP Sintase?", "id": "Enzim Yang Mensintesis Adenosine Triphosphate (ATP)." },
  { "en": "Apa Itu Kemiosmosis?", "id": "Pergerakan Ion Melintasi Membran Semipermeabel." },
  { "en": "Apa Itu Rubisco?", "id": "Enzim Kunci Dalam Siklus Calvin." },
  { "en": "Apa Fungsi Enzim Rubisco?", "id": "Mengikat Karbon Dioksida Pada Fotosintesis." },
  { "en": "Apa Itu Tumbuhan C3?", "id": "Tumbuhan Yang Menghasilkan Molekul Karbon." },
  { "en": "Apa Itu Tumbuhan C4?", "id": "Tumbuhan Dengan Jalur Fiksasi Karbon." },
  { "en": "Apa Contoh Tumbuhan C4?", "id": "Tebu, Jagung, Dan Juga Sorgum." },
  { "en": "Apa Itu Tumbuhan CAM (Crassulacean Acid)?", "id": "Tumbuhan Yang Membuka Stomata Malam." },
  { "en": "Apa Contoh Tumbuhan CAM?", "id": "Kaktus, Nanas, Dan Juga Agave." },
  { "en": "Apa Itu Titik Kompensasi Cahaya?", "id": "Laju Fotosintesis Sama Dengan Respirasi." },
  { "en": "Apa Itu Peta Genetik?", "id": "Diagram Posisi Relatif Gen." },
  { "en": "Apa Itu Penyakit Huntington?", "id": "Penyakit Genetik Kerusakan Sel Saraf." },
  { "en": "Apa Itu Sindrom Down?", "id": "Kelainan Genetik Akibat Kromosom 21." },
  { "en": "Apa Itu Sindrom Turner?", "id": "Kelainan Genetik Wanita Kurang Kromosom." },
  { "en": "Apa Itu Sindrom Klinefelter?", "id": "Kelainan Genetik Pria Kelebihan Kromosom." },
  { "en": "Apa Itu Sel Sabit Anemia?", "id": "Penyakit Genetik Bentuk Sel Darah." },
  { "en": "Apa Itu Intron?", "id": "Segmen Deoxyribonucleic Acid Tidak Mengkode Protein." },
  { "en": "Apa Itu Ekson?", "id": "Segmen Deoxyribonucleic Acid Yang Mengkode Protein." },
  { "en": "Apa Itu Splicing Ribonucleic Acid (RNA)?", "id": "Proses Pemotongan Intron Dari Ribonucleic Acid." },
  { "en": "Apa Itu Ilmu Genomik?", "id": "Studi Tentang Keseluruhan Genom Organisme." },
  { "en": "Apa Itu Ilmu Proteomik?", "id": "Studi Tentang Keseluruhan Set Protein." },
  { "en": "Apa Itu Telomer?", "id": "Ujung Pelindung Pada Kromosom Eukariotik." },
  { "en": "Apa Fungsi Utama Telomer?", "id": "Melindungi Kromosom Dari Kerusakan Degradasi." },
  { "en": "Apa Itu Protein Histon?", "id": "Protein Tempat Deoxyribonucleic Acid Melilit." },
  { "en": "Apa Itu Nukleosom?", "id": "Unit Dasar Kromatin Pada Eukariota." },
  { "en": "Apa Itu Epigenetik?", "id": "Studi Perubahan Ekspresi Gen." },
  { "en": "Apa Itu Bintik Buta (Blind Spot)?", "id": "Area Retina Tanpa Sel Fotoreseptor." },
  { "en": "Apa Itu Saraf Optik?", "id": "Saraf Pengirim Sinyal Visual Otak." },
  { "en": "Apa Itu Saluran Eustachius?", "id": "Saluran Penghubung Telinga Tengah Faring." },
  { "en": "Apa Fungsi Saluran Eustachius?", "id": "Menyeimbangkan Tekanan Udara Telinga Tengah." },
  { "en": "Apa Tiga Tulang Pendengaran Tengah?", "id": "Tulang Sanggurdi, Martil, Dan Landasan." },
  { "en": "Apa Itu Papila Lidah?", "id": "Tonjolan Lidah Tempat Kuncup Pengecap." },
  { "en": "Apa Empat Rasa Dasar Utama?", "id": "Manis, Asam, Asin, Dan Pahit." },
  { "en": "Rasa Dasar Apa Yang Kelima?", "id": "Rasa Gurih Atau Dikenal Umami." },
  { "en": "Apa Itu Sel Olfaktori?", "id": "Reseptor Pendeteksi Bau Di Hidung." },
  { "en": "Apa Itu Kelenjar Paratiroid?", "id": "Kelenjar Penghasil Hormon Paratiroid." },
  { "en": "Apa Fungsi Hormon Paratiroid?", "id": "Mengatur Kadar Kalsium Dalam Darah." },
  { "en": "Apa Fungsi Kelenjar Timus?", "id": "Berperan Dalam Pematangan Sel Limfosit." },
  { "en": "Apa Fungsi Kelenjar Pineal?", "id": "Menghasilkan Hormon Melatonin Untuk Tidur." },
  { "en": "Apa Fungsi Hormon Melatonin?", "id": "Mengatur Siklus Tidur Dan Bangun." },
  { "en": "Apa Fungsi Hormon Glukagon?", "id": "Meningkatkan Kadar Gula Dalam Darah." },
  { "en": "Apa Fungsi Hormon Oksitosin?", "id": "Merangsang Kontraksi Rahim Saat Melahirkan." },
  { "en": "Apa Itu Hormon Antidiuretik (ADH)?", "id": "Mengontrol Penyerapan Air Pada Ginjal." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Mekanisme Kontrol Hormon Yang Umum." },
  { "en": "Apa Itu Tekanan Akar?", "id": "Gaya Pendorong Air Naik Ke Batang." },
  { "en": "Apa Itu Daya Kapilaritas Batang?", "id": "Kemampuan Air Naik Melalui Xilem." },
  { "en": "Apa Itu Daya Isap Daun?", "id": "Gaya Tarik Akibat Proses Transpirasi." },
  { "en": "Apa Itu Mikronutrien Bagi Tumbuhan?", "id": "Nutrien Yang Dibutuhkan Jumlah Sedikit." },
  { "en": "Apa Contoh Mikronutrien Tumbuhan?", "id": "Besi, Mangan, Seng, Dan Tembaga." },
  { "en": "Apa Itu Makronutrien Bagi Tumbuhan?", "id": "Nutrien Yang Dibutuhkan Jumlah Banyak." },
  { "en": "Apa Contoh Makronutrien Tumbuhan?", "id": "Nitrogen, Fosfor, Dan Juga Kalium." },
  { "en": "Apa Itu Hidroponik?", "id": "Metode Menanam Tanpa Menggunakan Tanah." },
  { "en": "Apa Itu Aeroponik?", "id": "Metode Menanam Akar Di Udara." },
  { "en": "Apa Itu Seleksi Kerabat (Kin Selection)?", "id": "Seleksi Alam Yang Mendukung Altruisme." },
  { "en": "Apa Itu Poligini?", "id": "Satu Jantan Kawin Banyak Betina." },
  { "en": "Apa Itu Poliandri?", "id": "Satu Betina Kawin Banyak Jantan." },
  { "en": "Apa Itu Monogami?", "id": "Satu Jantan Kawin Satu Betina." },
  { "en": "Apa Itu Kesetimbangan Bersela?", "id": "Pola Evolusi Cepat Lalu Stagnan." },
  { "en": "Apa Itu Gradualisme?", "id": "Pola Evolusi Lambat Dan Bertahap." },
  { "en": "Apa Itu Asal Usul Kehidupan (Abiogenesis)?", "id": "Proses Kehidupan Muncul Dari Benda." },
  { "en": "Apa Itu Hipotesis Sup Purba?", "id": "Teori Pembentukan Molekul Organik." },
  { "en": "Siapa Penggagas Hipotesis Sup Purba?", "id": "Alexander Oparin Dan J.B.S. Haldane." },
  { "en": "Apa Itu Eksperimen Miller-Urey?", "id": "Simulasi Kondisi Atmosfer Bumi Purba." },
  { "en": "Apa Hasil Eksperimen Miller-Urey?", "id": "Terbentuknya Beberapa Jenis Asam Amino." },
  { "en": "Apa Itu Sel Somatik?", "id": "Semua Sel Tubuh Selain Gamet." },
  { "en": "Apa Itu Garis Benih (Germ Line)?", "id": "Sel Yang Akan Menjadi Gamet." },
  { "en": "Apa Fungsi Apoptosis Dalam Perkembangan?", "id": "Membentuk Jari Tangan Dan Kaki." },
  { "en": "Apa Itu Gen Hox?", "id": "Gen Pengatur Pola Dasar Tubuh." },
  { "en": "Apa Itu Induksi Embrionik?", "id": "Satu Kelompok Sel Mempengaruhi Perkembangan." },
  { "en": "Apa Sistem Peredaran Darah Terbuka?", "id": "Darah Tidak Selalu Dalam Pembuluh." },
  { "en": "Hewan Apa Punya Peredaran Darah Terbuka?", "id": "Serangga Dan Sebagian Besar Moluska." },
  { "en": "Apa Sistem Peredaran Darah Tertutup?", "id": "Darah Selalu Berada Dalam Pembuluh." },
  { "en": "Hewan Apa Punya Peredaran Darah Tertutup?", "id": "Semua Vertebrata Dan Cacing Tanah." },
  { "en": "Apa Itu Peredaran Darah Tunggal?", "id": "Darah Melewati Jantung Sekali Edar." },
  { "en": "Hewan Apa Punya Peredaran Darah Tunggal?", "id": "Ikan Dengan Jantung Dua Ruang." },
  { "en": "Apa Itu Peredaran Darah Ganda?", "id": "Darah Melewati Jantung Dua Kali." },
  { "en": "Apa Itu Insang?", "id": "Organ Pernapasan Untuk Di Air." },
  { "en": "Apa Itu Trakea Pada Serangga?", "id": "Sistem Tabung Untuk Proses Pernapasan." },
  { "en": "Apa Itu Paru-Paru Buku?", "id": "Alat Pernapasan Pada Kelompok Laba-Laba." },
  { "en": "Apa Itu Fosforilasi Tingkat Substrat?", "id": "Pembentukan Adenosine Triphosphate Langsung Substrat." },
  { "en": "Apa Itu Dekarboksilasi Oksidatif?", "id": "Reaksi Penghubung Glikolisis Siklus Krebs." },
  { "en": "Apa Produk Dekarboksilasi Oksidatif?", "id": "Asetil Ko-A, Karbon Dioksida, NADH." },
  { "en": "Apa Itu Aklimatisasi?", "id": "Penyesuaian Fisiologis Terhadap Perubahan Lingkungan." },
  { "en": "Apa Itu Glukoneogenesis?", "id": "Sintesis Glukosa Sumber Non-Karbohidrat." },
  { "en": "Apa Itu Siklus Urea?", "id": "Siklus Pengubahan Amonia Menjadi Urea." },
  { "en": "Di Mana Siklus Urea Terjadi?", "id": "Terutama Terjadi Di Dalam Organ Hati." },
  { "en": "Apa Itu Organisme Termofilik?", "id": "Organisme Yang Hidup Suhu Tinggi." },
  { "en": "Apa Itu Organisme Psikrofilik?", "id": "Organisme Yang Hidup Suhu Rendah." },
  { "en": "Apa Itu Organisme Halofilik?", "id": "Organisme Yang Hidup Kadar Garam." },
  { "en": "Apa Itu Organisme Asidofilik?", "id": "Organisme Yang Hidup Lingkungan Asam." },
  { "en": "Apa Itu Kemotaksis?", "id": "Gerakan Organisme Merespon Stimulus Kimia." },
  { "en": "Apa Itu Fototaksis?", "id": "Gerakan Organisme Merespon Stimulus Cahaya." },
  { "en": "Apa Itu Domain Dalam Klasifikasi?", "id": "Tingkatan Taksonomi Tertinggi Atas Kingdom." },
  { "en": "Sebutkan Tiga Domain Kehidupan?", "id": "Archaea, Bakteri, Dan Juga Eukarya." },
  { "en": "Apa Itu Kelompok Archaea?", "id": "Prokariota Yang Hidup Lingkungan Ekstrem." },
  { "en": "Apa Itu Peptidoglikan?", "id": "Polimer Penyusun Dinding Sel Bakteri." },
  { "en": "Dinding Sel Archaea Mengandung Apa?", "id": "Tidak Mengandung Senyawa Peptidoglikan." },
  { "en": "Apa Itu Bakteri Metanogen?", "id": "Archaea Yang Menghasilkan Gas Metana." },
  { "en": "Apa Itu Bakteri Termoasidofilik?", "id": "Archaea Yang Hidup Suhu Panas." },
  { "en": "Apa Itu Pili Pada Bakteri?", "id": "Struktur Rambut Untuk Melekat." },
  { "en": "Apa Fungsi Pili Seks?", "id": "Mentransfer Materi Genetik Saat Konjugasi." },
  { "en": "Apa Itu Transformasi Bakteri?", "id": "Pengambilan Deoxyribonucleic Acid Dari Lingkungan." },
  { "en": "Apa Itu Transduksi Bakteri?", "id": "Transfer Deoxyribonucleic Acid Melalui Virus." },
  { "en": "Apa Itu Cincin Saraf?", "id": "Sistem Saraf Sederhana Hewan Radial." },
  { "en": "Hewan Apa Punya Cincin Saraf?", "id": "Cnidaria Seperti Ubur-Ubur Bintang Laut." },
  { "en": "Apa Itu Sel Api?", "id": "Sel Ekskresi Khusus Cacing Pipih." },
  { "en": "Apa Itu Pembuluh Malpighi?", "id": "Organ Ekskresi Pada Kelompok Serangga." },
  { "en": "Apa Itu Hemolimfa?", "id": "Cairan Sirkulasi Sistem Peredaran Darah." },
  { "en": "Apa Itu Hemosianin?", "id": "Protein Pembawa Oksigen Berbasis Tembaga." },
  { "en": "Warna Apa Dihasilkan Hemosianin?", "id": "Memberikan Warna Biru Pada Darah." },
  { "en": "Hewan Apa Yang Memiliki Hemosianin?", "id": "Moluska Dan Juga Beberapa Arthropoda." },
  { "en": "Apa Itu Kelenjar Hijau?", "id": "Organ Ekskresi Pada Crustacea." },
  { "en": "Apa Itu Statosista?", "id": "Organ Keseimbangan Pada Invertebrata." },
  { "en": "Apa Itu Lamina Basal?", "id": "Lapisan Tipis Di Bawah Jaringan Epitel." },
  { "en": "Apa Itu Mikrovili?", "id": "Tonjolan Membran Sel Penambah Luas." },
  { "en": "Di Mana Mikrovili Sering Ditemukan?", "id": "Pada Sel Epitel Usus Halus." },
  { "en": "Apa Itu Persimpangan Celah (Gap Junction)?", "id": "Saluran Komunikasi Langsung Antar Sel." },
  { "en": "Apa Itu Desmosom?", "id": "Struktur Penyambung Kuat Antar Sel." },
  { "en": "Apa Itu Matriks Ekstraseluler?", "id": "Jaringan Molekul Di Luar Sel." },
  { "en": "Apa Komponen Utama Matriks Ekstraseluler?", "id": "Protein Seperti Kolagen Dan Elastin." },
  { "en": "Apa Fungsi Serat Kolagen?", "id": "Memberikan Kekuatan Tarik Pada Jaringan." },
  { "en": "Apa Fungsi Serat Elastin?", "id": "Memberikan Elastisitas Pada Jaringan." },
  { "en": "Apa Itu Sel Mast?", "id": "Sel Pelepas Histamin Saat Alergi." },
  { "en": "Apa Itu Bolus?", "id": "Gumpalan Makanan Yang Siap Ditelan." },
  { "en": "Apa Itu Kimus (Chyme)?", "id": "Bubur Makanan Dari Organ Lambung." },
  { "en": "Apa Fungsi Utama Cairan Empedu?", "id": "Mengemulsikan Lemak Menjadi Butiran Kecil." },
  { "en": "Apa Itu Penyerapan (Absorpsi)?", "id": "Proses Masuknya Nutrisi Ke Darah." },
  { "en": "Di Mana Penyerapan Nutrisi Utama Terjadi?", "id": "Di Dalam Organ Usus Halus." },
  { "en": "Apa Itu Laju Metabolisme Basal (BMR)?", "id": "Energi Minimum Saat Tubuh Istirahat." },
  { "en": "Apa Itu Indeks Massa Tubuh (IMT)?", "id": "Ukuran Lemak Tubuh Berdasarkan Tinggi." },
  { "en": "Apa Itu Asam Lemak Esensial?", "id": "Asam Lemak Yang Harus Dari Makanan." },
  { "en": "Apa Itu Asam Amino Esensial?", "id": "Asam Amino Yang Harus Dari Makanan." },
  { "en": "Apa Itu Pembuahan Ganda?", "id": "Ciri Khas Reproduksi Tumbuhan Berbunga." },
  { "en": "Apa Hasil Dari Pembuahan Ganda?", "id": "Zigot Diploid Dan Endosperma Triploid." },
  { "en": "Apa Itu Bakal Biji (Ovul)?", "id": "Struktur Yang Akan Menjadi Biji." },
  { "en": "Apa Itu Bakal Buah (Ovarium)?", "id": "Bagian Bunga Yang Menjadi Buah." },
  { "en": "Apa Itu Putik (Pistil)?", "id": "Organ Reproduksi Betina Pada Bunga." },
  { "en": "Apa Bagian-Bagian Dari Putik?", "id": "Kepala Putik, Tangkai, Dan Ovarium." },
  { "en": "Apa Itu Benang Sari (Stamen)?", "id": "Organ Reproduksi Jantan Pada Bunga." },
  { "en": "Apa Bagian-Bagian Benang Sari?", "id": "Kepala Sari Dan Tangkai Sari." },
  { "en": "Apa Itu Dormansi Biji?", "id": "Masa Istirahat Sebelum Biji Berkecambah." },
  { "en": "Apa Itu Biomagnifikasi (Bioakumulasi)?", "id": "Peningkatan Konsentrasi Racun Rantai Makanan." },
  { "en": "Apa Itu Sumber Daya Alam Terbarukan?", "id": "Sumber Daya Yang Dapat Pulih." },
  { "en": "Apa Contoh Sumber Daya Terbarukan?", "id": "Sinar Matahari, Angin, Air, Hutan." },
  { "en": "Apa Itu Sumber Daya Tak Terbarukan?", "id": "Sumber Daya Yang Akan Habis." },
  { "en": "Apa Contoh Sumber Daya Tak Terbarukan?", "id": "Minyak Bumi, Batu Bara, Gas Alam." },
  { "en": "Apa Itu Jejak Ekologis?", "id": "Ukuran Dampak Manusia Pada Alam." },
  { "en": "Apa Itu Pembangunan Berkelanjutan?", "id": "Pembangunan Tanpa Merusak Generasi Depan." },
  { "en": "Apa Itu Kompetisi Intraspesifik?", "id": "Persaingan Antara Individu Spesies Sama." },
  { "en": "Apa Itu Kompetisi Interspesifik?", "id": "Persaingan Antara Individu Spesies Berbeda." },
  { "en": "Apa Itu Potensial Membran?", "id": "Perbedaan Muatan Listrik Melintasi Membran." },
  { "en": "Apa Perbedaan Apoptosis Dan Nekrosis?", "id": "Apoptosis Terprogram, Nekrosis Akibat Cedera." },
  { "en": "Apa Fungsi Sel T Sitotoksik?", "id": "Sel Imun Yang Membunuh Sel." },
  { "en": "Apa Fungsi Sel Natural Killer (NK)?", "id": "Membunuh Sel Yang Terinfeksi Virus." },
  { "en": "Apa Itu Interferon?", "id": "Protein Sinyal Pelawan Infeksi Virus." },
  { "en": "Apa Itu Opsonisasi?", "id": "Proses Penandaan Patogen Untuk Fagositosis." },
  { "en": "Apa Itu Gen Holandrik?", "id": "Gen Yang Terpaut Kromosom Y." },
  { "en": "Apa Itu Gen Terpaut Seks?", "id": "Gen Terletak Pada Kromosom Seks." },
  { "en": "Apa Itu Poliploidi?", "id": "Kondisi Sel Memiliki Banyak Kromosom." },
  { "en": "Apa Itu Aneuploidi?", "id": "Perubahan Jumlah Satu Beberapa Kromosom." },
  { "en": "Apa Itu Delesi Kromosom?", "id": "Hilangnya Segmen Dari Suatu Kromosom." },
  { "en": "Apa Itu Duplikasi Kromosom?", "id": "Penggandaan Segmen Dari Suatu Kromosom." },
  { "en": "Apa Itu Inversi Kromosom?", "id": "Pembalikan Arah Segmen Suatu Kromosom." },
  { "en": "Apa Itu Translokasi Kromosom?", "id": "Pindahnya Segmen Kromosom Ke Non-Homolog." },
  { "en": "Apa Itu Peta Silsilah (Pedigree)?", "id": "Diagram Pewarisan Sifat Suatu Keluarga." },
  { "en": "Apa Itu Sistem Saraf Pusat?", "id": "Terdiri Dari Otak Sumsum Tulang." },
  { "en": "Apa Itu Sistem Saraf Tepi?", "id": "Saraf Di Luar Sistem Saraf." },
  { "en": "Apa Itu Cairan Serebrospinal?", "id": "Cairan Pelindung Otak Sumsum Tulang." },
  { "en": "Apa Itu Materi Kelabu (Grey Matter)?", "id": "Area Sistem Saraf Kaya Badan Sel." },
  { "en": "Apa Itu Materi Putih (White Matter)?", "id": "Area Sistem Saraf Kaya Akson." },
  { "en": "Apa Fungsi Bagian Hipotalamus?", "id": "Bagian Otak Pengatur Proses Homeostasis." },
  { "en": "Apa Fungsi Bagian Talamus?", "id": "Stasiun Pemancar Informasi Sensorik Otak." },
  { "en": "Apa Fungsi Lobus Frontal?", "id": "Bagian Otak Berpikir Dan Merencanakan." },
  { "en": "Apa Fungsi Lobus Temporal?", "id": "Bagian Otak Pendengaran Dan Memori." },
  { "en": "Apa Fungsi Lobus Parietal?", "id": "Bagian Otak Sentuhan Dan Persepsi." },
  { "en": "Apa Fungsi Lobus Oksipital?", "id": "Bagian Otak Proses Penglihatan." },
  { "en": "Apa Fungsi Korpus Kalosum?", "id": "Struktur Penghubung Dua Belahan Otak." },
  { "en": "Apa Itu Proses Koevolusi?", "id": "Dua Spesies Saling Mempengaruhi Evolusi." },
  { "en": "Apa Contoh Proses Koevolusi?", "id": "Bunga Dan Serangga Penyerbuknya." },
  { "en": "Apa Itu Seleksi Artifisial?", "id": "Seleksi Oleh Manusia Sifat Tertentu." },
  { "en": "Apa Contoh Seleksi Artifisial?", "id": "Berbagai Ras Anjing Varietas Kubis." },
  { "en": "Apa Itu Ilmu Biogeografi?", "id": "Studi Distribusi Geografis Makhluk Hidup." },
  { "en": "Apa Itu Pangea?", "id": "Benua Super Besar Di Masa Lalu." },
  { "en": "Apa Itu Kriptobiosis?", "id": "Keadaan Metabolisme Sangat Rendah Ekstrem." },
  { "en": "Apa Itu Tardigrada (Beruang Air)?", "id": "Mikro-Hewan Yang Mampu Melakukan Kriptobiosis." },
  { "en": "Apa Itu Mutualisme Obligat?", "id": "Simbiosis Yang Saling Membutuhkan Mutlak." },
  { "en": "Apa Itu Mutualisme Fakultatif?", "id": "Simbiosis Yang Tidak Saling Bergantung." },
  { "en": "Apa Perbedaan Ekskresi Dan Sekresi?", "id": "Ekskresi Membuang Sisa, Sekresi Melepas." },
  { "en": "Apa Itu Proses Defekasi?", "id": "Pengeluaran Sisa Makanan Padat Feses." },
  { "en": "Apa Itu Endodermis Pada Akar?", "id": "Lapisan Pemisah Korteks Silinder Pusat." },
  { "en": "Apa Itu Pita Kaspari?", "id": "Struktur Kedap Air Di Endodermis." },
  { "en": "Apa Fungsi Pita Kaspari?", "id": "Mengatur Jalur Masuk Air Mineral." },
  { "en": "Apa Itu Perisikel?", "id": "Lapisan Terluar Silinder Pusat Akar." },
  { "en": "Apa Fungsi Jaringan Perisikel?", "id": "Membentuk Cabang Akar Dan Kambium." },
  { "en": "Apa Itu Kelenjar Ludah (Saliva)?", "id": "Menghasilkan Air Liur Pencernaan Awal." },
  { "en": "Apa Itu Bilirubin?", "id": "Pigmen Kuning Hasil Pemecahan Hemoglobin." },
  { "en": "Apa Yang Menyebabkan Penyakit Kuning?", "id": "Penumpukan Zat Bilirubin Dalam Darah." },
  { "en": "Apa Itu Hormon Pertumbuhan Manusia?", "id": "Merangsang Pertumbuhan, Reproduksi Sel." },
  { "en": "Apa Itu Gigantisme?", "id": "Kelebihan Hormon Pertumbuhan Masa Anak." },
  { "en": "Apa Itu Akromegali?", "id": "Kelebihan Hormon Pertumbuhan Masa Dewasa." },
  { "en": "Apa Itu Dwarfisme?", "id": "Kekurangan Hormon Pertumbuhan Masa Anak." },
  { "en": "Apa Itu Sistem Saraf Simpatik?", "id": "Mempersiapkan Tubuh Respon Lawan Lari." },
  { "en": "Apa Itu Sistem Saraf Parasimpatik?", "id": "Menenangkan Tubuh Setelah Respon Stres." },
  { "en": "Apa Itu Aferen?", "id": "Membawa Sinyal Ke Sistem Saraf." },
  { "en": "Apa Itu Eferen?", "id": "Membawa Sinyal Dari Sistem Saraf." },
  { "en": "Apa Itu Korteks Serebral?", "id": "Lapisan Luar Otak Besar Manusia." },
  { "en": "Apa Itu Sfingolipid?", "id": "Jenis Lemak Ditemukan Membran Sel." },
  { "en": "Apa Itu Glikoprotein?", "id": "Protein Yang Terikat Dengan Karbohidrat." },
  { "en": "Apa Itu Glikolipid?", "id": "Lipid Yang Terikat Dengan Karbohidrat." },
  { "en": "Apa Itu Saluran Ion?", "id": "Protein Membran Pembentuk Pori-Pori." },
  { "en": "Apa Itu Perikardium?", "id": "Kantung Pelindung Yang Membungkus Jantung." },
  { "en": "Apa Itu Endokardium?", "id": "Lapisan Dalam Yang Melapisi Ruang Jantung." },
  { "en": "Apa Itu Miokardium?", "id": "Lapisan Otot Jantung Yang Tebal." },
  { "en": "Apa Itu Nodus Sinoatrial (SA node)?", "id": "Pemacu Alami Detak Jantung Manusia." },
  { "en": "Apa Itu Nodus Atrioventrikular (AV node)?", "id": "Mengatur Sinyal Listrik Ke Ventrikel." },
  { "en": "Apa Itu Elektrokardiogram (EKG)?", "id": "Rekaman Aktivitas Listrik Organ Jantung." },
  { "en": "Apa Itu Tekanan Darah?", "id": "Gaya Darah Mendorong Dinding Arteri." },
  { "en": "Apa Itu Sirkulasi Sistemik?", "id": "Peredaran Darah Jantung Seluruh Tubuh." },
  { "en": "Apa Itu Sirkulasi Pulmonal?", "id": "Peredaran Darah Jantung Ke Paru-Paru." },
  { "en": "Apa Itu Aterosklerosis?", "id": "Pengerasan Arteri Akibat Penumpukan Plak." },
  { "en": "Apa Penyusun Utama Dinding Sel Tumbuhan?", "id": "Terutama Tersusun Dari Zat Selulosa." },
  { "en": "Apa Itu Vernalisasi?", "id": "Kebutuhan Suhu Dingin Untuk Berbunga." },
  { "en": "Apa Itu Apikal Dominansi?", "id": "Pertumbuhan Tunas Ujung Hambat Samping." },
  { "en": "Hormon Apa Penyebab Apikal Dominansi?", "id": "Hormon Auksin Dihasilkan Pucuk Ujung." },
  { "en": "Apa Itu Etiolasi?", "id": "Pertumbuhan Cepat Di Tempat Gelap." },
  { "en": "Apa Ciri-Ciri Tumbuhan Etiolasi?", "id": "Pucat, Lemah, Dan Batangnya Panjang." },
  { "en": "Apa Itu Simetri Radial?", "id": "Bagian Tubuh Tersusun Melingkari Poros." },
  { "en": "Hewan Apa Yang Memiliki Simetri Radial?", "id": "Ubur-Ubur, Anemon Laut, Bintang Laut." },
  { "en": "Apa Itu Simetri Bilateral?", "id": "Tubuh Dapat Dibagi Dua Sama." },
  { "en": "Apa Itu Kelas Sefalopoda?", "id": "Kelas Moluska Seperti Cumi Gurita." },
  { "en": "Apa Itu Kelas Gastropoda?", "id": "Kelas Moluska Seperti Siput Keong." },
  { "en": "Apa Itu Kelas Bivalvia?", "id": "Kelas Moluska Seperti Kerang Tiram." },
  { "en": "Apa Itu Eksoskeleton?", "id": "Rangka Luar Yang Keras Pelindung." },
  { "en": "Eksoskeleton Arthropoda Terbuat Dari Apa?", "id": "Terbuat Dari Zat Bernama Kitin." },
  { "en": "Apa Itu Ekdisis (Molting)?", "id": "Proses Pelepasan Eksoskeleton Yang Lama." },
  { "en": "Apa Itu Mata Majemuk?", "id": "Mata Terdiri Dari Banyak Unit." },
  { "en": "Apa Itu Beta-Oksidasi?", "id": "Proses Pemecahan Molekul Asam Lemak." },
  { "en": "Apa Itu Ketogenesis?", "id": "Produksi Badan Keton Dari Lemak." },
  { "en": "Apa Itu Siklus Cori?", "id": "Siklus Asam Laktat Antara Otot." },
  { "en": "Apa Itu Titik Isoelektrik?", "id": "Nilai pH Saat Muatan Netral." },
  { "en": "Apa Itu Zimogen (Proenzim)?", "id": "Bentuk Tidak Aktif Suatu Enzim." },
  { "en": "Bagaimana Cara Zimogen Diaktifkan?", "id": "Dengan Pemotongan Bagian Tertentu." },
  { "en": "Apa Contoh Dari Zimogen?", "id": "Pepsinogen Diaktifkan Menjadi Enzim Pepsin." },
  { "en": "Apa Itu Regulasi Alosterik?", "id": "Regulasi Enzim Pada Sisi Non-Aktif." },
  { "en": "Apa Itu Inhibitor Kompetitif?", "id": "Menghambat Dengan Menempel Sisi Aktif." },
  { "en": "Apa Itu Inhibitor Non-Kompetitif?", "id": "Menghambat Dengan Menempel Sisi Lain." },
  { "en": "Apa Itu Epistasis?", "id": "Interaksi Gen Satu Gen Menutupi." },
  { "en": "Apa Itu Pleiotropi?", "id": "Satu Gen Mempengaruhi Banyak Sifat." },
  { "en": "Apa Itu Ekspresivitas Gen?", "id": "Tingkat Ekspresi Suatu Genotipe." },
  { "en": "Apa Itu Penetrasi Gen?", "id": "Persentase Individu Menunjukkan Fenotipe." },
  { "en": "Apa Itu Kromosom Homolog?", "id": "Sepasang Kromosom Dengan Gen Sama." },
  { "en": "Apa Itu Lokus Gen?", "id": "Posisi Spesifik Gen Pada Kromosom." },
  { "en": "Apa Itu Galur Murni?", "id": "Organisme Homozigot Sifat Tertentu." },
  { "en": "Apa Itu Uji Silang (Test Cross)?", "id": "Menyilangkan Individu Dengan Homozigot Resesif." },
  { "en": "Apa Tujuan Uji Silang?", "id": "Menentukan Genotipe Individu Yang Dominan." },
  { "en": "Apa Itu Non-Disjungsi?", "id": "Kegagalan Kromosom Berpisah Saat Meiosis." },
  { "en": "Apa Itu Kapasitas Vital Paru-Paru?", "id": "Volume Udara Maksimum Bisa Dihembuskan." },
  { "en": "Apa Itu Volume Tidal?", "id": "Volume Udara Saat Pernapasan Normal." },
  { "en": "Apa Itu Biliverdin?", "id": "Pigmen Hijau Hasil Pemecahan Hemoglobin." },
  { "en": "Apa Itu Filtrasi Glomerulus?", "id": "Langkah Pertama Proses Pembentukan Urine." },
  { "en": "Apa Itu Hormon Eritropoietin (EPO)?", "id": "Hormon Perangsang Produksi Sel Darah." },
  { "en": "Di Mana Hormon Eritropoietin Diproduksi?", "id": "Sebagian Besar Diproduksi Di Ginjal." },
  { "en": "Apa Itu Sistem Renin-Angiotensin?", "id": "Sistem Hormon Pengatur Tekanan Darah." },
  { "en": "Apa Itu Pepsinogen?", "id": "Bentuk Tidak Aktif Enzim Pepsin." },
  { "en": "Apa Yang Mengaktifkan Pepsinogen?", "id": "Asam Klorida (HCl) Di Lambung." },
  { "en": "Apa Itu Produktivitas Primer Kotor (PPK)?", "id": "Total Laju Fotosintesis Suatu Ekosistem." },
  { "en": "Apa Itu Produktivitas Primer Bersih (PPB)?", "id": "Energi Tersisa Setelah Respirasi Produsen." },
  { "en": "Apa Itu Efisiensi Ekologis?", "id": "Persentase Transfer Energi Antar Tingkat." },
  { "en": "Berapa Rata-Rata Efisiensi Ekologis?", "id": "Sekitar Sepuluh Persen." },
  { "en": "Apa Itu Piramida Jumlah?", "id": "Menggambarkan Jumlah Individu Tiap Tingkat." },
  { "en": "Apa Itu Piramida Biomassa?", "id": "Menggambarkan Total Massa Tiap Tingkat." },
  { "en": "Apa Itu Piramida Energi?", "id": "Menggambarkan Aliran Energi Tiap Tingkat." },
  { "en": "Piramida Apa Yang Tidak Bisa Terbalik?", "id": "Piramida Energi Tidak Pernah Terbalik." },
  { "en": "Apa Itu Zona Litoral?", "id": "Wilayah Tepi Danau Yang Dangkal." },
  { "en": "Apa Itu Zona Limnetik?", "id": "Wilayah Air Terbuka Suatu Danau." },
  { "en": "Apa Itu Teori Endosimbiosis?", "id": "Teori Asal Usul Mitokondria Kloroplas." },
  { "en": "Siapa Yang Mempopulerkan Teori Endosimbiosis?", "id": "Seorang Ilmuwan Bernama Lynn Margulis." },
  { "en": "Apa Itu Fosfolipid Bilayer?", "id": "Struktur Dasar Dari Membran Sel." },
  { "en": "Apa Itu Model Mosaik Fluida?", "id": "Model Struktur Dari Membran Sel." },
  { "en": "Apa Itu Protein Integral?", "id": "Protein Yang Menembus Membran Sel." },
  { "en": "Apa Itu Protein Perifer?", "id": "Protein Menempel Permukaan Membran." },
  { "en": "Apa Itu Akuaporin?", "id": "Kanal Protein Untuk Lewatnya Air." },
  { "en": "Apa Itu Pompa Natrium-Kalium?", "id": "Contoh Transpor Aktif Pada Sel." },
  { "en": "Apa Itu Endospora?", "id": "Struktur Pertahanan Bakteri Kondisi Ekstrem." },
  { "en": "Apa Itu Kapsul Bakteri?", "id": "Lapisan Luar Pelindung Beberapa Bakteri." },
  { "en": "Apa Itu Termoreseptor?", "id": "Reseptor Yang Peka Terhadap Suhu." },
  { "en": "Apa Itu Mekanoreseptor?", "id": "Reseptor Peka Terhadap Tekanan Sentuhan." },
  { "en": "Apa Itu Kemoreseptor?", "id": "Reseptor Yang Peka Zat Kimia." },
  { "en": "Apa Itu Fotoreseptor?", "id": "Reseptor Yang Peka Terhadap Cahaya." },
  { "en": "Apa Itu Nosiseptor?", "id": "Reseptor Yang Mendeteksi Rasa Sakit." },
  { "en": "Apa Itu Sel Glial?", "id": "Sel Pendukung Di Sistem Saraf." },
  { "en": "Apa Itu Sel Schwann?", "id": "Sel Glial Pembentuk Selubung Mielin." },
  { "en": "Apa Itu Mikroglia?", "id": "Sel Imun Di Sistem Saraf." },
  { "en": "Apa Itu Astrosit?", "id": "Sel Glial Berbentuk Bintang." },
  { "en": "Apa Itu Sel Ependimal?", "id": "Sel Pelapis Ventrikel Otak." },
  { "en": "Apa Itu Lengkung Refleks?", "id": "Jalur Saraf Untuk Gerak Refleks." },
  { "en": "Apa Itu Batang Otak?", "id": "Penghubung Otak Besar Sumsum Tulang." },
  { "en": "Apa Fungsi Medula Oblongata?", "id": "Mengatur Fungsi Vital Seperti Detak." },
  { "en": "Apa Itu Serebelum?", "id": "Nama Lain Untuk Otak Kecil." },
  { "en": "Apa Itu Serebrum?", "id": "Nama Lain Untuk Otak Besar." },
  { "en": "Apa Itu Amigdala?", "id": "Bagian Otak Terlibat Respon Emosi." },
  { "en": "Apa Itu Hipokampus?", "id": "Bagian Otak Berperan Pembentukan Memori." },
  { "en": "Apa Itu Hemolimfa?", "id": "Cairan Sirkulasi Sistem Peredaran Terbuka." },
  { "en": "Apa Itu Jantung Neurogenik?", "id": "Detak Jantung Diatur Sinyal Saraf." },
  { "en": "Apa Itu Jantung Miogenik?", "id": "Detak Jantung Diatur Sel Khusus." },
  { "en": "Apa Itu Diastol?", "id": "Fase Relaksasi Otot Jantung." },
  { "en": "Apa Itu Sistol?", "id": "Fase Kontraksi Otot Jantung." },
  { "en": "Apa Itu Klad?", "id": "Kelompok Monofiletik, Satu Nenek Moyang." },
  { "en": "Apa Itu Kelompok Polifiletik?", "id": "Kelompok Berasal Dari Banyak Nenek Moyang." },
  { "en": "Apa Itu Kelompok Parafiletik?", "id": "Kelompok Tidak Mencakup Semua Turunan." },
  { "en": "Apa Itu Karakter Plesiomorfik?", "id": "Karakter Primitif Yang Diwariskan." },
  { "en": "Apa Itu Karakter Apomorfik?", "id": "Karakter Turunan Yang Baru Berevolusi." },
  { "en": "Apa Itu Coelenterata?", "id": "Nama Lama Untuk Filum Cnidaria." },
  { "en": "Apa Itu Filum Nematoda?", "id": "Filum Cacing Gelang Tidak Bersegmen." },
  { "en": "Apa Contoh Nematoda Parasit?", "id": "Cacing Kremi, Tambang, Dan Filaria." },
  { "en": "Apa Itu Geotropisme (Gravitropisme)?", "id": "Gerak Pertumbuhan Merespon Gravitasi Bumi." },
  { "en": "Apa Itu Tigmotropisme?", "id": "Gerak Pertumbuhan Merespon Rangsang Sentuhan." },
  { "en": "Apa Itu Hidrotropisme?", "id": "Gerak Pertumbuhan Menuju Sumber Air." },
  { "en": "Akar Menunjukkan Geotropisme Jenis Apa?", "id": "Menunjukkan Geotropisme Positif." },
  { "en": "Batang Menunjukkan Geotropisme Jenis Apa?", "id": "Menunjukkan Geotropisme Negatif." },
  { "en": "Apa Itu Gerak Niktinasti?", "id": "Gerak Tidur Daun Pada Malam." },
  { "en": "Apa Contoh Gerak Niktinasti?", "id": "Menutupnya Daun Tanaman Polong-Polongan." },
  { "en": "Apa Itu Gerak Fotonasti?", "id": "Gerak Nasti Yang Dirangsang Cahaya." },
  { "en": "Apa Itu Ligan?", "id": "Molekul Yang Mengikat Protein Reseptor." },
  { "en": "Apa Itu Reseptor Permukaan Sel?", "id": "Protein Di Membran Penerima Sinyal." },
  { "en": "Apa Itu Reseptor Intraseluler?", "id": "Reseptor Di Dalam Sel Sitoplasma." },
  { "en": "Apa Itu Second Messenger?", "id": "Molekul Sinyal Kecil Dalam Sel." },
  { "en": "Apa Contoh Dari Second Messenger?", "id": "Siklik Adenosine Monophosphate Dan Ion Kalsium." },
  { "en": "Apa Itu Enzim Kinase?", "id": "Enzim Yang Menambahkan Gugus Fosfat." },
  { "en": "Apa Itu Enzim Fosfatase?", "id": "Enzim Yang Menghilangkan Gugus Fosfat." },
  { "en": "Apa Itu Kaskade Sinyal?", "id": "Rangkaian Reaksi Aktivasi Protein." },
  { "en": "Apa Itu Hormon Tropik?", "id": "Hormon Yang Merangsang Kelenjar Lain." },
  { "en": "Kelenjar Apa Hasilkan Hormon Tropik?", "id": "Kelenjar Hipofisis Atau Pituitari Anterior." },
  { "en": "Apa Itu Hormon Pelepas?", "id": "Hormon Hipotalamus Merangsang Kelenjar Hipofisis." },
  { "en": "Apa Itu Akson Terminal?", "id": "Ujung Akson Tempat Pelepasan Neurotransmitter." },
  { "en": "Apa Itu Vesikel Sinaptik?", "id": "Kantung Kecil Berisi Molekul Neurotransmitter." },
  { "en": "Apa Itu Periode Refraktori?", "id": "Masa Neuron Tidak Bisa Picu." },
  { "en": "Apa Itu Saltatory Conduction?", "id": "Loncatan Impuls Di Sepanjang Akson." },
  { "en": "Neurotransmitter Utama Sistem Parasimpatik?", "id": "Asetilkolin Adalah Neurotransmitter Utama." },
  { "en": "Apa Itu Komunikasi Taktil?", "id": "Komunikasi Melalui Sentuhan Fisik." },
  { "en": "Apa Itu Komunikasi Visual?", "id": "Komunikasi Melalui Sinyal Yang Terlihat." },
  { "en": "Apa Itu Komunikasi Auditori?", "id": "Komunikasi Melalui Gelombang Suara." },
  { "en": "Apa Itu Tarian Lebah?", "id": "Komunikasi Lebah Madu Sumber Makanan." },
  { "en": "Siapa Yang Meneliti Tarian Lebah?", "id": "Ilmuwan Bernama Karl Von Frisch." },
  { "en": "Apa Itu Pembelajaran Asosiatif?", "id": "Belajar Menghubungkan Satu Stimulus Lain." },
  { "en": "Apa Itu Pembelajaran Spasial?", "id": "Belajar Berdasarkan Struktur Lingkungan." },
  { "en": "Apa Itu Kognisi?", "id": "Proses Mental Perolehan Pengetahuan." },
  { "en": "Apa Itu Southern Blotting?", "id": "Teknik Mendeteksi Urutan Deoxyribonucleic Acid." },
  { "en": "Apa Itu Northern Blotting?", "id": "Teknik Mendeteksi Urutan Ribonucleic Acid." },
  { "en": "Apa Itu Western Blotting?", "id": "Teknik Mendeteksi Protein Spesifik." },
  { "en": "Apa Itu Microarray Deoxyribonucleic Acid?", "id": "Alat Analisis Ekspresi Ribuan Gen." },
  { "en": "Apa Itu Sekuensing Deoxyribonucleic Acid?", "id": "Proses Penentuan Urutan Basa Nukleotida." },
  { "en": "Apa Itu CRISPR-Cas9?", "id": "Sistem Penyuntingan Gen Yang Presisi." },
  { "en": "Apa Itu Sel Hibridoma?", "id": "Sel Hibrida Penghasil Antibodi Monoklonal." },
  { "en": "Apa Itu Kromatografi Afinitas?", "id": "Metode Pemurnian Protein Berdasarkan Ikatan." },
  { "en": "Apa Itu Protonefridia?", "id": "Sistem Ekskresi Sederhana Cacing Pipih." },
  { "en": "Apa Itu Metanefridia?", "id": "Sistem Ekskresi Cacing Tanah." },
  { "en": "Apa Itu Kelenjar Garam?", "id": "Organ Ekskresi Garam Burung Laut." },
  { "en": "Bagaimana Serangga Menghemat Air?", "id": "Dengan Sistem Ekskresi Asam Urat." },
  { "en": "Apa Itu Kantung Udara Burung?", "id": "Struktur Aliran Udara Searah." },
  { "en": "Apa Keuntungan Aliran Udara Searah?", "id": "Pertukaran Gas Yang Sangat Efisien." },
  { "en": "Apa Itu Pertukaran Lawan Arus?", "id": "Mekanisme Efisiensi Pertukaran Zat." },
  { "en": "Di Mana Pertukaran Lawan Arus Ditemukan?", "id": "Pada Insang Ikan Dan Ginjal." },
  { "en": "Apa Itu Selubung Nukleus?", "id": "Membran Ganda Yang Mengelilingi Inti." },
  { "en": "Apa Itu Pori Nukleus?", "id": "Saluran Pengatur Lalu Lintas Molekul." },
  { "en": "Apa Itu Nukleolus?", "id": "Tempat Perakitan Ribosom Di Inti." },
  { "en": "Apa Itu Kromatin?", "id": "Kompleks Deoxyribonucleic Acid Dan Protein." },
  { "en": "Apa Itu Eukromatin?", "id": "Bentuk Kromatin Yang Lebih Longgar." },
  { "en": "Apa Itu Heterokromatin?", "id": "Bentuk Kromatin Yang Lebih Padat." },
  { "en": "Apa Itu Lamina Nukleus?", "id": "Jaringan Serat Penopang Selubung Nukleus." },
  { "en": "Apa Itu Glikokalyx?", "id": "Lapisan Karbohidrat Di Luar Sel." },
  { "en": "Apa Itu Endositosis Dimediasi Reseptor?", "id": "Pemasukan Molekul Spesifik Ke Sel." },
  { "en": "Apa Itu Transkripsi Terbalik?", "id": "Sintesis Deoxyribonucleic Acid Dari Ribonucleic Acid." },
  { "en": "Enzim Apa Lakukan Transkripsi Terbalik?", "id": "Enzim Bernama Reverse Transcriptase." },
  { "en": "Virus Apa Gunakan Transkripsi Terbalik?", "id": "Retrovirus Seperti Human Immunodeficiency Virus." },
  { "en": "Apa Itu Prion?", "id": "Agen Infeksius Hanya Dari Protein." },
  { "en": "Penyakit Apa Disebabkan Oleh Prion?", "id": "Penyakit Sapi Gila Dan Creutzfeldt-Jakob." },
  { "en": "Apa Itu Viroid?", "id": "Agen Infeksius Hanya Molekul Ribonucleic Acid." },
  { "en": "Siapa Yang Diserang Oleh Viroid?", "id": "Hanya Menyerang Organisme Yaitu Tumbuhan." },
  { "en": "Apa Itu Kodon Start?", "id": "Kodon Penanda Awal Proses Translasi." },
  { "en": "Kodon Apa Menjadi Kodon Start?", "id": "AUG Mengkodekan Asam Amino Metionin." },
  { "en": "Apa Itu Kodon Stop?", "id": "Kodon Penanda Akhir Proses Translasi." },
  { "en": "Apa Itu Mutasi Titik?", "id": "Perubahan Pada Satu Pasangan Basa." },
  { "en": "Apa Itu Mutasi Diam (Silent Mutation)?", "id": "Mutasi Tidak Mengubah Asam Amino." },
  { "en": "Apa Itu Mutasi Salah Arti (Missense)?", "id": "Mutasi Mengubah Satu Asam Amino." },
  { "en": "Apa Itu Mutasi Nonsense?", "id": "Mutasi Menghasilkan Kodon Stop Prematur." },
  { "en": "Apa Itu Mutasi Frameshift?", "id": "Pergeseran Kerangka Baca Akibat Insersi." },
  { "en": "Apa Itu Mutagen?", "id": "Agen Fisik Kimia Penyebab Mutasi." },
  { "en": "Apa Itu Karsinogen?", "id": "Agen Yang Dapat Menyebabkan Kanker." },
  { "en": "Apa Itu Gen Supresor Tumor?", "id": "Gen Yang Menghambat Pertumbuhan Sel." },
  { "en": "Apa Contoh Gen Supresor Tumor?", "id": "Gen P53 Yang Terkenal." },
  { "en": "Apa Itu Proto-onkogen?", "id": "Gen Normal Pengatur Pertumbuhan Sel." },
  { "en": "Apa Itu Onkogen?", "id": "Gen Termutasi Penyebab Pertumbuhan Kanker." },
  { "en": "Apa Itu Metastasis?", "id": "Penyebaran Sel Kanker Bagian Lain." },
  { "en": "Apa Itu Uji Ames?", "id": "Tes Untuk Mengidentifikasi Potensi Karsinogen." },
  { "en": "Apa Itu Bakterioklorofil?", "id": "Pigmen Fotosintetik Pada Bakteri Tertentu." },
  { "en": "Apa Itu Fikobilin?", "id": "Pigmen Asesori Pada Cyanobacteria Alga." },
  { "en": "Apa Itu Karotenoid?", "id": "Pigmen Oranye Kuning Fotosintesis." },
  { "en": "Apa Itu Antosianin?", "id": "Pigmen Merah, Ungu, Biru Tumbuhan." },
  { "en": "Apa Itu Kelenjar Tiroid?", "id": "Kelenjar Endokrin Di Bagian Leher." },
  { "en": "Hormon Apa Dihasilkan Kelenjar Tiroid?", "id": "Hormon Tiroksin Dan Juga Kalsitonin." },
  { "en": "Apa Fungsi Hormon Tiroksin?", "id": "Mengatur Tingkat Laju Metabolisme Tubuh." },
  { "en": "Apa Fungsi Hormon Kalsitonin?", "id": "Menurunkan Kadar Kalsium Dalam Darah." },
  { "en": "Apa Itu Kerangka Aksial?", "id": "Rangka Sumbu Tubuh, Tengkorak, Tulang Belakang." },
  { "en": "Apa Itu Kerangka Apendikular?", "id": "Rangka Anggota Gerak Dan Gelangnya." },
  { "en": "Berapa Jumlah Ruas Tulang Belakang Manusia?", "id": "Tiga Puluh Tiga Ruas Tulang." },
  { "en": "Apa Lima Bagian Tulang Belakang?", "id": "Servikal, Torakal, Lumbar, Sakral, Koksigis." },
  { "en": "Tulang Apa Yang Disebut Tulang Belikat?", "id": "Tulang Skapula Di Bagian Bahu." },
  { "en": "Tulang Apa Yang Disebut Tulang Selangka?", "id": "Tulang Klavikula Di Bagian Bahu." },
  { "en": "Apa Itu Foramen Magnum?", "id": "Lubang Besar Di Dasar Tengkorak." },
  { "en": "Apa Itu Sutura Pada Tengkorak?", "id": "Garis Sendi Mati Antar Tulang." },
  { "en": "Apa Itu Sel Osteoblas?", "id": "Sel Yang Bertanggung Jawab Membentuk Tulang." },
  { "en": "Apa Itu Sel Osteoklas?", "id": "Sel Yang Bertanggung Jawab Merombak Tulang." },
  { "en": "Apa Itu Sel Osteosit?", "id": "Sel Tulang Dewasa Di Matriks." },
  { "en": "Apa Itu Sarkomer?", "id": "Unit Fungsional Dasar Dari Otot." },
  { "en": "Apa Itu Retikulum Sarkoplasma?", "id": "Retikulum Endoplasma Khusus Sel Otot." },
  { "en": "Apa Fungsi Retikulum Sarkoplasma?", "id": "Menyimpan Dan Melepaskan Ion Kalsium." },
  { "en": "Apa Itu Mioglobin?", "id": "Protein Pengikat Oksigen Di Otot." },
  { "en": "Apa Itu Kreatin Fosfat?", "id": "Molekul Penyimpan Energi Di Otot." },
  { "en": "Apa Itu Kontraksi Isotonik?", "id": "Kontraksi Otot Dengan Adanya Pemendekan." },
  { "en": "Apa Itu Kontraksi Isometrik?", "id": "Kontraksi Otot Tanpa Perubahan Panjang." },
  { "en": "Apa Itu Unit Motorik?", "id": "Satu Neuron Motorik Dan Seratnya." },
  { "en": "Apa Itu Origo Otot?", "id": "Ujung Otot Melekat Tulang Diam." },
  { "en": "Apa Itu Insersio Otot?", "id": "Ujung Otot Melekat Tulang Bergerak." },
  { "en": "Apa Itu Fitokrom?", "id": "Fotoreseptor Tumbuhan Untuk Deteksi Cahaya." },
  { "en": "Apa Peran Utama Fitokrom?", "id": "Mengatur Perkecambahan, Pembungaan, Dan Dormansi." },
  { "en": "Apa Itu Kriptokrom?", "id": "Fotoreseptor Peka Terhadap Cahaya Biru." },
  { "en": "Apa Itu Pembuluh Tapis?", "id": "Sel Utama Penyusun Jaringan Floem." },
  { "en": "Apa Fungsi Sel Pengiring?", "id": "Sel Pendukung Fungsi Pembuluh Tapis." },
  { "en": "Apa Itu Trakeid Dan Trakea?", "id": "Dua Jenis Sel Penyusun Xilem." },
  { "en": "Apa Itu Jalur Apoplas?", "id": "Transportasi Air Melalui Dinding Sel." },
  { "en": "Apa Itu Jalur Simplas?", "id": "Transportasi Air Melalui Sitoplasma Sel." },
  { "en": "Apa Itu Badan Sel (Soma)?", "id": "Bagian Utama Neuron Berisi Nukleus." },
  { "en": "Apa Itu Telinga Dalam?", "id": "Bagian Telinga Berisi Koklea Vestibula." },
  { "en": "Apa Itu Sistem Vestibular?", "id": "Sistem Indera Untuk Keseimbangan Tubuh." },
  { "en": "Apa Itu Bintik Kuning (Makula Lutea)?", "id": "Bagian Retina Kaya Sel Kerucut." },
  { "en": "Apa Itu Humor Aqueous?", "id": "Cairan Bening Di Depan Lensa." },
  { "en": "Apa Itu Humor Vitreous?", "id": "Cairan Seperti Gel Bola Mata." },
  { "en": "Apa Itu Akomodasi Mata?", "id": "Kemampuan Lensa Mata Mengubah Fokus." },
  { "en": "Apa Itu Miopia (Rabun Jauh)?", "id": "Bayangan Jatuh Di Depan Retina." },
  { "en": "Apa Itu Hipermetropia (Rabun Dekat)?", "id": "Bayangan Jatuh Di Belakang Retina." },
  { "en": "Apa Itu Astigmatisme?", "id": "Kelainan Kelengkungan Kornea Atau Lensa." },
  { "en": "Apa Itu Fragmen Okazaki?", "id": "Potongan Pendek Deoxyribonucleic Acid Untai Lambat." },
  { "en": "Apa Fungsi Enzim Ligase?", "id": "Enzim Menyambung Fragmen Deoxyribonucleic Acid." },
  { "en": "Apa Fungsi Enzim Primase?", "id": "Enzim Pembuat Primer Ribonucleic Acid." },
  { "en": "Apa Itu Untai Maju (Leading Strand)?", "id": "Disintesis Secara Kontinu Saat Replikasi." },
  { "en": "Apa Itu Untai Lambat (Lagging Strand)?", "id": "Disintesis Secara Terputus-Putus." },
  { "en": "Apa Itu Garpu Replikasi?", "id": "Wilayah Aktif Pemisahan Untai Deoxyribonucleic Acid." },
  { "en": "Apa Itu Operon Lac?", "id": "Operon Pengatur Metabolisme Laktosa Bakteri." },
  { "en": "Apa Itu Operon Trp?", "id": "Operon Pengatur Sintesis Asam Triptofan." },
  { "en": "Apa Itu Protein Represor?", "id": "Protein Yang Menghambat Proses Transkripsi." },
  { "en": "Apa Itu Induser?", "id": "Molekul Menginaktivasi Protein Represor." },
  { "en": "Apa Itu Resiliensi Ekologis?", "id": "Kemampuan Ekosistem Untuk Pulih Cepat." },
  { "en": "Apa Itu Resistensi Ekologis?", "id": "Kemampuan Ekosistem Menahan Suatu Gangguan." },
  { "en": "Apa Itu Relung Fundamental?", "id": "Seluruh Rentang Kondisi Bisa Dihuni." },
  { "en": "Apa Itu Relung Terealisasi?", "id": "Rentang Kondisi Yang Benar-Benar Ditempati." },
  { "en": "Apa Itu Prinsip Pengecualian Kompetitif?", "id": "Dua Spesies Tidak Bisa Hidup." },
  { "en": "Apa Itu Partisi Sumber Daya?", "id": "Pembagian Sumber Daya Kurangi Kompetisi." },
  { "en": "Apa Itu Pewarnaan Peringatan (Aposematisme)?", "id": "Warna Cerah Menandakan Bahaya Racun." },
  { "en": "Apa Itu Mimikri Batesian?", "id": "Spesies Tidak Berbahaya Meniru Berbahaya." },
  { "en": "Apa Itu Mimikri Mullerian?", "id": "Dua Spesies Berbahaya Saling Meniru." },
  { "en": "Apa Itu Klivaj (Pembelahan)?", "id": "Serangkaian Pembelahan Mitosis Sel Zigot." },
  { "en": "Apa Itu Blastomer?", "id": "Sel-Sel Hasil Dari Proses Klivaj." },
  { "en": "Apa Itu Korion?", "id": "Membran Terluar Yang Membungkus Embrio." },
  { "en": "Apa Itu Alantois?", "id": "Kantung Embrio Penyimpanan Sisa Metabolisme." },
  { "en": "Apa Itu Kantung Yolk (Kuning Telur)?", "id": "Sumber Nutrisi Utama Bagi Embrio." },
  { "en": "Apa Itu Holoblastik Klivaj?", "id": "Pembelahan Zigot Yang Terjadi Sempurna." },
  { "en": "Apa Itu Meroblastik Klivaj?", "id": "Pembelahan Zigot Tidak Terjadi Sempurna." },
  { "en": "Apa Itu Spermatogenesis?", "id": "Proses Pembentukan Sel Sperma Matang." },
  { "en": "Apa Itu Oogenesis?", "id": "Proses Pembentukan Sel Telur Matang." },
  { "en": "Apa Itu Badan Polar?", "id": "Sel Kecil Hasil Pembelahan Meiosis." },
  { "en": "Apa Fungsi Enzim Katalase?", "id": "Menguraikan Hidrogen Peroksida Menjadi Air." },
  { "en": "Apa Itu Hidrogen Peroksida (H2O2)?", "id": "Senyawa Beracun Hasil Metabolisme." },
  { "en": "Apa Itu Superoksida Dismutase?", "id": "Enzim Penetral Radikal Anion Superoksida." },
  { "en": "Apa Itu Siklooksigenase (COX)?", "id": "Enzim Target Obat Anti-Inflamasi." },
  { "en": "Apa Itu Fosfofruktokinase?", "id": "Enzim Kunci Pengatur Laju Glikolisis." },
  { "en": "Apa Itu Homeostasis Glukosa?", "id": "Pemeliharaan Kadar Gula Darah Stabil." },
  { "en": "Apa Itu Hipoglikemia?", "id": "Kondisi Kadar Gula Darah Rendah." },
  { "en": "Apa Itu Hiperglikemia?", "id": "Kondisi Kadar Gula Darah Tinggi." },
  { "en": "Apa Itu Glikogenolisis?", "id": "Proses Pemecahan Glikogen Menjadi Glukosa." },
  { "en": "Apa Itu Glikogenesis?", "id": "Proses Pembentukan Glikogen Dari Glukosa." },
  { "en": "Apa Itu Lipolisis?", "id": "Proses Pemecahan Lemak Menjadi Energi." },
  { "en": "Apa Itu Diuresis?", "id": "Peningkatan Laju Pembentukan Cairan Urine." },
  { "en": "Apa Itu Antidiuretik?", "id": "Zat Yang Dapat Menghambat Diuresis." },
  { "en": "Apa Itu Mikrobioma?", "id": "Komunitas Mikroba Di Lingkungan Tertentu." },
  { "en": "Apa Itu Mikrobiota Usus?", "id": "Komunitas Mikroba Di Saluran Pencernaan." },
  { "en": "Apa Itu Probiotik?", "id": "Mikroorganisme Hidup Yang Memberikan Manfaat." },
  { "en": "Apa Itu Prebiotik?", "id": "Senyawa Makanan Pendorong Pertumbuhan Probiotik." },
  { "en": "Apa Itu Quorum Sensing?", "id": "Sistem Komunikasi Bakteri Berdasarkan Kepadatan." },
  { "en": "Apa Itu Biofilm?", "id": "Komunitas Mikroba Melekat Permukaan." },
  { "en": "Apa Itu Organel?", "id": "Struktur Khusus Dalam Sel Eukariotik." },
  { "en": "Apa Itu Jaringan Dasar Tumbuhan?", "id": "Parenkim, Kolenkim, Dan Juga Sklerenkim." },
  { "en": "Apa Itu Selulosa Sintase?", "id": "Enzim Yang Mensintesis Serat Selulosa." },
  { "en": "Apa Itu Lignin?", "id": "Polimer Kompleks Pengeras Dinding Sel." },
  { "en": "Apa Fungsi Utama Lignin?", "id": "Memberikan Kekakuan Dan Kekuatan Struktural." },
  { "en": "Apa Itu Suberin?", "id": "Zat Lilin Kedap Air Tumbuhan." },
  { "en": "Apa Itu Eksositosis?", "id": "Proses Pelepasan Materi Dari Sel." },
  { "en": "Apa Itu Endositosis?", "id": "Proses Pemasukan Materi Ke Sel." },
  { "en": "Apa Itu Fagositosis?", "id": "Proses Sel Menelan Partikel Padat." },
  { "en": "Apa Itu Pinositosis?", "id": "Proses Sel Meminum Cairan Ekstraseluler." },
  { "en": "Apa Itu Anterograde Transport?", "id": "Transpor Dari Badan Sel Neuron." },
  { "en": "Apa Itu Retrograde Transport?", "id": "Transpor Menuju Badan Sel Neuron." },
  { "en": "Apa Itu Imunitas Bawaan (Innate Immunity)?", "id": "Respon Imun Non-Spesifik Sejak Lahir." },
  { "en": "Apa Itu Imunitas Adaptif (Acquired Immunity)?", "id": "Respon Imun Spesifik Yang Didapat." },
  { "en": "Apa Ciri Utama Imunitas Adaptif?", "id": "Spesifisitas Tinggi Dan Memiliki Memori." },
  { "en": "Apa Fungsi Sel T Helper?", "id": "Mengaktivasi Dan Mengoordinasikan Sel Imun." },
  { "en": "Apa Fungsi Sel B Plasma?", "id": "Sel B Yang Menghasilkan Antibodi." },
  { "en": "Apa Fungsi Sel B Memori?", "id": "Mengingat Antigen Respon Lebih Cepat." },
  { "en": "Apa Itu Histamin?", "id": "Zat Kimia Pemicu Reaksi Alergi." },
  { "en": "Apa Itu Makrofag?", "id": "Sel Fagosit Besar Di Jaringan." },
  { "en": "Apa Itu Neutrofil?", "id": "Jenis Sel Darah Putih Fagositik." },
  { "en": "Apa Itu Sistem Komplemen?", "id": "Sekelompok Protein Pelengkap Respon Imun." },
  { "en": "Apa Itu Determinasi Sel?", "id": "Komitmen Sel Pada Nasib Perkembangan." },
  { "en": "Apa Itu Morfogenesis?", "id": "Proses Perkembangan Bentuk Dan Struktur." },
  { "en": "Apa Itu Zona Pelusida?", "id": "Lapisan Luar Pelindung Sel Telur." },
  { "en": "Apa Itu Reaksi Akrosom?", "id": "Reaksi Sperma Menembus Sel Telur." },
  { "en": "Apa Itu Blok Polispermi?", "id": "Mekanisme Pencegah Pembuahan Banyak Sperma." },
  { "en": "Apa Itu Notokord?", "id": "Struktur Fleksibel Cikal Bakal Tulang." },
  { "en": "Apa Itu Tabung Saraf (Neural Tube)?", "id": "Struktur Cikal Bakal Sistem Saraf." },
  { "en": "Apa Itu Somit?", "id": "Blok Mesoderm Cikal Bakal Otot." },
  { "en": "Apa Itu Sel Punca Embrionik?", "id": "Sel Pluripoten Dari Massa Sel." },
  { "en": "Apa Itu Sel Punca Dewasa?", "id": "Sel Multipotensi Di Jaringan Dewasa." },
  { "en": "Apa Itu Pola Aksi Tetap?", "id": "Urutan Perilaku Naluriah Tidak Terpelajar." },
  { "en": "Apa Itu Stimulus Tanda?", "id": "Pemicu Eksternal Untuk Pola Aksi." },
  { "en": "Apa Itu Pembelajaran Operan?", "id": "Belajar Melalui Asosiasi Perilaku Konsekuensi." },
  { "en": "Siapa Pelopor Teori Pembelajaran Operan?", "id": "B.F. Skinner Dengan Kotak Skinner." },
  { "en": "Apa Itu Penguatan Positif?", "id": "Memberi Stimulus Menyenangkan Meningkatkan Perilaku." },
  { "en": "Apa Itu Pembelajaran Laten?", "id": "Belajar Tanpa Penguatan Langsung." },
  { "en": "Apa Itu Wawasan (Insight Learning)?", "id": "Pemecahan Masalah Tiba-Tiba Tanpa Coba-Coba." },
  { "en": "Apa Itu Perilaku Agonistik?", "id": "Perilaku Sosial Terkait Perkelahian." },
  { "en": "Apa Itu Energi Aktivasi?", "id": "Energi Minimum Untuk Memulai Reaksi." },
  { "en": "Bagaimana Enzim Menurunkan Energi Aktivasi?", "id": "Menyediakan Jalur Reaksi Alternatif." },
  { "en": "Apa Itu Model Kunci Gembok?", "id": "Model Interaksi Enzim Dan Substrat." },
  { "en": "Apa Itu Model Induced Fit?", "id": "Model Sisi Aktif Enzim Berubah." },
  { "en": "Apa Itu Kofaktor?", "id": "Komponen Non-Protein Dibutuhkan Enzim." },
  { "en": "Apa Contoh Kofaktor Anorganik?", "id": "Ion Logam Seperti Besi Seng." },
  { "en": "Apa Itu Denaturasi?", "id": "Kehilangan Struktur Tiga Dimensi Protein." },
  { "en": "Apa Itu Renaturasi?", "id": "Kembalinya Struktur Protein Setelah Denaturasi." },
  { "en": "Apa Itu Jaringan Parenkim?", "id": "Jaringan Dasar Dengan Banyak Fungsi." },
  { "en": "Apa Itu Jaringan Kolenkim?", "id": "Jaringan Penyokong Pada Organ Muda." },
  { "en": "Apa Itu Jaringan Sklerenkim?", "id": "Jaringan Penyokong Dinding Sel Tebal." },
  { "en": "Apa Dua Tipe Sel Sklerenkim?", "id": "Serat Atau Serabut Dan Sklereid." },
  { "en": "Apa Itu Sklereid (Sel Batu)?", "id": "Sel Sklerenkim Pendek Bentuk Bervariasi." },
  { "en": "Apa Itu Periderm?", "id": "Jaringan Pelindung Pengganti Epidermis Batang." },
  { "en": "Apa Itu Silinder Pusat (Stele)?", "id": "Bagian Tengah Akar Dan Batang." },
  { "en": "Apa Isi Silinder Pusat?", "id": "Xilem, Floem, Dan Juga Perisikel." },
  { "en": "Apa Itu Empulur?", "id": "Jaringan Parenkim Di Pusat Batang." },
  { "en": "Apa Itu Bunga Majemuk?", "id": "Kumpulan Banyak Bunga Satu Tangkai." },
  { "en": "Apa Itu Sistem Peredaran Limfatik?", "id": "Mengembalikan Cairan Ke Sirkulasi Darah." },
  { "en": "Apa Itu Edema?", "id": "Pembengkakan Akibat Penumpukan Cairan." },
  { "en": "Dari Mana Trombosit Berasal?", "id": "Berasal Dari Fragmen Sel Megakariosit." },
  { "en": "Apa Itu Fibrin?", "id": "Benang Protein Yang Membentuk Bekuan." },
  { "en": "Apa Itu Penyakit Hemofilia?", "id": "Penyakit Genetik Gangguan Pembekuan Darah." },
  { "en": "Apa Itu Bronkus?", "id": "Cabang Batang Tenggorokan Menuju Paru-Paru." },
  { "en": "Apa Itu Bronkiolus?", "id": "Cabang-Cabang Kecil Dari Organ Bronkus." },
  { "en": "Apa Itu Pleura?", "id": "Membran Rangkap Yang Membungkus Paru-Paru." },
  { "en": "Apa Itu Kepadatan Populasi?", "id": "Jumlah Individu Per Satuan Luas." },
  { "en": "Apa Itu Pola Sebaran Populasi?", "id": "Pola Penyebaran Individu Dalam Area." },
  { "en": "Sebutkan Tiga Pola Sebaran Utama?", "id": "Mengelompok, Seragam, Dan Juga Acak." },
  { "en": "Apa Itu Ilmu Demografi?", "id": "Studi Statistik Tentang Populasi Manusia." },
  { "en": "Apa Itu Tabel Kehidupan?", "id": "Ringkasan Kelangsungan Hidup Suatu Populasi." },
  { "en": "Apa Itu Kurva Survivorship?", "id": "Grafik Proporsi Individu Yang Bertahan." },
  { "en": "Apa Itu Pertumbuhan Eksponensial?", "id": "Pertumbuhan Populasi Dalam Kondisi Ideal." },
  { "en": "Apa Itu Pertumbuhan Logistik?", "id": "Pertumbuhan Populasi Dibatasi Faktor Lingkungan." },
  { "en": "Apa Itu Seleksi K?", "id": "Seleksi Sifat Menguntungkan Kepadatan Tinggi." },
  { "en": "Apa Itu Seleksi r?", "id": "Seleksi Sifat Memaksimalkan Laju Reproduksi." },
  { "en": "Apa Itu Taksonomi Linnaean?", "id": "Sistem Klasifikasi Hierarkis Carolus Linnaeus." },
  { "en": "Apa Itu Tingkatan Filum?", "id": "Tingkatan Taksonomi Di Bawah Kingdom." },
  { "en": "Apa Itu Tingkatan Kelas?", "id": "Tingkatan Taksonomi Di Bawah Filum." },
  { "en": "Apa Itu Tingkatan Ordo?", "id": "Tingkatan Taksonomi Di Bawah Kelas." },
  { "en": "Apa Itu Tingkatan Famili?", "id": "Tingkatan Taksonomi Di Bawah Ordo." },
  { "en": "Apa Itu Tingkatan Genus?", "id": "Tingkatan Taksonomi Di Bawah Famili." },
  { "en": "Apa Itu Hipotesis Nol?", "id": "Hipotesis Tidak Ada Efek Signifikan." },
  { "en": "Apa Itu Signifikansi Statistik?", "id": "Ukuran Kemungkinan Hasil Bukan Kebetulan." },
  { "en": "Apa Itu Korelasi?", "id": "Hubungan Statistik Antara Dua Variabel." },
  { "en": "Apa Itu Kausasi?", "id": "Satu Peristiwa Penyebab Peristiwa Lain." },
  { "en": "Apa Itu Tinjauan Sejawat (Peer Review)?", "id": "Evaluasi Karya Ilmiah Oleh Pakar." },
  { "en": "Apa Isi Teori Sel?", "id": "Semua Makhluk Hidup Terbuat Sel." },
  { "en": "Apa Itu Prinsip Biogenesis?", "id": "Prinsip Kehidupan Berasal Dari Kehidupan." },
  { "en": "Siapa Yang Membuktikan Prinsip Biogenesis?", "id": "Louis Pasteur Dengan Eksperimennya." },
  { "en": "Apa Itu Generasi Spontan?", "id": "Teori Kehidupan Muncul Benda Mati." },
  { "en": "Apa Itu Umbi Akar?", "id": "Akar Yang Membesar Menyimpan Makanan." },
  { "en": "Apa Itu Umbi Batang?", "id": "Batang Membesar Menyimpan Cadangan Makanan." },
  { "en": "Apa Itu Rizom (Rimpang)?", "id": "Batang Horizontal Tumbuh Bawah Tanah." },
  { "en": "Apa Itu Geragih (Stolon)?", "id": "Batang Horizontal Tumbuh Atas Tanah." },
  { "en": "Apa Itu Endosimbion?", "id": "Organisme Yang Hidup Dalam Organisme." },
  { "en": "Apa Itu Anoksigenik Fotosintesis?", "id": "Fotosintesis Yang Tidak Menghasilkan Oksigen." },
  { "en": "Siapa Yang Melakukan Fotosintesis Anoksigenik?", "id": "Bakteri Belerang Ungu Dan Hijau." },
  { "en": "Apa Itu Siklus Sel?", "id": "Serangkaian Peristiwa Pertumbuhan Pembelahan Sel." },
  { "en": "Apa Itu Titik Pengecekan Siklus Sel?", "id": "Titik Kontrol Pengatur Progresi Siklus." },
  { "en": "Apa Itu Siklin?", "id": "Protein Yang Mengatur Siklus Sel." },
  { "en": "Apa Itu Cyclin-Dependent Kinase (CDK)?", "id": "Enzim Diaktifkan Oleh Protein Siklin." },
  { "en": "Apa Itu Anafase Promoting Complex (APC)?", "id": "Kompleks Protein Pemicu Anafase Mitosis." },
  { "en": "Apa Itu Kinetokor?", "id": "Struktur Protein Di Sentromer Kromosom." },
  { "en": "Apa Fungsi Kinetokor?", "id": "Tempat Melekatnya Benang Spindel." },
  { "en": "Apa Itu Kohesi?", "id": "Gaya Tarik Menarik Molekul Air." },
  { "en": "Apa Itu Adhesi?", "id": "Gaya Tarik Air Permukaan Lain." },
  { "en": "Apa Itu Potensial Air?", "id": "Ukuran Energi Potensial Air." },
  { "en": "Air Bergerak Dari Potensial Apa?", "id": "Dari Potensial Air Tinggi Rendah." },
  { "en": "Apa Itu Plak Dinding Arteri?", "id": "Tumpukan Lemak Kolesterol Zat Lain." },
  { "en": "Apa Itu Trombus?", "id": "Gumpalan Darah Yang Terbentuk Pembuluh." },
  { "en": "Apa Itu Embolus?", "id": "Gumpalan Darah Bergerak Aliran Darah." },
  { "en": "Apa Itu Proses Autofagi?", "id": "Proses Sel Mendaur Ulang Komponennya." },
  { "en": "Apa Itu Vakuola Kontraktil?", "id": "Organel Pengatur Air Pada Protista." },
  { "en": "Apa Itu Dinding Sel Primer?", "id": "Dinding Sel Tipis Fleksibel Tumbuh." },
  { "en": "Apa Itu Dinding Sel Sekunder?", "id": "Dinding Sel Kaku Dalam Primer." },
  { "en": "Apa Itu Lamela Tengah?", "id": "Lapisan Perekat Antar Sel Tumbuhan." },
  { "en": "Apa Penyusun Utama Lamela Tengah?", "id": "Tersusun Dari Senyawa Bernama Pektin." },
  { "en": "Apa Itu Fluiditas Membran?", "id": "Kemampuan Molekul Bergerak Di Membran." },
  { "en": "Faktor Apa Pengaruhi Fluiditas Membran?", "id": "Suhu Dan Komposisi Asam Lemak." },
  { "en": "Apa Peran Kolesterol Di Membran?", "id": "Menjaga Fluiditas Membran Sel Hewan." },
  { "en": "Apa Itu Tubulus Seminiferus?", "id": "Saluran Testis Tempat Terjadi Spermatogenesis." },
  { "en": "Apa Fungsi Sel Sertoli?", "id": "Sel Penunjang Di Tubulus Seminiferus." },
  { "en": "Apa Fungsi Sel Leydig?", "id": "Sel Penghasil Hormon Testosteron." },
  { "en": "Apa Itu Epididimis?", "id": "Tempat Pematangan Dan Penyimpanan Sperma." },
  { "en": "Apa Itu Vas Deferens?", "id": "Saluran Yang Membawa Sperma." },
  { "en": "Apa Itu Folikel Ovarium?", "id": "Struktur Di Ovarium Berisi Oosit." },
  { "en": "Apa Itu Korpus Luteum?", "id": "Struktur Sisa Folikel Setelah Ovulasi." },
  { "en": "Apa Fungsi Korpus Luteum?", "id": "Menghasilkan Hormon Progesteron Dan Estrogen." },
  { "en": "Apa Fungsi Luteinizing Hormone (LH)?", "id": "Merangsang Ovulasi Dan Produksi Testosteron." },
  { "en": "Apa Fungsi Follicle-Stimulating Hormone (FSH)?", "id": "Merangsang Perkembangan Folikel Dan Sperma." },
  { "en": "Apa Itu Keanekaragaman Spesies?", "id": "Variasi Spesies Dalam Suatu Komunitas." },
  { "en": "Apa Itu Kekayaan Spesies?", "id": "Jumlah Total Spesies Yang Berbeda." },
  { "en": "Apa Itu Kelimpahan Relatif?", "id": "Proporsi Setiap Spesies Dalam Komunitas." },
  { "en": "Apa Itu Struktur Trofik?", "id": "Hubungan Makan-Memakan Dalam Komunitas." },
  { "en": "Apa Itu Hipotesis Gangguan Menengah?", "id": "Keanekaragaman Tertinggi Gangguan Sedang." },
  { "en": "Apa Itu Spesies Dominan?", "id": "Spesies Paling Melimpah Suatu Komunitas." },
  { "en": "Apa Itu Spesies Fondasi?", "id": "Spesies Yang Menciptakan Mengubah Habitat." },
  { "en": "Apa Itu Fasilitasi Dalam Suksesi?", "id": "Spesies Awal Mempermudah Spesies Lain." },
  { "en": "Apa Itu Inhibisi Dalam Suksesi?", "id": "Spesies Awal Menghambat Spesies Lain." },
  { "en": "Apa Itu Penyakit Zoonosis?", "id": "Penyakit Ditularkan Dari Hewan Manusia." },
  { "en": "Apa Itu Seludang Berkas Pembuluh?", "id": "Sel Pelindung Sekitar Jaringan Vaskular." },
  { "en": "Apa Itu Jaringan Mesofil?", "id": "Jaringan Dasar Daun Antara Epidermis." },
  { "en": "Apa Dua Jenis Jaringan Mesofil?", "id": "Jaringan Palisade Dan Jaringan Spons." },
  { "en": "Apa Itu Rizoid?", "id": "Struktur Mirip Akar Pada Lumut." },
  { "en": "Apa Itu Sporangium?", "id": "Struktur Yang Menghasilkan Spora." },
  { "en": "Apa Itu Sorus?", "id": "Kumpulan Sporangium Pada Daun Paku." },
  { "en": "Apa Itu Strobilus?", "id": "Struktur Kerucut Pembawa Sporangium." },
  { "en": "Apa Itu Hidrofili?", "id": "Penyerbukan Yang Dibantu Oleh Air." },
  { "en": "Apa Itu Anemofili?", "id": "Penyerbukan Yang Dibantu Oleh Angin." },
  { "en": "Apa Itu Entomofili?", "id": "Penyerbukan Yang Dibantu Oleh Serangga." },
  { "en": "Apa Itu Kiropterofili?", "id": "Penyerbukan Yang Dibantu Oleh Kelelawar." },
  { "en": "Apa Itu Inversi Parasentrik?", "id": "Inversi Kromosom Tidak Melibatkan Sentromer." },
  { "en": "Apa Itu Inversi Perisentrik?", "id": "Inversi Kromosom Yang Melibatkan Sentromer." },
  { "en": "Apa Itu Isokromosom?", "id": "Kromosom Dengan Dua Lengan Identik." },
  { "en": "Apa Itu Endoreduplikasi?", "id": "Replikasi Kromosom Tanpa Pembelahan Sel." },
  { "en": "Apa Itu Seleksi Penstabilan?", "id": "Menyeleksi Sifat Fenotipe Intermediet." },
  { "en": "Apa Itu Seleksi Direktional?", "id": "Menyeleksi Satu Fenotipe Ekstrem." },
  { "en": "Apa Itu Seleksi Disruptif?", "id": "Menyeleksi Dua Fenotipe Ekstrem." },
  { "en": "Apa Itu Seleksi Seksual?", "id": "Seleksi Sifat Tingkatkan Keberhasilan Kawin." },
  { "en": "Apa Itu Dimorfisme Seksual?", "id": "Perbedaan Penampilan Jantan Dan Betina." },
  { "en": "Apa Itu Hukum Pertama Termodinamika?", "id": "Energi Tidak Dapat Diciptakan Dihancurkan." },
  { "en": "Apa Itu Hukum Kedua Termodinamika?", "id": "Setiap Transfer Energi Meningkatkan Entropi." },
  { "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakteraturan Atau Keacakan." },
  { "en": "Apa Itu Energi Bebas Gibbs (G)?", "id": "Energi Yang Tersedia Melakukan Kerja." },
  { "en": "Apa Itu Reaksi Eksergonik?", "id": "Reaksi Yang Melepaskan Energi Bebas." },
  { "en": "Apa Itu Reaksi Endergonik?", "id": "Reaksi Yang Membutuhkan Energi Bebas." },
  { "en": "Apa Itu Kopling Energi?", "id": "Penggunaan Reaksi Eksergonik Mendorong Endergonik." },
  { "en": "Apa Itu Potensial Redoks?", "id": "Kecenderungan Molekul Mendapatkan Elektron." },
  { "en": "Apa Itu Proses Reduksi?", "id": "Proses Penambahan Atau Penerimaan Elektron." },
  { "en": "Apa Itu Proses Oksidasi?", "id": "Proses Pelepasan Atau Kehilangan Elektron." },
  { "en": "Apa Itu Kelenjar Sebasea?", "id": "Kelenjar Di Kulit Penghasil Sebum." },
  { "en": "Apa Itu Sebum?", "id": "Minyak Alami Pelumas Kulit Rambut." },
  { "en": "Apa Itu Sel Melanosit?", "id": "Sel Penghasil Pigmen Melanin." },
  { "en": "Apa Fungsi Pigmen Melanin?", "id": "Melindungi Kulit Dari Radiasi Ultraviolet." },
  { "en": "Apa Itu Diafisis?", "id": "Bagian Batang Panjang Tulang Pipa." },
  { "en": "Apa Itu Epifisis?", "id": "Bagian Ujung Membulat Tulang Pipa." },
  { "en": "Apa Itu Lempeng Epifisis?", "id": "Area Pertumbuhan Tulang Rawan." },
  { "en": "Apa Itu Sinus Paranasal?", "id": "Rongga Berisi Udara Di Tengkorak." },
  { "en": "Apa Itu Uvula?", "id": "Jaringan Kecil Menggantung Belakang Tenggorokan." },
  { "en": "Apa Itu Tonsil?", "id": "Jaringan Limfoid Sekitar Tenggorokan." },
  { "en": "Apa Itu Plasmid Ti?", "id": "Plasmid Bakteri Agrobacterium Tumefaciens." },
  { "en": "Untuk Apa Plasmid Ti Digunakan?", "id": "Sebagai Vektor Transfer Gen Tumbuhan." },
  { "en": "Apa Itu Agrobacterium Tumefaciens?", "id": "Bakteri Penyebab Penyakit Tumor Mahkota." },
  { "en": "Apa Itu Gene Gun?", "id": "Metode Transfer Gen Ke Sel." },
  { "en": "Apa Itu Elektroporasi?", "id": "Metode Membuat Pori Membran Sel." },
  { "en": "Apa Itu Complementary Deoxyribonucleic Acid (cDNA)?", "id": "Deoxyribonucleic Acid Disintesis Dari Cetakan Ribonucleic Acid." },
  { "en": "Apa Itu Pustaka Complementary Deoxyribonucleic Acid?", "id": "Koleksi Gen Yang Sedang Diekspresikan." },
  { "en": "Apa Itu Restriction Fragment Length Polymorphism?", "id": "Perbedaan Pola Fragmen Restriksi Deoxyribonucleic Acid." },
  { "en": "Apa Itu Real-Time Polymerase Chain Reaction?", "id": "Memantau Amplifikasi Deoxyribonucleic Acid Langsung." },
  { "en": "Apa Itu Mikrosatelit?", "id": "Urutan Deoxyribonucleic Acid Pendek Berulang." },
  { "en": "Apa Itu Morfogen?", "id": "Zat Pemberi Sinyal Posisi Sel." },
  { "en": "Apa Itu Blastopori?", "id": "Bukaan Pertama Terbentuk Saat Gastrulasi." },
  { "en": "Apa Itu Deuterostoma?", "id": "Blastopori Menjadi Anus, Contohnya Vertebrata." },
  { "en": "Apa Itu Protostoma?", "id": "Blastopori Menjadi Mulut, Contohnya Serangga." },
  { "en": "Apa Itu Selom?", "id": "Rongga Tubuh Dilapisi Jaringan Mesoderm." },
  { "en": "Apa Itu Hewan Aselomata?", "id": "Hewan Yang Tidak Memiliki Selom." },
  { "en": "Apa Itu Hewan Pseudoselomata?", "id": "Hewan Dengan Selom Semu." },
  { "en": "Apa Itu Hewan Euselomata?", "id": "Hewan Dengan Selom Sejati." },
  { "en": "Apa Itu Segmentasi Tubuh?", "id": "Pembagian Tubuh Menjadi Unit Berulang." },
  { "en": "Apa Itu Hemoglobin Glikosilasi (HbA1c)?", "id": "Ukuran Rata-Rata Gula Darah." },
  { "en": "Apa Itu Sfingter Esofagus?", "id": "Otot Cincin Antara Esofagus Lambung." },
  { "en": "Apa Itu Kardiak Output?", "id": "Volume Darah Dipompa Jantung Menit." },
  { "en": "Apa Itu Denyut Jantung Maksimal?", "id": "Denyut Jantung Tertinggi Saat Olahraga." },
  { "en": "Apa Itu Metabolisme Aerobik?", "id": "Produksi Energi Dengan Bantuan Oksigen." },
  { "en": "Apa Itu Metabolisme Anaerobik?", "id": "Produksi Energi Tanpa Bantuan Oksigen." },
  { "en": "Apa Fungsi Deoxyribonucleic Acid Ligase?", "id": "Menyambung Fragmen Deoxyribonucleic Acid Bersama." },
  { "en": "Siapa Penemu Fragmen Okazaki?", "id": "Reiji Dan Tsuneko Okazaki." },
  { "en": "Apa Itu Replisome?", "id": "Kompleks Protein Besar Garpu Replikasi." },
  { "en": "Apa Fungsi Protein Pengikat Untai Tunggal?", "id": "Menjaga Untai Deoxyribonucleic Acid Tetap Terpisah." },
  { "en": "Apa Fungsi Enzim Topoisomerase?", "id": "Mengatur Topologi Deoxyribonucleic Acid Superkoil." },
  { "en": "Apa Itu TATA Box?", "id": "Urutan Deoxyribonucleic Acid Promotor Eukariotik." },
  { "en": "Apa Itu Enhancer Dalam Genetika?", "id": "Urutan Deoxyribonucleic Acid Peningkat Laju Transkripsi." },
  { "en": "Apa Itu Silencer Dalam Genetika?", "id": "Urutan Deoxyribonucleic Acid Penekan Laju Transkripsi." },
  { "en": "Apa Itu Enzim Ribonucleic Acid Polimerase?", "id": "Enzim Utama Dalam Proses Transkripsi." },
  { "en": "Apa Itu Ribozim?", "id": "Molekul Ribonucleic Acid Berfungsi Enzim." },
  { "en": "Apa Fungsi Aminoasil-tRNA Sintetase?", "id": "Enzim Penempel Asam Amino tRNA." },
  { "en": "Apa Itu Poly-A Tail?", "id": "Ekor Poliadenin Di Ujung mRNA." },
  { "en": "Apa Itu 5' Cap?", "id": "Tudung Di Ujung 5' mRNA." },
  { "en": "Apa Itu Genom Mitokondria?", "id": "Materi Genetik Dalam Mitokondria." },
  { "en": "Dari Siapa Genom Mitokondria Diwariskan?", "id": "Hanya Diwariskan Dari Garis Ibu." },
  { "en": "Apa Itu Kontraksi Tetanik?", "id": "Kontraksi Otot Terus-Menerus Berkelanjutan." },
  { "en": "Apa Itu Rigor Mortis?", "id": "Kekakuan Otot Setelah Kematian." },
  { "en": "Apa Penyebab Rigor Mortis?", "id": "Ketiadaan Adenosine Triphosphate Melepas Miosin." },
  { "en": "Apa Itu Sistem Saraf Enterik?", "id": "Jaringan Neuron Pengontrol Saluran Pencernaan." },
  { "en": "Apa Arti Gelombang P EKG?", "id": "Menggambarkan Depolarisasi Atrium Jantung." },
  { "en": "Apa Arti Kompleks QRS EKG?", "id": "Menggambarkan Depolarisasi Ventrikel Jantung." },
  { "en": "Apa Arti Gelombang T EKG?", "id": "Menggambarkan Repolarisasi Ventrikel Jantung." },
  { "en": "Apa Fungsi Surfaktan Paru?", "id": "Cairan Pelapis Alveolus Cegah Kolaps." },
  { "en": "Di Mana Pusat Pernapasan Berada?", "id": "Terletak Di Medula Oblongata Pons." },
  { "en": "Apa Itu Sel Jukstaglomerular?", "id": "Sel Di Ginjal Penghasil Renin." },
  { "en": "Apa Fungsi Makula Densa?", "id": "Sel Pemantau Konsentrasi Garam Urine." },
  { "en": "Apa Itu Hormon Atrial Natriuretic Peptide?", "id": "Hormon Penurun Tekanan Darah." },
  { "en": "Apa Itu Koleoptil?", "id": "Selubung Pelindung Pucuk Tanaman Rumput." },
  { "en": "Apa Itu Koleoriza?", "id": "Selubung Pelindung Ujung Akar Rumput." },
  { "en": "Apa Itu Buah Partenokarpi?", "id": "Buah Yang Berkembang Tanpa Pembuahan." },
  { "en": "Apa Contoh Buah Partenokarpi?", "id": "Pisang Dan Beberapa Varietas Nanas." },
  { "en": "Apa Itu Kalus?", "id": "Massa Sel Tak Berdiferensiasi." },
  { "en": "Apa Itu Propagasi Vegetatif?", "id": "Perbanyakan Aseksual Pada Organisme Tumbuhan." },
  { "en": "Apa Itu Okulasi (Penempelan)?", "id": "Teknik Menggabungkan Mata Tunas." },
  { "en": "Apa Itu Penyambungan (Grafting)?", "id": "Teknik Menggabungkan Batang Atas Bawah." },
  { "en": "Apa Itu Bunga Banci (Hermafrodit)?", "id": "Bunga Dengan Putik Benang Sari." },
  { "en": "Apa Itu Tumbuhan Berumah Satu?", "id": "Bunga Jantan Betina Satu Tanaman." },
  { "en": "Apa Itu Tumbuhan Berumah Dua?", "id": "Bunga Jantan Betina Tanaman Berbeda." },
  { "en": "Apa Itu Prinsip Gause?", "id": "Nama Lain Prinsip Pengecualian Kompetitif." },
  { "en": "Kapan Pergeseran Karakter Sering Terjadi?", "id": "Saat Dua Spesies Hidup Bersama." },
  { "en": "Apa Itu Evolusi Divergen?", "id": "Spesies Berkerabat Menjadi Semakin Berbeda." },
  { "en": "Apa Pemicu Radiasi Adaptif?", "id": "Tersedianya Relung Ekologis Yang Baru." },
  { "en": "Apa Itu Spesies Kosmopolitan?", "id": "Spesies Yang Terdistribusi Secara Global." },
  { "en": "Apa Itu Komunitas Seral?", "id": "Tahap Transisi Dalam Proses Suksesi." },
  { "en": "Apa Itu Disclimax?", "id": "Komunitas Stabil Akibat Gangguan Berulang." },
  { "en": "Apa Itu Biogeografi Pulau?", "id": "Studi Faktor Penentu Keanekaragaman Pulau." },
  { "en": "Apa Itu Mikroevolusi?", "id": "Perubahan Frekuensi Alel Dalam Populasi." },
  { "en": "Apa Itu Makroevolusi?", "id": "Evolusi Pada Skala Atas Spesies." },
  { "en": "Apa Itu Isolasi Prezigotik?", "id": "Mekanisme Penghalang Pembuahan Sebelum Zigot." },
  { "en": "Apa Itu Isolasi Postzigotik?", "id": "Mekanisme Penghalang Setelah Zigot Terbentuk." },
  { "en": "Apa Contoh Isolasi Prezigotik?", "id": "Isolasi Habitat, Waktu, Dan Perilaku." },
  { "en": "Apa Contoh Isolasi Postzigotik?", "id": "Kematian Hibrida Atau Kemandulan Hibrida." },
  { "en": "Di Mana Sel Piala Banyak Ditemukan?", "id": "Di Saluran Pernapasan Dan Pencernaan." },
  { "en": "Apa Itu Kelenjar Lakrimal?", "id": "Kelenjar Yang Menghasilkan Air Mata." },
  { "en": "Apa Fungsi Utama Air Mata?", "id": "Melumasi Dan Membersihkan Bola Mata." },
  { "en": "Apa Nama Lain Untuk Saliva?", "id": "Saliva Adalah Nama Lain Air Liur." },
  { "en": "Apa Fungsi Enzim Lisozim?", "id": "Enzim Antibakteri Di Air Liur." },
  { "en": "Apa Itu Imunoglobulin A (IgA)?", "id": "Antibodi Utama Di Sekresi Mukosa." },
  { "en": "Apa Itu Haustra?", "id": "Kantung-Kantung Kecil Pada Usus Besar." },
  { "en": "Apa Itu Taenia Coli?", "id": "Tiga Pita Otot Usus Besar." },
  { "en": "Apa Itu Apendiks (Umbai Cacing)?", "id": "Kantung Kecil Terhubung Usus Besar." },
  { "en": "Apa Itu Sfingter Ani?", "id": "Otot Pengontrol Pengeluaran Zat Feses." },
  { "en": "Apa Itu Holotipe?", "id": "Spesimen Tunggal Acuan Penamaan Spesies." },
  { "en": "Apa Itu Paratipe?", "id": "Spesimen Lain Dideskripsikan Bersama Holotipe." },
  { "en": "Apa Itu Sinonim Dalam Taksonomi?", "id": "Dua Nama Berbeda Takson Sama." },
  { "en": "Apa Itu Homonim Dalam Taksonomi?", "id": "Satu Nama Untuk Dua Takson." },
  { "en": "Apa Itu Sentrifugasi Diferensial?", "id": "Pemisahan Organel Berdasarkan Ukuran Kecepatan." },
  { "en": "Apa Itu Reverse Transcription PCR (RT-PCR)?", "id": "Mengubah Ribonucleic Acid Menjadi Deoxyribonucleic Acid." },
  { "en": "Apa Itu Flow Cytometry?", "id": "Teknik Analisis Karakteristik Sel." },
  { "en": "Apa Itu Mikroskop Elektron Transmisi?", "id": "Mikroskop Menggunakan Elektron Menembus Spesimen." },
  { "en": "Apa Itu Mikroskop Elektron Payar?", "id": "Mikroskop Melihat Permukaan Tiga Dimensi." },
  { "en": "Apa Fungsi Spektrofotometer?", "id": "Alat Pengukur Absorbansi Cahaya Larutan." },
  { "en": "Di Mana Glikogen Terutama Disimpan?", "id": "Di Dalam Organ Hati Otot." },
  { "en": "Apa Itu Amilosa Dan Amilopektin?", "id": "Dua Komponen Utama Penyusun Pati." },
  { "en": "Apa Monomer Penyusun Molekul Kitin?", "id": "Monomernya Adalah N-Asetilglukosamin." },
  { "en": "Di Mana Proses Glikolisis Terjadi?", "id": "Terjadi Di Dalam Sitosol Sel." },
  { "en": "Apa Beda Fermentasi Alkohol Laktat?", "id": "Produk Akhir Yang Dihasilkan Berbeda." },
  { "en": "Apa Nama Lain Siklus Krebs?", "id": "Siklus Asam Sitrat." },
  { "en": "Apa Itu Proses Fosforilasi?", "id": "Proses Penambahan Gugus Fosfat." },
  { "en": "Apa Itu Proses Defosforilasi?", "id": "Proses Pelepasan Gugus Fosfat." },
  { "en": "Apa Contoh Mekanisme Homeostasis?", "id": "Pengaturan Suhu Tubuh Gula Darah." },
  { "en": "Apa Itu Gerak Ameboid?", "id": "Gerakan Merayap Seperti Sel Amoeba." },
  { "en": "Apa Itu Aliran Sitoplasma?", "id": "Pergerakan Sitoplasma Di Dalam Sel." },
  { "en": "Apa Itu Isomer?", "id": "Molekul Rumus Sama Struktur Berbeda." },
  { "en": "Apa Itu Enantiomer?", "id": "Stereoisomer Merupakan Bayangan Cermin." },
  { "en": "Apa Itu Gugus Fungsional?", "id": "Kelompok Atom Penentu Sifat Molekul." },
  { "en": "Apa Itu Gugus Hidroksil (-OH)?", "id": "Gugus Fungsional Pada Molekul Alkohol." },
  { "en": "Apa Itu Gugus Karboksil (-COOH)?", "id": "Gugus Fungsional Asam Karboksilat." },
  { "en": "Apa Itu Gugus Amino (-NH2)?", "id": "Gugus Fungsional Pada Molekul Amina." },
  { "en": "Apa Itu Ikatan Hidrogen?", "id": "Gaya Tarik Antar Molekul Lemah." },
  { "en": "Apa Itu Ikatan Kovalen?", "id": "Ikatan Kimia Kuat Berbagi Elektron." },
  { "en": "Apa Itu Ikatan Ionik?", "id": "Ikatan Akibat Serah Terima Elektron." },
  { "en": "Apa Itu Molekul Polar?", "id": "Molekul Distribusi Muatan Tidak Merata." },
  { "en": "Apa Itu Molekul Nonpolar?", "id": "Molekul Distribusi Muatan Merata." },
  { "en": "Apa Itu Sifat Hidrofobik?", "id": "Sifat 'Takut Air' Molekul Nonpolar." },
  { "en": "Apa Itu Sifat Hidrofilik?", "id": "Sifat 'Suka Air' Molekul Polar." },
  { "en": "Apa Itu Senyawa Amfipatik?", "id": "Memiliki Bagian Hidrofilik Dan Hidrofobik." },
  { "en": "Apa Contoh Senyawa Amfipatik?", "id": "Fosfolipid Dan Juga Sabun." }



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
