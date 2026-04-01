# IDX Portfolio Balancer
 
An automated portfolio drift monitor and rebalancing engine for IDX equities, built with Python, pandas, and yfinance. Deployed as an interactive dashboard via Streamlit.
 
---
 
## Overview
 
This tool monitors a 5-asset IDX portfolio (BBCA, BBRI, BUMI, TLKM, CASH) against user-defined target weights and automatically identifies which positions have drifted beyond a configurable threshold. When drift exceeds the threshold, the engine calculates the exact trade value (in IDR) required to restore target allocation and logs all actions.
 
---
 
## Features
 
- **Drift monitoring** — continuously compares current vs. target allocation weights per asset
- **Configurable threshold** — drift threshold adjustable from 1% to 20% via slider
- **Portfolio sizing** — supports portfolio values from Rp 10 jt to Rp 500 jt
- **Auto-check scheduling** — check intervals of 1 hour, 8 hours, daily, or weekly
- **Trade calculation** — computes exact IDR value to buy or sell per drifted asset
- **Market drift simulation** — randomises weights to simulate real price movement
- **Rebalance log** — timestamped audit trail of all checks and executed trades
- **Allocation bar chart** — visual comparison of current vs. target weights side by side
- **How rebalancing works** — in-app step-by-step explanation of the rebalancing logic
 
---
 
## Tech Stack
 
| Layer | Tools |
|---|---|
| Data ingestion | `yfinance` (live OHLCV for IDX tickers) |
| Data processing | `pandas`, `numpy` |
| Scheduling | `schedule` or Streamlit rerun |
| Web dashboard | `Streamlit` |
| Frontend demo | Vanilla HTML / CSS / JS |
 
---
 
## How It Works
 
```
yfinance → fetch current prices for portfolio tickers
pandas   → compute current market-value weights
numpy    → calculate drift = current_weight - target_weight
           if |drift| >= threshold → flag for rebalance
           trade_value = |drift| / 100 x portfolio_value
Streamlit → interactive dashboard with settings and log
```
 
**Rebalance logic:**
 
```python
for asset in portfolio:
    drift = asset.current_weight - asset.target_weight
    if abs(drift) >= threshold:
        action = 'SELL' if drift > 0 else 'BUY'
        trade_value = abs(drift) / 100 * portfolio_value
        execute_trade(asset, action, trade_value)
        asset.current_weight = asset.target_weight  # restore to target
```
 
---
 
## Setup & Usage
 
```bash
# Clone the repo
git clone https://github.com/<your-username>/idx-portfolio-balancer.git
cd idx-portfolio-balancer
 
# Install dependencies
pip install yfinance pandas numpy streamlit schedule
 
# Run the dashboard
streamlit run balancer.py
```
 
Open `http://localhost:8501`. Set your target weights, drift threshold, and portfolio size, then click **Rebalance now** or use **Simulate market drift** to test the engine.
 
---
 
## Project Structure
 
```
idx-portfolio-balancer/
├── balancer.py          # Main Streamlit app
├── portfolio/
│   ├── config.py        # Target weights and ticker definitions
│   └── engine.py        # Drift detection and trade calculation logic
├── scheduler/
│   └── monitor.py       # Periodic check loop
├── demo/
│   └── idx_portfolio_balancer.html   # Standalone frontend demo
└── README.md
```
 
---
 
## Example Output
 
```
[08:00] Portfolio monitor started
[08:00] Checking 5 assets against target weights...
[08:01] BBCA: +3.2% drift — within 5% threshold
[08:01] BBRI: -2.9% drift — within 5% threshold
[08:01] No rebalance needed. Next check in 8 hours.
 
[16:01] Rebalance triggered — threshold: 5%, portfolio: Rp 100,000,000
[16:01] BBCA: +6.1% drift → SELL Rp 6,100,000
[16:01] BBRI: -5.8% drift → BUY Rp 5,800,000
[16:01] Rebalance complete. All positions restored to target weights.
```
 
---
 
## Disclaimer
 
Data shown in the demo is simulated for portfolio/demonstration purposes only. This tool does not constitute financial advice.
 
---
