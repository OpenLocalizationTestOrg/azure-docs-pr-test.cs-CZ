---
title: "aaaDMZ příklad – vytváření DMZ tooprotect aplikací s bránou Firewall a skupin Nsg | Microsoft Docs"
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
ms.openlocfilehash: 18f116dc3897567bff14a509ae8c13f449182bfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-2--build-a-dmz-tooprotect-applications-with-a-firewall-and-nsgs"></a><span data-ttu-id="947a0-103">Příklad 2 – vytváření DMZ tooprotect aplikací s bránou Firewall a skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="947a0-103">Example 2 – Build a DMZ tooprotect applications with a Firewall and NSGs</span></span>
<span data-ttu-id="947a0-104">[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="947a0-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="947a0-105">Tento příklad vytvoří DMZ s bránou firewall, čtyři windows serverů a skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="947a0-105">This example will create a DMZ with a firewall, four windows servers, and Network Security Groups.</span></span> <span data-ttu-id="947a0-106">Je také provede všechny relevantní příkazy tooprovide hello podrobnější vysvětlení jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="947a0-106">It will also walk through each of hello relevant commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="947a0-107">K dispozici je také provozu scénář části tooprovide na podrobné podrobné jak provoz pokračuje prostřednictvím hello vrstev obrany ve hello hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="947a0-107">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="947a0-108">Nakonec v části odkazy hello je kompletní kód hello a instrukce toobuild tento tootest prostředí a experimentů pomocí různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="947a0-108">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="947a0-109">![Příchozí DMZ s hodnocení chyb zabezpečení a NSG][1]</span><span class="sxs-lookup"><span data-stu-id="947a0-109">![Inbound DMZ with NVA and NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="947a0-110">Popis prostředí</span><span class="sxs-lookup"><span data-stu-id="947a0-110">Environment Description</span></span>
<span data-ttu-id="947a0-111">V tomto příkladu je předplatné, které obsahuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="947a0-111">In this example there is a subscription that contains hello following:</span></span>

* <span data-ttu-id="947a0-112">Dva cloudové služby: "FrontEnd001" a "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="947a0-112">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="947a0-113">Virtuální síť "CorpNetwork" se dvěma podsítěmi: "FrontEnd" a "Back-end"</span><span class="sxs-lookup"><span data-stu-id="947a0-113">A Virtual Network “CorpNetwork”, with two subnets: “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="947a0-114">Jednu skupinu zabezpečení sítě, která je použitá tooboth podsítě</span><span class="sxs-lookup"><span data-stu-id="947a0-114">A single Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="947a0-115">Virtuální zařízení sítě, v tomto příkladu Barracuda NextGen Firewall, připojení toohello podsíť Frontend</span><span class="sxs-lookup"><span data-stu-id="947a0-115">A network virtual appliance, in this example a Barracuda NextGen Firewall, connected toohello Frontend subnet</span></span>
* <span data-ttu-id="947a0-116">Windows Server, který představuje server webových aplikací ("IIS01")</span><span class="sxs-lookup"><span data-stu-id="947a0-116">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="947a0-117">Dva windows servery, které představují aplikace zpět ukončení servery ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="947a0-117">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="947a0-118">Windows server, který představuje server DNS ("DNS01")</span><span class="sxs-lookup"><span data-stu-id="947a0-118">A Windows server that represents a DNS server (“DNS01”)</span></span>

> [!NOTE]
> <span data-ttu-id="947a0-119">I když tento příklad používá Barracuda NextGen Firewall, řadu hello různých síťových virtuálních zařízení může být použita pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="947a0-119">Although this example uses a Barracuda NextGen Firewall, many of hello different Network Virtual Appliances could be used for this example.</span></span>
> 
> 

<span data-ttu-id="947a0-120">V části hello odkazy níže je skript prostředí PowerShell, který bude vytvořit většinu hello prostředí popsané výše.</span><span class="sxs-lookup"><span data-stu-id="947a0-120">In hello references section below there is a PowerShell script that will build most of hello environment described above.</span></span> <span data-ttu-id="947a0-121">Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádějí hello ukázkový skript, nejsou popsané podrobně v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="947a0-121">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span>

<span data-ttu-id="947a0-122">toobuild hello prostředí:</span><span class="sxs-lookup"><span data-stu-id="947a0-122">toobuild hello environment:</span></span>

1. <span data-ttu-id="947a0-123">Uložit hello sítě konfigurační soubor xml v oddíle odkazy hello (aktualizovat název, umístění a IP adresy toomatch hello zadané scénář)</span><span class="sxs-lookup"><span data-stu-id="947a0-123">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="947a0-124">Aktualizace hello uživatelské proměnné ve hello skriptu toomatch hello prostředí hello skriptu je toobe spouštění (odběry, názvy služeb atd.)</span><span class="sxs-lookup"><span data-stu-id="947a0-124">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="947a0-125">Spuštění skriptu hello v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="947a0-125">Execute hello script in PowerShell</span></span>

<span data-ttu-id="947a0-126">**Poznámka:**: oblast hello označený ve hello skript prostředí PowerShell musí odpovídat hello oblast označený ve hello sítě konfigurační soubor xml.</span><span class="sxs-lookup"><span data-stu-id="947a0-126">**Note**: hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>

<span data-ttu-id="947a0-127">Po úspěšném spuštění skriptu hello mohou být přijata hello kroků po skriptu:</span><span class="sxs-lookup"><span data-stu-id="947a0-127">Once hello script runs successfully hello following post-script steps may be taken:</span></span>

1. <span data-ttu-id="947a0-128">Nastavit hello pravidla brány firewall, najdete v části hello níže, s názvem: pravidla brány Firewall.</span><span class="sxs-lookup"><span data-stu-id="947a0-128">Set up hello firewall rules, this is covered in hello section below titled: Firewall Rules.</span></span>
2. <span data-ttu-id="947a0-129">Volitelně můžete v části odkazy hello jsou dva skripty tooset hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="947a0-129">Optionally in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="947a0-130">Hello další části se dozvíte většinu hello skripty příkazy relativní tooNetwork skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="947a0-130">hello next section explains most of hello scripts statements relative tooNetwork Security Groups.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="947a0-131">Skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="947a0-131">Network Security Groups (NSG)</span></span>
<span data-ttu-id="947a0-132">V tomto příkladu je skupina NSG vytvořené a pak načtená šesti pravidla.</span><span class="sxs-lookup"><span data-stu-id="947a0-132">For this example, a NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="947a0-133">Obecně platí musí nejprve vytvořit konkrétní pravidel "Povolit" a pak poslední hello více obecná pravidla "Deny".</span><span class="sxs-lookup"><span data-stu-id="947a0-133">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="947a0-134">Hello prioritou stanoví, která pravidla se vyhodnocují jako první.</span><span class="sxs-lookup"><span data-stu-id="947a0-134">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="947a0-135">Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla.</span><span class="sxs-lookup"><span data-stu-id="947a0-135">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="947a0-136">Pravidla NSG můžete použít buď v hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).</span><span class="sxs-lookup"><span data-stu-id="947a0-136">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
> 
> 

