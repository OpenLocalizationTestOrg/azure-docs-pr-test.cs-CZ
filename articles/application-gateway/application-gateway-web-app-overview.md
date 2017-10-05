---
title: "Přehled back-endů s více tenanty s použitím služby Azure Application Gateway | Dokumentace Microsoftu"
description: "Tato stránka poskytuje přehled podpory služby Application Gateway pro back-endy s více tenanty."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: d944904db5b0bf176b214249ad59611e2b794ae0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a><span data-ttu-id="dd7a7-103">Podpora služby Application Gateway pro back-endy s více tenanty</span><span class="sxs-lookup"><span data-stu-id="dd7a7-103">Application Gateway support for multi-tenant back ends</span></span>

<span data-ttu-id="dd7a7-104">Azure Application Gateway jako součást fondů back-end podporuje škálovací sady virtuálních počítačů, síťová rozhraní, veřejné a privátní IP adresy nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-104">Azure Application Gateway supports virtual machine scale sets, network interfaces, public/private IP, or fully qualified domain name (FQDN) as part of its back end pools.</span></span> <span data-ttu-id="dd7a7-105">Ve výchozím nastavení služba Application Gateway nemění hlavičku hostitele příchozího požadavku HTTP od klienta a nezměněnou hlavičku posílá na back-end.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-105">By default, application gateway does not change the incoming HTTP host header from the client and sends the header unaltered to the back end.</span></span> <span data-ttu-id="dd7a7-106">Existuje řada služeb jako [Azure Web Apps](../app-service-web/app-service-web-overview.md) a [API Management](../api-management/api-management-key-concepts.md), které jsou ze své podstaty s více tenanty a při překládání na správný koncový bod se spoléhají na konkrétní hlavičku hostitele nebo rozšíření SNI.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-106">There are many services like [Azure Web Apps](../app-service-web/app-service-web-overview.md) and [API Management](../api-management/api-management-key-concepts.md) that are multi-tenant in nature and rely on a specific host header or SNI extension to resolve to the correct endpoint.</span></span> <span data-ttu-id="dd7a7-107">Application Gateway nyní podporuje možnost uživatelů přepsat hlavičku hostitele příchozího požadavku HTTP na základě nastavení HTTP na straně back-endu.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-107">Application Gateway now supports the ability for users to overwrite the incoming HTTP host header based on the back end HTTP settings.</span></span> <span data-ttu-id="dd7a7-108">Tato schopnost umožňuje podporu back-endů s více tenanty Azure Web Apps a API Management.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-108">This capability enables support for multi-tenant back ends Azure web apps and API management.</span></span> <span data-ttu-id="dd7a7-109">Tato schopnost je dostupná pro standardní skladové položky i pro skladové položky WAF.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-109">This capability is available for both the standard and WAF SKU.</span></span> <span data-ttu-id="dd7a7-110">Podpora back-endu s více tenanty funguje také s ukončováním protokolu SSL a scénáři koncového šifrování protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-110">Multi-tenant back end support also works with SSL termination and end to end SSL scenarios.</span></span>

![scénář webové aplikace](./media/application-gateway-web-app-overview/scenario.png)

<span data-ttu-id="dd7a7-112">Možnost určit přepsání hostitele se definuje v nastavení HTTP a během vytváření pravidla je možné ji použít na jakýkoli fond back-end.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-112">The ability to specify a host override is defined at the HTTP settings and can be applied to any back end pool during rule creation.</span></span> <span data-ttu-id="dd7a7-113">Back-endy s více tenanty podporují následující dva způsoby přepsání hlavičky hostitele a rozšíření SNI.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-113">Multi-tenant back ends support the following two ways of overriding host header and SNI extension.</span></span>

1. <span data-ttu-id="dd7a7-114">Možnost nastavit název hostitele na pevnou hodnotu v nastavení HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-114">The ability to set the host name to a fixed value in the HTTP settings.</span></span> <span data-ttu-id="dd7a7-115">Tato možnost zajišťuje, že se hlavička hostitele přepíše na tuto hodnotu u veškerého provozu přicházejícího do fondu back-end, na který se nastavení HTTP použilo.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-115">This capability ensures that the host header is overridden to this value for all traffic to the back end pool where the HTTP settings are applied.</span></span> <span data-ttu-id="dd7a7-116">Při použití koncového šifrování protokolu SSL se přepsaný název hostitele použije v rozšíření SNI.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-116">When using end to end SSL, this overridden host name is used in the SNI extension.</span></span> <span data-ttu-id="dd7a7-117">Tato možnost umožňuje scénáře, kdy farma fondu back-end očekává hlavičku hostitele, která je jiná než příchozí zákaznická hlavička hostitele.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-117">This capability enables scenarios where a back end pool farm expects a host header that is different from the incoming customer host header.</span></span>

