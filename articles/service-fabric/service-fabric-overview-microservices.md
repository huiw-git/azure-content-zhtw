<properties
   pageTitle="了解微服務 | Microsoft Azure"
   description="概述在現代應用程式開發中，為何使用微服務方法建置雲端應用程式很重要，以及 Azure Service Fabric 如何提供平台以達成此目標"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="bscholl"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/18/2015"
   ms.author="mfussell"/>

# 為何要用微服務方式建置應用程式？
身為軟體開發人員，已熟悉思考如何將應用程式分解成元件部分。它是物件導向、軟體抽象化和元件化的中心範型。現在，這種分解經常以共用程式庫和技術層之間的類別與介面呈現，通常是透過一種分層方法，有後端存放區、中間層商務邏輯和前端 UI。過去幾年來的變化是身為開發人員的我們，開始為商業驅動的雲端建置分散式應用程式。

變更的商務需求包括：

- 需要大規模建立和操作一項服務，以吸引更大的客戶群 -- 例如，開拓新的地理區域，或不需要在客戶地點進行部署。
- 更快速提供特色與功能，靈活地回應客戶的需求。
- 提高資源使用率來降低成本。

這些商務需求會影響我們「如何」建置應用程式。

## 單體式與微服務設計方法
所有應用程式會隨著時間而演化。成功的應用程式因為有實用性而演化。失敗的應用程式不會演化，最後會取代。問題在於您現在對需求了解多少，以及未來有何變化？ 比方說，如果您為某個部門建置報告應用程式，並確定這將會保留於公司的範圍內，而且報告將只會短暫存在，則您選擇的方法將不同於建置服務來傳遞視訊內容給數千萬個客戶的方式。在已知後來可以重新設計應用程式的情況下，有時向外尋求概念證明才是驅動因素。從來不用卻過度設計並沒有太大意義。這就是一般所謂的工程取捨。另一方面，公司談論建置雲端時都會期望成長和使用量。問題在於無法預期成長和範圍。我們想要能夠迅速建立原型，同時還要了解我們正在通往未來成功的路徑上。這是精簡的創業方法：建置、測量、學習、反覆執行。

在主從架構的時代，我們傾向專注於建立分層式應用程式，每一層都採用特定的技術。已針對這些方法衍生出「單體式」應用程式一詞。介面通常存在於各層之間，而在一層內的各元件之間採用更緊密結合的設計。開發人員設計並分解類別，將類別編譯成程式庫，然後連結起來成為一些 exe 和 dll。這類單體式設計方法有一些優點。設計較簡單，元件之間通常透過 IPC 而呼叫更快。此外，每個人都只測試單一產品，人力資源運用更有效率。缺點是應用程式在分層之內緊密結合，您無法調整個別的元件。如果您需要執行修正或升級，則必須等候其他人完成其測試，更難以發揮靈活度。

微服務解決這些缺點，更密切配合上述的商務需求，但它們本身也都有優缺點。微服務的優點通常會各自封裝較簡單的商務功能，可各自獨立地相應增加或減少、測試、部署和管理。微服務方法的一個重要優點是團隊傾向於較具商務案例的導向，而不是分層方法所建議的技術導向。實際上，這表示較小的團隊會採用他們選擇的任何技術，根據客戶案例來開發微服務。換句話說，組織不需要為了維護單體性而將技術標準化。此外，擁有服務的個別團隊可以根據團隊的專業知識，或什麼是最適合服務所嘗試解決的問題，各自發揮所長。當然，實際上，最好有一組建議的技術，例如特定的 NoSQL 存放區或 Web 應用程式架構。微服務的缺點包括需要管理越來越多的個別實體、處理更複雜的部署和版本控制、讓微服務之間擁有更多的網路流量，以及相對應的網路延遲。經過大量論述之後，可知非常細微的服務是效能夢魘的解決良方。如果沒有工具協助檢視這些相依性，很難「看見」整個系統。最後，標準就是讓微服務方法奏效的關鍵、在通訊方式上達成共識，以及只在乎您需要從服務取得什麼，而不是僵固的合約。必須在設計的初期定義這些聯繫，因為之後服務就會各自獨立地更新。另一個在設計微服務方法時出現的描述是「細緻的 SOA」。


