<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="InvestTracker">
<title>InvestTracker</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
body{font-family:-apple-system,BlinkMacSystemFont,'SF Pro Text',sans-serif;background:#f2f2f7;color:#1c1c1e;max-width:430px;margin:0 auto;min-height:100vh}
:root{--blue:#007aff;--green:#34c759;--red:#ff3b30;--gray:#8e8e93;--card:#fff;--border:#e5e5ea;--purple:#af52de;--orange:#ff9500;--teal:#5ac8fa;--indigo:#5856d6}
.header{background:rgba(255,255,255,0.95);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);padding:52px 20px 10px;position:sticky;top:0;z-index:100;border-bottom:1px solid var(--border)}
.header h1{font-size:24px;font-weight:700}
.sub{font-size:11px;color:var(--gray);margin-top:1px}
.tabs{display:flex;background:#e5e5ea;border-radius:10px;padding:2px;margin:10px 0 0}
.tab{flex:1;padding:7px 0;border:none;background:transparent;border-radius:8px;font-size:12px;font-weight:500;color:var(--gray);cursor:pointer;transition:all .2s}
.tab.active{background:#fff;color:#1c1c1e;box-shadow:0 1px 4px rgba(0,0,0,.12)}
.search-bar{padding:10px 20px 4px;position:relative}
.search-bar input{width:100%;padding:9px 36px 9px 14px;border:1.5px solid var(--border);border-radius:12px;font-size:14px;background:#fff;outline:none}
.search-bar input:focus{border-color:var(--blue)}
.search-bar .sclear{position:absolute;right:30px;top:50%;transform:translateY(-50%);color:var(--gray);font-size:18px;cursor:pointer;padding:2px 4px;border:none;background:transparent;line-height:1}
.yr-banner{margin:8px 20px 0;background:linear-gradient(135deg,#fff7ed,#fef3c7);border:1.5px solid #fcd34d;border-radius:14px;padding:12px 14px;display:flex;gap:10px;align-items:flex-start}
.yr-banner .yb-ico{font-size:22px;flex-shrink:0}
.yr-banner .yb-txt{font-size:12px;color:#92400e;line-height:1.5}
.yr-banner .yb-txt b{color:#78350f}
.summary{display:flex;gap:9px;padding:13px 20px;overflow-x:auto;scrollbar-width:none}
.summary::-webkit-scrollbar{display:none}
.scard{min-width:108px;background:var(--card);border-radius:14px;padding:11px 12px;box-shadow:0 1px 3px rgba(0,0,0,.08);flex-shrink:0}
.scard .lbl{font-size:10px;color:var(--gray);font-weight:600;text-transform:uppercase;letter-spacing:.4px}
.scard .val{font-size:16px;font-weight:700;margin-top:3px}
.val.pos{color:var(--green)}.val.neg{color:var(--red)}

/* ═══════════════════════════════════════════════════════
   CATEGORY BREAKDOWN PANEL  — new 2-row-per-category layout
   ═══════════════════════════════════════════════════════ */
.cat-panel{margin:0 20px 12px;background:var(--card);border-radius:16px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08)}
.cat-panel-hdr{display:flex;justify-content:space-between;align-items:center;padding:12px 14px;background:#f9f9f9;border-bottom:1px solid var(--border);cursor:pointer}
.cat-panel-hdr-left{display:flex;align-items:center;gap:7px;font-size:13px;font-weight:700;color:#1c1c1e}
.cat-chevron{font-size:11px;color:var(--gray);transition:transform .25s}
.cat-chevron.open{transform:rotate(180deg)}

/* Column header — 3 value columns */
.cat-col-hdr{display:grid;grid-template-columns:1.7fr 1fr 1fr 1fr;gap:0;padding:7px 14px 6px;background:#f2f2f7;border-bottom:1px solid var(--border)}
.cat-col-hdr span{font-size:9px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px;text-align:right}
.cat-col-hdr span:first-child{text-align:left}

/* Main value row per category */
.cat-val-row{display:grid;grid-template-columns:1.7fr 1fr 1fr 1fr;gap:0;padding:10px 14px 6px;border-bottom:none;align-items:center}
.cat-row-label{display:flex;align-items:flex-start;gap:7px}
.cat-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0;margin-top:3px}
.cat-row-name{font-size:12px;font-weight:700}
.cat-row-sub{font-size:10px;color:var(--gray);margin-top:1px}
.cat-bar-wrap{height:3px;background:#e9e9e9;border-radius:2px;margin-top:5px;width:100%}
.cat-bar{height:3px;border-radius:2px}
.cat-cell{text-align:right}
.cat-cell .cv{font-size:12px;font-weight:700;color:#1c1c1e}
.cat-cell .cs{font-size:10px;color:var(--gray);margin-top:1px}
.cv.pos{color:var(--green)}.cv.neg{color:var(--red)}

/* Dividend sub-row — full width, green tinted */
.cat-div-row{display:grid;grid-template-columns:1fr 1fr 1fr;gap:0;padding:7px 14px 11px 32px;background:linear-gradient(to right,#f6fef9,#f0fdf4);border-bottom:1px solid var(--border)}
.cat-div-row:last-of-type{border-bottom:none}
.cat-div-cell{text-align:center;padding:5px 4px;position:relative}
.cat-div-cell:not(:last-child)::after{content:'';position:absolute;right:0;top:20%;height:60%;width:1px;background:#d1fae5}
.cdl{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.3px;color:#6b7280;line-height:1.3}
.cdv{font-size:12px;font-weight:700;margin-top:3px}
.cdv.bf{color:#0369a1}.cdv.cy{color:#15803d}.cdv.tot{color:var(--purple)}
.cds{font-size:9px;color:#6b7280;margin-top:1px}

/* Totals row */
.cat-total-row{display:grid;grid-template-columns:1.7fr 1fr 1fr 1fr;gap:0;padding:9px 14px;background:#f0f7ff;border-top:2px solid #bae6fd;align-items:center}
.cat-total-div{display:grid;grid-template-columns:1fr 1fr 1fr;gap:0;padding:7px 14px 9px 32px;background:#f5f0ff;border-top:1px solid #e9d5ff}
.cat-total-div .cat-div-cell:not(:last-child)::after{background:#e9d5ff}
/* ═══════════════════════════════════════════════════════ */

.filter-bar{display:flex;gap:7px;padding:0 20px 11px;overflow-x:auto;scrollbar-width:none}
.filter-bar::-webkit-scrollbar{display:none}
.chip{padding:5px 13px;border-radius:20px;border:1.5px solid var(--border);background:#fff;font-size:12px;font-weight:600;cursor:pointer;white-space:nowrap;color:#3a3a3c}
.chip.on{background:var(--blue);color:#fff;border-color:var(--blue)}
.section{padding:0 20px 100px}
.sec-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
.sec-ttl{font-size:19px;font-weight:700}
.btn{background:var(--blue);color:#fff;border:none;border-radius:10px;padding:8px 16px;font-size:14px;font-weight:600;cursor:pointer;transition:opacity .2s}
.btn:active{opacity:.75}
.btn.sm{padding:5px 12px;font-size:12px;border-radius:8px}
.btn.xs{padding:3px 8px;font-size:11px;border-radius:6px}
.btn.out{background:transparent;color:var(--blue);border:1.5px solid var(--blue)}
.btn.red{background:var(--red)}.btn.grn{background:var(--green)}.btn.amber{background:var(--orange)}
.btn.raf{background:linear-gradient(135deg,#007aff,#5856d6);display:flex;align-items:center;gap:5px;font-size:13px;padding:8px 14px}
.btn:disabled{opacity:.45;cursor:not-allowed}
.card{background:var(--card);border-radius:16px;padding:15px;margin-bottom:12px;box-shadow:0 1px 3px rgba(0,0,0,.08)}
.inv-top{display:flex;justify-content:space-between;align-items:flex-start;gap:8px}
.inv-meta{flex:1;min-width:0}
.inv-name{font-size:15px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.inv-tkr{font-size:11px;color:var(--gray);margin-top:1px}
.badges{display:flex;gap:4px;flex-wrap:wrap;margin-top:5px}
.badge{font-size:10px;font-weight:600;padding:2px 7px;border-radius:20px;background:#e5e5ea;color:#3a3a3c}
.badge.usd{background:#dbeafe;color:#1d4ed8}.badge.sgd{background:#dcfce7;color:#15803d}
.badge.gbp{background:#f3e8ff;color:#7e22ce}.badge.eur{background:#fff7ed;color:#c2410c}
.badge.hkd{background:#fef9c3;color:#a16207}
.badge.cash{background:#dcfce7;color:#166534}.badge.srs{background:#fef9c3;color:#854d0e}.badge.cpf{background:#fce7f3;color:#9d174d}
.price-box{text-align:right;min-width:120px;flex-shrink:0}
.price-lbl{font-size:10px;color:var(--gray);margin-bottom:3px;display:flex;align-items:center;justify-content:flex-end;gap:4px}
.price-big{font-size:19px;font-weight:700}
.price-chg{font-size:11px;font-weight:600;margin-top:1px}
.price-ts{font-size:9px;color:var(--gray);margin-top:1px}
.price-resolved{font-size:9px;color:var(--blue);margin-top:1px;font-weight:600}
.abadge{font-size:9px;background:#dcfce7;color:#15803d;padding:1px 5px;border-radius:6px;font-weight:700}
.ibadge{font-size:9px;background:#dbeafe;color:#1d4ed8;padding:1px 5px;border-radius:6px;font-weight:700}
.mbadge{font-size:9px;background:#f2f2f7;color:var(--gray);padding:1px 5px;border-radius:6px;font-weight:700}
.fbadge{font-size:9px;background:#fce7f3;color:#9d174d;padding:1px 5px;border-radius:6px;font-weight:700}
.spin{display:inline-block;width:11px;height:11px;border:2px solid #e5e5ea;border-top-color:var(--blue);border-radius:50%;animation:spin .7s linear infinite;vertical-align:middle}
@keyframes spin{to{transform:rotate(360deg)}}
.manual-row{display:flex;align-items:center;gap:4px;justify-content:flex-end;margin-top:5px}
.spot-inp{width:88px;border:1.5px solid var(--border);border-radius:8px;padding:5px 8px;font-size:13px;text-align:right;background:#f9f9f9}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:7px;margin-top:11px}
.stat{background:#f2f2f7;border-radius:10px;padding:9px 10px}
.stat .sl{font-size:9px;color:var(--gray);font-weight:600;text-transform:uppercase;letter-spacing:.3px}
.stat .sv{font-size:13px;font-weight:700;margin-top:2px}
.div-strip{background:linear-gradient(135deg,#f0fdf4,#f0f9ff);border:1px solid #d1fae5;border-radius:12px;padding:11px 12px;margin-top:10px}
.div-strip-title{font-size:10px;font-weight:700;color:#15803d;text-transform:uppercase;letter-spacing:.4px;margin-bottom:8px}
.div-strip-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:0}
.div-col{text-align:center;padding:4px 2px}
.div-col:not(:last-child){border-right:1px solid #d1fae5}
.div-col .dc-lbl{font-size:9px;color:#6b7280;font-weight:600;text-transform:uppercase;letter-spacing:.3px;line-height:1.3}
.div-col .dc-val{font-size:13px;font-weight:700;color:#15803d;margin-top:3px}
.div-col .dc-sub{font-size:9px;color:#6b7280;margin-top:1px}
.yield-row{display:flex;justify-content:space-between;align-items:center;margin-top:8px;padding-top:8px;border-top:1px solid #d1fae5}
.yield-item{text-align:center;flex:1}
.yield-item .yl{font-size:9px;color:#6b7280;font-weight:600;text-transform:uppercase;letter-spacing:.3px}
.yield-item .yv{font-size:12px;font-weight:700;margin-top:2px}
.actions{display:flex;gap:7px;margin-top:11px;flex-wrap:wrap}
.tx-section{margin-top:12px}
.tx-hdr-row{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.tx-hdr{font-size:11px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px}
.tx-item{display:flex;justify-content:space-between;align-items:center;padding:8px 10px;border-bottom:1px solid var(--border);font-size:12px;gap:6px}
.tx-item:last-child{border-bottom:none}
.tx-item:first-child{border-top:1px solid var(--border)}
.tx-left{flex:1;min-width:0}
.tx-t{font-weight:700}
.tx-t.buy{color:var(--blue)}.tx-t.sell{color:var(--red)}.tx-t.rights{color:var(--orange)}.tx-t.free{color:var(--green)}
.tx-right{text-align:right;flex-shrink:0}
.tx-btns{display:flex;gap:4px;flex-shrink:0}
.src-info{background:#f0f9ff;border:1px solid #bae6fd;border-radius:9px;padding:7px 10px;margin-top:8px;font-size:11px;color:#0369a1;display:flex;align-items:flex-start;gap:6px;line-height:1.4}
.overlay{position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:200;display:flex;align-items:flex-end;justify-content:center}
.modal{background:#fff;border-radius:22px 22px 0 0;padding:22px 20px 44px;width:100%;max-width:430px;max-height:92vh;overflow-y:auto}
.modal h2{font-size:19px;font-weight:700;margin-bottom:4px}
.modal .modal-sub{font-size:12px;color:var(--gray);margin-bottom:16px}
.fg{margin-bottom:12px;position:relative}
.fg label{font-size:12px;font-weight:600;color:#444;display:block;margin-bottom:4px}
.fg input,.fg select{width:100%;padding:11px 13px;border:1.5px solid var(--border);border-radius:11px;font-size:15px;background:#fff;-webkit-appearance:none;appearance:none}
.fg input:focus,.fg select:focus{outline:none;border-color:var(--blue)}
.fg .hint{font-size:11px;color:var(--gray);margin-top:3px}
.row2{display:grid;grid-template-columns:1fr 1fr;gap:9px}
.m-actions{display:flex;gap:9px;margin-top:18px}
.m-actions .btn{flex:1;padding:13px}
.ac-drop{position:absolute;left:0;right:0;top:calc(100% - 2px);background:#fff;border:1.5px solid var(--blue);border-top:none;border-radius:0 0 11px 11px;z-index:50;max-height:210px;overflow-y:auto;box-shadow:0 8px 20px rgba(0,0,0,.12)}
.ac-item{padding:10px 13px;font-size:13px;cursor:pointer;border-bottom:1px solid var(--border);display:flex;flex-direction:column;gap:2px}
.ac-item:last-child{border-bottom:none}
.ac-item:active{background:#f0f7ff}
.ac-sym{font-weight:700;color:var(--blue);font-size:12px}
.ac-name{color:#1c1c1e;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.ac-ex{font-size:10px;color:var(--gray)}
.ac-loading{padding:14px;text-align:center;color:var(--gray);font-size:13px}
.div-yr-block{background:var(--card);border-radius:14px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:6px}
.div-yr-hdr{display:grid;grid-template-columns:1.4fr 0.8fr 1fr 0.8fr 60px;gap:4px;padding:8px 12px;background:#f2f2f7;font-size:10px;font-weight:700;color:var(--gray);text-transform:uppercase}
.div-yr-row{display:grid;grid-template-columns:1.4fr 0.8fr 1fr 0.8fr 60px;gap:4px;padding:10px 12px;border-top:1px solid var(--border);font-size:12px;align-items:center}
.ledger-inv{margin-bottom:18px}
.ledger-inv-hdr{font-size:14px;font-weight:700;display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.ledger-summary{display:grid;grid-template-columns:1fr 1fr 1fr;gap:7px;margin-bottom:8px}
.ls-box{background:#f0fdf4;border:1px solid #d1fae5;border-radius:10px;padding:8px 10px;text-align:center}
.ls-box.bf{background:#f0f9ff;border-color:#bae6fd}
.ls-box.tot{background:#fdf4ff;border-color:#e9d5ff}
.ls-lbl{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.3px;color:#6b7280}
.ls-val{font-size:13px;font-weight:700;margin-top:3px}
.ls-box.bf .ls-val{color:#0369a1}
.ls-box .ls-val{color:#15803d}
.ls-box.tot .ls-val{color:#7e22ce}
.sumpage{padding:0 20px 100px}
.sum-section{margin-bottom:20px}
.sum-section-title{font-size:16px;font-weight:700;margin-bottom:10px;display:flex;align-items:center;gap:6px}
.pie-wrap{background:var(--card);border-radius:16px;padding:18px;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:12px}
.pie-title{font-size:14px;font-weight:700;margin-bottom:14px}
.pie-body{display:flex;align-items:center;gap:16px}
.pie-svg{flex-shrink:0}
.pie-legend{flex:1;display:flex;flex-direction:column;gap:7px}
.pie-leg-item{display:flex;align-items:center;gap:7px}
.pie-dot{width:11px;height:11px;border-radius:50%;flex-shrink:0}
.pie-leg-label{font-size:12px;font-weight:500;color:#1c1c1e;flex:1}
.pie-leg-pct{font-size:10px;color:var(--gray);min-width:36px;text-align:right}
.breakdown-card{background:var(--card);border-radius:16px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:12px}
.bk-hdr{display:grid;gap:4px;padding:9px 14px;background:#f2f2f7;font-size:10px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px}
.bk-row{display:grid;gap:4px;padding:12px 14px;border-top:1px solid var(--border);font-size:13px;align-items:center}
.bk-label{font-weight:700;display:flex;align-items:center;gap:7px}
.bk-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0}
.prof-winner{background:#f0fdf4}.prof-loser{background:#fff5f5}
.holding-row{display:flex;justify-content:space-between;align-items:center;padding:10px 14px;border-top:1px solid var(--border)}
.holding-row:first-child{border-top:none}
.holding-bar-bg{height:4px;background:#e5e5ea;border-radius:2px;margin-top:4px}
.holding-bar-fill{height:4px;border-radius:2px}
.empty{text-align:center;padding:40px 20px;color:var(--gray)}
.empty .ico{font-size:38px;margin-bottom:8px}
.no-results{text-align:center;padding:30px 20px;color:var(--gray);font-size:14px}
.toast{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);background:#1c1c1e;color:#fff;padding:10px 20px;border-radius:20px;font-size:13px;font-weight:500;z-index:400;opacity:0;transition:opacity .3s;pointer-events:none;white-space:nowrap;max-width:90vw;text-align:center}
.toast.show{opacity:1}
</style>
</head>
<body>
<div id="app"></div>
<div class="toast" id="toast"></div>
<script>
'use strict';
const NOW=new Date(),CY=NOW.getFullYear().toString(),CMONTH=NOW.getMonth(),NY=(NOW.getFullYear()+1).toString();
const TYPE_COLORS={ETF:'#007aff',Stock:'#34c759','Unit Trust':'#ff9500',Bond:'#af52de',Other:'#8e8e93'};
const TYPE_FG={ETF:'#1d4ed8',Stock:'#166534','Unit Trust':'#c2410c',Bond:'#7e22ce',Other:'#6b7280'};
const ACC_COLORS={Cash:'#34c759',SRS:'#ff9f0a',CPF:'#ff2d55'};
const PROF_COLORS={Profit:'#34c759',Loss:'#ff3b30','Break-even':'#8e8e93'};
const PROXY='https://corsproxy.io/?';

// ─── Price Engine ─────────────────────────────────────────────────────────
async function yfFetch(p){const r=await fetch(PROXY+encodeURIComponent('https://query1.finance.yahoo.com'+p),{headers:{'x-requested-with':'XMLHttpRequest'}});if(!r.ok)throw new Error('HTTP '+r.status);return r.json();}
async function yf2Fetch(p){const r=await fetch(PROXY+encodeURIComponent('https://query2.finance.yahoo.com'+p),{headers:{'x-requested-with':'XMLHttpRequest'}});if(!r.ok)throw new Error('HTTP '+r.status);return r.json();}
async function fetchPriceDirect(sym){const j=await yfFetch(`/v8/finance/chart/${encodeURIComponent(sym)}?interval=1d&range=1d`);const m=j?.chart?.result?.[0]?.meta;if(!m)throw new Error('No meta');const p=m.regularMarketPrice||m.previousClose;if(!p)throw new Error('No price');const pv=m.chartPreviousClose||m.previousClose||p;return{price:p,change:p-pv,changePct:pv>0?((p-pv)/pv)*100:0,currency:m.currency||''};}
async function yfSearch(q){try{const j=await yf2Fetch(`/v1/finance/search?q=${encodeURIComponent(q)}&quotesCount=8&newsCount=0&enableFuzzyQuery=false&enableCb=false`);return(j?.quotes||[]).filter(x=>x.symbol&&!['FUTURE','OPTION','CURRENCY','CRYPTOCURRENCY'].includes(x.quoteType));}catch(e){return[];}}
async function openFIGILookup(isin){try{const res=await fetch('https://api.openfigi.com/v3/mapping',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify([{"idType":"ID_ISIN","idValue":isin}])});if(!res.ok)return[];const data=await res.json();return data?.[0]?.data||[];}catch(e){return[];}}
function exchSuffix(ec){const M={'SG':'.SI','LN':'.L','HK':'.HK','GY':'.DE','FP':'.PA','SM':'.MC','IM':'.MI','AV':'.VI','NA':'.AS','BB':'.BR','SW':'.SW','AU':'.AX','JP':'.T','KS':'.KS','TW':'.TW','US':'','UN':'','UA':'','UW':'','UR':'','UT':'','UP':''};return Object.prototype.hasOwnProperty.call(M,ec)?M[ec]:undefined;}
async function resolvePriceSource(inv){
  const ticker=(inv.ticker||'').trim().toUpperCase(),isin=(inv.isin||'').trim().toUpperCase();
  if(inv.resolvedTicker&&inv.resolvedTicker!==ticker){try{const r=await fetchPriceDirect(inv.resolvedTicker);return{...r,resolvedTicker:inv.resolvedTicker,resolvedBy:'cached'};}catch(e){inv.resolvedTicker=null;}}
  if(ticker){try{const r=await fetchPriceDirect(ticker);return{...r,resolvedTicker:ticker,resolvedBy:'ticker'};}catch(e){}
    if(/^\d{4,8}$/.test(ticker)){try{const r=await fetchPriceDirect(ticker+'.SI');return{...r,resolvedTicker:ticker+'.SI',resolvedBy:'SGX'};}catch(e){}}
    const yfT=await yfSearch(ticker);for(const q of yfT.slice(0,4)){try{const r=await fetchPriceDirect(q.symbol);return{...r,resolvedTicker:q.symbol,resolvedBy:'ticker-search'};}catch(e){}}}
  if(isin){const yfI=await yfSearch(isin);for(const q of yfI.slice(0,6)){try{const r=await fetchPriceDirect(q.symbol);return{...r,resolvedTicker:q.symbol,resolvedBy:'ISIN→YF'};}catch(e){}}
    const fl=await openFIGILookup(isin);const pt=['Open-End Fund','Exchange-Traded Fund','Common Stock','Closed-End Fund'];
    fl.sort((a,b)=>(pt.indexOf(a.securityType2||a.securityType||'')===-1?99:pt.indexOf(a.securityType2||a.securityType||''))-(pt.indexOf(b.securityType2||b.securityType||'')===-1?99:pt.indexOf(b.securityType2||b.securityType||'')));
    for(const f of fl.slice(0,10)){if(!f.ticker)continue;const sfx=exchSuffix(f.exchCode);if(sfx===undefined)continue;try{const r=await fetchPriceDirect(f.ticker+sfx);return{...r,resolvedTicker:f.ticker+sfx,resolvedBy:'ISIN→OpenFIGI'};}catch(e){}}}
  return null;
}
async function searchTicker(q){const j=await yf2Fetch(`/v1/finance/search?q=${encodeURIComponent(q)}&quotesCount=8&newsCount=0&enableFuzzyQuery=true&enableCb=false`);return(j?.quotes||[]).filter(x=>x.symbol&&x.quoteType&&!['FUTURE','OPTION','CURRENCY'].includes(x.quoteType)).map(x=>({symbol:x.symbol,name:x.shortname||x.longname||x.symbol,exchange:x.exchange||x.fullExchangeName||'',type:x.typeDisp||x.quoteType||''}));}
function resolvedByLabel(by){return{'ticker':'Ticker','SGX':'SGX (.SI)','ticker-search':'Ticker search','ISIN→YF':'ISIN via Yahoo','ISIN→OpenFIGI':'ISIN via OpenFIGI','cached':'Cached'}[by]||by||'';}

// ─── State ────────────────────────────────────────────────────────────────
let S={investments:[],dividends:[],tab:'portfolio',fCcy:'All',fAcc:'All',
       modal:null,editId:null,txId:null,txEditInvId:null,txEditId:null,divEditId:null,
       refreshing:{},refreshingAll:false,acResults:[],acLoading:false,acQuery:'',acTimer:null,
       searchQuery:'',showAllTx:{},catOpen:true};
try{const d=JSON.parse(localStorage.getItem('it2025')||'{}');S.investments=d.i||[];S.dividends=d.d||[];}catch(e){}
function save(){try{localStorage.setItem('it2025',JSON.stringify({i:S.investments,d:S.dividends}));}catch(e){}}
function uid(){return Date.now().toString(36)+Math.random().toString(36).slice(2);}
function fmt(n,dp=2){if(n==null||isNaN(n))return'–';return Number(n).toLocaleString('en-SG',{minimumFractionDigits:dp,maximumFractionDigits:dp});}
function fc(n,c,dp=2){const m={SGD:'S$',USD:'US$',GBP:'£',EUR:'€',HKD:'HK$'};const s=m[c]||(c?c+' ':'');if(n==null||isNaN(n))return s+'–';return(n<0?'-':'')+s+Math.abs(n).toLocaleString('en-SG',{minimumFractionDigits:dp,maximumFractionDigits:dp});}
function toast(msg,dur=3500){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');clearTimeout(t._t);t._t=setTimeout(()=>t.classList.remove('show'),dur);}
function tsAgo(ts){if(!ts)return'';const m=Math.round((Date.now()-ts)/60000);if(m<1)return'just now';if(m<60)return m+'m ago';const h=Math.round(m/60);return h<24?h+'h ago':Math.round(h/24)+'d ago';}

// ─── Price Refresh ────────────────────────────────────────────────────────
async function refreshOne(invId){
  const inv=S.investments.find(i=>i.id===invId);if(!inv)return;
  if(!inv.ticker&&!inv.isin){toast('⚠️ Add a ticker or ISIN first.');return;}
  S.refreshing[invId]=true;render();
  try{const r=await resolvePriceSource(inv);if(r&&r.price){inv.spotPrice=r.price;inv.priceChange=r.change;inv.priceChangePct=r.changePct;inv.priceUpdated=Date.now();inv.priceAuto=true;inv.resolvedTicker=r.resolvedTicker||null;inv.resolvedBy=r.resolvedBy||null;save();toast('✅ '+inv.name+': '+fc(r.price,inv.currency,3)+' via '+resolvedByLabel(r.resolvedBy));}else{inv.priceAuto=false;save();toast('⚠️ '+inv.name+': No price found. Enter manually.');}}catch(e){toast('⚠️ '+e.message);}
  S.refreshing[invId]=false;render();
}
async function refreshAll(){
  const list=S.investments.filter(i=>i.ticker||i.isin);if(!list.length){toast('No tickers or ISINs set.');return;}
  S.refreshingAll=true;render();let ok=0,fail=0;
  await Promise.all(list.map(async inv=>{S.refreshing[inv.id]=true;try{const r=await resolvePriceSource(inv);if(r&&r.price){inv.spotPrice=r.price;inv.priceChange=r.change;inv.priceChangePct=r.changePct;inv.priceUpdated=Date.now();inv.priceAuto=true;inv.resolvedTicker=r.resolvedTicker||null;inv.resolvedBy=r.resolvedBy||null;ok++;}else{fail++;}}catch(e){fail++;}S.refreshing[inv.id]=false;}));
  S.refreshingAll=false;save();toast('✅ '+ok+' updated'+(fail?' · ⚠️ '+fail+' not found':''));render();
}
setInterval(()=>{if(!document.hidden&&S.investments.some(i=>i.ticker||i.isin))refreshAll();},5*60*1000);
document.addEventListener('visibilitychange',()=>{if(!document.hidden&&S.investments.some(i=>i.ticker||i.isin))refreshAll();});

// ─── Autocomplete ─────────────────────────────────────────────────────────
function onNameInput(val){clearTimeout(S.acTimer);S.acQuery=val;S.acResults=[];if(val.length<2){S.acLoading=false;renderAC();return;}S.acLoading=true;renderAC();S.acTimer=setTimeout(async()=>{try{const res=await searchTicker(val);if(S.acQuery===val){S.acResults=res;S.acLoading=false;renderAC();}}catch(e){S.acLoading=false;renderAC();}},400);}
function renderAC(){const drop=document.getElementById('ac-drop');if(!drop)return;if(S.acLoading){drop.innerHTML='<div class="ac-loading"><span class="spin"></span> Searching…</div>';drop.style.display='block';return;}if(!S.acResults.length){drop.style.display='none';return;}drop.innerHTML=S.acResults.map((r,i)=>`<div class="ac-item" onclick="pickAC(${i})"><div style="display:flex;justify-content:space-between"><span class="ac-name">${r.name}</span><span class="ac-sym">${r.symbol}</span></div><span class="ac-ex">${r.exchange} · ${r.type}</span></div>`).join('');drop.style.display='block';}
function pickAC(i){const r=S.acResults[i];const ne=document.getElementById('fn'),te=document.getElementById('ftk');if(ne&&!ne.value)ne.value=r.name;if(te)te.value=r.symbol;const excCcy={NMS:'USD',NYQ:'USD',NGM:'USD',LSE:'GBP',SES:'SGD',HKG:'HKD',FRA:'EUR',EPA:'EUR'};const ccy=excCcy[r.exchange]||'';if(ccy){const el=document.getElementById('fc2');if(el)el.value=ccy;}const excMkt={NMS:'NASDAQ',NYQ:'NYSE',NGM:'NASDAQ',LSE:'LSE',SES:'SGX',HKG:'HKEX'};const mkt=excMkt[r.exchange]||'Other';const mel=document.getElementById('fm');if(mel)mel.value=mkt;S.acResults=[];S.acLoading=false;const drop=document.getElementById('ac-drop');if(drop)drop.style.display='none';}

// ─── Calc ─────────────────────────────────────────────────────────────────
function calc(inv){
  let sh=0,cost=0;
  for(const t of(inv.transactions||[])){const u=+t.shares||0,p=+t.price||0,f=+t.fees||0;if(t.type==='Buy'){cost+=u*p+f;sh+=u;}else if(t.type==='Sell'){const ac=sh>0?cost/sh:0;cost-=u*ac;sh-=u;}else if(t.type==='Rights Issue'){cost+=u*p+f;sh+=u;}else if(t.type==='Free Issue'){sh+=u;}}
  const avg=sh>0?cost/sh:0,spot=+inv.spotPrice||0,mv=spot*sh,pl=mv-cost,plp=cost>0?(pl/cost)*100:0;
  const myDivs=S.dividends.filter(d=>d.invId===inv.id);
  const dbf=myDivs.filter(d=>d.date&&d.date.slice(0,4)<CY).reduce((s,d)=>s+(+d.amount||0),0);
  const dcur=myDivs.filter(d=>d.date&&d.date.startsWith(CY)).reduce((s,d)=>s+(+d.amount||0),0);
  const dtot=dbf+dcur,dy=cost>0?(dcur/cost)*100:0,dytot=cost>0?(dtot/cost)*100:0;
  return{sh,cost,avg,spot,mv,pl,plp,dbf,dcur,dtot,dy,dytot};
}

// ─── Category Breakdown ───────────────────────────────────────────────────
function buildCatData(){
  const typeKeys=['ETF','Stock','Unit Trust','Bond','Other'];
  const byType={};typeKeys.forEach(k=>{byType[k]={cost:0,mv:0,pl:0,dbf:0,dcur:0,dtot:0,count:0};});
  S.investments.forEach(inv=>{const s=calc(inv);const k=typeKeys.includes(inv.type)?inv.type:'Other';byType[k].cost+=s.cost;byType[k].mv+=s.mv;byType[k].pl+=s.pl;byType[k].dbf+=s.dbf;byType[k].dcur+=s.dcur;byType[k].dtot+=s.dtot;byType[k].count++;});
  return{byType,typeKeys:typeKeys.filter(k=>byType[k].count>0)};
}

// ══════════════════════════════════════════════════════════
//  UPDATED: 2-row layout per category
//  Row A: Cost | Mkt Value | P&L
//  Row B: Div B/F | Div CY | Div Total  (green strip)
// ══════════════════════════════════════════════════════════
function renderCatBreakdown(tC,tM,tP,tDbf,tDcur,tDtot){
  const{byType,typeKeys}=buildCatData();
  if(!typeKeys.length)return'';
  const open=S.catOpen;
  return`
<div class="cat-panel">
  <!-- Header / toggle -->
  <div class="cat-panel-hdr" onclick="toggleCat()">
    <div class="cat-panel-hdr-left">
      📂 <span>Breakdown by Category</span>
      <span style="font-size:10px;font-weight:500;color:var(--gray);margin-left:2px">${typeKeys.length} type${typeKeys.length>1?'s':''}</span>
    </div>
    <span class="cat-chevron ${open?'open':''}">▼</span>
  </div>

  ${open?`
  <!-- Column headers for value row -->
  <div class="cat-col-hdr">
    <span style="text-align:left">Category</span>
    <span>Cost</span>
    <span>Mkt Value</span>
    <span>P &amp; L</span>
  </div>

  ${typeKeys.map(k=>{
    const d=byType[k];
    const mvPct=tM>0?(d.mv/tM*100):0;
    const costPct=tC>0?(d.cost/tC*100):0;
    const plPct=d.cost>0?(d.pl/d.cost*100):0;
    const plPos=d.pl>=0;
    return`
  <!-- ── Value row ── -->
  <div class="cat-val-row">
    <div class="cat-row-label">
      <div class="cat-dot" style="background:${TYPE_COLORS[k]};margin-top:4px"></div>
      <div>
        <div class="cat-row-name" style="color:${TYPE_FG[k]}">${k}</div>
        <div class="cat-row-sub">${d.count} holding${d.count>1?'s':''}</div>
        <div class="cat-bar-wrap" style="width:80px">
          <div class="cat-bar" style="width:${mvPct}%;background:${TYPE_COLORS[k]}"></div>
        </div>
      </div>
    </div>
    <div class="cat-cell">
      <div class="cv">${fc(d.cost,'SGD',0)}</div>
      <div class="cs">${fmt(costPct,1)}%</div>
    </div>
    <div class="cat-cell">
      <div class="cv">${fc(d.mv,'SGD',0)}</div>
      <div class="cs">${fmt(mvPct,1)}%</div>
    </div>
    <div class="cat-cell">
      <div class="cv ${plPos?'pos':'neg'}">${plPos?'+':''}${fc(d.pl,'SGD',0)}</div>
      <div class="cs ${plPos?'pos':'neg'}">${plPos?'+':''}${fmt(plPct,2)}%</div>
    </div>
  </div>

  <!-- ── Dividend sub-row ── -->
  <div class="cat-div-row">
    <div class="cat-div-cell">
      <div class="cdl">💰 Accum. B/F</div>
      <div class="cdv bf">${fc(d.dbf,'SGD',0)}</div>
      <div class="cds">Before ${CY}</div>
    </div>
    <div class="cat-div-cell">
      <div class="cdl">Div ${CY}</div>
      <div class="cdv cy">${fc(d.dcur,'SGD',0)}</div>
      <div class="cds">This year</div>
    </div>
    <div class="cat-div-cell">
      <div class="cdl">Accum. Total</div>
      <div class="cdv tot">${fc(d.dtot,'SGD',0)}</div>
      <div class="cds">All time</div>
    </div>
  </div>`;
  }).join('')}

  <!-- ── Grand Total value row ── -->
  <div class="cat-total-row">
    <div style="font-size:11px;font-weight:700;color:#374151;padding-left:16px">TOTAL</div>
    <div class="cat-cell"><div class="cv" style="font-size:13px">${fc(tC,'SGD',0)}</div></div>
    <div class="cat-cell"><div class="cv" style="font-size:13px">${fc(tM,'SGD',0)}</div></div>
    <div class="cat-cell"><div class="cv ${tP>=0?'pos':'neg'}" style="font-size:13px">${tP>=0?'+':''}${fc(tP,'SGD',0)}</div></div>
  </div>

  <!-- ── Grand Total dividend row ── -->
  <div class="cat-total-div">
    <div class="cat-div-cell">
      <div class="cdl">💰 Total B/F</div>
      <div class="cdv bf" style="font-size:13px">${fc(tDbf,'SGD',0)}</div>
    </div>
    <div class="cat-div-cell">
      <div class="cdl">Total ${CY}</div>
      <div class="cdv cy" style="font-size:13px">${fc(tDcur,'SGD',0)}</div>
    </div>
    <div class="cat-div-cell">
      <div class="cdl">Grand Total</div>
      <div class="cdv tot" style="font-size:13px">${fc(tDtot,'SGD',0)}</div>
    </div>
  </div>
  `:''}
</div>`;}

// ─── Pie Chart ────────────────────────────────────────────────────────────
function makePie(slices,size=140){
  const total=slices.reduce((s,x)=>s+x.value,0);if(!total)return'<div style="text-align:center;color:var(--gray);font-size:13px;padding:20px">No data yet</div>';
  const cx=size/2,cy=size/2,r=size/2-6,ri=r*0.52;let paths='',ang=-Math.PI/2;
  for(const sl of slices){const a=(sl.value/total)*2*Math.PI,ea=ang+a;const x1=cx+r*Math.cos(ang),y1=cy+r*Math.sin(ang),x2=cx+r*Math.cos(ea),y2=cy+r*Math.sin(ea);const xi1=cx+ri*Math.cos(ang),yi1=cy+ri*Math.sin(ang),xi2=cx+ri*Math.cos(ea),yi2=cy+ri*Math.sin(ea);const lg=a>Math.PI?1:0;paths+=`<path d="M${xi1},${yi1}L${x1},${y1}A${r},${r} 0 ${lg},1 ${x2},${y2}L${xi2},${yi2}A${ri},${ri} 0 ${lg},0 ${xi1},${yi1}" fill="${sl.color}" stroke="#fff" stroke-width="2"/>`;ang=ea;}
  return`<svg width="${size}" height="${size}" viewBox="0 0 ${size} ${size}" class="pie-svg">${paths}</svg>`;
}

// ─── Year-end Banner ──────────────────────────────────────────────────────
function yearEndBanner(){
  if(CMONTH<10)return'';
  const allStats=S.investments.map(i=>calc(i));
  const totalDiv=allStats.reduce((s,x)=>s+x.dtot,0),curDiv=allStats.reduce((s,x)=>s+x.dcur,0);
  const daysLeft=Math.ceil((new Date(NOW.getFullYear(),11,31)-NOW)/(1000*60*60*24));
  return`<div class="yr-banner"><div class="yb-ico">🔔</div><div class="yb-txt"><b>Year-end: ${daysLeft} day${daysLeft!==1?'s':''} left in ${CY}</b><br>On <b>1 Jan ${NY}</b>, ${CY} dividends (<b>${fc(curDiv,'SGD')}</b>) auto-become Accum. B/F for ${NY}. Total carry-forward: <b>${fc(totalDiv,'SGD')}</b>.</div></div>`;
}

// ─── Render ───────────────────────────────────────────────────────────────
function render(){
  const q=S.searchQuery.trim().toLowerCase();
  const invs=S.investments.filter(i=>{if(S.fCcy!=='All'&&i.currency!==S.fCcy)return false;if(S.fAcc!=='All'&&i.account!==S.fAcc)return false;if(q&&!i.name.toLowerCase().includes(q)&&!(i.ticker||'').toLowerCase().includes(q)&&!(i.isin||'').toLowerCase().includes(q))return false;return true;});
  const all=S.investments.map(i=>calc(i));
  const tC=all.reduce((s,x)=>s+x.cost,0),tM=all.reduce((s,x)=>s+x.mv,0),tP=tM-tC,
        tDbf=all.reduce((s,x)=>s+x.dbf,0),tDcur=all.reduce((s,x)=>s+x.dcur,0),tDtot=all.reduce((s,x)=>s+x.dtot,0);
  document.getElementById('app').innerHTML=`
<div class="header">
  <div style="display:flex;justify-content:space-between;align-items:center">
    <div><h1>📊 InvestTracker</h1><div class="sub">${NOW.toLocaleDateString('en-SG',{weekday:'short',day:'numeric',month:'short',year:'numeric'})}</div></div>
    <button class="btn raf" onclick="refreshAll()" ${S.refreshingAll?'disabled':''}>${S.refreshingAll?'<span class="spin"></span>':'🔄'} ${S.refreshingAll?'Updating…':'Refresh All'}</button>
  </div>
  <div class="tabs">
    <button class="tab ${S.tab==='portfolio'?'active':''}" onclick="setTab('portfolio')">Portfolio</button>
    <button class="tab ${S.tab==='dividends'?'active':''}" onclick="setTab('dividends')">Dividends</button>
    <button class="tab ${S.tab==='summary'?'active':''}" onclick="setTab('summary')">Summary</button>
  </div>
</div>
${S.tab==='portfolio'?renderPort(invs,tC,tM,tP,tDbf,tDcur,tDtot):S.tab==='dividends'?renderDivs():renderSummary()}
${renderModal()}`;
  document.querySelectorAll('.spi').forEach(el=>el.addEventListener('change',function(){const inv=S.investments.find(i=>i.id===this.dataset.id);if(inv){inv.spotPrice=parseFloat(this.value)||0;inv.priceAuto=false;inv.priceUpdated=Date.now();save();render();}}));
  const si=document.getElementById('search-input');if(si)si.addEventListener('input',function(){S.searchQuery=this.value;renderPortOnly();});
}

function renderPortOnly(){
  const q=S.searchQuery.trim().toLowerCase();
  const invs=S.investments.filter(i=>{if(S.fCcy!=='All'&&i.currency!==S.fCcy)return false;if(S.fAcc!=='All'&&i.account!==S.fAcc)return false;if(q&&!i.name.toLowerCase().includes(q)&&!(i.ticker||'').toLowerCase().includes(q)&&!(i.isin||'').toLowerCase().includes(q))return false;return true;});
  const el=document.getElementById('holdings-list');if(!el)return;
  const countEl=document.getElementById('holdings-count');if(countEl)countEl.textContent=`Holdings (${invs.length})`;
  el.innerHTML=invs.length===0?`<div class="${q?'no-results':'empty'}">${q?`No investments match "<b>${q}</b>"`:'<div class="ico">📈</div><div>No investments yet.</div>'}</div>`:invs.map(inv=>renderCard(inv)).join('');
  document.querySelectorAll('.spi').forEach(el=>el.addEventListener('change',function(){const inv=S.investments.find(i=>i.id===this.dataset.id);if(inv){inv.spotPrice=parseFloat(this.value)||0;inv.priceAuto=false;inv.priceUpdated=Date.now();save();renderPortOnly();}}));
}

function renderPort(invs,tC,tM,tP,tDbf,tDcur,tDtot){return`
<div class="summary">
  <div class="scard"><div class="lbl">Total Cost</div><div class="val">${fc(tC,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Mkt Value</div><div class="val">${fc(tM,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">P&L</div><div class="val ${tP>=0?'pos':'neg'}">${tP>=0?'+':''}${fc(tP,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div B/F</div><div class="val" style="color:#0369a1">${fc(tDbf,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div ${CY}</div><div class="val pos">${fc(tDcur,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div Total</div><div class="val" style="color:var(--purple)">${fc(tDtot,'SGD',0)}</div></div>
</div>
${renderCatBreakdown(tC,tM,tP,tDbf,tDcur,tDtot)}
${yearEndBanner()}
<div class="search-bar">
  <input id="search-input" placeholder="🔍  Search investments…" value="${S.searchQuery}" autocomplete="off">
  ${S.searchQuery?`<button class="sclear" onclick="clearSearch()">✕</button>`:''}
</div>
<div class="filter-bar">
  ${['All','SGD','USD','GBP','EUR','HKD'].map(c=>`<button class="chip ${S.fCcy===c?'on':''}" onclick="setF('c','${c}')">${c}</button>`).join('')}
  <span style="width:1px;background:#e5e5ea;flex-shrink:0"></span>
  ${['All','Cash','SRS','CPF'].map(a=>`<button class="chip ${S.fAcc===a?'on':''}" onclick="setF('a','${a}')">${a}</button>`).join('')}
</div>
<div class="section">
  <div class="sec-hdr">
    <span class="sec-ttl" id="holdings-count">Holdings (${invs.length})</span>
    <button class="btn sm" onclick="openM('addInv')">+ Add</button>
  </div>
  <div id="holdings-list">
    ${invs.length===0?`<div class="${S.searchQuery?'no-results':'empty'}">${S.searchQuery?`No investments match "<b>${S.searchQuery}</b>"`:'<div class="ico">📈</div><div>No investments yet.<br>Tap <b>+ Add</b> to begin.</div>'}</div>`:invs.map(inv=>renderCard(inv)).join('')}
  </div>
</div>`;}

function renderCard(inv){
  const s=calc(inv);const loading=S.refreshing[inv.id];const chg=inv.priceChange||0,chgP=inv.priceChangePct||0,cc=chg>=0?'var(--green)':'var(--red)';
  let srcBadge='';if(inv.priceAuto&&inv.spotPrice){if(inv.resolvedBy==='ISIN→OpenFIGI')srcBadge='<span class="fbadge">FIGI</span>';else if(inv.resolvedBy&&inv.resolvedBy.includes('ISIN'))srcBadge='<span class="ibadge">ISIN</span>';else srcBadge='<span class="abadge">LIVE</span>';}else{srcBadge='<span class="mbadge">MANUAL</span>';}
  const txs=inv.transactions||[];const showAll=S.showAllTx[inv.id];const displayTxs=showAll?[...txs].reverse():[...txs].reverse().slice(0,3);
  return`<div class="card" id="card-${inv.id}">
  <div class="inv-top">
    <div class="inv-meta">
      <div class="inv-name">${inv.name||'Unnamed'}</div>
      <div class="inv-tkr">${inv.ticker?`<b>${inv.ticker}</b>`:'No ticker'}${inv.isin?` · <span style="color:var(--blue)">${inv.isin}</span>`:''} · ${inv.market||'–'}</div>
      <div class="badges"><span class="badge">${inv.type||'Stock'}</span><span class="badge ${(inv.currency||'').toLowerCase()}">${inv.currency||'–'}</span><span class="badge ${(inv.account||'').toLowerCase()}">${inv.account||'–'}</span></div>
    </div>
    <div class="price-box">
      <div class="price-lbl">${srcBadge}<button class="btn sm out" style="padding:2px 8px;font-size:10px" onclick="refreshOne('${inv.id}')" ${loading?'disabled':''}>${loading?'<span class="spin"></span>':'🔄'}</button></div>
      ${inv.spotPrice?`<div class="price-big">${fc(inv.spotPrice,inv.currency,3)}</div>${inv.priceAuto?`<div class="price-chg" style="color:${cc}">${chg>=0?'+':''}${fc(chg,inv.currency,3)} (${chg>=0?'+':''}${fmt(chgP,2)}%)</div>`:''}<div class="price-ts">${tsAgo(inv.priceUpdated)}</div>${inv.resolvedTicker&&inv.resolvedTicker!==(inv.ticker||'').toUpperCase()?`<div class="price-resolved">via ${inv.resolvedTicker}</div>`:''}`:`<div style="font-size:11px;color:var(--gray);margin-top:6px">Tap 🔄 to fetch</div>`}
      <div class="manual-row"><input class="spot-inp spi" data-id="${inv.id}" type="number" step="0.0001" value="${inv.spotPrice||''}" placeholder="Manual"></div>
    </div>
  </div>
  ${inv.resolvedBy&&inv.resolvedBy.includes('ISIN')?`<div class="src-info">ℹ️ ISIN <b>${inv.isin}</b> → <b>${inv.resolvedTicker||''}</b> (${resolvedByLabel(inv.resolvedBy)})</div>`:''}
  <div class="grid2">
    <div class="stat"><div class="sl">Units Held</div><div class="sv">${fmt(s.sh,4)}</div></div>
    <div class="stat"><div class="sl">Avg Cost</div><div class="sv">${fc(s.avg,inv.currency,4)}</div></div>
    <div class="stat"><div class="sl">Total Cost</div><div class="sv">${fc(s.cost,inv.currency)}</div></div>
    <div class="stat"><div class="sl">Mkt Value</div><div class="sv">${fc(s.mv,inv.currency)}</div></div>
    <div class="stat"><div class="sl">Unreal. P&L</div><div class="sv ${s.pl>=0?'pos':'neg'}">${s.pl>=0?'+':''}${fc(s.pl,inv.currency)}<br><small>${s.pl>=0?'+':''}${fmt(s.plp,2)}%</small></div></div>
    <div class="stat"><div class="sl">Breakeven</div><div class="sv" style="font-size:11px">${fc(s.avg,inv.currency,4)}<br><small style="color:var(--gray)">avg cost/unit</small></div></div>
  </div>
  <div class="div-strip">
    <div class="div-strip-title">💰 Dividends Received</div>
    <div class="div-strip-grid">
      <div class="div-col"><div class="dc-lbl">Accum. B/F</div><div class="dc-val" style="color:#0369a1">${fc(s.dbf,inv.currency)}</div><div class="dc-sub">Before ${CY}</div></div>
      <div class="div-col"><div class="dc-lbl">Year ${CY}</div><div class="dc-val">${fc(s.dcur,inv.currency)}</div><div class="dc-sub">This year</div></div>
      <div class="div-col"><div class="dc-lbl">Accum. To-Date</div><div class="dc-val" style="color:var(--purple)">${fc(s.dtot,inv.currency)}</div><div class="dc-sub">All time</div></div>
    </div>
    <div class="yield-row">
      <div class="yield-item"><div class="yl">Yield ${CY}</div><div class="yv" style="color:var(--green)">${fmt(s.dy,2)}%</div></div>
      <div class="yield-item" style="border-left:1px solid #d1fae5;border-right:1px solid #d1fae5"><div class="yl">Yield Total</div><div class="yv" style="color:var(--purple)">${fmt(s.dytot,2)}%</div></div>
      <div class="yield-item"><div class="yl">Entries</div><div class="yv" style="color:var(--gray)">${S.dividends.filter(d=>d.invId===inv.id).length}</div></div>
    </div>
  </div>
  <div class="actions">
    <button class="btn sm out" onclick="openTx('${inv.id}')">+ Transaction</button>
    <button class="btn sm out" onclick="openDiv('${inv.id}')">+ Dividend</button>
    <button class="btn sm out" onclick="openEdit('${inv.id}')">Edit</button>
    <button class="btn sm red" onclick="delInv('${inv.id}')">Delete</button>
  </div>
  ${txs.length>0?`<div class="tx-section"><div class="tx-hdr-row"><div class="tx-hdr">Transactions (${txs.length})</div>${txs.length>3?`<button class="btn xs out" onclick="toggleTxAll('${inv.id}')">${showAll?'Show less':'Show all'}</button>`:''}</div>
    ${displayTxs.map(t=>`<div class="tx-item"><div class="tx-left"><span class="tx-t ${t.type.toLowerCase().replace(' issue','').replace(' ','')}">${t.type}</span><span style="color:var(--gray);margin-left:6px">${t.date||''}</span>${t.notes?`<div style="font-size:10px;color:var(--gray);margin-top:1px">${t.notes}</div>`:''}</div><div class="tx-right"><div>${fmt(t.shares,4)} units</div>${t.price?`<div style="color:var(--gray);font-size:11px">@ ${fc(t.price,inv.currency,4)}</div>`:''}${t.fees?`<div style="color:var(--gray);font-size:10px">fee ${fc(t.fees,inv.currency,2)}</div>`:''}</div><div class="tx-btns"><button class="btn xs out" onclick="openEditTx('${inv.id}','${t.id}')">✏️</button><button class="btn xs red" onclick="delTx('${inv.id}','${t.id}')">🗑</button></div></div>`).join('')}
  </div>`:''}
</div>`;}

function renderDivs(){
  const divs=S.dividends.slice().sort((a,b)=>(b.date||'').localeCompare(a.date||''));
  const allStats=S.investments.map(i=>calc(i));
  const totDbf=allStats.reduce((s,x)=>s+x.dbf,0),totDcur=allStats.reduce((s,x)=>s+x.dcur,0),totDtot=allStats.reduce((s,x)=>s+x.dtot,0);
  const byInv={};divs.forEach(d=>{if(!byInv[d.invId])byInv[d.invId]=[];byInv[d.invId].push(d);});
  return`
<div class="summary">
  <div class="scard"><div class="lbl">Accum. B/F</div><div class="val" style="color:#0369a1">${fc(totDbf,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Year ${CY}</div><div class="val pos">${fc(totDcur,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Accum. Total</div><div class="val" style="color:var(--purple)">${fc(totDtot,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Records</div><div class="val">${divs.length}</div></div>
</div>
${yearEndBanner()}
<div class="section" style="padding-top:12px">
  <div class="sec-hdr"><span class="sec-ttl">Dividend Ledger</span><button class="btn sm grn" onclick="openDivG()">+ Add</button></div>
  ${divs.length===0?`<div class="empty"><div class="ico">💰</div><div>No dividends recorded yet.</div></div>`:''}
  ${Object.entries(byInv).map(([id,ds])=>{
    const inv=S.investments.find(i=>i.id===id);const nm=inv?.name||'Unknown',ccy=inv?.currency||'SGD';
    const bf=ds.filter(d=>d.date&&d.date.slice(0,4)<CY).reduce((s,d)=>s+(+d.amount||0),0);
    const cur=ds.filter(d=>d.date&&d.date.startsWith(CY)).reduce((s,d)=>s+(+d.amount||0),0);const tot=bf+cur;
    const byYr={};ds.forEach(d=>{const y=(d.date||'????').slice(0,4);if(!byYr[y])byYr[y]=[];byYr[y].push(d);});
    return`<div class="ledger-inv">
      <div class="ledger-inv-hdr"><span>${nm} <span style="font-size:11px;color:var(--gray);font-weight:500">${inv?.ticker||''}</span></span></div>
      <div class="ledger-summary">
        <div class="ls-box bf"><div class="ls-lbl">Accum. B/F</div><div class="ls-val">${fc(bf,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">Before ${CY}</div></div>
        <div class="ls-box"><div class="ls-lbl">Year ${CY}</div><div class="ls-val">${fc(cur,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">Current year</div></div>
        <div class="ls-box tot"><div class="ls-lbl">Accum. Total</div><div class="ls-val">${fc(tot,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">All time</div></div>
      </div>
      ${Object.entries(byYr).sort((a,b)=>b[0].localeCompare(a[0])).map(([yr,yds])=>{
        const ytot=yds.reduce((s,d)=>s+(+d.amount||0),0);
        return`<div class="div-yr-block">
          <div class="div-yr-hdr"><span>📅 ${yr} (${yds.length})</span><span></span><span></span><span style="color:var(--green);font-weight:700">${fc(ytot,ccy)}</span><span></span></div>
          ${yds.map(d=>`<div class="div-yr-row"><span>${d.date||'–'}</span><span style="font-size:10px;color:var(--gray)">${d.divType||'Cash'}</span><span style="font-weight:700;color:var(--green)">${fc(d.amount,ccy,4)}</span><span style="color:var(--gray);font-size:10px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${d.notes||''}</span><div style="display:flex;gap:4px;justify-content:flex-end"><button class="btn xs out" onclick="openEditDiv('${d.id}')">✏️</button><button class="btn xs red" onclick="delDiv('${d.id}')">🗑</button></div></div>`).join('')}
        </div>`;}).join('')}
    </div>`;}).join('')}
</div>`;}

function renderSummary(){
  if(!S.investments.length)return`<div class="sumpage"><div class="empty" style="padding-top:60px"><div class="ico">📊</div><div>No investments yet.</div></div></div>`;
  const allStats=S.investments.map(inv=>({inv,s:calc(inv)}));
  const totalMv=allStats.reduce((s,x)=>s+x.s.mv,0),totalCost=allStats.reduce((s,x)=>s+x.s.cost,0),totalPl=totalMv-totalCost,totalDiv=allStats.reduce((s,x)=>s+x.s.dtot,0);
  const typeKeys=['ETF','Stock','Unit Trust','Bond','Other'];
  const byType={};typeKeys.forEach(k=>{byType[k]={mv:0,cost:0,count:0,pl:0,div:0};});
  allStats.forEach(({inv,s})=>{const k=typeKeys.includes(inv.type)?inv.type:'Other';byType[k].mv+=s.mv;byType[k].cost+=s.cost;byType[k].count++;byType[k].pl+=s.pl;byType[k].div+=s.dtot;});
  const typeSlices=typeKeys.filter(k=>byType[k].mv>0).map(k=>({label:k,value:byType[k].mv,color:TYPE_COLORS[k]}));
  const accKeys=['Cash','SRS','CPF'];const byAcc={};accKeys.forEach(k=>{byAcc[k]={mv:0,cost:0,count:0,pl:0,div:0};});
  allStats.forEach(({inv,s})=>{const k=accKeys.includes(inv.account)?inv.account:'Cash';byAcc[k].mv+=s.mv;byAcc[k].cost+=s.cost;byAcc[k].count++;byAcc[k].pl+=s.pl;byAcc[k].div+=s.dtot;});
  const accSlices=accKeys.filter(k=>byAcc[k].mv>0).map(k=>({label:k,value:byAcc[k].mv,color:ACC_COLORS[k]}));
  const profData={Profit:{mv:0,count:0},Loss:{mv:0,count:0},'Break-even':{mv:0,count:0}};
  allStats.forEach(({s})=>{const k=s.pl>0.01?'Profit':s.pl<-0.01?'Loss':'Break-even';profData[k].mv+=s.mv;profData[k].count++;});
  const profSlices=Object.entries(profData).filter(([,v])=>v.count>0).map(([k,v])=>({label:k,value:v.mv,color:PROF_COLORS[k]}));
  const topHoldings=[...allStats].sort((a,b)=>b.s.mv-a.s.mv).slice(0,6),maxMv=topHoldings[0]?.s.mv||1;
  return`
<div class="summary">
  <div class="scard"><div class="lbl">Total Value</div><div class="val">${fc(totalMv,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total Cost</div><div class="val">${fc(totalCost,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total P&L</div><div class="val ${totalPl>=0?'pos':'neg'}">${totalPl>=0?'+':''}${fc(totalPl,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total Div</div><div class="val" style="color:var(--purple)">${fc(totalDiv,'SGD',0)}</div></div>
</div>
<div class="sumpage">
  <div class="sum-section">
    <div class="sum-section-title">📂 By Investment Type</div>
    <div class="pie-wrap"><div class="pie-title">Market Value Allocation</div><div class="pie-body">${makePie(typeSlices)}<div class="pie-legend">${typeSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card"><div class="bk-hdr" style="grid-template-columns:1.4fr 1fr 1fr 1fr"><span>Type</span><span>Mkt Val</span><span>P&L</span><span>Div</span></div>
      ${typeKeys.filter(k=>byType[k].count>0).map(k=>{const d=byType[k];return`<div class="bk-row" style="grid-template-columns:1.4fr 1fr 1fr 1fr"><div class="bk-label"><div class="bk-dot" style="background:${TYPE_COLORS[k]}"></div>${k}</div><div>${fc(d.mv,'SGD',0)}<div style="font-size:10px;color:var(--gray)">${d.count} holding${d.count>1?'s':''}</div></div><div style="color:${d.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div><div style="color:var(--purple);font-weight:700">${fc(d.div,'SGD',0)}</div></div>`;}).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">💳 By Payment Mode</div>
    <div class="pie-wrap"><div class="pie-title">Market Value by Account</div><div class="pie-body">${makePie(accSlices)}<div class="pie-legend">${accSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card"><div class="bk-hdr" style="grid-template-columns:1.1fr 1fr 1fr 1fr"><span>Account</span><span>Mkt Val</span><span>P&L</span><span>Div</span></div>
      ${accKeys.filter(k=>byAcc[k].count>0).map(k=>{const d=byAcc[k];return`<div class="bk-row" style="grid-template-columns:1.1fr 1fr 1fr 1fr"><div class="bk-label"><div class="bk-dot" style="background:${ACC_COLORS[k]}"></div>${k}</div><div>${fc(d.mv,'SGD',0)}<div style="font-size:10px;color:var(--gray)">${d.count} holding${d.count>1?'s':''}</div></div><div style="color:${d.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div><div style="color:var(--purple);font-weight:700">${fc(d.div,'SGD',0)}</div></div>`;}).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">📈 Profitability</div>
    <div class="pie-wrap"><div class="pie-title">Portfolio Value by P&L Status</div><div class="pie-body">${makePie(profSlices)}<div class="pie-legend">${profSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card"><div class="bk-hdr" style="grid-template-columns:1.8fr 1fr 1fr"><span>Investment</span><span>P&L</span><span>P&L %</span></div>
      ${[...allStats].sort((a,b)=>b.s.pl-a.s.pl).map(({inv,s})=>`<div class="bk-row ${s.pl>=0?'prof-winner':'prof-loser'}" style="grid-template-columns:1.8fr 1fr 1fr"><div><div style="font-weight:600;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${inv.name}</div><div style="font-size:10px;color:var(--gray)">${inv.ticker||''} · ${inv.type}</div></div><div style="color:${s.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${s.pl>=0?'+':''}${fc(s.pl,inv.currency,0)}</div><div style="color:${s.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${s.pl>=0?'+':''}${fmt(s.plp,2)}%</div></div>`).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">🏆 Top Holdings</div>
    <div class="breakdown-card">
      ${topHoldings.map(({inv,s})=>{const pct=totalMv>0?s.mv/totalMv*100:0,bp=maxMv>0?s.mv/maxMv*100:0,tc=TYPE_COLORS[inv.type]||'#8e8e93';return`<div class="holding-row"><div style="flex:1;min-width:0"><div style="display:flex;justify-content:space-between;align-items:center"><div style="font-weight:600;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;max-width:60%">${inv.name}</div><div style="font-weight:700;font-size:13px">${fc(s.mv,inv.currency,0)}</div></div><div style="display:flex;justify-content:space-between;margin-top:2px"><div style="font-size:10px;color:var(--gray)">${inv.ticker||''} · <span style="color:${tc};font-weight:600">${inv.type}</span> · ${inv.account}</div><div style="font-size:10px;color:var(--gray)">${fmt(pct,1)}%</div></div><div class="holding-bar-bg"><div class="holding-bar-fill" style="width:${bp}%;background:${tc}"></div></div></div></div>`;}).join('')}
    </div>
  </div>
</div>`;}

function renderModal(){
  if(!S.modal)return'';const ov=`class="overlay" onclick="bgClose(event)"`;
  if(S.modal==='addInv'||S.modal==='editInv'){
    const v=S.modal==='editInv'?S.investments.find(i=>i.id===S.editId):{};const isnew=S.modal==='addInv';
    return`<div ${ov}><div class="modal"><h2>${isnew?'Add Investment':'Edit Investment'}</h2>
      <div class="fg"><label>Search by Name or ISIN</label><input id="fsearch" placeholder="Type name, ISIN, ticker…" oninput="onNameInput(this.value)" autocomplete="off"><div id="ac-drop" class="ac-drop" style="display:none"></div><div class="hint">Tap result → ticker, currency & market auto-filled</div></div>
      <div class="fg"><label>Investment Name *</label><input id="fn" placeholder="e.g. Schroder Asian Growth" value="${v?.name||''}"></div>
      <div class="row2"><div class="fg"><label>Ticker / Bloomberg</label><input id="ftk" placeholder="e.g. VWRA.L or 580703" value="${v?.ticker||''}"><div class="hint">Numeric codes auto-get .SI</div></div><div class="fg"><label>ISIN</label><input id="fis" placeholder="LU0943347566" value="${v?.isin||''}"><div class="hint">Fallback if ticker fails</div></div></div>
      <div class="row2"><div class="fg"><label>Type</label><select id="fty">${['ETF','Stock','Unit Trust','Bond'].map(x=>`<option ${v?.type===x?'selected':''}>${x}</option>`).join('')}</select></div><div class="fg"><label>Market</label><select id="fm">${['SGX','NYSE','NASDAQ','LSE','HKEX','Other'].map(x=>`<option ${v?.market===x?'selected':''}>${x}</option>`).join('')}</select></div></div>
      <div class="row2"><div class="fg"><label>Currency</label><select id="fc2">${['SGD','USD','GBP','EUR','HKD'].map(x=>`<option ${v?.currency===x?'selected':''}>${x}</option>`).join('')}</select></div><div class="fg"><label>Account</label><select id="fa">${['Cash','SRS','CPF'].map(x=>`<option ${v?.account===x?'selected':''}>${x}</option>`).join('')}</select></div></div>
      <div class="fg"><label>Notes</label><input id="fno" placeholder="Optional" value="${v?.notes||''}"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn" onclick="saveInv()">${isnew?'Add & Fetch Price':'Save & Refresh'}</button></div>
    </div></div>`;}
  if(S.modal==='addTx'){const inv=S.investments.find(i=>i.id===S.txId);return`<div ${ov}><div class="modal"><h2>Add Transaction</h2><div class="modal-sub">${inv?.name||''} · ${inv?.currency||''}</div><div class="fg"><label>Type *</label><select id="ftt">${['Buy','Sell','Rights Issue','Free Issue'].map(x=>`<option>${x}</option>`).join('')}</select></div><div class="row2"><div class="fg"><label>Date *</label><input id="ftd" type="date" value="${new Date().toISOString().slice(0,10)}"></div><div class="fg"><label>Units *</label><input id="fts" type="number" step="0.0001" placeholder="0.0000"></div></div><div class="row2"><div class="fg"><label>Price / Unit</label><input id="ftp" type="number" step="0.0001" placeholder="0.0000"></div><div class="fg"><label>Fees</label><input id="ftf" type="number" step="0.01" placeholder="0.00"></div></div><div class="fg"><label>Notes</label><input id="ftn" placeholder="Optional"></div><div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn" onclick="saveTx()">Add</button></div></div></div>`;}
  if(S.modal==='editTx'){const inv=S.investments.find(i=>i.id===S.txEditInvId);const tx=(inv?.transactions||[]).find(t=>t.id===S.txEditId);if(!tx)return'';return`<div ${ov}><div class="modal"><h2>Edit Transaction</h2><div class="modal-sub">${inv?.name||''} · ${inv?.currency||''}</div><div class="fg"><label>Type *</label><select id="ftt">${['Buy','Sell','Rights Issue','Free Issue'].map(x=>`<option ${tx.type===x?'selected':''}>${x}</option>`).join('')}</select></div><div class="row2"><div class="fg"><label>Date *</label><input id="ftd" type="date" value="${tx.date||''}"></div><div class="fg"><label>Units *</label><input id="fts" type="number" step="0.0001" value="${tx.shares||''}"></div></div><div class="row2"><div class="fg"><label>Price / Unit</label><input id="ftp" type="number" step="0.0001" value="${tx.price||''}"></div><div class="fg"><label>Fees</label><input id="ftf" type="number" step="0.01" value="${tx.fees||''}"></div></div><div class="fg"><label>Notes</label><input id="ftn" value="${tx.notes||''}" placeholder="Optional"></div><div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn amber" onclick="updateTx()">Save Changes</button></div></div></div>`;}
  if(S.modal==='addDiv'){return`<div ${ov}><div class="modal"><h2>Add Dividend</h2><div class="fg"><label>Investment *</label><select id="fdi">${S.investments.map(i=>`<option value="${i.id}" ${i.id===S.txId?'selected':''}>${i.name} (${i.ticker||i.isin||i.currency})</option>`).join('')}</select></div><div class="row2"><div class="fg"><label>Date *</label><input id="fdd" type="date" value="${new Date().toISOString().slice(0,10)}"></div><div class="fg"><label>Amount *</label><input id="fda" type="number" step="0.0001" placeholder="0.0000"></div></div><div class="fg"><label>Type</label><select id="fdt">${['Cash','Scrip','Special','Return of Capital'].map(x=>`<option>${x}</option>`).join('')}</select></div><div class="fg"><label>Notes</label><input id="fdn" placeholder="e.g. Q1 2025 dividend"></div><div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn grn" onclick="saveDiv()">Add</button></div></div></div>`;}
  if(S.modal==='editDiv'){const dv=S.dividends.find(d=>d.id===S.divEditId);if(!dv)return'';const inv=S.investments.find(i=>i.id===dv.invId);return`<div ${ov}><div class="modal"><h2>Edit Dividend</h2><div class="modal-sub">${inv?.name||'Unknown'}</div><div class="fg"><label>Investment</label><select id="fdi">${S.investments.map(i=>`<option value="${i.id}" ${i.id===dv.invId?'selected':''}>${i.name} (${i.ticker||i.currency})</option>`).join('')}</select></div><div class="row2"><div class="fg"><label>Date *</label><input id="fdd" type="date" value="${dv.date||''}"></div><div class="fg"><label>Amount *</label><input id="fda" type="number" step="0.0001" value="${dv.amount||''}"></div></div><div class="fg"><label>Type</label><select id="fdt">${['Cash','Scrip','Special','Return of Capital'].map(x=>`<option ${dv.divType===x?'selected':''}>${x}</option>`).join('')}</select></div><div class="fg"><label>Notes</label><input id="fdn" value="${dv.notes||''}" placeholder="e.g. Q1 2025 dividend"></div><div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn amber" onclick="updateDiv()">Save Changes</button></div></div></div>`;}
  return'';}

function setTab(t){S.tab=t;render();}
function setF(k,v){if(k==='c')S.fCcy=v;else S.fAcc=v;render();}
function clearSearch(){S.searchQuery='';render();}
function toggleTxAll(invId){S.showAllTx[invId]=!S.showAllTx[invId];render();}
function toggleCat(){S.catOpen=!S.catOpen;render();}
function openM(m){S.modal=m;S.editId=null;S.acResults=[];render();}
function openEdit(id){S.modal='editInv';S.editId=id;S.acResults=[];render();}
function openTx(id){S.modal='addTx';S.txId=id;render();}
function openDiv(id){S.modal='addDiv';S.txId=id;render();}
function openDivG(){S.modal='addDiv';S.txId=S.investments[0]?.id||null;render();}
function openEditTx(invId,txId){S.modal='editTx';S.txEditInvId=invId;S.txEditId=txId;render();}
function openEditDiv(divId){S.modal='editDiv';S.divEditId=divId;render();}
function closeM(){S.modal=null;S.editId=null;S.txId=null;S.txEditInvId=null;S.txEditId=null;S.divEditId=null;S.acResults=[];S.acLoading=false;render();}
function bgClose(e){if(e.target.classList.contains('overlay'))closeM();}
function saveInv(){
  const name=document.getElementById('fn').value.trim();if(!name){alert('Investment name is required.');return;}
  const ticker=document.getElementById('ftk').value.trim(),isin=document.getElementById('fis').value.trim();
  if(S.modal==='addInv'){const nv={id:uid(),name,ticker,isin,type:document.getElementById('fty').value,market:document.getElementById('fm').value,currency:document.getElementById('fc2').value,account:document.getElementById('fa').value,notes:document.getElementById('fno').value.trim(),spotPrice:0,transactions:[],resolvedTicker:null,resolvedBy:null};S.investments.push(nv);save();closeM();if(ticker||isin)setTimeout(()=>refreshOne(nv.id),300);}
  else{const inv=S.investments.find(i=>i.id===S.editId);if(inv){if(inv.ticker!==ticker||inv.isin!==isin){inv.resolvedTicker=null;inv.resolvedBy=null;}Object.assign(inv,{name,ticker,isin,type:document.getElementById('fty').value,market:document.getElementById('fm').value,currency:document.getElementById('fc2').value,account:document.getElementById('fa').value,notes:document.getElementById('fno').value.trim()});}save();closeM();if(inv&&(ticker||isin))setTimeout(()=>refreshOne(inv.id),300);}
}
function saveTx(){const sh=parseFloat(document.getElementById('fts').value);if(!sh||sh<=0){alert('Enter valid units.');return;}const inv=S.investments.find(i=>i.id===S.txId);if(!inv)return;if(!inv.transactions)inv.transactions=[];inv.transactions.push({id:uid(),type:document.getElementById('ftt').value,date:document.getElementById('ftd').value,shares:sh,price:parseFloat(document.getElementById('ftp').value)||0,fees:parseFloat(document.getElementById('ftf').value)||0,notes:document.getElementById('ftn').value.trim()});inv.transactions.sort((a,b)=>(a.date||'').localeCompare(b.date||''));save();closeM();}
function updateTx(){const sh=parseFloat(document.getElementById('fts').value);if(!sh||sh<=0){alert('Enter valid units.');return;}const inv=S.investments.find(i=>i.id===S.txEditInvId);if(!inv)return;const tx=(inv.transactions||[]).find(t=>t.id===S.txEditId);if(!tx)return;Object.assign(tx,{type:document.getElementById('ftt').value,date:document.getElementById('ftd').value,shares:sh,price:parseFloat(document.getElementById('ftp').value)||0,fees:parseFloat(document.getElementById('ftf').value)||0,notes:document.getElementById('ftn').value.trim()});inv.transactions.sort((a,b)=>(a.date||'').localeCompare(b.date||''));save();toast('✅ Transaction updated');closeM();}
function delTx(invId,txId){if(!confirm('Delete this transaction?'))return;const inv=S.investments.find(i=>i.id===invId);if(!inv)return;inv.transactions=(inv.transactions||[]).filter(t=>t.id!==txId);save();render();toast('🗑 Transaction deleted');}
function saveDiv(){const amt=parseFloat(document.getElementById('fda').value);if(!amt||amt<=0){alert('Enter valid amount.');return;}S.dividends.push({id:uid(),invId:document.getElementById('fdi').value,date:document.getElementById('fdd').value,amount:amt,divType:document.getElementById('fdt').value,notes:document.getElementById('fdn').value.trim()});save();closeM();}
function updateDiv(){const amt=parseFloat(document.getElementById('fda').value);if(!amt||amt<=0){alert('Enter valid amount.');return;}const dv=S.dividends.find(d=>d.id===S.divEditId);if(!dv)return;Object.assign(dv,{invId:document.getElementById('fdi').value,date:document.getElementById('fdd').value,amount:amt,divType:document.getElementById('fdt').value,notes:document.getElementById('fdn').value.trim()});save();toast('✅ Dividend updated');closeM();}
function delDiv(divId){if(!confirm('Delete this dividend entry?'))return;S.dividends=S.dividends.filter(d=>d.id!==divId);save();render();toast('🗑 Dividend deleted');}
function delInv(id){if(!confirm('Delete this investment and all its data?'))return;S.investments=S.investments.filter(i=>i.id!==id);S.dividends=S.dividends.filter(d=>d.invId!==id);save();render();}

render();
if(S.investments.some(i=>i.ticker||i.isin))setTimeout(refreshAll,1500);
</script>
</body>
</html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="InvestTracker">
<title>InvestTracker</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
body{font-family:-apple-system,BlinkMacSystemFont,'SF Pro Text',sans-serif;background:#f2f2f7;color:#1c1c1e;max-width:430px;margin:0 auto;min-height:100vh}
:root{--blue:#007aff;--green:#34c759;--red:#ff3b30;--gray:#8e8e93;--card:#fff;--border:#e5e5ea;--purple:#af52de;--orange:#ff9500;--teal:#5ac8fa;--indigo:#5856d6}
.header{background:rgba(255,255,255,0.95);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);padding:52px 20px 10px;position:sticky;top:0;z-index:100;border-bottom:1px solid var(--border)}
.header h1{font-size:24px;font-weight:700}
.sub{font-size:11px;color:var(--gray);margin-top:1px}
.tabs{display:flex;background:#e5e5ea;border-radius:10px;padding:2px;margin:10px 0 0}
.tab{flex:1;padding:7px 0;border:none;background:transparent;border-radius:8px;font-size:12px;font-weight:500;color:var(--gray);cursor:pointer;transition:all .2s}
.tab.active{background:#fff;color:#1c1c1e;box-shadow:0 1px 4px rgba(0,0,0,.12)}
.search-bar{padding:10px 20px 4px;position:relative}
.search-bar input{width:100%;padding:9px 36px 9px 14px;border:1.5px solid var(--border);border-radius:12px;font-size:14px;background:#fff;outline:none}
.search-bar input:focus{border-color:var(--blue)}
.search-bar .sclear{position:absolute;right:30px;top:50%;transform:translateY(-50%);color:var(--gray);font-size:18px;cursor:pointer;padding:2px 4px;border:none;background:transparent;line-height:1}
.yr-banner{margin:8px 20px 0;background:linear-gradient(135deg,#fff7ed,#fef3c7);border:1.5px solid #fcd34d;border-radius:14px;padding:12px 14px;display:flex;gap:10px;align-items:flex-start}
.yr-banner .yb-ico{font-size:22px;flex-shrink:0}
.yr-banner .yb-txt{font-size:12px;color:#92400e;line-height:1.5}
.yr-banner .yb-txt b{color:#78350f}
/* ── Top Summary Strip ── */
.summary{display:flex;gap:9px;padding:13px 20px;overflow-x:auto;scrollbar-width:none}
.summary::-webkit-scrollbar{display:none}
.scard{min-width:108px;background:var(--card);border-radius:14px;padding:11px 12px;box-shadow:0 1px 3px rgba(0,0,0,.08);flex-shrink:0}
.scard .lbl{font-size:10px;color:var(--gray);font-weight:600;text-transform:uppercase;letter-spacing:.4px}
.scard .val{font-size:16px;font-weight:700;margin-top:3px}
.val.pos{color:var(--green)}.val.neg{color:var(--red)}
/* ── Category Breakdown Panel ── */
.cat-panel{margin:0 20px 12px;background:var(--card);border-radius:16px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08)}
.cat-panel-hdr{display:flex;justify-content:space-between;align-items:center;padding:11px 14px;background:#f9f9f9;border-bottom:1px solid var(--border);cursor:pointer}
.cat-panel-hdr-left{display:flex;align-items:center;gap:7px;font-size:13px;font-weight:700;color:#1c1c1e}
.cat-chevron{font-size:11px;color:var(--gray);transition:transform .2s}
.cat-chevron.open{transform:rotate(180deg)}
.cat-body{overflow:hidden;transition:max-height .3s ease}
/* Column headers */
.cat-col-hdr{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 1fr;gap:0;padding:7px 14px;background:#f2f2f7;border-bottom:1px solid var(--border)}
.cat-col-hdr span{font-size:9px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px;text-align:right}
.cat-col-hdr span:first-child{text-align:left}
/* Category row */
.cat-row{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 1fr;gap:0;padding:11px 14px;border-bottom:1px solid var(--border);align-items:center}
.cat-row:last-child{border-bottom:none}
.cat-row-label{display:flex;align-items:center;gap:7px}
.cat-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.cat-row-name{font-size:12px;font-weight:700;color:#1c1c1e}
.cat-row-sub{font-size:10px;color:var(--gray);margin-top:1px}
.cat-cell{text-align:right}
.cat-cell .cv{font-size:12px;font-weight:700;color:#1c1c1e}
.cat-cell .cs{font-size:10px;color:var(--gray);margin-top:1px}
.cat-cell .pos{color:var(--green)}.cat-cell .neg{color:var(--red)}
.cat-cell .divv{color:var(--purple)}
/* Totals row */
.cat-total-row{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 1fr;gap:0;padding:10px 14px;background:#f0f7ff;border-top:2px solid #bae6fd}
.cat-total-row .cv{font-size:12px;font-weight:700}
/* Mini bar inside category row */
.cat-bar-wrap{height:3px;background:#e5e5ea;border-radius:2px;margin-top:4px}
.cat-bar{height:3px;border-radius:2px}
/* Filter chips */
.filter-bar{display:flex;gap:7px;padding:0 20px 11px;overflow-x:auto;scrollbar-width:none}
.filter-bar::-webkit-scrollbar{display:none}
.chip{padding:5px 13px;border-radius:20px;border:1.5px solid var(--border);background:#fff;font-size:12px;font-weight:600;cursor:pointer;white-space:nowrap;color:#3a3a3c}
.chip.on{background:var(--blue);color:#fff;border-color:var(--blue)}
.section{padding:0 20px 100px}
.sec-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
.sec-ttl{font-size:19px;font-weight:700}
.btn{background:var(--blue);color:#fff;border:none;border-radius:10px;padding:8px 16px;font-size:14px;font-weight:600;cursor:pointer;transition:opacity .2s}
.btn:active{opacity:.75}
.btn.sm{padding:5px 12px;font-size:12px;border-radius:8px}
.btn.xs{padding:3px 8px;font-size:11px;border-radius:6px}
.btn.out{background:transparent;color:var(--blue);border:1.5px solid var(--blue)}
.btn.red{background:var(--red)}.btn.grn{background:var(--green)}.btn.amber{background:var(--orange)}
.btn.raf{background:linear-gradient(135deg,#007aff,#5856d6);display:flex;align-items:center;gap:5px;font-size:13px;padding:8px 14px}
.btn:disabled{opacity:.45;cursor:not-allowed}
.card{background:var(--card);border-radius:16px;padding:15px;margin-bottom:12px;box-shadow:0 1px 3px rgba(0,0,0,.08)}
.inv-top{display:flex;justify-content:space-between;align-items:flex-start;gap:8px}
.inv-meta{flex:1;min-width:0}
.inv-name{font-size:15px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.inv-tkr{font-size:11px;color:var(--gray);margin-top:1px}
.badges{display:flex;gap:4px;flex-wrap:wrap;margin-top:5px}
.badge{font-size:10px;font-weight:600;padding:2px 7px;border-radius:20px;background:#e5e5ea;color:#3a3a3c}
.badge.usd{background:#dbeafe;color:#1d4ed8}.badge.sgd{background:#dcfce7;color:#15803d}
.badge.gbp{background:#f3e8ff;color:#7e22ce}.badge.eur{background:#fff7ed;color:#c2410c}
.badge.hkd{background:#fef9c3;color:#a16207}
.badge.cash{background:#dcfce7;color:#166534}.badge.srs{background:#fef9c3;color:#854d0e}.badge.cpf{background:#fce7f3;color:#9d174d}
.price-box{text-align:right;min-width:120px;flex-shrink:0}
.price-lbl{font-size:10px;color:var(--gray);margin-bottom:3px;display:flex;align-items:center;justify-content:flex-end;gap:4px}
.price-big{font-size:19px;font-weight:700}
.price-chg{font-size:11px;font-weight:600;margin-top:1px}
.price-ts{font-size:9px;color:var(--gray);margin-top:1px}
.price-resolved{font-size:9px;color:var(--blue);margin-top:1px;font-weight:600}
.abadge{font-size:9px;background:#dcfce7;color:#15803d;padding:1px 5px;border-radius:6px;font-weight:700}
.ibadge{font-size:9px;background:#dbeafe;color:#1d4ed8;padding:1px 5px;border-radius:6px;font-weight:700}
.mbadge{font-size:9px;background:#f2f2f7;color:var(--gray);padding:1px 5px;border-radius:6px;font-weight:700}
.fbadge{font-size:9px;background:#fce7f3;color:#9d174d;padding:1px 5px;border-radius:6px;font-weight:700}
.spin{display:inline-block;width:11px;height:11px;border:2px solid #e5e5ea;border-top-color:var(--blue);border-radius:50%;animation:spin .7s linear infinite;vertical-align:middle}
@keyframes spin{to{transform:rotate(360deg)}}
.manual-row{display:flex;align-items:center;gap:4px;justify-content:flex-end;margin-top:5px}
.spot-inp{width:88px;border:1.5px solid var(--border);border-radius:8px;padding:5px 8px;font-size:13px;text-align:right;background:#f9f9f9}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:7px;margin-top:11px}
.stat{background:#f2f2f7;border-radius:10px;padding:9px 10px}
.stat .sl{font-size:9px;color:var(--gray);font-weight:600;text-transform:uppercase;letter-spacing:.3px}
.stat .sv{font-size:13px;font-weight:700;margin-top:2px}
.div-strip{background:linear-gradient(135deg,#f0fdf4,#f0f9ff);border:1px solid #d1fae5;border-radius:12px;padding:11px 12px;margin-top:10px}
.div-strip-title{font-size:10px;font-weight:700;color:#15803d;text-transform:uppercase;letter-spacing:.4px;margin-bottom:8px}
.div-strip-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:0}
.div-col{text-align:center;padding:4px 2px}
.div-col:not(:last-child){border-right:1px solid #d1fae5}
.div-col .dc-lbl{font-size:9px;color:#6b7280;font-weight:600;text-transform:uppercase;letter-spacing:.3px;line-height:1.3}
.div-col .dc-val{font-size:13px;font-weight:700;color:#15803d;margin-top:3px}
.div-col .dc-sub{font-size:9px;color:#6b7280;margin-top:1px}
.yield-row{display:flex;justify-content:space-between;align-items:center;margin-top:8px;padding-top:8px;border-top:1px solid #d1fae5}
.yield-item{text-align:center;flex:1}
.yield-item .yl{font-size:9px;color:#6b7280;font-weight:600;text-transform:uppercase;letter-spacing:.3px}
.yield-item .yv{font-size:12px;font-weight:700;margin-top:2px}
.actions{display:flex;gap:7px;margin-top:11px;flex-wrap:wrap}
.tx-section{margin-top:12px}
.tx-hdr-row{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.tx-hdr{font-size:11px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px}
.tx-item{display:flex;justify-content:space-between;align-items:center;padding:8px 10px;border-bottom:1px solid var(--border);font-size:12px;gap:6px}
.tx-item:last-child{border-bottom:none}
.tx-item:first-child{border-top:1px solid var(--border)}
.tx-left{flex:1;min-width:0}
.tx-t{font-weight:700}
.tx-t.buy{color:var(--blue)}.tx-t.sell{color:var(--red)}.tx-t.rights{color:var(--orange)}.tx-t.free{color:var(--green)}
.tx-right{text-align:right;flex-shrink:0}
.tx-btns{display:flex;gap:4px;flex-shrink:0}
.src-info{background:#f0f9ff;border:1px solid #bae6fd;border-radius:9px;padding:7px 10px;margin-top:8px;font-size:11px;color:#0369a1;display:flex;align-items:flex-start;gap:6px;line-height:1.4}
.overlay{position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:200;display:flex;align-items:flex-end;justify-content:center}
.modal{background:#fff;border-radius:22px 22px 0 0;padding:22px 20px 44px;width:100%;max-width:430px;max-height:92vh;overflow-y:auto}
.modal h2{font-size:19px;font-weight:700;margin-bottom:4px}
.modal .modal-sub{font-size:12px;color:var(--gray);margin-bottom:16px}
.fg{margin-bottom:12px;position:relative}
.fg label{font-size:12px;font-weight:600;color:#444;display:block;margin-bottom:4px}
.fg input,.fg select{width:100%;padding:11px 13px;border:1.5px solid var(--border);border-radius:11px;font-size:15px;background:#fff;-webkit-appearance:none;appearance:none}
.fg input:focus,.fg select:focus{outline:none;border-color:var(--blue)}
.fg .hint{font-size:11px;color:var(--gray);margin-top:3px}
.row2{display:grid;grid-template-columns:1fr 1fr;gap:9px}
.m-actions{display:flex;gap:9px;margin-top:18px}
.m-actions .btn{flex:1;padding:13px}
.ac-drop{position:absolute;left:0;right:0;top:calc(100% - 2px);background:#fff;border:1.5px solid var(--blue);border-top:none;border-radius:0 0 11px 11px;z-index:50;max-height:210px;overflow-y:auto;box-shadow:0 8px 20px rgba(0,0,0,.12)}
.ac-item{padding:10px 13px;font-size:13px;cursor:pointer;border-bottom:1px solid var(--border);display:flex;flex-direction:column;gap:2px}
.ac-item:last-child{border-bottom:none}
.ac-item:active{background:#f0f7ff}
.ac-sym{font-weight:700;color:var(--blue);font-size:12px}
.ac-name{color:#1c1c1e;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.ac-ex{font-size:10px;color:var(--gray)}
.ac-loading{padding:14px;text-align:center;color:var(--gray);font-size:13px}
.div-yr-block{background:var(--card);border-radius:14px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:6px}
.div-yr-hdr{display:grid;grid-template-columns:1.4fr 0.8fr 1fr 0.8fr 60px;gap:4px;padding:8px 12px;background:#f2f2f7;font-size:10px;font-weight:700;color:var(--gray);text-transform:uppercase}
.div-yr-row{display:grid;grid-template-columns:1.4fr 0.8fr 1fr 0.8fr 60px;gap:4px;padding:10px 12px;border-top:1px solid var(--border);font-size:12px;align-items:center}
.ledger-inv{margin-bottom:18px}
.ledger-inv-hdr{font-size:14px;font-weight:700;display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.ledger-summary{display:grid;grid-template-columns:1fr 1fr 1fr;gap:7px;margin-bottom:8px}
.ls-box{background:#f0fdf4;border:1px solid #d1fae5;border-radius:10px;padding:8px 10px;text-align:center}
.ls-box.bf{background:#f0f9ff;border-color:#bae6fd}
.ls-box.tot{background:#fdf4ff;border-color:#e9d5ff}
.ls-lbl{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.3px;color:#6b7280}
.ls-val{font-size:13px;font-weight:700;margin-top:3px}
.ls-box.bf .ls-val{color:#0369a1}
.ls-box .ls-val{color:#15803d}
.ls-box.tot .ls-val{color:#7e22ce}
.sumpage{padding:0 20px 100px}
.sum-section{margin-bottom:20px}
.sum-section-title{font-size:16px;font-weight:700;margin-bottom:10px;display:flex;align-items:center;gap:6px}
.pie-wrap{background:var(--card);border-radius:16px;padding:18px;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:12px}
.pie-title{font-size:14px;font-weight:700;margin-bottom:14px}
.pie-body{display:flex;align-items:center;gap:16px}
.pie-svg{flex-shrink:0}
.pie-legend{flex:1;display:flex;flex-direction:column;gap:7px}
.pie-leg-item{display:flex;align-items:center;gap:7px}
.pie-dot{width:11px;height:11px;border-radius:50%;flex-shrink:0}
.pie-leg-label{font-size:12px;font-weight:500;color:#1c1c1e;flex:1}
.pie-leg-pct{font-size:10px;color:var(--gray);min-width:36px;text-align:right}
.breakdown-card{background:var(--card);border-radius:16px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:12px}
.bk-hdr{display:grid;gap:4px;padding:9px 14px;background:#f2f2f7;font-size:10px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px}
.bk-row{display:grid;gap:4px;padding:12px 14px;border-top:1px solid var(--border);font-size:13px;align-items:center}
.bk-label{font-weight:700;display:flex;align-items:center;gap:7px}
.bk-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0}
.prof-winner{background:#f0fdf4}.prof-loser{background:#fff5f5}
.holding-row{display:flex;justify-content:space-between;align-items:center;padding:10px 14px;border-top:1px solid var(--border)}
.holding-row:first-child{border-top:none}
.holding-bar-bg{height:4px;background:#e5e5ea;border-radius:2px;margin-top:4px}
.holding-bar-fill{height:4px;border-radius:2px}
.empty{text-align:center;padding:40px 20px;color:var(--gray)}
.empty .ico{font-size:38px;margin-bottom:8px}
.no-results{text-align:center;padding:30px 20px;color:var(--gray);font-size:14px}
.toast{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);background:#1c1c1e;color:#fff;padding:10px 20px;border-radius:20px;font-size:13px;font-weight:500;z-index:400;opacity:0;transition:opacity .3s;pointer-events:none;white-space:nowrap;max-width:90vw;text-align:center}
.toast.show{opacity:1}
</style>
</head>
<body>
<div id="app"></div>
<div class="toast" id="toast"></div>
<script>
'use strict';
const NOW=new Date(),CY=NOW.getFullYear().toString(),CMONTH=NOW.getMonth(),NY=(NOW.getFullYear()+1).toString();
const TYPE_COLORS={ETF:'#007aff',Stock:'#34c759','Unit Trust':'#ff9500',Bond:'#af52de',Other:'#8e8e93'};
const TYPE_BG={ETF:'#dbeafe',Stock:'#dcfce7','Unit Trust':'#fff7ed',Bond:'#fdf4ff',Other:'#f2f2f7'};
const TYPE_FG={ETF:'#1d4ed8',Stock:'#15803d','Unit Trust':'#c2410c',Bond:'#7e22ce',Other:'#6b7280'};
const ACC_COLORS={Cash:'#34c759',SRS:'#ff9f0a',CPF:'#ff2d55'};
const PROF_COLORS={Profit:'#34c759',Loss:'#ff3b30','Break-even':'#8e8e93'};
const PROXY='https://corsproxy.io/?';

// ─── Price Engine ─────────────────────────────────────────────────────────
async function yfFetch(p){const r=await fetch(PROXY+encodeURIComponent('https://query1.finance.yahoo.com'+p),{headers:{'x-requested-with':'XMLHttpRequest'}});if(!r.ok)throw new Error('HTTP '+r.status);return r.json();}
async function yf2Fetch(p){const r=await fetch(PROXY+encodeURIComponent('https://query2.finance.yahoo.com'+p),{headers:{'x-requested-with':'XMLHttpRequest'}});if(!r.ok)throw new Error('HTTP '+r.status);return r.json();}
async function fetchPriceDirect(sym){
  const j=await yfFetch(`/v8/finance/chart/${encodeURIComponent(sym)}?interval=1d&range=1d`);
  const m=j?.chart?.result?.[0]?.meta;if(!m)throw new Error('No meta');
  const p=m.regularMarketPrice||m.previousClose;if(!p)throw new Error('No price');
  const pv=m.chartPreviousClose||m.previousClose||p;
  return{price:p,change:p-pv,changePct:pv>0?((p-pv)/pv)*100:0,currency:m.currency||''};
}
async function yfSearch(q){try{const j=await yf2Fetch(`/v1/finance/search?q=${encodeURIComponent(q)}&quotesCount=8&newsCount=0&enableFuzzyQuery=false&enableCb=false`);return(j?.quotes||[]).filter(x=>x.symbol&&!['FUTURE','OPTION','CURRENCY','CRYPTOCURRENCY'].includes(x.quoteType));}catch(e){return[];}}
async function openFIGILookup(isin){try{const res=await fetch('https://api.openfigi.com/v3/mapping',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify([{"idType":"ID_ISIN","idValue":isin}])});if(!res.ok)return[];const data=await res.json();return data?.[0]?.data||[];}catch(e){return[];}}
function exchSuffix(ec){const M={'SG':'.SI','LN':'.L','HK':'.HK','GY':'.DE','FP':'.PA','SM':'.MC','IM':'.MI','AV':'.VI','NA':'.AS','BB':'.BR','SW':'.SW','AU':'.AX','JP':'.T','KS':'.KS','TW':'.TW','US':'','UN':'','UA':'','UW':'','UR':'','UT':'','UP':''};return Object.prototype.hasOwnProperty.call(M,ec)?M[ec]:undefined;}
async function resolvePriceSource(inv){
  const ticker=(inv.ticker||'').trim().toUpperCase(),isin=(inv.isin||'').trim().toUpperCase();
  if(inv.resolvedTicker&&inv.resolvedTicker!==ticker){try{const r=await fetchPriceDirect(inv.resolvedTicker);return{...r,resolvedTicker:inv.resolvedTicker,resolvedBy:'cached'};}catch(e){inv.resolvedTicker=null;}}
  if(ticker){
    try{const r=await fetchPriceDirect(ticker);return{...r,resolvedTicker:ticker,resolvedBy:'ticker'};}catch(e){}
    if(/^\d{4,8}$/.test(ticker)){try{const r=await fetchPriceDirect(ticker+'.SI');return{...r,resolvedTicker:ticker+'.SI',resolvedBy:'SGX'};}catch(e){}}
    const yfT=await yfSearch(ticker);for(const q of yfT.slice(0,4)){try{const r=await fetchPriceDirect(q.symbol);return{...r,resolvedTicker:q.symbol,resolvedBy:'ticker-search'};}catch(e){}}
  }
  if(isin){
    const yfI=await yfSearch(isin);for(const q of yfI.slice(0,6)){try{const r=await fetchPriceDirect(q.symbol);return{...r,resolvedTicker:q.symbol,resolvedBy:'ISIN→YF'};}catch(e){}}
    const figiList=await openFIGILookup(isin);
    const pt=['Open-End Fund','Exchange-Traded Fund','Common Stock','Closed-End Fund'];
    figiList.sort((a,b)=>(pt.indexOf(a.securityType2||a.securityType||'')===-1?99:pt.indexOf(a.securityType2||a.securityType||''))-(pt.indexOf(b.securityType2||b.securityType||'')===-1?99:pt.indexOf(b.securityType2||b.securityType||'')));
    for(const figi of figiList.slice(0,10)){if(!figi.ticker)continue;const sfx=exchSuffix(figi.exchCode);if(sfx===undefined)continue;try{const r=await fetchPriceDirect(figi.ticker+sfx);return{...r,resolvedTicker:figi.ticker+sfx,resolvedBy:'ISIN→OpenFIGI'};}catch(e){}}
  }
  return null;
}
async function searchTicker(q){const j=await yf2Fetch(`/v1/finance/search?q=${encodeURIComponent(q)}&quotesCount=8&newsCount=0&enableFuzzyQuery=true&enableCb=false`);return(j?.quotes||[]).filter(x=>x.symbol&&x.quoteType&&!['FUTURE','OPTION','CURRENCY'].includes(x.quoteType)).map(x=>({symbol:x.symbol,name:x.shortname||x.longname||x.symbol,exchange:x.exchange||x.fullExchangeName||'',type:x.typeDisp||x.quoteType||''}));}
function resolvedByLabel(by){return{'ticker':'Ticker','SGX':'SGX (.SI)','ticker-search':'Ticker search','ISIN→YF':'ISIN via Yahoo','ISIN→OpenFIGI':'ISIN via OpenFIGI','cached':'Cached'}[by]||by||'';}

// ─── State ────────────────────────────────────────────────────────────────
let S={investments:[],dividends:[],tab:'portfolio',fCcy:'All',fAcc:'All',
       modal:null,editId:null,txId:null,txEditInvId:null,txEditId:null,divEditId:null,
       refreshing:{},refreshingAll:false,acResults:[],acLoading:false,acQuery:'',acTimer:null,
       searchQuery:'',showAllTx:{},catOpen:true};
try{const d=JSON.parse(localStorage.getItem('it2025')||'{}');S.investments=d.i||[];S.dividends=d.d||[];}catch(e){}
function save(){try{localStorage.setItem('it2025',JSON.stringify({i:S.investments,d:S.dividends}));}catch(e){}}
function uid(){return Date.now().toString(36)+Math.random().toString(36).slice(2);}
function fmt(n,dp=2){if(n==null||isNaN(n))return'–';return Number(n).toLocaleString('en-SG',{minimumFractionDigits:dp,maximumFractionDigits:dp});}
function fc(n,c,dp=2){const m={SGD:'S$',USD:'US$',GBP:'£',EUR:'€',HKD:'HK$'};const s=m[c]||(c?c+' ':'');if(n==null||isNaN(n))return s+'–';return(n<0?'-':'')+s+Math.abs(n).toLocaleString('en-SG',{minimumFractionDigits:dp,maximumFractionDigits:dp});}
function toast(msg,dur=3500){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');clearTimeout(t._t);t._t=setTimeout(()=>t.classList.remove('show'),dur);}
function tsAgo(ts){if(!ts)return'';const m=Math.round((Date.now()-ts)/60000);if(m<1)return'just now';if(m<60)return m+'m ago';const h=Math.round(m/60);return h<24?h+'h ago':Math.round(h/24)+'d ago';}

// ─── Price Refresh ────────────────────────────────────────────────────────
async function refreshOne(invId){
  const inv=S.investments.find(i=>i.id===invId);if(!inv)return;
  if(!inv.ticker&&!inv.isin){toast('⚠️ Add a ticker or ISIN first.');return;}
  S.refreshing[invId]=true;render();
  try{const r=await resolvePriceSource(inv);if(r&&r.price){inv.spotPrice=r.price;inv.priceChange=r.change;inv.priceChangePct=r.changePct;inv.priceUpdated=Date.now();inv.priceAuto=true;inv.resolvedTicker=r.resolvedTicker||null;inv.resolvedBy=r.resolvedBy||null;save();toast('✅ '+inv.name+': '+fc(r.price,inv.currency,3)+' via '+resolvedByLabel(r.resolvedBy));}else{inv.priceAuto=false;save();toast('⚠️ '+inv.name+': No price found. Enter manually.');}}catch(e){toast('⚠️ '+e.message);}
  S.refreshing[invId]=false;render();
}
async function refreshAll(){
  const list=S.investments.filter(i=>i.ticker||i.isin);if(!list.length){toast('No tickers or ISINs set.');return;}
  S.refreshingAll=true;render();let ok=0,fail=0;
  await Promise.all(list.map(async inv=>{S.refreshing[inv.id]=true;try{const r=await resolvePriceSource(inv);if(r&&r.price){inv.spotPrice=r.price;inv.priceChange=r.change;inv.priceChangePct=r.changePct;inv.priceUpdated=Date.now();inv.priceAuto=true;inv.resolvedTicker=r.resolvedTicker||null;inv.resolvedBy=r.resolvedBy||null;ok++;}else{fail++;}}catch(e){fail++;}S.refreshing[inv.id]=false;}));
  S.refreshingAll=false;save();toast('✅ '+ok+' updated'+(fail?' · ⚠️ '+fail+' not found':''));render();
}
setInterval(()=>{if(!document.hidden&&S.investments.some(i=>i.ticker||i.isin))refreshAll();},5*60*1000);
document.addEventListener('visibilitychange',()=>{if(!document.hidden&&S.investments.some(i=>i.ticker||i.isin))refreshAll();});

// ─── Autocomplete ─────────────────────────────────────────────────────────
function onNameInput(val){clearTimeout(S.acTimer);S.acQuery=val;S.acResults=[];if(val.length<2){S.acLoading=false;renderAC();return;}S.acLoading=true;renderAC();S.acTimer=setTimeout(async()=>{try{const res=await searchTicker(val);if(S.acQuery===val){S.acResults=res;S.acLoading=false;renderAC();}}catch(e){S.acLoading=false;renderAC();}},400);}
function renderAC(){const drop=document.getElementById('ac-drop');if(!drop)return;if(S.acLoading){drop.innerHTML='<div class="ac-loading"><span class="spin"></span> Searching…</div>';drop.style.display='block';return;}if(!S.acResults.length){drop.style.display='none';return;}drop.innerHTML=S.acResults.map((r,i)=>`<div class="ac-item" onclick="pickAC(${i})"><div style="display:flex;justify-content:space-between"><span class="ac-name">${r.name}</span><span class="ac-sym">${r.symbol}</span></div><span class="ac-ex">${r.exchange} · ${r.type}</span></div>`).join('');drop.style.display='block';}
function pickAC(i){const r=S.acResults[i];const ne=document.getElementById('fn'),te=document.getElementById('ftk');if(ne&&!ne.value)ne.value=r.name;if(te)te.value=r.symbol;const excCcy={NMS:'USD',NYQ:'USD',NGM:'USD',LSE:'GBP',SES:'SGD',HKG:'HKD',FRA:'EUR',EPA:'EUR'};const ccy=excCcy[r.exchange]||'';if(ccy){const el=document.getElementById('fc2');if(el)el.value=ccy;}const excMkt={NMS:'NASDAQ',NYQ:'NYSE',NGM:'NASDAQ',LSE:'LSE',SES:'SGX',HKG:'HKEX'};const mkt=excMkt[r.exchange]||'Other';const mel=document.getElementById('fm');if(mel)mel.value=mkt;S.acResults=[];S.acLoading=false;const drop=document.getElementById('ac-drop');if(drop)drop.style.display='none';}

// ─── Calc ─────────────────────────────────────────────────────────────────
function calc(inv){
  let sh=0,cost=0;
  for(const t of(inv.transactions||[])){const u=+t.shares||0,p=+t.price||0,f=+t.fees||0;if(t.type==='Buy'){cost+=u*p+f;sh+=u;}else if(t.type==='Sell'){const ac=sh>0?cost/sh:0;cost-=u*ac;sh-=u;}else if(t.type==='Rights Issue'){cost+=u*p+f;sh+=u;}else if(t.type==='Free Issue'){sh+=u;}}
  const avg=sh>0?cost/sh:0,spot=+inv.spotPrice||0,mv=spot*sh,pl=mv-cost,plp=cost>0?(pl/cost)*100:0;
  const myDivs=S.dividends.filter(d=>d.invId===inv.id);
  const dbf=myDivs.filter(d=>d.date&&d.date.slice(0,4)<CY).reduce((s,d)=>s+(+d.amount||0),0);
  const dcur=myDivs.filter(d=>d.date&&d.date.startsWith(CY)).reduce((s,d)=>s+(+d.amount||0),0);
  const dtot=dbf+dcur,dy=cost>0?(dcur/cost)*100:0,dytot=cost>0?(dtot/cost)*100:0;
  return{sh,cost,avg,spot,mv,pl,plp,dbf,dcur,dtot,dy,dytot};
}

// ─── Category Breakdown ───────────────────────────────────────────────────
function buildCatData(){
  const typeKeys=['ETF','Stock','Unit Trust','Bond','Other'];
  const byType={};
  typeKeys.forEach(k=>{byType[k]={cost:0,mv:0,pl:0,dbf:0,dcur:0,dtot:0,count:0};});
  S.investments.forEach(inv=>{
    const s=calc(inv);
    const k=typeKeys.includes(inv.type)?inv.type:'Other';
    byType[k].cost+=s.cost; byType[k].mv+=s.mv; byType[k].pl+=s.pl;
    byType[k].dbf+=s.dbf; byType[k].dcur+=s.dcur; byType[k].dtot+=s.dtot;
    byType[k].count++;
  });
  return{byType,typeKeys:typeKeys.filter(k=>byType[k].count>0)};
}

function renderCatBreakdown(tC,tM,tP,tDbf,tDcur,tDtot){
  const {byType,typeKeys}=buildCatData();
  if(!typeKeys.length)return'';
  const open=S.catOpen;
  return`
<div class="cat-panel">
  <div class="cat-panel-hdr" onclick="toggleCat()">
    <div class="cat-panel-hdr-left">
      📂 <span>Breakdown by Category</span>
      <span style="font-size:10px;font-weight:500;color:var(--gray);margin-left:4px">${typeKeys.length} type${typeKeys.length>1?'s':''}</span>
    </div>
    <span class="cat-chevron ${open?'open':''}">▼</span>
  </div>
  ${open?`
  <div class="cat-body">
    <div class="cat-col-hdr">
      <span>Category</span>
      <span>Cost</span>
      <span>Mkt Val</span>
      <span>P&L</span>
      <span>Div Total</span>
    </div>
    ${typeKeys.map(k=>{
      const d=byType[k];
      const mvPct=tM>0?(d.mv/tM*100):0;
      const plColor=d.pl>=0?'pos':'neg';
      return`<div class="cat-row">
        <div class="cat-row-label">
          <div class="cat-dot" style="background:${TYPE_COLORS[k]}"></div>
          <div>
            <div class="cat-row-name" style="color:${TYPE_FG[k]}">${k}</div>
            <div class="cat-row-sub">${d.count} holding${d.count>1?'s':''}</div>
            <div class="cat-bar-wrap"><div class="cat-bar" style="width:${mvPct}%;background:${TYPE_COLORS[k]}"></div></div>
          </div>
        </div>
        <div class="cat-cell">
          <div class="cv">${fc(d.cost,'SGD',0)}</div>
          <div class="cs">${tC>0?fmt(d.cost/tC*100,1):'0'}%</div>
        </div>
        <div class="cat-cell">
          <div class="cv">${fc(d.mv,'SGD',0)}</div>
          <div class="cs">${fmt(mvPct,1)}%</div>
        </div>
        <div class="cat-cell">
          <div class="cv ${plColor}">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div>
          <div class="cs ${plColor}">${d.pl>=0?'+':''}${d.cost>0?fmt(d.pl/d.cost*100,2):'0'}%</div>
        </div>
        <div class="cat-cell">
          <div class="cv divv">${fc(d.dtot,'SGD',0)}</div>
          <div class="cs" style="color:#0369a1">B/F ${fc(d.dbf,'SGD',0)}</div>
          <div class="cs pos">${CY} ${fc(d.dcur,'SGD',0)}</div>
        </div>
      </div>`;
    }).join('')}
    <!-- Totals row -->
    <div class="cat-total-row">
      <div><div class="cv" style="font-size:11px;color:var(--gray)">TOTAL</div></div>
      <div class="cat-cell"><div class="cv">${fc(tC,'SGD',0)}</div></div>
      <div class="cat-cell"><div class="cv">${fc(tM,'SGD',0)}</div></div>
      <div class="cat-cell"><div class="cv ${tP>=0?'pos':'neg'}">${tP>=0?'+':''}${fc(tP,'SGD',0)}</div></div>
      <div class="cat-cell"><div class="cv divv">${fc(tDtot,'SGD',0)}</div></div>
    </div>
  </div>`:''}
</div>`;}

// ─── Pie Chart ────────────────────────────────────────────────────────────
function makePie(slices,size=140){
  const total=slices.reduce((s,x)=>s+x.value,0);
  if(!total)return'<div style="text-align:center;color:var(--gray);font-size:13px;padding:20px">No data yet</div>';
  const cx=size/2,cy=size/2,r=size/2-6,ri=r*0.52;
  let paths='',ang=-Math.PI/2;
  for(const sl of slices){const a=(sl.value/total)*2*Math.PI,ea=ang+a;const x1=cx+r*Math.cos(ang),y1=cy+r*Math.sin(ang),x2=cx+r*Math.cos(ea),y2=cy+r*Math.sin(ea);const xi1=cx+ri*Math.cos(ang),yi1=cy+ri*Math.sin(ang),xi2=cx+ri*Math.cos(ea),yi2=cy+ri*Math.sin(ea);const lg=a>Math.PI?1:0;paths+=`<path d="M${xi1},${yi1}L${x1},${y1}A${r},${r} 0 ${lg},1 ${x2},${y2}L${xi2},${yi2}A${ri},${ri} 0 ${lg},0 ${xi1},${yi1}" fill="${sl.color}" stroke="#fff" stroke-width="2"/>`;ang=ea;}
  return`<svg width="${size}" height="${size}" viewBox="0 0 ${size} ${size}" class="pie-svg">${paths}</svg>`;
}

// ─── Year-end Banner ──────────────────────────────────────────────────────
function yearEndBanner(){
  if(CMONTH<10)return'';
  const allStats=S.investments.map(i=>calc(i));
  const totalDiv=allStats.reduce((s,x)=>s+x.dtot,0),curDiv=allStats.reduce((s,x)=>s+x.dcur,0);
  const daysLeft=Math.ceil((new Date(NOW.getFullYear(),11,31)-NOW)/(1000*60*60*24));
  return`<div class="yr-banner"><div class="yb-ico">🔔</div><div class="yb-txt"><b>Year-end: ${daysLeft} day${daysLeft!==1?'s':''} left in ${CY}</b><br>On <b>1 Jan ${NY}</b>, ${CY} dividends (<b>${fc(curDiv,'SGD')}</b>) auto-become Accumulated B/F for ${NY}. Total carry-forward: <b>${fc(totalDiv,'SGD')}</b>.</div></div>`;
}

// ─── Render ───────────────────────────────────────────────────────────────
function render(){
  const q=S.searchQuery.trim().toLowerCase();
  const invs=S.investments.filter(i=>{
    if(S.fCcy!=='All'&&i.currency!==S.fCcy)return false;
    if(S.fAcc!=='All'&&i.account!==S.fAcc)return false;
    if(q&&!i.name.toLowerCase().includes(q)&&!(i.ticker||'').toLowerCase().includes(q)&&!(i.isin||'').toLowerCase().includes(q))return false;
    return true;
  });
  const all=S.investments.map(i=>calc(i));
  const tC=all.reduce((s,x)=>s+x.cost,0),tM=all.reduce((s,x)=>s+x.mv,0),tP=tM-tC,
        tDbf=all.reduce((s,x)=>s+x.dbf,0),tDcur=all.reduce((s,x)=>s+x.dcur,0),tDtot=all.reduce((s,x)=>s+x.dtot,0);
  document.getElementById('app').innerHTML=`
<div class="header">
  <div style="display:flex;justify-content:space-between;align-items:center">
    <div><h1>📊 InvestTracker</h1><div class="sub">${NOW.toLocaleDateString('en-SG',{weekday:'short',day:'numeric',month:'short',year:'numeric'})}</div></div>
    <button class="btn raf" onclick="refreshAll()" ${S.refreshingAll?'disabled':''}>
      ${S.refreshingAll?'<span class="spin"></span>':'🔄'} ${S.refreshingAll?'Updating…':'Refresh All'}
    </button>
  </div>
  <div class="tabs">
    <button class="tab ${S.tab==='portfolio'?'active':''}" onclick="setTab('portfolio')">Portfolio</button>
    <button class="tab ${S.tab==='dividends'?'active':''}" onclick="setTab('dividends')">Dividends</button>
    <button class="tab ${S.tab==='summary'?'active':''}" onclick="setTab('summary')">Summary</button>
  </div>
</div>
${S.tab==='portfolio'?renderPort(invs,tC,tM,tP,tDbf,tDcur,tDtot):S.tab==='dividends'?renderDivs():renderSummary()}
${renderModal()}`;
  document.querySelectorAll('.spi').forEach(el=>el.addEventListener('change',function(){
    const inv=S.investments.find(i=>i.id===this.dataset.id);
    if(inv){inv.spotPrice=parseFloat(this.value)||0;inv.priceAuto=false;inv.priceUpdated=Date.now();save();render();}
  }));
  const si=document.getElementById('search-input');
  if(si)si.addEventListener('input',function(){S.searchQuery=this.value;renderPortOnly();});
}

function renderPortOnly(){
  const q=S.searchQuery.trim().toLowerCase();
  const invs=S.investments.filter(i=>{
    if(S.fCcy!=='All'&&i.currency!==S.fCcy)return false;
    if(S.fAcc!=='All'&&i.account!==S.fAcc)return false;
    if(q&&!i.name.toLowerCase().includes(q)&&!(i.ticker||'').toLowerCase().includes(q)&&!(i.isin||'').toLowerCase().includes(q))return false;
    return true;
  });
  const el=document.getElementById('holdings-list');if(!el)return;
  const countEl=document.getElementById('holdings-count');if(countEl)countEl.textContent=`Holdings (${invs.length})`;
  el.innerHTML=invs.length===0
    ?`<div class="${q?'no-results':'empty'}">${q?`No investments match "<b>${q}</b>"`:'<div class="ico">📈</div><div>No investments yet.</div>'}</div>`
    :invs.map(inv=>renderCard(inv)).join('');
  document.querySelectorAll('.spi').forEach(el=>el.addEventListener('change',function(){
    const inv=S.investments.find(i=>i.id===this.dataset.id);
    if(inv){inv.spotPrice=parseFloat(this.value)||0;inv.priceAuto=false;inv.priceUpdated=Date.now();save();renderPortOnly();}
  }));
}

// ─── Portfolio Page ───────────────────────────────────────────────────────
function renderPort(invs,tC,tM,tP,tDbf,tDcur,tDtot){
  return`
<!-- ① Overall summary strip -->
<div class="summary">
  <div class="scard"><div class="lbl">Total Cost</div><div class="val">${fc(tC,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Mkt Value</div><div class="val">${fc(tM,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">P&L</div><div class="val ${tP>=0?'pos':'neg'}">${tP>=0?'+':''}${fc(tP,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div B/F</div><div class="val" style="color:#0369a1">${fc(tDbf,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div ${CY}</div><div class="val pos">${fc(tDcur,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div Total</div><div class="val" style="color:var(--purple)">${fc(tDtot,'SGD',0)}</div></div>
</div>

<!-- ② Category breakdown panel -->
${renderCatBreakdown(tC,tM,tP,tDbf,tDcur,tDtot)}

<!-- ③ Year-end banner (Nov/Dec only) -->
${yearEndBanner()}

<!-- ④ Search -->
<div class="search-bar">
  <input id="search-input" placeholder="🔍  Search investments…" value="${S.searchQuery}" autocomplete="off">
  ${S.searchQuery?`<button class="sclear" onclick="clearSearch()">✕</button>`:''}
</div>

<!-- ⑤ Currency / Account filters -->
<div class="filter-bar">
  ${['All','SGD','USD','GBP','EUR','HKD'].map(c=>`<button class="chip ${S.fCcy===c?'on':''}" onclick="setF('c','${c}')">${c}</button>`).join('')}
  <span style="width:1px;background:#e5e5ea;flex-shrink:0"></span>
  ${['All','Cash','SRS','CPF'].map(a=>`<button class="chip ${S.fAcc===a?'on':''}" onclick="setF('a','${a}')">${a}</button>`).join('')}
</div>

<!-- ⑥ Individual holdings -->
<div class="section">
  <div class="sec-hdr">
    <span class="sec-ttl" id="holdings-count">Holdings (${invs.length})</span>
    <button class="btn sm" onclick="openM('addInv')">+ Add</button>
  </div>
  <div id="holdings-list">
    ${invs.length===0
      ?`<div class="${S.searchQuery?'no-results':'empty'}">${S.searchQuery?`No investments match "<b>${S.searchQuery}</b>"`:'<div class="ico">📈</div><div>No investments yet.<br>Tap <b>+ Add</b> to begin.</div>'}</div>`
      :invs.map(inv=>renderCard(inv)).join('')}
  </div>
</div>`;}

function renderCard(inv){
  const s=calc(inv);const loading=S.refreshing[inv.id];
  const chg=inv.priceChange||0,chgP=inv.priceChangePct||0,cc=chg>=0?'var(--green)':'var(--red)';
  let srcBadge='';
  if(inv.priceAuto&&inv.spotPrice){if(inv.resolvedBy==='ISIN→OpenFIGI')srcBadge='<span class="fbadge">FIGI</span>';else if(inv.resolvedBy&&inv.resolvedBy.includes('ISIN'))srcBadge='<span class="ibadge">ISIN</span>';else srcBadge='<span class="abadge">LIVE</span>';}else{srcBadge='<span class="mbadge">MANUAL</span>';}
  const txs=inv.transactions||[];const showAll=S.showAllTx[inv.id];const displayTxs=showAll?[...txs].reverse():[...txs].reverse().slice(0,3);
  return`<div class="card" id="card-${inv.id}">
  <div class="inv-top">
    <div class="inv-meta">
      <div class="inv-name">${inv.name||'Unnamed'}</div>
      <div class="inv-tkr">${inv.ticker?`<b>${inv.ticker}</b>`:'No ticker'}${inv.isin?` · <span style="color:var(--blue)">${inv.isin}</span>`:''} · ${inv.market||'–'}</div>
      <div class="badges">
        <span class="badge">${inv.type||'Stock'}</span>
        <span class="badge ${(inv.currency||'').toLowerCase()}">${inv.currency||'–'}</span>
        <span class="badge ${(inv.account||'').toLowerCase()}">${inv.account||'–'}</span>
      </div>
    </div>
    <div class="price-box">
      <div class="price-lbl">${srcBadge}<button class="btn sm out" style="padding:2px 8px;font-size:10px" onclick="refreshOne('${inv.id}')" ${loading?'disabled':''}>${loading?'<span class="spin"></span>':'🔄'}</button></div>
      ${inv.spotPrice?`<div class="price-big">${fc(inv.spotPrice,inv.currency,3)}</div>${inv.priceAuto?`<div class="price-chg" style="color:${cc}">${chg>=0?'+':''}${fc(chg,inv.currency,3)} (${chg>=0?'+':''}${fmt(chgP,2)}%)</div>`:''}<div class="price-ts">${tsAgo(inv.priceUpdated)}</div>${inv.resolvedTicker&&inv.resolvedTicker!==(inv.ticker||'').toUpperCase()?`<div class="price-resolved">via ${inv.resolvedTicker}</div>`:''}`:`<div style="font-size:11px;color:var(--gray);margin-top:6px">Tap 🔄 to fetch</div>`}
      <div class="manual-row"><input class="spot-inp spi" data-id="${inv.id}" type="number" step="0.0001" value="${inv.spotPrice||''}" placeholder="Manual"></div>
    </div>
  </div>
  ${inv.resolvedBy&&inv.resolvedBy.includes('ISIN')?`<div class="src-info">ℹ️ ISIN <b>${inv.isin}</b> → <b>${inv.resolvedTicker||''}</b> (${resolvedByLabel(inv.resolvedBy)})</div>`:''}
  <div class="grid2">
    <div class="stat"><div class="sl">Units Held</div><div class="sv">${fmt(s.sh,4)}</div></div>
    <div class="stat"><div class="sl">Avg Cost</div><div class="sv">${fc(s.avg,inv.currency,4)}</div></div>
    <div class="stat"><div class="sl">Total Cost</div><div class="sv">${fc(s.cost,inv.currency)}</div></div>
    <div class="stat"><div class="sl">Mkt Value</div><div class="sv">${fc(s.mv,inv.currency)}</div></div>
    <div class="stat"><div class="sl">Unreal. P&L</div><div class="sv ${s.pl>=0?'pos':'neg'}">${s.pl>=0?'+':''}${fc(s.pl,inv.currency)}<br><small>${s.pl>=0?'+':''}${fmt(s.plp,2)}%</small></div></div>
    <div class="stat"><div class="sl">Breakeven</div><div class="sv" style="font-size:11px">${fc(s.avg,inv.currency,4)}<br><small style="color:var(--gray)">avg cost/unit</small></div></div>
  </div>
  <div class="div-strip">
    <div class="div-strip-title">💰 Dividends Received</div>
    <div class="div-strip-grid">
      <div class="div-col"><div class="dc-lbl">Accum. B/F</div><div class="dc-val" style="color:#0369a1">${fc(s.dbf,inv.currency)}</div><div class="dc-sub">Before ${CY}</div></div>
      <div class="div-col"><div class="dc-lbl">Year ${CY}</div><div class="dc-val">${fc(s.dcur,inv.currency)}</div><div class="dc-sub">This year</div></div>
      <div class="div-col"><div class="dc-lbl">Accum. To-Date</div><div class="dc-val" style="color:var(--purple)">${fc(s.dtot,inv.currency)}</div><div class="dc-sub">All time</div></div>
    </div>
    <div class="yield-row">
      <div class="yield-item"><div class="yl">Yield ${CY}</div><div class="yv" style="color:var(--green)">${fmt(s.dy,2)}%</div></div>
      <div class="yield-item" style="border-left:1px solid #d1fae5;border-right:1px solid #d1fae5"><div class="yl">Yield Total</div><div class="yv" style="color:var(--purple)">${fmt(s.dytot,2)}%</div></div>
      <div class="yield-item"><div class="yl">Entries</div><div class="yv" style="color:var(--gray)">${S.dividends.filter(d=>d.invId===inv.id).length}</div></div>
    </div>
  </div>
  <div class="actions">
    <button class="btn sm out" onclick="openTx('${inv.id}')">+ Transaction</button>
    <button class="btn sm out" onclick="openDiv('${inv.id}')">+ Dividend</button>
    <button class="btn sm out" onclick="openEdit('${inv.id}')">Edit</button>
    <button class="btn sm red" onclick="delInv('${inv.id}')">Delete</button>
  </div>
  ${txs.length>0?`
  <div class="tx-section">
    <div class="tx-hdr-row">
      <div class="tx-hdr">Transactions (${txs.length})</div>
      ${txs.length>3?`<button class="btn xs out" onclick="toggleTxAll('${inv.id}')">${showAll?'Show less':'Show all'}</button>`:''}
    </div>
    ${displayTxs.map(t=>`
    <div class="tx-item">
      <div class="tx-left"><span class="tx-t ${t.type.toLowerCase().replace(' issue','').replace(' ','')}">${t.type}</span><span style="color:var(--gray);margin-left:6px">${t.date||''}</span>${t.notes?`<div style="font-size:10px;color:var(--gray);margin-top:1px">${t.notes}</div>`:''}</div>
      <div class="tx-right"><div>${fmt(t.shares,4)} units</div>${t.price?`<div style="color:var(--gray);font-size:11px">@ ${fc(t.price,inv.currency,4)}</div>`:''}${t.fees?`<div style="color:var(--gray);font-size:10px">fee ${fc(t.fees,inv.currency,2)}</div>`:''}</div>
      <div class="tx-btns"><button class="btn xs out" onclick="openEditTx('${inv.id}','${t.id}')">✏️</button><button class="btn xs red" onclick="delTx('${inv.id}','${t.id}')">🗑</button></div>
    </div>`).join('')}
  </div>`:''}
</div>`;}

// ─── Dividend Ledger ──────────────────────────────────────────────────────
function renderDivs(){
  const divs=S.dividends.slice().sort((a,b)=>(b.date||'').localeCompare(a.date||''));
  const allStats=S.investments.map(i=>calc(i));
  const totDbf=allStats.reduce((s,x)=>s+x.dbf,0),totDcur=allStats.reduce((s,x)=>s+x.dcur,0),totDtot=allStats.reduce((s,x)=>s+x.dtot,0);
  const byInv={};divs.forEach(d=>{if(!byInv[d.invId])byInv[d.invId]=[];byInv[d.invId].push(d);});
  return`
<div class="summary">
  <div class="scard"><div class="lbl">Accum. B/F</div><div class="val" style="color:#0369a1">${fc(totDbf,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Year ${CY}</div><div class="val pos">${fc(totDcur,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Accum. Total</div><div class="val" style="color:var(--purple)">${fc(totDtot,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Records</div><div class="val">${divs.length}</div></div>
</div>
${yearEndBanner()}
<div class="section" style="padding-top:12px">
  <div class="sec-hdr"><span class="sec-ttl">Dividend Ledger</span><button class="btn sm grn" onclick="openDivG()">+ Add</button></div>
  ${divs.length===0?`<div class="empty"><div class="ico">💰</div><div>No dividends recorded yet.</div></div>`:''}
  ${Object.entries(byInv).map(([id,ds])=>{
    const inv=S.investments.find(i=>i.id===id);
    const nm=inv?.name||'Unknown',ccy=inv?.currency||'SGD';
    const bf=ds.filter(d=>d.date&&d.date.slice(0,4)<CY).reduce((s,d)=>s+(+d.amount||0),0);
    const cur=ds.filter(d=>d.date&&d.date.startsWith(CY)).reduce((s,d)=>s+(+d.amount||0),0);
    const tot=bf+cur;
    const byYr={};ds.forEach(d=>{const y=(d.date||'????').slice(0,4);if(!byYr[y])byYr[y]=[];byYr[y].push(d);});
    return`<div class="ledger-inv">
      <div class="ledger-inv-hdr"><span>${nm} <span style="font-size:11px;color:var(--gray);font-weight:500">${inv?.ticker||''}</span></span></div>
      <div class="ledger-summary">
        <div class="ls-box bf"><div class="ls-lbl">Accum. B/F</div><div class="ls-val">${fc(bf,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">Before ${CY}</div></div>
        <div class="ls-box"><div class="ls-lbl">Year ${CY}</div><div class="ls-val">${fc(cur,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">Current year</div></div>
        <div class="ls-box tot"><div class="ls-lbl">Accum. Total</div><div class="ls-val">${fc(tot,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">All time</div></div>
      </div>
      ${Object.entries(byYr).sort((a,b)=>b[0].localeCompare(a[0])).map(([yr,yds])=>{
        const ytot=yds.reduce((s,d)=>s+(+d.amount||0),0);
        return`<div class="div-yr-block">
          <div class="div-yr-hdr"><span>📅 ${yr} (${yds.length})</span><span></span><span></span><span style="color:var(--green);font-weight:700">${fc(ytot,ccy)}</span><span></span></div>
          ${yds.map(d=>`<div class="div-yr-row">
            <span>${d.date||'–'}</span><span style="font-size:10px;color:var(--gray)">${d.divType||'Cash'}</span>
            <span style="font-weight:700;color:var(--green)">${fc(d.amount,ccy,4)}</span>
            <span style="color:var(--gray);font-size:10px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${d.notes||''}</span>
            <div style="display:flex;gap:4px;justify-content:flex-end">
              <button class="btn xs out" onclick="openEditDiv('${d.id}')">✏️</button>
              <button class="btn xs red" onclick="delDiv('${d.id}')">🗑</button>
            </div>
          </div>`).join('')}
        </div>`;}).join('')}
    </div>`;}).join('')}
</div>`;}

// ─── Summary Page ─────────────────────────────────────────────────────────
function renderSummary(){
  if(!S.investments.length)return`<div class="sumpage"><div class="empty" style="padding-top:60px"><div class="ico">📊</div><div>No investments yet.</div></div></div>`;
  const allStats=S.investments.map(inv=>({inv,s:calc(inv)}));
  const totalMv=allStats.reduce((s,x)=>s+x.s.mv,0),totalCost=allStats.reduce((s,x)=>s+x.s.cost,0),totalPl=totalMv-totalCost,totalDiv=allStats.reduce((s,x)=>s+x.s.dtot,0);
  const typeKeys=['ETF','Stock','Unit Trust','Bond','Other'];
  const byType={};typeKeys.forEach(k=>{byType[k]={mv:0,cost:0,count:0,pl:0,div:0};});
  allStats.forEach(({inv,s})=>{const k=typeKeys.includes(inv.type)?inv.type:'Other';byType[k].mv+=s.mv;byType[k].cost+=s.cost;byType[k].count++;byType[k].pl+=s.pl;byType[k].div+=s.dtot;});
  const typeSlices=typeKeys.filter(k=>byType[k].mv>0).map(k=>({label:k,value:byType[k].mv,color:TYPE_COLORS[k]}));
  const accKeys=['Cash','SRS','CPF'];
  const byAcc={};accKeys.forEach(k=>{byAcc[k]={mv:0,cost:0,count:0,pl:0,div:0};});
  allStats.forEach(({inv,s})=>{const k=accKeys.includes(inv.account)?inv.account:'Cash';byAcc[k].mv+=s.mv;byAcc[k].cost+=s.cost;byAcc[k].count++;byAcc[k].pl+=s.pl;byAcc[k].div+=s.dtot;});
  const accSlices=accKeys.filter(k=>byAcc[k].mv>0).map(k=>({label:k,value:byAcc[k].mv,color:ACC_COLORS[k]}));
  const profData={Profit:{mv:0,count:0},Loss:{mv:0,count:0},'Break-even':{mv:0,count:0}};
  allStats.forEach(({s})=>{const k=s.pl>0.01?'Profit':s.pl<-0.01?'Loss':'Break-even';profData[k].mv+=s.mv;profData[k].count++;});
  const profSlices=Object.entries(profData).filter(([,v])=>v.count>0).map(([k,v])=>({label:k,value:v.mv,color:PROF_COLORS[k]}));
  const topHoldings=[...allStats].sort((a,b)=>b.s.mv-a.s.mv).slice(0,6),maxMv=topHoldings[0]?.s.mv||1;
  return`
<div class="summary">
  <div class="scard"><div class="lbl">Total Value</div><div class="val">${fc(totalMv,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total Cost</div><div class="val">${fc(totalCost,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total P&L</div><div class="val ${totalPl>=0?'pos':'neg'}">${totalPl>=0?'+':''}${fc(totalPl,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total Div</div><div class="val" style="color:var(--purple)">${fc(totalDiv,'SGD',0)}</div></div>
</div>
<div class="sumpage">
  <div class="sum-section">
    <div class="sum-section-title">📂 By Investment Type</div>
    <div class="pie-wrap"><div class="pie-title">Market Value Allocation</div><div class="pie-body">${makePie(typeSlices)}<div class="pie-legend">${typeSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card">
      <div class="bk-hdr" style="grid-template-columns:1.4fr 1fr 1fr 1fr"><span>Type</span><span>Mkt Val</span><span>P&L</span><span>Div</span></div>
      ${typeKeys.filter(k=>byType[k].count>0).map(k=>{const d=byType[k];return`<div class="bk-row" style="grid-template-columns:1.4fr 1fr 1fr 1fr"><div class="bk-label"><div class="bk-dot" style="background:${TYPE_COLORS[k]}"></div>${k}</div><div>${fc(d.mv,'SGD',0)}<div style="font-size:10px;color:var(--gray)">${d.count} holding${d.count>1?'s':''}</div></div><div style="color:${d.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div><div style="color:var(--purple);font-weight:700">${fc(d.div,'SGD',0)}</div></div>`;}).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">💳 By Payment Mode</div>
    <div class="pie-wrap"><div class="pie-title">Market Value by Account</div><div class="pie-body">${makePie(accSlices)}<div class="pie-legend">${accSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card">
      <div class="bk-hdr" style="grid-template-columns:1.1fr 1fr 1fr 1fr"><span>Account</span><span>Mkt Val</span><span>P&L</span><span>Div</span></div>
      ${accKeys.filter(k=>byAcc[k].count>0).map(k=>{const d=byAcc[k];return`<div class="bk-row" style="grid-template-columns:1.1fr 1fr 1fr 1fr"><div class="bk-label"><div class="bk-dot" style="background:${ACC_COLORS[k]}"></div>${k}</div><div>${fc(d.mv,'SGD',0)}<div style="font-size:10px;color:var(--gray)">${d.count} holding${d.count>1?'s':''}</div></div><div style="color:${d.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div><div style="color:var(--purple);font-weight:700">${fc(d.div,'SGD',0)}</div></div>`;}).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">📈 Profitability</div>
    <div class="pie-wrap"><div class="pie-title">Portfolio Value by P&L Status</div><div class="pie-body">${makePie(profSlices)}<div class="pie-legend">${profSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card">
      <div class="bk-hdr" style="grid-template-columns:1.8fr 1fr 1fr"><span>Investment</span><span>P&L</span><span>P&L %</span></div>
      ${[...allStats].sort((a,b)=>b.s.pl-a.s.pl).map(({inv,s})=>`<div class="bk-row ${s.pl>=0?'prof-winner':'prof-loser'}" style="grid-template-columns:1.8fr 1fr 1fr"><div><div style="font-weight:600;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${inv.name}</div><div style="font-size:10px;color:var(--gray)">${inv.ticker||''} · ${inv.type}</div></div><div style="color:${s.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${s.pl>=0?'+':''}${fc(s.pl,inv.currency,0)}</div><div style="color:${s.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${s.pl>=0?'+':''}${fmt(s.plp,2)}%</div></div>`).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">🏆 Top Holdings</div>
    <div class="breakdown-card">
      ${topHoldings.map(({inv,s})=>{const pct=totalMv>0?s.mv/totalMv*100:0,bp=maxMv>0?s.mv/maxMv*100:0,tc=TYPE_COLORS[inv.type]||'#8e8e93';return`<div class="holding-row"><div style="flex:1;min-width:0"><div style="display:flex;justify-content:space-between;align-items:center"><div style="font-weight:600;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;max-width:60%">${inv.name}</div><div style="font-weight:700;font-size:13px">${fc(s.mv,inv.currency,0)}</div></div><div style="display:flex;justify-content:space-between;margin-top:2px"><div style="font-size:10px;color:var(--gray)">${inv.ticker||''} · <span style="color:${tc};font-weight:600">${inv.type}</span> · ${inv.account}</div><div style="font-size:10px;color:var(--gray)">${fmt(pct,1)}%</div></div><div class="holding-bar-bg"><div class="holding-bar-fill" style="width:${bp}%;background:${tc}"></div></div></div></div>`;}).join('')}
    </div>
  </div>
</div>`;}

// ─── Modals ───────────────────────────────────────────────────────────────
function renderModal(){
  if(!S.modal)return'';
  const ov=`class="overlay" onclick="bgClose(event)"`;
  if(S.modal==='addInv'||S.modal==='editInv'){
    const v=S.modal==='editInv'?S.investments.find(i=>i.id===S.editId):{};const isnew=S.modal==='addInv';
    return`<div ${ov}><div class="modal"><h2>${isnew?'Add Investment':'Edit Investment'}</h2>
      <div class="fg"><label>Search by Name or ISIN</label><input id="fsearch" placeholder="Type name, ISIN, ticker…" oninput="onNameInput(this.value)" autocomplete="off"><div id="ac-drop" class="ac-drop" style="display:none"></div><div class="hint">Tap result → ticker, currency & market auto-filled</div></div>
      <div class="fg"><label>Investment Name *</label><input id="fn" placeholder="e.g. Schroder Asian Growth" value="${v?.name||''}"></div>
      <div class="row2">
        <div class="fg"><label>Ticker / Bloomberg</label><input id="ftk" placeholder="e.g. VWRA.L or 580703" value="${v?.ticker||''}"><div class="hint">Numeric codes auto-get .SI</div></div>
        <div class="fg"><label>ISIN</label><input id="fis" placeholder="LU0943347566" value="${v?.isin||''}"><div class="hint">Fallback if ticker fails</div></div>
      </div>
      <div class="row2">
        <div class="fg"><label>Type</label><select id="fty">${['ETF','Stock','Unit Trust','Bond'].map(x=>`<option ${v?.type===x?'selected':''}>${x}</option>`).join('')}</select></div>
        <div class="fg"><label>Market</label><select id="fm">${['SGX','NYSE','NASDAQ','LSE','HKEX','Other'].map(x=>`<option ${v?.market===x?'selected':''}>${x}</option>`).join('')}</select></div>
      </div>
      <div class="row2">
        <div class="fg"><label>Currency</label><select id="fc2">${['SGD','USD','GBP','EUR','HKD'].map(x=>`<option ${v?.currency===x?'selected':''}>${x}</option>`).join('')}</select></div>
        <div class="fg"><label>Account</label><select id="fa">${['Cash','SRS','CPF'].map(x=>`<option ${v?.account===x?'selected':''}>${x}</option>`).join('')}</select></div>
      </div>
      <div class="fg"><label>Notes</label><input id="fno" placeholder="Optional" value="${v?.notes||''}"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn" onclick="saveInv()">${isnew?'Add & Fetch Price':'Save & Refresh'}</button></div>
    </div></div>`;}
  if(S.modal==='addTx'){
    const inv=S.investments.find(i=>i.id===S.txId);
    return`<div ${ov}><div class="modal"><h2>Add Transaction</h2><div class="modal-sub">${inv?.name||''} · ${inv?.currency||''}</div>
      <div class="fg"><label>Type *</label><select id="ftt">${['Buy','Sell','Rights Issue','Free Issue'].map(x=>`<option>${x}</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="ftd" type="date" value="${new Date().toISOString().slice(0,10)}"></div><div class="fg"><label>Units *</label><input id="fts" type="number" step="0.0001" placeholder="0.0000"></div></div>
      <div class="row2"><div class="fg"><label>Price / Unit</label><input id="ftp" type="number" step="0.0001" placeholder="0.0000"></div><div class="fg"><label>Fees</label><input id="ftf" type="number" step="0.01" placeholder="0.00"></div></div>
      <div class="fg"><label>Notes</label><input id="ftn" placeholder="Optional"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn" onclick="saveTx()">Add</button></div>
    </div></div>`;}
  if(S.modal==='editTx'){
    const inv=S.investments.find(i=>i.id===S.txEditInvId);const tx=(inv?.transactions||[]).find(t=>t.id===S.txEditId);if(!tx)return'';
    return`<div ${ov}><div class="modal"><h2>Edit Transaction</h2><div class="modal-sub">${inv?.name||''} · ${inv?.currency||''}</div>
      <div class="fg"><label>Type *</label><select id="ftt">${['Buy','Sell','Rights Issue','Free Issue'].map(x=>`<option ${tx.type===x?'selected':''}>${x}</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="ftd" type="date" value="${tx.date||''}"></div><div class="fg"><label>Units *</label><input id="fts" type="number" step="0.0001" value="${tx.shares||''}"></div></div>
      <div class="row2"><div class="fg"><label>Price / Unit</label><input id="ftp" type="number" step="0.0001" value="${tx.price||''}"></div><div class="fg"><label>Fees</label><input id="ftf" type="number" step="0.01" value="${tx.fees||''}"></div></div>
      <div class="fg"><label>Notes</label><input id="ftn" value="${tx.notes||''}" placeholder="Optional"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn amber" onclick="updateTx()">Save Changes</button></div>
    </div></div>`;}
  if(S.modal==='addDiv'){
    return`<div ${ov}><div class="modal"><h2>Add Dividend</h2>
      <div class="fg"><label>Investment *</label><select id="fdi">${S.investments.map(i=>`<option value="${i.id}" ${i.id===S.txId?'selected':''}>${i.name} (${i.ticker||i.isin||i.currency})</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="fdd" type="date" value="${new Date().toISOString().slice(0,10)}"></div><div class="fg"><label>Amount *</label><input id="fda" type="number" step="0.0001" placeholder="0.0000"></div></div>
      <div class="fg"><label>Type</label><select id="fdt">${['Cash','Scrip','Special','Return of Capital'].map(x=>`<option>${x}</option>`).join('')}</select></div>
      <div class="fg"><label>Notes</label><input id="fdn" placeholder="e.g. Q1 2025 dividend"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn grn" onclick="saveDiv()">Add</button></div>
    </div></div>`;}
  if(S.modal==='editDiv'){
    const dv=S.dividends.find(d=>d.id===S.divEditId);if(!dv)return'';const inv=S.investments.find(i=>i.id===dv.invId);
    return`<div ${ov}><div class="modal"><h2>Edit Dividend</h2><div class="modal-sub">${inv?.name||'Unknown'}</div>
      <div class="fg"><label>Investment</label><select id="fdi">${S.investments.map(i=>`<option value="${i.id}" ${i.id===dv.invId?'selected':''}>${i.name} (${i.ticker||i.currency})</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="fdd" type="date" value="${dv.date||''}"></div><div class="fg"><label>Amount *</label><input id="fda" type="number" step="0.0001" value="${dv.amount||''}"></div></div>
      <div class="fg"><label>Type</label><select id="fdt">${['Cash','Scrip','Special','Return of Capital'].map(x=>`<option ${dv.divType===x?'selected':''}>${x}</option>`).join('')}</select></div>
      <div class="fg"><label>Notes</label><input id="fdn" value="${dv.notes||''}" placeholder="e.g. Q1 2025 dividend"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn amber" onclick="updateDiv()">Save Changes</button></div>
    </div></div>`;}
  return'';}

// ─── Actions ──────────────────────────────────────────────────────────────
function setTab(t){S.tab=t;render();}
function setF(k,v){if(k==='c')S.fCcy=v;else S.fAcc=v;render();}
function clearSearch(){S.searchQuery='';render();}
function toggleTxAll(invId){S.showAllTx[invId]=!S.showAllTx[invId];render();}
function toggleCat(){S.catOpen=!S.catOpen;render();}
function openM(m){S.modal=m;S.editId=null;S.acResults=[];render();}
function openEdit(id){S.modal='editInv';S.editId=id;S.acResults=[];render();}
function openTx(id){S.modal='addTx';S.txId=id;render();}
function openDiv(id){S.modal='addDiv';S.txId=id;render();}
function openDivG(){S.modal='addDiv';S.txId=S.investments[0]?.id||null;render();}
function openEditTx(invId,txId){S.modal='editTx';S.txEditInvId=invId;S.txEditId=txId;render();}
function openEditDiv(divId){S.modal='editDiv';S.divEditId=divId;render();}
function closeM(){S.modal=null;S.editId=null;S.txId=null;S.txEditInvId=null;S.txEditId=null;S.divEditId=null;S.acResults=[];S.acLoading=false;render();}
function bgClose(e){if(e.target.classList.contains('overlay'))closeM();}
function saveInv(){
  const name=document.getElementById('fn').value.trim();if(!name){alert('Investment name is required.');return;}
  const ticker=document.getElementById('ftk').value.trim(),isin=document.getElementById('fis').value.trim();
  if(S.modal==='addInv'){
    const nv={id:uid(),name,ticker,isin,type:document.getElementById('fty').value,market:document.getElementById('fm').value,currency:document.getElementById('fc2').value,account:document.getElementById('fa').value,notes:document.getElementById('fno').value.trim(),spotPrice:0,transactions:[],resolvedTicker:null,resolvedBy:null};
    S.investments.push(nv);save();closeM();if(ticker||isin)setTimeout(()=>refreshOne(nv.id),300);
  }else{
    const inv=S.investments.find(i=>i.id===S.editId);
    if(inv){if(inv.ticker!==ticker||inv.isin!==isin){inv.resolvedTicker=null;inv.resolvedBy=null;}Object.assign(inv,{name,ticker,isin,type:document.getElementById('fty').value,market:document.getElementById('fm').value,currency:document.getElementById('fc2').value,account:document.getElementById('fa').value,notes:document.getElementById('fno').value.trim()});}
    save();closeM();if(inv&&(ticker||isin))setTimeout(()=>refreshOne(inv.id),300);
  }
}
function saveTx(){
  const sh=parseFloat(document.getElementById('fts').value);if(!sh||sh<=0){alert('Enter valid units.');return;}
  const inv=S.investments.find(i=>i.id===S.txId);if(!inv)return;if(!inv.transactions)inv.transactions=[];
  inv.transactions.push({id:uid(),type:document.getElementById('ftt').value,date:document.getElementById('ftd').value,shares:sh,price:parseFloat(document.getElementById('ftp').value)||0,fees:parseFloat(document.getElementById('ftf').value)||0,notes:document.getElementById('ftn').value.trim()});
  inv.transactions.sort((a,b)=>(a.date||'').localeCompare(b.date||''));save();closeM();
}
function updateTx(){
  const sh=parseFloat(document.getElementById('fts').value);if(!sh||sh<=0){alert('Enter valid units.');return;}
  const inv=S.investments.find(i=>i.id===S.txEditInvId);if(!inv)return;
  const tx=(inv.transactions||[]).find(t=>t.id===S.txEditId);if(!tx)return;
  Object.assign(tx,{type:document.getElementById('ftt').value,date:document.getElementById('ftd').value,shares:sh,price:parseFloat(document.getElementById('ftp').value)||0,fees:parseFloat(document.getElementById('ftf').value)||0,notes:document.getElementById('ftn').value.trim()});
  inv.transactions.sort((a,b)=>(a.date||'').localeCompare(b.date||''));save();toast('✅ Transaction updated');closeM();
}
function delTx(invId,txId){if(!confirm('Delete this transaction?'))return;const inv=S.investments.find(i=>i.id===invId);if(!inv)return;inv.transactions=(inv.transactions||[]).filter(t=>t.id!==txId);save();render();toast('🗑 Transaction deleted');}
function saveDiv(){
  const amt=parseFloat(document.getElementById('fda').value);if(!amt||amt<=0){alert('Enter valid amount.');return;}
  S.dividends.push({id:uid(),invId:document.getElementById('fdi').value,date:document.getElementById('fdd').value,amount:amt,divType:document.getElementById('fdt').value,notes:document.getElementById('fdn').value.trim()});
  save();closeM();
}
function updateDiv(){
  const amt=parseFloat(document.getElementById('fda').value);if(!amt||amt<=0){alert('Enter valid amount.');return;}
  const dv=S.dividends.find(d=>d.id===S.divEditId);if(!dv)return;
  Object.assign(dv,{invId:document.getElementById('fdi').value,date:document.getElementById('fdd').value,amount:amt,divType:document.getElementById('fdt').value,notes:document.getElementById('fdn').value.trim()});
  save();toast('✅ Dividend updated');closeM();
}
function delDiv(divId){if(!confirm('Delete this dividend entry?'))return;S.dividends=S.dividends.filter(d=>d.id!==divId);save();render();toast('🗑 Dividend deleted');}
function delInv(id){if(!confirm('Delete this investment and all its data?'))return;S.investments=S.investments.filter(i=>i.id!==id);S.dividends=S.dividends.filter(d=>d.invId!==id);save();render();}

// ─── Boot ─────────────────────────────────────────────────────────────────
render();
if(S.investments.some(i=>i.ticker||i.isin))setTimeout(refreshAll,1500);
</script>
</body>
</html>
!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="InvestTracker">
<title>InvestTracker</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
body{font-family:-apple-system,BlinkMacSystemFont,'SF Pro Text',sans-serif;background:#f2f2f7;color:#1c1c1e;max-width:430px;margin:0 auto;min-height:100vh}
:root{--blue:#007aff;--green:#34c759;--red:#ff3b30;--gray:#8e8e93;--card:#fff;--border:#e5e5ea;--purple:#af52de;--orange:#ff9500;--teal:#5ac8fa;--indigo:#5856d6}
.header{background:rgba(255,255,255,0.95);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);padding:52px 20px 10px;position:sticky;top:0;z-index:100;border-bottom:1px solid var(--border)}
.header h1{font-size:24px;font-weight:700}
.sub{font-size:11px;color:var(--gray);margin-top:1px}
.tabs{display:flex;background:#e5e5ea;border-radius:10px;padding:2px;margin:10px 0 0}
.tab{flex:1;padding:7px 0;border:none;background:transparent;border-radius:8px;font-size:12px;font-weight:500;color:var(--gray);cursor:pointer;transition:all .2s}
.tab.active{background:#fff;color:#1c1c1e;box-shadow:0 1px 4px rgba(0,0,0,.12)}
.search-bar{padding:10px 20px 4px;position:relative}
.search-bar input{width:100%;padding:9px 36px 9px 14px;border:1.5px solid var(--border);border-radius:12px;font-size:14px;background:#fff;outline:none}
.search-bar input:focus{border-color:var(--blue)}
.search-bar .sclear{position:absolute;right:30px;top:50%;transform:translateY(-50%);color:var(--gray);font-size:18px;cursor:pointer;padding:2px 4px;border:none;background:transparent;line-height:1}
.yr-banner{margin:8px 20px 0;background:linear-gradient(135deg,#fff7ed,#fef3c7);border:1.5px solid #fcd34d;border-radius:14px;padding:12px 14px;display:flex;gap:10px;align-items:flex-start}
.yr-banner .yb-ico{font-size:22px;flex-shrink:0}
.yr-banner .yb-txt{font-size:12px;color:#92400e;line-height:1.5}
.yr-banner .yb-txt b{color:#78350f}
/* ── Top Summary Strip ── */
.summary{display:flex;gap:9px;padding:13px 20px;overflow-x:auto;scrollbar-width:none}
.summary::-webkit-scrollbar{display:none}
.scard{min-width:108px;background:var(--card);border-radius:14px;padding:11px 12px;box-shadow:0 1px 3px rgba(0,0,0,.08);flex-shrink:0}
.scard .lbl{font-size:10px;color:var(--gray);font-weight:600;text-transform:uppercase;letter-spacing:.4px}
.scard .val{font-size:16px;font-weight:700;margin-top:3px}
.val.pos{color:var(--green)}.val.neg{color:var(--red)}
/* ── Category Breakdown Panel ── */
.cat-panel{margin:0 20px 12px;background:var(--card);border-radius:16px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08)}
.cat-panel-hdr{display:flex;justify-content:space-between;align-items:center;padding:11px 14px;background:#f9f9f9;border-bottom:1px solid var(--border);cursor:pointer}
.cat-panel-hdr-left{display:flex;align-items:center;gap:7px;font-size:13px;font-weight:700;color:#1c1c1e}
.cat-chevron{font-size:11px;color:var(--gray);transition:transform .2s}
.cat-chevron.open{transform:rotate(180deg)}
.cat-body{overflow:hidden;transition:max-height .3s ease}
/* Column headers */
.cat-col-hdr{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 1fr;gap:0;padding:7px 14px;background:#f2f2f7;border-bottom:1px solid var(--border)}
.cat-col-hdr span{font-size:9px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px;text-align:right}
.cat-col-hdr span:first-child{text-align:left}
/* Category row */
.cat-row{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 1fr;gap:0;padding:11px 14px;border-bottom:1px solid var(--border);align-items:center}
.cat-row:last-child{border-bottom:none}
.cat-row-label{display:flex;align-items:center;gap:7px}
.cat-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.cat-row-name{font-size:12px;font-weight:700;color:#1c1c1e}
.cat-row-sub{font-size:10px;color:var(--gray);margin-top:1px}
.cat-cell{text-align:right}
.cat-cell .cv{font-size:12px;font-weight:700;color:#1c1c1e}
.cat-cell .cs{font-size:10px;color:var(--gray);margin-top:1px}
.cat-cell .pos{color:var(--green)}.cat-cell .neg{color:var(--red)}
.cat-cell .divv{color:var(--purple)}
/* Totals row */
.cat-total-row{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 1fr;gap:0;padding:10px 14px;background:#f0f7ff;border-top:2px solid #bae6fd}
.cat-total-row .cv{font-size:12px;font-weight:700}
/* Mini bar inside category row */
.cat-bar-wrap{height:3px;background:#e5e5ea;border-radius:2px;margin-top:4px}
.cat-bar{height:3px;border-radius:2px}
/* Filter chips */
.filter-bar{display:flex;gap:7px;padding:0 20px 11px;overflow-x:auto;scrollbar-width:none}
.filter-bar::-webkit-scrollbar{display:none}
.chip{padding:5px 13px;border-radius:20px;border:1.5px solid var(--border);background:#fff;font-size:12px;font-weight:600;cursor:pointer;white-space:nowrap;color:#3a3a3c}
.chip.on{background:var(--blue);color:#fff;border-color:var(--blue)}
.section{padding:0 20px 100px}
.sec-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
.sec-ttl{font-size:19px;font-weight:700}
.btn{background:var(--blue);color:#fff;border:none;border-radius:10px;padding:8px 16px;font-size:14px;font-weight:600;cursor:pointer;transition:opacity .2s}
.btn:active{opacity:.75}
.btn.sm{padding:5px 12px;font-size:12px;border-radius:8px}
.btn.xs{padding:3px 8px;font-size:11px;border-radius:6px}
.btn.out{background:transparent;color:var(--blue);border:1.5px solid var(--blue)}
.btn.red{background:var(--red)}.btn.grn{background:var(--green)}.btn.amber{background:var(--orange)}
.btn.raf{background:linear-gradient(135deg,#007aff,#5856d6);display:flex;align-items:center;gap:5px;font-size:13px;padding:8px 14px}
.btn:disabled{opacity:.45;cursor:not-allowed}
.card{background:var(--card);border-radius:16px;padding:15px;margin-bottom:12px;box-shadow:0 1px 3px rgba(0,0,0,.08)}
.inv-top{display:flex;justify-content:space-between;align-items:flex-start;gap:8px}
.inv-meta{flex:1;min-width:0}
.inv-name{font-size:15px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.inv-tkr{font-size:11px;color:var(--gray);margin-top:1px}
.badges{display:flex;gap:4px;flex-wrap:wrap;margin-top:5px}
.badge{font-size:10px;font-weight:600;padding:2px 7px;border-radius:20px;background:#e5e5ea;color:#3a3a3c}
.badge.usd{background:#dbeafe;color:#1d4ed8}.badge.sgd{background:#dcfce7;color:#15803d}
.badge.gbp{background:#f3e8ff;color:#7e22ce}.badge.eur{background:#fff7ed;color:#c2410c}
.badge.hkd{background:#fef9c3;color:#a16207}
.badge.cash{background:#dcfce7;color:#166534}.badge.srs{background:#fef9c3;color:#854d0e}.badge.cpf{background:#fce7f3;color:#9d174d}
.price-box{text-align:right;min-width:120px;flex-shrink:0}
.price-lbl{font-size:10px;color:var(--gray);margin-bottom:3px;display:flex;align-items:center;justify-content:flex-end;gap:4px}
.price-big{font-size:19px;font-weight:700}
.price-chg{font-size:11px;font-weight:600;margin-top:1px}
.price-ts{font-size:9px;color:var(--gray);margin-top:1px}
.price-resolved{font-size:9px;color:var(--blue);margin-top:1px;font-weight:600}
.abadge{font-size:9px;background:#dcfce7;color:#15803d;padding:1px 5px;border-radius:6px;font-weight:700}
.ibadge{font-size:9px;background:#dbeafe;color:#1d4ed8;padding:1px 5px;border-radius:6px;font-weight:700}
.mbadge{font-size:9px;background:#f2f2f7;color:var(--gray);padding:1px 5px;border-radius:6px;font-weight:700}
.fbadge{font-size:9px;background:#fce7f3;color:#9d174d;padding:1px 5px;border-radius:6px;font-weight:700}
.spin{display:inline-block;width:11px;height:11px;border:2px solid #e5e5ea;border-top-color:var(--blue);border-radius:50%;animation:spin .7s linear infinite;vertical-align:middle}
@keyframes spin{to{transform:rotate(360deg)}}
.manual-row{display:flex;align-items:center;gap:4px;justify-content:flex-end;margin-top:5px}
.spot-inp{width:88px;border:1.5px solid var(--border);border-radius:8px;padding:5px 8px;font-size:13px;text-align:right;background:#f9f9f9}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:7px;margin-top:11px}
.stat{background:#f2f2f7;border-radius:10px;padding:9px 10px}
.stat .sl{font-size:9px;color:var(--gray);font-weight:600;text-transform:uppercase;letter-spacing:.3px}
.stat .sv{font-size:13px;font-weight:700;margin-top:2px}
.div-strip{background:linear-gradient(135deg,#f0fdf4,#f0f9ff);border:1px solid #d1fae5;border-radius:12px;padding:11px 12px;margin-top:10px}
.div-strip-title{font-size:10px;font-weight:700;color:#15803d;text-transform:uppercase;letter-spacing:.4px;margin-bottom:8px}
.div-strip-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:0}
.div-col{text-align:center;padding:4px 2px}
.div-col:not(:last-child){border-right:1px solid #d1fae5}
.div-col .dc-lbl{font-size:9px;color:#6b7280;font-weight:600;text-transform:uppercase;letter-spacing:.3px;line-height:1.3}
.div-col .dc-val{font-size:13px;font-weight:700;color:#15803d;margin-top:3px}
.div-col .dc-sub{font-size:9px;color:#6b7280;margin-top:1px}
.yield-row{display:flex;justify-content:space-between;align-items:center;margin-top:8px;padding-top:8px;border-top:1px solid #d1fae5}
.yield-item{text-align:center;flex:1}
.yield-item .yl{font-size:9px;color:#6b7280;font-weight:600;text-transform:uppercase;letter-spacing:.3px}
.yield-item .yv{font-size:12px;font-weight:700;margin-top:2px}
.actions{display:flex;gap:7px;margin-top:11px;flex-wrap:wrap}
.tx-section{margin-top:12px}
.tx-hdr-row{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.tx-hdr{font-size:11px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px}
.tx-item{display:flex;justify-content:space-between;align-items:center;padding:8px 10px;border-bottom:1px solid var(--border);font-size:12px;gap:6px}
.tx-item:last-child{border-bottom:none}
.tx-item:first-child{border-top:1px solid var(--border)}
.tx-left{flex:1;min-width:0}
.tx-t{font-weight:700}
.tx-t.buy{color:var(--blue)}.tx-t.sell{color:var(--red)}.tx-t.rights{color:var(--orange)}.tx-t.free{color:var(--green)}
.tx-right{text-align:right;flex-shrink:0}
.tx-btns{display:flex;gap:4px;flex-shrink:0}
.src-info{background:#f0f9ff;border:1px solid #bae6fd;border-radius:9px;padding:7px 10px;margin-top:8px;font-size:11px;color:#0369a1;display:flex;align-items:flex-start;gap:6px;line-height:1.4}
.overlay{position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:200;display:flex;align-items:flex-end;justify-content:center}
.modal{background:#fff;border-radius:22px 22px 0 0;padding:22px 20px 44px;width:100%;max-width:430px;max-height:92vh;overflow-y:auto}
.modal h2{font-size:19px;font-weight:700;margin-bottom:4px}
.modal .modal-sub{font-size:12px;color:var(--gray);margin-bottom:16px}
.fg{margin-bottom:12px;position:relative}
.fg label{font-size:12px;font-weight:600;color:#444;display:block;margin-bottom:4px}
.fg input,.fg select{width:100%;padding:11px 13px;border:1.5px solid var(--border);border-radius:11px;font-size:15px;background:#fff;-webkit-appearance:none;appearance:none}
.fg input:focus,.fg select:focus{outline:none;border-color:var(--blue)}
.fg .hint{font-size:11px;color:var(--gray);margin-top:3px}
.row2{display:grid;grid-template-columns:1fr 1fr;gap:9px}
.m-actions{display:flex;gap:9px;margin-top:18px}
.m-actions .btn{flex:1;padding:13px}
.ac-drop{position:absolute;left:0;right:0;top:calc(100% - 2px);background:#fff;border:1.5px solid var(--blue);border-top:none;border-radius:0 0 11px 11px;z-index:50;max-height:210px;overflow-y:auto;box-shadow:0 8px 20px rgba(0,0,0,.12)}
.ac-item{padding:10px 13px;font-size:13px;cursor:pointer;border-bottom:1px solid var(--border);display:flex;flex-direction:column;gap:2px}
.ac-item:last-child{border-bottom:none}
.ac-item:active{background:#f0f7ff}
.ac-sym{font-weight:700;color:var(--blue);font-size:12px}
.ac-name{color:#1c1c1e;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.ac-ex{font-size:10px;color:var(--gray)}
.ac-loading{padding:14px;text-align:center;color:var(--gray);font-size:13px}
.div-yr-block{background:var(--card);border-radius:14px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:6px}
.div-yr-hdr{display:grid;grid-template-columns:1.4fr 0.8fr 1fr 0.8fr 60px;gap:4px;padding:8px 12px;background:#f2f2f7;font-size:10px;font-weight:700;color:var(--gray);text-transform:uppercase}
.div-yr-row{display:grid;grid-template-columns:1.4fr 0.8fr 1fr 0.8fr 60px;gap:4px;padding:10px 12px;border-top:1px solid var(--border);font-size:12px;align-items:center}
.ledger-inv{margin-bottom:18px}
.ledger-inv-hdr{font-size:14px;font-weight:700;display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.ledger-summary{display:grid;grid-template-columns:1fr 1fr 1fr;gap:7px;margin-bottom:8px}
.ls-box{background:#f0fdf4;border:1px solid #d1fae5;border-radius:10px;padding:8px 10px;text-align:center}
.ls-box.bf{background:#f0f9ff;border-color:#bae6fd}
.ls-box.tot{background:#fdf4ff;border-color:#e9d5ff}
.ls-lbl{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.3px;color:#6b7280}
.ls-val{font-size:13px;font-weight:700;margin-top:3px}
.ls-box.bf .ls-val{color:#0369a1}
.ls-box .ls-val{color:#15803d}
.ls-box.tot .ls-val{color:#7e22ce}
.sumpage{padding:0 20px 100px}
.sum-section{margin-bottom:20px}
.sum-section-title{font-size:16px;font-weight:700;margin-bottom:10px;display:flex;align-items:center;gap:6px}
.pie-wrap{background:var(--card);border-radius:16px;padding:18px;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:12px}
.pie-title{font-size:14px;font-weight:700;margin-bottom:14px}
.pie-body{display:flex;align-items:center;gap:16px}
.pie-svg{flex-shrink:0}
.pie-legend{flex:1;display:flex;flex-direction:column;gap:7px}
.pie-leg-item{display:flex;align-items:center;gap:7px}
.pie-dot{width:11px;height:11px;border-radius:50%;flex-shrink:0}
.pie-leg-label{font-size:12px;font-weight:500;color:#1c1c1e;flex:1}
.pie-leg-pct{font-size:10px;color:var(--gray);min-width:36px;text-align:right}
.breakdown-card{background:var(--card);border-radius:16px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:12px}
.bk-hdr{display:grid;gap:4px;padding:9px 14px;background:#f2f2f7;font-size:10px;font-weight:700;color:var(--gray);text-transform:uppercase;letter-spacing:.3px}
.bk-row{display:grid;gap:4px;padding:12px 14px;border-top:1px solid var(--border);font-size:13px;align-items:center}
.bk-label{font-weight:700;display:flex;align-items:center;gap:7px}
.bk-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0}
.prof-winner{background:#f0fdf4}.prof-loser{background:#fff5f5}
.holding-row{display:flex;justify-content:space-between;align-items:center;padding:10px 14px;border-top:1px solid var(--border)}
.holding-row:first-child{border-top:none}
.holding-bar-bg{height:4px;background:#e5e5ea;border-radius:2px;margin-top:4px}
.holding-bar-fill{height:4px;border-radius:2px}
.empty{text-align:center;padding:40px 20px;color:var(--gray)}
.empty .ico{font-size:38px;margin-bottom:8px}
.no-results{text-align:center;padding:30px 20px;color:var(--gray);font-size:14px}
.toast{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);background:#1c1c1e;color:#fff;padding:10px 20px;border-radius:20px;font-size:13px;font-weight:500;z-index:400;opacity:0;transition:opacity .3s;pointer-events:none;white-space:nowrap;max-width:90vw;text-align:center}
.toast.show{opacity:1}
</style>
</head>
<body>
<div id="app"></div>
<div class="toast" id="toast"></div>
<script>
'use strict';
const NOW=new Date(),CY=NOW.getFullYear().toString(),CMONTH=NOW.getMonth(),NY=(NOW.getFullYear()+1).toString();
const TYPE_COLORS={ETF:'#007aff',Stock:'#34c759','Unit Trust':'#ff9500',Bond:'#af52de',Other:'#8e8e93'};
const TYPE_BG={ETF:'#dbeafe',Stock:'#dcfce7','Unit Trust':'#fff7ed',Bond:'#fdf4ff',Other:'#f2f2f7'};
const TYPE_FG={ETF:'#1d4ed8',Stock:'#15803d','Unit Trust':'#c2410c',Bond:'#7e22ce',Other:'#6b7280'};
const ACC_COLORS={Cash:'#34c759',SRS:'#ff9f0a',CPF:'#ff2d55'};
const PROF_COLORS={Profit:'#34c759',Loss:'#ff3b30','Break-even':'#8e8e93'};
const PROXY='https://corsproxy.io/?';

// ─── Price Engine ─────────────────────────────────────────────────────────
async function yfFetch(p){const r=await fetch(PROXY+encodeURIComponent('https://query1.finance.yahoo.com'+p),{headers:{'x-requested-with':'XMLHttpRequest'}});if(!r.ok)throw new Error('HTTP '+r.status);return r.json();}
async function yf2Fetch(p){const r=await fetch(PROXY+encodeURIComponent('https://query2.finance.yahoo.com'+p),{headers:{'x-requested-with':'XMLHttpRequest'}});if(!r.ok)throw new Error('HTTP '+r.status);return r.json();}
async function fetchPriceDirect(sym){
  const j=await yfFetch(`/v8/finance/chart/${encodeURIComponent(sym)}?interval=1d&range=1d`);
  const m=j?.chart?.result?.[0]?.meta;if(!m)throw new Error('No meta');
  const p=m.regularMarketPrice||m.previousClose;if(!p)throw new Error('No price');
  const pv=m.chartPreviousClose||m.previousClose||p;
  return{price:p,change:p-pv,changePct:pv>0?((p-pv)/pv)*100:0,currency:m.currency||''};
}
async function yfSearch(q){try{const j=await yf2Fetch(`/v1/finance/search?q=${encodeURIComponent(q)}&quotesCount=8&newsCount=0&enableFuzzyQuery=false&enableCb=false`);return(j?.quotes||[]).filter(x=>x.symbol&&!['FUTURE','OPTION','CURRENCY','CRYPTOCURRENCY'].includes(x.quoteType));}catch(e){return[];}}
async function openFIGILookup(isin){try{const res=await fetch('https://api.openfigi.com/v3/mapping',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify([{"idType":"ID_ISIN","idValue":isin}])});if(!res.ok)return[];const data=await res.json();return data?.[0]?.data||[];}catch(e){return[];}}
function exchSuffix(ec){const M={'SG':'.SI','LN':'.L','HK':'.HK','GY':'.DE','FP':'.PA','SM':'.MC','IM':'.MI','AV':'.VI','NA':'.AS','BB':'.BR','SW':'.SW','AU':'.AX','JP':'.T','KS':'.KS','TW':'.TW','US':'','UN':'','UA':'','UW':'','UR':'','UT':'','UP':''};return Object.prototype.hasOwnProperty.call(M,ec)?M[ec]:undefined;}
async function resolvePriceSource(inv){
  const ticker=(inv.ticker||'').trim().toUpperCase(),isin=(inv.isin||'').trim().toUpperCase();
  if(inv.resolvedTicker&&inv.resolvedTicker!==ticker){try{const r=await fetchPriceDirect(inv.resolvedTicker);return{...r,resolvedTicker:inv.resolvedTicker,resolvedBy:'cached'};}catch(e){inv.resolvedTicker=null;}}
  if(ticker){
    try{const r=await fetchPriceDirect(ticker);return{...r,resolvedTicker:ticker,resolvedBy:'ticker'};}catch(e){}
    if(/^\d{4,8}$/.test(ticker)){try{const r=await fetchPriceDirect(ticker+'.SI');return{...r,resolvedTicker:ticker+'.SI',resolvedBy:'SGX'};}catch(e){}}
    const yfT=await yfSearch(ticker);for(const q of yfT.slice(0,4)){try{const r=await fetchPriceDirect(q.symbol);return{...r,resolvedTicker:q.symbol,resolvedBy:'ticker-search'};}catch(e){}}
  }
  if(isin){
    const yfI=await yfSearch(isin);for(const q of yfI.slice(0,6)){try{const r=await fetchPriceDirect(q.symbol);return{...r,resolvedTicker:q.symbol,resolvedBy:'ISIN→YF'};}catch(e){}}
    const figiList=await openFIGILookup(isin);
    const pt=['Open-End Fund','Exchange-Traded Fund','Common Stock','Closed-End Fund'];
    figiList.sort((a,b)=>(pt.indexOf(a.securityType2||a.securityType||'')===-1?99:pt.indexOf(a.securityType2||a.securityType||''))-(pt.indexOf(b.securityType2||b.securityType||'')===-1?99:pt.indexOf(b.securityType2||b.securityType||'')));
    for(const figi of figiList.slice(0,10)){if(!figi.ticker)continue;const sfx=exchSuffix(figi.exchCode);if(sfx===undefined)continue;try{const r=await fetchPriceDirect(figi.ticker+sfx);return{...r,resolvedTicker:figi.ticker+sfx,resolvedBy:'ISIN→OpenFIGI'};}catch(e){}}
  }
  return null;
}
async function searchTicker(q){const j=await yf2Fetch(`/v1/finance/search?q=${encodeURIComponent(q)}&quotesCount=8&newsCount=0&enableFuzzyQuery=true&enableCb=false`);return(j?.quotes||[]).filter(x=>x.symbol&&x.quoteType&&!['FUTURE','OPTION','CURRENCY'].includes(x.quoteType)).map(x=>({symbol:x.symbol,name:x.shortname||x.longname||x.symbol,exchange:x.exchange||x.fullExchangeName||'',type:x.typeDisp||x.quoteType||''}));}
function resolvedByLabel(by){return{'ticker':'Ticker','SGX':'SGX (.SI)','ticker-search':'Ticker search','ISIN→YF':'ISIN via Yahoo','ISIN→OpenFIGI':'ISIN via OpenFIGI','cached':'Cached'}[by]||by||'';}

// ─── State ────────────────────────────────────────────────────────────────
let S={investments:[],dividends:[],tab:'portfolio',fCcy:'All',fAcc:'All',
       modal:null,editId:null,txId:null,txEditInvId:null,txEditId:null,divEditId:null,
       refreshing:{},refreshingAll:false,acResults:[],acLoading:false,acQuery:'',acTimer:null,
       searchQuery:'',showAllTx:{},catOpen:true};
try{const d=JSON.parse(localStorage.getItem('it2025')||'{}');S.investments=d.i||[];S.dividends=d.d||[];}catch(e){}
function save(){try{localStorage.setItem('it2025',JSON.stringify({i:S.investments,d:S.dividends}));}catch(e){}}
function uid(){return Date.now().toString(36)+Math.random().toString(36).slice(2);}
function fmt(n,dp=2){if(n==null||isNaN(n))return'–';return Number(n).toLocaleString('en-SG',{minimumFractionDigits:dp,maximumFractionDigits:dp});}
function fc(n,c,dp=2){const m={SGD:'S$',USD:'US$',GBP:'£',EUR:'€',HKD:'HK$'};const s=m[c]||(c?c+' ':'');if(n==null||isNaN(n))return s+'–';return(n<0?'-':'')+s+Math.abs(n).toLocaleString('en-SG',{minimumFractionDigits:dp,maximumFractionDigits:dp});}
function toast(msg,dur=3500){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');clearTimeout(t._t);t._t=setTimeout(()=>t.classList.remove('show'),dur);}
function tsAgo(ts){if(!ts)return'';const m=Math.round((Date.now()-ts)/60000);if(m<1)return'just now';if(m<60)return m+'m ago';const h=Math.round(m/60);return h<24?h+'h ago':Math.round(h/24)+'d ago';}

// ─── Price Refresh ────────────────────────────────────────────────────────
async function refreshOne(invId){
  const inv=S.investments.find(i=>i.id===invId);if(!inv)return;
  if(!inv.ticker&&!inv.isin){toast('⚠️ Add a ticker or ISIN first.');return;}
  S.refreshing[invId]=true;render();
  try{const r=await resolvePriceSource(inv);if(r&&r.price){inv.spotPrice=r.price;inv.priceChange=r.change;inv.priceChangePct=r.changePct;inv.priceUpdated=Date.now();inv.priceAuto=true;inv.resolvedTicker=r.resolvedTicker||null;inv.resolvedBy=r.resolvedBy||null;save();toast('✅ '+inv.name+': '+fc(r.price,inv.currency,3)+' via '+resolvedByLabel(r.resolvedBy));}else{inv.priceAuto=false;save();toast('⚠️ '+inv.name+': No price found. Enter manually.');}}catch(e){toast('⚠️ '+e.message);}
  S.refreshing[invId]=false;render();
}
async function refreshAll(){
  const list=S.investments.filter(i=>i.ticker||i.isin);if(!list.length){toast('No tickers or ISINs set.');return;}
  S.refreshingAll=true;render();let ok=0,fail=0;
  await Promise.all(list.map(async inv=>{S.refreshing[inv.id]=true;try{const r=await resolvePriceSource(inv);if(r&&r.price){inv.spotPrice=r.price;inv.priceChange=r.change;inv.priceChangePct=r.changePct;inv.priceUpdated=Date.now();inv.priceAuto=true;inv.resolvedTicker=r.resolvedTicker||null;inv.resolvedBy=r.resolvedBy||null;ok++;}else{fail++;}}catch(e){fail++;}S.refreshing[inv.id]=false;}));
  S.refreshingAll=false;save();toast('✅ '+ok+' updated'+(fail?' · ⚠️ '+fail+' not found':''));render();
}
setInterval(()=>{if(!document.hidden&&S.investments.some(i=>i.ticker||i.isin))refreshAll();},5*60*1000);
document.addEventListener('visibilitychange',()=>{if(!document.hidden&&S.investments.some(i=>i.ticker||i.isin))refreshAll();});

// ─── Autocomplete ─────────────────────────────────────────────────────────
function onNameInput(val){clearTimeout(S.acTimer);S.acQuery=val;S.acResults=[];if(val.length<2){S.acLoading=false;renderAC();return;}S.acLoading=true;renderAC();S.acTimer=setTimeout(async()=>{try{const res=await searchTicker(val);if(S.acQuery===val){S.acResults=res;S.acLoading=false;renderAC();}}catch(e){S.acLoading=false;renderAC();}},400);}
function renderAC(){const drop=document.getElementById('ac-drop');if(!drop)return;if(S.acLoading){drop.innerHTML='<div class="ac-loading"><span class="spin"></span> Searching…</div>';drop.style.display='block';return;}if(!S.acResults.length){drop.style.display='none';return;}drop.innerHTML=S.acResults.map((r,i)=>`<div class="ac-item" onclick="pickAC(${i})"><div style="display:flex;justify-content:space-between"><span class="ac-name">${r.name}</span><span class="ac-sym">${r.symbol}</span></div><span class="ac-ex">${r.exchange} · ${r.type}</span></div>`).join('');drop.style.display='block';}
function pickAC(i){const r=S.acResults[i];const ne=document.getElementById('fn'),te=document.getElementById('ftk');if(ne&&!ne.value)ne.value=r.name;if(te)te.value=r.symbol;const excCcy={NMS:'USD',NYQ:'USD',NGM:'USD',LSE:'GBP',SES:'SGD',HKG:'HKD',FRA:'EUR',EPA:'EUR'};const ccy=excCcy[r.exchange]||'';if(ccy){const el=document.getElementById('fc2');if(el)el.value=ccy;}const excMkt={NMS:'NASDAQ',NYQ:'NYSE',NGM:'NASDAQ',LSE:'LSE',SES:'SGX',HKG:'HKEX'};const mkt=excMkt[r.exchange]||'Other';const mel=document.getElementById('fm');if(mel)mel.value=mkt;S.acResults=[];S.acLoading=false;const drop=document.getElementById('ac-drop');if(drop)drop.style.display='none';}

// ─── Calc ─────────────────────────────────────────────────────────────────
function calc(inv){
  let sh=0,cost=0;
  for(const t of(inv.transactions||[])){const u=+t.shares||0,p=+t.price||0,f=+t.fees||0;if(t.type==='Buy'){cost+=u*p+f;sh+=u;}else if(t.type==='Sell'){const ac=sh>0?cost/sh:0;cost-=u*ac;sh-=u;}else if(t.type==='Rights Issue'){cost+=u*p+f;sh+=u;}else if(t.type==='Free Issue'){sh+=u;}}
  const avg=sh>0?cost/sh:0,spot=+inv.spotPrice||0,mv=spot*sh,pl=mv-cost,plp=cost>0?(pl/cost)*100:0;
  const myDivs=S.dividends.filter(d=>d.invId===inv.id);
  const dbf=myDivs.filter(d=>d.date&&d.date.slice(0,4)<CY).reduce((s,d)=>s+(+d.amount||0),0);
  const dcur=myDivs.filter(d=>d.date&&d.date.startsWith(CY)).reduce((s,d)=>s+(+d.amount||0),0);
  const dtot=dbf+dcur,dy=cost>0?(dcur/cost)*100:0,dytot=cost>0?(dtot/cost)*100:0;
  return{sh,cost,avg,spot,mv,pl,plp,dbf,dcur,dtot,dy,dytot};
}

// ─── Category Breakdown ───────────────────────────────────────────────────
function buildCatData(){
  const typeKeys=['ETF','Stock','Unit Trust','Bond','Other'];
  const byType={};
  typeKeys.forEach(k=>{byType[k]={cost:0,mv:0,pl:0,dbf:0,dcur:0,dtot:0,count:0};});
  S.investments.forEach(inv=>{
    const s=calc(inv);
    const k=typeKeys.includes(inv.type)?inv.type:'Other';
    byType[k].cost+=s.cost; byType[k].mv+=s.mv; byType[k].pl+=s.pl;
    byType[k].dbf+=s.dbf; byType[k].dcur+=s.dcur; byType[k].dtot+=s.dtot;
    byType[k].count++;
  });
  return{byType,typeKeys:typeKeys.filter(k=>byType[k].count>0)};
}

function renderCatBreakdown(tC,tM,tP,tDbf,tDcur,tDtot){
  const {byType,typeKeys}=buildCatData();
  if(!typeKeys.length)return'';
  const open=S.catOpen;
  return`
<div class="cat-panel">
  <div class="cat-panel-hdr" onclick="toggleCat()">
    <div class="cat-panel-hdr-left">
      📂 <span>Breakdown by Category</span>
      <span style="font-size:10px;font-weight:500;color:var(--gray);margin-left:4px">${typeKeys.length} type${typeKeys.length>1?'s':''}</span>
    </div>
    <span class="cat-chevron ${open?'open':''}">▼</span>
  </div>
  ${open?`
  <div class="cat-body">
    <div class="cat-col-hdr">
      <span>Category</span>
      <span>Cost</span>
      <span>Mkt Val</span>
      <span>P&L</span>
      <span>Div Total</span>
    </div>
    ${typeKeys.map(k=>{
      const d=byType[k];
      const mvPct=tM>0?(d.mv/tM*100):0;
      const plColor=d.pl>=0?'pos':'neg';
      return`<div class="cat-row">
        <div class="cat-row-label">
          <div class="cat-dot" style="background:${TYPE_COLORS[k]}"></div>
          <div>
            <div class="cat-row-name" style="color:${TYPE_FG[k]}">${k}</div>
            <div class="cat-row-sub">${d.count} holding${d.count>1?'s':''}</div>
            <div class="cat-bar-wrap"><div class="cat-bar" style="width:${mvPct}%;background:${TYPE_COLORS[k]}"></div></div>
          </div>
        </div>
        <div class="cat-cell">
          <div class="cv">${fc(d.cost,'SGD',0)}</div>
          <div class="cs">${tC>0?fmt(d.cost/tC*100,1):'0'}%</div>
        </div>
        <div class="cat-cell">
          <div class="cv">${fc(d.mv,'SGD',0)}</div>
          <div class="cs">${fmt(mvPct,1)}%</div>
        </div>
        <div class="cat-cell">
          <div class="cv ${plColor}">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div>
          <div class="cs ${plColor}">${d.pl>=0?'+':''}${d.cost>0?fmt(d.pl/d.cost*100,2):'0'}%</div>
        </div>
        <div class="cat-cell">
          <div class="cv divv">${fc(d.dtot,'SGD',0)}</div>
          <div class="cs" style="color:#0369a1">B/F ${fc(d.dbf,'SGD',0)}</div>
          <div class="cs pos">${CY} ${fc(d.dcur,'SGD',0)}</div>
        </div>
      </div>`;
    }).join('')}
    <!-- Totals row -->
    <div class="cat-total-row">
      <div><div class="cv" style="font-size:11px;color:var(--gray)">TOTAL</div></div>
      <div class="cat-cell"><div class="cv">${fc(tC,'SGD',0)}</div></div>
      <div class="cat-cell"><div class="cv">${fc(tM,'SGD',0)}</div></div>
      <div class="cat-cell"><div class="cv ${tP>=0?'pos':'neg'}">${tP>=0?'+':''}${fc(tP,'SGD',0)}</div></div>
      <div class="cat-cell"><div class="cv divv">${fc(tDtot,'SGD',0)}</div></div>
    </div>
  </div>`:''}
</div>`;}

// ─── Pie Chart ────────────────────────────────────────────────────────────
function makePie(slices,size=140){
  const total=slices.reduce((s,x)=>s+x.value,0);
  if(!total)return'<div style="text-align:center;color:var(--gray);font-size:13px;padding:20px">No data yet</div>';
  const cx=size/2,cy=size/2,r=size/2-6,ri=r*0.52;
  let paths='',ang=-Math.PI/2;
  for(const sl of slices){const a=(sl.value/total)*2*Math.PI,ea=ang+a;const x1=cx+r*Math.cos(ang),y1=cy+r*Math.sin(ang),x2=cx+r*Math.cos(ea),y2=cy+r*Math.sin(ea);const xi1=cx+ri*Math.cos(ang),yi1=cy+ri*Math.sin(ang),xi2=cx+ri*Math.cos(ea),yi2=cy+ri*Math.sin(ea);const lg=a>Math.PI?1:0;paths+=`<path d="M${xi1},${yi1}L${x1},${y1}A${r},${r} 0 ${lg},1 ${x2},${y2}L${xi2},${yi2}A${ri},${ri} 0 ${lg},0 ${xi1},${yi1}" fill="${sl.color}" stroke="#fff" stroke-width="2"/>`;ang=ea;}
  return`<svg width="${size}" height="${size}" viewBox="0 0 ${size} ${size}" class="pie-svg">${paths}</svg>`;
}

// ─── Year-end Banner ──────────────────────────────────────────────────────
function yearEndBanner(){
  if(CMONTH<10)return'';
  const allStats=S.investments.map(i=>calc(i));
  const totalDiv=allStats.reduce((s,x)=>s+x.dtot,0),curDiv=allStats.reduce((s,x)=>s+x.dcur,0);
  const daysLeft=Math.ceil((new Date(NOW.getFullYear(),11,31)-NOW)/(1000*60*60*24));
  return`<div class="yr-banner"><div class="yb-ico">🔔</div><div class="yb-txt"><b>Year-end: ${daysLeft} day${daysLeft!==1?'s':''} left in ${CY}</b><br>On <b>1 Jan ${NY}</b>, ${CY} dividends (<b>${fc(curDiv,'SGD')}</b>) auto-become Accumulated B/F for ${NY}. Total carry-forward: <b>${fc(totalDiv,'SGD')}</b>.</div></div>`;
}

// ─── Render ───────────────────────────────────────────────────────────────
function render(){
  const q=S.searchQuery.trim().toLowerCase();
  const invs=S.investments.filter(i=>{
    if(S.fCcy!=='All'&&i.currency!==S.fCcy)return false;
    if(S.fAcc!=='All'&&i.account!==S.fAcc)return false;
    if(q&&!i.name.toLowerCase().includes(q)&&!(i.ticker||'').toLowerCase().includes(q)&&!(i.isin||'').toLowerCase().includes(q))return false;
    return true;
  });
  const all=S.investments.map(i=>calc(i));
  const tC=all.reduce((s,x)=>s+x.cost,0),tM=all.reduce((s,x)=>s+x.mv,0),tP=tM-tC,
        tDbf=all.reduce((s,x)=>s+x.dbf,0),tDcur=all.reduce((s,x)=>s+x.dcur,0),tDtot=all.reduce((s,x)=>s+x.dtot,0);
  document.getElementById('app').innerHTML=`
<div class="header">
  <div style="display:flex;justify-content:space-between;align-items:center">
    <div><h1>📊 InvestTracker</h1><div class="sub">${NOW.toLocaleDateString('en-SG',{weekday:'short',day:'numeric',month:'short',year:'numeric'})}</div></div>
    <button class="btn raf" onclick="refreshAll()" ${S.refreshingAll?'disabled':''}>
      ${S.refreshingAll?'<span class="spin"></span>':'🔄'} ${S.refreshingAll?'Updating…':'Refresh All'}
    </button>
  </div>
  <div class="tabs">
    <button class="tab ${S.tab==='portfolio'?'active':''}" onclick="setTab('portfolio')">Portfolio</button>
    <button class="tab ${S.tab==='dividends'?'active':''}" onclick="setTab('dividends')">Dividends</button>
    <button class="tab ${S.tab==='summary'?'active':''}" onclick="setTab('summary')">Summary</button>
  </div>
</div>
${S.tab==='portfolio'?renderPort(invs,tC,tM,tP,tDbf,tDcur,tDtot):S.tab==='dividends'?renderDivs():renderSummary()}
${renderModal()}`;
  document.querySelectorAll('.spi').forEach(el=>el.addEventListener('change',function(){
    const inv=S.investments.find(i=>i.id===this.dataset.id);
    if(inv){inv.spotPrice=parseFloat(this.value)||0;inv.priceAuto=false;inv.priceUpdated=Date.now();save();render();}
  }));
  const si=document.getElementById('search-input');
  if(si)si.addEventListener('input',function(){S.searchQuery=this.value;renderPortOnly();});
}

function renderPortOnly(){
  const q=S.searchQuery.trim().toLowerCase();
  const invs=S.investments.filter(i=>{
    if(S.fCcy!=='All'&&i.currency!==S.fCcy)return false;
    if(S.fAcc!=='All'&&i.account!==S.fAcc)return false;
    if(q&&!i.name.toLowerCase().includes(q)&&!(i.ticker||'').toLowerCase().includes(q)&&!(i.isin||'').toLowerCase().includes(q))return false;
    return true;
  });
  const el=document.getElementById('holdings-list');if(!el)return;
  const countEl=document.getElementById('holdings-count');if(countEl)countEl.textContent=`Holdings (${invs.length})`;
  el.innerHTML=invs.length===0
    ?`<div class="${q?'no-results':'empty'}">${q?`No investments match "<b>${q}</b>"`:'<div class="ico">📈</div><div>No investments yet.</div>'}</div>`
    :invs.map(inv=>renderCard(inv)).join('');
  document.querySelectorAll('.spi').forEach(el=>el.addEventListener('change',function(){
    const inv=S.investments.find(i=>i.id===this.dataset.id);
    if(inv){inv.spotPrice=parseFloat(this.value)||0;inv.priceAuto=false;inv.priceUpdated=Date.now();save();renderPortOnly();}
  }));
}

// ─── Portfolio Page ───────────────────────────────────────────────────────
function renderPort(invs,tC,tM,tP,tDbf,tDcur,tDtot){
  return`
<!-- ① Overall summary strip -->
<div class="summary">
  <div class="scard"><div class="lbl">Total Cost</div><div class="val">${fc(tC,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Mkt Value</div><div class="val">${fc(tM,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">P&L</div><div class="val ${tP>=0?'pos':'neg'}">${tP>=0?'+':''}${fc(tP,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div B/F</div><div class="val" style="color:#0369a1">${fc(tDbf,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div ${CY}</div><div class="val pos">${fc(tDcur,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Div Total</div><div class="val" style="color:var(--purple)">${fc(tDtot,'SGD',0)}</div></div>
</div>

<!-- ② Category breakdown panel -->
${renderCatBreakdown(tC,tM,tP,tDbf,tDcur,tDtot)}

<!-- ③ Year-end banner (Nov/Dec only) -->
${yearEndBanner()}

<!-- ④ Search -->
<div class="search-bar">
  <input id="search-input" placeholder="🔍  Search investments…" value="${S.searchQuery}" autocomplete="off">
  ${S.searchQuery?`<button class="sclear" onclick="clearSearch()">✕</button>`:''}
</div>

<!-- ⑤ Currency / Account filters -->
<div class="filter-bar">
  ${['All','SGD','USD','GBP','EUR','HKD'].map(c=>`<button class="chip ${S.fCcy===c?'on':''}" onclick="setF('c','${c}')">${c}</button>`).join('')}
  <span style="width:1px;background:#e5e5ea;flex-shrink:0"></span>
  ${['All','Cash','SRS','CPF'].map(a=>`<button class="chip ${S.fAcc===a?'on':''}" onclick="setF('a','${a}')">${a}</button>`).join('')}
</div>

<!-- ⑥ Individual holdings -->
<div class="section">
  <div class="sec-hdr">
    <span class="sec-ttl" id="holdings-count">Holdings (${invs.length})</span>
    <button class="btn sm" onclick="openM('addInv')">+ Add</button>
  </div>
  <div id="holdings-list">
    ${invs.length===0
      ?`<div class="${S.searchQuery?'no-results':'empty'}">${S.searchQuery?`No investments match "<b>${S.searchQuery}</b>"`:'<div class="ico">📈</div><div>No investments yet.<br>Tap <b>+ Add</b> to begin.</div>'}</div>`
      :invs.map(inv=>renderCard(inv)).join('')}
  </div>
</div>`;}

function renderCard(inv){
  const s=calc(inv);const loading=S.refreshing[inv.id];
  const chg=inv.priceChange||0,chgP=inv.priceChangePct||0,cc=chg>=0?'var(--green)':'var(--red)';
  let srcBadge='';
  if(inv.priceAuto&&inv.spotPrice){if(inv.resolvedBy==='ISIN→OpenFIGI')srcBadge='<span class="fbadge">FIGI</span>';else if(inv.resolvedBy&&inv.resolvedBy.includes('ISIN'))srcBadge='<span class="ibadge">ISIN</span>';else srcBadge='<span class="abadge">LIVE</span>';}else{srcBadge='<span class="mbadge">MANUAL</span>';}
  const txs=inv.transactions||[];const showAll=S.showAllTx[inv.id];const displayTxs=showAll?[...txs].reverse():[...txs].reverse().slice(0,3);
  return`<div class="card" id="card-${inv.id}">
  <div class="inv-top">
    <div class="inv-meta">
      <div class="inv-name">${inv.name||'Unnamed'}</div>
      <div class="inv-tkr">${inv.ticker?`<b>${inv.ticker}</b>`:'No ticker'}${inv.isin?` · <span style="color:var(--blue)">${inv.isin}</span>`:''} · ${inv.market||'–'}</div>
      <div class="badges">
        <span class="badge">${inv.type||'Stock'}</span>
        <span class="badge ${(inv.currency||'').toLowerCase()}">${inv.currency||'–'}</span>
        <span class="badge ${(inv.account||'').toLowerCase()}">${inv.account||'–'}</span>
      </div>
    </div>
    <div class="price-box">
      <div class="price-lbl">${srcBadge}<button class="btn sm out" style="padding:2px 8px;font-size:10px" onclick="refreshOne('${inv.id}')" ${loading?'disabled':''}>${loading?'<span class="spin"></span>':'🔄'}</button></div>
      ${inv.spotPrice?`<div class="price-big">${fc(inv.spotPrice,inv.currency,3)}</div>${inv.priceAuto?`<div class="price-chg" style="color:${cc}">${chg>=0?'+':''}${fc(chg,inv.currency,3)} (${chg>=0?'+':''}${fmt(chgP,2)}%)</div>`:''}<div class="price-ts">${tsAgo(inv.priceUpdated)}</div>${inv.resolvedTicker&&inv.resolvedTicker!==(inv.ticker||'').toUpperCase()?`<div class="price-resolved">via ${inv.resolvedTicker}</div>`:''}`:`<div style="font-size:11px;color:var(--gray);margin-top:6px">Tap 🔄 to fetch</div>`}
      <div class="manual-row"><input class="spot-inp spi" data-id="${inv.id}" type="number" step="0.0001" value="${inv.spotPrice||''}" placeholder="Manual"></div>
    </div>
  </div>
  ${inv.resolvedBy&&inv.resolvedBy.includes('ISIN')?`<div class="src-info">ℹ️ ISIN <b>${inv.isin}</b> → <b>${inv.resolvedTicker||''}</b> (${resolvedByLabel(inv.resolvedBy)})</div>`:''}
  <div class="grid2">
    <div class="stat"><div class="sl">Units Held</div><div class="sv">${fmt(s.sh,4)}</div></div>
    <div class="stat"><div class="sl">Avg Cost</div><div class="sv">${fc(s.avg,inv.currency,4)}</div></div>
    <div class="stat"><div class="sl">Total Cost</div><div class="sv">${fc(s.cost,inv.currency)}</div></div>
    <div class="stat"><div class="sl">Mkt Value</div><div class="sv">${fc(s.mv,inv.currency)}</div></div>
    <div class="stat"><div class="sl">Unreal. P&L</div><div class="sv ${s.pl>=0?'pos':'neg'}">${s.pl>=0?'+':''}${fc(s.pl,inv.currency)}<br><small>${s.pl>=0?'+':''}${fmt(s.plp,2)}%</small></div></div>
    <div class="stat"><div class="sl">Breakeven</div><div class="sv" style="font-size:11px">${fc(s.avg,inv.currency,4)}<br><small style="color:var(--gray)">avg cost/unit</small></div></div>
  </div>
  <div class="div-strip">
    <div class="div-strip-title">💰 Dividends Received</div>
    <div class="div-strip-grid">
      <div class="div-col"><div class="dc-lbl">Accum. B/F</div><div class="dc-val" style="color:#0369a1">${fc(s.dbf,inv.currency)}</div><div class="dc-sub">Before ${CY}</div></div>
      <div class="div-col"><div class="dc-lbl">Year ${CY}</div><div class="dc-val">${fc(s.dcur,inv.currency)}</div><div class="dc-sub">This year</div></div>
      <div class="div-col"><div class="dc-lbl">Accum. To-Date</div><div class="dc-val" style="color:var(--purple)">${fc(s.dtot,inv.currency)}</div><div class="dc-sub">All time</div></div>
    </div>
    <div class="yield-row">
      <div class="yield-item"><div class="yl">Yield ${CY}</div><div class="yv" style="color:var(--green)">${fmt(s.dy,2)}%</div></div>
      <div class="yield-item" style="border-left:1px solid #d1fae5;border-right:1px solid #d1fae5"><div class="yl">Yield Total</div><div class="yv" style="color:var(--purple)">${fmt(s.dytot,2)}%</div></div>
      <div class="yield-item"><div class="yl">Entries</div><div class="yv" style="color:var(--gray)">${S.dividends.filter(d=>d.invId===inv.id).length}</div></div>
    </div>
  </div>
  <div class="actions">
    <button class="btn sm out" onclick="openTx('${inv.id}')">+ Transaction</button>
    <button class="btn sm out" onclick="openDiv('${inv.id}')">+ Dividend</button>
    <button class="btn sm out" onclick="openEdit('${inv.id}')">Edit</button>
    <button class="btn sm red" onclick="delInv('${inv.id}')">Delete</button>
  </div>
  ${txs.length>0?`
  <div class="tx-section">
    <div class="tx-hdr-row">
      <div class="tx-hdr">Transactions (${txs.length})</div>
      ${txs.length>3?`<button class="btn xs out" onclick="toggleTxAll('${inv.id}')">${showAll?'Show less':'Show all'}</button>`:''}
    </div>
    ${displayTxs.map(t=>`
    <div class="tx-item">
      <div class="tx-left"><span class="tx-t ${t.type.toLowerCase().replace(' issue','').replace(' ','')}">${t.type}</span><span style="color:var(--gray);margin-left:6px">${t.date||''}</span>${t.notes?`<div style="font-size:10px;color:var(--gray);margin-top:1px">${t.notes}</div>`:''}</div>
      <div class="tx-right"><div>${fmt(t.shares,4)} units</div>${t.price?`<div style="color:var(--gray);font-size:11px">@ ${fc(t.price,inv.currency,4)}</div>`:''}${t.fees?`<div style="color:var(--gray);font-size:10px">fee ${fc(t.fees,inv.currency,2)}</div>`:''}</div>
      <div class="tx-btns"><button class="btn xs out" onclick="openEditTx('${inv.id}','${t.id}')">✏️</button><button class="btn xs red" onclick="delTx('${inv.id}','${t.id}')">🗑</button></div>
    </div>`).join('')}
  </div>`:''}
</div>`;}

// ─── Dividend Ledger ──────────────────────────────────────────────────────
function renderDivs(){
  const divs=S.dividends.slice().sort((a,b)=>(b.date||'').localeCompare(a.date||''));
  const allStats=S.investments.map(i=>calc(i));
  const totDbf=allStats.reduce((s,x)=>s+x.dbf,0),totDcur=allStats.reduce((s,x)=>s+x.dcur,0),totDtot=allStats.reduce((s,x)=>s+x.dtot,0);
  const byInv={};divs.forEach(d=>{if(!byInv[d.invId])byInv[d.invId]=[];byInv[d.invId].push(d);});
  return`
<div class="summary">
  <div class="scard"><div class="lbl">Accum. B/F</div><div class="val" style="color:#0369a1">${fc(totDbf,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Year ${CY}</div><div class="val pos">${fc(totDcur,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Accum. Total</div><div class="val" style="color:var(--purple)">${fc(totDtot,'SGD')}</div></div>
  <div class="scard"><div class="lbl">Records</div><div class="val">${divs.length}</div></div>
</div>
${yearEndBanner()}
<div class="section" style="padding-top:12px">
  <div class="sec-hdr"><span class="sec-ttl">Dividend Ledger</span><button class="btn sm grn" onclick="openDivG()">+ Add</button></div>
  ${divs.length===0?`<div class="empty"><div class="ico">💰</div><div>No dividends recorded yet.</div></div>`:''}
  ${Object.entries(byInv).map(([id,ds])=>{
    const inv=S.investments.find(i=>i.id===id);
    const nm=inv?.name||'Unknown',ccy=inv?.currency||'SGD';
    const bf=ds.filter(d=>d.date&&d.date.slice(0,4)<CY).reduce((s,d)=>s+(+d.amount||0),0);
    const cur=ds.filter(d=>d.date&&d.date.startsWith(CY)).reduce((s,d)=>s+(+d.amount||0),0);
    const tot=bf+cur;
    const byYr={};ds.forEach(d=>{const y=(d.date||'????').slice(0,4);if(!byYr[y])byYr[y]=[];byYr[y].push(d);});
    return`<div class="ledger-inv">
      <div class="ledger-inv-hdr"><span>${nm} <span style="font-size:11px;color:var(--gray);font-weight:500">${inv?.ticker||''}</span></span></div>
      <div class="ledger-summary">
        <div class="ls-box bf"><div class="ls-lbl">Accum. B/F</div><div class="ls-val">${fc(bf,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">Before ${CY}</div></div>
        <div class="ls-box"><div class="ls-lbl">Year ${CY}</div><div class="ls-val">${fc(cur,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">Current year</div></div>
        <div class="ls-box tot"><div class="ls-lbl">Accum. Total</div><div class="ls-val">${fc(tot,ccy)}</div><div style="font-size:9px;color:var(--gray);margin-top:2px">All time</div></div>
      </div>
      ${Object.entries(byYr).sort((a,b)=>b[0].localeCompare(a[0])).map(([yr,yds])=>{
        const ytot=yds.reduce((s,d)=>s+(+d.amount||0),0);
        return`<div class="div-yr-block">
          <div class="div-yr-hdr"><span>📅 ${yr} (${yds.length})</span><span></span><span></span><span style="color:var(--green);font-weight:700">${fc(ytot,ccy)}</span><span></span></div>
          ${yds.map(d=>`<div class="div-yr-row">
            <span>${d.date||'–'}</span><span style="font-size:10px;color:var(--gray)">${d.divType||'Cash'}</span>
            <span style="font-weight:700;color:var(--green)">${fc(d.amount,ccy,4)}</span>
            <span style="color:var(--gray);font-size:10px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${d.notes||''}</span>
            <div style="display:flex;gap:4px;justify-content:flex-end">
              <button class="btn xs out" onclick="openEditDiv('${d.id}')">✏️</button>
              <button class="btn xs red" onclick="delDiv('${d.id}')">🗑</button>
            </div>
          </div>`).join('')}
        </div>`;}).join('')}
    </div>`;}).join('')}
</div>`;}

// ─── Summary Page ─────────────────────────────────────────────────────────
function renderSummary(){
  if(!S.investments.length)return`<div class="sumpage"><div class="empty" style="padding-top:60px"><div class="ico">📊</div><div>No investments yet.</div></div></div>`;
  const allStats=S.investments.map(inv=>({inv,s:calc(inv)}));
  const totalMv=allStats.reduce((s,x)=>s+x.s.mv,0),totalCost=allStats.reduce((s,x)=>s+x.s.cost,0),totalPl=totalMv-totalCost,totalDiv=allStats.reduce((s,x)=>s+x.s.dtot,0);
  const typeKeys=['ETF','Stock','Unit Trust','Bond','Other'];
  const byType={};typeKeys.forEach(k=>{byType[k]={mv:0,cost:0,count:0,pl:0,div:0};});
  allStats.forEach(({inv,s})=>{const k=typeKeys.includes(inv.type)?inv.type:'Other';byType[k].mv+=s.mv;byType[k].cost+=s.cost;byType[k].count++;byType[k].pl+=s.pl;byType[k].div+=s.dtot;});
  const typeSlices=typeKeys.filter(k=>byType[k].mv>0).map(k=>({label:k,value:byType[k].mv,color:TYPE_COLORS[k]}));
  const accKeys=['Cash','SRS','CPF'];
  const byAcc={};accKeys.forEach(k=>{byAcc[k]={mv:0,cost:0,count:0,pl:0,div:0};});
  allStats.forEach(({inv,s})=>{const k=accKeys.includes(inv.account)?inv.account:'Cash';byAcc[k].mv+=s.mv;byAcc[k].cost+=s.cost;byAcc[k].count++;byAcc[k].pl+=s.pl;byAcc[k].div+=s.dtot;});
  const accSlices=accKeys.filter(k=>byAcc[k].mv>0).map(k=>({label:k,value:byAcc[k].mv,color:ACC_COLORS[k]}));
  const profData={Profit:{mv:0,count:0},Loss:{mv:0,count:0},'Break-even':{mv:0,count:0}};
  allStats.forEach(({s})=>{const k=s.pl>0.01?'Profit':s.pl<-0.01?'Loss':'Break-even';profData[k].mv+=s.mv;profData[k].count++;});
  const profSlices=Object.entries(profData).filter(([,v])=>v.count>0).map(([k,v])=>({label:k,value:v.mv,color:PROF_COLORS[k]}));
  const topHoldings=[...allStats].sort((a,b)=>b.s.mv-a.s.mv).slice(0,6),maxMv=topHoldings[0]?.s.mv||1;
  return`
<div class="summary">
  <div class="scard"><div class="lbl">Total Value</div><div class="val">${fc(totalMv,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total Cost</div><div class="val">${fc(totalCost,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total P&L</div><div class="val ${totalPl>=0?'pos':'neg'}">${totalPl>=0?'+':''}${fc(totalPl,'SGD',0)}</div></div>
  <div class="scard"><div class="lbl">Total Div</div><div class="val" style="color:var(--purple)">${fc(totalDiv,'SGD',0)}</div></div>
</div>
<div class="sumpage">
  <div class="sum-section">
    <div class="sum-section-title">📂 By Investment Type</div>
    <div class="pie-wrap"><div class="pie-title">Market Value Allocation</div><div class="pie-body">${makePie(typeSlices)}<div class="pie-legend">${typeSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card">
      <div class="bk-hdr" style="grid-template-columns:1.4fr 1fr 1fr 1fr"><span>Type</span><span>Mkt Val</span><span>P&L</span><span>Div</span></div>
      ${typeKeys.filter(k=>byType[k].count>0).map(k=>{const d=byType[k];return`<div class="bk-row" style="grid-template-columns:1.4fr 1fr 1fr 1fr"><div class="bk-label"><div class="bk-dot" style="background:${TYPE_COLORS[k]}"></div>${k}</div><div>${fc(d.mv,'SGD',0)}<div style="font-size:10px;color:var(--gray)">${d.count} holding${d.count>1?'s':''}</div></div><div style="color:${d.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div><div style="color:var(--purple);font-weight:700">${fc(d.div,'SGD',0)}</div></div>`;}).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">💳 By Payment Mode</div>
    <div class="pie-wrap"><div class="pie-title">Market Value by Account</div><div class="pie-body">${makePie(accSlices)}<div class="pie-legend">${accSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card">
      <div class="bk-hdr" style="grid-template-columns:1.1fr 1fr 1fr 1fr"><span>Account</span><span>Mkt Val</span><span>P&L</span><span>Div</span></div>
      ${accKeys.filter(k=>byAcc[k].count>0).map(k=>{const d=byAcc[k];return`<div class="bk-row" style="grid-template-columns:1.1fr 1fr 1fr 1fr"><div class="bk-label"><div class="bk-dot" style="background:${ACC_COLORS[k]}"></div>${k}</div><div>${fc(d.mv,'SGD',0)}<div style="font-size:10px;color:var(--gray)">${d.count} holding${d.count>1?'s':''}</div></div><div style="color:${d.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${d.pl>=0?'+':''}${fc(d.pl,'SGD',0)}</div><div style="color:var(--purple);font-weight:700">${fc(d.div,'SGD',0)}</div></div>`;}).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">📈 Profitability</div>
    <div class="pie-wrap"><div class="pie-title">Portfolio Value by P&L Status</div><div class="pie-body">${makePie(profSlices)}<div class="pie-legend">${profSlices.map(sl=>`<div class="pie-leg-item"><div class="pie-dot" style="background:${sl.color}"></div><span class="pie-leg-label">${sl.label}</span><span class="pie-leg-pct">${fmt(totalMv>0?sl.value/totalMv*100:0,1)}%</span></div>`).join('')}</div></div></div>
    <div class="breakdown-card">
      <div class="bk-hdr" style="grid-template-columns:1.8fr 1fr 1fr"><span>Investment</span><span>P&L</span><span>P&L %</span></div>
      ${[...allStats].sort((a,b)=>b.s.pl-a.s.pl).map(({inv,s})=>`<div class="bk-row ${s.pl>=0?'prof-winner':'prof-loser'}" style="grid-template-columns:1.8fr 1fr 1fr"><div><div style="font-weight:600;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${inv.name}</div><div style="font-size:10px;color:var(--gray)">${inv.ticker||''} · ${inv.type}</div></div><div style="color:${s.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${s.pl>=0?'+':''}${fc(s.pl,inv.currency,0)}</div><div style="color:${s.pl>=0?'var(--green)':'var(--red)'};font-weight:700">${s.pl>=0?'+':''}${fmt(s.plp,2)}%</div></div>`).join('')}
    </div>
  </div>
  <div class="sum-section">
    <div class="sum-section-title">🏆 Top Holdings</div>
    <div class="breakdown-card">
      ${topHoldings.map(({inv,s})=>{const pct=totalMv>0?s.mv/totalMv*100:0,bp=maxMv>0?s.mv/maxMv*100:0,tc=TYPE_COLORS[inv.type]||'#8e8e93';return`<div class="holding-row"><div style="flex:1;min-width:0"><div style="display:flex;justify-content:space-between;align-items:center"><div style="font-weight:600;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;max-width:60%">${inv.name}</div><div style="font-weight:700;font-size:13px">${fc(s.mv,inv.currency,0)}</div></div><div style="display:flex;justify-content:space-between;margin-top:2px"><div style="font-size:10px;color:var(--gray)">${inv.ticker||''} · <span style="color:${tc};font-weight:600">${inv.type}</span> · ${inv.account}</div><div style="font-size:10px;color:var(--gray)">${fmt(pct,1)}%</div></div><div class="holding-bar-bg"><div class="holding-bar-fill" style="width:${bp}%;background:${tc}"></div></div></div></div>`;}).join('')}
    </div>
  </div>
</div>`;}

// ─── Modals ───────────────────────────────────────────────────────────────
function renderModal(){
  if(!S.modal)return'';
  const ov=`class="overlay" onclick="bgClose(event)"`;
  if(S.modal==='addInv'||S.modal==='editInv'){
    const v=S.modal==='editInv'?S.investments.find(i=>i.id===S.editId):{};const isnew=S.modal==='addInv';
    return`<div ${ov}><div class="modal"><h2>${isnew?'Add Investment':'Edit Investment'}</h2>
      <div class="fg"><label>Search by Name or ISIN</label><input id="fsearch" placeholder="Type name, ISIN, ticker…" oninput="onNameInput(this.value)" autocomplete="off"><div id="ac-drop" class="ac-drop" style="display:none"></div><div class="hint">Tap result → ticker, currency & market auto-filled</div></div>
      <div class="fg"><label>Investment Name *</label><input id="fn" placeholder="e.g. Schroder Asian Growth" value="${v?.name||''}"></div>
      <div class="row2">
        <div class="fg"><label>Ticker / Bloomberg</label><input id="ftk" placeholder="e.g. VWRA.L or 580703" value="${v?.ticker||''}"><div class="hint">Numeric codes auto-get .SI</div></div>
        <div class="fg"><label>ISIN</label><input id="fis" placeholder="LU0943347566" value="${v?.isin||''}"><div class="hint">Fallback if ticker fails</div></div>
      </div>
      <div class="row2">
        <div class="fg"><label>Type</label><select id="fty">${['ETF','Stock','Unit Trust','Bond'].map(x=>`<option ${v?.type===x?'selected':''}>${x}</option>`).join('')}</select></div>
        <div class="fg"><label>Market</label><select id="fm">${['SGX','NYSE','NASDAQ','LSE','HKEX','Other'].map(x=>`<option ${v?.market===x?'selected':''}>${x}</option>`).join('')}</select></div>
      </div>
      <div class="row2">
        <div class="fg"><label>Currency</label><select id="fc2">${['SGD','USD','GBP','EUR','HKD'].map(x=>`<option ${v?.currency===x?'selected':''}>${x}</option>`).join('')}</select></div>
        <div class="fg"><label>Account</label><select id="fa">${['Cash','SRS','CPF'].map(x=>`<option ${v?.account===x?'selected':''}>${x}</option>`).join('')}</select></div>
      </div>
      <div class="fg"><label>Notes</label><input id="fno" placeholder="Optional" value="${v?.notes||''}"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn" onclick="saveInv()">${isnew?'Add & Fetch Price':'Save & Refresh'}</button></div>
    </div></div>`;}
  if(S.modal==='addTx'){
    const inv=S.investments.find(i=>i.id===S.txId);
    return`<div ${ov}><div class="modal"><h2>Add Transaction</h2><div class="modal-sub">${inv?.name||''} · ${inv?.currency||''}</div>
      <div class="fg"><label>Type *</label><select id="ftt">${['Buy','Sell','Rights Issue','Free Issue'].map(x=>`<option>${x}</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="ftd" type="date" value="${new Date().toISOString().slice(0,10)}"></div><div class="fg"><label>Units *</label><input id="fts" type="number" step="0.0001" placeholder="0.0000"></div></div>
      <div class="row2"><div class="fg"><label>Price / Unit</label><input id="ftp" type="number" step="0.0001" placeholder="0.0000"></div><div class="fg"><label>Fees</label><input id="ftf" type="number" step="0.01" placeholder="0.00"></div></div>
      <div class="fg"><label>Notes</label><input id="ftn" placeholder="Optional"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn" onclick="saveTx()">Add</button></div>
    </div></div>`;}
  if(S.modal==='editTx'){
    const inv=S.investments.find(i=>i.id===S.txEditInvId);const tx=(inv?.transactions||[]).find(t=>t.id===S.txEditId);if(!tx)return'';
    return`<div ${ov}><div class="modal"><h2>Edit Transaction</h2><div class="modal-sub">${inv?.name||''} · ${inv?.currency||''}</div>
      <div class="fg"><label>Type *</label><select id="ftt">${['Buy','Sell','Rights Issue','Free Issue'].map(x=>`<option ${tx.type===x?'selected':''}>${x}</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="ftd" type="date" value="${tx.date||''}"></div><div class="fg"><label>Units *</label><input id="fts" type="number" step="0.0001" value="${tx.shares||''}"></div></div>
      <div class="row2"><div class="fg"><label>Price / Unit</label><input id="ftp" type="number" step="0.0001" value="${tx.price||''}"></div><div class="fg"><label>Fees</label><input id="ftf" type="number" step="0.01" value="${tx.fees||''}"></div></div>
      <div class="fg"><label>Notes</label><input id="ftn" value="${tx.notes||''}" placeholder="Optional"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn amber" onclick="updateTx()">Save Changes</button></div>
    </div></div>`;}
  if(S.modal==='addDiv'){
    return`<div ${ov}><div class="modal"><h2>Add Dividend</h2>
      <div class="fg"><label>Investment *</label><select id="fdi">${S.investments.map(i=>`<option value="${i.id}" ${i.id===S.txId?'selected':''}>${i.name} (${i.ticker||i.isin||i.currency})</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="fdd" type="date" value="${new Date().toISOString().slice(0,10)}"></div><div class="fg"><label>Amount *</label><input id="fda" type="number" step="0.0001" placeholder="0.0000"></div></div>
      <div class="fg"><label>Type</label><select id="fdt">${['Cash','Scrip','Special','Return of Capital'].map(x=>`<option>${x}</option>`).join('')}</select></div>
      <div class="fg"><label>Notes</label><input id="fdn" placeholder="e.g. Q1 2025 dividend"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn grn" onclick="saveDiv()">Add</button></div>
    </div></div>`;}
  if(S.modal==='editDiv'){
    const dv=S.dividends.find(d=>d.id===S.divEditId);if(!dv)return'';const inv=S.investments.find(i=>i.id===dv.invId);
    return`<div ${ov}><div class="modal"><h2>Edit Dividend</h2><div class="modal-sub">${inv?.name||'Unknown'}</div>
      <div class="fg"><label>Investment</label><select id="fdi">${S.investments.map(i=>`<option value="${i.id}" ${i.id===dv.invId?'selected':''}>${i.name} (${i.ticker||i.currency})</option>`).join('')}</select></div>
      <div class="row2"><div class="fg"><label>Date *</label><input id="fdd" type="date" value="${dv.date||''}"></div><div class="fg"><label>Amount *</label><input id="fda" type="number" step="0.0001" value="${dv.amount||''}"></div></div>
      <div class="fg"><label>Type</label><select id="fdt">${['Cash','Scrip','Special','Return of Capital'].map(x=>`<option ${dv.divType===x?'selected':''}>${x}</option>`).join('')}</select></div>
      <div class="fg"><label>Notes</label><input id="fdn" value="${dv.notes||''}" placeholder="e.g. Q1 2025 dividend"></div>
      <div class="m-actions"><button class="btn out" onclick="closeM()">Cancel</button><button class="btn amber" onclick="updateDiv()">Save Changes</button></div>
    </div></div>`;}
  return'';}

// ─── Actions ──────────────────────────────────────────────────────────────
function setTab(t){S.tab=t;render();}
function setF(k,v){if(k==='c')S.fCcy=v;else S.fAcc=v;render();}
function clearSearch(){S.searchQuery='';render();}
function toggleTxAll(invId){S.showAllTx[invId]=!S.showAllTx[invId];render();}
function toggleCat(){S.catOpen=!S.catOpen;render();}
function openM(m){S.modal=m;S.editId=null;S.acResults=[];render();}
function openEdit(id){S.modal='editInv';S.editId=id;S.acResults=[];render();}
function openTx(id){S.modal='addTx';S.txId=id;render();}
function openDiv(id){S.modal='addDiv';S.txId=id;render();}
function openDivG(){S.modal='addDiv';S.txId=S.investments[0]?.id||null;render();}
function openEditTx(invId,txId){S.modal='editTx';S.txEditInvId=invId;S.txEditId=txId;render();}
function openEditDiv(divId){S.modal='editDiv';S.divEditId=divId;render();}
function closeM(){S.modal=null;S.editId=null;S.txId=null;S.txEditInvId=null;S.txEditId=null;S.divEditId=null;S.acResults=[];S.acLoading=false;render();}
function bgClose(e){if(e.target.classList.contains('overlay'))closeM();}
function saveInv(){
  const name=document.getElementById('fn').value.trim();if(!name){alert('Investment name is required.');return;}
  const ticker=document.getElementById('ftk').value.trim(),isin=document.getElementById('fis').value.trim();
  if(S.modal==='addInv'){
    const nv={id:uid(),name,ticker,isin,type:document.getElementById('fty').value,market:document.getElementById('fm').value,currency:document.getElementById('fc2').value,account:document.getElementById('fa').value,notes:document.getElementById('fno').value.trim(),spotPrice:0,transactions:[],resolvedTicker:null,resolvedBy:null};
    S.investments.push(nv);save();closeM();if(ticker||isin)setTimeout(()=>refreshOne(nv.id),300);
  }else{
    const inv=S.investments.find(i=>i.id===S.editId);
    if(inv){if(inv.ticker!==ticker||inv.isin!==isin){inv.resolvedTicker=null;inv.resolvedBy=null;}Object.assign(inv,{name,ticker,isin,type:document.getElementById('fty').value,market:document.getElementById('fm').value,currency:document.getElementById('fc2').value,account:document.getElementById('fa').value,notes:document.getElementById('fno').value.trim()});}
    save();closeM();if(inv&&(ticker||isin))setTimeout(()=>refreshOne(inv.id),300);
  }
}
function saveTx(){
  const sh=parseFloat(document.getElementById('fts').value);if(!sh||sh<=0){alert('Enter valid units.');return;}
  const inv=S.investments.find(i=>i.id===S.txId);if(!inv)return;if(!inv.transactions)inv.transactions=[];
  inv.transactions.push({id:uid(),type:document.getElementById('ftt').value,date:document.getElementById('ftd').value,shares:sh,price:parseFloat(document.getElementById('ftp').value)||0,fees:parseFloat(document.getElementById('ftf').value)||0,notes:document.getElementById('ftn').value.trim()});
  inv.transactions.sort((a,b)=>(a.date||'').localeCompare(b.date||''));save();closeM();
}
function updateTx(){
  const sh=parseFloat(document.getElementById('fts').value);if(!sh||sh<=0){alert('Enter valid units.');return;}
  const inv=S.investments.find(i=>i.id===S.txEditInvId);if(!inv)return;
  const tx=(inv.transactions||[]).find(t=>t.id===S.txEditId);if(!tx)return;
  Object.assign(tx,{type:document.getElementById('ftt').value,date:document.getElementById('ftd').value,shares:sh,price:parseFloat(document.getElementById('ftp').value)||0,fees:parseFloat(document.getElementById('ftf').value)||0,notes:document.getElementById('ftn').value.trim()});
  inv.transactions.sort((a,b)=>(a.date||'').localeCompare(b.date||''));save();toast('✅ Transaction updated');closeM();
}
function delTx(invId,txId){if(!confirm('Delete this transaction?'))return;const inv=S.investments.find(i=>i.id===invId);if(!inv)return;inv.transactions=(inv.transactions||[]).filter(t=>t.id!==txId);save();render();toast('🗑 Transaction deleted');}
function saveDiv(){
  const amt=parseFloat(document.getElementById('fda').value);if(!amt||amt<=0){alert('Enter valid amount.');return;}
  S.dividends.push({id:uid(),invId:document.getElementById('fdi').value,date:document.getElementById('fdd').value,amount:amt,divType:document.getElementById('fdt').value,notes:document.getElementById('fdn').value.trim()});
  save();closeM();
}
function updateDiv(){
  const amt=parseFloat(document.getElementById('fda').value);if(!amt||amt<=0){alert('Enter valid amount.');return;}
  const dv=S.dividends.find(d=>d.id===S.divEditId);if(!dv)return;
  Object.assign(dv,{invId:document.getElementById('fdi').value,date:document.getElementById('fdd').value,amount:amt,divType:document.getElementById('fdt').value,notes:document.getElementById('fdn').value.trim()});
  save();toast('✅ Dividend updated');closeM();
}
function delDiv(divId){if(!confirm('Delete this dividend entry?'))return;S.dividends=S.dividends.filter(d=>d.id!==divId);save();render();toast('🗑 Dividend deleted');}
function delInv(id){if(!confirm('Delete this investment and all its data?'))return;S.investments=S.investments.filter(i=>i.id!==id);S.dividends=S.dividends.filter(d=>d.invId!==id);save();render();}

// ─── Boot ─────────────────────────────────────────────────────────────────
render();
if(S.investments.some(i=>i.ticker||i.isin))setTimeout(refreshAll,1500);
</script>
</body>
</html>
