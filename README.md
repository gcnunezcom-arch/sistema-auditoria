# sistema-auditoria
 Internal Audit System Dashboard
[sistema-auditoria (2).html](https://github.com/user-attachments/files/26105424/sistema-auditoria.2.html)
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sistema de Auditoría Interna — Producción</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=Sora:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --font: 'Sora', sans-serif;
  --mono: 'IBM Plex Mono', monospace;
  --navy: #0B1829;
  --navy2: #112038;
  --navy3: #162847;
  --gold: #C8952A;
  --gold2: #E8B84B;
  --gold-dim: rgba(200,149,42,0.15);
  --accent: #1B6EC2;
  --accent-light: #1E3A5F;
  --red: #E05252;
  --red-dim: rgba(224,82,82,0.15);
  --amber: #D4811A;
  --amber-dim: rgba(212,129,26,0.15);
  --green: #2ECC7A;
  --green-dim: rgba(46,204,122,0.15);
  --purple: #8B7FF0;
  --purple-dim: rgba(139,127,240,0.15);
  --teal: #2EC4B6;
  --teal-dim: rgba(46,196,182,0.15);
  --text: #E8E4D9;
  --text2: #9A95A0;
  --text3: #5A5565;
  --border: rgba(255,255,255,0.07);
  --border2: rgba(255,255,255,0.12);
  --card: rgba(255,255,255,0.04);
  --card2: rgba(255,255,255,0.07);
}

html, body {
  height: 100%;
  background: var(--navy);
  color: var(--text);
  font-family: var(--font);
  font-size: 13px;
  line-height: 1.5;
}

/* ── LAYOUT ── */
.app { display: flex; height: 100vh; overflow: hidden; }

/* ── SIDEBAR ── */
.sidebar {
  width: 220px;
  flex-shrink: 0;
  background: var(--navy2);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}

.sidebar-logo {
  padding: 20px 18px 16px;
  border-bottom: 1px solid var(--border);
}
.logo-mark {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 4px;
}
.logo-icon {
  width: 30px; height: 30px;
  background: linear-gradient(135deg, var(--gold), var(--gold2));
  border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  font-size: 14px; font-weight: 700; color: var(--navy);
}
.logo-name { font-size: 13px; font-weight: 700; color: var(--text); letter-spacing: .02em; }
.logo-sub { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: .1em; }

.sidebar-section { padding: 10px 10px 4px; }
.sidebar-label { font-size: 9px; font-weight: 600; color: var(--text3); text-transform: uppercase; letter-spacing: .12em; padding: 0 8px; margin-bottom: 4px; }

.nav-item {
  display: flex; align-items: center; gap: 10px;
  padding: 8px 10px; border-radius: 7px; cursor: pointer;
  color: var(--text2); font-size: 12px; font-weight: 400;
  transition: all .15s; position: relative;
  border: none; background: transparent; width: 100%; text-align: left;
}
.nav-item:hover { background: var(--card2); color: var(--text); }
.nav-item.active { background: var(--gold-dim); color: var(--gold2); font-weight: 500; }
.nav-item.active::before {
  content: ''; position: absolute; left: 0; top: 50%; transform: translateY(-50%);
  width: 3px; height: 18px; background: var(--gold); border-radius: 0 2px 2px 0;
}
.nav-icon { font-size: 14px; width: 18px; text-align: center; }
.nav-badge {
  margin-left: auto; background: var(--red); color: #fff;
  font-size: 9px; font-weight: 700; padding: 1px 5px; border-radius: 10px;
}

.sidebar-footer {
  margin-top: auto; padding: 14px 18px;
  border-top: 1px solid var(--border);
}
.version-tag { font-size: 10px; color: var(--text3); font-family: var(--mono); }

/* ── MAIN ── */
.main { flex: 1; overflow-y: auto; display: flex; flex-direction: column; }

.topbar {
  padding: 14px 28px;
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center; justify-content: space-between;
  background: var(--navy);
  position: sticky; top: 0; z-index: 10;
}
.topbar-title { font-size: 15px; font-weight: 600; }
.topbar-sub { font-size: 11px; color: var(--text3); margin-top: 1px; }
.topbar-right { display: flex; gap: 8px; align-items: center; }
.date-badge {
  font-family: var(--mono); font-size: 11px; color: var(--text3);
  background: var(--card); border: 1px solid var(--border); border-radius: 5px;
  padding: 4px 10px;
}
.gold-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--green); box-shadow: 0 0 6px var(--green); }

.content { padding: 24px 28px; flex: 1; }

/* ── SECTIONS ── */
.section { display: none; animation: fadeIn .2s ease; }
.section.active { display: block; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

/* ── KPI GRID ── */
.kpi-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 22px; }
.kpi-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 10px; padding: 14px 16px;
  position: relative; overflow: hidden;
}
.kpi-card::after {
  content: ''; position: absolute; top: 0; left: 0; right: 0;
  height: 2px; border-radius: 2px;
}
.kpi-card.red::after { background: var(--red); }
.kpi-card.amber::after { background: var(--amber); }
.kpi-card.green::after { background: var(--green); }
.kpi-card.blue::after { background: var(--accent); }
.kpi-label { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: .08em; margin-bottom: 8px; }
.kpi-value { font-size: 28px; font-weight: 700; line-height: 1; }
.kpi-value.red { color: var(--red); }
.kpi-value.amber { color: var(--amber); }
.kpi-value.green { color: var(--green); }
.kpi-value.blue { color: var(--accent); }
.kpi-sub { font-size: 10px; color: var(--text3); margin-top: 5px; }

/* ── CARDS ── */
.card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 10px; padding: 18px;
}
.card-header {
  display: flex; align-items: center; justify-content: space-between;
  margin-bottom: 14px;
}
.card-title {
  font-size: 10px; font-weight: 600; color: var(--text3);
  text-transform: uppercase; letter-spacing: .1em;
}

/* ── BADGES ── */
.badge {
  display: inline-flex; align-items: center; gap: 4px;
  padding: 2px 8px; border-radius: 20px;
  font-size: 10px; font-weight: 600; letter-spacing: .03em;
}
.badge-red { background: var(--red-dim); color: var(--red); }
.badge-amber { background: var(--amber-dim); color: var(--amber); }
.badge-green { background: var(--green-dim); color: var(--green); }
.badge-blue { background: rgba(27,110,194,0.2); color: #6EB4F0; }
.badge-purple { background: var(--purple-dim); color: var(--purple); }
.badge-teal { background: var(--teal-dim); color: var(--teal); }
.badge-gray { background: var(--card2); color: var(--text2); }
.badge-gold { background: var(--gold-dim); color: var(--gold2); }

/* ── TABLE ── */
.table-wrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; }
th {
  padding: 8px 12px; text-align: left;
  font-size: 9px; font-weight: 700; color: var(--text3);
  text-transform: uppercase; letter-spacing: .1em;
  border-bottom: 1px solid var(--border2);
  background: var(--card);
  white-space: nowrap;
}
td {
  padding: 9px 12px;
  border-bottom: 1px solid var(--border);
  color: var(--text); font-size: 12px;
  vertical-align: middle;
}
tr:hover td { background: var(--card); }
.code {
  font-family: var(--mono); font-size: 11px;
  color: var(--gold2); background: var(--gold-dim);
  padding: 2px 6px; border-radius: 4px;
}

/* ── PROGRESS ── */
.progress-wrap { margin: 4px 0; }
.progress-label {
  display: flex; justify-content: space-between;
  font-size: 11px; margin-bottom: 4px; color: var(--text2);
}
.progress-bar { height: 5px; background: var(--card2); border-radius: 3px; overflow: hidden; }
.progress-fill { height: 100%; border-radius: 3px; transition: width .6s cubic-bezier(.4,0,.2,1); }

/* ── WORKFLOW ── */
.wf-list { display: flex; flex-direction: column; gap: 0; }
.wf-item { display: flex; gap: 16px; }
.wf-spine { display: flex; flex-direction: column; align-items: center; }
.wf-num {
  width: 34px; height: 34px; border-radius: 50%; flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
  font-weight: 700; font-size: 13px;
}
.wf-line { flex: 1; width: 1px; background: var(--border2); margin: 4px 0; min-height: 16px; }
.wf-body { padding-bottom: 22px; flex: 1; }
.wf-title { font-size: 13px; font-weight: 600; color: var(--text); margin-bottom: 5px; line-height: 1.4; }
.wf-desc { font-size: 12px; color: var(--text2); line-height: 1.6; }
.wf-chips { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 8px; }

/* ── COMM COLS ── */
.comm-cols { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; margin-bottom: 18px; }
.comm-col { border: 1px solid var(--border); border-radius: 10px; overflow: hidden; }
.comm-col-header { padding: 12px 16px; font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .1em; }
.comm-col-body { padding: 14px 16px; }
.comm-row { display: flex; gap: 10px; padding: 7px 0; border-bottom: 1px solid var(--border); font-size: 12px; color: var(--text2); }
.comm-row:last-child { border-bottom: none; }
.comm-dot { width: 5px; height: 5px; border-radius: 50%; flex-shrink: 0; margin-top: 5px; }
.comm-time { font-size: 10px; font-family: var(--mono); padding: 8px 16px; border-top: 1px solid var(--border); color: var(--text3); }

