<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Aulyeah! Predictive Trend Matrix 🔮</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@400;500;600;700&family=Orbitron:wght@700;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg:        #060b14;
    --panel:     #0a1220;
    --border:    #0e2040;
    --accent:    #00d4ff;
    --accent2:   #7b2fff;
    --gold:      #ffd600;
    --green:     #00ff88;
    --red:       #ff3b5c;
    --orange:    #ff9f00;
    --muted:     #3a5070;
    --text:      #c8dff5;
    --text-dim:  #4a6a8a;
    --strong-buy-bg:  #003322;
    --buy-bg:         #001a11;
    --lean-buy-bg:    #001f10;
    --mixed-bg:       #101820;
    --lean-sell-bg:   #1f1000;
    --sell-bg:        #1a0005;
    --strong-sell-bg: #220008;
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    font-size: 14px;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── Scanline overlay ── */
  body::before {
    content:'';
    position:fixed; inset:0;
    background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,212,255,0.015) 2px, rgba(0,212,255,0.015) 4px);
    pointer-events:none; z-index:9999;
  }

  /* ── Header ── */
  header {
    padding: 18px 28px 14px;
    border-bottom: 1px solid var(--border);
    background: linear-gradient(90deg, #060b14 0%, #0a1428 50%, #060b14 100%);
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky; top:0; z-index:100;
    backdrop-filter: blur(10px);
  }

  .logo {
    font-family: 'Orbitron', monospace;
    font-size: 18px;
    font-weight: 900;
    letter-spacing: 2px;
    color: var(--accent);
    text-shadow: 0 0 20px rgba(0,212,255,0.5);
  }
  .logo span { color: var(--gold); }

  .header-meta {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--text-dim);
    text-align: right;
    line-height: 1.6;
  }
  .header-meta b { color: var(--accent); }

  /* ── Controls ── */
  .controls {
    padding: 14px 28px;
    display: flex;
    align-items: center;
    gap: 16px;
    border-bottom: 1px solid var(--border);
    flex-wrap: wrap;
    background: var(--panel);
  }

  .ctrl-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  select, input[type=number] {
    background: var(--bg);
    border: 1px solid var(--border);
    color: var(--text);
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    padding: 6px 10px;
    border-radius: 4px;
    outline: none;
    cursor: pointer;
    transition: border-color .2s;
  }
  select:hover, input[type=number]:hover { border-color: var(--accent); }

  .btn {
    background: linear-gradient(135deg, #0047a0, #0080d4);
    border: 1px solid var(--accent);
    color: #fff;
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 2px;
    padding: 8px 20px;
    border-radius: 4px;
    cursor: pointer;
    transition: all .2s;
    text-transform: uppercase;
  }
  .btn:hover { background: linear-gradient(135deg, #005fcc, #00a8ff); box-shadow: 0 0 16px rgba(0,212,255,0.4); }
  .btn:disabled { opacity:.4; cursor:not-allowed; }

  .btn-export {
    background: linear-gradient(135deg, #1a3a00, #2e6000);
    border-color: var(--green);
    color: var(--green);
  }
  .btn-export:hover { background: linear-gradient(135deg, #2a5a00, #4a9200); box-shadow: 0 0 16px rgba(0,255,136,0.3); }

  /* ── Status bar ── */
  #status-bar {
    padding: 8px 28px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--text-dim);
    background: #070d18;
    border-bottom: 1px solid var(--border);
    min-height: 30px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .status-dot {
    width: 7px; height: 7px; border-radius: 50%;
    background: var(--text-dim); flex-shrink:0;
    transition: background .3s;
  }
  .status-dot.active { background: var(--accent); box-shadow: 0 0 8px var(--accent); animation: pulse 1s infinite; }
  .status-dot.done   { background: var(--green); box-shadow: 0 0 8px var(--green); animation: none; }
  .status-dot.error  { background: var(--red); }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.4} }

  /* ── Progress ── */
  #progress-wrap {
    height: 3px; background: var(--border);
    display: none;
  }
  #progress-bar {
    height: 100%;
    background: linear-gradient(90deg, var(--accent2), var(--accent));
    width: 0%;
    transition: width .3s;
    box-shadow: 0 0 10px var(--accent);
  }

  /* ── Summary cards ── */
  #summary {
    display: none;
    padding: 16px 28px;
    gap: 12px;
    flex-wrap: wrap;
    border-bottom: 1px solid var(--border);
    background: var(--panel);
  }
  .sum-card {
    flex: 1; min-width: 110px;
    padding: 10px 14px;
    border-radius: 6px;
    border: 1px solid var(--border);
    text-align: center;
  }
  .sum-card .sum-val {
    font-family: 'Orbitron', monospace;
    font-size: 22px;
    font-weight: 900;
    line-height: 1;
  }
  .sum-card .sum-lbl {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--text-dim);
    margin-top: 4px;
    letter-spacing: 1px;
  }
  .card-strong-buy  { background: var(--strong-buy-bg); border-color: #00ff88; }
  .card-strong-buy .sum-val { color: #00ff88; text-shadow: 0 0 12px #00ff88; }
  .card-buy         { background: var(--buy-bg); border-color: #00cc66; }
  .card-buy .sum-val { color: #00cc66; }
  .card-lean-buy    { background: var(--lean-buy-bg); border-color: #33cc77; }
  .card-lean-buy .sum-val { color: #33cc77; }
  .card-mixed       { background: var(--mixed-bg); border-color: var(--muted); }
  .card-mixed .sum-val { color: var(--text-dim); }
  .card-lean-sell   { background: var(--lean-sell-bg); border-color: #ff9900; }
  .card-lean-sell .sum-val { color: #ff9900; }
  .card-sell        { background: var(--sell-bg); border-color: #ff3b5c; }
  .card-sell .sum-val { color: #ff3b5c; }
  .card-strong-sell { background: var(--strong-sell-bg); border-color: #ff0033; }
  .card-strong-sell .sum-val { color: #ff0033; text-shadow: 0 0 12px #ff0033; }

  /* ── Table ── */
  .table-wrap {
    overflow-x: auto;
    padding: 0 28px 40px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 16px;
    font-size: 12px;
  }

  /* TF group headers */
  thead tr.tf-group th {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--accent);
    padding: 8px 6px 4px;
    border-bottom: 1px solid var(--accent);
    background: #050a12;
  }
  thead tr.tf-group th.th-meta { color: var(--text-dim); border-bottom-color: var(--border); }

  /* Column headers */
  thead tr.col-head th {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 1px;
    text-transform: uppercase;
    color: var(--text-dim);
    padding: 6px 6px;
    background: #070d18;
    border-bottom: 2px solid var(--border);
    white-space: nowrap;
    position: sticky; top: 0;
  }

  /* Rows */
  tbody tr {
    border-bottom: 1px solid rgba(14,32,64,0.6);
    transition: background .15s;
  }
  tbody tr:hover { background: rgba(0,212,255,0.04); }

  td {
    padding: 7px 6px;
    text-align: center;
    white-space: nowrap;
    border-right: 1px solid rgba(14,32,64,0.4);
  }
  td.td-num {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--text-dim);
    width: 36px;
  }
  td.td-symbol {
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    font-weight: 700;
    color: var(--accent);
    text-align: left;
    padding-left: 10px;
    width: 130px;
  }
  td.td-val {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    width: 68px;
  }
  td.td-sig {
    font-family: 'Rajdhani', sans-serif;
    font-size: 11px;
    font-weight: 700;
    width: 110px;
    letter-spacing: .5px;
  }
  td.td-overall {
    font-family: 'Rajdhani', sans-serif;
    font-size: 12px;
    font-weight: 700;
    width: 130px;
    letter-spacing: .5px;
  }
  td.td-price {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--gold);
    width: 90px;
  }

  /* Value coloring */
  .val-pos-strong { color: #00ff88; }
  .val-pos        { color: #00cc66; }
  .val-pos-weak   { color: #66bb88; }
  .val-neg-weak   { color: #bb8866; }
  .val-neg        { color: #ff6680; }
  .val-neg-strong { color: #ff3b5c; }
  .val-neutral    { color: var(--text-dim); }

  /* Signal badges */
  .sig { padding: 2px 7px; border-radius: 3px; display: inline-block; }
  .sig-strong-buy  { background: #004422; color: #00ff88; border: 1px solid #00ff88; }
  .sig-buy         { background: #002a14; color: #00cc66; border: 1px solid #00cc66; }
  .sig-weak-buy    { background: #001a0e; color: #66bb88; border: 1px solid #448855; }
  .sig-neutral     { background: #101820; color: #506070; border: 1px solid #304050; }
  .sig-weak-sell   { background: #1a0e00; color: #bb8833; border: 1px solid #886622; }
  .sig-sell        { background: #2a0008; color: #ff6680; border: 1px solid #cc3355; }
  .sig-strong-sell { background: #220005; color: #ff3b5c; border: 1px solid #ff3b5c; }

  /* Overall badges */
  .ov-strong-buy  { background: #003322; color: #00ff88; border: 1px solid #00ff88; box-shadow: 0 0 10px rgba(0,255,136,0.3); }
  .ov-buy         { background: #002214; color: #00cc66; border: 1px solid #00cc66; }
  .ov-lean-buy    { background: #001a10; color: #44aa66; border: 1px solid #33884f; }
  .ov-mixed       { background: #111920; color: #405060; border: 1px solid #253545; }
  .ov-lean-sell   { background: #1a0e00; color: #cc7700; border: 1px solid #aa6600; }
  .ov-sell        { background: #220006; color: #ff4466; border: 1px solid #cc2244; }
  .ov-strong-sell { background: #1a0004; color: #ff2244; border: 1px solid #ff2244; box-shadow: 0 0 10px rgba(255,34,68,0.3); }

  /* Filter bar */
  .filter-bar {
    padding: 10px 28px;
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    align-items: center;
    border-bottom: 1px solid var(--border);
    background: #070d18;
  }
  .filter-btn {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    padding: 4px 10px;
    border-radius: 3px;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--text-dim);
    cursor: pointer;
    letter-spacing: 1px;
    transition: all .15s;
  }
  .filter-btn:hover, .filter-btn.active { border-color: var(--accent); color: var(--accent); }
  .filter-btn.fb-strong-buy.active  { border-color: #00ff88; color: #00ff88; }
  .filter-btn.fb-strong-sell.active { border-color: #ff3b5c; color: #ff3b5c; }

  /* Skeleton loader */
  .skeleton td { background: linear-gradient(90deg, #0a1220 25%, #0f1c30 50%, #0a1220 75%); background-size: 200% 100%; animation: shimmer 1.2s infinite; }
  @keyframes shimmer { 0%{background-position:200% 0} 100%{background-position:-200% 0} }

  /* Scrollbar */
  ::-webkit-scrollbar { width:6px; height:6px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--muted); border-radius:3px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--accent); }

  .empty-state {
    text-align: center;
    padding: 80px 20px;
    font-family: 'Share Tech Mono', monospace;
    color: var(--text-dim);
  }
  .empty-state .empty-icon { font-size: 48px; margin-bottom: 16px; opacity: .5; }
  .empty-state p { font-size: 12px; letter-spacing: 1px; }

  #last-updated {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--text-dim);
  }
</style>
</head>
<body>

<!-- HEADER -->
<header>
  <div>
    <div class="logo">Aulyeah! <span>🔮</span> TREND MATRIX</div>
    <div style="font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--text-dim);margin-top:3px;letter-spacing:1px;">PREDICTIVE · BINANCE FUTURES · USDT PERPETUAL</div>
  </div>
  <div class="header-meta">
    <div><b id="coin-count">—</b> coins scanned</div>
    <div id="last-updated">—</div>
  </div>
</header>

<!-- CONTROLS -->
<div class="controls">
  <span class="ctrl-label">Top</span>
  <input type="number" id="top-n" value="50" min="10" max="200" step="10" style="width:70px">
  <span class="ctrl-label">koin</span>

  <span class="ctrl-label" style="margin-left:8px">Timeframes:</span>
  <label style="display:flex;align-items:center;gap:4px;cursor:pointer">
    <input type="checkbox" id="tf-15m" checked style="accent-color:var(--accent)"> <span style="font-family:'Share Tech Mono',monospace;font-size:11px">15m</span>
  </label>
  <label style="display:flex;align-items:center;gap:4px;cursor:pointer">
    <input type="checkbox" id="tf-1h" checked style="accent-color:var(--accent)"> <span style="font-family:'Share Tech Mono',monospace;font-size:11px">1h</span>
  </label>
  <label style="display:flex;align-items:center;gap:4px;cursor:pointer">
    <input type="checkbox" id="tf-4h" checked style="accent-color:var(--accent)"> <span style="font-family:'Share Tech Mono',monospace;font-size:11px">4h</span>
  </label>

  <button class="btn" id="scan-btn" onclick="startScan()">⬡ SCAN</button>
  <button class="btn btn-export" id="export-btn" onclick="exportCSV()" disabled>↓ CSV</button>
  <div id="last-updated" style="margin-left:auto;font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--text-dim)"></div>
</div>

<!-- PROGRESS -->
<div id="progress-wrap"><div id="progress-bar"></div></div>

<!-- STATUS -->
<div id="status-bar">
  <div class="status-dot" id="status-dot"></div>
  <span id="status-text">Siap. Tekan SCAN untuk memulai analisis.</span>
</div>

<!-- SUMMARY CARDS -->
<div id="summary" style="display:none;display:none">
  <div class="sum-card card-strong-buy"><div class="sum-val" id="cnt-strong-buy">0</div><div class="sum-lbl">STRONG BUY</div></div>
  <div class="sum-card card-buy"><div class="sum-val" id="cnt-buy">0</div><div class="sum-lbl">BUY</div></div>
  <div class="sum-card card-lean-buy"><div class="sum-val" id="cnt-lean-buy">0</div><div class="sum-lbl">LEAN BUY</div></div>
  <div class="sum-card card-mixed"><div class="sum-val" id="cnt-mixed">0</div><div class="sum-lbl">MIXED</div></div>
  <div class="sum-card card-lean-sell"><div class="sum-val" id="cnt-lean-sell">0</div><div class="sum-lbl">LEAN SELL</div></div>
  <div class="sum-card card-sell"><div class="sum-val" id="cnt-sell">0</div><div class="sum-lbl">SELL</div></div>
  <div class="sum-card card-strong-sell"><div class="sum-val" id="cnt-strong-sell">0</div><div class="sum-lbl">STRONG SELL</div></div>
</div>

<!-- FILTER BAR -->
<div class="filter-bar" id="filter-bar" style="display:none">
  <span style="font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--text-dim);letter-spacing:1px">FILTER:</span>
  <button class="filter-btn active" onclick="setFilter('all',this)">ALL</button>
  <button class="filter-btn fb-strong-buy" onclick="setFilter('strong-buy',this)">✅ STRONG BUY</button>
  <button class="filter-btn" onclick="setFilter('buy',this)">🟢 BUY</button>
  <button class="filter-btn" onclick="setFilter('lean-buy',this)">🟡 LEAN BUY</button>
  <button class="filter-btn" onclick="setFilter('mixed',this)">⚪ MIXED</button>
  <button class="filter-btn" onclick="setFilter('lean-sell',this)">🟠 LEAN SELL</button>
  <button class="filter-btn" onclick="setFilter('sell',this)">🔴 SELL</button>
  <button class="filter-btn fb-strong-sell" onclick="setFilter('strong-sell',this)">🔴 STRONG SELL</button>
</div>

<!-- TABLE -->
<div class="table-wrap">
  <div id="empty-state" class="empty-state">
    <div class="empty-icon">🔮</div>
    <p>TEKAN SCAN UNTUK MEMULAI ANALISIS</p>
    <p style="margin-top:8px;font-size:10px;opacity:.6">Binance Futures · USDT Perpetual · Real-time</p>
  </div>
  <table id="matrix-table" style="display:none">
    <thead id="table-head"></thead>
    <tbody id="table-body"></tbody>
  </table>
</div>

<script>
const BASE = "https://fapi.binance.com";

async function fetchJSON(url) {
  const PROXIES = [
    u => "https://corsproxy.io/?" + encodeURIComponent(u),
    u => "https://api.allorigins.win/raw?url=" + encodeURIComponent(u),
  ];
  // 1. Direct
  try {
    const r = await fetch(url, { signal: AbortSignal.timeout(8000) });
    if (r.ok) return await r.json();
  } catch (_) {}
  // 2. Proxy fallback
  for (const mkP of PROXIES) {
    try {
      const r = await fetch(mkP(url), { signal: AbortSignal.timeout(12000) });
      if (r.ok) return await r.json();
    } catch (_) {}
  }
  throw new Error("Semua koneksi gagal. Coba buka di Chrome desktop atau aktifkan VPN.");
}

async function fetchTopSymbols(n) {
  const data = await fetchJSON(BASE + "/fapi/v1/ticker/24hr");
  return data
    .filter(d => d.symbol.endsWith("USDT"))
    .sort((a, b) => parseFloat(b.quoteVolume) - parseFloat(a.quoteVolume))
    .slice(0, n)
    .map(d => ({ symbol: d.symbol, price: parseFloat(d.lastPrice) }));
}

async function fetchKlines(symbol, interval, limit) {
  limit = limit || 100;
  const url = BASE + "/fapi/v1/klines?symbol=" + symbol + "&interval=" + interval + "&limit=" + limit;
  const raw = await fetchJSON(url);
  if (!Array.isArray(raw)) throw new Error("Bad klines response");
  return raw.map(k => ({ o: +k[1], h: +k[2], l: +k[3], c: +k[4], v: +k[5] }));
}

async function analyzeSymbol(symbol, price, timeframes) {
  const row = { symbol, price, tfs: {} };
  const scores = [];

  for (const tf of timeframes) {
    try {
      const candles = await fetchKlines(symbol, tf, 100);
      const n = candles.length - 1;
      const vol    = calcVolWave(candles)[n];
      const trend  = calcTrendWave(candles)[n];
      const dom    = calcDomWave(candles)[n];
      const struct = calcStructWave(candles)[n];
      const { sig, score } = getSignal(vol, trend, dom, struct);
      row.tfs[tf] = { vol, trend, dom, struct, sig, score };
      scores.push(score);
    } catch {
      row.tfs[tf] = { vol: NaN, trend: NaN, dom: NaN, struct: NaN, sig: "ERR", score: null };
      scores.push(null);
    }
    await new Promise(r => setTimeout(r, 60)); // rate limit
  }

  row.overall = getOverall(scores.filter(s => s !== null));
  return row;
}

// ── UI Helpers ──────────────────────────────────────────────────

function fmtVal(v) {
  if (isNaN(v) || v === null) return '<span style="color:var(--text-dim)">—</span>';
  const cls = v > 60 ? 'val-pos-strong' : v > 20 ? 'val-pos' : v > 0 ? 'val-pos-weak'
            : v < -60 ? 'val-neg-strong' : v < -20 ? 'val-neg' : v < 0 ? 'val-neg-weak' : 'val-neutral';
  return `<span class="${cls}">${v.toFixed(1)}</span>`;
}

function sigClass(sig) {
  const s = sig.toUpperCase();
  if (s.includes("STRONG BUY"))  return 'sig-strong-buy';
  if (s.includes("STRONG SELL")) return 'sig-strong-sell';
  if (s.startsWith("BUY"))       return 'sig-buy';
  if (s.startsWith("SELL"))      return 'sig-sell';
  if (s.includes("WEAK BUY"))    return 'sig-weak-buy';
  if (s.includes("WEAK SELL"))   return 'sig-weak-sell';
  return 'sig-neutral';
}

function ovClass(ov) {
  if (ov.includes("STRONG BUY"))  return 'ov-strong-buy';
  if (ov.includes("STRONG SELL")) return 'ov-strong-sell';
  if (ov.includes("🟢"))          return 'ov-buy';
  if (ov.includes("🔴"))          return 'ov-sell';
  if (ov.includes("🟡"))          return 'ov-lean-buy';
  if (ov.includes("🟠"))          return 'ov-lean-sell';
  return 'ov-mixed';
}

function ovFilterKey(ov) {
  if (ov.includes("STRONG BUY"))  return 'strong-buy';
  if (ov.includes("STRONG SELL")) return 'strong-sell';
  if (ov.includes("🟢"))          return 'buy';
  if (ov.includes("🔴"))          return 'sell';
  if (ov.includes("🟡"))          return 'lean-buy';
  if (ov.includes("🟠"))          return 'lean-sell';
  return 'mixed';
}

function fmtPrice(p) {
  if (!p) return '—';
  return p >= 1000 ? p.toLocaleString('en-US', {maximumFractionDigits:2})
       : p >= 1    ? p.toFixed(4)
       : p.toFixed(6);
}

function buildHeader(timeframes) {
  const thead = document.getElementById('table-head');
  const tfCols = timeframes.length * 5;
  const totalCols = 3 + tfCols + 1;

  // TF group row
  let r1 = '<tr class="tf-group">';
  r1 += `<th colspan="3" class="th-meta" style="text-align:left;padding-left:10px">BINANCE USDT FUTURES</th>`;
  timeframes.forEach(tf => {
    r1 += `<th colspan="5">${tf.toUpperCase()}</th>`;
  });
  r1 += `<th class="th-meta">OVERALL</th></tr>`;

  // Col header row
  let r2 = '<tr class="col-head">';
  r2 += '<th>#</th><th style="text-align:left;padding-left:10px">SYMBOL</th><th>PRICE</th>';
  timeframes.forEach(() => {
    r2 += '<th>BANDAR</th><th>VELOC</th><th>DOM</th><th>STRUCT</th><th>SIGNAL</th>';
  });
  r2 += '<th>OVERALL SIGNAL</th></tr>';

  thead.innerHTML = r1 + r2;
}

function buildRow(row, idx, timeframes) {
  let html = `<tr data-filter="${ovFilterKey(row.overall)}">`;
  html += `<td class="td-num">${idx}</td>`;
  html += `<td class="td-symbol">${row.symbol.replace('USDT','')}<span style="color:var(--text-dim);font-size:9px">/USDT</span></td>`;
  html += `<td class="td-price">${fmtPrice(row.price)}</td>`;

  timeframes.forEach(tf => {
    const d = row.tfs[tf] || {};
    html += `<td class="td-val">${fmtVal(d.vol)}</td>`;
    html += `<td class="td-val">${fmtVal(d.trend)}</td>`;
    html += `<td class="td-val">${fmtVal(d.dom)}</td>`;
    html += `<td class="td-val">${fmtVal(d.struct)}</td>`;
    const sc = d.sig || '—';
    html += `<td class="td-sig"><span class="sig ${sigClass(sc)}">${sc}</span></td>`;
  });

  const ov = row.overall || '⚪ MIXED';
  html += `<td class="td-overall"><span class="sig ${ovClass(ov)}">${ov}</span></td>`;
  html += '</tr>';
  return html;
}

function updateSummary(rows) {
  const counts = { 'strong-buy':0, buy:0, 'lean-buy':0, mixed:0, 'lean-sell':0, sell:0, 'strong-sell':0 };
  rows.forEach(r => { const k = ovFilterKey(r.overall); if (counts[k] !== undefined) counts[k]++; });
  document.getElementById('cnt-strong-buy').textContent  = counts['strong-buy'];
  document.getElementById('cnt-buy').textContent         = counts['buy'];
  document.getElementById('cnt-lean-buy').textContent    = counts['lean-buy'];
  document.getElementById('cnt-mixed').textContent       = counts['mixed'];
  document.getElementById('cnt-lean-sell').textContent   = counts['lean-sell'];
  document.getElementById('cnt-sell').textContent        = counts['sell'];
  document.getElementById('cnt-strong-sell').textContent = counts['strong-sell'];
  document.getElementById('summary').style.display = 'flex';
}

function setFilter(key, btn) {
  activeFilter = key;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  applyFilter();
}

function applyFilter() {
  document.querySelectorAll('#table-body tr').forEach(tr => {
    if (activeFilter === 'all' || tr.dataset.filter === activeFilter) {
      tr.style.display = '';
    } else {
      tr.style.display = 'none';
    }
  });
}

// ── Main Scan ───────────────────────────────────────────────────

async function startScan() {
  const topN = parseInt(document.getElementById('top-n').value) || 50;
  const timeframes = ['15m','1h','4h'].filter(tf => document.getElementById(`tf-${tf}`).checked);
  if (!timeframes.length) { alert("Pilih minimal 1 timeframe!"); return; }

  const btn = document.getElementById('scan-btn');
  const dot = document.getElementById('status-dot');
  const statusTxt = document.getElementById('status-text');
  const progWrap = document.getElementById('progress-wrap');
  const progBar  = document.getElementById('progress-bar');

  btn.disabled = true;
  document.getElementById('export-btn').disabled = true;
  dot.className = 'status-dot active';
  progWrap.style.display = 'block';
  progBar.style.width = '0%';
  document.getElementById('empty-state').style.display = 'none';
  document.getElementById('matrix-table').style.display = 'table';
  document.getElementById('filter-bar').style.display = 'none';
  document.getElementById('summary').style.display = 'none';
  allRows = [];
  activeFilter = 'all';
  document.querySelectorAll('.filter-btn').forEach((b,i) => { if(i===0) b.classList.add('active'); else b.classList.remove('active'); });

  try {
    statusTxt.textContent = `Fetching top ${topN} symbols by volume...`;
    const symbols = await fetchTopSymbols(topN);
    document.getElementById('coin-count').textContent = symbols.length;

    buildHeader(timeframes);
    document.getElementById('table-body').innerHTML = '';

    for (let i = 0; i < symbols.length; i++) {
      const { symbol, price } = symbols[i];
      statusTxt.textContent = `[${i+1}/${symbols.length}] Analyzing ${symbol} (${timeframes.join(', ')})...`;
      progBar.style.width = `${Math.round((i / symbols.length) * 100)}%`;

      const row = await analyzeSymbol(symbol, price, timeframes);
      allRows.push(row);

      const tr = document.createElement('tr');
      tr.outerHTML; // dummy
      const tbody = document.getElementById('table-body');
      tbody.insertAdjacentHTML('beforeend', buildRow(row, i + 1, timeframes));
    }

    progBar.style.width = '100%';
    dot.className = 'status-dot done';
    const now = new Date().toLocaleTimeString('id-ID');
    statusTxt.textContent = `✓ Selesai! ${symbols.length} koin dianalisis pada ${now}`;
    document.querySelectorAll('.controls #last-updated').forEach(el => el.textContent = `Updated: ${now}`);

    updateSummary(allRows);
    document.getElementById('filter-bar').style.display = 'flex';
    document.getElementById('export-btn').disabled = false;

  } catch (err) {
    dot.className = 'status-dot error';
    statusTxt.textContent = `⚠ Error: ${err.message}`;
    console.error(err);
  }

  btn.disabled = false;
  setTimeout(() => { progWrap.style.display = 'none'; }, 1500);
}

// ── CSV Export ──────────────────────────────────────────────────

function exportCSV() {
  if (!allRows.length) return;
  const tfs = ['15m','1h','4h'].filter(tf => document.getElementById(`tf-${tf}`).checked);
  const headers = ['#','Symbol','Price'];
  tfs.forEach(tf => headers.push(`${tf}_Bandar`, `${tf}_Velocity`, `${tf}_Dominasi`, `${tf}_Struktur`, `${tf}_Signal`));
  headers.push('Overall');

  const lines = [headers.join(',')];
  allRows.forEach((r, i) => {
    const row = [i+1, r.symbol, r.price];
    tfs.forEach(tf => {
      const d = r.tfs[tf] || {};
      row.push(isNaN(d.vol) ? '' : d.vol.toFixed(2));
      row.push(isNaN(d.trend) ? '' : d.trend.toFixed(2));
      row.push(isNaN(d.dom) ? '' : d.dom.toFixed(2));
      row.push(isNaN(d.struct) ? '' : d.struct.toFixed(2));
      row.push(d.sig || '');
    });
    row.push(r.overall);
    lines.push(row.map(v => `"${v}"`).join(','));
  });

  const blob = new Blob([lines.join('\n')], { type: 'text/csv' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `Aulyeah_TrendMatrix_${new Date().toISOString().slice(0,16).replace('T','_')}.csv`;
  a.click();
}
</script>
</body>
</html>
