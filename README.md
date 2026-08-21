# Cybersecurity-Writeups
Personal cybersecurity portfolio with detailed write-ups on PortSwigger SQLi labs and future challenges

# Lab 1: SQL Injection – Retrieve Hidden Data

## Objective
Demonstrate how SQL injection can be used to extract hidden information from a vulnerable application.

## Steps Taken
- Navigated to the vulnerable page and identified input fields.
- Tested with basic SQL injection payloads.
- Observed how the application responded to injected queries.
- Used `' OR '1'='1' --` to manipulate the query and reveal hidden data.

## Key Payloads
```sql
' OR '1'='1' --









---

##📄 Lab 2: SQL Injection – Login Bypass
```
# Lab 2: SQL Injection – Login Bypass

## Objective
Show how SQL injection can bypass authentication mechanisms in a login form.

## Steps Taken
- Navigated to the login page.
- Entered test credentials to observe normal behavior.
- Injected SQL payloads into the username field.
- Used `' OR '1'='1' --` to bypass authentication.

## Key Payloads
```sql
' OR '1'='1' --




---
##📄 Lab 3: SQL Injection – UNION Attack 
```

# Lab 3: SQL Injection – UNION Attack (Column Count Discovery)

## Objective
Use the UNION operator to determine the number of columns in the query and prepare for data extraction.

## Steps Taken
- Identified a vulnerable parameter in the product category filter.
- Injected `' UNION SELECT NULL--` to test column compatibility.
- Incrementally added `NULL` values until the query executed successfully.
- Confirmed the number of columns required for a valid UNION SELECT.

## Key Payloads
```sql
' UNION SELECT NULL,NULL--





---

### 📄 Lab 4: SQL Injection – UNION Attack with String Data
```
# Lab 4: SQL Injection – UNION Attack with String Data

## Objective
Demonstrate how to use UNION SELECT with string values to identify which columns can display extracted data.

## Steps Taken
- Used the column count discovered in Lab 3.
- Injected string values into each column to test which ones are reflected in the application output.
- Example: `' UNION SELECT 'abc','def'--`
- Observed which string appeared on the page, confirming the display column.
- Prepared to use that column for extracting sensitive data.

## Key Payloads
```sql
' UNION SELECT 'abc','def'--



### 🧪 Lab 05: SQL injection UNION attack, retrieving data from other tables

- **Category:** SQL Injection (UNION Attacks)
- **Difficulty:** Practitioner 🟢
- **Status:** Completed ✅

#### 🎯 Objective
Retrieve sensitive data (usernames and passwords) from a separate database table (`users`) and log in as the administrator user.

#### 💡 Key Concept Learned
* **Cross-Table Querying:** Using a `UNION` attack to access arbitrary tables within the database schema, provided you map out the matching column numbers and data types of the original query.

#### 🛠️ Exploit Methodology
1. **Determine Column Count:**
   Used `ORDER BY` clauses to find the column limit. Discovered the query returns 2 columns.
2. **Verify Text Support:**
   Tested if both columns accept string data:
   ```sql
   ' UNION SELECT 'a', 'b'--
   ```
3. **Extract Credentials:**
   Targeted the `users` table to pull the `username` and `password` columns directly into the visible product grid:
   ```sql
   ' UNION SELECT username, password FROM users--
   ```
4. **Takeover:** Located the `administrator` account credentials in the server response and successfully authenticated.

---






### 🧪 Lab 06: SQL injection UNION attack, retrieving multiple values in a single column

- **Category:** SQL Injection (UNION Attacks)
- **Difficulty:** Practitioner 🟢
- **Status:** Completed ✅

#### 🎯 Objective
Bypass data type restrictions where only a single column supports text data, yet multiple fields of information (username and password) must be retrieved.

#### 💡 Key Concept Learned
* **String Concatenation:** When a query limits the number of visible text fields, you can combine multiple data fields into a single string using a custom delimiter character (like `~`) to bypass the column restriction.

#### 🛠️ Exploit Methodology
1. **Identify Single-Column Text Restriction:**
   Discovered that while the query returned 2 columns, only one of them would allow string injection without triggering a database error (`200 OK` on only one side).
2. **Concatenate and Dump:**
   Used the database's string concatenation operator (`||`) to merge the distinct credentials into the single text-compatible column slot:
   ```sql
   ' UNION SELECT NULL, username || '~' || password FROM users--
   ```
3. **Takeover:** Parsed out the administrator's password from the joined `username~password` format string in the HTML output and logged in.

---







### 📄 Lab 07: SQL Injection attack, querying the database type and version on oracle
```
### 🧪 Lab 07: SQL injection attack, querying the database type and version on Oracle

- **Category:** SQL Injection (Examining the Database)
- **Difficulty:** Practitioner 🟢
- **Status:** Completed ✅

#### 🎯 Objective
Identify the database type and version of an Oracle-backed web application by injecting a `UNION` attack vector.

#### 💡 Key Concept Learned
Oracle databases have strict syntax rules that differentiate them from MySQL or PostgreSQL:
1. **The `FROM` Requirement:** Every `SELECT` query must include a `FROM` clause. To select constants or test columns, Oracle provides a built-in dummy table named `DUAL`.
2. **Version Tracking:** System banners and software version information are kept inside the `v$version` table under the `banner` column.

#### 🛠️ Exploit Methodology
1. **Determine Column Count & Types:** 
   Discovered that the database returns 2 columns, both of which accept string data:
   ```sql
   ' UNION SELECT 'a', 'a' FROM dual--
   ```

2. **Extract Database Version:** 
   Queried the Oracle system table to reflect the version banner string onto the page:
   ```sql
   ' UNION SELECT banner, NULL FROM v\$version--
   ```

---















