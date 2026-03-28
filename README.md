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
        .logo{font-family:'Orbitron',monospace;font-size:16px;font-weight:900;letter-spacing:2px;color:var(--accent);text-shadow:0 0 20px rgba(0,212,255,.5)}
        .logo span{color:var(--gold)}
        .hmeta{font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--dim);text-align:right;line-height:1.7}
        .hmeta b{color:var(--accent)}
        .controls{padding:12px 20px;display:flex;align-items:center;gap:12px;border-bottom:1px solid var(--border);flex-wrap:wrap;background:var(--panel)}
        .cl{font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--dim);text-transform:uppercase;letter-spacing:1px}
        input[type=number]{background:var(--bg);border:1px solid var(--border);color:var(--text);font-family:'Share Tech Mono',monospace;font-size:11px;padding:5px 8px;border-radius:4px;outline:none;width:60px}
        .btn{background:linear-gradient(135deg,#0047a0,#0080d4);border:1px solid var(--accent);color:#fff;font-family:'Orbitron',monospace;font-size:10px;font-weight:700;letter-spacing:1px;padding:8px 14px;border-radius:4px;cursor:pointer;text-transform:uppercase}
        .btn:disabled{opacity:.4}
        
        /* Auto Refresh Switch */
        .auto-box{display:flex;align-items:center;gap:8px;padding-left:10px;border-left:1px solid var(--border)}
        .switch{position:relative;display:inline-block;width:34px;height:18px}
        .switch input{opacity:0;width:0;height:0}
        .slider{position:absolute;cursor:pointer;top:0;left:0;right:0;bottom:0;background:#1a2a40;transition:.4s;border-radius:18px;border:1px solid var(--border)}
        .slider:before{position:absolute;content:"";height:12px;width:12px;left:2px;bottom:2px;background:var(--dim);transition:.4s;border-radius:50%}
        input:checked + .slider{background:rgba(0,212,255,.2);border-color:var(--accent)}
        input:checked + .slider:before{transform:translateX(16px);background:var(--accent);box-shadow:0 0 8px var(--accent)}
        #timer-display{font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--gold);min-width:35px}

        #pw{height:3px;background:var(--border);display:none}
        #pb{height:100%;background:linear-gradient(90deg,#7b2fff,var(--accent));width:0%;transition:width .3s}
        #sb{padding:7px 20px;font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--dim);background:#070d18;border-bottom:1px solid var(--border);display:flex;align-items:center;gap:10px}
        .dot{width:7px;height:7px;border-radius:50%;background:var(--dim)}
        .dot.on{background:var(--accent);box-shadow:0 0 8px var(--accent);animation:pulse 1s infinite}
        .dot.ok{background:var(--green)}
        .dot.er{background:var(--red)}
        @keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}

        #summary{display:none;padding:12px 20px;gap:8px;flex-wrap:wrap;background:var(--panel);border-bottom:1px solid var(--border)}
        .sc{flex:1;min-width:80px;padding:8px;border-radius:6px;border:1px solid var(--border);text-align:center}
        .sv{font-family:'Orbitron',monospace;font-size:18px;font-weight:900}
        .sl{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--dim);margin-top:2px}
        
        /* Indicators Colors */
        .csb{border-color:var(--green);background:#002211}.csb .sv{color:var(--green)}
        .css2{border-color:var(--red);background:#220005}.css2 .sv{color:var(--red)}

        .fbar{padding:8px 20px;display:none;gap:5px;overflow-x:auto;background:#070d18;border-bottom:1px solid var(--border)}
        .fb{font-family:'Share Tech Mono',monospace;font-size:9px;padding:4px 8px;border-radius:3px;border:1px solid var(--border);background:transparent;color:var(--dim);white-space:nowrap}
        .fb.on{border-color:var(--accent);color:var(--accent)}

        .tw{overflow-x:auto;padding:0 10px 40px}
        table{width:100%;border-collapse:collapse;margin-top:10px;font-size:11px}
        th{font-family:'Share Tech Mono',monospace;font-size:8px;padding:8px 4px;background:#050a12;color:var(--accent);border-bottom:1px solid var(--border)}
        td{padding:8px 4px;text-align:center;border-bottom:1px solid rgba(14,32,64,.3)}
        .ts2{text-align:left;color:var(--accent);font-weight:700}
        .bdg{padding:2px 4px;border-radius:3px;font-size:9px;font-weight:bold}
        
        /* Signal Badges */
        .bsb{background:var(--green);color:#000}
        .bss{background:var(--red);color:#fff}
        .bn{background:var(--muted);color:var(--text)}

        .vps{color:var(--green)}.vns{color:var(--red)}.vz{color:var(--dim)}
    </style>
</head>
<body>

<header>
    <div>
        <div class="logo">Aulyeah! <span>&#127302;</span> MATRIX</div>
        <div style="font-size:8px;color:var(--dim);letter-spacing:1px">BINANCE FUTURES &middot; REAL-TIME</div>
    </div>
    <div class="hmeta">
        <div><b id="coin-count">0</b> coins</div>
        <div id="ts">STANDBY</div>
    </div>
</header>

<div class="controls">
    <span class="cl">TOP</span>
    <input type="number" id="top-n" value="50">
    <label style="font-size:10px"><input type="checkbox" id="tf-15m" checked> 15m</label>
    <label style="font-size:10px"><input type="checkbox" id="tf-1h" checked> 1h</label>
    <button class="btn" id="scan-btn" onclick="startScan()">SCAN</button>
    
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
<div id="sb"><div class="dot" id="dot"></div><span id="stxt">Siap menscan data...</span></div>

<div id="summary">
    <div class="sc csb"><div class="sv" id="c-sb">0</div><div class="sl">STR BUY</div></div>
    <div class="sc cm"><div class="sv" id="c-m">0</div><div class="sl">MIXED</div></div>
    <div class="sc css2"><div class="sv" id="c-ss">0</div><div class="sl">STR SELL</div></div>
</div>

<div class="fbar" id="fbar">
    <button class="fb on" onclick="setFilter('all',this)">ALL</button>
    <button class="fb" onclick="setFilter('strong-buy',this)">STRONG BUY</button>
    <button class="fb" onclick="setFilter('strong-sell',this)">STRONG SELL</button>
</div>

<div class="tw">
    <div id="empty" style="text-align:center;padding:40px;color:var(--dim)">TEKAN SCAN</div>
    <table id="tbl" style="display:none">
        <thead id="thead"></thead>
        <tbody id="tbody"></tbody>
    </table>
</div>

<script>
const BASE = "https://fapi.binance.com";
const PROXIES = [
    u => "https://api.allorigins.win/raw?url=" + encodeURIComponent(u),
    u => "https://corsproxy.io/?" + encodeURIComponent(u)
];

let allRows = [];
let isScanning = false;
let refreshTimer = null;
let timeRemaining = 900; // 15 menit

async function fetchJSON(url) {
    for (let proxy of PROXIES) {
        try {
            const resp = await fetch(proxy(url), { signal: AbortSignal.timeout(10000) });
            if (resp.ok) return await resp.json();
        } catch (e) { console.error("Proxy failed, trying next..."); }
    }
    throw new Error("Gagal mengambil data. Cek koneksi/VPN.");
}

function toggleAutoRefresh() {
    const isChecked = document.getElementById('auto-refresh-switch').checked;
    if (isChecked) {
        timeRemaining = 900;
        refreshTimer = setInterval(() => {
            if (isScanning) return;
            timeRemaining--;
            updateTimerUI();
            if (timeRemaining <= 0) startScan();
        }, 1000);
    } else {
        clearInterval(refreshTimer);
        document.getElementById('timer-display').textContent = "OFF";
    }
}

function updateTimerUI() {
    const m = Math.floor(timeRemaining / 60);
    const s = timeRemaining % 60;
    document.getElementById('timer-display').textContent = `${m}:${s < 10 ? '0' : ''}${s}`;
}

async function startScan() {
    if (isScanning) return;
    isScanning = true;
    
    const topN = document.getElementById('top-n').value;
    const tfs = ['15m', '1h'].filter(tf => document.getElementById('tf-' + tf).checked);
    
    const btn = document.getElementById('scan-btn');
    const dot = document.getElementById('dot');
    const stxt = document.getElementById('stxt');
    const pb = document.getElementById('pb');
    
    btn.disabled = true;
    dot.className = 'dot on';
    document.getElementById('pw').style.display = 'block';
    
    try {
        stxt.textContent = "Mengambil daftar koin...";
        let data = await fetchJSON(BASE + "/fapi/v1/ticker/24hr");
        
        // Handle AllOrigins wrapper if exists
        if (data.contents) data = JSON.parse(data.contents);
        
        const symbols = data.filter(d => d.symbol.endsWith("USDT"))
            .sort((a, b) => b.quoteVolume - a.quoteVolume)
            .slice(0, topN);

        document.getElementById('coin-count').textContent = symbols.length;
        renderHeader(tfs);
        
        allRows = [];
        const tbody = document.getElementById('tbody');
        tbody.innerHTML = "";
        document.getElementById('empty').style.display = 'none';
        document.getElementById('tbl').style.display = 'table';

        for (let i = 0; i < symbols.length; i++) {
            const s = symbols[i];
            stxt.textContent = `Analyzing [${i+1}/${symbols.length}] ${s.symbol}`;
            pb.style.width = `${((i + 1) / symbols.length) * 100}%`;
            
            const rowData = await analyzeSymbol(s.symbol, s.lastPrice, tfs);
            allRows.push(rowData);
            tbody.insertAdjacentHTML('beforeend', renderRow(rowData, i + 1, tfs));
            await new Promise(r => setTimeout(r, 50));
        }

        stxt.textContent = "Scan Selesai!";
        dot.className = 'dot ok';
        updateSummary();
        document.getElementById('fbar').style.display = 'flex';
        timeRemaining = 900;
    } catch (e) {
        stxt.textContent = "Error: " + e.message;
        dot.className = 'dot er';
    }
    
    btn.disabled = false;
    isScanning = false;
    setTimeout(() => document.getElementById('pw').style.display = 'none', 1000);
}

async function analyzeSymbol(symbol, price, tfs) {
    let scores = [];
    let tfData = {};
    for (let tf of tfs) {
        try {
            const k = await fetchJSON(`${BASE}/fapi/v1/klines?symbol=${symbol}&interval=${tf}&limit=50`);
            const c = k.map(x => parseFloat(x[4]));
            const last = c[c.length - 1];
            const prev = c[c.length - 2];
            const trend = ((last - prev) / prev) * 100;
            const signal = trend > 0.5 ? "STRONG BUY" : trend < -0.5 ? "STRONG SELL" : "NEUTRAL";
            
            tfData[tf] = { trend, signal };
            scores.push(trend > 0.5 ? 1 : trend < -0.5 ? -1 : 0);
        } catch (e) { tfData[tf] = { trend: 0, signal: "ERR" }; }
    }
    const avg = scores.reduce((a, b) => a + b, 0);
    const overall = avg >= 1 ? "STRONG BUY" : avg <= -1 ? "STRONG SELL" : "MIXED";
    return { symbol, price, tfData, overall };
}

function renderHeader(tfs) {
    let h = `<tr><th>#</th><th style="text-align:left">SYMBOL</th>`;
    tfs.forEach(tf => h += `<th>${tf.toUpperCase()}</th>`);
    h += `<th>OVERALL</th></tr>`;
    document.getElementById('thead').innerHTML = h;
}

function renderRow(r, idx, tfs) {
    let h = `<tr data-filter="${r.overall.toLowerCase().replace(' ','-')}">
        <td>${idx}</td>
        <td class="ts2">${r.symbol.replace('USDT','')}</td>`;
    tfs.forEach(tf => {
        const d = r.tfData[tf];
        const cls = d.trend > 0 ? 'vps' : d.trend < 0 ? 'vns' : 'vz';
        h += `<td><span class="${cls}">${d.trend.toFixed(2)}%</span></td>`;
    });
    const ovCls = r.overall === "STRONG BUY" ? "bsb" : r.overall === "STRONG SELL" ? "bss" : "bn";
    h += `<td><span class="bdg ${ovCls}">${r.overall}</span></td></tr>`;
    return h;
}

function updateSummary() {
    const counts = { 'strong-buy': 0, 'strong-sell': 0, 'mixed': 0 };
    allRows.forEach(r => {
        const key = r.overall.toLowerCase().replace(' ', '-');
        if (counts[key] !== undefined) counts[key]++;
    });
    document.getElementById('c-sb').textContent = counts['strong-buy'];
    document.getElementById('c-ss').textContent = counts['strong-sell'];
    document.getElementById('c-m').textContent = counts['mixed'];
    document.getElementById('summary').style.display = 'flex';
}

function setFilter(f, btn) {
    document.querySelectorAll('.fb').forEach(b => b.classList.remove('on'));
    btn.classList.add('on');
    document.querySelectorAll('#tbody tr').forEach(tr => {
        tr.style.display = (f === 'all' || tr.dataset.filter === f) ? '' : 'none';
    });
}
</script>
</body>
</html>
