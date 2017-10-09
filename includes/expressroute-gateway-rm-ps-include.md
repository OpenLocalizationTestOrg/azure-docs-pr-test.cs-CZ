Hello kroky pro tuto úlohu použijte virtuální sítě na základě hello hodnot v hello následující referenční seznam konfigurace. Názvy a další nastavení jsou také popsány v tomto seznamu. Tento seznam nepoužívá přímo v žádném z kroků hello, i když přidáme proměnné založené na hodnotách hello v tomto seznamu. Toouse hello seznamu můžete kopírovat jako odkaz, nahraďte hello hodnoty vlastními.

**Seznam odkazů konfigurace**

* Název virtuální sítě = "TestVNet"
* Virtuální adresní prostor sítě = 192.168.0.0/16
* Skupina prostředků = "TestRG"
* Subnet1 Name = "FrontEnd" 
* Adresní prostor Subnet1 = "192.168.1.0/24"
* Název podsítě brány: "GatewaySubnet" název musí být vždy podsíť brány *GatewaySubnet*.
* Adresního prostoru podsítě brány = "192.168.200.0/26"
* Oblast = "East US"
* Název brány = "GW"
* Název brány IP = "GWIP"
* Konfigurace IP brány Name = "gwipconf"
* Typ = "ExpressRoute" Tento typ je požadovaná konfigurace ExpressRoute.
* Název veřejné IP adresy brány = "gwpip"

## <a name="add-a-gateway"></a>Přidat bránu
1. Připojte tooyour předplatné Azure.

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Deklarace proměnných pro toto cvičení. Mít jistotu tooedit hello ukázka tooreflect hello nastavení, které chcete toouse.

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. Uložit jako proměnnou hello objekt virtuální sítě.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. Přidáte tooyour podsíť brány virtuální sítě. Hello podsíť brány musí mít název "GatewaySubnet". Měli byste vytvořit podsíť brány, který je/27 nebo větší (/ 26, / 25 atd.).

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. Nastavte konfiguraci hello.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Uložit jako proměnnou a podsíť brány hello.

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. Vyžádejte si veřejnou IP adresu. Před vytvořením brány hello je požadována Hello IP adresa. Nelze zadat hello IP adresu, které chcete toouse; se přidělí dynamicky. Tuto IP adresu budete používat v další části Konfigurace hello. Hello AllocationMethod musí být dynamické.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. Vytvoření hello konfigurace pro bránu. Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu. V tomto kroku určíte hello konfigurace, který se použije při vytváření brány hello. Tento krok není ve skutečnosti vytvoření objektu gateway hello. Ukázka hello níže toocreate používejte vlastní konfiguraci brány.

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. Vytvoření brány hello. V tomto kroku hello **- GatewayType** je obzvláště důležité. Je nutné použít hodnotu hello **ExpressRoute**. Po spuštění těchto rutin, může trvat hello brány 45 minut nebo další toocreate.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-hello-gateway-was-created"></a>Ověřte, zda byl vytvořen hello brány
Použijte následující příkazy, které vytvořil tooverify, který hello brány hello:

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Změnit velikost brány
Existuje řada [SKU brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Můžete použít následující příkaz toochange hello skladová položka brány kdykoli hello.

> [!IMPORTANT]
> Tento příkaz nefunguje pro UltraPerformance bránu. toochange vaší brány tooan UltraPerformance bránu, nejprve odeberte hello existující bránu ExpressRoute a poté vytvořit novou bránu UltraPerformance. toodowngrade bránu z bránu UltraPerformance nejprve odeberte hello UltraPerformance brány a poté vytvořit novou bránu.
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Odebrat bránu
Použijte následující příkaz tooremove bránu hello:

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```