<properties
	pageTitle="業務持續性和災害復原 (BCDR)：Azure 配對的區域 | Microsoft Azure"
	description="Azure 區域配對可確保當資料中心發生故障時應用程式可復原。"
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="raynew"/>

# 業務持續性和災害復原 (BCDR)：Azure 配對的區域

## 什麼是配對的區域？

Azure 能在世界各地多個地理位置運作。Azure 地理位置是包含至少一個 Azure 區域的已定義世界區域。Azure 區域是包含一或多個資料中心之地理位置內的區域。

每個 Azure 區域都會與相同地理位置內的其他區域配對 (巴西南部除外，其與自身地理位置外的區域配對)，因此形成了區域配對。


![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

圖 1 – Azure 區域配對圖表



| [地理位置] | 配對的區域 | |
| :-------------| :-------------   | :-------------   |
| 北美洲 | 美國中北部 | 美國中南部 |
| 北美洲 | 美國東部 | 美國西部 |
| 北美洲 | 美國東部 2 | 美國中部 |
| 歐洲 | 北歐 | 西歐 |
| 亞洲 | 東南亞 | 東亞 |
| 中國 | 中國東部 | 中國北部 |
| 日本 | 日本東部 | 日本西部 |
| 巴西 | 巴西南部 (1) | 美國中南部 |
| 澳大利亞 | 澳洲東部 | 澳洲東南部|
| 美國政府 | 美國政府愛荷華州 | 美國政府維吉尼亞州 |
| 印度 | 印度中部 | 印度南部 |

表 1 - Azure 區域配對對應表

> (1) 巴西南部與其他區域的不同點在於，其與自身地理位置以外的區域配對。請注意，巴西南部的次要區域是美國中南部，但是美國中南部的次要區域並不是巴西南部。

我們建議您複寫跨區域配對的工作負載，以善用 Azure 的隔離與可用性原則。例如，預定的 Azure 系統更新會跨配對區域循序部署 (並非同時)。這表示即使面臨罕見的更新錯誤事件，兩個區域也不會同時受到影響。此外，若遭遇少見的廣泛中斷事件，就能至少優先復原所有配對中的其中一個區域。

## 配對的區域範例
以下的圖 2 顯示使用地區配對以進行災害復原的假設應用程式。綠色的數字強調了三項 Azure 服務 (Azure 計算、儲存體和資料庫) 的跨區域活動，以及這些服務設定為跨區域複寫的方式。橘色數字則強調跨配對區域部署的獨特優點。


![配對區域優點概觀](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

圖 2 – 假設的 Azure 區域配對

## 跨區域活動
如圖 2 所示。

![1Green](./media/best-practices-availability-paired-regions/1Green.png) **Azure 計算 (PaaS)** – 您必須佈建額外的計算資源，以便確保發生嚴重損壞時資源可在其他區域中使用。如需詳細資訊，請參閱 [Azure 業務持續性技術指引](https://msdn.microsoft.com/library/azure/hh873027.aspx)

![2Green](./media/best-practices-availability-paired-regions/2Green.png) **Azure 儲存體** - 建立 Azure 儲存體帳戶時，系統預設會設定異地備援儲存體 (GRS)。使用 GRS 時，系統會在主要區域內將您的資料自動複寫三次，並在配對區域中複寫三次。如需詳細資訊，請參閱 [Azure 儲存體備援選項](../storage/storage-redundancy.md)。


![3Green](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL Database** – 使用 Azure SQL 標準異地複寫，您就可以設定交易至配對區域的非同步複寫。使用高階異地複寫，您就可以設定複寫至世界上任何區域。不過，我們建議您在配對區域中，為大部分的災害復原案例部署這些資源。如需詳細資訊，請參閱 [Azure SQL Database 中的異地複寫](https://msdn.microsoft.com/library/azure/dn783447.aspx)

![4Green](./media/best-practices-availability-paired-regions/4Green.png) **Azure 資源管理員 (ARM)** - ARM 原本就會跨區域提供服務管理元件的邏輯隔離。這表示某個區域中的邏輯失敗不太可能會影響另一個區域。

## 配對區域的優點
如圖 2 所示。

![5Orange](./media/best-practices-availability-paired-regions/5Orange.png) **實體隔離** – 在可能的情況下，Azure 會偏好區域配對中的資料中心之間距離至少要相隔 300 英哩，但是這在所有地理位置中並不實際也不可能。實體資料中心分隔能夠降低自然災害、社會動亂、電力中斷或實體網路中斷同時影響兩個區域的可能性。隔離會受限於地理位置內的條件約束 (地理位置大小、電源/網路基礎結構可用性、法規等等)。

![6Orange](./media/best-practices-availability-paired-regions/6Orange.png) **平台提供的複寫** - 異地備援儲存體之類的部分服務會提供自動複寫到配對的區域。

![7Orange](./media/best-practices-availability-paired-regions/7Orange.png) **區域復原順序** – 若發生廣泛中斷事件，會優先復原所有配對中的一個區域。跨配對區域部署的應用程式能夠保障其中一個區域優先復原。如果在未配對的區域中部署應用程式，就可能會發生復原延遲的情況；最壞的情況是，這兩個選定的區域可能都不會被復原。

![8Orange](./media/best-practices-availability-paired-regions/8Orange.png) **循序更新** – 預定的 Azure 系統更新會循序發行至配對的區域 (並非同時)，以便在出現罕見的不正確更新時，將停機時間、錯誤影響和邏輯故障的影響降到最低。


![9Orange](./media/best-practices-availability-paired-regions/9Orange.png) **資料常駐地** - 區域會駐留在相同的地理位置之內形成配對 (巴西南部除外)，以符合資料常駐地之稅務和執法管轄區的要求。

<!---HONumber=AcomDC_0121_2016-->