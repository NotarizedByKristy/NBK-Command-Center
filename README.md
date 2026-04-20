<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>NBK Command Center</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root {
  --navy:#1B3A6B; --navy-dark:#0f2444; --navy-light:#264d8a;
  --gold:#C8972A; --gold-light:#e0aa3a; --gold-dim:#a07820; --gold-pale:#f5e9cc;
  --cream:#FAF8F3; --white:#fff;
  --gray-100:#f4f4f6; --gray-200:#e8e8ee; --gray-400:#9999aa; --gray-600:#555566;
  --red:#c0392b; --green:#1a7a4a; --blue:#2980b9; --purple:#8e44ad;
  --shadow:0 4px 24px rgba(27,58,107,0.12);
  --radius:14px; --radius-sm:8px;
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
html{scroll-behavior:smooth}
body{font-family:'DM Sans',sans-serif;background:var(--navy-dark);color:var(--white);min-height:100vh;max-width:520px;margin:0 auto}

/* ── LOGIN ── */
#loginScreen{position:fixed;inset:0;background:linear-gradient(160deg,var(--navy-dark),var(--navy));display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:9999;padding:30px}
#loginScreen .login-logo{font-size:48px;margin-bottom:16px}
#loginScreen h1{font-family:'Playfair Display',serif;font-size:26px;color:var(--gold);margin-bottom:6px;text-align:center}
#loginScreen p{font-size:13px;color:var(--gray-400);margin-bottom:32px;text-align:center;letter-spacing:.5px;text-transform:uppercase}
#loginScreen input{width:100%;background:rgba(255,255,255,.08);border:1.5px solid rgba(200,151,42,.4);border-radius:var(--radius-sm);padding:14px 16px;font-family:'DM Sans',sans-serif;font-size:16px;color:var(--white);text-align:center;letter-spacing:3px;margin-bottom:12px}
#loginScreen input:focus{outline:none;border-color:var(--gold);background:rgba(255,255,255,.12)}
#loginScreen input::placeholder{letter-spacing:1px;color:var(--gray-400)}
#loginBtn{width:100%;background:linear-gradient(135deg,var(--gold),var(--gold-light));color:var(--navy-dark);border:none;border-radius:var(--radius-sm);padding:14px;font-family:'DM Sans',sans-serif;font-size:15px;font-weight:700;cursor:pointer;margin-bottom:10px}
#loginError{font-size:13px;color:#e07070;text-align:center;min-height:18px}
#loginScreen .login-hint{font-size:11px;color:rgba(255,255,255,.2);margin-top:20px;text-align:center}

/* ── APP SHELL ── */
#appShell{display:none}
.app-header{background:linear-gradient(135deg,var(--navy-dark),var(--navy));padding:16px 18px 12px;position:sticky;top:0;z-index:100;border-bottom:2px solid var(--gold)}
.header-row{display:flex;align-items:center;justify-content:space-between}
.brand-name{font-family:'Playfair Display',serif;font-size:16px;font-weight:700;color:var(--gold);line-height:1.1}
.brand-sub{font-size:9px;color:var(--gray-400);letter-spacing:1.5px;text-transform:uppercase}
.header-right{display:flex;align-items:center;gap:10px}
.header-date{font-size:11px;color:var(--gray-400);text-align:right}
.logout-btn{background:none;border:1px solid rgba(200,151,42,.3);color:var(--gold);font-size:10px;font-weight:700;padding:4px 10px;border-radius:20px;cursor:pointer;font-family:'DM Sans',sans-serif;letter-spacing:.5px}

/* STATS STRIP */
.stats-strip{background:var(--navy);padding:10px 14px;display:grid;grid-template-columns:repeat(4,1fr);gap:6px;border-bottom:1px solid rgba(200,151,42,.2)}
.stat-card{background:rgba(255,255,255,.05);border-radius:var(--radius-sm);padding:7px 5px;text-align:center;border:1px solid rgba(200,151,42,.12)}
.stat-label{font-size:8px;text-transform:uppercase;letter-spacing:.8px;color:var(--gray-400);margin-bottom:2px}
.stat-val{font-family:'Playfair Display',serif;font-size:14px;color:var(--gold);font-weight:700}

/* NAV */
.nav-tabs{background:var(--navy-dark);display:flex;padding:8px 10px 0;gap:3px;border-bottom:1px solid rgba(200,151,42,.2);overflow-x:auto;scrollbar-width:none}
.nav-tabs::-webkit-scrollbar{display:none}
.tab-btn{background:none;border:none;color:var(--gray-400);font-family:'DM Sans',sans-serif;font-size:10px;font-weight:500;padding:7px 10px;border-radius:8px 8px 0 0;cursor:pointer;white-space:nowrap;transition:all .2s;position:relative;bottom:-1px}
.tab-btn.active{background:var(--cream);color:var(--navy-dark);font-weight:700}
.tab-btn:not(.active):hover{color:var(--gold)}

/* MAIN */
.main-content{background:var(--cream);min-height:calc(100vh - 175px);padding:14px}
.tab-content{display:none}
.tab-content.active{display:block}

/* SECTION HEADER */
.section-title{font-family:'Playfair Display',serif;font-size:18px;color:var(--navy-dark);margin-bottom:12px;display:flex;align-items:center;gap:8px}
.section-badge{background:var(--gold);color:var(--navy-dark);font-family:'DM Sans',sans-serif;font-size:10px;font-weight:700;padding:2px 8px;border-radius:20px}
.section-label{font-size:11px;font-weight:600;letter-spacing:2px;text-transform:uppercase;color:var(--gold);margin-bottom:8px}

/* CARDS */
.card{background:var(--white);border-radius:var(--radius);padding:14px;margin-bottom:12px;box-shadow:var(--shadow);border:1px solid var(--gray-200)}
.card h3{font-size:11px;font-weight:700;color:var(--navy);text-transform:uppercase;letter-spacing:.6px;margin-bottom:10px;padding-bottom:7px;border-bottom:2px solid var(--gold)}

/* FORM */
.field-row{margin-bottom:10px}
.field-row label{display:block;font-size:11px;font-weight:600;color:var(--gray-600);text-transform:uppercase;letter-spacing:.5px;margin-bottom:3px}
.field-row input,.field-row select,.field-row textarea{width:100%;background:var(--gray-100);border:1.5px solid var(--gray-200);border-radius:var(--radius-sm);padding:9px 11px;font-family:'DM Sans',sans-serif;font-size:14px;color:var(--navy-dark);-webkit-appearance:none;transition:border-color .2s}
.field-row input:focus,.field-row select:focus,.field-row textarea:focus{outline:none;border-color:var(--gold);background:var(--white)}
.field-row textarea{resize:vertical;min-height:56px}
.two-col{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.three-col{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px}

/* BUTTONS */
.btn-primary{background:linear-gradient(135deg,var(--gold),var(--gold-light));color:var(--navy-dark);border:none;border-radius:var(--radius-sm);padding:12px 18px;font-family:'DM Sans',sans-serif;font-size:14px;font-weight:700;width:100%;cursor:pointer;transition:all .2s;box-shadow:0 3px 12px rgba(200,151,42,.3)}
.btn-primary:active{transform:scale(.98)}
.btn-secondary{background:var(--navy);color:var(--white);border:none;border-radius:var(--radius-sm);padding:9px 14px;font-family:'DM Sans',sans-serif;font-size:13px;font-weight:600;cursor:pointer}
.btn-sm{background:var(--navy);color:var(--gold);border:none;border-radius:6px;padding:4px 9px;font-size:10px;font-weight:700;cursor:pointer;font-family:'DM Sans',sans-serif}
.btn-danger{background:none;border:1.5px solid var(--red);color:var(--red);border-radius:6px;padding:4px 9px;font-size:10px;font-weight:700;cursor:pointer;font-family:'DM Sans',sans-serif}
.btn-success{background:var(--green);color:var(--white);border:none;border-radius:6px;padding:4px 9px;font-size:10px;font-weight:700;cursor:pointer;font-family:'DM Sans',sans-serif}

/* TYPE CHIPS */
.chips{display:flex;flex-wrap:wrap;gap:6px}
.chip{background:var(--gray-100);border:2px solid var(--gray-200);border-radius:20px;padding:5px 11px;font-size:12px;font-weight:500;color:var(--gray-600);cursor:pointer;transition:all .2s}
.chip.selected{background:var(--navy);border-color:var(--navy);color:var(--gold);font-weight:600}
.chip.confirmed.selected{background:#1a7a4a;border-color:#1a7a4a;color:#fff}
.chip.pending.selected{background:#b07d10;border-color:#b07d10;color:#fff}
.chip.completed.selected{background:var(--navy);border-color:var(--navy);color:var(--gold)}
.chip.cancelled.selected{background:var(--red);border-color:var(--red);color:#fff}

/* NOTARY COUNT */
.notary-count-row{background:linear-gradient(135deg,var(--navy),var(--navy-light));border-radius:var(--radius-sm);padding:10px 12px;margin-bottom:10px}
.notary-count-row label{color:var(--gold)!important}
.notary-count-row input{background:rgba(255,255,255,.12)!important;border-color:rgba(200,151,42,.4)!important;color:var(--white)!important;font-size:18px!important;font-weight:700!important;text-align:center}
.notary-count-note{font-size:10px;color:rgba(255,255,255,.5);margin-top:4px}

/* APPT CARD */
.appt-card{background:var(--white);border-radius:var(--radius);padding:12px;margin-bottom:9px;box-shadow:var(--shadow);border:1px solid var(--gray-200);border-left:4px solid var(--navy)}
.appt-card.type-depo{border-left-color:var(--blue)}
.appt-card.type-ron{border-left-color:var(--purple)}
.appt-card.type-apostille{border-left-color:var(--gold)}
.appt-card.type-general{border-left-color:var(--green)}
.appt-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:5px}
.type-tag{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.6px;padding:3px 7px;border-radius:4px;background:var(--navy);color:var(--gold)}
.type-tag.depo{background:var(--blue);color:#fff}
.type-tag.ron{background:var(--purple);color:#fff}
.type-tag.apostille{background:var(--gold);color:var(--navy-dark)}
.type-tag.general{background:var(--green);color:#fff}
.status-badge{font-size:9px;font-weight:700;padding:3px 7px;border-radius:4px}
.badge-confirmed{background:#e8f8ef;color:#1a7a4a}
.badge-pending{background:#fef9e7;color:#b07d10}
.badge-completed{background:#eaf0fb;color:var(--navy)}
.badge-cancelled{background:#fdeaea;color:var(--red)}
.appt-client{font-size:14px;font-weight:700;color:var(--navy-dark);margin-bottom:2px}
.appt-meta{font-size:11px;color:var(--gray-600);margin-bottom:5px}
.pills{display:flex;gap:5px;flex-wrap:wrap}
.pill{background:var(--gray-100);border-radius:5px;padding:2px 7px;font-size:10px;font-weight:600;color:var(--navy-dark)}
.pill.earned{background:#e8f8ef;color:#1a7a4a}
.pill.exp{background:#fdeaea;color:var(--red)}
.pill.miles{background:#eaf0fb;color:var(--navy)}
.pill.notary{background:linear-gradient(135deg,var(--navy),var(--navy-light));color:var(--gold)}
.appt-actions{display:flex;gap:5px;margin-top:8px;flex-wrap:wrap}

/* FILTER */
.filter-bar{display:flex;gap:5px;margin-bottom:10px;overflow-x:auto;scrollbar-width:none;padding-bottom:2px}
.filter-bar::-webkit-scrollbar{display:none}
.filter-pill{background:var(--white);border:1.5px solid var(--gray-200);border-radius:20px;padding:4px 10px;font-size:10px;font-weight:600;color:var(--gray-600);cursor:pointer;white-space:nowrap;font-family:'DM Sans',sans-serif;transition:all .2s}
.filter-pill.active{background:var(--navy);color:var(--gold);border-color:var(--navy)}
.month-row{display:flex;gap:8px;margin-bottom:10px}
.month-row select{background:var(--white);border:1.5px solid var(--gray-200);border-radius:var(--radius-sm);padding:7px 10px;font-size:12px;color:var(--navy-dark);font-family:'DM Sans',sans-serif;flex:1;-webkit-appearance:none}

/* EMPTY */
.empty-state{text-align:center;padding:32px 16px;color:var(--gray-400)}
.empty-icon{font-size:32px;margin-bottom:8px}
.empty-state p{font-size:13px}

/* APOSTILLE TRACKER */
.apostille-card{background:var(--white);border-radius:var(--radius);padding:13px;margin-bottom:10px;box-shadow:var(--shadow);border:1px solid var(--gray-200);border-left:4px solid var(--gold)}
.apostille-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:6px}
.apostille-client{font-size:14px;font-weight:700;color:var(--navy-dark);margin-bottom:2px}
.apostille-doc{font-size:12px;color:var(--gray-600);margin-bottom:6px}
.progress-bar-wrap{background:var(--gray-100);border-radius:6px;height:8px;margin-bottom:6px;overflow:hidden}
.progress-bar{height:100%;border-radius:6px;background:linear-gradient(90deg,var(--gold),var(--gold-light));transition:width .4s ease}
.stage-dots{display:flex;gap:4px;margin-bottom:8px;flex-wrap:wrap}
.stage-dot{font-size:9px;font-weight:700;padding:3px 7px;border-radius:4px;background:var(--gray-100);color:var(--gray-400)}
.stage-dot.done{background:#e8f8ef;color:#1a7a4a}
.stage-dot.current{background:var(--navy);color:var(--gold)}
.apost-fee{font-size:13px;font-weight:700;color:#1a7a4a}

/* TAX ESTIMATOR */
.tax-box{background:linear-gradient(135deg,var(--navy-dark),var(--navy));border-radius:var(--radius);padding:16px;margin-bottom:12px;border:1px solid rgba(200,151,42,.25)}
.tax-title{font-size:11px;font-weight:700;letter-spacing:1.5px;text-transform:uppercase;color:var(--gold);margin-bottom:12px}
.tax-row{display:flex;justify-content:space-between;align-items:center;padding:7px 0;border-bottom:1px solid rgba(255,255,255,.07)}
.tax-row:last-child{border-bottom:none}
.tax-label{font-size:12px;color:rgba(255,255,255,.65)}
.tax-val{font-family:'Playfair Display',serif;font-size:15px;color:var(--white);font-weight:700}
.tax-val.gold{color:var(--gold)}
.tax-val.green{color:#4ecf7f}
.tax-val.red{color:#e07070}
.tax-note{font-size:10px;color:rgba(255,255,255,.3);margin-top:10px;line-height:1.6;font-style:italic}
.quarter-card{background:var(--white);border-radius:var(--radius-sm);padding:12px;margin-bottom:8px;border-left:4px solid var(--navy);border:1px solid var(--gray-200);border-left:4px solid var(--navy)}
.quarter-card.due-soon{border-left-color:var(--red)}
.quarter-card.paid{border-left-color:var(--green)}
.q-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:4px}
.q-name{font-size:13px;font-weight:700;color:var(--navy-dark)}
.q-due{font-size:11px;color:var(--gray-400)}
.q-amount{font-family:'Playfair Display',serif;font-size:18px;font-weight:700;color:var(--navy-dark)}
.q-status{font-size:10px;font-weight:700;padding:3px 8px;border-radius:4px;cursor:pointer}
.q-status.unpaid{background:#fdeaea;color:var(--red)}
.q-status.paid{background:#e8f8ef;color:#1a7a4a}

/* GOAL TRACKER */
.goal-card{background:var(--white);border-radius:var(--radius);padding:14px;margin-bottom:10px;box-shadow:var(--shadow);border:1px solid var(--gray-200)}
.goal-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px}
.goal-month{font-family:'Playfair Display',serif;font-size:16px;font-weight:700;color:var(--navy-dark)}
.goal-pct{font-family:'Playfair Display',serif;font-size:20px;font-weight:700}
.goal-bar-wrap{background:var(--gray-100);border-radius:6px;height:10px;margin-bottom:8px;overflow:hidden}
.goal-bar{height:100%;border-radius:6px;transition:width .5s ease}
.goal-details{display:grid;grid-template-columns:1fr 1fr 1fr;gap:6px;text-align:center}
.goal-detail-label{font-size:9px;text-transform:uppercase;letter-spacing:.5px;color:var(--gray-400);margin-bottom:2px}
.goal-detail-val{font-size:13px;font-weight:700;color:var(--navy-dark)}
.goal-actions{display:flex;gap:6px;margin-top:8px}
.set-goal-form{background:var(--gray-100);border-radius:var(--radius-sm);padding:10px;margin-top:8px;display:none}

/* PLATFORM TRACKER (built-in to signing tracker) */
.platform-card{background:var(--white);border-radius:var(--radius-sm);padding:11px 13px;margin-bottom:7px;box-shadow:var(--shadow);border:1px solid var(--gray-200);display:flex;justify-content:space-between;align-items:center}
.platform-name{font-size:13px;font-weight:700;color:var(--navy-dark)}
.platform-count{font-size:11px;color:var(--gray-400)}
.platform-owed{font-family:'Playfair Display',serif;font-size:17px;font-weight:700}
.platform-owed.owed{color:var(--red)}
.platform-owed.clear{color:#1a7a4a}

/* DASHBOARD */
.dash-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px}
.dash-card{background:var(--white);border-radius:var(--radius);padding:14px;box-shadow:var(--shadow);text-align:center;border:1px solid var(--gray-200)}
.dash-label{font-size:9px;text-transform:uppercase;letter-spacing:.6px;color:var(--gray-400);font-weight:600;margin-bottom:4px}
.dash-val{font-family:'Playfair Display',serif;font-size:22px;font-weight:700}

/* MILEAGE */
.mile-card{background:var(--white);border-radius:var(--radius);padding:11px 13px;margin-bottom:7px;box-shadow:var(--shadow);border:1px solid var(--gray-200);display:flex;justify-content:space-between;align-items:center}
.mile-desc{font-size:12px;font-weight:600;color:var(--navy-dark)}
.mile-date{font-size:10px;color:var(--gray-400)}
.mile-val{font-family:'Playfair Display',serif;font-size:15px;color:var(--navy);font-weight:700;text-align:right}
.mile-ded{font-size:10px;color:var(--green);font-weight:600;text-align:right}

/* IRS NOTE / NOTICE */
.irs-note{background:linear-gradient(135deg,var(--navy),var(--navy-light));border-radius:var(--radius-sm);padding:9px 11px;font-size:11px;color:rgba(255,255,255,.8);margin-top:8px;border-left:3px solid var(--gold)}
.irs-note strong{color:var(--gold)}
.notice{background:#e8f3fb;border-left:3px solid var(--blue);border-radius:var(--radius-sm);padding:8px 11px;font-size:11px;color:#1a4a6a;margin-bottom:8px}

/* MODAL */
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(10,20,50,.88);z-index:200;align-items:flex-end}
.modal-overlay.open{display:flex}
.modal-sheet{background:var(--cream);border-radius:20px 20px 0 0;width:100%;max-height:92vh;overflow-y:auto;padding:18px 14px 32px;animation:slideUp .3s ease}
@keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
.modal-handle{width:38px;height:4px;background:var(--gray-200);border-radius:2px;margin:0 auto 14px}
.modal-title{font-family:'Playfair Display',serif;font-size:19px;color:var(--navy-dark);margin-bottom:14px}

/* MILEAGE MODE */
.modeBtn{flex:1;padding:7px;border-radius:7px;font-size:11px;font-weight:700;cursor:pointer;font-family:'DM Sans',sans-serif;transition:all .2s}
.modeBtn.active{background:var(--navy);color:var(--gold);border:2px solid var(--navy)}
.modeBtn.inactive{background:var(--gray-100);color:var(--gray-600);border:2px solid var(--gray-200)}

/* TOAST */
.toast{position:fixed;bottom:26px;left:50%;transform:translateX(-50%) translateY(100px);background:var(--navy-dark);color:var(--gold);padding:10px 22px;border-radius:30px;font-size:12px;font-weight:600;border:1px solid var(--gold);z-index:999;transition:transform .3s ease;white-space:nowrap}
.toast.show{transform:translateX(-50%) translateY(0)}

.divider{height:1px;background:var(--gray-200);margin:12px 0}
</style>
</head>
<body>

<!-- ══════════════════════════════════════════
     LOGIN SCREEN
══════════════════════════════════════════ -->
<div id="loginScreen">
  <div class="login-logo">⚓</div>
  <h1>NBK Command Center</h1>
  <p>Notarized By Kristy LLC · Private</p>
  <input type="password" id="pwInput" placeholder="Enter password" onkeydown="if(event.key==='Enter')login()">
  <button id="loginBtn" onclick="login()">Access Command Center</button>
  <div id="loginError"></div>
  <div class="login-hint">Secured · Private · Your data stays on your device</div>
</div>

<!-- ══════════════════════════════════════════
     APP SHELL
══════════════════════════════════════════ -->
<div id="appShell">

<div class="app-header">
  <div class="header-row">
    <div class="brand">
      <span style="font-size:20px;margin-right:8px">⚓</span>
      <div>
        <div class="brand-name">NBK Command Center</div>
        <div class="brand-sub">Notarized By Kristy LLC</div>
      </div>
    </div>
    <div class="header-right">
      <div class="header-date" id="headerDate"></div>
      <button class="logout-btn" onclick="logout()">Lock</button>
    </div>
  </div>
</div>

<div class="stats-strip">
  <div class="stat-card"><div class="stat-label">Income</div><div class="stat-val" style="color:#4ecf7f" id="statIncome">$0</div></div>
  <div class="stat-card"><div class="stat-label">Expenses</div><div class="stat-val" style="color:#e07070" id="statExpenses">$0</div></div>
  <div class="stat-card"><div class="stat-label">Appts</div><div class="stat-val" id="statAppts">0</div></div>
  <div class="stat-card"><div class="stat-label">Notarizations</div><div class="stat-val" id="statNotarizations">0</div></div>
</div>

<div class="nav-tabs">
  <button class="tab-btn active" onclick="showTab('dashboard')">🏠 Home</button>
  <button class="tab-btn" onclick="showTab('appointments')">📋 Appts</button>
  <button class="tab-btn" onclick="showTab('apostille')">🌐 Apostille</button>
  <button class="tab-btn" onclick="showTab('expenses')">💳 Expenses</button>
  <button class="tab-btn" onclick="showTab('tax')">🧾 Tax</button>
  <button class="tab-btn" onclick="showTab('goals')">🎯 Goals</button>
  <button class="tab-btn" onclick="showTab('exports')">📥 Export</button>
</div>

<div class="main-content">

<!-- ── DASHBOARD ── -->
<div id="tab-dashboard" class="tab-content active">
  <div class="section-title">Dashboard</div>

  <div class="dash-grid">
    <div class="dash-card"><div class="dash-label">This Month</div><div class="dash-val" style="color:#1a7a4a" id="d-income">$0</div></div>
    <div class="dash-card"><div class="dash-label">Expenses</div><div class="dash-val" style="color:var(--red)" id="d-expenses">$0</div></div>
    <div class="dash-card"><div class="dash-label">Net Profit</div><div class="dash-val" style="color:var(--navy)" id="d-net">$0</div></div>
    <div class="dash-card"><div class="dash-label">Unpaid</div><div class="dash-val" style="color:var(--gold-dim)" id="d-unpaid">$0</div></div>
  </div>

  <div class="dash-grid">
    <div class="dash-card"><div class="dash-label">Appointments</div><div class="dash-val" style="color:var(--navy)" id="d-appts">0</div></div>
    <div class="dash-card"><div class="dash-label">Notarizations</div><div class="dash-val" style="color:var(--gold-dim)" id="d-notarizations">0</div></div>
    <div class="dash-card"><div class="dash-label">Miles</div><div class="dash-val" style="color:var(--navy)" id="d-miles">0</div></div>
    <div class="dash-card"><div class="dash-label">Mile Deduction</div><div class="dash-val" style="color:#1a7a4a" id="d-deduction">$0</div></div>
  </div>

  <!-- Goal progress on dashboard -->
  <div class="card" id="d-goal-section">
    <h3>Monthly Goal Progress</h3>
    <div id="d-goal-content"><p style="font-size:13px;color:var(--gray-400)">Set a goal in the 🎯 Goals tab to see progress here.</p></div>
  </div>

  <!-- Active apostille jobs -->
  <div class="card">
    <h3>Active Apostille Jobs</h3>
    <div id="d-apostille-active"><p style="font-size:13px;color:var(--gray-400)">No active apostille jobs.</p></div>
  </div>

  <!-- Platform balances -->
  <div class="card">
    <h3>Platform Balances Owed</h3>
    <div id="d-platforms"><p style="font-size:13px;color:var(--gray-400)">No unpaid platform fees tracked yet.</p></div>
  </div>

  <!-- Next tax payment -->
  <div class="card">
    <h3>Next Estimated Tax Payment</h3>
    <div id="d-next-tax"><p style="font-size:13px;color:var(--gray-400)">Check the 🧾 Tax tab to see payment schedule.</p></div>
  </div>
</div>

<!-- ── APPOINTMENTS ── -->
<div id="tab-appointments" class="tab-content">
  <div class="section-title">Appointments <span class="section-badge" id="apptCount">0</span></div>
  <div class="filter-bar">
    <button class="filter-pill active" onclick="filterAppts('all',this)">All</button>
    <button class="filter-pill" onclick="filterAppts('loan',this)">Loan</button>
    <button class="filter-pill" onclick="filterAppts('depo',this)">Depo</button>
    <button class="filter-pill" onclick="filterAppts('ron',this)">RON</button>
    <button class="filter-pill" onclick="filterAppts('apostille',this)">Apostille</button>
    <button class="filter-pill" onclick="filterAppts('general',this)">General</button>
  </div>
  <div class="month-row">
    <select id="apptMonthFilter" onchange="renderAppts()"><option value="all">All Months</option></select>
  </div>
  <div id="apptList"><div class="empty-state"><div class="empty-icon">📋</div><p>No appointments yet.<br>Tap ➕ to add one.</p></div></div>
  <button class="btn-primary" onclick="openAddAppt()" style="margin-top:8px">➕ Add Appointment</button>
</div>

<!-- ── APOSTILLE TRACKER ── -->
<div id="tab-apostille" class="tab-content">
  <div class="section-title">Apostille Jobs <span class="section-badge" id="apostilleCount">0</span></div>

  <div class="filter-bar">
    <button class="filter-pill active" onclick="filterApostille('all',this)">All</button>
    <button class="filter-pill" onclick="filterApostille('active',this)">Active</button>
    <button class="filter-pill" onclick="filterApostille('completed',this)">Completed</button>
  </div>

  <div id="apostilleList"><div class="empty-state"><div class="empty-icon">🌐</div><p>No apostille jobs yet.</p></div></div>
  <button class="btn-primary" onclick="openAddApostille()" style="margin-top:8px">➕ New Apostille Job</button>
</div>

<!-- ── EXPENSES & MILEAGE ── -->
<div id="tab-expenses" class="tab-content">
  <div class="section-title">Expenses & Mileage</div>

  <!-- Expense form -->
  <div class="card">
    <h3>Log Expense</h3>
    <div class="field-row">
      <label>Category</label>
      <select id="e-cat">
        <option>Printing & Supplies</option><option>Shipping / Postage</option><option>Notary Fees / SOS</option>
        <option>Software / Subscriptions</option><option>Marketing</option><option>Professional Development</option>
        <option>E&O Insurance</option><option>Vehicle / Auto</option><option>Phone / Internet</option><option>Other</option>
      </select>
    </div>
    <div class="field-row"><label>Description</label><input type="text" id="e-desc" placeholder="What was it for?"></div>
    <div class="two-col">
      <div class="field-row"><label>Amount ($)</label><input type="number" id="e-amount" placeholder="0.00" step="0.01"></div>
      <div class="field-row"><label>Date</label><input type="date" id="e-date"></div>
    </div>
    <button class="btn-primary" onclick="saveExpense()">Save Expense</button>
  </div>

  <!-- Mileage form -->
  <div class="card">
    <h3>Log Mileage</h3>
    <div class="field-row"><label>Description</label><input type="text" id="m-desc" placeholder="e.g. SOS trip, supply run"></div>
    <div style="display:flex;gap:6px;margin-bottom:10px">
      <button class="modeBtn active" id="mModeManualBtn" onclick="setMileMode('manual')">✏️ Enter Miles</button>
      <button class="modeBtn inactive" id="mModeAddrBtn" onclick="setMileMode('address')">📍 Addresses</button>
    </div>
    <div id="mManualMode">
      <div class="three-col">
        <div class="field-row"><label>Miles</label><input type="number" id="m-miles" placeholder="0" step="0.1"></div>
        <div class="field-row"><label>Round Trip</label><select id="m-rt"><option value="yes">Yes</option><option value="no">No</option></select></div>
        <div class="field-row"><label>Date</label><input type="date" id="m-date"></div>
      </div>
    </div>
    <div id="mAddressMode" style="display:none">
      <div class="field-row"><label>From</label><input type="text" id="m-addr-start" placeholder="Starting address"></div>
      <div class="field-row"><label>To</label><input type="text" id="m-addr-end" placeholder="Destination address"></div>
      <div class="two-col" style="margin-bottom:8px">
        <div class="field-row"><label>Round Trip</label><select id="m-addr-rt"><option value="yes">Yes</option><option value="no">No</option></select></div>
        <div class="field-row"><label>Date</label><input type="date" id="m-addr-date"></div>
      </div>
      <button onclick="calcMileage()" class="btn-secondary" style="width:100%;margin-bottom:8px">📍 Calculate Distance</button>
      <div id="mDistResult" style="display:none;background:var(--gray-100);border-radius:8px;padding:9px;font-size:12px;color:var(--navy-dark);border:1.5px solid var(--gold);margin-bottom:8px">
        <span id="mDistText"></span><input type="hidden" id="m-miles-calc">
      </div>
    </div>
    <div class="irs-note"><strong>2025 IRS Rate: $0.70/mile</strong></div>
    <button class="btn-primary" onclick="saveMileage()" style="margin-top:8px">Save Mileage</button>
  </div>

  <div class="notice">🎙️ Deposition mileage tracked separately for 1099 — not included here.</div>

  <div class="section-title">Expense Log <span class="section-badge" id="expCount">0</span></div>
  <div id="expenseList"><div class="empty-state"><div class="empty-icon">💳</div><p>No expenses yet.</p></div></div>

  <div class="section-title" style="margin-top:8px">Mileage Log <span class="section-badge" id="mileCount">0</span></div>
  <div id="mileageList"><div class="empty-state"><div class="empty-icon">🚗</div><p>No mileage yet.</p></div></div>

  <div class="divider"></div>
  <button class="btn-secondary" onclick="exportCSV()" style="width:100%;margin-bottom:8px">📥 Export CSV</button>
</div>

<!-- ── TAX ESTIMATOR ── -->
<div id="tab-tax" class="tab-content">
  <div class="section-title">Tax Estimator</div>

  <div class="tax-box" id="taxSummary">
    <div class="tax-title">📊 SE Tax Breakdown — <span id="taxYear">2026</span></div>
    <div class="tax-row"><span class="tax-label">Gross Self-Employment Income</span><span class="tax-val gold" id="t-gross">$0</span></div>
    <div class="tax-row"><span class="tax-label">Business Expenses</span><span class="tax-val red" id="t-expenses">$0</span></div>
    <div class="tax-row"><span class="tax-label">Mileage Deduction ($0.70/mi)</span><span class="tax-val red" id="t-mileage">$0</span></div>
    <div class="tax-row"><span class="tax-label">Net SE Income</span><span class="tax-val" id="t-net">$0</span></div>
    <div class="tax-row"><span class="tax-label">SE Tax (15.3% × 92.35%)</span><span class="tax-val red" id="t-se-tax">$0</span></div>
    <div class="tax-row"><span class="tax-label">SE Deduction (½ SE Tax)</span><span class="tax-val green" id="t-se-deduct">$0</span></div>
    <div class="tax-row"><span class="tax-label">Est. Federal Income Tax (22%)</span><span class="tax-val red" id="t-fed-tax">$0</span></div>
    <div class="tax-row"><span class="tax-label">Total Estimated Tax</span><span class="tax-val gold" id="t-total-tax">$0</span></div>
    <div class="tax-row"><span class="tax-label">Per Quarter (÷4)</span><span class="tax-val green" id="t-per-quarter">$0</span></div>
    <div class="tax-note">* Estimates only. Based on SE income from notary work (excluding W-2/1099 deposition income). Consult your tax professional for your actual tax liability. Deposition income from DepoDirect/Esquire reported separately on 1099.</div>
  </div>

  <!-- Depo income input -->
  <div class="card">
    <h3>Add Deposition Income (1099)</h3>
    <p style="font-size:12px;color:var(--gray-600);margin-bottom:10px">Enter your total deposition income separately — this is contractor income not subject to the same SE tax calculation as notary work.</p>
    <div class="two-col">
      <div class="field-row"><label>Depo Income YTD ($)</label><input type="number" id="depo-income-input" placeholder="0.00" step="0.01" onchange="renderTax()"></div>
      <div class="field-row"><label>Depo Expenses ($)</label><input type="number" id="depo-expense-input" placeholder="0.00" step="0.01" onchange="renderTax()"></div>
    </div>
    <div class="tax-box" style="margin-top:0">
      <div class="tax-title">🎙️ Deposition 1099 Summary</div>
      <div class="tax-row"><span class="tax-label">Depo Income</span><span class="tax-val gold" id="t-depo-gross">$0</span></div>
      <div class="tax-row"><span class="tax-label">Depo Expenses</span><span class="tax-val red" id="t-depo-exp">$0</span></div>
      <div class="tax-row"><span class="tax-label">Depo Net</span><span class="tax-val green" id="t-depo-net">$0</span></div>
    </div>
  </div>

  <div class="section-title">Quarterly Payment Schedule</div>
  <div id="quarterlySchedule"></div>
</div>

<!-- ── GOALS ── -->
<div id="tab-goals" class="tab-content">
  <div class="section-title">Revenue Goals</div>

  <!-- Set goal form -->
  <div class="card">
    <h3>Set Monthly Goal</h3>
    <div class="two-col">
      <div class="field-row">
        <label>Month</label>
        <select id="goal-month" onchange="updateGoalYear()">
          <option value="01">January</option><option value="02">February</option><option value="03">March</option>
          <option value="04">April</option><option value="05">May</option><option value="06">June</option>
          <option value="07">July</option><option value="08">August</option><option value="09">September</option>
          <option value="10">October</option><option value="11">November</option><option value="12">December</option>
        </select>
      </div>
      <div class="field-row"><label>Year</label><select id="goal-year"><option value="2026">2026</option><option value="2027">2027</option></select></div>
    </div>
    <div class="field-row"><label>Revenue Target ($)</label><input type="number" id="goal-amount" placeholder="e.g. 3000" step="100"></div>
    <div class="field-row"><label>Appointment Target</label><input type="number" id="goal-appts" placeholder="e.g. 20" step="1"></div>
    <button class="btn-primary" onclick="saveGoal()">Save Goal</button>
  </div>

  <div class="section-title" style="margin-top:4px">Progress</div>
  <div id="goalsList"><div class="empty-state"><div class="empty-icon">🎯</div><p>No goals set yet.</p></div></div>
</div>

<!-- ── EXPORTS ── -->
<div id="tab-exports" class="tab-content">
  <div class="section-title">Export Center</div>

  <!-- MONTHLY EXPORT -->
  <div class="card">
    <h3>📅 Monthly Export</h3>
    <p style="font-size:12px;color:var(--gray-600);margin-bottom:12px">Download all data for a specific month. Do this at the end of each month to build your year-end file.</p>
    <div class="two-col">
      <div class="field-row">
        <label>Month</label>
        <select id="exp-month">
          <option value="01">January</option><option value="02">February</option><option value="03">March</option>
          <option value="04">April</option><option value="05">May</option><option value="06">June</option>
          <option value="07">July</option><option value="08">August</option><option value="09">September</option>
          <option value="10">October</option><option value="11">November</option><option value="12">December</option>
        </select>
      </div>
      <div class="field-row">
        <label>Year</label>
        <select id="exp-year">
          <option value="2026">2026</option><option value="2027">2027</option>
        </select>
      </div>
    </div>
    <button class="btn-primary" onclick="exportMonthly()" style="margin-bottom:8px">📥 Download Monthly CSV</button>
    <div id="monthly-preview" style="background:var(--gray-100);border-radius:var(--radius-sm);padding:10px;font-size:12px;color:var(--gray-600);display:none"></div>
  </div>

  <!-- YEAR END PACKAGE -->
  <div class="card">
    <h3>📊 Year-End Tax Package</h3>
    <p style="font-size:12px;color:var(--gray-600);margin-bottom:12px">Downloads 4 files formatted for your accountant — appointments, expenses, mileage, and a tax summary. Export these in January each year.</p>
    <div class="field-row">
      <label>Tax Year</label>
      <select id="exp-tax-year">
        <option value="2026">2026</option><option value="2027">2027</option>
      </select>
    </div>

    <!-- Tax year preview -->
    <div class="tax-box" id="yearPreviewBox" style="margin-bottom:12px">
      <div class="tax-title">📋 Year Preview</div>
      <div class="tax-row"><span class="tax-label">Total Appointments</span><span class="tax-val gold" id="yp-appts">—</span></div>
      <div class="tax-row"><span class="tax-label">Gross Income (notary)</span><span class="tax-val green" id="yp-income">—</span></div>
      <div class="tax-row"><span class="tax-label">Deposition Income (1099)</span><span class="tax-val gold" id="yp-depo">—</span></div>
      <div class="tax-row"><span class="tax-label">Total Expenses</span><span class="tax-val red" id="yp-expenses">—</span></div>
      <div class="tax-row"><span class="tax-label">Total Miles</span><span class="tax-val" id="yp-miles">—</span></div>
      <div class="tax-row"><span class="tax-label">Mile Deduction</span><span class="tax-val green" id="yp-milededuct">—</span></div>
      <div class="tax-row"><span class="tax-label">Total Notarizations</span><span class="tax-val gold" id="yp-notarizations">—</span></div>
      <div class="tax-row"><span class="tax-label">Estimated SE Tax</span><span class="tax-val red" id="yp-setax">—</span></div>
    </div>

    <button class="btn-primary" onclick="exportYearPackage()" style="margin-bottom:6px">📦 Download Full Year Package</button>
    <p style="font-size:11px;color:var(--gray-400);text-align:center">Downloads 4 CSV files — appointments, expenses, mileage, tax summary</p>
  </div>

  <!-- INDIVIDUAL EXPORTS -->
  <div class="card">
    <h3>📋 Individual Exports</h3>
    <div style="display:flex;flex-direction:column;gap:8px">
      <button class="btn-secondary" onclick="exportAllAppointments()" style="width:100%;text-align:left;padding:11px 14px">📋 All Appointments (complete history)</button>
      <button class="btn-secondary" onclick="exportAllExpenses()" style="width:100%;text-align:left;padding:11px 14px">💳 All Expenses</button>
      <button class="btn-secondary" onclick="exportAllMileage()" style="width:100%;text-align:left;padding:11px 14px">🚗 All Mileage Log</button>
      <button class="btn-secondary" onclick="exportApostilleJobs()" style="width:100%;text-align:left;padding:11px 14px">🌐 All Apostille Jobs</button>
    </div>
  </div>

  <!-- INSTRUCTIONS -->
  <div class="card">
    <h3>📁 Recommended Workflow</h3>
    <div style="font-size:13px;color:var(--gray-700);line-height:1.8">
      <div style="padding:6px 0;border-bottom:1px solid var(--gray-100);display:flex;gap:8px"><span style="color:var(--gold);font-weight:700;flex-shrink:0">1.</span>At the end of each month, tap <strong>Monthly Export</strong> and save the CSV to a folder in Google Drive called "NBK 2026 Monthly Reports"</div>
      <div style="padding:6px 0;border-bottom:1px solid var(--gray-100);display:flex;gap:8px"><span style="color:var(--gold);font-weight:700;flex-shrink:0">2.</span>Name each file clearly — e.g. <strong>NBK_April_2026.csv</strong></div>
      <div style="padding:6px 0;border-bottom:1px solid var(--gray-100);display:flex;gap:8px"><span style="color:var(--gold);font-weight:700;flex-shrink:0">3.</span>In January, run <strong>Year-End Tax Package</strong> and send all 4 files to your accountant</div>
      <div style="padding:6px 0;display:flex;gap:8px"><span style="color:var(--gold);font-weight:700;flex-shrink:0">4.</span>Keep all monthly CSVs as backup — they stack your full year history even if you change devices</div>
    </div>
  </div>
</div>

</div><!-- end main-content -->
</div><!-- end appShell -->

<!-- ══════════════════════════════════════════
     MODALS
══════════════════════════════════════════ -->

<!-- ADD APPOINTMENT MODAL -->
<div class="modal-overlay" id="addApptModal">
<div class="modal-sheet">
  <div class="modal-handle"></div>
  <div class="modal-title">New Appointment</div>

  <div class="card" style="margin-bottom:10px">
    <h3>Type</h3>
    <div class="chips" id="typeChips">
      <div class="chip selected" data-val="loan" onclick="selectType(this)">Loan Signing</div>
      <div class="chip" data-val="depo" onclick="selectType(this)">Deposition</div>
      <div class="chip" data-val="ron" onclick="selectType(this)">RON</div>
      <div class="chip" data-val="apostille" onclick="selectType(this)">Apostille</div>
      <div class="chip" data-val="general" onclick="selectType(this)">General Notary</div>
    </div>
  </div>

  <div class="field-row"><label>Client / Borrower</label><input type="text" id="f-client" placeholder="Full name"></div>
  <div class="two-col">
    <div class="field-row"><label>Date</label><input type="date" id="f-date"></div>
    <div class="field-row"><label>Time</label><input type="time" id="f-time"></div>
  </div>
  <div class="field-row"><label>Location</label><input type="text" id="f-location" placeholder="City or address"></div>
  <div class="field-row"><label>Platform / Company</label><input type="text" id="f-company" placeholder="e.g. Snapdocs, DepoDirect"></div>

  <div class="card" style="margin:10px 0">
    <h3>Status</h3>
    <div class="chips" id="statusChips">
      <div class="chip confirmed selected" data-val="confirmed" onclick="selectStatus(this)">Confirmed</div>
      <div class="chip pending" data-val="pending" onclick="selectStatus(this)">Pending</div>
      <div class="chip completed" data-val="completed" onclick="selectStatus(this)">Completed</div>
      <div class="chip cancelled" data-val="cancelled" onclick="selectStatus(this)">Cancelled</div>
    </div>
  </div>

  <div class="notary-count-row field-row" id="notaryCountRow">
    <label>📋 Number of Notarizations</label>
    <input type="number" id="f-notary-count" placeholder="0" min="0" step="1">
    <div class="notary-count-note">Each = $10 TX state fee · tracks SE tax basis</div>
  </div>

  <div class="two-col">
    <div class="field-row"><label>Fee Charged ($)</label><input type="number" id="f-fee" placeholder="0.00" step="0.01"></div>
    <div class="field-row"><label>Payment</label>
      <select id="f-paid"><option value="unpaid">Unpaid</option><option value="invoiced">Invoiced</option><option value="paid">Paid ✓</option></select>
    </div>
  </div>
  <div class="field-row"><label>Expenses ($)</label><input type="number" id="f-expense" placeholder="0.00" step="0.01"></div>

  <div id="mileageApptSection">
    <div style="display:flex;gap:6px;margin-bottom:8px;margin-top:4px">
      <button class="modeBtn active" id="fModeManualBtn" onclick="setApptMileMode('manual')">✏️ Miles</button>
      <button class="modeBtn inactive" id="fModeAddrBtn" onclick="setApptMileMode('address')">📍 Addresses</button>
    </div>
    <div id="fManualMode">
      <div class="two-col">
        <div class="field-row"><label>Miles</label><input type="number" id="f-miles" placeholder="0" step="0.1"></div>
        <div class="field-row"><label>Round Trip</label><select id="f-roundtrip"><option value="yes">Yes ×2</option><option value="no">No</option></select></div>
      </div>
    </div>
    <div id="fAddressMode" style="display:none">
      <div class="field-row"><label>From</label><input type="text" id="f-addr-start" placeholder="Starting address"></div>
      <div class="field-row"><label>To</label><input type="text" id="f-addr-end" placeholder="Destination address"></div>
      <div class="two-col" style="margin-bottom:8px">
        <div class="field-row"><label>Round Trip</label><select id="f-addr-rt"><option value="yes">Yes ×2</option><option value="no">No</option></select></div>
        <div style="display:flex;align-items:flex-end"><button onclick="calcApptMileage()" class="btn-secondary" style="width:100%;padding:9px;font-size:11px">Calculate</button></div>
      </div>
      <div id="fDistResult" style="display:none;background:var(--gray-100);border-radius:8px;padding:9px;font-size:12px;color:var(--navy-dark);border:1.5px solid var(--gold);margin-bottom:8px">
        <span id="fDistText"></span><input type="hidden" id="f-miles-calc">
      </div>
    </div>
    <div class="irs-note"><strong>2025 IRS Rate: $0.70/mile</strong></div>
  </div>
  <div class="notice" id="depoMileNotice" style="display:none;margin-top:8px">🎙️ Deposition mileage tracked separately for 1099 records.</div>

  <div class="field-row" style="margin-top:8px"><label>Notes</label><textarea id="f-notes" placeholder="Document count, instructions, notes..."></textarea></div>

  <div style="display:flex;gap:8px;margin-top:10px">
    <button class="btn-primary" onclick="saveAppointment()" style="flex:1">⚓ Save</button>
    <button class="btn-secondary" onclick="closeModal('addApptModal')" style="flex:0 0 70px">Cancel</button>
  </div>
</div>
</div>

<!-- EDIT APPOINTMENT MODAL -->
<div class="modal-overlay" id="editApptModal">
<div class="modal-sheet">
  <div class="modal-handle"></div>
  <div class="modal-title">Edit Appointment</div>
  <input type="hidden" id="edit-id">
  <div class="field-row"><label>Client</label><input type="text" id="edit-client"></div>
  <div class="two-col">
    <div class="field-row"><label>Date</label><input type="date" id="edit-date"></div>
    <div class="field-row"><label>Time</label><input type="time" id="edit-time"></div>
  </div>
  <div class="field-row"><label>Location</label><input type="text" id="edit-location"></div>
  <div class="field-row"><label>Company</label><input type="text" id="edit-company"></div>
  <div class="field-row"><label>Status</label>
    <select id="edit-status"><option value="confirmed">Confirmed</option><option value="pending">Pending</option><option value="completed">Completed</option><option value="cancelled">Cancelled</option></select>
  </div>
  <div class="notary-count-row field-row" id="editNotaryRow">
    <label>📋 Notarizations</label>
    <input type="number" id="edit-notary-count" placeholder="0" min="0">
    <div class="notary-count-note">Tracks SE tax basis</div>
  </div>
  <div class="two-col">
    <div class="field-row"><label>Fee ($)</label><input type="number" id="edit-fee" step="0.01"></div>
    <div class="field-row"><label>Payment</label><select id="edit-paid"><option value="unpaid">Unpaid</option><option value="invoiced">Invoiced</option><option value="paid">Paid ✓</option></select></div>
  </div>
  <div class="two-col">
    <div class="field-row"><label>Expenses ($)</label><input type="number" id="edit-expense" step="0.01"></div>
    <div class="field-row" id="editMilesRow"><label>Miles</label><input type="number" id="edit-miles" step="0.1"></div>
  </div>
  <div class="field-row"><label>Notes</label><textarea id="edit-notes"></textarea></div>
  <div style="display:flex;gap:8px;margin-top:10px">
    <button class="btn-primary" onclick="saveEdit()" style="flex:1">Save Changes</button>
    <button class="btn-secondary" onclick="closeModal('editApptModal')" style="flex:0 0 70px">Cancel</button>
  </div>
</div>
</div>

<!-- ADD APOSTILLE MODAL -->
<div class="modal-overlay" id="addApostilleModal">
<div class="modal-sheet">
  <div class="modal-handle"></div>
  <div class="modal-title">New Apostille Job</div>
  <div class="field-row"><label>Client Name</label><input type="text" id="ap-client" placeholder="Full name"></div>
  <div class="field-row"><label>Document Type</label>
    <select id="ap-doctype">
      <option>Birth Certificate</option><option>Diploma / Transcript</option><option>Marriage Certificate</option>
      <option>Court Document</option><option>Corporate Document</option><option>Notarized Affidavit</option>
      <option>Medical Record</option><option>Power of Attorney</option><option>Other</option>
    </select>
  </div>
  <div class="field-row"><label>Destination Country</label><input type="text" id="ap-country" placeholder="e.g. China, Germany, Mexico"></div>
  <div class="two-col">
    <div class="field-row"><label>Date Received</label><input type="date" id="ap-received"></div>
    <div class="field-row"><label>Fee Charged ($)</label><input type="number" id="ap-fee" placeholder="0.00" step="0.01"></div>
  </div>
  <div class="two-col">
    <div class="field-row"><label>Deposit Paid ($)</label><input type="number" id="ap-deposit" placeholder="0.00" step="0.01"></div>
    <div class="field-row"><label>International?</label><select id="ap-intl"><option value="yes">Yes</option><option value="no">No</option></select></div>
  </div>
  <div class="field-row"><label>Client Email / Phone</label><input type="text" id="ap-contact" placeholder="Contact info"></div>
  <div class="field-row"><label>Notes</label><textarea id="ap-notes" placeholder="Special instructions, tracking numbers, etc."></textarea></div>
  <div style="display:flex;gap:8px;margin-top:10px">
    <button class="btn-primary" onclick="saveApostille()" style="flex:1">Save Job</button>
    <button class="btn-secondary" onclick="closeModal('addApostilleModal')" style="flex:0 0 70px">Cancel</button>
  </div>
</div>
</div>

<!-- EDIT APOSTILLE MODAL -->
<div class="modal-overlay" id="editApostilleModal">
<div class="modal-sheet">
  <div class="modal-handle"></div>
  <div class="modal-title">Update Apostille Job</div>
  <input type="hidden" id="ap-edit-id">
  <div class="field-row"><label>Client</label><input type="text" id="ap-edit-client"></div>
  <div class="field-row"><label>Document Type</label><input type="text" id="ap-edit-doctype"></div>
  <div class="field-row"><label>Current Stage</label>
    <select id="ap-edit-stage">
      <option value="0">1 — Intake / Document Review</option>
      <option value="1">2 — Notarization</option>
      <option value="2">3 — SOS Submission</option>
      <option value="3">4 — SOS Processing</option>
      <option value="4">5 — Return Shipping</option>
      <option value="5">6 — Delivered ✓</option>
    </select>
  </div>
  <div class="two-col">
    <div class="field-row"><label>Fee ($)</label><input type="number" id="ap-edit-fee" step="0.01"></div>
    <div class="field-row"><label>Balance Due ($)</label><input type="number" id="ap-edit-balance" step="0.01"></div>
  </div>
  <div class="field-row"><label>Tracking Number</label><input type="text" id="ap-edit-tracking" placeholder="SOS or shipping tracking #"></div>
  <div class="field-row"><label>Notes</label><textarea id="ap-edit-notes"></textarea></div>
  <div style="display:flex;gap:8px;margin-top:10px">
    <button class="btn-primary" onclick="saveApostilleEdit()" style="flex:1">Update Job</button>
    <button class="btn-secondary" onclick="closeModal('editApostilleModal')" style="flex:0 0 70px">Cancel</button>
  </div>
</div>
</div>

<div class="toast" id="toast"></div>

<!-- ══════════════════════════════════════════
     JAVASCRIPT
══════════════════════════════════════════ -->
<script>
// ── CONSTANTS
const IRS_RATE = 0.70;
const TX_FEE = 10;
const PASS_HASH = btoa('NBKkristy2026!'); // Change this to your desired password

// ── DATA
let appointments = JSON.parse(localStorage.getItem('nbk_appts') || '[]');
let expenses = JSON.parse(localStorage.getItem('nbk_expenses') || '[]');
let mileage = JSON.parse(localStorage.getItem('nbk_mileage') || '[]');
let apostilleJobs = JSON.parse(localStorage.getItem('nbk_apostille') || '[]');
let goals = JSON.parse(localStorage.getItem('nbk_goals') || '[]');
let quarterlyStatus = JSON.parse(localStorage.getItem('nbk_quarters') || '{}');

let selectedType = 'loan';
let selectedStatus = 'confirmed';
let apptTypeFilter = 'all';
let apostilleFilter = 'all';

// ── AUTH
function login() {
  const pw = document.getElementById('pwInput').value;
  if(btoa(pw) === PASS_HASH) {
    sessionStorage.setItem('nbk_auth','1');
    document.getElementById('loginScreen').style.display='none';
    document.getElementById('appShell').style.display='block';
    initApp();
  } else {
    document.getElementById('loginError').textContent='Incorrect password. Try again.';
    document.getElementById('pwInput').value='';
    setTimeout(()=>document.getElementById('loginError').textContent='',3000);
  }
}
function logout() {
  sessionStorage.removeItem('nbk_auth');
  document.getElementById('loginScreen').style.display='flex';
  document.getElementById('appShell').style.display='none';
  document.getElementById('pwInput').value='';
}
// Check if already authenticated
if(sessionStorage.getItem('nbk_auth')==='1') {
  document.getElementById('loginScreen').style.display='none';
  document.getElementById('appShell').style.display='block';
}

// ── SAVE
function save() {
  localStorage.setItem('nbk_appts',JSON.stringify(appointments));
  localStorage.setItem('nbk_expenses',JSON.stringify(expenses));
  localStorage.setItem('nbk_mileage',JSON.stringify(mileage));
  localStorage.setItem('nbk_apostille',JSON.stringify(apostilleJobs));
  localStorage.setItem('nbk_goals',JSON.stringify(goals));
  localStorage.setItem('nbk_quarters',JSON.stringify(quarterlyStatus));
}

// ── INIT
function initApp() {
  const now = new Date();
  document.getElementById('headerDate').innerHTML = now.toLocaleDateString('en-US',{weekday:'short',month:'short',day:'numeric'});
  document.getElementById('taxYear').textContent = now.getFullYear();
  // Set default dates
  ['e-date','m-date','m-addr-date','f-date','ap-received'].forEach(id=>{const el=document.getElementById(id);if(el)el.value=now.toISOString().split('T')[0];});
  // Set default goal month
  const goalMonth = document.getElementById('goal-month');
  if(goalMonth) goalMonth.value = String(now.getMonth()+1).padStart(2,'0');
  populateMonthFilters();
  renderAll();
  renderTax();
  renderGoals();
  renderQuarterly();
  renderApostille();
}
if(sessionStorage.getItem('nbk_auth')==='1') initApp();

// ── TABS
function showTab(tab) {
  document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
  document.getElementById('tab-'+tab).classList.add('active');
  document.querySelectorAll('.tab-btn').forEach((b,i)=>{
    b.classList.toggle('active',['dashboard','appointments','apostille','expenses','tax','goals','exports'][i]===tab);
  });
  if(tab==='dashboard') renderDashboard();
  if(tab==='tax') renderTax();
  if(tab==='goals') renderGoals();
  if(tab==='apostille') renderApostille();
  if(tab==='exports') { updateYearPreview(); updateMonthDefault(); }
}

function updateMonthDefault(){
  const now=new Date();
  const mo=document.getElementById('exp-month');
  const yr=document.getElementById('exp-year');
  if(mo&&!mo.dataset.set){ mo.value=String(now.getMonth()+1).padStart(2,'0'); mo.dataset.set='1'; }
  if(yr&&!yr.dataset.set){ yr.value=now.getFullYear(); yr.dataset.set='1'; }
  document.getElementById('exp-tax-year').value=now.getFullYear();
}

// ── HELPERS
function fmtDate(d){if(!d)return'';const[y,m,day]=d.split('-');return new Date(y,m-1,day).toLocaleDateString('en-US',{month:'short',day:'numeric',year:'numeric'});}
function fmt12(t){if(!t)return'';const[h,m]=t.split(':');const hr=parseInt(h);return(hr%12||12)+':'+m+' '+(hr<12?'AM':'PM');}
function typeLabel(t){return{loan:'Loan Signing',depo:'Deposition',ron:'RON',apostille:'Apostille',general:'General Notary'}[t]||t;}
function statusClass(s){return{confirmed:'badge-confirmed',pending:'badge-pending',completed:'badge-completed',cancelled:'badge-cancelled'}[s]||'';}
function showToast(msg){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2500);}
function closeModal(id){document.getElementById(id).classList.remove('open');}
function openModal(id){document.getElementById(id).classList.add('open');}

// ── APPOINTMENTS
function openAddAppt(){
  const now=new Date();
  document.getElementById('f-date').value=now.toISOString().split('T')[0];
  openModal('addApptModal');
}
function selectType(el){
  document.querySelectorAll('#typeChips .chip').forEach(c=>c.classList.remove('selected'));
  el.classList.add('selected'); selectedType=el.dataset.val;
  const isDepo=selectedType==='depo';
  document.getElementById('mileageApptSection').style.display=isDepo?'none':'block';
  document.getElementById('depoMileNotice').style.display=isDepo?'block':'none';
  document.getElementById('notaryCountRow').style.display=isDepo?'none':'block';
}
function selectStatus(el){
  document.querySelectorAll('#statusChips .chip').forEach(c=>c.classList.remove('selected'));
  el.classList.add('selected'); selectedStatus=el.dataset.val;
}
function saveAppointment(){
  const client=document.getElementById('f-client').value.trim();
  const date=document.getElementById('f-date').value;
  if(!client||!date){showToast('⚠️ Client and date required');return;}
  let miles=0;
  if(selectedType!=='depo'){
    const fMan=document.getElementById('fManualMode');
    if(fMan&&fMan.style.display!=='none'){
      miles=parseFloat(document.getElementById('f-miles').value)||0;
      if(document.getElementById('f-roundtrip').value==='yes') miles*=2;
    } else {
      miles=parseFloat(document.getElementById('f-miles-calc').value)||0;
    }
  }
  const appt={
    id:Date.now(),type:selectedType,client,date,
    time:document.getElementById('f-time').value,
    location:document.getElementById('f-location').value,
    company:document.getElementById('f-company').value,
    status:selectedStatus,
    notaryCount:selectedType==='depo'?0:(parseInt(document.getElementById('f-notary-count').value)||0),
    fee:parseFloat(document.getElementById('f-fee').value)||0,
    paid:document.getElementById('f-paid').value,
    expense:parseFloat(document.getElementById('f-expense').value)||0,
    miles,notes:document.getElementById('f-notes').value,
    created:new Date().toISOString()
  };
  appointments.unshift(appt);
  save(); populateMonthFilters(); renderAll(); renderDashboard();
  showToast('✓ Appointment saved!');
  closeModal('addApptModal');
  resetApptForm();
}
function resetApptForm(){
  ['f-client','f-time','f-location','f-company','f-fee','f-expense','f-notes','f-notary-count','f-miles'].forEach(id=>{const el=document.getElementById(id);if(el)el.value='';});
  document.getElementById('f-paid').value='unpaid';
  document.getElementById('f-date').value=new Date().toISOString().split('T')[0];
  document.querySelectorAll('#typeChips .chip').forEach(c=>c.classList.remove('selected'));
  document.querySelector('#typeChips .chip[data-val="loan"]').classList.add('selected');
  selectedType='loan';
  document.getElementById('mileageApptSection').style.display='block';
  document.getElementById('depoMileNotice').style.display='none';
  document.getElementById('notaryCountRow').style.display='block';
  document.querySelectorAll('#statusChips .chip').forEach(c=>c.classList.remove('selected'));
  document.querySelector('#statusChips .chip[data-val="confirmed"]').classList.add('selected');
  selectedStatus='confirmed';
  setApptMileMode('manual');
  const fdr=document.getElementById('fDistResult');if(fdr)fdr.style.display='none';
}
function filterAppts(f,el){apptTypeFilter=f;document.querySelectorAll('.filter-pill').forEach(p=>p.classList.remove('active'));el.classList.add('active');renderAppts();}
function renderAppts(){
  const mo=document.getElementById('apptMonthFilter')?.value||'all';
  const list=appointments.filter(a=>{
    const tok=apptTypeFilter==='all'||a.type===apptTypeFilter;
    const mok=mo==='all'||a.date?.startsWith(mo);
    return tok&&mok;
  }).sort((a,b)=>b.date>a.date?1:-1);
  document.getElementById('apptCount').textContent=list.length;
  const c=document.getElementById('apptList');
  if(!list.length){c.innerHTML='<div class="empty-state"><div class="empty-icon">📋</div><p>No appointments found.</p></div>';return;}
  c.innerHTML=list.map(a=>{
    const ded=(a.miles*IRS_RATE).toFixed(2);
    const nFee=(a.notaryCount||0)*TX_FEE;
    return`<div class="appt-card type-${a.type}">
      <div class="appt-header">
        <span class="type-tag ${a.type}">${typeLabel(a.type)}</span>
        <span class="status-badge ${statusClass(a.status)}">${a.status?.toUpperCase()}</span>
      </div>
      <div class="appt-client">${a.client}</div>
      <div class="appt-meta">${a.date?fmtDate(a.date):''} ${a.time?'@ '+fmt12(a.time):''}${a.location?' · '+a.location:''}${a.company?'<br>📌 '+a.company:''}</div>
      <div class="pills">
        ${a.fee?`<span class="pill earned">💰 $${a.fee.toFixed(2)}</span>`:''}
        <span class="pill" style="background:${a.paid==='paid'?'#eaf0fb':a.paid==='invoiced'?'#fef9e7':'#fdeaea'};color:${a.paid==='paid'?'var(--navy)':a.paid==='invoiced'?'#b07d10':'var(--red)'}">${a.paid==='paid'?'✓ Paid':a.paid==='invoiced'?'📧 Invoiced':'⏳ Unpaid'}</span>
        ${a.notaryCount?`<span class="pill notary">📋 ${a.notaryCount} · $${nFee}</span>`:''}
        ${a.expense?`<span class="pill exp">Exp $${a.expense.toFixed(2)}</span>`:''}
        ${a.miles?`<span class="pill miles">🚗 ${a.miles}mi · $${ded}</span>`:''}
      </div>
      ${a.notes?`<div style="font-size:11px;color:var(--gray-600);margin-top:5px;padding-top:5px;border-top:1px solid var(--gray-200)">📝 ${a.notes}</div>`:''}
      <div class="appt-actions">
        <button class="btn-sm" onclick="editAppt(${a.id})">✏️ Edit</button>
        <button class="btn-danger" onclick="deleteAppt(${a.id})">Delete</button>
      </div>
    </div>`;
  }).join('');
}
function editAppt(id){
  const a=appointments.find(x=>x.id===id);if(!a)return;
  document.getElementById('edit-id').value=id;
  document.getElementById('edit-client').value=a.client;
  document.getElementById('edit-date').value=a.date;
  document.getElementById('edit-time').value=a.time||'';
  document.getElementById('edit-location').value=a.location||'';
  document.getElementById('edit-company').value=a.company||'';
  document.getElementById('edit-status').value=a.status;
  document.getElementById('edit-notary-count').value=a.notaryCount||'';
  document.getElementById('edit-fee').value=a.fee||'';
  document.getElementById('edit-paid').value=a.paid||'unpaid';
  document.getElementById('edit-expense').value=a.expense||'';
  document.getElementById('edit-miles').value=a.miles||'';
  document.getElementById('edit-notes').value=a.notes||'';
  document.getElementById('editMilesRow').style.display=a.type==='depo'?'none':'block';
  document.getElementById('editNotaryRow').style.display=a.type==='depo'?'none':'block';
  openModal('editApptModal');
}
function saveEdit(){
  const id=parseInt(document.getElementById('edit-id').value);
  const idx=appointments.findIndex(x=>x.id===id);if(idx===-1)return;
  const isDepo=appointments[idx].type==='depo';
  appointments[idx]={...appointments[idx],
    client:document.getElementById('edit-client').value,
    date:document.getElementById('edit-date').value,
    time:document.getElementById('edit-time').value,
    location:document.getElementById('edit-location').value,
    company:document.getElementById('edit-company').value,
    status:document.getElementById('edit-status').value,
    notaryCount:isDepo?0:(parseInt(document.getElementById('edit-notary-count').value)||0),
    fee:parseFloat(document.getElementById('edit-fee').value)||0,
    paid:document.getElementById('edit-paid').value,
    expense:parseFloat(document.getElementById('edit-expense').value)||0,
    miles:isDepo?0:(parseFloat(document.getElementById('edit-miles').value)||0),
    notes:document.getElementById('edit-notes').value
  };
  save();renderAll();renderDashboard();closeModal('editApptModal');showToast('✓ Updated!');
}
function deleteAppt(id){
  if(!confirm('Delete this appointment?'))return;
  appointments=appointments.filter(a=>a.id!==id);
  save();renderAll();renderDashboard();showToast('Deleted.');
}

// ── APOSTILLE
const AP_STAGES=['Intake','Notarization','SOS Submit','SOS Processing','Shipping','Delivered ✓'];
function openAddApostille(){openModal('addApostilleModal');}
function saveApostille(){
  const client=document.getElementById('ap-client').value.trim();
  if(!client){showToast('⚠️ Client name required');return;}
  const fee=parseFloat(document.getElementById('ap-fee').value)||0;
  const deposit=parseFloat(document.getElementById('ap-deposit').value)||0;
  apostilleJobs.unshift({
    id:Date.now(),client,
    doctype:document.getElementById('ap-doctype').value,
    country:document.getElementById('ap-country').value,
    received:document.getElementById('ap-received').value,
    fee,deposit,balance:fee-deposit,
    intl:document.getElementById('ap-intl').value==='yes',
    contact:document.getElementById('ap-contact').value,
    notes:document.getElementById('ap-notes').value,
    stage:0,tracking:'',
    created:new Date().toISOString()
  });
  save();renderApostille();renderDashboard();showToast('✓ Apostille job saved!');
  closeModal('addApostilleModal');
  ['ap-client','ap-country','ap-fee','ap-deposit','ap-contact','ap-notes'].forEach(id=>document.getElementById(id).value='');
}
function filterApostille(f,el){apostilleFilter=f;document.querySelectorAll('.filter-pill').forEach(p=>p.classList.remove('active'));el.classList.add('active');renderApostille();}
function renderApostille(){
  let list=apostilleJobs;
  if(apostilleFilter==='active') list=list.filter(j=>j.stage<5);
  if(apostilleFilter==='completed') list=list.filter(j=>j.stage>=5);
  document.getElementById('apostilleCount').textContent=list.length;
  const c=document.getElementById('apostilleList');
  if(!list.length){c.innerHTML='<div class="empty-state"><div class="empty-icon">🌐</div><p>No apostille jobs.</p></div>';return;}
  c.innerHTML=list.map(j=>{
    const pct=Math.round((j.stage/5)*100);
    const stageDots=AP_STAGES.map((s,i)=>`<span class="stage-dot ${i<j.stage?'done':i===j.stage?'current':''}">${i<j.stage?'✓':i+1} ${s}</span>`).join('');
    return`<div class="apostille-card">
      <div class="apostille-header">
        <div>
          <div class="apostille-client">${j.client}</div>
          <div class="apostille-doc">${j.doctype}${j.country?' → '+j.country:''}</div>
        </div>
        <div style="text-align:right">
          <div class="apost-fee">$${j.fee.toFixed(2)}</div>
          ${j.balance>0?`<div style="font-size:10px;color:var(--red);font-weight:600">Due: $${j.balance.toFixed(2)}</div>`:'<div style="font-size:10px;color:var(--green);font-weight:600">Paid ✓</div>'}
        </div>
      </div>
      <div class="progress-bar-wrap"><div class="progress-bar" style="width:${pct}%"></div></div>
      <div style="font-size:10px;color:var(--gray-400);margin-bottom:6px">${pct}% complete · Stage ${j.stage+1} of 6</div>
      <div class="stage-dots">${stageDots}</div>
      ${j.tracking?`<div style="font-size:10px;color:var(--blue);margin-bottom:4px">📦 Tracking: ${j.tracking}</div>`:''}
      ${j.notes?`<div style="font-size:11px;color:var(--gray-600);margin-bottom:6px">📝 ${j.notes}</div>`:''}
      <div style="display:flex;gap:5px;flex-wrap:wrap">
        <button class="btn-sm" onclick="editApostille(${j.id})">✏️ Update</button>
        ${j.stage<5?`<button class="btn-success" onclick="advanceApostille(${j.id})">→ Next Stage</button>`:''}
        <button class="btn-danger" onclick="deleteApostille(${j.id})">Delete</button>
      </div>
    </div>`;
  }).join('');
}
function advanceApostille(id){
  const idx=apostilleJobs.findIndex(j=>j.id===id);
  if(idx===-1||apostilleJobs[idx].stage>=5)return;
  apostilleJobs[idx].stage++;
  save();renderApostille();renderDashboard();showToast(`✓ Moved to: ${AP_STAGES[apostilleJobs[idx].stage]}`);
}
function editApostille(id){
  const j=apostilleJobs.find(x=>x.id===id);if(!j)return;
  document.getElementById('ap-edit-id').value=id;
  document.getElementById('ap-edit-client').value=j.client;
  document.getElementById('ap-edit-doctype').value=j.doctype;
  document.getElementById('ap-edit-stage').value=j.stage;
  document.getElementById('ap-edit-fee').value=j.fee||'';
  document.getElementById('ap-edit-balance').value=j.balance||'';
  document.getElementById('ap-edit-tracking').value=j.tracking||'';
  document.getElementById('ap-edit-notes').value=j.notes||'';
  openModal('editApostilleModal');
}
function saveApostilleEdit(){
  const id=parseInt(document.getElementById('ap-edit-id').value);
  const idx=apostilleJobs.findIndex(j=>j.id===id);if(idx===-1)return;
  apostilleJobs[idx]={...apostilleJobs[idx],
    client:document.getElementById('ap-edit-client').value,
    doctype:document.getElementById('ap-edit-doctype').value,
    stage:parseInt(document.getElementById('ap-edit-stage').value),
    fee:parseFloat(document.getElementById('ap-edit-fee').value)||0,
    balance:parseFloat(document.getElementById('ap-edit-balance').value)||0,
    tracking:document.getElementById('ap-edit-tracking').value,
    notes:document.getElementById('ap-edit-notes').value
  };
  save();renderApostille();renderDashboard();closeModal('editApostilleModal');showToast('✓ Updated!');
}
function deleteApostille(id){
  if(!confirm('Delete this apostille job?'))return;
  apostilleJobs=apostilleJobs.filter(j=>j.id!==id);
  save();renderApostille();renderDashboard();showToast('Deleted.');
}

// ── EXPENSES
function saveExpense(){
  const desc=document.getElementById('e-desc').value.trim();
  const amount=parseFloat(document.getElementById('e-amount').value)||0;
  if(!desc||!amount){showToast('⚠️ Description and amount required');return;}
  expenses.unshift({id:Date.now(),cat:document.getElementById('e-cat').value,desc,amount,date:document.getElementById('e-date').value});
  save();renderExpenses();renderStatsStrip();renderTax();showToast('✓ Expense saved!');
  document.getElementById('e-desc').value='';document.getElementById('e-amount').value='';
}
function renderExpenses(){
  const c=document.getElementById('expenseList');
  document.getElementById('expCount').textContent=expenses.length;
  if(!expenses.length){c.innerHTML='<div class="empty-state"><div class="empty-icon">💳</div><p>No expenses yet.</p></div>';return;}
  c.innerHTML=expenses.map(e=>`
    <div style="background:var(--white);border-radius:var(--radius-sm);padding:10px 12px;margin-bottom:6px;box-shadow:var(--shadow);border:1px solid var(--gray-200);display:flex;justify-content:space-between;align-items:center">
      <div><div style="font-size:9px;text-transform:uppercase;color:var(--gray-400);font-weight:600">${e.cat}</div><div style="font-size:13px;font-weight:600;color:var(--navy-dark)">${e.desc}</div><div style="font-size:10px;color:var(--gray-400)">${fmtDate(e.date)}</div></div>
      <div style="display:flex;flex-direction:column;align-items:flex-end;gap:4px">
        <div style="font-family:'Playfair Display',serif;font-size:16px;color:var(--red);font-weight:700">$${e.amount.toFixed(2)}</div>
        <button class="btn-danger" onclick="deleteExpense(${e.id})">Del</button>
      </div>
    </div>`).join('');
}
function deleteExpense(id){if(!confirm('Delete?'))return;expenses=expenses.filter(e=>e.id!==id);save();renderExpenses();renderStatsStrip();renderTax();}

// ── MILEAGE
function setMileMode(mode){
  const manBtn=document.getElementById('mModeManualBtn');
  const adrBtn=document.getElementById('mModeAddrBtn');
  const manDiv=document.getElementById('mManualMode');
  const adrDiv=document.getElementById('mAddressMode');
  if(mode==='manual'){manDiv.style.display='block';adrDiv.style.display='none';manBtn.className='modeBtn active';adrBtn.className='modeBtn inactive';}
  else{manDiv.style.display='none';adrDiv.style.display='block';adrBtn.className='modeBtn active';manBtn.className='modeBtn inactive';}
}
function setApptMileMode(mode){
  const manBtn=document.getElementById('fModeManualBtn');
  const adrBtn=document.getElementById('fModeAddrBtn');
  const manDiv=document.getElementById('fManualMode');
  const adrDiv=document.getElementById('fAddressMode');
  if(mode==='manual'){manDiv.style.display='block';adrDiv.style.display='none';manBtn.className='modeBtn active';adrBtn.className='modeBtn inactive';}
  else{manDiv.style.display='none';adrDiv.style.display='block';adrBtn.className='modeBtn active';manBtn.className='modeBtn inactive';}
}
function saveMileage(){
  const desc=document.getElementById('m-desc').value.trim();
  const manDiv=document.getElementById('mManualMode');
  let miles=0,date='';
  if(manDiv&&manDiv.style.display!=='none'){
    miles=parseFloat(document.getElementById('m-miles').value)||0;
    if(document.getElementById('m-rt').value==='yes')miles*=2;
    date=document.getElementById('m-date').value;
  } else {
    miles=parseFloat(document.getElementById('m-miles-calc').value)||0;
    date=document.getElementById('m-addr-date').value;
  }
  if(!desc||!miles){showToast('⚠️ Description and miles required');return;}
  mileage.unshift({id:Date.now(),desc,miles,date});
  save();renderMileage();renderStatsStrip();renderTax();showToast('✓ Mileage saved!');
  document.getElementById('m-desc').value='';document.getElementById('m-miles').value='';
  const mdr=document.getElementById('mDistResult');if(mdr)mdr.style.display='none';
}
function renderMileage(){
  const apptMile=appointments.filter(a=>a.miles&&a.type!=='depo').map(a=>({id:'a'+a.id,desc:a.client+' ('+typeLabel(a.type)+')',miles:a.miles,date:a.date,fromAppt:true}));
  const all=[...mileage,...apptMile].sort((a,b)=>b.date>a.date?1:-1);
  document.getElementById('mileCount').textContent=all.length;
  const c=document.getElementById('mileageList');
  if(!all.length){c.innerHTML='<div class="empty-state"><div class="empty-icon">🚗</div><p>No mileage yet.</p></div>';return;}
  c.innerHTML=all.map(m=>`
    <div class="mile-card">
      <div><div class="mile-desc">${m.desc}${m.fromAppt?' <span style="font-size:9px;color:var(--gold);font-weight:700">APPT</span>':''}</div><div class="mile-date">${fmtDate(m.date)}</div></div>
      <div><div class="mile-val">${m.miles} mi</div><div class="mile-ded">$${(m.miles*IRS_RATE).toFixed(2)}</div>${!m.fromAppt?`<button class="btn-danger" onclick="deleteMileage(${m.id})" style="margin-top:3px;font-size:9px;padding:2px 6px">Del</button>`:''}</div>
    </div>`).join('');
}
function deleteMileage(id){if(!confirm('Delete?'))return;mileage=mileage.filter(m=>m.id!==id);save();renderMileage();renderStatsStrip();renderTax();}

async function calcMileage(){
  const s=document.getElementById('m-addr-start').value.trim();
  const e=document.getElementById('m-addr-end').value.trim();
  const rt=document.getElementById('m-addr-rt').value==='yes';
  if(!s||!e){showToast('⚠️ Enter both addresses');return;}
  const rd=document.getElementById('mDistResult');
  const rt2=document.getElementById('mDistText');
  const hm=document.getElementById('m-miles-calc');
  rt2.textContent='⏳ Calculating...';rd.style.display='block';
  try{
    const geo=async(a)=>{const r=await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(a)}&limit=1`,{headers:{'Accept-Language':'en'}});const d=await r.json();if(!d.length)throw new Error('Not found: '+a);return{lat:d[0].lat,lon:d[0].lon};};
    const [st,en]=await Promise.all([geo(s),geo(e)]);
    const rr=await fetch(`https://router.project-osrm.org/route/v1/driving/${st.lon},${st.lat};${en.lon},${en.lat}?overview=false`);
    const dd=await rr.json();if(dd.code!=='Ok')throw new Error('Route not found');
    const miOne=dd.routes[0].distance/1609.344;
    const total=rt?miOne*2:miOne;
    hm.value=total.toFixed(1);
    rt2.innerHTML=`One way: <strong>${miOne.toFixed(1)} mi</strong>${rt?' × 2 = <strong>'+total.toFixed(1)+' mi</strong>':''}<br><span style="color:#1a7a4a;font-weight:700">Deduction: $${(total*IRS_RATE).toFixed(2)}</span>`;
  }catch(err){rt2.innerHTML=`<span style="color:var(--red)">⚠️ ${err.message}</span>`;hm.value='';}
}
async function calcApptMileage(){
  const s=document.getElementById('f-addr-start').value.trim();
  const e=document.getElementById('f-addr-end').value.trim();
  const rt=document.getElementById('f-addr-rt').value==='yes';
  if(!s||!e){showToast('⚠️ Enter both addresses');return;}
  const rd=document.getElementById('fDistResult');
  const rt2=document.getElementById('fDistText');
  const hm=document.getElementById('f-miles-calc');
  rt2.textContent='⏳ Calculating...';rd.style.display='block';
  try{
    const geo=async(a)=>{const r=await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(a)}&limit=1`,{headers:{'Accept-Language':'en'}});const d=await r.json();if(!d.length)throw new Error('Not found: '+a);return{lat:d[0].lat,lon:d[0].lon};};
    const [st,en]=await Promise.all([geo(s),geo(e)]);
    const rr=await fetch(`https://router.project-osrm.org/route/v1/driving/${st.lon},${st.lat};${en.lon},${en.lat}?overview=false`);
    const dd=await rr.json();if(dd.code!=='Ok')throw new Error('Route not found');
    const miOne=dd.routes[0].distance/1609.344;
    const total=rt?miOne*2:miOne;
    hm.value=total.toFixed(1);
    rt2.innerHTML=`One way: <strong>${miOne.toFixed(1)} mi</strong>${rt?' × 2 = <strong>'+total.toFixed(1)+' mi</strong>':''}<br><span style="color:#1a7a4a;font-weight:700">Deduction: $${(total*IRS_RATE).toFixed(2)}</span><br><span style="font-size:10px;color:var(--gray-400)">Tap Save to record</span>`;
  }catch(err){rt2.innerHTML=`<span style="color:var(--red)">⚠️ ${err.message}</span>`;hm.value='';}
}

// ── TAX ESTIMATOR
function renderTax(){
  const year=new Date().getFullYear();
  const yearAppts=appointments.filter(a=>a.date?.startsWith(year));
  const nonDepo=yearAppts.filter(a=>a.type!=='depo');
  const gross=nonDepo.reduce((s,a)=>s+(a.fee||0),0);
  const apptExp=yearAppts.reduce((s,a)=>s+(a.expense||0),0);
  const standExp=expenses.filter(e=>e.date?.startsWith(year)).reduce((s,e)=>s+e.amount,0);
  const totalExp=apptExp+standExp;
  const apptMiles=nonDepo.reduce((s,a)=>s+(a.miles||0),0);
  const standMiles=mileage.filter(m=>m.date?.startsWith(year)).reduce((s,m)=>s+m.miles,0);
  const mileDeduction=(apptMiles+standMiles)*IRS_RATE;
  const netSE=Math.max(0,gross-totalExp-mileDeduction);
  const seTax=netSE*0.9235*0.153;
  const seDeduct=seTax/2;
  const fedTax=Math.max(0,netSE-seDeduct)*0.22;
  const totalTax=seTax+fedTax;
  const perQtr=totalTax/4;

  const depoIncome=parseFloat(document.getElementById('depo-income-input')?.value)||0;
  const depoExp=parseFloat(document.getElementById('depo-expense-input')?.value)||0;

  document.getElementById('t-gross').textContent='$'+gross.toFixed(2);
  document.getElementById('t-expenses').textContent='-$'+totalExp.toFixed(2);
  document.getElementById('t-mileage').textContent='-$'+mileDeduction.toFixed(2);
  document.getElementById('t-net').textContent='$'+netSE.toFixed(2);
  document.getElementById('t-se-tax').textContent='$'+seTax.toFixed(2);
  document.getElementById('t-se-deduct').textContent='-$'+seDeduct.toFixed(2);
  document.getElementById('t-fed-tax').textContent='$'+fedTax.toFixed(2);
  document.getElementById('t-total-tax').textContent='$'+totalTax.toFixed(2);
  document.getElementById('t-per-quarter').textContent='$'+perQtr.toFixed(2);
  document.getElementById('t-depo-gross').textContent='$'+depoIncome.toFixed(2);
  document.getElementById('t-depo-exp').textContent='-$'+depoExp.toFixed(2);
  document.getElementById('t-depo-net').textContent='$'+Math.max(0,depoIncome-depoExp).toFixed(2);
  renderQuarterly(perQtr);
}

function renderQuarterly(perQtr){
  const quarters=[
    {name:'Q1 2026',period:'Jan–Mar 2026',due:'April 15, 2026',key:'q1_2026'},
    {name:'Q2 2026',period:'Apr–Jun 2026',due:'June 16, 2026',key:'q2_2026'},
    {name:'Q3 2026',period:'Jul–Sep 2026',due:'September 15, 2026',key:'q3_2026'},
    {name:'Q4 2026',period:'Oct–Dec 2026',due:'January 15, 2027',key:'q4_2026'},
    {name:'Q1 2027',period:'Jan–Mar 2027',due:'April 15, 2027',key:'q1_2027'},
  ];
  const today=new Date();
  const c=document.getElementById('quarterlySchedule');
  if(!c)return;
  c.innerHTML=quarters.map(q=>{
    const isPaid=quarterlyStatus[q.key]==='paid';
    const dueDate=new Date(q.due);
    const daysUntil=Math.ceil((dueDate-today)/(1000*60*60*24));
    const isDueSoon=!isPaid&&daysUntil<=30&&daysUntil>=0;
    const isPast=!isPaid&&daysUntil<0;
    return`<div class="quarter-card ${isDueSoon||isPast?'due-soon':''} ${isPaid?'paid':''}">
      <div class="q-header">
        <div>
          <div class="q-name">${q.name} · ${q.period}</div>
          <div class="q-due">Due: ${q.due}${isDueSoon?' · ⚠️ Due soon':isPast?' · ⚠️ Past due':''}</div>
        </div>
        <span class="q-status ${isPaid?'paid':'unpaid'}" onclick="toggleQuarter('${q.key}')">${isPaid?'✓ Paid':'Unpaid'}</span>
      </div>
      <div class="q-amount">${perQtr?'$'+perQtr.toFixed(2):'Calculate above'}</div>
    </div>`;
  }).join('');
}
function toggleQuarter(key){
  quarterlyStatus[key]=quarterlyStatus[key]==='paid'?'unpaid':'paid';
  save();renderTax();showToast(quarterlyStatus[key]==='paid'?'✓ Marked as paid':'Marked unpaid');
}

// ── GOALS
function updateGoalYear(){/* already reactive */}
function saveGoal(){
  const mo=document.getElementById('goal-month').value;
  const yr=document.getElementById('goal-year').value;
  const amount=parseFloat(document.getElementById('goal-amount').value)||0;
  const appts=parseInt(document.getElementById('goal-appts').value)||0;
  if(!amount){showToast('⚠️ Enter a revenue target');return;}
  const key=yr+'-'+mo;
  const existing=goals.findIndex(g=>g.key===key);
  const goal={key,month:mo,year:yr,amount,appts,created:new Date().toISOString()};
  if(existing>=0) goals[existing]=goal; else goals.unshift(goal);
  save();renderGoals();renderDashboard();showToast('✓ Goal saved!');
  document.getElementById('goal-amount').value='';document.getElementById('goal-appts').value='';
}
function renderGoals(){
  const c=document.getElementById('goalsList');
  if(!goals.length){c.innerHTML='<div class="empty-state"><div class="empty-icon">🎯</div><p>No goals set yet.</p></div>';return;}
  const sorted=[...goals].sort((a,b)=>b.key>a.key?1:-1);
  c.innerHTML=sorted.map(g=>{
    const monthAppts=appointments.filter(a=>a.date?.startsWith(g.key));
    const actual=monthAppts.reduce((s,a)=>s+(a.fee||0),0);
    const pct=Math.min(100,Math.round((actual/g.amount)*100));
    const apptActual=monthAppts.length;
    const apptPct=g.appts?Math.min(100,Math.round((apptActual/g.appts)*100)):0;
    const color=pct>=100?'#1a7a4a':pct>=75?'#b07d10':pct>=50?'var(--navy)':'var(--red)';
    const monthName=new Date(g.year,parseInt(g.month)-1,1).toLocaleDateString('en-US',{month:'long',year:'numeric'});
    return`<div class="goal-card">
      <div class="goal-header">
        <div class="goal-month">${monthName}</div>
        <div class="goal-pct" style="color:${color}">${pct}%</div>
      </div>
      <div class="goal-bar-wrap"><div class="goal-bar" style="width:${pct}%;background:${color}"></div></div>
      <div class="goal-details">
        <div><div class="goal-detail-label">Target</div><div class="goal-detail-val">$${g.amount.toLocaleString()}</div></div>
        <div><div class="goal-detail-label">Actual</div><div class="goal-detail-val" style="color:${color}">$${actual.toFixed(0)}</div></div>
        <div><div class="goal-detail-label">Gap</div><div class="goal-detail-val" style="color:${actual>=g.amount?'#1a7a4a':'var(--red)'}">${actual>=g.amount?'+$'+(actual-g.amount).toFixed(0):'-$'+(g.amount-actual).toFixed(0)}</div></div>
      </div>
      ${g.appts?`<div style="margin-top:8px">
        <div style="font-size:10px;color:var(--gray-400);margin-bottom:3px">Appointments: ${apptActual} / ${g.appts} (${apptPct}%)</div>
        <div style="background:var(--gray-100);border-radius:4px;height:5px;overflow:hidden"><div style="width:${apptPct}%;height:100%;background:var(--navy);border-radius:4px"></div></div>
      </div>`:''}
      <div style="display:flex;gap:5px;margin-top:8px">
        <button class="btn-danger" onclick="deleteGoal('${g.key}')" style="font-size:10px;padding:3px 8px">Delete</button>
      </div>
    </div>`;
  }).join('');
}
function deleteGoal(key){if(!confirm('Delete this goal?'))return;goals=goals.filter(g=>g.key!==key);save();renderGoals();renderDashboard();}

// ── DASHBOARD
function renderDashboard(){
  const now=new Date();
  const mo=now.toISOString().slice(0,7);
  const monthAppts=appointments.filter(a=>a.date?.startsWith(mo));
  const income=monthAppts.reduce((s,a)=>s+(a.fee||0),0);
  const apptExp=monthAppts.reduce((s,a)=>s+(a.expense||0),0);
  const standExp=expenses.filter(e=>e.date?.startsWith(mo)).reduce((s,e)=>s+e.amount,0);
  const totalExp=apptExp+standExp;
  const net=income-totalExp;
  const unpaid=monthAppts.filter(a=>a.paid!=='paid').reduce((s,a)=>s+(a.fee||0),0);
  const notarizations=monthAppts.filter(a=>a.type!=='depo').reduce((s,a)=>s+(a.notaryCount||0),0);
  const apptMiles=monthAppts.filter(a=>a.type!=='depo').reduce((s,a)=>s+(a.miles||0),0);
  const standMiles=mileage.filter(m=>m.date?.startsWith(mo)).reduce((s,m)=>s+m.miles,0);
  const totalMiles=apptMiles+standMiles;

  document.getElementById('d-income').textContent='$'+income.toFixed(2);
  document.getElementById('d-expenses').textContent='$'+totalExp.toFixed(2);
  document.getElementById('d-net').textContent=(net>=0?'$':'-$')+Math.abs(net).toFixed(2);
  document.getElementById('d-net').style.color=net>=0?'var(--navy)':'var(--red)';
  document.getElementById('d-unpaid').textContent='$'+unpaid.toFixed(2);
  document.getElementById('d-appts').textContent=monthAppts.length;
  document.getElementById('d-notarizations').textContent=notarizations;
  document.getElementById('d-miles').textContent=totalMiles.toFixed(1);
  document.getElementById('d-deduction').textContent='$'+(totalMiles*IRS_RATE).toFixed(2);

  // Current month goal
  const moKey=now.getFullYear()+'-'+String(now.getMonth()+1).padStart(2,'0');
  const goal=goals.find(g=>g.key===moKey);
  const gc=document.getElementById('d-goal-content');
  if(goal){
    const pct=Math.min(100,Math.round((income/goal.amount)*100));
    const color=pct>=100?'#1a7a4a':pct>=75?'#b07d10':pct>=50?'var(--navy)':'var(--red)';
    gc.innerHTML=`<div style="display:flex;justify-content:space-between;margin-bottom:6px"><span style="font-size:13px;color:var(--gray-600)">Target: <strong style="color:var(--navy-dark)">$${goal.amount.toLocaleString()}</strong></span><span style="font-size:16px;font-weight:700;color:${color}">${pct}%</span></div><div style="background:var(--gray-100);border-radius:6px;height:10px;overflow:hidden"><div style="width:${pct}%;height:100%;background:${color};border-radius:6px;transition:width .5s"></div></div><div style="font-size:11px;color:var(--gray-400);margin-top:4px">${income>=goal.amount?'🎉 Goal reached!':'$'+(goal.amount-income).toFixed(2)+' to go'}</div>`;
  } else { gc.innerHTML='<p style="font-size:13px;color:var(--gray-400)">No goal set for this month. Set one in 🎯 Goals.</p>'; }

  // Active apostille
  const active=apostilleJobs.filter(j=>j.stage<5);
  document.getElementById('d-apostille-active').innerHTML=active.length?
    active.slice(0,3).map(j=>`<div style="display:flex;justify-content:space-between;padding:5px 0;border-bottom:1px solid var(--gray-100);font-size:12px"><span style="font-weight:600;color:var(--navy-dark)">${j.client} · ${j.doctype}</span><span style="color:var(--gold);font-weight:600">Stage ${j.stage+1}: ${AP_STAGES[j.stage]}</span></div>`).join(''):
    '<p style="font-size:13px;color:var(--gray-400)">No active apostille jobs.</p>';

  // Platform balances
  const platforms={};
  appointments.filter(a=>a.paid==='unpaid'&&a.fee&&a.company).forEach(a=>{
    if(!platforms[a.company]) platforms[a.company]={owed:0,count:0};
    platforms[a.company].owed+=a.fee; platforms[a.company].count++;
  });
  const pc=document.getElementById('d-platforms');
  const pkeys=Object.keys(platforms);
  pc.innerHTML=pkeys.length?pkeys.map(p=>`
    <div style="display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid var(--gray-100)">
      <div><div style="font-size:13px;font-weight:600;color:var(--navy-dark)">${p}</div><div style="font-size:10px;color:var(--gray-400)">${platforms[p].count} unpaid appointment${platforms[p].count>1?'s':''}</div></div>
      <div style="font-family:'Playfair Display',serif;font-size:16px;color:var(--red);font-weight:700">$${platforms[p].owed.toFixed(2)}</div>
    </div>`).join(''):
    '<p style="font-size:13px;color:var(--gray-400)">All appointments paid. ✓</p>';

  // Next tax due
  const today2=new Date();
  const qDates=[
    {name:'Q1 2026 — April 15, 2026',date:new Date('2026-04-15'),key:'q1_2026'},
    {name:'Q2 2026 — June 16, 2026',date:new Date('2026-06-16'),key:'q2_2026'},
    {name:'Q3 2026 — September 15, 2026',date:new Date('2026-09-15'),key:'q3_2026'},
    {name:'Q4 2026 — January 15, 2027',date:new Date('2027-01-15'),key:'q4_2026'},
  ];
  const next=qDates.find(q=>q.date>=today2&&quarterlyStatus[q.key]!=='paid');
  document.getElementById('d-next-tax').innerHTML=next?
    `<div style="font-size:14px;font-weight:700;color:var(--navy-dark)">${next.name}</div><div style="font-size:12px;color:var(--gray-400);margin-top:2px">Check 🧾 Tax tab for payment estimate</div>`:
    '<p style="font-size:13px;color:#1a7a4a">✓ All upcoming payments marked paid.</p>';
}

// ── STATS STRIP
function renderStatsStrip(){
  const mo=new Date().toISOString().slice(0,7);
  const ma=appointments.filter(a=>a.date?.startsWith(mo));
  const income=ma.reduce((s,a)=>s+(a.fee||0),0);
  const exp=expenses.filter(e=>e.date?.startsWith(mo)).reduce((s,e)=>s+e.amount,0)+ma.reduce((s,a)=>s+(a.expense||0),0);
  const notarizations=ma.filter(a=>a.type!=='depo').reduce((s,a)=>s+(a.notaryCount||0),0);
  document.getElementById('statIncome').textContent='$'+income.toFixed(0);
  document.getElementById('statExpenses').textContent='$'+exp.toFixed(0);
  document.getElementById('statAppts').textContent=ma.length;
  document.getElementById('statNotarizations').textContent=notarizations;
}

// ── MONTH FILTERS
function populateMonthFilters(){
  const months=new Set();
  appointments.forEach(a=>{if(a.date)months.add(a.date.slice(0,7))});
  expenses.forEach(e=>{if(e.date)months.add(e.date.slice(0,7))});
  const sorted=[...months].sort().reverse();
  const sel=document.getElementById('apptMonthFilter');
  if(!sel)return;
  const cur=sel.value;
  sel.innerHTML='<option value="all">All Months</option>';
  sorted.forEach(m=>{
    const d=new Date(m+'-01');
    sel.innerHTML+=`<option value="${m}" ${m===cur?'selected':''}>${d.toLocaleDateString('en-US',{month:'long',year:'numeric'})}</option>`;
  });
}

// ── RENDER ALL
function renderAll(){renderAppts();renderExpenses();renderMileage();renderStatsStrip();populateMonthFilters();}

// ── CSV EXPORT SYSTEM
function dlCSV(content, filename) {
  const blob = new Blob([content], {type:'text/csv;charset=utf-8;'});
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url; link.download = filename; link.click();
  setTimeout(()=>URL.revokeObjectURL(url), 1000);
}

function exportMonthly() {
  const mo = document.getElementById('exp-month').value;
  const yr = document.getElementById('exp-year').value;
  const key = yr + '-' + mo;
  const monthName = new Date(yr, parseInt(mo)-1, 1).toLocaleDateString('en-US',{month:'long',year:'numeric'});

  const filtAppts = appointments.filter(a => a.date?.startsWith(key));
  const filtExp = expenses.filter(e => e.date?.startsWith(key));
  const filtMile = mileage.filter(m => m.date?.startsWith(key));

  if(!filtAppts.length && !filtExp.length && !filtMile.length) {
    showToast('⚠️ No data found for ' + monthName); return;
  }

  // Build combined monthly CSV
  let csv = 'NBK BUSINESS TRACKER — ' + monthName.toUpperCase() + '\n';
  csv += 'Exported: ' + new Date().toLocaleDateString('en-US') + '\n\n';

  // APPOINTMENTS
  csv += 'APPOINTMENTS\n';
  csv += 'Type,Client,Date,Time,Location,Company,Status,Notarizations,Notary Fees ($),Total Fee ($),Paid,Expenses ($),Miles,Mile Deduction ($),Notes\n';
  filtAppts.forEach(a => {
    const notaryFees = (a.notaryCount||0) * TX_FEE;
    const mileDed = (a.miles||0) * IRS_RATE;
    csv += [
      a.type, a.client, a.date, a.time||'', a.location||'', a.company||'',
      a.status, a.notaryCount||0, notaryFees.toFixed(2), (a.fee||0).toFixed(2),
      a.paid, (a.expense||0).toFixed(2), a.miles||0, mileDed.toFixed(2),
      `"${(a.notes||'').replace(/"/g,'""')}"`
    ].join(',') + '\n';
  });

  // SUMMARY ROW
  const totalIncome = filtAppts.reduce((s,a)=>s+(a.fee||0),0);
  const totalNotarizations = filtAppts.filter(a=>a.type!=='depo').reduce((s,a)=>s+(a.notaryCount||0),0);
  const totalApptExp = filtAppts.reduce((s,a)=>s+(a.expense||0),0);
  const totalApptMiles = filtAppts.filter(a=>a.type!=='depo').reduce((s,a)=>s+(a.miles||0),0);
  csv += `\nMONTH TOTALS,,,,,,, ${totalNotarizations}, $${(totalNotarizations*TX_FEE).toFixed(2)}, $${totalIncome.toFixed(2)},,,${totalApptMiles.toFixed(1)},$${(totalApptMiles*IRS_RATE).toFixed(2)}\n`;

  // EXPENSES
  csv += '\nEXPENSES\n';
  csv += 'Category,Description,Amount ($),Date\n';
  filtExp.forEach(e => { csv += [e.cat, e.desc, e.amount.toFixed(2), e.date].join(',') + '\n'; });
  const totalStandExp = filtExp.reduce((s,e)=>s+e.amount,0);
  csv += `TOTAL,,$${totalStandExp.toFixed(2)}\n`;

  // MILEAGE
  csv += '\nSTANDALONE MILEAGE\n';
  csv += 'Description,Miles,Deduction ($),Date\n';
  filtMile.forEach(m => { csv += [m.desc, m.miles, (m.miles*IRS_RATE).toFixed(2), m.date].join(',') + '\n'; });
  const totalStandMiles = filtMile.reduce((s,m)=>s+m.miles,0);
  csv += `TOTAL,${totalStandMiles.toFixed(1)},$${(totalStandMiles*IRS_RATE).toFixed(2)}\n`;

  // MONTH SUMMARY
  const totalAllExp = totalApptExp + totalStandExp;
  const totalAllMiles = totalApptMiles + totalStandMiles;
  csv += '\nMONTH SUMMARY\n';
  csv += `Gross Income,$${totalIncome.toFixed(2)}\n`;
  csv += `Total Expenses,$${totalAllExp.toFixed(2)}\n`;
  csv += `Total Miles,${totalAllMiles.toFixed(1)}\n`;
  csv += `Mile Deduction,$${(totalAllMiles*IRS_RATE).toFixed(2)}\n`;
  csv += `Net (before tax),$${(totalIncome-totalAllExp-(totalAllMiles*IRS_RATE)).toFixed(2)}\n`;
  csv += `Total Notarizations,${totalNotarizations}\n`;
  csv += `Notary State Fees,$${(totalNotarizations*TX_FEE).toFixed(2)}\n`;

  // Show preview
  const preview = document.getElementById('monthly-preview');
  preview.style.display = 'block';
  preview.innerHTML = `✅ <strong>${monthName}:</strong> ${filtAppts.length} appointments · $${totalIncome.toFixed(2)} income · ${totalNotarizations} notarizations · ${totalAllMiles.toFixed(1)} miles`;

  dlCSV(csv, `NBK_${monthName.replace(' ','_')}.csv`);
  showToast('📥 ' + monthName + ' exported!');
}

function exportYearPackage() {
  const yr = document.getElementById('exp-tax-year').value;
  const yearAppts = appointments.filter(a => a.date?.startsWith(yr));
  const yearExp = expenses.filter(e => e.date?.startsWith(yr));
  const yearMile = mileage.filter(m => m.date?.startsWith(yr));
  const nonDepo = yearAppts.filter(a => a.type !== 'depo');
  const depoAppts = yearAppts.filter(a => a.type === 'depo');

  const gross = nonDepo.reduce((s,a)=>s+(a.fee||0),0);
  const depoIncome = depoAppts.reduce((s,a)=>s+(a.fee||0),0);
  const apptExp = yearAppts.reduce((s,a)=>s+(a.expense||0),0);
  const standExp = yearExp.reduce((s,e)=>s+e.amount,0);
  const totalExp = apptExp + standExp;
  const apptMiles = nonDepo.reduce((s,a)=>s+(a.miles||0),0);
  const standMiles = yearMile.reduce((s,m)=>s+m.miles,0);
  const totalMiles = apptMiles + standMiles;
  const mileDeduct = totalMiles * IRS_RATE;
  const totalNotarizations = nonDepo.reduce((s,a)=>s+(a.notaryCount||0),0);
  const netSE = Math.max(0, gross - totalExp - mileDeduct);
  const seTax = netSE * 0.9235 * 0.153;
  const fedTax = Math.max(0, netSE - seTax/2) * 0.22;

  // FILE 1 — Appointments
  let csv1 = `NBK APPOINTMENTS — TAX YEAR ${yr}\n`;
  csv1 += 'Type,Client,Date,Time,Location,Company,Status,Notarizations,Notary Fees,Total Fee,Paid,Appt Expenses,Miles,Mile Deduction,Notes\n';
  yearAppts.sort((a,b)=>a.date>b.date?1:-1).forEach(a=>{
    csv1 += [a.type,a.client,a.date,a.time||'',a.location||'',a.company||'',a.status,
      a.notaryCount||0,((a.notaryCount||0)*TX_FEE).toFixed(2),(a.fee||0).toFixed(2),
      a.paid,(a.expense||0).toFixed(2),a.miles||0,((a.miles||0)*IRS_RATE).toFixed(2),
      `"${(a.notes||'').replace(/"/g,'""')}"`].join(',')+'\n';
  });
  dlCSV(csv1, `NBK_${yr}_Appointments.csv`);

  // FILE 2 — Expenses
  let csv2 = `NBK EXPENSES — TAX YEAR ${yr}\n`;
  csv2 += 'Category,Description,Amount,Date\n';
  yearExp.sort((a,b)=>a.date>b.date?1:-1).forEach(e=>{csv2+=[e.cat,e.desc,e.amount.toFixed(2),e.date].join(',')+'\n';});
  csv2 += `\nTOTAL,,$${standExp.toFixed(2)}\n`;
  csv2 += `Appointment Expenses (included above),,$${apptExp.toFixed(2)}\n`;
  csv2 += `GRAND TOTAL EXPENSES,,$${totalExp.toFixed(2)}\n`;
  dlCSV(csv2, `NBK_${yr}_Expenses.csv`);

  // FILE 3 — Mileage
  let csv3 = `NBK MILEAGE LOG — TAX YEAR ${yr}\n`;
  csv3 += 'Description,Miles,Deduction,Date,Source\n';
  const allMile=[
    ...nonDepo.filter(a=>a.miles).map(a=>({desc:a.client+' ('+typeLabel(a.type)+')',miles:a.miles,date:a.date,src:'Appointment'})),
    ...yearMile.map(m=>({desc:m.desc,miles:m.miles,date:m.date,src:'Standalone'}))
  ].sort((a,b)=>a.date>b.date?1:-1);
  allMile.forEach(m=>{csv3+=[m.desc,m.miles,(m.miles*IRS_RATE).toFixed(2),m.date,m.src].join(',')+'\n';});
  csv3+=`\nTOTAL MILES,${totalMiles.toFixed(1)},$${mileDeduct.toFixed(2)}\n`;
  csv3+=`IRS Rate,${IRS_RATE}/mile\n`;
  dlCSV(csv3, `NBK_${yr}_Mileage.csv`);

  // FILE 4 — Tax Summary (for accountant)
  let csv4 = `NBK TAX SUMMARY — ${yr}\nNotarized By Kristy LLC\nPrepared: ${new Date().toLocaleDateString('en-US')}\n\n`;
  csv4 += 'SELF-EMPLOYMENT INCOME (NOTARY WORK)\n';
  csv4 += `Gross Notary Income,$${gross.toFixed(2)}\n`;
  csv4 += `Total Notarizations,${totalNotarizations}\n`;
  csv4 += `TX State Notary Fees ($10 each),$${(totalNotarizations*TX_FEE).toFixed(2)}\n\n`;
  csv4 += '1099 CONTRACTOR INCOME (DEPOSITION)\n';
  csv4 += `Deposition Income (DepoDirect/Esquire),$${depoIncome.toFixed(2)}\n`;
  csv4 += `Deposition Appointments,${depoAppts.length}\n\n`;
  csv4 += 'DEDUCTIONS\n';
  csv4 += `Total Business Expenses,$${totalExp.toFixed(2)}\n`;
  csv4 += `Total Business Miles,${totalMiles.toFixed(1)}\n`;
  csv4 += `IRS Mileage Rate,$${IRS_RATE}/mile\n`;
  csv4 += `Mileage Deduction,$${mileDeduct.toFixed(2)}\n\n`;
  csv4 += 'SE TAX ESTIMATE (NOTARY INCOME ONLY)\n';
  csv4 += `Net SE Income,$${netSE.toFixed(2)}\n`;
  csv4 += `SE Tax (15.3% × 92.35%),$${seTax.toFixed(2)}\n`;
  csv4 += `½ SE Tax Deduction,$${(seTax/2).toFixed(2)}\n`;
  csv4 += `Est. Federal Income Tax (22%),$${fedTax.toFixed(2)}\n`;
  csv4 += `Total Estimated Tax,$${(seTax+fedTax).toFixed(2)}\n\n`;
  csv4 += 'APPOINTMENT BREAKDOWN BY TYPE\n';
  csv4 += 'Type,Count,Total Revenue,Notarizations\n';
  ['loan','depo','ron','apostille','general'].forEach(t=>{
    const ta=yearAppts.filter(a=>a.type===t);
    if(!ta.length)return;
    csv4+=[typeLabel(t),ta.length,ta.reduce((s,a)=>s+(a.fee||0),0).toFixed(2),ta.reduce((s,a)=>s+(a.notaryCount||0),0)].join(',')+'\n';
  });
  csv4 += '\nQUARTERLY PAYMENTS\n';
  csv4 += 'Quarter,Due Date,Est. Amount,Status\n';
  const perQ=((seTax+fedTax)/4).toFixed(2);
  [['Q1','April 15, '+yr,'q1_'+yr],['Q2','June 16, '+yr,'q2_'+yr],['Q3','September 15, '+yr,'q3_'+yr],['Q4','January 15, '+(parseInt(yr)+1),'q4_'+yr]].forEach(q=>{
    csv4+=`${q[0]},$${perQ},${q[1]},${quarterlyStatus[q[2]]==='paid'?'Paid':'Unpaid'}\n`;
  });
  csv4 += '\n* This summary is an estimate. Consult your tax professional for accurate filing.\n';
  csv4 += '* Deposition income from DepoDirect/Esquire reported separately on 1099-NEC.\n';
  dlCSV(csv4, `NBK_${yr}_Tax_Summary_For_Accountant.csv`);

  showToast('📦 Year package downloaded — 4 files!');
}

function exportAllAppointments(){
  let csv='Type,Client,Date,Time,Location,Company,Status,Notarizations,Fee,Paid,Expenses,Miles,Notes\n';
  appointments.sort((a,b)=>a.date>b.date?1:-1).forEach(a=>{
    csv+=[a.type,a.client,a.date,a.time||'',a.location||'',a.company||'',a.status,a.notaryCount||0,(a.fee||0).toFixed(2),a.paid,(a.expense||0).toFixed(2),a.miles||0,`"${(a.notes||'').replace(/"/g,'""')}"`].join(',')+'\n';
  });
  dlCSV(csv,'NBK_All_Appointments_'+new Date().toISOString().slice(0,10)+'.csv');
  showToast('📥 All appointments exported!');
}
function exportAllExpenses(){
  let csv='Category,Description,Amount,Date\n';
  expenses.sort((a,b)=>a.date>b.date?1:-1).forEach(e=>{csv+=[e.cat,e.desc,e.amount.toFixed(2),e.date].join(',')+'\n';});
  dlCSV(csv,'NBK_All_Expenses_'+new Date().toISOString().slice(0,10)+'.csv');
  showToast('📥 All expenses exported!');
}
function exportAllMileage(){
  let csv='Description,Miles,Deduction,Date\n';
  const all=[...mileage,...appointments.filter(a=>a.miles&&a.type!=='depo').map(a=>({desc:a.client+' ('+typeLabel(a.type)+')',miles:a.miles,date:a.date}))].sort((a,b)=>a.date>b.date?1:-1);
  all.forEach(m=>{csv+=[m.desc,m.miles,(m.miles*IRS_RATE).toFixed(2),m.date].join(',')+'\n';});
  dlCSV(csv,'NBK_All_Mileage_'+new Date().toISOString().slice(0,10)+'.csv');
  showToast('📥 All mileage exported!');
}
function exportApostilleJobs(){
  let csv='Client,Document,Country,Received,Stage,Fee,Balance,International,Tracking,Notes\n';
  apostilleJobs.forEach(j=>{csv+=[j.client,j.doctype,j.country||'',j.received||'',AP_STAGES[j.stage]||'',(j.fee||0).toFixed(2),(j.balance||0).toFixed(2),j.intl?'Yes':'No',j.tracking||'',`"${(j.notes||'').replace(/"/g,'""')}"`].join(',')+'\n';});
  dlCSV(csv,'NBK_Apostille_Jobs_'+new Date().toISOString().slice(0,10)+'.csv');
  showToast('📥 Apostille jobs exported!');
}

function updateYearPreview(){
  const yr=document.getElementById('exp-tax-year')?.value||new Date().getFullYear().toString();
  const ya=appointments.filter(a=>a.date?.startsWith(yr));
  const ye=expenses.filter(e=>e.date?.startsWith(yr));
  const ym=mileage.filter(m=>m.date?.startsWith(yr));
  const nd=ya.filter(a=>a.type!=='depo');
  const gross=nd.reduce((s,a)=>s+(a.fee||0),0);
  const depo=ya.filter(a=>a.type==='depo').reduce((s,a)=>s+(a.fee||0),0);
  const totalExp=ya.reduce((s,a)=>s+(a.expense||0),0)+ye.reduce((s,e)=>s+e.amount,0);
  const apptMiles=nd.reduce((s,a)=>s+(a.miles||0),0);
  const standMiles=ym.reduce((s,m)=>s+m.miles,0);
  const totalMiles=apptMiles+standMiles;
  const totalN=nd.reduce((s,a)=>s+(a.notaryCount||0),0);
  const netSE=Math.max(0,gross-totalExp-totalMiles*IRS_RATE);
  const seTax=netSE*0.9235*0.153;
  document.getElementById('yp-appts').textContent=ya.length;
  document.getElementById('yp-income').textContent='$'+gross.toFixed(2);
  document.getElementById('yp-depo').textContent='$'+depo.toFixed(2);
  document.getElementById('yp-expenses').textContent='$'+totalExp.toFixed(2);
  document.getElementById('yp-miles').textContent=totalMiles.toFixed(1)+' mi';
  document.getElementById('yp-milededuct').textContent='$'+(totalMiles*IRS_RATE).toFixed(2);
  document.getElementById('yp-notarizations').textContent=totalN;
  document.getElementById('yp-setax').textContent='~$'+seTax.toFixed(2);
}

// ── CSV EXPORT (legacy — kept for compatibility)
function exportCSV(){ exportAllAppointments(); }

// ── MODAL CLOSE ON OVERLAY CLICK
document.querySelectorAll('.modal-overlay').forEach(m=>{
  m.addEventListener('click',function(e){if(e.target===this)this.classList.remove('open');});
});

// ── PASSWORD ENTER KEY
document.getElementById('pwInput')?.addEventListener('keydown',e=>{if(e.key==='Enter')login();});
</script>
</body>
</html>
