<properties
	pageTitle="App Service API Apps - 變更的項目 | Microsoft Azure"
	description="了解 Azure App Service 中 API Apps 的新功能"
	services="app-service\api"
	documentationCenter=".net"
	authors="mohitsriv"
	manager="wpickett"
	editor="tdykstra"/>

<tags
	ms.service="app-service-api"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="01/13/2016"
	ms.author="mohisri"/>

# App Service API Apps - 變更的項目

在 2015 年 11 月的 Connect () 事件中，[宣告](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/)了許多 Azure App Service 的改進功能。這些改進功能包括 API Apps 的基礎變更，以進一步配合行動和 Web Apps、減少概念計數以及改善部署和執行階段效能。從 2015 年 11 月 30 日起，您使用 Azure 管理入口網站或最新的工具建立的新的 API 應用程式將會反映這些變更。本文說明這些變更，以及如何重新部署現有的應用程式，以充分利用功能。


> [AZURE.NOTE] API Apps 的初始預覽支援兩種主要案例：1) 自訂 API，以用於 Logic Apps 或您自己的用戶端和 2) Marketplace API (通常是 SaaS 連接器) 以用於 Logic Apps。本文說明第一個案例，自訂 API。對於 Marketplace API，改進的 Logic Apps 設計工具體驗和基本連線基礎將會在 2016 年初引入。現有的 Marketplace API 仍然可以在 Logic Apps 設計工具中使用。

## 功能變更
API Apps 的主要功能 – 驗證、CORS 和 API 中繼資料 – 已直接移至 App Service。透過這項變更，功能可以跨 Web、行動及 API Apps 使用。事實上，這三個項目共用資源管理員中相同的 **microsoft.web/sites** 資源類型。不再需要 API Apps 閘道或隨著 API Apps 提供。這也讓您可以更輕鬆地使用 Azure API 管理，因為只有單一 API 管理閘道。

![API Apps 概觀](./media/app-service-api-whats-changed/api-apps-overview.png)

API Apps 更新的主要設計原則是讓您可以使用您選擇的語言，直接帶入您的 API。如果您的 API 已經部署為 Web 應用程式或行動應用程式*，您不必重新部署應用程式即可充分利用新功能。

> [AZURE.NOTE] *如果您目前是在 API Apps 預覽，詳細的移轉指南如下。

### 驗證
現有的周全 API Apps 、行動服務/應用程式和 Web Apps 驗證功能已整合，且可在管理入口網站的單一 Azure App Service 驗證刀鋒視窗中使用。如需 App Service 中驗證服務的簡介，請參閱[擴充 App Service 驗證/授權](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)。

對於 API 案例，有一些相關的新功能：

- **支援直接使用 Azure Active Directory**，而不需要用戶端程式碼交換工作階段權杖的 AAD 權杖：您的用戶端可以根據持有人權杖規格，在 Authorization 標頭只包含 AAD 權杖。這也表示用戶端或伺服器端上不需要有任何 App Service 專用 SDK。 
- **服務對服務或「內部」存取**：如果您有精靈處理序或某些需要存取沒有介面之 API 的其他用戶端，可以使用 AAD 服務主體來要求權杖，並將它傳遞至 App Service 來驗證您的應用程式。
- **延遲授權**：許多應用程式對於應用程式的不同部分有各種不同的存取限制。也許您想要讓某些 API 可供公開使用，而有些則需要登入。原始的驗證/授權功能是孤注一擲，整個網站都需要登入。此選項仍然存在，但是您也可以選擇允許您的應用程式程式碼在 App Service 驗證使用者之後，呈現存取決策。
 
