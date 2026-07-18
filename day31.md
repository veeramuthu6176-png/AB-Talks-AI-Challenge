<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Supply Chain Control Tower</title>
<style>
/* ============================================================
   DESIGN TOKENS
   ============================================================ */
:root{
  --bg-0:#050810;
  --bg-1:#0a1122;
  --bg-2:#0e1830;
  --bg-3:#122040;
  --border:#1c2e4f;
  --border-bright:#2a4370;
  --cyan:#2fd7e6;
  --cyan-dim:#1a7f8c;
  --blue:#4d7bf3;
  --red:#ff4d5e;
  --red-dim:#5c2029;
  --orange:#ffa53e;
  --orange-dim:#5c3a15;
  --green:#33e6a8;
  --green-dim:#164533;
  --text:#dfe8fa;
  --text-dim:#8394b8;
  --text-faint:#4d5c80;
  --font-display: 'Segoe UI', -apple-system, system-ui, sans-serif;
  --font-mono: ui-monospace, 'SF Mono', 'Cascadia Mono', Consolas, monospace;
}

*{ box-sizing:border-box; margin:0; padding:0; }

html,body{
  background:var(--bg-0);
  color:var(--text);
  font-family:var(--font-display);
  height:100%;
  overflow-x:hidden;
}

/* subtle CRT scanline sweep across the whole console */
body::before{
  content:"";
  position:fixed; inset:0;
  pointer-events:none;
  background:linear-gradient(to bottom, transparent 0%, rgba(47,215,230,0.035) 50%, transparent 100%);
  background-size:100% 6px;
  z-index:9999;
  opacity:.5;
}

button{ font-family:inherit; cursor:pointer; }
::-webkit-scrollbar{ width:8px; height:8px; }
::-webkit-scrollbar-thumb{ background:var(--border-bright); border-radius:4px; }
::-webkit-scrollbar-track{ background:transparent; }

/* ============================================================
   LAYOUT SHELL
   ============================================================ */
.app{
  max-width:1400px;
  margin:0 auto;
  padding:18px 20px 40px;
  min-height:100vh;
  display:flex;
  flex-direction:column;
  gap:16px;
}

/* ============================================================
   HEADER
   ============================================================ */
.header{
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:16px;
  flex-wrap:wrap;
  border-bottom:1px solid var(--border);
  padding-bottom:14px;
}
.header-title{ display:flex; align-items:center; gap:14px; }

.radar{
  width:44px; height:44px;
  border-radius:50%;
  position:relative;
  border:1px solid var(--border-bright);
  background:radial-gradient(circle at center, rgba(47,215,230,.08), transparent 70%);
  flex-shrink:0;
}
.radar::before{
  content:"";
  position:absolute; inset:0;
  border-radius:50%;
  background:conic-gradient(from 0deg, rgba(47,215,230,.9), transparent 35%);
  animation:sweep 2.6s linear infinite;
  -webkit-mask-image:radial-gradient(circle, black 96%, transparent 100%);
  mask-image:radial-gradient(circle, black 96%, transparent 100%);
}
.radar::after{
  content:"";
  position:absolute; inset:16px;
  border-radius:50%;
  background:var(--cyan);
  box-shadow:0 0 8px 2px var(--cyan);
}
@keyframes sweep{ to{ transform:rotate(360deg); } }

.title-block h1{
  font-size:19px;
  letter-spacing:2.5px;
  text-transform:uppercase;
  font-weight:700;
  color:var(--text);
}
.title-block p{
  font-size:11.5px;
  letter-spacing:1.5px;
  text-transform:uppercase;
  color:var(--cyan);
  margin-top:2px;
}

.header-controls{ display:flex; align-items:center; gap:10px; flex-wrap:wrap; }

.icon-btn{
  width:38px; height:38px;
  border-radius:8px;
  border:1px solid var(--border-bright);
  background:var(--bg-2);
  color:var(--text-dim);
  font-size:16px;
  display:flex; align-items:center; justify-content:center;
  transition:all .15s ease;
}
.icon-btn:hover{ border-color:var(--cyan); color:var(--cyan); background:var(--bg-3); }
.icon-btn.active{ color:var(--green); border-color:var(--green); }

/* ============================================================
   TIMER / SCORE STRIP
   ============================================================ */
.flight-strip{
  display:flex; gap:14px; flex-wrap:wrap;
}
.flight-card{
  flex:1 1 160px;
  background:linear-gradient(160deg, var(--bg-2), var(--bg-1));
  border:1px solid var(--border-bright);
  border-radius:10px;
  padding:14px 18px;
  position:relative;
  overflow:hidden;
}
.flight-card .label{
  font-size:10.5px; letter-spacing:1.8px; text-transform:uppercase;
  color:var(--text-dim); margin-bottom:6px;
}
.flight-card .value{
  font-family:var(--font-mono);
  font-size:30px; font-weight:600; color:var(--cyan);
  letter-spacing:1px;
}
.flight-card.timer .value{ color:var(--orange); }
.flight-card.timer.low-time .value{ color:var(--red); animation:blink 1s infinite; }
@keyframes blink{ 50%{ opacity:.35; } }
.flight-card.score .value{ color:var(--green); }

/* ============================================================
   KPI GRID
   ============================================================ */
