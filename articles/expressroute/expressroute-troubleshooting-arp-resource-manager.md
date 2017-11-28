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
# <a name="getting-arp-tables-in-hello-resource-manager-deployment-model"></a><span data-ttu-id="228d3-103">Získání tabulky ARP v modelu nasazení Resource Manager hello</span><span class="sxs-lookup"><span data-stu-id="228d3-103">Getting ARP tables in hello Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="228d3-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="228d3-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="228d3-105">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="228d3-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="228d3-106">Tento článek vás provede hello hello toolearn kroky, které tabulky ARP pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="228d3-106">This article walks you through hello steps toolearn hello ARP tables for your ExpressRoute circuit.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="228d3-107">Tento dokument je určený toohelp diagnostikovat a opravit jednoduché problémy.</span><span class="sxs-lookup"><span data-stu-id="228d3-107">This document is intended toohelp you diagnose and fix simple issues.</span></span> <span data-ttu-id="228d3-108">Není určený toobe náhradní server pro podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="228d3-108">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="228d3-109">Je nutné otevřít lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud problém nelze toosolve hello podle pokynů hello popsané dole.</span><span class="sxs-lookup"><span data-stu-id="228d3-109">You must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are unable toosolve hello problem using hello guidance described below.</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="228d3-110">Adresa tabulky Resolution Protocol (ARP) a protokolu ARP</span><span class="sxs-lookup"><span data-stu-id="228d3-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="228d3-111">Protokol ARP (Address Resolution Protocol) je protokol vrstvy 2 definované v [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="228d3-111">Address Resolution Protocol (ARP) is a layer 2 protocol defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="228d3-112">Protokol ARP je použité toomap hello adresa sítě Ethernet (adresy MAC) s ip adresou.</span><span class="sxs-lookup"><span data-stu-id="228d3-112">ARP is used toomap hello Ethernet address (MAC address) with an ip address.</span></span>

<span data-ttu-id="228d3-113">Hello tabulky ARP poskytuje mapování hello ipv4 adresy a adresy MAC pro konkrétní partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="228d3-113">hello ARP table provides a mapping of hello ipv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="228d3-114">Hello na základě tabulky ARP pro okruh ExpressRoute vztahy poskytuje hello následující informace pro každé rozhraní (primární i sekundární)</span><span class="sxs-lookup"><span data-stu-id="228d3-114">hello ARP table for an ExpressRoute circuit peering provides hello following information for each interface (primary and secondary)</span></span>

1. <span data-ttu-id="228d3-115">Mapování místní směrovač rozhraní ip adresu toohello adresy MAC</span><span class="sxs-lookup"><span data-stu-id="228d3-115">Mapping of on-premises router interface ip address toohello MAC address</span></span>
2. <span data-ttu-id="228d3-116">Mapování rozhraní směrovače ExpressRoute, ip adresa MAC toohello adresa</span><span class="sxs-lookup"><span data-stu-id="228d3-116">Mapping of ExpressRoute router interface ip address toohello MAC address</span></span>
3. <span data-ttu-id="228d3-117">Stáří mapování hello</span><span class="sxs-lookup"><span data-stu-id="228d3-117">Age of hello mapping</span></span>

<span data-ttu-id="228d3-118">Tabulky ARP může pomoci ověřit konfiguraci vrstvy 2 a řešení potíží s základní vrstvy 2 problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="228d3-118">ARP tables can help validate layer 2 configuration and troubleshooting basic layer 2 connectivity issues.</span></span> 

<span data-ttu-id="228d3-119">Příklad tabulky ARP:</span><span class="sxs-lookup"><span data-stu-id="228d3-119">Example ARP table:</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


<span data-ttu-id="228d3-120">Hello následující část obsahuje informace o tom, jak můžete zobrazit hello tabulky ARP pohledu hello ExpressRoute hraniční směrovače.</span><span class="sxs-lookup"><span data-stu-id="228d3-120">hello following section provides information on how you can view hello ARP tables seen by hello ExpressRoute edge routers.</span></span> 

