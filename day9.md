<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NutriScope Pro - Enhanced</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
:root{--bg:#0f172a;--card:#1e293b;--text:#e2e8f0;--muted:#94a3b8;--accent:#22c55e;--warn:#f59e0b;--danger:#ef4444}
*{box-sizing:border-box;margin:0;padding:0;font-family:Inter,system-ui,Segoe UI,Arial}
body{background:var(--bg);color:var(--text);padding:16px}
.container{max-width:1300px;margin:0 auto}
h1{font-size:1.8rem;margin-bottom:8px}
.sub{color:var(--muted);margin-bottom:20px}
.grid{display:grid;gap:16px}
.grid-2{grid-template-columns:1fr 1fr}
.grid-3{grid-template-columns:repeat(3,1fr)}
@media(max-width:900px){.grid-2,.grid-3{grid-template-columns:1fr}}
.card{background:var(--card);border-radius:16px;padding:16px;border:1px solid #334155}
label{display:block;font-size:0.85rem;color:var(--muted);margin-bottom:4px}
input,select,button{width:100%;padding:10px;border-radius:8px;border:1px solid #334155;background:#0b1220;color:var(--text)}
button{background:var(--accent);border:none;font-weight:600;cursor:pointer}
button.ghost{background:transparent;border:1px solid #334155}
.row{display:flex;gap:8px}
.row>*{flex:1}
table{width:100%;border-collapse:collapse;margin-top:8px;font-size:0.9rem}
th,td{padding:8px;text-align:left;border-bottom:1px solid #334155}
th{color:var(--muted);font-weight:500}
.progress{height:10px;background:#334155;border-radius:6px;overflow:hidden}
.progress>div{height:100%;background:var(--accent);transition:width 0.3s}
.badge{padding:4px 8px;border-radius:6px;font-size:0.75rem}
.badge.good{background:rgba(34,197,94,0.2);color:#4ade80}
.badge.warn{background:rgba(245,158,11,0.2);color:#fbbf24}
.badge.bad{background:rgba(239,68,68,0.2);color:#f87171}
.kpi{font-size:1.5rem;font-weight:700}
.muted{color:var(--muted);font-size:0.85rem}
canvas{max-height:240px}
.tabs{display:flex;gap:8px;margin-bottom:12px}
.tab{padding:8px 12px;border-radius:8px;background:#334155;cursor:pointer}
.tab.active{background:var(--accent);color:#0b1220}
.disclaimer{font-size:0.8rem;color:var(--muted);border-left:3px solid var(--warn);padding-left:8px;margin-top:12px}
</style>
</head>
<body>
<div class="container">
<h1>🥗 NutriScope Pro</h1>
<p class="sub">Enhanced version with CSV + Meal Planner + Risk Analysis</p>

<div class="tabs">
  <div class="tab active" onclick="switchTab('tracker')">Tracker</div>
  <div class="tab" onclick="switchTab('planner')">2-Day Planner</div>
  <div class="tab" onclick="switchTab('risk')">Risk Analysis</div>
</div>

<div id="tracker">
<div class="grid grid-2">
  <div class="card">
    <h3>Profile</h3>
    <div class="grid grid-2" style="margin-top:12px">
      <div><label>Age</label><input id="age" type="number" value="25"></div>
      <div><label>Gender</label><select id="gender"><option>Male</option><option>Female</option></select></div>
      <div><label>Height cm</label><input id="height" type="number" value="170"></div>
      <div><label>Weight kg</label><input id="weight" type="number" value="70"></div>
      <div><label>Activity</label><select id="activity"><option value="1.2">Sedentary</option><option value="1.375">Light</option><option value="1.55" selected>Moderate</option><option value="1.725">Active</option></select></div>
      <div><label>Diet</label><select id="diet"><option>Vegetarian</option><option>Non-Vegetarian</option><option>Eggetarian</option></select></div>
    </div>
    <button style="margin-top:12px" onclick="updateTargets()">Update Targets</button>
  </div>
  <div class="card">
    <h3>Add Food + CSV Upload</h3>
    <div class="row" style="margin-top:12px">
      <div style="flex:2"><label>Food - 60 items</label><select id="foodSelect"></select></div>
      <div><label>Qty</label><input id="qty" type="number" value="100"></div>
      <div><label>Unit</label><select id="unit"><option value="g">g</option><option value="cup">cup</option><option value="piece">piece</option></select></div>
    </div>
    <button style="margin-top:12px" onclick="addFood()">Add Entry</button>
    <label style="margin-top:12px">Upload CSV: food,qty,unit</label>
    <input type="file" accept=".csv" onchange="uploadCSV(event)">
  </div>
</div>

<div class="grid grid-3" style="margin-top:16px">
  <div class="card"><div class="muted">Energy</div><div class="kpi"><span id="kcal">0</span>/<span id="kcalTarget">0</span> kcal</div><div class="progress"><div id="kcalBar"></div></div></div>
  <div class="card"><div class="muted">Protein</div><div class="kpi"><span id="protein">0</span>/<span id="proteinTarget">0</span> g</div><div class="progress"><div id="proteinBar"></div></div></div>
  <div class="card"><div class="muted">Carbs / Fat</div><div class="kpi"><span id="carbs">0</span>/<span id="carbsTarget">0</span> g | <span id="fat">0</span>/<span id="fatTarget">0</span> g</div><div class="progress"><div id="carbsBar"></div></div></div>
</div>

<div class="grid grid-2" style="margin-top:16px">
  <div class="card"><h3>Macro Distribution</h3><canvas id="macroChart"></canvas></div>
  <div class="card"><h3>Micronutrients Radar</h3><canvas id="microChart"></canvas></div>
</div>

<div class="card" style="margin-top:16px"><h3>Food Log</h3><table id="logTable"><thead><tr><th>Food</th><th>Qty</th><th>Kcal</th><th>Protein</th><th>Carbs</th><th>Fat</th><th>Fiber</th><th>Iron</th><th></th></tr></thead><tbody></tbody></table></div>
<div class="card" style="margin-top:16
