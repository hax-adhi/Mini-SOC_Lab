# 🧩 Setup Steps — SOC Project 1: Centralized Log Collection Using Splunk, Logstash, and Winlogbeat

## 1. Objective
Set up a centralized log collection system that gathers Windows Event Logs and sends them to Splunk for visibility and analysis.

---

## 2. Environment Setup
| Component | Purpose | Host OS |
|------------|----------|---------|
| Splunk Enterprise | SIEM platform for log analysis | Kali Linux |
| Logstash | Log parsing and forwarding | Kali Linux |
| Winlogbeat | Log shipper from Windows | Windows 10 |

---

## 3. Installation Steps

### 🧠 Step 1 — Install Splunk Enterprise on Kali
1. Download Splunk Enterprise `.deb` from [Splunk Downloads](https://www.splunk.com/en_us/download.html).  
2. Install and enable:
   ```bash
   sudo dpkg -i splunk*.deb
   sudo /opt/splunk/bin/splunk enable boot-start --accept-license --answer-yes
   sudo /opt/splunk/bin/splunk start
3.Access Splunk Web at:
👉 http://localhost:8000 (Default creds: admin / yourpassword)

## 🧩 Step 2 — Install Logstash

1.Install Logstash:
sudo apt install logstash

2. Create a pipeline config /etc/logstash/conf.d/logstash.conf:

   input {
  beats {
    port => 5044
  }
}

filter {
  # Add filters if needed (e.g., grok or mutate)
}

output {
  tcp {
    host => "127.0.0.1"
    port => 9997
  }
}


## 💻 Step 3 — Install and Configure Winlogbeat

1.Download from Elastic Winlogbeat
2.winlogbeat.event_logs:
  - name: Application
  - name: Security
  - name: System

output.logstash:
  hosts: ["<KALI_IP>:5044"]

3.Run as Administrator:
.\install-service-winlogbeat.ps1
Start-Service winlogbeat

## 🧮 Step 4 — Configure Splunk to Receive Data

1.In Splunk Web:

Go to Settings → Data Inputs → TCP → New Local TCP

Port: 9997

Source type: WinEventLog

Save and enable input.

2.Verify data ingestion:
index=* host=windows*

## 4. Validation

✅ Check Logstash logs:
sudo journalctl -u logstash
✅ Check Winlogbeat logs:
C:\ProgramData\winlogbeat\logs\winlogbeat.log
✅ Confirm data visible in Splunk Search dashboard.

## 5. Outcome
   
Successfully configured a working log pipeline:

Windows → Winlogbeat → Logstash → Splunk


---

### 📄 **docs/results_and_analysis.md**
```markdown
# 📊 Results and Analysis — SOC Project 1

## 1. Summary
After successful setup, Windows event logs were collected and visualized in Splunk dashboards. This established a baseline for monitoring endpoint activity in a simulated SOC environment.

---

## 2. Logs Collected
| Log Type | Purpose |
|-----------|----------|
| **Security** | Logon attempts, privilege changes |
| **System** | Service start/stop events |
| **Application** | Software errors and crashes |

---

## 3. Key Observations
- Splunk received continuous data flow from Winlogbeat via Logstash.
- Security events (4624, 4625, 4672) were visible in near real-time.
- SPL queries enabled filtering by event ID and host:
  ```spl
  index=* sourcetype="WinEventLog:Security" EventCode=4625


- Custom dashboard widgets were created for:

- Failed logons by username

- Top event sources

- System reboot patterns

## 4. Dashboards and Visualization

Example panels included:

Failed vs. Successful Logons

Service Changes (7036 events)

Event Volume Over Time

5. Results

Logs were parsed successfully and indexed correctly.

Splunk confirmed visibility into endpoint activities.

Validated event integrity and timestamp consistency.

6. Learnings

Importance of correct port configuration between Logstash and Splunk.

Understanding Windows Event IDs.

Using SPL for correlation and visualization.




















