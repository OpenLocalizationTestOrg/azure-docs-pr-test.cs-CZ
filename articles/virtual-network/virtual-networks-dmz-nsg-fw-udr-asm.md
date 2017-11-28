---
title: "aaaDMZ příklad – sestavení DMZ tooProtect sítím pomocí brány Firewall, UDR a NSG | Microsoft Docs"
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
ms.openlocfilehash: cc121f9cd5fe3c3e9ac2c70fbb7d982a80bb345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-3--build-a-dmz-tooprotect-networks-with-a-firewall-udr-and-nsg"></a><span data-ttu-id="daebc-103">Příklad 3 – vytvoření DMZ tooProtect sítím pomocí brány Firewall, UDR a NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-103">Example 3 – Build a DMZ tooProtect Networks with a Firewall, UDR, and NSG</span></span>
<span data-ttu-id="daebc-104">[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="daebc-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="daebc-105">Tento příklad vytvoří DMZ s bránou firewall, čtyři servery windows, uživatele definované směrování, předávání IP adres a skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-105">This example will create a DMZ with a firewall, four windows servers, User Defined Routing, IP Forwarding, and Network Security Groups.</span></span> <span data-ttu-id="daebc-106">Je také provede všechny relevantní příkazy tooprovide hello podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="daebc-106">It will also walk through each of hello relevant commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="daebc-107">K dispozici je také provozu scénář části tooprovide na podrobné podrobné jak provoz pokračuje prostřednictvím hello vrstev obrany ve hello hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-107">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="daebc-108">Nakonec v části odkazy hello je kompletní kód hello a instrukce toobuild tento tootest prostředí a experimentů pomocí různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="daebc-108">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="daebc-109">![DMZ obousměrně s hodnocení chyb zabezpečení, NSG a UDR][1]</span><span class="sxs-lookup"><span data-stu-id="daebc-109">![Bi-directional DMZ with NVA, NSG, and UDR][1]</span></span>

## <a name="environment-setup"></a><span data-ttu-id="daebc-110">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="daebc-110">Environment Setup</span></span>
<span data-ttu-id="daebc-111">V tomto příkladu je předplatné, které obsahuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="daebc-111">In this example there is a subscription that contains hello following:</span></span>

* <span data-ttu-id="daebc-112">Tři cloudové služby: "SecSvc001", "FrontEnd001" a "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="daebc-112">Three cloud services: “SecSvc001”, “FrontEnd001”, and “BackEnd001”</span></span>
* <span data-ttu-id="daebc-113">Virtuální síť "CorpNetwork", s tři podsítě: "SecNet", "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="daebc-113">A Virtual Network “CorpNetwork”, with three subnets: “SecNet”, “FrontEnd”, and “BackEnd”</span></span>
* <span data-ttu-id="daebc-114">Virtuální zařízení sítě, v tomto příkladu bránu firewall, připojení toohello SecNet podsítě</span><span class="sxs-lookup"><span data-stu-id="daebc-114">A network virtual appliance, in this example a firewall, connected toohello SecNet subnet</span></span>
* <span data-ttu-id="daebc-115">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="daebc-115">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="daebc-116">Dva windows servery, které představují aplikace zpět ukončení servery ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="daebc-116">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="daebc-117">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="daebc-117">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="daebc-118">V části hello odkazy níže je skript prostředí PowerShell, který bude vytvořit většinu hello prostředí popsané výše.</span><span class="sxs-lookup"><span data-stu-id="daebc-118">In hello references section below there is a PowerShell script that will build most of hello environment described above.</span></span> <span data-ttu-id="daebc-119">Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádějí hello ukázkový skript, nejsou popsané podrobně v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="daebc-119">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span>

<span data-ttu-id="daebc-120">toobuild hello prostředí:</span><span class="sxs-lookup"><span data-stu-id="daebc-120">toobuild hello environment:</span></span>

1. <span data-ttu-id="daebc-121">Uložit hello sítě konfigurační soubor xml v oddíle odkazy hello (aktualizovat název, umístění a IP adresy toomatch hello zadané scénář)</span><span class="sxs-lookup"><span data-stu-id="daebc-121">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="daebc-122">Aktualizace hello uživatelské proměnné ve hello skriptu toomatch hello prostředí hello skriptu je toobe spouštění (odběry, názvy služeb atd.)</span><span class="sxs-lookup"><span data-stu-id="daebc-122">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="daebc-123">Spuštění skriptu hello v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="daebc-123">Execute hello script in PowerShell</span></span>

<span data-ttu-id="daebc-124">**Poznámka:**: oblast hello označený ve hello skript prostředí PowerShell musí odpovídat hello oblast označený ve hello sítě konfigurační soubor xml.</span><span class="sxs-lookup"><span data-stu-id="daebc-124">**Note**: hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>

<span data-ttu-id="daebc-125">Po úspěšném spuštění skriptu hello mohou být přijata hello kroků po skriptu:</span><span class="sxs-lookup"><span data-stu-id="daebc-125">Once hello script runs successfully hello following post-script steps may be taken:</span></span>

1. <span data-ttu-id="daebc-126">Nastavit hello pravidla brány firewall, najdete v části hello níže, s názvem: popis pravidla brány Firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-126">Set up hello firewall rules, this is covered in hello section below titled: Firewall Rule Description.</span></span>
2. <span data-ttu-id="daebc-127">Volitelně můžete v části odkazy hello jsou dva skripty tooset hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-127">Optionally in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="daebc-128">Po úspěšném spuštění skriptu hello hello pravidla brány firewall třeba toobe dokončit, najdete v části hello s názvem: pravidla brány Firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-128">Once hello script runs successfully hello firewall rules will need toobe completed, this is covered in hello section titled: Firewall Rules.</span></span>

## <a name="user-defined-routing-udr"></a><span data-ttu-id="daebc-129">Uživatelem definované směrování (UDR)</span><span class="sxs-lookup"><span data-stu-id="daebc-129">User Defined Routing (UDR)</span></span>
<span data-ttu-id="daebc-130">Ve výchozím nastavení jsou hello následující systémové trasy definované jako:</span><span class="sxs-lookup"><span data-stu-id="daebc-130">By default, hello following system routes are defined as:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

<span data-ttu-id="daebc-131">Hello VNETLocal je vždy hello definované adresu prefix(es) Dobrý den virtuální sítě pro tuto konkrétní síť (ie se změní ze tooVNet virtuální sítě v závislosti na tom, jak je definována každý konkrétní virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="daebc-131">hello VNETLocal is always hello defined address prefix(es) of hello VNet for that specific network (ie it will change from VNet tooVNet depending on how each specific VNet is defined).</span></span> <span data-ttu-id="daebc-132">systémové trasy zbývající Hello statické a výchozí jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="daebc-132">hello remaining system routes are static and default as above.</span></span>

<span data-ttu-id="daebc-133">Jako prioritu trasy se zpracovávají prostřednictvím metody hello nejdelší shody předpony (LPM), proto hello nejvíce trasy v tabulce hello by použijí tooa zadané cílové adresy.</span><span class="sxs-lookup"><span data-stu-id="daebc-133">As for priority, routes are processed via hello Longest Prefix Match (LPM) method, thus hello most specific route in hello table would apply tooa given destination address.</span></span>

<span data-ttu-id="daebc-134">Přenos (například toohello DNS01 server, 10.0.2.4) určený pro hello místní sítě (10.0.0.0/16) by se tedy směrován přes hello virtuální síť tooits cílové kvůli toohello 10.0.0.0/16 trasy.</span><span class="sxs-lookup"><span data-stu-id="daebc-134">Therefore, traffic (for example toohello DNS01 server, 10.0.2.4) intended for hello local network (10.0.0.0/16) would be routed across hello VNet tooits destination due toohello 10.0.0.0/16 route.</span></span> <span data-ttu-id="daebc-135">Jinými slovy pro 10.0.2.4, trasy 10.0.0.0/16 hello je hello nejvíce trasy, i když hello 10.0.0.0/8 a 0.0.0.0/0 také může použít, ale vzhledem k tomu, že jsou méně specifické neovlivňuje tento provoz.</span><span class="sxs-lookup"><span data-stu-id="daebc-135">In other words, for 10.0.2.4, hello 10.0.0.0/16 route is hello most specific route, even though hello 10.0.0.0/8 and 0.0.0.0/0 also could apply, but since they are less specific they don’t affect this traffic.</span></span> <span data-ttu-id="daebc-136">Proto hello provoz too10.0.2.4 by měla mít další segment z hello virtuální místní síť a jednoduše trasy toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="daebc-136">Thus hello traffic too10.0.2.4 would have a next hop of hello local VNet and simply route toohello destination.</span></span>

<span data-ttu-id="daebc-137">Pokud se provoz určený pro 10.1.1.1 například, nebude platit hello 10.0.0.0/16 trasy, ale hello 10.0.0.0/8 by hello nejvíce a by tento provoz hello vyřadit ("černé dírkového"), protože hello další směrování je Null.</span><span class="sxs-lookup"><span data-stu-id="daebc-137">If traffic was intended for 10.1.1.1 for example, hello 10.0.0.0/16 route wouldn’t apply, but hello 10.0.0.0/8 would be hello most specific, and this hello traffic would be dropped (“black holed”) since hello next hop is Null.</span></span> 

<span data-ttu-id="daebc-138">Pokud cílový hello nepoužil tooany hello Null předpony nebo hello VNETLocal předpony, postupujte podle jeho by hello nejméně specifická směrování, 0.0.0.0/0 a směrovat odhlásit toohello Internetu jako další segment hello a proto si Azure a internet okraj.</span><span class="sxs-lookup"><span data-stu-id="daebc-138">If hello destination didn’t apply tooany of hello Null prefixes or hello VNETLocal prefixes, then it would follow hello least specific route, 0.0.0.0/0 and be routed out toohello Internet as hello next hop and thus out Azure’s internet edge.</span></span>

<span data-ttu-id="daebc-139">Pokud ve směrovací tabulce hello existují dva identické předpony, hello následuje hello pořadí podle priority na základě atributu "zdroj" hello trasy:</span><span class="sxs-lookup"><span data-stu-id="daebc-139">If there are two identical prefixes in hello route table, hello following is hello order of preference based on hello routes “source” attribute:</span></span>

1. <span data-ttu-id="daebc-140">"VirtualAppliance" = tabulku ručně přidaná toohello uživatelem definovaná trasa</span><span class="sxs-lookup"><span data-stu-id="daebc-140">"VirtualAppliance" = A User Defined Route manually added toohello table</span></span>
2. <span data-ttu-id="daebc-141">"Brána VPN" = dynamické směrování protokolu BGP (při použití s hybridní sítě), přidal protokol dynamické sítě, tyto trasy v průběhu času mění jako dynamický protokol hello automaticky odráží změny v peered sítě</span><span class="sxs-lookup"><span data-stu-id="daebc-141">“VPNGateway” = A Dynamic Route (BGP when used with hybrid networks), added by a dynamic network protocol, these routes may change over time as hello dynamic protocol automatically reflects changes in peered network</span></span>
3. <span data-ttu-id="daebc-142">"Výchozí" = hello systémové trasy, jak je znázorněno v výše uvedené tabulce směrování hello hello místní virtuální síť a hello statické záznamy.</span><span class="sxs-lookup"><span data-stu-id="daebc-142">“Default” = hello System Routes, hello local VNet and hello static entries as shown in hello route table above.</span></span>

> [!NOTE]
> <span data-ttu-id="daebc-143">Teď můžete použít uživatele definované směrování (UDR) s ExpressRoute a VPN Gateway tooforce odchozí a příchozí mezi různými místy provoz toobe tooa směrované sítě virtuální zařízení (hodnocení chyb zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="daebc-143">You can now use User Defined Routing (UDR) with ExpressRoute and VPN Gateways tooforce outbound and inbound cross-premises traffic toobe routed tooa network virtual appliance (NVA).</span></span>
> 
> 

#### <a name="creating-hello-local-routes"></a><span data-ttu-id="daebc-144">Vytváření hello místní trasy</span><span class="sxs-lookup"><span data-stu-id="daebc-144">Creating hello local routes</span></span>
<span data-ttu-id="daebc-145">V tomto příkladu jsou potřeba dvou směrovacích tabulek, jeden pro každé podsítě front-endové a back-end hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-145">In this example, two routing tables are needed, one each for hello Frontend and Backend subnets.</span></span> <span data-ttu-id="daebc-146">Každá tabulka je načtena s statické trasy, které jsou vhodné pro danou podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-146">Each table is loaded with static routes appropriate for hello given subnet.</span></span> <span data-ttu-id="daebc-147">Za účelem hello tohoto příkladu každá tabulka měla tři trasy:</span><span class="sxs-lookup"><span data-stu-id="daebc-147">For hello purpose of this example, each table has three routes:</span></span>

1. <span data-ttu-id="daebc-148">Provozu místních podsítí s žádné další směrování definované tooallow místní podsíti provozu toobypass hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-148">Local subnet traffic with no Next Hop defined tooallow local subnet traffic toobypass hello firewall</span></span>
2. <span data-ttu-id="daebc-149">Virtuální síťový provoz s další směrování definovat jako bránu firewall, přepíše se hello výchozí pravidlo, které přímo umožňuje místní tooroute provoz virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="daebc-149">Virtual Network traffic with a Next Hop defined as firewall, this overrides hello default rule that allows local VNet traffic tooroute directly</span></span>
3. <span data-ttu-id="daebc-150">Veškerý zbývající provoz (0/0) s další směrování definován jako hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-150">All remaining traffic (0/0) with a Next Hop defined as hello firewall</span></span>

<span data-ttu-id="daebc-151">Jakmile jsou vytvořeny hello směrovacích tabulek nejsou vázané tootheir podsítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-151">Once hello routing tables are created they are bound tootheir subnets.</span></span> <span data-ttu-id="daebc-152">Pro hello front-endu podsíť směrovací tabulky po vytvoření a vázaný toohello podsíť by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="daebc-152">For hello Frontend subnet routing table, once created and bound toohello subnet should look like this:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


<span data-ttu-id="daebc-153">V tomto příkladu hello následující příkazy, jsou použité toobuild hello směrovací tabulka, přidejte trasu definovanou uživatelem a pak vytvořte vazbu hello trasy tabulky tooa podsítě (Poznámka; všechny položky pod počínaje znak dolaru (např: $BESubnet) jsou proměnné definované uživatelem z hello skript v Hello odkaz části tohoto dokumentu):</span><span class="sxs-lookup"><span data-stu-id="daebc-153">For this example, hello following commands are used toobuild hello route table, add a user defined route, and then bind hello route table tooa subnet (Note; any items below beginning with a dollar sign (e.g.: $BESubnet) are user defined variables from hello script in hello reference section of this document):</span></span>

1. <span data-ttu-id="daebc-154">První hello základní směrovací tabulky musí být vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="daebc-154">First hello base routing table must be created.</span></span> <span data-ttu-id="daebc-155">Tento fragment kódu ukazuje vytvoření hello hello tabulky pro podsíť hello back-end.</span><span class="sxs-lookup"><span data-stu-id="daebc-155">This snippet shows hello creation of hello table for hello Backend subnet.</span></span> <span data-ttu-id="daebc-156">Ve skriptu hello je vytvořen pro podsíť Frontend hello také odpovídající tabulku.</span><span class="sxs-lookup"><span data-stu-id="daebc-156">In hello script, a corresponding table is also created for hello Frontend subnet.</span></span>
   
     <span data-ttu-id="daebc-157">Nové AzureRouteTable-název $BERouteTableName.</span><span class="sxs-lookup"><span data-stu-id="daebc-157">New-AzureRouteTable -Name $BERouteTableName \`</span></span>
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. <span data-ttu-id="daebc-158">Po vytvoření hello směrovací tabulka se dá přidat trasy definované uživatelem konkrétní.</span><span class="sxs-lookup"><span data-stu-id="daebc-158">Once hello route table is created, specific user defined routes can be added.</span></span> <span data-ttu-id="daebc-159">V tomto uvádíme veškerý provoz (0.0.0.0/0) budou směrovány přes virtuální zařízení hello (proměnné, $VMIP [0], je použít toopass v přiřazené při vytvoření virtuálního zařízení hello dříve ve skriptu hello hello IP adresy).</span><span class="sxs-lookup"><span data-stu-id="daebc-159">In this snipped, all traffic (0.0.0.0/0) will be routed through hello virtual appliance (a variable, $VMIP[0], is used toopass in hello IP address assigned when hello virtual appliance was created earlier in hello script).</span></span> <span data-ttu-id="daebc-160">Ve skriptu hello se také vytvoří odpovídající pravidlo v tabulce front-endu hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-160">In hello script, a corresponding rule is also created in hello Frontend table.</span></span>
   
     <span data-ttu-id="daebc-161">Get-AzureRouteTable $BERouteTableName | \`</span><span class="sxs-lookup"><span data-stu-id="daebc-161">Get-AzureRouteTable $BERouteTableName | \`</span></span>
   
         Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. <span data-ttu-id="daebc-162">přepíše Hello výše položka trasy hello výchozí "0.0.0.0/0" trasu, ale stále existuje, který by umožnil hello výchozí 10.0.0.0/16 pravidlo provozu v rámci virtuální sítě tooroute hello přímo toohello cíl a toohello virtuální síťové zařízení.</span><span class="sxs-lookup"><span data-stu-id="daebc-162">hello above route entry will override hello default "0.0.0.0/0" route, but hello default 10.0.0.0/16 rule still existing which would allow traffic within hello VNet tooroute directly toohello destination and not toohello Network Virtual Appliance.</span></span> <span data-ttu-id="daebc-163">toocorrect, je nutné přidat toto chování hello postupujte podle pravidlo.</span><span class="sxs-lookup"><span data-stu-id="daebc-163">toocorrect this behavior hello follow rule must be added.</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. <span data-ttu-id="daebc-164">V tomto okamžiku je volba toobe, provedeny.</span><span class="sxs-lookup"><span data-stu-id="daebc-164">At this point there is a choice toobe made.</span></span> <span data-ttu-id="daebc-165">S hello výše dvě trasy bude směrovat veškerý provoz toohello brány firewall pro vyhodnocení, i provoz v rámci jedné podsíti.</span><span class="sxs-lookup"><span data-stu-id="daebc-165">With hello above two routes all traffic will route toohello firewall for assessment, even traffic within a single subnet.</span></span> <span data-ttu-id="daebc-166">To může být požaduje, ale tooallow provozu v rámci podsítě tooroute místně bez zásahu hello brány firewall, lze přidat třetí, velmi konkrétní pravidlo.</span><span class="sxs-lookup"><span data-stu-id="daebc-166">This may be desired, however tooallow traffic within a subnet tooroute locally without involvement from hello firewall a third, very specific rule can be added.</span></span> <span data-ttu-id="daebc-167">Tato trasa stavy, které libovolná adresa destine pro místní podsíti hello můžete právě směrovat existuje přímo (NextHopType = VNETLocal).</span><span class="sxs-lookup"><span data-stu-id="daebc-167">This route states that any address destine for hello local subnet can just route there directly (NextHopType = VNETLocal).</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. <span data-ttu-id="daebc-168">S hello směrovací tabulky vytvořeny a naplněny s trasy definované uživatelem, nakonec hello tabulky musí být nyní vázané tooa podsítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-168">Finally, with hello routing table created and populated with a user defined routes, hello table must now be bound tooa subnet.</span></span> <span data-ttu-id="daebc-169">Ve skriptu hello hello front-endu směrovací tabulka je také vázané toohello podsítě front-endu.</span><span class="sxs-lookup"><span data-stu-id="daebc-169">In hello script, hello front end route table is also bound toohello Frontend subnet.</span></span> <span data-ttu-id="daebc-170">Zde je hello vazby skript pro podsíť hello back-end.</span><span class="sxs-lookup"><span data-stu-id="daebc-170">Here is hello binding script for hello back end subnet.</span></span>
   
     <span data-ttu-id="daebc-171">Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName.</span><span class="sxs-lookup"><span data-stu-id="daebc-171">Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName \`</span></span>
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a><span data-ttu-id="daebc-172">Předávání IP</span><span class="sxs-lookup"><span data-stu-id="daebc-172">IP Forwarding</span></span>
<span data-ttu-id="daebc-173">Funkce doprovodné tooUDR je předávání IP.</span><span class="sxs-lookup"><span data-stu-id="daebc-173">A companion feature tooUDR, is IP Forwarding.</span></span> <span data-ttu-id="daebc-174">Toto je, že nastavení na virtuální zařízení, který umožní tooreceive provozu není konkrétně řešit toohello zařízení a pak předávat této cílové ultimate tooits provoz.</span><span class="sxs-lookup"><span data-stu-id="daebc-174">This is a setting on a Virtual Appliance that allows it tooreceive traffic not specifically addressed toohello appliance and then forward that traffic tooits ultimate destination.</span></span>

<span data-ttu-id="daebc-175">Jako příklad Pokud provoz z AppVM01 provede požadavek toohello DNS01 server, by UDR směrovat tato toohello brána firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-175">As an example, if traffic from AppVM01 makes a request toohello DNS01 server, UDR would route this toohello firewall.</span></span> <span data-ttu-id="daebc-176">S povoleno předávání IP bude akceptovat hello zařízení (10.0.0.4) hello provoz pro cíl DNS01 hello (10.0.2.4) a tooits ultimate cílové (10.0.2.4), předá.</span><span class="sxs-lookup"><span data-stu-id="daebc-176">With IP Forwarding enabled, hello traffic for hello DNS01 destination (10.0.2.4) will be accepted by hello appliance (10.0.0.4) and then forwarded tooits ultimate destination (10.0.2.4).</span></span> <span data-ttu-id="daebc-177">Bez předávání IP adres na hello brány Firewall povolená by se provoz přijímat hello zařízení Přestože hello směrovací tabulka má hello brány firewall jako další segment hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-177">Without IP Forwarding enabled on hello Firewall, traffic would not be accepted by hello appliance even though hello route table has hello firewall as hello next hop.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="daebc-178">Je důležité tooremember tooenable předávání IP adres ve spojení s směrování definovaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="daebc-178">It’s critical tooremember tooenable IP Forwarding in conjunction with User Defined Routing.</span></span>
> 
> 

<span data-ttu-id="daebc-179">Nastavení předávání IP adres je jeden příkaz a lze provést v okamžiku vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="daebc-179">Setting up IP Forwarding is a single command and can be done at VM creation time.</span></span> <span data-ttu-id="daebc-180">Pro hello toku tento fragment kódu hello příklad je hello konci hello skriptu a seskupuje hello UDR příkazy:</span><span class="sxs-lookup"><span data-stu-id="daebc-180">For hello flow of this example hello code snippet is towards hello end of hello script and grouped with hello UDR commands:</span></span>

1. <span data-ttu-id="daebc-181">Volání hello instance virtuálního počítače, který je virtuálního zařízení, v takovém případě hello brány firewall a povolení předávání IP adres (Poznámka; libovolnou položku v red počínaje znak dolaru (např: $VMName[0]) je uživatelem definované proměnné ze skriptu hello v hello odkaz části tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="daebc-181">Call hello VM instance that is your virtual appliance, hello firewall in this case, and enable IP Forwarding (Note; any item in red beginning with a dollar sign (e.g.: $VMName[0]) is a user defined variable from hello script in hello reference section of this document.</span></span> <span data-ttu-id="daebc-182">Hello nula v hranatých závorkách [0], představuje hello první virtuální počítač v poli hello virtuálních počítačů, pro hello příklad skriptu toowork bez úprav, brány firewall hello hello, musí být první virtuální počítač (VM 0)):</span><span class="sxs-lookup"><span data-stu-id="daebc-182">hello zero in brackets, [0], represents hello first VM in hello array of VMs, for hello example script toowork without modification, hello first VM (VM 0) must be hello firewall):</span></span>
   
     <span data-ttu-id="daebc-183">Get-AzureVM-název $VMName [0] - ServiceName $ServiceName [0] | \`</span><span class="sxs-lookup"><span data-stu-id="daebc-183">Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | \`</span></span>
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a><span data-ttu-id="daebc-184">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="daebc-184">Network Security Groups (NSG)</span></span>
<span data-ttu-id="daebc-185">V tomto příkladu je skupina NSG vytvořené a pak načten s jedním pravidlem.</span><span class="sxs-lookup"><span data-stu-id="daebc-185">In this example, a NSG group is built and then loaded with a single rule.</span></span> <span data-ttu-id="daebc-186">Tato skupina je pak vázaný jenom toohello front-endové a back-end podsítě (ne hello SecNet).</span><span class="sxs-lookup"><span data-stu-id="daebc-186">This group is then bound only toohello Frontend and Backend subnets (not hello SecNet).</span></span> <span data-ttu-id="daebc-187">Deklarativně se sestavuje hello následující pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-187">Declaratively hello following rule is being built:</span></span>

1. <span data-ttu-id="daebc-188">Jakýkoli přenos (všechny porty) z Internetu toohello hello celý virtuální síť (všechny podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="daebc-188">Any traffic (all ports) from hello Internet toohello entire VNet (all subnets) is Denied</span></span>

<span data-ttu-id="daebc-189">I když v tomto příkladu se používají skupiny Nsg, je hlavním účelem jako vrstva sekundární ochranu proti ruční chybné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="daebc-189">Although NSGs are used in this example, it’s main purpose is as a secondary layer of defense against manual misconfiguration.</span></span> <span data-ttu-id="daebc-190">Chceme tooblock všechny příchozí provoz z hello internet tooeither hello front-end nebo back-end podsítě, provoz by měl pouze procházet skrz hello SecNet podsíť toohello brány firewall (a pak v případě vhodné na toohello front-end nebo back-end podsítě).</span><span class="sxs-lookup"><span data-stu-id="daebc-190">We want tooblock all inbound traffic from hello internet tooeither hello Frontend or Backend subnets, traffic should only flow through hello SecNet subnet toohello firewall (and then if appropriate on toohello Frontend or Backend subnets).</span></span> <span data-ttu-id="daebc-191">Plus s pravidly UDR hello v místě, přenosy, který do hello front-end nebo back-end podsítí by přesměrováni na toohello brány firewall (Děkujeme tooUDR).</span><span class="sxs-lookup"><span data-stu-id="daebc-191">Plus, with hello UDR rules in place, any traffic that did make it into hello Frontend or Backend subnets would be directed out toohello firewall (thanks tooUDR).</span></span> <span data-ttu-id="daebc-192">brány firewall Hello by to zobrazit jako asymetrický toku a by vyřadit hello odchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="daebc-192">hello firewall would see this as an asymmetric flow and would drop hello outbound traffic.</span></span> <span data-ttu-id="daebc-193">Proto existují tři vrstvy zabezpečení chránící hello front-endu a back-end podsítě; 1) bez otevřete koncových bodů na hello FrontEnd001 a BackEnd001 cloudové služby, 2) skupiny Nsg odepření přenosy z Internetu, brána firewall 3) hello vyřazení asymetrické provoz hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-193">Thus there are three layers of security protecting hello Frontend and Backend subnets; 1) no open endpoints on hello FrontEnd001 and BackEnd001 cloud services, 2) NSGs denying traffic from hello Internet, 3) hello firewall dropping asymmetric traffic.</span></span>

<span data-ttu-id="daebc-194">Jeden bod zajímavé týkající se hello skupinu zabezpečení sítě v tomto příkladu je, že obsahuje pouze jedno pravidlo, viz následující obrázek, který je toodeny internetové přenosy toohello celý virtuální síti, která bude zahrnovat podsítě zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-194">One interesting point regarding hello Network Security Group in this example is that it contains only one rule, shown below, which is toodeny internet traffic toohello entire virtual network which would include hello Security subnet.</span></span> 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet `
        from hello Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

<span data-ttu-id="daebc-195">Ale protože hello NSG je pouze vazbu toohello front-endu a back-end podsítě, hello pravidlo není zpracován na provoz příchozí toohello zabezpečení podsítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-195">However, since hello NSG is only bound toohello Frontend and Backend subnets, hello rule isn’t processed on traffic inbound toohello Security subnet.</span></span> <span data-ttu-id="daebc-196">V důsledku toho Přestože pravidla NSG hello uvádí žádné internetové přenosy tooany adresy na hello virtuální síť, protože hello NSG se nikdy vázán podsítě zabezpečení toohello, bude přenos toohello zabezpečení podsítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-196">As a result, even though hello NSG rule says no Internet traffic tooany address on hello VNet, because hello NSG was never bound toohello Security subnet, traffic will flow toohello Security subnet.</span></span>

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a><span data-ttu-id="daebc-197">Pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-197">Firewall Rules</span></span>
<span data-ttu-id="daebc-198">V bráně firewall hello předávání pravidla potřebovat toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="daebc-198">On hello firewall, forwarding rules will need toobe created.</span></span> <span data-ttu-id="daebc-199">Vzhledem k tomu, že brána firewall hello je blokování nebo předávání všechny vstupní, výstupní a intra-VNet provoz, je potřeba řada pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-199">Since hello firewall is blocking or forwarding all inbound, outbound, and intra-VNet traffic many firewall rules are needed.</span></span> <span data-ttu-id="daebc-200">Navíc se veškerý příchozí provoz setkají hello služby zabezpečení veřejnou IP adresu (v jiné porty), toobe zpracuje bránou hello firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-200">Also, all inbound traffic will hit hello Security Service public IP address (on different ports), toobe processed by hello firewall.</span></span> <span data-ttu-id="daebc-201">Osvědčeným postupem je toodiagram hello logické toky před nastavením přepracování tooavoid pravidla brány firewall a podsítí hello později.</span><span class="sxs-lookup"><span data-stu-id="daebc-201">A best practice is toodiagram hello logical flows before setting up hello subnets and firewall rules tooavoid rework later.</span></span> <span data-ttu-id="daebc-202">Hello následující obrázek je logickém zobrazení hello pravidla brány firewall v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="daebc-202">hello following figure is a logical view of hello firewall rules for this example:</span></span>

<span data-ttu-id="daebc-203">![Logickém zobrazení hello pravidla brány Firewall][2]</span><span class="sxs-lookup"><span data-stu-id="daebc-203">![Logical View of hello Firewall Rules][2]</span></span>

> [!NOTE]
> <span data-ttu-id="daebc-204">Podle hello virtuální síťové zařízení používá, se liší hello porty pro správu.</span><span class="sxs-lookup"><span data-stu-id="daebc-204">Based on hello Network Virtual Appliance used, hello management ports will vary.</span></span> <span data-ttu-id="daebc-205">V tomto příkladu, které se odkazuje Barracuda NextGen Firewall, který používá porty 22, 801 a 807.</span><span class="sxs-lookup"><span data-stu-id="daebc-205">In this example a Barracuda NextGen Firewall is referenced which uses ports 22, 801, and 807.</span></span> <span data-ttu-id="daebc-206">Přečtěte si hello zařízení dodavatele dokumentaci toofind hello přesný porty používané pro správu zařízení hello používá.</span><span class="sxs-lookup"><span data-stu-id="daebc-206">Please consult hello appliance vendor documentation toofind hello exact ports used for management of hello device being used.</span></span>
> 
> 

### <a name="logical-rule-description"></a><span data-ttu-id="daebc-207">Popis logické pravidla</span><span class="sxs-lookup"><span data-stu-id="daebc-207">Logical Rule Description</span></span>
<span data-ttu-id="daebc-208">V hello logického diagramu výše se nezobrazí podsítě zabezpečení hello vzhledem k tomu, že brána firewall hello je hello pouze prostředků na této podsíti a tohoto diagramu se zobrazuje hello pravidla brány firewall a jak se logicky povolit nebo odepřít tok přenosů dat a není hello skutečné směrované cesty.</span><span class="sxs-lookup"><span data-stu-id="daebc-208">In hello logical diagram above, hello security subnet is not shown since hello firewall is hello only resource on that subnet, and this diagram is showing hello firewall rules and how they logically allow or deny traffic flows and not hello actual routed path.</span></span> <span data-ttu-id="daebc-209">Navíc hello externí porty vybraný pro hello provoz protokolu RDP jsou porty vyšší pohyboval (8014 – 8026) a byly vybrané toosomewhat zarovnané s hello poslední dva oktety hello místní IP adresu pro snazší čitelnost (například místní server adresu 10.0.1.4 přidružen port externí 8014), ale může použít jakékoli vyšší-konfliktní porty.</span><span class="sxs-lookup"><span data-stu-id="daebc-209">Also, hello external ports selected for hello RDP traffic are higher ranged ports (8014 – 8026) and were selected toosomewhat align with hello last two octets of hello local IP address for easier readability (e.g. local server address 10.0.1.4 is associated with external port 8014), however any higher non-conflicting ports could be used.</span></span>

<span data-ttu-id="daebc-210">V tomto příkladu budeme potřebovat 7 typy pravidel, tyto typy pravidel jsou popsány takto:</span><span class="sxs-lookup"><span data-stu-id="daebc-210">For this example, we need 7 types of rules, these rule types are described as follows:</span></span>

* <span data-ttu-id="daebc-211">Externí pravidla (pro příchozí provoz):</span><span class="sxs-lookup"><span data-stu-id="daebc-211">External Rules (for inbound traffic):</span></span>
  1. <span data-ttu-id="daebc-212">Pravidlo brány firewall správy: Toto pravidlo přesměrování aplikace umožňuje provoz toopass toohello správu porty hello síťové virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="daebc-212">Firewall Management Rule: This App Redirect rule allows traffic toopass toohello management ports of hello network virtual appliance.</span></span>
  2. <span data-ttu-id="daebc-213">Pravidla protokolu RDP (pro každý server systému windows): tyto čtyři pravidla (jeden pro každý server) vám umožní správu hello jednotlivé servery prostřednictvím protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="daebc-213">RDP Rules (for each windows server): These four rules (one for each server) will allow management of hello individual servers via RDP.</span></span> <span data-ttu-id="daebc-214">To může také seskupeny do jedno pravidlo v závislosti na možnosti hello hello sítě používá virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="daebc-214">This could also be bundled into one rule depending on hello capabilities of hello Network Virtual Appliance being used.</span></span>
  3. <span data-ttu-id="daebc-215">Pravidla pro provoz aplikace: Existují dvě pravidla pro provoz aplikace, hello nejprve pro hello front-end webový provoz a hello druhý pro přenosy back-end hello (např webový server toodata vrstva).</span><span class="sxs-lookup"><span data-stu-id="daebc-215">Application Traffic Rules: There are two Application Traffic Rules, hello first for hello front end web traffic, and hello second for hello back end traffic (eg web server toodata tier).</span></span> <span data-ttu-id="daebc-216">Konfigurace Hello pravidla bude záviset na hello síťovou architekturu (kde jsou umístěné vaše servery) a tok přenosů dat (které směr hello přenosy dat a používaných portů).</span><span class="sxs-lookup"><span data-stu-id="daebc-216">hello configuration of these rules will depend on hello network architecture (where your servers are placed) and traffic flows (which direction hello traffic flows, and which ports are used).</span></span>
     * <span data-ttu-id="daebc-217">první pravidlo Hello vám umožní hello skutečné aplikace provoz tooreach hello aplikačního serveru.</span><span class="sxs-lookup"><span data-stu-id="daebc-217">hello first rule will allow hello actual application traffic tooreach hello application server.</span></span> <span data-ttu-id="daebc-218">Při hello ostatní pravidla povolit pro zabezpečení, správy, atd., jsou aplikace pravidla povolit externích uživatelů nebo služeb tooaccess aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-218">While hello other rules allow for security, management, etc., Application Rules are what allow external users or services tooaccess hello application(s).</span></span> <span data-ttu-id="daebc-219">V tomto příkladu je jednom webovém serveru na portu 80, proto jediné aplikace pravidlo firewallu přesměruje příchozí provoz toohello externí IP, toohello webové servery interní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="daebc-219">For this example, there is a single web server on port 80, thus a single firewall application rule will redirect inbound traffic toohello external IP, toohello web servers internal IP address.</span></span> <span data-ttu-id="daebc-220">Hello přesměrování provozu relace by být NAT měl toohello interního serveru.</span><span class="sxs-lookup"><span data-stu-id="daebc-220">hello redirected traffic session would be NAT’d toohello internal server.</span></span>
     * <span data-ttu-id="daebc-221">Hello je druhé pravidlo pro provoz aplikace hello back-end pravidlo tooallow hello Webový Server tootalk toohello AppVM01 serveru (ale ne AppVM02) prostřednictvím libovolný port.</span><span class="sxs-lookup"><span data-stu-id="daebc-221">hello second Application Traffic Rule is hello back end rule tooallow hello Web Server tootalk toohello AppVM01 server (but not AppVM02) via any port.</span></span>
* <span data-ttu-id="daebc-222">Vnitřní pravidla (pro provoz intra-VNet)</span><span class="sxs-lookup"><span data-stu-id="daebc-222">Internal Rules (for intra-VNet traffic)</span></span>
  1. <span data-ttu-id="daebc-223">TooInternet odchozí pravidlo: Toto pravidlo povolí přenosy z jakékoli sítě toopass toohello vybrané sítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-223">Outbound tooInternet Rule: This rule will allow traffic from any network toopass toohello selected networks.</span></span> <span data-ttu-id="daebc-224">Toto pravidlo je obvykle výchozí pravidlo už v bráně firewall hello, ale v zakázaném stavu.</span><span class="sxs-lookup"><span data-stu-id="daebc-224">This rule is usually a default rule already on hello firewall, but in a disabled state.</span></span> <span data-ttu-id="daebc-225">Toto pravidlo by měly být povoleny v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="daebc-225">This rule should be enabled for this example.</span></span>
  2. <span data-ttu-id="daebc-226">Pravidlo DNS: Toto pravidlo umožňuje pouze (port 53) provoz toopass toohello DNS server DNS.</span><span class="sxs-lookup"><span data-stu-id="daebc-226">DNS Rule: This rule allows only DNS (port 53) traffic toopass toohello DNS server.</span></span> <span data-ttu-id="daebc-227">Toto pravidlo pro toto prostředí, které se většina přenos dat z hello front-endu toohello back-end je blokován, konkrétně umožňuje DNS z jakékoli místní podsítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-227">For this environment most traffic from hello Frontend toohello Backend is blocked, this rule specifically allows DNS from any local subnet.</span></span>
  3. <span data-ttu-id="daebc-228">Podsíť tooSubnet pravidlo: Toto pravidlo je tooallow jakýkoli server na hello back end podsíť tooconnect tooany server na hello podsítě front end (ale není hello zpětné).</span><span class="sxs-lookup"><span data-stu-id="daebc-228">Subnet tooSubnet Rule: This rule is tooallow any server on hello back end subnet tooconnect tooany server on hello front end subnet (but not hello reverse).</span></span>
* <span data-ttu-id="daebc-229">Pohotovostního pravidlo (pro provoz, který nesplňuje některé z výše uvedených hello):</span><span class="sxs-lookup"><span data-stu-id="daebc-229">Fail-safe Rule (for traffic that doesn’t meet any of hello above):</span></span>
  1. <span data-ttu-id="daebc-230">Všechny přenosy pravidlo odepřít: To by mělo být vždy hello konečné pravidlo (z hlediska priorita) a jako takový Pokud přenosy dat se nezdaří toomatch žádné hello předcházející pravidla, která se zahodí tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="daebc-230">Deny All Traffic Rule: This should always be hello final rule (in terms of priority), and as such if a traffic flows fails toomatch any of hello preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="daebc-231">Toto je výchozí pravidlo a obvykle aktivaci žádné je obecně nutné provést změny.</span><span class="sxs-lookup"><span data-stu-id="daebc-231">This is a default rule and usually activated, no modifications are generally needed.</span></span>

> [!TIP]
> <span data-ttu-id="daebc-232">Na hello druhý aplikace pravidlo pro provoz jakéhokoli portu je povolen pro snadné tohoto příkladu, skutečné scénář hello nejvíce konkrétní port a rozsahy adres musí být použité tooreduce hello prostor pro útok toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="daebc-232">On hello second application traffic rule, any port is allowed for easy of this example, in a real scenario hello most specific port and address ranges should be used tooreduce hello attack surface of this rule.</span></span>
> 
> 

<br />

> [!IMPORTANT]
> <span data-ttu-id="daebc-233">Jakmile všechny hello výše pravidla vytvořeny, je důležité tooreview hello prioritu každé pravidlo tooensure provozu se povoluje nebo odepírá podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="daebc-233">Once all of hello above rules are created, it’s important tooreview hello priority of each rule tooensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="daebc-234">V tomto příkladu jsou hello pravidla v pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="daebc-234">For this example, hello rules are in priority order.</span></span> <span data-ttu-id="daebc-235">Je snadno toobe zamknout z brány firewall hello kvůli toomis seřazených pravidel.</span><span class="sxs-lookup"><span data-stu-id="daebc-235">It's easy toobe locked out of hello firewall due toomis-ordered rules.</span></span> <span data-ttu-id="daebc-236">Minimálně zkontrolujte, zda hello správy pro bránu firewall hello samotné vždy hello pravidlo absolutní nejvyšší prioritou.</span><span class="sxs-lookup"><span data-stu-id="daebc-236">At a minimum, ensure hello management for hello firewall itself is always hello absolute highest priority rule.</span></span>
> 
> 

### <a name="rule-prerequisites"></a><span data-ttu-id="daebc-237">Pravidla požadavků</span><span class="sxs-lookup"><span data-stu-id="daebc-237">Rule Prerequisites</span></span>
<span data-ttu-id="daebc-238">Jeden požadovaných hello virtuální počítač spuštěný hello brány firewall jsou veřejné koncové body.</span><span class="sxs-lookup"><span data-stu-id="daebc-238">One prerequisite for hello Virtual Machine running hello firewall are public endpoints.</span></span> <span data-ttu-id="daebc-239">Pro přenosy tooprocess hello brány firewall musí být otevřený hello odpovídající veřejné koncové body.</span><span class="sxs-lookup"><span data-stu-id="daebc-239">For hello firewall tooprocess traffic hello appropriate public endpoints must be open.</span></span> <span data-ttu-id="daebc-240">Existují tři typy přenosů dat v tomto příkladu; 1) správu provoz toocontrol hello bránu firewall a pravidla brány firewall, servery windows hello toocontrol provoz protokolu RDP 2) a provoz 3) aplikace.</span><span class="sxs-lookup"><span data-stu-id="daebc-240">There are three types of traffic in this example; 1) Management traffic toocontrol hello firewall and firewall rules, 2) RDP traffic toocontrol hello windows servers, and 3) Application Traffic.</span></span> <span data-ttu-id="daebc-241">Toto jsou tři sloupce hello typů přenosů v horním hello polovinu logickém zobrazení pravidla brány firewall hello výše.</span><span class="sxs-lookup"><span data-stu-id="daebc-241">These are hello three columns of traffic types in hello upper half of logical view of hello firewall rules above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="daebc-242">Je zde klíče takeway tooremember, **všechny** provoz se odešlou přes bránu firewall hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-242">A key takeway here is tooremember that **all** traffic will come through hello firewall.</span></span> <span data-ttu-id="daebc-243">Ano tooremote plochy toohello IIS01 serveru, i když je v hello Front End cloudové služby a v hello podsítě Front End, tooaccess tento server, které je nutné zadat tooRDP toohello brány firewall na portu 8014 a pak umožnit hello brány firewall tooroute hello RDP požadavek interně toohello IIS01 portu RDP.</span><span class="sxs-lookup"><span data-stu-id="daebc-243">So tooremote desktop toohello IIS01 server, even though it's in hello Front End Cloud Service and on hello Front End subnet, tooaccess this server we will need tooRDP toohello firewall on port 8014, and then allow hello firewall tooroute hello RDP request internally toohello IIS01 RDP Port.</span></span> <span data-ttu-id="daebc-244">Hello portál Azure "připojit" tlačítko nebude fungovat, protože neexistuje žádná přímá cesta tooIIS01 protokolu RDP (jde o hello portálu uvidí).</span><span class="sxs-lookup"><span data-stu-id="daebc-244">hello Azure portal's "Connect" button won't work because there is no direct RDP path tooIIS01 (as far as hello portal can see).</span></span> <span data-ttu-id="daebc-245">To znamená, všechna připojení z hello Internetu bude toohello služby zabezpečení a Port, například secscv001.cloudapp.net:xxxx (referenční dokumentace hello výše diagram hello mapování portů externí, interní IP a Port).</span><span class="sxs-lookup"><span data-stu-id="daebc-245">This means all connections from hello internet will be toohello Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx (reference hello above diagram for hello mapping of External Port and Internal IP and Port).</span></span>
> 
> 

