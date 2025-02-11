<properties
	pageTitle="使用混合式連線從 .NET 後端行動服務連線至內部佈署 SQL Server | Azure 行動服務"
	description="了解如何使用 Azure 混合式連線從 .NET 後端行動服務連線至內部佈署 SQL Server"
	services="mobile-services"
	documentationCenter=""
	authors="ggailey777"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="02/07/2016"
	ms.author="glenga"/>


# 使用混合式連接從 Azure 行動服務連接至內部部署 SQL Server

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


貴公司在轉移到雲端環境時，可能無法立即就將所有資產移轉至 Azure。混合式連接可讓 Azure 行動服務安全地連接到內部部署資產。因此，您可以藉由混合式連線讓行動用戶端使用 Azure 存取內部部署資料。支援的資產包括任何可在靜態 TCP 連接埠上執行的資源，例如 Microsoft SQL Server、MySQL、HTTP Web API 和大部分的自訂 Web 服務。混合式連線採用共用存取簽章 (SAS) 授權，以確保從行動服務和內部部署混合式連線管理員連往混合式連線的連線安全無虞。如需詳細資料，請參閱[混合式連線概觀](../integration-hybrid-connection-overview.md)。

在本教學課程中，您將了解如何修改 .NET 後端行動服務，以使用本機內部部署 SQL Server 資料庫，而不要使用隨著您的服務佈建的預設 Azure SQL Database。如[這篇文章](http://blogs.msdn.com/b/azuremobile/archive/2014/05/12/connecting-to-an-external-database-with-node-js-backend-in-azure-mobile-services.aspx)所述，JavaScript 後端行動服務也支援混合式連接。

##必要條件##

本教學課程要求您具備下列項目：

- **現有 .NET 後端行動服務** <br/>依照[開始使用行動服務]教學課程，從 [Azure 傳統入口網站]建立和下載新的 .NET 後端行動服務。

[AZURE.INCLUDE [hybrid-connections-prerequisites](../../includes/hybrid-connections-prerequisites.md)]

## 在內部部署中安裝 SQL Server Express、啟用 TCP/IP 及建立 SQL Server 資料庫

[AZURE.INCLUDE [hybrid-connections-create-on-premises-database](../../includes/hybrid-connections-create-on-premises-database.md)]

## 建立混合式連線

[AZURE.INCLUDE [hybrid-connections-create-new](../../includes/hybrid-connections-create-new.md)]

## 安裝內部部署混合式連線管理員以完成連線

[AZURE.INCLUDE [hybrid-connections-install-connection-manager](../../includes/hybrid-connections-install-connection-manager.md)]

## 設定行動服務專案以連線到 SQL Server 資料庫

在此步驟中，您將定義內部部署資料庫的連接字串，並修改行動服務以使用這個連接。

1. 在 Visual Studio 2013 中，開啟用以定義 .NET 後端行動服務的專案。

	若想了解如何下載 .NET 後端專案，請參閱[開始使用行動服務](mobile-services-dotnet-backend-windows-store-dotnet-get-started.md)。

2. 在 [方案總管] 中開啟 Web.config 檔案，找出 **connectionStrings** 區段，加入類似下面內容的新 SqlClient 項目，此項目指向內部部署 SQL Server 資料庫：

	    <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />

	請記得將這個字串中的 `<**secure_password**>` 取代為您為 HbyridConnectionLogin 建立的密碼。

3. 在 Visual Studio 中按一下 [儲存]，以儲存 Web.config 檔案。

	> [AZURE.NOTE]在本機電腦上執行時，會使用此連線設定。在 Azure 中執行時，則會以入口網站中所定義的連線設定覆寫這個設定。

4. 展開 **Models** 資料夾，然後開啟以 *Context.cs* 結尾的資料模型檔案。

6. 修改 **DbContext** 執行個體建構函式，以將 `OnPremisesDBConnection` 值傳遞至類似下列程式碼片段的基底 **DbContext** 建構函式：

        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }

	現在，服務將會使用新的 SQL Server 資料庫連線。

##在本機測試資料庫連線

在發佈至 Azure 並使用混合式連線之前，最好先確定資料庫連線能夠在本機執行時正常運作。如此一來，您就能更輕鬆地診斷並更正任何連線問題，然後再重新發佈並開始使用混合式連線。

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-api-documentation](../../includes/mobile-services-dotnet-backend-test-local-service-api-documentation.md)]

## 更新 Azure 以使用內部部署連接字串

您已驗證過資料庫連接，接下來您必須為這個新的連接字串新增應用程式設定，以便能夠從 Azure 使用並將行動服務發佈至 Azure。

1. 在 [Azure 傳統入口網站]中，瀏覽至您的行動服務。

1. 按一下 [設定] 索引標籤，然後找出 [連接字串] 區段。

	![Connection string for on-premises database](./media/mobile-services-dotnet-backend-hybrid-connections-get-started/11.png)

2. 加入名為 `OnPremisesDBConnection` 的新 **SQL Server** 連接字串，其值如下：

		Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>


	將 `<**secure_password**>` 替換為 *HybridConnectionLogin* 的安全密碼。

2. 按下 [**儲存**]，以儲存您剛剛建立的混合式連線和連接字串。

3. 使用 Visual Studio，將更新過的行動服務專案發佈至 Azure。

	服務啟動頁面隨即顯示。

4. 和之前一樣使用啟動頁面上的 [立即試用] 按鈕，或使用連線到行動服務的用戶端應用程式，叫用可產生資料庫變更的某些作業。

	>[AZURE.NOTE]當您使用 [**立即試用**] 按鈕來啟動說明 API 頁面時，請記得提供您的應用程式金鑰來做為密碼 (使用者名稱則為空白)。

4. 在 SQL Server Management Studio 中，連線到 SQL Server 執行個體，開啟物件總管，然後依序展開 [**OnPremisesDB**] 資料庫和 [**表格**]。

5. 以滑鼠右鍵按一下 **hybridService1.TodoItems** 資料表，然後選擇 [Select Top 1000 Rows] 以檢視結果。

	請注意，您的行動服務已使用混合式連接，將應用程式所產生的變更儲存到內部部署資料庫。

##另請參閱##

+ [混合式連線網站](../../services/biztalk-services/)
+ [混合式連線概觀](../integration-hybrid-connection-overview.md)
+ [BizTalk 服務：儀表板、監視器、調整、設定和混合式連線索引標籤](../biztalk-dashboard-monitor-scale-tabs.md)
+ [如何對 .NET 後端行動服務進行資料模型變更](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)

<!-- IMAGES -->


<!-- Links -->
[Azure 傳統入口網站]: http://manage.windowsazure.com
[開始使用行動服務]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md

<!---HONumber=AcomDC_0211_2016-->