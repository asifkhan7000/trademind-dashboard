# TradeMind AI - Cloud Dashboard

Real-time trading dashboard hosted on Netlify that auto-updates when your bot trades.

## 🏗️ Architecture

```
Your Laptop (Bot Running)              Netlify (Cloud)
┌─────────────────────────┐           ┌─────────────────────────┐
│ TradeMind Bot           │           │ Static Dashboard        │
│         ↓               │           │         ↓               │
│ SQLite Database         │  push     │ index.html              │
│         ↓               │ ────────► │         ↓               │
│ trademind_exporter.py   │  to       │ data.json               │
│         ↓               │  GitHub   │         ↓               │
│ data.json               │           │ Reads & Displays        │
└─────────────────────────┘           └─────────────────────────┘
```

## 📁 Files

| File | Description |
|------|-------------|
| `index.html` | Dashboard UI (deploy to Netlify) |
| `data.json` | Stats data (auto-updated by exporter) |
| `trademind_exporter.py` | Runs on laptop, exports DB to JSON |

---

## 🚀 Setup Instructions

### Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Create repo: `trademind-dashboard`
3. Make it **Public** (required for free Netlify)
4. Clone to your laptop:

```powershell
cd C:\Users\lab\Desktop\asif
git clone https://github.com/YOUR_USERNAME/trademind-dashboard.git
```

### Step 2: Add Dashboard Files

1. Copy `index.html` and `data.json` to the cloned repo folder
2. Push to GitHub:

```powershell
cd trademind-dashboard
git add .
git commit -m "Initial dashboard"
git push
```

### Step 3: Deploy to Netlify

1. Go to https://app.netlify.com
2. Click "Add new site" → "Import an existing project"
3. Connect your GitHub account
4. Select `trademind-dashboard` repo
5. Click "Deploy site"
6. Your dashboard is now live at: `https://YOUR-SITE.netlify.app`

### Step 4: Setup Auto-Sync on Laptop

1. Copy `trademind_exporter.py` to your bot folder:
   ```
   C:\Users\lab\Desktop\asif\forex-trademind-llm\trademind_exporter.py
   ```

2. Edit the config in `trademind_exporter.py`:
   ```python
   GITHUB_REPO_PATH = r"C:\Users\lab\Desktop\asif\trademind-dashboard"
   AUTO_PUSH_TO_GITHUB = True
   ```

3. Run alongside your bot:
   ```powershell
   python trademind_exporter.py
   ```

### Step 5: Auto-Start Both Scripts

Create `start_all.bat` in Startup folder:

```batch
@echo off
echo Starting TradeMind System...

:: Start MT5
start "" "C:\Program Files\MetaTrader 5 EXNESS\terminal64.exe"

:: Wait for MT5
timeout /t 30

:: Start Bot
cd /d C:\Users\lab\Desktop\asif\forex-trademind-llm
start "TradeMind Bot" python trademind_ai_learning_v4.1_final.py

:: Wait a bit
timeout /t 10

:: Start Exporter
start "TradeMind Exporter" python trademind_exporter.py
```

---

## 🔄 How It Works

1. **Bot runs** → Makes trades → Updates SQLite database
2. **Exporter runs** → Every 5 mins, reads DB → Creates `data.json`
3. **Exporter pushes** → `data.json` to GitHub
4. **Netlify detects** → GitHub change → Auto-deploys
5. **Dashboard updates** → Shows new data within 1-2 minutes

---

## 📱 Access From Anywhere

Your dashboard URL: `https://YOUR-SITE.netlify.app`

- ✅ Works on phone, tablet, any browser
- ✅ No login required
- ✅ Auto-refreshes every 60 seconds
- ✅ Updates within 5-7 minutes of trade

---

## ⚙️ Configuration Options

In `trademind_exporter.py`:

```python
EXPORT_INTERVAL = 300      # Export every 5 minutes
AUTO_PUSH_TO_GITHUB = True # Set False to disable auto-push
```

---

## 🔒 Security Notes

- Dashboard is **public** (anyone with URL can see)
- Only shows trading stats, no sensitive data
- For private dashboard, use Netlify password protection ($9/month)

---

## 🐛 Troubleshooting

**Dashboard shows "Waiting for Trades"**
- Normal if market is closed
- Check if bot has made any trades

**Data not updating**
- Check if exporter is running
- Check GitHub repo for recent commits
- Check Netlify deploy logs

**Git push fails**
- Run: `git config --global user.email "you@example.com"`
- Run: `git config --global user.name "Your Name"`
- Make sure you have push access to repo

---

## 📊 What You'll See

| Section | Description |
|---------|-------------|
| Header Stats | Today/Week/Total P&L |
| Total Trades | Count with today's trades |
| Win Rate | Animated ring chart |
| Equity Curve | Line chart of balance over time |
| Recent Trades | Last 8 trades with P&L |
| Neural Learning | AI-discovered patterns |
| Strategy Performance | Win rate per strategy |

---

Made with ❤️ for ASIF's TradeMind AI Bot
