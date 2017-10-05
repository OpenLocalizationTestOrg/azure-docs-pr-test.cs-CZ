---
title: "Optimalizace stahování velkých souborů přes síť doručování obsahu Azure"
description: "Optimalizace vysvětlené podrobněji stahování velkých souborů"
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
ms.openlocfilehash: 7a5d5d1d0de24ebb0a5115ede1e572f38454bd78
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="large-file-download-optimization-via-the-azure-content-delivery-network"></a><span data-ttu-id="d2df1-103">Optimalizace stahování velkých souborů přes síť doručování obsahu Azure</span><span class="sxs-lookup"><span data-stu-id="d2df1-103">Large file download optimization via the Azure Content Delivery Network</span></span>

<span data-ttu-id="d2df1-104">Velikosti souborů obsahu doručit přes Internet pořád roste s tím kvůli rozšířené funkce, vylepšené grafika a bohatý mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="d2df1-104">File sizes of content delivered over the Internet continue to grow due to enhanced functionality, improved graphics, and rich media content.</span></span> <span data-ttu-id="d2df1-105">Tento nárůst vycházejí z hlavních faktorů: širokopásmové průnikům, větší zařízení nenákladné úložiště, zvýšení rozšířeným vysokým rozlišením video a připojené k Internetu zařízení (IoT).</span><span class="sxs-lookup"><span data-stu-id="d2df1-105">This growth is driven by many factors: broadband penetration, larger inexpensive storage devices, widespread increase of high-definition video, and Internet-connected devices (IoT).</span></span> <span data-ttu-id="d2df1-106">Doručení rychlé a efektivní mechanismus pro velkých souborů je velmi důležité, aby bylo prostředí hladký a zábavná příjemce.</span><span class="sxs-lookup"><span data-stu-id="d2df1-106">A fast and efficient delivery mechanism for large files is critical to ensure a smooth and enjoyable consumer experience.</span></span>

<span data-ttu-id="d2df1-107">Doručování velkých souborů má několik problémy.</span><span class="sxs-lookup"><span data-stu-id="d2df1-107">Delivery of large files has several challenges.</span></span> <span data-ttu-id="d2df1-108">Průměrná doba stahování velký soubor nejdřív může být důležité, protože všechna data aplikace nemusí stahovat postupně.</span><span class="sxs-lookup"><span data-stu-id="d2df1-108">First, the average time to download a large file can be significant because applications might not download all data sequentially.</span></span> <span data-ttu-id="d2df1-109">V některých případech může aplikace stahovat poslední část souboru před první část.</span><span class="sxs-lookup"><span data-stu-id="d2df1-109">In some cases, applications might download the last part of a file before the first part.</span></span> <span data-ttu-id="d2df1-110">Pokud se požaduje jenom malé množství soubor nebo nastavení stahování, stahování může selhat.</span><span class="sxs-lookup"><span data-stu-id="d2df1-110">When only a small amount of a file is requested or a user pauses a download, the download can fail.</span></span> <span data-ttu-id="d2df1-111">Stahování také může dojít ke zpoždění až po síti pro doručování obsahu (CDN) načte celý soubor ze zdrojového serveru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-111">The download also might be delayed until after the content delivery network (CDN) retrieves the entire file from the origin server.</span></span> 

<span data-ttu-id="d2df1-112">Druhý latenci mezi počítačem uživatele a soubor určuje rychlost, kdy se může zobrazit obsah.</span><span class="sxs-lookup"><span data-stu-id="d2df1-112">Second, the latency between a user's machine and the file determines the speed at which they can view content.</span></span> <span data-ttu-id="d2df1-113">Kromě toho problémy zahlcení a kapacitu sítě také ovlivnit propustnost.</span><span class="sxs-lookup"><span data-stu-id="d2df1-113">In addition, network congestion and capacity problems also affect throughput.</span></span> <span data-ttu-id="d2df1-114">Větší vzdálenosti mezi servery a uživatelé vytvoření dalších možností pro ke ztrátě paketů odesílaných dojde k, což snižuje kvality.</span><span class="sxs-lookup"><span data-stu-id="d2df1-114">Greater distances between servers and users create additional opportunities for packet loss to occur, which reduces quality.</span></span> <span data-ttu-id="d2df1-115">Snížení kvality způsobené omezená propustnost a ztráta paketů vyšší může se prodloužit doba čekání stahování souborů k dokončení.</span><span class="sxs-lookup"><span data-stu-id="d2df1-115">The reduction in quality caused by limited throughput and increased packet loss might increase the wait time for a file download to finish.</span></span> 

