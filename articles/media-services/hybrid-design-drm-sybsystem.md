---
title: "návrh aaaHybrid DRM subsystem(s) využívající Azure Media Services | Microsoft Docs"
description: "Toto téma popisuje hybridního návrhu DRM subsystem(s) využívající Azure Media Services."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 4206248420ccd4dbfc9a87a86f4763534c6254a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-design-of-drm-subsystems"></a><span data-ttu-id="a3790-103">Hybridního návrhu DRM subsystem(s)</span><span class="sxs-lookup"><span data-stu-id="a3790-103">Hybrid design of DRM subsystem(s)</span></span>

<span data-ttu-id="a3790-104">Toto téma popisuje hybridního návrhu DRM subsystem(s) využívající Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="a3790-104">This topic discusses hybrid design of DRM subsystem(s) using Azure Media Services.</span></span>

## <a name="overview"></a><span data-ttu-id="a3790-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="a3790-105">Overview</span></span>

<span data-ttu-id="a3790-106">Azure Media Services poskytuje podporu pro následující tři systému DRM hello:</span><span class="sxs-lookup"><span data-stu-id="a3790-106">Azure Media Services provides support for hello following three DRM system:</span></span>

* <span data-ttu-id="a3790-107">PlayReady</span><span class="sxs-lookup"><span data-stu-id="a3790-107">PlayReady</span></span>
* <span data-ttu-id="a3790-108">Widevine (modulární)</span><span class="sxs-lookup"><span data-stu-id="a3790-108">Widevine (Modular)</span></span>
* <span data-ttu-id="a3790-109">FairPlay</span><span class="sxs-lookup"><span data-stu-id="a3790-109">FairPlay</span></span>

<span data-ttu-id="a3790-110">Podpora DRM Hello zahrnuje šifrování DRM (dynamické šifrování) a doručování licencí, s podporuje všechny 3 technologiemi DRM jako prohlížeč přehrávač SDK Azure Media Player.</span><span class="sxs-lookup"><span data-stu-id="a3790-110">hello DRM support includes DRM encryption (dynamic encryption) and license delivery, with Azure Media Player supporting all 3 DRMs as a browser player SDK.</span></span>

