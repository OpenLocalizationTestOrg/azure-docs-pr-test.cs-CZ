---
title: "aaaAzure přehled CDN | Microsoft Docs"
description: "Zjistěte, jaké hello je Azure sítě pro doručování obsahu (CDN) a jak toouse ho toodeliver širokopásmového obsahu pomocí ukládání do mezipaměti objektů BLOB a statického obsahu."
services: cdn
documentationcenter: 
author: smcevoy
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/08/2017
ms.author: v-semcev
ms.openlocfilehash: e0230a6e107969b845985f2f4d357bf93cd40d42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-azure-content-delivery-network-cdn"></a><span data-ttu-id="8fc1b-103">Přehled hello Azure Content Delivery Network (CDN)</span><span class="sxs-lookup"><span data-stu-id="8fc1b-103">Overview of hello Azure Content Delivery Network (CDN)</span></span>
> [!NOTE]
> <span data-ttu-id="8fc1b-104">Tento dokument popisuje, jaké hello je služba Content Delivery Network (CDN) Azure, jak to funguje a hello funkce všech produktů Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-104">This document describes what hello Azure Content Delivery Network (CDN) is, how it works, and hello features of each Azure CDN product.</span></span>  <span data-ttu-id="8fc1b-105">Pokud chcete tyto informace tooskip a přejděte rovnou tooa kurz o tom, najdete v části toocreate koncový bod CDN [používání CDN Azure](cdn-create-new-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-105">If you want tooskip this information and go straight tooa tutorial on how toocreate a CDN endpoint, see [Using Azure CDN](cdn-create-new-endpoint.md).</span></span>  <span data-ttu-id="8fc1b-106">Pokud chcete toosee seznam aktuálních umístění uzlů CDN, najdete v části [umístění POP CDN Azure](cdn-pop-locations.md).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-106">If you want toosee a list of current CDN node locations, see [Azure CDN POP Locations](cdn-pop-locations.md).</span></span>
> 
> 

<span data-ttu-id="8fc1b-107">Hello Azure sítě pro doručování obsahu (CDN) ukládá do mezipaměti na strategicky umístěných místech tooprovide maximální propustnost pro doručování obsahu toousers statický webový obsah.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-107">hello Azure Content Delivery Network (CDN) caches static web content at strategically placed locations tooprovide maximum throughput for delivering content toousers.</span></span>  <span data-ttu-id="8fc1b-108">Hello CDN nabízí vývojářům globální řešení pro doručování širokopásmového obsahu pomocí ukládání do mezipaměti obsah hello na fyzických uzlech napříč hello, world.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-108">hello CDN offers developers a global solution for delivering high-bandwidth content by caching hello content at physical nodes across hello world.</span></span> 

<span data-ttu-id="8fc1b-109">Příklady výhod Hello pomocí hello CDN toocache webových prostředků:</span><span class="sxs-lookup"><span data-stu-id="8fc1b-109">hello benefits of using hello CDN toocache web site assets include:</span></span>

* <span data-ttu-id="8fc1b-110">Lepší výkon a uživatelské prostředí pro koncové uživatele, zejména v případě, že pomocí aplikací, kde jsou vícenásobný potřeby tooload obsah.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-110">Better performance and user experience for end users, especially when using applications where multiple round-trips are required tooload content.</span></span>
* <span data-ttu-id="8fc1b-111">Velké škálování toobetter zvládání náhlého vysokého zatížení, jako při spuštění hello produktu spusťte událost.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-111">Large scaling toobetter handle instantaneous high load, like at hello start of a product launch event.</span></span>
* <span data-ttu-id="8fc1b-112">Distribuci uživatelských požadavků a poskytování obsahu ze serverů edge, méně přenosy se odesílají toohello původu.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-112">By distributing user requests and serving content from edge servers, less traffic is sent toohello origin.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="8fc1b-113">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="8fc1b-113">How it works</span></span>
![Přehled CDN](./media/cdn-overview/cdn-overview.png)

