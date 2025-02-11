<properties
	pageTitle="HDInsight 的 Hadoop 叢集版本的新功能 | Microsoft Azure"
	description="HDInsight 支援多個可部署的 Hadoop 叢集版本。請參閱支援的 Hadoop 和 HortonWorks Data Platform (HDP) 配送版本。"
	services="hdinsight"
	editor="cgronlun"
	manager="paulettm"
	authors="mumian"
	tags="azure-portal"
	documentationCenter=""/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="02/29/2016"
	ms.author="jgao"/>


#HDInsight 所提供 Hadoop 叢集版本的新功能

##HDInsight 版本和 Hadoop 元件
Azure HDInsight 支援多個可隨時部署的 Hadoop 叢集版本。每一個版本選擇都會建立特定版本的 Hortonworks Data Platform (HDP) 散發，以及該散發內包含的一組元件。下表列舉了與 HDInsight 叢集版本相關聯的元件版本。請注意，Azure HDInsight 目前所使用的預設叢集版本為 3.2 版，採用 HDP 2.2 (截止 2015/12/03 為止)。


元件|HDInsight 3.3 版 | HDInsight 3.2 版 (預設)|HDInsight 3.1 版 |HDInsight 3.0 版|
---|---|---|---|---
Hortonworks Data Platform|2\.3|2\.2|2\.1.7|2\.0|
Apache Hadoop & YARN|2\.7.1|2\.6.0|2\.4.0|2\.2.0|
Apache Tez|0\.7.0 | 0\.5.2|0\.4.0||
Apache Pig|0\.15.0|0\.14.0|0\.12.1|0\.12.0|
Apache Hive & HCatalog|1\.2.1|0\.14.0|0\.13.1|0\.12.0|
Apache HBase (英文) |1\.1.1|0\.98.4|0\.98.0||
Apache Sqoop|1\.4.6|1\.4.5|1\.4.4|1\.4.4|1\.4.3
Apache Oozie|4\.2.0|4\.1.0|4\.0.0|4\.0.0|
Apache Zookeeper|3\.4.6|3\.4.6|3\.4.5|3\.4.5|
Apache Storm|0\.10.0|0\.9.3|0\.9.1||
Apache Mahout|0\.9.0+|0\.9.0|0\.9.0||
Apache Phoenix|4\.4.0|4\.2.0|4\.0.0.2.1.7.0-2162||
Apache Spark|1\.5.2 (僅限 Linux/實驗性組建)|1\.3.1 (僅限 Windows)|||


**取得目前的元件版本資訊**

在 HDInsight 的未來更新中，可能會變更 HDInsight 叢集版本相關的元件版本。若要判斷可用的元件和驗證叢集所使用的版本，有一個辦法就是使用 Ambari REST API。**GetComponentInformation** 命令可用來擷取服務元件的相關資訊。如需詳細資訊，請參閱＜[Ambari 文件][ambari-docs]＞(英文)。另一種取得此資訊的作法就是使用遠端桌面登入叢集，然後直接檢查 "C:\\apps\\dist" 目錄的內容。


**版本資訊**

請參閱 [HDInsight 版本資訊](hdinsight-release-notes.md)，以取得 HDInsight 最新版本的其他版本資訊。

### 建立 HDInsight 叢集時選取版本

在透過 HDInsight Windows PowerShell Cmdlet 或 HDInsight .NET SDK 建立叢集時，您可以在 Azure 入口網站中使用 [選擇性組態] 刀鋒視窗上的 [HDInsight 版本] 下拉式清單，選擇 HDInsight Hadoop 的叢集版本。

##功能要點
一些 HDInsight 平台的突出功能包括：

