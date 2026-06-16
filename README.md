<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Vademécum Médico Digital</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@300;400;500;600&family=IBM+Plex+Mono:wght@400;500&family=Fraunces:ital,wght@0,300;0,600;1,300&display=swap" rel="stylesheet" />
<style>
/* ─── DESIGN TOKENS ─────────────────────────────────────────────── */
:root {
  --bg:           #F7FAFC;
  --bg-card:      #FFFFFF;
  --bg-sidebar:   #0F2540;
  --bg-sidebar-h: #1A3A5C;
  --accent-blue:  #2B6CB0;
  --accent-mid:   #1A365D;
  --accent-green: #276749;
  --accent-teal:  #2C7A7B;
  --alert-bg:     #FFFBEB;
  --alert-border: #D97706;
  --alert-text:   #92400E;
  --danger-bg:    #FFF5F5;
  --danger-border:#C53030;
  --text-main:    #1A202C;
  --text-muted:   #718096;
  --text-light:   #A0AEC0;
  --text-sidebar: #CBD5E0;
  --border:       #E2E8F0;
  --border-strong:#CBD5E0;
  --sidebar-w:    300px;
  --topbar-h:     60px;
  --radius:       8px;
  --radius-lg:    12px;
  --shadow-sm:    0 1px 3px rgba(0,0,0,.08);
  --shadow-md:    0 4px 16px rgba(0,0,0,.1);
  --transition:   0.22s ease;
  --mono:         'IBM Plex Mono', monospace;
  --sans:         'IBM Plex Sans', system-ui, sans-serif;
  --display:      'Fraunces', Georgia, serif;
}

/* ─── RESET & BASE ──────────────────────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: var(--sans);
  background: var(--bg);
  color: var(--text-main);
  font-size: 15px;
  line-height: 1.65;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  overflow-x: hidden;
}
a { color: var(--accent-blue); text-decoration: none; }
a:hover { text-decoration: underline; }

/* ─── TOPBAR ────────────────────────────────────────────────────── */
#topbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  height: var(--topbar-h);
  background: var(--accent-mid);
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 0 20px;
  z-index: 100;
  box-shadow: 0 2px 8px rgba(0,0,0,.25);
}
#topbar-logo {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-shrink: 0;
  color: #fff;
}
#topbar-logo svg { flex-shrink: 0; }
#topbar-logo-text { font-family: var(--display); font-weight: 600; font-size: 17px; letter-spacing: .02em; line-height: 1.2; }
#topbar-logo-sub { font-size: 10px; font-weight: 300; letter-spacing: .12em; text-transform: uppercase; opacity: .65; }
#sidebar-toggle {
  background: none;
  border: none;
  cursor: pointer;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
  border-radius: var(--radius);
  transition: background var(--transition);
  flex-shrink: 0;
}
#sidebar-toggle:hover { background: rgba(255,255,255,.12); }
#search-wrapper {
  flex: 1;
  max-width: 520px;
  margin-left: auto;
  position: relative;
}
#search-input {
  width: 100%;
  padding: 9px 16px 9px 38px;
  border-radius: 20px;
  border: none;
  background: rgba(255,255,255,.13);
  color: #fff;
  font-family: var(--sans);
  font-size: 14px;
  outline: none;
  transition: background var(--transition);
}
#search-input::placeholder { color: rgba(255,255,255,.5); }
#search-input:focus { background: rgba(255,255,255,.2); }
#search-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  opacity: .6;
  pointer-events: none;
}
#search-results {
  position: absolute;
  top: calc(100% + 6px);
  left: 0; right: 0;
  background: var(--bg-card);
  border-radius: var(--radius);
  box-shadow: var(--shadow-md);
  border: 1px solid var(--border);
  max-height: 340px;
  overflow-y: auto;
  display: none;
  z-index: 200;
}
.search-result-item {
  display: flex;
  flex-direction: column;
  padding: 10px 16px;
  cursor: pointer;
  border-bottom: 1px solid var(--border);
  transition: background var(--transition);
}
.search-result-item:last-child { border-bottom: none; }
.search-result-item:hover { background: var(--bg); }
.search-result-name { font-weight: 500; font-size: 14px; color: var(--text-main); }
.search-result-path { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
.search-result-empty { padding: 16px; text-align: center; color: var(--text-muted); font-size: 14px; }

/* ─── LAYOUT ────────────────────────────────────────────────────── */
#layout {
  display: flex;
  margin-top: var(--topbar-h);
  min-height: calc(100vh - var(--topbar-h));
}

/* ─── SIDEBAR ───────────────────────────────────────────────────── */
#sidebar {
  width: var(--sidebar-w);
  background: var(--bg-sidebar);
  flex-shrink: 0;
  position: fixed;
  top: var(--topbar-h);
  left: 0;
  bottom: 0;
  overflow-y: auto;
  transition: transform var(--transition);
  z-index: 90;
  scrollbar-width: thin;
  scrollbar-color: rgba(255,255,255,.15) transparent;
}
#sidebar.collapsed { transform: translateX(calc(-1 * var(--sidebar-w))); }
#sidebar::-webkit-scrollbar { width: 4px; }
#sidebar::-webkit-scrollbar-thumb { background: rgba(255,255,255,.15); border-radius: 4px; }

.nav-chapter {
  border-bottom: 1px solid rgba(255,255,255,.06);
}
.nav-chapter-btn {
  width: 100%;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 13px 16px;
  background: none;
  border: none;
  cursor: pointer;
  color: var(--text-sidebar);
  font-family: var(--sans);
  font-size: 12.5px;
  font-weight: 500;
  text-align: left;
  transition: background var(--transition), color var(--transition);
  line-height: 1.4;
}
.nav-chapter-btn:hover { background: var(--bg-sidebar-h); color: #fff; }
.nav-chapter-btn.active { background: var(--bg-sidebar-h); color: #fff; }
.nav-chapter-num {
  font-family: var(--mono);
  font-size: 10px;
  font-weight: 500;
  color: var(--accent-teal);
  flex-shrink: 0;
  margin-top: 1px;
  letter-spacing: .05em;
}
.nav-chapter-arrow {
  margin-left: auto;
  flex-shrink: 0;
  transition: transform var(--transition);
  opacity: .5;
}
.nav-chapter-btn.open .nav-chapter-arrow { transform: rotate(90deg); }

.nav-sub-list {
  list-style: none;
  padding: 0;
  display: none;
  background: rgba(0,0,0,.15);
}
.nav-sub-list.open { display: block; }
.nav-sub-item {
  border-bottom: 1px solid rgba(255,255,255,.04);
}
.nav-sub-btn {
  width: 100%;
  display: flex;
  align-items: flex-start;
  gap: 8px;
  padding: 9px 16px 9px 32px;
  background: none;
  border: none;
  cursor: pointer;
  color: rgba(203,213,224,.75);
  font-family: var(--sans);
  font-size: 12px;
  text-align: left;
  line-height: 1.4;
  transition: background var(--transition), color var(--transition);
}
.nav-sub-btn:hover { background: rgba(255,255,255,.05); color: #fff; }
.nav-sub-btn.active { color: #63B3ED; background: rgba(99,179,237,.08); }
.nav-sub-dot { font-size: 8px; margin-top: 4px; flex-shrink: 0; opacity: .5; }
.nav-sub-btn.active .nav-sub-dot { opacity: 1; color: #63B3ED; }

/* ─── MAIN CONTENT ──────────────────────────────────────────────── */
#main {
  margin-left: var(--sidebar-w);
  flex: 1;
  transition: margin-left var(--transition);
  min-width: 0;
}
#main.sidebar-collapsed { margin-left: 0; }

#content-area {
  max-width: 860px;
  margin: 0 auto;
  padding: 40px 36px 80px;
}

/* ─── HOME / WELCOME ────────────────────────────────────────────── */
#home-view {
  display: block;
}
.home-hero {
  background: linear-gradient(135deg, var(--accent-mid) 0%, #1E4E8C 100%);
  border-radius: var(--radius-lg);
  padding: 48px 40px;
  color: #fff;
  margin-bottom: 32px;
  position: relative;
  overflow: hidden;
}
.home-hero::before {
  content: '';
  position: absolute;
  top: -40px; right: -40px;
  width: 200px; height: 200px;
  border-radius: 50%;
  background: rgba(255,255,255,.04);
}
.home-hero::after {
  content: '';
  position: absolute;
  bottom: -60px; right: 60px;
  width: 280px; height: 280px;
  border-radius: 50%;
  background: rgba(255,255,255,.03);
}
.home-hero-label {
  font-family: var(--mono);
  font-size: 11px;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: #63B3ED;
  margin-bottom: 12px;
}
.home-hero h1 {
  font-family: var(--display);
  font-size: 36px;
  font-weight: 600;
  line-height: 1.15;
  margin-bottom: 14px;
}
.home-hero p {
  font-size: 15px;
  opacity: .8;
  max-width: 520px;
  line-height: 1.7;
}
.home-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 16px;
  margin-top: 28px;
}
.home-stats .home-stat {
  background: rgba(255,255,255,.08);
  border-radius: var(--radius);
  padding: 16px 20px;
  backdrop-filter: blur(4px);
}
.home-stat-num {
  font-family: var(--mono);
  font-size: 26px;
  font-weight: 500;
  color: #63B3ED;
  line-height: 1;
}
.home-stat-label { font-size: 12px; opacity: .7; margin-top: 4px; }

.home-chapters {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 14px;
}
.home-chapter-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 18px 20px;
  cursor: pointer;
  transition: box-shadow var(--transition), border-color var(--transition), transform var(--transition);
  display: flex;
  gap: 14px;
  align-items: flex-start;
}
.home-chapter-card:hover {
  box-shadow: var(--shadow-md);
  border-color: var(--accent-blue);
  transform: translateY(-1px);
}
.home-chapter-icon {
  width: 36px;
  height: 36px;
  border-radius: 8px;
  background: linear-gradient(135deg, #EBF4FF, #BEE3F8);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--mono);
  font-size: 11px;
  font-weight: 500;
  color: var(--accent-blue);
  flex-shrink: 0;
  border: 1px solid #BEE3F8;
}
.home-chapter-title {
  font-size: 13px;
  font-weight: 500;
  color: var(--text-main);
  line-height: 1.4;
}
.home-chapter-sub {
  font-size: 11px;
  color: var(--text-muted);
  margin-top: 3px;
}
.home-section-heading {
  font-family: var(--display);
  font-size: 18px;
  font-weight: 300;
  color: var(--text-main);
  margin-bottom: 16px;
  padding-bottom: 8px;
  border-bottom: 2px solid var(--border);
}

/* ─── DRUG SECTION VIEW ─────────────────────────────────────────── */
#section-view { display: none; }
.section-breadcrumb {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 12px;
  color: var(--text-muted);
  margin-bottom: 24px;
  flex-wrap: wrap;
}
.breadcrumb-sep { opacity: .4; }
.breadcrumb-link { cursor: pointer; color: var(--accent-blue); }
.breadcrumb-link:hover { text-decoration: underline; }
.breadcrumb-current { color: var(--text-main); font-weight: 500; }

.section-header {
  margin-bottom: 28px;
}
.section-chapter-label {
  font-family: var(--mono);
  font-size: 11px;
  letter-spacing: .1em;
  text-transform: uppercase;
  color: var(--accent-teal);
  margin-bottom: 6px;
}
.section-title {
  font-family: var(--display);
  font-size: 30px;
  font-weight: 600;
  color: var(--accent-mid);
  line-height: 1.2;
  margin-bottom: 8px;
}
.section-subtitle {
  font-size: 14px;
  color: var(--text-muted);
  line-height: 1.6;
}

/* DRUG CARD */
.drug-card {
  background: linear-gradient(135deg, #EBF8FF 0%, #E6FFFA 100%);
  border: 1px solid #BEE3F8;
  border-radius: var(--radius-lg);
  padding: 20px 24px;
  margin-bottom: 28px;
}
.drug-card-title {
  font-family: var(--mono);
  font-size: 11px;
  letter-spacing: .1em;
  text-transform: uppercase;
  color: var(--accent-teal);
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  gap: 6px;
}
.drug-card-title::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--accent-teal);
  opacity: .2;
}
.drug-tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
}
.drug-tag {
  background: var(--bg-card);
  border: 1px solid #BEE3F8;
  border-radius: 4px;
  padding: 4px 10px;
  font-size: 13px;
  font-weight: 500;
  color: var(--accent-mid);
  font-family: var(--mono);
}

