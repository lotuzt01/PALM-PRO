<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PalmCore ERP - Realtime Cloud</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, doc, deleteDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Konfigurasi Firebase (Otomatis terisi saat dijalankan)
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'palmcore-erp-prod';

        let currentUser = null;
        let dataStore = [];
        let mainChart = null;

        const initAuth = async () => {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (err) {
                console.error("Auth Error:", err);
            }
        };

        initAuth();

        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                document.getElementById('user-id-display').innerText = `UID: ${user.uid}`;
                setupRealtimeListener();
            }
        });

        function setupRealtimeListener() {
            if (!currentUser) return;
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'transaksi'));
            
            onSnapshot(q, (snapshot) => {
                dataStore = [];
                snapshot.forEach((doc) => {
                    dataStore.push({ id: doc.id, ...doc.data() });
                });
                dataStore.sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));
                renderAll();
            }, (error) => {
                console.error("Firestore Error:", error);
            });
        }

        window.simpanCloud = async (tipe) => {
            if (!currentUser) return alert("Koneksi terputus!");

            let payload = {
                tipe,
                createdAt: serverTimestamp(),
                userId: currentUser.uid,
                nama: '',
                netto: 0,
                total: 0
            };

            if (tipe === 'BELI') {
                const b = parseFloat(document.getElementById('b-bruto').value) || 0;
                const t = parseFloat(document.getElementById('b-tara').value) || 0;
                const p = parseFloat(document.getElementById('b-persen').value) || 0;
                const h = parseFloat(document.getElementById('b-harga').value) || 0;
                payload.netto = Math.round((b - t) * (1 - p/100));
                payload.total = payload.netto * h;
                payload.nama = document.getElementById('b-nama').value || 'Pemasok Umum';
            } else if (tipe === 'JUAL') {
                const n = parseFloat(document.getElementById('j-netto').value) || 0;
                const h = parseFloat(document.getElementById('j-harga').value) || 0;
                payload.netto = n;
                payload.total = n * h;
                payload.nama = document.getElementById('j-pabrik').value || 'PKS';
            } else if (tipe === 'BIAYA') {
                payload.total = parseFloat(document.getElementById('c-nominal').value) || 0;
                payload.nama = document.getElementById('c-ket').value || 'Operasional';
            } else if (tipe === 'MODAL') {
                payload.total = parseFloat(document.getElementById('m-nominal').value) || 0;
                payload.nama = document.getElementById('m-sumber').value || 'Modal';
            }

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transaksi'), payload);
                showToast(`Sinkronisasi ${tipe} Berhasil!`);
                resetInputs();
                navTo('dashboard');
            } catch (err) {
                alert("Gagal: " + err.message);
            }
        };

        window.hapusData = async (id) => {
            if (!confirm("Hapus data dari Cloud?")) return;
            await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'transaksi', id));
        };

        window.renderAll = () => {
            const start = document.getElementById('filter-start').value;
            const end = document.getElementById('filter-end').value;

            const filtered = dataStore.filter(d => {
                if (!d.createdAt) return true;
                const dDate = new Date(d.createdAt.seconds * 1000).toISOString().split('T')[0];
                if (start && dDate < start) return false;
                if (end && dDate > end) return false;
                return true;
            });

            let br = 0, jr = 0, exp = 0, mod = 0, kg = 0;
            const table = document.getElementById('table-log');
            table.innerHTML = '';

            filtered.forEach(d => {
                if(d.tipe==='BELI') { br += d.total; kg += d.netto; }
                if(d.tipe==='JUAL') { jr += d.total; kg -= d.netto; }
                if(d.tipe==='BIAYA') { exp += d.total; }
                if(d.tipe==='MODAL') { mod += d.total; }

                const tgl = d.createdAt ? new Date(d.createdAt.seconds * 1000).toLocaleDateString('id-ID') : 'Memproses...';

                table.insertAdjacentHTML('beforeend', `
                    <tr class="hover:bg-slate-50 border-b border-slate-100 font-bold text-slate-700">
                        <td class="p-4 text-[11px] text-slate-400">${tgl}</td>
                        <td class="p-4"><span class="px-3 py-1 rounded-lg text-[10px] font-black uppercase badge-${d.tipe.toLowerCase()}">${d.tipe}</span></td>
                        <td class="p-4 text-slate-900">${d.nama}</td>
                        <td class="p-4 text-right">${d.netto > 0 ? d.netto.toLocaleString() : '-'}</td>
                        <td class="p-4 text-right font-black text-slate-900">Rp ${d.total.toLocaleString()}</td>
                        <td class="p-4 text-center">
                            <button onclick="hapusData('${d.id}')" class="text-slate-300 hover:text-rose-600 transition-colors"><i class="fas fa-trash-alt"></i></button>
                        </td>
                    </tr>
                `);
            });

            document.getElementById('stok-val').innerText = kg.toLocaleString();
            document.getElementById('cash-val').innerText = 'Rp ' + (mod + jr - br - exp).toLocaleString();
            document.getElementById('modal-total-val').innerText = 'Rp ' + mod.toLocaleString();
            document.getElementById('profit-val').innerText = 'Rp ' + (jr - br - exp).toLocaleString();
            
            updateChart(filtered);
        };

        function updateChart(data) {
            const ctx = document.getElementById('chartView').getContext('2d');
            const recent = [...data].reverse().slice(-7);
            if(mainChart) mainChart.destroy();
            mainChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: recent.map(r => r.createdAt ? new Date(r.createdAt.seconds * 1000).toLocaleDateString('id-ID', {day:'2-digit', month:'short'}) : '?'),
                    datasets: [{
                        label: 'Nilai Transaksi',
                        data: recent.map(r => r.total),
                        borderColor: '#059669',
                        backgroundColor: 'rgba(5, 150, 105, 0.1)',
                        fill: true,
                        tension: 0.4
                    }]
                },
                options: { maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });
        }
    </script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; }
        .nav-btn { color: #64748b; font-weight: 700; border-radius: 14px; transition: all 0.2s; }
        .nav-btn.active { background: #047857 !important; color: white !important; box-shadow: 0 4px 12px rgba(4,120,87,0.2); }
        .glass-panel { background: white; border: 1px solid #e2e8f0; }
        .badge-beli { background: #dcfce7; color: #166534; }
        .badge-jual { background: #dbeafe; color: #1e40af; }
        .badge-biaya { background: #fee2e2; color: #991b1b; }
        .badge-modal { background: #fef3c7; color: #92400e; }
    </style>
</head>
<body class="text-slate-800">

    <nav class="fixed left-0 top-0 h-full w-20 lg:w-64 bg-white border-r border-slate-200 z-50">
        <div class="p-4 lg:p-6 flex flex-col h-full">
            <div class="mb-10 flex items-center gap-3 justify-center lg:justify-start">
                <div class="bg-emerald-600 p-2.5 rounded-xl text-white shadow-lg shadow-emerald-200"><i class="fas fa-leaf text-xl"></i></div>
                <span class="font-extrabold text-xl hidden lg:block tracking-tighter">PalmCore <span class="text-emerald-600">ERP</span></span>
            </div>
            
            <div class="space-y-2 flex-1">
                <button onclick="navTo('dashboard')" id="nav-dashboard" class="nav-btn active w-full flex items-center gap-4 p-4"><i class="fas fa-th-large"></i><span class="hidden lg:block">Dashboard</span></button>
                <button onclick="navTo('modal')" id="nav-modal" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-piggy-bank"></i><span class="hidden lg:block">Modal</span></button>
                <button onclick="navTo('beli')" id="nav-beli" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-shopping-cart"></i><span class="hidden lg:block">Beli TBS</span></button>
                <button onclick="navTo('jual')" id="nav-jual" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-truck-loading"></i><span class="hidden lg:block">Jual PKS</span></button>
                <button onclick="navTo('biaya')" id="nav-biaya" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-wallet"></i><span class="hidden lg:block">Biaya Ops</span></button>
            </div>

            <div class="mt-auto p-4 bg-slate-50 rounded-2xl hidden lg:block text-center">
                <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Status Cloud</p>
                <p id="user-id-display" class="text-[9px] font-mono text-emerald-600 truncate">Menghubungkan...</p>
            </div>
        </div>
    </nav>

    <main class="ml-20 lg:ml-64 p-4 lg:p-8 min-h-screen">
        <div id="page-dashboard" class="page-content">
            <div class="flex flex-wrap items-center gap-4 mb-8 glass-panel p-4 rounded-2xl border-l-4 border-emerald-500 shadow-sm">
                <div class="flex items-center gap-2"><i class="fas fa-calendar-alt text-emerald-600"></i><span class="text-xs font-bold text-slate-400">FILTER TANGGAL</span></div>
                <input type="date" id="filter-start" onchange="renderAll()" class="bg-slate-50 p-2 rounded-lg border border-slate-200 font-bold text-sm outline-none focus:ring-2 ring-emerald-500">
                <span class="font-bold text-slate-300">s/d</span>
                <input type="date" id="filter-end" onchange="renderAll()" class="bg-slate-50 p-2 rounded-lg border border-slate-200 font-bold text-sm outline-none focus:ring-2 ring-emerald-500">
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-emerald-500 shadow-sm">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Stok TBS (Kg)</p>
                    <h2 id="stok-val" class="text-3xl font-black text-slate-900">0</h2>
                </div>
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-blue-600 shadow-sm">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Kas / Tunai</p>
                    <h2 id="cash-val" class="text-2xl font-black text-blue-700">Rp 0</h2>
                </div>
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-amber-500 shadow-sm">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Total Modal</p>
                    <h2 id="modal-total-val" class="text-2xl font-black text-amber-700">Rp 0</h2>
                </div>
                <div class="bg-slate-900 p-6 rounded-3xl text-white shadow-xl">
                    <p class="text-[10px] font-bold text-emerald-400 uppercase tracking-widest mb-1">Laba Bersih</p>
                    <h2 id="profit-val" class="text-2xl font-black text-emerald-400">Rp 0</h2>
                </div>
            </div>

            <div class="glass-panel p-6 rounded-3xl h-[400px] mb-8 shadow-sm">
                <canvas id="chartView"></canvas>
            </div>

            <div class="glass-panel rounded-3xl overflow-hidden shadow-sm">
                <table class="w-full text-left text-sm">
                    <thead class="bg-slate-50 border-b border-slate-100">
                        <tr class="text-[10px] font-black text-slate-400 uppercase">
                            <th class="p-4">Tanggal</th>
                            <th class="p-4">Tipe</th>
                            <th class="p-4">Keterangan</th>
                            <th class="p-4 text-right">Kg</th>
                            <th class="p-4 text-right">Nominal</th>
                            <th class="p-4 text-center">Opsi</th>
                        </tr>
                    </thead>
                    <tbody id="table-log"></tbody>
                </table>
            </div>
        </div>

        <!-- Forms -->
        <div id="page-beli" class="page-content hidden max-w-2xl mx-auto glass-panel p-10 rounded-[2.5rem] shadow-xl border-t-8 border-emerald-600">
            <h3 class="text-3xl font-black text-emerald-900 mb-8">Pembelian TBS</h3>
            <div class="space-y-5">
                <input id="b-nama" type="text" placeholder="Nama Petani" class="w-full p-4 bg-slate-50 border-2 rounded-2xl font-bold focus:border-emerald-500 outline-none">
                <div class="grid grid-cols-2 gap-4">
                    <input id="b-bruto" type="number" placeholder="Bruto (Kg)" class="w-full p-4 border-2 rounded-2xl font-bold focus:border-emerald-500 outline-none">
                    <input id="b-tara" type="number" placeholder="Tara (Kg)" class="w-full p-4 border-2 rounded-2xl font-bold focus:border-emerald-500 outline-none">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <input id="b-persen" type="number" value="3" class="w-full p-4 border-2 rounded-2xl font-bold">
                    <input id="b-harga" type="number" placeholder="Harga Beli /Kg" class="w-full p-4 bg-emerald-50 border-emerald-200 border-2 rounded-2xl font-black text-emerald-800">
                </div>
                <button onclick="simpanCloud('BELI')" class="w-full py-5 bg-emerald-600 text-white font-black rounded-2xl text-xl shadow-lg hover:bg-emerald-700 transition-all">SIMPAN TRANSAKSI</button>
            </div>
        </div>

        <div id="page-jual" class="page-content hidden max-w-2xl mx-auto glass-panel p-10 rounded-[2.5rem] shadow-xl border-t-8 border-blue-600">
            <h3 class="text-3xl font-black text-blue-900 mb-8">Penjualan PKS</h3>
            <div class="space-y-5">
                <input id="j-pabrik" type="text" placeholder="Nama Pabrik" class="w-full p-4 bg-slate-50 border-2 rounded-2xl font-bold focus:border-blue-500 outline-none">
                <input id="j-netto" type="number" placeholder="Netto Pabrik (Kg)" class="w-full p-4 border-2 rounded-2xl font-bold focus:border-blue-500 outline-none">
                <input id="j-harga" type="number" placeholder="Harga Jual /Kg" class="w-full p-4 bg-blue-50 border-blue-200 border-2 rounded-2xl font-black text-blue-800 focus:border-blue-500 outline-none">
                <button onclick="simpanCloud('JUAL')" class="w-full py-5 bg-blue-600 text-white font-black rounded-2xl text-xl shadow-lg hover:bg-blue-700 transition-all">POSTING PENJUALAN</button>
            </div>
        </div>

        <div id="page-modal" class="page-content hidden max-w-xl mx-auto glass-panel p-10 rounded-[2.5rem] shadow-xl border-t-8 border-amber-500">
            <h3 class="text-3xl font-black text-amber-900 mb-8">Input Modal</h3>
            <div class="space-y-5">
                <input id="m-sumber" type="text" placeholder="Keterangan Modal" class="w-full p-4 border-2 rounded-2xl font-bold outline-none focus:border-amber-500">
                <input id="m-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 bg-amber-50 border-amber-200 border-2 rounded-2xl font-black text-amber-800">
                <button onclick="simpanCloud('MODAL')" class="w-full py-5 bg-amber-600 text-white font-black rounded-2xl text-xl shadow-lg">TAMBAH KAS</button>
            </div>
        </div>

        <div id="page-biaya" class="page-content hidden max-w-xl mx-auto glass-panel p-10 rounded-[2.5rem] shadow-xl border-t-8 border-rose-600">
            <h3 class="text-3xl font-black text-rose-900 mb-8">Biaya Ops</h3>
            <div class="space-y-5">
                <input id="c-ket" type="text" placeholder="Jenis Biaya" class="w-full p-4 border-2 rounded-2xl font-bold focus:border-rose-500 outline-none">
                <input id="c-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 bg-rose-50 border-rose-200 border-2 rounded-2xl font-black text-rose-800">
                <button onclick="simpanCloud('BIAYA')" class="w-full py-5 bg-rose-600 text-white font-black rounded-2xl text-xl shadow-lg">CATAT PENGELUARAN</button>
            </div>
        </div>
    </main>

    <div id="toast-container" class="fixed top-5 right-5 z-[9999] flex flex-col gap-2"></div>

    <script>
        window.navTo = (id) => {
            document.querySelectorAll('.page-content').forEach(p => p.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('page-' + id).classList.remove('hidden');
            document.getElementById('nav-' + id).classList.add('active');
        };

        window.showToast = (msg) => {
            const t = document.createElement('div');
            t.className = "bg-slate-900 text-white px-6 py-4 rounded-2xl shadow-2xl font-bold animate-slide-in";
            t.innerHTML = `<i class="fas fa-check-circle text-emerald-400 mr-2"></i> ${msg}`;
            document.getElementById('toast-container').appendChild(t);
            setTimeout(() => { t.style.opacity = '0'; setTimeout(() => t.remove(), 500); }, 3000);
        };

        function resetInputs() {
            document.querySelectorAll('input:not([type="date"])').forEach(i => { if(i.id !== 'b-persen') i.value = ''; });
        }
    </script>
</body>
</html>