/* ── EMAIL ── */
.email-tabs { display: flex; gap: 6px; flex-wrap: wrap; margin-bottom: 16px; }
.email-tab {
  padding: 6px 14px; border-radius: 20px; cursor: pointer;
  border: 1px solid var(--border2); background: transparent;
  font-size: 12px; color: var(--text2); font-family: var(--font);
  transition: all .15s;
}
.email-tab:hover { background: var(--card2); color: var(--text); }
.email-tab.active { background: var(--gold-dim); color: var(--gold2); border-color: var(--gold); font-weight: 500; }
.email-box { border: 1px solid var(--border2); border-radius: 10px; overflow: hidden; }
.email-head { padding: 14px 18px; background: var(--card); border-bottom: 1px solid var(--border); }
.email-meta { font-size: 11px; color: var(--text3); margin-bottom: 2px; }
.email-subject { font-size: 14px; font-weight: 600; color: var(--text); margin-top: 4px; }
.email-body {
  padding: 18px; font-size: 12px; line-height: 1.8;
  color: var(--text2); white-space: pre-wrap;
  font-family: var(--font);
}

/* ── ESCALAMIENTO ── */
.esc-grid { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
.esc-row {
  display: grid; grid-template-columns: 80px 1fr 1fr;
  gap: 16px; align-items: center;
  padding: 14px 18px; border-radius: 10px;
  border: 1px solid;
}
.esc-days-big { font-size: 22px; font-weight: 800; text-align: center; font-family: var(--mono); }
.esc-days-label { font-size: 9px; text-transform: uppercase; letter-spacing: .1em; text-align: center; margin-top: 1px; opacity: .7; }
.esc-rule-title { font-size: 13px; font-weight: 600; margin-bottom: 4px; }
.esc-rule-desc { font-size: 12px; opacity: .75; line-height: 1.5; }
.esc-who-label { font-size: 9px; text-transform: uppercase; letter-spacing: .1em; opacity: .6; margin-bottom: 4px; }
.esc-who { font-size: 12px; font-weight: 500; }

/* ── KPI TABLE ── */
.kpi-table-row {
  display: grid; grid-template-columns: 200px 1fr 80px;
  gap: 16px; padding: 12px 0;
  border-bottom: 1px solid var(--border);
  align-items: start;
}
.kpi-table-row:last-child { border-bottom: none; }
.kpi-t-name { font-size: 12px; font-weight: 600; color: var(--text); }
.kpi-t-desc { font-size: 11px; color: var(--text3); margin-top: 2px; }
.formula-pill {
  display: inline-block; font-family: var(--mono); font-size: 11px;
  background: rgba(200,149,42,0.12); color: var(--gold2);
  padding: 3px 8px; border-radius: 5px; margin-bottom: 4px;
  border: 1px solid rgba(200,149,42,0.2);
  word-break: break-all; line-height: 1.5;
}
.kpi-t-interpret { font-size: 11px; color: var(--text2); line-height: 1.5; }

/* ── AUTO ── */
.auto-section { margin-bottom: 22px; }
.auto-header { display: flex; align-items: center; gap: 10px; margin-bottom: 12px; }
.auto-badge {
  font-size: 10px; font-weight: 700; padding: 3px 9px;
  border-radius: 5px; text-transform: uppercase; letter-spacing: .08em;
}
.auto-title-text { font-size: 14px; font-weight: 600; color: var(--text); }
.code-block {
  background: #0D1B2E; border: 1px solid var(--border2);
  border-radius: 8px; padding: 14px 16px;
  font-family: var(--mono); font-size: 12px; color: #A8D8A8;
  line-height: 1.7; overflow-x: auto; margin-bottom: 8px;
}
.formula-block {
  background: rgba(200,149,42,0.08); border-left: 3px solid var(--gold);
  padding: 10px 14px; border-radius: 0 7px 7px 0;
  font-family: var(--mono); font-size: 12px; color: var(--gold2);
  margin: 6px 0; line-height: 1.6;
}
.auto-label { font-size: 11px; font-weight: 600; color: var(--text2); margin: 10px 0 5px; }

/* ── REPORTE ── */
.report-grid { display: grid; grid-template-columns: 200px 1fr; gap: 16px; }
.report-toc { display: flex; flex-direction: column; gap: 2px; }
.toc-item {
  display: flex; align-items: center; gap: 10px;
  padding: 7px 10px; border-radius: 7px; cursor: pointer;
  font-size: 12px; color: var(--text2); transition: all .15s;
  border: none; background: transparent; text-align: left; width: 100%;
}
.toc-item:hover { background: var(--card2); color: var(--text); }
.toc-num { font-family: var(--mono); font-size: 10px; color: var(--text3); width: 20px; }
.report-sections { display: flex; flex-direction: column; gap: 10px; }
.report-block {
  background: var(--card); border: 1px solid var(--border);
  border-radius: 10px; padding: 16px;
}
.report-block-title { font-size: 13px; font-weight: 600; color: var(--gold2); margin-bottom: 8px; }
.report-block-desc { font-size: 12px; color: var(--text2); line-height: 1.7; }

/* ── CODING ── */
.code-anatomy {
  display: flex; align-items: center; justify-content: center; gap: 0;
  background: var(--card); border: 1px solid var(--border);
  border-radius: 12px; padding: 24px; margin-bottom: 18px;
  flex-wrap: wrap; gap: 0;
}
.code-seg-wrap { text-align: center; }
.code-seg {
  font-family: var(--mono); font-size: 22px; font-weight: 700;
  padding: 8px 14px; border-radius: 8px; letter-spacing: .05em;
  display: inline-block;
}
.code-seg-label { font-size: 9px; color: var(--text3); text-transform: uppercase; letter-spacing: .1em; margin-top: 6px; }
.code-sep { font-family: var(--mono); font-size: 22px; font-weight: 700; color: var(--text3); padding: 8px 4px; }

/* ── DIVIDER ── */
.divider { height: 1px; background: var(--border); margin: 16px 0; }

/* ── GRID UTILS ── */
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 14px; }

/* ── SCROLLBAR ── */
::-webkit-scrollbar { width: 5px; height: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 3px; }

/* ── RESPONSIVE ── */
@media (max-width: 900px) {
  .sidebar { width: 180px; }
  .kpi-grid { grid-template-columns: repeat(2, 1fr); }
  .grid-2, .grid-3, .comm-cols { grid-template-columns: 1fr; }
}
@media (max-width: 640px) {
  .sidebar { display: none; }
  .content { padding: 16px; }
  .kpi-grid { grid-template-columns: repeat(2, 1fr); }
  .esc-row { grid-template-columns: 1fr; }
  .kpi-table-row { grid-template-columns: 1fr; }
  .report-grid { grid-template-columns: 1fr; }
}
</style>
</head>
<body>