- **Spark** - Apache Spark 是一個開放原始碼平行處理架構，可支援記憶體內部處理，大幅提升巨量資料分析應用程式的效能。Spark 的記憶體內計算功能，使其成為機器學習和圖表計算中反覆演算法的絕佳選擇 。

	Spark 也可用來執行傳統的磁碟型資料處理。Spark 以避免在中繼階段寫入磁碟的方式，改善傳統的 MapReduce 架構。此外，Spark 與 Hadoop 分散式檔案系統 (HDFS) 和 Azure Blob 儲存體相容，因此可以輕鬆地透過 Spark 來處理現有的資料。

	也可以使用指令碼動作新增 Spark。指令碼動作會將 Spark 1.2.0 新增至 HDInsight 3.2 叢集或將 Spark 1.0.2 新增至 HDInsight 3.1 叢集。如需詳細資訊，請參閱[在 HDInsight Hadoop 叢集上安裝及使用 Spark](hdinsight-hadoop-spark-install.md)。


- **Storm** - Azure HDInsight 上的 Storm 現在可供使用，只要一些點擊動作，就可以在幾分鐘內快速、輕易地部署即時分析。Azure HDInsight 上的 Apache Storm 是 Apache Hadoop 生態系統中的開放原始碼專案，提供能夠可靠地處理數百萬事件之分析平台的存取權。現在 Hadoop 使用者可以在事件發生時獲得深入見解，也能夠獲得過去發生之事件的深入見解。Microsoft 也提供與 Visual Studio 的內建整合，讓開發人員輕易地與 Storm 互動。您現在可以從 Visual Studio 開發、部署及偵錯 Storm 拓撲。

- **Linux 上的 HDInsight** - Azure HDInsight 提供建立 Hadoop 叢集的選項，該叢集在 Linux (Ubuntu) 虛擬機器 (VM) 上執行。如果您熟悉 Linux 或 Unix、要從現有的以 Linux 為基礎的 Hadoop 方案進行移轉，或想輕鬆整合針對 Linux 所建置的 Hadoop 生態系統元件，則可以使用此選項。您可以使用 Azure 入口網站、Azure CLI 或 HDInsight .NET SDK (僅限 Windows)，從執行 Windows 或 Linux 的用戶端電腦，在 Linux 上建立 HDInsight 叢集。

- **其他 VM 大小** - HDInsight 叢集現在有更多 VM 類型和大小可用。HDInsight 叢集現在可以利用針對一般用途建置的 A2 到 A7 大小；特色為固態硬碟 (SSD) 和處理器速度更快 60% 的 D 系列節點；以及具有 InfiniBand 支援可快速進行網路連線的 A8 和 A9 大小。Azure HDInsight 上的 Apache HBase 客戶可以從 D 系列更大的記憶體組態獲得效能增加的優點。Azure HDInsight 上的 Apache Storm 客戶可以獲得額外記憶體的優點，用來載入更大的參考資料集，以及更快的 CPU 可用於更高的輸送量。

- **叢集調整** - 叢集調整可讓您變更執行中 HDInsight 叢集的節點數目，而不必刪除或重建它。目前只有 Hadoop 查詢和 Apache Storm 有此功能，但對 Apache HBase 很快就會提供。

- **指令碼動作** - 此叢集自訂功能的預覽可讓您使用自訂指令碼以任意方式修改 Hadoop 叢集。利用此新功能，使用者可實驗從 Apache Hadoop 生態系統取得的專案，以及將專案部署到 Azure HDInsight 叢集。此自訂功能可在所有類型的 HDInsight 叢集上使用，包括 Hadoop、HBase 和 Storm。

- **HBase** - HBase 是一種低延遲的 NoSQL 資料庫，可用於巨量資料的線上交易式處理。HBase 會以受管理叢集的形式提供，並整合到 Azure 環境中。叢集設定成直接將資料儲存於 Azure Blob 儲存體中，此儲存體可降低延遲以及提高效能/成本的選擇彈性。這可讓客戶建置使用大型資料集的互動式網站、建置可儲存數百萬端點上之感應器和遙測資料的服務，以及使用 Hadoop 工作分析此資料。

