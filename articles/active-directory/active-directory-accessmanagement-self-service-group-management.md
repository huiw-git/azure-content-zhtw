<properties
	pageTitle="設定自助式服務應用程式存取管理的 Azure Active Directory | Microsoft Azure"
	description="自助式群組管理概觀，這種管理可讓使用者在 Azure Active Directory 中建立及管理安全性群組或 Office 群組，讓使用者可要求安全性群組或 Office 群組成員資格"
	services="active-directory"
	documentationCenter=""
  authors="curtand"
	manager="stevenpo"
	editor=""
	/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="02/10/2016"
	ms.author="curtand"/>

# 設定 Azure Active Directory 進行自助服務群組管理

自助式群組管理可讓使用者在 Azure Active Directory (Azure AD) 中建立及管理安全性群組或 Office 群組，並讓使用者要求之後可以由群組擁有者核准或拒絕的安全性群組或 Office 群組成員資格。藉由使用自助式群組管理功能，可以將群組成員資格的日常控制權委派給了解該成員資格的商務內容的人員。自助式群組管理功能僅供安全性群組和 Office 365 群組使用，具備郵件功能的安全性群組或通訊群組清單無法使用此功能。

自助式群組管理目前包含兩個重要案例：委派的群組管理和自助式群組管理。


- **委派的群組管理**：例如，管理對系統管理員公司所使用之 SaaS 應用程式存取權的系統管理員。管理這些存取權限會變得很麻煩，所以此系統管理員會要求企業業主建立新的群組。系統管理員現在會將應用程式的存取權指派給企業業主剛才建立的新群組，並將目前可以存取應用程式的所有人員放到此群組。接著，企業業主可以新增更多使用者，而且這些使用者稍後會自動佈建到應用程式。企業業主不需要等待系統管理員執行這個工作，但是可以自己為其使用者管理存取權。系統管理員可以為不同企業集團的行政助理做相同的事情，而且這兩個企業業主和此行政助理現在可以管理其使用者的存取權，而不需要接觸或看見彼此的使用者。系統管理員仍然可以看到可以存取應用程式的所有使用者，並視需要封鎖存取權。


- **自助式群組管理**：例如，都有獨立設定的 SharePoint Online 網站，但卻是真的想要能夠輕鬆地賦與彼此團隊存取權的兩個使用者。因此他們在 Azure AD 中建立一個群組，並各自在 SharePoint Online 中挑選該相同群組來提供對他們網站的存取權。當有人想要存取權時，他們會從 [存取面板] 中要求，而且在核准之後，他們就可以自動取得這兩個 SharePoint Online 網站的存取權。之後，其中一個人會決定存取他的網站的所有人員也可以存取特定的 SaaS 應用程式。他會要求此 SaaS 應用程式的系統管理員，將此應用程式的存取權限新增到他的網站。從那時起，他核准的任何要求都可以存取兩個 SharePoint Online 網站，也可以存取此 SaaS 應用程式。

## 提供可供一般使用者自助服務的群組

在 Azure 入口網站中的 [設定] 索引標籤上，將 [委派群組管理] 參數設定為 [啟用]，然後將 [使用者可以建立安全性群組參數] 或 [使用者可以建立 Office 群組參數] 設定為 [啟用]。

當 [使用者可以建立安全性群組] 參數設定為 [啟用] 時，目錄中的所有使用者都可以建立新的安全性群組，並將成員新增至這些群組。這些新的群組也會顯示在其他所有使用者的 [存取面板] 中，而且如果群組的原則設定允許，其他使用者可以建立加入這些群組的要求。如果此參數設定為 [已停用]，使用者就無法建立群組，也無法變更所屬群組擁有者的現有群組，但是他們仍然可以管理這些群組的成員資格，以及核准其他使用者加入其群組的要求。

您也能夠使用 [可為安全性群組使用自助的使用者] 參數，對您使用者的自助式群組管理功能實現更精細的存取控制。當 [使用者可以建立群組] 參數設定為 [已啟用] 時，目錄中的所有使用者都可以建立新的安全性群組，並將成員新增至這些群組。透過同時將 [可為安全性群組使用自助的使用者] 參數設定為 [部分]，表示您要安全性群組管理限制為只有一組有限的使用者。當此參數設定為 [部分] 時，在您的目錄中會建立一個名為 SSGMSecurityGroupsUsers 的群組，而且之後只有您已經建立此群組成員的使用者可以建立新的安全性群組，並將成員新增至您目錄中的這些群組。透過將 [可為安全性群組使用自助的使用者] 參數設定為 [全部]，表示您讓目錄中的所有使用者建立新的安全性群組。

您也能夠使用 [可為安全性群組使用自助的群組] 欄位 (預設設為 ‘SSGMSecurityGroupsUsers’)，為將保留所有使用者使用自助服務的能力的群組，指定您自己的自訂名稱，並在您的目錄中建立新的安全性群組。

## 其他資訊

這些文章提供有關 Azure Active Directory 的其他資訊。

* [使用 Azure Active Directory 群組管理資源的存取權](active-directory-manage-groups.md)

* [Article Index for Application Management in Azure Active Directory (Azure Active Directory 中應用程式管理的文件索引)](active-directory-apps-index.md)

* [什麼是 Azure Active Directory？](active-directory-whatis.md)

* [整合內部部署身分識別與 Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0218_2016-->