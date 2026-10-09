[index.html](https://github.com/user-attachments/files/33262137/index.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>21點量化決策 Pro+ (加密戰情室)</title>
<style>
  :root {
    --bg-main: #0B0E14;
    --card-bg: #141A24;
    --border-color: #242E3E;
    --accent-green: #00FFAA;
    --accent-blue: #00D8FF;
    --accent-red: #FF4D4D;
    --accent-yellow: #FFB300;
  }
  * { 
    box-sizing: border-box; 
    -webkit-tap-highlight-color: transparent;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    user-select: none;
  }
  body { 
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; 
    background: var(--bg-main); 
    color: #FFFFFF; 
    margin: 0; 
    padding: 8px 10px 20px 10px; 
    touch-action: manipulation;
  }
  
  #loginOverlay {
    position: fixed;
    top: 0; left: 0; width: 100%; height: 100%;
    background: #07090D;
    z-index: 9999;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 16px;
  }
  .login-card {
    background: #111722;
    border: 1.5px solid var(--accent-blue);
    border-radius: 14px;
    padding: 24px 20px;
    width: 100%;
    max-width: 340px;
    text-align: center;
    box-shadow: 0 10px 30px rgba(0,0,0,0.8);
  }
  .login-title { font-size: 1.15rem; font-weight: 900; color: var(--accent-green); margin-bottom: 6px; }
  .login-desc { font-size: 0.75rem; color: #8FA3BF; margin-bottom: 18px; }
  .login-input {
    width: 100%;
    background: #0B0E14;
    border: 1px solid #33445C;
    border-radius: 8px;
    padding: 10px 12px;
    font-size: 1.1rem;
    color: #FFF;
    text-align: center;
    letter-spacing: 3px;
    margin-bottom: 12px;
  }
  .login-input:focus { outline: none; border-color: var(--accent-green); box-shadow: 0 0 8px rgba(0,255,170,0.4); }
  .login-btn {
    width: 100%;
    background: linear-gradient(90deg, #00875A, var(--accent-green));
    border: none;
    border-radius: 8px;
    padding: 10px;
    font-size: 0.95rem;
    font-weight: bold;
    color: #07090D;
    cursor: pointer;
  }

  #appContainer { display: none; }

  .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
  .title { font-size: 1.0rem; font-weight: 900; color: var(--accent-green); letter-spacing: 0.5px; display: flex; align-items: center; gap: 4px; }
  .header-actions { display: flex; gap: 4px; }
  .btn-top-action { background: #1B2433; border: 1px solid var(--border-color); color: #BAC7D5; border-radius: 6px; padding: 4px 8px; font-size: 0.7rem; cursor: pointer; font-weight: bold; }
  .btn-top-action.matrix-btn { background: #012D21; border-color: #009647; color: #00FFAA; }

  .rule-settings {
    display: none;
    background: #111620;
    border: 1px solid #2B384E;
    border-radius: 8px;
    padding: 8px;
    margin-bottom: 8px;
    font-size: 0.75rem;
    grid-template-columns: repeat(2, 1fr);
    gap: 6px;
  }
  .rule-settings.open { display: grid; }
  .rule-item { display: flex; justify-content: space-between; align-items: center; background: #18202E; padding: 4px 8px; border-radius: 4px; }
  .rule-item select, .rule-item input { background: #0B0E14; color: #FFF; border: 1px solid #3B4B66; border-radius: 4px; padding: 2px 4px; font-size: 0.72rem; width: 80px; text-align: right; }

  .shoe-bar-wrap { margin-bottom: 8px; }
  .shoe-bar-info { display: flex; justify-content: space-between; font-size: 0.68rem; color: #6E829F; margin-bottom: 2px; }
  .shoe-progress-bg { width: 100%; height: 5px; background: #18202E; border-radius: 3px; overflow: hidden; }
  .shoe-progress-fill { height: 100%; width: 0%; background: linear-gradient(90deg, #00D8FF, var(--accent-green)); transition: width 0.2s; }

  .signal-box { border-radius: 10px; padding: 8px 10px; text-align: center; margin-bottom: 8px; font-weight: bold; font-size: 0.92rem; box-shadow: 0 4px 12px rgba(0,0,0,0.5); }
  .signal-neutral { background: #131822; color: #8FA3BF; border: 1px solid var(--border-color); }
  .signal-green { background: #013318; color: #00FF88; border: 1px solid #009647; box-shadow: 0 0 16px rgba(0,255,136,0.3); }
  .signal-red { background: #2B0D0D; color: #FF5A5A; border: 1px solid #631C1C; }
  .signal-sub { font-size: 0.72rem; font-weight: normal; margin-top: 3px; display: block; opacity: 0.95; }

  .stats-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 8px; }
  .stat-card { background: var(--card-bg); border-radius: 8px; padding: 6px 2px; text-align: center; border: 1px solid var(--border-color); }
  .stat-label { font-size: 0.62rem; color: #6F829D; margin-bottom: 1px; }
  .stat-value { font-size: 1.0rem; font-weight: 800; }

  /* 🎯 莊家手牌區塊 */
  .dealer-panel { background: #141A24; border: 1px solid #2B384E; border-radius: 10px; padding: 8px; margin-bottom: 8px; }
  .dealer-panel-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px; }
  .dealer-panel-title { font-size: 0.78rem; font-weight: bold; color: var(--accent-yellow); display: flex; align-items: center; gap: 6px; }
  .dealer-action-group { display: flex; gap: 4px; }
  .btn-dealer-action { background: #1B2433; border: 1px solid #2E3B50; color: #BAC7D5; border-radius: 5px; width: 26px; height: 26px; display: flex; align-items: center; justify-content: center; font-size: 0.8rem; cursor: pointer; font-weight: bold; }
  .btn-dealer-action.btn-plus { background: #33220B; border-color: #704214; color: var(--accent-yellow); }
  .btn-dealer-action:active { transform: scale(0.92); }

  .dealer-current-row { background: #0B0E14; border: 1px solid #222C3E; border-radius: 8px; padding: 6px 8px; margin-bottom: 6px; min-height: 38px; display: flex; align-items: center; }
  .dealer-cards-flex { display: flex; gap: 4px; align-items: center; flex-wrap: wrap; }
  .dealer-card-chip { background: #1C2433; border: 1px solid #33445C; color: var(--accent-yellow); padding: 3px 8px; border-radius: 5px; font-size: 0.8rem; font-weight: bold; cursor: pointer; position: relative; }
  .dealer-card-chip:hover { border-color: var(--accent-red); background: #2B1214; color: #FF5252; }
  .dealer-card-chip:hover::after { content: "×"; position: absolute; top: -4px; right: 2px; font-size: 0.7rem; color: #FF5252; }
  
  .dealer-history-list { display: flex; flex-direction: column; gap: 4px; max-height: 80px; overflow-y: auto; }
  .dealer-history-row { background: #0D1117; border: 1px solid #1C2433; border-radius: 6px; padding: 4px 8px; display: flex; justify-content: space-between; align-items: center; font-size: 0.72rem; }
  .dh-round { color: #6F829D; font-weight: bold; }
  .dh-cards { display: flex; gap: 3px; }
  .dh-tag { background: #182232; color: #FFF; padding: 1px 5px; border-radius: 3px; font-size: 0.68rem; }

  /* 🎛️ 輸入模式切換列 */
  .mode-switch-bar { display: grid; grid-template-columns: 1fr 1fr; background: #0D1117; border: 1px solid #242E3E; border-radius: 8px; padding: 3px; margin-bottom: 8px; gap: 4px; }
  .mode-tab { text-align: center; padding: 7px 4px; font-size: 0.75rem; font-weight: bold; color: #6F829D; border-radius: 6px; cursor: pointer; border: 1px solid transparent; }
  .mode-tab.shoe-active { background: #1B2433; color: var(--accent-green); border-color: #009647; }
  .mode-tab.dealer-active { background: #2E200C; color: var(--accent-yellow); border-color: #C67D00; }

  /* 🃏 牌靴軌跡紀錄面板 */
  .history-panel { background: var(--card-bg); border: 1px solid var(--border-color); border-radius: 10px; padding: 8px; margin-bottom: 8px; }
  .history-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px; font-size: 0.75rem; color: #8FA3BF; }
  
  .history-grid-box {
    display: grid;
    grid-template-columns: repeat(10, 1fr);
    gap: 4px;
    background: #0B0E14;
    border: 1px solid #1C2433;
    border-radius: 8px;
    padding: 6px;
    min-height: 50px;
    max-height: 100px;
    overflow-y: auto;
  }
  .history-cell {
    background: #141A24;
    border: 1px solid #242E3E;
    border-radius: 5px;
    height: 28px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 0.78rem;
    font-weight: bold;
    color: var(--accent-yellow);
    cursor: pointer;
    position: relative;
  }
  .history-cell.burn-cell { color: #FF9F0A; border-color: #5C3D18; background: #2B1D0E; }
  .history-cell:hover { border-color: var(--accent-red); background: #2B1214; color: #FF5252; }
  .history-cell:hover::after { content: "×"; position: absolute; top: -4px; right: 2px; font-size: 0.7rem; color: #FF5252; }
  .history-cell.empty { color: #2F3D52; font-size: 0.65rem; cursor: default; }

  /* 🎲 方格網狀記牌面板 */
  .grid-keypad-box { background: var(--card-bg); border: 1px solid var(--border-color); border-radius: 10px; padding: 8px; margin-bottom: 8px; }
  .grid-keypad-title { font-size: 0.72rem; color: #8FA3BF; margin-bottom: 6px; display: flex; justify-content: space-between; align-items: center; }
  .keypad-grid { display: grid; grid-template-columns: repeat(6, 1fr); gap: 4px; margin-bottom: 6px; }
  .btn-grid-card {
    background: #1B2433;
    color: #FFFFFF;
    border: 1px solid #2E3B50;
    border-radius: 8px;
    padding: 8px 1px;
    text-align: center;
    cursor: pointer;
    box-shadow: 0 2px 5px rgba(0,0,0,0.4);
  }
  .btn-grid-card:active { background: #0066FF; transform: scale(0.94); }
  .grid-card-val { font-size: 0.95rem; font-weight: 900; color: var(--accent-yellow); }
  .grid-card-sub { font-size: 0.52rem; color: #8FA3BF; margin-top: 2px; }
  .btn-burn-card { background: #221A11; border-color: #4A331A; }
  .btn-burn-card .grid-card-val { color: #FF9F0A; }

  .btn-actions { display: grid; grid-template-columns: 1fr; gap: 6px; }
  .btn-act { border: none; border-radius: 8px; padding: 10px 4px; font-size: 0.78rem; font-weight: bold; cursor: pointer; text-align: center; }
  .btn-reset { background: #2B1214; color: #FF5252; border: 1px solid #5C2226; }

  /* 📊 策略表頁面專屬樣式 (同畫面切換) */
  #matrixView { display: none; }
  .matrix-section-title { font-size: 0.82rem; font-weight: bold; color: var(--accent-yellow); margin: 10px 0 4px 0; }
  .matrix-table { width: 100%; border-collapse: collapse; text-align: center; background: #0B0E14; border-radius: 6px; overflow: hidden; margin-bottom: 6px; font-size: 0.7rem; }
  .matrix-table th, .matrix-table td { border: 1px solid #1E293B; padding: 4px 2px; }
  .matrix-table th { background: #182232; color: #94A3B8; }
  .m-cell { font-weight: bold; }
  .m-h { color: #00FFAA; background: #013318; } 
  .m-s { color: #FF4D4D; background: #2B0D0D; } 
  .m-d { color: #FFB300; background: #33220B; } 
  .m-p { color: #00D8FF; background: #0A2233; } 
  .m-r { color: #A0ABBA; background: #1C2433; } 
</style>
</head>
<body>

<!-- 🔐 密碼登入驗證層 -->
<div id="loginOverlay">
  <div class="login-card">
    <div class="login-title">🔒 戰情室安全驗證</div>
    <div class="login-desc">請輸入授權密碼以存取量化決策系統</div>
    <input type="password" id="txtPassword" class="login-input" placeholder="••••" onkeydown="if(event.key==='Enter') checkPassword()">
    <button type="button" class="login-btn" onclick="checkPassword()">解鎖進入</button>
    <div id="loginError" style="color:var(--accent-red); font-size:0.75rem; margin-top:8px; font-weight:bold;"></div>
  </div>
</div>

<!-- ♠️ 主程式戰情室 -->
<div id="appContainer">
  <div class="header">
    <div class="title" id="mainTitle">♠️ 21點量化決策 Pro+</div>
    <div class="header-actions">
      <button type="button" id="btnToggleView" class="btn-top-action matrix-btn" onclick="toggleMatrixView()">📊 策略表</button>
      <button type="button" class="btn-top-action" onclick="toggleRules()" id="btnSettingsToggle">⚙️ 設定</button>
    </div>
  </div>

  <!-- 📊 策略表檢視區 (同畫面切換) -->
  <div id="matrixView">
    <div style="font-size: 0.8rem; color: var(--accent-blue); margin-bottom: 6px; font-weight: bold;" id="matrixTcLabel">即時 TC: +0.0</div>
    
    <div class="matrix-section-title">硬點數表 (Hard Totals: 9 ~ 17+)</div>
    <table class="matrix-table" id="tableHard">
      <tr><th>手牌</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th><th>7</th><th>8</th><th>9</th><th>10</th><th>A</th></tr>
    </table>

    <div class="matrix-section-title">軟點數表 (Soft Totals: A,2 ~ A,7)</div>
    <table class="matrix-table" id="tableSoft">
      <tr><th>手牌</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th><th>7</th><th>8</th><th>9</th><th>10</th><th>A</th></tr>
    </table>

    <div class="matrix-section-title">分牌對子表 (Pair Splits)</div>
    <table class="matrix-table" id="tablePair">
      <tr><th>手牌</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th><th>7</th><th>8</th><th>9</th><th>10</th><th>A</th></tr>
    </table>
  </div>

  <!-- 🃏 主記牌操作區 -->
  <div id="mainGameView">
    <div id="ruleSettings" class="rule-settings">
      <div class="rule-item" style="grid-column: span 2; background: #22180A; border: 1px solid #5C3D18;">
        <span style="color:#FFB300; font-weight:bold;">💰 總本金金額 ($):</span>
        <input type="number" id="cfgBankroll" value="10000" step="500" onchange="applyConfig()" style="width: 100px; color:#00FFAA; font-weight:bold;">
      </div>
      <div class="rule-item">
        <span>牌靴副數:</span>
        <select id="cfgDecks" onchange="applyConfig()">
          <option value="8" selected>8 副</option>
          <option value="6">6 副</option>
          <option value="4">4 副</option>
        </select>
      </div>
      <div class="rule-item">
        <span>莊家軟17:</span>
        <select id="cfgDealerSoft17" onchange="applyConfig()">
          <option value="S17" selected>S17 (停牌)</option>
          <option value="H17">H17 (要牌)</option>
        </select>
      </div>
      <div class="rule-item">
        <span>允許投降:</span>
        <select id="cfgSurrender" onchange="applyConfig()">
          <option value="Y" selected>允許投降 (LS)</option>
          <option value="N">不可投降</option>
        </select>
      </div>
      <div class="rule-item">
        <span>分牌後加倍:</span>
        <select id="cfgDAS" onchange="applyConfig()">
          <option value="Y" selected>允許 DAS</option>
          <option value="N">禁止 DAS</option>
        </select>
      </div>
    </div>

    <div class="shoe-bar-wrap">
      <div class="shoe-bar-info">
        <span>牌靴滲透率: <b id="txtPenetration" style="color:#FFF;">0%</b></span>
        <span>已消耗: <b id="txtDealtCards" style="color:#FFF;">0</b> / <span id="txtTotalCards">416</span> 張</span>
      </div>
      <div class="shoe-progress-bg">
        <div id="shoeProgressFill" class="shoe-progress-fill"></div>
      </div>
    </div>

    <div id="signalBox" class="signal-box signal-neutral">
      ⚪ 旁觀等待 (下注 0)
      <span class="signal-sub">真實點數 TC ≤ +1，無數學優勢，純記牌不下注</span>
    </div>

    <!-- 🎯 莊家手牌區塊 -->
    <div class="dealer-panel">
      <div class="dealer-panel-header">
        <div class="dealer-panel-title">
          <span>莊家手牌 (點牌可單張刪除)</span>
          <span id="dealerSumBadge" style="font-size: 0.68rem; color:#8FA3BF; font-weight:normal;">Pts: 0</span>
        </div>
        <div class="dealer-action-group">
          <button type="button" class="btn-dealer-action" onclick="clearCurrentDealer()" title="清除本局莊家牌">✕</button>
          <button type="button" class="btn-dealer-action btn-plus" onclick="nextDealerRound()" title="進入下一局 (＋)">＋</button>
        </div>
      </div>
      <div class="dealer-current-row">
        <div id="dealerCurrentCards" class="dealer-cards-flex">
          <span style="font-size: 0.72rem; color:#4B5768;">切換至「🎯 點擊記莊家牌」後點擊牌面輸入...</span>
        </div>
      </div>
      <div id="dealerHistoryList" class="dealer-history-list">
        <div style="font-size: 0.68rem; color:#4B5768; text-align:center; padding: 2px;">尚無過往莊家局數紀錄</div>
      </div>
    </div>

    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-label">流水數 (RC)</div>
        <div id="valRC" class="stat-value">0</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">真實點數 (TC)</div>
        <div id="valTC" class="stat-value">+0.0</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">即時 EV</div>
        <div id="valEV" class="stat-value" style="color:#00D8FF;">-0.52%</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">建議旁注</div>
        <div id="valBet" class="stat-value" style="color:#8FA3BF;">0</div>
      </div>
    </div>

    <!-- 🎛️ 輸入模式切換列 -->
    <div class="mode-switch-bar">
      <div id="tabShoe" class="mode-tab shoe-active" onclick="switchMode('shoe')">🃏 完整牌靴記牌</div>
      <div id="tabDealer" class="mode-tab" onclick="switchMode('dealer')">🎯 點擊記莊家牌</div>
    </div>

    <!-- 🎲 方格網狀記牌面板 -->
    <div class="grid-keypad-box">
      <div class="grid-keypad-title">
        <span id="keypadTitleText">🃏 快速記牌面板 (牌靴記牌模式)</span>
        <span style="font-size: 0.65rem; color:#00FFAA;" id="keypadSubText">支援 2 ~ A 與 Ø 銷牌</span>
      </div>
      <div class="keypad-grid">
        <div class="btn-grid-card" onclick="handlePadClick(2)"><div class="grid-card-val">2</div><div class="grid-card-sub" id="rem-2">剩 32</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(3)"><div class="grid-card-val">3</div><div class="grid-card-sub" id="rem-3">剩 32</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(4)"><div class="grid-card-val">4</div><div class="grid-card-sub" id="rem-4">剩 32</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(5)"><div class="grid-card-val">5</div><div class="grid-card-sub" id="rem-5">剩 32</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(6)"><div class="grid-card-val">6</div><div class="grid-card-sub" id="rem-6">剩 32</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(7)"><div class="grid-card-val">7</div><div class="grid-card-sub" id="rem-7">剩 32</div></div>
        
        <div class="btn-grid-card" onclick="handlePadClick(8)"><div class="grid-card-val">8</div><div class="grid-card-sub" id="rem-8">剩 32</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(9)"><div class="grid-card-val">9</div><div class="grid-card-sub" id="rem-9">剩 32</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(10)"><div class="grid-card-val">10</div><div class="grid-card-sub" id="rem-10">剩 128</div></div>
        <div class="btn-grid-card" onclick="handlePadClick(1)"><div class="grid-card-val">A</div><div class="grid-card-sub" id="rem-1">剩 32</div></div>
        <div class="btn-grid-card btn-burn-card" onclick="handleBurnClick()"><div class="grid-card-val">Ø</div><div class="grid-card-sub">銷牌</div></div>
      </div>
    </div>

    <!-- 🃏 牌靴軌跡紀錄面板 -->
    <div class="history-panel">
      <div class="history-header">
        <span>📜 牌局記牌軌跡 (點擊任意牌可直接刪除)</span>
      </div>
      <div id="historyGrid" class="history-grid-box">
        <div class="history-cell empty" style="grid-column: span 10; text-align:center;">尚無記錄，請點下方按鈕記牌</div>
      </div>
    </div>

    <div class="btn-actions">
      <button type="button" class="btn-act btn-reset" onclick="resetShoe()">🔄 換靴洗牌</button>
    </div>
  </div>
</div>

<script>
  const CONFIG_PASSWORD = "58821817";

  function checkPassword() {
    let inputVal = document.getElementById("txtPassword").value.trim();
    let errEl = document.getElementById("loginError");
    if (inputVal === CONFIG_PASSWORD) {
      sessionStorage.setItem("bj_auth_passed", "true");
      document.getElementById("loginOverlay").style.display = "none";
      document.getElementById("appContainer").style.display = "block";
      initApp();
    } else {
      errEl.innerText = "❌ 密碼錯誤，請重新輸入";
      document.getElementById("txtPassword").value = "";
      document.getElementById("txtPassword").focus();
    }
  }

  window.addEventListener("DOMContentLoaded", () => {
    if (sessionStorage.getItem("bj_auth_passed") === "true") {
      document.getElementById("loginOverlay").style.display = "none";
      document.getElementById("appContainer").style.display = "block";
      initApp();
    } else {
      let pwdInput = document.getElementById("txtPassword");
      if(pwdInput) pwdInput.focus();
    }
  });

  let config = {
    bankroll: 10000,
    decks: 8,
    dealerSoft17: "S17",
    surrender: "Y",
    das: "Y"
  };

  let historyStack = [];
  let currentMode = 'shoe';
  let currentDealerCards = [];
  let dealerHistoryRounds = [];
  let isMatrixViewOpen = false;

  function initApp() {
    loadSavedState();
    updateUI();
    buildMatrixTables();
  }

  function toggleRules() {
    let el = document.getElementById("ruleSettings");
    el.classList.toggle("open");
  }

  function toggleMatrixView() {
    isMatrixViewOpen = !isMatrixViewOpen;
    let mainView = document.getElementById("mainGameView");
    let matrixView = document.getElementById("matrixView");
    let btnToggle = document.getElementById("btnToggleView");
    let btnSettings = document.getElementById("btnSettingsToggle");
    let titleEl = document.getElementById("mainTitle");

    if (isMatrixViewOpen) {
      mainView.style.display = "none";
      matrixView.style.display = "block";
      btnToggle.innerText = "🎲 記牌主畫面";
      btnSettings.style.display = "none";
      titleEl.innerText = "📊 策略決策表";
      updateMatrixDynamicValues();
    } else {
      mainView.style.display = "block";
      matrixView.style.display = "none";
      btnToggle.innerText = "📊 策略表";
      btnSettings.style.display = "inline-block";
      titleEl.innerText = "♠️ 21點量化決策 Pro+";
    }
  }

  function switchMode(mode) {
    currentMode = mode;
    let tabShoe = document.getElementById("tabShoe");
    let tabDealer = document.getElementById("tabDealer");
    let titleEl = document.getElementById("keypadTitleText");
    let subEl = document.getElementById("keypadSubText");

    if (mode === 'shoe') {
      tabShoe.className = "mode-tab shoe-active";
      tabDealer.className = "mode-tab";
      titleEl.innerText = "🃏 快速記牌面板 (牌靴記牌模式)";
      subEl.innerText = "支援 2 ~ A 與 Ø 銷牌";
      subEl.style.color = "var(--accent-green)";
    } else {
      tabShoe.className = "mode-tab";
      tabDealer.className = "mode-tab dealer-active";
      titleEl.innerText = "🎯 莊家手牌面板 (點擊記錄莊家牌)";
      subEl.innerText = "點擊下方數字同時記入莊家與牌靴";
      subEl.style.color = "var(--accent-yellow)";
    }
  }

  function handlePadClick(val) {
    recordCard(val);
    if (currentMode === 'dealer') {
      currentDealerCards.push({ val: val, label: val === 1 ? "A" : (val === 10 ? "10" : val.toString()) });
      updateDealerDisplay();
    }
  }

  function handleBurnClick() {
    if (currentMode === 'shoe') {
      recordBurnCard();
    } else {
      alert("⚠️ 目前為「莊家手牌」模式，銷牌請先切回「完整牌靴記牌」模式！");
    }
  }

  function recordCard(val) {
    let totalCards = config.decks * 52;
    if (historyStack.length >= totalCards) return;
    let weight = (val >= 2 && val <= 6) ? 1 : ((val >= 7 && val <= 9) ? 0 : -1);
    let label = val === 1 ? "A" : (val === 10 ? "10" : val.toString());
    historyStack.push({ weight: weight, label: label, value: val, isBurn: false });
    saveState();
    updateUI();
  }

  function recordBurnCard() {
    let totalCards = config.decks * 52;
    if (historyStack.length >= totalCards) return;
    historyStack.push({ weight: 0, label: "Ø", isBurn: true, count: 1 });
    saveState();
    updateUI();
  }

  function removeCardAt(index) {
    if (index >= 0 && index < historyStack.length) {
      historyStack.splice(index, 1);
      saveState();
      updateUI();
    }
  }

  function removeDealerCardAt(index) {
    if (index >= 0 && index < currentDealerCards.length) {
      let removedCard = currentDealerCards[index];
      currentDealerCards.splice(index, 1);
      
      for (let i = historyStack.length - 1; i >= 0; i--) {
        if (!historyStack[i].isBurn && historyStack[i].value === removedCard.val) {
          historyStack.splice(i, 1);
          break;
        }
      }

      saveState();
      updateDealerDisplay();
      updateUI();
    }
  }

  function clearCurrentDealer() {
    currentDealerCards = [];
    updateDealerDisplay();
  }

  function nextDealerRound() {
    if (currentDealerCards.length === 0) return;
    let sum = getDealerHandSum(currentDealerCards);
    if (sum < 17) {
      if (!confirm(`⚠️ 目前莊家點數為 ${sum} 點（未滿 17 點）。確定要直接結算並進入下一局嗎？`)) {
        return;
      }
    }
    
    dealerHistoryRounds.unshift({
      roundNum: dealerHistoryRounds.length + 1,
      cards: [...currentDealerCards],
      sum: sum
    });
    if (dealerHistoryRounds.length > 5) dealerHistoryRounds.pop();
    currentDealerCards = [];
    updateDealerDisplay();
    updateUI();
  }

  function getDealerHandSum(cards) {
    let sum = 0;
    let ace = 0;
    for (let c of cards) {
      if (c.val === 1) { ace++; sum += 11; }
      else { sum += c.val; }
    }
    while (sum > 21 && ace > 0) { sum -= 10; ace--; }
    return sum;
  }

  function updateDealerDisplay() {
    let container = document.getElementById("dealerCurrentCards");
    let badge = document.getElementById("dealerSumBadge");
    
    if (currentDealerCards.length === 0) {
      container.innerHTML = '<span style="font-size: 0.72rem; color:#4B5768;">切換至「🎯 點擊記莊家牌」後點擊牌面輸入...</span>';
      badge.innerText = "Pts: 0";
    } else {
      let sum = getDealerHandSum(currentDealerCards);
      badge.innerText = `Pts: ${sum}` + (sum > 21 ? " (爆牌)" : "");
      badge.style.color = sum > 21 ? "var(--accent-red)" : "var(--accent-green)";
      container.innerHTML = currentDealerCards.map((c, idx) => `<div class="dealer-card-chip" onclick="removeDealerCardAt(${idx})" title="點擊刪除此張莊家牌">${c.label}</div>`).join("");
    }

    let historyContainer = document.getElementById("dealerHistoryList");
    if (dealerHistoryRounds.length === 0) {
      historyContainer.innerHTML = '<div style="font-size: 0.68rem; color:#4B5768; text-align:center; padding: 2px;">尚無過往莊家局數紀錄</div>';
    } else {
      historyContainer.innerHTML = dealerHistoryRounds.map(r => `
        <div class="dealer-history-row">
          <span class="dh-round">RND ${String(dealerHistoryRounds.length - r.roundNum + 1).padStart(2, '0')}</span>
          <div class="dh-cards">${r.cards.map(c => `<span class="dh-tag">${c.label}</span>`).join("")}</div>
          <span style="color:${r.sum > 21 ? 'var(--accent-red)' : 'var(--accent-yellow)'}; font-weight:bold;">Pts: ${r.sum}</span>
        </div>
      `).join("");
    }
  }

  function resetShoe() {
    if (confirm("確認換靴洗牌？計數數據將完全清空！")) {
      historyStack = [];
      currentDealerCards = [];
      dealerHistoryRounds = [];
      updateDealerDisplay();
      saveState();
      updateUI();
    }
  }

  // 📊 策略表計算
  function getHardAction(sum, d, tc) {
    if (sum >= 17) return 's';
    if (sum === 16) {
      if (d === 10 && tc >= 0) return 's';
      if (d === 9 && tc >= 4) return 's';
      if (d >= 7) return (config.surrender === 'Y' && d >= 9) ? 'r' : 'h';
      return 's';
    }
    if (sum === 15) {
      if (d === 10 && tc >= 4) return 's';
      if (d >= 10) return (config.surrender === 'Y') ? 'r' : 'h';
      if (d === 9 && tc >= 2 && config.surrender === 'Y') return 'r';
      return (d >= 2 && d <= 6) ? 's' : 'h';
    }
    if (sum === 14) {
      if (d === 10 && tc >= 3 && config.surrender === 'Y') return 'r';
      return (d >= 2 && d <= 6) ? 's' : 'h';
    }
    if (sum === 13) return (d >= 2 && d <= 6) ? 's' : 'h';
    if (sum === 12) {
      if (d === 4 || d === 5 || d === 6) {
        if (d === 4 && tc < 0) return 'h';
        return 's';
      }
      if (d === 3 && tc >= 2) return 's';
      if (d === 2 && tc >= 3) return 's';
      return 'h';
    }
    if (sum === 11) return 'd';
    if (sum === 10) return (d >= 2 && d <= 9) ? 'd' : 'h';
    if (sum === 9) return (d >= 3 && d <= 6) ? 'd' : 'h';
    return 'h';
  }

  function getSoftAction(sum, d) {
    if (sum >= 8) return 's';
    if (sum === 7) return (d >= 3 && d <= 6) ? 'd' : ((d === 2 || d === 7 || d === 8) ? 's' : 'h');
    if (sum === 6) return (d >= 3 && d <= 6) ? 'd' : 'h';
    if (sum === 5 || sum === 4) return (d >= 4 && d <= 6) ? 'd' : 'h';
    return (d === 5 || d === 6) ? 'd' : 'h';
  }

  function getPairAction(p, d) {
    if (p === 1 || p === 8) return 'p';
    if (p === 10) return 's';
    if (p === 9) return (d === 7 || d === 10 || d === 11) ? 's' : 'p';
    if (p === 7) return (d >= 2 && d <= 7) ? 'p' : 'h';
    if (p === 6) return (d >= 3 && d <= 6) ? 'p' : 'h';
    if (p === 5) return (d >= 2 && d <= 9) ? 'd' : 'h';
    if (p === 4) return (d === 5 || d === 6) ? 'p' : 'h';
    if (p === 3 || p === 2) return (d >= 2 && d <= 7) ? 'p' : 'h';
    return 'h';
  }

  function getActionLabel(code) {
    switch(code) {
      case 'h': return '補';
      case 's': return '停';
      case 'd': return '加';
      case 'r': return '投';
      case 'p': return '分';
      default: return code;
    }
  }

  function buildMatrixTables() {
    let dealers = [2, 3, 4, 5, 6, 7, 8, 9, 10, 11];
    
    let hardHtml = `<tr><th>手牌</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th><th>7</th><th>8</th><th>9</th><th>10</th><th>A</th></tr>`;
    for (let sum = 9; sum <= 17; sum++) {
      hardHtml += `<tr id="h-row-${sum}"><td style="background:#182232; font-weight:bold;">${sum === 17 ? "17+" : sum}</td>`;
      for (let d of dealers) {
        hardHtml += `<td class="m-cell" id="h-${sum}-${d}">-</td>`;
      }
      hardHtml += `</tr>`;
    }
    document.getElementById("tableHard").innerHTML = hardHtml;

    let softHtml = `<tr><th>手牌</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th><th>7</th><th>8</th><th>9</th><th>10</th><th>A</th></tr>`;
    for (let s = 2; s <= 7; s++) {
      softHtml += `<tr id="s-row-${s}"><td style="background:#182232; font-weight:bold;">A,${s}</td>`;
      for (let d of dealers) {
        softHtml += `<td class="m-cell" id="s-${s}-${d}">-</td>`;
      }
      softHtml += `</tr>`;
    }
    document.getElementById("tableSoft").innerHTML = softHtml;

    let pairHtml = `<tr><th>手牌</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th><th>7</th><th>8</th><th>9</th><th>10</th><th>A</th></tr>`;
    let pairs = [2, 3, 4, 5, 6, 7, 8, 9, 10, 1];
    let names = {2:"2,2", 3:"3,3", 4:"4,4", 5:"5,5", 6:"6,6", 7:"7,7", 8:"8,8", 9:"9,9", 10:"10,10", 1:"A,A"};
    for (let p of pairs) {
      pairHtml += `<tr id="p-row-${p}"><td style="background:#182232; font-weight:bold;">${names[p]}</td>`;
      for (let d of dealers) {
        pairHtml += `<td class="m-cell" id="p-${p}-${d}">-</td>`;
      }
      pairHtml += `</tr>`;
    }
    document.getElementById("tablePair").innerHTML = pairHtml;
  }

  function updateMatrixDynamicValues() {
    let tc = parseFloat(document.getElementById("valTC").innerText) || 0;
    document.getElementById("matrixTcLabel").innerText = `即時 TC: ${(tc > 0 ? "+" : "") + tc.toFixed(1)}`;
    let dealers = [2, 3, 4, 5, 6, 7, 8, 9, 10, 11];

    for (let sum = 9; sum <= 17; sum++) {
      for (let d of dealers) {
        let code = getHardAction(sum, d, tc);
        let cell = document.getElementById(`h-${sum}-${d}`);
        if(cell) {
          cell.className = `m-cell m-${code}`;
          cell.innerText = getActionLabel(code);
        }
      }
    }

    for (let s = 2; s <= 7; s++) {
      for (let d of dealers) {
        let code = getSoftAction(s, d);
        let cell = document.getElementById(`s-${s}-${d}`);
        if(cell) {
          cell.className = `m-cell m-${code}`;
          cell.innerText = getActionLabel(code);
        }
      }
    }

    let pairs = [2, 3, 4, 5, 6, 7, 8, 9, 10, 1];
    for (let p of pairs) {
      for (let d of dealers) {
        let code = getPairAction(p, d);
        let cell = document.getElementById(`p-${p}-${d}`);
        if(cell) {
          cell.className = `m-cell m-${code}`;
          cell.innerText = getActionLabel(code);
        }
      }
    }
  }

  function applyConfig() {
    let bInput = parseFloat(document.getElementById("cfgBankroll").value);
    config.bankroll = isNaN(bInput) || bInput < 100 ? 10000 : bInput;
    config.decks = parseInt(document.getElementById("cfgDecks").value);
    config.dealerSoft17 = document.getElementById("cfgDealerSoft17").value;
    config.surrender = document.getElementById("cfgSurrender").value;
    config.das = document.getElementById("cfgDAS").value;
    saveState();
    updateUI();
  }

  function saveState() {
    try {
      localStorage.setItem("bj_pro_hist_auth_v32", JSON.stringify(historyStack));
      localStorage.setItem("bj_pro_cfg_auth_v32", JSON.stringify(config));
      localStorage.setItem("bj_pro_dealer_hist_v32", JSON.stringify(dealerHistoryRounds));
    } catch(e) {}
  }

  function loadSavedState() {
    try {
      let savedH = localStorage.getItem("bj_pro_hist_auth_v32");
      let savedC = localStorage.getItem("bj_pro_cfg_auth_v32");
      let savedD = localStorage.getItem("bj_pro_dealer_hist_v32");
      if (savedH) historyStack = JSON.parse(savedH);
      if (savedD) dealerHistoryRounds = JSON.parse(savedD);
      if (savedC) {
        config = Object.assign(config, JSON.parse(savedC));
        document.getElementById("cfgBankroll").value = config.bankroll;
        document.getElementById("cfgDecks").value = config.decks;
        document.getElementById("cfgDealerSoft17").value = config.dealerSoft17;
        document.getElementById("cfgSurrender").value = config.surrender;
        document.getElementById("cfgDAS").value = config.das;
      }
    } catch(e) {}
  }

  function updateUI() {
    let totalCards = config.decks * 52;
    let dealtCards = 0;
    let runningCount = 0;
    let counts = {1:0, 2:0, 3:0, 4:0, 5:0, 6:0, 7:0, 8:0, 9:0, 10:0};
    let totalPerVal = config.decks * 4;

    for (let item of historyStack) {
      if (item.isBurn) {
        dealtCards += item.count;
      } else {
        dealtCards += 1;
        runningCount += item.weight;
        if (item.value) { counts[item.value] = (counts[item.value] || 0) + 1; }
      }
    }

    for (let v = 1; v <= 10; v++) {
      let maxCount = (v === 10) ? (config.decks * 16) : totalPerVal;
      let rem = Math.max(0, maxCount - (counts[v] || 0));
      let el = document.getElementById(`rem-${v}`);
      if (el) el.innerText = `剩 ${rem}`;
    }

    let remCards = Math.max(0, totalCards - dealtCards);
    let remDecks = Math.max(0.5, (remCards / 52.0));
    let trueCount = runningCount / remDecks;

    let penetration = Math.min(100, Math.round((dealtCards / totalCards) * 100));
    document.getElementById("txtPenetration").innerText = `${penetration}%`;
    document.getElementById("txtDealtCards").innerText = dealtCards;
    document.getElementById("txtTotalCards").innerText = totalCards;
    document.getElementById("shoeProgressFill").style.width = `${penetration}%`;

    let rcEl = document.getElementById("valRC");
    rcEl.innerText = (runningCount > 0 ? "+" : "") + runningCount;
    rcEl.style.color = runningCount > 0 ? "var(--accent-green)" : (runningCount < 0 ? "var(--accent-red)" : "#FFF");

    let tcEl = document.getElementById("valTC");
    tcEl.innerText = (trueCount > 0 ? "+" : "") + trueCount.toFixed(1);
    tcEl.style.color = trueCount >= 2 ? "var(--accent-green)" : (trueCount < 0 ? "var(--accent-red)" : "#FFF");

    let liveEV = -0.52 + (trueCount * 0.5);
    let evEl = document.getElementById("valEV");
    evEl.innerText = (liveEV > 0 ? "+" : "") + liveEV.toFixed(2) + "%";
    evEl.style.color = liveEV > 0 ? "var(--accent-green)" : (liveEV < -1 ? "var(--accent-red)" : "#00D8FF");

    let recBet = 0;
    let bankroll = config.bankroll;
    if (trueCount >= 4.0) recBet = Math.floor(bankroll * 0.10);
    else if (trueCount >= 2.0) recBet = Math.floor(bankroll * 0.03);
    else recBet = 0;

    let betEl = document.getElementById("valBet");
    betEl.innerText = recBet.toLocaleString();
    betEl.style.color = recBet > 0 ? "var(--accent-green)" : "#8FA3BF";

    let histContainer = document.getElementById("historyGrid");
    if (historyStack.length === 0) {
      histContainer.innerHTML = '<div class="history-cell empty" style="grid-column: span 10; text-align:center;">尚無記錄，請點下方按鈕記牌</div>';
    } else {
      let html = "";
      for (let i = 0; i < historyStack.length; i++) {
        let item = historyStack[i];
        let cellClass = item.isBurn ? "history-cell burn-cell" : "history-cell";
        html += `<div class="${cellClass}" onclick="removeCardAt(${i})" title="點擊刪除">${item.label}</div>`;
      }
      histContainer.innerHTML = html;
      histContainer.scrollTop = histContainer.scrollHeight;
    }

    let recentRounds = dealerHistoryRounds.slice(0, 4);
    let consecutiveSmallRoundsCount = 0;
    for (let r of recentRounds) {
      let smallCountInRound = r.cards.filter(c => c.val >= 2 && c.val <= 6).length;
      if (r.cards.length >= 2 && smallCountInRound >= 2 && r.sum <= 21) {
        consecutiveSmallRoundsCount++;
      }
    }

    let box = document.getElementById("signalBox");
    let insuranceAlert = trueCount >= 3.0 ? " ⚠️【莊 A 必買保險】" : "";

    if (consecutiveSmallRoundsCount >= 2) {
      box.className = "signal-box signal-red";
      box.innerHTML = `🚨【宏觀風險：近期多局莊家連續補小牌未爆】<span class="signal-sub">牌靴大牌嚴重匱乏，近期連續多局陷阱局，建議：<b>強烈不宜下注 (下注 0)</b>${insuranceAlert}</span>`;
    } else if (dealtCards < 26) {
      box.className = "signal-box signal-neutral";
      box.innerHTML = `⚪ 開局觀察期 (下注 0)<span class="signal-sub">出牌未滿半副，純記牌觀察，絕對不出手${insuranceAlert}</span>`;
    } else if (trueCount >= 2.0) {
      box.className = "signal-box signal-green";
      box.innerHTML = `🟢【優勢翻正：旁注 $${recBet.toLocaleString()}】<span class="signal-sub">TC +${trueCount.toFixed(1)}，已具數學優勢，適度出手。${insuranceAlert}</span>`;
    } else {
      box.className = "signal-box signal-neutral";
      box.innerHTML = `⚪ 旁觀等待 (下注 0)<span class="signal-sub">處於常規波動區間 (TC: ${trueCount.toFixed(1)})，嚴禁出手${insuranceAlert}</span>`;
    }
  }
</script>

</body>
</html>