<span data-ttu-id="daebc-246">Koncový bod lze otevřít buď v době hello vytvoření virtuálního počítače nebo odeslat sestavení je provést v hello ukázkový skript a znázorněném na tento fragment kódu (Poznámka; všechny položky počínaje znak dolaru (např: $VMName[$i]) je uživatelem definované proměnné ze skriptu hello v hello referen část CE v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="daebc-246">An endpoint can be opened either at hello time of VM creation or post build as is done in hello example script and shown below in this code snippet (Note; any item beginning with a dollar sign (e.g.: $VMName[$i]) is a user defined variable from hello script in hello reference section of this document.</span></span> <span data-ttu-id="daebc-247">Hello "$i" v hranatých závorkách [$i] představuje hello pole počet konkrétní virtuální počítač v matici virtuálních počítačů):</span><span class="sxs-lookup"><span data-stu-id="daebc-247">hello “$i” in brackets, [$i], represents hello array number of a specific VM in an array of VMs):</span></span>

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

<span data-ttu-id="daebc-248">I když se zobrazují zde není jasně kvůli toohello použití proměnných, ale koncové body jsou **pouze** otevřít na hello zabezpečení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="daebc-248">Although not clearly shown here due toohello use of variables, but endpoints are **only** opened on hello Security Cloud Service.</span></span> <span data-ttu-id="daebc-249">Toto je tooensure, že se zpracovává veškerý příchozí provoz (směrovat, NAT měl, vynechané) bránou hello firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-249">This is tooensure that all inbound traffic is handled (routed, NAT'd, dropped) by hello firewall.</span></span>

