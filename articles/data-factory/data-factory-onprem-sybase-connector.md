<properties 
	pageTitle="從 Sybase 移動資料 | Azure Data Factory" 
	description="了解如何使用 Azure Data Factory 從 Sybase 資料庫移動資料。" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/01/2016" 
	ms.author="spelluru"/>

# 使用 Azure Data Factory 從 Sybase 移動資料 

本文將概述如何使用 Azure 資料處理站中的複製活動將資料從 Sybase 移動到另一個資料存放區。本文是根據[資料移動活動](data-factory-data-movement-activities.md)一文，該文呈現使用複製活動移動資料的一般概觀以及支援的資料存放區組合。

Data Factory 服務支援使用資料管理閘道器連接至內部部署 Sybase 來源。請參閱[在內部部署位置與雲端之間移動資料](data-factory-move-data-between-onprem-and-cloud.md)一文來了解資料管理閘道器和設定閘道器的逐步指示。

**請注意：**即使 Sybase 裝載於 Azure IaaS VM 中，您還是需要運用閘道器與其連接。如果您正嘗試連接到裝載於雲端中的 Sybase 執行個體，您也可以在 IaaS VM 中安裝閘道器執行個體。

資料處理站目前只支援將資料從 Sybase 移至其他資料存放區，而不支援從其他資料存放區移至 Sybase。

## 安裝

