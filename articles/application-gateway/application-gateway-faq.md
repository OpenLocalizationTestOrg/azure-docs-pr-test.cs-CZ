---
title: "Nejčastější dotazy pro Azure Application Gateway | Microsoft Docs"
description: "Tato stránka obsahuje odpovědi na nejčastější dotazy týkající se Azure Application Gateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 4e6244d92f41e0aa5c8a70db0db2881036984247
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a><span data-ttu-id="3f745-103">Nejčastější dotazy pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="3f745-103">Frequently asked questions for Application Gateway</span></span>

## <a name="general"></a><span data-ttu-id="3f745-104">Obecné</span><span class="sxs-lookup"><span data-stu-id="3f745-104">General</span></span>

<span data-ttu-id="3f745-105">**OTÁZKY. Co je aplikační brána?**</span><span class="sxs-lookup"><span data-stu-id="3f745-105">**Q. What is Application Gateway?**</span></span>

<span data-ttu-id="3f745-106">Služba Azure Application Gateway je řadiče doručení aplikace (ADC) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f745-106">Azure Application Gateway is an Application Delivery Controller (ADC) as a service, offering various layer 7 load balancing capabilities for your applications.</span></span> <span data-ttu-id="3f745-107">Nabízí vysoce dostupné a škálovatelné služby, která je plně spravovaná službou Azure.</span><span class="sxs-lookup"><span data-stu-id="3f745-107">It offers highly available and scalable service, which is fully managed by Azure.</span></span>

<span data-ttu-id="3f745-108">**OTÁZKY. Jaké funkce podporuje Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-108">**Q. What features does Application Gateway support?**</span></span>

