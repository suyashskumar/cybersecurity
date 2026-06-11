# SQLMap: The Basics

A tool to detect and exploit SQL injection vulnerabilities.

## Flags

| Flag | Description |
| :--- | :--- |
| --help | For help |
| --wizard | Good for beginners to avoid adding flags manually |
| --dbs | Helps extract all database names |
| -D DATABASE_NAME --tables | Extract information about the tables in a particular database |
| -D DATABASE_NAME -T TABLE_NAME --dump | Enumerate the records in those tables |
| -u | Test the URL with a GET parameter to check for SQLi vulnerability |
| --cookie | Cookie-based testing of the URL for SQLi vulnerability. Usage: --cookie="SESSIONID=session-id". Can also use PHPSESSID and JSESSIONID |
| --level | For in-depth scans. Usage: --level=number E.g.: --level=5|

**Usage**: sqlmap -u 'URL-WITH-GET-PARAM' --flag
**Note**: -u 'URL' is an important flag.