---
title: "Připojení vaší místní síti tooan virtuální síť Azure: Site-to-Site VPN: prostředí PowerShell | Microsoft Docs"
description: "Kroky toocreate připojení IPsec z místní sítě tooan virtuální síť Azure přes hello veřejného Internetu. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí PowerShellu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: cb8db1dab3a5488816a7f7e8e63908a4c02f55db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Vytvoření virtuální sítě pomocí připojení VPN Site-to-Site s použitím prostředí PowerShell

Tento článek ukazuje, jak toouse připojení brány sítě VPN prostředí PowerShell toocreate Site-to-Site z místní sítě toohello virtuální sítě. Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Rozhraní příkazového řádku](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Portál Azure Classic](vpn-gateway-site-to-site-create.md)
> 
>


Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit. Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-diagram.png)

## <a name="before"></a>Než začnete

Ověřte, že jste splnili hello před zahájením konfigurace následující kritéria:

* Zajistěte, aby byla kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho. Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).
* Ověřte, že máte veřejnou IPv4 adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Pokud jste obeznámeni s rozsahy IP adres hello umístěný v konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy. Když vytváříte tuto konfiguraci, musíte zadat hello IP adresa rozsahu předpony, Azure bude směrovat tooyour místní umístění. Žádná z podsítí hello vaší místní sítě můžete prostřednictvím okruhu s hello podsítě virtuální sítě, které chcete tooconnect k.
* Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello. Rutiny prostředí PowerShell jsou často aktualizuje a je obvykle nutné tooupdate vaše prostředí PowerShell rutiny tooget hello nejnovější funkce funkce. Pokud nechcete aktualizovat vaše rutiny prostředí PowerShell, hello hodnoty zadané se pravděpodobně nezdaří. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o stažení a instalaci rutin prostředí PowerShell.

### <a name="example"></a>Příklady hodnot

Hello příklady v tomto článku používají hello následující hodnoty. Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.

```
#Example values

VnetName                = TestVNet1
ResourceGroup           = TestRG1
Location                = East US 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.1.0/28 
GatewaySubnet           = 10.11.0.0/27
LocalNetworkGatewayName = Site2
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24, 20.0.0.0/24
Gateway Name            = VNet1GW
PublicIP                = VNet1GWIP
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2

```


## <a name="Login"></a>1. Připojení tooyour odběru

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="VNet"></a>2. Vytvoření virtuální sítě a podsítě brány

