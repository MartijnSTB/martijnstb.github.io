<!DOCTYPE html>
<html lang="nl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Draaiboek — WestFriesland Radio</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,400;0,500;1,400&family=Syne:wght@600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap" rel="stylesheet">
<style>
:root {
  --rood:#C0392B; --rood-licht:#FADBD8;
  --blauw:#1A5276; --blauw-licht:#D6EAF8;
  --groen:#1E8449; --groen-licht:#D5F5E3;
  --oranje:#D35400; --oranje-licht:#FDEBD0;
  --paars:#6C3483; --paars-licht:#E8DAEF;
  --geel:#F0B429; --geel-licht:#FEFDE7;
  --donker:#14181f; --donker2:#1c2230; --donker3:#242d3d;
  --grijs:#8892a4; --rand:#2d3548; --wit:#ffffff;
  --font-display:'Syne',sans-serif; --font-body:'DM Sans',sans-serif; --font-mono:'DM Mono',monospace;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--font-body);background:var(--donker);color:var(--wit);min-height:100vh;overflow-x:hidden}

/* ── NAV ── */
.app-nav{background:var(--donker2);border-bottom:2px solid var(--rood);display:flex;align-items:center;padding:0 20px;gap:0;position:sticky;top:0;z-index:200}
.nav-logo{font-family:var(--font-display);font-weight:800;font-size:18px;padding:14px 20px 14px 0;border-right:1px solid var(--rand);margin-right:12px;white-space:nowrap}
.nav-logo span{color:var(--rood)}
.nav-tab{padding:14px 16px;font-size:13px;font-weight:500;color:var(--grijs);cursor:pointer;border-bottom:2px solid transparent;margin-bottom:-2px;transition:color .2s,border-color .2s;white-space:nowrap}
.nav-tab:hover{color:var(--wit)}
.nav-tab.active{color:var(--wit);border-bottom-color:var(--rood)}
.nav-right{margin-left:auto;display:flex;align-items:center;gap:10px}

/* live klok */
.live-clock{font-family:var(--font-mono);font-size:22px;font-weight:500;color:var(--wit);padding:8px 14px;background:var(--donker3);border-radius:8px;letter-spacing:1px;min-width:80px;text-align:center}
.live-status{display:flex;align-items:center;gap:6px;font-size:12px;font-family:var(--font-mono)}
.live-dot{width:8px;height:8px;border-radius:50%;background:var(--grijs);transition:background .3s}
.live-dot.on-air{background:#f87171;animation:pulse 1.2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}

/* ── PAGINA's ── */
.page{display:none;padding:20px 24px;max-width:1100px;margin:0 auto}
.page.active{display:block}

/* ── META BAR ── */
.meta-bar{background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:14px 18px;display:flex;align-items:center;gap:16px;margin-bottom:16px;flex-wrap:wrap}
.meta-group{display:flex;flex-direction:column;gap:3px}
.meta-label{font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs)}
.meta-input{background:var(--donker3);border:1px solid var(--rand);color:var(--wit);padding:6px 10px;border-radius:6px;font-family:var(--font-body);font-size:13px;width:150px}
.meta-input:focus{outline:none;border-color:var(--rood)}