## <a name="prerequisites-for-learning-arp-tables"></a><span data-ttu-id="228d3-121">Předpoklady pro učení tabulky ARP</span><span class="sxs-lookup"><span data-stu-id="228d3-121">Prerequisites for learning ARP tables</span></span>
<span data-ttu-id="228d3-122">Zajistěte, abyste měli hello po předtím, než jste další průběhu</span><span class="sxs-lookup"><span data-stu-id="228d3-122">Ensure that you have hello following before you progress further</span></span>

* <span data-ttu-id="228d3-123">Okruh ExpressRoute platný nakonfigurované alespoň jeden partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="228d3-123">A Valid ExpressRoute circuit configured with at least one peering.</span></span> <span data-ttu-id="228d3-124">okruh Hello musí být plně nakonfigurované poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="228d3-124">hello circuit must be fully configured by hello connectivity provider.</span></span> <span data-ttu-id="228d3-125">Vy (nebo váš poskytovatel připojení) musí mít nakonfigurované alespoň jeden z partnerských vztahů hello (Azure privátní, veřejný Azure a Microsoft) na tomto okruhu.</span><span class="sxs-lookup"><span data-stu-id="228d3-125">You (or your connectivity provider) must have configured at least one of hello peerings (Azure private, Azure public and Microsoft) on this circuit.</span></span>
* <span data-ttu-id="228d3-126">Rozsahy IP adres použitý ke konfiguraci partnerských vztahů hello (Azure privátní, veřejný Azure a Microsoft).</span><span class="sxs-lookup"><span data-stu-id="228d3-126">IP address ranges used for configuring hello peerings (Azure private, Azure public and Microsoft).</span></span> <span data-ttu-id="228d3-127">Zkontrolujte hello ip adresu přiřazení příklady v hello [stránky požadavky na směrování ExpressRoute](expressroute-routing.md) toointerfaces na vaší straně a na hello ExpressRoute straně namapovaná tooget představu o tom, jak jsou ip adresy.</span><span class="sxs-lookup"><span data-stu-id="228d3-127">Review hello ip address assignment examples in hello [ExpressRoute routing requirements page](expressroute-routing.md) tooget an understanding of how ip addresses are mapped toointerfaces on your side and on hello ExpressRoute side.</span></span> <span data-ttu-id="228d3-128">Můžete získat informace o konfiguraci partnerského vztahu hello kontrolou hello [stránku konfigurace partnerského vztahu ExpressRoute](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="228d3-128">You can get information on hello peering configuration by reviewing hello [ExpressRoute peering configuration page](expressroute-howto-routing-arm.md).</span></span>
* <span data-ttu-id="228d3-129">Informace od týmu pro sítě / poskytovatele připojení na adresy MAC hello rozhraní použít s tyto IP adresy.</span><span class="sxs-lookup"><span data-stu-id="228d3-129">Information from your networking team / connectivity provider on hello MAC addresses of interfaces used with these IP addresses.</span></span>
* <span data-ttu-id="228d3-130">Musí mít hello nejnovější modul prostředí PowerShell pro Azure (verze 1.50 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="228d3-130">You must have hello latest PowerShell module for Azure (version 1.50 or newer).</span></span>

## <a name="getting-hello-arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="228d3-131">Získání hello ARP tabulek pro váš okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="228d3-131">Getting hello ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="228d3-132">Tato část obsahuje pokyny, jak můžete zobrazit hello tabulky ARP na partnerský vztah pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="228d3-132">This section provides instructions on how you can view hello ARP tables per peering using PowerShell.</span></span> <span data-ttu-id="228d3-133">Vy nebo váš poskytovatel připojení musíte nakonfigurovat před pokročíte další partnerský vztah hello.</span><span class="sxs-lookup"><span data-stu-id="228d3-133">You or your connectivity provider must have configured hello peering before progressing further.</span></span> <span data-ttu-id="228d3-134">Každý okruh obsahuje dvě cesty (primární i sekundární).</span><span class="sxs-lookup"><span data-stu-id="228d3-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="228d3-135">Můžete zkontrolovat hello na základě tabulky ARP pro každou z cest nezávisle.</span><span class="sxs-lookup"><span data-stu-id="228d3-135">You can check hello ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="228d3-136">Tabulky ARP pro soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="228d3-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="228d3-137">následující rutina Hello poskytuje hello ARP tabulky pro soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="228d3-137">hello following cmdlet provides hello ARP tables for Azure private peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

<span data-ttu-id="228d3-138">Ukázkový výstup je zobrazena níže pro jednu z cesty hello</span><span class="sxs-lookup"><span data-stu-id="228d3-138">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="228d3-139">Tabulky ARP pro veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="228d3-139">ARP tables for Azure public peering</span></span>
<span data-ttu-id="228d3-140">následující rutina Hello poskytuje hello ARP tabulky pro veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="228d3-140">hello following cmdlet provides hello ARP tables for Azure public peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


<span data-ttu-id="228d3-141">Ukázkový výstup je zobrazena níže pro jednu z cesty hello</span><span class="sxs-lookup"><span data-stu-id="228d3-141">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="228d3-142">Tabulky ARP pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="228d3-142">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="228d3-143">následující rutina Hello poskytuje hello ARP tabulky pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="228d3-143">hello following cmdlet provides hello ARP tables for Microsoft peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


<span data-ttu-id="228d3-144">Ukázkový výstup je zobrazena níže pro jednu z cesty hello</span><span class="sxs-lookup"><span data-stu-id="228d3-144">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a><span data-ttu-id="228d3-145">Jak toouse tyto informace</span><span class="sxs-lookup"><span data-stu-id="228d3-145">How toouse this information</span></span>
<span data-ttu-id="228d3-146">Hello tabulky ARP partnerský vztah můžete použít toodetermine ověřit konfiguraci vrstvy 2 a připojení.</span><span class="sxs-lookup"><span data-stu-id="228d3-146">hello ARP table of a peering can be used toodetermine validate layer 2 configuration and connectivity.</span></span> <span data-ttu-id="228d3-147">Tato část obsahuje přehled o tom, jak bude vypadat tabulky ARP v různých scénářích.</span><span class="sxs-lookup"><span data-stu-id="228d3-147">This section provides an overview of how ARP tables will look under different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a><span data-ttu-id="228d3-148">Na základě tabulky ARP když okruh v provozní stav (očekávaný stav)</span><span class="sxs-lookup"><span data-stu-id="228d3-148">ARP table when a circuit is in operational state (expected state)</span></span>
* <span data-ttu-id="228d3-149">Hello tabulky ARP bude mít položka pro místní straně hello s platnou IP adresu a adresu MAC a podobně jako položka pro hello straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="228d3-149">hello ARP table will have an entry for hello on-premises side with a valid IP address and MAC address and a similar entry for hello Microsoft side.</span></span> 
* <span data-ttu-id="228d3-150">poslední oktet Hello hello místní ip adresy, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="228d3-150">hello last octet of hello on-premises ip address will always be an odd number.</span></span>
* <span data-ttu-id="228d3-151">poslední oktetu Hello hello Microsoft ip adresu, bude vždy sudé číslo.</span><span class="sxs-lookup"><span data-stu-id="228d3-151">hello last octet of hello Microsoft ip address will always be an even number.</span></span>
* <span data-ttu-id="228d3-152">Hello stejnou adresu MAC se zobrazí na hello straně společnosti Microsoft pro všechny 3 partnerské vztahy (primární / sekundární).</span><span class="sxs-lookup"><span data-stu-id="228d3-152">hello same MAC address will appear on hello Microsoft side for all 3 peerings (primary / secondary).</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a><span data-ttu-id="228d3-153">Tabulky ARP, kdy místní / straně připojení zprostředkovatele došlo k problémům</span><span class="sxs-lookup"><span data-stu-id="228d3-153">ARP table when on-premises / connectivity provider side has problems</span></span>
<span data-ttu-id="228d3-154">Pokud dochází k potížím s hello místní nebo poskytovatele připojení, mohou se zobrazit, že buď jenom jedna položka se zobrazí v hello adresu MAC místní ARP pro tabulku nebo hello zobrazí neúplné.</span><span class="sxs-lookup"><span data-stu-id="228d3-154">If there are issues with hello on-premises or connectivity provider you may see that either only one entry will appear in hello ARP table or hello on-prem MAC address will show incomplete.</span></span> <span data-ttu-id="228d3-155">Zobrazí hello mapování mezi hello adresu MAC a IP adresu použitou při hello straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="228d3-155">This will show hello mapping between hello MAC address and IP address used in hello Microsoft side.</span></span> 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

<span data-ttu-id="228d3-156">nebo</span><span class="sxs-lookup"><span data-stu-id="228d3-156">or</span></span>
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> <span data-ttu-id="228d3-157">Otevřete žádost o podporu se vaše připojení poskytovatele toodebug tyto problémy.</span><span class="sxs-lookup"><span data-stu-id="228d3-157">Open a support request with your connectivity provider toodebug such issues.</span></span> <span data-ttu-id="228d3-158">Pokud nemá hello tabulky ARP IP adresy rozhraní hello mapované tooMAC adresy, hello zkontrolujte následující informace:</span><span class="sxs-lookup"><span data-stu-id="228d3-158">If hello ARP table does not have IP addresses of hello interfaces mapped tooMAC addresses, review hello following information:</span></span>
> 
> 1. <span data-ttu-id="228d3-159">Pokud přiřazená hello první IP adresa podsítě hello /30 pro hello propojení mezi hello MSEE PR a MSEE se používá na hello rozhraní MSEE PR.</span><span class="sxs-lookup"><span data-stu-id="228d3-159">If hello first IP address of hello /30 subnet assigned for hello link between hello MSEE-PR and MSEE is used on hello interface of MSEE-PR.</span></span> <span data-ttu-id="228d3-160">Azure vždy používá hello druhou IP adresu pro Msee.</span><span class="sxs-lookup"><span data-stu-id="228d3-160">Azure always uses hello second IP address for MSEEs.</span></span>
> 2. <span data-ttu-id="228d3-161">Ověřte, pokud zákazník hello (C-Tag) a značky VLAN služby (S-Tag) odpovídají na dvojice MSEE PR a MSEE.</span><span class="sxs-lookup"><span data-stu-id="228d3-161">Verify if hello customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a><span data-ttu-id="228d3-162">Na základě tabulky ARP při Microsoft straně má potíže s</span><span class="sxs-lookup"><span data-stu-id="228d3-162">ARP table when Microsoft side has problems</span></span>
* <span data-ttu-id="228d3-163">Neuvidíte na základě tabulky ARP pro partnerský vztah, pokud jsou na straně Microsoft hello problémy.</span><span class="sxs-lookup"><span data-stu-id="228d3-163">You will not see an ARP table shown for a peering if there are issues on hello Microsoft side.</span></span> 
* <span data-ttu-id="228d3-164">Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="228d3-164">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="228d3-165">Zadejte, že máte potíže s připojením vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="228d3-165">Specify that you have an issue with layer 2 connectivity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="228d3-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="228d3-166">Next Steps</span></span>
* <span data-ttu-id="228d3-167">Ověření konfigurace vrstvy 3 pro váš okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="228d3-167">Validate Layer 3 configurations for your ExpressRoute circuit</span></span>
  * <span data-ttu-id="228d3-168">Zjištění trasy souhrnné toodetermine hello stavu relací protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="228d3-168">Get route summary toodetermine hello state of BGP sessions</span></span> 
  * <span data-ttu-id="228d3-169">Získat toodetermine tabulky trasy, které předpony jsou ohlášené mezi ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="228d3-169">Get route table toodetermine which prefixes are advertised across ExpressRoute</span></span>
* <span data-ttu-id="228d3-170">Ověřit kontrolou bajtů vstupně -výstupnímu přenos dat</span><span class="sxs-lookup"><span data-stu-id="228d3-170">Validate data transfer by reviewing bytes in / out</span></span>
* <span data-ttu-id="228d3-171">Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud pořád dochází k problémům.</span><span class="sxs-lookup"><span data-stu-id="228d3-171">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

