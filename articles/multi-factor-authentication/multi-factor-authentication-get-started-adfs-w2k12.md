<properties 
	pageTitle="搭配 Windows Server 2012 R2 AD FS 使用 Azure MFA Server 保護雲端和內部部署資源" 
	description="這是說明如何在 Windows Server 2012 R2 上開始使用 Azure MFA 和 AD FS 的 Azure Multi-Factor Authentication 頁面。" 
	services="multi-factor-authentication" 
	documentationCenter="" 
	authors="billmath" 
	manager="stevenpo" 
	editor="curtland"/>

<tags ms.service="multi-factor-authentication" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="get-started-article" ms.date="03/04/2016"" ms.author="billmath"/>


# 搭配 Windows Server 2012 R2 AD FS 使用 Azure Multi-Factor Authentication Server 保護雲端和內部部署資源

如果您的組織與 Azure AD 結盟，而內部部署或雲端中有您想要保護的資源，您可以使用 Azure Multi-Factor Authentication Server 並將它設定為搭配 AD FS 運作，以便對高價值端點觸發多因素驗證。

這份文件將說明如何搭配 Windows Server 2012 R2 中的 AD FS 使用 Azure Multi-Factor Authentication Server。如需有關搭配 AD FS 2.0 使用 Azure Multi-Factor Authentication 的資訊，請參閱[搭配 AD FS 2.0 使用 Azure Multi-Factor Authentication Server 保護雲端和內部部署資源](multi-factor-authentication-get-started-adfs-adfs2.md)。

## 使用 Azure Multi-Factor Authentication Server 保護 Windows Server 2012 R2 AD FS

安裝 Azure Multi-Factor Authentication Server 時，您有下列兩個選項：

- 在與 AD FS 相同的伺服器本機上安裝 Azure Multi-Factor Authentication Server 
- 在 AD FS 伺服器本機上安裝 Azure Multi-Factor Authentication Adapter，並在另一部電腦上安裝 MFA Server

開始之前，請留意下列項目：

- 不一定要在 AD FS 同盟伺服器上安裝 Azure Multi-Factor Authentication Server，但必須在執行 AD FS 的 Windows Server 2012 R2 上安裝適用於 AD FS 的 Multi-Factor Authentication Adapter。您可以在不同的電腦上安裝伺服器 (只要是支援的版本即可) 以及在 AD FS 同盟伺服器上個別安裝 AD FS 配接器。如需個別安裝配接器的相關指示，請參閱下列程序。
- 在設計 Multi-Factor Authentication Server 的 AD FS 配接器時，預期 AD FS 可以將信賴憑證者的名稱傳遞至可做為應用程式名稱的配接器。不過，結果事實並非如此。如果使用簡訊或行動應用程式驗證方法，則在 [公司設定] 中定義的字串會包含預留位置 "<$application\_name$>"。使用 AD FS 配接器時並不會取代此預留位置。因此，建議在保護 AD FS 時，從適當的字串中移除此預留位置。

- 登入的帳戶必須有在 Active Directory 中建立安全性群組的權限。

- Multi-Factor Authentication AD FS Adapter 安裝精靈會在 Active Directory 中建立名為 PhoneFactor Admins 的安全性群組，然後將 Federation Service 的 AD FS 服務帳戶加入至這個群組。建議您在網域控制站上確認確實已建立 PhoneFactor Admins 群組，而且 AD FS 服務帳戶是此群組的成員。如有必要，請以手動方式將 AD FS 服務帳戶加入至網域控制站上的 PhoneFactor Admins 群組。
  

### 若要在與 AD FS 相同的伺服器本機上安裝 Azure Multi-Factor Authentication Server

