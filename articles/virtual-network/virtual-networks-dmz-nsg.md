---
title: "Příklad Azure DMZ – vytvoření jednoduché DMZ pomocí skupin Nsg | Microsoft Docs"
description: "Sestavení DMZ se skupinami zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: ec29e6b250f927a3a4a94ffdf83d6c7c0e325722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a><span data-ttu-id="2e9bb-103">Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2e9bb-103">Example 1 – Build a simple DMZ using NSGs with an Azure Resource Manager template</span></span>
<span data-ttu-id="2e9bb-104">[Návrat na stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="2e9bb-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2e9bb-105">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="2e9bb-106">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e9bb-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="2e9bb-107">Tento příklad vytvoří primitivní hraniční sítě se čtyřmi servery Windows a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="2e9bb-108">Tento příklad popisuje jednotlivých částech odpovídající šablonu zajistit podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-108">This example describes each of the relevant template sections to provide a deeper understanding of each step.</span></span> <span data-ttu-id="2e9bb-109">Je také části provoz scénář poskytnout podrobný podrobný rozbor toho, jak se provoz pokračuje prostřednictvím vrstev obrany v hraniční síti.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-109">There is also a Traffic Scenario section to provide an in-depth step-by-step look at how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="2e9bb-110">Nakonec v odkazy na části je kód dokončení šablony a pokyny k vytvoření tohoto prostředí pro testování a experimentovat s různými scénáři.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-110">Finally, in the references section is the complete template code and instructions to build this environment to test and experiment with various scenarios.</span></span> 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

<span data-ttu-id="2e9bb-111">![Příchozí DMZ s NSG][1]</span><span class="sxs-lookup"><span data-stu-id="2e9bb-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="2e9bb-112">Popis prostředí</span><span class="sxs-lookup"><span data-stu-id="2e9bb-112">Environment description</span></span>
<span data-ttu-id="2e9bb-113">V tomto příkladu obsahuje odběru v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-113">In this example a subscription contains the following resources:</span></span>

* <span data-ttu-id="2e9bb-114">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="2e9bb-114">A single resource group</span></span>
* <span data-ttu-id="2e9bb-115">Virtuální síť se dvěma podsítěmi; "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="2e9bb-115">A Virtual Network with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="2e9bb-116">Skupinu zabezpečení sítě, který se použije pro obě podsítě</span><span class="sxs-lookup"><span data-stu-id="2e9bb-116">A Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="2e9bb-117">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="2e9bb-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="2e9bb-118">Dva windows serverů, které představují servery back-end aplikace ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="2e9bb-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="2e9bb-119">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="2e9bb-119">A Windows server that represents a DNS server (“DNS01”)</span></span>
* <span data-ttu-id="2e9bb-120">Veřejné IP adresy přidružené k serveru webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2e9bb-120">A public IP address associated with the application web server</span></span>

<span data-ttu-id="2e9bb-121">V části odkazy je odkaz na šablonu Azure Resource Manager, který sestaví prostředí popsané v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-121">In the references section, there is a link to an Azure Resource Manager template that builds the environment described in this example.</span></span> <span data-ttu-id="2e9bb-122">Vytváření virtuálních počítačů a virtuálních sítí, i když provádí šabloně příklad nejsou podrobně popsány v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-122">Building the VMs and Virtual Networks, although done by the example template, are not described in detail in this document.</span></span> 

<span data-ttu-id="2e9bb-123">**K vytvoření tohoto prostředí** (podrobné pokyny naleznete v části odkazy tohoto dokumentu);</span><span class="sxs-lookup"><span data-stu-id="2e9bb-123">**To build this environment** (detailed instructions are in the references section of this document);</span></span>

1. <span data-ttu-id="2e9bb-124">Nasazení šablony Azure Resource Manager v: [šablony Azure rychlý start][Template]</span><span class="sxs-lookup"><span data-stu-id="2e9bb-124">Deploy the Azure Resource Manager Template at: [Azure Quickstart Templates][Template]</span></span>
2. <span data-ttu-id="2e9bb-125">Nainstalovat ukázkovou aplikaci v: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="2e9bb-125">Install the sample application at: [Sample Application Script][SampleApp]</span></span>

>[!NOTE]
><span data-ttu-id="2e9bb-126">Pro připojení RDP k žádnému back-end serverů v této instanci serveru IIS slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="2e9bb-126">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="2e9bb-127">První RDP na server služby IIS a pak z RDP Server služby IIS na back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-127">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span> <span data-ttu-id="2e9bb-128">Případně může být veřejnou IP adresu ke každému serveru síťový adaptér pro snazší RDP přidružena.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-128">Alternately a Public IP can be associated with each server NIC for easier RDP.</span></span>
> 
>

<span data-ttu-id="2e9bb-129">Následující části obsahují podrobný popis skupinu zabezpečení sítě a jak funguje v tomto příkladu pomocí s návodem klíče řádky šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-129">The following sections provide a detailed description of the Network Security Group and how it functions for this example by walking through key lines of the Azure Resource Manager Template.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="2e9bb-130">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-130">Network Security Groups (NSG)</span></span>
<span data-ttu-id="2e9bb-131">V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-131">For this example, an NSG group is built and then loaded with six rules.</span></span> 

>[!TIP]
><span data-ttu-id="2e9bb-132">Obecně řečeno měli byste vytvořit konkrétní pravidel "Povolit" nejprve a pak více obecná pravidla "Deny" poslední.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-132">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="2e9bb-133">Přiřazené priority určují, které jsou pravidla vyhodnocena první.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-133">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="2e9bb-134">Jakmile provoz nenajde Pokud chcete použít pro konkrétní pravidlo, jsou vyhodnotit žádná další pravidla.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-134">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="2e9bb-135">Pravidla NSG můžete použít buď v příchozí nebo odchozí směr (z hlediska podsítě).</span><span class="sxs-lookup"><span data-stu-id="2e9bb-135">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
>
>

<span data-ttu-id="2e9bb-136">Následující pravidla deklarativně, se budou vytvářeny pro příchozí provoz:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-136">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="2e9bb-137">Interní DNS provoz (port 53) je povolený</span><span class="sxs-lookup"><span data-stu-id="2e9bb-137">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="2e9bb-138">Provoz protokolu RDP (portu 3389) z Internetu do všech virtuálních počítačů je povoleno.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-138">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="2e9bb-139">Je povolen přenos HTTP (port 80) z Internetu webový server (IIS01)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-139">HTTP traffic (port 80) from the Internet to web server (IIS01) is allowed</span></span>
4. <span data-ttu-id="2e9bb-140">Všechny přenosy (všechny porty) z IIS01 na AppVM1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-140">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="2e9bb-141">Přenosy dat (všechny porty) z Internetu pro celou virtuální síť (obě podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-141">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="2e9bb-142">Přenosy dat (všechny porty) z podsítě front-endu do podsítě back-end byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-142">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="2e9bb-143">Pomocí těchto pravidel vázána na každou podsíť, pokud požadavek HTTP byl příchozí z Internetu webový server, jak pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-143">With these rules bound to each subnet, if an HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="2e9bb-144">Proto bude mít možnost požadavku HTTP k webovému serveru.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-144">Thus the HTTP request would be allowed to the web server.</span></span> <span data-ttu-id="2e9bb-145">Pokud tento stejný provoz se pokusil připojit k serveru DNS01, pravidlo 5 (Odepřít) bude první použití a přenos nebude možné předat serveru.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-145">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="2e9bb-146">Pravidlo 6 (Odepřít) blokuje podsíť Frontend z rozhovoru s back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), této sady pravidel chrání síť back-end v případě ohrožení útočník webové aplikace na Frontendový, útočník by mít omezený přístup k back-end "chráněná" Síťová (pouze pro prostředky zveřejněné na serveru AppVM01).</span><span class="sxs-lookup"><span data-stu-id="2e9bb-146">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="2e9bb-147">Je výchozí odchozí pravidlo, které umožňuje přenos se k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-147">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="2e9bb-148">V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-148">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="2e9bb-149">Použít zásady zabezpečení na provoz v obou směrech, směrování definovaného uživatele je povinná a je prozkoumali "Příklad 3" na [stránku osvědčené postupy zabezpečení hranic][HOME].</span><span class="sxs-lookup"><span data-stu-id="2e9bb-149">To apply security policy to traffic in both directions, User Defined Routing is required and is explored in “Example 3” on the [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="2e9bb-150">Každé pravidlo je podrobněji popsána následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-150">Each rule is discussed in more detail as follows:</span></span>

1. <span data-ttu-id="2e9bb-151">Prostředek skupinu zabezpečení sítě musí být vytvořena instance pro uložení pravidla:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-151">A Network Security Group resource must be instantiated to hold the rules:</span></span>

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. <span data-ttu-id="2e9bb-152">První pravidlo v tomto příkladu umožňuje přenosů mezi všechny interní sítě na server DNS v podsíti back-end.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-152">The first rule in this example allows DNS traffic between all internal networks to the DNS server on the backend subnet.</span></span> <span data-ttu-id="2e9bb-153">Toto pravidlo má některé důležité parametry:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-153">The rule has some important parameters:</span></span>
  * <span data-ttu-id="2e9bb-154">"destinationAddressPrefix" - pravidla můžete použít zvláštní typ předpona adresy názvem "Výchozí značka", tyto značky jsou identifikátory poskytované systémem, které umožňují snadný způsob, jak vyřešit vyšší kategorie předpon adres.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-154">"destinationAddressPrefix" - Rules can use a special type of address prefix called a "Default Tag", these tags are system-provided identifiers that allow an easy way to address a larger category of address prefixes.</span></span> <span data-ttu-id="2e9bb-155">Toto pravidlo používá výchozí značky "Internet" k označují každou adresu, mimo síť VNet.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-155">This rule uses the Default Tag “Internet” to signify any address outside of the VNet.</span></span> <span data-ttu-id="2e9bb-156">Ostatní předponu značky jsou virtuální síť a AzureLoadBalancer.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-156">Other prefix labels are VirtualNetwork and AzureLoadBalancer.</span></span>
  * <span data-ttu-id="2e9bb-157">"Směr" označuje, že ve směru toku provozu účinné toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-157">“Direction” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="2e9bb-158">Směr je z hlediska podsíť nebo virtuálního počítače (v závislosti na tom, kde je tato skupina NSG vázán).</span><span class="sxs-lookup"><span data-stu-id="2e9bb-158">The direction is from the perspective of the subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="2e9bb-159">Proto pokud je směr "Příchozí" a provoz vstupující podsíť, pravidlo vztahuje a odchozího provozu z podsítě ovlivněn tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-159">Thus if Direction is “Inbound” and traffic is entering the subnet, the rule would apply and traffic leaving the subnet would not be affected by this rule.</span></span>
  * <span data-ttu-id="2e9bb-160">"Priority" Nastaví pořadí, ve kterém je vyhodnocena tok přenosů.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-160">“Priority” sets the order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="2e9bb-161">Nižší počet tím vyšší je priorita.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-161">The lower the number the higher the priority.</span></span> <span data-ttu-id="2e9bb-162">Když se pravidlo vztahuje na konkrétní přenosový tok, žádná další pravidla se zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-162">When a rule applies to a specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="2e9bb-163">Proto pokud pravidlo s prioritou 1 umožňuje provoz a pravidlo s prioritou 2 odmítne provozu a použít obě pravidla pro provoz pak provoz se bude moct toku (vzhledem k tomu, že pravidlo 1 měl vyšší prioritu trvalo účinek a žádná další pravidla byly použity).</span><span class="sxs-lookup"><span data-stu-id="2e9bb-163">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply to traffic then the traffic would be allowed to flow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
  * <span data-ttu-id="2e9bb-164">"Přístup" označuje, že toto pravidlo je-li blokované ("Deny") nebo povolených ("Povolit").</span><span class="sxs-lookup"><span data-stu-id="2e9bb-164">“Access” signifies if traffic affected by this rule is blocked ("Deny") or allowed ("Allow").</span></span>

    ```JSON
    "properties": {
    "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
    ```

3. <span data-ttu-id="2e9bb-165">Toto pravidlo umožňuje provoz protokolu RDP, které jsou předávány z Internetu k portu RDP na libovolném serveru v vázané podsíti.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-165">This rule allows RDP traffic to flow from the internet to the RDP port on any server on the bound subnet.</span></span> 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. <span data-ttu-id="2e9bb-166">Toto pravidlo umožňuje příchozí internetové přenosy narazila na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-166">This rule allows inbound internet traffic to hit the web server.</span></span> <span data-ttu-id="2e9bb-167">Toto pravidlo nedojde ke změně chování směrování.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-167">This rule does not change the routing behavior.</span></span> <span data-ttu-id="2e9bb-168">Toto pravidlo umožňuje pouze provoz určený pro IIS01 předat.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-168">The rule only allows traffic destined for IIS01 to pass.</span></span> <span data-ttu-id="2e9bb-169">Takže pokud provoz z Internetu měl webový server jako svůj cíl toto pravidlo by se povolit a zastavit zpracování další pravidla.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-169">Thus if traffic from the Internet had the web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="2e9bb-170">(V pravidla s důležitostí 140 všechny ostatní příchozí internetový provoz blokováno).</span><span class="sxs-lookup"><span data-stu-id="2e9bb-170">(In the rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="2e9bb-171">Pokud máte pouze zpracování přenos HTTP, může být toto pravidlo další omezena a Povolit jenom cílový Port 80.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-171">If you're only processing HTTP traffic, this rule could be further restricted to only allow Destination Port 80.</span></span>

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet to [variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. <span data-ttu-id="2e9bb-172">Toto pravidlo umožňuje přenos dat ze serveru IIS01 k serveru AppVM01 novější bloky pravidel všechny front-end pro provoz back-end.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-172">This rule allows traffic to pass from the IIS01 server to the AppVM01 server, a later rule blocks all other Frontend to Backend traffic.</span></span> <span data-ttu-id="2e9bb-173">Aby se zlepšil toto pravidlo, pokud je znám port, který má být přidána.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-173">To improve this rule, if the port is known that should be added.</span></span> <span data-ttu-id="2e9bb-174">Například pokud server služby IIS je stiskne pouze SQL Server na AppVM01, rozsah cílových portů by mělo být změněno z "*" (Any) 1433 (SQL port), což umožňuje menší prostor pro příchozí útok na AppVM01 by měl webové aplikace někdy dojít k ohrožení.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-174">For example, if the IIS server is hitting only SQL Server on AppVM01, the Destination Port Range should be changed from “*” (Any) to 1433 (the SQL port) thus allowing a smaller inbound attack surface on AppVM01 should the web application ever be compromised.</span></span>

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] to [variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. <span data-ttu-id="2e9bb-175">Toto pravidlo na všechny servery v síti odmítne přenosy z Internetu.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-175">This rule denies traffic from the internet to any servers on the network.</span></span> <span data-ttu-id="2e9bb-176">Pravidla s důležitostí 110 a 120 účinek je umožnit pouze příchozí internetové přenosy pro brány firewall a porty protokolu RDP na serverech a bloky nic jiného.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-176">With the rules at priority 110 and 120, the effect is to allow only inbound internet traffic to the firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="2e9bb-177">Toto pravidlo je "pohotovostního" pravidlo pro zablokování všechny neočekávané toky.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-177">This rule is a "fail-safe" rule to block all unexpected flows.</span></span>

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate the [variables('VNetName')] VNet from the Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. <span data-ttu-id="2e9bb-178">Poslední pravidlo odmítne provozu z podsítě front-endu do podsítě back-end.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-178">The final rule denies traffic from the Frontend subnet to the Backend subnet.</span></span> <span data-ttu-id="2e9bb-179">Vzhledem k tomu, že toto pravidlo je pouze příchozí pravidlo, zpětné provoz je povolený (z back-end na front-endu).</span><span class="sxs-lookup"><span data-stu-id="2e9bb-179">Since this rule is an Inbound only rule, reverse traffic is allowed (from the Backend to the Frontend).</span></span>

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate the [variables('Subnet1Name')] subnet from the [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="2e9bb-180">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="2e9bb-180">Traffic scenarios</span></span>
#### <a name="allowed-internet-to-web-server"></a><span data-ttu-id="2e9bb-181">(*Povolené*) Internet na webový server</span><span class="sxs-lookup"><span data-stu-id="2e9bb-181">(*Allowed*) Internet to web server</span></span>
1. <span data-ttu-id="2e9bb-182">Internetu uživatel požádá o stránku HTTP z veřejné IP adresy přidružené k seskupování IIS01 síťového adaptéru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-182">An internet user requests an HTTP page from the public IP address of the NIC associated with the IIS01 NIC</span></span>
2. <span data-ttu-id="2e9bb-183">Veřejná IP adresa předá provoz do virtuální sítě směrem IIS01 (webový server)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-183">The Public IP address passes traffic to the VNet towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="2e9bb-184">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-184">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-185">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-185">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="2e9bb-186">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-186">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="2e9bb-187">Použít NSG pravidla 3 (Internet k IIS01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="2e9bb-187">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="2e9bb-188">Provoz dotkne interní IP adresu serveru webového IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-188">Traffic hits internal IP address of the web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="2e9bb-189">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku</span><span class="sxs-lookup"><span data-stu-id="2e9bb-189">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
6. <span data-ttu-id="2e9bb-190">IIS01 systému SQL Server na AppVM01 vyzve k zadání informací</span><span class="sxs-lookup"><span data-stu-id="2e9bb-190">IIS01 asks the SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="2e9bb-191">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-191">No outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="2e9bb-192">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-192">The Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-193">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-193">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="2e9bb-194">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-194">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="2e9bb-195">Není použít, přejděte k další pravidla NSG pravidla 3 (Internet do brány Firewall)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-195">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
  4. <span data-ttu-id="2e9bb-196">Skupina NSG pravidla 4 použít (IIS01 k AppVM01), provoz je povolený, zastavte zpracování pravidla</span><span class="sxs-lookup"><span data-stu-id="2e9bb-196">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="2e9bb-197">AppVM01 přijme příkaz jazyka SQL a odpovídá</span><span class="sxs-lookup"><span data-stu-id="2e9bb-197">AppVM01 receives the SQL Query and responds</span></span>
10. <span data-ttu-id="2e9bb-198">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti back-end, je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="2e9bb-198">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
11. <span data-ttu-id="2e9bb-199">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-199">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-200">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="2e9bb-200">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="2e9bb-201">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povolen.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-201">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
12. <span data-ttu-id="2e9bb-202">Server služby IIS obdrží odpověď SQL a dokončení odpovědi HTTP a odešle do žadatel</span><span class="sxs-lookup"><span data-stu-id="2e9bb-202">The IIS server receives the SQL response and completes the HTTP response and sends to the requester</span></span>
13. <span data-ttu-id="2e9bb-203">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti front-endu, odpověď je povoleno a Internetu uživatel obdrží požadované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-203">Since there are no outbound rules on the Frontend subnet, the response is allowed and the Internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-iis-server"></a><span data-ttu-id="2e9bb-204">(*Povolené*) protokolu RDP na server služby IIS</span><span class="sxs-lookup"><span data-stu-id="2e9bb-204">(*Allowed*) RDP to IIS server</span></span>
1. <span data-ttu-id="2e9bb-205">Správce serveru na Internetu požadavky relaci protokolu RDP pro IIS01 na veřejnou IP adresu na síťový adaptér přidružený IIS01 síťový adaptér (Tato veřejná IP adresa naleznete prostřednictvím portálu nebo prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-205">A Server Admin on internet requests an RDP session to IIS01 on the public IP address of the NIC associated with the IIS01 NIC (this public IP address can be found via the Portal or PowerShell)</span></span>
2. <span data-ttu-id="2e9bb-206">Veřejná IP adresa předá provoz do virtuální sítě směrem IIS01 (webový server)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-206">The Public IP address passes traffic to the VNet towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="2e9bb-207">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-207">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-208">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-208">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="2e9bb-209">Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="2e9bb-209">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="2e9bb-210">Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="2e9bb-210">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="2e9bb-211">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-211">RDP session is enabled</span></span>
6. <span data-ttu-id="2e9bb-212">IIS01 vyzve k zadání uživatelského jména a hesla</span><span class="sxs-lookup"><span data-stu-id="2e9bb-212">IIS01 prompts for the user name and password</span></span>

>[!NOTE]
><span data-ttu-id="2e9bb-213">Pro připojení RDP k žádnému back-end serverů v této instanci serveru IIS slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="2e9bb-213">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="2e9bb-214">První RDP na server služby IIS a pak z RDP Server služby IIS na back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-214">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="2e9bb-215">(*Povolené*) hledání DNS webového serveru na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="2e9bb-215">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="2e9bb-216">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale musí pro překlad adres.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-216">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="2e9bb-217">Konfigurace sítě pro virtuální síť seznamy DNS01 (10.0.2.4 v podsíti back-end) jako primární server DNS, IIS01 odešle žádost DNS do DNS01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-217">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="2e9bb-218">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-218">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="2e9bb-219">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-219">Backend subnet begins inbound rule processing:</span></span>
  * <span data-ttu-id="2e9bb-220">Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="2e9bb-220">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="2e9bb-221">DNS server obdrží požadavek</span><span class="sxs-lookup"><span data-stu-id="2e9bb-221">DNS server receives the request</span></span>
6. <span data-ttu-id="2e9bb-222">DNS server nemá adresu do mezipaměti a požádá kořenový server DNS na Internetu</span><span class="sxs-lookup"><span data-stu-id="2e9bb-222">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="2e9bb-223">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-223">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="2e9bb-224">Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="2e9bb-224">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="2e9bb-225">DNS server odpověď do mezipaměti a reaguje na počáteční požadavek zpět na IIS01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-225">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="2e9bb-226">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-226">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="2e9bb-227">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-227">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-228">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="2e9bb-228">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="2e9bb-229">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povoleno</span><span class="sxs-lookup"><span data-stu-id="2e9bb-229">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="2e9bb-230">IIS01 obdrží odpověď od DNS01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-230">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="2e9bb-231">(*Povolené*) přístup k souboru webového serveru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-231">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="2e9bb-232">IIS01 požádá o soubor na AppVM01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-232">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="2e9bb-233">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-233">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="2e9bb-234">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-234">The Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-235">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-235">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="2e9bb-236">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-236">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="2e9bb-237">Není použít, přejděte k další pravidla NSG pravidla 3 (Internet k IIS01)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-237">NSG Rule 3 (Internet to IIS01) doesn’t apply, move to next rule</span></span>
  4. <span data-ttu-id="2e9bb-238">Skupina NSG pravidla 4 použít (IIS01 k AppVM01), provoz je povolený, zastavte zpracování pravidla</span><span class="sxs-lookup"><span data-stu-id="2e9bb-238">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="2e9bb-239">AppVM01 obdrží požadavek a odpoví souboru (za předpokladu, že je autorizovaný přístup)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-239">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="2e9bb-240">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti back-end, je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="2e9bb-240">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
6. <span data-ttu-id="2e9bb-241">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-241">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-242">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="2e9bb-242">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="2e9bb-243">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povolen.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-243">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="2e9bb-244">Server služby IIS obdrží soubor</span><span class="sxs-lookup"><span data-stu-id="2e9bb-244">The IIS server receives the file</span></span>

#### <a name="denied-rdp-to-backend"></a><span data-ttu-id="2e9bb-245">(*Byl odepřen*) protokolu RDP na back-end</span><span class="sxs-lookup"><span data-stu-id="2e9bb-245">(*Denied*) RDP to backend</span></span>
1. <span data-ttu-id="2e9bb-246">Uživatelé Internetu pokusí protokolu RDP na server AppVM01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-246">An internet user tries to RDP to server AppVM01</span></span>
2. <span data-ttu-id="2e9bb-247">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte síť VNet a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-247">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="2e9bb-248">Pokud z nějakého důvodu byla povolená veřejnou IP adresu, ale by pravidla NSG 2 (RDP) povolit tento provoz</span><span class="sxs-lookup"><span data-stu-id="2e9bb-248">However if a Public IP address was enabled for some reason, NSG rule 2 (RDP) would allow this traffic</span></span>

>[!NOTE]
><span data-ttu-id="2e9bb-249">Pro připojení RDP k žádnému back-end serverů v této instanci serveru IIS slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="2e9bb-249">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="2e9bb-250">První RDP na server služby IIS a pak z RDP Server služby IIS na back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-250">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="2e9bb-251">(*Byl odepřen*) webové back-end server</span><span class="sxs-lookup"><span data-stu-id="2e9bb-251">(*Denied*) Web to backend server</span></span>
1. <span data-ttu-id="2e9bb-252">Uživatel s Internetu pokusí o přístup k souboru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-252">An internet user tries to access a file on AppVM01</span></span>
2. <span data-ttu-id="2e9bb-253">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte síť VNet a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-253">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="2e9bb-254">Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, by tento provoz blokovat pravidla NSG 5 (Internet do virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-254">If a Public IP address was enabled for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="2e9bb-255">(*Byl odepřen*) hledání DNS pro Web na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="2e9bb-255">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="2e9bb-256">Uživatelé Internetu pokusí vyhledat interní záznam DNS na DNS01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-256">An internet user tries to look up an internal DNS record on DNS01</span></span>
2. <span data-ttu-id="2e9bb-257">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte síť VNet a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-257">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="2e9bb-258">Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, pravidla NSG 5 (Internet do virtuální sítě) by blokovat tento provoz (Poznámka: aby pravidlo 1 (DNS) nebude použít, protože žádosti zdrojová adresa je Internetu a pravidla 1 se vztahuje pouze na místní virtuální síť jako zdroj)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-258">If a Public IP address was enabled for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply because the requests source address is the internet and Rule 1 only applies to the local VNet as the source)</span></span>

#### <a name="denied-sql-access-on-the-web-server"></a><span data-ttu-id="2e9bb-259">(*Byl odepřen*) přístup SQL na webovém serveru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-259">(*Denied*) SQL access on the web server</span></span>
1. <span data-ttu-id="2e9bb-260">Uživatel s Internetu vyžaduje SQL data z IIS01</span><span class="sxs-lookup"><span data-stu-id="2e9bb-260">An internet user requests SQL data from IIS01</span></span>
2. <span data-ttu-id="2e9bb-261">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte síť VNet a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-261">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="2e9bb-262">Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, podsíť Frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-262">If a Public IP address was enabled for some reason, the Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="2e9bb-263">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-263">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="2e9bb-264">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-264">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="2e9bb-265">Použít NSG pravidla 3 (Internet k IIS01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="2e9bb-265">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="2e9bb-266">Provoz dotkne interní IP adresu IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-266">Traffic hits internal IP address of the IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="2e9bb-267">IIS01 nenaslouchá na portu 1433, takže žádná odpověď na žádost</span><span class="sxs-lookup"><span data-stu-id="2e9bb-267">IIS01 isn't listening on port 1433, so no response to the request</span></span>

## <a name="conclusion"></a><span data-ttu-id="2e9bb-268">Závěr</span><span class="sxs-lookup"><span data-stu-id="2e9bb-268">Conclusion</span></span>
<span data-ttu-id="2e9bb-269">V tomto příkladu je relativně jednoduché a splněny následující způsob izolace back-end podsíť z příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-269">This example is a relatively simple and straight forward way of isolating the back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="2e9bb-270">Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].</span><span class="sxs-lookup"><span data-stu-id="2e9bb-270">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="2e9bb-271">Odkazy</span><span class="sxs-lookup"><span data-stu-id="2e9bb-271">References</span></span>
### <a name="azure-resource-manager-template"></a><span data-ttu-id="2e9bb-272">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2e9bb-272">Azure Resource Manager template</span></span>
<span data-ttu-id="2e9bb-273">Tento příklad používá šablonu Azure Resource Manager předdefinované v úložišti GitHub spravován společností Microsoft a otevřené celé komunitě.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-273">This example uses a predefined Azure Resource Manager template in a GitHub repository maintained by Microsoft and open to the community.</span></span> <span data-ttu-id="2e9bb-274">Tuto šablonu je možné nasazovat přímo z Githubu, nebo stáhnout a upravit tak, aby vyhovovaly vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-274">This template can be deployed straight out of GitHub, or downloaded and modified to fit your needs.</span></span> 

<span data-ttu-id="2e9bb-275">Hlavní šablona je v souboru s názvem "azuredeploy.json."</span><span class="sxs-lookup"><span data-stu-id="2e9bb-275">The main template is in the file named "azuredeploy.json."</span></span> <span data-ttu-id="2e9bb-276">Tato šablona jde odeslat prostřednictvím prostředí PowerShell nebo rozhraní příkazového řádku (souborem přidružené "azuredeploy.parameters.json") k nasazení této šablony.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-276">This template can be submitted via PowerShell or CLI (with the associated "azuredeploy.parameters.json" file) to deploy this template.</span></span> <span data-ttu-id="2e9bb-277">Najít Nejjednodušším způsobem je použít tlačítko "Nasadit do Azure" na stránce README.md v Githubu.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-277">I find the easiest way is to use the "Deploy to Azure" button on the README.md page at GitHub.</span></span>

<span data-ttu-id="2e9bb-278">Pokud chcete nasadit šablonu, která vytvoří tento příklad z Githubu a portálu Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2e9bb-278">To deploy the template that builds this example from GitHub and the Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="2e9bb-279">V prohlížeči přejděte na [šablony][Template]</span><span class="sxs-lookup"><span data-stu-id="2e9bb-279">From a browser, navigate to the [Template][Template]</span></span>
2. <span data-ttu-id="2e9bb-280">Klikněte na tlačítko "Nasadit do Azure" (nebo na tlačítko "Vizualizovat" v tématu grafické reprezentace této šablony)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-280">Click the "Deploy to Azure" button (or the "Visualize" button to see a graphical representation of this template)</span></span>
3. <span data-ttu-id="2e9bb-281">Zadejte účet úložiště, uživatelské jméno a heslo v okně parametry a potom klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="2e9bb-281">Enter the Storage Account, User Name, and Password in the Parameters blade, then click **OK**</span></span>
5. <span data-ttu-id="2e9bb-282">Vytvořte skupinu prostředků pro toto nasazení (můžete použít existující šablonu, ale I doporučujeme novou nejlepších výsledků dosáhnete)</span><span class="sxs-lookup"><span data-stu-id="2e9bb-282">Create a Resource Group for this deployment (You can use an existing one, but I recommend a new one for best results)</span></span>
6. <span data-ttu-id="2e9bb-283">V případě potřeby změňte nastavení předplatném a umístění pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-283">If necessary, change the Subscription and Location settings for your VNet.</span></span>
7. <span data-ttu-id="2e9bb-284">Klikněte na tlačítko **přečíst si právní podmínky**, přečtěte si podmínky a klikněte na tlačítko **nákupu** souhlasit.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-284">Click **Review legal terms**, read the terms, and click **Purchase** to agree.</span></span>
8. <span data-ttu-id="2e9bb-285">Klikněte na tlačítko **vytvořit** zahájíte nasazení této šablony.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-285">Click **Create** to begin the deployment of this template.</span></span>
9. <span data-ttu-id="2e9bb-286">Po nasazení skončí úspěšně, přejděte do skupiny prostředků vytvořené pro toto nasazení, najdete v materiálech nakonfigurované uvnitř.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-286">Once the deployment finishes successfully, navigate to the Resource Group created for this deployment to see the resources configured inside.</span></span>

>[!NOTE]
><span data-ttu-id="2e9bb-287">Tato šablona umožňuje RDP k serveru IIS01 (Najít veřejné IP adresy pro IIS01 na portálu.).</span><span class="sxs-lookup"><span data-stu-id="2e9bb-287">This template enables RDP to the IIS01 server only (find the Public IP for IIS01 on the Portal).</span></span> <span data-ttu-id="2e9bb-288">Pro připojení RDP k žádnému back-end serverů v této instanci serveru IIS slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="2e9bb-288">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="2e9bb-289">První RDP na server služby IIS a pak z RDP Server služby IIS na back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-289">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

<span data-ttu-id="2e9bb-290">Pokud chcete odebrat toto nasazení, odstraňte skupinu prostředků a všechny podřízené prostředky budou také odstraněny.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-290">To remove this deployment, delete the Resource Group and all child resources will also be deleted.</span></span>

#### <a name="sample-application-scripts"></a><span data-ttu-id="2e9bb-291">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="2e9bb-291">Sample application scripts</span></span>
<span data-ttu-id="2e9bb-292">Po úspěšném spuštění šablony můžete nastavit webový server a server aplikace s jednoduchou webovou aplikaci umožňující testování s touto konfigurací hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="2e9bb-292">Once the template runs successfully, you can set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span> <span data-ttu-id="2e9bb-293">Instalace ukázkové aplikace pro toto a další příklady hraniční sítě, jednu bylo zadáno na následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="2e9bb-293">To install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e9bb-294">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e9bb-294">Next steps</span></span>

* <span data-ttu-id="2e9bb-295">Tento příklad nasazení</span><span class="sxs-lookup"><span data-stu-id="2e9bb-295">Deploy this example</span></span>
* <span data-ttu-id="2e9bb-296">Vytvoření ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="2e9bb-296">Build the sample application</span></span>
* <span data-ttu-id="2e9bb-297">Testování různé přenosové toky prostřednictvím této hraniční sítě</span><span class="sxs-lookup"><span data-stu-id="2e9bb-297">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
<span data-ttu-id="2e9bb-298">[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Příchozí DMZ s NSG"</span><span class="sxs-lookup"><span data-stu-id="2e9bb-298">[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Inbound DMZ with NSG"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md