<properties
	pageTitle="開始使用適用於 HTML/JavaScript App 的 Azure 行動服務 | Microsoft Azure"
	description="遵循此教學課程，可開始使用 Azure 行動服務進行 HTML 開發。"
	services="mobile-services"
	documentationCenter=""
	authors="ggailey777"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-html5"
	ms.devlang="javascript"
	ms.topic="get-started-article" 
	ms.date="11/30/2015"
	ms.author="glenga"/>


# <a name="getting-started"> </a>開始使用行動服務

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

##概觀 

本教學課程說明如何使用 Azure行動服務在 HTML 應用程式中新增雲端型後端服務。在本教學課程中，您將建立新的行動服務和簡單的*待辦事項清單*應用程式，後者會在前者儲存應用程式資料。您可以於下方檢視本教學課程的影片版本。

> [AZURE.VIDEO mobile-get-started-html]
 
以下是完成應用程式的螢幕擷取畫面：

![][0]

此教學課程是 HTML 應用程式其他所有行動服務教學課程的先修課程。若為 PhoneGap/Cordova App，請參閱本教學課程的 [PhoneGap/Cordova 版本](mobile-services-javascript-backend-phonegap-get-started.md)。

##必要條件

需要有下列項目，才能完成本教學課程：

+ 您的本機電腦必須執行下列其中一部網頁伺服器：

	+  **在 Windows 上**：IIS Express。IIS Express 是由 [Microsoft Web Platform Installer] 所安裝。
	+  **在 MacOS X 上**：Python (應該已安裝)。
	+  **在 Linux 上**：Python。您必須安裝[最新版本的 Python] (英文)。

	您可以使用任何網頁伺服器來裝載應用程式，但是這些網頁伺服器需受所下載的指令碼支援。

+ 支援 HTML5 的網頁瀏覽器。
+ 一個 Azure 帳戶。如果您沒有帳戶，只需要幾分鐘的時間就可以建立免費試用帳戶。如需詳細資訊，請參閱 [Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fzh-TW%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-html%2F"%20target="_blank)。 


## <a name="create-new-service"> </a>建立新的行動服務

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 建立新的 HTML 應用程式

當您建立自己的行動服務之後，就可以依照 Azure 傳統入口網站中簡單的快速入門，來建立新的應用程式或修改現有的應用程式，以便連線到您的行動服務。

在本節中，您將建立連線至您行動服務的新 HTML 應用程式。

1.  在 [Azure 傳統入口網站]中，按一下 [行動服務]，然後按一下您剛建立的行動服務。


2. 在 [快速入門] 索引標籤中，按一下 [Choose platform] 下的 [Windows]，然後展開 [Create a new HTML app]。

   	![][6]

   	這將顯示三個簡單步驟，可用來建立和主控連接到您行動服務的 HTML 應用程式。

  	![][7]

3. 按一下 [Create TodoItems table] 以建立儲存應用程式資料的資料表。

4. 在 [Download and run your app] 下，按 [下載]。

  	如此會下載連接至行動服務之範例 _To do list_ 應用程式的網站檔案。請將壓縮檔儲存至本機電腦，並記下儲存位置。

5. 在 [設定] 索引標籤中，確認 `localhost` 已列在 [跨原始資源共用 (CORS)] 的 [允許來自主機名稱的要求] 清單中。否則，請在 [主機名稱] 欄位中輸入 `localhost`，然後按一下 [儲存]。

  	![][9]

	> [AZURE.IMPORTANT] 如果您將快速入門應用程式部署至 localhost 以外的 Web 伺服器，您必須將該 Web 伺服器的主機名稱新增至 [允許提出要求的主機名稱] 清單。如需詳細資訊，請參閱[跨原始來源資源分享](http://msdn.microsoft.com/library/windowsazure/dn155871.aspx) (英文)。

## 裝載並執行 HTML 應用程式

本教學課程的最終階段是在本機電腦上裝載並執行新的應用程式。

1. 瀏覽至儲存壓縮專案檔的位置，在電腦上將檔案解壓縮，然後從 **server** 子資料夾中啟動下列其中一個命令檔。

	+ **launch-windows** (Windows 電腦)
	+ **launch-mac.command** (Mac OS X 電腦)
	+ **launch-linux.sh** (Linux 電腦)

	> [AZURE.NOTE] 在 Windows 電腦上，PowerShell 要求您確認是否要執行指令碼時，請輸入 `R`。因為指令碼是從網際網路中下載，所以您的網頁瀏覽器可能會警告您不要執行指令碼。發生此情況時，您必須要求瀏覽器繼續載入指令碼。

	如此會在本機電腦上啟動網頁伺服器來裝載新的應用程式。

2. 在網頁瀏覽器中開啟 URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> 以啟動應用程式。

3. 在應用程式中，於 [Enter new task] 中輸入有意義的文字 (例如 **Complete the tutorial**)，然後按一下 [新增]。

   	![][10]

   	如此會傳送 POST 要求到 Azure 中代管的新行動服務。要求中的資料會插入 TodoItem 資料表中。行動服務會傳回資料表中儲存的項目，而該資料會顯示在應用程式的第二欄。

	> [AZURE.NOTE] 您可以檢視會存取您的行動服務來查詢及插入資料的程式碼 (位於 page.js 檔案中)。

4. 回到 [Azure 傳統入口網站]，按一下 [資料] 索引標籤，然後按一下 [TodoItems] 資料表。

   	![][11]

   	如此可讓您瀏覽由應用程式插入資料表中的資料。

   	![][12]

## <a name="next-steps"> </a>後續步驟
請注意，您已完成快速入門，並了解如何執行行動服務中的其他重要工作：

* **[在您的應用程式中新增驗證功能]** 
   了解如何利用身分識別提供者來驗證您應用程式的使用者。

* **[行動服務 HTML/JavaScript 作法概念參考]**
   深入了解如何搭配使用行動服務與 HTML/JavaScript


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Getting started with Mobile Services]: #getting-started
[Create a new mobile service]: #create-new-service
[Define the mobile service instance]: #define-mobile-service-instance
[Next Steps]: #next-steps

<!-- Images. -->
[0]: ./media/mobile-services-html-get-started/mobile-quickstart-completed-html.png

[6]: ./media/mobile-services-html-get-started/mobile-portal-quickstart-html.png
[7]: ./media/mobile-services-html-get-started/mobile-quickstart-steps-html.png

[9]: ./media/mobile-services-html-get-started/mobile-services-set-cors-localhost.png
[10]: ./media/mobile-services-html-get-started/mobile-quickstart-startup-html.png
[11]: ./media/mobile-services-html-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-html-get-started/mobile-data-browse.png


<!-- URLs. -->
[在您的應用程式中新增驗證功能]: mobile-services-html-get-started-users.md

[Azure 傳統入口網站]: https://manage.windowsazure.com/
[Microsoft Web Platform Installer]: http://go.microsoft.com/fwlink/p/?LinkId=286333
[最新版本的 Python]: http://go.microsoft.com/fwlink/p/?LinkId=286342
[行動服務 HTML/JavaScript 作法概念參考]: mobile-services-html-how-to-use-client-library.md
[Cross-origin resource sharing]: http://msdn.microsoft.com/library/azure/dn155871.aspx
 

<!---HONumber=AcomDC_0128_2016-->