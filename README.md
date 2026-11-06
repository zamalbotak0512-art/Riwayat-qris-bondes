<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#0b1220">
<title>QRIS Log — Riwayat Transaksi</title>
<style>
:root{--bg:#0b1220;--panel:#111c2e;--panel2:#16243a;--line:#263750;--text:#edf4ff;--muted:#91a4bd;--green:#34d399;--red:#fb7185;--yellow:#fbbf24;--blue:#60a5fa}
*{box-sizing:border-box}body{margin:0;background:radial-gradient(ellipse at top,#142743 0,#0b1220 52%);color:var(--text);font:15px/1.5 system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;min-height:100vh}
.wrap{max-width:1100px;margin:auto;padding:28px 18px 55px}.top{display:flex;align-items:center;justify-content:space-between;gap:14px;margin-bottom:24px}.brand{display:flex;align-items:center;gap:12px}.logo{width:45px;height:45px;border-radius:14px;background:linear-gradient(135deg,#34d399,#0ea5e9);display:grid;place-items:center;color:#05251f;font-size:23px;font-weight:900}.eyebrow{font-size:12px;color:var(--green);letter-spacing:1.6px;text-transform:uppercase;font-weight:800}.top h1{font-size:23px;margin:1px 0}.sub{color:var(--muted);font-size:13px}.tag{border:1px solid #27634f;color:#86efac;background:#0c2b25;padding:7px 11px;border-radius:999px;font-size:12px;white-space:nowrap}
.notice{background:#17253a;border:1px solid #31445e;border-radius:13px;padding:12px 15px;color:#c5d4e8;font-size:13px;margin-bottom:19px}.notice b{color:#fff}
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:13px;margin-bottom:18px}.stat,.card{background:rgba(17,28,46,.94);border:1px solid var(--line);border-radius:17px}.stat{padding:17px}.stat-label{font-size:12px;color:var(--muted)}.stat-value{font-size:clamp(19px,3vw,27px);font-weight:800;margin-top:6px;overflow-wrap:anywhere}.green{color:var(--green)}.blue{color:var(--blue)}
.layout{display:grid;grid-template-columns:350px 1fr;gap:16px;align-items:start}.card{padding:19px}.card h2{font-size:17px;margin:0 0 15px}.field{margin-bottom:12px}.field label{display:block;color:#b8c7da;font-size:12px;margin-bottom:6px}.field input,.field select,.search{width:100%;background:#0b1627;border:1px solid #30435e;border-radius:10px;padding:11px 12px;color:var(--text);font:inherit;outline:none}.field input:focus,.field select:focus,.search:focus{border-color:var(--green);box-shadow:0 0 0 3px #34d39920}input[type="datetime-local"]{color-scheme:dark}.actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:5px}button{border:0;border-radius:10px;padding:10px 13px;font:700 13px system-ui;cursor:pointer}.primary{background:var(--green);color:#06251d;flex:1}.secondary{background:#22334b;color:#dce9f8}.danger{background:#442333;color:#fecdd3}.small{padding:7px 9px;font-size:12px}.toolbar{display:flex;gap:9px;flex-wrap:wrap;margin-bottom:12px}.toolbar>*{flex:1;min-width:130px}.table-wrap{overflow-x:auto;border:1px solid var(--line);border-radius:12px}table{border-collapse:collapse;width:100%;min-width:620px}th,td{text-align:left;padding:12px 11px;border-bottom:1px solid #263750;font-size:13px}th{background:#17263c;color:#a9bad0;font-size:11px;text-transform:uppercase;letter-spacing:.6px}tr:last-child td{border-bottom:0}.amount{font-weight:800;white-space:nowrap}.muted{color:var(--muted)}.status{display:inline-block;padding:4px 8px;border-radius:99px;font-size:11px;font-weight:800}.success{color:#86efac;background:#123b30}.pending{color:#fde68a;background:#443817}.failed{color:#fda4af;background:#451e2a}.row-actions{display:flex;gap:5px}.empty{text-align:center;padding:35px 12px;color:var(--muted)}.foot{color:#71839b;text-align:center;font-size:12px;margin-top:20px}
@media(max-width:800px){.layout{grid-template-columns:1fr}.stats{gap:8px}.stat{padding:13px}.top h1{font-size:20px}}
@media(max-width:430px){.wrap{padding:20px 12px 40px}.tag{font-size:10px}.stats{grid-template-columns:1fr 1fr}.stats .stat:last-child{grid-column:span 2}.card{padding:15px}}
</style>
</head>
<body>
<div class="wrap">
  <header class="top">
    <div class="brand"><div class="logo">Q</div><div><div class="eyebrow">Personal finance tracker</div><h1>QRIS Transaction Log</h1><div class="sub">Catatan transaksi pembayaran manual</div></div></div>
    <div class="tag">● MODE MANUAL</div>
  </header>

  <div class="notice"><b>Catatan:</b> Website ini hanya untuk mencatat transaksi yang kamu masukkan sendiri. Website tidak terhubung ke bank atau penyedia QRIS dan tidak memverifikasi pembayaran.</div>

  <section class="stats">
    <div class="stat"><div class="stat-label">Total transaksi berhasil</div><div class="stat-value green" id="total">Rp0</div></div>
    <div class="stat"><div class="stat-label">Jumlah transaksi</div><div class="stat-value" id="count">0</div></div>
    <div class="stat"><div class="stat-label">Transaksi pending</div><div class="stat-value blue" id="pendingCount">0</div></div>
  </section>

  <main class="layout">
    <section class="card">
      <h2 id="formTitle">＋ Tambah transaksi</h2>
      <form id="txForm">
        <input type="hidden" id="editId">
        <div class="field"><label for="amount">Nominal (Rp)</label><input id="amount" type="number" min="1" step="1" placeholder="Contoh: 25000" required></div>
        <div class="field"><label for="date">Tanggal dan waktu</label><input id="date" type="datetime-local" required></div>
        <div class="field"><label for="ref">ID / catatan referensi (opsional)</label><input id="ref" maxlength="80" placeholder="Contoh: INV-001"></div>
        <div class="field"><label for="status">Status transaksi</label><select id="status"><option value="Berhasil">Berhasil</option><option value="Pending">Pending</option><option value="Gagal">Gagal</option></select></div>
        <div class="actions"><button class="primary" type="submit" id="saveBtn">Simpan transaksi</button><button class="secondary" type="button" id="cancelBtn" hidden>Batal</button></div>
      </form>
    </section>

    <section class="card">
      <h2>Riwayat transaksi</h2>
      <div class="toolbar">
        <input class="search" id="search" placeholder="Cari nama atau referensi...">
        <select class="search" id="filter"><option value="Semua">Semua status</option><option>Berhasil</option><option>Pending</option><option>Gagal</option></select>
        <button class="secondary" id="exportBtn" type="button">Ekspor CSV</button>
      </div>
      <div class="table-wrap">
        <table><thead><tr><th>Tanggal</th><th>Referensi</th><th>Nominal</th><th>Status</th><th>Aksi</th></tr></thead><tbody id="rows"></tbody></table>
        <div class="empty" id="empty">Belum ada transaksi. Tambahkan catatan pertama melalui formulir.</div>
      </div>
      <div class="actions" style="justify-content:flex-end;margin-top:12px"><button class="danger small" id="clearBtn" type="button">Hapus semua data</button></div>
    </section>
  </main>
  <div class="foot">Data disimpan di penyimpanan browser pada perangkat ini (localStorage). Ekspor CSV secara berkala sebagai cadangan.</div>
</div>
<script>
const $ = id => document.getElementById(id);
const KEY = 'qris_manual_transactions_v1';
let transactions = [];
try { transactions = JSON.parse(localStorage.getItem(KEY) || '[]'); if (!Array.isArray(transactions)) transactions=[]; } catch { transactions=[]; }
const rupiah = n => new Intl.NumberFormat('id-ID',{style:'currency',currency:'IDR',maximumFractionDigits:0}).format(Number(n)||0);
const esc = s => String(s ?? '').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const save = () => localStorage.setItem(KEY, JSON.stringify(transactions));
const dateLabel = s => { const d = new Date(s); return isNaN(d) ? s : d.toLocaleString('id-ID',{dateStyle:'medium',timeStyle:'short'}); };
function render(){
  $('total').textContent = rupiah(transactions.filter(t=>t.status==='Berhasil').reduce((a,t)=>a+Number(t.amount),0));
  $('count').textContent = transactions.length;
  $('pendingCount').textContent = transactions.filter(t=>t.status==='Pending').length;
  const q=$('search').value.trim().toLowerCase(), f=$('filter').value;
  const list=transactions.filter(t=>(f==='Semua'||t.status===f)&&(`${t.ref||'Transaksi QRIS'}`).toLowerCase().includes(q)).sort((a,b)=>new Date(b.date)-new Date(a.date));
  $('rows').innerHTML=list.map(t=>`<tr>
    <td>${esc(dateLabel(t.date))}</td><td><b>${esc(t.ref||'Transaksi QRIS')}</b></td>
    <td class="amount">${esc(rupiah(t.amount))}</td>
    <td><span class="status ${t.status==='Berhasil'?'success':t.status==='Pending'?'pending':'failed'}">${esc(t.status)}</span></td>
    <td><div class="row-actions"><button class="secondary small" data-edit="${esc(t.id)}" type="button">Edit</button><button class="danger small" data-delete="${esc(t.id)}" type="button">Hapus</button></div></td>
  </tr>`).join('');
  $('empty').style.display=list.length?'none':'block';
}
function resetForm(){
  $('txForm').reset(); $('editId').value=''; $('formTitle').textContent='＋ Tambah transaksi';
  $('saveBtn').textContent='Simpan transaksi'; $('cancelBtn').hidden=true;
  $('date').value=new Date(Date.now()-new Date().getTimezoneOffset()*60000).toISOString().slice(0,16);
}
$('txForm').addEventListener('submit',e=>{
  e.preventDefault();
  const id=$('editId').value;
  const item={id:id||`${Date.now()}-${Math.random().toString(16).slice(2)}`,amount:Number($('amount').value),date:$('date').value,ref:$('ref').value.trim(),status:$('status').value};
  if(!Number.isFinite(item.amount)||item.amount<=0||!item.date)return;
  if(id) transactions=transactions.map(t=>t.id===id?item:t); else transactions.push(item);
  save();render();resetForm();
});
$('cancelBtn').addEventListener('click',resetForm);
$('rows').addEventListener('click',e=>{
  const edit=e.target.closest('[data-edit]')?.dataset.edit;
  const del=e.target.closest('[data-delete]')?.dataset.delete;
  if(edit){
    const t=transactions.find(x=>x.id===edit); if(!t)return;
    $('editId').value=t.id;$('amount').value=t.amount;$('date').value=t.date;$('ref').value=t.ref||'';$('status').value=t.status;
    $('formTitle').textContent='✎ Edit transaksi';$('saveBtn').textContent='Simpan perubahan';$('cancelBtn').hidden=false;
    $('txForm').scrollIntoView({behavior:'smooth',block:'start'});
  }
  if(del&&confirm('Hapus catatan transaksi ini?')){transactions=transactions.filter(t=>t.id!==del);save();render();}
});
$('search').addEventListener('input',render);$('filter').addEventListener('change',render);
$('clearBtn').addEventListener('click',()=>{if(transactions.length&&confirm('Yakin ingin menghapus SEMUA catatan transaksi dari browser ini? Tindakan ini tidak bisa dibatalkan.')){transactions=[];save();render();resetForm();}});
$('exportBtn').addEventListener('click',()=>{
  if(!transactions.length){alert('Belum ada data untuk diekspor.');return;}
  const cols=['Tanggal','Nominal_Rp','Referensi','Status'];
  const rows=[cols,...transactions.slice().sort((a,b)=>new Date(a.date)-new Date(b.date)).map(t=>[dateLabel(t.date),t.amount,t.ref||'',t.status])];
  const csv='\uFEFF'+rows.map(r=>r.map(v=>'"'+String(v).replace(/"/g,'""')+'"').join(';')).join('\r\n');
  const blob=new Blob([csv],{type:'text/csv;charset=utf-8;'}),url=URL.createObjectURL(blob),a=document.createElement('a');
  a.href=url;a.download='riwayat-transaksi-qris.csv';a.click();URL.revokeObjectURL(url);
});
resetForm();render();
</script>
</body>
</html>
