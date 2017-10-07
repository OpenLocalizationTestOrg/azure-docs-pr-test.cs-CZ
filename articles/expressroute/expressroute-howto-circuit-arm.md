---
title: "Vytvoření a úprava okruhu ExpressRoute: prostředí PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate, zřizovat, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8d76c577a9cffdd393abac1b76cccc27d92e9e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Vytvoření a úprava okruhu ExpressRoute pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-circuit-classic.md)
>

Tento článek popisuje, jak okruhu Azure ExpressRoute toocreate pomocí modelu nasazení Azure Resource Manager prostředí PowerShell pro rutiny a hello. Tento článek také ukazuje, jak stav hello toocheck hello okruh, aktualizovat, nebo odstranit a zrušit jejich zřízení se.

## <a name="before-you-begin"></a>Než začnete
* Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello. Další informace najdete v tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview).
* Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.


## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvořit a zřídit okruhu ExpressRoute
### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a>1. Přihlaste se tooyour účet Azure a vybrat své předplatné
toobegin konfiguraci přihlášení tooyour účet Azure. Následující příklady toohelp, ke kterým se připojujete pomocí hello:

```powershell
Login-AzureRmAccount
```

Zkontrolujte předplatná hello pro účet hello:

```powershell
Get-AzureRmSubscription
```

Vyberte hello předplatné, které chcete toocreate pro okruh ExpressRoute:

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>2. Získat hello seznam podporovaných zprostředkovatelů, umístění a šířek pásma
Než vytvoříte okruh ExpressRoute, musíte hello seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma.

rutiny prostředí PowerShell text Hello **Get-AzureRmExpressRouteServiceProvider** vrátí tyto informace, které budete používat v dalších krocích:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

Pokud poskytovatel připojení se nezobrazí, zkontrolujte toosee. Poznamenejte si hello následující informace. Budete je potřebovat později při vytvoření okruhu.

* Name (Název)
* PeeringLocations
* BandwidthsOffered

Nyní jste připravené toocreate okruhu ExpressRoute.   

### <a name="3-create-an-expressroute-circuit"></a>3. Vytvoření okruhu ExpressRoute
Pokud ještě nemáte skupinu prostředků, je musíte vytvořit před vytvořením váš okruh ExpressRoute. Můžete tak učinit spuštěním hello následující příkaz:

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


Hello následující příklad ukazuje, jak toocreate 200 MB/s ExpressRoute okruhu prostřednictvím Equinix ze Silicon Valley. Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost. Následující Hello je požadavek příklad pro nový klíč služby:

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Ujistěte se, že jste zadali správné úrovně SKU hello a řada SKU:

* Úroveň SKU Určuje, zda je povolen ExpressRoute standard nebo doplněk ExpressRoute premium. Můžete zadat *standardní* tooget hello standardní SKU nebo *Premium* pro doplněk premium hello.
* Řada SKU Určuje typ fakturace hello. Můžete zadat *Metereddata* pro plán měření podle objemu dat a *Unlimiteddata* pro plán neomezená data na úrovni. Můžete změnit hello fakturace typu z *Metereddata* příliš*Unlimiteddata*, ale nemůžete změnit typ hello z *Unlimiteddata* příliš*Metereddata* .

> [!IMPORTANT]
> Váš okruh ExpressRoute bude účtován z hello okamžiku, kdy se objeví klíč služby. Ujistěte se, když hello poskytovatele připojení je připraven tooprovision hello okruh provedení této operace.
> 
> 

Hello odpovědi obsahuje klíč služby hello. Podrobný popis všech parametrů hello můžete získat spuštěním hello následující příkaz:

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. Zobrazí seznam všech okruhy ExpressRoute
seznam všech hello okruhy ExpressRoute, které jste vytvořili, tooget spustit hello **Get-AzureRmExpressRouteCircuit** příkaz:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

Hello odpověď bude vypadat podobně jako toohello následující ukázka:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Tyto informace kdykoli můžete načíst pomocí hello `Get-AzureRmExpressRouteCircuit` rutiny. Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello. Zobrazí se klíč služby v hello *klíč ServiceKey* pole:

```powershell
Get-AzureRmExpressRouteCircuit
```


