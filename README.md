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



