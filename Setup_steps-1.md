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























