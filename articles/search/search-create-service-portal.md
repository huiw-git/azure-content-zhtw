<properties
	pageTitle="在入口網站中建立 Azure 搜尋服務 | Microsoft Azure | 雲端搜尋服務"
	description="使用 Azure 入口網站將免費或標準的 Azure 搜尋服務新增到現有的訂用帳戶。Azure 搜尋服務是自訂應用程式的雲端託管搜尋服務。"
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="mblythe"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="na"
	ms.workload="search"
	ms.topic="hero-article"
	ms.tgt_pltfrm="na"
	ms.date="03/09/2016"
	ms.author="heidist"/>

# 在 Azure 入口網站中建立 Azure 搜尋服務

Microsoft Azure 搜尋服務是託管的雲端搜尋服務，可讓您將搜尋功能內嵌到自訂用程式。此服務可為您的搜尋資料提供搜尋引擎和儲存體，您可以透過 Azure 入口網站、.NET SDK 或 REST API 存取和管理這類資料。主要功能包括自動完成查詢、模糊比對、符合結果醒目提示、多面向導覽、發行設定檔和多語言支援。若要深入了解 Azure 搜尋服務可執行哪些作業，請參閱[什麼是 Azure 搜尋服務](search-what-is-azure-search.md)。

Azure 搜尋服務適用於範圍從免費 (共用)、基本到標準 (成本是根據您註冊多少容量按比例計算) 的定價層級。

## 將 Azure 搜尋服務免費加入您的訂用帳戶

身為管理員，如果選擇使用共用的服務，您可以免費將 Azure 搜尋服務加入現有的 Azure 訂用帳戶。您可以註冊[免費 Azure 試用訂用帳戶](../../includes/free-trial-note.md)開始進行評估。

