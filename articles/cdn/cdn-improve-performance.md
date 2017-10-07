---
title: "aaaImprove výkonu komprimací souborů v Azure CDN | Microsoft Docs"
description: "Zjistěte, jak přenos souborů tooimprove rychlost a zvyšuje zatížení stránky komprimací souborů v Azure CDN."
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
ms.openlocfilehash: 9a1698b6c29233c2e2e6fb17cdd8e919ae8bfa1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a><span data-ttu-id="f663e-103">Zvýšení výkonu komprimací souborů v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f663e-103">Improve performance by compressing files in Azure CDN</span></span>
<span data-ttu-id="f663e-104">Komprese je jednoduchá ale účinná metoda tooimprove souboru rychlost přenosu dat na a zvýšení výkonu zatížení stránky snížením velikosti souboru dříve, než je odeslána ze serveru hello.</span><span class="sxs-lookup"><span data-stu-id="f663e-104">Compression is a simple and effective method tooimprove file transfer speed and increase page load performance by reducing file size before it is sent from hello server.</span></span> <span data-ttu-id="f663e-105">Umožňuje snížit náklady na šířku pásma a poskytuje rychlejší reakce prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="f663e-105">It reduces bandwidth costs and provides a more responsive experience for your users.</span></span>

<span data-ttu-id="f663e-106">Existují dva způsoby tooenable komprese:</span><span class="sxs-lookup"><span data-stu-id="f663e-106">There are two ways tooenable compression:</span></span>

* <span data-ttu-id="f663e-107">Můžete povolit kompresi na původním serveru, v takovém případě hello předává CDN prostřednictvím hello komprimované soubory a poskytovat tooclients komprimované soubory, které o ně požádat.</span><span class="sxs-lookup"><span data-stu-id="f663e-107">You can enable compression on your origin server, in which case hello CDN passes through hello compressed files and deliver compressed files tooclients that request them.</span></span>
* <span data-ttu-id="f663e-108">Můžete povolit kompresi přímo na CDN hraniční servery, ve kterých případu hello CDN zkomprimuje hello soubory a je i v případě, že nejsou komprimované serverem počátek hello sloužit tooend uživatele.</span><span class="sxs-lookup"><span data-stu-id="f663e-108">You can enable compression directly on CDN edge servers, in which case hello CDN compresses hello files and serve them tooend users, even if they are not compressed by hello origin server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f663e-109">Změny konfigurace CDN trvat některé čas toopropagate přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="f663e-109">CDN configuration changes take some time toopropagate through hello network.</span></span>  <span data-ttu-id="f663e-110">Pro <b>Azure CDN společnosti Akamai</b> profily, šíření obvykle dokončení v části jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="f663e-110">For <b>Azure CDN from Akamai</b> profiles, propagation usually completes in under one minute.</span></span>  <span data-ttu-id="f663e-111">Pro <b>Azure CDN společnosti Verizon</b> profily, obvykle uvidíte změny použít během 90 minut.</span><span class="sxs-lookup"><span data-stu-id="f663e-111">For <b>Azure CDN from Verizon</b> profiles, you will usually see your changes apply within 90 minutes.</span></span>  <span data-ttu-id="f663e-112">Pokud je to hello poprvé, co jste nastavili komprese pro koncový bod CDN, měli byste zvážit čeká se, že rozšíření nastavení komprese hello bodů POP toohello před řešení potíží 1 – 2 hodiny toobe</span><span class="sxs-lookup"><span data-stu-id="f663e-112">If this is hello first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours toobe sure hello compression settings have propagated toohello POPs before troubleshooting</span></span>
> 
> 

