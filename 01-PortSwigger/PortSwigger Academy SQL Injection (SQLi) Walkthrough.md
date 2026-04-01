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