.kpi-grid{
  display:grid;
  grid-template-columns:repeat(6, 1fr);
  gap:12px;
}
.kpi-card{
  background:var(--bg-1);
  border:1px solid var(--border);
  border-radius:10px;
  padding:12px 14px;
  transition:box-shadow .3s ease, border-color .3s ease;
}
.kpi-card .label{
  font-size:10px; letter-spacing:1.4px; text-transform:uppercase;
  color:var(--text-dim); margin-bottom:8px;
}
.kpi-card .value{
  font-family:var(--font-mono);
  font-size:22px; font-weight:600;
}
.kpi-bar{
  margin-top:8px; height:4px; border-radius:2px;
  background:var(--bg-3); overflow:hidden;
}
.kpi-bar > div{ height:100%; transition:width .4s ease, background .4s ease; }

.kpi-card.pulse-good{ box-shadow:0 0 0 1px var(--green), 0 0 14px -2px var(--green); }
.kpi-card.pulse-bad{ box-shadow:0 0 0 1px var(--red), 0 0 14px -2px var(--red); }

/* ============================================================
   MAIN GRID: ALERTS + LOG
   ============================================================ */
.main-grid{
  display:grid;
  grid-template-columns:1.7fr 1fr;
  gap:16px;
  flex:1;
  min-height:420px;
}

.panel{
  background:var(--bg-1);
  border:1px solid var(--border);
  border-radius:12px;
  display:flex; flex-direction:column;
  overflow:hidden;
}
.panel-head{
  padding:12px 16px;
  border-bottom:1px solid var(--border);
  display:flex; align-items:center; justify-content:space-between;
}
.panel-head h2{
  font-size:12px; letter-spacing:1.8px; text-transform:uppercase; color:var(--text-dim);
}
.panel-head .count{
  font-family:var(--font-mono); font-size:12px; color:var(--cyan);
  background:var(--bg-2); border:1px solid var(--border-bright);
  padding:2px 8px; border-radius:999px;
}

.alerts-panel .panel-body{
  flex:1; overflow-y:auto; padding:14px; display:flex; flex-direction:column; gap:12px;
}
.empty-state{
  margin:auto; text-align:center; color:var(--text-faint); font-size:13px; padding:30px;
}

/* Alert Card */
.alert-card{
  background:var(--bg-2);
  border:1px solid var(--border-bright);
  border-left:4px solid var(--blue);
  border-radius:8px;
  padding:14px 16px;
  animation:slideIn .3s ease;
  position:relative;
}
@keyframes slideIn{ from{ opacity:0; transform:translateY(-8px);} to{ opacity:1; transform:translateY(0);} }

.alert-card.priority-high{ border-left-color:var(--red); }
.alert-card.priority-medium{ border-left-color:var(--orange); }
.alert-card.priority-low{ border-left-color:var(--cyan); }

.alert-card.priority-high .alert-head .icon{ animation:pulseIcon 1.1s infinite; }
@keyframes pulseIcon{ 0%,100%{ filter:drop-shadow(0 0 0 var(--red)); } 50%{ filter:drop-shadow(0 0 6px var(--red)); } }

.alert-head{ display:flex; align-items:flex-start; gap:10px; margin-bottom:6px; }
.alert-head .icon{ font-size:22px; line-height:1; }
.alert-head .titles{ flex:1; }
.alert-head .titles h3{ font-size:14.5px; font-weight:700; }
.alert-head .titles .impact{ font-size:11px; color:var(--text-dim); margin-top:2px; }

.priority-tag{
  font-size:9.5px; letter-spacing:1px; text-transform:uppercase;
  padding:3px 7px; border-radius:5px; font-weight:700; white-space:nowrap;
}
.priority-tag.high{ background:var(--red-dim); color:var(--red); }
.priority-tag.medium{ background:var(--orange-dim); color:var(--orange); }
.priority-tag.low{ background:rgba(47,215,230,.12); color:var(--cyan); }

.alert-desc{ font-size:12.5px; color:var(--text-dim); line-height:1.5; margin:8px 0 10px; }
.delayed-tag{ font-size:10px; color:var(--orange); letter-spacing:1px; text-transform:uppercase; margin-bottom:6px; display:inline-block; }

.countdown-bar{ height:4px; border-radius:2px; background:var(--bg-3); overflow:hidden; margin-bottom:12px; }
.countdown-bar > div{ height:100%; background:var(--cyan); transition:width .1s linear, background .3s ease; }

.alert-actions{ display:flex; flex-wrap:wrap; gap:8px; }
.action-btn{
  background:var(--bg-3);
  border:1px solid var(--border-bright);
  color:var(--text);
  font-size:11.5px;
  padding:7px 11px;
  border-radius:6px;
  transition:all .15s ease;
}
.action-btn:hover{ border-color:var(--cyan); color:var(--cyan); transform:translateY(-1px); }
.action-btn.ignore{ color:var(--text-faint); }
.action-btn.ignore:hover{ color:var(--red); border-color:var(--red); }
.action-btn.delay{ color:var(--orange); }
.action-btn.delay:hover{ border-color:var(--orange); }

/* Event log */
.log-panel .panel-body{
  flex:1; overflow-y:auto; padding:10px 14px; display:flex; flex-direction:column-reverse; gap:8px;
}
.log-entry{
  font-size:12px; line-height:1.5;
  border-left:2px solid var(--border-bright);
  padding:5px 10px;
  color:var(--text-dim);
  animation:slideIn .25s ease;
}
.log-entry .t{ font-family:var(--font-mono); color:var(--text-faint); font-size:10px; margin-right:6px; }
.log-entry.good{ border-left-color:var(--green); color:var(--text); }
.log-entry.ok{ border-left-color:var(--blue); color:var(--text); }
.log-entry.bad{ border-left-color:var(--red); color:var(--text); }
.log-entry.info{ border-left-color:var(--text-faint); }

