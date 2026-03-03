<div align="center">

# 🖥️ SystemPilot

**Powerful Telegram bot for remote PC monitoring and control**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Telegram](https://img.shields.io/badge/Telegram-Bot_API-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://core.telegram.org/bots/api)
[![License](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge)](LICENSE)
[![Windows](https://img.shields.io/badge/Platform-Windows-0078D4?style=for-the-badge&logo=windows&logoColor=white)](https://microsoft.com/windows)

</div>

---

## 📌 Table of Contents

- [Features](#-features)
- [Installation](#-installation-run-from-source)
- [Build EXE](#-build-executable-windows)
- [Dependencies](#-dependencies)
- [Configuration](#-configuration)
- [Commands & Menu](#-commands--menu)
- [HLS Streaming](#-hls-streaming)
- [Activity Monitoring](#-activity-monitoring)
- [Keylogger](#️-keylogger)
- [File Structure](#-file-structure)
- [Security](#-security)
- [FAQ](#-faq)

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 📡 Remote Monitoring
- 📸 Instant screenshots of all monitors
- 🎥 Live broadcast (screenshots every second)
- 📺 **HLS video stream** viewable in any browser
- 🖥️ Automatic RDP session detection & switching
- 📊 CPU / RAM / Disk / Uptime stats

</td>
<td width="50%">

### 🔍 Activity Monitoring
- 🖱️ Mouse movement and click tracking
- ⌨️ Keyboard keystroke monitoring (keylogger)
- 📈 Hourly and daily activity charts
- 🗂️ Data retention up to 365 days
- 🧠 8 activity classification types

</td>
</tr>
<tr>
<td>

### ⚡ PC Control
- 🔴 Shutdown (immediate / timer)
- 🔄 Restart (immediate / timer)
- 🔒 Lock screen
- ⏰ Flexible timer management

</td>
<td>

### 👥 Administration
- 👑 Role system: Super-Admin + Admins
- 🔑 Access management via Telegram
- 📋 Admin list
- 🛡️ HTTP Basic Auth for HLS stream

</td>
</tr>
</table>

---

## 🚀 Installation (run from source)

### 1. Clone the repository
```bash
git clone https://github.com/reposit0r/systempilot.git
cd systempilot
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure the bot

Open `main.py` and replace the token and super-admin ID:
```python
BOT_TOKEN = "YOUR_BOT_TOKEN_HERE"
SUPER_ADMIN_ID = 123456789  # Your Telegram ID
```

> 💡 Get your bot token from [@BotFather](https://t.me/BotFather) and your Telegram ID from [@userinfobot](https://t.me/userinfobot)

### 4. Run
```bash
python main.py
```

---

## 🔨 Build Executable (Windows)

The bot can be compiled into a standalone `.exe` — no Python installation required on the target machine.

### Requirements

- Python 3.10+ on the **build machine**
- All dependencies installed: `pip install -r requirements.txt`
- PowerShell (built into Windows)

### Build steps

**1.** Open PowerShell and navigate to the bot's root directory:
```powershell
cd "C:\path\to\systempilot"
```

**2.** Run the build script from inside the bot directory:
```powershell
powershell -ExecutionPolicy Bypass -File build_command.txt
```

> ⚠️ The script **must be run from the bot root directory**. The compiled `.exe` and all supporting files will be placed there.

**3.** After a successful build, the directory will contain:
```
systempilot/
├── main.exe                  # Compiled executable
├── ffmpeg.exe                # Copy here before running
├── bot_config.json           # Auto-created on first run
└── ...
```

**4.** Copy the **entire folder** to the target Windows machine and run `main.exe`.

> 💡 `ffmpeg.exe` must be in the same folder as `main.exe` for HLS streaming to work.

---

## 📦 Dependencies

| Library | Purpose | Required |
|---------|---------|:--------:|
| `aiohttp` | Async HTTP requests to Telegram API | ✅ |
| `Pillow` | Image processing and screenshots | ✅ |
| `mss` | Screen capture | ✅ |
| `pynput` | Mouse and keyboard monitoring | ✅ |
| `psutil` | System info (CPU, RAM, disk) | ✅ |
| `matplotlib` | Activity chart generation | ⚠️ Recommended |
| `numpy` | Math for charts | ⚠️ Recommended |
| `ffmpeg` | HLS video streaming (external binary) | 🎬 For HLS |
```bash
pip install aiohttp Pillow mss pynput psutil matplotlib numpy
```

Download `ffmpeg.exe` and place it in the bot root directory.

---

## ⚙️ Configuration

### Core parameters (`main.py`)
```python
# Core settings
BOT_TOKEN = "your_bot_token"
SUPER_ADMIN_ID = 123456789        # Main administrator Telegram ID

# Screen broadcast
BROADCAST_INTERVAL = 1.0          # Screenshot interval (sec)
MAX_BROADCAST_DURATION = 300      # Max broadcast duration (sec)

# Activity monitoring
DEFAULT_MONITOR_INTERVAL = 60     # Screenshot comparison interval (sec)
ACTIVITY_THRESHOLD = 5            # Screen change threshold (%)
MOUSE_INACTIVITY_THRESHOLD = 30   # Mouse inactivity threshold (sec)
KEYBOARD_INACTIVITY_THRESHOLD = 30

# HLS streaming
HLS_PORT = 8080
FFMPEG_FRAMERATE = 10             # FPS
FFMPEG_QUALITY = 25               # CRF (lower = better quality)
```

### Configuration files

| File | Description |
|------|-------------|
| `bot_config.json` | Admin list, auto-start settings |
| `activity_data.json` | Activity statistics database |
| `keylog.txt` | Keystroke log (current session) |
| `hls_auth.json` | HLS stream credentials |
| `bot_errors.log` | Error log (ERROR level and above) |

---

## 📋 Commands & Menu

### Main menu

| Button | Description |
|--------|-------------|
| `🎥 Screen Broadcast` | Manage broadcast (regular / HLS) |
| `🖼 Screenshot` | Instant screenshot of all monitors |
| `📊 System Load` | CPU, RAM, disk, uptime |
| `ℹ️ IP & Device Name` | Local IP, external IP, PC name |
| `⏰ Timers` | Set and manage shutdown timers |
| `🔒 Lock Screen` | Lock Windows workstation |
| `📈 Activity Stats` | Charts and activity statistics |
| `⌨️ Keylogger` | Get keystroke log file |
| `👑 Admin Management` | Add / remove administrators |
| `🔴 Shutdown PC` | Shutdown immediately or on timer |
| `🔄 Restart PC` | Restart immediately or on timer |

### Menu tree
```
/start
└── Main Menu
    ├── 🎥 Screen Broadcast
    │   ├── Start regular broadcast  (1/3/5/10 min)
    │   ├── Stop regular broadcast
    │   ├── Start HLS stream
    │   ├── Stop HLS stream
    │   ├── Get HLS link
    │   ├── Regenerate password
    │   ├── Check RDP status
    │   └── HLS Settings
    │       ├── HLS auto-start: ON/OFF
    │       ├── Upload custom placeholder
    │       └── Restart ffmpeg
    │
    ├── 📈 Activity Statistics
    │   ├── Last 1/3/6/12 hours
    │   ├── Today
    │   ├── Last 3/7/30 days
    │   ├── Custom period
    │   └── Monitoring settings
    │
    └── ⏰ Timers
        ├── Shutdown PC  (5/10/15/30 min / 1-2 h / custom)
        └── Restart PC   (same options)
```

---

## 📺 HLS Streaming

HLS (HTTP Live Streaming) lets you watch the PC screen **in real time** through any browser with no extra software needed.

### How it works
```
 ┌─────────────┐    ┌─────────────┐    ┌──────────────────┐
 │   Desktop   │───▶│   ffmpeg    │───▶│   HTTP server    │
 │             │    │  (HLS enc.) │    │      :8080       │
 └─────────────┘    └─────────────┘    └──────────────────┘
                                                │
                          ┌─────────────────────┤
                          ▼                     ▼
                   ┌──────────┐         ┌─────────────┐
                   │  Video   │         │ Placeholder │
                   │ (RDP on) │         │  (RDP off)  │
                   └──────────┘         └─────────────┘
```

### Auto-switching modes

| RDP state | Stream mode |
|:---------:|:-----------:|
| 🖥️ Visible / focused | 🎥 Live video (10 FPS) |
| 📺 Minimized | 🖼️ Static placeholder |

### Connect to the stream
```bash
# Browser (built-in HLS player)
http://YOUR_IP:8080

# VLC / media player
http://login:password@YOUR_IP:8080/live.m3u8
```

### Custom placeholder image

Send the bot a file named `custom_placeholder.png` (recommended 1280×720) to replace the default placeholder.

---

## 📈 Activity Monitoring

The system classifies activity into **8 types** based on three data sources:

| Type | Screen | Mouse | Keyboard | Description |
|------|:------:|:-----:|:--------:|-------------|
| 🟢 Full | ✅ | ✅ | ✅ | Normal work |
| 🔵 Screen+mouse | ✅ | ✅ | ❌ | Browsing / navigation |
| 🟣 Screen+keyboard | ✅ | ❌ | ✅ | Working without mouse |
| 🟡 Screen only | ✅ | ❌ | ❌ | Suspicious (video?) |
| 🎯 Mouse+keyboard | ❌ | ✅ | ✅ | Background work |
| 🖱 Mouse only | ❌ | ✅ | ❌ | Minimal activity |
| ⌨ Keyboard only | ❌ | ❌ | ✅ | Terminal / background |
| ⚫ Inactive | ❌ | ❌ | ❌ | No activity |

### Custom period input format
```
12           → 12:00–13:00 today
26.02 14     → 14:00–15:00 on Feb 26
13-17        → 13:00–18:00 today
23.02 14-17  → 14:00–18:00 on Feb 23
03.03        → Entire day of Mar 3
0-24         → Entire today
```

---

## ⌨️ Keylogger

The bot logs all keystrokes to `keylog.txt` in real time.

- Special keys appear in brackets: `[Enter]`, `[Backspace]`, `[F5]`, etc.
- The file is sent via the `⌨️ Keylogger` button in the menu
- Keystroke statistics are included in activity charts

> ⚠️ **Important**: only use on devices you own or have explicit permission to monitor.

---

## 📁 File Structure
```
systempilot/
│
├── main.py                   # Main bot source file
├── main.exe                  # Compiled executable (after build)
├── build_command.txt         # PowerShell build script
├── requirements.txt          # Python dependencies
├── ffmpeg.exe                # ffmpeg for Windows (download separately)
│
├── bot_config.json           # Config (auto-created on first run)
├── activity_data.json        # Activity database
├── keylog.txt                # Keystroke log
├── hls_auth.json             # HLS stream credentials
├── bot_errors.log            # Error log
│
├── custom_placeholder.png    # Custom placeholder image (optional)
│
└── hls_stream/               # HLS working files (auto-created)
    ├── hls_stream_*.ts
    ├── hls_stream.m3u8
    └── placeholder/
```

---

## 🛡️ Security

- 🔒 **HTTP Basic Auth** — HLS stream protected by randomly generated credentials
- 🔑 **Telegram ID authorization** — bot only responds to allowed users
- 📝 **Rotating error log** — `bot_errors.log` (10 MB × 3 files, ERROR+ only)
- 🔐 **Dangerous action confirmation** — shutdown/restart require explicit inline confirmation
- 🚫 **Super-Admin protection** — the main administrator cannot be removed

### Regenerate HLS credentials

Use the `🔄 Regenerate password` button in the broadcast menu — new credentials are sent to chat immediately.

---

## ❓ FAQ

<details>
<summary><b>Bot does not respond to commands</b></summary>

Check that `BOT_TOKEN` and `SUPER_ADMIN_ID` are set correctly. Your Telegram ID must match `SUPER_ADMIN_ID`.

</details>

<details>
<summary><b>HLS stream does not work / no video in browser</b></summary>

1. Make sure `ffmpeg.exe` is in the same folder as the bot / exe
2. Port **8080** must not be in use (`netstat -ano | findstr :8080`)
3. Press `🔄 Restart ffmpeg` in HLS settings
4. Make sure the RDP window is visible and not minimized

</details>

<details>
<summary><b>Activity charts are not generated</b></summary>
```bash
pip install matplotlib numpy
```

</details>

<details>
<summary><b>Keystrokes are not being recorded</b></summary>

Make sure `pynput` is installed. Try running the bot or `.exe` as Administrator.

</details>

<details>
<summary><b>Activity data disappeared after restart</b></summary>

Data is stored in `activity_data.json`. Make sure the bot has write permissions in its directory. Statistics are saved every 10 minutes and on shutdown.

</details>

<details>
<summary><b>Build fails or exe crashes on launch</b></summary>

1. Ensure all dependencies are installed in the Python environment used for the build
2. Run PowerShell as Administrator
3. Make sure you are **inside the bot root directory** before running `build_command.txt`
4. Check `bot_errors.log` after the first launch for detailed error info

</details>

---

## 📄 License

MIT License — free to use for personal and commercial purposes.

---

<div align="center">

**Built with ❤️ for remote administration**

*If this project was useful — leave a ⭐*

</div>
