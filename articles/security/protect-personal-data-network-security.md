---
title: "Ochrana osobních dat pomocí funkce zabezpečení sítě Azure | Microsoft Docs"
description: "Ochrana osobních dat pomocí funkce zabezpečení sítě Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: b7a6343f37f890b65d9536eac34e1069d24ad97e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a><span data-ttu-id="226c9-103">Ochrana osobních dat pomocí funkce zabezpečení sítě: Azure Application Gateway a skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="226c9-103">Protect personal data with network security features: Azure Application Gateway and Network Security Groups</span></span>

<span data-ttu-id="226c9-104">Tento článek obsahuje informace a postupy, které vám pomohou používat Azure Application Gateway a skupiny zabezpečení sítě na ochranu osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="226c9-104">This article provides information and procedures that will help you use Azure Application Gateway and Network Security Groups to protect personal data.</span></span>

<span data-ttu-id="226c9-105">Důležitým prvkem v strategie víceúrovňová zabezpečení k ochraně osobních údajů osobních údajů je obrana proti běžné zneužití ohrožení zabezpečení, například Injektáž SQL nebo skriptování mezi servery.</span><span class="sxs-lookup"><span data-stu-id="226c9-105">An important element in a multi-layered security strategy to protect the privacy of personal data is a defense against common vulnerability exploits such as SQL injection or cross-site scripting.</span></span> <span data-ttu-id="226c9-106">Zachování nežádoucí síťový provoz z vaší Azure virtuální sítě přispívá k ochraně proti potenciální ohrožení citlivých dat a Microsoft Azure nabízí nástroje, které pomáhají chránit vaše data proti útočníkům.</span><span class="sxs-lookup"><span data-stu-id="226c9-106">Keeping unwanted network traffic out of your Azure virtual network helps protect against potential compromise of sensitive data, and Microsoft Azure gives you tools to help protect your data against attackers.</span></span>

## <a name="scenario"></a><span data-ttu-id="226c9-107">Scénář</span><span class="sxs-lookup"><span data-stu-id="226c9-107">Scenario</span></span>

<span data-ttu-id="226c9-108">Velké výletních společnosti, centrálou ve Spojených státech amerických, je rozšířit jeho operací a nabídnout itineráře v Středomoří, Jaderského a baltský moři, jakož i Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="226c9-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="226c9-109">Při podpoře těchto úsilí získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a Spojeném království</span><span class="sxs-lookup"><span data-stu-id="226c9-109">In furtherance of those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span>

<span data-ttu-id="226c9-110">Společnost používá Microsoft Azure k ukládání firemních dat v cloudu a spouštět aplikace na virtuální počítače, které zpracovat a přístup k těmto datům.</span><span class="sxs-lookup"><span data-stu-id="226c9-110">The company uses Microsoft Azure to store corporate data in the cloud and run applications on virtual machines that process and access this data.</span></span> <span data-ttu-id="226c9-111">Tato data obsahují osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka.</span><span class="sxs-lookup"><span data-stu-id="226c9-111">This data includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="226c9-112">Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako jsou adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti.</span><span class="sxs-lookup"><span data-stu-id="226c9-112">It also includes traditional Human Resources information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="226c9-113">Na řádku výletních také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje ke sledování vztahů se zákazníky aktuální a starší.</span><span class="sxs-lookup"><span data-stu-id="226c9-113">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

<span data-ttu-id="226c9-114">Zaměstnanci společnosti přístup k síti ze vzdálených pobočkách společnosti a agenty cesta umístěné po celém světě mají přístup k některým prostředkům společnosti a pracovat s ním pomocí webové aplikace hostované ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="226c9-114">Corporate employees access the network from the company’s remote offices and travel agents located around the world have access to some company resources and use web-based applications hosted in Azure VMs to interact with it.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="226c9-115">Popis problému</span><span class="sxs-lookup"><span data-stu-id="226c9-115">Problem statement</span></span>

