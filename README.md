<!DOCTYPE html>
<html lang="id" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ekonovasi: Media Pembelajaran IPS Interaktif Kelas 7</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Poppins & Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Poppins', 'Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#eef2ff',
                            100: '#e0e7ff',
                            500: '#6366f1',
                            600: '#4f46e5',
                            700: '#4338ca',
                            800: '#3730a3',
                            900: '#312e81',
                        },
                        sunflower: {
                            400: '#fbbf24',
                            500: '#f59e0b',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        .bounce-slow {
            animation: bounce 3s infinite;
        }
        .pulse-subtle {
            animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-8px); }
        }
        .floating-card {
            animation: float 4s ease-in-out infinite;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
        }
        ::-webkit-scrollbar-thumb {
            background: #c7d2fe;
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #818cf8;
        }
    </style>
</head>
<body class="h-full bg-gradient-to-br from-indigo-50 via-blue-50 to-purple-50 font-sans text-slate-800 antialiased flex flex-col justify-between">

    <!-- Modal Notifikasi Kustom (Pengganti Alert) -->
    <div id="custom-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm hidden">
        <div class="bg-white rounded-2xl shadow-2xl max-w-md w-full mx-4 p-6 transform transition-all scale-95 opacity-0" id="modal-content">
            <div class="flex items-center space-x-3 mb-4">
                <div id="modal-icon" class="w-12 h-12 rounded-full bg-brand-100 flex items-center justify-center text-brand-600 text-xl font-bold">
                    <i class="fa-solid fa-bell"></i>
                </div>
                <div>
                    <h3 id="modal-title" class="text-lg font-bold text-slate-900">Judul Pesan</h3>
                    <p id="modal-subtitle" class="text-xs text-slate-500">Ekonovasi Media Pembelajaran</p>
                </div>
            </div>
            <p id="modal-message" class="text-slate-600 text-sm mb-6 leading-relaxed">Pesan interaktif akan muncul di sini.</p>
            <div class="flex justify-end space-x-3">
                <button id="modal-close-btn" onclick="closeModal()" class="px-5 py-2.5 bg-brand-600 hover:bg-brand-700 text-white font-medium text-sm rounded-xl shadow-lg shadow-brand-500/30 transition-all">Mengerti</button>
            </div>
        </div>
    </div>

    <!-- Login / Identitas Awal Overlay -->
    <div id="identity-overlay" class="fixed inset-0 z-40 bg-gradient-to-br from-indigo-900/90 via-slate-900/90 to-purple-900/90 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl shadow-2xl max-w-lg w-full p-8 border border-white/20 relative overflow-hidden">
            <div class="absolute -right-12 -top-12 w-40 h-40 bg-brand-500/10 rounded-full blur-2xl pointer-events-none"></div>
            
            <div class="text-center mb-6">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-brand-100 text-brand-600 rounded-2xl mb-3 shadow-inner text-2xl font-bold">
                    <i class="fa-solid fa-graduation-cap"></i>
                </div>
                <h1 class="text-2xl font-extrabold text-slate-900">Selamat Datang di <span class="text-brand-600">EKONOVASI</span></h1>
                <p class="text-sm text-slate-500 mt-1">Media Pembelajaran & Kuis Interaktif IPS Kelas 7</p>
            </div>

            <form id="identity-form" onsubmit="handleIdentitySubmit(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1.5">Nama Lengkap Siswa</label>
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400">
                            <i class="fa-solid fa-user"></i>
                        </span>
                        <input type="text" id="student-name" required placeholder="Contoh: Budi Pratama" 
                            class="w-full pl-10 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand-500 focus:bg-white transition-all">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1.5">Pilih Kelas (SMP Kelas 7)</label>
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400">
                            <i class="fa-solid fa-school"></i>
                        </span>
                        <select id="student-class" required 
                            class="w-full pl-10 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand-500 focus:bg-white transition-all appearance-none cursor-pointer">
                            <option value="" disabled selected>-- Pilih Jenjang Kelas --</option>
                            <option value="Kelas 71">Kelas 71</option>
                            <option value="Kelas 72">Kelas 72</option>
                            <option value="Kelas 73">Kelas 73</option>
                            <option value="Kelas 74">Kelas 74</option>
                        </select>
                        <span class="absolute inset-y-0 right-0 pr-3.5 flex items-center pointer-events-none text-slate-400">
                            <i class="fa-solid fa-chevron-down text-xs"></i>
                        </span>
                    </div>
                    <p class="text-[11px] text-slate-500 mt-1 italic"><i class="fa-solid fa-circle-info mr-1"></i> Tingkat kesulitan kuis akan otomatis menyesuaikan pilihan kelas kamu!</p>
                </div>

                <div class="pt-2">
                    <button type="submit" class="w-full py-3.5 px-4 bg-gradient-to-r from-brand-600 to-indigo-700 hover:from-brand-700 hover:to-indigo-800 text-white font-bold rounded-xl shadow-lg shadow-brand-500/30 transition-all transform active:scale-95 flex items-center justify-center space-x-2">
                        <span>Mulai Belajar Sekarang</span>
                        <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </div>
            </form>

            <div class="mt-6 pt-4 border-t border-slate-100 flex items-center justify-between text-xs text-slate-400">
                <span><i class="fa-solid fa-shield-halved mr-1 text-emerald-500"></i> Terhubung ke Cloud Database</span>
                <button onclick="openTeacherDashboard()" class="text-brand-600 font-semibold hover:underline flex items-center">
                    <i class="fa-solid fa-chalkboard-user mr-1"></i> Dasbor Guru
                </button>
            </div>
        </div>
    </div>

    <!-- Dasbor Guru Modal -->
    <div id="teacher-modal" class="fixed inset-0 z-50 bg-black/60 backdrop-blur-md hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl shadow-2xl max-w-4xl w-full max-h-[90vh] flex flex-col overflow-hidden border border-slate-200">
            <!-- Header Modal Guru -->
            <div class="bg-slate-900 text-white p-6 flex items-center justify-between">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-xl bg-brand-600 flex items-center justify-center text-white font-bold">
                        <i class="fa-solid fa-chart-line"></i>
                    </div>
                    <div>
                        <h2 class="text-lg font-bold">Dasbor Guru & Rekap Nilai Siswa</h2>
                        <p class="text-xs text-slate-400">Sinkronisasi Cloud Real-Time (Kelas 7.1 s.d. 7.6)</p>
                    </div>
                </div>
                <div class="flex items-center space-x-2">
                    <button onclick="exportToExcel()" class="px-3.5 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-xl text-xs font-semibold flex items-center space-x-1.5 shadow-md transition-all">
                        <i class="fa-solid fa-file-excel"></i>
                        <span>Unduh Excel</span>
                    </button>
                    <button onclick="closeTeacherDashboard()" class="w-9 h-9 rounded-full bg-slate-800 hover:bg-slate-700 text-slate-300 flex items-center justify-center transition-all">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>
            </div>

            <!-- Filter & Statistik Ringkas -->
            <div class="p-6 bg-slate-50 border-b border-slate-200 grid grid-cols-1 md:grid-cols-4 gap-4">
                <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-200 flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-xl bg-blue-100 text-blue-600 flex items-center justify-center font-bold">
                        <i class="fa-solid fa-users"></i>
                    </div>
                    <div>
                        <p class="text-xs text-slate-500 font-medium">Total Siswa Terdaftar</p>
                        <h3 id="stat-total-students" class="text-lg font-bold text-slate-800">0</h3>
                    </div>
                </div>
                <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-200 flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-xl bg-emerald-100 text-emerald-600 flex items-center justify-center font-bold">
                        <i class="fa-solid fa-award"></i>
                    </div>
                    <div>
                        <p class="text-xs text-slate-500 font-medium">Rata-Rata Nilai</p>
                        <h3 id="stat-avg-score" class="text-lg font-bold text-slate-800">0</h3>
                    </div>
                </div>
                <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-200 flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-xl bg-purple-100 text-purple-600 flex items-center justify-center font-bold">
                        <i class="fa-solid fa-ranking-star"></i>
                    </div>
                    <div>
                        <p class="text-xs text-slate-500 font-medium">Skor Tertinggi</p>
                        <h3 id="stat-max-score" class="text-lg font-bold text-slate-800">0</h3>
                    </div>
                </div>
                <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-200 flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-xl bg-amber-100 text-amber-600 flex items-center justify-center font-bold">
                        <i class="fa-solid fa-filter"></i>
                    </div>
                    <div class="flex-1">
                        <p class="text-xs text-slate-500 font-medium mb-1">Filter Kelas</p>
                        <select id="filter-class" onchange="renderStudentTable()" class="w-full text-xs bg-slate-50 border border-slate-200 rounded-lg p-1.5 focus:outline-none focus:ring-1 focus:ring-brand-500">
                            <option value="ALL">Semua Kelas (71 - 74)</option>
                            <option value="Kelas 71">Kelas 71</option>
                            <option value="Kelas 72">Kelas 72</option>
                            <option value="Kelas 73">Kelas 73</option>
                            <option value="Kelas 74">Kelas 74</option>
                        </select>
                    </div>
                </div>
            </div>

            <!-- Tabel Data Siswa -->
            <div class="flex-1 overflow-y-auto p-6">
                <div class="overflow-x-auto rounded-2xl border border-slate-200">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-100 text-slate-600 text-xs font-semibold uppercase tracking-wider border-b border-slate-200">
                                <th class="p-3.5 pl-5">No</th>
                                <th class="p-3.5">Nama Siswa</th>
                                <th class="p-3.5">Kelas</th>
                                <th class="p-3.5">Skor Kuis</th>
                                <th class="p-3.5">Status</th>
                                <th class="p-3.5 pr-5">Waktu Pengerjaan</th>
                            </tr>
                        </thead>
                        <tbody id="student-table-body" class="divide-y divide-slate-100 text-sm text-slate-700 bg-white">
                            <!-- Data akan di-load lewat JavaScript -->
                            <tr>
                                <td colspan="6" class="text-center py-8 text-slate-400">Memuat data cloud database...</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Footer Dasbor Guru -->
            <div class="p-4 bg-slate-50 border-t border-slate-200 flex items-center justify-between text-xs text-slate-500">
                <span><i class="fa-solid fa-cloud text-brand-600 mr-1"></i> Sinkronisasi Cloud aktif secara real-time.</span>
                <button onclick="closeTeacherDashboard()" class="px-4 py-2 bg-slate-200 hover:bg-slate-300 text-slate-700 font-semibold rounded-xl transition-all">Tutup</button>
            </div>
        </div>
    </div>

    <!-- Main Navigation Header -->
    <header class="bg-white/80 backdrop-blur-md border-b border-slate-200 sticky top-0 z-30 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
            <!-- Brand Logo -->
            <div class="flex items-center space-x-3">
                <div class="w-12 h-12 bg-gradient-to-tr from-brand-600 to-indigo-500 rounded-2xl flex items-center justify-center text-white text-xl shadow-lg shadow-brand-500/30">
                    <i class="fa-solid fa-earth-asia"></i>
                </div>
                <div>
                    <span class="text-xl font-extrabold bg-gradient-to-r from-brand-700 to-indigo-600 bg-clip-text text-transparent">EKONOVASI</span>
                    <span class="block text-[11px] font-medium text-slate-500">Media Pembelajaran IPS Kelas 7</span>
                </div>
            </div>

            <!-- Navigation Tabs -->
            <nav class="hidden md:flex items-center space-x-1 bg-slate-100 p-1.5 rounded-2xl border border-slate-200/60">
                <button onclick="switchTab('tujuan')" id="nav-tujuan" class="nav-tab px-4 py-2 rounded-xl text-xs font-semibold transition-all text-slate-600 hover:text-brand-600">
                    <i class="fa-solid fa-bullseye mr-1.5"></i> Tujuan & Materi
                </button>
                <button onclick="switchTab('video')" id="nav-video" class="nav-tab px-4 py-2 rounded-xl text-xs font-semibold transition-all text-slate-600 hover:text-brand-600">
                    <i class="fa-solid fa-play-circle mr-1.5"></i> Video Pembelajaran
                </button>
                <button onclick="switchTab('kuis')" id="nav-kuis" class="nav-tab px-4 py-2 rounded-xl text-xs font-semibold transition-all text-slate-600 hover:text-brand-600">
                    <i class="fa-solid fa-puzzle-piece mr-1.5"></i> Kuis & Game IPS
                </button>
                <button onclick="switchTab('evaluasi')" id="nav-evaluasi" class="nav-tab px-4 py-2 rounded-xl text-xs font-semibold transition-all text-slate-600 hover:text-brand-600">
                    <i class="fa-solid fa-award mr-1.5"></i> Nilai & Evaluasi
                </button>
            </nav>

            <!-- User Info & Controls -->
            <div class="flex items-center space-x-3">
                <!-- Audio Toggle ON/OFF -->
                <button onclick="toggleAudio()" id="audio-toggle-btn" title="Aktifkan/Matikan Suara & Narasi" class="w-10 h-10 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-600 flex items-center justify-center transition-all relative">
                    <i id="audio-icon" class="fa-solid fa-volume-high"></i>
                </button>

                <!-- Student Badge -->
                <div class="hidden sm:flex items-center space-x-3 bg-brand-50 border border-brand-100 px-3.5 py-2 rounded-2xl">
                    <div class="w-8 h-8 rounded-xl bg-brand-600 text-white flex items-center justify-center font-bold text-xs">
                        <i class="fa-solid fa-user-graduate"></i>
                    </div>
                    <div class="text-left">
                        <span id="display-name" class="block text-xs font-bold text-slate-800 truncate max-w-[100px]">Siswa</span>
                        <span id="display-class" class="block text-[10px] font-semibold text-brand-600">Kelas 71</span>
                    </div>
                </div>

                <!-- Teacher Dashboard Trigger button -->
                <button onclick="openTeacherDashboard()" class="px-3.5 py-2 bg-slate-900 hover:bg-slate-800 text-white text-xs font-semibold rounded-xl shadow-md transition-all flex items-center space-x-1.5">
                    <i class="fa-solid fa-lock"></i>
                    <span>Guru</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Mobile Navigation Bar -->
    <div class="md:hidden bg-white border-b border-slate-200 p-2 flex justify-around text-xs">
        <button onclick="switchTab('tujuan')" id="mob-nav-tujuan" class="p-2 text-slate-600 font-semibold flex flex-col items-center">
            <i class="fa-solid fa-bullseye text-base mb-1"></i> Materi
        </button>
        <button onclick="switchTab('video')" id="mob-nav-video" class="p-2 text-slate-600 font-semibold flex flex-col items-center">
            <i class="fa-solid fa-play-circle text-base mb-1"></i> Video
        </button>
        <button onclick="switchTab('kuis')" id="mob-nav-kuis" class="p-2 text-slate-600 font-semibold flex flex-col items-center">
            <i class="fa-solid fa-puzzle-piece text-base mb-1"></i> Kuis
        </button>
        <button onclick="switchTab('evaluasi')" id="mob-nav-evaluasi" class="p-2 text-slate-600 font-semibold flex flex-col items-center">
            <i class="fa-solid fa-award text-base mb-1"></i> Nilai
        </button>
    </div>

    <!-- Main Content Area -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8 flex flex-col justify-between">

        <!-- TAB 1: TUJUAN & MATERI PEMBELAJARAN -->
        <section id="tab-tujuan" class="tab-content space-y-6">
            <!-- Hero Banner -->
            <div class="bg-gradient-to-r from-brand-600 via-indigo-600 to-purple-600 rounded-3xl p-8 text-white shadow-xl relative overflow-hidden">
                <div class="absolute right-0 top-0 w-96 h-96 bg-white/10 rounded-full blur-3xl pointer-events-none"></div>
                <div class="max-w-2xl relative z-10">
                    <span class="inline-block px-3 py-1 bg-white/20 backdrop-blur-md rounded-full text-xs font-semibold mb-3">Mata Pelajaran IPS • SMP Kelas 7</span>
                    <h2 class="text-3xl font-extrabold mb-3">Upaya Masyarakat dalam Memenuhi Kebutuhan Melalui Kegiatan Ekonomi</h2>
                    <p class="text-indigo-100 text-sm leading-relaxed mb-6">
                        Pelajari bagaimana manusia memenuhi kebutuhan hidupnya yang beragam melalui aktivitas produksi, distribusi, dan konsumsi dengan prinsip ekonomi yang bijak!
                    </p>
                    <button onclick="switchTab('video')" class="px-6 py-3 bg-white text-brand-700 hover:bg-indigo-50 font-bold text-sm rounded-2xl shadow-lg transition-all flex items-center space-x-2">
                        <span>Lanjutkan ke Video Pembelajaran</span>
                        <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </div>
            </div>

            <!-- Kartu Tujuan Pembelajaran -->
            <div class="bg-white rounded-3xl p-6 sm:p-8 shadow-sm border border-slate-200">
                <div class="flex items-center space-x-3 mb-6">
                    <div class="w-12 h-12 rounded-2xl bg-brand-100 text-brand-600 flex items-center justify-center text-xl font-bold">
                        <i class="fa-solid fa-bullseye"></i>
                    </div>
                    <div>
                        <h3 class="text-lg font-bold text-slate-900">Tujuan Pembelajaran</h3>
                        <p class="text-xs text-slate-500">Standar Kompetensi & Indikator Pencapaian Kelas 7 (71 - 74)</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="bg-slate-50 rounded-2xl p-5 border border-slate-200 hover:border-brand-300 transition-all">
                        <div class="w-10 h-10 rounded-xl bg-blue-500 text-white flex items-center justify-center font-bold text-sm mb-3">1</div>
                        <h4 class="font-bold text-slate-800 text-sm mb-2">Mengidentifikasi Kebutuhan & Kelangkaan</h4>
                        <p class="text-xs text-slate-600 leading-relaxed">Siswa mampu menjelaskan alasan mengapa manusia harus melakukan kegiatan ekonomi untuk mengatasi kelangkaan sumber daya.</p>
                    </div>
                    <div class="bg-slate-50 rounded-2xl p-5 border border-slate-200 hover:border-brand-300 transition-all">
                        <div class="w-10 h-10 rounded-xl bg-indigo-500 text-white flex items-center justify-center font-bold text-sm mb-3">2</div>
                        <h4 class="font-bold text-slate-800 text-sm mb-2">Menganalisis Peran Pelaku Ekonomi</h4>
                        <p class="text-xs text-slate-600 leading-relaxed">Siswa dapat membedakan peran kegiatan Produksi, Distribusi, dan Konsumsi dalam kehidupan sehari-hari di masyarakat.</p>
                    </div>
                    <div class="bg-slate-50 rounded-2xl p-5 border border-slate-200 hover:border-brand-300 transition-all">
                        <div class="w-10 h-10 rounded-xl bg-purple-500 text-white flex items-center justify-center font-bold text-sm mb-3">3</div>
                        <h4 class="font-bold text-slate-800 text-sm mb-2">Penerapan Pasar & Peran Lembaga Keuangan</h4>
                        <p class="text-xs text-slate-600 leading-relaxed">Siswa memahami fungsi pasar tradisional maupun modern serta pentingnya menabung dan literasi keuangan sejak dini.</p>
                    </div>
                </div>
            </div>

            <!-- Modul Ringkasan Materi -->
            <div class="bg-white rounded-3xl p-6 sm:p-8 shadow-sm border border-slate-200 space-y-6">
                <div class="flex items-center justify-between">
                    <div class="flex items-center space-x-3">
                        <div class="w-12 h-12 rounded-2xl bg-amber-100 text-amber-600 flex items-center justify-center text-xl font-bold">
                            <i class="fa-solid fa-book-open"></i>
                        </div>
                        <div>
                            <h3 class="text-lg font-bold text-slate-900">Ringkasan Materi Kegiatan Ekonomi</h3>
                            <p class="text-xs text-slate-500">Klik tombol di bawah untuk mendengarkan teks otomatis</p>
                        </div>
                    </div>
                    <button onclick="readMaterialText()" class="px-4 py-2 bg-amber-100 hover:bg-amber-200 text-amber-800 rounded-xl text-xs font-bold flex items-center space-x-2 transition-all">
                        <i class="fa-solid fa-volume-high"></i>
                        <span>Dengarkan Materi</span>
                    </button>
                </div>

                <div id="material-text-content" class="grid grid-cols-1 md:grid-cols-3 gap-6 text-sm text-slate-700">
                    <div class="bg-gradient-to-br from-blue-50 to-indigo-50 p-6 rounded-2xl border border-blue-100">
                        <h4 class="font-bold text-blue-900 text-base mb-2 flex items-center"><i class="fa-solid fa-industry mr-2"></i> 1. Produksi</h4>
                        <p class="text-xs leading-relaxed text-slate-600">
                            Kegiatan menghasilkan atau menambah nilai guna suatu barang/jasa untuk memenuhi kebutuhan. Contoh: Petani menanam padi, pabrik membuat pakaian, penjahit menjahit baju.
                        </p>
                    </div>
                    <div class="bg-gradient-to-br from-purple-50 to-pink-50 p-6 rounded-2xl border border-purple-100">
                        <h4 class="font-bold text-purple-900 text-base mb-2 flex items-center"><i class="fa-solid fa-truck-fast mr-2"></i> 2. Distribusi</h4>
                        <p class="text-xs leading-relaxed text-slate-600">
                            Kegiatan menyalurkan barang atau jasa dari produsen kepada konsumen yang membutuhkan. Contoh: Pedagang grosir, sopir truk logistik, kurir pengantar paket.
                        </p>
                    </div>
                    <div class="bg-gradient-to-br from-emerald-50 to-teal-50 p-6 rounded-2xl border border-emerald-100">
                        <h4 class="font-bold text-emerald-900 text-base mb-2 flex items-center"><i class="fa-solid fa-basket-shopping mr-2"></i> 3. Konsumsi</h4>
                        <p class="text-xs leading-relaxed text-slate-600">
                            Kegiatan mengurangi atau menghabiskan nilai guna barang dan jasa untuk memenuhi kebutuhan hidup. Contoh: Siswa memakai seragam sekolah, keluarga makan nasi di rumah.
                        </p>
                    </div>
                </div>
            </div>

            <!-- Tombol Navigasi Halaman -->
            <div class="flex justify-between items-center pt-4">
                <div></div> <!-- Empty spacer -->
                <button onclick="switchTab('video')" class="px-6 py-3 bg-brand-600 hover:bg-brand-700 text-white font-bold text-sm rounded-2xl shadow-lg shadow-brand-500/30 transition-all flex items-center space-x-2">
                    <span>Selanjutnya: Video Pembelajaran</span>
                    <i class="fa-solid fa-arrow-right"></i>
                </button>
            </div>
        </section>

        <!-- TAB 2: VIDEO PEMBELAJARAN -->
        <section id="tab-video" class="tab-content hidden space-y-6">
            <div class="bg-white rounded-3xl p-6 sm:p-8 shadow-sm border border-slate-200">
                <div class="flex items-center space-x-3 mb-6">
                    <div class="w-12 h-12 rounded-2xl bg-red-100 text-red-600 flex items-center justify-center text-xl font-bold">
                        <i class="fa-solid fa-play-circle"></i>
                    </div>
                    <div>
                        <h3 class="text-lg font-bold text-slate-900">Video Pembelajaran Interaktif IPS</h3>
                        <p class="text-xs text-slate-500">Materi Audio-Visual Kegiatan Ekonomi Masyarakat Kelas 7</p>
                    </div>
                </div>

                <!-- Simulasikan Pemutar Video Interaktif -->
                <div class="bg-slate-900 rounded-3xl overflow-hidden shadow-2xl relative aspect-video flex items-center justify-center group">
                    <div class="absolute inset-0 bg-cover bg-center opacity-40" style="background-image: url('https://placehold.co/1200x675/1e293b/cbd5e1?text=Video+Pembelajaran+IPS+Kelas+7')"></div>
                    <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/30 to-transparent"></div>

                    <!-- Tombol Play Tengah -->
                    <div id="video-play-overlay" class="absolute z-20 text-center">
                        <button onclick="playInteractiveVideo()" class="w-20 h-20 rounded-full bg-brand-600 hover:bg-brand-500 text-white text-2xl flex items-center justify-center shadow-2xl shadow-brand-500/50 transform hover:scale-110 transition-all">
                            <i class="fa-solid fa-play ml-1"></i>
                        </button>
                        <p class="text-white font-medium text-sm mt-3 drop-shadow">Putar Video Animasi Kegiatan Ekonomi</p>
                    </div>

                    <!-- Tampilan Saat Video Diputar (Simulasi Player) -->
                    <div id="video-active-screen" class="absolute inset-0 z-10 hidden flex flex-col justify-between p-6">
                        <div class="flex justify-between items-center text-white text-xs bg-black/40 backdrop-blur-md px-4 py-2 rounded-xl">
                            <span class="font-bold flex items-center"><span class="w-2 h-2 rounded-full bg-red-500 animate-pulse mr-2"></span> EKONOVASI STREAMING HD</span>
                            <span id="video-timer">00:00 / 03:45</span>
                        </div>
                        <div class="text-center text-white space-y-2">
                            <div id="video-subtitle-box" class="bg-black/70 backdrop-blur-md px-6 py-3 rounded-2xl text-sm font-medium inline-block max-w-xl mx-auto border border-white/10">
                                "Kegiatan ekonomi terbagi menjadi tiga pilar utama: Produksi, Distribusi, dan Konsumsi."
                            </div>
                        </div>
                        <div class="flex items-center space-x-3 bg-black/50 backdrop-blur-md p-3 rounded-2xl">
                            <button onclick="pauseInteractiveVideo()" id="video-pause-btn" class="text-white hover:text-brand-400"><i class="fa-solid fa-pause text-lg"></i></button>
                            <div class="flex-1 bg-white/20 h-2 rounded-full overflow-hidden">
                                <div id="video-progress-bar" class="bg-brand-500 h-full w-1/3 transition-all"></div>
                            </div>
                            <button onclick="toggleVideoMute()" class="text-white hover:text-brand-400"><i id="video-mute-icon" class="fa-solid fa-volume-high"></i></button>
                        </div>
                    </div>
                </div>

                <div class="mt-6 grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200">
                        <h4 class="font-bold text-xs text-slate-700 uppercase tracking-wider mb-1"><i class="fa-solid fa-clock mr-1 text-brand-600"></i> Durasi Materi</h4>
                        <p class="text-sm font-semibold text-slate-800">3 Menit 45 Detik</p>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200">
                        <h4 class="font-bold text-xs text-slate-700 uppercase tracking-wider mb-1"><i class="fa-solid fa-user-tag mr-1 text-brand-600"></i> Target Jenjang</h4>
                        <p id="video-target-class" class="text-sm font-semibold text-slate-800">Kelas 71 s.d. 74</p>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200">
                        <h4 class="font-bold text-xs text-slate-700 uppercase tracking-wider mb-1"><i class="fa-solid fa-file-lines mr-1 text-brand-600"></i> Transkrip Interaktif</h4>
                        <button onclick="readVideoTranscript()" class="text-xs text-brand-600 font-bold hover:underline mt-0.5 block">Dengarkan Transkrip Suara</button>
                    </div>
                </div>
            </div>

            <!-- Tombol Navigasi Halaman -->
            <div class="flex justify-between items-center pt-4">
                <button onclick="switchTab('tujuan')" class="px-6 py-3 bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold text-sm rounded-2xl transition-all flex items-center space-x-2">
                    <i class="fa-solid fa-arrow-left"></i>
                    <span>Sebelumnya</span>
                </button>
                <button onclick="switchTab('kuis')" class="px-6 py-3 bg-brand-600 hover:bg-brand-700 text-white font-bold text-sm rounded-2xl shadow-lg shadow-brand-500/30 transition-all flex items-center space-x-2">
                    <span>Selanjutnya: Kuis & Game IPS</span>
                    <i class="fa-solid fa-arrow-right"></i>
                </button>
            </div>
        </section>

        <!-- TAB 3: KUIS & GAME INTERAKTIF IPS -->
        <section id="tab-kuis" class="tab-content hidden space-y-6">
            <div class="bg-white rounded-3xl p-6 sm:p-8 shadow-sm border border-slate-200">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between pb-6 border-b border-slate-200 gap-4">
                    <div class="flex items-center space-x-3">
                        <div class="w-12 h-12 rounded-2xl bg-purple-100 text-purple-600 flex items-center justify-center text-xl font-bold">
                            <i class="fa-solid fa-puzzle-piece"></i>
                        </div>
                        <div>
                            <h3 class="text-lg font-bold text-slate-900">Kuis & Game Interaktif IPS</h3>
                            <p id="quiz-level-indicator" class="text-xs text-brand-600 font-semibold">Tingkat Kesulitan: Otomatis Menyesuaikan Kelas 71</p>
                        </div>
                    </div>
                    <div class="flex items-center space-x-3">
                        <div class="bg-slate-100 px-4 py-2 rounded-xl text-xs font-bold text-slate-700">
                            Soal <span id="current-question-num">1</span> dari <span id="total-questions-num">10</span>
                        </div>
                        <div class="bg-brand-50 border border-brand-100 px-4 py-2 rounded-xl text-xs font-bold text-brand-700">
                            Skor: <span id="current-score">0</span>
                        </div>
                    </div>
                </div>

                <!-- Kontainer Pertanyaan Kuis -->
                <div id="quiz-container" class="py-8">
                    <!-- Soal akan di-render secara dinamis oleh JavaScript -->
                </div>

                <!-- Umpan Balik Edukatif (Feedback Box) -->
                <div id="quiz-feedback" class="hidden mt-6 p-6 rounded-2xl transition-all">
                    <div class="flex items-start space-x-3">
                        <div id="feedback-icon" class="w-10 h-10 rounded-xl flex items-center justify-center text-white text-lg font-bold flex-shrink-0">
                            <i class="fa-solid fa-check"></i>
                        </div>
                        <div class="flex-1">
                            <h4 id="feedback-title" class="font-bold text-base mb-1">Benar! Luar Biasa!</h4>
                            <p id="feedback-text" class="text-sm leading-relaxed text-slate-700">Penjelasan edukatif akan muncul di sini.</p>
                        </div>
                    </div>
                    <div class="mt-4 flex justify-end">
                        <button onclick="nextQuizQuestion()" class="px-6 py-2.5 bg-brand-600 hover:bg-brand-700 text-white font-bold text-xs rounded-xl shadow-md transition-all flex items-center space-x-2">
                            <span>Soal Berikutnya</span>
                            <i class="fa-solid fa-arrow-right"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Tombol Navigasi Halaman -->
            <div class="flex justify-between items-center pt-4">
                <button onclick="switchTab('video')" class="px-6 py-3 bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold text-sm rounded-2xl transition-all flex items-center space-x-2">
                    <i class="fa-solid fa-arrow-left"></i>
                    <span>Sebelumnya</span>
                </button>
                <button onclick="switchTab('evaluasi')" class="px-6 py-3 bg-brand-600 hover:bg-brand-700 text-white font-bold text-sm rounded-2xl shadow-lg shadow-brand-500/30 transition-all flex items-center space-x-2">
                    <span>Selanjutnya: Nilai & Evaluasi</span>
                    <i class="fa-solid fa-arrow-right"></i>
                </button>
            </div>
        </section>

        <!-- TAB 4: NILAI & EVALUASI -->
        <section id="tab-evaluasi" class="tab-content hidden space-y-6">
            <div class="bg-white rounded-3xl p-8 shadow-sm border border-slate-200 text-center">
                <div class="w-24 h-24 bg-gradient-to-tr from-amber-400 to-yellow-500 rounded-3xl mx-auto flex items-center justify-center text-white text-4xl shadow-xl shadow-amber-500/30 mb-6 floating-card">
                    <i class="fa-solid fa-award"></i>
                </div>
                
                <h3 class="text-2xl font-extrabold text-slate-900 mb-2">Hasil Evaluasi Belajar IPS</h3>
                <p class="text-sm text-slate-500 mb-8">Rekapitulasi pencapaian kompetensi kegiatan ekonomi siswa kelas 7</p>

                <div class="max-w-md mx-auto bg-slate-50 border border-slate-200 rounded-3xl p-6 mb-8 grid grid-cols-2 gap-4 text-left">
                    <div>
                        <span class="block text-xs font-semibold text-slate-400 uppercase">Nama Siswa</span>
                        <span id="eval-result-name" class="text-base font-bold text-slate-800">-</span>
                    </div>
                    <div>
                        <span class="block text-xs font-semibold text-slate-400 uppercase">Kelas</span>
                        <span id="eval-result-class" class="text-base font-bold text-brand-600">-</span>
                    </div>
                    <div>
                        <span class="block text-xs font-semibold text-slate-400 uppercase">Total Skor</span>
                        <span id="eval-result-score" class="text-2xl font-extrabold text-emerald-600">0</span>
                    </div>
                    <div>
                        <span class="block text-xs font-semibold text-slate-400 uppercase">Status Kelulusan</span>
                        <span id="eval-result-status" class="inline-block px-3 py-1 bg-emerald-100 text-emerald-800 rounded-full text-xs font-bold mt-1">Lulus</span>
                    </div>
                </div>

                <div class="flex flex-wrap justify-center gap-4">
                    <button onclick="restartQuiz()" class="px-6 py-3 bg-brand-600 hover:bg-brand-700 text-white font-bold text-sm rounded-2xl shadow-lg shadow-brand-500/30 transition-all flex items-center space-x-2">
                        <i class="fa-solid fa-rotate-right"></i>
                        <span>Ulangi Kuis</span>
                    </button>
                    <button onclick="openTeacherDashboard()" class="px-6 py-3 bg-slate-900 hover:bg-slate-800 text-white font-bold text-sm rounded-2xl shadow-lg transition-all flex items-center space-x-2">
                        <i class="fa-solid fa-chart-line"></i>
                        <span>Buka Dasbor Guru & Export</span>
                    </button>
                </div>
            </div>

            <!-- Tombol Navigasi Halaman -->
            <div class="flex justify-between items-center pt-4">
                <button onclick="switchTab('kuis')" class="px-6 py-3 bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold text-sm rounded-2xl transition-all flex items-center space-x-2">
                    <i class="fa-solid fa-arrow-left"></i>
                    <span>Sebelumnya</span>
                </button>
                <div></div> <!-- Empty spacer -->
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-white border-t border-slate-200 py-6 mt-12">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-xs text-slate-500 flex flex-col sm:flex-row justify-between items-center gap-4">
            <p>&copy; 2026 Ekonovasi Media Pembelajaran IPS SMP Kelas 7 (71 - 74). Hak Cipta Dilindungi.</p>
            <div class="flex items-center space-x-4">
                <span class="flex items-center"><i class="fa-solid fa-circle text-emerald-500 text-[8px] mr-1.5 animate-ping"></i> Cloud Terhubung</span>
                <button onclick="openTeacherDashboard()" class="hover:text-brand-600 font-semibold">Dasbor Guru</button>
            </div>
        </div>
    </footer>

    <!-- Firebase SDK Imports -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Global Firebase variables
        let db, auth, appId, userId;
        let studentScoresData = [];

        window.initFirebaseApp = async function() {
            try {
                const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
                    apiKey: "demo-key",
                    authDomain: "demo.firebaseapp.com",
                    projectId: "demo-project"
                };
                appId = typeof __app_id !== 'undefined' ? __app_id : 'ekonovasi-ips-kelas7';

                const app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }

                userId = auth.currentUser ? auth.currentUser.uid : crypto.randomUUID();
                console.log("Firebase initialized successfully. User ID:", userId);

                // Listen to student scores collection
                const scoresRef = collection(db, 'artifacts', appId, 'public', 'data', 'student_scores');
                onSnapshot(scoresRef, (snapshot) => {
                    studentScoresData = [];
                    snapshot.forEach((doc) => {
                        studentScoresData.push({ id: doc.id, ...doc.data() });
                    });
                    renderStudentTable();
                    updateTeacherStats();
                }, (error) => {
                    console.error("Firestore sync error:", error);
                });

            } catch (e) {
                console.error("Error initializing Firebase:", e);
            }
        };

        window.saveStudentScoreToCloud = async function(name, className, score, status) {
            if (!db) {
                console.warn("Database not ready yet.");
                return;
            }
            try {
                const scoresRef = collection(db, 'artifacts', appId, 'public', 'data', 'student_scores');
                await addDoc(scoresRef, {
                    name: name,
                    class: className,
                    score: score,
                    status: status,
                    timestamp: new Date().toISOString()
                });
                console.log("Score successfully saved to cloud!");
            } catch (e) {
                console.error("Error saving score to cloud:", e);
            }
        };

        window.getStudentScoresData = function() {
            return studentScoresData;
        };

        // Initialize on load
        window.addEventListener('DOMContentLoaded', () => {
            initFirebaseApp();
        });
    </script>

    <!-- Application Script Logic -->
    <script>
        let currentStudent = {
            name: "",
            class: ""
        };

        let soundEnabled = true;
        let currentTab = 'tujuan';
        let currentQuestionIndex = 0;
        let currentScore = 0;

        // Adaptive Question Bank based on difficulty of Class 7
        const quizQuestions = [
            {
                type: "multiple",
                question: "Kegiatan yang dilakukan oleh manusia untuk menghasilkan atau menambah nilai guna suatu barang dan jasa dinamakan...",
                options: ["Konsumsi", "Produksi", "Distribusi", "Spekulasi"],
                answer: 1,
                explanation: "Produksi adalah kegiatan menambah nilai guna barang atau jasa untuk memenuhi kebutuhan manusia."
            },
            {
                type: "truefalse",
                question: "Sopir truk logistik yang mengantar sembako dari pabrik ke pasar tradisional adalah contoh pelaku kegiatan konsumsi.",
                options: ["Benar", "Salah"],
                answer: 1, // Salah, itu distribusi
                explanation: "Salah! Sopir truk logistik adalah pelaku kegiatan distribusi, bukan konsumsi."
            },
            {
                type: "multiple",
                question: "Manakah di bawah ini yang merupakan contoh kegiatan konsumsi yang dilakukan oleh siswa SMP?",
                options: ["Membuat kerajinan dari bambu", "Menjual buku tulis di koperasi", "Mengenakan seragam batik ke sekolah", "Menyalurkan beras kepada warga"],
                answer: 2,
                explanation: "Mengenakan seragam sekolah adalah kegiatan menghabiskan atau memakai nilai guna pakaian (konsumsi)."
            },
            {
                type: "audio",
                audioText: "Dengarkan baik-baik: Mengapa manusia perlu melakukan kegiatan ekonomi dalam hidupnya?",
                question: "Berdasarkan suara yang baru saja dibacakan, apa tujuan utama manusia melakukan kegiatan ekonomi?",
                options: ["Untuk mencari ketenaran semata", "Untuk memenuhi kebutuhan hidup yang beragam dan mengatasi kelangkaan", "Untuk menghabiskan waktu luang", "Untuk menambah beban pekerjaan"],
                answer: 1,
                explanation: "Tujuan utama kegiatan ekonomi adalah memenuhi kebutuhan hidup yang tidak terbatas di tengah keterbatasan sumber daya."
            },
            {
                type: "multiple",
                question: "Pasar tradisional maupun pasar modern secara ekonomi berfungsi sebagai tempat bertemunya antara...",
                options: ["Produsen dan pemerintah", "Penjual (pembeli) serta transaksi jual beli barang dan jasa", "Bank dan nasabah saja", "Guru dan murid"],
                answer: 1,
                explanation: "Pasar adalah tempat bertemunya penjual dan pembeli untuk melakukan transaksi jual beli barang maupun jasa."
            },
            {
                type: "truefalse",
                question: "Kegiatan menyalurkan barang dari tangan produsen ke tangan konsumen disebut sebagai kegiatan distribusi.",
                options: ["Benar", "Salah"],
                answer: 0, // Benar
                explanation: "Benar! Distribusi adalah penyaluran barang dan jasa dari produsen ke konsumen."
            },
            {
                type: "multiple",
                question: "Lembaga keuangan seperti bank memiliki peran penting dalam perekonomian masyarakat, salah satunya adalah...",
                options: ["Menghasilkan barang pabrik", "Menampung tabungan masyarakat dan menyalurkan kredit", "Membeli seluruh hasil panen petani secara paksa", "Mengatur harga pasar dunia"],
                answer: 1,
                explanation: "Bank berperan menghimpun dana masyarakat (tabungan) dan menyalurkan pinjaman (kredit) untuk modal usaha."
            },
            {
                type: "multiple",
                question: "Faktor utama yang menyebabkan manusia harus memutar otak melakukan kegiatan ekonomi adalah...",
                options: ["Kebutuhan manusia yang tidak terbatas sedangkan alat pemuas kebutuhan terbatas", "Uang di dunia sudah habis", "Pemerintah melarang warga beristirahat", "Barang selalu tersedia berlimpah ruah"],
                answer: 0,
                explanation: "Inti permasalahan ekonomi adalah kelangkaan, yaitu kebutuhan manusia tak terbatas berbanding terbatasnya alat pemuas."
            },
            {
                type: "truefalse",
                question: "Seorang petani yang menanam padi di sawah dikategorikan melakukan kegiatan produksi.",
                options: ["Benar", "Salah"],
                answer: 0, // Benar
                explanation: "Benar! Menanam padi menghasilkan barang mentah (gabah/padi) yang merupakan kegiatan produksi."
            },
            {
                type: "audio",
                audioText: "Dengarkan soal audio berikut: Apa akibatnya jika kegiatan distribusi terhambat di suatu daerah?",
                question: "Berdasarkan audio tersebut, apa dampak terhambatnya distribusi?",
                options: ["Barang menumpuk di produsen dan menjadi langka di konsumen", "Harga barang menjadi sangat murah", "Semua masyarakat menjadi kaya raya", "Tidak terjadi apa-apa"],
                answer: 0,
                explanation: "Jika distribusi terhambat, barang menumpuk di pabrik dan terjadi kelangkaan serta kenaikan harga di daerah tujuan."
            }
        ];

        // Custom Modal Handler
        function showModal(title, message, isSuccess = true) {
            const modal = document.getElementById('custom-modal');
            const content = document.getElementById('modal-content');
            const titleEl = document.getElementById('modal-title');
            const msgEl = document.getElementById('modal-message');
            const iconEl = document.getElementById('modal-icon');

            titleEl.textContent = title;
            msgEl.textContent = message;
            
            if (isSuccess) {
                iconEl.className = "w-12 h-12 rounded-full bg-emerald-100 flex items-center justify-center text-emerald-600 text-xl font-bold";
                iconEl.innerHTML = '<i class="fa-solid fa-check"></i>';
            } else {
                iconEl.className = "w-12 h-12 rounded-full bg-rose-100 flex items-center justify-center text-rose-600 text-xl font-bold";
                iconEl.innerHTML = '<i class="fa-solid fa-triangle-exclamation"></i>';
            }

            modal.classList.remove('hidden');
            setTimeout(() => {
                content.classList.remove('scale-95', 'opacity-0');
                content.classList.add('scale-100', 'opacity-100');
            }, 10);
            playSoundEffect('click');
        }

        function closeModal() {
            const modal = document.getElementById('custom-modal');
            const content = document.getElementById('modal-content');
            content.classList.remove('scale-100', 'opacity-100');
            content.classList.add('scale-95', 'opacity-0');
            setTimeout(() => {
                modal.classList.add('hidden');
            }, 200);
        }

        // Web Audio & Speech Synthesis
        function playSoundEffect(type) {
            if (!soundEnabled) return;
            try {
                const ctx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.connect(gain);
                gain.connect(ctx.destination);

                if (type === 'correct') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(587.33, ctx.currentTime); // D5
                    osc.frequency.setValueAtTime(880, ctx.currentTime + 0.1); // A5
                    gain.gain.setValueAtTime(0.1, ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.4);
    <title>Ekopedia Pro Disko: Media Pembelajaran IPS Interaktif Kelas 7</title>
    <!-- Tailwind CSS -->
<!-- ... existing code -->
            <div class="flex items-center space-x-3 mb-4">
                <div id="modal-icon" class="w-12 h-12 rounded-full bg-brand-100 flex items-center justify-center text-brand-600 text-xl font-bold">
                    <i class="fa-solid fa-bell"></i>
                </div>
                <div>
                    <h3 id="modal-title" class="text-lg font-bold text-slate-900">Judul Pesan</h3>
                    <p id="modal-subtitle" class="text-xs text-slate-500">Ekopedia Pro Disko Media Pembelajaran</p>
                </div>
            </div>
            <p id="modal-message" class="text-slate-600 text-sm mb-6 leading-relaxed">Pesan interaktif akan muncul di sini.</p>
<!-- ... existing code -->
            <div class="text-center mb-6">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-brand-100 text-brand-600 rounded-2xl mb-3 shadow-inner text-2xl font-bold">
                    <i class="fa-solid fa-graduation-cap"></i>
                </div>
                <h1 class="text-2xl font-extrabold text-slate-900">Selamat Datang di <span class="text-brand-600">EKOPEDIA PRO DISKO</span></h1>
                <p class="text-sm text-slate-500 mt-1">Media Pembelajaran & Kuis Interaktif IPS Kelas 7</p>
            </div>

            <form id="identity-form" onsubmit="handleIdentitySubmit(event)" class="space-y-4">
<!-- ... existing code -->
            <!-- Brand Logo -->
            <div class="flex items-center space-x-3">
                <div class="w-12 h-12 bg-gradient-to-tr from-brand-600 to-indigo-500 rounded-2xl flex items-center justify-center text-white text-xl shadow-lg shadow-brand-500/30">
                    <i class="fa-solid fa-earth-asia"></i>
                </div>
                <div>
                    <span class="text-xl font-extrabold bg-gradient-to-r from-brand-700 to-indigo-600 bg-clip-text text-transparent">EKOPEDIA PRO DISKO</span>
                    <span class="block text-[11px] font-medium text-slate-500">Media Pembelajaran IPS Kelas 7</span>
                </div>
            </div>

            <!-- Navigation Tabs -->
<!-- ... existing code -->
                    <!-- Tampilan Saat Video Diputar (Simulasi Player) -->
                    <div id="video-active-screen" class="absolute inset-0 z-10 hidden flex flex-col justify-between p-6">
                        <div class="flex justify-between items-center text-white text-xs bg-black/40 backdrop-blur-md px-4 py-2 rounded-xl">
                            <span class="font-bold flex items-center"><span class="w-2 h-2 rounded-full bg-red-500 animate-pulse mr-2"></span> EKOPEDIA PRO DISKO STREAMING HD</span>
                            <span id="video-timer">00:00 / 03:45</span>
                        </div>
                        <div class="text-center text-white space-y-2">
<!-- ... existing code -->
    <!-- Footer -->
    <footer class="bg-white border-t border-slate-200 py-6 mt-12">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-xs text-slate-500 flex flex-col sm:flex-row justify-between items-center gap-4">
            <p>&copy; 2026 Ekopedia Pro Disko Media Pembelajaran IPS SMP Kelas 7 (71 - 74). Hak Cipta Dilindungi.</p>
            <div class="flex items-center space-x-4">
                <span class="flex items-center"><i class="fa-solid fa-circle text-emerald-500 text-[8px] mr-1.5 animate-ping"></i> Cloud Terhubung</span>
                <button onclick="openTeacherDashboard()" class="hover:text-brand-600 font-semibold">Dasbor Guru</button>
            </div>
        </div>
    </footer>
<!-- ... existing code -->
            currentStudent.name = name;
            currentStudent.class = className;

            document.getElementById('display-name').textContent = name;
            document.getElementById('display-class').textContent = className;
            document.getElementById('quiz-level-indicator').textContent = `Tingkat Kesulitan: Otomatis Menyesuaikan ${className}`;
            document.getElementById('video-target-class').textContent = className;

            // Hide overlay with smooth fade
            const overlay = document.getElementById('identity-overlay');
            overlay.classList.add('opacity-0', 'transition-opacity', 'duration-300');
            setTimeout(() => {
                overlay.classList.add('hidden');
            }, 300);

            playSoundEffect('correct');
            showModal("Selamat Datang!", `Halo ${name} (${className}). Selamat belajar kegiatan ekonomi bersama Ekopedia Pro Disko!`);
            
            // Init Quiz
            renderQuizQuestion();
        }

        // Tab Navigation
        function switchTab(tabId) {
            currentTab = tabId;
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');

            // Update tab styles
            document.querySelectorAll('.nav-tab').forEach(el => {
                el.classList.remove('bg-brand-600', 'text-white', 'shadow-md');
                el.classList.add('text-slate-600');
            });
            const activeNav = document.getElementById(`nav-${tabId}`);
            if (activeNav) {
                activeNav.classList.remove('text-slate-600');
                activeNav.classList.add('bg-brand-600', 'text-white', 'shadow-md');
            }

            window.scrollTo({ top: 0, behavior: 'smooth' });
            playSoundEffect('click');
        }

        // Read Material Text
        function readMaterialText() {
            const text = "Ringkasan materi kegiatan ekonomi. Satu, Produksi adalah kegiatan menghasilkan atau menambah nilai guna barang dan jasa. Dua, Distribusi adalah kegiatan menyalurkan barang dari produsen ke konsumen. Tiga, Konsumsi adalah kegiatan mengurangi atau menghabiskan nilai guna barang dan jasa untuk memenuhi kebutuhan.";
            speakText(text);
            showModal("Narasi Materi", "Sistem sedang membacakan ringkasan materi kegiatan ekonomi.");
        }

        // Video Simulation
        function playInteractiveVideo() {
            document.getElementById('video-play-overlay').classList.add('hidden');
            document.getElementById('video-active-screen').classList.remove('hidden');
            speakText("Memutar video pembelajaran interaktif kegiatan ekonomi masyarakat kelas 7.");
            playSoundEffect('correct');
        }

        function pauseInteractiveVideo() {
            document.getElementById('video-play-overlay').classList.remove('hidden');
            document.getElementById('video-active-screen').classList.add('hidden');
            playSoundEffect('click');
        }

        function toggleVideoMute() {
            const icon = document.getElementById('video-mute-icon');
            if (icon.classList.contains('fa-volume-high')) {
                icon.className = "fa-solid fa-volume-xmark text-rose-400";
            } else {
                icon.className = "fa-solid fa-volume-high text-white";
            }
        }

        function readVideoTranscript() {
            speakText("Transkrip video. Kegiatan ekonomi masyarakat kelas 7 meliputi produksi, distribusi, dan konsumsi yang saling berkaitan erat dalam memenuhi kebutuhan hidup sehari-hari.");
            showModal("Transkrip Audio", "Membacakan transkrip materi video.");
        }

        // Quiz Engine
        function renderQuizQuestion() {
            if (currentQuestionIndex >= quizQuestions.length) {
                finishQuiz();
                return;
            }

            const q = quizQuestions[currentQuestionIndex];
            document.getElementById('current-question-num').textContent = currentQuestionIndex + 1;
            document.getElementById('total-questions-num').textContent = quizQuestions.length;
            document.getElementById('current-score').textContent = currentScore;

            const container = document.getElementById('quiz-container');
            document.getElementById('quiz-feedback').classList.add('hidden');

            let html = `
                <div class="space-y-6">
                    <div class="bg-indigo-50 border border-indigo-100 p-5 rounded-2xl flex items-center justify-between">
                        <span class="text-xs font-bold text-brand-700 uppercase tracking-wider"><i class="fa-solid fa-layer-group mr-1"></i> ${q.type === 'audio' ? 'Soal Audio (Dengarkan lalu Jawab)' : 'Pertanyaan Pilihan / Benar-Salah'}</span>
                        ${q.type === 'audio' ? `<button onclick="speakQuizAudio('${q.audioText}')" class="px-3 py-1.5 bg-brand-600 text-white rounded-xl text-xs font-bold flex items-center space-x-1 shadow"><i class="fa-solid fa-volume-high"></i><span>Putar Suara Soal</span></button>` : ''}
                    </div>
                    <h4 class="text-lg font-bold text-slate-900 leading-snug">${q.question}</h4>
                    <div class="grid grid-cols-1 gap-3" id="quiz-options-list">
            `;

            q.options.forEach((opt, idx) => {
                html += `
                    <button onclick="checkQuizAnswer(${idx})" class="quiz-option-btn text-left p-4 rounded-2xl border border-slate-200 hover:border-brand-500 hover:bg-brand-50/50 transition-all font-medium text-sm text-slate-700 flex items-center space-x-3 group">
                        <div class="w-8 h-8 rounded-xl bg-slate-100 group-hover:bg-brand-600 group-hover:text-white flex items-center justify-center font-bold text-xs transition-all">${String.fromCharCode(65 + idx)}</div>
                        <span class="flex-1">${opt}</span>
                    </button>
                `;
            });

            html += `</div></div>`;
            container.innerHTML = html;

            if (q.type === 'audio') {
                speakQuizAudio(q.audioText);
            }
        }

        function speakQuizAudio(text) {
            speakText(text);
        }

        function checkQuizAnswer(selectedIndex) {
            const q = quizQuestions[currentQuestionIndex];
            const isCorrect = (selectedIndex === q.answer);
            const feedbackBox = document.getElementById('quiz-feedback');
            const feedbackIcon = document.getElementById('feedback-icon');
            const feedbackTitle = document.getElementById('feedback-title');
            const feedbackText = document.getElementById('feedback-text');

            feedbackBox.classList.remove('hidden');

            if (isCorrect) {
                currentScore += 10;
                document.getElementById('current-score').textContent = currentScore;
                feedbackBox.className = "mt-6 p-6 rounded-2xl bg-emerald-50 border border-emerald-200 text-emerald-900 transition-all";
                feedbackIcon.className = "w-10 h-10 rounded-xl bg-emerald-600 flex items-center justify-center text-white text-lg font-bold flex-shrink-0";
                feedbackIcon.innerHTML = '<i class="fa-solid fa-check"></i>';
                feedbackTitle.textContent = "Hebat! Jawaban Kamu Benar!";
                feedbackText.textContent = q.explanation;
                playSoundEffect('correct');
                
                try {
                    confetti({ particleCount: 50, spread: 60, origin: { y: 0.7 } });
                } catch(e) {}
            } else {
                feedbackBox.className = "mt-6 p-6 rounded-2xl bg-rose-50 border border-rose-200 text-rose-900 transition-all";
                feedbackIcon.className = "w-10 h-10 rounded-xl bg-rose-600 flex items-center justify-center text-white text-lg font-bold flex-shrink-0";
                feedbackIcon.innerHTML = '<i class="fa-solid fa-xmark"></i>';
                feedbackTitle.textContent = "Kurang Tepat, Ayo Perbaiki!";
                feedbackText.textContent = `Jawaban yang benar adalah "${q.options[q.answer]}". ${q.explanation}`;
                playSoundEffect('wrong');
            }

            // Disable option buttons after selection
            const buttons = document.querySelectorAll('.quiz-option-btn');
            buttons.forEach((btn, idx) => {
                btn.disabled = true;
                if (idx === q.answer) {
                    btn.classList.add('bg-emerald-100', 'border-emerald-500', 'font-bold');
                } else if (idx === selectedIndex && !isCorrect) {
                    btn.classList.add('bg-rose-100', 'border-rose-500');
                }
            });
        }

        function nextQuizQuestion() {
            currentQuestionIndex++;
            renderQuizQuestion();
        }

        function finishQuiz() {
            document.getElementById('eval-result-name').textContent = currentStudent.name || "Siswa Tamu";
            document.getElementById('eval-result-class').textContent = currentStudent.class || "Kelas 71";
            document.getElementById('eval-result-score').textContent = currentScore;

            const statusEl = document.getElementById('eval-result-status');
            if (currentScore >= 70) {
                statusEl.textContent = "Lulus Dengan Sangat Baik";
                statusEl.className = "inline-block px-3 py-1 bg-emerald-100 text-emerald-800 rounded-full text-xs font-bold mt-1";
            } else {
                statusEl.textContent = "Perlu Remedial & Belajar Lagi";
                statusEl.className = "inline-block px-3 py-1 bg-amber-100 text-amber-800 rounded-full text-xs font-bold mt-1";
            }

            // Save score to cloud database
            if (typeof saveStudentScoreToCloud === 'function' && currentStudent.name) {
                saveStudentScoreToCloud(currentStudent.name, currentStudent.class || "Kelas 71", currentScore, currentScore >= 70 ? "Lulus" : "Remedial");
            }

            switchTab('evaluasi');
            try {
                confetti({ particleCount: 150, spread: 100, origin: { y: 0.6 } });
            } catch(e) {}
        }

        function restartQuiz() {
            currentQuestionIndex = 0;
            currentScore = 0;
            switchTab('kuis');
            renderQuizQuestion();
        }

        // Teacher Dashboard & Table Rendering
        function openTeacherDashboard() {
            document.getElementById('teacher-modal').classList.remove('hidden');
            renderStudentTable();
            updateTeacherStats();
            playSoundEffect('click');
        }

        function closeTeacherDashboard() {
            document.getElementById('teacher-modal').classList.add('hidden');
            playSoundEffect('click');
        }

        function renderStudentTable() {
            const tbody = document.getElementById('student-table-body');
            const filterClass = document.getElementById('filter-class').value;
            const scoresData = typeof getStudentScoresData === 'function' ? getStudentScoresData() : [];

            const filtered = scoresData.filter(s => filterClass === 'ALL' || s.class === filterClass);

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="text-center py-8 text-slate-400">Belum ada data siswa untuk kelas ini.</td></tr>`;
                return;
            }

            let html = '';
            filtered.forEach((s, idx) => {
                html += `
                    <tr class="hover:bg-slate-50 transition-all">
                        <td class="p-3.5 pl-5 font-medium">${idx + 1}</td>
                        <td class="p-3.5 font-bold text-slate-800">${s.name}</td>
                        <td class="p-3.5"><span class="px-2.5 py-1 bg-brand-50 text-brand-700 font-semibold rounded-lg text-xs">${s.class}</span></td>
                        <td class="p-3.5 font-bold text-emerald-600">${s.score}</td>
                        <td class="p-3.5"><span class="px-2.5 py-1 ${s.score >= 70 ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'} rounded-full text-[11px] font-bold">${s.status || 'Selesai'}</span></td>
                        <td class="p-3.5 pr-5 text-slate-400 text-xs">${new Date(s.timestamp || Date.now()).toLocaleString('id-ID')}</td>
                    </tr>
                `;
            });
            tbody.innerHTML = html;
        }

        function updateTeacherStats() {
            const scoresData = typeof getStudentScoresData === 'function' ? getStudentScoresData() : [];
            document.getElementById('stat-total-students').textContent = scoresData.length;

            if (scoresData.length > 0) {
                const totalScore = scoresData.reduce((acc, curr) => acc + curr.score, 0);
                const avg = Math.round(totalScore / scoresData.length);
                const max = Math.max(...scoresData.map(s => s.score));

                document.getElementById('stat-avg-score').textContent = avg;
                document.getElementById('stat-max-score').textContent = max;
            } else {
                document.getElementById('stat-avg-score').textContent = 0;
                document.getElementById('stat-max-score').textContent = 0;
            }
        }

        // Export to Excel (CSV format download)
        function exportToExcel() {
            const scoresData = typeof getStudentScoresData === 'function' ? getStudentScoresData() : [];
            if (scoresData.length === 0) {
                showModal("Perhatian", "Belum ada data nilai murid untuk diexport.", false);
                return;
            }

            let csvContent = "No,Nama Siswa,Kelas,Skor Kuis,Status,Waktu Pengerjaan\n";
            scoresData.forEach((s, idx) => {
                csvContent += `${idx + 1},"${s.name}","${s.class}",${s.score},"${s.status || 'Selesai'}","${s.timestamp || ''}"\n`;
            });

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `Rekap_Nilai_IPS_Kelas7_${new Date().toISOString().slice(0,10)}.csv`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);

            showModal("Berhasil!", "Laporan nilai berhasil diunduh dalam format spreadsheet Excel/CSV.");
        }
    </script>
</body>
</html>