2. <span data-ttu-id="dd7a7-118">Schopnost odvodit název hostitele z IP adresy nebo plně kvalifikovaného názvu domény členů fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-118">The ability to derive the host name from the IP or FQDN of the back end pool members.</span></span> <span data-ttu-id="dd7a7-119">Nastavení HTTP nabízí také možnost vybrat název hostitele z plně kvalifikovaného názvu domény člena fondu back-end, pokud je nakonfigurovaná možnost odvodit název hostitele z jednotlivých členů fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-119">HTTP settings also provide an option to pick the host name from a back end pool member's FQDN if configured with the option to derive host name from an individual back end pool member.</span></span> <span data-ttu-id="dd7a7-120">Při použití koncového šifrování protokolu SSL se tento název hostitele odvodí z plně kvalifikovaného názvu domény a použije v rozšíření SNI.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-120">When using end to end SSL, this host name is derived from the FQDN and is used in the SNI extension.</span></span> <span data-ttu-id="dd7a7-121">Tato možnost umožňuje scénáře, kdy může mít fond back-end dvě nebo více služeb PaaS s více tenanty, jako jsou například webové aplikace Azure, a hlavička hostitele požadavku každého člena obsahuje název hostitele odvozený z jeho plně kvalifikovaného názvu domény.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-121">This capability enables scenarios where a back end pool can have two or more multi-tenant PaaS services like Azure web apps and the request's host header to each member contains the host name derived from its FQDN.</span></span>

> [!NOTE]
> <span data-ttu-id="dd7a7-122">V obou předchozích případech má nastavení vliv pouze na chování živého provozu a ne na chování sondy stavu.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-122">In both of the preceding cases the settings only affect the live traffic behavior and not the health probe behavior.</span></span> <span data-ttu-id="dd7a7-123">Vlastní sondy už podporují možnost zadat hlavičku hostitele v konfiguraci sondy.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-123">Custom probes already support the ability to specify a host header in the probe configuration.</span></span> <span data-ttu-id="dd7a7-124">Vlastní sondy nyní podporují také možnost odvodit chování hlavičky hostitele z aktuálně nakonfigurovaného nastavení HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-124">Custom probes now also support the ability to derive the host header behavior from the currently configured HTTP settings.</span></span> <span data-ttu-id="dd7a7-125">Tuto konfiguraci je možné zadat pomocí parametru `PickHostNameFromback endAddress` v konfiguraci sondy.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-125">This configuration can be specified by using the `PickHostNameFromback endAddress` parameter in the probe configuration.</span></span> <span data-ttu-id="dd7a7-126">Aby fungovala funkce koncového šifrování, sondu i nastavení HTTP je potřeba upravit tak, aby odrážely správnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-126">For end to end functionality to work, both the probe and the HTTP settings must be modified to reflect the correct configuration.</span></span>

<span data-ttu-id="dd7a7-127">Díky této schopnosti můžou zákazníci zadat možnosti v nastavení HTTP a vlastních sondách na odpovídající konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-127">With this capability, customers specify the options in the HTTP settings and custom probes to the appropriate configuration.</span></span> <span data-ttu-id="dd7a7-128">Toto nastavení se pak pomocí pravidla naváže na naslouchací proces a fond back-end.</span><span class="sxs-lookup"><span data-stu-id="dd7a7-128">This setting is then tied to a listener and a back end pool by using a rule.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd7a7-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd7a7-129">Next steps</span></span>

<span data-ttu-id="dd7a7-130">Zjistěte, jak nastavit službu Application Gateway s webovou aplikací jako členem fondu back-end, v tématu [Konfigurace webových aplikací App Service pomocí služby Application Gateway](application-gateway-web-app-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="dd7a7-130">Learn how to set up an application gateway with a web app as a back end pool member by visiting: [Configure App Service web apps with Application Gateway](application-gateway-web-app-powershell.md)</span></span>