<span data-ttu-id="daebc-250">Klient správy bude potřebovat toobe nainstalovaný na počítači toomanage hello firewall a vytvoření konfigurací hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="daebc-250">A management client will need toobe installed on a PC toomanage hello firewall and create hello configurations needed.</span></span> <span data-ttu-id="daebc-251">V tom, jak toomanage hello zařízení najdete v článku dodavatele hello dokumentace z brány firewall (nebo jiné hodnocení chyb zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="daebc-251">See hello documentation from your firewall (or other NVA) vendor on how toomanage hello device.</span></span> <span data-ttu-id="daebc-252">Hello zbytek této části a hello další části, vytváření pravidel brány Firewall, popíše hello konfigurace brány firewall hello, samostatně, prostřednictvím klienta pro správu dodavatelé hello (tzn. ne hello portál Azure nebo PowerShell).</span><span class="sxs-lookup"><span data-stu-id="daebc-252">hello remainder of this section and hello next section, Firewall Rules Creation, will describe hello configuration of hello firewall itself, through hello vendors management client (i.e. not hello Azure portal or PowerShell).</span></span>

<span data-ttu-id="daebc-253">Pokyny pro stažení klienta a připojování toohello Barracuda použité v tomto příkladu naleznete zde: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="daebc-253">Instructions for client download and connecting toohello Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="daebc-254">Po přihlášení na hello brány firewall, ale před vytvořením pravidel brány firewall, existují dvě třídy požadovaných objektů, které může usnadnit vytváření pravidel hello; Objekty, síť a služby.</span><span class="sxs-lookup"><span data-stu-id="daebc-254">Once logged onto hello firewall but before creating firewall rules, there are two prerequisite object classes that can make creating hello rules easier; Network & Service objects.</span></span>

<span data-ttu-id="daebc-255">V tomto příkladu tři objekty pojmenované síti musí být definované (jeden pro podsíť Frontend hello a hello back-end podsíť, také síťového objektu pro hello IP adresu serveru DNS hello).</span><span class="sxs-lookup"><span data-stu-id="daebc-255">For this example, three named network objects should be defined (one for hello Frontend subnet and hello Backend subnet, also a network object for hello IP address of hello DNS server).</span></span> <span data-ttu-id="daebc-256">toocreate pojmenované síti; od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfigurační oddíl klikněte Ruleset, pak klikněte na tlačítko "Sítě" v části nabídky hello objekty brány Firewall a pak klikněte na nový v nabídce Upravit sítě hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-256">toocreate a named network; starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset, then click “Networks” under hello Firewall Objects menu, then click New in hello Edit Networks menu.</span></span> <span data-ttu-id="daebc-257">objekt Hello sítě může být nyní vytvořen přidáním hello název a předponu hello:</span><span class="sxs-lookup"><span data-stu-id="daebc-257">hello network object can now be created, by adding hello name and hello prefix:</span></span>

<span data-ttu-id="daebc-258">![Vytvoření objektu front-endové síti][3]</span><span class="sxs-lookup"><span data-stu-id="daebc-258">![Create a FrontEnd Network Object][3]</span></span>

<span data-ttu-id="daebc-259">Tím se vytvoří pojmenované sítě pro podsíť FrontEnd hello, podobně jako objekt měl být vytvořen pro hello back-end i podsíť.</span><span class="sxs-lookup"><span data-stu-id="daebc-259">This will create a named network for hello FrontEnd subnet, a similar object should be created for hello BackEnd subnet as well.</span></span> <span data-ttu-id="daebc-260">Nyní hello podsítě lze snadněji odkazovat podle názvu v pravidlech brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-260">Now hello subnets can be more easily referenced by name in hello firewall rules.</span></span>

<span data-ttu-id="daebc-261">Pro hello objekt serveru DNS:</span><span class="sxs-lookup"><span data-stu-id="daebc-261">For hello DNS Server Object:</span></span>

<span data-ttu-id="daebc-262">![Vytvořit objekt serveru DNS][4]</span><span class="sxs-lookup"><span data-stu-id="daebc-262">![Create a DNS Server Object][4]</span></span>

<span data-ttu-id="daebc-263">V pravidle DNS později v dokumentu hello použije tento jeden odkaz na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="daebc-263">This single IP address reference will be utilized in a DNS rule later in hello document.</span></span>

<span data-ttu-id="daebc-264">Hello druhý požadované objekty jsou objekty služby.</span><span class="sxs-lookup"><span data-stu-id="daebc-264">hello second prerequisite objects are Services objects.</span></span> <span data-ttu-id="daebc-265">Toto bude reprezentovat hello porty pro připojení RDP pro každý server.</span><span class="sxs-lookup"><span data-stu-id="daebc-265">These will represent hello RDP connection ports for each server.</span></span> <span data-ttu-id="daebc-266">Vzhledem k tomu, že hello existující objekt služby RDP je vázané toohello, výchozí port protokolu RDP, 3389, nové služby lze vytvořit tooallow provoz z externí porty hello (8014-8026).</span><span class="sxs-lookup"><span data-stu-id="daebc-266">Since hello existing RDP service object is bound toohello default RDP port, 3389, new Services can be created tooallow traffic from hello external ports (8014-8026).</span></span> <span data-ttu-id="daebc-267">nové porty Hello také nelze přidat existující službu RDP toohello, ale pro usnadnění ukázku, můžete vytvořit jednotlivých pravidel pro každý server.</span><span class="sxs-lookup"><span data-stu-id="daebc-267">hello new ports could also be added toohello existing RDP service, but for ease of demonstration, an individual rule for each server can be created.</span></span> <span data-ttu-id="daebc-268">toocreate nové pravidlo protokolu RDP pro server; od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfiguraci části klikněte na tlačítko Ruleset, pak klikněte na tlačítko "Služby" v části hello objekty brány Firewall nabídky, přejděte dolů hello seznam služeb a vyberte hello Služba "RDP".</span><span class="sxs-lookup"><span data-stu-id="daebc-268">toocreate a new RDP rule for a server; starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset, then click “Services” under hello Firewall Objects menu, scroll down hello list of services and select hello “RDP” service.</span></span> <span data-ttu-id="daebc-269">Klikněte pravým tlačítkem a vyberte kopie, pak klikněte pravým tlačítkem a vyberte vložení.</span><span class="sxs-lookup"><span data-stu-id="daebc-269">Right-click and select copy, then right-click and select Paste.</span></span> <span data-ttu-id="daebc-270">Je nyní objekt služby RDP Copy1, který lze upravovat.</span><span class="sxs-lookup"><span data-stu-id="daebc-270">There is now a RDP-Copy1 Service Object that can be edited.</span></span> <span data-ttu-id="daebc-271">Klikněte pravým tlačítkem na RDP Copy1 a vyberte upravit, hello upravit objekt služby objeví se okno si, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="daebc-271">Right-click RDP-Copy1 and select Edit, hello Edit Service Object window will pop up as shown here:</span></span>

<span data-ttu-id="daebc-272">![Kopii výchozí pravidlo protokolu RDP][5]</span><span class="sxs-lookup"><span data-stu-id="daebc-272">![Copy of Default RDP Rule][5]</span></span>

<span data-ttu-id="daebc-273">Hello hodnoty mohou být potom upravenou toorepresent hello služba protokolu RDP pro určitý server.</span><span class="sxs-lookup"><span data-stu-id="daebc-273">hello values can then be edited toorepresent hello RDP service for a specific server.</span></span> <span data-ttu-id="daebc-274">Pro AppVM01 hello nad výchozí pravidlo protokolu RDP by měl být upravené tooreflect nový název služby, popis, a používá externí Port protokolu RDP v hello diagram obrázek 8 (Poznámka: porty hello se změnil z hello RDP výchozí 3389 portu externí toohello, které se používá pro tento konkrétní server, v případě hello AppVM01 hello externí portu je 8025) hello upravené služby jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="daebc-274">For AppVM01 hello above default RDP rule should be modified tooreflect a new Service Name, Description, and external RDP Port used in hello Figure 8 diagram (Note: hello ports are changed from hello RDP default of 3389 toohello external port being used for this specific server, in hello case of AppVM01 hello external Port is 8025) hello modified service is shown below:</span></span>

<span data-ttu-id="daebc-275">![Pravidlo AppVM01][6]</span><span class="sxs-lookup"><span data-stu-id="daebc-275">![AppVM01 Rule][6]</span></span>

<span data-ttu-id="daebc-276">Tento proces musí být opakovaných toocreate služby protokolu RDP pro hello zbývající servery; AppVM02, DNS01 a IIS01.</span><span class="sxs-lookup"><span data-stu-id="daebc-276">This process must be repeated toocreate RDP Services for hello remaining servers; AppVM02, DNS01, and IIS01.</span></span> <span data-ttu-id="daebc-277">Vytvoření Hello tyto služby budou vytvoření pravidla hello jednodušší a zřejmější v další části hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-277">hello creation of these Services will make hello Rule creation simpler and more obvious in hello next section.</span></span>

> [!NOTE]
> <span data-ttu-id="daebc-278">Služby protokolu RDP pro hello brány Firewall není potřeba dvou důvodů; je 1) první hello brána firewall virtuálních počítačů založenými na systému Linux obrázku tak, aby použila SSH port 22 pro správu virtuálních počítačů místo protokolu RDP a 2) port 22, a dva další správu porty jsou povolené v první pravidlo správy hello popsané dál tooallow pro možnosti připojení správy.</span><span class="sxs-lookup"><span data-stu-id="daebc-278">An RDP service for hello Firewall is not needed for two reasons; 1) first hello firewall VM is a Linux based image so SSH would be used on port 22 for VM management instead of RDP, and 2) port 22, and two other management ports are allowed in hello first management rule described below tooallow for management connectivity.</span></span>
> 
> 

