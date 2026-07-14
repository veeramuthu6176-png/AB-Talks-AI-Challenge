<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Operation Lifeline: Supply Chain Crisis Lab</title>
<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<style>
  :root{
    --bg:#0a0e17;
    --bg2:#0d1220;
    --panel:#131a2b;
    --panel2:#1a2338;
    --panel3:#202b45;
    --border:#26314d;
    --text:#e7ecf7;
    --text-dim:#9aa7c2;
    --text-faint:#6b7793;
    --accent:#3ba7ff;
    --accent2:#22d3ee;
    --purple:#a78bfa;
    --good:#34d399;
    --warn:#fbbf24;
    --bad:#f87171;
    --radius:18px;
    --radius-sm:12px;
    font-size:16px;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(1200px 600px at 10% -10%, #142033 0%, transparent 60%),
      radial-gradient(1000px 500px at 100% 0%, #10233a 0%, transparent 55%),
      linear-gradient(180deg, var(--bg) 0%, var(--bg2) 100%);
    color:var(--text);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Inter, Helvetica, Arial, sans-serif;
    min-height:100vh;
  }
  #root{min-height:100vh;}
  .app-shell{max-width:1100px;margin:0 auto;padding:24px 20px 80px;}
  .fade-in{animation:fadeIn .5s ease both;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(10px);}to{opacity:1;transform:translateY(0);}}
  .slide-up{animation:slideUp .45s ease both;}
  @keyframes slideUp{from{opacity:0;transform:translateY(18px);}to{opacity:1;transform:translateY(0);}}

  /* Header / Stepper */
  .top-bar{display:flex;align-items:center;justify-content:space-between;padding:6px 2px 22px;flex-wrap:wrap;gap:12px;}
  .brand{display:flex;align-items:center;gap:10px;}
  .brand-badge{
    width:38px;height:38px;border-radius:11px;
    background:linear-gradient(135deg,var(--accent),var(--purple));
    display:flex;align-items:center;justify-content:center;font-size:19px;
    box-shadow:0 4px 18px rgba(59,167,255,.35);
  }
  .brand-name{font-weight:700;font-size:16px;letter-spacing:.2px;}
  .brand-sub{font-size:11.5px;color:var(--text-faint);}
  .stepper{display:flex;gap:6px;align-items:center;}
  .step-dot{
    width:9px;height:9px;border-radius:50%;background:var(--panel3);transition:.3s;
  }
  .step-dot.active{background:var(--accent);width:22px;border-radius:6px;}
  .step-dot.done{background:var(--good);}

  /* Cards */
  .card{
    background:linear-gradient(180deg,var(--panel) 0%, var(--panel2) 100%);
    border:1px solid var(--border);
    border-radius:var(--radius);
    padding:22px;
    transition:transform .25s ease, box-shadow .25s ease, border-color .25s ease;
  }
  .card:hover{transform:translateY(-3px);box-shadow:0 14px 34px rgba(0,0,0,.35);border-color:#31406a;}
  .grid{display:grid;gap:16px;}
  .grid-2{grid-template-columns:repeat(2,1fr);}
  .grid-3{grid-template-columns:repeat(3,1fr);}
  .grid-4{grid-template-columns:repeat(4,1fr);}
  @media (max-width:820px){
    .grid-2,.grid-3,.grid-4{grid-template-columns:1fr 1fr;}
  }
  @media (max-width:560px){
    .grid-2,.grid-3,.grid-4{grid-template-columns:1fr;}
  }

  .stat-card{padding:16px 18px;}
  .stat-label{font-size:11.5px;text-transform:uppercase;letter-spacing:.6px;color:var(--text-faint);margin-bottom:6px;}
  .stat-value{font-size:22px;font-weight:700;}
  .stat-icon{font-size:20px;margin-bottom:8px;display:block;}

  h1,h2,h3,h4{margin:0;font-weight:700;letter-spacing:-.2px;}
  p{margin:0;line-height:1.55;}
  .muted{color:var(--text-dim);}
  .faint{color:var(--text-faint);}

  /* Buttons */
  .btn{
    border:none;cursor:pointer;font-weight:600;font-size:14.5px;
    padding:13px 26px;border-radius:12px;transition:.2s ease;
    font-family:inherit;
  }
  .btn-primary{
    background:linear-gradient(135deg,var(--accent),#2b8ce6);
    color:#fff;box-shadow:0 8px 22px rgba(59,167,255,.3);
  }
  .btn-primary:hover{transform:translateY(-2px);box-shadow:0 12px 28px rgba(59,167,255,.42);}
  .btn-primary:disabled{opacity:.35;cursor:not-allowed;transform:none;box-shadow:none;}
  .btn-secondary{
    background:var(--panel3);color:var(--text);border:1px solid var(--border);
  }
  .btn-secondary:hover{background:#28345,4;border-color:#3a4a72;transform:translateY(-2px);}
  .btn-ghost{background:transparent;color:var(--text-dim);border:1px solid var(--border);}
  .btn-ghost:hover{color:var(--text);border-color:var(--accent);}
  .btn-row{display:flex;gap:12px;justify-content:flex-end;margin-top:22px;flex-wrap:wrap;}
  .btn-row.center{justify-content:center;}
  .btn-row.between{justify-content:space-between;}

  /* Welcome */
  .welcome-wrap{
    min-height:80vh;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;gap:22px;padding:40px 10px;
  }
  .welcome-badge{
    width:78px;height:78px;border-radius:22px;
    background:linear-gradient(135deg,var(--accent),var(--purple));
    display:flex;align-items:center;justify-content:center;font-size:36px;
    box-shadow:0 14px 40px rgba(59,167,255,.4);
  }
  .welcome-title{font-size:clamp(28px,5vw,44px);font-weight:800;letter-spacing:-1px;
    background:linear-gradient(135deg,#fff,#aecdfb);-webkit-background-clip:text;background-clip:text;color:transparent;
  }
  .welcome-sub{font-size:16px;color:var(--text-dim);max-width:560px;}
  .welcome-features{display:flex;gap:14px;flex-wrap:wrap;justify-content:center;margin-top:10px;}
  .feature-pill{
    background:var(--panel2);border:1px solid var(--border);border-radius:999px;
    padding:8px 16px;font-size:13px;color:var(--text-dim);
  }

  /* Info box */
  .info-box{
    background:linear-gradient(135deg, rgba(59,167,255,.09), rgba(167,139,250,.06));
    border:1px solid rgba(59,167,255,.28);
    border-radius:var(--radius-sm);
    padding:16px 18px;
    margin-bottom:20px;
    display:flex;gap:12px;align-items:flex-start;
  }
  .info-box .info-icon{font-size:20px;line-height:1;}
  .info-box .info-title{font-size:12.5px;text-transform:uppercase;letter-spacing:.6px;color:var(--accent2);font-weight:700;margin-bottom:4px;}
  .info-box p{font-size:14px;color:var(--text-dim);}

  .section-head{margin-bottom:18px;}
  .section-eyebrow{font-size:12px;text-transform:uppercase;letter-spacing:1px;color:var(--accent2);font-weight:700;margin-bottom:6px;}
  .section-title{font-size:26px;margin-bottom:6px;}
  .section-desc{color:var(--text-dim);font-size:14.5px;max-width:680px;}

  /* Metric bars */
  .metric-row{margin-bottom:14px;}
  .metric-label-row{display:flex;justify-content:space-between;font-size:13px;margin-bottom:6px;color:var(--text-dim);}
  .metric-label-row b{color:var(--text);font-weight:600;}
  .metric-track{height:10px;border-radius:999px;background:var(--panel3);overflow:hidden;border:1px solid var(--border);}
  .metric-fill{height:100%;border-radius:999px;transition:width 1s cubic-bezier(.22,.9,.32,1);}

  /* Crisis card */
  .crisis-card{
    background:linear-gradient(135deg, rgba(248,113,113,.1), rgba(251,191,36,.05));
    border:1px solid rgba(248,113,113,.35);
  }
  .urgency-tag{
    display:inline-block;padding:5px 12px;border-radius:999px;font-size:11.5px;font-weight:700;letter-spacing:.4px;text-transform:uppercase;
  }
  .urgency-Medium{background:rgba(251,191,36,.16);color:var(--warn);border:1px solid rgba(251,191,36,.4);}
  .urgency-High{background:rgba(248,113,113,.16);color:#fb9292;border:1px solid rgba(248,113,113,.4);}
  .urgency-Critical{background:rgba(248,113,113,.25);color:#ff8686;border:1px solid rgba(248,113,113,.6);}

  /* Option cards (selectable) */
  .option-card{
    background:var(--panel2);border:1.5px solid var(--border);border-radius:14px;
    padding:16px 18px;cursor:pointer;transition:.2s ease;position:relative;
  }
  .option-card:hover{border-color:#3f5padding;border-color:var(--accent);transform:translateY(-2px);}
  .option-card.selected{border-color:var(--accent);background:linear-gradient(135deg, rgba(59,167,255,.14), rgba(167,139,250,.08));box-shadow:0 8px 24px rgba(59,167,255,.18);}
  .option-card.disabled{opacity:.4;cursor:not-allowed;}
  .option-check{
    position:absolute;top:14px;right:14px;width:22px;height:22px;border-radius:7px;
    border:1.5px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:13px;color:transparent;transition:.2s;
  }
  .option-card.selected .option-check{background:var(--accent);border-color:var(--accent);color:#fff;}
  .option-title{font-weight:700;font-size:15px;margin-bottom:6px;padding-right:26px;}
  .option-desc{font-size:13px;color:var(--text-dim);line-height:1.5;}
  .option-tags{display:flex;gap:6px;margin-top:10px;flex-wrap:wrap;}
  .tag{font-size:10.5px;padding:3px 8px;border-radius:999px;font-weight:700;letter-spacing:.3px;}
  .tag-up{background:rgba(52,211,153,.14);color:var(--good);}
  .tag-down{background:rgba(248,113,113,.14);color:var(--bad);}

  .explain-box{
    margin-top:16px;padding:14px 16px;border-radius:12px;
    background:var(--panel3);border-left:3px solid var(--accent2);
    font-size:13.5px;color:var(--text-dim);
  }

  .progress-line{font-size:13px;color:var(--text-faint);margin-bottom:8px;}

  /* Dashboard */
  .score-hero{
    text-align:center;padding:36px 20px;border-radius:22px;
    background:radial-gradient(600px 260px at 50% -10%, rgba(59,167,255,.18), transparent 60%),
      linear-gradient(180deg,var(--panel) 0%, var(--panel2) 100%);
    border:1px solid var(--border);margin-bottom:26px;
  }
  .score-ring-wrap{display:flex;justify-content:center;margin-bottom:14px;}
  .score-num{font-size:64px;font-weight:800;letter-spacing:-2px;}
  .score-tier{font-size:14px;font-weight:700;letter-spacing:.5px;text-transform:uppercase;margin-top:4px;}
  .feedback-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-top:22px;}
  @media (max-width:700px){.feedback-grid{grid-template-columns:1fr;}}
  .feedback-card{padding:18px 20px;border-radius:14px;}
  .feedback-card h4{font-size:13px;text-transform:uppercase;letter-spacing:.5px;margin-bottom:10px;display:flex;align-items:center;gap:8px;}
  .feedback-card p{font-size:14px;color:var(--text-dim);}
  .fc-best{background:rgba(52,211,153,.08);border:1px solid rgba(52,211,153,.3);}
  .fc-best h4{color:var(--good);}
  .fc-mistake{background:rgba(248,113,113,.08);border:1px solid rgba(248,113,113,.3);}
  .fc-mistake h4{color:var(--bad);}
  .fc-rec{background:rgba(59,167,255,.08);border:1px solid rgba(59,167,255,.3);}
  .fc-rec h4{color:var(--accent2);}
  .fc-lessons{background:var(--panel2);border:1px solid var(--border);}
  .fc-lessons h4{color:var(--purple);}
  .fc-lessons ul{margin:0;padding-left:18px;}
  .fc-lessons li{font-size:14px;color:var(--text-dim);margin-bottom:8px;line-height:1.5;}

  .company-name{font-size:15px;font-weight:700;}
  .country-chip{
    display:inline-block;background:var(--panel3);border:1px solid var(--border);
    padding:4px 10px;border-radius:999px;font-size:12px;color:var(--text-dim);margin:2px 4px 2px 0;
  }

  ::-webkit-scrollbar{width:9px;}
  ::-webkit-scrollbar-track{background:var(--bg);}
  ::-webkit-scrollbar-thumb{background:var(--panel3);border-radius:8px;}
</style>
</head>
<body>
<div id="root"></div>

<script type="text/babel" data-presets="react,env">
const { useState, useEffect, useMemo } = React;

/* ---------------------------------- helpers ---------------------------------- */
function randInt(min, max){ return Math.floor(Math.random() * (max - min + 1)) + min; }
function randChoice(arr){ return arr[randInt(0, arr.length - 1)]; }
function randSample(arr, n){
  const copy = [...arr];
  const out = [];
  n = Math.min(n, copy.length);
  for(let i=0;i<n;i++){
    const idx = randInt(0, copy.length-1);
    out.push(copy[idx]);
    copy.splice(idx,1);
  }
  return out;
}
function clamp(v){ return Math.max(0, Math.min(100, Math.round(v))); }

/* ---------------------------------- data ---------------------------------- */
const INDUSTRIES = ["Automotive Parts","Consumer Electronics","Pharmaceuticals","Fashion Retail","Food & Beverage","Industrial Machinery","Aerospace Components","Home Appliances","Medical Devices","Semiconductor Manufacturing"];
const ADJECTIVES = ["Apex","Nova","Summit","Vertex","Horizon","Pinnacle","Cascade","Meridian","Zenith","Atlas","Vanguard","Sterling","Ironclad","Nimbus"];
const NOUNS = ["Dynamics","Industries","Global","Systems","Manufacturing","Holdings","Logistics","Group","Works","Solutions"];
const COUNTRIES = ["Vietnam","Germany","Mexico","China","India","Poland","Brazil","South Korea","Thailand","USA","Taiwan","Malaysia","Turkey","Indonesia","Czech Republic"];

function generateCompany(){
  const industry = randChoice(INDUSTRIES);
  const name = `${randChoice(ADJECTIVES)} ${randChoice(NOUNS)}`;
  const revenue = randInt(80, 950);
  const factories = randInt(2, 14);
  const warehouses = randInt(3, 22);
  const suppliers = randInt(6, 45);
  const inventoryDays = randInt(12, 55);
  const leadTime = randInt(6, 40);
  const countries = randSample(COUNTRIES, randInt(3,7));
  return { name, industry, revenue, factories, warehouses, suppliers, inventoryDays, leadTime, countries };
}

const CRISES = [
  { type:"Factory Fire", icon:"🔥", template:(c)=>`A fire has broken out at one of ${c.name}'s primary manufacturing plants, halting production lines indefinitely.` },
  { type:"Supplier Bankruptcy", icon:"⚠️", template:(c)=>`A key raw-material supplier to ${c.name} has filed for bankruptcy, threatening ${c.industry.toLowerCase()} component flow.` },
  { type:"Port Strike", icon:"🚢", template:(c)=>`Dockworkers at major ports serving ${c.countries[0]} have gone on strike, stranding ${c.name}'s inbound and outbound shipments.` },
  { type:"Cyberattack", icon:"💻", template:(c)=>`${c.name}'s enterprise resource planning system was hit by a ransomware attack, disrupting order processing and warehouse visibility.` },
  { type:"Flood", icon:"🌊", template:(c)=>`Severe flooding near ${c.name}'s distribution hub in ${c.countries[Math.min(1,c.countries.length-1)]} has damaged inventory and blocked transport routes.` },
  { type:"Raw Material Shortage", icon:"📉", template:(c)=>`A sudden global shortage of critical raw materials is threatening ${c.name}'s production schedule.` },
  { type:"Political Conflict", icon:"🌍", template:(c)=>`Escalating political tensions in ${c.countries[0]} have triggered new export restrictions affecting ${c.name}'s supply routes.` },
  { type:"Shipping Delay", icon:"🕒", template:(c)=>`Widespread container shortages and vessel congestion have pushed ${c.name}'s average shipping times far beyond normal.` }
];

function generateCrisis(company){
  const base = randChoice(CRISES);
  const urgency = randChoice(["Medium","High","Critical"]);
  const impactPct = randInt(15,45);
  const metricWord = randChoice(["output","fulfillment capacity","on-time delivery rate","revenue run-rate"]);
  return {
    type: base.type,
    icon: base.icon,
    description: base.template(company),
    urgency,
    impact: `Analysts project a ${impactPct}% disruption to ${company.name}'s ${metricWord} if unaddressed within the next planning cycle.`
  };
}

const WAR_ROOM_ACTIONS = [
  { id:"air", title:"Expedite Air Freight", icon:"✈️",
    desc:"Fly critical inventory in instead of waiting on ocean or ground transport.",
    why:"Speeds up delivery dramatically but is very costly and slightly hurts margins.",
    effects:{cost:-15, inventory:10, profit:-8, delivery:18, satisfaction:6} },
  { id:"backup", title:"Activate Backup Supplier Network", icon:"🔗",
    desc:"Route orders through pre-vetted secondary suppliers.",
    why:"Improves inventory and speed with only a moderate cost hit — a strong all-around move.",
    effects:{cost:-5, inventory:15, profit:-3, delivery:8, satisfaction:4} },
  { id:"cut", title:"Cut Non-Essential Production", icon:"✂️",
    desc:"Pause lower-priority product lines to conserve resources.",
    why:"Saves money immediately but reduces inventory, slows delivery, and dents customer trust.",
    effects:{cost:12, inventory:-8, profit:-10, delivery:-2, satisfaction:-3} },
  { id:"price", title:"Dynamic Pricing Adjustment", icon:"💲",
    desc:"Raise prices on constrained items to protect margins.",
    why:"Boosts profit significantly but customers notice — satisfaction takes a real hit.",
    effects:{cost:2, inventory:-2, profit:14, delivery:0, satisfaction:-12} },
  { id:"comms", title:"Proactive Customer Communication", icon:"📣",
    desc:"Get ahead of delays with transparent, frequent updates.",
    why:"Cheap and highly effective for satisfaction, though it doesn't fix the underlying problem.",
    effects:{cost:-3, inventory:0, profit:-1, delivery:2, satisfaction:16} },
  { id:"cash", title:"Emergency Financing Drawdown", icon:"🏦",
    desc:"Tap credit lines or reserves to buy flexibility across the board.",
    why:"Buys you inventory and speed, but interest and fees eat directly into profit.",
    effects:{cost:-10, inventory:6, profit:-15, delivery:5, satisfaction:3} }
];

const NEGOTIATION_ROUNDS = [
  {
    title:"Round 1 · Opening Position",
    scenario:(c,cr)=>`Your lead supplier for ${c.industry.toLowerCase()} components knows you're under pressure from the ${cr.type.toLowerCase()}. How do you open the negotiation?`,
    options:[
      { label:"Push hard for an aggressive price cut", effects:{trust:-15,price:20,leadTime:-5},
        note:"Aggressive opening tactics can win a lower price fast, but they burn trust — and trust is what gets you flexibility later when you need it most." },
      { label:"Propose a collaborative, long-term partnership", effects:{trust:15,price:5,leadTime:5},
        note:"Leading with partnership signals builds trust that often pays bigger dividends than a one-time discount." },
      { label:"Present firm, data-backed terms", effects:{trust:5,price:12,leadTime:0},
        note:"A balanced, evidence-based approach protects the relationship while still moving price in your favor." }
    ]
  },
  {
    title:"Round 2 · Supplier Pushback",
    scenario:()=>`The supplier pushes back, citing their own rising costs. How do you respond?`,
    options:[
      { label:"Threaten to switch suppliers immediately", effects:{trust:-20,price:15,leadTime:-10},
        note:"Threats can work once, but they make suppliers deprioritize you the next time supply gets tight." },
      { label:"Offer a longer-term contract for a discount", effects:{trust:10,price:15,leadTime:8},
        note:"Trading future volume commitment for near-term relief is one of the most reliable levers in supply negotiations." },
      { label:"Meet in the middle on price and timeline", effects:{trust:5,price:8,leadTime:3},
        note:"Compromise keeps momentum but rarely produces a standout outcome." }
    ]
  },
  {
    title:"Round 3 · Lead Time Crunch",
    scenario:()=>`With inventory tight, lead time becomes the sticking point. What's your move?`,
    options:[
      { label:"Demand expedited shipping at the supplier's expense", effects:{trust:-12,price:-5,leadTime:18},
        note:"You get speed, but pushing costs onto the supplier erodes goodwill and can raise future prices." },
      { label:"Propose a shared investment in faster logistics", effects:{trust:12,price:-3,leadTime:12},
        note:"Sharing the cost of a solution builds trust while still improving speed — a genuine win-win." },
      { label:"Accept current lead times, focus on price instead", effects:{trust:3,price:10,leadTime:-2},
        note:"Sometimes the smartest move is picking your battle — you gain price at the cost of speed." }
    ]
  },
  {
    title:"Round 4 · Final Terms",
    scenario:()=>`Time to close. How do you finalize the agreement?`,
    options:[
      { label:"Push for maximum concessions before signing", effects:{trust:-15,price:18,leadTime:5},
        note:"Squeezing every last concession can look great on paper but often creates resentment that surfaces later." },
      { label:"Close with fair, win-win terms", effects:{trust:15,price:10,leadTime:10},
        note:"Deals that feel fair to both sides tend to be the most durable ones." },
      { label:"Accept the supplier's standard terms to close quickly", effects:{trust:8,price:2,leadTime:2},
        note:"Speed has value, but you leave negotiating room on the table." }
    ]
  }
];

const BOARDROOM_QUESTIONS = [
  {
    q:"Your factory has just gone offline because of the crisis. What's your first move as CEO?",
    options:[
      { label:"Publicly reassure investors before assessing the damage", points:2, note:"Speaking before you have facts risks your credibility if the situation changes." },
      { label:"Assemble a crisis response team and assess operational impact immediately", points:15, note:"Getting accurate facts first is the foundation of every good crisis decision that follows." },
      { label:"Wait for a full report before taking any action", points:8, note:"Patience has value, but waiting too long can let a fixable problem spiral." }
    ]
  },
  {
    q:"A key supplier says they can't meet 30% of your order volume. How do you respond?",
    options:[
      { label:"Terminate the contract immediately", points:2, note:"Cutting ties fast can leave you with zero supply while you scramble for a replacement." },
      { label:"Open a dialogue to understand the root cause and negotiate partial fulfillment", points:15, note:"Understanding the 'why' often reveals options — like partial shipments — that a snap decision would miss." },
      { label:"Absorb the loss silently and hope it resolves itself", points:8, note:"Hope isn't a strategy, but avoiding panic does keep the team calm while you gather options." }
    ]
  },
  {
    q:"Customer complaints are rising because of delivery delays. What do you prioritize?",
    options:[
      { label:"Transparent communication with realistic timelines", points:15, note:"Customers forgive delays far more easily than they forgive silence or broken promises." },
      { label:"Offer blanket discounts to every customer", points:8, note:"Discounts feel generous but are expensive and don't fix the underlying trust issue." },
      { label:"Ignore the complaints until the crisis passes", points:2, note:"Silence during a crisis is one of the fastest ways to lose customer loyalty." }
    ]
  },
  {
    q:"Your leadership team is split on investing in automation now versus waiting. What do you decide?",
    options:[
      { label:"Invest cautiously in high-impact, low-risk automation now", points:15, note:"Crises often reveal exactly where automation would help most — targeted investment now compounds over time." },
      { label:"Wait until the crisis fully resolves", points:8, note:"Waiting is safe, but you delay the benefits and may miss the moment when change is easiest to justify." },
      { label:"Go all-in on automation immediately regardless of cash flow", points:2, note:"Overextending financially during a crisis can turn one problem into two." }
    ]
  },
  {
    q:"The board asks for your long-term resilience strategy. What do you present?",
    options:[
      { label:"A diversified supply chain with redundancy and real-time visibility", points:15, note:"Diversification and visibility are the two things that consistently reduce the cost of the next crisis." },
      { label:"A plan to return to pre-crisis operations exactly as before", points:2, note:"Rebuilding the same fragile system just resets the clock until the next disruption." },
      { label:"A cost-cutting-only strategy", points:8, note:"Cost discipline matters, but resilience requires investment, not just cuts." }
    ]
  }
];

const AI_INVESTMENTS = [
  { id:"forecast", name:"Demand Forecasting", icon:"📈",
    desc:"Uses historical and real-time data to predict customer demand more accurately.",
    effects:{resilience:10, costControl:10, riskManagement:5, customerSatisfaction:10} },
  { id:"inventory", name:"Inventory Optimization", icon:"📦",
    desc:"Automatically balances stock levels across warehouses to avoid shortages and overstock.",
    effects:{resilience:15, costControl:12, riskManagement:5, customerSatisfaction:3} },
  { id:"risk", name:"Supplier Risk Monitoring", icon:"🛰️",
    desc:"Continuously scans supplier financial health, geopolitical risk, and news signals.",
    effects:{resilience:10, costControl:3, riskManagement:18, customerSatisfaction:2} },
  { id:"vision", name:"Warehouse Vision", icon:"👁️",
    desc:"Computer vision that tracks inventory accuracy and automates warehouse quality checks.",
    effects:{resilience:8, costControl:14, riskManagement:4, customerSatisfaction:6} },
  { id:"copilot", name:"Procurement Copilot", icon:"🤝",
    desc:"An AI assistant that drafts contracts, benchmarks pricing, and flags risky supplier terms.",
    effects:{resilience:6, costControl:15, riskManagement:12, customerSatisfaction:2} }
];

const METRIC_META = {
  cost:{ label:"Cost Efficiency", color:"linear-gradient(90deg,#22d3ee,#3ba7ff)" },
  inventory:{ label:"Inventory Health", color:"linear-gradient(90deg,#34d399,#22d3ee)" },
  profit:{ label:"Profit Margin", color:"linear-gradient(90deg,#fbbf24,#f59e0b)" },
  delivery:{ label:"Delivery Speed", color:"linear-gradient(90deg,#a78bfa,#3ba7ff)" },
  satisfaction:{ label:"Customer Satisfaction", color:"linear-gradient(90deg,#f472b6,#a78bfa)" }
};

const STEP_ORDER = ["company","crisis","warroom","negotiation","boardroom","ai","dashboard"];
const STEP_LABELS = {
  company:"Company", crisis:"Crisis", warroom:"War Room", negotiation:"Negotiation",
  boardroom:"Boardroom", ai:"AI Strategy", dashboard:"Results"
};

/* ---------------------------------- small components ---------------------------------- */
function MetricBar({ metricKey, value }){
  const meta = METRIC_META[metricKey];
  return (
    <div className="metric-row">
      <div className="metric-label-row"><span>{meta.label}</span><b>{Math.round(value)}</b></div>
      <div className="metric-track">
        <div className="metric-fill" style={{ width: `${clamp(value)}%`, background: meta.color }}></div>
      </div>
    </div>
  );
}

function ScoreBar({ label, value, color }){
  return (
    <div className="metric-row">
      <div className="metric-label-row"><span>{label}</span><b>{Math.round(value)}/100</b></div>
      <div className="metric-track">
        <div className="metric-fill" style={{ width: `${clamp(value)}%`, background: color }}></div>
      </div>
    </div>
  );
}

function InfoBox({ title, children }){
  return (
    <div className="info-box">
      <div className="info-icon">💡</div>
      <div>
        <div className="info-title">{title}</div>
        <p>{children}</p>
      </div>
    </div>
  );
}

function TopBar({ screen }){
  if(screen === "welcome") return null;
  const idx = STEP_ORDER.indexOf(screen);
  return (
    <div className="top-bar">
      <div className="brand">
        <div className="brand-badge">🛡️</div>
        <div>
          <div className="brand-name">Operation Lifeline</div>
          <div className="brand-sub">Supply Chain Crisis Lab</div>
        </div>
      </div>
      <div className="stepper">
        {STEP_ORDER.map((s,i)=>(
          <div key={s} className={"step-dot " + (i===idx ? "active" : i<idx ? "done" : "")} title={STEP_LABELS[s]}></div>
        ))}
      </div>
    </div>
  );
}

/* ---------------------------------- screens ---------------------------------- */
function WelcomeScreen({ onStart }){
  return (
    <div className="welcome-wrap fade-in">
      <div className="welcome-badge">🛡️</div>
      <div className="welcome-title">Operation Lifeline</div>
      <div className="welcome-sub">
        Supply Chain Crisis Lab — step into the role of a CEO facing a live supply chain emergency.
        Make real trade-offs across cost, inventory, speed, negotiation, and leadership — then see how your decisions add up.
      </div>
      <div className="welcome-features">
        <div className="feature-pill">🎲 Randomized company & crisis</div>
        <div className="feature-pill">📊 Live business metrics</div>
        <div className="feature-pill">🤝 Branching negotiation</div>
        <div className="feature-pill">🧠 CEO decision scoring</div>
        <div className="feature-pill">🤖 AI investment strategy</div>
      </div>
      <button className="btn btn-primary" style={{marginTop:18, padding:"15px 34px", fontSize:16}} onClick={onStart}>
        Start Simulation →
      </button>
      <p className="faint" style={{maxWidth:480, fontSize:12.5, marginTop:6}}>
        No two runs are the same — every playthrough generates a new company, crisis, and outcome.
      </p>
    </div>
  );
}

function CompanyScreen({ company, onNext }){
  const stats = [
    { icon:"🏭", label:"Factories", value: company.factories },
    { icon:"📦", label:"Warehouses", value: company.warehouses },
    { icon:"🔗", label:"Suppliers", value: company.suppliers },
    { icon:"📅", label:"Inventory Days", value: company.inventoryDays + " days" },
    { icon:"⏱️", label:"Avg. Lead Time", value: company.leadTime + " days" },
    { icon:"💰", label:"Annual Revenue", value: "$" + company.revenue + "M" }
  ];
  return (
    <div className="slide-up">
      <div className="section-head">
        <div className="section-eyebrow">Step 1 · Meet Your Company</div>
        <div className="section-title">{company.name}</div>
        <div className="section-desc">Industry: {company.industry}</div>
      </div>
      <InfoBox title="Why this matters">
        Before you can manage a crisis, you need to understand what you're protecting. These numbers define your
        exposure: more factories and suppliers mean more points of failure, while low inventory days mean less
        buffer before a disruption hits customers.
      </InfoBox>
      <div className="grid grid-3" style={{marginBottom:16}}>
        {stats.map((s,i)=>(
          <div className="card stat-card" key={i}>
            <span className="stat-icon">{s.icon}</span>
            <div className="stat-label">{s.label}</div>
            <div className="stat-value">{s.value}</div>
          </div>
        ))}
      </div>
      <div className="card">
        <div className="stat-label" style={{marginBottom:10}}>Operating Countries</div>
        <div>
          {company.countries.map((c,i)=>(<span className="country-chip" key={i}>{c}</span>))}
        </div>
      </div>
      <div className="btn-row">
        <button className="btn btn-primary" onClick={onNext}>Continue to Crisis Briefing →</button>
      </div>
    </div>
  );
}

function CrisisScreen({ company, crisis, onNext }){
  return (
    <div className="slide-up">
      <div className="section-head">
        <div className="section-eyebrow">Step 2 · Crisis Briefing</div>
        <div className="section-title">{crisis.icon} {crisis.type}</div>
      </div>
      <InfoBox title="Why this matters">
        Every crisis has an urgency level and a projected business impact. Reading this carefully tells you how much
        time you have and which metrics are most at risk — that context should shape which War Room actions you pick next.
      </InfoBox>
      <div className="card crisis-card" style={{marginBottom:16}}>
        <div style={{display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:14, flexWrap:"wrap", gap:10}}>
          <h3 style={{fontSize:19}}>Incident Report</h3>
          <span className={"urgency-tag urgency-" + crisis.urgency}>{crisis.urgency} Urgency</span>
        </div>
        <p className="muted" style={{marginBottom:14, fontSize:15}}>{crisis.description}</p>
        <div style={{borderTop:"1px solid var(--border)", paddingTop:14}}>
          <div className="stat-label" style={{marginBottom:6}}>Projected Business Impact</div>
          <p style={{fontSize:14}}>{crisis.impact}</p>
        </div>
      </div>
      <div className="btn-row">
        <button className="btn btn-primary" onClick={onNext}>Enter the War Room →</button>
      </div>
    </div>
  );
}

function WarRoomScreen({ crisis, metrics, onComplete }){
  const [selected, setSelected] = useState([]);
  const [phase, setPhase] = useState("select"); // select | results
  const [resultMetrics, setResultMetrics] = useState(metrics);
  const [chosenActions, setChosenActions] = useState([]);

  function toggle(id){
    setSelected(prev=>{
      if(prev.includes(id)) return prev.filter(x=>x!==id);
      if(prev.length>=3) return prev;
      return [...prev, id];
    });
  }

  function deploy(){
    const actions = WAR_ROOM_ACTIONS.filter(a=>selected.includes(a.id));
    const next = { ...metrics };
    Object.keys(next).forEach(k=>{
      let delta = 0;
      actions.forEach(a=>{ delta += a.effects[k]; });
      delta += randInt(-2,2);
      next[k] = clamp(next[k] + delta);
    });
    setChosenActions(actions);
    setPhase("results");
    // trigger animation on next tick
    setTimeout(()=> setResultMetrics(next), 60);
  }

  return (
    <div className="slide-up">
      <div className="section-head">
        <div className="section-eyebrow">Step 3 · War Room</div>
        <div className="section-title">Choose Your Response Strategy</div>
        <div className="section-desc">Select exactly 3 of the 6 available actions to respond to the {crisis.type.toLowerCase()}.</div>
      </div>
      <InfoBox title="Why this matters">
        There's no free lunch in crisis response — every action trades one metric for another. Speed usually costs money,
        margin protection usually costs satisfaction. Picking three that complement each other is the core skill of
        supply chain leadership.
      </InfoBox>

      {phase === "select" && (
        <React.Fragment>
          <div className="progress-line">Selected {selected.length} / 3</div>
          <div className="grid grid-2">
            {WAR_ROOM_ACTIONS.map(a=>{
              const isSelected = selected.includes(a.id);
              const isDisabled = !isSelected && selected.length>=3;
              return (
                <div key={a.id}
                  className={"option-card" + (isSelected? " selected":"") + (isDisabled? " disabled":"")}
                  onClick={()=> !isDisabled && toggle(a.id)}>
                  <div className="option-check">✓</div>
                  <div className="option-title">{a.icon} {a.title}</div>
                  <div className="option-desc">{a.desc}</div>
                  <div className="option-desc" style={{marginTop:8, fontStyle:"italic", color:"var(--text-faint)"}}>{a.why}</div>
                  <div className="option-tags">
                    {Object.entries(a.effects).map(([k,v])=>(
                      <span key={k} className={"tag " + (v>=0? "tag-up":"tag-down")}>
                        {METRIC_META[k].label} {v>0? "+":""}{v}
                      </span>
                    ))}
                  </div>
                </div>
              );
            })}
          </div>
          <div className="btn-row">
            <button className="btn btn-primary" disabled={selected.length!==3} onClick={deploy}>
              Deploy Response ({selected.length}/3) →
            </button>
          </div>
        </React.Fragment>
      )}

      {phase === "results" && (
        <div className="card fade-in">
          <h3 style={{marginBottom:6}}>Response Deployed</h3>
          <p className="muted" style={{fontSize:13.5, marginBottom:18}}>
            You chose: {chosenActions.map(a=>a.title).join(", ")}. Here's how your business metrics shifted.
          </p>
          {Object.keys(resultMetrics).map(k=>(
            <MetricBar key={k} metricKey={k} value={resultMetrics[k]} />
          ))}
          <div className="btn-row">
            <button className="btn btn-primary" onClick={()=> onComplete(resultMetrics, chosenActions)}>
              Continue to Negotiation →
            </button>
          </div>
        </div>
      )}
    </div>
