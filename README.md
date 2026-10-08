<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Пульс — социальная сеть</title>
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  :root{
    --bg:#eef1f6;
    --card:#ffffff;
    --card-2:#f7f8fb;
    --text:#12141d;
    --muted:#7a8194;
    --border:#e3e7ef;
    --accent:#6c5ce7;
    --accent-soft:#efedff;
    --accent-2:#a29bfe;
    --like:#ff4d6d;
    --shadow:0 1px 2px rgba(18,20,29,.06),0 8px 24px rgba(18,20,29,.06);
    --shadow-sm:0 1px 2px rgba(18,20,29,.05),0 3px 10px rgba(18,20,29,.05);
    --radius:16px;
  }
  [data-theme="dark"]{
    --bg:#0e1017;
    --card:#171a24;
    --card-2:#1e222e;
    --text:#eef1f8;
    --muted:#8c93a8;
    --border:#272b38;
    --accent:#8b7cff;
    --accent-soft:#231f45;
    --shadow:0 1px 2px rgba(0,0,0,.3),0 8px 24px rgba(0,0,0,.28);
    --shadow-sm:0 1px 2px rgba(0,0,0,.25),0 3px 10px rgba(0,0,0,.2);
  }
  html,body{height:100%}
  body{
    font-family:'Inter',-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,'Helvetica Neue',Arial,sans-serif;
    background:var(--bg);color:var(--text);
    -webkit-font-smoothing:antialiased;
    transition:background .25s ease,color .25s ease;
    padding-bottom:80px;
  }
  button{font-family:inherit;cursor:pointer;border:none;background:none;color:inherit}
  input,textarea{font-family:inherit;color:inherit}
  svg{display:block;flex-shrink:0}
  ::-webkit-scrollbar{width:8px;height:8px}
  ::-webkit-scrollbar-thumb{background:var(--border);border-radius:10px}
  ::-webkit-scrollbar-thumb:hover{background:var(--muted)}

  /* ── TOPBAR ── */
  .topbar{
    position:sticky;top:0;z-index:60;
    background:color-mix(in srgb,var(--card) 88%,transparent);
    backdrop-filter:saturate(180%) blur(16px);
    -webkit-backdrop-filter:saturate(180%) blur(16px);
    border-bottom:1px solid var(--border);
  }
  .topbar-inner{
    max-width:1240px;margin:0 auto;padding:0 20px;height:64px;
    display:flex;align-items:center;gap:16px;
  }
  .logo{display:flex;align-items:center;gap:10px;font-weight:800;font-size:19px;letter-spacing:-.4px}
  .logo-mark{
    width:34px;height:34px;border-radius:11px;
    background:linear-gradient(135deg,#6c5ce7,#a29bfe 55%,#fd79a8);
    display:grid;place-items:center;color:#fff;font-size:17px;font-weight:800;
    box-shadow:0 4px 14px rgba(108,92,231,.42);
  }
  .logo span{background:linear-gradient(90deg,var(--accent),#fd79a8);-webkit-background-clip:text;background-clip:text;color:transparent}
  .search{
    flex:1;max-width:420px;position:relative;
  }
  .search input{
    width:100%;height:42px;border-radius:12px;border:1px solid var(--border);
    background:var(--card-2);padding:0 14px 0 42px;font-size:14px;outline:none;
    transition:border-color .2s,box-shadow .2s,background .2s;
  }
  .search input:focus{border-color:var(--accent);box-shadow:0 0 0 3px var(--accent-soft);background:var(--card)}
  .search svg{position:absolute;left:13px;top:50%;transform:translateY(-50%);color:var(--muted)}
  .topbar-actions{margin-left:auto;display:flex;align-items:center;gap:6px}
  .icon-btn{
    width:42px;height:42px;border-radius:12px;display:grid;place-items:center;
    color:var(--muted);position:relative;transition:.18s;
  }
  .icon-btn:hover{background:var(--card-2);color:var(--accent)}
  .icon-btn.active{color:var(--accent);background:var(--accent-soft)}
  .badge{
    position:absolute;top:6px;right:6px;min-width:17px;height:17px;padding:0 4px;
    border-radius:9px;background:linear-gradient(135deg,#ff4d6d,#ff7a8f);color:#fff;
    font-size:10px;font-weight:700;display:grid;place-items:center;border:2px solid var(--card);
  }
  .avatar-btn{width:40px;height:40px;border-radius:50%;padding:2px;background:linear-gradient(135deg,#6c5ce7,#fd79a8);margin-left:4px}
  .avatar-btn .av{width:100%;height:100%;border-radius:50%;border:2px solid var(--card)}

  /* ── AVATARS ── */
  .av{
    border-radius:50%;display:grid;place-items:center;color:#fff;
    font-weight:700;flex-shrink:0;user-select:none;letter-spacing:.2px;
  }
  .av-28{width:28px;height:28px;font-size:11px}
  .av-36{width:36px;height:36px;font-size:13px}
  .av-44{width:44px;height:44px;font-size:15px}
  .av-64{width:64px;height:64px;font-size:22px}
  .av-84{width:84px;height:84px;font-size:29px}

  /* ── LAYOUT ── */
  .layout{
    max-width:1240px;margin:0 auto;padding:22px 20px 40px;
    display:grid;grid-template-columns:236px minmax(0,1fr) 296px;gap:22px;
    align-items:start;
  }
  .col-left,.col-right{position:sticky;top:86px;display:flex;flex-direction:column;gap:16px}

  .card{
    background:var(--card);border:1px solid var(--border);border-radius:var(--radius);
    box-shadow:var(--shadow-sm);
  }

  /* ── PROFILE MINI ── */
  .profile-mini{overflow:hidden;text-align:center;padding-bottom:8px}
  .profile-cover{height:74px;background:linear-gradient(120deg,#6c5ce7,#a29bfe 45%,#fd79a8);position:relative}
  .profile-cover::after{
    content:"";position:absolute;inset:0;
    background:radial-gradient(circle at 20% 120%,rgba(255,255,255,.45),transparent 55%);
  }
  .profile-mini .av{margin:-32px auto 0;border:4px solid var(--card);position:relative;z-index:1}
  .profile-mini h3{font-size:15px;margin-top:10px;font-weight:700}
  .profile-mini p{font-size:12.5px;color:var(--muted);margin-top:2px}
  .profile-stats{display:flex;margin-top:14px;border-top:1px solid var(--border);padding-top:12px}
  .profile-stats div{flex:1;cursor:pointer;transition:.18s;border-radius:8px;padding:2px}
  .profile-stats div:hover{background:var(--card-2)}
  .profile-stats b{display:block;font-size:15px;font-weight:700}
  .profile-stats small{font-size:11px;color:var(--muted)}

  /* ── NAV ── */
  .nav{padding:8px}
  .nav-item{
    display:flex;align-items:center;gap:12px;width:100%;padding:11px 12px;
    border-radius:11px;font-size:14.5px;font-weight:500;color:var(--muted);
    transition:.16s;position:relative;
  }
  .nav-item:hover{background:var(--card-2);color:var(--text)}
  .nav-item.active{
    background:linear-gradient(90deg,var(--accent-soft),transparent);
    color:var(--accent);font-weight:600;
  }
  .nav-item.active::before{
    content:"";position:absolute;left:0;top:50%;transform:translateY(-50%);
    width:3px;height:20px;border-radius:0 3px 3px 0;background:var(--accent);
  }
  .nav-item .count{
    margin-left:auto;font-size:11.5px;font-weight:700;color:#fff;
    background:linear-gradient(135deg,#ff4d6d,#ff7a8f);padding:2px 7px;border-radius:9px;
  }
  .nav-item .count.soft{background:var(--accent-soft);color:var(--accent)}

  /* ── STORIES ── */
  .stories{padding:14px;display:flex;gap:14px;overflow-x:auto;scrollbar-width:none}
  .stories::-webkit-scrollbar{display:none}
  .story{width:70px;flex-shrink:0;text-align:center;cursor:pointer}
  .story-ring{
    width:66px;height:66px;border-radius:50%;padding:3px;
    background:linear-gradient(135deg,#6c5ce7,#fd79a8,#fdcb6e);
    transition:transform .2s;
  }
  .story:hover .story-ring{transform:scale(1.06)}
  .story.seen .story-ring{background:var(--border)}
  .story-ring .av{width:100%;height:100%;border:3px solid var(--card)}
  .story p{font-size:11px;color:var(--muted);margin-top:6px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .story.mine{position:relative}
  .story.mine .plus{
    position:absolute;bottom:20px;right:2px;width:22px;height:22px;border-radius:50%;
    background:var(--accent);color:#fff;display:grid;place-items:center;
    border:2.5px solid var(--card);font-size:14px;font-weight:700;line-height:1;
  }

  /* ── COMPOSER ── */
  .composer{padding:16px}
  .composer-top{display:flex;gap:12px}
  .composer textarea{
    flex:1;border:none;background:transparent;resize:none;outline:none;
    font-size:15px;line-height:1.5;min-height:44px;max-height:200px;padding-top:9px;
  }
  .composer textarea::placeholder{color:var(--muted)}
  .composer-tools{
    display:flex;align-items:center;gap:4px;margin-top:12px;
    padding-top:12px;border-top:1px solid var(--border);
  }
  .tool{
    display:flex;align-items:center;gap:7px;padding:8px 11px;border-radius:10px;
    font-size:13px;font-weight:500;color:var(--muted);transition:.16s;
  }
  .tool:hover{background:var(--card-2);color:var(--accent)}
  .tool.green:hover{color:#00b894}.tool.orange:hover{color:#e17055}.tool.blue:hover{color:#0984e3}
  .btn-post{
    margin-left:auto;padding:10px 22px;border-radius:11px;font-size:14px;font-weight:600;
    color:#fff;background:linear-gradient(135deg,#6c5ce7,#8b7cff);
    box-shadow:0 4px 14px rgba(108,92,231,.35);transition:.18s;
  }
  .btn-post:hover{transform:translateY(-1px);box-shadow:0 6px 20px rgba(108,92,231,.45)}
  .btn-post:active{transform:translateY(0) scale(.98)}
  .btn-post:disabled{opacity:.45;cursor:not-allowed;box-shadow:none;transform:none}

  /* ── POST ── */
  .post{margin-bottom:16px;overflow:hidden}
  .post-head{display:flex;align-items:center;gap:12px;padding:16px 16px 0}
  .post-head .who{flex:1;min-width:0}
  .post-head .name{font-size:14.5px;font-weight:650;display:flex;align-items:center;gap:5px}
  .verified{color:var(--accent)}
  .post-head .meta{font-size:12.5px;color:var(--muted);display:flex;align-items:center;gap:5px;margin-top:2px}
  .dot{width:3px;height:3px;border-radius:50%;background:var(--muted)}
  .post-body{padding:12px 16px 0;font-size:15px;line-height:1.55;white-space:pre-wrap;word-wrap:break-word}
  .post-media{
    margin:13px 16px 0;border-radius:13px;height:250px;position:relative;overflow:hidden;
    display:grid;place-items:center;font-size:52px;
  }
  .post-media::after{
    content:"";position:absolute;inset:0;
    background:radial-gradient(circle at 75% 15%,rgba(255,255,255,.35),transparent 50%);
  }
  .post-media .emo{position:relative;z-index:1;filter:drop-shadow(0 6px 16px rgba(0,0,0,.28))}
  .post-stats{
    display:flex;align-items:center;gap:6px;padding:12px 16px 0;
    font-size:12.5px;color:var(--muted);
  }
  .like-bubble{
    width:19px;height:19px;border-radius:50%;display:grid;place-items:center;
    background:linear-gradient(135deg,#ff4d6d,#ff7a8f);color:#fff;
  }
  .post-stats .right{margin-left:auto;display:flex;gap:12px}
  .post-actions{
    display:flex;margin:10px 16px 0;padding:6px 0;border-top:1px solid var(--border);
  }
  .act{
    flex:1;display:flex;align-items:center;justify-content:center;gap:8px;
    padding:9px;border-radius:10px;font-size:13.5px;font-weight:550;color:var(--muted);
    transition:.15s;
  }
  .act:hover{background:var(--card-2);color:var(--text)}
  .act.liked{color:var(--like)}
  .act.liked svg{fill:var(--like);stroke:var(--like)}
  .act.liked .heart{animation:pop .35s cubic-bezier(.2,1.6,.5,1)}
  @keyframes pop{0%{transform:scale(1)}45%{transform:scale(1.4)}100%{transform:scale(1)}}
  .act.active{color:var(--accent)}

  /* ── COMMENTS ── */
  .comments{display:none;padding:12px 16px 16px;border-top:1px solid var(--border);margin-top:10px;background:var(--card-2)}
  .comments.open{display:block;animation:fade .22s ease}
  @keyframes fade{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:none}}
  .comment{display:flex;gap:10px;margin-bottom:12px}
  .comment .bubble{
    background:var(--card);border:1px solid var(--border);
    padding:9px 13px;border-radius:14px;border-top-left-radius:4px;max-width:100%;
  }
  .comment .cname{font-size:13px;font-weight:650}
  .comment .ctext{font-size:13.5px;line-height:1.45;margin-top:2px;word-wrap:break-word}
  .comment-form{display:flex;gap:9px;align-items:center}
  .comment-form input{
    flex:1;height:40px;border-radius:20px;border:1px solid var(--border);
    background:var(--card);padding:0 16px;font-size:13.5px;outline:none;transition:.18s;
  }
  .comment-form input:focus{border-color:var(--accent);box-shadow:0 0 0 3px var(--accent-soft)}
  .send-btn{
    width:40px;height:40px;border-radius:50%;display:grid;place-items:center;flex-shrink:0;
    background:linear-gradient(135deg,#6c5ce7,#8b7cff);color:#fff;transition:.18s;
  }
  .send-btn:hover{transform:scale(1.07)}

  /* ── RIGHT COLUMN ── */
  .side-title{
    display:flex;align-items:center;justify-content:space-between;
    padding:15px 16px 11px;font-size:14.5px;font-weight:700;
  }
  .side-title a{font-size:12.5px;color:var(--accent);font-weight:600;cursor:pointer}
  .side-title a:hover{text-decoration:underline}
  .friend{
    display:flex;align-items:center;gap:11px;padding:8px 16px;cursor:pointer;transition:.15s;
  }
  .friend:hover{background:var(--card-2)}
  .friend .info{flex:1;min-width:0}
  .friend .fname{font-size:13.5px;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .friend .fstatus{font-size:12px;color:var(--muted)}
  .online{width:9px;height:9px;border-radius:50%;background:#00d68f;box-shadow:0 0 0 2.5px var(--card);flex-shrink:0}
  .offline{width:9px;height:9px;border-radius:50%;background:var(--border);flex-shrink:0}
  .friend-wrap{position:relative}
  .friend-wrap .online,.friend-wrap .offline{position:absolute;left:38px;bottom:8px;z-index:2}
  .friend-list{padding-bottom:8px}

  .trend{padding:9px 16px;cursor:pointer;transition:.15s;border-radius:10px}
  .trend:hover{background:var(--card-2)}
  .trend .t-name{font-size:13.5px;font-weight:650}
  .trend .t-count{font-size:12px;color:var(--muted);margin-top:2px}

  .footer-note{padding:4px 16px 16px;font-size:11.5px;color:var(--muted);line-height:1.6}

  /* ── BOTTOM NAV (mobile) ── */
  .bottom-nav{
    display:none;position:fixed;bottom:0;left:0;right:0;z-index:70;
    background:color-mix(in srgb,var(--card) 92%,transparent);
    backdrop-filter:blur(16px);-webkit-backdrop-filter:blur(16px);
    border-top:1px solid var(--border);
    padding:8px 6px calc(8px + env(safe-area-inset-bottom));
    justify-content:space-around;
  }
  .bn-item{
    display:flex;flex-direction:column;align-items:center;gap:3px;
    font-size:10.5px;color:var(--muted);padding:4px 12px;border-radius:10px;transition:.15s;position:relative;
  }
  .bn-item.active{color:var(--accent)}
  .bn-item .badge{top:-2px;right:2px}

  /* ── TOAST ── */
  .toast{
    position:fixed;bottom:26px;left:50%;transform:translate(-50%,120px);
    background:var(--text);color:var(--card);padding:13px 22px;border-radius:13px;
    font-size:13.5px;font-weight:550;box-shadow:0 12px 34px rgba(0,0,0,.28);
    z-index:200;opacity:0;transition:.35s cubic-bezier(.2,.9,.3,1.2);
    display:flex;align-items:center;gap:9px;pointer-events:none;max-width:90vw;
  }
  .toast.show{transform:translate(-50%,0);opacity:1}

  .empty{padding:40px 20px;text-align:center;color:var(--muted);font-size:14px}

  /* ── RESPONSIVE ── */
  @media(max-width:1080px){
    .layout{grid-template-columns:220px minmax(0,1fr);}
    .col-right{display:none}
  }
  @media(max-width:760px){
    .layout{grid-template-columns:minmax(0,1fr);padding:14px 12px 30px}
    .col-left{display:none}
    .bottom-nav{display:flex}
    .topbar-inner{height:58px;padding:0 14px;gap:10px}
    .logo span{display:none}
    .search{max-width:none}
    .topbar-actions .icon-btn.desktop-only{display:none}
    .post-media{height:200px;margin-left:12px;margin-right:12px}
    .post-head{padding:14px 12px 0}
    .post-body{padding:10px 12px 0}
    .post-actions{margin:10px 8px 0}
    .comments{padding:12px}
    .composer{padding:13px}
    body{padding-bottom:74px}
  }
</style>
</head>
<body>

<!-- ══════════ TOPBAR ══════════ -->
<header class="topbar">
  <div class="topbar-inner">
    <div class="logo">
      <div class="logo-mark">П</div>
      <span>Пульс</span>
    </div>

    <div class="search">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
      <input type="text" id="globalSearch" placeholder="Поиск людей, постов, групп…">
    </div>

    <div class="topbar-actions">
      <button class="icon-btn desktop-only" id="themeBtn" title="Сменить тему"></button>
      <button class="icon-btn desktop-only" data-nav="messages" title="Сообщения">
        <svg width="21" height="21" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/></svg>
        <span class="badge">3</span>
      </button>
      <button class="icon-btn desktop-only" data-nav="notifications" title="Уведомления">
        <svg width="21" height="21" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg>
        <span class="badge">7</span>
      </button>
      <button class="avatar-btn" data-nav="profile" title="Мой профиль">
        <div class="av" id="topAvatar"></div>
      </button>
    </div>
  </div>
</header>

<!-- ══════════ LAYOUT ══════════ -->
<div class="layout">

  <!-- LEFT -->
  <aside class="col-left">
    <div class="card profile-mini">
      <div class="profile-cover"></div>
      <div class="av av-64" id="sideAvatar"></div>
      <h3 id="sideName">Алекс Морозов</h3>
      <p id="sideHandle">@alexmorozov</p>
      <div class="profile-stats">
        <div><b>248</b><small>постов</small></div>
        <div><b>1.2K</b><small>друзей</small></div>
        <div><b>8.4K</b><small>подписчиков</small></div>
      </div>
    </div>

    <nav class="card nav" id="mainNav">
      <button class="nav-item active" data-nav="feed">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>
        Новости
      </button>
      <button class="nav-item" data-nav="messages">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/></svg>
        Сообщения
        <span class="count">3</span>
      </button>
      <button class="nav-item" data-nav="friends">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>
        Друзья
        <span class="count soft">12</span>
      </button>
      <button class="nav-item" data-nav="groups">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 21h18"/><path d="M5 21V7l7-4 7 4v14"/><path d="M9 21v-6h6v6"/></svg>
        Группы
      </button>
      <button class="nav-item" data-nav="saved">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21l-7-5-7 5V5a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2z"/></svg>
        Закладки
      </button>
      <button class="nav-item" data-nav="settings">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.6 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
        Настройки
      </button>
    </nav>
  </aside>

  <!-- CENTER -->
  <main>
    <!-- Stories -->
    <div class="card stories" id="stories"></div>

    <!-- Composer -->
    <div class="card composer" style="margin-top:16px">
      <div class="composer-top">
        <div class="av av-44" id="composeAvatar"></div>
        <textarea id="postText" placeholder="Что у вас нового, Алекс?" rows="1"></textarea>
      </div>
      <div class="composer-tools">
        <button class="tool green" data-tool="photo">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
          Фото
        </button>
        <button class="tool orange" data-tool="mood">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M8 14s1.5 2 4 2 4-2 4-2"/><line x1="9" y1="9" x2="9.01" y2="9"/><line x1="15" y1="9" x2="15.01" y2="9"/></svg>
          Настроение
        </button>
        <button class="tool blue" data-tool="geo">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
          Место
        </button>
        <button class="btn-post" id="publishBtn" disabled>Опубликовать</button>
      </div>
    </div>

    <!-- Feed -->
    <div id="feed" style="margin-top:16px"></div>
  </main>

  <!-- RIGHT -->
  <aside class="col-right">
    <div class="card">
      <div class="side-title">Друзья онлайн <a>Все</a></div>
      <div class="friend-list" id="friendsList"></div>
    </div>

    <div class="card">
      <div class="side-title">Популярное сейчас</div>
      <div id="trendsList" style="padding-bottom:10px"></div>
      <div class="footer-note">
        © 2025 Пульс · О проекте · Помощь · Правила · Реклама
      </div>
    </div>
  </aside>
</div>

<!-- ══════════ BOTTOM NAV ══════════ -->
<nav class="bottom-nav" id="bottomNav">
  <button class="bn-item active" data-nav="feed">
    <svg width="21" height="21" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>
    Лента
  </button>
  <button class="bn-item" data-nav="messages">
    <svg width="21" height="21" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/></svg>
    Чаты
    <span class="badge">3</span>
  </button>
  <button class="bn-item" data-nav="create">
    <svg width="21" height="21" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
    Создать
  </button>
  <button class="bn-item" data-nav="friends">
    <svg width="21" height="21" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>
    Друзья
  </button>
  <button class="bn-item" data-nav="profile">
    <svg width="21" height="21" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
    Профиль
  </button>
</nav>

<div class="toast" id="toast"></div>

<script>
/* ═══════════════ ДАННЫЕ ═══════════════ */
const USER = { name:'Алекс Морозов', handle:'@alexmorozov' };

const PALETTES = [
  ['#6c5ce7','#a29bfe'], ['#00b894','#55efc4'], ['#e17055','#fab1a0'],
  ['#0984e3','#74b9ff'], ['#e84393','#fd79a8'], ['#fdcb6e','#e17055'],
  ['#00cec9','#81ecec'], ['#d63031','#ff7675'], ['#6c5ce7','#fd79a8'],
  ['#2d3436','#636e72']
];
const MEDIA_GRADS = [
  'linear-gradient(135deg,#667eea,#764ba2)',
  'linear-gradient(135deg,#f093fb,#f5576c)',
  'linear-gradient(135deg,#4facfe,#00f2fe)',
  'linear-gradient(135deg,#43e97b,#38f9d7)',
  'linear-gradient(135deg,#fa709a,#fee140)',
  'linear-gradient(135deg,#30cfd0,#330867)',
  'linear-gradient(135deg,#ff9a9e,#fecfef)',
  'linear-gradient(135deg,#a18cd1,#fbc2eb)'
];

function hashOf(str){ let h=0; for(let i=0;i<str.length;i++) h=(h*31+str.charCodeAt(i))>>>0; return h; }
function gradOf(seed){ const p=PALETTES[hashOf(seed)%PALETTES.length]; return `linear-gradient(135deg,${p[0]},${p[1]})`; }
function initials(name){
  const parts=name.trim().split(/\s+/).slice(0,2);
  return parts.map(p=>p[0]).join('').toUpperCase();
}
function avatarHTML(name, size='44'){
  return `<div class="av av-${size}" style="background:${gradOf(name)}">${initials(name)}</div>`;
}

let uid = 100;
const state = {
  posts: [
    {
      id: uid++, author:'Мария Соколова', time:'12 минут назад', place:'Москва',
      text:'Наконец-то закончила ремонт в мастерской! Три месяца пыли, краски и бессонных ночей — и вот оно, моё любимое место на земле. Кто заходил в гости, тот знает, чего это стоило 😅',
      media:{ grad:MEDIA_GRADS[0], emoji:'🎨' },
      likes:284, liked:false, shares:12, commentsOpen:false,
      comments:[
        { author:'Игорь Лебедев', text:'Выглядит потрясающе! Скинь фото с другого ракурса 🙌' },
        { author:'Мария Соколова', text:'Игорь, вечером выложу ещё, там есть на что посмотреть)' }
      ]
    },
    {
      id: uid++, author:'Дмитрий Волков', time:'1 час назад',
      text:'Мысли вслух: продуктивность — это не про то, как много ты успеваешь. Это про то, как мало ты делаешь лишнего.\n\nУбрал из недели три встречи, которые можно было решить сообщением. Освободилось 6 часов. Шесть!',
      media:null,
      likes:512, liked:true, shares:47, commentsOpen:false,
      comments:[
        { author:'Анна Петрова', text:'Вот это откликается. Особенно про встречи.' },
        { author:'Сергей Кузнецов', text:'Согласен, но как отказывать, когда настаивают?' },
        { author:'Дмитрий Волков', text:'Сергей, «давайте я пришлю summary письмом» — работает в 9 из 10 случаев.' }
      ]
    },
    {
      id: uid++, author:'Анна Петрова', time:'3 часа назад', place:'Санкт-Петербург',
      text:'Питер сегодня устроил нам вот это. Стояла на набережной минут двадцать и просто дышала.',
      media:{ grad:MEDIA_GRADS[3], emoji:'🌇' },
      likes:1204, liked:false, shares:88, commentsOpen:false,
      comments:[
        { author:'Ольга Смирнова', text:'Боже, какая красота 😍' }
      ]
    },
    {
      id: uid++, author:'Игорь Лебедев', time:'5 часов назад',
      text:'Собрал подборку из 10 книг, которые изменили мой взгляд на разработку. Первая — «Программист-прагматик», и она до сих пор вне конкуренции.\n\nКому интересно — ссылка в закладках.',
      media:{ grad:MEDIA_GRADS[5], emoji:'📚' },
      likes:376, liked:false, shares:64, commentsOpen:false,
      comments:[]
    }
  ],
  stories: [
    { name:'Ваша история', mine:true },
    { name:'Мария С.', seen:false },
    { name:'Дмитрий В.', seen:false },
    { name:'Анна П.', seen:false },
    { name:'Сергей К.', seen:true },
    { name:'Ольга С.', seen:false },
    { name:'Игорь Л.', seen:true }
  ],
  friends: [
    { name:'Мария Соколова', status:'в сети', online:true },
    { name:'Дмитрий Волков', status:'печатает…', online:true },
    { name:'Анна Петрова', status:'в сети', online:true },
    { name:'Сергей Кузнецов', status:'был(а) 5 мин назад', online:true },
    { name:'Ольга Смирнова', status:'был(а) 2 ч назад', online:false },
    { name:'Игорь Лебедев', status:'был(а) вчера', online:false }
  ],
  trends: [
    { tag:'#ПульсДизайн', count:'12.4K постов' },
    { tag:'#УтреннийКофе', count:'8.1K постов' },
    { tag:'#КодИКофе', count:'5.7K постов' },
    { tag:'#ПитерОсень', count:'4.2K постов' },
    { tag:'#КнижныйКлуб', count:'3.9K постов' }
  ]
};

/* ═══════════════ SVG ХЕЛПЕРЫ ═══════════════ */
const ICONS = {
  like:'<path d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 2-1.7l1.38-9a2 2 0 0 0-2-2.3zM7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3"/>',
  heart:'<path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/>',
  comment:'<path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/>',
  share:'<circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/>',
  send:'<line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/>',
  more:'<circle cx="12" cy="12" r="1.6"/><circle cx="19" cy="12" r="1.6"/><circle cx="5" cy="12" r="1.6"/>',
  check:'<polyline points="20 6 9 17 4 12"/>',
  sun:'<circle cx="12" cy="12" r="5"/><line x1="12" y1="1" x2="12" y2="3"/><line x1="12" y1="21" x2="12" y2="23"/><line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/><line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/><line x1="1" y1="12" x2="3" y2="12"/><line x1="21" y1="12" x2="23" y2="12"/><line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/><line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>',
  moon:'<path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>'
};
function svg(name, size=18, fill='none'){
  return `<svg width="${size}" height="${size}" viewBox="0 0 24 24" fill="${fill}" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">${ICONS[name]}</svg>`;
}

/* ═══════════════ УТИЛИТЫ ═══════════════ */
function formatNum(n){
  if (n >= 1000000) return (n/1000000).toFixed(1).replace('.0','') + 'M';
  if (n >= 1000) return (n/1000).toFixed(1).replace('.0','') + 'K';
  return String(n);
}
function plural(n, one, few, many){
  const m10 = n % 10, m100 = n % 100;
  if (m10 === 1 && m100 !== 11) return one;
  if (m10 >= 2 && m10 <= 4 && (m100 < 10 || m100 >= 20)) return few;
  return many;
}
function escapeHTML(s){
  return s.replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}
function toast(msg, iconName){
  const t = document.getElementById('toast');
  t.innerHTML = (iconName ? svg(iconName,18) : '') + '<span>' + escapeHTML(msg) + '</span>';
  t.classList.add('show');
  clearTimeout(t._tid);
  t._tid = setTimeout(()=>t.classList.remove('show'), 2200);
}

/* ═══════════════ РЕНДЕР: ШАПКА / ПРОФИЛЬ ═══════════════ */
function renderIdentity(){
  const av = document.getElementById('topAvatar');
  av.style.background = gradOf(USER.name);
  av.textContent = initials(USER.name);
  av.style.color = '#fff';

  const s = document.getElementById('sideAvatar');
  s.style.background = gradOf(USER.name);
  s.textContent = initials(USER.name);

  const ca = document.getElementById('composeAvatar');
  ca.style.background = gradOf(USER.name);
  ca.textContent = initials(USER.name);

  document.getElementById('sideName').textContent = USER.name;
  document.getElementById('sideHandle').textContent = USER.handle;
  document.getElementById('postText').placeholder = `Что у вас нового, ${USER.name.split(' ')[0]}?`;
}

/* ═══════════════ РЕНДЕР: STORIES ═══════════════ */
function renderStories(){
  const el = document.getElementById('stories');
  el.innerHTML = state.stories.map((s,i)=>{
    const cls = 'story' + (s.mine?' mine':'') + (s.seen?' seen':'');
    const label = s.mine ? 'Ваша' : s.name;
    const inner = s.mine
      ? `<div class="story-ring" style="background:var(--border)"><div class="av" style="width:100%;height:100%;border:3px solid var(--card);background:${gradOf(USER.name)}">${initials(USER.name)}</div></div><div class="plus">+</div>`
      : `<div class="story-ring"><div class="av" style="width:100%;height:100%;border:3px solid var(--card);background:${gradOf(s.name)}">${initials(s.name)}</div></div>`;
    return `<div class="${cls}" data-story="${i}">${inner}<p>${escapeHTML(label)}</p></div>`;
  }).join('');
}

/* ═══════════════ РЕНДЕР: ДРУЗЬЯ ═══════════════ */
function renderFriends(){
  document.getElementById('friendsList').innerHTML = state.friends.map(f=>`
    <div class="friend friend-wrap" data-friend="${escapeHTML(f.name)}">
      <div class="av av-36" style="background:${gradOf(f.name)}">${initials(f.name)}</div>
      <span class="${f.online?'online':'offline'}"></span>
      <div class="info">
        <div class="fname">${escapeHTML(f.name)}</div>
        <div class="fstatus">${escapeHTML(f.status)}</div>
      </div>
    </div>
  `).join('');
}

/* ═══════════════ РЕНДЕР: ТРЕНДЫ ═══════════════ */
function renderTrends(){
  document.getElementById('trendsList').innerHTML = state.trends.map(t=>`
    <div class="trend" data-trend="${escapeHTML(t.tag)}">
      <div class="t-name">${escapeHTML(t.tag)}</div>
      <div class="t-count">${escapeHTML(t.count)}</div>
    </div>
  `).join('');
}

/* ═══════════════ РЕНДЕР: ПОСТЫ ═══════════════ */
function postHTML(p){
  const media = p.media ? `
    <div class="post-media" style="background:${p.media.grad}">
      <span class="emo">${p.media.emoji}</span>
    </div>` : '';

  const likedClass = p.liked ? ' liked' : '';
  const likesLabel = p.likes > 0 ? formatNum(p.likes) : '';
  const cCount = p.comments.length;

  const commentsHTML = p.comments.map(c=>`
    <div class="comment">
      <div class="av av-28" style="background:${gradOf(c.author)}">${initials(c.author)}</div>
      <div class="bubble">
        <div class="cname">${escapeHTML(c.author)}</div>
        <div class="ctext">${escapeHTML(c.text)}</div>
      </div>
    </div>`).join('');

  return `
  <article class="card post" data-id="${p.id}">
    <div class="post-head">
      <div class="av av-44" style="background:${gradOf(p.author)}">${initials(p.author)}</div>
      <div class="who">
        <div class="name">${escapeHTML(p.author)}
          <span class="verified">${svg('check',13)}</span>
        </div>
        <div class="meta">
          <span>${escapeHTML(p.time)}</span>
          ${p.place ? `<span class="dot"></span><span>${escapeHTML(p.place)}</span>` : ''}
        </div>
      </div>
      <button class="icon-btn" style="width:36px;height:36px" data-action="more">${svg('more',20, 'currentColor')}</button>
    </div>

    <div class="post-body">${escapeHTML(p.text)}</div>
    ${media}

    <div class="post-stats">
      <span class="like-bubble">${svg('heart',11,'#fff')}</span>
      <span data-likes-label>${likesLabel}</span>
      <div class="right">
        <span data-ccount>${cCount} ${plural(cCount,'комментарий','комментария','комментариев')}</span>
        <span>${p.shares} ${plural(p.shares,'репост','репоста','репостов')}</span>
      </div>
    </div>

    <div class="post-actions">
      <button class="act${likedClass}" data-action="like">
        <span class="heart">${svg('heart',18)}</span> Нравится
      </button>
      <button class="act" data-action="toggle-comments">
        ${svg('comment',18)} Комментировать
      </button>
      <button class="act" data-action="share">
        ${svg('share',18)} Поделиться
      </button>
    </div>

    <div class="comments${p.commentsOpen?' open':''}" data-comments>
      <div data-comment-list>${commentsHTML}</div>
      <div class="comment-form">
        <div class="av av-28" style="background:${gradOf(USER.name)}">${initials(USER.name)}</div>
        <input type="text" placeholder="Написать комментарий…" data-comment-input>
        <button class="send-btn" data-action="send-comment">${svg('send',16)}</button>
      </div>
    </div>
  </article>`;
}

function renderFeed(){
  const feed = document.getElementById('feed');
  if (state.posts.length === 0){
    feed.innerHTML = '<div class="card empty">Пока нет постов. Напишите первый!</div>';
    return;
  }
  feed.innerHTML = state.posts.map(postHTML).join('');
}

/* ═══════════════ ДЕЙСТВИЯ ═══════════════ */
function findPost(id){ return state.posts.find(p => p.id === Number(id)); }

document.addEventListener('click', (e) => {
  const actBtn = e.target.closest('[data-action]');
  const postEl = e.target.closest('.post');

  if (actBtn && postEl){
    const post = findPost(postEl.dataset.id);
    if (!post) return;
    const action = actBtn.dataset.action;

    /* ── ЛАЙК ── */
    if (action === 'like'){
      post.liked = !post.liked;
      post.likes += post.liked ? 1 : -1;
      actBtn.classList.toggle('liked', post.liked);
      const label = postEl.querySelector('[data-likes-label]');
      label.textContent = post.likes > 0 ? formatNum(post.likes) : '';
      if (post.liked){
        const h = actBtn.querySelector('.heart');
        h.style.animation = 'none';
        void h.offsetWidth;
        h.style.animation = '';
        h.classList.add('heart');
      }
      return;
    }

    /* ── КОММЕНТАРИИ ── */
    if (action === 'toggle-comments'){
      const box = postEl.querySelector('[data-comments]');
      const isOpen = box.classList.toggle('open');
      post.commentsOpen = isOpen;
      actBtn.classList.toggle('active', isOpen);
      if (isOpen) setTimeout(()=>postEl.querySelector('[data-comment-input]')?.focus(), 120);
      return;
    }

    /* ── ОТПРАВИТЬ КОММЕНТАРИЙ ── */
    if (action === 'send-comment'){
      const input = postEl.querySelector('[data-comment-input]');
      const text = input.value.trim();
      if (!text) { input.focus(); return; }
      post.comments.push({ author: USER.name, text });
      const list = postEl.querySelector('[data-comment-list]');
      const div = document.createElement('div');
      div.className = 'comment';
      div.innerHTML = `
        <div class="av av-28" style="background:${gradOf(USER.name)}">${initials(USER.name)}</div>
        <div class="bubble">
          <div class="cname">${escapeHTML(USER.name)}</div>
          <div class="ctext">${escapeHTML(text)}</div>
        </div>`;
      list.appendChild(div);
      input.value = '';
      input.focus();
      const c = post.comments.length;
      postEl.querySelector('[data-ccount]').textContent =
        `${c} ${plural(c,'комментарий','комментария','комментариев')}`;
      return;
    }

    /* ── ПОДЕЛИТЬСЯ ── */
    if (action === 'share'){
      post.shares++;
      const stats = postEl.querySelectorAll('.post-stats .right span');
      stats[1].textContent = `${post.shares} ${plural(post.shares,'репост','репоста','репостов')}`;
      toast('Ссылка на пост скопирована', 'share');
      return;
    }

    if (action === 'more'){
      toast('Меню поста: скрыть, пожаловаться, сохранить');
      return;
    }
  }

  /* ── STORIES ── */
  const story = e.target.closest('[data-story]');
  if (story){
    const i = Number(story.dataset.story);
    if (state.stories[i].mine){
      document.getElementById('postText').focus();
      toast('Загрузите фото для своей истории');
    } else {
      state.stories[i].seen = true;
      renderStories();
      toast('История ' + state.stories[i].name + ' открыта');
    }
    return;
  }

  /* ── ДРУЗЬЯ ── */
  const friend = e.target.closest('[data-friend]');
  if (friend){
    toast('Открыт профиль: ' + friend.dataset.friend);
    return;
  }

  /* ── ТРЕНДЫ ── */
  const trend = e.target.closest('[data-trend]');
  if (trend){
    document.getElementById('globalSearch').value = trend.dataset.trend;
    toast('Поиск по ' + trend.dataset.trend);
    return;
  }

  /* ── НАВИГАЦИЯ ── */
  const nav = e.target.closest('[data-nav]');
  if (nav){
    const target = nav.dataset.nav;
    document.querySelectorAll('[data-nav]').forEach(n => n.classList.remove('active'));
    if (target === 'create'){
      document.getElementById('postText').focus();
      window.scrollTo({top:0, behavior:'smooth'});
      toast('Напишите что-нибудь ✍️');
      return;
    }
    document.querySelectorAll(`.nav-item[data-nav="${target}"], .bn-item[data-nav="${target}"]`)
      .forEach(n => n.classList.add('active'));
    if (target === 'feed') window.scrollTo({top:0, behavior:'smooth'});
    else if (target !== 'profile') toast('Раздел «' + nav.textContent.trim().split('\n')[0] + '» в разработке');
    else toast('Это ваш профиль — ' + USER.handle);
    return;
  }
});

/* Отправка комментария по Enter */
document.addEventListener('keydown', (e) => {
  if (e.key === 'Enter' && e.target.matches('[data-comment-input]')){
    e.preventDefault();
    e.target.closest('.comments').querySelector('[data-action="send-comment"]').click();
  }
});

/* ═══════════════ СОЗДАНИЕ ПОСТА ═══════════════ */
const postText = document.getElementById('postText');
const publishBtn = document.getElementById('publishBtn');

postText.addEventListener('input', () => {
  postText.style.height = 'auto';
  postText.style.height = Math.min(postText.scrollHeight, 200) + 'px';
  publishBtn.disabled = postText.value.trim().length === 0;
});

publishBtn.addEventListener('click', () => {
  const text = postText.value.trim();
  if (!text) return;

  state.posts.unshift({
    id: uid++,
    author: USER.name,
    time: 'только что',
    place: null,
    text,
    media: null,
    likes: 0,
    liked: false,
    shares: 0,
    commentsOpen: false,
    comments: []
  });

  postText.value = '';
  postText.style.height = 'auto';
  publishBtn.disabled = true;
  renderFeed();
  window.scrollTo({ top: 0, behavior: 'smooth' });
  toast('Пост опубликован', 'check');
});

/* Инструменты композера */
document.querySelectorAll('[data-tool]').forEach(btn => {
  btn.addEventListener('click', () => {
    const t = btn.dataset.tool;
    const msgs = {
      photo: 'Выберите фото из галереи 📷',
      mood: 'Настроение: отличное 😄',
      geo: 'Геолокация добавлена: Москва 📍'
    };
    toast(msgs[t]);
  });
});

/* ═══════════════ ТЕМА ═══════════════ */
const themeBtn = document.getElementById('themeBtn');
let theme = 'light';
function applyTheme(){
  document.documentElement.setAttribute('data-theme', theme);
  themeBtn.innerHTML = theme === 'light' ? svg('moon',20) : svg('sun',20);
}
themeBtn.addEventListener('click', () => {
  theme = theme === 'light' ? 'dark' : 'light';
  applyTheme();
  toast(theme === 'dark' ? 'Тёмная тема включена 🌙' : 'Светлая тема включена ☀️');
});

/* ═══════════════ ПОИСК ═══════════════ */
const searchInput = document.getElementById('globalSearch');
searchInput.addEventListener('input', () => {
  const q = searchInput.value.trim().toLowerCase();
  if (!q){ renderFeed(); return; }
  const feed = document.getElementById('feed');
  const found = state.posts.filter(p =>
    p.text.toLowerCase().includes(q) || p.author.toLowerCase().includes(q)
  );
  if (found.length === 0){
    feed.innerHTML = `<div class="card empty">Ничего не найдено по запросу «${escapeHTML(searchInput.value)}»</div>`;
  } else {
    feed.innerHTML = found.map(postHTML).join('');
  }
});

/* ═══════════════ ИНИЦИАЛИЗАЦИЯ ═══════════════ */
renderIdentity();
renderStories();
renderFriends();
renderTrends();
renderFeed();
applyTheme();
</script>
</body>
</html>
