### <a name="gwipnoconnection"></a>Brána místní sítě hello toomodify GatewayIpAddress - žádné připojení brány

Pokud hello zařízení VPN, které chcete tooconnect toohas změnit jeho veřejnou IP adresu, musíte toomodify hello místní síťové brány tooreflect, který změnit. Použijte hello příklad toomodify bránu místní sítě, která nemá připojení brány.

Při úpravě tuto hodnotu, můžete také upravit předpony adres hello v hello stejnou dobu. Zda toouse hello stávající název brány místní sítě se v pořadí toooverwrite hello aktuální nastavení. Pokud použijete jiný název, můžete vytvořit novou bránu místní sítě, místo přepsání hello existující.

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>Brána místní sítě hello toomodify GatewayIpAddress - existující připojení brány

Pokud hello zařízení VPN, které chcete tooconnect toohas změnit jeho veřejnou IP adresu, musíte toomodify hello místní síťové brány tooreflect, který změnit. Pokud připojení brány již existuje, je nutné nejprve tooremove hello připojení. Po odebrání hello připojení můžete upravit IP adresu brány hello a znovu vytvořte nové připojení. Můžete také upravit předpony adres hello v hello stejnou dobu. Způsobí to určitý výpadek připojení VPN. Při úpravě IP adresu brány hello, nepotřebujete toodelete hello VPN gateway. Potřebujete jenom tooremove hello připojení.
 

1. Odeberte připojení hello. Název hello připojení můžete najít pomocí rutiny hello 'Get-AzureRmVirtualNetworkGatewayConnection'.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Změníte hodnotu 'GatewayIpAddress' hello. Můžete také upravit předpony adres hello v hello stejnou dobu. Být jisti toouse hello stávající název vaší místní brány toooverwrite hello aktuální nastavení sítě. Pokud to neuděláte, můžete vytvořit novou bránu místní sítě, místo přepsání hello existující.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Vytvořte připojení hello. V tomto příkladu konfigurujeme typ připojení IPsec. Při opětovném vytvoření připojení, použijte typ hello připojení, který je zadán pro vaši konfiguraci. Typy další připojení, najdete v části hello [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) stránky.  Název VirtualNetworkGateway hello tooobtain, můžete spustit rutinu "Get-AzureRmVirtualNetworkGateway" hello.
   
    Nastavení proměnných hello.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Vytvořte připojení hello.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```