<span data-ttu-id="d2df1-116">Třetí mnoho velkých souborů nejsou doručeny jako celek.</span><span class="sxs-lookup"><span data-stu-id="d2df1-116">Third, many large files are not delivered in their entirety.</span></span> <span data-ttu-id="d2df1-117">Uživatelé mohou zrušit uprostřed stahování nebo si pusťte pouze první několik minut dlouhé video MP4.</span><span class="sxs-lookup"><span data-stu-id="d2df1-117">Users might cancel a download halfway through or watch only the first few minutes of a long MP4 video.</span></span> <span data-ttu-id="d2df1-118">Proto software a média doručení společnosti chcete poskytovat jenom ta část souboru, který je požadovaný.</span><span class="sxs-lookup"><span data-stu-id="d2df1-118">Therefore, software and media delivery companies want to deliver only the portion of a file that's requested.</span></span> <span data-ttu-id="d2df1-119">Efektivní distribuci požadované části snižuje výchozí přenos ze zdrojového serveru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-119">Efficient distribution of the requested portions reduces the egress traffic from the origin server.</span></span> <span data-ttu-id="d2df1-120">Efektivní distribuční zmenšuje paměti a vstupu a výstupu tlak na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-120">Efficient distribution also reduces the memory and I/O pressure on the origin server.</span></span> 

<span data-ttu-id="d2df1-121">Azure Content Delivery Network společnosti Akamai teď nabízí funkce, která poskytuje velkých souborů efektivně uživatelům po celém světě ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="d2df1-121">The Azure Content Delivery Network from Akamai now offers a feature that delivers large files efficiently to users across the globe at scale.</span></span> <span data-ttu-id="d2df1-122">Funkci snižuje latenci, protože snižuje zatížení serverů původu.</span><span class="sxs-lookup"><span data-stu-id="d2df1-122">The feature reduces latencies because it reduces the load on the origin servers.</span></span> <span data-ttu-id="d2df1-123">Tato funkce je k dispozici s Akamai Standard cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="d2df1-123">This feature is available with the Standard Akamai pricing tier.</span></span>

## <a name="configure-a-cdn-endpoint-to-optimize-delivery-of-large-files"></a><span data-ttu-id="d2df1-124">Konfigurace koncového bodu CDN k optimalizaci doručování velkých souborů</span><span class="sxs-lookup"><span data-stu-id="d2df1-124">Configure a CDN endpoint to optimize delivery of large files</span></span>

<span data-ttu-id="d2df1-125">Můžete nakonfigurovat koncový bod CDN k optimalizaci doručování velkých souborů prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d2df1-125">You can configure your CDN endpoint to optimize delivery for large files via the Azure portal.</span></span> <span data-ttu-id="d2df1-126">K tomu můžete také použít rozhraní API REST nebo některého z klienta sady SDK.</span><span class="sxs-lookup"><span data-stu-id="d2df1-126">You can also use our REST APIs or any of the client SDKs to do this.</span></span> <span data-ttu-id="d2df1-127">Následující kroky ukazují proces prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d2df1-127">The following steps show the process via the Azure portal.</span></span>

1. <span data-ttu-id="d2df1-128">Chcete-li přidat nový koncový bod na **profil CDN** vyberte **koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="d2df1-128">To add a new endpoint, on the **CDN profile** page, select **Endpoint**.</span></span>

    ![Nový koncový bod](./media/cdn-large-file-optimization/01_Adding.png)  
 
2. <span data-ttu-id="d2df1-130">V **optimalizované pro** rozevíracího seznamu vyberte **stahování velkých souborů**.</span><span class="sxs-lookup"><span data-stu-id="d2df1-130">In the **Optimized for** drop-down list, select **Large file download**.</span></span>

    ![Vybrané optimalizace velkých souborů](./media/cdn-large-file-optimization/02_Creating.png)


