在此步驟中，您會在容錯移轉叢集管理員和 SQL Server Management Studio (SSMS) 中手動建立可用性群組接聽程式。

1. 從裝載主要複本的節點開啟容錯移轉叢集管理員。

1. 選取 [網路] 節點，然後記下叢集網路名稱。這個名稱會用於 PowerShell 指令碼中的 $ClusterNetworkName 變數。

1. 展開叢集名稱，然後按一下 [角色]。

1. 在 [角色] 窗格中以滑鼠右鍵按一下可用性群組名稱，然後選取 [加入資源] > [用戶端存取點]。

	![加入可用性群組的用戶端存取點](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

1. 在 [名稱] 方塊中，建立這個新接聽程式的名稱，然後按兩次 [下一步]，再按一下 [完成]。目前請勿讓接聽程式或資源上線工作。

1. 按一下 [資源] 索引標籤，然後展開您剛才建立的用戶端存取點。您會看到叢集中每個叢集網路的 [IP 位址]資源。如果這是僅限 Azure 的解決方案，您只會看到一個 IP 位址資源。

1. 如果您正在設定混合式解決方案，請繼續執行此步驟。如果您正在設定僅限 Azure 的解決方案，請跳至下一個步驟。
	 - 以滑鼠右鍵按一下對應至您內部部署子網路的 IP 位址資源，然後選取 [屬性]。記下 IP 位址名稱和網路名稱。
	 - 選取 [靜態 IP 位址]、指派未使用的 IP 位址，然後按一下 [確定]。

1. 以滑鼠右鍵按一下對應至您 Azure 子網路的 IP 位址資源，然後選取 [屬性]。 

	>[AZURE.NOTE] 如果接聽程式稍後因為 DHCP 所選取的 IP 位址衝突而無法上線，您可以在此屬性視窗中設定有效的靜態 IP 位址。


1. 在同一個 [IP 位址屬性] 視窗中，變更 [IP 位址名稱]。此 IP 位址名稱將用於 PowerShell 指令碼的 **$IPResourceName** 變數。如果您的方案跨越多個 Azure VNet，請針對每個 IP 資源重複此步驟。

<!----HONumber=Oct15_HO3-->
