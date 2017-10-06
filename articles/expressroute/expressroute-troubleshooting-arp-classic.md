---
title: "Získávání tabulky ARP: Classic: řešení potíží s Azure ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje pokyny pro získávání hello ARP tabulky pro okruh ExpressRoute."
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: b5856acf-03c2-4933-8111-6ce12998d92a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: 2b01304a38fa0e0def27dbd7c391d7ad8bbdabff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-classic-deployment-model"></a>Získání tabulky ARP v modelu nasazení classic hello
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – Classic](expressroute-troubleshooting-arp-classic.md)
> 
> 

Tento článek vás provede kroky hello pro získání tabulek hello protokolu ARP (Address Resolution Protocol) pro váš okruh Azure ExpressRoute.

> [!IMPORTANT]
> Tento dokument je určený toohelp diagnostikovat a opravit jednoduché problémy. Není určený toobe náhradní server pro podporu společnosti Microsoft. Pokud se hello problém nelze vyřešit pomocí hello pokynů, otevřete žádost o podporu s [Microsoft Azure Nápověda a podpora](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresa tabulky Resolution Protocol (ARP) a protokolu ARP
ARP je protokol vrstvy 2, který je definován v [RFC 826](https://tools.ietf.org/html/rfc826). Protokol ARP je použité toomap IP adresu tooan adresy (adresy MAC) sítě Ethernet.

Základě tabulky ARP poskytuje mapování hello IPv4 adresy a adresy MAC pro konkrétní partnerský vztah. Hello na základě tabulky ARP pro okruh ExpressRoute vztahy poskytuje hello následující informace pro každé rozhraní (primární i sekundární):

1. Mapování adresy IP adresu tooa MAC místní směrovač rozhraní
2. Mapování adresy IP adresu tooa MAC ExpressRoute směrovač rozhraní
3. stáří Hello mapování hello

Tabulky ARP může pomoct s Probíhá ověřování konfigurace vrstvy 2 a řešení potíží s problémy s připojením k základní vrstvy 2.

Tady je příklad tabulky ARP:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Hello následující část obsahuje informace o tom, jak tooview hello tabulky ARP, které jsou vidět hello ExpressRoute hraniční směrovače.

## <a name="prerequisites-for-using-arp-tables"></a>Předpoklady pro použití tabulky ARP
Ujistěte se, že máte následující hello, než budete pokračovat:

* Platný okruh ExpressRoute, který je nakonfigurovaný s alespoň jeden partnerský vztah. okruh Hello musí být plně nakonfigurované poskytovatelem připojení hello. Vy (nebo váš poskytovatel připojení) na musíte nakonfigurovat alespoň jeden z partnerských vztahů hello (Azure privátní, veřejný Azure nebo Microsoft) tento okruh.
* Rozsahy IP adres, které se používají ke konfiguraci partnerských vztahů hello (Azure privátní, veřejný Azure a Microsoft). Zkontrolujte hello IP adresu přiřazení příklady v hello [stránky požadavky na směrování ExpressRoute](expressroute-routing.md) tooget představu o tom, jak jsou IP adresy namapované toointerfaces na vaše aise a na straně hello ExpressRoute. Můžete získat informace o konfiguraci partnerského vztahu hello kontrolou hello [stránku konfigurace partnerského vztahu ExpressRoute](expressroute-howto-routing-classic.md).
* Informace od poskytovatele tým nebo připojení k síťové adresy MAC hello hello rozhraní, které se používají s tyto IP adresy.
* Hello nejnovější modul Windows PowerShell pro Azure (verze 1.50 nebo novější).

## <a name="arp-tables-for-your-expressroute-circuit"></a>Tabulky ARP pro váš okruh ExpressRoute
Tato část obsahuje pokyny, jak tooview hello ARP tabulky pro každý typ partnerského vztahu pomocí prostředí PowerShell. Než budete pokračovat, budou vy nebo váš poskytovatel připojení partnerský vztah hello tooconfigure potřebuje. Každý okruh obsahuje dvě cesty (primární i sekundární). Můžete zkontrolovat hello na základě tabulky ARP pro každou z cest nezávisle.

### <a name="arp-tables-for-azure-private-peering"></a>Tabulky ARP pro soukromý partnerský vztah Azure
následující rutiny Hello poskytuje hello ARP tabulky pro soukromý partnerský vztah Azure:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Toto je ukázkový výstup pro jednu hello cesty:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Tabulky ARP pro veřejný partnerský vztah Azure:
následující rutiny Hello poskytuje hello ARP tabulky pro veřejný partnerský vztah Azure:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Toto je ukázkový výstup pro jednu hello cesty:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Toto je ukázkový výstup pro jednu hello cesty:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Tabulky ARP pro partnerský vztah Microsoftu
následující rutina Hello poskytuje hello ARP tabulky pro partnerský vztah Microsoftu:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Ukázkový výstup je zobrazena níže pro jednu hello cesty:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a>Jak toouse tyto informace
Hello tabulky ARP partnerský vztah může být použité toovalidate vrstvy 2 konfigurací a připojením. Tato část obsahuje přehled o tom, jak vzhledu tabulky ARP v různých scénářích.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Na základě tabulky ARP při okruh je ve stavu provozní (očekávaný)
* Hello tabulky ARP má záznam pro místní straně hello s platnou adresu IP a MAC a podobně jako položka pro hello straně společnosti Microsoft.
* poslední oktet Hello hello místní IP adresy je vždy lichý počet.
* poslední oktetu Hello hello Microsoft IP adresa je vždy sudé číslo.
* Hello stejnou adresu MAC se zobrazí na hello straně společnosti Microsoft pro všechny tři partnerské vztahy (primární nebo sekundární).

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-hello-connectivity-provider-side-has-problems"></a>Na základě tabulky ARP, když je na místě nebo když hello poskytovatele připojení straně má potíže s
 Zobrazí se jenom jedna položka v hello tabulky ARP. Zobrazuje hello mapování mezi adresa MAC hello a hello IP adresu, která se používá na straně Microsoft hello.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Pokud dojde k problému takto, otevřete podporu požadavků s vaší tooresolve poskytovatele připojení.
> 
> 

### <a name="arp-table-when-hello-microsoft-side-has-problems"></a>Na základě tabulky ARP při hello Microsoft straně má potíže s
* Neuvidíte na základě tabulky ARP pro partnerský vztah, pokud jsou na straně Microsoft hello problémy.
* Otevřete žádost o podporu s [Microsoft Azure Nápověda a podpora](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Zadejte, že máte potíže s připojením vrstvy 2.

## <a name="next-steps"></a>Další kroky
* Ověření konfigurace vrstvy 3 pro váš okruh ExpressRoute:
  * Zjištění stavu hello souhrnné toodetermine trasy relací protokolu BGP.
  * Získání toodetermine tabulky trasy předpon, které jsou ohlášené mezi ExpressRoute.
* Přenos dat ověřte kontrolou bajtů a odhlášení.
* Otevřete žádost o podporu s [Microsoft Azure Nápověda a podpora](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud pořád dochází k problémům.