<div class="app">

  <!-- SIDEBAR -->
  <aside class="sidebar">
    <div class="sidebar-logo">
      <div class="logo-mark">
        <div class="logo-icon">AI</div>
        <div>
          <div class="logo-name">AuditControl</div>
        </div>
      </div>
      <div class="logo-sub">Sistema Interno</div>
    </div>

    <div class="sidebar-section">
      <div class="sidebar-label">Principal</div>
      <button class="nav-item active" onclick="show('dashboard', this)">
        <span class="nav-icon">◈</span> Dashboard
        <span class="nav-badge">4</span>
      </button>
    </div>

    <div class="sidebar-section">
      <div class="sidebar-label">Sistema</div>
      <button class="nav-item" onclick="show('tablero', this)">
        <span class="nav-icon">▦</span> Tablero
      </button>
      <button class="nav-item" onclick="show('codigos', this)">
        <span class="nav-icon">⟨⟩</span> Códigos
      </button>
      <button class="nav-item" onclick="show('workflow', this)">
        <span class="nav-icon">⬡</span> Workflow
      </button>
    </div>

    <div class="sidebar-section">
      <div class="sidebar-label">Gestión</div>
      <button class="nav-item" onclick="show('comunicacion', this)">
        <span class="nav-icon">◎</span> Comunicación
      </button>
      <button class="nav-item" onclick="show('emails', this)">
        <span class="nav-icon">✉</span> Emails
      </button>
      <button class="nav-item" onclick="show('escalamiento', this)">
        <span class="nav-icon">▲</span> Escalamiento
      </button>
    </div>

    <div class="sidebar-section">
      <div class="sidebar-label">Análisis</div>
      <button class="nav-item" onclick="show('kpis', this)">
        <span class="nav-icon">◬</span> KPIs
      </button>
      <button class="nav-item" onclick="show('reporte', this)">
        <span class="nav-icon">☰</span> Reporte Ejec.
      </button>
      <button class="nav-item" onclick="show('automatizacion', this)">
        <span class="nav-icon">⚙</span> Automatización
      </button>
    </div>

    <div class="sidebar-footer">
      <div class="version-tag">v1.0 · 2026</div>
    </div>
  </aside>

  <!-- MAIN -->
  <div class="main">

    <!-- TOPBAR -->
    <div class="topbar">
      <div>
        <div class="topbar-title" id="topbar-title">Dashboard ejecutivo</div>
        <div class="topbar-sub" id="topbar-sub">Vista general del sistema de control interno</div>
      </div>
      <div class="topbar-right">
        <div class="date-badge" id="date-badge"></div>
        <div class="gold-dot"></div>
      </div>
    </div>

    <!-- CONTENT -->
    <div class="content">

      <!-- ═══════════════════════ DASHBOARD ═══════════════════════ -->
      <div id="dashboard" class="section active">

        <div class="kpi-grid">
          <div class="kpi-card red">
            <div class="kpi-label">Hallazgos activos</div>
            <div class="kpi-value red">14</div>
            <div class="kpi-sub">↑ 3 vs mes anterior</div>
          </div>
          <div class="kpi-card green">
            <div class="kpi-label">Tasa de remediación</div>
            <div class="kpi-value green">73%</div>
            <div class="kpi-sub">Meta: ≥ 80%</div>
          </div>
          <div class="kpi-card amber">
            <div class="kpi-label">Vencidos hoy</div>
            <div class="kpi-value amber">4</div>
            <div class="kpi-sub">Acción inmediata</div>
          </div>
          <div class="kpi-card red">
            <div class="kpi-label">Críticos abiertos</div>
            <div class="kpi-value red">2</div>
            <div class="kpi-sub">Escalar a Dirección</div>
          </div>
        </div>

        <div class="grid-2" style="margin-bottom: 16px;">
          <div class="card">
            <div class="card-title" style="margin-bottom:14px;">Estado de hallazgos</div>
            <div style="display:flex;flex-direction:column;gap:11px;">
              <div class="progress-wrap">
                <div class="progress-label"><span>En proceso</span><span style="color:var(--text);font-weight:600;">8</span></div>
                <div class="progress-bar"><div class="progress-fill" style="width:57%;background:var(--accent);"></div></div>
              </div>
              <div class="progress-wrap">
                <div class="progress-label"><span style="color:var(--red);">Vencidos</span><span style="color:var(--red);font-weight:600;">4</span></div>
                <div class="progress-bar"><div class="progress-fill" style="width:28%;background:var(--red);"></div></div>
              </div>
              <div class="progress-wrap">
                <div class="progress-label"><span style="color:var(--amber);">En revisión</span><span style="color:var(--amber);font-weight:600;">2</span></div>
                <div class="progress-bar"><div class="progress-fill" style="width:14%;background:var(--amber);"></div></div>
              </div>
              <div class="progress-wrap">
                <div class="progress-label"><span style="color:var(--green);">Cerrados (mes)</span><span style="color:var(--green);font-weight:600;">11</span></div>
                <div class="progress-bar"><div class="progress-fill" style="width:79%;background:var(--green);"></div></div>
              </div>
            </div>
          </div>
          <div class="card">
            <div class="card-title" style="margin-bottom:14px;">Distribución por riesgo</div>
            <div style="display:flex;flex-direction:column;gap:11px;">
              <div style="display:flex;align-items:center;gap:12px;">
                <div style="width:36px;height:36px;border-radius:8px;background:var(--red-dim);border:1px solid var(--red);display:flex;align-items:center;justify-content:center;font-size:17px;font-weight:800;color:var(--red);">2</div>
                <div><div style="font-weight:600;font-size:12px;">Crítico</div><div style="font-size:11px;color:var(--text3);">Impacto financiero alto · Acción inmediata</div></div>
              </div>
              <div style="display:flex;align-items:center;gap:12px;">
                <div style="width:36px;height:36px;border-radius:8px;background:var(--amber-dim);border:1px solid var(--amber);display:flex;align-items:center;justify-content:center;font-size:17px;font-weight:800;color:var(--amber);">5</div>
                <div><div style="font-weight:600;font-size:12px;">Alto</div><div style="font-size:11px;color:var(--text3);">Controles deficientes · 30 días</div></div>
              </div>
              <div style="display:flex;align-items:center;gap:12px;">
                <div style="width:36px;height:36px;border-radius:8px;background:rgba(27,110,194,0.2);border:1px solid var(--accent);display:flex;align-items:center;justify-content:center;font-size:17px;font-weight:800;color:#6EB4F0;">5</div>
                <div><div style="font-weight:600;font-size:12px;">Medio</div><div style="font-size:11px;color:var(--text3);">Mejoras recomendadas · 60 días</div></div>
              </div>
              <div style="display:flex;align-items:center;gap:12px;">
                <div style="width:36px;height:36px;border-radius:8px;background:var(--green-dim);border:1px solid var(--green);display:flex;align-items:center;justify-content:center;font-size:17px;font-weight:800;color:var(--green);">2</div>
                <div><div style="font-weight:600;font-size:12px;">Bajo</div><div style="font-size:11px;color:var(--text3);">Observaciones menores · 90 días</div></div>
              </div>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-header">
            <div class="card-title">Hallazgos críticos — acción requerida</div>
            <span class="badge badge-red">4 urgentes</span>
          </div>
          <div class="table-wrap">
            <table>
              <thead><tr>
                <th>Código</th><th>Descripción</th><th>Área</th><th>Días vencido</th><th>Responsable</th><th>Acción</th>
              </tr></thead>
              <tbody>
                <tr>
                  <td><span class="code">AUD-2026-FIN-CR-003</span></td>
                  <td>Conciliaciones bancarias sin revisar Q4 2025</td>
                  <td><span class="badge badge-purple">Finanzas</span></td>
                  <td><span style="color:var(--red);font-weight:700;font-family:var(--mono);">+18 días</span></td>
                  <td>J. Pérez</td>
                  <td><span class="badge badge-red">Escalar Dirección</span></td>
                </tr>
                <tr>
                  <td><span class="code">AUD-2026-INV-CR-007</span></td>
                  <td>Inventario sin custodia en almacén B</td>
                  <td><span class="badge badge-amber">Producción</span></td>
                  <td><span style="color:var(--red);font-weight:700;font-family:var(--mono);">+12 días</span></td>
                  <td>M. García</td>
                  <td><span class="badge badge-red">Escalar Dirección</span></td>
                </tr>
                <tr>
                  <td><span class="code">AUD-2026-TES-AL-011</span></td>
                  <td>Pagos sin segunda firma de aprobación</td>
                  <td><span class="badge badge-purple">Tesorería</span></td>
                  <td><span style="color:var(--amber);font-weight:700;font-family:var(--mono);">+5 días</span></td>
                  <td>R. Torres</td>
                  <td><span class="badge badge-amber">Recordatorio urgente</span></td>
                </tr>
                <tr>
                  <td><span class="code">AUD-2026-TI-AL-012</span></td>
                  <td>Accesos ERP sin segregación de funciones</td>
                  <td><span class="badge badge-teal">TI/Sistemas</span></td>
                  <td><span style="color:var(--amber);font-weight:700;font-family:var(--mono);">+3 días</span></td>
                  <td>L. Mendoza</td>
                  <td><span class="badge badge-amber">Recordatorio urgente</span></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

      </div><!-- /dashboard -->

      <!-- ═══════════════════════ TABLERO ═══════════════════════ -->
      <div id="tablero" class="section">

        <div class="grid-3" style="margin-bottom:18px;">
          <div class="card">
            <div class="card-title" style="margin-bottom:10px;">Identificación</div>
            <div style="font-size:12px;line-height:1.9;color:var(--text2);">Código hallazgo<br>Fecha identificación<br>Auditor responsable<br>Área auditada<br>Proceso auditado</div>
          </div>
          <div class="card">
            <div class="card-title" style="margin-bottom:10px;">Gestión</div>
            <div style="font-size:12px;line-height:1.9;color:var(--text2);">Nivel de riesgo<br>Tipo de control<br>Categoría COSO<br>Responsable remediación<br>Fecha límite compromiso</div>
          </div>
          <div class="card">
            <div class="card-title" style="margin-bottom:10px;">Seguimiento</div>
            <div style="font-size:12px;line-height:1.9;color:var(--text2);">% Avance remediación<br>Estado actual<br>Días de retraso<br>Nivel escalamiento<br>Evidencias recibidas</div>
          </div>
        </div>

        <div class="card">
          <div class="card-title" style="margin-bottom:14px;">Definición de los 20 campos</div>
          <div class="table-wrap">
            <table>
              <thead><tr><th>#</th><th>Campo</th><th>Tipo</th><th>Valores</th><th>Ejemplo</th></tr></thead>
              <tbody>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">01</td><td style="font-weight:500;">Código</td><td><span class="badge badge-blue">Texto</span></td><td>AUD-AAAA-AREA-RIESGO-NNN</td><td><span class="code">AUD-2026-FIN-CR-001</span></td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">02</td><td style="font-weight:500;">Título del hallazgo</td><td><span class="badge badge-blue">Texto</span></td><td>Descripción ≤80 chars</td><td>Falta de conciliación bancaria</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">03</td><td style="font-weight:500;">Descripción detallada</td><td><span class="badge badge-blue">Texto largo</span></td><td>Condición, criterio, causa, efecto</td><td>Se detectó que el área de Tesorería...</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">04</td><td style="font-weight:500;">Área responsable</td><td><span class="badge badge-purple">Lista</span></td><td>Finanzas / Producción / Compras / TI / RRHH</td><td>Finanzas</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">05</td><td style="font-weight:500;">Proceso auditado</td><td><span class="badge badge-blue">Texto</span></td><td>Nombre del subproceso</td><td>Conciliaciones bancarias</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">06</td><td style="font-weight:500;">Nivel de riesgo</td><td><span class="badge badge-purple">Lista</span></td><td>Crítico / Alto / Medio / Bajo</td><td><span class="badge badge-red">Crítico</span></td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">07</td><td style="font-weight:500;">Tipo de control</td><td><span class="badge badge-purple">Lista</span></td><td>Preventivo / Detectivo / Correctivo</td><td>Preventivo</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">08</td><td style="font-weight:500;">Categoría COSO</td><td><span class="badge badge-purple">Lista</span></td><td>Amb. Control / Eval. Riesgos / Act. Control / Info / Monitoreo</td><td>Actividades de control</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">09</td><td style="font-weight:500;">Fecha identificación</td><td><span class="badge badge-amber">Fecha</span></td><td>DD/MM/AAAA</td><td>05/01/2026</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">10</td><td style="font-weight:500;">Responsable remediación</td><td><span class="badge badge-blue">Texto</span></td><td>Nombre + cargo</td><td>Juan Pérez — Gerente Finanzas</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">11</td><td style="font-weight:500;">Fecha límite compromiso</td><td><span class="badge badge-amber">Fecha</span></td><td>DD/MM/AAAA</td><td>15/02/2026</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">12</td><td style="font-weight:500;">Estado</td><td><span class="badge badge-purple">Lista</span></td><td>Pendiente / En proceso / En revisión / Vencido / Cerrado</td><td><span class="badge badge-amber">En proceso</span></td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">13</td><td style="font-weight:500;">% Avance</td><td><span class="badge badge-green">Número</span></td><td>0% — 100%</td><td>60%</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">14</td><td style="font-weight:500;">Días de retraso</td><td><span class="badge badge-gold">Fórmula</span></td><td>=HOY()-Fecha_límite (si Estado≠Cerrado)</td><td style="font-family:var(--mono);color:var(--amber);">+7</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">15</td><td style="font-weight:500;">Nivel escalamiento</td><td><span class="badge badge-purple">Lista</span></td><td>Sin escalar / Gerencia / Dirección / Junta</td><td>Dirección</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">16</td><td style="font-weight:500;">Plan de acción</td><td><span class="badge badge-blue">Texto largo</span></td><td>Pasos específicos acordados</td><td>1) Asignar responsable... 2) Ejecutar...</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">17</td><td style="font-weight:500;">Evidencias recibidas</td><td><span class="badge badge-purple">Lista</span></td><td>Sí / No / Parcial</td><td><span class="badge badge-green">Sí</span></td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">18</td><td style="font-weight:500;">Comentarios auditor</td><td><span class="badge badge-blue">Texto</span></td><td>Notas internas privadas</td><td>Verificado con estado de cuenta enero</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">19</td><td style="font-weight:500;">Fecha cierre</td><td><span class="badge badge-amber">Fecha</span></td><td>Solo cuando Estado = Cerrado</td><td>28/02/2026</td></tr>
                <tr><td style="font-family:var(--mono);color:var(--text3);font-size:10px;">20</td><td style="font-weight:500;">Impacto estimado</td><td><span class="badge badge-green">Número</span></td><td>Valor monetario RD$ / USD</td><td>RD$ 450,000</td></tr>
              </tbody>
            </table>
          </div>
        </div>
      </div><!-- /tablero -->

      <!-- ═══════════════════════ CODIGOS ═══════════════════════ -->
      <div id="codigos" class="section">
        <div class="code-anatomy">
          <div class="code-seg-wrap">
            <div class="code-seg" style="background:rgba(27,110,194,0.15);color:#6EB4F0;">AUD</div>
            <div class="code-seg-label">Prefijo</div>
          </div>
          <div class="code-sep">—</div>
          <div class="code-seg-wrap">
            <div class="code-seg" style="background:var(--amber-dim);color:var(--gold2);">2026</div>
            <div class="code-seg-label">Año</div>
          </div>
          <div class="code-sep">—</div>
          <div class="code-seg-wrap">
            <div class="code-seg" style="background:var(--purple-dim);color:var(--purple);">FIN</div>
            <div class="code-seg-label">Área</div>
          </div>
          <div class="code-sep">—</div>
          <div class="code-seg-wrap">
            <div class="code-seg" style="background:var(--red-dim);color:var(--red);">CR</div>
            <div class="code-seg-label">Riesgo</div>
          </div>
          <div class="code-sep">—</div>
          <div class="code-seg-wrap">
            <div class="code-seg" style="background:var(--green-dim);color:var(--green);">001</div>
            <div class="code-seg-label">Secuencia</div>
          </div>
        </div>
        <div style="text-align:center;font-family:var(--mono);font-size:14px;color:var(--text3);margin-bottom:22px;">
          Resultado: <span style="color:var(--gold2);font-weight:700;font-size:16px;">AUD-2026-FIN-CR-001</span>
        </div>

        <div class="grid-2">
          <div class="card">
            <div class="card-title" style="margin-bottom:12px;">Códigos de área</div>
            <table>
              <thead><tr><th>Código</th><th>Área</th></tr></thead>
              <tbody>
                <tr><td><span class="code">FIN</span></td><td>Finanzas / Contabilidad</td></tr>
                <tr><td><span class="code">TES</span></td><td>Tesorería / Pagos</td></tr>
                <tr><td><span class="code">PRD</span></td><td>Producción / Operaciones</td></tr>
                <tr><td><span class="code">COM</span></td><td>Compras / Proveedores</td></tr>
                <tr><td><span class="code">INV</span></td><td>Inventario / Almacén</td></tr>
                <tr><td><span class="code">VEN</span></td><td>Ventas / Clientes</td></tr>
                <tr><td><span class="code">TI</span></td><td>Tecnología / Sistemas</td></tr>
                <tr><td><span class="code">RRH</span></td><td>Recursos Humanos</td></tr>
                <tr><td><span class="code">LEG</span></td><td>Legal / Cumplimiento</td></tr>
                <tr><td><span class="code">OPS</span></td><td>Operaciones Generales</td></tr>
              </tbody>
            </table>
          </div>
          <div class="card">
            <div class="card-title" style="margin-bottom:12px;">Códigos de riesgo</div>
            <table>
              <thead><tr><th>Código</th><th>Nivel</th><th>Plazo</th></tr></thead>
              <tbody>
                <tr><td><span class="badge badge-red" style="font-family:var(--mono);">CR</span></td><td>Crítico</td><td style="font-family:var(--mono);font-size:11px;color:var(--red);">Inmediato</td></tr>
                <tr><td><span class="badge badge-amber" style="font-family:var(--mono);">AL</span></td><td>Alto</td><td style="font-family:var(--mono);font-size:11px;color:var(--amber);">30 días</td></tr>
                <tr><td><span class="badge badge-blue" style="font-family:var(--mono);">ME</span></td><td>Medio</td><td style="font-family:var(--mono);font-size:11px;color:#6EB4F0;">60 días</td></tr>
                <tr><td><span class="badge badge-green" style="font-family:var(--mono);">BA</span></td><td>Bajo</td><td style="font-family:var(--mono);font-size:11px;color:var(--green);">90 días</td></tr>
              </tbody>
            </table>
            <div class="divider"></div>
            <div class="card-title" style="margin-bottom:10px;">Ejemplos reales</div>
            <div style="display:flex;flex-direction:column;gap:7px;">
              <div style="display:flex;align-items:center;gap:8px;font-size:12px;"><span class="code">AUD-2026-FIN-CR-001</span><span style="color:var(--text3);">Conciliación bancaria</span></div>
              <div style="display:flex;align-items:center;gap:8px;font-size:12px;"><span class="code">AUD-2026-INV-AL-002</span><span style="color:var(--text3);">Inventario sin custodia</span></div>
              <div style="display:flex;align-items:center;gap:8px;font-size:12px;"><span class="code">AUD-2026-TI-ME-003</span><span style="color:var(--text3);">Accesos ERP sin revisar</span></div>
              <div style="display:flex;align-items:center;gap:8px;font-size:12px;"><span class="code">AUD-2026-COM-BA-004</span><span style="color:var(--text3);">Expedientes incompletos</span></div>
            </div>
          </div>
        </div>
      </div><!-- /codigos -->

      <!-- ═══════════════════════ WORKFLOW ═══════════════════════ -->
      <div id="workflow" class="section">
        <div class="card">
          <div class="wf-list">
            <div class="wf-item">
              <div class="wf-spine">
                <div class="wf-num" style="background:rgba(27,110,194,0.2);color:#6EB4F0;border:1px solid var(--accent);">1</div>
                <div class="wf-line"></div>
              </div>
              <div class="wf-body">
                <div class="wf-title">Identificación del hallazgo</div>
                <div class="wf-desc">Durante la ejecución de pruebas de auditoría, el auditor detecta una desviación respecto al criterio establecido (política, norma, COSO). Documenta la condición observada, criterio de referencia, causa raíz y efecto potencial en los papeles de trabajo.</div>
                <div class="wf-chips"><span class="badge badge-blue">Papeles de trabajo</span><span class="badge badge-blue">Criterio COSO</span><span class="badge badge-blue">Evidencia inicial</span></div>
              </div>
            </div>
            <div class="wf-item">
              <div class="wf-spine">
                <div class="wf-num" style="background:var(--purple-dim);color:var(--purple);border:1px solid var(--purple);">2</div>
                <div class="wf-line"></div>
              </div>
              <div class="wf-body">
                <div class="wf-title">Registro y codificación</div>
                <div class="wf-desc">Registrar en el tablero dentro de las 24h de identificado. Asignar código según nomenclatura AUD-AAAA-ÁREA-RIESGO-NNN. Clasificar nivel de riesgo, tipo de control y componente COSO. Estimar impacto monetario si aplica.</div>
                <div class="wf-chips"><span class="badge badge-purple">Tablero principal</span><span class="badge badge-purple">Plazo: 24h</span><span class="badge badge-purple">Codificación</span></div>
              </div>
            </div>
            <div class="wf-item">
              <div class="wf-spine">
                <div class="wf-num" style="background:var(--amber-dim);color:var(--amber);border:1px solid var(--amber);">3</div>
                <div class="wf-line"></div>
              </div>
              <div class="wf-body">
                <div class="wf-title">Comunicación inicial al área</div>
                <div class="wf-desc">Enviar correo formal al gerente del área con el hallazgo preliminar. Solicitar reunión de presentación en máximo 3 días hábiles. Acordar plan de acción y fecha de compromiso. Documentar el plan acordado en el tablero.</div>
                <div class="wf-chips"><span class="badge badge-amber">Email formal</span><span class="badge badge-amber">Reunión presencial</span><span class="badge badge-amber">Acuerdo de plan</span></div>
              </div>
            </div>
            <div class="wf-item">
              <div class="wf-spine">
                <div class="wf-num" style="background:rgba(27,110,194,0.2);color:#6EB4F0;border:1px solid var(--accent);">4</div>
                <div class="wf-line"></div>
              </div>
              <div class="wf-body">
                <div class="wf-title">Seguimiento activo</div>
                <div class="wf-desc">Monitoreo semanal del avance. Actualizar % de avance, estado y fecha de última actualización en el tablero. Enviar recordatorio semanal si no hay avance documentado. Activar protocolo de escalamiento según reglas de días de retraso.</div>
                <div class="wf-chips"><span class="badge badge-blue">Revisión semanal</span><span class="badge badge-blue">Actualización tablero</span><span class="badge badge-blue">Recordatorios</span></div>
              </div>
            </div>
            <div class="wf-item">
              <div class="wf-spine">
                <div class="wf-num" style="background:var(--teal-dim);color:var(--teal);border:1px solid var(--teal);">5</div>
                <div class="wf-line"></div>
              </div>
              <div class="wf-body">
                <div class="wf-title">Validación de evidencias</div>
                <div class="wf-desc">El responsable envía evidencias de remediación. El auditor valida que son suficientes, pertinentes y demuestran que la causa raíz fue eliminada. Si la evidencia es insuficiente, se solicita información adicional por escrito.</div>
                <div class="wf-chips"><span class="badge badge-teal">Verificación independiente</span><span class="badge badge-teal">Suficiencia</span><span class="badge badge-teal">Prueba de efectividad</span></div>
              </div>
            </div>
            <div class="wf-item">
              <div class="wf-spine">
                <div class="wf-num" style="background:var(--green-dim);color:var(--green);border:1px solid var(--green);">6</div>
              </div>
              <div class="wf-body" style="padding-bottom:0;">
                <div class="wf-title">Cierre formal</div>
                <div class="wf-desc">Actualizar estado a "Cerrado" en tablero. Registrar fecha de cierre real vs. compromiso. Enviar correo de cierre formal al responsable y a Dirección. Archivar evidencias en expediente electrónico. Incluir en reporte ejecutivo mensual.</div>
                <div class="wf-chips"><span class="badge badge-green">Estado: Cerrado</span><span class="badge badge-green">Archivo evidencias</span><span class="badge badge-green">Notificación Dirección</span></div>
              </div>
            </div>
          </div>
        </div>
      </div><!-- /workflow -->

      <!-- ═══════════════════════ COMUNICACION ═══════════════════════ -->
      <div id="comunicacion" class="section">
        <div class="comm-cols">
          <div class="comm-col">
            <div class="comm-col-header" style="background:rgba(27,110,194,0.15);color:#6EB4F0;">Rutina diaria</div>
            <div class="comm-col-body">
              <div style="font-size:10px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.08em;margin-bottom:8px;">Cada mañana · 09:00 h</div>
              <div class="comm-row"><div class="comm-dot" style="background:#6EB4F0;"></div>Revisar tablero: hallazgos que vencen hoy o mañana</div>
              <div class="comm-row"><div class="comm-dot" style="background:#6EB4F0;"></div>Actualizar días de retraso y última actualización</div>
              <div class="comm-row"><div class="comm-dot" style="background:#6EB4F0;"></div>Enviar recordatorio puntual a responsables con vencimiento inminente</div>
              <div class="comm-row"><div class="comm-dot" style="background:#6EB4F0;"></div>Revisar correos con evidencias o consultas</div>
              <div class="comm-row"><div class="comm-dot" style="background:#6EB4F0;"></div>Registrar nuevas evidencias en tablero</div>
            </div>
            <div class="comm-time">Duración: 20–30 min</div>
          </div>
          <div class="comm-col">
            <div class="comm-col-header" style="background:var(--purple-dim);color:var(--purple);">Rutina semanal</div>
            <div class="comm-col-body">
              <div style="font-size:10px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.08em;margin-bottom:8px;">Cada lunes · 10:00 h</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--purple);"></div>Generar reporte semanal de todos los hallazgos</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--purple);"></div>Enviar correo de seguimiento semanal</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--purple);"></div>Activar escalamientos según días de retraso</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--purple);"></div>Actualizar métricas del tablero ejecutivo</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--purple);"></div>Enviar resumen al Director Financiero</div>
            </div>
            <div class="comm-time">Duración: 1.5–2 horas</div>
          </div>
          <div class="comm-col">
            <div class="comm-col-header" style="background:var(--amber-dim);color:var(--amber);">Rutina mensual</div>
            <div class="comm-col-body">
              <div style="font-size:10px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.08em;margin-bottom:8px;">Último viernes del mes</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--amber);"></div>Elaborar Reporte Ejecutivo Mensual</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--amber);"></div>Calcular KPIs del mes</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--amber);"></div>Presentar a Dirección / Comité de Auditoría</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--amber);"></div>Archivar evidencias del mes</div>
              <div class="comm-row"><div class="comm-dot" style="background:var(--amber);"></div>Identificar tendencias y áreas de riesgo recurrentes</div>
            </div>
            <div class="comm-time">Duración: 4–6 horas</div>
          </div>
        </div>

        <div class="card">
          <div class="card-title" style="margin-bottom:14px;">Matriz de comunicación por nivel de riesgo</div>
          <div class="table-wrap">
            <table>
              <thead><tr><th>Nivel</th><th>Comunicación inicial</th><th>Frecuencia seguimiento</th><th>Escalamiento</th><th>Audiencia mínima</th></tr></thead>
              <tbody>
                <tr><td><span class="badge badge-red">Crítico</span></td><td>Reunión en 24h + email</td><td>Cada 2 días</td><td>A los 5 días sin avance</td><td>Director General + Gerente</td></tr>
                <tr><td><span class="badge badge-amber">Alto</span></td><td>Email formal + reunión 3 días</td><td>Semanal</td><td>A los 10 días sin avance</td><td>Director Financiero + Gerente</td></tr>
                <tr><td><span class="badge badge-blue">Medio</span></td><td>Email formal</td><td>Quincenal</td><td>A los 15 días sin avance</td><td>Gerente del área</td></tr>
                <tr><td><span class="badge badge-green">Bajo</span></td><td>Email informativo</td><td>Mensual</td><td>A los 30 días sin avance</td><td>Jefe del área</td></tr>
              </tbody>
            </table>
          </div>
        </div>
      </div><!-- /comunicacion -->

      <!-- ═══════════════════════ EMAILS ═══════════════════════ -->
      <div id="emails" class="section">
        <div class="email-tabs">
          <button class="email-tab active" onclick="showEmail(0,this)">Solicitud de información</button>
          <button class="email-tab" onclick="showEmail(1,this)">Seguimiento semanal</button>
          <button class="email-tab" onclick="showEmail(2,this)">Escalamiento</button>
          <button class="email-tab" onclick="showEmail(3,this)">Cierre de hallazgo</button>
        </div>
        <div id="email-display"></div>
      </div><!-- /emails -->

      <!-- ═══════════════════════ ESCALAMIENTO ═══════════════════════ -->
      <div id="escalamiento" class="section">
        <div class="esc-grid">
          <div class="esc-row" style="background:var(--green-dim);border-color:rgba(46,204,122,0.3);">
            <div><div class="esc-days-big" style="color:var(--green);">1–7</div><div class="esc-days-label" style="color:var(--green);">días</div></div>
            <div><div class="esc-rule-title" style="color:var(--green);">Nivel 0 — Sin escalar</div><div class="esc-rule-desc" style="color:var(--text2);">Seguimiento normal. Correo de recordatorio semanal al responsable directo. Sin copia a superiores.</div></div>
            <div><div class="esc-who-label">A quién</div><div class="esc-who" style="color:var(--text);">Responsable directo del hallazgo</div></div>
          </div>
          <div class="esc-row" style="background:var(--amber-dim);border-color:rgba(212,129,26,0.3);">
            <div><div class="esc-days-big" style="color:var(--amber);">8–15</div><div class="esc-days-label" style="color:var(--amber);">días</div></div>
            <div><div class="esc-rule-title" style="color:var(--amber);">Nivel 1 — Escalamiento a Gerencia</div><div class="esc-rule-desc" style="color:var(--text2);">Correo formal copiando al Gerente del área. Solicitar nueva fecha de compromiso con justificación escrita.</div></div>
            <div><div class="esc-who-label">A quién</div><div class="esc-who" style="color:var(--text);">Responsable + Gerente del área</div></div>
          </div>
          <div class="esc-row" style="background:var(--red-dim);border-color:rgba(224,82,82,0.3);">
            <div><div class="esc-days-big" style="color:var(--red);">16–30</div><div class="esc-days-label" style="color:var(--red);">días</div></div>
            <div><div class="esc-rule-title" style="color:var(--red);">Nivel 2 — Escalamiento a Dirección</div><div class="esc-rule-desc" style="color:var(--text2);">Notificación formal al Director Financiero o Director General. Incluir en reporte ejecutivo como hallazgo crítico pendiente.</div></div>
            <div><div class="esc-who-label">A quién</div><div class="esc-who" style="color:var(--text);">Responsable + Gerente + Director Financiero</div></div>
          </div>
          <div class="esc-row" style="background:var(--purple-dim);border-color:rgba(139,127,240,0.3);">
            <div><div class="esc-days-big" style="color:var(--purple);">+30</div><div class="esc-days-label" style="color:var(--purple);">días</div></div>
            <div><div class="esc-rule-title" style="color:var(--purple);">Nivel 3 — Máxima Autoridad</div><div class="esc-rule-desc" style="color:var(--text2);">Escalamiento al Director General / Comité de Auditoría / Junta Directiva. Evaluar exposición legal y documentar en acta.</div></div>
            <div><div class="esc-who-label">A quién</div><div class="esc-who" style="color:var(--text);">Director General / Junta / Comité</div></div>
          </div>
        </div>
        <div class="card">
          <div class="card-title" style="margin-bottom:14px;">Principios del escalamiento</div>
          <div class="grid-2" style="gap:10px;">
            <div style="padding:12px;background:var(--card2);border-radius:8px;font-size:12px;line-height:1.6;color:var(--text2);"><strong style="color:var(--text);display:block;margin-bottom:3px;">Siempre por escrito</strong>Todo escalamiento queda documentado en email con copia al expediente. Nunca verbal.</div>
            <div style="padding:12px;background:var(--card2);border-radius:8px;font-size:12px;line-height:1.6;color:var(--text2);"><strong style="color:var(--text);display:block;margin-bottom:3px;">Tono profesional</strong>El objetivo es resolver el hallazgo. Framing: "apoyo para gestionar", no "incumplimiento de".</div>
            <div style="padding:12px;background:var(--card2);border-radius:8px;font-size:12px;line-height:1.6;color:var(--text2);"><strong style="color:var(--text);display:block;margin-bottom:3px;">Contexto completo</strong>Adjuntar historial de comunicaciones previas, fechas acordadas y compromisos incumplidos.</div>
            <div style="padding:12px;background:var(--card2);border-radius:8px;font-size:12px;line-height:1.6;color:var(--text2);"><strong style="color:var(--text);display:block;margin-bottom:3px;">Aceleración para críticos</strong>Hallazgos CR: escalar a Gerencia a los 5 días, a Dirección a los 10. Plazos normales no aplican.</div>
          </div>
        </div>
      </div><!-- /escalamiento -->

      <!-- ═══════════════════════ KPIs ═══════════════════════ -->
      <div id="kpis" class="section">
        <div class="card">
          <div class="card-title" style="margin-bottom:16px;">KPIs estratégicos del sistema de auditoría</div>
          <div>
            <div class="kpi-table-row" style="padding-top:0;font-size:9px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.1em;border-bottom:1px solid var(--border2);">
              <div>Indicador</div><div>Fórmula · Interpretación</div><div style="text-align:center;">Meta</div>
            </div>
            <div class="kpi-table-row">
              <div><div class="kpi-t-name">Tasa de remediación</div><div class="kpi-t-desc">Principal KPI de efectividad</div></div>
              <div><div class="formula-pill">(Cerrados / Total) × 100</div><div class="kpi-t-interpret">60%: gestión insuficiente · 80%+: sólida. Mide si el sistema realmente cierra hallazgos o solo los acumula.</div></div>
              <div style="text-align:center;"><span class="badge badge-green">≥ 80%</span></div>
            </div>
            <div class="kpi-table-row">
              <div><div class="kpi-t-name">% Hallazgos vencidos</div><div class="kpi-t-desc">Señal de resistencia organizacional</div></div>
              <div><div class="formula-pill">(Vencidos / Activos) × 100</div><div class="kpi-t-interpret">+25%: problema sistémico de cultura. Identificar si los vencidos se concentran en un área específica.</div></div>
              <div style="text-align:center;"><span class="badge badge-red">≤ 15%</span></div>
            </div>
            <div class="kpi-table-row">
              <div><div class="kpi-t-name">Tiempo promedio cierre</div><div class="kpi-t-desc">Eficiencia de remediación</div></div>
              <div><div class="formula-pill">Σ(Cierre - Identificación) / N</div><div class="kpi-t-interpret">Comparar contra el plazo comprometido. Una mejora mensual indica madurez del sistema.</div></div>
              <div style="text-align:center;"><span class="badge badge-blue">Tendencia ↓</span></div>
            </div>
            <div class="kpi-table-row">
              <div><div class="kpi-t-name">Tasa de recurrencia</div><div class="kpi-t-desc">Calidad de la remediación</div></div>
              <div><div class="formula-pill">(Reabiertos / Cerrados) × 100</div><div class="kpi-t-interpret">+10%: las remediaciones no eliminan la causa raíz. Investigar profundidad de los planes de acción.</div></div>
              <div style="text-align:center;"><span class="badge badge-red">≤ 5%</span></div>
            </div>
            <div class="kpi-table-row">
              <div><div class="kpi-t-name">Críticos abiertos</div><div class="kpi-t-desc">Exposición de riesgo máxima</div></div>
              <div><div class="formula-pill">CONTAR.SI(Riesgo="CR", Estado≠"Cerrado")</div><div class="kpi-t-interpret">Cualquier número > 0 requiere reporte inmediato a Dirección. Tolerancia cero.</div></div>
              <div style="text-align:center;"><span class="badge badge-red">= 0</span></div>
            </div>
            <div class="kpi-table-row">
              <div><div class="kpi-t-name">Cumplimiento comunicación</div><div class="kpi-t-desc">Disciplina del auditor</div></div>
              <div><div class="formula-pill">(Enviados a tiempo / Programados) × 100</div><div class="kpi-t-interpret">Mide si el auditor sigue su propia rutina. Base para demostrar proactividad ante Dirección.</div></div>
              <div style="text-align:center;"><span class="badge badge-green">= 100%</span></div>
            </div>
            <div class="kpi-table-row">
              <div><div class="kpi-t-name">Cobertura plan anual</div><div class="kpi-t-desc">Amplitud del plan de auditoría</div></div>
              <div><div class="formula-pill">(Áreas auditadas YTD / Planificadas) × 100</div><div class="kpi-t-interpret">Debajo del 70% en junio = plan en riesgo. Presentar en reporte mensual a Dirección.</div></div>
              <div style="text-align:center;"><span class="badge badge-blue">≥ Plan</span></div>
            </div>
            <div class="kpi-table-row" style="border-bottom:none;">
              <div><div class="kpi-t-name">Impacto financiero gestionado</div><div class="kpi-t-desc">ROI de auditoría interna</div></div>
              <div><div class="formula-pill">Σ impacto_estimado hallazgos cerrados</div><div class="kpi-t-interpret">KPI estratégico: demuestra el valor que genera auditoría. Presentar en informe anual a Junta.</div></div>
              <div style="text-align:center;"><span class="badge badge-gold">Máximo</span></div>
            </div>
          </div>
        </div>
      </div><!-- /kpis -->

      <!-- ═══════════════════════ REPORTE ═══════════════════════ -->
      <div id="reporte" class="section">
        <div class="report-grid">
          <div class="card report-toc">
            <div class="card-title" style="margin-bottom:12px;">Estructura del informe</div>
            <button class="toc-item"><span class="toc-num">I</span>Resumen ejecutivo</button>
            <button class="toc-item"><span class="toc-num">II</span>KPIs del período</button>
            <button class="toc-item"><span class="toc-num">III</span>Hallazgos activos</button>
            <button class="toc-item"><span class="toc-num">IV</span>Hallazgos cerrados</button>
            <button class="toc-item"><span class="toc-num">V</span>Escalamientos</button>
            <button class="toc-item"><span class="toc-num">VI</span>Análisis de tendencias</button>
            <button class="toc-item"><span class="toc-num">VII</span>Agenda próximo mes</button>
            <button class="toc-item"><span class="toc-num">VIII</span>Decisiones requeridas</button>
          </div>
          <div class="report-sections">
            <div class="report-block">
              <div class="report-block-title">I. Resumen ejecutivo (½ página máx.)</div>
              <div class="report-block-desc">4 párrafos breves: (1) Estado general del sistema de control interno del mes. (2) Hallazgos identificados, cerrados y pendientes. (3) Principales riesgos que ameritan atención de Dirección. (4) Una recomendación estratégica.<br><br><em style="color:var(--text3);">"Durante febrero 2026 se identificaron 6 nuevos hallazgos, de los cuales 2 son de riesgo crítico relacionados con el área de Tesorería. Se cerraron 4 hallazgos del mes anterior, alcanzando una tasa de remediación del 73%. Se recomienda a Dirección revisar el control de aprobaciones en pagos superiores a RD$ 500,000."</em></div>
            </div>
            <div class="report-block">
              <div class="report-block-title">II. KPIs del período</div>
              <div class="report-block-desc">Tabla con los 7 KPIs vs. meta y vs. mes anterior. Semáforo de estado (verde/amarillo/rojo). Gráfico de tendencia de tasa de remediación en los últimos 6 meses. Este es el bloque que más impacta visualmente ante Dirección.</div>
            </div>
            <div class="report-block">
              <div class="report-block-title">VI. Análisis de tendencias (sección estratégica)</div>
              <div class="report-block-desc">Comparar hallazgos del mes vs. mes anterior y vs. mismo mes del año pasado. Identificar áreas con hallazgos recurrentes (señal de debilidad estructural). Esta sección <strong style="color:var(--gold2);">posiciona al auditor como estratégico</strong>, no solo reportero. Es la diferencia entre un auditor que informa y uno que asesora.</div>
            </div>
            <div class="report-block">
              <div class="report-block-title">VIII. Decisiones requeridas de Dirección</div>
              <div class="report-block-desc">Listar explícitamente qué necesita aprobación o decisión de la alta dirección. Ejemplo: <em style="color:var(--text3);">"Se requiere aprobación para rediseñar el proceso de aprobación de pagos en Tesorería, estimado en 2 semanas de implementación."</em><br><br>Esta sección garantiza que el informe sea <strong style="color:var(--gold2);">accionable, no solo informativo.</strong></div>
            </div>
          </div>
        </div>
      </div><!-- /reporte -->

      <!-- ═══════════════════════ AUTOMATIZACION ═══════════════════════ -->
      <div id="automatizacion" class="section">

        <div class="auto-section">
          <div class="auto-header">
            <div class="auto-badge" style="background:var(--green-dim);color:var(--green);">EXCEL</div>
            <div class="auto-title-text">Fórmulas esenciales</div>
          </div>
          <div class="grid-2" style="gap:10px;">
            <div>
              <div class="auto-label">Días de retraso (automático)</div>
              <div class="formula-block">=SI([@Estado]&lt;&gt;"Cerrado", MAX(0, HOY()-[@[Fecha límite]]), "")</div>
              <div style="font-size:11px;color:var(--text3);margin-top:3px;">Calcula días vencidos solo si no está cerrado.</div>
            </div>
            <div>
              <div class="auto-label">Nivel de escalamiento (automático)</div>
              <div class="formula-block">=SI([@[Días retraso]]&gt;30,"Junta",SI([@[Días retraso]]&gt;15,"Dirección",SI([@[Días retraso]]&gt;7,"Gerencia","Sin escalar")))</div>
            </div>
            <div>
              <div class="auto-label">Semáforo de estado</div>
              <div class="formula-block">=SI([@[Días retraso]]&gt;15,"🔴 CRÍTICO",SI([@[Días retraso]]&gt;7,"🟡 ATENCIÓN","🟢 OK"))</div>
              <div style="font-size:11px;color:var(--text3);margin-top:3px;">Aplicar formato condicional con los mismos rangos.</div>
            </div>
            <div>
              <div class="auto-label">Alerta vencimiento próximo (7 días)</div>
              <div class="formula-block">=SI(Y([@[Fecha límite]]-HOY()&lt;=7, [@Estado]&lt;&gt;"Cerrado"), "⚠ VENCE PRONTO", "")</div>
            </div>
            <div>
              <div class="auto-label">Tasa de remediación (dashboard)</div>
              <div class="formula-block">=CONTAR.SI(Hallazgos[Estado],"Cerrado")/CONTARA(Hallazgos[Código])</div>
            </div>
            <div>
              <div class="auto-label">Impacto financiero gestionado</div>
              <div class="formula-block">=SUMAR.SI(Hallazgos[Estado],"Cerrado",Hallazgos[Impacto])</div>
            </div>
          </div>
        </div>

        <div class="auto-section">
          <div class="auto-header">
            <div class="auto-badge" style="background:var(--purple-dim);color:var(--purple);">NOTION</div>
            <div class="auto-title-text">Configuración recomendada</div>
          </div>
          <div class="grid-2" style="gap:10px;">
            <div class="card">
              <div class="card-title" style="margin-bottom:10px;">Propiedades</div>
              <div style="display:flex;flex-direction:column;gap:6px;font-size:12px;color:var(--text2);">
                <div style="display:flex;gap:8px;align-items:center;"><span class="badge badge-blue" style="font-family:var(--mono);font-size:9px;">Texto</span>Código, Responsable, Plan de acción</div>
                <div style="display:flex;gap:8px;align-items:center;"><span class="badge badge-purple" style="font-family:var(--mono);font-size:9px;">Select</span>Área, Riesgo, Estado, Escalamiento</div>
                <div style="display:flex;gap:8px;align-items:center;"><span class="badge badge-amber" style="font-family:var(--mono);font-size:9px;">Fecha</span>Identificación, Límite, Cierre</div>
                <div style="display:flex;gap:8px;align-items:center;"><span class="badge badge-green" style="font-family:var(--mono);font-size:9px;">Number</span>% Avance, Días retraso, Impacto $</div>
                <div style="display:flex;gap:8px;align-items:center;"><span class="badge badge-red" style="font-family:var(--mono);font-size:9px;">Formula</span>Días retraso automático</div>
                <div style="display:flex;gap:8px;align-items:center;"><span class="badge badge-gray" style="font-family:var(--mono);font-size:9px;">Files</span>Evidencias adjuntas</div>
              </div>
            </div>
            <div class="card">
              <div class="card-title" style="margin-bottom:10px;">Vistas recomendadas</div>
              <div style="display:flex;flex-direction:column;gap:7px;font-size:12px;color:var(--text2);">
                <div><strong style="color:var(--text);">Board:</strong> Columnas por Estado. Vista diaria del auditor.</div>
                <div><strong style="color:var(--text);">Table:</strong> Todos los campos. Vista completa de gestión.</div>
                <div><strong style="color:var(--text);">Calendar:</strong> Por Fecha límite. Detectar acumulación.</div>
                <div><strong style="color:var(--text);">Gallery:</strong> Por Área. Distribución visual de riesgo.</div>
                <div><strong style="color:var(--text);">Filter vencidos:</strong> Estado ≠ Cerrado AND Días &gt; 0</div>
              </div>
            </div>
          </div>
        </div>

        <div class="auto-section">
          <div class="auto-header">
            <div class="auto-badge" style="background:var(--amber-dim);color:var(--amber);">AIRTABLE</div>
            <div class="auto-title-text">Automatizaciones nativas</div>
          </div>
          <div style="display:flex;flex-direction:column;gap:10px;">
            <div class="card" style="padding:14px 16px;">
              <div style="font-size:12px;font-weight:600;color:var(--gold2);margin-bottom:5px;">Automatización 1: Recordatorio de vencimiento</div>
              <div style="font-size:12px;color:var(--text2);line-height:1.6;"><strong style="color:var(--text);">Trigger:</strong> 3 días antes de Fecha límite &nbsp;·&nbsp; <strong style="color:var(--text);">Acción:</strong> Email automático al Responsable con detalle del hallazgo y link directo al registro.</div>
            </div>
            <div class="card" style="padding:14px 16px;">
              <div style="font-size:12px;font-weight:600;color:var(--gold2);margin-bottom:5px;">Automatización 2: Cambio automático a Vencido</div>
              <div style="font-size:12px;color:var(--text2);line-height:1.6;"><strong style="color:var(--text);">Trigger:</strong> Fecha límite &lt; HOY() AND Estado = "En proceso" &nbsp;·&nbsp; <strong style="color:var(--text);">Acción:</strong> Cambiar Estado a "Vencido" + notificación al auditor.</div>
            </div>
            <div class="card" style="padding:14px 16px;">
              <div style="font-size:12px;font-weight:600;color:var(--gold2);margin-bottom:5px;">Automatización 3: Reporte semanal</div>
              <div style="font-size:12px;color:var(--text2);line-height:1.6;"><strong style="color:var(--text);">Trigger:</strong> Cada lunes 08:00 am &nbsp;·&nbsp; <strong style="color:var(--text);">Acción:</strong> Generar vista filtrada de hallazgos activos y enviar resumen por email al auditor.</div>
            </div>
            <div class="card" style="padding:14px 16px;">
              <div style="font-size:12px;font-weight:600;color:var(--gold2);margin-bottom:5px;">Automatización 4: Alerta nuevo hallazgo crítico</div>
              <div style="font-size:12px;color:var(--text2);line-height:1.6;"><strong style="color:var(--text);">Trigger:</strong> Nuevo registro con Nivel_riesgo = "Crítico" &nbsp;·&nbsp; <strong style="color:var(--text);">Acción:</strong> Notificación inmediata SMS/email al auditor y copia al Director Financiero.</div>
            </div>
          </div>
        </div>

      </div><!-- /automatizacion -->

    </div><!-- /content -->
  </div><!-- /main -->
