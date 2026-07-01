<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fuel Economics Dashboard</title>
<style>
  :root{
    --bg:#0a0f1e;
    --panel:rgba(255,255,255,0.045);
    --panel-border:rgba(255,255,255,0.09);
    --text:#e8ecf5;
    --text-dim:#8b93a7;
    --text-faint:#5b6376;
    --petrol:#4d8dff;
    --diesel:#9aa5b1;
    --cng:#34d399;
    --e85:#f5a623;
    --ev:#a78bfa;
    --radius:16px;
  }
  *{box-sizing:border-box;margin:0;padding:0;}
  body{
    background:
      radial-gradient(ellipse 900px 500px at 15% -10%, rgba(77,141,255,0.16), transparent 60%),
      radial-gradient(ellipse 700px 500px at 90% 10%, rgba(167,139,250,0.10), transparent 55%),
      var(--bg);
    color:var(--text);
    font-family:"Segoe UI",system-ui,-apple-system,sans-serif;
    min-height:100vh;
    padding:20px 16px 60px;
  }
  .wrap{max-width:1280px;margin:0 auto;}

  .note{
    max-width:1280px;margin:0 auto 16px;
    font-size:12px;color:var(--text-faint);
    border:1px dashed var(--panel-border);
    border-radius:10px;padding:8px 12px;
    font-family:Consolas,Monaco,monospace;
  }

  header{
    display:flex;flex-wrap:wrap;align-items:baseline;gap:10px 18px;
    padding:20px 4px 22px;
  }
  header h1{
    font-family:"Trebuchet MS",system-ui,sans-serif;
    font-size:clamp(22px,3.4vw,32px);
    font-weight:700;
    letter-spacing:0.3px;
  }
  header .meta{
    font-family:Consolas,Monaco,monospace;
    font-size:13px;color:var(--text-dim);
    letter-spacing:0.5px;
  }
  header .dot{color:var(--petrol);}

  .glass{
    background:var(--panel);
    border:1px solid var(--panel-border);
    border-radius:var(--radius);
    backdrop-filter:blur(14px);
    -webkit-backdrop-filter:blur(14px);
  }

  /* KPI cards */
  .kpi-grid{
    display:grid;
    grid-template-columns:repeat(5,1fr);
    gap:14px;margin-bottom:22px;
  }
  .kpi{
    padding:16px 16px 14px;
    display:flex;flex-direction:column;gap:6px;
  }
  .kpi .label{font-size:11px;color:var(--text-dim);text-transform:uppercase;letter-spacing:0.6px;}
  .kpi .value{font-family:Consolas,Monaco,monospace;font-size:24px;font-weight:700;}
  .kpi .sub{font-size:11px;color:var(--text-faint);}
  .kpi.accent .value{color:var(--petrol);}
  .kpi.e85 .value{color:var(--e85);}

  /* generic section grid */
  .row{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:16px;}
  .card{padding:18px 20px;}
  .card h2{font-size:14px;font-weight:600;letter-spacing:0.3px;margin-bottom:4px;}
  .card .desc{font-size:11px;color:var(--text-faint);margin-bottom:12px;}

  svg{width:100%;height:auto;display:block;overflow:visible;}
  .axis-label{fill:var(--text-faint);font-size:9px;font-family:Consolas,Monaco,monospace;}
  .bar{transition:opacity .15s;cursor:pointer;}
  .bar:hover{opacity:0.75;}
  .doughseg{transition:opacity .15s;cursor:pointer;}
  .doughseg:hover{opacity:0.75;}
  .lineage{cursor:pointer;}

  .legend{display:flex;flex-wrap:wrap;gap:10px 14px;margin-top:10px;}
  .legend span{display:inline-flex;align-items:center;gap:6px;font-size:11px;color:var(--text-dim);}
  .swatch{width:9px;height:9px;border-radius:2px;display:inline-block;}

  .tooltip{
    position:absolute;pointer-events:none;
    background:#111a30;border:1px solid var(--panel-border);
    border-radius:8px;padding:6px 10px;font-size:11px;
    font-family:Consolas,Monaco,monospace;
    color:var(--text);opacity:0;transition:opacity .1s;
    z-index:20;white-space:nowrap;
    box-shadow:0 8px 24px rgba(0,0,0,0.4);
  }
  .chartwrap{position:relative;}

  /* gauge */
  .gauge-row{display:grid;grid-template-columns:280px 1fr;gap:20px;align-items:center;}
  .verdict{font-size:14px;line-height:1.6;color:var(--text-dim);}
  .verdict b{color:var(--e85);}
  #gaugeArc{transition:stroke-dashoffset 1.4s cubic-bezier(.2,.8,.2,1);}
  .gauge-score{font-family:Consolas,Monaco,monospace;font-size:30px;font-weight:700;fill:var(--e85);}
  .gauge-max{font-size:12px;fill:var(--text-faint);}

  /* fuel cards */
  .fuel-grid{
    display:grid;grid-template-columns:repeat(5,1fr);gap:14px;margin-top:4px;
  }
  .fuel-card{
    padding:16px 14px;display:flex;flex-direction:column;gap:8px;
    border-top:3px solid var(--panel-border);
  }
  .fuel-card h3{font-size:14px;display:flex;align-items:center;gap:6px;}
  .fuel-card .dot{width:8px;height:8px;border-radius:50%;display:inline-block;}
  .fuel-card ul{list-style:none;font-size:11.5px;color:var(--text-dim);display:flex;flex-direction:column;gap:4px;}
  .fuel-card .bestfor{font-size:11px;color:var(--text-faint);margin-top:2px;border-top:1px solid var(--panel-border);padding-top:8px;}
  .fuel-card.highlight{
    border-top-color:var(--petrol);
    box-shadow:0 0 0 1px rgba(77,141,255,0.35), 0 0 32px rgba(77,141,255,0.28);
  }
  .fuel-card.highlight h3{color:var(--petrol);}

  footer{text-align:center;font-size:10px;color:var(--text-faint);margin-top:30px;font-family:Consolas,Monaco,monospace;}

  @media (max-width:1000px){
    .kpi-grid{grid-template-columns:repeat(2,1fr);}
    .row{grid-template-columns:1fr;}
    .fuel-grid{grid-template-columns:repeat(2,1fr);}
    .gauge-row{grid-template-columns:1fr;}
  }
  @media (max-width:480px){
    .kpi-grid{grid-template-columns:1fr 1fr;}
    .fuel-grid{grid-template-columns:1fr;}
  }
