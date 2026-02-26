<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Nexus — MOBA Stats Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600&family=DM+Mono:wght@400;500&family=Sora:wght@600;700&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg: #f5f5f4;
  --surface: #ffffff;
  --surface2: #fafaf9;
  --border: #e7e5e4;
  --border2: #d6d3d1;
  --text: #1c1917;
  --text2: #78716c;
  --text3: #a8a29e;
  --accent: #2563eb;
  --accent-light: #eff6ff;
  --win: #16a34a;
  --win-light: #f0fdf4;
  --loss: #dc2626;
  --loss-light: #fef2f2;
  --gold: #d97706;
  --gold-light: #fffbeb;
  --radius: 12px;
  --radius-sm: 8px;
  --shadow: 0 1px 3px rgba(0,0,0,0.08), 0 1px 2px rgba(0,0,0,0.04);
}

body {
  font-family: 'DM Sans', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  font-size: 14px;
  line-height: 1.5;
}

.layout { display: flex; min-height: 100vh; }

/* SIDEBAR */
.sidebar {
  width: 220px;
  background: var(--surface);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  padding: 24px 0;
  position: fixed;
  top: 0; bottom: 0; left: 0;
  z-index: 50;
}

.sidebar-logo {
  padding: 0 20px 24px;
  border-bottom: 1px solid var(--border);
  margin-bottom: 8px;
}
.logo-mark {
  font-family: 'Sora', sans-serif;
  font-size: 18px;
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.5px;
}
.logo-dot {
  display: inline-block;
  width: 8px; height: 8px;
  background: var(--accent);
  border-radius: 50%;
  margin-left: 2px;
  vertical-align: middle;
  transform: translateY(-2px);
}

.nav-section { padding: 8px 12px; margin-bottom: 4px; }
.nav-section-label {
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text3);
  padding: 0 8px;
  margin-bottom: 4px;
}
.nav-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 10px;
  border-radius: var(--radius-sm);
  color: var(--text2);
  font-weight: 500;
  font-size: 13.5px;
  cursor: pointer;
  transition: all 0.15s;
  text-decoration: none;
}
.nav-item:hover { background: var(--bg); color: var(--text); }
.nav-item.active { background: var(--accent-light); color: var(--accent); font-weight: 600; }
.nav-icon { width: 16px; text-align: center; font-size: 15px; }

.sidebar-footer {
  margin-top: auto;
  padding: 16px 20px 0;
  border-top: 1px solid var(--border);
}
.user-chip {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: background 0.15s;
}
.user-chip:hover { background: var(--bg); }
.user-avatar {
  width: 32px; height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #93c5fd, #3b82f6);
  display: flex; align-items: center; justify-content: center;
  font-size: 14px; flex-shrink: 0;
}
.user-name { font-weight: 600; font-size: 13px; }
.user-rank { font-size: 11px; color: var(--text3); }

/* MAIN */
.main { margin-left: 220px; flex: 1; display: flex; flex-direction: column; }

/* TOPBAR */
.topbar {
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  padding: 0 28px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: sticky; top: 0; z-index: 40;
}
.page-title {
  font-family: 'Sora', sans-serif;
  font-size: 16px; font-weight: 700; letter-spacing: -0.3px;
}
.topbar-right { display: flex; align-items: center; gap: 12px; }

.search-bar {
  display: flex; align-items: center; gap: 8px;
  background: var(--bg);
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  padding: 7px 12px;
  transition: border-color 0.15s;
}
.search-bar:focus-within { border-color: var(--accent); }
.search-bar input {
  background: none; border: none; outline: none;
  font-family: 'DM Sans', sans-serif; font-size: 13px; color: var(--text); width: 180px;
}
.search-bar input::placeholder { color: var(--text3); }

