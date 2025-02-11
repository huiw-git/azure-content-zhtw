<properties
    pageTitle="彈性資料庫工具功能概觀 | Microsoft Azure"
    description="軟體即服務 (SaaS) 開發人員可以輕鬆地使用這些工具在雲端中建立彈性的可擴充資料庫"
    services="sql-database"
    documentationCenter=""
    manager="jeffreyg"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="ddove;sidneyh"/>

# 彈性資料庫功能概觀

**彈性資料庫**功能可讓您使用 **Azure SQL Database** 中幾乎不受限制的資料庫資源，建立交易式工作負載的解決方案，尤其是軟體即服務 (SaaS) 應用程式。彈性資料庫功能的組成如下：

* 彈性資料庫工具：這兩個工具簡化分區化資料庫解決方案的開發與管理。這些工具是：[彈性資料庫用戶端程式庫](sql-database-elastic-database-client-library.md)和[彈性資料庫分割合併工具](sql-database-elastic-scale-overview-split-and-merge.md)。 
* [彈性資料庫集區](sql-database-elastic-pool-guidance.md) (預覽)：集區是可讓您隨時新增或移除資料庫的資料庫集合。集區中的資料庫共用固定數量的資源 (稱為資料庫交易單位，簡稱 DTU)。您對資源支付固定費用，在管理效能時很容易計算成本。 
* [彈性資料庫工作](sql-database-elastic-jobs-overview.md) (預覽)：使用工作來管理大量的 Azure SQL 資料庫。輕鬆執行系統管理作業，例如結構描述變更、認證管理、參考資料更新、效能資料收集，或使用工作的租用戶 (客戶) 遙測收集。
* [彈性資料庫查詢](sql-database-elastic-query-overview.md) (預覽)：可讓您跨多個 Azure SQL 資料庫執行 Transact-SQL 查詢。這可讓您連接至報告工具，例如 Excel、PowerBI、Tableau 等等。

下圖顯示的架構包含與資料庫集合有關的**彈性資料庫功能**。

![彈性資料庫工具][1]

