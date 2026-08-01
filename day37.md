<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Task Compass</title>
<style>
  :root{
    --bg1:#0f172a; --bg2:#1e293b; --glass:rgba(255,255,255,0.08);
    --text:#e2e8f0; --accent:#38bdf8; --ok:#22c55e; --warn:#f59e0b;
    --pm:#38bdf8; --fe:#818cf8; --be:#4ade80; --qa:#f472b6; --ux:#fbbf24; 
    --cs:#60a5fa; --em:#a78bfa; --devops:#fb7185; --data:#2dd4bf; --sales:#f97316;
  }
  *{box-sizing:border-box;margin:0;padding:0;font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif}
  body{background:linear-gradient(135deg,var(--bg1),var(--bg2));color:var(--text);min-height:100vh;display:flex;justify-content:center;align-items:center;padding:20px}
  .app{width:100%;max-width:950px;background:var(--glass);backdrop-filter:blur(20px);border-radius:24px;padding:28px;border:1px solid rgba(255,255,255,0.1);box-shadow:0 20px 60px rgba(0,0,0,0.4)}
  h1{font-size:2rem;font-weight:800;background:linear-gradient(90deg,#38bdf8,#818cf8);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
  .subtitle{opacity:0.8;margin-bottom:20px}
  .progress{height:8px;background:rgba(255,255,255,0.1);border-radius:10px;overflow:hidden;margin-bottom:24px}
  .progress-bar{height:100%;background:linear-gradient(90deg,var(--accent),#818cf8);width:0%;transition:width 0.6s ease}
  .stage-title{font-size:1.2rem;margin-bottom:12px;font-weight:600}
  .task-card{background:rgba(0,0,0,0.3);padding:18px;border-radius:16px;margin-bottom:20px;border:1px solid rgba(255,255,255,0.08)}
  .roles{display:flex;flex-wrap:wrap;gap:10px;margin-bottom:20px;min-height:80px}
  .role{padding:10px 16px;border-radius:14px;background:rgba(255,255,255,0.1);cursor:grab;transition:all 0.2s;border:2px solid transparent;user-select:none}
  .role:hover{transform:translateY(-3px);background:rgba(255,255,255,0.15)}
  .role.dragging{opacity:0.5}
 
