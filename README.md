
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.4/dist/chart.umd.min.js"></script>
  <title>FireCalc • Web App</title>
  <style>
    :root{
      --bg:#0a0e13;--panel:#0d1117;--panel-light:#161b22;--muted:#7d8590;--text:#e6edf3;--accent:#58a6ff;--success:#3fb950;--danger:#f85149;--warning:#d29922;--glass: rgba(255,255,255,0.03);--border:rgba(255,255,255,0.08);--shadow:rgba(1,4,9,0.8);
    }
    html,body{height:100%;margin:0;background:linear-gradient(135deg,#0a0e13 0%, #0d1117 50%, #0f1419 100%);font-family:'Inter',-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;color:var(--text);line-height:1.5}
    *{box-sizing:border-box}
    .container{max-width:1200px;margin:0 auto;padding:24px;}
    header{display:flex;justify-content:space-between;align-items:center;margin-bottom:32px;padding-bottom:20px;border-bottom:1px solid var(--border)}
    .header-left h1{font-size:28px;margin:0;font-weight:700;background:linear-gradient(135deg,var(--accent),#7c3aed);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
    .header-left .subtitle{color:var(--muted);font-size:14px;margin-top:4px}
    .header-right{text-align:right;color:var(--muted);font-size:13px;display:flex;align-items:flex-end;gap:12px;flex-direction:column}
    .lang-select{margin-bottom:6px}
    .grid{display:grid;grid-template-columns:1fr 380px;gap:28px;align-items:start}
    .section-card{background:linear-gradient(145deg,var(--panel), var(--panel-light));padding:24px;border-radius:16px;box-shadow:0 8px 32px var(--shadow);border:1px solid var(--border);margin-bottom:20px;position:relative;overflow:hidden}
    .section-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,var(--accent),var(--success),var(--warning));opacity:0.6}
    .section-header{display:flex;align-items:center;gap:12px;margin-bottom:20px}
    .section-icon{width:24px;height:24px;border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:bold;color:white}
    .section-title{font-weight:700;font-size:16px;color:var(--text);margin:0}
    .timeline .section-icon{background:linear-gradient(135deg,#58a6ff,#79c0ff)}
    .spending .section-icon{background:linear-gradient(135deg,#f85149,#ff7b72)}
    .portfolio .section-icon{background:linear-gradient(135deg,#3fb950,#56d364)}
    .pensions .section-icon{background:linear-gradient(135deg,#d29922,#f2cc60)}
    .other-income .section-icon{background:linear-gradient(135deg,#bc8cff,#d2a8ff)}
    .form-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:16px}
    .form-row{display:grid;grid-template-columns:repeat(2,1fr);gap:16px}
    .input-group{display:flex;flex-direction:column;gap:6px}
    label{font-size:13px;color:var(--muted);font-weight:500;letter-spacing:0.02em}
    input[type=number], input[type=text], select{width:100%;padding:10px 12px;border-radius:8px;background:rgba(255,255,255,0.05);border:1px solid var(--border);color:var(--text);font-size:14px;transition:all 0.2s ease;font-family:inherit}
    input[type=number]:focus, input[type=text]:focus, select:focus{outline:none;border-color:var(--accent);box-shadow:0 0 0 3px rgba(88,166,255,0.1);background:rgba(255,255,255,0.08)}
    .input-group:has(input:invalid) label {color: var(--danger)}
    input[type=number]:hover, input[type=text]:hover, select:hover{background:rgba(255,255,255,0.07)}
    input:invalid {border-color: var(--danger)}
    .checkbox-group{display:flex;align-items:center;gap:10px;padding:8px 0}
    input[type=checkbox]{transform:scale(1.2);accent-color:var(--accent)}
    .checkbox-label{margin:0;font-size:14px;color:var(--text)}
    .controls{display:flex;gap:12px;flex-wrap:wrap;margin-top:24px;padding-top:20px;border-top:1px solid var(--border)}
    button{padding:12px 18px;border-radius:10px;border:none;cursor:pointer;font-weight:600;font-size:14px;transition:all 0.2s ease;position:relative;overflow:hidden;font-family:inherit;letter-spacing:0.02em}
    button:hover{transform:translateY(-1px);box-shadow:0 4px 12px rgba(0,0,0,0.3)}
    button:active{transform:translateY(0)}
    .btn-primary{background:linear-gradient(135deg,var(--accent),#79c0ff);color:#0d1117}
    .btn-accent{background:linear-gradient(135deg,#7c3aed,#a855f7);color:white}
    .btn-danger{background:linear-gradient(135deg,#f85149,#ff7b72);color:white}
    .btn-success{background:linear-gradient(135deg,#3fb950,#56d364);color:white}
    .btn-secondary{background:rgba(255,255,255,0.05);border:1px solid var(--border);color:var(--muted)}
    .btn-secondary:hover{background:rgba(255,255,255,0.08);color:var(--text)}
    .sidebar .section-card{padding:20px}
    .status-section .section-icon{background:linear-gradient(135deg,#6b7280,#9ca3af)}
    .results-section .section-icon{background:linear-gradient(135deg,#10b981,#34d399)}
    .status-display{font-size:13px;color:var(--muted);margin-bottom:12px;min-height:18px}
    .progress{height:6px;background:rgba(255,255,255,0.06);border-radius:3px;overflow:hidden;margin-top:8px}
    .progress > span{display:block;height:100%;background:linear-gradient(90deg,var(--success),var(--accent));width:0%;transition:width 0.3s ease}
    .results{display:flex;flex-direction:column;gap:12px}
    .result-item{background:rgba(255,255,255,0.03);padding:12px 16px;border-radius:8px;display:flex;justify-content:space-between;align-items:center;border:1px solid rgba(255,255,255,0.05)}
    .result-label{font-size:13px;color:var(--muted);font-weight:500}
    .result-value{font-weight:600;color:var(--text)}
    .search-result{background:rgba(255,255,255,0.03);padding:16px;border-radius:8px;border:1px solid rgba(255,255,255,0.05);font-size:14px;min-height:48px;display:flex;align-items:center}
    .footer{margin-top:32px;text-align:center;color:var(--muted);font-size:12px;padding:20px;border-top:1px solid var(--border)}
    @media (max-width:1024px){.grid{grid-template-columns:1fr}.container{padding:16px}.form-grid{grid-template-columns:1fr}.form-row{grid-template-columns:1fr}.controls{flex-direction:column}button{width:100%}}
    .section-card{animation:fadeInUp 0.5s ease forwards}
    @keyframes fadeInUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
    .text-success{color:var(--success)}.text-danger{color:var(--danger)}.text-warning{color:var(--warning)}.small{font-size:12px}
    .btn-secondary.small{padding:8px 10px;font-size:13px;border-radius:8px}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="header-left">
        <h1 id="appTitle">FIRE Calculator</h1>
        <div id="headerSubtitle" class="subtitle" data-i18n="headerSubtitle">Monte Carlo Retirement Planning</div>
      </div>
      <div class="header-right">
        <div class="lang-select">
          <label for="langSelect" class="small" data-i18n="langLabel">Language</label>
          <select id="langSelect">
            <option value="en">English</option>
            <option value="it">Italiano</option>
          </select>
        </div>
        <div id="headerInfoTop" data-i18n="headerInfoTop">Dark theme • Client-side simulation</div>
        <div id="headerInfoBottom" data-i18n="headerInfoBottom">All inputs editable</div>
      </div>
    </header>

    <div class="grid">
      <main>
        <!-- Timeline & Simulation -->
        <div class="section-card timeline">
          <div class="section-header">
            <div class="section-icon">⏱</div>
            <h2 class="section-title" data-i18n="timelineTitle">Timeline & Simulation</h2>
          </div>
          <div class="form-grid">
            <div class="input-group">
              <label data-i18n="startYearLabel">Start Year</label>
              <input id="startYear" type="number" value="2025" min="2000" max="2100" required>
            </div>
            <div class="input-group">
              <label data-i18n="endYearLabel">End Retirement Year</label>
              <input id="endYear" type="number" value="2083" min="2000" max="2100" required>
            </div>
            <div class="input-group">
              <label data-i18n="retireFromLabel">Retire From Year</label>
              <input id="retireYear" type="number" value="2030" min="2000" max="2100" required>
            </div>
            <div class="input-group">
              <label data-i18n="numSimsLabel">Number of Simulations</label>
              <input id="numSims" type="number" value="1000" min="1" required>
            </div>
          </div>
        </div>

        <!-- Spending -->
        <div class="section-card spending">
          <div class="section-header">
            <div class="section-icon">💰</div>
            <h2 class="section-title" data-i18n="spendingTitle">Spending</h2>
          </div>
          <div class="form-row">
            <div class="input-group">
              <label data-i18n="currentMonthlySpendingLabel">Current Monthly Spending (€)</label>
              <input id="currentMonthlySpending" type="number" step="50" value="" min="0">
            </div>
            <div class="input-group">
              <label data-i18n="monthlySpendingLabel">Monthly Spending (€)</label>
              <input id="monthlySpending" type="number" step="50" value="4000" min="0" required>
            </div>
          </div>
          <div class="form-row">
            <div class="input-group">
              <label data-i18n="inflationLabel">Inflation Rate (%)</label>
              <input id="inflationRate" type="number" step="0.1" value="2.0" min="0" max="100" required>
            </div>
          </div>
        </div>

        <!-- Portfolio -->
        <div class="section-card portfolio">
          <div class="section-header">
            <div class="section-icon">📈</div>
            <h2 class="section-title" data-i18n="portfolioTitle">Portfolio</h2>
          </div>
          <div class="form-grid">
            <div class="input-group">
              <label data-i18n="initialPortfolioLabel">Initial Portfolio (€)</label>
              <input id="initialPortfolio" type="text" value="800000" min="0" required>
            </div>
            <div class="input-group">
              <label data-i18n="annualInvestmentLabel">Annual Investment (€)</label>
              <input id="annualInvestment" type="text" value="50000" min="0" required>
            </div>
            <div class="input-group">
              <label data-i18n="meanReturnLabel">Mean Return (%)</label>
              <input id="meanReturn" type="number" step="0.1" value="3.0" min="-100" max="100" required>
            </div>
            <div class="input-group">
              <label data-i18n="stdDevLabel">Standard Deviation (%)</label>
              <input id="stdDev" type="number" step="0.1" value="10.00" min="0" max="100" required>
            </div>
          </div>
        </div>

        <!-- Pensions -->
        <div class="section-card pensions">
          <div class="section-header">
           <div class="section-icon">🏛</div>
           <h2 class="section-title" data-i18n="pensionsTitle">Pensions</h2>
         </div>
         <div>
           <div class="form-row">
             <div class="input-group">
              <label data-i18n="aleStartLabel">Pension 1 Start Year</label>
              <input id="alePensionStartYear" type="number" value="2042" min="2000" max="2100" required aria-label="Pension 1 Start Year">
            </div>
            <div class="input-group">
              <label data-i18n="aleMonthlyLabel">Pension 1 Monthly Amount (€)</label>
              <input id="alePensionMonthly" type="number"  step="100" value="2000" min="0" required aria-label="Pension 1 Monthly Amount in Euros">
            </div>
          </div>
          <div class="form-row">
           <div class="input-group">
            <label data-i18n="davStartLabel">Pension 2 Start Year</label>
            <input id="davPensionStartYear" type="number" value="2062" min="2000" max="2100" aria-label="Pension 2 Start Year">
          </div>
          <div class="input-group">
            <label data-i18n="davMonthlyLabel">Pension 2 Monthly Amount (€)</label>
            <input id="davPensionMonthly" type="number" step="100" value="0" min="0" aria-label="Pension 2 Monthly Amount in Euros">
          </div>
        </div>
      </div>
    </div>

    <!-- Other Income -->
    <div class="section-card other-income">
      <div class="section-header">
        <div class="section-icon">💎</div>
        <h2 class="section-title" data-i18n="otherIncomeTitle">Other Income</h2>
      </div>

      <div style="margin-bottom:20px">
        <h3 style="font-size:14px;color:var(--muted);margin:0 0 12px 0;font-weight:600" data-i18n="dividendTitle">Dividend Income</h3>
        <div class="form-row">
          <div class="input-group">
            <label data-i18n="dividendStartLabel">Start Year</label>
            <input id="dividendStartYear" type="number" value="2031" min="2000" max="2100">
          </div>
          <div class="input-group">
            <label data-i18n="dividendMonthlyLabel">Monthly Amount (€)</label>
            <input id="dividendMonthly" type="number" step="100" value="1000" min="0">
          </div>
        </div>
        <div class="input-group" style="margin-top:12px">
          <label data-i18n="dividendGrowthLabel">Growth Rate (%)</label>
          <input id="dividendGrowthRate" type="number" step="0.1" value="2.0" min="0" max="100">
        </div>
      </div>

      <div>
        <h3 style="font-size:14px;color:var(--muted);margin:0 0 12px 0;font-weight:600" data-i18n="rentalTitle">Rental Income</h3>
        <div class="form-row">
          <div class="input-group">
            <label data-i18n="rentStartLabel">Start Year</label>
            <input id="rentStartYear" type="number" value="2031" min="2000" max="2100">
          </div>
          <div class="input-group">
            <label data-i18n="rentMonthlyLabel">Monthly Amount (€)</label>
            <input id="rentMonthly" type="number" step="50" value="900" min="0">
          </div>
        </div>
        <div class="input-group" style="margin-top:12px">
          <label data-i18n="rentGrowthLabel">Growth Rate (%)</label>
          <input id="rentGrowthRate" type="number" step="0.1" value="2.0" min="0" max="100">
        </div>
      </div>
    </div>

    <!-- One-off items -->
    <div class="section-card other-income">
      <div class="section-header">
        <div class="section-icon">🧾</div>
        <h2 class="section-title" data-i18n="oneoffTitle">One-off item (optional)</h2>
      </div>
      <div id="oneoffHelper" style="margin-bottom:10px;color:var(--muted);font-size:13px" data-i18n="oneoffHelper">Use these to model one-off incomes or expenses (positive = income, negative = expense). Example: buy house -200000 in 2040, or sell asset +300000 in 2035.</div>

      <div class="form-grid">
        <div class="input-group">
          <label data-i18n="itemAmountLabel1">Item 1 Amount (€)</label>
          <input id="oneoff1Amount" type="number" placeholder="e.g. -200000" data-i18n-placeholder="oneoff1Placeholder">
        </div>
        <div class="input-group">
          <label data-i18n="itemYearLabel1">Item 1 Year</label>
          <input id="oneoff1Year" type="number" placeholder="e.g. 2040" data-i18n-placeholder="oneoff1YearPlaceholder">
        </div>

        <div class="input-group">
          <label data-i18n="itemAmountLabel2">Item 2 Amount (€)</label>
          <input id="oneoff2Amount" type="number" placeholder="e.g. 300000" data-i18n-placeholder="oneoff2Placeholder">
        </div>
        <div class="input-group">
          <label data-i18n="itemYearLabel2">Item 2 Year</label>
          <input id="oneoff2Year" type="number" placeholder="e.g. 2035" data-i18n-placeholder="oneoff2YearPlaceholder">
        </div>

        <div class="input-group">
          <label data-i18n="itemAmountLabel3">Item 3 Amount (€)</label>
          <input id="oneoff3Amount" type="number" placeholder="e.g. -50000" data-i18n-placeholder="oneoff3Placeholder">
        </div>
        <div class="input-group">
          <label data-i18n="itemYearLabel3">Item 3 Year</label>
          <input id="oneoff3Year" type="number" placeholder="e.g. 2029" data-i18n-placeholder="oneoff3YearPlaceholder">
        </div>
      </div>
    </div>

    <!-- Controls -->
    <div class="section-card">
      <div class="checkbox-group">
        <input id="fastSearch" type="checkbox" checked>
        <label class="checkbox-label" data-i18n="fastSearchLabel">Fast search (fewer simulations during optimization)</label>
      </div>
      <div class="input-group" style="margin-top:12px;max-width:200px">
        <label data-i18n="targetLabel">Target Success Rate (%)</label>
        <input id="targetPct" type="number" value="90" min="0" max="100" required>
      </div>

      <div class="controls">
        <button id="btnRun" class="btn-primary" data-i18n="btnRun">🎯 Run Monte Carlo</button>
        <button id="btnFindYear" class="btn-accent" data-i18n="btnFindYear">📅 Find Earliest Retirement</button>
        <button id="btnMaxSpend" class="btn-danger" data-i18n="btnMaxSpend">💸 Max Spending</button>
        <button id="btnPortfolioNeeded" class="btn-success" data-i18n="btnPortfolioNeeded">💰 Portfolio Needed</button>
        <button id="btnIncomeNeeded" class="btn-success" data-i18n="btnIncomeNeeded">💵 Income Needed</button>
        <button id="btnReset" class="btn-secondary" data-i18n="btnReset">↻ Reset Inputs</button>
        <button id="btnSave" class="btn-secondary" data-i18n="btnSave">💾 Save</button>
        <button id="btnLoad" class="btn-secondary" data-i18n="btnLoad">📂 Load</button>
      </div>
    </div>
  </main>

  <aside class="sidebar">
    <!-- Status -->
    <div class="section-card status-section" id="outputArea">
      <div class="section-header">
        <div class="section-icon">ℹ</div>
        <h3 class="section-title" data-i18n="statusTitle">Status</h3>
      </div>
      <div id="statusLog" class="status-display">Ready</div>
      <div class="progress"><span id="progressBar"></span></div>
    </div>

    <!-- Results -->
    <div class="section-card results-section">
      <div class="section-header">
        <div class="section-icon">📊</div>
        <h3 class="section-title" data-i18n="resultsTitle">Monte Carlo Results</h3>
      </div>
      <div class="results">
        <div class="result-item">
          <div class="result-label" data-i18n="resLabelSuccess">Success Rate</div>
          <div class="result-value" id="resSuccess">—</div>
        </div>
        <div class="result-item">
          <div class="result-label" data-i18n="resLabelAvg">Average End Portfolio</div>
          <div class="result-value" id="resAvg">—</div>
        </div>
        <div class="result-item">
          <div class="result-label" data-i18n="resLabelMed">Median End Portfolio</div>
          <div class="result-value" id="resMed">—</div>
        </div>
        <div class="result-item">
          <div class="result-label" data-i18n="resLabelP10">10th Percentile</div>
          <div class="result-value" id="resP10">—</div>
        </div>
      </div>
    </div>

    <!-- Optimization Results -->
    <div class="section-card">
      <div class="section-header">
        <div class="section-icon">🎯</div>
        <h3 class="section-title" data-i18n="optimizationTitle">Optimization</h3>
      </div>

      <div style="margin-bottom:16px">
        <div style="font-size:13px;color:var(--muted);margin-bottom:8px;font-weight:500" data-i18n="earliestLabel">Earliest Retirement</div>
        <div class="search-result" id="earliestResult" data-i18n="earliestPlaceholder">Click "Find Earliest Retirement"</div>
      </div>

      <div style="margin-bottom:16px">
        <div style="font-size:13px;color:var(--muted);margin-bottom:8px;font-weight:500" data-i18n="maxSpendLabel">Max Spending</div>
        <div class="search-result" id="maxSpendResult" data-i18n="maxSpendPlaceholder">Click "Max Spending"</div>
      </div>

      <div style="margin-bottom:12px">
        <div style="font-size:13px;color:var(--muted);margin-bottom:8px;font-weight:500" data-i18n="portfolioNeededLabel">Portfolio Needed</div>
        <div class="search-result" id="portfolioNeededResult" data-i18n="portfolioNeededPlaceholder">Click "Portfolio Needed"</div>
      </div>

      <div>
        <div style="font-size:13px;color:var(--muted);margin-bottom:8px;font-weight:500" data-i18n="incomeNeededLabel">Income Needed</div>
        <div class="search-result" id="incomeNeededResult" data-i18n="incomeNeededPlaceholder">Click "Income Needed"</div>
      </div>
    </div>

    <!-- Saved simulations (3 slots) -->
    <div class="section-card">
      <div class="section-header">
        <div class="section-icon">💾</div>
        <h3 class="section-title" data-i18n="savedSimsTitle">Saved Simulations</h3>
      </div>
      <div style="display:flex;flex-direction:column;gap:10px;">
        <div style="display:flex;gap:8px;align-items:center;">
          <div id="savedSlotLabel1" style="flex:1;color:var(--muted);font-size:14px;">(empty)</div>
          <button id="btnLoadSlot1" class="btn-secondary small">📂 Load</button>
          <button id="btnDownloadSlot1" class="btn-secondary small">⬇️ Download</button>
        </div>
        <div style="display:flex;gap:8px;align-items:center;">
          <div id="savedSlotLabel2" style="flex:1;color:var(--muted);font-size:14px;">(empty)</div>
          <button id="btnLoadSlot2" class="btn-secondary small">📂 Load</button>
          <button id="btnDownloadSlot2" class="btn-secondary small">⬇️ Download</button>
        </div>
        <div style="display:flex;gap:8px;align-items:center;">
          <div id="savedSlotLabel3" style="flex:1;color:var(--muted);font-size:14px;">(empty)</div>
          <button id="btnLoadSlot3" class="btn-secondary small">📂 Load</button>
          <button id="btnDownloadSlot3" class="btn-secondary small">⬇️ Download</button>
        </div>
        <div style="display:flex;gap:8px;margin-top:8px;">
          <div style="flex:1"></div>
          <button id="btnClearSlots" class="btn-secondary small">🗑 Clear</button>
        </div>
      </div>
    </div>

  </aside>
</div>
<!-- Portfolio Evolution Chart Section -->
<div class="section-card results-section" id="chartSection" style="display:none; margin-top:32px;">
  <div class="section-header">
    <div class="section-icon">📈</div>
    <h3 class="section-title">Andamento simulato del portafoglio (1000 Monte Carlo)</h3>
  </div>
  <div style="margin:16px 0; font-size:13px; color:var(--muted);">
    Linee rappresentano i percentili delle simulazioni:<br>
    • <span style="color:#58a6ff">Mediana (50°)</span> — scenario più probabile<br>
    • <span style="color:#f85149">10° percentile</span> — scenario conservativo (90% meglio di questo)<br>
    • <span style="color:#3fb950">90° percentile</span> — scenario ottimistico
  </div>
  <canvas id="portfolioChart" style="max-height:400px; width:100%;"></canvas>
</div>
<div class="footer" data-i18n="footerText">
  Client-side Monte Carlo simulation — For illustrative purposes only. Validate assumptions before making financial decisions.
</div>

</div>

<script>
    // ---- i18n strings (added a few keys for new UI)----
  const I18N = {
    en: {
      headerSubtitle: 'Monte Carlo Retirement Planning',
      headerInfoTop: 'Dark theme • Client-side simulation',
      headerInfoBottom: 'All inputs editable',
      langLabel: 'Language',
      timelineTitle: 'Timeline & Simulation',
      startYearLabel: 'Start Year',
      endYearLabel: 'End Retirement Year',
      retireFromLabel: 'Retire From Year',
      numSimsLabel: 'Number of Simulations',
      spendingTitle: 'Spending',
      currentMonthlySpendingLabel: 'Current Monthly Spending (€)',
      monthlySpendingLabel: 'Future Monthly Spending(€)',
      inflationLabel: 'Inflation Rate (%)',
      portfolioTitle: 'Portfolio',
      initialPortfolioLabel: 'Initial Portfolio (€)',
      annualInvestmentLabel: 'Annual Investment (€)',
      meanReturnLabel: 'Mean Return (%)',
      stdDevLabel: 'Standard Deviation (%)',
      pensionsTitle: 'Pensions (no infl adj)',
      aleStartLabel: 'Pension1 Start Year',
      aleMonthlyLabel: 'Pension1 Monthly Amount (€)',
      davStartLabel: 'Pension2 Start Year',
      davMonthlyLabel: 'Pension2 Monthly Amount (€)',
      otherIncomeTitle: 'Other Income (optional)',
      dividendTitle: 'Dividend Income',
      dividendStartLabel: 'Start Year',
      dividendMonthlyLabel: 'Monthly Amount (€)',
      dividendGrowthLabel: 'Growth Rate (%)',
      rentalTitle: 'Rental Income',
      rentStartLabel: 'Start Year',
      rentMonthlyLabel: 'Monthly Amount (€)',
      rentGrowthLabel: 'Growth Rate (%)',
      oneoffTitle: 'One-off item (optional)',
      oneoffHelper: 'Use these to model one-off incomes or expenses (positive = income, negative = expense). Example: buy house -200000 in 2040, or sell asset +300000 in 2035.',
      itemAmountLabel1: 'Item 1 Amount (€)',
      itemYearLabel1: 'Item 1 Year',
      itemAmountLabel2: 'Item 2 Amount (€)',
      itemYearLabel2: 'Item 2 Year',
      itemAmountLabel3: 'Item 3 Amount (€)',
      itemYearLabel3: 'Item 3 Year',
      oneoff1Placeholder: 'e.g. -200000',
      oneoff1YearPlaceholder: 'e.g. 2040',
      oneoff2Placeholder: 'e.g. 300000',
      oneoff2YearPlaceholder: 'e.g. 2035',
      oneoff3Placeholder: 'e.g. -50000',
      oneoff3YearPlaceholder: 'e.g. 2029',
      fastSearchLabel: 'Fast search (fewer simulations during optimization)',
      targetLabel: 'Target Success Rate (%)',
      btnRun: '🎯 Run Monte Carlo',
      btnFindYear: '📅 Find Earliest Retirement',
      btnMaxSpend: '💸 Max Spending',
      btnPortfolioNeeded: '💰 Portfolio Needed',
      btnIncomeNeeded: '💵 Income Needed',
      btnReset: '↻ Reset Inputs',
      btnSave: '💾 Save',
      btnLoad: '📂 Load',
      statusTitle: 'Status',
      resultsTitle: 'Monte Carlo Results',
      resLabelSuccess: 'Success Rate',
      resLabelAvg: 'Average End Portfolio',
      resLabelMed: 'Median End Portfolio',
      resLabelP10: '10th Percentile',
      optimizationTitle: 'Optimization',
      earliestLabel: 'Earliest Retirement',
      earliestPlaceholder: 'Click "Find Earliest Retirement"',
      maxSpendLabel: 'Max Spending',
      maxSpendPlaceholder: 'Click "Max Spending"',
      portfolioNeededLabel: 'Additional Portfolio Needed',
      portfolioNeededPlaceholder: 'Click "Portfolio Needed"',
      incomeNeededLabel: 'Income Needed',
      incomeNeededPlaceholder: 'Click "Income Needed"',
      footerText: 'Client-side Monte Carlo simulation — For illustrative purposes only. Validate assumptions before making financial decisions.',
      monthlyLabel: 'Monthly',
      savedSimsTitle: 'Saved Simulations',
      savedSlotEmpty: '(empty)',
      btnLoadSlot: 'Load',
      btnDownloadSlot: 'Download',

        // runtime messages
      ready: 'Ready',
      runningMonteCarlo: 'Running Monte Carlo...',
      searchingYear: 'Searching earliest retirement year...',
      searchingMaxSpend: 'Searching max spending...',
      searchingPortfolio: 'Searching required portfolio...',
      searchingIncome: 'Searching required income...',
      testingRetireYear: 'Testing retire year {year} ...',
      testingPortfolio: 'Testing portfolio €{amt} ...',
      testingIncome: 'Testing income €{amt} ...',
      foundYear: 'Earliest year: {year} — Success: {pct}%',
      noFeasibleYear: 'No retirement year with target success found in horizon.',
      doneSuccess: 'Done — success {pct}%',
      errorDuring: 'Error: {msg}',
      noSolutionZero: 'Even €0/year fails to reach {target}% ({pct}%)',
      maxYearlyReachedZero: 'Max yearly spending: €0 (target reached)',
      portfolioSufficient: 'You don\'t need additional portfolio for {year} — Current: €{current}',
      portfolioNeededResult: 'Additional portfolio needed for {year}: €{amt} (Success: {pct}%)',
      noPortfolioSolution: 'No portfolio amount reaches {target}% success for {year} ({pct}%)',
      incomeNeededResult: 'Income needed for {year}: €{amt}/yr (≈ €{m}/mo) — Success: {pct}%',
      noIncomeSolution: 'No recurring income amount found to reach {target}% for {year} ({pct}%)',
      savingState: 'Saving state...',
      savedState: 'State saved to localStorage.',
      loadingState: 'Loading state...',
      loadedState: 'State loaded from localStorage.',
      done: 'Done',
      profileLoaded: 'Profile loaded'
    },
    it: {
      headerSubtitle: 'Pianificazione pensionistica – Monte Carlo',
      headerInfoTop: 'Tema scuro • Simulazione lato client',
      headerInfoBottom: 'Tutti i campi modificabili',
      langLabel: 'Lingua',
      timelineTitle: 'Timeline e Simulazione',
      startYearLabel: 'Anno di inizio',
      endYearLabel: 'Anno fine simulazione',
      retireFromLabel: 'Anno pensionamento',
      numSimsLabel: 'Numero di simulazioni',
      spendingTitle: 'Spese',
      currentMonthlySpendingLabel: 'Spesa Mensile Attuale (€)',
      monthlySpendingLabel: 'Spesa mensile Futura(€)',
      inflationLabel: 'Tasso di inflazione (%)',
      portfolioTitle: 'Portafoglio',
      initialPortfolioLabel: 'Portafoglio iniziale (€)',
      annualInvestmentLabel: 'Investimento annuo (€)',
      meanReturnLabel: 'Rendimento medio (%)',
      stdDevLabel: 'Deviazione standard (%)',
      pensionsTitle: 'Pensioni',
      aleStartLabel: 'Anno inizio pensione1',
      aleMonthlyLabel: 'Importo mensile pensione1 (€)',
      davStartLabel: 'Anno inizio pensione2',
      davMonthlyLabel: 'Importo mensile pensione2 (€)',
      otherIncomeTitle: 'Altri Redditi (opzionale)',
      dividendTitle: 'Redditi da dividendi',
      dividendStartLabel: 'Anno di inizio',
      dividendMonthlyLabel: 'Importo mensile (€)',
      dividendGrowthLabel: 'Tasso di crescita (%)',
      rentalTitle: 'Redditi da affitti',
      rentStartLabel: 'Anno di inizio',
      rentMonthlyLabel: 'Importo mensile (€)',
      rentGrowthLabel: 'Tasso di crescita (%)',
      oneoffTitle: 'Voce una tantum (opzionale)',
      oneoffHelper: 'Usa queste voci per modellare entrate o spese una tantum (positivo = entrata, negativo = spesa). Esempio: compra casa -200000 nel 2040, o vendi un asset +300000 nel 2035.',
      itemAmountLabel: 'Importo voce (€)',
      itemYearLabel: 'Anno voce',
      oneoff1Placeholder: 'es. -200000',
      oneoff1YearPlaceholder: 'es. 2040',
      oneoff2Placeholder: 'es. 300000',
      oneoff2YearPlaceholder: 'es. 2035',
      oneoff3Placeholder: 'es. -50000',
      oneoff3YearPlaceholder: 'es. 2029',
      fastSearchLabel: 'Ricerca rapida (meno simulazioni durante l\'ottimizzazione)',
      targetLabel: 'Obiettivo di successo (%)',
      btnRun: '🎯 Esegui Monte Carlo',
      btnFindYear: '📅 Trova il primo anno possibile',
      btnMaxSpend: '💸 Spesa massima',
      btnPortfolioNeeded: '💰 Portafoglio necessario',
      btnIncomeNeeded: '💵 Reddito necessario',
      btnReset: '↻ Reimposta campi',
      btnSave: '💾 Salva',
      btnLoad: '📂 Carica',
      statusTitle: 'Stato',
      resultsTitle: 'Risultati Monte Carlo',
      resLabelSuccess: 'Tasso di successo',
      resLabelAvg: 'Portafoglio medio finale',
      resLabelMed: 'Portafoglio mediano finale',
      resLabelP10: '10° percentile',
      optimizationTitle: 'Ottimizzazione',
      earliestLabel: 'Primo anno possibile',
      earliestPlaceholder: 'Clicca "Trova il primo anno possibile"',
      maxSpendLabel: 'Spesa massima',
      maxSpendPlaceholder: 'Clicca "Spesa massima"',
      portfolioNeededLabel: 'Portafoglio aggiuntivo necessario',
      portfolioNeededPlaceholder: 'Clicca "Portafoglio necessario"',
      incomeNeededLabel: 'Reddito necessario',
      incomeNeededPlaceholder: 'Clicca "Reddito necessario"',
      footerText: 'Simulazione Monte Carlo lato client — Solo a scopo illustrativo. Verifica le ipotesi prima di prendere decisioni finanziarie.',
      monthlyLabel: 'Mensile',
      savedSimsTitle: 'Simulazioni salvate',
      savedSlotEmpty: '(vuoto)',
      btnLoadSlot: 'Carica',
      btnDownloadSlot: 'Scarica',

        // runtime messages
      ready: 'Pronto',
      runningMonteCarlo: 'Esecuzione Monte Carlo...',
      searchingYear: 'Ricerca del primo anno possibile...',
      searchingMaxSpend: 'Ricerca spesa massima...',
      searchingPortfolio: 'Ricerca portafoglio necessario...',
      searchingIncome: 'Ricerca reddito necessario...',
      testingRetireYear: 'Test anno pensionamento {year} ...',
      testingPortfolio: 'Test portafoglio €{amt} ...',
      testingIncome: 'Test reddito €{amt} ...',
      foundYear: 'Primo anno: {year} — Successo: {pct}%',
      noFeasibleYear: 'Nessun anno con il target di successo trovato nell\'orizzonte.',
      doneSuccess: 'Fatto — successo {pct}%',
      errorDuring: 'Errore: {msg}',
      noSolutionZero: 'Anche €0/anno non raggiunge {target}% ({pct}%)',
      maxYearlyReachedZero: 'Spesa annua massima: €0 (target raggiunto)',
      portfolioSufficient: 'Non hai bisogno di un portafoglio aggiuntivo per il {year} — Attuale: €{current}',
      portfolioNeededResult: 'Portafoglio aggiuntivo necessario per il {year}: €{amt} (Successo: {pct}%)',
      noPortfolioSolution: 'Nessun importo di portafoglio raggiunge il {target}% di successo per il {year} ({pct}%)',
      incomeNeededResult: 'Reddito necessario per il {year}: €{amt}/anno (≈ €{m}/mese) — Successo: {pct}%',
      noIncomeSolution: 'Nessun importo di reddito ricorrente raggiunge il {target}% per il {year} ({pct}%)',
      savingState: 'Salvataggio stato...',
      savedState: 'Stato salvato in localStorage.',
      loadingState: 'Caricamento stato...',
      loadedState: 'Stato caricato da localStorage.',
      done: 'Fatto',
      profileLoaded: 'Profilo caricato'
    }
  };

    // current language
  let CUR_LANG = 'en';

    // simple formatter for translations with {var}
  function t(key, vars){
    const dict = (I18N[CUR_LANG] && I18N[CUR_LANG][key]) ? I18N[CUR_LANG][key] : (I18N['en'][key] || key);
    if(!vars) return dict;
    return String(dict).replace(/\{(.*?)\}/g, function(_, k){ return (vars[k]===undefined)? '' : vars[k]; });
  }

    // Apply translations to elements marked with data-i18n / data-i18n-placeholder
  function applyTranslations(){
    document.documentElement.lang = CUR_LANG === 'it' ? 'it' : 'en';
    document.querySelectorAll('[data-i18n]').forEach(function(el){
      const key = el.getAttribute('data-i18n');
      if(key && I18N[CUR_LANG] && I18N[CUR_LANG][key] !== undefined){ el.textContent = I18N[CUR_LANG][key]; }
    });
    document.querySelectorAll('[data-i18n-placeholder]').forEach(function(el){
      const key = el.getAttribute('data-i18n-placeholder');
      if(key && I18N[CUR_LANG] && I18N[CUR_LANG][key] !== undefined){ el.setAttribute('placeholder', I18N[CUR_LANG][key]); }
    });
      // update dynamic status and placeholders
    setStatus('ready');
      // update initial search-result texts
    const er = document.getElementById('earliestResult'); if(er) er.textContent = I18N[CUR_LANG].earliestPlaceholder;
    const ms = document.getElementById('maxSpendResult'); if(ms) ms.textContent = I18N[CUR_LANG].maxSpendPlaceholder;
    const pn = document.getElementById('portfolioNeededResult'); if(pn) pn.textContent = I18N[CUR_LANG].portfolioNeededPlaceholder;
    const inr = document.getElementById('incomeNeededResult'); if(inr) inr.textContent = I18N[CUR_LANG].incomeNeededPlaceholder;
    updateSavedSlotsUI();
  }

    // ---- Helpers ----
  function getVal(id, parse = true) {
    var el = document.getElementById(id);
    if (!el) return parse ? NaN : '';
      var v = el.value.replace(/,/g, ''); // remove commas
      return parse ? parseFloat(v) : el.value; // return original string if not parse
    }
    function setText(id, txt) { var el = document.getElementById(id); if(el) el.textContent = txt; }
    function fmtMoney(v){ if(typeof v !== 'number' || isNaN(v)) return '—'; if(Math.abs(v) >= 1e6) return '€' + (v/1e6).toFixed(2) + 'M'; if(Math.abs(v) >= 1e3) return '€' + Math.round(v).toLocaleString(); return '€' + v.toFixed(2); }

    // Box-Muller normal(0,1)
    function randNormal(){
      var u=0,v=0; while(u===0)u=Math.random(); while(v===0)v=Math.random(); return Math.sqrt(-2*Math.log(u))*Math.cos(2*Math.PI*v);
    }

    function average(arr){ if(!arr || !arr.length) return 0; return arr.reduce((a,b)=>a+b,0)/arr.length; }
    function median(arr){ if(!arr || !arr.length) return 0; const s=[...arr].sort((a,b)=>a-b); const m=Math.floor(s.length/2); return s.length%2? s[m] : (s[m-1]+s[m])/2; }
    function percentile(arr,p){ if(!arr || !arr.length) return 0; const s=[...arr].sort((a,b)=>a-b); const idx=Math.ceil(p*s.length)-1; return s[Math.max(0,Math.min(idx,s.length-1))]; }

    // Monte Carlo core (returns end portfolios and summary)
    // NOTE: added support for 'extraAnnualIncome' (annual amount added from retireYear onward)

    function monteCarlo(params){
      if(!params || typeof params !== 'object') throw new Error('Invalid params');
      var numSims = Math.max(1, Math.round(params.numSims || 0));

      const {
        initialPortfolio = 0, annualInvestment = 0, annualSpending = 0, inflationRate = 0.02,
        dividendGrowthRate = 0.02, rentGrowthRate = 0.02, meanReturn = 0.06, stdDev = 0.1,
        startYear = 2025, endYear = 2083, dividendStartYear = 2028, dividendMonthly = 0,
        rentStartYear = 2028, rentMonthly = 0, alePensionStartYear = 2040, alePensionMonthly = 0,
        davPensionStartYear = 2058, davPensionMonthly = 0, retireYear = 2028, oneOffs = [],
        extraAnnualIncome = 0
      } = params;

      const years = endYear - startYear + 1;
      if (years <= 0) throw new Error('Invalid endYear/startYear');

      const initialYearlySpending = Number(annualSpending) || 0;
      const initialDividend = (Number(dividendMonthly)||0)*12;
      const initialRent = (Number(rentMonthly)||0)*12;
      const initialAle = (Number(alePensionMonthly)||0)*12;
      const initialDav = (Number(davPensionMonthly)||0)*12;

      const spending = new Array(years).fill(0);
      const dividends = new Array(years).fill(0);
      const rent = new Array(years).fill(0);
      const ale = new Array(years).fill(0);
      const dav = new Array(years).fill(0);
      const oneOffAmounts = new Array(years).fill(0);

      let cs = initialYearlySpending;
      let cd = initialDividend;
      let cr = initialRent;
      let ca = initialAle;
      let cdav = initialDav;

      // Map one-off entries
      if(Array.isArray(oneOffs)){
        oneOffs.forEach(function(entry){
          var yr = Number(entry.year);
          var amt = Number(entry.amount) || 0;
          var idx = yr - startYear;
          if(!isNaN(idx) && idx >= 0 && idx < years){ oneOffAmounts[idx] += amt; }
        });
      }

      // Pre-calcola flussi ricorrenti anno per anno
      for(let y=0; y<years; y++){
        const cy = startYear + y;
        if(cy >= retireYear){ spending[y]=cs; cs *= (1+inflationRate); }
        if(cy >= dividendStartYear){ dividends[y]=cd; cd *= (1+dividendGrowthRate); }
        if(cy >= rentStartYear){ rent[y]=cr; cr *= (1+rentGrowthRate); }
        if(cy >= alePensionStartYear){ ale[y]=ca; ca *= (1+inflationRate); }
        if(cy >= davPensionStartYear){ dav[y]=cdav; cdav *= (1+inflationRate); }
      }

      const endPortfolios = [];
      let successCount = 0;

      // Array per memorizzare tutti i percorsi (simulazione × anno)
      const allPaths = []; // allPaths[sim][yearIndex] = portfolio value

      for(let sim=0; sim<numSims; sim++){
        let portfolio = Number(initialPortfolio) || 0;
        let depleted = false;

        const path = [portfolio]; // anno iniziale (prima di startYear)

        for(let y=0; y<years; y++){
          const cy = startYear + y;

          // Accumulazione pre-ritiro
          if(cy < retireYear) portfolio += Number(annualInvestment) || 0;

          // Rendimento casuale
          const z = randNormal();
          const ret = (Number(meanReturn) || 0) + (Number(stdDev) || 0) * z;
          portfolio *= (1 + ret);

          // Aggiungi redditi ricorrenti
          portfolio += (dividends[y]||0) + (rent[y]||0) + (ale[y]||0) + (dav[y]||0);

          // One-off
          portfolio += (oneOffAmounts[y]||0);

          // Extra recurring income (usato da IncomeNeeded)
          if(cy >= retireYear && Number(extraAnnualIncome)) portfolio += Number(extraAnnualIncome);

          // Prelievo spesa
          if(cy >= retireYear) portfolio -= (spending[y]||0);

          // Deplezione
          if(portfolio < 0){
            portfolio = 0;
            depleted = true;
          }

          path.push(portfolio);
        }

        allPaths.push(path);
        endPortfolios.push(path[path.length-1]);
        if(!depleted) successCount++;
      }

      // Calcolo percentili per ogni anno
      const numYears = years + 1; // +1 perché includiamo valore iniziale
      const medianPath = new Array(numYears);
      const p10Path = new Array(numYears);
      const p90Path = new Array(numYears);
      const yearsArray = new Array(numYears);

      for(let y=0; y<numYears; y++){
        const valuesAtYear = allPaths.map(p => p[y]);
        valuesAtYear.sort((a,b) => a - b);

        const midIdx = Math.floor(valuesAtYear.length / 2);
        medianPath[y] = valuesAtYear.length % 2 ?
        valuesAtYear[midIdx] :
        (valuesAtYear[midIdx-1] + valuesAtYear[midIdx]) / 2;

        const p10Idx = Math.floor(0.10 * valuesAtYear.length);
        p10Path[y] = valuesAtYear[p10Idx];

        const p90Idx = Math.floor(0.90 * valuesAtYear.length);
        p90Path[y] = valuesAtYear[p90Idx];

        // Anno corretto: y=0 → startYear, y=1 → startYear+1, ..., y=years → endYear
        yearsArray[y] = startYear + (y > 0 ? y - 1 : 0) + (y > 0 ? 1 : 0);
        // Correzione più semplice:
        yearsArray[y] = startYear + y - (y === 0 ? 0 : 0); // y=0 → startYear, y=1 → startYear+1, ecc.
      }
      // Fix esplicito per chiarezza
      for(let y=0; y<numYears; y++){
        yearsArray[y] = startYear + y;
      }

      return {
        successRate: successCount / numSims,
        avgEndPortfolio: average(endPortfolios),
        medianEndPortfolio: median(endPortfolios),
        tenthPercentileEndPortfolio: percentile(endPortfolios, 0.1),
        endPortfolios,
        // Nuovi output per il grafico
        years: yearsArray,
        medianPath: medianPath.map(v => Math.round(v)),
        p10Path: p10Path.map(v => Math.round(v)),
        p90Path: p90Path.map(v => Math.round(v))
      };
    }

    // --- Charting support ---
    let portfolioChart = null;

    function destroyChart() {
      if (portfolioChart) {
        portfolioChart.destroy();
        portfolioChart = null;
      }
    }

    function renderPortfolioChart(yearsArray, medianPath, p10Path, p90Path) {
      destroyChart();
      
      const ctx = document.getElementById('portfolioChart');
      if (!ctx) return;

      const labels = yearsArray.map(y => y.toString());

      const config = {
        type: 'line',
        data: {
          labels: labels,
// Nel config dei datasets:
          datasets: [
          {
            label: 'Mediana (50°)',
            data: medianPath,
    borderColor: '#60a5fa',          // blue-400 più morbido e luminoso
    backgroundColor: 'rgba(96, 165, 250, 0.08)', // per eventuale fill futuro
    tension: 0.2,
    borderWidth: 3,
    pointRadius: 0
  },
  {
    label: '10° percentile',
    data: p10Path,
    borderColor: '#f87171',          // red-400, meno aggressivo del #f85149
    borderDash: [6, 4],
    tension: 0.2,
    borderWidth: 2.5,
    pointRadius: 0
  },
  {
    label: '90° percentile',
    data: p90Path,
    borderColor: '#4ade80',          // green-400, più visibile del #3fb950
    borderDash: [6, 4],
    tension: 0.2,
    borderWidth: 2.5,
    pointRadius: 0
  }
  ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,

  // ── INTERAZIONE + TOOLTIP ────────────────────────────────────────────────
          interaction: {
    mode: 'index',          // Mostra tooltip per TUTTE le linee allo stesso anno (molto utile!)
    axis: 'x',              // Si attiva muovendo orizzontalmente (lungo l'asse tempo)
    intersect: false        // IMPORTANTISSIMO: tooltip appare anche se non sei esattamente sul punto
  },

  plugins: {
    tooltip: {
      enabled: true,        // Tooltip nativo attivo
      mode: 'index',
      intersect: false,
      position: 'nearest',  // Posizione ottimale vicino al cursore

      // Personalizzazione visiva (dark mode friendly)
      backgroundColor: 'rgba(30, 41, 59, 0.92)',   // slate-900 semi-trasparente
      titleColor: '#e2e8f0',
      bodyColor: '#cbd5e1',
      borderColor: 'rgba(148, 163, 184, 0.4)',
      borderWidth: 1,
      cornerRadius: 6,
      padding: 10,

      // Formattazione valori € (molto più leggibile)
      callbacks: {
        title: function(tooltipItems) {
          return 'Anno: ' + tooltipItems[0].label;  // es. "Anno: 2042"
        },
        label: function(context) {
          const value = context.parsed.y;
          let formatted = '€' + value.toLocaleString('it-IT', { maximumFractionDigits: 0 });
          if (value >= 1000000) {
            formatted = '€' + (value / 1000000).toFixed(1) + ' M';
          } else if (value >= 1000) {
            formatted = '€' + Math.round(value / 1000) + ' k';
          }
          return context.dataset.label + ': ' + formatted;
        }
      }
    },

    legend: { /* ... come prima ... */ }
  },

  // Scales come prima (grid, ticks, etc.)
  scales: {
  x: {
    title: { display: true, text: 'Anno', color: '#9ca3af', font: { size: 13 } },
    ticks: { color: '#9ca3af', maxTicksLimit: 15 },
    grid: { color: 'rgba(156,163,175,0.18)', borderDash: [5,5] }
  },
  y: {
    type: 'logarithmic',
    title: {
      display: true,
      text: 'Valore portafoglio (€) — scala log',
      color: '#9ca3af',
      font: { size: 13 }
    },
    ticks: {
      color: '#9ca3af',
      callback: function(value, index, values) {
        if (value >= 1000000) return '€' + (value / 1000000).toFixed(1) + 'M';
        if (value >= 1000)    return '€' + Math.round(value / 1000) + 'k';
        if (value >= 100)     return '€' + Math.round(value);
        return '€' + value.toFixed(0);
      },
      min: 1000,
      suggestedMin: 1000,
      suggestedMax: Math.max(...p90Path) * 1.3 || 10000000
    },
    grid: { color: 'rgba(156,163,175,0.18)', borderDash: [5,5] }
  }
}
}
};

portfolioChart = new Chart(ctx, config);
}

    // ---- UI wiring ----
const statusLog = document.getElementById('statusLog');
const progressBar = document.getElementById('progressBar');

function setStatus(keyOrText, vars){
  var text;
  if(I18N[CUR_LANG] && I18N[CUR_LANG][keyOrText]){
    text = t(keyOrText, vars);
  } else if(typeof keyOrText === 'string' && keyOrText.indexOf('{')>=0){
        text = keyOrText; // avoid accidental replacement
      } else {
        text = keyOrText;
      }
      if(statusLog) statusLog.textContent = text;
      console.log(text);
    }
    function setProgress(p){ if(progressBar) progressBar.style.width = (Math.max(0,Math.min(100,p))) + '%'; }

    function readParams(overrides={}){
      try {
        const startYear = parseInt(getVal('startYear')) || 2025;
        const endYear = parseInt(getVal('endYear')) || (startYear + 59);
        // gather one-off entries
        const oneOffEntries = [];
        for(let i=1;i<=3;i++){
          const amt = Number(getVal('oneoff'+i+'Amount'));
          const yr = parseInt(getVal('oneoff'+i+'Year'));
          if(!isNaN(amt) && amt !== 0 && !isNaN(yr)){
            oneOffEntries.push({ amount: amt, year: yr });
          }
        }

        const params = {
          initialPortfolio: Number(getVal('initialPortfolio')) || 0,
          annualInvestment: Number(getVal('annualInvestment')) || 0,
          monthlySpending: Number(getVal('monthlySpending')) || 0,
          // convert percentage inputs (user supplies e.g. 2.00 for 2%) to decimals
          inflationRate: (Number(getVal('inflationRate')) || 0) / 100,
          dividendGrowthRate: (Number(getVal('dividendGrowthRate')) || 0) / 100,
          rentGrowthRate: (Number(getVal('rentGrowthRate')) || 0) / 100,
          meanReturn: (Number(getVal('meanReturn')) || 0) / 100,
          stdDev: (Number(getVal('stdDev')) || 0) / 100,
          numSims: Math.max(1, Math.round(Number(getVal('numSims')) || 500)),
          retireYear: parseInt(getVal('retireYear')) || 2028,
          startYear: startYear,
          endYear: endYear,
          dividendStartYear: parseInt(getVal('dividendStartYear')) || 2028,
          dividendMonthly: Number(getVal('dividendMonthly')) || 0,
          rentStartYear: parseInt(getVal('rentStartYear')) || 2028,
          rentMonthly: Number(getVal('rentMonthly')) || 0,
          alePensionStartYear: parseInt(getVal('alePensionStartYear')) || 2040,
          alePensionMonthly: Number(getVal('alePensionMonthly')) || 0,
          davPensionStartYear: parseInt(getVal('davPensionStartYear')) || 2058,
          davPensionMonthly: Number(getVal('davPensionMonthly')) || 0,
          oneOffs: oneOffEntries
        };
        // convert monthlySpending to annualSpending for use
        params.annualSpending = (Number(params.monthlySpending)||0) * 12;
        // basic validation
        if (params.endYear < params.startYear) throw new Error(t('endYearLabel') + ' must be >= ' + t('startYearLabel'));
        if (params.retireYear < params.startYear || params.retireYear > params.endYear) throw new Error(t('retireFromLabel') + ' must be between ' + t('startYearLabel') + ' and ' + t('endYearLabel'));
        return Object.assign(params, overrides);
      } catch (e) {
        console.error('readParams failed', e);
        throw e;
      }
    }

    // Run single Monte Carlo
    document.getElementById('btnRun').addEventListener('click', function(){
      try {
        const input = readParams();
        setStatus('runningMonteCarlo'); 
        setProgress(6);

        setTimeout(()=>{
          try {
            const out = monteCarlo(Object.assign({}, input, {annualSpending: input.annualSpending}));

            // Aggiornamento testi risultati (come prima)
            setText('resSuccess', (out.successRate*100).toFixed(2)+'%');
            setText('resAvg', fmtMoney(out.avgEndPortfolio));
            setText('resMed', fmtMoney(out.medianEndPortfolio));
            setText('resP10', fmtMoney(out.tenthPercentileEndPortfolio));

            setStatus('doneSuccess', {pct: (out.successRate*100).toFixed(2)}); 
            setProgress(100);
            setTimeout(()=>setProgress(0), 800);

            // ── NUOVA PARTE: MOSTRA E AGGIORNA IL GRAFICO ───────────────────────────────
            if (out.medianPath && out.p10Path && out.p90Path && out.years) {
              const chartSection = document.getElementById('chartSection');
              if (chartSection) {
                chartSection.style.display = 'block';
                renderPortfolioChart(out.years, out.medianPath, out.p10Path, out.p90Path);
                
                // Scroll morbido alla sezione del grafico (dopo un piccolo ritardo per il rendering)
                setTimeout(() => {
                  chartSection.scrollIntoView({ behavior: "smooth", block: "start" });
                }, 300);
              }
            } else {
              console.warn("Dati per il grafico mancanti");
            }
            // ──────────────────────────────────────────────────────────────────────────────

            // Scroll originale ai risultati (puoi mantenerlo o commentarlo se preferisci solo grafico)
            document.getElementById("outputArea").scrollIntoView({ behavior: "smooth", block: "start" });

          } catch (err) {
            console.error(err);
            setStatus('errorDuring', {msg: err.message});
            setProgress(0);
          }
        }, 10);
      } catch (err) {
        setStatus('errorDuring', {msg: err.message});
      }
    });

    // Find earliest retirement year
    document.getElementById('btnFindYear').addEventListener('click', function(){
      try {
        const base = readParams();
        const target = (parseFloat(document.getElementById('targetPct').value)/100) || 0.9;
        const fast = document.getElementById('fastSearch').checked;
        const simsForSearch = fast ? Math.max(50, Math.round(base.numSims/6)) : base.numSims; // smaller for responsiveness
        setStatus('searchingYear'); setProgress(3);

        setTimeout(()=>{
          try {
            const horizonEnd = base.endYear;
            let found = null, foundSuccess = null;
            for(let cand = base.startYear; cand <= horizonEnd; cand++){
              setStatus('testingRetireYear', {year: cand});
              setProgress(((cand - base.startYear) / (horizonEnd - base.startYear + 1)) * 80 + 5);
              const p = Object.assign({}, base, {retireYear: cand, numSims: simsForSearch, annualSpending: base.annualSpending});
              const out = monteCarlo(p);
              if(out.successRate >= target){ found = cand; foundSuccess = out.successRate; break; }
            }
            if(found){ setText('earliestResult', t('foundYear', {year: found, pct: (foundSuccess*100).toFixed(2)})); setStatus('foundYear', {year: found, pct: (foundSuccess*100).toFixed(2)}); }
            else { setText('earliestResult', t('noFeasibleYear')); setStatus('noFeasibleYear'); }
            setProgress(100); setTimeout(()=>setProgress(0),600);
            // Auto scroll to status/results
            document.getElementById("outputArea").scrollIntoView({ behavior: "smooth", block: "start" });
          } catch (err) {
            console.error(err);
            setStatus('errorDuring', {msg: err.message});
          }
        },10);
      } catch (err) {
        setStatus('errorDuring', {msg: err.message});
      }
    });

    // Max yearly spending for target
    document.getElementById('btnMaxSpend').addEventListener('click', function(){
      try {
        const base = readParams();
        const target = (parseFloat(document.getElementById('targetPct').value)/100) || 0.9;
        const fast = document.getElementById('fastSearch').checked;
        const simsForSearch = fast ? Math.max(50, Math.round(base.numSims/6)) : base.numSims;
        setStatus('searchingMaxSpend'); setProgress(3);

        setTimeout(()=>{
          try {
            // Quick check: 0 spending
            const out0 = monteCarlo(Object.assign({}, base, {annualSpending:0, numSims: simsForSearch}));
            if(out0.successRate < target){
              setText('maxSpendResult', t('noSolutionZero', {target: (target*100).toFixed(0), pct: (out0.successRate*100).toFixed(2)}));
              setStatus('noSolutionZero', {target: (target*100).toFixed(0), pct: (out0.successRate*100).toFixed(2)});
              setProgress(100); setTimeout(()=>setProgress(0),600);
              return;
            }

            // bounds
            const yearsUntilRetire = Math.max(0, base.retireYear - base.startYear);
            const contributions = base.annualInvestment * yearsUntilRetire;
            let low = 0;
            let high = Math.max(base.annualSpending*5, base.initialPortfolio + contributions, 50000);

            // expand high until fail
            let attempts = 0;
            let outHigh = monteCarlo(Object.assign({}, base, {annualSpending: high, numSims: simsForSearch}));
            while(outHigh.successRate >= target && attempts < 8){
              high *= 2;
              outHigh = monteCarlo(Object.assign({}, base, {annualSpending: high, numSims: simsForSearch}));
              attempts++;
            }

            // binary search
            const tol = 100; let best = 0; let bestSuccess = 0; let iter = 0; const maxIter = 20;
            while((high-low) > tol && iter < maxIter){
              const mid = Math.round((low+high)/2);
              setStatus('testingRetireYear', {year: mid}); setProgress(10 + (iter/maxIter)*70);
              const out = monteCarlo(Object.assign({}, base, {annualSpending: mid, numSims: simsForSearch}));
              if(out.successRate >= target){ best = mid; bestSuccess = out.successRate; low = mid; }
              else { high = mid; }
              iter++;
            }

            if(best === 0){
              setText('maxSpendResult', t('maxYearlyReachedZero'));
              setStatus('done');
            } else {
              const monthlyLabel = t('monthlyLabel') || 'Monthly';
              const successText = (CUR_LANG === 'it') ? 'Successo' : 'Success';
              const resultText = t('maxSpendLabel') + ': €' + best.toLocaleString() +
              ' • ' + monthlyLabel + ': €' + Math.round(best/12).toLocaleString() +
              ' • ' + successText + ': ' + (bestSuccess*100).toFixed(2) + '%';

              setText('maxSpendResult', resultText);
              setStatus(resultText);   // <-- update status box too
              // Auto scroll to status/results
              document.getElementById("outputArea").scrollIntoView({ behavior: "smooth", block: "start" });
            }
            setProgress(100); setTimeout(()=>setProgress(0),600);
          } catch (err) {
            console.error(err);
            setStatus('errorDuring', {msg: err.message});
            setProgress(0);
          }
        },10);
      } catch (err) {
        setStatus('errorDuring', {msg: err.message});
      }
    });

    // Portfolio needed for target retirement year
    document.getElementById('btnPortfolioNeeded').addEventListener('click', function(){
      try {
        const base = readParams();
        const target = (parseFloat(document.getElementById('targetPct').value)/100) || 0.9;
        const fast = document.getElementById('fastSearch').checked;
        const simsForSearch = fast ? Math.max(50, Math.round(base.numSims/6)) : base.numSims;
        setStatus('searchingPortfolio'); setProgress(3);

        setTimeout(()=>{
          try {
            // First check current portfolio
            const currentOut = monteCarlo(Object.assign({}, base, {numSims: simsForSearch}));
            if(currentOut.successRate >= target){
              const resultText = t('portfolioSufficient', {
                year: base.retireYear,
                current: base.initialPortfolio.toLocaleString(),
                needed: Math.round(currentOut.medianEndPortfolio).toLocaleString(),
                pct: (currentOut.successRate*100).toFixed(2)
              });
              setText('portfolioNeededResult', resultText);
              setStatus(resultText);
              setProgress(100); setTimeout(()=>setProgress(0),600);
              document.getElementById("outputArea").scrollIntoView({ behavior: "smooth", block: "start" });
              return;
            }

            // Binary search for required portfolio
            let low = base.initialPortfolio;
            let high = Math.max(base.initialPortfolio * 10, 1000000);
            let attempts = 0;
            let outHigh = monteCarlo(Object.assign({}, base, {initialPortfolio: high, numSims: simsForSearch}));
            while(outHigh.successRate < target && attempts < 8){
              high *= 2;
              outHigh = monteCarlo(Object.assign({}, base, {initialPortfolio: high, numSims: simsForSearch}));
              attempts++;
            }

            const tol = 1000; let best = low; let bestSuccess = currentOut.successRate; let iter = 0; const maxIter = 20;
            while((high-low) > tol && iter < maxIter){
              const mid = Math.round((low+high)/2);
              setStatus('testingPortfolio', {amt: mid.toLocaleString()}); setProgress(10 + (iter/maxIter)*70);
              const out = monteCarlo(Object.assign({}, base, {initialPortfolio: mid, numSims: simsForSearch}));
              if(out.successRate >= target){ best = mid; bestSuccess = out.successRate; high = mid; }
              else { low = mid; bestSuccess = out.successRate; }
              iter++;
            }

            if(bestSuccess < target){
              setText('portfolioNeededResult', t('noPortfolioSolution', {target: (target*100).toFixed(0), year: base.retireYear, pct: (bestSuccess*100).toFixed(2)}));
              setStatus('noPortfolioSolution', {target: (target*100).toFixed(0), year: base.retireYear, pct: (bestSuccess*100).toFixed(2)});
            } else {
              const additional = Math.max(0, Math.round(best - base.initialPortfolio));
              const resultText = t('portfolioNeededResult', {year: base.retireYear, amt: additional.toLocaleString(), pct: (bestSuccess*100).toFixed(2)});
              setText('portfolioNeededResult', resultText);
              setStatus(resultText);
            }
            setProgress(100); setTimeout(()=>setProgress(0),600);
            document.getElementById("outputArea").scrollIntoView({ behavior: "smooth", block: "start" });
          } catch (err) {
            console.error(err);
            setStatus('errorDuring', {msg: err.message});
            setProgress(0);
          }
        },10);
      } catch (err) {
        setStatus('errorDuring', {msg: err.message});
      }
    });

    // Income needed (new) — finds extra recurring annual income (from retireYear onward) required
    document.getElementById('btnIncomeNeeded').addEventListener('click', function(){
      try {
        const base = readParams();
        const target = (parseFloat(document.getElementById('targetPct').value)/100) || 0.9;
        const fast = document.getElementById('fastSearch').checked;
        const simsForSearch = fast ? Math.max(50, Math.round(base.numSims/6)) : base.numSims;
        setStatus('searchingIncome'); setProgress(3);

        setTimeout(()=>{
          try {
            // quick check: if current already meets target
            const currentOut = monteCarlo(Object.assign({}, base, {numSims: simsForSearch, extraAnnualIncome: 0}));
            if(currentOut.successRate >= target){
              const resultText = t('incomeNeededResult', {
                year: base.retireYear,
                amt: 0,
                m: 0,
                pct: (currentOut.successRate*100).toFixed(2)
              });
              setText('incomeNeededResult', resultText);
              setStatus(resultText);
              setProgress(100); setTimeout(()=>setProgress(0),600);
              document.getElementById("outputArea").scrollIntoView({ behavior: "smooth", block: "start" });
              return;
            }

            // find upper bound by doubling
            let low = 0;
            let high = Math.max(20000, base.monthlySpending*12, 50000);
            let attempts = 0;
            let outHigh = monteCarlo(Object.assign({}, base, {numSims: simsForSearch, extraAnnualIncome: high}));
            while(outHigh.successRate < target && attempts < 12){
              high *= 2;
              outHigh = monteCarlo(Object.assign({}, base, {numSims: simsForSearch, extraAnnualIncome: high}));
              attempts++;
            }

            if(outHigh.successRate < target){
              // couldn't find feasible high within attempts
              setText('incomeNeededResult', t('noIncomeSolution', {target: (target*100).toFixed(0), year: base.retireYear, pct: (outHigh.successRate*100).toFixed(2)}));
              setStatus('noIncomeSolution', {target: (target*100).toFixed(0), year: base.retireYear, pct: (outHigh.successRate*100).toFixed(2)});
              setProgress(100); setTimeout(()=>setProgress(0),600);
              return;
            }

            // binary search for minimal annual extra income
            const tol = 100; let best = high; let bestSuccess = outHigh.successRate; let iter = 0; const maxIter = 30;
            while((high - low) > tol && iter < maxIter){
              const mid = Math.round((low + high)/2);
              setStatus('testingIncome', {amt: mid.toLocaleString()}); setProgress(10 + (iter/maxIter)*70);
              const out = monteCarlo(Object.assign({}, base, {numSims: simsForSearch, extraAnnualIncome: mid}));
              if(out.successRate >= target){ best = mid; bestSuccess = out.successRate; high = mid; }
              else { low = mid; }
              iter++;
            }

            if(bestSuccess < target){
              setText('incomeNeededResult', t('noIncomeSolution', {target: (target*100).toFixed(0), year: base.retireYear, pct: (bestSuccess*100).toFixed(2)}));
              setStatus('noIncomeSolution', {target: (target*100).toFixed(0), year: base.retireYear, pct: (bestSuccess*100).toFixed(2)});
            } else {
              const monthly = Math.round(best/12);
              const resultText = t('incomeNeededResult', {year: base.retireYear, amt: best.toLocaleString(), m: monthly.toLocaleString(), pct: (bestSuccess*100).toFixed(2)});
              setText('incomeNeededResult', resultText);
              setStatus(resultText);
            }
            setProgress(100); setTimeout(()=>setProgress(0),600);
            document.getElementById("outputArea").scrollIntoView({ behavior: "smooth", block: "start" });
          } catch (err) {
            console.error(err);
            setStatus('errorDuring', {msg: err.message});
            setProgress(0);
          }
        },10);
      } catch (err) {
        setStatus('errorDuring', {msg: err.message});
      }
    });

    // Reset inputs helper
document.getElementById('btnReset').addEventListener('click', function(){
  try {
        // Dynamically set startYear on reset as well
    const now = new Date();
    const currentYear = now.getFullYear();
    const currentMonth = now.getMonth() + 1;
    let startYear = currentYear;
    if (currentMonth >= 7) { startYear = currentYear + 1; }
    document.getElementById('startYear').value = startYear;
    document.getElementById('endYear').value = 2083; document.getElementById('retireYear').value = 2028;
    document.getElementById('numSims').value = 1000; 
    document.getElementById('currentMonthlySpending').value = '';
    document.getElementById('monthlySpending').value = 3000; document.getElementById('inflationRate').value = 2.00;
    document.getElementById('initialPortfolio').value = 800000; document.getElementById('annualInvestment').value = 50000; document.getElementById('meanReturn').value = 6.00; document.getElementById('stdDev').value = 10.00;
    document.getElementById('alePensionStartYear').value = 2040; document.getElementById('alePensionMonthly').value = 2100; document.getElementById('davPensionStartYear').value = 2058; document.getElementById('davPensionMonthly').value = 1600;
    document.getElementById('dividendStartYear').value = 2031; document.getElementById('dividendMonthly').value = 900; document.getElementById('dividendGrowthRate').value = 2.00;
    document.getElementById('rentStartYear').value = 2031; document.getElementById('rentMonthly').value = 0; document.getElementById('rentGrowthRate').value = 2.00;
        // reset one-off items
    document.getElementById('oneoff1Amount').value = ''; document.getElementById('oneoff1Year').value = '';
    document.getElementById('oneoff2Amount').value = ''; document.getElementById('oneoff2Year').value = '';
    document.getElementById('oneoff3Amount').value = ''; document.getElementById('oneoff3Year').value = '';

    setText('resSuccess','—'); setText('resAvg','—'); setText('resMed','—'); setText('resP10','—'); 
    setText('earliestResult', t('earliestPlaceholder')); 
    setText('maxSpendResult', t('maxSpendPlaceholder'));
    setText('portfolioNeededResult', t('portfolioNeededPlaceholder'));
    setText('incomeNeededResult', t('incomeNeededPlaceholder'));
    setStatus('ready');
    formatInputFields();
  } catch (err) {
    console.error(err);
    setStatus('errorDuring', {msg: err.message});
  }
});

    // Save / Load state via localStorage (JSON)
function collectState(){
      // collect all input values (list them explicitly)
  const inputs = [
    'startYear','endYear','retireYear','numSims','currentMonthlySpending','monthlySpending','inflationRate',
    'initialPortfolio','annualInvestment','meanReturn','stdDev',
    'alePensionStartYear','alePensionMonthly','davPensionStartYear','davPensionMonthly',
    'dividendStartYear','dividendMonthly','dividendGrowthRate',
    'rentStartYear','rentMonthly','rentGrowthRate',
    'oneoff1Amount','oneoff1Year','oneoff2Amount','oneoff2Year','oneoff3Amount','oneoff3Year',
    'fastSearch','targetPct','langSelect'
    ];
  const state = { inputs: {}, outputs: {} };
  inputs.forEach(id=>{
    const el = document.getElementById(id);
    if(!el) return;
    if(el.type === 'checkbox') state.inputs[id] = el.checked;
    else state.inputs[id] = el.value;
  });
      // outputs
  state.outputs = {
    resSuccess: document.getElementById('resSuccess')?.textContent || '',
    resAvg: document.getElementById('resAvg')?.textContent || '',
    resMed: document.getElementById('resMed')?.textContent || '',
    resP10: document.getElementById('resP10')?.textContent || '',
    earliestResult: document.getElementById('earliestResult')?.textContent || '',
    maxSpendResult: document.getElementById('maxSpendResult')?.textContent || '',
    portfolioNeededResult: document.getElementById('portfolioNeededResult')?.textContent || '',
    incomeNeededResult: document.getElementById('incomeNeededResult')?.textContent || '',
    statusLog: document.getElementById('statusLog')?.textContent || ''
  };
  return state;
}

function saveStateToLocalStorage(){
  try {
    setStatus('savingState');
    const state = collectState();
    localStorage.setItem('firecalc_state', JSON.stringify(state));
    setStatus('savedState');
  } catch (err) {
    console.error('saveState failed', err);
    setStatus('errorDuring', {msg: err.message});
  }
}

function loadStateFromLocalStorage(){
  try {
    setStatus('loadingState');
    const v = localStorage.getItem('firecalc_state');
    if(!v){ setStatus('errorDuring', {msg: 'No saved state found'}); return; }
    const state = JSON.parse(v);
    if(state && state.inputs){
      Object.keys(state.inputs).forEach(id=>{
        const el = document.getElementById(id);
        if(!el) return;
        if(el.type === 'checkbox') el.checked = !!state.inputs[id];
        else el.value = state.inputs[id];
      });
    }
        // apply language if present, before setting outputs
    const lang = (state && state.inputs && state.inputs.langSelect) ? state.inputs.langSelect : null;
    if(lang) { CUR_LANG = lang; document.getElementById('langSelect').value = CUR_LANG; applyTranslations(); }
    if(state && state.outputs){
      setText('resSuccess', state.outputs.resSuccess || '—');
      setText('resAvg', state.outputs.resAvg || '—');
      setText('resMed', state.outputs.resMed || '—');
      setText('resP10', state.outputs.resP10 || '—');
      setText('earliestResult', state.outputs.earliestResult || t('earliestPlaceholder'));
      setText('maxSpendResult', state.outputs.maxSpendResult || t('maxSpendPlaceholder'));
      setText('portfolioNeededResult', state.outputs.portfolioNeededResult || t('portfolioNeededPlaceholder'));
      setText('incomeNeededResult', state.outputs.incomeNeededResult || t('incomeNeededPlaceholder'));
      setStatus(state.outputs.statusLog || t('loadedState'));
    }
    formatInputFields();
    setStatus('loadedState');
  } catch (err) {
    console.error('loadState failed', err);
    setStatus('errorDuring', {msg: err.message});
  }
}

document.getElementById('btnSave').addEventListener('click', saveStateToLocalStorage);
document.getElementById('btnLoad').addEventListener('click', loadStateFromLocalStorage);

    // language switcher
document.getElementById('langSelect').addEventListener('change', function(e){
  CUR_LANG = e.target.value || 'en';
  applyTranslations();
});

    // --- quick self-test on load (lightweight) ---
window.addEventListener('load', function(){
  try {
        // Dynamically populate Start Year
    const now = new Date();
    const currentYear = now.getFullYear();
        const currentMonth = now.getMonth() + 1; // January=1, ..., December=12
        let startYear = currentYear;
        if (currentMonth >= 7) { startYear = currentYear + 1; }
        document.getElementById('startYear').value = startYear;

        // initialize language
        CUR_LANG = (navigator.language && navigator.language.startsWith('it')) ? 'it' : 'en';
        document.getElementById('langSelect').value = CUR_LANG;
        applyTranslations();

        formatInputFields();

        // use small numSims to verify code path (won't block UI)
        const params = readParams(); params.numSims = Math.min(20, Math.max(10, Math.round(params.numSims/10)));
        const out = monteCarlo(Object.assign({}, params, {annualSpending: params.annualSpending, numSims: params.numSims}));
        setStatus('doneSuccess', {pct: (out.successRate*100).toFixed(1)});
      } catch (err) {
        console.error('Self-test failed', err);
        setStatus('errorDuring', {msg: err && err.message ? err.message : String(err)});
      }
    });

    // --- Saved simulation slots (3) ---
function _slotKey(i){ return 'firecalc_slot_' + i; }
function updateSavedSlotsUI(){
  for(let i=1;i<=3;i++){
    const key = _slotKey(i);
    const saved = localStorage.getItem(key);
    const label = document.getElementById('savedSlotLabel'+i);
    if(saved){
      try{
        const obj = JSON.parse(saved);
        if(obj && obj.meta && obj.meta.name){ label.textContent = obj.meta.name; label.style.color = 'var(--text)'; }
        else { label.textContent = obj.name || 'saved'; label.style.color = 'var(--text)'; }
      }catch(e){ label.textContent = '(corrupt)'; label.style.color = 'var(--danger)'; }
    } else {
      label.textContent = t('savedSlotEmpty') || '(empty)'; label.style.color = 'var(--muted)';
    }
  }
}

async function quickSuccessForParams(params){
      // try to reuse displayed result if available
  const disp = document.getElementById('resSuccess')?.textContent || '';
  if(disp && disp.trim() !== '—'){
    const num = parseFloat(disp.replace('%',''));
    if(!isNaN(num)) return num/100;
  }
      // otherwise run a small monteCarlo (fast)
  const quick = Object.assign({}, params, {numSims: Math.min(200, Math.max(50, Math.round(params.numSims/5)))});
  const out = monteCarlo(quick);
  return out.successRate;
}

function collectFullState(){
  return collectState();
}

async function saveToSlot(i){
  try{
    setStatus('savingState');
    const params = readParams();
    const success = await quickSuccessForParams(params);
    const name = params.retireYear + '-' + Math.round(params.monthlySpending) + ' - ' + (Math.round(success*1000)/10) + '%';
    const data = {
      meta: { name: name, retireYear: params.retireYear, monthlySpending: Math.round(params.monthlySpending), successRate: success },
      state: collectFullState(),
      savedAt: (new Date()).toISOString()
    };
    localStorage.setItem(_slotKey(i), JSON.stringify(data));
    setStatus('savedState');
    updateSavedSlotsUI();
  }catch(err){ console.error(err); setStatus('errorDuring', {msg: err.message}); }
}

async function saveToFirstAvailableSlot(){
  try{
    for(let i=1;i<=3;i++){
      if(!localStorage.getItem(_slotKey(i))){
        await saveToSlot(i);
        return;
      }
    }
        // if all full, overwrite slot 1
    await saveToSlot(1);
  }catch(err){ console.error(err); setStatus('errorDuring', {msg: err.message}); }
}

function loadSlot(i){
  try{
    const saved = localStorage.getItem(_slotKey(i));
    if(!saved){ setStatus('errorDuring', {msg: t('savedSlotEmpty')}); return; }
    const obj = JSON.parse(saved);
    if(obj && obj.state){
      const st = obj.state;
          // apply inputs
      if(st.inputs){
        Object.keys(st.inputs).forEach(id=>{
          const el = document.getElementById(id);
          if(!el) return;
          if(el.type === 'checkbox') el.checked = !!st.inputs[id];
          else el.value = st.inputs[id];
        });
      }
          // apply language if present, before setting outputs
      const lang = (st.inputs && st.inputs.langSelect) ? st.inputs.langSelect : null;
      if(lang){ CUR_LANG = lang; document.getElementById('langSelect').value = CUR_LANG; applyTranslations(); }
          // apply outputs
      if(st.outputs){
        setText('resSuccess', st.outputs.resSuccess || '—');
        setText('resAvg', st.outputs.resAvg || '—');
        setText('resMed', st.outputs.resMed || '—');
        setText('resP10', st.outputs.resP10 || '—');
        setText('earliestResult', st.outputs.earliestResult || t('earliestPlaceholder'));
        setText('maxSpendResult', st.outputs.maxSpendResult || t('maxSpendPlaceholder'));
        setText('portfolioNeededResult', st.outputs.portfolioNeededResult || t('portfolioNeededPlaceholder'));
        setText('incomeNeededResult', st.outputs.incomeNeededResult || t('incomeNeededPlaceholder'));
        setStatus(st.outputs.statusLog || t('loadedState'));
      }
      formatInputFields();
      setStatus('loadedState');
    }
  }catch(err){ console.error(err); setStatus('errorDuring', {msg: err.message}); }
}

function downloadSlot(i){
  const saved = localStorage.getItem(_slotKey(i));
  if(!saved){ setStatus('errorDuring', {msg: t('savedSlotEmpty')}); return; }
  const blob = new Blob([saved], {type: 'application/json;charset=utf-8'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'firecalc_saved_slot_' + i + '.json';
  document.body.appendChild(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
}

function clearAllSlots(){
  for(let i=1;i<=3;i++) localStorage.removeItem(_slotKey(i));
    updateSavedSlotsUI();
  setStatus('done');
}

    // wire new buttons
document.getElementById('btnSave').removeEventListener?.('click', saveStateToLocalStorage);
document.getElementById('btnSave').addEventListener('click', function(e){ saveToFirstAvailableSlot(); });

document.getElementById('btnLoad').removeEventListener?.('click', loadStateFromLocalStorage);
document.getElementById('btnLoad').addEventListener('click', function(e){ loadStateFromLocalStorage(); });

document.getElementById('btnLoadSlot1').addEventListener('click', function(){ loadSlot(1); });
document.getElementById('btnLoadSlot2').addEventListener('click', function(){ loadSlot(2); });
document.getElementById('btnLoadSlot3').addEventListener('click', function(){ loadSlot(3); });
document.getElementById('btnDownloadSlot1').addEventListener('click', function(){ downloadSlot(1); });
document.getElementById('btnDownloadSlot2').addEventListener('click', function(){ downloadSlot(2); });
document.getElementById('btnDownloadSlot3').addEventListener('click', function(){ downloadSlot(3); });
document.getElementById('btnClearSlots').addEventListener('click', clearAllSlots);

    // initialize saved slots UI on load
updateSavedSlotsUI();

    // Function to update future monthly spending
function updateFutureSpending() {
  const currentSpend = Number(getVal('currentMonthlySpending')) || 0;
      if (currentSpend <= 0) return; // don't update if empty or zero
      const inf = Number(getVal('inflationRate')) / 100 || 0;
      const retireY = parseInt(getVal('retireYear')) || 0;
      const now = new Date();
      const currentY = now.getFullYear();
      const years = Math.max(0, retireY - currentY);
      const future = currentSpend * Math.pow(1 + inf, years) ;
      document.getElementById('monthlySpending').value = Math.round(future / 50) * 50; // round to nearest 100
    }

    // Add event listeners for updating future spending
    document.getElementById('currentMonthlySpending').addEventListener('input', updateFutureSpending);
    document.getElementById('inflationRate').addEventListener('input', updateFutureSpending);
    document.getElementById('retireYear').addEventListener('input', updateFutureSpending);

    // Thousand separator formatting
    function formatNumber(num) {
      return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }

    function unformatNumber(str) {
      return str.replace(/,/g, '');
    }

    function formatInputFields() {
      ['initialPortfolio', 'annualInvestment'].forEach(id => {
        const el = document.getElementById(id);
        if (el) {
          const val = unformatNumber(el.value);
          if (!isNaN(val) && val !== '') {
            el.value = formatNumber(val);
          }
        }
      });
    }

    // Add event listeners for formatting
    ['initialPortfolio', 'annualInvestment'].forEach(id => {
      const el = document.getElementById(id);
      el.addEventListener('focus', () => {
        el.value = unformatNumber(el.value);
      });
      el.addEventListener('blur', () => {
        const val = unformatNumber(el.value);
        if (!isNaN(val) && val !== '') {
          el.value = formatNumber(val);
        }
      });
    });

  </script>
</body>
</html>