若要讓資料管理閘道器連接至 Sybase 資料庫，您必須在與資料管理閘道器相同的系統上安裝 [Sybase 的資料提供者](http://go.microsoft.com/fwlink/?linkid=324846)。

> [AZURE.NOTE] 如需連接/閘道器相關問題的疑難排解秘訣，請參閱[閘道器疑難排解](data-factory-move-data-between-onprem-and-cloud.md#gateway-troubleshooting)。

## 範例：從 Sybase 複製資料到 Azure Blob
此範例示範如何將資料從 Sybase 資料庫複製到 Azure Blob 儲存體。不過，您可以在 Azure Data Factory 中使用複製活動，**直接**將資料複製到[這裡](data-factory-data-movement-activities.md#supported-data-stores)所說的任何接收器。
 
此範例具有下列 Data Factory 實體：

1.	[OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties) 類型的連結服務。
2.	[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) 類型的連結服務。
3.	[RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties) 類型的輸入[資料集](data-factory-create-datasets.md)。
4.	[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 類型的輸出[資料集](data-factory-create-datasets.md)。
4.	具有使用 [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) 和 [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) 之複製活動的[管線](data-factory-create-pipelines.md)。

此範例會每個小時將資料從 Sybase 資料庫中的查詢結果複製到 Blob。範例後面的各節會說明這些範例中使用的 JSON 屬性。

在第一個步驟中，請根據[在內部部署位置與雲端之間移動資料](data-factory-move-data-between-onprem-and-cloud.md)一文中的指示，設定資料管理閘道器。

**Sybase 連結服務：**

	{
	    "name": "OnPremSybaseLinkedService",
	    "properties": {
	        "type": "OnPremisesSybase",
	        "typeProperties": {
	            "server": "<server>",
	            "database": "<database>",
	            "schema": "<schema>",
	            "authenticationType": "<authentication type>",
	            "username": "<username>",
	            "password": "<password>",
	            "gatewayName": "<gatewayName>"
	        }
	    }
	}

**Azure Blob 儲存體連結服務：**

	{
	    "name": "AzureStorageLinkedService",
	    "properties": {
	        "type": "AzureStorageLinkedService",
			"typeProperties": {
	        	"connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
			}
	    }
	}


**Sybase 輸入資料集：**

此範例假設您已在 Sybase 中建立資料表 "MyTable"，其中包含時間序列資料的資料行 (名稱為 "timestamp")。

設定 “external”: true 和指定 externalData 原則即可通知 Azure Data Factory：這是 Data Factory 外部的資料表而且不是由 Data Factory 中的活動所產生。請注意，連結服務的**類型**需設為：**RelationalTable**。
	
	{
	    "name": "SybaseDataSet",
	    "properties": {
	        "type": "RelationalTable",
	        "linkedServiceName": "OnPremSybaseLinkedService",
	        "typeProperties": {},
	        "availability": {
	            "frequency": "Hour",
	            "interval": 1
	        },
			"external": true,
	        "policy": {
	            "externalData": {
	                "retryInterval": "00:01:00",
	                "retryTimeout": "00:10:00",
	                "maximumRetry": 3
	            }
	        }
	    }
	}


**Azure Blob 輸出資料集：**

資料會每小時寫入至新的 Blob (頻率：小時，間隔：1)。根據正在處理之配量的開始時間，以動態方式評估 Blob 的資料夾路徑。資料夾路徑會使用開始時間的年、月、日和小時部分。

	{
	    "name": "AzureBlobSybaseDataSet",
	    "properties": {
	        "type": "AzureBlob",
	        "linkedServiceName": "AzureStorageLinkedService",
	        "typeProperties": {
	            "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
	            "format": {
	                "type": "TextFormat",
	                "rowDelimiter": "\n",
	                "columnDelimiter": "\t"
	            },
	            "partitionedBy": [
	                {
	                    "name": "Year",
	                    "value": {
	                        "type": "DateTime",
	                        "date": "SliceStart",
	                        "format": "yyyy"
	                    }
	                },
	                {
	                    "name": "Month",
	                    "value": {
	                        "type": "DateTime",
	                        "date": "SliceStart",
	                        "format": "%M"
	                    }
	                },
	                {
	                    "name": "Day",
	                    "value": {
	                        "type": "DateTime",
	                        "date": "SliceStart",
	                        "format": "%d"
	                    }
	                },
	                {
	                    "name": "Hour",
	                    "value": {
	                        "type": "DateTime",
	                        "date": "SliceStart",
	                        "format": "%H"
	                    }
	                }
	            ]
	        },
	        "availability": {
	            "frequency": "Hour",
	            "interval": 1
	        }
	    }
	}


**具有複製活動的管線：**

此管線包含複製活動，該活動已設定為使用上述輸入和輸出資料集並排定為每小時執行。在管線 JSON 定義中，**source** 類型設為 **RelationalSource**，而 **sink** 類型設為 **BlobSink**。針對 **query** 屬性指定的 SQL 查詢會從資料庫中的 DBA.Orders 資料表選取資料。


	{
	    "name": "CopySybaseToBlob",
	    "properties": {
	        "description": "pipeline for copy activity",
	        "activities": [
	            {
	                "type": "Copy",
	                "typeProperties": {
	                    "source": {
	                        "type": "RelationalSource",
	                        "query": "select * from DBA.Orders"
	                    },
	                    "sink": {
	                        "type": "BlobSink"
	                    }
	                },
	                "inputs": [
	                    {
	                        "name": "SybaseDataSet"
	                    }
	                ],
	                "outputs": [
	                    {
	                        "name": "AzureBlobSybaseDataSet"
	                    }
	                ],
	                "policy": {
	                    "timeout": "01:00:00",
	                    "concurrency": 1
	                },
	                "scheduler": {
	                    "frequency": "Hour",
	                    "interval": 1
	                },
	                "name": "SybaseToBlob"
	            }
	        ],
	        "start": "2014-06-01T18:00:00Z",
	        "end": "2014-06-01T19:00:00Z"
	    }
	}


## Sybase 連結服務屬性

下表提供 Sybase 連結服務專屬 JSON 元素的描述。

屬性 | 說明 | 必要
-------- | ----------- | --------
類型 | 類型屬性必須設為：**OnPremisesSybase** | 是
伺服器 | Sybase 伺服器的名稱。 | 是
資料庫 | Sybase 資料庫的名稱。 | 是 
結構描述 | 在資料庫中的結構描述名稱。 | 否
authenticationType | 用來連接到 Sybase 資料庫的驗證類型。可能的值為：匿名、基本和 Windows。 | 是
username | 如果您使用基本或 Windows 驗證，請指定使用者名稱。 | 否
password | 指定您為使用者名稱所指定之使用者帳戶的密碼。 | 否
gatewayName | Data Factory 服務應該用來連接到內部部署 Sybase 資料庫的閘道器名稱。 | 是 

如需關於設定內部部署 Sybase 資料來源認證的詳細資料，請參閱[設定認證和安全性](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security)。

## Sybase 資料集類型屬性

如需定義資料集的區段和屬性完整清單，請參閱[建立資料集](data-factory-create-datasets.md)一文。資料集 JSON 的結構、可用性和原則等區段類似於所有的資料集類型 (SQL Azure、Azure Blob、Azure 資料表等)。

每個資料集類型的 typeProperties 區段都不同，可提供資料存放區中資料的位置相關資訊。**RelationalTable** 資料集類型的 **typeProperties** 區段 (包含 Sybase 資料集) 具有下列屬性。

屬性 | 說明 | 必要
-------- | ----------- | --------
tableName | Sybase 資料庫執行個體中連結服務所參照的資料表名稱。 | 否 (如果已指定 **RelationalSource** 的 **query**)

## Sybase 複製活動類型屬性 

如需定義活動的區段和屬性完整清單，請參閱[建立管線](data-factory-create-pipelines.md)一文。名稱、描述、輸入和輸出資料表、各種原則等屬性都適用於所有活動類型。

另一方面，活動的 typeProperties 區段中可用的屬性會隨著每個活動類型而有所不同，而在複製活動的案例中，可用的屬性會根據來源與接收的類型而有所不同。

在複製活動的案例中，如果來源類型為 **RelationalSource** (包含 Sybase)，則 **typeProperties** 區段可使用下列屬性：

屬性 | 說明 | 允許的值 | 必要
-------- | ----------- | -------------- | --------
query | 使用自訂查詢來讀取資料。 | SQL 查詢字串。例如：select * from MyTable。 | 否 (如果已指定 **dataset** 的 **tableName**)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## Sybase 的類型對應

如資料移動活動一文所述，複製活動會使用下列 2 個步驟的方法，執行從來源類型轉換成接收類型的自動類型轉換：

1. 從原生來源類型轉換成 .NET 類型
2. 從 .NET 類型轉換成原生接收類型

Sybase 支援 T-SQL 和 T-SQL 類型。如需從 sql 類型到.NET 類型的對應資料表，請參閱 [Azure SQL 連接器](data-factory-azure-sql-connector.md)一文。

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

<!---HONumber=AcomDC_0204_2016-->