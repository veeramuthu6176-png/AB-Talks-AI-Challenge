<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Marketing Detective</title>
<style>
  /* ============================================================
     MARKETING DETECTIVE — Midnight Blue Edition
     Design note: React+Babel-in-browser was intentionally skipped
     for a case-file app this interaction-heavy (custom drag/drop,
     flip reveals, timed animations across many DOM nodes) — a
     dependency-free vanilla build loads instantly and can never
     fail from a CDN hiccup or an in-browser transpile error when
     opened as a bare local file. Structured as small, reusable
     "component" render functions to keep the same spirit.
     ============================================================ */

  :root{
    --bg-deep:#050810;
    --bg-panel:#0c1220;
    --cork:#141d33;
    --cork-2:#0f1729;
    --line: rgba(180,196,224,0.14);
    --ink:#e9edf6;
    --ink-dim:#93a1bf;
    --ink-faint:#5c6a8a;
    --accent:#5b8dee;
    --accent-2:#8fb8ff;
    --accent-soft: rgba(91,141,238,0.16);
    --silver:#c7d0e0;
    --gold:#d9b25b;
    --thread:#b8433a;
    --success:#4fb583;
    --success-soft: rgba(79,181,131,0.14);
    --danger:#e0605a;
    --danger-soft: rgba(224,96,90,0.14);
    --paper:#eef0f4;
    --paper-2:#e4e8ef;
    --paper-ink:#20263a;
    --shadow-deep: 0 30px 60px -20px rgba(0,0,0,0.7);
    --font-display: Georgia, 'Times New Roman', 'Iowan Old Style', serif;
    --font-mono: 'Courier New', Courier, monospace;
    --font-ui: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  }

  *{ box-sizing:border-box; }
  html,body{ margin:0; padding:0; }
  body{
    background:
      radial-gradient(ellipse at 20% -10%, rgba(91,141,238,0.10), transparent 55%),
      radial-gradient(ellipse at 100% 10%, rgba(91,141,238,0.06), transparent 45%),
      var(--bg-deep);
    color:var(--ink);
    font-family:var(--font-ui);
    min-height:100vh;
    -webkit-font-smoothing:antialiased;
  }
  @media (prefers-reduced-motion: reduce){
    *{ animation-duration:0.001ms !important; animation-iteration-count:1 !important; transition-duration:0.001ms !important; }
  }

  ::-webkit-scrollbar{ width:10px; height:10px; }
  ::-webkit-scrollbar-track{ background:var(--bg-panel); }
  ::-webkit-scrollbar-thumb{ background:var(--cork); border-radius:8px; }

  a, button{ font-family:inherit; }
  button{ cursor:pointer; }
  button:focus-visible, [tabindex]:focus-visible, .evidence-note:focus-visible, .clue-scrap:focus-visible{
    outline:2px solid var(--accent-2);
    outline-offset:3px;
  }

  /* ---------- App shell ---------- */
  #app{
    max-width:1180px;
    margin:0 auto;
    padding:22px 20px 60px;
    min-height:100vh;
  }

  .agency-header{
    display:flex; align-items:baseline; justify-content:space-between;
    gap:16px; flex-wrap:wrap;
    padding-bottom:16px; margin-bottom:18px;
    border-bottom:1px solid var(--line);
  }
  .agency-header .brand{
    font-family:var(--font-display);
    font-size:22px; letter-spacing:0.5px;
  }
  .agency-header .brand b{ color:var(--accent-2); font-weight:700; }
  .agency-header .tag{
    font-family:var(--font-mono);
    font-size:11px; letter-spacing:2px; text-transform:uppercase;
    color:var(--ink-faint);
  }

  /* ---------- Progress rail ---------- */
  .rail{
    display:flex; align-items:center; gap:4px;
    margin:0 0 26px;
    overflow-x:auto;
    padding-bottom:4px;
  }
  .rail-step{
    display:flex; align-items:center; gap:8px;
    padding:8px 12px 8px 10px;
    border-radius:20px;
    border:1px solid var(--line);
    background:var(--bg-panel);
    white-space:nowrap;
    transition:all .35s ease;
  }
  .rail-step .dot{
    width:9px; height:9px; border-radius:50%;
    background:var(--ink-faint);
    transition:all .35s ease;
    flex:none;
  }
  .rail-step .label{
    font-family:var(--font-mono);
    font-size:11px; letter-spacing:1px; text-transform:uppercase;
    color:var(--ink-faint);
  }
  .rail-step.active{ border-color:var(--accent); box-shadow:0 0 0 1px var(--accent-soft), 0 0 18px -4px var(--accent-soft); }
  .rail-step.active .dot{ background:var(--accent-2); box-shadow:0 0 8px 1px var(--accent-2); }
  .rail-step.active .label{ color:var(--accent-2); }
  .rail-step.done .dot{ background:var(--success); }
  .rail-step.done .label{ color:var(--silver); }
  .rail-connector{ width:16px; height:1px; background:var(--line); flex:none; }

  .screen{ animation:fadeIn .5s ease both; }
  @keyframes fadeIn{ from{opacity:0; transform:translateY(8px);} to{opacity:1; transform:translateY(0);} }

  /* ---------- Buttons ---------- */
  .btn{
    display:inline-flex; align-items:center; gap:8px;
    padding:12px 22px;
    border-radius:8px;
    border:1px solid var(--accent);
    background:linear-gradient(180deg, rgba(91,141,238,0.22), rgba(91,141,238,0.08));
    color:var(--accent-2);
    font-size:14px; font-weight:600; letter-spacing:0.3px;
    transition:all .2s ease;
    box-shadow:0 0 0 0 var(--accent-soft);
  }
  .btn:hover{ background:linear-gradient(180deg, rgba(91,141,238,0.34), rgba(91,141,238,0.14)); box-shadow:0 0 22px -4px var(--accent-soft); transform:translateY(-1px); }
  .btn:active{ transform:translateY(0); }
  .btn[disabled]{ opacity:0.35; cursor:not-allowed; transform:none; box-shadow:none; }
  .btn.ghost{ background:transparent; border-color:var(--line); color:var(--ink-dim); }
  .btn.ghost:hover{ border-color:var(--silver); color:var(--silver); box-shadow:none; }
  .btn.gold{ border-color:var(--gold); color:var(--gold); background:linear-gradient(180deg, rgba(217,178,91,0.2), rgba(217,178,91,0.06)); }
  .btn.gold:hover{ box-shadow:0 0 22px -4px rgba(217,178,91,0.4); }

  /* ============================================================
     SCREEN 1 — CASE ASSIGNMENT
     ============================================================ */
  .assign-wrap{
    display:grid; grid-template-columns: 1.05fr 0.95fr; gap:34px;
    align-items:center;
    min-height:60vh;
  }
  @media (max-width:840px){ .assign-wrap{ grid-template-columns:1fr; } }

  .assign-copy .eyebrow{
    font-family:var(--font-mono); font-size:12px; letter-spacing:3px;
    text-transform:uppercase; color:var(--accent-2); margin-bottom:14px;
  }
  .assign-copy h1{
    font-family:var(--font-display);
    font-size:clamp(32px,5vw,50px);
    line-height:1.08;
    margin:0 0 16px;
  }
  .assign-copy h1 em{ color:var(--accent-2); font-style:normal; }
  .assign-copy p{ color:var(--ink-dim); font-size:15.5px; line-height:1.7; max-width:52ch; margin:0 0 26px; }

  .case-file-cover{
    position:relative;
    background:
      linear-gradient(180deg, rgba(255,255,255,0.02), transparent 30%),
      var(--cork);
    border:1px solid var(--line);
    border-radius:4px;
    padding:34px 30px;
    box-shadow:var(--shadow-deep);
    transform:rotate(-1.2deg);
  }
  .case-file-cover::before{
    content:"";
    position:absolute; inset:10px;
    border:1px dashed rgba(200,208,220,0.18);
    pointer-events:none;
  }
  .stamp-classified{
    position:absolute; top:22px; right:26px;
    font-family:var(--font-mono); font-weight:700; font-size:11px;
    letter-spacing:2px; color:var(--thread);
    border:2px solid var(--thread);
    padding:4px 10px;
    transform:rotate(8deg);
    opacity:0.85;
  }
  .case-file-cover .file-tab{
    font-family:var(--font-mono); font-size:11px; letter-spacing:2px;
    color:var(--ink-faint); text-transform:uppercase; margin-bottom:10px;
  }
  .case-file-cover h2{
    font-family:var(--font-display); font-size:26px; margin:0 0 6px; color:var(--silver);
  }
  .case-file-cover .redacted{
    display:inline-block; background:var(--ink-faint); color:transparent;
    border-radius:3px; user-select:none;
  }
  .case-file-cover .subline{ color:var(--ink-dim); font-size:13.5px; margin-bottom:22px; }
  .brief-grid{ display:grid; grid-template-columns:1fr 1fr; gap:14px 20px; }
  .brief-item .k{ font-family:var(--font-mono); font-size:10.5px; letter-spacing:1.5px; text-transform:uppercase; color:var(--ink-faint); margin-bottom:4px;}
  .brief-item .v{ font-size:14px; color:var(--ink); }

  .pin{
    position:absolute; width:14px; height:14px; border-radius:50%;
    background:radial-gradient(circle at 35% 30%, #ff8a7a, var(--thread) 70%);
    box-shadow:0 3px 5px rgba(0,0,0,0.5);
  }

  /* ============================================================
     SCREEN 2 — INVESTIGATION BOARD
     ============================================================ */
  .board-layout{ display:grid; grid-template-columns: 1fr 300px; gap:22px; align-items:start; }
  @media (max-width:960px){ .board-layout{ grid-template-columns:1fr; } }

  .board-toolbar{
    display:flex; align-items:center; justify-content:space-between; flex-wrap:wrap; gap:12px;
    margin-bottom:16px;
  }
  .board-toolbar h2{ font-family:var(--font-display); font-size:22px; margin:0; }
  .board-toolbar .hint{ font-size:12.5px; color:var(--ink-faint); font-family:var(--font-mono); }

  .corkboard{
    position:relative;
    background:
      repeating-linear-gradient(0deg, rgba(0,0,0,0.05) 0px, rgba(0,0,0,0.05) 1px, transparent 1px, transparent 3px),
      repeating-linear-gradient(90deg, rgba(0,0,0,0.04) 0px, rgba(0,0,0,0.04) 1px, transparent 1px, transparent 3px),
      radial-gradient(circle at 15% 20%, rgba(255,255,255,0.03), transparent 40%),
      radial-gradient(circle at 85% 75%, rgba(255,255,255,0.03), transparent 40%),
      linear-gradient(160deg, var(--cork), var(--cork-2));
    border:10px solid #0a0f1c;
    border-radius:6px;
    padding:26px;
    min-height:560px;
    box-shadow: inset 0 0 60px rgba(0,0,0,0.55), var(--shadow-deep);
  }
  .cork-frame-label{
    position:absolute; top:-10px; left:24px;
    background:#0a0f1c; padding:2px 10px; border-radius:0 0 6px 6px;
    font-family:var(--font-mono); font-size:10px; letter-spacing:2px; color:var(--ink-faint); text-transform:uppercase;
  }

  #threadSvg{ position:absolute; inset:0; width:100%; height:100%; pointer-events:none; z-index:1; }
  #threadSvg path{ fill:none; stroke:var(--thread); stroke-width:1.4; opacity:0.55; stroke-linecap:round; }

  .board-grid{
    position:relative; z-index:2;
    display:grid;
    grid-template-columns:repeat(auto-fill, minmax(190px,1fr));
    gap:22px 18px;
  }

  .evidence-note{
    position:relative;
    background: linear-gradient(180deg, var(--paper), var(--paper-2));
    color:var(--paper-ink);
    border-radius:2px;
    padding:16px 14px 14px;
    min-height:150px;
    box-shadow:0 10px 18px -8px rgba(0,0,0,0.55), 0 2px 0 rgba(255,255,255,0.4) inset;
    cursor:grab;
    transform:rotate(var(--rot,-1deg));
    transition:transform .2s ease, box-shadow .2s ease;
    font-family:var(--font-mono);
    user-select:none;
  }
  .evidence-note:hover{ transform:rotate(0deg) translateY(-4px) scale(1.02); box-shadow:0 18px 26px -10px rgba(0,0,0,0.65); z-index:5; }
  .evidence-note.dragging{ opacity:0.35; }
  .evidence-note.collected{ box-shadow:0 0 0 2px var(--success), 0 10px 18px -8px rgba(0,0,0,0.55); }
  .evidence-note .pin{ top:-7px; left:50%; margin-left:-7px; }
  .evidence-note .icon{ font-size:18px; }
  .evidence-note .label{
    font-weight:700; font-size:12px; letter-spacing:0.5px; text-transform:uppercase;
    margin:8px 0 6px; color:#2b3350;
  }
  .evidence-note .snippet{ font-size:12px; line-height:1.5; color:#3a415c; max-height:64px; overflow:hidden; }
  .evidence-note .tap-tip{ margin-top:10px; font-size:10px; color:#6d7597; letter-spacing:0.5px; }
  .evidence-note .check{
    position:absolute; bottom:8px; right:10px;
    color:var(--success); font-size:16px; opacity:0; transform:scale(0.5);
    transition:all .25s ease;
  }
  .evidence-note.collected .check{ opacity:1; transform:scale(1); }

  .clue-scrap{
    position:relative; z-index:2;
    background:#151e33;
    border:1px dashed var(--gold);
    border-radius:2px;
    padding:12px 12px 10px;
    min-height:80px;
    display:flex; flex-direction:column; align-items:flex-start; justify-content:center;
    cursor:pointer;
    transform:rotate(var(--rot,1deg));
    transition:transform .25s ease;
  }
  .clue-scrap .glass{ font-size:18px; }
  .clue-scrap .ctitle{ font-family:var(--font-mono); font-size:10.5px; letter-spacing:1.5px; color:var(--gold); margin-top:6px; text-transform:uppercase; }
  .clue-scrap .ctext{ font-size:12.5px; color:var(--ink); line-height:1.5; margin-top:6px; display:none; }
  .clue-scrap.revealed{ background:linear-gradient(180deg, rgba(217,178,91,0.14), rgba(217,178,91,0.05)); }
  .clue-scrap.revealed .ctext{ display:block; }
  .clue-scrap.revealed .tap-hint{ display:none; }
  .clue-scrap:hover{ transform:rotate(0deg) translateY(-2px); }
  .clue-scrap .tap-hint{ font-size:10px; color:var(--ink-faint); margin-top:6px; }

  .dropzone{
    background:var(--bg-panel);
    border:1px solid var(--line);
    border-radius:8px;
    padding:18px;
    min-height:220px;
    transition: box-shadow .25s ease, border-color .25s ease;
  }
  .dropzone.over{ border-color:var(--accent); box-shadow:0 0 0 1px var(--accent-soft), 0 0 26px -6px var(--accent-soft); }
  .dropzone h3{ font-family:var(--font-display); font-size:16px; margin:0 0 4px; }
  .dropzone .sub{ font-size:11.5px; color:var(--ink-faint); font-family:var(--font-mono); margin-bottom:14px; }
  .folder-slot{
    display:flex; align-items:center; gap:8px;
    padding:8px 8px; border-bottom:1px dashed var(--line);
    font-size:12.5px; color:var(--ink-faint);
  }
  .folder-slot.filled{ color:var(--silver); }
  .folder-slot .mark{ width:16px; text-align:center; color:var(--success); }

  .sidebar-panel{
    margin-top:18px;
    background:var(--bg-panel); border:1px solid var(--line); border-radius:8px; padding:16px;
  }
  .sidebar-panel h4{ font-family:var(--font-display); font-size:14px; margin:0 0 10px; color:var(--silver); }
  .progress-bar-mini{ height:6px; border-radius:4px; background:var(--cork); overflow:hidden; margin-bottom:8px; }
  .progress-bar-mini > i{ display:block; height:100%; background:linear-gradient(90deg, var(--accent), var(--accent-2)); transition:width .4s ease; }
  .mini-label{ font-family:var(--font-mono); font-size:11px; color:var(--ink-faint); }

  .board-footer{ display:flex; justify-content:flex-end; margin-top:20px; }

  /* ============================================================
     SCREEN 3 — SOLVE THE CASE
     ============================================================ */
  .solve-wrap{ max-width:820px; margin:0 auto; }
  .solve-wrap h2{ font-family:var(--font-display); font-size:26px; text-align:center; margin:0 0 6px; }
  .solve-wrap .sub{ text-align:center; color:var(--ink-dim); font-size:14px; margin-bottom:30px; }

  .lineup{
    display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:16px;
    margin-bottom:26px;
  }
  .suspect-card{
    background:var(--bg-panel);
    border:1px solid var(--line);
    border-radius:8px;
    padding:16px;
    cursor:grab;
    transition:all .2s ease;
    position:relative;
  }
  .suspect-card .num{
    font-family:var(--font-mono); font-size:10.5px; color:var(--ink-faint); letter-spacing:1.5px;
  }
  .suspect-card p{ font-size:13.5px; line-height:1.55; color:var(--ink); margin:8px 0 0; }
  .suspect-card:hover{ border-color:var(--accent); transform:translateY(-3px); box-shadow:0 14px 24px -12px rgba(0,0,0,0.6); }
  .suspect-card.selected{ border-color:var(--accent-2); box-shadow:0 0 0 1px var(--accent-2), 0 0 24px -6px var(--accent-soft); background:linear-gradient(180deg, rgba(91,141,238,0.1), transparent); }
  .suspect-card.dragging{ opacity:0.35; }

  .accuse-slot{
    border:2px dashed var(--line);
    border-radius:10px;
    padding:24px;
    text-align:center;
    color:var(--ink-faint);
    font-family:var(--font-mono); font-size:12.5px; letter-spacing:1px;
    margin-bottom:22px;
    transition:all .25s ease;
  }
  .accuse-slot.filled{
    border-style:solid; border-color:var(--accent);
    color:var(--ink); text-align:left; font-family:var(--font-ui); font-size:14px;
    background:var(--accent-soft);
  }
  .accuse-slot.over{ border-color:var(--accent-2); box-shadow:0 0 26px -6px var(--accent-soft); }

  .solve-actions{ display:flex; justify-content:center; gap:12px; }

  /* ============================================================
     SCREEN 4 — CASE CLOSED
     ============================================================ */
  .closed-wrap{
    min-height:60vh; display:flex; flex-direction:column; align-items:center; justify-content:center; text-align:center;
    position:relative;
  }
  .stamp{
    font-family:var(--font-display); font-weight:700;
    font-size:clamp(38px,8vw,74px);
    letter-spacing:4px;
    border:6px double currentColor;
    padding:10px 34px;
    border-radius:8px;
    transform:scale(0) rotate(-8deg);
    animation:stampIn .6s cubic-bezier(.2,1.6,.4,1) .15s forwards;
  }
  .stamp.solved{ color:var(--success); }
  .stamp.unsolved{ color:var(--danger); }
  @keyframes stampIn{
    0%{ transform:scale(2.2) rotate(-8deg); opacity:0; }
    60%{ transform:scale(0.94) rotate(-8deg); opacity:1; }
    100%{ transform:scale(1) rotate(-8deg); opacity:1; }
  }
  .closed-wrap .cw-sub{ margin-top:26px; color:var(--ink-dim); font-size:15px; max-width:46ch; opacity:0; animation:fadeIn .6s ease .5s forwards; }
  .closed-wrap .cw-actions{ margin-top:30px; opacity:0; animation:fadeIn .6s ease .7s forwards; }

  .ink-particle{
    position:absolute; border-radius:50%;
    background:var(--accent-2); opacity:0;
    animation:pop .9s ease forwards;
  }
  @keyframes pop{
    0%{ opacity:0.9; transform:translate(0,0) scale(1); }
    100%{ opacity:0; transform:translate(var(--tx), var(--ty)) scale(0.2); }
  }

  /* ============================================================
     SCREEN 5 — LEARNING REPORT
     ============================================================ */
  .report-wrap{ max-width:860px; margin:0 auto; }
  .report-header{ text-align:center; margin-bottom:26px; }
  .report-header .rank{
    display:inline-block; font-family:var(--font-mono); font-size:11px; letter-spacing:3px;
    text-transform:uppercase; color:var(--gold); border:1px solid var(--gold); border-radius:20px; padding:6px 16px; margin-bottom:14px;
  }
  .report-header h2{ font-family:var(--font-display); font-size:28px; margin:0 0 6px; }
  .report-header p{ color:var(--ink-dim); font-size:14px; }

  .report-grid{ display:grid; grid-template-columns:1fr 1fr; gap:18px; margin-bottom:20px; }
  @media (max-width:760px){ .report-grid{ grid-template-columns:1fr; } }

  .report-card{
    background:var(--bg-panel); border:1px solid var(--line); border-radius:10px; padding:20px;
  }
  .report-card h4{
    font-family:var(--font-mono); font-size:11px; letter-spacing:2px; text-transform:uppercase;
    color:var(--accent-2); margin:0 0 12px;
  }
  .report-card p{ font-size:14px; line-height:1.7; color:var(--ink); margin:0; }
  .report-card ul{ margin:0; padding-left:18px; }
  .report-card li{ font-size:13.5px; line-height:1.7; color:var(--ink); margin-bottom:6px; }

  .metrics-chart{
    background:var(--bg-panel); border:1px solid var(--line); border-radius:10px; padding:20px; margin-bottom:20px;
  }
  .metrics-chart h4{ font-family:var(--font-mono); font-size:11px; letter-spacing:2px; text-transform:uppercase; color:var(--accent-2); margin:0 0 16px; }
  .bar-row{ display:grid; grid-template-columns:110px 1fr 70px; align-items:center; gap:10px; margin-bottom:10px; }
  .bar-row .bl{ font-size:11.5px; color:var(--ink-faint); font-family:var(--font-mono); }
  .bar-track{ height:9px; border-radius:5px; background:var(--cork); overflow:hidden; }
  .bar-fill{ height:100%; width:0%; border-radius:5px; background:linear-gradient(90deg, var(--accent), var(--accent-2)); transition:width 1s cubic-bezier(.2,.8,.2,1); }
  .bar-row .bv{ font-size:12px; color:var(--silver); text-align:right; font-family:var(--font-mono); }

  .report-actions{ display:flex; justify-content:center; gap:12px; margin-top:8px; }

  .footer-note{ text-align:center; color:var(--ink-faint); font-family:var(--font-mono); font-size:11px; letter-spacing:1px; margin-top:40px; }

  @media (max-width:600px){
    .board-toolbar{ flex-direction:column; align-items:flex-start; }
    .brief-grid{ grid-template-columns:1fr; }
  }
</style>
</head>
<body>
<div id="app"></div>

<script>
(function(){
  "use strict";

  /* ============================================================
     CASE DATA
     ============================================================ */
  const CASES = [
    {
      company:"LumeWear", industry:"Fashion E-commerce (Activewear)",
      objective:"Increase online sales of the new yoga line by 25% in Q3.",
      audience:"Women aged 18-34, urban, wellness-focused, moderate-to-high disposable income.",
      channels:["Instagram Ads","Influencer Marketing","Email"],
      budget:[["Instagram Ads",50],["Influencers",30],["Email",15],["Other",5]],
      metrics:{reach:"1.2M", ctr:"0.4%", engagement:"2.1%", conversions:"310", sales:"$18,600"},
      comments:[
        "Beautiful ad, but the site crashed when I tried to buy.",
        "The price didn't match what the influencer quoted.",
        "Loved the video but couldn't find the product on the site."
      ],
      social:"Instagram: 45K followers reached via influencer posts, but on-post engagement sits at just 1.8% and most comments ask where to actually find the product on the website.",
      mistake:"A broken, mismatched checkout funnel meant ads drove strong attention but the website failed to convert that interest into a purchase.",
      clues:[
        "Click-through rate (0.4%) is healthy for the category, showing the ad creative itself was compelling.",
        "The conversion count is extremely low relative to reach despite the decent CTR — a sharp drop-off is happening after the click.",
        "Multiple customer comments point to technical or site issues, not the ad content or product itself."
      ],
      options:[
        "A broken, mismatched checkout funnel meant ads drove strong attention but the website failed to convert that interest into a purchase.",
        "The ad creative was low quality and failed to attract any attention.",
        "The target audience selected for the campaign was fundamentally wrong for the product.",
        "The budget was too small to generate meaningful reach."
      ],
      improvements:[
        "QA-test landing pages and the full checkout flow before every launch.",
        "Ensure influencer-quoted pricing or promo codes match what's live on the site.",
        "Send ad traffic to a dedicated product landing page, not the general homepage."
      ]
    },
    {
      company:"BrightDesk", industry:"B2B SaaS (Productivity Software)",
      objective:"Generate 500 qualified leads for a new project-management tool.",
      audience:"Small business owners and team leads, aged 25-45.",
      channels:["LinkedIn Ads","Google Search Ads","Webinars"],
      budget:[["LinkedIn Ads",40],["Google Ads",40],["Webinar Production",20]],
      metrics:{reach:"300K", ctr:"1.2%", engagement:"3.0%", conversions:"90 leads", sales:"12 demo bookings"},
      comments:[
        "Signed up for the webinar but never got the link.",
        "Ad looked great but the signup form kept giving an error.",
        "Interesting product, but pricing wasn't clear anywhere on the landing page."
      ],
      social:"LinkedIn page comments largely praise the ad copy and visuals, but several ask 'how do I actually sign up' — suggesting friction after the click.",
      mistake:"A broken lead-capture form and missing automated follow-up emails caused interested prospects to fall through the cracks after clicking.",
      clues:[
        "CTR (1.2%) is strong for B2B, confirming targeting and messaging reached the right people.",
        "There is a massive drop between clicks and completed leads — only 90 leads from a large, well-targeted click pool.",
        "Comments specifically cite form errors and a missing webinar link, not disinterest in the product."
      ],
      options:[
        "A broken lead-capture form and missing automated follow-up emails caused interested prospects to fall through the cracks after clicking.",
        "LinkedIn targeting missed the intended audience entirely.",
        "The ad budget was split too evenly across channels and diluted results.",
        "The product itself had no real market demand."
      ],
      improvements:[
        "Test every form across browsers and devices before a campaign goes live.",
        "Automate confirmation and reminder emails for webinar signups.",
        "Add clear, visible pricing information on the landing page to reduce hesitation."
      ]
    },
    {
      company:"QuickBite", industry:"Food Delivery App",
      objective:"Boost first-time orders in three newly launched cities by 40%.",
      audience:"Urban professionals aged 22-40 with busy schedules.",
      channels:["TikTok Ads","Local Radio","Push Notifications"],
      budget:[["TikTok Ads",45],["Local Radio",35],["Push Notifications",20]],
      metrics:{reach:"900K", ctr:"0.9%", engagement:"5.5%", conversions:"620 signups", sales:"$9,300 (low avg. order value)"},
      comments:[
        "Got the app because of a TikTok deal, but the discount code didn't work in my city.",
        "Great video ads, but the delivery fee was way more than expected.",
        "Confusing which cities the promo actually applied to."
      ],
      social:"The TikTok spot was widely shared nationally (well beyond the 3 launch cities), and its comment section is dominated by confusion over promo code validity.",
      mistake:"The promo code was advertised nationally but only valid in the three launch cities, without clear geo-targeting or messaging, causing widespread frustration.",
      clues:[
        "Engagement rate (5.5%) is excellent, showing the content resonated far beyond the intended local audience.",
        "Sales dollar value is unusually low despite decent signup numbers — a sign of abandoned or minimum-effort orders.",
        "Comments repeatedly reference promo code issues tied specifically to location."
      ],
      options:[
        "The promo code was advertised nationally but only valid in the three launch cities, without clear geo-targeting or messaging, causing widespread frustration.",
        "The TikTok content itself failed to engage viewers.",
        "Local radio ads had almost no reach in the target cities.",
        "The app's design was too complicated for new users to navigate."
      ],
      improvements:[
        "Geo-restrict ad delivery to match the cities where the promo is actually valid.",
        "State city eligibility clearly within the ad creative and caption.",
        "Auto-detect user location in-app and gracefully explain when a code doesn't apply."
      ]
    },
    {
      company:"PulseFit", industry:"Fitness App Subscription",
      objective:"Increase premium subscription signups by 30% during the New Year season.",
      audience:"New Year resolution-setters, aged 20-45, general fitness interest.",
      channels:["YouTube Ads","Facebook Ads","App Store Featuring"],
      budget:[["YouTube Ads",30],["Facebook Ads",50],["ASO / Featuring",20]],
      metrics:{reach:"2M", ctr:"0.6%", engagement:"2.8%", conversions:"4,800 trials", sales:"210 paid conversions (~4% trial-to-paid)"},
      comments:[
        "Started the free trial but forgot to cancel and got charged — annoyed.",
        "App kept crashing mid-workout so I gave up on it.",
        "Way too many notifications pushing me to upgrade."
      ],
      social:"Facebook comments show many people started the trial, but a recurring thread of complaints about crashes and constant upgrade prompts during the trial period.",
      mistake:"Poor in-app experience during the trial — frequent crashes and aggressive upsell notifications — drove users away before they ever converted to paid.",
      clues:[
        "Trial signups (4,800) comfortably beat expectations, proving acquisition and the offer itself worked well.",
        "Trial-to-paid conversion sits at roughly 4%, unusually low given how strong signups were.",
        "Comments focus on in-app stability and notification fatigue, not on the ads that brought people in."
      ],
      options:[
        "Poor in-app experience during the trial — frequent crashes and aggressive upsell notifications — drove users away before they ever converted to paid.",
        "Acquisition targeting attracted a completely wrong demographic.",
        "Too much of the ad spend went to YouTube instead of Facebook.",
        "The App Store listing used the wrong keywords."
      ],
      improvements:[
        "Fix core app stability issues before scaling any further acquisition spend.",
        "Redesign onboarding to build value before pushing an upgrade.",
        "Shift to behavior-based upsell timing instead of blanket daily notifications."
      ]
    },
    {
      company:"Glow & Co", industry:"Beauty & Skincare",
      objective:"Launch a new skincare serum and generate 1,000 pre-orders.",
      audience:"Skincare enthusiasts aged 25-40, mid-to-high income.",
      channels:["Instagram Influencers","Email List","Pinterest Ads"],
      budget:[["Influencers",60],["Email",25],["Pinterest Ads",15]],
      metrics:{reach:"750K", ctr:"0.3%", engagement:"1.1%", conversions:"140", sales:"$6,200"},
      comments:[
        "The influencer's post didn't even look like her, felt fake.",
        "Never heard of this brand before, not sure I trust it.",
        "Too many sponsored posts about this — feels like an ad overload."
      ],
      social:"Despite large influencer follower counts driving big reach numbers, on-post engagement rate is unusually low, and comments repeatedly question authenticity.",
      mistake:"Influencer selection didn't fit the brand, and the volume of near-identical sponsored posts made the campaign feel like generic advertising rather than a trusted recommendation.",
      clues:[
        "Reach is large thanks to influencer follower counts, but engagement rate is unusually low for influencer content.",
        "Comments explicitly describe the posts as 'feeling fake' or like 'ad overload.'",
        "Lower-risk owned channels (email, Pinterest) received a much smaller share of budget despite the trust problem surfacing on influencer content."
      ],
      options:[
        "Influencer selection didn't fit the brand, and the volume of near-identical sponsored posts made the campaign feel like generic advertising rather than a trusted recommendation.",
        "Pinterest ads had no visual appeal at all.",
        "The email list used for the campaign was outdated and inactive.",
        "The serum was priced far too high for the target audience."
      ],
      improvements:[
        "Choose fewer, better-aligned micro-influencers with genuine product fit.",
        "Encourage authentic, less scripted content over uniform sponsored posts.",
        "Shift more budget toward higher-trust owned channels like email and reviews."
      ]
    },
    {
      company:"Trailhead Travel", industry:"Adventure Travel Agency",
      objective:"Increase bookings for a new adventure tour package by 20%.",
      audience:"Adventure travelers aged 28-50 with disposable income.",
      channels:["Google Search Ads","Facebook Retargeting","Travel Blog Partnerships"],
      budget:[["Google Search Ads",45],["Facebook Retargeting",35],["Blog Partnerships",20]],
      metrics:{reach:"400K", ctr:"2.1%", engagement:"4.0%", conversions:"95 bookings", sales:"$47,500"},
      comments:[
        "Loved the blog post, but the booking page took forever to load.",
        "Kept seeing the same ad over and over — annoying.",
        "The booking process needed too many steps, I gave up halfway."
      ],
      social:"Facebook retargeting frequency per user is very high, and several comments call out seeing the same ad repeatedly.",
      mistake:"A slow, multi-step booking process combined with excessive retargeting frequency created friction and ad fatigue that caused interested travelers to abandon before completing a booking.",
      clues:[
        "CTR (2.1%) is very high, showing search ads and blog content generated strong genuine interest.",
        "Booking conversions are low relative to that strong CTR, meaning the drop-off happens after arriving at the booking page.",
        "Comments cite slow load times, too many steps, and repetitive ads — friction, not lack of interest."
      ],
      options:[
        "A slow, multi-step booking process combined with excessive retargeting frequency created friction and ad fatigue that caused interested travelers to abandon before completing a booking.",
        "Search ad keywords were poorly chosen and irrelevant.",
        "Blog partnerships reached an audience with no travel interest.",
        "The budget ignored retargeting entirely."
      ],
      improvements:[
        "Simplify the booking flow into fewer, faster-loading steps.",
        "Cap retargeting frequency per user to avoid ad fatigue.",
        "Add abandoned-booking email reminders that save the traveler's progress."
      ]
    },
    {
      company:"MindLeap", industry:"EdTech (Online Coding Bootcamp)",
      objective:"Drive 2,000 free trial signups for a new coding bootcamp.",
      audience:"Career changers aged 22-35 seeking to upskill.",
      channels:["YouTube Pre-roll Ads","Reddit Ads","Referral Program"],
      budget:[["YouTube Pre-roll",50],["Reddit Ads",30],["Referral Incentives",20]],
      metrics:{reach:"1.5M", ctr:"0.5%", engagement:"1.5%", conversions:"2,100 trials (goal exceeded)", sales:"38 paid enrollments"},
      comments:[
        "Signed up for the free trial just for the referral bonus, wasn't really interested in coding.",
        "Curriculum seemed too advanced for a total beginner like me.",
        "Got the gift card and cancelled right after."
      ],
      social:"Reddit comment threads talk mostly about the referral gift card itself rather than the actual coding curriculum.",
      mistake:"The referral incentive attracted signups motivated by the reward rather than genuine interest, inflating trial volume while paid conversion stayed extremely low.",
      clues:[
        "Trial signups (2,100) blew past the 2,000 goal, looking like a huge win on the surface.",
        "Paid enrollment conversion is under 2% despite the massive trial pool — most trials never intended to continue.",
        "Comments reveal signups were largely driven by the referral reward, not the course content."
      ],
      options:[
        "The referral incentive attracted signups motivated by the reward rather than genuine interest, inflating trial volume while paid conversion stayed extremely low.",
        "YouTube pre-roll ads were skipped too often to have any impact.",
        "The Reddit community rejected the ad outright.",
        "Course pricing was never communicated anywhere."
      ],
      improvements:[
        "Redesign referral rewards to trigger on genuine engagement, like completing a first lesson.",
        "Add a short intent-qualifying question during signup.",
        "Track and optimize for paid conversion rate, not just raw trial volume."
      ]
    },
    {
      company:"Nova Motors", industry:"Automotive Dealership (Electric SUV)",
      objective:"Increase test-drive bookings for a new electric SUV by 35%.",
      audience:"Eco-conscious families aged 30-55, homeowners.",
      channels:["Local TV","Facebook Ads","Direct Mail"],
      budget:[["Local TV",40],["Facebook Ads",35],["Direct Mail",25]],
      metrics:{reach:"600K", ctr:"0.2%", engagement:"1.0%", conversions:"45 bookings", sales:"6 purchases"},
      comments:[
        "Didn't know electric SUVs qualified for the tax credit until I visited — wish the ad mentioned that.",
        "Ad only talked about range and speed, I wanted to know about cost and incentives.",
        "Nice ad, but nothing about financing options."
      ],
      social:"Facebook comments repeatedly ask about pricing, tax incentives, and financing — questions the ad copy never answers.",
      mistake:"Ad messaging focused only on technical specs like range and speed while ignoring the financial questions — price, tax credits, financing — that most influence a family's EV purchase decision.",
      clues:[
        "Reach was solid across all three channels, so awareness of the SUV wasn't the underlying problem.",
        "Both bookings and purchases are very low relative to reach — a persuasion gap, not an awareness gap.",
        "Multiple comments explicitly ask about pricing, incentives, and financing, information absent from the ads."
      ],
      options:[
        "Ad messaging focused only on technical specs like range and speed while ignoring the financial questions — price, tax credits, financing — that most influence a family's EV purchase decision.",
        "Local TV ad slots aired at the wrong time of day.",
        "Direct mail lists were geographically mistargeted.",
        "Facebook ads suffered from technical delivery errors."
      ],
      improvements:[
        "Lead ad copy with cost savings, tax incentives, and financing options.",
        "Add a cost or incentive calculator link directly in the ad.",
        "Train the sales team to proactively cover financing in every booking follow-up call."
      ]
    },
    {
      company:"Pixel Quest", industry:"Mobile Gaming",
      objective:"Reach 100,000 installs for a new puzzle game launch.",
      audience:"Casual mobile-first gamers aged 18-45.",
      channels:["TikTok Ads","In-app Ad Networks","App Store Optimization"],
      budget:[["TikTok Ads",40],["In-app Ad Networks",45],["ASO",15]],
      metrics:{reach:"5M", ctr:"3.5%", engagement:"130,000 installs (goal exceeded)", conversions:"Very low in-app purchases", sales:"Day-7 retention: 4%"},
      comments:[
        "Uninstalled after a day, way too many ads inside the game itself.",
        "Not what I expected from the trailer — totally different gameplay.",
        "Fun for five minutes, then just ads everywhere."
      ],
      social:"The TikTok ad creative is visibly more dynamic and different from the real in-app gameplay; comments directly compare the trailer to the actual product.",
      mistake:"Ad creative oversold gameplay excitement that didn't match the real, ad-heavy app experience, driving huge install volume but destroying retention and monetization.",
      clues:[
        "Install volume vastly exceeded the goal thanks to an unusually high CTR (3.5%) — the ad creative was extremely compelling.",
        "Day-7 retention is only 4% despite the install success, meaning users leave almost immediately after trying the app.",
        "Comments explicitly say the real gameplay didn't match the trailer and that in-app ads were excessive."
      ],
      options:[
        "Ad creative oversold gameplay excitement that didn't match the real, ad-heavy app experience, driving huge install volume but destroying retention and monetization.",
        "ASO keywords failed to rank the app at all.",
        "The in-app ad network delivered fraudulent installs.",
        "The TikTok audience skewed too young for the game."
      ],
      improvements:[
        "Use real, accurate gameplay footage in ad creative.",
        "Reduce in-app ad frequency during the first few sessions to protect early retention.",
        "Optimize campaigns for retention and revenue, not install volume alone."
      ]
    },
    {
      company:"Brew & Bean", industry:"Coffee Chain",
      objective:"Drive foot traffic and app orders for a new seasonal drink by 20%.",
      audience:"Local commuters and students aged 18-35.",
      channels:["Instagram/Facebook Geo-targeted Ads","In-store Posters","App Push Notifications"],
      budget:[["Geo-targeted Social Ads",40],["In-store Posters",20],["Push Notifications",40]],
      metrics:{reach:"250K", ctr:"1.8%", engagement:"3.2%", conversions:"3,100 app orders", sales:"$14,000 (below expected per-order value)"},
      comments:[
        "Got 5 push notifications in one day about the same drink, so annoying I muted the app.",
        "Redeemed the discount but the drink was out of stock at my store.",
        "Cute promo but too many reminders."
      ],
      social:"App store reviews around the campaign period specifically mention notification fatigue tied to the seasonal drink push.",
      mistake:"Excessive push notification frequency caused fatigue and app-muting, while store inventory wasn't aligned with the demand the campaign generated.",
      clues:[
        "CTR and engagement were both healthy, showing the offer and creative genuinely appealed to the audience.",
        "Sales revenue underperformed relative to the number of app orders, consistent with stock-outs limiting real redemptions.",
        "Comments specifically cite notification overload and out-of-stock drinks, not disinterest in the product."
      ],
      options:[
        "Excessive push notification frequency caused fatigue and app-muting, while store inventory wasn't aligned with the demand the campaign generated.",
        "Geo-targeting completely missed nearby commuters.",
        "In-store posters were never actually printed or displayed.",
        "The social ad creative clashed with the brand's usual style."
      ],
      improvements:[
        "Cap and space out push notifications per user per day.",
        "Sync inventory planning with the demand expected from the marketing push.",
        "Personalize notification timing and content based on user behavior instead of blasting everyone identically."
      ]
    }
  ];

  /* ============================================================
     STATE
     ============================================================ */
  const state = {
    screen:"assignment",
    caseIndex:null,
    caseData:null,
    collected:new Set(),
    cluesRevealed:new Set(),
    selectedSuspect:null,
    accused:null,
    correct:null,
    lastCaseIndex:null
  };

  const EVIDENCE_DEFS = [
    {key:"objective", label:"Campaign Objective", icon:"&#127919;"},
    {key:"audience", label:"Target Audience", icon:"&#128101;"},
    {key:"channels", label:"Marketing Channels", icon:"&#128225;"},
    {key:"budget", label:"Budget Allocation", icon:"&#128176;"},
    {key:"metrics", label:"Campaign Metrics", icon:"&#128202;"},
    {key:"comments", label:"Customer Comments", icon:"&#128172;"},
    {key:"social", label:"Social Performance", icon:"&#128241;"}
  ];

  const STEPS = ["Assignment","Board","Investigate","Solve","Closed","Report"];

  function evidenceText(c, key){
    switch(key){
      case "objective": return c.objective;
      case "audience": return c.audience;
      case "channels": return c.channels.join(", ");
      case "budget": return c.budget.map(b=>b[0]+": "+b[1]+"%").join(" · ");
      case "metrics": return "Reach "+c.metrics.reach+" · CTR "+c.metrics.ctr+" · Eng. "+c.metrics.engagement+" · Conv. "+c.metrics.conversions+" · Sales "+c.metrics.sales;
      case "comments": return c.comments.map(x=>'"'+x+'"').join("  ");
      case "social": return c.social;
    }
    return "";
  }

  /* ============================================================
     UTIL
     ============================================================ */
  function el(tag, attrs, children){
    const e = document.createElement(tag);
    if(attrs) for(const k in attrs){
      if(k === "class") e.className = attrs[k];
      else if(k === "html") e.innerHTML = attrs[k];
      else if(k.indexOf("on") === 0 && typeof attrs[k] === "function") e.addEventListener(k.slice(2), attrs[k]);
      else e.setAttribute(k, attrs[k]);
    }
    (children||[]).forEach(c=>{ if(c) e.appendChild(typeof c === "string" ? document.createTextNode(c) : c); });
    return e;
  }
  function shuffle(arr){
    const a = arr.slice();
    for(let i=a.length-1;i>0;i--){
      const j = Math.floor(Math.random()*(i+1));
      [a[i],a[j]] = [a[j],a[i]];
    }
    return a;
  }
  function pickCaseIndex(){
    if(CASES.length === 1) return 0;
    let idx;
    do{ idx = Math.floor(Math.random()*CASES.length); } while(idx === state.lastCaseIndex);
    return idx;
  }

  /* ============================================================
     RENDER: SHELL
     ============================================================ */
  const app = document.getElementById("app");

  function render(){
    app.innerHTML = "";
    app.appendChild(Header());
    if(state.screen !== "assignment") app.appendChild(Rail());
    const screenEl = el("div", {class:"screen", key:state.screen});
    if(state.screen === "assignment") screenEl.appendChild(ScreenAssignment());
    if(state.screen === "board") screenEl.appendChild(ScreenBoard());
    if(state.screen === "solve") screenEl.appendChild(ScreenSolve());
    if(state.screen === "closed") screenEl.appendChild(ScreenClosed());
    if(state.screen === "report") screenEl.appendChild(ScreenReport());
    app.appendChild(screenEl);
    app.appendChild(el("div",{class:"footer-note"},["MARKETING DETECTIVE AGENCY — CASE FILES ARE CONFIDENTIAL"]));
  }

  function Header(){
    return el("div",{class:"agency-header"},[
      el("div",{class:"brand"},[document.createTextNode(""), (function(){const s=el("span",{},[]); s.innerHTML="&#128269; The <b>Marketing</b> Detective Agency"; return s;})()]),
      el("div",{class:"tag"},["EST. WHEREVER THE DATA LEADS"])
    ]);
  }

  function Rail(){
    const order = ["board","board","solve","closed","report"]; // Board covers both Board & Investigate labels
    const activeStepIndex = ({assignment:0, board:2, solve:3, closed:4, report:5})[state.screen];
    const wrap = el("div",{class:"rail"},[]);
    STEPS.forEach((label, i)=>{
      const isDone = i < activeStepIndex;
      const isActive = i === activeStepIndex || (state.screen==="board" && (i===1||i===2));
      const step = el("div",{class:"rail-step"+(isActive?" active":"")+(isDone?" done":"")},[
        el("span",{class:"dot"},[]),
        el("span",{class:"label"},[label])
      ]);
      wrap.appendChild(step);
      if(i < STEPS.length-1) wrap.appendChild(el("div",{class:"rail-connector"},[]));
    });
    return wrap;
  }

  /* ============================================================
     SCREEN 1: ASSIGNMENT
     ============================================================ */
  function ScreenAssignment(){
    const wrap = el("div",{class:"assign-wrap"},[]);

    const copy = el("div",{class:"assign-copy"},[
      el("div",{class:"eyebrow"},["NEW CASE FILE AVAILABLE"]),
      el("h1",{},[]),
      el("p",{},["A marketing campaign underperformed and nobody can agree on why. Reach looks fine on paper. Something in the funnel doesn't add up. Your job: read every piece of evidence, uncover the three hidden clues, and name the real mistake before the client loses faith."]),
    ]);
    copy.querySelector("h1").innerHTML = 'Every campaign <em>tells on itself</em>.<br>You just have to listen.';

    const preview = pickCaseIndex();
    const c = CASES[preview];
    const cover = el("div",{class:"case-file-cover"},[
      el("div",{class:"pin", style:"top:16px;left:50%;margin-left:-7px;"},[]),
      el("div",{class:"stamp-classified"},["ACTIVE CASE"]),
      el("div",{class:"file-tab"},["CASE FILE // UNOPENED"]),
      el("h2",{},[]),
      el("div",{class:"subline"},[c.industry]),
      el("div",{class:"brief-grid"},[
        BriefItem("Objective","Sealed until accepted"),
        BriefItem("Audience","Sealed until accepted"),
        BriefItem("Channels","Sealed until accepted"),
        BriefItem("Status","Awaiting detective")
      ]),
      el("div",{style:"margin-top:26px;"},[
        (function(){
          const b = el("button",{class:"btn gold", onclick:function(){
            state.caseIndex = preview;
            state.caseData = CASES[preview];
            state.lastCaseIndex = preview;
            state.collected = new Set();
            state.cluesRevealed = new Set();
            state.selectedSuspect = null;
            state.accused = null;
            state.correct = null;
            state.screen = "board";
            render();
          }},[]);
          b.innerHTML = "&#128193; Accept the Case";
          return b;
        })()
      ])
    ]);
    cover.querySelector("h2").innerHTML = '<span class="redacted">'+c.company+'</span>';

    wrap.appendChild(copy);
    wrap.appendChild(cover);
    return wrap;
  }
  function BriefItem(k,v){
    return el("div",{class:"brief-item"},[
      el("div",{class:"k"},[k]),
      el("div",{class:"v"},[v])
    ]);
  }

  /* ============================================================
     SCREEN 2: BOARD (investigation + drag/drop + clue reveal)
     ============================================================ */
  let dragKey = null;

  function ScreenBoard(){
    const c = state.caseData;
    const wrap = el("div",{},[]);

    wrap.appendChild(el("div",{class:"board-toolbar"},[
      (function(){ const h=el("h2",{},[]); h.innerHTML = "Case: "+c.company+" &mdash; "+c.industry; return h; })(),
      el("div",{class:"hint"},["DRAG EVIDENCE INTO THE CASE FILE, OR TAP TO PIN IT"])
    ]));

    const layout = el("div",{class:"board-layout"},[]);

    // corkboard
    const board = el("div",{class:"corkboard", id:"corkboard"},[
      el("div",{class:"cork-frame-label"},["EVIDENCE BOARD"]),
      (function(){ const svg=document.createElementNS("http://www.w3.org/2000/svg","svg"); svg.setAttribute("id","threadSvg"); return svg; })()
    ]);

    const grid = el("div",{class:"board-grid"},[]);

    EVIDENCE_DEFS.forEach((def, i)=>{
      const collected = state.collected.has(def.key);
      const note = el("div",{
        class:"evidence-note"+(collected?" collected":""),
        draggable:"true",
        tabindex:"0",
        style:"--rot:"+((i%2===0?-1:1)*(1+ (i%3)))+"deg;",
        ondragstart:function(ev){ dragKey = def.key; note.classList.add("dragging"); ev.dataTransfer.setData("text/plain", def.key); },
        ondragend:function(){ note.classList.remove("dragging"); },
        onclick:function(){ toggleCollect(def.key); },
        onkeydown:function(ev){ if(ev.key==="Enter"||ev.key===" "){ ev.preventDefault(); toggleCollect(def.key); } }
      },[]);
      note.appendChild(el("div",{class:"pin"},[]));
      const iconLine = el("div",{class:"icon"},[]); iconLine.innerHTML = def.icon;
      note.appendChild(iconLine);
      note.appendChild(el("div",{class:"label"},[def.label]));
      note.appendChild(el("div",{class:"snippet"},[evidenceText(c, def.key)]));
      note.appendChild(el("div",{class:"tap-tip"},[collected ? "Pinned to case file" : "Drag to file, or tap to pin"]));
      const check = el("div",{class:"check"},[]); check.innerHTML="&#10003;";
      note.appendChild(check);
      grid.appendChild(note);
    });

    // 3 clue scraps interleaved visually via separate row
    board.appendChild(grid);

    const cluesRow = el("div",{class:"board-grid", style:"margin-top:22px;"},[]);
    c.clues.forEach((clueText, i)=>{
      const revealed = state.cluesRevealed.has(i);
      const scrap = el("div",{
        class:"clue-scrap"+(revealed?" revealed":""),
        style:"--rot:"+((i%2===0?1:-1)*1.5)+"deg;",
        tabindex:"0",
        onclick:function(){ revealClue(i); },
        onkeydown:function(ev){ if(ev.key==="Enter"||ev.key===" "){ ev.preventDefault(); revealClue(i); } }
      },[]);
      const glass = el("div",{class:"glass"},[]); glass.innerHTML="&#128269;";
      scrap.appendChild(glass);
      scrap.appendChild(el("div",{class:"ctitle"},["Supporting Clue "+(i+1)]));
      scrap.appendChild(el("div",{class:"tap-hint"},["Tap to examine"]));
      scrap.appendChild(el("div",{class:"ctext"},[clueText]));
      cluesRow.appendChild(scrap);
    });
    board.appendChild(cluesRow);

    layout.appendChild(board);

    // sidebar: case file dropzone
    const sidebar = el("div",{},[]);
    const dz = el("div",{
      class:"dropzone",
      id:"dropzone",
      ondragover:function(ev){ ev.preventDefault(); dz.classList.add("over"); },
      ondragleave:function(){ dz.classList.remove("over"); },
      ondrop:function(ev){
        ev.preventDefault(); dz.classList.remove("over");
        const key = ev.dataTransfer.getData("text/plain") || dragKey;
        if(key) collectEvidence(key);
      }
    },[
      el("h3",{},["Case File"]),
      el("div",{class:"sub"},["DROP OR PIN EVIDENCE HERE"])
    ]);
    EVIDENCE_DEFS.forEach(def=>{
      const filled = state.collected.has(def.key);
      const slot = el("div",{class:"folder-slot"+(filled?" filled":"")},[
        el("span",{class:"mark"},[filled ? "\u2713" : "\u2022"]),
        el("span",{},[def.label])
      ]);
      dz.appendChild(slot);
    });
    sidebar.appendChild(dz);

    const progressPanel = el("div",{class:"sidebar-panel"},[
      el("h4",{},["Investigation Progress"]),
      el("div",{class:"mini-label"},["Evidence pinned: "+state.collected.size+" / "+EVIDENCE_DEFS.length]),
      el("div",{class:"progress-bar-mini"},[ el("i",{style:"width:"+(state.collected.size/EVIDENCE_DEFS.length*100)+"%"},[]) ]),
      el("div",{class:"mini-label", style:"margin-top:10px;"},["Clues examined: "+state.cluesRevealed.size+" / 3"]),
      el("div",{class:"progress-bar-mini"},[ el("i",{style:"width:"+(state.cluesRevealed.size/3*100)+"%; background:linear-gradient(90deg, var(--gold), #f0d38a);"},[]) ])
    ]);
    sidebar.appendChild(progressPanel);

    layout.appendChild(sidebar);
    wrap.appendChild(layout);

    const allDone = state.collected.size === EVIDENCE_DEFS.length && state.cluesRevealed.size === 3;
    const footer = el("div",{class:"board-footer"},[
      el("button",{class:"btn"+(allDone?"":" ghost"), disabled: allDone ? null : "disabled", onclick:function(){
        if(!allDone) return;
        state.screen = "solve";
        render();
      }},[allDone ? "Proceed to Solve the Case \u2192" : "Pin all evidence & examine all clues first"])
    ]);
    wrap.appendChild(footer);

    return wrap;
  }

  function collectEvidence(key){
    state.collected.add(key);
    render();
    requestAnimationFrame(drawThreads);
  }
  function toggleCollect(key){
    if(state.collected.has(key)) return;
    collectEvidence(key);
  }
  function revealClue(i){
    if(state.cluesRevealed.has(i)) return;
    state.cluesRevealed.add(i);
    render();
    requestAnimationFrame(drawThreads);
  }

  function drawThreads(){
    const board = document.getElementById("corkboard");
    const svg = document.getElementById("threadSvg");
    if(!board || !svg) return;
    svg.innerHTML = "";
    const boardRect = board.getBoundingClientRect();
    const notes = board.querySelectorAll(".evidence-note.collected, .clue-scrap.revealed");
    let prevCenter = null;
    notes.forEach(n=>{
      const r = n.getBoundingClientRect();
      const cx = r.left + r.width/2 - boardRect.left;
      const cy = r.top - boardRect.top;
      if(prevCenter){
        const path = document.createElementNS("http://www.w3.org/2000/svg","path");
        const midY = (prevCenter.y + cy)/2 - 30;
        path.setAttribute("d", "M "+prevCenter.x+" "+prevCenter.y+" Q "+((prevCenter.x+cx)/2)+" "+midY+" "+cx+" "+cy);
        svg.appendChild(path);
      }
      prevCenter = {x:cx, y:cy};
    });
  }

  /* ============================================================
     SCREEN 3: SOLVE
     ============================================================ */
  let shuffledOptions = null;
  let selectedText = null;

  function ScreenSolve(){
    const c = state.caseData;
    if(!shuffledOptions) shuffledOptions = shuffle(c.options);

    const wrap = el("div",{class:"solve-wrap"},[]);
    wrap.appendChild(el("h2",{},["Name the Marketing Mistake"]));
    wrap.appendChild(el("div",{class:"sub"},["Drag the correct suspect into the accusation slot, or tap to select it."]));

    const slot = el("div",{
      class:"accuse-slot"+(selectedText?" filled":""),
      id:"accuseSlot",
      ondragover:function(ev){ ev.preventDefault(); slot.classList.add("over"); },
      ondragleave:function(){ slot.classList.remove("over"); },
      ondrop:function(ev){
        ev.preventDefault(); slot.classList.remove("over");
        const txt = ev.dataTransfer.getData("text/plain");
        if(txt){ selectedText = txt; renderSolveInPlace(); }
      }
    },[ selectedText ? selectedText : "Drop your accusation here" ]);

    const lineup = el("div",{class:"lineup"},[]);
    shuffledOptions.forEach((opt, i)=>{
      const card = el("div",{
        class:"suspect-card"+(selectedText===opt?" selected":""),
        draggable:"true",
        tabindex:"0",
        ondragstart:function(ev){ card.classList.add("dragging"); ev.dataTransfer.setData("text/plain", opt); },
        ondragend:function(){ card.classList.remove("dragging"); },
        onclick:function(){ selectedText = opt; renderSolveInPlace(); },
        onkeydown:function(ev){ if(ev.key==="Enter"||ev.key===" "){ ev.preventDefault(); selectedText = opt; renderSolveInPlace(); } }
      },[
        el("div",{class:"num"},["SUSPECT 0"+(i+1)]),
        el("p",{},[opt])
      ]);
      lineup.appendChild(card);
    });

    wrap.appendChild(slot);
    wrap.appendChild(lineup);

    const actions = el("div",{class:"solve-actions"},[
      el("button",{class:"btn ghost", onclick:function(){ state.screen="board"; render(); }},["\u2190 Back to Board"]),
      el("button",{class:"btn gold"+(selectedText?"":""), disabled: selectedText ? null : "disabled", onclick:function(){
        state.accused = selectedText;
        state.correct = (selectedText === c.mistake);
        state.screen = "closed";
        render();
      }},["Lock In Accusation"])
    ]);
    wrap.appendChild(actions);

    return wrap;
  }

  function renderSolveInPlace(){
    // simplest reliable approach: full re-render, preserving local closures via module-level vars
    render();
  }

  /* ============================================================
     SCREEN 4: CASE CLOSED
     ============================================================ */
  function ScreenClosed(){
    const wrap = el("div",{class:"closed-wrap"},[]);
    const solved = !!state.correct;
    const stamp = el("div",{class:"stamp "+(solved?"solved":"unsolved")},[solved ? "CASE CLOSED" : "CASE REOPENED"]);
    wrap.appendChild(stamp);
    wrap.appendChild(el("div",{class:"cw-sub"},[
      solved
        ? "Sharp eyes, detective. The evidence lines up and the client finally has an answer."
        : "Not quite — the real culprit is still out there. But every good detective learns something from a wrong turn."
    ]));
    const actions = el("div",{class:"cw-actions"},[
      el("button",{class:"btn", onclick:function(){ state.screen="report"; render(); }},["View Full Report \u2192"])
    ]);
    wrap.appendChild(actions);

    // ink particle burst
    setTimeout(()=>{
      for(let i=0;i<18;i++){
        const p = document.createElement("div");
        p.className = "ink-particle";
        const size = 4 + Math.random()*6;
        p.style.width = size+"px";
        p.style.height = size+"px";
        p.style.left = "calc(50% + "+(Math.random()*40-20)+"px)";
        p.style.top = "38%";
        const tx = (Math.random()*400-200)+"px";
        const ty = (Math.random()*260-40)+"px";
        p.style.setProperty("--tx", tx);
        p.style.setProperty("--ty", ty);
        p.style.background = solved ? "var(--success)" : "var(--danger)";
        wrap.appendChild(p);
        setTimeout(()=>p.remove(), 950);
      }
    }, 150);

    return wrap;
  }

  /* ============================================================
     SCREEN 5: LEARNING REPORT
     ============================================================ */
  function ScreenReport(){
    const c = state.caseData;
    const wrap = el("div",{class:"report-wrap"},[]);

    let rank = "Rookie Sleuth";
    if(state.correct && state.cluesRevealed.size===3) rank = "Master Detective";
    else if(state.correct) rank = "Sharp Detective";
    else rank = "Detective in Training";

    const header = el("div",{class:"report-header"},[
      el("div",{class:"rank"},[rank]),
      el("h2",{},["Case Report: "+c.company]),
      el("p",{},[c.industry+" \u00b7 "+(state.correct ? "Correctly solved" : "Not solved on this attempt")])
    ]);
    wrap.appendChild(header);

    const grid = el("div",{class:"report-grid"},[
      el("div",{class:"report-card"},[
        el("h4",{},["What Actually Happened"]),
        el("p",{},[c.mistake])
      ]),
      el("div",{class:"report-card"},[
        el("h4",{},["Your Accusation"]),
        el("p",{},[state.accused || "\u2014"])
      ])
    ]);
    wrap.appendChild(grid);

    const improve = el("div",{class:"report-card", style:"margin-bottom:20px;"},[
      el("h4",{},["Suggested Improvements"]),
      (function(){ const ul=el("ul",{},[]); c.improvements.forEach(i=>ul.appendChild(el("li",{},[i]))); return ul; })()
    ]);
    wrap.appendChild(improve);

    const chart = el("div",{class:"metrics-chart"},[
      el("h4",{},["Campaign Metrics Snapshot"])
    ]);
    const barData = [
      ["Reach", c.metrics.reach, 100],
      ["CTR", c.metrics.ctr, parseFloat(c.metrics.ctr)*20],
      ["Engagement", c.metrics.engagement, parseFloat(c.metrics.engagement)*12],
      ["Conversions", c.metrics.conversions, 55],
      ["Sales", c.metrics.sales, 40]
    ];
    barData.forEach(row=>{
      const pct = Math.max(6, Math.min(100, row[2]||30));
      const barRow = el("div",{class:"bar-row"},[
        el("div",{class:"bl"},[row[0]]),
        el("div",{class:"bar-track"},[ el("div",{class:"bar-fill", "data-pct":pct},[]) ]),
        el("div",{class:"bv"},[String(row[1])])
      ]);
      chart.appendChild(barRow);
    });
    wrap.appendChild(chart);

    const actions = el("div",{class:"report-actions"},[
      el("button",{class:"btn ghost", onclick:function(){ state.screen="solve"; render(); }},["\u2190 Review Solve Screen"]),
      el("button",{class:"btn gold", onclick:function(){
        shuffledOptions = null;
        selectedText = null;
        state.screen = "assignment";
        render();
      }},["Start a New Case \u2192"])
    ]);
    wrap.appendChild(actions);

    setTimeout(()=>{
      wrap.querySelectorAll(".bar-fill").forEach(b=>{
        b.style.width = b.getAttribute("data-pct")+"%";
      });
    }, 80);

    return wrap;
  }

  /* ============================================================
     INIT
     ============================================================ */
  render();
})();
</script>
</body>
</html>
