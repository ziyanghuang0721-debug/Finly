# Finly
A tool to help users reconnect with their finances
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>看不见的钱</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #FAF9F6;
    --primary: #0F4C3D;
    --text: #2A2A2A;
    --muted: #6B6B6B;
    --hover-bg: #E8F2EE;
    --card: #FFFFFF;
    --border: #ECEAE4;
    --radius: 20px;
    --shadow: 0 4px 12px rgba(0,0,0,0.08);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Inter', -apple-system, 'SF Pro Display', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    justify-content: center;
  }
  .app {
    width: 100%;
    max-width: 640px;
    min-height: 100vh;
    position: relative;
  }
  .screen {
    display: none;
    width: 100%;
    min-height: 100vh;
    padding: 48px 24px 64px;
    animation: fadeIn 0.45s ease;
  }
  .screen.active { display: block; }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* ── HOME ── */
  .home-wrap {
    display: flex;
    flex-direction: column;
    min-height: calc(100vh - 112px);
    justify-content: center;
  }
  .home-label {
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 0.12em;
    color: var(--primary);
    text-transform: uppercase;
    margin-bottom: 28px;
    opacity: 0.7;
  }
  .home-title {
    font-size: 48px;
    font-weight: 700;
    line-height: 1.15;
    color: var(--primary);
    margin-bottom: 24px;
    letter-spacing: -0.02em;
  }
  .home-sub {
    font-size: 18px;
    font-weight: 400;
    color: var(--muted);
    line-height: 1.7;
    margin-bottom: 48px;
  }
  .home-sub span {
    font-weight: 600;
    color: var(--text);
  }
  .coin-row {
    display: flex;
    gap: 10px;
    margin-bottom: 48px;
    align-items: center;
  }
  .coin {
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: var(--primary);
    opacity: 0;
    animation: coinFall 0.6s ease forwards;
  }
  .coin:nth-child(1) { animation-delay: 0.2s; opacity: 0.15; }
  .coin:nth-child(2) { animation-delay: 0.4s; opacity: 0.3; width: 28px; height: 28px; }
  .coin:nth-child(3) { animation-delay: 0.6s; opacity: 0.5; width: 22px; height: 22px; }
  .coin:nth-child(4) { animation-delay: 0.8s; opacity: 0.7; width: 16px; height: 16px; }
  .coin:nth-child(5) { animation-delay: 1.0s; opacity: 0.9; width: 10px; height: 10px; }
  @keyframes coinFall {
    from { opacity: 0; transform: translateY(-12px); }
    to { transform: translateY(0); }
  }
  .btn-main {
    display: block;
    width: 100%;
    height: 56px;
    background: var(--primary);
    color: #fff;
    font-size: 17px;
    font-weight: 600;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: background 0.2s, transform 0.15s;
    letter-spacing: 0.01em;
  }
  .btn-main:hover {
    background: #0a3a2e;
    transform: translateY(-1px);
  }
  .home-note {
    margin-top: 20px;
    font-size: 13px;
    color: var(--muted);
    text-align: center;
  }

  /* ── QUESTION ── */
  .q-header {
    margin-bottom: 32px;
  }
  .q-meta {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
  }
  .q-num {
    font-size: 13px;
    font-weight: 600;
    color: var(--primary);
    letter-spacing: 0.05em;
  }
  .q-pct {
    font-size: 13px;
    font-weight: 600;
    color: var(--muted);
  }
  .progress-bar {
    height: 6px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
  }
  .progress-fill {
    height: 100%;
    background: var(--primary);
    border-radius: 99px;
    transition: width 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  }
  .q-text {
    font-size: 26px;
    font-weight: 700;
    color: var(--text);
    line-height: 1.4;
    margin-bottom: 8px;
    letter-spacing: -0.01em;
  }
  .q-hint {
    font-size: 14px;
    color: var(--muted);
    line-height: 1.6;
  }

  /* Options — single column */
  .options-grid {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 32px;
  }
  .option-card {
    background: var(--card);
    border: 1.5px solid var(--border);
    border-radius: var(--radius);
    padding: 18px 20px;
    cursor: pointer;
    transition: border-color 0.2s, background 0.2s, transform 0.15s;
    display: flex;
    align-items: center;
    gap: 14px;
    box-shadow: var(--shadow);
  }
  .option-card:hover {
    background: var(--hover-bg);
    border-color: var(--primary);
    transform: translateY(-1px);
  }
  .option-card.selected {
    background: var(--hover-bg);
    border-color: var(--primary);
    border-width: 2px;
  }
  .option-icon {
    width: 20px;
    height: 20px;
    flex-shrink: 0;
    color: var(--primary);
    opacity: 0.75;
  }
  .option-card.selected .option-icon {
    opacity: 1;
  }
  .option-letter {
    width: 26px;
    height: 26px;
    border-radius: 8px;
    background: var(--bg);
    border: 1.5px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 12px;
    font-weight: 700;
    color: var(--muted);
    flex-shrink: 0;
    transition: background 0.2s, color 0.2s, border-color 0.2s;
  }
  .option-card.selected .option-letter {
    background: var(--primary);
    color: #fff;
    border-color: var(--primary);
  }
  .option-label {
    font-size: 16px;
    font-weight: 500;
    color: var(--text);
    flex: 1;
  }

  .q-nav {
    display: flex;
    gap: 12px;
  }
  .btn-prev {
    flex: 1;
    height: 52px;
    background: var(--border);
    color: var(--text);
    font-size: 15px;
    font-weight: 600;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: background 0.2s;
  }
  .btn-prev:hover { background: #dddad3; }
  .btn-next {
    flex: 2;
    height: 52px;
    background: var(--primary);
    color: #fff;
    font-size: 15px;
    font-weight: 600;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: background 0.2s, transform 0.15s;
  }
  .btn-next:hover { background: #0a3a2e; transform: translateY(-1px); }
  .btn-next:disabled { opacity: 0.4; cursor: default; transform: none; }

  /* ── RESULT ── */
  .result-section {
    margin-bottom: 36px;
    animation: fadeIn 0.5s ease both;
  }
  .result-section:nth-child(2) { animation-delay: 0.1s; }
  .result-section:nth-child(3) { animation-delay: 0.2s; }
  .result-section:nth-child(4) { animation-delay: 0.3s; }
  .result-section:nth-child(5) { animation-delay: 0.4s; }
  .result-section:nth-child(6) { animation-delay: 0.5s; }

  .result-eyebrow {
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 0.12em;
    color: var(--primary);
    text-transform: uppercase;
    margin-bottom: 12px;
    opacity: 0.8;
  }
  .result-big-num {
    font-size: 72px;
    font-weight: 800;
    color: var(--primary);
    letter-spacing: -0.03em;
    line-height: 1;
    margin-bottom: 8px;
  }
  .result-range {
    font-size: 15px;
    color: var(--muted);
    margin-bottom: 6px;
  }
  .result-title {
    font-size: 22px;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 6px;
    letter-spacing: -0.01em;
  }

  .concrete-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
  }
  .concrete-card {
    background: var(--card);
    border-radius: var(--radius);
    padding: 20px 18px;
    box-shadow: var(--shadow);
    border: 1px solid var(--border);
  }
  .concrete-num {
    font-size: 28px;
    font-weight: 800;
    color: var(--primary);
    margin-bottom: 4px;
    letter-spacing: -0.02em;
  }
  .concrete-label {
    font-size: 13px;
    color: var(--muted);
    font-weight: 500;
  }

  .aha-quote {
    font-size: 22px;
    font-weight: 700;
    color: var(--text);
    line-height: 1.65;
    letter-spacing: -0.01em;
    padding: 28px 24px;
    background: var(--card);
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    border-left: 4px solid var(--primary);
  }
  .aha-attr {
    font-size: 14px;
    color: var(--primary);
    font-weight: 600;
    margin-top: 14px;
  }

  .drift-bar-wrap { margin-bottom: 14px; }
  .drift-bar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 7px;
  }
  .drift-bar-label {
    font-size: 15px;
    font-weight: 600;
    color: var(--text);
  }
  .drift-bar-pct {
    font-size: 14px;
    font-weight: 700;
    color: var(--primary);
  }
  .drift-bar-track {
    height: 8px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
  }
  .drift-bar-fill {
    height: 100%;
    background: var(--primary);
    border-radius: 99px;
    width: 0;
    transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .persona-card {
    background: var(--card);
    border-radius: var(--radius);
    padding: 24px;
    box-shadow: var(--shadow);
    border: 1px solid var(--border);
    margin-bottom: 16px;
  }
  .persona-badge {
    display: inline-block;
    background: var(--hover-bg);
    color: var(--primary);
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding: 4px 12px;
    border-radius: 99px;
    margin-bottom: 14px;
  }
  .persona-name {
    font-size: 22px;
    font-weight: 800;
    color: var(--primary);
    margin-bottom: 8px;
    letter-spacing: -0.02em;
  }
  .persona-desc {
    font-size: 15px;
    color: var(--muted);
    line-height: 1.65;
    margin-bottom: 18px;
  }
  .persona-row {
    margin-bottom: 14px;
  }
  .persona-row-label {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 6px;
  }
  .persona-row-text {
    font-size: 14px;
    color: var(--text);
    line-height: 1.6;
  }
  .persona-says {
    font-size: 14px;
    font-style: italic;
    color: var(--text);
    padding: 12px 16px;
    background: var(--hover-bg);
    border-radius: 12px;
    line-height: 1.6;
  }

  .hope-card {
    background: var(--primary);
    border-radius: var(--radius);
    padding: 28px 24px;
    color: #fff;
  }
  .hope-title {
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    opacity: 0.7;
    margin-bottom: 16px;
  }
  .hope-text {
    font-size: 20px;
    font-weight: 600;
    line-height: 1.75;
    opacity: 0.95;
  }

  .goal-card {
    background: var(--card);
    border-radius: var(--radius);
    padding: 24px;
    box-shadow: var(--shadow);
    border: 1px solid var(--border);
    margin-top: 16px;
  }
  .goal-label {
    font-size: 14px;
    color: var(--muted);
    margin-bottom: 8px;
  }
  .goal-val {
    font-size: 36px;
    font-weight: 800;
    color: var(--primary);
    margin-bottom: 6px;
    letter-spacing: -0.02em;
  }
  .goal-sub {
    font-size: 14px;
    color: var(--muted);
    margin-bottom: 16px;
  }
  .goal-prog-track {
    height: 10px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
  }
  .goal-prog-fill {
    height: 100%;
    background: var(--primary);
    border-radius: 99px;
    width: 0;
    transition: width 1.2s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .share-btn {
    display: block;
    width: 100%;
    height: 52px;
    background: var(--card);
    color: var(--primary);
    font-size: 15px;
    font-weight: 700;
    border: 2px solid var(--primary);
    border-radius: var(--radius);
    cursor: pointer;
    transition: background 0.2s;
    margin-top: 16px;
  }
  .share-btn:hover { background: var(--hover-bg); }

  .divider {
    height: 1px;
    background: var(--border);
    margin: 32px 0;
  }

  /* Toast */
  .toast {
    position: fixed;
    bottom: 32px;
    left: 50%;
    transform: translateX(-50%) translateY(20px);
    background: var(--text);
    color: #fff;
    padding: 12px 24px;
    border-radius: 99px;
    font-size: 14px;
    font-weight: 600;
    opacity: 0;
    transition: opacity 0.3s, transform 0.3s;
    pointer-events: none;
    white-space: nowrap;
    z-index: 999;
  }
  .toast.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }

  @media (max-width: 480px) {
    .home-title { font-size: 38px; }
    .result-big-num { font-size: 56px; }
    .concrete-grid { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>
<div class="app">

  <!-- HOME -->
  <div class="screen active" id="screen-home">
    <div class="home-wrap">
      <div class="home-label">看不见的钱</div>
      <h1 class="home-title">你以为自己<br>一个月只花3000？</h1>
      <p class="home-sub">
        实际上很多人一年会漏掉<span>3万～5万元</span><br>
        这些钱不是大额消费<br>
        而是你根本没注意到的<span>「看不见的钱」</span>
      </p>
      <div class="coin-row">
        <div class="coin"></div>
        <div class="coin"></div>
        <div class="coin"></div>
        <div class="coin"></div>
        <div class="coin"></div>
      </div>
      <button class="btn-main" onclick="startQuiz()">测测我一年漏掉多少钱</button>
      <p class="home-note">9道题 · 约2分钟 · 完全匿名</p>
    </div>
  </div>

  <!-- QUESTION -->
  <div class="screen" id="screen-question">
    <div class="q-header">
      <div class="q-meta">
        <span class="q-num" id="q-num-label">第 1 题 / 共 9 题</span>
        <span class="q-pct" id="q-pct-label">0%</span>
      </div>
      <div class="progress-bar">
        <div class="progress-fill" id="progress-fill" style="width:0%"></div>
      </div>
    </div>
    <div id="q-text" class="q-text"></div>
    <div id="q-hint" class="q-hint" style="margin-bottom:28px"></div>
    <div class="options-grid" id="options-grid"></div>
    <div class="q-nav">
      <button class="btn-prev" id="btn-prev" onclick="prevQ()">上一题</button>
      <button class="btn-next" id="btn-next" onclick="nextQ()" disabled>下一题</button>
    </div>
  </div>

  <!-- RESULT -->
  <div class="screen" id="screen-result">
    <div id="result-content"></div>
  </div>

  <div class="toast" id="toast"></div>
</div>

<script>
// ── SVG ICON HELPERS ──
const icons = {
  clock: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>`,
  calendar: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>`,
  history: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 1 0 .49-4.95"/></svg>`,
  coin: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>`,
  money_sm: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="6" width="20" height="12" rx="2"/><circle cx="12" cy="12" r="3"/><path d="M6 12h.01M18 12h.01"/></svg>`,
  money_md: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg>`,
  money_lg: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 8l2 2-2 2 2 2-2 2 2 2"/><path d="M22 8l-2 2 2 2-2 2 2 2-2 2"/><line x1="8" y1="12" x2="16" y2="12"/></svg>`,
  wallet: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12V7a2 2 0 0 0-2-2H5a2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-5z"/><path d="M16 12h.01"/></svg>`,
  smile: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M8 13s1.5 2 4 2 4-2 4-2"/><line x1="9" y1="9" x2="9.01" y2="9"/><line x1="15" y1="9" x2="15.01" y2="9"/></svg>`,
  sub_many: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8"/><path d="M12 17v4"/><line x1="6" y1="8" x2="6" y2="13"/><line x1="10" y1="8" x2="10" y2="13"/><line x1="14" y1="8" x2="14" y2="13"/></svg>`,
  sub_some: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8"/><path d="M12 17v4"/><line x1="7" y1="8" x2="7" y2="13"/><line x1="13" y1="8" x2="13" y2="13"/></svg>`,
  sub_few: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8"/><path d="M12 17v4"/><line x1="12" y1="8" x2="12" y2="13"/></svg>`,
  sub_none: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8"/><path d="M12 17v4"/><line x1="4" y1="4" x2="20" y2="17"/></svg>`,
  star: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>`,
  book: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/></svg>`,
  music: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/></svg>`,
  plane: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17.8 19.2L16 11l3.5-3.5a2.121 2.121 0 1 0-3-3L13 8 4.8 6.2a.5.5 0 0 0-.5.8l5.1 5.1-2.5 2.5-2.5-.5-.5 2.5 2.5.5.5 2.5 2.5-.5-.5-2.5 2.5-2.5 5.1 5.1a.5.5 0 0 0 .8-.5z"/></svg>`,
  piggy: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 5c-1.5 0-2.8 1.4-3 2-3.5-1.5-11-.3-11 5 0 1.8 0 3 2 4.5V20h4v-2h3v2h4v-3.5c1.5-1.5 2-3.5 2-5.5C20 8 19 5 19 5z"/><path d="M2 9.5C2 9.5 2 11 3 11"/><line x1="16" y1="8" x2="16" y2="8"/></svg>`,
  heart: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg>`,
  home_icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>`,
  hobby: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M8 14s1.5 2 4 2 4-2 4-2"/><line x1="9" y1="9" x2="9.01" y2="9"/><line x1="15" y1="9" x2="15.01" y2="9"/></svg>`,
  gift: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 12 20 22 4 22 4 12"/><rect x="2" y="7" width="20" height="5"/><line x1="12" y1="22" x2="12" y2="7"/><path d="M12 7H7.5a2.5 2.5 0 0 1 0-5C11 2 12 7 12 7z"/><path d="M12 7h4.5a2.5 2.5 0 0 0 0-5C13 2 12 7 12 7z"/></svg>`,
  delivery: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="1" y="3" width="15" height="13"/><polygon points="16 8 20 8 23 11 23 16 16 16 16 8"/><circle cx="5.5" cy="18.5" r="2.5"/><circle cx="18.5" cy="18.5" r="2.5"/></svg>`,
  box: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"/><polyline points="3.27 6.96 12 12.01 20.73 6.96"/><line x1="12" y1="22.08" x2="12" y2="12"/></svg>`,
};

function icon(key) {
  return `<span class="option-icon">${icons[key] || icons.coin}</span>`;
}

// ── QUESTION DATA ──
const questions = [
  {
    id: 'impulse',
    text: '最近一次你说：「先买了再说」是什么时候？',
    hint: '',
    type: 'single',
    options: [
      { label: '昨天', icon: 'clock', value: 'yesterday' },
      { label: '上周', icon: 'calendar', value: 'lastweek' },
      { label: '这个月', icon: 'history', value: 'thismonth' },
      { label: '想不起来', icon: 'history', value: 'dontremember' },
    ]
  },
  {
    id: 'small_spend',
    text: '最近一次你顺手买的小东西大概多少钱？',
    hint: '例如：奶茶、零食、小摆件',
    type: 'amount',
    options: [
      { label: '¥10 - ¥50', icon: 'coin', value: 30, annualMult: 52 },
      { label: '¥50 - ¥100', icon: 'money_sm', value: 75, annualMult: 52 },
      { label: '¥100 - ¥300', icon: 'money_md', value: 200, annualMult: 26 },
      { label: '¥300 以上', icon: 'wallet', value: 400, annualMult: 12 },
    ]
  },
  {
    id: 'mood_spend',
    text: '最近一次你因为心情奖励自己买东西花了多少？',
    hint: '',
    type: 'amount',
    options: [
      { label: '¥10 - ¥50', icon: 'smile', value: 30, annualMult: 52 },
      { label: '¥50 - ¥100', icon: 'money_sm', value: 75, annualMult: 52 },
      { label: '¥100 - ¥300', icon: 'money_md', value: 200, annualMult: 26 },
      { label: '¥300 以上', icon: 'wallet', value: 400, annualMult: 12 },
    ]
  },
  {
    id: 'subscription',
    text: '你有多少自动扣费项目？',
    hint: '例如：视频会员、音乐会员、软件会员',
    type: 'sub',
    options: [
      { label: '很多（5个以上）', icon: 'sub_many', value: 500, subAnnual: 500 },
      { label: '有一些（2-4个）', icon: 'sub_some', value: 250, subAnnual: 250 },
      { label: '很少（1个）', icon: 'sub_few', value: 80, subAnnual: 80 },
      { label: '没有', icon: 'sub_none', value: 0, subAnnual: 0 },
    ]
  },
  {
    id: 'influenced',
    text: '最近一次被广告或小红书种草购买花了多少？',
    hint: '',
    type: 'amount',
    options: [
      { label: '¥10 - ¥50', icon: 'star', value: 30, annualMult: 26 },
      { label: '¥50 - ¥100', icon: 'money_sm', value: 75, annualMult: 26 },
      { label: '¥100 - ¥300', icon: 'money_md', value: 200, annualMult: 12 },
      { label: '¥300 以上', icon: 'wallet', value: 400, annualMult: 6 },
    ]
  },
  {
    id: 'growth',
    text: '最近一次为了成为更好的自己而消费是多少？',
    hint: '例如：课程、健身卡、工具',
    type: 'amount',
    options: [
      { label: '¥10 - ¥50', icon: 'book', value: 30, annualMult: 26 },
      { label: '¥50 - ¥100', icon: 'money_sm', value: 75, annualMult: 26 },
      { label: '¥100 - ¥300', icon: 'money_md', value: 200, annualMult: 12 },
      { label: '¥300 以上', icon: 'wallet', value: 400, annualMult: 6 },
    ]
  },
  {
    id: 'convenience',
    text: '最近一次为了方便自己而消费是多少？',
    hint: '例如：外卖、跑腿、加速服务',
    type: 'amount',
    options: [
      { label: '¥10 - ¥50', icon: 'delivery', value: 30, annualMult: 52 },
      { label: '¥50 - ¥100', icon: 'money_sm', value: 75, annualMult: 52 },
      { label: '¥100 - ¥300', icon: 'money_md', value: 200, annualMult: 26 },
      { label: '¥300 以上', icon: 'wallet', value: 400, annualMult: 12 },
    ]
  },
  {
    id: 'unused',
    text: '最近一次买回来使用不足3次的东西花了多少？',
    hint: '例如：榨汁机、健身环、收纳用品',
    type: 'amount',
    options: [
      { label: '¥10 - ¥50', icon: 'box', value: 30, annualMult: 12 },
      { label: '¥50 - ¥100', icon: 'money_sm', value: 75, annualMult: 12 },
      { label: '¥100 - ¥300', icon: 'money_md', value: 200, annualMult: 6 },
      { label: '¥300 以上', icon: 'wallet', value: 400, annualMult: 4 },
    ]
  },
  {
    id: 'goal',
    text: '留住这些钱，你最希望用在哪里？',
    hint: '',
    type: 'goal',
    options: [
      { label: '旅行', icon: 'plane', value: 'travel' },
      { label: '学习成长', icon: 'book', value: 'study' },
      { label: '兴趣爱好', icon: 'hobby', value: 'hobby' },
      { label: '搬家 / 租房', icon: 'home_icon', value: 'rent' },
      { label: '储蓄', icon: 'piggy', value: 'save' },
      { label: '给未来的自己', icon: 'heart', value: 'future' },
    ]
  }
];

// Goal follow-up
const goalFollowUp = {
  travel: {
    text: '最想去哪里旅行？',
    hint: '',
    options: [
      { label: '日本', icon: 'plane', value: 'japan', cost: 8000 },
      { label: '韩国', icon: 'plane', value: 'korea', cost: 6000 },
      { label: '东南亚', icon: 'plane', value: 'sea', cost: 5000 },
      { label: '欧洲', icon: 'plane', value: 'europe', cost: 20000 },
      { label: '北美', icon: 'plane', value: 'namerica', cost: 25000 },
      { label: '其他', icon: 'plane', value: 'other', cost: 8000 },
    ]
  },
  save: {
    text: '今年最想存多少钱？',
    hint: '',
    options: [
      { label: '¥10,000', icon: 'piggy', value: 10000 },
      { label: '¥30,000', icon: 'piggy', value: 30000 },
      { label: '¥50,000', icon: 'piggy', value: 50000 },
      { label: '¥100,000', icon: 'piggy', value: 100000 },
    ]
  }
};

// ── PERSONA DATA ──
const personas = [
  {
    id: 'future',
    name: '被未来骗走钱的人',
    desc: '你对未来的自己有很高期待——所以你愿意为那个人提前付钱。只是那个人，不一定如期出现。',
    pros: '充满上进心，愿意为自我提升投资，对未来抱有希望。',
    risks: '为想象中的自己买单，实际使用率偏低，容易囤积课程和工具。',
    says: ['你买的不是课程，你买的是那个以为明天会开始努力的自己。', '你总相信未来的自己会做到，但付款的是现在的你。', '你收藏的不是计划，是对未来自己的期待。'],
  },
  {
    id: 'joy',
    name: '被快乐骗走钱的人',
    desc: '你花的不是钱，是情绪出口。每一次消费背后，都有一个需要被安慰的自己。',
    pros: '懂得照顾自己情绪，生活感受力强，不容易积压压力。',
    risks: '情绪消费难以察觉，单次金额小但频率极高，累计惊人。',
    says: ['你买的不是奶茶，你买的是那十分钟被治愈的感觉。', '快乐很真实，只是账单来得晚一点。', '你花钱买的不是东西，是一个让今天没那么难熬的理由。'],
  },
  {
    id: 'convenience',
    name: '被方便骗走钱的人',
    desc: '你的时间比金钱更值钱——至少你是这么告诉自己的。方便从来不是免费的。',
    pros: '效率意识强，重视时间价值，执行力高。',
    risks: '习惯性外包生活，长期积累的便利费用远超预期。',
    says: ['你买的不是外卖，你买的是「不想麻烦自己」。', '你省下了时间，却没看见它的价格。', '方便从来不是免费的。'],
  },
  {
    id: 'influenced',
    name: '被种草骗走钱的人',
    desc: '你买的从来不是商品，而是一种你想成为的生活方式。广告只是一面镜子。',
    pros: '对新事物敏感，审美在线，品味不断进化。',
    risks: '容易受外部信息驱动消费，冲动购买后悔率偏高。',
    says: ['你买的不是产品，你买的是那个看起来更好的自己。', '广告卖的从来不是商品，是一种想成为的生活。', '你以为自己在消费，其实你在向理想生活投票。'],
  },
  {
    id: 'justincase',
    name: '被以防万一骗走钱的人',
    desc: '你家里最贵的东西，往往是那些从来没用过的东西。你囤的不是物品，是一种对未来的想象。',
    pros: '居安思危，有备无患意识强，不怕突发情况。',
    risks: '过度囤积和提前准备，购买后实际使用率极低。',
    says: ['你买的不是榨汁机，你买的是那个以为以后每天都会榨果汁的自己。', '你囤的不是东西，是一种对未来的想象。', '家里最贵的东西，往往是那些从来没用过的东西。'],
  },
  {
    id: 'drift',
    name: '被没关系骗走钱的人',
    desc: '真正花掉你钱的，从来不是大消费，而是无数次「没关系，才多少钱」。',
    pros: '心态轻松，不为小钱焦虑，生活质量感受好。',
    risks: '低金额高频消费的累积效应被严重低估。',
    says: ['真正花掉你钱的，从来不是大消费，而是无数次「没关系」。', '每一次都不贵，加起来就很贵。', '钱不是一次花没的，是一点一点放走的。'],
  },
];

// ── STATE ──
let currentQ = 0;
let answers = {};
let totalQuestions = 9; // base 9 + possibly 1 follow-up
let extraQ = null;
let extraAnswer = null;
let isExtraQ = false;

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById('screen-' + id).classList.add('active');
  window.scrollTo(0, 0);
}

function startQuiz() {
  currentQ = 0;
  answers = {};
  extraQ = null;
  extraAnswer = null;
  isExtraQ = false;
  renderQ();
  showScreen('question');
}

function renderQ() {
  if (isExtraQ && extraQ) {
    renderExtraQ();
    return;
  }
  const q = questions[currentQ];
  const pct = Math.round((currentQ / questions.length) * 100);
  document.getElementById('q-num-label').textContent = `第 ${currentQ + 1} 题 / 共 ${questions.length} 题`;
  document.getElementById('q-pct-label').textContent = pct + '%';
  document.getElementById('progress-fill').style.width = pct + '%';
  document.getElementById('q-text').textContent = q.text;
  document.getElementById('q-hint').textContent = q.hint || '';
  document.getElementById('btn-prev').style.display = currentQ === 0 ? 'none' : 'block';

  const grid = document.getElementById('options-grid');
  grid.innerHTML = '';
  q.options.forEach((opt, i) => {
    const letters = ['A', 'B', 'C', 'D', 'E', 'F'];
    const isSelected = answers[q.id] === i;
    const card = document.createElement('div');
    card.className = 'option-card' + (isSelected ? ' selected' : '');
    card.innerHTML = `
      <div class="option-letter">${letters[i]}</div>
      ${icon(opt.icon)}
      <span class="option-label">${opt.label}</span>
    `;
    card.onclick = () => selectOption(i);
    grid.appendChild(card);
  });

  const nextBtn = document.getElementById('btn-next');
  nextBtn.disabled = answers[q.id] === undefined;
  nextBtn.textContent = currentQ === questions.length - 1 ? '查看结果' : '下一题';
}

function renderExtraQ() {
  const q = extraQ;
  const pct = 95;
  document.getElementById('q-num-label').textContent = `加分题`;
  document.getElementById('q-pct-label').textContent = pct + '%';
  document.getElementById('progress-fill').style.width = pct + '%';
  document.getElementById('q-text').textContent = q.text;
  document.getElementById('q-hint').textContent = q.hint || '';
  document.getElementById('btn-prev').style.display = 'block';

  const grid = document.getElementById('options-grid');
  grid.innerHTML = '';
  q.options.forEach((opt, i) => {
    const letters = ['A', 'B', 'C', 'D', 'E', 'F'];
    const isSelected = extraAnswer === i;
    const card = document.createElement('div');
    card.className = 'option-card' + (isSelected ? ' selected' : '');
    card.innerHTML = `
      <div class="option-letter">${letters[i]}</div>
      ${icon(opt.icon)}
      <span class="option-label">${opt.label}</span>
    `;
    card.onclick = () => selectExtraOption(i);
    grid.appendChild(card);
  });

  const nextBtn = document.getElementById('btn-next');
  nextBtn.disabled = extraAnswer === null;
  nextBtn.textContent = '查看结果';
}

function selectOption(i) {
  const q = questions[currentQ];
  answers[q.id] = i;
  document.querySelectorAll('.option-card').forEach((c, idx) => {
    c.classList.toggle('selected', idx === i);
  });
  document.getElementById('btn-next').disabled = false;
}

function selectExtraOption(i) {
  extraAnswer = i;
  document.querySelectorAll('.option-card').forEach((c, idx) => {
    c.classList.toggle('selected', idx === i);
  });
  document.getElementById('btn-next').disabled = false;
}

function nextQ() {
  if (isExtraQ) {
    showResult();
    return;
  }
  if (currentQ === questions.length - 1) {
    // Check if goal needs follow-up
    const goalIdx = answers['goal'];
    const goalOpt = questions[8].options[goalIdx];
    if (goalOpt && goalFollowUp[goalOpt.value]) {
      extraQ = goalFollowUp[goalOpt.value];
      extraAnswer = null;
      isExtraQ = true;
      renderQ();
    } else {
      showResult();
    }
    return;
  }
  currentQ++;
  renderQ();
}

function prevQ() {
  if (isExtraQ) {
    isExtraQ = false;
    extraQ = null;
    extraAnswer = null;
    renderQ();
    return;
  }
  if (currentQ > 0) {
    currentQ--;
    renderQ();
  }
}

// ── RESULT CALC ──
function calcResult() {
  let total = 0;
  const driftScores = { future: 0, joy: 0, convenience: 0, influenced: 0, justincase: 0, drift: 0 };

  // Q1 impulse
  const impulse = answers['impulse'];
  if (impulse === 0) { driftScores.drift += 30; driftScores.joy += 10; }
  else if (impulse === 1) { driftScores.drift += 20; }
  else if (impulse === 2) { driftScores.drift += 10; }

  // Q2 small_spend
  const ss = questions[1].options[answers['small_spend']];
  if (ss) { total += ss.value * ss.annualMult; driftScores.joy += 30; driftScores.drift += 20; }

  // Q3 mood
  const ms = questions[2].options[answers['mood_spend']];
  if (ms) { total += ms.value * ms.annualMult; driftScores.joy += 40; }

  // Q4 subscription — if "没有" subAnnual = 0
  const sub = questions[3].options[answers['subscription']];
  if (sub) {
    total += sub.subAnnual;
    if (sub.subAnnual > 0) {
      driftScores.drift += 20;
      driftScores.justincase += 15;
    }
  }

  // Q5 influenced
  const inf = questions[4].options[answers['influenced']];
  if (inf) { total += inf.value * inf.annualMult; driftScores.influenced += 40; }

  // Q6 growth
  const gr = questions[5].options[answers['growth']];
  if (gr) { total += gr.value * gr.annualMult; driftScores.future += 50; }

  // Q7 convenience
  const conv = questions[6].options[answers['convenience']];
  if (conv) { total += conv.value * conv.annualMult; driftScores.convenience += 50; }

  // Q8 unused
  const un = questions[7].options[answers['unused']];
  if (un) { total += un.value * un.annualMult; driftScores.justincase += 40; driftScores.future += 10; }

  // Normalize drift scores
  const driftTotal = Object.values(driftScores).reduce((a, b) => a + b, 0) || 1;
  const driftPct = {};
  Object.keys(driftScores).forEach(k => {
    driftPct[k] = Math.round((driftScores[k] / driftTotal) * 100);
  });

  // Sort top 3
  const sorted = Object.entries(driftPct).sort((a, b) => b[1] - a[1]);

  return { total, driftPct, sorted };
}

function showResult() {
  showScreen('result');
  const { total, driftPct, sorted } = calcResult();
  const low = Math.round(total * 0.85);
  const high = Math.round(total * 1.15);

  // Goal
  const goalIdx = answers['goal'];
  const goalOpt = questions[8].options[goalIdx];

  // Top persona
  const topPersonaId = sorted[0][0];
  const topPersona = personas.find(p => p.id === topPersonaId);
  const quote = topPersona.says[Math.floor(Math.random() * topPersona.says.length)];
  const secondPersonaId = sorted[1][0];
  const secondPersona = personas.find(p => p.id === secondPersonaId);

  // Concrete
  const teaCups = Math.round(total / 25);
  const japanTrips = Math.round(total / 8000);
  const rentMonths = Math.round(total / 3500);
  const phoneCount = Math.round(total / 5000);

  // Goal completion
  let goalSection = '';
  if (goalOpt) {
    if (goalOpt.value === 'travel' && extraAnswer !== null && extraQ) {
      const dest = extraQ.options[extraAnswer];
      const trips = (dest && dest.cost) ? Math.floor(total / dest.cost) : japanTrips;
      const destName = dest ? dest.label : '旅行';
      goalSection = `
        <div class="result-section">
          <div class="result-eyebrow">你的目标</div>
          <div class="goal-card">
            <div class="goal-label">如果这是你的${destName}旅行基金</div>
            <div class="goal-val">${trips} 次</div>
            <div class="goal-sub">你已经找回了 ${trips} 次${destName}旅行</div>
          </div>
        </div>`;
    } else if (goalOpt.value === 'save' && extraAnswer !== null && extraQ) {
      const target = extraQ.options[extraAnswer].value;
      const pct = Math.min(Math.round((total / target) * 100), 100);
      goalSection = `
        <div class="result-section">
          <div class="result-eyebrow">储蓄目标</div>
          <div class="goal-card">
            <div class="goal-label">目标金额 ¥${target.toLocaleString()}</div>
            <div class="goal-val">¥${total.toLocaleString()}</div>
            <div class="goal-sub">目标完成度</div>
            <div class="goal-prog-track">
              <div class="goal-prog-fill" id="goal-prog" style="width:0%"></div>
            </div>
            <div style="text-align:right;margin-top:6px;font-size:14px;font-weight:700;color:var(--primary)">${pct}%</div>
          </div>
        </div>`;
    } else {
      goalSection = `
        <div class="result-section">
          <div class="result-eyebrow">你的目标</div>
          <div class="goal-card">
            <div class="goal-label">如果你把这笔钱用于「${goalOpt.label}」</div>
            <div class="goal-val">¥${total.toLocaleString()}</div>
            <div class="goal-sub">这就是你找回的钱</div>
          </div>
        </div>`;
    }
  }

  document.getElementById('result-content').innerHTML = `
    <div class="result-section">
      <div class="result-eyebrow">你一年有这么多看不见的钱</div>
      <div class="result-big-num" id="big-num">¥0</div>
      <div class="result-range">估算区间 ¥${low.toLocaleString()} ~ ¥${high.toLocaleString()}</div>
    </div>

    <div class="result-section">
      <div class="result-eyebrow">这笔钱相当于</div>
      <div class="concrete-grid">
        <div class="concrete-card">
          <div class="concrete-num" data-target="${teaCups}">0</div>
          <div class="concrete-label">杯奶茶</div>
        </div>
        <div class="concrete-card">
          <div class="concrete-num" data-target="${japanTrips}">0</div>
          <div class="concrete-label">次日本旅行</div>
        </div>
        <div class="concrete-card">
          <div class="concrete-num" data-target="${rentMonths}">0</div>
          <div class="concrete-label">个月房租</div>
        </div>
        <div class="concrete-card">
          <div class="concrete-num" data-target="${phoneCount}">0</div>
          <div class="concrete-label">部新手机</div>
        </div>
      </div>
    </div>

    <div class="divider"></div>

    <div class="result-section">
      <div class="result-eyebrow">你的漏钱主角</div>
      <div class="aha-quote">${quote}<div class="aha-attr">— ${topPersona.name}</div></div>
    </div>

    <div class="result-section">
      <div class="result-eyebrow">漏钱来源分析</div>
      ${sorted.slice(0, 3).map(([id, pct]) => {
        const p = personas.find(x => x.id === id);
        return `<div class="drift-bar-wrap">
          <div class="drift-bar-header">
            <span class="drift-bar-label">${p.name}</span>
            <span class="drift-bar-pct">${pct}%</span>
          </div>
          <div class="drift-bar-track">
            <div class="drift-bar-fill" data-width="${pct}"></div>
          </div>
        </div>`;
      }).join('')}
    </div>

    <div class="divider"></div>

    <div class="result-section">
      <div class="result-eyebrow">你的主人格</div>
      <div class="persona-card">
        <div class="persona-badge">主人格</div>
        <div class="persona-name">${topPersona.name}</div>
        <div class="persona-desc">${topPersona.desc}</div>
        <div class="persona-row">
          <div class="persona-row-label">PROS</div>
          <div class="persona-row-text">${topPersona.pros}</div>
        </div>
        <div class="persona-row">
          <div class="persona-row-label">RISKS</div>
          <div class="persona-row-text">${topPersona.risks}</div>
        </div>
        <div class="persona-row">
          <div class="persona-row-label">OFTEN SAYS</div>
          <div class="persona-says">"${topPersona.says[0]}"</div>
        </div>
      </div>
      <div class="persona-card" style="opacity:0.85">
        <div class="persona-badge">副人格</div>
        <div class="persona-name">${secondPersona.name}</div>
        <div class="persona-desc">${secondPersona.desc}</div>
      </div>
    </div>

    ${goalSection}

    <div class="divider"></div>

    <div class="result-section">
      <div class="hope-card">
        <div class="hope-title">好消息</div>
        <div class="hope-text">
          这些钱并没有消失。<br>
          它只是流向了你没有注意的地方。<br><br>
          当你开始看见它，<br>
          你就已经开始留住它。
        </div>
      </div>
    </div>

    <button class="share-btn" onclick="shareResult()">生成分享卡片</button>
    <button class="btn-prev" style="display:block;width:100%;margin-top:12px;height:48px;border-radius:20px" onclick="showScreen('home')">重新测试</button>
  `;

  // Animate big number
  animateNum(document.getElementById('big-num'), total, true);

  // Animate concrete cards
  document.querySelectorAll('.concrete-num').forEach(el => {
    animateNum(el, parseInt(el.dataset.target), false);
  });

  // Animate drift bars
  setTimeout(() => {
    document.querySelectorAll('.drift-bar-fill').forEach(el => {
      el.style.width = el.dataset.width + '%';
    });
    const gp = document.getElementById('goal-prog');
    if (gp) {
      const pct = gp.parentElement.nextElementSibling
        ? parseInt(gp.parentElement.nextElementSibling.textContent) : 0;
      gp.style.width = pct + '%';
    }
  }, 400);
}

function animateNum(el, target, isMoney) {
  const duration = 1800;
  const start = Date.now();
  function tick() {
    const elapsed = Date.now() - start;
    const progress = Math.min(elapsed / duration, 1);
    const ease = 1 - Math.pow(1 - progress, 3);
    const cur = Math.round(target * ease);
    el.textContent = isMoney ? '¥' + cur.toLocaleString() : cur.toLocaleString();
    if (progress < 1) requestAnimationFrame(tick);
  }
  requestAnimationFrame(tick);
}

function shareResult() {
  showToast('请截图保存分享到朋友圈或小红书');
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2800);
}
</script>
</body>
</html>
```
