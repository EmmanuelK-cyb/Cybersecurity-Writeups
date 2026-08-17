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














