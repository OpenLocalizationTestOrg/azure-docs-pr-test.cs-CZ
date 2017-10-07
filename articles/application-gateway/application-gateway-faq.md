---
title: aaaFrequently dotazy pro Azure Application Gateway | Microsoft Docs
description: "Tato stránka obsahuje odpovědi toofrequently kladené dotazy týkající se Azure Application Gateway"
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
ms.openlocfilehash: b2df3a82a71a3264d3d34d317d08e4b4f72c6e3e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a><span data-ttu-id="962d7-103">Nejčastější dotazy pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="962d7-103">Frequently asked questions for Application Gateway</span></span>

## <a name="general"></a><span data-ttu-id="962d7-104">Obecné</span><span class="sxs-lookup"><span data-stu-id="962d7-104">General</span></span>

<span data-ttu-id="962d7-105">**OTÁZKY. Co je aplikační brána?**</span><span class="sxs-lookup"><span data-stu-id="962d7-105">**Q. What is Application Gateway?**</span></span>

<span data-ttu-id="962d7-106">Služba Azure Application Gateway je řadiče doručení aplikace (ADC) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="962d7-106">Azure Application Gateway is an Application Delivery Controller (ADC) as a service, offering various layer 7 load balancing capabilities for your applications.</span></span> <span data-ttu-id="962d7-107">Nabízí vysoce dostupné a škálovatelné služby, která je plně spravovaná službou Azure.</span><span class="sxs-lookup"><span data-stu-id="962d7-107">It offers highly available and scalable service, which is fully managed by Azure.</span></span>

<span data-ttu-id="962d7-108">**OTÁZKY. Jaké funkce podporuje Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-108">**Q. What features does Application Gateway support?**</span></span>

