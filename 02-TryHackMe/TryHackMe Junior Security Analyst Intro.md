
**Date:** 2026-04-07

**Status:** ✅ Completed

**Category:** #BlueTeam #SOC #IncidentResponse #Frameworks

## 🚩 Room Overview

This room provides a foundational look at the role of a **Security Operations Center (SOC)** Analyst. It focuses on how alerts are generated, triaged, and handled using industry-standard frameworks.

---

## 🧠 Key Concepts

### 1. The SOC Workflow

A SOC is the centralized unit that deals with security issues. The workflow generally follows this path:

- **Ingestion:** Collecting logs from firewalls, endpoints (EDR), and servers.
    
- **Detection:** SIEM (Security Information and Event Management) rules trigger alerts based on suspicious logs.
    
- **Analysis:** The Junior Analyst (Tier 1) determines if the alert is a **True Positive** or a **False Positive**.
    
- **Remediation:** If it's a real threat, the incident is escalated or contained.
    

### 2. Core Frameworks to Memorize

|**Framework**|**Purpose**|
|---|---|
|**Cyber Kill Chain**|Lockheed Martin's 7-stage model of an external attack (Recon to Actions on Objectives).|
|**MITRE ATT&CK**|A massive, living database of real-world adversary **Tactics, Techniques, and Procedures (TTPs)**.|
|**Diamond Model**|Used for clustering events and understanding the relationship between **Adversary, Capability, Infrastructure, and Victim**.|

---

## 🛠️ Analysis Tools

The room introduced basic log analysis and alert triaging.

- **SIEM:** The "Brain" of the SOC (e.g., Splunk, ELK, Sentinel).
    
- **Log Sources:** Windows Event Logs, Syslog, Web Traffic (HTTP) logs.
    

---

## 📝 Critical Takeaways for the Vault

### The Difference Between True and False Positives

> [!IMPORTANT]
> 
> - **True Positive (TP):** An alert that correctly identifies a real security incident (e.g., actual malware found).
>     
> - **False Positive (FP):** A legitimate action that incorrectly triggers an alert (e.g., a developer using a tool that looks like a scanner).
>     
> - **True Negative (TN):** No alert is fired, and no malicious activity is happening.
>     
> - **False Negative (FN):** The "Silent Killer"—malicious activity is happening, but the system failed to alert.
>     

---

## 💡 Practical Application (SQLi Connection)

While the PortSwigger labs focus on **how** to attack, this room explains **how we see it** from the other side.

- **Attack:** Using Hex entities in an XML stock check.
    
- **Detection:** An analyst would see an unusual `Content-Type: application/xml` coming from a source that usually sends form data, or they might see `&#x` characters in a WAF log.