## <a name="enabling-compression"></a><span data-ttu-id="f663e-113">Povolení komprese</span><span class="sxs-lookup"><span data-stu-id="f663e-113">Enabling compression</span></span>
> [!NOTE]
> <span data-ttu-id="f663e-114">Zadejte technologie Hello úrovních Standard a Premium CDN hello stejné funkce komprese, ale hello uživatelské rozhraní se liší.</span><span class="sxs-lookup"><span data-stu-id="f663e-114">hello Standard and Premium CDN tiers provide hello same compression functionality, but hello user interface differs.</span></span>  <span data-ttu-id="f663e-115">Další informace o hello rozdíly mezi úrovních Standard a Premium CDN najdete v tématu [přehled CDN Azure](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f663e-115">For more information about hello differences between Standard and Premium CDN tiers, see [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

### <a name="standard-tier"></a><span data-ttu-id="f663e-116">Úroveň Standard</span><span class="sxs-lookup"><span data-stu-id="f663e-116">Standard tier</span></span>
> [!NOTE]
> <span data-ttu-id="f663e-117">Tato část se týká příliš**Azure CDN Standard od společnosti Verizon** a **Azure CDN Standard od společnosti Akamai** profily.</span><span class="sxs-lookup"><span data-stu-id="f663e-117">This section applies too**Azure CDN Standard from Verizon** and **Azure CDN Standard from Akamai** profiles.</span></span>
> 
> 

1. <span data-ttu-id="f663e-118">Na stránce profil CDN hello klikněte na koncový bod CDN hello chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="f663e-118">From hello CDN profile page, click hello CDN endpoint you wish toomanage.</span></span>
   
    ![Koncové body stránky profilu CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    <span data-ttu-id="f663e-120">Otevře se stránka koncový bod CDN Hello.</span><span class="sxs-lookup"><span data-stu-id="f663e-120">hello CDN endpoint page opens.</span></span>
2. <span data-ttu-id="f663e-121">Klikněte na tlačítko hello **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f663e-121">Click hello **Configure** button.</span></span>
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-config-btn.png)
   
    <span data-ttu-id="f663e-123">Otevře se stránka konfigurace CDN Hello.</span><span class="sxs-lookup"><span data-stu-id="f663e-123">hello CDN Configuration page opens.</span></span>
3. <span data-ttu-id="f663e-124">Zapnout **komprese**.</span><span class="sxs-lookup"><span data-stu-id="f663e-124">Turn on **Compression**.</span></span>
   
    ![Možnosti komprese CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. <span data-ttu-id="f663e-126">Použijte výchozí typy hello nebo změňte hello seznamu odebrání nebo přidáním typy souborů.</span><span class="sxs-lookup"><span data-stu-id="f663e-126">Use hello default types, or modify hello list by removing or adding file types.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="f663e-127">Chvíli je možné, nedoporučuje se tooapply komprese toocompressed formátů, jako je například ZIP, MP3, MP4, JPG, atd.</span><span class="sxs-lookup"><span data-stu-id="f663e-127">While possible, it is not recommended tooapply compression toocompressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span>
   > 
   > 
5. <span data-ttu-id="f663e-128">Po provedení změny, klikněte na hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f663e-128">After making your changes, click hello **Save** button.</span></span>

### <a name="premium-tier"></a><span data-ttu-id="f663e-129">Úroveň Premium</span><span class="sxs-lookup"><span data-stu-id="f663e-129">Premium tier</span></span>
> [!NOTE]
> <span data-ttu-id="f663e-130">Tato část se týká příliš**Azure CDN Premium od společnosti Verizon** profily.</span><span class="sxs-lookup"><span data-stu-id="f663e-130">This section applies too**Azure CDN Premium from Verizon** profiles.</span></span>
> 
> 

1. <span data-ttu-id="f663e-131">Na stránce profil hello CDN, klikněte na tlačítko hello **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f663e-131">From hello CDN profile page, click hello **Manage** button.</span></span>
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    <span data-ttu-id="f663e-133">Otevře portál pro správu Hello CDN.</span><span class="sxs-lookup"><span data-stu-id="f663e-133">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="f663e-134">Hover přes hello **HTTP velké** kartu a potom hover přes hello **nastavení mezipaměti** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="f663e-134">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="f663e-135">Klikněte na **komprese**.</span><span class="sxs-lookup"><span data-stu-id="f663e-135">Click on **Compression**.</span></span>

    ![Výběr komprese souboru](./media/cdn-file-compression/cdn-compress-select.png)
   
    <span data-ttu-id="f663e-137">Zobrazí se možnosti komprese.</span><span class="sxs-lookup"><span data-stu-id="f663e-137">Compression options are displayed.</span></span>
   
    ![Kompresí souborů](./media/cdn-file-compression/cdn-compress-files.png)
3. <span data-ttu-id="f663e-139">Povolit kompresi kliknutím hello **povolená komprese** přepínač.</span><span class="sxs-lookup"><span data-stu-id="f663e-139">Enable compression by clicking hello **Compression Enabled** radio button.</span></span>  <span data-ttu-id="f663e-140">Zadejte typy MIME hello chcete toocompress jako seznam s položkami oddělenými čárkou (bez mezer) v hello **typy souborů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f663e-140">Enter hello MIME types you wish toocompress as a comma-delimited list (no spaces) in hello **File Types** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="f663e-141">Chvíli je možné, nedoporučuje se tooapply komprese toocompressed formátů, jako je například ZIP, MP3, MP4, JPG, atd.</span><span class="sxs-lookup"><span data-stu-id="f663e-141">While possible, it is not recommended tooapply compression toocompressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span> 
   > 
   > 
4. <span data-ttu-id="f663e-142">Po provedení změny, klikněte na hello **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f663e-142">After making your changes, click hello **Update** button.</span></span>

## <a name="compression-rules"></a><span data-ttu-id="f663e-143">Komprese pravidla</span><span class="sxs-lookup"><span data-stu-id="f663e-143">Compression rules</span></span>
<span data-ttu-id="f663e-144">Tyto tabulky popisují chování Azure CDN komprese pro každý scénář.</span><span class="sxs-lookup"><span data-stu-id="f663e-144">These tables describe Azure CDN compression behavior for every scenario.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f663e-145">Pro **Azure CDN společnosti Verizon** (Standard a Premium), jenom vhodné soubory jsou komprimovány.</span><span class="sxs-lookup"><span data-stu-id="f663e-145">For **Azure CDN from Verizon** (Standard and Premium), only eligible files are compressed.</span></span>  <span data-ttu-id="f663e-146">toobe vhodné pro kompresi, musíte do souboru:</span><span class="sxs-lookup"><span data-stu-id="f663e-146">toobe eligible for compression, a file must:</span></span>
> 
> * <span data-ttu-id="f663e-147">Být větší než 128 bajtů.</span><span class="sxs-lookup"><span data-stu-id="f663e-147">Be larger than 128 bytes.</span></span>
> * <span data-ttu-id="f663e-148">Být menší než 1 MB.</span><span class="sxs-lookup"><span data-stu-id="f663e-148">Be smaller than 1 MB.</span></span>
> 
> <span data-ttu-id="f663e-149">Pro **Azure CDN společnosti Akamai**, všechny soubory, které jsou způsobilé pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="f663e-149">For **Azure CDN from Akamai**, all files are eligible for compression.</span></span>
> 
> <span data-ttu-id="f663e-150">Pro všechny produkty Azure CDN, soubor musí být typ MIME, který byl [nakonfigurované pro kompresi](#enabling-compression).</span><span class="sxs-lookup"><span data-stu-id="f663e-150">For all Azure CDN products, a file must be a MIME type that has been [configured for compression](#enabling-compression).</span></span>
> 
> <span data-ttu-id="f663e-151">**Azure CDN společnosti Verizon** profilů (Standard a Premium) podporu **gzip** (GNU zip), **deflate**, **bzip2**, nebo **Brazílie**Kódování (Brotli).</span><span class="sxs-lookup"><span data-stu-id="f663e-151">**Azure CDN from Verizon** profiles (Standard and Premium) support **gzip** (GNU zip), **deflate**, **bzip2**, or  **br** (Brotli) encoding.</span></span> <span data-ttu-id="f663e-152">Pro Brotli kódování, komprese hello se provádí pouze na hranici hello.</span><span class="sxs-lookup"><span data-stu-id="f663e-152">For Brotli encoding, hello compression is done only at hello edge.</span></span> <span data-ttu-id="f663e-153">Hello nebo prohlížeče klienta, musí poslat žádost o hello pro kódování Brotli a komprimované asset hello musí komprimované na straně původu hello nejdřív.</span><span class="sxs-lookup"><span data-stu-id="f663e-153">hello client/browser must send hello request for Brotli encoding and hello compressed asset must have been compressed on hello origin side first.</span></span> 
>
><span data-ttu-id="f663e-154">**Azure CDN společnosti Akamai** profily podporují pouze **gzip** kódování.</span><span class="sxs-lookup"><span data-stu-id="f663e-154">**Azure CDN from Akamai** profiles support only **gzip** encoding.</span></span>
> 
> <span data-ttu-id="f663e-155">**Azure CDN společnosti Akamai** koncových bodů se vždy požadavku **gzip** kódovaný soubory z počátku hello, bez ohledu na žádost klienta hello.</span><span class="sxs-lookup"><span data-stu-id="f663e-155">**Azure CDN from Akamai** endpoints always request **gzip** encoded files from hello origin, regardless of hello client request.</span></span> 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a><span data-ttu-id="f663e-156">Komprese vypnuta nebo souboru je není vhodná pro kompresi</span><span class="sxs-lookup"><span data-stu-id="f663e-156">Compression disabled or file is ineligible for compression</span></span>
| <span data-ttu-id="f663e-157">Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="f663e-157">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="f663e-158">Formát souborů uložených v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f663e-158">Cached file format</span></span> | <span data-ttu-id="f663e-159">CDN odpovědi toohello klienta</span><span class="sxs-lookup"><span data-stu-id="f663e-159">CDN response toohello client</span></span> | <span data-ttu-id="f663e-160">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f663e-160">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f663e-161">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-161">Compressed</span></span> |<span data-ttu-id="f663e-162">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-162">Compressed</span></span> |<span data-ttu-id="f663e-163">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-163">Compressed</span></span> | |
| <span data-ttu-id="f663e-164">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-164">Compressed</span></span> |<span data-ttu-id="f663e-165">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-165">Uncompressed</span></span> |<span data-ttu-id="f663e-166">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-166">Uncompressed</span></span> | |
| <span data-ttu-id="f663e-167">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-167">Compressed</span></span> |<span data-ttu-id="f663e-168">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f663e-168">Not cached</span></span> |<span data-ttu-id="f663e-169">Komprimované nebo nekomprimovaným</span><span class="sxs-lookup"><span data-stu-id="f663e-169">Compressed or Uncompressed</span></span> |<span data-ttu-id="f663e-170">Závisí na počátku odpovědi</span><span class="sxs-lookup"><span data-stu-id="f663e-170">Depends on origin response</span></span> |
| <span data-ttu-id="f663e-171">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-171">Uncompressed</span></span> |<span data-ttu-id="f663e-172">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-172">Compressed</span></span> |<span data-ttu-id="f663e-173">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-173">Uncompressed</span></span> | |
| <span data-ttu-id="f663e-174">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-174">Uncompressed</span></span> |<span data-ttu-id="f663e-175">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-175">Uncompressed</span></span> |<span data-ttu-id="f663e-176">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-176">Uncompressed</span></span> | |
| <span data-ttu-id="f663e-177">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-177">Uncompressed</span></span> |<span data-ttu-id="f663e-178">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f663e-178">Not cached</span></span> |<span data-ttu-id="f663e-179">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-179">Uncompressed</span></span> | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a><span data-ttu-id="f663e-180">Komprese zapnuta a souboru je vhodné pro kompresi</span><span class="sxs-lookup"><span data-stu-id="f663e-180">Compression enabled and file is eligible for compression</span></span>
| <span data-ttu-id="f663e-181">Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="f663e-181">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="f663e-182">Formát souborů uložených v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f663e-182">Cached file format</span></span> | <span data-ttu-id="f663e-183">CDN odpovědi toohello klienta</span><span class="sxs-lookup"><span data-stu-id="f663e-183">CDN response toohello client</span></span> | <span data-ttu-id="f663e-184">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f663e-184">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f663e-185">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-185">Compressed</span></span> |<span data-ttu-id="f663e-186">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-186">Compressed</span></span> |<span data-ttu-id="f663e-187">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-187">Compressed</span></span> |<span data-ttu-id="f663e-188">CDN transcodes mezi podporované formáty</span><span class="sxs-lookup"><span data-stu-id="f663e-188">CDN transcodes between supported formats</span></span> |
| <span data-ttu-id="f663e-189">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-189">Compressed</span></span> |<span data-ttu-id="f663e-190">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-190">Uncompressed</span></span> |<span data-ttu-id="f663e-191">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-191">Compressed</span></span> |<span data-ttu-id="f663e-192">CDN provede kompresi</span><span class="sxs-lookup"><span data-stu-id="f663e-192">CDN performs compression</span></span> |
| <span data-ttu-id="f663e-193">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-193">Compressed</span></span> |<span data-ttu-id="f663e-194">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f663e-194">Not cached</span></span> |<span data-ttu-id="f663e-195">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-195">Compressed</span></span> |<span data-ttu-id="f663e-196">CDN provede kompresi, pokud zdroj vrátí nekomprimované.</span><span class="sxs-lookup"><span data-stu-id="f663e-196">CDN performs compression if origin returns uncompressed.</span></span>  <span data-ttu-id="f663e-197">**Azure CDN společnosti Verizon** předává hello nekomprimovaného souboru na první požadavek hello a zkomprimuje potom a mezipamětí hello souboru pro následné požadavky.</span><span class="sxs-lookup"><span data-stu-id="f663e-197">**Azure CDN from Verizon** passes hello uncompressed file on hello first request and then compresses and caches hello file for subsequent requests.</span></span>  <span data-ttu-id="f663e-198">Soubory s `Cache-Control: no-cache` záhlaví nikdy komprimuje.</span><span class="sxs-lookup"><span data-stu-id="f663e-198">Files with `Cache-Control: no-cache` header will never be compressed.</span></span> |
| <span data-ttu-id="f663e-199">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-199">Uncompressed</span></span> |<span data-ttu-id="f663e-200">Komprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-200">Compressed</span></span> |<span data-ttu-id="f663e-201">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-201">Uncompressed</span></span> |<span data-ttu-id="f663e-202">Při dekompresi provede CDN</span><span class="sxs-lookup"><span data-stu-id="f663e-202">CDN performs decompression</span></span> |
| <span data-ttu-id="f663e-203">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-203">Uncompressed</span></span> |<span data-ttu-id="f663e-204">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-204">Uncompressed</span></span> |<span data-ttu-id="f663e-205">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-205">Uncompressed</span></span> | |
| <span data-ttu-id="f663e-206">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-206">Uncompressed</span></span> |<span data-ttu-id="f663e-207">Není v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f663e-207">Not cached</span></span> |<span data-ttu-id="f663e-208">Nekomprimované</span><span class="sxs-lookup"><span data-stu-id="f663e-208">Uncompressed</span></span> | |

## <a name="media-services-cdn-compression"></a><span data-ttu-id="f663e-209">Komprese CDN služby Media Services</span><span class="sxs-lookup"><span data-stu-id="f663e-209">Media Services CDN Compression</span></span>
<span data-ttu-id="f663e-210">Pro Media Services CDN povoleno koncových bodů streamování, je komprese povolena ve výchozím nastavení pro následující typy obsahu hello: aplikace nebo vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, aplikace nebo f4m + xml.</span><span class="sxs-lookup"><span data-stu-id="f663e-210">For Media Services CDN enabled streaming endpoints, compression is enabled by default for hello following content types: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl, application/f4m+xml.</span></span> <span data-ttu-id="f663e-211">Můžete nelze povolit nebo zakázat komprese hello uvedených typů pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f663e-211">You cannot enable/disable compression for hello mentioned types using hello Azure portal.</span></span>  

## <a name="see-also"></a><span data-ttu-id="f663e-212">Viz také</span><span class="sxs-lookup"><span data-stu-id="f663e-212">See also</span></span>
* [<span data-ttu-id="f663e-213">Řešení potíží s kompresí souborů v síti CDN</span><span class="sxs-lookup"><span data-stu-id="f663e-213">Troubleshooting CDN file compression</span></span>](cdn-troubleshoot-compression.md)    

