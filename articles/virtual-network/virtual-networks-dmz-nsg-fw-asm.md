---
title: "Příklad DMZ – sestavení DMZ k ochraně aplikací s bránou Firewall a skupin Nsg | Microsoft Docs"
description: "Sestavení DMZ s bránou Firewall a skupiny zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc0e8a3fa749eb2e6f65ef92c2d3cb404cfc8bc0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a><span data-ttu-id="21d0d-103">Příklad 2 – Vytvoření DMZ k ochraně aplikací s bránou Firewall a skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="21d0d-103">Example 2 – Build a DMZ to protect applications with a Firewall and NSGs</span></span>
<span data-ttu-id="21d0d-104">[Návrat na stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="21d0d-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="21d0d-105">Tento příklad vytvoří DMZ s bránou firewall, čtyři windows serverů a skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="21d0d-105">This example will create a DMZ with a firewall, four windows servers, and Network Security Groups.</span></span> <span data-ttu-id="21d0d-106">Je také provede každou z relevantních příkazů, které poskytují podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="21d0d-106">It will also walk through each of the relevant commands to provide a deeper understanding of each step.</span></span> <span data-ttu-id="21d0d-107">Je také části provoz scénář zajistit podrobné krok za krokem, jak se provoz pokračuje prostřednictvím vrstev obrany v hraniční síti.</span><span class="sxs-lookup"><span data-stu-id="21d0d-107">There is also a Traffic Scenario section to provide an in-depth step-by-step how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="21d0d-108">Nakonec v odkazy na části je kompletní kód a pokyny k vytvoření tohoto prostředí pro testování a experimentovat s různými scénáři.</span><span class="sxs-lookup"><span data-stu-id="21d0d-108">Finally, in the references section is the complete code and instruction to build this environment to test and experiment with various scenarios.</span></span> 

<span data-ttu-id="21d0d-109">![Příchozí DMZ s hodnocení chyb zabezpečení a NSG][1]</span><span class="sxs-lookup"><span data-stu-id="21d0d-109">![Inbound DMZ with NVA and NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="21d0d-110">Popis prostředí</span><span class="sxs-lookup"><span data-stu-id="21d0d-110">Environment Description</span></span>
<span data-ttu-id="21d0d-111">V tomto příkladu je odběr, který obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="21d0d-111">In this example there is a subscription that contains the following:</span></span>

* <span data-ttu-id="21d0d-112">Dva cloudové služby: "FrontEnd001" a "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="21d0d-112">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="21d0d-113">Virtuální síť "CorpNetwork" se dvěma podsítěmi: "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="21d0d-113">A Virtual Network “CorpNetwork”, with two subnets: “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="21d0d-114">Jednu skupinu zabezpečení sítě, která se použije pro obě podsítě</span><span class="sxs-lookup"><span data-stu-id="21d0d-114">A single Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="21d0d-115">Virtuální zařízení sítě, v tomto příkladu Barracuda NextGen Firewall, připojený k podsíti front-endu</span><span class="sxs-lookup"><span data-stu-id="21d0d-115">A network virtual appliance, in this example a Barracuda NextGen Firewall, connected to the Frontend subnet</span></span>
* <span data-ttu-id="21d0d-116">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="21d0d-116">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="21d0d-117">Dva windows servery, které představují aplikace zpět ukončení servery ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="21d0d-117">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="21d0d-118">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="21d0d-118">A Windows server that represents a DNS server (“DNS01”)</span></span>

> [!NOTE]
> <span data-ttu-id="21d0d-119">I když tento příklad používá Barracuda NextGen Firewall, řadu různých síťových virtuálních zařízení by mohly být použity v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="21d0d-119">Although this example uses a Barracuda NextGen Firewall, many of the different Network Virtual Appliances could be used for this example.</span></span>
> 
> 

<span data-ttu-id="21d0d-120">V části odkazy níže je skript prostředí PowerShell, který bude vytvořit většinu prostředí popsané výše.</span><span class="sxs-lookup"><span data-stu-id="21d0d-120">In the references section below there is a PowerShell script that will build most of the environment described above.</span></span> <span data-ttu-id="21d0d-121">Vytváření virtuálních počítačů a virtuálních sítí, i když provádějí ukázkový skript, nejsou podrobně popsané v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="21d0d-121">Building the VMs and Virtual Networks, although are done by the example script, are not described in detail in this document.</span></span>

<span data-ttu-id="21d0d-122">K vytvoření prostředí:</span><span class="sxs-lookup"><span data-stu-id="21d0d-122">To build the environment:</span></span>

1. <span data-ttu-id="21d0d-123">Uložte soubor xml konfigurace sítě v oddíle odkazy (aktualizovat název, umístění a IP adresy, které odpovídají danému scénáři)</span><span class="sxs-lookup"><span data-stu-id="21d0d-123">Save the network config xml file included in the references section (updated with names, location, and IP addresses to match the given scenario)</span></span>
2. <span data-ttu-id="21d0d-124">Aktualizace uživatelské proměnné ve skriptu tak, aby odpovídaly prostředí, ve kterém je skript ke spouštění (odběry, názvy služeb atd.)</span><span class="sxs-lookup"><span data-stu-id="21d0d-124">Update the user variables in the script to match the environment the script is to be run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="21d0d-125">Spusťte skript v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="21d0d-125">Execute the script in PowerShell</span></span>

<span data-ttu-id="21d0d-126">**Poznámka:**: oblasti označený ve skriptu PowerShell musí odpovídat oblasti označeny v souboru xml konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="21d0d-126">**Note**: The region signified in the PowerShell script must match the region signified in the network configuration xml file.</span></span>

<span data-ttu-id="21d0d-127">Po úspěšném spuštění skriptu mohou být přijata po skriptu takto:</span><span class="sxs-lookup"><span data-stu-id="21d0d-127">Once the script runs successfully the following post-script steps may be taken:</span></span>

1. <span data-ttu-id="21d0d-128">Nastavit pravidla brány firewall, najdete v následující části s názvem: pravidla brány Firewall.</span><span class="sxs-lookup"><span data-stu-id="21d0d-128">Set up the firewall rules, this is covered in the section below titled: Firewall Rules.</span></span>
2. <span data-ttu-id="21d0d-129">Volitelně můžete v části odkazy jsou dva skripty k nastavení webového serveru a aplikačního serveru s jednoduchou webovou aplikaci umožňující testování s touto konfigurací DMZ.</span><span class="sxs-lookup"><span data-stu-id="21d0d-129">Optionally in the references section are two scripts to set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span>

<span data-ttu-id="21d0d-130">V další části vysvětluje většinu příkazů skripty, relativně k skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="21d0d-130">The next section explains most of the scripts statements relative to Network Security Groups.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="21d0d-131">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="21d0d-131">Network Security Groups (NSG)</span></span>
<span data-ttu-id="21d0d-132">V tomto příkladu je skupina NSG vytvořené a pak načtená šesti pravidla.</span><span class="sxs-lookup"><span data-stu-id="21d0d-132">For this example, a NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="21d0d-133">Obecně řečeno měli byste vytvořit konkrétní pravidel "Povolit" nejprve a pak více obecná pravidla "Deny" poslední.</span><span class="sxs-lookup"><span data-stu-id="21d0d-133">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="21d0d-134">Přiřazené priority určují, které jsou pravidla vyhodnocena první.</span><span class="sxs-lookup"><span data-stu-id="21d0d-134">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="21d0d-135">Jakmile provoz nenajde Pokud chcete použít pro konkrétní pravidlo, jsou vyhodnotit žádná další pravidla.</span><span class="sxs-lookup"><span data-stu-id="21d0d-135">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="21d0d-136">Pravidla NSG můžete použít buď v příchozí nebo odchozí směr (z hlediska podsítě).</span><span class="sxs-lookup"><span data-stu-id="21d0d-136">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
> 
> 

<span data-ttu-id="21d0d-137">Následující pravidla deklarativně, se budou vytvářeny pro příchozí provoz:</span><span class="sxs-lookup"><span data-stu-id="21d0d-137">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="21d0d-138">Interní DNS provoz (port 53) je povolený</span><span class="sxs-lookup"><span data-stu-id="21d0d-138">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="21d0d-139">Provoz protokolu RDP (portu 3389) z Internetu do všech virtuálních počítačů je povoleno.</span><span class="sxs-lookup"><span data-stu-id="21d0d-139">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="21d0d-140">Je povolen přenos HTTP (port 80) z Internetu do hodnocení chyb zabezpečení sítě (brány firewall)</span><span class="sxs-lookup"><span data-stu-id="21d0d-140">HTTP traffic (port 80) from the Internet to the NVA (firewall) is allowed</span></span>
4. <span data-ttu-id="21d0d-141">Všechny přenosy (všechny porty) z IIS01 na AppVM1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="21d0d-141">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="21d0d-142">Přenosy dat (všechny porty) z Internetu pro celou virtuální síť (obě podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="21d0d-142">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="21d0d-143">Přenosy dat (všechny porty) z podsítě front-endu do podsítě back-end byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="21d0d-143">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="21d0d-144">Pomocí těchto pravidel vázána na každou podsíť, pokud požadavek HTTP byl příchozí z Internetu webový server, jak pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit.</span><span class="sxs-lookup"><span data-stu-id="21d0d-144">With these rules bound to each subnet, if a HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="21d0d-145">Proto bude mít možnost požadavek HTTP do brány firewall.</span><span class="sxs-lookup"><span data-stu-id="21d0d-145">Thus the HTTP request would be allowed to the firewall.</span></span> <span data-ttu-id="21d0d-146">Pokud tento stejný provoz se pokusil připojit k serveru DNS01, pravidlo 5 (Odepřít) bude první použití a přenos nebude možné předat serveru.</span><span class="sxs-lookup"><span data-stu-id="21d0d-146">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="21d0d-147">Pravidlo 6 (Odepřít) blokuje podsíť Frontend z rozhovoru s back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), to chrání síť back-end v případě ohrožení útočník webové aplikace na Frontendový, útočník by mít omezený přístup k back-end "chráněná" Síťová (pouze pro prostředky zveřejněné na serveru AppVM01).</span><span class="sxs-lookup"><span data-stu-id="21d0d-147">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="21d0d-148">Je výchozí odchozí pravidlo, které umožňuje přenos se k Internetu.</span><span class="sxs-lookup"><span data-stu-id="21d0d-148">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="21d0d-149">V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="21d0d-149">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="21d0d-150">Zamknout provoz v jak pokynů, směrování definovaného uživatele je vyžadován, to je v jiné příkladu, který lze nalézt v prozkoumali [hlavní zabezpečení hranic dokumentu][HOME].</span><span class="sxs-lookup"><span data-stu-id="21d0d-150">To lock down traffic in both directions, User Defined Routing is required, this is explored in a different example that can found in the [main security boundary document][HOME].</span></span>

<span data-ttu-id="21d0d-151">Výše uvedené pravidla NSG jsou velmi podobné pravidla NSG v [Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg][Example1].</span><span class="sxs-lookup"><span data-stu-id="21d0d-151">The above discussed NSG rules are very similar to the NSG rules in [Example 1 - Build a Simple DMZ with NSGs][Example1].</span></span> <span data-ttu-id="21d0d-152">Přečtěte si popis NSG v tomto dokumentu pro podrobný pohled na každé pravidlo NSG a jeho atributy.</span><span class="sxs-lookup"><span data-stu-id="21d0d-152">Please review the NSG Description in that document for a detailed look at each NSG rule and it's attributes.</span></span>

## <a name="firewall-rules"></a><span data-ttu-id="21d0d-153">Pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="21d0d-153">Firewall Rules</span></span>
<span data-ttu-id="21d0d-154">Klient správy bude potřeba nainstalovat na počítač pro správu brány firewall a vytvoření konfigurací potřeby.</span><span class="sxs-lookup"><span data-stu-id="21d0d-154">A management client will need to be installed on a PC to manage the firewall and create the configurations needed.</span></span> <span data-ttu-id="21d0d-155">O tom, jak spravovat zařízení, najdete v článku dodavatele dokumentace z brány firewall (nebo jiné hodnocení chyb zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="21d0d-155">See the documentation from your firewall (or other NVA) vendor on how to manage the device.</span></span> <span data-ttu-id="21d0d-156">Zbytek této části se popisují konfiguraci brány firewall, samostatně, prostřednictvím dodavatele správy klienta (tzn. ne portál Azure nebo PowerShell).</span><span class="sxs-lookup"><span data-stu-id="21d0d-156">The remainder of this section will describe the configuration of the firewall itself, through the vendors management client (i.e. not the Azure portal or PowerShell).</span></span>

<span data-ttu-id="21d0d-157">Pokyny pro stažení klienta a připojení k Barracuda použité v tomto příkladu naleznete zde: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="21d0d-157">Instructions for client download and connecting to the Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="21d0d-158">V bráně firewall předávání pravidla bude nutné vytvořit.</span><span class="sxs-lookup"><span data-stu-id="21d0d-158">On the firewall, forwarding rules will need to be created.</span></span> <span data-ttu-id="21d0d-159">Vzhledem k tomu, že v tomto příkladu pouze trasy internetové přenosy v vázané do brány firewall a poté na webový server, je potřeba jenom jeden předávání pravidlo NAT.</span><span class="sxs-lookup"><span data-stu-id="21d0d-159">Since this example only routes internet traffic in-bound to the firewall and then to the web server, only one forwarding NAT rule is needed.</span></span> <span data-ttu-id="21d0d-160">V bráně Barracuda NextGen Firewall použité v tomto příkladu bude pravidlo NAT cílové pravidlo ("Dst NAT"), abyste předat tento provoz.</span><span class="sxs-lookup"><span data-stu-id="21d0d-160">On the Barracuda NextGen Firewall used in this example the rule would be a Destination NAT rule (“Dst NAT”) to pass this traffic.</span></span>

<span data-ttu-id="21d0d-161">Vytvořit následující pravidlo (nebo ověřit existující výchozí pravidla), od řídicím panelu Barracuda NG správce klienta, přejděte na kartu Konfigurace, v provozní konfiguraci oddíl, klikněte na Ruleset.</span><span class="sxs-lookup"><span data-stu-id="21d0d-161">To create the following rule (or verify existing default rules), starting from the Barracuda NG Admin client dashboard, navigate to the configuration tab, in the Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="21d0d-162">Mřížka názvem, zobrazí "Hlavní pravidla" existující pravidla aktivní a deaktivované v bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="21d0d-162">A grid called, “Main Rules” will show the existing active and deactivated rules on the firewall.</span></span> <span data-ttu-id="21d0d-163">V pravém horním rohu mřížce je malý, zelená "+" tlačítko, klepněte sem a vytvořit nové pravidlo (Poznámka: Brána firewall může "zamknout" změny, pokud se zobrazí tlačítko označeno "Zamknout" a nelze vytvořit nebo upravit pravidla, klikněte na toto tlačítko a "odemknout" Sada pravidel pro povolení úprav).</span><span class="sxs-lookup"><span data-stu-id="21d0d-163">In the upper right corner of this grid is a small, green “+” button, click this to create a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable to create or edit rules, click this button to “unlock” the ruleset and allow editing).</span></span> <span data-ttu-id="21d0d-164">Pokud chcete upravit existující pravidlo, vyberte toto pravidlo, klikněte pravým tlačítkem a vyberte Upravit pravidlo.</span><span class="sxs-lookup"><span data-stu-id="21d0d-164">If you wish to edit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="21d0d-165">Vytvořit nové pravidlo a zadejte název, jako je například "WebTraffic".</span><span class="sxs-lookup"><span data-stu-id="21d0d-165">Create a new rule and provide a name, such as "WebTraffic".</span></span> 

<span data-ttu-id="21d0d-166">Ikona pravidlo NAT cílové vypadá takto: ![ikonu cílové NAT][2]</span><span class="sxs-lookup"><span data-stu-id="21d0d-166">The Destination NAT rule icon looks like this: ![Destination NAT Icon][2]</span></span>

<span data-ttu-id="21d0d-167">Pravidlo samotné by vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="21d0d-167">The rule itself would look something like this:</span></span>

<span data-ttu-id="21d0d-168">![Pravidlo brány firewall][3]</span><span class="sxs-lookup"><span data-stu-id="21d0d-168">![Firewall Rule][3]</span></span>

<span data-ttu-id="21d0d-169">Zde žádné příchozí adresu, která dotkne bránu Firewall při pokusu o přístup protokolu HTTP (port 80 nebo 443 pro protokol HTTPS) budou odeslány mimo bránu Firewall "Místní IP adresy DHCP1" rozhraní a přesměruje na webový server s IP adresou 10.0.1.5.</span><span class="sxs-lookup"><span data-stu-id="21d0d-169">Here any inbound address that hits the Firewall trying to reach HTTP (port 80 or 443 for HTTPS) will be sent out the Firewall’s “DHCP1 Local IP” interface and redirected to the Web Server with the IP Address of 10.0.1.5.</span></span> <span data-ttu-id="21d0d-170">Vzhledem k tomu, že je provoz brzo na portu 80 a přejdete na webový server na portu 80 bylo potřeba žádné změny portu.</span><span class="sxs-lookup"><span data-stu-id="21d0d-170">Since the traffic is coming in on port 80 and going to the web server on port 80 no port change was needed.</span></span> <span data-ttu-id="21d0d-171">Ale cílového seznamu by byly 10.0.1.5:8080 Pokud naslouchali naše Webový Server na portu 8080, proto překladu příchozí port 80 v bráně firewall pro příchozí port 8080 na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="21d0d-171">However, the Target List could have been 10.0.1.5:8080 if our Web Server listened on port 8080 thus translating the inbound port 80 on the firewall to inbound port 8080 on the web server.</span></span>

<span data-ttu-id="21d0d-172">Metodu připojení musí také být označeny, cílový pravidla z Internetu, "Dynamické překládat pomocí SNAT" je nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="21d0d-172">A Connection Method should also be signified, for the Destination Rule from the Internet, "Dynamic SNAT" is most appropriate.</span></span> 

<span data-ttu-id="21d0d-173">I když existuje pouze jedno pravidlo je důležité, aby jeho priorita je nastavena správně.</span><span class="sxs-lookup"><span data-stu-id="21d0d-173">Although only one rule has been created it's important that its priority is set correctly.</span></span> <span data-ttu-id="21d0d-174">Pokud v mřížce všechna pravidla v bráně firewall toto nové pravidlo je v dolní (pod pravidlo "BLOCKALL") se nikdy přijít do play.</span><span class="sxs-lookup"><span data-stu-id="21d0d-174">If in the grid of all rules on the firewall this new rule is on the bottom (below the "BLOCKALL" rule) it will never come into play.</span></span> <span data-ttu-id="21d0d-175">Zajistěte, aby nově vytvořeného pravidla pro webový provoz je větší než BLOCKALL pravidlo.</span><span class="sxs-lookup"><span data-stu-id="21d0d-175">Ensure the newly created rule for web traffic is above the BLOCKALL rule.</span></span>

<span data-ttu-id="21d0d-176">Po vytvoření pravidla, musí být nabídnutých do brány firewall a pak se aktivuje, pokud to neuděláte pravidlo změny se neprojeví.</span><span class="sxs-lookup"><span data-stu-id="21d0d-176">Once the rule is created, it must be pushed to the firewall and then activated, if this is not done the rule change will not take effect.</span></span> <span data-ttu-id="21d0d-177">Proces nabízené a aktivace je popsaná v další části.</span><span class="sxs-lookup"><span data-stu-id="21d0d-177">The push and activation process is described in the next section.</span></span>

## <a name="rule-activation"></a><span data-ttu-id="21d0d-178">Aktivace pravidla</span><span class="sxs-lookup"><span data-stu-id="21d0d-178">Rule Activation</span></span>
<span data-ttu-id="21d0d-179">S ruleset upravit tak, aby přidat toto pravidlo musí být nahrán do brány firewall a aktivovat ruleset.</span><span class="sxs-lookup"><span data-stu-id="21d0d-179">With the ruleset modified to add this rule, the ruleset must be uploaded to the firewall and activated.</span></span>

<span data-ttu-id="21d0d-180">![Aktivace pravidla brány firewall][4]</span><span class="sxs-lookup"><span data-stu-id="21d0d-180">![Firewall Rule Activation][4]</span></span>

<span data-ttu-id="21d0d-181">V pravém horním rohu správy klienta jsou součástí clusteru tlačítka.</span><span class="sxs-lookup"><span data-stu-id="21d0d-181">In the upper right hand corner of the management client are a cluster of buttons.</span></span> <span data-ttu-id="21d0d-182">Kliknutím na tlačítko "Změny odeslat" poslat upravené pravidla brány firewall a potom klikněte na tlačítko "Aktivovat".</span><span class="sxs-lookup"><span data-stu-id="21d0d-182">Click the “Send Changes” button to send the modified rules to the firewall, then click the “Activate” button.</span></span>

<span data-ttu-id="21d0d-183">S aktivace sada pravidel brány firewall pro tento příklad prostředí sestavení je dokončena.</span><span class="sxs-lookup"><span data-stu-id="21d0d-183">With the activation of the firewall ruleset this example environment build is complete.</span></span> <span data-ttu-id="21d0d-184">Volitelně lze spustit skripty sestavení post v části odkazy a přidání aplikace do tohoto prostředí pro testování níže provoz scénáře.</span><span class="sxs-lookup"><span data-stu-id="21d0d-184">Optionally, the post build scripts in the References section can be run to add an application to this environment to test the below traffic scenarios.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21d0d-185">Je důležité si uvědomit, že nedosáhnete stropu webového serveru přímo.</span><span class="sxs-lookup"><span data-stu-id="21d0d-185">It is critical to realize that you will not hit the web server directly.</span></span> <span data-ttu-id="21d0d-186">Pokud prohlížeč požaduje stránku HTTP z FrontEnd001.CloudApp.Net, koncový bod HTTP (port 80) předá tento provoz není webový server do brány firewall.</span><span class="sxs-lookup"><span data-stu-id="21d0d-186">When a browser requests an HTTP page from FrontEnd001.CloudApp.Net, the HTTP endpoint (port 80) passes this traffic to the firewall not the web server.</span></span> <span data-ttu-id="21d0d-187">Brána firewall pak podle pravidla vytvořili výše, zařízení NAT, které požadují k webovému serveru.</span><span class="sxs-lookup"><span data-stu-id="21d0d-187">The firewall then, according to the rule created above, NATs that request to the Web Server.</span></span>
> 
> 

## <a name="traffic-scenarios"></a><span data-ttu-id="21d0d-188">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="21d0d-188">Traffic Scenarios</span></span>
#### <a name="allowed-web-to-web-server-through-firewall"></a><span data-ttu-id="21d0d-189">(Povoleno) Web pro webový Server přes bránu Firewall</span><span class="sxs-lookup"><span data-stu-id="21d0d-189">(Allowed) Web to Web Server through Firewall</span></span>
1. <span data-ttu-id="21d0d-190">Internet uživatelské požadavky HTTP stránky z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="21d0d-190">Internet user requests HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="21d0d-191">Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 k rozhraní místní brány firewall na 10.0.1.4:80</span><span class="sxs-lookup"><span data-stu-id="21d0d-191">Cloud service passes traffic through open endpoint on port 80 to firewall local interface on 10.0.1.4:80</span></span>
3. <span data-ttu-id="21d0d-192">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-192">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="21d0d-193">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="21d0d-193">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="21d0d-194">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="21d0d-194">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="21d0d-195">Použít NSG pravidla 3 (Internet do brány Firewall), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="21d0d-195">NSG Rule 3 (Internet to Firewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="21d0d-196">Provoz dotkne interní IP adresu brány firewall (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="21d0d-196">Traffic hits internal IP address of the firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="21d0d-197">To je provoz na portu 80, přesměruje na webový server IIS01 najdete v části předávání pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="21d0d-197">Firewall forwarding rule see this is port 80 traffic, redirects it to the web server IIS01</span></span>
6. <span data-ttu-id="21d0d-198">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku</span><span class="sxs-lookup"><span data-stu-id="21d0d-198">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
7. <span data-ttu-id="21d0d-199">IIS01 systému SQL Server na AppVM01 vyzve k zadání informací</span><span class="sxs-lookup"><span data-stu-id="21d0d-199">IIS01 asks the SQL Server on AppVM01 for information</span></span>
8. <span data-ttu-id="21d0d-200">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="21d0d-200">No outbound rules on Frontend subnet, traffic is allowed</span></span>
9. <span data-ttu-id="21d0d-201">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-201">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="21d0d-202">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="21d0d-202">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="21d0d-203">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="21d0d-203">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="21d0d-204">Není použít, přejděte k další pravidla NSG pravidla 3 (Internet do brány Firewall)</span><span class="sxs-lookup"><span data-stu-id="21d0d-204">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="21d0d-205">Skupina NSG pravidla 4 použít (IIS01 k AppVM01), provoz je povolený, zastavte zpracování pravidla</span><span class="sxs-lookup"><span data-stu-id="21d0d-205">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
10. <span data-ttu-id="21d0d-206">AppVM01 přijme příkaz jazyka SQL a odpovídá</span><span class="sxs-lookup"><span data-stu-id="21d0d-206">AppVM01 receives the SQL Query and responds</span></span>
11. <span data-ttu-id="21d0d-207">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti back-end je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="21d0d-207">Since there are no outbound rules on the Backend subnet the response is allowed</span></span>
12. <span data-ttu-id="21d0d-208">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-208">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="21d0d-209">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="21d0d-209">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="21d0d-210">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povolen.</span><span class="sxs-lookup"><span data-stu-id="21d0d-210">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
13. <span data-ttu-id="21d0d-211">Server služby IIS obdrží odpověď SQL a dokončení odpovědi HTTP a odešle do žadatel</span><span class="sxs-lookup"><span data-stu-id="21d0d-211">The IIS server receives the SQL response and completes the HTTP response and sends to the requestor</span></span>
14. <span data-ttu-id="21d0d-212">Vzhledem k tomu, že je relace NAT z brány firewall, cílový odpovědi (původně) je pro bránu Firewall</span><span class="sxs-lookup"><span data-stu-id="21d0d-212">Since this is a NAT session from the firewall, the response destination (initially) is for the Firewall</span></span>
15. <span data-ttu-id="21d0d-213">Bránu firewall, obdrží odpověď z webového serveru a předá zpět do Internetu uživatele</span><span class="sxs-lookup"><span data-stu-id="21d0d-213">The firewall receives the response from the Web Server and forwards back to the Internet User</span></span>
16. <span data-ttu-id="21d0d-214">Vzhledem k tomu, že neexistují žádná odchozí pravidla na podsíť Frontend odpověď je povoleno a Internet uživatel obdrží požadované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="21d0d-214">Since there are no outbound rules on the Frontend subnet the response is allowed, and the Internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-backend"></a><span data-ttu-id="21d0d-215">(Povoleno) RDP na back-end</span><span class="sxs-lookup"><span data-stu-id="21d0d-215">(Allowed) RDP to Backend</span></span>
1. <span data-ttu-id="21d0d-216">Správce serveru na Internetu požadavky protokolu RDP relace AppVM01 na BackEnd001.CloudApp.Net:xxxxx kde xxxxx je číslo náhodně přidělenému portu pro protokol RDP na AppVM01 (přidělenému portu naleznete na portálu Azure nebo pomocí prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="21d0d-216">Server Admin on internet requests RDP session to AppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is the randomly assigned port number for RDP to AppVM01 (the assigned port can be found on the Azure Portal or via PowerShell)</span></span>
2. <span data-ttu-id="21d0d-217">Vzhledem k tomu, že brána Firewall naslouchá jenom na adresu FrontEnd001.CloudApp.Net, není zapojená tento tok provozu</span><span class="sxs-lookup"><span data-stu-id="21d0d-217">Since the Firewall is only listening on the FrontEnd001.CloudApp.Net address, it is not involved with this traffic flow</span></span>
3. <span data-ttu-id="21d0d-218">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-218">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="21d0d-219">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="21d0d-219">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="21d0d-220">Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="21d0d-220">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="21d0d-221">Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="21d0d-221">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="21d0d-222">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="21d0d-222">RDP session is enabled</span></span>
6. <span data-ttu-id="21d0d-223">AppVM01 vyzve k zadání názvu heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="21d0d-223">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="21d0d-224">(Povoleno) Webový Server DNS vyhledávání na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="21d0d-224">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="21d0d-225">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale musí pro překlad adres.</span><span class="sxs-lookup"><span data-stu-id="21d0d-225">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="21d0d-226">Konfigurace sítě pro virtuální síť seznamy DNS01 (10.0.2.4 v podsíti back-end) jako primární server DNS, IIS01 odešle žádost DNS do DNS01</span><span class="sxs-lookup"><span data-stu-id="21d0d-226">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="21d0d-227">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="21d0d-227">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="21d0d-228">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-228">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="21d0d-229">Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="21d0d-229">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="21d0d-230">DNS server obdrží požadavek</span><span class="sxs-lookup"><span data-stu-id="21d0d-230">DNS server receives the request</span></span>
6. <span data-ttu-id="21d0d-231">DNS server nemá adresu do mezipaměti a požádá kořenový server DNS na Internetu</span><span class="sxs-lookup"><span data-stu-id="21d0d-231">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="21d0d-232">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="21d0d-232">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="21d0d-233">Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="21d0d-233">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="21d0d-234">DNS server odpověď do mezipaměti a reaguje na počáteční požadavek zpět na IIS01</span><span class="sxs-lookup"><span data-stu-id="21d0d-234">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="21d0d-235">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="21d0d-235">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="21d0d-236">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-236">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="21d0d-237">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="21d0d-237">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="21d0d-238">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povoleno</span><span class="sxs-lookup"><span data-stu-id="21d0d-238">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="21d0d-239">IIS01 obdrží odpověď od DNS01</span><span class="sxs-lookup"><span data-stu-id="21d0d-239">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="21d0d-240">(Povoleno) Přístup k souboru webového serveru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="21d0d-240">(Allowed) Web Server access file on AppVM01</span></span>
1. <span data-ttu-id="21d0d-241">IIS01 požádá o soubor na AppVM01</span><span class="sxs-lookup"><span data-stu-id="21d0d-241">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="21d0d-242">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="21d0d-242">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="21d0d-243">Podsíť back-end zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-243">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="21d0d-244">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="21d0d-244">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="21d0d-245">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="21d0d-245">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="21d0d-246">Není použít, přejděte k další pravidla NSG pravidla 3 (Internet do brány Firewall)</span><span class="sxs-lookup"><span data-stu-id="21d0d-246">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="21d0d-247">Skupina NSG pravidla 4 použít (IIS01 k AppVM01), provoz je povolený, zastavte zpracování pravidla</span><span class="sxs-lookup"><span data-stu-id="21d0d-247">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="21d0d-248">AppVM01 obdrží požadavek a odpoví souboru (za předpokladu, že je autorizovaný přístup)</span><span class="sxs-lookup"><span data-stu-id="21d0d-248">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="21d0d-249">Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti back-end je povoleno odpovědi</span><span class="sxs-lookup"><span data-stu-id="21d0d-249">Since there are no outbound rules on the Backend subnet the response is allowed</span></span>
6. <span data-ttu-id="21d0d-250">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-250">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="21d0d-251">Neexistuje žádná skupina NSG pravidlo, které platí pro příchozí provoz z back-end podsítě pro podsíť Frontend, aby žádný z NSG pravidla použít</span><span class="sxs-lookup"><span data-stu-id="21d0d-251">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
   2. <span data-ttu-id="21d0d-252">Výchozí pravidlo systému umožňuje provoz mezi podsítěmi by povolit tento provoz, provoz je povolen.</span><span class="sxs-lookup"><span data-stu-id="21d0d-252">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="21d0d-253">Server služby IIS obdrží soubor</span><span class="sxs-lookup"><span data-stu-id="21d0d-253">The IIS server receives the file</span></span>

#### <a name="denied-web-direct-to-web-server"></a><span data-ttu-id="21d0d-254">(Byl odepřen) Web přímo na webovém serveru</span><span class="sxs-lookup"><span data-stu-id="21d0d-254">(Denied) Web direct to Web Server</span></span>
<span data-ttu-id="21d0d-255">Vzhledem k tomu, že webový Server, IIS01 a brány Firewall jsou ve stejné cloudové službě sdílejí stejnou veřejné přístupných IP adresu.</span><span class="sxs-lookup"><span data-stu-id="21d0d-255">Since the Web Server, IIS01, and the Firewall are in the same Cloud Service they share the same public facing IP address.</span></span> <span data-ttu-id="21d0d-256">Přenosy dat protokolu HTTP by proto přesměrováni do brány firewall.</span><span class="sxs-lookup"><span data-stu-id="21d0d-256">Thus any HTTP traffic would be directed to the firewall.</span></span> <span data-ttu-id="21d0d-257">Při požadavku by úspěšně zprostředkoval, nelze přejít přímo na webovém serveru, je předán, tak, jak má, přes bránu Firewall nejdřív.</span><span class="sxs-lookup"><span data-stu-id="21d0d-257">While the request would be successfully served, it cannot go directly to the Web Server, it passed, as designed, through the Firewall first.</span></span> <span data-ttu-id="21d0d-258">V tématu prvního scénáře v této části pro tok přenosů.</span><span class="sxs-lookup"><span data-stu-id="21d0d-258">See the first Scenario in this section for the traffic flow.</span></span>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="21d0d-259">(Byl odepřen) Webový server back-end</span><span class="sxs-lookup"><span data-stu-id="21d0d-259">(Denied) Web to Backend Server</span></span>
1. <span data-ttu-id="21d0d-260">Internet uživatel pokusí přistoupit k souboru na AppVM01 prostřednictvím služby BackEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="21d0d-260">Internet user tries to access a file on AppVM01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="21d0d-261">Vzhledem k tomu, že jsou pro sdílené složky otevřené žádné koncové body, se nebude předat cloudové služby a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="21d0d-261">Since there are no endpoints open for file share, this would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="21d0d-262">Pokud z nějakého důvodu byly otevřené koncových bodů, by tento provoz blokovat pravidla NSG 5 (Internet do virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="21d0d-262">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-lookup-on-dns-server"></a><span data-ttu-id="21d0d-263">(Byl odepřen) Vyhledávání DNS web na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="21d0d-263">(Denied) Web DNS lookup on DNS server</span></span>
1. <span data-ttu-id="21d0d-264">Internet uživatel se pokusí vyhledat interní DNS záznam na DNS01 prostřednictvím služby BackEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="21d0d-264">Internet user tries to lookup an internal DNS record on DNS01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="21d0d-265">Vzhledem k tomu, že jsou pro DNS otevřené žádné koncové body, se nebude předat cloudové služby a nebude moci připojit k serveru</span><span class="sxs-lookup"><span data-stu-id="21d0d-265">Since there are no endpoints open for DNS, this would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="21d0d-266">Pokud z nějakého důvodu byly otevřené koncových bodů, pravidla NSG 5 (Internet do virtuální sítě) by blokovat tento provoz (Poznámka: Tento pravidlo 1 (DNS) nebude použít dvou důvodů, nejprve zdrojové adresy je Internetu, toto pravidlo se vztahuje pouze na místní virtuální síť jako zdroj, také to je pravidlo povolit, protože by nikdy zakážou provoz)</span><span class="sxs-lookup"><span data-stu-id="21d0d-266">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first the source address is the internet, this rule only applies to the local VNet as the source, also this is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-to-sql-access-through-firewall"></a><span data-ttu-id="21d0d-267">(Byl odepřen) Webový přístup SQL pomocí brány Firewall</span><span class="sxs-lookup"><span data-stu-id="21d0d-267">(Denied) Web to SQL access through Firewall</span></span>
1. <span data-ttu-id="21d0d-268">Internet uživatel požádá o dat SQL z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="21d0d-268">Internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="21d0d-269">Vzhledem k tomu, že nejsou otevřené pro SQL koncové body, se nebude předat cloudové služby a nebude kontaktovat bránu firewall</span><span class="sxs-lookup"><span data-stu-id="21d0d-269">Since there are no endpoints open for SQL, this would not pass the Cloud Service and wouldn’t reach the firewall</span></span>
3. <span data-ttu-id="21d0d-270">Pokud z nějakého důvodu byly otevřené koncových bodů, podsíť Frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="21d0d-270">If endpoints were open for some reason, the Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="21d0d-271">Není použít, přejděte k další pravidla NSG pravidlo 1 (DNS)</span><span class="sxs-lookup"><span data-stu-id="21d0d-271">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="21d0d-272">Není použít, přejděte k další pravidla NSG pravidlo 2 (RDP)</span><span class="sxs-lookup"><span data-stu-id="21d0d-272">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="21d0d-273">Použít NSG pravidla 2 (Internet do brány Firewall), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="21d0d-273">NSG Rule 2 (Internet to Firewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="21d0d-274">Provoz dotkne interní IP adresu brány firewall (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="21d0d-274">Traffic hits internal IP address of the firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="21d0d-275">Brány firewall pro SQL nemá žádná pravidla pro předávání a zahodí provoz</span><span class="sxs-lookup"><span data-stu-id="21d0d-275">Firewall has no forwarding rules for SQL and drops the traffic</span></span>

## <a name="conclusion"></a><span data-ttu-id="21d0d-276">Závěr</span><span class="sxs-lookup"><span data-stu-id="21d0d-276">Conclusion</span></span>
<span data-ttu-id="21d0d-277">Toto je poměrně splněny následující způsob ochraně vaší aplikace s bránou firewall a izolace back-end podsíť z příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="21d0d-277">This is a relatively straight forward way of protecting your application with a firewall and isolating the back end subnet from inbound traffic.</span></span>

<span data-ttu-id="21d0d-278">Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].</span><span class="sxs-lookup"><span data-stu-id="21d0d-278">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="21d0d-279">Odkazy</span><span class="sxs-lookup"><span data-stu-id="21d0d-279">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="21d0d-280">Hlavní skript a konfiguraci sítě</span><span class="sxs-lookup"><span data-stu-id="21d0d-280">Main Script and Network Config</span></span>
<span data-ttu-id="21d0d-281">Uložte úplné skript v souboru skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21d0d-281">Save the Full Script in a PowerShell script file.</span></span> <span data-ttu-id="21d0d-282">Uložte konfiguraci sítě do souboru s názvem "NetworkConf2.xml".</span><span class="sxs-lookup"><span data-stu-id="21d0d-282">Save the Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="21d0d-283">Podle potřeby změňte proměnné definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="21d0d-283">Modify the user defined variables as needed.</span></span> <span data-ttu-id="21d0d-284">Spusťte skript a potom postupujte podle pokynů instalace pravidlo brány Firewall výše.</span><span class="sxs-lookup"><span data-stu-id="21d0d-284">Run the script, then follow the Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="21d0d-285">Úplné skriptu</span><span class="sxs-lookup"><span data-stu-id="21d0d-285">Full Script</span></span>
<span data-ttu-id="21d0d-286">Tento skript bude na základě uživatelsky definované proměnných:</span><span class="sxs-lookup"><span data-stu-id="21d0d-286">This script will, based on the user defined variables:</span></span>

1. <span data-ttu-id="21d0d-287">Připojení k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="21d0d-287">Connect to an Azure subscription</span></span>
2. <span data-ttu-id="21d0d-288">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="21d0d-288">Create a new storage account</span></span>
3. <span data-ttu-id="21d0d-289">Vytvořit novou virtuální síť a dvě podsítě, jak jsou definovány v souboru konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="21d0d-289">Create a new VNet and two subnets as defined in the Network Config file</span></span>
4. <span data-ttu-id="21d0d-290">Sestavení 4 windows server virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="21d0d-290">Build 4 windows server VMs</span></span>
5. <span data-ttu-id="21d0d-291">Konfigurace, včetně NSG:</span><span class="sxs-lookup"><span data-stu-id="21d0d-291">Configure NSG including:</span></span>
   * <span data-ttu-id="21d0d-292">Vytváření skupina NSG</span><span class="sxs-lookup"><span data-stu-id="21d0d-292">Creating a NSG</span></span>
   * <span data-ttu-id="21d0d-293">Sestavování s pravidly</span><span class="sxs-lookup"><span data-stu-id="21d0d-293">Populating it with rules</span></span>
   * <span data-ttu-id="21d0d-294">Vytvoření vazby skupinu NSG na příslušné podsítě</span><span class="sxs-lookup"><span data-stu-id="21d0d-294">Binding the NSG to the appropriate subnets</span></span>

<span data-ttu-id="21d0d-295">Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.</span><span class="sxs-lookup"><span data-stu-id="21d0d-295">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21d0d-296">Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21d0d-296">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="21d0d-297">Pouze chybové zprávy červeně jsou příčinou problém.</span><span class="sxs-lookup"><span data-stu-id="21d0d-297">Only error messages in red are cause for concern.</span></span>
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
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
       myFirewall - 10.0.1.4
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
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

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
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
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


#### <a name="network-config-file"></a><span data-ttu-id="21d0d-298">Soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="21d0d-298">Network Config File</span></span>
<span data-ttu-id="21d0d-299">Uložte tento soubor xml s aktualizované umístění a přidat odkaz na tohoto souboru do $NetworkConfigFile proměnné ve skriptu výše.</span><span class="sxs-lookup"><span data-stu-id="21d0d-299">Save this xml file with updated location and add the link to this file to the $NetworkConfigFile variable in the script above.</span></span>

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

#### <a name="sample-application-scripts"></a><span data-ttu-id="21d0d-300">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="21d0d-300">Sample Application Scripts</span></span>
<span data-ttu-id="21d0d-301">Pokud chcete nainstalovat ukázkovou aplikaci pro toto a další příklady hraniční sítě, jednu bylo zadáno na následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="21d0d-301">If you wish to install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
<span data-ttu-id="21d0d-302">[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Příchozí DMZ s NSG"</span><span class="sxs-lookup"><span data-stu-id="21d0d-302">[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inbound DMZ with NSG"</span></span>
<span data-ttu-id="21d0d-303">[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Ikona cílové NAT"</span><span class="sxs-lookup"><span data-stu-id="21d0d-303">[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Destination NAT Icon"</span></span>
<span data-ttu-id="21d0d-304">[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Pravidlo brány firewall"</span><span class="sxs-lookup"><span data-stu-id="21d0d-304">[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewall Rule"</span></span>
<span data-ttu-id="21d0d-305">[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Aktivace pravidla brány firewall"</span><span class="sxs-lookup"><span data-stu-id="21d0d-305">[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Firewall Rule Activation"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