</style>
</head>
<body>
<div class="note">Sample/illustrative dataset — no CSV or vehicle details were provided yet. Replace with real data for accurate results.</div>
<div class="wrap">

  <header>
    <h1>Maruti Suzuki Swift <span class="dot">·</span> Petrol <span class="dot">·</span> Age: 3y <span class="dot">·</span> 1,200 km/mo</h1>
  </header>

  <!-- KPI CARDS -->
  <div class="kpi-grid">
    <div class="glass kpi accent">
      <div class="label">Your Cost / km</div>
      <div class="value">₹7.50</div>
      <div class="sub">Petrol</div>
    </div>
    <div class="glass kpi e85">
      <div class="label">E85 Cost / km</div>
      <div class="value">₹8.90</div>
      <div class="sub">vs your fuel</div>
    </div>
    <div class="glass kpi e85">
      <div class="label">E85 Premium</div>
      <div class="value">+18.7%</div>
      <div class="sub">running cost penalty</div>
    </div>
    <div class="glass kpi e85">
      <div class="label">E85 Break-even</div>
      <div class="value">₹75.83</div>
      <div class="sub">pump price / L</div>
    </div>
    <div class="glass kpi accent">
      <div class="label">Your Monthly Cost</div>
      <div class="value">₹9,000</div>
      <div class="sub">1,200 km @ ₹7.50/km</div>
    </div>
  </div>

  <!-- BAR + DOUGHNUT -->
  <div class="row">
    <div class="glass card">
      <h2>Cost / km by Fuel</h2>
      <div class="desc">₹ per kilometre, avg across sample fleet</div>
      <div class="chartwrap">
        <svg id="barChart" viewBox="0 0 420 230"></svg>
        <div class="tooltip" id="barTooltip"></div>
      </div>
    </div>
    <div class="glass card">
      <h2>CO₂ / km by Fuel</h2>
      <div class="desc">kg CO₂ emitted per kilometre</div>
      <div class="chartwrap">
        <svg id="doughChart" viewBox="0 0 260 230"></svg>
        <div class="tooltip" id="doughTooltip"></div>
      </div>
      <div class="legend" id="doughLegend"></div>
    </div>
  </div>

  <!-- LINE CHART -->
  <div class="glass card" style="margin-bottom:16px;">
    <h2>Cost / km vs Vehicle Age</h2>
    <div class="desc">Projected 0–12 years · dashed line marks your car's age (3y)</div>
    <div class="chartwrap">
      <svg id="lineChart" viewBox="0 0 900 260"></svg>
      <div class="tooltip" id="lineTooltip"></div>
    </div>
    <div class="legend" id="lineLegend"></div>
  </div>

  <!-- GAUGE -->
  <div class="glass card" style="margin-bottom:16px;">
    <h2>E85 Suitability Score</h2>
    <div class="desc">Weighted: Cost 4pt · CO₂ 3pt · Refuel time 2pt · Maintenance 1pt</div>
    <div class="gauge-row">
      <svg id="gaugeSvg" viewBox="0 0 280 160"></svg>
      <div class="verdict">
        <b>7.2 / 10 —</b> E85 cuts CO₂ and maintenance cost noticeably, but its ~19% running-cost penalty over petrol
        means it only pays off if pump prices sit near or below the ₹75.83/L break-even — worth it for
        emissions-focused flex-fuel owners, not for pure cost minimisers.
      </div>
    </div>
  </div>

  <!-- FUEL CARDS -->
  <div class="glass card">
    <h2>Fuel Type Comparison</h2>
    <div class="desc">Your vehicle runs on <b style="color:var(--petrol)">Petrol</b>, highlighted below</div>
    <div class="fuel-grid" id="fuelCards"></div>
  </div>

  <footer>SAMPLE DATA · Generated dashboard · Replace with your CSV for real figures</footer>
