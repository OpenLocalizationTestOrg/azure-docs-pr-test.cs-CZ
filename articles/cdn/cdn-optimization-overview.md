---
title: "Optimalizace doručování obsahu Azure pro váš scénář"
description: "K optimalizaci doručování obsahu pro konkrétní scénáře"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 98941c49b057380b3ef9164515bcc2a63ccb56ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a><span data-ttu-id="69330-103">Optimalizace doručování obsahu Azure pro váš scénář</span><span class="sxs-lookup"><span data-stu-id="69330-103">Optimize Azure content delivery for your scenario</span></span>

<span data-ttu-id="69330-104">Při doručování obsahu do velkou globální cílovou skupinu, je důležité zajistit optimalizované doručování obsahu.</span><span class="sxs-lookup"><span data-stu-id="69330-104">When you deliver content to a large global audience, it's critical to ensure the optimized delivery of your content.</span></span> <span data-ttu-id="69330-105">Sítě pro doručování obsahu Azure můžete optimalizovat prostředí doručení založené na typ obsahu, které máte.</span><span class="sxs-lookup"><span data-stu-id="69330-105">The Azure Content Delivery Network can optimize the delivery experience based on the type of content you have.</span></span> <span data-ttu-id="69330-106">Obsah může být web, živý datový proud, video nebo velký soubor ke stažení.</span><span class="sxs-lookup"><span data-stu-id="69330-106">Content can be a website, a live stream, a video, or a large file for download.</span></span> <span data-ttu-id="69330-107">Když vytvoříte koncový bod doručování obsahu (CDN) sítě, zadáte scénář v **optimalizované pro** možnost.</span><span class="sxs-lookup"><span data-stu-id="69330-107">When you create a content delivery network (CDN) endpoint, you specify a scenario in the **Optimized for** option.</span></span> <span data-ttu-id="69330-108">Vaše volba určuje, které optimalizace se použije k obsahu doručit z koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="69330-108">Your choice determines which optimization is applied to the content delivered from the CDN endpoint.</span></span>

<span data-ttu-id="69330-109">Možnosti optimalizace navržena pro použití chování osvědčené postupy pro zlepšení výkonu doručování obsahu a snižování zátěže lepší původu.</span><span class="sxs-lookup"><span data-stu-id="69330-109">Optimization choices are designed to use best-practice behaviors to improve content delivery performance and better origin offload.</span></span> <span data-ttu-id="69330-110">Vaše volby scénář ovlivnit výkon změnou konfigurace pro částečné ukládání do mezipaměti, objekt rozdělování a zásady opakování selhání původu.</span><span class="sxs-lookup"><span data-stu-id="69330-110">Your scenario choices affect performance by modifying configurations for partial caching, object chunking, and the origin failure retry policy.</span></span> 

<span data-ttu-id="69330-111">Tento článek obsahuje přehled různých funkcí optimalizace a kdy byste měli použít.</span><span class="sxs-lookup"><span data-stu-id="69330-111">This article provides an overview of various optimization features and when you should use them.</span></span> <span data-ttu-id="69330-112">Další informace o funkcích a omezení naleznete v příslušných článcích na každý typ jednotlivých optimalizace.</span><span class="sxs-lookup"><span data-stu-id="69330-112">For more information on features and limitations, see the respective articles on each individual optimization type.</span></span>

> [!NOTE]
> <span data-ttu-id="69330-113">Vaše **optimalizované pro** možnosti může lišit v závislosti na zprostředkovateli vyberete.</span><span class="sxs-lookup"><span data-stu-id="69330-113">Your **Optimized for** options can vary based on the provider you select.</span></span> <span data-ttu-id="69330-114">Vylepšení zprostředkovatele CDN použít různými způsoby v závislosti na scénáři.</span><span class="sxs-lookup"><span data-stu-id="69330-114">CDN providers apply enhancement in different ways, depending on the scenario.</span></span> 

## <a name="provider-options"></a><span data-ttu-id="69330-115">Možnosti zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="69330-115">Provider options</span></span>

<span data-ttu-id="69330-116">Podporuje Azure Content Delivery Network společnosti Akamai:</span><span class="sxs-lookup"><span data-stu-id="69330-116">The Azure Content Delivery Network from Akamai supports:</span></span>

* <span data-ttu-id="69330-117">Obecné webové doručení</span><span class="sxs-lookup"><span data-stu-id="69330-117">General web delivery</span></span> 