/* ============================================================
   MODALS
   ============================================================ */
.modal-overlay{
  position:fixed; inset:0; background:rgba(3,6,12,.75);
  backdrop-filter:blur(3px);
  display:none; align-items:center; justify-content:center; z-index:1000; padding:20px;
}
.modal-overlay.open{ display:flex; }
.modal{
  background:var(--bg-1); border:1px solid var(--border-bright); border-radius:14px;
  max-width:520px; width:100%; padding:26px 28px; max-height:86vh; overflow-y:auto;
}
.modal h2{ font-size:16px; letter-spacing:1.5px; text-transform:uppercase; color:var(--cyan); margin-bottom:14px; }
.modal h3{ font-size:12.5px; color:var(--text); margin:14px 0 6px; }
.modal p, .modal li{ font-size:13px; color:var(--text-dim); line-height:1.6; }
.modal ul{ padding-left:18px; }
.modal .close-row{ margin-top:20px; display:flex; justify-content:flex-end; }

.btn{
  background:var(--cyan); color:#00232a; font-weight:700; border:none;
  padding:10px 20px; border-radius:8px; font-size:13px; letter-spacing:.5px;
  transition:transform .15s ease, box-shadow .15s ease;
}
.btn:hover{ transform:translateY(-1px); box-shadow:0 4px 16px -4px var(--cyan); }
.btn.secondary{ background:transparent; border:1px solid var(--border-bright); color:var(--text); }

/* Pause overlay */
.pause-overlay{
  position:fixed; inset:0; background:rgba(3,6,12,.85);
  display:none; align-items:center; justify-content:center; flex-direction:column; gap:16px; z-index:900;
}
.pause-overlay.open{ display:flex; }
.pause-overlay h2{ font-size:26px; letter-spacing:4px; color:var(--cyan); text-transform:uppercase; }

/* Game over */
.gameover-modal{ max-width:640px; }
.grade-badge{
  font-family:var(--font-mono); font-size:52px; font-weight:700;
  text-align:center; margin:6px 0 4px;
}
.gameover-sub{ text-align:center; color:var(--text-dim); font-size:13px; margin-bottom:18px; }
.stat-grid{
  display:grid; grid-template-columns:repeat(3,1fr); gap:10px; margin:14px 0;
}
.stat-box{
  background:var(--bg-2); border:1px solid var(--border); border-radius:8px;
  padding:10px 12px; text-align:center;
}
.stat-box .l{ font-size:9.5px; letter-spacing:1px; text-transform:uppercase; color:var(--text-faint); }
.stat-box .v{ font-family:var(--font-mono); font-size:18px; font-weight:700; color:var(--text); margin-top:4px; }
.summary-box{
  background:var(--bg-2); border-left:3px solid var(--cyan); border-radius:6px;
  padding:12px 14px; font-size:12.5px; color:var(--text-dim); line-height:1.6; margin-top:14px;
}

/* ============================================================
   RESPONSIVE
   ============================================================ */
@media (max-width:960px){
  .kpi-grid{ grid-template-columns:repeat(3, 1fr); }
  .main-grid{ grid-template-columns:1fr; }
  .stat-grid{ grid-template-columns:repeat(2,1fr); }
}
@media (max-width:520px){
  .kpi-grid{ grid-template-columns:repeat(2, 1fr); }
  .flight-strip{ flex-direction:column; }
  .header{ flex-direction:column; align-items:flex-start; }
}
</style>
</head>
<body>

<div class="app">

  <!-- HEADER -->
  <div class="header">
    <div class="header-title">
      <div class="radar"></div>
      <div class="title-block">
        <h1>AI Supply Chain Control Tower</h1>
        <p>Head of Operations Console</p>
      </div>
    </div>
    <div class="header-controls">
      <button class="icon-btn active" id="soundBtn" title="Sound (visual only)">🔊</button>
      <button class="icon-btn" id="pauseBtn" title="Pause">⏸</button>
      <button class="icon-btn" id="helpBtn" title="Instructions">❓</button>
    </div>
  </div>

  <!-- TIMER / SCORE -->
  <div class="flight-strip">
    <div class="flight-card timer" id="timerCard">
      <div class="label">Time Remaining</div>
      <div class="value" id="timeValue">03:00</div>
    </div>
    <div class="flight-card score">
      <div class="label">Score</div>
      <div class="value" id="scoreValue">0</div>
    </div>
    <div class="flight-card">
      <div class="label">Revenue Protected</div>
      <div class="value" id="revenueValue" style="color:var(--green); font-size:26px;">$0</div>
    </div>
  </div>

  <!-- KPI GRID -->
  <div class="kpi-grid" id="kpiGrid">
    <div class="kpi-card" data-kpi="serviceLevel">
      <div class="label">Service Level</div>
      <div class="value">100%</div>
      <div class="kpi-bar"><div style="width:100%; background:var(--green);"></div></div>
    </div>
    <div class="kpi-card" data-kpi="satisfaction">
      <div class="label">Customer Satisfaction</div>
      <div class="value">100%</div>
      <div class="kpi-bar"><div style="width:100%; background:var(--green);"></div></div>
    </div>
    <div class="kpi-card" data-kpi="inventoryHealth">
      <div class="label">Inventory Health</div>
      <div class="value">100%</div>
      <div class="kpi-bar"><div style="width:100%; background:var(--green);"></div></div>
    </div>
    <div class="kpi-card" data-kpi="transportEfficiency">
      <div class="label">Transport Efficiency</div>
      <div class="value">100%</div>
      <div class="kpi-bar"><div style="width:100%; background:var(--green);"></div></div>
    </div>
    <div class="kpi-card" data-kpi="cost">
      <div class="label">Operating Cost Index</div>
      <div class="value">100</div>
      <div class="kpi-bar"><div style="width:50%; background:var(--cyan);"></div></div>
    </div>
    <div class="kpi-card" data-kpi="composite">
      <div class="label">Network Health</div>
      <div class="value">100%</div>
      <div class="kpi-bar"><div style="width:100%; background:var(--green);"></div></div>
    </div>
  </div>

  <!-- MAIN GRID -->
  <div class="main-grid">
    <div class="panel alerts-panel">
      <div class="panel-head">
        <h2>Active Alerts</h2>
        <span class="count" id="alertCount">0</span>
      </div>
      <div class="panel-body" id="alertsBody">
        <div class="empty-state" id="emptyState">Console idle. Press Start to open the control tower feed.</div>
      </div>
    </div>
    <div class="panel log-panel">
      <div class="panel-head">
        <h2>Operations Log</h2>
        <span class="count" id="logCount">0</span>
      </div>
      <div class="panel-body" id="logBody"></div>
    </div>
  </div>

