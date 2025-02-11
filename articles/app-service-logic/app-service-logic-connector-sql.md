<properties
   pageTitle="在 Logic Apps 中使用 SQL 連接器 | Microsoft Azure App Service"
   description="如何建立並設定 SQL 連接器或 API 應用程式，並在 Azure App Service 的邏輯應用程式中使用它"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="02/11/2016"
   ms.author="sameerch"/>


# 開始使用 Microsoft SQL 連接器並將它加入您的邏輯應用程式
>[AZURE.NOTE] 這一版文章適用於邏輯應用程式 2014-12-01-preview 結構描述版本。若為 Azure SQL 2015-08-01-preview 結構描述版本，請按一下 [SQL Azure API](../connectors/create-api-sqlazure.md)。

連接到內部部署 SQL Server 或 Azure SQL Database 來建立和變更資訊或資料。在 Logic Apps 中，連接器可以在「工作流程」中用來擷取、處理或推送資料。在工作流程中使用 SQL 連接器時，您可以達到各種案例的目的。例如，您可以：

- 使用 Web 或行動應用程式，公開位於 SQL 資料庫中的部分資料。
- 將資料插入 SQL 資料庫資料表以儲存。例如，您可以輸入員工記錄、更新銷售訂單等等。
- 從 SQL 取得資料，以供商務程序使用。例如，您可以取得客戶記錄，並將這些客戶記錄放在 SalesForce 中。

您可以將 SQL 連接器加入您的商務工作流程，就能在邏輯應用程式的該工作流程中處理資料。

## 觸發程序和動作
*觸發程序*是發生的事件。例如，訂單更新時或加入新客戶時。*動作*是觸發程序的結果。例如，當訂單更新時，傳送警示給銷售人員。或者，當加入新客戶時，傳送歡迎電子郵件給新客戶。

SQL 連接器可以在邏輯應用程式中做為觸發程序或動作，且支援 JSON 和 XML 格式的資料。對於封裝設定中包含的每個資料表 (本主題稍後詳細說明)，都有一組 JSON 動作和一組 XML 動作。

SQL 連接器提供下列觸發程序和動作：

觸發程序 | 動作
--- | ---
輪詢資料 | <ul><li>插入到資料表</li><li>更新資料表</li><li>從資料表選取</li><li>從資料表刪除</li><li>呼叫預存程序</li></ul>

## 建立 SQL 連接器

連接器可以在邏輯應用程式內建立，或直接從 Azure Marketplace 建立。從 Marketplace 建立連接器：

1. 在 Azure 開始面板中，選取 [**Marketplace**]。
2. 搜尋「SQL 連接器」，將其選取，然後選取 [建立]。
3. 輸入名稱、App Service 方案和其他屬性。
4. 輸入下列封裝設定：

	名稱 | 必要 | 說明
