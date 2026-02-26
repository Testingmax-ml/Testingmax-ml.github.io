
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Nexus Stats — MOBA Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,400&family=DM+Mono:wght@400;500&family=Sora:wght@600;700;800&display=swap" rel="stylesheet">
<style>
/* ===================== RESET & BASE ===================== */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --bg:         #f5f5f4;
  --surface:    #ffffff;
  --surface2:   #fafaf9;
  --border:     #e7e5e4;
  --border2:    #d6d3d1;
  --text:       #1c1917;
  --text2:      #78716c;
  --text3:      #a8a29e;
  --accent:     #2563eb;
  --accent-h:   #1d4ed8;
  --accent-l:   #eff6ff;
  --win:        #16a34a;
  --win-l:      #f0fdf4;
  --loss:       #dc2626;
  --loss-l:     #fef2f2;
  --gold:       #d97706;
  --gold-l:     #fffbeb;
  --r:          12px;
  --r-sm:       8px;
  --shadow:     0 1px 3px rgba(0,0,0,0.07), 0 1px 2px rgba(0,0,0,0.04);
  --shadow-md:  0 4px 16px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.04);
}
html { scroll-behavior: smooth; }
body {
  font-family: 'DM Sans', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  font-size: 14px;
  line-height: 1.5;
}
a { text-decoration: none; color: inherit; }
button { cursor: pointer; font-family: inherit; }
::-webkit-scrollbar { width: 5px; height: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 10px; }

/* ===================== LAYOUT ===================== */
.layout { display: flex; min-height: 100vh; }

/* ===================== SIDEBAR ===================== */
.sidebar {
  width: 224px;
  background: var(--surface);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0; bottom: 0; left: 0;
  z-index: 100;
  overflow-y: auto;
}
.sidebar-top { padding: 20px 20px 16px; border-bottom: 1px solid var(--border); }
.logo {
  font-family: 'Sora', sans-serif;
  font-size: 19px; font-weight: 800;
  color: var(--text); letter-spacing: -0.5px;
  display: flex; align-items: center; gap: 6px;
}
.logo-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--accent); flex-shrink: 0;
}
.logo-sub { font-size: 11px; color: var(--text3); font-weight: 400; margin-top: 1px; letter-spacing: 0.02em; }

.nav-body { padding: 12px 12px; flex: 1; }
.nav-group { margin-bottom: 20px; }
.nav-group-label {
  font-size: 10px; font-weight: 700; letter-spacing: 0.1em;
  text-transform: uppercase; color: var(--text3);
  padding: 0 8px; margin-bottom: 4px;
}
.nav-item {
  display: flex; align-items: center; gap: 10px;
  padding: 8px 10px; border-radius: var(--r-sm);
  color: var(--text2); font-weight: 500; font-size: 13.5px;
  cursor: pointer; transition: all 0.15s; margin-bottom: 2px;
  user-select: none;
}
.nav-item:hover { background: var(--bg); color: var(--text); }
.nav-item.active { background: var(--accent-l); color: var(--accent); font-weight: 600; }
.nav-icon { font-size: 15px; width: 18px; text-align: center; flex-shrink: 0; }
.nav-badge {
  margin-left: auto; background: var(--accent); color: #fff;
  font-size: 10px; font-weight: 700; padding: 1px 6px; border-radius: 100px;
}

.sidebar-footer {
  padding: 14px 12px; border-top: 1px solid var(--border);
}
.user-tile {
  display: flex; align-items: center; gap: 10px;
  padding: 8px 8px; border-radius: var(--r-sm);
  cursor: pointer; transition: background 0.15s;
}
.user-tile:hover { background: var(--bg); }
.user-avatar {
  width: 34px; height: 34px; border-radius: 50%;
  background: linear-gradient(135deg, #bfdbfe, #60a5fa);
  display: flex; align-items: center; justify-content: center;
  font-size: 16px; flex-shrink: 0;
  border: 2px solid var(--surface);
  box-shadow: 0 0 0 1.5px var(--border2);
}
.user-name { font-weight: 600; font-size: 13px; }
.user-meta { font-size: 11px; color: var(--text3); }

/* ===================== MAIN AREA ===================== */
.main { margin-left: 224px; flex: 1; display: flex; flex-direction: column; min-width: 0; }

.topbar {
  background: var(--surface); border-bottom: 1px solid var(--border);
  padding: 0 28px; height: 58px;
  display: flex; align-items: center; justify-content: space-between;
  position: sticky; top: 0; z-index: 50;
}
.page-heading { font-family: 'Sora', sans-serif; font-size: 15px; font-weight: 700; letter-spacing: -0.3px; }
.topbar-actions { display: flex; align-items: center; gap: 10px; }

.search-wrap {
  display: flex; align-items: center; gap: 8px;
  background: var(--bg); border: 1px solid var(--border2);
  border-radius: var(--r-sm); padding: 7px 12px;
  transition: border-color 0.15s;
}
.search-wrap:focus-within { border-color: var(--accent); background: var(--surface); }
.search-wrap input {
  background: none; border: none; outline: none;
  font-family: 'DM Sans', sans-serif; font-size: 13px; color: var(--text); width: 170px;
}
.search-wrap input::placeholder { color: var(--text3); }

.btn {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 7px 14px; border-radius: var(--r-sm);
  font-size: 13px; font-weight: 500; border: none;
  transition: all 0.15s; white-space: nowrap;
}
.btn-primary { background: var(--accent); color: #fff; }
.btn-primary:hover { background: var(--accent-h); }
.btn-ghost { background: none; border: 1px solid var(--border2); color: var(--text2); }
.btn-ghost:hover { background: var(--bg); color: var(--text); }
.btn-sm { padding: 5px 11px; font-size: 12px; }

/* ===================== PAGE CONTAINER ===================== */
.page { display: none; padding: 24px 28px 40px; animation: fadeUp 0.35s ease both; }
.page.active { display: block; }
@keyframes fadeUp { from { opacity:0; transform:translateY(6px); } to { opacity:1; transform:translateY(0); } }

/* ===================== SHARED COMPONENTS ===================== */

/* Card */
.card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); box-shadow: var(--shadow); overflow: hidden;
}
.card-header {
  padding: 14px 20px; border-bottom: 1px solid var(--border);
  display: flex; align-items: center; justify-content: space-between; gap: 12px;
}
.card-title { font-size: 13.5px; font-weight: 600; }
.card-sub { font-size: 12px; color: var(--text3); }

/* Badges */
.badge {
  display: inline-flex; align-items: center; gap: 3px;
  padding: 2px 7px; border-radius: 100px; font-size: 11px; font-weight: 600;
}
.b-win   { background: var(--win-l);  color: var(--win);  }
.b-loss  { background: var(--loss-l); color: var(--loss); }
.b-gold  { background: var(--gold-l); color: var(--gold); }
.b-blue  { background: var(--accent-l); color: var(--accent); }
.b-gray  { background: var(--bg); color: var(--text2); border: 1px solid var(--border); }

