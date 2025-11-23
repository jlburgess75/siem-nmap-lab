🛡️ Raspberry Pi Mini-SIEM
Real-Time Security Monitoring • SSH Brute-Force Detection • USB Event Alerts • Telegram Notifications

Powered by: Python · Raspberry Pi OS · syslog · auth.log · Nmap · Telegram Bot API

📌 Project Overview

This project transforms a Raspberry Pi into a mini SIEM capable of real-time:

Log monitoring

SSH brute-force detection

Invalid user login detection

USB device event detection

Telegram alert notifications

Local security event logging

Recon → Attack → Detection → Alerting workflow

This is a complete, hands-on SOC Tier-1 incident detection lab, using real tools, real logs, and real attack evidence.

🧰 Technologies Used
Component	Purpose
Python 3	SIEM logic, log parsing, alerting
Raspberry Pi OS	Host + logs
/var/log/auth.log	SSH/Authentication events
/var/log/syslog	USB + kernel events
Nmap	Reconnaissance
SSH brute-force attempts	Attack simulation
Telegram Bot API	Real-time alert delivery
🏗️ System Architecture
┌──────────────────────────┐           ┌────────────────────────┐
│ Windows Laptop (Attacker)│           │    Telegram (Analyst) │
│--------------------------│           │------------------------│
│ • Nmap scanning          │           │ • Receives alerts      │
│ • SSH brute-force        │           │ • Real-time monitoring │
└───────────────┬──────────┘           └──────────────┬────────┘
                │ Attack Traffic                     │
                ▼                                    │
        ┌──────────────────────────┐                 │
        │   Raspberry Pi SIEM      │────────────────┘
        │---------------------------│
        │ • Monitors auth.log       │
        │ • Monitors syslog         │
        │ • Detects anomalies       │
        │ • Sends Telegram alerts   │
        └───────────────────────────┘

📂 Repository Structure
siem-nmap-lab/
│
├── pi_security_monitor.py
├── README.md
├── config/
│   └── .gitignore   (optional, to hide telegram.env)
└── docs/
    └── images/
        ├── nmap-recon-1.png
        ├── nmap-recon-2.png
        ├── ssh-bruteforce-1.png
        ├── ssh-bruteforce-2.png
        ├── pi-siem-detect.png
        ├── telegram-alerts-bruteforce.png
        └── usb-alert.png

⚙️ Installation & Setup
1️⃣ Install dependencies
sudo apt update
sudo apt install python3 python3-pip nmap -y
pip3 install requests

2️⃣ Configure Telegram alerts

Create a Telegram bot:

Open Telegram

Search @BotFather

Run /newbot

Save your BOT_TOKEN

Send any message to your bot

Get your chat ID:

https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates

3️⃣ Create configuration file (DO NOT upload to GitHub)

config/telegram.env:

BOT_TOKEN=YOUR_BOT_TOKEN
CHAT_ID=YOUR_CHAT_ID

🚨 SIEM Script (Python)



🔥 Live Attack Simulation (Full Evidence)

This project includes real attacker screenshots and real SIEM detections.

1️⃣ Reconnaissance Phase — Nmap Scan

Attacker scans the Pi:

nmap -Pn 10.0.0.190


Findings:

22/tcp   open  ssh
80/tcp   open  http
8000/tcp open  http-alt
MAC Address: D8:3A:DD:D5:F0:22 (Raspberry Pi Trading)


📸 Screenshots:

docs/images/nmap-recon-1.png
docs/images/nmap-recon-2.png

2️⃣ SSH Brute-Force Attack Attempt

The attacker tries logging in using a fake username wronguser:

ssh wronguser@10.0.0.190


Output:

wronguser@10.0.0.190's password:
Permission denied, please try again.
Permission denied (publickey,password).


📸 Screenshots:

docs/images/ssh-bruteforce-1.png
docs/images/ssh-bruteforce-2.png

3️⃣ Raspberry Pi SIEM Detection (auth.log & syslog)

The SIEM immediately detects:

[ALERT GENERATED] Invalid user wronguser from 10.0.0.19
Failed SSH login detected
Connection reset by invalid user wronguser


📸 Screenshot:
docs/images/pi-siem-detect.png

4️⃣ Real-Time Telegram Alerts

Every detection instantly triggered a Telegram alert:

[Pi Alert] Invalid user login attempt:
Nov 23 13:12:31 sshd[28184]: Invalid user wronguser from 10.0.0.19


USB device alerts were also triggered:

[Pi Alert] USB device event...


📸 Screenshot:
docs/images/telegram-alerts-bruteforce.png

🧠 MITRE ATT&CK Mapping
Phase	Technique	ID
Reconnaissance	Network Scanning	T1046
Initial Access	Brute Force	T1110
Execution	SSH Attempt	T1059
Detection	Log Analysis	DS0002
Alerting	Automated Notifications	T1020 (Benign)
📝 Incident Summary (SOC-Ready)

On Nov 23, repeated SSH login attempts targeting the Raspberry Pi (10.0.0.190) were detected from 10.0.0.19 using invalid user wronguser.
The SIEM captured authentication failures, connection resets, and flagged brute-force behavior.
Multiple real-time alerts were delivered via Telegram, confirming the effectiveness of the detection pipeline.
