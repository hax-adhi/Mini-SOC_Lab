# 🧠 SOC Project 1: Centralized Log Collection Using Splunk, Logstash, and Winlogbeat

## 🎯 Objective
To design and deploy a centralized log collection system that aggregates Windows event logs into Splunk for real-time visibility and security monitoring.

## 🧩 Lab Environment
| Component | Description |
|------------|-------------|
| **Splunk Enterprise** | Installed on Kali Linux VM (192.168.x.x) |
| **Logstash** | Used for data parsing and forwarding |
| **Winlogbeat** | Installed on Windows 10 VM to forward logs |
| **OS** | Kali Linux, Windows 10 Pro |

## ⚙️ Configuration Steps
1. Installed Splunk Enterprise on Kali and verified web access at `http://localhost:8000`.
2. Installed and configured **Winlogbeat** on Windows to collect:
   - Security Logs
   - System Logs
   - Application Logs
3. Configured **Logstash pipeline (`logstash.conf`)** to forward data from Winlogbeat to Splunk.
4. Verified log ingestion using Splunk Search:
   ```spl
   index=* host=windows*
