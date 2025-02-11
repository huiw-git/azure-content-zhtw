<properties
	pageTitle="開始使用推播 (Android) | Microsoft Azure"
	description="了解如何使用 Azure 行動服務傳送推播通知至 Android .Net 應用程式。"
	services="mobile-services, notification-hubs"
	documentationCenter="android"
	authors="RickSaling"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="java"
	ms.topic="article"
	ms.date="02/07/2016"
	ms.author="ricksal"/>

# 將推播通知新增至行動服務應用程式

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

本主題說明如何使用 Azure 行動服務傳送推播通知至 Android 應用程式。在本教學課程中，您會使用 Google 雲端通訊 (GCM) 將推播通知新增至快速入門專案。完成後，行動服務就會在每次插入記錄時傳送推播通知。

本教學課程會以行動服務快速入門為基礎。開始本教學課程之前，您必須先完成[開始使用行動服務]，將專案連接到行動服務。同樣地，本教學課程也需要 Visual Studio 2013。

## 範例程式碼
若要檢視完整的原始程式碼，請前往[這裡](https://github.com/RickSaling/mobile-services-samples/tree/push/GettingStartedWithPush)。

## 啟用 Google 雲端通訊

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## 設定行動服務以傳送推播要求

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

## 更新行動服務以傳送推播通知

[AZURE.INCLUDE [mobile-services-dotnet-backend-android-push-update-service](../../includes/mobile-services-dotnet-backend-android-push-update-service.md)]

## 將推播通知新增至應用程式

###驗證 Android SDK 版本

[AZURE.INCLUDE [mobile-services-verify-android-sdk-version](../../includes/mobile-services-verify-android-sdk-version.md)]


下一個步驟是安裝 Google Play 服務。Google 雲端通訊在開發和測試方面有一些 API 層級的最低需求，這些是資訊清單中的 **minSdkVersion** 屬性所必須遵守。

如果您要以較舊的裝置進行測試，請參考 [設定 Google Play 服務 SDK]，以確認此值可以設得多低，並加以適當設定。

###新增 Google Play 服務至專案

[AZURE.INCLUDE [新增 Play 服務](../../includes/mobile-services-add-google-play-services.md)]

###新增程式碼

[AZURE.INCLUDE [mobile-services-android-getting-started-with-push](../../includes/mobile-services-android-getting-started-with-push.md)]

## 對已發佈的行動服務進行應用程式測試

您可以使用 USB 纜線直接連接 Android 手機，或使用模擬器中的虛擬裝置，對應用程式進行測試。

### 啟用推播通知以進行本機測試

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push](../../includes/mobile-services-dotnet-backend-configure-local-push.md)]

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

您已成功完成此教學課程。

## 後續步驟

本教學課程示範讓 Android 應用程式使用行動服務和通知中樞傳送推播通知的基礎。接下來，請考慮完成下一個教學課程「將推播通知傳送給驗證的使用者」，該課程將說明如何使用標記，從行動服務將推播通知只傳送給已驗證的使用者。

+ [將廣播通知傳送給訂閱者] <br/>了解使用者如何註冊及接收其所需之類別的推播通知。

+ [將範本型通知傳送給訂閱者] <br/>了解如何使用範本，從行動服務傳送推播通知，但不必在後端製作平台特定的裝載。

在下列主題中深入了解行動服務和通知中心：

* [什麼是通知中樞？] <br/>深入了解通知中心如何跨所有主要用戶端平台將通知傳遞到您的應用程式。

* [偵錯通知中心應用程式](http://go.microsoft.com/fwlink/p/?linkid=386630) </br>取得通知中心解決方案的疑難排解和偵錯指引。

* [如何使用適用於行動服務的 Android 用戶端程式庫] <br/>深入了解如何搭配 Android 使用行動服務。

<!-- Anchors. -->

[Create a new mobile service]: #create-service
[Download the service locally]: #download-the-service-locally
[Test the mobile service]: #test-the-service
[Download the GetStartedWithData project]: #download-app
[Update the app to use the mobile service for data access]: #update-app
[Test the Android App against the service hosted locally]: #test-locally-hosted
[Publish the mobile service to Azure]: #publish-mobile-service
[Test the Android App against the service hosted in Azure]: #test-azure-hosted
[Test the app against the published mobile service]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications (Eclipse)]: mobile-services-dotnet-backend-android-get-started-push-ec.md
[開始使用行動服務]: mobile-services-dotnet-backend-android-get-started.md
[Mobile Services SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545

[如何使用適用於行動服務的 Android 用戶端程式庫]: mobile-services-android-how-to-use-client-library.md

[什麼是通知中樞？]: ../notification-hubs-overview.md
[將廣播通知傳送給訂閱者]: ../notification-hubs-windows-store-dotnet-send-breaking-news.md
[將範本型通知傳送給訂閱者]: ../notification-hubs-windows-store-dotnet-send-localized-breaking-news.md

<!----HONumber=AcomDC_0211_2016-->