/* ALERT BOXES */
.alert-box {
  border-radius: var(--radius);
  padding: 14px 18px;
  margin: 20px 0;
  display: flex;
  gap: 12px;
  align-items: flex-start;
}
.alert-box.warning {
  background: var(--alert-bg);
  border: 1px solid #F6C05E;
  border-left: 4px solid var(--alert-border);
}
.alert-box.danger {
  background: var(--danger-bg);
  border: 1px solid #FC8181;
  border-left: 4px solid var(--danger-border);
}
.alert-box.info {
  background: #EBF8FF;
  border: 1px solid #90CDF4;
  border-left: 4px solid var(--accent-blue);
}
.alert-box.success {
  background: #F0FFF4;
  border: 1px solid #9AE6B4;
  border-left: 4px solid var(--accent-green);
}
.alert-box-icon { font-size: 16px; flex-shrink: 0; margin-top: 1px; }
.alert-box-body { flex: 1; }
.alert-box-title {
  font-size: 12.5px;
  font-weight: 600;
  margin-bottom: 4px;
  text-transform: uppercase;
  letter-spacing: .05em;
}
.alert-box.warning .alert-box-title { color: var(--alert-text); }
.alert-box.danger .alert-box-title { color: #742A2A; }
.alert-box.info .alert-box-title { color: var(--accent-mid); }
.alert-box.success .alert-box-title { color: var(--accent-green); }
.alert-box p, .alert-box ul { font-size: 13.5px; line-height: 1.6; }
.alert-box ul { padding-left: 16px; margin-top: 4px; }

/* 7-POINT CONTENT ──────────────────────────────────────────────── */
.content-block {
  margin-bottom: 32px;
}
.point-section {
  margin-bottom: 24px;
  padding-bottom: 24px;
  border-bottom: 1px solid var(--border);
}
.point-section:last-child { border-bottom: none; }
.point-header {
  display: flex;
  align-items: baseline;
  gap: 10px;
  margin-bottom: 10px;
}
.point-num {
  font-family: var(--mono);
  font-size: 11px;
  font-weight: 500;
  color: var(--accent-blue);
  background: #EBF4FF;
  border: 1px solid #BEE3F8;
  border-radius: 4px;
  padding: 2px 8px;
  flex-shrink: 0;
  letter-spacing: .06em;
}
.point-label {
  font-size: 14px;
  font-weight: 600;
  color: var(--text-main);
  letter-spacing: .01em;
}
.point-content {
  font-size: 14px;
  line-height: 1.75;
  color: var(--text-main);
  padding-left: 4px;
}
.point-content ul, .point-content ol {
  padding-left: 20px;
  margin-top: 6px;
}
.point-content li { margin-bottom: 4px; }
.point-content strong { font-weight: 600; color: var(--accent-mid); }
.point-content em { font-style: italic; color: var(--text-muted); }

/* DOSE TABLE */
.dose-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  margin-top: 10px;
  border-radius: var(--radius);
  overflow: hidden;
  border: 1px solid var(--border);
}
.dose-table th {
  background: var(--accent-mid);
  color: #fff;
  padding: 10px 14px;
  text-align: left;
  font-weight: 500;
  font-size: 12px;
  letter-spacing: .04em;
  text-transform: uppercase;
}
.dose-table td {
  padding: 9px 14px;
  border-bottom: 1px solid var(--border);
  vertical-align: top;
  line-height: 1.5;
}
.dose-table tr:last-child td { border-bottom: none; }
.dose-table tr:nth-child(even) td { background: var(--bg); }
.dose-table td:first-child { font-family: var(--mono); font-size: 12.5px; font-weight: 500; color: var(--accent-mid); }

/* SUB-FAMILY DIVIDER */
.subfamily-divider {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: 36px 0 20px;
}
.subfamily-divider-line { flex: 1; height: 1px; background: var(--border); }
.subfamily-divider-label {
  font-family: var(--mono);
  font-size: 10px;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: var(--text-light);
  white-space: nowrap;
}

/* PLACEHOLDER */
.section-placeholder {
  background: var(--bg-card);
  border: 2px dashed var(--border-strong);
  border-radius: var(--radius-lg);
  padding: 60px 40px;
  text-align: center;
  color: var(--text-muted);
}
.section-placeholder-icon { font-size: 40px; margin-bottom: 16px; opacity: .4; }
.section-placeholder h3 { font-size: 18px; font-weight: 500; margin-bottom: 8px; color: var(--text-main); }
.section-placeholder p { font-size: 14px; max-width: 380px; margin: 0 auto; }

/* ─── SCROLLBAR GLOBAL ──────────────────────────────────────────── */
::-webkit-scrollbar { width: 6px; height: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border-strong); border-radius: 6px; }

/* ─── RESPONSIVE ────────────────────────────────────────────────── */
@media (max-width: 768px) {
  :root { --sidebar-w: 280px; }
  #sidebar { transform: translateX(calc(-1 * var(--sidebar-w))); }
  #sidebar.open { transform: translateX(0); }
  #main { margin-left: 0 !important; }
  #content-area { padding: 24px 20px 60px; }
  .home-hero { padding: 32px 24px; }
  .home-hero h1 { font-size: 26px; }
  #sidebar-overlay { display: block; }
}
#sidebar-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,.4);
  z-index: 80;
}
@media (min-width: 769px) {
  #sidebar-overlay { display: none !important; }
}

/* ─── TRANSITIONS ───────────────────────────────────────────────── */
#home-view, #section-view {
  animation: fadeIn .2s ease;
}
@keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }
</style>
</head>
<body>

<div id="sidebar-overlay" onclick="closeSidebarMobile()"></div>

<header id="topbar">
  <button id="sidebar-toggle" onclick="toggleSidebar()" aria-label="Menú">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
      <line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/>
    </svg>
  </button>
  <div id="topbar-logo" onclick="showHome()" style="cursor:pointer">
    <svg width="28" height="28" viewBox="0 0 28 28" fill="none">
      <rect width="28" height="28" rx="6" fill="#2B6CB0"/>
      <path d="M14 6v16M6 14h16" stroke="#fff" stroke-width="2.5" stroke-linecap="round"/>
      <circle cx="14" cy="14" r="3" fill="#63B3ED" opacity=".7"/>
    </svg>
    <div>
      <div id="topbar-logo-text">Vademécum Médico</div>
      <div id="topbar-logo-sub">Referencia Clínica Digital</div>
    </div>
  </div>

  <div id="search-wrapper">
    <svg id="search-icon" width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round">
      <circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/>
    </svg>
    <input type="text" id="search-input" placeholder="Buscar fármaco, clase o mecanismo…" autocomplete="off" oninput="handleSearch(this.value)" onblur="setTimeout(()=>closeSearch(),150)" onfocus="handleSearch(this.value)" />
    <div id="search-results"></div>
  </div>
</header>

<div id="layout">
  <nav id="sidebar">
    <div id="nav-tree"></div>
  </nav>

  <main id="main">
    <div id="content-area">
      <div id="home-view">
        <div class="home-hero">
          <div class="home-hero-label">⚕ Referencia Farmacológica Clínica</div>
          <h1>Vademécum<br/>Médico Digital</h1>
          <p>Compendio farmacológico de referencia con rigor clínico: mecanismos de acción, espectro, farmacocinética, farmacodinamia, efectos adversos y dosificación estándar.</p>
          <div class="home-stats">
            <div class="home-stat">
              <div class="home-stat-num" id="stat-chapters">11</div>
              <div class="home-stat-label">Capítulos temáticos</div>
            </div>
            <div class="home-stat">
              <div class="home-stat-num" id="stat-sections">—</div>
              <div class="home-stat-label">Secciones</div>
            </div>
            <div class="home-stat">
              <div class="home-stat-num">7</div>
              <div class="home-stat-label">Puntos por clase</div>
            </div>
          </div>
        </div>

        <p class="home-section-heading">Capítulos del Compendio</p>
        <div class="home-chapters" id="home-chapter-grid"></div>
      </div>

      <div id="section-view">
        <div class="section-breadcrumb" id="section-breadcrumb"></div>
        <div class="section-header">
          <div class="section-chapter-label" id="section-chapter-label"></div>
          <h2 class="section-title" id="section-title"></h2>
          <p class="section-subtitle" id="section-subtitle"></p>
        </div>
        <div id="section-body"></div>
      </div>
    </div>
  </main>
</div>

<script>
/* ═══════════════════════════════════════════════════════════════════
   DATA — Navigation Tree
   Each chapter → sections → content (populated as we go)
   ═══════════════════════════════════════════════════════════════════ */
const CHAPTERS = [
  {
    id: 'c1', num: '01', title: 'Generalidades y Principios de Antibioticoterapia',
    icon: '🔬', color: '#2C7A7B',
    sections: [
      { id: 's1-1', title: 'Historia y evolución del uso de antimicrobianos', sub: 'Contexto histórico', tags: [] },
      { id: 's1-2', title: 'Principios generales y criterios de elección', sub: 'Espectro, CIM, CBM · Criterios del hospedero · Terapia empírica vs dirigida', tags: [] },
    ]
  },
  {
    id: 'c2', num: '02', title: 'Inhibidores de la Síntesis de Pared Celular',
    icon: '💊', color: '#2B6CB0',
    sections: [
      { id: 's2-1', title: 'Penicilinas y Combinaciones', sub: 'Naturales · Aminopenicilinas · Antiestafilocócicas', tags: ['Penicilina G','Amoxicilina','Dicloxacilina'] },
      { id: 's2-2', title: 'Inhibidores de Betalactamasas', sub: 'Ác. Clavulánico · Sulbactam · Tazobactam', tags: ['Ác. Clavulánico','Sulbactam','Tazobactam'] },
      { id: 's2-3', title: 'Cefalosporinas', sub: '1ª → 5ª Generación', tags: ['Cefalexina','Ceftriaxona','Cefepima','Ceftarolina'] },
      { id: 's2-4', title: 'Glucopéptidos', sub: 'Vancomicina · Teicoplanina', tags: ['Vancomicina','Teicoplanina'] },
    ]
  },
  {
    id: 'c3', num: '03', title: 'Inhibidores de la Síntesis de Proteínas',
    icon: '🧬', color: '#553C9A',
    sections: [
      { id: 's3-1', title: 'Macrólidos', sub: 'Subunidad 50S — Bacteriostáticos', tags: ['Eritromicina','Azitromicina','Claritromicina'] },
      { id: 's3-2', title: 'Aminoglucósidos', sub: 'Subunidad 30S — Bactericidas O₂-dependientes', tags: ['Gentamicina','Amikacina','Estreptomicina'] },
      { id: 's3-3', title: 'Tetraciclinas', sub: 'Subunidad 30S — Bacteriostáticos de amplio espectro', tags: ['Tetraciclina','Doxiciclina','Minociclina'] },
    ]
  },
  {
    id: 'c4', num: '04', title: 'Moduladores del ADN/ARN y Bloqueadores Metabólicos',
    icon: '🧪', color: '#C05621',
    sections: [
      { id: 's4-1', title: 'Quinolonas y Fluoroquinolonas', sub: '1ª→4ª Generación · Inhibidores de Topoisomerasas II y IV', tags: ['Ciprofloxacino','Levofloxacino','Moxifloxacino'] },
      { id: 's4-2', title: 'Sulfonamidas y Antifolatos', sub: 'Inhibición secuencial síntesis de Ác. Fólico', tags: ['Sulfametoxazol','Trimetoprima','Cotrimoxazol'] },
      { id: 's4-3', title: 'Inhibidores de ARN Polimerasa', sub: 'Rifampicina', tags: ['Rifampicina'] },
    ]
  },
  {
    id: 'c5', num: '05', title: 'Quimioterápicos, Antisépticos Urinarios y Misceláneos',
    icon: '⚗️', color: '#276749',
    sections: [
      { id: 's5-1', title: 'Antisépticos de Vías Urinarias', sub: 'Nitrofurantoína · Metenamina · Fenazopiridina', tags: ['Nitrofurantoína','Metenamina','Fenazopiridina'] },
      { id: 's5-2', title: 'Antibióticos Misceláneos', sub: 'Clindamicina · Cloranfenicol · Polimixinas · Ácido fusídico', tags: ['Clindamicina','Cloranfenicol','Colistina','Ácido fusídico'] },
    ]
  },
  {
    id: 'c6', num: '06', title: 'Farmacología Antiparasitaria',
    icon: '🦠', color: '#744210',
    sections: [
      { id: 's6-1', title: 'Fármacos Antipalúdicos', sub: 'Esquizonticidas sanguíneos/tisulares · Artemisininas', tags: ['Cloroquina','Primaquina','Artesunato'] },
      { id: 's6-2', title: 'Antiprotozoarios Clínicos', sub: 'Paromomicina · Metronidazol · Nitazoxanida', tags: ['Metronidazol','Tinidazol','Nitazoxanida','Benznidazol','Glucantime'] },
      { id: 's6-3', title: 'Fármacos Antihelmínticos', sub: 'Benzimidazoles · Ivermectina · Prazicuantel', tags: ['Albendazol','Mebendazol','Ivermectina','Prazicuantel'] },
    ]
  },
  {
    id: 'c7', num: '07', title: 'Antimicóticos (Antifúngicos)',
    icon: '🍄', color: '#6B46C1',
    sections: [
      { id: 's7-1', title: 'Agentes de Membrana Celular', sub: 'Azoles · Poliénicos · Alilaminas', tags: ['Ketoconazol','Fluconazol','Anfotericina B','Nistatina','Terbinafina'] },
      { id: 's7-2', title: 'Inhibidores de la División Celular', sub: 'Griseofulvina', tags: ['Griseofulvina'] },
    ]
  },
  {
    id: 'c8', num: '08', title: 'Antivirales',
    icon: '🦠', color: '#E53E3E',
    sections: [
      { id: 's8-1', title: 'Antivirales No Retrovirales', sub: 'Aciclovir · Ganciclovir · Ribavirina · Interferón', tags: ['Aciclovir','Ganciclovir','Ribavirina'] },
      { id: 's8-2', title: 'Antivirales Retrovirales — Esquema VIH', sub: 'ITIAN · Inhibidores de Proteasa', tags: ['Zidovudina','Saquinavir'] },
    ]
  },
  {
    id: 'c9', num: '09', title: 'Inmunomoduladores',
    icon: '🛡️', color: '#2C7A7B',
    sections: [
      { id: 's9-1', title: 'Inmunosupresores e Inmunoestimulantes', sub: 'Ciclosporina · Tacrolimus · Interferón · Factores de transferencia', tags: ['Ciclosporina','Tacrolimus'] },
    ]
  },
  {
    id: 'c10', num: '10', title: 'Fármacos en Sangre y Órganos Hematopoyéticos',
    icon: '🩸', color: '#C53030',
    sections: [
      { id: 's10-1', title: 'Fármacos Hematopoyéticos y Soporte', sub: 'Eritropoyetina · Filgrastim · Hierro · Vit B12 · Ác. fólico', tags: ['Eritropoyetina','Filgrastim','Sulfato ferroso'] },
    ]
  },
  {
    id: 'c11', num: '11', title: 'Hormonas y sus Antagonistas',
    icon: '⚡', color: '#D69E2E',
    sections: [
      { id: 's11-1', title: 'Hormonas Hipofisarias e Hipotalámicas', sub: 'Somatotropina · Leuprolida', tags: ['Somatotropina','Leuprolida'] },
      { id: 's11-2', title: 'Hormonas Tiroideas y Antitiroideos', sub: 'Levotiroxina · Metimazol · PTU', tags: ['Levotiroxina','Metimazol','Propiltiouracilo'] },
      { id: 's11-3', title: 'Estrógenos, Progestágenos y Andrógenos', sub: 'Estradiol · Progesterona · Levonorgestrel · Testosterona', tags: ['Estradiol','Levonorgestrel','Testosterona'] },
      { id: 's11-4', title: 'Moduladores Selectivos y Antagonistas', sub: 'Tamoxifeno · Clomifeno · Finasteride · Flutamida', tags: ['Tamoxifeno','Clomifeno','Finasteride'] },
      { id: 's11-5', title: 'Insulinas', sub: 'Rápidas · Intermedias · Prolongadas', tags: ['Insulina Regular','Insulina NPH','Insulina Glargina'] },
      { id: 's11-6', title: 'Antidiabéticos Orales e Incretinas', sub: 'Metformina · Glibenclamida · DPP4 · GLP1 · SGLT2', tags: ['Metformina','Sitagliptina','Semaglutida','Empagliflozina'] },
    ]
  },
];