/* ── DASHBOARD STATS ── */
.stats-bar{display:flex;gap:10px;margin-bottom:14px;flex-wrap:wrap}
.stat-card{background:var(--donker2);border:1px solid var(--rand);border-radius:8px;padding:10px 14px;display:flex;flex-direction:column;gap:2px;min-width:110px}
.stat-card-label{font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs)}
.stat-card-val{font-size:18px;font-weight:600;font-family:var(--font-mono)}
.stat-card.danger .stat-card-val{color:#f87171}
.stat-card.warning .stat-card-val{color:var(--geel)}
.stat-card.good .stat-card-val{color:#4ade80}
.stat-card.neutral .stat-card-val{color:var(--wit)}

/* tijdbalans balk */
.balans-card{background:var(--donker2);border:1px solid var(--rand);border-radius:8px;padding:10px 14px;flex:1;min-width:200px}
.balans-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.balans-lbl{font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs)}
.balans-val{font-size:16px;font-weight:600;font-family:var(--font-mono)}
.balans-track{height:8px;background:var(--donker3);border-radius:4px;overflow:hidden}
.balans-fill{height:100%;border-radius:4px;transition:width .4s,background .3s}

/* waarschuwingen */
.warnings-bar{display:flex;flex-direction:column;gap:6px;margin-bottom:14px}
.warning-item{background:rgba(248,113,113,.1);border:1px solid rgba(248,113,113,.3);border-radius:7px;padding:8px 14px;font-size:12px;color:#fca5a5;display:flex;align-items:center;gap:8px}
.warning-item.soft{background:rgba(251,191,36,.08);border-color:rgba(251,191,36,.25);color:#fde68a}

/* ── KOLOM LABELS ── */
.col-labels{display:grid;grid-template-columns:50px 76px 160px 1fr auto 100px 80px 40px;gap:0;margin-bottom:6px;padding:0 2px}
.col-lbl{font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:#3d4760;text-align:center;padding:3px}
.col-lbl:nth-child(4){text-align:left;padding-left:14px}

/* ── BLOK ── */
.blok{border-radius:9px;margin-bottom:5px;overflow:hidden;border:1.5px solid transparent;transition:border-color .2s,box-shadow .2s;position:relative}
.blok:hover{box-shadow:0 3px 16px rgba(0,0,0,.35)}
.blok.open{border-color:var(--accent-c,#444)}
.blok.live-now{border-color:#f87171!important;box-shadow:0 0 0 3px rgba(248,113,113,.2)!important}
.blok.done-blok{opacity:.55}

.blok-header{display:grid;grid-template-columns:50px 76px 160px 1fr auto 100px 80px 40px;align-items:center;cursor:pointer;user-select:none;min-height:46px}

.blok-nr{display:flex;align-items:center;justify-content:center;height:100%;min-height:46px;font-family:var(--font-mono);font-size:11px;color:rgba(255,255,255,.5);background:rgba(0,0,0,.2)}
.blok-tijd{display:flex;align-items:center;justify-content:center;height:100%;min-height:46px;font-family:var(--font-mono);font-weight:500;font-size:14px;color:var(--wit);background:rgba(0,0,0,.15);letter-spacing:.5px}
.blok-badge{display:flex;align-items:center;justify-content:center;height:100%;min-height:46px;font-size:9.5px;font-family:var(--font-mono);font-weight:500;text-transform:uppercase;letter-spacing:.7px;padding:0 10px;background:rgba(0,0,0,.1);color:rgba(255,255,255,.8);text-align:center}
.blok-inhoud{padding:0 14px;display:flex;flex-direction:column;justify-content:center;min-height:46px;overflow:hidden}
.blok-titel{font-size:13px;font-weight:500;color:var(--wit);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.blok-keynote{font-size:11px;color:rgba(255,255,255,.45);font-style:italic;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;margin-top:1px}

/* live now indicator */
.live-indicator{display:none;align-items:center;justify-content:center;height:100%;min-height:46px;padding:0 6px}
.live-indicator span{background:#f87171;color:#fff;font-size:9px;font-family:var(--font-mono);font-weight:500;padding:2px 7px;border-radius:10px;text-transform:uppercase;letter-spacing:.5px;animation:pulse 1.2s infinite}
.blok.live-now .live-indicator{display:flex}

/* countdown */
.blok-countdown{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100%;min-height:46px;padding:0 6px}
.countdown-val{font-family:var(--font-mono);font-size:12px;font-weight:500;color:var(--grijs)}
.countdown-val.urgent{color:#f87171}
.countdown-val.soon{color:var(--geel)}

.blok-duur-wrap{display:flex;align-items:center;justify-content:center;height:100%;min-height:46px;padding:0 6px;gap:4px}
.duur-input{background:rgba(0,0,0,.25);border:1px solid rgba(255,255,255,.12);color:var(--wit);font-family:var(--font-mono);font-size:13px;font-weight:500;text-align:center;padding:5px 6px;border-radius:6px;width:62px;transition:border-color .2s}
.duur-input:focus{outline:none;border-color:var(--geel);background:rgba(0,0,0,.4)}
.duur-input[readonly]{opacity:.4;cursor:default;pointer-events:none}

/* drag handle */
.drag-handle{display:flex;align-items:center;justify-content:center;height:100%;min-height:46px;color:rgba(255,255,255,.2);cursor:grab;font-size:14px;padding:0 4px;background:rgba(0,0,0,.1)}
.drag-handle:active{cursor:grabbing}

.blok-toggle{display:flex;align-items:center;justify-content:center;height:100%;min-height:46px;color:rgba(255,255,255,.35);font-size:16px;transition:transform .25s,color .2s;background:rgba(0,0,0,.1)}
.blok.open .blok-toggle{transform:rotate(180deg);color:rgba(255,255,255,.7)}

/* blok types */
.type-O,.type-A{background:linear-gradient(90deg,#252b3b,#1e2433);--accent-c:#6b7280}
.type-T{background:linear-gradient(90deg,#1e2433,#192030);--accent-c:#6b7280}
.type-L{background:linear-gradient(90deg,#3f1010,#331010);--accent-c:#ef4444}
.type-R{background:linear-gradient(90deg,#0b2235,#091c2d);--accent-c:#3b82f6}
.type-I{background:linear-gradient(90deg,#25103a,#1e0c2e);--accent-c:#a855f7}
.type-M{background:linear-gradient(90deg,#0c2415,#0a1e10);--accent-c:#22c55e}
.type-V{background:linear-gradient(90deg,#321808,#271206);--accent-c:#f97316}

.type-L .blok-badge{color:#fca5a5}.type-R .blok-badge{color:#93c5fd}
.type-I .blok-badge{color:#d8b4fe}.type-M .blok-badge{color:#86efac}
.type-V .blok-badge{color:#fdba74}
.type-L .duur-input{border-color:rgba(239,68,68,.35)}
.type-R .duur-input{border-color:rgba(59,130,246,.35)}
.type-I .duur-input{border-color:rgba(168,85,247,.35)}
.type-M .duur-input{border-color:rgba(34,197,94,.35)}
.type-V .duur-input{border-color:rgba(249,115,22,.35)}

/* ── DETAIL ── */
.blok-detail{display:none;background:var(--donker2);border-top:1px solid rgba(255,255,255,.06);padding:18px 22px 20px;animation:slideDown .18s ease}
.blok.open .blok-detail{display:block}
@keyframes slideDown{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:translateY(0)}}

.detail-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.field{display:flex;flex-direction:column;gap:4px}
.field.full{grid-column:1/-1}
.field-label{font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs)}
.field-input,.field-textarea,.field-select{background:var(--donker3);border:1px solid var(--rand);color:var(--wit);font-family:var(--font-body);font-size:13px;padding:8px 11px;border-radius:7px;width:100%;transition:border-color .2s}
.field-input:focus,.field-textarea:focus,.field-select:focus{outline:none;border-color:var(--rood)}
.field-textarea{resize:none;min-height:66px;line-height:1.5;overflow:hidden;box-sizing:border-box}
.field-select{cursor:pointer}

/* detail acties */
.detail-actions{display:flex;gap:8px;margin-top:14px;padding-top:12px;border-top:1px solid var(--rand)}

/* ── BLOK TOEVOEGEN ── */
.add-blok-bar{display:flex;gap:6px;flex-wrap:wrap;padding:10px 0 4px}
.add-btn{background:var(--donker3);border:1px dashed var(--rand);color:var(--grijs);padding:6px 12px;border-radius:6px;font-size:11px;font-family:var(--font-mono);cursor:pointer;transition:border-color .2s,color .2s}
.add-btn:hover{border-color:var(--geel);color:var(--geel)}

/* ── KNOPPEN ── */
.btn{display:inline-flex;align-items:center;gap:5px;background:var(--rood);color:#fff;border:none;padding:7px 14px;border-radius:7px;font-family:var(--font-body);font-size:12px;font-weight:500;cursor:pointer;transition:background .2s}
.btn:hover{background:#a93226}
.btn-sm{padding:5px 10px;font-size:11px}
.btn-ghost{background:var(--donker3);color:var(--grijs);border:1px solid var(--rand)}
.btn-ghost:hover{color:var(--wit);border-color:var(--grijs)}
.btn-green{background:var(--groen)}.btn-green:hover{background:#166638}
.btn-orange{background:var(--oranje)}.btn-orange:hover{background:#b04500}
.btn-danger{background:#7f1d1d;color:#fca5a5}.btn-danger:hover{background:#991b1b}

/* ── SEPARATOR ── */
.hour-sep{text-align:center;padding:10px 0 7px;font-family:var(--font-mono);font-size:10px;color:#3d4760;text-transform:uppercase;letter-spacing:2px;display:flex;align-items:center;gap:10px}
.hour-sep::before,.hour-sep::after{content:'';flex:1;height:1px;background:var(--rand)}

/* ── MODALVENSTER ── */
.modal-bg{display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:500;align-items:center;justify-content:center}
.modal-bg.open{display:flex}
.modal{background:var(--donker2);border:1px solid var(--rand);border-radius:12px;padding:24px;max-width:480px;width:90%;max-height:80vh;overflow-y:auto}
.modal h3{font-family:var(--font-display);font-size:18px;margin-bottom:16px}
.modal-actions{display:flex;gap:8px;margin-top:20px;justify-content:flex-end}

/* ── CONTACTEN PAGINA ── */
.contacts-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:20px}
.contact-card{background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:16px;position:relative}
.contact-card-title{font-family:var(--font-mono);font-size:10px;text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:10px;display:flex;justify-content:space-between;align-items:center}
.contact-fields{display:flex;flex-direction:column;gap:7px}
.contact-row{display:grid;grid-template-columns:90px 1fr;gap:6px;align-items:center}
.contact-row-label{font-size:11px;color:var(--grijs);font-family:var(--font-mono)}
.contact-input{background:var(--donker3);border:1px solid var(--rand);color:var(--wit);padding:5px 9px;border-radius:5px;font-size:12px;font-family:var(--font-body);width:100%}
.contact-input:focus{outline:none;border-color:var(--rood)}

/* notities blok */
.notes-block{background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:16px;margin-bottom:12px}
.notes-block h3{font-family:var(--font-mono);font-size:10px;text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:10px}
.notes-textarea{background:var(--donker3);border:1px solid var(--rand);color:var(--wit);padding:10px;border-radius:7px;width:100%;font-family:var(--font-body);font-size:13px;line-height:1.6;resize:none;min-height:80px;overflow:hidden;box-sizing:border-box}
.notes-textarea:focus{outline:none;border-color:var(--rood)}

/* ── CHECKLIST PAGINA ── */
.checklist-section{background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:16px;margin-bottom:12px}
.checklist-section h3{font-family:var(--font-display);font-size:14px;margin-bottom:12px;color:var(--wit)}
.check-item{display:flex;align-items:center;gap:10px;padding:7px 0;border-bottom:1px solid rgba(255,255,255,.04)}
.check-item:last-child{border-bottom:none}
.check-item input[type=checkbox]{width:16px;height:16px;accent-color:var(--rood);cursor:pointer;flex-shrink:0}
.check-item label{font-size:13px;color:rgba(255,255,255,.8);cursor:pointer;flex:1}
.check-item input:checked+label{text-decoration:line-through;color:var(--grijs)}
.check-progress{font-size:11px;font-family:var(--font-mono);color:var(--grijs);margin-bottom:8px}

/* ── PRINT ── */
@media print{
  *{-webkit-print-color-adjust:exact!important;print-color-adjust:exact!important}
  .app-nav,.add-blok-bar,.drag-handle,.detail-actions,.warnings-bar,.add-btn,.blok-toggle,
  .blok-countdown,.live-indicator,.col-labels,.stats-bar,.balans-card{display:none!important}
  body{background:#f4f4f4!important;color:#111!important;font-size:11px}
  .page{display:none!important;padding:10px}
  #page-draaiboek{display:block!important}
  .meta-bar{background:#fff!important;border:1px solid #ccc!important;border-radius:6px;padding:10px 14px;margin-bottom:10px;flex-wrap:wrap;gap:10px}
  .meta-label{color:#666!important}
  .meta-input{background:#fff!important;border:none!important;color:#111!important;font-weight:600;width:auto}
  #page-draaiboek::before{
    content:'DRAAIBOEK \2014  WestFriesland  |  Zaterdag 12:00\201314:00';
    display:block;background:#C0392B;color:#fff;font-family:sans-serif;
    font-size:15px;font-weight:800;text-align:center;padding:10px;
    border-radius:6px;margin-bottom:10px;
  }
  .blok{border-radius:6px;margin-bottom:6px;break-inside:avoid;overflow:visible;border:none!important;box-shadow:0 1px 4px rgba(0,0,0,.15)!important}
  .blok-detail{display:block!important;padding:10px 14px}
  .blok-header{min-height:36px}
  .blok-nr{min-height:36px;color:#fff!important;font-size:10px}
  .blok-tijd{min-height:36px;color:#fff!important;font-size:13px;font-weight:700}
  .blok-badge{min-height:36px;font-size:9px;color:#fff!important}
  .blok-inhoud{min-height:36px}
  .blok-titel{font-size:12px;color:#fff!important;font-weight:600}
  .blok-keynote{font-size:10px;color:rgba(255,255,255,.8)!important}
  .blok-duur-wrap{min-height:36px}
  .duur-input{color:#fff!important;border-color:rgba(255,255,255,.3)!important;background:rgba(0,0,0,.15)!important;font-size:12px}
  .type-O .blok-header,.type-A .blok-header{background:#4a4a5a!important}
  .type-T .blok-header{background:#2d3548!important}
  .type-L .blok-header{background:#8b1a1a!important}
  .type-R .blok-header{background:#1a3a5c!important}
  .type-I .blok-header{background:#4a1a6a!important}
  .type-M .blok-header{background:#1a5e30!important}
  .type-V .blok-header{background:#7a3a10!important}
  .type-L .blok-nr,.type-R .blok-nr,.type-I .blok-nr,.type-M .blok-nr,.type-V .blok-nr{background:rgba(0,0,0,.25)!important}
  .type-O .blok-detail,.type-A .blok-detail{background:#f0f0f0!important}
  .type-T .blok-detail{background:#f5f5f5!important}
  .type-L .blok-detail{background:#fff0f0!important}
  .type-R .blok-detail{background:#f0f4ff!important}
  .type-I .blok-detail{background:#f8f0ff!important}
  .type-M .blok-detail{background:#f0fff4!important}
  .type-V .blok-detail{background:#fff8f0!important}
  .detail-grid{gap:6px}
  .field-label{color:#555!important;font-size:8px}
  .field-input,.field-textarea,.field-select{background:#fff!important;border:1px solid #ddd!important;color:#111!important;font-size:11px;padding:5px 8px}
  .field-textarea,.field-input{height:auto!important;min-height:0!important;overflow:visible!important;white-space:pre-wrap!important;word-break:break-word!important}
  .hour-sep{color:#888!important;font-size:9px}
  .hour-sep::before,.hour-sep::after{background:#ccc}
}

.btn-ghost.active{color:var(--wit);border-color:var(--rood);background:rgba(192,57,43,.15)}

/* drag & drop */
.blok.dragging{opacity:.4;transform:scale(.98)}
.blok.drag-over{border-color:var(--geel)!important;border-style:dashed!important}
</style>
</head>
<body>

<!-- ── NAVIGATIE ── -->
<nav class="app-nav">
  <div class="nav-logo">West<span>Friesland</span> Radio</div>
  <div class="nav-tab active" onclick="showPage('draaiboek',this)">📋 Draaiboek</div>
  <div class="nav-tab" onclick="showPage('checklist',this)">✅ Checklist</div>
  <div class="nav-tab" onclick="showPage('contacts',this)">📞 Contacten</div>
  <div class="nav-tab" onclick="showPage('muziek',this)">🎵 Muziekbibliotheek</div>
  <div class="nav-tab" onclick="showPage('sjablonen',this)">📋 Sjablonen</div>
  <div class="nav-tab" onclick="showPage('weekplan',this)">🗓 Weekplanning</div>
  <div class="nav-tab" onclick="showPage('statistieken',this)">📊 Statistieken</div>
  <div class="nav-tab" onclick="showPage('stopwatch',this)">⏱ Timer</div>
  <div class="nav-tab" onclick="showPage('archief',this)">📂 Archief</div>
  <div class="nav-right">
    <div class="live-status">
      <div class="live-dot" id="live-dot"></div>
      <span id="live-status-txt" style="font-size:11px;color:var(--grijs)">Niet op lucht</span>
    </div>
    <div id="uitz-timer" style="display:none;font-family:var(--font-mono);font-size:13px;color:#4ade80;background:rgba(30,132,73,.15);border:1px solid rgba(30,132,73,.3);padding:4px 10px;border-radius:6px;letter-spacing:.5px">⏱ 00:00</div>
    <div class="live-clock" id="live-clock">--:--:--</div>
    <button class="btn btn-sm btn-ghost" onclick="toggleOnAir()" id="onair-btn">▶ Start uitzending</button>
    <button class="btn btn-sm btn-ghost" onclick="toggleScratchpad()" title="Snelle notities">📝</button>
    <button class="btn btn-sm btn-ghost" onclick="showResetModal()">↺ Nieuwe week</button>
    <button class="btn btn-sm btn-ghost" onclick="window.print()">🖨 Print</button>
  </div>
</nav>

<!-- ── ZWEVENDE SCRATCHPAD ── -->
<div id="scratchpad" style="display:none;position:fixed;bottom:20px;right:20px;width:300px;background:var(--donker2);border:1px solid var(--geel);border-radius:10px;z-index:500;box-shadow:0 8px 32px rgba(0,0,0,.5)">
  <div style="background:var(--geel);padding:8px 12px;border-radius:9px 9px 0 0;display:flex;align-items:center;justify-content:space-between">
    <span style="font-size:12px;font-weight:600;color:#1a1a00;font-family:var(--font-mono)">📝 SNELLE NOTITIES</span>
    <span onclick="toggleScratchpad()" style="cursor:pointer;color:#1a1a00;font-size:16px;line-height:1">×</span>
  </div>
  <textarea id="scratchpad-text" placeholder="Typ hier snel notities tijdens de uitzending — luisteraarsreacties, namen, ideeën…" style="width:100%;height:180px;background:transparent;border:none;color:var(--wit);padding:12px;font-family:var(--font-body);font-size:13px;line-height:1.5;resize:none;outline:none"></textarea>
  <div style="padding:6px 12px;border-top:1px solid var(--rand);display:flex;justify-content:space-between;align-items:center">
    <span style="font-size:10px;color:var(--grijs);font-family:var(--font-mono)">Automatisch opgeslagen</span>
    <button class="btn btn-sm btn-ghost" style="font-size:10px;padding:3px 8px" onclick="document.getElementById('scratchpad-text').value='';localStorage.removeItem('wf_scratchpad')">Wissen</button>
  </div>
</div>

<!-- ── DRAAIBOEK PAGINA ── -->
<div class="page active" id="page-draaiboek">

  <div class="meta-bar">
    <div class="meta-group">
      <div class="meta-label">Uitzenddatum</div>
      <input class="meta-input" id="meta-datum" type="text" placeholder="zaterdag dd-mm-jjjj">
    </div>
    <div class="meta-group">
      <div class="meta-label">Presentator</div>
      <input class="meta-input" id="meta-presentator" type="text" placeholder="Naam presentator">
    </div>
    <div class="meta-group">
      <div class="meta-label">Eindredacteur</div>
      <input class="meta-input" id="meta-redacteur" type="text" placeholder="Naam eindredacteur">
    </div>
    <div class="meta-group">
      <div class="meta-label">Technicus</div>
      <input class="meta-input" id="meta-technicus" type="text" placeholder="Naam technicus">
    </div>
    <div class="meta-group">
      <div class="meta-label">Thema uitzending</div>
      <input class="meta-input" style="width:220px" id="meta-thema" type="text" placeholder="Bijv. 'Woningnood in de regio'">
    </div>
  </div>

  <!-- warnings -->
  <div class="warnings-bar" id="warnings-bar"></div>

  <!-- stats -->
  <div class="stats-bar">
    <div class="stat-card neutral"><div class="stat-card-label">Totale duur</div><div class="stat-card-val" id="stat-totaal">—</div></div>
    <div class="stat-card neutral" id="stat-balans-card"><div class="stat-card-label">Over / Tekort</div><div class="stat-card-val" id="stat-delta">—</div></div>
    <div class="stat-card neutral"><div class="stat-card-label">Muziek</div><div class="stat-card-val" id="stat-muziek">—</div></div>
    <div class="stat-card neutral"><div class="stat-card-label">Inhoud</div><div class="stat-card-val" id="stat-inhoud">—</div></div>
    <div class="stat-card neutral"><div class="stat-card-label">Klaar</div><div class="stat-card-val" id="stat-klaar">—</div></div>
    <div class="balans-card">
      <div class="balans-top">
        <span class="balans-lbl">Tijdbalans (doel: 120 min)</span>
        <span class="balans-val" id="balans-val">—</span>
      </div>
      <div class="balans-track"><div class="balans-fill" id="balans-fill" style="width:0%"></div></div>
    </div>
  </div>

  <!-- kolom labels -->
  <div class="col-labels">
    <span class="col-lbl">#</span>
    <span class="col-lbl">Start</span>
    <span class="col-lbl">Type blok</span>
    <span class="col-lbl">Onderwerp / Inhoud</span>
    <span class="col-lbl">Nu</span>
    <span class="col-lbl">Duur (mm:ss)</span>
    <span class="col-lbl">Resterend</span>
    <span class="col-lbl"></span>
  </div>

  <div id="programma"></div>

  <!-- blok toevoegen -->
  <div class="add-blok-bar">
    <span style="font-size:10px;font-family:var(--font-mono);color:var(--grijs);align-self:center">+ Blok toevoegen:</span>
    <button class="add-btn" onclick="addBlok('T')">📰 Item</button>
    <button class="add-btn" onclick="addBlok('M')">🎵 Muziek</button>
    <button class="add-btn" onclick="addBlok('I')">🎙️ Interview</button>
    <button class="add-btn" onclick="addBlok('V')">📋 Rubriek</button>
    <button class="add-btn" onclick="addBlok('L')">📡 Landelijk nieuws</button>
    <button class="add-btn" onclick="addBlok('R')">📡 Regionaal nieuws</button>
  </div>
</div>

<!-- ── CHECKLIST PAGINA ── -->
<div class="page" id="page-checklist">
  <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:16px">
    <h2 style="font-family:var(--font-display);font-size:22px">✅ Voorbereiding checklist</h2>
    <button class="btn btn-sm btn-ghost" onclick="resetChecklist()">↺ Reset checklist</button>
  </div>

  <div class="checklist-section">
    <h3>📅 Dag vóór de uitzending</h3>
    <div class="check-progress" id="cp-dag"></div>
    <div id="cl-dag">
      <div class="check-item"><input type="checkbox" id="c1"><label for="c1">Thema en items bepaald voor de uitzending</label></div>
      <div class="check-item"><input type="checkbox" id="c2"><label for="c2">Gasten bevestigd en uitgenodigd</label></div>
      <div class="check-item"><input type="checkbox" id="c3"><label for="c3">Interviewvragen voorbereid en goedgekeurd</label></div>
      <div class="check-item"><input type="checkbox" id="c4"><label for="c4">Stelling voor De West-Friese Kwestie vastgesteld</label></div>
      <div class="check-item"><input type="checkbox" id="c5"><label for="c5">Muzieklijst samengesteld en ingepland</label></div>
      <div class="check-item"><input type="checkbox" id="c6"><label for="c6">Eindredacteur heeft draaiboek goedgekeurd</label></div>
      <div class="check-item"><input type="checkbox" id="c7"><label for="c7">Contactgegevens gasten gecontroleerd</label></div>
      <div class="check-item"><input type="checkbox" id="c8"><label for="c8">Nieuwsredactie geïnformeerd over bulletintijden</label></div>
      <div class="check-item"><input type="checkbox" id="c9"><label for="c9">Reactiekanaal voor luisteraars actief (app/sms)</label></div>
    </div>
  </div>

  <div class="checklist-section">
    <h3>🎙️ Ochtend van de uitzending</h3>
    <div class="check-progress" id="cp-ochtend"></div>
    <div id="cl-ochtend">
      <div class="check-item"><input type="checkbox" id="d1"><label for="d1">Studio gereserveerd en open</label></div>
      <div class="check-item"><input type="checkbox" id="d2"><label for="d2">Technicus aanwezig en geïnformeerd</label></div>
      <div class="check-item"><input type="checkbox" id="d3"><label for="d3">Muzieknummers ingeladen in het systeem</label></div>
      <div class="check-item"><input type="checkbox" id="d4"><label for="d4">Microfooncheck gedaan</label></div>
      <div class="check-item"><input type="checkbox" id="d5"><label for="d5">Hoofdtelefoons getest</label></div>
      <div class="check-item"><input type="checkbox" id="d6"><label for="d6">Telefoonlijn getest (voor eventueel telefonisch interview)</label></div>
      <div class="check-item"><input type="checkbox" id="d7"><label for="d7">Actueel nieuws gecheckt — aanpassingen in draaiboek?</label></div>
      <div class="check-item"><input type="checkbox" id="d8"><label for="d8">Opening en aankondigingen doorgelezen</label></div>
      <div class="check-item"><input type="checkbox" id="d9"><label for="d9">Gast(en) aanwezig of verbinding opgezet</label></div>
      <div class="check-item"><input type="checkbox" id="d10"><label for="d10">Draaiboek geopend en klaar op scherm</label></div>
    </div>
  </div>

  <div class="checklist-section">
    <h3>📡 Tijdens de uitzending</h3>
    <div class="check-progress" id="cp-tijdens"></div>
    <div id="cl-tijdens">
      <div class="check-item"><input type="checkbox" id="e1"><label for="e1">12:00 — Opening gedaan</label></div>
      <div class="check-item"><input type="checkbox" id="e2"><label for="e2">12:30 — Regionaal bulletin afgevinkt</label></div>
      <div class="check-item"><input type="checkbox" id="e3"><label for="e3">13:00 — Landelijk bulletin afgevinkt</label></div>
      <div class="check-item"><input type="checkbox" id="e4"><label for="e4">13:30 — Regionaal bulletin afgevinkt</label></div>
      <div class="check-item"><input type="checkbox" id="e5"><label for="e5">Reacties luisteraars verwerkt in De West-Friese Kwestie</label></div>
      <div class="check-item"><input type="checkbox" id="e6"><label for="e6">14:00 — Afsluiting gedaan en studio verlaten</label></div>
    </div>
  </div>

  <div class="checklist-section">
    <h3>🔄 Na de uitzending</h3>
    <div class="check-progress" id="cp-na"></div>
    <div id="cl-na">
      <div class="check-item"><input type="checkbox" id="f1"><label for="f1">Studio opgeruimd</label></div>
      <div class="check-item"><input type="checkbox" id="f2"><label for="f2">Evaluatie ingevuld in draaiboek (Contacten → Notities)</label></div>
      <div class="check-item"><input type="checkbox" id="f3"><label for="f3">Uitzending gecontroleerd op terugluister-platform</label></div>
      <div class="check-item"><input type="checkbox" id="f4"><label for="f4">Sociale media bijgewerkt</label></div>
      <div class="check-item"><input type="checkbox" id="f5"><label for="f5">Ideeën voor volgende week genoteerd</label></div>
    </div>
  </div>
</div>

<!-- ── CONTACTEN PAGINA ── -->
<div class="page" id="page-contacts">

  <!-- toolbar -->
  <div style="display:flex;align-items:center;gap:10px;margin-bottom:14px;flex-wrap:wrap">
    <h2 style="font-family:var(--font-display);font-size:22px;margin-right:4px">📞 Contacten</h2>
    <div style="flex:1;min-width:180px;position:relative">
      <span style="position:absolute;left:10px;top:50%;transform:translateY(-50%);color:var(--grijs);font-size:14px">🔍</span>
      <input id="contact-zoek" type="text" placeholder="Zoek op naam, onderwerp, organisatie…"
        style="width:100%;background:var(--donker3);border:1px solid var(--rand);color:var(--wit);padding:8px 10px 8px 32px;border-radius:7px;font-family:var(--font-body);font-size:13px"
        oninput="filterContacts()">
    </div>
    <div style="display:flex;gap:6px;flex-wrap:wrap">
      <button class="btn btn-sm btn-ghost ct-filter-btn active" data-cat="" onclick="setCatFilter('',this)">Alle</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Politiek"     onclick="setCatFilter('Politiek',this)">🏛 Politiek</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Maatschappij" onclick="setCatFilter('Maatschappij',this)">🤝 Maatschappij</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Cultuur"      onclick="setCatFilter('Cultuur',this)">🎭 Cultuur</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Sport"        onclick="setCatFilter('Sport',this)">⚽ Sport</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Onderwijs"    onclick="setCatFilter('Onderwijs',this)">📚 Onderwijs</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Economie"     onclick="setCatFilter('Economie',this)">💼 Economie</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Techniek"     onclick="setCatFilter('Techniek',this)">⚙️ Techniek</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="Overig"       onclick="setCatFilter('Overig',this)">📂 Overig</button>
      <button class="btn btn-sm btn-ghost ct-filter-btn" data-cat="⭐"           onclick="setCatFilter('⭐',this)">⭐ Favorieten</button>
    </div>
    <button class="btn btn-sm btn-green" onclick="newContact()">+ Nieuw contact</button>
  </div>

  <div id="ct-geen" style="display:none;background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:32px;text-align:center;color:var(--grijs)">
    <div style="font-size:32px;margin-bottom:10px">🔍</div>
    <div style="font-size:14px">Geen contacten gevonden.</div>
  </div>
  <div id="contacts-lijst" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(320px,1fr));gap:10px"></div>

  <!-- notities sectie -->
  <div style="margin-top:24px;border-top:1px solid var(--rand);padding-top:20px">
    <h3 style="font-family:var(--font-display);font-size:16px;margin-bottom:12px">📝 Redactionele notities</h3>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px">
      <div>
        <div style="font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:5px">Evaluatie & wat goed ging</div>
        <textarea class="notes-textarea" id="notes-redactie" placeholder="Noteer hier redactionele beslissingen, wat goed ging, wat beter kan…"></textarea>
      </div>
      <div>
        <div style="font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:5px">Ideeën voor volgende uitzendingen</div>
        <textarea class="notes-textarea" id="notes-ideeen" placeholder="Onderwerpen, gasten, rubrieken — alles wat je niet wil vergeten…"></textarea>
      </div>
      <div style="grid-column:1/-1">
        <div style="font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:5px">⚙️ Technische aandachtspunten</div>
        <textarea class="notes-textarea" id="notes-tech" placeholder="Technische bijzonderheden voor de technicus: inlades, verbindingen, niveaus, jingles…"></textarea>
      </div>
    </div>
  </div>
</div>

<!-- ── CONTACT EDIT MODAL ── -->
<div class="modal-bg" id="contact-modal">
  <div class="modal" style="max-width:560px;width:95%">
    <h3 id="contact-modal-title">Nieuw contact</h3>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:14px">

      <div class="field full">
        <label class="field-label">Naam *</label>
        <input class="field-input" id="cm-naam" placeholder="Volledige naam">
      </div>
      <div class="field">
        <label class="field-label">Organisatie / Medium</label>
        <input class="field-input" id="cm-org" placeholder="Bijv. Gemeente Hoorn, NHD…">
      </div>
      <div class="field">
        <label class="field-label">Functie / Rol</label>
        <input class="field-input" id="cm-functie" placeholder="Bijv. Wethouder, Woordvoerder…">
      </div>
      <div class="field">
        <label class="field-label">Telefoon (mobiel)</label>
        <input class="field-input" id="cm-tel" placeholder="06-…" type="tel">
      </div>
      <div class="field">
        <label class="field-label">Telefoon (vast / werk)</label>
        <input class="field-input" id="cm-telv" placeholder="0229-…" type="tel">
      </div>
      <div class="field">
        <label class="field-label">E-mail</label>
        <input class="field-input" id="cm-email" placeholder="naam@…" type="email">
      </div>
      <div class="field full">
        <label class="field-label">Onderwerp / Expertise</label>
        <input class="field-input" id="cm-onderwerp" placeholder="Bijv. woningmarkt, landbouw, gemeentepolitiek…">
      </div>
      <div class="field">
        <label class="field-label">Categorie</label>
        <select class="field-select" id="cm-cat">
          <option value="">— Kies categorie —</option>
          <option>Politiek</option><option>Maatschappij</option><option>Cultuur</option>
          <option>Sport</option><option>Onderwijs</option><option>Economie</option>
          <option>Techniek</option><option>Overig</option>
        </select>
      </div>
      <div class="field">
        <label class="field-label">Eerder te gast (datum)</label>
        <input class="field-input" id="cm-gast" placeholder="Bijv. 12-04-2025">
      </div>
      <div class="field full">
        <label class="field-label">Bereikbaarheid</label>
        <input class="field-input" id="cm-bereik" placeholder="Bijv. niet op maandag, voorkeur WhatsApp, alleen 's ochtends…">
      </div>
      <div class="field full">
        <label class="field-label">Notities (vrij veld)</label>
        <textarea class="field-textarea" id="cm-notitie" rows="3" placeholder="Alles wat niet in een ander veld past…"></textarea>
      </div>
      <div class="field full" style="display:flex;align-items:center;gap:8px">
        <input type="checkbox" id="cm-fav" style="width:16px;height:16px;accent-color:var(--geel)">
        <label for="cm-fav" style="font-size:13px;cursor:pointer">⭐ Markeren als favoriet</label>
      </div>
    </div>
    <div class="modal-actions">
      <button class="btn btn-sm btn-danger" id="cm-delete-btn" style="margin-right:auto;display:none" onclick="deleteContact()">🗑 Verwijderen</button>
      <button class="btn btn-ghost btn-sm" onclick="closeContactModal()">Annuleren</button>
      <button class="btn btn-sm btn-green" onclick="saveContact()">💾 Opslaan</button>
    </div>
  </div>
</div>

<!-- ── ARCHIEF PAGINA ── -->
<div class="page" id="page-archief">
  <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:16px">
    <h2 style="font-family:var(--font-display);font-size:22px">📂 Archief</h2>
    <button class="btn btn-sm btn-danger" onclick="clearAllArchive()">🗑 Volledig archief wissen</button>
  </div>
  <p style="color:var(--grijs);font-size:13px;margin-bottom:16px">Hier vind je alle opgeslagen uitzendingen. Klik op een uitzending om alle details te bekijken.</p>
  <div id="archief-lijst"></div>
</div>

<!-- ── MUZIEKBIBLIOTHEEK PAGINA ── -->
<div class="page" id="page-muziek">
  <div style="display:flex;align-items:center;gap:10px;margin-bottom:14px;flex-wrap:wrap">
    <h2 style="font-family:var(--font-display);font-size:22px">🎵 Muziekbibliotheek</h2>
    <div style="flex:1;min-width:160px;position:relative">
      <span style="position:absolute;left:10px;top:50%;transform:translateY(-50%);color:var(--grijs)">🔍</span>
      <input id="muz-zoek" type="text" placeholder="Zoek artiest of titel…" oninput="renderMuziek()"
        style="width:100%;background:var(--donker3);border:1px solid var(--rand);color:var(--wit);padding:8px 10px 8px 32px;border-radius:7px;font-family:var(--font-body);font-size:13px">
    </div>
    <select id="muz-genre-filter" onchange="renderMuziek()" style="background:var(--donker3);border:1px solid var(--rand);color:var(--wit);padding:7px 10px;border-radius:7px;font-size:13px">
      <option value="">Alle genres</option>
      <option>Pop</option><option>Rock</option><option>Folk</option><option>Klassiek</option>
      <option>Jazz</option><option>Wereldmuziek</option><option>Regionaal</option><option>Overig</option>
    </select>
    <button class="btn btn-sm btn-green" onclick="newMuziek()">+ Nummer toevoegen</button>
  </div>
  <div style="display:flex;gap:10px;margin-bottom:14px;flex-wrap:wrap">
    <div class="stat-card neutral"><div class="stat-card-label">Bibliotheek</div><div class="stat-card-val" id="muz-stat-totaal">—</div></div>
    <div class="stat-card neutral"><div class="stat-card-label">Totale duur</div><div class="stat-card-val" id="muz-stat-duur">—</div></div>
    <div class="stat-card neutral"><div class="stat-card-label">Regionaal</div><div class="stat-card-val" id="muz-stat-regio">—</div></div>
  </div>
  <div id="muz-lijst" style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;overflow:hidden"></div>
</div>

<!-- ── SJABLONEN PAGINA ── -->
<div class="page" id="page-sjablonen">
  <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:16px">
    <div>
      <h2 style="font-family:var(--font-display);font-size:22px">📋 Sjablonen</h2>
      <p style="color:var(--grijs);font-size:13px;margin-top:4px">Klik op "Zet in draaiboek" om een sjabloon als blok toe te voegen aan het huidige draaiboek.</p>
    </div>
    <button class="btn btn-sm btn-ghost" onclick="nieuwSjabloon()">+ Nieuw sjabloon</button>
  </div>
  <div id="sj-lijst" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:10px"></div>
</div>

<!-- ── WEEKPLANNING PAGINA ── -->
<div class="page" id="page-weekplan">
  <div style="margin-bottom:16px">
    <h2 style="font-family:var(--font-display);font-size:22px">🗓 Weekplanning</h2>
    <p style="color:var(--grijs);font-size:13px;margin-top:4px">Plan gasten, onderwerpen en rubrieken in voor komende uitzendingen.</p>
  </div>
  <div id="weekplan-content"></div>
</div>

<!-- ── STATISTIEKEN PAGINA ── -->
<div class="page" id="page-statistieken">
  <div style="margin-bottom:16px">
    <h2 style="font-family:var(--font-display);font-size:22px">📊 Statistieken</h2>
    <p style="color:var(--grijs);font-size:13px;margin-top:4px">Overzicht op basis van je gearchiveerde uitzendingen.</p>
  </div>
  <div id="stats-content"></div>
</div>

<!-- ── STOPWATCH / TIMER PAGINA ── -->
<div class="page" id="page-stopwatch">
  <h2 style="font-family:var(--font-display);font-size:22px;margin-bottom:16px">⏱ Segmenttimer & Stopwatch</h2>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
    <div style="background:var(--donker2);border:1px solid var(--rand);border-radius:12px;padding:28px;text-align:center">
      <div style="font-size:11px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:1px;color:var(--grijs);margin-bottom:12px">Stopwatch</div>
      <div id="sw-display" style="font-family:var(--font-mono);font-size:56px;font-weight:600;color:var(--wit);letter-spacing:2px;margin-bottom:20px">00:00</div>
      <div style="display:flex;gap:8px;justify-content:center;flex-wrap:wrap">
        <button id="sw-btn" class="btn" onclick="swToggle()" style="min-width:100px">▶ Start</button>
        <button class="btn btn-ghost" onclick="swLap()">📍 Lap</button>
        <button class="btn btn-ghost" onclick="swReset()">↺ Reset</button>
      </div>
      <div id="sw-laps" style="margin-top:16px;text-align:left;max-height:200px;overflow-y:auto"></div>
    </div>
    <div style="background:var(--donker2);border:1px solid var(--rand);border-radius:12px;padding:28px">
      <div style="font-size:11px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:1px;color:var(--grijs);margin-bottom:16px">Handige tijden</div>
      ${[['Opening','3:00'],['Item / Inleiding','4:00'],['Interview kort','5:00'],['Interview lang','10:00'],['Muzieknummer','3:30'],['Vaste rubriek','6:00'],['Nieuws regionaal','2:00'],['Nieuws landelijk','3:00']].map(([n,d])=>`
      <div style="display:flex;align-items:center;justify-content:space-between;padding:8px 0;border-bottom:1px solid rgba(255,255,255,.05)">
        <span style="font-size:13px">${n}</span>
        <span style="font-family:var(--font-mono);font-size:13px;color:var(--grijs)">${d}</span>
      </div>`).join('')}
      <div style="margin-top:16px;padding:12px;background:rgba(240,180,41,.08);border:1px solid rgba(240,180,41,.2);border-radius:8px">
        <div style="font-size:10px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--geel);margin-bottom:6px">Exporteer draaiboek als tekstbestand</div>
        <button class="btn btn-sm btn-ghost" onclick="exporteerDraaiboek()">📤 Download als .txt</button>
      </div>
    </div>
  </div>
</div>

<!-- ── MUZIEK MODAL ── -->
<div class="modal-bg" id="muz-modal">
  <div class="modal" style="max-width:480px;width:95%">
    <h3 id="mm-modal-title">Nummer toevoegen</h3>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:14px">
      <div class="field"><label class="field-label">Artiest *</label><input class="field-input" id="mm-artiest" placeholder="Naam artiest"></div>
      <div class="field"><label class="field-label">Titel *</label><input class="field-input" id="mm-titel" placeholder="Naam nummer"></div>
      <div class="field"><label class="field-label">Duur (mm:ss)</label><input class="field-input" id="mm-duur" placeholder="3:30"></div>
      <div class="field"><label class="field-label">Genre</label>
        <select class="field-select" id="mm-genre">
          <option value="">—</option><option>Pop</option><option>Rock</option><option>Folk</option>
          <option>Klassiek</option><option>Jazz</option><option>Wereldmuziek</option><option>Regionaal</option><option>Overig</option>
        </select>
      </div>
      <div class="field full"><label class="field-label">Notitie</label><input class="field-input" id="mm-notitie" placeholder="Bijzonderheden, sfeer, aanleiding…"></div>
      <div class="field full" style="display:flex;align-items:center;gap:8px">
        <input type="checkbox" id="mm-regionaal" style="width:16px;height:16px;accent-color:var(--groen)">
        <label for="mm-regionaal" style="font-size:13px;cursor:pointer">🌍 Regionaal artiest (uit West-Friesland)</label>
      </div>
    </div>
    <div class="modal-actions">
      <button class="btn btn-sm btn-danger" id="mm-delete-btn" style="margin-right:auto;display:none" onclick="deleteMuziek(muziekEditId)">🗑 Verwijderen</button>
      <button class="btn btn-ghost btn-sm" onclick="closeMuziekModal()">Annuleren</button>
      <button class="btn btn-sm btn-green" onclick="saveMuziek2()">💾 Opslaan</button>
    </div>
  </div>
</div>

<!-- ── RESET MODAL ── -->
<div class="modal-bg" id="reset-modal">
  <div class="modal">
    <h3>↺ Nieuwe week voorbereiden</h3>
    <p style="color:var(--grijs);font-size:13px;margin-bottom:14px">Wil je de huidige uitzending eerst opslaan in het archief?</p>
    <div style="background:rgba(240,180,41,.08);border:1px solid rgba(240,180,41,.25);border-radius:8px;padding:12px 14px;margin-bottom:14px">
      <label style="display:flex;align-items:center;gap:8px;font-size:13px;cursor:pointer;font-weight:500;color:var(--geel)">
        <input type="checkbox" id="reset-archive" checked> 💾 Huidige uitzending opslaan in archief
      </label>
      <p style="font-size:11px;color:var(--grijs);margin-top:6px;margin-left:24px">Je kunt het archief terugvinden via het tabblad "📂 Archief"</p>
    </div>
    <p style="color:var(--grijs);font-size:12px;margin-bottom:10px">Wat wil je daarna wissen voor de nieuwe week?</p>
    <label style="display:flex;align-items:center;gap:8px;font-size:13px;margin-bottom:8px;cursor:pointer">
      <input type="checkbox" id="reset-meta" checked> Datum, presentator, thema wissen
    </label>
    <label style="display:flex;align-items:center;gap:8px;font-size:13px;margin-bottom:8px;cursor:pointer">
      <input type="checkbox" id="reset-inhoud" checked> Ingevulde inhoud wissen (teksten, vragen, etc.)
    </label>
    <label style="display:flex;align-items:center;gap:8px;font-size:13px;margin-bottom:8px;cursor:pointer">
      <input type="checkbox" id="reset-status" checked> Status van blokken resetten naar 'Invullen'
    </label>
    <label style="display:flex;align-items:center;gap:8px;font-size:13px;margin-bottom:8px;cursor:pointer">
      <input type="checkbox" id="reset-check"> Checklist resetten
    </label>
    <label style="display:flex;align-items:center;gap:8px;font-size:13px;cursor:pointer">
      <input type="checkbox" id="reset-structuur" checked> Blokken terugzetten naar standaardvolgorde (verwijdert toegevoegde blokken)
    </label>
    <div class="modal-actions">
      <button class="btn btn-ghost btn-sm" onclick="closeModal()">Annuleren</button>
      <button class="btn btn-sm btn-orange" onclick="doReset()">Nieuwe week starten</button>
    </div>
  </div>
</div>

<!-- ── ARCHIEF BEKIJKEN MODAL ── -->
<div class="modal-bg" id="archive-view-modal">
  <div class="modal" style="max-width:700px;width:95%">
    <h3 id="archive-view-title">Archief bekijken</h3>
    <div id="archive-view-content" style="margin-top:12px;max-height:60vh;overflow-y:auto"></div>
    <div class="modal-actions">
      <button class="btn btn-sm btn-danger" id="archive-delete-btn">🗑 Verwijderen</button>
      <button class="btn btn-ghost btn-sm" onclick="closeArchiveView()">Sluiten</button>
    </div>
  </div>
</div>

<script>
// ══════════════════════════════════════════════════════════════════════
// DATA
// ══════════════════════════════════════════════════════════════════════
const BLOK_TEMPLATES = {
  O:{ label:'Opening',        icon:'🎬', bulletin:false, defaultDuur:'3:00',
      fields:[
        {id:'openingszin',label:'Openingszin (spreek letterlijk uit)',type:'textarea',hint:'De allereerste zin. Energiek en direct.',full:true},
        {id:'teaser1',label:'Teaser item 1',type:'input',hint:'Maak nieuwsgierig, verklap nog niets'},
        {id:'teaser2',label:'Teaser item 2',type:'input',hint:'Tweede aankondiging'},
        {id:'openingsmuziek',label:'Openingsmuziek (artiest – titel)',type:'input',hint:'Welk nummer volgt na de opening?'},
        {id:'tech',label:'Technische notitie',type:'input',hint:'Voor de technicus: jingle, niveau, fade, etc.',full:true},
      ]},
  A:{ label:'Afsluiting',     icon:'🎬', bulletin:false, defaultDuur:'5:00',
      fields:[
        {id:'terugblik',label:'Terugblik (1–2 zinnen)',type:'textarea',hint:'Hoogtepunten van de show',full:true},
        {id:'dankwoord',label:'Dankwoord gast(en)',type:'input',hint:'Naam + organisatie',full:true},
        {id:'volgendweek',label:'Volgende week (optioneel)',type:'input',hint:'Korte aankondiging',full:true},
        {id:'slotmuziek',label:'Slotmuziek (artiest – titel)',type:'input',hint:''},
        {id:'slotzin',label:'Allerlaatste zin (spreek letterlijk uit)',type:'textarea',hint:'Persoonlijk en herkenbaar',full:true},
      ]},
  T:{ label:'Item / Inleiding',icon:'📰', bulletin:false, defaultDuur:'4:00',
      fields:[
        {id:'onderwerp',label:'Onderwerp',type:'input',hint:'Eén zin, bondig en concreet',full:true},
        {id:'context',label:'Context (achtergrond)',type:'textarea',hint:'2–3 zinnen voor de luisteraar'},
        {id:'relevantie',label:'Relevantie voor West-Friesland',type:'textarea',hint:'Waarom belangrijk voor de regio?'},
        {id:'bron',label:'Bron',type:'input',hint:'Krant, persbericht, tip, etc.'},
        {id:'overgang',label:'Overgang naar volgend blok',type:'input',hint:'Hoe link je door?'},
        {id:'tech',label:'Technische notitie',type:'input',hint:'Voor de technicus'},
      ]},
  L:{ label:'Landelijk Nieuws',icon:'📡', bulletin:true, defaultDuur:'3:00',
      fields:[
        {id:'aankondiging',label:'Aankondigingszin (spreek letterlijk uit)',type:'textarea',hint:'Bijv. "We horen nu het nieuws van..."',full:true},
        {id:'bijz',label:'Bijzonderheden bulletin',type:'input',hint:'Opvallend item? Link naar volgend onderdeel?'},
      ]},
  R:{ label:'Regionaal Nieuws',icon:'📡', bulletin:true, defaultDuur:'2:00',
      fields:[
        {id:'aankondiging',label:'Aankondigingszin (spreek letterlijk uit)',type:'textarea',hint:'Bijv. "We horen nu het regionale nieuws van..."',full:true},
        {id:'bijz',label:'Bijzonderheden bulletin',type:'input',hint:'Regionaal item dat opvalt? Link naar volgend item?'},
      ]},
  I:{ label:'Interview',      icon:'🎙️', bulletin:false, defaultDuur:'8:00',
      fields:[
        {id:'gast',label:'Naam gast',type:'input',hint:'Volledige naam'},
        {id:'functie',label:'Functie / rol',type:'input',hint:'Bijv. wethouder gemeente Hoorn'},
        {id:'contact',label:'Contactgegevens (noodlijn)',type:'input',hint:'Tel. / e-mail voor noodgevallen'},
        {id:'aanleiding',label:'Aanleiding uitnodiging',type:'textarea',hint:'Koppeling aan actueel nieuws',full:true},
        {id:'invalshoek',label:'Centrale invalshoek / hoofdvraag',type:'textarea',hint:'De rode draad van het gesprek',full:true},
        {id:'intro',label:'Intro-tekst (spreek letterlijk uit)',type:'textarea',hint:'Hoe introduceer je de gast op de radio?',full:true},
        {id:'v1',label:'Vraag 1 — Opening',type:'textarea',hint:'Stel het onderwerp in, geef ruimte',full:true},
        {id:'v2',label:'Vraag 2 — Verdieping',type:'textarea',hint:'Ga in op een specifiek aspect',full:true},
        {id:'v3',label:'Vraag 3 — Kritisch / luisteraarsperspectief',type:'textarea',hint:'',full:true},
        {id:'v4',label:'Vraag 4 — Toekomst / afsluiting (opt.)',type:'textarea',hint:'',full:true},
        {id:'afsluiting',label:'Afsluiting gesprek',type:'textarea',hint:'Hoe bedank en sluit je af?',full:true},
        {id:'tech',label:'Technische notitie',type:'input',hint:'Telefonisch, skype, live in studio?',full:true},
      ]},
  M:{ label:'Muziek',         icon:'🎵', bulletin:false, defaultDuur:'3:30',
      fields:[
        {id:'artiest',label:'Artiest',type:'input',hint:'Naam artiest of band'},
        {id:'titel',label:'Titel',type:'input',hint:'Naam van het nummer'},
        {id:'reden',label:'Keuze / reden',type:'input',hint:'Regionaal artiest, thematisch, verzoek, etc.'},
        {id:'tech',label:'Technische notitie',type:'input',hint:'Inlaadnummer, kanaal, fade, etc.'},
        {id:'overgang',label:'Aankondiging na muziek',type:'input',hint:'Hoe leid je in na dit nummer?'},
      ]},
  V:{ label:'Vaste Rubriek',  icon:'📋', bulletin:false, defaultDuur:'6:00',
      fields:[
        {id:'rubriek',label:'Naam rubriek',type:'input',hint:'Bijv. "West-Fries Weekoverzicht"',full:true},
        {id:'intro',label:'Intro rubriek (spreek uit)',type:'textarea',hint:'Hoe introduceer je de rubriek?',full:true},
        {id:'item1kop',label:'Item 1 — Kop',type:'input',hint:'Pakkende kop'},
        {id:'item1toe',label:'Item 1 — Toelichting',type:'textarea',hint:'2–3 zinnen context'},
        {id:'item2kop',label:'Item 2 — Kop',type:'input',hint:'Ander thema'},
        {id:'item2toe',label:'Item 2 — Toelichting',type:'textarea',hint:'2–3 zinnen context'},
        {id:'item3kop',label:'Item 3 — Kop',type:'input',hint:'Mag luchtiger'},
        {id:'item3toe',label:'Item 3 — Toelichting',type:'textarea',hint:'2–3 zinnen context'},
        {id:'stelling',label:'Stelling (voor Kwestie-rubriek)',type:'input',hint:'Optioneel: als dit de Kwestie-rubriek is',full:true},
        {id:'oproep',label:'Oproep aan luisteraar',type:'textarea',hint:'Exacte tekst incl. reactiewijze',full:true},
        {id:'r1',label:'Reactie 1 (live)',type:'input',hint:'Naam/woonplaats + samenvatting',full:true},
        {id:'r2',label:'Reactie 2 (live)',type:'input',hint:'',full:true},
        {id:'r3',label:'Reactie 3 (live)',type:'input',hint:'',full:true},
        {id:'afsluiting',label:'Afsluiting rubriek',type:'textarea',hint:'Slotwoord + overgang',full:true},
      ]},
};

// Standaard programmastructuur
const DEFAULT_PROGRAMMA = [
  {type:'O',duur:'3:00'},{type:'T',duur:'4:00'},{type:'M',duur:'3:30'},
  {type:'V',duur:'6:00',data:{rubriek:'West-Fries Weekoverzicht'}},
  {type:'T',duur:'3:00'},{type:'M',duur:'4:00'},
  {type:'I',duur:'8:00'},
  {type:'R',duur:'2:00'},{type:'V',duur:'7:00',data:{rubriek:'De West-Friese Kwestie'}},
  {type:'M',duur:'4:00'},{type:'T',duur:'4:00'},{type:'M',duur:'3:30'},
  {type:'T',duur:'5:00'},{type:'M',duur:'3:00'},
  {type:'L',duur:'3:00'},
  {type:'I',duur:'8:00'},{type:'M',duur:'4:00'},
  {type:'T',duur:'5:00'},{type:'M',duur:'4:00'},{type:'T',duur:'6:00'},
  {type:'R',duur:'2:00'},{type:'M',duur:'4:00'},
  {type:'T',duur:'5:00'},{type:'M',duur:'4:00'},{type:'T',duur:'5:00'},
  {type:'M',duur:'3:00'},{type:'A',duur:'5:00'},
];

// ══════════════════════════════════════════════════════════════════════
// CONTACTEN DATABASE
// ══════════════════════════════════════════════════════════════════════
let contactenDB = [];
let contactEditId = null;
let catFilter = '';

function loadContacten() {
  try { contactenDB = JSON.parse(localStorage.getItem('wf_contacten_db') || '[]'); } catch(e){ contactenDB = []; }
}
function saveContacten() {
  localStorage.setItem('wf_contacten_db', JSON.stringify(contactenDB));
}

const CAT_COLORS = {
  'Politiek':'#8b1a1a','Maatschappij':'#1a3a5c','Cultuur':'#4a1a6a',
  'Sport':'#1a5e30','Onderwijs':'#7a3a10','Economie':'#1a4a3a',
  'Techniek':'#2d3548','Overig':'#3a3a4a','':`#2d3548`
};

function renderContacts() {
  loadContacten();
  renderContactsList();

  // notities
  ['redactie','ideeen','tech'].forEach(k => {
    const el = document.getElementById(`notes-${k}`);
    if (el) {
      el.value = localStorage.getItem(`wf_notes_${k}`) || '';
      el.removeEventListener('input', el._noteHandler);
      el._noteHandler = () => localStorage.setItem(`wf_notes_${k}`, el.value);
      el.addEventListener('input', el._noteHandler);
    }
  });
  setTimeout(initAutoResize, 10);
}

function renderContactsList() {
  const zoek = (document.getElementById('contact-zoek')?.value || '').toLowerCase();
  const lijst = document.getElementById('contacts-lijst');
  const geen  = document.getElementById('ct-geen');
  if (!lijst) return;

  let filtered = contactenDB.filter(c => {
    const matchCat = catFilter === '⭐' ? c.fav : (catFilter === '' || c.cat === catFilter);
    if (!matchCat) return false;
    if (!zoek) return true;
    return [c.naam,c.org,c.functie,c.onderwerp,c.cat,c.notitie,c.bereik]
      .some(v => (v||'').toLowerCase().includes(zoek));
  });

  // Sorteer: favorieten eerst, dan alfabetisch op naam
  filtered.sort((a,b) => (b.fav?1:0)-(a.fav?1:0) || (a.naam||'').localeCompare(b.naam||''));

  if (filtered.length === 0) {
    lijst.innerHTML = '';
    geen.style.display = 'block';
    return;
  }
  geen.style.display = 'none';

  lijst.innerHTML = filtered.map(c => {
    const bg = CAT_COLORS[c.cat] || CAT_COLORS[''];
    const initials = (c.naam||'?').split(' ').map(w=>w[0]).join('').toUpperCase().slice(0,2);
    return `<div style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;overflow:hidden;display:flex;flex-direction:column;transition:box-shadow .2s" onmouseover="this.style.boxShadow='0 3px 16px rgba(0,0,0,.35)'" onmouseout="this.style.boxShadow=''">
      <!-- kaart header -->
      <div style="background:${bg};padding:12px 14px;display:flex;align-items:center;gap:12px">
        <div style="width:40px;height:40px;border-radius:50%;background:rgba(255,255,255,.15);display:flex;align-items:center;justify-content:center;font-family:var(--font-display);font-weight:700;font-size:15px;color:#fff;flex-shrink:0">${initials}</div>
        <div style="flex:1;min-width:0">
          <div style="font-size:14px;font-weight:600;color:#fff;white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${c.fav?'⭐ ':''}${escape(c.naam||'(geen naam)')}</div>
          <div style="font-size:11px;color:rgba(255,255,255,.7);white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${escape([c.functie,c.org].filter(Boolean).join(' · '))}</div>
        </div>
        ${c.cat ? `<span style="background:rgba(0,0,0,.25);color:rgba(255,255,255,.8);font-size:9px;font-family:var(--font-mono);padding:2px 7px;border-radius:10px;white-space:nowrap">${escape(c.cat)}</span>` : ''}
      </div>
      <!-- kaart body -->
      <div style="padding:12px 14px;flex:1;display:flex;flex-direction:column;gap:6px">
        ${c.onderwerp ? `<div style="font-size:12px;color:#93c5fd;font-style:italic;margin-bottom:2px">📌 ${escape(c.onderwerp)}</div>` : ''}
        ${c.tel  ? `<div style="display:flex;align-items:center;gap:6px;font-size:12px"><span style="color:var(--grijs);width:14px">📱</span><a href="tel:${escape(c.tel)}" style="color:var(--wit);text-decoration:none">${escape(c.tel)}</a></div>` : ''}
        ${c.telv ? `<div style="display:flex;align-items:center;gap:6px;font-size:12px"><span style="color:var(--grijs);width:14px">☎️</span><a href="tel:${escape(c.telv)}" style="color:var(--wit);text-decoration:none">${escape(c.telv)}</a></div>` : ''}
        ${c.email? `<div style="display:flex;align-items:center;gap:6px;font-size:12px"><span style="color:var(--grijs);width:14px">✉️</span><a href="mailto:${escape(c.email)}" style="color:var(--wit);text-decoration:none">${escape(c.email)}</a></div>` : ''}
        ${c.bereik? `<div style="display:flex;align-items:center;gap:6px;font-size:11px;color:var(--grijs)"><span style="width:14px">🕐</span>${escape(c.bereik)}</div>` : ''}
        ${c.gast  ? `<div style="display:flex;align-items:center;gap:6px;font-size:11px;color:var(--grijs)"><span style="width:14px">🎙️</span>Eerder te gast: ${escape(c.gast)}</div>` : ''}
        ${c.notitie ? `<div style="font-size:11px;color:rgba(255,255,255,.5);margin-top:4px;line-height:1.4;border-top:1px solid var(--rand);padding-top:6px">${escape(c.notitie)}</div>` : ''}
      </div>
      <!-- kaart footer -->
      <div style="padding:8px 14px;border-top:1px solid var(--rand);display:flex;gap:6px">
        <button class="btn btn-sm btn-ghost" style="flex:1" onclick="editContact('${c.id}')">✏️ Bewerken</button>
        <button class="btn btn-sm btn-ghost" onclick="toggleFav('${c.id}')" title="${c.fav?'Favoriet verwijderen':'Favoriet'}" style="padding:5px 10px">${c.fav?'★':'☆'}</button>
      </div>
    </div>`;
  }).join('');
}

function filterContacts() { renderContactsList(); }

function setCatFilter(cat, btn) {
  catFilter = cat;
  document.querySelectorAll('.ct-filter-btn').forEach(b => b.classList.remove('active'));
  if (btn) btn.classList.add('active');
  renderContactsList();
}

function newContact() {
  contactEditId = null;
  document.getElementById('contact-modal-title').textContent = 'Nieuw contact';
  document.getElementById('cm-delete-btn').style.display = 'none';
  ['naam','org','functie','tel','telv','email','onderwerp','gast','bereik','notitie'].forEach(id => {
    const el = document.getElementById('cm-'+id);
    if (el) el.value = '';
  });
  document.getElementById('cm-cat').value = '';
  document.getElementById('cm-fav').checked = false;
  document.getElementById('contact-modal').classList.add('open');
  setTimeout(() => document.getElementById('cm-naam').focus(), 50);
}

function editContact(id) {
  const c = contactenDB.find(x => x.id === id);
  if (!c) return;
  contactEditId = id;
  document.getElementById('contact-modal-title').textContent = 'Contact bewerken';
  document.getElementById('cm-delete-btn').style.display = 'inline-flex';
  ['naam','org','functie','tel','telv','email','onderwerp','gast','bereik','notitie'].forEach(fld => {
    const el = document.getElementById('cm-'+fld);
    if (el) el.value = c[fld] || '';
  });
  document.getElementById('cm-cat').value = c.cat || '';
  document.getElementById('cm-fav').checked = !!c.fav;
  document.getElementById('contact-modal').classList.add('open');
}

function saveContact() {
  const naam = document.getElementById('cm-naam').value.trim();
  if (!naam) { document.getElementById('cm-naam').focus(); return; }

  const data = {
    id: contactEditId || ('ct_' + Date.now()),
    naam,
    org:      document.getElementById('cm-org').value.trim(),
    functie:  document.getElementById('cm-functie').value.trim(),
    tel:      document.getElementById('cm-tel').value.trim(),
    telv:     document.getElementById('cm-telv').value.trim(),
    email:    document.getElementById('cm-email').value.trim(),
    onderwerp:document.getElementById('cm-onderwerp').value.trim(),
    cat:      document.getElementById('cm-cat').value,
    gast:     document.getElementById('cm-gast').value.trim(),
    bereik:   document.getElementById('cm-bereik').value.trim(),
    notitie:  document.getElementById('cm-notitie').value.trim(),
    fav:      document.getElementById('cm-fav').checked,
  };

  if (contactEditId) {
    const idx = contactenDB.findIndex(x => x.id === contactEditId);
    if (idx >= 0) contactenDB[idx] = data;
  } else {
    contactenDB.push(data);
  }
  saveContacten();
  closeContactModal();
  renderContactsList();
}

function deleteContact() {
  if (!contactEditId || !confirm('Dit contact verwijderen?')) return;
  contactenDB = contactenDB.filter(x => x.id !== contactEditId);
  saveContacten();
  closeContactModal();
  renderContactsList();
}

function toggleFav(id) {
  const c = contactenDB.find(x => x.id === id);
  if (c) { c.fav = !c.fav; saveContacten(); renderContactsList(); }
}

function closeContactModal() {
  document.getElementById('contact-modal').classList.remove('open');
  contactEditId = null;
}

// ══════════════════════════════════════════════════════════════════════
// STATE
// ══════════════════════════════════════════════════════════════════════
let programma = [];
let onAir = false;
let clockInterval = null;
let dragSrcIdx = null;

function loadState() {
  const saved = localStorage.getItem('wf_programma');
  if (saved) {
    try { programma = JSON.parse(saved); return; } catch(e){}
  }
  programma = DEFAULT_PROGRAMMA.map((p,i) => ({
    id: 'blok_' + Date.now() + '_' + i,
    type: p.type,
    duur: p.duur,
    data: p.data || {},
    status: BLOK_TEMPLATES[p.type].bulletin ? 'bulletin' : 'invullen',
  }));
  saveState();
}

function saveState() {
  localStorage.setItem('wf_programma', JSON.stringify(programma));
}

function getBlok(id) { return programma.find(b => b.id === id); }
function getIdx(id)  { return programma.findIndex(b => b.id === id); }

// ══════════════════════════════════════════════════════════════════════
// HELPERS
// ══════════════════════════════════════════════════════════════════════
function parseDuur(str) {
  if (!str) return 0;
  const p = String(str).trim().split(':');
  if (p.length === 2) return (parseInt(p[0])||0) + (parseInt(p[1])||0)/60;
  return parseFloat(str)||0;
}
function formatTijd(minFromMidnight) {
  const t = Math.max(0, Math.round(minFromMidnight));
  return `${String(Math.floor(t/60)).padStart(2,'0')}:${String(t%60).padStart(2,'0')}`;
}
function formatDelta(min) {
  const a = Math.abs(min), m = Math.floor(a), s = Math.round((a-m)*60);
  return `${min>=0?'+':'–'}${m}:${String(s).padStart(2,'0')}`;
}
function formatCountdown(min) {
  const a = Math.abs(min), m = Math.floor(a), s = Math.round((a-m)*60);
  return `${m}:${String(s).padStart(2,'0')}`;
}
function nowMinutes() {
  if (onAir && onAirStartTime) {
    // Use anchored time: minutes since midnight based on when show "started"
    const anchor = new Date(onAirStartTime);
    const elapsedMin = (Date.now() - onAirStartTime) / 60000;
    const anchorMin = anchor.getHours() * 60 + anchor.getMinutes() + anchor.getSeconds() / 60;
    return anchorMin + elapsedMin;
  }
  const n = new Date();
  return n.getHours()*60 + n.getMinutes() + n.getSeconds()/60;
}
function escape(str) {
  return String(str||'').replace(/&/g,'&amp;').replace(/"/g,'&quot;').replace(/</g,'&lt;');
}

// ══════════════════════════════════════════════════════════════════════
// RENDER
// ══════════════════════════════════════════════════════════════════════
function render() {
  renderProgramma();
  updateStats();
  updateWarnings();
  renderContacts();
  loadMeta();
}

function renderProgramma() {
  const container = document.getElementById('programma');
  container.innerHTML = '';
  let minuten = 720; // 12:00

  programma.forEach((blok, idx) => {
    const tmpl = BLOK_TEMPLATES[blok.type];
    if (!tmpl) { minuten += parseDuur(blok.duur); return; }

    // separator
    if (blok.type === 'L' && idx > 0) {
      const sep = document.createElement('div');
      sep.className = 'hour-sep';
      sep.textContent = '13:00 — Landelijk nieuws (vast ankerpunt)';
      container.appendChild(sep);
    }

    const startMin = minuten;
    const duurMin = parseDuur(blok.duur);
    const endMin = startMin + duurMin;
    minuten = endMin;

    // live now?
    const now = nowMinutes();
    const isLive = onAir && now >= startMin && now < endMin;
    const remainMin = endMin - now;

    const div = document.createElement('div');
    div.className = `blok type-${blok.type}${isLive?' live-now':''}${blok.status==='vervalt'?' done-blok':''}`;
    div.id = `blok-el-${blok.id}`;
    div.dataset.id = blok.id;
    div.draggable = true;

    // detail velden
    let fieldsHTML = '<div class="detail-grid">';
    tmpl.fields.forEach(f => {
      const val = escape(blok.data[f.id] || '');
      const fullCls = f.full ? ' full' : '';
      if (f.type === 'textarea') {
        fieldsHTML += `<div class="field${fullCls}"><label class="field-label">${f.label}</label>
          <textarea class="field-textarea" data-blok="${blok.id}" data-field="${f.id}" placeholder="${escape(f.hint)}" rows="3">${val}</textarea></div>`;
      } else {
        fieldsHTML += `<div class="field${fullCls}"><label class="field-label">${f.label}</label>
          <input class="field-input" data-blok="${blok.id}" data-field="${f.id}" type="text" placeholder="${escape(f.hint)}" value="${val}"></div>`;
      }
    });
    // status
    const statOpts = ['invullen','bewerking','klaar','bulletin','vervalt'];
    const statLbls = {'invullen':'📝 Invullen','bewerking':'✏️ In bewerking','klaar':'✅ Klaar','bulletin':'📡 Bulletin (vast)','vervalt':'❌ Vervalt'};
    fieldsHTML += `<div class="field full" style="margin-top:6px"><label class="field-label">Status</label>
      <select class="field-select" data-blok="${blok.id}" data-field="__status">
        ${statOpts.map(s=>`<option value="${s}"${blok.status===s?' selected':''}>${statLbls[s]}</option>`).join('')}
      </select></div>`;
    fieldsHTML += '</div>';

    // detail actions
    fieldsHTML += `<div class="detail-actions">
      <button class="btn btn-sm btn-danger" onclick="removeBlok('${blok.id}')">🗑 Blok verwijderen</button>
      <button class="btn btn-sm btn-ghost" onclick="moveBlok('${blok.id}',-1)">↑ Omhoog</button>
      <button class="btn btn-sm btn-ghost" onclick="moveBlok('${blok.id}',1)">↓ Omlaag</button>
    </div>`;

    // preview tekst
    const previewField = tmpl.fields[0];
    const previewVal = blok.data[previewField?.id] || '';
    const muziekPreview = blok.type === 'M'
      ? [blok.data.artiest, blok.data.titel].filter(Boolean).join(' — ')
      : '';

    // countdown tekst
    let countdownHTML = '';
    if (onAir) {
      if (isLive) {
        const rem = remainMin;
        const urgentCls = rem < 1 ? 'urgent' : rem < 2 ? 'soon' : '';
        countdownHTML = `<div class="countdown-val ${urgentCls}">nog ${formatCountdown(rem)}</div>`;
      } else if (now < startMin) {
        countdownHTML = `<div class="countdown-val" style="color:var(--grijs);font-size:10px">over ${formatCountdown(startMin-now)}</div>`;
      }
    }

    div.innerHTML = `
      <div class="blok-header" onclick="toggleBlok('${blok.id}')">
        <div class="blok-nr">${idx+1}</div>
        <div class="blok-tijd">${formatTijd(startMin)}</div>
        <div class="blok-badge">${tmpl.icon} ${tmpl.label}</div>
        <div class="blok-inhoud">
          <div class="blok-titel">${muziekPreview || blok.data.onderwerp || blok.data.gast || blok.data.rubriek || blok.data.aankondiging || '<span style="color:var(--grijs);font-style:italic">Nog niet ingevuld</span>'}</div>
          <div class="blok-keynote">${previewVal && !muziekPreview ? escape(previewVal.substring(0,80)) : ''}</div>
        </div>
        <div class="live-indicator"><span>● NU LIVE</span></div>
        <div class="blok-duur-wrap" onclick="event.stopPropagation()">
          <input class="duur-input${tmpl.bulletin?' ':''}" id="duur-${blok.id}" value="${blok.duur}"
            ${tmpl.bulletin?'readonly':''} placeholder="0:00"
            onchange="onDuurChange('${blok.id}',this.value)"
            oninput="onDuurChange('${blok.id}',this.value)">
        </div>
        <div class="blok-countdown" onclick="event.stopPropagation()">${countdownHTML}</div>
        <div class="blok-toggle" onclick="event.stopPropagation();toggleBlok('${blok.id}')">▾</div>
      </div>
      <div class="blok-detail">${fieldsHTML}</div>`;

    // open state bewaren
    if (blok._open) div.classList.add('open');

    // drag events
    div.addEventListener('dragstart', e => { dragSrcIdx = idx; div.classList.add('dragging'); e.dataTransfer.effectAllowed='move'; });
    div.addEventListener('dragend', () => div.classList.remove('dragging'));
    div.addEventListener('dragover', e => { e.preventDefault(); div.classList.add('drag-over'); });
    div.addEventListener('dragleave', () => div.classList.remove('drag-over'));
    div.addEventListener('drop', e => {
      e.preventDefault(); div.classList.remove('drag-over');
      if (dragSrcIdx !== null && dragSrcIdx !== idx) {
        const moved = programma.splice(dragSrcIdx,1)[0];
        programma.splice(idx,0,moved);
        saveState(); render();
      }
    });

    container.appendChild(div);
  });

  // Bind field input events
  document.querySelectorAll('[data-blok][data-field]').forEach(el => {
    el.addEventListener('input', () => onFieldChange(el));
    el.addEventListener('change', () => onFieldChange(el));
  });
}

function toggleBlok(id) {
  const blok = getBlok(id);
  if (!blok) return;
  blok._open = !blok._open;
  const el = document.getElementById(`blok-el-${id}`);
  if (el) {
    el.classList.toggle('open', blok._open);
    if (blok._open) {
      // Wait for CSS transition to complete before measuring scrollHeight
      setTimeout(initAutoResize, 30);
      setTimeout(initAutoResize, 150);
    }
  }
}

function onFieldChange(el) {
  const id = el.dataset.blok;
  const field = el.dataset.field;
  const blok = getBlok(id);
  if (!blok) return;
  if (field === '__status') {
    blok.status = el.value;
  } else {
    blok.data[field] = el.value;
  }
  saveState();
  // Update preview in header without full re-render
  const elBlok = document.getElementById(`blok-el-${id}`);
  if (elBlok) {
    const tmpl = BLOK_TEMPLATES[blok.type];
    const titelEl = elBlok.querySelector('.blok-titel');
    const keynoteEl = elBlok.querySelector('.blok-keynote');
    if (titelEl) {
      const muziekPrev = blok.type === 'M'
        ? [blok.data.artiest, blok.data.titel].filter(Boolean).join(' — ')
        : '';
      titelEl.innerHTML = muziekPrev || blok.data.onderwerp || blok.data.gast || blok.data.rubriek || blok.data.aankondiging || '<span style="color:var(--grijs);font-style:italic">Nog niet ingevuld</span>';
      const previewField = tmpl.fields[0];
      const previewVal = blok.data[previewField?.id] || '';
      if (keynoteEl) keynoteEl.textContent = previewVal && !muziekPrev ? previewVal.substring(0,80) : '';
    }
  }
  updateStats();
  updateWarnings();
}

function onDuurChange(id, val) {
  const blok = getBlok(id);
  if (!blok) return;
  blok.duur = val;
  saveState();
  // Re-render tijden
  renderTijden();
  updateStats();
  updateWarnings();
}

function renderTijden() {
  let minuten = 720;
  programma.forEach(blok => {
    const el = document.getElementById(`blok-el-${blok.id}`);
    const tijdEl = el?.querySelector('.blok-tijd');
    if (tijdEl) tijdEl.textContent = formatTijd(minuten);
    minuten += parseDuur(blok.duur);
  });
}

// ══════════════════════════════════════════════════════════════════════
// STATS & WARNINGS
// ══════════════════════════════════════════════════════════════════════
function updateStats() {
  let total=0, muziek=0, klaar=0;
  programma.forEach(b => {
    const d = parseDuur(b.duur);
    total += d;
    if (b.type === 'M') muziek += d;
    if (b.status === 'klaar' || b.status === 'bulletin') klaar++;
  });
  const doel = 120, delta = total - doel;
  const inhoud = total - muziek;

  setText('stat-totaal', formatTijd(total*1+720).replace(':',':') + ' min', total);
  const deltaStr = formatDelta(delta);
  setText('stat-delta', deltaStr);
  const dc = document.getElementById('stat-balans-card');
  if (dc) dc.className = 'stat-card ' + (Math.abs(delta)<1?'good':delta>3?'danger':'warning');

  setText('stat-muziek', Math.round(muziek) + ' min');
  setText('stat-inhoud', Math.round(inhoud) + ' min');
  setText('stat-klaar', klaar + '/' + programma.length);

  // totaal in minuten
  document.getElementById('stat-totaal').textContent = Math.round(total) + ' min';
  document.getElementById('stat-delta').textContent = deltaStr;

  // balk
  const pct = Math.min(110, (total/doel)*100);
  const fill = document.getElementById('balans-fill');
  const val = document.getElementById('balans-val');
  if (fill) { fill.style.width = Math.min(100,pct)+'%'; fill.style.background = Math.abs(delta)<1?'#4ade80':delta>0?'#fbbf24':'#f87171'; }
  if (val) { val.textContent = deltaStr; val.style.color = Math.abs(delta)<1?'#4ade80':delta>0?'#fbbf24':'#f87171'; }

  // checklist progress
  ['dag','ochtend','tijdens','na'].forEach(s => updateChecklistProgress(s));
}

function setText(id, val) {
  const el = document.getElementById(id);
  if (el) el.textContent = val;
}

function updateWarnings() {
  const bar = document.getElementById('warnings-bar');
  if (!bar) return;
  const warns = [];

  let total = 0;
  programma.forEach(b => total += parseDuur(b.duur));
  const delta = total - 120;
  if (delta < -5) warns.push({msg:`⏱ Je bent ${formatCountdown(Math.abs(delta))} tekort — vul tijden aan of voeg blokken toe.`, soft:false});
  if (delta > 5)  warns.push({msg:`⏱ Je bent ${formatCountdown(delta)} te lang — schrap of verkort blokken.`, soft:false});
  if (Math.abs(delta) > 1 && Math.abs(delta) <= 5) warns.push({msg:`⚠️ Tijdbalans: ${formatDelta(delta)} — controleer je blokduren.`, soft:true});

  programma.forEach((b,i) => {
    const tmpl = BLOK_TEMPLATES[b.type];
    if (!tmpl || tmpl.bulletin || b.status === 'vervalt') return;
    const filled = Object.values(b.data).filter(v=>v&&v.trim()).length;
    if (filled === 0 && b.status !== 'klaar') {
      warns.push({msg:`📝 Blok ${i+1} (${tmpl.label}) is nog helemaal leeg.`, soft:true});
    }
  });

  bar.innerHTML = warns.slice(0,4).map(w =>
    `<div class="warning-item${w.soft?' soft':''}">${w.msg}</div>`
  ).join('');
}

// ══════════════════════════════════════════════════════════════════════
// LIVE KLOK & ON-AIR
// ══════════════════════════════════════════════════════════════════════
function startClock() {
  if (clockInterval) clearInterval(clockInterval);
  clockInterval = setInterval(() => {
    const n = new Date();
    const h = String(n.getHours()).padStart(2,'0');
    const m = String(n.getMinutes()).padStart(2,'0');
    const s = String(n.getSeconds()).padStart(2,'0');
    setText('live-clock', `${h}:${m}:${s}`);
    if (onAir) updateLiveBlokken();
  }, 1000);
}

function updateLiveBlokken() {
  const now = nowMinutes();
  let minuten = 720;
  programma.forEach(blok => {
    const duur = parseDuur(blok.duur);
    const start = minuten, end = minuten + duur;
    minuten = end;
    const el = document.getElementById(`blok-el-${blok.id}`);
    if (!el) return;
    const isLive = now >= start && now < end;
    el.classList.toggle('live-now', isLive);

    // countdown
    const cdEl = el.querySelector('.blok-countdown');
    if (cdEl) {
      if (isLive) {
        const rem = end - now;
        const cls = rem < 1 ? 'urgent' : rem < 2 ? 'soon' : '';
        cdEl.innerHTML = `<div class="countdown-val ${cls}">nog ${formatCountdown(rem)}</div>`;
        // auto-scroll naar live blok
        if (isLive && !el.dataset.scrolled) {
          el.scrollIntoView({behavior:'smooth', block:'center'});
          el.dataset.scrolled = '1';
        }
      } else if (now < start) {
        const diff = start - now;
        if (diff < 10) {
          cdEl.innerHTML = `<div class="countdown-val" style="font-size:10px;color:var(--grijs)">over ${formatCountdown(diff)}</div>`;
        } else cdEl.innerHTML = '';
      } else {
        cdEl.innerHTML = '';
        if (el.dataset.scrolled) delete el.dataset.scrolled;
      }
    }
  });
}

let onAirStartTime = null; // real Date.now() when uitzending started
let onAirTimerInterval = null;

function toggleOnAir() {
  onAir = !onAir;
  const btn = document.getElementById('onair-btn');
  const dot = document.getElementById('live-dot');
  const txt = document.getElementById('live-status-txt');
  const timerEl = document.getElementById('uitz-timer');

  if (onAir) {
    // ── Auto-sync: anchor to 12:00 today ─────────────────────────────
    // We track when the "virtual" show started relative to real time.
    // If the user clicks within 5 min of 12:00, we snap back to 12:00
    // so the block calculations stay in sync.
    const now = new Date();
    const noon = new Date(now);
    noon.setHours(12, 0, 0, 0);
    const diffMs = now - noon; // ms since noon (negative = before noon)
    // Snap if within ±5 minutes of noon
    if (Math.abs(diffMs) <= 5 * 60 * 1000) {
      onAirStartTime = noon.getTime(); // anchor to exactly 12:00
    } else {
      onAirStartTime = now.getTime();
    }

    btn.textContent = '⏹ Stop uitzending';
    btn.classList.remove('btn-ghost');
    btn.style.background = '#7f1d1d';
    dot.classList.add('on-air');
    txt.textContent = 'ON AIR';
    txt.style.color = '#f87171';
    if (timerEl) timerEl.style.display = 'flex';

    // Uitzending timer
    onAirTimerInterval = setInterval(() => {
      const elapsed = Date.now() - onAirStartTime;
      const totalSec = Math.floor(elapsed / 1000);
      const h = Math.floor(totalSec / 3600);
      const m = Math.floor((totalSec % 3600) / 60);
      const s = totalSec % 60;
      const display = h > 0
        ? `⏱ ${h}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`
        : `⏱ ${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
      if (timerEl) timerEl.textContent = display;
    }, 1000);

    // Reset scroll flags
    programma.forEach(b => {
      const el = document.getElementById(`blok-el-${b.id}`);
      if (el) delete el.dataset.scrolled;
    });
    updateLiveBlokken();
    setTimeout(scheduleWaarschuwingen, 500);
  } else {
    clearInterval(onAirTimerInterval);
    onAirTimerInterval = null;
    onAirStartTime = null;
    btn.textContent = '▶ Start uitzending';
    btn.classList.add('btn-ghost');
    btn.style.background = '';
    dot.classList.remove('on-air');
    txt.textContent = 'Niet op lucht';
    txt.style.color = 'var(--grijs)';
    if (timerEl) { timerEl.style.display = 'none'; timerEl.textContent = '⏱ 00:00'; }
    document.querySelectorAll('.blok.live-now').forEach(el => el.classList.remove('live-now'));
    document.querySelectorAll('.blok-countdown').forEach(el => el.innerHTML = '');
  }
}

// ══════════════════════════════════════════════════════════════════════
// BLOK BEHEER
// ══════════════════════════════════════════════════════════════════════
function addBlok(type) {
  const tmpl = BLOK_TEMPLATES[type];
  programma.push({
    id: 'blok_' + Date.now(),
    type,
    duur: tmpl.defaultDuur,
    data: {},
    status: tmpl.bulletin ? 'bulletin' : 'invullen',
    _open: true,
  });
  saveState(); render();
  // scroll naar laatste
  const last = document.querySelector('#programma .blok:last-child');
  if (last) last.scrollIntoView({behavior:'smooth', block:'center'});
}

function removeBlok(id) {
  if (!confirm('Dit blok verwijderen?')) return;
  programma = programma.filter(b => b.id !== id);
  saveState(); render();
}

function moveBlok(id, dir) {
  const idx = getIdx(id);
  const newIdx = idx + dir;
  if (newIdx < 0 || newIdx >= programma.length) return;
  const item = programma.splice(idx,1)[0];
  programma.splice(newIdx,0,item);
  saveState(); render();
}

// ══════════════════════════════════════════════════════════════════════
// META
// ══════════════════════════════════════════════════════════════════════
function loadMeta() {
  ['datum','presentator','redacteur','technicus','thema'].forEach(k => {
    const el = document.getElementById(`meta-${k}`);
    if (el) el.value = localStorage.getItem(`wf_meta_${k}`) || '';
  });
}
['datum','presentator','redacteur','technicus','thema'].forEach(k => {
  document.addEventListener('DOMContentLoaded', () => {});
  setTimeout(() => {
    const el = document.getElementById(`meta-${k}`);
    if (el) el.addEventListener('input', () => localStorage.setItem(`wf_meta_${k}`, el.value));
  }, 100);
});

// ══════════════════════════════════════════════════════════════════════
// CONTACTEN
// ══════════════════════════════════════════════════════════════════════
// ══════════════════════════════════════════════════════════════════════
// CHECKLIST
// ══════════════════════════════════════════════════════════════════════
function initChecklist() {
  document.querySelectorAll('.check-item input[type=checkbox]').forEach(cb => {
    cb.checked = localStorage.getItem(`wf_check_${cb.id}`) === '1';
    cb.addEventListener('change', () => {
      localStorage.setItem(`wf_check_${cb.id}`, cb.checked?'1':'0');
      updateChecklistProgress('dag');
      updateChecklistProgress('ochtend');
      updateChecklistProgress('tijdens');
      updateChecklistProgress('na');
    });
  });
  updateChecklistProgress('dag');
  updateChecklistProgress('ochtend');
  updateChecklistProgress('tijdens');
  updateChecklistProgress('na');
}

function updateChecklistProgress(section) {
  const secMap = {dag:'c',ochtend:'d',tijdens:'e',na:'f'};
  const prefix = secMap[section];
  if (!prefix) return;
  const all = document.querySelectorAll(`input[id^="${prefix}"]`);
  const done = [...all].filter(cb => cb.checked).length;
  const el = document.getElementById(`cp-${section}`);
  if (el) el.textContent = `${done} van ${all.length} afgevinkt`;
}

function resetChecklist() {
  if (!confirm('Checklist resetten?')) return;
  document.querySelectorAll('.check-item input[type=checkbox]').forEach(cb => {
    cb.checked = false;
    localStorage.removeItem(`wf_check_${cb.id}`);
  });
  ['dag','ochtend','tijdens','na'].forEach(s => updateChecklistProgress(s));
}

// ══════════════════════════════════════════════════════════════════════
// ARCHIEF
// ══════════════════════════════════════════════════════════════════════
function getArchief() {
  try { return JSON.parse(localStorage.getItem('wf_archief') || '[]'); } catch(e){ return []; }
}
function saveArchief(arr) {
  localStorage.setItem('wf_archief', JSON.stringify(arr));
}

function archiveHuidigeUitzending() {
  const datum   = localStorage.getItem('wf_meta_datum') || '(geen datum)';
  const pres    = localStorage.getItem('wf_meta_presentator') || '(onbekend)';
  const thema   = localStorage.getItem('wf_meta_thema') || '';
  const notes   = localStorage.getItem('wf_notes_redactie') || '';
  const ideeen  = localStorage.getItem('wf_notes_ideeen') || '';
  const techNotes = localStorage.getItem('wf_notes_tech') || '';

  const snapshot = {
    id: Date.now(),
    opgeslagenOp: new Date().toLocaleString('nl-NL'),
    datum, pres, thema, notes, ideeen, techNotes,
    programma: JSON.parse(JSON.stringify(programma)),
    meta: {
      datum, pres,
      redacteur: localStorage.getItem('wf_meta_redacteur') || '',
      technicus: localStorage.getItem('wf_meta_technicus') || '',
      thema,
    }
  };

  const archief = getArchief();
  archief.unshift(snapshot); // nieuwste bovenaan
  // Bewaar maximaal 20 uitzendingen
  if (archief.length > 20) archief.splice(20);
  saveArchief(archief);
}

function renderArchief() {
  const lijst = document.getElementById('archief-lijst');
  if (!lijst) return;
  const archief = getArchief();

  if (archief.length === 0) {
    lijst.innerHTML = `<div style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:32px;text-align:center;color:var(--grijs)">
      <div style="font-size:32px;margin-bottom:10px">📭</div>
      <div style="font-size:14px">Nog geen uitzendingen opgeslagen.</div>
      <div style="font-size:12px;margin-top:6px">Klik op "↺ Nieuwe week" en vink "Opslaan in archief" aan.</div>
    </div>`;
    return;
  }

  lijst.innerHTML = archief.map((item, idx) => {
    const totaalMin = item.programma.reduce((s,b) => s + parseDuur(b.duur), 0);
    const muziekMin = item.programma.filter(b=>b.type==='M').reduce((s,b) => s + parseDuur(b.duur), 0);
    const nrBlokken = item.programma.length;
    const typeCount = {};
    item.programma.forEach(b => { typeCount[b.type] = (typeCount[b.type]||0)+1; });

    return `<div style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;margin-bottom:10px;overflow:hidden">
      <div style="display:flex;align-items:center;justify-content:space-between;padding:14px 18px;cursor:pointer;gap:16px" onclick="toggleArchiveItem(${idx})">
        <div style="display:flex;align-items:center;gap:14px;flex:1;min-width:0">
          <div style="background:var(--rood);color:#fff;border-radius:6px;padding:6px 12px;font-family:var(--font-mono);font-size:13px;font-weight:600;white-space:nowrap">
            ${escape(item.datum)}
          </div>
          <div style="min-width:0">
            <div style="font-size:13px;font-weight:500;color:var(--wit)">${escape(item.pres)}${item.thema ? ' &mdash; <em style=\'color:var(--grijs);font-weight:300\'>' + escape(item.thema) + '</em>' : ''}</div>
            <div style="font-size:11px;color:var(--grijs);margin-top:2px;font-family:var(--font-mono)">Opgeslagen op ${escape(item.opgeslagenOp)}</div>
          </div>
        </div>
        <div style="display:flex;gap:14px;align-items:center;flex-shrink:0">
          <div style="text-align:center"><div style="font-size:16px;font-weight:600;font-family:var(--font-mono)">${Math.round(totaalMin)}</div><div style="font-size:9px;color:var(--grijs);font-family:var(--font-mono);text-transform:uppercase">min</div></div>
          <div style="text-align:center"><div style="font-size:16px;font-weight:600;font-family:var(--font-mono);color:#86efac">${Math.round(muziekMin)}</div><div style="font-size:9px;color:var(--grijs);font-family:var(--font-mono);text-transform:uppercase">muziek</div></div>
          <div style="text-align:center"><div style="font-size:16px;font-weight:600;font-family:var(--font-mono)">${nrBlokken}</div><div style="font-size:9px;color:var(--grijs);font-family:var(--font-mono);text-transform:uppercase">blokken</div></div>
          <button class="btn btn-sm" onclick="event.stopPropagation();openArchiveView(${idx})">👁 Bekijken</button>
          <button class="btn btn-sm btn-danger" onclick="event.stopPropagation();deleteArchiveItem(${idx})">🗑</button>
        </div>
      </div>
      <div id="archive-item-${idx}" style="display:none;border-top:1px solid var(--rand);padding:14px 18px">
        <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:10px;margin-bottom:14px">
          ${['datum','redacteur','technicus','thema'].map(k => `
            <div><div style="font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:3px">${k}</div>
            <div style="font-size:13px;color:var(--wit)">${escape(item.meta[k] || '—')}</div></div>`).join('')}
        </div>
        ${item.notes ? `<div style="margin-bottom:10px"><div style="font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:4px">Evaluatie</div><div style="font-size:12px;color:rgba(255,255,255,.75);line-height:1.5;background:var(--donker3);border-radius:6px;padding:10px">${escape(item.notes)}</div></div>` : ''}
        ${item.ideeen ? `<div style="margin-bottom:10px"><div style="font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:4px">Ideeën voor volgende keer</div><div style="font-size:12px;color:rgba(255,255,255,.75);line-height:1.5;background:var(--donker3);border-radius:6px;padding:10px">${escape(item.ideeen)}</div></div>` : ''}
        <div style="font-size:11px;color:var(--grijs);font-family:var(--font-mono)">${item.programma.map((b,i) => {
          const tmpl = BLOK_TEMPLATES[b.type];
          return `<span style="margin-right:8px">${i+1}. ${tmpl?.icon||''} ${b.data?.onderwerp||b.data?.artiest||b.data?.gast||b.data?.rubriek||tmpl?.label||b.type} <span style="opacity:.5">(${b.duur})</span></span>`;
        }).join('')}</div>
      </div>
    </div>`;
  }).join('');
}

function toggleArchiveItem(idx) {
  const el = document.getElementById(`archive-item-${idx}`);
  if (el) el.style.display = el.style.display === 'none' ? 'block' : 'none';
}

let currentArchiveIdx = null;
function openArchiveView(idx) {
  currentArchiveIdx = idx;
  const archief = getArchief();
  const item = archief[idx];
  if (!item) return;

  document.getElementById('archive-view-title').textContent = `📂 ${item.datum} — ${item.pres}`;

  const content = document.getElementById('archive-view-content');
  content.innerHTML = item.programma.map((b, i) => {
    const tmpl = BLOK_TEMPLATES[b.type];
    if (!tmpl) return '';
    const typeColors = {O:'#4a4a5a',A:'#4a4a5a',T:'#2d3548',L:'#8b1a1a',R:'#1a3a5c',I:'#4a1a6a',M:'#1a5e30',V:'#7a3a10'};
    const bg = typeColors[b.type] || '#2d3548';
    const fields = tmpl.fields.filter(f => b.data[f.id]).map(f =>
      `<div style="margin-bottom:8px"><div style="font-size:9px;font-family:monospace;text-transform:uppercase;letter-spacing:.8px;color:#888;margin-bottom:2px">${f.label}</div>
       <div style="font-size:12px;color:#ddd;white-space:pre-wrap">${escape(b.data[f.id])}</div></div>`
    ).join('');

    return `<div style="border-radius:7px;margin-bottom:8px;overflow:hidden;border:1px solid rgba(255,255,255,.08)">
      <div style="background:${bg};padding:8px 14px;display:flex;align-items:center;gap:12px">
        <span style="font-family:monospace;font-size:12px;color:rgba(255,255,255,.6)">${i+1}</span>
        <span style="font-family:monospace;font-weight:700;color:#fff;font-size:13px">—</span>
        <span style="font-size:10px;font-family:monospace;text-transform:uppercase;letter-spacing:.7px;color:rgba(255,255,255,.8)">${tmpl.icon} ${tmpl.label}</span>
        <span style="font-family:monospace;font-size:11px;color:rgba(255,255,255,.5);margin-left:auto">${b.duur}</span>
      </div>
      ${fields ? `<div style="background:#1c2230;padding:10px 14px">${fields}</div>` : ''}
    </div>`;
  }).join('');

  document.getElementById('archive-delete-btn').onclick = () => { deleteArchiveItem(idx); closeArchiveView(); };
  document.getElementById('archive-view-modal').classList.add('open');
}

function closeArchiveView() {
  document.getElementById('archive-view-modal').classList.remove('open');
}

function deleteArchiveItem(idx) {
  if (!confirm('Deze uitzending uit het archief verwijderen?')) return;
  const archief = getArchief();
  archief.splice(idx, 1);
  saveArchief(archief);
  renderArchief();
}

function clearAllArchive() {
  if (!confirm('Weet je zeker dat je het volledige archief wilt wissen? Dit kan niet ongedaan worden gemaakt.')) return;
  localStorage.removeItem('wf_archief');
  renderArchief();
}

// ══════════════════════════════════════════════════════════════════════
// RESET NIEUWE WEEK
// ══════════════════════════════════════════════════════════════════════
function showResetModal() {
  document.getElementById('reset-modal').classList.add('open');
}
function closeModal() {
  document.getElementById('reset-modal').classList.remove('open');
}
function doReset() {
  const archive  = document.getElementById('reset-archive').checked;
  const meta     = document.getElementById('reset-meta').checked;
  const inhoud   = document.getElementById('reset-inhoud').checked;
  const status   = document.getElementById('reset-status').checked;
  const check    = document.getElementById('reset-check').checked;
  const structuur = document.getElementById('reset-structuur').checked;

  // Eerst archiveren vóór wissen
  if (archive) archiveHuidigeUitzending();

  if (meta) {
    ['datum','presentator','redacteur','technicus','thema'].forEach(k => {
      localStorage.removeItem(`wf_meta_${k}`);
      const el = document.getElementById(`meta-${k}`);
      if (el) el.value = '';
    });
    ['redactie','ideeen','tech'].forEach(k => {
      localStorage.removeItem(`wf_notes_${k}`);
      const el = document.getElementById(`notes-${k}`);
      if (el) el.value = '';
    });
  }
  if (inhoud) {
    programma.forEach(b => { b.data = {}; b._open = false; });
  }
  if (status) {
    programma.forEach(b => {
      b.status = BLOK_TEMPLATES[b.type]?.bulletin ? 'bulletin' : 'invullen';
    });
  }
  if (check) {
    document.querySelectorAll('.check-item input[type=checkbox]').forEach(cb => {
      cb.checked = false;
      localStorage.removeItem(`wf_check_${cb.id}`);
    });
  }
  if (structuur) {
    programma = DEFAULT_PROGRAMMA.map((p, i) => ({
      id: 'blok_' + Date.now() + '_' + i,
      type: p.type,
      duur: p.duur,
      data: p.data || {},
      status: BLOK_TEMPLATES[p.type].bulletin ? 'bulletin' : 'invullen',
    }));
  }

  saveState();
  closeModal();
  render();

  if (archive) {
    setTimeout(() => {
      const tabs = document.querySelectorAll('.nav-tab');
      tabs.forEach(t => t.classList.remove('active'));
      // kleine toast
      const toast = document.createElement('div');
      toast.style.cssText = 'position:fixed;bottom:24px;right:24px;background:#1a5e30;color:#fff;padding:12px 20px;border-radius:8px;font-size:13px;font-family:sans-serif;z-index:999;box-shadow:0 4px 20px rgba(0,0,0,.4)';
      toast.textContent = '✅ Uitzending opgeslagen in archief';
      document.body.appendChild(toast);
      setTimeout(() => toast.remove(), 3500);
    }, 100);
  }
}

// ══════════════════════════════════════════════════════════════════════
// NAVIGATIE
// ══════════════════════════════════════════════════════════════════════
function showPage(id, tab) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  document.getElementById('page-'+id).classList.add('active');
  if (tab) tab.classList.add('active');
  if (id === 'contacts')     { renderContacts(); setTimeout(initAutoResize,50); }
  if (id === 'archief')      renderArchief();
  if (id === 'muziek')       renderMuziek();
  if (id === 'sjablonen')    renderSjablonen();
  if (id === 'weekplan')     renderWeekplan();
  if (id === 'statistieken') renderStatistieken();
}

// ══════════════════════════════════════════════════════════════════════
// SCRATCHPAD
// ══════════════════════════════════════════════════════════════════════
function toggleScratchpad() {
  const sp = document.getElementById('scratchpad');
  const vis = sp.style.display !== 'none';
  sp.style.display = vis ? 'none' : 'block';
  if (!vis) {
    const ta = document.getElementById('scratchpad-text');
    ta.value = localStorage.getItem('wf_scratchpad') || '';
    ta.focus();
    ta.addEventListener('input', () => localStorage.setItem('wf_scratchpad', ta.value));
  }
}

// ══════════════════════════════════════════════════════════════════════
// STOPWATCH (segmenttimer)
// ══════════════════════════════════════════════════════════════════════
let swRunning = false, swStart = 0, swElapsed = 0, swInterval = null;
function swToggle() {
  if (!swRunning) {
    swStart = Date.now() - swElapsed;
    swInterval = setInterval(() => {
      swElapsed = Date.now() - swStart;
      const s = Math.floor(swElapsed/1000), m = Math.floor(s/60);
      document.getElementById('sw-display').textContent =
        `${String(m).padStart(2,'0')}:${String(s%60).padStart(2,'0')}`;
    }, 200);
    swRunning = true;
    document.getElementById('sw-btn').textContent = '⏸ Pauzeer';
    document.getElementById('sw-btn').style.background = '#7f1d1d';
  } else {
    clearInterval(swInterval); swRunning = false;
    document.getElementById('sw-btn').textContent = '▶ Start';
    document.getElementById('sw-btn').style.background = '';
  }
}
function swReset() {
  clearInterval(swInterval); swRunning = false; swElapsed = 0;
  document.getElementById('sw-display').textContent = '00:00';
  document.getElementById('sw-btn').textContent = '▶ Start';
  document.getElementById('sw-btn').style.background = '';
}
function swLap() {
  const d = document.getElementById('sw-display').textContent;
  const list = document.getElementById('sw-laps');
  const li = document.createElement('div');
  li.style.cssText = 'font-family:var(--font-mono);font-size:12px;padding:4px 0;border-bottom:1px solid var(--rand);color:var(--grijs)';
  li.textContent = `Lap ${list.children.length+1}: ${d}`;
  list.prepend(li);
}

// ══════════════════════════════════════════════════════════════════════
// MUZIEKBIBLIOTHEEK
// ══════════════════════════════════════════════════════════════════════
let muziekDB = [];
let muziekEditId = null;
function loadMuziek() { try { muziekDB = JSON.parse(localStorage.getItem('wf_muziek_db')||'[]'); } catch(e){ muziekDB=[]; } }
function saveMuziek() { localStorage.setItem('wf_muziek_db', JSON.stringify(muziekDB)); }

function renderMuziek() {
  loadMuziek();
  const zoek = (document.getElementById('muz-zoek')?.value||'').toLowerCase();
  const genre = document.getElementById('muz-genre-filter')?.value||'';
  let list = muziekDB.filter(m => {
    if (genre && m.genre !== genre) return false;
    if (!zoek) return true;
    return [m.artiest,m.titel,m.genre,m.notitie].some(v=>(v||'').toLowerCase().includes(zoek));
  }).sort((a,b)=>(a.artiest||'').localeCompare(b.artiest||''));

  const container = document.getElementById('muz-lijst');
  if (!container) return;

  if (list.length === 0) {
    container.innerHTML = '<div style="text-align:center;padding:32px;color:var(--grijs)"><div style="font-size:32px;margin-bottom:10px">🎵</div><div>Nog geen nummers in de bibliotheek. Voeg er een toe!</div></div>';
    return;
  }

  // Stats
  const totDuur = muziekDB.reduce((s,m)=>s+parseDuur(m.duur||'0:00'),0);
  document.getElementById('muz-stat-totaal').textContent = muziekDB.length + ' nummers';
  document.getElementById('muz-stat-duur').textContent = Math.round(totDuur) + ' min totaal';
  const regio = muziekDB.filter(m=>m.regionaal).length;
  document.getElementById('muz-stat-regio').textContent = regio + ' regionaal';

  container.innerHTML = `<table style="width:100%;border-collapse:collapse">
    <thead><tr style="background:var(--donker3)">
      ${['Artiest','Titel','Duur','Genre','Regionaal','Gespeeld','Notitie',''].map(h=>
        `<th style="padding:8px 12px;text-align:left;font-size:9px;font-family:var(--font-mono);text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);white-space:nowrap">${h}</th>`
      ).join('')}
    </tr></thead>
    <tbody>${list.map((m,i)=>`
    <tr style="border-bottom:1px solid var(--rand);${i%2?'background:rgba(255,255,255,.02)':''}">
      <td style="padding:8px 12px;font-size:13px;font-weight:500">${escape(m.artiest||'')}</td>
      <td style="padding:8px 12px;font-size:13px;color:rgba(255,255,255,.8)">${escape(m.titel||'')}</td>
      <td style="padding:8px 12px;font-family:var(--font-mono);font-size:12px;color:var(--grijs)">${escape(m.duur||'')}</td>
      <td style="padding:8px 12px;font-size:11px">${m.genre?`<span style="background:var(--donker3);padding:2px 7px;border-radius:10px;font-family:var(--font-mono);font-size:9px">${escape(m.genre)}</span>`:''}</td>
      <td style="padding:8px 12px;text-align:center">${m.regionaal?'🌍':''}</td>
      <td style="padding:8px 12px;font-family:var(--font-mono);font-size:11px;color:var(--grijs)">${m.gespeeld||0}×</td>
      <td style="padding:8px 12px;font-size:11px;color:var(--grijs);max-width:160px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${escape(m.notitie||'')}</td>
      <td style="padding:8px 12px;white-space:nowrap">
        <button class="btn btn-sm btn-ghost" style="font-size:10px;padding:3px 8px;margin-right:4px" onclick="editMuziek('${m.id}')">✏️</button>
        <button class="btn btn-sm btn-ghost" style="font-size:10px;padding:3px 8px" onclick="deleteMuziek('${m.id}')">🗑</button>
      </td>
    </tr>`).join('')}
    </tbody></table>`;
}

function editMuziek(id) {
  const m = muziekDB.find(x=>x.id===id);
  if (!m) return;
  muziekEditId = id;
  document.getElementById('mm-artiest').value = m.artiest||'';
  document.getElementById('mm-titel').value = m.titel||'';
  document.getElementById('mm-duur').value = m.duur||'';
  document.getElementById('mm-genre').value = m.genre||'';
  document.getElementById('mm-regionaal').checked = !!m.regionaal;
  document.getElementById('mm-notitie').value = m.notitie||'';
  document.getElementById('mm-modal-title').textContent = 'Nummer bewerken';
  document.getElementById('mm-delete-btn').style.display = 'inline-flex';
  document.getElementById('muz-modal').classList.add('open');
}

function newMuziek() {
  muziekEditId = null;
  ['artiest','titel','duur','notitie'].forEach(f=>{const el=document.getElementById('mm-'+f);if(el)el.value='';});
  document.getElementById('mm-genre').value='';
  document.getElementById('mm-regionaal').checked=false;
  document.getElementById('mm-modal-title').textContent='Nummer toevoegen';
  document.getElementById('mm-delete-btn').style.display='none';
  document.getElementById('muz-modal').classList.add('open');
  setTimeout(()=>document.getElementById('mm-artiest').focus(),50);
}

function saveMuziek2() {
  const artiest = document.getElementById('mm-artiest').value.trim();
  if (!artiest) { document.getElementById('mm-artiest').focus(); return; }
  const data = {
    id: muziekEditId||('muz_'+Date.now()),
    artiest, titel:document.getElementById('mm-titel').value.trim(),
    duur:document.getElementById('mm-duur').value.trim(),
    genre:document.getElementById('mm-genre').value,
    regionaal:document.getElementById('mm-regionaal').checked,
    notitie:document.getElementById('mm-notitie').value.trim(),
    gespeeld: muziekEditId ? (muziekDB.find(x=>x.id===muziekEditId)?.gespeeld||0) : 0,
  };
  if (muziekEditId) { const i=muziekDB.findIndex(x=>x.id===muziekEditId); if(i>=0) muziekDB[i]=data; }
  else muziekDB.push(data);
  saveMuziek(); closeMuziekModal(); renderMuziek();
}
function deleteMuziek(id) {
  if (!confirm('Dit nummer verwijderen?')) return;
  muziekDB = muziekDB.filter(x=>x.id!==id);
  saveMuziek(); closeMuziekModal(); renderMuziek();
}
function closeMuziekModal() { document.getElementById('muz-modal').classList.remove('open'); muziekEditId=null; }

// ══════════════════════════════════════════════════════════════════════
// STATISTIEKEN
// ══════════════════════════════════════════════════════════════════════
function renderStatistieken() {
  const archief = getArchief();
  const container = document.getElementById('stats-content');
  if (!container) return;

  if (archief.length < 2) {
    container.innerHTML = '<div style="text-align:center;padding:32px;color:var(--grijs)"><div style="font-size:32px;margin-bottom:10px">📊</div><div>Sla minstens 2 uitzendingen op in het archief om statistieken te zien.</div></div>';
    return;
  }

  const totaalMin = archief.map(a=>a.programma.reduce((s,b)=>s+parseDuur(b.duur),0));
  const muziekMin = archief.map(a=>a.programma.filter(b=>b.type==='M').reduce((s,b)=>s+parseDuur(b.duur),0));
  const gem = arr => arr.reduce((a,b)=>a+b,0)/arr.length;
  const overshoot = totaalMin.map(t=>t-120);

  // Gast frequentie
  const gastFreq = {};
  archief.forEach(a=>a.programma.filter(b=>b.type==='I').forEach(b=>{
    const g=b.data?.gast; if(g&&g.trim()&&!g.startsWith('[')) gastFreq[g]=(gastFreq[g]||0)+1;
  }));
  const topGasten = Object.entries(gastFreq).sort((a,b)=>b[1]-a[1]).slice(0,5);

  container.innerHTML = `
  <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));gap:10px;margin-bottom:20px">
    ${[
      ['Uitzendingen gearchiveerd', archief.length, ''],
      ['Gem. programma duur', Math.round(gem(totaalMin))+' min', ''],
      ['Gem. muziek per show', Math.round(gem(muziekMin))+' min', ''],
      ['Gem. afwijking van 120 min', (gem(overshoot)>=0?'+':'')+Math.round(gem(overshoot))+' min', gem(overshoot)>3?'danger':gem(overshoot)<-3?'warning':'good'],
    ].map(([l,v,c])=>`<div class="stat-card ${c||'neutral'}"><div class="stat-card-label">${l}</div><div class="stat-card-val">${v}</div></div>`).join('')}
  </div>

  <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
    <div style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:16px">
      <h3 style="font-family:var(--font-mono);font-size:10px;text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:12px">⏱ Tijdbalans per uitzending</h3>
      ${archief.slice(0,10).reverse().map(a=>{
        const t=a.programma.reduce((s,b)=>s+parseDuur(b.duur),0);
        const d=t-120; const pct=Math.min(100,Math.abs(d)/10*100);
        return `<div style="margin-bottom:8px">
          <div style="display:flex;justify-content:space-between;font-size:11px;margin-bottom:3px">
            <span style="color:var(--grijs);font-family:var(--font-mono)">${escape(a.datum)}</span>
            <span style="font-family:var(--font-mono);color:${d>3?'#f87171':d<-3?'#fbbf24':'#4ade80'}">${d>=0?'+':''}${Math.round(d)} min</span>
          </div>
          <div style="height:5px;background:var(--donker3);border-radius:3px">
            <div style="height:100%;width:${Math.min(100,50+d/10*50)}%;background:${d>3?'#f87171':d<-3?'#fbbf24':'#4ade80'};border-radius:3px;transition:width .3s"></div>
          </div>
        </div>`;
      }).join('')}
    </div>
    <div style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:16px">
      <h3 style="font-family:var(--font-mono);font-size:10px;text-transform:uppercase;letter-spacing:.8px;color:var(--grijs);margin-bottom:12px">🎙️ Meest uitgenodigde gasten</h3>
      ${topGasten.length ? topGasten.map(([g,n],i)=>`
        <div style="display:flex;align-items:center;justify-content:space-between;padding:8px 0;border-bottom:1px solid rgba(255,255,255,.04)">
          <div style="font-size:13px">${i+1}. ${escape(g)}</div>
          <div style="font-family:var(--font-mono);font-size:12px;color:var(--grijs)">${n}× te gast</div>
        </div>`).join('')
      : '<div style="color:var(--grijs);font-size:13px">Nog geen interviewdata beschikbaar.</div>'}
    </div>
  </div>`;
}

// ══════════════════════════════════════════════════════════════════════
// WEEKPLANNING
// ══════════════════════════════════════════════════════════════════════
let weekplanItems = [];
function loadWeekplan() { try { weekplanItems = JSON.parse(localStorage.getItem('wf_weekplan')||'[]'); } catch(e){ weekplanItems=[]; } }
function saveWeekplan() { localStorage.setItem('wf_weekplan', JSON.stringify(weekplanItems)); }

function renderWeekplan() {
  loadWeekplan();
  const container = document.getElementById('weekplan-content');
  if (!container) return;

  // Generate next 8 Saturdays
  const saturdays = [];
  const today = new Date();
  const d = new Date(today);
  d.setDate(d.getDate() + ((6 - d.getDay() + 7) % 7 || 7));
  for (let i = 0; i < 8; i++) {
    saturdays.push(new Date(d));
    d.setDate(d.getDate() + 7);
  }

  container.innerHTML = saturdays.map(sat => {
    const key = sat.toISOString().slice(0,10);
    const items = weekplanItems.filter(x=>x.datum===key);
    const label = sat.toLocaleDateString('nl-NL',{weekday:'long',day:'numeric',month:'long'});
    return `<div style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;margin-bottom:10px;overflow:hidden">
      <div style="background:var(--donker3);padding:10px 16px;display:flex;align-items:center;justify-content:space-between">
        <div style="font-family:var(--font-display);font-size:14px;font-weight:600">${label}</div>
        <button class="btn btn-sm btn-ghost" onclick="addWeekplanItem('${key}')">+ Toevoegen</button>
      </div>
      <div id="wp-${key}" style="padding:${items.length?'10px 16px':'0'}">
        ${items.map(item=>`
          <div style="display:flex;align-items:center;gap:10px;padding:6px 0;border-bottom:1px solid rgba(255,255,255,.04)">
            <span style="font-size:10px;font-family:var(--font-mono);background:var(--donker3);padding:2px 7px;border-radius:10px;color:${
              item.type==='gast'?'#c084fc':item.type==='onderwerp'?'#93c5fd':'#86efac'
            };white-space:nowrap">${item.type==='gast'?'🎙️ Gast':item.type==='onderwerp'?'📰 Onderwerp':'📋 Rubriek'}</span>
            <span style="font-size:13px;flex:1">${escape(item.tekst)}</span>
            <button onclick="deleteWeekplanItem('${item.id}')" style="background:none;border:none;color:var(--grijs);cursor:pointer;font-size:14px;padding:0 4px">×</button>
          </div>`).join('')}
        ${items.length===0?'<div style="padding:10px 0;color:var(--grijs);font-size:12px;font-style:italic">Nog niets ingepland voor deze datum.</div>':''}
      </div>
    </div>`;
  }).join('');
}

function addWeekplanItem(datum) {
  const tekst = prompt('Wat wil je inplannen? (gast, onderwerp of rubriek)');
  if (!tekst) return;
  const type = tekst.toLowerCase().includes('rubriek') ? 'rubriek'
             : prompt('Type: gast / onderwerp / rubriek?', 'onderwerp') || 'onderwerp';
  weekplanItems.push({ id:'wp_'+Date.now(), datum, tekst, type });
  saveWeekplan(); renderWeekplan();
}
function deleteWeekplanItem(id) {
  weekplanItems = weekplanItems.filter(x=>x.id!==id);
  saveWeekplan(); renderWeekplan();
}

// ══════════════════════════════════════════════════════════════════════
// SJABLONEN
// ══════════════════════════════════════════════════════════════════════
const SJABLOON_DEFAULTS = [
  { id:'sj_weekoverzicht', naam:'West-Fries Weekoverzicht', type:'V', icon:'📋',
    beschrijving:'Terugblik op 3 regionale nieuwsfeiten. Luchtig maar informatief. Vaste duur: 6 min.',
    data:{rubriek:'West-Fries Weekoverzicht'}, duur:'6:00' },
  { id:'sj_kwestie', naam:'De West-Friese Kwestie', type:'V', icon:'📋',
    beschrijving:'Stellingendebat met luisteraars via app/sms. Live reacties verwerken. Duur: 7 min.',
    data:{rubriek:'De West-Friese Kwestie'}, duur:'7:00' },
  { id:'sj_interview_kort', naam:'Kort interview (5 min)', type:'I', icon:'🎙️',
    beschrijving:'Snel gesprek — 3 vragen, helder en bondig.', data:{}, duur:'5:00' },
  { id:'sj_interview_lang', naam:'Lang interview (12 min)', type:'I', icon:'🎙️',
    beschrijving:'Diepte-interview met 5+ vragen en ruimte voor uitweiding.', data:{}, duur:'12:00' },
];

let sjablonenDB = [];
function loadSjablonen() {
  try { sjablonenDB = JSON.parse(localStorage.getItem('wf_sjablonen')||'null') || SJABLOON_DEFAULTS; }
  catch(e){ sjablonenDB = SJABLOON_DEFAULTS; }
}
function saveSjablonen() { localStorage.setItem('wf_sjablonen', JSON.stringify(sjablonenDB)); }

function renderSjablonen() {
  loadSjablonen();
  const c = document.getElementById('sj-lijst');
  if (!c) return;
  c.innerHTML = sjablonenDB.map(sj=>`
    <div style="background:var(--donker2);border:1px solid var(--rand);border-radius:10px;padding:16px;display:flex;flex-direction:column;gap:10px">
      <div style="display:flex;align-items:center;gap:10px">
        <span style="font-size:22px">${sj.icon||'📋'}</span>
        <div>
          <div style="font-size:14px;font-weight:600">${escape(sj.naam)}</div>
          <div style="font-size:11px;color:var(--grijs);margin-top:2px">${escape(sj.beschrijving||'')}</div>
        </div>
        <div style="margin-left:auto;font-family:var(--font-mono);font-size:12px;color:var(--grijs)">${sj.duur}</div>
      </div>
      <div style="display:flex;gap:6px">
        <button class="btn btn-sm btn-green" style="flex:1" onclick="sjaabloonNaarDraaiboek('${sj.id}')">+ Zet in draaiboek</button>
        <button class="btn btn-sm btn-ghost" onclick="deleteSjabloon('${sj.id}')">🗑</button>
      </div>
    </div>`).join('');
}

function sjaabloonNaarDraaiboek(id) {
  const sj = sjablonenDB.find(x=>x.id===id);
  if (!sj) return;
  programma.push({
    id:'blok_'+Date.now(), type:sj.type, duur:sj.duur,
    data:JSON.parse(JSON.stringify(sj.data||{})),
    status:'invullen', _open:true,
  });
  saveState();
  showPage('draaiboek', document.querySelector('.nav-tab'));
  render();
  setTimeout(()=>document.querySelector('#programma .blok:last-child')?.scrollIntoView({behavior:'smooth',block:'center'}),100);
}

function deleteSjabloon(id) {
  if (!confirm('Sjabloon verwijderen?')) return;
  sjablonenDB = sjablonenDB.filter(x=>x.id!==id);
  saveSjablonen(); renderSjablonen();
}

function nieuwSjabloon() {
  const naam = prompt('Naam van het sjabloon?');
  if (!naam) return;
  const types = {O:'Opening',A:'Afsluiting',T:'Item',L:'Landelijk nieuws',R:'Regionaal nieuws',I:'Interview',M:'Muziek',V:'Vaste rubriek'};
  const typeStr = prompt('Type (O/A/T/L/R/I/M/V):\n'+Object.entries(types).map(([k,v])=>`${k} = ${v}`).join('\n'), 'V');
  const type = typeStr?.trim().toUpperCase();
  if (!types[type]) return;
  const duur = prompt('Standaard duur (mm:ss)?', '5:00') || '5:00';
  const beschr = prompt('Korte beschrijving?') || '';
  sjablonenDB.push({ id:'sj_'+Date.now(), naam, type, icon:BLOK_TEMPLATES[type]?.icon||'📋', beschrijving:beschr, data:{}, duur });
  saveSjablonen(); renderSjablonen();
}

// ══════════════════════════════════════════════════════════════════════
// EXPORTEREN (download als tekst)
// ══════════════════════════════════════════════════════════════════════
function exporteerDraaiboek() {
  const datum = localStorage.getItem('wf_meta_datum')||'(geen datum)';
  const pres  = localStorage.getItem('wf_meta_presentator')||'';
  const red   = localStorage.getItem('wf_meta_redacteur')||'';
  const tech  = localStorage.getItem('wf_meta_technicus')||'';
  const thema = localStorage.getItem('wf_meta_thema')||'';

  let txt = `DRAAIBOEK — WESTFRIESLAND RADIO\n`;
  txt += `${'='.repeat(50)}\n`;
  txt += `Datum:       ${datum}\nPresentator: ${pres}\nEindredacteur: ${red}\nTechnicus:   ${tech}\nThema:       ${thema}\n`;
  txt += `${'='.repeat(50)}\n\n`;

  let minuten = 720;
  programma.forEach((b,i) => {
    const tmpl = BLOK_TEMPLATES[b.type];
    if (!tmpl) return;
    const startStr = formatTijd(minuten);
    minuten += parseDuur(b.duur);
    txt += `[${startStr}] ${tmpl.icon} ${tmpl.label.toUpperCase()} (${b.duur})\n`;
    txt += `${'-'.repeat(40)}\n`;
    tmpl.fields.forEach(f => {
      if (b.data[f.id]) txt += `${f.label}:\n${b.data[f.id]}\n\n`;
    });
    txt += '\n';
  });

  txt += `\nNOTITIES REDACTIE:\n${localStorage.getItem('wf_notes_redactie')||''}\n`;
  txt += `\nIDEEÈN:\n${localStorage.getItem('wf_notes_ideeen')||''}\n`;
  txt += `\nTECHNISCHE NOTITIES:\n${localStorage.getItem('wf_notes_tech')||''}\n`;

  const blob = new Blob([txt], {type:'text/plain;charset=utf-8'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `draaiboek_${datum.replace(/[^0-9a-z]/gi,'_')}.txt`;
  a.click();
}

// tijdwaarschuwingen (browser notification)
let warnTimers = [];
function scheduleWaarschuwingen() {
  warnTimers.forEach(clearTimeout);
  warnTimers = [];
  if (!onAir) return;
  const now = nowMinutes();
  let minuten = 720;
  programma.forEach(b => {
    const start = minuten;
    minuten += parseDuur(b.duur);
    const tmpl = BLOK_TEMPLATES[b.type];
    if (!tmpl) return;
    // Warn 2 minutes before each block
    const warnAt = start - 2;
    if (warnAt > now) {
      const msDelay = (warnAt - now) * 60000;
      const t = setTimeout(() => {
        showToast(`⏰ Over 2 minuten: ${tmpl.icon} ${tmpl.label} om ${formatTijd(start)}`, 6000, '#1A5276');
      }, msDelay);
      warnTimers.push(t);
    }
  });
}

function showToast(msg, duration=3500, bg='#1a5e30') {
  const toast = document.createElement('div');
  toast.style.cssText = `position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:${bg};color:#fff;padding:12px 20px;border-radius:8px;font-size:13px;font-family:sans-serif;z-index:9999;box-shadow:0 4px 20px rgba(0,0,0,.5);white-space:nowrap;max-width:90vw;text-align:center`;
  toast.textContent = msg;
  document.body.appendChild(toast);
  setTimeout(() => toast.style.opacity='0', duration-400);
  setTimeout(() => toast.remove(), duration);
}
// ══════════════════════════════════════════════════════════════════════
function autoResize(el) {
  if (!el) return;
  // Must temporarily set to auto to allow shrink
  el.style.height = '0px';
  const sh = el.scrollHeight;
  el.style.height = Math.max(sh, 60) + 'px';
}
function initAutoResize() {
  document.querySelectorAll('.field-textarea,.notes-textarea').forEach(el => {
    el.style.overflow = 'hidden';
    el.style.resize = 'none';
    el.style.boxSizing = 'border-box';
    // Always re-run, don't skip already-bound elements
    autoResize(el);
    if (!el._arBound) {
      el._arBound = () => autoResize(el);
      el.addEventListener('input', el._arBound);
    }
  });
}

// ══════════════════════════════════════════════════════════════════════
// BOOT
// ══════════════════════════════════════════════════════════════════════
loadState();
loadContacten();
loadMuziek();
loadSjablonen();
loadWeekplan();
render();
initChecklist();
startClock();
// run once after first render, then after every render via MutationObserver
initAutoResize();
const _arObserver = new MutationObserver(() => initAutoResize());
_arObserver.observe(document.getElementById('programma'), { childList: true, subtree: false });
</script>
</body>
</html>
