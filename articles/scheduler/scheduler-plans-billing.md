<properties 
 pageTitle="Azure 排程器的計劃和計費" 
 description="" 
 services="scheduler" 
 documentationCenter=".NET" 
 authors="krisragh" 
 manager="dwrede" 
 editor=""/>
<tags 
 ms.service="scheduler" 
 ms.workload="infrastructure-services" 
 ms.tgt_pltfrm="na" 
 ms.devlang="dotnet" 
 ms.topic="article" 
 ms.date="12/04/2015" 
 ms.author="krisragh"/>
 
# Azure 排程器的計劃和計費

## 工作集合計劃

工作集合是 Azure 排程器中可計費的實體。工作集合包含許多工作，並分成三個計劃 (免費、標準和高階)，如下所述。

|**工作集合計劃**|**每個工作集合的工作數上限**|**最大週期**|**每個訂用帳戶的工作集合數上限**|**限制**|
|:---|:---|:---|:---|:---|
|**免費**|每個工作集合 5 個工作|每小時一次。執行工作不得超過一個小時|允許 1 個訂用帳戶最多 1 個免費工作集合|無法使用 [HTTP 輸出授權物件](scheduler-outbound-authentication.md)
|**標準**|每個工作集合 50 個工作|每分鐘一次。執行工作不得超過一分鐘|允許一個訂用帳戶最多 100 個標準工作集合|存取排程器的完整功能集|
|**高級**|每個工作集合 50 個工作|每分鐘一次。執行工作不得超過一分鐘|允許一個訂用帳戶最多 10,000 個高階工作集合如需詳細資訊，請<a href="mailto:wapteams@microsoft.com">與我們連絡</a>。|存取排程器的完整功能集|

## 升級或降級工作集合計劃

您隨時可在免費、標準和高階計劃之間升級或降級工作集合計畫。不過，當降級至免費工作集合時，降級可能由於下列其中一個原因而失敗：

- 免費工作集合已存在於訂用帳戶中
- 工作集合中的工作有更高的週期，超過免費工作集合中工作允許的週期。免費工作集合中允許的最大週期為每小時一次
- 工作集合中有 5 個以上的工作
- 工作集合中的工作具有一個使用 [HTTP 輸出授權物件](scheduler-outbound-authentication.md)的 HTTP 或 HTTPS 動作。

## 計費與 Azure 計劃

不會對免費工作集合的訂用帳戶計費。如果您有 100 個以上的標準工作集合 (10 個標準計費單位)，則具有高階計劃中的所有工作集合，這是更好的方式。

如果有一個標準工作集合和一個高階工作集合，則您會支付一個標準計費單位_和_ 一個高階計費單位。排程器服務會根據設定為標準或高階的作用中工作集合數目來計費；下兩節將有進一步的說明。

## 標準可計費單位

標準可計費單位可以包含最多 10 個標準工作集合。由於標準工作集合可以讓每個工作集合最多 50 個工作，因此一個標準計費單位可讓一個訂用帳戶最多具有 500 個工作 – 每個月幾乎高達 2 千 2 百萬個工作執行。

如果有 1 到 10 個標準工作集合，則您將支付 1 個標準計費單位。如果有 11 到 20 個標準工作集合，則您將支付 2 個標準計費單位。如果有 21 到 30 個標準工作集合，則您將支付 3 個標準計費單位。

## 高級可計費單位

高級可計費單位可以包含最多 10,000 個高級工作集合。由於高級工作集合可以讓每個工作集合最多 50 個工作，因此一個高級計費單位可讓一個訂用帳戶最多具有 500,000 個工作 – 每個月幾乎高達 2 百 2 十億個工作執行。

如果有 1 到 10,000 個高級工作集合，則您將支付 1 個高級計費單位。如果有 10,001 到 20,000 個高級工作集合，則您將支付 2 個高級計費單位。

因此，高階工作集合具有與標準工作集合相同的功能，但萬一您的應用程式需要大量工作集合時提供折價。

## 計費和作用中狀態

工作集合會永遠作用中，除非您整個訂用帳戶由於計費問題而進入部分暫時停用狀態。確保工作集合不計費的唯一方法，就是將它設定成_免費_計劃或刪除工作集合。

雖然您可能在單一作業中停用工作集合內的所有工作，但是它不會變更工作集合的計費狀態 – 工作集合_仍_會計費。同樣地，空白的工作集合會被視為作用中，並將計費。

## 定價

如需定價詳細資料，請參閱[排程器定價](https://azure.microsoft.com/pricing/details/scheduler/)。

## 另請參閱
 

 [排程器是什麼？](scheduler-intro.md)
 
 [Azure 排程器概念、術語及實體階層](scheduler-concepts-terms.md)

 [在 Azure 入口網站中開始使用排程器](scheduler-get-started-portal.md)

 [Azure 排程器 REST API 參考](https://msdn.microsoft.com/library/mt629143)

 [Azure 排程器 PowerShell Cmdlet 參考](scheduler-powershell-reference.md)

 [Azure 排程器高可用性和可靠性](scheduler-high-availability-reliability.md)

 [Azure 排程器限制、預設值和錯誤碼](scheduler-limits-defaults-errors.md)

 [Azure 排程器輸出驗證](scheduler-outbound-authentication.md)
 
  

  

<!---HONumber=AcomDC_0128_2016-->