</div>

<!-- START OVERLAY -->
<div class="pause-overlay open" id="startOverlay">
  <h2>Control Tower Offline</h2>
  <p style="color:var(--text-dim); max-width:420px; text-align:center; font-size:13px;">
    You are the new Head of Operations. Alerts will stream in from ports, factories, warehouses and
    fleets worldwide. Resolve each one before its clock runs out and keep the network's KPIs healthy.
  </p>
  <button class="btn" id="startBtn">Start 3-Minute Shift</button>
  <button class="btn secondary" id="startHelpBtn">How to play</button>
</div>

<!-- PAUSE OVERLAY -->
<div class="pause-overlay" id="pauseOverlay">
  <h2>Paused</h2>
  <button class="btn" id="resumeBtn">Resume Shift</button>
</div>

<!-- HELP MODAL -->
<div class="modal-overlay" id="helpModal">
  <div class="modal">
    <h2>Console Briefing</h2>
    <h3>Objective</h3>
    <p>Keep the global supply chain running for 3 minutes. Alerts will appear on the left. Choose the
      best action before the countdown bar empties.</p>
    <h3>Actions</h3>
    <ul>
      <li><strong>Contextual actions</strong> (e.g. Reroute Trucks, Expedite Shipment) — pick the one
        that best matches the alert. The strongest match gives the biggest KPI boost.</li>
      <li><strong>Ignore</strong> — dismisses the alert immediately with a penalty.</li>
      <li><strong>Delay Decision</strong> — buys extra time but weakens the eventual outcome and adds a
        small cost penalty. Can only be used once per alert.</li>
    </ul>
    <h3>KPIs</h3>
    <p>Service Level, Customer Satisfaction, Inventory Health and Transport Efficiency drop when issues
      go unresolved or are mishandled. Operating Cost Index rises with bad decisions. Revenue Protected
      and Score climb with good ones.</p>
    <h3>Difficulty</h3>
    <p>Alerts arrive faster and stack up as the clock runs down — triage matters more late in the shift.</p>
    <div class="close-row"><button class="btn" id="closeHelpBtn">Got it</button></div>
  </div>
</div>

<!-- GAME OVER MODAL -->
<div class="modal-overlay" id="gameOverModal">
  <div class="modal gameover-modal">
    <h2>Shift Debrief</h2>
    <div class="grade-badge" id="gradeBadge">A</div>
    <div class="gameover-sub" id="finalScoreLine">Final Score: 0</div>
    <div class="stat-grid" id="finalKpiGrid"></div>
    <div class="stat-grid">
      <div class="stat-box"><div class="l">Alerts Resolved</div><div class="v" id="statResolved">0</div></div>
      <div class="stat-box"><div class="l">Correct Decisions</div><div class="v" id="statCorrect" style="color:var(--green);">0</div></div>
      <div class="stat-box"><div class="l">Wrong Decisions</div><div class="v" id="statWrong" style="color:var(--red);">0</div></div>
    </div>
    <div class="summary-box" id="summaryText"></div>
    <div class="close-row">
      <button class="btn secondary" id="closeGameOverBtn" style="margin-right:8px;">Close</button>
      <button class="btn" id="playAgainBtn">Play Again</button>
    </div>
  </div>
</div>

<script>
/* ============================================================
   AI SUPPLY CHAIN CONTROL TOWER — GAME ENGINE
   Vanilla JS, single file, no dependencies.
   ============================================================ */

/* ---------- STATIC DATA ---------- */

const ACTIONS = {
  expedite:   'Expedite Shipment',
  backup:     'Use Backup Supplier',
  reroute:    'Reroute Trucks',
  production: 'Increase Production',
  transfer:   'Transfer Inventory',
  air:        'Approve Air Freight'
};