- **Apache Phoenix** - Apache Phoenix 是對 HBase 的結構化查詢語言 (SQL) 查詢層。它支援有限的 SQL 查詢語言規格子集，包括次要索引支援。它是以用戶端內嵌的 Java 資料庫連接 (JDBC) 驅動程式形式傳遞，以對 HBase 資料的低延遲查詢為目標。Apache Phoenix 會取得您的 SQL 查詢、加以編譯成一系列的 HBase 掃描和副處理器呼叫，並產生一般的 JDBC 結果集。Apache Phoenix 是對 HBase 的關聯式資料庫層。它是以用戶端內嵌的 JDBC 驅動程式形式傳遞，以對 HBase 資料的低延遲查詢為目標。Apache Phoenix 會取得您的 SQL 查詢、加以編譯成一系列的 HBase 掃描，並協調這些掃描的執行來產生一般 JDBC 結果集。

- **叢集儀表板** - 部署到 HDInsight 叢集的新 Web 應用程式。您可透過此功能來執行 Hive 查詢、檢查工作記錄，以及瀏覽 Azure Blob 儲存體。用來存取 Web 應用程式的 URL 為 <*ClusterName*>.azurehdinsight.net。

- **Microsoft Avro Library** - 此程式庫可在 Microsoft.NET 環境中實作 Apache Avro 資料序列化系統。Apache Avro 提供一個可用於序列化的壓縮二進位資料交換格式。它會使用 JavaScript 物件標記法 (JSON) 來定義不限語言的結構描述，以負責提供語言互通性。以某個語言序列化的資料可以使用另一個語言讀取。目前支援 C、C++、C#、Java、PHP、Python 和 Ruby。Azure HDInsight 廣泛採用 Apache Avro 序列化格式，以呈現 Hadoop MapReduce 工作中的複雜資料結構。

- **YARN** - 這是一套新的、通用、分散式的應用程式管理架構，取代標準的 Apache Hadoop MapReduce 架構來處理 Hadoop 叢集的資料。實際上是當做 Hadoop 作業系統，可從批次處理的單一用途資料平台，將 Hadoop 移轉至多重用途平台上，以進行批次、互動式、線上和串流處理。這個新的管理架構可根據容量保證、公平性和服務等級協定 (SLA) 等準則，提高延展性和叢集使用率。

- **Tez (僅限 HDInsight 3.1 和更新版本)** - 這是一套通用且可自訂的架構，可在 Hadoop 的大小規模工作負載中建立簡化的資料處理工作。它可讓您執行單一工作的複雜定向非循環圖 (DAG) 工作，以便 Apache Hadoop 生態系統 (例如 Apache Hive 和 Apache Pig) 中的專案可滿足人類互動回應時間和極高輸送量 (以 PB 為單位) 的需求。請注意，Hive 0.13 允許在 Tez (而不是 MapReduce) 上執行 Hive 查詢。

- **高可用性 (HA)** - 已在 HDInsight 所部署的 Hadoop 叢集中新增第二個前端節點，以提升服務的可用性。Hadoop 叢集的標準實作通常包含單一前端節點。HDInsight 會透過新增次要前端節點，來將這個單一失敗點移除。除非客戶使用超大型前端節點 (而不是預設的大型節點) 來建立叢集，否則新的 HA 叢集組態開關並不會變更叢集價格。

- **Hive 效能** - 採用**最佳化資料列單欄式 (Optimized Row Columnar, ORC)** 格式，大幅改善 Hive 查詢回應時間 (最快達到 40x) 和資料壓縮 (最高達到 80%)。

- **Pig、Sqoop、Oozie、Ambari** - HDinsight 叢集 3.0 版 (HDP 2.0/Hadoop 2.2) 的元件版本升級，與 HDinsight 叢集 2.1 版 (HDP 1.3/Hadoop 1.2) 的地位同等。詳情請參閱下列版本表格。

- **Mahout** - HDInsight 3.1 (和更新版本) Hadoop 叢集會預先安裝這個可調整的機器學習演算法程式庫。以便您在無需任何其他叢集組態需求的情況下執行 Mahout 工作。

- **虛擬網路支援** - HDInsight 叢集可以與 Azure 虛擬網路一起使用來支援隔離雲端資源，或支援將這些雲端資源與資料中心裡的那些雲端資源連結的混合式案例。


## 支援的版本
下表列出目前可用的 HDInsight 版本、它們使用的相對應 Hortonworks Data Platform 版本及其發行日期。另外也會提供其支援到期日和淘汰日期 (已知道的話)。請注意：

