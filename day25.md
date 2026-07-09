<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Tank — AI Pitch Simulator</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Big+Shoulders+Display:wght@600;700;800;900&family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/canvas-confetti/1.9.2/confetti.browser.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
  :root{
    --bg:#0a0c10;
    --bg-2:#0d1016;
    --panel:#131722;
    --panel-2:#1b2130;
    --panel-border:#262d3d;
    --gold:#d8a93c;
    --gold-dim:#8a7433;
    --teal:#33c9b0;
    --blue:#4fa3e3;
    --coral:#e1735c;
    --red:#e14b4b;
    --green:#4bc97a;
    --text:#edeae2;
    --muted:#8a93a6;
    --muted-2:#5c6579;
    --radius:14px;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:
      radial-gradient(ellipse 900px 500px at 50% -10%, rgba(216,169,60,0.14), transparent 60%),
      radial-gradient(ellipse 700px 500px at 90% 10%, rgba(51,201,176,0.08), transparent 60%),
      var(--bg);
    color:var(--text);
    font-family:'Inter',sans-serif;
    min-height:100vh;
    -webkit-font-smoothing:antialiased;
  }
  @media (prefers-reduced-motion: reduce){
    *{animation-duration:0.001s !important; transition-duration:0.001s !important;}
  }
  h1,h2,h3,.display{
    font-family:'Big Shoulders Display',sans-serif;
    text-transform:uppercase;
    letter-spacing:0.01em;
    font-weight:800;
    margin:0;
  }
  .mono{font-family:'JetBrains Mono',monospace;}
  a{color:inherit;}
  button{font-family:inherit;}

  /* ---------- topbar ---------- */
  .topbar{
    display:flex;align-items:center;justify-content:space-between;
    padding:18px 28px;
    border-bottom:1px solid var(--panel-border);
    position:sticky; top:0; z-index:40;
    background:rgba(10,12,16,0.85);
    backdrop-filter:blur(10px);
  }
  .brand{display:flex;align-items:center;gap:10px;}
  .brand .fin{font-size:26px;}
  .brand h1{font-size:22px; letter-spacing:0.04em;}
  .brand small{display:block; color:var(--muted); font-family:'Inter'; font-size:10px; text-transform:uppercase; letter-spacing:0.14em; font-weight:600;}
  .topbar nav{display:flex; gap:10px;}
  .btn{
    border:1px solid var(--panel-border);
    background:var(--panel);
    color:var(--text);
    padding:10px 16px;
    border-radius:999px;
    cursor:pointer;
    font-weight:600;
    font-size:13px;
    transition:transform .15s ease, border-color .15s ease, background .15s ease;
  }
  .btn:hover{border-color:var(--gold-dim); transform:translateY(-1px);}
  .btn:focus-visible, button:focus-visible, input:focus-visible, textarea:focus-visible{
    outline:2px solid var(--gold); outline-offset:2px;
  }
  .btn-gold{
    background:linear-gradient(135deg,var(--gold),#b98a2c);
    border:none; color:#171008; font-weight:800;
  }
  .btn-gold:hover{filter:brightness(1.08);}
  .btn-ghost{background:transparent;}

  main{max-width:1080px; margin:0 auto; padding:36px 20px 90px;}
  .screen{display:none; animation:riseIn .5s ease both;}
  .screen.active{display:block;}
  @keyframes riseIn{from{opacity:0; transform:translateY(14px);} to{opacity:1; transform:translateY(0);}}

  /* ---------- hero / intro ---------- */
  .hero{
    text-align:center; padding:34px 10px 10px;
  }
  .hero .eyebrow{
    color:var(--gold); font-family:'JetBrains Mono'; font-size:12px; letter-spacing:0.2em; text-transform:uppercase; font-weight:600;
  }
  .hero h2{font-size:clamp(38px,7vw,66px); line-height:0.98; margin-top:8px;}
  .hero h2 span{color:var(--gold);}
  .hero p{color:var(--muted); max-width:560px; margin:14px auto 0; font-size:15px; line-height:1.6;}

  /* ---------- form ---------- */
  .form-card{
    background:linear-gradient(180deg,var(--panel),var(--bg-2));
    border:1px solid var(--panel-border);
    border-radius:var(--radius);
    padding:30px;
    margin-top:32px;
    box-shadow:0 30px 60px -30px rgba(0,0,0,0.6);
  }
  .field{margin-bottom:20px;}
  .field label{
    display:flex; justify-content:space-between; align-items:baseline;
    font-size:12px; text-transform:uppercase; letter-spacing:0.1em; font-weight:700; color:var(--muted);
    margin-bottom:8px;
  }
  .field label .hint{text-transform:none; letter-spacing:0; color:var(--muted-2); font-weight:400; font-size:11px;}
  .field input, .field textarea{
    width:100%;
    background:var(--bg-2);
    border:1px solid var(--panel-border);
    border-radius:10px;
    padding:13px 14px;
    color:var(--text);
    font-size:14.5px;
    font-family:inherit;
    resize:vertical;
    transition:border-color .15s ease;
  }
  .field input:focus, .field textarea:focus{border-color:var(--gold-dim);}
  .field textarea{min-height:78px; line-height:1.5;}
  .form-grid{display:grid; grid-template-columns:1fr 1fr; gap:0 20px;}
  .form-grid .full{grid-column:1/-1;}
  .form-actions{display:flex; justify-content:flex-end; gap:12px; margin-top:6px; align-items:center;}
  .err-msg{color:var(--red); font-size:12.5px; margin-right:auto; display:none;}

  /* ---------- judges strip ---------- */
  .judges-strip{display:grid; grid-template-columns:repeat(4,1fr); gap:14px; margin-top:34px;}
  .judge-mini{
    background:var(--panel); border:1px solid var(--panel-border); border-radius:12px;
    padding:16px; text-align:center;
  }
  .judge-mini .fin{font-size:26px;}
  .judge-mini h3{font-size:16px; margin-top:6px; letter-spacing:0.02em;}
  .judge-mini p{color:var(--muted); font-size:11.5px; margin:4px 0 0; line-height:1.4;}

  /* ---------- pitch stage ---------- */
  .stage-header{text-align:center; margin-bottom:26px;}
  .stage-header .eyebrow{color:var(--gold); font-family:'JetBrains Mono'; font-size:11px; letter-spacing:0.2em; text-transform:uppercase; font-weight:700;}
  .stage-header h2{font-size:clamp(30px,5vw,46px); margin-top:6px;}
  .pitch-recap{
    background:var(--panel); border:1px solid var(--panel-border); border-radius:var(--radius);
    padding:22px 26px; margin-bottom:30px;
  }
  .pitch-recap h3{font-size:20px; color:var(--gold); margin-bottom:10px;}
  .recap-grid{display:grid; grid-template-columns:1fr 1fr; gap:10px 26px; font-size:13.5px;}
  .recap-grid div b{display:block; color:var(--muted); font-size:10.5px; text-transform:uppercase; letter-spacing:0.08em; margin-bottom:3px; font-weight:700;}
  .recap-grid div span{color:var(--text); line-height:1.5;}

  .qa-track{display:flex; flex-direction:column; gap:16px;}
  .judge-card{
    border:1px solid var(--panel-border);
    background:var(--panel);
    border-radius:var(--radius);
    padding:20px 22px;
    opacity:0.45;
    filter:grayscale(0.4);
    transition:opacity .4s ease, filter .4s ease, border-color .4s ease, transform .4s ease;
  }
  .judge-card.active{
    opacity:1; filter:none; transform:translateY(0);
    box-shadow:0 18px 40px -22px rgba(0,0,0,0.7);
  }
  .judge-card.done{opacity:0.85; filter:none;}
  .judge-head{display:flex; align-items:center; gap:12px; margin-bottom:4px;}
  .judge-head .fin{font-size:28px;}
  .judge-head .jname{font-size:19px; letter-spacing:0.02em;}
  .judge-head .jfocus{font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:0.08em; font-weight:700; margin-top:1px;}
  .qline{margin-top:14px; padding-top:14px; border-top:1px dashed var(--panel-border);}
  .qline:first-of-type{margin-top:16px;}
  .q-label{font-size:10.5px; text-transform:uppercase; letter-spacing:0.1em; color:var(--jc,var(--gold)); font-weight:800; margin-bottom:6px;}
  .q-text{font-size:15px; line-height:1.55; font-weight:500;}
  .a-box{margin-top:10px;}
  .a-box textarea{width:100%; min-height:64px; background:var(--bg-2); border:1px solid var(--panel-border); border-radius:10px; padding:11px 13px; color:var(--text); font-family:inherit; font-size:13.5px; line-height:1.5;}
  .a-box .row{display:flex; justify-content:flex-end; margin-top:8px; gap:8px;}
  .a-box .row .skip{background:transparent; border:1px solid var(--panel-border); color:var(--muted); padding:8px 14px; border-radius:999px; font-size:12.5px; cursor:pointer;}
  .a-box .row .send{border:none; padding:8px 16px; border-radius:999px; font-size:12.5px; font-weight:700; cursor:pointer; color:#101216;}
  .answer-echo{
    margin-top:10px; background:var(--bg-2); border-left:3px solid var(--jc,var(--gold));
    border-radius:0 8px 8px 0; padding:10px 13px; font-size:13.5px; color:var(--muted); line-height:1.5;
  }
  .answer-echo b{color:var(--text); display:block; font-size:10.5px; text-transform:uppercase; letter-spacing:0.08em; margin-bottom:4px;}
  .reaction{
    margin-top:8px; font-size:13.5px; font-style:italic; color:var(--jc,var(--gold));
    display:flex; gap:8px; align-items:flex-start; opacity:0; animation:fadeUp .5s ease forwards;
  }
  @keyframes fadeUp{from{opacity:0; transform:translateY(6px);} to{opacity:1; transform:translateY(0);}}

  .stage-actions{display:flex; justify-content:center; margin-top:30px;}
  .stage-actions .btn-gold{padding:14px 34px; font-size:15px; border-radius:999px; opacity:0.4; pointer-events:none;}
  .stage-actions .btn-gold.ready{opacity:1; pointer-events:all;}

  /* ---------- scoring ---------- */
  .score-wrap{max-width:640px; margin:0 auto;}
  .score-row{margin-bottom:22px;}
  .score-row .srow-top{display:flex; justify-content:space-between; align-items:baseline; margin-bottom:7px;}
  .score-row .slabel{font-size:14px; font-weight:700;}
  .score-row .sval{font-family:'JetBrains Mono'; font-size:15px; color:var(--gold); font-weight:700;}
  .sbar-track{height:10px; border-radius:999px; background:var(--panel-2); overflow:hidden; border:1px solid var(--panel-border);}
  .sbar-fill{height:100%; width:0%; border-radius:999px; background:linear-gradient(90deg,var(--gold-dim),var(--gold)); transition:width 1.1s cubic-bezier(.2,.8,.2,1);}
  .score-overall{
    text-align:center; margin-top:34px; padding-top:28px; border-top:1px solid var(--panel-border);
  }
  .score-overall .big{font-family:'Big Shoulders Display'; font-weight:900; font-size:76px; color:var(--gold); line-height:1;}
  .score-overall p{color:var(--muted); margin-top:6px; font-size:13px; text-transform:uppercase; letter-spacing:0.12em; font-weight:700;}
  .scoring-actions{display:flex; justify-content:center; margin-top:30px;}

  /* ---------- verdict ---------- */
  .verdict-wrap{text-align:center;}
  .chip{
    width:150px; height:150px; margin:10px auto 6px; border-radius:50%;
    display:flex; align-items:center; justify-content:center; flex-direction:column;
    border:6px solid var(--gold);
    background:radial-gradient(circle at 35% 30%, #1e2436, #0b0e14);
    animation:chipSpin 0.9s cubic-bezier(.2,.9,.25,1) both;
    box-shadow:0 0 60px -10px rgba(216,169,60,0.5);
  }
  @keyframes chipSpin{
    0%{transform:rotateY(0deg) scale(0.3); opacity:0;}
    60%{transform:rotateY(720deg) scale(1.05); opacity:1;}
    100%{transform:rotateY(1080deg) scale(1);}
  }
  .chip .fin{font-size:44px;}
  .verdict-title{font-size:clamp(34px,6vw,54px); margin-top:18px;}
  .verdict-sub{color:var(--muted); font-size:14.5px; max-width:560px; margin:10px auto 0; line-height:1.6;}
  .deal-grid{
    display:grid; grid-template-columns:1fr 1fr; gap:14px; max-width:560px; margin:28px auto 0;
  }
  .deal-card{background:var(--panel); border:1px solid var(--panel-border); border-radius:12px; padding:18px;}
  .deal-card b{display:block; font-size:10.5px; text-transform:uppercase; letter-spacing:0.1em; color:var(--muted); margin-bottom:6px;}
  .deal-card span{font-family:'JetBrains Mono'; font-size:22px; color:var(--gold); font-weight:700;}
  .reasoning{
    text-align:left; max-width:640px; margin:26px auto 0; background:var(--panel); border:1px solid var(--panel-border);
    border-radius:var(--radius); padding:22px 24px; font-size:14px; line-height:1.7; color:var(--muted);
  }
  .reasoning b{color:var(--text);}
  .verdict-actions{display:flex; flex-wrap:wrap; justify-content:center; gap:12px; margin-top:30px;}

  /* ---------- leaderboard modal ---------- */
  .modal-overlay{
    position:fixed; inset:0; background:rgba(6,7,10,0.75); backdrop-filter:blur(4px);
    display:none; align-items:center; justify-content:center; z-index:100; padding:20px;
  }
  .modal-overlay.open{display:flex;}
  .modal{
    background:var(--panel); border:1px solid var(--panel-border); border-radius:var(--radius);
    max-width:640px; width:100%; max-height:80vh; overflow-y:auto; padding:26px 26px 30px;
    animation:riseIn .3s ease both;
  }
  .modal-top{display:flex; justify-content:space-between; align-items:center; margin-bottom:16px;}
  .modal-top h2{font-size:26px;}
  .modal-close{background:transparent; border:none; color:var(--muted); font-size:22px; cursor:pointer; line-height:1;}
  .lb-row{
    display:grid; grid-template-columns:34px 1fr auto auto; gap:12px; align-items:center;
    padding:12px 10px; border-bottom:1px solid var(--panel-border); font-size:13.5px;
  }
  .lb-row:last-child{border-bottom:none;}
  .lb-rank{font-family:'JetBrains Mono'; color:var(--gold); font-weight:700;}
  .lb-name{font-weight:700;}
  .lb-name small{display:block; color:var(--muted); font-weight:400; font-size:11px;}
  .lb-score{font-family:'JetBrains Mono'; color:var(--gold);}
  .lb-decision{font-size:10.5px; padding:4px 9px; border-radius:999px; text-transform:uppercase; letter-spacing:0.06em; font-weight:700; white-space:nowrap;}
  .dec-invest{background:rgba(75,201,122,0.15); color:var(--green);}
  .dec-acquire{background:rgba(216,169,60,0.15); color:var(--gold);}
  .dec-later{background:rgba(79,163,227,0.15); color:var(--blue);}
  .dec-reject{background:rgba(225,75,75,0.15); color:var(--red);}
  .lb-empty{color:var(--muted); text-align:center; padding:30px 10px; font-size:13.5px;}

  .toast{
    position:fixed; bottom:26px; left:50%; transform:translateX(-50%) translateY(20px);
    background:var(--panel-2); border:1px solid var(--gold-dim); color:var(--text);
    padding:12px 20px; border-radius:999px; font-size:13px; opacity:0; pointer-events:none;
    transition:opacity .3s ease, transform .3s ease; z-index:200;
  }
  .toast.show{opacity:1; transform:translateX(-50%) translateY(0);}

  footer{text-align:center; color:var(--muted-2); font-size:11.5px; padding:20px; letter-spacing:0.02em;}

  @media (max-width:720px){
    .form-grid, .recap-grid, .deal-grid{grid-template-columns:1fr;}
    .judges-strip{grid-template-columns:1fr 1fr;}
    .topbar{padding:14px 16px;}
  }
</style>
</head>
<body>

<div class="topbar">
  <div class="brand">
    <span class="fin">🦈</span>
    <div>
      <h1>THE TANK</h1>
      <small>AI Pitch Simulator</small>
    </div>
  </div>
  <nav>
    <button class="btn" id="btn-leaderboard-nav">Leaderboard</button>
    <button class="btn btn-ghost" id="btn-restart-nav" style="display:none;">Pitch Again</button>
  </nav>
</div>

<main>

  <!-- SCREEN 1: INPUT -->
  <section class="screen active" id="screen-input">
    <div class="hero">
      <div class="eyebrow">Episode 01 · Cold Open</div>
      <h2>Pitch your idea<br>to the <span>sharks.</span></h2>
      <p>Four AI judges. Eight tough questions. One decision that could make — or sink — your startup. Fill in your pitch below and step onto the stage.</p>
    </div>

    <form class="form-card" id="pitch-form" novalidate>
      <div class="form-grid">
        <div class="field full">
          <label for="f-name">Startup Name <span class="hint">what's on the sign?</span></label>
          <input id="f-name" type="text" maxlength="60" placeholder="e.g. Loopline" required>
        </div>
        <div class="field full">
          <label for="f-problem">Problem Statement <span class="hint">what's broken?</span></label>
          <textarea id="f-problem" maxlength="500" placeholder="Describe the real pain point you're solving..." required></textarea>
        </div>
        <div class="field full">
          <label for="f-solution">Solution <span class="hint">how do you fix it?</span></label>
          <textarea id="f-solution" maxlength="500" placeholder="Describe your product and why it works..." required></textarea>
        </div>
        <div class="field">
          <label for="f-revenue">Revenue Model <span class="hint">how you make money</span></label>
          <textarea id="f-revenue" maxlength="300" placeholder="e.g. $29/mo subscription, 15% take rate..." required></textarea>
        </div>
        <div class="field">
          <label for="f-audience">Target Audience <span class="hint">who buys this</span></label>
          <textarea id="f-audience" maxlength="300" placeholder="e.g. solo dentists running independent practices..." required></textarea>
        </div>
        <div class="field full">
          <label for="f-ask">Funding Ask <span class="hint">amount &amp; equity, e.g. $250,000 for 8%</span></label>
          <input id="f-ask" type="text" maxlength="80" placeholder="$250,000 for 8% equity" required>
        </div>
      </div>
      <div class="form-actions">
        <span class="err-msg" id="form-err">Fill in every field before you step into the tank.</span>
        <button type="submit" class="btn btn-gold">Enter The Tank →</button>
      </div>
    </form>

    <div class="judges-strip">
      <div class="judge-mini"><div class="fin">🦈</div><h3>Venture Capitalist</h3><p>Market size &amp; scalability</p></div>
      <div class="judge-mini"><div class="fin">🦈</div><h3>Founder</h3><p>Execution &amp; grit</p></div>
      <div class="judge-mini"><div class="fin">🦈</div><h3>Customer</h3><p>Real-world usefulness</p></div>
      <div class="judge-mini"><div class="fin">🦈</div><h3>Angel Investor</h3><p>Profitability &amp; unit economics</p></div>
    </div>
  </section>

  <!-- SCREEN 2: PITCH / Q&A -->
  <section class="screen" id="screen-pitch">
    <div class="stage-header">
      <div class="eyebrow">Episode 01 · The Pitch</div>
      <h2 id="stage-title">Taking the Stage</h2>
    </div>

    <div class="pitch-recap" id="pitch-recap"></div>

    <div class="qa-track" id="qa-track"></div>

    <div class="stage-actions">
      <button class="btn btn-gold" id="btn-to-scoring">See My Scores →</button>
    </div>
  </section>

  <!-- SCREEN 3: SCORING -->
  <section class="screen" id="screen-scoring">
    <div class="stage-header">
      <div class="eyebrow">Episode 01 · Deliberation</div>
      <h2>The Sharks Confer</h2>
    </div>
    <div class="score-wrap" id="score-wrap"></div>
    <div class="scoring-actions">
      <button class="btn btn-gold" id="btn-to-verdict">Reveal The Verdict →</button>
    </div>
  </section>

  <!-- SCREEN 4: VERDICT -->
  <section class="screen" id="screen-verdict">
    <div class="verdict-wrap">
      <div class="chip" id="verdict-chip"><span class="fin" id="verdict-emoji">🦈</span></div>
      <h2 class="verdict-title" id="verdict-title">—</h2>
      <p class="verdict-sub" id="verdict-sub"></p>

      <div class="deal-grid" id="deal-grid"></div>
      <div class="reasoning" id="reasoning"></div>

      <div class="verdict-actions">
        <button class="btn btn-gold" id="btn-download-pdf">⬇ Download Pitch Report</button>
        <button class="btn" id="btn-share">↗ Share Result</button>
        <button class="btn" id="btn-view-leaderboard">🏆 Leaderboard</button>
        <button class="btn btn-ghost" id="btn-restart">↻ Pitch Again</button>
      </div>
    </div>
  </section>

</main>

<footer>THE TANK is a fictional AI simulation for entertainment &amp; practice purposes. No real investment offers are made.</footer>

<!-- LEADERBOARD MODAL -->
<div class="modal-overlay" id="lb-overlay">
  <div class="modal">
    <div class="modal-top">
      <h2>🏆 Leaderboard</h2>
      <button class="modal-close" id="lb-close">×</button>
    </div>
    <div id="lb-list"></div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
(function(){
  "use strict";

  /* ============================================================
     STATE
  ============================================================ */
  const state = {
    pitch: {name:"", problem:"", solution:"", revenue:"", audience:"", ask:""},
    answers: [],        // {judgeId, qIndex, text, quality}
    scores: {},         // market, innovation, business, execution, worthiness
    decision: null
  };

  const LB_KEY = "shark_tank_leaderboard_v1";

  /* ============================================================
     JUDGES CONFIG
  ============================================================ */
  const JUDGES = [
    {
      id:"vc", name:"Marcus Vale", role:"Venture Capitalist", emoji:"🦈", color:"#d8a93c",
      focus:"market size & scalability",
      questions:[
        p => `If ${p.name} takes off, are we talking a niche tool or something that owns the ${p.audience || "whole"} market? Paint me the big picture.`,
        p => `Say you land your first 1,000 customers — what breaks first when you try to 10x that, and how do you scale past it?`
      ],
      reactions:{
        low:["Vague. I need numbers, not vibes.", "That's a hand-wave, not a market. I'm not seeing the size here.", "You're describing a hobby, not a category."],
        mid:["Okay, there's something here, but I need more conviction on scale.", "Not bad — but you're going to get out-funded by someone with a bigger vision.", "I see a market. I don't yet see a dominant position in it."],
        high:["Now that's a market worth chasing.", "That's the kind of scale story that gets a term sheet.", "You clearly understand how big this could get. I like that."]
      }
    },
    {
      id:"founder", name:"Priya Chen", role:"Founder", emoji:"🦈", color:"#33c9b0",
      focus:"execution",
      questions:[
        p => `Walk me through what actually exists today for ${p.name} — is this a live product, a prototype, or a slide deck?`,
        p => `What's the single biggest thing that could kill ${p.name} in the next twelve months, and what are you doing about it right now?`
      ],
      reactions:{
        low:["I've built companies. This sounds like it's still just an idea.", "You don't have a plan, you have a hope. That worries me.", "I need to see you've actually shipped something, not just imagined it."],
        mid:["There's real work here, but execution risk is still high.", "You're further along than most, but the hard part is still ahead of you.", "I respect the hustle. I need to see it's repeatable, not lucky."],
        high:["That's how a founder talks. You've clearly done the work.", "You know exactly where the bodies are buried. That's rare.", "This is execution I can bet on."]
      }
    },
    {
      id:"customer", name:"Jordan Reyes", role:"Customer", emoji:"🦈", color:"#4fa3e3",
      focus:"usefulness",
      questions:[
        p => `Honestly — why would a ${p.audience || "customer like me"} choose ${p.name} over just living with the problem, or duct-taping it with a spreadsheet?`,
        p => `The day after I stop using ${p.name}, do I actually notice it's gone, or does life just carry on?`
      ],
      reactions:{
        low:["I wouldn't pay for that. It doesn't solve a real pain for me.", "That's a nice-to-have, not a need-to-have.", "I could live without this. That's a problem for you."],
        mid:["I might try it once, but you haven't convinced me I'd stay.", "It's useful-ish. I need a stronger reason to switch my habits.", "You're solving *a* problem, just not sure it's *my* problem yet."],
        high:["Okay, I'd actually use that every week.", "That's the kind of thing I'd tell my friends about.", "You clearly understand what your customer actually feels."]
      }
    },
    {
      id:"angel", name:"Elena Osei", role:"Angel Investor", emoji:"🦈", color:"#e1735c",
      focus:"profitability",
      questions:[
        p => `Walk me through the unit economics — what does it cost you to win and serve one customer, versus what they pay you?`,
        p => `You're asking for ${p.ask || "funding"}. What does that number actually buy you, month by month?`
      ],
      reactions:{
        low:["I didn't hear a single number I could underwrite.", "That's not a business model, that's a wish list.", "You need to know your margins cold before you ask me for a check."],
        mid:["The math is directionally okay, but thin on detail.", "I can see a path to profit, it's just foggy right now.", "You're close. Tighten the numbers and come back."],
        high:["Now those are numbers I can underwrite.", "You know your margins better than most founders twice your age.", "That's exactly the kind of discipline that gets my money."]
      }
    }
  ];

  /* ============================================================
     HELPERS
  ============================================================ */
  function $(sel, ctx){ return (ctx||document).querySelector(sel); }
  function $all(sel, ctx){ return Array.from((ctx||document).querySelectorAll(sel)); }
  function esc(str){
    const d = document.createElement('div');
    d.textContent = str == null ? "" : str;
    return d.innerHTML;
  }
  function clamp(n,min,max){ return Math.max(min, Math.min(max,n)); }
  function showToast(msg){
    const t = $("#toast");
    t.textContent = msg;
    t.classList.add("show");
    clearTimeout(t._timer);
    t._timer = setTimeout(()=>t.classList.remove("show"), 2600);
  }
  function switchScreen(id){
    $all(".screen").forEach(s=>s.classList.remove("active"));
    $("#"+id).classList.add("active");
    window.scrollTo({top:0, behavior:"smooth"});
  }

  /* ---- lightweight text-quality heuristic (no backend / no real AI) ---- */
  const STRONG_WORDS = ["data","revenue","margin","%","percent","customer","users","pilot","tested","validated",
    "retention","recurring","proprietary","patent","partnership","contract","waitlist","growth","month","cac","ltv",
    "subscription","license","market","scalable","automation","proven","launched","live","shipped","signed","paying"];
  const WEAK_WORDS = ["maybe","probably","not sure","i think","hopefully","idk","dunno","someday","eventually"];

  function scoreAnswerQuality(text){
    if(!text || !text.trim()) return 22; // skipped / empty answer
    const t = text.toLowerCase();
    let score = 40;
    score += clamp(text.trim().split(/\s+/).length, 0, 60) * 0.6; // length rewards specificity, capped
    STRONG_WORDS.forEach(w=>{ if(t.includes(w)) score += 4; });
    WEAK_WORDS.forEach(w=>{ if(t.includes(w)) score -= 6; });
    if(/\$|\d/.test(text)) score += 6; // contains numbers/figures
    return clamp(Math.round(score), 5, 100);
  }

  function scoreFieldQuality(text, keywords, base){
    if(!text || !text.trim()) return base - 20;
    const t = text.toLowerCase();
    let score = base;
    score += clamp(text.trim().split(/\s+/).length, 0, 50) * 0.7;
    keywords.forEach(w=>{ if(t.includes(w)) score += 5; });
    if(/\$|\d/.test(text)) score += 5;
    return clamp(Math.round(score), 5, 100);
  }

  function reactionTier(score){
    if(score >= 68) return "high";
    if(score >= 42) return "mid";
    return "low";
  }
  function pick(arr){ return arr[Math.floor(Math.random()*arr.length)]; }

  /* ============================================================
     SCREEN 1 → 2 : FORM SUBMIT
  ============================================================ */
  const form = $("#pitch-form");
  form.addEventListener("submit", function(e){
    e.preventDefault();
    const p = {
      name: $("#f-name").value.trim(),
      problem: $("#f-problem").value.trim(),
      solution: $("#f-solution").value.trim(),
      revenue: $("#f-revenue").value.trim(),
      audience: $("#f-audience").value.trim(),
      ask: $("#f-ask").value.trim()
    };
    if(Object.values(p).some(v=>!v)){
      $("#form-err").style.display = "inline";
      return;
    }
    $("#form-err").style.display = "none";
    state.pitch = p;
    state.answers = [];
    buildPitchStage();
    switchScreen("screen-pitch");
  });

  /* ============================================================
     SCREEN 2 : PITCH STAGE + Q&A
  ============================================================ */
  function buildPitchStage(){
    const p = state.pitch;
    $("#stage-title").textContent = `${p.name} Takes the Stage`;

    $("#pitch-recap").innerHTML = `
      <h3>🎤 ${esc(p.name)}</h3>
      <div class="recap-grid">
        <div><b>Problem</b><span>${esc(p.problem)}</span></div>
        <div><b>Solution</b><span>${esc(p.solution)}</span></div>
        <div><b>Revenue Model</b><span>${esc(p.revenue)}</span></div>
        <div><b>Target Audience</b><span>${esc(p.audience)}</span></div>
        <div class="full" style="grid-column:1/-1;"><b>Funding Ask</b><span>${esc(p.ask)}</span></div>
      </div>`;

    const track = $("#qa-track");
    track.innerHTML = "";
    JUDGES.forEach((j, jIdx)=>{
      const card = document.createElement("div");
      card.className = "judge-card";
      card.id = `judge-${j.id}`;
      card.style.setProperty("--jc", j.color);

      const qs = j.questions.map(fn=>fn(p));
      card.innerHTML = `
        <div class="judge-head">
          <span class="fin">${j.emoji}</span>
          <div>
            <div class="jname">${esc(j.name)} <span style="color:var(--muted); font-weight:400; font-size:13px;">— ${esc(j.role)}</span></div>
            <div class="jfocus">Focus: ${esc(j.focus)}</div>
          </div>
        </div>
        ${qs.map((q,i)=>`
          <div class="qline" data-qidx="${i}">
            <div class="q-label">Question ${i+1}</div>
            <div class="q-text">${esc(q)}</div>
            <div class="a-box" data-role="abox">
              <textarea placeholder="Your answer to ${esc(j.name)}..." data-jid="${j.id}" data-qidx="${i}"></textarea>
              <div class="row">
                <button type="button" class="skip" data-action="skip">Skip</button>
                <button type="button" class="send" data-action="answer" style="background:${j.color}">Answer</button>
              </div>
            </div>
          </div>
        `).join("")}
      `;
      track.appendChild(card);
    });

    activateJudge(0);
    updateStageCTA();
  }

  function activateJudge(idx){
    JUDGES.forEach((j,i)=>{
      const card = $("#judge-"+j.id);
      card.classList.toggle("active", i===idx);
    });
  }

  document.addEventListener("click", function(e){
    const btn = e.target.closest("[data-action]");
    if(!btn) return;
    const action = btn.dataset.action;
    const qline = btn.closest(".qline");
    const card = btn.closest(".judge-card");
    const jid = card.id.replace("judge-","");
    const judge = JUDGES.find(j=>j.id===jid);
    const qIdx = parseInt(qline.dataset.qidx,10);
    const textarea = qline.querySelector("textarea");
    const text = action==="skip" ? "" : textarea.value.trim();

    handleAnswer(judge, qIdx, text, qline, card);
  });

  function handleAnswer(judge, qIdx, text, qline, card){
    const quality = scoreAnswerQuality(text);
    state.answers.push({judgeId:judge.id, qIndex:qIdx, text, quality});

    const abox = qline.querySelector('[data-role="abox"]');
    const tier = reactionTier(quality);
    const line = pick(judge.reactions[tier]);

    abox.outerHTML = `
      <div class="answer-echo" style="--jc:${judge.color}">
        <b>Your answer</b>${text ? esc(text) : "<em style='color:var(--muted-2)'>(skipped this one)</em>"}
      </div>
      <div class="reaction" style="--jc:${judge.color}"><span>${judge.emoji}</span><span>"${esc(line)}"</span></div>
    `;

    // move to next judge if this judge's questions are all done
    const judgeIdx = JUDGES.findIndex(j=>j.id===judge.id);
    const answeredForJudge = state.answers.filter(a=>a.judgeId===judge.id).length;
    if(answeredForJudge >= judge.questions.length){
      card.classList.remove("active");
      card.classList.add("done");
      if(judgeIdx < JUDGES.length - 1){
        activateJudge(judgeIdx+1);
      }
    }
    updateStageCTA();
  }

  function updateStageCTA(){
    const totalQs = JUDGES.reduce((n,j)=>n+j.questions.length,0);
    const done = state.answers.length >= totalQs;
    $("#btn-to-scoring").classList.toggle("ready", done);
  }

  $("#btn-to-scoring").addEventListener("click", function(){
    if(!this.classList.contains("ready")){
      showToast("Answer or skip every question first — the sharks are waiting.");
      return;
    }
    computeScores();
    renderScoring();
    switchScreen("screen-scoring");
  });

  /* ============================================================
     SCORING ENGINE
  ============================================================ */
  function avgQualityFor(judgeId){
    const arr = state.answers.filter(a=>a.judgeId===judgeId);
    if(!arr.length) return 30;
    return arr.reduce((s,a)=>s+a.quality,0)/arr.length;
  }

  function computeScores(){
    const p = state.pitch;
    const vcQ = avgQualityFor("vc");
    const founderQ = avgQualityFor("founder");
    const customerQ = avgQualityFor("customer");
    const angelQ = avgQualityFor("angel");

    const marketPotential = clamp(Math.round(
      0.55*scoreFieldQuality(p.audience, ["million","billion","global","everyone","industry","market"], 50) +
      0.45*vcQ
    ),1,100);

    const innovation = clamp(Math.round(
      0.6*scoreFieldQuality(p.solution, ["ai","platform","first","unique","proprietary","automat","algorithm","patent"], 52) +
      0.4*customerQ
    ),1,100);

    const businessModel = clamp(Math.round(
      0.55*scoreFieldQuality(p.revenue, ["subscription","saas","commission","margin","recurring","license","fee","%"], 50) +
      0.45*angelQ
    ),1,100);

    const execution = clamp(Math.round(
      0.7*founderQ + 0.3*scoreFieldQuality(p.problem, ["validated","tested","pilot","launched","customers"], 48)
    ),1,100);

    const investmentWorthiness = clamp(Math.round(
      0.28*marketPotential + 0.2*innovation + 0.22*businessModel + 0.18*execution + 0.12*((vcQ+founderQ+customerQ+angelQ)/4)
    ),1,100);

    state.scores = {marketPotential, innovation, businessModel, execution, investmentWorthiness};
  }

  function renderScoring(){
    const s = state.scores;
    const rows = [
      ["Market Potential", s.marketPotential],
      ["Innovation", s.innovation],
      ["Business Model", s.businessModel],
      ["Execution", s.execution],
      ["Investment Worthiness", s.investmentWorthiness]
    ];
    const wrap = $("#score-wrap");
    wrap.innerHTML = rows.map(([label,val])=>`
      <div class="score-row">
        <div class="srow-top"><span class="slabel">${label}</span><span class="sval mono">${val}/100</span></div>
        <div class="sbar-track"><div class="sbar-fill" data-val="${val}"></div></div>
      </div>
    `).join("") + `
      <div class="score-overall">
        <div class="big mono">${s.investmentWorthiness}</div>
        <p>Overall Tank Score</p>
      </div>
    `;
    requestAnimationFrame(()=>{
      requestAnimationFrame(()=>{
        $all(".sbar-fill", wrap).forEach(el=>{ el.style.width = el.dataset.val + "%"; });
      });
    });
  }

  /* ============================================================
     DECISION ENGINE
  ============================================================ */
  function decide(){
    const s = state.scores;
    const w = s.investmentWorthiness;
    let type;
    if(w >= 78) type = "invest";
    else if(w >= 62) type = "acquire";
    else if(w >= 45) type = "later";
    else type = "reject";

    // funding + valuation heuristics (fictional, for simulation flavor)
    let fundingAmount = 0, equity = 0, valuation = 0;
    const tierMult = w/100;
    if(type === "invest"){
      fundingAmount = Math.round((150000 + tierMult*1350000)/25000)*25000;
      equity = clamp(Math.round(28 - tierMult*15), 6, 25);
      valuation = Math.round(fundingAmount / (equity/100));
    } else if(type === "acquire"){
      valuation = Math.round((400000 + tierMult*2600000)/50000)*50000;
      fundingAmount = valuation; // outright offer
      equity = 100;
    } else if(type === "later"){
      fundingAmount = 0; equity = 0; valuation = 0;
    } else {
      fundingAmount = 0; equity = 0; valuation = 0;
    }

    const reasoningParts = [];
    const p = state.pitch;
    reasoningParts.push(`<b>${esc(p.name)}</b> walked away with an overall Tank Score of <b>${w}/100</b>.`);
    if(type==="invest"){
      reasoningParts.push(`The sharks were convinced by the combination of market potential (${s.marketPotential}) and a business model (${s.businessModel}) that pencils out. This is a deal worth doing.`);
    } else if(type==="acquire"){
      reasoningParts.push(`The sharks don't want to fund the journey — they'd rather buy the destination outright. Strong enough fundamentals (execution ${s.execution}, innovation ${s.innovation}) to take the whole thing off your hands.`);
    } else if(type==="later"){
      reasoningParts.push(`There's a real business in here, but too many open questions on ${s.businessModel < s.marketPotential ? "the business model" : "execution"} for a check today. Tighten the numbers, get some traction, and come back.`);
    } else {
      reasoningParts.push(`The math didn't work for the sharks today — weak spots in ${[["market potential",s.marketPotential],["innovation",s.innovation],["business model",s.businessModel],["execution",s.execution]].sort((a,b)=>a[1]-b[1])[0][0]} made this too risky.`);
    }
    reasoningParts.push(`Individually: the Venture Capitalist weighed market size, the Founder judged your execution, the Customer judged real-world usefulness, and the Angel Investor stress-tested your profitability.`);

    state.decision = {type, fundingAmount, equity, valuation, reasoning: reasoningParts.join(" ")};
    return state.decision;
  }

  const DECISION_META = {
    invest: {emoji:"🤝", title:"WE HAVE A DEAL!", cls:"dec-invest", tone:"var(--green)"},
    acquire:{emoji:"💰", title:"WE WANT TO ACQUIRE YOU", cls:"dec-acquire", tone:"var(--gold)"},
    later:  {emoji:"⏳", title:"COME BACK LATER", cls:"dec-later", tone:"var(--blue)"},
    reject: {emoji:"❌", title:"I'M OUT.", cls:"dec-reject", tone:"var(--red)"}
  };

  $("#btn-to-verdict").addEventListener("click", function(){
    const d = decide();
    renderVerdict(d);
    saveToLeaderboard(d);
    switchScreen("screen-verdict");
    if(d.type==="invest" || d.type==="acquire"){
      launchConfetti();
    }
  });

  function fmtMoney(n){
    if(!n) return "—";
    return "$" + n.toLocaleString("en-US");
  }

  function renderVerdict(d){
    const meta = DECISION_META[d.type];
    const p = state.pitch;
    $("#verdict-emoji").textContent = meta.emoji;
    $("#verdict-chip").style.borderColor = meta.tone;
    $("#verdict-title").textContent = meta.title;
    $("#verdict-title").style.color = meta.tone;

    const subMap = {
      invest: `The sharks are backing ${p.name}. Time to build.`,
      acquire: `The sharks like it enough to buy it outright.`,
      later: `Not a no — but not a yes today either.`,
      reject: `${p.name} didn't land a deal this time.`
    };
    $("#verdict-sub").textContent = subMap[d.type];

    let dealHtml = "";
    if(d.type==="invest"){
      dealHtml = `
        <div class="deal-card"><b>Funding Amount</b><span>${fmtMoney(d.fundingAmount)}</span></div>
        <div class="deal-card"><b>Equity Requested</b><span>${d.equity}%</span></div>
        <div class="deal-card" style="grid-column:1/-1;"><b>Implied Valuation</b><span>${fmtMoney(d.valuation)}</span></div>
      `;
    } else if(d.type==="acquire"){
      dealHtml = `
        <div class="deal-card" style="grid-column:1/-1;"><b>Acquisition Offer</b><span>${fmtMoney(d.valuation)}</span></div>
      `;
    } else {
      dealHtml = `<div class="deal-card" style="grid-column:1/-1;"><b>Suggested Valuation</b><span>Not offered today</span></div>`;
    }
    $("#deal-grid").innerHTML = dealHtml;

    $("#reasoning").innerHTML = `<b>Why the sharks decided this:</b><br><br>${d.reasoning}`;
  }

  function launchConfetti(){
    if(typeof confetti !== "function") return;
    const duration = 2200;
    const end = Date.now() + duration;
    (function frame(){
      confetti({ particleCount: 4, angle: 60, spread: 60, origin:{x:0}, colors:['#d8a93c','#33c9b0','#edeae2'] });
      confetti({ particleCount: 4, angle: 120, spread: 60, origin:{x:1}, colors:['#d8a93c','#33c9b0','#edeae2'] });
      if(Date.now() < end) requestAnimationFrame(frame);
    })();
    confetti({ particleCount: 140, spread: 100, origin:{y:0.6}, colors:['#d8a93c','#33c9b0','#edeae2'] });
  }

  /* ============================================================
     LEADERBOARD
  ============================================================ */
  function loadLB(){
    try{ return JSON.parse(localStorage.getItem(LB_KEY)) || []; }
    catch(e){ return []; }
  }
  function saveToLeaderboard(d){
    const lb = loadLB();
    lb.push({
      name: state.pitch.name,
      score: state.scores.investmentWorthiness,
      decision: d.type,
      valuation: d.valuation,
      ts: Date.now()
    });
    lb.sort((a,b)=>b.score-a.score);
    localStorage.setItem(LB_KEY, JSON.stringify(lb.slice(0,50)));
  }
  function renderLeaderboard(){
    const lb = loadLB();
    const list = $("#lb-list");
    if(!lb.length){
      list.innerHTML = `<div class="lb-empty">No pitches yet. Step into the tank to claim the top spot.</div>`;
      return;
    }
    list.innerHTML = lb.slice(0,20).map((row,i)=>{
      const meta = DECISION_META[row.decision] || DECISION_META.reject;
      const date = new Date(row.ts);
      return `
        <div class="lb-row">
          <div class="lb-rank">#${i+1}</div>
          <div class="lb-name">${esc(row.name)}<small>${date.toLocaleDateString()}</small></div>
          <div class="lb-score">${row.score}</div>
          <div class="lb-decision ${meta.cls}">${row.decision}</div>
        </div>`;
    }).join("");
  }

  $("#btn-leaderboard-nav").addEventListener("click", ()=>{ renderLeaderboard(); $("#lb-overlay").classList.add("open"); });
  $("#btn-view-leaderboard").addEventListener("click", ()=>{ renderLeaderboard(); $("#lb-overlay").classList.add("open"); });
  $("#lb-close").addEventListener("click", ()=> $("#lb-overlay").classList.remove("open"));
  $("#lb-overlay").addEventListener("click", (e)=>{ if(e.target.id==="lb-overlay") $("#lb-overlay").classList.remove("open"); });

  /* ============================================================
     RESTART
  ============================================================ */
  function restart(){
    form.reset();
    $("#form-err").style.display = "none";
    state.pitch = {name:"", problem:"", solution:"", revenue:"", audience:"", ask:""};
    state.answers = [];
    state.scores = {};
    state.decision = null;
    $("#btn-restart-nav").style.display = "none";
    switchScreen("screen-input");
  }
  $("#btn-restart").addEventListener("click", restart);
  $("#btn-restart-nav").addEventListener("click", restart);

  // reveal "pitch again" nav button once user leaves the input screen
  const observer = new MutationObserver(()=>{
    $("#btn-restart-nav").style.display = $("#screen-input").classList.contains("active") ? "none" : "inline-block";
  });
  observer.observe($("#screen-input"), {attributes:true, attributeFilter:["class"]});

  /* ============================================================
     SHARE
  ============================================================ */
  $("#btn-share").addEventListener("click", async function(){
    const d = state.decision;
    const p = state.pitch;
    const meta = DECISION_META[d.type];
    const text = `🦈 ${p.name} just pitched THE TANK and got: ${meta.title} (Tank Score: ${state.scores.investmentWorthiness}/100)`;
    if(navigator.share){
      try{ await navigator.share({title:"The Tank — My Pitch Result", text}); }
      catch(e){ /* user cancelled */ }
    } else {
      try{
        await navigator.clipboard.writeText(text);
        showToast("Result copied to clipboard!");
      }catch(e){
        showToast(text);
      }
    }
  });

  /* ============================================================
     PDF REPORT
  ============================================================ */
  $("#btn-download-pdf").addEventListener("click", function(){
    if(!window.jspdf){ showToast("PDF library failed to load — check your connection."); return; }
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({unit:"pt", format:"a4"});
    const p = state.pitch, s = state.scores, d = state.decision;
    const meta = DECISION_META[d.type];
    const margin = 48;
    let y = margin;
    const pageW = doc.internal.pageSize.getWidth();
    const maxW = pageW - margin*2;

    function heading(text, size){
      doc.setFont("helvetica","bold"); doc.setFontSize(size||16);
      doc.setTextColor(20,20,20);
      doc.text(text, margin, y); y += size ? size*0.9 : 20;
    }
    function body(text, size){
      doc.setFont("helvetica","normal"); doc.setFontSize(size||10.5);
      doc.setTextColor(60,60,60);
      const lines = doc.splitTextToSize(text || "—", maxW);
      doc.text(lines, margin, y);
      y += lines.length * (size ? size*1.15 : 13) + 6;
    }
    function rule(){ doc.setDrawColor(210,210,210); doc.line(margin, y, pageW-margin, y); y += 14; }
    function checkPage(){ if(y > 760){ doc.addPage(); y = margin; } }

    doc.setFillColor(10,12,16); doc.rect(0,0,pageW,70,"F");
    doc.setTextColor(216,169,60); doc.setFont("helvetica","bold"); doc.setFontSize(20);
    doc.text("🦈 THE TANK — PITCH REPORT", margin, 44);
    y = 96;

    heading(p.name, 22);
    doc.setFontSize(10); doc.setTextColor(120,120,120);
    doc.text(new Date().toLocaleDateString(), margin, y); y += 20;
    rule();

    heading("The Pitch", 13);
    body("Problem: " + p.problem);
    body("Solution: " + p.solution);
    body("Revenue Model: " + p.revenue);
    body("Target Audience: " + p.audience);
    body("Funding Ask: " + p.ask);
    rule(); checkPage();

    heading("Judge Q&A", 13);
    JUDGES.forEach(j=>{
      checkPage();
      doc.setFont("helvetica","bold"); doc.setFontSize(11); doc.setTextColor(30,30,30);
      doc.text(`${j.name} — ${j.role}`, margin, y); y += 14;
      j.questions.forEach((fn,i)=>{
        const q = fn(p);
        const ans = state.answers.find(a=>a.judgeId===j.id && a.qIndex===i);
        checkPage();
        body("Q: " + q, 10);
        body("A: " + (ans && ans.text ? ans.text : "(skipped)"), 10);
      });
      y += 4;
    });
    rule(); checkPage();

    heading("Scores", 13);
    [["Market Potential", s.marketPotential],["Innovation", s.innovation],["Business Model", s.businessModel],
     ["Execution", s.execution],["Investment Worthiness", s.investmentWorthiness]].forEach(([label,val])=>{
      checkPage();
      body(`${label}: ${val}/100`, 11);
    });
    rule(); checkPage();

    heading("Verdict: " + meta.title, 14);
    if(d.type==="invest"){
      body(`Funding Amount: ${fmtMoney(d.fundingAmount)}  |  Equity: ${d.equity}%  |  Implied Valuation: ${fmtMoney(d.valuation)}`);
    } else if(d.type==="acquire"){
      body(`Acquisition Offer: ${fmtMoney(d.valuation)}`);
    } else {
      body("No valuation offered at this time.");
    }
    body(d.reasoning.replace(/<[^>]+>/g,""));

    doc.setFontSize(8); doc.setTextColor(160,160,160);
    doc.text("Generated by The Tank — a fictional AI pitch simulator. No real investment offer.", margin, 810);

    doc.save(`${(p.name||"pitch").replace(/[^a-z0-9]+/gi,"_")}_TankReport.pdf`);
    showToast("Pitch report downloaded!");
  });

})();
</script>
</body>
</html>