/* WR pill */
.wr { display: inline-flex; align-items: center; border-radius: 100px; padding: 3px 10px; font-family: 'DM Mono', monospace; font-size: 12px; font-weight: 500; white-space: nowrap; }
.wr-g  { background: var(--win-l);  color: var(--win);  border: 1px solid #bbf7d0; }
.wr-w  { background: var(--gold-l); color: var(--gold); border: 1px solid #fde68a; }
.wr-b  { background: var(--loss-l); color: var(--loss); border: 1px solid #fecaca; }

/* Tabs */
.tabs { display: flex; }
.tab {
  padding: 10px 14px; font-size: 13px; font-weight: 500; color: var(--text2);
  cursor: pointer; border-bottom: 2px solid transparent; transition: all 0.15s;
  white-space: nowrap;
}
.tab:hover { color: var(--text); }
.tab.active { color: var(--accent); border-bottom-color: var(--accent); font-weight: 600; }

/* Section header */
.section-head { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.section-title { font-family: 'Sora', sans-serif; font-size: 15px; font-weight: 700; letter-spacing: -0.3px; }

/* Grid helpers */
.g2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
.g3 { display: grid; grid-template-columns: repeat(3,1fr); gap: 16px; }
.g4 { display: grid; grid-template-columns: repeat(4,1fr); gap: 16px; }
.gap16 { gap: 16px; }
.mb16 { margin-bottom: 16px; }
.mb20 { margin-bottom: 20px; }
.flex-col { display: flex; flex-direction: column; gap: 16px; }
.main-split { display: grid; grid-template-columns: 1fr 336px; gap: 16px; }

/* Divider */
.divider { height: 1px; background: var(--border); }

/* ===================== PROFILE HEADER ===================== */
.profile-bar {
  background: var(--surface); border: 1px solid var(--border); border-radius: var(--r);
  padding: 22px 24px; display: flex; align-items: center; gap: 20px;
  box-shadow: var(--shadow); margin-bottom: 20px;
}
.pa {
  width: 62px; height: 62px; border-radius: 50%;
  background: linear-gradient(135deg, #dbeafe, #93c5fd);
  display: flex; align-items: center; justify-content: center;
  font-size: 27px; flex-shrink: 0;
  border: 3px solid var(--surface);
  box-shadow: 0 0 0 2px var(--border2);
}
.pname { font-family: 'Sora', sans-serif; font-size: 19px; font-weight: 700; letter-spacing: -0.4px; }
.pmeta { font-size: 12.5px; color: var(--text2); display: flex; gap: 10px; margin-top: 2px; }
.pmeta-dot { color: var(--border2); }
.pstats {
  display: flex; gap: 1px; background: var(--border);
  border-radius: var(--r-sm); overflow: hidden; margin-left: auto;
}
.pstat {
  background: var(--surface); padding: 12px 18px; text-align: center;
  min-width: 90px; transition: background 0.12s; cursor: default;
}
.pstat:hover { background: var(--surface2); }
.pstat-val { font-family: 'Sora', sans-serif; font-size: 17px; font-weight: 700; letter-spacing: -0.4px; }
.pstat-lbl { font-size: 10.5px; color: var(--text3); font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; margin-top: 1px; }
.rank-tag {
  display: flex; align-items: center; gap: 8px;
  background: var(--gold-l); border: 1px solid #fde68a;
  border-radius: 100px; padding: 7px 16px; white-space: nowrap; flex-shrink: 0;
}
.rank-tag-icon { font-size: 20px; }
.rank-tag-name { font-size: 13px; font-weight: 700; color: var(--gold); }
.rank-tag-lp { font-size: 11.5px; color: #92400e; font-family: 'DM Mono', monospace; }

/* ===================== KPI CARDS ===================== */
.kpi { padding: 20px; }
.kpi-lbl { font-size: 11px; color: var(--text3); font-weight: 600; text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 8px; display: flex; align-items: center; gap: 5px; }
.kpi-val { font-family: 'Sora', sans-serif; font-size: 26px; font-weight: 700; letter-spacing: -0.8px; line-height: 1; margin-bottom: 7px; }
.kpi-sub-unit { font-size: 15px; color: var(--text3); font-weight: 400; }
.kpi-change { font-size: 12px; font-weight: 600; }
.up { color: var(--win); } .down { color: var(--loss); }
.mini-bar { height: 3px; background: var(--border); border-radius: 100px; overflow: hidden; margin-top: 10px; }
.mini-fill { height: 100%; border-radius: 100px; }

/* ===================== MATCH LIST ===================== */
.match {
  display: flex; align-items: center; gap: 13px;
  padding: 11px 20px; border-bottom: 1px solid var(--border);
  cursor: pointer; transition: background 0.12s;
}
.match:last-child { border-bottom: none; }
.match:hover { background: var(--surface2); }
.m-stripe { width: 3px; border-radius: 100px; align-self: stretch; flex-shrink: 0; }
.match.win .m-stripe { background: var(--win); }
.match.loss .m-stripe { background: var(--loss); }
.m-icon {
  width: 40px; height: 40px; border-radius: var(--r-sm);
  background: var(--bg); border: 1px solid var(--border);
  display: flex; align-items: center; justify-content: center;
  font-size: 19px; flex-shrink: 0;
}
.m-info { flex: 1; min-width: 0; }
.m-top { display: flex; align-items: center; gap: 6px; margin-bottom: 2px; flex-wrap: wrap; }
.m-champ { font-weight: 600; font-size: 13.5px; }
.m-kda { font-family: 'DM Mono', monospace; font-size: 12px; color: var(--text2); }
.m-right { text-align: right; flex-shrink: 0; }
.m-ratio { font-family: 'Sora', sans-serif; font-size: 15px; font-weight: 700; letter-spacing: -0.3px; }
.m-dur { font-size: 11px; color: var(--text3); font-family: 'DM Mono', monospace; }

/* ===================== BAR CHART ===================== */
.bar-chart { display: flex; align-items: flex-end; gap: 5px; }
.bcol { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 4px; }
.bcol-bars { display: flex; gap: 2px; align-items: flex-end; width: 100%; }
.bseg { flex: 1; border-radius: 3px 3px 0 0; cursor: pointer; transition: opacity 0.15s; }
.bseg:hover { opacity: 0.7; }
.bw { background: var(--win); }
.bl { background: #fca5a5; }
.bday { font-size: 9px; color: var(--text3); font-weight: 600; font-family: 'DM Mono', monospace; letter-spacing: 0.04em; }
.chart-legend { display: flex; gap: 14px; }
.leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); }
.leg-dot { width: 8px; height: 8px; border-radius: 2px; }

/* ===================== ROLE CHIPS ===================== */
.role-row { display: flex; gap: 7px; padding: 12px 20px; border-top: 1px solid var(--border); }
.role-chip {
  flex: 1; padding: 9px 5px; border-radius: var(--r-sm);
  border: 1px solid var(--border); background: var(--surface2);
  text-align: center; cursor: pointer; transition: all 0.15s;
}
.role-chip:hover { border-color: var(--accent); background: var(--accent-l); }
.role-chip.active { border-color: var(--accent); background: var(--accent-l); }
.rc-icon { font-size: 17px; margin-bottom: 3px; }
.rc-name { font-size: 9px; color: var(--text3); font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; }
.rc-pct { font-size: 12px; font-weight: 700; color: var(--text); margin-top: 1px; }
.role-chip.active .rc-pct { color: var(--accent); }

/* ===================== LIVE MATCH ===================== */
.live-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--loss); animation: blink 1.2s infinite; }
@keyframes blink { 0%,100% { opacity:1; } 50% { opacity:0.15; } }
.live-lbl { font-size: 10.5px; font-weight: 700; color: var(--loss); letter-spacing: 0.07em; text-transform: uppercase; }
.live-timer-block { text-align: center; padding: 18px 20px; border-bottom: 1px solid var(--border); }
.live-time { font-family: 'Sora', sans-serif; font-size: 42px; font-weight: 800; letter-spacing: -2px; line-height: 1; margin-bottom: 3px; }
.live-sub { font-size: 11px; color: var(--text3); font-weight: 600; letter-spacing: 0.07em; text-transform: uppercase; }
.live-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1px; background: var(--border); }
.live-cell { background: var(--surface); padding: 13px 14px; text-align: center; transition: background 0.12s; }
.live-cell:hover { background: var(--surface2); }
.live-num { font-family: 'Sora', sans-serif; font-size: 22px; font-weight: 700; letter-spacing: -0.5px; line-height: 1; margin-bottom: 2px; }
.live-lbl2 { font-size: 9.5px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.08em; font-weight: 600; }
.cmp-block { padding: 14px 20px; border-top: 1px solid var(--border); }
.cmp-row { margin-bottom: 10px; }
.cmp-row:last-child { margin-bottom: 0; }
.cmp-meta { display: flex; justify-content: space-between; font-size: 11px; font-weight: 600; margin-bottom: 5px; }
.cmp-track { height: 5px; background: var(--border); border-radius: 100px; overflow: hidden; }
.cmp-fill { height: 100%; border-radius: 100px; }

/* ===================== CHAMPION ROWS ===================== */
.champ-row {
  display: flex; align-items: center; gap: 11px;
  padding: 10px 20px; border-bottom: 1px solid var(--border);
  cursor: pointer; transition: background 0.12s;
}
.champ-row:last-child { border-bottom: none; }
.champ-row:hover { background: var(--surface2); }
.cr-num { width: 20px; font-size: 11px; color: var(--text3); font-weight: 600; font-family: 'DM Mono', monospace; flex-shrink: 0; }
.cr-portrait {
  width: 36px; height: 36px; border-radius: 50%;
  background: var(--bg); border: 1.5px solid var(--border);
  display: flex; align-items: center; justify-content: center; font-size: 17px; flex-shrink: 0;
}
.cr-info { flex: 1; min-width: 0; }
.cr-name { font-weight: 600; font-size: 13.5px; }
.cr-games { font-size: 11px; color: var(--text3); }
.cr-stats { display: flex; gap: 14px; flex-shrink: 0; }
.cr-s { text-align: right; }
.cr-sv { font-size: 13px; font-weight: 600; font-family: 'DM Mono', monospace; }
.cr-sl { font-size: 9.5px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.05em; }

/* ===================== LEADERBOARD ===================== */
.lb-row {
  display: flex; align-items: center; gap: 14px;
  padding: 12px 20px; border-bottom: 1px solid var(--border);
  cursor: pointer; transition: background 0.12s;
}
.lb-row:last-child { border-bottom: none; }
.lb-row:hover { background: var(--surface2); }
.lb-rank {
  width: 28px; text-align: center; font-family: 'Sora', sans-serif;
  font-size: 14px; font-weight: 700; flex-shrink: 0;
}
.lb-rank.top1 { color: #f59e0b; }
.lb-rank.top2 { color: #94a3b8; }
.lb-rank.top3 { color: #b45309; }
.lb-av {
  width: 36px; height: 36px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center; font-size: 17px;
  flex-shrink: 0; border: 1.5px solid var(--border);
}
.lb-info { flex: 1; min-width: 0; }
.lb-name { font-weight: 600; font-size: 13.5px; }
.lb-server { font-size: 11px; color: var(--text3); }
.lb-stats { display: flex; gap: 20px; flex-shrink: 0; }
.lb-s { text-align: right; }
.lb-sv { font-size: 13.5px; font-weight: 600; font-family: 'DM Mono', monospace; }
.lb-sl { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.05em; }
.tier-badge {
  padding: 3px 10px; border-radius: 100px; font-size: 11px; font-weight: 700;
  font-family: 'DM Mono', monospace; white-space: nowrap;
}
.tier-chall { background: #fefce8; color: #92400e; border: 1px solid #fde68a; }
.tier-gm    { background: #fff7ed; color: #c2410c; border: 1px solid #fed7aa; }
.tier-master{ background: #fdf4ff; color: #7e22ce; border: 1px solid #e9d5ff; }
.tier-diamond{background: var(--accent-l); color: var(--accent); border: 1px solid #bfdbfe; }
.tier-plat  { background: #f0fdfa; color: #0f766e; border: 1px solid #99f6e4; }
.tier-gold  { background: var(--gold-l); color: var(--gold); border: 1px solid #fde68a; }

/* ===================== STAT TABLE ===================== */
.stat-table { width: 100%; border-collapse: collapse; }
.stat-table th, .stat-table td { padding: 10px 20px; text-align: left; font-size: 13px; }
.stat-table th { font-size: 10.5px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.07em; color: var(--text3); border-bottom: 1px solid var(--border); background: var(--surface2); }
.stat-table td { border-bottom: 1px solid var(--border); }
.stat-table tr:last-child td { border-bottom: none; }
.stat-table tr:hover td { background: var(--surface2); cursor: pointer; }
.stat-table td:first-child { font-weight: 600; }

/* ===================== PROGRESS BARS ===================== */
.prog-row { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
.prog-row:last-child { margin-bottom: 0; }
.prog-label { font-size: 12.5px; font-weight: 500; min-width: 90px; }
.prog-track { flex: 1; height: 6px; background: var(--border); border-radius: 100px; overflow: hidden; }
.prog-fill { height: 100%; border-radius: 100px; transition: width 1s ease; }
.prog-val { font-size: 12px; font-family: 'DM Mono', monospace; font-weight: 500; min-width: 36px; text-align: right; color: var(--text2); }

/* ===================== OVERVIEW EXTRA ===================== */
.recent-champ-row {
  display: flex; align-items: center; gap: 9px;
  padding: 9px 20px; border-bottom: 1px solid var(--border);
}
.recent-champ-row:last-child { border-bottom: none; }
.rcs { flex: 1; }
.rcs-name { font-weight: 600; font-size: 13px; }
.rcs-detail { font-size: 11px; color: var(--text3); font-family: 'DM Mono', monospace; }
.rcs-bar { height: 4px; background: var(--border); border-radius: 100px; overflow: hidden; margin-top: 5px; }
.rcs-fill { height: 100%; border-radius: 100px; }

/* ===================== FILTER BAR ===================== */
.filter-bar {
  display: flex; align-items: center; gap: 8px;
  padding: 12px 20px; border-bottom: 1px solid var(--border);
  flex-wrap: wrap;
}
.filter-pill {
  padding: 5px 12px; border-radius: 100px; font-size: 12px; font-weight: 500;
  border: 1px solid var(--border2); background: var(--surface); color: var(--text2);
  cursor: pointer; transition: all 0.15s;
}
.filter-pill:hover { border-color: var(--accent); color: var(--accent); }
.filter-pill.active { background: var(--accent); color: #fff; border-color: var(--accent); }
.filter-label { font-size: 12px; color: var(--text3); font-weight: 500; margin-right: 2px; }

/* ===================== SCROLLABLE CONTENT ===================== */
.scroll-x { overflow-x: auto; }

/* ===================== EMPTY / LOADING ===================== */
.empty-state { padding: 40px; text-align: center; color: var(--text3); }
.empty-icon { font-size: 32px; margin-bottom: 8px; }
.empty-text { font-size: 14px; }

/* ===================== STAT SUMMARY GRID ===================== */
.sum-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 1px; background: var(--border); }
.sum-cell { background: var(--surface); padding: 16px; text-align: center; transition: background 0.12s; }
.sum-cell:hover { background: var(--surface2); cursor: default; }
.sum-val { font-family: 'Sora', sans-serif; font-size: 20px; font-weight: 700; letter-spacing: -0.5px; margin-bottom: 2px; }
.sum-lbl { font-size: 10.5px; color: var(--text3); font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; }

/* Hover card elevation */
.card-hover { transition: box-shadow 0.2s, transform 0.2s; }
.card-hover:hover { box-shadow: var(--shadow-md); transform: translateY(-1px); }

/* Tooltip-style number highlight */
.hi-blue { color: var(--accent); }
.hi-green { color: var(--win); }
.hi-gold  { color: var(--gold); }
.hi-red   { color: var(--loss); }
.hi-dim   { color: var(--text3); font-size: 0.85em; }
</style>
</head>
<body>
<div class="layout">

<!-- ═══════════════════════ SIDEBAR ═══════════════════════ -->
<aside class="sidebar">
  <div class="sidebar-top">
    <div class="logo"><span>nexus</span><span class="logo-dot"></span></div>
    <div class="logo-sub">MOBA Stats Tracker</div>
  </div>
  <nav class="nav-body">
    <div class="nav-group">
      <div class="nav-group-label">Dashboard</div>
      <div class="nav-item active" data-page="overview">
        <span class="nav-icon">📊</span> Overview
      </div>
      <div class="nav-item" data-page="champions">
        <span class="nav-icon">🗡️</span> Champions
      </div>
      <div class="nav-item" data-page="matches">
        <span class="nav-icon">📋</span> Match History
      </div>
      <div class="nav-item" data-page="performance">
        <span class="nav-icon">📈</span> Performance
      </div>
    </div>
    <div class="nav-group">
      <div class="nav-group-label">Community</div>
      <div class="nav-item" data-page="leaderboard">
        <span class="nav-icon">🏆</span> Leaderboard
      </div>
      <div class="nav-item" data-page="friends">
        <span class="nav-icon">👥</span> Friends <span class="nav-badge">3</span>
      </div>
    </div>
  </nav>
  <div class="sidebar-footer">
    <div class="user-tile">
      <div class="user-avatar">⚡</div>
      <div>
        <div class="user-name">NightHawk_X</div>
        <div class="user-meta">Gold I · EUW</div>
      </div>
    </div>
  </div>
</aside>

<!-- ═══════════════════════ MAIN ═══════════════════════ -->
<div class="main">
  <header class="topbar">
    <div class="page-heading" id="pageHeading">Overview</div>
    <div class="topbar-actions">
      <div class="search-wrap">
        <span>🔍</span>
        <input type="text" placeholder="Search summoner…" id="searchInput">
      </div>
      <button class="btn btn-ghost">Season 2025</button>
      <button class="btn btn-primary" id="refreshBtn">↺ Refresh</button>
    </div>
  </header>

  <!-- ════════════ PAGE: OVERVIEW ════════════ -->
  <div class="page active" id="page-overview">

    <!-- Profile bar -->
    <div class="profile-bar">
      <div class="pa">⚡</div>
      <div>
        <div class="pname">NightHawk_X</div>
        <div class="pmeta">
          <span>EUW Server</span><span class="pmeta-dot">·</span>
          <span>Level 247</span><span class="pmeta-dot">·</span>
          <span style="color:var(--text3)">Active 12 min ago</span>
        </div>
      </div>
      <div class="pstats">
        <div class="pstat"><div class="pstat-val">57.3<span class="hi-dim">%</span></div><div class="pstat-lbl">Win Rate</div></div>
        <div class="pstat"><div class="pstat-val">3.82</div><div class="pstat-lbl">KDA</div></div>
        <div class="pstat"><div class="pstat-val">284</div><div class="pstat-lbl">Games</div></div>
        <div class="pstat"><div class="pstat-val">7.4</div><div class="pstat-lbl">CS/min</div></div>
      </div>
      <div class="rank-tag">
        <span class="rank-tag-icon">🏆</span>
        <span class="rank-tag-name">Gold I</span>
        <span class="rank-tag-lp">78 LP</span>
      </div>
    </div>

    <!-- KPI row -->
    <div class="g4 mb20">
      <div class="card card-hover">
        <div class="kpi">
          <div class="kpi-lbl">⚔️ Avg K/D/A</div>
          <div class="kpi-val">8 <span class="hi-dim">/ 3 / 11</span></div>
          <div class="kpi-change up">↑ +0.4 vs last week</div>
        </div>
      </div>
      <div class="card card-hover">
        <div class="kpi">
          <div class="kpi-lbl">💥 Avg Damage</div>
          <div class="kpi-val">22<span class="hi-dim">k</span></div>
          <div class="kpi-change up">↑ +1.2k vs last week</div>
          <div class="mini-bar"><div class="mini-fill" style="width:68%;background:var(--accent)"></div></div>
        </div>
      </div>
      <div class="card card-hover">
        <div class="kpi">
          <div class="kpi-lbl">⏱ Avg Duration</div>
          <div class="kpi-val">24<span class="hi-dim">:31</span></div>
          <div class="kpi-change down">↓ −2:12 vs last week</div>
        </div>
      </div>
      <div class="card card-hover">
        <div class="kpi">
          <div class="kpi-lbl">👁 Vision Score</div>
          <div class="kpi-val">28<span class="hi-dim">.1</span></div>
          <div class="kpi-change up">↑ +3.2 vs last week</div>
          <div class="mini-bar"><div class="mini-fill" style="width:44%;background:var(--win)"></div></div>
        </div>
      </div>
    </div>

    <!-- Main split -->
    <div class="main-split">
      <div class="flex-col">

        <!-- Recent matches -->
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border);padding-right:16px">
            <div class="tabs" style="border-bottom:none" id="tabs-overview">
              <div class="tab active">All</div>
              <div class="tab">Ranked</div>
              <div class="tab">Normal</div>
            </div>
            <span style="font-size:12px;color:var(--text3)">284 games</span>
          </div>
          <div id="overview-matches"></div>
        </div>

        <!-- 7 day chart -->
        <div class="card">
          <div class="card-header">
            <div class="card-title">7-Day Performance</div>
            <div class="chart-legend">
              <div class="leg"><div class="leg-dot" style="background:var(--win)"></div>Wins</div>
              <div class="leg"><div class="leg-dot" style="background:#fca5a5"></div>Losses</div>
            </div>
          </div>
          <div style="padding:20px">
            <div class="bar-chart" style="height:90px" id="perf-chart"></div>
          </div>
          <div class="role-row" id="role-chips"></div>
        </div>

      </div>

      <!-- Right col -->
      <div class="flex-col">

        <!-- Live match -->
        <div class="card">
          <div class="card-header">
            <div class="card-title">Live Match</div>
            <div style="display:flex;align-items:center;gap:6px">
              <div class="live-dot"></div>
              <div class="live-lbl">Live</div>
            </div>
          </div>
          <div class="live-timer-block">
            <div class="live-time" id="live-clock">18:43</div>
            <div class="live-sub">Ranked Solo · Mid Lane</div>
          </div>
          <div class="live-grid">
            <div class="live-cell"><div class="live-num hi-green">7</div><div class="live-lbl2">Kills</div></div>
            <div class="live-cell"><div class="live-num hi-red">2</div><div class="live-lbl2">Deaths</div></div>
            <div class="live-cell"><div class="live-num hi-blue">14</div><div class="live-lbl2">Assists</div></div>
            <div class="live-cell"><div class="live-num hi-gold">189</div><div class="live-lbl2">CS</div></div>
          </div>
          <div class="cmp-block">
            <div class="cmp-row">
              <div class="cmp-meta"><span class="hi-blue">Blue 34.2k</span><span style="color:var(--text3)">Team Gold</span><span>28.8k Red</span></div>
              <div class="cmp-track"><div class="cmp-fill" style="width:54%;background:var(--accent)"></div></div>
            </div>
            <div class="cmp-row">
              <div class="cmp-meta"><span class="hi-blue">Blue 22</span><span style="color:var(--text3)">Kills</span><span>16 Red</span></div>
              <div class="cmp-track"><div class="cmp-fill" style="width:58%;background:var(--accent)"></div></div>
            </div>
          </div>
        </div>

        <!-- Top champs mini -->
        <div class="card">
          <div class="card-header">
            <div class="card-title">Top Champions</div>
            <button class="btn btn-ghost btn-sm" data-page="champions">View all →</button>
          </div>
          <div id="mini-champs"></div>
        </div>

      </div>
    </div>
  </div>

  <!-- ════════════ PAGE: CHAMPIONS ════════════ -->
  <div class="page" id="page-champions">
    <div class="section-head mb20">
      <div class="section-title">Champion Statistics</div>
      <div style="display:flex;gap:8px">
        <button class="btn btn-ghost btn-sm">Export CSV</button>
      </div>
    </div>

    <div class="g3 mb20">
      <div class="card card-hover">
        <div class="kpi">
          <div class="kpi-lbl">🥇 Best Champion</div>
          <div class="kpi-val" style="font-size:20px">Voidwhisper</div>
          <div class="kpi-change hi-green">70% win rate · 41 games</div>
        </div>
      </div>
      <div class="card card-hover">
        <div class="kpi">
          <div class="kpi-lbl">🎯 Most Played</div>
          <div class="kpi-val" style="font-size:20px">Stormcaller</div>
          <div class="kpi-change hi-blue">68 games · Mid lane</div>
        </div>
      </div>
      <div class="card card-hover">
        <div class="kpi">
          <div class="kpi-lbl">📉 Needs Work</div>
          <div class="kpi-val" style="font-size:20px">Frostbite</div>
          <div class="kpi-change hi-red">40% win rate · 28 games</div>
        </div>
      </div>
    </div>

    <div class="main-split">
      <div class="flex-col">
        <div class="card">
          <div class="card-header">
            <div class="card-title">All Champions</div>
            <div class="card-sub">19 champions played</div>
          </div>
          <div class="filter-bar">
            <span class="filter-label">Role:</span>
            <div class="filter-pill active" data-filter="role" data-val="all">All</div>
            <div class="filter-pill" data-filter="role" data-val="mid">Mid</div>
            <div class="filter-pill" data-filter="role" data-val="top">Top</div>
            <div class="filter-pill" data-filter="role" data-val="bot">Bot</div>
            <div class="filter-pill" data-filter="role" data-val="jungle">Jungle</div>
            <div class="filter-pill" data-filter="role" data-val="support">Support</div>
          </div>
          <div id="champ-list"></div>
        </div>
      </div>
      <div class="flex-col">
        <div class="card">
          <div class="card-header"><div class="card-title">Win Rate by Role</div></div>
          <div style="padding:20px">
            <div class="prog-row"><span class="prog-label">Mid Lane</span><div class="prog-track"><div class="prog-fill" style="width:61%;background:var(--accent)"></div></div><span class="prog-val hi-blue">61%</span></div>
            <div class="prog-row"><span class="prog-label">Bot Lane</span><div class="prog-track"><div class="prog-fill" style="width:55%;background:var(--win)"></div></div><span class="prog-val hi-green">55%</span></div>
            <div class="prog-row"><span class="prog-label">Top Lane</span><div class="prog-track"><div class="prog-fill" style="width:52%;background:var(--gold)"></div></div><span class="prog-val hi-gold">52%</span></div>
            <div class="prog-row"><span class="prog-label">Jungle</span><div class="prog-track"><div class="prog-fill" style="width:48%;background:var(--text3)"></div></div><span class="prog-val">48%</span></div>
            <div class="prog-row"><span class="prog-label">Support</span><div class="prog-track"><div class="prog-fill" style="width:44%;background:#fca5a5;"></div></div><span class="prog-val hi-red">44%</span></div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">Avg Stats Summary</div></div>
          <div class="sum-grid">
            <div class="sum-cell"><div class="sum-val">8.1</div><div class="sum-lbl">CS/min</div></div>
            <div class="sum-cell"><div class="sum-val">3.82</div><div class="sum-lbl">KDA</div></div>
            <div class="sum-cell"><div class="sum-val">22k</div><div class="sum-lbl">Damage</div></div>
            <div class="sum-cell"><div class="sum-val">28</div><div class="sum-lbl">Vision</div></div>
            <div class="sum-cell"><div class="sum-val">18%</div><div class="sum-lbl">Kill Part.</div></div>
            <div class="sum-cell"><div class="sum-val">57%</div><div class="sum-lbl">Win Rate</div></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ════════════ PAGE: MATCH HISTORY ════════════ -->
  <div class="page" id="page-matches">
    <div class="section-head mb20">
      <div class="section-title">Match History</div>
      <div style="display:flex;gap:8px">
        <button class="btn btn-ghost btn-sm">Filter</button>
        <button class="btn btn-ghost btn-sm">Export</button>
      </div>
    </div>

    <div class="g4 mb20">
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">📅 Last 20 Games</div>
        <div class="kpi-val">13<span class="hi-dim">W</span> <span style="font-size:16px;font-weight:400;color:var(--text3)">·</span> 7<span class="hi-dim">L</span></div>
        <div class="kpi-change hi-green">65% win rate</div>
      </div></div>
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">🔥 Longest Win Streak</div>
        <div class="kpi-val">7<span class="hi-dim"> games</span></div>
        <div class="kpi-change hi-blue">Current: 2W</div>
      </div></div>
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">⚔️ Best KDA Game</div>
        <div class="kpi-val">27<span class="hi-dim">.0</span></div>
        <div class="kpi-change hi-gold">6/1/21 Stormcaller</div>
      </div></div>
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">💀 Most Deaths</div>
        <div class="kpi-val">14<span class="hi-dim"> deaths</span></div>
        <div class="kpi-change hi-red">2/14/3 Ironclad</div>
      </div></div>
    </div>

    <div class="card">
      <div style="display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border)">
        <div class="tabs" style="border-bottom:none;padding:0 12px" id="tabs-matches">
          <div class="tab active">All</div>
          <div class="tab">Ranked Solo</div>
          <div class="tab">Normal</div>
          <div class="tab">ARAM</div>
        </div>
        <div style="padding:0 20px;font-size:12px;color:var(--text3)">284 total games</div>
      </div>
      <div id="all-matches"></div>
    </div>
  </div>

  <!-- ════════════ PAGE: PERFORMANCE ════════════ -->
  <div class="page" id="page-performance">
    <div class="section-head mb20">
      <div class="section-title">Performance Analysis</div>
      <div style="display:flex;gap:8px">
        <button class="btn btn-ghost btn-sm">Last 30 Days</button>
        <button class="btn btn-ghost btn-sm">Last 90 Days</button>
        <button class="btn btn-primary btn-sm">Season 2025</button>
      </div>
    </div>

    <div class="g4 mb20">
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">📊 Overall WR</div>
        <div class="kpi-val">57.3<span class="hi-dim">%</span></div>
        <div class="kpi-change up">↑ Top 23% of players</div>
        <div class="mini-bar"><div class="mini-fill" style="width:57.3%;background:var(--win)"></div></div>
      </div></div>
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">🎯 Avg KDA</div>
        <div class="kpi-val">3.82</div>
        <div class="kpi-change up">↑ Above average</div>
        <div class="mini-bar"><div class="mini-fill" style="width:72%;background:var(--accent)"></div></div>
      </div></div>
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">🌾 CS/min</div>
        <div class="kpi-val">7.4</div>
        <div class="kpi-change down">↓ Below Gold avg</div>
        <div class="mini-bar"><div class="mini-fill" style="width:58%;background:var(--gold)"></div></div>
      </div></div>
      <div class="card card-hover"><div class="kpi">
        <div class="kpi-lbl">💰 Gold/min</div>
        <div class="kpi-val">412</div>
        <div class="kpi-change up">↑ Top 35%</div>
        <div class="mini-bar"><div class="mini-fill" style="width:65%;background:var(--gold)"></div></div>
      </div></div>
    </div>

    <div class="g2 mb16">
      <div class="card">
        <div class="card-header"><div class="card-title">Weekly Win Rate Trend</div><div class="card-sub">Last 8 weeks</div></div>
        <div style="padding:20px">
          <div class="bar-chart" style="height:100px" id="trend-chart"></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Strength Breakdown</div><div class="card-sub">vs Gold I avg</div></div>
        <div style="padding:20px">
          <div class="prog-row"><span class="prog-label">Laning</span><div class="prog-track"><div class="prog-fill" style="width:72%;background:var(--win)"></div></div><span class="prog-val hi-green">72</span></div>
          <div class="prog-row"><span class="prog-label">Teamfight</span><div class="prog-track"><div class="prog-fill" style="width:65%;background:var(--accent)"></div></div><span class="prog-val hi-blue">65</span></div>
          <div class="prog-row"><span class="prog-label">Vision</span><div class="prog-track"><div class="prog-fill" style="width:48%;background:var(--text3)"></div></div><span class="prog-val">48</span></div>
          <div class="prog-row"><span class="prog-label">CS Farming</span><div class="prog-track"><div class="prog-fill" style="width:55%;background:var(--gold)"></div></div><span class="prog-val hi-gold">55</span></div>
          <div class="prog-row"><span class="prog-label">Objectives</span><div class="prog-track"><div class="prog-fill" style="width:61%;background:var(--accent)"></div></div><span class="prog-val hi-blue">61</span></div>
          <div class="prog-row"><span class="prog-label">Consistency</span><div class="prog-track"><div class="prog-fill" style="width:78%;background:var(--win)"></div></div><span class="prog-val hi-green">78</span></div>
        </div>
      </div>
    </div>

    <div class="g2">
      <div class="card">
        <div class="card-header"><div class="card-title">Game Length Performance</div></div>
        <div class="scroll-x">
          <table class="stat-table">
            <thead>
              <tr><th>Duration</th><th>Games</th><th>Win Rate</th><th>Avg KDA</th><th>Avg CS</th></tr>
            </thead>
            <tbody>
              <tr><td>&lt; 20 min</td><td>28</td><td><span class="badge b-win">71%</span></td><td class="hi-green">6.2</td><td>178</td></tr>
              <tr><td>20–30 min</td><td>112</td><td><span class="badge b-win">62%</span></td><td class="hi-green">4.1</td><td>224</td></tr>
              <tr><td>30–40 min</td><td>98</td><td><span class="badge b-gold">51%</span></td><td>3.3</td><td>267</td></tr>
              <tr><td>40–50 min</td><td>36</td><td><span class="badge b-loss">42%</span></td><td class="hi-red">2.6</td><td>298</td></tr>
              <tr><td>50+ min</td><td>10</td><td><span class="badge b-loss">30%</span></td><td class="hi-red">1.9</td><td>341</td></tr>
            </tbody>
          </table>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Ranked LP History</div><div class="card-sub">Season 2025</div></div>
        <div style="padding:20px">
          <div style="margin-bottom:14px">
            <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px">
              <span style="font-size:12px;color:var(--text3)">Gold I</span>
              <span style="font-family:'DM Mono',monospace;font-size:13px;font-weight:600">78 LP</span>
            </div>
            <div style="height:6px;background:var(--border);border-radius:100px;overflow:hidden">
              <div style="width:78%;height:100%;background:var(--gold);border-radius:100px"></div>
            </div>
            <div style="display:flex;justify-content:space-between;font-size:10px;color:var(--text3);margin-top:3px"><span>0 LP</span><span>100 LP → Promo</span></div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:1px;background:var(--border);border-radius:var(--r-sm);overflow:hidden;margin-top:8px">
            <div class="sum-cell"><div class="sum-val hi-green">163</div><div class="sum-lbl">Wins</div></div>
            <div class="sum-cell"><div class="sum-val hi-red">121</div><div class="sum-lbl">Losses</div></div>
            <div class="sum-cell"><div class="sum-val">+18</div><div class="sum-lbl">LP/Win</div></div>
            <div class="sum-cell"><div class="sum-val">−16</div><div class="sum-lbl">LP/Loss</div></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ════════════ PAGE: LEADERBOARD ════════════ -->
  <div class="page" id="page-leaderboard">
    <div class="section-head mb20">
      <div class="section-title">Leaderboard</div>
      <div style="display:flex;gap:8px">
        <button class="btn btn-ghost btn-sm">Global</button>
        <button class="btn btn-primary btn-sm">EUW</button>
      </div>
    </div>

    <div class="g4 mb20">
      <div class="card card-hover"><div class="kpi"><div class="kpi-lbl">📍 Your Rank</div><div class="kpi-val">#<span>1,842</span></div><div class="kpi-change hi-blue">Top 4% EUW</div></div></div>
      <div class="card card-hover"><div class="kpi"><div class="kpi-lbl">🌍 Server Players</div><div class="kpi-val">48<span class="hi-dim">k</span></div><div class="kpi-change hi-green">Active this season</div></div></div>
      <div class="card card-hover"><div class="kpi"><div class="kpi-lbl">🔺 Highest LP</div><div class="kpi-val">1,847</div><div class="kpi-change hi-gold">Challenger player</div></div></div>
      <div class="card card-hover"><div class="kpi"><div class="kpi-lbl">📊 Avg LP (Gold)</div><div class="kpi-val">62<span class="hi-dim"> LP</span></div><div class="kpi-change hi-blue">You're above avg</div></div></div>
    </div>

    <div class="main-split">
      <div class="flex-col">
        <div class="card">
          <div class="card-header">
            <div class="card-title">Top Players — EUW</div>
          </div>
          <div class="filter-bar">
            <span class="filter-label">Tier:</span>
            <div class="filter-pill active">All</div>
            <div class="filter-pill">Challenger</div>
            <div class="filter-pill">Grandmaster</div>
            <div class="filter-pill">Master</div>
            <div class="filter-pill">Diamond</div>
          </div>
          <div id="lb-list"></div>
        </div>
      </div>
      <div class="flex-col">
        <div class="card">
          <div class="card-header"><div class="card-title">Rank Distribution</div><div class="card-sub">EUW Server</div></div>
          <div style="padding:20px">
            <div class="prog-row"><span class="prog-label">Challenger</span><div class="prog-track"><div class="prog-fill" style="width:1%;background:#f59e0b"></div></div><span class="prog-val">0.01%</span></div>
            <div class="prog-row"><span class="prog-label">Grandmaster</span><div class="prog-track"><div class="prog-fill" style="width:2%;background:#ef4444"></div></div><span class="prog-val">0.05%</span></div>
            <div class="prog-row"><span class="prog-label">Master</span><div class="prog-track"><div class="prog-fill" style="width:4%;background:#a855f7"></div></div><span class="prog-val">0.4%</span></div>
            <div class="prog-row"><span class="prog-label">Diamond</span><div class="prog-track"><div class="prog-fill" style="width:8%;background:var(--accent)"></div></div><span class="prog-val">2.1%</span></div>
            <div class="prog-row"><span class="prog-label">Platinum</span><div class="prog-track"><div class="prog-fill" style="width:18%;background:#14b8a6"></div></div><span class="prog-val">9.3%</span></div>
            <div class="prog-row"><span class="prog-label" style="font-weight:700;color:var(--gold)">Gold ← You</span><div class="prog-track"><div class="prog-fill" style="width:28%;background:var(--gold)"></div></div><span class="prog-val hi-gold">22.7%</span></div>
            <div class="prog-row"><span class="prog-label">Silver</span><div class="prog-track"><div class="prog-fill" style="width:50%;background:#94a3b8"></div></div><span class="prog-val">32.4%</span></div>
            <div class="prog-row"><span class="prog-label">Bronze</span><div class="prog-track"><div class="prog-fill" style="width:65%;background:#b45309"></div></div><span class="prog-val">28.1%</span></div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">Most Played — Top 50</div></div>
          <div style="padding:16px 20px">
            <div class="recent-champ-row"><div class="cr-portrait" style="width:32px;height:32px;font-size:14px">🧙</div><div class="rcs"><div class="rcs-name">Stormcaller</div><div class="rcs-detail">42 games · 61% WR</div><div class="rcs-bar"><div class="rcs-fill" style="width:61%;background:var(--accent)"></div></div></div></div>
            <div class="recent-champ-row"><div class="cr-portrait" style="width:32px;height:32px;font-size:14px">🌙</div><div class="rcs"><div class="rcs-name">Voidwhisper</div><div class="rcs-detail">31 games · 68% WR</div><div class="rcs-bar"><div class="rcs-fill" style="width:68%;background:var(--win)"></div></div></div></div>
            <div class="recent-champ-row"><div class="cr-portrait" style="width:32px;height:32px;font-size:14px">🔥</div><div class="rcs"><div class="rcs-name">Emberlord</div><div class="rcs-detail">24 games · 54% WR</div><div class="rcs-bar"><div class="rcs-fill" style="width:54%;background:var(--gold)"></div></div></div></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ════════════ PAGE: FRIENDS ════════════ -->
  <div class="page" id="page-friends">
    <div class="section-head mb20">
      <div class="section-title">Friends</div>
      <button class="btn btn-primary btn-sm">+ Add Friend</button>
    </div>
    <div class="g3 mb20">
      <div class="card card-hover"><div class="kpi"><div class="kpi-lbl">👥 Friends</div><div class="kpi-val">12</div><div class="kpi-change hi-green">3 online now</div></div></div>
      <div class="card card-hover"><div class="kpi"><div class="kpi-lbl">⚔️ Games Together</div><div class="kpi-val">47</div><div class="kpi-change hi-blue">58% win rate duo</div></div></div>
      <div class="card card-hover"><div class="kpi"><div class="kpi-lbl">🔥 Best Duo WR</div><div class="kpi-val">72<span class="hi-dim">%</span></div><div class="kpi-change hi-gold">w/ ShadowBlaze99</div></div></div>
    </div>
    <div class="card">
      <div class="card-header"><div class="card-title">Friends List</div></div>
      <div id="friends-list"></div>
    </div>
  </div>

</div><!-- /main -->
</div><!-- /layout -->

<script>
/* ══════════════════════════════════════════════
   DATA
══════════════════════════════════════════════ */
const MATCHES = [
  { icon:'🧙', champ:'Stormcaller', result:'win',  mode:'Ranked', role:'Mid',     k:9,  d:2, a:14, cs:234, dmg:'31k', dur:'28:44', ratio:12.5 },
  { icon:'⚔️', champ:'Ironclad',    result:'loss', mode:'Ranked', role:'Top',     k:3,  d:8, a:5,  cs:181, dmg:'14k', dur:'36:12', ratio:1.0  },
  { icon:'🌙', champ:'Voidwhisper', result:'win',  mode:'Ranked', role:'Mid',     k:7,  d:3, a:18, cs:268, dmg:'28k', dur:'22:19', ratio:8.3  },
  { icon:'🔥', champ:'Emberlord',   result:'win',  mode:'Normal', role:'Bot',     k:12, d:4, a:7,  cs:302, dmg:'38k', dur:'31:05', ratio:4.75 },
  { icon:'❄️', champ:'Frostbite',   result:'loss', mode:'Ranked', role:'Support', k:2,  d:6, a:9,  cs:193, dmg:'19k', dur:'41:27', ratio:1.83 },
  { icon:'🧙', champ:'Stormcaller', result:'win',  mode:'Ranked', role:'Mid',     k:6,  d:1, a:21, cs:289, dmg:'34k', dur:'19:48', ratio:27.0 },
  { icon:'🌿', champ:'Thornwood',   result:'win',  mode:'Normal', role:'Jungle',  k:5,  d:3, a:16, cs:144, dmg:'18k', dur:'33:10', ratio:7.0  },
  { icon:'🌊', champ:'Tidesurge',   result:'loss', mode:'Ranked', role:'Support', k:1,  d:5, a:11, cs:42,  dmg:'8k',  dur:'29:55', ratio:2.4  },
  { icon:'⚡', champ:'Stormrazor',  result:'win',  mode:'Ranked', role:'Jungle',  k:8,  d:3, a:12, cs:168, dmg:'22k', dur:'25:30', ratio:6.67 },
  { icon:'🌑', champ:'Darkfall',    result:'loss', mode:'ARAM',   role:'Mid',     k:4,  d:7, a:8,  cs:0,   dmg:'21k', dur:'18:20', ratio:1.71 },
];

const CHAMPIONS = [
  { icon:'🧙', name:'Stormcaller', games:68, wins:39, role:'Mid',     kda:4.2, cs:8.1, dmg:'26k' },
  { icon:'🌙', name:'Voidwhisper', games:41, wins:29, role:'Mid',     kda:5.1, cs:9.3, dmg:'31k' },
  { icon:'🔥', name:'Emberlord',   games:33, wins:18, role:'Bot',     kda:3.5, cs:8.8, dmg:'29k' },
  { icon:'❄️', name:'Frostbite',   games:28, wins:11, role:'Support', kda:2.8, cs:2.1, dmg:'11k' },
  { icon:'⚡', name:'Stormrazor',  games:19, wins:12, role:'Jungle',  kda:3.9, cs:5.4, dmg:'20k' },
  { icon:'🌊', name:'Tidesurge',   games:16, wins:8,  role:'Support', kda:3.1, cs:1.8, dmg:'9k'  },
  { icon:'🌿', name:'Thornwood',   games:14, wins:7,  role:'Jungle',  kda:3.3, cs:5.9, dmg:'18k' },
  { icon:'⚔️', name:'Ironclad',    games:12, wins:5,  role:'Top',     kda:2.2, cs:6.8, dmg:'17k' },
  { icon:'🌑', name:'Darkfall',    games:11, wins:6,  role:'Mid',     kda:3.7, cs:8.4, dmg:'25k' },
  { icon:'🛡️', name:'Aegisborn',   games:9,  wins:5,  role:'Top',     kda:2.9, cs:6.2, dmg:'15k' },
];

const LEADERBOARD = [
  { rank:1,  icon:'👑', name:'CelestialVoid',  server:'EUW', tier:'Challenger',   lp:1847, wr:68, kda:8.2 },
  { rank:2,  icon:'🌊', name:'TidalForce',     server:'EUW', tier:'Challenger',   lp:1721, wr:65, kda:7.6 },
  { rank:3,  icon:'🔥', name:'PhoenixAsh',     server:'EUW', tier:'Challenger',   lp:1698, wr:63, kda:6.9 },
  { rank:4,  icon:'⚡', name:'StormBringer',   server:'EUW', tier:'Grandmaster',  lp:980,  wr:61, kda:6.2 },
  { rank:5,  icon:'🌙', name:'MoonShadow',     server:'EUW', tier:'Grandmaster',  lp:812,  wr:59, kda:5.8 },
  { rank:6,  icon:'🌿', name:'NatureWarden',   server:'EUW', tier:'Master',       lp:428,  wr:57, kda:5.1 },
  { rank:7,  icon:'🌑', name:'DarkOracle',     server:'EUW', tier:'Master',       lp:301,  wr:56, kda:4.9 },
  { rank:8,  icon:'🧙', name:'ArcaneScholar',  server:'EUW', tier:'Diamond',      lp:87,   wr:55, kda:4.3 },
  { rank:9,  icon:'🛡️', name:'IronWall',       server:'EUW', tier:'Diamond',      lp:62,   wr:53, kda:4.1 },
  { rank:10, icon:'🏹', name:'EternalArcher',  server:'EUW', tier:'Platinum',     lp:91,   wr:52, kda:3.9 },
];

const FRIENDS = [
  { icon:'🌊', name:'ShadowBlaze99',  server:'EUW', tier:'Platinum II',  online:true,  lastGame:'8 min ago',  wr:64 },
  { icon:'🔥', name:'IceFang_X',      server:'EUW', tier:'Gold III',     online:true,  lastGame:'In game…',   wr:51 },
  { icon:'⚡', name:'NovaDrift',       server:'EUNE',tier:'Diamond IV',   online:true,  lastGame:'Just now',   wr:58 },
  { icon:'🌙', name:'VoidStep',        server:'EUW', tier:'Gold II',      online:false, lastGame:'3 hrs ago',  wr:49 },
  { icon:'🧙', name:'Arcane_Seven',   server:'EUW', tier:'Platinum I',   online:false, lastGame:'Yesterday',  wr:55 },
  { icon:'🌿', name:'JungleLurker',   server:'EUW', tier:'Silver I',     online:false, lastGame:'2 days ago', wr:47 },
];

const WEEKLY_DATA = [
  {d:'MON',w:3,l:2},{d:'TUE',w:5,l:1},{d:'WED',w:2,l:4},
  {d:'THU',w:4,l:3},{d:'FRI',w:6,l:2},{d:'SAT',w:7,l:1},{d:'SUN',w:3,l:0}
];
const TREND_DATA = [
  {d:'W1',wr:48},{d:'W2',wr:52},{d:'W3',wr:50},{d:'W4',wr:55},
  {d:'W5',wr:53},{d:'W6',wr:58},{d:'W7',wr:61},{d:'W8',wr:57}
];
const ROLES = [
  {icon:'✨',name:'Mid',    pct:52},
  {icon:'🏹',name:'Bot',    pct:21},
  {icon:'🗡️',name:'Top',   pct:12},
  {icon:'🌿',name:'Jungle', pct:8},
  {icon:'🛡️',name:'Support',pct:7},
];

/* ══════════════════════════════════════════════
   HELPERS
══════════════════════════════════════════════ */
function ratioColor(r) {
  if (r >= 10) return 'var(--gold)';
  if (r >= 5)  return 'var(--win)';
  if (r >= 3)  return 'var(--text)';
  if (r >= 2)  return 'var(--text2)';
  return 'var(--loss)';
}
function wrClass(pct) {
  if (pct >= 60) return 'wr-g';
  if (pct >= 50) return 'wr-w';
  return 'wr-b';
}
function tierClass(t) {
  if (t==='Challenger')  return 'tier-chall';
  if (t==='Grandmaster') return 'tier-gm';
  if (t==='Master')      return 'tier-master';
  if (t==='Diamond')     return 'tier-diamond';
  if (t==='Platinum')    return 'tier-plat';
  return 'tier-gold';
}
function matchHTML(m) {
  return `<div class="match ${m.result}">
    <div class="m-stripe"></div>
    <div class="m-icon">${m.icon}</div>
    <div class="m-info">
      <div class="m-top">
        <span class="m-champ">${m.champ}</span>
        <span class="badge ${m.result==='win'?'b-win':'b-loss'}">${m.result==='win'?'Win':'Loss'}</span>
        <span class="badge b-gray">${m.role}</span>
        <span class="badge b-blue">${m.mode}</span>
      </div>
      <div class="m-kda">${m.k} / ${m.d} / ${m.a} &nbsp;·&nbsp; ${m.cs} CS &nbsp;·&nbsp; ${m.dmg} dmg</div>
    </div>
    <div class="m-right">
      <div class="m-ratio" style="color:${ratioColor(m.ratio)}">${m.ratio.toFixed(1)}</div>
      <div class="m-dur">${m.dur}</div>
    </div>
  </div>`;
}

/* ══════════════════════════════════════════════
   RENDER FUNCTIONS
══════════════════════════════════════════════ */
function renderOverviewMatches() {
  const el = document.getElementById('overview-matches');
  el.innerHTML = MATCHES.slice(0,6).map(matchHTML).join('');
}

function renderAllMatches(filter='all') {
  const el = document.getElementById('all-matches');
  const filtered = filter === 'all' ? MATCHES : MATCHES.filter(m => {
    if (filter==='ranked') return m.mode==='Ranked';
    if (filter==='normal') return m.mode==='Normal';
    if (filter==='aram')   return m.mode==='ARAM';
    return true;
  });
  el.innerHTML = filtered.length ? filtered.map(matchHTML).join('') : `<div class="empty-state"><div class="empty-icon">🎮</div><div class="empty-text">No matches found</div></div>`;
}

function renderMiniChamps() {
  const el = document.getElementById('mini-champs');
  el.innerHTML = CHAMPIONS.slice(0,5).map((c,i) => {
    const wr = Math.round((c.wins/c.games)*100);
    return `<div class="champ-row">
      <div class="cr-num">${String(i+1).padStart(2,'0')}</div>
      <div class="cr-portrait">${c.icon}</div>
      <div class="cr-info"><div class="cr-name">${c.name}</div><div class="cr-games">${c.games} games · ${c.role}</div></div>
      <div class="cr-stats">
        <div class="cr-s"><div class="cr-sv">${c.kda}</div><div class="cr-sl">KDA</div></div>
        <div class="cr-s"><div class="cr-sv">${c.cs}</div><div class="cr-sl">CS/m</div></div>
      </div>
      <div class="wr ${wrClass(wr)}">${wr}%</div>
    </div>`;
  }).join('');
}

function renderChampList(roleFilter='all') {
  const el = document.getElementById('champ-list');
  const list = roleFilter==='all' ? CHAMPIONS : CHAMPIONS.filter(c => c.role.toLowerCase()===roleFilter);
  el.innerHTML = list.map((c,i) => {
    const wr = Math.round((c.wins/c.games)*100);
    return `<div class="champ-row">
      <div class="cr-num">${String(i+1).padStart(2,'0')}</div>
      <div class="cr-portrait">${c.icon}</div>
      <div class="cr-info"><div class="cr-name">${c.name}</div><div class="cr-games">${c.games} games · ${c.wins}W ${c.games-c.wins}L · ${c.role}</div></div>
      <div class="cr-stats">
        <div class="cr-s"><div class="cr-sv">${c.kda}</div><div class="cr-sl">KDA</div></div>
        <div class="cr-s"><div class="cr-sv">${c.cs}</div><div class="cr-sl">CS/m</div></div>
        <div class="cr-s"><div class="cr-sv">${c.dmg}</div><div class="cr-sl">Dmg</div></div>
      </div>
      <div class="wr ${wrClass(wr)}">${wr}%</div>
    </div>`;
  }).join('');
}

function renderLeaderboard() {
  const el = document.getElementById('lb-list');
  el.innerHTML = LEADERBOARD.map(p => {
    let rankClass = p.rank <= 3 ? ['top1','top2','top3'][p.rank-1] : '';
    let rankIcon = p.rank === 1 ? '🥇' : p.rank === 2 ? '🥈' : p.rank === 3 ? '🥉' : p.rank;
    return `<div class="lb-row">
      <div class="lb-rank ${rankClass}">${rankIcon}</div>
      <div class="lb-av" style="background:var(--bg)">${p.icon}</div>
      <div class="lb-info">
        <div class="lb-name">${p.name}${p.name==='NightHawk_X'?' <span class="badge b-blue">You</span>':''}</div>
        <div class="lb-server">${p.server}</div>
      </div>
      <div class="lb-stats">
        <div class="lb-s"><div class="lb-sv">${p.wr}%</div><div class="lb-sl">Win Rate</div></div>
        <div class="lb-s"><div class="lb-sv">${p.kda}</div><div class="lb-sl">KDA</div></div>
        <div class="lb-s"><div class="lb-sv">${p.lp}</div><div class="lb-sl">LP</div></div>
      </div>
      <div class="tier-badge ${tierClass(p.tier)}">${p.tier}</div>
    </div>`;
  }).join('');
}

function renderFriends() {
  const el = document.getElementById('friends-list');
  el.innerHTML = FRIENDS.map(f => {
    return `<div class="lb-row">
      <div style="width:10px;height:10px;border-radius:50%;background:${f.online?'var(--win)':'var(--border2)'};flex-shrink:0;box-shadow:${f.online?'0 0 6px rgba(22,163,74,0.5)':'none'}"></div>
      <div class="lb-av" style="background:var(--bg)">${f.icon}</div>
      <div class="lb-info">
        <div class="lb-name">${f.name} ${f.online?'<span class="badge b-win" style="font-size:9px">Online</span>':''}</div>
        <div class="lb-server">${f.server} · ${f.tier}</div>
      </div>
      <div class="lb-stats">
        <div class="lb-s"><div class="lb-sv">${f.wr}%</div><div class="lb-sl">WR</div></div>
        <div class="lb-s" style="min-width:90px"><div class="lb-sv" style="font-size:11px;color:var(--text3)">${f.lastGame}</div><div class="lb-sl">Last Seen</div></div>
      </div>
      <button class="btn btn-ghost btn-sm">View</button>
    </div>`;
  }).join('');
}

function renderBarChart(id, data, maxH) {
  const el = document.getElementById(id);
  if (!el) return;
  const max = Math.max(...data.map(d => d.w + (d.l||0)));
  el.innerHTML = '';
  el.style.height = maxH + 'px';
  data.forEach(d => {
    const g = document.createElement('div');
    g.className = 'bcol';
    const wH = Math.round((d.w / max) * (maxH - 16));
    const lH = d.l !== undefined ? Math.round((d.l / max) * (maxH - 16)) : 0;
    const lBar = d.l !== undefined ? `<div class="bseg bl" style="height:${lH}px" title="${d.l} losses"></div>` : '';
    g.innerHTML = `<div class="bcol-bars"><div class="bseg bw" style="height:${wH}px" title="${d.w}"></div>${lBar}</div><div class="bday">${d.d}</div>`;
    el.appendChild(g);
  });
}

function renderRoles() {
  const el = document.getElementById('role-chips');
  el.innerHTML = ROLES.map((r, i) =>
    `<div class="role-chip ${i===0?'active':''}" data-role-idx="${i}">
      <div class="rc-icon">${r.icon}</div>
      <div class="rc-name">${r.name}</div>
      <div class="rc-pct">${r.pct}%</div>
    </div>`
  ).join('');
  el.querySelectorAll('.role-chip').forEach(c => {
    c.addEventListener('click', () => {
      el.querySelectorAll('.role-chip').forEach(x => x.classList.remove('active'));
      c.classList.add('active');
    });
  });
}

function renderTrendChart() {
  const el = document.getElementById('trend-chart');
  if (!el) return;
  const max = Math.max(...TREND_DATA.map(d => d.wr));
  el.style.height = '100px';
  el.innerHTML = '';
  TREND_DATA.forEach(d => {
    const g = document.createElement('div');
    g.className = 'bcol';
    const h = Math.round((d.wr / max) * 84);
    const color = d.wr >= 60 ? 'var(--win)' : d.wr >= 50 ? 'var(--accent)' : '#fca5a5';
    g.innerHTML = `<div class="bcol-bars"><div class="bseg" style="height:${h}px;background:${color};flex:1" title="${d.wr}% WR"></div></div><div class="bday">${d.d}</div>`;
    el.appendChild(g);
  });
}

/* ══════════════════════════════════════════════
   NAVIGATION
══════════════════════════════════════════════ */
const PAGE_LABELS = {
  overview: 'Overview',
  champions: 'Champions',
  matches: 'Match History',
  performance: 'Performance',
  leaderboard: 'Leaderboard',
  friends: 'Friends',
};

function navigate(pageId) {
  // hide all pages
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  // activate page
  const target = document.getElementById('page-' + pageId);
  if (target) {
    target.classList.add('active');
    // re-trigger animation
    target.style.animation = 'none';
    requestAnimationFrame(() => { target.style.animation = ''; });
  }
  // update nav
  document.querySelectorAll('.nav-item').forEach(n => {
    n.classList.toggle('active', n.dataset.page === pageId);
  });
  // update heading
  document.getElementById('pageHeading').textContent = PAGE_LABELS[pageId] || pageId;
  // lazy renders
  if (pageId === 'performance') { renderTrendChart(); }
}

// Nav clicks
document.querySelectorAll('.nav-item').forEach(n => {
  n.addEventListener('click', () => navigate(n.dataset.page));
});

// "View all" button in overview top-champs
document.querySelectorAll('[data-page]').forEach(btn => {
  if (btn.tagName === 'BUTTON') {
    btn.addEventListener('click', () => navigate(btn.dataset.page));
  }
});

/* ══════════════════════════════════════════════
   TABS
══════════════════════════════════════════════ */
function initTabs(groupId, onSelect) {
  const group = document.getElementById(groupId);
  if (!group) return;
  group.querySelectorAll('.tab').forEach((t, i) => {
    t.addEventListener('click', () => {
      group.querySelectorAll('.tab').forEach(x => x.classList.remove('active'));
      t.classList.add('active');
      onSelect(i, t.textContent.trim());
    });
  });
}
initTabs('tabs-overview', () => {}); // visual only for overview
initTabs('tabs-matches', (i, label) => {
  const map = { 'All':'all', 'Ranked Solo':'ranked', 'Normal':'normal', 'ARAM':'aram' };
  renderAllMatches(map[label] || 'all');
});

/* ══════════════════════════════════════════════
   FILTERS
══════════════════════════════════════════════ */
document.querySelectorAll('[data-filter="role"]').forEach(pill => {
  pill.addEventListener('click', () => {
    pill.closest('.filter-bar').querySelectorAll('[data-filter="role"]').forEach(p => p.classList.remove('active'));
    pill.classList.add('active');
    renderChampList(pill.dataset.val);
  });
});
document.querySelectorAll('.filter-pill:not([data-filter])').forEach(pill => {
  pill.addEventListener('click', () => {
    pill.closest('.filter-bar').querySelectorAll('.filter-pill').forEach(p => p.classList.remove('active'));
    pill.classList.add('active');
  });
});

/* ══════════════════════════════════════════════
   LIVE CLOCK
══════════════════════════════════════════════ */
let liveSec = 18 * 60 + 43;
setInterval(() => {
  liveSec++;
  const m = Math.floor(liveSec / 60), s = liveSec % 60;
  const el = document.getElementById('live-clock');
  if (el) el.textContent = `${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
}, 1000);

/* ══════════════════════════════════════════════
   SEARCH (visual placeholder)
══════════════════════════════════════════════ */
document.getElementById('searchInput').addEventListener('keydown', e => {
  if (e.key === 'Enter' && e.target.value.trim()) {
    document.getElementById('refreshBtn').textContent = '↺ Loading…';
    setTimeout(() => { document.getElementById('refreshBtn').textContent = '↺ Refresh'; }, 1200);
  }
});
document.getElementById('refreshBtn').addEventListener('click', () => {
  const btn = document.getElementById('refreshBtn');
  btn.textContent = '↺ Refreshing…';
  setTimeout(() => { btn.textContent = '↺ Refresh'; }, 1200);
});

/* ══════════════════════════════════════════════
   INIT
══════════════════════════════════════════════ */
renderOverviewMatches();
renderAllMatches();
renderMiniChamps();
renderChampList();
renderLeaderboard();
renderFriends();
renderBarChart('perf-chart', WEEKLY_DATA, 90);
renderRoles();
</script>
</body>
</html>
