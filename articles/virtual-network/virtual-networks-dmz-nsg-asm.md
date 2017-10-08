---
title: "aaaAzure příklad DMZ – vytvoření jednoduché DMZ pomocí skupin Nsg | Microsoft Docs"
description: "Sestavení DMZ se skupinami zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 32a40a8dc7539c4c7293988e6c36e5e32ef11045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a><span data-ttu-id="9fb76-103">Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="9fb76-103">Example 1 – Build a simple DMZ using NSGs with classic PowerShell</span></span>
<span data-ttu-id="9fb76-104">[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="9fb76-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9fb76-105">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="9fb76-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="9fb76-106">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9fb76-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="9fb76-107">Tento příklad vytvoří primitivní hraniční sítě se čtyřmi servery Windows a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="9fb76-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="9fb76-108">Tento příklad popisuje každý hello relevantní prostředí PowerShell příkazy tooprovide podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="9fb76-108">This example describes each of hello relevant PowerShell commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="9fb76-109">K dispozici je také provozu scénář části tooprovide na podrobné podrobné jak provoz pokračuje prostřednictvím hello vrstev obrany ve hello hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="9fb76-109">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="9fb76-110">Nakonec v části odkazy hello je kompletní kód hello a instrukce toobuild tento tootest prostředí a experimentů pomocí různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="9fb76-110">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="9fb76-111">![Příchozí DMZ s NSG][1]</span><span class="sxs-lookup"><span data-stu-id="9fb76-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="9fb76-112">Popis prostředí</span><span class="sxs-lookup"><span data-stu-id="9fb76-112">Environment description</span></span>
<span data-ttu-id="9fb76-113">V tomto příkladu obsahuje předplatné hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="9fb76-113">In this example a subscription contains hello following resources:</span></span>

* <span data-ttu-id="9fb76-114">Dva cloudové služby: "FrontEnd001" a "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="9fb76-114">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="9fb76-115">Virtuální síť, "CorpNetwork" se dvěma podsítěmi; "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="9fb76-115">A Virtual Network, “CorpNetwork”, with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="9fb76-116">Skupinu zabezpečení sítě, která je použitá tooboth podsítě</span><span class="sxs-lookup"><span data-stu-id="9fb76-116">A Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="9fb76-117">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="9fb76-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="9fb76-118">Dva windows serverů, které představují servery back-end aplikace ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="9fb76-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="9fb76-119">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="9fb76-119">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="9fb76-120">V části odkazy hello je skript prostředí PowerShell, který sestaví většinu hello prostředí popsané v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="9fb76-120">In hello references section, there is a PowerShell script that builds most of hello environment described in this example.</span></span> <span data-ttu-id="9fb76-121">Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádějí hello ukázkový skript, nejsou popsané podrobně v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9fb76-121">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span> 

<span data-ttu-id="9fb76-122">toobuild hello prostředí;</span><span class="sxs-lookup"><span data-stu-id="9fb76-122">toobuild hello environment;</span></span>

1. <span data-ttu-id="9fb76-123">Uložit hello sítě konfigurační soubor xml v oddíle odkazy hello (aktualizovat název, umístění a IP adresy toomatch hello zadané scénář)</span><span class="sxs-lookup"><span data-stu-id="9fb76-123">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="9fb76-124">Aktualizace hello uživatelské proměnné ve hello skriptu toomatch hello prostředí hello skriptu je toobe spouštění (odběry, názvy služeb atd.)</span><span class="sxs-lookup"><span data-stu-id="9fb76-124">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc.)</span></span>
3. <span data-ttu-id="9fb76-125">Spuštění skriptu hello v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9fb76-125">Execute hello script in PowerShell</span></span>

>[!Note]
><span data-ttu-id="9fb76-126">oblast Hello označený ve hello skript prostředí PowerShell musí odpovídat hello oblast označený ve hello sítě konfigurační soubor xml.</span><span class="sxs-lookup"><span data-stu-id="9fb76-126">hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>
>
>