### <a name="firewall-rules-creation"></a><span data-ttu-id="daebc-279">Vytvoření pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-279">Firewall Rules Creation</span></span>
<span data-ttu-id="daebc-280">Existují tři typy pravidel brány firewall použité v tomto příkladu, všechny mají odlišné ikony:</span><span class="sxs-lookup"><span data-stu-id="daebc-280">There are three types of firewall rules used in this example, they all have distinct icons:</span></span>

<span data-ttu-id="daebc-281">pravidlo přesměrování aplikace Hello: ![přesměrování ikona aplikace][7]</span><span class="sxs-lookup"><span data-stu-id="daebc-281">hello Application Redirect rule: ![Application Redirect Icon][7]</span></span>

<span data-ttu-id="daebc-282">pravidlo NAT cílové Hello: ![ikonu cílové NAT][8]</span><span class="sxs-lookup"><span data-stu-id="daebc-282">hello Destination NAT rule: ![Destination NAT Icon][8]</span></span>

<span data-ttu-id="daebc-283">pravidlo průchodu Hello: ![předat ikonu][9]</span><span class="sxs-lookup"><span data-stu-id="daebc-283">hello Pass rule: ![Pass Icon][9]</span></span>

<span data-ttu-id="daebc-284">Další informace o těchto pravidel lze najít na hello Barracuda webové stránky.</span><span class="sxs-lookup"><span data-stu-id="daebc-284">More information on these rules can be found at hello Barracuda web site.</span></span>

<span data-ttu-id="daebc-285">toocreate hello následující pravidla (nebo ověřit existující výchozí pravidla), od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfigurace oddíl, klikněte na Ruleset.</span><span class="sxs-lookup"><span data-stu-id="daebc-285">toocreate hello following rules (or verify existing default rules), starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="daebc-286">Mřížka názvem, zobrazí "Hlavní pravidla" hello existující aktivní a deaktivované pravidla v této bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-286">A grid called, “Main Rules” will show hello existing active and deactivated rules on this firewall.</span></span> <span data-ttu-id="daebc-287">V horním pravém rohu hello tento mřížky je malý, zelená "+" tlačítko, klikněte na tento toocreate nové pravidlo (Poznámka: Brána firewall může být "uzamčen" změny, pokud se zobrazí tlačítko označené "Zámek" a jsou nelze toocreate nebo upravit pravidla, klikněte na toto tlačítko příliš "odemknout" hello se pravidlo t a povolení úprav).</span><span class="sxs-lookup"><span data-stu-id="daebc-287">In hello upper right corner of this grid is a small, green “+” button, click this toocreate a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable toocreate or edit rules, click this button too“unlock” hello rule set and allow editing).</span></span> <span data-ttu-id="daebc-288">Pokud chcete tooedit existující pravidlo, vyberte toto pravidlo, klikněte pravým tlačítkem a vyberte Upravit pravidlo.</span><span class="sxs-lookup"><span data-stu-id="daebc-288">If you wish tooedit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="daebc-289">Jakmile pravidel se vytvořit nebo upravit, musí být nabídnutých toohello brány firewall a pak se aktivuje, pokud to neuděláte hello pravidlo změny se neprojeví.</span><span class="sxs-lookup"><span data-stu-id="daebc-289">Once your rules are created and/or modified, they must be pushed toohello firewall and then activated, if this is not done hello rule changes will not take effect.</span></span> <span data-ttu-id="daebc-290">Hello nabízení a aktivace proces je popsán níže hello podrobnosti pravidlo popisy.</span><span class="sxs-lookup"><span data-stu-id="daebc-290">hello push and activation process is described below hello details rule descriptions.</span></span>

<span data-ttu-id="daebc-291">Specifikace Hello každé pravidlo vyžaduje toocomplete v tomto příkladu jsou popsány následovně:</span><span class="sxs-lookup"><span data-stu-id="daebc-291">hello specifics of each rule required toocomplete this example are described as follows:</span></span>

* <span data-ttu-id="daebc-292">**Brány firewall pravidla správy**: Tato aplikace přesměrování pravidlo umožňuje provoz toopass toohello správu porty hello sítě virtuální zařízení, v tomto příkladu Barracuda NextGen Firewall.</span><span class="sxs-lookup"><span data-stu-id="daebc-292">**Firewall Management Rule**: This App Redirect rule allows traffic toopass toohello management ports of hello network virtual appliance, in this example a Barracuda NextGen Firewall.</span></span> <span data-ttu-id="daebc-293">Hello správu porty jsou 801, 807 a volitelně 22.</span><span class="sxs-lookup"><span data-stu-id="daebc-293">hello management ports are 801, 807 and optionally 22.</span></span> <span data-ttu-id="daebc-294">Hello externí i interní porty jsou hello stejné (tj. žádné překlad port).</span><span class="sxs-lookup"><span data-stu-id="daebc-294">hello external and internal ports are hello same (i.e. no port translation).</span></span> <span data-ttu-id="daebc-295">Toto pravidlo, instalační program-MGMT-přístup, je výchozí pravidlo a povolena ve výchozím nastavení (Barracuda NextGen Firewall verze 6.1).</span><span class="sxs-lookup"><span data-stu-id="daebc-295">This rule, SETUP-MGMT-ACCESS, is a default rule and enabled by default (in Barracuda NextGen Firewall version 6.1).</span></span>
  
    <span data-ttu-id="daebc-296">![Pravidlo brány firewall správy][10]</span><span class="sxs-lookup"><span data-stu-id="daebc-296">![Firewall Management Rule][10]</span></span>

> [!TIP]
> <span data-ttu-id="daebc-297">Hello zdroj adresní prostor v tomto pravidle je všechny, pokud hello správu rozsahů IP adres jsou známé, tento zúžíte by také snížit hello útoku prostor toohello správu porty.</span><span class="sxs-lookup"><span data-stu-id="daebc-297">hello source address space in this rule is Any, if hello management IP address ranges are known, reducing this scope would also reduce hello attack surface toohello management ports.</span></span>
> 
> 

