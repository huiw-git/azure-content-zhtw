<properties 
	pageTitle="SQL Database 的 XEvent 信號緩衝區程式碼 | Microsoft Azure" 
	description="提供 Transact-SQL 程式碼範例，可在 Azure SQL Database 中輕鬆又快速使用環形緩衝區目標。" 
	services="sql-database" 
	documentationCenter="" 
	authors="MightyPen" 
	manager="jeffreyg" 
	editor="" 
	tags=""/>


<tags 
	ms.service="sql-database" 
	ms.workload="data-management" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="12/30/2015" 
	ms.author="genemi"/>


# SQL Database 中擴充事件的環形緩衝區目標程式碼


您想要完整的程式碼範例以最簡單快速的方式在測試期間擷取和報告擴充事件的資訊。擴充事件資料最簡單的目標是[環形緩衝區目標](http://msdn.microsoft.com/library/ff878182.aspx)。


本主題顯示的 Transact-SQL 程式碼範例：


1. 使用資料建立要示範的資料表。

2. 建立現有擴充事件的工作階段，名稱為 **sqlserver.sql\_statement\_starting**。
 - 此事件僅限於包含特定 Update 字串的 SQL 陳述式：**statement LIKE '%UPDATE tabEmployee%'**。
 - 選擇要將事件的輸出傳送給環形緩衝區類型的目標，名稱為 **package0.ring\_buffer**。

3. 啟動事件工作階段。

4. 發出幾個簡單的 SQL UPDATE 陳述式。

5. 發出 SQL SELECT 擷取環形緩衝區的事件輸出。
 - **sys.dm\_xe\_database\_session\_targets** 和其他動態管理檢視 (DMV) 會聯結在一起。

6. 停止事件工作階段。

7. 卸除環形緩衝區目標以釋放其資源。

8. 卸除事件工作階段和示範資料表。


## 必要條件


- Azure 帳戶和訂用帳戶。您可以註冊[免費試用](https://azure.microsoft.com/pricing/free-trial/)。


- 您可以在當中建立資料表的任何資料庫。
 - 您可以選擇性快速[建立 **AdventureWorksLT** 示範資料庫](sql-database-get-started.md)。


- SQL Server Management Studio (ssms.exe) 在 2015 年 8 月的預覽版或更新版本。您可以從下列位置下載最新的 ssms.exe：
 - [主題中的連結。](http://msdn.microsoft.com/library/mt238290.aspx)
 - [下載的直接連結。](http://go.microsoft.com/fwlink/?linkid=616025)
 - Microsoft 建議您定期 (或每月) 更新 ssms.exe。


## 程式碼範例


只要稍加修改，就可以在 Azure SQL Database 或 Microsoft SQL Server 上執行下列環形緩衝區的程式碼範例。不同之處在於步驟 5 的 FROM 子句中，有些動態管理檢視 (DMV) 的名稱會出現 '\_database'。例如：

- sys.dm\_xe**\_database**\_session\_targets
- sys.dm\_xe\_session\_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
	(SELECT * FROM sys.objects
		WHERE type = 'U' and name = 'tabEmployee')
BEGIN
	DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
	EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
	EmployeeId           int                not null  identity(1,1),
	EmployeeKudosCount   int                not null  default 0,
	EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
	VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
	(SELECT * from sys.database_event_sessions
		WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
	DROP EVENT SESSION eventsession_gm_azuresqldb51
		ON DATABASE;
END
GO


CREATE
	EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	ADD EVENT
		sqlserver.sql_statement_starting
			(
			ACTION (sqlserver.sql_text)
			WHERE statement LIKE '%UPDATE tabEmployee%'
			)
	ADD TARGET
		package0.ring_buffer
			(SET
				max_memory = 500   -- Units of KB.
			);
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
	SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
	SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## 環形緩衝區內容


我們使用 ssms.exe 執行程式碼範例。


若要檢視結果，請按一下 **target\_data\_XML** 資料欄標題下的儲存格。

然後在結果窗格中，按一下 **target\_data\_XML** 資料欄標題下的儲存格。這會在 ssms.exe 中以結果儲存格內容顯示的順序，以 XML 格式建立另一個檔案索引標籤。


輸出如下列區塊所示。結果看起來很長，但其實只是兩個 **<event>** 元素。


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### 釋放環形緩衝區佔用的資源


當您處理完環形緩衝區時，可以發出 **ALTER** 將它移除並釋放其資源，如下所示：


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	DROP TARGET package0.ring_buffer;
GO
```


事件工作階段的定義會更新，但不會卸除。稍後您可以將環形緩衝區的另一個執行個體加入事件工作階段：


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	ADD TARGET
		package0.ring_buffer
			(SET
				max_memory = 500   -- Units of KB.
			);
```


## 詳細資訊


Azure SQL Database 上擴充事件的主要主題是：


- [SQL Database 中的擴充事件考量](sql-database-xevent-db-diff-from-svr.md)，對比 Azure SQL Database 與 Microsoft SQL Server 之間擴充事件的不同層面。


下列連結提供擴充事件的其他程式碼範例主題。不過，您必須定期檢查所有範例以查看範例是否適用於 Microsoft SQL Server 與 Azure SQL Database。然後您可以決定是否需要稍加變更來執行範例。


- Azure SQL Database 的程式碼範例：[SQL Database 中擴充事件的事件檔案目標程式碼](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->

<!---HONumber=AcomDC_0128_2016-->