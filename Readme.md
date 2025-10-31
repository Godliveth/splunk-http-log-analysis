# Splunk Basics – HTTP Log Analysis  
*Day 21 of  #30DaysOfSOC Challenge*

---

## 📘 Overview  

This project demonstrates how to ingest and analyze **HTTP logs** in **Splunk** to simulate web activity monitoring and detection.  
The analysis focuses on identifying **client/server errors**, **suspicious User-Agents**, and **large file transfers** that could indicate abnormal behavior.

---

## 🎯 Objectives  

In this lab, I:  
- Ingested JSON-formatted Zeek-style HTTP logs into Splunk.  
- Detected **client and server errors** (4xx & 5xx).  
- Identified **suspicious automated User-Agents** (e.g., curl, sqlmap, python-requests).  
- Flagged **large file transfers** (>500 KB) and abnormal URIs.

---

## 🧰 Lab Setup  

| Component | Details |
|------------|----------|
| **Tool** | Splunk Enterprise |
| **Dataset** | Synthetic Zeek HTTP Logs (JSON format) |
| **Index** | `"main"`|
| **Sourcetype** | `_json` or `zeek:http` |
| **Data Source** | [HTTP Log file](https://raw.githubusercontent.com/0xrajneesh/30-Days-SOC-Challenge-Beginner/refs/heads/main/http_logs.json) |

---
# 📂Data Set

📥 [Download synthetic_zeek_http.json](./synthetic_zeek_http.json)).

---

## ⚙️ Steps to Upload HTTP Log into Splunk  

1. Go to **Settings → Add Data**.  
2. Choose **Upload**, and select `synthetic_zeek_http.json`.  
3. Set **Source type** to `_json` (or create `zeek:http`).  
4. Choose **Index** → `"main"` or  `"http_lab"`.  
5. Complete the upload and verify indexing via **Search & Reporting**.


---

## 🔍 SPL Queries  

### ✅ Task 1: Top 10 endpoints generating web traffic  

spl                                                    

index="main" sourcetype="_json"
| stats count by "id.orig_h"
| sort -count
| head 10

---

✅ Task 2: Count the number of server errors (5xx) observed

spl

index="main"sourcetype="_json" status_code>=500 status_code<600
| stats count as server_errors

---

✅ Task 3: Identify User-Agents associated with possible scripted attacks

spl

index="main" sourcetype="_json" user_agent IN ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-checker/1.0")
| stats count by user_agent


---

✅ Task 4: Find large file transfers (greater than 500 KB)

spl

index="main" sourcetype="_json" resp_body_len>500000
| table ts "id.orig_h" "id.resp_h" uri resp_body_len
| sort -resp_body_len

---

## 📸 Screenshots
All screenshots from this analysis are stored in the folder below 👇


📸 **[🔗 View Screenshot Folder](./screenshots)**

---

## 📦 Project Files
File	Description
synthetic_zeek_http.json	HTTP log dataset used for ingestion
screenshots/	Folder containing all analysis screenshots


---

## 🧠 Key Takeaways
Gained hands-on experience with HTTP log analysis using Splunk.

Learned to identify error trends, automation indicators, and data exfiltration attempts.

Reinforced core SOC analytical skills using Splunk SPL.









---


🧩 Author

👤 Godliveth Madu

SOC Analyst Trainee 

🔗 LinkedIn
www.linkedin.com/in/godliveth-madu-1771b6251
