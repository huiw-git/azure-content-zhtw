<properties
	pageTitle="儲存體簡介 | Microsoft Azure"
	description="Azure 儲存體概觀，Microsoft 的線上雲端資料儲存體。了解如何在您的應用程式中使用可用的最佳雲端儲存體解決方案。"
	services="storage"
	documentationCenter=""
	authors="tamram"
	manager="carmonm"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="02/24/2016"
	ms.author="tamram"/>

# Microsoft Azure 儲存體簡介

## 概觀

Azure 儲存體是現代應用程式的雲端儲存體解決方案，這些應用程式仰賴持續性、可用性和可調整性來滿足其客戶的需求。透過閱讀此文件，開發人員、IT 專業人員，以及商業決策人員可以了解：

- 何謂 Azure 儲存體，以及可以如何在雲端、行動、伺服器和桌面應用程式中加以充分運用
- 您可以使用 Azure 儲存體儲存哪些資料：Blob (物件) 資料、NoSQL 資料表資料、佇列訊息，以及檔案共用。
- 如何管理 Azure 儲存體的資料存取
- 如何透過備援和複寫來保護 Azure 儲存體資料
- 若要打造第一個 Azure 儲存體應用程式下一步該怎麼做

若要快速啟動並執行 Azure 儲存體，請參閱[在 5 分鐘內開始使用 Azure 儲存體](storage-getting-started-guide.md)。

