---
title: "kompresí souborů aaaTroubleshooting v Azure CDN | Microsoft Docs"
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
ms.openlocfilehash: f00b98beaf6b3b3cd30108ece65a8191edc06ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-file-compression"></a><span data-ttu-id="53b7f-103">Poradce při potížích s kompresí souborů CDN</span><span class="sxs-lookup"><span data-stu-id="53b7f-103">Troubleshooting CDN file compression</span></span>
<span data-ttu-id="53b7f-104">Tento článek vám pomůže vyřešit problémy s [komprese souboru CDN](cdn-improve-performance.md).</span><span class="sxs-lookup"><span data-stu-id="53b7f-104">This article helps you troubleshoot issues with [CDN file compression](cdn-improve-performance.md).</span></span>

<span data-ttu-id="53b7f-105">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="53b7f-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and hello Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="53b7f-106">Alternativně můžete také soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="53b7f-106">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="53b7f-107">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na tlačítko **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="53b7f-107">Go toohello [Azure Support site](https://azure.microsoft.com/support/options/) and click **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="53b7f-108">Příznaky</span><span class="sxs-lookup"><span data-stu-id="53b7f-108">Symptom</span></span>
<span data-ttu-id="53b7f-109">Je povolená komprese pro svůj koncový bod, ale soubory se vrací nekomprimované.</span><span class="sxs-lookup"><span data-stu-id="53b7f-109">Compression for your endpoint is enabled, but files are being returned uncompressed.</span></span>

