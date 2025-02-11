<properties
 pageTitle="管理 HPC Pack 叢集運算節點 |Microsoft Azure"
 description="了解可在 Azure 中新增、移除、啟動和停止 HPC Pack 叢集運算節點的 PowerShell 指令碼工具"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="01/08/2016"
 ms.author="danlep"/>

# 在 Azure 的 HPC Pack 叢集中管理運算節點的數目和可用性

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]資源管理員模型。


如果您已在 Azure VM 中建立 HPC Pack 叢集，您可能會需要可輕易地在叢集中新增、移除、啟動 (佈建) 或停止 (解除佈建) 多個運算節點 VM 的方法。若要執行這些工作，請執行安裝在前端節點 VM 上的 Azure PowerShell 指令碼 (自 HPC Pack 2012 R2 Update 1 開始)。這些指令碼可協助您控制 HPC Pack 叢集資源的數目和可用性，讓您得以控制成本。

>[AZURE.NOTE]這些指令碼位於前端節點上的 %CCP\_HOME%bin 資料夾中。您必須以系統管理員身分執行每個指令碼。

## 必要條件

* **Azure VM 中的 HPC Pack 叢集** - 使用 HPC Pack 2012 R2 Update 1 或更新版本，在傳統 (服務管理) 部署模型中建立 HPC Pack 叢集。例如，您可以使用 Azure Marketplace 中的 HPC Pack VM 映像和 Azure PowerShell 指令碼，將部署自動化。如需相關資訊和必要條件，請參閱[使用 HPC Pack IaaS 部署指令碼建立 HPC 叢集](virtual-machines-hpcpack-cluster-powershell-script.md)。

* **Azure 發佈設定檔或管理憑證** - 您必須前端節點上執行下列其中一項：

    * **匯入 Azure 發佈設定檔**。若要這麼做，請在前端節點上執行下列 Azure PowerShell Cmdlet：

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **前端節點上設定 Azure 管理憑證**。如果您有 .cer 檔案，請在 CurrentUser\\My certificate store 中將其匯入，並為您的 Azure 環境 (AzureCloud 或 AzureChinaCloud) 執行下列 Azure PowerShell Cmdlet：

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## 新增運算節點 VM

使用 **Add-HpcIaaSNode.ps1** 指令碼新增運算節點。

### 語法
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### 參數

* **ServiceName** - 將會新增運算節點 VM 之雲端服務的名稱。

* **ImageName** - Azure VM 映像名稱，透過 Azure 傳統入口網站或 Azure PowerShell Cmdlet **Get-AzureVMImage** 可以取得此名稱。這些映像必須符合下列需求：

    1. 必須安裝 Windows 作業系統。

    2. 必須在運算節點角色中安裝 HPC Pack。

    3. 映像必須是使用者類別中的私人映像，而不是公用 Azure VM 映像。

* **Quantity**- 要新增的運算節點 VM 數目。

* **InstanceSize** - 運算節點 VM 的大小。

* **DomainUserName** - 網域使用者名稱，會用來將新的 VM 加入網域中。

* **DomainUserPassword** - 網域使用者的密碼。

* **NodeNameSeries** (選用) - 計算節點的命名模式。格式必須是 &lt;*Root\_Name*&gt;&lt;*Start\_Number*&gt;%。例如，MyCN%10% 表示從 MyCN11 開始的一系列運算節點名稱。如果未指定，指令碼會使用 HPC 叢集中已設定的節點命名序列。

### 範例

下列範例會根據 VM 映像 *hpccnimage1*，在雲端服務 *hpcservice1* 中新增 20 個大型計算節點 VM。

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## 移除運算節點 VM

使用 **Remove-HpcIaaSNode.ps1** 指令碼移除運算節點。

### 語法

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### 參數

* **Name** - 要移除之叢集節點的名稱。支援萬用字元。參數集名稱是 Name。您無法同時指定 **Name** 和 **Node** 參數。

* **Node** - 要移除之節點的 HpcNode 物件，可透過 HPC PowerShell Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx) 取得。參數集名稱是 Node。您無法同時指定 **Name** 和 **Node** 參數。

* **DeleteVHD** (選擇性) - 針對已移除的 VM 進行相關磁碟的刪除時所使用的設定。

* **Force** (選擇性) - 在移除 HPC 節點前強制使其離線的設定。

* **Confirm** (選擇性) - 執行命令前先行確認的提示。

* **WhatIf** - 用來說明您所執行的命令未實際執行時將會有何狀況的設定。

### 範例

下列範例會使名稱開頭為 *HPCNode-CN-* 的節點強制離線，然後移除節點及其相關聯的磁碟。

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## 啟動運算節點 VM

使用 **Start-HpcIaaSNode.ps1** 指令碼啟動運算節點。

### 語法

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### 參數

* **Name** - 要啟動之叢集節點的名稱。支援萬用字元。參數集名稱是 Name。您無法同時指定 **Name** 和 **Node** 參數。

* **Node** - 要啟動之節點的 HpcNode 物件，可透過 HPC PowerShell Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx) 取得。參數集名稱是 Node。您無法同時指定 **Name** 和 **Node** 參數。

### 範例

下列範例會啟動名稱開頭為 *HPCNode-CN-* 的節點。

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## 停止運算節點 VM

使用 **Stop-HpcIaaSNode.ps1** 指令碼停止計算節點。

### 語法

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### 參數


* **Name** - 要停止之叢集節點的名稱。支援萬用字元。參數集名稱是 Name。您無法同時指定 **Name** 和 **Node** 參數。

* **Node** - 要停止之節點的 HpcNode 物件，可透過 HPC PowerShell Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx) 取得。參數集名稱是 Node。您無法同時指定 **Name** 和 **Node** 參數。

* **Force** (選擇性) - 在停止 HPC 節點前強制使其離線的設定。

### 範例

下列範例會使名稱開頭為 *HPCNode-CN-* 的節點強制離線，然後停止節點。

Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force

## 後續步驟

* 如果您需要能根據叢集上目前的工作和任務的工作負載自動增加和縮減叢集節點的方法，請參閱[在 HPC Pack 叢集中自動增加和縮減 Azure 計算資源](virtual-machines-hpcpack-cluster-node-autogrowshrink.md)。

<!---HONumber=AcomDC_0114_2016-->