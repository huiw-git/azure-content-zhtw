<properties
	pageTitle="Azure VM 的計劃性維護 | Microsoft Azure"
	description="了解什麼是 Azure 計劃性維護，以及它會如何影響在 Azure 中執行的虛擬機器。"
	services="virtual-machines"
	documentationCenter=""
	authors="drewm"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/05/2016"
	ms.author="drewm"/>


# Azure 虛擬機器的計劃性維護

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Azure 執行計劃性維護的原因

為提升虛擬機器之基礎主機基礎結構的可靠性、效能和安全性，Microsoft Azure 會全球定期執行更新。許多這些更新在執行時並不會對虛擬機器或雲端服務造成任何影響，包括記憶體保留的更新。

不過，有些更新的確需要將虛擬機器重新開機，才能將必要更新套用至基礎結構。我們會在修補基礎結構時將虛擬機器關機，然後再將虛擬機器重新啟動。

請注意，有兩種類型的維護會對虛擬機器的可用性造成影響：規劃和非規劃性。本頁面說明 Microsoft Azure 會如何執行計劃性維護。如需非計劃性維護的詳細資訊，請參閱[了解計劃性與非計劃性維護](virtual-machines-manage-availability.md)。

## 記憶體保留的更新

對於 Microsoft Azure 中的某個更新類別，客戶不會看到其執行中的虛擬機器受到任何影響。許多這些更新都是針對元件或服務，更新時不會干擾執行中的執行個體。這些更新有些是主機作業系統上的平台基礎結構更新，不需要將虛擬機器完整重新開機就可以套用。

這些更新是透過可進行即時移轉 (「記憶體保留」更新) 的技術來完成。更新時，虛擬機器會進入「暫停」狀態，在 RAM 中保留記憶體，而基礎主機作業系統會收到必要的更新和修補程式。虛擬機器會在暫停後 30 秒內繼續執行。繼續執行後，系統將會自動同步化虛擬機器的時鐘。

並非所有更新都可以透過這種機制來部署，但因為有短暫的暫停期間，以這種方式部署更新可大幅減少對虛擬機器的影響。

多重執行個體更新 (針對可用性集合中的虛擬機器) 一次僅套用到一個更新網域。

## 虛擬機器組態

虛擬機器組態有兩種：多重執行個體和單一執行個體。在多重執行個體組態中，類似的虛擬機器會放置在可用性集合。

多重執行個體組態可提供跨實體機器、電源及網路的備援，因此我們建議您採用此組態以確保應用程式的可用性。可用性設定組中所有虛擬機器對您應用程式的用途都應該相同。

如需設定虛擬機器以取得高可用性的詳細資訊，請參閱[管理虛擬機器的可用性](virtual-machines-manage-availability.md)。

相較之下，單一執行個體組態會用於未放置在可用性集合的獨立虛擬機器。這些虛擬機器並不符合服務等級協定 (SLA) 的資格，服務等級協定需要在相同可用性集合底下部署兩部以上的虛擬機器。

如需 SLA 的詳細資訊，請參閱[服務等級協定](https://azure.microsoft.com/support/legal/sla/)中的〈雲端服務、虛擬機器和虛擬網路〉一節。


## 多重執行個體組態更新

計劃性維護期間，Azure 平台首先會更新裝載多重執行個體組態的虛擬機器集合。這會讓這些虛擬機器重新開機，而停機時間大約為 15 分鐘。

在多重執行個體組態更新中，系統會以保留可用性的方式來更新虛擬機器 (假設每部虛擬機器提供的功能都與集合中其他機器類似)。

基礎 Azure 平台會為可用性集合中的每部虛擬機器指派一個更新網域和一個容錯網域。每個更新網域是指一個虛擬機器群組，且群組內的虛擬機器會在相同的時間範圍內重新開機。每個容錯網域是指一個虛擬機器群組，且群組內的虛擬機器會共用通用電源和網路交換器。

如需有關更新網域和容錯網域的詳細資訊，請參閱[在可用性集合中設定多個虛擬機器以取得備援](virtual-machines-manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)。

若要防止更新網域同時離線，則需將更新網域中的每部虛擬機器關機、將更新套用至主機電腦、重新啟動虛擬機器以及移到下一個更新網域來執行維護。更新所有更新網域之後，規劃性維護事件才會結束。

規劃性維護期間的更新網域可能不會循序重新啟動，而是一次僅將一個更新網域重新開機。目前 Azure 對於多重執行個體組態中虛擬機器的計畫性維護作業，會提供 1 週的事前通知。

還原虛擬機器之後，以下是您的 Windows 事件檢視器可能會顯示的範例：

<!--Image reference-->
![][image2]

使用檢視器判斷哪些虛擬機器是使用 Azure 入口網站、Azure PowerShell 或 Azure CLI 設定於多執行個體設定中。比方說，若要判斷多執行個體設定中的虛擬機器，您可以將可用性設定組資料行加入虛擬機器瀏覽對話方塊來瀏覽虛擬機器清單。在下列範例中，Example-VM1 和 Example-VM2 虛擬機器的組態為多重執行個體組態：

<!--Image reference-->
![][image4]

## 單一執行個體組態更新

多重執行個體組態更新完成之後，Azure 將會執行單一執行個體組態更新。未在可用性集合中執行的虛擬機器也會因為此更新而重新開機。

請注意，即使在可用性集合中只有一個執行中的執行個體，Azure 平台仍會將其視為多重執行個體組態更新。

若是在單一執行個體組態中的虛擬機器，系統更新虛擬機器的方式是將虛擬機器關機、在主機機器上套用更新，然後重新啟動虛擬機器，而停機時間大約為 15 分鐘。區域中的所有虛擬機器會在單一維護期間執行這些更新。

此計劃性維護事件將會對此虛擬機器組態類型的應用程式可用性造成影響。在單一執行個體組態中，對於虛擬機器的計劃性維護，Azure 提供一週的事先通知。

### 電子郵件通知

Azure 只會針對單一執行個體和多重執行個體虛擬機器組態，事先傳送電子郵件來通知您即將到來的計畫性維護作業 (會提前 1 週通知您)。這封電子郵件將會傳送到訂用帳戶中提供的帳戶管理員和共同系統管理員電子郵件帳戶。以下是這種電子郵件類型的範例：

<!--Image reference-->
![][image1]

## 區域配對

Azure 在執行維護作業時，只會更新區域配對中單一區域的虛擬機器執行個體。舉例來說，Azure 在更新美國中北部的虛擬機器時，就不會同時更新任何在美國中南部的虛擬機器。這兩次更新作業會分別排定在不同的時間，以啟用區域之間的容錯移轉或負載平衡功能。不過，像是北歐等其他區域可以和美國東部一同進行維護。

請參閱下表以取得目前區域配對的相關資訊：

區域 1 | 區域 2
:----- | ------:
美國中北部 | 美國中南部
美國東部 | 美國西部
美國東部 2 | 美國中部
北歐 | 西歐
東南亞 | 東亞
中國東部 | 中國北部
日本東部 | 日本西部
巴西南部 | 美國中南部
澳洲東南部 | 澳洲東部
美國政府愛荷華州 | 美國政府維吉尼亞州

<!--Anchors-->
[image1]: ./media/virtual-machines-planned-maintenance/vmplanned1.png
[image2]: ./media/virtual-machines-planned-maintenance/EventViewerPostReboot.png
[image3]: ./media/virtual-machines-planned-maintenance/RegionPairs.PNG
[image4]: ./media/virtual-machines-planned-maintenance/AvailabilitySetExample.png


<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/

<!---HONumber=AcomDC_0204_2016-->