<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1, user-scalable=no">
<title>Face Puzzle</title>
<style>
  :root{
    --bg1:#1e1b4b;
    --bg2:#312e81;
    --accent:#818cf8;
    --accent2:#6366f1;
    --green:#22c55e;
    --orange:#f59e0b;
    --card:#ffffff;
    --text-dark:#1e1b2e;
    --muted:#6b7280;
  }
  *{box-sizing:border-box;}
  html,body{
    margin:0;
    padding:0;
    min-height:100%;
    font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,Helvetica,Arial,sans-serif;
    background:linear-gradient(135deg,var(--bg1),var(--bg2) 60%,#4c1d95);
    background-attachment:fixed;
    color:#f3f4f6;
    -webkit-tap-highlight-color:transparent;
  }
  #app{
    min-height:100vh;
    display:flex;
    flex-direction:column;
    align-items:center;
    padding:20px 14px 40px;
  }
  h1.title{
    font-size:26px;
    margin:6px 0 2px;
    letter-spacing:.5px;
    text-align:center;
    background:linear-gradient(90deg,#a5b4fc,#f0abfc);
    -webkit-background-clip:text;
    background-clip:text;
    color:transparent;
    font-weight:800;
  }
  p.subtitle{
    margin:0 0 20px;
    color:#c7d2fe;
    font-size:14px;
    text-align:center;
  }
  .screen{
    display:none;
    width:100%;
    max-width:560px;
    flex-direction:column;
    align-items:center;
    animation:fadeIn .35s ease;
  }
  .screen.active{display:flex;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}

  .card{
    background:rgba(255,255,255,0.06);
    border:1px solid rgba(255,255,255,0.12);
    backdrop-filter:blur(10px);
    border-radius:20px;
    padding:18px;
    width:100%;
    box-shadow:0 10px 30px rgba(0,0,0,.25);
  }

  .video-wrap{
    position:relative;
    width:100%;
    max-width:420px;
    aspect-ratio:1/1;
    margin:0 auto 16px;
    border-radius:18px;
    overflow:hidden;
    background:#000;
    border:2px solid rgba(255,255,255,.15);
  }
  video{
    width:100%;
    height:100%;
    object-fit:cover;
    transform:scaleX(-1);
    display:block;
  }
  .video-overlay-msg{
    position:absolute;
    inset:0;
    display:flex;
    align-items:center;
    justify-content:center;
    text-align:center;
    padding:20px;
    font-size:14px;
    color:#fca5a5;
    background:rgba(0,0,0,.55);
  }
  .video-overlay-msg.hidden{display:none;}

  .btn{
    border:none;
    border-radius:12px;
    padding:12px 20px;
    font-size:15px;
    font-weight:600;
    cursor:pointer;
    transition:transform .15s ease, box-shadow .15s ease, opacity .15s ease;
    margin:6px;
    color:white;
  }
  .btn:active{transform:scale(.96);}
  .btn:disabled{opacity:.5;cursor:not-allowed;}
  .btn-primary{background:linear-gradient(135deg,#6366f1,#818cf8);box-shadow:0 4px 14px rgba(99,102,241,.4);}
  .btn-secondary{background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.25);}
  .btn-success{background:linear-gradient(135deg,#16a34a,#22c55e);box-shadow:0 4px 14px rgba(34,197,94,.4);}
  .btn-warning{background:linear-gradient(135deg,#d97706,#f59e0b);box-shadow:0 4px 14px rgba(245,158,11,.35);}

  .btn-row{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    width:100%;
  }

  .preview-img-wrap{
    width:100%;
    max-width:320px;
    aspect-ratio:1/1;
    margin:0 auto 18px;
    border-radius:16px;
    overflow:hidden;
    border:2px solid rgba(255,255,255,.2);
  }
  .preview-img-wrap img{width:100%;height:100%;object-fit:cover;display:block;}

  .difficulty-options{
    display:flex;
    gap:10px;
    justify-content:center;
    flex-wrap:wrap;
    margin-bottom:6px;
  }
  .diff-btn{
    background:rgba(255,255,255,.08);
    border:2px solid rgba(255,255,255,.18);
    border-radius:14px;
    padding:16px 14px;
    color:#fff;
    cursor:pointer;
    text-align:center;
    min-width:90px;
    transition:all .15s ease;
  }
  .diff-btn:hover{border-color:var(--accent);background:rgba(129,140,248,.18);}
  .diff-btn .num{font-size:22px;font-weight:800;display:block;}
  .diff-btn .label{font-size:12px;color:#c7d2fe;}

  .stats-bar{
    display:flex;
    justify-content:space-between;
    width:100%;
    max-width:500px;
    margin:4px auto 14px;
    gap:8px;
    flex-wrap:wrap;
  }
  .stat-box{
    flex:1;
    min-width:90px;
    background:rgba(255,255,255,.08);
    border:1px solid rgba(255,255,255,.15);
    border-radius:12px;
    padding:8px 10px;
    text-align:center;
  }
  .stat-box .val{font-size:18px;font-weight:800;color:#fff;font-variant-numeric:tabular-nums;}
  .stat-box .lbl{font-size:11px;color:#c7d2fe;letter-spacing:.5px;text-transform:uppercase;}

  .puzzle-outer{
    width:min(500px,92vw);
    margin:0 auto 16px;
  }
  .puzzle-grid{
    position:relative;
    width:100%;
    aspect-ratio:1/1;
    border-radius:16px;
    overflow:visible;
    background:#000;
    border:2px solid rgba(255,255,255,.2);
    box-shadow:0 10px 30px rgba(0,0,0,.4);
    touch-action:none;
  }
  .cell-line{
    position:absolute;
    background:rgba(255,255,255,.15);
    pointer-events:none;
  }
  .tile{
    position:absolute;
    top:0;left:0;
    background-repeat:no-repeat;
    border:2px solid rgba(255,255,255,.25);
    box-sizing:border-box;
    border-radius:4px;
    cursor:grab;
    transition:transform .22s cubic-bezier(.2,.8,.2,1), border-color .2s ease, box-shadow .2s ease;
    user-select:none;
    -webkit-user-select:none;
    touch-action:none;
    will-change:transform;
  }
  .tile.dragging{
    z-index:500;
    cursor:grabbing;
    border-color:var(--orange);
    box-shadow:0 10px 26px rgba(0,0,0,.5);
    transition:border-color .2s ease, box-shadow .2s ease;
  }
  .tile.correct{
    border-color:var(--green);
    box-shadow:0 0 0 2px rgba(34,197,94,.55) inset;
  }
  .drop-indicator{
    position:absolute;
    border:2px dashed rgba(129,140,248,.9);
    border-radius:6px;
    pointer-events:none;
    display:none;
    z-index:400;
    transition:transform .06s linear;
  }

  .leaderboard{
    width:100%;
    margin-top:6px;
  }
  .leaderboard h3{
    margin:0 0 10px;
    font-size:15px;
    color:#e0e7ff;
    text-align:center;
    letter-spacing:.5px;
  }
  table.lb-table{
    width:100%;
    border-collapse:collapse;
    font-size:13px;
  }
  table.lb-table th, table.lb-table td{
    padding:6px 8px;
    text-align:center;
    border-bottom:1px solid rgba(255,255,255,.1);
  }
  table.lb-table th{color:#a5b4fc;font-weight:600;font-size:11px;text-transform:uppercase;letter-spacing:.5px;}
  table.lb-table td{color:#f3f4f6;}
  .lb-empty{text-align:center;color:#9ca3af;font-size:13px;padding:10px 0;}

  .overlay{
    position:fixed;
    inset:0;
    background:rgba(10,8,25,.78);
    display:none;
    align-items:center;
    justify-content:center;
    z-index:1000;
    padding:16px;
    backdrop-filter:blur(4px);
  }
  .overlay.active{display:flex;}
  .result-card{
    background:linear-gradient(160deg,#2e1065,#312e81);
    border:1px solid rgba(255,255,255,.15);
    border-radius:22px;
    padding:28px 24px;
    max-width:420px;
    width:100%;
    text-align:center;
    box-shadow:0 20px 60px rgba(0,0,0,.5);
    animation:popIn .3s ease;
  }
  @keyframes popIn{from{opacity:0;transform:scale(.85);}to{opacity:1;transform:scale(1);}}
  .result-card h2{
    margin:0 0 6px;
    font-size:26px;
    background:linear-gradient(90deg,#fde68a,#f0abfc);
    -webkit-background-clip:text;background-clip:text;color:transparent;
  }
  .result-card .sub{color:#c7d2fe;font-size:13px;margin-bottom:18px;}
  .result-stats{
    display:flex;
    justify-content:center;
    gap:10px;
    margin-bottom:18px;
    flex-wrap:wrap;
  }
  .result-stats .stat-box{min-width:100px;}

  .footer-note{
    margin-top:22px;
    font-size:11px;
    color:rgba(255,255,255,.35);
    text-align:center;
  }
  canvas{display:none;}
  @media (max-width:420px){
    h1.title{font-size:22px;}
    .diff-btn{min-width:78px;padding:12px 10px;}
  }
</style>
</head>
<body>
<div id="app">
  <h1 class="title">🧩 Face Puzzle</h1>
  <p class="subtitle">Snap a selfie, scramble it, and race to put your face back together.</p>

  <!-- CAMERA SCREEN -->
  <div class="screen active" id="screen-camera">
    <div class="card">
      <div class="video-wrap">
        <video id="video" autoplay playsinline muted></video>
        <div class="video-overlay-msg hidden" id="cameraMsg"></div>
      </div>
      <div class="btn-row">
        <button class="btn btn-primary" id="btnTakePhoto" disabled>📸 Take Photo</button>
        <button class="btn btn-secondary hidden" id="btnRetryCamera" style="display:none;">🔁 Retry Camera</button>
      </div>
    </div>
    <div class="card leaderboard" style="margin-top:16px;">
      <h3>🏆 Best Times</h3>
      <div id="lbContainerMain"></div>
    </div>
  </div>

  <!-- DIFFICULTY SCREEN -->
  <div class="screen" id="screen-difficulty">
    <div class="card">
      <div class="preview-img-wrap">
        <img id="previewImg" alt="Captured face preview">
      </div>
      <p style="text-align:center;margin:0 0 14px;color:#c7d2fe;font-size:14px;">Choose your difficulty</p>
      <div class="difficulty-options">
        <div class="diff-btn" data-n="3"><span class="num">3×3</span><span class="label">Easy</span></div>
        <div class="diff-btn" data-n="4"><span class="num">4×4</span><span class="label">Medium</span></div>
        <div class="diff-btn" data-n="5"><span class="num">5×5</span><span class="label">Hard</span></div>
      </div>
      <div class="btn-row" style="margin-top:6px;">
        <button class="btn btn-secondary" id="btnRetake1">🔁 Retake Photo</button>
      </div>
    </div>
  </div>

  <!-- PUZZLE SCREEN -->
  <div class="screen" id="screen-puzzle">
    <div class="stats-bar">
      <div class="stat-box"><div class="val" id="statTime">00:00.0</div><div class="lbl">Time</div></div>
      <div class="stat-box"><div class="val" id="statMoves">0</div><div class="lbl">Moves</div></div>
      <div class="stat-box"><div class="val" id="statCorrect">0 / 0</div><div class="lbl">Correct</div></div>
    </div>
    <div class="puzzle-outer">
      <div class="puzzle-grid" id="puzzleGrid">
        <div class="drop-indicator" id="dropIndicator"></div>
      </div>
    </div>
    <div class="btn-row">
      <button class="btn btn-secondary" id="btnRetake2">🔁 Retake Photo</button>
      <button class="btn btn-secondary" id="btnNewPhoto2">🖼️ New Photo</button>
      <button class="btn btn-warning" id="btnShuffleAgain">🔀 Shuffle Again</button>
    </div>
  </div>

  <div class="footer-note">Your photo stays in your browser — nothing is uploaded anywhere.</div>
</div>

<canvas id="captureCanvas" width="640" height="640"></canvas>

<!-- RESULTS OVERLAY -->
<div class="overlay" id="resultOverlay">
  <div class="result-card">
    <h2>🎉 Solved!</h2>
    <div class="sub" id="resultSub">Great job piecing yourself back together.</div>
    <div class="result-stats">
      <div class="stat-box"><div class="val" id="resTime">00:00.0</div><div class="lbl">Time</div></div>
      <div class="stat-box"><div class="val" id="resMoves">0</div><div class="lbl">Moves</div></div>
      <div class="stat-box"><div class="val" id="resDiff">3×3</div><div class="lbl">Difficulty</div></div>
    </div>
    <div class="leaderboard">
      <h3>🏆 Top 5 Best Times</h3>
      <div id="lbContainerResult"></div>
    </div>
    <div class="btn-row" style="margin-top:16px;">
      <button class="btn btn-success" id="btnPlayAgain">🔁 Play Again</button>
      <button class="btn btn-secondary" id="btnNewPhoto3">🖼️ New Photo</button>
    </div>
  </div>
</div>

<script>
(function(){
  "use strict";

  // ---------- STATE ----------
  var video = document.getElementById('video');
  var cameraMsg = document.getElementById('cameraMsg');
  var btnTakePhoto = document.getElementById('btnTakePhoto');
  var btnRetryCamera = document.getElementById('btnRetryCamera');
  var captureCanvas = document.getElementById('captureCanvas');
  var previewImg = document.getElementById('previewImg');
  var puzzleGrid = document.getElementById('puzzleGrid');
  var dropIndicator = document.getElementById('dropIndicator');
  var statTime = document.getElementById('statTime');
  var statMoves = document.getElementById('statMoves');
  var statCorrect = document.getElementById('statCorrect');
  var resultOverlay = document.getElementById('resultOverlay');

  var mediaStream = null;
  var capturedDataURL = null;
  var n = 3; // grid dimension
  var cellSize = 0;
  var pieceToCell = [];   // pieceId -> current cell index
  var cellToPiece = [];   // cell index -> pieceId
  var tileEls = [];       // pieceId -> DOM element
  var moves = 0;
  var startTime = 0;
  var timerInterval = null;
  var running = false;
  var currentDrag = null; // {pieceId, offsetX, offsetY}
  var lastElapsed = 0;

  var LB_KEY = 'facePuzzleLeaderboard';

  function showScreen(id){
    var screens = document.querySelectorAll('.screen');
    for (var i=0;i<screens.length;i++){ screens[i].classList.remove('active'); }
    document.getElementById(id).classList.add('active');
  }

  // ---------- CAMERA ----------
  function initCamera(){
    cameraMsg.classList.add('hidden');
    cameraMsg.textContent = '';
    btnRetryCamera.style.display = 'none';
    btnTakePhoto.disabled = true;

    if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia){
      showCameraError('Your browser does not support camera access. Please try a modern browser like Chrome, Firefox, or Safari.');
      return;
    }

    var constraints = {
      video: {
        facingMode: 'user',
        width: { ideal: 720 },
        height: { ideal: 720 }
      },
      audio: false
    };

    navigator.mediaDevices.getUserMedia(constraints).then(function(stream){
      mediaStream = stream;
      video.srcObject = stream;
      video.onloadedmetadata = function(){
        video.play();
        btnTakePhoto.disabled = false;
      };
    }).catch(function(err){
      var msg = 'Could not access your camera.';
      if (err && err.name === 'NotAllowedError'){
        msg = 'Camera access was denied. Please allow camera permissions in your browser settings and try again.';
      } else if (err && err.name === 'NotFoundError'){
        msg = 'No camera was found on this device.';
      } else if (err && err.name === 'NotReadableError'){
        msg = 'Your camera is already in use by another application.';
      }
      showCameraError(msg);
    });
  }

  function showCameraError(msg){
    cameraMsg.textContent = msg;
    cameraMsg.classList.remove('hidden');
    btnRetryCamera.style.display = 'inline-block';
    btnTakePhoto.disabled = true;
  }

  btnRetryCamera.addEventListener('click', initCamera);

  btnTakePhoto.addEventListener('click', function(){
    if (!video.videoWidth || !video.videoHeight) return;
    var vw = video.videoWidth, vh = video.videoHeight;
    var srcSize = Math.min(vw, vh);
    var srcX = (vw - srcSize) / 2;
    var srcY = (vh - srcSize) / 2;
    var size = captureCanvas.width; // 640
    var ctx = captureCanvas.getContext('2d');
    ctx.clearRect(0,0,size,size);
    ctx.save();
    ctx.translate(size, 0);
    ctx.scale(-1, 1); // mirror to match preview
    ctx.drawImage(video, srcX, srcY, srcSize, srcSize, 0, 0, size, size);
    ctx.restore();
    capturedDataURL = captureCanvas.toDataURL('image/jpeg', 0.92);
    previewImg.src = capturedDataURL;
    showScreen('screen-difficulty');
  });

  document.getElementById('btnRetake1').addEventListener('click', function(){
    showScreen('screen-camera');
  });
  document.getElementById('btnRetake2').addEventListener('click', function(){
    stopTimer();
    showScreen('screen-camera');
  });
  document.getElementById('btnNewPhoto2').addEventListener('click', function(){
    stopTimer();
    showScreen('screen-camera');
  });
  document.getElementById('btnNewPhoto3').addEventListener('click', function(){
    resultOverlay.classList.remove('active');
    stopTimer();
    showScreen('screen-camera');
  });

  // ---------- DIFFICULTY SELECTION ----------
  var diffButtons = document.querySelectorAll('.diff-btn');
  for (var d=0; d<diffButtons.length; d++){
    diffButtons[d].addEventListener('click', function(){
      n = parseInt(this.getAttribute('data-n'), 10);
      startPuzzle();
    });
  }

  document.getElementById('btnShuffleAgain').addEventListener('click', function(){
    startPuzzle();
  });

  document.getElementById('btnPlayAgain').addEventListener('click', function(){
    resultOverlay.classList.remove('active');
    startPuzzle();
  });

  // ---------- PUZZLE SETUP ----------
  function startPuzzle(){
    showScreen('screen-puzzle');
    buildBoard();
    moves = 0;
    updateMovesDisplay();
    updateCorrectDisplay();
    resetTimer();
    startTimer();
    // measure after layout settles
    requestAnimationFrame(function(){
      measureAndRender();
    });
  }

  function buildBoard(){
    var total = n * n;
    // clear existing tiles
    puzzleGrid.querySelectorAll('.tile').forEach(function(t){ t.remove(); });
    tileEls = new Array(total);
    pieceToCell = new Array(total);
    cellToPiece = new Array(total);

    var perm = [];
    for (var i=0;i<total;i++) perm.push(i);
    shuffleUntilScrambled(perm);

    for (var cell=0; cell<total; cell++){
      cellToPiece[cell] = perm[cell];
      pieceToCell[perm[cell]] = cell;
    }

    for (var p=0; p<total; p++){
      var tile = document.createElement('div');
      tile.className = 'tile';
      tile.dataset.pieceId = p;
      tile.draggable = false;
      tile.style.backgroundImage = 'url(' + capturedDataURL + ')';
      puzzleGrid.appendChild(tile);
      tileEls[p] = tile;
      attachDragHandlers(tile, p);
    }
  }

  function shuffleUntilScrambled(arr){
    var total = arr.length;
    var attempts = 0;
    do {
      for (var i = total - 1; i > 0; i--){
        var j = Math.floor(Math.random() * (i+1));
        var tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
      }
      attempts++;
    } while (isIdentity(arr) && attempts < 10);
  }

  function isIdentity(arr){
    for (var i=0;i<arr.length;i++){ if (arr[i] !== i) return false; }
    return true;
  }

  function measureAndRender(){
    var rect = puzzleGrid.getBoundingClientRect();
    cellSize = rect.width / n;
    for (var p=0;p<tileEls.length;p++){
      var tile = tileEls[p];
      tile.style.width = cellSize + 'px';
      tile.style.height = cellSize + 'px';
      var srcCol = p % n, srcRow = Math.floor(p / n);
      tile.style.backgroundSize = (cellSize * n) + 'px ' + (cellSize * n) + 'px';
      tile.style.backgroundPosition = (-srcCol * cellSize) + 'px ' + (-srcRow * cellSize) + 'px';
    }
    renderPositions(false);
  }

  function renderPositions(animate){
    for (var p=0; p<tileEls.length; p++){
      var cell = pieceToCell[p];
      var col = cell % n, row = Math.floor(cell / n);
      var x = col * cellSize, y = row * cellSize;
      var tile = tileEls[p];
      if (!animate) {
        tile.style.transition = 'none';
      } else {
        tile.style.transition = '';
      }
      tile.style.transform = 'translate(' + x + 'px,' + y + 'px)';
      if (!animate){
        // force reflow then restore transition
        void tile.offsetWidth;
        tile.style.transition = '';
      }
      if (cell === p){
        tile.classList.add('correct');
      } else {
        tile.classList.remove('correct');
      }
    }
  }

  window.addEventListener('resize', debounce(function(){
    if (!document.getElementById('screen-puzzle').classList.contains('active')) return;
    measureAndRender();
  }, 150));

  function debounce(fn, wait){
    var t;
    return function(){
      clearTimeout(t);
      var args = arguments;
      t = setTimeout(function(){ fn.apply(null, args); }, wait);
    };
  }

  // ---------- DRAG & DROP (Pointer Events, mouse + touch) ----------
  function attachDragHandlers(tile, pieceId){
    tile.addEventListener('pointerdown', function(e){
      e.preventDefault();
      if (typeof tile.setPointerCapture === 'function'){
        try { tile.setPointerCapture(e.pointerId); } catch(err){}
      }
      var gridRect = puzzleGrid.getBoundingClientRect();
      var tileCell = pieceToCell[pieceId];
      var tileX = (tileCell % n) * cellSize;
      var tileY = Math.floor(tileCell / n) * cellSize;
      currentDrag = {
        pieceId: pieceId,
        startCell: tileCell,
        offsetX: (e.clientX - gridRect.left) - tileX,
        offsetY: (e.clientY - gridRect.top) - tileY,
        gridRect: gridRect
      };
      tile.classList.add('dragging');
      dropIndicator.style.width = cellSize + 'px';
      dropIndicator.style.height = cellSize + 'px';
      dropIndicator.style.display = 'block';
      positionIndicatorForCell(tileCell);
    });

    tile.addEventListener('pointermove', function(e){
      if (!currentDrag || currentDrag.pieceId !== pieceId) return;
      var gridRect = currentDrag.gridRect;
      var rawX = (e.clientX - gridRect.left) - currentDrag.offsetX;
      var rawY = (e.clientY - gridRect.top) - currentDrag.offsetY;
      var maxPos = cellSize * n - cellSize;
      var clampedX = Math.min(Math.max(rawX, -cellSize*0.4), maxPos + cellSize*0.4);
      var clampedY = Math.min(Math.max(rawY, -cellSize*0.4), maxPos + cellSize*0.4);
      tile.style.transform = 'translate(' + clampedX + 'px,' + clampedY + 'px)';

      var centerX = clampedX + cellSize/2;
      var centerY = clampedY + cellSize/2;
      var col = Math.min(Math.max(Math.floor(centerX / cellSize), 0), n-1);
      var row = Math.min(Math.max(Math.floor(centerY / cellSize), 0), n-1);
      var targetCell = row * n + col;
      positionIndicatorForCell(targetCell);
    });

    function endDrag(e){
      if (!currentDrag || currentDrag.pieceId !== pieceId) return;
      var gridRect = currentDrag.gridRect;
      var rawX = (e.clientX - gridRect.left) - currentDrag.offsetX;
      var rawY = (e.clientY - gridRect.top) - currentDrag.offsetY;
      var centerX = rawX + cellSize/2;
      var centerY = rawY + cellSize/2;
      var col = Math.min(Math.max(Math.floor(centerX / cellSize), 0), n-1);
      var row = Math.min(Math.max(Math.floor(centerY / cellSize), 0), n-1);
      var targetCell = row * n + col;

      tile.classList.remove('dragging');
      dropIndicator.style.display = 'none';

      var originalCell = currentDrag.startCell;
      if (targetCell !== originalCell){
        var otherPiece = cellToPiece[targetCell];
        cellToPiece[targetCell] = pieceId;
        cellToPiece[originalCell] = otherPiece;
        pieceToCell[pieceId] = targetCell;
        pieceToCell[otherPiece] = originalCell;
        moves++;
        updateMovesDisplay();
      }
      renderPositions(true);
      updateCorrectDisplay();
      currentDrag = null;
      checkWin();
    }

    tile.addEventListener('pointerup', endDrag);
    tile.addEventListener('pointercancel', endDrag);
  }

  function positionIndicatorForCell(cell){
    var col = cell % n, row = Math.floor(cell / n);
    dropIndicator.style.transform = 'translate(' + (col*cellSize) + 'px,' + (row*cellSize) + 'px)';
  }

  // ---------- STATS ----------
  function updateMovesDisplay(){
    statMoves.textContent = moves;
  }

  function updateCorrectDisplay(){
    var correct = 0;
    for (var p=0;p<pieceToCell.length;p++){ if (pieceToCell[p] === p) correct++; }
    statCorrect.textContent = correct + ' / ' + pieceToCell.length;
    return correct;
  }

  // ---------- TIMER ----------
  function resetTimer(){
    clearInterval(timerInterval);
    running = false;
    lastElapsed = 0;
    statTime.textContent = '00:00.0';
  }

  function startTimer(){
    startTime = Date.now();
    running = true;
    clearInterval(timerInterval);
    timerInterval = setInterval(function(){
      lastElapsed = Date.now() - startTime;
      statTime.textContent = formatTime(lastElapsed);
    }, 100);
  }

  function stopTimer(){
    clearInterval(timerInterval);
    running = false;
  }

  function formatTime(ms){
    var totalTenths = Math.floor(ms / 100);
    var tenths = totalTenths % 10;
    var totalSeconds = Math.floor(totalTenths / 10);
    var seconds = totalSeconds % 60;
    var minutes = Math.floor(totalSeconds / 60);
    return pad(minutes) + ':' + pad(seconds) + '.' + tenths;
  }

  function pad(v){
    return v < 10 ? '0' + v : '' + v;
  }

  // ---------- WIN DETECTION ----------
  function checkWin(){
    for (var p=0;p<pieceToCell.length;p++){
      if (pieceToCell[p] !== p) return false;
    }
    if (!running) return true;
    stopTimer();
    var finalElapsed = lastElapsed;
    var finalMoves = moves;
    var diffLabel = n + '×' + n;
    document.getElementById('resTime').textContent = formatTime(finalElapsed);
    document.getElementById('resMoves').textContent = finalMoves;
    document.getElementById('resDiff').textContent = diffLabel;
    saveScore(finalElapsed, finalMoves, diffLabel);
    renderLeaderboard('lbContainerResult');
    resultOverlay.classList.add('active');
    return true;
  }

  // ---------- LEADERBOARD ----------
  function getLeaderboard(){
    try {
      var raw = localStorage.getItem(LB_KEY);
      if (!raw) return [];
      var parsed = JSON.parse(raw);
      if (!Array.isArray(parsed)) return [];
      return parsed;
    } catch(e){
      return [];
    }
  }

  function saveScore(elapsedMs, moveCount, difficultyLabel){
    var list = getLeaderboard();
    var now = new Date();
    var dateStr = now.toLocaleDateString();
    list.push({
      date: dateStr,
      time: elapsedMs,
      moves: moveCount,
      difficulty: difficultyLabel
    });
    list.sort(function(a,b){ return a.time - b.time; });
    list = list.slice(0, 5);
    try {
      localStorage.setItem(LB_KEY, JSON.stringify(list));
    } catch(e){}
  }

  function renderLeaderboard(containerId){
    var container = document.getElementById(containerId);
    if (!container) return;
    var list = getLeaderboard();
    if (list.length === 0){
      container.innerHTML = '<div class="lb-empty">No times yet — be the first to finish!</div>';
      return;
    }
    var html = '<table class="lb-table"><thead><tr>' +
      '<th>#</th><th>Time</th><th>Moves</th><th>Grid</th><th>Date</th>' +
      '</tr></thead><tbody>';
    for (var i=0;i<list.length;i++){
      var entry = list[i];
      html += '<tr>' +
        '<td>' + (i+1) + '</td>' +
        '<td>' + formatTime(entry.time) + '</td>' +
        '<td>' + entry.moves + '</td>' +
        '<td>' + entry.difficulty + '</td>' +
        '<td>' + entry.date + '</td>' +
        '</tr>';
    }
    html += '</tbody></table>';
    container.innerHTML = html;
  }

  // ---------- INIT ----------
  renderLeaderboard('lbContainerMain');
  initCamera();

  window.addEventListener('beforeunload', function(){
    if (mediaStream){
      mediaStream.getTracks().forEach(function(t){ t.stop(); });
    }
  });

})();
</script>
</body>
</html>
