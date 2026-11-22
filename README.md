<!doctype html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>عيد ميلاد سعيد 🎂</title>
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@500;800&family=Pacifico&display=swap" rel="stylesheet">
<style>
  :root{--bg1:#fff1f5;--bg2:#e8f0ff;--ink:#5a1f3d}
  *{box-sizing:border-box}
  body{margin:0;min-height:100vh;display:flex;flex-direction:column;align-items:center;
    background:linear-gradient(180deg,var(--bg1),var(--bg2));font-family:'Tajawal',system-ui,sans-serif;color:var(--ink);
    overflow-x:hidden;padding:12px 16px 32px}
  #tree{width:min(92vw,440px);height:48vh;max-height:420px;display:block}
  #final{display:flex;flex-direction:column;align-items:center;opacity:0;transform:translateY(24px);
    transition:opacity 1.2s ease,transform 1.2s ease;text-align:center}
  #final.show{opacity:1;transform:none}
  svg.cake{width:min(70vw,260px);height:auto}
  .flame{transform-box:fill-box;transform-origin:50% 100%;animation:flick .5s infinite alternate ease-in-out}
  .flame:nth-of-type(2){animation-delay:.15s}.flame:nth-of-type(3){animation-delay:.3s}
  @keyframes flick{from{transform:scale(1,1) rotate(-4deg)}to{transform:scale(.85,1.15) rotate(4deg)}}
  h1{font-size:clamp(1.6rem,6vw,2.2rem);font-weight:800;margin:.6rem 0 .2rem}
  p.wish{font-size:1.2rem;margin:.2rem 0 1rem;font-weight:500}
  .miss{position:relative;display:inline-block;font-size:1.9rem;font-weight:800;padding-bottom:.3rem;color:#d6336c}
  .miss::after{content:"";position:absolute;right:0;bottom:0;height:4px;width:0;border-radius:4px;
    background:linear-gradient(90deg,#e03131,#1c7ed6)}
  #final.show .miss::after{width:100%;transition:width 1.4s ease .9s}
  button{margin-top:1.6rem;padding:.5rem 1.2rem;border:0;border-radius:10px;background:var(--ink);color:#fff;
    font-family:inherit;font-size:1rem;cursor:pointer;opacity:.85}
</style>
</head>
<body>
<canvas id="tree"></canvas>

<div id="final">
  <svg class="cake" viewBox="0 0 240 210" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <clipPath id="bot"><rect x="30" y="130" width="180" height="55" rx="10"/></clipPath>
      <clipPath id="top"><rect x="55" y="92" width="130" height="42" rx="10"/></clipPath>
    </defs>
    <ellipse cx="120" cy="190" rx="112" ry="12" fill="#d0d4dc"/>
    <!-- الطبقة السفلية: نص أحمر ونص أزرق -->
    <g clip-path="url(#bot)">
      <rect x="30" y="130" width="90" height="55" fill="#e03131"/>
      <rect x="120" y="130" width="90" height="55" fill="#1c7ed6"/>
    </g>
    <!-- الطبقة العلوية -->
    <g clip-path="url(#top)">
      <rect x="55" y="92" width="65" height="42" fill="#c92a2a"/>
      <rect x="120" y="92" width="65" height="42" fill="#1864ab"/>
    </g>
    <!-- الكريمة -->
    <path d="M55 100 q10 14 20 0 t20 0 t20 0 t20 0 t20 0 t20 0 t10 0 v-8 h-130z" fill="#fff"/>
    <path d="M30 138 q11 15 22 0 t22 0 t22 0 t22 0 t22 0 t22 0 t22 0 t22 0 v-8 h-180z" fill="#fff"/>
    <!-- الاسم بالكريمة -->
    <text x="120" y="172" text-anchor="middle" direction="ltr"
          font-family="'Pacifico',cursive" font-size="27" fill="#fff"
          stroke="#ffd6e0" stroke-width="1.5" paint-order="stroke"
          style="filter:drop-shadow(0 1.5px 1px rgba(0,0,0,.25))">Ikhlas</text>
    <!-- الشموع -->
    <rect x="90" y="62" width="8" height="30" rx="2" fill="#e03131"/>
    <rect x="116" y="58" width="8" height="34" rx="2" fill="#fff" stroke="#ccc"/>
    <rect x="142" y="62" width="8" height="30" rx="2" fill="#1c7ed6"/>
    <path class="flame" d="M94 36c-7 8-5 18 0 22 5-4 7-14 0-22z" fill="#ffa94d"/>
    <path class="flame" d="M120 32c-7 8-5 18 0 22 5-4 7-14 0-22z" fill="#ffa94d"/>
    <path class="flame" d="M146 36c-7 8-5 18 0 22 5-4 7-14 0-22z" fill="#ffa94d"/>
  </svg>

  <h1 id="title">عيد ميلاد سعيد 🎂</h1>
  <p class="wish">كل عام وإنتي بخير، وأيامك كلها فرح 🌸</p>
  <span class="miss">ما ناسيك</span>
  <button onclick="start()">إعادة ↻</button>
</div>

<script>
const cv = document.getElementById('tree'), ctx = cv.getContext('2d');
const final = document.getElementById('final');
const MAX = 9, STEP = 380;
let branches = [], leaves = [], t0 = 0, shown = false, raf;

function size(){
  const d = window.devicePixelRatio || 1, r = cv.getBoundingClientRect();
  cv.width = r.width * d; cv.height = r.height * d;
  ctx.setTransform(d,0,0,d,0,0);
  return r;
}

function build(x, y, len, ang, depth, w){
  if (depth > MAX) return;
  const x2 = x + Math.sin(ang)*len, y2 = y - Math.cos(ang)*len;
  branches.push({x, y, x2, y2, depth, w});
  if (depth === MAX) leaves.push({x:x2, y:y2});
  const k = 0.78;
  build(x2, y2, len*k, ang - 0.38 - Math.random()*0.2, depth+1, w*0.72);
  build(x2, y2, len*k, ang + 0.38 + Math.random()*0.2, depth+1, w*0.72);
}

function start(){
  cancelAnimationFrame(raf);
  final.classList.remove('show'); shown = false;
  const r = size();
  branches = []; leaves = [];
  build(r.width/2, r.height - 6, r.height*0.2, 0, 0, r.width*0.03);
  leaves.forEach(l => { l.r = 3 + Math.random()*4; l.c = ['#f783ac','#faa2c1','#ffd6e0','#8ce99a'][Math.floor(Math.random()*4)]; });
  t0 = performance.now();
  raf = requestAnimationFrame(frame);
}

function frame(now){
  const t = now - t0, r = cv.getBoundingClientRect();
  ctx.clearRect(0, 0, r.width, r.height);
  ctx.lineCap = 'round'; ctx.strokeStyle = '#6b4226';
  for (const b of branches){
    const p = Math.max(0, Math.min(1, (t - b.depth*STEP)/STEP));
    if (p <= 0) continue;
    ctx.lineWidth = Math.max(1, b.w);
    ctx.beginPath(); ctx.moveTo(b.x, b.y);
    ctx.lineTo(b.x + (b.x2-b.x)*p, b.y + (b.y2-b.y)*p); ctx.stroke();
  }
  const lt = t - (MAX+1)*STEP;
  if (lt > 0){
    for (const l of leaves){
      const s = Math.min(1, lt/900);
      ctx.fillStyle = l.c; ctx.beginPath(); ctx.arc(l.x, l.y, l.r*s*1.6, 0, 7); ctx.fill();
    }
  }
  if (lt > 1400 && !shown){ shown = true; final.classList.add('show'); }
  if (!shown || lt < 2500) raf = requestAnimationFrame(frame);
}

addEventListener('resize', start);
start();
</script>
</body>
</html>