.btn {
  display: flex; align-items: center; gap: 6px;
  padding: 7px 14px;
  border-radius: var(--radius-sm);
  font-family: 'DM Sans', sans-serif; font-size: 13px; font-weight: 500;
  cursor: pointer; border: none; transition: all 0.15s;
}
.btn-primary { background: var(--accent); color: #fff; }
.btn-primary:hover { background: #1d4ed8; }
.btn-outline { background: transparent; border: 1px solid var(--border2); color: var(--text2); }
.btn-outline:hover { background: var(--bg); color: var(--text); }

/* CONTENT */
.content { padding: 24px 28px; flex: 1; }

/* PROFILE HEADER */
.profile-header {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 24px;
  display: flex; align-items: center; gap: 20px;
  margin-bottom: 20px;
  box-shadow: var(--shadow);
  animation: fadeUp 0.4s ease both;
}
.profile-avatar {
  width: 64px; height: 64px; border-radius: 50%;
  background: linear-gradient(135deg, #dbeafe, #93c5fd);
  display: flex; align-items: center; justify-content: center;
  font-size: 28px; flex-shrink: 0;
  border: 3px solid var(--surface);
  box-shadow: 0 0 0 2px var(--border2);
}
.profile-text { flex: 1; }
.profile-name {
  font-family: 'Sora', sans-serif;
  font-size: 20px; font-weight: 700; letter-spacing: -0.5px; margin-bottom: 2px;
}
.profile-meta { display: flex; align-items: center; gap: 12px; color: var(--text2); font-size: 13px; }
.meta-dot { color: var(--border2); }

.profile-stats {
  display: flex; gap: 1px; background: var(--border);
  border-radius: var(--radius-sm); overflow: hidden;
}
.profile-stat {
  background: var(--surface); padding: 14px 20px; text-align: center; min-width: 100px;
  transition: background 0.12s; cursor: default;
}
.profile-stat:hover { background: var(--surface2); }
.ps-val {
  font-family: 'Sora', sans-serif;
  font-size: 18px; font-weight: 700; letter-spacing: -0.5px; margin-bottom: 2px;
}
.ps-lbl { font-size: 11px; color: var(--text3); font-weight: 500; text-transform: uppercase; letter-spacing: 0.05em; }

.rank-pill {
  display: flex; align-items: center; gap: 8px;
  background: var(--gold-light); border: 1px solid #fde68a;
  border-radius: 100px; padding: 6px 16px; white-space: nowrap;
}
.rank-pill-icon { font-size: 18px; }
.rank-pill-name { font-size: 13px; font-weight: 600; color: var(--gold); }
.rank-pill-lp { font-size: 12px; color: #92400e; font-family: 'DM Mono', monospace; }

/* KPI GRID */
.kpi-grid {
  display: grid; grid-template-columns: repeat(4,1fr); gap: 16px; margin-bottom: 20px;
}
.kpi-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 20px;
  animation: fadeUp 0.4s ease both;
}
.kpi-card:nth-child(2) { animation-delay: 0.05s; }
.kpi-card:nth-child(3) { animation-delay: 0.1s; }
.kpi-card:nth-child(4) { animation-delay: 0.15s; }

.kpi-label {
  font-size: 11.5px; color: var(--text3); font-weight: 600;
  text-transform: uppercase; letter-spacing: 0.06em;
  margin-bottom: 8px; display: flex; align-items: center; gap: 6px;
}
.kpi-value {
  font-family: 'Sora', sans-serif;
  font-size: 28px; font-weight: 700; letter-spacing: -1px; line-height: 1; margin-bottom: 8px;
}
.kpi-change { display: flex; align-items: center; gap: 4px; font-size: 12px; font-weight: 500; }
.up { color: var(--win); }
.down { color: var(--loss); }
.kpi-bar { height: 3px; background: var(--border); border-radius: 100px; overflow: hidden; margin-top: 8px; }
.kpi-bar-fill { height: 100%; border-radius: 100px; transition: width 1s ease; }

/* MAIN GRID */
.main-grid { display: grid; grid-template-columns: 1fr 340px; gap: 16px; }
.left-col { display: flex; flex-direction: column; gap: 16px; }
.right-col { display: flex; flex-direction: column; gap: 16px; }

/* CARD */
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  overflow: hidden;
  animation: fadeUp 0.4s ease both;
}
.card-header {
  padding: 15px 20px;
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center; justify-content: space-between;
}
.card-title { font-size: 13.5px; font-weight: 600; }
.card-sub { font-size: 12px; color: var(--text3); }

/* BADGE */
.badge {
  display: inline-flex; align-items: center; gap: 3px;
  padding: 2px 7px; border-radius: 100px;
  font-size: 11px; font-weight: 600;
}
.badge-win { background: var(--win-light); color: var(--win); }
.badge-loss { background: var(--loss-light); color: var(--loss); }
.badge-neutral { background: var(--bg); color: var(--text2); border: 1px solid var(--border); }
.badge-accent { background: var(--accent-light); color: var(--accent); }

/* MATCH LIST */
.match-item {
  display: flex; align-items: center; gap: 14px;
  padding: 12px 20px;
  border-bottom: 1px solid var(--border);
  cursor: pointer; transition: background 0.12s;
}
.match-item:last-child { border-bottom: none; }
.match-item:hover { background: var(--surface2); }
.match-stripe { width: 3px; border-radius: 100px; align-self: stretch; flex-shrink: 0; }
.match-item.win .match-stripe { background: var(--win); }
.match-item.loss .match-stripe { background: var(--loss); }
.champ-icon {
  width: 40px; height: 40px; border-radius: var(--radius-sm);
  background: var(--bg); border: 1px solid var(--border);
  display: flex; align-items: center; justify-content: center;
  font-size: 19px; flex-shrink: 0;
}
.match-info { flex: 1; min-width: 0; }
.match-top { display: flex; align-items: center; gap: 7px; margin-bottom: 3px; }
.match-champ { font-weight: 600; font-size: 14px; }
.match-kda { font-family: 'DM Mono', monospace; font-size: 12px; color: var(--text2); }
.match-right { text-align: right; flex-shrink: 0; }
.match-ratio { font-family: 'Sora', sans-serif; font-size: 16px; font-weight: 700; letter-spacing: -0.3px; }
.match-dur { font-size: 11px; color: var(--text3); font-family: 'DM Mono', monospace; }

/* TABS */
.tabs { display: flex; border-bottom: 1px solid var(--border); padding: 0 20px; }
.tab {
  padding: 10px 14px; font-size: 13px; font-weight: 500; color: var(--text2);
  cursor: pointer; border-bottom: 2px solid transparent; margin-bottom: -1px; transition: all 0.15s;
}
.tab:hover { color: var(--text); }
.tab.active { color: var(--accent); border-bottom-color: var(--accent); font-weight: 600; }

/* CHART */
.chart-area { padding: 20px; }
.bar-chart { display: flex; align-items: flex-end; gap: 6px; height: 90px; }
.bar-grp { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 4px; }
.bar-grp-bars { display: flex; gap: 2px; align-items: flex-end; height: 75px; width: 100%; }
.bar-seg { flex: 1; border-radius: 3px 3px 0 0; cursor: pointer; transition: opacity 0.15s; }
.bar-seg:hover { opacity: 0.7; }
.bar-w { background: var(--win); }
.bar-l { background: #fca5a5; }
.bar-day { font-size: 9px; color: var(--text3); font-weight: 600; font-family: 'DM Mono', monospace; letter-spacing: 0.04em; }
.chart-legend { display: flex; gap: 16px; margin-top: 12px; justify-content: flex-end; }
.legend-item { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); }
.legend-dot { width: 8px; height: 8px; border-radius: 2px; }

/* ROLE ROW */
.role-row { display: flex; gap: 8px; padding: 12px 20px; border-top: 1px solid var(--border); }
.role-chip {
  flex: 1; padding: 10px 6px;
  border-radius: var(--radius-sm); border: 1px solid var(--border);
  background: var(--surface2); text-align: center; cursor: pointer; transition: all 0.15s;
}
.role-chip:hover { border-color: var(--accent); background: var(--accent-light); }
.role-chip.active { border-color: var(--accent); background: var(--accent-light); }
.role-emoji { font-size: 18px; margin-bottom: 4px; }
.role-name { font-size: 9px; color: var(--text3); font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; }
.role-pct { font-size: 12px; font-weight: 700; color: var(--text); margin-top: 1px; }
.role-chip.active .role-pct { color: var(--accent); }

/* LIVE */
.live-header { display: flex; align-items: center; gap: 6px; }
.live-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--loss); animation: blink 1.2s infinite; }
@keyframes blink { 0%,100% { opacity: 1; } 50% { opacity: 0.2; } }
.live-label { font-size: 11px; font-weight: 600; color: var(--loss); letter-spacing: 0.05em; text-transform: uppercase; }