// Each alert type defines the actions relevant to it, in order of quality.
// relevant[0] is the "best" action; the rest are plausible-but-suboptimal.
const ALERT_TYPES = [
  { key:'port',     icon:'🚢', title:'Port Congestion',            desc:'Containers are stuck at the port awaiting customs clearance.',            impact:'transportEfficiency', relevant:['reroute','expedite','air'] },
  { key:'supplier', icon:'🏭', title:'Supplier Delay',              desc:'A key supplier reports a 48-hour delay on critical components.',          impact:'inventoryHealth',      relevant:['backup','transfer','expedite'] },
  { key:'truck',    icon:'🚛', title:'Truck Breakdown',             desc:'A delivery truck has broken down mid-route.',                              impact:'transportEfficiency', relevant:['reroute','backup','air'] },
  { key:'stock',    icon:'📦', title:'Warehouse Running Low',       desc:'A regional warehouse is nearing zero stock on a fast-moving SKU.',         impact:'inventoryHealth',      relevant:['transfer','backup','production'] },
  { key:'customs',  icon:'🛃', title:'Customs Inspection',          desc:'A shipment has been flagged for a random customs inspection.',            impact:'transportEfficiency', relevant:['expedite','air','reroute'] },
  { key:'demand',   icon:'📈', title:'Demand Spike',                desc:'A sudden surge in orders is hitting a flagship product line.',            impact:'satisfaction',         relevant:['production','transfer','backup'] },
  { key:'machine',  icon:'⚙️', title:'Factory Machine Failure',     desc:'A production line machine has malfunctioned, halting output.',            impact:'inventoryHealth',      relevant:['production','backup','transfer'] },
  { key:'weather',  icon:'🌪️', title:'Weather Disruption',          desc:'A storm system is disrupting regional logistics routes.',                 impact:'transportEfficiency', relevant:['reroute','air','expedite'] },
  { key:'miscount', icon:'📋', title:'Wrong Inventory Count',       desc:'A system audit reveals an inventory count mismatch at a DC.',              impact:'inventoryHealth',      relevant:['transfer','backup','production'] },
  { key:'damaged',  icon:'💥', title:'Damaged Shipment',            desc:'A shipment has arrived with visibly damaged goods.',                       impact:'satisfaction',         relevant:['air','expedite','backup'] }
];

const PRIORITIES = {
  high:   { label:'High',   duration:14, dollar:15000, mult:1.3 },
  medium: { label:'Medium', duration:20, dollar:8000,  mult:1.0 },
  low:    { label:'Low',    duration:26, dollar:4000,  mult:0.7 }
};

const GAME_DURATION = 180;   // seconds
const MAX_CONCURRENT = 6;

/* ---------- STATE ---------- */

let state = null;
let tickHandle = null;
let alertIdCounter = 0;

function freshState(){
  return {
    running:false,
    paused:false,
    elapsed:0,
    timeLeft:GAME_DURATION,
    spawnAcc:0,
    kpi:{
      serviceLevel:100,
      satisfaction:100,
      inventoryHealth:100,
      transportEfficiency:100,
      cost:100
    },
    revenue:0,
    score:0,
    resolved:0,
    correct:0,
    wrong:0,
    logId:0,
    alerts:[]   // active alert objects
  };
}

/* ---------- UTIL ---------- */

function clamp(v,min,max){ return Math.max(min, Math.min(max, v)); }

function formatTime(s){
  s = Math.max(0, Math.ceil(s));
  const m = Math.floor(s/60);
  const sec = s%60;
  return String(m).padStart(2,'0') + ':' + String(sec).padStart(2,'0');
}

function formatMoney(n){
  const sign = n<0 ? '-' : '';
  n = Math.abs(Math.round(n));
  if(n>=1000) return sign+'$'+(n/1000).toFixed(1)+'K';
  return sign+'$'+n;
}

function pickWeightedPriority(elapsedRatio){
  // As the shift progresses, high-priority alerts become more likely.
  const highChance = 0.25 + elapsedRatio*0.35;
  const medChance = 0.4;
  const r = Math.random();
  if(r < highChance) return 'high';
  if(r < highChance+medChance) return 'medium';
  return 'low';
}

/* ---------- SPAWNING ---------- */

function spawnInterval(elapsedRatio){
  // 7s at start, tightening to ~2.5s near the end.
  return Math.max(2.5, 7 - elapsedRatio*4.5);
}

function createAlert(){
  const type = ALERT_TYPES[Math.floor(Math.random()*ALERT_TYPES.length)];
  const priorityKey = pickWeightedPriority(state.elapsed/GAME_DURATION);
  const priority = PRIORITIES[priorityKey];
  alertIdCounter++;
  return {
    id:'a'+alertIdCounter,
    type,
    priorityKey,
    priority,
    duration:priority.duration,
    timeLeft:priority.duration,
    delayed:false,
    delayUsed:false
  };
}

/* ---------- KPI EFFECTS ---------- */

const EFFECT_TABLE = {
  best:   { impact:6,  serviceLevel:3,  satisfaction:2,  cost:-3, revenueMult:1,   scoreBase:120 },
  ok:     { impact:2,  serviceLevel:1,  satisfaction:0,  cost:2,  revenueMult:0.4, scoreBase:40  },
  wrong:  { impact:-5, serviceLevel:-2, satisfaction:-3, cost:6,  revenueMult:0,   scoreBase:-60 },
  timeout:{ impact:-7, serviceLevel:-4, satisfaction:-5, cost:8,  revenueMult:0,   scoreBase:-90 }
};

