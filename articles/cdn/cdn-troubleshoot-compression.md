---
title: "Řešení potíží s kompresí souborů v Azure CDN | Microsoft Docs"
description: "Vyřešte problémy s kompresí souborů Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5ef8a8262eb40aa827161764f03a63d031e43273
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-cdn-file-compression"></a><span data-ttu-id="02eb0-103">Poradce při potížích s kompresí souborů CDN</span><span class="sxs-lookup"><span data-stu-id="02eb0-103">Troubleshooting CDN file compression</span></span>
<span data-ttu-id="02eb0-104">Tento článek vám pomůže vyřešit problémy s [komprese souboru CDN](cdn-improve-performance.md).</span><span class="sxs-lookup"><span data-stu-id="02eb0-104">This article helps you troubleshoot issues with [CDN file compression](cdn-improve-performance.md).</span></span>

<span data-ttu-id="02eb0-105">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [MSDN Azure a fóra Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="02eb0-105">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and the Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="02eb0-106">Alternativně můžete také soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="02eb0-106">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="02eb0-107">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na tlačítko **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-107">Go to the [Azure Support site](https://azure.microsoft.com/support/options/) and click **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="02eb0-108">Příznaky</span><span class="sxs-lookup"><span data-stu-id="02eb0-108">Symptom</span></span>
<span data-ttu-id="02eb0-109">Je povolená komprese pro svůj koncový bod, ale soubory se vrací nekomprimované.</span><span class="sxs-lookup"><span data-stu-id="02eb0-109">Compression for your endpoint is enabled, but files are being returned uncompressed.</span></span>

> [!TIP]
> <span data-ttu-id="02eb0-110">Pokud chcete zkontrolovat, zda jsou vaše soubory nevrátila komprimovaný, budete muset použít nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) nebo prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="02eb0-110">To check whether your files are being returned compressed, you need to use a tool like [Fiddler](http://www.telerik.com/fiddler) or your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>  <span data-ttu-id="02eb0-111">Hlavičky odpovědi zkontrolujte HTTP vrátil s vaší mezipaměti CDN obsahu.</span><span class="sxs-lookup"><span data-stu-id="02eb0-111">Check the HTTP response headers returned with your cached CDN content.</span></span>  <span data-ttu-id="02eb0-112">Pokud je záhlaví s názvem `Content-Encoding` s hodnotou **gzip**, **bzip2**, nebo **deflate**, obsah je komprimován.</span><span class="sxs-lookup"><span data-stu-id="02eb0-112">If there is a header named `Content-Encoding` with a value of **gzip**, **bzip2**, or **deflate**, your content is compressed.</span></span>
> 
> ![Kódování obsahu hlaviček](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a><span data-ttu-id="02eb0-114">Příčina</span><span class="sxs-lookup"><span data-stu-id="02eb0-114">Cause</span></span>
<span data-ttu-id="02eb0-115">Existuje několik možných příčin, včetně:</span><span class="sxs-lookup"><span data-stu-id="02eb0-115">There are several possible causes, including:</span></span>

* <span data-ttu-id="02eb0-116">Požadovaný obsah nejsou vhodné pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="02eb0-116">The requested content is not eligible for compression.</span></span>
* <span data-ttu-id="02eb0-117">Typ požadovaný soubor není povolená komprese.</span><span class="sxs-lookup"><span data-stu-id="02eb0-117">Compression is not enabled for the requested file type.</span></span>
* <span data-ttu-id="02eb0-118">Požadavek HTTP neobsahuje hlavičku požaduje typ platný komprese.</span><span class="sxs-lookup"><span data-stu-id="02eb0-118">The HTTP request did not include a header requesting a valid compression type.</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="02eb0-119">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="02eb0-119">Troubleshooting steps</span></span>
> [!TIP]
> <span data-ttu-id="02eb0-120">Stejně jako u nasazení nové koncové body, provést změny konfigurace CDN některé čas potřebný k šíření přes síť.</span><span class="sxs-lookup"><span data-stu-id="02eb0-120">As with deploying new endpoints, CDN configuration changes take some time to propagate through the network.</span></span>  <span data-ttu-id="02eb0-121">Obvykle změny se použijí během 90 minut.</span><span class="sxs-lookup"><span data-stu-id="02eb0-121">Usually, changes are applied within 90 minutes.</span></span>  <span data-ttu-id="02eb0-122">Pokud je poprvé, které jste nastavili komprese pro koncový bod CDN, měli byste zvážit čekání 1 – 2 hodiny jistotu komprese, které se mají nastavení rozšíří do bodů POP.</span><span class="sxs-lookup"><span data-stu-id="02eb0-122">If this is the first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours to be sure the compression settings have propagated to the POPs.</span></span> 
> 
> 

### <a name="verify-the-request"></a><span data-ttu-id="02eb0-123">Ověřit požadavek</span><span class="sxs-lookup"><span data-stu-id="02eb0-123">Verify the request</span></span>
<span data-ttu-id="02eb0-124">Nejdřív by měl provedeme kontrolu rychlé správností v požadavku.</span><span class="sxs-lookup"><span data-stu-id="02eb0-124">First, we should do a quick sanity check on the request.</span></span>  <span data-ttu-id="02eb0-125">Můžete použít v prohlížeči na [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) zobrazíte požadavky prováděné.</span><span class="sxs-lookup"><span data-stu-id="02eb0-125">You can use your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) to view the requests being made.</span></span>

* <span data-ttu-id="02eb0-126">Ověření požadavku je odesílána vaše adresa URL koncového bodu `<endpointname>.azureedge.net`a ne do zdrojového umístění.</span><span class="sxs-lookup"><span data-stu-id="02eb0-126">Verify the request is being sent to your endpoint URL, `<endpointname>.azureedge.net`, and not your origin.</span></span>
* <span data-ttu-id="02eb0-127">Ověřit požadavek obsahuje **Accept-Encoding** obsahuje hlavičku a hodnotu této hlavičky **gzip**, **deflate**, nebo **bzip2**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-127">Verify the request contains an **Accept-Encoding** header, and the value for that header contains **gzip**, **deflate**, or **bzip2**.</span></span>

> [!NOTE]
> <span data-ttu-id="02eb0-128">**Azure CDN společnosti Akamai** profily podporuje se jen **gzip** kódování.</span><span class="sxs-lookup"><span data-stu-id="02eb0-128">**Azure CDN from Akamai** profiles only support **gzip** encoding.</span></span>
> 
> 

![Hlavičky žádosti CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a><span data-ttu-id="02eb0-130">Ověřte nastavení komprese (profil CDN úrovně Standard)</span><span class="sxs-lookup"><span data-stu-id="02eb0-130">Verify compression settings (Standard CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="02eb0-131">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Standard od společnosti Verizon** nebo **Azure CDN Standard od společnosti Akamai** profilu.</span><span class="sxs-lookup"><span data-stu-id="02eb0-131">This step only applies if your CDN profile is an **Azure CDN Standard from Verizon** or **Azure CDN Standard from Akamai** profile.</span></span> 
> 
> 

<span data-ttu-id="02eb0-132">Přejděte na váš koncový bod v [portál Azure](https://portal.azure.com) a klikněte na tlačítko **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02eb0-132">Navigate to your endpoint in the [Azure portal](https://portal.azure.com) and click the **Configure** button.</span></span>

* <span data-ttu-id="02eb0-133">Ověřte, že je povolená komprese.</span><span class="sxs-lookup"><span data-stu-id="02eb0-133">Verify compression is enabled.</span></span>
* <span data-ttu-id="02eb0-134">Ověřte, že typ MIME pro obsah, aby se komprimoval je obsažena v seznamu komprimované formátů.</span><span class="sxs-lookup"><span data-stu-id="02eb0-134">Verify the MIME type for the content to be compressed is included in the list of compressed formats.</span></span>

![Nastavení komprese CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a><span data-ttu-id="02eb0-136">Ověřte nastavení komprese (profil Premium CDN)</span><span class="sxs-lookup"><span data-stu-id="02eb0-136">Verify compression settings (Premium CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="02eb0-137">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Premium od společnosti Verizon** profilu.</span><span class="sxs-lookup"><span data-stu-id="02eb0-137">This step only applies if your CDN profile is an **Azure CDN Premium from Verizon** profile.</span></span>
> 
> 

<span data-ttu-id="02eb0-138">Přejděte na váš koncový bod v [portál Azure](https://portal.azure.com) a klikněte na tlačítko **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02eb0-138">Navigate to your endpoint in the [Azure portal](https://portal.azure.com) and click the **Manage** button.</span></span>  <span data-ttu-id="02eb0-139">Otevře se na doplňkovém portálu.</span><span class="sxs-lookup"><span data-stu-id="02eb0-139">The supplemental portal will open.</span></span>  <span data-ttu-id="02eb0-140">Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="02eb0-140">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="02eb0-141">Klikněte na tlačítko **komprese**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-141">Click **Compression**.</span></span> 

* <span data-ttu-id="02eb0-142">Ověřte, že je povolená komprese.</span><span class="sxs-lookup"><span data-stu-id="02eb0-142">Verify compression is enabled.</span></span>
* <span data-ttu-id="02eb0-143">Ověřte **typy souborů** seznam obsahuje seznam oddělený čárkami (bez mezer) typů standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="02eb0-143">Verify the **File Types** list contains a comma-separated list (no spaces) of MIME types.</span></span>
* <span data-ttu-id="02eb0-144">Ověřte, že typ MIME pro obsah, aby se komprimoval je obsažena v seznamu komprimované formátů.</span><span class="sxs-lookup"><span data-stu-id="02eb0-144">Verify the MIME type for the content to be compressed is included in the list of compressed formats.</span></span>

![Nastavení komprese CDN premium](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a><span data-ttu-id="02eb0-146">Ověřte, že obsah se uloží do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="02eb0-146">Verify the content is cached</span></span>
> [!NOTE]
> <span data-ttu-id="02eb0-147">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).</span><span class="sxs-lookup"><span data-stu-id="02eb0-147">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="02eb0-148">Používání nástrojů pro vývojáře v prohlížeči, zkontrolujte hlavičky odpovědi zajistit, že soubor je uložené v mezipaměti v oblasti, kde jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="02eb0-148">Using your browser's developer tools, check the response headers to ensure the file is cached in the region where it is being requested.</span></span>

* <span data-ttu-id="02eb0-149">Zkontrolujte **Server** hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="02eb0-149">Check the **Server** response header.</span></span>  <span data-ttu-id="02eb0-150">Záhlaví musí mít formát **platformy (ID POP nebo serveru)**, jak je vidět v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="02eb0-150">The header should have the format **Platform (POP/Server ID)**, as seen in the following example.</span></span>
* <span data-ttu-id="02eb0-151">Zkontrolujte **X mezipaměti** hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="02eb0-151">Check the **X-Cache** response header.</span></span>  <span data-ttu-id="02eb0-152">Záhlaví měli přečíst **DOSÁHL**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-152">The header should read **HIT**.</span></span>  

![Hlavičky odpovědi CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a><span data-ttu-id="02eb0-154">Ověřte, že soubor splňuje požadavky na velikost</span><span class="sxs-lookup"><span data-stu-id="02eb0-154">Verify the file meets the size requirements</span></span>
> [!NOTE]
> <span data-ttu-id="02eb0-155">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).</span><span class="sxs-lookup"><span data-stu-id="02eb0-155">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="02eb0-156">Přijatelné pro kompresi soubor musí splňovat následující požadavky na velikost:</span><span class="sxs-lookup"><span data-stu-id="02eb0-156">To be eligible for compression, a file must meet the following size requirements:</span></span>

* <span data-ttu-id="02eb0-157">Větší než 128 bajtů.</span><span class="sxs-lookup"><span data-stu-id="02eb0-157">Larger than 128 bytes.</span></span>
* <span data-ttu-id="02eb0-158">Menší než 1 MB.</span><span class="sxs-lookup"><span data-stu-id="02eb0-158">Smaller than 1 MB.</span></span>

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a><span data-ttu-id="02eb0-159">Zkontrolujte požadavek na původním serveru **prostřednictvím** záhlaví</span><span class="sxs-lookup"><span data-stu-id="02eb0-159">Check the request at the origin server for a **Via** header</span></span>
<span data-ttu-id="02eb0-160">**Prostřednictvím** hlavičky protokolu HTTP na webový server označuje, že požadavek je předávána pomocí serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="02eb0-160">The **Via** HTTP header indicates to the web server that the request is being passed by a proxy server.</span></span>  <span data-ttu-id="02eb0-161">Webové servery Microsoft IIS ve výchozím nastavení není kompresi odpovědí, pokud požadavek obsahuje **prostřednictvím** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="02eb0-161">Microsoft IIS web servers by default do not compress responses when the request contains a **Via** header.</span></span>  <span data-ttu-id="02eb0-162">Chcete-li toto chování potlačit, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="02eb0-162">To override this behavior, perform the following:</span></span>

* <span data-ttu-id="02eb0-163">**Služby IIS 6**: [nastavit HcNoCompressionForProxies = "FALSE" ve vlastnostech metabáze služby IIS](https://msdn.microsoft.com/library/ms525390.aspx)</span><span class="sxs-lookup"><span data-stu-id="02eb0-163">**IIS 6**: [Set HcNoCompressionForProxies="FALSE" in the IIS Metabase properties](https://msdn.microsoft.com/library/ms525390.aspx)</span></span>
* <span data-ttu-id="02eb0-164">**Službu IIS 7 a vyšší**: [nastavit obě **noCompressionForHttp10** a **noCompressionForProxies** na hodnotu False v konfiguraci serveru](http://www.iis.net/configreference/system.webserver/httpcompression)</span><span class="sxs-lookup"><span data-stu-id="02eb0-164">**IIS 7 and up**: [Set both **noCompressionForHttp10** and **noCompressionForProxies** to False in the server configuration](http://www.iis.net/configreference/system.webserver/httpcompression)</span></span>

