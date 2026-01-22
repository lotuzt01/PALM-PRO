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
    
    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, doc, deleteDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Konfigurasi dari Environment
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'palmcore-erp';

        // State Global
        let currentUser = null;
        let dataStore = [];
        let mainChart = null;

        // 1. Inisialisasi Auth
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
                document.getElementById('user-id-display').innerText = `ID: ${user.uid}`;
                setupRealtimeListener();
            }
        });

        // 2. Listener Realtime Firestore
        function setupRealtimeListener() {
            if (!currentUser) return;
            
            // Menggunakan path sesuai Rule 1: /artifacts/{appId}/public/data/{collectionName}
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'transaksi'));
            
            onSnapshot(q, (snapshot) => {
                dataStore = [];
                snapshot.forEach((doc) => {
                    dataStore.push({ id: doc.id, ...doc.data() });
                });
                
                // Urutkan berdasarkan timestamp di memori (Rule 2)
                dataStore.sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));
                
                renderAll();
            }, (error) => {
                console.error("Firestore Error:", error);
            });
        }

        // 3. Fungsi Simpan ke Cloud
        window.simpanCloud = async (tipe) => {
            if (!currentUser) return alert("Belum terhubung ke server!");

            let payload = {
                tipe,
                createdAt: serverTimestamp(),
                userId: currentUser.uid
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
                payload.nama = document.getElementById('c-ket').value || 'Biaya Ops';
                payload.netto = 0;
            } else if (tipe === 'MODAL') {
                payload.total = parseFloat(document.getElementById('m-nominal').value) || 0;
                payload.nama = document.getElementById('m-sumber').value || 'Investasi';
                payload.netto = 0;
            }

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transaksi'), payload);
                showToast(`Data ${tipe} berhasil disinkronkan!`);
                resetInputs();
                navTo('dashboard');
            } catch (err) {
                alert("Gagal menyimpan: " + err.message);
            }
        };

        window.hapusData = async (id) => {
            if (!confirm("Hapus data ini dari cloud?")) return;
            await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'transaksi', id));
        };

        // UI Logic tetap sama namun mengambil dari dataStore realtime
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

                const tgl = d.createdAt ? new Date(d.createdAt.seconds * 1000).toLocaleDateString('id-ID') : 'Pending...';

                table.insertAdjacentHTML('beforeend', `
                    <tr class="hover:bg-slate-50 border-b border-slate-100 font-bold text-slate-700">
                        <td class="p-4 text-xs font-bold text-slate-400">${tgl}</td>
                        <td class="p-4"><span class="px-3 py-1 rounded-lg text-[10px] font-black uppercase badge-${d.tipe.toLowerCase()}">${d.tipe}</span></td>
                        <td class="p-4 text-slate-900">${d.nama}</td>
                        <td class="p-4 text-right">${d.netto > 0 ? d.netto.toLocaleString() : '-'}</td>
                        <td class="p-4 text-right font-black text-slate-900">Rp ${d.total.toLocaleString()}</td>
                        <td class="p-4 text-center">
                            <button onclick="hapusData('${d.id}')" class="text-slate-300 hover:text-rose-600"><i class="fas fa-trash-alt"></i></button>
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
                        label: 'Arus Kas (Rp)',
                        data: recent.map(r => r.total),
                        borderColor: '#059669',
                        backgroundColor: 'rgba(5, 150, 105, 0.1)',
                        fill: true,
                        tension: 0.4
                    }]
                },
                options: { maintainAspectRatio: false }
            });
        }
    </script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; }
        .nav-btn { color: #64748b; font-weight: 700; border-radius: 12px; }
        .nav-btn.active { background: #047857 !important; color: white !important; }
        .glass-panel { background: white; border: 1px solid #e2e8f0; box-shadow: 0 1px 3px rgba(0,0,0,0.05); }
        .badge-beli { background: #dcfce7; color: #166534; }
        .badge-jual { background: #dbeafe; color: #1e40af; }
        .badge-biaya { background: #fee2e2; color: #991b1b; }
        .badge-modal { background: #fef3c7; color: #92400e; }
    </style>
</head>
<body class="text-slate-800">

    <nav class="fixed left-0 top-0 h-full w-20 lg:w-64 bg-white border-r border-slate-200 z-50">
        <div class="p-4 lg:p-6 flex flex-col h-full">
            <div class="mb-10 flex items-center gap-3">
                <div class="bg-emerald-600 p-2 rounded-xl text-white"><i class="fas fa-sync-alt fa-spin"></i></div>
                <span class="font-black text-lg hidden lg:block uppercase tracking-tighter">PalmCore <span class="text-emerald-600">Cloud</span></span>
            </div>
            
            <div class="space-y-2 flex-1">
                <button onclick="navTo('dashboard')" id="nav-dashboard" class="nav-btn active w-full flex items-center gap-4 p-4"><i class="fas fa-chart-pie"></i><span class="hidden lg:block">Dashboard</span></button>
                <button onclick="navTo('modal')" id="nav-modal" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-vault"></i><span class="hidden lg:block">Modal</span></button>
                <button onclick="navTo('beli')" id="nav-beli" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-shopping-basket"></i><span class="hidden lg:block">Beli TBS</span></button>
                <button onclick="navTo('jual')" id="nav-jual" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-truck-loading"></i><span class="hidden lg:block">Jual PKS</span></button>
                <button onclick="navTo('biaya')" id="nav-biaya" class="nav-btn w-full flex items-center gap-4 p-4"><i class="fas fa-receipt"></i><span class="hidden lg:block">Biaya Ops</span></button>
            </div>

            <div class="mt-auto p-4 bg-slate-50 rounded-2xl hidden lg:block">
                <p class="text-[10px] font-bold text-slate-400">STATUS CLOUD</p>
                <p id="user-id-display" class="text-[9px] font-mono text-emerald-600 truncate">Menghubungkan...</p>
            </div>
        </div>
    </nav>

    <main class="ml-20 lg:ml-64 p-4 lg:p-8">
        <!-- Dashboard Content -->
        <div id="page-dashboard" class="page-content">
            <div class="flex flex-wrap items-center gap-4 mb-8 glass-panel p-4 rounded-2xl border-l-4 border-emerald-500">
                <input type="date" id="filter-start" onchange="renderAll()" class="bg-slate-50 p-2 rounded-lg border font-bold text-sm">
                <span class="font-bold text-slate-400">sampai</span>
                <input type="date" id="filter-end" onchange="renderAll()" class="bg-slate-50 p-2 rounded-lg border font-bold text-sm">
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-emerald-500">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Stok TBS (Kg)</p>
                    <h2 id="stok-val" class="text-3xl font-black">0</h2>
                </div>
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-blue-600">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Saldo Kas</p>
                    <h2 id="cash-val" class="text-2xl font-black text-blue-700">Rp 0</h2>
                </div>
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-amber-500">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Total Modal</p>
                    <h2 id="modal-total-val" class="text-2xl font-black text-amber-700">Rp 0</h2>
                </div>
                <div class="bg-slate-900 p-6 rounded-3xl text-white">
                    <p class="text-[10px] font-bold text-emerald-400 uppercase tracking-widest mb-1">Estimasi Laba</p>
                    <h2 id="profit-val" class="text-2xl font-black text-emerald-400">Rp 0</h2>
                </div>
            </div>

            <div class="glass-panel p-6 rounded-3xl h-[400px]">
                <canvas id="chartView"></canvas>
            </div>

            <div class="mt-8 glass-panel rounded-3xl overflow-hidden">
                <table class="w-full text-left">
                    <thead class="bg-slate-50 border-b">
                        <tr class="text-[10px] font-black text-slate-400 uppercase">
                            <th class="p-4">Waktu</th>
                            <th class="p-4">Kategori</th>
                            <th class="p-4">Keterangan</th>
                            <th class="p-4 text-right">Kg</th>
                            <th class="p-4 text-right">Nilai</th>
                            <th class="p-4 text-center">Aksi</th>
                        </tr>
                    </thead>
                    <tbody id="table-log"></tbody>
                </table>
            </div>
        </div>

        <!-- Forms (Simplified View for brevity, using simpanCloud) -->
        <div id="page-beli" class="page-content hidden max-w-2xl mx-auto glass-panel p-8 rounded-[2rem]">
            <h3 class="text-2xl font-black mb-6">Input Beli TBS</h3>
            <div class="space-y-4">
                <input id="b-nama" type="text" placeholder="Nama Petani" class="w-full p-4 bg-slate-50 border rounded-xl font-bold">
                <div class="grid grid-cols-2 gap-4">
                    <input id="b-bruto" type="number" placeholder="Bruto" class="w-full p-4 border rounded-xl font-bold">
                    <input id="b-tara" type="number" placeholder="Tara" class="w-full p-4 border rounded-xl font-bold">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <input id="b-persen" type="number" value="3" class="w-full p-4 border rounded-xl font-bold">
                    <input id="b-harga" type="number" placeholder="Harga /Kg" class="w-full p-4 bg-emerald-50 border-emerald-200 border rounded-xl font-bold">
                </div>
                <button onclick="simpanCloud('BELI')" class="w-full py-5 bg-emerald-600 text-white font-black rounded-xl text-xl shadow-lg">POSTING KE CLOUD</button>
            </div>
        </div>

        <div id="page-jual" class="page-content hidden max-w-2xl mx-auto glass-panel p-8 rounded-[2rem]">
            <h3 class="text-2xl font-black mb-6">Input Jual PKS</h3>
            <div class="space-y-4">
                <input id="j-pabrik" type="text" placeholder="Nama Pabrik" class="w-full p-4 bg-slate-50 border rounded-xl font-bold">
                <input id="j-netto" type="number" placeholder="Netto Pabrik" class="w-full p-4 border rounded-xl font-bold">
                <input id="j-harga" type="number" placeholder="Harga Jual" class="w-full p-4 bg-blue-50 border rounded-xl font-bold">
                <button onclick="simpanCloud('JUAL')" class="w-full py-5 bg-blue-600 text-white font-black rounded-xl text-xl shadow-lg">POSTING KE CLOUD</button>
            </div>
        </div>

        <div id="page-modal" class="page-content hidden max-w-xl mx-auto glass-panel p-8 rounded-[2rem]">
            <h3 class="text-2xl font-black mb-6">Tambah Modal Tunai</h3>
            <div class="space-y-4">
                <input id="m-sumber" type="text" placeholder="Sumber Modal" class="w-full p-4 border rounded-xl font-bold">
                <input id="m-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 bg-amber-50 border rounded-xl font-bold">
                <button onclick="simpanCloud('MODAL')" class="w-full py-5 bg-amber-600 text-white font-black rounded-xl text-xl shadow-lg">DAPATKAN MODAL</button>
            </div>
        </div>

        <div id="page-biaya" class="page-content hidden max-w-xl mx-auto glass-panel p-8 rounded-[2rem]">
            <h3 class="text-2xl font-black mb-6">Input Biaya Operasional</h3>
            <div class="space-y-4">
                <input id="c-ket" type="text" placeholder="Keterangan Biaya" class="w-full p-4 border rounded-xl font-bold">
                <input id="c-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 bg-rose-50 border rounded-xl font-bold">
                <button onclick="simpanCloud('BIAYA')" class="w-full py-5 bg-rose-600 text-white font-black rounded-xl text-xl shadow-lg">CATAT BIAYA</button>
            </div>
        </div>
    </main>

    <div id="toast-container" class="fixed top-5 right-5 z-[9999]"></div>

    <script>
        // UI Helpers
        window.navTo = (id) => {
            document.querySelectorAll('.page-content').forEach(p => p.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('page-' + id).classList.remove('hidden');
            document.getElementById('nav-' + id).classList.add('active');
        };

        window.showToast = (msg) => {
            const t = document.createElement('div');
            t.className = "bg-slate-800 text-white px-6 py-4 rounded-xl shadow-2xl mb-2 animate-bounce font-bold";
            t.innerText = msg;
            document.getElementById('toast-container').appendChild(t);
            setTimeout(() => t.remove(), 3000);
        };

        function resetInputs() {
            document.querySelectorAll('input:not([type="date"])').forEach(i => { if(i.id !== 'b-persen') i.value = ''; });
        }
    </script>
</body>
</html>

