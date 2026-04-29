---
case_id: PHISH-2026-04-001
threat_type: Smishing / Recruitment Scam
target: Personal Mobile / Recruitment Identity
reporter: User
status: 🔴 Blocked/Reported
date: 2026-04-29
---

# Phishing Incident Report: Recruitment "Task" Scam

## 1. Executive Summary
- **Incident Overview:** Received an unsolicited SMS claiming to be from "Sophia," a recruiting specialist at **Indeed**.
- **The Lure:** High-paying remote work ($300-$700/day) for minimal hours (60-90 mins/day).
- **The Goal:** Likely a "Task Scam" designed to harvest PII or solicit "deposits" for premium tasks.

## 2. Artifacts & Indicators (IoCs)
| Type | Value | Notes |
| :--- | :--- | :--- |
| **Phone Number** | `1-505-218-5657` | Spoofed New Mexico number. |
| **Email Address** | `teresamedoza-sp@outlook.com` | Non-Indeed domain; misspelled name. |
| **Claimed Identity** | Sophia (Indeed Recruiter) | Impersonation of a legitimate platform. |
| **Attack Vector** | SMS (Smishing) | Bypasses corporate email filters. |

## 3. Analysis of Red Flags
1. **Economic Impossibility:** The hourly rate ($200-$450/hr) is inconsistent with entry-level remote work.
2. **Domain Mismatch:** Recruiter using `@outlook.com` instead of `@indeed.com`.
3. **Platform Shifting:** Texting a personal number to potentially move to WhatsApp/Telegram.
4. **Urgency/Low Barrier:** Offering high pay for extremely low time commitment to trigger FOMO (Fear Of Missing Out).

## 4. Response Actions Taken
- [x] Initial analysis of headers/sender info.
- [x] Verification of legitimate Indeed communication channels.
- [x] Blocked sender phone number.
- [x] Flagged/Reported to Indeed Security.
- [x] Documented in Obsidian Vault for TTP tracking.

## 5. Related TTPs (MITRE ATT&CK)
- **T1566.002:** Phishing: Spearphishing Link (potential).
- **T1586.002:** Compromise Accounts: Email Accounts.
- **T1591.002:** Gather Victim Network Information: Business Relationships.

---
[[Social-Engineering-Master-Index|Back to SE Index]] | [[LetsDefend-Master-Index|Blue Team Study Notes]]