<span data-ttu-id="226c9-116">Společnosti musí chránit ochranu osobních údajů zákazníků a zaměstnanců osobních dat od útočníky, kteří zneužívají ohrožení zabezpečení softwaru ke spuštění škodlivého kódu, který mohla vystavit osobní data uložená nebo použít cloudových aplikací společnosti.</span><span class="sxs-lookup"><span data-stu-id="226c9-116">The company must protect the privacy of customers’ and employees’ personal data from attackers who exploit software vulnerabilities to run malicious code that could expose personal data stored or used by the company’s cloud-based applications.</span></span>

## <a name="company-goal"></a><span data-ttu-id="226c9-117">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="226c9-117">Company goal</span></span>

<span data-ttu-id="226c9-118">Cílem společnosti zajistit, že neoprávněné osoby přístup k podnikovým virtuálních sítí Azure a aplikace a data, která jsou umístěny existuje zneužitím známých chyb zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="226c9-118">The company’s goal to ensure that unauthorized persons cannot access corporate Azure Virtual Networks and the applications and data that reside there by exploiting common vulnerabilities.</span></span> 

## <a name="solutions"></a><span data-ttu-id="226c9-119">Řešení</span><span class="sxs-lookup"><span data-stu-id="226c9-119">Solutions</span></span>

<span data-ttu-id="226c9-120">Microsoft Azure nabízí mechanismy zabezpečení, aby pomohly zabránit nežádoucí provoz zadávání virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="226c9-120">Microsoft Azure provides security mechanisms to help prevent unwanted traffic from entering Azure Virtual Networks.</span></span> <span data-ttu-id="226c9-121">Řízení příchozí a odchozí přenosy tradičně provádí brány firewall.</span><span class="sxs-lookup"><span data-stu-id="226c9-121">Control of inbound and outbound traffic is traditionally performed by firewalls.</span></span> <span data-ttu-id="226c9-122">V Azure můžete službu Application Gateway s brány Firewall webových aplikací a skupin zabezpečení sítě (NSG), které slouží jako jednoduchý distribuovanou bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="226c9-122">In Azure, you can use the Application Gateway with the Web Application Firewall and Network Security Groups (NSG), which act as a simple distributed firewall.</span></span> <span data-ttu-id="226c9-123">Tyto nástroje vám umožňují detekovat a blokovat nežádoucí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="226c9-123">These tools enable you to detect and block unwanted network traffic.</span></span>

### <a name="application-gatewayweb-application-firewall"></a><span data-ttu-id="226c9-124">Brány Firewall aplikací brány nebo webové aplikace</span><span class="sxs-lookup"><span data-stu-id="226c9-124">Application Gateway/Web Application Firewall</span></span>

<span data-ttu-id="226c9-125">[Brány Firewall webových aplikací](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) součást (firewall webových aplikací) [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) chrání webových aplikací, které jsou stále cíle nebezpečné útoky, které využívají známé běžné chyby zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="226c9-125">The [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF) component of the [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) protects web applications, which are increasingly targets of malicious attacks that exploit common known vulnerabilities.</span></span> <span data-ttu-id="226c9-126">Centralizované firewall webových aplikací chrání před útoky na web a zjednodušuje správu zabezpečení bez nutnosti změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="226c9-126">A centralized WAF both protects against web attacks and simplifies security management without requiring any application changes.</span></span>

<span data-ttu-id="226c9-127">Různé kategorie útoku, včetně Injektáž SQL, skriptování mezi weby, porušení protokolu HTTP a anomálií, robotů, prohledávací moduly, skenerů, časté nesprávné konfigurace aplikace, HTTP Denial of Service a další běžné útoky, jako adresy Azure firewall webových aplikací příkaz vkládání, požadavek HTTP pašování, rozdělení odpovědi HTTP a útoky zahrnutí vzdáleného souboru.</span><span class="sxs-lookup"><span data-stu-id="226c9-127">Azure WAF addresses various attack categories including SQL injection, cross site scripting, HTTP protocol violations and anomalies, bots, crawlers, scanners, common application misconfigurations, HTTP Denial of Service, and other common attacks such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attacks.</span></span> 

<span data-ttu-id="226c9-128">Můžete vytvořit služby application gateway s firewall webových aplikací nebo existující aplikační brány přidat firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="226c9-128">You can create an application gateway with WAF, or add WAF to an existing application gateway.</span></span> <span data-ttu-id="226c9-129">V obou případech Azure Application Gateway vyžaduje vlastní podsíti.</span><span class="sxs-lookup"><span data-stu-id="226c9-129">In either case, Azure Application Gateway requires its own subnet.</span></span>

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a><span data-ttu-id="226c9-130">Vytvoření služby application gateway s firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="226c9-130">How do I create an application gateway with WAF?</span></span> 

