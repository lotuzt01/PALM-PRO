<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PalmCore ERP - Finance & RAM Sawit</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f1f5f9; }
        .nav-btn.active { background: #059669 !important; color: white !important; }
        .card-shadow { box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05); }
        @media print { .no-print { display: none !important; } #print-area { display: block !important; } }
    </style>
</head>
<body class="text-slate-800">

    <!-- Sidebar Navigation -->
    <nav class="fixed left-0 top-0 h-full w-20 lg:w-64 bg-white border-r border-slate-200 z-50 no-print">
        <div class="p-6 flex flex-col h-full">
            <div class="flex items-center gap-3 mb-10">
                <div class="bg-emerald-600 p-2 rounded-xl text-white"><i class="fas fa-chart-line text-xl"></i></div>
                <span class="font-bold text-xl hidden lg:block text-emerald-900">Palm<span class="text-emerald-500">Finance</span></span>
            </div>
            <div class="space-y-1 flex-1 overflow-y-auto">
                <button onclick="navTo('dashboard')" id="nav-dashboard" class="nav-btn active w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 hover:bg-slate-50 transition-all">
                    <i class="fas fa-th-large w-5"></i><span class="font-bold hidden lg:block text-sm">Dashboard</span>
                </button>
                <button onclick="navTo('beli')" id="nav-beli" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 hover:bg-slate-50 transition-all">
                    <i class="fas fa-shopping-cart w-5"></i><span class="font-bold hidden lg:block text-sm">Pembelian TBS</span>
                </button>
                <button onclick="navTo('jual')" id="nav-jual" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 hover:bg-slate-50 transition-all">
                    <i class="fas fa-truck-loading w-5"></i><span class="font-bold hidden lg:block text-sm">Klaim PKS</span>
                </button>
                <button onclick="navTo('biaya')" id="nav-biaya" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 hover:bg-slate-50 transition-all">
                    <i class="fas fa-wallet w-5"></i><span class="font-bold hidden lg:block text-sm">Biaya & Modal</span>
                </button>
                <div class="pt-4 pb-2 text-[10px] font-black text-slate-400 uppercase tracking-widest hidden lg:block text-center lg:text-left">Laporan Keuangan</div>
                <button onclick="navTo('laba-rugi')" id="nav-laba-rugi" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 hover:bg-slate-50 transition-all">
                    <i class="fas fa-file-invoice-dollar w-5"></i><span class="font-bold hidden lg:block text-sm">Laba / Rugi</span>
                </button>
                <button onclick="navTo('neraca')" id="nav-neraca" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 hover:bg-slate-50 transition-all">
                    <i class="fas fa-balance-scale w-5"></i><span class="font-bold hidden lg:block text-sm">Neraca</span>
                </button>
                <button onclick="navTo('modal')" id="nav-modal" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 hover:bg-slate-50 transition-all">
                    <i class="fas fa-coins w-5"></i><span class="font-bold hidden lg:block text-sm">Perubahan Modal</span>
                </button>
            </div>

            <!-- Export Button -->
            <div class="mt-auto pt-6 border-t border-slate-100">
                <button onclick="exportFullReport()" class="w-full bg-emerald-100 hover:bg-emerald-200 text-emerald-700 font-bold p-3 rounded-xl flex items-center justify-center gap-2 transition-all">
                    <i class="fas fa-file-excel"></i> <span class="hidden lg:block">Export Excel</span>
                </button>
            </div>
        </div>
    </nav>

    <main class="ml-20 lg:ml-64 p-4 lg:p-8 no-print">
        
        <!-- DASHBOARD SECTION -->
        <div id="page-dashboard" class="page-content text-center lg:text-left">
            <div class="flex flex-col lg:flex-row lg:items-center justify-between mb-8 gap-4">
                <div>
                    <h1 class="text-3xl font-black text-slate-900">Dashboard Keuangan</h1>
                    <p class="text-slate-500">Monitor arus kas dan stok RAM secara real-time.</p>
                </div>
                <div class="flex gap-2 justify-center lg:justify-end">
                    <div class="bg-white p-4 rounded-2xl border border-slate-200 text-right min-w-[200px]">
                        <p class="text-[10px] font-black text-slate-400 uppercase">Saldo Kas Saat Ini</p>
                        <h3 id="dash-cash" class="text-xl font-black text-emerald-600">Rp 0</h3>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
                <div class="bg-white p-6 rounded-3xl border border-slate-200 card-shadow">
                    <div class="flex items-center gap-3 mb-4 text-emerald-600"><i class="fas fa-warehouse text-xl"></i><span class="text-[10px] font-black uppercase">Stok di RAM</span></div>
                    <h2 id="dash-stok" class="text-3xl font-black">0 <span class="text-sm font-normal text-slate-400">Kg</span></h2>
                </div>
                <div class="bg-white p-6 rounded-3xl border border-slate-200 card-shadow">
                    <div class="flex items-center gap-3 mb-4 text-blue-600"><i class="fas fa-truck-moving text-xl"></i><span class="text-[10px] font-black uppercase">Stok OTW PKS</span></div>
                    <h2 id="dash-otw" class="text-3xl font-black">0 <span class="text-sm font-normal text-slate-400">Kg</span></h2>
                </div>
                <div class="bg-white p-6 rounded-3xl border border-slate-200 card-shadow">
                    <div class="flex items-center gap-3 mb-4 text-amber-600"><i class="fas fa-compress-arrows-alt text-xl"></i><span class="text-[10px] font-black uppercase">Total Susut</span></div>
                    <h2 id="dash-susut" class="text-3xl font-black">0 <span class="text-sm font-normal text-slate-400">Kg</span></h2>
                </div>
                <div class="bg-slate-900 p-6 rounded-3xl border border-slate-800 card-shadow text-white">
                    <div class="flex items-center gap-3 mb-4 text-emerald-400"><i class="fas fa-funnel-dollar text-xl"></i><span class="text-[10px] font-black uppercase">Laba Berjalan</span></div>
                    <h2 id="dash-profit" class="text-2xl font-black">Rp 0</h2>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white p-8 rounded-[2.5rem] border border-slate-200 h-[400px]">
                    <h4 class="font-black mb-6 flex items-center gap-2"><i class="fas fa-chart-bar text-emerald-500"></i> Tren Pembelian 7 Hari</h4>
                    <canvas id="chartBeli"></canvas>
                </div>
                <div class="bg-white p-8 rounded-[2.5rem] border border-slate-200 overflow-hidden overflow-y-auto max-h-[400px]">
                    <h4 class="font-black mb-6 flex items-center gap-2"><i class="fas fa-history text-blue-500"></i> Transaksi Terakhir</h4>
                    <div id="recent-logs" class="space-y-4 text-left"></div>
                </div>
            </div>
        </div>

        <!-- PAGE PEMBELIAN -->
        <div id="page-beli" class="page-content hidden">
            <div class="max-w-4xl mx-auto bg-white p-10 rounded-[3rem] shadow-2xl border-t-[12px] border-emerald-500">
                <h2 class="text-3xl font-black text-slate-900 mb-8">Penerimaan TBS</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div class="space-y-4">
                        <input id="in-beli-nama" type="text" placeholder="Nama Pemasok" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                        <div class="grid grid-cols-2 gap-4">
                            <input id="in-beli-bruto" type="number" oninput="calcBeli()" placeholder="Brutto (Kg)" class="w-full p-4 border rounded-2xl font-black">
                            <input id="in-beli-tara" type="number" oninput="calcBeli()" placeholder="Tarra (Kg)" class="w-full p-4 border rounded-2xl font-black">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <input id="in-beli-pot" type="number" oninput="calcBeli()" value="3" class="w-full p-4 bg-rose-50 border-rose-100 rounded-2xl font-black text-rose-600">
                            <input id="in-beli-harga" type="number" oninput="calcBeli()" placeholder="Harga Beli" class="w-full p-4 bg-emerald-50 border-emerald-200 rounded-2xl font-black text-emerald-700">
                        </div>
                        <input id="in-beli-date" type="date" class="w-full p-4 border rounded-2xl font-bold">
                    </div>
                    <div class="bg-emerald-900 rounded-[2.5rem] p-8 text-white flex flex-col justify-between">
                        <div>
                            <p class="text-[10px] font-black opacity-50 uppercase mb-4">Ringkasan Pembayaran</p>
                            <div class="flex justify-between border-b border-emerald-800 pb-2 mb-2"><span>Netto</span><span id="res-beli-netto" class="font-bold">0 Kg</span></div>
                            <div class="flex justify-between"><span>Potongan</span><span id="res-beli-potkg" class="text-rose-400">0 Kg</span></div>
                        </div>
                        <div class="text-center mt-10">
                            <h2 id="res-beli-total" class="text-4xl font-black mb-6">Rp 0</h2>
                            <button onclick="saveTx('BELI')" class="w-full py-5 bg-emerald-500 hover:bg-emerald-400 rounded-2xl font-black transition-all shadow-lg">SIMPAN & BAYAR</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- PAGE KLAIM PKS -->
        <div id="page-jual" class="page-content hidden">
            <div class="max-w-4xl mx-auto bg-white p-10 rounded-[3rem] shadow-2xl border-t-[12px] border-blue-500">
                <h2 class="text-3xl font-black text-slate-900 mb-8">Klaim Hasil Pabrik</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div class="space-y-4">
                        <select id="in-jual-ref" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                            <option value="">-- Pilih DO Belum Diklaim --</option>
                        </select>
                        <div class="grid grid-cols-2 gap-4">
                            <input id="in-jual-bruto" type="number" oninput="calcJual()" placeholder="Brutto PKS" class="w-full p-4 border rounded-2xl font-black text-blue-600">
                            <input id="in-jual-tara" type="number" oninput="calcJual()" placeholder="Tarra PKS" class="w-full p-4 border rounded-2xl font-black">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <input id="in-jual-sortasi" type="number" oninput="calcJual()" placeholder="Sortasi %" class="w-full p-4 border rounded-2xl font-black">
                            <input id="in-jual-harga" type="number" oninput="calcJual()" placeholder="Harga PKS" class="w-full p-4 border rounded-2xl font-black text-emerald-600">
                        </div>
                        <input id="in-jual-date" type="date" class="w-full p-4 border rounded-2xl font-bold">
                    </div>
                    <div class="bg-blue-900 rounded-[2.5rem] p-8 text-white flex flex-col justify-between text-center">
                        <div>
                            <p class="text-[10px] font-black opacity-50 uppercase mb-4">Total Klaim Piutang</p>
                            <h2 id="res-jual-total" class="text-4xl font-black mb-2">Rp 0</h2>
                            <p id="res-jual-netto" class="text-blue-300 font-bold">Netto PKS: 0 Kg</p>
                        </div>
                        <button onclick="saveTx('JUAL')" class="w-full py-5 bg-blue-500 hover:bg-blue-400 rounded-2xl font-black mt-8">SIMPAN KLAIM</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- PAGE BIAYA & MODAL -->
        <div id="page-biaya" class="page-content hidden">
            <div class="max-w-5xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white p-8 rounded-[2.5rem] border-t-8 border-rose-500 shadow-xl">
                    <h3 class="font-black text-xl mb-6">Input Biaya Operasional</h3>
                    <div class="space-y-4">
                        <input id="in-exp-ket" type="text" placeholder="Keterangan (Gaji, Bensin, dll)" class="w-full p-4 border rounded-2xl font-bold">
                        <input id="in-exp-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 border rounded-2xl font-black text-rose-600">
                        <input id="in-exp-date" type="date" class="w-full p-4 border rounded-2xl font-bold">
                        <button onclick="saveTx('BIAYA')" class="w-full py-4 bg-rose-600 text-white rounded-2xl font-black">CATAT BIAYA</button>
                    </div>
                </div>
                <div class="bg-white p-8 rounded-[2.5rem] border-t-8 border-amber-500 shadow-xl">
                    <h3 class="font-black text-xl mb-6">Manajemen Modal & Prive</h3>
                    <div class="space-y-4">
                        <select id="in-modal-tipe" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                            <option value="MODAL">Setoran Modal (Kas Masuk)</option>
                            <option value="PRIVE">Prive / Tarik Tunai (Kas Keluar)</option>
                        </select>
                        <input id="in-modal-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 border rounded-2xl font-black text-amber-600">
                        <input id="in-modal-date" type="date" class="w-full p-4 border rounded-2xl font-bold">
                        <button onclick="saveTx('CAPITAL')" class="w-full py-4 bg-amber-500 text-white rounded-2xl font-black">PROSES MODAL</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- LAPORAN LABA RUGI -->
        <div id="page-laba-rugi" class="page-content hidden">
            <div class="max-w-4xl mx-auto bg-white p-8 lg:p-12 rounded-[3rem] shadow-xl border border-slate-200">
                <div class="text-center mb-12 border-b-2 border-slate-100 pb-8">
                    <h2 class="text-3xl font-black text-slate-900 uppercase">Laporan Laba / Rugi</h2>
                    <p class="text-slate-400 font-bold" id="pl-period">Periode Berjalan 2026</p>
                </div>
                
                <div class="space-y-8">
                    <section>
                        <h4 class="font-black text-emerald-600 border-b mb-4 uppercase text-xs tracking-widest">Pendapatan</h4>
                        <div class="flex justify-between items-center py-2">
                            <span class="font-medium">Hasil Klaim Penjualan TBS (PKS)</span>
                            <span id="pl-jual" class="font-black">Rp 0</span>
                        </div>
                    </section>

                    <section>
                        <h4 class="font-black text-rose-600 border-b mb-4 uppercase text-xs tracking-widest">Beban Pokok Penjualan</h4>
                        <div class="flex justify-between items-center py-2">
                            <span class="font-medium">Pembelian TBS (RAM)</span>
                            <span id="pl-beli" class="font-black text-rose-600">(Rp 0)</span>
                        </div>
                        <div class="flex justify-between items-center py-4 border-t-2 border-slate-900 mt-2">
                            <span class="font-black text-lg">Laba Kotor</span>
                            <span id="pl-laba-kotor" class="font-black text-lg">Rp 0</span>
                        </div>
                    </section>

                    <section>
                        <h4 class="font-black text-slate-400 border-b mb-4 uppercase text-xs tracking-widest">Beban Operasional</h4>
                        <div id="pl-list-biaya" class="space-y-2"></div>
                        <div class="flex justify-between items-center py-2 font-bold italic">
                            <span>Total Beban Operasional</span>
                            <span id="pl-total-biaya" class="text-rose-600">Rp 0</span>
                        </div>
                    </section>

                    <section class="bg-slate-900 text-white p-6 lg:p-8 rounded-3xl mt-12 flex justify-between items-center">
                        <div>
                            <h3 class="text-xl lg:text-2xl font-black uppercase">Laba Bersih</h3>
                        </div>
                        <h2 id="pl-netto" class="text-3xl lg:text-4xl font-black text-emerald-400">Rp 0</h2>
                    </section>
                </div>
            </div>
        </div>

        <!-- LAPORAN NERACA -->
        <div id="page-neraca" class="page-content hidden">
            <div class="max-w-6xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white p-8 lg:p-10 rounded-[3rem] shadow-xl border border-slate-200">
                    <h3 class="text-2xl font-black mb-8 text-emerald-700">Aktiva (Aset)</h3>
                    <div class="space-y-6">
                        <div>
                            <p class="text-[10px] font-black text-slate-400 uppercase mb-2">Aset Lancar</p>
                            <div class="flex justify-between py-2 border-b border-slate-50"><span>Kas & Bank</span><span id="bl-kas" class="font-bold">Rp 0</span></div>
                            <div class="flex justify-between py-2 border-b border-slate-50"><span>Persediaan TBS (RAM)</span><span id="bl-stok" class="font-bold">Rp 0</span></div>
                            <div class="flex justify-between py-2 border-b border-slate-50"><span>TBS dalam Perjalanan (OTW)</span><span id="bl-otw" class="font-bold">Rp 0</span></div>
                            <div class="flex justify-between py-2 border-b border-slate-50"><span>Piutang PKS (Belum Cair)</span><span id="bl-piutang" class="font-bold">Rp 0</span></div>
                        </div>
                        <div class="pt-10 flex justify-between items-center border-t-4 border-emerald-500">
                            <span class="text-xl font-black uppercase">Total Aktiva</span>
                            <span id="bl-total-aktiva" class="text-2xl font-black text-emerald-600">Rp 0</span>
                        </div>
                    </div>
                </div>

                <div class="bg-white p-8 lg:p-10 rounded-[3rem] shadow-xl border border-slate-200">
                    <h3 class="text-2xl font-black mb-8 text-blue-700">Pasiva (Kewajiban & Modal)</h3>
                    <div class="space-y-6">
                        <div>
                            <p class="text-[10px] font-black text-slate-400 uppercase mb-2">Modal Pemilik</p>
                            <div class="flex justify-between py-2 border-b border-slate-50"><span>Modal Awal / Disetor</span><span id="bl-modal-disetor" class="font-bold">Rp 0</span></div>
                            <div class="flex justify-between py-2 border-b border-slate-50"><span>Laba Ditahan / Berjalan</span><span id="bl-laba-berjalan" class="font-bold">Rp 0</span></div>
                            <div class="flex justify-between py-2 border-b border-slate-50 text-rose-500"><span>Prive (Penarikan)</span><span id="bl-prive" class="font-bold">(Rp 0)</span></div>
                        </div>
                        <div class="pt-10 flex justify-between items-center border-t-4 border-blue-500">
                            <span class="text-xl font-black uppercase">Total Pasiva</span>
                            <span id="bl-total-pasiva" class="text-2xl font-black text-blue-600">Rp 0</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- LAPORAN PERUBAHAN MODAL -->
        <div id="page-modal" class="page-content hidden">
            <div class="max-w-4xl mx-auto bg-white p-8 lg:p-12 rounded-[3rem] shadow-xl">
                 <div class="text-center mb-12">
                    <h2 class="text-3xl font-black text-slate-900 uppercase">Laporan Perubahan Modal</h2>
                    <p class="text-slate-400 font-bold">Periode Berakhir 2026</p>
                </div>
                <div class="space-y-4 max-w-2xl mx-auto">
                    <div class="flex justify-between py-4 border-b">
                        <span class="font-medium text-slate-500">Modal Awal Periode</span>
                        <span id="cm-awal" class="font-black text-lg">Rp 0</span>
                    </div>
                    <div class="flex justify-between py-4 border-b">
                        <span class="font-medium text-slate-500">Laba Bersih Tahun Berjalan</span>
                        <span id="cm-laba" class="font-black text-lg text-emerald-600">+ Rp 0</span>
                    </div>
                    <div class="flex justify-between py-4 border-b">
                        <span class="font-medium text-slate-500 text-rose-500">Pengambilan Pribadi (Prive)</span>
                        <span id="cm-prive" class="font-black text-lg text-rose-500">- Rp 0</span>
                    </div>
                    <div class="flex justify-between py-8 mt-10 border-t-4 border-slate-900 bg-slate-50 px-6 rounded-2xl">
                        <span class="text-xl font-black">MODAL AKHIR</span>
                        <span id="cm-akhir" class="text-2xl font-black text-blue-600">Rp 0</span>
                    </div>
                </div>
            </div>
        </div>

    </main>

    <script>
        let db = JSON.parse(localStorage.getItem('palm_finance_db')) || { tx: [] };

        const navTo = (id) => {
            document.querySelectorAll('.page-content').forEach(p => p.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('page-' + id).classList.remove('hidden');
            document.getElementById('nav-' + id).classList.add('active');
            renderAll();
        };

        const setToday = () => {
            const now = new Date().toISOString().split('T')[0];
            ['in-beli-date', 'in-jual-date', 'in-exp-date', 'in-modal-date'].forEach(i => {
                const el = document.getElementById(i); if(el) el.value = now;
            });
        };

        function calcBeli() {
            const b = parseFloat(document.getElementById('in-beli-bruto').value) || 0;
            const t = parseFloat(document.getElementById('in-beli-tara').value) || 0;
            const p = parseFloat(document.getElementById('in-beli-pot').value) || 0;
            const h = parseFloat(document.getElementById('in-beli-harga').value) || 0;
            const kotor = b - t;
            const potkg = Math.round(kotor * (p/100));
            const netto = kotor - potkg;
            const total = netto * h;
            document.getElementById('res-beli-netto').innerText = netto.toLocaleString() + ' Kg';
            document.getElementById('res-beli-potkg').innerText = potkg.toLocaleString() + ' Kg';
            document.getElementById('res-beli-total').innerText = 'Rp ' + total.toLocaleString();
            return { netto, total, kotor, h };
        }

        function calcJual() {
            const b = parseFloat(document.getElementById('in-jual-bruto').value) || 0;
            const t = parseFloat(document.getElementById('in-jual-tara').value) || 0;
            const s = parseFloat(document.getElementById('in-jual-sortasi').value) || 0;
            const h = parseFloat(document.getElementById('in-jual-harga').value) || 0;
            const kotor = b - t;
            const netto = kotor - Math.round(kotor * (s/100));
            const total = netto * h;
            document.getElementById('res-jual-netto').innerText = 'Netto PKS: ' + netto.toLocaleString() + ' Kg';
            document.getElementById('res-jual-total').innerText = 'Rp ' + total.toLocaleString();
            return { netto, total, kotor };
        }

        function saveTx(tipe) {
            let data = { id: Date.now(), tipe, ts: Date.now() };
            
            if(tipe === 'BELI') {
                const r = calcBeli();
                data.nama = document.getElementById('in-beli-nama').value || 'Pemasok';
                data.netto = r.netto; data.total = r.total; data.h = r.h;
                data.ts = new Date(document.getElementById('in-beli-date').value).getTime();
            } else if(tipe === 'JUAL') {
                const r = calcJual();
                data.refId = document.getElementById('in-jual-ref').value;
                data.nama = "Klaim PKS DO " + data.refId;
                data.netto = r.netto; data.total = r.total;
                data.ts = new Date(document.getElementById('in-jual-date').value).getTime();
                const ref = db.tx.find(x => x.id == data.refId);
                if(ref) data.susut = ref.netto - r.kotor;
            } else if(tipe === 'BIAYA') {
                data.nama = document.getElementById('in-exp-ket').value;
                data.total = parseFloat(document.getElementById('in-exp-nominal').value) || 0;
                data.ts = new Date(document.getElementById('in-exp-date').value).getTime();
            } else if(tipe === 'CAPITAL') {
                data.tipeModal = document.getElementById('in-modal-tipe').value;
                data.total = parseFloat(document.getElementById('in-modal-nominal').value) || 0;
                data.nama = data.tipeModal === 'MODAL' ? 'Setoran Modal' : 'Prive';
                data.ts = new Date(document.getElementById('in-modal-date').value).getTime();
            }

            db.tx.unshift(data);
            localStorage.setItem('palm_finance_db', JSON.stringify(db));
            renderAll(); setToday();
            alert("Berhasil!");
        }

        function renderAll() {
            let stok=0, otw=0, piutang=0, susut=0, beliRp=0, jualRp=0, biayaRp=0, kas=0, modalRp=0, priveRp=0;
            const claimed = db.tx.filter(x => x.tipe === 'JUAL').map(x => x.refId);
            
            db.tx.forEach(t => {
                if(t.tipe === 'BELI') {
                    beliRp += t.total; kas -= t.total;
                    if(!claimed.includes(t.id.toString())) otw += t.netto;
                }
                if(t.tipe === 'JUAL') {
                    jualRp += t.total; piutang += t.total;
                    if(t.susut) susut += t.susut;
                }
                if(t.tipe === 'BIAYA') { biayaRp += t.total; kas -= t.total; }
                if(t.tipe === 'CAPITAL') {
                    if(t.tipeModal === 'MODAL') { modalRp += t.total; kas += t.total; }
                    else { priveRp += t.total; kas -= t.total; }
                }
            });

            const labaBersih = jualRp - beliRp - biayaRp;
            const estStokVal = (otw + stok) * 2000;

            document.getElementById('dash-stok').innerText = stok.toLocaleString();
            document.getElementById('dash-otw').innerText = otw.toLocaleString();
            document.getElementById('dash-susut').innerText = susut.toLocaleString();
            document.getElementById('dash-cash').innerText = 'Rp ' + kas.toLocaleString();
            document.getElementById('dash-profit').innerText = 'Rp ' + labaBersih.toLocaleString();

            document.getElementById('pl-jual').innerText = 'Rp ' + jualRp.toLocaleString();
            document.getElementById('pl-beli').innerText = '(Rp ' + beliRp.toLocaleString() + ')';
            document.getElementById('pl-laba-kotor').innerText = 'Rp ' + (jualRp - beliRp).toLocaleString();
            document.getElementById('pl-total-biaya').innerText = 'Rp ' + biayaRp.toLocaleString();
            document.getElementById('pl-netto').innerText = 'Rp ' + labaBersih.toLocaleString();
            
            const plList = document.getElementById('pl-list-biaya');
            plList.innerHTML = '';
            db.tx.filter(x => x.tipe === 'BIAYA').forEach(b => {
                plList.insertAdjacentHTML('beforeend', `<div class="flex justify-between py-1 text-sm"><span>${b.nama}</span><span>Rp ${b.total.toLocaleString()}</span></div>`);
            });

            document.getElementById('bl-kas').innerText = 'Rp ' + kas.toLocaleString();
            document.getElementById('bl-stok').innerText = 'Rp ' + (stok * 2000).toLocaleString();
            document.getElementById('bl-otw').innerText = 'Rp ' + (otw * 2000).toLocaleString();
            document.getElementById('bl-piutang').innerText = 'Rp ' + piutang.toLocaleString();
            document.getElementById('bl-total-aktiva').innerText = 'Rp ' + (kas + estStokVal + piutang).toLocaleString();

            document.getElementById('bl-modal-disetor').innerText = 'Rp ' + modalRp.toLocaleString();
            document.getElementById('bl-laba-berjalan').innerText = 'Rp ' + labaBersih.toLocaleString();
            document.getElementById('bl-prive').innerText = '(Rp ' + priveRp.toLocaleString() + ')';
            document.getElementById('bl-total-pasiva').innerText = 'Rp ' + (modalRp + labaBersih - priveRp).toLocaleString();

            document.getElementById('cm-awal').innerText = 'Rp ' + modalRp.toLocaleString();
            document.getElementById('cm-laba').innerText = '+ Rp ' + labaBersih.toLocaleString();
            document.getElementById('cm-prive').innerText = '- Rp ' + priveRp.toLocaleString();
            document.getElementById('cm-akhir').innerText = 'Rp ' + (modalRp + labaBersih - priveRp).toLocaleString();

            const sel = document.getElementById('in-jual-ref');
            const curVal = sel.value;
            sel.innerHTML = '<option value="">-- Pilih DO Belum Diklaim --</option>';
            db.tx.filter(x => x.tipe === 'BELI' && !claimed.includes(x.id.toString())).forEach(x => {
                sel.insertAdjacentHTML('beforeend', `<option value="${x.id}">DO-${x.id} | ${x.nama} (${x.netto}Kg)</option>`);
            });
            sel.value = curVal;

            const logs = document.getElementById('recent-logs');
            logs.innerHTML = '';
            db.tx.slice(0, 5).forEach(x => {
                logs.insertAdjacentHTML('beforeend', `
                    <div class="flex items-center justify-between p-4 bg-slate-50 rounded-2xl">
                        <div><p class="font-black text-sm">${x.nama}</p><p class="text-[10px] text-slate-400 uppercase font-bold">${x.tipe}</p></div>
                        <div class="text-right"><p class="font-black text-sm ${x.tipe==='BELI'||x.tipe==='BIAYA'?'text-rose-600':'text-emerald-600'}">Rp ${x.total.toLocaleString()}</p><p class="text-[10px] text-slate-400 font-bold">${new Date(x.ts).toLocaleDateString()}</p></div>
                    </div>
                `);
            });
            renderCharts();
        }

        let chartBeli;
        function renderCharts() {
            const ctx = document.getElementById('chartBeli');
            const last7Days = [...Array(7)].map((_, i) => {
                const d = new Date(); d.setDate(d.getDate() - i);
                return d.toISOString().split('T')[0];
            }).reverse();
            const data = last7Days.map(day => db.tx.filter(x => x.tipe === 'BELI' && new Date(x.ts).toISOString().split('T')[0] === day).reduce((a, b) => a + b.netto, 0));
            if(chartBeli) chartBeli.destroy();
            chartBeli = new Chart(ctx, {
                type: 'bar',
                data: { labels: ['H-6','H-5','H-4','H-3','H-2','H-1','Hari Ini'], datasets: [{ label: 'Kg', data: data, backgroundColor: '#10b981', borderRadius: 10 }] },
                options: { maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });
        }

        // --- NEW: EXCEL EXPORT FEATURE ---
        function exportFullReport() {
            const wb = XLSX.utils.book_new();
            
            // 1. Sheet Laba Rugi
            let plData = [
                ["LAPORAN LABA RUGI"],
                ["Periode:", new Date().toLocaleDateString()],
                [],
                ["Keterangan", "Nominal"],
                ["Pendapatan Klaim PKS", parseFloat(document.getElementById('pl-jual').innerText.replace(/[^0-9]/g, ""))],
                ["Beban Pembelian TBS", -parseFloat(document.getElementById('pl-beli').innerText.replace(/[^0-9]/g, ""))],
                ["LABA KOTOR", parseFloat(document.getElementById('pl-laba-kotor').innerText.replace(/[^0-9]/g, ""))],
                ["Total Biaya Operasional", -parseFloat(document.getElementById('pl-total-biaya').innerText.replace(/[^0-9]/g, ""))],
                ["LABA BERSIH", parseFloat(document.getElementById('pl-netto').innerText.replace(/[^0-9]/g, ""))]
            ];
            const wsPL = XLSX.utils.aoa_to_sheet(plData);
            XLSX.utils.book_append_sheet(wb, wsPL, "Laba Rugi");

            // 2. Sheet Neraca
            let blData = [
                ["LAPORAN NERACA"],
                ["AKTIVA", "", "PASIVA", ""],
                ["Kas & Bank", parseFloat(document.getElementById('bl-kas').innerText.replace(/[^0-9]/g, "")), "Modal Disetor", parseFloat(document.getElementById('bl-modal-disetor').innerText.replace(/[^0-9]/g, ""))],
                ["Stok RAM", parseFloat(document.getElementById('bl-stok').innerText.replace(/[^0-9]/g, "")), "Laba Berjalan", parseFloat(document.getElementById('bl-laba-berjalan').innerText.replace(/[^0-9]/g, ""))],
                ["OTW PKS", parseFloat(document.getElementById('bl-otw').innerText.replace(/[^0-9]/g, "")), "Prive", -parseFloat(document.getElementById('bl-prive').innerText.replace(/[^0-9]/g, ""))],
                ["Piutang PKS", parseFloat(document.getElementById('bl-piutang').innerText.replace(/[^0-9]/g, ""))],
                ["TOTAL AKTIVA", parseFloat(document.getElementById('bl-total-aktiva').innerText.replace(/[^0-9]/g, "")), "TOTAL PASIVA", parseFloat(document.getElementById('bl-total-pasiva').innerText.replace(/[^0-9]/g, ""))]
            ];
            const wsBL = XLSX.utils.aoa_to_sheet(blData);
            XLSX.utils.book_append_sheet(wb, wsBL, "Neraca");

            // 3. Sheet Perubahan Modal
            let cmData = [
                ["LAPORAN PERUBAHAN MODAL"],
                ["Keterangan", "Jumlah"],
                ["Modal Awal", parseFloat(document.getElementById('cm-awal').innerText.replace(/[^0-9]/g, ""))],
                ["Laba Bersih", parseFloat(document.getElementById('cm-laba').innerText.replace(/[^0-9]/g, ""))],
                ["Prive", -parseFloat(document.getElementById('cm-prive').innerText.replace(/[^0-9]/g, ""))],
                ["MODAL AKHIR", parseFloat(document.getElementById('cm-akhir').innerText.replace(/[^0-9]/g, ""))]
            ];
            const wsCM = XLSX.utils.aoa_to_sheet(cmData);
            XLSX.utils.book_append_sheet(wb, wsCM, "Modal");

            // 4. Sheet Jurnal Transaksi Detail
            const journal = db.tx.map(x => ({
                Tanggal: new Date(x.ts).toLocaleDateString(),
                Tipe: x.tipe,
                Keterangan: x.nama,
                Netto: x.netto || 0,
                Harga: x.h || 0,
                Total: x.total
            }));
            const wsJournal = XLSX.utils.json_to_sheet(journal);
            XLSX.utils.book_append_sheet(wb, wsJournal, "Detail Transaksi");

            XLSX.writeFile(wb, `PalmCore_Full_Report_${new Date().toISOString().split('T')[0]}.xlsx`);
        }

        window.onload = () => { setToday(); renderAll(); };
    </script>
</body>
</html>
