---
title: aaaUsing castLabs toodeliver Widevine licence tooAzure Media Services | Microsoft Docs
description: "Tento článek popisuje, jak můžete použít Azure Media Services (AMS) toodeliver datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. licence PlayReady Hello pochází z Media Services PlayReady licenčního serveru a licence Widevine doručuje castLabs licenční server."
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
ms.openlocfilehash: 80d2778fb283a96361e7e511990a36c2f551a310
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-castlabs-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="2348e-104">Pomocí castLabs toodeliver Widevine licence tooAzure Media Services</span><span class="sxs-lookup"><span data-stu-id="2348e-104">Using castLabs toodeliver Widevine licenses tooAzure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2348e-105">Axinom</span><span class="sxs-lookup"><span data-stu-id="2348e-105">Axinom</span></span>](media-services-axinom-integration.md)
> * [<span data-ttu-id="2348e-106">castLabs</span><span class="sxs-lookup"><span data-stu-id="2348e-106">castLabs</span></span>](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="2348e-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="2348e-107">Overview</span></span>
<span data-ttu-id="2348e-108">Tento článek popisuje, jak můžete použít Azure Media Services (AMS) toodeliver datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS.</span><span class="sxs-lookup"><span data-stu-id="2348e-108">This article describes how you can use Azure Media Services (AMS) toodeliver a stream that is dynamically encrypted by AMS with both PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="2348e-109">licence PlayReady Hello pochází z Media Services PlayReady licenční server a je doručil licence Widevine **castLabs** licenční server.</span><span class="sxs-lookup"><span data-stu-id="2348e-109">hello PlayReady license comes from Media Services PlayReady license server and Widevine license is delivered by **castLabs** license server.</span></span>

<span data-ttu-id="2348e-110">tooplayback streamování obsahu chráněného CENC (PlayReady nebo Widevine), můžete pomocí [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="2348e-110">tooplayback streaming content protected by CENC (PlayReady and/or Widevine), you can use  [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="2348e-111">V tématu [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2348e-111">See [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details.</span></span>

<span data-ttu-id="2348e-112">Hello následující diagram ukazuje základní Architektura integrace Azure Media Services a castLabs.</span><span class="sxs-lookup"><span data-stu-id="2348e-112">hello following diagram demonstrates a high-level Azure Media Services and castLabs integration architecture.</span></span>

![Integrace](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a><span data-ttu-id="2348e-114">Typických systémových nastavení</span><span class="sxs-lookup"><span data-stu-id="2348e-114">Typical system set up</span></span>
* <span data-ttu-id="2348e-115">Mediální obsah uložený v AMS.</span><span class="sxs-lookup"><span data-stu-id="2348e-115">Media content is stored in AMS.</span></span>
* <span data-ttu-id="2348e-116">Klíč ID obsahu klíče jsou uloženy v castLabs i AMS.</span><span class="sxs-lookup"><span data-stu-id="2348e-116">Key IDs of content keys are stored in both castLabs and AMS.</span></span>
* <span data-ttu-id="2348e-117">castLabs a AMS mají součástí ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="2348e-117">castLabs and AMS both have token authentication built in.</span></span> <span data-ttu-id="2348e-118">Hello následující oddíly popisují tokeny ověřování.</span><span class="sxs-lookup"><span data-stu-id="2348e-118">hello following sections discuss authentication tokens.</span></span> 
* <span data-ttu-id="2348e-119">Když klient požádá o toostream hello video, je obsah hello dynamicky šifrován **Common Encryption** (CENC) a dynamicky zabalené AMS tooSmooth, Streaming a POMLČKY.</span><span class="sxs-lookup"><span data-stu-id="2348e-119">When a client requests toostream hello video, hello content is dynamically encrypted with **Common Encryption** (CENC) and dynamically packaged by AMS tooSmooth Streaming and DASH.</span></span> <span data-ttu-id="2348e-120">Můžeme také poskytovat šifrování PlayReady M2TS základní stream pro protokol pro streamování HLS.</span><span class="sxs-lookup"><span data-stu-id="2348e-120">We also deliver PlayReady M2TS elementary stream encryption for HLS streaming protocol.</span></span>
* <span data-ttu-id="2348e-121">Licence PlayReady se načítají AMS licenčního serveru a licence Widevine se načítají z castLabs licenční server.</span><span class="sxs-lookup"><span data-stu-id="2348e-121">PlayReady license is retrieved from AMS license server and Widevine license is retrieved from castLabs license server.</span></span> 
* <span data-ttu-id="2348e-122">Přehrávač médií automaticky rozhoduje, které toofetch licencí na základě funkcí platformy hello klienta.</span><span class="sxs-lookup"><span data-stu-id="2348e-122">Media Player automatically decides which license toofetch based on hello client platform capability.</span></span> 

## <a name="authentication-token-generation-for-getting-a-license"></a><span data-ttu-id="2348e-123">Generování tokenů ověřování pro získání licence</span><span class="sxs-lookup"><span data-stu-id="2348e-123">Authentication token generation for getting a license</span></span>
<span data-ttu-id="2348e-124">CastLabs i AMS podporují tooauthorize používá formát tokenu JWT (JSON Web Token) licenci.</span><span class="sxs-lookup"><span data-stu-id="2348e-124">Both castLabs and AMS support JWT (JSON Web Token) token format used tooauthorize a license.</span></span> 

### <a name="jwt-token-in-ams"></a><span data-ttu-id="2348e-125">Token JWT v AMS</span><span class="sxs-lookup"><span data-stu-id="2348e-125">JWT token in AMS</span></span>
<span data-ttu-id="2348e-126">Hello následující tabulka popisuje token JWT v AMS.</span><span class="sxs-lookup"><span data-stu-id="2348e-126">hello following table describes JWT token in AMS.</span></span> 

| <span data-ttu-id="2348e-127">Vystavitel</span><span class="sxs-lookup"><span data-stu-id="2348e-127">Issuer</span></span> | <span data-ttu-id="2348e-128">Řetězec vystavitele z hello vybrali zabezpečení tokenu služby (STS)</span><span class="sxs-lookup"><span data-stu-id="2348e-128">Issuer string from hello chosen Secure Token Service (STS)</span></span> |
| --- | --- |
| <span data-ttu-id="2348e-129">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="2348e-129">Audience</span></span> |<span data-ttu-id="2348e-130">Cílová skupina řetězec z hello používá službu tokenů zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2348e-130">Audience string from hello used STS</span></span> |
| <span data-ttu-id="2348e-131">Deklarace identity</span><span class="sxs-lookup"><span data-stu-id="2348e-131">Claims</span></span> |<span data-ttu-id="2348e-132">Sadu deklarací identity</span><span class="sxs-lookup"><span data-stu-id="2348e-132">A set of claims</span></span> |
| <span data-ttu-id="2348e-133">Neplatí před</span><span class="sxs-lookup"><span data-stu-id="2348e-133">NotBefore</span></span> |<span data-ttu-id="2348e-134">Zahájení platnosti tokenu hello</span><span class="sxs-lookup"><span data-stu-id="2348e-134">Start validity of hello token</span></span> |
| <span data-ttu-id="2348e-135">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="2348e-135">Expires</span></span> |<span data-ttu-id="2348e-136">End platnosti tokenu hello</span><span class="sxs-lookup"><span data-stu-id="2348e-136">End validity of hello token</span></span> |
| <span data-ttu-id="2348e-137">SigningCredentials</span><span class="sxs-lookup"><span data-stu-id="2348e-137">SigningCredentials</span></span> |<span data-ttu-id="2348e-138">Hello klíč, který je sdílen PlayReady licenční Server castLabs licenční Server a službu tokenů zabezpečení, může to být buď symetrický, nebo asymetrický klíč.</span><span class="sxs-lookup"><span data-stu-id="2348e-138">hello key that is shared among PlayReady License Server, castLabs License Server and STS, it could be either symmetric or asymmetric key.</span></span> |

### <a name="jwt-token-in-castlabs"></a><span data-ttu-id="2348e-139">Token JWT v castLabs</span><span class="sxs-lookup"><span data-stu-id="2348e-139">JWT token in castLabs</span></span>
<span data-ttu-id="2348e-140">Hello následující tabulka popisuje token JWT v castLabs.</span><span class="sxs-lookup"><span data-stu-id="2348e-140">hello following table describes JWT token in castLabs.</span></span> 

| <span data-ttu-id="2348e-141">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="2348e-141">Name</span></span> | <span data-ttu-id="2348e-142">Popis</span><span class="sxs-lookup"><span data-stu-id="2348e-142">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2348e-143">optData</span><span class="sxs-lookup"><span data-stu-id="2348e-143">optData</span></span> |<span data-ttu-id="2348e-144">Řetězec formátu JSON, který obsahuje informace o vás.</span><span class="sxs-lookup"><span data-stu-id="2348e-144">A JSON string containing information about you.</span></span> |
| <span data-ttu-id="2348e-145">CRT</span><span class="sxs-lookup"><span data-stu-id="2348e-145">crt</span></span> |<span data-ttu-id="2348e-146">A JSON řetězec obsahující informace o hello asset, jeho licenční informace a přehrávání práva.</span><span class="sxs-lookup"><span data-stu-id="2348e-146">A JSON string containing information about hello asset, its license info and playback rights.</span></span> |
| <span data-ttu-id="2348e-147">IAT</span><span class="sxs-lookup"><span data-stu-id="2348e-147">iat</span></span> |<span data-ttu-id="2348e-148">Hello aktuální datum a čas v epoch.</span><span class="sxs-lookup"><span data-stu-id="2348e-148">hello current datetime in epoch.</span></span> |
| <span data-ttu-id="2348e-149">jti</span><span class="sxs-lookup"><span data-stu-id="2348e-149">jti</span></span> |<span data-ttu-id="2348e-150">Jedinečný identifikátor o tento token (každý token může být použit pouze jednou v systému castLabs hello).</span><span class="sxs-lookup"><span data-stu-id="2348e-150">A unique identifier about this token (every token can only be used once in hello castLabs system).</span></span> |

## <a name="sample-solution-set-up"></a><span data-ttu-id="2348e-151">Nastavení ukázkové řešení</span><span class="sxs-lookup"><span data-stu-id="2348e-151">Sample solution set up</span></span>
<span data-ttu-id="2348e-152">Hello [ukázkové řešení](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) se skládá ze dvou projektů:</span><span class="sxs-lookup"><span data-stu-id="2348e-152">hello [sample solution](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) consists of two projects:</span></span>

* <span data-ttu-id="2348e-153">Konzolovou aplikaci, kterou lze použít tooset omezení prostředek už ingestovaný DRM PlayReady a Widevine.</span><span class="sxs-lookup"><span data-stu-id="2348e-153">A console app that can be used tooset DRM restrictions on an already ingested asset, for both PlayReady and Widevine.</span></span>
* <span data-ttu-id="2348e-154">Webovou aplikaci, která předá se tokeny, které může považovat za velmi zjednodušené verzi služby tokenů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2348e-154">A Web Application that hands out tokens, which could be seen as a VERY SIMPLIFIED version of an STS.</span></span>

<span data-ttu-id="2348e-155">toouse hello konzolové aplikace:</span><span class="sxs-lookup"><span data-stu-id="2348e-155">toouse hello console application:</span></span>

1. <span data-ttu-id="2348e-156">Změnit pověření toosetup AMS hello app.config, castLabs přihlašovací údaje, konfigurace služby tokenů zabezpečení a sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="2348e-156">Change hello app.config toosetup AMS credentials, castLabs credentials, STS configuration and shared key.</span></span>
2. <span data-ttu-id="2348e-157">Odešlete prostředek do AMS.</span><span class="sxs-lookup"><span data-stu-id="2348e-157">Upload an Asset into AMS.</span></span>
3. <span data-ttu-id="2348e-158">Get hello UUID z hello nahrát Asset a změňte 32 řádku v souboru Program.cs hello:</span><span class="sxs-lookup"><span data-stu-id="2348e-158">Get hello UUID from hello uploaded Asset, and change Line 32 in hello Program.cs file:</span></span>
   
      <span data-ttu-id="2348e-159">var objIAsset = _kontextu. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1 500-80bd-b864-f1e4b62594cf"). FirstOrDefault();</span><span class="sxs-lookup"><span data-stu-id="2348e-159">var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();</span></span>
4. <span data-ttu-id="2348e-160">Použijte ID pro pojmenování hello asset systému castLabs hello (44 řádku v souboru Program.cs hello).</span><span class="sxs-lookup"><span data-stu-id="2348e-160">Use an AssetId for naming hello asset in hello castLabs system (Line 44 in hello Program.cs file).</span></span>
   
   <span data-ttu-id="2348e-161">Je nutné nastavit ID pro **castLabs**; je nutné toobe jedinečný alfanumerický řetězec.</span><span class="sxs-lookup"><span data-stu-id="2348e-161">You must set AssetId for **castLabs**; it needs toobe a unique alphanumeric string.</span></span>
5. <span data-ttu-id="2348e-162">Spuštění programu hello.</span><span class="sxs-lookup"><span data-stu-id="2348e-162">Run hello program.</span></span>

<span data-ttu-id="2348e-163">toouse hello webové aplikace (STS):</span><span class="sxs-lookup"><span data-stu-id="2348e-163">toouse hello Web Application (STS):</span></span>

1. <span data-ttu-id="2348e-164">Změna hello web.config toosetup castlabs obchodní ID, konfigurace služby tokenů zabezpečení hello a hello sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="2348e-164">Change hello web.config toosetup castlabs merchant ID, hello STS configuration and hello shared key.</span></span>
2. <span data-ttu-id="2348e-165">Nasaďte tooAzure weby.</span><span class="sxs-lookup"><span data-stu-id="2348e-165">Deploy tooAzure Websites.</span></span>
3. <span data-ttu-id="2348e-166">Přejděte toohello webu.</span><span class="sxs-lookup"><span data-stu-id="2348e-166">Navigate toohello website.</span></span>

## <a name="playing-back-a-video"></a><span data-ttu-id="2348e-167">Přehrávání videa</span><span class="sxs-lookup"><span data-stu-id="2348e-167">Playing back a video</span></span>
<span data-ttu-id="2348e-168">tooplayback video šifrován běžné šifrování (PlayReady nebo Widevine), můžete použít hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="2348e-168">tooplayback a video encrypted with common encryption (PlayReady and/or Widevine), you can use hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="2348e-169">Když spustíte hello konzolovou aplikaci, jsou opakována hello ID obsahu klíč a hello Manifest adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2348e-169">When running hello console app, hello Content Key ID and hello Manifest URL are echoed.</span></span>

1. <span data-ttu-id="2348e-170">Otevřete novou kartu a spusťte vaší služby tokenů zabezpečení: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span><span class="sxs-lookup"><span data-stu-id="2348e-170">Open a new tab and launch your STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span></span>
2. <span data-ttu-id="2348e-171">Přejděte příliš[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="2348e-171">Go too[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
3. <span data-ttu-id="2348e-172">Vložte adresu URL streamování hello.</span><span class="sxs-lookup"><span data-stu-id="2348e-172">Paste in hello streaming URL.</span></span>
4. <span data-ttu-id="2348e-173">Klikněte na tlačítko hello **pokročilé možnosti** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="2348e-173">Click hello **Advanced Options** checkbox.</span></span>
5. <span data-ttu-id="2348e-174">V hello **ochrany** rozevíracího seznamu vyberte PlayReady nebo Widevine.</span><span class="sxs-lookup"><span data-stu-id="2348e-174">In hello **Protection** dropdown, select PlayReady and/or Widevine.</span></span>
6. <span data-ttu-id="2348e-175">Vložte hello token, který jste dostali od vaší služby tokenů zabezpečení v textovém poli hello Token.</span><span class="sxs-lookup"><span data-stu-id="2348e-175">Paste hello token that you got from your STS in hello Token textbox.</span></span> 
   
   <span data-ttu-id="2348e-176">Hello castLab licenční server nemusí hello "nosiče =" předponu před hello token.</span><span class="sxs-lookup"><span data-stu-id="2348e-176">hello castLab license server does not need hello “Bearer=” prefix in front of hello token.</span></span> <span data-ttu-id="2348e-177">Proto prosím odebrat před odesláním hello token.</span><span class="sxs-lookup"><span data-stu-id="2348e-177">So please remove that before submitting hello token.</span></span>
7. <span data-ttu-id="2348e-178">Aktualizujte hello přehrávač.</span><span class="sxs-lookup"><span data-stu-id="2348e-178">Update hello player.</span></span>
8. <span data-ttu-id="2348e-179">měli přehrávání videa Hello.</span><span class="sxs-lookup"><span data-stu-id="2348e-179">hello video should be playing.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="2348e-180">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="2348e-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2348e-181">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="2348e-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