* <span data-ttu-id="daebc-298">**Pravidla RDP**: pravidla NAT tyto cílové vám umožní správu hello jednotlivé servery prostřednictvím protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="daebc-298">**RDP Rules**:  These Destination NAT rules will allow management of hello individual servers via RDP.</span></span>
  <span data-ttu-id="daebc-299">Toto pravidlo existují čtyři toocreate kritické potřebná pole:</span><span class="sxs-lookup"><span data-stu-id="daebc-299">There are four critical fields needed toocreate this rule:</span></span>
  
  1. <span data-ttu-id="daebc-300">Zdroj – tooallow RDP z libovolného místa, odkaz hello "Žádný" se používá v pole hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="daebc-300">Source – tooallow RDP from anywhere, hello reference “Any” is used in hello Source field.</span></span>
  2. <span data-ttu-id="daebc-301">Služba – použít odpovídající objekt služby vytvořený v tomto případě "AppVM01 RDP" hello, externí porty hello přesměrování toohello servery místní IP adresu a tooport 3386 (hello výchozí port protokolu RDP).</span><span class="sxs-lookup"><span data-stu-id="daebc-301">Service – use hello appropriate Service Object created earlier, in this case “AppVM01 RDP”, hello external ports redirect toohello servers local IP address and tooport 3386 (hello default RDP port).</span></span> <span data-ttu-id="daebc-302">Tato konkrétní pravidlo je tooAppVM01 přístup RDP.</span><span class="sxs-lookup"><span data-stu-id="daebc-302">This specific rule is for RDP access tooAppVM01.</span></span>
  3. <span data-ttu-id="daebc-303">Cíl – musí být hello *místního portu v bráně firewall hello*, "IP místní server DHCP 1" nebo eth0, pokud se používá statické IP adresy.</span><span class="sxs-lookup"><span data-stu-id="daebc-303">Destination – should be hello *local port on hello firewall*, “DCHP 1 Local IP” or eth0 if using static IPs.</span></span> <span data-ttu-id="daebc-304">Hello řadová číslovka (eth0, eth1 atd.) může být odlišné, pokud vaše síťové zařízení má více místní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="daebc-304">hello ordinal number (eth0, eth1, etc) may be different if your network appliance has multiple local interfaces.</span></span> <span data-ttu-id="daebc-305">Je to hello port odesílá hello brány firewall z (může být hello stejné jako hello přijetí port), skutečný směrované cíl hello je v poli cílového seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-305">This is hello port hello firewall is sending out from (may be hello same as hello receiving port), hello actual routed destination is in hello Target List field.</span></span>
  4. <span data-ttu-id="daebc-306">Přesměrování – v této části informuje virtuální zařízení hello kde tooultimately přesměrování tento provoz.</span><span class="sxs-lookup"><span data-stu-id="daebc-306">Redirection – this section tells hello virtual appliance where tooultimately redirect this traffic.</span></span> <span data-ttu-id="daebc-307">Nejjednodušší přesměrování Hello je tooplace hello IP a portu (volitelné) v poli cílového seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-307">hello simplest redirection is tooplace hello IP and Port (optional) in hello Target List field.</span></span> <span data-ttu-id="daebc-308">Pokud není port je použité hello cílový port na hello příchozí požadavek bude používat (ie žádné překlad), pokud je port určený hello port bude také NAT by spolu s hello IP adres.</span><span class="sxs-lookup"><span data-stu-id="daebc-308">If no port is used hello destination port on hello inbound request will be used (ie no translation), if a port is designated hello port will also be NAT’d along with hello IP address.</span></span>
     
     <span data-ttu-id="daebc-309">![Pravidlo brány firewall protokolu RDP][11]</span><span class="sxs-lookup"><span data-stu-id="daebc-309">![Firewall RDP Rule][11]</span></span>
     
     <span data-ttu-id="daebc-310">Celkem čtyři pravidla RDP potřebovat toobe vytvořit:</span><span class="sxs-lookup"><span data-stu-id="daebc-310">A total of four RDP rules will need toobe created:</span></span> 
     
     | <span data-ttu-id="daebc-311">Název pravidla</span><span class="sxs-lookup"><span data-stu-id="daebc-311">Rule Name</span></span> | <span data-ttu-id="daebc-312">Server</span><span class="sxs-lookup"><span data-stu-id="daebc-312">Server</span></span> | <span data-ttu-id="daebc-313">Služba</span><span class="sxs-lookup"><span data-stu-id="daebc-313">Service</span></span> | <span data-ttu-id="daebc-314">Cílového seznamu</span><span class="sxs-lookup"><span data-stu-id="daebc-314">Target List</span></span> |
     | --- | --- | --- | --- |
     | <span data-ttu-id="daebc-315">RDP IIS01</span><span class="sxs-lookup"><span data-stu-id="daebc-315">RDP-to-IIS01</span></span> |<span data-ttu-id="daebc-316">IIS01</span><span class="sxs-lookup"><span data-stu-id="daebc-316">IIS01</span></span> |<span data-ttu-id="daebc-317">IIS01 PROTOKOLU RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-317">IIS01 RDP</span></span> |<span data-ttu-id="daebc-318">10.0.1.4:3389</span><span class="sxs-lookup"><span data-stu-id="daebc-318">10.0.1.4:3389</span></span> |
     | <span data-ttu-id="daebc-319">RDP DNS01</span><span class="sxs-lookup"><span data-stu-id="daebc-319">RDP-to-DNS01</span></span> |<span data-ttu-id="daebc-320">DNS01</span><span class="sxs-lookup"><span data-stu-id="daebc-320">DNS01</span></span> |<span data-ttu-id="daebc-321">DNS01 PROTOKOLU RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-321">DNS01 RDP</span></span> |<span data-ttu-id="daebc-322">10.0.2.4:3389</span><span class="sxs-lookup"><span data-stu-id="daebc-322">10.0.2.4:3389</span></span> |
     | <span data-ttu-id="daebc-323">RDP AppVM01</span><span class="sxs-lookup"><span data-stu-id="daebc-323">RDP-to-AppVM01</span></span> |<span data-ttu-id="daebc-324">AppVM01</span><span class="sxs-lookup"><span data-stu-id="daebc-324">AppVM01</span></span> |<span data-ttu-id="daebc-325">AppVM01 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-325">AppVM01 RDP</span></span> |<span data-ttu-id="daebc-326">10.0.2.5:3389</span><span class="sxs-lookup"><span data-stu-id="daebc-326">10.0.2.5:3389</span></span> |
     | <span data-ttu-id="daebc-327">RDP AppVM02</span><span class="sxs-lookup"><span data-stu-id="daebc-327">RDP-to-AppVM02</span></span> |<span data-ttu-id="daebc-328">AppVM02</span><span class="sxs-lookup"><span data-stu-id="daebc-328">AppVM02</span></span> |<span data-ttu-id="daebc-329">AppVm02 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-329">AppVm02 RDP</span></span> |<span data-ttu-id="daebc-330">10.0.2.6:3389</span><span class="sxs-lookup"><span data-stu-id="daebc-330">10.0.2.6:3389</span></span> |

> [!TIP]
> <span data-ttu-id="daebc-331">Zužující hello oboru hello zdroje a pole služby se zmenší prostor pro útoky hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-331">Narrowing down hello scope of hello Source and Service fields will reduce hello attack surface.</span></span> <span data-ttu-id="daebc-332">Hello nejvíc omezenou oboru, který vám umožní funkce je třeba použít.</span><span class="sxs-lookup"><span data-stu-id="daebc-332">hello most limited scope that will allow functionality should be used.</span></span>
> 
> 

* <span data-ttu-id="daebc-333">**Pravidla pro provoz aplikace**: existují dvě pravidla pro provoz aplikace, hello nejprve pro hello front-end webový provoz a hello druhý pro přenosy back-end hello (např webový server toodata vrstva).</span><span class="sxs-lookup"><span data-stu-id="daebc-333">**Application Traffic Rules**: There are two Application Traffic Rules, hello first for hello front end web traffic, and hello second for hello back end traffic (eg web server toodata tier).</span></span> <span data-ttu-id="daebc-334">Tato pravidla bude záviset na hello síťovou architekturu (kde jsou umístěné vaše servery) a tok přenosů dat (které směr hello přenosy dat a používaných portů).</span><span class="sxs-lookup"><span data-stu-id="daebc-334">These rules will depend on hello network architecture (where your servers are placed) and traffic flows (which direction hello traffic flows, and which ports are used).</span></span>
  
    <span data-ttu-id="daebc-335">Nejprve popsané je pravidlo hello front-endu pro webový provoz:</span><span class="sxs-lookup"><span data-stu-id="daebc-335">First discussed is hello front end rule for web traffic:</span></span>
  
    <span data-ttu-id="daebc-336">![Pravidla brány firewall na webu][12]</span><span class="sxs-lookup"><span data-stu-id="daebc-336">![Firewall Web Rule][12]</span></span>
  
    <span data-ttu-id="daebc-337">Toto pravidlo NAT cílové umožňuje hello skutečné aplikace provoz tooreach hello aplikačnímu serveru.</span><span class="sxs-lookup"><span data-stu-id="daebc-337">This Destination NAT rule allows hello actual application traffic tooreach hello application server.</span></span> <span data-ttu-id="daebc-338">Při hello ostatní pravidla povolit pro zabezpečení, správy, atd., jsou aplikace pravidla povolit externích uživatelů nebo služeb tooaccess aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-338">While hello other rules allow for security, management, etc., Application Rules are what allow external users or services tooaccess hello application(s).</span></span> <span data-ttu-id="daebc-339">V tomto příkladu je jednom webovém serveru na portu 80, proto hello jediné aplikace pravidlo firewallu přesměruje příchozí provoz toohello externí IP, toohello webové servery interní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="daebc-339">For this example, there is a single web server on port 80, thus hello single firewall application rule will redirect inbound traffic toohello external IP, toohello web servers internal IP address.</span></span>
  
    <span data-ttu-id="daebc-340">**Poznámka:**: že neexistuje žádná portu přiřazené do pole cílového seznamu hello, proto hello příchozí port 80 (nebo 443 pro hello služby vybrali) bude použita v hello přesměrování hello webového serveru.</span><span class="sxs-lookup"><span data-stu-id="daebc-340">**Note**: that there is no port assigned in hello Target List field, thus hello inbound port 80 (or 443 for hello Service selected) will be used in hello redirection of hello web server.</span></span> <span data-ttu-id="daebc-341">Pokud hello webový server naslouchá na jiný port, například port 8080, hello cílového seznamu pole může být tooallow aktualizované too10.0.1.4:8080 hello Port také přesměrování.</span><span class="sxs-lookup"><span data-stu-id="daebc-341">If hello web server is listening on a different port, for example port 8080, hello Target List field could be updated too10.0.1.4:8080 tooallow for hello Port redirection as well.</span></span>
  
    <span data-ttu-id="daebc-342">Dobrý den, je další pravidlo pro provoz aplikace hello back-end pravidlo tooallow hello Webový Server tootalk toohello AppVM01 serveru (ale ne AppVM02) prostřednictvím jakékoli služby:</span><span class="sxs-lookup"><span data-stu-id="daebc-342">hello next Application Traffic Rule is hello back end rule tooallow hello Web Server tootalk toohello AppVM01 server (but not AppVM02) via Any service:</span></span>
  
    <span data-ttu-id="daebc-343">![Pravidlo brány firewall AppVM01][13]</span><span class="sxs-lookup"><span data-stu-id="daebc-343">![Firewall AppVM01 Rule][13]</span></span>
  
    <span data-ttu-id="daebc-344">Toto pravidlo průchodu umožňuje žádné serveru IIS na hello front-endu podsíť tooreach hello AppVM01 (IP adresa 10.0.2.5) na libovolném portu pomocí jakékoli protokol tooaccess dat, které hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="daebc-344">This Pass rule allows any IIS server on hello Frontend subnet tooreach hello AppVM01 (IP Address 10.0.2.5) on Any port, using any Protocol tooaccess data needed by hello web application.</span></span>
  
    <span data-ttu-id="daebc-345">V tento snímek obrazovky "\<explicitní dest\>" se používá v hello cílového pole toosignify 10.0.2.5 jako cíl hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-345">In this screen shot an "\<explicit-dest\>" is used in hello Destination field toosignify 10.0.2.5 as hello destination.</span></span> <span data-ttu-id="daebc-346">To může být buď explicitní znázorněné nebo názvem objektu sítě (stejně jako ve hello požadavky pro hello DNS server).</span><span class="sxs-lookup"><span data-stu-id="daebc-346">This could be either explicit as shown or a named Network Object (as was done in hello prerequisites for hello DNS server).</span></span> <span data-ttu-id="daebc-347">Toto je až toohello Správce brány firewall hello jako toowhich metoda se použije.</span><span class="sxs-lookup"><span data-stu-id="daebc-347">This is up toohello administrator of hello firewall as toowhich method will be used.</span></span> <span data-ttu-id="daebc-348">dvakrát klikněte na první prázdný řádek hello pod tooadd 10.0.2.5 jako Explict Desitnation \<explicitní dest\> a zadejte adresu hello v okně hello, která se objeví.</span><span class="sxs-lookup"><span data-stu-id="daebc-348">tooadd 10.0.2.5 as an Explict Desitnation, double-click on hello first blank row under \<explicit-dest\> and enter hello address in hello window that pops up.</span></span>
  
    <span data-ttu-id="daebc-349">S tímto pravidlem předat je potřeba žádné NAT vzhledem k tomu, že toto je interní provoz, takže hello metodu připojení lze nastavit příliš "Ne překládat pomocí SNAT".</span><span class="sxs-lookup"><span data-stu-id="daebc-349">With this Pass Rule, no NAT is needed since this is internal traffic, so hello Connection Method can be set too"No SNAT".</span></span>
  
    <span data-ttu-id="daebc-350">**Poznámka:**: hello zdrojové síti v tomto pravidle je jakýkoli prostředek na podsíť FrontEnd hello, pokud budou existovat jenom jedna nebo známé konkrétní počet webových serverů, objekt síťový prostředek může být vytvořena toobe konkrétnější toothose přesnou IP adresy místo Hello celé podsítě front-endu.</span><span class="sxs-lookup"><span data-stu-id="daebc-350">**Note**: hello Source network in this rule is any resource on hello FrontEnd subnet, if there will only be one, or a known specific number of web servers, a Network Object resource could be created toobe more specific toothose exact IP addresses instead of hello entire Frontend subnet.</span></span>

> [!TIP]
> <span data-ttu-id="daebc-351">Toto pravidlo používá službu hello "Žádné" toomake hello jednodušší toosetup ukázkové aplikace a používat, to také umožní ICMPv4 (ping) v jediné pravidlo.</span><span class="sxs-lookup"><span data-stu-id="daebc-351">This rule uses hello service “Any” toomake hello sample application easier toosetup and use, this will also allow ICMPv4 (ping) in a single rule.</span></span> <span data-ttu-id="daebc-352">Je to ale není doporučený postup.</span><span class="sxs-lookup"><span data-stu-id="daebc-352">However, this is not a recommended practice.</span></span> <span data-ttu-id="daebc-353">Hello porty a protokoly ("služby") by měl být zúžit toohello minimální možné, že umožňuje aplikaci operaci tooreduce hello útok napříč tuto hranici.</span><span class="sxs-lookup"><span data-stu-id="daebc-353">hello ports and protocols (“Services”) should be narrowed toohello minimum possible that allows application operation tooreduce hello attack surface across this boundary.</span></span>
> 
> 

<br />

> [!TIP]
> <span data-ttu-id="daebc-354">I když toto pravidlo zobrazuje odkaz na explicitní dest používá, je třeba použít jednotný přístup v rámci konfigurace brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-354">Although this rule shows an explicit-dest reference being used, a consistent approach should be used throughout hello firewall configuration.</span></span> <span data-ttu-id="daebc-355">Doporučuje se použít tento hello s názvem objektu sítě v rámci pro snazší čitelnost a podpoře.</span><span class="sxs-lookup"><span data-stu-id="daebc-355">It is recommended that hello named Network Object be used throughout for easier readability and supportability.</span></span> <span data-ttu-id="daebc-356">explicitní Hello-dest je použité tooshow sem pouze odkaz na alternativní metoda a se obecně nedoporučuje (hlavně u komplexní konfigurace).</span><span class="sxs-lookup"><span data-stu-id="daebc-356">hello explicit-dest is used here only tooshow an alternative reference method and is not generally recommended (especially for complex configurations).</span></span>
> 
> 

* <span data-ttu-id="daebc-357">**TooInternet odchozí pravidlo**: předat toto pravidlo povolí provoz z jakékoli zdroj toopass toohello vybrané cílové sítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-357">**Outbound tooInternet Rule**: This Pass rule will allow traffic from any Source network toopass toohello selected Destination networks.</span></span> <span data-ttu-id="daebc-358">Toto pravidlo je výchozí pravidlo obvykle již v hello Barracuda NextGen firewall, ale je v zakázaném stavu.</span><span class="sxs-lookup"><span data-stu-id="daebc-358">This rule is a default rule usually already on hello Barracuda NextGen firewall, but is in a disabled state.</span></span> <span data-ttu-id="daebc-359">Pravým tlačítkem myši na toto pravidlo můžete přístup k příkazu aktivovat pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-359">Right-clicking on this rule can access hello Activate Rule command.</span></span> <span data-ttu-id="daebc-360">pravidlo Hello tady uvedené byl upravený tooadd hello dvě místní podsítě, které byly vytvořeny jako odkazy v hello požadovaných části tohoto dokumentu toohello zdrojového atributu tohoto pravidla.</span><span class="sxs-lookup"><span data-stu-id="daebc-360">hello rule shown here has been modified tooadd hello two local subnets that were created as references in hello prerequisite section of this document toohello Source attribute of this rule.</span></span>
  
    <span data-ttu-id="daebc-361">![Odchozí pravidlo brány firewall][14]</span><span class="sxs-lookup"><span data-stu-id="daebc-361">![Firewall Outbound Rule][14]</span></span>