如需使用 Azure 儲存體的工具、程式庫及其他資源的詳細資訊，請參閱下方的[後續步驟](#next-steps)。

## 何謂 Azure 儲存體？

雲端運算針對資料需要可擴充、可持久和高可用性儲存體的應用程式，提供了全新案例 - 這也是 Microsoft 開發 Azure 儲存體的真正原因。除了可讓開發人員打造支援全新案例的大規模應用程式之外，Azure 儲存體還針對 Microsoft 虛擬機器提供儲存基礎，並更進一步證明了其穩固性。

Azure 儲存體可大幅擴充，因此您可以儲存並處理數百 TB 的資料，藉此支援科學、財務分析和媒體應用程式等所需的巨量資料案例。或者您可以儲存小型企業網站所需的少量資料。無論您的需求屬於何者，只需要為您儲存的資料付費。Azure 儲存體目前儲存了數十兆的獨特客戶物件，且平均每秒處理上百萬個要求。

Azure 儲存體非常有彈性，因此您可以為大量的全球使用者設計應用程式，然後視需要擴充這些應用程式 - 都是根據儲存的資料量和要求資料的次數。您只需在使用時，針對所使用資料進行付費即可。

Azure 儲存體使用自動分割系統，可自動根據流量負載平衡您的資料。這代表隨著應用程式需求不斷成長，Azure 儲存體會自動配置適當資源以符合需求。

您可從世界各地、任何應用程式類型 (無論是在雲端、桌面、內部部署伺服器、行動或平板裝置上執行) 存取 Azure 儲存體。您可以在行動案例中使用 Azure 儲存體，案例中的應用程式會在裝置上儲存資料子集，然後將它與儲存在雲端中的整組資料同步處理。

為了方便開發，Azure 儲存體支援使用各種作業系統 (包括 Windows 和 Linux) 和多種程式設計語言 (包括 .NET、Java 和 C++) 的用戶端。Azure 儲存體還可透過簡單 REST API 公開資料資源，REST API 適用於任何能夠透過 HTTP/HTTPS 傳送與接收資料的用戶端。

Azure Premium 儲存體對於 Azure 虛擬機器上執行的 I/O 密集工作負載提供高效能、低延遲磁碟支援。透過 Azure Premium 儲存體，您可以將多個持續資料磁碟連接到虛擬機器，並設定這些磁碟符合您的效能需求。各個資料磁碟都有 Azure Premium 儲存體的 SSD 磁碟做為備援，充分達到最大的 I/O 效能。如需詳細資訊，請參閱 [Premium 儲存體：Azure 虛擬機器工作負載適用的高效能儲存體](storage-premium-storage.md)。

## Azure 儲存體服務簡介

Azure 儲存體帳戶是可讓您存取 Azure 儲存體服務的安全帳戶。您的儲存體帳戶提供儲存體資源的唯一命名空間。儲存體帳戶分為兩種類型：

- 標準儲存體帳戶包含 Blob、資料表、佇列和檔案儲存體。
- Premium 儲存體帳戶目前僅支援 Azure 虛擬機器磁碟。

在建立儲存體帳戶之前，您必須擁有 Azure 訂閱，訂閱是指可讓您存取各種 Azure 服務的方案。單一訂用帳戶可讓您建立最多 100 個唯一命名的儲存體帳戶。如需關於儲存體帳戶限制的詳細資訊，請參閱[Azure 儲存體延展性和效能目標](storage-scalability-targets.md)。如需批量價格的詳細資訊，請參閱 [Azure 儲存體價格](https://azure.microsoft.com/pricing/details/storage/)。

您可以利用[免費帳戶](https://azure.microsoft.com/pricing/free-trial/)來開始使用 Azure。在決定購買訂用計畫之後，您便可以選擇各種[購買選項](https://azure.microsoft.com/pricing/purchase-options/)。如果您是 [MSDN 訂閱者](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，您將取得可在 Azure 服務 (包括 Azure 儲存體) 中使用的每月免費額度。

### 標準儲存體帳戶

標準儲存體帳戶可供您存取 Azure 儲存體資料服務：Blob 儲存體、表格儲存體、佇列儲存體和檔案儲存體：

- 「Blob 儲存體」可儲存檔案資料。Blob 可以是任何類型的文字或二進位資料，例如文件、媒體檔案或應用程式安裝程式。Blob 儲存體也稱為物件儲存體。
- 「表格儲存體」可儲存結構化資料集。資料表儲存體屬於 NoSQL 索引鍵屬性資料儲存，可允許快速開發和迅速存取大量資料。
- 「佇列儲存體」可為工作流程處理及雲端服務元件間的通訊，提供可靠的訊息服務。
- 「檔案儲存體」可為使用標準 SMB 通訊協定的舊版應用程式提供共用儲存體。Azure 虛擬機器和雲端服務可以透過掛接的共用，在應用程式元件之間共用檔案資料，而內部部署應用程式可以透過檔案服務 REST API，存取共用中的檔案資料。

每個標準儲存體帳戶都可包含多達 500 TB 的 Blob、佇列、資料表和檔案資料的組合。如需標準儲存體帳戶容量的詳細資訊，請參閱 [Azure 儲存體延展性和效能目標](storage-scalability-targets.md)。

下圖顯示標準儲存體帳戶中 Azure 儲存體資源之間的關係：

![Azure 儲存體資源](./media/storage-introduction/storage-concepts.png)

若要了解如何建立標準儲存體帳戶，請參閱[建立儲存體帳戶](storage-create-storage-account.md#create-a-storage-account)以取得詳細資訊。

### Premium 儲存體帳戶

Azure Premium 儲存體目前僅支援 Azure 虛擬機器磁碟。如需 Azure Premium 儲存體的深入概觀，請參閱 [Premium 儲存體：Azure 虛擬機器工作負載適用的高效能儲存體](storage-premium-storage.md)。

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## Blob 儲存體

若是擁有大量非結構化資料要儲存於雲端的使用者，Blob 儲存體提供具有成本效益且可擴充的解決方案。您可以使用 Blob 儲存體來儲存如下所示的內容：

- 文件
- 社交資料 (例如照片、視訊、音樂和部落格)
- 檔案、電腦、資料庫和裝置的備份
- Web 應用程式的影像和文字
- 雲端應用程式的組態資料
- 巨量資料 (例如記錄和其他大型資料集)

每個 Blob 會組織成一個容器。容器也提供對物件群組指派安全原則的實用方式。儲存體帳戶可包含任意數目的容器，而容器可包含任意數目的 Blob，儲存體帳戶的容量限制高達 500 TB。

Blob 儲存體提供三種類型的 Blob，包括區塊 Blob、附加 Blob 及分頁 Blob (磁碟)。區塊 Blob 已針對串流和儲存雲端物件進行最佳化，是儲存文件、媒體檔案、備份等的不錯選擇。附加 Blob 和區塊 Blob 類似，但已針對附加作業最佳化。附加 Blob 只能透過在結尾加入新的區塊來更新。對於如記錄等僅需要將新資料寫入 Blob 結尾的情況，附加 Blob 是不錯的選擇。

頁面 Blob 已針對顯示 IaaS 磁碟與支援隨機寫入進行最佳化，且大小可能可以高達 1 TB。Azure 虛擬機器網路附加 IaaS 磁碟是以頁面 Blob 方式儲存的 VHD。

若是超大型資料集，網路限制會使得透過線路上傳或下載資料至 Blob 儲存體變得不切實際，您可以將硬碟送至 Microsoft，以便直接從資料中心匯入或匯出資料。請參閱[使用 Microsoft Azure 匯入/匯出服務將資料移轉至 Blob 儲存體](storage-import-export-service.md)。

## 資料表儲存體

新式應用程式通常會要求以超越前幾代軟體需求的延伸性和彈性儲存資料。資料表儲存體提供高可用性且可大幅擴充的儲存體，方便您的應用程式自動擴充以滿足使用者需求。資料表儲存體屬於 Microsoft 的 NoSQL 索引鍵屬性儲存，它的無結構描述設計讓它有別於傳統的關聯式資料庫。透過無結構描述資料儲存，便可輕易隨著應用程式發展需求改寫資料。資料表儲存體非常容易使用，方便開發人員可以快速建立應用程式。所有類型的應用程式都可以用快速且具成本效益的方式存取資料。相較於類似資料量的傳統 SQL，資料表儲存體通常可大幅降低成本。

資料表儲存體屬於索引鍵屬性儲存，代表資料表中的每個值會與具型別的屬性名稱一起儲存。屬性名稱可用來篩選與指定選取條件。一組屬性及其值就構成一個實體。因為資料表儲存體沒有結構描述，因此相同資料表中的兩個實體可包含不同屬性集合，且這些屬性可以是不同類型。

您可以使用資料表儲存體來儲存具彈性的資料集，例如 Web 應用程式的使用者資料、通訊錄、裝置資訊，以及服務所需的任何其他中繼資料類型。您可以在資料表中儲存任意數目的實體，且儲存體帳戶可包含任意數目的資料表，最高可達儲存體帳戶的容量限制。

如同 Blob 和佇列，開發人員可以使用標準 REST 通訊協定來管理與存取表格儲存體，不過，表格儲存體也支援 OData 通訊協定的子集，進而簡化進階查詢功能，並啟用 JSON 和 AtomPub (以 XML 為基礎) 格式。

在現今的網際網路架構應用程式中，NoSQL 資料庫 (如資料表儲存體) 提供傳統關聯式資料庫的熱門替代方式。

## 佇列儲存體

設計擴充性的應用程式時，會經常分離應用程式元件，以便進行個別擴充。佇列儲存體針對應用程式元件間的非同步通訊，提供可靠的訊息服務解決方案，無論應用程式元件是在雲端、桌面、內部部署伺服器或行動裝置上執行。佇列儲存體也支援管理非同步工作並建置處理工作流程。

儲存體帳戶可包含任意數目的佇列。佇列可包含任意數目的訊息，最高可達儲存體帳戶的容量限制。個別訊息的大小可能高達 64 KB。

## 檔案儲存體

Azure 檔案儲存體提供雲端架構的 SMB 檔案共用，可讓您快速地將依賴檔案共用的舊式應用程式移轉至 Azure，而不必浪費成本來重新撰寫程式。利用 Azure 檔案儲存體，在 Azure 虛擬機器或雲端服務中執行的應用程式，可掛接雲端中的檔案共用，就像桌面應用程式掛接一般 SMB 共用一樣。可同時掛接和存取檔案儲存體共用的應用程式元件數量沒有限制。

因為檔案儲存體是標準 SMB 檔案共用，所以 Azure 中執行的應用程式可透過檔案系統 I/O API 來存取共用中的資料。因此，開發人員可利用現有的程式碼和技能來移轉現有的應用程式。IT 專業人員在管理 Azure 應用程式時，可以使用 PowerShell Cmdlet 來建立、掛接和管理檔案儲存體共用。

就像其他 Azure 儲存體服務一樣，檔案儲存體也公開 REST API 來存取共用中的資料。內部部署應用程式可以呼叫檔案儲存體 REST API 來存取檔案共用中的資料。因此，企業可以選擇將某些舊式應用程式移轉至 Azure，並繼續在其自己的組織內執行其他舊式應用程式。請注意，只有 Azure 中執行的應用程式才能掛接檔案共用。內部部署應用程式只能透過 REST API 來存取檔案共用。

分散式應用程式也可以利用檔案儲存體，以儲存並共用實用的應用程式資料及開發和測試工具。例如，應用程式可以在檔案儲存體共用中儲存組態檔和診斷資料，像是記錄、度量和損毀傾印，供多個虛擬機器或角色使用。開發人員和管理員可以在所有元件可用的檔案儲存體共用中，儲存他們建置或管理應用程式所需的公用程式，而不需要將公用程式安裝在每個虛擬機器或角色執行個體上。

## 存取 Blob、資料表、佇列和檔案資源

依預設，只有儲存體帳戶擁有者可以存取儲存體帳戶內的資源。為了資料的安全性，針對帳戶內資源所做的每個要求都必須經過驗證。驗證會仰賴共用金鑰模型。Blob 也可設定為支援匿名驗證。

建立儲存體帳戶時所指派的兩組存取金鑰可用於驗證。當您定期重新產生金鑰作為一般安全性金鑰管理作法時，擁有兩組金鑰可確保您的應用程式保持為可用狀態。

如果您確實需要允許使用者對儲存體資源進行控管存取，則您可以建立共用存取簽章。共用存取簽章 (SAS) 是指可附加在 URL 後面的權杖，可提供儲存體資源的委派存取。在權杖有效的期限內，擁有權杖的任何人都可以存取它在指定權限中所指向的資源。從 2015-04-05 版開始，Azure 儲存體支援兩種共用存取簽章：服務 SAS 與帳戶 SAS。

服務 SAS 只會將存取權限委派給一種儲存體服務資源：Blob、佇列、資料表或檔案服務。

帳戶 SAS 則將存取權限委派給一或多個儲存體服務的資源。您可以將存取權限委派給服務 SAS 所沒有的服務層級作業。您也可以將 Blob 容器、資料表、佇列和檔案共用的讀取、寫入和刪除作業的存取權限，委派給本無權限的服務 SAS。

最後，您可以指定容器及其 Blob 或特定的 Blob 是否可供公用存取。當您將容器或 Blob 指定為公用時，任何人都可以進行匿名讀取；不需要驗證。公用容器和 Blob 對於公開資源 (例如網站上所託管的媒體和文件) 而言非常有用。若要縮短全球使用者的網路延遲，您可以使用 Azure CDN 來快取網站所用的 Blob 資料。

如需共用存取簽章的詳細資訊，請參閱[共用存取簽章：了解 SAS 模型](storage-dotnet-shared-access-signature-part-1.md)。如需安全存取儲存體帳戶的詳細資訊，請參閱[管理對容器與 Blob 的匿名讀取權限](storage-manage-access-to-resources.md)和 [Azure 儲存體服務的驗證](https://msdn.microsoft.com/library/azure/dd179428.aspx)。

## 持久性和高可用性的複寫

[AZURE.INCLUDE [storage-replication-options-include](../../includes/storage-replication-options-include.md)]

## 從 Azure 儲存體來回傳輸資料

您可以使用 AzCopy 命令列公用程式，在儲存體帳戶內或跨儲存體帳戶複製 Blob、檔案和資料表資料。如需詳細資訊，請參閱[使用 AzCopy 命令列公用程式傳輸資料](storage-use-azcopy.md)。

AzCopy 建置於 [Azure 資料移動程式庫](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)之上，其目前可供預覽。

Azure 匯入/匯出服務透過寄送至 Azure 資料中心的硬碟磁碟，提供將 Blob 資料匯入儲存體帳戶或加以從中匯出的方式。如需匯入/匯出服務的詳細資訊，請參閱[使用 Microsoft Azure 匯入/匯出服務將資料傳輸至 Blob 儲存體](storage-import-export-service.md)。

## 定價

客戶的 Azure 儲存體費用依據 4 項因素進行計算：使用的儲存體容量、選取的複寫選項、針對服務所做的要求次數，以及出口流量。

儲存體容量是指您用於儲存資料的儲存體帳戶配額。若只是儲存資料，則成本是由您所儲存的資料量和複寫資料的方式來決定。對 Azure 儲存體進行的每個讀取和寫入操作也會對服務提出要求。出口流量是指傳出 Microsoft Azure 區域的資料。當您儲存體帳戶中的資料受不同地區中執行的應用程式存取時，不論該應用程式是雲端服務還是其他某類應用程式，您都要負擔出口流量的費用。(若為 Microsoft Azure 服務，您可以採取步驟，將資料和服務群組在相同的資料中心，以減少或消除處理和出口流量費用)。

[Azure 儲存體價格](https://azure.microsoft.com/pricing/details/storage/)頁面提供了儲存體容量、複寫和交易的詳細價格資料。[資料傳輸定價詳細資料](https://azure.microsoft.com/pricing/details/data-transfers/)則提供了出口流量的詳細定價資訊。您可以使用 [Azure 儲存體定價計算機](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)，以協助消除成本。

## 儲存體 API、程式庫和工具

任何可提出 HTTP/HTTPS 要求的語言皆可存取 Azure 儲存體資源。另外，Azure 儲存體還提供了幾種熱門語言的程式設計程式庫。這些程式庫可透過處理詳細資料 (例如同步和非同步叫用、進行批次操作、例外狀況管理、自動重試、運作方式等等) 來簡化使用 Azure 儲存體的許多項目。程式庫目前適用於下列語言和平台，以及正在研發的其他語言和平台：

### Azure 儲存體資料服務

- [儲存體服務 REST API](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [適用於 .NET、Windows Phone 和 Windows 執行階段的儲存體用戶端程式庫](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp)
- [適用於 Java/Android 的儲存體用戶端程式庫](/develop/java/)
- [適用於 Node.js 的儲存體用戶端程式庫](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [適用於 PHP 的儲存體用戶端程式庫](/develop/php/)
- [適用於 Ruby 的儲存體用戶端程式庫](/develop/ruby/)
- [適用於 Python 的儲存體用戶端程式庫](/develop/python/)
- [PowerShell 1.0 的儲存體 Cmdlet](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### Azure 儲存體管理服務

- [儲存體資源提供者 REST API 參考](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [適用於 .NET 的儲存體資源提供者用戶端程式庫](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [適用於 PowerShell 1.0 的儲存體資源提供者 Cmdlet](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [儲存體服務管理 REST API (傳統)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure 儲存體資料移動服務 

- [儲存體匯入/匯出服務 REST API](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [適用於 .NET 的儲存體資料移動 用戶端程式庫](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### 工具和公用程式

- [Azure SDK 及工具](https://azure.microsoft.com/downloads/)
- [Azure 儲存體模擬器](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [AzCopy 命令列公用程式](http://aka.ms/downloadazcopy)

## 後續步驟

如需深入了解 Azure 儲存體，請探索這些資源：

### 文件

- [Azure 儲存體文件](https://azure.microsoft.com/documentation/services/storage/)

### 針對 PowerShell 使用者

- [搭配使用 Azure PowerShell 與 Azure 儲存體](storage-powershell-guide-full.md)

### 針對 .NET 開發人員

- [以 .NET 開始使用 Azure Blob 儲存體](storage-dotnet-how-to-use-blobs.md)
- [以 .NET 開始使用 Azure 表格儲存體](storage-dotnet-how-to-use-tables.md)
- [以 .NET 開始使用 Azure 佇列儲存體](storage-dotnet-how-to-use-queues.md)
- [在 Windows 上開始使用 Azure 檔案儲存體](storage-dotnet-how-to-use-files.md)

### 針對 Java/Android 開發人員

- [如何使用 Java 的 Blob 儲存體](storage-java-how-to-use-blob-storage.md)
- [如何使用 Java 的資料表儲存體](storage-java-how-to-use-table-storage.md)
- [如何使用 Java 的佇列儲存體](storage-java-how-to-use-queue-storage.md)
- [如何使用 Java 的檔案儲存體](storage-java-how-to-use-file-storage.md) 

### 針對 Node.js 開發人員

- [如何使用 Node.js 的 Blob 儲存體](storage-nodejs-how-to-use-blob-storage.md)
- [如何使用 Node.js 的資料表儲存體](storage-nodejs-how-to-use-table-storage.md)
- [如何使用 Node.js 的佇列儲存體](storage-nodejs-how-to-use-queues.md)

### 針對 PHP 開發人員

- [如何使用 PHP 的 Blob 儲存體](storage-php-how-to-use-blobs.md)
- [如何使用 PHP 的資料表儲存體](storage-php-how-to-use-table-storage.md)
- [如何使用 PHP 的佇列儲存體](storage-php-how-to-use-queues.md)

### 針對 Ruby 開發人員

- [如何使用 Ruby 的 Blob 儲存體](storage-ruby-how-to-use-blob-storage.md)
- [如何使用 Ruby 的資料表儲存體](storage-ruby-how-to-use-table-storage.md)
- [如何使用 Ruby 的佇列儲存體](storage-ruby-how-to-use-queue-storage.md)

### 針對 Python 開發人員

- [如何使用 Python 的 Blob 儲存體](storage-python-how-to-use-blob-storage.md)
- [如何使用 Python 的資料表儲存體](storage-python-how-to-use-table-storage.md)
- [如何使用 Python 的佇列儲存體](storage-python-how-to-use-queue-storage.md)
- [如何使用 Python 的檔案儲存體](storage-python-how-to-use-file-storage.md)

<!---HONumber=AcomDC_0309_2016-->