--- | --- | ---
伺服器名稱 | 是 | 輸入 SQL Server 名稱。例如，輸入 *SQLserver/sqlexpress* 或 *SQLserver.mydomain.com*。
連接埠 | 否 | 預設值為 1433。
使用者名稱 | 是 | 輸入可以登入 SQL Server 的使用者名稱。如果要連線到內部部署 SQL Server，請輸入 SQL 驗證認證。
密碼 | 是 | 輸入使用者名稱密碼。
資料庫名稱 | 是 | 輸入您要連接的資料庫。例如，您可以輸入 *Customers* 或 *dbo/orders*。
內部部署 | 是 | 預設值為 False。如果要連接到 Azure SQL Database，請輸入 False。如果要連線到內部部署 SQL Server，請輸入 True。
服務匯流排連接字串 | 否 | 如果您要連線至內部部署，請輸入服務匯流排轉送連接字串。<br/><br/>[使用混合式連線管理員](app-service-logic-hybrid-connection-manager.md)<br/>[服務匯流排定價](https://azure.microsoft.com/pricing/details/service-bus/)
夥伴伺服器名稱 | 否 | 如果主要伺服器無法使用，您可以輸入夥伴伺服器做為替代或備份伺服器。
資料表 | 否 | 列出可由連接器更新的資料庫資料表。例如，輸入 *OrdersTable* 或 *EmployeeTable*。如果不輸入任何資料表，則可以使用所有資料表。需要有效的資料表和/或預存程序，才能使用此連接器做為動作。
預存程序 | 否 | 輸入可供連接器呼叫的現有預存程序。例如，輸入 *sp\_IsEmployeeEligible* 或 *sp\_CalculateOrderDiscount*。需要有效的資料表和/或預存程序，才能使用此連接器做為動作。
資料可用查詢 | 觸發程序支援 | 判斷是否有任何資料可供輪詢 SQL Server 資料庫資料表的 SQL 陳述式。這應該會傳回數值，代表可用的資料的資料列數目。範例：SELECT COUNT(*) from table\_name。
輪詢資料查詢 | 觸發程序支援 | SQL Server 資料庫資料表的 SQL 陳述式。您可以輸入任意數目的 SQL 陳述式，以分號隔開。此陳述式以交易式方式執行，而且只有在資料安全地儲存在邏輯應用程式中時才會認可。範例：SELECT * FROM table\_name; DELETE FROM table\_name。<br/><br/>**注意**<br/>您必須提供輪詢陳述式，避免因為刪除、移動或更新選取的資料而造成無限迴圈，以確保不會重複輪詢相同的資料。

5. 完成時，[封裝設定] 看起來如下：![][1]

6. 選取 [**建立**]。


## 使用連接器做為觸發程序
讓我們以一個簡單的邏輯應用程式為例，它會輪詢 SQL 資料表的資料、在另一個資料表中加入資料，以及更新資料。

若要使用 SQL 連接器做為觸發程序，請輸入**資料可用查詢**和**輪詢資料查詢**值。**資料可用查詢**依您輸入的排程執行，並判斷是否有任何可用的資料。因為此查詢只傳回純量數字，可在經常執行時微調並最佳化。

**輪詢資料查詢**只在「資料可用查詢」指出有資料可用時才會執行。這個陳述式在交易內執行，而且只有在擷取的資料永久儲存在工作流程中時才會認可。必須避免一再地重複擷取相同的資料。此執行的交易式本質可用來刪除或更新資料，以確保下次查詢資料時不會收集到同樣的資料。

> [AZURE.NOTE] 此陳述式所傳回的結構描述可識別連接器中可用的屬性。所有資料行都必須命名。

#### 資料可用查詢範例

	SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### 輪詢資料查詢範例

	SELECT *, GetData() as 'PollTime' FROM [Order]
		WHERE OrderStatus = 'ProcessedForCollection'
		ORDER BY Id DESC;
	UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
		WHERE Id =
		(SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### 加入觸發程序
1. 建立或編輯邏輯應用程式時，請選取您建立的連接器做為觸發程序。這樣會列出可用的觸發程序：**輪詢資料 (JSON)** 和**輪詢資料 (XML)**：![][5]

2. 選取 [輪詢資料 (JSON)] 觸發程序，輸入頻率，然後按一下 ✓：![][6]

3. 現在，觸發程序在邏輯應用程式中顯示為已設定。其中顯示觸發程序的輸出，在任何後續動作中可做為輸入：![][7]

## 使用連接器做為動作
以我們簡單的邏輯應用程式案例為例，它會輪詢 SQL 資料表的資料、在另一個資料表中加入資料，以及更新資料。

若要使用 SQL 連接器做為動作，請輸入您建立 SQL 連接器時所輸入的資料表及/或預存程序的名稱：

1. 在觸發程序之後 (或選擇 [手動執行此邏輯])，從資源庫加入您建立的 SQL 連接器。選取其中一個「插入」動作，例如*插入到 TempEmployeeDetails (JSON)*：![][8]

2. 輸入要插入的記錄的輸入值，然後按一下 ✓：![][9]

3. 從資源庫選取您建立的同一個 SQL 連接器。在相同資料表上選取「更新」動作當做動作，例如 *Update EmployeeDetails*：![][11]

4. 輸入更新動作的輸入值，然後按一下 ✓：![][12]

您可以在所輪詢的資料表中加入新記錄，以測試邏輯應用程式。

## 可執行和無法執行的作業

SQL 查詢 | 支援 | 不支援
--- | --- | ---
Where 子句 | <ul><li>運算子：AND、OR、=、<>、<、<=、>、>= 以及 LIKE</li><li>‘(‘ 和 ‘)’可以結合多個子條件</li><li>字串常值、日期時間 (以單引號括住)、數字 (應該只包含數字字元)</li><li>嚴格應該二進位運算式格式，例如 ((運算元運算子運算元) AND/OR (運算元運算子運算元))**</li></ul> | <ul><li>運算子：Between、IN</li><li>所有內建的功能，例如 ADD()、MAX() NOW()、POWER() 等</li><li>數學運算子，例如 *、-、+ 等</li><li>使用 + 的字串串連。</li><li>所有聯結</li><li>IS NULL 和 IS NOT Null</li><li>包含非數字字元的任何數字，例如十六進位數字</li></ul>
功能變數 (在 SELECT 查詢中) | <ul><li>以逗號分隔的有效資料行名稱。不允許任何資料表名稱前置詞 (連接器一次處理一個資料表)。</li><li>可以使用 ‘[‘ 和 ‘]’ 逸出名稱</li></ul> | <ul><li>關鍵字，例如 TOP、DISTINCT 等</li><li>別名，例如 Street + City + Zip AS Address</li><li>所有內建函式，例如 ADD()、MAX() NOW()、POWER() 等</li><li>數學運算子，例如 *、-、+ 等</li><li>使用 + 的字串串連</li></ul>

#### 秘訣

- 對於進階查詢，建議您建立預存程序，並使用執行預存程序 API 來執行。
- 使用內部查詢時，請在預存程序內加以使用。
- 若要聯結多個條件，您可以使用 'AND' 和 'OR' 運算子。

## 混合式組態 (選用)

> [AZURE.NOTE] 只有當您在防火牆後方使用 SQL Server 內部部署時，才需要此步驟。

App Service 使用混合式組態管理員來安全地連線到內部部署系統。如果您的連接器使用內部部署 SQL Server，則需要混合式連線管理員。

> [AZURE.NOTE] 如果您想在註冊 Azure 帳戶前開始使用 Azure Logic Apps，請移至[試用 Logic App](https://tryappservice.azure.com/?appservice=logic)，即可在 App Service 中立即建立短期入門邏輯應用程式。不需要信用卡，無需承諾。

請參閱[使用混合式連線管理員](app-service-logic-hybrid-connection-manager.md)。


## 進一步運用您的連接器
現在已建立連接器，您可以將它加入到使用邏輯應用程式的商務工作流程。請參閱[什麼是邏輯應用程式？](app-service-logic-what-are-logic-apps.md)。

檢視位於[連接器和 API Apps 參考](http://go.microsoft.com/fwlink/p/?LinkId=529766)的 Swagger REST API 參考。

您也可以檢閱連接器的效能統計資料及控制安全性。請參閱[管理和監視內建 API Apps 和連接器](app-service-logic-monitor-your-connectors.md)。


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png

<!---HONumber=AcomDC_0224_2016-->