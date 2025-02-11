<properties
   pageTitle="在 Azure 資訊安全中心中套用磁碟加密 | Microsoft Azure"
   description="本文件說明如何實作 Azure 資訊安全中心建議的「套用磁碟加密」。"
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/23/2016"
   ms.author="terrylan"/>

# 在 Azure 資訊安全中心中套用磁碟加密

如果您有未使用 Azure 磁碟加密進行加密的 Windows 或 Linux VM 磁碟，Azure 資訊安全中心會建議您套用磁碟加密。磁碟加密可讓您替 Windows 和 Linux IaaS VM 磁碟加密。建議您的 VM 上的作業系統和資料磁碟區都進行加密。


磁碟加密利用 Windows 的業界標準 [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) 功能和 Linux 的 [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) 功能以及資料加密功能，協助保護您的資料並滿足您組織的安全性和法規遵循承諾。磁碟加密與 [Azure 金鑰保存庫](https://azure.microsoft.com/documentation/services/key-vault/)整合，可幫助您控制和管理您的金鑰保存庫訂用帳戶中的磁碟加密金鑰和密碼，同時確保 VM 磁碟中的所有資料會在您的 [Azure 儲存體](https://azure.microsoft.com/documentation/services/storage/)中輕鬆加密。

> [AZURE.NOTE] 下列 Windows 伺服器作業系統支援 Azure 磁碟加密 - Windows Server 2008 R2、Windows Server 2012 和 Windows Server 2012 R2。下列 Linux 伺服器作業系統支援磁碟加密 - Ubuntu、CentOS、SUSE 和 SUSE Linux Enterprise Server (SLES)。

## 實作建議

1. 在 [建議] 刀鋒視窗中，選取 [套用磁碟加密]。
2. 在 [套用磁碟加密] 刀鋒視窗中，您會看到建議進行磁碟加密的 VM 清單。
3. 依照指示將加密套用至這些 VM。

![][1]

## 後續步驟

本文件說明如何實作 Azure 資訊安全中心建議的「套用磁碟加密」。 若要深入了解磁碟加密，請參閱下列主題：

- [Azure 金鑰保存庫的加密和金鑰管理](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (視訊，36 分 39 秒) -- 了解如何使用的 IaaS VM 和 Azure 金鑰保存庫的磁碟加密管理功能，協助保護您的資料。
- [Azure 磁碟加密](../azure-security-disk-encryption.md) (文件) -- 了解如何為 Windows 和 Linux VM 啟用磁碟加密。

如要深入了解資訊安全中心，請參閱下列主題：

- [設定 Azure 資訊安全中心的安全性原則](security-center-policies.md) -- 了解如何設定安全性原則。
- [Azure 資訊安全中心的安全性健康情況監視](security-center-monitoring.md) -- 了解如何監視 Azure 資源的健康情況。
- [管理與回應 Azure 資訊安全中心的安全性警示](security-center-managing-and-responding-alerts.md) -- 了解如何管理與回應安全性警示。
- [管理 Azure 資訊安全中心的安全性建議](security-center-recommendations.md) -- 了解建議如何協助您保護您的 Azure 資源。
- [Azure 資訊安全中心常見問題集](security-center-faq.md) -- 尋找在使用服務時經常會遇到的問題。
- [Azure 安全性部落格](http://blogs.msdn.com/b/azuresecurity/) -- 尋找有關 Azure 安全性與相容性的部落格文章。



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png

<!---HONumber=AcomDC_0224_2016-->