<span data-ttu-id="9fb76-127">Jakmile hello skript spustí úspěšně další volitelné kroky mohou být přijata, v části hello odkazy jsou dva skripty tooset hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="9fb76-127">Once hello script runs successfully additional optional steps may be taken, in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="9fb76-128">Hello následující části obsahují podrobný popis skupiny zabezpečení sítě a jak fungují v tomto příkladu pomocí klíče řádky skriptu prostředí PowerShell hello s návodem.</span><span class="sxs-lookup"><span data-stu-id="9fb76-128">hello following sections provide a detailed description of Network Security Groups and how they function for this example by walking through key lines of hello PowerShell script.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="9fb76-129">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="9fb76-129">Network Security Groups (NSG)</span></span>
<span data-ttu-id="9fb76-130">V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla.</span><span class="sxs-lookup"><span data-stu-id="9fb76-130">For this example, an NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="9fb76-131">Obecně platí musí nejprve vytvořit konkrétní pravidel "Povolit" a pak poslední hello více obecná pravidla "Deny".</span><span class="sxs-lookup"><span data-stu-id="9fb76-131">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="9fb76-132">Hello prioritou stanoví, která pravidla se vyhodnocují jako první.</span><span class="sxs-lookup"><span data-stu-id="9fb76-132">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="9fb76-133">Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla.</span><span class="sxs-lookup"><span data-stu-id="9fb76-133">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="9fb76-134">Pravidla NSG můžete použít buď v hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).</span><span class="sxs-lookup"><span data-stu-id="9fb76-134">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
> 
> 

