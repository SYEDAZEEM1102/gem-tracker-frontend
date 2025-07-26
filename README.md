# gem-tracker-frontend
gem-tracker-frontend
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gem Tracker Dashboard - Live Test</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            background: white;
            border-radius: 16px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
        }

        .header h1 {
            color: #333;
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        .header p {
            color: #666;
            font-size: 1.1rem;
        }

        .status-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card {
            background: white;
            border-radius: 16px;
            padding: 25px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            border-left: 5px solid;
        }

        .card.accounts { border-left-color: #4CAF50; }
        .card.calls { border-left-color: #2196F3; }
        .card.success { border-left-color: #FF9800; }
        .card.uptime { border-left-color: #9C27B0; }

        .card h3 {
            color: #333;
            font-size: 1rem;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .card .value {
            font-size: 2.5rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 5px;
        }

        .card .subtitle {
            color: #666;
            font-size: 0.9rem;
        }

        .monitoring-section {
            background: white;
            border-radius: 16px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }

        .monitoring-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }

        .monitoring-header h2 {
            color: #333;
            font-size: 1.5rem;
        }

        .status-indicator {
            display: flex;
            align-items: center;
            gap: 10px;
            font-weight: bold;
        }

        .status-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #f44336;
        }

        .status-dot.connected {
            background: #4CAF50;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-start {
            background: #4CAF50;
            color: white;
        }

        .btn-start:hover {
            background: #45a049;
        }

        .btn-stop {
            background: #f44336;
            color: white;
        }

        .btn-stop:hover {
            background: #da190b;
        }

        .btn:disabled {
            background: #ccc;
            cursor: not-allowed;
        }

        .accounts-section {
            background: white;
            border-radius: 16px;
            padding: 30px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }

        .accounts-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        .accounts-table th {
            background: #f8f9fa;
            padding: 15px;
            text-align: left;
            font-weight: bold;
            color: #333;
            border-bottom: 2px solid #dee2e6;
        }

        .accounts-table td {
            padding: 15px;
            border-bottom: 1px solid #dee2e6;
        }

        .accounts-table tr:hover {
            background: #f8f9fa;
        }

        .category-badge {
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: bold;
            text-transform: uppercase;
        }

        .badge-main {
            background: #e3f2fd;
            color: #1976d2;
        }

        .badge-emerging {
            background: #f3e5f5;
            color: #7b1fa2;
        }

        .badge-watch {
            background: #fff3e0;
            color: #f57c00;
        }

        .loading {
            text-align: center;
            padding: 40px;
            color: #666;
        }

        .error {
            background: #ffebee;
            color: #c62828;
            padding: 15px;
            border-radius: 8px;
            margin: 10px 0;
        }

        .success {
            background: #e8f5e8;
            color: #2e7d32;
            padding: 15px;
            border-radius: 8px;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <div class="header">
            <h1>💎 Gem Tracker Dashboard</h1>
            <p>Live Backend Testing - Automated Crypto Gem Call Analysis</p>
        </div>

        <!-- Status Cards -->
        <div class="status-cards" id="statusCards">
            <div class="card accounts">
                <h3>Total Accounts</h3>
                <div class="value" id="totalAccounts">-</div>
                <div class="subtitle" id="activeAccounts">Loading...</div>
            </div>
            <div class="card calls">
                <h3>Calls Today</h3>
                <div class="value" id="callsToday">-</div>
                <div class="subtitle" id="successRate">Loading...</div>
            </div>
            <div class="card success">
                <h3>Portfolio Value</h3>
                <div class="value" id="totalValue">-</div>
                <div class="subtitle">Tracked value</div>
            </div>
            <div class="card uptime">
                <h3>System Status</h3>
                <div class="value" id="systemUptime">-</div>
                <div class="subtitle">Uptime</div>
            </div>
        </div>

        <!-- Monitoring Section -->
        <div class="monitoring-section">
            <div class="monitoring-header">
                <h2>🔗 Backend Connection Test</h2>
                <div class="status-indicator">
                    <div class="status-dot" id="connectionDot"></div>
                    <span id="connectionStatus">Connecting...</span>
                </div>
            </div>
            
            <div>
                <button class="btn btn-start" id="startBtn" onclick="startMonitoring()">
                    Start Twitter Monitoring
                </button>
                <button class="btn btn-stop" id="stopBtn" onclick="stopMonitoring()" style="display: none;">
                    Stop Monitoring
                </button>
            </div>

            <div id="statusMessage"></div>
        </div>

        <!-- Accounts Section -->
        <div class="accounts-section">
            <h2>📊 Account Performance</h2>
            <div id="accountsContent">
                <div class="loading">Loading accounts data...</div>
            </div>
        </div>
    </div>

    <script>
        const API_BASE = 'https://nodejs-production-b2333.up.railway.app';
        let isMonitoring = false;

        // Load data when page loads
        window.addEventListener('load', () => {
            loadSystemStats();
            loadAccounts();
            checkConnection();
        });

        // Check backend connection
        async function checkConnection() {
            try {
                const response = await fetch(`${API_BASE}/api/system/status`);
                const data = await response.json();
                
                document.getElementById('connectionDot').classList.add('connected');
                document.getElementById('connectionStatus').textContent = 'Connected to Backend ✅';
                isMonitoring = data.isMonitoring || false;
                updateMonitoringButtons();
                
            } catch (error) {
                document.getElementById('connectionStatus').textContent = 'Connection Failed ❌';
                showError('Cannot connect to backend. Please check if your Railway service is running.');
            }
        }

        // Load system statistics
        async function loadSystemStats() {
            try {
                const response = await fetch(`${API_BASE}/api/system/stats`);
                const stats = await response.json();
                
                document.getElementById('totalAccounts').textContent = stats.totalAccounts || 0;
                document.getElementById('activeAccounts').textContent = `${stats.activeAccounts || 0} active`;
                document.getElementById('callsToday').textContent = stats.callsToday || 0;
                document.getElementById('successRate').textContent = `${(stats.successRate || 0).toFixed(1)}% success rate`;
                document.getElementById('totalValue').textContent = `$${((stats.totalValue || 0) / 1000000).toFixed(2)}M`;
                document.getElementById('systemUptime').textContent = stats.systemUptime || 'Unknown';
                
            } catch (error) {
                showError('Failed to load system statistics');
            }
        }

        // Load accounts data
        async function loadAccounts() {
            try {
                const response = await fetch(`${API_BASE}/api/accounts`);
                const accounts = await response.json();
                
                if (accounts.error) {
                    throw new Error(accounts.error);
                }
                
                displayAccounts(accounts);
                
            } catch (error) {
                document.getElementById('accountsContent').innerHTML = 
                    `<div class="error">Failed to load accounts: ${error.message}</div>`;
            }
        }

        // Display accounts in table
        function displayAccounts(accounts) {
            if (!accounts || accounts.length === 0) {
                document.getElementById('accountsContent').innerHTML = 
                    '<div class="loading">No accounts found. Start monitoring to discover gem callers!</div>';
                return;
            }

            let html = `
                <table class="accounts-table">
                    <thead>
                        <tr>
                            <th>Account</th>
                            <th>Category</th>
                            <th>Total Calls</th>
                            <th>Wins</th>
                            <th>Win Rate</th>
                            <th>Avg Return</th>
                            <th>Sharpe Ratio</th>
                            <th>Last Active</th>
                        </tr>
                    </thead>
                    <tbody>
            `;

            accounts.forEach(account => {
                const categoryClass = account.category === 'GEM_MAIN' ? 'badge-main' : 
                                     account.category === 'GEM_EMERGING' ? 'badge-emerging' : 'badge-watch';
                
                html += `
                    <tr>
                        <td>
                            <strong>${account.handle}</strong><br>
                            <small style="color: #666;">${account.specialty || 'General'}</small>
                        </td>
                        <td>
                            <span class="category-badge ${categoryClass}">
                                ${(account.category || 'WATCH_LIST').replace('_', ' ')}
                            </span>
                        </td>
                        <td>${account.total_calls || 0}</td>
                        <td style="color: #4CAF50; font-weight: bold;">${account.wins || 0}</td>
                        <td style="font-weight: bold; color: ${(account.win_rate || 0) >= 70 ? '#4CAF50' : (account.win_rate || 0) >= 50 ? '#FF9800' : '#f44336'}">
                            ${(account.win_rate || 0).toFixed(1)}%
                        </td>
                        <td style="color: #4CAF50; font-weight: bold;">+${(account.avg_return || 0).toFixed(1)}%</td>
                        <td>${(account.sharpe_ratio || 0).toFixed(2)}</td>
                        <td style="color: #666; font-size: 0.9rem;">
                            ${account.last_active ? new Date(account.last_active).toLocaleDateString() : 'Never'}
                        </td>
                    </tr>
                `;
            });

            html += '</tbody></table>';
            document.getElementById('accountsContent').innerHTML = html;
        }

        // Start monitoring
        async function startMonitoring() {
            try {
                showMessage('Starting Twitter monitoring...', 'info');
                
                const response = await fetch(`${API_BASE}/api/monitoring/start`, {
                    method: 'POST'
                });
                const result = await response.json();
                
                if (result.success) {
                    isMonitoring = true;
                    updateMonitoringButtons();
                    showMessage('✅ Twitter monitoring started successfully! Watching for gem calls...', 'success');
                } else {
                    throw new Error(result.error || 'Failed to start monitoring');
                }
                
            } catch (error) {
                showError(`Failed to start monitoring: ${error.message}`);
            }
        }

        // Stop monitoring
        async function stopMonitoring() {
            try {
                showMessage('Stopping Twitter monitoring...', 'info');
                
                const response = await fetch(`${API_BASE}/api/monitoring/stop`, {
                    method: 'POST'
                });
                const result = await response.json();
                
                if (result.success) {
                    isMonitoring = false;
                    updateMonitoringButtons();
                    showMessage('⏹️ Twitter monitoring stopped.', 'info');
                } else {
                    throw new Error(result.error || 'Failed to stop monitoring');
                }
                
            } catch (error) {
                showError(`Failed to stop monitoring: ${error.message}`);
            }
        }

        // Update monitoring buttons
        function updateMonitoringButtons() {
            const startBtn = document.getElementById('startBtn');
            const stopBtn = document.getElementById('stopBtn');
            
            if (isMonitoring) {
                startBtn.style.display = 'none';
                stopBtn.style.display = 'inline-block';
            } else {
                startBtn.style.display = 'inline-block';
                stopBtn.style.display = 'none';
            }
        }

        // Show message
        function showMessage(message, type) {
            const statusMessage = document.getElementById('statusMessage');
            statusMessage.innerHTML = `<div class="${type}">${message}</div>`;
            
            if (type !== 'error') {
                setTimeout(() => {
                    statusMessage.innerHTML = '';
                }, 5000);
            }
        }

        // Show error
        function showError(message) {
            showMessage(message, 'error');
        }

        // Auto-refresh data every 30 seconds
        setInterval(() => {
            loadSystemStats();
            loadAccounts();
        }, 30000);
    </script>
</body>
</html>
