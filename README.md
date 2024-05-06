# TDE_MSSQL

### Encrypt a User Database:

1. **Create a Master Key**:
   ```sql
   USE master;
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'AYK@123';
2. **Create Create a Certificate**:
 ```sql
   CREATE CERTIFICATE democert WITH SUBJECT = 'my demomy cert subject';
```
3. **Create a Database Encryption Key**:
```sql
 USE mydb;
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_256
ENCRYPTION BY SERVER CERTIFICATE democert;
```
4. **Backup the Certificate and Key**:
 ```sql
   USE master;
BACKUP CERTIFICATE democert
TO FILE = 'F:\dbbackups\democert_cert.cer'
WITH PRIVATE KEY (FILE = 'F:\dbbackups\democert_key.key' , ENCRYPTION BY PASSWORD = 'AYK@1123');
```
5. **Set Encryption ON for the Database**:
   ```sql
   ALTER DATABASE demo SET ENCRYPTION ON;

   ```
6. **Verify Encryption Status**:
 ```sql
SELECT name, database_id, is_encrypted
FROM sys.databases;
```
### Restore an Encrypted Database:
7. **Create a Master Key**:
 ```sql
USE master;
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'aungyekyaw@123';
 ```
8. **Create a Certificate Using the Backup of Source Certificate & Key**:
 ```sql
   CREATE CERTIFICATE democert
   FROM FILE = 'F:\dbbackups\democert_cert.cer'
   WITH PRIVATE KEY (FILE = 'F:\dbbackups\democert_key.key', DECRYPTION BY PASSWORD = 'AYK@1123');
```
9. **Restore the Encrypted Database**:
 ```sql
RESTORE DATABASE mydb
FROM DISK = 'F:\dbbackups\mydb.bak';
```
   





   