**簡而言之，微服務設計方法是低耦合的服務同盟，各自獨立變更，並達成一致的通訊標準。**


隨著越多的雲端應用程式產生，更多人發現這種將整體應用程式分解成獨立、案例焦點式服務的做法，在長期上是較好的方法。
## 應用程式開發方法的比較

![Service Fabric 平台應用程式開發][Image1]

1. 單體式應用程式包含領域特定功能，通常會依功能層來劃分，例如 Web、商務和資料。

2. 單體式應用程式可藉由複製到多部伺服器/VM/容器上進行擴充。

3. 微服務應用程式會將個別功能分隔成個別較小的服務。

4. 這種方法可獨立部署每個服務而相應放大，跨伺服器/VM/容器建立這些服務的執行個體。


使用微服務方法來設計並非所有專案的萬靈丹，但確實較符合稍早所述的商務目標。另外，如果您知道稍後有機會將程式碼修改為微服務設計，也許可以接受從使用單體式方法開始。較常見的情況是您一開始具有一個單體式應用程式，您可以從需要更高調整性或靈活度的功能領域開始，分階段緩慢地將此架構分解。

總而言之，微服務方法是以部署在叢集各處的容器中執行的許多較小服務來組成應用程式。每個服務都是由專責某個案例的較小團隊所開發，且每個服務各自獨立地測試、控制版本、部署和調整規模，所以應用程式可以整體演化。

## 什麼是微服務？

微服務有不同的定義，搜尋網際網路會找到許多很好的資源，各有其觀點和定義。但在微服務的下列大部分特性上，已廣泛達成共識：

- 它們會封裝客戶或商務案例。您要解決什麼問題？
- 它們是由小型工程團隊所開發。
- 它們可以使用任何程式設計語言來撰寫，以及使用任何架構。
- 它們是由獨立控制版本、部署及調整的程式碼和 (選擇性) 狀態所組成。
- 它們會透過定義完善的介面和通訊協定，與其他微服務互動。
- 它們具有可用來解析位置的唯一名稱 (URL)。
- 它們會在失敗時維持一致且可用的。

一言以蔽之：

**微服務應用程式是由獨立控制版本和可調整的客戶焦點式服務所組成，這些服務透過標準通訊協定和定義完善的介面彼此通訊。**


我們在上一節已說明前兩點，接下來將闡明並釐清其他各點。

### 可以使用任何程式設計語言撰寫並使用任何架構
身為開發人員，我們應該根據本身的技術或服務需求，自由選擇任何所需的語言或架構。在某些服務中，您可能認為 C++ 的效能優點勝於一切，而在其他服務中，C# 或 Java 的簡易管理開發可能才是最重要的。在某些情況下，您可能需要使用特定的協力廠商程式庫、資料儲存技術，或向用戶端公開服務的方式。

選擇技術之後，接下來的課題就是服務的操作或生命週期管理和調整。

### 允許獨立控制版本、部署及調整的程式碼和狀態  

不論您選擇何種方式來撰寫微服務，都應該針對程式碼和 (選擇性) 狀態，獨立地部署、升級及調整每個微服務。這確實是難以解決的問題之一，因為這涉及到您選擇的技術，以及在調整方面，需要了解如何分割 (或分區化) 程式碼和狀態。當程式碼和狀態使用不同的技術時 (目前的普遍情況)，微服務的部署指令碼必須能夠妥善調整兩者。這也關乎靈活度和彈性，如此您就能升級部分微服務，而不需一次全部升級。暫時回到單體式和微服務方法的比較，下圖顯示狀態儲存方法的差異。

#### 應用程式樣式之間的狀態儲存
![Service Fabric 平台狀態儲存體][Image2]

**左邊是單體式方法，有單一資料庫和多層的特定技術。**

**右邊是微服務方法，圖中顯示互連的微服務，其中狀態通常以微服務為範圍，並會使用各種技術。**