function actionQuality(alert, actionId){
  if(actionId === alert.type.relevant[0]) return 'best';
  if(alert.type.relevant.includes(actionId)) return 'ok';
  return 'wrong';
}

function applyEffect(alert, quality, isDelayed){
  const eff = EFFECT_TABLE[quality];
  const scale = isDelayed ? 0.5 : 1;
  const impactKey = alert.type.impact;

  state.kpi[impactKey] = clamp(state.kpi[impactKey] + eff.impact*scale, 0, 100);
  state.kpi.serviceLevel = clamp(state.kpi.serviceLevel + eff.serviceLevel*scale, 0, 100);
  state.kpi.satisfaction = clamp(state.kpi.satisfaction + eff.satisfaction*scale, 0, 100);
  state.kpi.cost = clamp(state.kpi.cost + eff.cost*scale + (isDelayed?2:0), 40, 200);

  const revenue = eff.revenueMult * alert.priority.dollar * scale;
  state.revenue += revenue;

  const scoreDelta = Math.round(eff.scoreBase * alert.priority.mult * scale);
  state.score += scoreDelta;

  if(quality==='best' || quality==='ok'){ state.correct++; } else { state.wrong++; }
  state.resolved++;

  return scoreDelta;
}

/* ---------- ALERT RESOLUTION ---------- */

function handleAction(alertId, actionId){
  const idx = state.alerts.findIndex(a=>a.id===alertId);
  if(idx===-1) return;
  const alert = state.alerts[idx];

  if(actionId==='delay'){
    if(alert.delayUsed) return; // one delay per alert
    alert.delayUsed = true;
    alert.delayed = true;
    alert.timeLeft = Math.min(alert.timeLeft, 10) + 8;
    state.kpi.cost = clamp(state.kpi.cost + 1, 40, 200);
    state.score -= 10;
    logEvent(`Decision delayed on "${alert.type.title}" — outcome will be weaker.`, 'info');
    renderAll();
    return;
  }

  let quality;
  if(actionId==='ignore'){
    quality = 'timeout';
  } else {
    quality = actionQuality(alert, actionId);
  }

  const scoreDelta = applyEffect(alert, quality, alert.delayed);
  const label = actionId==='ignore' ? 'Ignore' : ACTIONS[actionId];
  logResolution(alert, label, quality, scoreDelta);

  state.alerts.splice(idx,1);
  renderAll();
  checkKpiFlashes();
}

function autoTimeout(alert){
  const idx = state.alerts.findIndex(a=>a.id===alert.id);
  if(idx===-1) return;
  const scoreDelta = applyEffect(alert, 'timeout', alert.delayed);
  logResolution(alert, 'No action (timed out)', 'timeout', scoreDelta);
  state.alerts.splice(idx,1);
}

function logResolution(alert, actionLabel, quality, scoreDelta){
  const cls = quality==='best' ? 'good' : quality==='ok' ? 'ok' : 'bad';
  const verb = quality==='best' ? 'Resolved well' : quality==='ok' ? 'Resolved' : quality==='wrong' ? 'Mishandled' : 'Missed';
  const sign = scoreDelta>=0 ? '+' : '';
  logEvent(`${verb}: "${alert.type.title}" → ${actionLabel} (${sign}${scoreDelta} pts)`, cls);
}

/* ---------- LOG ---------- */

function logEvent(text, cls){
  const el = document.getElementById('logBody');
  const div = document.createElement('div');
  div.className = 'log-entry ' + (cls||'info');
  const t = formatTime(state.timeLeft);
  div.innerHTML = `<span class="t">${t}</span>${escapeHtml(text)}`;
  el.appendChild(div);
  state.logId++;
  document.getElementById('logCount').textContent = state.logId;
}

function escapeHtml(str){
  const d = document.createElement('div');
  d.textContent = str;
  return d.innerHTML;
}

/* ---------- RENDERING ---------- */

function renderAll(){
  renderAlerts();
  renderKpis();
  renderStrip();
}

function renderAlerts(){
  const body = document.getElementById('alertsBody');
  const empty = document.getElementById('emptyState');
  document.getElementById('alertCount').textContent = state.alerts.length;

  // Preserve DOM nodes where possible would be nicer, but a full re-render
  // keeps the logic simple and the alert set changes frequently anyway.
  body.querySelectorAll('.alert-card').forEach(n=>n.remove());
  if(empty) empty.style.display = state.alerts.length===0 && state.running ? 'block' : 'none';
  if(!state.running && empty){ empty.style.display='block'; }

  state.alerts.forEach(alert=>{
    body.appendChild(buildAlertCard(alert));
  });
}

function buildAlertCard(alert){
  const card = document.createElement('div');
  card.className = `alert-card priority-${alert.priorityKey}`;
  card.dataset.id = alert.id;

  const pct = clamp((alert.timeLeft/alert.duration)*100, 0, 100);
  const barColor = pct>50 ? 'var(--green)' : pct>20 ? 'var(--orange)' : 'var(--red)';

  const contextButtons = alert.type.relevant.slice(0,3).map(a=>
    `<button class="action-btn" data-action="${a}">${ACTIONS[a]}</button>`
  ).join('');

  card.innerHTML = `
    <div class="alert-head">
      <div class="icon">${alert.type.icon}</div>
      <div class="titles">
        <h3>${alert.type.title}</h3>
        <div class="impact">Impacts: ${labelForImpact(alert.type.impact)}</div>
      </div>
      <div class="priority-tag ${alert.priorityKey}">${alert.priority.label}</div>
    </div>
    ${alert.delayed ? '<span class="delayed-tag">Delayed — reduced outcome</span>' : ''}
    <p class="alert-desc">${alert.type.desc}</p>
    <div class="countdown-bar"><div style="width:${pct}%; background:${barColor};"></div></div>
    <div class="alert-actions">
      ${contextButtons}
      <button class="action-btn delay" data-action="delay" ${alert.delayUsed?'disabled style="opacity:.35;pointer-events:none;"':''}>Delay Decision</button>
      <button class="action-btn ignore" data-action="ignore">Ignore</button>
    </div>
  `;

  card.querySelectorAll('.action-btn').forEach(btn=>{
    btn.addEventListener('click', ()=>handleAction(alert.id, btn.dataset.action));
  });

  return card;
}