.live-timer-block {
  text-align: center; padding: 18px 20px; border-bottom: 1px solid var(--border);
}
.live-time {
  font-family: 'Sora', sans-serif;
  font-size: 44px; font-weight: 700; letter-spacing: -2px; line-height: 1; margin-bottom: 4px;
}
.live-sub { font-size: 11.5px; color: var(--text3); font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; }

.live-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1px; background: var(--border); }
.live-cell { background: var(--surface); padding: 14px 16px; text-align: center; transition: background 0.12s; }
.live-cell:hover { background: var(--surface2); }
.live-num {
  font-family: 'Sora', sans-serif;
  font-size: 24px; font-weight: 700; letter-spacing: -0.5px; line-height: 1; margin-bottom: 3px;
}
.live-lbl { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.07em; font-weight: 600; }

.compare-block { padding: 16px 20px; border-top: 1px solid var(--border); }
.compare-row { margin-bottom: 12px; }
.compare-row:last-child { margin-bottom: 0; }
.compare-meta { display: flex; justify-content: space-between; font-size: 11px; font-weight: 600; margin-bottom: 5px; }
.compare-track { flex: 1; height: 5px; background: var(--border); border-radius: 100px; overflow: hidden; }
.compare-fill { height: 100%; border-radius: 100px; transition: width 1s ease; }