如需此圖形的可列印版本，請移至[彈性資料庫概觀下載](http://aka.ms/axmybc)。

此圖中，資料庫色彩代表結構描述。相同色彩的資料庫共用相同的結構描述。

1. 一組 **Azure SQL 資料庫**使用分區化架構裝載於 Azure 中。 
2. **彈性資料庫用戶端程式庫**用來管理分區集。
3. 一個資料庫子集放入**彈性資料庫集區**中。(請參閱[使用彈性資料庫有效駕馭爆炸性的成長](sql-database-elastic-pool.md))。 
4. **彈性資料庫工作**針對所有資料庫執行 T-SQL 指令碼。
5. **分割合併工具**用來將資料移到另一個的分區。
6. **彈性資料庫查詢**可讓您撰寫一個跨分區集所有資料庫的查詢。
  
## 承諾和挑戰

已簡單達成雲端應用程式在計算和 blob 儲存體方面的彈性和縮放 -- 增加或減少單位即可。但對於關聯式資料庫中可設定狀態的資料處理，仍然是個挑戰。我們已知這些挑戰最主要出現在下列兩個案例中：

* 針對工作負載的關聯式資料庫部分放大和縮小容量。
* 管理可能會影響特定資料子集的作用點 - 例如特別忙碌的終端客戶 (租用戶)。

傳統上，要解決這類情況，都是經由投資更大型的資料庫伺服器來支援應用程式。不過，此選項在雲端有其限制，因為所有處理都發生在預先定義的商用硬體上。相反地，將資料和處理分散至許多相同結構的資料庫 (一種稱為「分區化」的相應放大模式)，不論在成本或彈性上，都是超越傳統相應增加方法的另一種選擇。

## 水平和垂直縮放

下圖顯示縮放的水平和垂直面向，也是彈性資料庫的基本縮放方法。

![水平與垂直相應放大][2]

水平縮放是指加入或移除資料庫來調整容量或整體效能。這也稱為「相應放大」。分區化是常用的水平縮放實作方法，主要是將資料分割到結構相同的一組資料庫上。

垂直縮放是指增加或減少個別資料庫的效能層級，這也稱為「相應增加」。

大部分雲端級別的資料庫應用程式都採用這些兩種策略的組合。比方說，軟體即服務應用程式可能使用水平擴充來供應終端客戶，使用垂直縮放來允許每個終端客戶的資料庫隨工作負載所需而擴大或縮減資源。

* 水平縮放是透過[彈性資料庫用戶端程式庫](sql-database-elastic-database-client-library.md)來管理。

* 垂直縮放是透過 Azure PowerShell Cmdlet 變更服務層，或將資料庫放入彈性資料庫集區中來達成。

## 單一和多租用戶模式

*分區化*是一種將大量相同結構的資料分散在許多獨立資料庫的技術。為一般客戶或企業建立軟體即服務 (SAAS) 供應項目的開發人員尤其愛用。這些一般客戶通常稱為「租用戶」。需要使用分區化可能有各種原因：

* 資料總量太大而超出單一資料庫的條件約束
* 整體工作負載的交易輸送量超過單一資料庫的能力
* 租用戶可能需要彼此實際隔離，因此每個租用戶需要個別的資料庫
* 基於規範、效能或地理政治的理由，資料庫的不同區段可能需要位於不同的地理位置。

在其他情況下，例如從分散式裝置擷取資料，分區化可用於填滿一組暫時組織的資料庫。例如，一個不同的資料庫可專供每日或每週使用。在此情況下，分區化索引鍵可以是表示日期的整數 (出現在分區化資料表的所有資料列)，而擷取日期範圍資訊的查詢，必須由應用程式遞送至涵蓋相關範圍的資料庫子集。

當應用程式中的每一筆交易可以限制為單一分區化索引鍵的單一值時，分區化表現最佳。這可確保所有交易都在特定資料庫的範圍內發生。

有些應用程式採用最簡單的方法，為每個租用戶建立個別的資料庫。這就是**單一租用戶分區化模式**，在租用戶的細微層級上提供隔離、備份/還原能力和資源縮放。使用單一租用戶分區化時，每個資料庫與特定的租用戶識別碼值 (或客戶索引鍵值) 相關聯，但該索引鍵不一定出現在資料本身。應用程式必須負責將每個要求遞送至適當的資料庫 - 用戶端程式庫可以簡化此工作。

![單一租用戶與多租用戶][4]

其他案例將多個租用戶一起放入資料庫中，而不是將它們隔離至個別的資料庫。這就是一般的**多租用戶分區化模式** – 可能是因為應用程式管理大量非常小的租用戶。在多租用戶分區化中，資料庫資料表中的資料列都設計成具有索引鍵 (識別租用戶識別碼) 或分區化索引鍵。同樣地，應用程式層負責將租用戶的要求遞送至適當的資料庫，而彈性資料庫用戶端程式庫支援此功能。此外，資料列層級安全性可用於篩選每個租用戶可以存取的資料列 – 如需詳細資訊，請參閱[使用彈性資料庫工具和資料列層級安全性的多租用戶應用程式](sql-database-elastic-tools-multi-tenant-row-level-security.md)。多租用戶分區化模式可能需要在資料庫之間重新分配資料，而彈性資料庫分割合併工具可協助達成此工作。

### 將資料從多租用戶資料庫移到單一租用戶資料庫
當建立 SaaS 應用程式時，通常會提供試用版軟體給潛在客戶。在此情況下，使用多租用戶資料庫來處理資料較符合成本效益。不過，當潛在客戶變成真正客戶時，單一租用戶資料庫就比較好，因為提供更好的效能。如果客戶已在試用期間建立資料，請使用[分割合併工具](sql-database-elastic-scale-overview-split-and-merge.md)，將資料從多租用戶資料庫移到新的單一租用戶資料庫。

## 後續步驟

如需示範用戶端程式庫的範例應用程式，請參閱[開始使用彈性資料庫工具](sql-database-elastic-scale-get-started.md)。

若要使用分割合併工具，您必須[設定安全性](sql-database-elastic-scale-split-merge-security-configuration.md)。

若要查看彈性資料庫集區的細節，請參閱[彈性資料庫集區的價格和效能考量](sql-database-elastic-pool-guidance.md)，或透過[教學課程](sql-database-elastic-pool-portal.md)建立新的集區。

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

### 懇請給予意見反應！
有什麼我們能改進的地方？ 本主題是否清楚解說此功能？ 或者，您是否對任何地方有疑惑？ 我們致力於提供最好的服務，因此請使用投票按鈕投票，並告訴我們須改進之處 (或可取之處)。如果想要我們連絡您，請在意見反應內留下您的電子郵件。


<!--Anchors-->
<!--Image references-->
[1]: ./media/sql-database-elastic-scale-introduction/tools.png
[2]: ./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]: ./media/sql-database-elastic-scale-introduction/overview.png
[4]: ./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

<!---HONumber=AcomDC_0302_2016-------->