</div><!-- /app -->

<script>
const topbarData = {
  dashboard:      { title: 'Dashboard ejecutivo', sub: 'Vista general del sistema de control interno' },
  tablero:        { title: 'Tablero de seguimiento', sub: 'Estructura de campos · Tipos de datos · Ejemplos' },
  codigos:        { title: 'Sistema de codificación', sub: 'Nomenclatura estandarizada de hallazgos' },
  workflow:       { title: 'Flujo de trabajo', sub: 'Desde identificación hasta cierre formal' },
  comunicacion:   { title: 'Rutina de comunicación', sub: 'Cadencia diaria, semanal y mensual' },
  emails:         { title: 'Plantillas de email', sub: '4 correos profesionales listos para usar' },
  escalamiento:   { title: 'Sistema de escalamiento', sub: 'Reglas por días de retraso · 4 niveles' },
  kpis:           { title: 'Indicadores KPI', sub: 'Fórmulas exactas · Metas · Interpretación' },
  reporte:        { title: 'Reporte ejecutivo mensual', sub: 'Estructura para presentar a Dirección' },
  automatizacion: { title: 'Automatización', sub: 'Excel · Notion · Airtable' },
};

function show(id, btn) {
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if (btn) btn.classList.add('active');
  const d = topbarData[id];
  if (d) {
    document.getElementById('topbar-title').textContent = d.title;
    document.getElementById('topbar-sub').textContent = d.sub;
  }
}

