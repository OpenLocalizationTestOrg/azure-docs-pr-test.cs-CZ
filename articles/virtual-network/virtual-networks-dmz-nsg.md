---
title: "aaaAzure příklad DMZ – vytvoření jednoduché DMZ pomocí skupin Nsg | Microsoft Docs"
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
ms.openlocfilehash: 11c5c6026da30fbc9c5e585f5c16e2d411d6fd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a><span data-ttu-id="1fcb8-103">Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1fcb8-103">Example 1 – Build a simple DMZ using NSGs with an Azure Resource Manager template</span></span>
<span data-ttu-id="1fcb8-104">[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="1fcb8-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1fcb8-105">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1fcb8-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="1fcb8-106">Classic – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1fcb8-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="1fcb8-107">Tento příklad vytvoří primitivní hraniční sítě se čtyřmi servery Windows a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="1fcb8-108">Tento příklad popisuje každý hello odpovídající šablonu části tooprovide podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-108">This example describes each of hello relevant template sections tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="1fcb8-109">Je zde také na provoz scénář části tooprovide podrobný podrobný rozbor toho, jak se provoz pokračuje prostřednictvím hello vrstev obrany ve hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-109">There is also a Traffic Scenario section tooprovide an in-depth step-by-step look at how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="1fcb8-110">Nakonec v části odkazy hello je hello úplnou šablonu kódu a pokyny toobuild tento tootest prostředí a experimentů pomocí různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-110">Finally, in hello references section is hello complete template code and instructions toobuild this environment tootest and experiment with various scenarios.</span></span> 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

<span data-ttu-id="1fcb8-111">![Příchozí DMZ s NSG][1]</span><span class="sxs-lookup"><span data-stu-id="1fcb8-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="1fcb8-112">Popis prostředí</span><span class="sxs-lookup"><span data-stu-id="1fcb8-112">Environment description</span></span>
<span data-ttu-id="1fcb8-113">V tomto příkladu obsahuje předplatné hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-113">In this example a subscription contains hello following resources:</span></span>

* <span data-ttu-id="1fcb8-114">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="1fcb8-114">A single resource group</span></span>
* <span data-ttu-id="1fcb8-115">Virtuální síť se dvěma podsítěmi; "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="1fcb8-115">A Virtual Network with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="1fcb8-116">Skupinu zabezpečení sítě, která je použitá tooboth podsítě</span><span class="sxs-lookup"><span data-stu-id="1fcb8-116">A Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="1fcb8-117">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="1fcb8-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="1fcb8-118">Dva windows serverů, které představují servery back-end aplikace ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="1fcb8-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="1fcb8-119">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="1fcb8-119">A Windows server that represents a DNS server (“DNS01”)</span></span>
* <span data-ttu-id="1fcb8-120">Veřejné IP adresy přidružené k serveru webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1fcb8-120">A public IP address associated with hello application web server</span></span>

<span data-ttu-id="1fcb8-121">V části odkazy hello je šablony Azure Resource Manageru tooan odkaz, který sestaví hello prostředí popsané v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-121">In hello references section, there is a link tooan Azure Resource Manager template that builds hello environment described in this example.</span></span> <span data-ttu-id="1fcb8-122">Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádí hello příklad šablony, nejsou popsané podrobně v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-122">Building hello VMs and Virtual Networks, although done by hello example template, are not described in detail in this document.</span></span> 

<span data-ttu-id="1fcb8-123">**toobuild toto prostředí** (podrobné pokyny naleznete v části odkazy hello tohoto dokumentu);</span><span class="sxs-lookup"><span data-stu-id="1fcb8-123">**toobuild this environment** (detailed instructions are in hello references section of this document);</span></span>

1. <span data-ttu-id="1fcb8-124">Nasazení šablony Azure Resource Manageru v hello: [šablon Azure rychlý start][Template]</span><span class="sxs-lookup"><span data-stu-id="1fcb8-124">Deploy hello Azure Resource Manager Template at: [Azure Quickstart Templates][Template]</span></span>
2. <span data-ttu-id="1fcb8-125">Nainstalujte hello ukázkovou aplikaci v: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="1fcb8-125">Install hello sample application at: [Sample Application Script][SampleApp]</span></span>

>[!NOTE]
><span data-ttu-id="1fcb8-126">tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="1fcb8-126">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="1fcb8-127">První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-127">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span> <span data-ttu-id="1fcb8-128">Případně může být veřejnou IP adresu ke každému serveru síťový adaptér pro snazší RDP přidružena.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-128">Alternately a Public IP can be associated with each server NIC for easier RDP.</span></span>
> 
>

<span data-ttu-id="1fcb8-129">Hello následující části obsahují podrobný popis hello skupinu zabezpečení sítě a jak funguje v tomto příkladu pomocí s návodem klíče řádků hello šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-129">hello following sections provide a detailed description of hello Network Security Group and how it functions for this example by walking through key lines of hello Azure Resource Manager Template.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="1fcb8-130">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-130">Network Security Groups (NSG)</span></span>
<span data-ttu-id="1fcb8-131">V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-131">For this example, an NSG group is built and then loaded with six rules.</span></span> 

>[!TIP]
><span data-ttu-id="1fcb8-132">Obecně platí musí nejprve vytvořit konkrétní pravidel "Povolit" a pak poslední hello více obecná pravidla "Deny".</span><span class="sxs-lookup"><span data-stu-id="1fcb8-132">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="1fcb8-133">Hello prioritou stanoví, která pravidla se vyhodnocují jako první.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-133">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="1fcb8-134">Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-134">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="1fcb8-135">Pravidla NSG můžete použít buď v hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).</span><span class="sxs-lookup"><span data-stu-id="1fcb8-135">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
>
>