<span data-ttu-id="962d7-109">Aplikační brána podporuje protokol SSL snižování zátěže a end tooend SSL, brány Firewall webových aplikací, spřažení na základě souboru cookie relace, adresa url na základě cesty směrování, hostování více lokality a ostatní.</span><span class="sxs-lookup"><span data-stu-id="962d7-109">Application Gateway supports SSL offloading and end tooend SSL, Web Application Firewall, cookie-based session affinity, url path-based routing, multi site hosting, and others.</span></span> <span data-ttu-id="962d7-110">Úplný seznam podporovaných funkcích najdete na adrese [Úvod tooApplication brány](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="962d7-110">For a full list of supported features, visit [Introduction tooApplication Gateway](application-gateway-introduction.md)</span></span>

<span data-ttu-id="962d7-111">**OTÁZKY. Co je hello rozdíl mezi aplikační bránu a nástroj pro vyrovnávání zatížení Azure?**</span><span class="sxs-lookup"><span data-stu-id="962d7-111">**Q. What is hello difference between Application Gateway and Azure Load Balancer?**</span></span>

<span data-ttu-id="962d7-112">Application Gateway je Vyrovnávání zatížení vrstvy 7, což znamená, že pracuje pouze webový provoz (HTTP nebo HTTPS/WebSocket).</span><span class="sxs-lookup"><span data-stu-id="962d7-112">Application Gateway is a layer 7 load balancer, which means it works with web traffic only (HTTP/HTTPS/WebSocket).</span></span> <span data-ttu-id="962d7-113">Podporuje možnosti, například ukončení protokolu SSL, spřažení relace na základě souborů cookie a kruhové dotazování pro provoz služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="962d7-113">It supports capabilities such as SSL termination, cookie-based session affinity, and round robin for load balancing traffic.</span></span> <span data-ttu-id="962d7-114">Nástroj pro vyrovnávání zatížení a načtěte provozu zůstatky na vrstvě 4 (TCP/UDP).</span><span class="sxs-lookup"><span data-stu-id="962d7-114">Load Balancer, load balances traffic at layer 4 (TCP/UDP).</span></span>

<span data-ttu-id="962d7-115">**OTÁZKY. Jaké protokoly podporuje Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-115">**Q. What protocols does Application Gateway support?**</span></span>

<span data-ttu-id="962d7-116">Aplikační brána podporuje protokoly HTTP, HTTPS a protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="962d7-116">Application Gateway supports HTTP, HTTPS, and WebSocket.</span></span>

<span data-ttu-id="962d7-117">**OTÁZKY. Jaké prostředky jsou dnes podporovány v rámci fondu back-end?**</span><span class="sxs-lookup"><span data-stu-id="962d7-117">**Q. What resources are supported today as part of backend pool?**</span></span>

<span data-ttu-id="962d7-118">Back-endové fondy může skládat ze síťových adaptérů, sady škálování virtuálního počítače, veřejné IP adresy, názvy interní IP adres, plně kvalifikované domény (FQDN) a víceklientské back EndY jako Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="962d7-118">Backend pools can be composed of NICs, virtual machine scale sets, public IPs, internal IPs, fully qualified domain names (FQDN), and multi-tenant back-ends like Azure Web Apps.</span></span> <span data-ttu-id="962d7-119">Aplikační brány, které nejsou členy fondu back-end svázané tooan sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="962d7-119">Application Gateway backend pool members are not tied tooan availability set.</span></span> <span data-ttu-id="962d7-120">Členy fondu back-end může být napříč clustery, datových center, nebo mimo Azure, dokud mají připojení pomocí protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="962d7-120">Members of backend pools can be across clusters, data centers, or outside of Azure as long as they have IP connectivity.</span></span>

<span data-ttu-id="962d7-121">**OTÁZKY. Jaké oblasti je dostupná v hello služby?**</span><span class="sxs-lookup"><span data-stu-id="962d7-121">**Q. What regions is hello service available in?**</span></span>

<span data-ttu-id="962d7-122">Application Gateway je k dispozici ve všech oblastech globální Azure.</span><span class="sxs-lookup"><span data-stu-id="962d7-122">Application Gateway is available in all regions of global Azure.</span></span> <span data-ttu-id="962d7-123">Je také dostupná v [Azure China](https://www.azure.cn/) a [Azure Government.](https://azure.microsoft.com/en-us/overview/clouds/government/)</span><span class="sxs-lookup"><span data-stu-id="962d7-123">It is also available in [Azure China](https://www.azure.cn/) and [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span></span>

<span data-ttu-id="962d7-124">**OTÁZKY. To je vyhrazený pro Moje předplatné nasazení nebo je sdílen na zákazníky?**</span><span class="sxs-lookup"><span data-stu-id="962d7-124">**Q. Is this a dedicated deployment for my subscription or is it shared across customers?**</span></span>

<span data-ttu-id="962d7-125">Application Gateway je vyhrazené nasazení ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="962d7-125">Application Gateway is a dedicated deployment in your virtual network.</span></span>

<span data-ttu-id="962d7-126">**OTÁZKY. Je HTTP -> HTTPS přesměrování podporovány?**</span><span class="sxs-lookup"><span data-stu-id="962d7-126">**Q. Is HTTP->HTTPS redirection supported?**</span></span>

<span data-ttu-id="962d7-127">Přesměrování je podporována.</span><span class="sxs-lookup"><span data-stu-id="962d7-127">Redirection is supported.</span></span> <span data-ttu-id="962d7-128">Navštivte [přehled přesměrování Application Gateway](application-gateway-redirect-overview.md) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="962d7-128">Visit [Application Gateway redirect overview](application-gateway-redirect-overview.md) toolearn more.</span></span>

<span data-ttu-id="962d7-129">**OTÁZKY. Pořadí, v jakém jsou naslouchací procesy zpracování?**</span><span class="sxs-lookup"><span data-stu-id="962d7-129">**Q. In what order are listeners processed?**</span></span>

<span data-ttu-id="962d7-130">Moduly pro naslouchání jsou zpracovány v hello pořadí, ve kterém jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="962d7-130">Listeners are processed in hello order they are shown.</span></span> <span data-ttu-id="962d7-131">Z tohoto důvodu Pokud základní naslouchací proces odpovídá příchozí požadavek zpracuje jej nejprve.</span><span class="sxs-lookup"><span data-stu-id="962d7-131">For that reason if a basic listener matches an incoming request it processes it first.</span></span>  <span data-ttu-id="962d7-132">Moduly pro naslouchání více lokalit by měl být nakonfigurovaný před přenosem tooensure základní naslouchací proces směrované toohello správné back-end.</span><span class="sxs-lookup"><span data-stu-id="962d7-132">Multi-site listeners should be configured before a basic listener tooensure traffic is routed toohello correct back-end.</span></span>

<span data-ttu-id="962d7-133">**OTÁZKY. Kde najít IP a DNS Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-133">**Q. Where do I find Application Gateway’s IP and DNS?**</span></span>

<span data-ttu-id="962d7-134">Pokud používáte veřejnou IP adresu jako koncový bod, tyto informace naleznete na prostředek hello veřejné IP adresy nebo na stránku přehled hello pro hello Application Gateway hello portálu.</span><span class="sxs-lookup"><span data-stu-id="962d7-134">When using a public IP address as an endpoint, this information can be found on hello public IP address resource or on hello Overview page for hello Application Gateway in hello portal.</span></span> <span data-ttu-id="962d7-135">Pro interní IP adresy najdete na stránce Přehled hello.</span><span class="sxs-lookup"><span data-stu-id="962d7-135">For internal IP addresses, this can be found on hello Overview page.</span></span>

<span data-ttu-id="962d7-136">**OTÁZKY. Hello IP adresu nebo DNS mění přes hello životnost hello Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-136">**Q. Does hello IP or DNS change over hello lifetime of hello Application Gateway?**</span></span>

<span data-ttu-id="962d7-137">Pokud po zastavení a spuštění zákazníkem hello hello brány, můžete změnit Hello VIP.</span><span class="sxs-lookup"><span data-stu-id="962d7-137">hello VIP can change if hello gateway is stopped and started by hello customer.</span></span> <span data-ttu-id="962d7-138">Hello DNS přidružené Application Gateway přes životního cyklu hello hello brány nezmění.</span><span class="sxs-lookup"><span data-stu-id="962d7-138">hello DNS associated with Application Gateway does not change over hello lifecycle of hello gateway.</span></span> <span data-ttu-id="962d7-139">Z tohoto důvodu je doporučeno toouse CNAME alias a nasměrujete ho adresa DNS toohello hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="962d7-139">For this reason, it is recommended toouse a CNAME alias and point it toohello DNS address of hello Application Gateway.</span></span>

<span data-ttu-id="962d7-140">**OTÁZKY. Podporuje Application Gateway statickou IP adresu?**</span><span class="sxs-lookup"><span data-stu-id="962d7-140">**Q. Does Application Gateway support static IP?**</span></span>

<span data-ttu-id="962d7-141">Ne, aplikační brána nepodporuje statické veřejné IP adresy, ale podporuje statické interní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="962d7-141">No, Application Gateway does not support static public IP addresses, but it does support static internal IPs.</span></span>

<span data-ttu-id="962d7-142">**OTÁZKY. Podporuje Application Gateway víc veřejných IP adres v bráně hello?**</span><span class="sxs-lookup"><span data-stu-id="962d7-142">**Q. Does Application Gateway support multiple public IPs on hello gateway?**</span></span>

<span data-ttu-id="962d7-143">Na aplikační brány je podporovaná pouze jednu veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="962d7-143">Only one public IP address is supported on an Application Gateway.</span></span>

<span data-ttu-id="962d7-144">**OTÁZKY. Podporuje Application Gateway hlavičky x předávaných pro?**</span><span class="sxs-lookup"><span data-stu-id="962d7-144">**Q. Does Application Gateway support x-forwarded-for headers?**</span></span>

<span data-ttu-id="962d7-145">Ano, Application Gateway vloží x předávaných pro, x předávaných proto a hlavičky x předávaných port do žádosti o hello předávaných toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="962d7-145">Yes, Application Gateway inserts x-forwarded-for, x-forwarded-proto, and x-forwarded-port headers into hello request forwarded toohello backend.</span></span> <span data-ttu-id="962d7-146">Hello formát hlavičky x předávaných pro je čárkami oddělený seznam IP: port.</span><span class="sxs-lookup"><span data-stu-id="962d7-146">hello format for x-forwarded-for header is a comma-separated list of IP:Port.</span></span> <span data-ttu-id="962d7-147">Hello platné hodnoty x předávaných proto jsou protokolu http nebo https.</span><span class="sxs-lookup"><span data-stu-id="962d7-147">hello valid values for x-forwarded-proto are http or https.</span></span> <span data-ttu-id="962d7-148">X předávaných port Určuje hello port, na které hello žádost dosáhla hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="962d7-148">X-forwarded-port specifies hello port at which hello request reached at hello Application Gateway.</span></span>

<span data-ttu-id="962d7-149">**OTÁZKY. Jak dlouho trvá toodeploy služby Application Gateway? Moje aplikace brány stále funguje při aktualizaci?**</span><span class="sxs-lookup"><span data-stu-id="962d7-149">**Q. How long does it take toodeploy an Application Gateway? Does my Application Gateway still work when being updated?**</span></span>

<span data-ttu-id="962d7-150">Nová nasazení aplikací brány může trvat až tooprovision too20 minut.</span><span class="sxs-lookup"><span data-stu-id="962d7-150">New Application Gateway deployments can take up too20 minutes tooprovision.</span></span> <span data-ttu-id="962d7-151">Tooinstance změny velikosti nebo počet nejsou rušivý a hello brány zůstává aktivní během této doby.</span><span class="sxs-lookup"><span data-stu-id="962d7-151">Changes tooinstance size/count are not disruptive, and hello gateway remains active during this time.</span></span>

## <a name="configuration"></a><span data-ttu-id="962d7-152">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="962d7-152">Configuration</span></span>

<span data-ttu-id="962d7-153">**OTÁZKY. Je aplikační brána vždy nasazený ve virtuální síti?**</span><span class="sxs-lookup"><span data-stu-id="962d7-153">**Q. Is Application Gateway always deployed in a virtual network?**</span></span>

<span data-ttu-id="962d7-154">Ano, aplikační brány je vždy nasazena v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="962d7-154">Yes, Application Gateway is always deployed in a virtual network subnet.</span></span> <span data-ttu-id="962d7-155">Tato podsíť může obsahovat pouze Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="962d7-155">This subnet can only contain Application Gateways.</span></span>

<span data-ttu-id="962d7-156">**OTÁZKY. Může kontaktovat Application Gateway tooinstances mimo jeho virtuální síť?**</span><span class="sxs-lookup"><span data-stu-id="962d7-156">**Q. Can Application Gateway talk tooinstances outside its virtual network?**</span></span>

<span data-ttu-id="962d7-157">Aplikační brána může kontaktovat tooinstances mimo hello virtuální síť, která je v, dokud není IP připojení.</span><span class="sxs-lookup"><span data-stu-id="962d7-157">Application Gateway can talk tooinstances outside of hello virtual network that it is in as long as there is IP connectivity.</span></span> <span data-ttu-id="962d7-158">Pokud máte v plánu toouse vyžaduje interní IP adresy jako členy fondu back-end, pak [VNET Peering](../virtual-network/virtual-network-peering-overview.md) nebo [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="962d7-158">If you plan toouse internal IPs as backend pool members, then it requires [VNET Peering](../virtual-network/virtual-network-peering-overview.md) or [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>

<span data-ttu-id="962d7-159">**OTÁZKY. Lze nasadit nic jiného v podsíť brány aplikace hello?**</span><span class="sxs-lookup"><span data-stu-id="962d7-159">**Q. Can I deploy anything else in hello Application Gateway subnet?**</span></span>

<span data-ttu-id="962d7-160">Ne, ale můžete nasadit další aplikace v hello podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="962d7-160">No, but you can deploy other application gateways in hello subnet.</span></span>

<span data-ttu-id="962d7-161">**OTÁZKY. Podporuje na podsíť brány aplikace hello skupin zabezpečení sítě?**</span><span class="sxs-lookup"><span data-stu-id="962d7-161">**Q. Are Network Security Groups supported on hello Application Gateway subnet?**</span></span>

<span data-ttu-id="962d7-162">Skupiny zabezpečení sítě jsou podporovány v podsíti hello Application Gateway s hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="962d7-162">Network Security Groups are supported on hello Application Gateway subnet with hello following restrictions:</span></span>

* <span data-ttu-id="962d7-163">Výjimky musí být umístěno v pro příchozí komunikaci na portech 65503 65534 pro back-end stavu toowork správně.</span><span class="sxs-lookup"><span data-stu-id="962d7-163">Exceptions must be put in for incoming traffic on ports 65503-65534 for backend health toowork correctly.</span></span>

* <span data-ttu-id="962d7-164">Odchozí připojení k Internetu, nejde blokovat.</span><span class="sxs-lookup"><span data-stu-id="962d7-164">Outbound internet connectivity can not be blocked.</span></span>

* <span data-ttu-id="962d7-165">Provoz z hello značka AzureLoadBalancer musí být povoleno.</span><span class="sxs-lookup"><span data-stu-id="962d7-165">Traffic from hello AzureLoadBalancer tag must be allowed.</span></span>

<span data-ttu-id="962d7-166">**OTÁZKY. Jaká jsou omezení hello ve Application Gateway? Může tyto limity zvýšit?**</span><span class="sxs-lookup"><span data-stu-id="962d7-166">**Q. What are hello limits on Application Gateway? Can I increase these limits?**</span></span>

<span data-ttu-id="962d7-167">Navštivte [omezení brány aplikací](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello omezení.</span><span class="sxs-lookup"><span data-stu-id="962d7-167">Visit [Application Gateway Limits](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello limits.</span></span>

<span data-ttu-id="962d7-168">**OTÁZKY. Lze použít aplikační brány pro externí i interní provoz současně?**</span><span class="sxs-lookup"><span data-stu-id="962d7-168">**Q. Can I use Application Gateway for both external and internal traffic simultaneously?**</span></span>

<span data-ttu-id="962d7-169">Ano, podporuje aplikační brány s jeden interní IP adresy a jeden externí IP adresu na aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="962d7-169">Yes, Application Gateway supports having one internal IP and one external IP per Application Gateway.</span></span>

<span data-ttu-id="962d7-170">**OTÁZKY. VNet peering je podporovaný?**</span><span class="sxs-lookup"><span data-stu-id="962d7-170">**Q. Is VNet peering supported?**</span></span>

<span data-ttu-id="962d7-171">Ano, partnerský vztah virtuální sítě je podporovaný a je výhodné pro provoz v jiných virtuálních sítí Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="962d7-171">Yes, VNet peering is supported and is beneficial for load balancing traffic in other virtual networks.</span></span>

<span data-ttu-id="962d7-172">**OTÁZKY. Může kontaktovat tooon místní servery, když jsou připojeni pomocí ExpressRoute nebo VPN tunely?**</span><span class="sxs-lookup"><span data-stu-id="962d7-172">**Q. Can I talk tooon-premises servers when they are connected by ExpressRoute or VPN tunnels?**</span></span>

<span data-ttu-id="962d7-173">Ano, tak dlouho, dokud provoz je povolený.</span><span class="sxs-lookup"><span data-stu-id="962d7-173">Yes, as long as traffic is allowed.</span></span>

<span data-ttu-id="962d7-174">**OTÁZKY. Může mít jeden fond back-end obsluhující mnoho aplikací na jiné porty?**</span><span class="sxs-lookup"><span data-stu-id="962d7-174">**Q. Can I have one backend pool serving many applications on different ports?**</span></span>

<span data-ttu-id="962d7-175">Architektura malých služby je podporována.</span><span class="sxs-lookup"><span data-stu-id="962d7-175">Micro service architecture is supported.</span></span> <span data-ttu-id="962d7-176">Potřebovali byste více tooprobe nastavení protokolu http na jiné porty.</span><span class="sxs-lookup"><span data-stu-id="962d7-176">You would need multiple http settings configured tooprobe on different ports.</span></span>

<span data-ttu-id="962d7-177">**OTÁZKY. Podporují vlastní testy paměti na data odpovědi zástupné znaky/regex?**</span><span class="sxs-lookup"><span data-stu-id="962d7-177">**Q. Do custom probes support wildcards/regex on response data?**</span></span>

<span data-ttu-id="962d7-178">Vlastní testy paměti nepodporují zástupných znaků nebo regex na data odpovědi.</span><span class="sxs-lookup"><span data-stu-id="962d7-178">Custom probes do not support wildcard or regex on response data.</span></span> 

<span data-ttu-id="962d7-179">**OTÁZKY. Jak se zpracovávají pravidla?**</span><span class="sxs-lookup"><span data-stu-id="962d7-179">**Q. How are rules processed?**</span></span>

<span data-ttu-id="962d7-180">Pravidla se zpracovávají v pořadí hello, které jsou nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="962d7-180">Rules are processed in hello order they are configured.</span></span> <span data-ttu-id="962d7-181">Doporučuje se, zda pravidla více lokalit jsou nakonfigurovány před základních pravidel tooreduce hello šance, že se provoz směrovat nevhodných back-end toohello jako základní pravidlo hello odpovídá provoz založený na pravidlu více lokalit předchozí toohello port vyhodnocovaný.</span><span class="sxs-lookup"><span data-stu-id="962d7-181">It is recommended that multi-site rules are configured before basic rules tooreduce hello chance that traffic is routed toohello inappropriate backend as hello basic rule would match traffic based on port prior toohello multi-site rule being evaluated.</span></span>

<span data-ttu-id="962d7-182">**OTÁZKY. Jak se zpracovávají pravidla?**</span><span class="sxs-lookup"><span data-stu-id="962d7-182">**Q. How are rules processed?**</span></span>

<span data-ttu-id="962d7-183">Pravidla se zpracovávají v pořadí hello, které byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="962d7-183">Rules are processed in hello order they are created.</span></span> <span data-ttu-id="962d7-184">Doporučuje se, že před základních pravidel jsou nakonfigurovaná pravidla více lokalit.</span><span class="sxs-lookup"><span data-stu-id="962d7-184">It is recommended that multi-site rules are configured before basic rules.</span></span> <span data-ttu-id="962d7-185">Nakonfigurováním nejprve naslouchací procesy více lokalit, tato konfigurace snižuje riziko hello, provoz se směruje toohello nevhodných back-end.</span><span class="sxs-lookup"><span data-stu-id="962d7-185">By configuring multi-site listeners first, this configuration reduces hello chance that traffic is routed toohello inappropriate backend.</span></span> <span data-ttu-id="962d7-186">Tento problém směrování může docházet k základní pravidlo hello odpovídá provoz založený na pravidlu více lokalit předchozí toohello port vyhodnocovaný.</span><span class="sxs-lookup"><span data-stu-id="962d7-186">This routing issue can occur as hello basic rule would match traffic based on port prior toohello multi-site rule being evaluated.</span></span>

<span data-ttu-id="962d7-187">**OTÁZKY. Co označují hello pole hostitel pro vlastní testy paměti?**</span><span class="sxs-lookup"><span data-stu-id="962d7-187">**Q. What does hello Host field for custom probes signify?**</span></span>

<span data-ttu-id="962d7-188">Pole hostitel Určuje hello název toosend hello testu do.</span><span class="sxs-lookup"><span data-stu-id="962d7-188">Host field specifies hello name toosend hello probe to.</span></span> <span data-ttu-id="962d7-189">Platí jenom v případě více lokalit je nakonfigurovaná na aplikační bránu, v opačném případě použijte "127.0.0.1".</span><span class="sxs-lookup"><span data-stu-id="962d7-189">Applicable only when multi-site is configured on Application Gateway, otherwise use '127.0.0.1'.</span></span> <span data-ttu-id="962d7-190">Tato hodnota se liší od názvu hostitele virtuálních počítačů a je ve formátu \<protokol\>://\<hostitele\>:\<port\>\<cestu\>.</span><span class="sxs-lookup"><span data-stu-id="962d7-190">This value is different from VM host name and is in format \<protocol\>://\<host\>:\<port\>\<path\>.</span></span>

<span data-ttu-id="962d7-191">**OTÁZKY. Seznam povolených adres aplikační brány přístup tooa se dá několika zdrojové IP adresy?**</span><span class="sxs-lookup"><span data-stu-id="962d7-191">**Q. Can I whitelist Application Gateway access tooa few source IPs?**</span></span>

<span data-ttu-id="962d7-192">Tento scénář lze provést pomocí skupin Nsg na podsítě brány aplikace.</span><span class="sxs-lookup"><span data-stu-id="962d7-192">This scenario can be done using NSGs on Application Gateway subnet.</span></span> <span data-ttu-id="962d7-193">následující omezení Hello měly být umístěny v podsíti hello v hello uvedené pořadí podle priority:</span><span class="sxs-lookup"><span data-stu-id="962d7-193">hello following restrictions should be put on hello subnet in hello listed order of priority:</span></span>

* <span data-ttu-id="962d7-194">Povolí příchozí provoz ze zdrojových rozsah IP/IP.</span><span class="sxs-lookup"><span data-stu-id="962d7-194">Allow incoming traffic from source IP/IP range.</span></span>

* <span data-ttu-id="962d7-195">Povolit příchozí požadavky od všech zdrojů tooports 65503 65534 pro [komunikace stavu back-end](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="962d7-195">Allow incoming requests from all sources tooports 65503-65534 for [backend health communication](application-gateway-diagnostics.md).</span></span>

* <span data-ttu-id="962d7-196">Povolit příchozí nástroj pro vyrovnávání zatížení Azure sondy (značka AzureLoadBalancer) a příchozí přenosy virtuální sítě (virtuální síť značky) na hello [NSG](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="962d7-196">Allow incoming Azure Load Balancer probes (AzureLoadBalancer tag) and inbound virtual network traffic (VirtualNetwork tag) on hello [NSG](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="962d7-197">Blokovat všechna ostatní příchozí přenosy s odepření všechna pravidla.</span><span class="sxs-lookup"><span data-stu-id="962d7-197">Block all other incoming traffic with a Deny all rule.</span></span>

* <span data-ttu-id="962d7-198">Povolit odchozí přenosy toohello internet pro všechna místa určení.</span><span class="sxs-lookup"><span data-stu-id="962d7-198">Allow outbound traffic toohello internet for all destinations.</span></span>

## <a name="performance"></a><span data-ttu-id="962d7-199">Výkon</span><span class="sxs-lookup"><span data-stu-id="962d7-199">Performance</span></span>

<span data-ttu-id="962d7-200">**OTÁZKY. Jak Application Gateway podporuje vysokou dostupnost a škálovatelnost?**</span><span class="sxs-lookup"><span data-stu-id="962d7-200">**Q. How does Application Gateway support high availability and scalability?**</span></span>

<span data-ttu-id="962d7-201">Aplikační brána podporuje scénáře s vysokou dostupností, až budete mít dva nebo více instancí nasazení.</span><span class="sxs-lookup"><span data-stu-id="962d7-201">Application Gateway supports high availability scenarios when you have two or more instances deployed.</span></span> <span data-ttu-id="962d7-202">Azure distribuuje tyto instance v aktualizaci a odolnost tooensure domény, který v hello nedošlo k selhání všech instancí současně.</span><span class="sxs-lookup"><span data-stu-id="962d7-202">Azure distributes these instances across update and fault domains tooensure that all instances do not fail at hello same time.</span></span> <span data-ttu-id="962d7-203">Aplikační brána podporuje škálovatelnost přidáním více instancí hello stejné zatížení hello tooshare brány.</span><span class="sxs-lookup"><span data-stu-id="962d7-203">Application Gateway supports scalability by adding multiple instances of hello same gateway tooshare hello load.</span></span>

<span data-ttu-id="962d7-204">**OTÁZKY. Jak dosáhnout scénář zotavení po Havárii napříč datovými centry s Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-204">**Q. How do I achieve DR scenario across data centers with Application Gateway?**</span></span>

<span data-ttu-id="962d7-205">Zákazníci mohou používat Traffic Manager toodistribute provoz napříč více bran aplikace v různých datových centrech.</span><span class="sxs-lookup"><span data-stu-id="962d7-205">Customers can use Traffic Manager toodistribute traffic across multiple Application Gateways in different datacenters.</span></span>

<span data-ttu-id="962d7-206">**OTÁZKY. Automatické škálování je podporovaný?**</span><span class="sxs-lookup"><span data-stu-id="962d7-206">**Q. Is auto scaling supported?**</span></span>

<span data-ttu-id="962d7-207">Ne, ale Application Gateway poskytuje metriky propustnosti, kterou lze použít tooalert můžete po dosažení prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="962d7-207">No, but Application Gateway has a throughput metric that can be used tooalert you when a threshold is reached.</span></span> <span data-ttu-id="962d7-208">Přidávání instancí nebo změna velikosti ručně nerestartuje hello brány a nemá negativní vliv na stávající přenosy dat.</span><span class="sxs-lookup"><span data-stu-id="962d7-208">Manually adding instances or changing size does not restart hello gateway and does not impact existing traffic.</span></span>

<span data-ttu-id="962d7-209">**OTÁZKY. Podporuje ruční škálování nahoru/dolů příčina výpadku?**</span><span class="sxs-lookup"><span data-stu-id="962d7-209">**Q. Does manual scale up/down cause downtime?**</span></span>

<span data-ttu-id="962d7-210">Neexistuje žádné výpadky, instancí jsou rozmístěny v upgradu domén a domén selhání.</span><span class="sxs-lookup"><span data-stu-id="962d7-210">There is no downtime, instances are distributed across upgrade domains and fault domains.</span></span>

<span data-ttu-id="962d7-211">**OTÁZKY. Můžete změnit velikost instance ze střední toolarge bez přerušení?**</span><span class="sxs-lookup"><span data-stu-id="962d7-211">**Q. Can I change instance size from medium toolarge without disruption?**</span></span>

<span data-ttu-id="962d7-212">Ano, Azure rozděluje instance na aktualizace a selhání tooensure domény, který v hello nedošlo k selhání všech instancí současně.</span><span class="sxs-lookup"><span data-stu-id="962d7-212">Yes, Azure distributes instances across update and fault domains tooensure that all instances do not fail at hello same time.</span></span> <span data-ttu-id="962d7-213">Aplikace podporuje brány škálování přidáním více instancí hello stejné zatížení hello tooshare brány.</span><span class="sxs-lookup"><span data-stu-id="962d7-213">Application Gateway supports scaling by adding multiple instances of hello same gateway tooshare hello load.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="962d7-214">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="962d7-214">SSL Configuration</span></span>

<span data-ttu-id="962d7-215">**OTÁZKY. Jaké certifikáty jsou podporovány ve Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-215">**Q. What certificates are supported on Application Gateway?**</span></span>

<span data-ttu-id="962d7-216">Vlastní podepsané certifikáty, certifikátů certifikační Autority a certifikátů zástupnému znaku. jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="962d7-216">Self signed certs, CA certs, and wild-card certs are supported.</span></span> <span data-ttu-id="962d7-217">Rozšířené ověření certifikátů nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="962d7-217">EV certs are not supported.</span></span>

<span data-ttu-id="962d7-218">**OTÁZKY. Jaké jsou hello aktuální šifrovací sada podporovaná Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-218">**Q. What are hello current cipher suites supported by Application Gateway?**</span></span>

<span data-ttu-id="962d7-219">Hello následují hello aktuální šifrovací sada podporovaná aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="962d7-219">hello following are hello current cipher suites supported by application gateway.</span></span> <span data-ttu-id="962d7-220">Navštivte: [konfigurace SSL verze zásad a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn jak toocustomize možností protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="962d7-220">Visit: [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn how toocustomize SSL options.</span></span>

- <span data-ttu-id="962d7-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="962d7-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="962d7-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="962d7-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="962d7-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="962d7-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="962d7-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="962d7-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="962d7-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="962d7-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="962d7-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="962d7-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="962d7-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="962d7-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="962d7-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="962d7-233">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-233">TLS_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="962d7-234">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-234">TLS_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="962d7-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="962d7-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="962d7-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="962d7-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="962d7-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="962d7-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="962d7-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="962d7-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="962d7-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="962d7-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="962d7-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="962d7-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="962d7-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span>
- <span data-ttu-id="962d7-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="962d7-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span>

<span data-ttu-id="962d7-247">**OTÁZKY. Application Gateway také podporuje znova šifrovat provoz toohello back-end?**</span><span class="sxs-lookup"><span data-stu-id="962d7-247">**Q. Does Application Gateway also support re-encryption of traffic toohello backend?**</span></span>

<span data-ttu-id="962d7-248">Ano, přesměrování zpracování SSL Aplikační brána podporuje a end tooend SSL, který znovu zašifruje hello provoz toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="962d7-248">Yes, Application Gateway supports SSL offload, and end tooend SSL, which re-encrypts hello traffic toohello backend.</span></span>

<span data-ttu-id="962d7-249">**OTÁZKY. Můžete nakonfigurovat verzí protokolu SSL toocontrol zásady protokolu SSL?**</span><span class="sxs-lookup"><span data-stu-id="962d7-249">**Q. Can I configure SSL policy toocontrol SSL Protocol versions?**</span></span>

<span data-ttu-id="962d7-250">Ano, můžete nakonfigurovat Application Gateway toodeny TLS1.0, TLS1.1 a TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="962d7-250">Yes, you can configure Application Gateway toodeny TLS1.0, TLS1.1, and TLS1.2.</span></span> <span data-ttu-id="962d7-251">Protokol SSL 2.0 a 3.0 jsou už ve výchozím nastavení zakázána a se nedají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="962d7-251">SSL 2.0 and 3.0 are already disabled by default and are not configurable.</span></span>

<span data-ttu-id="962d7-252">**OTÁZKY. Můžete nakonfigurovat šifrovací sady a pořadí zásad?**</span><span class="sxs-lookup"><span data-stu-id="962d7-252">**Q. Can I configure cipher suites and policy order?**</span></span>

<span data-ttu-id="962d7-253">Ano, [konfigurace šifrovací sady](application-gateway-ssl-policy-overview.md) je podporována.</span><span class="sxs-lookup"><span data-stu-id="962d7-253">Yes, [configuration of cipher suites](application-gateway-ssl-policy-overview.md) is supported.</span></span> <span data-ttu-id="962d7-254">Při definování vlastní zásady, musí být povolena alespoň jeden z následujících šifrovací sady hello.</span><span class="sxs-lookup"><span data-stu-id="962d7-254">When defining a custom policy, at least one of hello following cipher suites must be enabled.</span></span> <span data-ttu-id="962d7-255">Aplikační brána používá SHA256 toofor back-end správy.</span><span class="sxs-lookup"><span data-stu-id="962d7-255">Application gateway uses SHA256 toofor backend management.</span></span>

* <span data-ttu-id="962d7-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span> 
* <span data-ttu-id="962d7-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
* <span data-ttu-id="962d7-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="962d7-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="962d7-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
* <span data-ttu-id="962d7-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="962d7-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>

<span data-ttu-id="962d7-262">**OTÁZKY. Jak velký počet certifikátů SSL jsou podporovány?**</span><span class="sxs-lookup"><span data-stu-id="962d7-262">**Q. How many SSL certificates are supported?**</span></span>

<span data-ttu-id="962d7-263">Až too20 SSL certifikáty jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="962d7-263">Up too20 SSL certificates are supported.</span></span>

<span data-ttu-id="962d7-264">**OTÁZKY. Kolik ověřovací certifikáty pro back-end znova šifrovat jsou podporovány?**</span><span class="sxs-lookup"><span data-stu-id="962d7-264">**Q. How many authentication certificates for backend re-encryption are supported?**</span></span>

<span data-ttu-id="962d7-265">Výchozí hodnota je 5 je podporováno až too10 certifikáty pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="962d7-265">Up too10 authentication certificates are supported with a default of 5.</span></span>

<span data-ttu-id="962d7-266">**OTÁZKY. Umožňuje Application Gateway integraci s Azure Key Vault nativně?**</span><span class="sxs-lookup"><span data-stu-id="962d7-266">**Q. Does Application Gateway integrate with Azure Key Vault natively?**</span></span>

<span data-ttu-id="962d7-267">Ne, není integrované s Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="962d7-267">No, it is not integrated with Azure Key Vault.</span></span>

## <a name="web-application-firewall-waf-configuration"></a><span data-ttu-id="962d7-268">Konfigurace brány Firewall (firewall webových aplikací) webové aplikace</span><span class="sxs-lookup"><span data-stu-id="962d7-268">Web Application Firewall (WAF) Configuration</span></span>

<span data-ttu-id="962d7-269">**OTÁZKY. Nabízí hello firewall webových aplikací SKU všechny hello funkcí dostupných v hello standardní SKU?**</span><span class="sxs-lookup"><span data-stu-id="962d7-269">**Q. Does hello WAF SKU offer all hello features available with hello Standard SKU?**</span></span>

<span data-ttu-id="962d7-270">Ano, firewall webových aplikací podporuje všechny funkce hello v hello standardní SKU.</span><span class="sxs-lookup"><span data-stu-id="962d7-270">Yes, WAF supports all hello features in hello Standard SKU.</span></span>

<span data-ttu-id="962d7-271">**OTÁZKY. Co je aplikační brána podporuje verze řádku hello?**</span><span class="sxs-lookup"><span data-stu-id="962d7-271">**Q. What is hello CRS version Application Gateway supports?**</span></span>

<span data-ttu-id="962d7-272">Aplikační brána podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a řádku [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span><span class="sxs-lookup"><span data-stu-id="962d7-272">Application Gateway supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span></span>

<span data-ttu-id="962d7-273">**OTÁZKY. Jak se monitorování firewall webových aplikací?**</span><span class="sxs-lookup"><span data-stu-id="962d7-273">**Q. How do I monitor WAF?**</span></span>

<span data-ttu-id="962d7-274">Firewall webových aplikací je monitorována prostřednictvím protokolování diagnostiky, další informace o protokolování diagnostiky naleznete na [protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="962d7-274">WAF is monitored through diagnostic logging, more information on diagnostic logging can be found at [Diagnostics Logging and Metrics for Application Gateway](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="962d7-275">**OTÁZKY. Detekce režimu blokování provozu?**</span><span class="sxs-lookup"><span data-stu-id="962d7-275">**Q. Does detection mode block traffic?**</span></span>

<span data-ttu-id="962d7-276">Ne, detekce režimu jenom protokoly přenosy, která spustí pravidlo firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="962d7-276">No, detection mode only logs traffic, which triggered a WAF rule.</span></span>

<span data-ttu-id="962d7-277">**OTÁZKY. Jak lze přizpůsobit pravidla firewall webových aplikací?**</span><span class="sxs-lookup"><span data-stu-id="962d7-277">**Q. How do I customize WAF rules?**</span></span>

<span data-ttu-id="962d7-278">Ano, pravidla firewall webových aplikací jsou přizpůsobitelné, další informace o tom, jak se toocustomize je navštívit [skupiny pravidel přizpůsobit firewall webových aplikací a pravidla](application-gateway-customize-waf-rules-portal.md)</span><span class="sxs-lookup"><span data-stu-id="962d7-278">Yes, WAF rules are customizable, for more information on how toocustomize them visit [Customize WAF rule groups and rules](application-gateway-customize-waf-rules-portal.md)</span></span>

<span data-ttu-id="962d7-279">**OTÁZKY. Jaká pravidla jsou nyní k dispozici?**</span><span class="sxs-lookup"><span data-stu-id="962d7-279">**Q. What rules are currently available?**</span></span>

<span data-ttu-id="962d7-280">Firewall webových aplikací v současné době podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), které poskytují základní zabezpečení proti většinu hello ohrožení zabezpečení prvních 10 identifikovaný hello otevřete webovou aplikaci zabezpečení projektu (OWASP) je zde uveden [OWASP top 10 ohrožení zabezpečení](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span><span class="sxs-lookup"><span data-stu-id="962d7-280">WAF currently supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), which provide baseline security against most of hello top 10 vulnerabilities identified by hello Open Web Application Security Project (OWASP) found here [OWASP top 10 Vulnerabilities](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span></span>

* <span data-ttu-id="962d7-281">Ochrana před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="962d7-281">SQL injection protection</span></span>

* <span data-ttu-id="962d7-282">Ochrana před skriptováním mezi weby.</span><span class="sxs-lookup"><span data-stu-id="962d7-282">Cross site scripting protection</span></span>

* <span data-ttu-id="962d7-283">Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.</span><span class="sxs-lookup"><span data-stu-id="962d7-283">Common Web Attacks Protection such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attack</span></span>

* <span data-ttu-id="962d7-284">Ochrana před narušením protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="962d7-284">Protection against HTTP protocol violations</span></span>

* <span data-ttu-id="962d7-285">Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.</span><span class="sxs-lookup"><span data-stu-id="962d7-285">Protection against HTTP protocol anomalies such as missing host user-agent and accept headers</span></span>

* <span data-ttu-id="962d7-286">Ochrana před roboty, prohledávacími moduly a skenery.</span><span class="sxs-lookup"><span data-stu-id="962d7-286">Prevention against bots, crawlers, and scanners</span></span>

* <span data-ttu-id="962d7-287">Detekce časté nesprávné konfigurace aplikace (tedy Apache, IIS, atd.)</span><span class="sxs-lookup"><span data-stu-id="962d7-287">Detection of common application misconfigurations (that is, Apache, IIS, etc.)</span></span>

<span data-ttu-id="962d7-288">**OTÁZKY. Firewall webových aplikací taky podporuje DDoS prevence?**</span><span class="sxs-lookup"><span data-stu-id="962d7-288">**Q. Does WAF also support DDoS prevention?**</span></span>

<span data-ttu-id="962d7-289">Ne, firewall webových aplikací neposkytuje DDoS prevence.</span><span class="sxs-lookup"><span data-stu-id="962d7-289">No, WAF does not provide DDoS prevention.</span></span>

## <a name="diagnostics-and-logging"></a><span data-ttu-id="962d7-290">Protokolování a diagnostiky</span><span class="sxs-lookup"><span data-stu-id="962d7-290">Diagnostics and Logging</span></span>

<span data-ttu-id="962d7-291">**OTÁZKY. Jaké typy protokolů jsou k dispozici s Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-291">**Q. What types of logs are available with Application Gateway?**</span></span>

<span data-ttu-id="962d7-292">Nejsou k dispozici pro službu Application Gateway tři protokoly.</span><span class="sxs-lookup"><span data-stu-id="962d7-292">There are three logs available for Application Gateway.</span></span> <span data-ttu-id="962d7-293">Další informace o těchto protokolů a jiné diagnostické funkce, najdete v článku [back-end stavu, protokolů diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="962d7-293">For more information on these logs and other diagnostic capabilities, visit [Backend health, diagnostics logs, and metrics for Application Gateway](application-gateway-diagnostics.md).</span></span>

- <span data-ttu-id="962d7-294">**ApplicationGatewayAccessLog** – protokol přístupu hello obsahuje každý odeslaný požadavek toohello Application Gateway front-endu.</span><span class="sxs-lookup"><span data-stu-id="962d7-294">**ApplicationGatewayAccessLog** - hello access log contains each request submitted toohello Application Gateway frontend.</span></span> <span data-ttu-id="962d7-295">Hello data zahrnují hello volajícího IP adresy, požadovaná, adresa URL odpovědi latence, návratový kód, bajtů a odhlášení. Protokol přístupu se shromažďují každých 300 sekund.</span><span class="sxs-lookup"><span data-stu-id="962d7-295">hello data includes hello caller's IP, URL requested, response latency, return code, bytes in and out. Access log is collected every 300 seconds.</span></span> <span data-ttu-id="962d7-296">Tento protokol obsahuje jeden záznam za instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="962d7-296">This log contains one record per instance of Application Gateway.</span></span>
- <span data-ttu-id="962d7-297">**ApplicationGatewayPerformanceLog** -hello výkonu protokolu zaznamená informace o výkonu na základě za instance včetně celkový požadavek zpracovat, propustnost v bajtech, celkový počet požadavků zpracovaných, počet chybných požadavků, v pořádku a není v pořádku počet instancí back-end.</span><span class="sxs-lookup"><span data-stu-id="962d7-297">**ApplicationGatewayPerformanceLog** - hello performance log captures performance information on per instance basis including total request served, throughput in bytes, total requests served, failed request count, healthy and unhealthy back-end instance count.</span></span>
- <span data-ttu-id="962d7-298">**ApplicationGatewayFirewallLog** -hello brány firewall protokol obsahuje požadavky, které se protokolují prostřednictvím zjišťování nebo zabránění režim služby application gateway, která je konfigurovaná pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="962d7-298">**ApplicationGatewayFirewallLog** - hello firewall log contains requests that are logged through either detection or prevention mode of an application gateway that is configured with web application firewall.</span></span>

<span data-ttu-id="962d7-299">**OTÁZKY. Jak poznám, pokud jsou moje členy fondu back-end v pořádku?**</span><span class="sxs-lookup"><span data-stu-id="962d7-299">**Q. How do I know if my backend pool members are healthy?**</span></span>

<span data-ttu-id="962d7-300">Můžete použít rutiny prostředí PowerShell hello `Get-AzureRmApplicationGatewayBackendHealth` nebo ověřit stav prostřednictvím portálu hello tak, že navštívíte [diagnostiku brány aplikace](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="962d7-300">You can use hello PowerShell cmdlet `Get-AzureRmApplicationGatewayBackendHealth` or verify health through hello portal by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="962d7-301">**OTÁZKY. Co je hello zásady uchovávání informací na hello diagnostické protokoly?**</span><span class="sxs-lookup"><span data-stu-id="962d7-301">**Q. What is hello retention policy on hello diagnostics logs?**</span></span>

<span data-ttu-id="962d7-302">Diagnostické protokoly účet úložiště zákazníci toohello toku a zákazníků můžete nastavit zásady uchovávání informací hello podle jejich předvoleb.</span><span class="sxs-lookup"><span data-stu-id="962d7-302">Diagnostic logs flow toohello customers storage account and customers can set hello retention policy based on their preference.</span></span> <span data-ttu-id="962d7-303">Diagnostické protokoly můžete odeslat také tooan centra událostí nebo analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="962d7-303">Diagnostic logs can also be sent tooan Event Hub or Log Analytics.</span></span> <span data-ttu-id="962d7-304">Navštivte [Application Diagnostics brány](application-gateway-diagnostics.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="962d7-304">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) for more details.</span></span>

<span data-ttu-id="962d7-305">**OTÁZKY. Získání protokolů auditu pro službu Application Gateway**</span><span class="sxs-lookup"><span data-stu-id="962d7-305">**Q. How do I get audit logs for Application Gateway?**</span></span>

<span data-ttu-id="962d7-306">Protokoly auditu jsou k dispozici pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="962d7-306">Audit logs are available for Application Gateway.</span></span> <span data-ttu-id="962d7-307">Hello portálu, klikněte na tlačítko **protokol aktivit** v okně nabídky hello protokolu auditu hello tooaccess Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="962d7-307">In hello portal, click **Activity Log** in hello menu blade of an Application Gateway tooaccess hello audit log.</span></span> 

<span data-ttu-id="962d7-308">**OTÁZKY. Můžete nastavit výstrahy s Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="962d7-308">**Q. Can I set alerts with Application Gateway?**</span></span>

<span data-ttu-id="962d7-309">Ano, aplikační brána podporuje výstrahy, se konfigurují vypnout metriky.</span><span class="sxs-lookup"><span data-stu-id="962d7-309">Yes, Application Gateway does support alerts, alerts are configured off metrics.</span></span>  <span data-ttu-id="962d7-310">Application Gateway aktuálně poskytuje metriky propustnosti"", což může být nakonfigurované tooalert.</span><span class="sxs-lookup"><span data-stu-id="962d7-310">Application Gateway currently has a metric of "throughput", which can be configured tooalert.</span></span> <span data-ttu-id="962d7-311">toolearn více o výstrahách, navštivte [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="962d7-311">toolearn more about alerts, visit [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="962d7-312">**OTÁZKY. Back-end stavu vrátí Neznámý stav, co by mohlo být příčinou tento stav?**</span><span class="sxs-lookup"><span data-stu-id="962d7-312">**Q. Backend health returns unknown status, what could be causing this status?**</span></span>

<span data-ttu-id="962d7-313">Nejčastější příčinou Hello je skupina NSG nebo vlastní DNS je blokován přístup toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="962d7-313">hello most common reason is access toohello backend is being blocked by an NSG or custom DNS.</span></span> <span data-ttu-id="962d7-314">Navštivte [back-end stavu, protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="962d7-314">Visit [Backend health, diagnostics logging, and metrics for Application Gateway](application-gateway-diagnostics.md) toolearn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="962d7-315">Další kroky</span><span class="sxs-lookup"><span data-stu-id="962d7-315">Next Steps</span></span>

<span data-ttu-id="962d7-316">toolearn najdete informace o bráně aplikace [Úvod tooApplication brány](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="962d7-316">toolearn more about Application Gateway visit [Introduction tooApplication Gateway](application-gateway-introduction.md).</span></span>
