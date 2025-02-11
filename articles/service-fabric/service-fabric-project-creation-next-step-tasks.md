<properties
   pageTitle="Service Fabric 專案建立後續步驟 |Microsoft Azure"
   description="本文包含一組用於 Service Fabric 的核心開發工作連結"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="12/06/2015"
   ms.author="seanmck"/>

# 您的 Service Fabric 應用程式和後續步驟
您的 Azure Service Fabric 應用程式已經建立。本文說明您專案的建立方式和一些潛在後續步驟。

## 您的應用程式
每個新應用程式都包含一個應用程式專案。視所選擇的服務類型而定，可能會有一個或兩個額外的專案。

### 應用程式專案
應用程式專案包含：

- 一組對構成應用程式之服務的參考。

- 兩個發行設定檔 (本機和雲端)，可用來維護喜好設定，以搭配不同的環境使用，例如叢集端點的相關喜好設定，以及是否預設要執行升級部署。

- 兩個應用程式參數檔案 (本機和雲端)，可用來維護環境特定應用程式組態，例如為服務建立之分割區的數目。

- 部署指令碼，可用來從命令列部署您的應用程式，或透過自動化連續整合管線中部署您的應用程式。

- 應用程式資訊清單，描述應用程式。

### Reliable Services
當您加入一個新的 Reliable Service 時，Visual Studio 會將一個服務專案加入至您的解決方案。服務專案中包含從 `StatelessService` 或 `StatefulService` (視您選擇的類型而定) 延伸的類別。

### Reliable Actors
當您加入一個新的 Reliable Actor 時，Visual Studio 會將兩個專案加入至您的解決方案：動作項目專案和介面專案。

動作項目專案定義了動作項目類型及 (針對可設定狀態的動作項目) 其狀態。介面專案提供了其他服務可用來叫用動作項目的介面。

請注意，動作項目專案並不包含任何預設啟動行為，因為動作項目必須由其他服務啟用。請考慮加入一個 Reliable Service 或 ASP.NET 專案，以建立動作項目並與其互動。

### ASP.NET 5
針對用於 Service Fabric 應用程式而提供的 ASP.NET 5 範本與針對獨立建立的 ASP.NET 5 專案提供的範本幾乎完全相同。只有下列差異：

- 專案包含 **PackageRoot** 資料夾，用於儲存 ServiceManifest 檔案及資料和組態封裝。

- 專案參考額外的 NuGet 封裝 (Microsoft.ServiceFabric.AspNet.Hosting)，做為 .NET 執行環境 (DNX) 和 Service Fabric 之間的橋接器。

## 後續步驟
### 將 Web 前端新增至應用程式
Service Fabric 提供與 ASP.NET 5 的整合，可建置您應用程式的 Web 型進入點。如需了解如何根據 ASP.NET Web API 建立 REST 介面，請參閱[為應用程式新增 Web 前端][add-web-frontend]。

### 建立 Azure 叢集
Service Fabric SDK 提供一個用於開發和測試的本機叢集。如要在 Azure 中建立叢集，請參閱[從 Azure 入口網站設定 Service Fabric 叢集][create-cluster-in-portal]。

### 嘗試使用派對叢集部署到免費的 Azure

如果您想要嘗試在 Azure 中部署管理應用程式，而不設定自己的叢集，您可以使用免費的[派對叢集服務](http://aka.ms/tryservicefabric)。

### 將應用程式發行至 Azure
您可以直接從 Visual Studio 將應用程式發行至 Azure 叢集。若要瞭解做法，請參閱[將應用程式發行至 Azure][publish-app-to-azure]。

### 使用 Service Fabric 總管將叢集視覺化
「Service Fabric 總管」提供一個將叢集 (包括已部署的應用程式和實體配置) 視覺化的簡單方法。如要深入了解，請參閱[使用 Service Fabric 總管視覺化叢集][visualize-with-sfx]。

### 進行服務版本設定和升級
Service Fabric 可讓您為應用程式中的獨立服務進行獨立的版本設定和升級。若要深入了解，請參閱[進行服務版本設定和升級][app-upgrade-tutorial]。

### 使用 Visual Studio Team Services 設定持續整合
若要深入了解如何為 Service Fabric 應用程式設定持續整合程序，請參閱[使用 Visual Studio Team Services 設定持續整合][ci-with-vso]。



<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md

<!---HONumber=AcomDC_0204_2016-->