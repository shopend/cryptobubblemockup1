<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Crypto Bubbles UI</title>
  <style>
    :root {
      --bg: radial-gradient(circle at top, #1f2937 0%, #020617 55%, #000 100%);
      --panel: rgba(3, 7, 18, 0.35);
      --panel-solid: rgba(15, 23, 42, 0.96);
      --border: rgba(248, 250, 252, 0.08);
      --text: #e2e8f0;
      --muted: rgba(226, 232, 240, 0.55);
      --primary: #22c55e;
      --danger: #f43f5e;
      --radius-lg: 1.25rem;
    }
    * {
      box-sizing: border-box;
    }
    html, body {
      margin: 0;
      height: 100%;
      background: #020617;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      color: #fff;
      overflow: hidden;
    }
    body {
      background: var(--bg);
      backdrop-filter: blur(10px);
    }
    .app-shell {
      display: flex;
      min-height: 100vh;
      width: 100%;
      position: relative;
    }
    .sidebar {
      width: 270px;
      background: linear-gradient(165deg, rgba(15, 23, 42, 0.78) 0%, rgba(2, 6, 23, 0.22) 100%);
      border-right: 1px solid rgba(255, 255, 255, 0.02);
      backdrop-filter: blur(30px);
      padding: 1.5rem 1.25rem 1.25rem;
      display: flex;
      flex-direction: column;
      gap: 1.4rem;
      z-index: 30;
    }
    .sidebar-head {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: .8rem;
    }
    .brand {
      display: flex;
      align-items: center;
      gap: .55rem;
    }
    .brand-icon {
      width: 34px;
      height: 34px;
      border-radius: .95rem;
      background: radial-gradient(circle, rgba(34, 197, 94, 0.35) 0, rgba(9, 121, 105, 0) 45%), linear-gradient(160deg, #22c55e 0%, #166534 100%);
      display: inline-flex;
      align-items: center;
      justify-content: center;
      font-size: .85rem;
      font-weight: 700;
      box-shadow: 0 12px 30px rgba(34, 197, 94, .3);
    }
    .brand-name {
      font-weight: 600;
      font-size: 1rem;
      line-height: 1;
    }
    .sidebar small {
      color: rgba(226, 232, 240, .45);
      font-size: .6rem;
      letter-spacing: .12em;
      text-transform: uppercase;
    }
    .sidebar .close-btn {
      display: none;
      background: rgba(2,6,23,.12);
      border: 1px solid rgba(248,250,252,.06);
      width: 32px;
      height: 32px;
      border-radius: .7rem;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      color: #e2e8f0;
    }
    .sidebar-section-title {
      font-size: .65rem;
      letter-spacing: .1em;
      text-transform: uppercase;
      color: rgba(226, 232, 240, .3);
      margin-bottom: .45rem;
    }
    .filter-chips {
      display: flex;
      flex-wrap: wrap;
      gap: .45rem;
    }
    .chip {
      background: rgba(15, 23, 42, .2);
      border: 1px solid rgba(226, 232, 240, .04);
      border-radius: 9999px;
      padding: .35rem .65rem .25rem;
      font-size: .7rem;
      color: rgba(226, 232, 240, .65);
      display: inline-flex;
      gap: .25rem;
      align-items: center;
      cursor: pointer;
      transition: .15s ease-out background, .15s ease-out color;
      backdrop-filter: blur(10px);
    }
    .chip.active, .chip:hover {
      background: rgba(34, 197, 94, .16);
      border-color: rgba(34, 197, 94, .35);
      color: #fff;
    }
    .metrics {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: .45rem;
    }
    .metric {
      background: radial-gradient(circle at bottom, rgba(15, 23, 42, .6) 0, rgba(15, 23, 42, 0) 50%), rgba(15, 23, 42, .12);
      border: 1px solid rgba(226, 232, 240, .05);
      border-radius: .9rem;
      padding: .45rem .5rem .55rem;
    }
    .metric span {
      font-size: .6rem;
      color: rgba(226, 232, 240, .35);
    }
    .metric strong {
      display: block;
      margin-top: .15rem;
      font-size: .9rem;
      font-weight: 600;
    }
    .metric small {
      display: inline-flex;
      gap: .2rem;
      margin-top: .4rem;
      border-radius: 9999px;
      padding: .15rem .45rem .1rem;
    }
    .metric small.pos {
      background: rgba(34,197,94,.12);
      color: rgba(190,242,100,1);
    }
    .metric small.neg {
      background: rgba(244,63,94,.12);
      color: rgba(254,226,226,1);
    }
    .sidebar-footer {
      margin-top: auto;
      display: flex;
      flex-direction: column;
      gap: .5rem;
    }
    .btn-secondary {
      background: rgba(15, 23, 42, .2);
      border: 1px solid rgba(226, 232, 240, .08);
      border-radius: .85rem;
      padding: .45rem .65rem;
      font-size: .65rem;
      display: inline-flex;
      gap: .4rem;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      color: #e2e8f0;
      transition: .15s ease-out background;
    }
    .btn-secondary:hover {
      background: rgba(15, 23, 42, .4);
    }

    .main {
      flex: 1;
      display: flex;
      flex-direction: column;
      position: relative;
      overflow: hidden;
      background: radial-gradient(circle at 40% 40%, rgba(15, 23, 42, .3) 0, rgba(15, 23, 42, 0) 50%), linear-gradient(140deg, #020617 0, #020617 40%, #000 100%);
    }
    .topbar {
      height: 4.5rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 1.6rem;
      gap: 1.25rem;
      border-bottom: 1px solid rgba(148, 163, 184, .1);
      background: radial-gradient(circle at top, rgba(2,6,23,.6) 0, rgba(2,6,23,.12) 100%);
      backdrop-filter: blur(20px);
      z-index: 20;
    }
    .title-block {
      display: flex;
      align-items: center;
      gap: .85rem;
    }
    .menu-btn {
      display: none;
      width: 34px;
      height: 34px;
      background: rgba(15,23,42,.1);
      border: 1px solid rgba(248,250,252,.05);
      border-radius: .75rem;
      align-items: center;
      justify-content: center;
      cursor: pointer;
    }
    .title-block h1 {
      margin: 0;
      font-size: 1.06rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: .4rem;
    }
    .title-block p {
      margin: 0;
      font-size: .65rem;
      color: rgba(226, 232, 240, .35);
    }
    .badge-live {
      background: rgba(34,197,94,.1);
      color: #a7f3d0;
      border: 1px solid rgba(34,197,94,.27);
      border-radius: 999px;
      padding: .15rem .5rem .15rem;
      font-size: .6rem;
    }
    .controls {
      display: flex;
      gap: 1rem;
      align-items: center;
    }
    .timeframe {
      display: inline-flex;
      gap: .35rem;
      background: rgba(15, 23, 42, .2);
      border: 1px solid rgba(226, 232, 240, .04);
      border-radius: 9999px;
      padding: .25rem .4rem;
    }
    .timeframe button {
      background: transparent;
      border: 0;
      color: rgba(226, 232, 240, .6);
      font-size: .68rem;
      border-radius: 9999px;
      padding: .25rem .6rem .2rem;
      cursor: pointer;
    }
    .timeframe button.active {
      background: rgba(15, 23, 42, .7);
      color: #fff;
      border: 1px solid rgba(236, 252, 203, .02);
      box-shadow: 0 8px 20px rgba(15, 23, 42, .3);
    }
    .search-box {
      background: rgba(15, 23, 42, .15);
      border: 1px solid rgba(148, 163, 184, .2);
      border-radius: .7rem;
      display: flex;
      align-items: center;
      gap: .4rem;
      padding: .35rem .6rem;
    }
    .search-box input {
      background: transparent;
      border: 0;
      color: #e2e8f0;
      font-size: .68rem;
      outline: none;
      width: 120px;
    }
    .search-box input::placeholder {
      color: rgba(148, 163, 184, .4);
    }
    .bubble-area {
      position: relative;
      width: 100%;
      height: calc(100vh - 4.5rem);
      overflow: hidden;
      padding: 1rem 1.6rem 1.5rem;
    }
    .bubble-area::after {
      content: "";
      position: absolute;
      inset: -30% 0 0;
      background: radial-gradient(circle at 50% 0, rgba(30, 144, 255, 0.09) 0, rgba(0,0,0,0) 60%);
      pointer-events: none;
    }
    .bubble {
      position: absolute;
      border-radius: 9999px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      gap: .25rem;
      color: #fff;
      cursor: grab;
      user-select: none;
      box-shadow:
        0 12px 36px rgba(0,0,0,.22),
        inset 0 0 60px rgba(255,255,255,0.03);
      border: 1px solid rgba(255, 255, 255, 0.12);
      backdrop-filter: blur(1px);
      animation: float 16s ease-in-out infinite alternate;
      will-change: transform;
    }
    .bubble.up {
      background: radial-gradient(circle at top, rgba(34,197,94,.95) 0, rgba(6,95,70,.65) 70%);
    }
    .bubble.down {
      background: radial-gradient(circle at top, rgba(244,63,94,.95) 0, rgba(153,27,27,.55) 70%);
    }
    .bubble .symbol {
      font-weight: 600;
      letter-spacing: .02em;
      font-size: clamp(.7rem, 1.1vw, 1.05rem);
    }
    .bubble .change {
      font-size: .6rem;
      background: rgba(15,23,42,.12);
      border: 1px solid rgba(15,23,42,.0);
      border-radius: 9999px;
      padding: .15rem .55rem .1rem;
    }
    .bubble.up .change {
      background: rgba(22,163,74,.35);
    }
    .bubble.down .change {
      background: rgba(248,113,113,.35);
    }
    .bubble .name {
      font-size: .55rem;
      color: rgba(255,255,255,.75);
    }
    .bubble.is-big .name {
      display: block;
    }
    .bubble:not(.is-big) .name {
      display: none;
    }
    .bubble.dragging {
      animation-play-state: paused;
      cursor: grabbing;
      box-shadow: 0 10px 36px rgba(0,0,0,.32);
    }
    @keyframes float {
      from { transform: translate3d(0, 0, 0); }
      to { transform: translate3d(0, calc(var(--float, 24px)), 0) scale(1.03); }
    }

    .info-panel {
      position: absolute;
      top: 5.1rem;
      right: 1.6rem;
      width: 280px;
      background: rgba(2, 6, 23, .25);
      border: 1px solid rgba(148, 163, 184, .1);
      border-radius: 1.1rem;
      backdrop-filter: blur(26px);
      padding: .85rem .75rem .7rem;
      opacity: 0;
      transform: translateY(12px);
      pointer-events: none;
      transition: .2s ease-out;
      z-index: 20;
    }
    .info-panel.show {
      opacity: 1;
      transform: translateY(0);
      pointer-events: auto;
    }
    .info-panel-header {
      display: flex;
      justify-content: space-between;
      gap: .5rem;
      align-items: flex-start;
    }
    .info-panel h2 {
      font-size: .85rem;
      margin: 0;
    }
    .info-panel small {
      display: block;
      color: rgba(226,232,240,.3);
      font-size: .58rem;
    }
    .info-close {
      background: rgba(15, 23, 42, .12);
      border: 1px solid rgba(248, 250, 252, .04);
      border-radius: .75rem;
      width: 26px;
      height: 26px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      color: rgba(226,232,240,.4);
    }
    .info-price {
      margin-top: .65rem;
      display: flex;
      gap: .4rem;
      align-items: center;
    }
    .info-price strong {
      font-size: 1.28rem;
    }
    .info-change {
      background: rgba(34, 197, 94, .08);
      border: 1px solid rgba(34, 197, 94, .25);
      border-radius: 999px;
      padding: .15rem .55rem .1rem;
      font-size: .6rem;
    }
    .info-change.neg {
      background: rgba(244,63,94,.1);
      border-color: rgba(244,63,94,.25);
    }
    .info-meta {
      margin-top: .7rem;
      display: flex;
      gap: .55rem;
    }
    .info-meta-item {
      background: rgba(2, 6, 23, .4);
      border: 1px solid rgba(148, 163, 184, .05);
      border-radius: .75rem;
      padding: .35rem .55rem .3rem;
      flex: 1;
    }
    .info-meta-item span {
      font-size: .55rem;
      color: rgba(148, 163, 184, .5);
    }
    .info-meta-item strong {
      display: block;
      font-size: .7rem;
      margin-top: .15rem;
    }
    .btn-primary {
      margin-top: .7rem;
      background: linear-gradient(160deg, #22c55e 0%, #15803d 100%);
      border: 0;
      color: #02131e;
      font-weight: 600;
      border-radius: .75rem;
      padding: .35rem .6rem;
      width: 100%;
      font-size: .68rem;
      cursor: pointer;
      display: inline-flex;
      gap: .25rem;
      align-items: center;
      justify-content: center;
      box-shadow: 0 10px 24px rgba(34, 197, 94, .3);
    }

    .sidebar-backdrop {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(15,23,42,.3);
      backdrop-filter: blur(2px);
      z-index: 25;
    }

    @media (max-width: 1100px) {
      .sidebar {
        position: fixed;
        inset: 0 auto 0 0;
        transform: translateX(-100%);
        transition: transform .25s ease-out;
        width: 275px;
      }
      .sidebar.show {
        transform: translateX(0);
      }
      .sidebar .close-btn {
        display: inline-flex;
      }
      .sidebar-backdrop.show {
        display: block;
      }
      .menu-btn {
        display: inline-flex;
      }
      .controls {
        gap: .65rem;
      }
      .search-box input {
        width: 90px;
      }
      .bubble-area {
        padding-inline: 1rem;
      }
    }
    @media (max-width: 700px) {
      .controls {
        gap: .4rem;
      }
      .timeframe {
        gap: .25rem;
      }
      .search-box {
        min-width: 95px;
      }
      .title-block h1 {
        font-size: .95rem;
      }
      .info-panel {
        width: min(100% - 2.1rem, 300px);
        right: 1rem;
      }
      .bubble-area {
        height: calc(100vh - 4.5rem);
      }
    }
    @media (max-width: 480px) {
      .search-box {
        display: none;
      }
      .timeframe button:nth-child(3) {
        display: none;
      }
    }
  </style>
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar" class="sidebar">
      <div class="sidebar-head">
        <div class="brand">
          <div class="brand-icon">B</div>
          <div>
            <div class="brand-name">Bubbleboard</div>
            <small>live market view</small>
          </div>
        </div>
        <button class="close-btn" id="close-sidebar" aria-label="Close">
          <svg width="16" height="16" fill="none" stroke="currentColor" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round">
            <path d="M4 4l8 8M12 4l-8 8"></path>
          </svg>
        </button>
      </div>

      <div>
        <div class="sidebar-section-title">Quick filters</div>
        <div class="filter-chips">
          <button class="chip active" data-filter="all">All assets</button>
          <button class="chip" data-filter="gainers">
            <span style="width:6px;height:6px;background:#22c55e;border-radius:999px;display:inline-block;"></span>
            Gainers
          </button>
          <button class="chip" data-filter="losers">
            <span style="width:6px;height:6px;background:#f43f5e;border-radius:999px;display:inline-block;"></span>
            Losers
          </button>
          <button class="chip" data-filter="l1">Layer 1</button>
          <button class="chip" data-filter="l2">Layer 2</button>
          <button class="chip" data-filter="defi">DeFi</button>
          <button class="chip" data-filter="meme">Meme</button>
        </div>
      </div>

      <div>
        <div class="sidebar-section-title">Market overview</div>
        <div class="metrics">
          <div class="metric">
            <span>Total Mcap</span>
            <strong id="mcap-total">$1.72T</strong>
            <small class="pos" id="mcap-change">+1.8%</small>
          </div>
          <div class="metric">
            <span>24h Vol</span>
            <strong id="vol-total">$68.4B</strong>
            <small class="pos" id="vol-change">+4.2%</small>
          </div>
          <div class="metric">
            <span>Assets</span>
            <strong id="assets-count">24</strong>
            <small class="pos" id="gainers-sm">gainers</small>
          </div>
          <div class="metric">
            <span>Sentiment</span>
            <strong id="sentiment-ind">Neutral</strong>
            <small class="pos" id="sentiment-sm">52 / 100</small>
          </div>
        </div>
      </div>

      <div>
        <div class="sidebar-section-title">Legend</div>
        <div style="display:flex;gap:.45rem;flex-wrap:wrap">
          <span style="display:inline-flex;align-items:center;gap:.25rem;font-size:.6rem;color:rgba(226,232,240,.4);">
            <span style="width:10px;height:10px;border-radius:999px;background:rgba(34,197,94,.8);"></span>Price up
          </span>
          <span style="display:inline-flex;align-items:center;gap:.25rem;font-size:.6rem;color:rgba(226,232,240,.4);">
            <span style="width:10px;height:10px;border-radius:999px;background:rgba(244,63,94,.8);"></span>Price down
          </span>
          <span style="display:inline-flex;align-items:center;gap:.25rem;font-size:.6rem;color:rgba(226,232,240,.4);">
            bubble size = volume
          </span>
        </div>
      </div>

      <div class="sidebar-footer">
        <button class="btn-secondary" id="reset-layout">
          <svg width="14" height="14" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="1 4 1 10 7 10"></polyline>
            <polyline points="23 20 23 14 17 14"></polyline>
            <path d="M20.49 9A9 9 0 0 0 5.64 5.64L1 10m22 4-4.64 4.36A9 9 0 0 1 3.51 15"></path>
          </svg>
          Reset layout
        </button>
        <button class="btn-secondary" id="toggle-grid">
          <svg width="14" height="14" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.4" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <rect x="3" y="3" width="7" height="7"></rect>
            <rect x="14" y="3" width="7" height="7"></rect>
            <rect x="14" y="14" width="7" height="7"></rect>
            <rect x="3" y="14" width="7" height="7"></rect>
          </svg>
          Toggle grid
        </button>
      </div>
    </aside>
    <div id="sidebar-backdrop" class="sidebar-backdrop"></div>

    <main class="main">
      <header class="topbar">
        <div class="title-block">
          <button id="open-sidebar" class="menu-btn" aria-label="Open sidebar">
            <svg width="16" height="16" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.4" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <line x1="3" y1="12" x2="21" y2="12"></line>
              <line x1="3" y1="6" x2="21" y2="6"></line>
              <line x1="3" y1="18" x2="21" y2="18"></line>
            </svg>
          </button>
          <div>
            <h1>Crypto Bubbles <span class="badge-live">Live</span></h1>
            <p>Drag bubbles around. Tap to see details. Filters on the left.</p>
          </div>
        </div>
        <div class="controls">
          <div class="timeframe" id="timeframe-switcher">
            <button type="button" data-timeframe="1h">1H</button>
            <button type="button" data-timeframe="24h" class="active">24H</button>
            <button type="button" data-timeframe="7d">7D</button>
          </div>
          <div class="search-box">
            <svg width="13" height="13" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.4" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="11" cy="11" r="7"></circle>
              <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
            </svg>
            <input id="search-input" type="text" placeholder="Search..." autocomplete="off" />
          </div>
        </div>
      </header>

      <div class="bubble-area" id="bubble-area" aria-label="Crypto bubbles board"></div>

      <section id="info-panel" class="info-panel" aria-live="polite">
        <div class="info-panel-header">
          <div>
            <h2 id="info-title">Bitcoin (BTC)</h2>
            <small id="info-subtitle">Layer 1</small>
          </div>
          <button id="info-close" class="info-close" aria-label="Close panel">
            <svg width="15" height="15" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round">
              <path d="M4 4l7 7M11 4l-7 7"></path>
            </svg>
          </button>
        </div>
        <div class="info-price">
          <strong id="info-change">+2.4%</strong>
          <span id="info-timeframe" style="font-size:.6rem;color:rgba(226,232,240,.4);">24h change</span>
        </div>
        <div class="info-meta">
          <div class="info-meta-item">
            <span>Volume</span>
            <strong id="info-volume">$35.4B</strong>
          </div>
          <div class="info-meta-item">
            <span>Liquidity</span>
            <strong id="info-liq">78%</strong>
          </div>
        </div>
        <button class="btn-primary" id="info-watch">
          <svg width="14" height="14" viewBox="0 0 24 24" stroke="#00330a" stroke-width="1.4" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path d="M12 20l-7-4V8l7 4 7-4v8z"></path>
          </svg>
          Add to watchlist
        </button>
      </section>
    </main>
  </div>

  <script>
    (function() {
      const baseCoins = [
        { symbol: 'BTC', name: 'Bitcoin', sector: 'L1', baseSize: 130 },
        { symbol: 'ETH', name: 'Ethereum', sector: 'L1', baseSize: 118 },
        { symbol: 'SOL', name: 'Solana', sector: 'L1', baseSize: 108 },
        { symbol: 'BNB', name: 'BNB', sector: 'L1', baseSize: 104 },
        { symbol: 'XRP', name: 'XRP', sector: 'Payments', baseSize: 101 },
        { symbol: 'DOGE', name: 'Dogecoin', sector: 'Meme', baseSize: 95 },
        { symbol: 'ADA', name: 'Cardano', sector: 'L1', baseSize: 92 },
        { symbol: 'AVAX', name: 'Avalanche', sector: 'L1', baseSize: 90 },
        { symbol: 'TON', name: 'Toncoin', sector: 'L1', baseSize: 86 },
        { symbol: 'LINK', name: 'Chainlink', sector: 'Infrastructure', baseSize: 83 },
        { symbol: 'PEPE', name: 'Pepe', sector: 'Meme', baseSize: 80 },
        { symbol: 'SHIB', name: 'Shiba Inu', sector: 'Meme', baseSize: 77 },
        { symbol: 'TRX', name: 'Tron', sector: 'L1', baseSize: 78 },
        { symbol: 'UNI', name: 'Uniswap', sector: 'DeFi', baseSize: 76 },
        { symbol: 'ARB', name: 'Arbitrum', sector: 'L2', baseSize: 72 },
        { symbol: 'OP', name: 'Optimism', sector: 'L2', baseSize: 70 },
        { symbol: 'INJ', name: 'Injective', sector: 'DeFi', baseSize: 74 },
        { symbol: 'TIA', name: 'Celestia', sector: 'Infrastructure', baseSize: 70 },
        { symbol: 'SUI', name: 'Sui', sector: 'L1', baseSize: 68 },
        { symbol: 'APT', name: 'Aptos', sector: 'L1', baseSize: 67 },
        { symbol: 'PYTH', name: 'Pyth', sector: 'Infrastructure', baseSize: 66 },
        { symbol: 'SEI', name: 'Sei', sector: 'L1', baseSize: 64 },
        { symbol: 'FLOKI', name: 'Floki', sector: 'Meme', baseSize: 62 },
        { symbol: 'WIF', name: 'dogwifhat', sector: 'Meme', baseSize: 64 }
      ];

      const bubbleArea = document.getElementById('bubble-area');
      const infoPanel = document.getElementById('info-panel');
      const infoTitle = document.getElementById('info-title');
      const infoSubtitle = document.getElementById('info-subtitle');
      const infoChange = document.getElementById('info-change');
      const infoVolume = document.getElementById('info-volume');
      const infoLiq = document.getElementById('info-liq');
      const infoTimeframe = document.getElementById('info-timeframe');
      const infoClose = document.getElementById('info-close');
      const timeframeSwitcher = document.getElementById('timeframe-switcher');
      const sidebar = document.getElementById('sidebar');
      const openSidebar = document.getElementById('open-sidebar');
      const closeSidebar = document.getElementById('close-sidebar');
      const sidebarBackdrop = document.getElementById('sidebar-backdrop');
      const searchInput = document.getElementById('search-input');
      const filterButtons = document.querySelectorAll('.chip[data-filter]');
      const resetLayoutBtn = document.getElementById('reset-layout');
      const toggleGridBtn = document.getElementById('toggle-grid');

      const assetsCount = document.getElementById('assets-count');
      const gainersSm = document.getElementById('gainers-sm');
      const mcapTotal = document.getElementById('mcap-total');
      const volTotal = document.getElementById('vol-total');
      const mcapChange = document.getElementById('mcap-change');
      const volChange = document.getElementById('vol-change');

      let currentTimeframe = '24h';
      let currentFilter = 'all';
      let searchTerm = '';
      let currentCoins = [];
      let gridMode = false;

      function randBetween(min, max) {
        return Math.random() * (max - min) + min;
      }

      function generateCoinsForTimeframe(tf) {
        const volatility = tf === '1h' ? 3 : tf === '24h' ? 9 : 19;
        const coins = baseCoins.map((coin, idx) => {
          const change = parseFloat((Math.random() * volatility * 2 - volatility).toFixed(1));
          const volume = +(Math.random() * 6 + 0.4).toFixed(2); // billions
          const liquidity = Math.floor(Math.random() * 35) + 60; // 60-95
          const size = coin.baseSize + (Math.abs(change) * 1.6) + (volume * 2);
          return {
            ...coin,
            change,
            volume,
            liquidity,
            bubbleSize: Math.max(58, Math.min(size, 180)),
            id: coin.symbol + '-' + tf
          };
        });
        return coins;
      }

      function applyFilters() {
        return currentCoins.filter(c => {
          if (searchTerm && !c.symbol.toLowerCase().includes(searchTerm) && !c.name.toLowerCase().includes(searchTerm)) return false;
          switch (currentFilter) {
            case 'gainers': return c.change > 0;
            case 'losers': return c.change < 0;
            case 'l1': return c.sector === 'L1';
            case 'l2': return c.sector === 'L2';
            case 'defi': return c.sector === 'DeFi';
            case 'meme': return c.sector === 'Meme';
            default: return true;
          }
        });
      }

      function renderBubbles(coins) {
        bubbleArea.innerHTML = '';
        const rect = bubbleArea.getBoundingClientRect();
        const width = rect.width;
        const height = rect.height;
        const placed = [];

        coins.forEach((coin, index) => {
          const bubble = document.createElement('div');
          const size = coin.bubbleSize;
          bubble.className = 'bubble ' + (coin.change >= 0 ? 'up' : 'down') + (size > 108 ? ' is-big' : '');
          bubble.dataset.symbol = coin.symbol;

          const nameHtml = size > 115 ? '<span class="name">' + coin.name + '</span>' : '';
          bubble.innerHTML = '<span class="symbol">' + coin.symbol + '</span>' + nameHtml + '<span class="change">' + (coin.change > 0 ? '+' : '') + coin.change.toFixed(1) + '%</span>';

          bubble.style.width = size + 'px';
          bubble.style.height = size + 'px';
          bubble.style.setProperty('--float', (randBetween(14, 34)) + 'px');

          // simple collision-aware placement
          let x, y, safe = false, tries = 0, maxTries = 80;
          while (!safe && tries < maxTries) {
            x = randBetween(0, width - size);
            y = randBetween(0, height - size);
            safe = true;
            for (const p of placed) {
              const dx = p.x - x;
              const dy = p.y - y;
              const dist = Math.sqrt(dx * dx + dy * dy);
              if (dist < (p.size + size) * 0.52) {
                safe = false;
                break;
              }
            }
            tries++;
          }

          bubble.style.left = x + 'px';
          bubble.style.top = y + 'px';
          placed.push({ x, y, size, el: bubble, coin });

          // drag logic
          let isDragging = false;
          let dragOffsetX = 0;
          let dragOffsetY = 0;

          bubble.addEventListener('pointerdown', function(e) {
            isDragging = true;
            dragOffsetX = e.clientX - bubble.offsetLeft;
            dragOffsetY = e.clientY - bubble.offsetTop;
            bubble.classList.add('dragging');
            bubbleArea.appendChild(bubble); // bring to front
          });
          window.addEventListener('pointermove', function(e) {
            if (!isDragging) return;
            const nx = Math.min(Math.max(0, e.clientX - dragOffsetX), width - size);
            const ny = Math.min(Math.max(0, e.clientY - dragOffsetY), height - size);
            bubble.style.left = nx + 'px';
            bubble.style.top = ny + 'px';
          });
          window.addEventListener('pointerup', function() {
            if (!isDragging) return;
            isDragging = false;
            bubble.classList.remove('dragging');
          });

          bubble.addEventListener('click', function(e) {
            e.stopPropagation();
            showInfoForCoin(coin);
          });

          bubbleArea.appendChild(bubble);
        });

        updateStats(coins);
      }

      function updateStats(coins) {
        const total = coins.length;
        const gainers = coins.filter(c => c.change > 0).length;
        const losers = coins.filter(c => c.change < 0).length;
        assetsCount.textContent = total.toString();
        gainersSm.textContent = gainers + ' gainers';
        mcapTotal.textContent = '$' + (1.5 + Math.random() * 0.3).toFixed(2) + 'T';
        volTotal.textContent = '$' + (60 + Math.random() * 10).toFixed(1) + 'B';
        mcapChange.textContent = (Math.random() > 0.5 ? '+' : '-') + (Math.random() * 2.5).toFixed(1) + '%';
        volChange.textContent = (Math.random() > 0.35 ? '+' : '-') + (Math.random() * 4.5).toFixed(1) + '%';
      }

      function showInfoForCoin(coin) {
        infoTitle.textContent = coin.name + ' (' + coin.symbol + ')';
        infoSubtitle.textContent = coin.sector;
        infoChange.textContent = (coin.change > 0 ? '+' : '') + coin.change.toFixed(1) + '%';
        infoChange.classList.toggle('neg', coin.change < 0);
        infoVolume.textContent = '$' + coin.volume.toFixed(2) + 'B';
        infoLiq.textContent = coin.liquidity + '%';
        infoTimeframe.textContent = currentTimeframe + ' change';
        infoPanel.classList.add('show');
      }

      function regenerateAndRender() {
        currentCoins = generateCoinsForTimeframe(currentTimeframe);
        const filtered = applyFilters();
        renderBubbles(filtered);
      }

      // timeframe click
      timeframeSwitcher.addEventListener('click', function(e) {
        const btn = e.target.closest('button[data-timeframe]');
        if (!btn) return;
        document.querySelectorAll('#timeframe-switcher button').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        currentTimeframe = btn.dataset.timeframe;
        regenerateAndRender();
      });

      // filters click
      filterButtons.forEach(btn => {
        btn.addEventListener('click', function() {
          filterButtons.forEach(b => b.classList.remove('active'));
          this.classList.add('active');
          currentFilter = this.dataset.filter;
          const filtered = applyFilters();
          renderBubbles(filtered);
        });
      });

      // search
      searchInput.addEventListener('input', function() {
        searchTerm = this.value.trim().toLowerCase();
        const filtered = applyFilters();
        renderBubbles(filtered);
      });

      // info close
      infoClose.addEventListener('click', function() {
        infoPanel.classList.remove('show');
      });

      // open / close sidebar (mobile)
      openSidebar.addEventListener('click', function() {
        sidebar.classList.add('show');
        sidebarBackdrop.classList.add('show');
      });
      closeSidebar.addEventListener('click', function() {
        sidebar.classList.remove('show');
        sidebarBackdrop.classList.remove('show');
      });
      sidebarBackdrop.addEventListener('click', function() {
        sidebar.classList.remove('show');
        sidebarBackdrop.classList.remove('show');
      });

      // reset layout
      resetLayoutBtn.addEventListener('click', function() {
        const filtered = applyFilters();
        renderBubbles(filtered);
      });

      // toggle grid overlay
      toggleGridBtn.addEventListener('click', function() {
        gridMode = !gridMode;
        bubbleArea.style.backgroundImage = gridMode
          ? 'linear-gradient(rgba(15,23,42,.28) 1px, transparent 1px), linear-gradient(90deg, rgba(15,23,42,.28) 1px, transparent 1px)'
          : 'none';
        if (gridMode) {
          bubbleArea.style.backgroundSize = '40px 40px';
        }
      });

      // click on empty area hides info
      bubbleArea.addEventListener('click', function() {
        infoPanel.classList.remove('show');
      });

      // re-render on resize (debounced)
      let resizeTimeout;
      window.addEventListener('resize', function() {
        clearTimeout(resizeTimeout);
        resizeTimeout = setTimeout(function() {
          const filtered = applyFilters();
          renderBubbles(filtered);
        }, 200);
      });

      // init
      regenerateAndRender();
    })();
  </script>
</body>
</html>
