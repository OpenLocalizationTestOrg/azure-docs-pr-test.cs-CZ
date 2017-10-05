---
title: "Zvýšení výkonu komprimací souborů v Azure CDN | Microsoft Docs"
description: "Naučte se zlepšit rychlost přenosu souborů a zvyšuje zatížení stránky komprimací souborů v Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7546650e6096a880f4fb4d0c94dd4ecc00b70160
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a><span data-ttu-id="a2d80-103">Zvýšení výkonu komprimací souborů v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="a2d80-103">Improve performance by compressing files in Azure CDN</span></span>
<span data-ttu-id="a2d80-104">Komprese je jednoduchá ale účinná metoda a zvýšit rychlost přenosu souborů zvyšuje zatížení stránky omezení velikosti souboru před odesláním ze serveru.</span><span class="sxs-lookup"><span data-stu-id="a2d80-104">Compression is a simple and effective method to improve file transfer speed and increase page load performance by reducing file size before it is sent from the server.</span></span> <span data-ttu-id="a2d80-105">Umožňuje snížit náklady na šířku pásma a poskytuje rychlejší reakce prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="a2d80-105">It reduces bandwidth costs and provides a more responsive experience for your users.</span></span>

<span data-ttu-id="a2d80-106">Existují dva způsoby, jak povolit kompresi:</span><span class="sxs-lookup"><span data-stu-id="a2d80-106">There are two ways to enable compression:</span></span>

* <span data-ttu-id="a2d80-107">Můžete povolit kompresi na serveru původu, ve kterém případ CDN projdou komprimované soubory a poskytovat komprimovaných souborů na klienty, kteří požadují je.</span><span class="sxs-lookup"><span data-stu-id="a2d80-107">You can enable compression on your origin server, in which case the CDN passes through the compressed files and deliver compressed files to clients that request them.</span></span>
* <span data-ttu-id="a2d80-108">Můžete povolit kompresi přímo na servery edge CDN, ve kterých případ CDN komprimaci soubory a poskytovat koncovým uživatelům, i v případě, že nejsou komprimované serverem původu.</span><span class="sxs-lookup"><span data-stu-id="a2d80-108">You can enable compression directly on CDN edge servers, in which case the CDN compresses the files and serve them to end users, even if they are not compressed by the origin server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2d80-109">Změny konfigurace CDN trvat nějakou dobu šířit přes síť.</span><span class="sxs-lookup"><span data-stu-id="a2d80-109">CDN configuration changes take some time to propagate through the network.</span></span>  <span data-ttu-id="a2d80-110">Pro <b>Azure CDN společnosti Akamai</b> profily, šíření obvykle dokončení v části jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="a2d80-110">For <b>Azure CDN from Akamai</b> profiles, propagation usually completes in under one minute.</span></span>  <span data-ttu-id="a2d80-111">Pro <b>Azure CDN společnosti Verizon</b> profily, obvykle uvidíte změny použít během 90 minut.</span><span class="sxs-lookup"><span data-stu-id="a2d80-111">For <b>Azure CDN from Verizon</b> profiles, you will usually see your changes apply within 90 minutes.</span></span>  <span data-ttu-id="a2d80-112">Pokud je to poprvé, které jste nastavili komprese pro koncový bod CDN, měli byste zvážit čekání 1 – 2 hodiny jistotu komprese, které se mají nastavení rozšíří do bodů POP před řešení potíží s</span><span class="sxs-lookup"><span data-stu-id="a2d80-112">If this is the first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours to be sure the compression settings have propagated to the POPs before troubleshooting</span></span>
> 
> 