<span data-ttu-id="947a0-137">Deklarativně jsou pro příchozí provoz sestavuje hello následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="947a0-137">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="947a0-138">Interní DNS provoz (port 53) je povolený</span><span class="sxs-lookup"><span data-stu-id="947a0-138">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="947a0-139">Provoz protokolu RDP (portu 3389) z Internetu tooany hello virtuálních počítačů je povoleno.</span><span class="sxs-lookup"><span data-stu-id="947a0-139">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="947a0-140">Je povolen přenos HTTP (port 80) z Internetu toohello hello hodnocení chyb zabezpečení sítě (brány firewall)</span><span class="sxs-lookup"><span data-stu-id="947a0-140">HTTP traffic (port 80) from hello Internet toohello NVA (firewall) is allowed</span></span>
4. <span data-ttu-id="947a0-141">Jakýkoli přenos (všechny porty) z IIS01 tooAppVM1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="947a0-141">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="947a0-142">Jakýkoli přenos (všechny porty) z Internetu toohello hello celý virtuální síť (obě podsítě) byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="947a0-142">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="947a0-143">Jakýkoli přenos (všechny porty) z hello front-endu podsíť toohello back-end podsítě byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="947a0-143">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="947a0-144">Pomocí těchto pravidel vázané tooeach podsíti, pokud požadavek HTTP byl příchozí z hello Internet toohello webový server, obě pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit.</span><span class="sxs-lookup"><span data-stu-id="947a0-144">With these rules bound tooeach subnet, if a HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="947a0-145">Proto hello HTTP požadavku bude mít možnost toohello brány firewall.</span><span class="sxs-lookup"><span data-stu-id="947a0-145">Thus hello HTTP request would be allowed toohello firewall.</span></span> <span data-ttu-id="947a0-146">Pokud tento stejný provoz pokoušel tooreach hello DNS01 server, pravidlo 5 (Odepřít) bude, že hello první tooapply hello přenosů dat a nebude možné toopass toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="947a0-146">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="947a0-147">Pravidla 6 (Odepřít) bloky hello front-endu podsíť z rozhovoru toohello back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), to v případě, že útočník ohrožení hello webovou aplikaci na hello front-endu, útočník hello být chrání síť back-end hello omezený přístup toohello back-end "chráněna" sítě (jenom tooresources zveřejněné na serveru AppVM01 hello).</span><span class="sxs-lookup"><span data-stu-id="947a0-147">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="947a0-148">Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="947a0-148">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="947a0-149">V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="947a0-149">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="947a0-150">toolock dolů provoz v obou směrech směrování definovaná uživatele je vyžadován, to je prozkoumali v různých příkladu, který lze nalézt v hello [hlavní zabezpečení hranic dokumentu][HOME].</span><span class="sxs-lookup"><span data-stu-id="947a0-150">toolock down traffic in both directions, User Defined Routing is required, this is explored in a different example that can found in hello [main security boundary document][HOME].</span></span>

<span data-ttu-id="947a0-151">pravidla NSG Hello výše popsané jsou velmi podobné pravidla NSG toohello v [Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg][Example1].</span><span class="sxs-lookup"><span data-stu-id="947a0-151">hello above discussed NSG rules are very similar toohello NSG rules in [Example 1 - Build a Simple DMZ with NSGs][Example1].</span></span> <span data-ttu-id="947a0-152">Přečtěte si hello popis NSG v tomto dokumentu pro podrobný pohled na každé pravidlo NSG a jeho atributy.</span><span class="sxs-lookup"><span data-stu-id="947a0-152">Please review hello NSG Description in that document for a detailed look at each NSG rule and it's attributes.</span></span>

