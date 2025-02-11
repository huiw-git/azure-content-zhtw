<properties 
   pageTitle="設定流量管理員容錯移轉流量路由方法 |Microsoft Azure"
   description="本文將協助您在流量管理員中設定容錯移轉流量路由方法"
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/01/2015"
   ms.author="joaoma" />

# 設定容錯移轉路由方法

組織通常會想要為其服務提供可靠性。這樣做的方法是透過提供備份服務，以避免發生其主要服務當機的情況。服務容錯移轉的一個常見模式是提供一組完全相同的服務，且將流量傳送到主要服務，並同時維護一或多個備份服務的已設定清單。您可以依照下列程序，在 Azure 雲端服務和網站中設定此種類型的備份。

請注意，不論網站模式為何，Azure 網站已為資料中心 (也稱為「區域」) 內的網站提供容錯移轉流量路由方法功能。流量管理員可讓您在不同的資料中心網站中指定容錯移轉流量路由方法。

## 設定容錯移轉流量路由方法：

1. 在 Azure 入口網站的左側窗格中，按一下 [流量管理員] 圖示以開啟 [流量管理員] 窗格。如果您尚未建立流量管理員設定檔，請參閱[管理流量管理員設定檔](traffic-manager-manage-profiles.md)的步驟來建立基本的流量管理員設定檔。
2. 在 Azure 入口網站的 [流量管理員] 窗格中，找出包含您要修改的流量管理員設定檔設定，然後按一下設定檔名稱右側的箭號。這會開啟設定檔的設定頁面。
3. 在您的設定檔頁面上，按一下頁面頂端的 [端點]，並確認您要納入組態的雲端服務和網站 (端點) 已存在。如需加入或移除端點的步驟，請參閱[在流量管理員中管理端點](traffic-manager-endpoints.md)。
4. 在您的設定檔頁面上，按一下頂端的 [設定] 以開啟 [組態] 頁面。
5. 在 [**流量路由方法設定**] 中，確認流量路由方法為 [**容錯移轉**]。如果不是，請按一下下拉式清單中的 [容錯移轉]。
6. 在 [容錯移轉優先順序清單] 中，調整您端點的容錯移轉順序。當您選取 [**容錯移轉**] 流量路由方法時，選取端點的順序便很重要。主要端點會位於最上方。視需要使用向上和向下箭頭來變更順序。如需如何使用 Windows PowerShell 設定容錯移轉優先權的相關資訊，請參閱 [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880)。
7. 確認已正確地設定 [監視設定]。監視可確保處於離線狀態的端點不會傳送流量。為了要能夠監視端點，您必須指定路徑和檔案名稱。請注意，正斜線 “/“ 是相對路徑的有效項目，表示檔案位於根目錄 (預設值)。如需監視的詳細資訊，請參閱[流量管理員監視](traffic-manager-monitoring.md)。
8. 完成組態變更之後，請按一下頁面底部的 [儲存]。
9. 測試組態中的變更。如需詳細資訊，請參閱[測試流量管理員設定](traffic-manager-testing-settings.md)。
10. 在已設定並執行流量管理員設定檔之後，您可以編輯授權 DNS 伺服器上的 DNS 記錄，以將您的公司網域名稱指向流量管理員網域名稱。如需有關如何執行這項操作的詳細資訊，請參閱[將公司網際網路網域指向流量管理員網域](traffic-manager-point-internet-domain.md)。

## 後續步驟

[關於流量管理員流量路由方法](traffic-manager-load-balancing-methods.md)

[什麼是流量管理員？](traffic-manager-overview.md)

[流量管理員 - 停用、啟用或刪除設定檔](disable-enable-or-delete-a-profile.md)

[流量管理員 - 停用或啟用端點](disable-or-enable-an-endpoint.md)

[雲端服務](http://go.microsoft.com/fwlink/?LinkId=314074)

[網站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[流量管理員的相關作業 (REST API 參考)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure 流量管理員 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)
 

<!---HONumber=AcomDC_0128_2016-->