* <span data-ttu-id="69330-118">Obecné streamování médií</span><span class="sxs-lookup"><span data-stu-id="69330-118">General media streaming</span></span>

* <span data-ttu-id="69330-119">Streamování videa na přání médií</span><span class="sxs-lookup"><span data-stu-id="69330-119">Video-on-demand media streaming</span></span>

* <span data-ttu-id="69330-120">Stažení velkých souborů</span><span class="sxs-lookup"><span data-stu-id="69330-120">Large file download</span></span>

* <span data-ttu-id="69330-121">Akcelerace dynamických webů</span><span class="sxs-lookup"><span data-stu-id="69330-121">Dynamic site acceleration</span></span> 

<span data-ttu-id="69330-122">Azure Content Delivery Network od společnosti Verizon podporuje pouze obecné webové doručení.</span><span class="sxs-lookup"><span data-stu-id="69330-122">The Azure Content Delivery Network from Verizon supports general web delivery only.</span></span> <span data-ttu-id="69330-123">Slouží pro video na vyžádání a stahování velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="69330-123">It can be used for video on demand and large file download.</span></span> <span data-ttu-id="69330-124">Nemáte chcete vybrat typ optimalizace.</span><span class="sxs-lookup"><span data-stu-id="69330-124">You don't have to select an optimization type.</span></span>

<span data-ttu-id="69330-125">Důrazně doporučujeme, abyste otestovali výkonu rozdíly mezi různé zprostředkovatele můžete vybrat optimální zprostředkovatele pro vaše doručení.</span><span class="sxs-lookup"><span data-stu-id="69330-125">We highly recommend that you test performance variations between different providers to select the optimal provider for your delivery.</span></span>

## <a name="select-and-configure-optimization-types"></a><span data-ttu-id="69330-126">Vyberte a nakonfigurujte optimalizace typy</span><span class="sxs-lookup"><span data-stu-id="69330-126">Select and configure optimization types</span></span>

<span data-ttu-id="69330-127">Pokud chcete vytvořit nový koncový bod, vyberte typ optimalizace, který nejlépe odpovídá scénář a typ obsahu, který chcete, aby koncový bod pro doručování.</span><span class="sxs-lookup"><span data-stu-id="69330-127">To create a new endpoint, select an optimization type that best matches the scenario and type of content that you want the endpoint to deliver.</span></span> <span data-ttu-id="69330-128">**Obecné webové doručení** je výchozí výběr.</span><span class="sxs-lookup"><span data-stu-id="69330-128">**General web delivery** is the default selection.</span></span> <span data-ttu-id="69330-129">Můžete aktualizovat možnosti optimalizace pro jakékoli existující koncový bod Akamai kdykoli.</span><span class="sxs-lookup"><span data-stu-id="69330-129">You can update the optimization option for any existing Akamai endpoint at any time.</span></span> <span data-ttu-id="69330-130">Tato změna nemá přerušit doručování z CDN.</span><span class="sxs-lookup"><span data-stu-id="69330-130">This change doesn't interrupt delivery from the CDN.</span></span> 

1. <span data-ttu-id="69330-131">Vyberte koncový bod v rámci profilu Standard Akamai.</span><span class="sxs-lookup"><span data-stu-id="69330-131">Select an endpoint within a Standard Akamai profile.</span></span>

    ![<span data-ttu-id="69330-132">Výběr koncového bodu</span><span class="sxs-lookup"><span data-stu-id="69330-132">Endpoint selection</span></span> ](./media/cdn-optimization-overview/01_Akamai.png)

