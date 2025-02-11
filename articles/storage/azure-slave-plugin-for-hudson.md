<properties
    pageTitle="如何搭配使用 Azure 從屬外掛程式與 Hudson 連續整合"
    description="說明如何搭配使用 Azure 從屬外掛程式與 Hudson 連續整合。"
	services="storage"
	documentationCenter="java"
	authors="rmcmurray"
	manager="wpickett"
	editor="jimbe" />

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="01/11/2016"
	ms.author="robmcm"/>

# 如何搭配使用 Azure 從屬外掛程式與 Hudson 連續整合

適用於 Hudson 的 Azure 從屬外掛程式可讓您在執行分散式組建時在 Azure 上佈建從屬節點。

## 安裝 Azure 從屬外掛程式
1. 在 Hudson 儀表板中，按一下 [**管理 Hudson**]。
2. 在 [**管理 Hudson**] 頁面中，按一下 [**管理外掛程式**]。
3. 按一下 [Available] 索引標籤。
4. 按一下 [**搜尋**] 並輸入 **Azure**，讓清單只顯示相關外掛程式。

	如果您選擇透過捲動來查看可用的外掛程式清單，您會在 [**其他**] 索引標籤的 [**叢集管理和分散式組建**] 區段下找到 Azure 從屬外掛程式。

5. 選取 [**Azure 從屬外掛程式**] 的核取方塊。
6. 按一下 [Install]。
7. 重新啟動 Hudson。

現在您已安裝外掛程式，接下來的步驟是在 Azure 訂用帳戶設定檔中設定外掛程式，以及建立用來建立從屬節點 VM 的範本。


## 在 Azure 訂用帳戶設定檔中設定 Azure 從屬外掛程式

訂用帳戶設定檔也稱為發佈設定，它是一個 XML 檔案，內含要在開發環境中使用 Azure 所需具備的安全認證和一些額外資訊。若要設定 Azure 從屬外掛程式，您需要：

* 訂用帳戶識別碼
* 訂用帳戶的管理憑證

