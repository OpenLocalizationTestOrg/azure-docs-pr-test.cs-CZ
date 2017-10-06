---
title: "aaaAzure Media Services nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy (FAQ)"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 6d48a5c1291f3c2559d8445921d571718d0a0a6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions"></a><span data-ttu-id="29e2f-103">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="29e2f-103">Frequently asked questions</span></span>

<span data-ttu-id="29e2f-104">Tento článek řeší nejčastější dotazy aktivováno komunit uživatelů hello Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="29e2f-104">This article addresses frequently asked questions raised by hello Azure Media Services (AMS) user community.</span></span>

## <a name="general-ams-faqs"></a><span data-ttu-id="29e2f-105">Nejčastější dotazy k AMS obecné</span><span class="sxs-lookup"><span data-stu-id="29e2f-105">General AMS FAQs</span></span>
<span data-ttu-id="29e2f-106">Otázka: jak můžete škálovat indexování?</span><span class="sxs-lookup"><span data-stu-id="29e2f-106">Q: How do you scale indexing?</span></span>

<span data-ttu-id="29e2f-107">Odpověď: hello vyhrazené jednotky jsou hello stejné pro kódování a indexování úlohy.</span><span class="sxs-lookup"><span data-stu-id="29e2f-107">A: hello reserved units are hello same for Encoding and Indexing tasks.</span></span> <span data-ttu-id="29e2f-108">Postupujte podle pokynů na [jak tooScale jednotky rezervované pro kódování](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="29e2f-108">Follow instructions on [How tooScale Encoding Reserved Units](media-services-scale-media-processing-overview.md).</span></span> <span data-ttu-id="29e2f-109">**Poznámka:** Indexer výkonu není ovlivněn vyhrazený typ jednotky.</span><span class="sxs-lookup"><span data-stu-id="29e2f-109">**Note** that Indexer performance is not affected by Reserved Unit Type.</span></span>

<span data-ttu-id="29e2f-110">Otázka: I nahrán, kódování a publikování videa.</span><span class="sxs-lookup"><span data-stu-id="29e2f-110">Q: I uploaded, encoded, and published a video.</span></span> <span data-ttu-id="29e2f-111">Co by hello důvod hello video nehraje při toostream ho?</span><span class="sxs-lookup"><span data-stu-id="29e2f-111">What would be hello reason hello video does not play when I try toostream it?</span></span>

<span data-ttu-id="29e2f-112">Odpověď: jeden z hello nejčastějším důvodů, proč je nemáte hello koncový bod, ze kterého se pokoušíte tooplayback v hello streamování **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="29e2f-112">A: One of hello most common reasons is you do not have hello streaming endpoint from which you are trying tooplayback in hello **Running** state.</span></span>  

<span data-ttu-id="29e2f-113">Otázka: je možné dělat skládání na živý datový proud?</span><span class="sxs-lookup"><span data-stu-id="29e2f-113">Q: Can I do compositing on a live stream?</span></span>

<span data-ttu-id="29e2f-114">A: skládání na datových proudů za provozu není aktuálně nabízí ve službě Azure Media Services, měli byste toopre-tvoří ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="29e2f-114">A: Compositing on live streams is currently not offered in Azure Media Services, so you would need toopre-compose on your computer.</span></span>

<span data-ttu-id="29e2f-115">Otázka: je možné použít Azure CDN s živým streamováním?</span><span class="sxs-lookup"><span data-stu-id="29e2f-115">Q: Can I use Azure CDN with Live Streaming?</span></span>

<span data-ttu-id="29e2f-116">Odpověď: Media Services podporuje integraci se službou Azure CDN (Další informace najdete v tématu [jak tooManage koncových bodů streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)).</span><span class="sxs-lookup"><span data-stu-id="29e2f-116">A: Media Services supports integration with Azure CDN (for more information, see [How tooManage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)).</span></span>  <span data-ttu-id="29e2f-117">Můžete použít živé streamování s CDN.</span><span class="sxs-lookup"><span data-stu-id="29e2f-117">You can use Live streaming with CDN.</span></span> <span data-ttu-id="29e2f-118">Azure Media Services poskytuje technologie Smooth Streaming, MPEG-DASH v HLS výstupy.</span><span class="sxs-lookup"><span data-stu-id="29e2f-118">Azure Media Services provides Smooth Streaming, HLS and MPEG-DASH outputs.</span></span> <span data-ttu-id="29e2f-119">Všechny tyto formáty prostřednictvím protokolu HTTP pro přenos dat a získejte výhody ukládání do mezipaměti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="29e2f-119">All these formats use HTTP for transferring data and get benefits of HTTP caching.</span></span> <span data-ttu-id="29e2f-120">V za provozu, streamování skutečná data video nebo zvuk je rozdělený toofragments a tento jednotlivé fragmenty získat uložené v mezipaměti v CDN.</span><span class="sxs-lookup"><span data-stu-id="29e2f-120">In live streaming actual video/audio data is divided toofragments and this individual fragments get cached in CDN.</span></span> <span data-ttu-id="29e2f-121">Pouze data potřebám toobe aktualizovat jsou hello manifestu data.</span><span class="sxs-lookup"><span data-stu-id="29e2f-121">Only data needs toobe refreshed is hello manifest data.</span></span> <span data-ttu-id="29e2f-122">CDN se pravidelně aktualizuje manifestu data.</span><span class="sxs-lookup"><span data-stu-id="29e2f-122">CDN periodically refreshes manifest data.</span></span>

<span data-ttu-id="29e2f-123">Otázka: nemá Azure Media services podporuje ukládání Image?</span><span class="sxs-lookup"><span data-stu-id="29e2f-123">Q: Does Azure Media services support storing images?</span></span>

<span data-ttu-id="29e2f-124">Odpověď: Pokud hledáte pouze toostore JPEG nebo PNG obrázků, byste měli mít v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="29e2f-124">A: If you are just looking toostore JPEG or PNG images, you should keep those in Azure Blob Storage.</span></span> <span data-ttu-id="29e2f-125">Neexistuje žádné tooputting výhody, které služby Media Services je účet, pokud chcete, aby tookeep, který je přidružený k Video nebo zvuk prostředky.</span><span class="sxs-lookup"><span data-stu-id="29e2f-125">There is no benefit tooputting them in your Media Services account unless you want tookeep them associated with your Video or Audio Assets.</span></span> <span data-ttu-id="29e2f-126">Nebo pokud bude možná potřeba toouse hello bitové kopie jako překryvy v kodér videa hello. Media Encoder Standard podporuje překrytí bitové kopie nad videa, a co obsahuje JPEG a PNG jako podporovaný vstupní formáty.</span><span class="sxs-lookup"><span data-stu-id="29e2f-126">Or if you might have a need toouse hello images as overlays in hello video encoder.Media Encoder Standard supports overlaying images on top of videos, and that is what it lists JPEG and PNG as supported input formats.</span></span> <span data-ttu-id="29e2f-127">Další informace najdete v tématu [vytváření překryvy](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="29e2f-127">For more information, see [Creating Overlays](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

<span data-ttu-id="29e2f-128">Otázka: jak můžete zkopírovat prostředků z jednoho tooanother účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="29e2f-128">Q: How can I copy assets from one Media Services account tooanother.</span></span>

<span data-ttu-id="29e2f-129">Odpověď: toocopy prostředků z jednoho tooanother účtu Media Services pomocí rozhraní .NET, použijte [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) rozšíření metoda dostupná ve hello [rozšíření Azure Media Services .NET SDK](https://github.com/Azure/azure-sdk-for-media-services-extensions/) úložiště.</span><span class="sxs-lookup"><span data-stu-id="29e2f-129">A: toocopy assets from one Media Services account tooanother using .NET, use [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extension method available in hello [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) repository.</span></span> <span data-ttu-id="29e2f-130">Další informace najdete v tématu [to](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) fórum pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="29e2f-130">For more information, see [this](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread.</span></span>

<span data-ttu-id="29e2f-131">Otázka: co jsou hello podporovány znaků pro pojmenovávání souborů při práci s AMS?</span><span class="sxs-lookup"><span data-stu-id="29e2f-131">Q: What are hello supported characters for naming files when working with AMS?</span></span>

<span data-ttu-id="29e2f-132">Odpověď: Media Services použije hodnotu hello hello vlastnost IAssetFile.Name při sestavování adresy URL pro hello streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech.</span><span class="sxs-lookup"><span data-stu-id="29e2f-132">A: Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="29e2f-133">Hello hodnotu hello **název** vlastnost nemůže mít žádné z následujících hello [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="29e2f-133">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="29e2f-134">Navíc může existovat pouze jedna '. "</span><span class="sxs-lookup"><span data-stu-id="29e2f-134">Also, there can only be one ‘.’</span></span> <span data-ttu-id="29e2f-135">pro hello příponu názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="29e2f-135">for hello file name extension.</span></span>

<span data-ttu-id="29e2f-136">Otázka: pomocí tooconnect jak REST?</span><span class="sxs-lookup"><span data-stu-id="29e2f-136">Q: How tooconnect using REST?</span></span>

<span data-ttu-id="29e2f-137">Odpověď: Další informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="29e2f-137">A: For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> <span data-ttu-id="29e2f-138">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="29e2f-138">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="29e2f-139">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="29e2f-139">You must make subsequent calls toohello new URI.</span></span> 

<span data-ttu-id="29e2f-140">Otázka: jak můžete otočit video během hello kódování procesu.</span><span class="sxs-lookup"><span data-stu-id="29e2f-140">Q: How can I rotate a video during hello encoding process.</span></span>

<span data-ttu-id="29e2f-141">Odpověď: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) podporuje oběh podle úhly 90, 180 nebo 270.</span><span class="sxs-lookup"><span data-stu-id="29e2f-141">A: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 90/180/270.</span></span> <span data-ttu-id="29e2f-142">Hello výchozí chování je "Auto", kde se pokusí toodetect hello otočení metadata v souboru MP4 nebo MOV příchozí hello a kompenzovat ho.</span><span class="sxs-lookup"><span data-stu-id="29e2f-142">hello default behavior is "Auto", where it tries toodetect hello rotation metadata in hello incoming MP4/MOV file and compensate for it.</span></span> <span data-ttu-id="29e2f-143">Zahrnout hello následující **zdroje** element tooone přednastavení json hello definované [sem](media-services-mes-presets-overview.md):</span><span class="sxs-lookup"><span data-stu-id="29e2f-143">Include hello following **Sources** element tooone of hello json presets defined [here](media-services-mes-presets-overview.md):</span></span>

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a><span data-ttu-id="29e2f-144">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="29e2f-144">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="29e2f-145">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="29e2f-145">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