<span data-ttu-id="1fcb8-136">Deklarativně jsou pro příchozí provoz sestavuje hello následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-136">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="1fcb8-137">Interní DNS provoz (port 53) je povolený</span><span class="sxs-lookup"><span data-stu-id="1fcb8-137">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="1fcb8-138">Provoz protokolu RDP (portu 3389) z Internetu tooany hello virtuálních počítačů je povoleno.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-138">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="1fcb8-139">Je povolen přenos HTTP (port 80) ze serveru tooweb Internet hello (IIS01)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-139">HTTP traffic (port 80) from hello Internet tooweb server (IIS01) is allowed</span></span>
4. <span data-ttu-id="1fcb8-140">Jakýkoli přenos (všechny porty) z IIS01 tooAppVM1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-140">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="1fcb8-141">Jakýkoli přenos (všechny porty) z Internetu toohello hello celý virtuální síť (obě podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-141">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="1fcb8-142">Jakýkoli přenos (všechny porty) z hello front-endu podsíť toohello back-end podsítě byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-142">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="1fcb8-143">Pomocí těchto pravidel vázané tooeach podsíti, pokud požadavek HTTP byl příchozí z hello Internet toohello webový server, obě pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-143">With these rules bound tooeach subnet, if an HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="1fcb8-144">Proto hello požadavku HTTP bude mít možnost toohello webový server.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-144">Thus hello HTTP request would be allowed toohello web server.</span></span> <span data-ttu-id="1fcb8-145">Pokud tento stejný provoz pokoušel tooreach hello DNS01 server, pravidlo 5 (Odepřít) bude, že hello první tooapply hello přenosů dat a nebude možné toopass toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-145">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="1fcb8-146">Pravidlo 6 (Odepřít) blokuje podsíť Frontend hello z rozhovoru toohello back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), této sady pravidel chrání síť back-end hello v případě, že by útočník ohrožení hello webovou aplikaci na hello front-endu, útočník hello mít omezený přístup toohello back-end "chráněné" sítě (jenom tooresources zveřejněné na serveru AppVM01 hello).</span><span class="sxs-lookup"><span data-stu-id="1fcb8-146">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="1fcb8-147">Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-147">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="1fcb8-148">V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-148">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="1fcb8-149">tooapply tootraffic zásad zabezpečení v obou směrech, uživatel definovaná směrování je povinný a je prozkoumali "Příklad 3" na hello [stránku osvědčené postupy zabezpečení hranic][HOME].</span><span class="sxs-lookup"><span data-stu-id="1fcb8-149">tooapply security policy tootraffic in both directions, User Defined Routing is required and is explored in “Example 3” on hello [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="1fcb8-150">Každé pravidlo je podrobněji popsána následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-150">Each rule is discussed in more detail as follows:</span></span>

1. <span data-ttu-id="1fcb8-151">Prostředek skupinu zabezpečení sítě musí být instancí toohold hello pravidla:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-151">A Network Security Group resource must be instantiated toohold hello rules:</span></span>

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

2. <span data-ttu-id="1fcb8-152">první pravidlo Hello v tomto příkladu umožňuje přenosů mezi všechny server DNS toohello interní sítě v podsíti hello back-end.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-152">hello first rule in this example allows DNS traffic between all internal networks toohello DNS server on hello backend subnet.</span></span> <span data-ttu-id="1fcb8-153">pravidlo Hello má některé důležité parametry:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-153">hello rule has some important parameters:</span></span>
  * <span data-ttu-id="1fcb8-154">"destinationAddressPrefix" - pravidla můžete použít zvláštní typ předpona adresy názvem "Výchozí značka", tyto značky jsou identifikátory poskytované systémem, které umožňují snadný způsob tooaddress vyšší kategorie předpon adres.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-154">"destinationAddressPrefix" - Rules can use a special type of address prefix called a "Default Tag", these tags are system-provided identifiers that allow an easy way tooaddress a larger category of address prefixes.</span></span> <span data-ttu-id="1fcb8-155">Toto pravidlo používá hello výchozí značka "Internet" toosignify žádné adresy mimo hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-155">This rule uses hello Default Tag “Internet” toosignify any address outside of hello VNet.</span></span> <span data-ttu-id="1fcb8-156">Ostatní předponu značky jsou virtuální síť a AzureLoadBalancer.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-156">Other prefix labels are VirtualNetwork and AzureLoadBalancer.</span></span>
  * <span data-ttu-id="1fcb8-157">"Směr" označuje, že ve směru toku provozu účinné toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-157">“Direction” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="1fcb8-158">Směr Hello je z hlediska hello hello podsíť nebo virtuálního počítače (v závislosti na tom, kde je tato skupina NSG vázán).</span><span class="sxs-lookup"><span data-stu-id="1fcb8-158">hello direction is from hello perspective of hello subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="1fcb8-159">Proto pokud je směr "Příchozí" a provoz vstupující hello podsíť, hello pravidlo vztahuje a odchozího provozu z podsítě hello ovlivněn tímto pravidlem.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-159">Thus if Direction is “Inbound” and traffic is entering hello subnet, hello rule would apply and traffic leaving hello subnet would not be affected by this rule.</span></span>
  * <span data-ttu-id="1fcb8-160">"Priority" Nastaví hello pořadí, ve kterém je přenosový tok vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-160">“Priority” sets hello order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="1fcb8-161">Hello nižší hello číslo hello vyšší hello prioritou.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-161">hello lower hello number hello higher hello priority.</span></span> <span data-ttu-id="1fcb8-162">Když se pravidlo vztahuje tooa konkrétní přenosový tok, žádná další pravidla se zpracovávají.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-162">When a rule applies tooa specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="1fcb8-163">Proto pokud pravidlo s prioritou 1 umožňuje provoz a pravidlo s prioritou 2 odmítne provozu a obě pravidla použít tootraffic pak provoz hello bude mít možnost tooflow (vzhledem k tomu, že pravidlo 1 měl vyšší prioritu trvalo účinek a žádná další pravidla byly použity).</span><span class="sxs-lookup"><span data-stu-id="1fcb8-163">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply tootraffic then hello traffic would be allowed tooflow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
  * <span data-ttu-id="1fcb8-164">"Přístup" označuje, že toto pravidlo je-li blokované ("Deny") nebo povolených ("Povolit").</span><span class="sxs-lookup"><span data-stu-id="1fcb8-164">“Access” signifies if traffic affected by this rule is blocked ("Deny") or allowed ("Allow").</span></span>

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

3. <span data-ttu-id="1fcb8-165">Toto pravidlo umožňuje tooflow provoz protokolu RDP z hello internet toohello portu RDP na libovolném serveru v hello vázaný podsítě.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-165">This rule allows RDP traffic tooflow from hello internet toohello RDP port on any server on hello bound subnet.</span></span> 

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

4. <span data-ttu-id="1fcb8-166">Toto pravidlo umožňuje příchozí internetové přenosy toohit hello webový server.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-166">This rule allows inbound internet traffic toohit hello web server.</span></span> <span data-ttu-id="1fcb8-167">Toto pravidlo nedojde ke změně chování směrování hello.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-167">This rule does not change hello routing behavior.</span></span> <span data-ttu-id="1fcb8-168">Hello pravidlo umožňuje pouze provoz určený pro IIS01 toopass.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-168">hello rule only allows traffic destined for IIS01 toopass.</span></span> <span data-ttu-id="1fcb8-169">Takže pokud provoz z Internetu hello měl hello webový server jako svůj cíl toto pravidlo by se povolit a zastavit zpracování další pravidla.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-169">Thus if traffic from hello Internet had hello web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="1fcb8-170">(V hello pravidla s důležitostí 140 všechny ostatní příchozí internetový provoz blokováno).</span><span class="sxs-lookup"><span data-stu-id="1fcb8-170">(In hello rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="1fcb8-171">Pokud máte pouze zpracování přenos HTTP, může být toto pravidlo další s omezeným přístupem tooonly povolit cílový Port 80.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-171">If you're only processing HTTP traffic, this rule could be further restricted tooonly allow Destination Port 80.</span></span>

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet too[variables('VM01Name')]",
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

5. <span data-ttu-id="1fcb8-172">Toto pravidlo umožňuje toopass provoz ze serveru IIS01 hello toohello AppVM01 serveru, novější pravidlo blokuje všechny ostatní přenosy tooBackend front-endu.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-172">This rule allows traffic toopass from hello IIS01 server toohello AppVM01 server, a later rule blocks all other Frontend tooBackend traffic.</span></span> <span data-ttu-id="1fcb8-173">tooimprove, který toto pravidlo, pokud hello port se ví, že má být přidána.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-173">tooimprove this rule, if hello port is known that should be added.</span></span> <span data-ttu-id="1fcb8-174">Například pokud server služby IIS hello je stiskne pouze SQL Server na AppVM01, rozsah cílových portů hello by mělo být změněno z "*" (Any) too1433 (hello port služby SQL) umožňuje menší prostor pro příchozí útok na AppVM01, by měla webová aplikace hello někdy dojít k ohrožení.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-174">For example, if hello IIS server is hitting only SQL Server on AppVM01, hello Destination Port Range should be changed from “*” (Any) too1433 (hello SQL port) thus allowing a smaller inbound attack surface on AppVM01 should hello web application ever be compromised.</span></span>

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] too[variables('VM02Name')]",
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

6. <span data-ttu-id="1fcb8-175">Toto pravidlo odmítne provoz z hello internetové tooany servery v síti hello.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-175">This rule denies traffic from hello internet tooany servers on hello network.</span></span> <span data-ttu-id="1fcb8-176">S hello pravidla s důležitostí 110 a 120 hello efekt je tooallow pouze příchozí internetové přenosy toohello brány firewall a porty protokolu RDP na serverech a blokuje nic jiného.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-176">With hello rules at priority 110 and 120, hello effect is tooallow only inbound internet traffic toohello firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="1fcb8-177">Toto pravidlo je "jistotu" pravidlo tooblock všechny neočekávané toky.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-177">This rule is a "fail-safe" rule tooblock all unexpected flows.</span></span>

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate hello [variables('VNetName')] VNet from hello Internet",
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

7. <span data-ttu-id="1fcb8-178">poslední pravidlo Hello odmítne provoz z hello front-endu podsíť toohello back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-178">hello final rule denies traffic from hello Frontend subnet toohello Backend subnet.</span></span> <span data-ttu-id="1fcb8-179">Vzhledem k tomu, že toto pravidlo je pouze příchozí pravidlo, zpětné provoz je povolený (z back-end toohello hello front-endu).</span><span class="sxs-lookup"><span data-stu-id="1fcb8-179">Since this rule is an Inbound only rule, reverse traffic is allowed (from hello Backend toohello Frontend).</span></span>

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate hello [variables('Subnet1Name')] subnet from hello [variables('Subnet2Name')] subnet",
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

## <a name="traffic-scenarios"></a><span data-ttu-id="1fcb8-180">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="1fcb8-180">Traffic scenarios</span></span>
#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="1fcb8-181">(*Povolené*) internetového tooweb serveru</span><span class="sxs-lookup"><span data-stu-id="1fcb8-181">(*Allowed*) Internet tooweb server</span></span>
1. <span data-ttu-id="1fcb8-182">Internetu uživatel požádá o stránku HTTP z hello veřejnou IP adresu hello seskupování přidruženého hello IIS01 seskupování</span><span class="sxs-lookup"><span data-stu-id="1fcb8-182">An internet user requests an HTTP page from hello public IP address of hello NIC associated with hello IIS01 NIC</span></span>
2. <span data-ttu-id="1fcb8-183">Hello veřejnou IP adresu předá toohello provoz virtuální sítě směrem IIS01 (hello webový server)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-183">hello Public IP address passes traffic toohello VNet towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="1fcb8-184">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-184">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-185">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-185">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="1fcb8-186">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-186">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="1fcb8-187">Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="1fcb8-187">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="1fcb8-188">Provoz dotkne interní IP adresu hello webového serveru IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-188">Traffic hits internal IP address of hello web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="1fcb8-189">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello</span><span class="sxs-lookup"><span data-stu-id="1fcb8-189">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
6. <span data-ttu-id="1fcb8-190">IIS01 hello systému SQL Server na AppVM01 vyzve k zadání informací</span><span class="sxs-lookup"><span data-stu-id="1fcb8-190">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="1fcb8-191">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-191">No outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="1fcb8-192">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-192">hello Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-193">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-193">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="1fcb8-194">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-194">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="1fcb8-195">Skupina NSG pravidla 3 (Internet tooFirewall) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-195">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
  4. <span data-ttu-id="1fcb8-196">Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="1fcb8-196">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="1fcb8-197">AppVM01 přijímá hello dotazu SQL a odpoví</span><span class="sxs-lookup"><span data-stu-id="1fcb8-197">AppVM01 receives hello SQL Query and responds</span></span>
10. <span data-ttu-id="1fcb8-198">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti hello back-end, je povoleno hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="1fcb8-198">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
11. <span data-ttu-id="1fcb8-199">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-199">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-200">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="1fcb8-200">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="1fcb8-201">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-201">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
12. <span data-ttu-id="1fcb8-202">server služby IIS Hello obdrží odpověď hello SQL a dokončí hello odpovědi HTTP a odešle toohello žadatel</span><span class="sxs-lookup"><span data-stu-id="1fcb8-202">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requester</span></span>
13. <span data-ttu-id="1fcb8-203">Vzhledem k tomu, že neexistují žádná odchozí pravidla na podsíť Frontend hello, hello odpovědi je povolen a hello Internet uživatel obdrží webovou stránku hello požadovaný.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-203">Since there are no outbound rules on hello Frontend subnet, hello response is allowed and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-tooiis-server"></a><span data-ttu-id="1fcb8-204">(*Povolené*) serveru tooIIS RDP</span><span class="sxs-lookup"><span data-stu-id="1fcb8-204">(*Allowed*) RDP tooIIS server</span></span>
1. <span data-ttu-id="1fcb8-205">Správce serveru na Internetu požadavky tooIIS01 relace protokolu RDP na hello veřejnou IP adresu hello seskupování přidruženého hello IIS01 seskupování (Tato veřejná IP adresa naleznete prostřednictvím hello portálu nebo prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-205">A Server Admin on internet requests an RDP session tooIIS01 on hello public IP address of hello NIC associated with hello IIS01 NIC (this public IP address can be found via hello Portal or PowerShell)</span></span>
2. <span data-ttu-id="1fcb8-206">Hello veřejnou IP adresu předá toohello provoz virtuální sítě směrem IIS01 (hello webový server)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-206">hello Public IP address passes traffic toohello VNet towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="1fcb8-207">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-207">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-208">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-208">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="1fcb8-209">Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="1fcb8-209">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="1fcb8-210">Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="1fcb8-210">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="1fcb8-211">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-211">RDP session is enabled</span></span>
6. <span data-ttu-id="1fcb8-212">IIS01 vyzve k zadání hello uživatelské jméno a heslo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-212">IIS01 prompts for hello user name and password</span></span>

>[!NOTE]
><span data-ttu-id="1fcb8-213">tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="1fcb8-213">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="1fcb8-214">První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-214">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="1fcb8-215">(*Povolené*) hledání DNS webového serveru na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="1fcb8-215">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="1fcb8-216">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale potřebuje tooresolve hello adresu.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-216">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="1fcb8-217">Hello konfiguraci sítě pro virtuální síť seznamy hello DNS01 (10.0.2.4 v podsíti hello back-end) jako primární server DNS hello IIS01 odešle tooDNS01 požadavek DNS hello</span><span class="sxs-lookup"><span data-stu-id="1fcb8-217">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="1fcb8-218">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-218">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="1fcb8-219">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-219">Backend subnet begins inbound rule processing:</span></span>
  * <span data-ttu-id="1fcb8-220">Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="1fcb8-220">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="1fcb8-221">DNS server obdrží požadavek hello</span><span class="sxs-lookup"><span data-stu-id="1fcb8-221">DNS server receives hello request</span></span>
6. <span data-ttu-id="1fcb8-222">DNS server nemá hello adresu do mezipaměti a požádá kořenový server DNS na hello Internetu</span><span class="sxs-lookup"><span data-stu-id="1fcb8-222">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="1fcb8-223">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-223">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="1fcb8-224">Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="1fcb8-224">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="1fcb8-225">DNS server ukládá do mezipaměti odpovědi hello a odpoví back tooIIS01 toohello úvodního požadavku</span><span class="sxs-lookup"><span data-stu-id="1fcb8-225">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="1fcb8-226">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-226">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="1fcb8-227">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-227">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-228">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="1fcb8-228">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="1fcb8-229">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený</span><span class="sxs-lookup"><span data-stu-id="1fcb8-229">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="1fcb8-230">IIS01 obdrží odpověď hello z DNS01</span><span class="sxs-lookup"><span data-stu-id="1fcb8-230">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="1fcb8-231">(*Povolené*) přístup k souboru webového serveru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="1fcb8-231">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="1fcb8-232">IIS01 požádá o soubor na AppVM01</span><span class="sxs-lookup"><span data-stu-id="1fcb8-232">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="1fcb8-233">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-233">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="1fcb8-234">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-234">hello Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-235">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-235">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="1fcb8-236">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-236">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="1fcb8-237">Skupina NSG pravidla 3 (Internet tooIIS01) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-237">NSG Rule 3 (Internet tooIIS01) doesn’t apply, move toonext rule</span></span>
  4. <span data-ttu-id="1fcb8-238">Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="1fcb8-238">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="1fcb8-239">AppVM01 obdrží požadavek na hello a odpoví souboru (za předpokladu, že je autorizovaný přístup)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-239">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="1fcb8-240">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti hello back-end, je povoleno hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="1fcb8-240">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
6. <span data-ttu-id="1fcb8-241">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-241">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-242">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="1fcb8-242">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="1fcb8-243">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-243">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="1fcb8-244">server služby IIS Hello obdrží soubor hello</span><span class="sxs-lookup"><span data-stu-id="1fcb8-244">hello IIS server receives hello file</span></span>

#### <a name="denied-rdp-toobackend"></a><span data-ttu-id="1fcb8-245">(*Byl odepřen*) toobackend protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="1fcb8-245">(*Denied*) RDP toobackend</span></span>
1. <span data-ttu-id="1fcb8-246">Uživatel s Internetu pokusí tooRDP tooserver AppVM01</span><span class="sxs-lookup"><span data-stu-id="1fcb8-246">An internet user tries tooRDP tooserver AppVM01</span></span>
2. <span data-ttu-id="1fcb8-247">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="1fcb8-247">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="1fcb8-248">Pokud z nějakého důvodu byla povolená veřejnou IP adresu, ale by pravidla NSG 2 (RDP) povolit tento provoz</span><span class="sxs-lookup"><span data-stu-id="1fcb8-248">However if a Public IP address was enabled for some reason, NSG rule 2 (RDP) would allow this traffic</span></span>

>[!NOTE]
><span data-ttu-id="1fcb8-249">tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="1fcb8-249">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="1fcb8-250">První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-250">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="1fcb8-251">(*Byl odepřen*) toobackend webu</span><span class="sxs-lookup"><span data-stu-id="1fcb8-251">(*Denied*) Web toobackend server</span></span>
1. <span data-ttu-id="1fcb8-252">Uživatel s Internetu pokusí tooaccess souboru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="1fcb8-252">An internet user tries tooaccess a file on AppVM01</span></span>
2. <span data-ttu-id="1fcb8-253">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="1fcb8-253">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="1fcb8-254">Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, by tento provoz blokovat pravidla NSG 5 (Internet tooVNet)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-254">If a Public IP address was enabled for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="1fcb8-255">(*Byl odepřen*) hledání DNS pro Web na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="1fcb8-255">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="1fcb8-256">Uživatel s Internetu pokusí toolook až interní záznam DNS na DNS01</span><span class="sxs-lookup"><span data-stu-id="1fcb8-256">An internet user tries toolook up an internal DNS record on DNS01</span></span>
2. <span data-ttu-id="1fcb8-257">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="1fcb8-257">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="1fcb8-258">Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, pravidla NSG 5 (tooVNet Internetu) by blokovat tento provoz (Poznámka: aby pravidlo 1 (DNS) nebude použít, protože hello požadavky zdrojové adresy je hello internet a pravidla 1 uplatňuje se pouze toohello virtuální místní síť jako zdroj hello)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-258">If a Public IP address was enabled for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply because hello requests source address is hello internet and Rule 1 only applies toohello local VNet as hello source)</span></span>

#### <a name="denied-sql-access-on-hello-web-server"></a><span data-ttu-id="1fcb8-259">(*Byl odepřen*) SQL přístup na webový server hello</span><span class="sxs-lookup"><span data-stu-id="1fcb8-259">(*Denied*) SQL access on hello web server</span></span>
1. <span data-ttu-id="1fcb8-260">Uživatel s Internetu vyžaduje SQL data z IIS01</span><span class="sxs-lookup"><span data-stu-id="1fcb8-260">An internet user requests SQL data from IIS01</span></span>
2. <span data-ttu-id="1fcb8-261">Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="1fcb8-261">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="1fcb8-262">Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, podsíť Frontend hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-262">If a Public IP address was enabled for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="1fcb8-263">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-263">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="1fcb8-264">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="1fcb8-264">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="1fcb8-265">Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="1fcb8-265">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="1fcb8-266">Provoz dotkne interní IP adresu hello IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-266">Traffic hits internal IP address of hello IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="1fcb8-267">IIS01 nenaslouchá na portu 1433, takže žádný požadavek na toohello odpovědi</span><span class="sxs-lookup"><span data-stu-id="1fcb8-267">IIS01 isn't listening on port 1433, so no response toohello request</span></span>

## <a name="conclusion"></a><span data-ttu-id="1fcb8-268">Závěr</span><span class="sxs-lookup"><span data-stu-id="1fcb8-268">Conclusion</span></span>
<span data-ttu-id="1fcb8-269">V tomto příkladu je relativně jednoduché a splněny následující způsob izolace hello back-end podsíť z příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-269">This example is a relatively simple and straight forward way of isolating hello back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="1fcb8-270">Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].</span><span class="sxs-lookup"><span data-stu-id="1fcb8-270">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="1fcb8-271">Odkazy</span><span class="sxs-lookup"><span data-stu-id="1fcb8-271">References</span></span>
### <a name="azure-resource-manager-template"></a><span data-ttu-id="1fcb8-272">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1fcb8-272">Azure Resource Manager template</span></span>
<span data-ttu-id="1fcb8-273">Tento příklad používá šablonu Azure Resource Manager předdefinované v úložišti GitHub spravován společností Microsoft a otevřete toohello komunity.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-273">This example uses a predefined Azure Resource Manager template in a GitHub repository maintained by Microsoft and open toohello community.</span></span> <span data-ttu-id="1fcb8-274">Tuto šablonu můžete nasazovat přímo z Githubu, nebo stáhli a upravili toofit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-274">This template can be deployed straight out of GitHub, or downloaded and modified toofit your needs.</span></span> 

<span data-ttu-id="1fcb8-275">Hlavní šablona Hello je v souboru hello s názvem "azuredeploy.json."</span><span class="sxs-lookup"><span data-stu-id="1fcb8-275">hello main template is in hello file named "azuredeploy.json."</span></span> <span data-ttu-id="1fcb8-276">Tato šablona jde odeslat prostřednictvím prostředí PowerShell nebo rozhraní příkazového řádku (s soubor přidružené "azuredeploy.parameters.json" hello) toodeploy této šablony.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-276">This template can be submitted via PowerShell or CLI (with hello associated "azuredeploy.parameters.json" file) toodeploy this template.</span></span> <span data-ttu-id="1fcb8-277">Najít hello nejjednodušší způsob je toouse hello tlačítko "Nasadit tooAzure" na stránce README.md hello v Githubu.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-277">I find hello easiest way is toouse hello "Deploy tooAzure" button on hello README.md page at GitHub.</span></span>

<span data-ttu-id="1fcb8-278">toodeploy hello šablonu, která vytvoří tento příklad z Githubu a hello portál Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1fcb8-278">toodeploy hello template that builds this example from GitHub and hello Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="1fcb8-279">V prohlížeči přejděte toohello [šablony][Template]</span><span class="sxs-lookup"><span data-stu-id="1fcb8-279">From a browser, navigate toohello [Template][Template]</span></span>
2. <span data-ttu-id="1fcb8-280">Klikněte na tlačítko "Nasadit tooAzure" hello (nebo hello "Vizualizovat" tlačítko toosee grafické reprezentace této šablony)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-280">Click hello "Deploy tooAzure" button (or hello "Visualize" button toosee a graphical representation of this template)</span></span>
3. <span data-ttu-id="1fcb8-281">Zadejte v okně parametry hello hello účet úložiště, uživatelské jméno a heslo a potom klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="1fcb8-281">Enter hello Storage Account, User Name, and Password in hello Parameters blade, then click **OK**</span></span>
5. <span data-ttu-id="1fcb8-282">Vytvořte skupinu prostředků pro toto nasazení (můžete použít existující šablonu, ale I doporučujeme novou nejlepších výsledků dosáhnete)</span><span class="sxs-lookup"><span data-stu-id="1fcb8-282">Create a Resource Group for this deployment (You can use an existing one, but I recommend a new one for best results)</span></span>
6. <span data-ttu-id="1fcb8-283">V případě potřeby změňte hello předplatném a umístění nastavení pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-283">If necessary, change hello Subscription and Location settings for your VNet.</span></span>
7. <span data-ttu-id="1fcb8-284">Klikněte na tlačítko **přečíst si právní podmínky**, přečtěte si podmínky hello a klikněte na tlačítko **nákupu** tooagree.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-284">Click **Review legal terms**, read hello terms, and click **Purchase** tooagree.</span></span>
8. <span data-ttu-id="1fcb8-285">Klikněte na tlačítko **vytvořit** toobegin hello nasazení této šablony.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-285">Click **Create** toobegin hello deployment of this template.</span></span>
9. <span data-ttu-id="1fcb8-286">Po nasazení hello skončí úspěšně, přejděte toohello, který vytvořili skupinu prostředků pro toto nasazení prostředky hello toosee nakonfigurované uvnitř.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-286">Once hello deployment finishes successfully, navigate toohello Resource Group created for this deployment toosee hello resources configured inside.</span></span>

>[!NOTE]
><span data-ttu-id="1fcb8-287">Tato šablona umožňuje RDP toohello IIS01 pouze server (hello Najít veřejnou IP adresu pro IIS01 na hello portálu).</span><span class="sxs-lookup"><span data-stu-id="1fcb8-287">This template enables RDP toohello IIS01 server only (find hello Public IP for IIS01 on hello Portal).</span></span> <span data-ttu-id="1fcb8-288">tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole."</span><span class="sxs-lookup"><span data-stu-id="1fcb8-288">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="1fcb8-289">První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-289">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

<span data-ttu-id="1fcb8-290">tooremove tato nasazení, hello odstranit skupinu prostředků a všechny podřízené prostředky budou také odstraněny.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-290">tooremove this deployment, delete hello Resource Group and all child resources will also be deleted.</span></span>

#### <a name="sample-application-scripts"></a><span data-ttu-id="1fcb8-291">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="1fcb8-291">Sample application scripts</span></span>
<span data-ttu-id="1fcb8-292">Po úspěšném spuštění hello šablony můžete nastavit hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="1fcb8-292">Once hello template runs successfully, you can set up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span> <span data-ttu-id="1fcb8-293">tooinstall ukázkové aplikace pro toto a další příklady hraniční sítě, jednu bylo zadáno v hello následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="1fcb8-293">tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="1fcb8-294">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1fcb8-294">Next steps</span></span>

* <span data-ttu-id="1fcb8-295">Tento příklad nasazení</span><span class="sxs-lookup"><span data-stu-id="1fcb8-295">Deploy this example</span></span>
* <span data-ttu-id="1fcb8-296">Vytvoření ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1fcb8-296">Build hello sample application</span></span>
* <span data-ttu-id="1fcb8-297">Testování různé přenosové toky prostřednictvím této hraniční sítě</span><span class="sxs-lookup"><span data-stu-id="1fcb8-297">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Příchozí DMZ s NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md