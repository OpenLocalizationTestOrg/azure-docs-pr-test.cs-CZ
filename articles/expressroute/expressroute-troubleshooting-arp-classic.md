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
# <a name="getting-arp-tables-in-hello-classic-deployment-model"></a><span data-ttu-id="4ac47-103">Získání tabulky ARP v modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="4ac47-103">Getting ARP tables in hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4ac47-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4ac47-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="4ac47-105">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="4ac47-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="4ac47-106">Tento článek vás provede kroky hello pro získání tabulek hello protokolu ARP (Address Resolution Protocol) pro váš okruh Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4ac47-106">This article walks you through hello steps for getting hello Address Resolution Protocol (ARP) tables for your Azure ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4ac47-107">Tento dokument je určený toohelp diagnostikovat a opravit jednoduché problémy.</span><span class="sxs-lookup"><span data-stu-id="4ac47-107">This document is intended toohelp you diagnose and fix simple issues.</span></span> <span data-ttu-id="4ac47-108">Není určený toobe náhradní server pro podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4ac47-108">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="4ac47-109">Pokud se hello problém nelze vyřešit pomocí hello pokynů, otevřete žádost o podporu s [Microsoft Azure Nápověda a podpora](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="4ac47-109">If you can't solve hello problem by using hello following guidance, open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="4ac47-110">Adresa tabulky Resolution Protocol (ARP) a protokolu ARP</span><span class="sxs-lookup"><span data-stu-id="4ac47-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="4ac47-111">ARP je protokol vrstvy 2, který je definován v [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="4ac47-111">ARP is a Layer 2 protocol that's defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="4ac47-112">Protokol ARP je použité toomap IP adresu tooan adresy (adresy MAC) sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="4ac47-112">ARP is used toomap an Ethernet address (MAC address) tooan IP address.</span></span>

<span data-ttu-id="4ac47-113">Základě tabulky ARP poskytuje mapování hello IPv4 adresy a adresy MAC pro konkrétní partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="4ac47-113">An ARP table provides a mapping of hello IPv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="4ac47-114">Hello na základě tabulky ARP pro okruh ExpressRoute vztahy poskytuje hello následující informace pro každé rozhraní (primární i sekundární):</span><span class="sxs-lookup"><span data-stu-id="4ac47-114">hello ARP table for an ExpressRoute circuit peering provides hello following information for each interface (primary and secondary):</span></span>

1. <span data-ttu-id="4ac47-115">Mapování adresy IP adresu tooa MAC místní směrovač rozhraní</span><span class="sxs-lookup"><span data-stu-id="4ac47-115">Mapping of an on-premises router interface IP address tooa MAC address</span></span>
2. <span data-ttu-id="4ac47-116">Mapování adresy IP adresu tooa MAC ExpressRoute směrovač rozhraní</span><span class="sxs-lookup"><span data-stu-id="4ac47-116">Mapping of an ExpressRoute router interface IP address tooa MAC address</span></span>
3. <span data-ttu-id="4ac47-117">stáří Hello mapování hello</span><span class="sxs-lookup"><span data-stu-id="4ac47-117">hello age of hello mapping</span></span>

<span data-ttu-id="4ac47-118">Tabulky ARP může pomoct s Probíhá ověřování konfigurace vrstvy 2 a řešení potíží s problémy s připojením k základní vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="4ac47-118">ARP tables can help with validating Layer 2 configuration and with troubleshooting basic Layer 2 connectivity issues.</span></span>

<span data-ttu-id="4ac47-119">Tady je příklad tabulky ARP:</span><span class="sxs-lookup"><span data-stu-id="4ac47-119">Following is an example of an ARP table:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="4ac47-120">Hello následující část obsahuje informace o tom, jak tooview hello tabulky ARP, které jsou vidět hello ExpressRoute hraniční směrovače.</span><span class="sxs-lookup"><span data-stu-id="4ac47-120">hello following section provides information about how tooview hello ARP tables that are seen by hello ExpressRoute edge routers.</span></span>

