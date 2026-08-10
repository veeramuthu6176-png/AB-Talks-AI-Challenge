<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DebugLens - AI Code Debugger</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism-tomorrow.min.css">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { 
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
      background: #0d1117; 
      color: #c9d1d9; 
      padding: 20px; 
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    .container { max-width: 900px; margin: 0 auto; flex: 1; }
    h1 { color: #58a6ff; text-align: center; margin-bottom: 8px; }
    .subtitle { text-align: center; color: #8b949e; margin-bottom: 24px; }
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 12px; padding: 20px; margin-bottom: 20px; }
    label { display: block; font-weight: 600; margin-bottom: 8px; color: #c9d1d9; }
    textarea, select { 
      width: 100%; 
      background: #0d1117; 
      border: 1px solid #30363d; 
      color: #c9d1d9; 
      border-radius: 8px; 
      padding: 12px; 
      font-family: 'Consolas', monospace; 
      font-size: 14px;
    }
    textarea { min-height: 150px; resize: vertical; }
    .row { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
    .checkbox { display: flex; align-items: center; gap: 8px; margin-top: 12px; }
    button { 
      width: 100%; 
      background: #238636; 
      color: white; 
      border: none; 
      padding: 14px; 
      border-radius: 8px; 
      font-size: 16px; 
      font-weight: 600; 
      cursor: pointer; 
      margin-top: 16px;
    }
    button:hover { background: #2ea043; }
    button:disabled { background: #30363d; cursor: not-allowed; }
    .loader { display: none; text-align: center; padding: 20px; }
    .results { display: none; }
    .result-box { background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px; margin-bottom: 12px; }
    .result-box h3 { color: #58a6ff; font-size: 14px; margin-bottom: 8px; }
    pre { margin: 0; overflow-x: auto; }
    #errorBox { display: none; background: #da3633; color: white; padding: 12px; border-radius: 8px; margin-bottom: 16px; font-weight: 500; }
    .copy-btn { background: #1f6feb; padding: 8px 16px; font-size: 14px; width: auto; margin-top: 8px; }
    footer { text-align: center; padding: 20px; color: #8b949e; font-size: 12px; border-top: 1px solid #30363d; margin-top: 40px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>DebugLens</h1>
    <p class="
