---
title: "Příklad Azure DMZ – vytvoření jednoduché DMZ pomocí skupin Nsg | Microsoft Docs"
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
ms.openlocfilehash: ed172d552e1e4c9ee27c58abcd7ad2d98df21579
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a><span data-ttu-id="15b7d-103">Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="15b7d-103">Example 1 – Build a simple DMZ using NSGs with classic PowerShell</span></span>
<span data-ttu-id="15b7d-104">[Návrat na stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="15b7d-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="15b7d-105">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="15b7d-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="15b7d-106">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="15b7d-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="15b7d-107">Tento příklad vytvoří primitivní hraniční sítě se čtyřmi servery Windows a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="15b7d-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="15b7d-108">Tento příklad popisuje všechny relevantní příkazy prostředí PowerShell, zadejte podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="15b7d-108">This example describes each of the relevant PowerShell commands to provide a deeper understanding of each step.</span></span> <span data-ttu-id="15b7d-109">Je také části provoz scénář zajistit podrobné krok za krokem, jak se provoz pokračuje prostřednictvím vrstev obrany v hraniční síti.</span><span class="sxs-lookup"><span data-stu-id="15b7d-109">There is also a Traffic Scenario section to provide an in-depth step-by-step how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="15b7d-110">Nakonec v odkazy na části je kompletní kód a pokyny k vytvoření tohoto prostředí pro testování a experimentovat s různými scénáři.</span><span class="sxs-lookup"><span data-stu-id="15b7d-110">Finally, in the references section is the complete code and instruction to build this environment to test and experiment with various scenarios.</span></span> 

<span data-ttu-id="15b7d-111">![Příchozí DMZ s NSG][1]</span><span class="sxs-lookup"><span data-stu-id="15b7d-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="15b7d-112">Popis prostředí</span><span class="sxs-lookup"><span data-stu-id="15b7d-112">Environment description</span></span>
<span data-ttu-id="15b7d-113">V tomto příkladu obsahuje odběru v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="15b7d-113">In this example a subscription contains the following resources:</span></span>

* <span data-ttu-id="15b7d-114">Dva cloudové služby: "FrontEnd001" a "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="15b7d-114">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="15b7d-115">Virtuální síť, "CorpNetwork" se dvěma podsítěmi; "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="15b7d-115">A Virtual Network, “CorpNetwork”, with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="15b7d-116">Skupinu zabezpečení sítě, který se použije pro obě podsítě</span><span class="sxs-lookup"><span data-stu-id="15b7d-116">A Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="15b7d-117">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="15b7d-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="15b7d-118">Dva windows serverů, které představují servery back-end aplikace ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="15b7d-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="15b7d-119">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="15b7d-119">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="15b7d-120">V části odkazy je skript prostředí PowerShell, který sestaví většinu prostředí popsané v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="15b7d-120">In the references section, there is a PowerShell script that builds most of the environment described in this example.</span></span> <span data-ttu-id="15b7d-121">Vytváření virtuálních počítačů a virtuálních sítí, i když provádějí ukázkový skript, nejsou podrobně popsané v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="15b7d-121">Building the VMs and Virtual Networks, although are done by the example script, are not described in detail in this document.</span></span> 

<span data-ttu-id="15b7d-122">K vytvoření prostředí;</span><span class="sxs-lookup"><span data-stu-id="15b7d-122">To build the environment;</span></span>

1. <span data-ttu-id="15b7d-123">Uložte soubor xml konfigurace sítě v oddíle odkazy (aktualizovat název, umístění a IP adresy, které odpovídají danému scénáři)</span><span class="sxs-lookup"><span data-stu-id="15b7d-123">Save the network config xml file included in the references section (updated with names, location, and IP addresses to match the given scenario)</span></span>
2. <span data-ttu-id="15b7d-124">Aktualizace uživatelské proměnné ve skriptu tak, aby odpovídaly prostředí, ve kterém je skript ke spouštění (odběry, názvy služeb atd.)</span><span class="sxs-lookup"><span data-stu-id="15b7d-124">Update the user variables in the script to match the environment the script is to be run against (subscriptions, service names, etc.)</span></span>
3. <span data-ttu-id="15b7d-125">Spusťte skript v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="15b7d-125">Execute the script in PowerShell</span></span>

>[!Note]
><span data-ttu-id="15b7d-126">Oblast označený ve skriptu PowerShell musí odpovídat oblasti označeny v souboru xml konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="15b7d-126">The region signified in the PowerShell script must match the region signified in the network configuration xml file.</span></span>
>
>

<span data-ttu-id="15b7d-127">Jakmile se skript spustí úspěšně další volitelné kroky mohou být přijata, v části odkazy jsou dva skripty, nastavení webového serveru a aplikačního serveru s jednoduchou webovou aplikaci umožňující testování s touto konfigurací hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="15b7d-127">Once the script runs successfully additional optional steps may be taken, in the references section are two scripts to set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span>

<span data-ttu-id="15b7d-128">V následujících částech najdete podrobný popis skupiny zabezpečení sítě a jak fungují v tomto příkladu pomocí klíče řádky skriptu prostředí PowerShell s návodem.</span><span class="sxs-lookup"><span data-stu-id="15b7d-128">The following sections provide a detailed description of Network Security Groups and how they function for this example by walking through key lines of the PowerShell script.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="15b7d-129">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="15b7d-129">Network Security Groups (NSG)</span></span>
<span data-ttu-id="15b7d-130">V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla.</span><span class="sxs-lookup"><span data-stu-id="15b7d-130">For this example, an NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="15b7d-131">Obecně řečeno měli byste vytvořit konkrétní pravidel "Povolit" nejprve a pak více obecná pravidla "Deny" poslední.</span><span class="sxs-lookup"><span data-stu-id="15b7d-131">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="15b7d-132">Přiřazené priority určují, které jsou pravidla vyhodnocena první.</span><span class="sxs-lookup"><span data-stu-id="15b7d-132">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="15b7d-133">Jakmile provoz nenajde Pokud chcete použít pro konkrétní pravidlo, jsou vyhodnotit žádná další pravidla.</span><span class="sxs-lookup"><span data-stu-id="15b7d-133">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="15b7d-134">Pravidla NSG můžete použít buď v příchozí nebo odchozí směr (z hlediska podsítě).</span><span class="sxs-lookup"><span data-stu-id="15b7d-134">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
> 
> 

<span data-ttu-id="15b7d-135">Následující pravidla deklarativně, se budou vytvářeny pro příchozí provoz:</span><span class="sxs-lookup"><span data-stu-id="15b7d-135">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="15b7d-136">Interní DNS provoz (port 53) je povolený</span><span class="sxs-lookup"><span data-stu-id="15b7d-136">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="15b7d-137">Provoz protokolu RDP (portu 3389) z Internetu do všech virtuálních počítačů je povoleno.</span><span class="sxs-lookup"><span data-stu-id="15b7d-137">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="15b7d-138">Je povolen přenos HTTP (port 80) z Internetu webový server (IIS01)</span><span class="sxs-lookup"><span data-stu-id="15b7d-138">HTTP traffic (port 80) from the Internet to web server (IIS01) is allowed</span></span>
4. <span data-ttu-id="15b7d-139">Všechny přenosy (všechny porty) z IIS01 na AppVM1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="15b7d-139">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="15b7d-140">Přenosy dat (všechny porty) z Internetu pro celou virtuální síť (obě podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="15b7d-140">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="15b7d-141">Přenosy dat (všechny porty) z podsítě front-endu do podsítě back-end byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="15b7d-141">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="15b7d-142">Pomocí těchto pravidel vázána na každou podsíť, pokud požadavek HTTP byl příchozí z Internetu webový server, jak pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit.</span><span class="sxs-lookup"><span data-stu-id="15b7d-142">With these rules bound to each subnet, if an HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="15b7d-143">Proto bude mít možnost požadavku HTTP k webovému serveru.</span><span class="sxs-lookup"><span data-stu-id="15b7d-143">Thus the HTTP request would be allowed to the web server.</span></span> <span data-ttu-id="15b7d-144">Pokud tento stejný provoz se pokusil připojit k serveru DNS01, pravidlo 5 (Odepřít) bude první použití a přenos nebude možné předat serveru.</span><span class="sxs-lookup"><span data-stu-id="15b7d-144">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="15b7d-145">Pravidlo 6 (Odepřít) blokuje podsíť Frontend z rozhovoru s back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), této sady pravidel chrání síť back-end v případě ohrožení útočník webové aplikace na Frontendový, útočník by mít omezený přístup k back-end "chráněná" Síťová (pouze pro prostředky zveřejněné na serveru AppVM01).</span><span class="sxs-lookup"><span data-stu-id="15b7d-145">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="15b7d-146">Je výchozí odchozí pravidlo, které umožňuje přenos se k Internetu.</span><span class="sxs-lookup"><span data-stu-id="15b7d-146">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="15b7d-147">V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="15b7d-147">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="15b7d-148">Zamknout provoz v obou směrech, směrování definovaného uživatele je povinná a je prozkoumali "Příklad 3" na [stránku osvědčené postupy zabezpečení hranic][HOME].</span><span class="sxs-lookup"><span data-stu-id="15b7d-148">To lock down traffic in both directions, User Defined Routing is required and is explored in “Example 3” on the [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="15b7d-149">Každé pravidlo je podrobněji popsána takto (**Poznámka**: libovolnou položku v následujícím seznamu počínaje znak dolaru (například: $NSGName) je uživatelské proměnné ve skriptu v části odkaz na tohoto dokumentu):</span><span class="sxs-lookup"><span data-stu-id="15b7d-149">Each rule is discussed in more detail as follows (**Note**: any item in the following list beginning with a dollar sign (for example: $NSGName) is a user-defined variable from the script in the reference section of this document):</span></span>

1. <span data-ttu-id="15b7d-150">Skupina zabezpečení sítě musí nejprve vytvořeny tak, aby udržení pravidla:</span><span class="sxs-lookup"><span data-stu-id="15b7d-150">First a Network Security Group must be built to hold the rules:</span></span>

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. <span data-ttu-id="15b7d-151">První pravidlo v tomto příkladu umožňuje přenosů mezi všechny interní sítě na server DNS v podsíti back-end.</span><span class="sxs-lookup"><span data-stu-id="15b7d-151">The first rule in this example allows DNS traffic between all internal networks to the DNS server on the backend subnet.</span></span> <span data-ttu-id="15b7d-152">Toto pravidlo má některé důležité parametry:</span><span class="sxs-lookup"><span data-stu-id="15b7d-152">The rule has some important parameters:</span></span>
   
   * <span data-ttu-id="15b7d-153">"Typ" označuje, že ve směru toku provozu účinné toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="15b7d-153">“Type” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="15b7d-154">Směr je z hlediska podsíť nebo virtuálního počítače (v závislosti na tom, kde je tato skupina NSG vázán).</span><span class="sxs-lookup"><span data-stu-id="15b7d-154">The direction is from the perspective of the subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="15b7d-155">Proto pokud je typ "Příchozí" a provoz vstupující podsíť, pravidlo vztahuje a odchozího provozu z podsítě ovlivněn tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="15b7d-155">Thus if Type is “Inbound” and traffic is entering the subnet, the rule would apply and traffic leaving the subnet would not be affected by this rule.</span></span>
   * <span data-ttu-id="15b7d-156">"Priority" Nastaví pořadí, ve kterém je vyhodnocena tok přenosů.</span><span class="sxs-lookup"><span data-stu-id="15b7d-156">“Priority” sets the order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="15b7d-157">Nižší počet tím vyšší je priorita.</span><span class="sxs-lookup"><span data-stu-id="15b7d-157">The lower the number the higher the priority.</span></span> <span data-ttu-id="15b7d-158">Když se pravidlo vztahuje na konkrétní přenosový tok, žádná další pravidla se zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="15b7d-158">When a rule applies to a specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="15b7d-159">Proto pokud pravidlo s prioritou 1 umožňuje provoz a pravidlo s prioritou 2 odmítne provozu a použít obě pravidla pro provoz pak provoz se bude moct toku (vzhledem k tomu, že pravidlo 1 měl vyšší prioritu trvalo účinek a žádná další pravidla byly použity).</span><span class="sxs-lookup"><span data-stu-id="15b7d-159">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply to traffic then the traffic would be allowed to flow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
   * <span data-ttu-id="15b7d-160">"Action" označuje, že pokud toto pravidlo přenosů blokovat nebo povolit.</span><span class="sxs-lookup"><span data-stu-id="15b7d-160">“Action” signifies if traffic affected by this rule is blocked or allowed.</span></span>

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. <span data-ttu-id="15b7d-161">Toto pravidlo umožňuje provoz protokolu RDP, které jsou předávány z Internetu k portu RDP na libovolném serveru v vázané podsíti.</span><span class="sxs-lookup"><span data-stu-id="15b7d-161">This rule allows RDP traffic to flow from the internet to the RDP port on any server on the bound subnet.</span></span> <span data-ttu-id="15b7d-162">Toto pravidlo používá dva speciální typy předpon adres; "VIRTUAL_NETWORK" a "INTERNET".</span><span class="sxs-lookup"><span data-stu-id="15b7d-162">This rule uses two special types of address prefixes; “VIRTUAL_NETWORK” and “INTERNET.”</span></span> <span data-ttu-id="15b7d-163">Tyto značky jsou snadný způsob, jak vyřešit vyšší kategorie předpon adres.</span><span class="sxs-lookup"><span data-stu-id="15b7d-163">These tags are an easy way to address a larger category of address prefixes.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. <span data-ttu-id="15b7d-164">Toto pravidlo umožňuje příchozí internetové přenosy narazila na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="15b7d-164">This rule allows inbound internet traffic to hit the web server.</span></span> <span data-ttu-id="15b7d-165">Toto pravidlo nedojde ke změně chování směrování.</span><span class="sxs-lookup"><span data-stu-id="15b7d-165">This rule does not change the routing behavior.</span></span> <span data-ttu-id="15b7d-166">Toto pravidlo umožňuje pouze provoz určený pro IIS01 předat.</span><span class="sxs-lookup"><span data-stu-id="15b7d-166">The rule only allows traffic destined for IIS01 to pass.</span></span> <span data-ttu-id="15b7d-167">Takže pokud provoz z Internetu měl webový server jako svůj cíl toto pravidlo by se povolit a zastavit zpracování další pravidla.</span><span class="sxs-lookup"><span data-stu-id="15b7d-167">Thus if traffic from the Internet had the web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="15b7d-168">(V pravidla s důležitostí 140 všechny ostatní příchozí internetový provoz blokováno).</span><span class="sxs-lookup"><span data-stu-id="15b7d-168">(In the rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="15b7d-169">Pokud máte pouze zpracování přenos HTTP, může být toto pravidlo další omezena a Povolit jenom cílový Port 80.</span><span class="sxs-lookup"><span data-stu-id="15b7d-169">If you're only processing HTTP traffic, this rule could be further restricted to only allow Destination Port 80.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. <span data-ttu-id="15b7d-170">Toto pravidlo umožňuje přenos dat ze serveru IIS01 k serveru AppVM01 novější bloky pravidel všechny front-end pro provoz back-end.</span><span class="sxs-lookup"><span data-stu-id="15b7d-170">This rule allows traffic to pass from the IIS01 server to the AppVM01 server, a later rule blocks all other Frontend to Backend traffic.</span></span> <span data-ttu-id="15b7d-171">Aby se zlepšil toto pravidlo, pokud je znám port, který má být přidána.</span><span class="sxs-lookup"><span data-stu-id="15b7d-171">To improve this rule, if the port is known that should be added.</span></span> <span data-ttu-id="15b7d-172">Například pokud server služby IIS je stiskne pouze SQL Server na AppVM01, rozsah cílových portů by mělo být změněno z "*" (Any) 1433 (SQL port), což umožňuje menší prostor pro příchozí útok na AppVM01 by měl webové aplikace někdy dojít k ohrožení.</span><span class="sxs-lookup"><span data-stu-id="15b7d-172">For example, if the IIS server is hitting only SQL Server on AppVM01, the Destination Port Range should be changed from “*” (Any) to 1433 (the SQL port) thus allowing a smaller inbound attack surface on AppVM01 should the web application ever be compromised.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. <span data-ttu-id="15b7d-173">Toto pravidlo na všechny servery v síti odmítne přenosy z Internetu.</span><span class="sxs-lookup"><span data-stu-id="15b7d-173">This rule denies traffic from the internet to any servers on the network.</span></span> <span data-ttu-id="15b7d-174">Pravidla s důležitostí 110 a 120 účinek je umožnit pouze příchozí internetové přenosy pro brány firewall a porty protokolu RDP na serverech a bloky nic jiného.</span><span class="sxs-lookup"><span data-stu-id="15b7d-174">With the rules at priority 110 and 120, the effect is to allow only inbound internet traffic to the firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="15b7d-175">Toto pravidlo je "pohotovostního" pravidlo pro zablokování všechny neočekávané toky.</span><span class="sxs-lookup"><span data-stu-id="15b7d-175">This rule is a "fail-safe" rule to block all unexpected flows.</span></span>
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $VNetName VNet from the Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. <span data-ttu-id="15b7d-176">Poslední pravidlo odmítne provozu z podsítě front-endu do podsítě back-end.</span><span class="sxs-lookup"><span data-stu-id="15b7d-176">The final rule denies traffic from the Frontend subnet to the Backend subnet.</span></span> <span data-ttu-id="15b7d-177">Vzhledem k tomu, že toto pravidlo je pouze příchozí pravidlo, zpětné provoz je povolený (z back-end na front-endu).</span><span class="sxs-lookup"><span data-stu-id="15b7d-177">Since this rule is an Inbound only rule, reverse traffic is allowed (from the Backend to the Frontend).</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="15b7d-178">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="15b7d-178">Traffic scenarios</span></span>
#### <a name="allowed-internet-to-web-server"></a><span data-ttu-id="15b7d-179">(*Povolené*) Internet na webový server</span><span class="sxs-lookup"><span data-stu-id="15b7d-179">(*Allowed*) Internet to web server</span></span>
1. <span data-ttu-id="15b7d-180">Internetu uživatel požádá o stránku HTTP z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="15b7d-180">An internet user requests an HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="15b7d-181">Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 směrem IIS01 (webový server)</span><span class="sxs-lookup"><span data-stu-id="15b7d-181">Cloud service passes traffic through open endpoint on port 80 towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="15b7d-182">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-182">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="15b7d-183">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="15b7d-183">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="15b7d-184">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="15b7d-184">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="15b7d-185">Použít NSG pravidla 3 (Internet k IIS01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="15b7d-185">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="15b7d-186">Provoz dotkne interní IP adresu serveru webového IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="15b7d-186">Traffic hits internal IP address of the web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="15b7d-187">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku</span><span class="sxs-lookup"><span data-stu-id="15b7d-187">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
6. <span data-ttu-id="15b7d-188">IIS01 systému SQL Server na AppVM01 vyzve k zadání informací</span><span class="sxs-lookup"><span data-stu-id="15b7d-188">IIS01 asks the SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="15b7d-189">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti front-endu, provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="15b7d-189">Since there are no outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="15b7d-190">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-190">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="15b7d-191">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="15b7d-191">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="15b7d-192">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="15b7d-192">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="15b7d-193">Není použít, přejděte k další pravidla NSG pravidla 3 (Internet do brány Firewall)</span><span class="sxs-lookup"><span data-stu-id="15b7d-193">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="15b7d-194">Skupina NSG pravidla 4 použít (IIS01 k AppVM01), provoz je povolený, zastavte zpracování pravidla</span><span class="sxs-lookup"><span data-stu-id="15b7d-194">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="15b7d-195">AppVM01 přijme příkaz jazyka SQL a odpovídá</span><span class="sxs-lookup"><span data-stu-id="15b7d-195">AppVM01 receives the SQL Query and responds</span></span>
10. <span data-ttu-id="15b7d-196">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti back-end, je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="15b7d-196">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
11. <span data-ttu-id="15b7d-197">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-197">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="15b7d-198">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="15b7d-198">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="15b7d-199">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povolen.</span><span class="sxs-lookup"><span data-stu-id="15b7d-199">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
12. <span data-ttu-id="15b7d-200">Server služby IIS obdrží odpověď SQL a dokončení odpovědi HTTP a odešle do žadatel</span><span class="sxs-lookup"><span data-stu-id="15b7d-200">The IIS server receives the SQL response and completes the HTTP response and sends to the requestor</span></span>
13. <span data-ttu-id="15b7d-201">Vzhledem k tomu, že neexistují žádná odchozí pravidla Internetu uživatel obdrží požadovanou webovou stránku a na podsíť Frontend odpovědi povoleny.</span><span class="sxs-lookup"><span data-stu-id="15b7d-201">Since there are no outbound rules on the Frontend subnet the response is allowed, and the internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-backend"></a><span data-ttu-id="15b7d-202">(*Povolené*) protokolu RDP na back-end</span><span class="sxs-lookup"><span data-stu-id="15b7d-202">(*Allowed*) RDP to backend</span></span>
1. <span data-ttu-id="15b7d-203">Správce serveru na Internetu požadavky protokolu RDP relace AppVM01 na BackEnd001.CloudApp.Net:xxxxx kde xxxxx je číslo náhodně přidělenému portu pro protokol RDP na AppVM01 (přidělenému portu naleznete na portálu Azure nebo pomocí prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="15b7d-203">Server Admin on internet requests RDP session to AppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is the randomly assigned port number for RDP to AppVM01 (the assigned port can be found on the Azure portal or via PowerShell)</span></span>
2. <span data-ttu-id="15b7d-204">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-204">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="15b7d-205">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="15b7d-205">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="15b7d-206">Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="15b7d-206">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
3. <span data-ttu-id="15b7d-207">Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="15b7d-207">With no outbound rules, default rules apply and return traffic is allowed</span></span>
4. <span data-ttu-id="15b7d-208">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="15b7d-208">RDP session is enabled</span></span>
5. <span data-ttu-id="15b7d-209">AppVM01 vyzve k zadání uživatelského jména a hesla</span><span class="sxs-lookup"><span data-stu-id="15b7d-209">AppVM01 prompts for the user name and password</span></span>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="15b7d-210">(*Povolené*) hledání DNS webového serveru na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="15b7d-210">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="15b7d-211">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale musí pro překlad adres.</span><span class="sxs-lookup"><span data-stu-id="15b7d-211">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="15b7d-212">Konfigurace sítě pro virtuální síť seznamy DNS01 (10.0.2.4 v podsíti back-end) jako primární server DNS, IIS01 odešle žádost DNS do DNS01</span><span class="sxs-lookup"><span data-stu-id="15b7d-212">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="15b7d-213">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="15b7d-213">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="15b7d-214">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-214">Backend subnet begins inbound rule processing:</span></span>
   * <span data-ttu-id="15b7d-215">Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="15b7d-215">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="15b7d-216">DNS server obdrží požadavek</span><span class="sxs-lookup"><span data-stu-id="15b7d-216">DNS server receives the request</span></span>
6. <span data-ttu-id="15b7d-217">DNS server nemá adresu do mezipaměti a požádá kořenový server DNS na Internetu</span><span class="sxs-lookup"><span data-stu-id="15b7d-217">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="15b7d-218">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="15b7d-218">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="15b7d-219">Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="15b7d-219">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="15b7d-220">DNS server odpověď do mezipaměti a reaguje na počáteční požadavek zpět na IIS01</span><span class="sxs-lookup"><span data-stu-id="15b7d-220">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="15b7d-221">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="15b7d-221">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="15b7d-222">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-222">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="15b7d-223">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="15b7d-223">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="15b7d-224">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povoleno</span><span class="sxs-lookup"><span data-stu-id="15b7d-224">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="15b7d-225">IIS01 obdrží odpověď od DNS01</span><span class="sxs-lookup"><span data-stu-id="15b7d-225">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="15b7d-226">(*Povolené*) přístup k souboru webového serveru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="15b7d-226">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="15b7d-227">IIS01 požádá o soubor na AppVM01</span><span class="sxs-lookup"><span data-stu-id="15b7d-227">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="15b7d-228">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="15b7d-228">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="15b7d-229">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-229">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="15b7d-230">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="15b7d-230">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="15b7d-231">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="15b7d-231">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="15b7d-232">Není použít, přejděte k další pravidla NSG pravidla 3 (Internet k IIS01)</span><span class="sxs-lookup"><span data-stu-id="15b7d-232">NSG Rule 3 (Internet to IIS01) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="15b7d-233">Skupina NSG pravidla 4 použít (IIS01 k AppVM01), provoz je povolený, zastavte zpracování pravidla</span><span class="sxs-lookup"><span data-stu-id="15b7d-233">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="15b7d-234">AppVM01 obdrží požadavek a odpoví souboru (za předpokladu, že je autorizovaný přístup)</span><span class="sxs-lookup"><span data-stu-id="15b7d-234">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="15b7d-235">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti back-end, je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="15b7d-235">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
6. <span data-ttu-id="15b7d-236">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-236">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="15b7d-237">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="15b7d-237">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
   2. <span data-ttu-id="15b7d-238">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povolen.</span><span class="sxs-lookup"><span data-stu-id="15b7d-238">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="15b7d-239">Server služby IIS obdrží soubor</span><span class="sxs-lookup"><span data-stu-id="15b7d-239">The IIS server receives the file</span></span>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="15b7d-240">(*Byl odepřen*) webové back-end server</span><span class="sxs-lookup"><span data-stu-id="15b7d-240">(*Denied*) Web to backend server</span></span>
1. <span data-ttu-id="15b7d-241">Uživatel s Internetu pokusí o přístup k souboru na AppVM01 prostřednictvím služby BackEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="15b7d-241">An internet user tries to access a file on AppVM01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="15b7d-242">Vzhledem k tomu, že nejsou otevřené pro sdílené složky koncové body, tato komunikace by předat cloudové služby a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="15b7d-242">Since there are no endpoints open for file share, this traffic would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="15b7d-243">Pokud z nějakého důvodu byly otevřené koncových bodů, by tento provoz blokovat pravidla NSG 5 (Internet do virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="15b7d-243">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="15b7d-244">(*Byl odepřen*) hledání DNS pro Web na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="15b7d-244">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="15b7d-245">Uživatelé Internetu pokusí vyhledat interní DNS záznam na DNS01 prostřednictvím služby BackEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="15b7d-245">An internet user tries to look up an internal DNS record on DNS01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="15b7d-246">Vzhledem k tomu, že nejsou otevřené pro DNS koncové body, tato komunikace by předat cloudové služby a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="15b7d-246">Since there are no endpoints open for DNS, this traffic would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="15b7d-247">Pokud z nějakého důvodu byly otevřené koncových bodů, pravidla NSG 5 (Internet do virtuální sítě) by blokovat tento provoz (Poznámka: Tento pravidlo 1 (DNS) nebude použít dvou důvodů, nejprve zdrojové adresy je Internetu, toto pravidlo se vztahuje pouze na místní virtuální síť jako zdroj, také toto pravidlo je pravidlo povolení, takže by nikdy zakážou provoz)</span><span class="sxs-lookup"><span data-stu-id="15b7d-247">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first the source address is the internet, this rule only applies to the local VNet as the source, also this rule is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-to-sql-access-through-firewall"></a><span data-ttu-id="15b7d-248">(*Byl odepřen*) webový přístup SQL pomocí brány firewall</span><span class="sxs-lookup"><span data-stu-id="15b7d-248">(*Denied*) Web to SQL access through firewall</span></span>
1. <span data-ttu-id="15b7d-249">Uživatel s Internetu vyžaduje SQL data z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="15b7d-249">An internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="15b7d-250">Vzhledem k tomu, že jsou pro SQL otevřené žádné koncové body, tento provoz by předat cloudové služby a nebude kontaktovat bránu firewall</span><span class="sxs-lookup"><span data-stu-id="15b7d-250">Since there are no endpoints open for SQL, this traffic would not pass the Cloud Service and wouldn’t reach the firewall</span></span>
3. <span data-ttu-id="15b7d-251">Pokud z nějakého důvodu byly otevřené koncových bodů, podsíť Frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="15b7d-251">If endpoints were open for some reason, the Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="15b7d-252">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="15b7d-252">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="15b7d-253">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="15b7d-253">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="15b7d-254">Použít NSG pravidla 3 (Internet k IIS01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="15b7d-254">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="15b7d-255">Provoz dotkne interní IP adresu IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="15b7d-255">Traffic hits internal IP address of the IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="15b7d-256">IIS01 nenaslouchá na portu 1433, takže žádná odpověď na žádost</span><span class="sxs-lookup"><span data-stu-id="15b7d-256">IIS01 isn't listening on port 1433, so no response to the request</span></span>

## <a name="conclusion"></a><span data-ttu-id="15b7d-257">Závěr</span><span class="sxs-lookup"><span data-stu-id="15b7d-257">Conclusion</span></span>
<span data-ttu-id="15b7d-258">V tomto příkladu je relativně jednoduché a splněny následující způsob izolace back-end podsíť z příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="15b7d-258">This example is a relatively simple and straight forward way of isolating the back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="15b7d-259">Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].</span><span class="sxs-lookup"><span data-stu-id="15b7d-259">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="15b7d-260">Odkazy</span><span class="sxs-lookup"><span data-stu-id="15b7d-260">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="15b7d-261">Hlavní skript a síťové konfigurace</span><span class="sxs-lookup"><span data-stu-id="15b7d-261">Main script and network config</span></span>
<span data-ttu-id="15b7d-262">Uložte úplné skript v souboru skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="15b7d-262">Save the Full Script in a PowerShell script file.</span></span> <span data-ttu-id="15b7d-263">Uložte konfiguraci sítě do souboru s názvem "NetworkConf1.xml."</span><span class="sxs-lookup"><span data-stu-id="15b7d-263">Save the Network Config into a file named “NetworkConf1.xml.”</span></span>
<span data-ttu-id="15b7d-264">Uživatelem definované proměnné podle potřeby upravte a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="15b7d-264">Modify the user-defined variables as needed and run the script.</span></span>

#### <a name="full-script"></a><span data-ttu-id="15b7d-265">Úplné skriptu</span><span class="sxs-lookup"><span data-stu-id="15b7d-265">Full script</span></span>
<span data-ttu-id="15b7d-266">Tento skript bude na základě proměnných uživatelem definované;</span><span class="sxs-lookup"><span data-stu-id="15b7d-266">This script will, based on the user-defined variables;</span></span>

1. <span data-ttu-id="15b7d-267">Připojení k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="15b7d-267">Connect to an Azure subscription</span></span>
2. <span data-ttu-id="15b7d-268">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="15b7d-268">Create a storage account</span></span>
3. <span data-ttu-id="15b7d-269">Vytvořit virtuální síť a dvě podsítě, jak jsou definovány v souboru konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="15b7d-269">Create a VNet and two subnets as defined in the Network Config file</span></span>
4. <span data-ttu-id="15b7d-270">Sestavení windows čtyři virtuální počítače serveru</span><span class="sxs-lookup"><span data-stu-id="15b7d-270">Build four windows server VMs</span></span>
5. <span data-ttu-id="15b7d-271">Konfigurace, včetně NSG:</span><span class="sxs-lookup"><span data-stu-id="15b7d-271">Configure NSG including:</span></span>
   * <span data-ttu-id="15b7d-272">Vytvořit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="15b7d-272">Creating an NSG</span></span>
   * <span data-ttu-id="15b7d-273">Sestavování s pravidly</span><span class="sxs-lookup"><span data-stu-id="15b7d-273">Populating it with rules</span></span>
   * <span data-ttu-id="15b7d-274">Vytvoření vazby skupinu NSG na příslušné podsítě</span><span class="sxs-lookup"><span data-stu-id="15b7d-274">Binding the NSG to the appropriate subnets</span></span>

<span data-ttu-id="15b7d-275">Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.</span><span class="sxs-lookup"><span data-stu-id="15b7d-275">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="15b7d-276">Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="15b7d-276">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="15b7d-277">Pouze chybové zprávy červeně jsou příčinou problém.</span><span class="sxs-lookup"><span data-stu-id="15b7d-277">Only error messages in red are cause for concern.</span></span>
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
   - One server on the FrontEnd Subnet
   - Three Servers on the BackEnd Subnet
   - Network Security Groups to allow/deny traffic patterns as declared

  Before running script, ensure the network configuration file is created in
  the directory referenced by $NetworkConfigFile variable (or update the
  variable to reflect the path and file name of the config file being used).

 .Notes
  Security requirements are different for each use case and can be addressed in a
  myriad of ways. Please be sure that any sensitive data or applications are behind
  the appropriate layer(s) of protection. This script serves as an example of some
  of the techniques that can be used, but should not be used for all scenarios. You
  are responsible to assess your security needs and the appropriate protections
  needed, and then effectively implement those protections.

  FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
   IIS01      - 10.0.1.5

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

# User-Defined Global Variables
  # These should be changes to reflect your subscription and services
  # Invalid options will fail in the validation section

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
    # Note: To ensure proper NSG Rule creation later in this script:
    #       - The Web Server must be VM 0
    #       - The AppVM1 Server must be VM 1
    #       - The DNS server must be VM 3
    #
    #       Otherwise the NSG rules in the last section of this
    #       script will need to be changed to match the modified
    #       VM array numbers ($i) so the NSG Rule IP addresses
    #       are aligned to the associated VM IP addresses.

    # VM 0 - The Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - The First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - The Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - The DNS Server
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

  # Update Subscription Pointer to New Storage Account
    Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
    Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

# Validation
$FatalError = $false

If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
     Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
     $FatalError = $true}

If (Test-AzureName -Service -Name $FrontEndService) { 
    Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

If (Test-AzureName -Service -Name $BackEndService) { 
    Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

If (-Not (Test-Path $NetworkConfigFile)) { 
    Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "The network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
    Return}
Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

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
    Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

  # Build the NSG
    Write-Host "Building the NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
        -Protocol *

    # Assign the NSG to the Subnets
        Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

# Optional Post-script Manual Configuration
  # Install Test Web App (Run Post-Build Script on the IIS Server)
  # Install Backend resource (Run Post-Build Script on the AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a><span data-ttu-id="15b7d-278">Soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="15b7d-278">Network config file</span></span>
<span data-ttu-id="15b7d-279">Uložte tento soubor xml s aktualizované umístění a přidat odkaz na tento soubor do proměnné $NetworkConfigFile v předchozí skript.</span><span class="sxs-lookup"><span data-stu-id="15b7d-279">Save this xml file with updated location and add the link to this file to the $NetworkConfigFile variable in the preceding script.</span></span>

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

#### <a name="sample-application-scripts"></a><span data-ttu-id="15b7d-280">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="15b7d-280">Sample application scripts</span></span>
<span data-ttu-id="15b7d-281">Pokud chcete nainstalovat ukázkovou aplikaci pro toto a další příklady hraniční sítě, jednu bylo zadáno na následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="15b7d-281">If you wish to install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="15b7d-282">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15b7d-282">Next steps</span></span>
* <span data-ttu-id="15b7d-283">Aktualizace a uložte soubor XML</span><span class="sxs-lookup"><span data-stu-id="15b7d-283">Update and save XML file</span></span>
* <span data-ttu-id="15b7d-284">Spustit skript prostředí PowerShell k vytvoření prostředí</span><span class="sxs-lookup"><span data-stu-id="15b7d-284">Run the PowerShell script to build the environment</span></span>
* <span data-ttu-id="15b7d-285">Instalace ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="15b7d-285">Install the sample application</span></span>
* <span data-ttu-id="15b7d-286">Testování různé přenosové toky prostřednictvím této hraniční sítě</span><span class="sxs-lookup"><span data-stu-id="15b7d-286">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
<span data-ttu-id="15b7d-287">[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Příchozí DMZ s NSG"</span><span class="sxs-lookup"><span data-stu-id="15b7d-287">[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Inbound DMZ with NSG"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

