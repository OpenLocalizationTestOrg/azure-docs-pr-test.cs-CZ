---
title: "Získávání tabulky ARP: Resource Manager: řešení potíží s Azure ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje pokyny, získávání tabulky ARP pro okruh ExpressRoute"
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
ms.openlocfilehash: a65b1ba2998eae33b3e73bd2492fbbf025eb5946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a><span data-ttu-id="841f0-103">Získání tabulky ARP v modelu nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="841f0-103">Getting ARP tables in the Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="841f0-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="841f0-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="841f0-105">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="841f0-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="841f0-106">Tento článek vás provede kroky pro další tabulky ARP pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="841f0-106">This article walks you through the steps to learn the ARP tables for your ExpressRoute circuit.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="841f0-107">Tento dokument je určený vám pomohou diagnostikovat a opravit problémy, jednoduché.</span><span class="sxs-lookup"><span data-stu-id="841f0-107">This document is intended to help you diagnose and fix simple issues.</span></span> <span data-ttu-id="841f0-108">Rozhraní není určeno k jako náhrada za podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="841f0-108">It is not intended to be a replacement for Microsoft support.</span></span> <span data-ttu-id="841f0-109">Je nutné otevřít lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) nejde k jejich vyřešení podle pokynů popsaných níže.</span><span class="sxs-lookup"><span data-stu-id="841f0-109">You must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are unable to solve the problem using the guidance described below.</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="841f0-110">Adresa tabulky Resolution Protocol (ARP) a protokolu ARP</span><span class="sxs-lookup"><span data-stu-id="841f0-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="841f0-111">Protokol ARP (Address Resolution Protocol) je protokol vrstvy 2 definované v [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="841f0-111">Address Resolution Protocol (ARP) is a layer 2 protocol defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="841f0-112">Slouží k mapování adresa sítě Ethernet (adresy MAC) s ip adresou.</span><span class="sxs-lookup"><span data-stu-id="841f0-112">ARP is used to map the Ethernet address (MAC address) with an ip address.</span></span>

<span data-ttu-id="841f0-113">Základě tabulky ARP poskytuje mapování adresy ipv4 a adresa MAC pro konkrétní partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="841f0-113">The ARP table provides a mapping of the ipv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="841f0-114">Tabulky ARP pro okruh ExpressRoute vztahy poskytuje následující informace pro každé rozhraní (primární i sekundární)</span><span class="sxs-lookup"><span data-stu-id="841f0-114">The ARP table for an ExpressRoute circuit peering provides the following information for each interface (primary and secondary)</span></span>

1. <span data-ttu-id="841f0-115">Mapování místní směrovač rozhraní ip adresy pro adresu MAC</span><span class="sxs-lookup"><span data-stu-id="841f0-115">Mapping of on-premises router interface ip address to the MAC address</span></span>
2. <span data-ttu-id="841f0-116">Mapování ExpressRoute směrovač rozhraní ip adresy pro adresu MAC</span><span class="sxs-lookup"><span data-stu-id="841f0-116">Mapping of ExpressRoute router interface ip address to the MAC address</span></span>
3. <span data-ttu-id="841f0-117">Stáří mapování</span><span class="sxs-lookup"><span data-stu-id="841f0-117">Age of the mapping</span></span>

<span data-ttu-id="841f0-118">Tabulky ARP může pomoci ověřit konfiguraci vrstvy 2 a řešení potíží s základní vrstvy 2 problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="841f0-118">ARP tables can help validate layer 2 configuration and troubleshooting basic layer 2 connectivity issues.</span></span> 

<span data-ttu-id="841f0-119">Příklad tabulky ARP:</span><span class="sxs-lookup"><span data-stu-id="841f0-119">Example ARP table:</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


<span data-ttu-id="841f0-120">Následující část obsahuje informace o tom, jak můžete zobrazit tabulky ARP pohledu hraniční směrovače ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="841f0-120">The following section provides information on how you can view the ARP tables seen by the ExpressRoute edge routers.</span></span> 

## <a name="prerequisites-for-learning-arp-tables"></a><span data-ttu-id="841f0-121">Předpoklady pro učení tabulky ARP</span><span class="sxs-lookup"><span data-stu-id="841f0-121">Prerequisites for learning ARP tables</span></span>
<span data-ttu-id="841f0-122">Zajistěte, abyste měli následující předtím, než jste další průběhu</span><span class="sxs-lookup"><span data-stu-id="841f0-122">Ensure that you have the following before you progress further</span></span>

* <span data-ttu-id="841f0-123">Okruh ExpressRoute platný nakonfigurované alespoň jeden partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="841f0-123">A Valid ExpressRoute circuit configured with at least one peering.</span></span> <span data-ttu-id="841f0-124">Okruh musí být plně nakonfigurované poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="841f0-124">The circuit must be fully configured by the connectivity provider.</span></span> <span data-ttu-id="841f0-125">Vy (nebo váš poskytovatel připojení) musí mít nakonfigurované alespoň jeden z partnerských vztahů (Azure privátní, veřejný Azure a Microsoft) na tomto okruhu.</span><span class="sxs-lookup"><span data-stu-id="841f0-125">You (or your connectivity provider) must have configured at least one of the peerings (Azure private, Azure public and Microsoft) on this circuit.</span></span>
* <span data-ttu-id="841f0-126">Rozsahy IP adres použitý ke konfiguraci partnerských vztahů (Azure privátní, veřejný Azure a Microsoft).</span><span class="sxs-lookup"><span data-stu-id="841f0-126">IP address ranges used for configuring the peerings (Azure private, Azure public and Microsoft).</span></span> <span data-ttu-id="841f0-127">Zkontrolujte příklady přiřazení ip adres v [stránky požadavky na směrování ExpressRoute](expressroute-routing.md) získat představu o tom, jak jsou ip adresy mapované na rozhraní na vaší straně a na straně ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="841f0-127">Review the ip address assignment examples in the [ExpressRoute routing requirements page](expressroute-routing.md) to get an understanding of how ip addresses are mapped to interfaces on your side and on the ExpressRoute side.</span></span> <span data-ttu-id="841f0-128">Informace o konfiguraci partnerského vztahu můžete získat kontrolou [stránku konfigurace partnerského vztahu ExpressRoute](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="841f0-128">You can get information on the peering configuration by reviewing the [ExpressRoute peering configuration page](expressroute-howto-routing-arm.md).</span></span>
* <span data-ttu-id="841f0-129">Informace od týmu pro sítě / poskytovatele připojení na adresy MAC rozhraní použít s tyto IP adresy.</span><span class="sxs-lookup"><span data-stu-id="841f0-129">Information from your networking team / connectivity provider on the MAC addresses of interfaces used with these IP addresses.</span></span>
* <span data-ttu-id="841f0-130">Musí mít nejnovější modul prostředí PowerShell pro Azure (verze 1.50 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="841f0-130">You must have the latest PowerShell module for Azure (version 1.50 or newer).</span></span>

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="841f0-131">Získávání tabulky ARP pro váš okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="841f0-131">Getting the ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="841f0-132">Tato část obsahuje informace o tom, jak můžete zobrazit tabulky ARP na partnerský vztah pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="841f0-132">This section provides instructions on how you can view the ARP tables per peering using PowerShell.</span></span> <span data-ttu-id="841f0-133">Vy nebo váš poskytovatel připojení musíte nakonfigurovat partnerský vztah než pokročíte Další.</span><span class="sxs-lookup"><span data-stu-id="841f0-133">You or your connectivity provider must have configured the peering before progressing further.</span></span> <span data-ttu-id="841f0-134">Každý okruh obsahuje dvě cesty (primární i sekundární).</span><span class="sxs-lookup"><span data-stu-id="841f0-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="841f0-135">Tabulky ARP pro každou z cest můžete zkontrolovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="841f0-135">You can check the ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="841f0-136">Tabulky ARP pro soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="841f0-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="841f0-137">Následující rutiny najdete protokolu ARP tabulky pro soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="841f0-137">The following cmdlet provides the ARP tables for Azure private peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

<span data-ttu-id="841f0-138">Ukázkový výstup je zobrazena níže pro jedna z cest</span><span class="sxs-lookup"><span data-stu-id="841f0-138">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="841f0-139">Tabulky ARP pro veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="841f0-139">ARP tables for Azure public peering</span></span>
<span data-ttu-id="841f0-140">Následující rutiny najdete protokolu ARP tabulky pro veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="841f0-140">The following cmdlet provides the ARP tables for Azure public peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


<span data-ttu-id="841f0-141">Ukázkový výstup je zobrazena níže pro jedna z cest</span><span class="sxs-lookup"><span data-stu-id="841f0-141">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="841f0-142">Tabulky ARP pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="841f0-142">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="841f0-143">Následující rutiny najdete protokolu ARP tabulky pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="841f0-143">The following cmdlet provides the ARP tables for Microsoft peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


<span data-ttu-id="841f0-144">Ukázkový výstup je zobrazena níže pro jedna z cest</span><span class="sxs-lookup"><span data-stu-id="841f0-144">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a><span data-ttu-id="841f0-145">Jak používat tyto informace</span><span class="sxs-lookup"><span data-stu-id="841f0-145">How to use this information</span></span>
<span data-ttu-id="841f0-146">Tabulky ARP partnerský vztah můžete použít k určení ověřit konfiguraci vrstvy 2 a připojení.</span><span class="sxs-lookup"><span data-stu-id="841f0-146">The ARP table of a peering can be used to determine validate layer 2 configuration and connectivity.</span></span> <span data-ttu-id="841f0-147">Tato část obsahuje přehled o tom, jak bude vypadat tabulky ARP v různých scénářích.</span><span class="sxs-lookup"><span data-stu-id="841f0-147">This section provides an overview of how ARP tables will look under different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a><span data-ttu-id="841f0-148">Na základě tabulky ARP když okruh v provozní stav (očekávaný stav)</span><span class="sxs-lookup"><span data-stu-id="841f0-148">ARP table when a circuit is in operational state (expected state)</span></span>
* <span data-ttu-id="841f0-149">Tabulky ARP bude mít položka pro místní straně s platnou IP adresu a adresu MAC a podobně jako položka pro straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="841f0-149">The ARP table will have an entry for the on-premises side with a valid IP address and MAC address and a similar entry for the Microsoft side.</span></span> 
* <span data-ttu-id="841f0-150">Poslední oktet místní ip adresu, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="841f0-150">The last octet of the on-premises ip address will always be an odd number.</span></span>
* <span data-ttu-id="841f0-151">Poslední oktet ip adresy Microsoft bude vždy sudé číslo.</span><span class="sxs-lookup"><span data-stu-id="841f0-151">The last octet of the Microsoft ip address will always be an even number.</span></span>
* <span data-ttu-id="841f0-152">Zobrazí se stejnou adresu MAC na straně společnosti Microsoft pro všechny 3 partnerské vztahy (primární / sekundární).</span><span class="sxs-lookup"><span data-stu-id="841f0-152">The same MAC address will appear on the Microsoft side for all 3 peerings (primary / secondary).</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a><span data-ttu-id="841f0-153">Tabulky ARP, kdy místní / straně připojení zprostředkovatele došlo k problémům</span><span class="sxs-lookup"><span data-stu-id="841f0-153">ARP table when on-premises / connectivity provider side has problems</span></span>
<span data-ttu-id="841f0-154">Pokud dochází k potížím s místní nebo poskytovatele připojení, které se můžete setkat, zobrazí se buď jenom jedna položka v tabulky ARP nebo adresu MAC místní zobrazí neúplné.</span><span class="sxs-lookup"><span data-stu-id="841f0-154">If there are issues with the on-premises or connectivity provider you may see that either only one entry will appear in the ARP table or the on-prem MAC address will show incomplete.</span></span> <span data-ttu-id="841f0-155">Zobrazí mapování mezi adresa MAC a IP adresu použitou na straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="841f0-155">This will show the mapping between the MAC address and IP address used in the Microsoft side.</span></span> 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

<span data-ttu-id="841f0-156">nebo</span><span class="sxs-lookup"><span data-stu-id="841f0-156">or</span></span>
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> <span data-ttu-id="841f0-157">Otevřete žádost o podporu se svého poskytovatele připojení k ladění tyto problémy.</span><span class="sxs-lookup"><span data-stu-id="841f0-157">Open a support request with your connectivity provider to debug such issues.</span></span> <span data-ttu-id="841f0-158">Pokud základě tabulky ARP nemá IP adresy rozhraní namapované na adresy MAC, projděte si následující informace:</span><span class="sxs-lookup"><span data-stu-id="841f0-158">If the ARP table does not have IP addresses of the interfaces mapped to MAC addresses, review the following information:</span></span>
> 
> 1. <span data-ttu-id="841f0-159">Pokud první IP adresa/30 podsítě přiřazené pro propojení mezi MSEE PR a MSEE se používá na rozhraní MSEE PR.</span><span class="sxs-lookup"><span data-stu-id="841f0-159">If the first IP address of the /30 subnet assigned for the link between the MSEE-PR and MSEE is used on the interface of MSEE-PR.</span></span> <span data-ttu-id="841f0-160">Azure vždy používá druhou IP adresu pro Msee.</span><span class="sxs-lookup"><span data-stu-id="841f0-160">Azure always uses the second IP address for MSEEs.</span></span>
> 2. <span data-ttu-id="841f0-161">Ověřte, pokud zákazník (C-Tag) a značky VLAN služby (S-Tag) odpovídají na dvojice MSEE PR a MSEE.</span><span class="sxs-lookup"><span data-stu-id="841f0-161">Verify if the customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a><span data-ttu-id="841f0-162">Na základě tabulky ARP při Microsoft straně má potíže s</span><span class="sxs-lookup"><span data-stu-id="841f0-162">ARP table when Microsoft side has problems</span></span>
* <span data-ttu-id="841f0-163">Neuvidíte na základě tabulky ARP pro partnerský vztah, pokud dochází k potížím na straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="841f0-163">You will not see an ARP table shown for a peering if there are issues on the Microsoft side.</span></span> 
* <span data-ttu-id="841f0-164">Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="841f0-164">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="841f0-165">Zadejte, že máte potíže s připojením vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="841f0-165">Specify that you have an issue with layer 2 connectivity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="841f0-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="841f0-166">Next Steps</span></span>
* <span data-ttu-id="841f0-167">Ověření konfigurace vrstvy 3 pro váš okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="841f0-167">Validate Layer 3 configurations for your ExpressRoute circuit</span></span>
  * <span data-ttu-id="841f0-168">Získání trasy shrnutí k určení stavu relací protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="841f0-168">Get route summary to determine the state of BGP sessions</span></span> 
  * <span data-ttu-id="841f0-169">Získejte tabulku směrování k určení, které předpony jsou ohlášené mezi ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="841f0-169">Get route table to determine which prefixes are advertised across ExpressRoute</span></span>
* <span data-ttu-id="841f0-170">Ověřit kontrolou bajtů vstupně -výstupnímu přenos dat</span><span class="sxs-lookup"><span data-stu-id="841f0-170">Validate data transfer by reviewing bytes in / out</span></span>
* <span data-ttu-id="841f0-171">Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud pořád dochází k problémům.</span><span class="sxs-lookup"><span data-stu-id="841f0-171">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