## <a name="firewall-rules"></a><span data-ttu-id="947a0-153">Pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="947a0-153">Firewall Rules</span></span>
<span data-ttu-id="947a0-154">Klient správy bude potřebovat toobe nainstalovaný na počítači toomanage hello firewall a vytvoření konfigurací hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="947a0-154">A management client will need toobe installed on a PC toomanage hello firewall and create hello configurations needed.</span></span> <span data-ttu-id="947a0-155">V tom, jak toomanage hello zařízení najdete v článku dodavatele hello dokumentace z brány firewall (nebo jiné hodnocení chyb zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="947a0-155">See hello documentation from your firewall (or other NVA) vendor on how toomanage hello device.</span></span> <span data-ttu-id="947a0-156">Hello zbytek této části se popisují hello konfigurace brány firewall hello, samostatně, prostřednictvím klienta pro správu dodavatelé hello (tzn. ne hello portál Azure nebo PowerShell).</span><span class="sxs-lookup"><span data-stu-id="947a0-156">hello remainder of this section will describe hello configuration of hello firewall itself, through hello vendors management client (i.e. not hello Azure portal or PowerShell).</span></span>

<span data-ttu-id="947a0-157">Pokyny pro stažení klienta a připojování toohello Barracuda použité v tomto příkladu naleznete zde: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="947a0-157">Instructions for client download and connecting toohello Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="947a0-158">V bráně firewall hello předávání pravidla potřebovat toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="947a0-158">On hello firewall, forwarding rules will need toobe created.</span></span> <span data-ttu-id="947a0-159">Vzhledem k tomu, že v tomto příkladu pouze směruje provoz v vázané toohello brána firewall pro přístup k Internetu a pak toohello webový server, je potřeba jenom jeden předávání pravidlo NAT.</span><span class="sxs-lookup"><span data-stu-id="947a0-159">Since this example only routes internet traffic in-bound toohello firewall and then toohello web server, only one forwarding NAT rule is needed.</span></span> <span data-ttu-id="947a0-160">Na hello Barracuda NextGen Firewall používá v hello tento příklad bude pravidlo NAT cílové tento provoz pravidlo toopass ("Dst NAT").</span><span class="sxs-lookup"><span data-stu-id="947a0-160">On hello Barracuda NextGen Firewall used in this example hello rule would be a Destination NAT rule (“Dst NAT”) toopass this traffic.</span></span>

<span data-ttu-id="947a0-161">toocreate hello následující pravidla (nebo ověřit existující výchozí pravidla), od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfigurace oddíl, klikněte na Ruleset.</span><span class="sxs-lookup"><span data-stu-id="947a0-161">toocreate hello following rule (or verify existing default rules), starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="947a0-162">Mřížka názvem, zobrazí "Hlavní pravidla" hello existující aktivní a deaktivované pravidla v bráně firewall hello.</span><span class="sxs-lookup"><span data-stu-id="947a0-162">A grid called, “Main Rules” will show hello existing active and deactivated rules on hello firewall.</span></span> <span data-ttu-id="947a0-163">V horním pravém rohu hello tento mřížky je malý, zelená "+" tlačítko, klikněte na tento toocreate nové pravidlo (Poznámka: Brána firewall může být "uzamčen" změny, pokud se zobrazí tlačítko označené "Zamknout" a jsou nelze toocreate nebo upravit pravidla, klikněte na toto tlačítko příliš "odemknutí" hello Sada pravidel pro a povolení úprav).</span><span class="sxs-lookup"><span data-stu-id="947a0-163">In hello upper right corner of this grid is a small, green “+” button, click this toocreate a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable toocreate or edit rules, click this button too“unlock” hello ruleset and allow editing).</span></span> <span data-ttu-id="947a0-164">Pokud chcete tooedit existující pravidlo, vyberte toto pravidlo, klikněte pravým tlačítkem a vyberte Upravit pravidlo.</span><span class="sxs-lookup"><span data-stu-id="947a0-164">If you wish tooedit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="947a0-165">Vytvořit nové pravidlo a zadejte název, jako je například "WebTraffic".</span><span class="sxs-lookup"><span data-stu-id="947a0-165">Create a new rule and provide a name, such as "WebTraffic".</span></span> 

<span data-ttu-id="947a0-166">Ikona pravidlo NAT cílové Hello vypadá takto: ![ikonu cílové NAT][2]</span><span class="sxs-lookup"><span data-stu-id="947a0-166">hello Destination NAT rule icon looks like this: ![Destination NAT Icon][2]</span></span>

<span data-ttu-id="947a0-167">Hello pravidlo samotné by vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="947a0-167">hello rule itself would look something like this:</span></span>

<span data-ttu-id="947a0-168">![Pravidlo brány firewall][3]</span><span class="sxs-lookup"><span data-stu-id="947a0-168">![Firewall Rule][3]</span></span>