<span data-ttu-id="a3790-111">Podrobné zpracování DRM nebo CENC subsystému návrhu a implementace, najdete v tématu hello dokument s názvem [CENC s více technologiemi DRM a řízením přístupu](media-services-cenc-with-multidrm-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="a3790-111">For a detailed treatment of DRM/CENC subsystem design and implementation, please see hello document titled [CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md).</span></span>

<span data-ttu-id="a3790-112">I když jsme plně podporují pro tři systémy DRM, někdy nutné zákazníci toouse různých částí vlastní infrastrukturu nebo subsystémy v přidání tooAzure Media Services toobuild hybridní subsystému DRM.</span><span class="sxs-lookup"><span data-stu-id="a3790-112">Although we offer complete support for three DRM systems, sometimes customers need toouse various parts of their own infrastructure/subsystems in addition tooAzure Media Services toobuild a hybrid DRM subsystem.</span></span>

<span data-ttu-id="a3790-113">Níže jsou některé běžné dotazy požaduje od zákazníků:</span><span class="sxs-lookup"><span data-stu-id="a3790-113">Below are some common questions asked by customers:</span></span>

* <span data-ttu-id="a3790-114">"Je možné pomocí vlastní servery licence DRM?"</span><span class="sxs-lookup"><span data-stu-id="a3790-114">"Can I use my own DRM license servers?"</span></span> <span data-ttu-id="a3790-115">(V tomto případě zákazníků investovaly do serverové farmy licence DRM s embedded obchodní logiku).</span><span class="sxs-lookup"><span data-stu-id="a3790-115">(In this case, customers have invested in DRM license server farm with embedded business logic).</span></span>
* <span data-ttu-id="a3790-116">"Je možné pomocí pouze vaše doručování licencí DRM ve službě Azure Media Services bez hostování obsahu v AMS?"</span><span class="sxs-lookup"><span data-stu-id="a3790-116">"Can I use only your DRM license delivery in Azure Media Services without hosting content in AMS?"</span></span>

## <a name="modularity-of-hello-ams-drm-platform"></a><span data-ttu-id="a3790-117">Modularitu hello platformy AMS DRM</span><span class="sxs-lookup"><span data-stu-id="a3790-117">Modularity of hello AMS DRM platform</span></span>

<span data-ttu-id="a3790-118">V rámci komplexní cloudové platformy, video má Azure Media Services DRM návrh s flexibilitu a modularitu v paměti.</span><span class="sxs-lookup"><span data-stu-id="a3790-118">As part of a comprehensive cloud video platform, Azure Media Services DRM has a design with flexibility and modularity in mind.</span></span> <span data-ttu-id="a3790-119">Azure Media Services můžete použít s žádným z hello následující různé kombinace popsané v následující tabulce hello, (následuje vysvětlení hello zápis použitý v tabulce hello).</span><span class="sxs-lookup"><span data-stu-id="a3790-119">You can use Azure Media Services with any of hello following different combinations described in hello table below (an explanation of hello notation used in hello table follows).</span></span> 

|<span data-ttu-id="a3790-120">**Hostování obsahu & počátek**</span><span class="sxs-lookup"><span data-stu-id="a3790-120">**Content hosting & origin**</span></span>|<span data-ttu-id="a3790-121">**Šifrování obsahu**</span><span class="sxs-lookup"><span data-stu-id="a3790-121">**Content encryption**</span></span>|<span data-ttu-id="a3790-122">**Doručování licencí DRM**</span><span class="sxs-lookup"><span data-stu-id="a3790-122">**DRM license delivery**</span></span>|
|---|---|---|
|<span data-ttu-id="a3790-123">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-123">AMS</span></span>|<span data-ttu-id="a3790-124">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-124">AMS</span></span>|<span data-ttu-id="a3790-125">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-125">AMS</span></span>|
|<span data-ttu-id="a3790-126">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-126">AMS</span></span>|<span data-ttu-id="a3790-127">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-127">AMS</span></span>|<span data-ttu-id="a3790-128">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-128">Third-party</span></span>|
|<span data-ttu-id="a3790-129">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-129">AMS</span></span>|<span data-ttu-id="a3790-130">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-130">Third-party</span></span>|<span data-ttu-id="a3790-131">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-131">AMS</span></span>|
|<span data-ttu-id="a3790-132">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-132">AMS</span></span>|<span data-ttu-id="a3790-133">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-133">Third-party</span></span>|<span data-ttu-id="a3790-134">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-134">Third-party</span></span>|
|<span data-ttu-id="a3790-135">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-135">Third-party</span></span>|<span data-ttu-id="a3790-136">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-136">Third-party</span></span>|<span data-ttu-id="a3790-137">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-137">AMS</span></span>|

### <a name="content-hosting--origin"></a><span data-ttu-id="a3790-138">Hostování obsahu & počátek</span><span class="sxs-lookup"><span data-stu-id="a3790-138">Content hosting & origin</span></span>

* <span data-ttu-id="a3790-139">AMS: asset videa je hostován v AMS a vysílání datového proudu je prostřednictvím koncových bodů streamování AMS (ale nemusí být dynamické balení).</span><span class="sxs-lookup"><span data-stu-id="a3790-139">AMS: video asset is hosted in AMS and streaming is through AMS streaming endpoints (but not necessarily dynamic packaging).</span></span>
* <span data-ttu-id="a3790-140">Třetí strany: video je hostovaná a doručit na platformě streamování třetích stran mimo AMS.</span><span class="sxs-lookup"><span data-stu-id="a3790-140">Third-party: video is hosted and delivered on a third-party streaming platform outside of AMS.</span></span>

### <a name="content-encryption"></a><span data-ttu-id="a3790-141">Šifrování obsahu</span><span class="sxs-lookup"><span data-stu-id="a3790-141">Content encryption</span></span>

* <span data-ttu-id="a3790-142">AMS: šifrování obsahu se provádí dynamicky nebo na vyžádání pomocí dynamického šifrování AMS.</span><span class="sxs-lookup"><span data-stu-id="a3790-142">AMS: content encryption is performed dynamically/on-demand by AMS dynamic encryption.</span></span>
* <span data-ttu-id="a3790-143">Třetí strany: šifrování obsahu se provádí mimo AMS použití předem zpracování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a3790-143">Third-party: content encryption is performed outside of AMS using a pre-processing workflow.</span></span>

### <a name="drm-license-delivery"></a><span data-ttu-id="a3790-144">Doručování licencí DRM</span><span class="sxs-lookup"><span data-stu-id="a3790-144">DRM license delivery</span></span>

* <span data-ttu-id="a3790-145">AMS: Licence DRM je doručil doručení licenční služby AMS.</span><span class="sxs-lookup"><span data-stu-id="a3790-145">AMS: DRM license is delivered by AMS license delivery service.</span></span>
* <span data-ttu-id="a3790-146">Třetích stran: Licence DRM doručuje licenčním serverem třetích stran DRM mimo AMS.</span><span class="sxs-lookup"><span data-stu-id="a3790-146">Third-party: DRM license is delivered by a third-party DRM license server outside of AMS.</span></span>

## <a name="configure-based-on-your-hybrid-scenario"></a><span data-ttu-id="a3790-147">Konfigurace na základě v hybridním scénáři</span><span class="sxs-lookup"><span data-stu-id="a3790-147">Configure based on your hybrid scenario</span></span>

### <a name="content-key"></a><span data-ttu-id="a3790-148">Klíč obsahu</span><span class="sxs-lookup"><span data-stu-id="a3790-148">Content key</span></span>

<span data-ttu-id="a3790-149">Prostřednictvím konfigurace klíč obsahu můžete řídit následující atributy AMS dynamického šifrování a doručení licenční služby AMS hello:</span><span class="sxs-lookup"><span data-stu-id="a3790-149">Through configuration of a content key, you can control hello following attributes of both AMS dynamic encryption and AMS license delivery service:</span></span>

* <span data-ttu-id="a3790-150">klíč obsahu Hello používá pro dynamické šifrování DRM.</span><span class="sxs-lookup"><span data-stu-id="a3790-150">hello content key used for dynamic DRM encryption.</span></span>
* <span data-ttu-id="a3790-151">Obsahu toobe licence DRM doručil služeb doručování licence: práva, klíče k obsahu a omezení.</span><span class="sxs-lookup"><span data-stu-id="a3790-151">DRM license content toobe delivered by license delivery services: rights, content key and restrictions.</span></span>
* <span data-ttu-id="a3790-152">Typ **obsahu omezení zásad autorizace pro klíč**: otevřete, IP adresu nebo omezení s tokenem.</span><span class="sxs-lookup"><span data-stu-id="a3790-152">Type of **content key authorization policy restriction**: open, IP, or token restriction.</span></span>
* <span data-ttu-id="a3790-153">Pokud **tokenu** typ **slouží k omezení zásad autorizace klíče obsahu**, hello **obsahu omezení zásad autorizace pro klíč** musí být splněné před vydáním licenci.</span><span class="sxs-lookup"><span data-stu-id="a3790-153">If **token** type of **content key authorization policy restriction is used**, hello **content key authorization policy restriction** must be met before a license is issued.</span></span>

### <a name="asset-delivery-policy"></a><span data-ttu-id="a3790-154">Zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="a3790-154">Asset delivery policy</span></span>

<span data-ttu-id="a3790-155">Prostřednictvím konfigurace zásady pro doručení assetu můžete řídit hello následující atributy používané dynamické Balíčkovač AMS a dynamické šifrování AMS koncový bod streamování:</span><span class="sxs-lookup"><span data-stu-id="a3790-155">Through configuration of an asset delivery policy, you can control hello following attributes used by AMS dynamic packager and dynamic encryption of an AMS streaming endpoint:</span></span>

* <span data-ttu-id="a3790-156">Streamování kombinace šifrování DRM a protokolu, jako je například DASH pod CENC (PlayReady a Widevine), funkce smooth streaming v rámci technologie PlayReady, HLS pod Widevine nebo PlayReady.</span><span class="sxs-lookup"><span data-stu-id="a3790-156">Streaming protocol and DRM encryption combination, such as DASH under CENC (PlayReady and Widevine), smooth streaming under PlayReady, HLS under Widevine or PlayReady.</span></span>
* <span data-ttu-id="a3790-157">Hello výchozí nebo vložených licence doručení adresy URL pro každou hello podílejí technologiemi DRM.</span><span class="sxs-lookup"><span data-stu-id="a3790-157">hello default/embedded license delivery URLs for each of hello involved DRMs.</span></span>
* <span data-ttu-id="a3790-158">Jestli licence získání adresy URL (LA_URLs) v DASH MPD nebo HLS seznam stop obsahovat řetězec dotazu klíče ID (DĚTSKÝ) pro Widevine a FairPlay, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="a3790-158">Whether license acquisition URLs (LA_URLs) in DASH MPD or HLS playlist contain query string of key ID (KID) for Widevine and FairPlay, respectively.</span></span>

## <a name="scenarios-and-samples"></a><span data-ttu-id="a3790-159">Scénáře a ukázky</span><span class="sxs-lookup"><span data-stu-id="a3790-159">Scenarios and samples</span></span>

<span data-ttu-id="a3790-160">Podle toho, vysvětlení hello v předchozí části hello, hello následujících pět hybridní scénáře použití příslušných **klíč obsahu**-**zásady doručení Assetu** kombinace konfigurací (hello Tabulka hello podle ukázky uvedený v posledním sloupci hello):</span><span class="sxs-lookup"><span data-stu-id="a3790-160">Based on hello explanations in hello previous section, hello following five hybrid scenarios use respective **Content key**-**Asset delivery policy** configuration combinations (hello samples mentioned in hello last column follow hello table):</span></span>

|<span data-ttu-id="a3790-161">**Hostování obsahu & počátek**</span><span class="sxs-lookup"><span data-stu-id="a3790-161">**Content hosting & origin**</span></span>|<span data-ttu-id="a3790-162">**Šifrování DRM**</span><span class="sxs-lookup"><span data-stu-id="a3790-162">**DRM encryption**</span></span>|<span data-ttu-id="a3790-163">**Doručování licencí DRM**</span><span class="sxs-lookup"><span data-stu-id="a3790-163">**DRM license delivery**</span></span>|<span data-ttu-id="a3790-164">**Konfigurace klíč obsahu**</span><span class="sxs-lookup"><span data-stu-id="a3790-164">**Configure content key**</span></span>|<span data-ttu-id="a3790-165">**Konfigurace zásad doručení assetu**</span><span class="sxs-lookup"><span data-stu-id="a3790-165">**Configure asset delivery policy**</span></span>|<span data-ttu-id="a3790-166">**Ukázka**</span><span class="sxs-lookup"><span data-stu-id="a3790-166">**Sample**</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="a3790-167">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-167">AMS</span></span>|<span data-ttu-id="a3790-168">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-168">AMS</span></span>|<span data-ttu-id="a3790-169">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-169">AMS</span></span>|<span data-ttu-id="a3790-170">Ano</span><span class="sxs-lookup"><span data-stu-id="a3790-170">Yes</span></span>|<span data-ttu-id="a3790-171">Ano</span><span class="sxs-lookup"><span data-stu-id="a3790-171">Yes</span></span>|<span data-ttu-id="a3790-172">Ukázka 1</span><span class="sxs-lookup"><span data-stu-id="a3790-172">Sample 1</span></span>|
|<span data-ttu-id="a3790-173">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-173">AMS</span></span>|<span data-ttu-id="a3790-174">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-174">AMS</span></span>|<span data-ttu-id="a3790-175">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-175">Third-party</span></span>|<span data-ttu-id="a3790-176">Ano</span><span class="sxs-lookup"><span data-stu-id="a3790-176">Yes</span></span>|<span data-ttu-id="a3790-177">Ano</span><span class="sxs-lookup"><span data-stu-id="a3790-177">Yes</span></span>|<span data-ttu-id="a3790-178">Ukázka 2</span><span class="sxs-lookup"><span data-stu-id="a3790-178">Sample 2</span></span>|
|<span data-ttu-id="a3790-179">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-179">AMS</span></span>|<span data-ttu-id="a3790-180">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-180">Third-party</span></span>|<span data-ttu-id="a3790-181">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-181">AMS</span></span>|<span data-ttu-id="a3790-182">Ano</span><span class="sxs-lookup"><span data-stu-id="a3790-182">Yes</span></span>|<span data-ttu-id="a3790-183">Ne</span><span class="sxs-lookup"><span data-stu-id="a3790-183">No</span></span>|<span data-ttu-id="a3790-184">Ukázka 3</span><span class="sxs-lookup"><span data-stu-id="a3790-184">Sample 3</span></span>|
|<span data-ttu-id="a3790-185">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-185">AMS</span></span>|<span data-ttu-id="a3790-186">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-186">Third-party</span></span>|<span data-ttu-id="a3790-187">Mimo</span><span class="sxs-lookup"><span data-stu-id="a3790-187">Outside</span></span>|<span data-ttu-id="a3790-188">Ne</span><span class="sxs-lookup"><span data-stu-id="a3790-188">No</span></span>|<span data-ttu-id="a3790-189">Ne</span><span class="sxs-lookup"><span data-stu-id="a3790-189">No</span></span>|<span data-ttu-id="a3790-190">Ukázka 4</span><span class="sxs-lookup"><span data-stu-id="a3790-190">Sample 4</span></span>|
|<span data-ttu-id="a3790-191">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-191">Third-party</span></span>|<span data-ttu-id="a3790-192">Třetí strany</span><span class="sxs-lookup"><span data-stu-id="a3790-192">Third-party</span></span>|<span data-ttu-id="a3790-193">AMS</span><span class="sxs-lookup"><span data-stu-id="a3790-193">AMS</span></span>|<span data-ttu-id="a3790-194">Ano</span><span class="sxs-lookup"><span data-stu-id="a3790-194">Yes</span></span>|<span data-ttu-id="a3790-195">Ne</span><span class="sxs-lookup"><span data-stu-id="a3790-195">No</span></span>|    

<span data-ttu-id="a3790-196">V hello ukázky PlayReady ochrany funguje pro DASH a technologie smooth streaming.</span><span class="sxs-lookup"><span data-stu-id="a3790-196">In hello samples, PlayReady protection works for both DASH and smooth streaming.</span></span> <span data-ttu-id="a3790-197">Hello video se adresy URL níže technologie smooth streaming adresy URL.</span><span class="sxs-lookup"><span data-stu-id="a3790-197">hello video URLs below are smooth streaming URLs.</span></span> <span data-ttu-id="a3790-198">tooget hello odpovídající adresy URL DASH, stačí připojit "(formát = mpd. čas csf)".</span><span class="sxs-lookup"><span data-stu-id="a3790-198">tooget hello corresponding DASH URLs, just append "(format=mpd-time-csf)".</span></span> <span data-ttu-id="a3790-199">Můžete použít hello [testování azure media player](http://aka.ms/amtest) tootest v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a3790-199">You could use hello [azure media test player](http://aka.ms/amtest) tootest in a browser.</span></span> <span data-ttu-id="a3790-200">Umožňuje tooconfigure který streamování toouse protokol, pod které technologie.</span><span class="sxs-lookup"><span data-stu-id="a3790-200">It allows you tooconfigure which streaming protocol toouse, under which tech.</span></span> <span data-ttu-id="a3790-201">Podpora technologie PlayReady prostřednictvím EME IE11 a MS Edge ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="a3790-201">IE11 and MS Edge on Windows 10 support PlayReady through EME.</span></span> <span data-ttu-id="a3790-202">Další informace najdete v tématu [podrobnosti o hello testovací nástroj](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).</span><span class="sxs-lookup"><span data-stu-id="a3790-202">For more information, see [details about hello test tool](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).</span></span>

### <a name="sample-1"></a><span data-ttu-id="a3790-203">Ukázka 1</span><span class="sxs-lookup"><span data-stu-id="a3790-203">Sample 1</span></span>

* <span data-ttu-id="a3790-204">Zdroj (základní) adresu URL: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="a3790-204">Source (base) URL: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest</span></span> 
* <span data-ttu-id="a3790-205">LA_URL PlayReady (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span><span class="sxs-lookup"><span data-stu-id="a3790-205">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span></span> 
* <span data-ttu-id="a3790-206">Widevine LA_URL (pomlčka): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4</span><span class="sxs-lookup"><span data-stu-id="a3790-206">Widevine LA_URL (DASH): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4</span></span> 
* <span data-ttu-id="a3790-207">LA_URL FairPlay (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8</span><span class="sxs-lookup"><span data-stu-id="a3790-207">FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8</span></span> 

### <a name="sample-2"></a><span data-ttu-id="a3790-208">Ukázka 2</span><span class="sxs-lookup"><span data-stu-id="a3790-208">Sample 2</span></span>

* <span data-ttu-id="a3790-209">Zdroj (základní) adresu URL: http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="a3790-209">Source (base) URL: http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest</span></span> 
* <span data-ttu-id="a3790-210">LA_URL PlayReady (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx</span><span class="sxs-lookup"><span data-stu-id="a3790-210">PlayReady LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx</span></span> 

### <a name="sample-3"></a><span data-ttu-id="a3790-211">Ukázka 3</span><span class="sxs-lookup"><span data-stu-id="a3790-211">Sample 3</span></span>

* <span data-ttu-id="a3790-212">Adresa URL zdroje: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="a3790-212">Source URL: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest</span></span> 
* <span data-ttu-id="a3790-213">LA_URL PlayReady (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span><span class="sxs-lookup"><span data-stu-id="a3790-213">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span></span> 

### <a name="sample-4"></a><span data-ttu-id="a3790-214">Ukázka 4</span><span class="sxs-lookup"><span data-stu-id="a3790-214">Sample 4</span></span>

* <span data-ttu-id="a3790-215">Adresa URL zdroje: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="a3790-215">Source URL: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest</span></span> 
* <span data-ttu-id="a3790-216">LA_URL PlayReady (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx</span><span class="sxs-lookup"><span data-stu-id="a3790-216">PlayReady LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx</span></span> 

## <a name="summary"></a><span data-ttu-id="a3790-217">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a3790-217">Summary</span></span>

<span data-ttu-id="a3790-218">V souhrnu jsou součástí Azure Media Services DRM flexibilní, můžete je v hybridním scénáři tak, že správně nakonfigurujete klíč obsahu a zásady doručení assetu, jak je popsáno v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="a3790-218">In summary, Azure Media Services DRM components are flexible, you can use them in a hybrid scenario by properly configuring content key and asset delivery policy, as described in this topic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3790-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3790-219">Next steps</span></span>
<span data-ttu-id="a3790-220">Zobrazení Media Services kurzů.</span><span class="sxs-lookup"><span data-stu-id="a3790-220">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a3790-221">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="a3790-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

