# Email-Siem-Automation
Python-based  Email Monitoring and Splunk SIEM Integration


# Email SIEM Automation & Splunk Integration

## Overview

I built this project to understand how an automated email-monitoring workflow can feed security events into a SIEM.

The workflow connects:

![work_flow_diagraml Script](screenshots/01-send-email.png)

The Python components run inside a Windows 10 VirtualBox VM, while Splunk Enterprise runs on the host machine.

This project also gave me hands-on experience with VM networking, HTTPS/TLS, Splunk HEC, event ingestion, and SPL searches.

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| Python | Email monitoring and automation |
| Gmail | Email source |
| Splunk Enterprise | SIEM |
| Splunk HTTP Event Collector (HEC) | Event ingestion |
| VirtualBox | Virtualization |
| Windows 10 | Python environment |
| VS Code | Development |
| VirusTotal API | URL analysis |

---

## Project Architecture

```text
Gmail
  │
  ▼
Monitor_Email.py
  │
  ├── Email monitoring
  ├── Email parsing
  ├── Metadata extraction
  └── URL extraction / analysis
  │
  ▼
Splunk HTTP Event Collector
  │
  ▼
Splunk Enterprise
  │
  ▼
SPL Search & Analysis
```

---

## 1. Test Email Generation

I created controlled test emails using `Send_Email.py` to provide data for the monitoring workflow.

![Send Email Script](screenshots/01-send-email.png)

---

## 2. Email Monitoring

`Monitor_Email.py` monitors the configured mailbox and processes incoming messages.

The script extracts relevant information such as email metadata, message content, URLs, and attachments.

![Email Monitoring](screenshots/02-monitor-email.png)

---

## 3. VM-to-Host Connectivity

The Python environment was running inside my Windows 10 VM while Splunk Enterprise was running on my host machine.

I initially configured the connection using `localhost`. Since the Python script was running inside the VM, I changed the configuration to use the host machine's reachable IP address.

I then tested connectivity to the Splunk HEC endpoint on port `8088`.

```powershell
Test-NetConnection <SPLUNK_HOST_IP> -Port 8088
```

![Splunk Connectivity Test](screenshots/03-splunk-connectivity.png)

---

## 4. URL Extraction & Analysis

The monitoring workflow extracts URLs from email messages and includes VirusTotal API functionality for URL analysis.

![URL Analysis](screenshots/04-url-analysis.png)

---

## 5. Splunk HEC Integration

I configured the monitoring script to forward processed email events to Splunk through the HTTP Event Collector (HEC) over HTTPS.

![Splunk HEC Event Transmission](screenshots/05-events-sent-to-splunk.png)

---

## 6. Splunk Event Verification

The events were configured with the following metadata:

```text
source = email_monitor
sourcetype = email_event
```

I used the following SPL search to locate the events:

```spl
source="email_monitor" sourcetype="email_event"
```

![Splunk Events](screenshots/06-splunk-events.png)

---

## Final Workflow

```text
Test Email
    ↓
Gmail
    ↓
Monitor_Email.py
    ↓
Email & URL Processing
    ↓
Splunk HEC
    ↓
Splunk Enterprise
    ↓
SPL Search
    ↓
Security Analysis
```

---

## Key Takeaways

- Built a Python-based email monitoring workflow.
- Integrated Python with Splunk HEC.
- Worked with VM-to-host networking.
- Used HTTPS for SIEM event transmission.
- Worked with Splunk event metadata.
- Used SPL to locate and verify ingested events.
- Validated the end-to-end security-event pipeline.
- Gained practical experience troubleshooting a multi-layer SIEM integration.

---

## Security Considerations

Sensitive credentials and API keys were stored in `.env` rather than directly in the Python scripts.

The local Splunk instance used a self-signed certificate. For this controlled laboratory environment, certificate verification was disabled using:

```python
verify=False
```

For a production environment, certificate verification should remain enabled and the appropriate trusted CA certificate should be configured.

---

## Project Structure

```text
email-siem-project/
│
├── README.md
├── Send_Email.py
├── Monitor_Email.py
├── .gitignore
│
└── screenshots/
    ├── 01-send-email.png
    ├── 02-monitor-email.png
    ├── 03-splunk-connectivity.png
    ├── 04-url-analysis.png
    ├── 05-events-sent-to-splunk.png
    └── 06-splunk-events.png
```

---

## Project Status

**Completed and successfully tested.**
