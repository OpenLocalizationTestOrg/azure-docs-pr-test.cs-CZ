---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: prostředí PowerShell: classic: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě (virtuální sítě) tooExpressRoute okruhy pomocí modelu nasazení classic hello a prostředí PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.openlocfilehash: 6b8a01dcd4bbb9618ec3dd438cf0107538fb2a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-powershell-classic"></a>Připojit okruh ExpressRoute tooan virtuální sítě pomocí prostředí PowerShell (klasické)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-linkvnet-classic.md)
>

Tento článek vám pomůže propojení virtuální sítě (virtuální sítě) okruhy ExpressRoute tooAzure pomocí modelu nasazení classic hello a prostředí PowerShell. Virtuální sítě může být v hello stejného předplatného nebo můžou být součástí jiné předplatné.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Předpoklady konfigurace
1. Je nutné hello nejnovější verzi modulů prostředí Azure PowerShell hello. Nejnovější moduly Powershellu hello si můžete stáhnout z hello prostředí PowerShell části hello [Azure stáhne stránky](https://azure.microsoft.com/downloads/). Postupujte podle pokynů hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) podrobný návod jak tooconfigure modulů prostředí Azure PowerShell hello toouse vašeho počítače.
2. Je třeba tooreview hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
3. Musí mít aktivní okruh ExpressRoute.
   * Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a váš poskytovatel připojení povolit hello okruh.
   * Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh. V tématu hello [konfigurace směrování](expressroute-howto-routing-classic.md) směrování pokyny najdete v článku.
   * Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.
   * Musí mít virtuální sítě a brány virtuální sítě vytvoří a plně zřízený. Postupujte podle pokynů hello příliš[konfigurace virtuální sítě pro ExpressRoute](expressroute-howto-vnet-portal-classic.md).

Můžete propojit až too10 virtuální sítě tooan okruh ExpressRoute. Všechny virtuální sítě musí být v hello stejné geopolitické oblasti. Můžete propojit větší počet virtuálních sítí tooyour okruh ExpressRoute nebo propojení virtuálních sítí, které jsou v dalších geopolitických oblastí, pokud jste povolili hello doplněk ExpressRoute premium. Zkontrolujte hello [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Připojit virtuální síť v hello stejnému okruhu tooa předplatného
Pomocí následující rutiny hello můžete propojit virtuální sítě tooan okruh ExpressRoute. Ujistěte se, že hello brány virtuální sítě se vytvoří a je připravený k propojení před spuštěním rutiny hello.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Připojit virtuální síť v okruhu tooa jiném předplatném.
Okruh ExpressRoute můžete sdílet mezi více předplatných. Hello následující obrázek znázorňuje jednoduchý schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.

Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci. Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné pro nasazení jejich služeb – ale hello oddělení může sdílet jeden back tooyour místní síť tooconnect okruhu ExpressRoute. Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute. Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.

> [!NOTE]
> Připojení a šířku pásma poplatky za hello vyhrazené okruhu bude použité toohello vlastníka okruhu ExpressRoute. Všechny virtuální sítě sdílejí hello stejné šířky pásma.
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Správa
Hello *vlastníka okruhu* je hello správce nebo spolusprávce předplatného hello, ve které hello ExpressRoute je vytvořen okruh. Hello vlastníka okruhu může autorizovat správci nebo coadministrators další odběry, označují tooas *okruhů uživatelů*, toouse hello vyhrazené okruh, kterou vlastní. Okruh uživatelé, kteří jsou může okruh ExpressRoute organizace hello autorizovaný toouse propojit hello virtuální síť ve své předplatné toohello okruh ExpressRoute po jejich oprávnění.

vlastníka okruhu Hello má hello power toomodify a odvolat oprávnění kdykoli. Odvolání povolení bude výsledkem všechny odkazy odstraňuje z předplatného hello jehož přístup byl odvolán.

### <a name="circuit-owner-operations"></a>Operace vlastníka okruhu

**Vytvoření ověření**

Hello vlastníka okruhu autorizuje hello správci odběr toouse hello zadaný okruh. V následujícím příkladu hello správce hello okruhu hello (Contoso IT) umožňuje hello správce jiné předplatné (Dev-Test) toolink si okruh toohello tootwo virtuální sítě. Správce Contoso IT Hello umožňuje to tak, že zadáte ID hello Microsoft Dev-Test. rutiny Hello neodešle toohello e-mailu zadat ID společnosti Microsoft. vlastníka okruhu Hello musí tooexplicitly oznámit hello jiných vlastník předplatného, které hello autorizace je dokončena.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

**Kontrola povolení**

vlastníka okruhu Hello můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním následující rutiny hello:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


**Aktualizace oprávnění**

vlastníka okruhu Hello můžete upravit autorizací pomocí hello následující rutiny:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**Odstranění autorizací**

vlastníka okruhu Hello můžete odvolání nebo odstranění autorizací toohello uživatele spuštěním následující rutiny hello:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Operace okruh uživatele

**Kontrola povolení**

uživatele okruhu Hello můžete zkontrolovat autorizací pomocí hello následující rutiny:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

**Uplatňuje autorizací propojení**

uživatele okruhu Hello můžete spustit následující rutiny tooredeem hello odkaz autorizace:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

Spusťte tento příkaz v předplatném hello nově propojené hello virtuální sítě:

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a>Další kroky
Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

