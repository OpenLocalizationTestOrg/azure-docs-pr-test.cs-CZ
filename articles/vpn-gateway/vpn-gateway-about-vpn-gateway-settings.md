---
title: "Nastavení brány sítě VPN pro Azure připojení mezi různými místy | Microsoft Docs"
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
ms.openlocfilehash: 07aa6946b9c3994c5afc5c88837f23567b95d8a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a>O nastavení konfigurace brány sítě VPN

Brána sítě VPN je typ brány virtuální sítě, které odesílá šifrovaný provoz mezi virtuální sítí a vaše místní umístění přes připojení veřejné. Brána sítě VPN můžete použít také k posílání provozu mezi virtuálními sítěmi přes páteřní strukturu Azure.

Připojení k bráně VPN závisí na konfiguraci více prostředků, z nichž každý obsahuje konfigurovat nastavení. Části v tomto článku popisují prostředky a nastavení, které se týkají brány VPN pro virtuální síti vytvořené v modelu nasazení Resource Manager. Můžete najít popisy a diagramy topologie pro každé připojení řešení v [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) článku.

## <a name="gwtype"></a>Typy brány

Každá virtuální síť může mít pouze jednu bránu virtuální sítě každého typu. Při vytváření brány virtuální sítě, musí se ujistěte, že je typ brány odpovídá vaší konfiguraci.

Dostupné hodnoty pro - GatewayType jsou:

* Vpn
* ExpressRoute

Brána sítě VPN vyžaduje `-GatewayType` *Vpn*.

Příklad:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>SKU brány

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-the-gateway-sku"></a>Konfigurace skladová položka brány

#### <a name="azure-portal"></a>portál Azure

Pokud používáte portál Azure k vytvoření brány virtuální sítě Resource Manager, můžete vybrat SKU brány pomocí rozevíracího seznamu. Možnosti, které se zobrazí s odpovídají typ brány a typ sítě VPN, který jste vybrali.

#### <a name="powershell"></a>PowerShell

Následující příklad PowerShell Určuje `-GatewaySku` jako VpnGw1.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="resize"></a>Změna (změny velikosti) skladová položka brány

Pokud chcete upgradovat bránu SKU výkonnější SKU, můžete použít `Resize-AzureRmVirtualNetworkGateway` rutiny prostředí PowerShell. Můžete také starší verzi brány velikost SKU použití této rutiny.

Následující příklad PowerShell ukazuje změnu velikosti na VpnGw2 SKU brány.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <a name="connectiontype"></a>Typy připojení

V modelu nasazení Resource Manager Každá konfigurace vyžaduje typ připojení brány konkrétní virtuální sítě. Dostupné hodnoty prostředí PowerShell v Resource Manageru pro `-ConnectionType` jsou:

* Protokol IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

V následujícím příkladu prostředí PowerShell, vytvoříme připojení S2S, která vyžaduje typ připojení *IPsec*.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>Typy sítě VPN

Když vytvoříte bránu virtuální sítě pro konfiguraci brány VPN, musíte zadat typ sítě VPN. Typ sítě VPN, který zvolíte, závisí na topologii připojení, který chcete vytvořit. Například připojení P2S vyžaduje typ sítě VPN RouteBased. Typ sítě VPN můžete také závisí na hardwaru, který používáte. Konfigurace S2S vyžadují zařízení VPN. Některá zařízení VPN podporují pouze určitý typ sítě VPN.

Typ sítě VPN, kterou vyberete, musí splňovat všechny připojení požadavky pro řešení, že které chcete vytvořit. Pokud chcete vytvořit připojení k bráně S2S VPN a připojení k bráně P2S VPN pro stejnou virtuální síť, je třeba, použijte typ sítě VPN *RouteBased* protože P2S vyžaduje typ sítě VPN RouteBased. Musíte také ověřte, že vaše zařízení VPN podporuje připojení k síti VPN RouteBased. 

Po vytvoření brány virtuální sítě, nelze změnit typ sítě VPN. Budete muset odstranit bránu virtuální sítě a vytvořte novou. Existují dva typy sítě VPN:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Následující příklad PowerShell Určuje `-VpnType` jako *RouteBased*. Při vytváření brány se musíte ujistit, že parametr -VpnType odpovídá vaší konfiguraci.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Požadavky na brány

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Podsíť brány

Před vytvořením brány VPN, musíte vytvořit podsíť brány. Podsíť brány obsahuje IP adresy, které používají bránu virtuální sítě virtuálních počítačů a služeb. Když vytvoříte bránu virtuální sítě, virtuální počítače brány jsou nasazené na podsíť brány a nakonfigurovaný pomocí požadovaného nastavení brány sítě VPN. Nikdy jakékoli jiné (například další virtuální počítače) musíte nasadit na podsíť brány. Podsíť brány musí mít název "GatewaySubnet" správně fungovat. Pojmenování podsíť brány "GatewaySubnet" umožňuje vědět, že se jedná o podsítě k nasazení brány virtuální sítě virtuálních počítačů a služeb do Azure.

Při vytváření podsítě brány zadáte počet IP adres, které podsíť obsahuje. IP adresy v podsíti brány mají při přidělování brány virtuální počítače a služby brány. Některé konfigurace vyžadují více IP adres než jiné. Podívejte se na pokyny pro konfiguraci, kterou chcete vytvořit a ověřit, jestli podsíť brány, kterou chcete vytvořit splňuje tyto požadavky. Kromě toho můžete ujistěte, že podsítě brány obsahuje dost IP adres, aby dokázala pojmout možné budoucí další konfigurace. Můžete si sice vytvořit podsíť brány jako malé/29, doporučujeme vytvořit podsíť brány/28 nebo větší (/ 28, / 27, /26 atd.). Tímto způsobem, pokud přidáte funkci v budoucnu nebudete mít k oddělení bránu, pak odstraňte a vytvořte znovu podsíť brány, aby bylo možné víc IP adres.

Následující příklad správce prostředků PowerShell ukazuje podsíť brány s názvem GatewaySubnet. Uvidíte, že zápis CIDR Určuje velikost/27, která umožňuje dost IP adres pro většinu konfigurace, které aktuálně neexistuje.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Brány místní sítě

Při vytváření konfiguraci brány VPN, bránu místní sítě často představuje vaše místní umístění. V modelu nasazení Classic se brána místní sítě označovala jako „místní lokalita“. 

Pojmenujte bránu místní sítě, veřejnou IP adresu místního zařízení VPN a zadáte předpony adres, které se nacházejí na místní umístění. Azure zjistí předpony cílových adres pro síťový provoz, zajímají konfiguraci, která jste zadali pro bránu místní sítě a směruje pakety odpovídajícím způsobem. Je také zadat brány místní sítě pro sítě VNet-to-VNet konfigurace, které používají připojení k bráně VPN.

Následující příklad PowerShell vytvoří novou bránu místní sítě:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

V některých případech budete muset změnit nastavení brány místní sítě. Například když přidáváte nebo odebíráte rozsah adres nebo pokud se IP adresa zařízení VPN změní. Pro klasické virtuální síti můžete změnit tato nastavení na portálu classic na stránce místní sítě. Pro správce prostředků najdete v části [upravit nastavení brány místní sítě pomocí prostředí PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API

Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API, rutiny prostředí PowerShell nebo rozhraní příkazového řádku Azure pro konfigurace brány sítě VPN najdete na následujících stránkách:

| **Classic** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azure#networking) |[PowerShell](/powershell/module/azurerm.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Nepodporuje se | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Další kroky

Další informace o konfiguracích dostupné připojení najdete v tématu [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).