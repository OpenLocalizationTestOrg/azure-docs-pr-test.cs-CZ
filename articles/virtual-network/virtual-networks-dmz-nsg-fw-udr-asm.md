---
title: "Příklad DMZ – sestavení DMZ k ochraně sítě s bránu Firewall, UDR a NSG | Microsoft Docs"
description: "Sestavení DMZ s bránou Firewall, uživatelem definované směrování (UDR) a skupiny zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: fdb3c5cbd3acee90386352c6f180a71aa81f54fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a><span data-ttu-id="58ce7-103">Příklad 3 – vytvoření DMZ k ochraně sítě s bránu Firewall, UDR a NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-103">Example 3 – Build a DMZ to Protect Networks with a Firewall, UDR, and NSG</span></span>
<span data-ttu-id="58ce7-104">[Návrat na stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="58ce7-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="58ce7-105">Tento příklad vytvoří DMZ s bránou firewall, čtyři servery windows, uživatele definované směrování, předávání IP adres a skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="58ce7-105">This example will create a DMZ with a firewall, four windows servers, User Defined Routing, IP Forwarding, and Network Security Groups.</span></span> <span data-ttu-id="58ce7-106">Je také provede každou z relevantních příkazů, které poskytují podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="58ce7-106">It will also walk through each of the relevant commands to provide a deeper understanding of each step.</span></span> <span data-ttu-id="58ce7-107">Je také části provoz scénář zajistit podrobné krok za krokem, jak se provoz pokračuje prostřednictvím vrstev obrany v hraniční síti.</span><span class="sxs-lookup"><span data-stu-id="58ce7-107">There is also a Traffic Scenario section to provide an in-depth step-by-step how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="58ce7-108">Nakonec v odkazy na části je kompletní kód a pokyny k vytvoření tohoto prostředí pro testování a experimentovat s různými scénáři.</span><span class="sxs-lookup"><span data-stu-id="58ce7-108">Finally, in the references section is the complete code and instruction to build this environment to test and experiment with various scenarios.</span></span> 

<span data-ttu-id="58ce7-109">![DMZ obousměrně s hodnocení chyb zabezpečení, NSG a UDR][1]</span><span class="sxs-lookup"><span data-stu-id="58ce7-109">![Bi-directional DMZ with NVA, NSG, and UDR][1]</span></span>

## <a name="environment-setup"></a><span data-ttu-id="58ce7-110">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="58ce7-110">Environment Setup</span></span>
<span data-ttu-id="58ce7-111">V tomto příkladu je odběr, který obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="58ce7-111">In this example there is a subscription that contains the following:</span></span>

* <span data-ttu-id="58ce7-112">Tři cloudové služby: "SecSvc001", "FrontEnd001" a "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="58ce7-112">Three cloud services: “SecSvc001”, “FrontEnd001”, and “BackEnd001”</span></span>
* <span data-ttu-id="58ce7-113">Virtuální síť "CorpNetwork", s tři podsítě: "SecNet", "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="58ce7-113">A Virtual Network “CorpNetwork”, with three subnets: “SecNet”, “FrontEnd”, and “BackEnd”</span></span>
* <span data-ttu-id="58ce7-114">Virtuální zařízení sítě, v tomto příkladu bránu firewall, připojený k podsíti SecNet</span><span class="sxs-lookup"><span data-stu-id="58ce7-114">A network virtual appliance, in this example a firewall, connected to the SecNet subnet</span></span>
* <span data-ttu-id="58ce7-115">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="58ce7-115">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="58ce7-116">Dva windows servery, které představují aplikace zpět ukončení servery ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="58ce7-116">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="58ce7-117">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="58ce7-117">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="58ce7-118">V části odkazy níže je skript prostředí PowerShell, který bude vytvořit většinu prostředí popsané výše.</span><span class="sxs-lookup"><span data-stu-id="58ce7-118">In the references section below there is a PowerShell script that will build most of the environment described above.</span></span> <span data-ttu-id="58ce7-119">Vytváření virtuálních počítačů a virtuálních sítí, i když provádějí ukázkový skript, nejsou podrobně popsané v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-119">Building the VMs and Virtual Networks, although are done by the example script, are not described in detail in this document.</span></span>

<span data-ttu-id="58ce7-120">K vytvoření prostředí:</span><span class="sxs-lookup"><span data-stu-id="58ce7-120">To build the environment:</span></span>

1. <span data-ttu-id="58ce7-121">Uložte soubor xml konfigurace sítě v oddíle odkazy (aktualizovat název, umístění a IP adresy, které odpovídají danému scénáři)</span><span class="sxs-lookup"><span data-stu-id="58ce7-121">Save the network config xml file included in the references section (updated with names, location, and IP addresses to match the given scenario)</span></span>
2. <span data-ttu-id="58ce7-122">Aktualizace uživatelské proměnné ve skriptu tak, aby odpovídaly prostředí, ve kterém je skript ke spouštění (odběry, názvy služeb atd.)</span><span class="sxs-lookup"><span data-stu-id="58ce7-122">Update the user variables in the script to match the environment the script is to be run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="58ce7-123">Spusťte skript v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="58ce7-123">Execute the script in PowerShell</span></span>

<span data-ttu-id="58ce7-124">**Poznámka:**: oblasti označený ve skriptu PowerShell musí odpovídat oblasti označeny v souboru xml konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="58ce7-124">**Note**: The region signified in the PowerShell script must match the region signified in the network configuration xml file.</span></span>

<span data-ttu-id="58ce7-125">Po úspěšném spuštění skriptu mohou být přijata po skriptu takto:</span><span class="sxs-lookup"><span data-stu-id="58ce7-125">Once the script runs successfully the following post-script steps may be taken:</span></span>

1. <span data-ttu-id="58ce7-126">Nastavit pravidla brány firewall, najdete v následující části s názvem: popis pravidla brány Firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-126">Set up the firewall rules, this is covered in the section below titled: Firewall Rule Description.</span></span>
2. <span data-ttu-id="58ce7-127">Volitelně můžete v části odkazy jsou dva skripty k nastavení webového serveru a aplikačního serveru s jednoduchou webovou aplikaci umožňující testování s touto konfigurací DMZ.</span><span class="sxs-lookup"><span data-stu-id="58ce7-127">Optionally in the references section are two scripts to set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span>

<span data-ttu-id="58ce7-128">Jakmile se skript spustí úspěšně bránu firewall, která pravidla bude třeba provést, najdete v části s názvem: pravidla brány Firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-128">Once the script runs successfully the firewall rules will need to be completed, this is covered in the section titled: Firewall Rules.</span></span>

## <a name="user-defined-routing-udr"></a><span data-ttu-id="58ce7-129">Uživatelem definované směrování (UDR)</span><span class="sxs-lookup"><span data-stu-id="58ce7-129">User Defined Routing (UDR)</span></span>
<span data-ttu-id="58ce7-130">Ve výchozím nastavení následující systémové trasy, které jsou definované jako:</span><span class="sxs-lookup"><span data-stu-id="58ce7-130">By default, the following system routes are defined as:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

<span data-ttu-id="58ce7-131">VNETLocal je vždy prefix(es) definovaných adresních sítě vnet pro tuto konkrétní síť (ie se změní z virtuální sítě do virtuální sítě v závislosti na tom, jak je definována každý konkrétní virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="58ce7-131">The VNETLocal is always the defined address prefix(es) of the VNet for that specific network (ie it will change from VNet to VNet depending on how each specific VNet is defined).</span></span> <span data-ttu-id="58ce7-132">Zbývající systémové trasy statické a výchozí jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="58ce7-132">The remaining system routes are static and default as above.</span></span>

<span data-ttu-id="58ce7-133">Jako prioritu trasy se zpracovávají prostřednictvím metody nejdelší shody předpony (LPM), proto by nejvíce trasy v tabulce platí pro danou cílovou adresu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-133">As for priority, routes are processed via the Longest Prefix Match (LPM) method, thus the most specific route in the table would apply to a given destination address.</span></span>

<span data-ttu-id="58ce7-134">Proto by provozu (třeba k serveru DNS01 10.0.2.4) určený pro místní síť (10.0.0.0/16) směrovat přes síť VNet do cíle z důvodu 10.0.0.0/16 trasy.</span><span class="sxs-lookup"><span data-stu-id="58ce7-134">Therefore, traffic (for example to the DNS01 server, 10.0.2.4) intended for the local network (10.0.0.0/16) would be routed across the VNet to its destination due to the 10.0.0.0/16 route.</span></span> <span data-ttu-id="58ce7-135">Jinými slovy pro 10.0.2.4, 10.0.0.0/16 trasy, která je nejvíce trasy, i když 10.0.0.0/8 a 0.0.0.0/0 také může použít, ale vzhledem k tomu, že jsou menší konkrétní neovlivňují tento provoz.</span><span class="sxs-lookup"><span data-stu-id="58ce7-135">In other words, for 10.0.2.4, the 10.0.0.0/16 route is the most specific route, even though the 10.0.0.0/8 and 0.0.0.0/0 also could apply, but since they are less specific they don’t affect this traffic.</span></span> <span data-ttu-id="58ce7-136">Proto provoz do 10.0.2.4 by mít dalšího směrování místní sítě vnet a jednoduše směrovat do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="58ce7-136">Thus the traffic to 10.0.2.4 would have a next hop of the local VNet and simply route to the destination.</span></span>

<span data-ttu-id="58ce7-137">Pokud se provoz určený pro 10.1.1.1 například, 10.0.0.0/16 trasy, která nebude platit, ale 10.0.0.0/8 by nejvíce konkrétní a provoz by to byl vyřazen ("černé dírkového"), protože dalšího směrování je Null.</span><span class="sxs-lookup"><span data-stu-id="58ce7-137">If traffic was intended for 10.1.1.1 for example, the 10.0.0.0/16 route wouldn’t apply, but the 10.0.0.0/8 would be the most specific, and this the traffic would be dropped (“black holed”) since the next hop is Null.</span></span> 

<span data-ttu-id="58ce7-138">Pokud k některému z předpony hodnotu Null nebo VNETLocal předpony nepoužil, cíl, pak by postupujte podle nejméně specifická směrování, 0.0.0.0/0 a připojit k Internetu jako další segment a proto si Azure a internet okraj.</span><span class="sxs-lookup"><span data-stu-id="58ce7-138">If the destination didn’t apply to any of the Null prefixes or the VNETLocal prefixes, then it would follow the least specific route, 0.0.0.0/0 and be routed out to the Internet as the next hop and thus out Azure’s internet edge.</span></span>

<span data-ttu-id="58ce7-139">Pokud existují dva identické předpony v tabulce směrování, zde je v pořadí podle preference podle atributu "zdroje" trasy:</span><span class="sxs-lookup"><span data-stu-id="58ce7-139">If there are two identical prefixes in the route table, the following is the order of preference based on the routes “source” attribute:</span></span>

1. <span data-ttu-id="58ce7-140">"VirtualAppliance" = trasu definovaná uživatelem ručně přidat do tabulky</span><span class="sxs-lookup"><span data-stu-id="58ce7-140">"VirtualAppliance" = A User Defined Route manually added to the table</span></span>
2. <span data-ttu-id="58ce7-141">"Brána VPN" = dynamické směrování protokolu BGP (při použití s hybridní sítě), přidal protokol dynamické sítě, tyto trasy v průběhu času mění jako protokol dynamické automaticky odráží změny v peered sítě</span><span class="sxs-lookup"><span data-stu-id="58ce7-141">“VPNGateway” = A Dynamic Route (BGP when used with hybrid networks), added by a dynamic network protocol, these routes may change over time as the dynamic protocol automatically reflects changes in peered network</span></span>
3. <span data-ttu-id="58ce7-142">"Výchozí" = systémové trasy, místní virtuální síť a statické záznamy, jak je znázorněno v předchozí tabulce směrování.</span><span class="sxs-lookup"><span data-stu-id="58ce7-142">“Default” = The System Routes, the local VNet and the static entries as shown in the route table above.</span></span>

> [!NOTE]
> <span data-ttu-id="58ce7-143">Teď můžete použít uživatele definované směrování (UDR) s ExpressRoute a VPN Gateway vynutit odchozí a příchozí provoz směrovat na virtuální síťové zařízení (hodnocení chyb zabezpečení) mezi různými místy.</span><span class="sxs-lookup"><span data-stu-id="58ce7-143">You can now use User Defined Routing (UDR) with ExpressRoute and VPN Gateways to force outbound and inbound cross-premises traffic to be routed to a network virtual appliance (NVA).</span></span>
> 
> 

#### <a name="creating-the-local-routes"></a><span data-ttu-id="58ce7-144">Vytváření místní trasy</span><span class="sxs-lookup"><span data-stu-id="58ce7-144">Creating the local routes</span></span>
<span data-ttu-id="58ce7-145">V tomto příkladu jsou potřeba dvou směrovacích tabulek, jeden pro každé podsítě front-endové a back-end.</span><span class="sxs-lookup"><span data-stu-id="58ce7-145">In this example, two routing tables are needed, one each for the Frontend and Backend subnets.</span></span> <span data-ttu-id="58ce7-146">Každá tabulka je načtena s statické trasy, které jsou vhodné pro dané podsíti.</span><span class="sxs-lookup"><span data-stu-id="58ce7-146">Each table is loaded with static routes appropriate for the given subnet.</span></span> <span data-ttu-id="58ce7-147">Pro účely tohoto příkladu každá tabulka měla tři trasy:</span><span class="sxs-lookup"><span data-stu-id="58ce7-147">For the purpose of this example, each table has three routes:</span></span>

1. <span data-ttu-id="58ce7-148">Provozu místních podsítí s žádné další směrování definované umožňující provozu místních podsítí obejít bránu firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-148">Local subnet traffic with no Next Hop defined to allow local subnet traffic to bypass the firewall</span></span>
2. <span data-ttu-id="58ce7-149">Virtuální síťový provoz s další směrování definovat jako bránu firewall, přepíše výchozí pravidlo, které umožňuje místní virtuální síť provoz směrovat přímo</span><span class="sxs-lookup"><span data-stu-id="58ce7-149">Virtual Network traffic with a Next Hop defined as firewall, this overrides the default rule that allows local VNet traffic to route directly</span></span>
3. <span data-ttu-id="58ce7-150">Veškerý zbývající provoz (0/0) se na další směrování definovat jako bránu firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-150">All remaining traffic (0/0) with a Next Hop defined as the firewall</span></span>

<span data-ttu-id="58ce7-151">Po vytvoření směrovacích tabulek jsou vázány na podsítě.</span><span class="sxs-lookup"><span data-stu-id="58ce7-151">Once the routing tables are created they are bound to their subnets.</span></span> <span data-ttu-id="58ce7-152">Pro podsíť Frontend směrovací tabulky, po vytvoření a vázaný k podsíti by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="58ce7-152">For the Frontend subnet routing table, once created and bound to the subnet should look like this:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


<span data-ttu-id="58ce7-153">V tomto příkladu se používají následující příkazy pro vytváření směrovací tabulka, přidejte trasu definovanou uživatelem a pak vytvořte vazbu tabulku směrování pro podsíť (Poznámka; všechny položky pod počínaje znak dolaru (např: $BESubnet) jsou uživatelem definované proměnné ve skriptu v části odkaz na tohoto dokumentu):</span><span class="sxs-lookup"><span data-stu-id="58ce7-153">For this example, the following commands are used to build the route table, add a user defined route, and then bind the route table to a subnet (Note; any items below beginning with a dollar sign (e.g.: $BESubnet) are user defined variables from the script in the reference section of this document):</span></span>

1. <span data-ttu-id="58ce7-154">Základní směrovací tabulky musí být nejprve vytvořen.</span><span class="sxs-lookup"><span data-stu-id="58ce7-154">First the base routing table must be created.</span></span> <span data-ttu-id="58ce7-155">Tento fragment kódu ukazuje vytvoření tabulky pro podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="58ce7-155">This snippet shows the creation of the table for the Backend subnet.</span></span> <span data-ttu-id="58ce7-156">Ve skriptu je pro podsíť Frontend také vytvořit odpovídající tabulku.</span><span class="sxs-lookup"><span data-stu-id="58ce7-156">In the script, a corresponding table is also created for the Frontend subnet.</span></span>
   
     <span data-ttu-id="58ce7-157">Nové AzureRouteTable-název $BERouteTableName.</span><span class="sxs-lookup"><span data-stu-id="58ce7-157">New-AzureRouteTable -Name $BERouteTableName \`</span></span>
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. <span data-ttu-id="58ce7-158">Po vytvoření směrovací tabulka se dá přidat trasy definované uživatelem konkrétní.</span><span class="sxs-lookup"><span data-stu-id="58ce7-158">Once the route table is created, specific user defined routes can be added.</span></span> <span data-ttu-id="58ce7-159">V tomto uvádíme veškerý provoz (0.0.0.0/0) budou směrovány přes virtuální zařízení (proměnné, $VMIP [0], je sloužící k předávání IP adresu přiřadit při vytvoření virtuálního zařízení ve skriptu dříve).</span><span class="sxs-lookup"><span data-stu-id="58ce7-159">In this snipped, all traffic (0.0.0.0/0) will be routed through the virtual appliance (a variable, $VMIP[0], is used to pass in the IP address assigned when the virtual appliance was created earlier in the script).</span></span> <span data-ttu-id="58ce7-160">Ve skriptu se také vytvoří odpovídající pravidlo v tabulce front-endu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-160">In the script, a corresponding rule is also created in the Frontend table.</span></span>
   
     <span data-ttu-id="58ce7-161">Get-AzureRouteTable $BERouteTableName | \`</span><span class="sxs-lookup"><span data-stu-id="58ce7-161">Get-AzureRouteTable $BERouteTableName | \`</span></span>
   
         Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. <span data-ttu-id="58ce7-162">Výše uvedené položky trasy přepíše výchozí "0.0.0.0/0" trasu, ale stále existuje výchozí pravidlo 10.0.0.0/16 který by umožnil provoz v rámci virtuální sítě pro směrování přímo do cílového umístění, ne na virtuální síťové zařízení.</span><span class="sxs-lookup"><span data-stu-id="58ce7-162">The above route entry will override the default "0.0.0.0/0" route, but the default 10.0.0.0/16 rule still existing which would allow traffic within the VNet to route directly to the destination and not to the Network Virtual Appliance.</span></span> <span data-ttu-id="58ce7-163">Pro správné toto chování postupujte podle pravidla musí být přidaný.</span><span class="sxs-lookup"><span data-stu-id="58ce7-163">To correct this behavior the follow rule must be added.</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. <span data-ttu-id="58ce7-164">V tomto okamžiku je volbou má být provedeno.</span><span class="sxs-lookup"><span data-stu-id="58ce7-164">At this point there is a choice to be made.</span></span> <span data-ttu-id="58ce7-165">Pomocí výše uvedené dvě cesty bude směrovat veškerý provoz do brány firewall pro vyhodnocení, i provoz v rámci jedné podsíti.</span><span class="sxs-lookup"><span data-stu-id="58ce7-165">With the above two routes all traffic will route to the firewall for assessment, even traffic within a single subnet.</span></span> <span data-ttu-id="58ce7-166">To může být požaduje, ale pokud chcete povolit přenosy v rámci jedné podsítě pro směrování místně bez zásahu bránu firewall jiného, mohou být přidány velmi konkrétní pravidlo.</span><span class="sxs-lookup"><span data-stu-id="58ce7-166">This may be desired, however to allow traffic within a subnet to route locally without involvement from the firewall a third, very specific rule can be added.</span></span> <span data-ttu-id="58ce7-167">Tato trasa stavy, které libovolná adresa destine pro místní podsíti může právě směrovat existuje přímo (NextHopType = VNETLocal).</span><span class="sxs-lookup"><span data-stu-id="58ce7-167">This route states that any address destine for the local subnet can just route there directly (NextHopType = VNETLocal).</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. <span data-ttu-id="58ce7-168">Nakonec se do směrovací tabulky vytvořeny a naplněny s trasy definované uživatelem v tabulce musí nyní být vázána na podsíť.</span><span class="sxs-lookup"><span data-stu-id="58ce7-168">Finally, with the routing table created and populated with a user defined routes, the table must now be bound to a subnet.</span></span> <span data-ttu-id="58ce7-169">Ve skriptu je front-end směrovací tabulka také vázána podsítě front-endu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-169">In the script, the front end route table is also bound to the Frontend subnet.</span></span> <span data-ttu-id="58ce7-170">Zde je vazba skript pro podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="58ce7-170">Here is the binding script for the back end subnet.</span></span>
   
     <span data-ttu-id="58ce7-171">Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName.</span><span class="sxs-lookup"><span data-stu-id="58ce7-171">Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName \`</span></span>
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a><span data-ttu-id="58ce7-172">Předávání IP</span><span class="sxs-lookup"><span data-stu-id="58ce7-172">IP Forwarding</span></span>
<span data-ttu-id="58ce7-173">Doprovodná funkce, která UDR, je předávání IP.</span><span class="sxs-lookup"><span data-stu-id="58ce7-173">A companion feature to UDR, is IP Forwarding.</span></span> <span data-ttu-id="58ce7-174">Je toto nastavení na virtuální zařízení, která umožňuje přijímání dat adresovaných není konkrétně pro zařízení, a pak tento přenosu do konečného ultimate.</span><span class="sxs-lookup"><span data-stu-id="58ce7-174">This is a setting on a Virtual Appliance that allows it to receive traffic not specifically addressed to the appliance and then forward that traffic to its ultimate destination.</span></span>

<span data-ttu-id="58ce7-175">Jako příklad Pokud provoz z AppVM01 provede požadavek na server DNS01 UDR by směrovat to do brány firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-175">As an example, if traffic from AppVM01 makes a request to the DNS01 server, UDR would route this to the firewall.</span></span> <span data-ttu-id="58ce7-176">S povoleno předávání IP přenosy dat pro cílový DNS01 (10.0.2.4) akceptovat zařízení (10.0.0.4) a potom předána do cílového ultimate (10.0.2.4).</span><span class="sxs-lookup"><span data-stu-id="58ce7-176">With IP Forwarding enabled, the traffic for the DNS01 destination (10.0.2.4) will be accepted by the appliance (10.0.0.4) and then forwarded to its ultimate destination (10.0.2.4).</span></span> <span data-ttu-id="58ce7-177">Bez předávání IP zapnuta brána Firewall nebude možné provoz přijímat zařízení Přestože směrovací tabulka má bránu firewall jako další segment.</span><span class="sxs-lookup"><span data-stu-id="58ce7-177">Without IP Forwarding enabled on the Firewall, traffic would not be accepted by the appliance even though the route table has the firewall as the next hop.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="58ce7-178">Je důležité si pamatovat, abyste povolili předávání IP ve spojení s směrování definovaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="58ce7-178">It’s critical to remember to enable IP Forwarding in conjunction with User Defined Routing.</span></span>
> 
> 

<span data-ttu-id="58ce7-179">Nastavení předávání IP adres je jeden příkaz a lze provést v okamžiku vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="58ce7-179">Setting up IP Forwarding is a single command and can be done at VM creation time.</span></span> <span data-ttu-id="58ce7-180">Pro tok tohoto příkladu fragmentu kódu je na konci skript a seskupuje UDR příkazy:</span><span class="sxs-lookup"><span data-stu-id="58ce7-180">For the flow of this example the code snippet is towards the end of the script and grouped with the UDR commands:</span></span>

1. <span data-ttu-id="58ce7-181">Volání instance virtuálního počítače, který v tomto případě je vaše virtuální zařízení brány firewall a povolení předávání IP adres (Poznámka; libovolnou položku v red počínaje znak dolaru (např: $VMName[0]) je uživatelem definované proměnné ve skriptu v části odkaz na tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-181">Call the VM instance that is your virtual appliance, the firewall in this case, and enable IP Forwarding (Note; any item in red beginning with a dollar sign (e.g.: $VMName[0]) is a user defined variable from the script in the reference section of this document.</span></span> <span data-ttu-id="58ce7-182">Nula v hranatých závorkách [0], představuje první virtuální počítač v poli virtuálních počítačů pro ukázkový skript pracovat bez úprav, první virtuální počítač (VM 0) musí být bránu firewall):</span><span class="sxs-lookup"><span data-stu-id="58ce7-182">The zero in brackets, [0], represents the first VM in the array of VMs, for the example script to work without modification, the first VM (VM 0) must be the firewall):</span></span>
   
     <span data-ttu-id="58ce7-183">Get-AzureVM-název $VMName [0] - ServiceName $ServiceName [0] | \`</span><span class="sxs-lookup"><span data-stu-id="58ce7-183">Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | \`</span></span>
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a><span data-ttu-id="58ce7-184">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="58ce7-184">Network Security Groups (NSG)</span></span>
<span data-ttu-id="58ce7-185">V tomto příkladu je skupina NSG vytvořené a pak načten s jedním pravidlem.</span><span class="sxs-lookup"><span data-stu-id="58ce7-185">In this example, a NSG group is built and then loaded with a single rule.</span></span> <span data-ttu-id="58ce7-186">Tato skupina je pak vázán pouze na podsítě front-endové a back-end (ne SecNet).</span><span class="sxs-lookup"><span data-stu-id="58ce7-186">This group is then bound only to the Frontend and Backend subnets (not the SecNet).</span></span> <span data-ttu-id="58ce7-187">Deklarativně se sestavuje následující pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-187">Declaratively the following rule is being built:</span></span>

1. <span data-ttu-id="58ce7-188">Přenosy dat (všechny porty) z Internetu do celý virtuální sítě (všechny podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="58ce7-188">Any traffic (all ports) from the Internet to the entire VNet (all subnets) is Denied</span></span>

<span data-ttu-id="58ce7-189">I když v tomto příkladu se používají skupiny Nsg, je hlavním účelem jako vrstva sekundární ochranu proti ruční chybné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="58ce7-189">Although NSGs are used in this example, it’s main purpose is as a secondary layer of defense against manual misconfiguration.</span></span> <span data-ttu-id="58ce7-190">Chceme blokovat všechna příchozí provoz z Internetu do buď front-end nebo back-end podsítě, provoz by měl pouze procházet skrz SecNet podsítě do brány firewall (a pak v případě vhodné k front-end nebo back-end podsítě).</span><span class="sxs-lookup"><span data-stu-id="58ce7-190">We want to block all inbound traffic from the internet to either the Frontend or Backend subnets, traffic should only flow through the SecNet subnet to the firewall (and then if appropriate on to the Frontend or Backend subnets).</span></span> <span data-ttu-id="58ce7-191">Plus s pravidly UDR v místě, jakýkoli přenos, který zkontrolujte do podsítí front-end nebo back-end by přesměrováni se do brány firewall (díky UDR).</span><span class="sxs-lookup"><span data-stu-id="58ce7-191">Plus, with the UDR rules in place, any traffic that did make it into the Frontend or Backend subnets would be directed out to the firewall (thanks to UDR).</span></span> <span data-ttu-id="58ce7-192">Brána firewall by to zobrazit jako asymetrický toku a by vyřadit odchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="58ce7-192">The firewall would see this as an asymmetric flow and would drop the outbound traffic.</span></span> <span data-ttu-id="58ce7-193">Proto existují tři vrstvy zabezpečení, ochraně podsítě front-endu a back-end; 1) žádné otevřete koncové body na FrontEnd001 a BackEnd001 cloudových služeb, skupin Nsg 2), odepření přenosy z Internetu, 3) brána firewall vyřazování asymetrické provoz.</span><span class="sxs-lookup"><span data-stu-id="58ce7-193">Thus there are three layers of security protecting the Frontend and Backend subnets; 1) no open endpoints on the FrontEnd001 and BackEnd001 cloud services, 2) NSGs denying traffic from the Internet, 3) the firewall dropping asymmetric traffic.</span></span>

<span data-ttu-id="58ce7-194">Jeden bod zajímavé týkající se skupina zabezpečení sítě v tomto příkladu je, že obsahuje pouze jedno pravidlo, viz následující obrázek, který je tak, aby odepřel internetové přenosy na celý virtuální síť, která bude zahrnovat podsítě zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="58ce7-194">One interesting point regarding the Network Security Group in this example is that it contains only one rule, shown below, which is to deny internet traffic to the entire virtual network which would include the Security subnet.</span></span> 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

<span data-ttu-id="58ce7-195">Ale vzhledem k tomu, že NSG je vázaný jenom na podsítě front-endové a back-end, pravidlo není zpracován na provoz příchozí na podsíť. možnosti zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="58ce7-195">However, since the NSG is only bound to the Frontend and Backend subnets, the rule isn’t processed on traffic inbound to the Security subnet.</span></span> <span data-ttu-id="58ce7-196">Výsledkem je i když pravidla NSG uvádí žádné internetový provoz pro každou adresu, na virtuální síti, protože NSG se nikdy vázána na podsíť. možnosti zabezpečení, bude přenos na podsíť. možnosti zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="58ce7-196">As a result, even though the NSG rule says no Internet traffic to any address on the VNet, because the NSG was never bound to the Security subnet, traffic will flow to the Security subnet.</span></span>

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a><span data-ttu-id="58ce7-197">Pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-197">Firewall Rules</span></span>
<span data-ttu-id="58ce7-198">V bráně firewall předávání pravidla bude nutné vytvořit.</span><span class="sxs-lookup"><span data-stu-id="58ce7-198">On the firewall, forwarding rules will need to be created.</span></span> <span data-ttu-id="58ce7-199">Vzhledem k tomu, že brána firewall je blokování nebo předávání všechny vstupní, výstupní a intra-VNet provoz, je potřeba řada pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-199">Since the firewall is blocking or forwarding all inbound, outbound, and intra-VNet traffic many firewall rules are needed.</span></span> <span data-ttu-id="58ce7-200">Navíc veškerý příchozí provoz, se setkají služby zabezpečení veřejnou IP adresu (na jiné porty), mají být zpracovány bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-200">Also, all inbound traffic will hit the Security Service public IP address (on different ports), to be processed by the firewall.</span></span> <span data-ttu-id="58ce7-201">Osvědčeným postupem je diagram logické toky před nastavením podsítě a pravidla brány firewall, aby se zabránilo přepracování později.</span><span class="sxs-lookup"><span data-stu-id="58ce7-201">A best practice is to diagram the logical flows before setting up the subnets and firewall rules to avoid rework later.</span></span> <span data-ttu-id="58ce7-202">Na následujícím obrázku je logický zobrazení pravidla brány firewall v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="58ce7-202">The following figure is a logical view of the firewall rules for this example:</span></span>

<span data-ttu-id="58ce7-203">![Logickém zobrazení pravidla brány Firewall][2]</span><span class="sxs-lookup"><span data-stu-id="58ce7-203">![Logical View of the Firewall Rules][2]</span></span>

> [!NOTE]
> <span data-ttu-id="58ce7-204">Založená na síti virtuální zařízení používá, se liší porty pro správu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-204">Based on the Network Virtual Appliance used, the management ports will vary.</span></span> <span data-ttu-id="58ce7-205">V tomto příkladu, které se odkazuje Barracuda NextGen Firewall, který používá porty 22, 801 a 807.</span><span class="sxs-lookup"><span data-stu-id="58ce7-205">In this example a Barracuda NextGen Firewall is referenced which uses ports 22, 801, and 807.</span></span> <span data-ttu-id="58ce7-206">Najdete v dokumentaci výrobce zařízení najít přesnou porty používané ke správě zařízení používá.</span><span class="sxs-lookup"><span data-stu-id="58ce7-206">Please consult the appliance vendor documentation to find the exact ports used for management of the device being used.</span></span>
> 
> 

### <a name="logical-rule-description"></a><span data-ttu-id="58ce7-207">Popis logické pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-207">Logical Rule Description</span></span>
<span data-ttu-id="58ce7-208">Ve výše uvedeném logického diagramu se nezobrazí podsítě zabezpečení vzhledem k tomu, že brána firewall je pouze prostředků na této podsíti a tohoto diagramu se zobrazuje pravidla brány firewall a jak se logicky povolit nebo odepřít tok přenosů dat a ne skutečné směrované cesty.</span><span class="sxs-lookup"><span data-stu-id="58ce7-208">In the logical diagram above, the security subnet is not shown since the firewall is the only resource on that subnet, and this diagram is showing the firewall rules and how they logically allow or deny traffic flows and not the actual routed path.</span></span> <span data-ttu-id="58ce7-209">Také externí porty, vybraný pro provoz protokolu RDP jsou vyšší pohyboval porty (8014 – 8026) a nebyly vybrány poněkud vyrovnání v posledních dvou oktety místní IP adresu pro snazší čitelnost (například místní server adresu 10.0.1.4 je přidružen externí port 8014), ale může použít jakékoli vyšší-konfliktní porty.</span><span class="sxs-lookup"><span data-stu-id="58ce7-209">Also, the external ports selected for the RDP traffic are higher ranged ports (8014 – 8026) and were selected to somewhat align with the last two octets of the local IP address for easier readability (e.g. local server address 10.0.1.4 is associated with external port 8014), however any higher non-conflicting ports could be used.</span></span>

<span data-ttu-id="58ce7-210">V tomto příkladu budeme potřebovat 7 typy pravidel, tyto typy pravidel jsou popsány takto:</span><span class="sxs-lookup"><span data-stu-id="58ce7-210">For this example, we need 7 types of rules, these rule types are described as follows:</span></span>

* <span data-ttu-id="58ce7-211">Externí pravidla (pro příchozí provoz):</span><span class="sxs-lookup"><span data-stu-id="58ce7-211">External Rules (for inbound traffic):</span></span>
  1. <span data-ttu-id="58ce7-212">Pravidlo brány firewall správy: Toto pravidlo přesměrování aplikace umožňuje přenos dat na porty správy zařízení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="58ce7-212">Firewall Management Rule: This App Redirect rule allows traffic to pass to the management ports of the network virtual appliance.</span></span>
  2. <span data-ttu-id="58ce7-213">Pravidla protokolu RDP (pro každý server systému windows): tyto čtyři pravidla (jeden pro každý server) vám umožní správu jednotlivých serverů prostřednictvím protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="58ce7-213">RDP Rules (for each windows server): These four rules (one for each server) will allow management of the individual servers via RDP.</span></span> <span data-ttu-id="58ce7-214">To může také seskupeny do jedno pravidlo v závislosti na možnosti síťového virtuální zařízení používá.</span><span class="sxs-lookup"><span data-stu-id="58ce7-214">This could also be bundled into one rule depending on the capabilities of the Network Virtual Appliance being used.</span></span>
  3. <span data-ttu-id="58ce7-215">Pravidla pro provoz aplikace: Existují dvě pravidla provoz aplikace první pro webový provoz front-endu a druhý pro přenosy back-end (např. webový server na datové vrstvě).</span><span class="sxs-lookup"><span data-stu-id="58ce7-215">Application Traffic Rules: There are two Application Traffic Rules, the first for the front end web traffic, and the second for the back end traffic (eg web server to data tier).</span></span> <span data-ttu-id="58ce7-216">Konfigurace tato pravidla se závisí na síťovou architekturu (kde jsou umístěné vaše servery) a provoz toků (směru tok přenosů dat a který porty se používají).</span><span class="sxs-lookup"><span data-stu-id="58ce7-216">The configuration of these rules will depend on the network architecture (where your servers are placed) and traffic flows (which direction the traffic flows, and which ports are used).</span></span>
     * <span data-ttu-id="58ce7-217">První pravidlo povolí provoz skutečné aplikaci připojit k serveru aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ce7-217">The first rule will allow the actual application traffic to reach the application server.</span></span> <span data-ttu-id="58ce7-218">Zatímco ostatní pravidla povolit pro zabezpečení, správy, atd., jsou pravidla aplikace co povolit externích uživatelů nebo služeb pro přístup k aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ce7-218">While the other rules allow for security, management, etc., Application Rules are what allow external users or services to access the application(s).</span></span> <span data-ttu-id="58ce7-219">V tomto příkladu je jednom webovém serveru na portu 80, proto jediné aplikace pravidlo firewallu přesměruje příchozí přenosy na externí IP adresu, na webové servery interní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-219">For this example, there is a single web server on port 80, thus a single firewall application rule will redirect inbound traffic to the external IP, to the web servers internal IP address.</span></span> <span data-ttu-id="58ce7-220">Relace přesměrovaného přenosy by se NAT i interního serveru.</span><span class="sxs-lookup"><span data-stu-id="58ce7-220">The redirected traffic session would be NAT’d to the internal server.</span></span>
     * <span data-ttu-id="58ce7-221">Druhé pravidlo pro provoz aplikace je back-end pravidlo, kterým povolíte Webový Server, aby komunikoval s AppVM01 serveru (ale ne AppVM02) prostřednictvím libovolný port.</span><span class="sxs-lookup"><span data-stu-id="58ce7-221">The second Application Traffic Rule is the back end rule to allow the Web Server to talk to the AppVM01 server (but not AppVM02) via any port.</span></span>
* <span data-ttu-id="58ce7-222">Vnitřní pravidla (pro provoz intra-VNet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-222">Internal Rules (for intra-VNet traffic)</span></span>
  1. <span data-ttu-id="58ce7-223">Odchozí do internetové pravidlo: Toto pravidlo povolí provoz od všech sítí, které mají být předána do vybraných sítí.</span><span class="sxs-lookup"><span data-stu-id="58ce7-223">Outbound to Internet Rule: This rule will allow traffic from any network to pass to the selected networks.</span></span> <span data-ttu-id="58ce7-224">Toto pravidlo je obvykle výchozí pravidlo už v bráně firewall, ale v zakázaném stavu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-224">This rule is usually a default rule already on the firewall, but in a disabled state.</span></span> <span data-ttu-id="58ce7-225">Toto pravidlo by měly být povoleny v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-225">This rule should be enabled for this example.</span></span>
  2. <span data-ttu-id="58ce7-226">Pravidlo DNS: Toto pravidlo umožňuje předat serveru DNS pouze provoz DNS (port 53).</span><span class="sxs-lookup"><span data-stu-id="58ce7-226">DNS Rule: This rule allows only DNS (port 53) traffic to pass to the DNS server.</span></span> <span data-ttu-id="58ce7-227">Toto pravidlo pro toto prostředí, které se většina provoz z front-endu na back-end je blokovaný, konkrétně umožňuje DNS z jakékoli místní podsítě.</span><span class="sxs-lookup"><span data-stu-id="58ce7-227">For this environment most traffic from the Frontend to the Backend is blocked, this rule specifically allows DNS from any local subnet.</span></span>
  3. <span data-ttu-id="58ce7-228">Pravidlo podsítě pro podsíť: Toto pravidlo se má povolit všechny servery v podsíti back-end připojení k libovolnému serveru na podsítě front end (ale nikoli naopak).</span><span class="sxs-lookup"><span data-stu-id="58ce7-228">Subnet to Subnet Rule: This rule is to allow any server on the back end subnet to connect to any server on the front end subnet (but not the reverse).</span></span>
* <span data-ttu-id="58ce7-229">Pohotovostního pravidlo (pro přenosy, které nesplňují výše uvedených možností):</span><span class="sxs-lookup"><span data-stu-id="58ce7-229">Fail-safe Rule (for traffic that doesn’t meet any of the above):</span></span>
  1. <span data-ttu-id="58ce7-230">Všechny přenosy pravidlo odepřít: To by mělo být vždy poslední pravidlo (z hlediska priorita) a jako takový Pokud přenosy dat se nezdaří tak, aby odpovídaly některé z předchozích pravidel, které se zahodí tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="58ce7-230">Deny All Traffic Rule: This should always be the final rule (in terms of priority), and as such if a traffic flows fails to match any of the preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="58ce7-231">Toto je výchozí pravidlo a obvykle aktivaci žádné je obecně nutné provést změny.</span><span class="sxs-lookup"><span data-stu-id="58ce7-231">This is a default rule and usually activated, no modifications are generally needed.</span></span>

> [!TIP]
> <span data-ttu-id="58ce7-232">Na druhé pravidlo provoz aplikace jakéhokoli portu je povolen pro snadné tohoto příkladu, ve scénáři skutečné nejvíce konkrétní port a rozsahy adres se má použít pro snížení rizika útoku tohoto pravidla.</span><span class="sxs-lookup"><span data-stu-id="58ce7-232">On the second application traffic rule, any port is allowed for easy of this example, in a real scenario the most specific port and address ranges should be used to reduce the attack surface of this rule.</span></span>
> 
> 

<br />

> [!IMPORTANT]
> <span data-ttu-id="58ce7-233">Po vytvoření všech výše uvedených pravidel, je důležité zkontrolovat prioritu každé pravidlo zajistit provoz se povolí nebo zakáže podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="58ce7-233">Once all of the above rules are created, it’s important to review the priority of each rule to ensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="58ce7-234">V tomto příkladu jsou pravidla v pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="58ce7-234">For this example, the rules are in priority order.</span></span> <span data-ttu-id="58ce7-235">Je snadné se uzamkne mimo bránu firewall kvůli nemá seřazené pravidla.</span><span class="sxs-lookup"><span data-stu-id="58ce7-235">It's easy to be locked out of the firewall due to mis-ordered rules.</span></span> <span data-ttu-id="58ce7-236">Minimálně zkontrolujte, zda správy pro bránu firewall, samotné vždy absolutní pravidlo nejvyšší prioritou.</span><span class="sxs-lookup"><span data-stu-id="58ce7-236">At a minimum, ensure the management for the firewall itself is always the absolute highest priority rule.</span></span>
> 
> 

### <a name="rule-prerequisites"></a><span data-ttu-id="58ce7-237">Pravidla požadavků</span><span class="sxs-lookup"><span data-stu-id="58ce7-237">Rule Prerequisites</span></span>
<span data-ttu-id="58ce7-238">Jeden předpoklad pro virtuální počítač spuštěný brány firewall jsou veřejné koncové body.</span><span class="sxs-lookup"><span data-stu-id="58ce7-238">One prerequisite for the Virtual Machine running the firewall are public endpoints.</span></span> <span data-ttu-id="58ce7-239">Pro bránu firewall pro zpracování provozu je třeba otevřít odpovídající veřejné koncové body.</span><span class="sxs-lookup"><span data-stu-id="58ce7-239">For the firewall to process traffic the appropriate public endpoints must be open.</span></span> <span data-ttu-id="58ce7-240">Existují tři typy přenosů dat v tomto příkladu; Provoz protokolu RDP 1) provoz správy pro řízení brány firewall a pravidla brány firewall, 2) k řízení serverů se systémem windows a provoz 3) aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ce7-240">There are three types of traffic in this example; 1) Management traffic to control the firewall and firewall rules, 2) RDP traffic to control the windows servers, and 3) Application Traffic.</span></span> <span data-ttu-id="58ce7-241">Toto jsou tři sloupce typů přenosů v horní polovině logickém zobrazení pravidla brány firewall výše.</span><span class="sxs-lookup"><span data-stu-id="58ce7-241">These are the three columns of traffic types in the upper half of logical view of the firewall rules above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58ce7-242">Je zde klíče takeway nezapomeňte, že **všechny** provoz se odešlou přes bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-242">A key takeway here is to remember that **all** traffic will come through the firewall.</span></span> <span data-ttu-id="58ce7-243">Proto vzdálené plochy k serveru IIS01, i když je počítač v cloudové službě Front End a na podsítě Front End, pro přístup k tomuto serveru jsme bude muset RDP do brány firewall na portu 8014 a potom povolit bránu firewall pro směrování požadavku protokolu RDP interně k portu RDP IIS01.</span><span class="sxs-lookup"><span data-stu-id="58ce7-243">So to remote desktop to the IIS01 server, even though it's in the Front End Cloud Service and on the Front End subnet, to access this server we will need to RDP to the firewall on port 8014, and then allow the firewall to route the RDP request internally to the IIS01 RDP Port.</span></span> <span data-ttu-id="58ce7-244">Tlačítko "Připojit" portálu Azure nebude fungovat, protože neexistuje přímé cesta protokolu RDP na IIS01 (jde o můžete zobrazit na portálu).</span><span class="sxs-lookup"><span data-stu-id="58ce7-244">The Azure portal's "Connect" button won't work because there is no direct RDP path to IIS01 (as far as the portal can see).</span></span> <span data-ttu-id="58ce7-245">To znamená, že všechna připojení z Internetu bude služby zabezpečení a Port, například secscv001.cloudapp.net:xxxx (referenční dokumentace diagramu pro mapování portů externí, interní IP a Port).</span><span class="sxs-lookup"><span data-stu-id="58ce7-245">This means all connections from the internet will be to the Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx (reference the above diagram for the mapping of External Port and Internal IP and Port).</span></span>
> 
> 

<span data-ttu-id="58ce7-246">Koncový bod lze otevřít buď při vytváření virtuálních počítačů a post sestavení, jak se provádí v ukázkový skript a znázorněném na tento fragment kódu (Poznámka; všechny položky počínaje znak dolaru (např: $VMName[$i]) je uživatelem definované proměnné ve skriptu v části odkaz na tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-246">An endpoint can be opened either at the time of VM creation or post build as is done in the example script and shown below in this code snippet (Note; any item beginning with a dollar sign (e.g.: $VMName[$i]) is a user defined variable from the script in the reference section of this document.</span></span> <span data-ttu-id="58ce7-247">"$I" v hranatých závorkách [$i] představuje číslo pole konkrétní virtuální počítač v matici virtuálních počítačů):</span><span class="sxs-lookup"><span data-stu-id="58ce7-247">The “$i” in brackets, [$i], represents the array number of a specific VM in an array of VMs):</span></span>

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

<span data-ttu-id="58ce7-248">I když se zobrazují zde není jasně kvůli použití proměnných, ale koncové body jsou **pouze** otevřít v cloudové službě zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="58ce7-248">Although not clearly shown here due to the use of variables, but endpoints are **only** opened on the Security Cloud Service.</span></span> <span data-ttu-id="58ce7-249">Tím je zajištěno, že se zpracovává veškerý příchozí provoz (směrovat, NAT měl, vynechané) bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-249">This is to ensure that all inbound traffic is handled (routed, NAT'd, dropped) by the firewall.</span></span>

<span data-ttu-id="58ce7-250">Klient správy bude potřeba nainstalovat na počítač pro správu brány firewall a vytvoření konfigurací potřeby.</span><span class="sxs-lookup"><span data-stu-id="58ce7-250">A management client will need to be installed on a PC to manage the firewall and create the configurations needed.</span></span> <span data-ttu-id="58ce7-251">O tom, jak spravovat zařízení, najdete v článku dodavatele dokumentace z brány firewall (nebo jiné hodnocení chyb zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="58ce7-251">See the documentation from your firewall (or other NVA) vendor on how to manage the device.</span></span> <span data-ttu-id="58ce7-252">Zbývající část tohoto tématu a v další části vytváření pravidel brány Firewall, bude popisují konfiguraci brány firewall, samostatně, prostřednictvím dodavatele správy klienta (tzn. ne portál Azure nebo PowerShell).</span><span class="sxs-lookup"><span data-stu-id="58ce7-252">The remainder of this section and the next section, Firewall Rules Creation, will describe the configuration of the firewall itself, through the vendors management client (i.e. not the Azure portal or PowerShell).</span></span>

<span data-ttu-id="58ce7-253">Pokyny pro stažení klienta a připojení k Barracuda použité v tomto příkladu naleznete zde: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="58ce7-253">Instructions for client download and connecting to the Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="58ce7-254">Po přihlášení na bránu firewall, ale před vytvořením pravidel brány firewall, existují dvě třídy požadovaných objektů, které můžou vytváření pravidel snadněji; Objekty, síť a služby.</span><span class="sxs-lookup"><span data-stu-id="58ce7-254">Once logged onto the firewall but before creating firewall rules, there are two prerequisite object classes that can make creating the rules easier; Network & Service objects.</span></span>

<span data-ttu-id="58ce7-255">V tomto příkladu tři objekty pojmenované síti musí být definované (jeden pro podsítě front-endu a back-end podsíť, také síťového objektu pro IP adresu serveru DNS).</span><span class="sxs-lookup"><span data-stu-id="58ce7-255">For this example, three named network objects should be defined (one for the Frontend subnet and the Backend subnet, also a network object for the IP address of the DNS server).</span></span> <span data-ttu-id="58ce7-256">Vytvořit síť s názvem; spouštění z řídicího panelu Barracuda NG správce klienta, přejděte na kartu Konfigurace, v části Konfigurace provozní klikněte Ruleset, pak klikněte na tlačítko "Sítě" v nabídce objekty brány Firewall a pak v nabídce Upravit sítě klikněte na tlačítko Nový.</span><span class="sxs-lookup"><span data-stu-id="58ce7-256">To create a named network; starting from the Barracuda NG Admin client dashboard, navigate to the configuration tab, in the Operational Configuration section click Ruleset, then click “Networks” under the Firewall Objects menu, then click New in the Edit Networks menu.</span></span> <span data-ttu-id="58ce7-257">Objekt sítě může být nyní vytvořen přidáním názvu a předponu:</span><span class="sxs-lookup"><span data-stu-id="58ce7-257">The network object can now be created, by adding the name and the prefix:</span></span>

<span data-ttu-id="58ce7-258">![Vytvoření objektu front-endové síti][3]</span><span class="sxs-lookup"><span data-stu-id="58ce7-258">![Create a FrontEnd Network Object][3]</span></span>

<span data-ttu-id="58ce7-259">Tím se vytvoří pojmenované sítě pro podsíť FrontEnd, podobně jako objekt měl být vytvořen v back-end podsíti.</span><span class="sxs-lookup"><span data-stu-id="58ce7-259">This will create a named network for the FrontEnd subnet, a similar object should be created for the BackEnd subnet as well.</span></span> <span data-ttu-id="58ce7-260">Nyní podsítě lze snadněji odkazovat podle názvu v pravidlech brány firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-260">Now the subnets can be more easily referenced by name in the firewall rules.</span></span>

<span data-ttu-id="58ce7-261">Pro objekt serveru DNS:</span><span class="sxs-lookup"><span data-stu-id="58ce7-261">For the DNS Server Object:</span></span>

<span data-ttu-id="58ce7-262">![Vytvořit objekt serveru DNS][4]</span><span class="sxs-lookup"><span data-stu-id="58ce7-262">![Create a DNS Server Object][4]</span></span>

<span data-ttu-id="58ce7-263">V pravidle DNS později v dokumentu použije tento jeden odkaz na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-263">This single IP address reference will be utilized in a DNS rule later in the document.</span></span>

<span data-ttu-id="58ce7-264">Druhý požadované objekty jsou objekty služby.</span><span class="sxs-lookup"><span data-stu-id="58ce7-264">The second prerequisite objects are Services objects.</span></span> <span data-ttu-id="58ce7-265">Toto bude reprezentovat porty pro připojení RDP pro každý server.</span><span class="sxs-lookup"><span data-stu-id="58ce7-265">These will represent the RDP connection ports for each server.</span></span> <span data-ttu-id="58ce7-266">Vzhledem k tomu, že existující objekt služby RDP je vázána výchozí port protokolu RDP, 3389, nové služby můžete vytvořit Pokud chcete povolit přenosy z externí porty (8014-8026).</span><span class="sxs-lookup"><span data-stu-id="58ce7-266">Since the existing RDP service object is bound to the default RDP port, 3389, new Services can be created to allow traffic from the external ports (8014-8026).</span></span> <span data-ttu-id="58ce7-267">Nové porty nebylo možné přidat také do existující služba protokolu RDP, ale pro usnadnění ukázku, můžete vytvořit jednotlivých pravidel pro každý server.</span><span class="sxs-lookup"><span data-stu-id="58ce7-267">The new ports could also be added to the existing RDP service, but for ease of demonstration, an individual rule for each server can be created.</span></span> <span data-ttu-id="58ce7-268">Chcete-li vytvořit nové pravidlo protokolu RDP pro server; spouštění z řídicího panelu Barracuda NG správce klienta, přejděte na kartu Konfigurace, v provozní konfigurační oddíl klikněte na Ruleset, pak klikněte na tlačítko "Služby" v nabídce objekty brány Firewall, přejděte dolů v seznamu služeb a vyberte službu "RDP".</span><span class="sxs-lookup"><span data-stu-id="58ce7-268">To create a new RDP rule for a server; starting from the Barracuda NG Admin client dashboard, navigate to the configuration tab, in the Operational Configuration section click Ruleset, then click “Services” under the Firewall Objects menu, scroll down the list of services and select the “RDP” service.</span></span> <span data-ttu-id="58ce7-269">Klikněte pravým tlačítkem a vyberte kopie, pak klikněte pravým tlačítkem a vyberte vložení.</span><span class="sxs-lookup"><span data-stu-id="58ce7-269">Right-click and select copy, then right-click and select Paste.</span></span> <span data-ttu-id="58ce7-270">Je nyní objekt služby RDP Copy1, který lze upravovat.</span><span class="sxs-lookup"><span data-stu-id="58ce7-270">There is now a RDP-Copy1 Service Object that can be edited.</span></span> <span data-ttu-id="58ce7-271">Klikněte pravým tlačítkem na RDP Copy1 a vyberte upravit, upravit objekt služby okno bude pop až, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="58ce7-271">Right-click RDP-Copy1 and select Edit, the Edit Service Object window will pop up as shown here:</span></span>

<span data-ttu-id="58ce7-272">![Kopii výchozí pravidlo protokolu RDP][5]</span><span class="sxs-lookup"><span data-stu-id="58ce7-272">![Copy of Default RDP Rule][5]</span></span>

<span data-ttu-id="58ce7-273">Hodnoty lze upravit k reprezentaci služba protokolu RDP pro určitý server.</span><span class="sxs-lookup"><span data-stu-id="58ce7-273">The values can then be edited to represent the RDP service for a specific server.</span></span> <span data-ttu-id="58ce7-274">Pro AppVM01 výše uvedené výchozí pravidlo protokolu RDP by měl být upraven tak, aby odrážela nový název služby, popis a externí portu RDP v diagramu obrázek 8 (Poznámka: porty jsou změnit z výchozích RDP 3389 k externí port používán pro tento konkrétní server, v případě AppVM01 externí Port je 8025) upravené služby jsou uvedeny níže :</span><span class="sxs-lookup"><span data-stu-id="58ce7-274">For AppVM01 the above default RDP rule should be modified to reflect a new Service Name, Description, and external RDP Port used in the Figure 8 diagram (Note: the ports are changed from the RDP default of 3389 to the external port being used for this specific server, in the case of AppVM01 the external Port is 8025) the modified service is shown below:</span></span>

<span data-ttu-id="58ce7-275">![Pravidlo AppVM01][6]</span><span class="sxs-lookup"><span data-stu-id="58ce7-275">![AppVM01 Rule][6]</span></span>

<span data-ttu-id="58ce7-276">Tento proces opakuje pro vytvoření služby protokolu RDP pro ostatní servery; AppVM02, DNS01 a IIS01.</span><span class="sxs-lookup"><span data-stu-id="58ce7-276">This process must be repeated to create RDP Services for the remaining servers; AppVM02, DNS01, and IIS01.</span></span> <span data-ttu-id="58ce7-277">Vytvoření tyto služby budou vytvoření pravidla jednodušší a zřejmější v další části.</span><span class="sxs-lookup"><span data-stu-id="58ce7-277">The creation of these Services will make the Rule creation simpler and more obvious in the next section.</span></span>

> [!NOTE]
> <span data-ttu-id="58ce7-278">Služby protokolu RDP pro bránu Firewall není potřeba dvou důvodů; první 1) brána firewall virtuálního počítače je bitová kopie založenými na systému Linux tak, aby SSH se použije na port 22 pro správu virtuálních počítačů místo protokolu RDP a 2) port 22, a dva další správu porty jsou povolené v první pravidlo správy popsané dál umožňující možnosti připojení správy.</span><span class="sxs-lookup"><span data-stu-id="58ce7-278">An RDP service for the Firewall is not needed for two reasons; 1) first the firewall VM is a Linux based image so SSH would be used on port 22 for VM management instead of RDP, and 2) port 22, and two other management ports are allowed in the first management rule described below to allow for management connectivity.</span></span>
> 
> 

### <a name="firewall-rules-creation"></a><span data-ttu-id="58ce7-279">Vytvoření pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-279">Firewall Rules Creation</span></span>
<span data-ttu-id="58ce7-280">Existují tři typy pravidel brány firewall použité v tomto příkladu, všechny mají odlišné ikony:</span><span class="sxs-lookup"><span data-stu-id="58ce7-280">There are three types of firewall rules used in this example, they all have distinct icons:</span></span>

<span data-ttu-id="58ce7-281">Pravidlo aplikace přesměrování: ![přesměrování ikona aplikace][7]</span><span class="sxs-lookup"><span data-stu-id="58ce7-281">The Application Redirect rule: ![Application Redirect Icon][7]</span></span>

<span data-ttu-id="58ce7-282">Pravidlo NAT cílové: ![ikonu cílové NAT][8]</span><span class="sxs-lookup"><span data-stu-id="58ce7-282">The Destination NAT rule: ![Destination NAT Icon][8]</span></span>

<span data-ttu-id="58ce7-283">Pravidlo průchodu: ![předat ikonu][9]</span><span class="sxs-lookup"><span data-stu-id="58ce7-283">The Pass rule: ![Pass Icon][9]</span></span>

<span data-ttu-id="58ce7-284">Další informace o těchto pravidel lze najít na webu Barracuda.</span><span class="sxs-lookup"><span data-stu-id="58ce7-284">More information on these rules can be found at the Barracuda web site.</span></span>

<span data-ttu-id="58ce7-285">Vytvořit následující pravidla (nebo ověřit existující výchozí pravidla), od řídicím panelu Barracuda NG správce klienta, přejděte na kartu Konfigurace, v provozní konfiguraci oddíl, klikněte na Ruleset.</span><span class="sxs-lookup"><span data-stu-id="58ce7-285">To create the following rules (or verify existing default rules), starting from the Barracuda NG Admin client dashboard, navigate to the configuration tab, in the Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="58ce7-286">Mřížka názvem, zobrazí "Hlavní pravidla" existující pravidla aktivní a deaktivované na tato brána firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-286">A grid called, “Main Rules” will show the existing active and deactivated rules on this firewall.</span></span> <span data-ttu-id="58ce7-287">V pravém horním rohu mřížce je malý, zelená "+" tlačítko, klepněte sem a vytvořit nové pravidlo (Poznámka: Brána firewall může "zamknout" změny, pokud se zobrazí tlačítko označené "Zamknout" a nelze vytvořit nebo upravit pravidla, klikněte na toto tlačítko "odemknutí" je sada pravidel a povolit úpravy).</span><span class="sxs-lookup"><span data-stu-id="58ce7-287">In the upper right corner of this grid is a small, green “+” button, click this to create a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable to create or edit rules, click this button to “unlock” the rule set and allow editing).</span></span> <span data-ttu-id="58ce7-288">Pokud chcete upravit existující pravidlo, vyberte toto pravidlo, klikněte pravým tlačítkem a vyberte Upravit pravidlo.</span><span class="sxs-lookup"><span data-stu-id="58ce7-288">If you wish to edit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="58ce7-289">Jakmile jsou pravidla vytvořit nebo upravit, musí být nabídnutých do brány firewall a pak se aktivuje, pokud to neuděláte pravidlo změny neprojeví.</span><span class="sxs-lookup"><span data-stu-id="58ce7-289">Once your rules are created and/or modified, they must be pushed to the firewall and then activated, if this is not done the rule changes will not take effect.</span></span> <span data-ttu-id="58ce7-290">Nabízení a aktivace proces je popsán níže popisy pravidlo podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="58ce7-290">The push and activation process is described below the details rule descriptions.</span></span>

<span data-ttu-id="58ce7-291">Jaké jsou specifikace každé pravidlo mohou provést pouze v tomto příkladu jsou popsány následovně:</span><span class="sxs-lookup"><span data-stu-id="58ce7-291">The specifics of each rule required to complete this example are described as follows:</span></span>

* <span data-ttu-id="58ce7-292">**Brány firewall pravidla správy**: Tato aplikace přesměrování pravidlo umožňuje předat porty správu zařízení virtuální sítě v tomto příkladu Barracuda NextGen Firewall provoz.</span><span class="sxs-lookup"><span data-stu-id="58ce7-292">**Firewall Management Rule**: This App Redirect rule allows traffic to pass to the management ports of the network virtual appliance, in this example a Barracuda NextGen Firewall.</span></span> <span data-ttu-id="58ce7-293">Porty pro správu jsou 801, 807 a volitelně 22.</span><span class="sxs-lookup"><span data-stu-id="58ce7-293">The management ports are 801, 807 and optionally 22.</span></span> <span data-ttu-id="58ce7-294">Externí i interní porty jsou stejné (tj. žádné překlad port).</span><span class="sxs-lookup"><span data-stu-id="58ce7-294">The external and internal ports are the same (i.e. no port translation).</span></span> <span data-ttu-id="58ce7-295">Toto pravidlo, instalační program-MGMT-přístup, je výchozí pravidlo a povolena ve výchozím nastavení (Barracuda NextGen Firewall verze 6.1).</span><span class="sxs-lookup"><span data-stu-id="58ce7-295">This rule, SETUP-MGMT-ACCESS, is a default rule and enabled by default (in Barracuda NextGen Firewall version 6.1).</span></span>
  
    <span data-ttu-id="58ce7-296">![Pravidlo brány firewall správy][10]</span><span class="sxs-lookup"><span data-stu-id="58ce7-296">![Firewall Management Rule][10]</span></span>

> [!TIP]
> <span data-ttu-id="58ce7-297">Adresní prostor zdroje v tomto pravidle je existuje, pokud se ví, že rozsahy správy IP adres, snižuje tento obor by také omezit možnost útoku na porty správy.</span><span class="sxs-lookup"><span data-stu-id="58ce7-297">The source address space in this rule is Any, if the management IP address ranges are known, reducing this scope would also reduce the attack surface to the management ports.</span></span>
> 
> 

* <span data-ttu-id="58ce7-298">**Pravidla RDP**: pravidla NAT tyto cílové vám umožní správu jednotlivých serverů prostřednictvím protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="58ce7-298">**RDP Rules**:  These Destination NAT rules will allow management of the individual servers via RDP.</span></span>
  <span data-ttu-id="58ce7-299">Existují čtyři kritické pole, které jsou potřebné k vytvoření tohoto pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-299">There are four critical fields needed to create this rule:</span></span>
  
  1. <span data-ttu-id="58ce7-300">Zdroj – chcete-li povolit RDP z libovolného místa, odkaz na "Žádný" se používá v poli zdroje.</span><span class="sxs-lookup"><span data-stu-id="58ce7-300">Source – to allow RDP from anywhere, the reference “Any” is used in the Source field.</span></span>
  2. <span data-ttu-id="58ce7-301">Služba – použít na příslušný objekt služby vytvořený v tomto případě "AppVM01 RDP", externí porty přesměrování na servery místní IP adresu a port 3386 (výchozí port protokolu RDP).</span><span class="sxs-lookup"><span data-stu-id="58ce7-301">Service – use the appropriate Service Object created earlier, in this case “AppVM01 RDP”, the external ports redirect to the servers local IP address and to port 3386 (the default RDP port).</span></span> <span data-ttu-id="58ce7-302">Tato konkrétní pravidlo je pro přístup k protokolu RDP na AppVM01.</span><span class="sxs-lookup"><span data-stu-id="58ce7-302">This specific rule is for RDP access to AppVM01.</span></span>
  3. <span data-ttu-id="58ce7-303">Cíl – musí být *místního portu v bráně firewall*, "IP místní server DHCP 1" nebo eth0, pokud se používá statické IP adresy.</span><span class="sxs-lookup"><span data-stu-id="58ce7-303">Destination – should be the *local port on the firewall*, “DCHP 1 Local IP” or eth0 if using static IPs.</span></span> <span data-ttu-id="58ce7-304">Řadová číslovka (eth0, eth1 atd.), může lišit, pokud vaše síťové zařízení má více místní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="58ce7-304">The ordinal number (eth0, eth1, etc) may be different if your network appliance has multiple local interfaces.</span></span> <span data-ttu-id="58ce7-305">Toto je port brány firewall odesílá z (může být stejný jako přijímající port), skutečný směrované cíl je v poli cílového seznamu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-305">This is the port the firewall is sending out from (may be the same as the receiving port), the actual routed destination is in the Target List field.</span></span>
  4. <span data-ttu-id="58ce7-306">Přesměrování – v této části informuje virtuální zařízení kde nakonec přesměrování tento provoz.</span><span class="sxs-lookup"><span data-stu-id="58ce7-306">Redirection – this section tells the virtual appliance where to ultimately redirect this traffic.</span></span> <span data-ttu-id="58ce7-307">Nejjednodušší přesměrování je umístit do cílového seznamu pole IP adresy a portu (volitelné).</span><span class="sxs-lookup"><span data-stu-id="58ce7-307">The simplest redirection is to place the IP and Port (optional) in the Target List field.</span></span> <span data-ttu-id="58ce7-308">Pokud žádné port je používán cílový port na příchozí žádosti bude používat (ie žádné překlad), pokud je port určený port bude také NAT by spolu s IP adres.</span><span class="sxs-lookup"><span data-stu-id="58ce7-308">If no port is used the destination port on the inbound request will be used (ie no translation), if a port is designated the port will also be NAT’d along with the IP address.</span></span>
     
     <span data-ttu-id="58ce7-309">![Pravidlo brány firewall protokolu RDP][11]</span><span class="sxs-lookup"><span data-stu-id="58ce7-309">![Firewall RDP Rule][11]</span></span>
     
     <span data-ttu-id="58ce7-310">Celkem čtyři pravidla RDP bude muset vytvořit:</span><span class="sxs-lookup"><span data-stu-id="58ce7-310">A total of four RDP rules will need to be created:</span></span> 
     
     | <span data-ttu-id="58ce7-311">Název pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-311">Rule Name</span></span> | <span data-ttu-id="58ce7-312">Server</span><span class="sxs-lookup"><span data-stu-id="58ce7-312">Server</span></span> | <span data-ttu-id="58ce7-313">Služba</span><span class="sxs-lookup"><span data-stu-id="58ce7-313">Service</span></span> | <span data-ttu-id="58ce7-314">Cílového seznamu</span><span class="sxs-lookup"><span data-stu-id="58ce7-314">Target List</span></span> |
     | --- | --- | --- | --- |
     | <span data-ttu-id="58ce7-315">RDP IIS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-315">RDP-to-IIS01</span></span> |<span data-ttu-id="58ce7-316">IIS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-316">IIS01</span></span> |<span data-ttu-id="58ce7-317">IIS01 PROTOKOLU RDP</span><span class="sxs-lookup"><span data-stu-id="58ce7-317">IIS01 RDP</span></span> |<span data-ttu-id="58ce7-318">10.0.1.4:3389</span><span class="sxs-lookup"><span data-stu-id="58ce7-318">10.0.1.4:3389</span></span> |
     | <span data-ttu-id="58ce7-319">RDP DNS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-319">RDP-to-DNS01</span></span> |<span data-ttu-id="58ce7-320">DNS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-320">DNS01</span></span> |<span data-ttu-id="58ce7-321">DNS01 PROTOKOLU RDP</span><span class="sxs-lookup"><span data-stu-id="58ce7-321">DNS01 RDP</span></span> |<span data-ttu-id="58ce7-322">10.0.2.4:3389</span><span class="sxs-lookup"><span data-stu-id="58ce7-322">10.0.2.4:3389</span></span> |
     | <span data-ttu-id="58ce7-323">RDP AppVM01</span><span class="sxs-lookup"><span data-stu-id="58ce7-323">RDP-to-AppVM01</span></span> |<span data-ttu-id="58ce7-324">AppVM01</span><span class="sxs-lookup"><span data-stu-id="58ce7-324">AppVM01</span></span> |<span data-ttu-id="58ce7-325">AppVM01 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="58ce7-325">AppVM01 RDP</span></span> |<span data-ttu-id="58ce7-326">10.0.2.5:3389</span><span class="sxs-lookup"><span data-stu-id="58ce7-326">10.0.2.5:3389</span></span> |
     | <span data-ttu-id="58ce7-327">RDP AppVM02</span><span class="sxs-lookup"><span data-stu-id="58ce7-327">RDP-to-AppVM02</span></span> |<span data-ttu-id="58ce7-328">AppVM02</span><span class="sxs-lookup"><span data-stu-id="58ce7-328">AppVM02</span></span> |<span data-ttu-id="58ce7-329">AppVm02 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="58ce7-329">AppVm02 RDP</span></span> |<span data-ttu-id="58ce7-330">10.0.2.6:3389</span><span class="sxs-lookup"><span data-stu-id="58ce7-330">10.0.2.6:3389</span></span> |

> [!TIP]
> <span data-ttu-id="58ce7-331">Zužující dolů oboru pole zdrojové a služby se zmenší prostor pro útoky.</span><span class="sxs-lookup"><span data-stu-id="58ce7-331">Narrowing down the scope of the Source and Service fields will reduce the attack surface.</span></span> <span data-ttu-id="58ce7-332">Většina omezeným oborem, který vám umožní funkce je třeba použít.</span><span class="sxs-lookup"><span data-stu-id="58ce7-332">The most limited scope that will allow functionality should be used.</span></span>
> 
> 

* <span data-ttu-id="58ce7-333">**Pravidla pro provoz aplikace**: existují dvě pravidla provoz aplikace první pro webový provoz front-endu a druhý pro přenosy back-end (např. webový server na datové vrstvě).</span><span class="sxs-lookup"><span data-stu-id="58ce7-333">**Application Traffic Rules**: There are two Application Traffic Rules, the first for the front end web traffic, and the second for the back end traffic (eg web server to data tier).</span></span> <span data-ttu-id="58ce7-334">Tato pravidla se závisí na síťovou architekturu (kde jsou umístěné vaše servery) a provoz toků (směru tok přenosů dat a který porty se používají).</span><span class="sxs-lookup"><span data-stu-id="58ce7-334">These rules will depend on the network architecture (where your servers are placed) and traffic flows (which direction the traffic flows, and which ports are used).</span></span>
  
    <span data-ttu-id="58ce7-335">Nejprve popsané je pravidlo front-endu pro webový provoz:</span><span class="sxs-lookup"><span data-stu-id="58ce7-335">First discussed is the front end rule for web traffic:</span></span>
  
    <span data-ttu-id="58ce7-336">![Pravidla brány firewall na webu][12]</span><span class="sxs-lookup"><span data-stu-id="58ce7-336">![Firewall Web Rule][12]</span></span>
  
    <span data-ttu-id="58ce7-337">Toto pravidlo NAT cílové umožňuje přenos skutečné aplikaci připojit k serveru aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ce7-337">This Destination NAT rule allows the actual application traffic to reach the application server.</span></span> <span data-ttu-id="58ce7-338">Zatímco ostatní pravidla povolit pro zabezpečení, správy, atd., jsou pravidla aplikace co povolit externích uživatelů nebo služeb pro přístup k aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ce7-338">While the other rules allow for security, management, etc., Application Rules are what allow external users or services to access the application(s).</span></span> <span data-ttu-id="58ce7-339">V tomto příkladu je jednom webovém serveru na portu 80, proto jediné aplikaci pravidlo firewallu přesměruje příchozí přenosy na externí IP adresu, na webové servery interní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-339">For this example, there is a single web server on port 80, thus the single firewall application rule will redirect inbound traffic to the external IP, to the web servers internal IP address.</span></span>
  
    <span data-ttu-id="58ce7-340">**Poznámka:**: že neexistuje žádná portu přiřazené do pole cílového seznamu, proto příchozí port 80 (nebo 443 pro službu vybrané) se použije v přesměrování webového serveru.</span><span class="sxs-lookup"><span data-stu-id="58ce7-340">**Note**: that there is no port assigned in the Target List field, thus the inbound port 80 (or 443 for the Service selected) will be used in the redirection of the web server.</span></span> <span data-ttu-id="58ce7-341">Pokud webový server naslouchá na jiný port, například port 8080, mohlo dojít k aktualizaci pole cílového seznamu k 10.0.1.4:8080 umožňující také přesměrování portu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-341">If the web server is listening on a different port, for example port 8080, the Target List field could be updated to 10.0.1.4:8080 to allow for the Port redirection as well.</span></span>
  
    <span data-ttu-id="58ce7-342">Pravidlo pro provoz další aplikace je back-end pravidlo, kterým povolíte Webový Server, aby komunikoval s AppVM01 serveru (ale ne AppVM02) prostřednictvím jakékoli služby:</span><span class="sxs-lookup"><span data-stu-id="58ce7-342">The next Application Traffic Rule is the back end rule to allow the Web Server to talk to the AppVM01 server (but not AppVM02) via Any service:</span></span>
  
    <span data-ttu-id="58ce7-343">![Pravidlo brány firewall AppVM01][13]</span><span class="sxs-lookup"><span data-stu-id="58ce7-343">![Firewall AppVM01 Rule][13]</span></span>
  
    <span data-ttu-id="58ce7-344">Toto pravidlo průchodu umožňuje jakýkoli server služby IIS na podsíť Frontend vás zastihnout AppVM01 (IP adresa 10.0.2.5) na libovolném portu pomocí libovolný protokol pro přístup k datům, které jsou potřebné pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="58ce7-344">This Pass rule allows any IIS server on the Frontend subnet to reach the AppVM01 (IP Address 10.0.2.5) on Any port, using any Protocol to access data needed by the web application.</span></span>
  
    <span data-ttu-id="58ce7-345">V tento snímek obrazovky "\<explicitní dest\>" se používá v poli cílové místo 10.0.2.5 jako cíl.</span><span class="sxs-lookup"><span data-stu-id="58ce7-345">In this screen shot an "\<explicit-dest\>" is used in the Destination field to signify 10.0.2.5 as the destination.</span></span> <span data-ttu-id="58ce7-346">To může být buď explicitní znázorněné nebo názvem objektu sítě (stejně jako ve požadavky pro DNS server).</span><span class="sxs-lookup"><span data-stu-id="58ce7-346">This could be either explicit as shown or a named Network Object (as was done in the prerequisites for the DNS server).</span></span> <span data-ttu-id="58ce7-347">Toto je až správce brány firewall, která se použije metoda.</span><span class="sxs-lookup"><span data-stu-id="58ce7-347">This is up to the administrator of the firewall as to which method will be used.</span></span> <span data-ttu-id="58ce7-348">Přidat Explict Desitnation 10.0.2.5, dvakrát klikněte na první prázdný řádek pod \<explicitní dest\> a v okně, které se zobrazí, zadejte adresu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-348">To add 10.0.2.5 as an Explict Desitnation, double-click on the first blank row under \<explicit-dest\> and enter the address in the window that pops up.</span></span>
  
    <span data-ttu-id="58ce7-349">Toto pravidlo předat není nutné žádné NAT vzhledem k tomu, že toto je interní provoz, takže metodu připojení může být nastaven na "Ne překládat pomocí SNAT".</span><span class="sxs-lookup"><span data-stu-id="58ce7-349">With this Pass Rule, no NAT is needed since this is internal traffic, so the Connection Method can be set to "No SNAT".</span></span>
  
    <span data-ttu-id="58ce7-350">**Poznámka:**: zdrojový sítě v tomto pravidle je jakémukoli prostředku, na podsíť FrontEnd, pokud bude existovat pouze jedna nebo známé konkrétní počet webových serverů, prostředek objektu sítě by bylo možné vytvořit být konkrétnější těchto přesnou IP adresy místo celé podsítě front-endu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-350">**Note**: The Source network in this rule is any resource on the FrontEnd subnet, if there will only be one, or a known specific number of web servers, a Network Object resource could be created to be more specific to those exact IP addresses instead of the entire Frontend subnet.</span></span>

> [!TIP]
> <span data-ttu-id="58ce7-351">Toto pravidlo používá službu "Žádné" usnadnění ukázkovou aplikaci nastavit a používat, to také umožní ICMPv4 (ping) v jediné pravidlo.</span><span class="sxs-lookup"><span data-stu-id="58ce7-351">This rule uses the service “Any” to make the sample application easier to setup and use, this will also allow ICMPv4 (ping) in a single rule.</span></span> <span data-ttu-id="58ce7-352">Je to ale není doporučený postup.</span><span class="sxs-lookup"><span data-stu-id="58ce7-352">However, this is not a recommended practice.</span></span> <span data-ttu-id="58ce7-353">Porty a protokoly ("služby") by měl být zúžit možné minimální, který umožňuje aplikaci operace redukovat prostor pro útok napříč tuto hranici.</span><span class="sxs-lookup"><span data-stu-id="58ce7-353">The ports and protocols (“Services”) should be narrowed to the minimum possible that allows application operation to reduce the attack surface across this boundary.</span></span>
> 
> 

<br />

> [!TIP]
> <span data-ttu-id="58ce7-354">I když toto pravidlo zobrazuje odkaz na explicitní dest používá, je třeba použít jednotný přístup v rámci konfigurace brány firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-354">Although this rule shows an explicit-dest reference being used, a consistent approach should be used throughout the firewall configuration.</span></span> <span data-ttu-id="58ce7-355">Doporučuje se použít s názvem objektu sítě v rámci pro snazší čitelnost a podpoře.</span><span class="sxs-lookup"><span data-stu-id="58ce7-355">It is recommended that the named Network Object be used throughout for easier readability and supportability.</span></span> <span data-ttu-id="58ce7-356">Explicitní dest tady je použita pouze k zobrazení metodu alternativní odkaz a se obecně nedoporučuje (hlavně u komplexní konfigurace).</span><span class="sxs-lookup"><span data-stu-id="58ce7-356">The explicit-dest is used here only to show an alternative reference method and is not generally recommended (especially for complex configurations).</span></span>
> 
> 

* <span data-ttu-id="58ce7-357">**Odchozí do internetové pravidlo**: předat toto pravidlo povolí provoz z jakékoli zdrojové síti mají být předána do vybrané cílové sítě.</span><span class="sxs-lookup"><span data-stu-id="58ce7-357">**Outbound to Internet Rule**: This Pass rule will allow traffic from any Source network to pass to the selected Destination networks.</span></span> <span data-ttu-id="58ce7-358">Toto pravidlo je výchozí pravidlo obvykle už v bráně firewall Barracuda NextGen, ale je v zakázaném stavu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-358">This rule is a default rule usually already on the Barracuda NextGen firewall, but is in a disabled state.</span></span> <span data-ttu-id="58ce7-359">Pravým tlačítkem myši na toto pravidlo můžete přístup k příkazu aktivovat pravidlo.</span><span class="sxs-lookup"><span data-stu-id="58ce7-359">Right-clicking on this rule can access the Activate Rule command.</span></span> <span data-ttu-id="58ce7-360">Přidejte dva místní podsítě, které byly vytvořeny jako odkazy v části požadavků tohoto dokumentu ke zdrojovému atributu tohoto pravidla se změnilo pravidlo zobrazeny zde.</span><span class="sxs-lookup"><span data-stu-id="58ce7-360">The rule shown here has been modified to add the two local subnets that were created as references in the prerequisite section of this document to the Source attribute of this rule.</span></span>
  
    <span data-ttu-id="58ce7-361">![Odchozí pravidlo brány firewall][14]</span><span class="sxs-lookup"><span data-stu-id="58ce7-361">![Firewall Outbound Rule][14]</span></span>
* <span data-ttu-id="58ce7-362">**Pravidlo DNS**: předat toto pravidlo umožňuje předat serveru DNS pouze provoz DNS (port 53).</span><span class="sxs-lookup"><span data-stu-id="58ce7-362">**DNS Rule**: This Pass rule allows only DNS (port 53) traffic to pass to the DNS server.</span></span> <span data-ttu-id="58ce7-363">Pro toto prostředí, které se většina provoz z front-endu na back-end je blokovaný konkrétně toto pravidlo umožňuje DNS.</span><span class="sxs-lookup"><span data-stu-id="58ce7-363">For this environment most traffic from the FrontEnd to the BackEnd is blocked, this rule specifically allows DNS.</span></span>
  
    <span data-ttu-id="58ce7-364">![Pravidlo brány firewall DNS][15]</span><span class="sxs-lookup"><span data-stu-id="58ce7-364">![Firewall DNS Rule][15]</span></span>
  
    <span data-ttu-id="58ce7-365">**Poznámka:**: na této obrazovce snímek metoda připojení je součástí.</span><span class="sxs-lookup"><span data-stu-id="58ce7-365">**Note**: In this screen shot the Connection Method is included.</span></span> <span data-ttu-id="58ce7-366">Protože toto pravidlo je pro interní IP adresu pro interní IP adresu provoz, žádné NATing je vyžadován, tato metoda připojení nastavena na "Ne překládat pomocí SNAT" pro toto pravidlo průchodu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-366">Because this rule is for internal IP to internal IP address traffic, no NATing is required, this the Connection Method is set to “No SNAT” for this Pass rule.</span></span>
* <span data-ttu-id="58ce7-367">**Pravidlo podsítě pro podsíť**: předat toto pravidlo je výchozí pravidlo, které se aktivuje a upravit tak, aby povolit všechny servery v podsíti back-end připojení k libovolnému serveru na podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="58ce7-367">**Subnet to Subnet Rule**: This Pass rule is a default rule that has been activated and modified to allow any server on the back end subnet to connect to any server on the front end subnet.</span></span> <span data-ttu-id="58ce7-368">Toto pravidlo je všechny interní provoz, takže metodu připojení může být nastaven na žádný překládat pomocí SNAT.</span><span class="sxs-lookup"><span data-stu-id="58ce7-368">This rule is all internal traffic so the Connection Method can be set to No SNAT.</span></span>
  
    <span data-ttu-id="58ce7-369">![Pravidlo brány firewall Intra-VNet][16]</span><span class="sxs-lookup"><span data-stu-id="58ce7-369">![Firewall Intra-VNet Rule][16]</span></span>
  
    <span data-ttu-id="58ce7-370">**Poznámka:**: zaškrtávací políčko obousměrný nastavení není kontrolován (ani je zaregistrováno většina pravidla), to je důležité pro toto pravidlo, umožňuje tato pravidla "jeden směrové", připojení lze inicializovat z back-end podsítě síť front-end, ale nikoli naopak.</span><span class="sxs-lookup"><span data-stu-id="58ce7-370">**Note**: The Bi-directional checkbox is not checked (nor is it checked in most rules), this is significant for this rule in that it makes this rule “one directional”, a connection can be initiated from the back end subnet to the front end network, but not the reverse.</span></span> <span data-ttu-id="58ce7-371">Pokud toto zaškrtávací políčko je zaškrtnuto, by toto pravidlo povolit obousměrný přenos, který je z našich logického diagramu není žádoucí.</span><span class="sxs-lookup"><span data-stu-id="58ce7-371">If that checkbox was checked, this rule would enable bi-directional traffic, which from our logical diagram is not desired.</span></span>
* <span data-ttu-id="58ce7-372">**Všechny přenosy pravidlo Odepřít**: to by mělo být vždy poslední pravidlo (z hlediska priorita), a jako takový Pokud selže tak, aby odpovídaly některé z přenosy podle předchozích pravidel se zahodí tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="58ce7-372">**Deny All Traffic Rule**: This should always be the final rule (in terms of priority), and as such if a traffic flows fails to match any of the preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="58ce7-373">Toto je výchozí pravidlo a obvykle aktivaci žádné je obecně nutné provést změny.</span><span class="sxs-lookup"><span data-stu-id="58ce7-373">This is a default rule and usually activated, no modifications are generally needed.</span></span> 
  
    <span data-ttu-id="58ce7-374">![Pravidlo brány firewall Odepřít][17]</span><span class="sxs-lookup"><span data-stu-id="58ce7-374">![Firewall Deny Rule][17]</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58ce7-375">Po vytvoření všech výše uvedených pravidel, je důležité zkontrolovat prioritu každé pravidlo zajistit provoz se povolí nebo zakáže podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="58ce7-375">Once all of the above rules are created, it’s important to review the priority of each rule to ensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="58ce7-376">V tomto příkladu jsou pravidla v pořadí, ve kterém by se zobrazit v hlavní mřížky předávání pravidla v Barracuda správy klienta.</span><span class="sxs-lookup"><span data-stu-id="58ce7-376">For this example, the rules are in the order they should appear in the Main Grid of forwarding rules in the Barracuda Management Client.</span></span>
> 
> 

## <a name="rule-activation"></a><span data-ttu-id="58ce7-377">Aktivace pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-377">Rule Activation</span></span>
<span data-ttu-id="58ce7-378">S ruleset upravit tak, aby specifikace diagramu logiku musí být nahrán do brány firewall a pak se aktivuje ruleset.</span><span class="sxs-lookup"><span data-stu-id="58ce7-378">With the ruleset modified to the specification of the logic diagram, the ruleset must be uploaded to the firewall and then activated.</span></span>

<span data-ttu-id="58ce7-379">![Aktivace pravidla brány firewall][18]</span><span class="sxs-lookup"><span data-stu-id="58ce7-379">![Firewall Rule Activation][18]</span></span>

<span data-ttu-id="58ce7-380">V pravém horním rohu správy klienta jsou součástí clusteru tlačítka.</span><span class="sxs-lookup"><span data-stu-id="58ce7-380">In the upper right hand corner of the management client are a cluster of buttons.</span></span> <span data-ttu-id="58ce7-381">Kliknutím na tlačítko "Změny odeslat" poslat upravené pravidla brány firewall a potom klikněte na tlačítko "Aktivovat".</span><span class="sxs-lookup"><span data-stu-id="58ce7-381">Click the “Send Changes” button to send the modified rules to the firewall, then click the “Activate” button.</span></span>

<span data-ttu-id="58ce7-382">S aktivace sada pravidel brány firewall pro tento příklad prostředí sestavení je dokončena.</span><span class="sxs-lookup"><span data-stu-id="58ce7-382">With the activation of the firewall ruleset this example environment build is complete.</span></span>

## <a name="traffic-scenarios"></a><span data-ttu-id="58ce7-383">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="58ce7-383">Traffic Scenarios</span></span>
> [!IMPORTANT]
> <span data-ttu-id="58ce7-384">Nezapomeňte, že klíče takeway je **všechny** provoz se odešlou přes bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="58ce7-384">A key takeway is to remember that **all** traffic will come through the firewall.</span></span> <span data-ttu-id="58ce7-385">Proto vzdálené plochy k serveru IIS01, i když je počítač v cloudové službě Front End a na podsítě Front End, pro přístup k tomuto serveru jsme bude muset RDP do brány firewall na portu 8014 a potom povolit bránu firewall pro směrování požadavku protokolu RDP interně k portu RDP IIS01.</span><span class="sxs-lookup"><span data-stu-id="58ce7-385">So to remote desktop to the IIS01 server, even though it's in the Front End Cloud Service and on the Front End subnet, to access this server we will need to RDP to the firewall on port 8014, and then allow the firewall to route the RDP request internally to the IIS01 RDP Port.</span></span> <span data-ttu-id="58ce7-386">Tlačítko "Připojit" portálu Azure nebude fungovat, protože neexistuje přímé cesta protokolu RDP na IIS01 (jde o můžete zobrazit na portálu).</span><span class="sxs-lookup"><span data-stu-id="58ce7-386">The Azure portal's "Connect" button won't work because there is no direct RDP path to IIS01 (as far as the portal can see).</span></span> <span data-ttu-id="58ce7-387">To znamená, že všechna připojení z Internetu bude služby zabezpečení a Port, například secscv001.cloudapp.net:xxxx.</span><span class="sxs-lookup"><span data-stu-id="58ce7-387">This means all connections from the internet will be to the Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx.</span></span>
> 
> 

<span data-ttu-id="58ce7-388">Pro tyto scénáře následující pravidla brány firewall musí být k dispozici:</span><span class="sxs-lookup"><span data-stu-id="58ce7-388">For these scenarios, the following firewall rules should be in place:</span></span>

1. <span data-ttu-id="58ce7-389">Správa brány firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-389">Firewall Management</span></span>
2. <span data-ttu-id="58ce7-390">RDP na IIS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-390">RDP to IIS01</span></span>
3. <span data-ttu-id="58ce7-391">RDP na DNS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-391">RDP to DNS01</span></span>
4. <span data-ttu-id="58ce7-392">RDP na AppVM01</span><span class="sxs-lookup"><span data-stu-id="58ce7-392">RDP to AppVM01</span></span>
5. <span data-ttu-id="58ce7-393">RDP na AppVM02</span><span class="sxs-lookup"><span data-stu-id="58ce7-393">RDP to AppVM02</span></span>
6. <span data-ttu-id="58ce7-394">Provoz aplikace na webu</span><span class="sxs-lookup"><span data-stu-id="58ce7-394">App Traffic to the Web</span></span>
7. <span data-ttu-id="58ce7-395">Provoz aplikace AppVM01</span><span class="sxs-lookup"><span data-stu-id="58ce7-395">App Traffic to AppVM01</span></span>
8. <span data-ttu-id="58ce7-396">Odchozí do Internetu</span><span class="sxs-lookup"><span data-stu-id="58ce7-396">Outbound to the Internet</span></span>
9. <span data-ttu-id="58ce7-397">Front-endu do DNS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-397">Frontend to DNS01</span></span>
10. <span data-ttu-id="58ce7-398">Přenosy mezi podsítěmi (back-endu front-endu pouze)</span><span class="sxs-lookup"><span data-stu-id="58ce7-398">Intra-Subnet Traffic (back end to front end only)</span></span>
11. <span data-ttu-id="58ce7-399">Odepřít vše</span><span class="sxs-lookup"><span data-stu-id="58ce7-399">Deny All</span></span>

<span data-ttu-id="58ce7-400">Ruleset skutečné brány firewall budou s největší pravděpodobností mít mnoho pravidla kromě těchto, pravidla na jakékoli dané brány firewall bude také obsahovat čísla jinou prioritu než ty, které jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="58ce7-400">The actual firewall ruleset will most likely have many other rules in addition to these, the rules on any given firewall will also have different priority numbers than the ones listed here.</span></span> <span data-ttu-id="58ce7-401">Tento seznam a přidružená čísla jsou zajistit relevance mezi právě tyto 11 pravidla a relativní Priorita jedna z nich.</span><span class="sxs-lookup"><span data-stu-id="58ce7-401">This list and associated numbers are to provide relevance between just these eleven rules and the relative priority amongst them.</span></span> <span data-ttu-id="58ce7-402">Jinými slovy; v bráně firewall skutečné "protokolu RDP na IIS01" může být číslo pravidla 5, ale dokud je nižší než pravidlo "Brány Firewall správy" a výše "Protokolu RDP na DNS01" pravidlo by zarovnat tak, aby tento seznam.</span><span class="sxs-lookup"><span data-stu-id="58ce7-402">In other words; on the actual firewall, the “RDP to IIS01” may be rule number 5, but as long as it’s below the “Firewall Management” rule and above the “RDP to DNS01” rule it would align with the intention of this list.</span></span> <span data-ttu-id="58ce7-403">V seznamu bude také pomáhají při níže scénáře povolení jako stručný výtah; například "Pravidlo FW 9 (DNS)".</span><span class="sxs-lookup"><span data-stu-id="58ce7-403">The list will also aid in the below scenarios allowing brevity; e.g. “FW Rule 9 (DNS)”.</span></span> <span data-ttu-id="58ce7-404">Také jako stručný výtah čtyři pravidla RDP souhrnně volaná, "pravidla RDP" Pokud scénář provoz nesouvisí s RDP.</span><span class="sxs-lookup"><span data-stu-id="58ce7-404">Also for brevity, the four RDP rules will be collectively called, “the RDP rules” when the traffic scenario is unrelated to RDP.</span></span>

<span data-ttu-id="58ce7-405">Taky odvolat, že skupiny zabezpečení sítě jsou místní pro příchozí přenosy z Internetu v podsítích front-endové a back-end.</span><span class="sxs-lookup"><span data-stu-id="58ce7-405">Also recall that Network Security Groups are in-place for inbound internet traffic on the Frontend and Backend subnets.</span></span>

#### <a name="allowed-internet-to-web-server"></a><span data-ttu-id="58ce7-406">(Povoleno) Internet na webový Server</span><span class="sxs-lookup"><span data-stu-id="58ce7-406">(Allowed) Internet to Web Server</span></span>
1. <span data-ttu-id="58ce7-407">Internet uživatelské požadavky HTTP stránky z SecSvc001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="58ce7-407">Internet user requests HTTP page from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="58ce7-408">Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 k rozhraní brány firewall na 10.0.0.4:80</span><span class="sxs-lookup"><span data-stu-id="58ce7-408">Cloud service passes traffic through open endpoint on port 80 to firewall interface on 10.0.0.4:80</span></span>
3. <span data-ttu-id="58ce7-409">Žádné skupiny NSG přiřazené podsítě zabezpečení, tak pravidla NSG systému povolit provoz do brány firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-409">No NSG assigned to Security subnet, so system NSG rules allow traffic to firewall</span></span>
4. <span data-ttu-id="58ce7-410">Provoz dotkne interní IP adresu brány firewall (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="58ce7-410">Traffic hits internal IP address of the firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="58ce7-411">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-411">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="58ce7-412">Není použít, přejděte k další pravidla FW pravidla 1 (FW Mgmt)</span><span class="sxs-lookup"><span data-stu-id="58ce7-412">FW Rule 1 (FW Mgmt) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="58ce7-413">Nemusíte použít, přejděte k další pravidla FW pravidla 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="58ce7-413">FW Rules 2 - 5 (RDP Rules) don’t apply, move to next rule</span></span>
   3. <span data-ttu-id="58ce7-414">FW pravidla 6 (aplikace: webové) použít, provoz je povolený, zařízení brány firewall NAT. jeho 10.0.1.4 (IIS01)</span><span class="sxs-lookup"><span data-stu-id="58ce7-414">FW Rule 6 (App: Web) does apply, traffic is allowed, firewall NATs it to 10.0.1.4 (IIS01)</span></span>
6. <span data-ttu-id="58ce7-415">Podsíť Frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-415">The Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="58ce7-416">Netýká NSG pravidlo 1 (bloku Internet) (Tento provoz byl NAT by bránou firewall, proto je zdrojovou adresu nyní brány firewall, která je v podsíti, zabezpečení a prohlížet podsíť Frontend NSG jako "místní" provoz a je proto povolen), přesunout do dalšího pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-416">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by the firewall, thus the source address is now the firewall which is on the Security subnet and seen by the Frontend subnet NSG to be “local” traffic and is thus allowed), move to next rule</span></span>
   2. <span data-ttu-id="58ce7-417">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-417">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="58ce7-418">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku</span><span class="sxs-lookup"><span data-stu-id="58ce7-418">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
8. <span data-ttu-id="58ce7-419">IIS01 pokusy o zahájí relace FTP na AppVM01 v podsíti back-end</span><span class="sxs-lookup"><span data-stu-id="58ce7-419">IIS01 attempts to initiates an FTP session to AppVM01 on Backend subnet</span></span>
9. <span data-ttu-id="58ce7-420">UDR trasy na podsíť Frontend umožňuje bráně firewall dalšího směrování</span><span class="sxs-lookup"><span data-stu-id="58ce7-420">The UDR route on Frontend subnet makes the firewall the next hop</span></span>
10. <span data-ttu-id="58ce7-421">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="58ce7-421">No outbound rules on Frontend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="58ce7-422">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-422">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="58ce7-423">Není použít, přejděte k další pravidla FW pravidla 1 (FW Mgmt)</span><span class="sxs-lookup"><span data-stu-id="58ce7-423">FW Rule 1 (FW Mgmt) doesn’t apply, move to next rule</span></span>
    2. <span data-ttu-id="58ce7-424">Nemusíte použít, přejděte k další pravidla FW pravidlo 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="58ce7-424">FW Rule 2 - 5 (RDP Rules) don’t apply, move to next rule</span></span>
    3. <span data-ttu-id="58ce7-425">FW pravidla 6 (aplikace: webové) není použít, přejděte k další pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-425">FW Rule 6 (App: Web) doesn’t apply, move to next rule</span></span>
    4. <span data-ttu-id="58ce7-426">FW pravidla 7 (aplikace: back-end) použít, provoz je povolený, brány firewall předává přenos do 10.0.2.5 (AppVM01)</span><span class="sxs-lookup"><span data-stu-id="58ce7-426">FW Rule 7 (App: Backend) does apply, traffic is allowed, firewall forwards traffic to 10.0.2.5 (AppVM01)</span></span>
12. <span data-ttu-id="58ce7-427">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-427">The Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="58ce7-428">Není použít, přejděte k další pravidla NSG pravidlo 1 (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-428">NSG Rule 1 (Block Internet) doesn’t apply, move to next rule</span></span>
    2. <span data-ttu-id="58ce7-429">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-429">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
13. <span data-ttu-id="58ce7-430">AppVM01 obdrží požadavek a inicializuje relaci a odpovídá</span><span class="sxs-lookup"><span data-stu-id="58ce7-430">AppVM01 receives the request and initiates the session and responds</span></span>
14. <span data-ttu-id="58ce7-431">UDR trasy v podsíti back-end umožňuje bráně firewall dalšího směrování</span><span class="sxs-lookup"><span data-stu-id="58ce7-431">The UDR route on Backend subnet makes the firewall the next hop</span></span>
15. <span data-ttu-id="58ce7-432">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na podsítě back-end odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="58ce7-432">Since there are no outbound NSG rules on the Backend subnet the response is allowed</span></span>
16. <span data-ttu-id="58ce7-433">Protože to vrací provoz na navázanou relaci předává odpověď zpět do webového serveru (IIS01)</span><span class="sxs-lookup"><span data-stu-id="58ce7-433">Because this is returning traffic on an established session the firewall passes the response back to the web server (IIS01)</span></span>
17. <span data-ttu-id="58ce7-434">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-434">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="58ce7-435">Není použít, přejděte k další pravidla NSG pravidlo 1 (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-435">NSG Rule 1 (Block Internet) doesn’t apply, move to next rule</span></span>
    2. <span data-ttu-id="58ce7-436">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-436">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
18. <span data-ttu-id="58ce7-437">Server služby IIS obdrží odpověď, dokončení transakce s AppVM01 a potom dokončí vytváření odpovědi HTTP, tato odpověď HTTP posílá žadatel</span><span class="sxs-lookup"><span data-stu-id="58ce7-437">The IIS server receives the response, completes the transaction with AppVM01, and then completes building the HTTP response, this HTTP response is sent to the requestor</span></span>
19. <span data-ttu-id="58ce7-438">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na podsítě front-endu odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="58ce7-438">Since there are no outbound NSG rules on the Frontend subnet the response is allowed</span></span>
20. <span data-ttu-id="58ce7-439">Odpověď HTTP dotkne brány firewall a vzhledem k tomu, že toto je odpověď na navázanou relaci NAT přijat branou firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-439">The HTTP response hits the firewall, and because this is the response to an established NAT session is accepted by the firewall</span></span>
21. <span data-ttu-id="58ce7-440">Brána firewall pak přesměruje odpověď zpět do Internetu uživatele</span><span class="sxs-lookup"><span data-stu-id="58ce7-440">The firewall then redirects the response back to the Internet User</span></span>
22. <span data-ttu-id="58ce7-441">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG nebo UDR směrování na front-endu podsítě, které je povoleno odpovědi a Internet uživatel obdrží požadované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="58ce7-441">Since there are no outbound NSG rules or UDR hops on the Frontend subnet the response is allowed, and the Internet User receives the web page requested.</span></span>

#### <a name="allowed-internet-rdp-to-backend"></a><span data-ttu-id="58ce7-442">(Povoleno) Internet protokolu RDP na back-end</span><span class="sxs-lookup"><span data-stu-id="58ce7-442">(Allowed) Internet RDP to Backend</span></span>
1. <span data-ttu-id="58ce7-443">Správce serveru na Internetu požadavky protokolu RDP relace AppVM01 prostřednictvím SecSvc001.CloudApp.Net:8025, kde 8025 je číslo portu přiřazeného uživatele pro pravidlo brány firewall "Protokolu RDP na AppVM01"</span><span class="sxs-lookup"><span data-stu-id="58ce7-443">Server Admin on internet requests RDP session to AppVM01 via SecSvc001.CloudApp.Net:8025, where 8025 is the user assigned port number for the “RDP to AppVM01” firewall rule</span></span>
2. <span data-ttu-id="58ce7-444">Cloudové služby předá provoz prostřednictvím otevřených koncových bodů na portu 8025 rozhraní brány firewall na 10.0.0.4:8025</span><span class="sxs-lookup"><span data-stu-id="58ce7-444">The cloud service passes traffic through the open endpoint on port 8025 to firewall interface on 10.0.0.4:8025</span></span>
3. <span data-ttu-id="58ce7-445">Žádné skupiny NSG přiřazené podsítě zabezpečení, tak pravidla NSG systému povolit provoz do brány firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-445">No NSG assigned to Security subnet, so system NSG rules allow traffic to firewall</span></span>
4. <span data-ttu-id="58ce7-446">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-446">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="58ce7-447">Není použít, přejděte k další pravidla FW pravidla 1 (FW Mgmt)</span><span class="sxs-lookup"><span data-stu-id="58ce7-447">FW Rule 1 (FW Mgmt) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="58ce7-448">FW pravidla 2 (RDP IIS) není použít, přejděte k další pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-448">FW Rule 2 (RDP IIS) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="58ce7-449">FW pravidla 3 (RDP DNS01) není použít, přejděte k další pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-449">FW Rule 3 (RDP DNS01) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="58ce7-450">Použít FW pravidla 4 (RDP AppVM01), provoz je povolený, zařízení brány firewall NAT. jeho 10.0.2.5:3386 (portu RDP na AppVM01)</span><span class="sxs-lookup"><span data-stu-id="58ce7-450">FW Rule 4 (RDP AppVM01) does apply, traffic is allowed, firewall NATs it to 10.0.2.5:3386 (RDP port on AppVM01)</span></span>
5. <span data-ttu-id="58ce7-451">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-451">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="58ce7-452">Netýká NSG pravidlo 1 (bloku Internet) (Tento provoz byl NAT by bránou firewall, proto je zdrojovou adresu nyní brány firewall, která je v podsíti, zabezpečení a prohlížet podsíť back-end NSG jako "místní" provoz a je proto povolen), přesunout do dalšího pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-452">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by the firewall, thus the source address is now the firewall which is on the Security subnet and seen by the Backend subnet NSG to be “local” traffic and is thus allowed), move to next rule</span></span>
   2. <span data-ttu-id="58ce7-453">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-453">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="58ce7-454">AppVM01 naslouchá pro provoz protokolu RDP a odpovídá</span><span class="sxs-lookup"><span data-stu-id="58ce7-454">AppVM01 is listening for RDP traffic and responds</span></span>
7. <span data-ttu-id="58ce7-455">Žádná odchozí pravidla NSG použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="58ce7-455">With no outbound NSG rules, default rules apply and return traffic is allowed</span></span>
8. <span data-ttu-id="58ce7-456">UDR odchozí provoz směrovat na bránu firewall jako další segment</span><span class="sxs-lookup"><span data-stu-id="58ce7-456">UDR routes outbound traffic to the firewall as the next hop</span></span>
9. <span data-ttu-id="58ce7-457">Protože to vrací provoz na navázanou relaci předává odpověď zpět do Internetu uživatele</span><span class="sxs-lookup"><span data-stu-id="58ce7-457">Because this is returning traffic on an established session the firewall passes the response back to the internet user</span></span>
10. <span data-ttu-id="58ce7-458">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="58ce7-458">RDP session is enabled</span></span>
11. <span data-ttu-id="58ce7-459">AppVM01 vyzve k zadání názvu heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="58ce7-459">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="58ce7-460">(Povoleno) Webový Server DNS vyhledávání na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="58ce7-460">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="58ce7-461">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale musí pro překlad adres.</span><span class="sxs-lookup"><span data-stu-id="58ce7-461">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="58ce7-462">Konfigurace sítě pro virtuální síť seznamy DNS01 (10.0.2.4 v podsíti back-end) jako primární server DNS, IIS01 odešle žádost DNS do DNS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-462">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="58ce7-463">UDR odchozí provoz směrovat na bránu firewall jako další segment</span><span class="sxs-lookup"><span data-stu-id="58ce7-463">UDR routes outbound traffic to the firewall as the next hop</span></span>
4. <span data-ttu-id="58ce7-464">Žádná odchozí pravidla NSG je vázána na podsíť Frontend, provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="58ce7-464">No outbound NSG rules are bound to the Frontend subnet, traffic is allowed</span></span>
5. <span data-ttu-id="58ce7-465">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-465">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="58ce7-466">Není použít, přejděte k další pravidla FW pravidla 1 (FW Mgmt)</span><span class="sxs-lookup"><span data-stu-id="58ce7-466">FW Rule 1 (FW Mgmt) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="58ce7-467">Nemusíte použít, přejděte k další pravidla FW pravidlo 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="58ce7-467">FW Rule 2 - 5 (RDP Rules) don’t apply, move to next rule</span></span>
   3. <span data-ttu-id="58ce7-468">Nemusíte použít, přejděte k další pravidla FW pravidla 6 a 7 (pravidla aplikace)</span><span class="sxs-lookup"><span data-stu-id="58ce7-468">FW Rules 6 & 7 (App Rules) don’t apply, move to next rule</span></span>
   4. <span data-ttu-id="58ce7-469">FW pravidlo 8 (Internet k) není použít, přejděte k další pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-469">FW Rule 8 (To Internet) doesn’t apply, move to next rule</span></span>
   5. <span data-ttu-id="58ce7-470">Použít FW pravidlo 9 (DNS), provoz je povolený, brány firewall předává přenos do 10.0.2.4 (DNS01)</span><span class="sxs-lookup"><span data-stu-id="58ce7-470">FW Rule 9 (DNS) does apply, traffic is allowed, firewall forwards traffic to 10.0.2.4 (DNS01)</span></span>
6. <span data-ttu-id="58ce7-471">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-471">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="58ce7-472">Není použít, přejděte k další pravidla NSG pravidlo 1 (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-472">NSG Rule 1 (Block Internet) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="58ce7-473">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-473">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="58ce7-474">DNS server obdrží požadavek</span><span class="sxs-lookup"><span data-stu-id="58ce7-474">DNS server receives the request</span></span>
8. <span data-ttu-id="58ce7-475">DNS server nemá adresu do mezipaměti a požádá kořenový server DNS na Internetu</span><span class="sxs-lookup"><span data-stu-id="58ce7-475">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
9. <span data-ttu-id="58ce7-476">UDR odchozí provoz směrovat na bránu firewall jako další segment</span><span class="sxs-lookup"><span data-stu-id="58ce7-476">UDR routes outbound traffic to the firewall as the next hop</span></span>
10. <span data-ttu-id="58ce7-477">Žádná odchozí pravidla NSG na podsítě back-end provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="58ce7-477">No outbound NSG rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="58ce7-478">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-478">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="58ce7-479">Není použít, přejděte k další pravidla FW pravidla 1 (FW Mgmt)</span><span class="sxs-lookup"><span data-stu-id="58ce7-479">FW Rule 1 (FW Mgmt) doesn’t apply, move to next rule</span></span>
    2. <span data-ttu-id="58ce7-480">Nemusíte použít, přejděte k další pravidla FW pravidlo 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="58ce7-480">FW Rule 2 - 5 (RDP Rules) don’t apply, move to next rule</span></span>
    3. <span data-ttu-id="58ce7-481">Nemusíte použít, přejděte k další pravidla FW pravidla 6 a 7 (pravidla aplikace)</span><span class="sxs-lookup"><span data-stu-id="58ce7-481">FW Rules 6 & 7 (App Rules) don’t apply, move to next rule</span></span>
    4. <span data-ttu-id="58ce7-482">Použít pravidlo 8 FW (do Internetu), provoz je povolený, relace je překládat pomocí SNAT na kořenový server DNS na Internetu</span><span class="sxs-lookup"><span data-stu-id="58ce7-482">FW Rule 8 (To Internet) does apply, traffic is allowed, session is SNAT out to root DNS server on the Internet</span></span>
12. <span data-ttu-id="58ce7-483">Server DNS pro Internet odpoví, protože tato relace byla inicializována z brány firewall, odpovědi je přijímán bránou firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-483">Internet DNS server responds, since this session was initiated from the firewall, the response is accepted by the firewall</span></span>
13. <span data-ttu-id="58ce7-484">Protože se jedná navázanou relaci, brána firewall předává odpověď na inicializace serveru DNS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-484">As this is an established session, the firewall forwards the response to the initiating server, DNS01</span></span>
14. <span data-ttu-id="58ce7-485">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-485">The Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="58ce7-486">Není použít, přejděte k další pravidla NSG pravidlo 1 (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-486">NSG Rule 1 (Block Internet) doesn’t apply, move to next rule</span></span>
    2. <span data-ttu-id="58ce7-487">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-487">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
15. <span data-ttu-id="58ce7-488">DNS server obdrží odpověď do mezipaměti a poté odpoví na počáteční žádost zpět na IIS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-488">The DNS server receives and caches the response, and then responds to the initial request back to IIS01</span></span>
16. <span data-ttu-id="58ce7-489">UDR trasy v podsíti back-end umožňuje bráně firewall dalšího směrování</span><span class="sxs-lookup"><span data-stu-id="58ce7-489">The UDR route on backend subnet makes the firewall the next hop</span></span>
17. <span data-ttu-id="58ce7-490">Neexistují žádná odchozí pravidla NSG na podsítě back-end, provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="58ce7-490">No outbound NSG rules exist on the Backend subnet, traffic is allowed</span></span>
18. <span data-ttu-id="58ce7-491">Toto je navázanou relaci v bráně firewall, odpověď se předá brány firewall zpět na server služby IIS</span><span class="sxs-lookup"><span data-stu-id="58ce7-491">This is an established session on the firewall, the response is forwarded by the firewall back to the IIS server</span></span>
19. <span data-ttu-id="58ce7-492">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-492">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="58ce7-493">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="58ce7-493">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="58ce7-494">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povoleno</span><span class="sxs-lookup"><span data-stu-id="58ce7-494">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
20. <span data-ttu-id="58ce7-495">IIS01 obdrží odpověď od DNS01</span><span class="sxs-lookup"><span data-stu-id="58ce7-495">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-backend-server-to-frontend-server"></a><span data-ttu-id="58ce7-496">(Povoleno) Back-end serveru na server pro front-endu</span><span class="sxs-lookup"><span data-stu-id="58ce7-496">(Allowed) Backend server to Frontend server</span></span>
1. <span data-ttu-id="58ce7-497">Správce přihlášený k AppVM02 prostřednictvím protokolu RDP požádá o soubor přímo ze serveru IIS01 pomocí Průzkumníka souborů systému windows</span><span class="sxs-lookup"><span data-stu-id="58ce7-497">An administrator logged on to AppVM02 via RDP requests a file directly from the IIS01 server via windows file explorer</span></span>
2. <span data-ttu-id="58ce7-498">UDR trasy v podsíti back-end umožňuje bráně firewall dalšího směrování</span><span class="sxs-lookup"><span data-stu-id="58ce7-498">The UDR route on Backend subnet makes the firewall the next hop</span></span>
3. <span data-ttu-id="58ce7-499">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na podsítě back-end odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="58ce7-499">Since there are no outbound NSG rules on the Backend subnet the response is allowed</span></span>
4. <span data-ttu-id="58ce7-500">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-500">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="58ce7-501">Není použít, přejděte k další pravidla FW pravidla 1 (FW Mgmt)</span><span class="sxs-lookup"><span data-stu-id="58ce7-501">FW Rule 1 (FW Mgmt) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="58ce7-502">Nemusíte použít, přejděte k další pravidla FW pravidlo 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="58ce7-502">FW Rule 2 - 5 (RDP Rules) don’t apply, move to next rule</span></span>
   3. <span data-ttu-id="58ce7-503">Nemusíte použít, přejděte k další pravidla FW pravidla 6 a 7 (pravidla aplikace)</span><span class="sxs-lookup"><span data-stu-id="58ce7-503">FW Rules 6 & 7 (App Rules) don’t apply, move to next rule</span></span>
   4. <span data-ttu-id="58ce7-504">FW pravidlo 8 (Internet k) není použít, přejděte k další pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-504">FW Rule 8 (To Internet) doesn’t apply, move to next rule</span></span>
   5. <span data-ttu-id="58ce7-505">Není použít, přejděte k další pravidla FW pravidlo 9 (DNS)</span><span class="sxs-lookup"><span data-stu-id="58ce7-505">FW Rule 9 (DNS) doesn’t apply, move to next rule</span></span>
   6. <span data-ttu-id="58ce7-506">Použít pravidlo 10 FW (Intra-podsítě), provoz je povolený, brány firewall předá provoz 10.0.1.4 (IIS01)</span><span class="sxs-lookup"><span data-stu-id="58ce7-506">FW Rule 10 (Intra-Subnet) does apply, traffic is allowed, firewall passes traffic to 10.0.1.4 (IIS01)</span></span>
5. <span data-ttu-id="58ce7-507">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-507">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="58ce7-508">Není použít, přejděte k další pravidla NSG pravidlo 1 (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-508">NSG Rule 1 (Block Internet) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="58ce7-509">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-509">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="58ce7-510">Za předpokladu, že správné ověření a autorizaci, IIS01 přijme žádost a odpovídá</span><span class="sxs-lookup"><span data-stu-id="58ce7-510">Assuming proper authentication and authorization, IIS01 accepts the request and responds</span></span>
7. <span data-ttu-id="58ce7-511">UDR trasy na podsíť Frontend umožňuje bráně firewall dalšího směrování</span><span class="sxs-lookup"><span data-stu-id="58ce7-511">The UDR route on Frontend subnet makes the firewall the next hop</span></span>
8. <span data-ttu-id="58ce7-512">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na podsítě front-endu odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="58ce7-512">Since there are no outbound NSG rules on the Frontend subnet the response is allowed</span></span>
9. <span data-ttu-id="58ce7-513">Protože se jedná o existující relaci v bráně firewall této odpovědi je povolen a bránu firewall vrátí odpověď na AppVM02</span><span class="sxs-lookup"><span data-stu-id="58ce7-513">As this is an existing session on the firewall this response is allowed and the firewall returns the response to AppVM02</span></span>
10. <span data-ttu-id="58ce7-514">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="58ce7-514">Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="58ce7-515">Není použít, přejděte k další pravidla NSG pravidlo 1 (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-515">NSG Rule 1 (Block Internet) doesn’t apply, move to next rule</span></span>
    2. <span data-ttu-id="58ce7-516">Výchozí pravidla NSG povolit podsítě pro podsíť provozu, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-516">Default NSG Rules allow subnet to subnet traffic, traffic is allowed, stop NSG rule processing</span></span>
11. <span data-ttu-id="58ce7-517">AppVM02 obdrží odpověď</span><span class="sxs-lookup"><span data-stu-id="58ce7-517">AppVM02 receives the response</span></span>

#### <a name="denied-internet-direct-to-web-server"></a><span data-ttu-id="58ce7-518">(Byl odepřen) Internet přímo na webovém serveru</span><span class="sxs-lookup"><span data-stu-id="58ce7-518">(Denied) Internet direct to Web Server</span></span>
1. <span data-ttu-id="58ce7-519">Uživatel Internetu pokusí o přístup k webovému serveru, IIS01, prostřednictvím služby FrontEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="58ce7-519">Internet user tries to access the web server, IIS01, through the FrontEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="58ce7-520">Vzhledem k tomu, že jsou pro přenos HTTP otevřené žádné koncové body, se nebude předávat cloudové služby a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="58ce7-520">Since there are no endpoints open for HTTP traffic, this would not pass through the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="58ce7-521">Pokud z nějakého důvodu byly otevřené koncových bodů, by tento provoz blokovat NSG (bloku Internet) na podsíť Frontend</span><span class="sxs-lookup"><span data-stu-id="58ce7-521">If the endpoints were open for some reason, the NSG (Block Internet) on the Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="58ce7-522">Nakonec trasy UDR podsítě front-endu byste odesílali všechny odchozí přenosy z IIS01 do brány firewall jako další segment a bránu firewall by najdete jako asymetrický provoz a vyřadit odchozí odpovědi proto existují aspoň tři nezávislá vrstev obrany mezi internet a IIS01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-522">Finally, the Frontend subnet UDR route would send any outbound traffic from IIS01 to the firewall as the next hop, and the firewall would see this as asymmetric traffic and drop the outbound response Thus there are at least three independent layers of defense between the internet and IIS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-to-backend-server"></a><span data-ttu-id="58ce7-523">(Byl odepřen) Internet back-end server</span><span class="sxs-lookup"><span data-stu-id="58ce7-523">(Denied) Internet to Backend Server</span></span>
1. <span data-ttu-id="58ce7-524">Internet uživatel pokusí přistoupit k souboru na AppVM01 prostřednictvím služby BackEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="58ce7-524">Internet user tries to access a file on AppVM01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="58ce7-525">Vzhledem k tomu, že jsou pro sdílené složky otevřené žádné koncové body, se nebude předat cloudové služby a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="58ce7-525">Since there are no endpoints open for file share, this would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="58ce7-526">Pokud z nějakého důvodu byly otevřené koncových bodů, by tento provoz blokovat NSG (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="58ce7-526">If the endpoints were open for some reason, the NSG (Block Internet) would block this traffic</span></span>
4. <span data-ttu-id="58ce7-527">Nakonec UDR trasy, která byste odesílali všechny odchozí přenosy z AppVM01 do brány firewall jako další segment a bránu firewall by najdete jako asymetrický provoz a vyřadit odchozí odpovědi proto existují aspoň tři nezávislá vrstev obrany mezi internet a AppVM01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-527">Finally, the UDR route would send any outbound traffic from AppVM01 to the firewall as the next hop, and the firewall would see this as asymmetric traffic and drop the outbound response Thus there are at least three independent layers of defense between the internet and AppVM01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-frontend-server-to-backend-server"></a><span data-ttu-id="58ce7-528">(Byl odepřen) Front-endu serveru back-end server</span><span class="sxs-lookup"><span data-stu-id="58ce7-528">(Denied) Frontend server to Backend Server</span></span>
1. <span data-ttu-id="58ce7-529">Předpokládejme, IIS01 došlo k ohrožení a běží škodlivý kód pokusu o zjištění podsíť back-end servery.</span><span class="sxs-lookup"><span data-stu-id="58ce7-529">Assume IIS01 was compromised and is running malicious code trying to scan the Backend subnet servers.</span></span>
2. <span data-ttu-id="58ce7-530">Trasy UDR podsítě front-endu byste odesílali všechny odchozí přenosy z IIS01 do brány firewall jako další segment.</span><span class="sxs-lookup"><span data-stu-id="58ce7-530">The Frontend subnet UDR route would send any outbound traffic from IIS01 to the firewall as the next hop.</span></span> <span data-ttu-id="58ce7-531">To však není něco, co může být změněna ohrožené virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="58ce7-531">This is not something that can be altered by the compromised VM.</span></span>
3. <span data-ttu-id="58ce7-532">Brána firewall by zpracovat provoz, pokud se požadavek na AppVM01 nebo na server DNS pro vyhledávání DNS, které provoz může být potenciálně povolený bránou firewall (z důvodu FW pravidla 7 a 9).</span><span class="sxs-lookup"><span data-stu-id="58ce7-532">The firewall would process the traffic, if the request was to AppVM01, or to the DNS server for DNS lookups that traffic could potentially be allowed by the firewall (due to FW Rules 7 and 9).</span></span> <span data-ttu-id="58ce7-533">Všechny ostatní přenosy by se zablokovaly podle FW pravidlo 11 (odmítnout vše).</span><span class="sxs-lookup"><span data-stu-id="58ce7-533">All other traffic would be blocked by FW Rule 11 (Deny All).</span></span>
4. <span data-ttu-id="58ce7-534">Pokud rozšířené detekce hrozeb byl povolen v bráně firewall (který není zahrnuté v tomto dokumentu, najdete v dokumentaci dodavatele pro vaše konkrétní síťové zařízení advanced threat možnosti), i provoz, který bude mít možnost pravidly základní předávání popisovaný v tomto dokumentu může zabránit, pokud provoz obsažené známé podpisům a vzorů, které příznak pravidlo rozšířené hrozba.</span><span class="sxs-lookup"><span data-stu-id="58ce7-534">If advanced threat detection was enabled on the firewall (which is not covered in this document, see the vendor documentation for your specific network appliance advanced threat capabilities), even traffic that would be allowed by the basic forwarding rules discussed in this document could be prevented if the traffic contained known signatures or patterns that flag an advanced threat rule.</span></span>

#### <a name="denied-internet-dns-lookup-on-dns-server"></a><span data-ttu-id="58ce7-535">(Byl odepřen) Vyhledávání DNS pro Internet na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="58ce7-535">(Denied) Internet DNS lookup on DNS server</span></span>
1. <span data-ttu-id="58ce7-536">Internet uživatel se pokusí vyhledat interní DNS záznam na DNS01 BackEnd001.CloudApp.Net pomocí služby</span><span class="sxs-lookup"><span data-stu-id="58ce7-536">Internet user tries to lookup an internal DNS record on DNS01 through BackEnd001.CloudApp.Net service</span></span> 
2. <span data-ttu-id="58ce7-537">Vzhledem k tomu, že jsou pro přenosy DNS otevřené žádné koncové body, se nebude předávat cloudové služby a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="58ce7-537">Since there are no endpoints open for DNS traffic, this would not pass through the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="58ce7-538">Pokud z nějakého důvodu byly otevřené koncových bodů, by tento provoz blokovat pravidla NSG (bloku Internet) na podsíť Frontend</span><span class="sxs-lookup"><span data-stu-id="58ce7-538">If the endpoints were open for some reason, the NSG rule (Block Internet) on the Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="58ce7-539">Nakonec směrování back-endu podsíť UDR byste odesílali všechny odchozí přenosy z DNS01 do brány firewall jako další segment a bránu firewall by najdete jako asymetrický provoz a vyřadit odchozí odpovědi proto existují aspoň tři nezávislá vrstev obrany mezi internet a DNS01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.</span><span class="sxs-lookup"><span data-stu-id="58ce7-539">Finally, the Backend subnet UDR route would send any outbound traffic from DNS01 to the firewall as the next hop, and the firewall would see this as asymmetric traffic and drop the outbound response Thus there are at least three independent layers of defense between the internet and DNS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-to-sql-access-through-firewall"></a><span data-ttu-id="58ce7-540">(Byl odepřen) Internetu, aby SQL přístup přes bránu Firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-540">(Denied) Internet to SQL access through Firewall</span></span>
1. <span data-ttu-id="58ce7-541">Internet uživatel požádá o dat SQL z SecSvc001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="58ce7-541">Internet user requests SQL data from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="58ce7-542">Vzhledem k tomu, že nejsou otevřené pro SQL koncové body, se nebude předat cloudové služby a nebude kontaktovat bránu firewall</span><span class="sxs-lookup"><span data-stu-id="58ce7-542">Since there are no endpoints open for SQL, this would not pass the Cloud Service and wouldn’t reach the firewall</span></span>
3. <span data-ttu-id="58ce7-543">Pokud z nějakého důvodu byly otevřené koncové body SQL, brány firewall se začne zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="58ce7-543">If SQL endpoints were open for some reason, the firewall would begin rule processing:</span></span>
   1. <span data-ttu-id="58ce7-544">Není použít, přejděte k další pravidla FW pravidla 1 (FW Mgmt)</span><span class="sxs-lookup"><span data-stu-id="58ce7-544">FW Rule 1 (FW Mgmt) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="58ce7-545">Nemusíte použít, přejděte k další pravidla FW pravidla 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="58ce7-545">FW Rules 2 - 5 (RDP Rules) don’t apply, move to next rule</span></span>
   3. <span data-ttu-id="58ce7-546">Nemusíte použít, přejděte k další pravidla FW pravidla 6 a 7 (pravidla pro aplikace)</span><span class="sxs-lookup"><span data-stu-id="58ce7-546">FW Rule 6 & 7 (Application Rules) don’t apply, move to next rule</span></span>
   4. <span data-ttu-id="58ce7-547">FW pravidlo 8 (Internet k) není použít, přejděte k další pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-547">FW Rule 8 (To Internet) doesn’t apply, move to next rule</span></span>
   5. <span data-ttu-id="58ce7-548">Není použít, přejděte k další pravidla FW pravidlo 9 (DNS)</span><span class="sxs-lookup"><span data-stu-id="58ce7-548">FW Rule 9 (DNS) doesn’t apply, move to next rule</span></span>
   6. <span data-ttu-id="58ce7-549">FW pravidlo 10 (Intra-podsítě) není použít, přejděte k další pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-549">FW Rule 10 (Intra-Subnet) doesn’t apply, move to next rule</span></span>
   7. <span data-ttu-id="58ce7-550">Použít FW pravidlo 11 (odmítnout vše), provoz je zpracování blokované, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-550">FW Rule 11 (Deny All) does apply, traffic is blocked, stop rule processing</span></span>

## <a name="references"></a><span data-ttu-id="58ce7-551">Odkazy</span><span class="sxs-lookup"><span data-stu-id="58ce7-551">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="58ce7-552">Hlavní skript a konfiguraci sítě</span><span class="sxs-lookup"><span data-stu-id="58ce7-552">Main Script and Network Config</span></span>
<span data-ttu-id="58ce7-553">Uložte úplné skript v souboru skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="58ce7-553">Save the Full Script in a PowerShell script file.</span></span> <span data-ttu-id="58ce7-554">Uložte konfiguraci sítě do souboru s názvem "NetworkConf2.xml".</span><span class="sxs-lookup"><span data-stu-id="58ce7-554">Save the Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="58ce7-555">Podle potřeby změňte proměnné definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="58ce7-555">Modify the user defined variables as needed.</span></span> <span data-ttu-id="58ce7-556">Spusťte skript a potom postupujte podle pokynů instalace pravidlo brány Firewall výše.</span><span class="sxs-lookup"><span data-stu-id="58ce7-556">Run the script, then follow the Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="58ce7-557">Úplné skriptu</span><span class="sxs-lookup"><span data-stu-id="58ce7-557">Full Script</span></span>
<span data-ttu-id="58ce7-558">Tento skript bude na základě uživatelsky definované proměnných:</span><span class="sxs-lookup"><span data-stu-id="58ce7-558">This script will, based on the user defined variables:</span></span>

1. <span data-ttu-id="58ce7-559">Připojení k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="58ce7-559">Connect to an Azure subscription</span></span>
2. <span data-ttu-id="58ce7-560">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="58ce7-560">Create a new storage account</span></span>
3. <span data-ttu-id="58ce7-561">Vytvořit novou virtuální síť a tři podsítě, jak jsou definovány v souboru konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="58ce7-561">Create a new VNet and three subnets as defined in the Network Config file</span></span>
4. <span data-ttu-id="58ce7-562">Sestavení pět virtuálních počítačů brány firewall 1 a 4 windows server virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="58ce7-562">Build five virtual machines; 1 firewall and 4 windows server VMs</span></span>
5. <span data-ttu-id="58ce7-563">Konfigurace, včetně UDR:</span><span class="sxs-lookup"><span data-stu-id="58ce7-563">Configure UDR including:</span></span>
   1. <span data-ttu-id="58ce7-564">Vytváření dva nové směrovací tabulky</span><span class="sxs-lookup"><span data-stu-id="58ce7-564">Creating two new route tables</span></span>
   2. <span data-ttu-id="58ce7-565">Přidání tras do tabulky</span><span class="sxs-lookup"><span data-stu-id="58ce7-565">Add routes to the tables</span></span>
   3. <span data-ttu-id="58ce7-566">Vytvořit vazbu tabulky k příslušné podsítě</span><span class="sxs-lookup"><span data-stu-id="58ce7-566">Bind tables to appropriate subnets</span></span>
6. <span data-ttu-id="58ce7-567">Povolení předávání IP na hodnocení chyb zabezpečení</span><span class="sxs-lookup"><span data-stu-id="58ce7-567">Enable IP Forwarding on the NVA</span></span>
7. <span data-ttu-id="58ce7-568">Konfigurace, včetně NSG:</span><span class="sxs-lookup"><span data-stu-id="58ce7-568">Configure NSG including:</span></span>
   1. <span data-ttu-id="58ce7-569">Vytváření skupina NSG</span><span class="sxs-lookup"><span data-stu-id="58ce7-569">Creating a NSG</span></span>
   2. <span data-ttu-id="58ce7-570">Přidávání pravidla</span><span class="sxs-lookup"><span data-stu-id="58ce7-570">Adding a rule</span></span>
   3. <span data-ttu-id="58ce7-571">Vytvoření vazby skupinu NSG na příslušné podsítě</span><span class="sxs-lookup"><span data-stu-id="58ce7-571">Binding the NSG to the appropriate subnets</span></span>

<span data-ttu-id="58ce7-572">Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.</span><span class="sxs-lookup"><span data-stu-id="58ce7-572">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58ce7-573">Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="58ce7-573">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="58ce7-574">Pouze chybové zprávy červeně jsou příčinou problém.</span><span class="sxs-lookup"><span data-stu-id="58ce7-574">Only error messages in red are cause for concern.</span></span>
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a><span data-ttu-id="58ce7-575">Soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="58ce7-575">Network Config File</span></span>
<span data-ttu-id="58ce7-576">Uložte tento soubor xml s aktualizované umístění a přidat odkaz na tohoto souboru do $NetworkConfigFile proměnné ve skriptu výše.</span><span class="sxs-lookup"><span data-stu-id="58ce7-576">Save this xml file with updated location and add the link to this file to the $NetworkConfigFile variable in the script above.</span></span>

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a><span data-ttu-id="58ce7-577">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="58ce7-577">Sample Application Scripts</span></span>
<span data-ttu-id="58ce7-578">Pokud chcete nainstalovat ukázkovou aplikaci pro toto a další příklady hraniční sítě, jednu bylo zadáno na následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="58ce7-578">If you wish to install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
<span data-ttu-id="58ce7-579">[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "DMZ obousměrně s hodnocení chyb zabezpečení, NSG a UDR"</span><span class="sxs-lookup"><span data-stu-id="58ce7-579">[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bi-directional DMZ with NVA, NSG, and UDR"</span></span>
<span data-ttu-id="58ce7-580">[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logickém zobrazení pravidla brány Firewall"</span><span class="sxs-lookup"><span data-stu-id="58ce7-580">[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logical View of the Firewall Rules"</span></span>
<span data-ttu-id="58ce7-581">[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Vytvoření objektu front-endové síti"</span><span class="sxs-lookup"><span data-stu-id="58ce7-581">[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Create a FrontEnd Network Object"</span></span>
<span data-ttu-id="58ce7-582">[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Vytvořit objekt serveru DNS"</span><span class="sxs-lookup"><span data-stu-id="58ce7-582">[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Create a DNS Server Object"</span></span>
<span data-ttu-id="58ce7-583">[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopii výchozí pravidlo protokolu RDP"</span><span class="sxs-lookup"><span data-stu-id="58ce7-583">[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Copy of Default RDP Rule"</span></span>
<span data-ttu-id="58ce7-584">[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "Pravidlo AppVM01"</span><span class="sxs-lookup"><span data-stu-id="58ce7-584">[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 Rule"</span></span>
<span data-ttu-id="58ce7-585">[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Ikona aplikace přesměrování"</span><span class="sxs-lookup"><span data-stu-id="58ce7-585">[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Application Redirect Icon"</span></span>
<span data-ttu-id="58ce7-586">[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ikona cílové NAT"</span><span class="sxs-lookup"><span data-stu-id="58ce7-586">[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Destination NAT Icon"</span></span>
<span data-ttu-id="58ce7-587">[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Ikona průchodu"</span><span class="sxs-lookup"><span data-stu-id="58ce7-587">[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pass Icon"</span></span>
<span data-ttu-id="58ce7-588">[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Pravidlo brány firewall správy"</span><span class="sxs-lookup"><span data-stu-id="58ce7-588">[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Firewall Management Rule"</span></span>
<span data-ttu-id="58ce7-589">[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Pravidlo brány firewall protokolu RDP"</span><span class="sxs-lookup"><span data-stu-id="58ce7-589">[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Firewall RDP Rule"</span></span>
<span data-ttu-id="58ce7-590">[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Pravidla brány firewall na webu"</span><span class="sxs-lookup"><span data-stu-id="58ce7-590">[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Firewall Web Rule"</span></span>
<span data-ttu-id="58ce7-591">[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Pravidlo brány firewall AppVM01"</span><span class="sxs-lookup"><span data-stu-id="58ce7-591">[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Firewall AppVM01 Rule"</span></span>
<span data-ttu-id="58ce7-592">[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Odchozí pravidlo brány firewall"</span><span class="sxs-lookup"><span data-stu-id="58ce7-592">[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Firewall Outbound Rule"</span></span>
<span data-ttu-id="58ce7-593">[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Pravidlo brány firewall DNS"</span><span class="sxs-lookup"><span data-stu-id="58ce7-593">[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Firewall DNS Rule"</span></span>
<span data-ttu-id="58ce7-594">[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Pravidlo brány firewall Intra-VNet"</span><span class="sxs-lookup"><span data-stu-id="58ce7-594">[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Firewall Intra-VNet Rule"</span></span>
<span data-ttu-id="58ce7-595">[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Pravidlo brány firewall Odepřít"</span><span class="sxs-lookup"><span data-stu-id="58ce7-595">[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Firewall Deny Rule"</span></span>
<span data-ttu-id="58ce7-596">[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktivace pravidla brány firewall"</span><span class="sxs-lookup"><span data-stu-id="58ce7-596">[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Firewall Rule Activation"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
