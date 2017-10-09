---
title: "aaaTroubleshoot trasy – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot směruje v modelu nasazení Azure Resource Manager hello pomocí Azure PowerShell."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 7a07806df5c1d0caee921187e6ad29f6755ab535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-azure-powershell"></a><span data-ttu-id="d8f0c-103">Řešení potíží s postupy pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d8f0c-103">Troubleshoot routes using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d8f0c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d8f0c-104">Azure Portal</span></span>](virtual-network-routes-troubleshoot-portal.md)
> * [<span data-ttu-id="d8f0c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d8f0c-105">PowerShell</span></span>](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="d8f0c-106">Máte-li tooor problémy síťové připojení z vaší virtuální počítač Azure (VM), tras, které mohou ovlivňovat vaše datové přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-106">If you are experiencing network connectivity issues tooor from your Azure Virtual Machine (VM), routes may be impacting your VM traffic flows.</span></span> <span data-ttu-id="d8f0c-107">Tento článek obsahuje přehled možností diagnostiky pro trasy toohelp pokračovat v řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-107">This article provides an overview of diagnostics capabilities for routes toohelp troubleshoot further.</span></span>

<span data-ttu-id="d8f0c-108">Směrovací tabulky jsou spojeny s podsítěmi a platí na všech síťových rozhraní (NIC) v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-108">Route tables are associated with subnets and are effective on all network interfaces (NIC) in that subnet.</span></span> <span data-ttu-id="d8f0c-109">následující typy tras Hello může být použité tooeach síťové rozhraní:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-109">hello following types of routes can be applied tooeach network interface:</span></span>

* <span data-ttu-id="d8f0c-110">**Systémové trasy:** ve výchozím nastavení, má každá podsíť vytvořená ve virtuální síti Azure (VNet) systému směrovací tabulky, které umožňují místní provoz VNet, místní provoz prostřednictvím brány sítě VPN a přenosy z Internetu.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-110">**System routes:** By default, every subnet created in an Azure Virtual Network (VNet) has system route tables that allow local VNet traffic, on-premises traffic via VPN gateways, and Internet traffic.</span></span> <span data-ttu-id="d8f0c-111">Systémové trasy existovat také pro peered virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-111">System routes also exist for peered VNets.</span></span>
* <span data-ttu-id="d8f0c-112">**Trasy protokolu BGP:** rozšíří toonetwork rozhraní prostřednictvím ExpressRoute nebo VPN připojení site-to-site.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-112">**BGP routes:** Propagated toonetwork interfaces through ExpressRoute or site-to-site VPN connections.</span></span> <span data-ttu-id="d8f0c-113">Další informace o směrování protokolu BGP načtením hello [protokol BGP s bránami VPN](../vpn-gateway/vpn-gateway-bgp-overview.md) a [přehled ExpressRoute](../expressroute/expressroute-introduction.md) články.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-113">Learn more about BGP routing by reading hello [BGP with VPN gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) and [ExpressRoute overview](../expressroute/expressroute-introduction.md) articles.</span></span>
* <span data-ttu-id="d8f0c-114">**Trasy definované uživatelem (UDR):** použití virtuálních zařízení sítě nebo jsou vynutit tunelové propojení provoz tooan do místní sítě prostřednictvím sítě site-to-site VPN, může mít trasy definované uživatelem (udr) přidružené tabulky tras podsítě.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-114">**User-defined routes (UDR):** If you are using network virtual appliances or are forced-tunneling traffic tooan on-premises network via a site-to-site VPN, you may have user-defined routes (UDRs) associated with your subnet route table.</span></span> <span data-ttu-id="d8f0c-115">Pokud si nejste obeznámeni s udr, přečtěte si hello [trasy definované uživatelem](virtual-networks-udr-overview.md#user-defined-routes) článku.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-115">If you're not familiar with UDRs, read hello [user-defined routes](virtual-networks-udr-overview.md#user-defined-routes) article.</span></span>

<span data-ttu-id="d8f0c-116">S hello různé trasy, které můžou být použité tooa síťového rozhraní, může být obtížné toodetermine které agregační trasy jsou platné.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-116">With hello various routes that can be applied tooa network interface, it can be difficult toodetermine which aggregate routes are effective.</span></span> <span data-ttu-id="d8f0c-117">toohelp řešení potíží s připojení k síti virtuálních počítačů, můžete zobrazit všechny hello efektivní trasy pro rozhraní sítě v modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-117">toohelp troubleshoot VM network connectivity, you can view all hello effective routes for a network interface in hello Azure Resource Manager deployment model.</span></span>

## <a name="using-effective-routes-tootroubleshoot-vm-traffic-flow"></a><span data-ttu-id="d8f0c-118">Pomocí tok přenosů dat virtuálního počítače tootroubleshoot efektivní trasy</span><span class="sxs-lookup"><span data-stu-id="d8f0c-118">Using Effective Routes tootroubleshoot VM traffic flow</span></span>
<span data-ttu-id="d8f0c-119">Tento článek používá hello podle scénáře jako tooillustrate příklad, jak tootroubleshoot hello platné tras pro síťové rozhraní:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-119">This article uses hello following scenario as an example tooillustrate how tootroubleshoot hello effective routes for a network interface:</span></span>

<span data-ttu-id="d8f0c-120">Virtuální počítač (*VM1*) připojené toohello virtuální síť (*VNet1*, předpona: 10.9.0.0/16) selže tooconnect tooa VM(VM3) ve virtuální síti nově peered (*VNet3*, předpony 10.10.0.0/16).</span><span class="sxs-lookup"><span data-stu-id="d8f0c-120">A VM (*VM1*) connected toohello VNet (*VNet1*, prefix: 10.9.0.0/16) fails tooconnect tooa VM(VM3) in a newly peered VNet (*VNet3*, prefix 10.10.0.0/16).</span></span> <span data-ttu-id="d8f0c-121">Neexistují žádné udr nebo BGP směruje použité tooVM1 NIC1 síťové rozhraní připojené toohello virtuálních počítačů, se použijí jenom systémové trasy.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-121">There are no UDRs or BGP routes applied tooVM1-NIC1 network interface connected toohello VM, only system routes are applied.</span></span>

<span data-ttu-id="d8f0c-122">Tento článek vysvětluje, jak toodetermine hello způsobit selhání připojení hello, pomocí funkce efektivní trasy v modelu nasazení správou prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-122">This article explains how toodetermine hello cause of hello connection failure, using effective routes capability in Azure Resource Management deployment model.</span></span>
<span data-ttu-id="d8f0c-123">Příklad hello používá jenom systémové trasy, hello stejný postup lze použít toodetermine selhání příchozí a odchozí připojení přes jakýkoli typ trasy.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-123">While hello example uses only system routes, hello same steps can be used toodetermine inbound and outbound connection failures over any route type.</span></span>

> [!NOTE]
> <span data-ttu-id="d8f0c-124">Pokud virtuální počítač má více než jeden síťový adaptér připojený, zkontrolujte efektivní trasy pro každou hello tooand problémy síťové adaptéry toodiagnose síťové připojení z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-124">If your VM has more than one NIC attached, check effective routes for each of hello NICs toodiagnose network connectivity issues tooand from a VM.</span></span>
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a><span data-ttu-id="d8f0c-125">Zobrazit účinné postupy pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="d8f0c-125">View effective routes for a virtual machine</span></span>
<span data-ttu-id="d8f0c-126">toosee hello agregační tras, které jsou použité tooa virtuálních počítačů, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-126">toosee hello aggregate routes that are applied tooa VM, complete hello following steps:</span></span>

### <a name="view-effective-routes-for-a-network-interface"></a><span data-ttu-id="d8f0c-127">Zobrazit účinné postupy pro rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="d8f0c-127">View effective routes for a network interface</span></span>
<span data-ttu-id="d8f0c-128">toosee hello agregační tras, které jsou použité tooa síťového rozhraní, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-128">toosee hello aggregate routes that are applied tooa network interface, complete hello following steps:</span></span>

1. <span data-ttu-id="d8f0c-129">Spusťte tooAzure relaci a přihlaste se prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-129">Start an Azure PowerShell session and login tooAzure.</span></span> <span data-ttu-id="d8f0c-130">Pokud si nejste obeznámeni s prostředím Azure PowerShell, přečtěte si hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-130">If you’re not familiar with Azure PowerShell, read hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="d8f0c-131">Hello následující příkaz vrátí všechny trasy použít tooa síťové rozhraní s názvem *VM1 NIC1* ve skupině prostředků hello *RG1*.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-131">hello following command returns all routes applied tooa network interface named *VM1-NIC1* in hello resource group *RG1*.</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="d8f0c-132">Pokud neznáte název hello síťového rozhraní, zadejte následující příkaz tooretrieve hello názvy všech síťových rozhraní v group.* prostředků hello</span><span class="sxs-lookup"><span data-stu-id="d8f0c-132">If you don’t know hello name of a network interface, type hello following command tooretrieve hello names of all network interfaces in a resource group.*</span></span>
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   <span data-ttu-id="d8f0c-133">Hello následující výstup vypadá podobně jako výstup toohello pro každý trasy použít toohello podsíť hello síťový adaptér je připojen k:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-133">hello following output looks similar toohello output for each route applied toohello subnet hello NIC is connected to:</span></span>
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   <span data-ttu-id="d8f0c-134">Všimněte si následujících hello ve výstupu hello:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-134">Notice hello following in hello output:</span></span>
   
   * <span data-ttu-id="d8f0c-135">**Název**: název platná směrovací hello může být prázdný, pokud není výslovně uvedeno, pro trasy definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-135">**Name**: Name of hello effective route may be empty, unless explicitly specified, for user-defined routes.</span></span> 
   * <span data-ttu-id="d8f0c-136">**Stav**: označuje stav platná směrovací hello.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-136">**State**: Indicates state of hello effective route.</span></span> <span data-ttu-id="d8f0c-137">Možné hodnoty jsou "Aktivní" nebo "Neplatná"</span><span class="sxs-lookup"><span data-stu-id="d8f0c-137">Possible values are "Active" or "Invalid"</span></span>
   * <span data-ttu-id="d8f0c-138">**Adres**: Určuje předponu adresy hello hello efektivní trasy v notaci CIDR.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-138">**AddressPrefixes**: Specifies hello address prefix of hello effective route in CIDR notation.</span></span> 
   * <span data-ttu-id="d8f0c-139">**nextHopType**: označuje hello další segment pro danou trasu hello.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-139">**nextHopType**: Indicates hello next hop for hello given route.</span></span> <span data-ttu-id="d8f0c-140">Možné hodnoty jsou *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, nebo *Null*.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-140">Possible values are *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, or *Null*.</span></span> <span data-ttu-id="d8f0c-141">Hodnota *Null* pro **nextHopType** UDR může znamenat neplatná.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-141">A value of *Null* for **nextHopType** in a UDR may indicate an invalid route.</span></span> <span data-ttu-id="d8f0c-142">Například pokud **nextHopType** je *VirtualAppliance* a hello sítě virtuální zařízení virtuálního počítače není zřízený nebo spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-142">For example, if **nextHopType** is *VirtualAppliance* and hello network virtual appliance VM is not in a provisioned/running state.</span></span> <span data-ttu-id="d8f0c-143">Pokud **nextHopType** je *Brána VPN* a neexistuje žádná brána zřízený/běží v hello zadané virtuální síti, může zneplatní hello trasy.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-143">If **nextHopType** is *VPNGateway* and there is no gateway provisioned/running in hello given VNet, hello route may become invalid.</span></span>
   * <span data-ttu-id="d8f0c-144">**NextHopIpAddress**: Určuje hello IP adresa dalšího směrování hello hello efektivní trasy.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-144">**NextHopIpAddress**: Specifies hello IP address of hello next hop of hello effective route.</span></span>
   
   <span data-ttu-id="d8f0c-145">Hello následující příkaz vrátí hello trasy v jednodušší tooview tabulce:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-145">hello following command returns hello routes in an easier tooview table:</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   <span data-ttu-id="d8f0c-146">Hello následující výstup je některý obdržel hello scénář popsaný dřív výstup hello:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-146">hello following output is some of hello output received for hello scenario described previously:</span></span>
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. <span data-ttu-id="d8f0c-147">Neexistuje žádná trasa uvedené toohello *WestUS VNet3* virtuální síť (předpony 10.10.0.0/16)** z *WestUS VNet1* (předpony 10.9.0.0/16) ve výstupu hello hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-147">There is no route listed toohello *WestUS-VNet3* VNet (Prefix 10.10.0.0/16)** from *WestUS-VNet1* (Prefix 10.9.0.0/16) in hello output from hello previous step.</span></span> <span data-ttu-id="d8f0c-148">Jak ukazuje následující obrázek hello, hello partnerského vztahu propojení virtuální sítě s hello *WestUS VNet3* virtuální sítě je v hello *odpojeno* stavu.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-148">As shown in hello following picture, hello VNet peering link with hello *WestUS-VNet3* VNet is in hello *Disconnected* state.</span></span>
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    <span data-ttu-id="d8f0c-149">Hello obousměrný odkaz pro partnerský vztah hello se přeruší, která vysvětluje, proč VM1 se nemohl připojit tooVM3 v hello *WestUS VNet3* virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-149">hello bi-directional link for hello peering is broken, which explains why VM1 could not connect tooVM3 in hello *WestUS-VNet3* VNet.</span></span> <span data-ttu-id="d8f0c-150">Instalační program partnerského vztahu odkaz obousměrný virtuální síť znovu pro *WestUS VNet1* a *WestUS VNet3* virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-150">Setup a bi-directional VNet peering link again for *WestUS-VNet1* and *WestUS-VNet3* VNets.</span></span> <span data-ttu-id="d8f0c-151">výstup Hello po správně vytvoření partnerského vztahu propojení virtuální sítě hello takto:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-151">hello output returned after hello VNet peering link is correctly established follows:</span></span>
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    <span data-ttu-id="d8f0c-152">Po určení hello problém, můžete přidat, odebrat, nebo změňte trasy a směrovacích tabulek.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-152">Once you determine hello issue, you can add, remove, or change routes and route tables.</span></span> <span data-ttu-id="d8f0c-153">Zadejte následující příkaz toosee seznam hello příkazy používané toodo tak hello:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-153">Type hello following command toosee a list of hello commands used toodo so:</span></span>
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a><span data-ttu-id="d8f0c-154">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8f0c-154">Considerations</span></span>
<span data-ttu-id="d8f0c-155">Vrátí pár věcí tookeep pamatujte při revizi hello seznam tras:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-155">A few things tookeep in mind when reviewing hello list of routes returned:</span></span>

* <span data-ttu-id="d8f0c-156">Směrování je založena na nejdelší shody předpony (LPM) mezi udr, směrování protokolu BGP a systému.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-156">Routing is based on Longest Prefix Match (LPM) among UDRs, BGP and system routes.</span></span> <span data-ttu-id="d8f0c-157">Pokud existuje víc tras s hello stejné LPM shodují, pak trasa se vybere na základě původu v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-157">If there is more than one route with hello same LPM match, then a route is selected based on its origin in hello following order:</span></span>
  
  * <span data-ttu-id="d8f0c-158">Trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="d8f0c-158">User-defined route</span></span>
  * <span data-ttu-id="d8f0c-159">Trasa protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="d8f0c-159">BGP route</span></span>
  * <span data-ttu-id="d8f0c-160">Trasy systému (výchozí)</span><span class="sxs-lookup"><span data-stu-id="d8f0c-160">System (Default) route</span></span>
    
    <span data-ttu-id="d8f0c-161">S efektivní směrování uvidí jenom účinné postupy, které jsou založené na všechny dostupné trasy hello shodou LPM.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-161">With effective routes, you can only see effective routes that are LPM match based on all hello availble routes.</span></span> <span data-ttu-id="d8f0c-162">Ukazuje, jak se ve skutečnosti vyhodnocují hello trasy pro daný síťový adaptér, proto je mnohem snazší konkrétní trasy tootroubleshoot, které mohou ovlivňovat připojení z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-162">By showing how hello routes are actually evaluated for a given NIC, this makes it a lot easier tootroubleshoot specific routes that may be impacting connectivity to/from your VM.</span></span>
* <span data-ttu-id="d8f0c-163">Pokud máte udr a odesílání přenosů tooa sítě virtuální zařízení (hodnocení chyb zabezpečení), s *VirtualAppliance* jako **nextHopType**, ujistěte se, zda je povoleno předávání IP na provoz přijímající hello hello hodnocení chyb zabezpečení nebo dojde ke ztrátě paketů.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-163">If you have UDRs and are sending traffic tooa network virtual appliance (NVA), with *VirtualAppliance* as **nextHopType**, ensure that IP forwarding is enabled on hello NVA receiving hello traffic or packets are dropped.</span></span> 
* <span data-ttu-id="d8f0c-164">Pokud vynuceném tunelovém propojení je povoleno, bude veškerý odchozí internetový provoz směrovaný tooon místní.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-164">If Forced tunneling is enabled, all outbound Internet traffic will be routed tooon-premises.</span></span> <span data-ttu-id="d8f0c-165">Připojení RDP/SSH z Internetu tooyour, kterou virtuální počítač nemusí fungovat s tímto nastavením, v závislosti na tom, jak hello místní zpracovává tento provoz.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-165">RDP/SSH from Internet tooyour VM may not work with this setting, depending on how hello on-premises handles this traffic.</span></span> 
  <span data-ttu-id="d8f0c-166">Můžete povolit vynucené tunelování:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-166">Forced-tunneling can be enabled:</span></span>
  * <span data-ttu-id="d8f0c-167">Pokud připojení site-to-site VPN pomocí nastavení trasy definované uživatelem (UDR) s nextHopType jako brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="d8f0c-167">If using site-to-site VPN, by setting a user-defined route (UDR) with nextHopType as VPN Gateway</span></span>
  * <span data-ttu-id="d8f0c-168">Pokud je výchozí trasy inzerované přes protokol BGP</span><span class="sxs-lookup"><span data-stu-id="d8f0c-168">If a default route is advertised over BGP</span></span>
* <span data-ttu-id="d8f0c-169">Pro virtuální síť správně, partnerský vztah toowork provozu systému trasa se **nextHopType** *VNetPeering* pro hello peered rozsahu předpony virtuální sítě, musí existovat.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-169">For VNet peering traffic toowork correctly, a system route with **nextHopType** *VNetPeering* must exist for hello peered VNet’s prefix range.</span></span> <span data-ttu-id="d8f0c-170">Pokud tyto trasy neexistuje a hello partnerský vztah virtuální síť odkaz vypadá v pořádku:</span><span class="sxs-lookup"><span data-stu-id="d8f0c-170">If such a route doesn’t exist and hello VNet peering link looks OK:</span></span>
  * <span data-ttu-id="d8f0c-171">Počkejte několik sekund a opakovat, pokud se jedná o nově vytvořeným partnerského vztahu odkaz.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-171">Wait a few seconds and retry if it's a newly established peering link.</span></span> <span data-ttu-id="d8f0c-172">Někdy trvá déle, toopropagate trasy tooall hello síťových rozhraní v podsíti.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-172">It occasionally takes longer toopropagate routes tooall hello network interfaces in a subnet.</span></span>
  * <span data-ttu-id="d8f0c-173">Skupina zabezpečení sítě (NSG) pravidla, které mohou ovlivňovat hello přenosové toky.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-173">Network Security Group (NSG) rules may be impacting hello traffic flows.</span></span> <span data-ttu-id="d8f0c-174">Další informace najdete v tématu hello [odstraňování skupin zabezpečení sítě](virtual-network-nsg-troubleshoot-powershell.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d8f0c-174">For more information, see hello [Troubleshoot Network Security Groups](virtual-network-nsg-troubleshoot-powershell.md) article.</span></span>