function labelForImpact(key){
  return key==='inventoryHealth' ? 'Inventory Health'
       : key==='transportEfficiency' ? 'Transport Efficiency'
       : 'Customer Satisfaction';
}

// Update just the countdown bars without a full re-render (called every tick)
function updateCountdownBars(){
  state.alerts.forEach(alert=>{
    const card = document.querySelector(`.alert-card[data-id="${alert.id}"]`);
    if(!card) return;
    const pct = clamp((alert.timeLeft/alert.duration)*100, 0, 100);
    const barColor = pct>50 ? 'var(--green)' : pct>20 ? 'var(--orange)' : 'var(--red)';
    const bar = card.querySelector('.countdown-bar > div');
    if(bar){ bar.style.width = pct+'%'; bar.style.background = barColor; }
  });
}

function renderKpis(){
  setKpi('serviceLevel', Math.round(state.kpi.serviceLevel)+'%', state.kpi.serviceLevel);
  setKpi('satisfaction', Math.round(state.kpi.satisfaction)+'%', state.kpi.satisfaction);
  setKpi('inventoryHealth', Math.round(state.kpi.inventoryHealth)+'%', state.kpi.inventoryHealth);
  setKpi('transportEfficiency', Math.round(state.kpi.transportEfficiency)+'%', state.kpi.transportEfficiency);

  const costCard = document.querySelector('.kpi-card[data-kpi="cost"]');
  const costPct = clamp(100 - (state.kpi.cost-40), 0, 100);
  const costColor = state.kpi.cost<=100 ? 'var(--green)' : state.kpi.cost<=140 ? 'var(--orange)' : 'var(--red)';
  costCard.querySelector('.value').textContent = Math.round(state.kpi.cost);
  costCard.querySelector('.value').style.color = costColor;
  costCard.querySelector('.kpi-bar > div').style.width = costPct+'%';
  costCard.querySelector('.kpi-bar > div').style.background = costColor;

  const composite = (state.kpi.serviceLevel + state.kpi.satisfaction + state.kpi.inventoryHealth + state.kpi.transportEfficiency)/4;
  const compCard = document.querySelector('.kpi-card[data-kpi="composite"]');
  const compColor = composite>=80 ? 'var(--green)' : composite>=55 ? 'var(--orange)' : 'var(--red)';
  compCard.querySelector('.value').textContent = Math.round(composite)+'%';
  compCard.querySelector('.value').style.color = compColor;
  compCard.querySelector('.kpi-bar > div').style.width = composite+'%';
  compCard.querySelector('.kpi-bar > div').style.background = compColor;
}

function setKpi(key, text, value){
  const card = document.querySelector(`.kpi-card[data-kpi="${key}"]`);
  const color = value>=80 ? 'var(--green)' : value>=55 ? 'var(--orange)' : 'var(--red)';
  card.querySelector('.value').textContent = text;
  card.querySelector('.value').style.color = color;
  card.querySelector('.kpi-bar > div').style.width = value+'%';
  card.querySelector('.kpi-bar > div').style.background = color;
}

let lastComposite = 100;
function checkKpiFlashes(){
  const composite = (state.kpi.serviceLevel + state.kpi.satisfaction + state.kpi.inventoryHealth + state.kpi.transportEfficiency)/4;
  const compCard = document.querySelector('.kpi-card[data-kpi="composite"]');
  compCard.classList.remove('pulse-good','pulse-bad');
  if(composite > lastComposite) compCard.classList.add('pulse-good');
  else if(composite < lastComposite) compCard.classList.add('pulse-bad');
  setTimeout(()=>compCard.classList.remove('pulse-good','pulse-bad'), 500);
  lastComposite = composite;
}

function renderStrip(){
  document.getElementById('timeValue').textContent = formatTime(state.timeLeft);
  document.getElementById('scoreValue').textContent = state.score;
  document.getElementById('revenueValue').textContent = formatMoney(state.revenue);
  const timerCard = document.getElementById('timerCard');
  timerCard.classList.toggle('low-time', state.timeLeft<=20 && state.running);
}

/* ---------- GAME LOOP ---------- */

function tick(){
  if(!state.running || state.paused) return;

  const dt = 0.1; // seconds per tick (interval = 100ms)
  state.elapsed += dt;
  state.timeLeft -= dt;

  // advance alert timers
  const expired = [];
  state.alerts.forEach(alert=>{
    alert.timeLeft -= dt;
    if(alert.timeLeft <= 0) expired.push(alert);
  });
  expired.forEach(alert=>autoTimeout(alert));
  if(expired.length){ renderAlerts(); }
  updateCountdownBars();

  // spawn new alerts
  state.spawnAcc += dt;
  const interval = spawnInterval(state.elapsed/GAME_DURATION);
  if(state.spawnAcc >= interval && state.alerts.length < MAX_CONCURRENT){
    state.spawnAcc = 0;
    state.alerts.push(createAlert());
    renderAlerts();
  }

  renderStrip();

  if(state.timeLeft <= 0){
    state.timeLeft = 0;
    renderStrip();
    endGame();
  }
}

