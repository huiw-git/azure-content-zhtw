<properties
   pageTitle="設定開發環境 | Microsoft Azure"
   description="安裝執行階段、SDK 和工具，並建立本機開發叢集。完成此設定之後，您就可以開始建置應用程式。"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="02/09/2016"
   ms.author="seanmck"/>

# 準備您的開發環境
 若要在您的開發機器上建置並執行 [Azure Service Fabric 應用程式][1]，您需要安裝執行階段、SDK 和工具。您也必須執行 SDK 中包含的 Windows PowerShell 指令碼。

## 必要條件
### 支援的作業系統版本
下列為支援的作業系統版本：

- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10

### Visual Studio 2015

適用於 Service Fabric 的工具取決於 Visual Studio 2015，您可以在 [Visual Studio 網站][2]上找到。

> [AZURE.NOTE] 若您並未執行其中一個支援的作業系統版本，或不想在您的電腦上安裝 Visual Studio 2015，則您可以設定已預先安裝 Windows Server 2012 R2 和 Visual Studio 2015 的 Azure 虛擬機器。您可以使用 Azure 虛擬機器資源庫中的映像來執行這項操作。

## 安裝執行階段、SDK 和工具

Web Platform Installer 會執行 Service Fabric 元件的安裝。遵循這些指示來進行安裝：

1. 使用 Web Platform Installer [下載 SDK][3]。

2. 按一下 [安裝] 開始安裝程序。

3. 檢閱並接受使用者授權合約。

系統將自動繼續安裝。

## 啟用 PowerShell 指令碼執行

Service Fabric 會使用 Windows PowerShell 指令碼，以便建立本機開發叢集，以及從 Visual Studio 部署應用程式。根據預設，Windows 會封鎖這些指令碼的執行。若要啟用其，您必須修改 PowerShell 執行原則。以系統管理員身分開啟 PowerShell 並輸入下列命令：

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## 後續步驟
現在您的開發環境已完成設定，您可以開始建置和執行應用程式。

- [在 Visual Studio 中建立第一個 Service Fabric 應用程式](service-fabric-create-your-first-application-in-visual-studio.md)
- [了解如何在本機叢集上部署和管理應用程式](service-fabric-get-started-with-a-local-cluster.md)
- [深入了解程式設計模型：Reliable Services 和 Reliable Actors](service-fabric-choose-framework.md)
- [請查看 GitHub 上的 Service Fabric 程式碼範例](https://aka.ms/servicefabricsamples)
- [使用 Service Fabric 總管將叢集視覺化](service-fabric-visualizing-your-cluster.md)
- [遵循 Service Fabric 學習路徑來取得廣泛的平台簡介](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/campaigns/service-fabric/ "Service Fabric 活動頁面"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[3]: http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric "WebPI 連結"

<!---HONumber=AcomDC_0218_2016-->