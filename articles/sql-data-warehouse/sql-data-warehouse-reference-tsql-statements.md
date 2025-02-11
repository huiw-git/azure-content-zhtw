<properties
   pageTitle="SQL 資料倉儲 Transact-SQL 參考 | Microsoft Azure"
   description="SQL 資料倉儲所使用的 Transact-SQL 主題的參考內容連結。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="02/01/2016"
   ms.author="barbkess;sonyama"/>

#Transact-SQL 主題

## 資料定義語言 (DDL) 陳述式

- [ALTER DATABASE](https://msdn.microsoft.com/library/mt204042.aspx)
- [ALTER INDEX](https://msdn.microsoft.com/library/ms188388.aspx)
- [ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- [ALTER SCHEMA](https://msdn.microsoft.com/library/ms173423.aspx)
- [ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)
- [CREATE COLUMNSTORE INDEX](https://msdn.microsoft.com/library/gg492153.aspx)
- [CREATE DATABASE](https://msdn.microsoft.com/library/mt204021.aspx)
- [建立資料庫範圍認證](https://msdn.microsoft.com/library/mt270260.aspx)
- [CREATE EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx)
- [CREATE EXTERNAL FILE FORMAT](https://msdn.microsoft.com/library/dn935026.aspx)
- [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx)
- [CREATE FUNCTION](https://msdn.microsoft.com/library/mt203952.aspx)
- [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
- [CREATE PROCEDURE](https://msdn.microsoft.com/library/ms187926.aspx)
- [CREATE SCHEMA](https://msdn.microsoft.com/library/ms189462.aspx)
- [CREATE STATISTICS](https://msdn.microsoft.com/library/ms188038.aspx)
- [CREATE TABLE](https://msdn.microsoft.com/library/mt203953.aspx)
- [CREATE TABLE AS SELECT](https://msdn.microsoft.com/library/mt204041.aspx)
- [CREATE VIEW](https://msdn.microsoft.com/library/ms187956.aspx)
- [DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/mt146367.aspx)
- [DROP EXTERNAL FILE FORMAT](https://msdn.microsoft.com/library/mt146379.aspx)
- [DROP EXTERNAL TABLE](https://msdn.microsoft.com/library/mt130698.aspx)
- [DROP INDEX](https://msdn.microsoft.com/library/ms176118.aspx)
- [DROP PROCEDURE](https://msdn.microsoft.com/library/ms174969.aspx)
- [DROP STATISTICS](https://msdn.microsoft.com/library/ms175075.aspx)
- [DROP TABLE](https://msdn.microsoft.com/library/ms173790.aspx)
- [DROP SCHEMA](https://msdn.microsoft.com/library/ms186751.aspx)
- [DROP VIEW](https://msdn.microsoft.com/library/ms173492.aspx)
- [RENAME](https://msdn.microsoft.com/library/mt631611.aspx)
- [TRUNCATE TABLE](https://msdn.microsoft.com/library/ms177570.aspx)
- [UPDATE STATISTICS](https://msdn.microsoft.com/library/ms187348.aspx)

## 資料操作語言 (DML) 陳述式

- [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)
- [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)
- [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)

## 資料庫主控台命令

- [DBCC DROPCLEANBUFFERS](https://msdn.microsoft.com/library/ms187762.aspx)
- [DBCC FREEPROCCACHE](https://msdn.microsoft.com/library/mt204018.aspx)
- [DBCC SHRINKLOG](https://msdn.microsoft.com/library/mt204020.aspx)
- [DBCC PDW\_SHOWEXECUTIONPLAN](https://msdn.microsoft.com/library/mt204017.aspx)
- [DBCC PDW\_SHOWPARTITIONSTATS](https://msdn.microsoft.com/library/mt204013.aspx)
- [DBCC PDW\_SHOWSPACEUSED](https://msdn.microsoft.com/library/mt204028.aspx)
- [DBCC SHOW\_STATISTICS](https://msdn.microsoft.com/library/mt204043.aspx)

## 查詢陳述式

- [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)
- [WITH common\_table\_expression](https://msdn.microsoft.com/library/ms175972.aspx)
- [EXCEPT 和 INTERSECT](https://msdn.microsoft.com/library/ms188055.aspx)
- [EXPLAIN](https://msdn.microsoft.com/library/mt631615.aspx)
- [FROM](https://msdn.microsoft.com/library/ms177634.aspx)
- [使用 PIVOT 和 UNPIVOT](https://msdn.microsoft.com/library/ms177410.aspx)
- [GROUP BY](https://msdn.microsoft.com/library/ms177673.aspx)
- [HAVING](https://msdn.microsoft.com/library/ms180199.aspx)
- [ORDER BY](https://msdn.microsoft.com/library/ms188385.aspx)
- [OPTION](https://msdn.microsoft.com/library/ms190322.aspx)
- [UNION](https://msdn.microsoft.com/library/ms180026.aspx)
- [WHERE](https://msdn.microsoft.com/library/ms188047.aspx)
- [TOP](https://msdn.microsoft.com/library/ms189463.aspx)
- [別名](https://msdn.microsoft.com/library/mt631614.aspx)
- [搜尋條件](https://msdn.microsoft.com/library/ms173545.aspx)
- [子查詢](https://msdn.microsoft.com/library/mt631613.aspx)

## 安全性陳述式

- 權限：[GRANT](https://msdn.microsoft.com/library/ms187965.aspx)、[DENY](https://msdn.microsoft.com/library/ms188338.aspx)、[REVOKE](https://msdn.microsoft.com/library/ms187728.aspx)
- [ALTER AUTHORIZATION](https://msdn.microsoft.com/library/ms187359.aspx)
- [ALTER CERTIFICATE](https://msdn.microsoft.com/library/ms189511.aspx)
- [ALTER DATABASE ENCRYPTION KEY](https://msdn.microsoft.com/library/bb630389.aspx)
- [ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)
- [ALTER MASTER KEY](https://msdn.microsoft.com/library/ms186937.aspx)
- [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx)
- [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)
- [BACKUP CERTIFICATE](https://msdn.microsoft.com/library/ms178578.aspx)
- [CLOSE MASTER KEY](https://msdn.microsoft.com/library/ms188387.aspx)
- [CREATE CERTIFICATE](https://msdn.microsoft.com/library/ms187798.aspx)
- [CREATE DATABASE ENCRYPTION KEY](https://msdn.microsoft.com/library/bb677241.aspx)
- [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx)
- [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
- [CREATE ROLE](https://msdn.microsoft.com/library/ms187936.aspx)
- [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx)
- [DROP CERTIFICATE](https://msdn.microsoft.com/library/ms179906.aspx)
- [DROP DATABASE ENCRYPTION KEY](https://msdn.microsoft.com/library/bb630256.aspx)
- [DROP LOGIN](https://msdn.microsoft.com/library/ms188012.aspx)
- [DROP MASTER KEY](https://msdn.microsoft.com/library/ms180071.aspx)
- [DROP ROLE](https://msdn.microsoft.com/library/ms174988.aspx)
- [DROP USER](https://msdn.microsoft.com/library/ms189438.aspx)
- [OPEN MASTER KEY](https://msdn.microsoft.com/library/ms174433.aspx)


## 後續步驟
如需更多 TSQL 範例，請參閱 [SQL 資料倉儲開發概觀][]。

<!--Image references-->

<!--Article references-->
[SQL 資料倉儲開發概觀]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0218_2016-->