<span data-ttu-id="226c9-131">Pokud chcete vytvořit novou aplikační bránu s povolen firewall webových aplikací, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="226c9-131">To create a new application gateway with WAF enabled, do the following:</span></span>

1. <span data-ttu-id="226c9-132">Přihlaste se k portálu Azure a v **Oblíbené** podokně portálu, klikněte na tlačítko **nový**</span><span class="sxs-lookup"><span data-stu-id="226c9-132">Log in to the Azure portal and in the **Favorites** pane of the portal, click **New**</span></span>

2. <span data-ttu-id="226c9-133">V okně **Nový** klikněte na **Sítě**.</span><span class="sxs-lookup"><span data-stu-id="226c9-133">In the **New** blade, click **Networking**.</span></span>

3. <span data-ttu-id="226c9-134">Klikněte na tlačítko **Aplikační brána**.</span><span class="sxs-lookup"><span data-stu-id="226c9-134">Click **Application Gateway**.</span></span>

4. <span data-ttu-id="226c9-135">Přejděte na portál Azure, **kliknutím na tlačítko Nová \> sítě \> Application Gateway.**</span><span class="sxs-lookup"><span data-stu-id="226c9-135">Navigate to the Azure portal, **click New \> Networking \> Application Gateway.**</span></span>

   ![vytváření application Gateway](media/protect-netsec/app-gateway-01.png)

5. <span data-ttu-id="226c9-137">V **Základy** okno, které se zobrazí, zadejte hodnoty pro následující pole: název, vrstvy (Standard nebo firewall webových aplikací), velikost SKU (malé, střední nebo velké) Instance počet (2 pro zajištění vysoké dostupnosti), předplatné, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="226c9-137">In the **Basics** blade that appears, enter the values for the following fields: Name, Tier (Standard or WAF), SKU size (Small, Medium, or Large),  Instance count (2 for high availability), Subscription, Resource group, and Location.</span></span>

6. <span data-ttu-id="226c9-138">V **nastavení** okno, které se zobrazí pod **virtuální síť**, klikněte na tlačítko **vyberte virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="226c9-138">In the **Settings** blade that appears under **Virtual network**, click **Choose a virtual network**.</span></span> <span data-ttu-id="226c9-139">Tento krok se otevře, zadejte v okně vyberte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="226c9-139">This step opens enter the Choose virtual network blade.</span></span>

7. <span data-ttu-id="226c9-140">Klikněte na tlačítko **vytvořit nový** otevřete **vytvořit virtuální síť** okno.</span><span class="sxs-lookup"><span data-stu-id="226c9-140">Click **Create new** to open the **Create virtual network** blade.</span></span>

8. <span data-ttu-id="226c9-141">Zadejte následující hodnoty: název, adresní prostor, název podsítě, rozsah adres podsítě.</span><span class="sxs-lookup"><span data-stu-id="226c9-141">Enter the following values: Name, Address space, Subnet name, Subnet address range.</span></span> <span data-ttu-id="226c9-142">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="226c9-142">Click **OK**.</span></span>

9. <span data-ttu-id="226c9-143">Na **nastavení** okno pod **konfigurace IP front-endu**, vyberte typ IP adresy.</span><span class="sxs-lookup"><span data-stu-id="226c9-143">On the **Settings** blade under **Frontend IP configuration**, choose the IP address type.</span></span>

10. <span data-ttu-id="226c9-144">Klikněte na tlačítko **zvolte veřejnou IP adresu,** pak **vytvořit nové.**</span><span class="sxs-lookup"><span data-stu-id="226c9-144">Click **Choose a public IP address,** then **Create new.**</span></span>

11. <span data-ttu-id="226c9-145">Přijměte výchozí hodnotu a klikněte na tlačítko **OK.**</span><span class="sxs-lookup"><span data-stu-id="226c9-145">Accept the default value, and click **OK.**</span></span>