您可以在訂用帳戶設定檔中找到這些資訊。如果您沒有訂用帳戶設定檔，可以從[這裡](https://manage.windowsazure.com/publishsettings/Index?SchemaVersion=2.0)下載。以下是訂用帳戶設定檔的範例。

	<?xml version="1.0" encoding="utf-8"?>

		<PublishData>

  		<PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

    	<Subscription

      		ServiceManagementUrl="https://management.core.windows.net"

      		Id="<Subscription ID>"

      		Name="Pay-As-You-Go"
			ManagementCertificate="<Management certificate value>" />

  		</PublishProfile>

	</PublishData>

有了訂用帳戶設定檔後，請遵循下列步驟來設定 Azure 從屬外掛程式。

1. 在 Hudson 儀表板中，按一下 [**管理 Hudson**]。
2. 按一下 [**設定系統**]。
3. 向下捲動頁面來找出 [**雲端**] 區段。
4. 按一下 [**新增雲端 > Microsoft Azure**]。



	![新增雲端](./media/azure-slave-plugin-for-hudson/hudson-setup-addcloud.png)

	此動作會顯示一些欄位，您必須在其中輸入訂用帳戶的詳細資料。

	![設定設定檔](./media/azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png)

5. 從訂用帳戶設定檔複製訂用帳戶識別碼和管理憑證，並貼至適當欄位。

	在複製訂用帳戶識別碼和管理憑證時，請**不要**將括住值的引號也包括進來。

6. 按一下 [**驗證組態**]。
7. 成功驗證組態後，按一下 [**儲存**]。

## 設定 Azure 從屬外掛程式的虛擬機器範本

虛擬機器範本會定義外掛程式用來在 Azure 上建立從屬節點的參數。在下列步驟中，我們將會建立 Ubuntu VM 的範本。

1. 在 Hudson 儀表板中，按一下 [**管理 Hudson**]。
2. 按一下 [**設定系統**]。
3. 向下捲動頁面來找出 [**雲端**] 區段。
4. 在 [**雲端**] 區段內，找到 [**新增 Azure 虛擬機器範本**]，然後按一下 [**新增**] 按鈕。



	![新增 VM 範本](./media/azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png)

5. 在 [**名稱**] 欄位中，指定雲端服務名稱。如果您指定的名稱參照現有雲端服務，便會在該服務中佈建 VM。否則，Azure 會建立一個新的。
6. 在 [**說明**] 欄位中，輸入您要建立之範本的說明文字。此資訊僅供記錄之用，並不會用於佈建 VM。
7. 在 [**標籤**] 欄位中，輸入 **linux**。此標籤可用來識別您要建立的範本，而且後續會用來在建立 Hudson 工作時參照範本。
8. 選取將要建立 VM 的區域。
9. 選取適當的 VM 大小。
10. 指定將要建立 VM 的儲存體帳戶。請確定它與您將要使用的雲端服務位在相同的區域中。如果您想要建立新的儲存體，可以將此欄位保留空白。
11. 保留時間會指定 Hudson 要等待幾分鐘的時間才將閒置的從屬節點刪除。請讓此欄位保持使用預設值 60。
12. 在 [**使用量**] 中，選取將會使用此從屬節點的適當條件。就目前的情況，請選取 [**盡可能利用此節點**]。



	到目前為止，您的表單應該會與下圖類似：

	![範本設定](./media/azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png)

13. 在 [**映像系列或識別碼**] 中，您必須指定要在您的 VM 上安裝的系統映像。您可以從映像系列清單中進行選取，或指定自訂映像。

	如果您想從映像系列清單中進行選取，請輸入映像系列名稱的第一個字元 (需區分大小寫)。例如，輸入 **U** 將會顯示 Ubuntu Server 系列的清單。在從清單中進行選取後，Jenkins 將會在佈建 VM 時使用該系列的該系統映像的最新版本。

	![作業系統系列清單](./media/azure-slave-plugin-for-hudson/hudson-oslist.png)

	如果您想要改用自訂映像，請輸入該自訂映像的名稱。清單中不會顯示自訂映像名稱，因此您必須確實輸入正確的名稱。

	針對此教學課程，請輸入 **U** 來顯示 Ubuntu 映像的`清單，並選取 [**Ubuntu Server 14.04 LTS**]。

14. 在 [**啟動方法**] 中，選取 [**SSH**]。
15. 複製下列指令碼並貼到 [**Init 指令碼**] 欄位。

		# Install Java
		sudo apt-get -y update

		sudo apt-get install -y openjdk-7-jdk

		sudo apt-get -y update --fix-missing

		sudo apt-get install -y openjdk-7-jdk



		# Install git

		sudo apt-get install -y git



		#Install ant

		sudo apt-get install -y ant

		sudo apt-get -y update --fix-missing

		sudo apt-get install -y ant

	[**Init 指令碼**] 就會在 VM 建立好之後執行。在此範例中，指令碼會安裝 Java、git 和 ant。

16. 在 [**使用者名稱**] 和 [**密碼**] 欄位中，針對將在 VM 上建立的系統管理員帳戶輸入偏好使用的值。
17. 按一下 [**驗證範本**] 以檢查所指定的參數是否有效。
18. 按一下 [**儲存**]。


## 建立在 Azure 的從屬節點上執行的 Hudson 工作

在本節中，您將建立在 Azure 的從屬節點上執行的 Hudson 工作。

1. 在 Hudson 儀表板中，按一下 [**新增工作**]。
2. 輸入要建立之工作的名稱。
3. 針對工作類型選取 [**建置自由樣式的軟體作業**]。
4. 按一下 [**確定**]。
5. 在工作組態頁面中，選取 [**限制可以執行這個專案的位置**]。
6. 選取 [**節點和標籤功能表**]，然後選取 [**linux**] \(上一節在建立虛擬機器範本時，我們指定了這個標籤)。

7. 在 [**組件**] 區段中，按一下 [**新增組件步驟**]，然後選取 [**執行殼層**]。
8. 編輯下列指令碼，將 **(github 帳戶名稱)**、**(專案名稱)** 和 **(專案目錄)** 取代為適當值，並在出現的文字區域中貼上編輯過的指令碼。


		# Clone from git repo

		currentDir="$PWD"

		if [ -e (your project directory) ]; then

  			cd (your project directory)

  			git pull origin master

		else

  			git clone https://github.com/(your github account name)/(your project name).git

		fi

		# change directory to project

		cd $currentDir/(your project directory)



		#Execute build task

		ant

9. 按一下 [**儲存**]。
10. 在 Hudson 儀表板中，找到您剛才建立的工作，然後按一下 [**排程組建**] 圖示。

Hudson 就會使用上一節建立的範本建立從屬節點，並執行您針對這項工作指定於組建步驟中的指令碼。

<!----HONumber=AcomDC_0114_2016-->