如需新驗證功能的詳細資訊，請參閱 [Azure App Service 中 API Apps 的驗證和授權](app-service-api-authentication.md)。如需如何將現有 API 應用程式從先前的 API 應用程式模型的轉到新模型的詳細資訊，請參閱本文章稍後的[移轉現有的 API 應用程式](#migrating-existing-api-apps)。
 
### CORS
不是以逗號分隔的 **MS_CrossDomainOrigins** 應用程式設定，現在在 Azure 管理入口網站會有一個刀鋒視窗可供設定 CORS。或者，可以使用資源管理員工具，例如 Azure PowerShell、CLI 或[資源總管](https://resources.azure.com/)來進行設定。在您的 **&lt;site name&gt;/web** 資源的 **Microsoft.Web/sites/config** 資源類型上設定 **cors** 屬性。例如：

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### API 中繼資料
API 定義刀鋒視窗可以透過 Web、行動及 API Apps 使用。在管理入口網站中，您可以指定相對 URL，或是指向主控您的 API 的 Swagger 2.0 表示端點的絕對 URL。或者，可以使用資源管理員工具進行設定。在您的 **&lt;site name&gt;/web** 資源的 **Microsoft.Web/sites/config** 資源類型上設定 **apiDefinition** 屬性。例如：

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

目前，對於許多下游用戶端，中繼資料端點必須不經過驗證即可公開存取 (例如 Visual Studio REST API 用戶端產生和 PowerApps「新增 API」流程) 來使用它。這表示如果您使用 App Service 驗證，而且想要從應用程式本身公開 API 定義，您必須使用先前所述的「延遲驗證」選項，讓您的 Swagger 中繼資料的路由是公開的。

## 管理入口網站
在入口網站中選取 [新增] > [Web + 行動] > [API 應用程式]，將會建立 API 應用程式，反映本文所述的功能。[瀏覽] > [API Apps] 只會顯示這些新的 API 應用程式。一旦您瀏覽至 API 應用程式，刀鋒視窗會共用與 Web Apps 和 Mobile Apps 的刀鋒視窗相同的配置和功能。唯一差異是快速入門內容和設定順序。

現有的 API 應用程式 (或從 Logic Apps 建立的 Marketplace API 應用程式) 與先前的預覽功能仍會在 Logic Apps 設計工具中以及於瀏覽資源群組中的所有資源時顯示。如果您需要建立具有先前的預覽功能的 API 應用程式，可以使用封裝，並且可於 Azure Marketplace 的 [Web + 行動] > [API Apps (預覽)] 中搜尋得到。

## Visual Studio

大部分的 Web Apps 工具將會使用新的 API 應用程式，因為它們會共用相同的基礎 **Microsoft.Web/sites** 資源類型。但是，Azure Visual Studio 工具應該升級為版本 2.8.1 或更新版本，因為它會公開一些特定的 API 功能。從 [Azure 下載頁面](https://azure.microsoft.com/downloads/)下載 SDK。

隨著 App Service 類型的合理化，發佈也會在 [發佈] > [Microsoft Azure App Service] 底下統一：

![API Apps 發佈](./media/app-service-api-whats-changed/api-apps-publish.png)

若要深入了解 SDK 2.8.1，請閱讀公告[部落格文章](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)。

或者，您可以從管理入口網站手動匯入發佈設定檔以啟用發佈。不過，雲端總管、程式碼產生和 API 應用程式選取/建立需要 SDK 2.8.1 或更高版本。

發佈至具有先前預覽的現有 API 應用程式的功能，在 SDK 2.8.1 中仍然可以使用。如果您已發佈專案，則不需要任何進一步的動作。若要設定發佈，從發佈對話方塊的 [更多選項] 下拉式清單中選擇 [API Apps (傳統)]。

## 移轉現有的 API 應用程式
如果您的自訂 API 部署至先前預覽版本的 API Apps，我們要求您在 2015 年 12 月 31 日之前移轉至新的 API Apps 模型。因為舊的和新的模型都是根據 App Service 中裝載的 Web API，所以大部分現有的程式碼都可以重複使用。

### 裝載和重新部署
重新部署的步驟與將任何現有的 Web API 部署到 App Service 的步驟相同。步驟：

1. 建立空的 API 應用程式。做法是在入口網站中使用 [新增] > [API 應用程式]，在 Visual Studio 中使用 [發佈]，或使用資源管理員工具。如果是使用資源管理員工具或範本，在 **Microsoft.Web/sites** 資源類型上將 [種類] 值設為 **api**，將管理入口網站中的快速入門和設定導向 API 案例。
2. 使用 App Service 支援的任何部署機制，連接及部署專案到空的 API 應用程式。如需詳細資訊，請參閱 [Azure App Service 部署文件](../app-service-web/web-sites-deploy.md)。 
  
### 驗證
App Service 驗證服務支援相同的功能，這些功能可用於先前的 API 應用程式模型。如果您使用工作階段權杖並且需要 SDK，請使用下列用戶端與伺服器 SDK：

- 用戶端：[Azure 行動用戶端 SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- 伺服器：[Microsoft Azure 行動應用程式 .NET 驗證延伸模組](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

如果您改為使用 App Service alpha SDK，這些項目目前已被取代：

- 用戶端：[Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- 伺服器：[Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

但是，特別是使用 Azure Active Directory，如果您直接使用 AAD 權杖，則不需要任何 App Service 特定項目。

### 內部存取
先前的 API Apps 模型包含內建內部存取層級。這需要使用 SDK 來簽署要求。如先前所述，使用新的 API Apps 模型，AAD 服務主體可以做為服務對服務驗證的替代，而不用要求 App Service 特定 SDK。在[在 Azure App Service 中 API Apps 的服務主體驗證](app-service-api-dotnet-service-principal-auth.md)中深入了解。

### 探索
先前的 API Apps 模型具有 API，在相同閘道後方的相同資源群組中的執行階段，探索其他 API 應用程式。這在實作微服務模式的架構中特別有用。雖然不直接支援，但是有許多選項可用：

1. 使用 Azure 資源管理員 API 進行探索。
2. 將 Azure API 管理放在您的 App Service 裝載 API 的前面。Azure API 管理做為外觀，可以提供穩定的對外 URL，即使您的內部拓撲變更。
3. 建置您自己的探索 API 應用程式，並且讓其他 API 應用程式在啟動時註冊探索應用程式。
4. 在部署階段，以其他 API 應用程式的端點填入所有 API 應用程式 (和用戶端) 的應用程式設定。這是可行的範本部署，因為 API Apps 現在提供您 URL 的控制權。

### Logic Apps
Logic Apps 設計工具將會在 2016 年初新增與新的 API Apps 模型的特別緊密整合。內建至 Logic Apps 的 HTTP 連接器可以叫用任何 HTTP 端點，並且支援服務主體驗證，這也受到 App Service 驗證服務的原生支援。在[將您裝載在 App Service 上的自訂 API 與 Logic Apps 一起使用](../app-service-logic/app-service-logic-custom-hosted-api.md)中，深入了解如何在邏輯應用程式中使用 App Service 裝載 API。

### <a id="documentation"></a> 先前的 API Apps 模型的文件
為舊版 API Apps 模型撰寫的某些 [azure.microsoft.com](https://azure.microsoft.com/) 文章不再適用新的模型，並將會從站台移除。其 URL 將會被重新導向至新模型適用、最接近的對等項目，但您仍然可以在 [azure.microsoft.com 的 GitHub 文件儲存機制](https://github.com/Azure/azure-content)中看到舊的文章。您需要的多數文章將可在 [articles/app-service-api](https://github.com/Azure/azure-content/tree/master/articles/app-service-api) 資料夾中找到。如果您要支援較舊的 API 應用程式，或如果您從 Marketplace 建立新的連接器 API 應用程式，以下是其中一些最可能有幫助的直接連結。

* [驗證概觀](https://github.com/Azure/azure-content/tree/master/articles/app-service/app-service-authentication-overview.md)
* [保護 API 應用程式](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-add-authentication.md)
* [取用內部 API 應用程式](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-consume-internal.md)
* [使用用戶端流程驗證取用](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-authentication-client-flow.md)
* [部署及設定 SaaS 連接器 API 應用程式](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-connnect-your-app-to-saas-connector.md)
* [以新的閘道佈建 API 應用程式](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-arm-new-gateway-provision.md)
* [偵錯 API 應用程式](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-debug.md)
* [連接到 SaaS 平台](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-connect-to-saas.md)
* [增強 Logic Apps 的 API 應用程式](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-optimize-for-logic-apps.md)
* [API 應用程式觸發程序](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-triggers.md)

## 後續步驟
若要深入了解，請參閱 [API Apps 文件小節](https://azure.microsoft.com/documentation/services/app-service/api/)中的文章。這些文章已更新以反映 API Apps 的新模型。此外，務必造訪論壇以取得其他詳細資料或移轉的指引：

- [MSDN 論壇](https://social.msdn.microsoft.com/Forums/zh-TW/home?forum=AzureAPIApps)
- [堆疊溢位](http://stackoverflow.com/questions/tagged/azure-api-apps)

<!---HONumber=AcomDC_0128_2016-->