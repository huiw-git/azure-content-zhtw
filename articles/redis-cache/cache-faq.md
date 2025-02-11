<properties 
	pageTitle="Azure Redis 快取常見問題集" 
	description="了解 Azure Redis 快取常見問題、模式和最佳作法的答案" 
	services="redis-cache" 
	documentationCenter="" 
	authors="steved0x" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="cache" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="cache-redis" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/21/2016" 
	ms.author="sdanie"/>

# Azure Redis 快取常見問題集

了解 Azure Redis 快取常見問題、模式和最佳作法的答案。

<a name="cache-size"></a>
## 應該使用哪個 Redis 快取供應項目和大小？
每個 Azure Redis 快取供應項目提供不同層級的**大小**、**頻寬**、**高可用性**和 **SLA** 選項。

下列是選擇快取供應項目的考量。

-	**記憶體**：基本層和標準層提供 250 MB – 53 GB。高階層可提供多達 530 GB，[要求](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)時可提供更多。如需詳細資訊，請參閱 [Azure Redis 快取價格](https://azure.microsoft.com/pricing/details/cache/)。
-	**網路效能**：如果您的工作負載需要高輸送量，高階層提供的頻寬大於標準或基本層。此外，因為每一層內有裝載快取的基礎 VM，較大型快取還有更大頻寬。如需詳細資訊，請參閱[下列表格](#cache-performance)。
-	**輸送量**：進階層提供最大的可用輸送量。如果快取伺服器或用戶端達到頻寬限制，您會在用戶端上收到逾時。如需詳細資訊，請參閱下列表格。
-	**高可用性/SLA**：Azure Redis 快取保證標準/進階快取的可用性時間不低於 99.9%。若要深入了解我們的 SLA，請參閱 [Azure Redis 快取價格](https://azure.microsoft.com/support/legal/sla/cache/v1_0/)。SLA 的範圍僅涵蓋與快取端點的連線。SLA 未涵蓋資料遺失防護。建議您使用高階層中的 Redis 資料永續性功能，以增加資料遺失時的復原能力。
-	**Redis 資料持續性**：進階層可讓您將快取資料保存在 Azure 儲存體帳戶。在基本/標準快取中，所有資料都只儲存在記憶體中。如果基礎結構發生問題，資料可能會遺失。建議您使用高階層中的 Redis 資料永續性功能，以增加資料遺失時的復原能力。Azure Redis 快取在 Redis 永續性中提供 RDB 和 AOF (即將推出) 選項。如需詳細資訊，請參閱[如何設定進階 Azure Redis 快取的持續性](cache-how-to-premium-persistence.md)。
-	**Redis 叢集**：如果您想建立大於 53 GB 的快取，或想跨多個 Redis 節點共用資料，可以使用進階層中的 Redis 叢集。每個節點均包含一個主要/複本快取組以提供高可用性。如需詳細資訊，請參閱[如何設定進階 Azure Redis 快取叢集](cache-how-to-premium-clustering.md)。
-	**增強的安全性和網路隔離**：Azure 虛擬網路 (VNET) 部署可為您的 Azure Redis 快取、子網路、存取控制原則和其他功能提供增強的安全性和隔離，以進一步限制存取權。如需詳細資訊，請參閱[如何設定進階 Azure Redis 快取的虛擬網路支援](cache-how-to-premium-vnet.md)。
-	**設定 Redis**：在標準層和進階層中，您可以設定 Redis 以接收 Keyspace 通知。
-	**用戶端連線的最大數目**：進階層提供可連線至 Redis 的最大用戶端數目，針對較大型的快取有更高的連線數目。[如需詳細資訊，請參閱定價頁面](https://azure.microsoft.com/pricing/details/cache/)。
-	**Redis 伺服器的專用核心**：在進階層中，所有的快取大小均有 Redis 專用核心。在基本/標準層中，C1 以上的大小有 Redis 伺服器專用核心。
-	**Redis 為單一執行緒**，因此兩個以上核心所提供的優點與只有兩個核心相同，但較大的 VM 大小通常會比較小的有更多頻寬。如果快取伺服器或用戶端達到頻寬限制，則您會在用戶端上收到逾時。
-	**效能改良**：進階層中的快取是部署在處理器較快的硬體上，因此效能優於基本或標準層。高階層快取的輸送量較高，延遲性較低。

<a name="cache-performance"></a> 下表顯示從 Iaas VM 使用 `redis-benchmark.exe` 對 Azure Redis 快取端點測試各種標準和進階快取大小時觀察到的最大頻寬值。請注意，這些值並非保證值，亦無關於這些數字的 SLA，但應該是一般情況。您應該載入測試自己的應用程式，以判斷應用程式的正確快取大小。

我們可以從此表格中獲得下列結論。

-	相較於標準層，高階層中相同大小的快取，其輸送量較高。例如：針對 6 GB 快取，相較於 C3 的 49K，P1 的輸送量是 140K 個 RPS。
-	使用 Redis 叢集，當您增加叢集中的分區 (節點) 數目時，輸送量會呈線性增加。例如：如果您建立具有 10 個分區的 P4 叢集，則可用的輸送量為 250K *10 = 250 萬個 RPS
-	相較於標準層，高階層中的金鑰大小越大，輸送量就越高。

| 定價層 | 大小 | 可用的頻寬 | 1 KB 金鑰大小 |
|----------------------|--------|----------------------------|--------------------------------|
| **標準快取大小** | &nbsp; |**以 MB / 秒 (Mbps) 為單位** | **每秒要求數目 (RPS)** |
| C0 | 250 MB | 5 | 600 |
| C1 | 1 GB | 100 | 12200 |
| C2 | 2\.5 GB | 200 | 24000 |
| C3 | 6 GB | 400 | 49000 |
| C4 | 13 GB | 500 | 61000 |
| C5 | 26 GB | 1000 | 115000 |
| C6 | 53 GB | 2000 | 150000 |
| **高階快取大小** | &nbsp; | &nbsp; | **每一分區的每秒要求數目 (RPS)** |
| P1 | 6 GB | 1000 | 140000 |
| P2 | 13 GB | 2000 | 220000 |
| P3 | 26 GB | 2000 | 220000 |
| P4 | 53 GB | 4000 | 250000 |


如需下載 Redis 工具 (例如，`redis-benchmark.exe`) 的指示，請參閱[如何執行 Redis 命令？](#cache-commands)小節。

<a name="cache-region"></a>
## 我應該在哪個區域找到快取？

為獲得最佳效能和最低延遲，請在與快取用戶端應用程式相同的區域中找到 Azure Redis 快取。

<a name="cache-billing"></a>
## Azure Redis 快取如何收費？

[此處](https://azure.microsoft.com/pricing/details/cache/)提供 Azure Redis 快取價格。定價頁面所列的價格為每小時的費率。快取是根據從建立快取到刪除快取的時間，以分鐘為單位來收費。沒有用於停止或暫停快取收費的選項。

<a name="cache-timeouts"></a>
## 為什麼看到逾時？

用來與 Redis 溝通的用戶端發生逾時。在大多數的情況下，Redis 伺服器不會逾時。將命令傳送到 Redis 伺服器時，會將命令排入佇列，而且 Redis 伺服器最後會挑選並執行命令。不過，用戶端可能會在此程序期間逾時，而且，如果是這樣，則會在呼叫端引發例外狀況。如需疑難排解逾時問題的詳細資訊，請參閱[調查 StackExchange.Redis 中 Azure Redis 快取的逾時例外狀況](https://azure.microsoft.com/blog/2015/02/10/investigating-timeout-exceptions-in-stackexchange-redis-for-azure-redis-cache/)。

<a name="cache-monitor"></a>
## 如何監視快取的健全狀況和效能？

Microsoft Azure Redis 快取執行個體可以在 [Azure 入口網站](https://portal.azure.com)中進行監視。您可以檢視度量、將度量圖表釘選到「開始面板」、自訂監視圖表的日期和時間範圍、新增和移除圖表中的度量，以及設定符合特定條件時的警示。這些工具可讓您監視 Azure Redis 快取執行個體的健全狀況，並協助您管理快取應用程式。如需監視快取的詳細資訊，請參閱[監視 Azure Redis 快取](https://msdn.microsoft.com/library/azure/dn763945.aspx)。

<a name="cache-disconnect"></a>
## 我的用戶端為什麼中斷與快取的連線？

下面是快取中斷連線的一些常見原因。

-	用戶端原因
	-	已重新部署用戶端應用程式。
	-	用戶端應用程式已執行調整作業。
		-	如果是雲端服務或 Web Apps，這可能是自動調整所造成。
	-	用戶端上的網路層已變更。
	-	在用戶端或用戶端與伺服器之間的網路節點中，發生暫時性錯誤。
	-	已達頻寬閾值限制。
	-	CPU 繫結作業用了太多時間才完成。
-	伺服器端原因
	-	在標準快取供應項目上，Azure Redis 快取服務會起始從主要節點到次要節點的容錯移轉。
	-	Azure 會修補已部署快取的執行個體
		-	這可能適用於 Redis 伺服器更新或一般 VM 維護。

<a name="cache-configuration"></a>
## StackExchange.Redis 設定選項的作用為何？

StackExchange.Redis NuGet 封裝有許多選項。本節談論一些常見設定。如需 StackExchange.Redis 選項的詳細資訊，請參閱 [StackExchange.Redis 設定](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md)。

ConfigurationOptions|說明|建議
---|---|---
AbortOnConnectFail|設定為 true 時，在網路失敗之後不會重新連線。|設定為 false，並讓 StackExchange.Redis 自動重新連線。
ConnectRetry|初始連線期間的重複連線嘗試次數。||
ConnectTimeout|連線作業的逾時 (毫秒)。|

在大部分情況下，用戶端的預設值就已足夠。您可以根據工作負載來微調選項。

-	重試
	-	對於 ConnectRetry 和 ConnectTimeout，一般指引是快速失敗，然後再試一次。這根據您的工作負載以及用戶端發出 Redis 命令以及接收回應所需的平均時間。
	-	讓 StackExchange.Redis 自動重新連線，而不檢查連線狀態，並自行重新連線。**避免使用 ConnectionMultiplexer.IsConnected 屬性**。
	-	雪球處理 - 有時，您可能會在重試此雪球時碰到問題，永遠無法復原。在此情況下，您應該考慮使用指數輪詢重試演算法 (如 Microsoft Patterns & Practices 群組所發佈的[重試一般指引](https://github.com/mspnp/azure-guidance/blob/master/Retry-General.md)所述)。
-	逾時值
	-	請考慮您的工作負載，並據此設定值。如果您要儲存大的值，請將逾時設定為較高的值。
		-	將 ABortOnConnectFail 設定為 false，並讓 StackExchange.Redis 重新連線。
-	使用應用程式的單一 ConnectionMultiplexer 執行個體。您可以使用 LazyConnection 建立 Connection 屬性所傳回的單一執行個體 (如[使用 ConnectionMultiplexer 類別連線至快取](https://msdn.microsoft.com/library/azure/dn690521.aspx#Connect)所示)。
-	將 `ConnectionMultiplexer.ClientName` 屬性設定為應用程式執行個體唯一名稱，以進行診斷。
-	針對自訂工作負載，使用多個 `ConnectionMultiplexer` 執行個體。
	-	如果您的應用程式中有不同的負載，則可以遵循此模型。例如：
		-	您可以有一個多工器來處理大型索引鍵。 
		-	您可以有一個多工器來處理小型索引鍵。 
		-	您可以設定連線逾時的不同值，以及每個所使用 ConnectionMultiplexer 的重試邏輯。
		-	在每個多工器上設定 `ClientName` 屬性，以協助診斷。 
		-	這會導致每個 `ConnectionMultiplexer` 的更簡化延遲。

<a name="threadpool"></a>
## 執行緒集區成長的重要詳細資料

CLR 執行緒集區有兩種類型的執行緒：「背景工作」和「I/O 完成連接埠」(也稱為 IOCP) 執行緒。

-	背景工作執行緒用於處理 `Task.Run(…)` 或 `ThreadPool.QueueUserWorkItem(…)` 方法之類的作業。需要在背景執行緒上開啟工作時，CLR 中的各種元件也會使用這些執行緒。
-	發生非同步 IO (例如從網路讀取) 時，會使用 IOCP 執行緒。  

執行緒集區可視需要提供新背景工作執行緒或 I/O 完成執行緒 (而不需要任何節流)，直到它到達每個類型執行緒的「最低」設定。根據預設，執行緒的數目下限設為系統上的處理器數目。

一旦現有 (忙碌) 執行緒的數量達到執行緒集區的「最低」執行緒數目，執行緒集區會以每 500 毫秒將一個新執行緒插入執行緒的速率節流。這表示，如果您的系統需要 IOCP 執行緒的工作暴增，系統將可快速處理該工作。不過，如果暴增的工作超過設定的「最低」設定，部份工作的處理會發生一些延遲，因為執行緒集區會等待一或兩個狀況發生。

1. 現有的執行緒變得可用來處理工作。
1. 有 500 毫秒沒有現有的執行緒變得可用，因此會建立一個新的執行緒。

基本上，這表示，當忙碌執行緒數目超過「最低」執行緒數量時，在應用程式處理網路流量之前，您可能要支付 500 毫秒延遲。此外，務必注意，當現有的執行緒處於閒置的時間超過 15 秒 (根據我的記憶)，它將會被清除，而且這個循環的擴大和縮減可以重複。

如果我們查看來自 StackExchange.Redis (建置 1.0.450 或更新版本) 的範例錯誤訊息，您會看到它現在會列印執行緒集區統計資料 (請參閱以下的 IOCP 和背景工作詳細資料)。

	System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
	queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
	IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
	WORKER: (Busy=3,Free=997,Min=4,Max=1000)

在上述範例中，您可以看到 IOCP 執行緒有 6 個忙碌執行緒，而系統設定成允許最低 4 個執行緒。在此情況下，用戶端就可能會看到兩個 500 毫秒延遲，因為 6 > 4。

請注意，如果 IOCP 或背景工作執行緒的成長發生節流，StackExchange.Redis 可能達到逾時。

### 建議

經由這項資訊，我們強烈建議客戶將 IOCP 和背景工作執行緒的「最低」組態值設定成大於預設值的數值。對於這個值應該為何，我們無法提供一體適用的指引，因為某個應用程式的適用值對另一個應用程式來說將會太高/低。這項設定也會影響複雜應用程式其他部分的效能，因此每位客戶需要微調此設定來滿足其特定需求。200 或 300 是好的起點，那麼請測試並視需要調整。

如何設定這項設定：

-	在 ASP.NET 中，請使用 web.config 中 `<processModel>` 組態元素下的 ["minIoThreads" 組態設定][]。如果您在 Azure 網站內執行，此設定不會透過組態選項公開。不過，您應該仍然能夠透過 global.asax.cs 的 Application\_Start 方法以程式設計方式設定 (如下所示)。

> **重要事項：**這個組態元素中指定的值是*每一核心*設定。例如，如果您有 4 核心機器，並且想要在在執行階段將您的 minIOThreads 設定為 200，您會使用 `<processModel minIoThreads="50"/>`。

-	在 ASP.NET 之外，使用 [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API。

<a name="server-gc"></a>
## 使用 StackExchange.Redis 時啟用伺服器 GC 在用戶端上取得更多輸送量

啟用伺服器 GC 可以最佳化用戶端，並在使用 StackExchange.Redis 時提供較佳的效能和輸送量。如需有關伺服器 GC，以及如何加以啟用的詳細資訊，請參閱下列文件。

-	[啟用伺服器 GC](https://msdn.microsoft.com/library/ms229357.aspx)
-	[Fundamentals of Garbage Collection (記憶體回收的基本概念)](https://msdn.microsoft.com/library/ee787088.aspx)
-	[Garbage Collection and Performance (記憶體回收與效能)](https://msdn.microsoft.com/library/ee851764.aspx)

<a name="cache-redis-commands"></a>
## 使用常見 Redis 命令時的一些考量為何？

-	如果不了解需要花很長時間才能完成之特定 Redis 命令的影響，則不應該執行這些命令。
	-	例如，不要在生產環境中執行 [KEYS](http://redis.io/commands/keys) 命令，因為根據索引鍵數目，它可能會花很長時間才會傳回。Redis 是單一執行緒伺服器，並且一次處理一個命令。如果您在 KEYS 之後發出其他命令，則除非 Redis 處理 KEYS 命令，否則不會處理它們。
-	索引鍵大小 - 應該使用較小的索引鍵/值還是較大的索引鍵/值？ 一般而言，這取決於案例。如果您的案例需要較大的索引鍵，則可以調整 ConnectionTimeout，以及重試值並調整重試邏輯。從 Redis 伺服器的觀點，觀察到較小的值，即表示有較佳的效能。
	-	這不表示您無法在 Redis 中儲存較大的值；您必須注意下列考量。延遲會比較高。如果您有一個較大的資料集和一個較小的值，則可以使用多個 ConnectionMultiplexer 執行個體，而每個執行個體都會設定一組不同的逾時和重試值 (如先前的 [StackExchange.Redis 設定選項的作用為何](#cache-configuration)小節所述)。


<a name="cache-ssl"></a>
## 何時應該啟用非 SSL 連接埠來連線至 Redis？

Redis 伺服器不支援非預設 SSL，但是 Azure Redis 快取則支援。如果您是連線至 Azure Redis 快取，並且您的用戶端支援 SSL (例如 StackExchange.Redis)，則應該使用 SSL。

請注意，預設會停用新 Azure Redis 快取執行個體的非 SSL 連接埠。如果您的用戶端不支援 SSL，則必須遵循[在 Azure Redis 快取中設定快取](https://msdn.microsoft.com/library/azure/dn793612.aspx)文章之[存取連接埠](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts)小節中的指示來啟用非 SSL 連接埠。

Redis 工具 (例如 `redis-cli`) 未使用 SSL 連接埠，但您可以遵循[宣佈 Redis 預覽版本的 ASP.NET 工作階段狀態提供者](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)部落格文章中的指示，使用公用程式 (例如 `stunnel`) 將工具安全地連線至 SSL 連接埠。

如需下載 Redis 工具的指示，請參閱[如何執行 Redis 命令？](#cache-commands)小節。

<a name="cache-benchmarking"></a>
## 如何效能評定和測試我快取的效能？

-	[啟用快取診斷](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics)，以[監視](https://msdn.microsoft.com/library/azure/dn763945.aspx)您快取的健全狀況。您可以在 Azure 入口網站中檢視度量，也可以使用您選擇的工具[下載並檢閱](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring)它們。
-	您可以使用 redis-benchmark.exe 對 Redis 伺服器進行負載測試。
	-	請確定負載測試用戶端與 Redis 快取位於相同的區域。
-	使用 redis-cli.exe，並使用 INFO 命令來監視快取。
	-	如果您的負載造成高記憶體分散，則應該相應增加為較大的快取大小。
-	如需下載 Redis 工具的指示，請參閱[如何執行 Redis 命令？](#cache-commands)小節。

<a name="cache-commands"></a>
## 如何執行 Redis 命令？

您可以使用 [Redis 命令](http://redis.io/commands#)中列出的任何命令，但不包含 [Azure Redis 快取中不支援的 Redis 命令](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache)中列出的命令。您有幾種方式可以執行 Redis 命令。

-	如果您有標準或進階快取，就可以使用 [Redis 主控台](cache-configure.md#redis-console)來執行 Redis 命令。這可提供在 Azure 入口網站中執行 Redis 命令的安全方式。
-	您也可以使用 Redis 命令列工具。若要使用那些工具，請執行下列步驟。
	-	下載 [Redis 命令列工具](https://github.com/MSOpenTech/redis/releases/download/win-2.8.19.1/redis-2.8.19.zip)。
	-	使用 `redis-cli.exe` 連線至快取。使用-h 參數傳入快取端點，以及使用 -a 傳入索引鍵 (如下列範例所示)。
		-	`redis-cli -h <your cache name>.redis.cache.windows.net -a <key>`
	-	請注意，Redis 命令列工具未使用 SSL 連接埠，但您可以遵循[宣佈 Redis 預覽版本的 ASP.NET 工作階段狀態提供者](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)部落格文章中的指示，使用公用程式 (例如 `stunnel`) 將工具安全地連線至 SSL 連接埠。

<a name="cache-common-patterns"></a>
## 一些常見的快取模式和考量為何？

-	Microsoft Patterns & Practices 具有下列指引。
	-	[快取指引](https://github.com/mspnp/azure-guidance/blob/master/Caching.md)。
	-	[Azure 雲端應用程式設計和實作指引](https://github.com/mspnp/azure-guidance)
-	[Azure Redis 快取的常見快取模式](cache-howto-common-cache-patterns.md)

<a name="cache-reference"></a>
## Azure Redis 快取為什麼沒有像一些其他 Azure 服務的 MSDN 類別庫參考？

Microsoft Azure Redis 快取是基於受歡迎的開放原始碼 Redis 快取，可讓您存取 Microsoft 所管理的專用安全 Redis 快取。有各種 [Redis 用戶端](http://redis.io/clients)可供許多程式設計語言使用。每個用戶端都有它自己的 API，以使用 [Redis 命令](http://redis.io/commands)來呼叫 Redis 快取執行個體。

因為每個用戶端都不同，所以 MSDN 上沒有一個集中式類別參考；而是每個用戶端都會維護其專屬的參考文件。除了參考文件之外，Azure.com 上還會有數個教學課程，可顯示如何使用 [[Redis 快取文件](https://azure.microsoft.com/documentation/services/redis-cache/)] 頁面上的不同語言和快取用戶端來開始使用 Azure Redis 快取。


## 我適合使用哪個 Azure 快取服務？

>[AZURE.IMPORTANT] Microsoft 建議所有新的開發採用 Azure Redis Cache。

Azure 快取目前有三個提供項目：

-	Azure Redis 快取
-	Azure 受管理的快取服務
-	Azure In-Role Cache

>[AZURE.IMPORTANT]我們現在宣布將在 2016 年 11 月 30 日淘汰「Azure 受管理的快取服務」和 Azure In-Role Cache。我們建議您移轉至 Azure Redis 快取以為這次淘汰做準備。
>
>自服務正式運作以來，Azure Redis 快取一直是 Azure 中建議的快取解決方案，而且它現在可在所有的 Azure 區域中使用，包括中國和美國政府。基於此原因，我們要宣佈即將淘汰受管理的快取服務和 In-Role Cache 服務。
>
>自 2015 年 11 月 30 日宣佈之後，現有的客戶最多仍可使用受管理的快取服務和 In-Role Cache 服務 12 個月，兩個服務終止的日期將在 2016 年 11 月 30 日結束。在這個日期之後，將會關閉受管理的快取服務，而且將不再支援 In-Role Cache 服務。
>
>我們將在 2016 年 2 月 1 日之後發行的第一個 Azure SDK 版本中撤除建立新的角色中快取的支援。客戶將可以開啟具有角色中快取的現有專案。
>
>在此期間，我們建議所有現有受管理的快取服務和 In-Role Cache 服務客戶移轉至 Azure Redis 快取。Azure Redis 快取提供更多功能以及更好的整體價值。如需有關移轉的詳細資訊，請瀏覽[從受管理的快取服務移轉至 Azure Redis 快取](cache-migrate-to-redis.md)文件網頁。
>
>如果您有任何問題，請[連絡我們](https://azure.microsoft.com/support/options/?WT.mc_id=azurebg_email_Trans_933)。

### Azure Redis 快取
Azure Redis 快取已正式提供，大小最多 53 GB 和可用性 SLA 99.9%。新的[進階層](cache-premium-tier-intro.md)以 99.9% SLA 提供高達 530 GB 的大小，並且支援叢集、VNET 和持續性。

Azure Redis Cache 可以讓客戶使用 Microsoft 所管理的安全、專用 Redis 快取。使用這項提供項目，您可以利用 Redis 提供的豐富功能集和生態系統，並使用 Microsoft 提供的可靠託管及監控服務。

不同於僅處理金鑰-值組的傳統快取，Redis 受到歡迎是因為其高效能資料類型。Redis 也支援在這些類型上執行不可部分完成的作業，例如附加至字串、在雜湊中遞增值、推入至清單、計算集合交集、聯集和差異，或取得已排序集合中最高排名的成員。其他功能包括支援交易、發行/訂用、Lua 指令碼撰寫、存活時間有限的索引鍵及組態設定，可讓 Redis 的行為更像傳統快取。

Redis 成功的另一個重要層面是建置健全、有活力的開放原始碼生態系統。這會反映在跨多種語言可用的各式各樣 Redis 用戶端。您在 Azure 內建置的任何工作負載，幾乎都可使用它。

如需有關如何開始使用 Azure Redis 快取的詳細資訊，請參閱[如何使用 Azure Redis 快取](cache-dotnet-how-to-use-azure-redis-cache.md)和 [Azure Redis 快取文件](https://azure.microsoft.com/documentation/services/redis-cache/)。

### 受管理的快取服務
受管理的快取服務已設定於 2016 年 11 月 30 日淘汰。

### 角色中快取
In-Role Cache 已設定於 2016 年 11 月 30 日淘汰。

["minIoThreads" 組態設定]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx

<!---HONumber=AcomDC_0128_2016-->