/* ═══════════════════════════════════════════════════════════════════
   CONTENT STORE — populated per section as we develop
   ═══════════════════════════════════════════════════════════════════ */
const CONTENT = {
  // CAPÍTULO 01
  's1-1': {
    points: [
      "El desarrollo de los antimicrobianos comenzó formalmente con las ideas de selectividad terapéutica propuestas por Paul Ehrlich y consolidó su hito histórico más importante tras el descubrimiento accidental de la penicilina por Alexander Fleming en el año 1928[cite: 507].",
      "Moléculas específicas aisladas de hongos o bacterias, compuestos sintéticos y combinaciones sinérgicas diseñadas químicamente para interferir con dianas microbiológicas selectivas[cite: 507, 541, 584].",
      "Actúan de manera dirigida bloqueando dianas específicas esenciales del patógeno, tales como la integridad de su pared celular, los procesos de replicación o transcripción de material genético, o bien la maquinaria ribosomal de traducción de proteínas estructurales, minimizando el impacto citotóxico en células del huésped[cite: 507, 555, 675, 786].",
      "Interacciones dinámicas y selectivas fármaco-receptor enfocadas en interrumpir de forma irreversible o reversible las cascadas enzimáticas fundamentales de los microorganismos diana[cite: 507, 555, 675].",
      "La producción farmacéutica experimentó una notable era industrial y evolución tecnológica, transitando desde la elaboración artesanal de preparaciones magistrales dentro de boticas tradicionales hacia la estandarización en masa de medicamentos industriales con estrictos controles de pureza, estabilidad y calidad molecular uniformes[cite: 508, 525, 528, 531, 534].",
      "Reacciones asociadas a la pureza de los primeros compuestos biológicos, variabilidad en la respuesta biológica individual y riesgos inherentes a fallos en la estabilidad de fórmulas no estandarizadas industriales primigenias[cite: 508, 534].",
      "Evolución de dosificaciones desde enfoques empíricos no medidos hacia regímenes estandarizados modernos validados mediante ensayos clínicos internacionales rigurosos[cite: 508, 531]."
    ]
  },
  's1-2': {
    points: [
      "La elección óptima de la antibioticoterapia se fundamenta en un equilibrio tripartito analítico que considera de forma exhaustiva los criterios del hospedero, los perfiles microbiológicos del patógeno infeccioso y las características intrínsecas de la molécula farmacológica seleccionada[cite: 536, 537, 513, 520].",
      "Microorganismos patógenos diversos clasificados según sus perfiles bioquímicos de susceptibilidad, considerando parámetros microbiológicos cuantitativos clave como la Concentración Inhibitoria Mínima (CIM) y la Concentración Bactericida Mínima (CBM)[cite: 5].",
      "Mediante la selección racional basada en el espectro antibacteriano exacto del fármaco, la confirmación microbiológica del agente causal y la evaluación sistemática de los patrones locales de resistencia reportados en el antibiograma del paciente[cite: 513, 522].",
      "Relación dinámica dependiente del espectro antimicrobiano, los niveles de toxicidad sistémica y el establecimiento de perfiles PK/PD precisos para optimizar la erradicación del microorganismo con la menor tasa de inducción de resistencia colateral[cite: 522, 523].",
      "Determinada críticamente por los factores del huésped como la edad, función renal y hepática, estado de embarazo o lactancia, sitio anatómico específico de la infección y antecedentes inmunológicos o de alergias. El fármaco evalúa su penetración tisular y relación costo-beneficio global[cite: 510, 511, 513, 523, 524, 537, 538, 540].",
      "Riesgo potencial de toxicidad celular directa, reacciones adversas sistémicas graves por disfunción orgánica acumulativa (falla renal o hepática no dosificada) y la inducción de hipersensibilidad inmunológica[cite: 510, 513, 523, 538].",
      "Diferenciación estricta de pautas según la indicación terapéutica definida: esquemas para terapia empírica inicial (basada en sospecha epidemiológica), terapia dirigida (guiada por el antibiograma del patógeno aislado) o terapia profiláctica quirúrgica/médica[cite: 31, 34, 513]."
    ]
  },

  // CAPÍTULO 02
  's2-1': {
    points: [
      "Inhiben la síntesis de la pared celular bacteriana al unirse de forma irreversible a las PBPs (proteínas fijadoras de penicilina), bloqueando la transpeptidación y la formación del peptidoglicano[cite: 555].",
      "<strong>Naturales:</strong> <em>Streptococcus spp.</em>, <em>Treponema pallidum</em> (Sífilis), <em>Neisseria meningitidis</em> y anaerobios de la boca (<em>Fusobacterium</em>)[cite: 559, 560]. <strong>Aminopenicilinas:</strong> Suma <em>Enterococcus faecalis</em>, <em>Listeria monocytogenes</em> y Gram negativos (<em>H. influenzae, E. coli, P. mirabilis</em>)[cite: 561, 562]. <strong>Antiestafilocócicas:</strong> <em>Staphylococcus aureus</em> sensible a meticilina (SASM) y <em>Streptococcus spp.</em> [cite: 565]",
      "Provocan el debilitamiento estructural de la pared celular, lo que activa las autolisinas bacterianas, llevando a la lisis osmótica y muerte celular (Efecto bactericida)[cite: 566].",
      "Bactericidas dependientes del tiempo; el éxito terapéutico se correlaciona de forma directa con el porcentaje del intervalo de dosificación en que la concentración de fármaco libre permanece por encima de la CIM (%T > CIM)[cite: 567].",
      "Amplia distribución tisular (penetran mal a SNC, excepto con meninges inflamadas)[cite: 568]. Eliminación renal por filtración y secreción tubular activa, requiriendo ajuste de dosis en falla renal, excepto las antiestafilocócicas (nafcilina, oxacilina, dicloxacilina) que se eliminan por vía biliar/hepática[cite: 569]. Amoxicilina es estable en medio ácido y con alimentos; ampicilina y dicloxacilina requieren ayuno[cite: 570, 571].",
      "Reacciones de hipersensibilidad (desde rash hasta shock anafiláctico), diarrea (frecuente en amoxicilina/clavulanato), náuseas, nefritis intersticial inmunológica y neurotoxicidad (convulsiones) a dosis muy elevadas[cite: 572].",
      "<strong>Amoxicilina:</strong> 500 mg - 1g VO cada 8 h[cite: 575]. <strong>Ampicilina:</strong> 500 mg - 1 g VO/IV cada 6 h[cite: 576]. <strong>Dicloxacilina:</strong> 250 - 500 mg VO cada 6 h[cite: 577]. <strong>Penicilina G benzatínica:</strong> 1.2 - 2.4 millones UI IM dosis única o semanal[cite: 578]. <strong>Oxacilina/Nafcilina:</strong> 1-2g IV cada 4-6 h[cite: 579]. <strong>Amoxicilina/Clavulanato:</strong> 875/125 mg cada 12 h o 500/125 mg cada 8 h[cite: 581, 582]."
    ]
  },
  's2-2': {
    points: [
      "Inhiben irreversiblemente las enzimas betalactamasas bacterianas, actuando como adyuvantes suicidas que protegen al antibiótico betalactámico asociado de su degradación hidrolítica[cite: 586, 589].",
      "Bacterias Gram positivas y Gram negativas productoras de betalactamasas, ampliando el espectro original frente a <em>Staphylococcus aureus</em>, <em>Haemophilus influenzae</em>, <em>Escherichia coli</em> y <em>Klebsiella spp.</em> [cite: 592, 593, 595, 598]",
      "Neutralizan de forma directa las enzimas de resistencia bacteriana (betalactamasas), impidiendo la destrucción del anillo betalactámico y permitiendo que el antibiótico acompañante ejerza su acción bactericida[cite: 599].",
      "La farmacodinamia es indirecta y depende estrictamente del perfil bactericida dependiente del tiempo del betalactámico con el que se combinan[cite: 600].",
      "El ácido clavulánico posee una excelente biodisponibilidad por vía oral; el sulbactam y el tazobactam son agentes parenterales exclusivos[cite: 601]. Todos presentan una vía de eliminación mayoritariamente renal[cite: 602].",
      "Trastornos gastrointestinales frecuentes que incluyen diarrea y náuseas, riesgo de hepatotoxicidad transitoria y reacciones alérgicas de hipersensibilidad cutánea e inmunológica[cite: 603, 605, 606].",
      "<strong>Amoxicilina/Clavulanato:</strong> 875/125 mg VO cada 12 h[cite: 610, 611]. <strong>Ampicilina/Sulbactam:</strong> 1,5 - 3 g IV/IM cada 6 h[cite: 613, 614]. <strong>Piperacilina/Tazobactam:</strong> 4,5 g IV cada 6 h[cite: 615]."
    ]
  },
  's2-3': {
    points: [
      "Inhiben la síntesis de la pared celular bacteriana mediante la unión covalente y el bloqueo de las proteínas fijadoras de penicilina (PBPs) esenciales en la fase final de transpeptidación[cite: 617, 618].",
      "<strong>Cefalexina (1ª Gen):</strong> Actúa contra bacterias Gram positivas y algunos bacilos Gram negativos seleccionados[cite: 620, 621]. <strong>Ceftriaxona (3ª Gen):</strong> Presenta un amplio espectro enfocado en bacterias Gram negativas y cocos exigentes como <em>Neisseria spp.</em> [cite: 621, 623]",
      "Provocan una alteración estructural crítica de la arquitectura de la pared celular bacteriana que conduce a la inestabilidad osmótica, activación de autolisinas y lisis celular bacteriana rápida (Efecto bactericida)[cite: 626].",
      "Bactericidas dependientes del tiempo; la eficacia clínica de erradicación se correlaciona con el tiempo que el fármaco libre supera la CIM (%T > CIM)[cite: 627, 628].",
      "La cefalexina presenta una adecuada absorción por vía oral[cite: 629]. La ceftriaxona se administra exclusivamente por vía parenteral (IV o IM) y destaca por su excelente penetración a través de la barrera hematoencefálica hacia el líquido cefalorraquídeo (LCR)[cite: 630, 631].",
      "Reacciones alérgicas e hipersensibilidad, sobredesarrollo de patógenos oportunistas causando colitis por <em>Clostridioides difficile</em> y precipitación biliar (barro biliar) asociada al uso de ceftriaxona[cite: 632, 633, 634].",
      "<strong>Cefalexina:</strong> 250 - 500 mg VO cada 6 h[cite: 635]. <strong>Ceftriaxona:</strong> 1 - 2 g IV/IM cada 12 a 24 h[cite: 636]."
    ]
  },
  's2-4': {
    points: [
      "Inhiben la síntesis de la pared celular bacteriana en una etapa temprana al unirse de forma específica y con alta afinidad al extremo terminal D-Ala-D-Ala de los precursores del peptidoglicano, bloqueando la transglucosilación[cite: 64, 654, 657].",
      "Bacterias Gram positivas exclusivamente, incluyendo cepas multirresistentes como <em>Staphylococcus aureus</em> resistente a meticilina (SAMR), <em>Streptococcus spp.</em> y especies de <em>Enterococcus spp.</em> [cite: 637, 640, 641]",
      "Impiden de manera directa la polimerización y formación de la malla de peptidoglicano en la pared celular en desarrollo, desestabilizando la membrana y produciendo lisis bacteriana selectiva[cite: 644].",
      "Eficacia bactericida determinada por la relación de la farmacodinamia cuantitativa entre el Área Bajo la Curva y la CIM (AUC/CIM), requiriendo estrecho monitoreo terapéutico[cite: 645, 648].",
      "La vancomicina requiere administración por infusión intravenosa (IV) para infecciones sistémicas (la vía oral se limita a acción luminal), mientras que la teicoplanina permite administración IV e intramuscular (IM)[cite: 659, 661]. Ambos presentan eliminación renal[cite: 662].",
      "Riesgo documentado de nefrotoxicidad, ototoxicidad (auditiva y vestibular) y el desarrollo del síndrome del hombre rojo mediado por liberación histamínica si la infusión IV de vancomicina es rápida[cite: 666, 667].",
      "<strong>Vancomicina IV:</strong> 15 - 20 mg/kg cada 8 a 12 h[cite: 669, 670]. <strong>Vancomicina VO:</strong> 125 - 500 mg cada 6 h (acción intraluminal)[cite: 671, 672]. <strong>Teicoplanina:</strong> 6 - 12 mg/kg IV/IM[cite: 673, 674]."
    ]
  },

  // CAPÍTULO 03
  's3-1': {
    points: [
      "Se unen de forma reversible a la subunidad ribosomal 50S bacteriana, bloqueando el proceso de translocación del ARNm y deteniendo de forma prematura la síntesis de proteínas[cite: 675].",
      "Bacterias Gram positivas, cocobacilos Gram negativos seleccionados y patógenos intracelulares o atípicos (<em>Chlamydia spp., Mycoplasma pneumoniae, Legionella spp.</em>)[cite: 676]. La claritromicina posee actividad específica frente a <em>Helicobacter pylori</em>[cite: 676].",
      "Bloquean la elongación peptídica y la producción de proteínas esenciales requeridas para el crecimiento, metabolismo y replicación, ejerciendo un efecto bacteriostático[cite: 677, 678, 690].",
      "Antibióticos bacteriostáticos que poseen un marcado efecto posantibiótico (EPA); su éxito clínico se correlaciona con el índice farmacodinámico de la relación AUC/CIM[cite: 691, 692, 694].",
      "Presentan una adecuada absorción por vía oral y amplia distribución en tejidos[cite: 695, 696]. La azitromicina destaca por alcanzar elevadas concentraciones intracelulares y una vida media prolongada[cite: 699]. La eritromicina y claritromicina sufren metabolismo hepático activo[cite: 699].",
      "Efectos gastrointestinales (náuseas, diarrea, dolor abdominal), hepatotoxicidad, prolongación del intervalo QT e interacciones medicamentosas graves debido a la inhibición del citocromo CYP450 (eritromicina/claritromicina)[cite: 700].",
      "<strong>Azitromicina:</strong> 500 mg VO cada 24 h por 3 días[cite: 702]. <strong>Claritromicina:</strong> 250 - 500 mg VO cada 12 h[cite: 720]. <strong>Eritromicina:</strong> 250 - 500 mg VO cada 6 h[cite: 723]."
    ]
  },
  's3-2': {
    points: [
      "Se unen irreversiblemente a la subunidad ribosomal 30S bacteriana, provocando una lectura errónea del código genético en el ARNm y la consiguiente producción de proteínas defectuosas[cite: 703, 704, 706, 709].",
      "Principalmente activos frente a bacilos Gram negativos aerobios severos, incluyendo <em>Escherichia coli</em>, especies de <em>Klebsiella spp.</em> y <em>Pseudomonas aeruginosa</em>[cite: 712].",
      "Las proteínas defectuosas generadas se insertan y dañan de forma irreversible la integridad estructural de la membrana citoplasmática bacteriana, induciendo una rápida muerte celular (Efecto bactericida)[cite: 713, 715, 716].",
      "Agentes bactericidas potentes dependientes de la concentración, caracterizados por presentar un prolongado y significativo efecto posantibiótico[cite: 718, 719, 721, 724].",
      "No poseen absorción significativa por el tracto gastrointestinal (vía oral); requieren administración parenteral IV o IM[cite: 725, 727, 730]. Se distribuyen en el espacio extracelular y se eliminan sin cambios por filtración renal[cite: 733, 736].",
      "Presentan un estrecho margen terapéutico con riesgo elevado de nefrotoxicidad tubular reversible, ototoxicidad irreversible (coclear y vestibular) y riesgo de bloqueo neuromuscular agudo[cite: 741, 742, 747].",
      "<strong>Gentamicina / Tobramicina:</strong> 3 - 5 mg/kg IV o IM administrado una vez al día[cite: 749]. <strong>Amikacina:</strong> 15 mg/kg IV o IM una vez al día[cite: 750]. <strong>Estreptomicina:</strong> 15 mg/kg IM al día[cite: 751]. <strong>Neomicina:</strong> Uso exclusivamente tópico o VO para descontaminación luminal[cite: 752]."
    ]
  },
  's3-3': {
    points: [
      "Se unen de forma reversible a la subunidad ribosomal 30S bacteriana, impidiendo mecánicamente el acceso y unión del complejo aminoacil-ARNt al sitio aceptor del ribosoma, deteniendo la síntesis proteica[cite: 765].",
      "Amplio espectro que abarca bacterias Gram positivas, Gram negativas y microorganismos de crecimiento atípico o intracelular tales como <em>Chlamydia spp.</em>, <em>Rickettsia spp.</em> y <em>Borrelia burgdorferi</em>[cite: 753, 754].",
      "Bloquean de forma efectiva la incorporación de nuevos aminoácidos a las cadenas peptídicas en crecimiento, deteniendo el desarrollo bacteriano y ejerciendo un efecto bacteriostático[cite: 755, 756, 759].",
      "Agentes bacteriostáticos clásicos cuya respuesta terapéutica óptima está directamente ligada a la optimización de la relación entre el Área Bajo la Curva y la CIM (AUC/CIM)[cite: 761, 763].",
      "Buena absorción por vía oral, con una biodisponibilidad y vida media superior en el caso de la doxiciclina[cite: 767, 769, 774]. Su absorción se ve disminuida por la coadministración con lácteos, antiácidos y suplementos que contengan calcio o hierro debido a procesos de quelación[cite: 771, 772, 775].",
      "Intolerancia gastrointestinal, reacciones cutáneas de fotosensibilidad, tinción dental permanente por depósito de calcio y alteración reversible del crecimiento óseo; contraindicadas en gestantes y niños menores de 8 años[cite: 776, 777, 780, 781].",
      "<strong>Doxiciclina:</strong> 100 mg VO cada 12 h[cite: 783]. <strong>Tetraciclina:</strong> 250 - 500 mg VO cada 6 h[cite: 784, 785]."
    ]
  },

  // CAPÍTULO 04
  's4-1': {
    points: [
      "Inhiben de forma directa las enzimas bacterianas ADN girasa (topoisomerasa II) y topoisomerasa IV, bloqueando los procesos de replicación, transcripción, reparación y superenrollamiento del ADN bacteriano[cite: 786, 788, 789, 792, 793, 794].",
      "<strong>1ª Gen (Nalidíxico):</strong> Gram negativos urinarios (<em>E. coli, Proteus spp.</em>)[cite: 796, 797]. <strong>2ª Gen (Cipro):</strong> Gram negativos sistémicos y <em>Pseudomonas aeruginosa</em>[cite: 798, 801]. <strong>3ª Gen (Levo):</strong> Gram positivos y atípicos[cite: 806, 808]. <strong>4ª Gen (Moxi):</strong> Anaerobios y patógenos respiratorios[cite: 810, 811].",
      "El bloqueo de las topoisomerasas estabiliza los complejos de corte enzimático, induciendo roturas dobles e irreversibles en las hebras del cromosoma bacteriano, gatillando la muerte celular (Efecto bactericida)[cite: 812].",
      "Bactericidas potentes dependientes de la concentración; su éxito clínico y prevención de resistencia se correlacionan con los índices Cmáx/CIM y AUC/CIM[cite: 813].",
      "Excelente biodisponibilidad por vía oral y una muy amplia penetración en fluidos y tejidos corporales[cite: 814]. Su absorción digestiva disminuye de forma drástica si se administra junto con lácteos, antiácidos o suplementos minerales multivalentes[cite: 815, 816].",
      "Trastornos gastrointestinales, cefalea, mareos, prolongación del intervalo QT, riesgo de tendinitis y ruptura del tendón de Aquiles; contraindicadas formalmente en niños, adolescentes y durante el embarazo[cite: 816, 817].",
      "<strong>Ciprofloxacino:</strong> 250 - 750 mg VO o 200 - 400 mg IV cada 12 h[cite: 820]. <strong>Levofloxacino:</strong> 500 - 750 mg VO/IV cada 24 h[cite: 821]. <strong>Moxifloxacino:</strong> 400 mg VO/IV cada 24 h[cite: 822]."
    ]
  },
  's4-2': {
    points: [
      "Las sulfonamidas inhiben competitivamente a la dihidropteroato sintasa bloqueando la síntesis de ácido fólico[cite: 826, 828]. La trimetoprima inhibe a la dihidrofolato reductasa impidiendo la producción de tetrahidrofolato metabólico[cite: 828, 829].",
      "Bacterias Gram positivas, Gram negativas, y microorganismos específicos como <em>Pneumocystis jirovecii</em>, especies de <em>Nocardia spp.</em> y el protozoario <em>Toxoplasma gondii</em>[cite: 830, 831].",
      "Por separado actúan como agentes bacteriostáticos; al combinarse en proporciones sinérgicas fijas (<em>cotrimoxazol</em>) inducen un bloqueo secuencial letal en la ruta biosintética del folato, volviéndose bactericidas[cite: 832, 833].",
      "El cotrimoxazol ejerce un efecto bactericida sinérgico dependiente del tiempo, correlacionándose de forma directa con el mantenimiento de los niveles séricos por encima de la CIM[cite: 834, 835].",
      "Rápida y completa absorción por vía oral con una excelente distribución sistémica que incluye una adecuada penetración al sistema nervioso central (líquido cefalorraquídeo)[cite: 837, 838, 839, 842]. Su eliminación es principalmente por vía renal[cite: 842, 843].",
      "Reacciones dermatológicas graves de hipersensibilidad como el síndrome de Stevens-Johnson (SSJ) y NET, nefrotoxicidad por cristaluria, hiperpotasemia, y riesgo de hemólisis aguda en pacientes con deficiencia de G6PD[cite: 846, 847, 848, 850, 851, 852].",
      "<strong>Cotrimoxazol (SMX/TMP):</strong> 800/160 mg VO o IV cada 12 h[cite: 856]. En infecciones severas por <em>Pneumocystis jirovecii</em> se dosifica a razón de 15 - 20 mg/kg/día (basado en el componente TMP) dividido cada 6 h[cite: 856]."
    ]
  },
  's4-3': {
    points: [
      "Inhibe de forma específica y reversible a la enzima ARN polimerasa dependiente de ADN bacteriana, bloqueando el inicio de la polimerización y la transcripción de los genes bacterianos[cite: 878].",
      "Altamente efectiva contra micobacterias (<em>Mycobacterium tuberculosis</em>, <em>Mycobacterium leprae</em>), cocos Gram positivos (<em>Staphylococcus aureus</em>) y cocos Gram negativos (<em>Neisseria meningitidis</em>)[cite: 845, 858].",
      "Suprime por completo la síntesis de ARN mensajero bacteriano, deteniendo de golpe la producción de proteínas esenciales y celulares, lo que resulta en la muerte del patógeno (Efecto bactericida)[cite: 858, 861, 862].",
      "Ejerce una potente actividad bactericida que es estrictamente dependiente de la concentración; su parámetro farmacodinámico predictor es el índice AUC/CIM[cite: 865, 868, 869, 871].",
      "Excelente absorción por vía digestiva y amplia difusión tisular, atravesando la barrera hematoencefálica[cite: 872, 874, 877]. Sufre un metabolismo de desacetilación parcial en el hígado y posee una ruta de eliminación predominantemente biliar[cite: 879].",
      "Hepatotoxicidad severa, desarrollo de un síndrome pseudogripal inmunológico y coloración naranja-rojiza inocua de fluidos corporales (orina, lágrimas, sudor)[cite: 884, 885, 887, 889, 890, 891]. Es un potente inductor de las enzimas hepáticas CYP450[cite: 892, 894].",
      "<strong>Rifampicina:</strong> 10 mg/kg VO una vez al día en ayunas (la dosis estándar habitual en adultos es de 600 mg VO cada 24 h)[cite: 896]."
    ]
  },

  // CAPÍTULO 05
  's5-1': {
    points: [
      "<strong>Nitrofurantoína:</strong> Reducida por flavoproteínas bacterianas a metabolitos intermedios altamente reactivos que dañan de forma inespecífica el ADN, ARN y proteínas[cite: 902, 903, 905]. <strong>Metenamina:</strong> Se hidroliza químicamente en medio ácido liberando formaldehído desnaturalizante[cite: 907]. <strong>Fenazopiridina:</strong> Analgésico azoico local de la mucosa urinaria (sin acción antibacteriana)[cite: 163, 913].",
      "Patógenos comunes del tracto urinario bajo, incluyendo cepas de <em>Escherichia coli</em>, <em>Staphylococcus saprophyticus</em>, especies de <em>Enterococcus spp.</em> y <em>Klebsiella pneumoniae</em>[cite: 910, 914]. No poseen utilidad clínica en infecciones altas como pielonefritis[cite: 915].",
      "La nitrofurantoína y el formaldehído libre de la metenamina provocan múltiples roturas e inactivación molecular irreversible local, induciendo lisis bacteriana y un efecto bactericida urinario[cite: 915].",
      "Bactericidas locales en orina; la nitrofurantoína optimiza su espectro en pH urinario bajo, mientras que la metenamina requiere obligatoriamente un pH urinario menor a 5.5 para poder hidrolizarse[cite: 918, 919, 921].",
      "Presentan una rápida absorción por vía oral y concentran de forma masiva y casi exclusiva en el espacio urinario intraluminal[cite: 922]. La nitrofurantoína pierde eficacia y acumula toxicidad sistémica si la filtración renal cae[cite: 923].",
      "Molestias gastrointestinales, fibrosis pulmonar intersticial severa, neuropatía periférica motora y sensitiva, y anemia hemolítica[cite: 925, 927, 928]. La fenazopiridina tiñe la orina de un color rojo-anaranjado intenso[cite: 928].",
      "<strong>Nitrofurantoína:</strong> 100 mg VO cada 12 h por 5 días para cistitis aguda[cite: 930]. Esquema profiláctico de recurrencias: 50 - 100 mg VO por las noches[cite: 931]. <strong>Fenazopiridina:</strong> 100 - 200 mg VO cada 8 h (restringido a un uso máximo de 2 días)[cite: 932]."
    ]
  },
  's5-2': {
    points: [
      "<strong>Clindamicina / Cloranfenicol:</strong> Inhiben la síntesis de proteínas al unirse de forma reversible a la subunidad ribosomal 50S bacteriana[cite: 970, 972]. <strong>Polimixinas (Colistina):</strong> Detergentes catiónicos que rompen la membrana externa Gram negativa[cite: 179, 933]. <strong>Ácido fusídico:</strong> Bloquea el factor de elongación EF-G[cite: 936, 937].",
      "<strong>Clindamicina:</strong> Gram positivos y anaerobios estrictos[cite: 943, 948]. <strong>Cloranfenicol:</strong> Amplio espectro (Gram positivos, negativos, anaerobios)[cite: 950]. <strong>Polimixinas:</strong> Bacilos Gram negativos multirresistentes (<em>Pseudomonas aeruginosa</em>, <em>Acinetobacter baumannii</em>)[cite: 959, 960, 963, 964]. <strong>Ácido fusídico:</strong> <em>Staphylococcus aureus</em>[cite: 968, 973].",
      "Clindamicina, cloranfenicol y ácido fusídico detienen la elongación y el crecimiento bacteriano de forma bacteriostática[cite: 974, 977, 978]. Las polimixinas destruyen la integridad osmótica de la membrana bacteriana, provocando una lisis y muerte celular bactericida rápida[cite: 978, 980, 981].",
      "Polimixinas son agentes bactericidas potentes dependientes de la concentración[cite: 984, 986]. Clindamicina, cloranfenicol y ácido fusídico ejercen una actividad que depende del tiempo de exposición sérica[cite: 989, 990].",
      "Clindamicina destaca por su excelente penetración en tejido óseo y partes blandas[cite: 993, 995]. El cloranfenicol posee una sobresaliente capacidad para atravesar la barrera hematoencefálica hacia el SNC[cite: 996]. La colistina se elimina por vía renal, y el ácido fusídico tiene eliminación biliar[cite: 997, 999].",
      "<strong>Clindamicina:</strong> Alto riesgo de colitis pseudomembranosa por <em>Clostridioides difficile</em>[cite: 1001, 1003]. <strong>Cloranfenicol:</strong> Toxicidad hematológica grave (depresión medular dose-dependiente y anemia aplásica irreversible)[cite: 1005, 1006]. <strong>Polimixinas:</strong> Elevada nefrotoxicidad tubular y neurotoxicidad[cite: 1007].",
      "<strong>Clindamicina:</strong> 150 - 450 mg VO cada 6 a 8 h, o 600 - 900 mg IV cada 8 h[cite: 1014, 1015, 1016]. <strong>Cloranfenicol:</strong> 50 mg/kg/día dividido por vía VO o IV[cite: 1017]. <strong>Colistina:</strong> Dosis de carga de 9 millones UI IV, seguida de un mantenimiento de 4.5 millones UI IV cada 12 h[cite: 1018]. <strong>Ácido fusídico:</strong> 500 mg VO cada 8 h[cite: 1019]."
    ]
  },

  // CAPÍTULO 06
  's6-1': {
    points: [
      "Interfieren con vías metabólicas vitales del parásito intracelular; la cloroquina bloquea la polimerización del grupo hemo citotóxico; las artemisininas generan radicales libres endoperóxidos; la primaquina altera el transporte electrónico de la mitocondria[cite: 1020, 1040, 1056].",
      "Especies del género <em>Plasmodium</em> (<em>P. falciparum</em>, <em>P. vivax</em>, <em>P. ovale</em>, y <em>P. malariae</em>)[cite: 1025]. La primaquina erradica de forma selectiva los estadios latentes tisulares hepáticos (hipnozoítos)[cite: 1026, 1040].",
      "Destruyen selectivamente las formas parasitarias alojadas en los eritrocitos o en los hepatocitos humanos, deteniendo de forma abrupta el ciclo esquizogónico y previniendo las recaídas clínicas de la enfermedad[cite: 1032, 1038, 1040].",
      "Los esquizonticidas sanguíneos actúan eliminando las formas intraeritrocitarias causantes de los síntomas, mientras que los esquizonticidas tisulares (primaquina) destruyen las formas hepáticas latentes[cite: 1035, 1038, 1040].",
      "Adecuada absorción por vía oral; la cloroquina posee un inmenso volumen aparente de distribución y una vida media extremadamente prolongada en tejidos[cite: 1046, 1047]. El artesunato permite la vía IV de rescate en malaria grave[cite: 1049].",
      "Cloroquina se asocia a prurito intenso y retinopatía irreversible dose-dependiente[cite: 1051]. Primaquina causa crisis de hemólisis intravascular aguda severa en pacientes con deficiencia de G6PD[cite: 1052, 1053]. Quinina produce cinchonismo sistémico e hipoglucemia severa[cite: 1055, 1057]." ,
      "<strong>Cloroquina:</strong> 1000 mg VO como dosis inicial de carga, seguido de 500 mg administrados a las 6, 24 y 48 h posteriores[cite: 1067, 1068]. <strong>Primaquina:</strong> 15 mg VO al día por un periodo estricto de 14 días[cite: 1069, 1070]. <strong>Artesunato IV:</strong> 2.4 mg/kg aplicado a las 0, 12 y 24 h[cite: 1075, 1076]."
    ]
  },
  's6-2': {
    points: [
      "Inducen toxicidad selectiva mediante diversos mecanismos: el metronidazol y tinidazol forman intermedios reducidos reactivos que rompen el ADN helicoidal [cite: 1088]; la nitazoxanida interfiere con la transferencia de electrones; la paromomicina altera la traducción ribosomal[cite: 1088, 1089].",
      "Protozoarios patógenos clínicos diversos que incluyen <em>Entamoeba histolytica</em>, <em>Giardia lamblia</em>, <em>Trichomonas vaginalis</em>, <em>Trypanosoma cruzi</em> (Chagas) y especies de <em>Leishmania spp.</em> [cite: 1060, 1061, 1062, 1064]",
      "Provocan la disrupción de las hebras de material genético, la inhibición irreversible de las enzimas metabólicas respiratorias o la lisis celular directa, conduciendo a la muerte del protozoario[cite: 1074].",
      "Agentes con actividad parasiticida directa y potente, cuya velocidad de erradicación clínica demuestra ser dependiente de la concentración lograda en los tejidos o la luz intestinal[cite: 1077].",
      "El metronidazol y el tinidazol poseen una excelente y rápida absorción oral logrando una óptima penetración en la mayoría de tejidos y el SNC[cite: 1081, 1083]. La paromomicina es un aminoglucósido no absorbible que actúa localmente en la luz intestinal[cite: 1090, 1091].",
      "Metronidazol produce sabor metálico persistente y efecto disulfiram si se ingiere alcohol[cite: 1093, 1094, 1095]. Benznidazol/Nifurtimox causan neuropatía periférica dolorosa y dermatitis[cite: 1097, 1098]. Antimoniales (Glucantime) causan cardiotoxicidad (prolongación QT) y pancreatitis aguda[cite: 1099, 1100, 1101]. Melarsoprol causa encefalopatía reactiva letal[cite: 1103].",
      "<strong>Metenidazol:</strong> 500 - 750 mg VO cada 8 h por un ciclo de 7 a 10 días[cite: 1106]. <strong>Nitazoxanida:</strong> 500 mg VO cada 12 h por 3 días[cite: 1107, 1108]. <strong>Paromomicina:</strong> 25 - 35 mg/kg/día VO dividido por 7 días[cite: 1110]. <strong>Glucantime:</strong> 20 mg/kg/día IV o IM continuo por un periodo de 20 a 30 días[cite: 1111]."
    ]
  },
  's6-3': {
    points: [
      "<strong>Benzimidazoles (Albendazol):</strong> Inhiben de forma selectiva la polimerización de la beta-tubulina del helminto, desestructurando los microtúbulos[cite: 228, 229, 230, 231, 1129]. <strong>Ivermectina:</strong> Abre canales de cloro regulados por glutamato[cite: 256, 1129]. <strong>Prazicuantel:</strong> Eleva la permeabilidad celular al calcio generando contracción tetánica[cite: 263, 283].",
      "Nemátodos, céstodos y trématodos intestinales y tisulares, incluyendo de forma común infecciones por <em>Ascaris lumbricoides</em>, estadios de <em>Taenia solium</em>, especies de <em>Schistosoma spp.</em> y <em>Fasciola hepatica</em>[cite: 1113].",
      "Interrumpen el transporte de glucosa celular y la neurotransmisión o inducen una parálisis neuromuscular masiva, lo que provoca la muerte del parásito o su desprendimiento y expulsión mecánica del tubo digestivo[cite: 1114, 1117].",
      "Se dividen claramente según su efecto sobre la musculatura del helminto: inductores de parálisis espástica rígida (pamoato de pirantel, prazicuantel) o inductores de parálisis flácida completa (ivermectina)[cite: 1118].",
      "Muchos de estos agentes se caracterizan por poseer una baja e ineficaz absorción sistémica intestinal[cite: 1131]. Sin embargo, la absorción del albendazol aumenta significativamente si se coadministra junto con alimentos de alto contenido graso[cite: 1131]." ,
      "Los benzimidazoles pueden inducen hepatotoxicidad transitoria y supresión de la médula ósea en tratamientos largos[cite: 1132, 1133]. La ivermectina puede gatillar la reacción inmunológica de Mazzotti en filariasis[cite: 1135]. El prazicuantel induce inflamación focal severa en neurocisticercosis activa[cite: 1136, 1137].",
      "<strong>Albendazol:</strong> 400 mg VO como dosis única estándar en parasitosis intestinales; en protocolos de neurocisticercosis tisular se calcula a 15 mg/kg/día[cite: 1139]. <strong>Mebendazol:</strong> 100 mg VO cada 12 h por 3 días, o bien una dosis única concentrada de 500 mg[cite: 1149, 1150]. <strong>Prazicuantel:</strong> 5 - 10 mg/kg VO en toma única para teniasis[cite: 1151]. <strong>Ivermectina:</strong> 200 $\mu\text{g/kg}$ VO administrado como dosis única[cite: 1152]."
    ]
  },

  // CAPÍTULO 07
  's7-1': {
    points: [
      "<strong>Azoles (Fluco/Keto):</strong> Inhiben de manera selectiva a la enzima fúngica 14-alfa-desmetilasa, bloqueando la conversión de lanosterol a ergosterol[cite: 268, 270, 1155]. <strong>Poliénicos (Anfotericina B/Nistatina):</strong> Se unen directamente al ergosterol estructural de la membrana fúngica[cite: 295, 1156]. <strong>Alilaminas (Terbinafina):</strong> Inhiben la escualeno epoxidasa[cite: 298, 1142, 1146].",
      "<strong>Azoles:</strong> Especies de <em>Candida spp.</em>, <em>Cryptococcus neoformans</em> y dermatofitos[cite: 1157, 1158, 1159]. <strong>Anfotericina B:</strong> Amplio espectro frente a micosis sistémicas profundas y oportunistas[cite: 1160, 1161, 1163]. <strong>Nistatina:</strong> Exclusivo para candidiasis mucocutánea superficial[cite: 1164, 1165]. <strong>Terbinafina:</strong> Dermatofitos (<em>Trichophyton, Microsporum, Epidermophyton</em>)[cite: 1166, 1170].",
      "Los azoles y alilaminas desestructuran la fluidez de la membrana por depleción de esteroles[cite: 1171]. La anfotericina y nistatina forman poros acuosos masivos intermembrana que causan la pérdida transmembrana de iones esenciales, induciendo la muerte del hongo[cite: 295, 1156, 1171].",
      "Los compuestos azólicos y las alilaminas actúan principalmente como fungistáticos [cite: 1172]; la anfotericina B y nistatina actúan de forma directa como potentes fungicidas biológicos[cite: 1173].",
      "El fluconazol posee una excelente biodisponibilidad oral y una óptima penetración hacia el SNC a través de la barrera hematoencefálica[cite: 1175, 1176, 1177]. El ketoconazol oral requiere obligatoriamente un pH gástrico sumamente ácido para disolverse y absorberse[cite: 1178, 1182, 1184]. La anfotericina B requiere infusión IV central selectiva[cite: 1187, 1190]. La terbinafina es lipofílica y se acumula masivamente en estrato córneo, piel y uñas[cite: 1191, 1192].",
      "Los azoles se asocian a hepatotoxicidad dose-dependiente y prolongación del intervalo QT [cite: 1195, 1196]; el ketoconazol bloquea la síntesis de esteroides humanos causando ginecomastia, impotencia y trastornos endocrinos[cite: 1197, 1198, 1200]. La anfotericina B causa nefrotoxicidad severa dose-limitante, hipopotasemia marcada y reacciones febriles durante la infusión[cite: 1201, 1202, 1205].",
      "<strong>Fluconazol:</strong> 200 - 400 mg VO/IV diarios; cistitis o candidiasis vaginal: 150 mg VO dosis única[cite: 1179]. <strong>Anfotericina B deoxicolato:</strong> 0.5 - 1.5 mg/kg/día mediante infusión intravenosa lenta[cite: 1181]. <strong>Nistatina:</strong> 400 000 - 600 000 UI en suspensión VO aplicado 4 veces al día (acción tópica luminal)[cite: 1189]. <strong>Terbinafina:</strong> 250 mg VO una vez al día[cite: 1191]."
    ]
  },
  's7-2': {
    points: [
      "Inhibe de forma específica la polimerización y ensamblaje de los microtúbulos intracelulares fúngicos al unirse al dominio de la tubulina, provocando la disrupción del huso mitótico durante la metafase celular[cite: 307, 1213, 1214].",
      "Especies de hongos dermatofitos superficiales queratinofílicos, afectando de forma exclusiva a los géneros <em>Trichophyton</em>, <em>Microsporum</em> y <em>Epidermophyton</em>[cite: 1216, 1227].",
      "Detiene por completo la mitosis y el proceso de división celular fúngica, impidiendo mecánicamente la colonización e invasión hacia las nuevas estructuras de tejido queratinizado en formación[cite: 1217, 1218].",
      "Ejerce una actividad de tipo fungistática clásica de superficie; su eficacia clínica en el aclaramiento fúngico depende de mantener concentraciones séricas estables y prolongadas en los tejidos diana[cite: 1218, 1219].",
      "Demuestra una absorción gastrointestinal moderada que se ve incrementada significativamente si se administra en conjunto con alimentos con alto contenido graso[cite: 1220]. Posee una alta afinidad por la queratina, acumulándose de forma selectiva en la epidermis, cabello y la matriz ungueal[cite: 1220].",
      "Cefaleas intensas frecuentes al inicio del tratamiento, náuseas, mareos, reacciones cutáneas de fotosensibilidad; actúa como un potente inductor enzimático hepático, reduciendo la eficacia clínica de anticonceptivos orales y anticoagulantes[cite: 1221].",
      "<strong>Griseofulvina microcristalina:</strong> 500 - 1000 mg VO al día, fraccionada o en toma única diaria, administrada siempre junto con comidas grasas[cite: 1223]. La duración varía de 4 a 8 semanas según el sitio anatómico de la tiña[cite: 1224, 1225]."
    ]
  },

  // CAPÍTULO 08
  's8-1': {
    points: [
      "<strong>Aciclovir / Ganciclovir:</strong> Inhiben la ADN polimerasa viral de forma competitiva tras ser fosforilados, induciendo la terminación prematura de la cadena de ADN[cite: 1239]. <strong>Amantadina:</strong> Bloquea de forma selectiva el canal iónico de la proteína M2 del virus de la influenza[cite: 330, 1228, 1229]. <strong>Ribavirina:</strong> Inhibe de forma competitiva la ARN polimerasa viral[cite: 331, 1234].",
      "<strong>Aciclovir:</strong> Virus del herpes simple tipo 1 y 2 (VHS-1, VHS-2) y virus de varicela-zóster (VVZ)[cite: 1242, 1243, 1244]. <strong>Ganciclovir:</strong> Citomegalovirus humano (CMV)[cite: 1245, 1246]. <strong>Amantadina:</strong> Exclusivo para Influenza tipo A[cite: 1247]. <strong>Ribavirina:</strong> Virus sincitial respiratorio (VSR) y virus de la Hepatitis C[cite: 1248, 1250].",
      "Inactivan de forma directa los complejos de replicación genómica viral o impiden los procesos críticos de pérdida de la cubierta proteica o desnudamiento del virión dentro de la célula huésped, frenando la expansión infecciosa[cite: 330, 1254].",
      "Actúan de manera selectiva como potentes agentes virustáticos; el aciclovir y el ganciclovir demuestran una elevada selectividad tisular ya que su primera fosforilación depende estrictamente de enzimas cinasas codificadas por el propio virus[cite: 1254, 1256, 1257].",
      "El aciclovir presenta una biodisponibilidad oral moderada, amplia distribución en fluidos y excreción inalterada por filtración renal[cite: 1258, 1260, 1271]. El ganciclovir sistémico requiere administración intravenosa (IV) selectiva y depuración renal[cite: 1272, 1273]. El interferón requiere inyección subcutánea (SC) o intramuscular (IM)[cite: 1276, 1277].",
      "<strong>Aciclovir:</strong> Nefrotoxicidad por precipitación cristalina intratubular si se infunde rápido, y neurotoxicidad reversible[cite: 1279]. <strong>Ganciclovir:</strong> Mielosupresión severa dosis-limitante (leucopenia, neutropenia)[cite: 1281, 1282]. <strong>Ribavirina:</strong> Anemia hemolítica marcada y elevado potencial teratogénico selectivo[cite: 1285, 1286, 1289].",
      "<strong>Aciclovir:</strong> 200 - 400 mg VO administrado 5 veces al día para herpes; en encefalitis herpética severa se eleva a 10 mg/kg IV cada 8 h[cite: 1265, 1266]. <strong>Ganciclovir:</strong> 5 mg/kg IV infundido cada 12 h[cite: 1267]. <strong>Ribavirina:</strong> 1000 - 1200 mg VO al día dividido en dosis[cite: 1268]. <strong>Peginterferón alfa-2a:</strong> 180 $\mu\text{g}$ SC aplicado una vez por semana[cite: 1269, 1270]."
    ]
  },
  's8-2': {
    points: [
      "<strong>Zidovudina (AZT):</strong> Inhibidor nucleósido de la transcriptasa inversa (ITIAN) que se incorpora al ADN proviral bloqueando la elongación[cite: 318, 1310, 1312, 1313]. <strong>Saquinavir:</strong> Inhibidor competitivo selectivo de la enzima proteasa del VIH[cite: 320, 1314].",
      "Actúan de manera dirigida y exclusiva contra los retrovirus de la inmunodeficiencia humana tipo 1 y tipo 2 (VIH-1 y VIH-2)[cite: 1292, 1294].",
      "El AZT aborta por completo la transcripción del ARN viral a ADN proviral[cite: 1297]. El saquinavir impide la ruptura de las poliproteínas Gag-Pol, bloqueando la maduración estructural de los viriones liberados, produciendo partículas virales no infecciosas[cite: 1300, 1302, 1304, 1316].",
      "Provocan una disminución drástica y sostenida de la carga viral plasmática y permiten la recuperación y aumento progresivo en el recuento de linfocitos T CD4+[cite: 1318, 1320]. Se administran en esquemas de Terapia Antirretroviral Combinada (TARV)[cite: 1321].",
      "La zidovudina (AZT) posee una excelente absorción digestiva por vía oral y destaca por lograr una adecuada penetración al SNC superando la barrera hematoencefálica[cite: 1323, 1324]. El saquinavir posee una baja biodisponibilidad oral intrínseca, requiriendo coadministración con ritonavir como potenciador farmacocinético[cite: 1325, 1326, 1327].",
      "<strong>Zidovudina (AZT):</strong> Toxicidad mitocondrial expresada como anemia severa macrocítica, neutropenia profunda y desarrollo de miopatías crónicas[cite: 1333]. <strong>Saquinavir:</strong> Síndrome de lipodistrofia periférica, hiperlipidemia marcada y resistencia a la insulina[cite: 1334, 1335].",
      "<strong>Zidovudina (AZT):</strong> 300 mg VO cada 12 h[cite: 1338]. <strong>Saquinavir potenciado:</strong> 1000 mg de Saquinavir combinado con 100 mg de Ritonavir por vía VO, administrado cada 12 h junto con comidas[cite: 1339, 1340, 1341]."
    ]
  },

  // CAPÍTULO 09
  's9-1': {
    points: [
      "<strong>Ciclosporina / Tacrolimus:</strong> Se unen a inmunofilinas intracelulares (ciclofilina y FKBP respectivamente), formando un complejo que inhibe de forma selectiva a la calcineurina celular, bloqueando la transcripción de la IL-2[cite: 323, 1346]. <strong>Interferón alfa:</strong> Activa la cascada de señalización JAK-STAT intracelular[cite: 1347].",
      "Prevención inmunológica del rechazo agudo y crónico de aloinjertos de órganos sólidos y control de patologías autoinmunes severas[cite: 1351]. El interferón alfa se indica en Hepatitis crónica B y C y condilomatosis por VPH[cite: 1352]. Carecen de acción antimicrobiana directa[cite: 1350, 1356].",
      "Los inmunosupresores suprimen la transcripción citocínica deteniendo la proliferación clonal de linfocitos T colaboradores[cite: 1358]. Los inmunoestimulantes potencian la expresión de antígenos e incrementan la actividad citotóxica defensiva de las células NK y macrófagos[cite: 1359, 1360].",
      "Los inhibidores de la calcineurina suprimen selectivamente la inmunidad celular mediada por células T[cite: 1362, 1363]. El interferón alfa despliega potentes propiedades antiproliferativas, antivirales indirectas y reguladoras de la respuesta inmune[cite: 1367, 1368, 1370, 1372].",
      "La ciclosporina y el tacrolimus demuestran una absorción oral errática e incompleta, sufren un extenso metabolismo hepático oxidativo a través de la vía enzimática del CYP3A4 y poseen eliminación biliar[cite: 1374, 1376, 1377, 1379]. Exigen un monitoreo estrecho de los niveles séricos[cite: 1380, 1400].",
      "Ambos fármacos comparten una marcada nefrotoxicidad dose-dependiente, neurotoxicidad (temblores), hiperglucemia e hiperpotasemia severa[cite: 1386, 1387, 1388, 1389]. La ciclosporina induce hiperplasia gingival densa e hirsutismo[cite: 1391, 1392]. El tacrolimus causa alopecia y diabetes postrasplante[cite: 1393, 1395].",
      "<strong>Ciclosporina:</strong> 2.5 - 15 mg/kg/día VO, fraccionada estrictamente en dos dosis diarias[cite: 1407, 1408]. <strong>Tacrolimus:</strong> 0.1 - 0.2 mg/kg/día VO repartido cada 12 h[cite: 1410, 1411]. <strong>Peginterferón alfa-2a:</strong> 180 $\mu\text{g}$ SC inyectado una vez por semana[cite: 1412, 1413]."
    ]
  },

  // CAPÍTULO 10
  's10-1': {
    points: [
      "<strong>Eritropoyetina:</strong> Activa el receptor de eritropoyetina en progenitores eritroides de la médula ósea[cite: 1429]. <strong>Filgrastim:</strong> Factor de crecimiento recombinante que estimula los receptores G-CSF[cite: 361, 1430]. <strong>Hierro / Vitaminas (B9, B12):</strong> Aportan sustratos esenciales para la síntesis molecular de hemoglobina y la replicación del ADN celular[cite: 1431, 1433, 1437].",
      "<strong>Eritropoyetina:</strong> Anemia severa secundaria a insuficiencia renal crónica o inducida por quimioterapia antineoplásica[cite: 1440, 1441, 1442]. <strong>Filgrastim:</strong> Neutropenia febril o severa secundaria a quimioterapia cytotóxica[cite: 1443, 1445, 1446]. <strong>Hierro y Vitaminas:</strong> Anemias de tipo ferropénica y anemias megaloblásticas[cite: 1447, 1451].",
      "Estimulan de forma directa la celularidad y los procesos de maduración mitótica dentro de la médula ósea humana o reponen de forma inmediata los almacenes bioquímicos deficitarios, normalizando los recuentos celulares periféricos[cite: 1452].",
      "La eritropoyetina induce un incremento medible en el recuento periférico de reticulocitos tras un periodo de 7 a 10 días[cite: 1454]. El filgrastim induce una elevación drástica y de rápida aparición en el recuento absoluto de neutrófilos[cite: 1456, 1458].",
      "La eritropoyetina y el filgrastim requieren administración parenteral por vía subcutánea (SC) o intravenosa (IV)[cite: 1462, 1463]. El hierro por vía oral optimiza su transporte y absorción digestiva ante la presencia conjunta de vitamina C[cite: 1464, 1465, 1466]. La vitamina B12 requiere la unión obligatoria al factor intrínseco gástrico[cite: 1467, 1468].",
      "<strong>Eritropoyetina:</strong> Exacerbación de hipertensión arterial sistémica y eventos trombóticos vasculares mayores[cite: 1470, 1471]. <strong>Filgrastim:</strong> Dolor óseo de tipo medular de intensidad moderada a severa[cite: 1472]. <strong>Hierro oral:</strong> Estreñimiento severo y coloración oscura fecal[cite: 1473, 1474].",
      "<strong>Eritropoyetina:</strong> 50 - 100 UI/kg SC o IV administrado 3 veces por semana[cite: 1482, 1483]. <strong>Filgrastim:</strong> 5 $\mu\text{g/kg/día}$ SC o IV diario[cite: 1484]. <strong>Sulfato ferroso:</strong> 150 - 200 mg al día de hierro elemental neto calculado por vía VO[cite: 1485, 1486]. <strong>Cianocobalamina (B12):</strong> 1000 $\mu\text{g}$ IM de acuerdo al esquema clínico[cite: 374, 1487, 1488]."
    ]
  },

  // CAPÍTULO 11
  's11-1': {
    points: [
      "<strong>Somatotropina:</strong> Activa receptores de hormona de crecimiento transmembrana acoplados a la vía kinasa JAK/STAT, induciendo la expresión de la hormona somatomedina C o IGF-1[cite: 1500]. <strong>Leuprolida:</strong> Agonista del receptor de GnRH que desensibiliza de forma continua el eje gonadotropo[cite: 434, 1501, 1523].",
      "<strong>Somatotropina:</strong> Retraso del crecimiento longitudinal por déficit endógeno de GH, síndrome de Turner y Prader-Willi[cite: 1505, 1506, 1507]. <strong>Leuprolida:</strong> Tratamiento avanzado de cáncer de próstata metastásico, endometriosis y pubertad precoz[cite: 1509, 1510]. Carecen de acción antimicrobiana[cite: 1493, 1495, 1504].",
      "La somatotropina promueve de manera directa el crecimiento y la condrogénesis en las placas epifisarias de los huesos largos[cite: 1513]. El leuprolida suprime de forma profunda y química la producción gonadal de testosterona y estrógenos[cite: 1514, 1515].",
      "La hormona de crecimiento humana ejerce potentes efectos metabólicos sistémicos de tipo anabólico y lipolítico[cite: 1517]. Los análogos de GnRH provocan un efecto de estimulación inicial transitorio (&quot;flare&quot;) seguido de una supresión hormonal profunda duradera[cite: 1518, 1519, 1520].",
      "Requieren administración parenteral estricta mediante inyecciones subcutáneas (SC) o intramusculares (IM)[cite: 1522]. El desarrollo de formulaciones de liberación prolongada (&quot;depot&quot;) permite una dosificación espaciada[cite: 1524, 1526, 1528].",
      "Somatotropina se asocia al desarrollo de edema periférico moderado, artralgias y riesgo de hiperglucemia transitoria[cite: 1538, 1539]. El leuprolida gatilla sofocos intensos, osteoporosis ósea acelerada y pérdida marcada de la libido[cite: 1551, 1552].",
      "<strong>Somatotropina:</strong> 0.16 - 0.24 mg/kg/semana administrado por vía SC dividido en tomas[cite: 1554, 1555]. <strong>Leuprolida depot:</strong> 3.75 mg SC o IM aplicado con una periodicidad mensual regular[cite: 1556]."
    ]
  },
  's11-2': {
    points: [
      "<strong>Levotiroxina (T4):</strong> Profármaco de origen sintético que se desyoda periféricamente a la forma activa triyodotironina (T3), uniéndose a receptores nucleares tiroideos y modificando la transcripción génica celular[cite: 1561, 1563, 1564]. <strong>Metimazol / PTU:</strong> Inhiben de manera directa la enzima peroxidasa tiroidea[cite: 435, 1565].",
      "<strong>Levotiroxina:</strong> Terapia hormonal sustitutiva en hipotiroidismo de cualquier etiología[cite: 1541]. <strong>Metimazol / PTU (Tionamidas):</strong> Control del hipertiroidismo primario y manifestaciones autoinmunes de la enfermedad de Graves-Basedow[cite: 1542, 1543].",
      "La levotiroxina suple de forma molecular exacta la deficiencia endógena, restableciendo los niveles sistémicos y controlando el eje hipofisario[cite: 1569]. Las tionamidas bloquean de forma drástica la yodación de la tiroglobulina y la síntesis de nuevas hormonas[cite: 1571, 1572].",
      "La levotiroxina regula por retroalimentación negativa la secreción de la TSH hipofisaria[cite: 1575]. Las tionamidas reducen de forma progresiva y sostenida los niveles circulantes totales de las hormonas T3 y T4[cite: 1576, 1578].",
      "La levotiroxina posee una absorción oral óptima siempre que se administre estrictamente en condiciones de ayuno prolongado; posee una vida media sérica prolongada de aproximadamente 7 días[cite: 1580, 1581]. El metimazol demuestra una mayor duración de acción clínica frente al PTU[cite: 1582, 1584].",
      "Levotiroxina puede inducir taquicardia, arritmias y síntomas francos de hipertiroidismo clínico iatrogénico ante sobredosificación[cite: 1587, 1588]. Las tionamidas presentan el riesgo raro pero severo de inducir agranulocitosis inmunológica; el PTU asocia hepatotoxicidad fulminante[cite: 1591, 1592, 1594].",
      "<strong>Levotiroxina:</strong> 1.6 mcg/kg/día por vía VO, ajustado según niveles de TSH[cite: 1598, 1599]. <strong>Metimazol:</strong> 15 - 40 mg/día VO en toma única o fraccionada[cite: 1600]. <strong>Propiltiouracilo (PTU):</strong> 100 mg VO administrado cada 8 h[cite: 1601]."
    ]
  },
  's11-3': {
    points: [
      "Se unen de forma selectiva y con alta afinidad a receptores nucleares esteroideos específicos localizados en el citoplasma y núcleo celular, actuando como factores de transcripción ligando-dependientes que modulan la expresión génica[cite: 1640, 1641, 1643].",
      "Indicados clínicamente en regímenes de anticoncepción hormonal regular o de emergencia, terapia hormonal sustitutiva de la menopausia y el manejo del hipogonadismo primario[cite: 1615]. No presentan propiedad antimicrobiana[cite: 1611, 1613].",
      "Inhiben la secreción pulsátil de gonadotropinas suprimiendo de forma efectiva la ovulación ovárica, modifican las propiedades fisicoquímicas del moco cervical uterino o restauran los niveles sistémicos hormonales fisiológicos[cite: 1617, 1618, 1620].",
      "Los estrógenos inducen de forma potente la proliferación del tejido endometrial[cite: 1622]. Los progestágenos inducen la diferenciación secretora y estabilizan el endometrio[cite: 1623, 1624]. La testosterona incrementa de forma anabólica la masa muscular y estimula la eritropoyesis[cite: 1625, 1626, 1632].",
      "Presentan una adecuada biodisponibilidad a través de la vía de administración oral, transdérmica o mediante preparados intramusculares (IM) de depósito[cite: 1634, 1635]. Experimentan un extenso y rápido metabolismo de primer paso hepático[cite: 1636, 1638].",
      "Los estrógenos elevan de forma significativa el riesgo relativo de eventos tromboembólicos venosos y arteriales y el riesgo de cáncer endometrial sin oposición[cite: 1648, 1649]. Los progestágenos asocian aumento de peso corporal[cite: 1650]. La testosterona causa poliglobulia severa y ginecomastia[cite: 1651, 1652].",
      "<strong>Levonorgestrel (Emergencia):</strong> 1.5 mg VO administrado en una única dosis lo antes posible dentro de las 72 h[cite: 1654, 1655]. <strong>Medroxiprogesterona de depósito:</strong> 150 mg IM profundo aplicado de forma estricta cada 3 meses[cite: 1656]. <strong>Testosterona (Enantato):</strong> 100 - 250 mg IM cada 2 a 4 semanas[cite: 1657]."
    ]
  },
  's11-4': {
    points: [
      "<strong>Tamoxifeno:</strong> Modulador selectivo del receptor de estrógeno (SERM) que ejerce antagonismo competitivo en tejido mamario[cite: 1661, 1663]. <strong>Clomifeno:</strong> Bloquea el feedback estrogénico hipotalámico[cite: 1664]. <strong>Finasteride:</strong> Inhibidor competitivo de la enzima 5-alfa-reductasa[cite: 1665, 1666]. <strong>Flutamida:</strong> Antagonista puro del receptor androgénico[cite: 1667, 1669].",
      "Manejo adyuvante del cáncer de mama estrógeno-dependiente, inducción de la ovulación en infertilidad por anovulación, hiperplasia prostática benigna (HPB) y manejo avanzado del adenocarcinoma de próstata[cite: 1677, 1679, 1680, 1681].",
      "Inhiben o suprimen por completo los estímulos y señales hormonales tróficas que son directamente responsables del crecimiento tumoral neoplásico o de la hiperplasia celular tisular benigna[cite: 1686, 1688, 1690, 1691].",
      "El tamoxifeno demuestra un efecto agonista beneficioso en el tejido óseo pero actúa como estimulante en el endometrio[cite: 1693, 1694]. El finasteride bloquea la producción de DHT reduciendo de forma objetiva los niveles del Antígeno Prostático Específico (APE)[cite: 1695].",
      "Todos los agentes de este grupo demuestran tener una rápida y excelente absorción por la vía oral, siendo metabolizados de forma activa por sistemas enzimáticos del hígado[cite: 1697, 1698].",
      "<strong>Tamoxifeno:</strong> Incremento en el riesgo de trombosis venosa profunda y desarrollo iatrogénico de cáncer de endometrio[cite: 1700]. <strong>Clomifeno:</strong> Elevada tasa de inducción de embarazos múltiples[cite: 1701, 1702]. <strong>Finasteride:</strong> Disfunción eréctil y pérdida de la libido[cite: 1703, 1704]. <strong>Flutamida:</strong> Riesgo elevado de hepatotoxicidad severa[cite: 1713].",
      "<strong>Tamoxifeno:</strong> 20 mg/día VO de forma continua[cite: 1715]. <strong>Clomifeno:</strong> 50 mg/día VO administrado durante un ciclo estricto de 5 días consecutivos[cite: 1716]. <strong>Finasteride:</strong> 5 mg/día VO continuo para HPB[cite: 1717]. <strong>Flutamida:</strong> 250 mg VO administrado cada 8 h[cite: 1718]."
    ]
  },
  's11-5': {
    points: [
      "Activa el receptor de insulina de membrana con actividad intrínseca de tirosina kinasa, desencadenando la fosforilación de sustratos intracelulares (IRS) y promoviendo la translocación exocítica de transportadores GLUT-4[cite: 1721, 1722].",
      "Tratamiento de la Diabetes Mellitus Tipo 1 (DM1), estadios avanzados o descompensados de Diabetes Mellitus Tipo 2 (DM2), manejo de la cetoacidosis diabética y manejo de emergencia de la hiperkalemia severa[cite: 1711, 1712].",
      "Induce una rápida depuración y captación celular activa de la glucosa circulante desde el espacio intravascular hacia el tejido muscular y adiposo, e inhibe de forma paralela la producción y gluconeogénesis hepática[cite: 1722, 1727, 1728].",
      "Despliega potentes efectos metabólicos de tipo anabólico; estimula activamente la glucogénesis intracelular, la síntesis de proteínas y favorece la lipogénesis, inhibiendo la degradación tisular[cite: 1730, 1731, 1732].",
      "Se administra de forma regular mediante inyecciones subcutáneas (SC) para mantenimiento basal o prandial; permite la vía intravenosa (IV) exclusiva para la forma Regular[cite: 1734]. Las análogas rápidas actúan en minutos; las basales duran hasta 42 h[cite: 1735, 1737].",
      "El efecto adverso más frecuente y peligroso es el desarrollo de hipoglucemia aguda, aumento de peso corporal graso secundario y desarrollo focal de lipodistrofia en los sitios anatómicos de inyección repetida[cite: 1739, 1740].",
      "<strong>Régimen Basal de inicio:</strong> Se calcula a razón de 0.1 - 0.2 UI/kg/día mediante inyección por vía SC administrado por las noches o según el análogo[cite: 1742]. <strong>Esquema Prandial de corrección:</strong> 4 - 6 UI SC aplicado inmediatamente antes de las comidas principales[cite: 1743, 1744]."
    ]
  },
  's11-6': {
    points: [
      "<strong>Metformina:</strong> Activa la proteína quinasa AMP (AMPK) disminuyendo la gluconeogénesis[cite: 463, 1782, 1784]. <strong>Sulfonilureas:</strong> Bloquean canales de potasio dependientes de ATP en células beta[cite: 466, 1787, 1788]. <strong>DPP-4:</strong> Inhiben la degradación de incretinas endógenas[cite: 467, 1750]. <strong>GLP-1:</strong> Agonistas incretínicos directos[cite: 470]. <strong>SGLT2:</strong> Inhiben el cotransportador renal de glucosa[cite: 473, 479].",
      "Indicados de forma estándar y combinada para el tratamiento y control metabólico a largo plazo de la Diabetes Mellitus Tipo 2 (DM2) de forma exclusiva[cite: 1763]. No poseen ninguna propiedad antimicrobiana[cite: 1760, 1762].",
      "Reducen de forma efectiva los niveles séricos de glucemia basal y posprandial a través de mecanismos complementarios no pancreáticos, secretagogos puros o mediante la inducción de glucosuria terapéutica[cite: 1756, 1757, 1767, 1769, 1787].",
      "Los agonistas de los receptores de GLP-1 retrasan el vaciamiento gástrico y favorecen de forma clínica la pérdida de peso[cite: 1777]. Los inhibidores del cotransportador SGLT2 ofrecen una demostrada protección orgánica a nivel renal y cardiovascular[cite: 1779, 1780].",
      "La gran mayoría de estas familias moleculares se administran cómodamente por la vía oral[cite: 1785]. Sin embargo, la clase de los agonistas de los receptores de GLP-1 requiere una administración parenteral por vía subcutánea (SC)[cite: 1786, 1789].",
      "<strong>Metformina:</strong> Molestias gastrointestinales severas dosis-dependientes y riesgo raro de acidosis láctica[cite: 1791, 1792]. <strong>Sulfonilureas:</strong> Elevado riesgo de inducir hipoglucemias severas prolongadas[cite: 1793, 1794]. <strong>GLP-1:</strong> Náuseas intensas[cite: 1795]. <strong>SGLT2:</strong> Infecciones micóticas y bacterianas genitourinarias de repetición[cite: 1796, 1797].",
      "<strong>Metformina:</strong> 500 - 850 mg VO al día, con un incremento gradual ajustado hasta una dosis máxima tolerada de 2000 - 2550 mg/día[cite: 1801, 1802]. <strong>Sitagliptina (DPP-4):</strong> 100 mg VO una vez al día[cite: 1803]. <strong>Semaglutida (GLP-1):</strong> Dosis inicial de escalamiento de 0.25 mg SC una vez por semana[cite: 1804, 1805]. <strong>Empagliflozina (SGLT2):</strong> 10 - 25 mg VO una vez al día[cite: 1806]."
    ]
  }
};