12. <span data-ttu-id="226c9-146">Na **nastavení** okno pod **konfiguraci naslouchacího procesu**, vyberte pomocí protokolu HTTP nebo HTTPS v části **protokol**.</span><span class="sxs-lookup"><span data-stu-id="226c9-146">On the **Settings** blade under **Listener configuration**, select to use HTTP or HTTPS under **Protocol**.</span></span> <span data-ttu-id="226c9-147">K používání protokolu HTTPS, je požadovaný certifikát.</span><span class="sxs-lookup"><span data-stu-id="226c9-147">To use HTTPS, a certificate is required.</span></span>

13. <span data-ttu-id="226c9-148">Konfigurovat konkrétní nastavení firewall webových aplikací: **brány Firewall stav** (**povoleno**) a **režimu brány Firewall** (**prevence**).</span><span class="sxs-lookup"><span data-stu-id="226c9-148">Configure the WAF specific settings: **Firewall status** (**Enabled**) and **Firewall mode** (**Prevention**).</span></span> <span data-ttu-id="226c9-149">Pokud se rozhodnete **detekce** režim, je provoz jenom protokolována.</span><span class="sxs-lookup"><span data-stu-id="226c9-149">If you choose **Detection** as the mode, traffic is only logged.</span></span>

14. <span data-ttu-id="226c9-150">Zkontrolujte **Souhrn** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="226c9-150">Review the **Summary** page and click **OK**.</span></span> <span data-ttu-id="226c9-151">Službu application gateway je nyní zařazen do fronty a vytvořit.</span><span class="sxs-lookup"><span data-stu-id="226c9-151">Now the application gateway is queued up and created.</span></span>

<span data-ttu-id="226c9-152">Po vytvoření služby application gateway, můžete přejít na ni na portálu a pokračovat v konfiguraci služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="226c9-152">After the application gateway has been created, you can navigate to it in the portal and continue configuration of the application gateway.</span></span>

