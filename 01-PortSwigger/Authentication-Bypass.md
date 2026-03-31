# 🧪 Lab: Authentication Bypass (SQL Injection)

## 📌 Category
SQL Injection – Authentication Bypass

## 🔗 Lab Link
https://portswigger.net/web-security/sql-injection/authentication/lab-login-bypass

---

## 🎯 Objective
Bypass the login form using SQL injection to authenticate as any user.

---

## 🔍 Recon
- Target URL: `/login`
- Parameters: `username`, `password`
- Observations:
  - Login form does not sanitize input
  - No account lockout mechanism

---

## ⚙️ Vulnerability
```sql
SELECT * FROM users WHERE username = '$username' AND password = '$password';
```