<span data-ttu-id="947a0-169">Zde žádné příchozí adresa, hello přístupů k pokusu o tooreach protokolu HTTP (port 80 nebo 443 pro protokol HTTPS) brány Firewall budou odeslána hello "Místní IP adresy DHCP1" rozhraní a přesměrovaného toohello webového serveru pomocí hello 10.0.1.5 IP adresu brány Firewall na.</span><span class="sxs-lookup"><span data-stu-id="947a0-169">Here any inbound address that hits hello Firewall trying tooreach HTTP (port 80 or 443 for HTTPS) will be sent out hello Firewall’s “DHCP1 Local IP” interface and redirected toohello Web Server with hello IP Address of 10.0.1.5.</span></span> <span data-ttu-id="947a0-170">Vzhledem k tomu, že rovinu hello přenosy na portu 80 a probíhající toohello webový server na portu 80 bylo potřeba žádná změna portu.</span><span class="sxs-lookup"><span data-stu-id="947a0-170">Since hello traffic is coming in on port 80 and going toohello web server on port 80 no port change was needed.</span></span> <span data-ttu-id="947a0-171">Dobrý den, kterou je cílového seznamu může 10.0.1.5:8080 Pokud naslouchali naše Webový Server na portu 8080 proto překladu však hello příchozí port 80 na hello brány firewall tooinbound portu 8080 na webovém serveru hello.</span><span class="sxs-lookup"><span data-stu-id="947a0-171">However, hello Target List could have been 10.0.1.5:8080 if our Web Server listened on port 8080 thus translating hello inbound port 80 on hello firewall tooinbound port 8080 on hello web server.</span></span>

<span data-ttu-id="947a0-172">Metoda připojení musí také být označeny, pro hello cílové pravidlo z Internetu, hello "Dynamické překládat pomocí SNAT" je nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="947a0-172">A Connection Method should also be signified, for hello Destination Rule from hello Internet, "Dynamic SNAT" is most appropriate.</span></span> 

<span data-ttu-id="947a0-173">I když existuje pouze jedno pravidlo je důležité, aby jeho priorita je nastavena správně.</span><span class="sxs-lookup"><span data-stu-id="947a0-173">Although only one rule has been created it's important that its priority is set correctly.</span></span> <span data-ttu-id="947a0-174">Pokud v mřížce hello všech pravidel v bráně firewall hello toto nové pravidlo na dolní hello (pod pravidlo "BLOCKALL" hello) vrátí nikdy se do play.</span><span class="sxs-lookup"><span data-stu-id="947a0-174">If in hello grid of all rules on hello firewall this new rule is on hello bottom (below hello "BLOCKALL" rule) it will never come into play.</span></span> <span data-ttu-id="947a0-175">Zkontrolujte, zda pravidlo hello nově vytvořený pro webový provoz výše hello BLOCKALL pravidlo.</span><span class="sxs-lookup"><span data-stu-id="947a0-175">Ensure hello newly created rule for web traffic is above hello BLOCKALL rule.</span></span>

<span data-ttu-id="947a0-176">Po vytvoření pravidla hello musí být nabídnutých toohello brány firewall a pak se aktivuje, pokud to neuděláte hello pravidlo změny se neprojeví.</span><span class="sxs-lookup"><span data-stu-id="947a0-176">Once hello rule is created, it must be pushed toohello firewall and then activated, if this is not done hello rule change will not take effect.</span></span> <span data-ttu-id="947a0-177">Hello nabízení a aktivace proces je popsán v další části hello.</span><span class="sxs-lookup"><span data-stu-id="947a0-177">hello push and activation process is described in hello next section.</span></span>

## <a name="rule-activation"></a><span data-ttu-id="947a0-178">Aktivace pravidla</span><span class="sxs-lookup"><span data-stu-id="947a0-178">Rule Activation</span></span>
<span data-ttu-id="947a0-179">S hello ruleset upravit tooadd toto pravidlo, musí být hello ruleset odeslán toohello brány firewall a aktivovat.</span><span class="sxs-lookup"><span data-stu-id="947a0-179">With hello ruleset modified tooadd this rule, hello ruleset must be uploaded toohello firewall and activated.</span></span>

<span data-ttu-id="947a0-180">![Aktivace pravidla brány firewall][4]</span><span class="sxs-lookup"><span data-stu-id="947a0-180">![Firewall Rule Activation][4]</span></span>

<span data-ttu-id="947a0-181">V pravém horním rohu hello hello správy klienta jsou součástí clusteru tlačítka.</span><span class="sxs-lookup"><span data-stu-id="947a0-181">In hello upper right hand corner of hello management client are a cluster of buttons.</span></span> <span data-ttu-id="947a0-182">Klikněte na tlačítko hello "odeslat změny" tlačítko toosend hello upravit pravidla brány firewall toohello a pak klikněte na tlačítko "Aktivovat" hello.</span><span class="sxs-lookup"><span data-stu-id="947a0-182">Click hello “Send Changes” button toosend hello modified rules toohello firewall, then click hello “Activate” button.</span></span>

