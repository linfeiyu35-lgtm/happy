<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>戳戳消气怪</title>
  <style>
    :root{
      --bg1:#fff1f6;
      --bg2:#f3f7ff;
      --card:#ffffffcc;
      --text:#222;
      --muted:#666;
      --accent:#ff4d8d;
      --accent2:#6c7cff;
      --shadow: 0 12px 30px rgba(0,0,0,.10);
      --radius: 18px;
    }
    *{box-sizing:border-box; -webkit-tap-highlight-color: transparent;}
    html,body{height:100%; margin:0; font-family: ui-sans-serif, system-ui, -apple-system, "PingFang SC", "Microsoft YaHei", Arial;}
    body{
      color:var(--text);
      background: radial-gradient(1200px 700px at 20% 10%, var(--bg1), transparent 60%),
                  radial-gradient(1200px 700px at 80% 0%, var(--bg2), transparent 60%),
                  linear-gradient(180deg, #fff, #f7fbff);
      overflow:hidden;
    }
    .wrap{
      height:100%;
      display:flex;
      flex-direction:column;
      padding: env(safe-area-inset-top) 14px env(safe-area-inset-bottom);
      gap:10px;
      max-width:520px;
      margin:0 auto;
    }
    header{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
      padding:12px 14px;
      border-radius: var(--radius);
      background: var(--card);
      box-shadow: var(--shadow);
      backdrop-filter: blur(10px);
    }
    .title{
      display:flex; flex-direction:column; gap:2px;
    }
    .title h1{
      font-size:16px; margin:0; font-weight:800;
    }
    .title p{
      font-size:12px; margin:0; color:var(--muted);
    }
    .hud{
      display:flex; gap:10px; align-items:center;
      font-variant-numeric: tabular-nums;
    }
    .pill{
      padding:8px 10px;
      border-radius: 999px;
      background:#fff;
      box-shadow: 0 6px 16px rgba(0,0,0,.08);
      font-size:12px;
      display:flex;
      gap:6px;
      align-items:center;
      white-space:nowrap;
    }
    .pill b{font-size:13px;}
    .btn{
      border:none;
      background: linear-gradient(135deg, var(--accent), #ff7ab3);
      color:#fff;
      padding:10px 12px;
      border-radius: 12px;
      font-weight:800;
      box-shadow: 0 12px 24px rgba(255,77,141,.25);
      cursor:pointer;
    }
    .btn.secondary{
      background: linear-gradient(135deg, var(--accent2), #9aa5ff);
      box-shadow: 0 12px 24px rgba(108,124,255,.22);
    }
    main{
      position:relative;
      flex:1;
      border-radius: var(--radius);
      background: #ffffffaa;
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .hint{
      position:absolute;
      left:12px; right:12px; top:12px;
      background:#fff;
      border-radius: 14px;
      padding:10px 12px;
      box-shadow: 0 10px 24px rgba(0,0,0,.08);
      display:flex; align-items:center; justify-content:space-between;
      gap:10px;
      z-index:5;
    }
    .hint .small{font-size:12px; color:var(--muted); line-height:1.2;}
    .hint .small b{color:var(--text);}
    canvas{width:100%; height:100%; display:block;}
    .overlay{
      position:absolute; inset:0;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:18px;
      z-index:10;
      background: linear-gradient(180deg, rgba(255,255,255,.75), rgba(255,255,255,.92));
      backdrop-filter: blur(10px);
    }
    .card{
      width:100%;
      border-radius: 22px;
      background:#fff;
      box-shadow: var(--shadow);
      padding:16px;
      max-width:520px;
    }
    .card h2{margin:0 0 6px; font-size:18px;}
    .card p{margin:8px 0; font-size:13px; color:#333; line-height:1.6;}
    .row{display:flex; gap:10px; flex-wrap:wrap; margin-top:12px;}
    .row .btn{flex:1; min-width:130px;}
    .badge{
      display:inline-flex; align-items:center; gap:6px;
      padding:6px 10px; border-radius:999px;
      background:#fff3f8; color:#b10044; font-weight:700;
      font-size:12px;
      border:1px solid rgba(255,77,141,.25);
      margin-bottom:10px;
    }
    .toast{
      position:absolute;
      left:50%; bottom:16px;
      transform: translateX(-50%);
      background:#111;
      color:#fff;
      padding:10px 12px;
      border-radius: 999px;
      font-size:12px;
      opacity:0;
      transition:.25s ease;
      z-index:20;
      pointer-events:none;
      box-shadow: 0 12px 30px rgba(0,0,0,.25);
    }
    .toast.show{opacity:1; transform:translateX(-50%) translateY(-2px);}
    .footerNote{
      font-size:11px;
      color:var(--muted);
      text-align:center;
      padding-bottom:2px;
    }
  </style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="title">
      <h1 id="gameTitle">戳戳消气怪</h1>
      <p id="subtitle">把小乌云😤戳成笑脸😊，收集爱心💗</p>
    </div>
    <div class="hud">
      <div class="pill">⏳ <b id="time">30.0</b>s</div>
      <div class="pill">💗 <b id="score">0</b></div>
      <button class="btn secondary" id="pauseBtn" aria-label="暂停">暂停</button>
    </div>
  </header>

  <main id="stage">
    <div class="hint" id="hint">
      <div class="small">
        <b>玩法：</b>点/戳“生气云😤”就会变好心情😊并掉落💗。<br/>
        <b>目标：</b><span id="targetText">收集 80💗</span> 解锁惊喜～
      </div>
      <button class="btn" id="startBtn">开始哄哄</button>
    </div>

    <canvas id="c"></canvas>

    <div class="overlay" id="overlay" style="display:none;">
      <div class="card">
        <div class="badge" id="badge">🎁 解锁惊喜</div>
        <h2 id="overlayTitle">你把坏情绪打跑啦！</h2>
        <p id="letter"></p>
        <div class="row">
          <button class="btn" id="playAgain">再玩一次</button>
          <button class="btn secondary" id="copyBtn">复制哄哄话</button>
        </div>
      </div>
    </div>

    <div class="toast" id="toast">已复制 ✅</div>
  </main>

  <div class="footerNote">提示：全程触屏友好。可以把这页发给她打开就能玩。</div>
</div>

<script>
/** =========================
 *  可自定义文案（改这里就行）
 *  ========================= */
const gfName = "宝宝";        // 改成她的昵称
const myName = "我";          // 改成你的称呼/名字
const sorryReason = "刚刚让你不开心了"; // 改成原因（可留空）

const targetScore = 80; // 达标分数：到这个分数就解锁“哄哄信”
const gameSeconds = 30; // 游戏时长

const cheerLines = [
  `今天的任务：让${gfName}开心 +100！`,
  `我认真反省：${sorryReason || "我不该惹你生气"}。`,
  `你生气也很可爱，但我更想看到你笑～`,
  `如果可以，我想把你的不开心都打包带走。`,
  `给你一个抱抱🤍 再加一个亲亲😘`,
  `现在请允许我：正式申请你的原谅 ✅`
];

/** =========================
 *  简易小游戏：戳生气云 + 掉落爱心
 *  ========================= */
const stage = document.getElementById("stage");
const canvas = document.getElementById("c");
const ctx = canvas.getContext("2d", { alpha: true });

const hint = document.getElementById("hint");
const startBtn = document.getElementById("startBtn");
const pauseBtn = document.getElementById("pauseBtn");

const timeEl = document.getElementById("time");
const scoreEl = document.getElementById("score");
const targetText = document.getElementById("targetText");

const overlay = document.getElementById("overlay");
const overlayTitle = document.getElementById("overlayTitle");
const letterEl = document.getElementById("letter");
const playAgainBtn = document.getElementById("playAgain");
const copyBtn = document.getElementById("copyBtn");
const toast = document.getElementById("toast");

targetText.textContent = `收集 ${targetScore}💗`;

function resize(){
  const r = stage.getBoundingClientRect();
  const dpr = Math.max(1, Math.min(2, window.devicePixelRatio || 1));
  canvas.width = Math.floor(r.width * dpr);
  canvas.height = Math.floor(r.height * dpr);
  canvas.style.width = r.width + "px";
  canvas.style.height = r.height + "px";
  ctx.setTransform(dpr,0,0,dpr,0,0);
}
window.addEventListener("resize", resize);
resize();

let running = false;
let paused = false;
let tLeft = gameSeconds;
let score = 0;

const clouds = [];
const hearts = [];
const pops = [];

function rand(a,b){ return a + Math.random()*(b-a); }

function spawnCloud(){
  const w = canvas.clientWidth || stage.clientWidth;
  const h = canvas.clientHeight || stage.clientHeight;
  const size = rand(44, 70);
  clouds.push({
    x: rand(20, w-20),
    y: rand(90, h-40),
    r: size,
    mood: "angry", // angry -> happy
    vx: rand(-18,18),
    vy: rand(-10,10),
    born: performance.now(),
    life: rand(2200, 3200),
    hit: false
  });
}

function spawnHeart(x,y){
  hearts.push({
    x, y,
    vy: rand(-80, -120),
    vx: rand(-20, 20),
    a: 1,
    s: rand(0.9, 1.25),
    rot: rand(-0.8, 0.8)
  });
}

function spawnPop(x,y,txt){
  pops.push({
    x,y, txt,
    vy: -40,
    a: 1
  });
}

function drawCloud(c){
  // body
  const face = c.mood === "angry" ? "😤" : "😊";
  ctx.save();
  ctx.translate(c.x, c.y);
  ctx.globalAlpha = 0.98;
  // soft blob
  ctx.beginPath();
  ctx.fillStyle = c.mood === "angry" ? "rgba(60,72,90,.22)" : "rgba(255,77,141,.14)";
  ctx.arc(0, 0, c.r*0.62, 0, Math.PI*2);
  ctx.arc(-c.r*0.45, c.r*0.05, c.r*0.45, 0, Math.PI*2);
  ctx.arc(c.r*0.45, c.r*0.08, c.r*0.50, 0, Math.PI*2);
  ctx.arc(0, c.r*0.22, c.r*0.55, 0, Math.PI*2);
  ctx.fill();

  // outline
  ctx.lineWidth = 2;
  ctx.strokeStyle = c.mood === "angry" ? "rgba(60,72,90,.25)" : "rgba(255,77,141,.22)";
  ctx.stroke();

  // emoji face
  ctx.font = `${Math.floor(c.r*0.9)}px system-ui, Apple Color Emoji, Segoe UI Emoji`;
  ctx.textAlign = "center";
  ctx.textBaseline = "middle";
  ctx.fillText(face, 0, -2);

  ctx.restore();
}

function drawHeart(h){
  ctx.save();
  ctx.translate(h.x, h.y);
  ctx.rotate(h.rot);
  ctx.globalAlpha = Math.max(0, h.a);
  ctx.font = `${Math.floor(22*h.s)}px system-ui, Apple Color Emoji, Segoe UI Emoji`;
  ctx.textAlign = "center";
  ctx.textBaseline = "middle";
  ctx.fillText("💗", 0, 0);
  ctx.restore();
}

function drawPop(p){
  ctx.save();
  ctx.globalAlpha = Math.max(0, p.a);
  ctx.font = "700 14px system-ui";
  ctx.fillStyle = "rgba(0,0,0,.75)";
  ctx.textAlign = "center";
  ctx.fillText(p.txt, p.x, p.y);
  ctx.restore();
}

let last = performance.now();
let spawnAcc = 0;

function step(now){
  const dt = Math.min(0.033, (now - last)/1000);
  last = now;

  const w = canvas.clientWidth || stage.clientWidth;
  const h = canvas.clientHeight || stage.clientHeight;

  if (running && !paused){
    tLeft -= dt;
    if (tLeft <= 0){
      tLeft = 0;
      endGame();
    }
    timeEl.textContent = tLeft.toFixed(1);

    spawnAcc += dt;
    if (spawnAcc > 0.55){
      spawnAcc = 0;
      if (clouds.length < 7) spawnCloud();
    }

    // update clouds
    for (let i=clouds.length-1; i>=0; i--){
      const c = clouds[i];
      c.x += c.vx * dt;
      c.y += c.vy * dt;
      // bounce
      if (c.x < 26 || c.x > w-26) c.vx *= -1;
      if (c.y < 92 || c.y > h-26) c.vy *= -1;

      if (now - c.born > c.life){
        clouds.splice(i,1);
      }
    }

    // update hearts
    for (let i=hearts.length-1; i>=0; i--){
      const hh = hearts[i];
      hh.x += hh.vx * dt;
      hh.y += hh.vy * dt;
      hh.vy += 180 * dt; // gravity
      hh.a -= 0.55 * dt;
      if (hh.a <= 0 || hh.y > h+30) hearts.splice(i,1);
    }

    // update pops
    for (let i=pops.length-1; i>=0; i--){
      const p = pops[i];
      p.y += p.vy * dt;
      p.a -= 1.4 * dt;
      if (p.a <= 0) pops.splice(i,1);
    }
  }

  // draw
  ctx.clearRect(0,0,w,h);

  // soft dots background
  ctx.save();
  ctx.globalAlpha = 0.35;
  for(let i=0;i<24;i++){
    ctx.beginPath();
    const x = (i*37 + (now/40)%37) % (w+40) - 20;
    const y = 80 + (i*53) % (h-80);
    ctx.fillStyle = i%2 ? "rgba(255,77,141,.15)" : "rgba(108,124,255,.12)";
    ctx.arc(x, y, 4 + (i%3), 0, Math.PI*2);
    ctx.fill();
  }
  ctx.restore();

  clouds.forEach(drawCloud);
  hearts.forEach(drawHeart);
  pops.forEach(drawPop);

  requestAnimationFrame(step);
}
requestAnimationFrame(step);

function setScore(v){
  score = v;
  scoreEl.textContent = score;
  if (score >= targetScore && running){
    // 提前解锁：让她更快看到惊喜
    endGame(true);
  }
}

function startGame(){
  running = true;
  paused = false;
  tLeft = gameSeconds;
  setScore(0);
  clouds.length = 0;
  hearts.length = 0;
  pops.length = 0;
  hint.style.display = "none";
  overlay.style.display = "none";
  pauseBtn.textContent = "暂停";
  // 初始生成几只
  for(let i=0;i<3;i++) spawnCloud();
  timeEl.textContent = tLeft.toFixed(1);
}

function endGame(earlyWin=false){
  running = false;
  paused = false;

  const win = score >= targetScore || earlyWin;
  overlayTitle.textContent = win ? `好耶！${gfName}的坏心情被我戳跑啦～` : `差一点点！再给我一次机会🥺`;
  const letter =
`${gfName}：

${cheerLines.join("\n")}

—— ${myName}`;
  letterEl.textContent = letter;

  overlay.style.display = "flex";
  hint.style.display = "flex";
  startBtn.textContent = "再开始一次";
}

function toastMsg(text){
  toast.textContent = text;
  toast.classList.add("show");
  clearTimeout(toast._t);
  toast._t = setTimeout(()=>toast.classList.remove("show"), 900);
}

function togglePause(){
  if (!running) return;
  paused = !paused;
  pauseBtn.textContent = paused ? "继续" : "暂停";
  toastMsg(paused ? "已暂停" : "继续哄哄！");
}

pauseBtn.addEventListener("click", togglePause);
startBtn.addEventListener("click", startGame);
playAgainBtn.addEventListener("click", startGame);

copyBtn.addEventListener("click", async ()=>{
  try{
    await navigator.clipboard.writeText(letterEl.textContent);
    toastMsg("已复制 ✅");
  }catch(e){
    toastMsg("复制失败（可手动选中复制）");
  }
});

// touch / click to hit clouds
function getPos(e){
  const rect = canvas.getBoundingClientRect();
  const p = (e.touches && e.touches[0]) ? e.touches[0] : e;
  return { x: p.clientX - rect.left, y: p.clientY - rect.top };
}

function hitTest(x,y){
  // 从上往下找：更像“点到最上面的那个”
  for (let i=clouds.length-1; i>=0; i--){
    const c = clouds[i];
    const dx = x - c.x, dy = y - c.y;
    if (dx*dx + dy*dy < (c.r*0.72)*(c.r*0.72)){
      if (c.mood === "angry"){
        c.mood = "happy";
        spawnPop(c.x, c.y - c.r*0.8, "+💗");
        // 掉落几颗心
        const n = 2 + Math.floor(Math.random()*3);
        for(let k=0;k<n;k++) spawnHeart(c.x + rand(-10,10), c.y + rand(-6,10));
        setScore(score + 6 + Math.floor(Math.random()*4));
      }else{
        spawnPop(c.x, c.y - c.r*0.8, "😊");
        setScore(score + 1);
      }
      return true;
    }
  }
  return false;
}

canvas.addEventListener("pointerdown", (e)=>{
  if (!running || paused) return;
  const {x,y} = getPos(e);
  const ok = hitTest(x,y);
  if (!ok){
    spawnPop(x,y,"再戳准一点点～");
  }
});

canvas.addEventListener("touchstart", (e)=>{ e.preventDefault(); }, {passive:false});

// initial overlay text
document.getElementById("gameTitle").textContent = `戳戳消气怪 · 哄哄${gfName}`;
document.getElementById("subtitle").textContent = `把小乌云😤戳成笑脸😊，收集爱心💗`;
</script>
</body>
</html>