在單體式方法中，應用程式通常會使用單一資料庫。優點是這是單一位置，很容易部署。每個元件可以有單一資料表來儲存其狀態。困難之處是團隊必須嚴格區分狀態，無可避免地就想直接將新的資料行加入至現有的客戶資料表、在資料表之間執行聯結，通常會對儲存層形成相依性。一旦發生這種情況，您無法調整個別的元件。在微服務方法中，每個服務都會管理並儲存自己的狀態，這表示它負責同時調整程式碼和狀態，以滿足服務的需求。在需要對應用程式的資料建立任何檢視或查詢時，就能看出缺點，因為您必須跨這些不同的狀態存放區進行查詢。為了解決此問題，通常是由一個獨立的微服務建置一個跨許多微服務的檢視。如果您需要在資料上執行多個臨機操作查詢，則每個微服務應該考慮將其資料寫入資料倉儲服務，供離線分析。


版本控制是已部署的微服務版本所特有。它也是為了能夠推出和並行執行多個不同版本所不可或缺。當較新版的微服務在升級期間失敗，而需要回復至舊版時，版本控制可以解決這種情況。版本控制的另一種情況是執行 A/B 樣式測試，其中不同的使用者會體驗到不同版本的服務。比方說，在更廣泛推出新功能之前，通常會先針對一組特定的客戶升級微服務以測試新功能。在微服務的生命週期管理之後，這現在會將我們帶往它們之間的通訊。


### 透過定義完善的介面和通訊協定，與其他微服務互動

本主題無需太多著墨，請閱讀過去 10 年來發佈的大量服務導向架構文獻，其中有許多都以通訊模式為主題。一般而言，這現在可歸結到使用 REST 方法，並搭配 HTTP 和 TCP 通訊協定及 XML 或 JSON 做為序列化格式。從介面觀點來看，這是有關採用 Web 設計方法。但並不禁止您使用二進位通訊協定或您自己的資料格式。只是您公開的微服務會較難使用，要有心理準備。

### 具有可用來解析位置的唯一名稱 (URL)

別忘了，我們一直表示微服務如何像 Web 一樣？ 就像 Web 一樣，微服務不論在何處執行，都必須可定址。如果您正考慮要在哪一部電腦上執行特定的微服務，很快就會陷入困境。就像 DNS 解析特定電腦的特定 URL 一樣，微服務需要有唯一的名稱來探索它目前所在的位置。微服務需要有可定址的名稱，才能獨立於它們執行時所在的基礎結構之外。當然，這意味著服務的部署和探索方式之間會互相影響，因為需要有服務登錄。同樣地，發生電腦故障時和如何影響微服務之間，也需要有互相影響，登錄服務才能指出它現在執行的位置。接下來的課題：恢復能力和一致性。

### 失敗時維持一致且可用

處理非預期的失敗是最難解決的問題之一，特別是在分散式系統中。開發人員撰寫的程式碼大多是在處理例外狀況，這也是測試時花費最多時間的地方。但問題比撰寫程式碼來處理失敗更複雜。當執行微服務的電腦故障時，該怎麼辦？ 您不僅需要偵測此微服務失敗 (本身就是棘手的問題)，還需要設法重新啟動微服務。微服務必須能夠從失敗中恢復，基於可用性理由，通常還必須能夠在另一部電腦上重新啟動。這也涉及到代表微服務儲存了何種狀態、可從何處復原此狀態，以及是否能夠順利重新啟動它。換句話說，需要能夠恢復計算 (也就是重新啟動處理序)，以及狀態或資料的恢復能力 (不遺失任何資料，資料保持一致)。

在其他情況下，恢復能力的問題更難處理，例如應用程式升級期間失敗。在搭配部署系統一起運作時，微服務不僅需要復原。它也需要判斷是否可以繼續升級到較新版本，還是復原為舊版來維護一致的狀態。需要考慮一些問題，例如，是否有足夠的電腦可繼續升級，以及如何復原舊版的微服務。這需要微服務發出狀況資訊，才能做出這些決定。

### 報告狀況和診斷

