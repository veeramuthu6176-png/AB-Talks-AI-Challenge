<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Personal AI Playbook</title>
<style>
:root{
  --bg:#0f1117; --card:#171923; --text:#e2e8f0; --muted:#94a3b8; --accent:#6366f1; --accent2:#22d3ee; --border:#2d3748;
  --success:#10b981; --danger:#ef4444;
}
[data-theme="light"]{
  --bg:#f8fafc; --card:#ffffff; --text:#1e293b; --muted:#64748b; --border:#e2e8f0;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Inter, system-ui, -apple-system, Segoe UI, Roboto, sans-serif;}
body{background:var(--bg);color:var(--text);transition:.3s;}
.container{max-width:1200px;margin:0 auto;padding:20px;}
header{position:sticky;top:0;z-index:10;background:var(--card);border-bottom:1px solid var(--border);padding:12px 20px;display:flex;justify-content:space-between;align-items:center;}
.logo{font-weight:700;font-size:18px;} .logo span{color:var(--accent);}
.explainer{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#fff;padding:10px 16px;border-radius:8px;font-size:13px;margin:16px 0;display:flex;justify-content:space-between;align-items:center;}
nav{display:flex;gap:8px;margin-bottom:20px;flex-wrap:wrap;}
nav button{background:var(--card);border:1px solid var(--border);color:var(--text);padding:10px 16px;border-radius:8px;cursor:pointer;font-weight:500;}
nav button.active{background:var(--accent);border-color:var(--accent);}
.card{background:var(--card);border:1px solid var(--border);border-radius:12px
