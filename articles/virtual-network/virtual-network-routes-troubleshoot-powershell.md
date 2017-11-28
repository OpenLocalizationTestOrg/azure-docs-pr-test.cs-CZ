---
title: "Řešení potíží s trasy – prostředí PowerShell | Microsoft Docs"
description: "Informace o řešení potíží s trasy v modelu nasazení Azure Resource Manager pomocí Azure PowerShell."
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
ms.openlocfilehash: 141e3c571d744470fd07e99538b6e38d4144e8d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-routes-using-azure-powershell"></a><span data-ttu-id="a6887-103">Řešení potíží s postupy pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a6887-103">Troubleshoot routes using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a6887-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a6887-104">Azure Portal</span></span>](virtual-network-routes-troubleshoot-portal.md)
> * [<span data-ttu-id="a6887-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a6887-105">PowerShell</span></span>](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="a6887-106">Pokud dochází k problémům se síťovým připojením do nebo z vaší virtuální počítač Azure (VM), tras, které mohou ovlivňovat vaše datové přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a6887-106">If you are experiencing network connectivity issues to or from your Azure Virtual Machine (VM), routes may be impacting your VM traffic flows.</span></span> <span data-ttu-id="a6887-107">Tento článek obsahuje přehled možností diagnostiky pro směrování, aby mohl pomoct řešit problémy s další.</span><span class="sxs-lookup"><span data-stu-id="a6887-107">This article provides an overview of diagnostics capabilities for routes to help troubleshoot further.</span></span>

<span data-ttu-id="a6887-108">Směrovací tabulky jsou spojeny s podsítěmi a platí na všech síťových rozhraní (NIC) v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="a6887-108">Route tables are associated with subnets and are effective on all network interfaces (NIC) in that subnet.</span></span> <span data-ttu-id="a6887-109">Každé rozhraní sítě můžete použít následující typy trasy:</span><span class="sxs-lookup"><span data-stu-id="a6887-109">The following types of routes can be applied to each network interface:</span></span>

* <span data-ttu-id="a6887-110">**Systémové trasy:** ve výchozím nastavení, má každá podsíť vytvořená ve virtuální síti Azure (VNet) systému směrovací tabulky, které umožňují místní provoz VNet, místní provoz prostřednictvím brány sítě VPN a přenosy z Internetu.</span><span class="sxs-lookup"><span data-stu-id="a6887-110">**System routes:** By default, every subnet created in an Azure Virtual Network (VNet) has system route tables that allow local VNet traffic, on-premises traffic via VPN gateways, and Internet traffic.</span></span> <span data-ttu-id="a6887-111">Systémové trasy existovat také pro peered virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a6887-111">System routes also exist for peered VNets.</span></span>
* <span data-ttu-id="a6887-112">**Trasy protokolu BGP:** rozšíří na rozhraní sítě prostřednictvím ExpressRoute nebo VPN připojení site-to-site.</span><span class="sxs-lookup"><span data-stu-id="a6887-112">**BGP routes:** Propagated to network interfaces through ExpressRoute or site-to-site VPN connections.</span></span> <span data-ttu-id="a6887-113">Další informace o směrování protokolu BGP načtením [protokol BGP s bránami VPN](../vpn-gateway/vpn-gateway-bgp-overview.md) a [přehled ExpressRoute](../expressroute/expressroute-introduction.md) články.</span><span class="sxs-lookup"><span data-stu-id="a6887-113">Learn more about BGP routing by reading the [BGP with VPN gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) and [ExpressRoute overview](../expressroute/expressroute-introduction.md) articles.</span></span>
* <span data-ttu-id="a6887-114">**Trasy definované uživatelem (UDR):** Pokud používáte virtuální zařízení sítě nebo jsou vynucené tunelování provozu do místní sítě prostřednictvím sítě site-to-site VPN, může mít trasy definované uživatelem (udr) přidružené tabulky tras podsítě.</span><span class="sxs-lookup"><span data-stu-id="a6887-114">**User-defined routes (UDR):** If you are using network virtual appliances or are forced-tunneling traffic to an on-premises network via a site-to-site VPN, you may have user-defined routes (UDRs) associated with your subnet route table.</span></span> <span data-ttu-id="a6887-115">Pokud si nejste obeznámeni s udr, přečtěte si [trasy definované uživatelem](virtual-networks-udr-overview.md#user-defined-routes) článku.</span><span class="sxs-lookup"><span data-stu-id="a6887-115">If you're not familiar with UDRs, read the [user-defined routes](virtual-networks-udr-overview.md#user-defined-routes) article.</span></span>

<span data-ttu-id="a6887-116">S různými tras, které lze použít k síťovému rozhraní může být obtížné určit, které agregační trasy jsou platné.</span><span class="sxs-lookup"><span data-stu-id="a6887-116">With the various routes that can be applied to a network interface, it can be difficult to determine which aggregate routes are effective.</span></span> <span data-ttu-id="a6887-117">Pomoc při řešení potíží s připojení k síti virtuálních počítačů, můžete zobrazit všechny efektivní trasy pro rozhraní sítě v modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a6887-117">To help troubleshoot VM network connectivity, you can view all the effective routes for a network interface in the Azure Resource Manager deployment model.</span></span>

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a><span data-ttu-id="a6887-118">Řešení potíží s přenosy virtuálních počítačů pomocí efektivní trasy</span><span class="sxs-lookup"><span data-stu-id="a6887-118">Using Effective Routes to troubleshoot VM traffic flow</span></span>
<span data-ttu-id="a6887-119">Tento článek používá následující scénář jako příklad k ukazují, jak vyřešit efektivní trasy pro síťové rozhraní:</span><span class="sxs-lookup"><span data-stu-id="a6887-119">This article uses the following scenario as an example to illustrate how to troubleshoot the effective routes for a network interface:</span></span>

<span data-ttu-id="a6887-120">Virtuální počítač (*VM1*) připojené k virtuální síti (*VNet1*, předpona: 10.9.0.0/16) nepodaří připojit k VM(VM3) ve virtuální síti nově peered (*VNet3*, předpony 10.10.0.0/16).</span><span class="sxs-lookup"><span data-stu-id="a6887-120">A VM (*VM1*) connected to the VNet (*VNet1*, prefix: 10.9.0.0/16) fails to connect to a VM(VM3) in a newly peered VNet (*VNet3*, prefix 10.10.0.0/16).</span></span> <span data-ttu-id="a6887-121">Neexistují žádné udr nebo BGP směruje u VM1 NIC1 síťové rozhraní připojené k virtuálnímu počítači, se použijí jenom systémové trasy.</span><span class="sxs-lookup"><span data-stu-id="a6887-121">There are no UDRs or BGP routes applied to VM1-NIC1 network interface connected to the VM, only system routes are applied.</span></span>

<span data-ttu-id="a6887-122">Tento článek vysvětluje, jak a zjistěte příčinu selhání připojení pomocí funkce efektivní trasy v modelu nasazení správou prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a6887-122">This article explains how to determine the cause of the connection failure, using effective routes capability in Azure Resource Management deployment model.</span></span>
<span data-ttu-id="a6887-123">Tento příklad používá jenom systémové trasy, stejný postup slouží k určení selhání příchozí a odchozí připojení přes jakýkoli typ trasy.</span><span class="sxs-lookup"><span data-stu-id="a6887-123">While the example uses only system routes, the same steps can be used to determine inbound and outbound connection failures over any route type.</span></span>

> [!NOTE]
> <span data-ttu-id="a6887-124">Pokud virtuální počítač má více než jeden síťový adaptér připojený, zkontrolujte efektivní trasy pro jednotlivé síťové adaptéry diagnostikovat problémy s připojením k síti do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a6887-124">If your VM has more than one NIC attached, check effective routes for each of the NICs to diagnose network connectivity issues to and from a VM.</span></span>
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a><span data-ttu-id="a6887-125">Zobrazit účinné postupy pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a6887-125">View effective routes for a virtual machine</span></span>
<span data-ttu-id="a6887-126">Agregace tras, které se použijí k virtuálnímu počítači najdete proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6887-126">To see the aggregate routes that are applied to a VM, complete the following steps:</span></span>

### <a name="view-effective-routes-for-a-network-interface"></a><span data-ttu-id="a6887-127">Zobrazit účinné postupy pro rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="a6887-127">View effective routes for a network interface</span></span>
<span data-ttu-id="a6887-128">Agregace tras, které se použijí k síťovému rozhraní najdete proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6887-128">To see the aggregate routes that are applied to a network interface, complete the following steps:</span></span>

1. <span data-ttu-id="a6887-129">Spuštění z relace prostředí Azure PowerShell a do Azure.</span><span class="sxs-lookup"><span data-stu-id="a6887-129">Start an Azure PowerShell session and login to Azure.</span></span> <span data-ttu-id="a6887-130">Pokud si nejste obeznámeni s prostředím Azure PowerShell, přečtěte si [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="a6887-130">If you’re not familiar with Azure PowerShell, read the [How to install and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="a6887-131">Následující příkaz vrátí všechny trasy použije k síťovému rozhraní s názvem *VM1 NIC1* ve skupině prostředků *RG1*.</span><span class="sxs-lookup"><span data-stu-id="a6887-131">The following command returns all routes applied to a network interface named *VM1-NIC1* in the resource group *RG1*.</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="a6887-132">Pokud neznáte název síťového rozhraní, zadejte následující příkaz pro načtení názvy všech síťových rozhraní v group.* prostředků</span><span class="sxs-lookup"><span data-stu-id="a6887-132">If you don’t know the name of a network interface, type the following command to retrieve the names of all network interfaces in a resource group.*</span></span>
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   <span data-ttu-id="a6887-133">Tento výstup bude vypadat podobně jako výstup pro každý postup použije na podsíť, které síťový adaptér je připojen k:</span><span class="sxs-lookup"><span data-stu-id="a6887-133">The following output looks similar to the output for each route applied to the subnet the NIC is connected to:</span></span>
   
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
   
   <span data-ttu-id="a6887-134">Všimněte si následující výstup:</span><span class="sxs-lookup"><span data-stu-id="a6887-134">Notice the following in the output:</span></span>
   
   * <span data-ttu-id="a6887-135">**Název**: název efektivní trasy, může být prázdný, pokud není výslovně uvedeno, pro trasy definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="a6887-135">**Name**: Name of the effective route may be empty, unless explicitly specified, for user-defined routes.</span></span> 
   * <span data-ttu-id="a6887-136">**Stav**: označuje stav efektivní trasy.</span><span class="sxs-lookup"><span data-stu-id="a6887-136">**State**: Indicates state of the effective route.</span></span> <span data-ttu-id="a6887-137">Možné hodnoty jsou "Aktivní" nebo "Neplatná"</span><span class="sxs-lookup"><span data-stu-id="a6887-137">Possible values are "Active" or "Invalid"</span></span>
   * <span data-ttu-id="a6887-138">**Adres**: Určuje předpona adresy efektivní trasy v notaci CIDR.</span><span class="sxs-lookup"><span data-stu-id="a6887-138">**AddressPrefixes**: Specifies the address prefix of the effective route in CIDR notation.</span></span> 
   * <span data-ttu-id="a6887-139">**nextHopType**: označuje dalšího přechodu pro danou trasu.</span><span class="sxs-lookup"><span data-stu-id="a6887-139">**nextHopType**: Indicates the next hop for the given route.</span></span> <span data-ttu-id="a6887-140">Možné hodnoty jsou *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, nebo *Null*.</span><span class="sxs-lookup"><span data-stu-id="a6887-140">Possible values are *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, or *Null*.</span></span> <span data-ttu-id="a6887-141">Hodnota *Null* pro **nextHopType** UDR může znamenat neplatná.</span><span class="sxs-lookup"><span data-stu-id="a6887-141">A value of *Null* for **nextHopType** in a UDR may indicate an invalid route.</span></span> <span data-ttu-id="a6887-142">Například pokud **nextHopType** je *VirtualAppliance* a virtuální zařízení sítě virtuálního počítače není zřízený nebo spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="a6887-142">For example, if **nextHopType** is *VirtualAppliance* and the network virtual appliance VM is not in a provisioned/running state.</span></span> <span data-ttu-id="a6887-143">Pokud **nextHopType** je *Brána VPN* a neexistuje žádná brána zřízený/běží v dané virtuální síti, může zneplatní trasy.</span><span class="sxs-lookup"><span data-stu-id="a6887-143">If **nextHopType** is *VPNGateway* and there is no gateway provisioned/running in the given VNet, the route may become invalid.</span></span>
   * <span data-ttu-id="a6887-144">**NextHopIpAddress**: Určuje IP adresu dalšího přechodu efektivní trasy.</span><span class="sxs-lookup"><span data-stu-id="a6887-144">**NextHopIpAddress**: Specifies the IP address of the next hop of the effective route.</span></span>
   
   <span data-ttu-id="a6887-145">Následující příkaz vrátí trasy v snazší zobrazení tabulky:</span><span class="sxs-lookup"><span data-stu-id="a6887-145">The following command returns the routes in an easier to view table:</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   <span data-ttu-id="a6887-146">Následující výstup je některé výstup, přijal scénář popsaný dříve:</span><span class="sxs-lookup"><span data-stu-id="a6887-146">The following output is some of the output received for the scenario described previously:</span></span>
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. <span data-ttu-id="a6887-147">Neexistuje žádná trasa uvedené *WestUS VNet3* virtuální síť (předpony 10.10.0.0/16)** z *WestUS VNet1* (předpony 10.9.0.0/16) ve výstupu z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="a6887-147">There is no route listed to the *WestUS-VNet3* VNet (Prefix 10.10.0.0/16)** from *WestUS-VNet1* (Prefix 10.9.0.0/16) in the output from the previous step.</span></span> <span data-ttu-id="a6887-148">Jak je znázorněno na následujícím obrázku, virtuální síť partnerského vztahu propojení *WestUS VNet3* virtuální sítě je v *odpojeno* stavu.</span><span class="sxs-lookup"><span data-stu-id="a6887-148">As shown in the following picture, the VNet peering link with the *WestUS-VNet3* VNet is in the *Disconnected* state.</span></span>
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    <span data-ttu-id="a6887-149">Obousměrná odkaz pro partnerského vztahu se přeruší, která vysvětluje, proč VM1 se nemohl připojit k VM3 v *WestUS VNet3* virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a6887-149">The bi-directional link for the peering is broken, which explains why VM1 could not connect to VM3 in the *WestUS-VNet3* VNet.</span></span> <span data-ttu-id="a6887-150">Instalační program partnerského vztahu odkaz obousměrný virtuální síť znovu pro *WestUS VNet1* a *WestUS VNet3* virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a6887-150">Setup a bi-directional VNet peering link again for *WestUS-VNet1* and *WestUS-VNet3* VNets.</span></span> <span data-ttu-id="a6887-151">Výstup po správně vytvoření partnerského vztahu propojení virtuální sítě zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="a6887-151">The output returned after the VNet peering link is correctly established follows:</span></span>
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    <span data-ttu-id="a6887-152">Po určení problém, můžete přidat, odebrat, nebo změňte trasy a směrovacích tabulek.</span><span class="sxs-lookup"><span data-stu-id="a6887-152">Once you determine the issue, you can add, remove, or change routes and route tables.</span></span> <span data-ttu-id="a6887-153">Pomocí následujícího příkazu zobrazte seznam příkazů určených k tomu:</span><span class="sxs-lookup"><span data-stu-id="a6887-153">Type the following command to see a list of the commands used to do so:</span></span>
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a><span data-ttu-id="a6887-154">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a6887-154">Considerations</span></span>
<span data-ttu-id="a6887-155">Pokud kontrola seznam tras vrátí mějte na paměti několik akcí:</span><span class="sxs-lookup"><span data-stu-id="a6887-155">A few things to keep in mind when reviewing the list of routes returned:</span></span>

* <span data-ttu-id="a6887-156">Směrování je založena na nejdelší shody předpony (LPM) mezi udr, směrování protokolu BGP a systému.</span><span class="sxs-lookup"><span data-stu-id="a6887-156">Routing is based on Longest Prefix Match (LPM) among UDRs, BGP and system routes.</span></span> <span data-ttu-id="a6887-157">Pokud existuje víc tras se stejnou shodou LPM, pak trasa se vybere na základě původu v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="a6887-157">If there is more than one route with the same LPM match, then a route is selected based on its origin in the following order:</span></span>
  
  * <span data-ttu-id="a6887-158">Trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="a6887-158">User-defined route</span></span>
  * <span data-ttu-id="a6887-159">Trasa protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="a6887-159">BGP route</span></span>
  * <span data-ttu-id="a6887-160">Trasy systému (výchozí)</span><span class="sxs-lookup"><span data-stu-id="a6887-160">System (Default) route</span></span>
    
    <span data-ttu-id="a6887-161">S efektivní směrování uvidí jenom účinné postupy, které jsou založené na všechny dostupné trasy shodou LPM.</span><span class="sxs-lookup"><span data-stu-id="a6887-161">With effective routes, you can only see effective routes that are LPM match based on all the availble routes.</span></span> <span data-ttu-id="a6887-162">Ukazuje, jak se ve skutečnosti vyhodnocují trasy pro daný síťový adaptér, díky tomu mnohem snazší řešení potíží s konkrétní trasy, které mohou ovlivňovat připojení z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a6887-162">By showing how the routes are actually evaluated for a given NIC, this makes it a lot easier to troubleshoot specific routes that may be impacting connectivity to/from your VM.</span></span>
* <span data-ttu-id="a6887-163">Pokud máte udr a odesílání provozu pro virtuální síťové zařízení (hodnocení chyb zabezpečení), s *VirtualAppliance* jako **nextHopType**, ujistěte se, zda je na hodnocení chyb zabezpečení, který provoz přijímá povolena předávání IP nebo dojde ke ztrátě paketů.</span><span class="sxs-lookup"><span data-stu-id="a6887-163">If you have UDRs and are sending traffic to a network virtual appliance (NVA), with *VirtualAppliance* as **nextHopType**, ensure that IP forwarding is enabled on the NVA receiving the traffic or packets are dropped.</span></span> 
* <span data-ttu-id="a6887-164">Pokud vynuceném tunelovém propojení je povoleno, veškerý odchozí internetový provoz se směruje na místní.</span><span class="sxs-lookup"><span data-stu-id="a6887-164">If Forced tunneling is enabled, all outbound Internet traffic will be routed to on-premises.</span></span> <span data-ttu-id="a6887-165">Připojení RDP/SSH z Internetu do virtuální počítač nemusí fungovat s tímto nastavením, v závislosti na tom, jak místní zpracovává tento provoz.</span><span class="sxs-lookup"><span data-stu-id="a6887-165">RDP/SSH from Internet to your VM may not work with this setting, depending on how the on-premises handles this traffic.</span></span> 
  <span data-ttu-id="a6887-166">Můžete povolit vynucené tunelování:</span><span class="sxs-lookup"><span data-stu-id="a6887-166">Forced-tunneling can be enabled:</span></span>
  * <span data-ttu-id="a6887-167">Pokud připojení site-to-site VPN pomocí nastavení trasy definované uživatelem (UDR) s nextHopType jako brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="a6887-167">If using site-to-site VPN, by setting a user-defined route (UDR) with nextHopType as VPN Gateway</span></span>
  * <span data-ttu-id="a6887-168">Pokud je výchozí trasy inzerované přes protokol BGP</span><span class="sxs-lookup"><span data-stu-id="a6887-168">If a default route is advertised over BGP</span></span>
* <span data-ttu-id="a6887-169">Pro virtuální síť partnerského vztahu provoz fungovala správně, systémová trasa s **nextHopType** *VNetPeering* peered VNet předponu rozsahu, musí existovat.</span><span class="sxs-lookup"><span data-stu-id="a6887-169">For VNet peering traffic to work correctly, a system route with **nextHopType** *VNetPeering* must exist for the peered VNet’s prefix range.</span></span> <span data-ttu-id="a6887-170">Pokud tyto trasy neexistuje a partnerského vztahu propojení virtuální sítě vypadá v pořádku:</span><span class="sxs-lookup"><span data-stu-id="a6887-170">If such a route doesn’t exist and the VNet peering link looks OK:</span></span>
  * <span data-ttu-id="a6887-171">Počkejte několik sekund a opakovat, pokud se jedná o nově vytvořeným partnerského vztahu odkaz.</span><span class="sxs-lookup"><span data-stu-id="a6887-171">Wait a few seconds and retry if it's a newly established peering link.</span></span> <span data-ttu-id="a6887-172">Někdy trvá déle šířil trasy na všech síťových rozhraní v podsíti.</span><span class="sxs-lookup"><span data-stu-id="a6887-172">It occasionally takes longer to propagate routes to all the network interfaces in a subnet.</span></span>
  * <span data-ttu-id="a6887-173">Skupina zabezpečení sítě (NSG) pravidla, které mohou ovlivňovat tok provozu.</span><span class="sxs-lookup"><span data-stu-id="a6887-173">Network Security Group (NSG) rules may be impacting the traffic flows.</span></span> <span data-ttu-id="a6887-174">Další informace najdete v tématu [odstraňování skupin zabezpečení sítě](virtual-network-nsg-troubleshoot-powershell.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a6887-174">For more information, see the [Troubleshoot Network Security Groups](virtual-network-nsg-troubleshoot-powershell.md) article.</span></span>

