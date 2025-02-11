<properties
	pageTitle="在您的邏輯應用程式中新增 FTP API | Microsoft Azure"
	description="搭配 REST API 參數來使用 FTP API 的概觀"
	services=""
	documentationCenter="" 
	authors="MandiOhlinger"
	manager="erikre"
	editor=""
	tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="02/25/2016"
   ms.author="mandia"/>

# 開始使用 FTP API
連線到 FTP 伺服器來管理您的檔案，包括上傳檔案、刪除檔案等等。您可以從下列應用程式使用 FTP API：

- 邏輯應用程式

>[AZURE.NOTE] 這一版的文章適用於邏輯應用程式 2015-08-01-preview 結構描述版本。對於 2014-12-01-preview 結構描述版本，請按一下 [FTP 連接器](../app-service-logic/app-service-logic-connector-ftp.md)。

您可以利用 FTP 來：

- 根據您從 FTP 所取得的資料，來建置您的商務流程。 
- 在檔案更新時使用觸發程序。
- 使用會產生檔案、取得檔案內容等等的動作。這些動作會收到回應，然後輸出能讓其他動作使用的資料。舉例來說，您可以取得檔案內容，然後更新 SQL 資料庫。 

如果要在邏輯應用程式中新增作業，請參閱[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)。


## 觸發程序及動作
FTP 提供下列觸發程序及動作。

觸發程序 | 動作
--- | ---
<ul><li>取得已更新的檔案</li></ul> | <ul><li>建立檔案</li><li>複製檔案</li><li>刪除檔案</li><li>解壓縮資料夾</li><li>取得檔案內容</li><li>使用路徑來取得檔案內容</li><li>取得檔案中繼資料</li><li>使用路徑來取得檔案中繼資料</li><li>取得已更新的檔案</li><li>更新檔案</li></ul>

所有 API 都支援 JSON 和 XML 格式的資料。

## 建立至 FTP 的連線
當您將這個 API 新增到邏輯應用程式時，請輸入下列的值：

|屬性| 必要|說明|
| ---|---|---|
|伺服器位址| 是 | 輸入 FTP 伺服器的完整網域 (FQDN) 或 IP 位址。|
|使用者名稱| 是 | 輸入要連線到 FTP 伺服器的使用者名稱。|
|密碼 | 是 | 輸入使用者名稱的密碼。|

當您建立連線之後，請輸入 FTP 的屬性，例如來源檔案或目的資料夾。本主題的＜REST API 參考＞一節說明這些屬性。

>[AZURE.TIP] 您可以在其他的邏輯應用程式中，使用這個相同的 FTP 連線。

## Swagger REST API 參考
適用的版本：1.0。

### 建立檔案
把檔案上傳到 FTP 伺服器。```POST: /datasets/default/files```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|folderPath|字串|yes|query|無 |用來把檔案上傳到 FTP 伺服器的資料夾路徑|
|名稱|字串|yes|query| 無|要在 FTP 伺服器中建立之檔案的名稱|
|body| |yes|body|無 |要上傳到 FTP 伺服器之檔案的內容|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|

### 複製檔案
把檔案複製到 FTP 伺服器。```POST: /datasets/default/copyFile```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|來源|字串|yes|query|無 |來源檔案的 URL|
|目的地|字串|yes|query|無 |FTP 伺服器中的目的檔案路徑，包括目標檔案名稱|
|overwrite|布林值|no|query|無 |如果設定為「True」，則會覆寫目的檔案|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|

### 刪除檔案 
刪除 FTP 伺服器中的檔案。```DELETE: /datasets/default/files/{id}```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|id|字串|yes|路徑|無 |要從 FTP 伺服器刪除之檔案的唯一識別碼|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|

### 解壓縮到資料夾
將封存檔案 (例如 .zip) 解壓縮到 FTP 伺服器中的資料夾。```POST: /datasets/default/extractFolderV2```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|來源|字串|yes|query| 無|封存檔案的路徑|
|目的地|字串|yes|query| 無|目的資料夾的路徑|
|overwrite|布林值|no|query|無|如果設定為「True」，則會覆寫目的檔案|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|

### 取得檔案內容
使用識別碼來擷取 FTP 伺服器中的檔案內容。```GET: /datasets/default/files/{id}/content```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|id|字串|yes|路徑|無 |檔案的唯一識別碼|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|


### 使用路徑來取得檔案內容
使用路徑來擷取 FTP 伺服器中的檔案內容。```GET: /datasets/default/GetFileContentByPath```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|路徑|字串|yes|query|無 |FTP 伺服器中檔案的唯一路徑|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|


### 取得檔案中繼資料 
使用檔案識別碼來擷取 FTP 伺服器中的檔案中繼資料。```GET: /datasets/default/files/{id}```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|id|字串|yes|路徑|無|檔案的唯一識別碼|

#### Response
| 名稱 | 說明 |
| --- | --- |
| 200 | OK | 
| 預設值 | 作業失敗。


### 使用路徑來取得檔案中繼資料
使用路徑來擷取 FTP 伺服器中的檔案中繼資料。```GET: /datasets/default/GetFileByPath```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|路徑|字串|yes|query| 無|FTP 伺服器中檔案的唯一路徑|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|


### 取得已更新的檔案
取得已更新的檔案。```GET: /datasets/default/triggers/onupdatedfile```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|folderId|字串|yes|query|無 |用來尋找已更新檔案的資料夾識別碼|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|


### 更新檔案 
更新 FTP 伺服器中的檔案。```PUT: /datasets/default/files/{id}```

| 名稱| 資料類型|必要|位於|預設值|說明|
| ---|---|---|---|---|---|
|id|字串|yes|路徑| 無|FTP 伺服器中要更新之檔案的唯一識別碼|
|body| |yes|body|無 |FTP 伺服器中要更新之檔案的內容|

#### Response
|名稱|說明|
|---|---|
|200|OK|
|預設值|作業失敗。|


## 物件定義

#### DataSetsMetadata

| 名稱 | 資料類型 | 必要 |
|---|---|---|
|表格式|未定義|no|
|blob|未定義|no|

#### TabularDataSetsMetadata

| 名稱 | 資料類型 | 必要 |
|---|---|---|
|來源|字串|no|
|displayName|字串|no|
|urlEncoding|字串|no|
|tableDisplayName|字串|no|
|tablePluralName|字串|no|

#### BlobDataSetsMetadata

| 名稱 | 資料類型 | 必要 |
|---|---|---|
|來源|字串|no|
|displayName|字串|no|
|urlEncoding|字串|no|

#### BlobMetadata

| 名稱 | 資料類型 | 必要 |
|---|---|---|
|識別碼|字串|no|
|名稱|字串|no|
|DisplayName|字串|no|
|Path|字串|no|
|LastModified|字串|no|
|大小|integer|no|
|MediaType|字串|no|
|IsFolder|布林值|no|
|ETag|字串|no|
|FileLocator|字串|no|

## 後續步驟

[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)。

<!---HONumber=AcomDC_0302_2016-------->