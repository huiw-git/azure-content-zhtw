
<properties
	pageTitle="疑難排解群組的動態成員資格 | Microsoft Azure"
	description="在 Azure AD 中列出群組動態成員資格的疑難排解秘訣的主題。"
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
	ms.topic="article"
	ms.date="02/09/2016"
	ms.author="curtand"/>


# 疑難排解群組的動態成員資格

| 徵狀 | 動作 |
|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 我針對某個群組上設定了一個規則，但是群組中的成員資格沒有更新 | 請確認 [**啟用委派的群組管理**] 設定在 [目錄設定] 索引標籤中設為 [是]。只有當您以 Azure Active Directory Premium 授權指派對象的使用者身分登入時，才看得到這項設定。請針對規則確認使用者屬性的值：是否有符合規則的使用者？ |
| 我設定了一個規則，但是現在現有的規則成員遭到移除 | 這是預期行為。因為在啟用或變更規則時，現有群組的成員會遭到移除。從評估規則所傳回的使用者會當作成員，新增至群組。 |
| 當我新增或變更規則時，我沒有立即看到成員資格變更，為什麼？ | 專用的成員資格評估會定期在非同步的背景處理序中完成。該程序所需的時間，取決於您目錄中的使用者數目以及因規則所建立的群組大小。具有少量使用者的目錄通常會在很短的時間內看到群組成員資格變更。具有大量使用者的目錄則可能需要 30 分鐘或更長的時間填入。 |

這些文章提供有關 Azure Active Directory 的其他資訊。

* [使用 Azure Active Directory 群組管理資源的存取權](active-directory-manage-groups.md)
* [Article Index for Application Management in Azure Active Directory (Azure Active Directory 中應用程式管理的文件索引)](active-directory-apps-index.md)
* [什麼是 Azure Active Directory？](active-directory-whatis.md)
* [整合內部部署身分識別與 Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0211_2016-->