Pokud ještě nemáte virtuální síť, vytvořte si ji. Při vytváření virtuální sítě, ujistěte se, že hello adresní prostory, které zadáte nepřekrývají s hello adresní prostory, které máte ve vaší místní síti.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [No NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="vnet"></a>toocreate virtuální síť a podsíť brány

Tento příklad vytvoří virtuální síť a podsíť brány. Pokud již máte virtuální síť, která potřebujete tooadd podsíť brány k, najdete v části [tooadd brány podsítě tooa virtuální síť jste již vytvořili](#gatewaysubnet).

Vytvořte skupinu prostředků:

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location 'East US'
```

Vytvořte virtuální síť.

1. Nastavení proměnných hello.

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix 10.11.1.0/28
  ```
2. Vytvořte hello virtuální sítě.

  ```powershell
  New-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1 `
  -Location 'East US' -AddressPrefix 10.11.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <a name="gatewaysubnet"></a>tooadd brány podsítě tooa virtuální sítě, které jste už vytvořili

1. Nastavení proměnných hello.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name TestVet1
  ```
2. Vytvořte podsíť brány hello.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27 -VirtualNetwork $vnet
  ```
3. Nastavte konfiguraci hello.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## 3. <a name="localnet"></a>Vytvoření brány místní sítě hello

Hello brány místní sítě obvykle odkazuje tooyour místní umístění. Zadejte název, pomocí kterého můžete Azure odkazovat tooit a pak zadejte IP adresu hello hello lokality z hello místní VPN zařízení toowhich se vytvoří připojení. Můžete také určit hello předpony IP adres, které budou směrovány prostřednictvím zařízení VPN toohello brány sítě VPN hello. Hello předpony, které zadáte předpony hello umístěn ve vaší místní síti. Pokud se změní na místní síti, můžete snadno aktualizovat hello předpony.

Použijte hello následující hodnoty:

* Hello *GatewayIPAddress* je hello IP adresa vašeho místního zařízení VPN. Zařízení VPN nesmí být umístěné za překladem adres (NAT).
* Hello *AddressPrefix* je místní adresní prostor.

tooadd bránu místní sítě s jednou předponou adresy:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.0.0.0/24'
  ```

tooadd bránu místní sítě s více předponami adresy:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

předpony toomodify IP adresy pro bránu místní sítě:<br>
Někdy dojde ke změně předpon brány místní sítě. kroky Hello si toomodify vaší IP adresu, kterou předpony závisí na tom, jestli jste vytvořili připojení brány sítě VPN. V tématu hello [úprava předpon IP adresy pro bránu místní sítě](#modify) tohoto článku.

## <a name="PublicIP"></a>4. Vyžádání veřejné IP adresy

Brána VPN musí mít veřejnou IP adresu. Nejprve požádali o prostředek hello IP adresy a pak odkazovat tooit při vytváření brány virtuální sítě. Hello je přiřazená IP adresa dynamicky toohello prostředků při vytváření brány VPN hello. Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy. Nemůžete si vyžádat statické přiřazení IP adresy. Však neznamená to, že hello IP adresa změní po byl přiřazen tooyour brány VPN. Hello jenom jednou hello změny veřejné IP adresy je při hello brány je odstraní a znovu vytvoří. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.

Požádat o veřejnou IP adresu, která bude přiřazena tooyour virtuální sítě VPN gateway.

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. Vytvoření konfigurace adresování IP brány hello

Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu. Použijte následující příklad toocreate hello vlastní konfiguraci brány:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. Vytvoření brány VPN hello

Vytvořte bránu VPN virtuální sítě hello. Vytvoření brány VPN může trvat až minut too45 nebo další toocomplete.

Použijte hello následující hodnoty:

* Hello *- GatewayType* pro Site-to-Site je konfigurace *Vpn*. Typ brány Hello je vždy konkrétní toohello konfigurace, kterou implementujete. Například jiné konfigurace brány mohou vyžadovat jako -GatewayType hodnotu ExpressRoute.
* Hello *- VpnType* může být *RouteBased* (označované tooas dynamickou bránu v některé dokumentaci), nebo *PolicyBased* (označované tooas statická brána v některé dokumentaci ). Další informace o typech brány VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).
* Vyberte hello skladová položka brány, které chcete toouse. Pro určité skladové jednotky (SKU) platí omezení konfigurace. Další informace najdete v části [Skladové jednotky (SKU) brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku). Pokud dojde k chybě při vytváření brány VPN hello týkající se hello - GatewaySku, ověřte, že jste nainstalovali nejnovější verzi rutin prostředí PowerShell hello hello.

```powershell
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="ConfigureVPNDevice"></a>7. Konfigurace zařízení VPN

Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN. V tomto kroku nakonfigurujete zařízení VPN. Při konfiguraci zařízení VPN, je třeba hello následující:

- Sdílený klíč. To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN. V našich ukázkách používáme základní sdílený klíč. Doporučujeme, abyste vygenerování složitější klíče toouse.
- Hello veřejnou IP adresu brány virtuální sítě. Hello veřejnou IP adresu můžete zobrazit pomocí hello portálu Azure, PowerShell nebo rozhraní příkazového řádku. toofind hello veřejnou IP adresu brány virtuální sítě pomocí prostředí PowerShell, použijte hello následující ukázka:

  ```powershell
  Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG1
  ```

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>8. Vytvoření připojení VPN hello

Dále vytvořte připojení Site-to-Site VPN hello mezi bránou virtuální sítě a zařízením VPN. Být jisti tooreplace hello hodnoty vlastními. Hello sdílený klíč musí odpovídat hello hodnotu, která jste použili pro konfiguraci zařízení VPN. Všimněte si, že hello '-ConnectionType' pro Site-to-Site je *IPsec*.

1. Nastavení proměnných hello.
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  $local = Get-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1
  ```

2. Vytvořte připojení hello.
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite2 -ResourceGroupName TestRG1 `
  -Location 'East US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

Za malou chvíli hello bude vytvořeno připojení.

## <a name="toverify"></a>9. Ověření připojení VPN hello

Existuje několik různých způsobů tooverify připojení k síti VPN.

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="connectVM"></a>tooconnect tooa virtuálního počítače

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <a name="modify"></a>Úprava předpon IP adres pro bránu místní sítě

Pokud hello předpony IP adres, které chcete směrovat tooyour místní umístění změnit, můžete upravit hello brány místní sítě. K dispozici jsou dvě sady pokynů. Hello pokyny, které zvolíte, závisí na tom, zda jste již vytvořili připojení brány.

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Upravit IP adresu brány hello pro bránu místní sítě

[!INCLUDE [Modify gateway IP address](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Další kroky

*  Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