<span data-ttu-id="d2df1-132">Po vytvoření koncového bodu CDN, bude se vztahovat optimalizace velkých souborů pro všechny soubory, které splňují určitá kritéria.</span><span class="sxs-lookup"><span data-stu-id="d2df1-132">After you create the CDN endpoint, it applies the large file optimizations for all files that match certain criteria.</span></span> <span data-ttu-id="d2df1-133">Následující část popisuje tento proces.</span><span class="sxs-lookup"><span data-stu-id="d2df1-133">The following section describes this process.</span></span>

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-akamai"></a><span data-ttu-id="d2df1-134">Optimalizace pro doručování velkých souborů s Azure Content Delivery Network společnosti Akamai</span><span class="sxs-lookup"><span data-stu-id="d2df1-134">Optimize for delivery of large files with the Azure Content Delivery Network from Akamai</span></span>

<span data-ttu-id="d2df1-135">Typ funkce optimalizace velkých souborů zapne optimalizace sítě a konfigurace k poskytování velkých souborů rychlejší a více responsively.</span><span class="sxs-lookup"><span data-stu-id="d2df1-135">The large file optimization type feature turns on network optimizations and configurations to deliver large files faster and more responsively.</span></span> <span data-ttu-id="d2df1-136">Obecné webové přenosy s Akamai pouze následující 1,8 GB soubory ukládá do mezipaměti, a můžete soubory tunelového propojení (ne mezipaměť) až 150 GB.</span><span class="sxs-lookup"><span data-stu-id="d2df1-136">General web delivery with Akamai caches files only below 1.8 GB and can tunnel (not cache) files up to 150 GB.</span></span> <span data-ttu-id="d2df1-137">Velkých souborů optimalizace mezipaměti souborů až do 150 GB.</span><span class="sxs-lookup"><span data-stu-id="d2df1-137">Large file optimization caches files up to 150 GB.</span></span>

<span data-ttu-id="d2df1-138">Optimalizace velkých souborů je účinný, pokud jsou splněny určité podmínky.</span><span class="sxs-lookup"><span data-stu-id="d2df1-138">Large file optimization is effective when certain conditions are satisfied.</span></span> <span data-ttu-id="d2df1-139">Podmínky zahrnují, jak funguje na zdrojový server a velikosti a typy souborů, které jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="d2df1-139">Conditions include how the origin server operates and the sizes and types of the files that are requested.</span></span> <span data-ttu-id="d2df1-140">Předtím, než se nám získat do podrobnosti o těchto tématech, byste měli porozumět, jak funguje optimalizace.</span><span class="sxs-lookup"><span data-stu-id="d2df1-140">Before we get into details on these subjects, you should understand how the optimization works.</span></span> 

### <a name="object-chunking"></a><span data-ttu-id="d2df1-141">Objekt rozdělování</span><span class="sxs-lookup"><span data-stu-id="d2df1-141">Object chunking</span></span> 

<span data-ttu-id="d2df1-142">Azure Content Delivery Network společnosti Akamai využívá techniku, názvem rozdělování objektu.</span><span class="sxs-lookup"><span data-stu-id="d2df1-142">The Azure Content Delivery Network from Akamai uses a technique called object chunking.</span></span> <span data-ttu-id="d2df1-143">Pokud se požaduje velký soubor, CDN načte menší části souboru z tohoto počátku.</span><span class="sxs-lookup"><span data-stu-id="d2df1-143">When a large file is requested, the CDN retrieves smaller pieces of the file from the origin.</span></span> <span data-ttu-id="d2df1-144">Jakmile serveru edge POP CDN obdrží žádost o úplné nebo rozsah bajtů souboru, zkontroluje, zda pro tato optimalizace je podporován typ souborů.</span><span class="sxs-lookup"><span data-stu-id="d2df1-144">After the CDN edge/POP server receives a full or byte-range file request, it checks whether the file type is supported for this optimization.</span></span> <span data-ttu-id="d2df1-145">Také zkontroluje, zda typ souboru splňuje požadavky na velikost souboru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-145">It also checks whether the file type meets the file size requirements.</span></span> <span data-ttu-id="d2df1-146">Pokud velikost souboru je větší než 10 MB, požaduje CDN edge server soubor z tohoto počátku v bloky 2 MB.</span><span class="sxs-lookup"><span data-stu-id="d2df1-146">If the file size is greater than 10 MB, the CDN edge server requests the file from the origin in chunks of 2 MB.</span></span> 

