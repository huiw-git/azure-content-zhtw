<properties 
	pageTitle="驗證偵測期間發生錯誤" 
	description="Active directory 連線精靈偵測到不相容的驗證類型" 
	services="active-directory" 
	documentationCenter="" 
	authors="TomArcher" 
	manager="douge" 
	editor=""/>
  
<tags 
	ms.service="active-directory" 
	ms.workload="web" 
	ms.tgt_pltfrm="vs-getting-started" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="12/10/2015" 
	ms.author="tarcher"/>

# 驗證偵測期間發生錯誤

偵測先前的驗證程式碼時，精靈偵測到不相容的驗證類型。

##檢查什麼？

**注意：**必須建置專案，才能正確偵測專案中先前的認證程式碼。如果遇到這個錯誤，且您的專案中沒有先前的驗證碼，請重建並再試一次。

###專案類型

精靈會檢查您正在開發的專案類型，以便可以將正確的驗證邏輯插入專案。如果專案中有衍生自 `ApiController` 的任何控制器，則該專案將被視為 WebAPI 專案。如果專案中只有衍生自 `MVC.Controller` 的控制器，則該專案將被視為 MVC 專案。精靈不支援其他類型的專案。目前不支援 WebForms 專案。

###相容的驗證碼

精靈也會檢查先前對精靈設定或與精靈相容的驗證設定。如果所有設定皆存在，則會將它視為可重新進入的情況，而精靈將會開啟並顯示設定。如果只有部分設定存在，則會將它視為錯誤情況。

在 MVC 專案中，精靈會檢查先前使用此精靈所產生的以下任何設定：

	<add key="ida:ClientId" value="" />
	<add key="ida:Tenant" value="" />
	<add key="ida:AADInstance" value="" />
	<add key="ida:PostLogoutRedirectUri" value="" />

此外，精靈會檢查先前使用此精靈所產生 Web API 專案中的以下任何設定：

	<add key="ida:ClientId" value="" />
	<add key="ida:Tenant" value="" />
	<add key="ida:Audience" value="" />

###不相容的驗證碼

最後，精靈會嘗試偵測舊版 Visual Studio 所設定的驗證碼版本。如果收到此錯誤，表示您的專案包含不相容的驗證類型。精靈會從舊版 Visual Studio 中偵測下列驗證類型：

* Windows 驗證 
* 個別使用者帳戶 
* 組織帳戶 
 

為偵測 MVC 專案中的「Windows 驗證」，精靈會在您的 **web.config** 檔案中尋找 `authentication` 元素。

<pre>
	&lt;configuration>
	    &lt;system.web>
	        <span style="background-color: yellow">&lt;authentication mode="Windows" /></span>
	    &lt;/system.web>
	&lt;/configuration>
</pre>

為偵測 Web API 專案中的「Windows 驗證」，精靈會在您專案的 **.csproj** 檔案中尋找 `IISExpressWindowsAuthentication` 元素：

<pre>
	&lt;Project>
	    &lt;PropertyGroup>
	        <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication>enabled&lt;/IISExpressWindowsAuthentication></span>
	    &lt;/PropertyGroup>
	&lt;/Project>
</pre>

為偵測「個別使用者帳戶」驗證，精靈會在您的 **Packages.config** 檔案中尋找 package 元素。

<pre>
	&lt;packages>
	    <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /></span>
	&lt;/packages>
</pre>

為偵測舊式「組織帳戶」驗證，精靈會在 **web.config** 中尋找下列元素：

<pre>
	&lt;configuration>
	    &lt;appSettings>
	        <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /></span>
	    &lt;/appSettings>
	&lt;/configuration>
</pre>

若要變更驗證類型，請移除不相容的驗證類型，然後重新執行精靈。

如需詳細資訊，請參閱 [Azure AD 的驗證案例](active-directory-authentication-scenarios.md)。

<!---HONumber=AcomDC_1217_2015-->