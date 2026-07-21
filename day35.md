<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🧩 Prompt Puzzle — Master AI Prompting Through Play</title>
<style>
  :root{
    --bg1:#0b0e1a; --bg2:#131832; --card:#161c38cc; --card-solid:#161c38;
    --accent:#7c5cff; --accent2:#22d3ee; --good:#34d399; --bad:#fb7185; --warn:#fbbf24;
    --text:#e9ecfb; --sub:#9aa3c7; --border:#2b3260;
    --radius:16px;
  }
  *{box-sizing:border-box;}
  html,body{height:100%;}
  body{
    margin:0; min-height:100vh; color:var(--text);
    font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
    background:
      radial-gradient(1200px 600px at 10% -10%, #2a1f6b55, transparent),
      radial-gradient(1000px 600px at 110% 10%, #0891b255, transparent),
      linear-gradient(160deg, var(--bg1), var(--bg2));
    background-attachment:fixed;
    overflow-x:hidden;
  }
  .hidden{display:none !important;}
  #app{max-width:920px; margin:0 auto; padding:18px 16px 60px;}
  header.topbar{
    display:flex; align-items:center; justify-content:space-between;
    padding:14px 18px; border-radius:var(--radius);
    background:var(--card); backdrop-filter:blur(10px);
    border:1px solid var(--border); margin-bottom:18px;
    position:sticky; top:10px; z-index:20;
  }
  header.topbar h1{font-size:16px; margin:0; font-weight:700; letter-spacing:.2px;}
  .badge{
    font-size:11px; padding:4px 10px; border-radius:99px; border:1px solid var(--border);
    background:linear-gradient(135deg,#7c5cff33,#22d3ee22); color:var(--sub); font-weight:600;
  }
  .badges{display:flex; gap:6px; flex-wrap:wrap;}

  .card{
    background:var(--card); border:1px solid var(--border); border-radius:var(--radius);
    padding:20px; backdrop-filter:blur(10px);
    box-shadow:0 10px 30px -12px #00000066;
  }
  .center{text-align:center;}
  .btn{
    appearance:none; border:none; cursor:pointer; font-weight:700; font-size:14px;
    padding:12px 22px; border-radius:12px; color:#0b0e1a;
    background:linear-gradient(135deg,var(--accent2),var(--accent));
    transition:transform .15s ease, box-shadow .15s ease, opacity .15s ease;
    box-shadow:0 8px 20px -8px #22d3ee66;
  }
  .btn:hover{transform:translateY(-2px); box-shadow:0 12px 26px -8px #7c5cffaa;}
  .btn:active{transform:translateY(0px) scale(.98);}
  .btn.secondary{
    background:transparent; color:var(--text); border:1px solid var(--border);
    box-shadow:none;
  }
  .btn.secondary:hover{border-color:var(--accent2); color:var(--accent2);}
  .btn:disabled{opacity:.4; cursor:not-allowed; transform:none;}
  .btn-row{display:flex; gap:10px; flex-wrap:wrap; margin-top:16px;}

  h2{margin:0 0 6px; font-size:20px;}
  p.sub{color:var(--sub); font-size:14px; line-height:1.5; margin:0 0 14px;}

  /* START SCREEN */
  .start-hero{padding:34px 22px;}
  .start-hero .emoji{font-size:44px;}
  .pill-row{display:flex; gap:8px; justify-content:center; flex-wrap:wrap; margin:14px 0 6px;}
  .pill{
    background:#ffffff0d; border:1px solid var(--border); padding:8px 14px; border-radius:99px;
    font-size:13px; color:var(--sub);
  }
  .how-list{text-align:left; m