/* ═══════════════════════════════════════════════════════════════════
   BUILD NAVIGATION
   ═══════════════════════════════════════════════════════════════════ */
function buildNav() {
  const tree = document.getElementById('nav-tree');
  tree.innerHTML = '';
  CHAPTERS.forEach(ch => {
    const div = document.createElement('div');
    div.className = 'nav-chapter';
    div.innerHTML = `
      <button class="nav-chapter-btn" onclick="toggleChapter('sub-${ch.id}', this)">
        <span class="nav-chapter-num">${ch.num}</span>
        <span style="flex:1">${ch.title}</span>
        <svg class="nav-chapter-arrow" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><polyline points="9 18 15 12 9 6"/></svg>
      </button>
      <ul class="nav-sub-list" id="sub-${ch.id}">
        ${ch.sections.map(sec => `
          <li class="nav-sub-item">
            <button class="nav-sub-btn" id="nav-${sec.id}" onclick="showSection('${ch.id}','${sec.id}')">
              <span class="nav-sub-dot">●</span>
              <span>${sec.title}</span>
            </button>
          </li>`).join('')}
      </ul>`;
    tree.appendChild(div);
  });
}

function toggleChapter(subId, btn) {
  const list = document.getElementById(subId);
  const isOpen = list.classList.contains('open');
  // close all
  document.querySelectorAll('.nav-sub-list').forEach(l => l.classList.remove('open'));
  document.querySelectorAll('.nav-chapter-btn').forEach(b => b.classList.remove('open'));
  if (!isOpen) {
    list.classList.add('open');
    btn.classList.add('open');
  }
}

