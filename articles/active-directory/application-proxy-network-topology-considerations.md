---
title: "Aspekty topologie sítě, při použití aplikace Proxy Azure Active Directory | Microsoft Docs"
description: "Popisuje aspekty topologie sítě, při použití Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 11244e0044eef8441e3a37ab8aeff0da30dacdb8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a><span data-ttu-id="7dd14-103">Aspekty topologie sítě při použití aplikace Proxy Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7dd14-103">Network topology considerations when using Azure Active Directory Application Proxy</span></span>

<span data-ttu-id="7dd14-104">Tento článek vysvětluje aspekty topologie sítě, při použití aplikace Proxy Azure Active Directory (Azure AD) pro publikování a vzdálený přístup k vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-104">This article explains network topology considerations when using Azure Active Directory (Azure AD) Application Proxy for publishing and accessing your applications remotely.</span></span>

## <a name="traffic-flow"></a><span data-ttu-id="7dd14-105">Tok přenosů dat</span><span class="sxs-lookup"><span data-stu-id="7dd14-105">Traffic flow</span></span>

<span data-ttu-id="7dd14-106">Při publikování aplikace prostřednictvím proxy aplikace služby Azure AD, prochází provoz z uživatelů k aplikacím tři připojení:</span><span class="sxs-lookup"><span data-stu-id="7dd14-106">When an application is published through Azure AD Application Proxy, traffic from the users to the applications flows through three connections:</span></span>

1. <span data-ttu-id="7dd14-107">Připojení uživatele k Azure AD Application Proxy veřejný koncový bod služby v Azure</span><span class="sxs-lookup"><span data-stu-id="7dd14-107">The user connects to the Azure AD Application Proxy service public endpoint on Azure</span></span>
2. <span data-ttu-id="7dd14-108">Služba Proxy aplikace připojí ke konektoru Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-108">The Application Proxy service connects to the Application Proxy connector</span></span>
3. <span data-ttu-id="7dd14-109">Konektor Proxy aplikace se připojuje k cílové aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-109">The Application Proxy connector connects to the target application</span></span>

