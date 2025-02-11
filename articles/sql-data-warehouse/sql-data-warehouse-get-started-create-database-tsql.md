<properties
   pageTitle="使用 TSQL 建立 SQL 資料倉儲 | Microsoft Azure"
   description="了解如何使用 TSQL 建立 Azure SQL 資料倉儲"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="03/03/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# 使用 Transact-SQL (TSQL) 建立 SQL 資料倉儲資料庫

> [AZURE.SELECTOR]
- [Azure 入口網站](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

本文將說明如何使用 Transact-SQL (TSQL) 建立 SQL 資料倉儲資料庫。

## 開始之前

若要完成這篇文章中的步驟，您需要下列項目︰

- Azure 訂用帳戶。如果需要 Azure 訂用帳戶，可以先按一下此頁面頂端的 [免費試用]，然後再回來完成這篇文章。
- 。如需免費的 Visual Studio，請參閱 [Visual Studio 下載](https://www.visualstudio.com/downloads/download-visual-studio-vs)頁面。
- V12 邏輯 SQL Server。您將需要 V12 SQL Server 來建立 SQL 資料倉儲。如果您沒有 V12 邏輯 SQL 伺服器，[Azure 入口網站教學課程][]會為您示範如何建立。

## 使用 Visual Studio 建立資料庫

本文不會涵蓋如何使用 Visual Studio 正確設定與連接。如需如何進行的完整說明，請參閱[連接及查詢][]文件。若要開始，請在 Visual Studio 中開啟 SQL Server 物件總管，並連接到您將用來建立 SQL 資料倉儲資料庫的伺服器。一旦您這麼做，您就能針對「主要」資料庫執行下列命令來建立 SQL 資料倉儲：

        CREATE DATABASE <Name> (EDITION='datawarehouse', SERVICE_OBJECTIVE = '<Compute Size - DW####>', MAXSIZE= <Storage Size - #### GB>);

## 使用 sqlcmd 建立資料庫

您也能透過開啟命令列並執行下列命令建立 SQL 資料倉儲：

        sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE <Name> (EDITION='datawarehouse', SERVICE_OBJECTIVE = '<Compute Size - DW####>', MAXSIZE= <Storage Size - #### GB>)"

當執行上述 TSQL 陳述式時，請注意 MAXSIZE 和 SERVICE\_OBJECTIVE 參數將會要求初始的儲存體大小，而且計算到資料倉儲執行個體的分配。MAXSIZE 接受下列大小，建議選擇較大的空間大小以保留成長空間：250 GB、500 GB、750 GB、1024 GB、5120 GB、10240 GB、20480 GB、30720 GB、40960 GB、51200 GB。

SERVICE\_OBJECTIVE 會指出您的執行個體起始的 DWU 數目，並接受下列值：DW100、DW200、DW300、DW400、DW500、DW600、DW1000、DW1200、DW1500、DW2000。如需這些參數的計費影響詳細資訊，請參閱我們的[價格頁面][]。

## 後續步驟
您的 SQL 資料倉儲完成佈建之後，您可以[載入範例資料][]或查看如何[開發][]、[載入][]，或[移轉][]。

[Azure 入口網站教學課程]: ./sql-data-warehouse-get-started-provision.md
[連接及查詢]: ./sql-data-warehouse-get-started-connect.md
[移轉]: ./sql-data-warehouse-overview-migrate.md
[開發]: ./sql-data-warehouse-overview-develop.md
[載入]: ./sql-data-warehouse-overview-load.md
[載入範例資料]: ./sql-data-warehouse-get-started-manually-load-samples.md
[價格頁面]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/

<!---HONumber=AcomDC_0309_2016-->