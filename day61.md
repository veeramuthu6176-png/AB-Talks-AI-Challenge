<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>AB Talks 60-Day Claude AI Challenge — Veeramuthu A</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --blue-deep:#0B2545;
    --blue-mid:#123B6B;
    --line-cyan:#7EC8E3;
    --paper:#EAF4FB;
    --paper-dim:#CFE3F2;
    --stamp-red:#E4572E;
    --ink:#0B2545;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--blue-deep);
    color:var(--paper);
    font-family:'IBM Plex Sans',sans-serif;
    background-image:
      linear-gradient(var(--blue-mid) 1px, transparent 1px),
      linear-gradient(90deg, var(--blue-mid) 1px, transparent 1px);
    background-size:28px 28px;
    background-position:-1px -1px;
  }
  @media (prefers-reduced-motion: reduce){
    *{animation:none !important; transition:none !important;}
  }

  a{color:var(--line-cyan);}

  .wrap{max-width:980px;margin:0 auto;padding:0 24px 120px;}

  /* HERO */
  header.hero{
    position:relative;
    padding:72px 24px 56px;
    border-bottom:1px solid rgba(126,200,227,0.35);
    overflow:hidden;
  }
  .hero-inner{max-width:980px;margin:0 auto;position:relative;}
  .blueprint-tag{
    font-family:'IBM Plex Mono',monospace;
    font-size:12px;
    letter-spacing:0.14em;
    text-transform:uppercase;
    color:var(--line-cyan);
    border:1px solid rgba(126,200,227,0.5);
    display:inline-block;
    padding:4px 10px;
    border-radius:2px;
    opacity:0;
    animation:fadeUp .6s ease forwards;
  }
  h1.title{
    font-family:'Space Mono',monospace;
    font-weight:700;
    font-size:clamp(32px,5.5vw,54px);
    line-height:1.08;
    margin:18px 0 10px;
    max-width:760px;
    opacity:0;
    animation:fadeUp .7s ease .1s forwards;
  }
  .subtitle{
    font-size:17px;
    color:var(--paper-dim);
    max-width:600px;
    line-height:1.55;
    opacity:0;
    animation:fadeUp .7s ease .22s forwards;
  }
  .hero-meta{
    margin-top:34px;
    display:flex;
    flex-wrap:wrap;
    gap:28px;
    font-family:'IBM Plex Mono',monospace;
    font-size:13px;
    color:var(--paper-dim);
    opacity:0;
    animation:fadeUp .7s ease .34s forwards;
  }
  .hero-meta div span{display:block;color:var(--line-cyan);font-size:11px;letter-spacing:.08em;text-transform:uppercase;margin-bottom:4px;}
  .hero-meta div strong{color:var(--paper);font-weight:600;font-size:14px;}

  @keyframes fadeUp{from{opacity:0;transform:translateY(14px);}to{opacity:1;transform:translateY(0);}}

  /* SIGNATURE: rolled ribbon timeline */
  .journey{padding:64px 0 20px;position:relative;}
  .journey h2{
    font-family:'Space Mono',monospace;
    font-size:22px;
    letter-spacing:0.02em;
    margin-bottom:8px;
  }
  .journey p.lead{color:var(--paper-dim);max-width:640px;margin-bottom:48px;line-height:1.6;font-size:15px;}

  .timeline{position:relative;padding-left:36px;}
  .timeline::before{
    content:"";
    position:absolute;
    left:9px;top:6px;bottom:6px;
    width:2px;
    background:linear-gradient(var(--line-cyan), rgba(126,200,227,0.15));
  }
  .phase{
    position:relative;
    margin-bottom:38px;
    padding:20px 22px 22px;
    background:rgba(234,244,251,0.04);
    border:1px solid rgba(126,200,227,0.28);
    border-radius:3px;
    opacity:0;
    animation:fadeUp .6s ease forwards;
  }
  .phase::before{
    content:attr(data-num);
    position:absolute;
    left:-36px;
    top:18px;
    width:20px;height:20px;
    background:var(--blue-deep);
    border:2px solid var(--line-cyan);
    border-radius:50%;
    font-family:'IBM Plex Mono',monospace;
    font-size:10px;
    display:flex;align-items:center;justify-content:center;
    color:var(--line-cyan);
  }
  .phase .range{
    font-family:'IBM Plex Mono',monospace;
    font-size:11px;
    letter-spacing:.1em;
    text-transform:uppercase;
    color:var(--line-cyan);
    margin-bottom:6px;
  }
  .phase h3{margin:0 0 8px;font-family:'Space Mono',monospace;font-size:18px;}
  .phase p{margin:0;color:var(--paper-dim);font-size:14.5px;line-height:1.6;}

  /* STAMP - signature element */
  .stamp{
    position:absolute;
    right:18px;
    top:16px;
    width:64px;height:64px;
    border:2px solid var(--stamp-red);
    border-radius:50%;
    display:flex;align-items:center;justify-content:center;
    transform:rotate(-12deg);
    opacity:0.85;
  }
  .stamp span{
    font-family:'Space Mono',monospace;
    font-size:9px;
    font-weight:700;
    color:var(--stamp-red);
    text-align:center;
    letter-spacing:.02em;
    text-transform:uppercase;
    line-height:1.2;
  }

  /* CAPSTONE callout */
  .capstone{
    margin-top:12px;
    padding:32px;
    background:linear-gradient(135deg, rgba(126,200,227,0.12), rgba(228,87,46,0.08));
    border:1px solid var(--line-cyan);
    border-radius:4px;
    position:relative;
  }
  .capstone .eyebrow{
    font-family:'IBM Plex Mono',monospace;
    font-size:11px;
    letter-spacing:.14em;
    text-transform:uppercase;
    color:var(--stamp-red);
    margin-bottom:10px;
  }
  .capstone h2{font-family:'Space Mono',monospace;font-size:26px;margin:0 0 12px;}
  .capstone p{color:var(--paper-dim);line-height:1.65;font-size:15px;max-width:680px;}
  .capstone .seal{
    position:absolute;
    top:28px;right:32px;
    width:88px;height:88px;
    border:2px dashed var(--stamp-red);
    border-radius:50%;
    display:flex;align-items:center;justify-content:center;
    text-align:center;
    transform:rotate(8deg);
  }
  .capstone .seal span{
    font-family:'Space Mono',monospace;
    font-size:11px;font-weight:700;
    color:var(--stamp-red);
    line-height:1.3;
  }

  footer{
    max-width:980px;margin:0 auto;padding:40px 24px 0;
    font-family:'IBM Plex Mono',monospace;
    font-size:12px;
    color:var(--paper-dim);
    border-top:1px solid rgba(126,200,227,0.25);
    text-align:center;
  }

  @media (max-width:600px){
    .stamp,.seal{display:none;}
    header.hero{padding:48px 18px 40px;}
  }