![Diagram zobrazuje tok přenosů dat od uživatele do cílové aplikace](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a><span data-ttu-id="7dd14-111">Umístění klienta a Proxy aplikace služby</span><span class="sxs-lookup"><span data-stu-id="7dd14-111">Tenant location and Application Proxy service</span></span>

<span data-ttu-id="7dd14-112">Při registraci pro klienta služby Azure AD oblasti klienta služby je určen podle země, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="7dd14-112">When you sign up for an Azure AD tenant, the region of your tenant is determined by the country you specify.</span></span> <span data-ttu-id="7dd14-113">Když povolíte Proxy aplikace, instance služby Proxy aplikace vašeho klienta jsou zvolené nebo vytvořit ve stejné oblasti jako váš klient Azure AD nebo nejbližší oblast na ni.</span><span class="sxs-lookup"><span data-stu-id="7dd14-113">When you enable Application Proxy, the Application Proxy service instances for your tenant are chosen or created in the same region as your Azure AD tenant, or the closest region to it.</span></span>

<span data-ttu-id="7dd14-114">Pokud je klient Azure AD oblast Evropské unie (EU), všechny konektory Proxy aplikací pomocí instance služby v datových centrech Azure v EU.</span><span class="sxs-lookup"><span data-stu-id="7dd14-114">For example, if your Azure AD tenant’s region is the European Union (EU), all your Application Proxy connectors use service instances in Azure datacenters in the EU.</span></span> <span data-ttu-id="7dd14-115">Při publikování aplikace přístup vašich uživatelů jejich provoz prochází instance služby Proxy aplikace v tomto umístění.</span><span class="sxs-lookup"><span data-stu-id="7dd14-115">When your users access published applications, their traffic goes through the Application Proxy service instances in this location.</span></span>

## <a name="considerations-for-reducing-latency"></a><span data-ttu-id="7dd14-116">Důležité informace týkající se sníží se latence</span><span class="sxs-lookup"><span data-stu-id="7dd14-116">Considerations for reducing latency</span></span>

<span data-ttu-id="7dd14-117">Všechna řešení pro proxy představovat latence na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="7dd14-117">All proxy solutions introduce latency into your network connection.</span></span> <span data-ttu-id="7dd14-118">Bez ohledu na to, které proxy server nebo řešení sítě VPN zvolíte jako řešení pro vzdálený přístup vždy obsahuje sadu servery umožňující připojení k uvnitř firemní sítě.</span><span class="sxs-lookup"><span data-stu-id="7dd14-118">No matter which proxy or VPN solution you choose as your remote access solution, it always includes a set of servers enabling the connection to inside your corporate network.</span></span>

<span data-ttu-id="7dd14-119">Organizace obvykle obsahovat koncové body serveru v jejich hraniční síti.</span><span class="sxs-lookup"><span data-stu-id="7dd14-119">Organizations typically include server endpoints in their perimeter network.</span></span> <span data-ttu-id="7dd14-120">S Azure AD Application Proxy ale přenosy dat v cloudu pomocí služby proxy server při konektory jsou umístěné ve vaší podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="7dd14-120">With Azure AD Application Proxy, however, traffic flows through the proxy service in the cloud while the connectors reside on your corporate network.</span></span> <span data-ttu-id="7dd14-121">Je potřeba žádné hraniční síť.</span><span class="sxs-lookup"><span data-stu-id="7dd14-121">No perimeter network is required.</span></span>

<span data-ttu-id="7dd14-122">Další oddíly obsahují další návrhy vám pomůže zmenšit latence ještě víc.</span><span class="sxs-lookup"><span data-stu-id="7dd14-122">The next sections contain additional suggestions to help you reduce latency even further.</span></span> 

### <a name="connector-placement"></a><span data-ttu-id="7dd14-123">Umístění konektoru</span><span class="sxs-lookup"><span data-stu-id="7dd14-123">Connector placement</span></span>

<span data-ttu-id="7dd14-124">Proxy aplikací zvolí instancí umístění pro vás, na základě vašeho umístění klienta.</span><span class="sxs-lookup"><span data-stu-id="7dd14-124">Application Proxy chooses the location of instances for you, based on your tenant location.</span></span> <span data-ttu-id="7dd14-125">Ale zobrazí se rozhodnout, kam se má nainstalovat konektor, budete k definování charakteristik latence síťových přenosů.</span><span class="sxs-lookup"><span data-stu-id="7dd14-125">However, you get to decide where to install the connector, giving you the power to define the latency characteristics of your network traffic.</span></span>

<span data-ttu-id="7dd14-126">Pokud instalujete službu Proxy aplikací, položte si následující otázky:</span><span class="sxs-lookup"><span data-stu-id="7dd14-126">When setting up the Application Proxy service, ask the following questions:</span></span>

* <span data-ttu-id="7dd14-127">Kde je umístěná aplikace?</span><span class="sxs-lookup"><span data-stu-id="7dd14-127">Where is the app located?</span></span>
* <span data-ttu-id="7dd14-128">Kde se nachází většina uživatelů, kteří přistupují k aplikaci?</span><span class="sxs-lookup"><span data-stu-id="7dd14-128">Where are most users who access the app located?</span></span>
* <span data-ttu-id="7dd14-129">Kde se nachází instance Proxy aplikace?</span><span class="sxs-lookup"><span data-stu-id="7dd14-129">Where is the Application Proxy instance located?</span></span>
* <span data-ttu-id="7dd14-130">Už máte vyhrazený síťové připojení k datových centrech Azure nastavení, jako je Azure ExpressRoute nebo podobné VPN?</span><span class="sxs-lookup"><span data-stu-id="7dd14-130">Do you already have a dedicated network connection to Azure datacenters set up, like Azure ExpressRoute or a similar VPN?</span></span>

<span data-ttu-id="7dd14-131">Konektor musí komunikovat s Azure a aplikací (kroky 2 a 3 v diagram toku dat), takže umístění ovlivňuje konektor latence tyto dvě připojení.</span><span class="sxs-lookup"><span data-stu-id="7dd14-131">The connector has to communicate with both Azure and your applications (steps 2 and 3 in the Traffic flow diagram), so the placement of the connector affects the latency of those two connections.</span></span> <span data-ttu-id="7dd14-132">Při vyhodnocení umístění konektoru, mějte na paměti následující body:</span><span class="sxs-lookup"><span data-stu-id="7dd14-132">When evaluating the placement of the connector, keep in mind the following points:</span></span>

* <span data-ttu-id="7dd14-133">Pokud chcete použít omezené delegování protokolu Kerberos (použitím KCD) pro jednotné přihlašování, konektor musí směrem pohledu do datového centra.</span><span class="sxs-lookup"><span data-stu-id="7dd14-133">If you want to use Kerberos constrained delegation (KCD) for single sign-on, then the connector needs a line of sight to a datacenter.</span></span> <span data-ttu-id="7dd14-134">Kromě toho server konektor musí být připojené k doméně.</span><span class="sxs-lookup"><span data-stu-id="7dd14-134">Additionally, the connector server needs to be domain joined.</span></span>  
* <span data-ttu-id="7dd14-135">Pokud máte pochybnosti, nainstalujte konektor blíže k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7dd14-135">When in doubt, install the connector closer to the application.</span></span>

### <a name="general-approach-to-minimize-latency"></a><span data-ttu-id="7dd14-136">Obecný přístup k zajištění minimální latence</span><span class="sxs-lookup"><span data-stu-id="7dd14-136">General approach to minimize latency</span></span>

<span data-ttu-id="7dd14-137">Můžete-li minimalizovat čekací doba provozu začátku do konce, optimalizace každé síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="7dd14-137">You can minimize the latency of the end-to-end traffic by optimizing each network connection.</span></span> <span data-ttu-id="7dd14-138">Každé připojení možné ji optimalizovat podle:</span><span class="sxs-lookup"><span data-stu-id="7dd14-138">Each connection can be optimized by:</span></span>

* <span data-ttu-id="7dd14-139">Snižuje vzdálenost mezi dva elementy end směrování.</span><span class="sxs-lookup"><span data-stu-id="7dd14-139">Reducing the distance between the two ends of the hop.</span></span>
* <span data-ttu-id="7dd14-140">Výběr správného síťového procházení.</span><span class="sxs-lookup"><span data-stu-id="7dd14-140">Choosing the right network to traverse.</span></span> <span data-ttu-id="7dd14-141">Například procházení privátní síti, nikoli veřejného Internetu může být rychlejší, z důvodu vyhrazené odkazy.</span><span class="sxs-lookup"><span data-stu-id="7dd14-141">For example, traversing a private network rather than the public Internet may be faster, due to dedicated links.</span></span>

<span data-ttu-id="7dd14-142">Pokud máte vyhrazené sítě VPN nebo ExpressRoute propojení mezi Azure a k podnikové síti, můžete použít.</span><span class="sxs-lookup"><span data-stu-id="7dd14-142">If you have a dedicated VPN or ExpressRoute link between Azure and your corporate network, you may want to use that.</span></span>

## <a name="focus-your-optimization-strategy"></a><span data-ttu-id="7dd14-143">Zaměřit strategie optimalizace</span><span class="sxs-lookup"><span data-stu-id="7dd14-143">Focus your optimization strategy</span></span>

<span data-ttu-id="7dd14-144">Existuje mnoho, které můžete provést k řízení připojení mezi uživatele a služba Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-144">There's little that you can do to control the connection between your users and the Application Proxy service.</span></span> <span data-ttu-id="7dd14-145">Uživatelé mohou přistupovat k vaší aplikace z domácí sítě, v kavárně nebo jiné země.</span><span class="sxs-lookup"><span data-stu-id="7dd14-145">Users may access your apps from a home network, a coffee shop, or a different country.</span></span> <span data-ttu-id="7dd14-146">Místo toho můžete optimalizovat připojení ze služby Proxy aplikace k Proxy aplikace konektory a aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-146">Instead, you can optimize the connections from the Application Proxy service to the Application Proxy connectors to the apps.</span></span> <span data-ttu-id="7dd14-147">Zvažte využití následujících charakteristik ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="7dd14-147">Consider incorporating the following patterns in your environment.</span></span>

### <a name="pattern-1-put-the-connector-close-to-the-application"></a><span data-ttu-id="7dd14-148">Vzor 1: Put konektor blízko aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-148">Pattern 1: Put the connector close to the application</span></span>

<span data-ttu-id="7dd14-149">Místní konektor blízko cílová aplikace v síti zákazníka.</span><span class="sxs-lookup"><span data-stu-id="7dd14-149">Place the connector close to the target application in the customer network.</span></span> <span data-ttu-id="7dd14-150">Tato konfigurace minimalizuje krok 3 v diagramu topografie, protože jsou zavřít konektoru a aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-150">This configuration minimizes step 3 in the topography diagram, because the connector and application are close.</span></span> 

<span data-ttu-id="7dd14-151">Pokud vaše konektor potřebuje směrem pohledu řadiče domény, je tento vzor výhodné.</span><span class="sxs-lookup"><span data-stu-id="7dd14-151">If your connector needs a line of sight to the domain controller, then this pattern is advantageous.</span></span> <span data-ttu-id="7dd14-152">Většina našich zákazníků použít tento vzor, protože je vhodný pro většinu scénářů.</span><span class="sxs-lookup"><span data-stu-id="7dd14-152">Most of our customers use this pattern, because it works well for most scenarios.</span></span> <span data-ttu-id="7dd14-153">Tento vzor může být spojen s vzor 2 k optimalizaci přenosů mezi službou a konektor.</span><span class="sxs-lookup"><span data-stu-id="7dd14-153">This pattern can also be combined with pattern 2 to optimize traffic between the service and the connector.</span></span>

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a><span data-ttu-id="7dd14-154">Vzor 2: Využít výhod ExpressRoute s veřejného partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="7dd14-154">Pattern 2: Take advantage of ExpressRoute with public peering</span></span>

<span data-ttu-id="7dd14-155">Pokud máte nastavit veřejného partnerského vztahu ExpressRoute, můžete pro přenosy mezi Proxy aplikací a konektor rychlejší připojení ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="7dd14-155">If you have ExpressRoute set up with public peering, you can use the faster ExpressRoute connection for traffic between Application Proxy and the connector.</span></span> <span data-ttu-id="7dd14-156">Konektor je stále v síti, blíží aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-156">The connector is still on your network, close to the app.</span></span>

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a><span data-ttu-id="7dd14-157">Vzor 3: Využít výhod ExpressRoute s soukromého partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="7dd14-157">Pattern 3: Take advantage of ExpressRoute with private peering</span></span>

<span data-ttu-id="7dd14-158">Pokud máte vyhrazené sítě VPN nebo ExpressRoute nastavit s soukromého partnerského vztahu mezi Azure a k podnikové síti, existuje další možnost.</span><span class="sxs-lookup"><span data-stu-id="7dd14-158">If you have a dedicated VPN or ExpressRoute set up with private peering between Azure and your corporate network, you have another option.</span></span> <span data-ttu-id="7dd14-159">V této konfiguraci se obvykle považuje virtuální sítě v Azure jako rozšíření podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="7dd14-159">In this configuration, the virtual network in Azure is typically considered as an extension of the corporate network.</span></span> <span data-ttu-id="7dd14-160">Proto můžete nainstalovat konektor v datovém centru Azure a stále splňují požadavky s nízkou latencí připojení konektoru aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-160">So you can install the connector in the Azure datacenter, and still satisfy the low latency requirements of the connector-to-app connection.</span></span>

<span data-ttu-id="7dd14-161">Latence nejsou ohrožená, protože provoz je předávaných přes vyhrazené připojení.</span><span class="sxs-lookup"><span data-stu-id="7dd14-161">Latency is not compromised because traffic is flowing over a dedicated connection.</span></span> <span data-ttu-id="7dd14-162">Zlepšení latence služby konektoru Proxy aplikace můžete také získat, protože je konektor nainstalovaný v datovém centru Azure blízko vaše umístění klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dd14-162">You also get improved Application Proxy service-to-connector latency because the connector is installed in an Azure datacenter close to your Azure AD tenant location.</span></span>

![Diagram zobrazující konektoru nainstalovány v datovém centru Azure](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a><span data-ttu-id="7dd14-164">Jiné postupy</span><span class="sxs-lookup"><span data-stu-id="7dd14-164">Other approaches</span></span>

<span data-ttu-id="7dd14-165">I když fokus tohoto článku je konektor umístění, můžete určit také umístění aplikace a získat lepší charakteristiky latence.</span><span class="sxs-lookup"><span data-stu-id="7dd14-165">Although the focus of this article is connector placement, you can also change the placement of the application to get better latency characteristics.</span></span>

<span data-ttu-id="7dd14-166">Organizace stále, jsou přesunutí jejich sítě do hostovaného prostředí.</span><span class="sxs-lookup"><span data-stu-id="7dd14-166">Increasingly, organizations are moving their networks into hosted environments.</span></span> <span data-ttu-id="7dd14-167">Díky tomu budou moci umístíte své aplikace v hostovaném prostředí, která je také součástí své podnikové síti a přesto být v rámci domény.</span><span class="sxs-lookup"><span data-stu-id="7dd14-167">This enables them to place their apps in a hosted environment that is also part of their corporate network, and still be within the domain.</span></span> <span data-ttu-id="7dd14-168">V takovém případě můžete použít vzory popsané v předchozí části do nového umístění aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-168">In this case, the patterns discussed in the preceding sections can be applied to the new application location.</span></span> <span data-ttu-id="7dd14-169">Pokud zvažujete tuto možnost, přečtěte si téma [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7dd14-169">If you're considering this option, see [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md).</span></span>

<span data-ttu-id="7dd14-170">Kromě toho zvažte uspořádání vaší konektorů pomocí [konektor skupiny](active-directory-application-proxy-connectors.md) cílové aplikace, které jsou v různých umístěních a sítě.</span><span class="sxs-lookup"><span data-stu-id="7dd14-170">Additionally, consider organizing your connectors using [connector groups](active-directory-application-proxy-connectors.md) to target apps that are in different locations and networks.</span></span> 

## <a name="common-use-cases"></a><span data-ttu-id="7dd14-171">Běžné případy použití</span><span class="sxs-lookup"><span data-stu-id="7dd14-171">Common use cases</span></span>

<span data-ttu-id="7dd14-172">V této části jsme provede několik běžných scénářů.</span><span class="sxs-lookup"><span data-stu-id="7dd14-172">In this section, we walk through a few common scenarios.</span></span> <span data-ttu-id="7dd14-173">Předpokládáme, že klient Azure AD (a proto koncový bod služby proxy serveru) se nachází v USA (US).</span><span class="sxs-lookup"><span data-stu-id="7dd14-173">Assume that the Azure AD tenant (and therefore proxy service endpoint) is located in the United States (US).</span></span> <span data-ttu-id="7dd14-174">Jaké jsou požadavky popsané v těchto případech se rovněž vztahují na jiných oblastech po celém světě používat.</span><span class="sxs-lookup"><span data-stu-id="7dd14-174">The considerations discussed in these use cases also apply to other regions around the globe.</span></span>

<span data-ttu-id="7dd14-175">Pro tyto scénáře jsme volání každé připojení "směrování" a číslo je snazší diskusi:</span><span class="sxs-lookup"><span data-stu-id="7dd14-175">For these scenarios, we call each connection a "hop" and number them for easier discussion:</span></span>

- <span data-ttu-id="7dd14-176">**Směrování 1**: uživatele ke službě Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-176">**Hop 1**: User to the Application Proxy service</span></span>
- <span data-ttu-id="7dd14-177">**Směrování 2**: Proxy aplikace služby konektoru Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-177">**Hop 2**: Application Proxy service to the Application Proxy connector</span></span>
- <span data-ttu-id="7dd14-178">**Směrování 3**: konektor Proxy aplikace do cílové aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-178">**Hop 3**: Application Proxy connector to the target application</span></span> 

### <a name="use-case-1"></a><span data-ttu-id="7dd14-179">Případ použití 1</span><span class="sxs-lookup"><span data-stu-id="7dd14-179">Use case 1</span></span>

<span data-ttu-id="7dd14-180">**Scénář:** aplikace je v síti organizace v USA, s uživateli ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="7dd14-180">**Scenario:** The app is in an organization's network in the US, with users in the same region.</span></span> <span data-ttu-id="7dd14-181">Mezi datové centrum Azure a k podnikové síti neexistuje žádná ExpressRoute nebo VPN.</span><span class="sxs-lookup"><span data-stu-id="7dd14-181">No ExpressRoute or VPN exists between the Azure datacenter and the corporate network.</span></span>

<span data-ttu-id="7dd14-182">**Doporučení:** vzor postupujte podle kroků 1, popsané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="7dd14-182">**Recommendation:** Follow pattern 1, explained in the previous section.</span></span> <span data-ttu-id="7dd14-183">Pro zlepšení latence vezměte v úvahu pomocí služby ExpressRoute, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="7dd14-183">For improved latency, consider using ExpressRoute, if needed.</span></span>

<span data-ttu-id="7dd14-184">Toto je jednoduchá vzor.</span><span class="sxs-lookup"><span data-stu-id="7dd14-184">This is a simple pattern.</span></span> <span data-ttu-id="7dd14-185">Můžete optimalizovat směrování 3 tím, že konektor téměř aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-185">You optimize hop 3 by placing the connector near the app.</span></span> <span data-ttu-id="7dd14-186">Toto je také přirozené volbou, protože konektor je obvykle nainstalován s směrem pohledu do aplikace a do datového centra k provádění operací použitím KCD.</span><span class="sxs-lookup"><span data-stu-id="7dd14-186">This is also a natural choice, because the connector typically is installed with line of sight to the app and to the datacenter to perform KCD operations.</span></span>

![Diagram znázorňující, že uživatelé, proxy, konektor a aplikace jsou všechny v USA](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a><span data-ttu-id="7dd14-188">Případ použití 2</span><span class="sxs-lookup"><span data-stu-id="7dd14-188">Use case 2</span></span>

<span data-ttu-id="7dd14-189">**Scénář:** aplikace je v síti organizace v USA, s uživateli šíření globálně.</span><span class="sxs-lookup"><span data-stu-id="7dd14-189">**Scenario:** The app is in an organization's network in the US, with users spread out globally.</span></span> <span data-ttu-id="7dd14-190">Mezi datové centrum Azure a k podnikové síti neexistuje žádná ExpressRoute nebo VPN.</span><span class="sxs-lookup"><span data-stu-id="7dd14-190">No ExpressRoute or VPN exists between the Azure datacenter and the corporate network.</span></span>

<span data-ttu-id="7dd14-191">**Doporučení:** vzor postupujte podle kroků 1, popsané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="7dd14-191">**Recommendation:** Follow pattern 1, explained in the previous section.</span></span> 

<span data-ttu-id="7dd14-192">Znovu běžné vzor je za účelem optimalizace směrování 3, kam umístíte konektor téměř aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-192">Again, the common pattern is to optimize hop 3, where you place the connector near the app.</span></span> <span data-ttu-id="7dd14-193">Směrování 3 není obvykle levnější, pokud je všechny ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="7dd14-193">Hop 3 is not typically expensive, if it is all within the same region.</span></span> <span data-ttu-id="7dd14-194">Směrování 1 však může být dražší, v závislosti na tom, kde je uživatel, protože uživatelé po celém světě musí připojit k instanci Proxy aplikace v USA.</span><span class="sxs-lookup"><span data-stu-id="7dd14-194">However, hop 1 can be more expensive depending on where the user is, because users across the world must access the Application Proxy instance in the US.</span></span> <span data-ttu-id="7dd14-195">Je vhodné poznamenat, že řešení proxy serveru má podobnou charakteristikou týkající se uživatelů se šíření globálně.</span><span class="sxs-lookup"><span data-stu-id="7dd14-195">It's worth noting that any proxy solution has similar characteristics regarding users being spread out globally.</span></span>

![Diagram zobrazující, že uživatelé jsou rozloženy globálně, ale proxy, konektor a aplikace jsou v USA](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a><span data-ttu-id="7dd14-197">Případ použití 3</span><span class="sxs-lookup"><span data-stu-id="7dd14-197">Use case 3</span></span>

<span data-ttu-id="7dd14-198">**Scénář:** aplikace je v síti organizace v USA.</span><span class="sxs-lookup"><span data-stu-id="7dd14-198">**Scenario:** The app is in an organization's network in the US.</span></span> <span data-ttu-id="7dd14-199">Existuje ExpressRoute s veřejný partnerský vztah mezi Azure a k podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="7dd14-199">ExpressRoute with public peering exists between Azure and the corporate network.</span></span>

<span data-ttu-id="7dd14-200">**Doporučení:** postupujte podle vzorů 1 a 2, které jsou popsané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="7dd14-200">**Recommendation:** Follow patterns 1 and 2, explained in the previous section.</span></span>

<span data-ttu-id="7dd14-201">Nejprve umístíte konektor jak nejblíže k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7dd14-201">First, place the connector as close as possible to the app.</span></span> <span data-ttu-id="7dd14-202">Pak systém automaticky pomocí ExpressRoute pro směrování 2.</span><span class="sxs-lookup"><span data-stu-id="7dd14-202">Then, the system automatically uses ExpressRoute for hop 2.</span></span> 

<span data-ttu-id="7dd14-203">Pokud odkaz ExpressRoute používá veřejný partnerský vztah, provoz mezi proxy serveru a konektor toky přes tento odkaz.</span><span class="sxs-lookup"><span data-stu-id="7dd14-203">If the ExpressRoute link is using public peering, the traffic between the proxy and the connector flows over that link.</span></span> <span data-ttu-id="7dd14-204">Směrování 2 optimalizovala latence.</span><span class="sxs-lookup"><span data-stu-id="7dd14-204">Hop 2 has optimized latency.</span></span>

![Diagram zobrazující ExpressRoute mezi proxy a konektor](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a><span data-ttu-id="7dd14-206">Případ použití 4</span><span class="sxs-lookup"><span data-stu-id="7dd14-206">Use case 4</span></span>

<span data-ttu-id="7dd14-207">**Scénář:** aplikace je v síti organizace v USA.</span><span class="sxs-lookup"><span data-stu-id="7dd14-207">**Scenario:** The app is in an organization's network in the US.</span></span> <span data-ttu-id="7dd14-208">Existuje ExpressRoute s soukromého partnerského vztahu mezi Azure a k podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="7dd14-208">ExpressRoute with private peering exists between Azure and the corporate network.</span></span>

<span data-ttu-id="7dd14-209">**Doporučení:** postupujte podle vzoru 3, popsané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="7dd14-209">**Recommendation:** Follow pattern 3, explained in the previous section.</span></span>

<span data-ttu-id="7dd14-210">Místní konektor v datovém centru Azure, která je připojena k podnikové síti prostřednictvím soukromého partnerského vztahu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="7dd14-210">Place the connector in the Azure datacenter that is connected to the corporate network through ExpressRoute private peering.</span></span> 

<span data-ttu-id="7dd14-211">Konektor se může v datovém centru Azure.</span><span class="sxs-lookup"><span data-stu-id="7dd14-211">The connector can be placed in the Azure datacenter.</span></span> <span data-ttu-id="7dd14-212">Vzhledem k tomu, že konektor má stále směrem pohledu aplikace a datového centra prostřednictvím privátní sítě, zůstane optimalizované směrování 3.</span><span class="sxs-lookup"><span data-stu-id="7dd14-212">Since the connector still has a line of sight to the application and the datacenter through the private network, hop 3 remains optimized.</span></span> <span data-ttu-id="7dd14-213">Kromě toho směrování 2 je optimalizovaná Další.</span><span class="sxs-lookup"><span data-stu-id="7dd14-213">In addition, hop 2 is optimized further.</span></span>

![Diagram zobrazující konektor v datovém centru Azure a ExpressRoute mezi konektoru a aplikace](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a><span data-ttu-id="7dd14-215">Případ použití 5</span><span class="sxs-lookup"><span data-stu-id="7dd14-215">Use case 5</span></span>

<span data-ttu-id="7dd14-216">**Scénář:** aplikace je v síti organizace v EU s instance Proxy aplikace a většina uživatelů v USA.</span><span class="sxs-lookup"><span data-stu-id="7dd14-216">**Scenario:** The app is in an organization's network in the EU, with the Application Proxy instance and most users in the US.</span></span>

<span data-ttu-id="7dd14-217">**Doporučení:** umístit konektor téměř aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dd14-217">**Recommendation:** Place the connector near the app.</span></span> <span data-ttu-id="7dd14-218">Protože USA uživatelé přistupují k instanci Proxy aplikace, která se stane být ve stejné oblasti, není příliš nákladné směrování 1.</span><span class="sxs-lookup"><span data-stu-id="7dd14-218">Because US users are accessing an Application Proxy instance that happens to be in the same region, hop 1 is not too expensive.</span></span> <span data-ttu-id="7dd14-219">3 směrování je optimalizované.</span><span class="sxs-lookup"><span data-stu-id="7dd14-219">Hop 3 is optimized.</span></span> <span data-ttu-id="7dd14-220">Zvažte použití ExpressRoute za účelem optimalizace směrování 2.</span><span class="sxs-lookup"><span data-stu-id="7dd14-220">Consider using ExpressRoute to optimize hop 2.</span></span> 

![Diagram zobrazující uživatelů a proxy serveru v USA, s konektoru a aplikace v EU](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

<span data-ttu-id="7dd14-222">Můžete také použít jeden další variant v této situaci.</span><span class="sxs-lookup"><span data-stu-id="7dd14-222">You can also consider using one other variant in this situation.</span></span> <span data-ttu-id="7dd14-223">Pokud většina uživatelů v organizaci se v USA, pak pravděpodobné, který rozšiřuje vaší sítě do US také.</span><span class="sxs-lookup"><span data-stu-id="7dd14-223">If most users in the organization are in the US, then chances are that your network extends to the US as well.</span></span> <span data-ttu-id="7dd14-224">Umístěte konektor v USA a použijte řádek vyhrazené interní podnikové síti do aplikace v EU.</span><span class="sxs-lookup"><span data-stu-id="7dd14-224">Place the connector in the US, and use the dedicated internal corporate network line to the application in the EU.</span></span> <span data-ttu-id="7dd14-225">Tento způsob směrování 2 a 3 jsou optimalizované.</span><span class="sxs-lookup"><span data-stu-id="7dd14-225">This way hops 2 and 3 are optimized.</span></span>

![Diagram zobrazující proxy, uživatelů a konektor v USA, aplikace v EU](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a><span data-ttu-id="7dd14-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7dd14-227">Next steps</span></span>

- [<span data-ttu-id="7dd14-228">Povolení Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-228">Enable Application Proxy</span></span>](active-directory-application-proxy-enable.md)
- [<span data-ttu-id="7dd14-229">Povolení jednoduchého přihlášení</span><span class="sxs-lookup"><span data-stu-id="7dd14-229">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
- [<span data-ttu-id="7dd14-230">Povolení podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="7dd14-230">Enable conditional access</span></span>](active-directory-application-proxy-conditional-access.md)
- [<span data-ttu-id="7dd14-231">Řešení problémů, které máte s pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="7dd14-231">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)
