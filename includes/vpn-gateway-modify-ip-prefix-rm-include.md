如果您需要變更您的本機站台首碼，請使用下列指示。提供兩組指示，並取決於您是否已建立 VPN 閘道連線。

### 新增或移除沒有 VPN 閘道連線的首碼


- 若要**新增**其他位址首碼到您建立的本機站台，但還沒有 VPN 閘道連線，請使用下列範例。

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- 若要從沒有 VPN 連線的本機站台**移除**位址首碼，請使用以下範例。省略您不再需要的首碼。此範例中不再需要首碼 20.0.0.0/24 (來自先前的範例)，因此我們將更新本機站台，並排除該首碼。

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### 新增或移除有 VPN 閘道連線的首碼

如果您已建立 VPN 連線並想要新增或移除包含在本機站台中的 IP 位址首碼，您必須依序執行下列步驟。這會導致 VPN 連線的一些停機時間，因為您需要移除並重新建立閘道器。不過，因為您已要求連線的 IP 位址，所以您不需要重新設定您的內部部署 VPN 路由器，除非您決定要變更您先前使用的值。

 
1. 移除閘道器連線。 
2. 修改本機站台的首碼。 
3. 建立新的閘道器連線。 

您可以使用下列範例當做指導方針。


	$gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

	Remove-AzureRmVirtualNetworkGatewayConnection -Name vnetgw1 -ResourceGroupName testrg

	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
	Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
	
	New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

<!---HONumber=AcomDC_0107_2016-->