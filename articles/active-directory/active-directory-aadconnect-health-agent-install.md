<properties
	pageTitle="Azure AD Connect Health 代理程式安裝 | Microsoft Azure"
	description="這是 Azure AD Connect Health 頁面，其中說明 AD FS 與同步處理的代理程式安裝。"
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="02/17/2016"
	ms.author="billmath"/>






# Azure AD Connect Health 代理程式安裝

本文件將逐步引導您安裝和設定適用於 AD FS 的 Azure AD Connect Health 代理程式與同步處理。

>[AZURE.NOTE]請記住，您必須先在目標伺服器上安裝 Azure AD Connect Health 代理程式，才會在 Azure AD Connect Health 執行個體中看見 AD FS 資料。請務必在安裝代理程式之前，先完成[這裡](active-directory-aadconnect-health.md#requirements)的需求。您可以在[這裡](http://go.microsoft.com/fwlink/?LinkID=518973)下載代理程式：


## 安裝適用於 AD FS 的 Azure AD Connect Health 代理程式
若要啟動代理程式安裝，請按兩下您所下載的 .exe 檔案。在第一個畫面上，按一下 [安裝]。

![驗證 Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install1.png)

一旦安裝完成之後，按一下 [立即設定]。

![驗證 Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install2.png)

這將會啟動命令提示字元，然後再啟動將執行 Register-AzureADConnectHealthADFSAgent 的特定 PowerShell。系統將提示您登入 Azure。繼續進行並登入。

![驗證 Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install3.png)


登入後，PowerShell 將會繼續。一旦完成之後，您就可以關閉 PowerShell，且設定已完成。

此時，應該會自動啟動服務，且代理程式現在將會監視並收集資料。請注意，如果您還未符合前幾節所述的所有必要條件，您將會在 PowerShell 視窗中看到警告。請務必在安裝代理程式之前，先完成[這裡](active-directory-aadconnect-health.md#requirements)的需求。下列螢幕擷取畫面是這些錯誤的其中一個範例。

![驗證 Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install4.png)

若要確認已安裝代理程式，請開啟服務，並尋找下列項目。如果您已完成設定，這些服務應該正在執行。否則，他們將不會啟動，直到設定完成為止。

- Azure AD Connect Health AD FS 診斷服務
- Azure AD Connect Health AD FS Insights 服務
- Azure AD Connect Health AD FS 監視服務

![驗證 Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install5.png)


### Windows Server 2008 R2 伺服器上的代理程式安裝

若是 Windows Server 2008 R2 伺服器，請執行以下操作：

1. 請確定伺服器在 Service Pack 1 或更高版本上執行。
1. 關閉 IE ESC 以安裝代理程式：
1. 在安裝 AD Health 代理程式之前，先在每部伺服器上安裝 Windows PowerShell 4.0。安裝 Windows PowerShell 4.0：
 - 使用下列連結下載離線安裝程式，以安裝 [Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779)。
 - 安裝 PowerShell ISE (從 Windows 功能)
 - 安裝 [Windows Management Framework 4.0。](https://www.microsoft.com/download/details.aspx?id=40855)
 - 在伺服器上安裝 Internet Explorer 10 或更新版本。這是 Health 服務所必需，才能使用 Azure 系統管理員認證進行驗證。
1. 如需有關在 Windows Server 2008 R2 上安裝 Windows PowerShell 4.0 的其他資訊，請參閱[這裡](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx)的 Wiki 文章。

### 啟用 AD FS 的稽核

若要讓使用情況分析功能收集並分析資料，Azure AD Connect Health 代理程式需要 AD FS 稽核記錄檔中的資訊。預設不會啟用這些記錄檔。這只適用於 AD FS 同盟伺服器。您不需要在 AD FS Proxy 伺服器或 Web 應用程式 Proxy 伺服器上啟用稽核。使用下列程序啟用 AD FS 稽核，並找出 AD FS 稽核記錄檔。

#### 啟用 AD FS 2.0 的稽核

1. 按一下 [開始]，依序指向 [程式集] 和 [系統管理工具]，然後按一下 [本機安全性原則]。
2. 瀏覽至 **Security Settings\\Local Policies\\User Rights Management** 資料夾，然後按兩下 [產生安全性稽核]。
3. 在 [本機安全性設定] 索引標籤上，確認已列出 AD FS 2.0 服務帳戶。如果不存在，按一下 [新增使用者或群組]，並將其新增至清單中，然後按一下 [確定]。
4. 使用提高的權限開啟命令提示字元，然後執行下列命令以啟用稽核：<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. 關閉 [本機安全性原則]，然後開啟 [管理嵌入式管理單元]。若要開啟 [管理嵌入式管理單元]，按一下 [開始]，依序指向 [程式集] 和 [系統管理工具]，然後按一下 [AD FS 2.0 管理]。
6. 在 [動作] 窗格中，按一下 [編輯同盟服務屬性]。
7. 在 [同盟服務屬性] 對話方塊中，按一下 [事件] 索引標籤。
8. 選取 [成功稽核] 和 [失敗稽核] 核取方塊。
9. 按一下 [確定]。

#### 在 Windows Server 2012 R2 上啟用 AD FS 的稽核

1. 在 [開始] 畫面上開啟 [伺服器管理員]，或在桌面上的工作列中開啟 [伺服器管理員]，以開啟 [本機安全性原則]，然後按一下 [工具/本機安全性原則]。
2. 瀏覽至 **Security Settings\\Local Policies\\User Rights Assignment** 資料夾，然後按兩下 [產生安全性稽核]。
3. 在 [本機安全性設定] 索引標籤上，確認已列出 AD FS 服務帳戶。如果不存在，按一下 [新增使用者或群組]，並將其新增至清單中，然後按一下 [確定]。
4. 使用提高的權限開啟命令提示字元，然後執行下列命令以啟用稽核：<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. 關閉 [本機安全性原則]，然後開啟 [AD FS 管理] 嵌入式管理單元 (在 [伺服器管理員] 中，按一下 [工具]，然後選取 [AD FS 管理])。
6. 在 [動作] 窗格中，按一下 [編輯同盟服務屬性]。
7. 在 [同盟服務屬性] 對話方塊中，按一下 [事件] 索引標籤。
8. 選取 [成功稽核] 和 [失敗稽核] 核取方塊，然後按一下 [確定]。






#### 找出 AD FS 稽核記錄檔


1. 開啟 [事件檢視器]。
2. 移至 [Windows 記錄]，然後選取 [安全性]。
3. 按一下右側的 [篩選目前的記錄檔]。
4. 在 [事件來源] 下，選取 [AD FS 稽核]。

![AD FS 稽核記錄](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] 如果您有即將停用 AD FS 稽核的群組原則，則 Azure AD Connect Health 代理程式將無法收集資訊。請確定您沒有可能即將停用稽核的群組原則。

[//]: # "Start of Agent Proxy Configuration Section"

## 安裝適用於同步處理的 Azure AD Connect Health 代理程式
適用於同步處理的 Azure AD Connect Health 代理程式會自動安裝在 Azure AD Connect 的最新組建中。如果要使用適用於同步處理的 Azure AD Connect，您必須下載最新版的 Azure AD Connect 並安裝它。您可以在[此處](http://www.microsoft.com/download/details.aspx?id=47594)下載最新版本。

若要確認已安裝代理程式，請開啟服務，並尋找下列項目。如果您已完成設定，這些服務應該正在執行。否則，他們將不會啟動，直到設定完成為止。

- Azure AD Connect Health AadSync Insights 服務
- Azure AD Connect Health AadSync Monitoring 服務

![驗證適用於同步處理的 Azure AD Connect Health](./media/active-directory-aadconnect-health-sync/services.png)

>[Azure.NOTE] 請記住，使用 Azure AD Connect Health 需要 Azure AD Premium。如果您沒有 Azure AD Premium，將無法完成 Azure 入口網站中的組態。如需詳細資訊，請參閱[此處](active-directory-aadconnect-health.md#requirements)的需求。




## 設定 Azure AD Connect Health 代理程式使用 HTTP Proxy
您可以設定 Azure AD Connect Health 代理程式使用 HTTP Proxy。

>[AZURE.NOTE]
- "Netsh WinHttp set Proxy" 無法運作，因為代理程式使用 System.Net (而不是 Microsoft Windows HTTP 服務) 提出 Web 要求。
- 設定的 Http Proxy 地址會用來當做傳遞加密 Https 訊息。
- 不支援已驗證的 Proxy (使用 HTTPBasic)。

### 變更健康情況代理程式 Proxy 組態
您有下列選項來設定 Azure AD Connect Health 代理程式使用 HTTP Proxy。

>[AZURE.NOTE] 您必須重新啟動所有 Azure AD Connect Health 代理程式服務，Proxy 設定才會更新。執行下列命令：<br> Restart-Service AdHealth*

#### 匯入現有的 Proxy 設定

##### 從 Internet Explorer 匯入
您可以匯入您的 Internet Explorer HTTP Proxy 設定，並將它們用於 Azure AD Connect Health 代理程式，方法是在執行健康情況代理程式的每部伺服器上執行下列 PowerShell 命令。

	Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### 從 WinHTTP 匯入
您可以在執行健康情況代理程式的每部伺服器上執行下列 PowerShell 命令，以匯入您的 WinHTTP Proxy 設定。

	Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### 以手動方式指定 Proxy 位址
您可以在執行健康情況代理程式的每部伺服器上執行下列 PowerShell 命令，以手動指定 Proxy 伺服器。

	Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

範例：*Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver:443*

- 「位址」可以是可解析的 DNS 伺服器名稱或 IPv4 位址
- 「連接埠」可以省略。如果省略，則會選擇 443 做為預設連接埠。

#### 清除現有的 Proxy 設定
您可以執行下列命令來清除現有的 Proxy 設定。

	Set-AzureAdConnectHealthProxySettings -NoProxy


### 讀取目前的 Proxy 設定
您可以使用下列命令來讀取的目前設定的 Proxy 設定。

	Get-AzureAdConnectHealthProxySettings


## 測試對 Azure AD Connect Health 服務的連線
有可能會發生導致 Azure AD Connect Health 代理程式與 Azure AD Connect Health 服務連線中斷的問題。這些包括網路問題、權限問題或各種其他原因。

如果代理程式無法將資料傳送給 Azure AD Connect Health 服務達 2 小時以上，您將會看到指出「健康情況服務資料不是最新的資料。」的警示。 如果發生這種情況，您現在可以從代理程式發生問題的電腦執行下列 PowerShell 命令，來測試 Azure AD Connect Health 代理程式是否能夠將資料上傳到 Azure AD Connect Health 服務。

    Test-AzureADConnectHealthConnectivity -Role Adfs

角色參數目前可接受下列值：
	
- Adfs
- Sync

您可以在命令中使用 -ShowResults 旗標來檢視詳細的記錄。請使用下列範例：

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]為了使用連線工具，您必須先完成代理程式註冊。如果您無法完成代理程式註冊，請確定您已符合 Azure AD Connect Health 的所有[需求](active-directory-aadconnect-health.md#requirements)。這項連線測試預設是在代理程式註冊期間執行。


## 相關連結

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health 操作](active-directory-aadconnect-health-operations.md)
* [在 AD FS 使用 Azure AD Connect Health](active-directory-aadconnect-health-adfs.md)
* [使用 Azure AD Connect Health 進行同步處理](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health 常見問題集](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health 版本歷程記錄](active-directory-aadconnect-health-version-history.md)

<!---HONumber=AcomDC_0309_2016-->