* <span data-ttu-id="daebc-362">**Pravidlo DNS**: předat toto pravidlo umožňuje pouze (port 53) provoz toopass toohello DNS server DNS.</span><span class="sxs-lookup"><span data-stu-id="daebc-362">**DNS Rule**: This Pass rule allows only DNS (port 53) traffic toopass toohello DNS server.</span></span> <span data-ttu-id="daebc-363">Pro toto prostředí, které se většina přenos dat z hello front-endu toohello back-end je blokován konkrétně toto pravidlo umožňuje DNS.</span><span class="sxs-lookup"><span data-stu-id="daebc-363">For this environment most traffic from hello FrontEnd toohello BackEnd is blocked, this rule specifically allows DNS.</span></span>
  
    <span data-ttu-id="daebc-364">![Pravidlo brány firewall DNS][15]</span><span class="sxs-lookup"><span data-stu-id="daebc-364">![Firewall DNS Rule][15]</span></span>
  
    <span data-ttu-id="daebc-365">**Poznámka:**: na této obrazovce je součástí snímek hello metodu připojení.</span><span class="sxs-lookup"><span data-stu-id="daebc-365">**Note**: In this screen shot hello Connection Method is included.</span></span> <span data-ttu-id="daebc-366">Vzhledem k tomu, že toto pravidlo je pro interní IP toointernal IP adresu provoz, není třeba žádné NATing, tento hello způsob připojení nastaven příliš "Ne překládat pomocí SNAT" pro toto pravidlo průchodu.</span><span class="sxs-lookup"><span data-stu-id="daebc-366">Because this rule is for internal IP toointernal IP address traffic, no NATing is required, this hello Connection Method is set too“No SNAT” for this Pass rule.</span></span>
* <span data-ttu-id="daebc-367">**Podsíť tooSubnet pravidlo**: předat toto pravidlo je výchozí pravidlo, které byl aktivován a upravené tooallow jakýkoli server na hello zpět končí na podsíť tooconnect tooany server hello podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="daebc-367">**Subnet tooSubnet Rule**: This Pass rule is a default rule that has been activated and modified tooallow any server on hello back end subnet tooconnect tooany server on hello front end subnet.</span></span> <span data-ttu-id="daebc-368">Toto pravidlo je všechny interní provoz, takže hello metodu připojení lze nastavit tooNo překládat pomocí SNAT.</span><span class="sxs-lookup"><span data-stu-id="daebc-368">This rule is all internal traffic so hello Connection Method can be set tooNo SNAT.</span></span>
  
    <span data-ttu-id="daebc-369">![Pravidlo brány firewall Intra-VNet][16]</span><span class="sxs-lookup"><span data-stu-id="daebc-369">![Firewall Intra-VNet Rule][16]</span></span>
  
    <span data-ttu-id="daebc-370">**Poznámka:**: zaškrtávací políčko hello obousměrný nastavení není kontrolován (ani je zaregistrováno většina pravidla), to je důležité pro toto pravidlo, umožňuje tato pravidla "jeden směrové", připojení lze inicializovat z hello back-end podsíť toohello front-endu sítě, ale není hello zpětného.</span><span class="sxs-lookup"><span data-stu-id="daebc-370">**Note**: hello Bi-directional checkbox is not checked (nor is it checked in most rules), this is significant for this rule in that it makes this rule “one directional”, a connection can be initiated from hello back end subnet toohello front end network, but not hello reverse.</span></span> <span data-ttu-id="daebc-371">Pokud toto zaškrtávací políčko je zaškrtnuto, by toto pravidlo povolit obousměrný přenos, který je z našich logického diagramu není žádoucí.</span><span class="sxs-lookup"><span data-stu-id="daebc-371">If that checkbox was checked, this rule would enable bi-directional traffic, which from our logical diagram is not desired.</span></span>
* <span data-ttu-id="daebc-372">**Všechny přenosy pravidlo Odepřít**: to by mělo být vždy hello konečné pravidlo (z hlediska priorita), a jako takový Pokud přenosem toků selže toomatch některé z předchozích pravidel hello ho budou vynechána tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="daebc-372">**Deny All Traffic Rule**: This should always be hello final rule (in terms of priority), and as such if a traffic flows fails toomatch any of hello preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="daebc-373">Toto je výchozí pravidlo a obvykle aktivaci žádné je obecně nutné provést změny.</span><span class="sxs-lookup"><span data-stu-id="daebc-373">This is a default rule and usually activated, no modifications are generally needed.</span></span> 
  
    <span data-ttu-id="daebc-374">![Pravidlo brány firewall Odepřít][17]</span><span class="sxs-lookup"><span data-stu-id="daebc-374">![Firewall Deny Rule][17]</span></span>

> [!IMPORTANT]
> <span data-ttu-id="daebc-375">Jakmile všechny hello výše pravidla vytvořeny, je důležité tooreview hello prioritu každé pravidlo tooensure provozu se povoluje nebo odepírá podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="daebc-375">Once all of hello above rules are created, it’s important tooreview hello priority of each rule tooensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="daebc-376">V tomto příkladu jsou hello pravidla v hello pořadí, ve kterém by se zobrazit v hello hlavní mřížku předávání pravidla v hello Barracuda klienta pro správu.</span><span class="sxs-lookup"><span data-stu-id="daebc-376">For this example, hello rules are in hello order they should appear in hello Main Grid of forwarding rules in hello Barracuda Management Client.</span></span>
> 
> 

## <a name="rule-activation"></a><span data-ttu-id="daebc-377">Aktivace pravidla</span><span class="sxs-lookup"><span data-stu-id="daebc-377">Rule Activation</span></span>
<span data-ttu-id="daebc-378">Hello ruleset hello ruleset upravit toohello specifikaci diagram hello logiku, musí být odeslán toohello brány firewall a pak se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="daebc-378">With hello ruleset modified toohello specification of hello logic diagram, hello ruleset must be uploaded toohello firewall and then activated.</span></span>

<span data-ttu-id="daebc-379">![Aktivace pravidla brány firewall][18]</span><span class="sxs-lookup"><span data-stu-id="daebc-379">![Firewall Rule Activation][18]</span></span>

<span data-ttu-id="daebc-380">V pravém horním rohu hello hello správy klienta jsou součástí clusteru tlačítka.</span><span class="sxs-lookup"><span data-stu-id="daebc-380">In hello upper right hand corner of hello management client are a cluster of buttons.</span></span> <span data-ttu-id="daebc-381">Klikněte na tlačítko hello "odeslat změny" tlačítko toosend hello upravit pravidla brány firewall toohello a pak klikněte na tlačítko "Aktivovat" hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-381">Click hello “Send Changes” button toosend hello modified rules toohello firewall, then click hello “Activate” button.</span></span>

<span data-ttu-id="daebc-382">Tento příklad prostředí sestavení je s hello aktivace sada pravidel brány firewall pro hello dokončena.</span><span class="sxs-lookup"><span data-stu-id="daebc-382">With hello activation of hello firewall ruleset this example environment build is complete.</span></span>

## <a name="traffic-scenarios"></a><span data-ttu-id="daebc-383">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="daebc-383">Traffic Scenarios</span></span>
> [!IMPORTANT]
> <span data-ttu-id="daebc-384">Klíče takeway je tooremember, **všechny** provoz se odešlou přes bránu firewall hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-384">A key takeway is tooremember that **all** traffic will come through hello firewall.</span></span> <span data-ttu-id="daebc-385">Ano tooremote plochy toohello IIS01 serveru, i když je v hello Front End cloudové služby a v hello podsítě Front End, tooaccess tento server, které je nutné zadat tooRDP toohello brány firewall na portu 8014 a pak umožnit hello brány firewall tooroute hello RDP požadavek interně toohello IIS01 portu RDP.</span><span class="sxs-lookup"><span data-stu-id="daebc-385">So tooremote desktop toohello IIS01 server, even though it's in hello Front End Cloud Service and on hello Front End subnet, tooaccess this server we will need tooRDP toohello firewall on port 8014, and then allow hello firewall tooroute hello RDP request internally toohello IIS01 RDP Port.</span></span> <span data-ttu-id="daebc-386">Hello portál Azure "připojit" tlačítko nebude fungovat, protože neexistuje žádná přímá cesta tooIIS01 protokolu RDP (jde o hello portálu uvidí).</span><span class="sxs-lookup"><span data-stu-id="daebc-386">hello Azure portal's "Connect" button won't work because there is no direct RDP path tooIIS01 (as far as hello portal can see).</span></span> <span data-ttu-id="daebc-387">To znamená, všechna připojení z hello Internetu bude toohello služby zabezpečení a Port, například secscv001.cloudapp.net:xxxx.</span><span class="sxs-lookup"><span data-stu-id="daebc-387">This means all connections from hello internet will be toohello Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx.</span></span>
> 
> 

<span data-ttu-id="daebc-388">Pro tyto scénáře hello následující pravidla brány firewall musí být k dispozici:</span><span class="sxs-lookup"><span data-stu-id="daebc-388">For these scenarios, hello following firewall rules should be in place:</span></span>

1. <span data-ttu-id="daebc-389">Správa brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-389">Firewall Management</span></span>
2. <span data-ttu-id="daebc-390">TooIIS01 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-390">RDP tooIIS01</span></span>
3. <span data-ttu-id="daebc-391">TooDNS01 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-391">RDP tooDNS01</span></span>
4. <span data-ttu-id="daebc-392">TooAppVM01 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-392">RDP tooAppVM01</span></span>
5. <span data-ttu-id="daebc-393">TooAppVM02 protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-393">RDP tooAppVM02</span></span>
6. <span data-ttu-id="daebc-394">Provoz aplikace toohello Web</span><span class="sxs-lookup"><span data-stu-id="daebc-394">App Traffic toohello Web</span></span>
7. <span data-ttu-id="daebc-395">TooAppVM01 provoz aplikace</span><span class="sxs-lookup"><span data-stu-id="daebc-395">App Traffic tooAppVM01</span></span>
8. <span data-ttu-id="daebc-396">Odchozí toohello Internetu</span><span class="sxs-lookup"><span data-stu-id="daebc-396">Outbound toohello Internet</span></span>
9. <span data-ttu-id="daebc-397">TooDNS01 front-endu</span><span class="sxs-lookup"><span data-stu-id="daebc-397">Frontend tooDNS01</span></span>
10. <span data-ttu-id="daebc-398">Přenosy mezi podsítěmi (back-end toofront end pouze)</span><span class="sxs-lookup"><span data-stu-id="daebc-398">Intra-Subnet Traffic (back end toofront end only)</span></span>
11. <span data-ttu-id="daebc-399">Odepřít vše</span><span class="sxs-lookup"><span data-stu-id="daebc-399">Deny All</span></span>

<span data-ttu-id="daebc-400">Hello skutečné brány firewall ruleset bude mít s největší pravděpodobností mnoha dalším pravidlům v přidání toothese, pravidla hello na jakékoli dané brány firewall bude také obsahovat čísla jinou prioritu než těch, které jsou zde uvedeny hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-400">hello actual firewall ruleset will most likely have many other rules in addition toothese, hello rules on any given firewall will also have different priority numbers than hello ones listed here.</span></span> <span data-ttu-id="daebc-401">Tento seznam a přidružená čísla jsou tooprovide relevance mezi právě tyto 11 pravidla a relativní Priorita hello jedna z nich.</span><span class="sxs-lookup"><span data-stu-id="daebc-401">This list and associated numbers are tooprovide relevance between just these eleven rules and hello relative priority amongst them.</span></span> <span data-ttu-id="daebc-402">Jinými slovy; hello "RDP tooIIS01" na hello skutečné brány firewall, může být číslo pravidla 5, ale také je nad pravidlo "RDP tooDNS01" hello, které by zarovnané s hello záměr v tomto seznamu a pravidel "Správa brány Firewall" hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-402">In other words; on hello actual firewall, hello “RDP tooIIS01” may be rule number 5, but as long as it’s below hello “Firewall Management” rule and above hello “RDP tooDNS01” rule it would align with hello intention of this list.</span></span> <span data-ttu-id="daebc-403">seznam Hello bude také pomáhají při hello níže scénáře povolení jako stručný výtah; například "Pravidlo FW 9 (DNS)".</span><span class="sxs-lookup"><span data-stu-id="daebc-403">hello list will also aid in hello below scenarios allowing brevity; e.g. “FW Rule 9 (DNS)”.</span></span> <span data-ttu-id="daebc-404">Jako stručný výtah hello čtyři pravidla RDP bude souhrnně nazývané také, "hello RDP pravidla" po nesouvisejícími tooRDP hello provoz scénář.</span><span class="sxs-lookup"><span data-stu-id="daebc-404">Also for brevity, hello four RDP rules will be collectively called, “hello RDP rules” when hello traffic scenario is unrelated tooRDP.</span></span>

<span data-ttu-id="daebc-405">Odvolat také, že skupiny zabezpečení sítě jsou na místě pro příchozí přenosy z Internetu na hello front-endu a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-405">Also recall that Network Security Groups are in-place for inbound internet traffic on hello Frontend and Backend subnets.</span></span>

#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="daebc-406">(Povoleno) Internet tooWeb serveru</span><span class="sxs-lookup"><span data-stu-id="daebc-406">(Allowed) Internet tooWeb Server</span></span>
1. <span data-ttu-id="daebc-407">Internet uživatelské požadavky HTTP stránky z SecSvc001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="daebc-407">Internet user requests HTTP page from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="daebc-408">Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 toofirewall rozhraní na 10.0.0.4:80</span><span class="sxs-lookup"><span data-stu-id="daebc-408">Cloud service passes traffic through open endpoint on port 80 toofirewall interface on 10.0.0.4:80</span></span>
3. <span data-ttu-id="daebc-409">Žádné skupiny NSG přiřazené podsítě tooSecurity, tak pravidla NSG systému povolit provoz toofirewall</span><span class="sxs-lookup"><span data-stu-id="daebc-409">No NSG assigned tooSecurity subnet, so system NSG rules allow traffic toofirewall</span></span>
4. <span data-ttu-id="daebc-410">Provoz dotkne interní IP adresu brány firewall hello (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="daebc-410">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="daebc-411">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="daebc-411">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="daebc-412">Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-412">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="daebc-413">Nemusíte použít, přesunout toonext pravidlo FW pravidla 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="daebc-413">FW Rules 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="daebc-414">FW pravidla 6 (aplikace: webové) použít, provoz je povolený, zařízení brány firewall NAT. ho too10.0.1.4 (IIS01)</span><span class="sxs-lookup"><span data-stu-id="daebc-414">FW Rule 6 (App: Web) does apply, traffic is allowed, firewall NATs it too10.0.1.4 (IIS01)</span></span>
6. <span data-ttu-id="daebc-415">podsíť Frontend Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-415">hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="daebc-416">Netýká NSG pravidlo 1 (bloku Internet) (Tento provoz byl NAT by bránou hello firewall, proto hello zdrojové adresy je nyní hello brány firewall, která je v podsíti hello zabezpečení a prohlížet podsíť Frontend hello "místní" provoz toobe NSG a je proto povoleno), přesunout toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-416">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by hello firewall, thus hello source address is now hello firewall which is on hello Security subnet and seen by hello Frontend subnet NSG toobe “local” traffic and is thus allowed), move toonext rule</span></span>
   2. <span data-ttu-id="daebc-417">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-417">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="daebc-418">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello</span><span class="sxs-lookup"><span data-stu-id="daebc-418">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
8. <span data-ttu-id="daebc-419">IIS01 pokusí tooinitiates tooAppVM01 relace FTP v podsíti back-end</span><span class="sxs-lookup"><span data-stu-id="daebc-419">IIS01 attempts tooinitiates an FTP session tooAppVM01 on Backend subnet</span></span>
9. <span data-ttu-id="daebc-420">Hello UDR trasy na podsíť Frontend provede další segment hello hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-420">hello UDR route on Frontend subnet makes hello firewall hello next hop</span></span>
10. <span data-ttu-id="daebc-421">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="daebc-421">No outbound rules on Frontend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="daebc-422">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="daebc-422">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="daebc-423">Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-423">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="daebc-424">Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-424">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
    3. <span data-ttu-id="daebc-425">FW pravidla 6 (aplikace: webové) není použít, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-425">FW Rule 6 (App: Web) doesn’t apply, move toonext rule</span></span>
    4. <span data-ttu-id="daebc-426">FW pravidla 7 (aplikace: back-end) použít, provoz je povolený, brány firewall předává too10.0.2.5 provoz (AppVM01)</span><span class="sxs-lookup"><span data-stu-id="daebc-426">FW Rule 7 (App: Backend) does apply, traffic is allowed, firewall forwards traffic too10.0.2.5 (AppVM01)</span></span>
