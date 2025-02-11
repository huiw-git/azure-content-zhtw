<properties 
   pageTitle="流量管理員監視"
   description="本文將協助了解和設定流量管理員的監視功能"
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/07/2015"
   ms.author="joaoma" />

# 關於流量管理員監視

Azure 流量管理員會監視您的各個端點 (包括雲端服務和網站)，確保它們都可以使用。為了讓監視正常運作，您必須對流量管理員設定檔中指定的每個端點以相同的方式進行設定。設定監視之後，流量管理員會在 Azure 傳統入口網站中顯示端點和設定檔的狀態。您可以在 Azure 傳統入口網站中的 [設定] 頁面上，設定流量管理員設定檔的監視設定。您可以指定下列設定：

- **通訊協定** – 選擇 HTTP 或 HTTPS。請務必注意，HTTPS 監視不會驗證您的 SSL 憑證是否有效，它只會檢查該憑證是否存在。

- **連接埠** – 選擇要求所使用的連接埠。標準 HTTP 和 HTTPS 連接埠皆可。

- **相對路徑和檔案名稱** – 提供監視系統將嘗試存取的檔案路徑和名稱。請注意，正斜線 "/" 可用於相對路徑，表示檔案位於根目錄中 (預設值)。

## 關於監視健全狀態

Azure 流量管理員會在 Azure 傳統入口網站中顯示設定檔和端點服務的健康情況。設定檔和端點的狀態欄會顯示最新的監視狀態。您可以根據流量管理員監視設定，使用此狀態了解設定檔的健康情況。當您的設定檔狀況良好時，DNS 查詢會根據設定檔的流量路由設定 (循環配置資源、效能或容錯移轉) 散發到您的服務。一旦流量管理員監視系統偵測到監視狀態中有變更，它會更新 Azure 傳統入口網站中的狀態項目。從狀態變更到重新整理可能需要五分鐘的時間。

### 端點監視狀態

下表中的端點監視狀態是端點健康情況探查結果與您的設定檔和端點組態組合的結果。

|設定檔狀態|端點狀態|端點監視狀態 (API 和入口網站)|注意事項|
|---|---|---|---|
|已停用|已啟用|非使用中|已停用的設定檔不會受到監視。但是已停用設定檔中的端點狀態仍然可以進行管理。|
|「任何」|已停用|已停用|已停用的設定檔不會受到監視。但是已停用設定檔中的端點狀態仍然可以進行管理。|
|已啟用|已啟用|線上|端點受到監視且狀況良好。|
|已啟用|已啟用|已降級|端點受到監視且狀況不良。|
|已啟用|已啟用|正在檢查端點|端點受到監視，但尚未收到第一次探查的結果。此狀態只會在設定檔中剛加入新端點，或者剛啟用端點或設定檔時暫時發生。|
|已啟用|已啟用|已停止|基礎雲端服務或網站未執行。|

### 設定檔監視狀態

下表中的設定檔監視狀態是端點監視狀態與設定的設定檔狀態組合的結果。

|設定檔狀態 (依設定)|端點監視狀態|設定檔監視狀態 (API 和入口網站)|注意事項|
|---|---|---|---|
|已停用|「任何」或未定義端點的設定檔。|已停用|端點不會受到監視。|
|已啟用|至少有一個端點的狀態為「已降級」。|已降級|這是不需要客戶動作的旗標。|
|已啟用|至少有一個端點的狀態為「線上」。沒有任何端點為「已降級」。|線上|服務正在接收流量，不需要客戶動作。|
|已啟用|至少有一個端點的狀態為「正在檢查端點」。沒有端點為「線上」或「已降級」。|正在檢查端點|轉換狀態。這通常發生在設定檔剛啟用，而且正在探查端點健康情況。|
|已啟用|設定檔中定義的所有端點狀態為「已停用」或「已停止」，或者設定檔有未定義的端點。|非使用中|沒有端點作用中，但設定檔仍然啟用中。|

## 監視的運作方式

以下顯示的範例時間軸，說明單一雲端服務的監視程序。此案例顯示下列資訊：

- 雲端服務可以使用，而且「只」透過此流量管理員設定檔接收流量。

- 雲端服務變為無法使用。

- 雲端服務持續無法使用的時間超過 DNS 存留時間 (TTL)。

- 雲端服務再次變為可以使用。

- 雲端服務恢復「只」透過此流量管理員設定檔接收流量。

![流量管理員監視順序](./media/traffic-manager-monitoring/IC697947.jpg)

**圖 1** – 監視順序範例。圖表中的號碼對應至下列編號說明。

1. **GET** – 流量管理員監視系統對您在監視設定中指定的路徑和檔案執行 GET。
2. **200 OK** – 監視系統預期在 10 秒鐘內收到 HTTP 200 OK 訊息。當系統收到此回應時，會假設雲端服務可以使用。 

