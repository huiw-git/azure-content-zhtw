<properties
   pageTitle="可用性檢查清單 | Microsoft Azure"
   description="提供設計期間可用性考量指引的檢查清單。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="masimms"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="masashin"/>

# 可用性檢查清單

![](media/best-practices-availability-checklist/pnp-logo.png)

## 應用程式設計

- **請避免單一失敗點。** 為避免單一失敗點影響可用性，應該將所有的元件、服務、資源和計算執行個體部署為多個執行個體。這包括驗證機制。將應用程式設計為可以設定使用多個執行個體，自動偵測失敗，並將要求重新導向至非失敗的執行個體 (平台並不會在其中自動執行此動作)。
- **分解每個不同 SLA 的工作負載。** 如果服務由重要和比較不重要的工作負載組成，請以不同的方式加以管理，並指定服務功能及執行個體數目，以符合其可用性需求。
- **最小化和了解服務相依性。** 盡可能將使用過的不同服務數目降到最低，並確定您了解所有存在於系統中的功能和服務相依性。這包括這些相依性的本質、失敗的影響，或整體應用程式上每個服務減少的效能。Microsoft 保證大部分的服務都會有至少 99.9%的可用性，但這表示應用程式依賴的每個其他服務可能會降低 0.1% 的系統整體可用性 SLA。
- **盡可能將工作和訊息設計為等冪**，以便重複的要求不會造成問題。例如，服務可以做為處理訊息的取用者，這些訊息由系統中做為生產者的其他部分傳送為要求。如果取用者在處理訊息之後，在確認訊息已處理過之前失敗，生產者可能會提交重複要求，由取用者的另一個執行個體處理。基於這個理由，取用者與它們執行的作業應該具有等冪性，以便重複先前執行的作業不會使結果無效。這可能表示偵測重複訊息，或是使用開放式的方法來處理衝突，以確保一致性。
- **使用可實作重要交易之高可用性的訊息代理人。** 起始工作或存取遠端服務的許多案例都使用傳訊在應用程式和目標服務之間傳遞指示。為了達到最佳效能，應用程式應該能夠傳送訊息，然後返回處理更多的要求，而不需要等待回覆。若要保證訊息的傳遞，傳訊系統應該提供高可用性。服務匯流排訊息佇列會實作_至少一個_語意，雖然可能在某些情況下傳送複本，張貼到佇列的每個訊息都不會遺失。如果訊息處理是等冪 (請參閱上一個項目)，重複傳遞應該不是問題。
- 達到資源限制時**設計適當降級的應用程式**，並採取適當動作將對使用者的影響降到最低。在某些情況下，應用程式上的負載可能會超出其中一或多個組件的容量，導致降低可用性和失敗的連線。縮放比例有助於減輕這點，但是它可能會到達資源可用性或成本等因素所加諸的限制。設計應用程式，以便在此情況下，它可以自動適當降級。例如，在電子商務系統中，如果訂單處理子系統處於疲勞狀態 (或甚至完全失敗)，系統的該部分可以暫時停用，同時允許其他功能 (例如瀏覽產品目錄) 繼續運作。適當作法是延後對失敗子系統的要求，例如讓客戶可以提交訂單，但是將訂單儲存至佇列或其他安全的儲存機制，以供訂單子系統稍後再次可用時加以處理。
- **適當處理急促高載事件。** 大部分的應用程式會在不同時間處理不同的工作負載，例如在早上的商務應用程式中，或在電子商務網站中發行新產品時處理第一件事的尖峰。自動調整有助於處理負載，但是它可能需要一些時間，讓其他執行個體上線並處理要求。將應用程式設計為將要求加入其使用之服務的佇列，並在佇列接近完整容量時適當降級，以防止突然和非預期的活動高載導致超出應用程式的處理能力。請確定有足夠的效能和容量可供非高載條件清空佇列和處理未完成的要求。如需詳細資訊，請參閱[佇列型負載調節模式](https://msdn.microsoft.com/library/dn589783.aspx)。

## 部署和維護

- **為每個服務部署角色的多個執行個體。** Microsoft 提供您所建立與部署之服務的可用性保證，但這些保證的有效前提是您必須在服務中為每個角色部署至少兩個執行個體。這可讓其中一個角色無法使用，但其他角色仍在作用中。如果您必須將更新部署到即時系統，而不要中斷用戶端的活動，這點特別重要。執行個體可以個別中斷和升級，而其他執行個體則維持線上狀態。
- **在多個資料中心裝載應用程式。** 雖然非常不可能，但是整個資料中心還是可能因為天然災害或主幹網際網路失敗等事件而離線。重要的商務應用程式應該裝載在多個資料中心，以提供最大的可用性。這一點的其他好處在於它可以減少本機使用者的延遲，並在更新應用程式時提供彈性的其他機會。
- **自動化和測試部署和維護工作。** 分散式應用程式由多個必須相互合作的部分所組成。因此，部署應該使用經過測試和實證的機制 (力如可更新並驗證組態，以及自動化部署程序的指令碼和部署應用程式) 自動化。自動化的技術也應該用來執行全部或部分應用程式的更新。請務必完整測試所有程序，以確保錯誤不會造成其他停機情況。所有的部署工具必須有適當的安全性限制，來保護已部署的應用程式。仔細定義和強制執行部署原則並將人為介入的需要降到最低。
- **請考慮使用平台的預備和生產功能**，其中皆提供上述內容。例如，使用 Azure 雲端服務預備與生產環境可讓應用程式透過虛擬 IP 位址交換 (VIP 交換) 的方式立即彼此切換。不過，如果您想要暫置內部部署，或同時部署不同版本的應用程式，並逐漸移轉使用者，您可能無法使用 VIP 交換作業。
- **套用組態變更，而盡可能不回收**執行個體。在許多情況下，您可以變更 Azure 應用程式或服務的組態設定，而不需要重新啟動角色。角色會公開可以處理的事件以偵測組態變更，並將其套用至應用程式內的元件。不過，一些核心平台設定的變更將需要重新啟動角色。建置元件和服務時，藉由將其設計為可接受組態設定的變更，而不需要重新啟動整個應用程式，即可最大化可用性，並將停機情況減至最少。
- **使用升級網域以達成更新期間的零停機情況。** Azure 計算單位 (如 web 和背景工作角色) 會配置給升級網域。升級網域群組角色執行個體，這麼一來，在發生輪流更新時，升級網域中的每個角色都會輪流停止、更新並且重新啟動，才能將對應用程式可用性的影響降至最低。您可以指定部署服務時，應該為服務建立幾個升級網域。

	> [AZURE.NOTE] 角色也分布在容錯網域，每一個容錯網域在伺服器機架、電源和冷卻佈建等方面都是彼此獨立，以將失敗影響所有角色執行個體的機會降至最低。此分佈會自動發生，您無法控制它。

- **設定 Azure 虛擬機器的可用性設定組。** 將兩個或多個虛擬機器放在同一個可用性設定組中，可保證這些虛擬機器不會部署至相同的容錯網域。若要最大化可用性，您應該為系統使用的每個重要虛擬機器建立多個執行個體，並將這些執行個體放在相同的可用性設定組中。如果您正在執行多個具有不同服務用途的虛擬機器，請為每個虛擬機器建立可用性設定組，並將每個虛擬機器的執行個體加入至每個可用性設定組。例如，如果您已經建立個別的虛擬機器，以做為 web 伺服器和報表伺服器，請為 web 伺服器建立一個可用性設定組，並為報表伺服器建立另一個可用性設定組。將 web 伺服器虛擬機器的執行個體加入至 web 伺服器可用性設定組，並將報表伺服器虛擬機器的執行個體加入至報表伺服器可用性設定組。

## 資料管理

- 透過本機和地理備援**利用資料複寫**。Azure 儲存體中的資料會自動複寫來防止基礎結構失敗情況下的遺失，而且可以設定此複寫的幾項因素。例如，資料的唯讀複本可能會在多個地理區域中複寫 (稱為全域讀取存取備援儲存體或 RA-GRS)。請注意，使用 RA-GRS 會產生額外費用，請參閱 Microsoft 網站上的 [Azure 儲存體定價](https://azure.microsoft.com/pricing/details/storage/)頁面以取得詳細資料。
- 盡可能**使用開放式並行存取和最終一致性**。透過鎖定 (封閉式並行存取) 封鎖資源存取權的交易可能會造成效能不佳，並且大幅降低可用性。這些問題在分散式系統中可能會變得特別嚴重。在許多情況下，謹慎的設計和技術 (例如資料分割) 可以將發生更新衝突的機會降至最低。複寫資料或從個別更新存放區中讀取資料時，資料才會最終一致，但其優點通常會超越使用交易來確保立即一致性所造成的可用性影響。
- **使用定期備份和還原時間點**，並確定它符合復原點目標 (RPO)。定期自動備份未保留在其他位置的資料，並確認您可以在失敗發生時，可靠地還原資料和應用程式本身。資料複寫並不是備份功能，因為透過失敗、錯誤或惡意作業導入的錯誤和不一致將會複寫到所有商店。備份程序必須是安全的，才能保護傳輸和儲存體中的資料。資料庫或部分的資料存放區通常可以使用交易紀錄檔復原至先前的時間點。Microsoft Azure 會提供備份設備給儲存在 Microsoft Azure SQL Database 中的資料。資料會匯出至 Microsoft Azure blob 儲存體上的備份封裝，並且可以下載至儲存體的安全內部部署位置。
- **啟用高可用性選項來維護 Redis 快取的次要複本。** 使用 Redis 快取時，請選擇標準選項，以維護內容的次要複本。如需詳細資訊，請參閱 Microsoft 網站上的[在 Azure Redis 快取中建立快取](https://msdn.microsoft.com/library/dn690516.aspx)頁面。

## 錯誤和失敗

- **導入逾時的概念。** 服務和資源可能會變成無法使用，導致要求失敗。請確定您所套用的逾時都適用於每個服務或資源，以及存取服務或資源的用戶端 (在某些情況下，允許較長的逾時以適用於特定用戶端的執行個體會比較適合，視用戶端正在執行的內容和其他動作而定)。非常短的逾時可能會對具有大量延遲的服務及資源造成過多的重試作業，但是如果大量的要求排入佇列等待服務或資源回應，很長的逾時可能會導致封鎖。
- **重試暫時性錯誤所造成的失敗作業。** 設計存取所有服務和資源的重試策略，因為它們原本不支援自動連線重試。使用策略包含在重試之間增加的延遲，做為失敗增加的數目，以避免資源的多載，並允許它適當地復原並處理排入佇列的要求。具有非常短的延遲的持續重試很可能會使問題惡化。
- 遠端服務時無法使用時請**停止傳送要求以避免連鎖失敗**。可能有一些情況中，暫時性或其他錯誤 (嚴重性範圍從部分的連線中斷到服務的完全失敗) 會花費比預期還長的時間才能返回正常狀態。此外，如果服務非常忙碌，系統某部分中的失敗可能導致連鎖失敗，而造成許多作業在保存重要的系統資源 (例如記憶體、執行緒和資料庫連接) 時遭到封鎖。應用程式不會持續重試不太可能成功的作業，而是應該快速接受作業已失敗，且適當地處理這項失敗。斷路器模式可用來拒絕定義期間的特定作業要求。Microsoft 網站上的[斷路器模式](https://msdn.microsoft.com/library/dn589784.aspx)頁面會提供更多詳細資料。
- **撰寫或回到多個元件**以緩和特定服務離線或無法使用的影響。設計應用程式，以盡可能充分利用多個執行個體，而不影響作業和現有的連接。使用多個執行個體並分散它們之間的要求，偵測並避免傳送要求給失敗的執行個體，藉以最大化可用性。
- 盡可能**回到不同的服務或工作流程**。例如，如果寫入 SQL Database 失敗，請暫時將資料儲存在 blob 儲存體，並提供設備在服務可用時將 blob 儲存體中的資料重新寫入 SQL Database。在某些情況下，失敗的作業可能會有替代動作，允許應用程式繼續運作，甚至是當元件或服務失敗時也可繼續運作。盡可能偵測失敗並將要求重新導向至其他可提供適當替代功能的服務，或者在主要服務離線時，備分或減少可維護核心作業的功能執行個體。

## 監視和災害復原

- **為可能的失敗和失敗事件提供豐富的檢測**以將情況回報給操作人員。為可能但尚未發生的失敗提供足夠的資料，以便作業人員判斷其原因，緩和情況，並確保系統仍然可用。對於已經發生的失敗，應用程式應該將適當的錯誤訊息傳回給使用者，雖然功能減少了，但仍嘗試繼續執行。在所有情況下，監視系統應該擷取完整的詳細資料，以便作業人員達成快速復原，並視需要讓設計人員和開發人員修改系統，以防止再次發生這個情況。
- **實作檢查函數來監視系統健全狀況。** 應用程式的健全狀況和效能可能會在不同時間產生不明顯的降級，直到失敗為止。防範此情況的方法之一是實作探查或檢查定期從應用程式外部執行的函數。這些檢查和測量整體應用程式、應用程式的個別部分、應用程式使用的個別服務、和個別元件的回應時間一樣簡單。檢查函數可以執行程序，以確保它們會產生有效的結果、測量延遲及檢查可用性，並從系統中擷取資訊。
- **定期測試所有容錯移轉和後援系統**以確保它們可以使用並如預期般運作。系統與作業的變更可能會影響容錯移轉和後援函數，但是在主要系統失敗或負載過重之前可能都無法偵測到影響。好的作法之一是在執行階段中必須彌補即時問題之前加以測試。
- **測試監視系統。** 自動化的容錯移轉和後援系統，以及使用儀表板手動進行系統健全狀況和效能視覺化，都取決於正常運作的監視和檢測。如果這些項目失敗，遺漏重要資訊或回報不正確的資料，則操作人員可能不知道系統是狀況不良還是失敗。
- **追蹤長時間執行之工作流程的進度**並再試一次失敗。長時間執行的工作流程通常以多個步驟組成。設計這些類型的工作流程時，請確定每個步驟都是獨立且可以重試的，以將整個工作流程都必須回復的機會，或必須執行多個補償交易的機會降到最低。藉由實作模式 (例如排程器代理程式監督員) 監視並管理長時間執行之工作流程的進度。如需詳細資訊，請參閱 Microsoft 網站上的[排程器代理程式監督員模式](https://msdn.microsoft.com/library/dn589780.aspx)頁面。
- **規劃災害復原。** 針對可能導致主要系統全部或部分無法使用的任何類型的失敗，請確定有已記錄、協議和完整測試的計劃。定期測試程序，並確定所有的作業人員熟悉程序。

<!---HONumber=AcomDC_0128_2016-->