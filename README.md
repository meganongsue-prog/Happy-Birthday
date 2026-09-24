<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sofia's 16th Birthday</title>
<style>
  :root{
    --blue:#2f6fd6;
    --blue-dark:#1f4fa8;
    --cream:#faf3e3;
    color-scheme: light;
  }
  *{ box-sizing: border-box; }
  html,body{
    height:100%; margin:0; padding:0;
    background:#0e1b33;
    font-family: 'Georgia', 'Iowan Old Style', serif;
    overflow:hidden;
    -webkit-tap-highlight-color: transparent;
  }
  body{
    display:flex; align-items:center; justify-content:center;
    padding-top:env(safe-area-inset-top,0px);
    padding-bottom:env(safe-area-inset-bottom,0px);
  }
  #stage{
    position:relative;
    width:min(94vw, calc(94vh * 0.7069));
    height:min(94vh, calc(94vw / 0.7069));
    aspect-ratio: 595 / 842;
    background:#7fb0e8;
    border-radius:18px;
    overflow:hidden;
    box-shadow:0 30px 80px rgba(0,0,0,0.55), 0 0 0 1px rgba(255,255,255,0.08);
  }
  .slide{
    position:absolute; inset:0;
    background-size:cover; background-position:center;
    opacity:0; visibility:hidden;
    pointer-events:none;
    transition: opacity .55s ease;
  }
  .slide.active{
    opacity:1; visibility:visible;
    pointer-events:auto;
  }
  .slide img.bg{
    position:absolute; inset:0; width:100%; height:100%; object-fit:cover;
    display:block;
    -webkit-user-drag:none; user-select:none;
  }

  /* --- generic arrow button --- */
  .nav-arrow{
    position:absolute;
    right:16px; bottom:16px;
    width:58px; height:58px;
    border-radius:50%;
    border:none;
    background:var(--blue);
    color:white;
    display:flex; align-items:center; justify-content:center;
    box-shadow:0 8px 20px rgba(0,0,0,0.35);
    cursor:pointer;
    z-index:20;
    opacity:0; transform:translateY(10px) scale(0.85);
    pointer-events:none;
    transition: opacity .4s ease, transform .4s ease, background .2s ease;
  }
  .nav-arrow.show{ opacity:1; transform:translateY(0) scale(1); pointer-events:auto; }
  .nav-arrow:hover{ background:var(--blue-dark); }
  .nav-arrow svg{ width:26px; height:26px; }

  .back-arrow{
    left:16px; right:auto;
  }

  /* --- slide 1 : cover --- */
  #s1 .bg{ animation: coverIn 1.1s ease both; }
  @keyframes coverIn{
    from{ opacity:0; transform:scale(1.06); }
    to{ opacity:1; transform:scale(1); }
  }

  /* --- slide 2 : blow candles --- */
  #s2 .hint{
    position:absolute;
    left:50%; bottom:9%;
    transform:translateX(-50%);
    width:82%;
    text-align:center;
    color:#0d2b57;
    background:rgba(255,255,255,0.88);
    border-radius:16px;
    padding:12px 14px;
    font-size:clamp(11px, 2.6vw, 14px);
    line-height:1.45;
    box-shadow:0 6px 18px rgba(0,0,0,0.18);
    z-index:15;
  }
  #s2 .hint b{ color:var(--blue-dark); }
  #s2 .blow-btn{
    display:inline-flex; align-items:center; gap:6px;
    margin-top:8px;
    background:var(--blue);
    color:#fff;
    border:none;
    border-radius:999px;
    padding:8px 16px;
    font-family:inherit;
    font-size:clamp(11px,2.6vw,13px);
    cursor:pointer;
    user-select:none;
  }
  #s2 .blow-btn:active{ background:var(--blue-dark); }
  #s2 .mic-btn{
    background:transparent;
    border:1px solid var(--blue);
    color:var(--blue-dark);
    border-radius:999px;
    padding:8px 14px;
    font-family:inherit;
    font-size:clamp(11px,2.6vw,13px);
    cursor:pointer;
    margin-left:6px;
  }
  #s2 .status{
    display:block;
    margin-top:6px;
    font-size:clamp(10px,2.3vw,12px);
    color:#3a4a63;
    min-height:14px;
  }
  #s2 .progress-wrap{
    width:100%;
    height:9px;
    margin-top:9px;
    background:rgba(13,43,87,0.15);
    border-radius:999px;
    overflow:hidden;
  }
  #s2 .progress-bar{
    height:100%; width:0%;
    background:linear-gradient(90deg, var(--blue), #6fb1ff);
    border-radius:999px;
  }
  .flame-cover{
    position:absolute;
    width:3.4%; height:8%;
    left:0; top:0;
    transform:translate(-50%,-50%) scale(0.4);
    border-radius:50%;
    background:radial-gradient(ellipse at center, rgba(107,162,218,1) 55%, rgba(107,162,218,0) 100%);
    opacity:0;
    pointer-events:none;
    z-index:8;
    transition:opacity .35s ease, transform .35s ease;
  }
  .flame-cover.show{ opacity:1; transform:translate(-50%,-50%) scale(1); }
  .smoke{
    position:absolute;
    width:10px; height:10px;
    border-radius:50%;
    background:rgba(255,255,255,0.8);
    pointer-events:none;
    z-index:9;
    transform:translate(-50%,-50%);
    animation: smokeRise 1.1s ease-out forwards;
  }
  @keyframes smokeRise{
    0%{ opacity:0.85; transform:translate(-50%,-50%) scale(0.6); }
    100%{ opacity:0; transform:translate(-50%,-220%) scale(2.1); }
  }

  /* --- video-like auto sequence slides 3-6 --- */
  #s3, #s4, #s5, #s6{ background:#7fb0e8; }
  #s3 .bg, #s4 .bg{
    position:absolute; inset:0; width:100%; height:100%; object-fit:cover;
  }
  #s4.active .bg{ animation: flashPop .45s ease both; }
  @keyframes flashPop{
    0%{ opacity:0; transform:scale(0.97); filter:brightness(1); }
    35%{ opacity:1; transform:scale(1.01); filter:brightness(1.9); }
    100%{ opacity:1; transform:scale(1); filter:brightness(1); }
  }
  .whiteflash{
    position:absolute; inset:0; background:#fff; opacity:0; z-index:5; pointer-events:none;
  }
  #s4.active .whiteflash{ animation: flashWhite .45s ease both; }
  @keyframes flashWhite{
    0%{ opacity:0; }
    18%{ opacity:.95; }
    100%{ opacity:0; }
  }

  #s5 .strip{
    position:absolute; inset:0; width:100%; height:100%; object-fit:cover;
    transform:translateY(0);
  }
  #s5.active .strip{ animation: stripUp 1.05s cubic-bezier(.2,.8,.2,1) both; }
  @keyframes stripUp{
    0%{ transform:translateY(70%); opacity:0; }
    100%{ transform:translateY(0); opacity:1; }
  }

  #s6 .bg{ position:absolute; inset:0; width:100%; height:100%; object-fit:cover; opacity:0; }
  /* middle column (photobooth strip) stays put — no animation */
  #s6 .colM{ opacity:1; transform:none; }
  /* left & right columns rise up into place */
  #s6.active .colL{ animation: comeUp .75s cubic-bezier(.2,.8,.2,1) both; animation-delay:.1s; }
  #s6.active .colR{ animation: comeUp .75s cubic-bezier(.2,.8,.2,1) both; animation-delay:.35s; }
  @keyframes comeUp{ from{ transform:translateY(65%); opacity:0;} to{ transform:translateY(0); opacity:1;} }
  #s6 .col{
    position:absolute; top:0; height:100%;
    background-image:var(--fullbg);
    background-size: 300% 100%;
    background-repeat:no-repeat;
  }
  .colL{ left:0; width:33.4%; background-position: 0% 0; }
  .colM{ left:33.3%; width:33.4%; background-position: 50% 0; }
  .colR{ left:66.6%; width:33.4%; background-position: 100% 0; }

  .skip-hint{
    position:absolute; top:12px; left:50%; transform:translateX(-50%);
    color:rgba(255,255,255,0.85);
    background:rgba(0,0,0,0.28);
    padding:5px 12px; border-radius:999px;
    font-size:11px; letter-spacing:.03em;
    z-index:25;
  }

  /* --- balloons on the cover --- */
  .balloon{
    position:absolute;
    bottom:-18%;
    width:9%;
    aspect-ratio: 0.72;
    pointer-events:none;
    opacity:0;
    animation: balloonRise linear infinite;
    z-index:6;
    filter: drop-shadow(0 6px 10px rgba(0,0,0,0.15));
  }
  .balloon svg{ width:100%; height:100%; display:block; }
  @keyframes balloonRise{
    0%{ bottom:-18%; opacity:0; transform:translateX(0) rotate(-4deg); }
    8%{ opacity:0.95; }
    50%{ transform:translateX(16px) rotate(4deg); }
    92%{ opacity:0.95; }
    100%{ bottom:112%; opacity:0; transform:translateX(-12px) rotate(-3deg); }
  }

  /* --- floating music toggle --- */
  .music-btn{
    position:absolute;
    top:14px; right:14px;
    z-index:40;
    background:rgba(13,26,50,0.55);
    color:#fff;
    border:none;
    border-radius:999px;
    padding:8px 14px;
    font-size:12px;
    font-family:inherit;
    cursor:pointer;
    display:flex; align-items:center; gap:6px;
    box-shadow:0 4px 14px rgba(0,0,0,0.3);
    backdrop-filter: blur(4px);
  }
  .music-btn:active{ transform:scale(0.96); }

  .sound-toast{
    position:absolute;
    top:14px; right:14px;
    transform:translateY(52px);
    z-index:39;
    background:rgba(13,26,50,0.85);
    color:#fff;
    border-radius:10px;
    padding:6px 10px;
    font-size:10.5px;
    font-family:inherit;
    white-space:nowrap;
    box-shadow:0 4px 14px rgba(0,0,0,0.3);
    opacity:0;
    pointer-events:none;
    transition:opacity .4s ease;
  }
  .sound-toast.show{ opacity:1; }

  .player-dock{
    position:absolute;
    left:-9999px; top:0;
    width:1px; height:1px;
    opacity:0;
    pointer-events:none;
    overflow:hidden;
  }
  .player-dock iframe{
    position:absolute; inset:0; width:100%; height:100%; border:0;
  }

  /* --- slide 7 envelope --- */
  #s7 .bg{ position:absolute; inset:0; width:100%; height:100%; object-fit:cover; }
  #s7 .tap-hint{
    position:absolute; left:50%; bottom:8%; transform:translateX(-50%);
    color:#0d2b57; background:rgba(255,255,255,0.88);
    padding:9px 16px; border-radius:14px;
    font-size:clamp(11px,2.6vw,14px);
    box-shadow:0 6px 18px rgba(0,0,0,0.18);
    animation: pulseHint 1.6s ease-in-out infinite;
  }
  @keyframes pulseHint{ 0%,100%{ transform:translateX(-50%) scale(1);} 50%{ transform:translateX(-50%) scale(1.05);} }
  #s7 .env-tap{
    position:absolute; left:12%; right:12%; top:55%; bottom:14%;
    cursor:pointer;
    z-index:10;
  }
  #s7.open .bg{ animation: envOpen .6s ease both; }
  @keyframes envOpen{
    0%{ transform:scale(1); filter:brightness(1); }
    60%{ transform:scale(1.04); filter:brightness(1.15); }
    100%{ transform:scale(1.5); opacity:0; filter:brightness(1.3); }
  }

  /* --- slide 8 letter --- */
  #s8 .bg{ position:absolute; inset:0; width:100%; height:100%; object-fit:cover; }
  #s8.active .bg{ animation: letterIn .7s ease both; }
  @keyframes letterIn{
    from{ opacity:0; transform:translateY(8%) scale(0.98); }
    to{ opacity:1; transform:translateY(0) scale(1); }
  }

  canvas#confetti{
    position:absolute; inset:0; z-index:30; pointer-events:none;
  }

  .dots{
    position:absolute; top:14px; left:50%; transform:translateX(-50%);
    display:flex; gap:6px; z-index:25;
  }
  .dots span{
    width:6px; height:6px; border-radius:50%;
    background:rgba(255,255,255,0.35);
    transition: background .3s ease, transform .3s ease;
  }
  .dots span.on{ background:#fff; transform:scale(1.25); }
</style>
</head>
<body>

<div id="stage">

  <div class="dots" id="dots"></div>

  <button class="music-btn" id="musicBtn" aria-label="Toggle music">🔈 Music</button>
  <div class="sound-toast" id="soundToast">tap anywhere for sound 🔊</div>
  <div class="player-dock" id="playerDock">
    <iframe id="ytAudio"
      src="https://www.youtube.com/embed/5XDG75LPXqE?enablejsapi=1&autoplay=1&mute=1&loop=1&playlist=5XDG75LPXqE&controls=1&modestbranding=1&rel=0&playsinline=1"
      allow="autoplay; encrypted-media" frameborder="0" allowfullscreen></iframe>
  </div>

  <!-- SLIDE 1 : COVER -->
  <div class="slide active" id="s1">
    <img class="bg" src="data:image/jpeg;base64,__IMG1__" alt="Happy 16th Birthday Sofia Sy Su">
    <div class="balloon" style="left:8%;  width:8.5%;  animation-duration:11s; animation-delay:-2s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#2f6fd6"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#2f6fd6"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <div class="balloon" style="left:22%; width:7%;    animation-duration:9s;  animation-delay:-5s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#6fb1ff"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#6fb1ff"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <div class="balloon" style="left:38%; width:9.5%;  animation-duration:13s; animation-delay:-1s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#1f4fa8"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#1f4fa8"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <div class="balloon" style="left:55%; width:7.5%;  animation-duration:10s; animation-delay:-7s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#4a90e2"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#4a90e2"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <div class="balloon" style="left:68%; width:8%;    animation-duration:12s; animation-delay:-4s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#86c5ff"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#86c5ff"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <div class="balloon" style="left:80%; width:7%;    animation-duration:9.5s; animation-delay:-3s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#234a8a"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#234a8a"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <div class="balloon" style="left:14%; width:6.5%;  animation-duration:14s; animation-delay:-9s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#5aa0ff"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#5aa0ff"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <div class="balloon" style="left:90%; width:8.5%;  animation-duration:11.5s; animation-delay:-6s;"><svg viewBox="0 0 60 80"><ellipse cx="30" cy="30" rx="26" ry="30" fill="#3f7fe0"/><ellipse cx="21" cy="18" rx="7" ry="10" fill="rgba(255,255,255,0.35)"/><polygon points="26,58 34,58 30,66" fill="#3f7fe0"/><path d="M30 66 C 28 70, 33 74, 30 80" stroke="rgba(255,255,255,0.55)" stroke-width="1.4" fill="none"/></svg></div>
    <button class="nav-arrow show" onclick="goTo(2)" aria-label="Next">
      <svg viewBox="0 0 24 24" fill="none"><path d="M5 12h14M13 6l6 6-6 6" stroke="white" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </button>
  </div>

  <!-- SLIDE 2 : BLOW CANDLES -->
  <div class="slide" id="s2">
    <img class="bg" src="data:image/jpeg;base64,__IMG2__" alt="Blow your candles">
    <div class="hint">
      <div><b>Make a wish!</b> Hold the button (or hold the Space bar) for 3 seconds, or turn on your mic and blow 🎂</div>
      <div>
        <button class="blow-btn" id="holdBtn">🕯️ Hold to blow</button>
        <button class="mic-btn" id="micBtn">🎤 Use mic</button>
      </div>
      <div class="progress-wrap"><div class="progress-bar" id="blowProgress"></div></div>
      <span class="status" id="micStatus"></span>
    </div>
    <button class="nav-arrow" id="toS3" onclick="startSequence()" aria-label="Next">
      <svg viewBox="0 0 24 24" fill="none"><path d="M5 12h14M13 6l6 6-6 6" stroke="white" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </button>
  </div>

  <!-- SLIDE 3 : CAMERA -->
  <div class="slide" id="s3">
    <img class="bg" src="data:image/jpeg;base64,__IMG3__" alt="Camera">
    <div class="skip-hint">tap to skip ▸</div>
  </div>

  <!-- SLIDE 4 : CAMERA FLASH -->
  <div class="slide" id="s4">
    <img class="bg" src="data:image/jpeg;base64,__IMG4__" alt="Camera flash">
    <div class="whiteflash"></div>
    <div class="skip-hint">tap to skip ▸</div>
  </div>

  <!-- SLIDE 5 : PHOTOBOOTH STRIP -->
  <div class="slide" id="s5">
    <img class="bg" src="data:image/jpeg;base64,__IMG4__" alt="">
    <img class="strip" src="data:image/jpeg;base64,__IMG5__" alt="Photobooth strip">
    <div class="skip-hint">tap to skip ▸</div>
  </div>

  <!-- SLIDE 6 : POLAROID GRID -->
  <div class="slide" id="s6">
    <img class="bg" src="data:image/jpeg;base64,__IMG6__" alt="Photos">
    <div class="col colL" style="--fullbg:url('data:image/jpeg;base64,__IMG6__')"></div>
    <div class="col colM" style="--fullbg:url('data:image/jpeg;base64,__IMG6__')"></div>
    <div class="col colR" style="--fullbg:url('data:image/jpeg;base64,__IMG6__')"></div>
    <button class="nav-arrow" id="toS7" onclick="goTo(7)" aria-label="Next">
      <svg viewBox="0 0 24 24" fill="none"><path d="M5 12h14M13 6l6 6-6 6" stroke="white" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </button>
  </div>

  <!-- SLIDE 7 : ENVELOPE -->
  <div class="slide" id="s7">
    <img class="bg" src="data:image/jpeg;base64,__IMG7__" alt="Envelope with letter">
    <div class="env-tap" onclick="openEnvelope()"></div>
    <div class="tap-hint">💌 tap the envelope to open it</div>
  </div>

  <!-- SLIDE 8 : LETTER -->
  <div class="slide" id="s8">
    <img class="bg" src="data:image/jpeg;base64,__IMG8__" alt="Birthday letter">
    <button class="nav-arrow back-arrow show" onclick="restart()" aria-label="Back to start">
      <svg viewBox="0 0 24 24" fill="none"><path d="M19 12H5M11 6l-6 6 6 6" stroke="white" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </button>
  </div>

  <canvas id="confetti"></canvas>
</div>

<script>
(function(){
  var order = ['s1','s2','s3','s4','s5','s6','s7','s8'];
  var current = 1;
  var dotsEl = document.getElementById('dots');
  order.forEach(function(_,i){
    var d = document.createElement('span');
    if(i===0) d.className='on';
    dotsEl.appendChild(d);
  });

  function setDots(n){
    Array.prototype.forEach.call(dotsEl.children, function(el, i){
      el.classList.toggle('on', i === (n-1));
    });
  }

  window.goTo = function(n, cb){
    var cur = document.getElementById(order[current-1]);
    var next = document.getElementById(order[n-1]);
    if(cur) cur.classList.remove('active');
    next.classList.add('active');
    current = n;
    setDots(n);
    if(cb) setTimeout(cb, 50);
  };

  // ---------- background music (YouTube embed) ----------
  var yt = document.getElementById('ytAudio');
  var musicBtn = document.getElementById('musicBtn');
  var soundToast = document.getElementById('soundToast');
  var musicMuted = true;   // starts muted so autoplay is allowed by the browser
  var ytReady = false;
  function ytCmd(func, args){
    try{
      yt.contentWindow.postMessage(JSON.stringify({event:'command', func:func, args: args || []}), '*');
    }catch(e){}
  }
  yt.addEventListener('load', function(){
    ytReady = true;
    ytCmd('mute');
    ytCmd('playVideo');
    soundToast.classList.add('show');
    setTimeout(function(){ soundToast.classList.remove('show'); }, 5000);
  });

  function unmuteMusic(){
    ytCmd('unMute');
    ytCmd('playVideo');
    musicMuted = false;
    musicBtn.textContent = '🔊 Music';
  }
  function muteMusic(){
    ytCmd('mute');
    musicMuted = true;
    musicBtn.textContent = '🔈 Music';
  }

  musicBtn.addEventListener('click', function(e){
    e.stopPropagation();
    if(musicMuted){ unmuteMusic(); } else { muteMusic(); }
  });

  // browsers block audible autoplay, so unmute the moment the visitor
  // interacts with the page at all (tap/click/key) — this guarantees
  // the song actually starts playing very early, without them having
  // to find the music button themselves.
  function firstInteractionUnmute(){
    if(musicMuted){ unmuteMusic(); }
    soundToast.classList.remove('show');
    document.removeEventListener('pointerdown', firstInteractionUnmute, true);
    document.removeEventListener('keydown', firstInteractionUnmute, true);
  }
  document.addEventListener('pointerdown', firstInteractionUnmute, true);
  document.addEventListener('keydown', firstInteractionUnmute, true);

  // ---------- SLIDE 2 : blow candles ----------
  var blown = false;
  var s2 = document.getElementById('s2');
  var holdBtn = document.getElementById('holdBtn');
  var micBtn = document.getElementById('micBtn');
  var micStatus = document.getElementById('micStatus');
  var toS3 = document.getElementById('toS3');
  var progressBar = document.getElementById('blowProgress');

  // build the 9 flame-cover elements at the candle-flame positions
  var flameX = [25.6,31.8,38.3,44.6,52.0,58.7,64.9,71.2,78.3]; // % of width
  var flameY = 59.0; // % of height
  var flameCovers = flameX.map(function(x){
    var el = document.createElement('div');
    el.className = 'flame-cover';
    el.style.left = x + '%';
    el.style.top = flameY + '%';
    s2.appendChild(el);
    return el;
  });

  function spawnSmoke(x){
    var s = document.createElement('div');
    s.className = 'smoke';
    s.style.left = x + '%';
    s.style.top = flameY + '%';
    s2.appendChild(s);
    setTimeout(function(){ s.remove(); }, 1200);
  }

  function doBlow(){
    if(blown) return;
    blown = true;
    s2.classList.add('blown');
    flameCovers.forEach(function(el, i){
      setTimeout(function(){
        el.classList.add('show');
        spawnSmoke(flameX[i]);
      }, i*35);
    });
    burstConfetti();
    toS3.classList.add('show');
    micStatus.textContent = '🎉 candles blown out — make your wish!';
    progressBar.style.width = '100%';
  }

  // hold-to-blow: 3 second hold, either the on-screen button or the Space bar
  var HOLD_MS = 3000;
  var holding = false;
  var holdRAF = null;
  var holdStart = null;

  function startHold(){
    if(blown || holding) return;
    holding = true;
    holdBtn.style.transform = 'scale(0.96)';
    holdStart = performance.now();
    function step(ts){
      if(!holding) return;
      var elapsed = ts - holdStart;
      var pct = Math.min(100, (elapsed / HOLD_MS) * 100);
      progressBar.style.width = pct + '%';
      if(elapsed >= HOLD_MS){
        holding = false;
        doBlow();
        return;
      }
      holdRAF = requestAnimationFrame(step);
    }
    holdRAF = requestAnimationFrame(step);
  }
  function cancelHold(){
    holdBtn.style.transform = 'scale(1)';
    if(!holding) return;
    holding = false;
    if(holdRAF) cancelAnimationFrame(holdRAF);
    if(!blown) progressBar.style.width = '0%';
  }
  holdBtn.addEventListener('pointerdown', startHold);
  holdBtn.addEventListener('pointerup', cancelHold);
  holdBtn.addEventListener('pointerleave', cancelHold);
  holdBtn.addEventListener('pointercancel', cancelHold);

  window.addEventListener('keydown', function(e){
    if((e.code === 'Space' || e.key === ' ') && current === 2 && !blown){
      e.preventDefault();
      startHold();
    }
  });
  window.addEventListener('keyup', function(e){
    if(e.code === 'Space' || e.key === ' '){
      cancelHold();
    }
  });

  // mic blow detection
  var audioCtx = null, analyser = null, micStream = null, rafId = null;
  micBtn.addEventListener('click', function(){
    if(blown) return;
    if(audioCtx){
      micStatus.textContent = 'listening… blow into your mic 💨';
      return;
    }
    micStatus.textContent = 'requesting microphone…';
    navigator.mediaDevices.getUserMedia({ audio: true }).then(function(stream){
      micStream = stream;
      audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      var src = audioCtx.createMediaStreamSource(stream);
      analyser = audioCtx.createAnalyser();
      analyser.fftSize = 512;
      src.connect(analyser);
      var data = new Uint8Array(analyser.frequencyBinCount);
      var loudFrames = 0;
      micStatus.textContent = 'listening… blow into your mic 💨';
      function poll(){
        if(blown) return;
        analyser.getByteFrequencyData(data);
        var sum = 0;
        for(var i=0;i<data.length;i++) sum += data[i];
        var avg = sum / data.length;
        if(avg > 28){ loudFrames++; } else { loudFrames = Math.max(0, loudFrames-1); }
        if(loudFrames > 6){ doBlow(); return; }
        rafId = requestAnimationFrame(poll);
      }
      poll();
    }).catch(function(){
      micStatus.textContent = 'mic unavailable — try “Hold to blow” instead';
    });
  });

  // ---------- confetti ----------
  var canvas = document.getElementById('confetti');
  var ctx = canvas.getContext('2d');
  var stage = document.getElementById('stage');
  function fit(){
    canvas.width = stage.clientWidth;
    canvas.height = stage.clientHeight;
  }
  fit();
  window.addEventListener('resize', fit);

  var particles = [];
  var colors = ['#ff6b8a','#ffd166','#4cc9f0','#8ac926','#c084fc','#ff9f1c','#ffffff'];
  function burstConfetti(){
    for(var i=0;i<140;i++){
      particles.push({
        x: Math.random()*canvas.width,
        y: -20 - Math.random()*canvas.height*0.4,
        vx: (Math.random()-0.5)*2.2,
        vy: 2 + Math.random()*3.2,
        size: 5 + Math.random()*6,
        color: colors[Math.floor(Math.random()*colors.length)],
        rot: Math.random()*Math.PI*2,
        vr: (Math.random()-0.5)*0.3,
        life: 0
      });
    }
    if(!confettiRunning){ confettiRunning = true; requestAnimationFrame(runConfetti); }
  }
  var confettiRunning = false;
  function runConfetti(){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    var alive = false;
    for(var i=0;i<particles.length;i++){
      var p = particles[i];
      if(!p) continue;
      p.life++;
      p.x += p.vx;
      p.y += p.vy;
      p.vy += 0.02;
      p.rot += p.vr;
      if(p.y < canvas.height + 20 && p.life < 480){
        alive = true;
        ctx.save();
        ctx.translate(p.x, p.y);
        ctx.rotate(p.rot);
        ctx.fillStyle = p.color;
        ctx.fillRect(-p.size/2, -p.size/3, p.size, p.size*0.6);
        ctx.restore();
      }
    }
    if(alive){
      requestAnimationFrame(runConfetti);
    } else {
      particles = [];
      confettiRunning = false;
      ctx.clearRect(0,0,canvas.width,canvas.height);
    }
  }

  // ---------- auto sequence slides 3-6 ----------
  window.startSequence = function(){
    stopMic();
    playSeq();
  };
  var seqTimers = [];
  function clearSeq(){ seqTimers.forEach(clearTimeout); seqTimers = []; }
  function playSeq(){
    clearSeq();
    goTo(3);
    seqTimers.push(setTimeout(function(){ goTo(4); }, 1100));
    seqTimers.push(setTimeout(function(){ goTo(5); }, 1100 + 900));
    seqTimers.push(setTimeout(function(){ goTo(6, function(){
      document.getElementById('toS7').classList.add('show');
    }); }, 1100 + 900 + 1500));
  }
  // allow tapping slides 3-5 to skip ahead quickly
  [3,4,5].forEach(function(n){
    document.getElementById('s'+n).addEventListener('click', function(){
      clearSeq();
      if(n < 6){ goTo(n+1 === 6 ? 6 : n+1); }
      if(n+1 === 6){ document.getElementById('toS7').classList.add('show'); }
      else { playSeqFrom(n+1); }
    });
  });
  function playSeqFrom(n){
    clearSeq();
    var delays = {3:1100, 4:900, 5:1500};
    var idxs = [];
    for(var s=n+1; s<=6; s++) idxs.push(s);
    var wait = 0;
    idxs.forEach(function(step){
      wait += delays[step-1] || 1000;
      seqTimers.push(setTimeout(function(){
        goTo(step, step===6 ? function(){ document.getElementById('toS7').classList.add('show'); } : null);
      }, wait));
    });
  }

  function stopMic(){
    if(rafId) cancelAnimationFrame(rafId);
    if(micStream){ micStream.getTracks().forEach(function(t){ t.stop(); }); }
    if(audioCtx){ try{ audioCtx.close(); }catch(e){} }
    audioCtx = null;
  }

  // ---------- envelope open ----------
  window.openEnvelope = function(){
    var s7 = document.getElementById('s7');
    if(s7.classList.contains('open')) return;
    s7.classList.add('open');
    setTimeout(function(){ goTo(8); }, 520);
  };

  // ---------- restart ----------
  window.restart = function(){
    document.getElementById('s7').classList.remove('open');
    document.getElementById('s2').classList.remove('blown');
    document.getElementById('toS3').classList.remove('show');
    document.getElementById('toS7').classList.remove('show');
    blown = false;
    micStatus.textContent = '';
    stopMic();
    goTo(1);
  };
})();
</script>
</body>
</html>
