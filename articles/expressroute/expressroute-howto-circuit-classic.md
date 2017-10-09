---
title: "Vytvoření a úprava okruhu ExpressRoute: prostředí PowerShell: portál Azure classic | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování okruhu ExpressRoute. Tento článek také ukazuje, jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení váš okruh."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 9897c88776a2153ba22aa9ff328becb9f12b660b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Vytvoření a úprava okruhu ExpressRoute pomocí prostředí PowerShell (klasické)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-circuit-classic.md)
>

Tento článek vás provede kroky toocreate hello okruh Azure ExpressRoute pomocí modelu nasazení classic rutiny a hello prostředí PowerShell. Tento článek také ukazuje, jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení okruhu ExpressRoute.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Než začnete
### <a name="step-1-review-hello-prerequisites-and-workflow-articles"></a>Krok 1. Hello požadavky a pracovní postup články
Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.  

### <a name="step-2-install-hello-latest-versions-of-hello-azure-service-management-sm-powershell-modules"></a>Krok 2. Nainstalujte nejnovější verzi modulů prostředí PowerShell Azure Service Management (SM) hello hello
Postupujte podle pokynů hello v [Začínáme s rutinami prostředí Azure PowerShell](/powershell/azure/overview) podrobný návod jak tooconfigure modulů prostředí Azure PowerShell hello toouse vašeho počítače.

### <a name="step-3-log-in-tooyour-azure-account-and-select-a-subscription"></a>Krok 3. Přihlaste se tooyour účet Azure a vybrat odběr
1. Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu. Použijte následující příklad toohelp, ke kterým se připojujete hello:

        Login-AzureRmAccount

2. Zkontrolujte předplatná hello pro účet hello.

        Get-AzureRmSubscription

3. Pokud máte více než jedno předplatné, vyberte hello předplatné, které chcete toouse.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Pak pomocí následující rutiny tooadd hello tooPowerShell vaše předplatné Azure pro model nasazení classic hello.

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvořit a zřídit okruhu ExpressRoute
### <a name="step-1-import-hello-powershell-modules-for-expressroute"></a>Krok 1. Naimportujte hello moduly Powershellu pro ExpressRoute
 Pokud jste tak již neučinili, je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello. Importovat hello moduly z hello umístění, které byly nainstalované tooon místního počítače. V závislosti na metodě hello používá tooinstall hello moduly, může být jiný než hello následující příklad ukazuje umístění hello. Příklad hello podle potřeby upravte.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>Krok 2. Získat hello seznam podporovaných zprostředkovatelů, umístění a šířek pásma
Než vytvoříte okruh ExpressRoute, musíte hello seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma.

rutiny prostředí PowerShell text Hello `Get-AzureDedicatedCircuitServiceProvider` vrátí tyto informace, které budete používat v dalších krocích:

    Get-AzureDedicatedCircuitServiceProvider

Pokud poskytovatel připojení se nezobrazí, zkontrolujte toosee. Poznamenejte si následující informace, protože budete je potřebovat později při vytvoření okruhu hello:

* Name (Název)
* PeeringLocations
* BandwidthsOffered

Nyní jste připravené toocreate okruhu ExpressRoute.         

### <a name="step-3-create-an-expressroute-circuit"></a>Krok 3. Vytvoření okruhu ExpressRoute
Hello následující příklad ukazuje, jak toocreate 200 MB/s ExpressRoute okruhu prostřednictvím Equinix ze Silicon Valley. Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost.

> [!IMPORTANT]
> Váš okruh ExpressRoute bude účtován z hello okamžiku, kdy se objeví klíč služby. Ujistěte se, když hello poskytovatele připojení je připraven tooprovision hello okruh provedení této operace.
> 
> 

Následující Hello je požadavek příklad pro nový klíč služby:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Nebo, pokud chcete toocreate okruh ExpressRoute s doplňkem hello premium, hello použijte následující příklad. Odkazovat toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Hello odpověď bude obsahovat klíč služby hello. Podrobný popis všech parametrů hello můžete získat spuštěním hello následující:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-hello-expressroute-circuits"></a>Krok 4. Zobrazí seznam všech okruhy ExpressRoute hello
Můžete spustit hello `Get-AzureDedicatedCircuit` příkaz tooget seznam všech hello okruhy ExpressRoute, které jste vytvořili:

    Get-AzureDedicatedCircuit