</div>

<script>
const COLORS = { Petrol:'#4d8dff', Diesel:'#9aa5b1', CNG:'#34d399', E85:'#f5a623', EV:'#a78bfa' };

const costPerKm = { Petrol:7.5, Diesel:6.2, CNG:3.8, E85:8.9, EV:1.2 };
const co2PerKm  = { Petrol:0.15, Diesel:0.18, CNG:0.10, E85:0.09, EV:0.05 };
const lineBase  = { Petrol:[7.0,0.12], Diesel:[5.8,0.11], CNG:[3.4,0.08], E85:[8.4,0.10], EV:[1.0,0.045] };
const CAR_AGE = 3;

/* ---------- BAR CHART ---------- */
(function(){
  const svg = document.getElementById('barChart');
  const tip = document.getElementById('barTooltip');
  const fuels = Object.keys(costPerKm);
  const max = Math.max(...Object.values(costPerKm)) * 1.2;
  const chartH = 170, chartW = 380, left = 30, top = 10;
  const bw = chartW / fuels.length * 0.55;
  const gap = chartW / fuels.length;

  fuels.forEach((f,i)=>{
    const val = costPerKm[f];
    const h = (val/max)*chartH;
    const x = left + i*gap + (gap-bw)/2;
    const y = top + chartH - h;
    const rect = document.createElementNS('http://www.w3.org/2000/svg','rect');
    rect.setAttribute('x',x); rect.setAttribute('y',y);
    rect.setAttribute('width',bw); rect.setAttribute('height',h);
    rect.setAttribute('rx',4);
    rect.setAttribute('fill',COLORS[f]);
    rect.setAttribute('class','bar');
    rect.addEventListener('mousemove',(e)=>{
      tip.style.opacity=1;
      tip.style.left=(e.offsetX+12)+'px';
      tip.style.top=(e.offsetY-10)+'px';
      tip.textContent = f+': ₹'+val.toFixed(2)+'/km';
    });
    rect.addEventListener('mouseleave',()=>tip.style.opacity=0);
    svg.appendChild(rect);

    const label = document.createElementNS('http://www.w3.org/2000/svg','text');
    label.setAttribute('x', x+bw/2); label.setAttribute('y', top+chartH+16);
    label.setAttribute('text-anchor','middle'); label.setAttribute('class','axis-label');
    label.textContent = f;
    svg.appendChild(label);

    const vlabel = document.createElementNS('http://www.w3.org/2000/svg','text');
    vlabel.setAttribute('x', x+bw/2); vlabel.setAttribute('y', y-6);
    vlabel.setAttribute('text-anchor','middle'); vlabel.setAttribute('class','axis-label');
    vlabel.setAttribute('fill', COLORS[f]);
    vlabel.textContent = '₹'+val.toFixed(1);
    svg.appendChild(vlabel);
  });
})();