function startGame(){
  state = freshState();
  state.running = true;
  document.getElementById('logBody').innerHTML = '';
  document.getElementById('startOverlay').classList.remove('open');
  document.getElementById('emptyState').style.display = 'none';
  renderAll();
  logEvent('Shift started. Control tower is live.', 'info');
  if(tickHandle) clearInterval(tickHandle);
  tickHandle = setInterval(tick, 100);
}

function endGame(){
  state.running = false;
  clearInterval(tickHandle);
  showGameOver();
}

function togglePause(){
  if(!state || !state.running) return;
  state.paused = !state.paused;
  document.getElementById('pauseOverlay').classList.toggle('open', state.paused);
  document.getElementById('pauseBtn').textContent = state.paused ? '▶' : '⏸';
}

/* ---------- GAME OVER SCREEN ---------- */

function computeGrade(){
  const composite = (state.kpi.serviceLevel + state.kpi.satisfaction + state.kpi.inventoryHealth + state.kpi.transportEfficiency)/4;
  if(composite>=90) return {grade:'A+', color:'var(--green)'};
  if(composite>=80) return {grade:'A',  color:'var(--green)'};
  if(composite>=65) return {grade:'B',  color:'var(--cyan)'};
  if(composite>=50) return {grade:'C',  color:'var(--orange)'};
  return {grade:'D', color:'var(--red)'};
}

function summaryFor(grade){
  const map = {
    'A+':'Exceptional shift. The network stayed resilient under pressure and every major disruption was neutralized before it could hit customers.',
    'A':'Strong operational control. Most disruptions were handled with the right playbook, keeping service levels and costs in a healthy band.',
    'B':'Solid but uneven performance. Several issues were resolved well, though some avoidable delays crept into the network.',
    'C':'A rocky shift. Reactive decisions and missed alerts put pressure on service levels and drove operating costs up.',
    'D':'The network struggled to keep pace. Frequent timeouts and mismatched responses eroded customer trust and inflated costs.'
  };
  return map[grade];
}

function showGameOver(){
  const { grade, color } = computeGrade();
  const badge = document.getElementById('gradeBadge');
  badge.textContent = grade;
  badge.style.color = color;
  document.getElementById('finalScoreLine').textContent = `Final Score: ${state.score}`;

  const kpiGrid = document.getElementById('finalKpiGrid');
  kpiGrid.innerHTML = `
    <div class="stat-box"><div class="l">Service Level</div><div class="v">${Math.round(state.kpi.serviceLevel)}%</div></div>
    <div class="stat-box"><div class="l">Satisfaction</div><div class="v">${Math.round(state.kpi.satisfaction)}%</div></div>
    <div class="stat-box"><div class="l">Inventory Health</div><div class="v">${Math.round(state.kpi.inventoryHealth)}%</div></div>
    <div class="stat-box"><div class="l">Transport Efficiency</div><div class="v">${Math.round(state.kpi.transportEfficiency)}%</div></div>
    <div class="stat-box"><div class="l">Operating Cost</div><div class="v">${Math.round(state.kpi.cost)}</div></div>
    <div class="stat-box"><div class="l">Revenue Protected</div><div class="v">${formatMoney(state.revenue)}</div></div>
  `;

  document.getElementById('statResolved').textContent = state.resolved;
  document.getElementById('statCorrect').textContent = state.correct;
  document.getElementById('statWrong').textContent = state.wrong;
  document.getElementById('summaryText').textContent = summaryFor(grade);

  document.getElementById('gameOverModal').classList.add('open');
}

/* ---------- EVENT WIRING ---------- */

document.getElementById('startBtn').addEventListener('click', startGame);
document.getElementById('playAgainBtn').addEventListener('click', ()=>{
  document.getElementById('gameOverModal').classList.remove('open');
  startGame();
});
document.getElementById('closeGameOverBtn').addEventListener('click', ()=>{
  document.getElementById('gameOverModal').classList.remove('open');
  document.getElementById('startOverlay').classList.add('open');
});

document.getElementById('pauseBtn').addEventListener('click', togglePause);
document.getElementById('resumeBtn').addEventListener('click', togglePause);

let soundOn = true;
document.getElementById('soundBtn').addEventListener('click', function(){
  soundOn = !soundOn;
  this.textContent = soundOn ? '🔊' : '🔇';
  this.classList.toggle('active', soundOn);
});

document.getElementById('helpBtn').addEventListener('click', ()=>document.getElementById('helpModal').classList.add('open'));
document.getElementById('startHelpBtn').addEventListener('click', ()=>document.getElementById('helpModal').classList.add('open'));
document.getElementById('closeHelpBtn').addEventListener('click', ()=>document.getElementById('helpModal').classList.remove('open'));
document.getElementById('helpModal').addEventListener('click', (e)=>{ if(e.target.id==='helpModal') e.target.classList.remove('open'); });

/* Initialize a blank state so KPI cards render sensibly before the first shift */
state = freshState();
renderStrip();
</script>

</body>
</html>