<span data-ttu-id="947a0-183">Tento příklad prostředí sestavení je s hello aktivace sada pravidel brány firewall pro hello dokončena.</span><span class="sxs-lookup"><span data-stu-id="947a0-183">With hello activation of hello firewall ruleset this example environment build is complete.</span></span> <span data-ttu-id="947a0-184">Volitelně hello post sestavení v hello odkazy na části lze spustit skripty tooadd aplikace toothis prostředí tootest hello níže provoz scénáře.</span><span class="sxs-lookup"><span data-stu-id="947a0-184">Optionally, hello post build scripts in hello References section can be run tooadd an application toothis environment tootest hello below traffic scenarios.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="947a0-185">Je důležité toorealize hello webový server nebude přímo přístupů.</span><span class="sxs-lookup"><span data-stu-id="947a0-185">It is critical toorealize that you will not hit hello web server directly.</span></span> <span data-ttu-id="947a0-186">Pokud prohlížeč požaduje stránku HTTP z FrontEnd001.CloudApp.Net, hello HTTP koncový bod (port 80) předává tento provoz toohello firewall není hello webový server.</span><span class="sxs-lookup"><span data-stu-id="947a0-186">When a browser requests an HTTP page from FrontEnd001.CloudApp.Net, hello HTTP endpoint (port 80) passes this traffic toohello firewall not hello web server.</span></span> <span data-ttu-id="947a0-187">Hello brány firewall, pak podle pravidla toohello vytvořili výše, zařízení NAT, které požadují toohello Webový Server.</span><span class="sxs-lookup"><span data-stu-id="947a0-187">hello firewall then, according toohello rule created above, NATs that request toohello Web Server.</span></span>
> 
> 

