<properties
   pageTitle="Testability 概觀 | Microsoft Azure"
   description="本文描述 Service Fabric 中的 Testability 子系統，以引發錯誤和針對您的服務執行測試案例。"
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="01/26/2016"
   ms.author="rsinha"/>

# Testability 概觀

Testability 子系統的設計目的是測試建置於 Microsoft Azure Service Fabric 上的服務。利用 Testability，您可以產生有意義的錯誤，並執行案例。這些錯誤及案例會在受控制、安全且一致的情況下，執行及驗證服務在其生命週期會發生的各種狀態和轉換情形。

Testability 提供啟用這些功能的動作和案例。動作是個別錯誤，可鎖定服務以進行測試。服務開發人員可以使用這些動作當做建置組塊，來撰寫複雜的案例。例如：

  * 重新啟動節點，以模擬任意次數的機器或 VM 重新開機情況。

  * 移動您的具狀態服務複本，以模擬負載平衡、容錯移轉或應用程式升級。

  * 在具狀態服務上叫用仲裁遺失，以建立無法繼續進行寫入作業的狀況，因為可接受新資料的「備份」或「次要」複本不足。

  * 在具狀態服務上叫用資料遺失，以建立所有記憶體內狀態為完全抹除的狀況。

案例都是一個或多個動作組成的複雜作業。因為這些動作是 PowerShell 命令和 C# API 呼叫，所以案例可以採取任何形式：長時間執行的服務、PowerShell 命令、命令列應用程式等等。在 Testability 中我們提供兩個現成可用的案例：

  * 混亂案例
  * 容錯移轉案例

Testability 會公開 PowerShell 和 C# API。這可讓服務開發人員在有需要時，能更敏捷地使用 PowerShell 指令碼，以及更有效地控制 C# API。

## Testability 的重要性

Service Fabric 大幅簡化了撰寫和管理分散式可擴充應用程式的工作。同樣地，Service Fabric 中的 Testability 子系統也讓測試分散式應用程式更加輕鬆。測試時，有三個主要問題需要解決：

1. 模擬/產生真實案例中可能發生的失敗：Service Fabric 其中一個重要的層面是，它讓分散式應用程式從各種不同的失敗中復原。但為了測試應用程式是否可以從這些失敗中復原，我們需要能夠在受控制的測試環境中模擬/產生這些真實失敗情形的機制。

2. 能夠產生相互關聯的失敗：系統中的基本失敗，例如網路故障和機器故障，很容易個別產生。由於這些個別失敗的互動是重要的，所以會產生大量實際會發生的案例。

3. 跨各種開發和部署層級的統一經驗：許多錯誤插入系統都能讓您執行各種類型的失敗。不過，當您從一整體開發人員案例，移至大型測試環境中執行相同測試，以將案例用在生產環境中測試時，經驗就會不佳。

雖然有許多可解決這些問題的機制，但是在從一整體開發人員環境到在生產環境叢集進行測試的過程中，卻缺少能以必要保證發揮相同作用的系統。Testability 子系統可讓應用程式開發人員專注於測試他們的商務邏輯。Testability 提供所有必要功能，以測試服務與基礎分散式系統的互動。

### 模擬/產生真實失敗案例

若要測試分散式系統抵抗失敗的強健性，我們需要一個可產生失敗的機制。雖然理論上產生失敗似乎很簡單，例如節點停機，卻也發生了與 Service Fabric 正嘗試解決的同一組一致性問題。如果我們想要將關閉節點做為範例，則需要的工作流程如下：

1. 從用戶端發出關閉節點要求。

2. 將要求傳送至正確的節點。

    a.如果找不到節點，要求應該會失敗。

    b.如果找到節點，應該只會在節點關閉時才傳回要求。

若要從測試角度確認失敗，測試必須知道在導致此失敗時，失敗確實會發生。Service Fabric 提供的保證就是，當命令到達該節點時，節點會停機，或是已經停機。在任一情況中，測試應該都能正確地推論出狀態，以及正確地推斷驗證是否成功。在 Service Fabric 外部實作的系統如果執行一組相同的失敗，可能會發生許多網路、硬體和軟體問題，這會導致無法提供上述的保證。如果出現前述問題，Service Fabric 會重設叢集狀態以因應問題，使 Testability 子系統仍然可以提供一組適當的保證。

### 產生必要的事件和案例

雖然一致地模擬真實失敗情形並不容易著手，但可以產生相互關聯的失敗更是難上加難。舉例來說，發生下列狀況時，具狀態的持續性服務中會遺失資料：

1. 只有複本寫入仲裁會在複寫中被趕上。所有次要複本均落後於主要複本。

2. 因為複本停止運作 (由於程式碼封裝或節點停止運作)，導致寫入仲裁停止運作。

3. 因為複本的資料遺失 (由於磁碟損毀或機器重新安裝映像)，寫入仲裁無法恢復。

這些相互關聯的失敗確實會在真實情況中發生，但是不像個別失敗那麼頻繁。在這些案例於生產環境中發生前，能夠先測試這些案例會很重要。更重要的是在受控制的情況下 (所有工程師都在待命的白天時)，能夠在生產環境的工作負載中模擬這些案例。這樣會比生產環境在清晨 2 點第一次發生這些案例要來得更好。

### 跨不同環境的統一經驗

傳統作法已建立了三組不同的經驗，一個用於開發環境、一個用於測試環境，另一個則用於生產環境。此模型為：

1. 在開發環境中產生狀態轉換，讓個別方法能進行單元測試。

2. 在測試環境中產生失敗，以使用練習各種失敗案例的端對端測試。

3. 將生產環境保留原狀，以防止任何人為失敗，並確保有非常快速的人力可回應失敗。

我們打算在 Service Fabric 中透過 Testability 子系統扭轉此情形，並從開發人員環境到生產環境中使用相同的方法。作法有二：

1. 若要引發受控制的失敗，請在一整體環境到生產環境叢集中，一律使用 Testability API。

2. 若要讓叢集故障，使失敗自動發生，請使用 Testability 子系統產生自動失敗。透過組態來控制失敗率，可讓不同環境以不同的方式測試同一個服務。

在 Service Fabric 中，雖然失敗的規模會因為環境不同而有所差異，但實際機制是相同的。這不但可讓您用更快的程式碼部署管線，還能在真實情況處於負載時測試服務。

## 使用 Testability

### 以 C 使用 Testability#

System.Fabric.Testability.dll 中有 Testability 功能。此 dll 位於 Microsoft.ServiceFabric.Testability.nupack 的 NuGet 封裝中。若要使用 Testability 功能，請在您的專案中包含此 NuGet 封裝做為參考。

### 以 PowerShell 使用 Testability

若要使用 Testability PowerShell，您必須安裝執行階段 MSI。安裝 MSI 後，會自動載入 ServiceFabricTestability PowerShell 模組供開發人員使用。

## 結論

為了建立真實的雲端規模調整服務，能夠在部署前後確保服務可以承受真實失敗是不可或缺的。對現今的服務產業而言，具有快速創新以及快速將程式碼移至生產環境的能力非常重要。Service Fabric Testability 正好能協助服務開發人員這麼做。

## 後續步驟

- [Testability 動作](service-fabric-testability-actions.md)
- [Testability 案例](service-fabric-testability-actions.md)
- 如何測試您的服務
  - [模擬服務工作負載期間的失敗案例](service-fabric-testability-workload-tests.md)
  - [服務對服務間通訊的失敗案例](service-fabric-testability-scenarios-service-communication.md)

<!---HONumber=AcomDC_0204_2016-->