Hello odpověď bude vypadat podobně jako toohello následující ukázka:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Podrobný popis všech parametrů hello můžete získat spuštěním hello následující příkaz:

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>5. Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování
*ServiceProviderProvisioningState* poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb. Stav poskytuje hello stavu na straně Microsoft hello. Další informace o zřizování stavy okruhu najdete v tématu hello [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.

Když vytvoříte nový okruh ExpressRoute, bude mít okruh hello hello následující stav:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



okruh Hello změní toohello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Pro jste toobe možné toouse okruh ExpressRoute musí být v hello následující stav:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>6. Pravidelně kontrolovat hello stav a stav hello hello okruh klíče
Kontrola stavu hello a stavu hello hello okruh klíče umožňuje vědět, pokud se váš poskytovatel povolil váš okruh. Po nakonfiguroval hello okruh *ServiceProviderProvisioningState* se zobrazí jako *zajištěno*, jak ukazuje následující příklad hello:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Hello odpověď bude vypadat podobně jako toohello následující ukázka:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Vytvořte vlastní konfiguraci směrování
Podrobné pokyny najdete v tématu hello [konfigurace směrování pro okruh ExpressRoute](expressroute-howto-routing-arm.md) článek toocreate a upravit partnerských vztahů pro okruh.

> [!IMPORTANT]
> Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a>8. Propojení virtuální sítě tooan okruh ExpressRoute
V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute. Použití hello [propojení virtuální sítě okruhů tooExpressRoute](expressroute-howto-linkvnet-arm.md) článek při práci s modelem nasazení Resource Manager hello.

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Získávání hello stav okruhu ExpressRoute
Tyto informace kdykoli můžete načíst pomocí hello **Get-AzureRmExpressRouteCircuit** rutiny. Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello.

```powershell
Get-AzureRmExpressRouteCircuit
```


odpověď Hello bude podobné toohello následující ukázka:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Informace o konkrétní okruh ExpressRoute můžete získat pomocí předání hello název skupiny prostředků a název okruh jako parametr toohello volání:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Hello odpověď bude vypadat podobně jako toohello následující ukázka:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Podrobný popis všech parametrů hello můžete získat spuštěním hello následující příkaz:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify"></a>Úprava okruhu ExpressRoute
Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.

Můžete provést následující bez výpadků hello:

* Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.
* Zvýšení hello šířka pásma okruhu ExpressRoute, pokud na portu hello je dostupné kapacity. Přechod na starší verzi hello šířka pásma okruhu není podporována. 
* Změnit plán z tooUnlimited dat – měření podle objemu dat měření hello. Změna hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.
* Můžete povolit nebo zakázat *povolit klasické operace*.

Další informace o omezení a omezení, najdete v části toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

### <a name="tooenable-hello-expressroute-premium-add-on"></a>doplněk ExpressRoute premium tooenable hello
Doplněk ExpressRoute premium hello u existujícího okruhu můžete povolit pomocí hello následující fragment kódu prostředí PowerShell:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Hello okruhu bude mít teď hello ExpressRoute premium rozšíření funkce povolené. Začneme hned, jak byl úspěšně spuštěn příkaz hello fakturace pro funkci rozšíření hello premium.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>doplněk ExpressRoute premium toodisable hello
> [!IMPORTANT]
> Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.
> 
> 

Vezměte na vědomí následující hello:

* Můžete se downgradovat z úrovně premium toostandard, je nutné zajistit hello počet virtuálních sítí, které jsou propojeny toohello okruh je menší než 10. Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a jsme vám bude účtovat za zvýhodněné sazby.
* Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti. Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a jsme vám bude účtovat za zvýhodněné sazby.
* Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah. Pokud vaše velikost tabulky trasy je větší než 4 000 tras, relace BGP hello zahodí a nebude možné opětovně povolena dokud hello počet předpon inzerovaných přejde nižší než 4 000.

Doplněk ExpressRoute premium hello u existujícího okruhu hello můžete zakázat pomocí hello následující rutiny prostředí PowerShell:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>šířku pásma okruhu ExpressRoute tooupdate hello
Pro možnosti podporované šířky pásma pro zprostředkovatele, zkontrolujte hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md). Můžete vybrat libovolnou velikost, která je větší než velikost hello existujícím okruhem.

> [!IMPORTANT]
> Pokud je na existující port hello nedostatečné kapacity, mohou mít okruh ExpressRoute toorecreate hello. Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.
>
> Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení. Přechod na starší verzi šířky pásma vyžaduje, abyste okruh ExpressRoute hello toodeprovision a pak znova nezajistíte nové okruh ExpressRoute.
> 

Až se rozhodnete, jakou velikost, budete potřebovat, použijte následující příkaz tooresize hello váš okruh:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Váš okruh bude mít velikost na straně Microsoft hello. Pak bude nutné kontaktovat vaše připojení poskytovatele tooupdate konfigurace na jejich straně toomatch tuto změnu. Když provedete toto oznámení, začneme fakturace můžete pro možnost hello aktualizovat šířky pásma.

### <a name="toomove-hello-sku-from-metered-toounlimited"></a>toomove hello SKU z monitorovaných toounlimited
Hello SKU okruhu ExpressRoute můžete změnit pomocí hello následující fragment kódu prostředí PowerShell:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a>toocontrol přístup toohello classic a Resource Manager prostředí
Přečtěte si pokyny hello v [okruhy ExpressRoute přesunout z modelu nasazení Resource Manager classic toohello hello](expressroute-howto-move-arm.md).  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zřízení a odstraňování okruhu ExpressRoute
Vezměte na vědomí následující hello:

* Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute. Pokud se tato operace nezdaří, zkontrolujte, že toosee, pokud žádné virtuální sítě propojené toohello okruh.
* Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno** na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby. Jsme bude i nadále tooreserve prostředky a dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nám vám účtovat.
* Pokud má poskytovatel služeb hello zrušit okruh hello (Stav zřizování poskytovatele služeb hello je nastaven příliš**není zajišťováno**) pak můžete odstranit okruh hello. Tato akce ukončí fakturace pro okruh hello

Váš okruh ExpressRoute můžete odstranit spuštěním hello následující příkaz:

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Další kroky

Po vytvoření okruhu, ujistěte se, že hello následující:

* [Vytvoření a úprava směrování pro okruhu ExpressRoute](expressroute-howto-routing-arm.md)
* [Propojení vaší virtuální sítě tooyour okruh ExpressRoute](expressroute-howto-linkvnet-arm.md)