## <a name="traffic-scenarios"></a><span data-ttu-id="947a0-188">Provoz scénáře</span><span class="sxs-lookup"><span data-stu-id="947a0-188">Traffic Scenarios</span></span>
#### <a name="allowed-web-tooweb-server-through-firewall"></a><span data-ttu-id="947a0-189">(Povoleno) TooWeb Webový Server přes bránu Firewall</span><span class="sxs-lookup"><span data-stu-id="947a0-189">(Allowed) Web tooWeb Server through Firewall</span></span>
1. <span data-ttu-id="947a0-190">Internet uživatelské požadavky HTTP stránky z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="947a0-190">Internet user requests HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="947a0-191">Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 toofirewall místní rozhraní na 10.0.1.4:80</span><span class="sxs-lookup"><span data-stu-id="947a0-191">Cloud service passes traffic through open endpoint on port 80 toofirewall local interface on 10.0.1.4:80</span></span>
3. <span data-ttu-id="947a0-192">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-192">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="947a0-193">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-193">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="947a0-194">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-194">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="947a0-195">Použít NSG pravidla 3 (tooFirewall Internetu), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="947a0-195">NSG Rule 3 (Internet tooFirewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="947a0-196">Provoz dotkne interní IP adresu brány firewall hello (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="947a0-196">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="947a0-197">To je provoz na portu 80, přesměruje ho toohello webový server IIS01 najdete v části pravidla brány firewall předávání</span><span class="sxs-lookup"><span data-stu-id="947a0-197">Firewall forwarding rule see this is port 80 traffic, redirects it toohello web server IIS01</span></span>
6. <span data-ttu-id="947a0-198">IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello</span><span class="sxs-lookup"><span data-stu-id="947a0-198">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
7. <span data-ttu-id="947a0-199">IIS01 hello systému SQL Server na AppVM01 vyzve k zadání informací</span><span class="sxs-lookup"><span data-stu-id="947a0-199">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
8. <span data-ttu-id="947a0-200">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="947a0-200">No outbound rules on Frontend subnet, traffic is allowed</span></span>
9. <span data-ttu-id="947a0-201">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-201">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="947a0-202">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-202">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="947a0-203">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-203">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="947a0-204">Skupina NSG pravidla 3 (Internet tooFirewall) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-204">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="947a0-205">Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="947a0-205">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
10. <span data-ttu-id="947a0-206">AppVM01 přijímá hello dotazu SQL a odpoví</span><span class="sxs-lookup"><span data-stu-id="947a0-206">AppVM01 receives hello SQL Query and responds</span></span>
11. <span data-ttu-id="947a0-207">Vzhledem k tomu, že neexistují žádná odchozí pravidla na hello back-end podsíť hello odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="947a0-207">Since there are no outbound rules on hello Backend subnet hello response is allowed</span></span>
12. <span data-ttu-id="947a0-208">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-208">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="947a0-209">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="947a0-209">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="947a0-210">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.</span><span class="sxs-lookup"><span data-stu-id="947a0-210">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
13. <span data-ttu-id="947a0-211">server služby IIS Hello obdrží odpověď hello SQL a dokončí hello odpovědi HTTP a odešle toohello žadatele</span><span class="sxs-lookup"><span data-stu-id="947a0-211">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requestor</span></span>
14. <span data-ttu-id="947a0-212">Vzhledem k tomu, že je relace NAT od hello brány firewall, cíl odpovědi hello (původně) je pro hello brány Firewall</span><span class="sxs-lookup"><span data-stu-id="947a0-212">Since this is a NAT session from hello firewall, hello response destination (initially) is for hello Firewall</span></span>
15. <span data-ttu-id="947a0-213">brány firewall Hello obdrží odpověď hello z hello Webový Server a předává zpět toohello Internet uživatele</span><span class="sxs-lookup"><span data-stu-id="947a0-213">hello firewall receives hello response from hello Web Server and forwards back toohello Internet User</span></span>
16. <span data-ttu-id="947a0-214">Vzhledem k tomu, že nejsou žádná odchozí pravidla na hello odpovědi hello podsítě front-endu je povolen a hello Internet uživatel obdrží webovou stránku hello požadovaný.</span><span class="sxs-lookup"><span data-stu-id="947a0-214">Since there are no outbound rules on hello Frontend subnet hello response is allowed, and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-toobackend"></a><span data-ttu-id="947a0-215">(Povoleno) TooBackend protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="947a0-215">(Allowed) RDP tooBackend</span></span>
1. <span data-ttu-id="947a0-216">Správce serveru na Internetu požadavky tooAppVM01 relace protokolu RDP na BackEnd001.CloudApp.Net:xxxxx kde xxxxx je číslo portu hello náhodně přiřadí tooAppVM01 protokolu RDP (portu přiřazené hello naleznete na hello portálu Azure nebo pomocí prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="947a0-216">Server Admin on internet requests RDP session tooAppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is hello randomly assigned port number for RDP tooAppVM01 (hello assigned port can be found on hello Azure Portal or via PowerShell)</span></span>
2. <span data-ttu-id="947a0-217">Od hello brány Firewall naslouchá jenom na hello FrontEnd001.CloudApp.Net adresu není zapojená tento tok provozu</span><span class="sxs-lookup"><span data-stu-id="947a0-217">Since hello Firewall is only listening on hello FrontEnd001.CloudApp.Net address, it is not involved with this traffic flow</span></span>
3. <span data-ttu-id="947a0-218">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-218">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="947a0-219">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-219">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="947a0-220">Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="947a0-220">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="947a0-221">Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený</span><span class="sxs-lookup"><span data-stu-id="947a0-221">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="947a0-222">Je povoleno relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="947a0-222">RDP session is enabled</span></span>
6. <span data-ttu-id="947a0-223">AppVM01 vyzve k zadání názvu heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="947a0-223">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="947a0-224">(Povoleno) Webový Server DNS vyhledávání na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="947a0-224">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="947a0-225">Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale potřebuje tooresolve hello adresu.</span><span class="sxs-lookup"><span data-stu-id="947a0-225">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="947a0-226">Hello konfiguraci sítě pro virtuální síť seznamy hello DNS01 (10.0.2.4 v podsíti hello back-end) jako primární server DNS hello IIS01 odešle tooDNS01 požadavek DNS hello</span><span class="sxs-lookup"><span data-stu-id="947a0-226">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="947a0-227">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="947a0-227">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="947a0-228">Back-end podsíť zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-228">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="947a0-229">Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="947a0-229">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="947a0-230">DNS server obdrží požadavek hello</span><span class="sxs-lookup"><span data-stu-id="947a0-230">DNS server receives hello request</span></span>
6. <span data-ttu-id="947a0-231">DNS server nemá hello adresu do mezipaměti a požádá kořenový server DNS na hello Internetu</span><span class="sxs-lookup"><span data-stu-id="947a0-231">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="947a0-232">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="947a0-232">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="947a0-233">Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="947a0-233">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="947a0-234">DNS server ukládá do mezipaměti odpovědi hello a odpoví back tooIIS01 toohello úvodního požadavku</span><span class="sxs-lookup"><span data-stu-id="947a0-234">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="947a0-235">Žádná odchozí pravidla na back-end podsítě provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="947a0-235">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="947a0-236">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-236">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="947a0-237">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="947a0-237">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="947a0-238">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený</span><span class="sxs-lookup"><span data-stu-id="947a0-238">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="947a0-239">IIS01 obdrží odpověď hello z DNS01</span><span class="sxs-lookup"><span data-stu-id="947a0-239">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="947a0-240">(Povoleno) Přístup k souboru webového serveru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="947a0-240">(Allowed) Web Server access file on AppVM01</span></span>
1. <span data-ttu-id="947a0-241">IIS01 požádá o soubor na AppVM01</span><span class="sxs-lookup"><span data-stu-id="947a0-241">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="947a0-242">Žádná odchozí pravidla na podsíť Frontend provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="947a0-242">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="947a0-243">podsíť back-end Hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-243">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="947a0-244">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-244">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="947a0-245">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-245">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="947a0-246">Skupina NSG pravidla 3 (Internet tooFirewall) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-246">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="947a0-247">Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="947a0-247">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="947a0-248">AppVM01 obdrží požadavek na hello a odpoví souboru (za předpokladu, že je autorizovaný přístup)</span><span class="sxs-lookup"><span data-stu-id="947a0-248">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="947a0-249">Vzhledem k tomu, že neexistují žádná odchozí pravidla na hello back-end podsíť hello odpovědi je povoleno</span><span class="sxs-lookup"><span data-stu-id="947a0-249">Since there are no outbound rules on hello Backend subnet hello response is allowed</span></span>
6. <span data-ttu-id="947a0-250">Podsíť frontend zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-250">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="947a0-251">Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí</span><span class="sxs-lookup"><span data-stu-id="947a0-251">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
   2. <span data-ttu-id="947a0-252">Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.</span><span class="sxs-lookup"><span data-stu-id="947a0-252">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="947a0-253">server služby IIS Hello obdrží soubor hello</span><span class="sxs-lookup"><span data-stu-id="947a0-253">hello IIS server receives hello file</span></span>

#### <a name="denied-web-direct-tooweb-server"></a><span data-ttu-id="947a0-254">(Byl odepřen) Přímé tooWeb webového serveru</span><span class="sxs-lookup"><span data-stu-id="947a0-254">(Denied) Web direct tooWeb Server</span></span>
<span data-ttu-id="947a0-255">Od hello Webový Server, IIS01 a hello brány Firewall jsou v hello sdílejí stejné cloudové služby hello stejnou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="947a0-255">Since hello Web Server, IIS01, and hello Firewall are in hello same Cloud Service they share hello same public facing IP address.</span></span> <span data-ttu-id="947a0-256">Proto by přesměrovat přenosy HTTP toohello brány firewall.</span><span class="sxs-lookup"><span data-stu-id="947a0-256">Thus any HTTP traffic would be directed toohello firewall.</span></span> <span data-ttu-id="947a0-257">Při požadavku hello by úspěšně zprostředkoval, nemůže přejít přímo toohello Webový Server, je předaná, tak, jak má, prostřednictvím hello brány Firewall nejdřív.</span><span class="sxs-lookup"><span data-stu-id="947a0-257">While hello request would be successfully served, it cannot go directly toohello Web Server, it passed, as designed, through hello Firewall first.</span></span> <span data-ttu-id="947a0-258">V tématu hello prvního scénáře v této části pro tok přenosů hello.</span><span class="sxs-lookup"><span data-stu-id="947a0-258">See hello first Scenario in this section for hello traffic flow.</span></span>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="947a0-259">(Byl odepřen) TooBackend webového serveru</span><span class="sxs-lookup"><span data-stu-id="947a0-259">(Denied) Web tooBackend Server</span></span>
1. <span data-ttu-id="947a0-260">Uživatel Internetu pokusí tooaccess souboru na AppVM01 prostřednictvím hello BackEnd001.CloudApp.Net služby</span><span class="sxs-lookup"><span data-stu-id="947a0-260">Internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="947a0-261">Vzhledem k tomu, že jsou pro sdílené složky otevřené žádné koncové body, se nebude předat hello cloudové služby a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="947a0-261">Since there are no endpoints open for file share, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="947a0-262">Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat pravidla NSG 5 (Internet tooVNet)</span><span class="sxs-lookup"><span data-stu-id="947a0-262">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-lookup-on-dns-server"></a><span data-ttu-id="947a0-263">(Byl odepřen) Vyhledávání DNS web na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="947a0-263">(Denied) Web DNS lookup on DNS server</span></span>
1. <span data-ttu-id="947a0-264">Uživatel Internetu pokusí toolookup interní DNS záznam na DNS01 prostřednictvím hello BackEnd001.CloudApp.Net služby</span><span class="sxs-lookup"><span data-stu-id="947a0-264">Internet user tries toolookup an internal DNS record on DNS01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="947a0-265">Vzhledem k tomu, že jsou pro DNS otevřené žádné koncové body, se nebude předat hello cloudové služby a nebude kontaktovat hello server</span><span class="sxs-lookup"><span data-stu-id="947a0-265">Since there are no endpoints open for DNS, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="947a0-266">Pokud z nějakého důvodu byly otevřené hello koncových bodů, pravidla NSG 5 (tooVNet Internetu) by blokovat tento provoz (Poznámka: Tento pravidlo 1 (DNS) nebude použít dvou důvodů, první zdrojové adresy hello je hello internet, toto pravidlo se vztahuje pouze na toohello virtuální místní síť jako hello také zdroje To je pravidlo povolení, takže by nikdy zakážou provoz)</span><span class="sxs-lookup"><span data-stu-id="947a0-266">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first hello source address is hello internet, this rule only applies toohello local VNet as hello source, also this is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-toosql-access-through-firewall"></a><span data-ttu-id="947a0-267">(Byl odepřen) Webový přístup k tooSQL přes bránu Firewall</span><span class="sxs-lookup"><span data-stu-id="947a0-267">(Denied) Web tooSQL access through Firewall</span></span>
1. <span data-ttu-id="947a0-268">Internet uživatel požádá o dat SQL z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)</span><span class="sxs-lookup"><span data-stu-id="947a0-268">Internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="947a0-269">Vzhledem k tomu, že nejsou otevřené pro SQL koncové body, se nebude předat hello cloudové služby a nebude dosáhnout hello brány firewall</span><span class="sxs-lookup"><span data-stu-id="947a0-269">Since there are no endpoints open for SQL, this would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="947a0-270">Pokud z nějakého důvodu byly otevřené koncových bodů, podsíť Frontend hello zahájí zpracování příchozí pravidlo:</span><span class="sxs-lookup"><span data-stu-id="947a0-270">If endpoints were open for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="947a0-271">Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-271">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="947a0-272">Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo</span><span class="sxs-lookup"><span data-stu-id="947a0-272">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="947a0-273">Použít NSG pravidla 2 (tooFirewall Internetu), Probíhá zpracování povolených, zastavení pravidla</span><span class="sxs-lookup"><span data-stu-id="947a0-273">NSG Rule 2 (Internet tooFirewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="947a0-274">Provoz dotkne interní IP adresu brány firewall hello (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="947a0-274">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="947a0-275">Brány firewall pro SQL nemá žádná pravidla pro předávání a vyřazuje hello provoz</span><span class="sxs-lookup"><span data-stu-id="947a0-275">Firewall has no forwarding rules for SQL and drops hello traffic</span></span>