* 依預設，系統會為 HDInsight 2.1 和更新版本部署搭配兩個前端節點的高可用性叢集。HDInsight 1.6 叢集並不適用。
* 在特定版本的支援到期後，您可能無法透過 Azure 入口網站取得。下表指出可在 Azure 傳統入口網站上取得的版本。您可透過 Windows PowerShell [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) 命令中的 `Version` 參數和 .NET SDK 持續取得叢集版本，直到其淘汰日期為止。

HDInsight 版本|HDP 版本|高可用性|發行日期|可在 Azure 入口網站上取得|支援到期日|淘汰日期
---|---|---|---|---|---|---
HDI 3.3|HDP 2.3|是|12/02/2015|是||
HDI 3.2|HDP 2.2|是|2015/2/18|是||
HDI 3.1|HDP 2.1|是|2014/6/24|是||
HDI 3.0|HDP 2.0|是|02/11/2014|是|09/17/2014|06/30/2015
HDI 2.1|HDP 1.3|是|10/28/2013|是|05/12/2014|05/31/2015
HDI 1.6|HDP 1.1|否|10/28/2013|是|04/26/2014|05/31/2015

**非預設叢集的部署**

### HDInsight 叢集版本的服務等級協定

SLA 是根據「支援期間」來定義。「支援期間」是指 Microsoft 客戶服務與支援中心支援 HDInsight 叢集版本的一段時間。如果 HDInsight 叢集版本的 [支援到期日] 超過目前日期，則表示該叢集不在「支援期間」。上表中可找到支援的 HDInsight 叢集版本清單。特定 HDInsight X (在較新的 X+1 版本推出後) 版本的支援到期日計算方式會以下列較晚的時間為準：

- 公式 1：將 HDInsight 叢集 X 版發行日期加上 180 天。
- 公式 2：將 HDInsight 叢集 X+1 版日期加上 90 天 (X 之後的後續版本) 可用於入口網站。

[**淘汰日期**] 是指在此日期之後便無法在 HDInsight 上建立叢集版本。

> [AZURE.NOTE] HDInsight 2.1 和 3.0 叢集可在使用 Windows Server 2012 R2 64 位元版本並支援 .NET Framework 4.0、4.5 和 4.5.1 的 Azure 客體 OS [Family 4](../cloud-services/cloud-services-guestos-update-matrix.md) 上執行。

## 與 HDInsight 版本相關聯的 Hortonworks 版本資訊##

* HDInsight 叢集 3.3 版採用以 [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) 為基礎的 Hadoop 散發。
	* Apache Storm 版本資訊可從[這裡](https://storm.apache.org/2015/11/05/storm0100-released.html)取得。
	* Apache Hive 版本資訊可從[這裡](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843)取得。

* HDInsight 叢集 3.2 版採用以 [Hortonworks Data Platform 2.2][hdp-2-2] 為基礎的 Hadoop 散發。這是使用入口網站時所建立的**預設** Hadoop 叢集。

	* 特定 Apache 元件的版本資訊 - [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450)、[Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954)、[HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810)、[Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581)、[M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180)、[HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181)、[YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197)、[Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179)、[Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742)、[Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486)、[Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112)、[Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620)。


* HDInsight 叢集 3.1 版採用以 [Hortonworks Data Platform 2.1.7][hdp-2-1-7] 為基礎的 Hadoop 散發。在 2014/11/7 之前建立的 HDInsight 3.1 叢集是以 [Hortonworks Data Platform 2.1.1][hdp-2-1-1] 為基礎。

* HDInsight 叢集 3.0 版採用以 [Hortonworks Data Platform 2.0][hdp-2-0-8] 為基礎的 Hadoop 散發。

* HDInsight 叢集 2.1 版採用以 [Hortonworks Data Platform 1.3][hdp-1-3-0] 為基礎的 Hadoop 散發。

* HDInsight 叢集 1.6 版採用以 [Hortonworks Data Platform 1.1][hdp-1-1-0] 為基礎的 Hadoop 散發。


[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/

<!---HONumber=AcomDC_0302_2016-->