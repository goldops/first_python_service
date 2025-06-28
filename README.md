---

```markdown
# Flask App Systemd Service Setup

This guide explains how to run a Python Flask application as a systemd service using a virtual environment.

---

## 🧱 Project Structure Example

```

/home/ubuntu/myapp/
├── app.py
├── venv/                  # Virtual environment with Flask installed
└── python.service         # Systemd unit file (optional, or placed in /etc/systemd/system)

````

---

## ✅ 1. Set Up Python Virtual Environment

```bash
cd /home/ubuntu/myapp
python3.11 -m venv venv
source venv/bin/activate
pip install flask
````

Confirm Flask is installed:

```bash
venv/bin/python3.11 -m pip show flask
```

---

## ⚙️ 2. Create systemd Unit File

Create and edit the unit file:

```bash
sudo nano /etc/systemd/system/python.service
```

Paste the following (adjust paths if needed):

```ini
[Unit]
Description=Flask App Service
After=network.target

[Service]
Type=simple
WorkingDirectory=/home/ubuntu/myapp
ExecStart=/home/ubuntu/myapp/venv/bin/python3.11 app.py
Restart=always
RestartSec=5

# Optional Logging
StandardOutput=journal
StandardError=journal

# Optional: Security and Resource Limits
# User=webapp
# Group=webapp
# MemoryLimit=256M
# CPUQuota=50%

[Install]
WantedBy=multi-user.target
```

---

## 🔄 3. Reload systemd and Start the Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable python.service
sudo systemctl start python.service
```

Check status:

```bash
sudo systemctl status python.service
```

---

## 🔍 4. Troubleshooting

* Check logs:

  ```bash
  journalctl -u python.service --no-pager -e
  ```
* Manually test:

  ```bash
  /home/ubuntu/myapp/venv/bin/python3.11 app.py
  ```

---

## 🧼 5. Updating the App

If you change `app.py` or update dependencies:

```bash
# If code changes
sudo systemctl restart python.service

# If Python dependencies change
source venv/bin/activate
pip install -r requirements.txt
```

---

## 🛑 6. Stopping and Disabling the Service

```bash
sudo systemctl stop python.service
sudo systemctl disable python.service
```

---

## ✅ Done

Your Flask app is now running as a resilient background service managed by `systemd`.