/* ---------- DOUGHNUT ---------- */
(function(){
  const svg = document.getElementById('doughChart');
  const tip = document.getElementById('doughTooltip');
  const legend = document.getElementById('doughLegend');
  const fuels = Object.keys(co2PerKm);
  const total = Object.values(co2PerKm).reduce((a,b)=>a+b,0);
  const cx=100, cy=115, r=70, sw=26;
  const circumference = 2*Math.PI*r;
  let offsetAcc = 0;

  fuels.forEach(f=>{
    const val = co2PerKm[f];
    const frac = val/total;
    const dash = frac*circumference;
    const circle = document.createElementNS('http://www.w3.org/2000/svg','circle');
    circle.setAttribute('cx',cx); circle.setAttribute('cy',cy); circle.setAttribute('r',r);
    circle.setAttribute('fill','none');
    circle.setAttribute('stroke',COLORS[f]);
    circle.setAttribute('stroke-width',sw);
    circle.setAttribute('stroke-dasharray', dash+' '+(circumference-dash));
    circle.setAttribute('stroke-dashoffset', -offsetAcc);
    circle.setAttribute('transform','rotate(-90 '+cx+' '+cy+')');
    circle.setAttribute('class','doughseg');
    circle.addEventListener('mousemove',(e)=>{
      tip.style.opacity=1;
      tip.style.left=(e.offsetX+12)+'px';
      tip.style.top=(e.offsetY-10)+'px';
      tip.textContent = f+': '+val.toFixed(2)+' kg/km ('+(frac*100).toFixed(0)+'%)';
    });
    circle.addEventListener('mouseleave',()=>tip.style.opacity=0);
    svg.appendChild(circle);
    offsetAcc += dash;

    const sw2 = document.createElement('span');
    sw2.innerHTML = '<span class="swatch" style="background:'+COLORS[f]+'"></span>'+f;
    legend.appendChild(sw2);
  });

  const center = document.createElementNS('http://www.w3.org/2000/svg','text');
  center.setAttribute('x',cx); center.setAttribute('y',cy+4);
  center.setAttribute('text-anchor','middle');
  center.setAttribute('fill','#e8ecf5');
  center.setAttribute('font-size','13');
  center.setAttribute('font-family','Consolas,Monaco,monospace');
  center.textContent = 'CO₂/km';
  svg.appendChild(center);
})();

