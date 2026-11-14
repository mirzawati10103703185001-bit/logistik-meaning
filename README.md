<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Font Awesome untuk Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Inter --><!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart-Growth Lab - Virtual Laboratory Pertumbuhan & Perkembangan Tumbuhan</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js CDN untuk Grafik -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#ecfdf5',
                            100: '#d1fae5',
                            500: '#10b981',
                            600: '#059669',
                            700: '#047857',
                            800: '#065f46',
                            900: '#064e3b',
                        }
                    }
                }
            }
        }
    </script>

    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f5f9;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #10b981;
            border-radius: 4px;
        }
        .tab-btn.active {
            border-bottom: 3px solid #34d399;
            color: #ffffff;
            font-weight: 600;
            background-color: rgba(255, 255, 255, 0.1);
        }
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        @media print {
            header, footer, #section-lab, #section-teori, #section-kuis, .no-print {
                display: none !important;
            }
            #section-data {
                display: block !important;
            }
        }
    </style>
</head>
<body class="flex flex-col min-h-screen text-slate-800 bg-slate-100">

    <!-- TOAST NOTIFICATION (Pengganti Alert) -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- HEADER & NAVBAR -->
    <header class="bg-gradient-to-r from-emerald-900 via-teal-800 to-emerald-900 text-white shadow-lg sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-20">
                <!-- Brand Title -->
                <div class="flex items-center space-x-3">
                    <div class="bg-emerald-500/20 p-2.5 rounded-2xl backdrop-blur-md border border-emerald-400/30 shadow-inner">
                        <i class="fa-solid fa-seedling text-emerald-300 text-2xl sm:text-3xl"></i>
                    </div>
                    <div>
                        <h1 class="text-lg sm:text-2xl font-extrabold tracking-tight text-white flex items-center gap-2">
                            Smart-Growth Lab
                            <span class="text-[10px] sm:text-xs font-bold bg-emerald-500/30 text-emerald-200 px-2.5 py-0.5 rounded-full border border-emerald-400/30">VLab v2.0</span>
                        </h1>
                        <p class="text-xs text-emerald-100/90 font-normal hidden sm:block">
                            Laboratorium Virtual Pertumbuhan & Perkembangan Tumbuhan
                        </p>
                    </div>
                </div>

                <!-- Teacher Profile Info -->
                <div class="hidden md:flex items-center space-x-3 bg-emerald-950/50 px-4 py-2 rounded-2xl border border-emerald-500/20">
                    <div class="w-9 h-9 rounded-full bg-gradient-to-br from-emerald-400 to-teal-600 flex items-center justify-center font-bold text-white shadow-md">
                        M
                    </div>
                    <div class="text-right">
                        <div class="text-[10px] text-emerald-300 uppercase tracking-wider">Pengembang / Guru:</div>
                        <div class="text-xs font-bold text-white">Mirzawati, S.Pd.</div>
                        <div class="text-[10px] text-emerald-200">MTsN 5 Bireuen</div>
                    </div>
                </div>
            </div>

            <!-- Navigation Tabs -->
            <nav class="flex space-x-1 sm:space-x-2 border-t border-emerald-700/50 overflow-x-auto no-scrollbar">
                <button onclick="switchTab('lab')" id="tab-lab" class="tab-btn active px-4 py-3 text-xs sm:text-sm font-medium text-white hover:bg-white/5 rounded-t-lg transition flex items-center gap-2 whitespace-nowrap">
                    <i class="fa-solid fa-flask"></i> Laboratorium Virtual
                </button>
                <button onclick="switchTab('teori')" id="tab-teori" class="tab-btn px-4 py-3 text-xs sm:text-sm font-medium text-emerald-100 hover:bg-white/5 rounded-t-lg transition flex items-center gap-2 whitespace-nowrap">
                    <i class="fa-solid fa-book-open"></i> Panduan & Teori
                </button>
                <button onclick="switchTab('data')" id="tab-data" class="tab-btn px-4 py-3 text-xs sm:text-sm font-medium text-emerald-100 hover:bg-white/5 rounded-t-lg transition flex items-center gap-2 whitespace-nowrap">
                    <i class="fa-solid fa-chart-line"></i> Data Logger & Grafik
                </button>
                <button onclick="switchTab('kuis')" id="tab-kuis" class="tab-btn px-4 py-3 text-xs sm:text-sm font-medium text-emerald-100 hover:bg-white/5 rounded-t-lg transition flex items-center gap-2 whitespace-nowrap">
                    <i class="fa-solid fa-list-check"></i> Evaluasi & Kuis
                </button>
            </nav>
        </div>
    </header>

    <!-- MAIN CONTENT AREA -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-3 sm:px-6 lg:px-8 py-6">

        <!-- ================= SECTION 1: SIMULASI VIRTUAL LAB ================= -->
        <section id="section-lab" class="space-y-6">
            
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                
                <!-- CONTROL PANEL (KIRI) -->
                <div class="lg:col-span-4 bg-white rounded-2xl shadow-sm border border-slate-200/80 p-5 space-y-4">
                    <div class="border-b border-slate-100 pb-3 flex items-center justify-between">
                        <h2 class="text-base sm:text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i class="fa-solid fa-sliders text-emerald-600"></i> Kontrol Variabel
                        </h2>
                        <span class="text-[11px] bg-emerald-50 text-emerald-700 border border-emerald-200 px-2.5 py-0.5 rounded-full font-semibold">Eksperimen</span>
                    </div>

                    <!-- Input 1: Jenis Biji -->
                    <div>
                        <label for="param-biji" class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">
                            1. Jenis Biji (Tipe Perkecambahan)
                        </label>
                        <select id="param-biji" onchange="resetSimulasi()" class="w-full bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl p-2.5 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 transition outline-none">
                            <option value="kacang_hijau">Kacang Hijau (Perkecambahan Epigeal)</option>
                            <option value="jagung">Jagung (Perkecambahan Hipogeal)</option>
                        </select>
                    </div>

                    <!-- Input 2: Intensitas Cahaya -->
                    <div>
                        <label for="param-cahaya" class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">
                            2. Intensitas Cahaya
                        </label>
                        <select id="param-cahaya" onchange="resetSimulasi()" class="w-full bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl p-2.5 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 transition outline-none">
                            <option value="gelap">Gelap / Tanpa Cahaya (Kondisi Etiolasi)</option>
                            <option value="remang">Remang-remang (Cahaya Sedang)</option>
                            <option value="terang" selected>Terang (Cahaya Optimal / Matahari)</option>
                        </select>
                    </div>

                    <!-- Input 3: Kelembaban / Air -->
                    <div>
                        <label for="param-air" class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">
                            3. Kadar Air / Kelembaban Media
                        </label>
                        <select id="param-air" onchange="resetSimulasi()" class="w-full bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl p-2.5 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 transition outline-none">
                            <option value="kering">Kering / Sangat Minim Air (Rendah)</option>
                            <option value="sedang" selected>Lembab / Cukup Air (Optimal)</option>
                            <option value="tinggi">Tergenang Air (Berlebih/Anaerob)</option>
                        </select>
                    </div>

                    <!-- Input 4: Suhu Lingkungan -->
                    <div>
                        <label for="param-suhu" class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">
                            4. Suhu Lingkungan (°C)
                        </label>
                        <select id="param-suhu" onchange="resetSimulasi()" class="w-full bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl p-2.5 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 transition outline-none">
                            <option value="dingin">Suhu Dingin (10 °C)</option>
                            <option value="optimal" selected>Suhu Optimal (25 °C - 30 °C)</option>
                            <option value="panas">Suhu Panas / Ekstrem (42 °C)</option>
                        </select>
                    </div>

                    <!-- Input 5: Nutrisi / Pupuk -->
                    <div>
                        <label for="param-nutrisi" class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">
                            5. Dosis Nutrisi (NPK)
                        </label>
                        <select id="param-nutrisi" onchange="resetSimulasi()" class="w-full bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl p-2.5 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 transition outline-none">
                            <option value="tanpa">Tanpa Nutrisi Tambahan</option>
                            <option value="cukup" selected>Dosis NPK Cukup (Optimal)</option>
                            <option value="berlebih">Dosis NPK Berlebih (Toksisitas)</option>
                        </select>
                    </div>

                    <!-- TOMBOL SIMULASI -->
                    <div class="pt-3 border-t border-slate-100 space-y-2.5">
                        <div class="grid grid-cols-2 gap-2.5">
                            <button id="btn-start" onclick="toggleSimulasi()" class="w-full bg-emerald-600 hover:bg-emerald-700 active:scale-95 text-white font-semibold py-3 px-4 rounded-xl shadow-md shadow-emerald-600/20 transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-play" id="icon-start"></i> <span id="text-start">Mulai</span>
                            </button>
                            <button onclick="resetSimulasi()" class="w-full bg-slate-100 hover:bg-slate-200 active:scale-95 text-slate-700 font-semibold py-3 px-4 rounded-xl border border-slate-200 transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-rotate-left"></i> Reset
                            </button>
                        </div>
                        
                        <!-- Speed Control -->
                        <div class="flex items-center justify-between bg-slate-50 p-2.5 rounded-xl border border-slate-200">
                            <span class="text-xs font-semibold text-slate-600"><i class="fa-solid fa-gauge-high text-emerald-600 mr-1"></i> Kecepatan:</span>
                            <div class="flex gap-1">
                                <button onclick="setSpeed(1000)" id="spd-1" class="px-3 py-1 text-xs rounded-lg font-bold bg-emerald-600 text-white transition">1x</button>
                                <button onclick="setSpeed(500)" id="spd-2" class="px-3 py-1 text-xs rounded-lg font-bold bg-slate-200 text-slate-700 hover:bg-slate-300 transition">2x</button>
                                <button onclick="setSpeed(250)" id="spd-3" class="px-3 py-1 text-xs rounded-lg font-bold bg-slate-200 text-slate-700 hover:bg-slate-300 transition">4x</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- ANIMASI & REALTIME MONITOR (KANAN) -->
                <div class="lg:col-span-8 flex flex-col space-y-6">
                    
                    <!-- CANVAS SIMULASI -->
                    <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-4 relative overflow-hidden flex flex-col items-center">
                        <!-- Top Bar Status -->
                        <div class="w-full flex flex-wrap justify-between items-center gap-2 mb-3 px-1">
                            <div class="flex items-center gap-2">
                                <span id="badge-status" class="px-3 py-1 rounded-full text-xs font-bold bg-slate-100 text-slate-700 flex items-center gap-1.5">
                                    <span class="w-2 h-2 rounded-full bg-slate-400"></span> Siap Dijalankan
                                </span>
                            </div>
                            <div class="flex items-center gap-3">
                                <div class="text-right">
                                    <span class="text-xs text-slate-500 font-medium">Waktu:</span>
                                    <span id="display-hari" class="text-base sm:text-lg font-black text-emerald-700 ml-1">Hari ke-0</span>
                                    <span class="text-xs text-slate-400">/ 14</span>
                                </div>
                            </div>
                        </div>

                        <!-- Progress Bar Hari -->
                        <div class="w-full bg-slate-100 h-1.5 rounded-full overflow-hidden mb-3">
                            <div id="day-progress" class="bg-emerald-500 h-full transition-all duration-300" style="width: 0%"></div>
                        </div>

                        <!-- Canvas Workspace -->
                        <div id="canvas-wrapper" class="w-full relative bg-slate-900 rounded-xl overflow-hidden shadow-inner border border-slate-800 flex justify-center items-center" style="height: 380px;">
                            <canvas id="growthCanvas" class="w-full h-full block"></canvas>
                            
                            <!-- Overlay Text Banner ketika tanaman mati/etiolasi parah -->
                            <div id="canvas-overlay" class="hidden absolute inset-0 bg-slate-950/70 backdrop-blur-sm flex items-center justify-center p-4">
                                <div class="bg-white px-6 py-5 rounded-2xl shadow-2xl text-center max-w-sm border-2 border-red-200">
                                    <div class="w-12 h-12 rounded-full bg-red-100 text-red-600 flex items-center justify-center mx-auto mb-3 text-2xl">
                                        <i id="overlay-icon" class="fa-solid fa-skull-crossbones"></i>
                                    </div>
                                    <h4 id="overlay-title" class="font-bold text-slate-900 text-base">Status Pertumbuhan</h4>
                                    <p id="overlay-desc" class="text-xs text-slate-600 mt-1.5 leading-relaxed">Deskripsi status.</p>
                                    <button onclick="resetSimulasi()" class="mt-4 px-4 py-2 bg-slate-800 hover:bg-slate-900 text-white text-xs font-semibold rounded-xl transition">
                                        Ulangi Percobaan
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- REAL-TIME TELEMETRY PANEL -->
                    <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-5">
                        <div class="flex items-center justify-between mb-3">
                            <h3 class="text-xs font-bold text-slate-400 uppercase tracking-wider">
                                Parameter Observasi Real-Time
                            </h3>
                            <span id="telemetry-seed-tag" class="text-[11px] font-semibold bg-emerald-50 text-emerald-700 px-2.5 py-0.5 rounded-md">
                                Epigeal (Kacang Hijau)
                            </span>
                        </div>
                        <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 sm:gap-4">
                            <!-- High Stem -->
                            <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200/60">
                                <div class="text-xs text-slate-500 font-medium mb-1"><i class="fa-solid fa-ruler-vertical text-emerald-600 mr-1"></i> Tinggi Batang</div>
                                <div id="stat-tinggi" class="text-xl font-extrabold text-slate-800">0.0 <span class="text-xs font-normal text-slate-500">cm</span></div>
                            </div>

                            <!-- Leaf Count -->
                            <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200/60">
                                <div class="text-xs text-slate-500 font-medium mb-1"><i class="fa-solid fa-leaf text-emerald-600 mr-1"></i> Jumlah Daun</div>
                                <div id="stat-daun" class="text-xl font-extrabold text-slate-800">0 <span class="text-xs font-normal text-slate-500">helai</span></div>
                            </div>

                            <!-- Leaf Color -->
                            <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200/60">
                                <div class="text-xs text-slate-500 font-medium mb-1"><i class="fa-solid fa-palette text-emerald-600 mr-1"></i> Warna Daun</div>
                                <div id="stat-warna" class="text-xs sm:text-sm font-bold text-slate-800 truncate">-</div>
                            </div>

                            <!-- Root Condition -->
                            <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200/60">
                                <div class="text-xs text-slate-500 font-medium mb-1"><i class="fa-solid fa-seedling text-emerald-600 mr-1"></i> Kondisi Akar</div>
                                <div id="stat-akar" class="text-xs sm:text-sm font-bold text-slate-800 truncate">-</div>
                            </div>
                        </div>

                        <!-- Ringkasan Fenomena Fisiologis -->
                        <div class="mt-4 p-3.5 bg-emerald-50/80 rounded-xl border border-emerald-200/70 text-xs text-emerald-950 flex items-start gap-3">
                            <i class="fa-solid fa-microscope text-emerald-600 text-lg mt-0.5"></i>
                            <div class="leading-relaxed">
                                <span class="font-bold text-emerald-900">Analisis Fisiologis: </span>
                                <span id="stat-fisiologi">Silakan klik tombol Mulai untuk mengamati proses imbibisi dan pertumbuhan.</span>
                            </div>
                        </div>
                    </div>

                </div>

            </div>

        </section>


        <!-- ================= SECTION 2: PANDUAN & TEORI ================= -->
        <section id="section-teori" class="hidden space-y-6">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 md:p-8 space-y-8">
                
                <!-- Title Header -->
                <div class="border-b border-slate-100 pb-4">
                    <h2 class="text-xl sm:text-2xl font-bold text-slate-900 flex items-center gap-3">
                        <i class="fa-solid fa-book-bookmark text-emerald-600"></i> Landasan Teori Pertumbuhan & Perkembangan
                    </h2>
                    <p class="text-xs sm:text-sm text-slate-500 mt-1">
                        Materi Biologi Fase F / Kelas XII - Tipe Perkecambahan & Faktor Eksternal Pertumbuhan
                    </p>
                </div>

                <!-- Sub-bagian 1: Epigeal vs Hipogeal -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    
                    <!-- Epigeal Card -->
                    <div class="bg-emerald-50/60 rounded-2xl p-6 border border-emerald-200/80">
                        <div class="flex items-center gap-3 mb-3">
                            <span class="w-8 h-8 rounded-xl bg-emerald-600 text-white flex items-center justify-center font-bold text-sm shadow-sm">1</span>
                            <h3 class="text-lg font-bold text-emerald-950">Perkecambahan Epigeal</h3>
                        </div>
                        <p class="text-xs sm:text-sm text-slate-700 leading-relaxed mb-4">
                            Perkecambahan di mana <strong>hipokotil memanjang</strong> sehingga mengangkat kotiledon (keping biji) dan plumula (calon daun) ke <strong>atas permukaan tanah</strong>.
                        </p>
                        <div class="bg-white p-3.5 rounded-xl border border-emerald-200 text-xs text-slate-600 space-y-1.5">
                            <div><strong class="text-emerald-800">Contoh Tumbuhan:</strong> Kacang Hijau (<em>Vigna radiata</em>), Kacang Tanah, Melon.</div>
                            <div><strong class="text-emerald-800">Ciri Khas:</strong> Kotiledon terangkat ke udara, berfotosintesis sementara, lalu menyusut setelah cadangan makanan habis.</div>
                        </div>
                    </div>

                    <!-- Hipogeal Card -->
                    <div class="bg-teal-50/60 rounded-2xl p-6 border border-teal-200/80">
                        <div class="flex items-center gap-3 mb-3">
                            <span class="w-8 h-8 rounded-xl bg-teal-600 text-white flex items-center justify-center font-bold text-sm shadow-sm">2</span>
                            <h3 class="text-lg font-bold text-teal-950">Perkecambahan Hipogeal</h3>
                        </div>
                        <p class="text-xs sm:text-sm text-slate-700 leading-relaxed mb-4">
                            Perkecambahan di mana <strong>epikotil memanjang</strong> sehingga plumula menembus ke atas tanah, sedangkan kotiledon <strong>tetap tertinggal di dalam tanah</strong>.
                        </p>
                        <div class="bg-white p-3.5 rounded-xl border border-teal-200 text-xs text-slate-600 space-y-1.5">
                            <div><strong class="text-teal-800">Contoh Tumbuhan:</strong> Jagung (<em>Zea mays</em>), Kacang Kapri, Padi.</div>
                            <div><strong class="text-teal-800">Ciri Khas:</strong> Kotiledon tetap berada di bawah permukaan tanah hingga cadangan makanannya habis diserap embrio.</div>
                        </div>
                    </div>

                </div>

                <!-- Sub-bagian 2: Faktor Eksternal -->
                <div>
                    <h3 class="text-base sm:text-lg font-bold text-slate-800 mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-sun text-amber-500"></i> Pengaruh Faktor Eksternal Lingkungan
                    </h3>

                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        
                        <!-- Cahaya & Etiolasi -->
                        <div class="p-4 bg-slate-50 rounded-xl border border-slate-200">
                            <h4 class="font-bold text-slate-800 text-sm mb-2 flex items-center gap-2">
                                <i class="fa-solid fa-moon text-indigo-500"></i> Cahaya & Etiolasi
                            </h4>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Cahaya menghambat kerja hormon <strong>Auksin</strong>. Di tempat gelap, auksin memicu pemanjangan sel ekstrem (<strong>etiolasi</strong>): batang sangat tinggi, pucat, dan lemah.
                            </p>
                        </div>

                        <!-- Kelembaban / Air -->
                        <div class="p-4 bg-slate-50 rounded-xl border border-slate-200">
                            <h4 class="font-bold text-slate-800 text-sm mb-2 flex items-center gap-2">
                                <i class="fa-solid fa-droplet text-blue-500"></i> Air (Imbibisi)
                            </h4>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Air memicu proses <strong>imbibisi</strong> untuk mengaktifkan enzim metabolik (amilase). Tanpa air biji tetap dorman; air tergenang memicu kebusukan akar (anaerob).
                            </p>
                        </div>

                        <!-- Suhu -->
                        <div class="p-4 bg-slate-50 rounded-xl border border-slate-200">
                            <h4 class="font-bold text-slate-800 text-sm mb-2 flex items-center gap-2">
                                <i class="fa-solid fa-temperature-half text-rose-500"></i> Suhu Lingkungan
                            </h4>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Enzim bekerja optimum pada suhu 25°C–30°C. Suhu dingin (10°C) memperlambat kerja enzim, sedangkan suhu ekstrem (>40°C) menyebabkan denaturasi enzim.
                            </p>
                        </div>

                        <!-- Nutrisi NPK -->
                        <div class="p-4 bg-slate-50 rounded-xl border border-slate-200">
                            <h4 class="font-bold text-slate-800 text-sm mb-2 flex items-center gap-2">
                                <i class="fa-solid fa-flask-vial text-emerald-500"></i> Nutrisi (NPK)
                            </h4>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Unsur N, P, dan K penting untuk pembentukan klorofil dan jaringan baru. Dosis berlebih meningkatkan tekanan osmotik tanah sehingga akar mengalami plasmolisis.
                            </p>
                        </div>

                    </div>
                </div>

                <!-- Langkah Penggunaan Lab Virtual -->
                <div class="bg-slate-900 text-white rounded-2xl p-6">
                    <h3 class="text-base font-bold text-emerald-400 mb-3 flex items-center gap-2">
                        <i class="fa-solid fa-circle-play"></i> Panduan Praktikum Virtual
                    </h3>
                    <ol class="list-decimal list-inside text-xs sm:text-sm text-slate-300 space-y-2">
                        <li>Buka tab <strong>Laboratorium Virtual</strong> dan atur kombinasi variabel bebas pada <strong>Panel Kontrol</strong>.</li>
                        <li>Tekan tombol <strong>Mulai</strong> untuk menjalankan simulasi selama 14 hari pengamatan.</li>
                        <li>Amati perubahan morfologi batang, posisi kotiledon, warna daun, dan analisis fisiologis secara real-time.</li>
                        <li>Buka tab <strong>Data Logger & Grafik</strong> untuk melihat kurva pertumbuhan dan menyalin atau mencetak tabel hasil pengamatan.</li>
                        <li>Kerjakan soal pada tab <strong>Evaluasi & Kuis</strong> untuk menguji pemahaman konsep Anda.</li>
                    </ol>
                </div>

            </div>
        </section>


        <!-- ================= SECTION 3: TABEL & GRAFIK (DATA LOGGER) ================= -->
        <section id="section-data" class="hidden space-y-6">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-4 sm:p-5 flex flex-wrap items-center justify-between gap-3">
                <div>
                    <h2 class="text-base sm:text-lg font-bold text-slate-800">Laporan Hasil Pengamatan Praktikum</h2>
                    <p id="report-params-summary" class="text-xs text-slate-500 mt-0.5">Parameter: Kacang Hijau | Cahaya Terang | Air Lembab | Suhu Optimal | NPK Cukup</p>
                </div>
                <div class="flex gap-2 no-print">
                    <button onclick="copyTableData()" class="px-3.5 py-2 text-xs bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold rounded-xl border border-slate-200 transition flex items-center gap-1.5">
                        <i class="fa-solid fa-copy"></i> Salin Data
                    </button>
                    <button onclick="window.print()" class="px-3.5 py-2 text-xs bg-emerald-600 hover:bg-emerald-700 text-white font-semibold rounded-xl shadow-sm transition flex items-center gap-1.5">
                        <i class="fa-solid fa-print"></i> Cetak Laporan
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                
                <!-- GRAFIK CHART.JS (LEFT/TOP) -->
                <div class="lg:col-span-7 bg-white rounded-2xl shadow-sm border border-slate-200 p-5 sm:p-6 space-y-4">
                    <div class="flex items-center justify-between border-b border-slate-100 pb-3">
                        <div>
                            <h3 class="text-base font-bold text-slate-800">Kurva Pertumbuhan Tinggi Tanaman</h3>
                            <p class="text-xs text-slate-500">Grafik Dinamis (Tinggi Batang vs Hari Pengamatan)</p>
                        </div>
                        <span class="text-xs font-semibold bg-emerald-100 text-emerald-800 px-2.5 py-1 rounded-full">Chart.js Live</span>
                    </div>

                    <div class="relative w-full h-72 sm:h-80">
                        <canvas id="growthChart"></canvas>
                    </div>
                </div>

                <!-- DATA LOGGER TABLE (RIGHT/BOTTOM) -->
                <div class="lg:col-span-5 bg-white rounded-2xl shadow-sm border border-slate-200 p-5 sm:p-6 flex flex-col space-y-4">
                    <div class="flex items-center justify-between border-b border-slate-100 pb-3">
                        <div>
                            <h3 class="text-base font-bold text-slate-800">Tabel Rekap Data Harian</h3>
                            <p class="text-xs text-slate-500">Data kuantitatif & kualitatif terukur</p>
                        </div>
                    </div>

                    <!-- Scrollable Table -->
                    <div class="flex-grow overflow-y-auto max-h-80 border border-slate-200 rounded-xl custom-scrollbar">
                        <table class="w-full text-left text-xs text-slate-700" id="data-table">
                            <thead class="bg-slate-100 text-slate-800 uppercase font-bold sticky top-0">
                                <tr>
                                    <th class="p-2.5">Hari</th>
                                    <th class="p-2.5">Tinggi (cm)</th>
                                    <th class="p-2.5">Daun</th>
                                    <th class="p-2.5">Kondisi / Warna</th>
                                </tr>
                            </thead>
                            <tbody id="table-body" class="divide-y divide-slate-100">
                                <tr>
                                    <td colspan="4" class="text-center p-4 text-slate-400 italic">Belum ada data pengamatan. Jalankan simulasi terlebih dahulu.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

            </div>
        </section>


        <!-- ================= SECTION 4: EVALUASI & KUIS ================= -->
        <section id="section-kuis" class="hidden space-y-6">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 sm:p-8 max-w-4xl mx-auto">
                
                <!-- Kuis Header -->
                <div class="border-b border-slate-100 pb-4 mb-6 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="text-xl sm:text-2xl font-bold text-slate-900 flex items-center gap-3">
                            <i class="fa-solid fa-clipboard-question text-emerald-600"></i> Evaluasi Pemahaman Konsep
                        </h2>
                        <p class="text-xs sm:text-sm text-slate-500 mt-1">
                            Uji pemahaman Anda mengenai fenomena etiolasi, tipe perkecambahan, dan faktor pertumbuhan.
                        </p>
                    </div>
                    <div id="quiz-badge-score" class="hidden items-center gap-2 bg-emerald-50 border border-emerald-200 px-4 py-2 rounded-xl text-emerald-800 font-bold text-sm">
                        <span>Skor Anda:</span>
                        <span id="final-score" class="text-xl font-black text-emerald-600">0</span> / 100
                    </div>
                </div>

                <!-- Quiz Container -->
                <div id="quiz-container" class="space-y-6">
                    <!-- Dynamic Questions generated via JS -->
                </div>

                <!-- Submit Button -->
                <div class="pt-6 mt-6 border-t border-slate-100 flex flex-col sm:flex-row justify-between items-center gap-4">
                    <button id="btn-submit-quiz" onclick="submitQuiz()" class="w-full sm:w-auto bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3 px-8 rounded-xl shadow-md transition flex items-center justify-center gap-2">
                        <i class="fa-solid fa-paper-plane"></i> Kirim Jawaban & Lihat Hasil
                    </button>
                    <button id="btn-reset-quiz" onclick="resetQuiz()" class="hidden w-full sm:w-auto bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold py-3 px-6 rounded-xl transition flex items-center justify-center gap-2">
                        <i class="fa-solid fa-rotate-right"></i> Ulangi Kuis
                    </button>
                </div>

            </div>
        </section>

    </main>

    <!-- FOOTER -->
    <footer class="bg-slate-900 text-slate-400 text-xs py-6 mt-12 border-t border-slate-800">
        <div class="max-w-7xl mx-auto px-4 text-center space-y-1.5">
            <p class="font-medium text-slate-300">
                Smart-Growth Lab &copy; 2026 — Dikembangkan oleh <span class="text-emerald-400 font-bold">Mirzawati, S.Pd.</span> (MTsN 5 Bireuen)
            </p>
            <p class="text-slate-500">
                Inovasi Media Pembelajaran Biologi Berbantuan Web-Based Virtual Laboratory.
            </p>
        </div>
    </footer>


    <!-- ================= JAVASCRIPT LOGIC ================= -->
    <script>
        /* ================= STATE & VARIABLES ================= */
        let currentTab = 'lab';
        let isRunning = false;
        let simInterval = null;
        let currentDay = 0;
        const maxDays = 14;
        let simSpeed = 1000;

        let historyData = [];
        let growthChartInstance = null;
        let canvas, ctx;

        /* ================= TOAST NOTIFICATION ================= */
        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            const bgColor = type === 'success' ? 'bg-emerald-800 border-emerald-600' : 'bg-amber-700 border-amber-500';
            const icon = type === 'success' ? 'fa-circle-check text-emerald-300' : 'fa-circle-exclamation text-amber-300';

            toast.className = `${bgColor} border text-white px-4 py-3 rounded-xl shadow-lg text-xs font-medium flex items-center gap-2.5 transform transition-all duration-300 translate-y-2 opacity-0`;
            toast.innerHTML = `<i class="fa-solid ${icon} text-base"></i> <span>${message}</span>`;

            container.appendChild(toast);
            requestAnimationFrame(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            });

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        /* ================= QUIZ DATA ================= */
        const quizData = [
            {
                id: 1,
                question: "Pada tumbuhan kacang hijau, saat berkecambah bagian hipokotil memanjang sehingga mengangkat kotiledon dan tunas ke atas permukaan tanah. Tipe perkecambahan ini disebut...",
                options: ["Epigeal", "Hipogeal", "Etiolasi", "Dormansi"],
                answer: 0,
                explanation: "Perkecambahan epigeal ditandai dengan memanjangnya hipokotil sehingga kotiledon terangkat ke atas permukaan tanah (contoh: Kacang Hijau)."
            },
            {
                id: 2,
                question: "Seorang siswa meletakkan pot tanaman di dalam kardus tertutup rapat tanpa cahaya. Setelah 7 hari, tanaman tumbuh sangat tinggi tetapi batangnya lemah dan daunnya berwarna kuning pucat. Fenomena ini dinamakan...",
                options: ["Fotoperiodisme", "Etiolasi", "Plasmolisis", "Gutasi"],
                answer: 1,
                explanation: "Etiolasi adalah pertumbuhan tanaman yang sangat cepat di tempat gelap karena hormon auksin tidak terurai oleh cahaya, namun tanaman menjadi pucat dan lemah karena kurangnya pembentukan klorofil."
            },
            {
                id: 3,
                question: "Hormon tumbuhan yang aktivitasnya terhambat atau terurai jika terkena intensitas cahaya matahari tinggi adalah...",
                options: ["Sitokinin", "Giberelin", "Auksin", "Asam Absisat"],
                answer: 2,
                explanation: "Hormon Auksin bersifat peka terhadap cahaya. Jika terkena cahaya terang, auksin akan terurai sehingga menghambat pemanjangan sel dibandingkan sisi yang gelap."
            },
            {
                id: 4,
                question: "Mengapa pemberian pupuk NPK dalam dosis yang terlalu berlebihan justru dapat menyebabkan tanaman layu dan mati?",
                options: [
                    "Pupuk memblokir masuknya sinar matahari ke daun",
                    "Konsentrasi zat terlarut di luar akar menjadi sangat tinggi sehingga terjadi osmosis air keluar dari sel akar (plasmolisis)",
                    "Pupuk merusak struktur klorofil secara langsung",
                    "Suhu tanah meningkat drastis hingga mendidih"
                ],
                answer: 1,
                explanation: "Pupuk berlebih meningkatkan kepekatan (konsentrasi garam) di media tanam, menyebabkan cairan dalam sel akar tertarik keluar secara osmosis (plasmolisis / akar terbakar)."
            },
            {
                id: 5,
                question: "Proses masuknya air ke dalam sel-sel biji yang kering untuk mengaktifkan enzim metabolik awal perkecambahan disebut...",
                options: ["Transpirasi", "Imbibisi", "Difusi Terfasilitasi", "Fotosintesis"],
                answer: 1,
                explanation: "Imbibisi adalah penyerapan air oleh ruang interseluler dan dinding sel biji kering yang memicu pengaktifan enzim amilase untuk memecah cadangan makanan."
            }
        ];

        /* ================= INITIALIZATION ================= */
        window.onload = function() {
            initCanvas();
            initChart();
            renderQuiz();
            resetSimulasi();
        };

        window.addEventListener('resize', () => {
            if (canvas) {
                resizeCanvas();
                drawPlant();
            }
        });

        /* ================= TAB NAVIGATION ================= */
        function switchTab(tabName) {
            currentTab = tabName;
            
            document.querySelectorAll('main > section').forEach(sec => sec.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active', 'text-white');
                btn.classList.add('text-emerald-100');
            });

            document.getElementById(`section-${tabName}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`tab-${tabName}`);
            activeBtn.classList.add('active', 'text-white');
            activeBtn.classList.remove('text-emerald-100');

            if (tabName === 'lab') {
                resizeCanvas();
                drawPlant();
            }
            if (tabName === 'data' && growthChartInstance) {
                updateReportSummary();
                growthChartInstance.update();
            }
        }

        function updateReportSummary() {
            const bijiText = document.getElementById('param-biji').options[document.getElementById('param-biji').selectedIndex].text.split(' (')[0];
            const cahayaText = document.getElementById('param-cahaya').options[document.getElementById('param-cahaya').selectedIndex].text.split(' (')[0];
            const airText = document.getElementById('param-air').options[document.getElementById('param-air').selectedIndex].text.split(' (')[0];
            const suhuText = document.getElementById('param-suhu').options[document.getElementById('param-suhu').selectedIndex].text;
            const nutrisiText = document.getElementById('param-nutrisi').options[document.getElementById('param-nutrisi').selectedIndex].text.split(' (')[0];

            document.getElementById('report-params-summary').innerText = `Parameter: ${bijiText} | ${cahayaText} | ${airText} | ${suhuText} | ${nutrisiText}`;
        }

        /* ================= SPEED CONTROL ================= */
        function setSpeed(speedMs) {
            simSpeed = speedMs;
            document.querySelectorAll('[id^="spd-"]').forEach(btn => {
                btn.className = "px-3 py-1 text-xs rounded-lg font-bold bg-slate-200 text-slate-700 hover:bg-slate-300 transition";
            });
            
            const activeClass = "px-3 py-1 text-xs rounded-lg font-bold bg-emerald-600 text-white transition";
            if (speedMs === 1000) document.getElementById('spd-1').className = activeClass;
            if (speedMs === 500) document.getElementById('spd-2').className = activeClass;
            if (speedMs === 250) document.getElementById('spd-3').className = activeClass;

            if (isRunning) {
                clearInterval(simInterval);
                simInterval = setInterval(stepSimulasi, simSpeed);
            }
        }

        /* ================= SIMULATION CORE LOGIC ================= */
        function toggleSimulasi() {
            if (isRunning) {
                pauseSimulasi();
            } else {
                if (currentDay >= maxDays) {
                    resetSimulasi();
                }
                startSimulasi();
            }
        }

        function startSimulasi() {
            isRunning = true;
            document.getElementById('icon-start').className = 'fa-solid fa-pause';
            document.getElementById('text-start').innerText = 'Jeda';
            document.getElementById('badge-status').className = 'px-3 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-800 flex items-center gap-1.5';
            document.getElementById('badge-status').innerHTML = '<span class="w-2 h-2 rounded-full bg-emerald-500 animate-ping"></span> Simulasi Berjalan...';
            
            simInterval = setInterval(stepSimulasi, simSpeed);
        }

        function pauseSimulasi() {
            isRunning = false;
            clearInterval(simInterval);
            document.getElementById('icon-start').className = 'fa-solid fa-play';
            document.getElementById('text-start').innerText = currentDay > 0 && currentDay < maxDays ? 'Lanjut' : 'Mulai';
            document.getElementById('badge-status').className = 'px-3 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-800 flex items-center gap-1.5';
            document.getElementById('badge-status').innerHTML = '<span class="w-2 h-2 rounded-full bg-amber-500"></span> Dijeda';
        }

        function resetSimulasi() {
            pauseSimulasi();
            currentDay = 0;
            historyData = [];
            
            document.getElementById('text-start').innerText = 'Mulai';
            document.getElementById('display-hari').innerText = `Hari ke-0`;
            document.getElementById('day-progress').style.width = '0%';
            document.getElementById('badge-status').className = 'px-3 py-1 rounded-full text-xs font-bold bg-slate-100 text-slate-700 flex items-center gap-1.5';
            document.getElementById('badge-status').innerHTML = '<span class="w-2 h-2 rounded-full bg-slate-400"></span> Siap Dijalankan';
            document.getElementById('canvas-overlay').classList.add('hidden');

            const bijiVal = document.getElementById('param-biji').value;
            document.getElementById('telemetry-seed-tag').innerText = bijiVal === 'kacang_hijau' ? 'Epigeal (Kacang Hijau)' : 'Hipogeal (Jagung)';

            recordDataStep();
            updateUI();
            drawPlant();
        }

        function stepSimulasi() {
            if (currentDay < maxDays) {
                currentDay++;
                document.getElementById('display-hari').innerText = `Hari ke-${currentDay}`;
                document.getElementById('day-progress').style.width = `${(currentDay / maxDays) * 100}%`;
                
                recordDataStep();
                updateUI();
                drawPlant();

                const state = calculateGrowthState(currentDay);
                if (state.isDead) {
                    pauseSimulasi();
                    document.getElementById('badge-status').className = 'px-3 py-1 rounded-full text-xs font-bold bg-red-100 text-red-800 flex items-center gap-1.5';
                    document.getElementById('badge-status').innerHTML = '<i class="fa-solid fa-circle-xmark"></i> Pertumbuhan Terhenti';
                    document.getElementById('canvas-overlay').classList.remove('hidden');
                    document.getElementById('overlay-title').innerText = "Pertumbuhan Terhenti / Mati";
                    document.getElementById('overlay-desc').innerText = state.deadReason;
                }
            } else {
                pauseSimulasi();
                document.getElementById('text-start').innerText = 'Ulangi';
                document.getElementById('badge-status').className = 'px-3 py-1 rounded-full text-xs font-bold bg-blue-100 text-blue-800 flex items-center gap-1.5';
                document.getElementById('badge-status').innerHTML = '<i class="fa-solid fa-circle-check"></i> Selesai (Hari 14)';
                showToast("Simulasi 14 hari selesai! Cek Data Logger & Grafik.", "success");
            }
        }

        /* ================= MATHEMATICAL & BIOLOGICAL MODEL ================= */
        function calculateGrowthState(day) {
            const biji = document.getElementById('param-biji').value;
            const cahaya = document.getElementById('param-cahaya').value;
            const air = document.getElementById('param-air').value;
            const suhu = document.getElementById('param-suhu').value;
            const nutrisi = document.getElementById('param-nutrisi').value;

            if (day === 0) {
                return {
                    height: 0,
                    leaves: 0,
                    leafColor: 'Belum Muncul',
                    root: 'Fase Dormansi',
                    isDead: false,
                    isEtiolated: false,
                    bijiType: biji,
                    fisiologi: 'Biji berada dalam fase dormansi. Siap menyerap air (imbibisi) saat simulasi dimulai.'
                };
            }

            // 1. Cek Kematian / Failure awal
            if (suhu === 'panas') {
                return { height: 0, leaves: 0, leafColor: 'Tidak Tumbuh', root: 'Rusak (Denaturasi)', isDead: true, deadReason: 'Suhu 42°C terlalu ekstrem. Enzim mengalami denaturasi sehingga biji gagal berkecambah.', fisiologi: 'Terjadi denaturasi protein enzim akibat suhu ekstrem.' };
            }
            if (air === 'kering') {
                return { height: 0, leaves: 0, leafColor: 'Tidak Tumbuh', root: 'Dorman / Kering', isDead: true, deadReason: 'Media tanam kering. Proses imbibisi air tidak terjadi sehingga enzim tetap tidak aktif.', fisiologi: 'Kekurangan air menghambat proses imbibisi awal perkecambahan.' };
            }
            if (air === 'tinggi' && day > 3) {
                return { height: parseFloat((day * 0.4).toFixed(1)), leaves: 0, leafColor: 'Layu / Busuk', root: 'Membusuk (Anaerob)', isDead: true, deadReason: 'Media tergenang air menyebabkan kekurangan oksigen (anoksia). Akar membusuk.', fisiologi: 'Respirasi seluler akar terhenti akibat kondisi anaerob.' };
            }
            if (nutrisi === 'berlebih' && day > 4) {
                return { height: parseFloat((day * 0.5).toFixed(1)), leaves: 1, leafColor: 'Cokelat Kering', root: 'Plasmolisis', isDead: true, deadReason: 'Dosis pupuk NPK terlalu pekat memicu keluarnya air sel akar secara osmosis (plasmolisis).', fisiologi: 'Terjadi plasmolisis sel akar akibat tekanan osmotik tanah terlalu tinggi.' };
            }

            // 2. Laju pertumbuhan dasar (cm/hari)
            let growthRate = biji === 'jagung' ? 1.35 : 1.15;

            if (suhu === 'dingin') growthRate *= 0.35;

            let isEtiolated = false;
            let leafColor = 'Hijau Segar';

            if (cahaya === 'gelap') {
                growthRate *= 2.2; // Etiolasi: pemanjangan cepat
                isEtiolated = true;
                leafColor = 'Kuning Pucat (Etiolasi)';
            } else if (cahaya === 'remang') {
                growthRate *= 1.3;
                leafColor = 'Hijau Muda';
            } else {
                growthRate *= 1.0;
                leafColor = 'Hijau Tua Sehat';
            }

            if (nutrisi === 'tanpa' && day > 7) {
                growthRate *= 0.65;
                if (!isEtiolated) leafColor = 'Hijau Kekuningan (Klorosis)';
            }

            // Kurva pertumbuhan sigmoid ringan
            let height = day * growthRate * (1 - (day * 0.012));
            let leaves = 0;

            if (day >= 3) {
                leaves = Math.floor((day - 2) * (isEtiolated ? 0.45 : 0.75));
            }
            if (leaves > 8) leaves = 8;

            let rootCond = 'Kuat & Bercabang';
            if (suhu === 'dingin') rootCond = 'Pendek / Lambat';
            if (isEtiolated) rootCond = 'Tipis & Lemah';

            let fisiologi = 'Fotosintesis dan pembelahan sel meristem berjalan seimbang.';
            if (day <= 2) {
                fisiologi = 'Terjadi imbibisi air, mengaktifkan hormon giberelin dan enzim amilase untuk memecah cadangan makanan.';
            } else if (isEtiolated) {
                fisiologi = 'Auksin bekerja maksimal di tempat gelap memicu pemanjangan batang drastis, namun kloroplas gagal membentuk klorofil.';
            } else if (suhu === 'dingin') {
                fisiologi = 'Suhu rendah (10°C) memperlambat energi kinetik molekul sehingga aktivitas enzim melambat.';
            } else if (nutrisi === 'tanpa' && day > 7) {
                fisiologi = 'Cadangan makanan kotiledon habis; tanaman kekurangan Nitrogen dan Magnesium untuk sintesis klorofil.';
            }

            return {
                height: parseFloat(height.toFixed(1)),
                leaves: leaves,
                leafColor: leafColor,
                root: rootCond,
                isDead: false,
                isEtiolated: isEtiolated,
                bijiType: biji,
                fisiologi: fisiologi
            };
        }

        /* ================= RECORD & UPDATE DATA ================= */
        function recordDataStep() {
            const state = calculateGrowthState(currentDay);
            historyData[currentDay] = {
                day: currentDay,
                height: state.height,
                leaves: state.leaves,
                color: state.leafColor,
                root: state.root,
                fisiologi: state.fisiologi
            };

            updateTableUI();
            updateChartUI();
        }

        function updateUI() {
            const state = calculateGrowthState(currentDay);
            document.getElementById('stat-tinggi').innerHTML = `${state.height} <span class="text-xs font-normal text-slate-500">cm</span>`;
            document.getElementById('stat-daun').innerHTML = `${state.leaves} <span class="text-xs font-normal text-slate-500">helai</span>`;
            document.getElementById('stat-warna').innerText = state.leafColor;
            document.getElementById('stat-akar').innerText = state.root;
            document.getElementById('stat-fisiologi').innerText = state.fisiologi;
        }

        function updateTableUI() {
            const tbody = document.getElementById('table-body');
            tbody.innerHTML = '';

            historyData.forEach(item => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-emerald-50/40 border-b border-slate-100 transition';
                tr.innerHTML = `
                    <td class="p-2.5 font-semibold text-slate-800">Hari ${item.day}</td>
                    <td class="p-2.5 text-emerald-700 font-bold">${item.height} cm</td>
                    <td class="p-2.5">${item.leaves} helai</td>
                    <td class="p-2.5 text-xs text-slate-600">${item.color}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function updateChartUI() {
            if (!growthChartInstance) return;
            growthChartInstance.data.labels = historyData.map(d => `Hari ${d.day}`);
            growthChartInstance.data.datasets[0].data = historyData.map(d => d.height);
            growthChartInstance.update();
        }

        /* ================= CANVAS ANIMATION & DRAWING ================= */
        function initCanvas() {
            canvas = document.getElementById('growthCanvas');
            ctx = canvas.getContext('2d');
            resizeCanvas();
        }

        function resizeCanvas() {
            const wrapper = document.getElementById('canvas-wrapper');
            if (!wrapper || !canvas) return;
            canvas.width = wrapper.clientWidth;
            canvas.height = wrapper.clientHeight;
        }

        function drawPlant() {
            if (!ctx || !canvas) return;
            const width = canvas.width;
            const height = canvas.height;

            ctx.clearRect(0, 0, width, height);

            const state = calculateGrowthState(currentDay);
            const bijiType = document.getElementById('param-biji').value;
            const cahaya = document.getElementById('param-cahaya').value;
            const air = document.getElementById('param-air').value;

            const groundLevel = height - 95;

            // 1. Sky / Room Background
            if (cahaya === 'gelap') {
                ctx.fillStyle = '#0f172a';
            } else if (cahaya === 'remang') {
                ctx.fillStyle = '#334155';
            } else {
                const skyGrad = ctx.createLinearGradient(0, 0, 0, groundLevel);
                skyGrad.addColorStop(0, '#bae6fd');
                skyGrad.addColorStop(1, '#f0f9ff');
                ctx.fillStyle = skyGrad;
            }
            ctx.fillRect(0, 0, width, groundLevel);

            // 2. Sun / Light Source
            if (cahaya === 'terang' || cahaya === 'remang') {
                const sunX = width - 55;
                const sunY = 48;
                ctx.save();
                ctx.beginPath();
                ctx.arc(sunX, sunY, cahaya === 'terang' ? 24 : 16, 0, Math.PI * 2);
                ctx.fillStyle = cahaya === 'terang' ? '#facc15' : '#fef08a';
                ctx.shadowColor = '#fde047';
                ctx.shadowBlur = cahaya === 'terang' ? 25 : 10;
                ctx.fill();
                ctx.restore();
            }

            // 3. Virtual Ruler on Left Side (0 - 30 cm)
            const rulerX = 28;
            const maxRulerCm = 30;
            const pxPerCm = (groundLevel - 30) / maxRulerCm;

            ctx.strokeStyle = cahaya === 'terang' ? '#64748b' : '#94a3b8';
            ctx.fillStyle = cahaya === 'terang' ? '#475569' : '#cbd5e1';
            ctx.font = '10px Inter, sans-serif';
            ctx.lineWidth = 1;

            ctx.beginPath();
            ctx.moveTo(rulerX, 30);
            ctx.lineTo(rulerX, groundLevel);
            ctx.stroke();

            for (let cm = 0; cm <= maxRulerCm; cm += 5) {
                const yPos = groundLevel - (cm * pxPerCm);
                ctx.beginPath();
                ctx.moveTo(rulerX - 5, yPos);
                ctx.lineTo(rulerX + 4, yPos);
                ctx.stroke();
                ctx.fillText(`${cm}cm`, rulerX + 7, yPos + 3);
            }

            // 4. Soil & Pot
            ctx.fillStyle = '#291509';
            ctx.fillRect(0, groundLevel, width, height - groundLevel);

            const centerX = width / 2;
            const potTopWidth = Math.min(220, width * 0.45);
            const potBottomWidth = potTopWidth * 0.75;

            // Pot Body
            ctx.fillStyle = '#c2410c';
            ctx.beginPath();
            ctx.moveTo(centerX - potTopWidth / 2, groundLevel);
            ctx.lineTo(centerX + potTopWidth / 2, groundLevel);
            ctx.lineTo(centerX + potBottomWidth / 2, height - 10);
            ctx.lineTo(centerX - potBottomWidth / 2, height - 10);
            ctx.closePath();
            ctx.fill();

            // Pot Rim
            ctx.fillStyle = '#ea580c';
            ctx.fillRect(centerX - (potTopWidth / 2) - 6, groundLevel - 4, potTopWidth + 12, 12);

            // Soil Inside Pot (Cross-section window so roots are visible)
            ctx.fillStyle = air === 'tinggi' ? '#1e3a8a' : (air === 'kering' ? '#92400e' : '#3f2312');
            ctx.beginPath();
            ctx.roundRect(centerX - potBottomWidth / 2 + 12, groundLevel + 10, potBottomWidth - 24, height - groundLevel - 28, 8);
            ctx.fill();

            const soilSurfaceY = groundLevel + 12;

            // Water puddle effect if air === 'tinggi'
            if (air === 'tinggi') {
                ctx.fillStyle = 'rgba(56, 189, 248, 0.55)';
                ctx.fillRect(centerX - potTopWidth / 2 + 8, groundLevel + 4, potTopWidth - 16, 8);
            }

            // 5. Roots (Underground inside cutaway)
            if (currentDay >= 1 && !['panas', 'kering'].includes(document.getElementById('param-suhu').value) && air !== 'kering') {
                ctx.strokeStyle = state.isDead ? '#78350f' : '#fef3c7';
                ctx.lineWidth = 2.5;
                ctx.beginPath();
                ctx.moveTo(centerX, soilSurfaceY + 4);
                const rootDepth = Math.min(currentDay * 4.5, 58);
                ctx.lineTo(centerX, soilSurfaceY + rootDepth);

                if (currentDay >= 2) {
                    ctx.moveTo(centerX, soilSurfaceY + 14);
                    ctx.lineTo(centerX - 18, soilSurfaceY + 28);
                    ctx.moveTo(centerX, soilSurfaceY + 22);
                    ctx.lineTo(centerX + 20, soilSurfaceY + 38);
                }
                if (currentDay >= 5) {
                    ctx.moveTo(centerX, soilSurfaceY + 32);
                    ctx.lineTo(centerX - 24, soilSurfaceY + 50);
                    ctx.moveTo(centerX, soilSurfaceY + 40);
                    ctx.lineTo(centerX + 16, soilSurfaceY + 55);
                }
                ctx.stroke();
            }

            // 6. Seed at Day 0 or Failed Germination
            if (currentDay === 0 || state.height === 0) {
                ctx.fillStyle = bijiType === 'kacang_hijau' ? '#16a34a' : '#f59e0b';
                ctx.beginPath();
                ctx.ellipse(centerX, soilSurfaceY + 4, 8, 5, 0, 0, Math.PI * 2);
                ctx.fill();
                return;
            }

            // 7. Stem (Above Ground scaled to Ruler)
            const stemPixelHeight = Math.min(state.height * pxPerCm, groundLevel - 20);
            const stemTopY = groundLevel - stemPixelHeight;

            let stemColor = '#16a34a';
            if (state.isEtiolated) stemColor = '#fef08a';
            if (state.isDead) stemColor = '#78350f';

            ctx.strokeStyle = stemColor;
            ctx.lineWidth = state.isEtiolated ? 3 : 5;
            ctx.lineCap = 'round';
            ctx.beginPath();
            ctx.moveTo(centerX, soilSurfaceY);
            const sway = state.isEtiolated ? 12 : 4;
            ctx.quadraticCurveTo(centerX - sway, groundLevel - stemPixelHeight * 0.5, centerX + (state.isEtiolated ? 6 : 0), stemTopY);
            ctx.stroke();

            const topX = centerX + (state.isEtiolated ? 6 : 0);

            // 8. Cotyledon Position (Epigeal vs Hipogeal)
            if (bijiType === 'kacang_hijau') {
                // EPIGEAL: Cotyledon lifted up above soil
                const cotY = Math.max(groundLevel - (stemPixelHeight * 0.65), stemTopY + 12);
                ctx.fillStyle = state.isDead ? '#78350f' : (state.isEtiolated ? '#fde047' : '#86efac');
                ctx.beginPath();
                ctx.ellipse(centerX - 6, cotY, 6, 3.5, -0.3, 0, Math.PI * 2);
                ctx.ellipse(centerX + 6, cotY, 6, 3.5, 0.3, 0, Math.PI * 2);
                ctx.fill();
            } else {
                // HIPOGEAL: Seed stays underground
                ctx.fillStyle = '#d97706';
                ctx.beginPath();
                ctx.ellipse(centerX, soilSurfaceY + 5, 8, 5, 0, 0, Math.PI * 2);
                ctx.fill();
            }

            // 9. Leaves
            if (state.leaves > 0 && !state.isDead) {
                let leafColorHex = '#15803d';
                if (state.isEtiolated) leafColorHex = '#facc15';
                else if (state.leafColor.includes('Muda') || state.leafColor.includes('Kekuningan')) leafColorHex = '#65a30d';

                ctx.fillStyle = leafColorHex;

                for (let i = 0; i < state.leaves; i++) {
                    const progressRatio = (i + 1) / (state.leaves + 1);
                    const leafY = groundLevel - (stemPixelHeight * (0.45 + progressRatio * 0.55));
                    const isLeft = i % 2 === 0;
                    const leafSize = bijiType === 'jagung' ? 16 : 12;

                    ctx.beginPath();
                    if (isLeft) {
                        ctx.ellipse(topX - leafSize, leafY, leafSize, bijiType === 'jagung' ? 3.5 : 5, -Math.PI / 6, 0, Math.PI * 2);
                    } else {
                        ctx.ellipse(topX + leafSize, leafY, leafSize, bijiType === 'jagung' ? 3.5 : 5, Math.PI / 6, 0, Math.PI * 2);
                    }
                    ctx.fill();
                }
            }
        }

        /* ================= CHART.JS INTEGRATION ================= */
        function initChart() {
            const ctxChart = document.getElementById('growthChart').getContext('2d');
            growthChartInstance = new Chart(ctxChart, {
                type: 'line',
                data: {
                    labels: ['Hari 0'],
                    datasets: [{
                        label: 'Tinggi Batang (cm)',
                        data: [0],
                        borderColor: '#059669',
                        backgroundColor: 'rgba(16, 185, 129, 0.15)',
                        borderWidth: 3,
                        fill: true,
                        tension: 0.3,
                        pointRadius: 4,
                        pointBackgroundColor: '#047857'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: true,
                            position: 'top'
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Tinggi Batang (cm)'
                            }
                        },
                        x: {
                            title: {
                                display: true,
                                text: 'Hari Pengamatan'
                            }
                        }
                    }
                }
            });
        }

        /* ================= TABLE EXPORT / COPY ================= */
        function copyTableData() {
            let text = "Hari\tTinggi (cm)\tJumlah Daun\tKondisi\n";
            historyData.forEach(d => {
                text += `Hari ${d.day}\t${d.height}\t${d.leaves}\t${d.color}\n`;
            });

            const dummy = document.createElement("textarea");
            document.body.appendChild(dummy);
            dummy.value = text;
            dummy.select();
            document.execCommand("copy");
            document.body.removeChild(dummy);

            showToast("Data tabel pengamatan berhasil disalin ke clipboard!", "success");
        }

        /* ================= QUIZ LOGIC ================= */
        function renderQuiz() {
            const container = document.getElementById('quiz-container');
            container.innerHTML = '';

            quizData.forEach((q, idx) => {
                const card = document.createElement('div');
                card.className = 'bg-slate-50 rounded-2xl p-5 border border-slate-200 space-y-4';
                
                let optionsHTML = '';
                q.options.forEach((opt, oIdx) => {
                    optionsHTML += `
                        <label id="opt-label-${q.id}-${oIdx}" class="flex items-start gap-3 p-3 rounded-xl border border-slate-200 bg-white hover:bg-emerald-50/60 cursor-pointer transition">
                            <input type="radio" name="question-${q.id}" value="${oIdx}" class="mt-1 accent-emerald-600">
                            <span class="text-xs sm:text-sm text-slate-700">${opt}</span>
                        </label>
                    `;
                });

                card.innerHTML = `
                    <div class="flex items-start gap-3">
                        <span class="w-7 h-7 rounded-full bg-emerald-600 text-white font-bold text-xs flex items-center justify-center shrink-0 mt-0.5">${idx + 1}</span>
                        <h4 class="font-bold text-slate-800 text-sm sm:text-base leading-snug">${q.question}</h4>
                    </div>
                    <div class="space-y-2 pl-0 sm:pl-10">
                        ${optionsHTML}
                    </div>
                    <div id="explanation-${q.id}" class="hidden pl-0 sm:pl-10 mt-3 p-3.5 rounded-xl bg-blue-50 border border-blue-200 text-xs text-blue-950 leading-relaxed">
                        <strong><i class="fa-solid fa-lightbulb text-amber-500 mr-1"></i> Pembahasan:</strong> ${q.explanation}
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function submitQuiz() {
            let answeredCount = 0;
            quizData.forEach(q => {
                if (document.querySelector(`input[name="question-${q.id}"]:checked`)) {
                    answeredCount++;
                }
            });

            if (answeredCount < quizData.length) {
                showToast(`Harap jawab semua pertanyaan (${answeredCount}/${quizData.length} terjawab).`, "warning");
                return;
            }

            let correctCount = 0;
            quizData.forEach(q => {
                const selected = document.querySelector(`input[name="question-${q.id}"]:checked`);
                const expDiv = document.getElementById(`explanation-${q.id}`);
                if (expDiv) expDiv.classList.remove('hidden');

                const selectedVal = parseInt(selected.value);
                const correctLabel = document.getElementById(`opt-label-${q.id}-${q.answer}`);
                correctLabel.classList.add('border-emerald-500', 'bg-emerald-50', 'font-semibold');

                if (selectedVal === q.answer) {
                    correctCount++;
                } else {
                    const wrongLabel = document.getElementById(`opt-label-${q.id}-${selectedVal}`);
                    wrongLabel.classList.add('border-red-400', 'bg-red-50');
                }
            });

            const score = (correctCount / quizData.length) * 100;
            document.getElementById('final-score').innerText = score;
            const badge = document.getElementById('quiz-badge-score');
            badge.classList.remove('hidden');
            badge.classList.add('flex');
            
            document.getElementById('btn-submit-quiz').classList.add('hidden');
            document.getElementById('btn-reset-quiz').classList.remove('hidden');
            
            showToast(`Kuis selesai! Skor Anda: ${score}/100`, "success");
            document.getElementById('section-kuis').scrollIntoView({ behavior: 'smooth' });
        }

        function resetQuiz() {
            renderQuiz();
            const badge = document.getElementById('quiz-badge-score');
            badge.classList.add('hidden');
            badge.classList.remove('flex');
            document.getElementById('btn-submit-quiz').classList.remove('hidden');
            document.getElementById('btn-reset-quiz').classList.add('hidden');
        }
    </script>
</body>
</html>
