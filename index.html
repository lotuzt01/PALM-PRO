import React, { useState, useEffect, useMemo } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getFirestore, collection, doc, setDoc, getDoc, 
  onSnapshot, query, addDoc, updateDoc, deleteDoc 
} from 'firebase/firestore';
import { 
  getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged 
} from 'firebase/auth';
import { 
  LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, AreaChart, Area 
} from 'recharts';
import { 
  LayoutDashboard, Database, Wallet, PlusCircle, FileSpreadsheet, 
  LogOut, Truck, TrendingUp, Menu, X, Filter, Calendar, Palmtree,
  TrendingDown, Info, Trash2, AlertTriangle
} from 'lucide-react';

// Firebase Configuration (Provided by environment)
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'adhi-group-erp-v1';

const App = () => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);
  const [user, setUser] = useState(null);
  const [transactions, setTransactions] = useState([]);
  const [loading, setLoading] = useState(true);
  
  // Modal State untuk Hapus
  const [deleteModal, setDeleteModal] = useState({ isOpen: false, id: null, entity: '' });

  // Filters
  const [filterDate, setFilterDate] = useState('');
  const [filterType, setFilterType] = useState('ALL');

  const [form, setForm] = useState({
    type: 'IN',
    flow: '+', 
    entity: '',
    driver: '',
    plate: '',
    bruto: '',
    tara: '',
    pot: '3.0',
    price: '',
    note: '',
    category: 'TBS'
  });

  // --- AUTHENTICATION ---
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (error) {
        console.error("Auth Error:", error);
      }
    };
    initAuth();

    const unsubscribe = onAuthStateChanged(auth, (u) => {
      setUser(u);
    });
    return () => unsubscribe();
  }, []);

  // --- DATA FETCHING ---
  useEffect(() => {
    if (!user) return;

    setLoading(true);
    const q = collection(db, 'artifacts', appId, 'public', 'data', 'transactions');
    
    const unsubscribe = onSnapshot(q, (snapshot) => {
      const data = snapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
      const sortedData = data.sort((a, b) => b.timestamp - a.timestamp);
      setTransactions(sortedData);
      setLoading(false);
    }, (error) => {
      console.error("Firestore Error:", error);
      setLoading(false);
    });

    return () => unsubscribe();
  }, [user]);

  // --- CALCULATION LOGIC ---
  const stats = useMemo(() => {
    let totalIn = 0, totalOut = 0, totalCost = 0, kgIn = 0, kgOut = 0, kgLoss = 0, rupiahLoss = 0;
    let totalDebt = 0, totalCapital = 0;

    transactions.forEach(t => {
      const val = parseFloat(t.total) || 0;
      const kg = parseFloat(t.netto) || 0;
      
      if (t.type === 'IN') { totalIn += val; kgIn += kg; }
      else if (t.type === 'OUT') { totalOut += val; kgOut += kg; }
      else if (t.type === 'COST') { totalCost += val; }
      else if (t.type === 'LOSSES') { 
        kgLoss += kg; 
        rupiahLoss += val; 
      }
      else if (t.type === 'DEBT') {
        totalDebt += (t.flow === '+' ? val : -val);
      }
      else if (t.type === 'CAPITAL') {
        totalCapital += (t.flow === '+' ? val : -val);
      }
    });

    const cashBalance = (totalCapital + totalDebt + totalOut) - (totalIn + totalCost);
    const profit = totalOut - totalIn - totalCost - rupiahLoss;
    
    return { 
      totalIn, totalOut, totalCost, totalDebt, totalCapital,
      profit, 
      stock: kgIn - kgOut - kgLoss,
      kgLoss,
      rupiahLoss,
      cash: cashBalance
    };
  }, [transactions]);

  const filteredTransactions = useMemo(() => {
    return transactions.filter(t => {
      const matchDate = filterDate ? t.dateIso === filterDate : true;
      const matchType = filterType === 'ALL' ? true : t.type === filterType;
      return matchDate && matchType;
    });
  }, [transactions, filterDate, filterType]);

  // --- ACTIONS ---
  const handleSave = async (e) => {
    e.preventDefault();
    if (!user) return;

    const brutoVal = parseFloat(form.bruto) || 0;
    const taraVal = parseFloat(form.tara) || 0;
    const potVal = parseFloat(form.pot) || 0;
    const priceVal = parseFloat(form.price) || 0;
    
    let netto = 0;
    let total = 0;

    if (form.type === 'COST' || form.type === 'DEBT' || form.type === 'CAPITAL') {
      total = priceVal;
      netto = 0;
    } else if (form.type === 'LOSSES') {
      netto = brutoVal; 
      total = netto * priceVal; 
    } else {
      netto = form.type === 'IN' 
        ? (brutoVal - taraVal) * (1 - (potVal / 100)) 
        : brutoVal;
      total = netto * priceVal;
    }

    const now = new Date();
    const newTransaction = { 
      ...form, 
      netto: parseFloat(netto.toFixed(2)), 
      total: Math.round(total),
      timestamp: now.getTime(),
      dateDisplay: now.toLocaleDateString('id-ID'),
      dateIso: now.toISOString().split('T')[0],
      createdBy: user.uid
    };
    
    try {
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), newTransaction);
      setForm({ ...form, entity: '', driver: '', plate: '', bruto: '', tara: '', price: '', note: '' });
      setActiveTab('database');
    } catch (err) {
      console.error("Save error:", err);
    }
  };

  const confirmDelete = (t) => {
    setDeleteModal({ isOpen: true, id: t.id, entity: t.entity });
  };

  const handleDelete = async () => {
    if (!user || !deleteModal.id) return;
    try {
      await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'transactions', deleteModal.id));
      setDeleteModal({ isOpen: false, id: null, entity: '' });
    } catch (err) {
      console.error("Delete error:", err);
    }
  };

  const NavItem = ({ id, icon: Icon, label }) => (
    <button
      onClick={() => { setActiveTab(id); setIsSidebarOpen(false); }}
      className={`flex items-center gap-3 w-full px-4 py-4 rounded-xl transition-all ${
        activeTab === id 
        ? 'bg-emerald-500 text-white shadow-lg shadow-emerald-500/30' 
        : 'text-slate-400 hover:bg-slate-800 hover:text-white'
      }`}
    >
      <Icon size={20} />
      <span className="font-bold text-xs tracking-widest">{label}</span>
    </button>
  );

  return (
    <div className="flex min-h-screen bg-[#0f172a] text-slate-200 font-sans">
      
      {/* MODAL HAPUS (CUSTOM UI) */}
      {deleteModal.isOpen && (
        <div className="fixed inset-0 bg-black/80 backdrop-blur-sm z-[100] flex items-center justify-center p-6">
          <div className="bg-[#1e293b] border border-slate-700 w-full max-w-sm rounded-3xl p-8 shadow-2xl animate-in zoom-in duration-200">
            <div className="w-16 h-16 bg-rose-500/10 text-rose-500 rounded-full flex items-center justify-center mx-auto mb-6">
              <AlertTriangle size={32} />
            </div>
            <h3 className="text-center text-xl font-black text-white mb-2">HAPUS DATA?</h3>
            <p className="text-center text-slate-400 text-sm leading-relaxed mb-8">
              Transaksi <span className="text-white font-bold italic">"{deleteModal.entity}"</span> akan dihapus permanen dari cloud server.
            </p>
            <div className="flex gap-3">
              <button 
                onClick={() => setDeleteModal({ isOpen: false, id: null, entity: '' })}
                className="flex-1 py-4 bg-slate-800 text-slate-300 font-bold rounded-2xl hover:bg-slate-700 transition-colors"
              >
                BATAL
              </button>
              <button 
                onClick={handleDelete}
                className="flex-1 py-4 bg-rose-600 text-white font-bold rounded-2xl hover:bg-rose-500 transition-colors shadow-lg shadow-rose-600/20"
              >
                YA, HAPUS
              </button>
            </div>
          </div>
        </div>
      )}

      {/* Mobile Top Bar */}
      <div className="lg:hidden fixed top-0 left-0 right-0 h-16 bg-[#1e293b] border-b border-slate-800 z-50 flex items-center justify-between px-6">
        <div className="flex items-center gap-2">
           <Palmtree className="text-emerald-500" size={24} />
           <span className="font-black text-white italic tracking-tighter uppercase text-sm">ADHI GROUP</span>
        </div>
        <button onClick={() => setIsSidebarOpen(!isSidebarOpen)} className="p-2 bg-slate-800 rounded-lg text-emerald-500">
          {isSidebarOpen ? <X size={24}/> : <Menu size={24}/>}
        </button>
      </div>

      {/* Sidebar */}
      <aside className={`fixed lg:sticky top-0 left-0 h-screen w-72 bg-[#1e293b] border-r border-slate-800 p-6 flex flex-col z-[60] transition-transform duration-300 lg:translate-x-0 ${isSidebarOpen ? 'translate-x-0' : '-translate-x-full'}`}>
        <div className="hidden lg:flex items-center gap-3 mb-12 px-2">
          <div className="bg-emerald-500 p-2 rounded-lg"><Palmtree className="text-white" size={28} /></div>
          <div>
            <h1 className="text-xl font-black text-white tracking-tighter leading-none italic uppercase">ADHI GROUP</h1>
            <p className="text-[8px] font-bold text-emerald-500 tracking-[0.3em] mt-1">SAWINDO PRATAMA</p>
          </div>
        </div>

        <nav className="space-y-2 flex-1 mt-12 lg:mt-0">
          <NavItem id="dashboard" icon={LayoutDashboard} label="DASHBOARD" />
          <NavItem id="input" icon={PlusCircle} label="INPUT DATA" />
          <NavItem id="database" icon={Database} label="LEDGER" />
          <NavItem id="financial" icon={Wallet} label="FINANCIAL" />
        </nav>

        <div className="mt-auto pt-6 border-t border-slate-800/50">
          <div className="flex items-center gap-3 mb-4 px-2 text-xs">
            <div className="w-8 h-8 rounded-full bg-slate-700 flex items-center justify-center font-bold text-emerald-400">
              {user ? 'AG' : '...'}
            </div>
            <div className="overflow-hidden">
              <p className="font-black text-slate-500 uppercase truncate">Operator</p>
              <p className="text-[10px] text-slate-400 truncate font-mono">{user?.uid || 'Connecting...'}</p>
            </div>
          </div>
        </div>
      </aside>

      {/* Main Content */}
      <main className="flex-1 p-4 lg:p-10 mt-16 lg:mt-0 overflow-x-hidden">
        
        <div className="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4">
          <div>
            <h2 className="text-2xl lg:text-3xl font-bold text-white uppercase tracking-tight">
              {activeTab === 'financial' ? 'LAPORAN KEUANGAN' : 
               activeTab === 'database' ? 'JURNAL TRANSAKSI' : 
               activeTab === 'input' ? 'ENTRY BARU' : 'DASHBOARD'}
            </h2>
            <p className="text-emerald-500 text-[10px] font-black uppercase tracking-[.2em] mt-1">Sistem ERP Sawit Terintegrasi</p>
          </div>
        </div>

        {loading ? (
          <div className="h-[60vh] flex flex-col items-center justify-center gap-4">
            <div className="w-12 h-12 border-4 border-emerald-500 border-t-transparent rounded-full animate-spin"></div>
            <p className="text-slate-500 font-bold text-[10px] tracking-widest animate-pulse">SINKRONISASI CLOUD...</p>
          </div>
        ) : (
          <>
            {activeTab === 'dashboard' && (
              <div className="space-y-6">
                <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
                  {[
                    { label: 'CASH ON HAND', val: stats.cash, color: 'emerald', icon: Wallet },
                    { label: 'RUGI SUSUT (IDR)', val: stats.rupiahLoss, color: 'rose', icon: TrendingDown },
                    { label: 'STOK BERSIH (KG)', val: stats.stock, color: 'blue', icon: Truck },
                    { label: 'LABA BERJALAN', val: stats.profit, color: 'emerald', icon: TrendingUp },
                  ].map((item, i) => (
                    <div key={i} className="bg-[#1e293b] p-5 rounded-3xl border border-slate-800 shadow-xl">
                      <div className={`p-2 bg-${item.color}-500/10 text-${item.color}-500 rounded-lg w-fit mb-4`}>
                        <item.icon size={18} />
                      </div>
                      <h4 className="text-slate-500 text-[9px] font-black tracking-widest uppercase mb-1">{item.label}</h4>
                      <p className="text-lg lg:text-2xl font-black text-white tracking-tight leading-none">
                        {item.label.includes('(KG)') ? item.val.toLocaleString() : `Rp ${item.val.toLocaleString()}`}
                      </p>
                    </div>
                  ))}
                </div>

                <div className="bg-[#1e293b] p-6 lg:p-8 rounded-[2.5rem] border border-slate-800">
                  <h3 className="font-black text-white uppercase tracking-widest text-[10px] opacity-70 mb-8">Visualisasi Arus Barang (Kg)</h3>
                  <div className="h-64 lg:h-96 w-full">
                    <ResponsiveContainer width="100%" height="100%">
                      <AreaChart data={transactions.slice(0, 10).reverse()}>
                        <defs>
                          <linearGradient id="colorVal" x1="0" y1="0" x2="0" y2="1">
                            <stop offset="5%" stopColor="#10b981" stopOpacity={0.3}/>
                            <stop offset="95%" stopColor="#10b981" stopOpacity={0}/>
                          </linearGradient>
                        </defs>
                        <CartesianGrid strokeDasharray="3 3" stroke="#334155" vertical={false} opacity={0.3} />
                        <XAxis dataKey="dateDisplay" stroke="#64748b" fontSize={10} axisLine={false} tickLine={false} dy={10} />
                        <YAxis stroke="#64748b" fontSize={10} axisLine={false} tickLine={false} />
                        <Tooltip contentStyle={{ backgroundColor: '#1e293b', border: 'none', borderRadius: '16px', fontSize: '12px', boxShadow: '0 20px 25px -5px rgb(0 0 0 / 0.1)' }} />
                        <Area type="monotone" dataKey="netto" stroke="#10b981" fill="url(#colorVal)" strokeWidth={4} />
                      </AreaChart>
                    </ResponsiveContainer>
                  </div>
                </div>
              </div>
            )}

            {activeTab === 'input' && (
              <div className="max-w-4xl mx-auto">
                <form onSubmit={handleSave} className="bg-[#1e293b] p-8 lg:p-12 rounded-[3rem] border border-slate-800 shadow-2xl space-y-10">
                  <div className="grid grid-cols-3 lg:grid-cols-6 gap-3">
                    {[
                      { id: 'IN', label: 'BELI' },
                      { id: 'OUT', label: 'JUAL' },
                      { id: 'COST', label: 'BIAYA' },
                      { id: 'LOSSES', label: 'SUSUT' },
                      { id: 'DEBT', label: 'HUTANG' },
                      { id: 'CAPITAL', label: 'MODAL' }
                    ].map(t => (
                      <button 
                        key={t.id} 
                        type="button" 
                        onClick={() => setForm({...form, type: t.id})} 
                        className={`py-4 rounded-2xl border-2 font-black text-[10px] tracking-widest transition-all ${form.type === t.id ? 'border-emerald-500 bg-emerald-500/10 text-emerald-500' : 'border-slate-800 text-slate-700 hover:border-slate-700'}`}
                      >
                        {t.label}
                      </button>
                    ))}
                  </div>

                  <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
                    {/* Fields vary based on type */}
                    {form.type === 'LOSSES' ? (
                      <>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Berat Susut (Kg)</label>
                          <input type="number" value={form.bruto} onChange={e => setForm({...form, bruto: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-rose-500 text-rose-400 font-black text-xl" required />
                        </div>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Harga Beli Rata-rata (Rp/Kg)</label>
                          <input type="number" value={form.price} onChange={e => setForm({...form, price: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-rose-500 text-emerald-400 font-black text-xl" required />
                        </div>
                        <div className="space-y-3 md:col-span-2">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Keterangan / Alasan Susut</label>
                          <input type="text" value={form.entity} onChange={e => setForm({...form, entity: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-rose-500" placeholder="Susut timbangan PKS / Bongkar muat" required />
                        </div>
                      </>
                    ) : form.type === 'COST' || form.type === 'DEBT' || form.type === 'CAPITAL' ? (
                      <>
                        <div className="space-y-3 md:col-span-2">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Deskripsi Transaksi</label>
                          <input type="text" value={form.entity} onChange={e => setForm({...form, entity: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500 uppercase font-bold" required />
                        </div>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Nominal Rupiah (IDR)</label>
                          <input type="number" value={form.price} onChange={e => setForm({...form, price: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500 text-emerald-400 font-black text-xl" required />
                        </div>
                      </>
                    ) : (
                      <>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Mitra / PKS</label>
                          <input type="text" value={form.entity} onChange={e => setForm({...form, entity: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500 uppercase font-bold" required />
                        </div>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">No. Plat Kendaraan</label>
                          <input type="text" value={form.plate} onChange={e => setForm({...form, plate: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500 uppercase" />
                        </div>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Bruto (Kg)</label>
                          <input type="number" value={form.bruto} onChange={e => setForm({...form, bruto: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500" required />
                        </div>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Tara (Kg)</label>
                          <input type="number" value={form.tara} onChange={e => setForm({...form, tara: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500" />
                        </div>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Harga Per Kg (IDR)</label>
                          <input type="number" value={form.price} onChange={e => setForm({...form, price: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500 text-emerald-400 font-black text-xl" required />
                        </div>
                        <div className="space-y-3">
                          <label className="text-[10px] font-black text-slate-500 uppercase tracking-widest">Potongan (%)</label>
                          <input type="number" step="0.1" value={form.pot} onChange={e => setForm({...form, pot: e.target.value})} className="w-full bg-[#0f172a] border border-slate-700 rounded-2xl px-5 py-4 outline-none focus:border-emerald-500" />
                        </div>
                      </>
                    )}
                  </div>

                  <button type="submit" className={`w-full py-6 rounded-3xl font-black text-sm tracking-[0.4em] text-white shadow-2xl active:scale-[0.98] transition-all ${form.type === 'LOSSES' ? 'bg-rose-600 shadow-rose-600/20' : 'bg-emerald-600 shadow-emerald-600/20'}`}>
                    UPLOAD DATA KE CLOUD
                  </button>
                </form>
              </div>
            )}

            {activeTab === 'database' && (
              <div className="space-y-6">
                <div className="bg-[#1e293b] p-5 rounded-[2rem] border border-slate-800 flex flex-wrap gap-4 items-center shadow-lg">
                   <div className="flex items-center gap-3 bg-[#0f172a] px-5 py-3 rounded-2xl border border-slate-700 text-xs font-bold">
                      <Calendar size={16} className="text-emerald-500" />
                      <input type="date" value={filterDate} onChange={e => setFilterDate(e.target.value)} className="bg-transparent outline-none text-slate-200" />
                   </div>
                   <div className="flex items-center gap-3 bg-[#0f172a] px-5 py-3 rounded-2xl border border-slate-700 text-xs font-bold">
                      <Filter size={16} className="text-emerald-500" />
                      <select value={filterType} onChange={e => setFilterType(e.target.value)} className="bg-transparent outline-none text-slate-200">
                         <option value="ALL">SEMUA TIPE</option>
                         <option value="IN">BELI (IN)</option>
                         <option value="OUT">JUAL (OUT)</option>
                         <option value="COST">BIAYA OPERASIONAL</option>
                         <option value="LOSSES">SUSUT BARANG</option>
                      </select>
                   </div>
                   <div className="ml-auto text-[10px] font-black text-slate-500 uppercase tracking-[0.2em]">
                      Total: {filteredTransactions.length} baris data
                   </div>
                </div>

                <div className="bg-[#1e293b] rounded-[3rem] border border-slate-800 overflow-hidden shadow-2xl">
                  <div className="overflow-x-auto">
                    <table className="w-full text-left border-collapse">
                      <thead className="bg-[#0f172a] text-slate-500 text-[10px] uppercase font-black tracking-widest border-b border-slate-800">
                        <tr>
                          <th className="px-8 py-6">Tanggal</th>
                          <th className="px-8 py-6">Keterangan</th>
                          <th className="px-8 py-6 text-right">Netto (Kg)</th>
                          <th className="px-8 py-6 text-right">Total Nominal</th>
                          <th className="px-8 py-6 text-center">Menu</th>
                        </tr>
                      </thead>
                      <tbody className="divide-y divide-slate-800/50">
                        {filteredTransactions.map((t) => (
                          <tr key={t.id} className="hover:bg-slate-800/30 transition-all group">
                            <td className="px-8 py-6">
                               <div className="text-[11px] text-slate-400 font-bold">{t.dateDisplay}</div>
                               <div className={`mt-1 inline-block px-2 py-0.5 rounded text-[8px] font-black border ${
                                  t.type === 'IN' ? 'border-emerald-500/50 text-emerald-500' : 
                                  t.type === 'OUT' ? 'border-blue-500/50 text-blue-500' : 
                                  t.type === 'COST' ? 'border-rose-500/50 text-rose-500' :
                                  'border-orange-500/50 text-orange-500'
                               }`}>
                                 {t.type}
                               </div>
                            </td>
                            <td className="px-8 py-6">
                               <div className="font-bold text-white text-[15px] uppercase tracking-tight">{t.entity}</div>
                               <div className="text-[10px] text-slate-500 font-black tracking-wider mt-1 uppercase opacity-70">
                                 {t.plate || t.note || 'Transaksi Umum'}
                               </div>
                            </td>
                            <td className="px-8 py-6 text-right font-bold text-slate-300">
                              {t.netto > 0 ? t.netto.toLocaleString() : '-'}
                            </td>
                            <td className={`px-8 py-6 text-right font-black text-[15px] ${t.type === 'OUT' || (t.type === 'CAPITAL' && t.flow === '+') ? 'text-emerald-400' : 'text-rose-400'}`}>
                              {t.type === 'LOSSES' ? `-Rp ${t.total?.toLocaleString()}` : `Rp ${t.total?.toLocaleString()}`}
                            </td>
                            <td className="px-8 py-6 text-center">
                              {/* TOMBOL HAPUS */}
                              <button 
                                onClick={() => confirmDelete(t)} 
                                className="p-3 text-slate-600 hover:text-rose-500 hover:bg-rose-500/10 rounded-2xl transition-all active:scale-90"
                                title="Hapus Transaksi"
                              >
                                <Trash2 size={20} />
                              </button>
                            </td>
                          </tr>
                        ))}
                      </tbody>
                    </table>
                  </div>
                  {filteredTransactions.length === 0 && (
                    <div className="p-20 text-center">
                       <Database className="mx-auto text-slate-800 mb-4" size={48} />
                       <p className="text-slate-600 font-bold uppercase text-xs tracking-widest">Tidak ada data ditemukan</p>
                    </div>
                  )}
                </div>
              </div>
            )}

            {activeTab === 'financial' && (
               <div className="grid grid-cols-1 md:grid-cols-2 gap-8 pb-10">
                  <div className="bg-[#1e293b] p-10 rounded-[3rem] border border-emerald-500/10 shadow-2xl relative overflow-hidden group">
                     <div className="absolute top-0 right-0 p-10 opacity-[0.03] group-hover:scale-110 transition-transform">
                       <Palmtree size={180} className="text-emerald-500" />
                     </div>
                     <div className="flex items-center gap-4 mb-12">
                        <div className="p-3 bg-emerald-500/10 text-emerald-500 rounded-2xl"><TrendingUp size={20} /></div>
                        <h3 className="text-emerald-500 font-black text-xs tracking-[.4em] uppercase">NERACA AKTIVA</h3>
                     </div>
                     <div className="space-y-8 relative z-10">
                        <div className="flex justify-between items-end border-b border-slate-800/50 pb-6">
                           <div><p className="text-white font-bold text-lg">Saldo Kas</p><p className="text-[10px] text-slate-600 font-black uppercase">Liquid Asset</p></div>
                           <span className="text-2xl font-black text-white">Rp {stats.cash.toLocaleString()}</span>
                        </div>
                        <div className="flex justify-between items-end border-b border-slate-800/50 pb-6">
                           <div><p className="text-white font-bold text-lg">Estimasi Aset Stok</p><p className="text-[10px] text-slate-600 font-black uppercase">{stats.stock.toLocaleString()} Kg Sawit</p></div>
                           <span className="text-2xl font-black text-white">Rp {(stats.stock * 2200).toLocaleString()}</span>
                        </div>
                        <div className="pt-8 flex justify-between items-center text-emerald-400">
                           <span className="font-black text-xs tracking-[0.3em] uppercase opacity-70">Total Nilai Aset</span>
                           <span className="text-4xl font-black tracking-tighter italic">Rp {(stats.cash + (stats.stock * 2200)).toLocaleString()}</span>
                        </div>
                     </div>
                  </div>

                  <div className="bg-[#1e293b] p-10 rounded-[3rem] border border-indigo-500/10 shadow-2xl relative overflow-hidden group">
                     <div className="absolute top-0 right-0 p-10 opacity-[0.03] group-hover:scale-110 transition-transform">
                       <Info size={180} className="text-indigo-500" />
                     </div>
                     <div className="flex items-center gap-4 mb-12">
                        <div className="p-3 bg-indigo-500/10 text-indigo-500 rounded-2xl"><Info size={20} /></div>
                        <h3 className="text-indigo-500 font-black text-xs tracking-[.4em] uppercase">PASIVA & LABA</h3>
                     </div>
                     <div className="space-y-8 relative z-10">
                        <div className="flex justify-between items-end border-b border-slate-800/50 pb-6">
                           <div><p className="text-white font-bold text-lg">Total Kewajiban</p><p className="text-[10px] text-slate-600 font-black uppercase">Debts/Hutang</p></div>
                           <span className="text-2xl font-black text-amber-500">Rp {stats.totalDebt.toLocaleString()}</span>
                        </div>
                        <div className="flex justify-between items-end border-b border-slate-800/50 pb-6">
                           <div><p className="text-white font-bold text-lg">Laba Bersih</p><p className="text-[10px] text-rose-500 font-black uppercase">Potongan Susut: Rp {stats.rupiahLoss.toLocaleString()}</p></div>
                           <span className="text-2xl font-black text-emerald-400">Rp {stats.profit.toLocaleString()}</span>
                        </div>
                        <div className="pt-8 flex justify-between items-center text-indigo-400">
                           <span className="font-black text-xs tracking-[0.3em] uppercase opacity-70">Ekuitas & Pasiva</span>
                           <span className="text-4xl font-black tracking-tighter italic">Rp {(stats.totalDebt + stats.totalCapital + stats.profit).toLocaleString()}</span>
                        </div>
                     </div>
                  </div>
               </div>
            )}
          </>
        )}
      </main>
    </div>
  );
};

export default App;

