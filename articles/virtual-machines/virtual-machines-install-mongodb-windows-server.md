<properties
	pageTitle="在 Windows VM 上安裝 MongoDB | Microsoft Azure"
	description="了解如何在以執行 Windows Server 的傳統部署模型建立的 Azure VM 上安裝 MongoDB。"
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="10/14/2015"
	ms.author="dkshir"/>

#在 Windows VM 上安裝 MongoDB

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]資源管理員模型。


[MongoDB][MongoDB] 是受歡迎的高效能開放原始碼 NoSQL 資料庫。您可以使用 [Azure 傳統入口網站][AzurePortal]，利用傳統部署模型從映像庫建立一個執行 Windows Server 的虛擬機器。然後在虛擬機器上安裝及設定 MongoDB 資料庫。


## 建立執行 Windows Server 的虛擬機器

請遵循這些指示來建立虛擬機器。

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE]您可以在建立虛擬機器時為 MongoDB 加入端點，並遵循下列方式進行設定：將它命名為 **Mongo**，使用 **TCP** 作為通訊協定，然後將公用和私人通訊埠設定為 **27017**。

## 連接資料磁碟
若要為虛擬機器提供儲存體，請連接並初始化資料磁碟，這樣 Windows 才能使用該磁碟。您可以連接現有磁碟 (如果您已擁有想要使用的資料的話)，或連接空白磁碟。

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

如需初始化磁碟的指示，請參閱[如何將資料磁碟連接至 Windows 虛擬機器](storage-windows-attach-disk.md)中的〈作法：在 Windows Server 中初始化新的資料磁碟〉。

## 在虛擬機器上安裝及執行 MongoDB

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

##摘要
在本教學課程中，您已了解如何建立執行 Windows Server 的虛擬機器、從遠端連線至此虛擬機器，以及連接資料磁碟。同時也已了解如何在 Windows 虛擬機器上安裝及設定 MongoDB。您現在可以存取 Windows 虛擬機器上 MongoDB，方法是遵循 [MongoDB 文件][MongoDocs]中的進階主題。

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com

<!---HONumber=AcomDC_1203_2015-->