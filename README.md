<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Aulyeah! Predictive Trend Matrix</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@400;600;700&family=Orbitron:wght@700;900&display=swap" rel="stylesheet">
<style>
:root{--bg:#060b14;--panel:#0a1220;--border:#0e2040;--accent:#00d4ff;--gold:#ffd600;--green:#00ff88;--red:#ff3b5c;--muted:#3a5070;--text:#c8dff5;--dim:#4a6a8a}
*{margin:0;padding:0;box-sizing:border-box}
body{background:var(--bg);color:var(--text);font-family:'Rajdhani',sans-serif;font-size:14px;min-height:100vh;overflow-x:hidden}
body::before{content:'';position:fixed;inset:0;background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(0,212,255,.015) 2px,rgba(0,212,255,.015) 4px);pointer-events:none;z-index:9999}
header{padding:16px 20px 12px;border-bottom:1px solid var(--border);background:linear-gradient(90deg,#060b14,#0a1428,#060b14);display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100}
.logo{font-family:'Orbitron',monospace;font-size:18px;font-weight:900;letter-spacing:2px;color:var(--accent);text-shadow:0 0 20px rgba(0,212,255,.5)}
.logo span{color:var(--gold)}
.hmeta{font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--dim);text-align:right;line-height:1.7}
.hmeta b{color:var(--accent)}
.controls{padding:12px 20px;display:flex;align-items:center;gap:12px;border-bottom:1px solid var(--border);flex-wrap:wrap;background:var(--panel)}
.cl{font-family:'Share Tech Mono',monospace;font-size:11px;color:var(--dim);text-transform:uppercase;letter-spacing:1px}
input[type=number]{background:var(--bg);border:1px solid var(--border);color:var(--text);font-family:'Share Tech Mono',monospace;font-size:12px;padding:6px 8px;border-radius:4px;outline:none;width:65px}
.btn{background:linear-gradient(135deg,#0047a0,#0080d4);border:1px solid var(--accent);color:#fff;font-family:'Orbitron',monospace;font-size:11px;font-weight:700;letter-spacing:1px;padding:8px 16px;border-radius:4px;cursor:pointer;transition:all .2s;text-transform:uppercase}
.btn:hover{box-shadow:0 0 16px rgba(0,212,255,.4)}
.btn:disabled{opacity:.4;cursor:not-allowed}
.bcsv{background:linear-gradient(135deg,#1a3a00,#2e6000);border-color:var(--green);color:var(--green)}

/* Auto Refresh Switch UI */
.auto-box{display:flex;align-items:center;gap:8px;border-left:1px solid var(--border);padding-left:12px;margin-left:5px}
.switch{position:relative;display:inline-block;width:36px;height:20px}
.switch input{opacity:0;width:0;height:0}
.slider{position:absolute;cursor:pointer;top:0;left:0;right:0;bottom:0;background:#1a2a40;transition:.4s;border-radius:20px;border:1px solid var(--border)}
.slider:before{position:absolute;content:"";height:14px;width:14px;left:2px;bottom:2px;background:var(--dim);transition:.4s;border-radius:50%}
input:checked + .slider{background:rgba(0,212,255,.2);border-color:var(--accent)}
input:checked + .slider:before{transform:translateX(16px);background:var(--accent);box-shadow:0 0 8px var(--accent)}
#timer-display{font-family:'Share Tech Mono',monospace;font-size:11px;color:var(--gold);min-width:35px}

#pw{height:3px;background:var(--border);display:none}
#pb{height:100%;background:linear-gradient(90deg,#7b2fff,var(--accent));width:0%;transition:width .3s}
#sb{padding:8px 20px;font-family:'Share Tech Mono',monospace;font-size:11px;color:var(--dim);background:#070d18;border-bottom:1px solid var(--border);min-height:30px;display:flex;align-items:center;gap:10px}
.dot{width:8px;height:8px;border-radius:50%;background:var(--dim)}
.dot.on{background:var(--accent);box-shadow:0 0 8px var(--accent);animation:pulse 1s infinite}
.dot.ok{background:var(--green)}
.dot.er{background:var(--red)}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}

#summary{display:none;padding:12px 20px;gap:8px;flex-wrap:wrap;border-bottom:1px solid var(--border);background:var(--panel)}
.sc{flex:1;min-width:90px;padding:8px 12px;border-radius:6px;border:1px solid var(--border);text-align:center}
.sv{font-family:'Orbitron',monospace;font-size:22px;font-weight:900;line-height:1}
.sl{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--dim);margin-top:4px;letter-spacing:1px}
.csb{background:#003322;border-color:#00ff88}.csb .sv{color:#00ff88}
.cb{background:#002214;border-color:#00cc66}.cb .sv{color:#00cc66}
.clb{background:#001a10;border-color:#33cc77}.clb .sv{color:#33cc77}
.cm{background:#101820;border-color:var(--muted)}.cm .sv{color:var(--dim)}
.cls{background:#1f1000;border-color:#ff9900}.cls .sv{color:#ff9900}
.cs{background:#1a0005;border-color:#ff3b5c}.cs .sv{color:#ff3b5c}
.css2{background:#220008;border-color:#ff0033}.css2 .sv{color:#ff0033}

.fbar{padding:8px 20px;display:none;gap:7px;overflow-x:auto;border-bottom:1px solid var(--border);background:#070d18}
.fb{font-family:'Share Tech Mono',monospace;font-size:10px;padding:5px 12px;border-radius:3px;border:1px solid var(--border);background:transparent;color:var(--dim);cursor:pointer;white-space:nowrap}
.fb.on{border-color:var(--accent);color:var(--accent);background:rgba(0,212,255,0.05)}

.tw{overflow-x:auto;padding:0 10px 40px}
table{width:100%;border-collapse:collapse;margin-top:14px;font-size:11px}
thead th{font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--accent);padding:8px 5px;background:#050a12;border-bottom:1px solid var(--accent);border-right:1px solid rgba(14,32,64,.4)}
thead tr:nth-child(2) th{font-size:9px;color:var(--dim);background:#070d18;border-bottom:2px solid var(--border)}
tbody tr{border-bottom:1px solid rgba(14,32,64,.4)}
tbody tr:hover{background:rgba(0,212,255,0.03)}
td{padding:10px 5px;text-align:center;white-space:nowrap;border-right:1px solid rgba(14,32,64,.2)}
.ts2{font-family:'Share Tech Mono',monospace;color:var(--accent);font-weight:700;text-align:left;padding-left:10px}
.vps{color:#00ff88}.vns{color:#ff3b5c}.vz{color:var(--dim)}
.bdg{padding:3px 6px;border-radius:3px;display:inline-block;font-weight:700;font-size:9px;white-space:nowrap}
.bsb{background:#004422;color:#00ff88;border:1px solid #00ff88}
.bss{background:#2a0008;color:#ff3b5c;border:1px solid #ff3b5c}
.bn{background:#101820;color:#506070;border:1px solid #304050}
</style>
</head>
<body>

<header>
  <div>
    <div class="logo">Aulyeah! <span>&#127302;</span> MATRIX</div>
    <div style="font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--dim);margin-top:2px;letter-spacing:1px">BINANCE FUTURES &middot; REAL-TIME</div>
  </div>
  <div class="hmeta"><div><b id="coin-count">0</b> coins</div><div id="ts">STANDBY</div></div>
</header>

<div class="controls">
  <span class="cl">TOP</span>
  <input type="number" id="top-n" value="50" min="10" max="200">
  <span class="cl">TF:</span>
  <label style="display:flex;align-items:center;gap:3px;cursor:pointer"><input type="checkbox" id="tf-15m" value="15m" checked><span class="cl">15m</span></label>
  <label style="display:flex;align-items:center;gap:3px;cursor:pointer"><input type="checkbox" id="tf-1h" value="1h" checked><span class="cl">1h</span></label>
  <label style="display:flex;align-items:center;gap:3px;cursor:pointer"><input type="checkbox" id="tf-4h" value="4h" checked><span class="cl">4h</span></label>
  <button class="btn" id="scan-btn" onclick="startScan()">SCAN</button>
  <button class="btn bcsv" id="csv-btn" onclick="exportCSV()" disabled>CSV</button>
  
  <div class="auto-box">
    <span class="cl">AUTO</span>
    <label class="switch">
      <input type="checkbox" id="auto-refresh-switch" onchange="toggleAutoRefresh()">
      <span class="slider"></span>
    </label>
    <span id="timer-display">OFF</span>
  </div>
</div>

<div id="pw"><div id="pb"></div></div>
<div id="sb"><div class="dot" id="dot"></div><span id="stxt">Siap. Klik SCAN untuk mulai.</span></div>

<div id="summary">
  <div class="sc csb"><div class="sv" id="c-sb">0</div><div class="sl">STR BUY</div></div>
  <div class="sc cb"><div class="sv" id="c-b">0</div><div class="sl">BUY</div></div>
  <div class="sc clb"><div class="sv" id="c-lb">0</div><div class="sl">LEAN BUY</div></div>
  <div class="sc cm"><div class="sv" id="c-m">0</div><div class="sl">MIXED</div></div>
  <div class="sc cls"><div class="sv" id="c-ls">0</div><div class="sl">LEAN SELL</div></div>
  <div class="sc cs"><div class="sv" id="c-s">0</div><div class="sl">SELL</div></div>
  <div class="sc css2"><div class="sv" id="c-ss">0</div><div class="sl">STR SELL</div></div>
</div>

<div class="fbar" id="fbar">
  <button class="fb on" onclick="setFilter('all',this)">ALL</button>
  <button class="fb" onclick="setFilter('strong-buy',this)">STR BUY</button>
  <button class="fb" onclick="setFilter('strong-sell',this)">STR SELL</button>
</div>

<div class="tw">
  <div id="empty" style="text-align:center;padding:60px;color:var(--dim);font-family:'Share Tech Mono',monospace">KLIK SCAN UNTUK DATA REAL-TIME</div>
  <table id="tbl" style="display:none"><thead id="thead"></thead><tbody id="tbody"></tbody></table>
</div>

<script>
const BASE = "https://fapi.binance.com";
// Menggunakan proxy yang paling jarang error
const PROXIES = [
  "https://api.codetabs.com/v1/proxy?quest=",
  "https://api.allorigins.win/raw?url=",
  "https://corsproxy.io/?"
];

let allRows = [];
let isScanning = false;
let refreshTimer = null;
let timeRemaining = 900; // 15 Menit

async function fetchJSON(url) {
  // Coba jalur tanpa proxy dulu (terkadang Binance buka akses)
  try {
    let r = await fetch(url, { signal: AbortSignal.timeout(5000) });
    if(r.ok) return await r.json();
  } catch(e) {}

  // Jika gagal, gunakan proxy secara berurutan
  for (let proxy of PROXIES) {
    try {
      let r = await fetch(proxy + encodeURIComponent(url), { signal: AbortSignal.timeout(10000) });
      if (r.ok) {
        let text = await r.text();
        return JSON.parse(text); 
      }
    } catch (e) { console.warn("Proxy gagal, ganti jalur..."); }
  }
  throw new Error("Gagal koneksi API. Periksa internet atau matikan VPN.");
}

function toggleAutoRefresh() {
  if (document.getElementById('auto-refresh-switch').checked) {
    timeRemaining = 900;
    if (!refreshTimer) refreshTimer = setInterval(tick, 1000);
  } else {
    clearInterval(refreshTimer); 
    refreshTimer = null;
    document.getElementById('timer-display').textContent = "OFF";
  }
}

function tick() {
  if (isScanning) return;
  timeRemaining--;
  let m = Math.floor(timeRemaining / 60), s = timeRemaining % 60;
  document.getElementById('timer-display').textContent = `${m}:${s < 10 ? '0' : ''}${s}`;
  if (timeRemaining <= 0) { timeRemaining = 900; startScan(); }
}

// === LOGIKA MATEMATIKA INDIKATOR ===
function ema(arr,p){
  let k=2/(p+1), out=arr.map(()=>NaN), start=arr.findIndex(v=>!isNaN(v));
  if(start===-1) return out; out[start]=arr[start];
  for(let i=start+1;i<arr.length;i++) out[i]=arr[i]*k+out[i-1]*(1-k);
  return out;
}
function sma(arr,p){ return arr.map((_,i)=>{if(i<p-1)return NaN; let s=0; for(let j=i-p+1;j<=i;j++)s+=arr[j]; return s/p;}); }

function calcVol(c){
  let mfv=c.map(x=>((x.c-x.l)-(x.h-x.c))/Math.max(x.h-x.l,0.0001)*x.v);
  let sm=sma(mfv,20), sv=sma(c.map(x=>x.v),20);
  return ema(sm.map((m,i)=>isNaN(m)?NaN:m/sv[i]*100),5);
}
function calcTrend(c){
  let pc=c.map((x,i)=>i===0?NaN:x.c-c[i-1].c);
  let ds=ema(ema(pc,25),13), da=ema(ema(pc.map(Math.abs),25),13);
  return ds.map((v,i)=>isNaN(v)?NaN:100*v/da[i]);
}
function calcDom(c){
  let d=c.map((x,i)=>i===0?NaN:x.c-c[i-1].c);
  let g=sma(d.map(v=>Math.max(v,0)),14), l=sma(d.map(v=>Math.max(-v,0)),14);
  let rsi=g.map((x,i)=>l[i]===0?NaN:100-100/(1+x/l[i]));
  return ema(rsi.map(r=>isNaN(r)?NaN:(r-50)*2),3);
}
function calcStruct(c){
  let hh=c.map((_,i)=>{if(i<19)return NaN; let m=-Infinity; for(let j=i-19;j<=i;j++)m=Math.max(m,c[j].h); return m;});
  let ll=c.map((_,i)=>{if(i<19)return NaN; let m=Infinity; for(let j=i-19;j<=i;j++)m=Math.min(m,c[j].l); return m;});
  let raw=c.map((x,i)=>isNaN(hh[i])?NaN:((x.c-ll[i])/Math.max(hh[i]-ll[i],0.0001))*200-100);
  return raw.map((v,i,a)=>{ if(i<7)return NaN; let s=0; for(let j=i-7;j<=i;j++)s+=a[j]*(j-(i-7)+1); return s/36; });
}

function getSignal(v,t,d,s){
  let vals=[v,t,d,s].filter(x=>!isNaN(x));
  if(!vals.length) return {sig:"N/A",score:0};
  let avg=vals.reduce((a,b)=>a+b,0)/vals.length;
  if(vals.every(x=>x>60)) return {sig:"STR BUY",score:4};
  if(vals.every(x=>x<-60)) return {sig:"STR SELL",score:-4};
  return avg>20?{sig:"BUY",score:2}:avg<-20?{sig:"SELL",score:-2}:{sig:"NEUTRAL",score:0};
}

async function startScan(){
  if(isScanning) return; 
  
  // Deteksi Timeframe yang dicentang dengan aman
  let tfs = [];
  document.querySelectorAll('input[id^="tf-"]:checked').forEach(cb => tfs.push(cb.value));
  
  if(tfs.length === 0){ alert("Harap centang minimal 1 timeframe (15m, 1h, atau 4h)!"); return; }
  
  isScanning=true;
  let topN=document.getElementById('top-n').value;
  let dot=document.getElementById('dot'), stxt=document.getElementById('stxt'), pb=document.getElementById('pb');
  
  document.getElementById('scan-btn').disabled=true;
  dot.className='dot on'; document.getElementById('pw').style.display='block';

  try{
    stxt.textContent="Menyambungkan ke Binance API...";
    let data = await fetchJSON(BASE + "/fapi/v1/ticker/24hr");
    
    // Validasi super ketat untuk menghindari Error data.filter
    if (!Array.isArray(data)) {
      if (data && Array.isArray(data.data)) data = data.data; 
      else throw new Error("Format data ditolak oleh Proxy. Silakan klik SCAN lagi.");
    }
    
    let symbols = data.filter(d=> d.symbol && d.symbol.endsWith("USDT"))
                      .sort((a,b)=> parseFloat(b.quoteVolume) - parseFloat(a.quoteVolume))
                      .slice(0,topN);
                      
    if(symbols.length === 0) throw new Error("Tidak ada koin USDT yang ditemukan.");

    document.getElementById('coin-count').textContent=symbols.length;

    renderHeader(tfs);
    allRows=[]; document.getElementById('tbody').innerHTML="";
    document.getElementById('empty').style.display='none';
    document.getElementById('tbl').style.display='table';

    for(let i=0; i<symbols.length; i++){
      let s = symbols[i];
      stxt.textContent=`[${i+1}/${symbols.length}] Menganalisa ${s.symbol}...`;
      pb.style.width=`${((i+1)/symbols.length)*100}%`;
      
      let row = { symbol: s.symbol, price: parseFloat(s.lastPrice), tfs: {} };
      let scores = [];
      for(let tf of tfs){
        try{
          let k = await fetchJSON(`${BASE}/fapi/v1/klines?symbol=${s.symbol}&interval=${tf}&limit=100`);
          if(!Array.isArray(k)) throw new Error("Bad klines");
          let c = k.map(x=>({o:+x[1],h:+x[2],l:+x[3],c:+x[4],v:+x[5]}));
          let n=c.length-1;
          let v=calcVol(c)[n], t=calcTrend(c)[n], d=calcDom(c)[n], st=calcStruct(c)[n];
          let res=getSignal(v,t,d,st);
          row.tfs[tf]={v,t,d,st,sig:res.sig};
          scores.push(res.score);
        }catch(e){ row.tfs[tf]={sig:"ERR"}; scores.push(0); }
        await new Promise(r=>setTimeout(r,50)); // Jeda anti-banned
      }
      let avg = scores.reduce((a,b)=>a+b,0);
      row.overall = avg>=3?"STRONG BUY":avg>=1?"BUY":avg<=-3?"STRONG SELL":avg<=-1?"SELL":"MIXED";
      
      allRows.push(row);
      document.getElementById('tbody').insertAdjacentHTML('beforeend', renderRow(row, i+1, tfs));
    }
    
    dot.className='dot ok'; stxt.textContent="Scan Selesai 100%!";
    document.getElementById('ts').textContent="Updated: "+new Date().toLocaleTimeString();
    updateSummary();
    document.getElementById('fbar').style.display='flex';
    document.getElementById('csv-btn').disabled=false;
    timeRemaining=900; 
  }catch(e){ 
    dot.className='dot er'; 
    stxt.textContent="Gagal: "+e.message; 
    console.error(e);
  }
  
  isScanning=false; document.getElementById('scan-btn').disabled=false;
  setTimeout(()=>document.getElementById('pw').style.display='none', 1500);
}

function renderHeader(tfs){
  let h1 = `<tr><th rowspan="2">#</th><th rowspan="2" style="text-align:left">SYMBOL</th><th rowspan="2">PRICE</th>`;
  tfs.forEach(tf=> h1+=`<th colspan="5" style="border-bottom:1px solid var(--accent)">${tf.toUpperCase()}</th>`);
  h1 += `<th rowspan="2">OVERALL</th></tr>`;
  
  let h2 = `<tr>`;
  tfs.forEach(()=> h2+=`<th>BDR</th><th>VEL</th><th>DOM</th><th>STR</th><th>SIG</th>`);
  h2 += `</tr>`;
  
  document.getElementById('thead').innerHTML=h1+h2;
}

function renderRow(r, idx, tfs){
  let h = `<tr data-filter="${r.overall.toLowerCase().replace(' ','-')}">
            <td>${idx}</td>
            <td class="ts2">${r.symbol.replace('USDT','')}</td>
            <td style="color:var(--gold)">${r.price >= 1 ? r.price.toFixed(4) : r.price.toFixed(6)}</td>`;
  tfs.forEach(tf=>{
    let d=r.tfs[tf];
    if(!d || d.sig==="ERR") h+=`<td colspan="5" style="color:var(--red)">DATA ERROR</td>`;
    else {
      h+=`<td class="${d.v>0?'vps':'vns'}">${d.v.toFixed(0)}</td>`;
      h+=`<td class="${d.t>0?'vps':'vns'}">${d.t.toFixed(0)}</td>`;
      h+=`<td class="${d.d>0?'vps':'vns'}">${d.d.toFixed(0)}</td>`;
      h+=`<td class="${d.st>0?'vps':'vns'}">${d.st.toFixed(0)}</td>`;
      h+=`<td><span class="bdg ${d.sig.includes('BUY')?'bsb':d.sig.includes('SELL')?'bss':'bn'}">${d.sig}</span></td>`;
    }
  });
  h+=`<td><span class="bdg ${r.overall.includes('BUY')?'bsb':r.overall.includes('SELL')?'bss':'bn'}">${r.overall.replace('STRONG','STR')}</span></td></tr>`;
  return h;
}

function updateSummary(){
  let counts={'strong-buy':0,'buy':0,'lean-buy':0,'mixed':0,'lean-sell':0,'sell':0,'strong-sell':0};
  allRows.forEach(r=>{ let k=r.overall.toLowerCase().replace(' ','-'); if(counts[k]!==undefined) counts[k]++; });
  document.getElementById('c-sb').textContent=counts['strong-buy'];
  document.getElementById('c-b').textContent=counts['buy'];
  document.getElementById('c-lb').textContent=counts['lean-buy'];
  document.getElementById('c-m').textContent=counts['mixed'];
  document.getElementById('c-ls').textContent=counts['lean-sell'];
  document.getElementById('c-s').textContent=counts['sell'];
  document.getElementById('c-ss').textContent=counts['strong-sell'];
  document.getElementById('summary').style.display='flex';
}

function setFilter(f,b){
  document.querySelectorAll('.fb').forEach(x=>x.classList.remove('on')); b.classList.add('on');
  document.querySelectorAll('#tbody tr').forEach(tr=> tr.style.display=(f==='all'||tr.dataset.filter===f)?'':'none');
}

function exportCSV(){
  let lines=["Symbol,Price,Overall"];
  allRows.forEach(r=> lines.push(`${r.symbol},${r.price},${r.overall}`));
  let blob=new Blob([lines.join('\n')], {type:'text/csv'});
  let a=document.createElement('a'); a.href=URL.createObjectURL(blob);
  a.download=`TrendMatrix_${new Date().getTime()}.csv`; a.click();
}
</script>
</body>
</html>