/* ═══════════════════════════════════════════════════════════════════
   HOME
   ═══════════════════════════════════════════════════════════════════ */
function buildHomeGrid() {
  const grid = document.getElementById('home-chapter-grid');
  grid.innerHTML = '';
  let totalSections = 0;
  CHAPTERS.forEach(ch => {
    totalSections += ch.sections.length;
    const card = document.createElement('div');
    card.className = 'home-chapter-card';
    card.onclick = () => { toggleChapter(`sub-${ch.id}`, document.querySelector(`[onclick="toggleChapter('sub-${ch.id}', this)"]`)); showSection(ch.id, ch.sections[0].id); };
    card.innerHTML = `
      <div class="home-chapter-icon">${ch.num}</div>
      <div>
        <div class="home-chapter-title">${ch.title}</div>
        <div class="home-chapter-sub">${ch.sections.length} sección${ch.sections.length>1?'es':''}</div>
      </div>`;
    grid.appendChild(card);
  });
  document.getElementById('stat-sections').textContent = totalSections;
  document.getElementById('stat-chapters').textContent = CHAPTERS.length;
}

function showHome() {
  document.getElementById('home-view').style.display = 'block';
  document.getElementById('section-view').style.display = 'none';
  document.querySelectorAll('.nav-sub-btn').forEach(b => b.classList.remove('active'));
  if (window.innerWidth < 769) closeSidebarMobile();
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION VIEW
   ═══════════════════════════════════════════════════════════════════ */
function showSection(chapterId, sectionId) {
  const chapter = CHAPTERS.find(c => c.id === chapterId);
  const section = chapter.sections.find(s => s.id === sectionId);
  if (!chapter || !section) return;

  // Nav highlights
  document.querySelectorAll('.nav-sub-btn').forEach(b => b.classList.remove('active'));
  const navBtn = document.getElementById(`nav-${sectionId}`);
  if (navBtn) navBtn.classList.add('active');

  // Ensure chapter is open
  const subList = document.getElementById(`sub-${chapterId}`);
  if (subList && !subList.classList.contains('open')) {
    subList.classList.add('open');
    const chBtn = subList.previousElementSibling;
    if (chBtn) chBtn.classList.add('open');
  }

  // Breadcrumb
  document.getElementById('section-breadcrumb').innerHTML = `
    <span class="breadcrumb-link" onclick="showHome()">Inicio</span>
    <span class="breadcrumb-sep">›</span>
    <span>${chapter.title}</span>
    <span class="breadcrumb-sep">›</span>
    <span class="breadcrumb-current">${section.title}</span>`;

  // Header
  document.getElementById('section-chapter-label').textContent = `Capítulo ${chapter.num}`;
  document.getElementById('section-title').textContent = section.title;
  document.getElementById('section-subtitle').textContent = section.sub;

  // Body
  const body = document.getElementById('section-body');
  if (CONTENT[sectionId]) {
    body.innerHTML = renderContent(CONTENT[sectionId], section);
  } else {
    body.innerHTML = renderPlaceholder(section);
  }

  // Show
  document.getElementById('home-view').style.display = 'none';
  const sv = document.getElementById('section-view');
  sv.style.display = 'block';
  sv.style.animation = 'none';
  requestAnimationFrame(() => { sv.style.animation = ''; });

  window.scrollTo({ top: 0, behavior: 'smooth' });
  if (window.innerWidth < 769) closeSidebarMobile();
}

/* ═══════════════════════════════════════════════════════════════════
   CONTENT RENDERER — takes structured data object
   ═══════════════════════════════════════════════════════════════════ */
function renderContent(data, section) {
  let html = '';

  // Drug reference card
  if (section.tags && section.tags.length) {
    html += `<div class="drug-card">
      <div class="drug-card-title">Fármacos de referencia</div>
      <div class="drug-tag-list">${section.tags.map(t=>`<span class="drug-tag">${t}</span>`).join('')}</div>
    </div>`;
  }

  // Alert box (optional)
  if (data.alert) {
    html += renderAlert(data.alert);
  }

  // Subfamilies
  if (data.subfamilies) {
    data.subfamilies.forEach((sf, i) => {
      html += `<div class="subfamily-divider"><div class="subfamily-divider-line"></div><div class="subfamily-divider-label">${sf.name}</div><div class="subfamily-divider-line"></div></div>`;
      if (sf.tags) {
        html += `<div class="drug-card" style="margin-bottom:20px"><div class="drug-card-title">Agentes incluidos</div><div class="drug-tag-list">${sf.tags.map(t=>`<span class="drug-tag">${t}</span>`).join('')}</div></div>`;
      }
      html += renderPoints(sf.points, sf.alerts);
    });
  } else if (data.points) {
    html += renderPoints(data.points, data.alerts);
  }

  return html;
}

function renderPoints(points, alerts) {
  const LABELS = ['Mecanismo de acción','En qué microorganismos actúa','Cómo actúa contra esos microorganismos','Farmacodinamia','Farmacocinética','Efectos adversos','Dosis'];
  let html = '<div class="content-block">';
  points.forEach((pt, i) => {
    html += `<div class="point-section">
      <div class="point-header">
        <span class="point-num">0${i+1}</span>
        <span class="point-label">${LABELS[i] || ''}</span>
      </div>
      <div class="point-content">${pt}</div>
    </div>`;
    // insert alert after point 6 (ADRs) if present
    if (i === 5 && alerts) {
      alerts.forEach(a => { html += renderAlert(a); });
    }
  });
  html += '</div>';
  return html;
}

function renderAlert({ type = 'warning', title, body }) {
  const icons = { warning: '⚠️', danger: '🚨', info: 'ℹ️', success: '✅' };
  return `<div class="alert-box ${type}">
    <div class="alert-box-icon">${icons[type]||'⚠️'}</div>
    <div class="alert-box-body">
      ${title ? `<div class="alert-box-title">${title}</div>` : ''}
      <div>${body}</div>
    </div>
  </div>`;
}

function renderPlaceholder(section) {
  return `
    <div class="drug-card">
      <div class="drug-card-title">Fármacos de referencia</div>
      <div class="drug-tag-list">${(section.tags||[]).map(t=>`<span class="drug-tag">${t}</span>`).join('') || '<span style="color:var(--text-muted);font-size:13px">Pendiente de desarrollo</span>'}</div>
    </div>
    <div class="section-placeholder">
      <div class="section-placeholder-icon">📋</div>
      <h3>Sección pendiente de desarrollo</h3>
      <p>El contenido farmacológico detallado de esta sección será añadido en la siguiente fase del vademécum.</p>
    </div>`;
}

/* ═══════════════════════════════════════════════════════════════════
   SEARCH
   ═══════════════════════════════════════════════════════════════════ */
function buildSearchIndex() {
  const idx = [];
  CHAPTERS.forEach(ch => {
    ch.sections.forEach(sec => {
      idx.push({ label: sec.title, path: `${ch.title}`, chId: ch.id, secId: sec.id, tags: sec.tags || [] });
      sec.tags.forEach(tag => {
        idx.push({ label: tag, path: `${ch.title} › ${sec.title}`, chId: ch.id, secId: sec.id, tags: [] });
      });
    });
  });
  return idx;
}
const SEARCH_INDEX = [];

function handleSearch(q) {
  const box = document.getElementById('search-results');
  if (!q.trim()) { box.style.display = 'none'; return; }
  const results = SEARCH_INDEX.filter(item =>
    item.label.toLowerCase().includes(q.toLowerCase()) ||
    item.path.toLowerCase().includes(q.toLowerCase())
  ).slice(0, 8);

  if (!results.length) {
    box.innerHTML = `<div class="search-result-empty">Sin resultados para "<strong>${q}</strong>"</div>`;
  } else {
    box.innerHTML = results.map((r,i) => `
      <div class="search-result-item" onclick="showSection('${r.chId}','${r.secId}');document.getElementById('search-input').value='';closeSearch()">
        <span class="search-result-name">${highlight(r.label, q)}</span>
        <span class="search-result-path">${r.path}</span>
      </div>`).join('');
  }
  box.style.display = 'block';
}

function highlight(text, q) {
  const re = new RegExp(`(${q.replace(/[.*+?^${}()|[\]\\]/g,'\\$&')})`, 'gi');
  return text.replace(re, '<strong style="color:var(--accent-blue)">$1</strong>');
}

function closeSearch() {
  document.getElementById('search-results').style.display = 'none';
}

/* ═══════════════════════════════════════════════════════════════════
   SIDEBAR TOGGLE
   ═══════════════════════════════════════════════════════════════════ */
function toggleSidebar() {
  const sidebar = document.getElementById('sidebar');
  const main = document.getElementById('main');
  if (window.innerWidth < 769) {
    sidebar.classList.toggle('open');
    document.getElementById('sidebar-overlay').style.display = sidebar.classList.contains('open') ? 'block' : 'none';
  } else {
    sidebar.classList.toggle('collapsed');
    main.classList.toggle('sidebar-collapsed');
  }
}

function closeSidebarMobile() {
  document.getElementById('sidebar').classList.remove('open');
  document.getElementById('sidebar-overlay').style.display = 'none';
}

/* ═══════════════════════════════════════════════════════════════════
   INIT
   ═══════════════════════════════════════════════════════════════════ */
document.addEventListener('DOMContentLoaded', () => {
  buildNav();
  buildHomeGrid();
  // build search index
  CHAPTERS.forEach(ch => {
    ch.sections.forEach(sec => {
      SEARCH_INDEX.push({ label: sec.title, path: ch.title, chId: ch.id, secId: sec.id, tags: sec.tags || [] });
      (sec.tags || []).forEach(tag => {
        SEARCH_INDEX.push({ label: tag, path: `${ch.title} › ${sec.title}`, chId: ch.id, secId: sec.id, tags: [] });
      });
    });
  });
});
</script>
</body>
</html>