>[AZURE.NOTE]如果傳回的訊息是 200 OK，流量管理員只會將端點視為線上。如果系統收到非 200 的回應時，會假設端點無法使用，而且會將此次計算為失敗的檢查。如需疑難排解失敗的檢查的詳細資訊，請參閱[疑難排解 Azure 流量管理員上的已降級狀態](traffic-manager-troubleshooting-degraded.md)。

3. **每次檢查之間間隔 30 秒** – 每隔 30 秒執行這項檢查。
4. **雲端服務無法使用** – 雲端服務變為無法使用。流量管理員要直到下次監視檢查時才會知道。
5. **嘗試存取監視檔案 (嘗試 4 次)** – 監視系統執行 GET，但沒有在 10 秒鐘或更短時間之內收到回應。接著會每隔 30 秒執行一次，再嘗試三次。這表示監視系統最多需要花費大約 1.5 分鐘的時間偵測出服務變為無法使用。如果其中一次嘗試成功，會重設嘗試次數。雖然圖表中沒有顯示，但如果在 GET 之後超過 10 秒鐘才傳回 200 OK 訊息，監視系統仍會將此次計算為失敗的檢查。
6. **標示為降級** – 連續四次失敗之後，監視系統會將無法使用的雲端服務標示為「已降級」。 

7. **雲端服務的流量減少** – 流量可能會繼續流向無法使用的雲端服務。用戶端將會遇到失敗，因為服務無法使用。用戶端和次要 DNS 伺服器已經快取無法使用之雲端服務的 IP 位址的 DNS 記錄。它們會繼續將公司網域的 DNS 名稱解析為服務的 IP 位址。此外，次要 DNS 伺服器可能仍會送出無法使用之服務的 DNS 資訊。由於用戶端和次要 DNS 伺服器會更新，流向無法使用之服務的 IP 位址的流量會變慢。監視系統繼續每隔 30 秒執行一次檢查。在此範例中，服務沒有回應，並且仍然無法使用。
8. **雲端服務的流量停止** – 這時候，大部分的 DNS 伺服器和用戶端應該都已經更新，流向無法使用之服務的流量會停止。流量完全停止之前的最長時間取決於 TTL 時間。預設的 DNS TTL 為 300 秒 (5 分鐘)。使用此值，用戶端會在 5 分鐘之後停止使用服務。監視系統繼續每隔 30 秒執行一次檢查，雲端服務沒有回應。
9. **雲端服務回到線上狀態並接收流量** – 服務變為可以使用，但是流量管理員要直到監視系統執行檢查之後才會知道。
10. **服務的流量恢復** - 流量管理員傳送 GET 並在 10 秒鐘內收到 200 OK。然後，它開始在 DNS 伺服器要求更新時送出雲端服務的 DNS 名稱。結果，流量會開始再次流向服務。

## 巢狀設定檔的子端點和父端點狀態

下表描述流量管理員在使用巢狀設定檔的子設定檔和父設定檔與 minChildEndpoints 設定時的監視行為。如需詳細資訊，請參閱[什麼是流量管理員？](traffic-manager-overview.md)。

|子設定檔監視狀態|父端點監視狀態|注意事項|
|---|---|---|
|已停用：這是由於您停用設定檔所造成。|已停止|父端點狀態為「已停止」，不是「已停用」。「已停用」狀態會保留，表示您已停用父設定檔中的端點。|
|已降級：至少有一個子端點為「已降級」狀態。|如果子設定檔中的「線上」端點數目至少為 minChildEndpoints 的值，則為「線上」狀態。如果子設定檔中的「線上」加上「正在檢查端點」端點數目至少為 minChildEndpoints 的值，則為「正在檢查端點」狀態。否則為「已降級」狀態。|流量會路由至「正在檢查端點」狀態的端點。如果 minChildEndpoints 設定太高，父端點將會一律為已降級。|
|線上：至少有一個子項為「線上」狀態，且都不在「已降級」狀態。|同上。||
|正在檢查端點：至少有一個為「正在檢查端點」；沒有任何為「線上」或「已降級」|同上。||
|非使用中：所有端點都「已停用」或「已停止」，或者這是沒有端點的設定檔|已停止||

## 設定監視特定的路徑和檔案名稱

1. 在您計畫要納入設定檔中的每個端點上建立具有相同名稱的檔案。
2. 針對每個端點，使用網頁瀏覽器來測試檔案的存取權。URL 由特定端點 (雲端服務或網站) 的網域名稱、檔案的路徑和檔案名稱所組成。 
3. 在 Azure 傳統入口網站中的 [監視設定] 下，於 [相對路徑和檔案名稱] 欄位中，指定路徑和檔案名稱。
4. 當您完成變更組態時，請按一下頁面底部的 [儲存]。

## 另請參閱

[建立設定檔](traffic-manager-manage-profiles.md)

[新增端點。](traffic-manager-endpoints.md)

[疑難排解 Azure 流量管理員上的已降級狀態](traffic-manager-troubleshooting-degraded.md)
 

<!-------HONumber=AcomDC_1210_2015--->