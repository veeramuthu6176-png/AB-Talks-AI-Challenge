<style>
  @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap');

  :root{
    --ink:#0B1220;
    --panel:#121B2E;
    --panel-2:#0F1826;
    --line:#223047;
    --text:#E8EEF9;
    --muted:#8B9BB4;
    --red:#FF5D5D;
    --amber:#FFB84D;
    --green:#4ADE80;
    --cyan:#4CC9F0;
    --violet:#9B8CFF;
  }

  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;background:var(--ink);color:var(--text);font-family:'Inter',sans-serif;}
  body{
    background-image:
      radial-gradient(circle at 15% 0%, rgba(76,201,240,0.06), transparent 40%),
      radial-gradient(circle at 85% 20%, rgba(155,140,255,0.06), transparent 40%);
  }

  .mono{font-family:'JetBrains Mono',monospace;}
  .display{font-family:'Space Grotesk',sans-serif;}

  .wrap{max-width:1200px;margin:0 auto;padding:28px 24px 80px;}

  /* ---------- HEADER ---------- */
  .topbar{
    display:flex;justify-content:space-between;align-items:flex-end;
    border-bottom:1px solid var(--line);padding-bottom:18px;margin-bottom:28px;flex-wrap:wrap;gap:12px;
  }
  .topbar .eyebrow{
    font-size:11px;letter-spacing:.14em;text-transform:uppercase;color:var(--cyan);margin-bottom:6px;
  }
  .topbar h1{font-size:26px;margin:0;font-weight:700;letter-spacing:-0.01em;}
  .topbar .meta{text-align:right;color:var(--muted);font-size:12px;line-height:1.6;}
  .dot-live{display:inline-block;width:7px;height:7px;border-radius:50%;background:var(--green);margin-right:6px;box-shadow:0 0 8px var(--green);animation:pulse 2s infinite;}
  @keyframes pulse{0%,100%{opacity:1;}50%{opacity:.35;}}

  .disclaimer{
    background:var(--panel-2);border:1px solid var(--line);border-left:3px solid var(--violet);
    border-radius:8px;padding:12px 16px;font-size:12.5px;color:var(--muted);margin-bottom:28px;
  }
  .disclaimer b{color:var(--text);}

  /* ---------- SCORE HERO ---------- */
  .hero{display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:20px;}
  @media(max-width:760px){.hero{grid-template-columns:1fr;}}

  .score-card{
    background:linear-gradient(160deg,var(--panel),var(--panel-2));
    border:1px solid var(--line);border-radius:16px;padding:24px;
    display:flex;align-items:center;gap:22px;position:relative;overflow:hidden;
  }
  .score-card::after{
    content:"";position:absolute;inset:0;background:radial-gradient(circle at 90% 0%, rgba(255,255,255,0.05), transparent 55%);
  }
  .dial{position:relative;width:118px;height:118px;flex:none;}
  .dial svg{transform:rotate(-90deg);}
  .dial .bg{fill:none;stroke:var(--line);stroke-width:10;}
  .dial .fg{fill:none;stroke-width:10;stroke-linecap:round;transition:stroke-dashoffset 1.1s ease;}
  .dial .num{
    position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;
  }
  .dial .num span{font-size:28px;font-weight:700;font-family:'Space Grotesk',sans-serif;}
  .dial .num small{font-size:10px;color:var(--muted);letter-spacing:.06em;}

  .score-info .label{font-size:12px;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;margin-bottom:4px;}
  .score-info h2{margin:0 0 6px;font-size:19px;font-weight:600;}
  .badge{display:inline-flex;align-items:center;gap:6px;font-size:12px;font-weight:600;padding:4px 10px;border-radius:20px;}
  .badge.orange{background:rgba(255,184,77,0.12);color:var(--amber);border:1px solid rgba(255,184,77,0.3);}
  .badge.yellow{background:rgba(255,214,77,0.12);color:#FFD64D;border:1px solid rgba(255,214,77,0.3);}
  .badge.green{background:rgba(74,222,128,0.12);color:var(--green);border:1px solid rgba(74,222,128,0.3);}
  .badge.red{background:rgba(255,93,93,0.12);color:var(--red);border:1px solid rgba(255,93,93,0.3);}

  /* ---------- STAT ROW ---------- */
  .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-bottom:32px;}
  @media(max-width:760px){.stats{grid-template-columns:repeat(2,1fr);}}
  .stat{background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:16px;}
  .stat .v{font-family:'Space Grotesk',sans-serif;font-size:24px;font-weight:700;}
  .stat .k{font-size:11.5px;color:var(--muted);text-transform:uppercase;letter-spacing:.05em;margin-top:2px;}

  /* ---------- SECTION SHELL ---------- */
  .section{margin-bottom:30px;}
  .section-head{display:flex;align-items:baseline;justify-content:space-between;margin-bottom:14px;flex-wrap:wrap;gap:8px;}
  .section-head h3{font-family:'Space Grotesk',sans-serif;font-size:17px;margin:0;display:flex;align-items:center;gap:8px;}
  .section-head .tag{font-size:10.5px;padding:2px 8px;border-radius:20px;text-transform:uppercase;letter-spacing:.05em;font-weight:600;}
  .tag.fact{background:rgba(76,201,240,0.12);color:var(--cyan);border:1px solid rgba(76,201,240,.3);}
  .tag.estimate{background:rgba(155,140,255,0.12);color:var(--violet);border:1px solid rgba(155,140,255,.3);}

  .card{background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:20px;}

  /* ---------- HEATMAP ---------- */
  .heat-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;}
  @media(max-width:700px){.heat-grid{grid-template-columns:repeat(2,1fr);}}
  .heat-cell{border-radius:10px;padding:14px;border:1px solid var(--line);position:relative;}
  .heat-cell .cat{font-size:12.5px;font-weight:600;margin-bottom:6px;}
  .heat-cell .val{font-family:'JetBrains Mono',monospace;font-size:20px;font-weight:600;}
  .heat-cell .lv{font-size:10.5px;color:var(--muted);text-transform:uppercase;letter-spacing:.05em;}

  /* ---------- COMPANY RANKING ---------- */
  .rank-row{display:grid;grid-template-columns:150px 1fr 60px;align-items:center;gap:12px;margin-bottom:12px;font-size:13px;}
  .rank-row .name{font-weight:600;}
  .rank-row .services{color:var(--muted);font-size:11px;display:block;}
  .bar-track{background:var(--panel-2);border-radius:20px;height:10px;overflow:hidden;border:1px solid var(--line);}
  .bar-fill{height:100%;border-radius:20px;transition:width .6s ease;}
  .rank-row .pct{text-align:right;font-family:'JetBrains Mono',monospace;color:var(--muted);}

  /* ---------- MATRIX TABLE ---------- */
  table.matrix{width:100%;border-collapse:collapse;font-size:12.5px;}
  table.matrix th,table.matrix td{padding:9px 8px;border-bottom:1px solid var(--line);text-align:center;}
  table.matrix th{color:var(--muted);font-weight:600;text-transform:uppercase;font-size:10.5px;letter-spacing:.04em;}
  table.matrix td:first-child, table.matrix th:first-child{text-align:left;}
  table.matrix td:first-child{font-weight:600;}
  .dotmark{display:inline-block;width:9px;height:9px;border-radius:50%;}
  .dotmark.on{background:var(--cyan);box-shadow:0 0 6px rgba(76,201,240,.6);}
  .dotmark.off{background:var(--line);}
  .scrollx{overflow-x:auto;}

  /* ---------- RADAR ---------- */
  .radar-wrap{display:flex;gap:24px;align-items:center;flex-wrap:wrap;}
  .radar-svg{flex:none;}
  .radar-legend{flex:1;min-width:220px;}
  .radar-legend .item{display:flex;justify-content:space-between;font-size:13px;padding:7px 0;border-bottom:1px solid var(--line);}
  .radar-legend .item:last-child{border-bottom:none;}
  .radar-legend .item span.v{font-family:'JetBrains Mono',monospace;color:var(--amber);}
  .sweep{transform-origin:150px 150px;animation:spin 6s linear infinite;}
  @keyframes spin{to{transform:rotate(360deg);}}
  @media (prefers-reduced-motion: reduce){.sweep{animation:none;} .dot-live{animation:none;}}

  /* ---------- TWIN PROFILE ---------- */
  .twin{display:grid;grid-template-columns:1fr 1.4fr;gap:20px;}
  @media(max-width:760px){.twin{grid-template-columns:1fr;}}
  .twin-avatar{background:var(--panel-2);border:1px solid var(--line);border-radius:14px;padding:20px;display:flex;flex-direction:column;gap:10px;align-items:center;justify-content:center;text-align:center;}
  .twin-avatar .ring{width:84px;height:84px;border-radius:50%;background:conic-gradient(var(--cyan),var(--violet),var(--amber),var(--cyan));display:flex;align-items:center;justify-content:center;}
  .twin-avatar .ring div{width:70px;height:70px;border-radius:50%;background:var(--panel-2);display:flex;align-items:center;justify-content:center;font-size:26px;}
  .twin-traits{display:flex;flex-direction:column;gap:10px;}
  .trait{background:var(--panel-2);border:1px solid var(--line);border-radius:10px;padding:12px 14px;font-size:13px;}
  .trait b{display:block;font-size:12.5px;color:var(--violet);margin-bottom:3px;}

  /* ---------- ASSETS ---------- */
  .assets{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px;}
  .asset-card{background:var(--panel-2);border:1px solid var(--line);border-radius:12px;padding:16px;position:relative;}
  .asset-card .rank{font-family:'Space Grotesk',sans-serif;font-size:22px;font-weight:700;color:var(--violet);opacity:.5;position:absolute;top:10px;right:14px;}
  .asset-card h4{margin:0 0 6px;font-size:14px;}
  .asset-card p{margin:0;font-size:12px;color:var(--muted);line-height:1.5;}

  /* ---------- SIMULATOR ---------- */
  .sim{display:grid;grid-template-columns:1.1fr 1fr;gap:20px;}
  @media(max-width:760px){.sim{grid-template-columns:1fr;}}
  .toggle-list{display:flex;flex-wrap:wrap;gap:8px;}
  .toggle-chip{
    display:flex;align-items:center;gap:6px;background:var(--panel-2);border:1px solid var(--line);
    border-radius:20px;padding:6px 12px;font-size:12px;cursor:pointer;user-select:none;transition:.2s;
  }
  .toggle-chip input{accent-color:var(--cyan);}
  .toggle-chip.off{opacity:.4;text-decoration:line-through;}
  .sim-result{background:var(--panel-2);border:1px solid var(--line);border-radius:12px;padding:18px;display:flex;flex-direction:column;gap:14px;}
  .sim-result .row{display:flex;justify-content:space-between;align-items:center;}
  .sim-result .row .k{font-size:12.5px;color:var(--muted);}
  .sim-result .row .v{font-family:'JetBrains Mono',monospace;font-size:20px;font-weight:600;}

  .plan-list{display:flex;flex-direction:column;gap:10px;margin-top:16px;}
  .plan-item{display:flex;gap:12px;align-items:flex-start;background:var(--panel-2);border:1px solid var(--line);border-radius:10px;padding:12px 14px;font-size:13px;}
  .plan-item .n{font-family:'JetBrains Mono',monospace;color:var(--cyan);font-size:12px;flex:none;padding-top:1px;}

  /* ---------- WOW INSIGHTS ---------- */
  .wow-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:14px;}
  .wow-card{background:linear-gradient(150deg,var(--panel-2),var(--panel));border:1px solid var(--line);border-radius:12px;padding:16px;font-size:13px;line-height:1.5;}
  .wow-card .icon{font-size:18px;margin-bottom:8px;display:block;}

  /* ---------- VERDICT ---------- */
  .verdict{
    background:linear-gradient(150deg, rgba(255,184,77,0.08), rgba(155,140,255,0.06));
    border:1px solid var(--line);border-radius:16px;padding:26px;text-align:center;
  }
  .verdict .vscore{font-family:'Space Grotesk',sans-serif;font-size:40px;font-weight:700;margin:8px 0;}
  .verdict p{color:var(--muted);max-width:620px;margin:10px auto 0;font-size:13.5px;line-height:1.6;}

  footer{text-align:center;color:var(--muted);font-size:11.5px;margin-top:40px;line-height:1.7;}