## <a name="conclusion"></a><span data-ttu-id="947a0-276">Závěr</span><span class="sxs-lookup"><span data-stu-id="947a0-276">Conclusion</span></span>
<span data-ttu-id="947a0-277">Toto je poměrně splněny následující způsob ochraně vaší aplikace s bránou firewall a izolace hello back-end podsíť z příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="947a0-277">This is a relatively straight forward way of protecting your application with a firewall and isolating hello back end subnet from inbound traffic.</span></span>

<span data-ttu-id="947a0-278">Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].</span><span class="sxs-lookup"><span data-stu-id="947a0-278">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="947a0-279">Odkazy</span><span class="sxs-lookup"><span data-stu-id="947a0-279">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="947a0-280">Hlavní skript a konfiguraci sítě</span><span class="sxs-lookup"><span data-stu-id="947a0-280">Main Script and Network Config</span></span>
<span data-ttu-id="947a0-281">Uložte hello úplné skript v souboru skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="947a0-281">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="947a0-282">Uložte do souboru s názvem "NetworkConf2.xml" hello konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="947a0-282">Save hello Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="947a0-283">Podle potřeby změňte hello proměnné definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="947a0-283">Modify hello user defined variables as needed.</span></span> <span data-ttu-id="947a0-284">Spusťte skript hello a potom postupujte podle hello brány Firewall pravidla instalace instrukce výše.</span><span class="sxs-lookup"><span data-stu-id="947a0-284">Run hello script, then follow hello Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="947a0-285">Úplné skriptu</span><span class="sxs-lookup"><span data-stu-id="947a0-285">Full Script</span></span>
<span data-ttu-id="947a0-286">Tento skript bude na základě proměnných uživatelsky definované hello:</span><span class="sxs-lookup"><span data-stu-id="947a0-286">This script will, based on hello user defined variables:</span></span>

