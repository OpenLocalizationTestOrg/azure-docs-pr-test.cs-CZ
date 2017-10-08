---
title: "nastavení brány aaaVPN pro Azure připojení mezi různými místy | Microsoft Docs"
description: "Další informace o nastavení brány sítě VPN pro brány virtuální sítě Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: cherylmc
ms.openlocfilehash: 01229d99fa37e30e00aa00f939f488d631b5593c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a>O nastavení konfigurace brány sítě VPN

Brána sítě VPN je typ brány virtuální sítě, které odesílá šifrovaný provoz mezi virtuální sítí a vaše místní umístění přes připojení veřejné. Můžete také použít přenosem toosend brány VPN mezi virtuálními sítěmi přes hello páteřní síti Azure.

Připojení k bráně VPN je závislá na konfiguraci hello více prostředků, z nichž každý obsahuje konfigurovat nastavení. Hello části v tomto článku popisují hello prostředky a nastavení, které se týkají brány VPN tooa pro virtuální síti vytvořené v modelu nasazení Resource Manager. Můžete najít popisy a diagramy topologie pro každé připojení řešení v hello [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) článku.

## <a name="gwtype"></a>Typy brány

Každá virtuální síť může mít pouze jednu bránu virtuální sítě každého typu. Při vytváření brány virtuální sítě, musí se ujistěte, že typ brány hello je správný pro vaši konfiguraci.

Hello - GatewayType dostupné hodnoty jsou:

* Vpn
* ExpressRoute

Brána sítě VPN vyžaduje hello `-GatewayType` *Vpn*.

Příklad:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>SKU brány

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-hello-gateway-sku"></a>Konfigurace brány hello SKU

#### <a name="azure-portal"></a>portál Azure

Pokud používáte hello Azure portálu toocreate bránu virtuální sítě Resource Manager, můžete vybrat hello skladová položka brány pomocí rozevíracího seznamu hello. Hello možností, které se zobrazí s odpovídají toohello typ brány a typ sítě VPN, který jste vybrali.

#### <a name="powershell"></a>PowerShell

Hello následující příklad PowerShell určuje hello `-GatewaySku` jako VpnGw1.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="resize"></a>Změna (změny velikosti) skladová položka brány

Pokud chcete tooupgrade tooa skladová položka vaší brány výkonnější SKU, můžete použít hello `Resize-AzureRmVirtualNetworkGateway` rutiny prostředí PowerShell. Také může ponížit velikost SKU brány hello použití této rutiny.

Hello následující příklad PowerShell ukazuje, že SKU brány se po změně velikosti tooVpnGw2.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <a name="connectiontype"></a>Typy připojení

V modelu nasazení Resource Manager hello Každá konfigurace vyžaduje typ připojení brány konkrétní virtuální sítě. Hello k dispozici Resource Manager hodnoty prostředí PowerShell pro `-ConnectionType` jsou:

* Protokol IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

V následující příklad PowerShell text hello, vytvoříme připojení S2S, která vyžaduje typ připojení hello *IPsec*.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>Typy sítě VPN

Když vytvoříte hello brány virtuální sítě pro konfiguraci brány VPN, musíte zadat typ sítě VPN. Hello typ sítě VPN, který zvolíte, závisí na topologii hello připojení, které chcete toocreate. Například připojení P2S vyžaduje typ sítě VPN RouteBased. Typ sítě VPN můžete také závisí na hello hardwaru, který používáte. Konfigurace S2S vyžadují zařízení VPN. Některá zařízení VPN podporují pouze určitý typ sítě VPN.

Hello typ sítě VPN, který jste vybrali musí splňovat všechny požadavky připojení hello hello řešení, že chcete toocreate. Například pokud chcete, aby toocreate připojení S2S brány VPN a připojení k bráně P2S VPN pro hello stejné virtuální síti, používáte typ sítě VPN *RouteBased* protože P2S vyžaduje typ sítě VPN RouteBased. Také potřebujete tooverify, že vaše zařízení VPN podporuje připojení k síti VPN RouteBased. 