<span data-ttu-id="d2df1-147">Po bloku dat dorazí na hranici CDN, nemá v mezipaměti a okamžitě poskytovaného pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="d2df1-147">After the chunk arrives at the CDN edge, it's cached and immediately served to the user.</span></span> <span data-ttu-id="d2df1-148">CDN pak prefetches další blok paralelně.</span><span class="sxs-lookup"><span data-stu-id="d2df1-148">The CDN then prefetches the next chunk in parallel.</span></span> <span data-ttu-id="d2df1-149">Toto předběžné načtení zajistí, že obsah zůstane jeden blok před uživatele, což snižuje latence.</span><span class="sxs-lookup"><span data-stu-id="d2df1-149">This prefetch ensures that the content stays one chunk ahead of the user, which reduces latency.</span></span> <span data-ttu-id="d2df1-150">Tento proces pokračuje, dokud celý stažení souboru (je-li požadována), všechny rozsahů bajtů jsou k dispozici (je-li požadována), nebo klienta, ukončí připojení.</span><span class="sxs-lookup"><span data-stu-id="d2df1-150">This process continues until the entire file is downloaded (if requested), all byte ranges are available (if requested), or the client terminates the connection.</span></span> 

<span data-ttu-id="d2df1-151">Další informace o žádosti o rozsah bajtů, najdete v části [RFC 7233](https://tools.ietf.org/html/rfc7233).</span><span class="sxs-lookup"><span data-stu-id="d2df1-151">For more information on the byte-range request, see [RFC 7233](https://tools.ietf.org/html/rfc7233).</span></span>

<span data-ttu-id="d2df1-152">CDN ukládá do mezipaměti všechny bloky dat po přijetí.</span><span class="sxs-lookup"><span data-stu-id="d2df1-152">The CDN caches any chunks as they're received.</span></span> <span data-ttu-id="d2df1-153">Celý soubor nemá ukládat do mezipaměti v mezipaměti CDN.</span><span class="sxs-lookup"><span data-stu-id="d2df1-153">The entire file doesn't have to be cached on the CDN cache.</span></span> <span data-ttu-id="d2df1-154">Odeslání dalších žádostí o souboru nebo bajtů rozsahy se zpracovávají z mezipaměti CDN.</span><span class="sxs-lookup"><span data-stu-id="d2df1-154">Subsequent requests for the file or byte ranges are served from the CDN cache.</span></span> <span data-ttu-id="d2df1-155">Není-li všechny bloky dat jsou do mezipaměti na CDN, předběžné načtení slouží k vyžádání bloků dat z tohoto počátku.</span><span class="sxs-lookup"><span data-stu-id="d2df1-155">If not all the chunks are cached on the CDN, prefetch is used to request chunks from the origin.</span></span> <span data-ttu-id="d2df1-156">Tato optimalizace spoléhá na možnost na zdrojový server pro podporu požadavků rozsah bajtů.</span><span class="sxs-lookup"><span data-stu-id="d2df1-156">This optimization relies on the ability of the origin server to support byte-range requests.</span></span> <span data-ttu-id="d2df1-157">_Pokud je zdrojový server nepodporuje požadavky rozsah bajtů, optimalizace není platná._</span><span class="sxs-lookup"><span data-stu-id="d2df1-157">_If the origin server doesn't support byte-range requests, this optimization isn't effective._</span></span> 

### <a name="caching"></a><span data-ttu-id="d2df1-158">Ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d2df1-158">Caching</span></span>
<span data-ttu-id="d2df1-159">Optimalizace velkých souborů používá jiný výchozí doba ukládání do mezipaměti vypršení platnosti z obecné webové doručení.</span><span class="sxs-lookup"><span data-stu-id="d2df1-159">Large file optimization uses different default caching-expiration times from general web delivery.</span></span> <span data-ttu-id="d2df1-160">Rozlišuje ukládání do mezipaměti kladné a záporné ukládání do mezipaměti na základě kódů odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2df1-160">It differentiates between positive caching and negative caching based on HTTP response codes.</span></span> <span data-ttu-id="d2df1-161">Pokud je zdrojový server určuje čas vypršení platnosti prostřednictvím ovládacího prvku mezipaměti nebo vyprší platnost hlavičky v odpovědi, CDN ctí tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d2df1-161">If the origin server specifies an expiration time via a cache-control or expires header in the response, the CDN honors that value.</span></span> <span data-ttu-id="d2df1-162">Když soubor odpovídá podmínkám typ a velikost pro tento typ optimalizace počátek neuvádí, CDN použije výchozí hodnoty pro optimalizaci velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="d2df1-162">When the origin doesn’t specify and the file matches the type and size conditions for this optimization type, the CDN uses the default values for large file optimization.</span></span> <span data-ttu-id="d2df1-163">CDN, jinak používá výchozí hodnoty pro obecné webové doručení.</span><span class="sxs-lookup"><span data-stu-id="d2df1-163">Otherwise, the CDN uses defaults for general web delivery.</span></span>


|    | <span data-ttu-id="d2df1-164">Obecné web</span><span class="sxs-lookup"><span data-stu-id="d2df1-164">General web</span></span> | <span data-ttu-id="d2df1-165">Optimalizace velkých souborů</span><span class="sxs-lookup"><span data-stu-id="d2df1-165">Large file optimization</span></span> 
--- | --- | --- 
<span data-ttu-id="d2df1-166">Ukládání do mezipaměti: kladné</span><span class="sxs-lookup"><span data-stu-id="d2df1-166">Caching: Positive</span></span> <br> <span data-ttu-id="d2df1-167">HTTP 200, 203, 300,</span><span class="sxs-lookup"><span data-stu-id="d2df1-167">HTTP 200, 203, 300,</span></span> <br> <span data-ttu-id="d2df1-168">301, 302 a 410</span><span class="sxs-lookup"><span data-stu-id="d2df1-168">301, 302, and 410</span></span> | <span data-ttu-id="d2df1-169">7 dní</span><span class="sxs-lookup"><span data-stu-id="d2df1-169">7 days</span></span> |<span data-ttu-id="d2df1-170">1 den</span><span class="sxs-lookup"><span data-stu-id="d2df1-170">1 day</span></span>  
<span data-ttu-id="d2df1-171">Ukládání do mezipaměti: záporná</span><span class="sxs-lookup"><span data-stu-id="d2df1-171">Caching: Negative</span></span> <br> <span data-ttu-id="d2df1-172">HTTP 204, 305, 404,</span><span class="sxs-lookup"><span data-stu-id="d2df1-172">HTTP 204, 305, 404,</span></span> <br> <span data-ttu-id="d2df1-173">a 405</span><span class="sxs-lookup"><span data-stu-id="d2df1-173">and 405</span></span> | <span data-ttu-id="d2df1-174">Žádný</span><span class="sxs-lookup"><span data-stu-id="d2df1-174">None</span></span> | <span data-ttu-id="d2df1-175">1 sekunda</span><span class="sxs-lookup"><span data-stu-id="d2df1-175">1 second</span></span> 

### <a name="deal-with-origin-failure"></a><span data-ttu-id="d2df1-176">Řešení s chybou počátek</span><span class="sxs-lookup"><span data-stu-id="d2df1-176">Deal with origin failure</span></span>

<span data-ttu-id="d2df1-177">Délka časového limitu pro čtení počátek zvyšuje ze dvou sekund k doručení obecné webové do dvou minut pro typ optimalizace velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="d2df1-177">The origin read-timeout length increases from two seconds for general web delivery to two minutes for the large file optimization type.</span></span> <span data-ttu-id="d2df1-178">Toto zvýšení účty pro velkých souborů, aby se zabránilo předčasné vypršení časového limitu připojení.</span><span class="sxs-lookup"><span data-stu-id="d2df1-178">This increase accounts for the larger file sizes to avoid a premature timeout connection.</span></span>

<span data-ttu-id="d2df1-179">Pokud vyprší časový limit připojení, CDN opakuje několikrát, než odešle "504 – časový limit brány" Chyba do klienta.</span><span class="sxs-lookup"><span data-stu-id="d2df1-179">When a connection times out, the CDN retries a number of times before it sends a "504 - Gateway Timeout" error to the client.</span></span> 

### <a name="conditions-for-large-file-optimization"></a><span data-ttu-id="d2df1-180">Podmínky pro optimalizaci velkých souborů</span><span class="sxs-lookup"><span data-stu-id="d2df1-180">Conditions for large file optimization</span></span>

<span data-ttu-id="d2df1-181">Následující tabulka uvádí sadu kritérií, které je třeba splnit pro optimalizaci velkých souborů:</span><span class="sxs-lookup"><span data-stu-id="d2df1-181">The following table lists the set of criteria to be satisfied for large file optimization:</span></span>

<span data-ttu-id="d2df1-182">Podmínka</span><span class="sxs-lookup"><span data-stu-id="d2df1-182">Condition</span></span> | <span data-ttu-id="d2df1-183">Hodnoty</span><span class="sxs-lookup"><span data-stu-id="d2df1-183">Values</span></span> 
--- | --- 
<span data-ttu-id="d2df1-184">Podporované typy souborů</span><span class="sxs-lookup"><span data-stu-id="d2df1-184">Supported file types</span></span> | <span data-ttu-id="d2df1-185">3g, 2, 3gp, amp, avi, bz2, dmg, exe, f4v, flv,</span><span class="sxs-lookup"><span data-stu-id="d2df1-185">3g2, 3gp, asf, avi, bz2, dmg, exe, f4v, flv,</span></span> <br> <span data-ttu-id="d2df1-186">GZ, hdp, iso, jxr, m4v, mkv, mov, mp4,</span><span class="sxs-lookup"><span data-stu-id="d2df1-186">gz, hdp, iso, jxr, m4v, mkv, mov, mp4,</span></span> <br> <span data-ttu-id="d2df1-187">MPEG, mpg, serveru mts, pkg, RT, rm, swf, vkládání,</span><span class="sxs-lookup"><span data-stu-id="d2df1-187">mpeg, mpg, mts, pkg, qt, rm, swf, tar,</span></span> <br> <span data-ttu-id="d2df1-188">TGZ, wdp, WBEM, webp, wma, wmv, zip</span><span class="sxs-lookup"><span data-stu-id="d2df1-188">tgz, wdp, webm, webp, wma, wmv, zip</span></span>  
<span data-ttu-id="d2df1-189">Minimální velikost souboru</span><span class="sxs-lookup"><span data-stu-id="d2df1-189">Minimum file size</span></span> | <span data-ttu-id="d2df1-190">10 MB.</span><span class="sxs-lookup"><span data-stu-id="d2df1-190">10 MB</span></span> 
<span data-ttu-id="d2df1-191">Maximální velikost souboru</span><span class="sxs-lookup"><span data-stu-id="d2df1-191">Maximum file size</span></span> | <span data-ttu-id="d2df1-192">150 GB</span><span class="sxs-lookup"><span data-stu-id="d2df1-192">150 GB</span></span> 
<span data-ttu-id="d2df1-193">Vlastnosti serveru počátek</span><span class="sxs-lookup"><span data-stu-id="d2df1-193">Origin server characteristics</span></span> | <span data-ttu-id="d2df1-194">Žádosti o rozsah bajtů musí podporovat.</span><span class="sxs-lookup"><span data-stu-id="d2df1-194">Must support byte-range requests</span></span> 

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-verizon"></a><span data-ttu-id="d2df1-195">Optimalizace pro doručování velkých souborů s Azure Content Delivery Network od společnosti Verizon</span><span class="sxs-lookup"><span data-stu-id="d2df1-195">Optimize for delivery of large files with the Azure Content Delivery Network from Verizon</span></span>

<span data-ttu-id="d2df1-196">Azure Content Delivery Network od společnosti Verizon nabízí velké soubory bez cap na velikost souboru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-196">The Azure Content Delivery Network from Verizon delivers large files without a cap on file size.</span></span> <span data-ttu-id="d2df1-197">Další funkce jsou zapnuté ve výchozím nastavení rychleji doručování velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="d2df1-197">Additional features are turned on by default to make delivery of large files faster.</span></span>

### <a name="complete-cache-fill"></a><span data-ttu-id="d2df1-198">Dokončení mezipaměti výplně</span><span class="sxs-lookup"><span data-stu-id="d2df1-198">Complete cache fill</span></span>

<span data-ttu-id="d2df1-199">Funkce Výchozí dokončení mezipaměti výplně umožňuje od CDN k vyžádání obsahu souboru do mezipaměti, při opuštění nebo ke ztrátě počáteční požadavku.</span><span class="sxs-lookup"><span data-stu-id="d2df1-199">The default complete cache fill feature enables the CDN to pull a file into the cache when an initial request is abandoned or lost.</span></span> 

<span data-ttu-id="d2df1-200">Výplně dokončení mezipaměti je nejvhodnější pro velké prostředky.</span><span class="sxs-lookup"><span data-stu-id="d2df1-200">Complete cache fill is most useful for large assets.</span></span> <span data-ttu-id="d2df1-201">Většinou uživatelé je nemusíte stahovat od začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="d2df1-201">Typically, users don't download them from start to finish.</span></span> <span data-ttu-id="d2df1-202">Používají progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="d2df1-202">They use progressive download.</span></span> <span data-ttu-id="d2df1-203">Výchozí chování vynutí hraničního serveru k zahájení načítání na pozadí prostředku ze zdrojového serveru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-203">The default behavior forces the edge server to initiate a background fetch of the asset from the origin server.</span></span> <span data-ttu-id="d2df1-204">Potom prostředku je v hraniční server místní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d2df1-204">Afterward, the asset is in the edge server's local cache.</span></span> <span data-ttu-id="d2df1-205">Po úplné objekt je v mezipaměti, edge server plnit požadavky rozsah bajtů k CDN pro objekt uložená v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d2df1-205">After the full object is in the cache, the edge server fulfills byte-range requests to the CDN for the cached object.</span></span>

<span data-ttu-id="d2df1-206">Výchozí chování lze zakázat pomocí stroj pravidel ve vrstvě Verizon Premium.</span><span class="sxs-lookup"><span data-stu-id="d2df1-206">The default behavior can be disabled through the Rules Engine in the Verizon Premium tier.</span></span>

### <a name="peer-cache-fill-hot-filing"></a><span data-ttu-id="d2df1-207">Sdílená mezipaměť vyplnění horkou podání</span><span class="sxs-lookup"><span data-stu-id="d2df1-207">Peer cache fill hot-filing</span></span>

<span data-ttu-id="d2df1-208">Funkce výchozích sdílené mezipaměti výplně horkou podání používá sofistikované vlastního algoritmu.</span><span class="sxs-lookup"><span data-stu-id="d2df1-208">The default peer cache fill hot-filing feature uses a sophisticated proprietary algorithm.</span></span> <span data-ttu-id="d2df1-209">Používá další hraniční ukládání do mezipaměti servery založené na šířku pásma a agregace požadavků metriky, aby vyplnila požadavky klienta pro velkých, vysoce oblíbených objekty.</span><span class="sxs-lookup"><span data-stu-id="d2df1-209">It uses additional edge caching servers based on bandwidth and aggregate requests metrics to fulfill client requests for large, highly popular objects.</span></span> <span data-ttu-id="d2df1-210">Tato funkce zabraňuje situaci, v níž jsou odesílány velký počet navíc požadavků uživatele zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="d2df1-210">This feature prevents a situation in which large numbers of extra requests are sent to a user's origin server.</span></span> 

### <a name="conditions-for-large-file-optimization"></a><span data-ttu-id="d2df1-211">Podmínky pro optimalizaci velkých souborů</span><span class="sxs-lookup"><span data-stu-id="d2df1-211">Conditions for large file optimization</span></span>

<span data-ttu-id="d2df1-212">Ve výchozím nastavení jsou zapnuté funkce optimalizace pro Verizon.</span><span class="sxs-lookup"><span data-stu-id="d2df1-212">The optimization features for Verizon are turned on by default.</span></span> <span data-ttu-id="d2df1-213">Neexistují žádná omezení na maximální velikost souboru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-213">There are no limits on maximum file size.</span></span> 

## <a name="additional-considerations"></a><span data-ttu-id="d2df1-214">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="d2df1-214">Additional considerations</span></span>

<span data-ttu-id="d2df1-215">Vezměte v úvahu následující další aspekty pro tento typ optimalizace.</span><span class="sxs-lookup"><span data-stu-id="d2df1-215">Consider the following additional aspects for this optimization type.</span></span>
 
### <a name="azure-content-delivery-network-from-akamai"></a><span data-ttu-id="d2df1-216">Síť pro doručování obsahu Azure společnosti Akamai</span><span class="sxs-lookup"><span data-stu-id="d2df1-216">Azure Content Delivery Network from Akamai</span></span>

- <span data-ttu-id="d2df1-217">Proces bloku dat vygeneruje další požadavky na zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="d2df1-217">The chunking process generates additional requests to the origin server.</span></span> <span data-ttu-id="d2df1-218">Celkový objem dat doručit od počátku je však mnohem menší.</span><span class="sxs-lookup"><span data-stu-id="d2df1-218">However, the overall volume of data delivered from the origin is much smaller.</span></span> <span data-ttu-id="d2df1-219">Bloku dat za následek lepší ukládání do mezipaměti charakteristiky v CDN.</span><span class="sxs-lookup"><span data-stu-id="d2df1-219">Chunking results in better caching characteristics at the CDN.</span></span>

- <span data-ttu-id="d2df1-220">Paměť a vstupně-výstupních operací přetížení jsou omezeny na počátku vzhledem k tomu, že jsou doručovány na menší části souboru.</span><span class="sxs-lookup"><span data-stu-id="d2df1-220">Memory and I/O pressure are reduced at the origin because smaller pieces of the file are delivered.</span></span>

- <span data-ttu-id="d2df1-221">U bloky dat uložené v mezipaměti v CDN neexistují žádné další požadavky na počátek až vyprší platnost obsahu nebo je vyřazena z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d2df1-221">For chunks cached at the CDN, there are no additional requests to the origin until the content expires or it's evicted from the cache.</span></span>

- <span data-ttu-id="d2df1-222">Mohou uživatelé provádět požadavky na rozsah k CDN a jste zacházet jako jakýkoli normální soubor.</span><span class="sxs-lookup"><span data-stu-id="d2df1-222">Users can make range requests to the CDN, and they're treated like any normal file.</span></span> <span data-ttu-id="d2df1-223">Optimalizace platí jenom v případě, že je platný typ souboru a rozsah bajtů je mezi 10 MB a 150 GB.</span><span class="sxs-lookup"><span data-stu-id="d2df1-223">Optimization applies only if it's a valid file type and the byte range is between 10 MB and 150 GB.</span></span> <span data-ttu-id="d2df1-224">Pokud je požadovaná velikost průměrná soubor je menší než 10 MB, můžete místo toho použijte obecné webové doručení.</span><span class="sxs-lookup"><span data-stu-id="d2df1-224">If the average file size requested is smaller than 10 MB, you might want to use general web delivery instead.</span></span>

### <a name="azure-content-delivery-network-from-verizon"></a><span data-ttu-id="d2df1-225">Doručování obsahu Azure sítě od společnosti Verizon</span><span class="sxs-lookup"><span data-stu-id="d2df1-225">Azure Content Delivery Network from Verizon</span></span>

<span data-ttu-id="d2df1-226">Daný typ doručení optimalizace obecné webové doručovat velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="d2df1-226">The general web delivery optimization type can deliver large files.</span></span>