## <a name="enabling-compression"></a><span data-ttu-id="a2d80-113">Povolení komprese</span><span class="sxs-lookup"><span data-stu-id="a2d80-113">Enabling compression</span></span>
> [!NOTE]
> <span data-ttu-id="a2d80-114">Úrovních Standard a Premium CDN nabízí stejnou funkčnost kompresi, ale uživatelské rozhraní se liší.</span><span class="sxs-lookup"><span data-stu-id="a2d80-114">The Standard and Premium CDN tiers provide the same compression functionality, but the user interface differs.</span></span>  <span data-ttu-id="a2d80-115">Další informace o rozdílech mezi úrovních Standard a Premium CDN najdete v tématu [přehled CDN Azure](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2d80-115">For more information about the differences between Standard and Premium CDN tiers, see [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

### <a name="standard-tier"></a><span data-ttu-id="a2d80-116">Úroveň Standard</span><span class="sxs-lookup"><span data-stu-id="a2d80-116">Standard tier</span></span>
> [!NOTE]
> <span data-ttu-id="a2d80-117">Tato část se týká **Azure CDN Standard od společnosti Verizon** a **Azure CDN Standard od společnosti Akamai** profily.</span><span class="sxs-lookup"><span data-stu-id="a2d80-117">This section applies to **Azure CDN Standard from Verizon** and **Azure CDN Standard from Akamai** profiles.</span></span>
> 
> 

1. <span data-ttu-id="a2d80-118">Na stránce profil CDN klikněte na koncový bod CDN, které chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="a2d80-118">From the CDN profile page, click the CDN endpoint you wish to manage.</span></span>
   
    ![Koncové body stránky profilu CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    <span data-ttu-id="a2d80-120">Otevře se stránka koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="a2d80-120">The CDN endpoint page opens.</span></span>
2. <span data-ttu-id="a2d80-121">Klikněte **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a2d80-121">Click the **Configure** button.</span></span>
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-config-btn.png)
   
    <span data-ttu-id="a2d80-123">Otevře se stránka konfigurace CDN.</span><span class="sxs-lookup"><span data-stu-id="a2d80-123">The CDN Configuration page opens.</span></span>
3. <span data-ttu-id="a2d80-124">Zapnout **komprese**.</span><span class="sxs-lookup"><span data-stu-id="a2d80-124">Turn on **Compression**.</span></span>
   
    ![Možnosti komprese CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. <span data-ttu-id="a2d80-126">Použijte výchozí typy nebo upravte seznam, odebrání nebo přidáním typy souborů.</span><span class="sxs-lookup"><span data-stu-id="a2d80-126">Use the default types, or modify the list by removing or adding file types.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="a2d80-127">Chvíli to možné, není doporučeno použít komprimaci pro komprimovaný formáty, například ZIP, MP3, MP4, JPG, atd.</span><span class="sxs-lookup"><span data-stu-id="a2d80-127">While possible, it is not recommended to apply compression to compressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span>
   > 
   > 
5. <span data-ttu-id="a2d80-128">Po provedení změny, klikněte na **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a2d80-128">After making your changes, click the **Save** button.</span></span>

### <a name="premium-tier"></a><span data-ttu-id="a2d80-129">Úroveň Premium</span><span class="sxs-lookup"><span data-stu-id="a2d80-129">Premium tier</span></span>
> [!NOTE]
> <span data-ttu-id="a2d80-130">Tato část se týká **Azure CDN Premium od společnosti Verizon** profily.</span><span class="sxs-lookup"><span data-stu-id="a2d80-130">This section applies to **Azure CDN Premium from Verizon** profiles.</span></span>
> 
> 

1. <span data-ttu-id="a2d80-131">Na stránce profil CDN, klikněte **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a2d80-131">From the CDN profile page, click the **Manage** button.</span></span>
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    <span data-ttu-id="a2d80-133">Otevře se na portálu pro správu CDN.</span><span class="sxs-lookup"><span data-stu-id="a2d80-133">The CDN management portal opens.</span></span>
2. <span data-ttu-id="a2d80-134">Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="a2d80-134">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="a2d80-135">Klikněte na **komprese**.</span><span class="sxs-lookup"><span data-stu-id="a2d80-135">Click on **Compression**.</span></span>

    ![Výběr komprese souboru](./media/cdn-file-compression/cdn-compress-select.png)
   
    <span data-ttu-id="a2d80-137">Zobrazí se možnosti komprese.</span><span class="sxs-lookup"><span data-stu-id="a2d80-137">Compression options are displayed.</span></span>
   
    ![Kompresí souborů](./media/cdn-file-compression/cdn-compress-files.png)
3. <span data-ttu-id="a2d80-139">Povolit kompresi kliknutím **povolená komprese** přepínač.</span><span class="sxs-lookup"><span data-stu-id="a2d80-139">Enable compression by clicking the **Compression Enabled** radio button.</span></span>  <span data-ttu-id="a2d80-140">Zadejte typy MIME, který chcete komprimovat jako seznam s položkami oddělenými čárkou (bez mezer) **typy souborů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a2d80-140">Enter the MIME types you wish to compress as a comma-delimited list (no spaces) in the **File Types** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="a2d80-141">Chvíli to možné, není doporučeno použít komprimaci pro komprimovaný formáty, například ZIP, MP3, MP4, JPG, atd.</span><span class="sxs-lookup"><span data-stu-id="a2d80-141">While possible, it is not recommended to apply compression to compressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span> 
   > 
   > 
4. <span data-ttu-id="a2d80-142">Po provedení změny, klikněte na **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a2d80-142">After making your changes, click the **Update** button.</span></span>

## <a name="compression-rules"></a><span data-ttu-id="a2d80-143">Komprese pravidla</span><span class="sxs-lookup"><span data-stu-id="a2d80-143">Compression rules</span></span>
<span data-ttu-id="a2d80-144">Tyto tabulky popisují chování Azure CDN komprese pro každý scénář.</span><span class="sxs-lookup"><span data-stu-id="a2d80-144">These tables describe Azure CDN compression behavior for every scenario.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2d80-145">Pro **Azure CDN společnosti Verizon** (Standard a Premium), jenom vhodné soubory jsou komprimovány.</span><span class="sxs-lookup"><span data-stu-id="a2d80-145">For **Azure CDN from Verizon** (Standard and Premium), only eligible files are compressed.</span></span>  <span data-ttu-id="a2d80-146">Způsobilé k komprese, musíte do souboru:</span><span class="sxs-lookup"><span data-stu-id="a2d80-146">To be eligible for compression, a file must:</span></span>
> 
> * <span data-ttu-id="a2d80-147">Být větší než 128 bajtů.</span><span class="sxs-lookup"><span data-stu-id="a2d80-147">Be larger than 128 bytes.</span></span>
> * <span data-ttu-id="a2d80-148">Být menší než 1 MB.</span><span class="sxs-lookup"><span data-stu-id="a2d80-148">Be smaller than 1 MB.</span></span>
> 
> <span data-ttu-id="a2d80-149">Pro **Azure CDN společnosti Akamai**, všechny soubory, které jsou způsobilé pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="a2d80-149">For **Azure CDN from Akamai**, all files are eligible for compression.</span></span>
> 
> <span data-ttu-id="a2d80-150">Pro všechny produkty Azure CDN, soubor musí být typ MIME, který byl [nakonfigurované pro kompresi](#enabling-compression).</span><span class="sxs-lookup"><span data-stu-id="a2d80-150">For all Azure CDN products, a file must be a MIME type that has been [configured for compression](#enabling-compression).</span></span>
> 
> <span data-ttu-id="a2d80-151">**Azure CDN společnosti Verizon** profilů (Standard a Premium) podporu **gzip** (GNU zip), **deflate**, **bzip2**, nebo **Brazílie**Kódování (Brotli).</span><span class="sxs-lookup"><span data-stu-id="a2d80-151">**Azure CDN from Verizon** profiles (Standard and Premium) support **gzip** (GNU zip), **deflate**, **bzip2**, or  **br** (Brotli) encoding.</span></span> <span data-ttu-id="a2d80-152">Pro Brotli kódování, komprese provádí pouze na hranici.</span><span class="sxs-lookup"><span data-stu-id="a2d80-152">For Brotli encoding, the compression is done only at the edge.</span></span> <span data-ttu-id="a2d80-153">V klientském prohlížeči musíte odeslat žádost o Brotli kódování a komprimované asset musí mít komprimované na straně původu nejdřív.</span><span class="sxs-lookup"><span data-stu-id="a2d80-153">The client/browser must send the request for Brotli encoding and the compressed asset must have been compressed on the origin side first.</span></span> 
>
><span data-ttu-id="a2d80-154">**Azure CDN společnosti Akamai** profily podporují pouze **gzip** kódování.</span><span class="sxs-lookup"><span data-stu-id="a2d80-154">**Azure CDN from Akamai** profiles support only **gzip** encoding.</span></span>
> 
> <span data-ttu-id="a2d80-155">**Azure CDN společnosti Akamai** koncových bodů se vždy požadavku **gzip** kódovaný soubory z tohoto počátku, a to bez ohledu na žádost klienta.</span><span class="sxs-lookup"><span data-stu-id="a2d80-155">**Azure CDN from Akamai** endpoints always request **gzip** encoded files from the origin, regardless of the client request.</span></span> 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a><span data-ttu-id="a2d80-156">Komprese vypnuta nebo souboru je není vhodná pro kompresi</span><span class="sxs-lookup"><span data-stu-id="a2d80-156">Compression disabled or file is ineligible for compression</span></span>
| <span data-ttu-id="a2d80-157">Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="a2d80-157">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="a2d80-158">Formát souborů uložených v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a2d80-158">Cached file format</span></span> | <span data-ttu-id="a2d80-159">CDN odpověď klientovi</span><span class="sxs-lookup"><span data-stu-id="a2d80-159">CDN response to the client</span></span> | <span data-ttu-id="a2d80-160">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a2d80-160">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a2d80-161">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-161">Compressed</span></span> |<span data-ttu-id="a2d80-162">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-162">Compressed</span></span> |<span data-ttu-id="a2d80-163">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-163">Compressed</span></span> | |
| <span data-ttu-id="a2d80-164">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-164">Compressed</span></span> |<span data-ttu-id="a2d80-165">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-165">Uncompressed</span></span> |<span data-ttu-id="a2d80-166">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-166">Uncompressed</span></span> | |
| <span data-ttu-id="a2d80-167">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-167">Compressed</span></span> |<span data-ttu-id="a2d80-168">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a2d80-168">Not cached</span></span> |<span data-ttu-id="a2d80-169">Komprimované nebo nekomprimovaným</span><span class="sxs-lookup"><span data-stu-id="a2d80-169">Compressed or Uncompressed</span></span> |<span data-ttu-id="a2d80-170">Závisí na počátku odpovědi</span><span class="sxs-lookup"><span data-stu-id="a2d80-170">Depends on origin response</span></span> |
| <span data-ttu-id="a2d80-171">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-171">Uncompressed</span></span> |<span data-ttu-id="a2d80-172">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-172">Compressed</span></span> |<span data-ttu-id="a2d80-173">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-173">Uncompressed</span></span> | |
| <span data-ttu-id="a2d80-174">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-174">Uncompressed</span></span> |<span data-ttu-id="a2d80-175">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-175">Uncompressed</span></span> |<span data-ttu-id="a2d80-176">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-176">Uncompressed</span></span> | |
| <span data-ttu-id="a2d80-177">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-177">Uncompressed</span></span> |<span data-ttu-id="a2d80-178">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a2d80-178">Not cached</span></span> |<span data-ttu-id="a2d80-179">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-179">Uncompressed</span></span> | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a><span data-ttu-id="a2d80-180">Komprese zapnuta a souboru je vhodné pro kompresi</span><span class="sxs-lookup"><span data-stu-id="a2d80-180">Compression enabled and file is eligible for compression</span></span>
| <span data-ttu-id="a2d80-181">Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="a2d80-181">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="a2d80-182">Formát souborů uložených v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a2d80-182">Cached file format</span></span> | <span data-ttu-id="a2d80-183">CDN odpověď klientovi</span><span class="sxs-lookup"><span data-stu-id="a2d80-183">CDN response to the client</span></span> | <span data-ttu-id="a2d80-184">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a2d80-184">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a2d80-185">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-185">Compressed</span></span> |<span data-ttu-id="a2d80-186">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-186">Compressed</span></span> |<span data-ttu-id="a2d80-187">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-187">Compressed</span></span> |<span data-ttu-id="a2d80-188">CDN transcodes mezi podporované formáty</span><span class="sxs-lookup"><span data-stu-id="a2d80-188">CDN transcodes between supported formats</span></span> |
| <span data-ttu-id="a2d80-189">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-189">Compressed</span></span> |<span data-ttu-id="a2d80-190">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-190">Uncompressed</span></span> |<span data-ttu-id="a2d80-191">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-191">Compressed</span></span> |<span data-ttu-id="a2d80-192">CDN provede kompresi</span><span class="sxs-lookup"><span data-stu-id="a2d80-192">CDN performs compression</span></span> |
| <span data-ttu-id="a2d80-193">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-193">Compressed</span></span> |<span data-ttu-id="a2d80-194">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a2d80-194">Not cached</span></span> |<span data-ttu-id="a2d80-195">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-195">Compressed</span></span> |<span data-ttu-id="a2d80-196">CDN provede kompresi, pokud zdroj vrátí nekomprimované.</span><span class="sxs-lookup"><span data-stu-id="a2d80-196">CDN performs compression if origin returns uncompressed.</span></span>  <span data-ttu-id="a2d80-197">**Azure CDN společnosti Verizon** předá nekomprimovaného souboru na první požadavek a pak komprimuje a ukládá do mezipaměti souborů pro následné požadavky.</span><span class="sxs-lookup"><span data-stu-id="a2d80-197">**Azure CDN from Verizon** passes the uncompressed file on the first request and then compresses and caches the file for subsequent requests.</span></span>  <span data-ttu-id="a2d80-198">Soubory s `Cache-Control: no-cache` záhlaví nikdy komprimuje.</span><span class="sxs-lookup"><span data-stu-id="a2d80-198">Files with `Cache-Control: no-cache` header will never be compressed.</span></span> |
| <span data-ttu-id="a2d80-199">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-199">Uncompressed</span></span> |<span data-ttu-id="a2d80-200">Komprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-200">Compressed</span></span> |<span data-ttu-id="a2d80-201">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-201">Uncompressed</span></span> |<span data-ttu-id="a2d80-202">Při dekompresi provede CDN</span><span class="sxs-lookup"><span data-stu-id="a2d80-202">CDN performs decompression</span></span> |
| <span data-ttu-id="a2d80-203">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-203">Uncompressed</span></span> |<span data-ttu-id="a2d80-204">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-204">Uncompressed</span></span> |<span data-ttu-id="a2d80-205">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-205">Uncompressed</span></span> | |
| <span data-ttu-id="a2d80-206">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-206">Uncompressed</span></span> |<span data-ttu-id="a2d80-207">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a2d80-207">Not cached</span></span> |<span data-ttu-id="a2d80-208">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="a2d80-208">Uncompressed</span></span> | |

## <a name="media-services-cdn-compression"></a><span data-ttu-id="a2d80-209">Komprese CDN služby Media Services</span><span class="sxs-lookup"><span data-stu-id="a2d80-209">Media Services CDN Compression</span></span>
<span data-ttu-id="a2d80-210">Pro Media Services CDN povoleno koncových bodů streamování, je komprese povolena ve výchozím nastavení pro následující typy obsahu: aplikace nebo vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, aplikace nebo f4m + xml.</span><span class="sxs-lookup"><span data-stu-id="a2d80-210">For Media Services CDN enabled streaming endpoints, compression is enabled by default for the following content types: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl, application/f4m+xml.</span></span> <span data-ttu-id="a2d80-211">Můžete nelze povolit nebo zakázat komprese uvedených typů pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a2d80-211">You cannot enable/disable compression for the mentioned types using the Azure portal.</span></span>  

## <a name="see-also"></a><span data-ttu-id="a2d80-212">Viz také</span><span class="sxs-lookup"><span data-stu-id="a2d80-212">See also</span></span>
* [<span data-ttu-id="a2d80-213">Řešení potíží s kompresí souborů v síti CDN</span><span class="sxs-lookup"><span data-stu-id="a2d80-213">Troubleshooting CDN file compression</span></span>](cdn-troubleshoot-compression.md)    