1. 登入 [Azure 入口網站](https://portal.azure.com)。

2. 在動態工具列中，按一下 [新增] > [資料 + 儲存體] > [搜尋]。

     ![][1]

3. 設定服務名稱、定價層、資源群組、訂用帳戶及位置。這些設定都是必須設定的項目，且在佈建服務後就不能變更。

     ![][2]

	- **服務名稱**必須是唯一的、小寫、少於 60 個字元，且不得包含空格。此名稱會成為 Azure 搜尋服務端點的一部分 (例如，"https://**my-service-name**.search.windows.net")。如需命名慣例的詳細資訊，請參閱[命名規則](https://msdn.microsoft.com/library/azure/dn857353.aspx)。

	- **定價層**決定可用容量和計費方式。以下兩個定價層提供相同的功能，但所處的資源層級不同。

		- **免費版**是在與其他訂戶共用的叢集上執行。此版本所提供的容量，已足夠用來試驗教學課程以及撰寫用於證明概念的程式碼，但並不適用於生產應用程式。部署免費版服務通常只需要幾分鐘時間。
		- **基本 (預覽)** 是在專用資源上執行，但上限和價格較低，適用於較小的生產工作負載。您可以相應增加至 3 個複本和 1 個資料分割，此規模已經足以應付執行查詢時的高可用性需求。
		- **標準版**是在專用的資源上執行作業，且具備高度可擴充性。標準版服務一開始是以一個複本和一個資料分割進行佈建，不過您可以在建立服務後將容量上調到最多 36 個搜尋單位。部署標準版服務需要較長的時間，通常大約需要 15 分鐘。

	- **資源群組**是適用於一般用途之服務和資源的容器。例如，如果您以 Azure 搜尋、Azure App Service 中的 Web Apps 功能，以及 Azure Blob 儲存體為基礎來建置自訂搜尋應用程式，那麼您可以建立資源群組，將這些服務一起放在入口網站管理頁面中。

	- **訂用帳戶**可讓您在多個訂用帳戶間選擇 (如果您有多個訂用帳戶)。

	- **位置**是資料中心區域。目前，所有資源都必須在同一個資料中心內執行。不支援將資源分配到多個資料中心。

4. 按一下 [建立] 即可佈建服務。

注意 Jumpbar 中的通知。服務一旦準備就緒可供使用時就會顯示通知。

<a id="sub-3"></a>
## 新增基本或標準層搜尋服務以取得專屬資源

許多客戶會從免費服務著手，然後再切換至基本或標準層以適應較大的工作負載。基本和標準層會在 Azure 資料中心內為您提供專用資源。

Azure 搜尋作業同時需要儲存體和服務複本。相較於沒有選項可加入資源的免費服務，標準層可讓您提升對您的工作負載日益重要的資源，進而相應增加以加入更多儲存體或查詢支援。基本層也可讓您向上調整規模，但僅限複本，並限制最多三個。

若要使用基本或標準層，您必須在該定價層級建立新的搜尋服務。您可以重複本文先前的步驟來建立新的 Azure 搜尋服務。請注意，設定專用資源可能需要一些時間，可能長達 15 分鐘或更長的時間。

免費版沒有就地升級選項。在可能調整規模的情況下切換階層，會需要新的服務。您將需要重新載入搜尋應用程式所使用的索引和文件。

基本或標準層中的 Azure 搜尋服務會各以一個複本和分割區來建立，但可輕鬆地在更高的資源層級中重新調整規模。

1.	建立服務後，請返回服務儀表板。

2.	按一下 [調整] 圖格。

3.	使用滑桿為標準層新增複本和 (或) 分割區。若是基本層，您可以增加到最多三個複本。

額外的複本和資料分割會依搜尋單位計費。當您新增資源時，頁面上會顯示要支援任何特定資源設定所需要的搜尋單位總數。

您可以查看[定價詳細資料](http://go.microsoft.com/fwlink/p/?LinkID=509792)以取得每單位的計費資訊。請參閱[容量規劃](search-capacity-planning.md)取得選擇分割區和複本組合的說明。

<a id="sub-2"></a>
## 尋找 Azure 搜尋服務的服務名稱和 API 金鑰

建立服務之後，您可以返回 Azure 入口網站來取得 URL 或 `api-key`。若要連接至 Azure 搜尋服務，必須同時擁有 URL 和 `api-key` 才能驗證呼叫。

1. 在動態工具列中，依序按一下 [首頁]，然後按一下 Azure 搜尋服務以開啟服務儀表板。

2. 您會在服務儀表板上看到基本資訊圖格，以及存取系統管理金鑰的鑰匙圖示。

  	![][3]

3. 複製服務 URL 和系統管理金鑰，進到下一個工作 ([確認服務可用性](#sub-4)) 時，您將會需要這些資料。


<a id="sub-4"></a>
## 確認服務可用性

設定 Azure 搜尋的最後一個步驟是，確認您的服務是否可從用戶端應用程式進行操作和存取。您可以使用 [Azure 搜尋服務 Fiddler](search-fiddler.md) 來驗證服務可用性。

<!--Next steps and links -->
<a id="next-steps"></a>
## 後續步驟

您現已建立服務，接著可以採取後續步驟：建立[索引](search-what-is-an-index.md)、[查詢索引](search-query-overview.md)、建立及管理使用 Azure 搜尋服務的搜尋應用程式。

- [在 Azure 入口網站中建立 Azure 搜尋服務索引](search-create-index-portal.md)

- [在 Azure 入口網站中使用搜尋總管查詢 Azure 搜尋服務索引](search-explorer.md)

- [如何以 .NET 使用 Azure 搜尋服務](search-howto-dotnet-sdk.md)

- [在 Microsoft Azure 中管理搜尋方案](search-manage.md)


<!--Anchors-->
[Find the service name and api-keys of your Azure Search service]: #sub-2
[Upgrade to the standard tier]: #sub-3
[Test service operations]: #sub-4
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/search-create-service-portal/create-search-portal-1.PNG
[2]: ./media/search-create-service-portal/create-search-portal-2.PNG
[3]: ./media/search-create-service-portal/create-search-portal-3.PNG

<!---HONumber=AcomDC_0309_2016-->