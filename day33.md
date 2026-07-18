<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Media Integrity Analyzer</title>
<style>
  :root{
    --bg: #070b14;
    --bg-2: #0a1120;
    --surface: #101a2c;
    --surface-2: #16213a;
    --surface-3: #1c2941;
    --border: rgba(148,163,184,0.14);
    --border-strong: rgba(148,163,184,0.28);
    --text: #e8edf6;
    --text-dim: #93a1b8;
    --text-faint: #5c6a84;
    --accent: #3b82f6;
    --accent-2: #60a5fa;
    --accent-soft: rgba(59,130,246,0.14);
    --accent-soft-2: rgba(59,130,246,0.28);
    --good: #34d399;
    --good-soft: rgba(52,211,153,0.14);
    --warn: #fbbf24;
    --warn-soft: rgba(251,191,36,0.14);
    --bad: #f87171;
    --bad-soft: rgba(248,113,113,0.14);
    --serif: Georgia, 'Times New Roman', Times, serif;
    --sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Inter, Roboto, Helvetica, Arial, sans-serif;
    --radius: 16px;
    --radius-sm: 10px;
  }

  *{ box-sizing: border-box; }
  html{ scroll-behavior: smooth; }

  body{
    margin:0;
    min-height:100vh;
    background:
      radial-gradient(ellipse 900px 500px at 15% -10%, rgba(59,130,246,0.10), transparent 60%),
      radial-gradient(ellipse 700px 500px at 100% 10%, rgba(96,165,250,0.06), transparent 55%),
      var(--bg);
    color: var(--text);
    font-family: var(--sans);
    line-height:1.55;
    -webkit-font-smoothing: antialiased;
  }

  ::selection{ background: var(--accent-soft-2); color: #fff; }

  a{ color: var(--accent-2); }

  /* ---------- Layout shell ---------- */
  .shell{
    max-width: 980px;
    margin: 0 auto;
    padding: 28px 20px 90px;
  }

  header.masthead{
    text-align:center;
    padding: 18px 0 30px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 30px;
    animation: fadeDown .7s ease both;
  }
  header.masthead .kicker{
    display:inline-flex;
    align-items:center;
    gap:8px;
    font-size:11px;
    letter-spacing:2.5px;
    text-transform:uppercase;
    color: var(--accent-2);
    background: var(--accent-soft);
    border: 1px solid var(--accent-soft-2);
    padding: 6px 14px;
    border-radius: 999px;
    margin-bottom: 18px;
  }
  header.masthead .dot{
    width:6px; height:6px; border-radius:50%;
    background: var(--accent-2);
    box-shadow: 0 0 8px var(--accent-2);
  }
  header.masthead h1{
    font-family: var(--serif);
    font-weight: 700;
    font-size: clamp(30px, 5vw, 46px);
    margin: 0 0 10px;
    letter-spacing: -0.5px;
  }
  header.masthead p{
    color: var(--text-dim);
    max-width: 560px;
    margin: 0 auto;
    font-size: 15px;
  }

  /* ---------- Progress steps ---------- */
  .progress-track{
    display:flex;
    align-items:center;
    justify-content:center;
    gap: 6px;
    margin: 0 0 34px;
    flex-wrap: wrap;
  }
  .progress-step{
    display:flex; align-items:center; gap:10px;
    padding: 8px 14px 8px 8px;
    border-radius: 999px;
    border: 1px solid var(--border);
    background: var(--surface);
    font-size: 13px;
    color: var(--text-faint);
    transition: all .4s ease;
  }
  .progress-step .num{
    width: 22px; height: 22px;
    border-radius:50%;
    display:flex; align-items:center; justify-content:center;
    font-size: 11px;
    font-weight:700;
    background: var(--surface-3);
    color: var(--text-faint);
    border: 1px solid var(--border-strong);
    transition: all .4s ease;
  }
  .progress-step.current{
    border-color: var(--accent-soft-2);
    background: var(--accent-soft);
    color: var(--text);
  }
  .progress-step.current .num{ background: var(--accent); color:#fff; border-color: var(--accent); box-shadow:0 0 12px rgba(59,130,246,.6); }
  .progress-step.done{ color: var(--good); border-color: rgba(52,211,153,0.3); }
  .progress-step.done .num{ background: var(--good); color:#04150e; border-color: var(--good); }
  .progress-line{ width: 22px; height: 1px; background: var(--border-strong); }

  /* ---------- Metrics dashboard bar ---------- */
  .metrics-panel{
    background: linear-gradient(180deg, var(--surface), var(--bg-2));
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px 22px;
    margin-bottom: 30px;
    display:grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 18px;
    animation: fadeUp .7s ease both;
  }
  .metric{ min-width:0; }
  .metric .m-top{
    display:flex; justify-content:space-between; align-items:baseline;
    margin-bottom:8px;
    gap: 6px;
  }
  .metric .m-label{
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1.1px;
    color: var(--text-faint);
    white-space: nowrap;
    overflow:hidden;
    text-overflow: ellipsis;
  }
  .metric .m-value{ font-size: 14px; font-weight:700; color: var(--text-dim); font-variant-numeric: tabular-nums; }
  .metric .m-value.set{ color: var(--text); }
  .m-track{
    height: 7px;
    border-radius: 999px;
    background: var(--surface-3);
    overflow:hidden;
    border: 1px solid var(--border);
  }
  .m-fill{
    height:100%;
    width:0%;
    border-radius: 999px;
    transition: width 1.1s cubic-bezier(.22,1,.36,1), background .6s ease;
    background: var(--text-faint);
  }
  .m-fill.good{ background: linear-gradient(90deg,#22c98a,#34d399); }
  .m-fill.warn{ background: linear-gradient(90deg,#f5a524,#fbbf24); }
  .m-fill.bad{ background: linear-gradient(90deg,#f0575c,#f87171); }

  @media(max-width:680px){
    .metrics-panel{ grid-template-columns: repeat(2,1fr); }
  }

  /* ---------- Step sections ---------- */
  .step{ display:none; }
  .step.active{ display:block; animation: fadeUp .55s cubic-bezier(.22,1,.36,1) both; }

  .card{
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 26px 26px 24px;
    margin-bottom: 22px;
    position:relative;
    overflow:hidden;
  }
  .card::before{
    content:"";
    position:absolute; inset:0 0 auto 0; height:1px;
    background: linear-gradient(90deg, transparent, var(--accent-soft-2), transparent);
  }

  .lesson-card{
    background: linear-gradient(160deg, var(--surface-2), var(--surface));
    border: 1px solid var(--border-strong);
  }
  .lesson-card .tag{
    display:inline-block;
    font-size: 10.5px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--accent-2);
    background: var(--accent-soft);
    padding: 4px 10px;
    border-radius:6px;
    margin-bottom: 14px;
    font-weight:700;
  }
  .lesson-card h2{
    font-family: var(--serif);
    font-size: 24px;
    margin: 0 0 12px;
  }
  .lesson-card p{ color: var(--text-dim); margin: 0 0 10px; font-size: 15px; }
  .lesson-card .why{
    margin-top: 14px;
    padding: 14px 16px;
    background: rgba(59,130,246,0.06);
    border-left: 3px solid var(--accent);
    border-radius: 8px;
    font-size: 13.5px;
    color: var(--text-dim);
  }
  .lesson-card .why b{ color: var(--text); }

  /* Article / post display */
  .exhibit{
    background: var(--bg-2);
    border: 1px solid var(--border-strong);
    border-radius: var(--radius-sm);
    padding: 22px 24px;
    margin: 18px 0;
  }
  .exhibit .platform{
    font-size: 11px;
    letter-spacing: 1.2px;
    text-transform:uppercase;
    color: var(--text-faint);
    margin-bottom: 10px;
    display:flex; align-items:center; gap:8px;
  }
  .exhibit .platform::before{
    content:""; width:7px; height:7px; border-radius:50%;
    background: var(--accent-2);
  }
  .exhibit h3{
    font-family: var(--serif);
    font-size: 21px;
    line-height:1.35;
    margin: 0 0 14px;
    font-weight:700;
  }
  .exhibit .body-text{
    font-size: 15px;
    color: #cbd5e6;
    line-height: 1.75;
  }

  .phrase{
    cursor:pointer;
    border-bottom: 2px dashed rgba(148,163,184,0.45);
    padding: 1px 1px;
    border-radius: 3px;
    transition: background .2s ease, color .2s ease, border-color .2s ease;
  }
  .phrase:hover{ background: rgba(148,163,184,0.12); }
  .phrase.selected{
    background: var(--accent-soft-2);
    color:#fff;
    border-bottom-color: var(--accent-2);
  }
  .locked .phrase{ cursor: default; }
  .locked .phrase:hover{ background:inherit; }
  .phrase.correct{ background: var(--good-soft); border-bottom-color: var(--good); color:#c8f7e4; }
  .phrase.missed{ background: var(--bad-soft); border-bottom-color: var(--bad); color:#fecdd0; text-decoration: underline wavy var(--bad); }
  .phrase.false-positive{ background: var(--warn-soft); border-bottom-color: var(--warn); color:#fde3ab; }

  .hint-row{
    display:flex; align-items:center; gap:8px;
    font-size: 12.5px; color: var(--text-faint);
    margin-top: 14px;
  }

  /* Choice buttons */
  .choice-row{ display:flex; gap:10px; flex-wrap:wrap; margin: 16px 0 6px; }
  .btn{
    font-family: var(--sans);
    border: 1px solid var(--border-strong);
    background: var(--surface-2);
    color: var(--text);
    padding: 11px 20px;
    border-radius: 999px;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    transition: transform .15s ease, box-shadow .2s ease, border-color .2s ease, background .2s ease;
  }
  .btn:hover{ transform: translateY(-2px); border-color: var(--accent-soft-2); box-shadow: 0 8px 20px -8px rgba(59,130,246,.4); }
  .btn:active{ transform: translateY(0); }
  .btn.selected{ background: var(--accent); border-color: var(--accent); color:#fff; box-shadow: 0 8px 20px -8px rgba(59,130,246,.6); }
  .btn.primary{ background: var(--accent); border-color: var(--accent); color:#fff; }
  .btn.primary:hover{ box-shadow: 0 10px 26px -8px rgba(59,130,246,.65); }
  .btn:disabled{ opacity:.4; cursor:not-allowed; transform:none; box-shadow:none; }
  .btn.ghost{ background:transparent; }
  .btn-row{ display:flex; gap:12px; margin-top: 22px; flex-wrap:wrap; }

  .chip-row{ display:flex; gap:9px; flex-wrap:wrap; margin: 14px 0 4px; }
  .chip{
    font-size: 13px;
    padding: 8px 15px;
    border-radius: 999px;
    border: 1px solid var(--border-strong);
    background: var(--surface-2);
    color: var(--text-dim);
    cursor:pointer;
    transition: all .18s ease;
  }
  .chip:hover{ border-color: var(--accent-soft-2); color: var(--text); }
  .chip.selected{ background: var(--accent-soft); border-color: var(--accent); color: var(--accent-2); font-weight:600; }

  /* Reveal panel */
  .reveal{
    margin-top: 22px;
    display:none;
  }
  .reveal.show{ display:block; animation: fadeUp .6s ease both; }

  .score-banner{
    display:flex; align-items:center; gap:20px;
    padding: 20px 22px;
    border-radius: var(--radius-sm);
    background: linear-gradient(135deg, var(--surface-2), var(--surface-3));
    border: 1px solid var(--border-strong);
    margin-bottom: 18px;
    flex-wrap: wrap;
  }
  .score-ring{
    width: 78px; height:78px; border-radius:50%;
    flex-shrink:0;
    display:flex; align-items:center; justify-content:center;
    font-size: 20px; font-weight:800;
    background: conic-gradient(var(--accent-2) calc(var(--pct,0)*1%), var(--surface-3) 0);
    position:relative;
  }
  .score-ring::after{
    content: attr(data-pct) "%";
    position:absolute; inset:6px;
    background: var(--surface-2);
    border-radius:50%;
    display:flex; align-items:center; justify-content:center;
    font-size: 18px;
  }
  .score-banner .score-text b{ display:block; font-size:15px; margin-bottom:3px; }
  .score-banner .score-text span{ font-size: 13px; color: var(--text-dim); }

  .reveal-grid{
    display:grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
    margin: 16px 0;
  }
  @media(max-width:680px){ .reveal-grid{ grid-template-columns: 1fr; } }

  .info-block{
    background: var(--bg-2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 15px 17px;
  }
  .info-block .lbl{
    font-size: 10.5px;
    text-transform: uppercase;
    letter-spacing: 1.2px;
    color: var(--accent-2);
    font-weight:700;
    margin-bottom: 7px;
  }
  .info-block p{ margin:0; font-size: 13.8px; color: var(--text-dim); }

  .rewrite-block{
    margin-top: 16px;
    padding: 16px 18px;
    border-radius: var(--radius-sm);
    background: var(--good-soft);
    border: 1px solid rgba(52,211,153,0.35);
  }
  .rewrite-block .lbl{ font-size:10.5px; text-transform:uppercase; letter-spacing:1.2px; color: var(--good); font-weight:700; margin-bottom:6px; }
  .rewrite-block p{ margin:0; font-family: var(--serif); font-size: 16px; color:#e6fbf2; }

  .takeaway-block{
    margin-top: 14px;
    padding: 14px 18px;
    border-radius: var(--radius-sm);
    background: var(--accent-soft);
    border: 1px solid var(--accent-soft-2);
    font-size: 13.8px;
    display:flex; gap:10px; align-items:flex-start;
  }
  .takeaway-block .ic{ font-size:16px; }

  .legend{ display:flex; gap:16px; flex-wrap:wrap; margin-top:14px; font-size:12px; color: var(--text-faint); }
  .legend span{ display:inline-flex; align-items:center; gap:6px; }
  .legend i{ width:10px; height:10px; border-radius:3px; display:inline-block; }

  /* Dashboard */
  .dash-hero{
    text-align:center;
    padding: 34px 20px 30px;
  }
  .dash-hero .big-score{
    font-family: var(--serif);
    font-size: 64px;
    font-weight:800;
    background: linear-gradient(135deg, var(--accent-2), #a7c8ff);
    -webkit-background-clip:text; background-clip:text; color:transparent;
    line-height:1;
  }
  .dash-hero .big-score-label{ color: var(--text-dim); font-size:13px; letter-spacing:1px; text-transform:uppercase; margin-top:8px; }

  .dash-list{ list-style:none; margin:0; padding:0; display:flex; flex-direction:column; gap:10px; }
  .dash-list li{
    display:flex; gap:12px; align-items:flex-start;
    font-size: 14px; color: var(--text-dim);
    background: var(--bg-2);
    border: 1px solid var(--border);
    padding: 12px 14px;
    border-radius: var(--radius-sm);
  }
  .dash-list li b{ color: var(--text); }
  .dash-list .num-badge{
    flex-shrink:0;
    width: 22px; height:22px; border-radius:50%;
    background: var(--accent-soft); color: var(--accent-2);
    font-size: 12px; font-weight:800;
    display:flex; align-items:center; justify-content:center;
  }

  .redflag-banner{
    display:flex; gap:14px; align-items:center;
    padding: 18px 20px;
    border-radius: var(--radius-sm);
    background: var(--bad-soft);
    border: 1px solid rgba(248,113,113,0.4);
    margin-bottom: 20px;
  }
  .redflag-banner .ic{ font-size: 26px; }
  .redflag-banner b{ display:block; color:#ffd9db; font-size:15px; }
  .redflag-banner span{ font-size:13px; color:#f3c3c5; }

  footer{
    text-align:center;
    color: var(--text-faint);
    font-size: 12.5px;
    margin-top: 50px;
    padding-top: 20px;
    border-top: 1px solid var(--border);
  }

  @keyframes fadeUp{ from{ opacity:0; transform: translateY(14px);} to{ opacity:1; transform:none; } }
  @keyframes fadeDown{ from{ opacity:0; transform: translateY(-10px);} to{ opacity:1; transform:none; } }
</style>
</head>
<body>
<div class="shell">

  <header class="masthead">
    <div class="kicker"><span class="dot"></span>Media Literacy Lab · Midnight Blue</div>
    <h1>Media Integrity Analyzer</h1>
    <p>Learn to spot exaggeration, manipulation, and bias by investigating real-feeling headlines and posts — not by memorizing rules.</p>
  </header>

  <div class="progress-track" id="progressTrack"></div>

  <div class="metrics-panel" id="metricsPanel">
    <div class="metric">
      <div class="m-top"><span class="m-label">Headline Accuracy</span><span class="m-value" id="mv-headline">—</span></div>
      <div class="m-track"><div class="m-fill" id="mf-headline"></div></div>
    </div>
    <div class="metric">
      <div class="m-top"><span class="m-label">Source Reliability</span><span class="m-value" id="mv-source">—</span></div>
      <div class="m-track"><div class="m-fill" id="mf-source"></div></div>
    </div>
    <div class="metric">
      <div class="m-top"><span class="m-label">Emotional Manipulation</span><span class="m-value" id="mv-emotion">—</span></div>
      <div class="m-track"><div class="m-fill" id="mf-emotion"></div></div>
    </div>
    <div class="metric">
      <div class="m-top"><span class="m-label">Audience Targeting</span><span class="m-value" id="mv-target">—</span></div>
      <div class="m-track"><div class="m-fill" id="mf-target"></div></div>
    </div>
  </div>

  <!-- INTRO -->
  <section class="step active" id="step-intro">
    <div class="card lesson-card">
      <span class="tag">Welcome</span>
      <h2>Before we start</h2>
      <p>Every day you scroll past dozens of headlines and posts engineered to grab your attention. Most weren't written to inform you first — they were written to make you <i>click, feel, or share</i> first.</p>
      <p>In this lab you'll examine two fictional pieces of media like a detective: observe it, form your own judgment, then reveal what's really going on underneath.</p>
      <div class="why"><b>Why it matters:</b> Media literacy isn't about distrusting everything — it's about noticing the difference between information and persuasion, so you can decide for yourself with clear eyes.</div>
      <div class="btn-row">
        <button class="btn primary" id="beginBtn">Begin Challenge 1 →</button>
      </div>
    </div>
  </section>

  <!-- CHALLENGE 1 -->
  <section class="step" id="step-ch1">
    <div class="card lesson-card">
      <span class="tag">Challenge 1 · Headline Detective</span>
      <h2>Headlines aren't always honest summaries</h2>
      <p>Publishers earn attention, not necessarily trust — and dramatic words like "secretly," "shocking," or "never" earn more clicks than accurate ones.</p>
      <div class="why"><b>Why it matters:</b> A headline is often the <i>only</i> part of a story most people read. If it misrepresents the article, misinformation spreads even among careful readers.</div>
    </div>

    <div class="card">
      <div class="exhibit">
        <div class="platform">Fictional News Outlet</div>
        <h3 id="ch1-headline"></h3>
        <div class="body-text" id="ch1-article"></div>
      </div>

      <p style="font-size:14px;color:var(--text-dim);margin-top:18px;">Based on the headline alone — would you click this?</p>
      <div class="choice-row" id="ch1-choices">
        <button class="btn" data-choice="Yes">🙂 Yes</button>
        <button class="btn" data-choice="Maybe">🤔 Maybe</button>
        <button class="btn" data-choice="No">🚫 No</button>
      </div>

      <div class="hint-row">👆 Now click any phrases in the article above that feel exaggerated or misleading, then reveal the analysis.</div>

      <div class="btn-row">
        <button class="btn primary" id="ch1-reveal" disabled>Reveal Analysis</button>
      </div>

      <div class="reveal" id="ch1-revealPanel">
        <div class="score-banner">
          <div class="score-ring" id="ch1-ring" data-pct="0" style="--pct:0"></div>
          <div class="score-text">
            <b>Headline Accuracy Score</b>
            <span>How faithfully this headline represents its own article.</span>
          </div>
        </div>
        <div class="legend">
          <span><i style="background:var(--good)"></i>You correctly flagged this</span>
          <span><i style="background:var(--bad)"></i>Misleading — you missed it</span>
          <span><i style="background:var(--warn)"></i>You flagged it, but it's accurate</span>
        </div>
        <div class="reveal-grid" style="margin-top:16px;">
          <div class="info-block"><div class="lbl">Why It's Misleading</div><p id="ch1-explanation"></p></div>
          <div class="info-block"><div class="lbl">Your Click Instinct</div><p id="ch1-instinct"></p></div>
        </div>
        <div class="rewrite-block">
          <div class="lbl">Fair, Accurate Headline</div>
          <p id="ch1-fair"></p>
        </div>
        <div class="takeaway-block"><span class="ic">💡</span><span id="ch1-takeaway"></span></div>
        <div class="btn-row">
          <button class="btn primary" id="toCh2Btn">Continue to Challenge 2 →</button>
        </div>
      </div>
    </div>
  </section>

  <!-- CHALLENGE 2 -->
  <section class="step" id="step-ch2">
    <div class="card lesson-card">
      <span class="tag">Challenge 2 · Emotion Detector</span>
      <h2>Feelings can be engineered</h2>
      <p>Urgency, guilt, outrage, and fear all make posts spread faster — because we share what makes us <i>feel</i> something before we fact-check it.</p>
      <div class="why"><b>Why it matters:</b> Recognizing that a strong emotional reaction was the goal, not a side effect, helps you pause before reacting or sharing.</div>
    </div>

    <div class="card">
      <div class="exhibit">
        <div class="platform" id="ch2-platform"></div>
        <div class="body-text" id="ch2-post"></div>
      </div>

      <p style="font-size:14px;color:var(--text-dim);margin-top:18px;">How did reading this make you feel? (pick any that apply)</p>
      <div class="chip-row" id="ch2-emotions">
        <div class="chip" data-emotion="Anxious">Anxious</div>
        <div class="chip" data-emotion="Angry">Angry</div>
        <div class="chip" data-emotion="Guilty">Guilty</div>
        <div class="chip" data-emotion="Urgent">Urgent</div>
        <div class="chip" data-emotion="Suspicious">Suspicious</div>
        <div class="chip" data-emotion="Inspired">Inspired</div>
        <div class="chip" data-emotion="Sad">Sad</div>
        <div class="chip" data-emotion="Neutral">Neutral</div>
      </div>

      <div class="hint-row">👆 Now click the specific words or phrases above that influenced that feeling, then reveal the analysis.</div>

      <div class="btn-row">
        <button class="btn primary" id="ch2-reveal" disabled>Reveal Analysis</button>
      </div>

      <div class="reveal" id="ch2-revealPanel">
        <div class="reveal-grid">
          <div class="info-block"><div class="lbl">Target Audience</div><p id="ch2-audience"></p></div>
          <div class="info-block"><div class="lbl">Intended Emotional Response</div><p id="ch2-intended"></p></div>
        </div>
        <div class="info-block" style="margin-top:14px;"><div class="lbl">Manipulation Technique</div><p id="ch2-technique"></p></div>
        <div class="legend" style="margin-top:16px;">
          <span><i style="background:var(--good)"></i>You correctly flagged this</span>
          <span><i style="background:var(--bad)"></i>Emotional trigger you missed</span>
          <span><i style="background:var(--warn)"></i>Flagged, but fairly neutral</span>
        </div>
        <div class="rewrite-block">
          <div class="lbl">Neutral Rewrite</div>
          <p id="ch2-rewrite"></p>
        </div>
        <div class="takeaway-block"><span class="ic">💡</span><span id="ch2-takeaway"></span></div>
        <div class="btn-row">
          <button class="btn primary" id="toDashBtn">See My Dashboard →</button>
        </div>
      </div>
    </div>
  </section>

  <!-- DASHBOARD -->
  <section class="step" id="step-dash">
    <div class="card">
      <div class="dash-hero">
        <div class="big-score" id="dash-score">--</div>
        <div class="big-score-label">Overall Media Integrity Score</div>
      </div>
    </div>

    <div class="card" id="dash-redflag-card">
      <div class="redflag-banner">
        <span class="ic">🚩</span>
        <div>
          <b id="dash-redflag-title">Biggest Red Flag</b>
          <span id="dash-redflag-desc"></span>
        </div>
      </div>

      <div class="lbl" style="font-size:10.5px;text-transform:uppercase;letter-spacing:1.2px;color:var(--accent-2);font-weight:700;margin-bottom:10px;">What You Learned</div>
      <ul class="dash-list" id="dash-learned"></ul>
    </div>

    <div class="card">
      <div class="lbl" style="font-size:10.5px;text-transform:uppercase;letter-spacing:1.2px;color:var(--accent-2);font-weight:700;margin-bottom:10px;">Three Habits for Everyday Media Literacy</div>
      <ul class="dash-list" id="dash-habits"></ul>
      <div class="btn-row">
        <button class="btn primary" id="replayBtn">🔁 Replay With New Scenarios</button>
      </div>
    </div>
  </section>

  <footer>Media Integrity Analyzer · A fictional, offline media-literacy exercise. All headlines, posts, and studies referenced are invented for teaching purposes.</footer>
</div>

<script>
(function(){
  "use strict";

  /* ---------------- Scenario data ---------------- */
  var headlineScenarios = [
    {
      headline: "Doctors SHOCKED: Your Morning Coffee Is Secretly Destroying Your Liver, Shocking New Study Reveals",
      text: "A study released this week examined coffee habits among [[an observational study of 4,000 adults::N]] over ten years, focusing on [[people who drink more than six cups a day::N]]. Researchers found a statistical association between very high intake and elevated liver enzyme markers, though [[the researchers noted the link does not prove causation::N]]. The headline claims coffee is [[secretly destroying your liver::M]], but the paper itself calls the results preliminary. No physicians were quoted, despite the article's framing that [[doctors were shocked::M]] by the findings. The study's lead author called the results \"worth watching, not alarming\" — a description at odds with the [[shocking new study::M]] framing used above.",
      accuracyScore: 24,
      sourceReliability: 45,
      explanation: "The headline converts a modest, preliminary association into a dramatic, secret threat. No doctors were actually consulted, and the researchers' own cautious language was dropped from the framing.",
      fairHeadline: "Small Study Finds Possible Link Between Very High Coffee Intake and Liver Enzyme Changes",
      takeaway: "Words like \u201csecretly,\u201d \u201cshocking,\u201d and unnamed \u201cdoctors\u201d are common signals that a headline is dramatizing a modest finding."
    },
    {
      headline: "Senator's 'Secret' Tax Plan Would Bankrupt Every Family, Insiders Claim",
      text: "A proposal introduced this week by [[a state senator::N]] would adjust [[the property tax rate by 0.4 percent::N]] over [[three years, according to the bill's public text::N]]. The plan has been posted publicly on the legislature's website since Tuesday, which contradicts the headline's claim that it is [[a secret plan::M]]. No named source would confirm the claim that it would [[bankrupt every family::M]], and the sole 'insider' quoted turned out to be [[a rival campaign staffer with no independent expertise::M]]. Independent budget analysts estimated the change would cost [[the median household about $40 a year::N]].",
      accuracyScore: 18,
      sourceReliability: 30,
      explanation: "The plan is public, not secret, and the dramatic 'bankrupt every family' claim traces back to an undisclosed political rival, not an independent analysis.",
      fairHeadline: "Proposed 0.4% Property Tax Adjustment Would Cost Median Household About $40 a Year, Analysts Say",
      takeaway: "When a claim sounds extreme, check whether the source has a stake in the outcome and whether the underlying document is actually public."
    },
    {
      headline: "This New Phone Battery Will Never Die, Company Promises",
      text: "At a product event, executives said the new battery uses [[a redesigned lithium cell::N]] that lasts [[roughly 30 percent longer between charges::N]], based on [[internal lab testing under standard conditions::M]]. No company representative ever used the phrase [[will never die::M]]; the actual claim was about improved daily battery life, not infinite battery life. The company acknowledged that [[real-world results may vary by usage::N]], and [[third-party testing is still pending::N]] to confirm the 30 percent figure.",
      accuracyScore: 40,
      sourceReliability: 65,
      explanation: "The company never claimed infinite battery life — that phrase was invented for the headline. The real claim is more modest, self-reported, and still awaiting independent verification.",
      fairHeadline: "New Phone Battery Offers About 30% Longer Life in Company's Internal Tests",
      takeaway: "Compare a headline's exact wording to what's actually quoted in the article. Absolute words like 'never' and self-reported-only claims are red flags."
    }
  ];

  var emotionScenarios = [
    {
      platform: "Instagram Reel Caption",
      text: "🚨 Only 3 spots left in my transformation program!! I've watched people [[change their entire life::M]] in just 30 days and I don't want YOU to be the one who [[regrets waiting::M]] again. Every time I post this, it sells out — [[don't let this be the year you stayed the same::M]]. Comment 'READY' before spots [[disappear forever::M]]. This is [[literally your last chance::M]] to join at this price. [[Spots are limited to keep group sizes small::N]].",
      targetAudience: "Adults seeking a fitness or lifestyle change who may already feel behind on their personal goals.",
      intendedEmotion: "Urgency and fear of missing out (FOMO), plus mild shame about past inaction.",
      technique: "Artificial scarcity combined with loss-framing — implying permanent regret rather than describing specific, real benefits.",
      neutralRewrite: "My 30-day program has a few spots left this month. If you want structured support for your fitness goals, comment 'READY' and I'll send details.",
      takeaway: "Phrases like 'only X spots left' and 'last chance' are designed to make you decide fast, before you can think it through.",
      manipulationScore: 82,
      targetingScore: 75
    },
    {
      platform: "Local Facebook Group Post",
      text: "I am LIVID. [[The city council just voted::N]] to [[quietly gut funding::M]] for our neighborhood park while [[nobody was watching::M]]. This is exactly what happens when [[you let them get away with it::M]] — they think we won't notice. Share this NOW before they [[bury the story::M]]. If you actually [[care about this community::M]], you'll repost before it's too late. It's about [[funding for our neighborhood park::N]], plain and simple.",
      targetAudience: "Local residents emotionally invested in community identity, primed to feel ignored by local government.",
      intendedEmotion: "Outrage and moral urgency — the sense that staying silent makes you complicit.",
      technique: "Guilt-tripping combined with implied conspiracy ('quietly,' 'nobody was watching') to manufacture a villain and pressure sharing without verification.",
      neutralRewrite: "The city council voted this week to reduce park funding. If you want to weigh in, the next public meeting is listed below, along with how to contact your representative.",
      takeaway: "Posts that tell you exactly how to feel — and blame you for not sharing — are using guilt as a lever, not evidence.",
      manipulationScore: 88,
      targetingScore: 70
    },
    {
      platform: "Crowdfunding Campaign Excerpt",
      text: "Every single day you [[scroll past this::M]], another family goes without [[basic necessities::N]] tonight. I almost didn't post this because [[it's hard to relive::M]], but if it saves even one child, it's worth it. [[Most people won't even read this far::M]] — but if you're one of the few who actually care, [[a donation of any amount::N]] [[proves you're not like the rest::M]]. The clock is ticking and [[tomorrow may be too late::M]].",
      targetAudience: "Empathetic social media users, often reached through guilt and a desire to see themselves as compassionate.",
      intendedEmotion: "Guilt and moral superiority — donating becomes proof of being a 'good person' rather than a considered choice.",
      technique: "Guilt-shaming plus a false sense of moral exclusivity ('few who actually care') to pressure quick, unverified giving.",
      neutralRewrite: "Our organization is raising funds to provide basic necessities for families in need. Here's how the money is used and how to verify our nonprofit status before donating.",
      takeaway: "Legitimate causes describe their impact with specifics and transparency, not by implying you're a bad person for scrolling past.",
      manipulationScore: 79,
      targetingScore: 68
    }
  ];

  /* ---------------- State ---------------- */
  var state = {
    step: "intro",
    hIdx: -1,
    eIdx: -1,
    ch1: { choice: null, revealed: false },
    ch2: { emotions: [], revealed: false },
    metrics: { headline: null, source: null, emotion: null, target: null }
  };

  var STEPS = ["intro","ch1","ch2","dash"];
  var STEP_LABELS = { intro: "Intro", ch1: "Headline Detective", ch2: "Emotion Detector", dash: "Dashboard" };

  /* ---------------- Helpers ---------------- */
  function $(id){ return document.getElementById(id); }
  function pickIndex(arr, exclude){
    if(arr.length <= 1) return 0;
    var idx;
    do{ idx = Math.floor(Math.random()*arr.length); } while(idx === exclude);
    return idx;
  }

  function parseText(text){
    return text.replace(/\[\[(.+?)::(M|N)\]\]/g, function(match, phrase, flag){
      return '<span class="phrase" data-m="' + (flag === "M" ? "1" : "0") + '">' + phrase + '</span>';
    });
  }

  function colorClass(value, invert){
    var v = invert ? (100 - value) : value;
    if(v >= 70) return "good";
    if(v >= 40) return "warn";
    return "bad";
  }

  function setMetric(key, value, invert){
    var map = { headline:"headline", source:"source", emotion:"emotion", target:"target" };
    var suffix = map[key];
    var fill = $("mf-" + suffix);
    var val = $("mv-" + suffix);
    fill.className = "m-fill " + colorClass(value, invert);
    requestAnimationFrame(function(){ fill.style.width = value + "%"; });
    val.textContent = value + "%";
    val.classList.add("set");
    state.metrics[key] = value;
  }

  /* ---------------- Progress track ---------------- */
  function renderProgress(){
    var order = ["ch1","ch2","dash"];
    var html = "";
    for(var i=0;i<order.length;i++){
      var s = order[i];
      var cls = "progress-step";
      var currentIdx = order.indexOf(state.step);
      if(s === state.step) cls += " current";
      else if(currentIdx > i) cls += " done";
      html += '<div class="' + cls + '"><span class="num">' + (currentIdx > i ? "✓" : (i+1)) + '</span>' + STEP_LABELS[s] + '</div>';
      if(i < order.length-1) html += '<div class="progress-line"></div>';
    }
    $("progressTrack").innerHTML = html;
  }

  function goToStep(step){
    state.step = step;
    STEPS.forEach(function(s){
      $("step-" + s).classList.toggle("active", s === step);
    });
    renderProgress();
    window.scrollTo({ top: 0, behavior: "smooth" });
  }

  /* ---------------- Challenge 1 ---------------- */
  function renderCh1(){
    var s = headlineScenarios[state.hIdx];
    $("ch1-headline").textContent = s.headline;
    $("ch1-article").innerHTML = parseText(s.text);
    $("ch1-revealPanel").classList.remove("show");
    $("ch1-article").parentElement.parentElement.classList.remove("locked");
    $("ch1-reveal").disabled = true;
    state.ch1.choice = null;
    state.ch1.revealed = false;

    var choiceBtns = document.querySelectorAll("#ch1-choices .btn");
    choiceBtns.forEach(function(b){
      b.classList.remove("selected");
      b.onclick = function(){
        choiceBtns.forEach(function(x){ x.classList.remove("selected"); });
        this.classList.add("selected");
        state.ch1.choice = this.getAttribute("data-choice");
        $("ch1-reveal").disabled = false;
      };
    });

    var phrases = document.querySelectorAll("#ch1-article .phrase");
    phrases.forEach(function(p){
      p.onclick = function(){
        if(state.ch1.revealed) return;
        this.classList.toggle("selected");
      };
    });
  }

  function revealCh1(){
    if(state.ch1.revealed) return;
    state.ch1.revealed = true;
    var s = headlineScenarios[state.hIdx];
    var container = document.querySelector("#step-ch1 .exhibit");
    container.classList.add("locked");

    var phrases = document.querySelectorAll("#ch1-article .phrase");
    phrases.forEach(function(p){
      var isM = p.getAttribute("data-m") === "1";
      var isSel = p.classList.contains("selected");
      if(isM && isSel) p.classList.add("correct");
      else if(isM && !isSel) p.classList.add("missed");
      else if(!isM && isSel) p.classList.add("false-positive");
    });

    $("ch1-ring").style.setProperty("--pct", s.accuracyScore);
    $("ch1-ring").setAttribute("data-pct", s.accuracyScore);
    $("ch1-explanation").textContent = s.explanation;
    var instinctText = {
      Yes: "You said you'd click it — that's exactly the reaction this style of headline is built to trigger.",
      Maybe: "You hesitated — a healthy instinct. That hesitation is worth listening to.",
      No: "You wouldn't click it — your skepticism was well placed here."
    };
    $("ch1-instinct").textContent = instinctText[state.ch1.choice] || "";
    $("ch1-fair").textContent = s.fairHeadline;
    $("ch1-takeaway").textContent = s.takeaway;
    $("ch1-revealPanel").classList.add("show");

    setMetric("headline", s.accuracyScore, false);
    setMetric("source", s.sourceReliability, false);
  }

  /* ---------------- Challenge 2 ---------------- */
  function renderCh2(){
    var s = emotionScenarios[state.eIdx];
    $("ch2-platform").textContent = s.platform;
    $("ch2-post").innerHTML = parseText(s.text);
    $("ch2-revealPanel").classList.remove("show");
    document.querySelector("#step-ch2 .exhibit").classList.remove("locked");
    $("ch2-reveal").disabled = true;
    state.ch2.emotions = [];
    state.ch2.revealed = false;

    var chips = document.querySelectorAll("#ch2-emotions .chip");
    chips.forEach(function(c){
      c.classList.remove("selected");
      c.onclick = function(){
        var em = this.getAttribute("data-emotion");
        this.classList.toggle("selected");
        var idx = state.ch2.emotions.indexOf(em);
        if(idx > -1) state.ch2.emotions.splice(idx,1);
        else state.ch2.emotions.push(em);
        $("ch2-reveal").disabled = state.ch2.emotions.length === 0;
      };
    });

    var phrases = document.querySelectorAll("#ch2-post .phrase");
    phrases.forEach(function(p){
      p.onclick = function(){
        if(state.ch2.revealed) return;
        this.classList.toggle("selected");
      };
    });
  }

  function revealCh2(){
    if(state.ch2.revealed) return;
    state.ch2.revealed = true;
    var s = emotionScenarios[state.eIdx];
    document.querySelector("#step-ch2 .exhibit").classList.add("locked");

    var phrases = document.querySelectorAll("#ch2-post .phrase");
    phrases.forEach(function(p){
      var isM = p.getAttribute("data-m") === "1";
      var isSel = p.classList.contains("selected");
      if(isM && isSel) p.classList.add("correct");
      else if(isM && !isSel) p.classList.add("missed");
      else if(!isM && isSel) p.classList.add("false-positive");
    });

    $("ch2-audience").textContent = s.targetAudience;
    $("ch2-intended").textContent = s.intendedEmotion;
    $("ch2-technique").textContent = s.technique;
    $("ch2-rewrite").textContent = s.neutralRewrite;
    $("ch2-takeaway").textContent = s.takeaway;
    $("ch2-revealPanel").classList.add("show");

    setMetric("emotion", s.manipulationScore, true);
    setMetric("target", s.targetingScore, true);
  }

  /* ---------------- Dashboard ---------------- */
  function renderDashboard(){
    var m = state.metrics;
    var overall = Math.round(
      (m.headline + m.source + (100 - m.emotion) + (100 - m.target)) / 4
    );
    $("dash-score").textContent = overall;

    var risks = [
      { key:"headline", risk: 100 - m.headline, title: "Exaggerated Headlines", desc: "The headline you examined significantly overstated what its own article actually said." },
      { key:"source", risk: 100 - m.source, title: "Weak or Undisclosed Sources", desc: "Claims were backed by unnamed 'insiders' or unverified self-reported data rather than independent evidence." },
      { key:"emotion", risk: m.emotion, title: "Emotional Manipulation", desc: "The content leaned heavily on urgency, guilt, or outrage to drive a reaction rather than informing you." },
      { key:"target", risk: m.target, title: "Audience Targeting", desc: "The message was precision-tuned to a specific emotional vulnerability in its intended readers." }
    ];
    risks.sort(function(a,b){ return b.risk - a.risk; });
    var top = risks[0];
    $("dash-redflag-title").textContent = "Biggest Red Flag: " + top.title;
    $("dash-redflag-desc").textContent = top.desc;

    var hS = headlineScenarios[state.hIdx];
    var eS = emotionScenarios[state.eIdx];
    var learned = [
      "A dramatic headline (\"" + hS.headline.slice(0,44) + "…\") scored only " + hS.accuracyScore + "% on accuracy against its own article.",
      "A post from a " + eS.platform.toLowerCase() + " used " + eS.technique.split(" — ")[0].toLowerCase() + " to drive a " + Math.round(eS.manipulationScore) + "% manipulation rating.",
      "Sensational language (\"secretly,\" \"never,\" \"last chance,\" unnamed 'insiders') is a reliable signal to slow down before trusting or sharing.",
      "Emotional intensity and factual accuracy are two separate things — a post can feel true without being accurate."
    ];
    $("dash-learned").innerHTML = learned.map(function(item, i){
      return '<li><span class="num-badge">' + (i+1) + '</span><span>' + item + '</span></li>';
    }).join("");

    var habits = [
      "<b>Read past the headline</b> before reacting, judging, or sharing — headlines are optimized for clicks, not accuracy.",
      "<b>Ask who benefits</b> from you feeling urgency, guilt, or outrage right now — strong emotion is often the point, not a side effect.",
      "<b>Check the source</b> — is a claim backed by a named expert and verifiable data, or by 'insiders,' vague studies, or unnamed sources?"
    ];
    $("dash-habits").innerHTML = habits.map(function(item, i){
      return '<li><span class="num-badge">' + (i+1) + '</span><span>' + item + '</span></li>';
    }).join("");
  }

  /* ---------------- Replay ---------------- */
  function newScenarios(){
    state.hIdx = pickIndex(headlineScenarios, state.hIdx);
    state.eIdx = pickIndex(emotionScenarios, state.eIdx);
  }

  function resetMetricsUI(){
    ["headline","source","emotion","target"].forEach(function(suffix){
      $("mf-" + suffix).style.width = "0%";
      $("mf-" + suffix).className = "m-fill";
      var v = $("mv-" + suffix);
      v.textContent = "—";
      v.classList.remove("set");
    });
    state.metrics = { headline:null, source:null, emotion:null, target:null };
  }

  function replay(){
    resetMetricsUI();
    newScenarios();
    renderCh1();
    renderCh2();
    goToStep("ch1");
  }

  /* ---------------- Wire up ---------------- */
  $("beginBtn").addEventListener("click", function(){
    newScenarios();
    renderCh1();
    goToStep("ch1");
  });

  $("ch1-reveal").addEventListener("click", revealCh1);
  $("toCh2Btn").addEventListener("click", function(){
    renderCh2();
    goToStep("ch2");
  });

  $("ch2-reveal").addEventListener("click", revealCh2);
  $("toDashBtn").addEventListener("click", function(){
    renderDashboard();
    goToStep("dash");
  });

  $("replayBtn").addEventListener("click", replay);

  /* ---------------- Init ---------------- */
  renderProgress();
})();
</script>
</body>
</html>