微服務必須報告其健康狀態和診斷，這一點看似明顯，但卻經常被輕忽。否則，難以從操作觀點上深入了解。所面臨的挑戰是查看一組獨立服務的診斷事件之間的相互關聯 (每一個服務獨立記錄)，並修正電腦時間差異以了解事件順序。同樣地，透過同意的通訊協定和資料格式來與微服務互動時，需要將健康狀態和診斷事件的記錄方式標準化，這些事件最後會寫入可供查詢和檢視的事件存放區。在微服務方法中，關鍵在於不同團隊同意採用單一記錄格式，因為需要有一致的方法來檢視整個應用程式中的診斷事件。

狀況與診斷不同。健康狀態是指微服務報告其目前狀態，以便採取行動。最明顯的情況是使用升級和部署機制來維護可用性。例如，目前服務可能由於處理序損毀或重新開機而狀況不良，但仍可運作。您不應該執行升級而讓情況惡化。最好是先進行調查，或讓微服務有時間復原。因此，微服務的健康狀態事件可讓我們做出明智的決策，實際上有助於建立自我修復的服務。

## Service Fabric 做為微服務平台

Azure Service Fabric 源於 Microsoft 從提供盒裝產品 (通常是單體式) 轉換到提供服務。Service Fabric 主要是由建置和操作大型服務的經驗來推動，例如 Azure SQL 資料庫、DocumentDB 和其他核心 Azure 服務。我們完全貼近調整、靈活度、獨立團隊的商務需求，並讓平台隨著越來越多服務採用它而演化。重要的是，Service Fabric 必須在任何地方執行，不僅在 Azure 中，還有在獨立式 Windows Server 部署中。

**Service Fabric 的目標是解決建置和執行服務方面的艱難問題 (例如失敗與升級)，並有效率地利用基礎結構資源，讓團隊可以使用微服務方法來解決商務問題。**

Service Fabric 提供兩個廣泛的領域，協助您使用微服務方法來建置應用程式：

- 由一組系統服務所組成的平台，這些系統服務負責部署、升級、偵測並重新啟動失敗的服務、探索目前執行服務的位置、狀態管理、健康狀態監控等等。這些系統服務實際上具備上述微服務的許多特性。

-  協助您將應用程式建置成微服務的程式設計 API 或架構。提供的程式設計 API 稱為 [Reliable Actors 和 Reliable Services](service-fabric-choose-framework.md)。當然，您可以使用您選擇的任何程式碼來建置微服務。但使用這些 API 不僅可讓工作更簡單，也更深入地與平台整合。例如，您可以取得健康狀態和診斷資訊，或利用內建的高可用性。

***Service Fabric 不會限制您建置服務的方式，您可以使用任何技術。不過，它提供可讓您輕鬆建置微服務的內建程式設計 API。***

### 微服務適合我的應用程式嗎？

可能。根據我們的經驗，隨著 Microsoft 中越來越多團隊被告知基於商業理由，應該以雲端為目標來建置，有許多團隊都了解到採用類似微服務的方法所帶來的優點。例如，Bing 多年來在搜尋方面就一直這樣做。這對於其他團隊而言相當新穎。他們發現需要解決困難的問題，但這並非他們的強項。這就是為什麼 Service Fabric 受到重視而成為建置服務的最佳技術。

Service Fabric 的目標是將使用微服務方法建置應用程式時的複雜性降低，讓您不需要經歷許多耗費成本的重新設計工作。方法就是從小規模開始、需要時調整、淘汰服務、加入新服務、隨客戶用法而演化。我們也知道，為了讓微服務更易於為大部分開發人員所接受，事實上還有許多其他尚待解決的問題。容器和 Actor 程式設計模型都是朝此目標前進的一小步，我們確信將會浮現更多創新來輕鬆達成目標。<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 後續步驟

* 其他資訊：
	* [Service Fabric 概觀](service-fabric-overview.md)
	* [技術概觀](service-fabric-technical-overview.md)
* 設定 Service Fabric [開發環境](service-fabric-get-started.md)
* 為您的服務選擇[程式設計模型架構](service-fabric-choose-framework.md)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png

<!---HONumber=AcomDC_0107_2016-->