2. <span data-ttu-id="69330-133">V části **nastavení**, vyberte **optimalizace**.</span><span class="sxs-lookup"><span data-stu-id="69330-133">Under **SETTINGS**, select **Optimization**.</span></span> <span data-ttu-id="69330-134">Pak vyberte typ z **optimalizované pro** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="69330-134">Then select a type from the **Optimized for** drop-down list.</span></span>

    ![Optimalizace a typ výběru](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a><span data-ttu-id="69330-136">Optimalizace pro konkrétní scénáře</span><span class="sxs-lookup"><span data-stu-id="69330-136">Optimization for specific scenarios</span></span>

<span data-ttu-id="69330-137">Koncový bod CDN pro jednu z následujících scénářů, můžete optimalizovat.</span><span class="sxs-lookup"><span data-stu-id="69330-137">You can optimize the CDN endpoint for one of the following scenarios.</span></span> 

### <a name="general-web-delivery"></a><span data-ttu-id="69330-138">Obecné webové doručení</span><span class="sxs-lookup"><span data-stu-id="69330-138">General web delivery</span></span>

<span data-ttu-id="69330-139">Obecné webové doručení je nejběžnější možnosti optimalizace.</span><span class="sxs-lookup"><span data-stu-id="69330-139">General web delivery is the most common optimization option.</span></span> <span data-ttu-id="69330-140">Je určený pro optimalizaci obecné webového obsahu, například webových stránek a webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="69330-140">It's designed for general web content optimization, such as webpages and web applications.</span></span> <span data-ttu-id="69330-141">Tato optimalizace můžete použít také pro soubor, a stáhne video.</span><span class="sxs-lookup"><span data-stu-id="69330-141">This optimization also can be used for file and video downloads.</span></span>

<span data-ttu-id="69330-142">Typické webu obsahuje obsah statické a dynamické.</span><span class="sxs-lookup"><span data-stu-id="69330-142">A typical website contains static and dynamic content.</span></span> <span data-ttu-id="69330-143">Statický obsah zahrnuje obrázků, knihovny jazyka JavaScript a stylů, které lze do mezipaměti a doručit různým uživatelům.</span><span class="sxs-lookup"><span data-stu-id="69330-143">Static content includes images, JavaScript libraries, and style sheets that can be cached and delivered to different users.</span></span> <span data-ttu-id="69330-144">Dynamický obsah je přizpůsobené pro jednotlivé uživatele, například položky zpráv, které jsou přizpůsobené profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="69330-144">Dynamic content is personalized for an individual user, such as news items that are tailored to a user profile.</span></span> <span data-ttu-id="69330-145">Dynamický obsah neukládá do mezipaměti, protože je jedinečný pro každého uživatele, třeba nákupní košík obsah.</span><span class="sxs-lookup"><span data-stu-id="69330-145">Dynamic content isn't cached because it's unique to each user, such as shopping cart contents.</span></span> <span data-ttu-id="69330-146">Obecné webové doručení můžete optimalizovat celý web.</span><span class="sxs-lookup"><span data-stu-id="69330-146">General web delivery can optimize your entire website.</span></span> 

> [!NOTE]
> <span data-ttu-id="69330-147">Pokud používáte Azure Content Delivery Network společnosti Akamai, můžete chtít použít optimalizace, pokud vaše Průměrná velikost je menší než 10 MB.</span><span class="sxs-lookup"><span data-stu-id="69330-147">If you use the Azure Content Delivery Network from Akamai, you might want to use this optimization if your average file size is smaller than 10 MB.</span></span> <span data-ttu-id="69330-148">Pokud vaše Průměrná velikost souboru je větší než 10 MB, vyberte **stahování velkých souborů** z **optimalizované pro** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="69330-148">If your average file size is larger than 10 MB, select **Large file download** from the **Optimized for** drop-down list.</span></span>

### <a name="general-media-streaming"></a><span data-ttu-id="69330-149">Obecné streamování médií</span><span class="sxs-lookup"><span data-stu-id="69330-149">General media streaming</span></span>

<span data-ttu-id="69330-150">Pokud budete muset použít koncový bod pro živé streamování a streamování videa na přání, doporučujeme streamování optimalizace obecné médií.</span><span class="sxs-lookup"><span data-stu-id="69330-150">If you need to use the endpoint for live streaming and video-on-demand streaming, we recommend general media streaming optimization.</span></span>

<span data-ttu-id="69330-151">Datové proudy Media je čas citlivé, protože paketů, které přicházejí pozdní na straně klienta může způsobit sníženou zobrazení prostředí, jako je například časté ukládání do vyrovnávací paměti obsahu videa.</span><span class="sxs-lookup"><span data-stu-id="69330-151">Media streaming is time sensitive, because packets that arrive late on the client can cause a degraded viewing experience, such as frequent buffering of video content.</span></span> <span data-ttu-id="69330-152">Optimalizace streamování médií snižuje latenci doručování obsahu pro média a poskytuje technologie smooth streaming prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="69330-152">Media streaming optimization reduces the latency of media content delivery and provides a smooth streaming experience for users.</span></span> 

<span data-ttu-id="69330-153">Tento scénář je běžný u Zákazníci využívající službu Azure media.</span><span class="sxs-lookup"><span data-stu-id="69330-153">This scenario is common for Azure media service customers.</span></span> <span data-ttu-id="69330-154">Když používáte službu Azure media services, zobrazí jeden streamování koncový bod, který lze použít pro datové proudy živě i na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="69330-154">When you use Azure media services, you get one streaming endpoint that can be used for both live and on-demand streaming.</span></span> <span data-ttu-id="69330-155">V tomto scénáři není nutné přepnout na jiný koncový bod, když se změní ze za provozu se streamování na vyžádání zákazníků.</span><span class="sxs-lookup"><span data-stu-id="69330-155">With this scenario, customers don't need to switch to another endpoint when they change from live to on-demand streaming.</span></span> <span data-ttu-id="69330-156">Obecné média streamování optimalizace podporuje tento typ scénáře.</span><span class="sxs-lookup"><span data-stu-id="69330-156">General media streaming optimization supports this type of scenario.</span></span>

<span data-ttu-id="69330-157">Síť doručování obsahu Azure od společnosti Verizon používá daný typ doručení optimalizace obecné webové při doručování obsahu streamování médií.</span><span class="sxs-lookup"><span data-stu-id="69330-157">The Azure Content Delivery Network from Verizon uses the general web delivery optimization type to deliver streaming media content.</span></span>

<span data-ttu-id="69330-158">Další informace o optimalizaci streamování médií najdete v tématu [optimalizace streamování médií](cdn-media-streaming-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="69330-158">To learn more about media streaming optimization, see [Media streaming optimization](cdn-media-streaming-optimization.md).</span></span>

### <a name="video-on-demand-media-streaming"></a><span data-ttu-id="69330-159">Streamování videa na přání médií</span><span class="sxs-lookup"><span data-stu-id="69330-159">Video-on-demand media streaming</span></span>

<span data-ttu-id="69330-160">Optimalizace streamování na vyžádání Video-on-Demand média zlepšuje streamování obsahu na vyžádání video-on-demand.</span><span class="sxs-lookup"><span data-stu-id="69330-160">Video-on-demand media streaming optimization improves video-on-demand streaming content.</span></span> <span data-ttu-id="69330-161">Pokud používáte koncový bod pro vyžádání video-on-demand streamování, můžete chtít použít tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="69330-161">If you use an endpoint for video-on-demand streaming, you might want to use this option.</span></span>

<span data-ttu-id="69330-162">Síť doručování obsahu Azure od společnosti Verizon používá daný typ doručení optimalizace obecné webové při doručování obsahu streamování médií.</span><span class="sxs-lookup"><span data-stu-id="69330-162">The Azure Content Delivery Network from Verizon uses the general web delivery optimization type to deliver streaming media content.</span></span>

<span data-ttu-id="69330-163">Další informace o optimalizaci streamování médií najdete v tématu [optimalizace streamování médií](cdn-media-streaming-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="69330-163">To learn more about media streaming optimization, see [Media streaming optimization](cdn-media-streaming-optimization.md).</span></span>

> [!NOTE]
> <span data-ttu-id="69330-164">Pokud koncový bod slouží především na vyžádání video-on-demand obsahu, použijte tento typ optimalizace.</span><span class="sxs-lookup"><span data-stu-id="69330-164">If the endpoint primarily serves video-on-demand content, use this optimization type.</span></span> <span data-ttu-id="69330-165">Hlavní rozdíl mezi optimalizace a streamování optimalizace obecné médií je časový limit opakování připojení.</span><span class="sxs-lookup"><span data-stu-id="69330-165">The major difference between this optimization and the general media streaming optimization is the connection retry time-out.</span></span> <span data-ttu-id="69330-166">Časový limit je mnohem kratší pro práci s živé streamování scénáře.</span><span class="sxs-lookup"><span data-stu-id="69330-166">The time-out is much shorter to work with live streaming scenarios.</span></span>

### <a name="large-file-download"></a><span data-ttu-id="69330-167">Stažení velkých souborů</span><span class="sxs-lookup"><span data-stu-id="69330-167">Large file download</span></span>

<span data-ttu-id="69330-168">Pokud používáte Azure Content Delivery Network společnosti Akamai, musíte použít velkých souborů stahování dodávat soubory větší než 1,8 GB.</span><span class="sxs-lookup"><span data-stu-id="69330-168">If you use the Azure Content Delivery Network from Akamai, you must use large file download to deliver files larger than 1.8 GB.</span></span> <span data-ttu-id="69330-169">Azure Content Delivery Network od společnosti Verizon nemá omezení na soubor stáhnout velikost v jeho doručení optimalizaci obecné webů.</span><span class="sxs-lookup"><span data-stu-id="69330-169">The Azure Content Delivery Network from Verizon doesn't have a limitation on file download size in its general web delivery optimization.</span></span>

<span data-ttu-id="69330-170">Pokud používáte Azure Network doručování obsahu společnosti Akamai, stahování velkých souborů jsou optimalizované pro obsah větší než 10 MB.</span><span class="sxs-lookup"><span data-stu-id="69330-170">If you use the Azure Content Delivery Network from Akamai, large file downloads are optimized for content larger than 10 MB.</span></span> <span data-ttu-id="69330-171">Pokud vaše Průměrná velikost souboru je menší než 10 MB, můžete chtít použít obecné webové doručení.</span><span class="sxs-lookup"><span data-stu-id="69330-171">If your average file size is smaller than 10 MB, you might want to use general web delivery.</span></span> <span data-ttu-id="69330-172">Pokud vaše soubory průměrné velikosti jsou konzistentně větší než 10 MB, může to být efektivnější vytvořit samostatné koncový bod pro velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="69330-172">If your average files sizes are consistently larger than 10 MB, it might be more efficient to create a separate endpoint for large files.</span></span> <span data-ttu-id="69330-173">Aktualizace firmwaru a softwaru jsou například obvykle velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="69330-173">For example, firmware or software updates typically are large files.</span></span>

<span data-ttu-id="69330-174">Síť doručování obsahu Azure od společnosti Verizon používá daný typ doručení optimalizace obecné webové při doručování obsahu streamování médií.</span><span class="sxs-lookup"><span data-stu-id="69330-174">The Azure Content Delivery Network from Verizon uses the general web delivery optimization type to deliver streaming media content.</span></span>

<span data-ttu-id="69330-175">Další informace o optimalizaci velkých souborů najdete v tématu [velkých souborů optimalizace](cdn-large-file-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="69330-175">To learn more about large file optimization, see [Large file optimization](cdn-large-file-optimization.md).</span></span>

### <a name="dynamic-site-acceleration"></a><span data-ttu-id="69330-176">Akcelerace dynamických webů</span><span class="sxs-lookup"><span data-stu-id="69330-176">Dynamic site acceleration</span></span>

 <span data-ttu-id="69330-177">Akcelerace dynamických webů je k dispozici z Akamai a Verizon Content Delivery Network profily.</span><span class="sxs-lookup"><span data-stu-id="69330-177">Dynamic site acceleration is available from both Akamai and Verizon Content Delivery Network profiles.</span></span> <span data-ttu-id="69330-178">Tato optimalizace zahrnuje další poplatek používat.</span><span class="sxs-lookup"><span data-stu-id="69330-178">This optimization involves an additional fee to use.</span></span> <span data-ttu-id="69330-179">Další informace najdete na stránce s cenami.</span><span class="sxs-lookup"><span data-stu-id="69330-179">For more information, see the pricing page.</span></span>

<span data-ttu-id="69330-180">Akcelerace dynamických webů zahrnuje různé techniky využívající latenci a výkonu dynamického obsahu.</span><span class="sxs-lookup"><span data-stu-id="69330-180">Dynamic site acceleration includes various techniques that benefit the latency and performance of dynamic content.</span></span> <span data-ttu-id="69330-181">Mezi dostupné techniky patří trasy a síťové optimalizace, optimalizace TCP a další.</span><span class="sxs-lookup"><span data-stu-id="69330-181">Techniques include route and network optimization, TCP optimization, and more.</span></span> 

<span data-ttu-id="69330-182">Tato optimalizace vám pomůže urychlit webovou aplikaci, která zahrnuje mnoho odpovědí, které nejsou lze uložit do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="69330-182">You can use this optimization to accelerate a web app that includes numerous responses that aren't cacheable.</span></span> <span data-ttu-id="69330-183">Příklady jsou výsledky hledání, najdete v článku věnovaném transakce nebo dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="69330-183">Examples are search results, checkout transactions, or real-time data.</span></span> <span data-ttu-id="69330-184">Můžete nadále používat možnosti ukládání do mezipaměti jádra CDN statických dat.</span><span class="sxs-lookup"><span data-stu-id="69330-184">You can continue to use core CDN caching capabilities for static data.</span></span> 