12. <span data-ttu-id="daebc-427">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-427">hello Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="daebc-428">Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-428">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="daebc-429">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-429">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
13. <span data-ttu-id="daebc-430">AppVM01 obdrží požadavek na hello a inicializuje hello relace a odpovídá</span><span class="sxs-lookup"><span data-stu-id="daebc-430">AppVM01 receives hello request and initiates hello session and responds</span></span>
14. <span data-ttu-id="daebc-431">Hello UDR trasy v podsíti back-end umožňuje další segment hello hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-431">hello UDR route on Backend subnet makes hello firewall hello next hop</span></span>
15. <span data-ttu-id="daebc-432">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello back-end podsíť hello odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="daebc-432">Since there are no outbound NSG rules on hello Backend subnet hello response is allowed</span></span>
16. <span data-ttu-id="daebc-433">Protože to vrací provoz na bránu firewall navázanou relaci hello předá hello odpověď zpět toohello webový server (IIS01)</span><span class="sxs-lookup"><span data-stu-id="daebc-433">Because this is returning traffic on an established session hello firewall passes hello response back toohello web server (IIS01)</span></span>
17. <span data-ttu-id="daebc-434">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-434">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="daebc-435">Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-435">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="daebc-436">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-436">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
18. <span data-ttu-id="daebc-437">server služby IIS Hello obdrží odpověď hello, dokončení transakce hello s AppVM01 a potom dokončí vytváření hello odpověď HTTP, tato odpověď HTTP odeslán toohello žadatele</span><span class="sxs-lookup"><span data-stu-id="daebc-437">hello IIS server receives hello response, completes hello transaction with AppVM01, and then completes building hello HTTP response, this HTTP response is sent toohello requestor</span></span>
19. <span data-ttu-id="daebc-438">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello odpovědi hello podsítě front-endu je povoleno</span><span class="sxs-lookup"><span data-stu-id="daebc-438">Since there are no outbound NSG rules on hello Frontend subnet hello response is allowed</span></span>
20. <span data-ttu-id="daebc-439">Hello HTTP odpovědi přístupů hello brány firewall a vzhledem k tomu, že toto je tooan odpovědi hello navázat relaci NAT přijat bránou hello firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-439">hello HTTP response hits hello firewall, and because this is hello response tooan established NAT session is accepted by hello firewall</span></span>
21. <span data-ttu-id="daebc-440">brány firewall Hello pak přesměruje hello odpověď zpět toohello Internet uživatele</span><span class="sxs-lookup"><span data-stu-id="daebc-440">hello firewall then redirects hello response back toohello Internet User</span></span>
22. <span data-ttu-id="daebc-441">Vzhledem k tomu, že neexistují žádné odchozí pravidla NSG nebo UDR směrování na podsíť Frontend hello je povoleno hello odpovědi a hello Internet uživatel obdrží webovou stránku hello požadovaný.</span><span class="sxs-lookup"><span data-stu-id="daebc-441">Since there are no outbound NSG rules or UDR hops on hello Frontend subnet hello response is allowed, and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-internet-rdp-toobackend"></a><span data-ttu-id="daebc-442">(Povoleno) TooBackend Internet RDP</span><span class="sxs-lookup"><span data-stu-id="daebc-442">(Allowed) Internet RDP tooBackend</span></span>
1. <span data-ttu-id="daebc-443">Správce serveru na Internetu požadavky tooAppVM01 relace RDP prostřednictvím SecSvc001.CloudApp.Net:8025, kde 8025 je číslo portu hello přiřazeného uživatele pro pravidlo brány firewall "RDP tooAppVM01" hello</span><span class="sxs-lookup"><span data-stu-id="daebc-443">Server Admin on internet requests RDP session tooAppVM01 via SecSvc001.CloudApp.Net:8025, where 8025 is hello user assigned port number for hello “RDP tooAppVM01” firewall rule</span></span>
2. <span data-ttu-id="daebc-444">Hello Cloudová služba předá provoz prostřednictvím hello otevřených koncových bodů na portu 8025 toofirewall rozhraní na 10.0.0.4:8025</span><span class="sxs-lookup"><span data-stu-id="daebc-444">hello cloud service passes traffic through hello open endpoint on port 8025 toofirewall interface on 10.0.0.4:8025</span></span>
3. <span data-ttu-id="daebc-445">Žádné skupiny NSG přiřazené podsítě tooSecurity, tak pravidla NSG systému povolit provoz toofirewall</span><span class="sxs-lookup"><span data-stu-id="daebc-445">No NSG assigned tooSecurity subnet, so system NSG rules allow traffic toofirewall</span></span>
4. <span data-ttu-id="daebc-446">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="daebc-446">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="daebc-447">Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-447">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="daebc-448">Netýká FW pravidla 2 (RDP IIS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-448">FW Rule 2 (RDP IIS) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="daebc-449">Netýká FW pravidla 3 (RDP DNS01), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-449">FW Rule 3 (RDP DNS01) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="daebc-450">Použít FW pravidla 4 (RDP AppVM01), provoz je povolený, zařízení brány firewall NAT. ho too10.0.2.5:3386 (portu RDP na AppVM01)</span><span class="sxs-lookup"><span data-stu-id="daebc-450">FW Rule 4 (RDP AppVM01) does apply, traffic is allowed, firewall NATs it too10.0.2.5:3386 (RDP port on AppVM01)</span></span>
5. <span data-ttu-id="daebc-451">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-451">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="daebc-452">Netýká NSG pravidlo 1 (bloku Internet) (Tento provoz byl NAT by bránou hello firewall, proto hello zdrojové adresy je nyní hello brány firewall, která je v podsíti hello zabezpečení a prohlížet hello back-end podsítě provoz "local" toobe NSG a je proto povoleno), přesunout toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-452">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by hello firewall, thus hello source address is now hello firewall which is on hello Security subnet and seen by hello Backend subnet NSG toobe “local” traffic and is thus allowed), move toonext rule</span></span>
   2. <span data-ttu-id="daebc-453">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-453">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="daebc-454">AppVM01 naslouchá pro provoz protokolu RDP a odpovídá</span><span class="sxs-lookup"><span data-stu-id="daebc-454">AppVM01 is listening for RDP traffic and responds</span></span>
