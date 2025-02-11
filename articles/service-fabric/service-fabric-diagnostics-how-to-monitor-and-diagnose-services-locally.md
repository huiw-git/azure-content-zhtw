<properties
   pageTitle="在本機監視和診斷使用 Azure Service Fabric 所撰寫的服務 | Microsoft Azure"
   description="了解如何監視和診斷在本機開發電腦上使用 Microsoft Azure Service Fabric 所撰寫的服務。"
   services="service-fabric"
   documentationCenter=".net"
   authors="kunaldsingh"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="02/12/2016"
   ms.author="toddabel"/>


# 監視和診斷本機開發設定中的服務
監視、偵測、診斷和疑難排解可讓服務繼續順利執行，盡可能減少服務中斷的使用者經驗。雖然監視和診斷在實際部署的生產環境中相當重要，然而效率將取決於服務開發期間所採用的類似模型服務，以便在您移動至實際設定時能確保其正常運作。Service Fabric 可讓服務開發人員輕鬆實作診斷，可以在單一電腦本機開發設定和實際生產叢集設定上順暢地工作。

## Windows 事件追蹤的優點
[Windows 事件追蹤](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) 是我們建議的技術，可用於追蹤 Service Fabric 中的訊息。這樣做的原因是：

* **ETW 相當快速。** 其是以一種追蹤技術建置而成，並對您程式碼執行時間的影響降到最低。
* **ETW 會在本機開發環境以及實際叢集設定順暢地進行追蹤。** 這表示當您準備好將程式碼部署至實際叢集時，您不需要重寫追蹤程式碼。
* **Service Fabric 系統程式碼也會將 ETW 用於內部追蹤。** 這可讓您檢視與 Service Fabric 系統追蹤交錯的應用程式追蹤。同時協助您更輕鬆了解在基礎系統中應用程式程式碼與事件之間的序列和相互關係。
* **內建支援的 Service Fabric Visual Studio 工具可供您檢視 ETW 事件。**


## 在 Visual Studio 中檢視 Service Fabric 系統事件

Service Fabric 會發出 ETW 事件，以協助應用程式開發人員了解平台中發生的事情。如果您還沒有這麼做，請繼續遵循[在 Visual Studio 中建立第一個應用程式](service-fabric-create-your-first-application-in-visual-studio.md)中的步驟。此資訊將協助您啟動應用程式，並執行可顯示追蹤訊息的 [診斷事件檢視器]。

1. 若 [診斷事件] 視窗不會自動顯示，請移至 Visual Studio 中的 [伺服器總管] 索引標籤，以滑鼠右鍵按一下 [Service Fabric 叢集]，並選擇內容功能表中的 [檢視診斷事件]。

  ![開啟 Visual Studio 診斷事件檢視器](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/ServerExViewDiagEvents.png)

2. 每個事件皆有標準的中繼資料資訊，可告訴您事件所來自的節點、應用程式和服務。您也可以使用事件視窗頂端的 [篩選事件] 方塊來篩選事件清單。例如，您可以依 [節點名稱] 或 [服務名稱] 進行篩選。 而當您查看事件詳細資料時，您也可以使用事件視窗頂端的 [暫停] 按鈕來暫停，稍後再繼續，而不會遺失任何事件。

  ![Visual Studio 診斷事件檢視器](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## 將您自己自訂的追蹤新增至應用程式程式碼
Service Fabric Visual Studio 專案範本包含範例程式碼。程式碼示範如何新增自訂的應用程式程式碼 ETW 追蹤，其會與來自 Service Fabric 的系統追蹤一併顯示在 Visual Studio ETW 檢視器中。這個方法的優點是中繼資料會自動新增至追蹤，且 Visual Studio 診斷事件檢視器已設定為顯示追蹤。

針對從**服務範本** (無狀態或可設定狀態) 所建立專案，只要搜尋 `RunAsync` 實作即可：

1. 使用 `RunAsync` 方法呼叫 `ServiceEventSource.Current.ServiceMessage` 時會顯示來自應用程式程式碼的自訂 ETW 追蹤範例。
2. 在 **ServiceEventSource.cs** 檔案中，您會找到 `ServiceEventSource.ServiceMessage` 方法的多載，出於效能因素，應該將其用於高頻率事件。

針對從**動作項目範本** (無狀態或可設定狀態) 所建立專案：

1. 開啟 **"ProjectName".cs** 檔案，其中 *ProjectName* 是您針對 Visual Studio 專案所選擇的名稱。  
2. 使用 *DoWorkAsync* 方法找到程式碼 `ActorEventSource.Current.ActorMessage(this, "Doing Work");`。這是來自應用程式程式碼撰寫的自訂 ETW 追蹤範例。  
3. 在 **ActorEventSource.cs** 中，您會找到 `ActorEventSource.ActorMessage` 方法的多載，出於效能因素，應該將其用於高頻率事件。

將自訂 ETW 追蹤新增至服務程式碼之後，您可以再次建置、部署，以及執行應用程式以查看診斷事件檢視器中的事件。如果您使用 **F5** 來偵錯應用程式，診斷事件檢視器將會自動開啟。

## 後續步驟
在 Azure 叢集上執行應用程式時，您針對本機診斷在上方應用程式所新增的相同追蹤程式碼，將會使用可以用來檢視這些事件的工具。請參閱下列文章，其中討論工具的不同選項，並說明如何設定這些工具。*[在 Azure 診斷和 Operational Insights 中從 Service Fabric 叢集收集記錄檔](service-fabric-diagnostics-how-to-setup-wad-operational-insights.md) * [使用 ElasticSearch 做為 Service Fabric 應用程式追蹤存放區](service-fabric-diagnostic-how-to-use-elasticsearch.md)

<!---HONumber=AcomDC_0224_2016-->