/* ---------- LINE CHART ---------- */
(function(){
  const svg = document.getElementById('lineChart');
  const tip = document.getElementById('lineTooltip');
  const legend = document.getElementById('lineLegend');
  const left=50, top=15, w=820, h=200, maxAge=12;
  const maxCost = 10.5;

  // axes
  for(let g=0; g<=maxCost; g+=2){
    const y = top+h - (g/maxCost)*h;
    const line = document.createElementNS('http://www.w3.org/2000/svg','line');
    line.setAttribute('x1',left); line.setAttribute('x2',left+w);
    line.setAttribute('y1',y); line.setAttribute('y2',y);
    line.setAttribute('stroke','rgba(255,255,255,0.06)');
    svg.appendChild(line);
    const lbl = document.createElementNS('http://www.w3.org/2000/svg','text');
    lbl.setAttribute('x',left-8); lbl.setAttribute('y',y+3);
    lbl.setAttribute('text-anchor','end'); lbl.setAttribute('class','axis-label');
    lbl.textContent = '₹'+g;
    svg.appendChild(lbl);
  }
  for(let a=0;a<=maxAge;a+=2){
    const x = left + (a/maxAge)*w;
    const lbl = document.createElementNS('http://www.w3.org/2000/svg','text');
    lbl.setAttribute('x',x); lbl.setAttribute('y',top+h+18);
    lbl.setAttribute('text-anchor','middle'); lbl.setAttribute('class','axis-label');
    lbl.textContent = a+'y';
    svg.appendChild(lbl);
  }

  // car age marker
  const ax = left + (CAR_AGE/maxAge)*w;
  const vline = document.createElementNS('http://www.w3.org/2000/svg','line');
  vline.setAttribute('x1',ax); vline.setAttribute('x2',ax);
  vline.setAttribute('y1',top); vline.setAttribute('y2',top+h);
  vline.setAttribute('stroke','#f5a623'); vline.setAttribute('stroke-dasharray','4 4');
  vline.setAttribute('stroke-width',1.5);
  svg.appendChild(vline);
  const vlabel = document.createElementNS('http://www.w3.org/2000/svg','text');
  vlabel.setAttribute('x',ax+5); vlabel.setAttribute('y',top+12);
  vlabel.setAttribute('fill','#f5a623'); vlabel.setAttribute('font-size','10');
  vlabel.setAttribute('font-family','Consolas,Monaco,monospace');
  vlabel.textContent = 'your car (3y)';
  svg.appendChild(vlabel);

  Object.keys(lineBase).forEach(f=>{
    const [base,k] = lineBase[f];
    let d = '';
    const pts = [];
    for(let a=0;a<=maxAge;a+=1){
      const cost = base + k*a;
      const x = left + (a/maxAge)*w;
      const y = top+h - (cost/maxCost)*h;
      pts.push([x,y,a,cost]);
      d += (a===0?'M':'L')+x+' '+y+' ';
    }
    const path = document.createElementNS('http://www.w3.org/2000/svg','path');
    path.setAttribute('d',d);
    path.setAttribute('fill','none');
    path.setAttribute('stroke',COLORS[f]);
    path.setAttribute('stroke-width',2.4);
    path.setAttribute('class','lineage');
    svg.appendChild(path);

    pts.forEach(([x,y,a,cost])=>{
      const dot = document.createElementNS('http://www.w3.org/2000/svg','circle');
      dot.setAttribute('cx',x); dot.setAttribute('cy',y); dot.setAttribute('r',3);
      dot.setAttribute('fill',COLORS[f]); dot.setAttribute('opacity',0);
      dot.addEventListener('mousemove',(e)=>{
        tip.style.opacity=1;
        tip.style.left=(e.offsetX+12)+'px';
        tip.style.top=(e.offsetY-10)+'px';
        tip.textContent = f+' @ '+a+'y: ₹'+cost.toFixed(2)+'/km';
      });
      dot.addEventListener('mouseenter',()=>dot.setAttribute('opacity',1));
      dot.addEventListener('mouseleave',()=>{dot.setAttribute('opacity',0);tip.style.opacity=0;});
      svg.appendChild(dot);
    });

    const sw = document.createElement('span');
    sw.innerHTML = '<span class="swatch" style="background:'+COLORS[f]+'"></span>'+f;
    legend.appendChild(sw);
  });
})();