</style>

<div class="wrap">

  <div class="topbar">
    <div>
      <div class="eyebrow"><span class="dot-live"></span>Live simulated scan</div>
      <h1 class="display">Digital Footprint Audit</h1>
    </div>
    <div class="meta mono">
      SOURCE: user-reported app list<br>
      SERVICES DETECTED: 15 &nbsp;|&nbsp; SCAN MODE: estimate-based
    </div>
  </div>

  <div class="disclaimer">
    <b>How to read this dashboard —</b> Everything below is generated only from the 15 services you listed. Claude has no access to any private, telecom, or third-party database — nothing here comes from real tracking data. Items marked <span style="color:var(--cyan)">FACT</span> come directly from your list. Items marked <span style="color:var(--violet)">ESTIMATE</span> are inferred, approximate, and never certain — treat them as directional, not diagnostic.
  </div>

  <!-- SCORE HERO -->
  <div class="hero">
    <div class="score-card">
      <div class="dial">
        <svg width="118" height="118" viewBox="0 0 118 118">
          <circle class="bg" cx="59" cy="59" r="50"></circle>
          <circle class="fg" id="fpRing" cx="59" cy="59" r="50" stroke="#FFB84D" stroke-dasharray="314" stroke-dashoffset="314"></circle>
        </svg>
        <div class="num"><span id="fpNum">--</span><small>/ 100</small></div>
      </div>
      <div class="score-info">
        <div class="label">Digital Footprint Score</div>
        <h2>How much of you is visible online</h2>
        <span class="badge orange" id="fpBadge">🟠 Significant</span>
      </div>
    </div>

    <div class="score-card">
      <div class="dial">
        <svg width="118" height="118" viewBox="0 0 118 118">
          <circle class="bg" cx="59" cy="59" r="50"></circle>
          <circle class="fg" id="pvRing" cx="59" cy="59" r="50" stroke="#FFB84D" stroke-dasharray="314" stroke-dashoffset="314"></circle>
        </svg>
        <div class="num"><span id="pvNum">--</span><small>/ 100</small></div>
      </div>
      <div class="score-info">
        <div class="label">Privacy Score</div>
        <h2>How well-defended that footprint is</h2>
        <span class="badge orange" id="pvBadge">🟠 Fair</span>
      </div>
    </div>
  </div>

  <!-- STAT ROW -->
  <div class="stats">
    <div class="stat"><div class="v mono" id="statServices">15</div><div class="k">Total services used</div></div>
    <div class="stat"><div class="v mono" id="statCompanies">11</div><div class="k">Parent companies</div></div>
    <div class="stat"><div class="v mono" id="statConcentration">27%</div><div class="k">Ecosystem concentration (Google)</div></div>
    <div class="stat"><div class="v mono" id="statSurface">High</div><div class="k">Estimated tracking surface</div></div>
  </div>

  <!-- EXPOSURE HEATMAP -->
  <div class="section">
    <div class="section-head"><h3>🗺️ Exposure Heatmap</h3><span class="tag estimate">Estimate</span></div>
    <div class="card">
      <div class="heat-grid" id="heatGrid"></div>
    </div>
  </div>

  <!-- COMPANY EXPOSURE RANKING -->
  <div class="section">
    <div class="section-head"><h3>🏢 Company Exposure Ranking</h3><span class="tag fact">Fact: services</span> <span class="tag estimate">Estimate: weighting</span></div>
    <div class="card" id="companyRanking"></div>
  </div>

  <!-- DATA COLLECTION MATRIX -->
  <div class="section">
    <div class="section-head"><h3>🧩 Data Collection Matrix</h3><span class="tag estimate">Estimate</span></div>
    <div class="card scrollx">
      <table class="matrix" id="matrixTable"></table>
    </div>
  </div>

  <!-- RISK RADAR -->
  <div class="section">
    <div class="section-head"><h3>📡 Risk Radar</h3><span class="tag estimate">Estimate</span></div>
    <div class="card radar-wrap">
      <svg class="radar-svg" width="300" height="300" viewBox="0 0 300 300" id="radarSvg"></svg>
      <div class="radar-legend" id="radarLegend"></div>
    </div>
  </div>

  <!-- DIGITAL TWIN PROFILE -->
  <div class="section">
    <div class="section-head"><h3>👤 Digital Twin Profile</h3><span class="tag estimate">Estimate — not a real identity</span></div>
    <div class="card twin">
      <div class="twin-avatar">
        <div class="ring"><div>🧬</div></div>
        <div style="font-weight:600;font-size:14px;">Modeled Profile</div>
        <div style="font-size:12px;color:var(--muted);">Built only from your 15 services — never from real personal data</div>
      </div>
      <div class="twin-traits">
        <div class="trait"><b>Demographic lean</b>Mix of Roblox, PUBG Mobile, Snapchat and TikTok suggests a younger or Gen‑Z‑skewing usage pattern, alongside general-audience apps like WhatsApp and YouTube.</div>
        <div class="trait"><b>Geographic signal</b>Meesho and Google Pay together suggest likely usage patterns common in the Indian mobile-commerce and UPI-payments market.</div>
        <div class="trait"><b>Lifestyle pattern</b>Heavy short-form video and messaging usage (TikTok, Instagram, Snapchat, Discord) points toward a socially-networked, mobile-first lifestyle.</div>
        <div class="trait"><b>Spending behavior</b>Presence of Amazon, Meesho and Google Pay suggests routine mobile shopping and digital-payment adoption.</div>
      </div>
    </div>
  </div>

  <!-- MOST VALUABLE DATA ASSETS -->
  <div class="section">
    <div class="section-head"><h3>💎 Most Valuable Data Assets</h3><span class="tag estimate">Estimate</span></div>
    <div class="assets">
      <div class="asset-card"><span class="rank">01</span><h4>Payment behavior</h4><p>Google Pay likely holds transaction timing, merchant, and amount signals — typically the single most monetizable data type.</p></div>
      <div class="asset-card"><span class="rank">02</span><h4>Social graph</h4><p>Instagram, Snapchat, WhatsApp and Discord together could map who you talk to and how often.</p></div>
      <div class="asset-card"><span class="rank">03</span><h4>Shopping intent</h4><p>Amazon, Meesho and Google Search may combine to signal what you're about to buy.</p></div>
      <div class="asset-card"><span class="rank">04</span><h4>Content preferences</h4><p>YouTube, Spotify and TikTok likely build a detailed taste and attention profile.</p></div>
      <div class="asset-card"><span class="rank">05</span><h4>Gaming & peer network</h4><p>Roblox, PUBG Mobile and Discord may reveal peer groups and play patterns.</p></div>
    </div>
  </div>

  <!-- WOW INSIGHTS -->
  <div class="section">
    <div class="section-head"><h3>✨ WOW Insights</h3><span class="tag estimate">Estimate</span></div>
    <div class="wow-grid">
      <div class="wow-card"><span class="icon">🔗</span>4 of your 15 services — YouTube, Search, Pay, Photos — all funnel back to a single parent, Alphabet/Google.</div>
      <div class="wow-card"><span class="icon">🌍</span>Your 15 apps trace back to companies headquartered across at least three regions: the US, China (ByteDance) and India (Meesho).</div>
      <div class="wow-card"><span class="icon">⚡</span>TikTok carries the single highest estimated per-app tracking weight in your entire list.</div>
      <div class="wow-card"><span class="icon">🔒</span>iMessage is your lowest-tracking service on this list, largely due to end-to-end encryption by default.</div>
    </div>
  </div>

  <!-- PRIVACY IMPROVEMENT SIMULATOR -->
  <div class="section">
    <div class="section-head"><h3>🎛️ Privacy Improvement Simulator</h3><span class="tag estimate">Interactive Estimate</span></div>
    <div class="card sim">
      <div>
        <p style="margin-top:0;color:var(--muted);font-size:13px;">Uncheck a service to simulate removing it from your digital life. Scores recalculate instantly — this is a modeled estimate only.</p>
        <div class="toggle-list" id="toggleList"></div>
        <div class="plan-list">
          <div class="plan-item"><span class="n">01</span>Turn off ad personalization inside your Google Account and Meta Accounts Center.</div>
          <div class="plan-item"><span class="n">02</span>Review app permissions on Instagram, Snapchat and TikTok — revoke location and contacts access where not essential.</div>
          <div class="plan-item"><span class="n">03</span>Enable two-factor authentication on Google Pay, Amazon and WhatsApp.</div>
          <div class="plan-item"><span class="n">04</span>Regularly clear Google Search and YouTube history, or set auto-delete.</div>
          <div class="plan-item"><span class="n">05</span>Prefer iMessage/WhatsApp (encrypted) over Discord/Snapchat for sensitive conversations.</div>
          <div class="plan-item"><span class="n">06</span>Audit Roblox and PUBG Mobile privacy settings for public profile and chat visibility.</div>
        </div>
      </div>
      <div class="sim-result">
        <div class="row"><span class="k">Simulated Footprint Score</span><span class="v" id="simFp">--</span></div>
        <div class="row"><span class="k">Simulated Privacy Score</span><span class="v" id="simPv">--</span></div>
        <div class="row"><span class="k">Active services</span><span class="v" id="simCount">--</span></div>
        <div class="row"><span class="k">Active parent companies</span><span class="v" id="simCompanies">--</span></div>
      </div>
    </div>
  </div>

  <!-- FINAL VERDICT -->
  <div class="section">
    <div class="section-head"><h3>🧾 Final Verdict</h3><span class="tag estimate">Summary — not certainty</span></div>
    <div class="verdict">
      <div style="font-size:12px;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;">Overall Assessment</div>
      <div class="vscore" id="verdictLabel">Significant Exposure, Fair Defense</div>
      <p>Across 15 services and 11 parent companies, your footprint leans toward the <b>Significant</b> range — driven mainly by social, short-form video, and payment apps. Your privacy score sits in the <b>Fair</b> range: encrypted messaging (iMessage, WhatsApp) helps, but concentration around Alphabet/Google and high-tracking apps like TikTok and Instagram pull the score down. None of this is certain — it's a directional estimate based only on the services you listed.</p>
    </div>
  </div>

  <footer>
    This dashboard is generated entirely from a user-provided list of 15 services. No private, telecom, financial, or third-party tracking database was accessed. All demographic, behavioral, and lifestyle conclusions are labeled Estimates and should not be treated as factual or certain.
  </footer>