<span data-ttu-id="3f745-109">Aplikační brána podporuje snižování zátěže protokolu SSL a koncová SSL, brány Firewall webových aplikací, spřažení na základě souboru cookie relace, adresa url na základě cesty směrování, hostování více lokality a ostatní.</span><span class="sxs-lookup"><span data-stu-id="3f745-109">Application Gateway supports SSL offloading and end to end SSL, Web Application Firewall, cookie-based session affinity, url path-based routing, multi site hosting, and others.</span></span> <span data-ttu-id="3f745-110">Úplný seznam podporovaných funkcích najdete na adrese [Úvod aplikační brány](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="3f745-110">For a full list of supported features, visit [Introduction to Application Gateway](application-gateway-introduction.md)</span></span>

<span data-ttu-id="3f745-111">**OTÁZKY. Jaký je rozdíl mezi aplikační bránu a nástroj pro vyrovnávání zatížení Azure?**</span><span class="sxs-lookup"><span data-stu-id="3f745-111">**Q. What is the difference between Application Gateway and Azure Load Balancer?**</span></span>

<span data-ttu-id="3f745-112">Application Gateway je Vyrovnávání zatížení vrstvy 7, což znamená, že pracuje pouze webový provoz (HTTP nebo HTTPS/WebSocket).</span><span class="sxs-lookup"><span data-stu-id="3f745-112">Application Gateway is a layer 7 load balancer, which means it works with web traffic only (HTTP/HTTPS/WebSocket).</span></span> <span data-ttu-id="3f745-113">Podporuje možnosti, například ukončení protokolu SSL, spřažení relace na základě souborů cookie a kruhové dotazování pro provoz služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3f745-113">It supports capabilities such as SSL termination, cookie-based session affinity, and round robin for load balancing traffic.</span></span> <span data-ttu-id="3f745-114">Nástroj pro vyrovnávání zatížení a načtěte provozu zůstatky na vrstvě 4 (TCP/UDP).</span><span class="sxs-lookup"><span data-stu-id="3f745-114">Load Balancer, load balances traffic at layer 4 (TCP/UDP).</span></span>

<span data-ttu-id="3f745-115">**OTÁZKY. Jaké protokoly podporuje Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-115">**Q. What protocols does Application Gateway support?**</span></span>

<span data-ttu-id="3f745-116">Aplikační brána podporuje protokoly HTTP, HTTPS a protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3f745-116">Application Gateway supports HTTP, HTTPS, and WebSocket.</span></span>

<span data-ttu-id="3f745-117">**OTÁZKY. Jaké prostředky jsou dnes podporovány v rámci fondu back-end?**</span><span class="sxs-lookup"><span data-stu-id="3f745-117">**Q. What resources are supported today as part of backend pool?**</span></span>

<span data-ttu-id="3f745-118">Back-endové fondy může skládat ze síťových adaptérů, sady škálování virtuálního počítače, veřejné IP adresy, názvy interní IP adres, plně kvalifikované domény (FQDN) a víceklientské back EndY jako Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="3f745-118">Backend pools can be composed of NICs, virtual machine scale sets, public IPs, internal IPs, fully qualified domain names (FQDN), and multi-tenant back-ends like Azure Web Apps.</span></span> <span data-ttu-id="3f745-119">Členy fondu back-end brány aplikace nejsou vázaný na sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="3f745-119">Application Gateway backend pool members are not tied to an availability set.</span></span> <span data-ttu-id="3f745-120">Členy fondu back-end může být napříč clustery, datových center, nebo mimo Azure, dokud mají připojení pomocí protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="3f745-120">Members of backend pools can be across clusters, data centers, or outside of Azure as long as they have IP connectivity.</span></span>

<span data-ttu-id="3f745-121">**OTÁZKY. Jaké oblasti je služba k dispozici v?**</span><span class="sxs-lookup"><span data-stu-id="3f745-121">**Q. What regions is the service available in?**</span></span>

<span data-ttu-id="3f745-122">Application Gateway je k dispozici ve všech oblastech globální Azure.</span><span class="sxs-lookup"><span data-stu-id="3f745-122">Application Gateway is available in all regions of global Azure.</span></span> <span data-ttu-id="3f745-123">Je také dostupná v [Azure China](https://www.azure.cn/) a [Azure Government.](https://azure.microsoft.com/en-us/overview/clouds/government/)</span><span class="sxs-lookup"><span data-stu-id="3f745-123">It is also available in [Azure China](https://www.azure.cn/) and [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span></span>

<span data-ttu-id="3f745-124">**OTÁZKY. To je vyhrazený pro Moje předplatné nasazení nebo je sdílen na zákazníky?**</span><span class="sxs-lookup"><span data-stu-id="3f745-124">**Q. Is this a dedicated deployment for my subscription or is it shared across customers?**</span></span>

<span data-ttu-id="3f745-125">Application Gateway je vyhrazené nasazení ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="3f745-125">Application Gateway is a dedicated deployment in your virtual network.</span></span>

<span data-ttu-id="3f745-126">**OTÁZKY. Je HTTP -> HTTPS přesměrování podporovány?**</span><span class="sxs-lookup"><span data-stu-id="3f745-126">**Q. Is HTTP->HTTPS redirection supported?**</span></span>

<span data-ttu-id="3f745-127">Přesměrování je podporována.</span><span class="sxs-lookup"><span data-stu-id="3f745-127">Redirection is supported.</span></span> <span data-ttu-id="3f745-128">Navštivte [přehled přesměrování Application Gateway](application-gateway-redirect-overview.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3f745-128">Visit [Application Gateway redirect overview](application-gateway-redirect-overview.md) to learn more.</span></span>

<span data-ttu-id="3f745-129">**OTÁZKY. Pořadí, v jakém jsou naslouchací procesy zpracování?**</span><span class="sxs-lookup"><span data-stu-id="3f745-129">**Q. In what order are listeners processed?**</span></span>

<span data-ttu-id="3f745-130">Moduly pro naslouchání jsou zpracovány v pořadí, ve kterém jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="3f745-130">Listeners are processed in the order they are shown.</span></span> <span data-ttu-id="3f745-131">Z tohoto důvodu Pokud základní naslouchací proces odpovídá příchozí požadavek zpracuje jej nejprve.</span><span class="sxs-lookup"><span data-stu-id="3f745-131">For that reason if a basic listener matches an incoming request it processes it first.</span></span>  <span data-ttu-id="3f745-132">Moduly pro naslouchání více lokalit by měl být nakonfigurovaný před základní naslouchací proces pro zajištění, že provoz se směruje na správný back-end.</span><span class="sxs-lookup"><span data-stu-id="3f745-132">Multi-site listeners should be configured before a basic listener to ensure traffic is routed to the correct back-end.</span></span>

<span data-ttu-id="3f745-133">**OTÁZKY. Kde najít IP a DNS Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-133">**Q. Where do I find Application Gateway’s IP and DNS?**</span></span>

<span data-ttu-id="3f745-134">Pokud používáte veřejnou IP adresu jako koncový bod, tyto informace naleznete na prostředek veřejné IP adresy nebo na stránce Přehled pro službu Application Gateway na portálu.</span><span class="sxs-lookup"><span data-stu-id="3f745-134">When using a public IP address as an endpoint, this information can be found on the public IP address resource or on the Overview page for the Application Gateway in the portal.</span></span> <span data-ttu-id="3f745-135">Pro interní IP adresy najdete na stránce Přehled.</span><span class="sxs-lookup"><span data-stu-id="3f745-135">For internal IP addresses, this can be found on the Overview page.</span></span>

<span data-ttu-id="3f745-136">**OTÁZKY. IP adresa nebo DNS mění za dobu existence služby Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-136">**Q. Does the IP or DNS change over the lifetime of the Application Gateway?**</span></span>

<span data-ttu-id="3f745-137">Virtuální IP adresu můžete změnit, pokud brána po zastavení a spuštění zákazník.</span><span class="sxs-lookup"><span data-stu-id="3f745-137">The VIP can change if the gateway is stopped and started by the customer.</span></span> <span data-ttu-id="3f745-138">DNS přidružené Application Gateway přes životního cyklu brány nezmění.</span><span class="sxs-lookup"><span data-stu-id="3f745-138">The DNS associated with Application Gateway does not change over the lifecycle of the gateway.</span></span> <span data-ttu-id="3f745-139">Z tohoto důvodu doporučujeme použít CNAME alias a přejděte na adresu DNS aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="3f745-139">For this reason, it is recommended to use a CNAME alias and point it to the DNS address of the Application Gateway.</span></span>

<span data-ttu-id="3f745-140">**OTÁZKY. Podporuje Application Gateway statickou IP adresu?**</span><span class="sxs-lookup"><span data-stu-id="3f745-140">**Q. Does Application Gateway support static IP?**</span></span>

<span data-ttu-id="3f745-141">Ne, aplikační brána nepodporuje statické veřejné IP adresy, ale podporuje statické interní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="3f745-141">No, Application Gateway does not support static public IP addresses, but it does support static internal IPs.</span></span>

<span data-ttu-id="3f745-142">**OTÁZKY. Podporuje Application Gateway na bráně víc veřejných IP adres?**</span><span class="sxs-lookup"><span data-stu-id="3f745-142">**Q. Does Application Gateway support multiple public IPs on the gateway?**</span></span>

<span data-ttu-id="3f745-143">Na aplikační brány je podporovaná pouze jednu veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="3f745-143">Only one public IP address is supported on an Application Gateway.</span></span>

<span data-ttu-id="3f745-144">**OTÁZKY. Podporuje Application Gateway hlavičky x předávaných pro?**</span><span class="sxs-lookup"><span data-stu-id="3f745-144">**Q. Does Application Gateway support x-forwarded-for headers?**</span></span>

<span data-ttu-id="3f745-145">Ano, Application Gateway vloží záhlaví x předávaných pro, x předávaných proto a x předávaných portu do žádosti předávaných do back-end.</span><span class="sxs-lookup"><span data-stu-id="3f745-145">Yes, Application Gateway inserts x-forwarded-for, x-forwarded-proto, and x-forwarded-port headers into the request forwarded to the backend.</span></span> <span data-ttu-id="3f745-146">Formát hlavičky x předávaných pro je čárkami oddělený seznam IP: port.</span><span class="sxs-lookup"><span data-stu-id="3f745-146">The format for x-forwarded-for header is a comma-separated list of IP:Port.</span></span> <span data-ttu-id="3f745-147">Platné hodnoty pro x předávaných proto jsou protokolu http nebo https.</span><span class="sxs-lookup"><span data-stu-id="3f745-147">The valid values for x-forwarded-proto are http or https.</span></span> <span data-ttu-id="3f745-148">X předávaných port Určuje port, kdy požadavek dosáhla aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="3f745-148">X-forwarded-port specifies the port at which the request reached at the Application Gateway.</span></span>

<span data-ttu-id="3f745-149">**OTÁZKY. Jak dlouho trvá nasazení služby Application Gateway? Moje aplikace brány stále funguje při aktualizaci?**</span><span class="sxs-lookup"><span data-stu-id="3f745-149">**Q. How long does it take to deploy an Application Gateway? Does my Application Gateway still work when being updated?**</span></span>

<span data-ttu-id="3f745-150">Nová nasazení aplikací brány může trvat až 20 minut zřídit.</span><span class="sxs-lookup"><span data-stu-id="3f745-150">New Application Gateway deployments can take up to 20 minutes to provision.</span></span> <span data-ttu-id="3f745-151">Změny velikosti nebo počet instancí nejsou rušivý a během této doby je nadále aktivní brány.</span><span class="sxs-lookup"><span data-stu-id="3f745-151">Changes to instance size/count are not disruptive, and the gateway remains active during this time.</span></span>

## <a name="configuration"></a><span data-ttu-id="3f745-152">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3f745-152">Configuration</span></span>

<span data-ttu-id="3f745-153">**OTÁZKY. Je aplikační brána vždy nasazený ve virtuální síti?**</span><span class="sxs-lookup"><span data-stu-id="3f745-153">**Q. Is Application Gateway always deployed in a virtual network?**</span></span>

<span data-ttu-id="3f745-154">Ano, aplikační brány je vždy nasazena v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3f745-154">Yes, Application Gateway is always deployed in a virtual network subnet.</span></span> <span data-ttu-id="3f745-155">Tato podsíť může obsahovat pouze Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="3f745-155">This subnet can only contain Application Gateways.</span></span>

<span data-ttu-id="3f745-156">**OTÁZKY. Můžete aplikační brány komunikují s instancí mimo jeho virtuální síť?**</span><span class="sxs-lookup"><span data-stu-id="3f745-156">**Q. Can Application Gateway talk to instances outside its virtual network?**</span></span>

<span data-ttu-id="3f745-157">Aplikační brána může komunikovat instancí mimo virtuální síť, která je v, dokud není IP připojení.</span><span class="sxs-lookup"><span data-stu-id="3f745-157">Application Gateway can talk to instances outside of the virtual network that it is in as long as there is IP connectivity.</span></span> <span data-ttu-id="3f745-158">Pokud máte v úmyslu používat interní IP adresy jako členy fondu back-end, pak vyžaduje [VNET Peering](../virtual-network/virtual-network-peering-overview.md) nebo [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="3f745-158">If you plan to use internal IPs as backend pool members, then it requires [VNET Peering](../virtual-network/virtual-network-peering-overview.md) or [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>

<span data-ttu-id="3f745-159">**OTÁZKY. Lze nasadit nic jiného v podsíť brány aplikace?**</span><span class="sxs-lookup"><span data-stu-id="3f745-159">**Q. Can I deploy anything else in the Application Gateway subnet?**</span></span>

<span data-ttu-id="3f745-160">Ne, ale můžete nasadit další application Gateway v podsíti.</span><span class="sxs-lookup"><span data-stu-id="3f745-160">No, but you can deploy other application gateways in the subnet.</span></span>

<span data-ttu-id="3f745-161">**OTÁZKY. Skupiny zabezpečení sítě podporuje na podsíť brány aplikace?**</span><span class="sxs-lookup"><span data-stu-id="3f745-161">**Q. Are Network Security Groups supported on the Application Gateway subnet?**</span></span>

<span data-ttu-id="3f745-162">Skupiny zabezpečení sítě jsou podporovány v podsíti Application Gateway s následujícími omezeními:</span><span class="sxs-lookup"><span data-stu-id="3f745-162">Network Security Groups are supported on the Application Gateway subnet with the following restrictions:</span></span>

* <span data-ttu-id="3f745-163">Výjimky musíte umístit pro příchozí komunikaci na portech 65503 65534 pro back-end stavu fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="3f745-163">Exceptions must be put in for incoming traffic on ports 65503-65534 for backend health to work correctly.</span></span>

* <span data-ttu-id="3f745-164">Odchozí připojení k Internetu, nejde blokovat.</span><span class="sxs-lookup"><span data-stu-id="3f745-164">Outbound internet connectivity can not be blocked.</span></span>

* <span data-ttu-id="3f745-165">Provoz z značka AzureLoadBalancer musí být povoleno.</span><span class="sxs-lookup"><span data-stu-id="3f745-165">Traffic from the AzureLoadBalancer tag must be allowed.</span></span>

<span data-ttu-id="3f745-166">**OTÁZKY. Jaké jsou omezení pro aplikační bránu? Může tyto limity zvýšit?**</span><span class="sxs-lookup"><span data-stu-id="3f745-166">**Q. What are the limits on Application Gateway? Can I increase these limits?**</span></span>

<span data-ttu-id="3f745-167">Navštivte [omezení brány aplikací](../azure-subscription-service-limits.md#application-gateway-limits) zobrazíte omezení.</span><span class="sxs-lookup"><span data-stu-id="3f745-167">Visit [Application Gateway Limits](../azure-subscription-service-limits.md#application-gateway-limits) to view the limits.</span></span>

<span data-ttu-id="3f745-168">**OTÁZKY. Lze použít aplikační brány pro externí i interní provoz současně?**</span><span class="sxs-lookup"><span data-stu-id="3f745-168">**Q. Can I use Application Gateway for both external and internal traffic simultaneously?**</span></span>

<span data-ttu-id="3f745-169">Ano, podporuje aplikační brány s jeden interní IP adresy a jeden externí IP adresu na aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="3f745-169">Yes, Application Gateway supports having one internal IP and one external IP per Application Gateway.</span></span>

<span data-ttu-id="3f745-170">**OTÁZKY. VNet peering je podporovaný?**</span><span class="sxs-lookup"><span data-stu-id="3f745-170">**Q. Is VNet peering supported?**</span></span>

<span data-ttu-id="3f745-171">Ano, partnerský vztah virtuální sítě je podporovaný a je výhodné pro provoz v jiných virtuálních sítí Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3f745-171">Yes, VNet peering is supported and is beneficial for load balancing traffic in other virtual networks.</span></span>

<span data-ttu-id="3f745-172">**OTÁZKY. Mi může sdělit místních serverů, když jsou připojeni pomocí ExpressRoute nebo VPN tunely?**</span><span class="sxs-lookup"><span data-stu-id="3f745-172">**Q. Can I talk to on-premises servers when they are connected by ExpressRoute or VPN tunnels?**</span></span>

<span data-ttu-id="3f745-173">Ano, tak dlouho, dokud provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="3f745-173">Yes, as long as traffic is allowed.</span></span>

<span data-ttu-id="3f745-174">**OTÁZKY. Může mít jeden fond back-end obsluhující mnoho aplikací na jiné porty?**</span><span class="sxs-lookup"><span data-stu-id="3f745-174">**Q. Can I have one backend pool serving many applications on different ports?**</span></span>

<span data-ttu-id="3f745-175">Architektura malých služby je podporována.</span><span class="sxs-lookup"><span data-stu-id="3f745-175">Micro service architecture is supported.</span></span> <span data-ttu-id="3f745-176">Potřebovali byste více nastavení http, které jsou nakonfigurované tak, aby sběru dat na jiné porty.</span><span class="sxs-lookup"><span data-stu-id="3f745-176">You would need multiple http settings configured to probe on different ports.</span></span>

<span data-ttu-id="3f745-177">**OTÁZKY. Podporují vlastní testy paměti na data odpovědi zástupné znaky/regex?**</span><span class="sxs-lookup"><span data-stu-id="3f745-177">**Q. Do custom probes support wildcards/regex on response data?**</span></span>

<span data-ttu-id="3f745-178">Vlastní testy paměti nepodporují zástupných znaků nebo regex na data odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3f745-178">Custom probes do not support wildcard or regex on response data.</span></span> 

<span data-ttu-id="3f745-179">**OTÁZKY. Jak se zpracovávají pravidla?**</span><span class="sxs-lookup"><span data-stu-id="3f745-179">**Q. How are rules processed?**</span></span>

<span data-ttu-id="3f745-180">Pravidla se zpracovávají v pořadí, ve kterém jsou nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="3f745-180">Rules are processed in the order they are configured.</span></span> <span data-ttu-id="3f745-181">Doporučuje se, zda pravidla více lokalit jsou nakonfigurovány před basic pravidla může snížit pravděpodobnost, že provoz se směruje na nevhodných back-end jako základní pravidlo odpovídá provozu na port před vyhodnocovaný pravidlo více lokalit.</span><span class="sxs-lookup"><span data-stu-id="3f745-181">It is recommended that multi-site rules are configured before basic rules to reduce the chance that traffic is routed to the inappropriate backend as the basic rule would match traffic based on port prior to the multi-site rule being evaluated.</span></span>

<span data-ttu-id="3f745-182">**OTÁZKY. Jak se zpracovávají pravidla?**</span><span class="sxs-lookup"><span data-stu-id="3f745-182">**Q. How are rules processed?**</span></span>

<span data-ttu-id="3f745-183">Pravidla se zpracovávají v pořadí, v jakém byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="3f745-183">Rules are processed in the order they are created.</span></span> <span data-ttu-id="3f745-184">Doporučuje se, že před základních pravidel jsou nakonfigurovaná pravidla více lokalit.</span><span class="sxs-lookup"><span data-stu-id="3f745-184">It is recommended that multi-site rules are configured before basic rules.</span></span> <span data-ttu-id="3f745-185">Nakonfigurováním nejprve naslouchací procesy více lokalit, tato konfigurace snižuje riziko, že provoz se směruje na nevhodných back-end.</span><span class="sxs-lookup"><span data-stu-id="3f745-185">By configuring multi-site listeners first, this configuration reduces the chance that traffic is routed to the inappropriate backend.</span></span> <span data-ttu-id="3f745-186">Tento problém směrování může docházet k základní pravidlo odpovídá provozu na port před vyhodnocovaný pravidlo více lokalit.</span><span class="sxs-lookup"><span data-stu-id="3f745-186">This routing issue can occur as the basic rule would match traffic based on port prior to the multi-site rule being evaluated.</span></span>

<span data-ttu-id="3f745-187">**OTÁZKY. Co označují pole hostitel pro vlastní testy paměti?**</span><span class="sxs-lookup"><span data-stu-id="3f745-187">**Q. What does the Host field for custom probes signify?**</span></span>

<span data-ttu-id="3f745-188">Pole hostitel Určuje název odeslat sondy k.</span><span class="sxs-lookup"><span data-stu-id="3f745-188">Host field specifies the name to send the probe to.</span></span> <span data-ttu-id="3f745-189">Platí jenom v případě více lokalit je nakonfigurovaná na aplikační bránu, v opačném případě použijte "127.0.0.1".</span><span class="sxs-lookup"><span data-stu-id="3f745-189">Applicable only when multi-site is configured on Application Gateway, otherwise use '127.0.0.1'.</span></span> <span data-ttu-id="3f745-190">Tato hodnota se liší od názvu hostitele virtuálních počítačů a je ve formátu \<protokol\>://\<hostitele\>:\<port\>\<cestu\>.</span><span class="sxs-lookup"><span data-stu-id="3f745-190">This value is different from VM host name and is in format \<protocol\>://\<host\>:\<port\>\<path\>.</span></span>

<span data-ttu-id="3f745-191">**OTÁZKY. Můžete vytvořit bílou Application Gateway přístup k několika zdrojové IP adresy?**</span><span class="sxs-lookup"><span data-stu-id="3f745-191">**Q. Can I whitelist Application Gateway access to a few source IPs?**</span></span>

<span data-ttu-id="3f745-192">Tento scénář lze provést pomocí skupin Nsg na podsítě brány aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f745-192">This scenario can be done using NSGs on Application Gateway subnet.</span></span> <span data-ttu-id="3f745-193">Následující omezení měly být umístěny v podsíti v uvedených pořadí podle priority:</span><span class="sxs-lookup"><span data-stu-id="3f745-193">The following restrictions should be put on the subnet in the listed order of priority:</span></span>

* <span data-ttu-id="3f745-194">Povolí příchozí provoz ze zdrojových rozsah IP/IP.</span><span class="sxs-lookup"><span data-stu-id="3f745-194">Allow incoming traffic from source IP/IP range.</span></span>

* <span data-ttu-id="3f745-195">Povolit příchozí požadavky ze všech zdrojů na porty 65503 65534 [komunikace stavu back-end](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="3f745-195">Allow incoming requests from all sources to ports 65503-65534 for [backend health communication](application-gateway-diagnostics.md).</span></span>

* <span data-ttu-id="3f745-196">Povolit příchozí nástroj pro vyrovnávání zatížení Azure sondy (značka AzureLoadBalancer) a příchozí přenosy virtuální sítě (virtuální síť značky) na [NSG](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="3f745-196">Allow incoming Azure Load Balancer probes (AzureLoadBalancer tag) and inbound virtual network traffic (VirtualNetwork tag) on the [NSG](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="3f745-197">Blokovat všechna ostatní příchozí přenosy s odepření všechna pravidla.</span><span class="sxs-lookup"><span data-stu-id="3f745-197">Block all other incoming traffic with a Deny all rule.</span></span>

* <span data-ttu-id="3f745-198">Povolte odchozí přenosy na Internet pro všechna místa určení.</span><span class="sxs-lookup"><span data-stu-id="3f745-198">Allow outbound traffic to the internet for all destinations.</span></span>

## <a name="performance"></a><span data-ttu-id="3f745-199">Výkon</span><span class="sxs-lookup"><span data-stu-id="3f745-199">Performance</span></span>

<span data-ttu-id="3f745-200">**OTÁZKY. Jak Application Gateway podporuje vysokou dostupnost a škálovatelnost?**</span><span class="sxs-lookup"><span data-stu-id="3f745-200">**Q. How does Application Gateway support high availability and scalability?**</span></span>

<span data-ttu-id="3f745-201">Aplikační brána podporuje scénáře s vysokou dostupností, až budete mít dva nebo více instancí nasazení.</span><span class="sxs-lookup"><span data-stu-id="3f745-201">Application Gateway supports high availability scenarios when you have two or more instances deployed.</span></span> <span data-ttu-id="3f745-202">Azure distribuuje tyto instance napříč doménami aktualizace a odolnost zajistit, aby všechny instance nedošlo k selhání ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="3f745-202">Azure distributes these instances across update and fault domains to ensure that all instances do not fail at the same time.</span></span> <span data-ttu-id="3f745-203">Aplikační brána podporuje škálovatelnost přidáním více instancí stejné bráně sdílení zatížení.</span><span class="sxs-lookup"><span data-stu-id="3f745-203">Application Gateway supports scalability by adding multiple instances of the same gateway to share the load.</span></span>

<span data-ttu-id="3f745-204">**OTÁZKY. Jak dosáhnout scénář zotavení po Havárii napříč datovými centry s Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-204">**Q. How do I achieve DR scenario across data centers with Application Gateway?**</span></span>

<span data-ttu-id="3f745-205">Zákazníci mohou Traffic Manager slouží k distribuci provoz napříč více bran aplikace v různých datových centrech.</span><span class="sxs-lookup"><span data-stu-id="3f745-205">Customers can use Traffic Manager to distribute traffic across multiple Application Gateways in different datacenters.</span></span>

<span data-ttu-id="3f745-206">**OTÁZKY. Automatické škálování je podporovaný?**</span><span class="sxs-lookup"><span data-stu-id="3f745-206">**Q. Is auto scaling supported?**</span></span>

<span data-ttu-id="3f745-207">Ne, ale Application Gateway poskytuje metriky propustnosti, který slouží k upozornění, když je dosaženo prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3f745-207">No, but Application Gateway has a throughput metric that can be used to alert you when a threshold is reached.</span></span> <span data-ttu-id="3f745-208">Ruční přidání instance nebo změna velikosti nerestartuje brány a nemá negativní vliv na stávající přenosy dat.</span><span class="sxs-lookup"><span data-stu-id="3f745-208">Manually adding instances or changing size does not restart the gateway and does not impact existing traffic.</span></span>

<span data-ttu-id="3f745-209">**OTÁZKY. Podporuje ruční škálování nahoru/dolů příčina výpadku?**</span><span class="sxs-lookup"><span data-stu-id="3f745-209">**Q. Does manual scale up/down cause downtime?**</span></span>

<span data-ttu-id="3f745-210">Neexistuje žádné výpadky, instancí jsou rozmístěny v upgradu domén a domén selhání.</span><span class="sxs-lookup"><span data-stu-id="3f745-210">There is no downtime, instances are distributed across upgrade domains and fault domains.</span></span>

<span data-ttu-id="3f745-211">**OTÁZKY. Můžete změnit velikost instance ze střední velké bez přerušení?**</span><span class="sxs-lookup"><span data-stu-id="3f745-211">**Q. Can I change instance size from medium to large without disruption?**</span></span>

<span data-ttu-id="3f745-212">Ano, Azure distribuuje instancí napříč doménami aktualizace a odolnost zajistit, aby všechny instance nedošlo k selhání ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="3f745-212">Yes, Azure distributes instances across update and fault domains to ensure that all instances do not fail at the same time.</span></span> <span data-ttu-id="3f745-213">Aplikační brána podporuje škálování přidáním více instancí stejné bráně sdílení zatížení.</span><span class="sxs-lookup"><span data-stu-id="3f745-213">Application Gateway supports scaling by adding multiple instances of the same gateway to share the load.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="3f745-214">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="3f745-214">SSL Configuration</span></span>

<span data-ttu-id="3f745-215">**OTÁZKY. Jaké certifikáty jsou podporovány ve Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-215">**Q. What certificates are supported on Application Gateway?**</span></span>

<span data-ttu-id="3f745-216">Vlastní podepsané certifikáty, certifikátů certifikační Autority a certifikátů zástupnému znaku. jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="3f745-216">Self signed certs, CA certs, and wild-card certs are supported.</span></span> <span data-ttu-id="3f745-217">Rozšířené ověření certifikátů nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="3f745-217">EV certs are not supported.</span></span>

<span data-ttu-id="3f745-218">**OTÁZKY. Jaké jsou aktuální šifrovací sada podporovaná Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-218">**Q. What are the current cipher suites supported by Application Gateway?**</span></span>

<span data-ttu-id="3f745-219">Níže jsou uvedeny aktuální šifrovací sada podporovaná aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="3f745-219">The following are the current cipher suites supported by application gateway.</span></span> <span data-ttu-id="3f745-220">Navštivte: [konfigurace SSL verze zásad a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) se dozvíte, jak přizpůsobit možnosti protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="3f745-220">Visit: [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) to learn how to customize SSL options.</span></span>

- <span data-ttu-id="3f745-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="3f745-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="3f745-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="3f745-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="3f745-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="3f745-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="3f745-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="3f745-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="3f745-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="3f745-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="3f745-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="3f745-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="3f745-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="3f745-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="3f745-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="3f745-233">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-233">TLS_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="3f745-234">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-234">TLS_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="3f745-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="3f745-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="3f745-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="3f745-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="3f745-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="3f745-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="3f745-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="3f745-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="3f745-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="3f745-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="3f745-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="3f745-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="3f745-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span>
- <span data-ttu-id="3f745-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="3f745-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span>

<span data-ttu-id="3f745-247">**OTÁZKY. Application Gateway také podporuje znova šifrovat provoz na back-end?**</span><span class="sxs-lookup"><span data-stu-id="3f745-247">**Q. Does Application Gateway also support re-encryption of traffic to the backend?**</span></span>

<span data-ttu-id="3f745-248">Ano, aplikační brána podporuje přesměrování zpracování SSL a koncová SSL, který znovu zašifruje provoz na back-end.</span><span class="sxs-lookup"><span data-stu-id="3f745-248">Yes, Application Gateway supports SSL offload, and end to end SSL, which re-encrypts the traffic to the backend.</span></span>

<span data-ttu-id="3f745-249">**OTÁZKY. Můžete nakonfigurovat zásady protokolu SSL pro řízení verzí protokolu SSL?**</span><span class="sxs-lookup"><span data-stu-id="3f745-249">**Q. Can I configure SSL policy to control SSL Protocol versions?**</span></span>

<span data-ttu-id="3f745-250">Ano, můžete nakonfigurovat tak, aby odepřel TLS1.0, TLS1.1 a TLS1.2 Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="3f745-250">Yes, you can configure Application Gateway to deny TLS1.0, TLS1.1, and TLS1.2.</span></span> <span data-ttu-id="3f745-251">Protokol SSL 2.0 a 3.0 jsou už ve výchozím nastavení zakázána a se nedají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="3f745-251">SSL 2.0 and 3.0 are already disabled by default and are not configurable.</span></span>

<span data-ttu-id="3f745-252">**OTÁZKY. Můžete nakonfigurovat šifrovací sady a pořadí zásad?**</span><span class="sxs-lookup"><span data-stu-id="3f745-252">**Q. Can I configure cipher suites and policy order?**</span></span>

<span data-ttu-id="3f745-253">Ano, [konfigurace šifrovací sady](application-gateway-ssl-policy-overview.md) je podporována.</span><span class="sxs-lookup"><span data-stu-id="3f745-253">Yes, [configuration of cipher suites](application-gateway-ssl-policy-overview.md) is supported.</span></span> <span data-ttu-id="3f745-254">Při definování vlastní zásady, musí být povolena alespoň jeden z následujících sad šifer.</span><span class="sxs-lookup"><span data-stu-id="3f745-254">When defining a custom policy, at least one of the following cipher suites must be enabled.</span></span> <span data-ttu-id="3f745-255">Aplikační brána používá SHA256 k pro správu back-end.</span><span class="sxs-lookup"><span data-stu-id="3f745-255">Application gateway uses SHA256 to for backend management.</span></span>

* <span data-ttu-id="3f745-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span> 
* <span data-ttu-id="3f745-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
* <span data-ttu-id="3f745-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="3f745-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="3f745-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
* <span data-ttu-id="3f745-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="3f745-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>

<span data-ttu-id="3f745-262">**OTÁZKY. Jak velký počet certifikátů SSL jsou podporovány?**</span><span class="sxs-lookup"><span data-stu-id="3f745-262">**Q. How many SSL certificates are supported?**</span></span>

<span data-ttu-id="3f745-263">Až 20 SSL certifikáty jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="3f745-263">Up to 20 SSL certificates are supported.</span></span>

<span data-ttu-id="3f745-264">**OTÁZKY. Kolik ověřovací certifikáty pro back-end znova šifrovat jsou podporovány?**</span><span class="sxs-lookup"><span data-stu-id="3f745-264">**Q. How many authentication certificates for backend re-encryption are supported?**</span></span>

<span data-ttu-id="3f745-265">Až 10 ověřování certifikáty jsou podporovány výchozí hodnota je 5.</span><span class="sxs-lookup"><span data-stu-id="3f745-265">Up to 10 authentication certificates are supported with a default of 5.</span></span>

<span data-ttu-id="3f745-266">**OTÁZKY. Umožňuje Application Gateway integraci s Azure Key Vault nativně?**</span><span class="sxs-lookup"><span data-stu-id="3f745-266">**Q. Does Application Gateway integrate with Azure Key Vault natively?**</span></span>

<span data-ttu-id="3f745-267">Ne, není integrované s Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3f745-267">No, it is not integrated with Azure Key Vault.</span></span>

## <a name="web-application-firewall-waf-configuration"></a><span data-ttu-id="3f745-268">Konfigurace brány Firewall (firewall webových aplikací) webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3f745-268">Web Application Firewall (WAF) Configuration</span></span>

<span data-ttu-id="3f745-269">**OTÁZKY. SKU firewall webových aplikací nabízí všechny funkce, které jsou k dispozici standardní SKU systém?**</span><span class="sxs-lookup"><span data-stu-id="3f745-269">**Q. Does the WAF SKU offer all the features available with the Standard SKU?**</span></span>

<span data-ttu-id="3f745-270">Ano, firewall webových aplikací podporuje všechny funkce v standardní SKU.</span><span class="sxs-lookup"><span data-stu-id="3f745-270">Yes, WAF supports all the features in the Standard SKU.</span></span>

<span data-ttu-id="3f745-271">**OTÁZKY. Co je verze řádku Application Gateway podporuje?**</span><span class="sxs-lookup"><span data-stu-id="3f745-271">**Q. What is the CRS version Application Gateway supports?**</span></span>

<span data-ttu-id="3f745-272">Aplikační brána podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a řádku [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span><span class="sxs-lookup"><span data-stu-id="3f745-272">Application Gateway supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span></span>

<span data-ttu-id="3f745-273">**OTÁZKY. Jak se monitorování firewall webových aplikací?**</span><span class="sxs-lookup"><span data-stu-id="3f745-273">**Q. How do I monitor WAF?**</span></span>

<span data-ttu-id="3f745-274">Firewall webových aplikací je monitorována prostřednictvím protokolování diagnostiky, další informace o protokolování diagnostiky naleznete na [protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="3f745-274">WAF is monitored through diagnostic logging, more information on diagnostic logging can be found at [Diagnostics Logging and Metrics for Application Gateway](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="3f745-275">**OTÁZKY. Detekce režimu blokování provozu?**</span><span class="sxs-lookup"><span data-stu-id="3f745-275">**Q. Does detection mode block traffic?**</span></span>

<span data-ttu-id="3f745-276">Ne, detekce režimu jenom protokoly přenosy, která spustí pravidlo firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="3f745-276">No, detection mode only logs traffic, which triggered a WAF rule.</span></span>

<span data-ttu-id="3f745-277">**OTÁZKY. Jak lze přizpůsobit pravidla firewall webových aplikací?**</span><span class="sxs-lookup"><span data-stu-id="3f745-277">**Q. How do I customize WAF rules?**</span></span>

<span data-ttu-id="3f745-278">Ano, pravidla firewall webových aplikací jsou přizpůsobitelné, další informace o tom, jak přizpůsobit návštěvu [skupiny pravidel přizpůsobit firewall webových aplikací a pravidla](application-gateway-customize-waf-rules-portal.md)</span><span class="sxs-lookup"><span data-stu-id="3f745-278">Yes, WAF rules are customizable, for more information on how to customize them visit [Customize WAF rule groups and rules](application-gateway-customize-waf-rules-portal.md)</span></span>

<span data-ttu-id="3f745-279">**OTÁZKY. Jaká pravidla jsou nyní k dispozici?**</span><span class="sxs-lookup"><span data-stu-id="3f745-279">**Q. What rules are currently available?**</span></span>

<span data-ttu-id="3f745-280">Firewall webových aplikací v současné době podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), které poskytují základní zabezpečení proti většinu prvních 10 ohrožení zabezpečení, identifikovat pomocí otevřít webové aplikace zabezpečení projektu (OWASP) je zde uveden [OWASP top 10 ohrožení zabezpečení](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span><span class="sxs-lookup"><span data-stu-id="3f745-280">WAF currently supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), which provide baseline security against most of the top 10 vulnerabilities identified by the Open Web Application Security Project (OWASP) found here [OWASP top 10 Vulnerabilities](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span></span>

* <span data-ttu-id="3f745-281">Ochrana před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="3f745-281">SQL injection protection</span></span>

* <span data-ttu-id="3f745-282">Ochrana před skriptováním mezi weby.</span><span class="sxs-lookup"><span data-stu-id="3f745-282">Cross site scripting protection</span></span>

* <span data-ttu-id="3f745-283">Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.</span><span class="sxs-lookup"><span data-stu-id="3f745-283">Common Web Attacks Protection such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attack</span></span>

* <span data-ttu-id="3f745-284">Ochrana před narušením protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f745-284">Protection against HTTP protocol violations</span></span>

* <span data-ttu-id="3f745-285">Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.</span><span class="sxs-lookup"><span data-stu-id="3f745-285">Protection against HTTP protocol anomalies such as missing host user-agent and accept headers</span></span>

* <span data-ttu-id="3f745-286">Ochrana před roboty, prohledávacími moduly a skenery.</span><span class="sxs-lookup"><span data-stu-id="3f745-286">Prevention against bots, crawlers, and scanners</span></span>

* <span data-ttu-id="3f745-287">Detekce časté nesprávné konfigurace aplikace (tedy Apache, IIS, atd.)</span><span class="sxs-lookup"><span data-stu-id="3f745-287">Detection of common application misconfigurations (that is, Apache, IIS, etc.)</span></span>

<span data-ttu-id="3f745-288">**OTÁZKY. Firewall webových aplikací taky podporuje DDoS prevence?**</span><span class="sxs-lookup"><span data-stu-id="3f745-288">**Q. Does WAF also support DDoS prevention?**</span></span>

<span data-ttu-id="3f745-289">Ne, firewall webových aplikací neposkytuje DDoS prevence.</span><span class="sxs-lookup"><span data-stu-id="3f745-289">No, WAF does not provide DDoS prevention.</span></span>

## <a name="diagnostics-and-logging"></a><span data-ttu-id="3f745-290">Protokolování a diagnostiky</span><span class="sxs-lookup"><span data-stu-id="3f745-290">Diagnostics and Logging</span></span>

<span data-ttu-id="3f745-291">**OTÁZKY. Jaké typy protokolů jsou k dispozici s Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-291">**Q. What types of logs are available with Application Gateway?**</span></span>

<span data-ttu-id="3f745-292">Nejsou k dispozici pro službu Application Gateway tři protokoly.</span><span class="sxs-lookup"><span data-stu-id="3f745-292">There are three logs available for Application Gateway.</span></span> <span data-ttu-id="3f745-293">Další informace o těchto protokolů a jiné diagnostické funkce, najdete v článku [back-end stavu, protokolů diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="3f745-293">For more information on these logs and other diagnostic capabilities, visit [Backend health, diagnostics logs, and metrics for Application Gateway](application-gateway-diagnostics.md).</span></span>

- <span data-ttu-id="3f745-294">**ApplicationGatewayAccessLog** – protokol přístupu obsahuje každý odeslaný požadavek na front-endu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="3f745-294">**ApplicationGatewayAccessLog** - The access log contains each request submitted to the Application Gateway frontend.</span></span> <span data-ttu-id="3f745-295">Data obsahují volajícího IP adresy, požadovaná, adresa URL odpovědi latence, návratový kód, bajtů a odhlášení. Protokol přístupu se shromažďují každých 300 sekund.</span><span class="sxs-lookup"><span data-stu-id="3f745-295">The data includes the caller's IP, URL requested, response latency, return code, bytes in and out. Access log is collected every 300 seconds.</span></span> <span data-ttu-id="3f745-296">Tento protokol obsahuje jeden záznam za instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="3f745-296">This log contains one record per instance of Application Gateway.</span></span>
- <span data-ttu-id="3f745-297">**ApplicationGatewayPerformanceLog** -výkonu protokolu zaznamená informace o výkonu na základě za instance včetně celkový požadavek zpracovat, propustnost v bajtech, počtu žádostí o obsloužit se nezdařila celkový počet požadavků, počet instancí back-end v pořádku a není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="3f745-297">**ApplicationGatewayPerformanceLog** - The performance log captures performance information on per instance basis including total request served, throughput in bytes, total requests served, failed request count, healthy and unhealthy back-end instance count.</span></span>
- <span data-ttu-id="3f745-298">**ApplicationGatewayFirewallLog** -protokolu brány firewall obsahuje požadavky, které se protokolují prostřednictvím zjišťování nebo zabránění režim služby application gateway, která je konfigurovaná pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="3f745-298">**ApplicationGatewayFirewallLog** - The firewall log contains requests that are logged through either detection or prevention mode of an application gateway that is configured with web application firewall.</span></span>

<span data-ttu-id="3f745-299">**OTÁZKY. Jak poznám, pokud jsou moje členy fondu back-end v pořádku?**</span><span class="sxs-lookup"><span data-stu-id="3f745-299">**Q. How do I know if my backend pool members are healthy?**</span></span>

<span data-ttu-id="3f745-300">Můžete použít rutiny prostředí PowerShell `Get-AzureRmApplicationGatewayBackendHealth` nebo ověřte stav prostřednictvím portálu, navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="3f745-300">You can use the PowerShell cmdlet `Get-AzureRmApplicationGatewayBackendHealth` or verify health through the portal by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="3f745-301">**OTÁZKY. Co je zásady uchovávání informací na protokolů diagnostiky?**</span><span class="sxs-lookup"><span data-stu-id="3f745-301">**Q. What is the retention policy on the diagnostics logs?**</span></span>

<span data-ttu-id="3f745-302">Diagnostické protokoly toku k účtu úložiště zákazníků a zákazníků můžete nastavit zásady uchovávání informací podle jejich předvoleb.</span><span class="sxs-lookup"><span data-stu-id="3f745-302">Diagnostic logs flow to the customers storage account and customers can set the retention policy based on their preference.</span></span> <span data-ttu-id="3f745-303">Diagnostické protokoly můžete odeslat také do centra událostí nebo analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3f745-303">Diagnostic logs can also be sent to an Event Hub or Log Analytics.</span></span> <span data-ttu-id="3f745-304">Navštivte [Application Diagnostics brány](application-gateway-diagnostics.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3f745-304">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) for more details.</span></span>

<span data-ttu-id="3f745-305">**OTÁZKY. Získání protokolů auditu pro službu Application Gateway**</span><span class="sxs-lookup"><span data-stu-id="3f745-305">**Q. How do I get audit logs for Application Gateway?**</span></span>

<span data-ttu-id="3f745-306">Protokoly auditu jsou k dispozici pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="3f745-306">Audit logs are available for Application Gateway.</span></span> <span data-ttu-id="3f745-307">Na portálu, klikněte na tlačítko **protokol aktivit** v okně nabídky aplikační brány pro přístup k protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="3f745-307">In the portal, click **Activity Log** in the menu blade of an Application Gateway to access the audit log.</span></span> 

<span data-ttu-id="3f745-308">**OTÁZKY. Můžete nastavit výstrahy s Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="3f745-308">**Q. Can I set alerts with Application Gateway?**</span></span>

<span data-ttu-id="3f745-309">Ano, aplikační brána podporuje výstrahy, se konfigurují vypnout metriky.</span><span class="sxs-lookup"><span data-stu-id="3f745-309">Yes, Application Gateway does support alerts, alerts are configured off metrics.</span></span>  <span data-ttu-id="3f745-310">Aplikační brána v současnosti má metriky propustnosti"", kterého lze nakonfigurovat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="3f745-310">Application Gateway currently has a metric of "throughput", which can be configured to alert.</span></span> <span data-ttu-id="3f745-311">Další informace o výstrahách naleznete [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="3f745-311">To learn more about alerts, visit [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="3f745-312">**OTÁZKY. Back-end stavu vrátí Neznámý stav, co by mohlo být příčinou tento stav?**</span><span class="sxs-lookup"><span data-stu-id="3f745-312">**Q. Backend health returns unknown status, what could be causing this status?**</span></span>

<span data-ttu-id="3f745-313">Nejčastější příčinou je skupina NSG nebo vlastní DNS je blokován přístup k back-end.</span><span class="sxs-lookup"><span data-stu-id="3f745-313">The most common reason is access to the backend is being blocked by an NSG or custom DNS.</span></span> <span data-ttu-id="3f745-314">Navštivte [back-end stavu, protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3f745-314">Visit [Backend health, diagnostics logging, and metrics for Application Gateway](application-gateway-diagnostics.md) to learn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f745-315">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f745-315">Next Steps</span></span>

<span data-ttu-id="3f745-316">Další informace o návštěvě Application Gateway [Úvod do Application Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f745-316">To learn more about Application Gateway visit [Introduction to Application Gateway](application-gateway-introduction.md).</span></span>
