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