## <a name="prerequisites-for-using-arp-tables"></a><span data-ttu-id="4ac47-121">Předpoklady pro použití tabulky ARP</span><span class="sxs-lookup"><span data-stu-id="4ac47-121">Prerequisites for using ARP tables</span></span>
<span data-ttu-id="4ac47-122">Ujistěte se, že máte následující hello, než budete pokračovat:</span><span class="sxs-lookup"><span data-stu-id="4ac47-122">Ensure that you have hello following before you continue:</span></span>

* <span data-ttu-id="4ac47-123">Platný okruh ExpressRoute, který je nakonfigurovaný s alespoň jeden partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="4ac47-123">A valid ExpressRoute circuit that's configured with at least one peering.</span></span> <span data-ttu-id="4ac47-124">okruh Hello musí být plně nakonfigurované poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="4ac47-124">hello circuit must be fully configured by hello connectivity provider.</span></span> <span data-ttu-id="4ac47-125">Vy (nebo váš poskytovatel připojení) na musíte nakonfigurovat alespoň jeden z partnerských vztahů hello (Azure privátní, veřejný Azure nebo Microsoft) tento okruh.</span><span class="sxs-lookup"><span data-stu-id="4ac47-125">You (or your connectivity provider) must configure at least one of hello peerings (Azure private, Azure public, or Microsoft) on this circuit.</span></span>
* <span data-ttu-id="4ac47-126">Rozsahy IP adres, které se používají ke konfiguraci partnerských vztahů hello (Azure privátní, veřejný Azure a Microsoft).</span><span class="sxs-lookup"><span data-stu-id="4ac47-126">IP address ranges that are used for configuring hello peerings (Azure private, Azure public, and Microsoft).</span></span> <span data-ttu-id="4ac47-127">Zkontrolujte hello IP adresu přiřazení příklady v hello [stránky požadavky na směrování ExpressRoute](expressroute-routing.md) tooget představu o tom, jak jsou IP adresy namapované toointerfaces na vaše aise a na straně hello ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4ac47-127">Review hello IP address assignment examples in hello [ExpressRoute routing requirements page](expressroute-routing.md) tooget an understanding of how IP addresses are mapped toointerfaces on your aise and on hello ExpressRoute side.</span></span> <span data-ttu-id="4ac47-128">Můžete získat informace o konfiguraci partnerského vztahu hello kontrolou hello [stránku konfigurace partnerského vztahu ExpressRoute](expressroute-howto-routing-classic.md).</span><span class="sxs-lookup"><span data-stu-id="4ac47-128">You can get information about hello peering configuration by reviewing hello [ExpressRoute peering configuration page](expressroute-howto-routing-classic.md).</span></span>
* <span data-ttu-id="4ac47-129">Informace od poskytovatele tým nebo připojení k síťové adresy MAC hello hello rozhraní, které se používají s tyto IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4ac47-129">Information from your networking team or connectivity provider about hello MAC addresses of hello interfaces that are used with these IP addresses.</span></span>
* <span data-ttu-id="4ac47-130">Hello nejnovější modul Windows PowerShell pro Azure (verze 1.50 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="4ac47-130">hello latest Windows PowerShell module for Azure (version 1.50 or later).</span></span>

## <a name="arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="4ac47-131">Tabulky ARP pro váš okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="4ac47-131">ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="4ac47-132">Tato část obsahuje pokyny, jak tooview hello ARP tabulky pro každý typ partnerského vztahu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ac47-132">This section provides instructions about how tooview hello ARP tables for each type of peering by using PowerShell.</span></span> <span data-ttu-id="4ac47-133">Než budete pokračovat, budou vy nebo váš poskytovatel připojení partnerský vztah hello tooconfigure potřebuje.</span><span class="sxs-lookup"><span data-stu-id="4ac47-133">Before you continue, either you or your connectivity provider needs tooconfigure hello peering.</span></span> <span data-ttu-id="4ac47-134">Každý okruh obsahuje dvě cesty (primární i sekundární).</span><span class="sxs-lookup"><span data-stu-id="4ac47-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="4ac47-135">Můžete zkontrolovat hello na základě tabulky ARP pro každou z cest nezávisle.</span><span class="sxs-lookup"><span data-stu-id="4ac47-135">You can check hello ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="4ac47-136">Tabulky ARP pro soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="4ac47-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="4ac47-137">následující rutiny Hello poskytuje hello ARP tabulky pro soukromý partnerský vztah Azure:</span><span class="sxs-lookup"><span data-stu-id="4ac47-137">hello following cmdlet provides hello ARP tables for Azure private peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

<span data-ttu-id="4ac47-138">Toto je ukázkový výstup pro jednu hello cesty:</span><span class="sxs-lookup"><span data-stu-id="4ac47-138">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="4ac47-139">Tabulky ARP pro veřejný partnerský vztah Azure:</span><span class="sxs-lookup"><span data-stu-id="4ac47-139">ARP tables for Azure public peering:</span></span>
<span data-ttu-id="4ac47-140">následující rutiny Hello poskytuje hello ARP tabulky pro veřejný partnerský vztah Azure:</span><span class="sxs-lookup"><span data-stu-id="4ac47-140">hello following cmdlet provides hello ARP tables for Azure public peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

<span data-ttu-id="4ac47-141">Toto je ukázkový výstup pro jednu hello cesty:</span><span class="sxs-lookup"><span data-stu-id="4ac47-141">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="4ac47-142">Toto je ukázkový výstup pro jednu hello cesty:</span><span class="sxs-lookup"><span data-stu-id="4ac47-142">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="4ac47-143">Tabulky ARP pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="4ac47-143">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="4ac47-144">následující rutina Hello poskytuje hello ARP tabulky pro partnerský vztah Microsoftu:</span><span class="sxs-lookup"><span data-stu-id="4ac47-144">hello following cmdlet provides hello ARP tables for Microsoft peering:</span></span>

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


<span data-ttu-id="4ac47-145">Ukázkový výstup je zobrazena níže pro jednu hello cesty:</span><span class="sxs-lookup"><span data-stu-id="4ac47-145">Sample output is shown below for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a><span data-ttu-id="4ac47-146">Jak toouse tyto informace</span><span class="sxs-lookup"><span data-stu-id="4ac47-146">How toouse this information</span></span>
<span data-ttu-id="4ac47-147">Hello tabulky ARP partnerský vztah může být použité toovalidate vrstvy 2 konfigurací a připojením.</span><span class="sxs-lookup"><span data-stu-id="4ac47-147">hello ARP table of a peering can be used toovalidate Layer 2 configuration and connectivity.</span></span> <span data-ttu-id="4ac47-148">Tato část obsahuje přehled o tom, jak vzhledu tabulky ARP v různých scénářích.</span><span class="sxs-lookup"><span data-stu-id="4ac47-148">This section provides an overview of how ARP tables look in different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a><span data-ttu-id="4ac47-149">Na základě tabulky ARP při okruh je ve stavu provozní (očekávaný)</span><span class="sxs-lookup"><span data-stu-id="4ac47-149">ARP table when a circuit is in an operational (expected) state</span></span>
* <span data-ttu-id="4ac47-150">Hello tabulky ARP má záznam pro místní straně hello s platnou adresu IP a MAC a podobně jako položka pro hello straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4ac47-150">hello ARP table has an entry for hello on-premises side with a valid IP and MAC address, and a similar entry for hello Microsoft side.</span></span>
* <span data-ttu-id="4ac47-151">poslední oktet Hello hello místní IP adresy je vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="4ac47-151">hello last octet of hello on-premises IP address is always an odd number.</span></span>
* <span data-ttu-id="4ac47-152">poslední oktetu Hello hello Microsoft IP adresa je vždy sudé číslo.</span><span class="sxs-lookup"><span data-stu-id="4ac47-152">hello last octet of hello Microsoft IP address is always an even number.</span></span>
* <span data-ttu-id="4ac47-153">Hello stejnou adresu MAC se zobrazí na hello straně společnosti Microsoft pro všechny tři partnerské vztahy (primární nebo sekundární).</span><span class="sxs-lookup"><span data-stu-id="4ac47-153">hello same MAC address appears on hello Microsoft side for all three peerings (primary/secondary).</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-hello-connectivity-provider-side-has-problems"></a><span data-ttu-id="4ac47-154">Na základě tabulky ARP, když je na místě nebo když hello poskytovatele připojení straně má potíže s</span><span class="sxs-lookup"><span data-stu-id="4ac47-154">ARP table when it's on-premises or when hello connectivity-provider side has problems</span></span>
 <span data-ttu-id="4ac47-155">Zobrazí se jenom jedna položka v hello tabulky ARP.</span><span class="sxs-lookup"><span data-stu-id="4ac47-155">Only one entry appears in hello ARP table.</span></span> <span data-ttu-id="4ac47-156">Zobrazuje hello mapování mezi adresa MAC hello a hello IP adresu, která se používá na straně Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="4ac47-156">It shows hello mapping between hello MAC address and hello IP address that's used on hello Microsoft side.</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> <span data-ttu-id="4ac47-157">Pokud dojde k problému takto, otevřete podporu požadavků s vaší tooresolve poskytovatele připojení.</span><span class="sxs-lookup"><span data-stu-id="4ac47-157">If you experience an issue like this, open a support request with your connectivity provider tooresolve it.</span></span>
> 
> 

### <a name="arp-table-when-hello-microsoft-side-has-problems"></a><span data-ttu-id="4ac47-158">Na základě tabulky ARP při hello Microsoft straně má potíže s</span><span class="sxs-lookup"><span data-stu-id="4ac47-158">ARP table when hello Microsoft side has problems</span></span>
* <span data-ttu-id="4ac47-159">Neuvidíte na základě tabulky ARP pro partnerský vztah, pokud jsou na straně Microsoft hello problémy.</span><span class="sxs-lookup"><span data-stu-id="4ac47-159">You will not see an ARP table shown for a peering if there are issues on hello Microsoft side.</span></span>
* <span data-ttu-id="4ac47-160">Otevřete žádost o podporu s [Microsoft Azure Nápověda a podpora](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="4ac47-160">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="4ac47-161">Zadejte, že máte potíže s připojením vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="4ac47-161">Specify that you have an issue with Layer 2 connectivity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ac47-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ac47-162">Next steps</span></span>
* <span data-ttu-id="4ac47-163">Ověření konfigurace vrstvy 3 pro váš okruh ExpressRoute:</span><span class="sxs-lookup"><span data-stu-id="4ac47-163">Validate Layer 3 configurations for your ExpressRoute circuit:</span></span>
  * <span data-ttu-id="4ac47-164">Zjištění stavu hello souhrnné toodetermine trasy relací protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="4ac47-164">Get a route summary toodetermine hello state of BGP sessions.</span></span>
  * <span data-ttu-id="4ac47-165">Získání toodetermine tabulky trasy předpon, které jsou ohlášené mezi ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4ac47-165">Get a route table toodetermine which prefixes are advertised across ExpressRoute.</span></span>
* <span data-ttu-id="4ac47-166">Přenos dat ověřte kontrolou bajtů a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="4ac47-166">Validate data transfer by reviewing bytes in and out.</span></span>
* <span data-ttu-id="4ac47-167">Otevřete žádost o podporu s [Microsoft Azure Nápověda a podpora](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud pořád dochází k problémům.</span><span class="sxs-lookup"><span data-stu-id="4ac47-167">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