</style>
</head>
<body>

<header class="hero">
  <div class="hero-inner">
    <span class="blueprint-tag">Blueprint · Skills Ledger</span>
    <h1 class="title">60 Days of Building with Claude</h1>
    <p class="subtitle">A record of how AI foundations, prompting discipline, and hands-on iteration compounded into a shipped, production-ready capstone.</p>
    <div class="hero-meta">
      <div><span>Builder</span><strong>Veeramuthu A</strong></div>
      <div><span>Program</span><strong>AB Talks — 60-Day Claude AI Challenge</strong></div>
      <div><span>Outcome</span><strong>Capstone v1.0.0 shipped</strong></div>
    </div>
  </div>
</header>

<div class="wrap">
  <section class="journey">
    <h2>The Build Log</h2>
    <p class="lead">Ten phases, each layering a new capability on top of the last — from first prompts to an autonomous, multi-agent-capable build shipped as a real product.</p>

    <div class="timeline">
      <div class="phase" data-num="1" style="animation-delay:.02s">
        <div class="range">Phase 1</div>
        <h3>AI Foundations</h3>
        <p>Built a working mental model of how large language models reason, respond, and fail — the base layer everything after this relied on.</p>
      </div>

      <div class="phase" data-num="2" style="animation-delay:.06s">
        <div class="range">Phase 2</div>
        <h3>Effective Prompting</h3>
        <p>Moved from vague asks to structured, intentional prompts — learning how framing, context, and constraints change output quality.</p>
      </div>

      <div class="phase" data-num="3" style="animation-delay:.10s">
        <div class="range">Phase 3</div>
        <h3>Reasoning & Research Workflows</h3>
        <p>Practiced breaking problems into steps, verifying claims, and using AI as a research partner rather than a shortcut.</p>
      </div>

      <div class="phase" data-num="4" style="animation-delay:.14s">
        <div class="range">Phase 4</div>
        <h3>Automation & Product Thinking</h3>
        <p>Shifted from isolated tasks to designing workflows — starting to think in terms of users, requirements, and outcomes, not just outputs.</p>
      </div>

      <div class="phase" data-num="5" style="animation-delay:.18s">
        <div class="range">Phase 5</div>
        <h3>Increasingly Sophisticated AI Projects</h3>
        <p>Took on builds with more moving parts, applying earlier lessons to projects that demanded real architecture decisions.</p>
      </div>

      <div class="phase" data-num="6" style="animation-delay:.22s">
        <div class="range">Phase 6</div>
        <h3>Multimodal AI</h3>
        <p>Worked across text, image, and other input types — extending AI-assisted development beyond a single modality.</p>
      </div>

      <div class="phase" data-num="7" style="animation-delay:.26s">
        <div class="range">Phase 7</div>
        <h3>Knowledge Systems</h3>
        <p>Explored how AI systems store, retrieve, and reason over information — groundwork for more capable, context-aware applications.</p>
      </div>

      <div class="phase" data-num="8" style="animation-delay:.30s">
        <div class="range">Phase 8</div>
        <h3>Autonomous Agent Architectures</h3>
        <p>Designed systems that could plan and act with less direct supervision — a meaningful jump in complexity and responsibility.</p>
      </div>

      <div class="phase" data-num="9" style="animation-delay:.34s">
        <div class="range">Phase 9</div>
        <h3>Multi-Agent Collaboration</h3>
        <p>Coordinated multiple AI roles working together — the closest rehearsal for the full software development lifecycle ahead.</p>
      </div>

      <div class="phase" data-num="10" style="animation-delay:.38s">
        <div class="range">Phase 10 · Days 1–10 of the Capstone</div>
        <h3>The 10-Day Capstone Sprint</h3>
        <p>Every prior phase converged here: requirements gathering, build, debugging, deployment, and a final review — ending in a shipped, production-ready release.</p>
        <div class="stamp"><span>Skills<br>Applied</span></div>
      </div>
    </div>

    <div class="capstone">
      <div class="seal"><span>V1.0.0<br>Shipped</span></div>
      <div class="eyebrow">Final Milestone</div>
      <h2>From Challenge to Shipped Product</h2>
      <p>Sixty days of incremental skill-building closed with a complete software development lifecycle — requirements, implementation, debugging, and deployment — resulting in a production-ready Version&nbsp;1.0.0. This capstone is the proof of everything on this ledger.</p>
    </div>
  </section>
</div>

<footer>
  AB Talks · 60-Day Claude AI Challenge — Skills Ledger for Veeramuthu A
</footer>

</body>
</html>