/* ---------- GAUGE ---------- */
(function(){
  const svg = document.getElementById('gaugeSvg');
  const score = 7.2, maxScore = 10;
  const cx=140, cy=140, r=100;
  const circumference = Math.PI*r; // semicircle

  const bgArc = document.createElementNS('http://www.w3.org/2000/svg','path');
  bgArc.setAttribute('d', describeArc(cx,cy,r,-180,0));
  bgArc.setAttribute('fill','none');
  bgArc.setAttribute('stroke','rgba(255,255,255,0.08)');
  bgArc.setAttribute('stroke-width',16);
  bgArc.setAttribute('stroke-linecap','round');
  svg.appendChild(bgArc);

  const arc = document.createElementNS('http://www.w3.org/2000/svg','path');
  arc.setAttribute('id','gaugeArc');
  arc.setAttribute('d', describeArc(cx,cy,r,-180,0));
  arc.setAttribute('fill','none');
  arc.setAttribute('stroke','#f5a623');
  arc.setAttribute('stroke-width',16);
  arc.setAttribute('stroke-linecap','round');
  arc.setAttribute('stroke-dasharray', circumference);
  arc.setAttribute('stroke-dashoffset', circumference);
  svg.appendChild(arc);

  const scoreText = document.createElementNS('http://www.w3.org/2000/svg','text');
  scoreText.setAttribute('x',cx); scoreText.setAttribute('y',cy-6);
  scoreText.setAttribute('text-anchor','middle');
  scoreText.setAttribute('class','gauge-score');
  scoreText.textContent = score.toFixed(1);
  svg.appendChild(scoreText);

  const maxText = document.createElementNS('http://www.w3.org/2000/svg','text');
  maxText.setAttribute('x',cx); maxText.setAttribute('y',cy+14);
  maxText.setAttribute('text-anchor','middle');
  maxText.setAttribute('class','gauge-max');
  maxText.textContent = 'out of 10';
  svg.appendChild(maxText);

  setTimeout(()=>{
    const offset = circumference * (1 - score/maxScore);
    arc.style.strokeDashoffset = offset;
  }, 150);

  function describeArc(cx,cy,r,startAngle,endAngle){
    const start = polarToCartesian(cx,cy,r,endAngle);
    const end = polarToCartesian(cx,cy,r,startAngle);
    return 'M '+start.x+' '+start.y+' A '+r+' '+r+' 0 0 0 '+end.x+' '+end.y;
  }
  function polarToCartesian(cx,cy,r,angleDeg){
    const a = (angleDeg-90)*Math.PI/180;
    return { x: cx + r*Math.cos(a), y: cy + r*Math.sin(a) };
  }
})();

/* ---------- FUEL CARDS ---------- */
(function(){
  const data = [
    { fuel:'Petrol', highlight:true, pros:['Widely available nationwide','Higher resale value'],
      cons:['Higher CO₂ than CNG/EV','Costlier per km than CNG'], best:'Mixed city + highway driving' },
    { fuel:'Diesel', pros:['Better highway mileage','Lower cost/km than petrol'],
      cons:['Higher maintenance cost','Higher NOx emissions'], best:'High-distance highway or fleet use' },
    { fuel:'CNG', pros:['Lowest running cost among ICE','Lower emissions than petrol/diesel'],
      cons:['Limited refuelling stations','Reduced boot space'], best:'High-mileage city driving on a budget' },
    { fuel:'E85', pros:['Lower CO₂ than petrol','Renewable-blend fuel'],
      cons:['Lower mileage raises cost/km','Needs flex-fuel engine'], best:'Eco-conscious flex-fuel owners' },
    { fuel:'EV', pros:['Lowest cost/km by far','Minimal maintenance'],
      cons:['Long recharge time','Charging infra still growing'], best:'Daily city commute with home charging' },
  ];
  const grid = document.getElementById('fuelCards');
  data.forEach(d=>{
    const card = document.createElement('div');
    card.className = 'glass fuel-card' + (d.highlight?' highlight':'');
    card.style.borderTopColor = COLORS[d.fuel];
    card.innerHTML = `
      <h3><span class="dot" style="background:${COLORS[d.fuel]}"></span>${d.fuel}</h3>
      <ul>
        ${d.pros.map(p=>`<li>✅ ${p}</li>`).join('')}
        ${d.cons.map(c=>`<li>❌ ${c}</li>`).join('')}
      </ul>
      <div class="bestfor">🚗 ${d.best}</div>
    `;
    grid.appendChild(card);
  });
})();
</script>
</body>
</html>