1. 在 AD FS 同盟伺服器上下載並安裝 Azure Multi-Factor Authentication Server。如需有關安裝 Azure Multi-Factor Authentication 伺服器的資訊，請參閱[開始使用 Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-server.md)
2. 在 Azure Multi-Factor Authentication Server 使用者介面中，選取 [AD FS] 圖示並選取 [允許使用者註冊] 和 [允許使用者選取方法] 的選項。
3. 選取其他任何選項。
4. 按一下 [安裝 AD FS 配接器]。
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. 如果電腦已加入某個網域，而用於保護 AD FS Adapter 與 Multi-Factor Authentication 服務間通訊的 Active Directory 設定尚未完成，就會顯示 Active Directory 步驟。按 [下一步] 按鈕自動完成此設定，或勾選 [略過自動 Active Directory 設定並手動進行設定] 核取方塊，然後按 [下一步]。
6. 如果電腦未加入某個網域，而用於保護 AD FS Adapter 與 Multi-Factor Authentication 服務間通訊的本機群組設定尚未完成，就會顯示本機群組步驟。按 [下一步] 按鈕自動完成此設定，或勾選 [略過自動本機群組設定並手動進行設定] 核取方塊，然後按 [下一步]。
7. 安裝精靈隨即出現，按 [下一步] 可讓 Azure Multi-Factor Authentication Server 建立 PhoneFactor Admins 群組並將 AD FS 服務帳戶加入至 PhoneFactor Admins 群組。
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. 在 [啟動安裝程式] 步驟中，按 [下一步]。
9. 在 Multi-Factor Authentication AD FS Adapter 安裝程式中，按 [下一步]。
10. 在安裝完成時按一下 [關閉]。
11. 現已安裝配接器，但必須向 AD FS 登錄。開啟 Windows PowerShell 並執行下列命令：C:\\Program Files\\Multi-Factor Authentication Server\\Register-MultiFactorAuthenticationAdfsAdapter.ps1
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. 現在我們需要編輯 AD FS 中的通用驗證原則，才能使用最近登錄的配接器。在 AD FS 管理主控台中，瀏覽至 [驗證原則] 節點，然後在 [Multi-Factor Authentication] 區段下，按一下 [通用設定] 子區段旁邊的 [編輯] 連結。在 [編輯通用驗證原則] 視窗中，選取 [Multi-Factor Authentication] 作為其他驗證方法，然後按一下 [確定]。此配接器會登錄為 WindowsAzureMultiFactorAuthentication。您必須重新啟動 AD FS 服務，登錄才會生效。

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

此時，Multi-Factor Authentication Server 已設定為要搭配 AD FS 使用的其他驗證提供者。

## 使用 Web 服務 SDK 安裝 AD FS Adapter 獨立版
1. 在執行 Multi-Factor Authentication Server 的伺服器上安裝 Web 服務 SDK
2. 將 \\Program Files\\Multi-Factor Authentication Server 目錄中的 MultiFactorAuthenticationAdfsAdapterSetup64.msi、Register-MultiFactorAuthenticationAdfsAdapter.ps1、Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 和 MultiFactorAuthenticationAdfsAdapter.config 檔案複製到您打算安裝 AD FS Adapter 的伺服器。
3. 執行 MultiFactorAuthenticationAdfsAdapterSetup64.msi。
4. 在 Multi-Factor Authentication AD FS Adapter 安裝程式中，按 [下一步] 來執行安裝。
5. 在安裝完成時按一下 [關閉] 按鈕。
6. 請執行下列作業來編輯 MultiFactorAuthenticationAdfsAdapter.config 檔案：