Po vytvoření brány virtuální sítě, nelze změnit typ sítě VPN hello. Máte toodelete hello brány virtuální sítě a vytvořte novou. Existují dva typy sítě VPN:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Hello následující příklad PowerShell určuje hello `-VpnType` jako *RouteBased*. Při vytváření brány, ujistěte se, že hello - VpnType odpovídá vaší konfiguraci.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Požadavky na brány

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Podsíť brány

Před vytvořením brány VPN, musíte vytvořit podsíť brány. podsíť brány Hello obsahuje hello IP adres tohoto hello brány virtuální sítě virtuálních počítačů a služeb použití. Když vytvoříte bránu virtuální sítě, brána virtuální počítače jsou nasazené toohello podsíť brány a nakonfigurované hello požadované nastavení brány sítě VPN. Je nutné nasadit nikdy cokoli jiného (např. dalších virtuálních počítačů) toohello podsíť brány. Hello podsíť brány musí mít název "GatewaySubnet" toowork správně. Pojmenování podsíť brány hello "GatewaySubnet" umožňuje Azure vědět, že se jedná hello podsíť toodeploy hello brány virtuální sítě virtuálních počítačů a služeb na.

Při vytváření podsítě brány hello určíte, že obsahuje hello počet IP adres, které hello podsítě. Hello IP adresy v podsíti brány hello jsou přidělené toohello brány virtuální počítače a služby brány. Některé konfigurace vyžadují více IP adres než jiné. Podívejte se na hello pokyny pro konfiguraci hello má toocreate a ověřte, že tuto podsíť brány hello chcete toocreate splňuje tyto požadavky. Kromě toho můžete toomake se, že podsítě brány obsahuje dostatek IP adresy tooaccommodate možné budoucí další konfigurace. Můžete si sice vytvořit podsíť brány jako malé/29, doporučujeme vytvořit podsíť brány/28 nebo větší (/ 28, / 27, /26 atd.). Tímto způsobem, pokud přidáte funkci v hello budoucí, nebude máte tootear bránu, pak odstranit a znovu vytvořte tooallow podsíť brány hello více IP adres.

Hello následující příklad správce prostředků PowerShell ukazuje podsíť brány s názvem GatewaySubnet. Uvidíte, že hello notaci CIDR Určuje velikost/27, která umožňuje dost IP adres pro většinu konfigurace, které aktuálně existují.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Brány místní sítě

Při vytváření konfiguraci brány VPN, brána místní sítě hello často představuje vaše místní umístění. V modelu nasazení classic hello hello brány místní sítě se označují tooas místního webu. 

Zadejte název brány místní sítě hello, hello veřejná IP adresa zařízení VPN místní hello a zadejte hello předpony, které se nacházejí na místní umístění hello. Azure zjistí hello předpony cílových adres pro síťový provoz, zajímají hello konfigurace, který jste zadali pro bránu místní sítě a směruje pakety odpovídajícím způsobem. Je také zadat brány místní sítě pro sítě VNet-to-VNet konfigurace, které používají připojení k bráně VPN.

Následující příklad PowerShell Hello vytvoří novou bránu místní sítě:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Někdy je nutné nastavení brány místní sítě toomodify hello. Například když přidat nebo upravit rozsah adres hello nebo pokud se změní hello IP adresa zařízení VPN hello. Pro klasické virtuální síti můžete změnit tato nastavení na portálu classic hello na stránce hello místní sítě. Pro správce prostředků najdete v části [upravit nastavení brány místní sítě pomocí prostředí PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API

Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API, rutiny prostředí PowerShell nebo rozhraní příkazového řádku Azure pro konfigurace brány VPN najdete v tématu hello následující stránky:

| **Classic** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azure#networking) |[PowerShell](/powershell/module/azurerm.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Nepodporuje se | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Další kroky

Další informace o konfiguracích dostupné připojení najdete v tématu [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).