/* CHAMPION MASTERY */
.champ-row {
  display: flex; align-items: center; gap: 12px;
  padding: 11px 20px; border-bottom: 1px solid var(--border);
  cursor: pointer; transition: background 0.12s;
}
.champ-row:last-child { border-bottom: none; }
.champ-row:hover { background: var(--surface2); }
.champ-num { width: 18px; font-size: 11px; color: var(--text3); font-weight: 600; font-family: 'DM Mono', monospace; }
.champ-portrait {
  width: 36px; height: 36px; border-radius: 50%;
  background: var(--bg); border: 1px solid var(--border);
  display: flex; align-items: center; justify-content: center; font-size: 17px;
}
.champ-info { flex: 1; }
.champ-name { font-weight: 600; font-size: 13.5px; }
.champ-games { font-size: 11px; color: var(--text3); }
.champ-metrics { display: flex; gap: 16px; }
.metric { text-align: right; }
.metric-val { font-size: 13px; font-weight: 600; font-family: 'DM Mono', monospace; }
.metric-lbl { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.05em; }
.wr-pill {
  display: flex; align-items: center;
  border-radius: 100px; padding: 3px 10px;
  border: 1px solid var(--border); font-family: 'DM Mono', monospace; font-size: 12px; font-weight: 500;
}
.wr-good { color: var(--win); background: var(--win-light); border-color: #bbf7d0; }
.wr-warn { color: var(--gold); background: var(--gold-light); border-color: #fde68a; }
.wr-bad { color: var(--loss); background: var(--loss-light); border-color: #fecaca; }

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}
::-webkit-scrollbar { width: 5px; height: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 10px; }
</style>
</head>
<body>
<div class="layout">

  <aside class="sidebar">
    <div class="sidebar-logo">
      <div class="logo-mark">nexus<span class="logo-dot"></span></div>
    </div>
    <div class="nav-section">
      <div class="nav-section-label">Main</div>
      <a class="nav-item active"><span class="nav-icon">📊</span> Overview</a>
      <a class="nav-item"><span class="nav-icon">🗡️</span> Champions</a>
      <a class="nav-item"><span class="nav-icon">📋</span> Match History</a>
      <a class="nav-item"><span class="nav-icon">📈</span> Performance</a>
    </div>
    <div class="nav-section">
      <div class="nav-section-label">Community</div>
      <a class="nav-item"><span class="nav-icon">🏆</span> Leaderboard</a>
      <a class="nav-item"><span class="nav-icon">👥</span> Friends</a>
      <a class="nav-item"><span class="nav-icon">🔎</span> Find Players</a>
    </div>
    <div class="sidebar-footer">
      <div class="user-chip">
        <div class="user-avatar">⚡</div>
        <div>
          <div class="user-name">NightHawk_X</div>
          <div class="user-rank">Gold I · 78 LP</div>
        </div>
      </div>
    </div>
  </aside>

  <div class="main">
    <header class="topbar">
      <div class="page-title">Overview</div>
      <div class="topbar-right">
        <div class="search-bar">
          <span style="font-size:13px">🔍</span>
          <input type="text" placeholder="Search summoner…">
        </div>
        <button class="btn btn-outline">Season 2025</button>
        <button class="btn btn-primary">↺ Refresh</button>
      </div>
    </header>

    <div class="content">

      <!-- PROFILE -->
      <div class="profile-header">
        <div class="profile-avatar">⚡</div>
        <div class="profile-text">
          <div class="profile-name">NightHawk_X</div>
          <div class="profile-meta">
            <span>EUW Server</span>
            <span class="meta-dot">·</span>
            <span>Level 247</span>
            <span class="meta-dot">·</span>
            <span style="color:var(--text3)">Last seen 12 min ago</span>
          </div>
        </div>
        <div class="profile-stats">
          <div class="profile-stat">
            <div class="ps-val">57.3<span style="font-size:13px;color:var(--text3)">%</span></div>
            <div class="ps-lbl">Win Rate</div>
          </div>
          <div class="profile-stat">
            <div class="ps-val">3.82</div>
            <div class="ps-lbl">KDA</div>
          </div>
          <div class="profile-stat">
            <div class="ps-val">284</div>
            <div class="ps-lbl">Games</div>
          </div>
          <div class="profile-stat">
            <div class="ps-val">7.4</div>
            <div class="ps-lbl">CS/min</div>
          </div>
        </div>
        <div class="rank-pill">
          <span class="rank-pill-icon">🏆</span>
          <span class="rank-pill-name">Gold I</span>
          <span class="rank-pill-lp">78 LP</span>
        </div>
      </div>

      <!-- KPI ROW -->
      <div class="kpi-grid">
        <div class="kpi-card">
          <div class="kpi-label">⚔️ Avg K/D/A</div>
          <div class="kpi-value">8 <span style="font-size:16px;color:var(--text3);font-weight:400">/ 3 / 11</span></div>
          <div class="kpi-change up">↑ +0.4 vs last week</div>
        </div>
        <div class="kpi-card">
          <div class="kpi-label">💥 Avg Damage</div>
          <div class="kpi-value">22<span style="font-size:16px;color:var(--text3);font-weight:400">k</span></div>
          <div class="kpi-change up">↑ +1.2k vs last week</div>
          <div class="kpi-bar"><div class="kpi-bar-fill" style="width:68%;background:var(--accent)"></div></div>
        </div>
        <div class="kpi-card">
          <div class="kpi-label">⏱ Avg Duration</div>
          <div class="kpi-value">24<span style="font-size:16px;color:var(--text3);font-weight:400">:31</span></div>
          <div class="kpi-change down">↓ -2:12 vs last week</div>
        </div>
        <div class="kpi-card">
          <div class="kpi-label">👁 Vision Score</div>
          <div class="kpi-value">28<span style="font-size:16px;color:var(--text3);font-weight:400">.1</span></div>
          <div class="kpi-change up">↑ +3.2 vs last week</div>
          <div class="kpi-bar"><div class="kpi-bar-fill" style="width:42%;background:var(--win)"></div></div>
        </div>
      </div>

      <!-- MAIN GRID -->
      <div class="main-grid">
        <div class="left-col">

          <!-- MATCH HISTORY -->
          <div class="card">
            <div style="display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border)">
              <div class="tabs" style="border:none;flex:1">
                <div class="tab active">All Games</div>
                <div class="tab">Ranked</div>
                <div class="tab">Normal</div>
              </div>
              <div style="padding-right:16px;font-size:12px;color:var(--text3)">284 games</div>
            </div>

            <div class="match-item win">
              <div class="match-stripe"></div>
              <div class="champ-icon">🧙</div>
              <div class="match-info">
                <div class="match-top">
                  <span class="match-champ">Stormcaller</span>
                  <span class="badge badge-win">Win</span>
                  <span class="badge badge-neutral">Mid</span>
                  <span class="badge badge-accent">Ranked</span>
                </div>
                <div class="match-kda">9 / 2 / 14 &nbsp;·&nbsp; 234 CS &nbsp;·&nbsp; 31k dmg</div>
              </div>
              <div class="match-right">
                <div class="match-ratio" style="color:var(--gold)">12.5</div>
                <div class="match-dur">28:44</div>
              </div>
            </div>

            <div class="match-item loss">
              <div class="match-stripe"></div>
              <div class="champ-icon">⚔️</div>
              <div class="match-info">
                <div class="match-top">
                  <span class="match-champ">Ironclad</span>
                  <span class="badge badge-loss">Loss</span>
                  <span class="badge badge-neutral">Top</span>
                  <span class="badge badge-accent">Ranked</span>
                </div>
                <div class="match-kda">3 / 8 / 5 &nbsp;·&nbsp; 181 CS &nbsp;·&nbsp; 14k dmg</div>
              </div>
              <div class="match-right">
                <div class="match-ratio" style="color:var(--loss)">1.0</div>
                <div class="match-dur">36:12</div>
              </div>
            </div>

            <div class="match-item win">
              <div class="match-stripe"></div>
              <div class="champ-icon">🌙</div>
              <div class="match-info">
                <div class="match-top">
                  <span class="match-champ">Voidwhisper</span>
                  <span class="badge badge-win">Win</span>
                  <span class="badge badge-neutral">Mid</span>
                  <span class="badge badge-accent">Ranked</span>
                </div>
                <div class="match-kda">7 / 3 / 18 &nbsp;·&nbsp; 268 CS &nbsp;·&nbsp; 28k dmg</div>
              </div>
              <div class="match-right">
                <div class="match-ratio" style="color:var(--win)">8.3</div>
                <div class="match-dur">22:19</div>
              </div>
            </div>

            <div class="match-item win">
              <div class="match-stripe"></div>
              <div class="champ-icon">🔥</div>
              <div class="match-info">
                <div class="match-top">
                  <span class="match-champ">Emberlord</span>
                  <span class="badge badge-win">Win</span>
                  <span class="badge badge-neutral">Bot</span>
                  <span class="badge badge-neutral">Normal</span>
                </div>
                <div class="match-kda">12 / 4 / 7 &nbsp;·&nbsp; 302 CS &nbsp;·&nbsp; 38k dmg</div>
              </div>
              <div class="match-right">
                <div class="match-ratio" style="color:var(--text)">4.75</div>
                <div class="match-dur">31:05</div>
              </div>
            </div>

            <div class="match-item loss">
              <div class="match-stripe"></div>
              <div class="champ-icon">❄️</div>
              <div class="match-info">
                <div class="match-top">
                  <span class="match-champ">Frostbite</span>
                  <span class="badge badge-loss">Loss</span>
                  <span class="badge badge-neutral">Support</span>
                  <span class="badge badge-accent">Ranked</span>
                </div>
                <div class="match-kda">2 / 6 / 9 &nbsp;·&nbsp; 193 CS &nbsp;·&nbsp; 19k dmg</div>
              </div>
              <div class="match-right">
                <div class="match-ratio" style="color:var(--text2)">1.83</div>
                <div class="match-dur">41:27</div>
              </div>
            </div>

            <div class="match-item win">
              <div class="match-stripe"></div>
              <div class="champ-icon">🧙</div>
              <div class="match-info">
                <div class="match-top">
                  <span class="match-champ">Stormcaller</span>
                  <span class="badge badge-win">Win</span>
                  <span class="badge badge-neutral">Mid</span>
                  <span class="badge badge-accent">Ranked</span>
                </div>
                <div class="match-kda">6 / 1 / 21 &nbsp;·&nbsp; 289 CS &nbsp;·&nbsp; 34k dmg</div>
              </div>
              <div class="match-right">
                <div class="match-ratio" style="color:var(--gold)">27.0</div>
                <div class="match-dur">19:48</div>
              </div>
            </div>
          </div>

          <!-- CHART -->
          <div class="card">
            <div class="card-header">
              <div class="card-title">7-Day Performance</div>
              <div class="card-sub">163W · 121L</div>
            </div>
            <div class="chart-area">
              <div class="bar-chart" id="barChart"></div>
              <div class="chart-legend">
                <div class="legend-item"><div class="legend-dot" style="background:var(--win)"></div>Wins</div>
                <div class="legend-item"><div class="legend-dot" style="background:#fca5a5"></div>Losses</div>
              </div>
            </div>
            <div class="role-row">
              <div class="role-chip active"><div class="role-emoji">✨</div><div class="role-name">Mid</div><div class="role-pct">52%</div></div>
              <div class="role-chip"><div class="role-emoji">🏹</div><div class="role-name">Bot</div><div class="role-pct">21%</div></div>
              <div class="role-chip"><div class="role-emoji">🗡️</div><div class="role-name">Top</div><div class="role-pct">12%</div></div>
              <div class="role-chip"><div class="role-emoji">🌿</div><div class="role-name">Jungle</div><div class="role-pct">8%</div></div>
              <div class="role-chip"><div class="role-emoji">🛡️</div><div class="role-name">Support</div><div class="role-pct">7%</div></div>
            </div>
          </div>

        </div>

        <!-- RIGHT -->
        <div class="right-col">

          <!-- LIVE MATCH -->
          <div class="card">
            <div class="card-header">
              <div class="card-title">Live Match</div>
              <div class="live-header"><div class="live-dot"></div><div class="live-label">Live</div></div>
            </div>
            <div class="live-timer-block">
              <div class="live-time" id="liveTimer">18:43</div>
              <div class="live-sub">Ranked Solo · Mid Lane</div>
            </div>
            <div class="live-grid">
              <div class="live-cell">
                <div class="live-num" style="color:var(--win)">7</div>
                <div class="live-lbl">Kills</div>
              </div>
              <div class="live-cell">
                <div class="live-num" style="color:var(--loss)">2</div>
                <div class="live-lbl">Deaths</div>
              </div>
              <div class="live-cell">
                <div class="live-num" style="color:var(--accent)">14</div>
                <div class="live-lbl">Assists</div>
              </div>
              <div class="live-cell">
                <div class="live-num" style="color:var(--gold)">189</div>
                <div class="live-lbl">CS</div>
              </div>
            </div>
            <div class="compare-block">
              <div class="compare-row">
                <div class="compare-meta">
                  <span style="color:var(--accent)">Blue 34.2k</span>
                  <span style="color:var(--text3)">Team Gold</span>
                  <span style="color:var(--text2)">28.8k Red</span>
                </div>
                <div style="display:flex;align-items:center;gap:4px">
                  <div class="compare-track">
                    <div class="compare-fill" style="width:54%;background:var(--accent)"></div>
                  </div>
                </div>
              </div>
              <div class="compare-row">
                <div class="compare-meta">
                  <span style="color:var(--accent)">Blue 22</span>
                  <span style="color:var(--text3)">Team Kills</span>
                  <span style="color:var(--text2)">16 Red</span>
                </div>
                <div style="display:flex;align-items:center;gap:4px">
                  <div class="compare-track">
                    <div class="compare-fill" style="width:58%;background:var(--accent)"></div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- CHAMPION MASTERY -->
          <div class="card">
            <div class="card-header">
              <div class="card-title">Champion Mastery</div>
              <button class="btn btn-outline" style="padding:4px 10px;font-size:12px">View all</button>
            </div>

            <div class="champ-row">
              <div class="champ-num">01</div>
              <div class="champ-portrait">🧙</div>
              <div class="champ-info">
                <div class="champ-name">Stormcaller</div>
                <div class="champ-games">68 games · Mid</div>
              </div>
              <div class="champ-metrics">
                <div class="metric"><div class="metric-val">4.2</div><div class="metric-lbl">KDA</div></div>
                <div class="metric"><div class="metric-val">8.1</div><div class="metric-lbl">CS/m</div></div>
              </div>
              <div class="wr-pill wr-good">57%</div>
            </div>

            <div class="champ-row">
              <div class="champ-num">02</div>
              <div class="champ-portrait">🌙</div>
              <div class="champ-info">
                <div class="champ-name">Voidwhisper</div>
                <div class="champ-games">41 games · Mid</div>
              </div>
              <div class="champ-metrics">
                <div class="metric"><div class="metric-val">5.1</div><div class="metric-lbl">KDA</div></div>
                <div class="metric"><div class="metric-val">9.3</div><div class="metric-lbl">CS/m</div></div>
              </div>
              <div class="wr-pill wr-good">70%</div>
            </div>

            <div class="champ-row">
              <div class="champ-num">03</div>
              <div class="champ-portrait">🔥</div>
              <div class="champ-info">
                <div class="champ-name">Emberlord</div>
                <div class="champ-games">33 games · Bot</div>
              </div>
              <div class="champ-metrics">
                <div class="metric"><div class="metric-val">3.5</div><div class="metric-lbl">KDA</div></div>
                <div class="metric"><div class="metric-val">8.8</div><div class="metric-lbl">CS/m</div></div>
              </div>
              <div class="wr-pill wr-warn">55%</div>
            </div>

            <div class="champ-row">
              <div class="champ-num">04</div>
              <div class="champ-portrait">❄️</div>
              <div class="champ-info">
                <div class="champ-name">Frostbite</div>
                <div class="champ-games">28 games · Support</div>
              </div>
              <div class="champ-metrics">
                <div class="metric"><div class="metric-val">2.8</div><div class="metric-lbl">KDA</div></div>
                <div class="metric"><div class="metric-val">2.1</div><div class="metric-lbl">CS/m</div></div>
              </div>
              <div class="wr-pill wr-bad">40%</div>
            </div>

            <div class="champ-row">
              <div class="champ-num">05</div>
              <div class="champ-portrait">⚡</div>
              <div class="champ-info">
                <div class="champ-name">Stormrazor</div>
                <div class="champ-games">19 games · Jungle</div>
              </div>
              <div class="champ-metrics">
                <div class="metric"><div class="metric-val">3.9</div><div class="metric-lbl">KDA</div></div>
                <div class="metric"><div class="metric-val">5.4</div><div class="metric-lbl">CS/m</div></div>
              </div>
              <div class="wr-pill wr-good">63%</div>
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>
</div>

<script>
  // Bar chart
  const days = [
    {d:'MON',w:3,l:2},{d:'TUE',w:5,l:1},{d:'WED',w:2,l:4},
    {d:'THU',w:4,l:3},{d:'FRI',w:6,l:2},{d:'SAT',w:7,l:1},{d:'SUN',w:3,l:0}
  ];
  const maxT = Math.max(...days.map(d=>d.w+d.l));
  const chart = document.getElementById('barChart');
  days.forEach(d => {
    const g = document.createElement('div');
    g.className = 'bar-grp';
    const wH = Math.round((d.w/maxT)*70);
    const lH = Math.round((d.l/maxT)*70);
    g.innerHTML = `<div class="bar-grp-bars">
      <div class="bar-seg bar-w" style="height:${wH}px" title="${d.w}W"></div>
      <div class="bar-seg bar-l" style="height:${lH}px" title="${d.l}L"></div>
    </div><div class="bar-day">${d.d}</div>`;
    chart.appendChild(g);
  });

  // Live timer
  let s = 18*60+43;
  setInterval(()=>{
    s++;
    document.getElementById('liveTimer').textContent =
      `${String(Math.floor(s/60)).padStart(2,'0')}:${String(s%60).padStart(2,'0')}`;
  },1000);

  // Tabs
  document.querySelectorAll('.tab').forEach(t=>{
    t.addEventListener('click',()=>{
      t.closest('.tabs').querySelectorAll('.tab').forEach(x=>x.classList.remove('active'));
      t.classList.add('active');
    });
  });

  // Nav
  document.querySelectorAll('.nav-item').forEach(n=>{
    n.addEventListener('click',()=>{
      document.querySelectorAll('.nav-item').forEach(x=>x.classList.remove('active'));
      n.classList.add('active');
    });
  });

  // Roles
  document.querySelectorAll('.role-chip').forEach(r=>{
    r.addEventListener('click',()=>{
      document.querySelectorAll('.role-chip').forEach(x=>x.classList.remove('active'));
      r.classList.add('active');
    });
  });
</script>
</body>
</html>