1. <span data-ttu-id="947a0-287">Připojit tooan předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="947a0-287">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="947a0-288">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="947a0-288">Create a new storage account</span></span>
3. <span data-ttu-id="947a0-289">Vytvořit novou virtuální síť a dvě podsítě, jak jsou definovány v souboru konfigurace sítě hello</span><span class="sxs-lookup"><span data-stu-id="947a0-289">Create a new VNet and two subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="947a0-290">Sestavení 4 windows server virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="947a0-290">Build 4 windows server VMs</span></span>
5. <span data-ttu-id="947a0-291">Konfigurace, včetně NSG:</span><span class="sxs-lookup"><span data-stu-id="947a0-291">Configure NSG including:</span></span>
   * <span data-ttu-id="947a0-292">Vytváření skupina NSG</span><span class="sxs-lookup"><span data-stu-id="947a0-292">Creating a NSG</span></span>
   * <span data-ttu-id="947a0-293">Sestavování s pravidly</span><span class="sxs-lookup"><span data-stu-id="947a0-293">Populating it with rules</span></span>
   * <span data-ttu-id="947a0-294">Vazba hello NSG toohello příslušné podsítě</span><span class="sxs-lookup"><span data-stu-id="947a0-294">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="947a0-295">Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.</span><span class="sxs-lookup"><span data-stu-id="947a0-295">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="947a0-296">Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="947a0-296">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="947a0-297">Pouze chybové zprávy červeně jsou příčinou problém.</span><span class="sxs-lookup"><span data-stu-id="947a0-297">Only error messages in red are cause for concern.</span></span>
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
       - One server on hello FrontEnd Subnet (plus hello NVA on hello FrontEnd subnet)
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
       myFirewall - 10.0.1.4
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
        # Note: tooensure proper NSG Rule creation later in this script:
        #       - hello Web Server must be VM 1
        #       - hello AppVM1 Server must be VM 2
        #       - hello DNS server must be VM 4
        #
        #       Otherwise hello NSG rules in hello last section of this
        #       script will need toobe changed toomatch hello modified
        #       VM array numbers ($i) so hello NSG Rule IP addresses
        #       are aligned toohello associated VM IP addresses.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

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
                # Note: Web traffic goes through hello firewall, so we'll need tooset up a HTTP endpoint.
                #       Also, hello firewall will be redirecting web traffic tooa new IP and Port in a
                #       forwarding rule, so hello HTTP endpoint here will have hello same public and local
                #       port and hello firewall will do hello NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
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
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) too$($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
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


#### <a name="network-config-file"></a><span data-ttu-id="947a0-298">Soubor konfigurace sítě</span><span class="sxs-lookup"><span data-stu-id="947a0-298">Network Config File</span></span>
<span data-ttu-id="947a0-299">Uložte tento soubor xml s aktualizované umístění a přidáte odkaz toothis hello souboru toohello $NetworkConfigFile proměnné ve skriptu hello výše.</span><span class="sxs-lookup"><span data-stu-id="947a0-299">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello script above.</span></span>

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

#### <a name="sample-application-scripts"></a><span data-ttu-id="947a0-300">Ukázkové skripty aplikace</span><span class="sxs-lookup"><span data-stu-id="947a0-300">Sample Application Scripts</span></span>
<span data-ttu-id="947a0-301">Pokud chcete tooinstall ukázková aplikace pro toto a další příklady DMZ jeden bylo zadáno v hello následující odkaz: [ukázkový skript aplikace][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="947a0-301">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Příchozí DMZ s NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Ikona cílové NAT"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Pravidlo brány firewall"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Aktivace pravidla brány firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
