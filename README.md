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
input[type=number]{background:var(--bg);border:1px solid var(--border);color:var(--text);font-family:'Share Tech Mono',monospace;font-size:11px;padding:5px 8px;border-radius:4px;outline:none;width:65px}
input[type=number]:hover{border-color:var(--accent)}
.btn{background:linear-gradient(135deg,#0047a0,#0080d4);border:1px solid var(--accent);color:#fff;font-family:'Orbitron',monospace;font-size:10px;font-weight:700;letter-spacing:2px;padding:7px 16px;border-radius:4px;cursor:pointer;transition:all .2s;text-transform:uppercase}
.btn:hover{box-shadow:0 0 16px rgba(0,212,255,.4)}
.btn:disabled{opacity:.4;cursor:not-allowed}
.bcsv{background:linear-gradient(135deg,#1a3a00,#2e6000);border-color:var(--green);color:var(--green)}
.bcsv:hover{box-shadow:0 0 16px rgba(0,255,136,.3)}

/* Auto Refresh Switch Styles */
.auto-refresh-box { display: flex; align-items: center; gap: 8px; border-left: 1px solid var(--border); padding-left: 15px; margin-left: 5px; }
.switch { position: relative; display: inline-block; width: 34px; height: 18px; }
.switch input { opacity: 0; width: 0; height: 0; }
.slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #1a2a40; transition: .4s; border-radius: 18px; border: 1px solid var(--border); }
.slider:before { position: absolute; content: ""; height: 12px; width: 12px; left: 2px; bottom: 2px; background-color: var(--dim); transition: .4s; border-radius: 50%; }
input:checked + .slider { background-color: rgba(0, 212, 255, 0.2); border-color: var(--accent); }
input:checked + .slider:before { transform: translateX(16px); background-color: var(--accent); box-shadow: 0 0 8px var(--accent); }
#timer-display { font-family: 'Share Tech Mono', monospace; font-size: 10px; color: var(--gold); min-width: 40px; }

#pw{height:3px;background:var(--border);display:none}
#pb{height:100%;background:linear-gradient(90deg,#7b2fff,var(--accent));width:0%;transition:width .3s;box-shadow:0 0 10px var(--accent)}
#sb{padding:7px 20px;font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--dim);background:#070d18;border-bottom:1px solid var(--border);min-height:28px;display:flex;align-items:center;gap:10px}
.dot{width:7px;height:7px;border-radius:50%;background:var(--dim);flex-shrink:0;transition:background .3s}
.dot.on{background:var(--accent);box-shadow:0 0 8px var(--accent);animation:pulse 1s infinite}
.dot.ok{background:var(--green);box-shadow:0 0 8px var(--green);animation:none}
.dot.er{background:var(--red)}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}
#summary{display:none;padding:12px 20px;gap:10px;flex-wrap:wrap;border-bottom:1px solid var(--border);background:var(--panel)}
.sc{flex:1;min-width:90px;padding:8px 12px;border-radius:6px;border:1px solid var(--border);text-align:center}
.sv{font-family:'Orbitron',monospace;font-size:20px;font-weight:900;line-height:1}
.sl{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--dim);margin-top:3px;letter-spacing:1px}
.csb{background:#003322;border-color:#00ff88}.csb .sv{color:#00ff88;text-shadow:0 0 12px #00ff88}
.cb{background:#002214;border-color:#00cc66}.cb .sv{color:#00cc66}
.clb{background:#001a10;border-color:#33cc77}.clb .sv{color:#33cc77}
.cm{background:#101820;border-color:var(--muted)}.cm .sv{color:var(--dim)}
.cls{background:#1f1000;border-color:#ff9900}.cls .sv{color:#ff9900}
.cs{background:#1a0005;border-color:#ff3b5c}.cs .sv{color:#ff3b5c}
.css2{background:#220008;border-color:#ff0033}.css2 .sv{color:#ff0033;text-shadow:0 0 12px #ff0033}
.fbar{padding:8px 20px;display:none;gap:7px;flex-wrap:wrap;align-items:center;border-bottom:1px solid var(--border);background:#070d18}
.fb{font-family:'Share Tech Mono',monospace;font-size:9px;padding:3px 9px;border-radius:3px;border:1px solid var(--border);background:transparent;color:var(--dim);cursor:pointer;letter-spacing:1px;transition:all .15s}
.fb:hover,.fb.on{border-color:var(--accent);color:var(--accent)}
.tw{overflow-x:auto;padding:0 20px 40px}
table{width:100%;border-collapse:collapse;margin-top:14px;font-size:11px}
thead tr.tg th{font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:2px;text-transform:uppercase;color:var(--accent);padding:7px 5px 4px;border-bottom:1px solid var(--accent);background:#050a12}
thead tr.tg th.mt{color:var(--dim);border-bottom-color:var(--border)}
thead tr.ch th{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:1px;text-transform:uppercase;color:var(--dim);padding:5px;background:#070d18;border-bottom:2px solid var(--border);white-space:nowrap}
tbody tr{border-bottom:1px solid rgba(14,32,64,.6);transition:background .15s}
tbody tr:hover{background:rgba(0,212,255,.04)}
td{padding:6px 5px;text-align:center;white-space:nowrap;border-right:1px solid rgba(14,32,64,.4)}
.tn{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--dim);width:30px}
.ts2{font-family:'Share Tech Mono',monospace;font-size:11px;font-weight:700;color:var(--accent);text-align:left;padding-left:8px;width:120px}
.tv{font-family:'Share Tech Mono',monospace;font-size:9px;width:60px}
.tsi{font-family:'Rajdhani',sans-serif;font-size:10px;font-weight:700;width:110px}
.to{font-family:'Rajdhani',sans-serif;font-size:10px;font-weight:700;width:120px}
.tp{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--gold);width:80px}
.vps{color:#00ff88}.vp{color:#00cc66}.vpw{color:#66bb88}.vnw{color:#bb8866}.vn{color:#ff6680}.vns{color:#ff3b5c}.vz{color:var(--dim)}
.bdg{padding:2px 6px;border-radius:3px;display:inline-block}
.bsb{background:#004422;color:#00ff88;border:1px solid #00ff88}
.bb{background:#002a14;color:#00cc66;border:1px solid #00cc66}
.bwb{background:#001a0e;color:#66bb88;border:1px solid #448855}
.bn{background:#101820;color:#506070;border:1px solid #304050}
.bws{background:#1a0e00;color:#bb8833;border:1px solid #886622}
.bsl{background:#2a0008;color:#ff6680;border:1px solid #cc3355}
.bss{background:#220005;color:#ff3b5c;border:1px solid #ff3b5c}
.obsb{background:#003322;color:#00ff88;border:1px solid #00ff88;box-shadow:0 0 10px rgba(0,255,136,.3)}
.obb{background:#002214;color:#00cc66;border:1px solid #00cc66}
.oblb{background:#001a10;color:#44aa66;border:1px solid #33884f}
.obm{background:#111920;color:#405060;border:1px solid #253545}
.obls{background:#1a0e00;color:#cc7700;border:1px solid #aa6600}
.obs{background:#220006;color:#ff4466;border:1px solid #cc2244}
.obss{background:#1a0004;color:#ff2244;border:1px solid #ff2244;box-shadow:0 0 10px rgba(255,34,68,.3)}
.empty{text-align:center;padding:60px 20px;font-family:'Share Tech Mono',monospace;color:var(--dim)}
.ei{font-size:40px;margin-bottom:12px;opacity:.5}
::-webkit-scrollbar{width:5px;height:5px}
::-webkit-scrollbar-track{background:var(--bg)}
::-webkit-scrollbar-thumb{background:var(--muted);border-radius:3px}
</style>
</head>
<body>
<header>
  <div>
    <div class="logo">Aulyeah! <span>&#127302;</span> TREND MATRIX</div>
    <div style="font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--dim);margin-top:2px;letter-spacing:1px">BINANCE FUTURES &middot; USDT PERPETUAL &middot; REAL-TIME</div>
  </div>
  <div class="hmeta"><div><b id="coin-count">&#8212;</b> coins</div><div id="ts">&#8212;</div></div>
</header>
<div class="controls">
  <span class="cl">Top</span>
  <input type="number" id="top-n" value="50" min="10" max="200" step="10">
  <span class="cl">TF:</span>
  <label style="display:flex;align-items:center;gap:3px;cursor:pointer"><input type="checkbox" id="tf-15m" checked style="accent-color:var(--accent)"><span style="font-family:'Share Tech Mono',monospace;font-size:10px">15m</span></label>
  <label style="display:flex;align-items:center;gap:3px;cursor:pointer"><input type="checkbox" id="tf-1h" checked style="accent-color:var(--accent)"><span style="font-family:'Share Tech Mono',monospace;font-size:10px">1h</span></label>
  <label style="display:flex;align-items:center;gap:3px;cursor:pointer"><input type="checkbox" id="tf-4h" checked style="accent-color:var(--accent)"><span style="font-family:'Share Tech Mono',monospace;font-size:10px">4h</span></label>
  <button class="btn" id="scan-btn" onclick="startScan()">&#11041; SCAN</button>
  <button class="btn bcsv" id="csv-btn" onclick="exportCSV()" disabled>&#8595; CSV</button>
  
  <div class="auto-refresh-box">
    <span class="cl">AUTO:</span>
    <label class="switch">
      <input type="checkbox" id="auto-refresh-switch" onchange="toggleAutoRefresh()">
      <span class="slider"></span>
    </label>
    <span id="timer-display">OFF</span>
  </div>
</div>
<div id="pw"><div id="pb"></div></div>
<div id="sb"><div class="dot" id="dot"></div><span id="stxt">Siap. Tekan SCAN untuk mulai.</span></div>
<div id="summary">
  <div class="sc csb"><div class="sv" id="c-sb">0</div><div class="sl">STRONG BUY</div></div>
  <div class="sc cb"><div class="sv" id="c-b">0</div><div class="sl">BUY</div></div>
  <div class="sc clb"><div class="sv" id="c-lb">0</div><div class="sl">LEAN BUY</div></div>
  <div class="sc cm"><div class="sv" id="c-m">0</div><div class="sl">MIXED</div></div>
  <div class="sc cls"><div class="sv" id="c-ls">0</div><div class="sl">LEAN SELL</div></div>
  <div class="sc cs"><div class="sv" id="c-s">0</div><div class="sl">SELL</div></div>
  <div class="sc css2"><div class="sv" id="c-ss">0</div><div class="sl">STRONG SELL</div></div>
</div>
<div class="fbar" id="fbar">
  <span style="font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--dim)">FILTER:</span>
  <button class="fb on" onclick="setFilter('all',this)">ALL</button>
  <button class="fb" onclick="setFilter('strong-buy',this)">STRONG BUY</button>
  <button class="fb" onclick="setFilter('buy',this)">BUY</button>
  <button class="fb" onclick="setFilter('lean-buy',this)">LEAN BUY</button>
  <button class="fb" onclick="setFilter('mixed',this)">MIXED</button>
  <button class="fb" onclick="setFilter('lean-sell',this)">LEAN SELL</button>
  <button class="fb" onclick="setFilter('sell',this)">SELL</button>
  <button class="fb" onclick="setFilter('strong-sell',this)">STRONG SELL</button>
</div>
<div class="tw">
  <div id="empty" class="empty"><div class="ei">&#127302;</div><p>TEKAN SCAN UNTUK MULAI</p></div>
  <table id="tbl" style="display:none"><thead id="thead"></thead><tbody id="tbody"></tbody></table>
</div>

<script>
var BASE = "https://fapi.binance.com";
var PROXIES = [
  function(u){return "https://corsproxy.io/?" + encodeURIComponent(u);},
  function(u){return "https://api.allorigins.win/raw?url=" + encodeURIComponent(u);}
];
var allRows = [];
var activeFilter = "all";
var isScanning = false;

// Auto Refresh Variables
var refreshInterval = 15 * 60; // 15 menit dalam detik
var timeRemaining = refreshInterval;
var refreshTimer = null;

function toggleAutoRefresh() {
  const isChecked = document.getElementById('auto-refresh-switch').checked;
  const timerDisplay = document.getElementById('timer-display');
  
  if (isChecked) {
    timeRemaining = refreshInterval;
    updateTimerDisplay();
    if (!refreshTimer) {
      refreshTimer = setInterval(handleTimerTick, 1000);
    }
  } else {
    clearInterval(refreshTimer);
    refreshTimer = null;
    timerDisplay.textContent = "OFF";
  }
}

function handleTimerTick() {
  if (isScanning) return; // Jangan hitung mundur saat sedang scan
  
  timeRemaining--;
  updateTimerDisplay();
  
  if (timeRemaining <= 0) {
    timeRemaining = refreshInterval;
    startScan();
  }
}

function updateTimerDisplay() {
  const min = Math.floor(timeRemaining / 60);
  const sec = timeRemaining % 60;
  document.getElementById('timer-display').textContent = 
    (min < 10 ? "0" : "") + min + ":" + (sec < 10 ? "0" : "") + sec;
}

async function fetchJSON(url){
  try{
    var r = await fetch(url,{signal:AbortSignal.timeout(8000)});
    if(r.ok) return await r.json();
  }catch(e){}
  for(var i=0;i<PROXIES.length;i++){
    try{
      var r2 = await fetch(PROXIES[i](url),{signal:AbortSignal.timeout(12000)});
      if(r2.ok) return await r2.json();
    }catch(e){}
  }
  throw new Error("Koneksi ke Binance gagal. Cek koneksi internet atau aktifkan VPN.");
}

async function fetchSymbols(n){
  var data = await fetchJSON(BASE + "/fapi/v1/ticker/24hr");
  return data.filter(function(d){return d.symbol.slice(-4) === "USDT";})
    .sort(function(a,b){return parseFloat(b.quoteVolume)-parseFloat(a.quoteVolume);})
    .slice(0,n).map(function(d){return{symbol:d.symbol,price:parseFloat(d.lastPrice)};});
}

async function fetchKlines(symbol,interval,limit){
  var raw = await fetchJSON(BASE + "/fapi/v1/klines?symbol=" + symbol + "&interval=" + interval + "&limit=" + (limit||100));
  if(!Array.isArray(raw)) throw new Error("Bad response");
  return raw.map(function(k){return{o:+k[1],h:+k[2],l:+k[3],c:+k[4],v:+k[5]};});
}

function ema(arr,period){
  var k=2/(period+1),out=arr.map(function(){return NaN;}),start=-1;
  for(var i=0;i<arr.length;i++){if(!isNaN(arr[i])){start=i;break;}}
  if(start<0)return out;
  out[start]=arr[start];
  for(var i=start+1;i<arr.length;i++){out[i]=isNaN(out[i-1])?arr[i]:arr[i]*k+out[i-1]*(1-k);}
  return out;
}
function sma(arr,period){
  return arr.map(function(_,i){
    if(i<period-1)return NaN;
    var s=0;for(var j=i-period+1;j<=i;j++)s+=arr[j];return s/period;
  });
}
function wma(arr,period){
  return arr.map(function(_,i){
    if(i<period-1)return NaN;
    var s=0,w=0;for(var j=0;j<period;j++){s+=arr[i-period+1+j]*(j+1);w+=(j+1);}return s/w;
  });
}

function calcVol(c){
  var mint=1e-10;
  var mfv=c.map(function(x){return((x.c-x.l)-(x.h-x.c))/Math.max(x.h-x.l,mint)*x.v;});
  var sm=sma(mfv,20),sv=sma(c.map(function(x){return x.v;}),20).map(function(v){return Math.max(v||0,mint);});
  return ema(sm.map(function(m,i){return isNaN(m)?NaN:m/sv[i]*100;}),5);
}
function calcTrend(c){
  var mint=1e-10;
  var pc=c.map(function(x,i){return i===0?NaN:x.c-c[i-1].c;});
  var ds=ema(ema(pc,25),13);
  var da=ema(ema(pc.map(function(v){return isNaN(v)?NaN:Math.abs(v);}),25),13).map(function(v){return Math.max(v||0,mint);});
  return ds.map(function(v,i){return isNaN(v)?NaN:100*v/da[i];});
}
function calcDom(c){
  var delta=c.map(function(x,i){return i===0?NaN:x.c-c[i-1].c;});
  var g=sma(delta.map(function(v){return isNaN(v)?NaN:Math.max(v,0);}),14);
  var l=sma(delta.map(function(v){return isNaN(v)?NaN:Math.max(-v,0);}),14);
  var rsi=g.map(function(x,i){return(isNaN(x)||isNaN(l[i])||l[i]===0)?NaN:100-100/(1+x/l[i]);});
  return ema(rsi.map(function(r){return isNaN(r)?NaN:(r-50)*2;}),3);
}
function calcStruct(c){
  var mint=1e-10;
  var hh=c.map(function(_,i){
    if(i<19)return NaN;var mx=-Infinity;
    for(var j=i-19;j<=i;j++)if(c[j].h>mx)mx=c[j].h;return mx;
  });
  var ll=c.map(function(_,i){
    if(i<19)return NaN;var mn=Infinity;
    for(var j=i-19;j<=i;j++)if(c[j].l<mn)mn=c[j].l;return mn;
  });
  var raw=c.map(function(x,i){return isNaN(hh[i])?NaN:((x.c-ll[i])/Math.max(hh[i]-ll[i],mint))*200-100;});
  return wma(raw,8);
}

function getSignal(vol,trend,dom,struct){
  var vals=[vol,trend,dom,struct].filter(function(v){return!isNaN(v);});
  if(!vals.length)return{sig:"N/A",score:0};
  var avg=vals.reduce(function(a,b){return a+b;},0)/vals.length;
  var pos=vals.filter(function(v){return v>0;}).length;
  var neg=vals.filter(function(v){return v<0;}).length;
  if(vals.every(function(v){return v>80;}))return{sig:"STRONG BUY",score:4};
  if(vals.every(function(v){return v<-80;}))return{sig:"STRONG SELL",score:-4};
  if(vals.every(function(v){return v>40;}))return{sig:"BUY",score:3};
  if(vals.every(function(v){return v<-40;}))return{sig:"SELL",score:-3};
  if(avg>20&&pos>=3)return{sig:"BUY",score:2};
  if(avg<-20&&neg>=3)return{sig:"SELL",score:-2};
  if(avg>5)return{sig:"WEAK BUY",score:1};
  if(avg<-5)return{sig:"WEAK SELL",score:-1};
  return{sig:"NEUTRAL",score:0};
}

function getOverall(scores){
  var valid=scores.filter(function(s){return s!==null;});
  if(!valid.length)return "MIXED";
  var avg=valid.reduce(function(a,b){return a+b;},0)/valid.length;
  var buys=valid.filter(function(s){return s>0;}).length;
  var sells=valid.filter(function(s){return s<0;}).length;
  if(buys===valid.length&&avg>=2)return "STRONG BUY";
  if(sells===valid.length&&avg<=-2)return "STRONG SELL";
  if(buys===valid.length)return "BUY";
  if(sells===valid.length)return "SELL";
  if(buys>=2 && avg > 0)return "LEAN BUY";
  if(sells>=2 && avg < 0)return "LEAN SELL";
  return "MIXED";
}

async function analyzeSymbol(symbol,price,tfs){
  var row={symbol:symbol,price:price,tfs:{}};
  var scores=[];
  for(var t=0;t<tfs.length;t++){
    var tf=tfs[t];
    try{
      var c=await fetchKlines(symbol,tf,100);
      var n=c.length-1;
      var vol=calcVol(c)[n],trend=calcTrend(c)[n],dom=calcDom(c)[n],struct=calcStruct(c)[n];
      var res=getSignal(vol,trend,dom,struct);
      row.tfs[tf]={vol:vol,trend:trend,dom:dom,struct:struct,sig:res.sig,score:res.score};
      scores.push(res.score);
    }catch(e){
      row.tfs[tf]={vol:NaN,trend:NaN,dom:NaN,struct:NaN,sig:"ERR",score:null};
      scores.push(null);
    }
    await new Promise(function(r){setTimeout(r,50);});
  }
  row.overall=getOverall(scores);
  return row;
}

function fmtV(v){
  if(isNaN(v)||v===null||v===undefined)return '<span class="vz">—</span>';
  var c=v>60?'vps':v>20?'vp':v>0?'vpw':v<-60?'vns':v<-20?'vn':v<0?'vnw':'vz';
  return '<span class="'+c+'">'+v.toFixed(1)+'</span>';
}
function fmtP(p){
  if(!p)return '—';
  return p>=1000?p.toLocaleString('en-US',{maximumFractionDigits:2}):p>=1?p.toFixed(4):p.toFixed(6);
}
function sigBC(s){
  if(s==='STRONG BUY')return 'bsb';if(s==='STRONG SELL')return 'bss';
  if(s==='BUY')return 'bb';if(s==='SELL')return 'bsl';
  if(s==='WEAK BUY')return 'bwb';if(s==='WEAK SELL')return 'bws';return 'bn';
}
function ovBC(s){
  if(s==='STRONG BUY')return 'obsb';if(s==='STRONG SELL')return 'obss';
  if(s==='BUY')return 'obb';if(s==='SELL')return 'obs';
  if(s==='LEAN BUY')return 'oblb';if(s==='LEAN SELL')return 'obls';return 'obm';
}
function ovFK(s){
  if(s==='STRONG BUY')return 'strong-buy';if(s==='STRONG SELL')return 'strong-sell';
  if(s==='BUY')return 'buy';if(s==='SELL')return 'sell';
  if(s==='LEAN BUY')return 'lean-buy';if(s==='LEAN SELL')return 'lean-sell';return 'mixed';
}
function sigLabel(s){
  var m={'STRONG BUY':'STRONG BUY ⬆⬆','BUY':'BUY ⬆','WEAK BUY':'WEAK BUY →',
  'STRONG SELL':'STRONG SELL ⬇⬇','SELL':'SELL ⬇','WEAK SELL':'WEAK SELL →','NEUTRAL':'NEUTRAL'};
  return m[s]||s||'—';
}
function ovLabel(s){
  var m={'STRONG BUY':'✅ STRONG BUY','BUY':'🟢 BUY','LEAN BUY':'🟡 LEAN BUY',
  'MIXED':'⚪ MIXED','LEAN SELL':'🟠 LEAN SELL','SELL':'🔴 SELL','STRONG SELL':'🔴 STRONG SELL'};
  return m[s]||s||'—';
}

function buildHeader(tfs){
  var r1='<tr class="tg"><th colspan="3" class="mt" style="text-align:left;padding-left:8px">BINANCE USDT FUTURES</th>';
  tfs.forEach(function(tf){r1+='<th colspan="5">⎯⎯ '+tf.toUpperCase()+' ⎯⎯</th>';});
  r1+='<th class="mt">OVERALL</th></tr>';
  var r2='<tr class="ch"><th>#</th><th style="text-align:left;padding-left:8px">SYMBOL</th><th>PRICE</th>';
  tfs.forEach(function(){r2+='<th>BANDAR</th><th>VELOC</th><th>DOM</th><th>STRUCT</th><th>SIGNAL</th>';});
  r2+='<th>OVERALL</th></tr>';
  document.getElementById('thead').innerHTML=r1+r2;
}

function buildRow(row,idx,tfs){
  var fk=ovFK(row.overall);
  var h='<tr data-filter="'+fk+'">';
  h+='<td class="tn">'+idx+'</td>';
  h+='<td class="ts2">'+row.symbol.replace('USDT','')+'<span style="color:var(--dim);font-size:8px">/USDT</span></td>';
  h+='<td class="tp">'+fmtP(row.price)+'</td>';
  tfs.forEach(function(tf){
    var d=row.tfs[tf]||{};
    h+='<td class="tv">'+fmtV(d.vol)+'</td><td class="tv">'+fmtV(d.trend)+'</td>';
    h+='<td class="tv">'+fmtV(d.dom)+'</td><td class="tv">'+fmtV(d.struct)+'</td>';
    var sc=d.sig||'N/A';
    h+='<td class="tsi"><span class="bdg '+sigBC(sc)+'">'+sigLabel(sc)+'</span></td>';
  });
  var ov=row.overall||'MIXED';
  h+='<td class="to"><span class="bdg '+ovBC(ov)+'">'+ovLabel(ov)+'</span></td></tr>';
  return h;
}

function updateSummary(rows){
  var c={'strong-buy':0,'buy':0,'lean-buy':0,'mixed':0,'lean-sell':0,'sell':0,'strong-sell':0};
  rows.forEach(function(r){var k=ovFK(r.overall);if(c[k]!==undefined)c[k]++;});
  document.getElementById('c-sb').textContent=c['strong-buy'];
  document.getElementById('c-b').textContent=c['buy'];
  document.getElementById('c-lb').textContent=c['lean-buy'];
  document.getElementById('c-m').textContent=c['mixed'];
  document.getElementById('c-ls').textContent=c['lean-sell'];
  document.getElementById('c-s').textContent=c['sell'];
  document.getElementById('c-ss').textContent=c['strong-sell'];
  document.getElementById('summary').style.display='flex';
}

function setFilter(key,btn){
  activeFilter=key;
  document.querySelectorAll('.fb').forEach(function(b){b.classList.remove('on');});
  btn.classList.add('on');
  document.querySelectorAll('#tbody tr').forEach(function(tr){
    tr.style.display=(activeFilter==='all'||tr.dataset.filter===activeFilter)?'':'none';
  });
}

async function startScan(){
  if(isScanning) return;
  isScanning = true;
  
  var topN=parseInt(document.getElementById('top-n').value)||50;
  var tfs=['15m','1h','4h'].filter(function(tf){return document.getElementById('tf-'+tf).checked;});
  if(!tfs.length){alert("Pilih minimal 1 timeframe!"); isScanning=false; return;}
  
  var btn=document.getElementById('scan-btn'),dot=document.getElementById('dot');
  var stxt=document.getElementById('stxt'),pw=document.getElementById('pw'),pb=document.getElementById('pb');
  
  btn.disabled=true;
  document.getElementById('csv-btn').disabled=true;
  dot.className='dot on'; pw.style.display='block'; pb.style.width='0%';
  document.getElementById('empty').style.display='none';
  document.getElementById('tbl').style.display='table';
  document.getElementById('tbody').innerHTML='';
  
  allRows=[];
  
  try{
    stxt.textContent='Fetching symbols...';
    var symbols=await fetchSymbols(topN);
    document.getElementById('coin-count').textContent=symbols.length;
    buildHeader(tfs);
    
    for(var i=0;i<symbols.length;i++){
      var sym=symbols[i];
      stxt.textContent='[' +(i+1)+'/'+symbols.length+'] '+sym.symbol;
      pb.style.width=Math.round(i/symbols.length*100)+'%';
      var row=await analyzeSymbol(sym.symbol,sym.price,tfs);
      allRows.push(row);
      document.getElementById('tbody').insertAdjacentHTML('beforeend',buildRow(row,i+1,tfs));
    }
    
    pb.style.width='100%'; dot.className='dot ok';
    var now=new Date().toLocaleTimeString('id-ID');
    stxt.textContent='Selesai pkl '+now;
    document.getElementById('ts').textContent='Updated: '+now;
    updateSummary(allRows);
    document.getElementById('fbar').style.display='flex';
    document.getElementById('csv-btn').disabled=false;
    
    // Reset timer auto refresh setelah scan selesai
    timeRemaining = refreshInterval;
    
  }catch(err){
    dot.className='dot er'; stxt.textContent='Error: '+err.message;
  }
  
  btn.disabled=false;
  isScanning = false;
  setTimeout(function(){pw.style.display='none';},1500);
}

function exportCSV(){
  if(!allRows.length)return;
  var tfs=['15m','1h','4h'].filter(function(tf){return document.getElementById('tf-'+tf).checked;});
  var h=['#','Symbol','Price'];
  tfs.forEach(function(tf){h.push(tf+'_Bandar',tf+'_Veloc',tf+'_Dom',tf+'_Struct',tf+'_Signal');});
  h.push('Overall');
  var lines=[h.join(',')];
  allRows.forEach(function(r,i){
    var row=[i+1,r.symbol,r.price];
    tfs.forEach(function(tf){
      var d=r.tfs[tf]||{};
      row.push(isNaN(d.vol)?'':d.vol.toFixed(2));
      row.push(isNaN(d.trend)?'':d.trend.toFixed(2));
      row.push(isNaN(d.dom)?'':d.dom.toFixed(2));
      row.push(isNaN(d.struct)?'':d.struct.toFixed(2));
      row.push(d.sig||'');
    });
    row.push(r.overall);
    lines.push(row.map(function(v){return '"'+v+'"';}).join(','));
  });
  var blob=new Blob([lines.join('\n')],{type:'text/csv'});
  var a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download='TrendMatrix_'+new Date().toISOString().slice(0,16)+'.csv';
  a.click();
}
</script>
</body>
</html>
