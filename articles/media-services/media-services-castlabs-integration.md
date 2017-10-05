---
title: "Při doručování licence na Widevine do služby Azure Media Services pomocí castLabs | Microsoft Docs"
description: "Tento článek popisuje, jak můžete použít Azure Media Services (AMS) k poskytování datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. Licence PlayReady pochází z Media Services PlayReady licenčního serveru a licence Widevine doručuje castLabs licenční server."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 5b69e804809f834e81221fb2787a997a52dbe286
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a><span data-ttu-id="0cb92-104">Distribuce licencí Widevine pro Azure Media Services pomocí castLabs</span><span class="sxs-lookup"><span data-stu-id="0cb92-104">Using castLabs to deliver Widevine licenses to Azure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0cb92-105">Axinom</span><span class="sxs-lookup"><span data-stu-id="0cb92-105">Axinom</span></span>](media-services-axinom-integration.md)
> * [<span data-ttu-id="0cb92-106">castLabs</span><span class="sxs-lookup"><span data-stu-id="0cb92-106">castLabs</span></span>](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="0cb92-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="0cb92-107">Overview</span></span>
<span data-ttu-id="0cb92-108">Tento článek popisuje, jak můžete použít Azure Media Services (AMS) k poskytování datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS.</span><span class="sxs-lookup"><span data-stu-id="0cb92-108">This article describes how you can use Azure Media Services (AMS) to deliver a stream that is dynamically encrypted by AMS with both PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="0cb92-109">Licence PlayReady pochází z Media Services PlayReady licenční server a je doručil licence Widevine **castLabs** licenční server.</span><span class="sxs-lookup"><span data-stu-id="0cb92-109">The PlayReady license comes from Media Services PlayReady license server and Widevine license is delivered by **castLabs** license server.</span></span>

<span data-ttu-id="0cb92-110">K přehrávání streamování obsah chráněný CENC (PlayReady nebo Widevine), můžete použít [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="0cb92-110">To playback streaming content protected by CENC (PlayReady and/or Widevine), you can use  [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="0cb92-111">V tématu [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0cb92-111">See [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details.</span></span>

<span data-ttu-id="0cb92-112">Následující diagram ukazuje základní Azure Media Services a Architektura integrace castLabs.</span><span class="sxs-lookup"><span data-stu-id="0cb92-112">The following diagram demonstrates a high-level Azure Media Services and castLabs integration architecture.</span></span>

![Integrace](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a><span data-ttu-id="0cb92-114">Typických systémových nastavení</span><span class="sxs-lookup"><span data-stu-id="0cb92-114">Typical system set up</span></span>
* <span data-ttu-id="0cb92-115">Mediální obsah uložený v AMS.</span><span class="sxs-lookup"><span data-stu-id="0cb92-115">Media content is stored in AMS.</span></span>
* <span data-ttu-id="0cb92-116">Klíč ID obsahu klíče jsou uloženy v castLabs i AMS.</span><span class="sxs-lookup"><span data-stu-id="0cb92-116">Key IDs of content keys are stored in both castLabs and AMS.</span></span>
* <span data-ttu-id="0cb92-117">castLabs a AMS mají součástí ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="0cb92-117">castLabs and AMS both have token authentication built in.</span></span> <span data-ttu-id="0cb92-118">Následující části popisují tokeny ověřování.</span><span class="sxs-lookup"><span data-stu-id="0cb92-118">The following sections discuss authentication tokens.</span></span> 
* <span data-ttu-id="0cb92-119">Když klient požádá o Streamovat videa, je obsah dynamicky šifrován **Common Encryption** (CENC) a zabalené dynamicky podle AMS technologie Smooth Streaming a POMLČKY.</span><span class="sxs-lookup"><span data-stu-id="0cb92-119">When a client requests to stream the video, the content is dynamically encrypted with **Common Encryption** (CENC) and dynamically packaged by AMS to Smooth Streaming and DASH.</span></span> <span data-ttu-id="0cb92-120">Můžeme také poskytovat šifrování PlayReady M2TS základní stream pro protokol pro streamování HLS.</span><span class="sxs-lookup"><span data-stu-id="0cb92-120">We also deliver PlayReady M2TS elementary stream encryption for HLS streaming protocol.</span></span>
* <span data-ttu-id="0cb92-121">Licence PlayReady se načítají AMS licenčního serveru a licence Widevine se načítají z castLabs licenční server.</span><span class="sxs-lookup"><span data-stu-id="0cb92-121">PlayReady license is retrieved from AMS license server and Widevine license is retrieved from castLabs license server.</span></span> 
* <span data-ttu-id="0cb92-122">Přehrávač médií automaticky rozhoduje, které licenci k načtení podle funkcí platformy klienta.</span><span class="sxs-lookup"><span data-stu-id="0cb92-122">Media Player automatically decides which license to fetch based on the client platform capability.</span></span> 

## <a name="authentication-token-generation-for-getting-a-license"></a><span data-ttu-id="0cb92-123">Generování tokenů ověřování pro získání licence</span><span class="sxs-lookup"><span data-stu-id="0cb92-123">Authentication token generation for getting a license</span></span>
<span data-ttu-id="0cb92-124">CastLabs i AMS podporují formát tokenu JWT (JSON Web Token) použitý k autorizaci licenci.</span><span class="sxs-lookup"><span data-stu-id="0cb92-124">Both castLabs and AMS support JWT (JSON Web Token) token format used to authorize a license.</span></span> 

### <a name="jwt-token-in-ams"></a><span data-ttu-id="0cb92-125">Token JWT v AMS</span><span class="sxs-lookup"><span data-stu-id="0cb92-125">JWT token in AMS</span></span>
<span data-ttu-id="0cb92-126">Následující tabulka popisuje token JWT v AMS.</span><span class="sxs-lookup"><span data-stu-id="0cb92-126">The following table describes JWT token in AMS.</span></span> 

| <span data-ttu-id="0cb92-127">Vystavitel</span><span class="sxs-lookup"><span data-stu-id="0cb92-127">Issuer</span></span> | <span data-ttu-id="0cb92-128">Řetězec vystavitele z vybraného zabezpečení tokenu služby (STS)</span><span class="sxs-lookup"><span data-stu-id="0cb92-128">Issuer string from the chosen Secure Token Service (STS)</span></span> |
| --- | --- |
| <span data-ttu-id="0cb92-129">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="0cb92-129">Audience</span></span> |<span data-ttu-id="0cb92-130">Cílová skupina řetězec z použité služby tokenů zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0cb92-130">Audience string from the used STS</span></span> |
| <span data-ttu-id="0cb92-131">Deklarace identity</span><span class="sxs-lookup"><span data-stu-id="0cb92-131">Claims</span></span> |<span data-ttu-id="0cb92-132">Sadu deklarací identity</span><span class="sxs-lookup"><span data-stu-id="0cb92-132">A set of claims</span></span> |
| <span data-ttu-id="0cb92-133">Neplatí před</span><span class="sxs-lookup"><span data-stu-id="0cb92-133">NotBefore</span></span> |<span data-ttu-id="0cb92-134">Zahájení platnosti tokenu</span><span class="sxs-lookup"><span data-stu-id="0cb92-134">Start validity of the token</span></span> |
| <span data-ttu-id="0cb92-135">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="0cb92-135">Expires</span></span> |<span data-ttu-id="0cb92-136">End platnosti tokenu</span><span class="sxs-lookup"><span data-stu-id="0cb92-136">End validity of the token</span></span> |
| <span data-ttu-id="0cb92-137">SigningCredentials</span><span class="sxs-lookup"><span data-stu-id="0cb92-137">SigningCredentials</span></span> |<span data-ttu-id="0cb92-138">Klíč, který je sdílen PlayReady licenční Server castLabs licenční Server a službu tokenů zabezpečení, to může být buď symetrický, nebo asymetrický klíč.</span><span class="sxs-lookup"><span data-stu-id="0cb92-138">The key that is shared among PlayReady License Server, castLabs License Server and STS, it could be either symmetric or asymmetric key.</span></span> |

### <a name="jwt-token-in-castlabs"></a><span data-ttu-id="0cb92-139">Token JWT v castLabs</span><span class="sxs-lookup"><span data-stu-id="0cb92-139">JWT token in castLabs</span></span>
<span data-ttu-id="0cb92-140">Následující tabulka popisuje token JWT v castLabs.</span><span class="sxs-lookup"><span data-stu-id="0cb92-140">The following table describes JWT token in castLabs.</span></span> 

| <span data-ttu-id="0cb92-141">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0cb92-141">Name</span></span> | <span data-ttu-id="0cb92-142">Popis</span><span class="sxs-lookup"><span data-stu-id="0cb92-142">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0cb92-143">optData</span><span class="sxs-lookup"><span data-stu-id="0cb92-143">optData</span></span> |<span data-ttu-id="0cb92-144">Řetězec formátu JSON, který obsahuje informace o vás.</span><span class="sxs-lookup"><span data-stu-id="0cb92-144">A JSON string containing information about you.</span></span> |
| <span data-ttu-id="0cb92-145">CRT</span><span class="sxs-lookup"><span data-stu-id="0cb92-145">crt</span></span> |<span data-ttu-id="0cb92-146">Řetězec formátu JSON, který obsahuje informace o prostředku, jeho licenční informace a přehrávání práva.</span><span class="sxs-lookup"><span data-stu-id="0cb92-146">A JSON string containing information about the asset, its license info and playback rights.</span></span> |
| <span data-ttu-id="0cb92-147">IAT</span><span class="sxs-lookup"><span data-stu-id="0cb92-147">iat</span></span> |<span data-ttu-id="0cb92-148">Aktuální hodnota datetime v epoch.</span><span class="sxs-lookup"><span data-stu-id="0cb92-148">The current datetime in epoch.</span></span> |
| <span data-ttu-id="0cb92-149">jti</span><span class="sxs-lookup"><span data-stu-id="0cb92-149">jti</span></span> |<span data-ttu-id="0cb92-150">Jedinečný identifikátor o tento token (každý token může být použit pouze jednou v systému castLabs).</span><span class="sxs-lookup"><span data-stu-id="0cb92-150">A unique identifier about this token (every token can only be used once in the castLabs system).</span></span> |

## <a name="sample-solution-set-up"></a><span data-ttu-id="0cb92-151">Nastavení ukázkové řešení</span><span class="sxs-lookup"><span data-stu-id="0cb92-151">Sample solution set up</span></span>
<span data-ttu-id="0cb92-152">[Ukázkové řešení](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) se skládá ze dvou projektů:</span><span class="sxs-lookup"><span data-stu-id="0cb92-152">The [sample solution](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) consists of two projects:</span></span>

* <span data-ttu-id="0cb92-153">Konzolovou aplikaci, která slouží k nastavení omezení DRM prostředek už ingestovaný pro technologie PlayReady a Widevine.</span><span class="sxs-lookup"><span data-stu-id="0cb92-153">A console app that can be used to set DRM restrictions on an already ingested asset, for both PlayReady and Widevine.</span></span>
* <span data-ttu-id="0cb92-154">Webovou aplikaci, která předá se tokeny, které může považovat za velmi zjednodušené verzi služby tokenů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0cb92-154">A Web Application that hands out tokens, which could be seen as a VERY SIMPLIFIED version of an STS.</span></span>

<span data-ttu-id="0cb92-155">Použití konzolové aplikace:</span><span class="sxs-lookup"><span data-stu-id="0cb92-155">To use the console application:</span></span>

1. <span data-ttu-id="0cb92-156">Změňte app.config nastavit přihlašovací údaje AMS, castLabs přihlašovací údaje, konfigurace služby tokenů zabezpečení a sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="0cb92-156">Change the app.config to setup AMS credentials, castLabs credentials, STS configuration and shared key.</span></span>
2. <span data-ttu-id="0cb92-157">Odešlete prostředek do AMS.</span><span class="sxs-lookup"><span data-stu-id="0cb92-157">Upload an Asset into AMS.</span></span>
3. <span data-ttu-id="0cb92-158">Získat identifikátor UUID z nahrané Asset a změňte 32 řádku v souboru Program.cs:</span><span class="sxs-lookup"><span data-stu-id="0cb92-158">Get the UUID from the uploaded Asset, and change Line 32 in the Program.cs file:</span></span>
   
      <span data-ttu-id="0cb92-159">var objIAsset = _kontextu. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1 500-80bd-b864-f1e4b62594cf"). FirstOrDefault();</span><span class="sxs-lookup"><span data-stu-id="0cb92-159">var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();</span></span>
4. <span data-ttu-id="0cb92-160">Použijte ID pro pojmenování prostředku v systému castLabs (44 řádku v souboru Program.cs).</span><span class="sxs-lookup"><span data-stu-id="0cb92-160">Use an AssetId for naming the asset in the castLabs system (Line 44 in the Program.cs file).</span></span>
   
   <span data-ttu-id="0cb92-161">Je nutné nastavit ID pro **castLabs**; musí se jednat o jedinečný alfanumerický řetězec.</span><span class="sxs-lookup"><span data-stu-id="0cb92-161">You must set AssetId for **castLabs**; it needs to be a unique alphanumeric string.</span></span>
5. <span data-ttu-id="0cb92-162">Spusťte program.</span><span class="sxs-lookup"><span data-stu-id="0cb92-162">Run the program.</span></span>

<span data-ttu-id="0cb92-163">Používat webové aplikace (STS):</span><span class="sxs-lookup"><span data-stu-id="0cb92-163">To use the Web Application (STS):</span></span>

1. <span data-ttu-id="0cb92-164">Změňte souboru web.config na instalační program castlabs obchodní ID, nastavení služby tokenů zabezpečení a sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="0cb92-164">Change the web.config to setup castlabs merchant ID, the STS configuration and the shared key.</span></span>
2. <span data-ttu-id="0cb92-165">Nasaďte na weby Azure.</span><span class="sxs-lookup"><span data-stu-id="0cb92-165">Deploy to Azure Websites.</span></span>
3. <span data-ttu-id="0cb92-166">Přejděte na web.</span><span class="sxs-lookup"><span data-stu-id="0cb92-166">Navigate to the website.</span></span>

## <a name="playing-back-a-video"></a><span data-ttu-id="0cb92-167">Přehrávání videa</span><span class="sxs-lookup"><span data-stu-id="0cb92-167">Playing back a video</span></span>
<span data-ttu-id="0cb92-168">K přehrávání videa šifrován běžné šifrování (PlayReady nebo Widevine), můžete použít [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="0cb92-168">To playback a video encrypted with common encryption (PlayReady and/or Widevine), you can use the [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="0cb92-169">Při spuštění aplikace konzoly, jsou opakována ID obsahu klíč a adresy URL Manifest.</span><span class="sxs-lookup"><span data-stu-id="0cb92-169">When running the console app, the Content Key ID and the Manifest URL are echoed.</span></span>

1. <span data-ttu-id="0cb92-170">Otevřete novou kartu a spusťte vaší služby tokenů zabezpečení: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span><span class="sxs-lookup"><span data-stu-id="0cb92-170">Open a new tab and launch your STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span></span>
2. <span data-ttu-id="0cb92-171">Přejděte na [přehrávač médií Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="0cb92-171">Go to [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
3. <span data-ttu-id="0cb92-172">Vložte adresu URL streamování.</span><span class="sxs-lookup"><span data-stu-id="0cb92-172">Paste in the streaming URL.</span></span>
4. <span data-ttu-id="0cb92-173">Klikněte **pokročilé možnosti** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0cb92-173">Click the **Advanced Options** checkbox.</span></span>
5. <span data-ttu-id="0cb92-174">V **ochrany** rozevíracího seznamu vyberte PlayReady nebo Widevine.</span><span class="sxs-lookup"><span data-stu-id="0cb92-174">In the **Protection** dropdown, select PlayReady and/or Widevine.</span></span>
6. <span data-ttu-id="0cb92-175">Vložte token, který jste získali z vaší služby tokenů zabezpečení do textového pole Token.</span><span class="sxs-lookup"><span data-stu-id="0cb92-175">Paste the token that you got from your STS in the Token textbox.</span></span> 
   
   <span data-ttu-id="0cb92-176">Není nutné castLab licenční server "nosiče =" předponu před token.</span><span class="sxs-lookup"><span data-stu-id="0cb92-176">The castLab license server does not need the “Bearer=” prefix in front of the token.</span></span> <span data-ttu-id="0cb92-177">Proto prosím odebrat před odesláním token.</span><span class="sxs-lookup"><span data-stu-id="0cb92-177">So please remove that before submitting the token.</span></span>
7. <span data-ttu-id="0cb92-178">Aktualizujte přehrávač.</span><span class="sxs-lookup"><span data-stu-id="0cb92-178">Update the player.</span></span>
8. <span data-ttu-id="0cb92-179">By měl být přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="0cb92-179">The video should be playing.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="0cb92-180">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="0cb92-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0cb92-181">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="0cb92-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

