# 🧪 Lab: Retrieve Hidden Data (SQL Injection)

## 📌 Category
SQL Injection – Basic

## 🔗 Lab Link
https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

---

## 🎯 Objective
Retrieve hidden products that are not intended to be displayed by manipulating the SQL query.

---

## 🔍 Recon
- Target: Product category filter
- Parameter: `category`
- Example request: /filter?category=Gifts

- Observation:
- Only certain products are shown
- Hidden/unreleased products are excluded

---

## ⚙️ Vulnerability
```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;