# portswigger-sqli-notes

# 🛡️ PortSwigger SQL Injection Notes & Writeups

This repository documents my hands-on learning journey through the **PortSwigger Web Security Academy**, focused specifically on **SQL Injection vulnerabilities**.

The goal of this repo is to demonstrate practical understanding of:
- How SQL Injection attacks work
- How to identify vulnerable parameters
- How to exploit SQL Injection in controlled lab environments
- How to map offensive techniques to defensive mitigations

---

## 🎯 Purpose of This Repository

This is not just a collection of notes — it is a structured breakdown of real-world web application vulnerabilities.

It reflects my ability to:
- Analyze web application behavior
- Build and test payloads in controlled environments
- Understand database interaction at a functional level
- Document technical findings clearly and consistently

---

## 🧠 Topics Covered

### 🔹 Basic SQL Injection
- Identifying injectable parameters
- Using `' OR 1=1--` style authentication bypass
- Understanding query structure

### 🔹 UNION-Based SQL Injection
- Determining number of columns using `ORDER BY`
- Extracting data using `UNION SELECT`
- Identifying database schema information

### 🔹 Blind SQL Injection
- Boolean-based inference techniques
- Time-based payload testing
- Response analysis and validation

### 🔹 Authentication Bypass Techniques
- Login form manipulation
- Query logic exploitation
- Session-related weaknesses

---

## 🧪 Example Methodology (Lab Workflow)

When approaching a SQL Injection lab, my process typically includes:

1. **Identify Input Vectors**
   - URL parameters
   - Form fields
   - Cookies / headers

2. **Test for Injection**
   - `'`
   - `"`
   - `' OR 1=1--`

3. **Determine Query Structure**
   - Use `ORDER BY` to find column count
   - Use `UNION SELECT` to test output reflection

4. **Extract Data**
   - Database version
   - Table names
   - User credentials (in lab environments)

5. **Validate Results**
   - Confirm output changes
   - Observe response behavior differences

---

## 🧰 Tools Used

- Burp Suite (request interception & manipulation)
- Browser Dev Tools
- PortSwigger Web Security Academy Labs
- Manual payload crafting

---

## 🛡️ Security Takeaways (Defensive Perspective)

For each vulnerability explored, I also document mitigations:

- Use of **parameterized queries (prepared statements)**
- Strict **input validation & sanitization**
- Least-privilege database accounts
- Suppressing detailed database error messages
- Web Application Firewall (WAF) rules as secondary defense

---

## 🔗 Skills Demonstrated

- Web Application Security Testing
- SQL Injection Exploitation Techniques
- Security Thinking & Threat Modeling
- Technical Documentation
- Problem Solving in Controlled Environments

---

## 🚀 Professional Goal

This repository is part of my ongoing development toward a career in:

- Cybersecurity Analysis
- Security Operations (SOC)
- Web Application Security
- Vulnerability Management

I am actively building hands-on experience through structured labs and self-directed security research.

---

## 📌 Disclaimer

All testing and exploitation described in this repository was performed in **legal, controlled training environments provided by PortSwigger Web Security Academy**.

No real-world systems were accessed or affected.