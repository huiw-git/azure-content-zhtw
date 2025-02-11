<properties 
	pageTitle="連接到 SQL Database：最佳做法 | Microsoft Azure" 
	description="針對從 ADO.NET 和 PHP 之類的技術連接到 Azure SQL Database 的用戶端程式，統合收集連結和最佳做法建議的入門主題。" 
	services="sql-database" 
	documentationCenter="" 
	authors="MightyPen" 
	manager="jeffreyg" 
	editor=""/>


<tags 
	ms.service="sql-database" 
	ms.workload="data-management" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/07/2016" 
	ms.author="genemi"/>


# 連接到 SQL Database：最佳做法和設計方針


本主題是開始使用至 Azure SQL Database 的用戶端連線的好地方。它提供指向各種技術之程式碼範例的連結，可用來連接到 SQL Database 並與其互動。這些技術包括企業程式庫、JDBC 和 PHP 和更多技術。所提供的資訊適用於您用來連接到 SQL Database 的所有特定技術。


<a id="a-tech-independent-recommend" name="a-tech-independent-recommend"></a>

## 與技術無關的建議


- [以程式設計方式連接到 Azure SQL Database 的方針](http://msdn.microsoft.com/library/azure/ee336282.aspx) - 討論包括：
 - [連接埠和防火牆](sql-database-configure-firewall-settings.md)
 - 連接字串
- [Azure SQL Database 資源管理](http://msdn.microsoft.com/library/azure/dn338083.aspx) - 討論包括：
 - 資源管理
 - 限制強制執行
 - 節流


<a id="b-authentication-recommend" name="b-authentication-recommend"></a>

## 驗證建議


- 使用 Azure SQL Database 驗證，而不是 Azure SQL Database 中未提供的 Windows 驗證。
- 指定一個特定的資料庫，而非預設為 *master* 資料庫。
 - 您無法在 SQL Database 上使用 **USE myDatabaseName;** 陳述式來切換到其他資料庫。


### 自主使用者


將某人新增為 SQL Database 的使用者時，您有一些選擇：

- 將*登入*與密碼新增至 **master** 資料庫，然後再將對應的*使用者*新增至相同伺服器上的一或多個其他資料庫。

- 將*自主使用者*與密碼新增至一或多個資料庫，但沒有 **master** 中任何*登入*的連結。


自主使用者方法有其優點和缺點︰

- 優點是當資料庫中的所有使用者都是自主使用者時，資料庫即可輕鬆地在 Azure SQL Database 伺服器之間移動。

- 缺點是例行管理更加困難。例如：
 - 卸除數個自主使用者比卸除一個登入更具挑戰性。
 - 身為數個資料庫中的自主使用者的人員可能需要記住或更新更多密碼。


如需進一步資訊，請參閱[自主資料庫使用者 - 讓資料庫具有可攜性](http://msdn.microsoft.com/library/ff929188.aspx)。


<a id="c-connection-recommend" name="c-connection-recommend"></a>

## 連接建議


- 在您的用戶端連線邏輯中，將預設的逾時覆寫為 30 秒。
 - 預設 15 秒對於依賴網際網路的連線而言太短。


- 在託管您的用戶端程式的電腦上，請確定防火牆允許連接埠 1433 上的傳出 TCP 通訊。


- 如果您的用戶端在 Azure 虛擬機器 (VM) 上執行時，用戶端程式連接至 SQL Database V12，您就必須開啟此 VM 上 11000-11999 和 14000-14999 範圍的連接埠。如需詳細資訊，請按一下[這裡](sql-database-develop-direct-route-ports-adonet-v12.md)。


- 若要處理*暫時性錯誤*，請將[*重試*邏輯](#TransientFaultsAndRetryLogicGm)加入至與 Azure SQL Database 互動的用戶端程式。


### 連接集區


如果您使用的是[連接集區](http://msdn.microsoft.com/library/8xx3tyca.aspx)，請在程式未主動使用時即時關閉連接，而不是準備重複使用連接。

除非您的程式將立即重複使用其他作業的連線，而不暫停，否則我們建議使用下列模式：

- 開啟連線。
- 透過連線執行一項作業。
- 關閉連線。


### V12 中 1433 以外的連接埠


與 Azure SQL Database V12 的用戶端連線有時會略過 proxy 並直接與資料庫互動。1433 以外的連接埠變得重要。如需詳細資訊，請參閱：<br/> [針對 ADO.NET 4.5 和 SQL Database V12 的 1433 以外的連接埠](sql-database-develop-direct-route-ports-adonet-v12.md)


下一節會更詳細說明重試邏輯和暫時性錯誤處理。



<a name="TransientFaultsAndRetryLogicGm" id="TransientFaultsAndRetryLogicGm"></a>

&nbsp;

## 暫時性錯誤和重試邏輯


Azure 系統能夠在 SQL Database 服務出現繁重的工作負載時動態重新設定伺服器。

不過，重新設定可能會導致您的用戶端程式遺失其 SQL Database 連接。這種錯誤稱為「暫時性錯誤」。

如果您的用戶端程式具有重試邏輯，在給予暫時性錯誤一些時間自行修正後，可以嘗試重新建立連接。

我們建議您在您第一次重試前延遲 5 秒鐘。在少於 5 秒的延遲後重試，雲端服務會有超過負荷的風險。對於後續每次重試，延遲應以指數方式成長，最大值為 60 秒。

在 [SQL Server 連接集區 (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx) 中可找使用 ADO.NET 之用戶端的封鎖期間討論。


如需說明重試邏輯的程式碼範例，請參閱：

- [SQL Database 的用戶端快速入門程式碼範例](sql-database-develop-quick-start-client-code-samples.md)


### 暫時性錯誤的錯誤號碼


當 SQL Database 發生任何錯誤時，會擲回 [SqlException](http://msdn.microsoft.com/library/system.data.sqlclient.sqlexception.aspx)。**SqlException** 在它的 **Number** 屬性中包含一個數字錯誤碼。如果錯誤碼識別暫時性錯誤，您的程式應該重試呼叫。


- [SQL Database 用戶端程式的錯誤訊息](sql-database-develop-error-messages.md#bkmk_connection_errors)
 - 它的 [**暫時性錯誤、連接中斷錯誤**] 區段是一個有理由自動重試的暫時性錯誤清單。
 - 例如，如果出現錯誤號碼 40613，表示發生類似如下的狀況，則重試：<br/>*伺服器 'theserver' 上的資料庫 'mydatabase' 目前無法使用。*


如需進一步資訊，請參閱：

- [Azure SQL Database 開發：使用說明主題](http://msdn.microsoft.com/library/azure/ee621787.aspx)

<!--  (per Penny Lee, 2016/01/07.  MightyPen==GeneMi)
- [Troubleshoot connection problems to Azure SQL Database](http://support.microsoft.com/kb/2980233/)
-->


<a id="e-technologies" name="e-technologies"></a>

## 技術


下列主題包含數個語言和驅動程式技術的程式碼範例連結，可讓您從用戶端程式連接到 Azure SQL Database。


會針對在 Windows、Linux 和 Mac OS X 上執行的用戶端提供各種程式碼範例。


**一般範例：**有各種不同的程式設計語言 (包括 PHP、Python、Node.js 和 .NET CSharp) 的[程式碼範例](sql-database-develop-quick-start-client-code-samples.md)。此外，也提供在 Windows、Linux 和 Mac OS X 上執行的用戶端範例。


**Elastic Scale：**如需 Elastic Scale 資料庫連接的相關資訊，請參閱：

- [開始使用 Azure SQL Database Elastic Scale 預覽](sql-database-elastic-scale-get-started.md)
- [資料相依路由](sql-database-elastic-scale-data-dependent-routing.md)


**驅動程式庫：**如需連接驅動程式庫的相關資訊 (包括建議的版本)，請參閱：

- [SQL Database 和 SQL Server 的連接庫](sql-database-libraries.md)

<!---HONumber=AcomDC_0114_2016-->