<span data-ttu-id="9fb76-135">Deklarativně jsou pro příchozí provoz sestavuje hello následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="9fb76-135">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="9fb76-136">Interní DNS provoz (port 53) je povolený</span><span class="sxs-lookup"><span data-stu-id="9fb76-136">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="9fb76-137">Provoz protokolu RDP (portu 3389) z Internetu tooany hello virtuálních počítačů je povoleno.</span><span class="sxs-lookup"><span data-stu-id="9fb76-137">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="9fb76-138">Je povolen přenos HTTP (port 80) ze serveru tooweb Internet hello (IIS01)</span><span class="sxs-lookup"><span data-stu-id="9fb76-138">HTTP traffic (port 80) from hello Internet tooweb server (IIS01) is allowed</span></span>
4. <span data-ttu-id="9fb76-139">Jakýkoli přenos (všechny porty) z IIS01 tooAppVM1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="9fb76-139">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="9fb76-140">Jakýkoli přenos (všechny porty) z Internetu toohello hello celý virtuální síť (obě podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="9fb76-140">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="9fb76-141">Jakýkoli přenos (všechny porty) z hello front-endu podsíť toohello back-end podsítě byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="9fb76-141">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="9fb76-142">Pomocí těchto pravidel vázané tooeach podsíti, pokud požadavek HTTP byl příchozí z hello Internet toohello webový server, obě pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit.</span><span class="sxs-lookup"><span data-stu-id="9fb76-142">With these rules bound tooeach subnet, if an HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="9fb76-143">Proto hello požadavku HTTP bude mít možnost toohello webový server.</span><span class="sxs-lookup"><span data-stu-id="9fb76-143">Thus hello HTTP request would be allowed toohello web server.</span></span> <span data-ttu-id="9fb76-144">Pokud tento stejný provoz pokoušel tooreach hello DNS01 server, pravidlo 5 (Odepřít) bude, že hello první tooapply hello přenosů dat a nebude možné toopass toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="9fb76-144">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="9fb76-145">Pravidlo 6 (Odepřít) blokuje podsíť Frontend hello z rozhovoru toohello back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), této sady pravidel chrání síť back-end hello v případě, že by útočník ohrožení hello webovou aplikaci na hello front-endu, útočník hello mít omezený přístup toohello back-end "chráněné" sítě (jenom tooresources zveřejněné na serveru AppVM01 hello).</span><span class="sxs-lookup"><span data-stu-id="9fb76-145">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="9fb76-146">Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="9fb76-146">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="9fb76-147">V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="9fb76-147">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="9fb76-148">toolock dolů provoz v obou směrech uživatele definovaná směrování je povinný a je prozkoumali "Příklad 3" na hello [stránku osvědčené postupy zabezpečení hranic][HOME].</span><span class="sxs-lookup"><span data-stu-id="9fb76-148">toolock down traffic in both directions, User Defined Routing is required and is explored in “Example 3” on hello [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="9fb76-149">Každé pravidlo je podrobněji popsána takto (**Poznámka**: libovolnou položku v následujícím seznamu počínaje znak dolaru hello (například: $NSGName) je uživatelem definované proměnné ze skriptu hello v hello odkaz části tohoto dokumentu):</span><span class="sxs-lookup"><span data-stu-id="9fb76-149">Each rule is discussed in more detail as follows (**Note**: any item in hello following list beginning with a dollar sign (for example: $NSGName) is a user-defined variable from hello script in hello reference section of this document):</span></span>

1. <span data-ttu-id="9fb76-150">Skupina zabezpečení sítě musí být nejprve sestaven toohold hello pravidla:</span><span class="sxs-lookup"><span data-stu-id="9fb76-150">First a Network Security Group must be built toohold hello rules:</span></span>

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. <span data-ttu-id="9fb76-151">první pravidlo Hello v tomto příkladu umožňuje přenosů mezi všechny server DNS toohello interní sítě v podsíti hello back-end.</span><span class="sxs-lookup"><span data-stu-id="9fb76-151">hello first rule in this example allows DNS traffic between all internal networks toohello DNS server on hello backend subnet.</span></span> <span data-ttu-id="9fb76-152">pravidlo Hello má některé důležité parametry:</span><span class="sxs-lookup"><span data-stu-id="9fb76-152">hello rule has some important parameters:</span></span>
   
   * <span data-ttu-id="9fb76-153">"Typ" označuje, že ve směru toku provozu účinné toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="9fb76-153">“Type” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="9fb76-154">Směr Hello je z hlediska hello hello podsíť nebo virtuálního počítače (v závislosti na tom, kde je tato skupina NSG vázán).</span><span class="sxs-lookup"><span data-stu-id="9fb76-154">hello direction is from hello perspective of hello subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="9fb76-155">Proto pokud je typ "Příchozí" a provoz vstupující hello podsíť, hello pravidlo vztahuje a odchozího provozu z podsítě hello ovlivněn tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="9fb76-155">Thus if Type is “Inbound” and traffic is entering hello subnet, hello rule would apply and traffic leaving hello subnet would not be affected by this rule.</span></span>
   * <span data-ttu-id="9fb76-156">"Priority" Nastaví hello pořadí, ve kterém je přenosový tok vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="9fb76-156">“Priority” sets hello order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="9fb76-157">Hello nižší hello číslo hello vyšší hello prioritou.</span><span class="sxs-lookup"><span data-stu-id="9fb76-157">hello lower hello number hello higher hello priority.</span></span> <span data-ttu-id="9fb76-158">Když se pravidlo vztahuje tooa konkrétní přenosový tok, žádná další pravidla se zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="9fb76-158">When a rule applies tooa specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="9fb76-159">Proto pokud pravidlo s prioritou 1 umožňuje provoz a pravidlo s prioritou 2 odmítne provozu a obě pravidla použít tootraffic pak provoz hello bude mít možnost tooflow (vzhledem k tomu, že pravidlo 1 měl vyšší prioritu trvalo účinek a žádná další pravidla byly použity).</span><span class="sxs-lookup"><span data-stu-id="9fb76-159">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply tootraffic then hello traffic would be allowed tooflow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
   * <span data-ttu-id="9fb76-160">"Action" označuje, že pokud toto pravidlo přenosů blokovat nebo povolit.</span><span class="sxs-lookup"><span data-stu-id="9fb76-160">“Action” signifies if traffic affected by this rule is blocked or allowed.</span></span>

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. <span data-ttu-id="9fb76-161">Toto pravidlo umožňuje tooflow provoz protokolu RDP z hello internet toohello portu RDP na libovolném serveru v hello vázaný podsítě.</span><span class="sxs-lookup"><span data-stu-id="9fb76-161">This rule allows RDP traffic tooflow from hello internet toohello RDP port on any server on hello bound subnet.</span></span> <span data-ttu-id="9fb76-162">Toto pravidlo používá dva speciální typy předpon adres; "VIRTUAL_NETWORK" a "INTERNET".</span><span class="sxs-lookup"><span data-stu-id="9fb76-162">This rule uses two special types of address prefixes; “VIRTUAL_NETWORK” and “INTERNET.”</span></span> <span data-ttu-id="9fb76-163">Tyto značky se snadný způsob tooaddress vyšší kategorie předpon adres.</span><span class="sxs-lookup"><span data-stu-id="9fb76-163">These tags are an easy way tooaddress a larger category of address prefixes.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. <span data-ttu-id="9fb76-164">Toto pravidlo umožňuje příchozí internetové přenosy toohit hello webový server.</span><span class="sxs-lookup"><span data-stu-id="9fb76-164">This rule allows inbound internet traffic toohit hello web server.</span></span> <span data-ttu-id="9fb76-165">Toto pravidlo nedojde ke změně chování směrování hello.</span><span class="sxs-lookup"><span data-stu-id="9fb76-165">This rule does not change hello routing behavior.</span></span> <span data-ttu-id="9fb76-166">Hello pravidlo umožňuje pouze provoz určený pro IIS01 toopass.</span><span class="sxs-lookup"><span data-stu-id="9fb76-166">hello rule only allows traffic destined for IIS01 toopass.</span></span> <span data-ttu-id="9fb76-167">Takže pokud provoz z Internetu hello měl hello webový server jako svůj cíl toto pravidlo by se povolit a zastavit zpracování další pravidla.</span><span class="sxs-lookup"><span data-stu-id="9fb76-167">Thus if traffic from hello Internet had hello web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="9fb76-168">(V hello pravidla s důležitostí 140 všechny ostatní příchozí internetový provoz blokováno).</span><span class="sxs-lookup"><span data-stu-id="9fb76-168">(In hello rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="9fb76-169">Pokud máte pouze zpracování přenos HTTP, může být toto pravidlo další s omezeným přístupem tooonly povolit cílový Port 80.</span><span class="sxs-lookup"><span data-stu-id="9fb76-169">If you're only processing HTTP traffic, this rule could be further restricted tooonly allow Destination Port 80.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet too$VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. <span data-ttu-id="9fb76-170">Toto pravidlo umožňuje toopass provoz ze serveru IIS01 hello toohello AppVM01 serveru, novější pravidlo blokuje všechny ostatní přenosy tooBackend front-endu.</span><span class="sxs-lookup"><span data-stu-id="9fb76-170">This rule allows traffic toopass from hello IIS01 server toohello AppVM01 server, a later rule blocks all other Frontend tooBackend traffic.</span></span> <span data-ttu-id="9fb76-171">tooimprove, který toto pravidlo, pokud hello port se ví, že má být přidána.</span><span class="sxs-lookup"><span data-stu-id="9fb76-171">tooimprove this rule, if hello port is known that should be added.</span></span> <span data-ttu-id="9fb76-172">Například pokud server služby IIS hello je stiskne pouze SQL Server na AppVM01, rozsah cílových portů hello by mělo být změněno z "*" (Any) too1433 (hello port služby SQL) umožňuje menší prostor pro příchozí útok na AppVM01, by měla webová aplikace hello někdy dojít k ohrožení.</span><span class="sxs-lookup"><span data-stu-id="9fb76-172">For example, if hello IIS server is hitting only SQL Server on AppVM01, hello Destination Port Range should be changed from “*” (Any) too1433 (hello SQL port) thus allowing a smaller inbound attack surface on AppVM01 should hello web application ever be compromised.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] too$VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. <span data-ttu-id="9fb76-173">Toto pravidlo odmítne provoz z hello internetové tooany servery v síti hello.</span><span class="sxs-lookup"><span data-stu-id="9fb76-173">This rule denies traffic from hello internet tooany servers on hello network.</span></span> <span data-ttu-id="9fb76-174">S hello pravidla s důležitostí 110 a 120 hello efekt je tooallow pouze příchozí internetové přenosy toohello brány firewall a porty protokolu RDP na serverech a blokuje nic jiného.</span><span class="sxs-lookup"><span data-stu-id="9fb76-174">With hello rules at priority 110 and 120, hello effect is tooallow only inbound internet traffic toohello firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="9fb76-175">Toto pravidlo je "jistotu" pravidlo tooblock všechny neočekávané toky.</span><span class="sxs-lookup"><span data-stu-id="9fb76-175">This rule is a "fail-safe" rule tooblock all unexpected flows.</span></span>
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $VNetName VNet from hello Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. <span data-ttu-id="9fb76-176">poslední pravidlo Hello odmítne provoz z hello front-endu podsíť toohello back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="9fb76-176">hello final rule denies traffic from hello Frontend subnet toohello Backend subnet.</span></span> <span data-ttu-id="9fb76-177">Vzhledem k tomu, že toto pravidlo je pouze příchozí pravidlo, zpětné provoz je povolený (z back-end toohello hello front-endu).</span><span class="sxs-lookup"><span data-stu-id="9fb76-177">Since this rule is an Inbound only rule, reverse traffic is allowed (from hello Backend toohello Frontend).</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="9fb76-178">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="9fb76-178">Traffic scenarios</span></span>
#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="9fb76-179">(*Povolené*) internetového tooweb serveru</span><span class="sxs-lookup"><span data-stu-id="9fb76-179">(*Allowed*) Internet tooweb server</span></span>
1. <span data-ttu-id="9fb76-180">Internetu uživatel požádá o stránku HTTP z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="9fb76-180">An internet user requests an HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="9fb76-181">Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 směrem IIS01 (hello webový server)</span><span class="sxs-lookup"><span data-stu-id="9fb76-181">Cloud service passes traffic through open endpoint on port 80 towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="9fb76-182">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-182">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="9fb76-183">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-183">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="9fb76-184">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-184">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="9fb76-185">Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="9fb76-185">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="9fb76-186">Provoz dotkne interní IP adresu hello webového serveru IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="9fb76-186">Traffic hits internal IP address of hello web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="9fb76-187">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello</span><span class="sxs-lookup"><span data-stu-id="9fb76-187">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
6. <span data-ttu-id="9fb76-188">IIS01 hello systému SQL Server na AppVM01 vyzve k zadání informací</span><span class="sxs-lookup"><span data-stu-id="9fb76-188">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="9fb76-189">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti front-endu, provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="9fb76-189">Since there are no outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="9fb76-190">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-190">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="9fb76-191">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-191">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="9fb76-192">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-192">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="9fb76-193">Skupina NSG pravidla 3 (Internet tooFirewall) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-193">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="9fb76-194">Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="9fb76-194">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="9fb76-195">AppVM01 přijímá hello dotazu SQL a odpoví</span><span class="sxs-lookup"><span data-stu-id="9fb76-195">AppVM01 receives hello SQL Query and responds</span></span>
10. <span data-ttu-id="9fb76-196">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti hello back-end, je povoleno hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="9fb76-196">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
11. <span data-ttu-id="9fb76-197">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-197">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="9fb76-198">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="9fb76-198">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="9fb76-199">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.</span><span class="sxs-lookup"><span data-stu-id="9fb76-199">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
12. <span data-ttu-id="9fb76-200">server služby IIS Hello obdrží odpověď hello SQL a dokončí hello odpovědi HTTP a odešle toohello žadatele</span><span class="sxs-lookup"><span data-stu-id="9fb76-200">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requestor</span></span>
13. <span data-ttu-id="9fb76-201">Vzhledem k tomu, že neexistují žádná odchozí pravidla na podsíť Frontend hello hello odpovědi je povolen a hello internet uživatel obdrží webovou stránku hello požadovaný.</span><span class="sxs-lookup"><span data-stu-id="9fb76-201">Since there are no outbound rules on hello Frontend subnet hello response is allowed, and hello internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-toobackend"></a><span data-ttu-id="9fb76-202">(*Povolené*) toobackend protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="9fb76-202">(*Allowed*) RDP toobackend</span></span>
1. <span data-ttu-id="9fb76-203">Správce serveru na Internetu požadavky tooAppVM01 relace protokolu RDP na BackEnd001.CloudApp.Net:xxxxx kde xxxxx je číslo portu hello náhodně přiřadí tooAppVM01 protokolu RDP (portu přiřazené hello naleznete na hello portál Azure nebo pomocí prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="9fb76-203">Server Admin on internet requests RDP session tooAppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is hello randomly assigned port number for RDP tooAppVM01 (hello assigned port can be found on hello Azure portal or via PowerShell)</span></span>
2. <span data-ttu-id="9fb76-204">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-204">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="9fb76-205">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-205">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="9fb76-206">Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="9fb76-206">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
3. <span data-ttu-id="9fb76-207">Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="9fb76-207">With no outbound rules, default rules apply and return traffic is allowed</span></span>
4. <span data-ttu-id="9fb76-208">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="9fb76-208">RDP session is enabled</span></span>
5. <span data-ttu-id="9fb76-209">AppVM01 vyzve k zadání hello uživatelské jméno a heslo</span><span class="sxs-lookup"><span data-stu-id="9fb76-209">AppVM01 prompts for hello user name and password</span></span>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="9fb76-210">(*Povolené*) hledání DNS webového serveru na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="9fb76-210">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="9fb76-211">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale potřebuje tooresolve hello adresu.</span><span class="sxs-lookup"><span data-stu-id="9fb76-211">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="9fb76-212">Hello konfiguraci sítě pro virtuální síť seznamy hello DNS01 (10.0.2.4 v podsíti hello back-end) jako primární server DNS hello IIS01 odešle tooDNS01 požadavek DNS hello</span><span class="sxs-lookup"><span data-stu-id="9fb76-212">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="9fb76-213">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="9fb76-213">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="9fb76-214">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-214">Backend subnet begins inbound rule processing:</span></span>
   * <span data-ttu-id="9fb76-215">Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="9fb76-215">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="9fb76-216">DNS server obdrží požadavek hello</span><span class="sxs-lookup"><span data-stu-id="9fb76-216">DNS server receives hello request</span></span>
6. <span data-ttu-id="9fb76-217">DNS server nemá hello adresu do mezipaměti a požádá kořenový server DNS na hello Internetu</span><span class="sxs-lookup"><span data-stu-id="9fb76-217">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="9fb76-218">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="9fb76-218">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="9fb76-219">Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="9fb76-219">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="9fb76-220">DNS server ukládá do mezipaměti odpovědi hello a odpoví back tooIIS01 toohello úvodního požadavku</span><span class="sxs-lookup"><span data-stu-id="9fb76-220">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="9fb76-221">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="9fb76-221">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="9fb76-222">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-222">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="9fb76-223">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="9fb76-223">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="9fb76-224">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený</span><span class="sxs-lookup"><span data-stu-id="9fb76-224">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="9fb76-225">IIS01 obdrží odpověď hello z DNS01</span><span class="sxs-lookup"><span data-stu-id="9fb76-225">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="9fb76-226">(*Povolené*) přístup k souboru webového serveru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="9fb76-226">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="9fb76-227">IIS01 požádá o soubor na AppVM01</span><span class="sxs-lookup"><span data-stu-id="9fb76-227">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="9fb76-228">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="9fb76-228">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="9fb76-229">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-229">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="9fb76-230">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-230">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="9fb76-231">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-231">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="9fb76-232">Skupina NSG pravidla 3 (Internet tooIIS01) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-232">NSG Rule 3 (Internet tooIIS01) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="9fb76-233">Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="9fb76-233">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="9fb76-234">AppVM01 obdrží požadavek na hello a odpoví souboru (za předpokladu, že je autorizovaný přístup)</span><span class="sxs-lookup"><span data-stu-id="9fb76-234">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="9fb76-235">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti hello back-end, je povoleno hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="9fb76-235">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
6. <span data-ttu-id="9fb76-236">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-236">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="9fb76-237">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="9fb76-237">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
   2. <span data-ttu-id="9fb76-238">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.</span><span class="sxs-lookup"><span data-stu-id="9fb76-238">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="9fb76-239">server služby IIS Hello obdrží soubor hello</span><span class="sxs-lookup"><span data-stu-id="9fb76-239">hello IIS server receives hello file</span></span>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="9fb76-240">(*Byl odepřen*) toobackend webu</span><span class="sxs-lookup"><span data-stu-id="9fb76-240">(*Denied*) Web toobackend server</span></span>
1. <span data-ttu-id="9fb76-241">Uživatel s Internetu pokusí tooaccess souboru na AppVM01 prostřednictvím hello BackEnd001.CloudApp.Net služby</span><span class="sxs-lookup"><span data-stu-id="9fb76-241">An internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="9fb76-242">Vzhledem k tomu, že nejsou otevřené pro sdílené složky koncové body, tato komunikace by předat hello cloudové služby a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="9fb76-242">Since there are no endpoints open for file share, this traffic would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="9fb76-243">Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat pravidla NSG 5 (Internet tooVNet)</span><span class="sxs-lookup"><span data-stu-id="9fb76-243">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="9fb76-244">(*Byl odepřen*) hledání DNS pro Web na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="9fb76-244">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="9fb76-245">Uživatel s Internetu pokusí toolook až interní DNS záznam na DNS01 prostřednictvím hello BackEnd001.CloudApp.Net služby</span><span class="sxs-lookup"><span data-stu-id="9fb76-245">An internet user tries toolook up an internal DNS record on DNS01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="9fb76-246">Vzhledem k tomu, že nejsou otevřené pro DNS koncové body, tato komunikace by předat hello cloudové služby a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="9fb76-246">Since there are no endpoints open for DNS, this traffic would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="9fb76-247">Pokud z nějakého důvodu byly otevřené hello koncových bodů, pravidla NSG 5 (tooVNet Internetu) by blokovat tento provoz (Poznámka: Tento pravidlo 1 (DNS) nebude použít dvou důvodů, první zdrojové adresy hello je hello internet, toto pravidlo se vztahuje pouze na toohello virtuální místní síť jako hello také zdroje Toto pravidlo je pravidlo povolení, takže by nikdy zakážou provoz)</span><span class="sxs-lookup"><span data-stu-id="9fb76-247">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first hello source address is hello internet, this rule only applies toohello local VNet as hello source, also this rule is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-toosql-access-through-firewall"></a><span data-ttu-id="9fb76-248">(*Byl odepřen*) webové tooSQL přístup přes bránu firewall</span><span class="sxs-lookup"><span data-stu-id="9fb76-248">(*Denied*) Web tooSQL access through firewall</span></span>
1. <span data-ttu-id="9fb76-249">Uživatel s Internetu vyžaduje SQL data z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="9fb76-249">An internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="9fb76-250">Vzhledem k tomu, že jsou pro SQL otevřené žádné koncové body, tento provoz by předat hello cloudové služby a nebude dosáhnout hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="9fb76-250">Since there are no endpoints open for SQL, this traffic would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="9fb76-251">Pokud z nějakého důvodu byly otevřené koncových bodů, podsíť Frontend hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="9fb76-251">If endpoints were open for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="9fb76-252">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-252">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="9fb76-253">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="9fb76-253">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="9fb76-254">Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="9fb76-254">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="9fb76-255">Provoz dotkne interní IP adresu hello IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="9fb76-255">Traffic hits internal IP address of hello IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="9fb76-256">IIS01 nenaslouchá na portu 1433, takže žádný požadavek na toohello odpovědi</span><span class="sxs-lookup"><span data-stu-id="9fb76-256">IIS01 isn't listening on port 1433, so no response toohello request</span></span>

## <a name="conclusion"></a><span data-ttu-id="9fb76-257">Závěr</span><span class="sxs-lookup"><span data-stu-id="9fb76-257">Conclusion</span></span>
<span data-ttu-id="9fb76-258">V tomto příkladu je relativně jednoduché a splněny následující způsob izolace hello back-end podsíť z příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="9fb76-258">This example is a relatively simple and straight forward way of isolating hello back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="9fb76-259">Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].</span><span class="sxs-lookup"><span data-stu-id="9fb76-259">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="9fb76-260">Odkazy</span><span class="sxs-lookup"><span data-stu-id="9fb76-260">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="9fb76-261">Hlavní skript a síťové konfigurace</span><span class="sxs-lookup"><span data-stu-id="9fb76-261">Main script and network config</span></span>
<span data-ttu-id="9fb76-262">Uložte hello úplné skript v souboru skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9fb76-262">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="9fb76-263">Uložit hello konfiguraci sítě do souboru s názvem "NetworkConf1.xml."</span><span class="sxs-lookup"><span data-stu-id="9fb76-263">Save hello Network Config into a file named “NetworkConf1.xml.”</span></span>
<span data-ttu-id="9fb76-264">Umožňuje změnit proměnné uživatelem definované hello jako hello potřebné a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="9fb76-264">Modify hello user-defined variables as needed and run hello script.</span></span>

#### <a name="full-script"></a><span data-ttu-id="9fb76-265">Celý skript</span><span class="sxs-lookup"><span data-stu-id="9fb76-265">Full script</span></span>
<span data-ttu-id="9fb76-266">Tento skript bude na základě proměnných hello uživatelem definované;</span><span class="sxs-lookup"><span data-stu-id="9fb76-266">This script will, based on hello user-defined variables;</span></span>

1. <span data-ttu-id="9fb76-267">Připojit tooan předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="9fb76-267">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="9fb76-268">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="9fb76-268">Create a storage account</span></span>
3. <span data-ttu-id="9fb76-269">Vytvořit virtuální síť a dvě podsítě, jak jsou definovány v souboru konfigurace sítě hello</span><span class="sxs-lookup"><span data-stu-id="9fb76-269">Create a VNet and two subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="9fb76-270">Sestavení windows čtyři virtuální počítače serveru</span><span class="sxs-lookup"><span data-stu-id="9fb76-270">Build four windows server VMs</span></span>
5. <span data-ttu-id="9fb76-271">Konfigurace, včetně NSG:</span><span class="sxs-lookup"><span data-stu-id="9fb76-271">Configure NSG including:</span></span>
   * <span data-ttu-id="9fb76-272">Vytvořit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="9fb76-272">Creating an NSG</span></span>
   * <span data-ttu-id="9fb76-273">Sestavování s pravidly</span><span class="sxs-lookup"><span data-stu-id="9fb76-273">Populating it with rules</span></span>
   * <span data-ttu-id="9fb76-274">Vazba hello NSG toohello příslušné podsítě</span><span class="sxs-lookup"><span data-stu-id="9fb76-274">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="9fb76-275">Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.</span><span class="sxs-lookup"><span data-stu-id="9fb76-275">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9fb76-276">Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9fb76-276">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="9fb76-277">Pouze chybové zprávy červeně jsou příčinou problém.</span><span class="sxs-lookup"><span data-stu-id="9fb76-277">Only error messages in red are cause for concern.</span></span>
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on hello FrontEnd Subnet
   - Three Servers on hello BackEnd Subnet
   - Network Security Groups tooallow/deny traffic patterns as declared

  Before running script, ensure hello network configuration file is created in
  hello directory referenced by $NetworkConfigFile variable (or update the
  variable tooreflect hello path and file name of hello config file being used).

 .Notes
  Security requirements are different for each use case and can be addressed in a
  myriad of ways. Please be sure that any sensitive data or applications are behind
  hello appropriate layer(s) of protection. This script serves as an example of some
  of hello techniques that can be used, but should not be used for all scenarios. You
  are responsible tooassess your security needs and hello appropriate protections
  needed, and then effectively implement those protections.

  FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
   IIS01      - 10.0.1.5

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

# User-Defined Global Variables
  # These should be changes tooreflect your subscription and services
  # Invalid options will fail in hello validation section

  # Subscription Access Details
    $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

  # VM Account, Location, and Storage Details
    $LocalAdmin = "theAdmin"
    $DeploymentLocation = "Central US"
    $StorageAccountName = "vmstore02"

  # Service Details
    $FrontEndService = "FrontEnd001"
    $BackEndService = "BackEnd001"

  # Network Details
    $VNetName = "CorpNetwork"
    $FESubnet = "FrontEnd"
    $FEPrefix = "10.0.1.0/24"
    $BESubnet = "BackEnd"
    $BEPrefix = "10.0.2.0/24"
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: tooensure proper NSG Rule creation later in this script:
    #       - hello Web Server must be VM 0
    #       - hello AppVM1 Server must be VM 1
    #       - hello DNS server must be VM 3
    #
    #       Otherwise hello NSG rules in hello last section of this
    #       script will need toobe changed toomatch hello modified
    #       VM array numbers ($i) so hello NSG Rule IP addresses
    #       are aligned toohello associated VM IP addresses.

    # VM 0 - hello Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - hello First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - hello Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - hello DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
# Configuration past this point #
# ----------------------------- #    

  # Get your Azure accounts
    Add-AzureAccount
    Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
    Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

  # Create Storage Account
    If (Test-AzureName -Storage -Name $StorageAccountName) { 
        Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
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

If (Test-AzureName -Service -Name $FrontEndService) { 
    Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

If (Test-AzureName -Service -Name $BackEndService) { 
    Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

If (-Not (Test-Path $NetworkConfigFile)) { 
    Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation variable is correct and hello network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see hello above messages for more information." -ForegroundColor Red
    Return}
Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

# Create VNET
    Write-Host "Creating VNET" -ForegroundColor Cyan 
    Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

# Create Services
    Write-Host "Creating Services" -ForegroundColor Cyan
    New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
    New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

# Build VMs
    $i=0
    $VMName | Foreach {
        Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

  # Build hello NSG
    Write-Host "Building hello NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) too$($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
        -Protocol *

    # Assign hello NSG toohello Subnets
        Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

# Optional Post-script Manual Configuration
  # Install Test Web App (Run Post-Build Script on hello IIS Server)
  # Install Backend resource (Run Post-Build Script on hello AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a><span data-ttu-id="9fb76-278">Soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="9fb76-278">Network config file</span></span>
<span data-ttu-id="9fb76-279">Uložte tento soubor xml s aktualizované umístění a přidejte hello odkaz toothis soubor toohello $NetworkConfigFile proměnné v hello předcházející skriptu.</span><span class="sxs-lookup"><span data-stu-id="9fb76-279">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello preceding script.</span></span>

```XML
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
```

#### <a name="sample-application-scripts"></a><span data-ttu-id="9fb76-280">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="9fb76-280">Sample application scripts</span></span>
<span data-ttu-id="9fb76-281">Pokud chcete tooinstall ukázková aplikace pro toto a další příklady DMZ jeden bylo zadáno v hello následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="9fb76-281">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fb76-282">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9fb76-282">Next steps</span></span>
* <span data-ttu-id="9fb76-283">Aktualizace a uložte soubor XML</span><span class="sxs-lookup"><span data-stu-id="9fb76-283">Update and save XML file</span></span>
* <span data-ttu-id="9fb76-284">Spusťte prostředí hello hello PowerShell skriptu toobuild</span><span class="sxs-lookup"><span data-stu-id="9fb76-284">Run hello PowerShell script toobuild hello environment</span></span>
* <span data-ttu-id="9fb76-285">Nainstalujte hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="9fb76-285">Install hello sample application</span></span>
* <span data-ttu-id="9fb76-286">Testování různé přenosové toky prostřednictvím této hraniční sítě</span><span class="sxs-lookup"><span data-stu-id="9fb76-286">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Příchozí DMZ s NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