![Vytvoření aplikační brány](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-to-an-existing-application"></a><span data-ttu-id="226c9-154">Jak přidat firewall webových aplikací na existující aplikaci?</span><span class="sxs-lookup"><span data-stu-id="226c9-154">How do I add WAF to an existing application?</span></span>

<span data-ttu-id="226c9-155">Pokud chcete aktualizovat existující aplikační brány pro podporu firewall webových aplikací v režimu prevence, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="226c9-155">To update an existing application gateway to support WAF in prevention mode, do the following:</span></span>

1. <span data-ttu-id="226c9-156">Na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="226c9-156">In the Azure portal **Favorites** pane, click **All resources**.</span></span>

2. <span data-ttu-id="226c9-157">Klikněte na existující aplikační brána v **všechny prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="226c9-157">Click the existing Application Gateway in the **All resources** blade.</span></span> 
>[!NOTE]
<span data-ttu-id="226c9-158">Poznámka: Pokud odběr, který jste již vybrali neobsahuje několik prostředků, můžete zadat název ve filtru podle názvu...</span><span class="sxs-lookup"><span data-stu-id="226c9-158">Note: If the subscription you selected already has several resources in it, you can enter the name in the Filter by name…</span></span> <span data-ttu-id="226c9-159">pro snadný přístup k zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="226c9-159">box to easily access the DNS zone.</span></span>
3. <span data-ttu-id="226c9-160">Klikněte na tlačítko **brány firewall webových aplikací** a aktualizovat nastavení aplikační brány: **upgradujte firewall webových aplikací úrovně** (zaškrtnuto), **brány Firewall stav** (povoleno),  **Režimu brány firewall** (prevence).</span><span class="sxs-lookup"><span data-stu-id="226c9-160">Click **Web application firewall** and update the application gateway settings: **Upgrade to WAF Tier** (checked), **Firewall status** (enabled),     **Firewall mode** (Prevention).</span></span> <span data-ttu-id="226c9-161">Musíte také nakonfigurovat sadu pravidel a nakonfigurovat zakázaná pravidla.</span><span class="sxs-lookup"><span data-stu-id="226c9-161">You also need to configure the rule set, and configure disabled rules.</span></span>

<span data-ttu-id="226c9-162">Podrobnější informace o tom, jak vytvořit novou aplikační bránu firewall webových aplikací a jak přidat firewall webových aplikací do existující aplikace bránu, najdete v části [vytvoření služby application gateway pomocí brány firewall webových aplikací pomocí portálu.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)</span><span class="sxs-lookup"><span data-stu-id="226c9-162">For more detailed information on how to create a new application gateway with WAF and how to add WAF to an existing application gateway, see [Create an application gateway with web application firewall by using the portal.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)</span></span>

### <a name="network-security-groups"></a><span data-ttu-id="226c9-163">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="226c9-163">Network Security Groups</span></span>

<span data-ttu-id="226c9-164">A [skupinu zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) obsahuje seznam pravidel zabezpečení, která povolují nebo odpírají síťový provoz prostředky připojenými k [virtuálních sítí Azure](https://docs.microsoft.com/azure/virtual-network/) (VNet).</span><span class="sxs-lookup"><span data-stu-id="226c9-164">A [network security group](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) contains a list of security rules that allow or deny network traffic to resources connected to [Azure Virtual Networks](https://docs.microsoft.com/azure/virtual-network/) (VNet).</span></span> <span data-ttu-id="226c9-165">Skupiny Nsg můžou být přidružena k podsítě nebo jednotlivé virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="226c9-165">NSGs can be associated to subnets or individual VMs.</span></span> <span data-ttu-id="226c9-166">Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla se vztahují na všechny prostředky, které jsou připojené k příslušné podsíti.</span><span class="sxs-lookup"><span data-stu-id="226c9-166">When an NSG is associated to a subnet, the rules apply to all resources connected to the subnet.</span></span> <span data-ttu-id="226c9-167">Provoz se dá dále omezit přidružením skupiny zabezpečení sítě k virtuálnímu počítači nebo síťové kartě.</span><span class="sxs-lookup"><span data-stu-id="226c9-167">Traffic can further be restricted by also associating an NSG to a VM or NIC.</span></span>

<span data-ttu-id="226c9-168">Skupiny Nsg obsahují čtyři vlastnosti: název, oblast, skupinu prostředků a pravidla.</span><span class="sxs-lookup"><span data-stu-id="226c9-168">NSGs contain four properties: Name, Region, Resource group, and Rules.</span></span>
>[!Note]
<span data-ttu-id="226c9-169">Ačkoli skupina zabezpečení sítě existuje ve skupině prostředků, dá se přidružit k prostředkům v libovolné skupině prostředků, pokud příslušný prostředek patří do stejné oblasti Azure jako příslušná skupina zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="226c9-169">Although an NSG exists in a resource group, it can be associated to resources in any resource group, as long as the resource is part of the same Azure region as the NSG.</span></span>

<span data-ttu-id="226c9-170">Pravidla NSG obsahují devět vlastnosti: název, protokol (TCP, UDP nebo \*, což zahrnuje ICMP a také protokolu UDP a TCP), zdroj rozsah portů, rozsah cílových portů, zdrojová adresa předpony, předpona cílové adresy směr (příchozí nebo odchozí), () s prioritou rozsahu od 100 do 4096) a přístup k typu (povolit nebo zakázat).</span><span class="sxs-lookup"><span data-stu-id="226c9-170">NSG rules contain nine properties: Name, Protocol (TCP, UDP, or \*, which includes ICMP as well as UDP and TCP), Source port range, Destination port range, Source address prefix, Destination address prefix, Direction (inbound or outbound), Priority (between 100 and 4096) and Access type (allow or deny).</span></span> <span data-ttu-id="226c9-171">Všechny skupiny Nsg obsahují sadu výchozích pravidel, která je možné odstranit, nebo přepsat pravidly, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="226c9-171">All NSGs contain a set of default rules that can be deleted, or overridden by the rules you create.</span></span>

#### <a name="how-do-i-implement-nsgs"></a><span data-ttu-id="226c9-172">Jak implementovat skupiny Nsg?</span><span class="sxs-lookup"><span data-stu-id="226c9-172">How do I implement NSGs?</span></span>

<span data-ttu-id="226c9-173">Implementací skupin Nsg vyžaduje plánování a existuje několik aspekty návrhu, které je třeba vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="226c9-173">Implementing NSGs requires planning, and there are several design considerations you need to take into account.</span></span> <span data-ttu-id="226c9-174">Patří mezi ně omezení pro počet skupin Nsg na předplatné a pravidla na skupinu NSG; Virtuální síť a podsíť návrhu, zvláštní pravidla, provoz protokolu ICMP, izolace vrstev s podsítí, nástroje pro vyrovnávání zatížení a další.</span><span class="sxs-lookup"><span data-stu-id="226c9-174">These include limits on the number of NSGs per subscription and rules per NSG; VNet and subnet design, special rules, ICMP traffic, isolation of tiers with subnets, load balancers, and more.</span></span>

<span data-ttu-id="226c9-175">Další pokyny plánování a implementace skupiny Nsg a vzorový scénář nasazení najdete v tématu [filtrování provozu sítě přenosů se skupinami zabezpečení sítě.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)</span><span class="sxs-lookup"><span data-stu-id="226c9-175">For more guidance in planning and implementing NSGs, and a sample deployment scenario, see [Filter network traffic with network security groups.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)</span></span>

#### <a name="how-do-i-create-rules-in-an-nsg"></a><span data-ttu-id="226c9-176">Vytvoření pravidla v skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="226c9-176">How do I create rules in an NSG?</span></span>

<span data-ttu-id="226c9-177">Pokud chcete vytvořit příchozích pravidel v existující skupině, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="226c9-177">To create inbound rules in an existing NSG, do the following:</span></span>

1. <span data-ttu-id="226c9-178">Klikněte na tlačítko **Procházet**a potom **skupin zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="226c9-178">Click **Browse**, and then **Network security groups**.</span></span>

2. <span data-ttu-id="226c9-179">V seznamu skupin Nsg, klikněte na **NSG front-endu**a potom **příchozí pravidla zabezpečení.**</span><span class="sxs-lookup"><span data-stu-id="226c9-179">In the list of NSGs, click **NSG-FrontEnd**, and then **Inbound security rules.**</span></span>

3. <span data-ttu-id="226c9-180">V seznamu příchozí pravidla zabezpečení, klikněte na **přidat.**</span><span class="sxs-lookup"><span data-stu-id="226c9-180">In the list of Inbound security rules, click **Add.**</span></span>

4. <span data-ttu-id="226c9-181">Zadejte hodnoty do následujících polí: název, Priority, zdroj, protokol, zdrojový rozsah, cíl, cílový rozsah portů a akce.</span><span class="sxs-lookup"><span data-stu-id="226c9-181">Enter the values in the following fields: Name, Priority, Source, Protocol, Source range, Destination, Destination port range, and Action.</span></span>

<span data-ttu-id="226c9-182">Nové pravidlo se zobrazí v této skupině za několik sekund.</span><span class="sxs-lookup"><span data-stu-id="226c9-182">The new rule will appear in the NSG after a few seconds.</span></span>

![pravidla zabezpečení sítě](media/protect-netsec/inbound-security.png)

<span data-ttu-id="226c9-184">Další pokyny o tom, jak vytvářet skupiny Nsg v podsítích, vytvořit pravidla a přidružit skupinu NSG k podsíti front-end a back-end, najdete v části [vytvoření skupin zabezpečení sítě pomocí portálu Azure.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)</span><span class="sxs-lookup"><span data-stu-id="226c9-184">For more instructions on how to create NSGs in subnets, create rules, and associate an NSG with a front-end and back-end subnet, see [Create network security groups using the Azure portal.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)</span></span>

## <a name="next-steps"></a><span data-ttu-id="226c9-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="226c9-185">Next steps</span></span>

[<span data-ttu-id="226c9-186">Zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="226c9-186">Azure Network Security</span></span>](https://azure.microsoft.com/blog/azure-network-security/)

[<span data-ttu-id="226c9-187">Osvědčené postupy zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="226c9-187">Azure Network Security Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[<span data-ttu-id="226c9-188">Získání informací o skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="226c9-188">Get information about a network security group</span></span>](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[<span data-ttu-id="226c9-189">Brány firewall webových aplikací (firewall webových aplikací)</span><span class="sxs-lookup"><span data-stu-id="226c9-189">Web application firewall (WAF)</span></span>](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