7. <span data-ttu-id="daebc-455">Žádná odchozí pravidla NSG použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="daebc-455">With no outbound NSG rules, default rules apply and return traffic is allowed</span></span>
8. <span data-ttu-id="daebc-456">UDR trasy odchozí provoz brány firewall toohello jako další segment hello</span><span class="sxs-lookup"><span data-stu-id="daebc-456">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
9. <span data-ttu-id="daebc-457">Protože to vrací provoz na bránu firewall navázanou relaci hello předá hello odpověď zpět toohello internet uživatele</span><span class="sxs-lookup"><span data-stu-id="daebc-457">Because this is returning traffic on an established session hello firewall passes hello response back toohello internet user</span></span>
10. <span data-ttu-id="daebc-458">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="daebc-458">RDP session is enabled</span></span>
11. <span data-ttu-id="daebc-459">AppVM01 vyzve k zadání názvu heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="daebc-459">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="daebc-460">(Povoleno) Webový Server DNS vyhledávání na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="daebc-460">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="daebc-461">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale potřebuje tooresolve hello adresu.</span><span class="sxs-lookup"><span data-stu-id="daebc-461">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="daebc-462">Hello konfiguraci sítě pro virtuální síť seznamy hello DNS01 (10.0.2.4 v podsíti hello back-end) jako primární server DNS hello IIS01 odešle tooDNS01 požadavek DNS hello</span><span class="sxs-lookup"><span data-stu-id="daebc-462">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="daebc-463">UDR trasy odchozí provoz brány firewall toohello jako další segment hello</span><span class="sxs-lookup"><span data-stu-id="daebc-463">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
4. <span data-ttu-id="daebc-464">Podsíť Frontend vázané toohello žádná odchozí pravidla NSG se provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="daebc-464">No outbound NSG rules are bound toohello Frontend subnet, traffic is allowed</span></span>
5. <span data-ttu-id="daebc-465">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="daebc-465">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="daebc-466">Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-466">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="daebc-467">Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-467">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="daebc-468">Nemusíte použít, přesuňte toonext pravidlo FW pravidla 6 a 7 (pravidla aplikace)</span><span class="sxs-lookup"><span data-stu-id="daebc-468">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="daebc-469">Netýká FW pravidlo 8 (tooInternet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-469">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="daebc-470">Použít FW pravidlo 9 (DNS), provoz je povolený, brány firewall předává too10.0.2.4 provoz (DNS01)</span><span class="sxs-lookup"><span data-stu-id="daebc-470">FW Rule 9 (DNS) does apply, traffic is allowed, firewall forwards traffic too10.0.2.4 (DNS01)</span></span>
6. <span data-ttu-id="daebc-471">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-471">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="daebc-472">Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-472">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="daebc-473">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-473">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="daebc-474">DNS server obdrží požadavek hello</span><span class="sxs-lookup"><span data-stu-id="daebc-474">DNS server receives hello request</span></span>
8. <span data-ttu-id="daebc-475">DNS server nemá hello adresu do mezipaměti a požádá kořenový server DNS na hello Internetu</span><span class="sxs-lookup"><span data-stu-id="daebc-475">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
9. <span data-ttu-id="daebc-476">UDR trasy odchozí provoz brány firewall toohello jako další segment hello</span><span class="sxs-lookup"><span data-stu-id="daebc-476">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
10. <span data-ttu-id="daebc-477">Žádná odchozí pravidla NSG na podsítě back-end provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="daebc-477">No outbound NSG rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="daebc-478">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="daebc-478">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="daebc-479">Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-479">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="daebc-480">Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-480">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
    3. <span data-ttu-id="daebc-481">Nemusíte použít, přesuňte toonext pravidlo FW pravidla 6 a 7 (pravidla aplikace)</span><span class="sxs-lookup"><span data-stu-id="daebc-481">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
    4. <span data-ttu-id="daebc-482">Použít pravidlo 8 FW (tooInternet), provoz je povolený, relace je překládat pomocí SNAT out tooroot server DNS na Internetu hello</span><span class="sxs-lookup"><span data-stu-id="daebc-482">FW Rule 8 (tooInternet) does apply, traffic is allowed, session is SNAT out tooroot DNS server on hello Internet</span></span>
12. <span data-ttu-id="daebc-483">Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo inicializováno hello brány firewall, hello odpověď byla přijata bránou hello firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-483">Internet DNS server responds, since this session was initiated from hello firewall, hello response is accepted by hello firewall</span></span>
13. <span data-ttu-id="daebc-484">Protože se jedná navázanou relaci, brány firewall hello předává toohello hello odpověď inicializace serveru DNS01</span><span class="sxs-lookup"><span data-stu-id="daebc-484">As this is an established session, hello firewall forwards hello response toohello initiating server, DNS01</span></span>
14. <span data-ttu-id="daebc-485">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-485">hello Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="daebc-486">Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-486">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="daebc-487">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-487">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
15. <span data-ttu-id="daebc-488">Hello DNS server obdrží a ukládá do mezipaměti odpovědi hello a poté odpoví back tooIIS01 toohello úvodního požadavku</span><span class="sxs-lookup"><span data-stu-id="daebc-488">hello DNS server receives and caches hello response, and then responds toohello initial request back tooIIS01</span></span>
16. <span data-ttu-id="daebc-489">Hello UDR trasy v podsíti back-end umožňuje další segment hello hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-489">hello UDR route on backend subnet makes hello firewall hello next hop</span></span>
17. <span data-ttu-id="daebc-490">Neexistují žádná odchozí pravidla NSG v podsíti hello back-end, provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="daebc-490">No outbound NSG rules exist on hello Backend subnet, traffic is allowed</span></span>
18. <span data-ttu-id="daebc-491">Toto je navázanou relaci v bráně firewall hello, odpověď hello předá serveru IIS back toohello hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-491">This is an established session on hello firewall, hello response is forwarded by hello firewall back toohello IIS server</span></span>
19. <span data-ttu-id="daebc-492">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-492">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="daebc-493">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="daebc-493">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="daebc-494">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený</span><span class="sxs-lookup"><span data-stu-id="daebc-494">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
20. <span data-ttu-id="daebc-495">IIS01 obdrží odpověď hello z DNS01</span><span class="sxs-lookup"><span data-stu-id="daebc-495">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-backend-server-toofrontend-server"></a><span data-ttu-id="daebc-496">(Povoleno) Back-end serveru tooFrontend serveru</span><span class="sxs-lookup"><span data-stu-id="daebc-496">(Allowed) Backend server tooFrontend server</span></span>
1. <span data-ttu-id="daebc-497">Správce přihlášený tooAppVM02 prostřednictvím protokolu RDP požádá o soubor přímo ze serveru IIS01 hello pomocí Průzkumníka souborů systému windows</span><span class="sxs-lookup"><span data-stu-id="daebc-497">An administrator logged on tooAppVM02 via RDP requests a file directly from hello IIS01 server via windows file explorer</span></span>
2. <span data-ttu-id="daebc-498">Hello UDR trasy v podsíti back-end umožňuje další segment hello hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-498">hello UDR route on Backend subnet makes hello firewall hello next hop</span></span>
3. <span data-ttu-id="daebc-499">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello back-end podsíť hello odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="daebc-499">Since there are no outbound NSG rules on hello Backend subnet hello response is allowed</span></span>
4. <span data-ttu-id="daebc-500">Brány firewall zahájí zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="daebc-500">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="daebc-501">Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-501">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="daebc-502">Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-502">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="daebc-503">Nemusíte použít, přesuňte toonext pravidlo FW pravidla 6 a 7 (pravidla aplikace)</span><span class="sxs-lookup"><span data-stu-id="daebc-503">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="daebc-504">Netýká FW pravidlo 8 (tooInternet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-504">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="daebc-505">Netýká FW pravidlo 9 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-505">FW Rule 9 (DNS) doesn’t apply, move toonext rule</span></span>
   6. <span data-ttu-id="daebc-506">Použít pravidlo 10 FW (Intra-podsítě), provoz je povolený, brány firewall předá too10.0.1.4 provoz (IIS01)</span><span class="sxs-lookup"><span data-stu-id="daebc-506">FW Rule 10 (Intra-Subnet) does apply, traffic is allowed, firewall passes traffic too10.0.1.4 (IIS01)</span></span>
5. <span data-ttu-id="daebc-507">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-507">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="daebc-508">Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-508">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="daebc-509">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-509">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="daebc-510">Za předpokladu, že správné ověření a autorizaci, IIS01 přijme požadavek hello a odpoví</span><span class="sxs-lookup"><span data-stu-id="daebc-510">Assuming proper authentication and authorization, IIS01 accepts hello request and responds</span></span>
7. <span data-ttu-id="daebc-511">Hello UDR trasy na podsíť Frontend provede další segment hello hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-511">hello UDR route on Frontend subnet makes hello firewall hello next hop</span></span>
8. <span data-ttu-id="daebc-512">Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello odpovědi hello podsítě front-endu je povoleno</span><span class="sxs-lookup"><span data-stu-id="daebc-512">Since there are no outbound NSG rules on hello Frontend subnet hello response is allowed</span></span>
9. <span data-ttu-id="daebc-513">Protože se jedná o existující relaci v bráně firewall hello této odpovědi je povoleno a brány firewall hello vrátí tooAppVM02 odpovědi hello</span><span class="sxs-lookup"><span data-stu-id="daebc-513">As this is an existing session on hello firewall this response is allowed and hello firewall returns hello response tooAppVM02</span></span>
10. <span data-ttu-id="daebc-514">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="daebc-514">Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="daebc-515">Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-515">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="daebc-516">Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-516">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
11. <span data-ttu-id="daebc-517">AppVM02 obdrží odpověď hello</span><span class="sxs-lookup"><span data-stu-id="daebc-517">AppVM02 receives hello response</span></span>

#### <a name="denied-internet-direct-tooweb-server"></a><span data-ttu-id="daebc-518">(Byl odepřen) Internet přímé tooWeb serveru</span><span class="sxs-lookup"><span data-stu-id="daebc-518">(Denied) Internet direct tooWeb Server</span></span>
1. <span data-ttu-id="daebc-519">Uživatel Internetu pokusí tooaccess hello webový server, IIS01, prostřednictvím hello FrontEnd001.CloudApp.Net služby</span><span class="sxs-lookup"><span data-stu-id="daebc-519">Internet user tries tooaccess hello web server, IIS01, through hello FrontEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="daebc-520">Vzhledem k tomu, že jsou pro přenos HTTP otevřené žádné koncové body, se nebude předávat hello cloudové služby a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="daebc-520">Since there are no endpoints open for HTTP traffic, this would not pass through hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="daebc-521">Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat hello NSG (bloku Internet) na podsíť Frontend hello</span><span class="sxs-lookup"><span data-stu-id="daebc-521">If hello endpoints were open for some reason, hello NSG (Block Internet) on hello Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="daebc-522">Nakonec hello front-endu podsíť UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello IIS01 jako další segment hello a brány firewall hello by tato objeví jako asymetrický provoz a vyřadit hello odchozí odpovědi zacílí existují aspoň tři nezávislá vrstev Defense mezi hello Internetu a IIS01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.</span><span class="sxs-lookup"><span data-stu-id="daebc-522">Finally, hello Frontend subnet UDR route would send any outbound traffic from IIS01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and IIS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-toobackend-server"></a><span data-ttu-id="daebc-523">(Byl odepřen) Internet tooBackend serveru</span><span class="sxs-lookup"><span data-stu-id="daebc-523">(Denied) Internet tooBackend Server</span></span>
1. <span data-ttu-id="daebc-524">Uživatel Internetu pokusí tooaccess souboru na AppVM01 prostřednictvím hello BackEnd001.CloudApp.Net služby</span><span class="sxs-lookup"><span data-stu-id="daebc-524">Internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="daebc-525">Vzhledem k tomu, že jsou pro sdílené složky otevřené žádné koncové body, se nebude předat hello cloudové služby a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="daebc-525">Since there are no endpoints open for file share, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="daebc-526">Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat hello NSG (bloku Internet)</span><span class="sxs-lookup"><span data-stu-id="daebc-526">If hello endpoints were open for some reason, hello NSG (Block Internet) would block this traffic</span></span>
4. <span data-ttu-id="daebc-527">Nakonec hello UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello AppVM01 jako další segment hello a brány firewall hello by tato objeví jako asymetrický provoz a vyřadit hello odchozí odpovědi zacílí existují aspoň tři nezávislá vrstev obrany mezi Hello Internetu a AppVM01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.</span><span class="sxs-lookup"><span data-stu-id="daebc-527">Finally, hello UDR route would send any outbound traffic from AppVM01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and AppVM01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-frontend-server-toobackend-server"></a><span data-ttu-id="daebc-528">(Byl odepřen) Front-endu server tooBackend serveru</span><span class="sxs-lookup"><span data-stu-id="daebc-528">(Denied) Frontend server tooBackend Server</span></span>
1. <span data-ttu-id="daebc-529">Předpokládejme, IIS01 došlo k ohrožení a běží škodlivý kód při tooscan hello back-end podsíť servery.</span><span class="sxs-lookup"><span data-stu-id="daebc-529">Assume IIS01 was compromised and is running malicious code trying tooscan hello Backend subnet servers.</span></span>
2. <span data-ttu-id="daebc-530">Hello front-endu podsíť UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello IIS01 jako další segment hello.</span><span class="sxs-lookup"><span data-stu-id="daebc-530">hello Frontend subnet UDR route would send any outbound traffic from IIS01 toohello firewall as hello next hop.</span></span> <span data-ttu-id="daebc-531">Toto není něco, co může být změněna pomocí hello ohrožené virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="daebc-531">This is not something that can be altered by hello compromised VM.</span></span>
3. <span data-ttu-id="daebc-532">brány firewall Hello by zpracovat hello provoz, pokud byl požadavek hello tooAppVM01 nebo toohello serveru DNS pro vyhledávání DNS, které provoz může být potenciálně povolený bránou firewall hello (z důvodu tooFW pravidla 7 a 9).</span><span class="sxs-lookup"><span data-stu-id="daebc-532">hello firewall would process hello traffic, if hello request was tooAppVM01, or toohello DNS server for DNS lookups that traffic could potentially be allowed by hello firewall (due tooFW Rules 7 and 9).</span></span> <span data-ttu-id="daebc-533">Všechny ostatní přenosy by se zablokovaly podle FW pravidlo 11 (odmítnout vše).</span><span class="sxs-lookup"><span data-stu-id="daebc-533">All other traffic would be blocked by FW Rule 11 (Deny All).</span></span>
4. <span data-ttu-id="daebc-534">Pokud rozšířené detekce hrozeb byl povolen v bráně firewall hello (který není zahrnuté v tomto dokumentu, najdete v dokumentaci dodavatele hello pro vaše konkrétní síťové zařízení advanced threat možnosti), dokonce i provoz, který bude mít možnost podle hello základní předávání pravidla popsané v tomto dokumentu může zabránit, pokud provoz hello obsažené známé podpisům a vzorů, které příznak pravidlo rozšířené hrozba.</span><span class="sxs-lookup"><span data-stu-id="daebc-534">If advanced threat detection was enabled on hello firewall (which is not covered in this document, see hello vendor documentation for your specific network appliance advanced threat capabilities), even traffic that would be allowed by hello basic forwarding rules discussed in this document could be prevented if hello traffic contained known signatures or patterns that flag an advanced threat rule.</span></span>

#### <a name="denied-internet-dns-lookup-on-dns-server"></a><span data-ttu-id="daebc-535">(Byl odepřen) Vyhledávání DNS pro Internet na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="daebc-535">(Denied) Internet DNS lookup on DNS server</span></span>
1. <span data-ttu-id="daebc-536">Uživatel Internetu pokusí toolookup interní DNS záznam na DNS01 BackEnd001.CloudApp.Net pomocí služby</span><span class="sxs-lookup"><span data-stu-id="daebc-536">Internet user tries toolookup an internal DNS record on DNS01 through BackEnd001.CloudApp.Net service</span></span> 
2. <span data-ttu-id="daebc-537">Vzhledem k tomu, že jsou pro přenosy DNS otevřené žádné koncové body, se nebude předávat hello cloudové služby a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="daebc-537">Since there are no endpoints open for DNS traffic, this would not pass through hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="daebc-538">Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat pravidla NSG hello (bloku Internet) na podsíť Frontend hello</span><span class="sxs-lookup"><span data-stu-id="daebc-538">If hello endpoints were open for some reason, hello NSG rule (Block Internet) on hello Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="daebc-539">Nakonec hello back-end podsíť UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello DNS01 jako další segment hello a brány firewall hello by tato objeví jako asymetrický provoz a vyřadit hello odchozí odpovědi zacílí existují aspoň tři nezávislá vrstev Defense mezi hello Internetu a DNS01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.</span><span class="sxs-lookup"><span data-stu-id="daebc-539">Finally, hello Backend subnet UDR route would send any outbound traffic from DNS01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and DNS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-toosql-access-through-firewall"></a><span data-ttu-id="daebc-540">(Byl odepřen) Přístup k Internetu tooSQL prostřednictvím brány Firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-540">(Denied) Internet tooSQL access through Firewall</span></span>
1. <span data-ttu-id="daebc-541">Internet uživatel požádá o dat SQL z SecSvc001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="daebc-541">Internet user requests SQL data from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="daebc-542">Vzhledem k tomu, že nejsou otevřené pro SQL koncové body, se nebude předat hello cloudové služby a nebude dosáhnout hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="daebc-542">Since there are no endpoints open for SQL, this would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="daebc-543">Pokud z nějakého důvodu byly otevřené koncové body SQL, brány firewall hello se začne zpracování pravidla:</span><span class="sxs-lookup"><span data-stu-id="daebc-543">If SQL endpoints were open for some reason, hello firewall would begin rule processing:</span></span>
   1. <span data-ttu-id="daebc-544">Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-544">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="daebc-545">Nemusíte použít, přesunout toonext pravidlo FW pravidla 2 až 5 (RDP pravidla)</span><span class="sxs-lookup"><span data-stu-id="daebc-545">FW Rules 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="daebc-546">Nemusíte použít, přesunout pravidlo toonext FW pravidla 6 a 7 (pravidla pro aplikace)</span><span class="sxs-lookup"><span data-stu-id="daebc-546">FW Rule 6 & 7 (Application Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="daebc-547">Netýká FW pravidlo 8 (tooInternet), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-547">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="daebc-548">Netýká FW pravidlo 9 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-548">FW Rule 9 (DNS) doesn’t apply, move toonext rule</span></span>
   6. <span data-ttu-id="daebc-549">Netýká FW pravidlo 10 (Intra-podsítě), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="daebc-549">FW Rule 10 (Intra-Subnet) doesn’t apply, move toonext rule</span></span>
   7. <span data-ttu-id="daebc-550">Použít FW pravidlo 11 (odmítnout vše), provoz je zpracování blokované, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="daebc-550">FW Rule 11 (Deny All) does apply, traffic is blocked, stop rule processing</span></span>

## <a name="references"></a><span data-ttu-id="daebc-551">Odkazy</span><span class="sxs-lookup"><span data-stu-id="daebc-551">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="daebc-552">Hlavní skript a konfiguraci sítě</span><span class="sxs-lookup"><span data-stu-id="daebc-552">Main Script and Network Config</span></span>
<span data-ttu-id="daebc-553">Uložte hello úplné skript v souboru skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="daebc-553">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="daebc-554">Uložte do souboru s názvem "NetworkConf2.xml" hello konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="daebc-554">Save hello Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="daebc-555">Podle potřeby změňte hello proměnné definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="daebc-555">Modify hello user defined variables as needed.</span></span> <span data-ttu-id="daebc-556">Spusťte skript hello a potom postupujte podle hello brány Firewall pravidla instalace instrukce výše.</span><span class="sxs-lookup"><span data-stu-id="daebc-556">Run hello script, then follow hello Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="daebc-557">Úplné skriptu</span><span class="sxs-lookup"><span data-stu-id="daebc-557">Full Script</span></span>
<span data-ttu-id="daebc-558">Tento skript bude na základě proměnných uživatelsky definované hello:</span><span class="sxs-lookup"><span data-stu-id="daebc-558">This script will, based on hello user defined variables:</span></span>

1. <span data-ttu-id="daebc-559">Připojit tooan předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="daebc-559">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="daebc-560">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="daebc-560">Create a new storage account</span></span>
3. <span data-ttu-id="daebc-561">Vytvořit novou virtuální síť a tři podsítě, jak jsou definovány v souboru konfigurace sítě hello</span><span class="sxs-lookup"><span data-stu-id="daebc-561">Create a new VNet and three subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="daebc-562">Sestavení pět virtuálních počítačů brány firewall 1 a 4 windows server virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="daebc-562">Build five virtual machines; 1 firewall and 4 windows server VMs</span></span>
5. <span data-ttu-id="daebc-563">Konfigurace, včetně UDR:</span><span class="sxs-lookup"><span data-stu-id="daebc-563">Configure UDR including:</span></span>
   1. <span data-ttu-id="daebc-564">Vytváření dva nové směrovací tabulky</span><span class="sxs-lookup"><span data-stu-id="daebc-564">Creating two new route tables</span></span>
   2. <span data-ttu-id="daebc-565">Přidání tras toohello tabulek</span><span class="sxs-lookup"><span data-stu-id="daebc-565">Add routes toohello tables</span></span>
   3. <span data-ttu-id="daebc-566">Vytvoření vazby tabulky tooappropriate podsítě</span><span class="sxs-lookup"><span data-stu-id="daebc-566">Bind tables tooappropriate subnets</span></span>
6. <span data-ttu-id="daebc-567">Povolení předávání IP adres na hello hodnocení chyb zabezpečení</span><span class="sxs-lookup"><span data-stu-id="daebc-567">Enable IP Forwarding on hello NVA</span></span>
7. <span data-ttu-id="daebc-568">Konfigurace, včetně NSG:</span><span class="sxs-lookup"><span data-stu-id="daebc-568">Configure NSG including:</span></span>
   1. <span data-ttu-id="daebc-569">Vytváření skupina NSG</span><span class="sxs-lookup"><span data-stu-id="daebc-569">Creating a NSG</span></span>
   2. <span data-ttu-id="daebc-570">Přidávání pravidla</span><span class="sxs-lookup"><span data-stu-id="daebc-570">Adding a rule</span></span>
   3. <span data-ttu-id="daebc-571">Vazba hello NSG toohello příslušné podsítě</span><span class="sxs-lookup"><span data-stu-id="daebc-571">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="daebc-572">Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.</span><span class="sxs-lookup"><span data-stu-id="daebc-572">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="daebc-573">Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="daebc-573">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="daebc-574">Pouze chybové zprávy červeně jsou příčinou problém.</span><span class="sxs-lookup"><span data-stu-id="daebc-574">Only error messages in red are cause for concern.</span></span>
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
       - One server on hello FrontEnd Subnet
       - Three Servers on hello BackEnd Subnet
       - IP Forwading from hello FireWall out toohello internet
       - User Defined Routing FrontEnd and BackEnd Subnets toohello NVA

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind hello appropriate
      layer(s) of protection. This script serves as an example of some of hello techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and hello appropriate protections needed, and then effectively
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
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes tooreflect your subscription and services
      # Invalid options will fail in hello validation section

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
        # Note: tooensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be hello NVA.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - hello First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - hello Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - hello DNS Server
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

      # Update Subscription Pointer tooNew Storage Account
        Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "hello SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation varible is correct and hello netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see hello above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

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
                # Set up all hello EndPoints we'll need once we're up and running
                # Note: All traffic goes through hello firewall, so we'll need tooset up all ports here.
                #       Also, hello firewall will be redirecting traffic tooa new IP and Port in a forwarding
                #       rule, so all of these endpoint have hello same public and local port and hello firewall
                #       will do hello mapping, NATing, and/or redirection as declared in hello firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
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

      # Create hello Route Tables
        Write-Host "Creating hello Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes tooRoute Tables
        Write-Host "Adding Routes toohello Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate hello Route Tables with hello Subnets
        Write-Host "Binding Route Tables toohello Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on hello Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign hello NSG tootwo Subnets
        # hello NSG is *not* bound toohello Security Subnet. hello result
        # is that internet traffic flows only toohello Security subnet
        # since hello NSG bound toohello Frontend and Backback subnets
        # will Deny internet traffic toothose subnets.
        Write-Host "Binding hello NSG tootwo subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on hello IIS Server)
      # Install Backend resource (Run Post-Build Script on hello AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a><span data-ttu-id="daebc-575">Soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="daebc-575">Network Config File</span></span>
<span data-ttu-id="daebc-576">Uložte tento soubor xml s aktualizované umístění a přidáte odkaz toothis hello souboru toohello $NetworkConfigFile proměnné ve skriptu hello výše.</span><span class="sxs-lookup"><span data-stu-id="daebc-576">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello script above.</span></span>

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

#### <a name="sample-application-scripts"></a><span data-ttu-id="daebc-577">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="daebc-577">Sample Application Scripts</span></span>
<span data-ttu-id="daebc-578">Pokud chcete tooinstall ukázková aplikace pro toto a další příklady DMZ jeden bylo zadáno v hello následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="daebc-578">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "DMZ obousměrně s hodnocení chyb zabezpečení, NSG a UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logickém zobrazení hello pravidla brány Firewall"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Vytvoření objektu front-endové síti"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Vytvořit objekt serveru DNS"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopii výchozí pravidlo protokolu RDP"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "Pravidlo AppVM01"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Ikona aplikace přesměrování"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ikona cílové NAT"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Ikona průchodu"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Pravidlo brány firewall správy"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Pravidlo brány firewall protokolu RDP"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Pravidla brány firewall na webu"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Pravidlo brány firewall AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Odchozí pravidlo brány firewall"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Pravidlo brány firewall DNS"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Pravidlo brány firewall Intra-VNet"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Pravidlo brány firewall Odepřít"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktivace pravidla brány firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
