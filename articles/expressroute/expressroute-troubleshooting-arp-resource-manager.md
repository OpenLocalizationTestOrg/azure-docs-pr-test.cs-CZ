---
title: "Získávání tabulky ARP: Resource Manager: řešení potíží s Azure ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje pokyny, získávání hello ARP tabulky pro okruh ExpressRoute"
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: c386b031814d40ef6ea3ce5e0eaaab9634470e8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-resource-manager-deployment-model"></a>Získání tabulky ARP v modelu nasazení Resource Manager hello
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – Classic](expressroute-troubleshooting-arp-classic.md)
> 
> 

Tento článek vás provede hello hello toolearn kroky, které tabulky ARP pro váš okruh ExpressRoute. 

> [!IMPORTANT]
> Tento dokument je určený toohelp diagnostikovat a opravit jednoduché problémy. Není určený toobe náhradní server pro podporu společnosti Microsoft. Je nutné otevřít lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud problém nelze toosolve hello podle pokynů hello popsané dole.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresa tabulky Resolution Protocol (ARP) a protokolu ARP
Protokol ARP (Address Resolution Protocol) je protokol vrstvy 2 definované v [RFC 826](https://tools.ietf.org/html/rfc826). Protokol ARP je použité toomap hello adresa sítě Ethernet (adresy MAC) s ip adresou.

Hello tabulky ARP poskytuje mapování hello ipv4 adresy a adresy MAC pro konkrétní partnerský vztah. Hello na základě tabulky ARP pro okruh ExpressRoute vztahy poskytuje hello následující informace pro každé rozhraní (primární i sekundární)

1. Mapování místní směrovač rozhraní ip adresu toohello adresy MAC
2. Mapování rozhraní směrovače ExpressRoute, ip adresa MAC toohello adresa
3. Stáří mapování hello

Tabulky ARP může pomoci ověřit konfiguraci vrstvy 2 a řešení potíží s základní vrstvy 2 problémy s připojením. 

Příklad tabulky ARP: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


Hello následující část obsahuje informace o tom, jak můžete zobrazit hello tabulky ARP pohledu hello ExpressRoute hraniční směrovače. 

## <a name="prerequisites-for-learning-arp-tables"></a>Předpoklady pro učení tabulky ARP
Zajistěte, abyste měli hello po předtím, než jste další průběhu

* Okruh ExpressRoute platný nakonfigurované alespoň jeden partnerský vztah. okruh Hello musí být plně nakonfigurované poskytovatelem připojení hello. Vy (nebo váš poskytovatel připojení) musí mít nakonfigurované alespoň jeden z partnerských vztahů hello (Azure privátní, veřejný Azure a Microsoft) na tomto okruhu.
* Rozsahy IP adres použitý ke konfiguraci partnerských vztahů hello (Azure privátní, veřejný Azure a Microsoft). Zkontrolujte hello ip adresu přiřazení příklady v hello [stránky požadavky na směrování ExpressRoute](expressroute-routing.md) toointerfaces na vaší straně a na hello ExpressRoute straně namapovaná tooget představu o tom, jak jsou ip adresy. Můžete získat informace o konfiguraci partnerského vztahu hello kontrolou hello [stránku konfigurace partnerského vztahu ExpressRoute](expressroute-howto-routing-arm.md).
* Informace od týmu pro sítě / poskytovatele připojení na adresy MAC hello rozhraní použít s tyto IP adresy.
* Musí mít hello nejnovější modul prostředí PowerShell pro Azure (verze 1.50 nebo novější).

## <a name="getting-hello-arp-tables-for-your-expressroute-circuit"></a>Získání hello ARP tabulek pro váš okruh ExpressRoute
Tato část obsahuje pokyny, jak můžete zobrazit hello tabulky ARP na partnerský vztah pomocí prostředí PowerShell. Vy nebo váš poskytovatel připojení musíte nakonfigurovat před pokročíte další partnerský vztah hello. Každý okruh obsahuje dvě cesty (primární i sekundární). Můžete zkontrolovat hello na základě tabulky ARP pro každou z cest nezávisle.

### <a name="arp-tables-for-azure-private-peering"></a>Tabulky ARP pro soukromý partnerský vztah Azure
následující rutina Hello poskytuje hello ARP tabulky pro soukromý partnerský vztah Azure

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Ukázkový výstup je zobrazena níže pro jednu z cesty hello

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Tabulky ARP pro veřejný partnerský vztah Azure
následující rutina Hello poskytuje hello ARP tabulky pro veřejný partnerský vztah Azure

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Ukázkový výstup je zobrazena níže pro jednu z cesty hello

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Tabulky ARP pro partnerský vztah Microsoftu
následující rutina Hello poskytuje hello ARP tabulky pro partnerský vztah Microsoftu

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Ukázkový výstup je zobrazena níže pro jednu z cesty hello

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a>Jak toouse tyto informace
Hello tabulky ARP partnerský vztah můžete použít toodetermine ověřit konfiguraci vrstvy 2 a připojení. Tato část obsahuje přehled o tom, jak bude vypadat tabulky ARP v různých scénářích.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Na základě tabulky ARP když okruh v provozní stav (očekávaný stav)
* Hello tabulky ARP bude mít položka pro místní straně hello s platnou IP adresu a adresu MAC a podobně jako položka pro hello straně společnosti Microsoft. 
* poslední oktet Hello hello místní ip adresy, bude vždy lichý počet.
* poslední oktetu Hello hello Microsoft ip adresu, bude vždy sudé číslo.
* Hello stejnou adresu MAC se zobrazí na hello straně společnosti Microsoft pro všechny 3 partnerské vztahy (primární / sekundární). 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>Tabulky ARP, kdy místní / straně připojení zprostředkovatele došlo k problémům
Pokud dochází k potížím s hello místní nebo poskytovatele připojení, mohou se zobrazit, že buď jenom jedna položka se zobrazí v hello adresu MAC místní ARP pro tabulku nebo hello zobrazí neúplné. Zobrazí hello mapování mezi hello adresu MAC a IP adresu použitou při hello straně společnosti Microsoft. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

nebo
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Otevřete žádost o podporu se vaše připojení poskytovatele toodebug tyto problémy. Pokud nemá hello tabulky ARP IP adresy rozhraní hello mapované tooMAC adresy, hello zkontrolujte následující informace:
> 
> 1. Pokud přiřazená hello první IP adresa podsítě hello /30 pro hello propojení mezi hello MSEE PR a MSEE se používá na hello rozhraní MSEE PR. Azure vždy používá hello druhou IP adresu pro Msee.
> 2. Ověřte, pokud zákazník hello (C-Tag) a značky VLAN služby (S-Tag) odpovídají na dvojice MSEE PR a MSEE.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>Na základě tabulky ARP při Microsoft straně má potíže s
* Neuvidíte na základě tabulky ARP pro partnerský vztah, pokud jsou na straně Microsoft hello problémy. 
* Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Zadejte, že máte potíže s připojením vrstvy 2. 

## <a name="next-steps"></a>Další kroky
* Ověření konfigurace vrstvy 3 pro váš okruh ExpressRoute
  * Zjištění trasy souhrnné toodetermine hello stavu relací protokolu BGP 
  * Získat toodetermine tabulky trasy, které předpony jsou ohlášené mezi ExpressRoute
* Ověřit kontrolou bajtů vstupně -výstupnímu přenos dat
* Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud pořád dochází k problémům.

