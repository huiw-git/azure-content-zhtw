<properties
    pageTitle="使用 .NET SDK 建立 Azure 搜尋服務索引 | Microsoft Azure | 雲端託管搜尋服務"
    description="使用 Azure 搜尋服務 .NET SDK 在程式碼中建立索引。"
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager=""
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="03/09/2016"
    ms.author="brjohnst"/>

# 使用 .NET SDK 建立 Azure 搜尋服務索引
> [AZURE.SELECTOR]
- [概觀](search-what-is-an-index.md)
- [入口網站](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


本文將逐步引導您完成使用 [Azure 搜尋服務 .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) 建立 Azure 搜尋服務[索引](https://msdn.microsoft.com/library/azure/dn798941.aspx)的程序。

在按照本指南進行並建立索引錢，請先[建立好 Azure 搜尋服務](search-create-service-portal.md)。

請注意，本文中的所有範例程式碼都是以 C# 撰寫的。您可以在 [GitHub](http://aka.ms/search-dotnet-howto) 上找到完整的原始程式碼。

## I.識別 Azure 搜尋服務的系統管理 API 金鑰
既然您已佈建好 Azure 搜尋服務，您幾乎已經準備好針對使用 .NET SDK 的服務端點發出要求。首先，必須取得一個針對您佈建的搜尋服務所產生的管理員 API 金鑰。.NET SDK 將會在每個要求上將此 API 金鑰傳送給您的服務。擁有有效的金鑰就能為每個要求在傳送要求之應用程式與處理要求之服務間建立信任。

1. 若要尋找服務的 API 金鑰，您必須登入 [Azure 入口網站](https://portal.azure.com/)。
2. 前往 Azure 搜尋服務的刀鋒視窗。
3. 按一下 [金鑰] 圖示。

服務會有*系統管理金鑰*和*查詢金鑰*。

  - 主要和次要*系統管理金鑰*會授與所有作業的完整權限，包括管理服務以及建立和刪除索引、索引子與資料來源的能力。由於有兩個金鑰，因此如果您決定重新產生主要金鑰，您可以繼續使用次要金鑰，反之亦然。
  - *查詢金鑰*會授與索引和文件的唯讀存取權，且通常會分派給發出搜尋要求的用戶端應用程式。

主要或次要系統管理金鑰都可用於建立索引。

<a name="CreateSearchServiceClient"></a>
## II.建立 SearchServiceClient 類別的執行個體
若要開始使用 Azure 搜尋服務 .NET SDK，您必須建立 `SearchServiceClient` 類別的執行個體。這個類別有數個建構函式。您所需的建構函式會取得您的搜尋服務名稱和 `SearchCredentials` 物件做為參數。`SearchCredentials` 會包裝您的 API 金鑰。

下列程式碼會使用搜尋服務名稱的值，以及儲存於應用程式組態檔中的 API 金鑰 (`app.config` 或 `web.config`)，來建立新的 `SearchServiceClient`：

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient` 具有 `Indexes` 屬性。這個屬性會提供您需要用來建立、列出、更新或刪除 Azure 搜尋服務索引的所有方法。

> [AZURE.NOTE] `SearchServiceClient` 類別會管理連至搜尋服務的連線。為了避免開啟太多連線，如果可以，您應該嘗試在應用程式中共用 `SearchServiceClient` 的單一執行個體。它的方法是可啟用這類共用的安全執行緒。

<a name="DefineIndex"></a>
## III.使用 `Index` 類別定義 Azure 搜尋服務索引
對 `Indexes.Create` 方法的單一呼叫會建立您的索引。這個方法會將 `Index` 物件做為參數，來定義您的 Azure 搜尋服務索引。您必須建立 `Index` 物件並將它初始化，如下所示 ︰

1. 將 `Index` 物件的 `Name` 屬性設定為索引的名稱。
2. 將 `Index` 物件的 `Fields` 屬性設定為 `Field` 物件的陣列。每個 `Field` 物件都會定義索引中的欄位行為。您可以為建構函式提供欄位的名稱，以及資料類型 (或字串欄位的分析器)。您也可以設定其他屬性，例如 `IsSearchable`、`IsFilterable` 等屬性。

由於必須為每個 `Field` 指派[適當屬性](https://msdn.microsoft.com/library/azure/dn798941.aspx)，因此在設計索引時，請務必牢記搜尋服務使用者體驗和商務需求。這些屬性可控制要對哪些欄位套用哪些搜尋功能 (篩選、面向設定、排序全文檢索搜尋等)。針對您未明確設定的任何屬性，除非您特別啟用，否則 `Field` 類別預設會停用對應的搜尋功能。

在我們的範例中，我們已將索引命名為 "hotels"，並將欄位定義如下：

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[] 
    { 
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

我們已根據每個 `Field` 在應用程式中的可能使用方式仔細選擇其各自的屬性值。例如，搜尋旅館的人很可能會對 `description` 欄位上的關鍵字相符項目感興趣，因此我們藉由將 `IsSearchable` 設為`true` 來啟用該欄位的全文檢索搜尋。

請注意，`DataType.String` 類型的索引中必須有一個欄位 (且只有一個) 指定為 _key_ 欄位，方法是將 `IsKey` 設為 `true` (請參閱上述範例中的 `hotelId`)。

`description_fr` 欄位會用來儲存法文文字，因此上述索引定義會對此欄位使用自訂語言分析器。如需語言分析器的詳細資訊，請參閱 [MSDN 上的語言支援主題](https://msdn.microsoft.com/library/azure/dn879793.aspx)以及對應的[部落格文章](https://azure.microsoft.com/blog/language-support-in-azure-search/)。

> [AZURE.NOTE]  請注意，在建構函式中傳遞 `AnalyzerName.FrLucene`，`Field` 的類型將會自動成為 `DataType.String`，而且必須將 `IsSearchable` 設為 `true`。

## IV.建立索引
您現在已初始化 `Index` 物件，您只需在 `SearchServiceClient` 物件上呼叫 `Indexes.Create`，就能建立索引：

```csharp
serviceClient.Indexes.Create(definition);
```

若要求成功，方法將會正常傳回。如果要求發生問題 (例如無效的參數)，方法即會擲回 `CloudException`。

索引已使用完畢而想要將其刪除時，只需在 `SearchServiceClient` 上呼叫 `Indexes.Delete` 方法。例如，以下是我們刪除 "hotels" 索引的方式：

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] 為簡單起見，本文的範例程式碼使用 Azure 搜尋服務 .NET SDK 的同步方法。我們建議您在應用程式中使用非同步方法，讓應用程式保有可擴充性且回應靈敏。例如，您可以在上述範例中使用 `CreateAsync` 和 `DeleteAsync`，而非 `Create` 和 `Delete`。

## 下一步
建立好 Azure 搜尋服務索引後，您就可以上傳內容到索引以便開始搜尋資料。如需詳細資訊，請參閱[使用 .NET SDK 將資料匯入至 Azure 搜尋服務](search-import-data-dotnet.md)。

<!---HONumber=AcomDC_0309_2016-->