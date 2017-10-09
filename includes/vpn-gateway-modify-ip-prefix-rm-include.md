### <a name="noconnection"></a>toomodify místní síťové brány IP předpon adres – žádné připojení brány

tooadd Další předpony adresy:

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

předpony adres tooremove:<br>
Vynechte předpony hello, které už nepotřebujete. V tomto příkladu jsme už nepotřebujeme předponu 20.0.0.0/24 (z hello předchozího příkladu), takže jsme aktualizovat bránu místní sítě hello, s výjimkou tuto předponu.

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <a name="withconnection"></a>toomodify předpon brány místní sítě IP adresy - existující připojení brány

Chcete-li mít připojení brány a tooadd nebo odebrat předpony IP adres hello obsažené v bránu místní sítě, je nutné toodo hello následující kroky v pořadí. Způsobí to určitý výpadek připojení VPN. Úpravy předpon adres IP, nepotřebujete toodelete hello VPN gateway. Potřebujete jenom tooremove hello připojení.


1. Odeberte připojení hello.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Úprava předpon adres hello pro bránu místní sítě.
   
  Hello proměnnou pro hello LocalNetworkGateway nastavit.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Úprava předpon hello.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Vytvořte připojení hello. V tomto příkladu konfigurujeme typ připojení IPsec. Při opětovném vytvoření připojení, použijte typ hello připojení, který je zadán pro vaši konfiguraci. Typy další připojení, najdete v části hello [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) stránky.
   
  Hello proměnnou pro hello VirtualNetworkGateway nastavit.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Vytvořte připojení hello. Tento příklad používá proměnnou hello $local nastavený v kroku 2.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```