MultiFactorAuthenticationAdfsAdapter.config 步驟| 子步驟
:------------- | :------------- |
將 UseWebServiceSdk 節點設定為 true。||
將 WebServiceSdkUrl 設定為 Multi-Factor Authentication Web 服務 SDK 的 URL。||
選項 1 - 以使用者名稱和密碼設定 Web 服務 SDK。|<ol><li>將 WebServiceSdkUsername 設定為屬於 PhoneFactor Admins 安全性群組的帳戶。使用 <domain><username> 格式。<li>將 WebServiceSdkPassword 設定為適當的帳戶密碼。</li></ol>
選項 2 - 以用戶端憑證設定 Web 服務 SDK。|<ol><li>從執行 Web 服務 SDK 之伺服器的憑證授權單位取得用戶端憑證。</li><li>將用戶端憑證匯入執行 Web 服務 SDK 的伺服器上的本機電腦個人憑證存放區。注意：請確定憑證授權單位的公開憑證是在受信任的根憑證中。</li><li>將用戶端憑證的公開和私人金鑰匯出至 .pfx 檔案。</li><li>將 Base 64 格式的公開金鑰匯出至 .cer 檔案。</li><li>在 [伺服器管理員] 中，確認已安裝網頁伺服器 (IIS)\\網頁伺服器\\安全性\\IIS 用戶端憑證對應驗證功能。</li><li>如果未安裝，請選擇 [新增角色及功能] 來新增此功能。</li><li>在 [IIS 管理員] 中，按兩下包含 Web 服務 SDK 虛擬目錄的網站中的 [設定編輯器]。注意：請務必在網站層級而非虛擬目錄層級執行此作業。</li><li>瀏覽至 system.webServer/security/authentication/iisClientCertificateMappingAuthentication 區段。</li><li>將 enabled 設定為 true。</li><li>將 oneToOneCertificateMappingsEnabled 設定為 true。</li><li>按一下 oneToOneMappings 旁邊的 [...] 按鈕。</li><li>按一下 [新增] 連結。</li><li>開啟先前匯出的 base-64.cer 檔案。移除 -----BEGIN CERTIFICATE-----、-----END CERTIFICATE----- 和任何分行符號。複製產生的字串。</li><li>將憑證設定為在上一個步驟中複製的字串。</li><li>將 enabled 設定為 true。</li><li>將 userName 設定為屬於 PhoneFactor Admins 安全性群組的帳戶。使用 <domain><username> 格式。</li><li>將 password 設定為適當的帳戶密碼。</li><li>關閉 [集合編輯器]。</li><li>按一下 [套用] 連結。</li><li>瀏覽至 Web 服務 SDK 虛擬目錄。</li><li>按兩下 [驗證]。</li><li>確認 [ASP.NET 模擬] 和 [基本驗證] 為 [已啟用]，而所有其他項目為 [已停用]。</li><li>再次瀏覽至 Web 服務 SDK 虛擬目錄。</li><li>按兩下 [SSL 設定]。</li><li>將 [用戶端憑證] 設定為 [接受]，然後按一下 [套用]。</li><li>將先前匯出的 .pfx 檔案複製到執行 AD FS 配接器的伺服器。</li><li>將 .pfx 檔案匯入至本機電腦個人憑證存放區。</li><li>從快顯功能表中選擇 [管理私密金鑰]，並將讀取權授與用來登入 Active Directory Federation Services 服務的帳戶。</li><li>開啟用戶端憑證，並從 [詳細資料] 索引標籤複製憑證指紋。</li><li>在 MultiFactorAuthenticationAdfsAdapter.config 檔案中，將 WebServiceSdkCertificateThumbprint 設定為在上一個步驟中複製的字串。</li></ol>
編輯 Register-MultiFactorAuthenticationAdfsAdapter.ps1 指令碼，將 -ConfigurationFilePath <path> 加入至 Register-AdfsAuthenticationProvider 命令的結尾，其中 <path> 是 MultiFactorAuthenticationAdfsAdapter.config 檔案的完整路徑。|


現在，在 PowerShell 中執行 \\Program Files\\Multi-Factor Authentication Server\\Register-MultiFactorAuthenticationAdfsAdapter.ps1 指令碼以註冊配接器。此配接器會登錄為 WindowsAzureMultiFactorAuthentication。您必須重新啟動 AD FS 服務，登錄才會生效。




























 

 


 

 


 





 


 

























































































 


 

 






 

<!---HONumber=AcomDC_0309_2016-->