</div>

<script>
  // ---------- DATA MODEL ----------
  const services = [
    {name:"Instagram", company:"Meta", weight:9, cat:"Social & Messaging", data:["Location","Contacts","Browsing","Social Graph","Media"]},
    {name:"Snapchat", company:"Snap Inc.", weight:8, cat:"Social & Messaging", data:["Location","Contacts","Social Graph","Media"]},
    {name:"TikTok", company:"ByteDance", weight:10, cat:"Social & Messaging", data:["Location","Browsing","Social Graph","Media"]},
    {name:"YouTube", company:"Alphabet", weight:7, cat:"Entertainment & Media", data:["Browsing","Media"]},
    {name:"Discord", company:"Discord Inc.", weight:5, cat:"Social & Messaging", data:["Contacts","Social Graph"]},
    {name:"WhatsApp", company:"Meta", weight:6, cat:"Social & Messaging", data:["Contacts","Social Graph"]},
    {name:"iMessage", company:"Apple", weight:3, cat:"Social & Messaging", data:["Contacts"]},
    {name:"Spotify", company:"Spotify Technology", weight:4, cat:"Entertainment & Media", data:["Browsing","Media"]},
    {name:"Roblox", company:"Roblox Corp.", weight:6, cat:"Gaming", data:["Social Graph","Media"]},
    {name:"PUBG Mobile", company:"Krafton", weight:6, cat:"Gaming", data:["Location","Social Graph"]},
    {name:"Amazon", company:"Amazon", weight:8, cat:"Shopping", data:["Location","Payment","Browsing"]},
    {name:"Meesho", company:"Meesho", weight:6, cat:"Shopping", data:["Location","Payment","Browsing"]},
    {name:"Google Search", company:"Alphabet", weight:8, cat:"Search & Info", data:["Location","Browsing"]},
    {name:"Google Pay", company:"Alphabet", weight:9, cat:"Finance", data:["Payment","Location"]},
    {name:"Google Photos", company:"Alphabet", weight:6, cat:"Cloud & Photos", data:["Media","Location"]},
  ];
  const allDataTypes = ["Location","Contacts","Browsing","Payment","Social Graph","Media"];
  const maxWeightSum = services.length * 10;

  function scoreColor(v, kind){
    // kind: 'fp' (footprint, high=bad) or 'pv' (privacy, high=good)
    if(kind==='fp'){
      if(v<=30) return {c:'#4ADE80', label:'Minimal', emoji:'🟢', cls:'green'};
      if(v<=60) return {c:'#FFD64D', label:'Moderate', emoji:'🟡', cls:'yellow'};
      if(v<=80) return {c:'#FFB84D', label:'Significant', emoji:'🟠', cls:'orange'};
      return {c:'#FF5D5D', label:'Extensive', emoji:'🔴', cls:'red'};
    } else {
      if(v<=30) return {c:'#FF5D5D', label:'Weak', emoji:'🔴', cls:'red'};
      if(v<=60) return {c:'#FFB84D', label:'Fair', emoji:'🟠', cls:'orange'};
      if(v<=80) return {c:'#FFD64D', label:'Good', emoji:'🟡', cls:'yellow'};
      return {c:'#4ADE80', label:'Strong', emoji:'🟢', cls:'green'};
    }
  }

  function computeScores(activeList){
    const sum = activeList.reduce((a,s)=>a+s.weight,0);
    const max = activeList.length*10 || 1;
    const footprint = Math.round((sum/max)*100 * (activeList.length? (activeList.length/services.length)*0.4+0.6 : 0));
    // concentration penalty for privacy
    const companyCounts = {};
    activeList.forEach(s=>companyCounts[s.company]=(companyCounts[s.company]||0)+1);
    const maxShare = activeList.length? Math.max(...Object.values(companyCounts))/activeList.length : 0;
    const encryptedBonus = activeList.some(s=>s.name==="iMessage"||s.name==="WhatsApp") ? 8 : 0;
    let privacy = 100 - Math.round(footprint*0.55) - Math.round(maxShare*25) + encryptedBonus;
    privacy = Math.max(0, Math.min(100, privacy));
    return {footprint: Math.max(0,Math.min(100,footprint)), privacy, companyCounts};
  }

  function setRing(ringId, numId, badgeId, value, kind){
    const ring = document.getElementById(ringId);
    const num = document.getElementById(numId);
    const badge = document.getElementById(badgeId);
    const info = scoreColor(value, kind);
    const circumference = 314;
    const offset = circumference - (value/100)*circumference;
    ring.style.stroke = info.c;
    ring.setAttribute('stroke-dashoffset', offset);
    num.textContent = value;
    if(badge){
      badge.className = 'badge ' + info.cls;
      badge.textContent = info.emoji + ' ' + info.label;
    }
    return info;
  }

  // ---------- INITIAL FULL SCORES ----------
  const initial = computeScores(services);
  setRing('fpRing','fpNum','fpBadge', initial.footprint, 'fp');
  setRing('pvRing','pvNum','pvBadge', initial.privacy, 'pv');

  // ---------- HEATMAP ----------
  const cats = {};
  services.forEach(s=>{
    if(!cats[s.cat]) cats[s.cat] = {sum:0,n:0};
    cats[s.cat].sum += s.weight; cats[s.cat].n++;
  });
  const heatGrid = document.getElementById('heatGrid');
  Object.entries(cats).forEach(([cat,v])=>{
    const avg = v.sum/v.n;
    let color, lvl;
    if(avg>=8){color='rgba(255,93,93,0.14)';lvl='Very High';}
    else if(avg>=6.5){color='rgba(255,184,77,0.14)';lvl='High';}
    else if(avg>=5){color='rgba(255,214,77,0.12)';lvl='Moderate';}
    else {color='rgba(74,222,128,0.12)';lvl='Low';}
    const cell = document.createElement('div');
    cell.className='heat-cell';
    cell.style.background=color;
    cell.innerHTML = `<div class="cat">${cat}</div><div class="val mono">${avg.toFixed(1)}</div><div class="lv">${lvl} tracking intensity</div>`;
    heatGrid.appendChild(cell);
  });

  // ---------- COMPANY RANKING ----------
  const companyMap = {};
  services.forEach(s=>{
    if(!companyMap[s.company]) companyMap[s.company]={weight:0, apps:[]};
    companyMap[s.company].weight += s.weight;
    companyMap[s.company].apps.push(s.name);
  });
  const maxCompanyWeight = Math.max(...Object.values(companyMap).map(c=>c.weight));
  const rankingEl = document.getElementById('companyRanking');
  const sortedCompanies = Object.entries(companyMap).sort((a,b)=>b[1].weight-a[1].weight);
  const barColors = ['#FF5D5D','#FFB84D','#FFD64D','#4CC9F0','#9B8CFF'];
  sortedCompanies.forEach(([name,v],i)=>{
    const pct = Math.round((v.weight/maxCompanyWeight)*100);
    const row = document.createElement('div');
    row.className='rank-row';
    row.innerHTML = `
      <div><span class="name">${name}</span><span class="services">${v.apps.join(', ')}</span></div>
      <div class="bar-track"><div class="bar-fill" style="width:${pct}%;background:${barColors[i%barColors.length]}"></div></div>
      <div class="pct">${v.weight}</div>`;
    rankingEl.appendChild(row);
  });

  // ---------- MATRIX ----------
  const matrixTable = document.getElementById('matrixTable');
  let thead = '<tr><th>Service</th>' + allDataTypes.map(d=>`<th>${d}</th>`).join('') + '</tr>';
  let rows = services.map(s=>{
    return '<tr><td>'+s.name+'</td>' + allDataTypes.map(d=>{
      const on = s.data.includes(d);
      return `<td><span class="dotmark ${on?'on':'off'}"></span></td>`;
    }).join('') + '</tr>';
  }).join('');
  matrixTable.innerHTML = thead + rows;

  // ---------- RADAR ----------
  const radarAxes = [
    {label:'Behavioral Tracking', value: 82},
    {label:'Location Exposure', value: 74},
    {label:'Financial Data', value: 66},
    {label:'Social Graph Exposure', value: 88},
    {label:'Content Consumption', value: 70},
    {label:'Identity Linkage', value: 60},
  ];
  const cx=150, cy=150, R=105;
  const n = radarAxes.length;
  function pointFor(i, val){
    const angle = (Math.PI*2*i/n) - Math.PI/2;
    const r = (val/100)*R;
    return [cx + r*Math.cos(angle), cy + r*Math.sin(angle)];
  }
  const svg = document.getElementById('radarSvg');
  let svgHtml = '';
  // grid rings
  [0.25,0.5,0.75,1].forEach(f=>{
    let pts = [];
    for(let i=0;i<n;i++){ pts.push(pointFor(i, f*100).join(',')); }
    svgHtml += `<polygon points="${pts.join(' ')}" fill="none" stroke="#223047" stroke-width="1"/>`;
  });
  // axis lines + labels
  for(let i=0;i<n;i++){
    const [x,y] = pointFor(i,100);
    svgHtml += `<line x1="${cx}" y1="${cy}" x2="${x}" y2="${y}" stroke="#223047" stroke-width="1"/>`;
  }
  // data polygon
  let dataPts = radarAxes.map((a,i)=>pointFor(i,a.value).join(',')).join(' ');
  svgHtml += `<polygon points="${dataPts}" fill="rgba(255,184,77,0.18)" stroke="#FFB84D" stroke-width="2"/>`;
  radarAxes.forEach((a,i)=>{
    const [x,y] = pointFor(i,a.value);
    svgHtml += `<circle cx="${x}" cy="${y}" r="4" fill="#FFB84D"/>`;
  });
  // sweep signature element
  svgHtml += `<g class="sweep"><line x1="${cx}" y1="${cy}" x2="${cx}" y2="${cy-R}" stroke="#4CC9F0" stroke-width="2" opacity="0.7"/><circle cx="${cx}" cy="${cy}" r="${R}" fill="none" stroke="#4CC9F0" stroke-width="0.5" opacity="0.25"/></g>`;
  svgHtml += `<circle cx="${cx}" cy="${cy}" r="2.5" fill="#8B9BB4"/>`;
  svg.innerHTML = svgHtml;

  const radarLegend = document.getElementById('radarLegend');
  radarLegend.innerHTML = radarAxes.map(a=>`<div class="item"><span>${a.label}</span><span class="v">${a.value}</span></div>`).join('');

  // ---------- SIMULATOR ----------
  const toggleList = document.getElementById('toggleList');
  const activeState = {};
  services.forEach(s=> activeState[s.name] = true);

  services.forEach(s=>{
    const chip = document.createElement('label');
    chip.className='toggle-chip';
    chip.innerHTML = `<input type="checkbox" checked data-name="${s.name}"> ${s.name}`;
    toggleList.appendChild(chip);
  });

  function runSimulation(){
    const active = services.filter(s=>activeState[s.name]);
    const res = computeScores(active);
    document.getElementById('simFp').textContent = active.length? res.footprint : 0;
    document.getElementById('simPv').textContent = active.length? res.privacy : 100;
    document.getElementById('simCount').textContent = active.length;
    document.getElementById('simCompanies').textContent = Object.keys(res.companyCounts).length;
  }
  toggleList.addEventListener('change', (e)=>{
    if(e.target.matches('input[type=checkbox]')){
      const name = e.target.getAttribute('data-name');
      activeState[name] = e.target.checked;
      e.target.closest('.toggle-chip').classList.toggle('off', !e.target.checked);
      runSimulation();
    }
  });
  runSimulation();
</script>
