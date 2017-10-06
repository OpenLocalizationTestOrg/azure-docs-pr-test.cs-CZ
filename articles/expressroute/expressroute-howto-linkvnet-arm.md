---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě (virtuální sítě) tooExpressRoute okruhy pomocí modelu nasazení Resource Manager hello a prostředí PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: ganesr
ms.openlocfilehash: e75a9f6b42fa8e1a579e4f19882ec99b277b545f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a>Připojit virtuální síť tooan okruh ExpressRoute
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-linkvnet-classic.md)
>

Tento článek pomáhá propojení virtuální sítě (virtuální sítě) okruhy ExpressRoute tooAzure pomocí modelu nasazení Resource Manager hello a prostředí PowerShell. Virtuální sítě může být buď v hello stejné předplatné nebo součástí jiné předplatné. Tento článek také ukazuje, jak propojit tooupdate virtuální sítě. 

## <a name="before-you-begin"></a>Než začnete
* Nainstalujte nejnovější verzi modulů prostředí Azure PowerShell hello hello. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* Zkontrolujte hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Musí mít aktivní okruh ExpressRoute. 
  * Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít hello ho povolený vaším poskytovatelem připojení. 
  * Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh. V tématu hello [konfigurace směrování](expressroute-howto-routing-arm.md) směrování pokyny najdete v článku. 
  * Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.
  * Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený. Postupujte podle pokynů hello příliš[vytvořit bránu virtuální sítě pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Brána virtuální sítě pro ExpressRoute používá hello GatewayType 'ExpressRoute' není síť VPN.

* Můžete propojit až too10 virtuální sítě tooa standardní okruh ExpressRoute. Všechny virtuální sítě musí být v hello stejné geopolitické oblasti při použití standardní okruh ExpressRoute. 

* Můžete propojit virtuální sítě mimo hello geopolitické oblasti hello okruh ExpressRoute nebo připojit větší počet virtuálních sítí tooyour okruh ExpressRoute, pokud jste povolili hello doplněk ExpressRoute premium. Zkontrolujte hello [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.


## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Připojit virtuální síť v hello stejnému okruhu tooa předplatného
Pomocí následující rutiny hello můžete připojit virtuální síti brány tooan okruh ExpressRoute. Ujistěte se, že hello brány virtuální sítě se vytvoří a je připravený k propojení před spuštěním rutiny hello:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Připojit virtuální síť v okruhu tooa jiném předplatném.
Okruh ExpressRoute můžete sdílet mezi více předplatných. Hello následující obrázek znázorňuje jednoduchý schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.

Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci. Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné pro nasazení můžete sdílet své služby--ale jedné back tooyour místní sítě tooconnect okruhu ExpressRoute. Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute. Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.

> [!NOTE]
> Připojení a šířku pásma poplatky za hello okruh ExpressRoute bude použité toohello vlastník předplatného. Všechny virtuální sítě sdílejí hello stejné šířky pásma.
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a>Správa - vlastníky okruhu a okruh uživatelů

Hello 'vlastníka okruhu, je oprávněný uživatel napájení z hello prostředků okruhu ExpressRoute. vlastníka okruhu Hello můžete vytvořit autorizací, které lze uplatnit podle 'okruh uživatele'. Vlastníci virtuální sítě jsou uživatelé okruh brány, které nejsou uvnitř hello stejného předplatného jako hello okruh ExpressRoute. Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).

vlastníka okruhu Hello má hello power toomodify a odvolat oprávnění kdykoli. Odvolání autorizaci má za následek všechna připojení odkaz odstraňuje z předplatného hello jehož přístup byl odvolán.

### <a name="circuit-owner-operations"></a>Operace vlastníka okruhu

**toocreate povolení**

vlastníka okruhu Hello vytvoří povolení. To vede k vytváření hello autorizační klíč, který může být používán tooconnect uživatele okruhu jejich toohello brány virtuální sítě okruh ExpressRoute. Povolení je platný pro jenom jedno připojení.

následující rutina fragment kódu ukazuje, jak Hello toocreate povolení:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


Hello odpovědi toothis bude obsahovat autorizační klíč hello a stav:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**povolení tooreview**

vlastníka okruhu Hello můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním následující rutiny hello:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**povolení tooadd**

vlastníka okruhu Hello můžete přidat oprávnění pomocí hello následující rutiny:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**povolení toodelete**

vlastníka okruhu Hello můžete odvolání nebo odstranění autorizací toohello uživatele spuštěním následující rutiny hello:

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Operace okruh uživatele

Hello okruh uživatel potřebuje hello ID partnera a autorizační klíč z vlastníka okruhu hello. Hello autorizační klíč je identifikátor GUID.

ID partnera můžete zkontrolovat z hello následující příkaz:

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**tooredeem autorizace připojení**

uživatele okruhu Hello můžete spustit následující rutiny tooredeem hello odkaz autorizace:

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**toorelease autorizace připojení**

Povolení můžete vydat odstraněním hello připojení, který odkazuje hello ExpressRoute okruhu toohello virtuální sítě.

## <a name="modify-a-virtual-network-connection"></a>Upravit připojení virtuální sítě
Můžete aktualizovat některé vlastnosti připojení k virtuální síti. 

**Váha připojení tooupdate hello**

Virtuální sítě může být připojené toomultiple okruhy ExpressRoute. Můžete obdržet hello stejné předpony z více než jeden okruh ExpressRoute. Jaký připojení toosend provoz určený pro tuto předponu toochoose, můžete změnit *RoutingWeight* připojení. Přenosy se odesílají na hello připojení s nejvyšší hello *RoutingWeight*.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

Hello rozsah *RoutingWeight* je 0 too32000. Hello výchozí hodnota je 0.

## <a name="next-steps"></a>Další kroky
Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
