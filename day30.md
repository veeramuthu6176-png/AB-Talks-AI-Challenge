<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Supply Chain Builder</title>
<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<style>
  :root{
    --bg:#0b0f1a;
    --bg2:#111726;
    --card:#161d2e;
    --card-hover:#1c2438;
    --border:#232c42;
    --text:#e7ebf5;
    --muted:#8a93ab;
    --accent:#6d8dfd;
    --accent2:#22d3ee;
    --green:#3ddc97;
    --amber:#f5b942;
    --red:#f2556b;
    --radius:16px;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(circle at 15% 0%, rgba(109,141,253,0.12), transparent 45%),
      radial-gradient(circle at 85% 10%, rgba(34,211,238,0.10), transparent 40%),
      var(--bg);
    color:var(--text);
    font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
    min-height:100vh;
  }
  #root{min-height:100vh;}
  .app-shell{
    max-width:1280px;
    margin:0 auto;
    padding:24px 20px 60px;
  }
  .top-bar{
    display:flex;
    align-items:center;
    justify-content:space-between;
    margin-bottom:28px;
    flex-wrap:wrap;
    gap:12px;
  }
  .brand{
    display:flex;
    align-items:center;
    gap:10px;
    font-weight:700;
    font-size:18px;
    letter-spacing:0.2px;
  }
  .brand-badge{
    width:36px;height:36px;border-radius:10px;
    background:linear-gradient(135deg,var(--accent),var(--accent2));
    display:flex;align-items:center;justify-content:center;
    font-size:18px;
  }
  .progress-track{
    flex:1;
    max-width:420px;
    height:8px;
    background:var(--bg2);
    border-radius:99px;
    overflow:hidden;
    border:1px solid var(--border);
  }
  .progress-fill{
    height:100%;
    background:linear-gradient(90deg,var(--accent),var(--accent2));
    border-radius:99px;
    transition:width 0.6s cubic-bezier(.4,0,.2,1);
  }
  .step-label{
    font-size:13px;
    color:var(--muted);
    white-space:nowrap;
  }

  .card{
    background:var(--card);
    border:1px solid var(--border);
    border-radius:var(--radius);
    padding:24px;
    transition:transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease, background 0.25s ease;
  }
  .card:hover{
    transform:translateY(-2px);
    box-shadow:0 10px 30px rgba(0,0,0,0.35);
  }

  /* WELCOME */
  .hero{
    text-align:center;
    padding:60px 20px 20px;
    animation:fadeUp 0.6s ease;
  }
  .hero h1{
    font-size:42px;
    margin:0 0 14px;
    background:linear-gradient(90deg,#fff,var(--accent2));
    -webkit-background-clip:text;
    background-clip:text;
    color:transparent;
  }
  .hero p.sub{
    color:var(--muted);
    font-size:17px;
    max-width:640px;
    margin:0 auto 36px;
    line-height:1.6;
  }
  .explain-grid{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
    gap:16px;
    max-width:900px;
    margin:0 auto 40px;
    text-align:left;
  }
  .explain-grid .card h3{margin:0 0 8px;font-size:16px;}
  .explain-grid .card p{margin:0;color:var(--muted);font-size:14px;line-height:1.55;}
  .icon-lg{font-size:28px;display:block;margin-bottom:10px;}

  .btn{
    background:linear-gradient(135deg,var(--accent),var(--accent2));
    color:#0b0f1a;
    border:none;
    font-weight:700;
    font-size:15px;
    padding:14px 30px;
    border-radius:12px;
    cursor:pointer;
    transition:transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
    box-shadow:0 6px 20px rgba(109,141,253,0.25);
  }
  .btn:hover{transform:translateY(-2px);filter:brightness(1.08);box-shadow:0 10px 26px rgba(109,141,253,0.35);}
  .btn:active{transform:translateY(0px);}
  .btn.secondary{
    background:transparent;
    color:var(--text);
    border:1px solid var(--border);
    box-shadow:none;
  }
  .btn.secondary:hover{background:var(--card-hover);}
  .btn:disabled{opacity:0.4;cursor:not-allowed;transform:none;box-shadow:none;}

  /* COMPANY SCREEN */
  .company-wrap{
    max-width:820px;
    margin:20px auto;
    animation:fadeUp 0.5s ease;
  }
  .company-header{
    display:flex;
    align-items:center;
    gap:16px;
    margin-bottom:18px;
  }
  .company-logo{
    width:56px;height:56px;border-radius:14px;
    background:linear-gradient(135deg,var(--accent),var(--accent2));
    display:flex;align-items:center;justify-content:center;
    font-size:26px;flex-shrink