odpověď Hello bude něco podobné toohello následující ukázka:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Tyto informace kdykoli můžete načíst pomocí hello `Get-AzureDedicatedCircuit` rutiny. Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello. Zobrazí se klíč služby v hello *klíč ServiceKey* pole.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Podrobný popis všech parametrů hello můžete získat spuštěním hello následující:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>Krok 5. Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování
*ServiceProviderProvisioningState* poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb. *Stav* poskytuje hello stavu na straně Microsoft hello. Další informace o zřizování stavy okruhu najdete v tématu hello [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.

Když vytvoříte nový okruh ExpressRoute, bude mít okruh hello hello následující stav:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


okruh Hello přejde toohello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Okruh ExpressRoute musí být v následujícím stavu můžete toobe možné toouse hello ho:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>Krok 6. Pravidelně kontrolovat hello stav a stav hello hello okruh klíče
Tímto způsobem zjistíte, když má poskytovatel povoleno váš okruh. Po nakonfiguroval hello okruh *ServiceProviderProvisioningState* se zobrazí jako *zajištěno* jak ukazuje následující příklad hello:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>Krok 7. Vytvořte vlastní konfiguraci směrování
Odkazovat toohello [konfigurace směrování pro okruh ExpressRoute (vytvoření a úprava okruhu partnerských vztahů)](expressroute-howto-routing-classic.md) podrobné pokyny najdete v článku.

> [!IMPORTANT]
> Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.
> 
> 

### <a name="step-8-link-a-virtual-network-tooan-expressroute-circuit"></a>Krok 8. Propojení virtuální sítě tooan okruh ExpressRoute
V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute. Odkazovat příliš[okruhy ExpressRoute propojení sítí toovirtual](expressroute-howto-linkvnet-classic.md) podrobné pokyny. Pokud potřebujete toocreate virtuální sítě pomocí modelu nasazení classic hello pro ExpressRoute, přečtěte si [vytvoření virtuální sítě pro ExpressRoute](expressroute-howto-vnet-portal-classic.md).

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Získávání hello stav okruhu ExpressRoute
Tyto informace kdykoli můžete načíst pomocí hello `Get-AzureCircuit` rutiny. Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Informace o konkrétní okruh ExpressRoute můžete získat pomocí předání klíče služby hello jako parametr toohello volání.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Podrobný popis všech parametrů hello můžete získat spuštěním hello následující ukázka:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Úprava okruhu ExpressRoute
Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.

Můžete provést následující bez výpadků hello:

* Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.
* Zvýšení hello šířka pásma okruhu ExpressRoute, pokud na portu hello je dostupné kapacity. Všimněte si, že hello šířka pásma okruhu přechod na starší verzi není podporován. 
* Změnit plán z tooUnlimited dat – měření podle objemu dat měření hello. Poznámka: Změna hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.
* Můžete povolit nebo zakázat *povolit klasické operace*.

Odkazovat toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) Další informace o omezení.

### <a name="tooenable-hello-expressroute-premium-add-on"></a>doplněk ExpressRoute premium tooenable hello
Doplněk ExpressRoute premium hello pro váš okruh. existující můžete povolit pomocí následující rutiny prostředí PowerShell hello:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Váš okruh bude mít teď hello ExpressRoute premium rozšíření funkce povolené. Všimněte si, že jsme spustí hned, jak byl úspěšně spuštěn příkaz hello fakturace pro funkci rozšíření hello premium.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>doplněk ExpressRoute premium toodisable hello
> [!IMPORTANT]
> Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.
> 
> 

#### <a name="considerations"></a>Požadavky

* Je nutné zajistit, že hello počet virtuální sítě propojené toohello okruh je menší než 10 před downgradovat z úrovně premium toostandard. Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete fakturovaná hello zvýhodněné sazby.
* Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti. Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete fakturovaná hello zvýhodněné sazby.
* Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah. Pokud vaše velikost tabulky trasy je větší než 4 000 tras, relace BGP hello bude vyřaďte a nebude možné opětovně povolena dokud hello počet předpon inzerovaných přejde nižší než 4 000.

#### <a name="disable-hello-premium-add-on"></a>Zakázat doplněk premium hello
Pomocí následující rutiny prostředí PowerShell hello můžete zakázat doplněk ExpressRoute premium hello pro váš okruh. stávající:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>šířku pásma okruhu ExpressRoute tooupdate hello
Zkontrolujte hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) pro podporované možnosti šířky pásma pro poskytovatele. Můžete vybrat libovolnou velikost, která je větší než velikost hello existujícího okruhu tak dlouho, dokud umožňuje hello fyzického portu (na kterém je vytvořený váš okruh).

> [!IMPORTANT]
> Pokud je na existující port hello nedostatečné kapacity, mohou mít okruh ExpressRoute toorecreate hello. Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.
>
> Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení. Přechod na starší verzi šířky pásma vyžaduje, abyste okruh ExpressRoute hello toodeprovision a pak znova nezajistíte nové okruh ExpressRoute.
> 
> 

#### <a name="resize-a-circuit"></a>Změna velikosti okruhu

Až se rozhodnete, jakou velikost, je nutné, můžete použít následující příkaz tooresize hello váš okruh:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Váš okruh bude mít byla velikosti na straně Microsoft hello. Obraťte se na vaše připojení poskytovatele tooupdate konfigurace na jejich straně toomatch tuto změnu. Všimněte si, že jsme spustí fakturace můžete pro hello aktualizovat na možnost šířky pásma z tohoto bodu.

Pokud se zobrazí následující chyba, když zvýšení šířku pásma okruhu hello hello, znamená to, existuje ponecháno žádné dostatečnou šířku pásma na hello fyzického portu, kde se má vytvořit existujícím okruhem. Máte toodelete tento okruh a vytvořit nové okruh hello velikosti, které potřebujete. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available tooperform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zřízení a odstraňování okruhu ExpressRoute

### <a name="considerations"></a>Požadavky

* Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute pro tuto operaci toosucceed. Zkontrolujte toosee, pokud máte žádné virtuální sítě, které jsou propojené toohello okruh, pokud se tato operace nezdaří.
* Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno** na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby. Jsme bude i nadále tooreserve prostředky a dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nám vám účtovat.
* Pokud má poskytovatel služeb hello zrušit okruh hello (Stav zřizování poskytovatele služeb hello je nastaven příliš**není zajišťováno**) pak můžete odstranit okruh hello. To se zastaví fakturace hello okruh.

#### <a name="delete-a-circuit"></a>Odstranit okruh

Váš okruh ExpressRoute můžete odstranit spuštěním hello následující příkaz:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Další kroky
Po vytvoření okruhu, ujistěte se, že hello následující:

* [Vytvoření a úprava směrování pro okruhu ExpressRoute](expressroute-howto-routing-classic.md)
* [Propojení vaší virtuální sítě tooyour okruh ExpressRoute](expressroute-howto-linkvnet-classic.md)