> [!TIP]
> <span data-ttu-id="53b7f-110">toocheck zda vaše soubory se vrací komprimovaný, je nutné toouse jako nástroj [Fiddler](http://www.telerik.com/fiddler) nebo prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="53b7f-110">toocheck whether your files are being returned compressed, you need toouse a tool like [Fiddler](http://www.telerik.com/fiddler) or your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>  <span data-ttu-id="53b7f-111">Hlavičky odpovědi zkontrolujte hello HTTP vrátil s vaší mezipaměti CDN obsahu.</span><span class="sxs-lookup"><span data-stu-id="53b7f-111">Check hello HTTP response headers returned with your cached CDN content.</span></span>  <span data-ttu-id="53b7f-112">Pokud je záhlaví s názvem `Content-Encoding` s hodnotou **gzip**, **bzip2**, nebo **deflate**, obsah je komprimován.</span><span class="sxs-lookup"><span data-stu-id="53b7f-112">If there is a header named `Content-Encoding` with a value of **gzip**, **bzip2**, or **deflate**, your content is compressed.</span></span>
> 
> ![Kódování obsahu hlaviček](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a><span data-ttu-id="53b7f-114">Příčina</span><span class="sxs-lookup"><span data-stu-id="53b7f-114">Cause</span></span>
<span data-ttu-id="53b7f-115">Existuje několik možných příčin, včetně:</span><span class="sxs-lookup"><span data-stu-id="53b7f-115">There are several possible causes, including:</span></span>

* <span data-ttu-id="53b7f-116">Hello požadovaného obsahu nejsou vhodné pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="53b7f-116">hello requested content is not eligible for compression.</span></span>
* <span data-ttu-id="53b7f-117">Není povolená komprese hello požadovaný typ souboru.</span><span class="sxs-lookup"><span data-stu-id="53b7f-117">Compression is not enabled for hello requested file type.</span></span>
* <span data-ttu-id="53b7f-118">požadavek HTTP Hello nezahrnuli hlavičku požaduje typ platný komprese.</span><span class="sxs-lookup"><span data-stu-id="53b7f-118">hello HTTP request did not include a header requesting a valid compression type.</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="53b7f-119">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="53b7f-119">Troubleshooting steps</span></span>
> [!TIP]
> <span data-ttu-id="53b7f-120">Stejně jako u nasazení nové koncové body, provést změny konfigurace CDN některé čas toopropagate přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="53b7f-120">As with deploying new endpoints, CDN configuration changes take some time toopropagate through hello network.</span></span>  <span data-ttu-id="53b7f-121">Obvykle změny se použijí během 90 minut.</span><span class="sxs-lookup"><span data-stu-id="53b7f-121">Usually, changes are applied within 90 minutes.</span></span>  <span data-ttu-id="53b7f-122">Pokud je to hello poprvé, co jste nastavili komprese pro koncový bod CDN, měli byste zvážit čeká se, že rozšíření bodů POP toohello nastavení komprese hello 1 – 2 hodiny toobe.</span><span class="sxs-lookup"><span data-stu-id="53b7f-122">If this is hello first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours toobe sure hello compression settings have propagated toohello POPs.</span></span> 
> 
> 

### <a name="verify-hello-request"></a><span data-ttu-id="53b7f-123">Ověřit požadavek hello</span><span class="sxs-lookup"><span data-stu-id="53b7f-123">Verify hello request</span></span>
<span data-ttu-id="53b7f-124">Nejdřív by měl provedeme kontrolu rychlé správností hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="53b7f-124">First, we should do a quick sanity check on hello request.</span></span>  <span data-ttu-id="53b7f-125">Můžete použít v prohlížeči na [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello požadavků zasílaných.</span><span class="sxs-lookup"><span data-stu-id="53b7f-125">You can use your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello requests being made.</span></span>

* <span data-ttu-id="53b7f-126">Ověřte hello odesílá se požadavek adresy URL koncového bodu tooyour, `<endpointname>.azureedge.net`a ne do zdrojového umístění.</span><span class="sxs-lookup"><span data-stu-id="53b7f-126">Verify hello request is being sent tooyour endpoint URL, `<endpointname>.azureedge.net`, and not your origin.</span></span>
* <span data-ttu-id="53b7f-127">Ověřit požadavek hello obsahuje **Accept-Encoding** záhlaví a hello hodnotu této hlavičky obsahuje **gzip**, **deflate**, nebo **bzip2** .</span><span class="sxs-lookup"><span data-stu-id="53b7f-127">Verify hello request contains an **Accept-Encoding** header, and hello value for that header contains **gzip**, **deflate**, or **bzip2**.</span></span>

> [!NOTE]
> <span data-ttu-id="53b7f-128">**Azure CDN společnosti Akamai** profily podporuje se jen **gzip** kódování.</span><span class="sxs-lookup"><span data-stu-id="53b7f-128">**Azure CDN from Akamai** profiles only support **gzip** encoding.</span></span>
> 
> 

![Hlavičky žádosti CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a><span data-ttu-id="53b7f-130">Ověřte nastavení komprese (profil CDN úrovně Standard)</span><span class="sxs-lookup"><span data-stu-id="53b7f-130">Verify compression settings (Standard CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="53b7f-131">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Standard od společnosti Verizon** nebo **Azure CDN Standard od společnosti Akamai** profilu.</span><span class="sxs-lookup"><span data-stu-id="53b7f-131">This step only applies if your CDN profile is an **Azure CDN Standard from Verizon** or **Azure CDN Standard from Akamai** profile.</span></span> 
> 
> 

<span data-ttu-id="53b7f-132">Přejděte tooyour koncového bodu v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko hello **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="53b7f-132">Navigate tooyour endpoint in hello [Azure portal](https://portal.azure.com) and click hello **Configure** button.</span></span>

* <span data-ttu-id="53b7f-133">Ověřte, že je povolená komprese.</span><span class="sxs-lookup"><span data-stu-id="53b7f-133">Verify compression is enabled.</span></span>
* <span data-ttu-id="53b7f-134">Ověřte hello typ MIME pro hello obsahu toobe komprimované je obsažena v seznamu hello komprimované formátů.</span><span class="sxs-lookup"><span data-stu-id="53b7f-134">Verify hello MIME type for hello content toobe compressed is included in hello list of compressed formats.</span></span>

![Nastavení komprese CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a><span data-ttu-id="53b7f-136">Ověřte nastavení komprese (profil Premium CDN)</span><span class="sxs-lookup"><span data-stu-id="53b7f-136">Verify compression settings (Premium CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="53b7f-137">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Premium od společnosti Verizon** profilu.</span><span class="sxs-lookup"><span data-stu-id="53b7f-137">This step only applies if your CDN profile is an **Azure CDN Premium from Verizon** profile.</span></span>
> 
> 

<span data-ttu-id="53b7f-138">Přejděte tooyour koncového bodu v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko hello **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="53b7f-138">Navigate tooyour endpoint in hello [Azure portal](https://portal.azure.com) and click hello **Manage** button.</span></span>  <span data-ttu-id="53b7f-139">Otevře se Hello doplňkovém portálu.</span><span class="sxs-lookup"><span data-stu-id="53b7f-139">hello supplemental portal will open.</span></span>  <span data-ttu-id="53b7f-140">Hover přes hello **HTTP velké** kartu a potom hover přes hello **nastavení mezipaměti** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="53b7f-140">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="53b7f-141">Klikněte na tlačítko **komprese**.</span><span class="sxs-lookup"><span data-stu-id="53b7f-141">Click **Compression**.</span></span> 

* <span data-ttu-id="53b7f-142">Ověřte, že je povolená komprese.</span><span class="sxs-lookup"><span data-stu-id="53b7f-142">Verify compression is enabled.</span></span>
* <span data-ttu-id="53b7f-143">Ověřte hello **typy souborů** seznam obsahuje seznam oddělený čárkami (bez mezer) typů standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="53b7f-143">Verify hello **File Types** list contains a comma-separated list (no spaces) of MIME types.</span></span>
* <span data-ttu-id="53b7f-144">Ověřte hello typ MIME pro hello obsahu toobe komprimované je obsažena v seznamu hello komprimované formátů.</span><span class="sxs-lookup"><span data-stu-id="53b7f-144">Verify hello MIME type for hello content toobe compressed is included in hello list of compressed formats.</span></span>

![Nastavení komprese CDN premium](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-hello-content-is-cached"></a><span data-ttu-id="53b7f-146">Ověřte, zda text hello obsah se uloží do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="53b7f-146">Verify hello content is cached</span></span>
> [!NOTE]
> <span data-ttu-id="53b7f-147">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).</span><span class="sxs-lookup"><span data-stu-id="53b7f-147">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="53b7f-148">Používání nástrojů pro vývojáře v prohlížeči, zkontrolujte, že hello odpověď záhlaví tooensure hello souboru je uložené v mezipaměti v hello oblasti, kde jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="53b7f-148">Using your browser's developer tools, check hello response headers tooensure hello file is cached in hello region where it is being requested.</span></span>

* <span data-ttu-id="53b7f-149">Zkontrolujte hello **Server** hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="53b7f-149">Check hello **Server** response header.</span></span>  <span data-ttu-id="53b7f-150">Hlavička Hello by měl mít formát hello **platformy (ID POP nebo serveru)**, jak je vidět v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="53b7f-150">hello header should have hello format **Platform (POP/Server ID)**, as seen in hello following example.</span></span>
* <span data-ttu-id="53b7f-151">Zkontrolujte hello **X mezipaměti** hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="53b7f-151">Check hello **X-Cache** response header.</span></span>  <span data-ttu-id="53b7f-152">záhlaví Hello měli přečíst **DOSÁHL**.</span><span class="sxs-lookup"><span data-stu-id="53b7f-152">hello header should read **HIT**.</span></span>  

![Hlavičky odpovědi CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-hello-file-meets-hello-size-requirements"></a><span data-ttu-id="53b7f-154">Ověřte, zda text hello soubor splňuje požadavky na velikost hello</span><span class="sxs-lookup"><span data-stu-id="53b7f-154">Verify hello file meets hello size requirements</span></span>
> [!NOTE]
> <span data-ttu-id="53b7f-155">Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).</span><span class="sxs-lookup"><span data-stu-id="53b7f-155">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="53b7f-156">toobe vhodné pro kompresi, soubor, musí splňovat následující požadavky na velikost hello:</span><span class="sxs-lookup"><span data-stu-id="53b7f-156">toobe eligible for compression, a file must meet hello following size requirements:</span></span>

* <span data-ttu-id="53b7f-157">Větší než 128 bajtů.</span><span class="sxs-lookup"><span data-stu-id="53b7f-157">Larger than 128 bytes.</span></span>
* <span data-ttu-id="53b7f-158">Menší než 1 MB.</span><span class="sxs-lookup"><span data-stu-id="53b7f-158">Smaller than 1 MB.</span></span>

### <a name="check-hello-request-at-hello-origin-server-for-a-via-header"></a><span data-ttu-id="53b7f-159">Zkontrolujte hello požadavek na původní server hello pro **prostřednictvím** záhlaví</span><span class="sxs-lookup"><span data-stu-id="53b7f-159">Check hello request at hello origin server for a **Via** header</span></span>
<span data-ttu-id="53b7f-160">Hello **prostřednictvím** hlavičky protokolu HTTP označuje toohello webový server, který hello požadavku je předávána pomocí serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="53b7f-160">hello **Via** HTTP header indicates toohello web server that hello request is being passed by a proxy server.</span></span>  <span data-ttu-id="53b7f-161">Webové servery Microsoft IIS ve výchozím nastavení Nekomprimovat odpovědí hello požadavek obsahuje **prostřednictvím** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="53b7f-161">Microsoft IIS web servers by default do not compress responses when hello request contains a **Via** header.</span></span>  <span data-ttu-id="53b7f-162">toooverride toto chování, proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="53b7f-162">toooverride this behavior, perform hello following:</span></span>

* <span data-ttu-id="53b7f-163">**Služby IIS 6**: [nastavit HcNoCompressionForProxies = "FALSE" ve vlastnostech hello metabáze služby IIS](https://msdn.microsoft.com/library/ms525390.aspx)</span><span class="sxs-lookup"><span data-stu-id="53b7f-163">**IIS 6**: [Set HcNoCompressionForProxies="FALSE" in hello IIS Metabase properties](https://msdn.microsoft.com/library/ms525390.aspx)</span></span>
* <span data-ttu-id="53b7f-164">**Službu IIS 7 a vyšší**: [nastavit obě **noCompressionForHttp10** a **noCompressionForProxies** tooFalse v konfiguraci serveru hello](http://www.iis.net/configreference/system.webserver/httpcompression)</span><span class="sxs-lookup"><span data-stu-id="53b7f-164">**IIS 7 and up**: [Set both **noCompressionForHttp10** and **noCompressionForProxies** tooFalse in hello server configuration](http://www.iis.net/configreference/system.webserver/httpcompression)</span></span>