const emails = [
  {
    label: 'Solicitud de información',
    para: 'gerente.area@empresa.com',
    asunto: 'Solicitud de información — Auditoría Interna [AUD-2026-FIN-001]',
    badge: 'badge-blue',
    cuerpo: `Estimado/a [Nombre del Gerente],

Espero que se encuentre bien. Me dirijo a usted en el marco del Plan de Auditoría 2026, específicamente en relación con la revisión del proceso de [NOMBRE DEL PROCESO] correspondiente al área de [ÁREA].

Para continuar con el desarrollo de nuestra evaluación, requiero que nos haga llegar la siguiente información a más tardar el día [FECHA]:

  • [Documento 1: descripción específica]
  • [Documento 2: descripción específica]
  • [Acceso o reporte del sistema: descripción]

El objetivo de esta revisión es fortalecer los controles internos del área y asegurar el cumplimiento de las políticas institucionales. Toda la información será tratada con estricta confidencialidad.

Si tiene alguna consulta sobre los documentos solicitados o necesita una reunión para aclarar el alcance, quedo a su disposición para coordinar un espacio en agenda.

Agradezco de antemano su colaboración.

Atentamente,
[Nombre del Auditor]
Auditoría Interna
[Empresa] | Ext. [XXX]`
  },
  {
    label: 'Seguimiento semanal',
    para: 'responsable@empresa.com; gerente@empresa.com',
    asunto: 'Seguimiento semanal — Hallazgos de auditoría asignados [Semana del 17-03-2026]',
    badge: 'badge-purple',
    cuerpo: `Estimado/a [Nombre],

Le envío el estado actualizado de los hallazgos de auditoría asignados a su área correspondientes a esta semana.

HALLAZGOS BAJO SU RESPONSABILIDAD:

  ▸ AUD-2026-FIN-CR-001 | Conciliaciones bancarias Q4
    Estado: En proceso (60% avance)
    Fecha límite: 31/03/2026 | Días restantes: 14
    Acción requerida: Envío de conciliaciones de diciembre

  ▸ AUD-2026-FIN-AL-003 | Segregación de funciones ERP
    Estado: Pendiente (0% avance)
    Fecha límite: 15/04/2026 | Días restantes: 29
    Acción requerida: Propuesta de roles actualizada

Agradecería recibir una actualización de avance a más tardar el próximo miércoles [FECHA]. De no recibir respuesta, procederé a registrar el estado sin actualización en el sistema de seguimiento.

Si existe algún impedimento para cumplir con las fechas comprometidas, le pido nos lo comunique con anticipación para evaluar opciones de gestión.

Quedo disponible para cualquier consulta.

Cordialmente,
[Nombre del Auditor]
Auditoría Interna`
  },
  {
    label: 'Escalamiento',
    para: 'director@empresa.com',
    cc: 'gerente.area@empresa.com; responsable@empresa.com',
    asunto: 'URGENTE — Escalamiento hallazgo vencido [AUD-2026-FIN-CR-001] | 18 días de retraso',
    badge: 'badge-red',
    cuerpo: `Estimado/a [Director/a General / Director/a Financiero/a],

Me dirijo a usted en razón del incumplimiento reiterado en la remediación del siguiente hallazgo de auditoría, que ha superado el umbral de tolerancia establecido en nuestra política de seguimiento.

DETALLE DEL HALLAZGO:

  Código:              AUD-2026-FIN-CR-001
  Descripción:         Conciliaciones bancarias de Q4 2025 sin ejecutar
  Nivel de riesgo:     CRÍTICO
  Responsable:         Juan Pérez — Gerente de Finanzas
  Fecha compromiso:    15/02/2026
  Días de retraso:     18 días
  Avance reportado:    0%

HISTORIAL DE COMUNICACIONES:
  • 16/02/2026 — Recordatorio vía email (sin respuesta)
  • 23/02/2026 — Segundo recordatorio + llamada (compromiso verbal sin cumplimiento)
  • 03/03/2026 — Escalamiento a Gerencia del área (sin respuesta documentada)

RIESGO ACTUAL:
La falta de conciliaciones bancarias por 2 trimestres expone a la empresa a errores en estados financieros, posibles irregularidades no detectadas y riesgo de auditoría externa.

SOLICITUD A DIRECCIÓN:
Se requiere su intervención para asegurar que el área ejecute las acciones correctivas en un plazo no mayor a 5 días hábiles a partir de la presente comunicación.

Adjunto: Historial completo de comunicaciones y detalle del hallazgo.

A su disposición para ampliar la información.

Atentamente,
[Nombre del Auditor]
Auditoría Interna`
  },
  {
    label: 'Cierre de hallazgo',
    para: 'responsable@empresa.com; director@empresa.com',
    asunto: 'Cierre formal de hallazgo [AUD-2026-FIN-CR-001] — Remediación satisfactoria',
    badge: 'badge-green',
    cuerpo: `Estimado/a [Nombre],

Me complace comunicarle que, tras la revisión de las evidencias presentadas, el hallazgo de auditoría que se detalla a continuación ha sido cerrado de manera satisfactoria.

HALLAZGO CERRADO:

  Código:            AUD-2026-FIN-CR-001
  Descripción:       Conciliaciones bancarias Q4 2025 sin ejecutar
  Fecha de apertura: 05/01/2026
  Fecha de cierre:   17/03/2026
  Responsable:       Juan Pérez — Gerente de Finanzas

EVIDENCIAS VALIDADAS:
  ✓ Conciliaciones bancarias de octubre, noviembre y diciembre 2025
  ✓ Firma de revisión del contador general
  ✓ Procedimiento actualizado de conciliación mensual
  ✓ Asignación de responsable permanente documentada

RESULTADO:
La acción correctiva implementada elimina la causa raíz identificada. El control queda operando de acuerdo al criterio establecido en la política institucional.

Este hallazgo queda registrado como CERRADO en el sistema de seguimiento de auditoría a partir de esta fecha.

Le agradecemos su colaboración y compromiso en la implementación de las acciones correctivas.

Atentamente,
[Nombre del Auditor]
Auditoría Interna`
  }
];

function showEmail(idx, btn) {
  document.querySelectorAll('.email-tab').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  const e = emails[idx];
  const cc = e.cc ? `<div class="email-meta"><strong>CC:</strong> ${e.cc}</div>` : '';
  document.getElementById('email-display').innerHTML = `
    <div class="email-box">
      <div class="email-head">
        <div class="email-meta"><strong>Para:</strong> ${e.para}</div>
        ${cc}
        <div class="email-subject">${e.asunto}</div>
        <div style="margin-top:6px;"><span class="badge ${e.badge}">${e.label}</span></div>
      </div>
      <div class="email-body">${e.cuerpo}</div>
    </div>`;
}

// Date
const now = new Date();
document.getElementById('date-badge').textContent =
  now.toLocaleDateString('es-DO', { day:'2-digit', month:'short', year:'numeric' });

// Init email
showEmail(0, document.querySelector('.email-tab'));
</script>
</body>
</html>
