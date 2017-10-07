---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: rozhraní příkazového řádku: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě (virtuální sítě) tooExpressRoute okruhy pomocí modelu nasazení Resource Manager hello a rozhraní příkazového řádku."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 1251f016d9b94d3fee81de1df164cb085cbe9d78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-cli"></a>Připojit okruh ExpressRoute tooan virtuální sítě pomocí rozhraní příkazového řádku

Tento článek pomáhá odkaz okruhy ExpressRoute tooAzure virtuální sítě (virtuální sítě) pomocí rozhraní příkazového řádku. použití Azure CLI, toolink hello virtuální sítě musí být vytvořen pomocí modelu nasazení Resource Manager hello. Může být buď v hello stejného předplatného, nebo její část jiné předplatné. Pokud chcete toouse jinou metodu tooconnect vaší virtuální sítě tooan okruh ExpressRoute, můžete vybrat článek ze hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Předpoklady konfigurace

* Je nutné hello nejnovější verzi hello rozhraní příkazového řádku (CLI). Další informace najdete v tématu [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
* Je třeba tooreview hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Musí mít aktivní okruh ExpressRoute. 
  * Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](howto-circuit-cli.md) a mít hello ho povolený vaším poskytovatelem připojení. 
  * Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh. V tématu hello [konfigurace směrování](howto-routing-cli.md) směrování pokyny najdete v článku. 
  * Zkontrolujte, jestli je nakonfigurovaný soukromého partnerského vztahu Azure. Hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem musí být si tak, že povolíte připojení klient server.
  * Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený. Postupujte podle pokynů hello příliš[nakonfigurovat bránu virtuální sítě pro ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli). Být jisti toouse `--gateway-type ExpressRoute`.

* Můžete propojit až too10 virtuální sítě tooa standardní okruh ExpressRoute. Všechny virtuální sítě musí být v hello stejné geopolitické oblasti při použití standardní okruh ExpressRoute. 

* Pokud povolíte hello doplněk ExpressRoute premium, můžete propojení virtuální sítě mimo hello geopolitické oblasti hello okruh ExpressRoute nebo připojit větší počet virtuálních sítí tooyour okruh ExpressRoute. Další informace o rozšíření hello premium najdete v tématu hello [– nejčastější dotazy](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Připojit virtuální síť v hello stejnému okruhu tooa předplatného

Okruh ExpressRoute tooan brány virtuální sítě můžete připojit pomocí příklad hello. Ujistěte se, že hello brány virtuální sítě se vytvoří a je připravený k propojení před spuštěním příkazu hello.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Připojit virtuální síť v okruhu tooa jiném předplatném.

Okruh ExpressRoute můžete sdílet mezi více předplatných. Hello na následujícím obrázku jednoduchou schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.

Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci. Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné pro nasazení můžete sdílet své služby--ale jedné back tooyour místní sítě tooconnect okruhu ExpressRoute. Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute. Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.

> [!NOTE]
> Připojení a šířku pásma poplatky za hello vyhrazené okruhu bude použité toohello vlastníka okruhu ExpressRoute. Všechny virtuální sítě sdílejí hello stejné šířky pásma.
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Správa - vlastníky okruhu a okruh uživatelů

Hello 'vlastníka okruhu, je oprávněný uživatel napájení z hello prostředků okruhu ExpressRoute. Hello vlastníka okruhu můžete vytvořit autorizací, které lze uplatnit podle 'Okruh uživatele'. Vlastníci virtuální sítě jsou uživatelé okruh brány, které nejsou uvnitř hello stejného předplatného jako hello okruh ExpressRoute. Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).

Hello vlastníka okruhu má hello power toomodify a odvolat oprávnění kdykoli. Při povolení odvolán, všechna připojení odkaz se odstraní z předplatného hello jehož přístup byl odvolán.

### <a name="circuit-owner-operations"></a>Operace vlastníka okruhu

**toocreate povolení**

Hello vlastníka okruhu vytvoří autorizaci, které vytvoří autorizační klíč, který může být používán tooconnect uživatele okruhu jejich toohello brány virtuální sítě okruh ExpressRoute. Povolení je platný pro jenom jedno připojení.

Následující příklad ukazuje, jak Hello toocreate povolení:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

Hello odpovědi obsahuje hello autorizační klíč a stav:

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**povolení tooreview**

Hello vlastníka okruhu můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním hello následující ukázka:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**povolení tooadd**

Hello vlastníka okruhu můžete přidat oprávnění pomocí hello následující ukázka:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**povolení toodelete**

Hello vlastníka okruhu můžete odvolání nebo odstranění autorizací toohello uživatele spuštěním hello následující ukázka:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Operace okruh uživatele

Hello uživatele okruhu musí hello ID partnera a autorizační klíč z hello vlastníka okruhu. Hello autorizační klíč je identifikátor GUID.

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**tooredeem autorizace připojení**

Hello uživatele okruhu můžete spustit následující příklad tooredeem hello odkaz autorizace:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**toorelease autorizace připojení**

Povolení můžete vydat odstraněním hello připojení, který odkazuje hello ExpressRoute okruhu toohello virtuální sítě.

## <a name="next-steps"></a>Další kroky

Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
