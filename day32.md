<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Think Like a Marketing Strategist: Grow This Brand</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#0B0D12;
    --panel:#141821;
    --panel-alt:#1B212C;
    --panel-raised:#212836;
    --border:rgba(255,255,255,0.08);
    --border-strong:rgba(255,255,255,0.16);
    --text:#ECEAE4;
    --muted:#98A0B3;
    --muted-dim:#6B7182;
    --amber:#FFB454;
    --amber-dim:#8A6832;
    --teal:#4FD8C4;
    --teal-dim:#2C6E64;
    --danger:#FF7A7A;
    --font-display:'Space Grotesk', ui-sans-serif, system-ui, sans-serif;
    --font-body:'Inter', ui-sans-serif, system-ui, sans-serif;
    --font-mono:'IBM Plex Mono', ui-monospace, SFMono-Regular, monospace;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(circle at 12% -10%, rgba(255,180,84,0.08), transparent 40%),
      radial-gradient(circle at 90% 10%, rgba(79,216,196,0.07), transparent 45%),
      var(--ink);
    color:var(--text);
    font-family:var(--font-body);
    min-height:100vh;
    -webkit-font-smoothing:antialiased;
  }
  #root{min-height:100vh;}
  ::selection{background:var(--amber); color:#1a1200;}

  /* ---------- layout shell ---------- */
  .app{
    max-width:980px;
    margin:0 auto;
    padding:28px 20px 80px;
  }
  .topbar{
    display:flex;
    align-items:center;
    justify-content:space-between;
    gap:16px;
    margin-bottom:28px;
    flex-wrap:wrap;
  }
  .brandmark{
    display:flex;
    align-items:center;
    gap:10px;
    font-family:var(--font-display);
    font-weight:700;
    font-size:15px;
    letter-spacing:0.02em;
  }
  .brandmark .dot{
    width:9px;height:9px;border-radius:50%;
    background:linear-gradient(135deg, var(--amber), var(--teal));
    box-shadow:0 0 12px rgba(255,180,84,0.6);
  }
  .brandmark small{
    display:block;
    font-family:var(--font-mono);
    font-weight:400;
    font-size:10.5px;
    color:var(--muted-dim);
    letter-spacing:0.08em;
    text-transform:uppercase;
    margin-top:2px;
  }

  /* insight meter - signature element */
  .insight-meter{
    display:flex;
    align-items:center;
    gap:10px;
    background:var(--panel);
    border:1px solid var(--border);
    border-radius:100px;
    padding:6px 14px 6px 6px;
  }
  .insight-meter .ring{
    position:relative;
    width:34px;height:34px;
    border-radius:50%;
    background:conic-gradient(var(--amber) calc(var(--pct,0)*1%), var(--teal) calc(var(--pct,0)*1%), rgba(255,255,255,0.08) 0);
    display:flex;align-items:center;justify-content:center;
    transition:background 0.6s ease;
  }
  .insight-meter .ring::after{
    content:"";
    position:absolute;
    inset:4px;
    background:var(--panel);
    border-radius:50%;
  }
  .insight-meter .ring span{
    position:relative;
    z-index:1;
    font-family:var(--font-mono);
    font-size:9.5px;
    color:var(--text);
  }
  .insight-meter .label{
    font-family:var(--font-mono);
    font-size:10px;
    text-transform:uppercase;
    letter-spacing:0.08em;
    color:var(--muted);
    line-height:1.3;
  }
  .insight-meter .label b{
    display:block;
    color:var(--text);
    font-size:12px;
    letter-spacing:0.02em;
  }

  /* progress rail */
  .rail{
    display:flex;
    gap:4px;
    margin-bottom:26px;
    overflow-x:auto;
    padding-bottom:4px;
  }
  .rail-step{
    flex:1;
    min-width:64px;
    text-align:left;
    padding-bottom:8px;
    border-bottom:2px solid var(--border);
    font-family:var(--font-mono);
    font-size:10px;
    letter-spacing:0.06em;
    color:var(--muted-dim);
    text-transform:uppercase;
    white-space:nowrap;
    transition:all 0.3s ease;
  }
  .rail-step.done{ border-color:var(--teal-dim); color:var(--teal);}
  .rail-step.active{ border-color:var(--amber); color:var(--amber);}

  /* ---------- panel / card system ---------- */
  .step-panel{ animation:riseIn 0.45s cubic-bezier(.2,.8,.2,1); }
  @keyframes riseIn{
    from{opacity:0; transform:translateY(10px);}
    to{opacity:1; transform:translateY(0);}
  }
  @media (prefers-reduced-motion: reduce){
    .step-panel{animation:none;}
    *{animation-duration:0.001ms !important; transition-duration:0.001ms !important;}
  }

  .eyebrow{
    font-family:var(--font-mono);
    font-size:11px;
    letter-spacing:0.14em;
    text-transform:uppercase;
    color:var(--amber);
    margin:0 0 10px;
    display:flex;
    align-items:center;
    gap:8px;
  }
  .eyebrow::before{
    content:"";
    width:16px;height:1px;
    background:var(--amber);
    display:inline-block;
  }
  h1.headline{
    font-family:var(--font-display);
    font-size:clamp(26px,4vw,38px);
    line-height:1.15;
    margin:0 0 14px;
    letter-spacing:-0.01em;
  }
  h2.headline{
    font-family:var(--font-display);
    font-size:clamp(20px,3vw,26px);
    line-height:1.2;
    margin:0 0 10px;
  }
  p.lede{
    font-size:15.5px;
    line-height:1.65;
    color:var(--muted);
    max-width:640px;
    margin:0 0 22px;
  }

  .card{
    background:var(--panel);
    border:1px solid var(--border);
    border-radius:14px;
    padding:20px 22px;
    margin-bottom:16px;
  }
  .card-raised{ background:var(--panel-raised); border-color:var(--border-strong); }

  .why-box{
    display:grid;
    grid-template-columns:auto 1fr;
    gap:10px 12px;
    background:var(--panel-alt);
    border:1px solid var(--border);
    border-left:2px solid var(--teal);
    border-radius:10px;
    padding:14px 16px;
    margin:16px 0 22px;
  }
  .why-box .tag{
    font-family:var(--font-mono);
    font-size:10.5px;
    letter-spacing:0.08em;
    text-transform:uppercase;
    color:var(--teal);
    padding-top:1px;
    white-space:nowrap;
  }
  .why-box p{ margin:0; font-size:13.8px; line-height:1.6; color:var(--muted); }
  .why-box p+p{margin-top:6px;}

  /* selectable grid options */
  .grid{
    display:grid;
    grid-template-columns:repeat(auto-fit, minmax(220px,1fr));
    gap:12px;
    margin:18px 0;
  }
  .opt{
    text-align:left;
    background:var(--panel);
    border:1px solid var(--border);
    border-radius:12px;
    padding:16px 16px 14px;
    cursor:pointer;
    color:var(--text);
    font-family:var(--font-body);
    transition:border-color 0.2s ease, transform 0.15s ease, background 0.2s ease;
    position:relative;
  }
  .opt:hover{ border-color:var(--border-strong); transform:translateY(-2px); }
  .opt.selected{
    border-color:var(--amber);
    background:linear-gradient(180deg, rgba(255,180,84,0.09), rgba(255,180,84,0.02));
  }
  .opt.recommended::after{
    content:"RECOMMENDED FIT";
    position:absolute;
    top:10px; right:10px;
    font-family:var(--font-mono);
    font-size:8.5px;
    letter-spacing:0.06em;
    color:var(--teal);
    border:1px solid var(--teal-dim);
    padding:2px 6px;
    border-radius:100px;
  }
  .opt .opt-icon{ font-size:20px; margin-bottom:8px; display:block; }
  .opt .opt-name{ font-family:var(--font-display); font-weight:600; font-size:15.5px; margin-bottom:4px; }
  .opt .opt-desc{ font-size:13px; color:var(--muted); line-height:1.5; }
  .opt .opt-check{
    position:absolute; bottom:12px; right:14px;
    width:18px; height:18px; border-radius:50%;
    border:1.5px solid var(--border-strong);
    display:flex; align-items:center; justify-content:center;
    font-size:11px; color:transparent;
  }
  .opt.selected .opt-check{ background:var(--amber); border-color:var(--amber); color:#1a1200; }

  .selection-feedback{
    margin-top:4px;
    padding:12px 14px;
    background:var(--panel-alt);
    border:1px solid var(--border);
    border-radius:10px;
    font-size:13.3px;
    line-height:1.6;
    color:var(--muted);
  }
  .selection-feedback b{ color:var(--text); }
  .selection-feedback.good{ border-left:2px solid var(--teal); }
  .selection-feedback.warn{ border-left:2px solid var(--danger); }

  /* forms */
  label.field-label{
    display:block;
    font-family:var(--font-mono);
    font-size:10.5px;
    letter-spacing:0.06em;
    text-transform:uppercase;
    color:var(--muted);
    margin:14px 0 6px;
  }
  input.field, textarea.field{
    width:100%;
    background:var(--panel-alt);
    border:1px solid var(--border);
    border-radius:9px;
    padding:11px 13px;
    color:var(--text);
    font-family:var(--font-body);
    font-size:14px;
    resize:vertical;
    transition:border-color 0.2s ease;
  }
  input.field:focus, textarea.field:focus{ outline:none; border-color:var(--amber); }
  input.field::placeholder, textarea.field::placeholder{ color:var(--muted-dim); }

  /* buttons */
  .btn{
    font-family:var(--font-body);
    font-weight:600;
    font-size:14px;
    border-radius:10px;
    padding:12px 22px;
    border:1px solid var(--border-strong);
    background:var(--panel-raised);
    color:var(--text);
    cursor:pointer;
    transition:all 0.2s ease;
  }
  .btn:hover:not(:disabled){ border-color:var(--amber); transform:translateY(-1px); }
  .btn:disabled{ opacity:0.35; cursor:not-allowed; }
  .btn-primary{
    background:linear-gradient(135deg, var(--amber), #E0913A);
    color:#1a1200;
    border:none;
  }
  .btn-primary:hover:not(:disabled){ filter:brightness(1.08); transform:translateY(-1px); }
  .btn-ghost{ background:transparent; border-color:var(--border); color:var(--muted); }
  .btn-ghost:hover:not(:disabled){ color:var(--text); }
  .btn-block{ width:100%; }
  .btn:focus-visible, .opt:focus-visible, input.field:focus-visible, textarea.field:focus-visible{
    outline:2px solid var(--teal); outline-offset:2px;
  }

  .nav-row{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-top:28px;
    gap:12px;
  }
  .nav-row .spacer{flex:1;}

  /* claude prompt card */
  .claude-card{
    background:var(--panel-alt);
    border:1px dashed var(--border-strong);
    border-radius:12px;
    padding:16px 18px;
    margin-top:22px;
  }
  .claude-card .cc-head{
    display:flex; align-items:center; justify-content:space-between;
    margin-bottom:10px; gap:10px; flex-wrap:wrap;
  }
  .claude-card .cc-title{
    display:flex; align-items:center; gap:8px;
    font-family:var(--font-mono);
    font-size:11px; letter-spacing:0.07em; text-transform:uppercase;
    color:var(--amber);
  }
  .claude-card pre{
    font-family:var(--font-mono);
    font-size:12.5px;
    line-height:1.65;
    color:#D9E6E2;
    background:var(--ink);
    border:1px solid var(--border);
    border-radius:9px;
    padding:14px 15px;
    white-space:pre-wrap;
    word-break:break-word;
    margin:0;
  }
  .claude-card .cc-why{
    margin-top:10px;
    font-size:12.5px;
    color:var(--muted);
    line-height:1.55;
  }
  .copy-btn{
    font-family:var(--font-mono);
    font-size:11px;
    background:transparent;
    border:1px solid var(--border-strong);
    color:var(--muted);
    padding:6px 10px;
    border-radius:7px;
    cursor:pointer;
    transition:all 0.15s ease;
  }
  .copy-btn:hover{ border-color:var(--teal); color:var(--teal); }
  .copy-btn.copied{ border-color:var(--teal); color:var(--teal); }

  /* roadmap weeks */
  .week{
    display:grid;
    grid-template-columns:70px 1fr;
    gap:16px;
    padding:16px 0;
    border-top:1px solid var(--border);
  }
  .week:first-child{border-top:none;}
  .week .wk-num{
    font-family:var(--font-display);
    font-size:28px;
    font-weight:700;
    color:var(--muted-dim);
    line-height:1;
  }
  .week .wk-num small{
    display:block;
    font-family:var(--font-mono);
    font-size:9.5px;
    color:var(--muted-dim);
    text-transform:uppercase;
    margin-top:4px;
    letter-spacing:0.06em;
  }
  .week .wk-title{ font-family:var(--font-display); font-size:16.5px; font-weight:600; margin-bottom:6px; }
  .week .wk-goal{ font-size:13.8px; color:var(--muted); line-height:1.6; }

  /* curveball */
  .curveball-banner{
    background:linear-gradient(135deg, rgba(255,180,84,0.14), rgba(79,216,196,0.08));
    border:1px solid var(--border-strong);
    border-radius:14px;
    padding:22px;
    margin-bottom:18px;
  }
  .curveball-banner .cb-tag{
    font-family:var(--font-mono);
    font-size:11px;
    letter-spacing:0.1em;
    text-transform:uppercase;
    color:var(--danger);
    margin-bottom:8px;
    display:block;
  }
  .consequence{
    margin-top:14px;
    padding:14px 16px;
    background:var(--panel-alt);
    border-radius:10px;
    border-left:2px solid var(--amber);
    font-size:13.8px;
    line-height:1.6;
    color:var(--muted);
  }
  .consequence b{color:var(--text);}

  /* report */
  .report-grid{
    display:grid;
    grid-template-columns:repeat(auto-fit, minmax(260px,1fr));
    gap:14px;
    margin-bottom:16px;
  }
  .report-card .rc-label{
    font-family:var(--font-mono);
    font-size:10.5px;
    letter-spacing:0.08em;
    text-transform:uppercase;
    color:var(--teal);
    margin-bottom:8px;
  }
  .report-card p{ font-size:13.6px; line-height:1.65; color:var(--muted); margin:0; }
  .report-card.decision .rc-label{color:var(--amber);}
  .report-card.mistake .rc-label{color:var(--danger);}
  .lessons-list{ list-style:none; margin:0; padding:0; counter-reset:lesson; }
  .lessons-list li{
    counter-increment:lesson;
    padding:14px 0 14px 40px;
    border-top:1px solid var(--border);
    position:relative;
    font-size:14px;
    line-height:1.6;
    color:var(--muted);
  }
  .lessons-list li:first-child{border-top:none;}
  .lessons-list li::before{
    content:counter(lesson);
    position:absolute; left:0; top:14px;
    width:26px;height:26px;
    border-radius:50%;
    background:var(--panel-alt);
    border:1px solid var(--border-strong);
    color:var(--amber);
    font-family:var(--font-mono);
    font-size:12px;
    display:flex; align-items:center; justify-content:center;
  }
  .lessons-list li b{color:var(--text);}

  .pill{
    display:inline-flex; align-items:center; gap:5px;
    font-family:var(--font-mono);
    font-size:11px;
    padding:4px 10px;
    border-radius:100px;
    border:1px solid var(--border-strong);
    color:var(--muted);
    margin:2px 4px 2px 0;
  }
  .pill.amber{ color:var(--amber); border-color:var(--amber-dim); }
  .pill.teal{ color:var(--teal); border-color:var(--teal-dim); }

  .brief-grid{
    display:grid;
    grid-template-columns:repeat(auto-fit, minmax(180px,1fr));
    gap:12px;
    margin:18px 0;
  }
  .brief-field{
    background:var(--panel-alt);
    border:1px solid var(--border);
    border-radius:10px;
    padding:12px 14px;
  }
  .brief-field .bf-label{
    font-family:var(--font-mono);
    font-size:9.5px;
    text-transform:uppercase;
    letter-spacing:0.08em;
    color:var(--muted-dim);
    margin-bottom:5px;
  }
  .brief-field .bf-value{ font-size:13.6px; color:var(--text); line-height:1.5; }

  .mode-hero{
    text-align:center;
    padding:6px 0 10px;
  }
  .footer-note{
    text-align:center;
    color:var(--muted-dim);
    font-family:var(--font-mono);
    font-size:11px;
    margin-top:50px;
    letter-spacing:0.03em;
  }

  @media (max-width:640px){
    .week{ grid-template-columns:44px 1fr; }
    .why-box{ grid-template-columns:1fr; }
  }
</style>
</head>
<body>
<div id="root"></div>

<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

<script type="text/babel" data-presets="react">
const { useState, useMemo, useRef } = React;

/* =========================================================
   DATA
   ========================================================= */

const STEP_LABELS = ["Welcome","Path","Brief","Audience","Platforms","Pillars","Roadmap","Curveball","Report"];

const INDUSTRIES = [
  "Artisan Coffee Roastery","Sustainable Fashion Label","Home Fitness App",
  "Boutique Hotel Group","Pet Wellness Brand","Plant-Based Meal Kit",
  "Indie Video Game Studio","Handmade Jewelry Shop","Language Learning Platform",
  "Local Craft Brewery","Kids' STEM Toy Line","Clean Skincare Startup"
];
const AUDIENCES = [
  "busy young professionals aged 25–34","eco-conscious millennials",
  "new parents balancing work and family","Gen Z college students",
  "remote workers trying to stay well","pet owners who treat pets like family",
  "budget-conscious first-time travelers","small business owners wearing every hat"
];
const BUDGETS = [
  "$500/month — bootstrapped and scrappy","$2,000/month — small but steady",
  "$10,000/month — growth stage, some help","$50,000/month — investor-backed, a real team"
];
const COMPETITOR_STYLES = [
  "a bigger, better-funded version of itself","a scrappy newcomer growing faster on TikTok",
  "a legacy brand that's slow but trusted","three lookalike brands fighting for the same audience"
];
const CHALLENGES = [
  "Almost nobody outside a small circle knows this brand exists yet.",
  "Sales are steady, but growth has completely stalled for months.",
  "A viral trend passed the brand by, and now it feels behind.",
  "Loyal customers exist, but reviews and word-of-mouth are inconsistent.",
  "The founder wants to expand into a new city with zero local audience.",
  "Engagement looks great on paper, but it never turns into sales."
];
const EXPERTISE_AREAS = [
  "career coaching for career-changers","freelance graphic design","personal finance for beginners",
  "fitness for busy parents","productivity systems for founders","ethical AI development",
  "urban gardening on a budget","indie filmmaking on a tiny budget"
];

function pick(arr){ return arr[Math.floor(Math.random()*arr.length)]; }
function pickTwo(arr){
  const a = pick(arr);
  let b = pick(arr);
  while(b===a) b = pick(arr);
  return [a,b];
}

function randomBusiness(){
  const industry = pick(INDUSTRIES);
  const [c1,c2] = pickTwo(COMPETITOR_STYLES);
  return {
    name: industry, // used as display label; user can imagine actual name
    industry,
    audience: pick(AUDIENCES),
    budget: pick(BUDGETS),
    competitors: `Faces ${c1}, and separately ${c2}.`,
    challenge: pick(CHALLENGES)
  };
}

const PLATFORMS = [
  { id:'instagram', name:'Instagram', icon:'📸',
    blurb:'Visual storytelling through photos, Reels and Stories.',
    bizWhy:'Strong when the product is visually distinctive — food, fashion, décor, lifestyle — where seeing it is most of the sell.',
    personalWhy:'Good for showing personality and daily life, but weaker than LinkedIn or X for building authority.'},
  { id:'tiktok', name:'TikTok', icon:'🎵',
    blurb:'Short, fast, algorithm-driven video built for discovery.',
    bizWhy:'Best when the brand can be entertaining or trend-aware quickly — huge reach with younger audiences.',
    personalWhy:'Can spring an unknown name into visibility fast, but rewards volume and trends over deep authority.'},
  { id:'facebook', name:'Facebook', icon:'👥',
    blurb:'Community groups, local reach, and older demographics.',
    bizWhy:'Strong for local businesses and community-building, especially with audiences over 35.',
    personalWhy:'Rarely the first choice for a personal brand unless the audience is explicitly older or local.'},
  { id:'linkedin', name:'LinkedIn', icon:'💼',
    blurb:'A professional network built around expertise and career.',
    bizWhy:'Works well for B2B brands selling to other businesses or professionals.',
    personalWhy:'Often the single best platform for building professional authority and attracting opportunities.'},
  { id:'x', name:'X / Twitter', icon:'𝕏',
    blurb:'Real-time text conversation and fast-moving ideas.',
    bizWhy:'Useful for customer service and joining public conversations, but weak for direct sales.',
    personalWhy:'Ideal for sharp opinions, building a following of peers, and getting noticed by other experts.'},
  { id:'youtube', name:'YouTube', icon:'▶️',
    blurb:'Long-form video that stays searchable and evergreen.',
    bizWhy:'Good when the product benefits from demonstration, tutorials or unboxing.',
    personalWhy:'The best platform for demonstrating deep expertise over time — content keeps working for years.'},
  { id:'pinterest', name:'Pinterest', icon:'📌',
    blurb:'A visual search engine people use to plan purchases.',
    bizWhy:'Strong for physical products in home, fashion, food and weddings.',
    personalWhy:'Rarely useful here — it is a discovery tool for products, not a relationship-building platform.'},
  { id:'newsletter', name:'Newsletter / Email', icon:'✉️',
    blurb:'Direct, owned access to an audience\u2019s inbox.',
    bizWhy:'The highest-ROI channel for repeat customers — the brand owns the list, not an algorithm.',
    personalWhy:'Essential for personal brands — it turns followers into a real, owned relationship.'}
];

function recommendedPlatforms(mode, brand){
  if(mode==='personal') return ['linkedin','x','youtube','newsletter'];
  const ind = (brand.industry||'').toLowerCase();
  if(/fashion|jewelry|skincare|toy/.test(ind)) return ['instagram','tiktok','pinterest'];
  if(/app|game|studio/.test(ind)) return ['x','youtube','tiktok'];
  if(/hotel/.test(ind)) return ['instagram','pinterest','facebook'];
  if(/brewery|coffee/.test(ind)) return ['instagram','facebook','tiktok'];
  if(/meal|wellness/.test(ind)) return ['instagram','pinterest','tiktok'];
  if(/language/.test(ind)) return ['youtube','tiktok','instagram'];
  return ['instagram','facebook','tiktok'];
}

const CONTENT_PILLARS = [
  { id:'education', name:'Education', desc:'Teach the audience something useful and related to the brand.',
    goal:'Builds trust and positions the brand as helpful, not just promotional.'},
  { id:'bts', name:'Behind the Scenes', desc:'Show the process, the people, and the effort behind the work.',
    goal:'Builds relatability and humanizes the brand.', personal:true},
  { id:'ugc', name:'Community & UGC', desc:'Feature customers, fans or audience members.',
    goal:'Builds social proof and a sense of belonging.'},
  { id:'promo', name:'Promotional', desc:'Direct offers, launches and calls to action.',
    goal:'Drives immediate sales or sign-ups.'},
  { id:'trend', name:'Trend & Entertainment', desc:'Light, timely, entertaining content tied to what\u2019s current.',
    goal:'Boosts reach and discoverability with brand-new audiences.'},
  { id:'thoughtleadership', name:'Thought Leadership', desc:'Share opinions and insight on where the space is headed.',
    goal:'Builds authority and attracts high-value opportunities.', personal:true},
  { id:'personalstory', name:'Personal Story', desc:'Share the journey — the struggles and the turning points.',
    goal:'Builds emotional connection and memorability.', personal:true},
  { id:'audienceed', name:'Audience Education', desc:'Break down concepts the audience doesn\u2019t understand yet.',
    goal:'Builds trust through generosity and clarity.', personal:true}
];

const BIZ_EVENTS = [
  { title:'A Post Goes Viral Overnight',
    desc:'One of last week\u2019s posts is suddenly everywhere. Followers are pouring in, but most have never heard of {brand} before.',
    choices:[
      {label:'Ride the wave — post follow-up content immediately', consequence:'Momentum builds fast, but reacting without a plan risks diluting the brand voice built so far.'},
      {label:'Pause and prepare a strategic follow-up for tomorrow', consequence:'A more polished response lands, but some of the viral attention has already moved on by then.'},
      {label:'Reply personally to the top comments', consequence:'Deepens trust with new followers, though it doesn\u2019t use the reach as aggressively as new content would.'}
    ]},
  { title:'A Competitor Starts Copying the Brand',
    desc:'A rival has started posting content that looks suspiciously similar to {brand}\u2019s recent campaign.',
    choices:[
      {label:'Ignore it and keep executing the existing plan', consequence:'Preserves focus and dignity — being copied is common and rarely worth a public reaction.'},
      {label:'Call it out publicly', consequence:'Creates short-term attention, but risks making the brand look insecure or petty.'},
      {label:'Go deeper on what makes {brand} original', consequence:'Strengthens brand identity long-term, turning a threat into a reason to sharpen the message.'}
    ]},
  { title:'A Negative Review Goes Semi-Viral',
    desc:'A disappointed customer\u2019s review of {brand} is being shared, picking up sympathetic comments.',
    choices:[
      {label:'Respond publicly and transparently', consequence:'Shows accountability and often earns respect, even from people who weren\u2019t directly affected.'},
      {label:'Try to get the review taken down', consequence:'Can backfire badly if noticed, turning a small issue into a bigger trust crisis.'},
      {label:'Resolve it privately and say nothing publicly', consequence:'Solves the individual problem, but the public conversation continues without the brand\u2019s side of the story.'}
    ]},
  { title:'An Influencer Mentions the Brand, Unpaid',
    desc:'A mid-size creator in {brand}\u2019s space mentioned it positively, unprompted, in a recent video.',
    choices:[
      {label:'Reach out to build a real relationship — no hard pitch', consequence:'Plants the seed for a genuine long-term partnership rather than a one-off transaction.'},
      {label:'Immediately offer a paid deal', consequence:'Might convert, but can feel transactional and undercut the organic goodwill just created.'},
      {label:'Repost and thank them publicly, then move on', consequence:'Low-risk and appreciated, though it may not lead anywhere further without more outreach.'}
    ]}
];

const PERSONAL_EVENTS = [
  { title:'One Post Goes Viral',
    desc:'A post about {expertise} is suddenly everywhere. {name}\u2019s follower count is climbing by the hour.',
    choices:[
      {label:'Post a clear pinned "start here" intro right away', consequence:'Converts the sudden attention into people who actually understand who {name} is and stick around.'},
      {label:'Chase the momentum with rapid follow-up posts', consequence:'Keeps reach high short-term, but risks posting reactively instead of on-message.'},
      {label:'Slow down and reply to comments personally', consequence:'Builds real relationships with the most engaged new followers, though it uses less of the spike.'}
    ]},
  { title:'A Podcast Invite Arrives',
    desc:'A podcast host in the {expertise} space invites {name} on, with almost no notice.',
    choices:[
      {label:'Say yes immediately and prepare fast', consequence:'Captures the opportunity while it\u2019s hot, though prep time is tight.'},
      {label:'Ask for two weeks to prepare properly', consequence:'Leads to a stronger appearance, though some hosts move on to another guest if asked to wait.'},
      {label:'Decline — it doesn\u2019t feel ready yet', consequence:'Protects against a weak first impression, but chances like this don\u2019t always come twice.'}
    ]},
  { title:'A Public Disagreement Breaks Out',
    desc:'Someone with a large following publicly disagrees with a take {name} posted about {expertise}.',
    choices:[
      {label:'Respond thoughtfully and stand by the point', consequence:'Reinforces a clear point of view and builds credibility, even if it invites some pushback.'},
      {label:'Quietly delete the post', consequence:'Avoids conflict but can look evasive if people already saw or saved it.'},
      {label:'Turn it into material for a deeper follow-up post', consequence:'Converts controversy into substance — often the highest-leverage response of the three.'}
    ]},
  { title:'Someone Copies the Content',
    desc:'Another creator has been posting content strikingly similar to {name}\u2019s recent ideas about {expertise}.',
    choices:[
      {label:'Let it go and keep publishing', consequence:'Saves energy for original work — imitation is common and rarely worth chasing.'},
      {label:'Message them directly about it', consequence:'Might resolve it quietly, but could also escalate into a public back-and-forth.'},
      {label:'Go deeper and more specific than they can copy', consequence:'The strongest long-term move — original experience and story are hard to imitate.'}
    ]},
  { title:'A Sudden Follower Spike',
    desc:'{name}\u2019s following doubled overnight after an unrelated trend pushed traffic to the profile.',
    choices:[
      {label:'Post a clear "start here" pinned intro immediately', consequence:'Converts new attention into people who understand and stick around.'},
      {label:'Keep posting as normal', consequence:'Feels natural, but many new followers may disengage without more context.'},
      {label:'Launch a newsletter sign-up push', consequence:'Turns fleeting platform attention into an owned, lasting audience.'}
    ]}
];

/* =========================================================
   SMALL HELPERS
   ========================================================= */

function fill(str, brand){
  return str
    .replaceAll('{brand}', brand.name || 'the brand')
    .replaceAll('{name}', brand.name || 'you')
    .replaceAll('{expertise}', brand.expertise || 'their niche');
}

function CopyButton({ text }){
  const [copied, setCopied] = useState(false);
  const doCopy = () => {
    const done = () => { setCopied(true); setTimeout(()=>setCopied(false), 1600); };
    if(navigator.clipboard && navigator.clipboard.writeText){
      navigator.clipboard.writeText(text).then(done).catch(done);
    } else {
      done();
    }
  };
  return (
    <button className={"copy-btn" + (copied ? " copied" : "")} onClick={doCopy}>
      {copied ? "✓ Copied" : "Copy prompt"}
    </button>
  );
}

function ClaudeCard({ title, prompt, why }){
  return (
    <div className="claude-card">
      <div className="cc-head">
        <div className="cc-title">🤖 How to ask Claude — {title}</div>
        <CopyButton text={prompt} />
      </div>
      <pre>{prompt}</pre>
      {why && <div className="cc-why">{why}</div>}
    </div>
  );
}

function WhyBox({ items }){
  return (
    <div className="why-box">
      {items.map((it,i)=>(
        <React.Fragment key={i}>
          <div className="tag">{it.tag}</div>
          <div>{it.lines.map((l,j)=><p key={j}>{l}</p>)}</div>
        </React.Fragment>
      ))}
    </div>
  );
}

function InsightMeter({ pct, label }){
  return (
    <div className="insight-meter" style={{'--pct': pct}}>
      <div className="ring"><span>{pct}%</span></div>
      <div className="label">Strategy Signal<b>{label}</b></div>
    </div>
  );
}

/* =========================================================
   APP
   ========================================================= */

function App(){
  const [step, setStep] = useState(0);
  const [mode, setMode] = useState(null); // 'own' | 'personal' | 'random'
  const [brand, setBrand] = useState(null);
  const [platforms, setPlatforms] = useState([]);
  const [pillars, setPillars] = useState([]);
  const [event, setEvent] = useState(null);
  const [eventChoiceIdx, setEventChoiceIdx] = useState(null);

  const [ownForm, setOwnForm] = useState({ name:'', industry:'', audience:'', budget:'', competitors:'', challenge:'' });
  const [personalForm, setPersonalForm] = useState({ name:'', expertise:'', story:'', audience:'' });

  const goto = (n) => { setStep(n); window.scrollTo({top:0, behavior:'smooth'}); };
  const next = () => goto(Math.min(step+1, STEP_LABELS.length-1));
  const back = () => goto(Math.max(step-1, 0));

  const isPersonal = mode === 'personal';

  const recommended = useMemo(()=> brand ? recommendedPlatforms(mode, brand) : [], [mode, brand]);
  const pillarPool = useMemo(()=>{
    if(!isPersonal) return CONTENT_PILLARS;
    // prioritize personal-tagged pillars first for personal-brand mode, keep all available
    return [...CONTENT_PILLARS].sort((a,b)=> (b.personal?1:0) - (a.personal?1:0));
  }, [isPersonal]);

  const eventPool = isPersonal ? PERSONAL_EVENTS : BIZ_EVENTS;

  // insight score: how many "recommended" platforms picked + whether exactly 3 pillars/platforms chosen
  const insightPct = useMemo(()=>{
    let score = 0;
    if(platforms.length===3) score += 20;
    const recHits = platforms.filter(p=>recommended.includes(p)).length;
    score += recHits * 12;
    if(pillars.length===3) score += 20;
    if(eventChoiceIdx!==null) score += 12;
    if(step>=2) score += 8;
    return Math.max(4, Math.min(100, Math.round(score)));
  }, [platforms, pillars, eventChoiceIdx, recommended, step]);

  function startMode(m){
    setMode(m);
    if(m==='random') setBrand(randomBusiness());
    else setBrand(null);
    goto(2);
  }

  function confirmOwnBusiness(){
    setBrand({
      name: ownForm.name || 'Your Business',
      industry: ownForm.industry || 'General business',
      audience: ownForm.audience || 'a broad audience',
      budget: ownForm.budget || 'an undefined budget',
      competitors: ownForm.competitors || 'a few similar businesses in the space',
      challenge: ownForm.challenge || 'Growth has been inconsistent.'
    });
    next();
  }

  function confirmPersonalBrand(){
    setBrand({
      name: personalForm.name || 'You',
      expertise: personalForm.expertise || 'your area of expertise',
      audience: personalForm.audience || 'people who want to learn what you know',
      story: personalForm.story || 'a personal journey worth sharing',
      competitors: 'a few people in your space you genuinely admire'
    });
    next();
  }

  function togglePlatform(id){
    setPlatforms(prev=>{
      if(prev.includes(id)) return prev.filter(p=>p!==id);
      if(prev.length>=3) return prev;
      return [...prev, id];
    });
  }
  function togglePillar(id){
    setPillars(prev=>{
      if(prev.includes(id)) return prev.filter(p=>p!==id);
      if(prev.length>=3) return prev;
      return [...prev, id];
    });
  }

  function rollEvent(){
    setEvent(pick(eventPool));
    setEventChoiceIdx(null);
    next();
  }

  function replay(){
    setStep(0); setMode(null); setBrand(null);
    setPlatforms([]); setPillars([]); setEvent(null); setEventChoiceIdx(null);
    setOwnForm({ name:'', industry:'', audience:'', budget:'', competitors:'', challenge:'' });
    setPersonalForm({ name:'', expertise:'', story:'', audience:'' });
    window.scrollTo({top:0, behavior:'smooth'});
  }

  return (
    <div className="app">
      <div className="topbar">
        <div className="brandmark">
          <span className="dot"></span>
          <div>
            Think Like a Marketing Strategist
            <small>Grow This Brand — a strategy simulator</small>
          </div>
        </div>
        <InsightMeter pct={insightPct} label={insightPct < 30 ? "Warming up" : insightPct < 65 ? "Building a case" : "Sharp strategy"} />
      </div>

      <div className="rail">
        {STEP_LABELS.map((label, i)=>(
          <div key={label} className={"rail-step" + (i<step?" done":"") + (i===step?" active":"")}>
            {String(i+1).padStart(2,'0')} {label}
          </div>
        ))}
      </div>

      <div className="step-panel" key={step}>
        {step===0 && <Welcome onStart={()=>goto(1)} />}
        {step===1 && <PathChoice onChoose={startMode} />}
        {step===2 && mode==='own' && <OwnBusinessForm form={ownForm} setForm={setOwnForm} onNext={confirmOwnBusiness} onBack={back} />}
        {step===2 && mode==='personal' && <PersonalBrandForm form={personalForm} setForm={setPersonalForm} onNext={confirmPersonalBrand} onBack={back} />}
        {step===2 && mode==='random' && <RandomClientReveal brand={brand} onReroll={()=>setBrand(randomBusiness())} onNext={next} onBack={back} />}
        {step===3 && <AudienceLesson mode={mode} brand={brand} onNext={next} onBack={back} />}
        {step===4 && <PlatformStep mode={mode} brand={brand} platforms={platforms} toggle={togglePlatform} recommended={recommended} onNext={next} onBack={back} />}
        {step===5 && <PillarStep mode={mode} brand={brand} pillars={pillars} toggle={togglePillar} pool={pillarPool} onNext={next} onBack={back} />}
        {step===6 && <RoadmapStep mode={mode} brand={brand} platforms={platforms} pillars={pillars} onNext={rollEvent} onBack={back} />}
        {step===7 && <CurveballStep mode={mode} brand={brand} event={event} choiceIdx={eventChoiceIdx} setChoiceIdx={setEventChoiceIdx} onNext={next} onBack={back} />}
        {step===8 && <ReportStep mode={mode} brand={brand} platforms={platforms} pillars={pillars} event={event} choiceIdx={eventChoiceIdx} onReplay={replay} onBack={back} />}
      </div>

      <div className="footer-note">Runs fully offline · No data leaves your browser · Replay anytime for a new client</div>
    </div>
  );
}

/* =========================================================
   STEP 0 — WELCOME
   ========================================================= */

function Welcome({ onStart }){
  return (
    <div className="mode-hero">
      <p className="eyebrow" style={{justifyContent:'center'}}>Marketing strategy, from the inside</p>
      <h1 className="headline">Anyone can post content.<br/>Strategists know <em>why</em> to post it.</h1>
      <p className="lede" style={{margin:'0 auto 26px'}}>
        This is a hands-on simulator, not a content generator. You'll take on the role of a marketing
        strategist — understanding an audience, choosing platforms on purpose, building a content plan,
        and reacting to a real curveball. Every step explains <b>what</b> you're doing and <b>why</b> it matters,
        so the thinking sticks even after the session ends.
      </p>
      <div className="why-box" style={{textAlign:'left', maxWidth:640, margin:'0 auto 26px'}}>
        <div className="tag">Why it matters</div>
        <div>
          <p>Most beginners jump straight to "what should I post?" Strategists ask "who is this for, and what does success look like?" first.</p>
          <p>Getting that order right is the entire difference between content that performs and content that just exists.</p>
        </div>
      </div>
      <button className="btn btn-primary" onClick={onStart}>Start the simulation →</button>
    </div>
  );
}

/* =========================================================
   STEP 1 — PATH CHOICE
   ========================================================= */

function PathChoice({ onChoose }){
  const options = [
    { id:'own', icon:'🏢', name:'Use My Own Business', desc:'Bring a real business or idea you already have, and build a strategy for it.' },
    { id:'personal', icon:'🙋', name:'Build My Personal Brand', desc:'No business? Use your own name, expertise and story as the brand instead.' },
    { id:'random', icon:'🎲', name:'A New Client Has Arrived', desc:'A random client gets generated for you — industry, audience, budget, competitors and a live challenge.' }
  ];
  return (
    <div>
      <p className="eyebrow">Step 1 · Choose your path</p>
      <h1 className="headline">Whose brand are we growing?</h1>
      <p className="lede">Every real strategist starts with a brief. Pick how you want yours to arrive.</p>
      <div className="grid">
        {options.map(o=>(
          <button key={o.id} className="opt" onClick={()=>onChoose(o.id)} style={{cursor:'pointer'}}>
            <span className="opt-icon">{o.icon}</span>
            <div className="opt-name">{o.name}</div>
            <div className="opt-desc">{o.desc}</div>
          </button>
        ))}
      </div>
    </div>
  );
}

/* =========================================================
   STEP 2 — BRIEF (three variants)
   ========================================================= */

function OwnBusinessForm({ form, setForm, onNext, onBack }){
  const set = (k) => (e) => setForm({...form, [k]: e.target.value});
  const valid = form.name.trim().length>0;
  return (
    <div>
      <p className="eyebrow">Step 2 · The brief</p>
      <h1 className="headline">Tell me about your business.</h1>
      <p className="lede">Even a rough answer is fine — a strategist works with whatever the client gives them.</p>
      <div className="card">
        <label className="field-label">Business name</label>
        <input className="field" placeholder="e.g. Marigold Studio" value={form.name} onChange={set('name')} />
        <label className="field-label">Industry / what you sell</label>
        <input className="field" placeholder="e.g. Handmade ceramics" value={form.industry} onChange={set('industry')} />
        <label className="field-label">Who is it for?</label>
        <input className="field" placeholder="e.g. Gift-buyers who want something one-of-a-kind" value={form.audience} onChange={set('audience')} />
        <label className="field-label">Rough monthly marketing budget</label>
        <input className="field" placeholder="e.g. $300/month, mostly my own time" value={form.budget} onChange={set('budget')} />
        <label className="field-label">Who else is competing for this audience?</label>
        <input className="field" placeholder="e.g. Etsy sellers, a big-box home store" value={form.competitors} onChange={set('competitors')} />
        <label className="field-label">Biggest current challenge</label>
        <textarea className="field" rows="3" placeholder="e.g. People love it when they see it in person, but nobody finds us online." value={form.challenge} onChange={set('challenge')} />
      </div>
      <WhyBox items={[{tag:'Why it matters', lines:[
        "A strategist can't recommend anything useful without knowing the audience, the constraints and the current problem first.",
        "Vague briefs are normal — part of the job is asking good questions to sharpen them."
      ]}]} />
      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" disabled={!valid} onClick={onNext}>Continue →</button>
      </div>
    </div>
  );
}

function PersonalBrandForm({ form, setForm, onNext, onBack }){
  const set = (k) => (e) => setForm({...form, [k]: e.target.value});
  const valid = form.name.trim().length>0 && form.expertise.trim().length>0;
  return (
    <div>
      <p className="eyebrow">Step 2 · The brief</p>
      <h1 className="headline">You are the brand. Let's define it.</h1>
      <p className="lede">No company needed — your name, your expertise and your story are the product here.</p>
      <div className="card">
        <label className="field-label">Your name (or the name you'll post under)</label>
        <input className="field" placeholder="e.g. Jordan Reyes" value={form.name} onChange={set('name')} />
        <label className="field-label">Your area of expertise</label>
        <input className="field" placeholder="e.g. Helping people switch careers into UX design" value={form.expertise} onChange={set('expertise')} />
        <label className="field-label">Who do you want to reach?</label>
        <input className="field" placeholder="e.g. Mid-career professionals feeling stuck" value={form.audience} onChange={set('audience')} />
        <label className="field-label">What's a short version of your story?</label>
        <textarea className="field" rows="3" placeholder="e.g. Spent 8 years in finance before retraining — now I help others do it faster than I did." value={form.story} onChange={set('story')} />
      </div>
      <WhyBox items={[{tag:'Why it matters', lines:[
        "In a personal brand, the 'product' is your lived expertise — so the brief is really about clarifying your point of view, not a catalog of features.",
        "A specific story beats a generic bio every time: 'I help X do Y' is far stronger than 'I'm passionate about marketing.'"
      ]}]} />
      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" disabled={!valid} onClick={onNext}>Continue →</button>
      </div>
    </div>
  );
}

function RandomClientReveal({ brand, onReroll, onNext, onBack }){
  if(!brand) return null;
  return (
    <div>
      <p className="eyebrow">Step 2 · The brief</p>
      <h1 className="headline">A new client just landed in your inbox.</h1>
      <p className="lede">Here's what they sent over. It's a little messy — real briefs usually are.</p>
      <div className="card card-raised">
        <div className="brief-grid">
          <div className="brief-field"><div className="bf-label">Industry</div><div className="bf-value">{brand.industry}</div></div>
          <div className="brief-field"><div className="bf-label">Target audience</div><div className="bf-value">{brand.audience}</div></div>
          <div className="brief-field"><div className="bf-label">Monthly budget</div><div className="bf-value">{brand.budget}</div></div>
          <div className="brief-field"><div className="bf-label">Competitive landscape</div><div className="bf-value">{brand.competitors}</div></div>
        </div>
        <div className="brief-field" style={{marginTop:12}}>
          <div className="bf-label">Their stated challenge</div>
          <div className="bf-value">{brand.challenge}</div>
        </div>
      </div>
      <WhyBox items={[{tag:'Why it matters', lines:[
        "Real clients rarely arrive with a perfect brief — part of strategy work is reading between the lines of what they actually need.",
        "Notice the challenge statement: it usually hints at the whole strategy problem you're about to solve."
      ]}]} />
      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <button className="btn btn-ghost" onClick={onReroll}>🎲 Different client</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" onClick={onNext}>Take the brief →</button>
      </div>
    </div>
  );
}

/* =========================================================
   STEP 3 — AUDIENCE UNDERSTANDING
   ========================================================= */

function AudienceLesson({ mode, brand, onNext, onBack }){
  const isPersonal = mode==='personal';
  const label = isPersonal ? brand.name : brand.name;
  const promptText = isPersonal
    ? `I'm building a personal brand around ${brand.expertise}. My target audience is: ${brand.audience}. Help me write 3 sentences describing what this audience worries about, what they secretly want, and what would make them trust a stranger's advice on this topic.`
    : `I run a business in ${brand.industry}. My target audience is: ${brand.audience}. Help me write 3 sentences describing what this audience worries about, what they want, and what would make them trust a new brand.`;
  return (
    <div>
      <p className="eyebrow">Step 3 · Understand the {isPersonal ? "audience" : "business & audience"}</p>
      <h1 className="headline">Before any platform or post: who are we actually talking to?</h1>
      <p className="lede">
        {isPersonal
          ? "In a personal brand, your \u201cproduct\u201d is your expertise and story — but it only matters if it solves something real for a specific audience."
          : "Every platform and content choice downstream depends entirely on getting this part right first."}
      </p>

      <div className="card">
        <h2 className="headline" style={{fontSize:17}}>{isPersonal ? "The person" : "The business"}</h2>
        {isPersonal ? (
          <p style={{color:'var(--muted)', fontSize:14, lineHeight:1.6}}>
            <b style={{color:'var(--text)'}}>{brand.name}</b> is building expertise in <b style={{color:'var(--text)'}}>{brand.expertise}</b>.
            Their story: {brand.story}
          </p>
        ) : (
          <p style={{color:'var(--muted)', fontSize:14, lineHeight:1.6}}>
            <b style={{color:'var(--text)'}}>{brand.name}</b> operates in <b style={{color:'var(--text)'}}>{brand.industry}</b>, with a budget of {brand.budget}.
            Current challenge: {brand.challenge}
          </p>
        )}
        <h2 className="headline" style={{fontSize:17, marginTop:16}}>The audience</h2>
        <p style={{color:'var(--muted)', fontSize:14, lineHeight:1.6}}>{brand.audience}</p>
        <h2 className="headline" style={{fontSize:17, marginTop:16}}>{isPersonal ? "People in your space you admire" : "Competitors"}</h2>
        <p style={{color:'var(--muted)', fontSize:14, lineHeight:1.6}}>{brand.competitors}</p>
      </div>

      <WhyBox items={[{tag:'Why it matters', lines:[
        isPersonal
          ? "Naming people you admire (instead of \u201ccompetitors\u201d) reframes the exercise: you're studying what resonates, not fighting for the same territory."
          : "Naming the audience precisely — not \u201ceveryone\u201d — is what makes every later decision (platform, pillar, roadmap) actually defensible.",
        "A strategist who can describe the audience's worries and desires in one sentence almost always outperforms one who can't."
      ]}]} />

      <ClaudeCard
        title="Understanding your audience"
        prompt={promptText}
        why="Notice this prompt asks for worries, wants, and trust — not demographics. Demographics describe who someone is; psychology explains why they'd act."
      />

      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" onClick={onNext}>Continue to platforms →</button>
      </div>
    </div>
  );
}

/* =========================================================
   STEP 4 — PLATFORM CHOICE
   ========================================================= */

function PlatformStep({ mode, brand, platforms, toggle, recommended, onNext, onBack }){
  const isPersonal = mode==='personal';
  const canContinue = platforms.length===3;
  const promptText = isPersonal
    ? `I'm building a personal brand as ${brand.name} around ${brand.expertise}, targeting ${brand.audience}. Which 3 social platforms should I prioritize first, and why — considering that I have limited time each week?`
    : `My business "${brand.name}" is in ${brand.industry}, targeting ${brand.audience}, with a budget of ${brand.budget}. Which 3 social platforms should I prioritize first, and why?`;

  return (
    <div>
      <p className="eyebrow">Step 4 · Choose your platforms</p>
      <h1 className="headline">Pick exactly 3 platforms. Not the trendiest — the rightest.</h1>
      <p className="lede">
        {isPersonal
          ? "For personal brands, LinkedIn, X, YouTube and newsletters usually carry more weight than visual platforms — authority compounds differently than aesthetics."
          : "Every platform has a real trade-off. The goal isn't to be everywhere — it's to be deliberate."}
      </p>

      <div className="grid">
        {PLATFORMS.map(p=>{
          const selected = platforms.includes(p.id);
          const isRec = recommended.includes(p.id);
          return (
            <button key={p.id} className={"opt" + (selected?" selected":"") + (isRec?" recommended":"")} onClick={()=>toggle(p.id)}>
              <span className="opt-icon">{p.icon}</span>
              <div className="opt-name">{p.name}</div>
              <div className="opt-desc">{p.blurb}</div>
              <div className="opt-check">✓</div>
            </button>
          );
        })}
      </div>

      {platforms.length>0 && (
        <div className="card">
          <h2 className="headline" style={{fontSize:15.5, marginBottom:12}}>Why these fit (or don't)</h2>
          {platforms.map(id=>{
            const p = PLATFORMS.find(x=>x.id===id);
            const isRec = recommended.includes(id);
            return (
              <div key={id} className={"selection-feedback " + (isRec ? "good" : "warn")} style={{marginBottom:10}}>
                <b>{p.icon} {p.name}</b> — {isPersonal ? p.personalWhy : p.bizWhy}
                {!isRec && <span> Consider whether this is the best use of limited time and energy right now.</span>}
              </div>
            );
          })}
        </div>
      )}

      <WhyBox items={[{tag:'Why it matters', lines:[
        "Spreading effort across too many platforms is one of the most common beginner mistakes — three focused channels almost always beat six neglected ones.",
        isPersonal ? "Notice which platforms are marked as a recommended fit for personal brands specifically." : "Notice which platforms are marked as a recommended fit for this industry and audience."
      ]}]} />

      <ClaudeCard
        title="Choosing platforms"
        prompt={promptText}
        why="This prompt gives Claude the audience and constraint (time or budget) up front, which is what turns a generic answer into a genuinely useful one."
      />

      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" disabled={!canContinue} onClick={onNext}>
          {canContinue ? "Continue to content pillars →" : `Select ${3-platforms.length} more`}
        </button>
      </div>
    </div>
  );
}

/* =========================================================
   STEP 5 — CONTENT PILLARS
   ========================================================= */

function PillarStep({ mode, brand, pillars, toggle, pool, onNext, onBack }){
  const isPersonal = mode==='personal';
  const canContinue = pillars.length===3;
  const promptText = isPersonal
    ? `I'm ${brand.name}, building a personal brand around ${brand.expertise}. Suggest 3 content pillars I should post about consistently, and explain what goal each one serves (trust, reach, authority, or connection).`
    : `My business "${brand.name}" sells to ${brand.audience}. Suggest 3 content pillars I should post about consistently, and explain what goal each one serves (trust, reach, authority, or conversions).`;

  return (
    <div>
      <p className="eyebrow">Step 5 · Content pillars</p>
      <h1 className="headline">Choose exactly 3 pillars to build the whole content plan on.</h1>
      <p className="lede">
        A content pillar is a recurring theme — not a single post idea, but a category you can return to weekly.
        {isPersonal ? " For a personal brand, story and point-of-view matter as much as pure education." : " Each one below serves a different marketing goal."}
      </p>

      <div className="grid">
        {pool.map(p=>{
          const selected = pillars.includes(p.id);
          return (
            <button key={p.id} className={"opt" + (selected?" selected":"") + (isPersonal && p.personal ? " recommended":"")} onClick={()=>toggle(p.id)}>
              <div className="opt-name">{p.name}</div>
              <div className="opt-desc">{p.desc}</div>
              <div className="opt-desc" style={{marginTop:8, color:'var(--teal)', fontSize:12}}>🎯 {p.goal}</div>
              <div className="opt-check">✓</div>
            </button>
          );
        })}
      </div>

      <WhyBox items={[{tag:'Why it matters', lines:[
        "Picking exactly three forces trade-offs — real strategists can't chase every good idea, they choose the ones that compound.",
        "A strong pillar mix usually balances one pillar that builds trust, one that builds reach, and one that builds relationship or authority."
      ]}]} />

      <ClaudeCard
        title="Choosing content pillars"
        prompt={promptText}
        why="Asking Claude to name the goal behind each pillar (not just the topic) is what turns a content list into an actual strategy."
      />

      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" disabled={!canContinue} onClick={onNext}>
          {canContinue ? "Build the 30-day roadmap →" : `Select ${3-pillars.length} more`}
        </button>
      </div>
    </div>
  );
}

/* =========================================================
   STEP 6 — 30 DAY ROADMAP
   ========================================================= */

function buildRoadmap(mode, brand, platformNames, pillarNames){
  const isPersonal = mode==='personal';
  const platStr = platformNames.join(' and ');
  const [p1,p2,p3] = pillarNames;
  if(isPersonal){
    return [
      { title:'Define & Optimize', goal:`Nail down ${brand.name}'s point of view on ${brand.expertise}, and rewrite the bio/profile on ${platStr} so a stranger understands the niche in five seconds.` },
      { title:'Establish Rhythm', goal:`Start publishing consistently on ${platStr}, leaning on ${p1 || 'the first pillar'} content to build initial trust with new visitors.` },
      { title:'Deepen Connection', goal:`Introduce ${p2 || 'the second pillar'} content to build a more three-dimensional relationship with the audience.` },
      { title:'Expand Reach', goal:`Use ${p3 || 'the third pillar'} content, plus light engagement with peers in the space, to grow beyond the first audience.` }
    ];
  }
  return [
    { title:'Foundation', goal:`Audit ${brand.name}'s current presence, lock in a consistent brand voice, and get profiles fully set up on ${platStr}.` },
    { title:'Build Awareness', goal:`Publish consistently using ${p1 || 'the first pillar'} content to introduce the brand to a cold, unfamiliar audience.` },
    { title:'Build Trust', goal:`Layer in ${p2 || 'the second pillar'} content to turn casual viewers into people who actually follow along.` },
    { title:'Drive Action', goal:`Introduce ${p3 || 'the third pillar'} content with clear calls to action, converting followers into customers.` }
  ];
}

function RoadmapStep({ mode, brand, platforms, pillars, onNext, onBack }){
  const platformNames = platforms.map(id=>PLATFORMS.find(p=>p.id===id).name);
  const pillarNames = pillars.map(id=>CONTENT_PILLARS.find(p=>p.id===id).name);
  const weeks = buildRoadmap(mode, brand, platformNames, pillarNames);
  const isPersonal = mode==='personal';
  const promptText = isPersonal
    ? `I'm ${brand.name}, building a personal brand. My platforms are ${platformNames.join(', ')} and my content pillars are ${pillarNames.join(', ')}. Write a 4-week roadmap with one strategic goal per week — not individual post ideas.`
    : `My business "${brand.name}" is using ${platformNames.join(', ')} with content pillars: ${pillarNames.join(', ')}. Write a 4-week roadmap with one strategic goal per week — not individual post ideas.`;

  return (
    <div>
      <p className="eyebrow">Step 6 · The 30-day roadmap</p>
      <h1 className="headline">Zoom out. What should each week actually accomplish?</h1>
      <p className="lede">A roadmap isn't a content calendar — it's a sequence of goals. The posts come later; the strategy comes first.</p>

      <div className="card">
        {weeks.map((w,i)=>(
          <div className="week" key={i}>
            <div className="wk-num">{String(i+1).padStart(2,'0')}<small>Week</small></div>
            <div>
              <div className="wk-title">{w.title}</div>
              <div className="wk-goal">{w.goal}</div>
            </div>
          </div>
        ))}
      </div>

      <div style={{marginBottom:20}}>
        {platformNames.map(n=><span className="pill teal" key={n}>{n}</span>)}
        {pillarNames.map(n=><span className="pill amber" key={n}>{n}</span>)}
      </div>

      <WhyBox items={[{tag:'Why it matters', lines:[
        "Notice the roadmap never mentions specific posts. Strategists plan direction; execution details follow once the direction is set.",
        isPersonal ? "Week 1 focuses on point-of-view and profile — because in a personal brand, unclear positioning undermines every post that follows." : "Each week builds on the last — awareness before trust, trust before a direct ask."
      ]}]} />

      <ClaudeCard
        title="Building a roadmap"
        prompt={promptText}
        why="Asking for weekly goals (not daily posts) keeps Claude's answer strategic instead of turning into a giant, hard-to-maintain content calendar."
      />

      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" onClick={onNext}>Something unexpected happens →</button>
      </div>
    </div>
  );
}

/* =========================================================
   STEP 7 — CURVEBALL EVENT
   ========================================================= */

function CurveballStep({ mode, brand, event, choiceIdx, setChoiceIdx, onNext, onBack }){
  if(!event) return null;
  const isPersonal = mode==='personal';
  const title = fill(event.title, brand);
  const desc = fill(event.desc, brand);
  const chosen = choiceIdx!==null ? event.choices[choiceIdx] : null;

  const promptText = isPersonal
    ? `I'm ${brand.name}, a personal brand around ${brand.expertise}. This just happened: "${title} — ${desc}" How should I respond in the next 24 hours, and what's the risk of each option?`
    : `My business "${brand.name}" just had this happen: "${title} — ${desc}" How should we respond in the next 24 hours, and what's the risk of each option?`;

  return (
    <div>
      <p className="eyebrow">Step 7 · The curveball</p>
      <h1 className="headline">Plans meet reality.</h1>
      <p className="lede">No strategy survives contact with the real world untouched. Here's what just landed.</p>

      <div className="curveball-banner">
        <span className="cb-tag">⚡ Unexpected event</span>
        <h2 className="headline" style={{fontSize:19}}>{title}</h2>
        <p style={{color:'var(--muted)', fontSize:14.5, lineHeight:1.6, margin:0}}>{desc}</p>
      </div>

      <div className="grid" style={{gridTemplateColumns:'1fr'}}>
        {event.choices.map((c,i)=>(
          <button key={i} className={"opt" + (choiceIdx===i?" selected":"")} onClick={()=>setChoiceIdx(i)}>
            <div className="opt-name" style={{fontSize:14.5}}>{fill(c.label, brand)}</div>
            <div className="opt-check">✓</div>
          </button>
        ))}
      </div>

      {chosen && (
        <div className="consequence">
          <b>What happens next:</b> {fill(chosen.consequence, brand)}
        </div>
      )}

      <WhyBox items={[{tag:'Why it matters', lines:[
        "There's rarely a single 'correct' response to a curveball — good strategists weigh trade-offs quickly instead of freezing or overreacting.",
        "Notice that even the 'safe' option here has a cost. Every choice trades something for something else."
      ]}]} />

      <ClaudeCard
        title="Handling a curveball"
        prompt={promptText}
        why="Asking for risk alongside the recommendation forces a more honest, useful answer than just 'what should I do?'"
      />

      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" disabled={choiceIdx===null} onClick={onNext}>See the growth report →</button>
      </div>
    </div>
  );
}

/* =========================================================
   STEP 8 — GROWTH REPORT
   ========================================================= */

function buildReport(mode, brand, platformNames, pillarNames, chosen){
  const isPersonal = mode==='personal';
  const audienceUnderstanding = isPersonal
    ? `${brand.name} is speaking to ${brand.audience}, using lived experience in ${brand.expertise} as proof rather than credentials alone. That specificity is what will make the content feel earned instead of generic.`
    : `${brand.name} is targeting ${brand.audience}, a group defined clearly enough to guide real decisions — not "everyone," which is what makes the rest of this strategy possible.`;

  const platformStrategy = `Focusing on ${platformNames.join(', ')} concentrates effort instead of spreading it thin. ${isPersonal ? "For a personal brand, this mix leans into authority-building platforms rather than purely visual ones." : "This mix was chosen based on where this audience actually spends attention, not just where it's trendy to post."}`;

  const contentStrategy = `The three pillars — ${pillarNames.join(', ')} — work together rather than duplicating each other: each pulls a different lever (trust, reach, or relationship) instead of repeating the same message in different formats.`;

  const growthPotential = isPersonal
    ? `With a clear point of view and consistent posting on the chosen platforms, ${brand.name} has a realistic path to a small, genuinely engaged audience within 30 days — the real growth curve tends to bend upward after week 6 to 8, once trust compounds.`
    : `Given the budget of ${brand.budget}, steady growth is realistic within 30 days, though the biggest gains usually show up after the first full content cycle, once the audience has seen the brand more than a handful of times.`;

  const bestDecision = chosen
    ? `Choosing to "${chosen.label.toLowerCase()}" in response to the curveball was the standout decision — it balanced the moment's urgency against the brand's longer-term credibility.`
    : `Picking exactly three focused platforms and three focused pillars, instead of trying to do everything, was the best decision in this simulation.`;

  const biggestMistake = platformNames.length && !platformNames.every(n => true)
    ? `The easiest mistake to fall into here is treating the 30-day roadmap like a content calendar instead of a sequence of goals — it's tempting to skip straight to "what do I post" before the weekly goals are clear.`
    : `A common mistake at this stage is under-investing in Week 1 groundwork to rush toward visible output sooner.`;

  const lessonsGeneral = [
    `Audience clarity comes first. Every platform, pillar, and content decision downstream only works because the audience was defined precisely instead of broadly.`,
    `Focus beats coverage. Three deliberate platforms and three deliberate pillars will outperform a scattered presence across everything, every time.`,
    `Strategy is a sequence of goals, not a list of posts. The roadmap worked because each week built on the trust the last one earned.`
  ];
  const lessonsPersonal = [
    `Authenticity is a strategy, not just a value. ${brand.name}'s real story is harder to copy than any content format, which makes it the most durable advantage available.`,
    `Consistency compounds slower than it feels like it should. The gap between posting once and posting every week for a month is where most of the visible growth actually happens.`,
    `Niche clarity attracts the right 1,000 people faster than broad appeal attracts the wrong 10,000. A sharp point of view on ${brand.expertise} will do more than trying to appeal to everyone.`
  ];

  return {
    audienceUnderstanding, platformStrategy, contentStrategy, growthPotential,
    bestDecision, biggestMistake,
    lessons: isPersonal ? lessonsPersonal : lessonsGeneral
  };
}

function ReportStep({ mode, brand, platforms, pillars, event, choiceIdx, onReplay, onBack }){
  const isPersonal = mode==='personal';
  const platformNames = platforms.map(id=>PLATFORMS.find(p=>p.id===id).name);
  const pillarNames = pillars.map(id=>CONTENT_PILLARS.find(p=>p.id===id).name);
  const chosen = event && choiceIdx!==null ? { ...event.choices[choiceIdx], label: fill(event.choices[choiceIdx].label, brand) } : null;
  const report = buildReport(mode, brand, platformNames, pillarNames, chosen);

  const promptText = isPersonal
    ? `Act as a marketing strategist. I'm ${brand.name}, building a personal brand around ${brand.expertise} for ${brand.audience}. My platforms: ${platformNames.join(', ')}. My content pillars: ${pillarNames.join(', ')}. Give me a growth report covering: audience understanding, platform strategy, content strategy, growth potential, best decision, biggest risk, and 3 personal branding lessons.`
    : `Act as a marketing strategist. My business "${brand.name}" (${brand.industry}) targets ${brand.audience}. My platforms: ${platformNames.join(', ')}. My content pillars: ${pillarNames.join(', ')}. Give me a growth report covering: audience understanding, platform strategy, content strategy, growth potential, best decision, biggest risk, and 3 marketing lessons.`;

  return (
    <div>
      <p className="eyebrow">Step 8 · Growth report</p>
      <h1 className="headline">The debrief.</h1>
      <p className="lede">Here's how the strategy for {isPersonal ? brand.name : brand.name} actually holds up.</p>

      <div className="report-grid">
        <div className="card report-card"><div className="rc-label">Audience Understanding</div><p>{report.audienceUnderstanding}</p></div>
        <div className="card report-card"><div className="rc-label">Platform Strategy</div><p>{report.platformStrategy}</p></div>
        <div className="card report-card"><div className="rc-label">Content Strategy</div><p>{report.contentStrategy}</p></div>
        <div className="card report-card"><div className="rc-label">Growth Potential</div><p>{report.growthPotential}</p></div>
        <div className="card report-card decision"><div className="rc-label">Best Decision</div><p>{report.bestDecision}</p></div>
        <div className="card report-card mistake"><div className="rc-label">Biggest Mistake to Avoid</div><p>{report.biggestMistake}</p></div>
      </div>

      <div className="card">
        <div className="rc-label" style={{color:'var(--amber)', marginBottom:4}}>Three Marketing Lessons</div>
        <ul className="lessons-list">
          {report.lessons.map((l,i)=><li key={i}>{l}</li>)}
        </ul>
      </div>

      <ClaudeCard
        title="Getting a full strategy from scratch"
        prompt={promptText}
        why="This is the master prompt — the same shape you'd use to get Claude to act as a strategist for a brand-new business or brand at any time."
      />

      <div className="nav-row">
        <button className="btn btn-ghost" onClick={onBack}>← Back</button>
        <div className="spacer"></div>
        <button className="btn btn-primary" onClick={onReplay}>🔁 Replay with a new client</button>
      </div>
    </div>
  );
}

/* =========================================================
   MOUNT
   ========================================================= */

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
</script>
</body>
</html>