1. <span data-ttu-id="8fc1b-115">Uživatel (Alice) požaduje soubor (také označovaný jako prostředek) pomocí adresy URL se speciálním názvem domény, například `<endpointname>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-115">A user (Alice) requests a file (also called an asset) using a URL with a special domain name, such as `<endpointname>.azureedge.net`.</span></span>  <span data-ttu-id="8fc1b-116">DNS přesměruje požadavek hello toohello nejlépe provádění Point of Presence (POP) umístění.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-116">DNS routes hello request toohello best performing Point-of-Presence (POP) location.</span></span>  <span data-ttu-id="8fc1b-117">To je obvykle POP, který je geograficky nejblíže uživatele toohello hello.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-117">Usually this is hello POP that is geographically closest toohello user.</span></span>
2. <span data-ttu-id="8fc1b-118">Pokud servery edge hello v hello POP nemají hello soubor v mezipaměti, hello edge server požádá o soubor hello z počátku hello.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-118">If hello edge servers in hello POP do not have hello file in their cache, hello edge server requests hello file from hello origin.</span></span>  <span data-ttu-id="8fc1b-119">původ Hello může být webová aplikace Azure, Cloudová služba Azure, účet úložiště Azure nebo jakékoli veřejně přístupný webový server.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-119">hello origin can be an Azure Web App, Azure Cloud Service, Azure Storage account, or any publicly accessible web server.</span></span>
3. <span data-ttu-id="8fc1b-120">Zdroj Hello vrátí hello souboru toohello serveru edge včetně volitelných hlaviček protokolu HTTP, které popisují hello souboru Time-to-Live (TTL).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-120">hello origin returns hello file toohello edge server, including optional HTTP headers describing hello file's Time-to-Live (TTL).</span></span>
4. <span data-ttu-id="8fc1b-121">Hello edge server ukládá do mezipaměti hello souboru a vrátí hello souboru toohello původnímu žadateli (Alici).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-121">hello edge server caches hello file and returns hello file toohello original requestor (Alice).</span></span>  <span data-ttu-id="8fc1b-122">soubor Hello zůstává v mezipaměti na serveru edge hello do vypršení platnosti hello TTL.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-122">hello file remains cached on hello edge server until hello TTL expires.</span></span>  <span data-ttu-id="8fc1b-123">Pokud hello zdroj nezadal hodnotu TTL, hello výchozí hodnota TTL je sedm dní.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-123">If hello origin didn't specify a TTL, hello default TTL is seven days.</span></span>
5. <span data-ttu-id="8fc1b-124">Další uživatelé, kteří mohou žádost hello stejný soubor pomocí stejné adresy URL a také může být směrovanou toothat stejný POP.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-124">Additional users may then request hello same file using that same URL, and may also be directed toothat same POP.</span></span>
6. <span data-ttu-id="8fc1b-125">Pokud hello TTL hello souboru nevypršela, vrátí hello edge server hello souboru z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-125">If hello TTL for hello file hasn't expired, hello edge server returns hello file from hello cache.</span></span>  <span data-ttu-id="8fc1b-126">To má za následek rychlejší a rychleji reagující uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-126">This results in a faster, more responsive user experience.</span></span>

## <a name="azure-cdn-features"></a><span data-ttu-id="8fc1b-127">Funkce Azure CDN</span><span class="sxs-lookup"><span data-stu-id="8fc1b-127">Azure CDN Features</span></span>
<span data-ttu-id="8fc1b-128">Existují tři produkty Azure CDN: **Azure CDN Standard od společnosti Akamai**, **Azure CDN Standard od společnosti Verizon** a **Azure CDN Premium od společnosti Verizon**.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-128">There are three Azure CDN products:  **Azure CDN Standard from Akamai**, **Azure CDN Standard from Verizon**, and **Azure CDN Premium from Verizon**.</span></span>  <span data-ttu-id="8fc1b-129">Hello následující tabulka uvádí funkce hello k dispozici u každého produktu.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-129">hello following table lists hello features available with each product.</span></span>

|  | <span data-ttu-id="8fc1b-130">Akamai Standard</span><span class="sxs-lookup"><span data-stu-id="8fc1b-130">Standard Akamai</span></span> | <span data-ttu-id="8fc1b-131">Verizon Standard</span><span class="sxs-lookup"><span data-stu-id="8fc1b-131">Standard Verizon</span></span> | <span data-ttu-id="8fc1b-132">Verizon Premium</span><span class="sxs-lookup"><span data-stu-id="8fc1b-132">Premium Verizon</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8fc1b-133">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Funkce a optimalizace výkonu__</span><span class="sxs-lookup"><span data-stu-id="8fc1b-133">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Performance Features and Optimizations__</span></span> |
| [<span data-ttu-id="8fc1b-134">Akcelerace dynamického webu</span><span class="sxs-lookup"><span data-stu-id="8fc1b-134">Dynamic Site Acceleration</span></span>](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | <span data-ttu-id="8fc1b-135">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-135">**&#x2713;**</span></span>  | <span data-ttu-id="8fc1b-136">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-136">**&#x2713;**</span></span> | <span data-ttu-id="8fc1b-137">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-137">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-138">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Akcelerace dynamického webu – adaptivní komprese obrázků](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only)</span><span class="sxs-lookup"><span data-stu-id="8fc1b-138">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamic Site Acceleration - Adaptive Image Compression](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only)</span></span> | <span data-ttu-id="8fc1b-139">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-139">**&#x2713;**</span></span>  |  |  |
| <span data-ttu-id="8fc1b-140">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Akcelerace dynamického webu – předběžné načtení objektů](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only)</span><span class="sxs-lookup"><span data-stu-id="8fc1b-140">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamic Site Acceleration - Object Prefetch](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only)</span></span> | <span data-ttu-id="8fc1b-141">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-141">**&#x2713;**</span></span>  |  |  |
| [<span data-ttu-id="8fc1b-142">Optimalizace streamování videa</span><span class="sxs-lookup"><span data-stu-id="8fc1b-142">Video Streaming Optimization</span></span>](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | <span data-ttu-id="8fc1b-143">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-143">**&#x2713;**</span></span>  | \* |  \* |
| [<span data-ttu-id="8fc1b-144">Optimalizace velkých souborů</span><span class="sxs-lookup"><span data-stu-id="8fc1b-144">Large File Optimization</span></span>](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | <span data-ttu-id="8fc1b-145">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-145">**&#x2713;**</span></span>  | \* |  \* |
| [<span data-ttu-id="8fc1b-146">Vyrovnávání zatížení globálního serveru (GSLB)</span><span class="sxs-lookup"><span data-stu-id="8fc1b-146">Global Server Load balancing (GSLB)</span></span>](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |<span data-ttu-id="8fc1b-147">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-147">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-148">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-148">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-149">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-149">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-150">Rychlé vyprázdnění</span><span class="sxs-lookup"><span data-stu-id="8fc1b-150">Fast purge</span></span>](cdn-purge-endpoint.md) |<span data-ttu-id="8fc1b-151">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-151">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-152">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-152">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-153">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-153">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-154">Předběžné načítání prostředku</span><span class="sxs-lookup"><span data-stu-id="8fc1b-154">Asset pre-loading</span></span>](cdn-preload-endpoint.md) | |<span data-ttu-id="8fc1b-155">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-155">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-156">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-156">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-157">Ukládání řetězce dotazu do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="8fc1b-157">Query string caching</span></span>](cdn-query-string.md) |<span data-ttu-id="8fc1b-158">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-158">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-159">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-159">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-160">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-160">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-161">Duální sada protokolů IPv4/IPv6</span><span class="sxs-lookup"><span data-stu-id="8fc1b-161">IPv4/IPv6 dual-stack</span></span> |<span data-ttu-id="8fc1b-162">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-162">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-163">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-163">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-164">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-164">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-165">Podpora HTTP/2</span><span class="sxs-lookup"><span data-stu-id="8fc1b-165">HTTP/2 support</span></span>](cdn-http2.md) |<span data-ttu-id="8fc1b-166">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-166">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-167">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-167">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-168">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-168">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-169">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Zabezpečení__</span><span class="sxs-lookup"><span data-stu-id="8fc1b-169">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Security__</span></span> |
| <span data-ttu-id="8fc1b-170">Podpora HTTPS s koncovými body CDN</span><span class="sxs-lookup"><span data-stu-id="8fc1b-170">HTTPS support with CDN endpoint</span></span> |<span data-ttu-id="8fc1b-171">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-171">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-172">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-172">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-173">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-173">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-174">HTTPS pro vlastní doménu</span><span class="sxs-lookup"><span data-stu-id="8fc1b-174">Custom domain HTTPS</span></span>](cdn-custom-ssl.md) | |<span data-ttu-id="8fc1b-175">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-175">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-176">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-176">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-177">Podpora vlastních názvů domén</span><span class="sxs-lookup"><span data-stu-id="8fc1b-177">Custom domain name support</span></span>](cdn-map-content-to-custom-domain.md) |<span data-ttu-id="8fc1b-178">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-178">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-179">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-179">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-180">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-180">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-181">Geografická filtrování</span><span class="sxs-lookup"><span data-stu-id="8fc1b-181">Geo-filtering</span></span>](cdn-restrict-access-by-country.md) |<span data-ttu-id="8fc1b-182">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-182">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-183">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-183">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-184">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-184">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-185">Ověření tokenu</span><span class="sxs-lookup"><span data-stu-id="8fc1b-185">Token authentication</span></span>](cdn-token-auth.md)|  |  |<span data-ttu-id="8fc1b-186">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-186">**&#x2713;**</span></span>| 
| [<span data-ttu-id="8fc1b-187">Ochrana proti útoku DDoS</span><span class="sxs-lookup"><span data-stu-id="8fc1b-187">DDOS protection</span></span>](https://www.us-cert.gov/ncas/tips/ST04-015) |<span data-ttu-id="8fc1b-188">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-188">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-189">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-189">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-190">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-190">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-191">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Analýzy a generování sestav__</span><span class="sxs-lookup"><span data-stu-id="8fc1b-191">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Analytics and Reporting__</span></span> |
| [<span data-ttu-id="8fc1b-192">Základní analýza</span><span class="sxs-lookup"><span data-stu-id="8fc1b-192">Core analytics</span></span>](cdn-analyze-usage-patterns.md) | <span data-ttu-id="8fc1b-193">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-193">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-194">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-194">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-195">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-195">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-196">Rozšířené sestavy HTTP</span><span class="sxs-lookup"><span data-stu-id="8fc1b-196">Advanced HTTP reports</span></span>](cdn-advanced-http-reports.md) | | |<span data-ttu-id="8fc1b-197">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-197">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-198">Statistiky v reálném čase</span><span class="sxs-lookup"><span data-stu-id="8fc1b-198">Real-time stats</span></span>](cdn-real-time-stats.md) | | |<span data-ttu-id="8fc1b-199">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-199">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-200">Výstrahy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="8fc1b-200">Real-time alerts</span></span>](cdn-real-time-alerts.md) | | |<span data-ttu-id="8fc1b-201">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-201">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-202">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Snadné použití__</span><span class="sxs-lookup"><span data-stu-id="8fc1b-202">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Ease of Use__</span></span> |
| <span data-ttu-id="8fc1b-203">Snadná integrace se službami Azure, jako jsou [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/app-service-web-tutorial-content-delivery-network.md) a [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="8fc1b-203">Easy integration with Azure services such as [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/app-service-web-tutorial-content-delivery-network.md), and [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md)</span></span> |<span data-ttu-id="8fc1b-204">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-204">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-205">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-205">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-206">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-206">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-207">Správa prostřednictvím [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) nebo [prostředí PowerShellu](cdn-manage-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="8fc1b-207">Management via [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md), or [PowerShell](cdn-manage-powershell.md).</span></span> |<span data-ttu-id="8fc1b-208">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-208">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-209">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-209">**&#x2713;**</span></span> |<span data-ttu-id="8fc1b-210">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-210">**&#x2713;**</span></span> |
| [<span data-ttu-id="8fc1b-211">Přizpůsobitelný modul doručování obsahu založený na pravidlech</span><span class="sxs-lookup"><span data-stu-id="8fc1b-211">Customizable, rule-based content delivery engine</span></span>](cdn-rules-engine.md) | | |<span data-ttu-id="8fc1b-212">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-212">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-213">Nastavení mezipaměti nebo hlaviček (pomocí [stroje pravidel](cdn-rules-engine.md))</span><span class="sxs-lookup"><span data-stu-id="8fc1b-213">Cache/header settings (using [rules engine](cdn-rules-engine.md))</span></span> | | |<span data-ttu-id="8fc1b-214">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-214">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-215">Přesměrování nebo přepsání adresy URL (pomocí [stroje pravidel](cdn-rules-engine.md))</span><span class="sxs-lookup"><span data-stu-id="8fc1b-215">URL redirect/rewrite  (using [rules engine](cdn-rules-engine.md))</span></span> | | |<span data-ttu-id="8fc1b-216">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-216">**&#x2713;**</span></span> |
| <span data-ttu-id="8fc1b-217">Pravidla mobilních zařízení (pomocí [stroje pravidel](cdn-rules-engine.md))</span><span class="sxs-lookup"><span data-stu-id="8fc1b-217">Mobile device rules (using [rules engine](cdn-rules-engine.md))</span></span> | | |<span data-ttu-id="8fc1b-218">**&amp;#x2713;**</span><span class="sxs-lookup"><span data-stu-id="8fc1b-218">**&#x2713;**</span></span> |

<span data-ttu-id="8fc1b-219">\* Verizon podporuje přímé doručování velkých souborů a médií prostřednictvím Obecného doručování webu.</span><span class="sxs-lookup"><span data-stu-id="8fc1b-219">\* Verizon supports delivering large files and media directly via General Web Delivery.</span></span>


> [!TIP]
> <span data-ttu-id="8fc1b-220">Je k dispozici funkci chcete toosee v Azure CDN?</span><span class="sxs-lookup"><span data-stu-id="8fc1b-220">Is there a feature you'd like toosee in Azure CDN?</span></span>  <span data-ttu-id="8fc1b-221">[Sdělte nám svůj názor](https://feedback.azure.com/forums/169397-cdn)!</span><span class="sxs-lookup"><span data-stu-id="8fc1b-221">[Give us feedback](https://feedback.azure.com/forums/169397-cdn)!</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="8fc1b-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8fc1b-222">Next steps</span></span>
<span data-ttu-id="8fc1b-223">tooget začít s CDN, najdete v části [používání CDN Azure](cdn-create-new-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-223">tooget started with CDN, see [Using Azure CDN](cdn-create-new-endpoint.md).</span></span>

<span data-ttu-id="8fc1b-224">Pokud jste stávající zákazník CDN, můžete nyní spravovat koncové body CDN prostřednictvím hello [portálu Microsoft Azure](https://portal.azure.com) nebo s [prostředí PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-224">If you are an existing CDN customer, you can now manage your CDN endpoints through hello [Microsoft Azure portal](https://portal.azure.com) or with [PowerShell](cdn-manage-powershell.md).</span></span>

<span data-ttu-id="8fc1b-225">toosee hello CDN v akci, podívejte se na hello [video z naší konference Build 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-225">toosee hello CDN in action, check out hello [video of our Build 2016 session](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).</span></span>

<span data-ttu-id="8fc1b-226">Zjistěte, jak tooautomate Azure CDN s [.NET](cdn-app-dev-net.md) nebo [Node.js](cdn-app-dev-node.md).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-226">Learn how tooautomate Azure CDN with [.NET](cdn-app-dev-net.md) or [Node.js](cdn-app-dev-node.md).</span></span>

<span data-ttu-id="8fc1b-227">Informace o cenách naleznete v tématu [Ceny CDN](https://azure.microsoft.com/pricing/details/cdn/).</span><span class="sxs-lookup"><span data-stu-id="8fc1b-227">For pricing information, see [CDN Pricing](https://azure.microsoft.com/pricing/details/cdn/).</span></span>
