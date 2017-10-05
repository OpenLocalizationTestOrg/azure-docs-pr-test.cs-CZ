---
title: "Nejčastější dotazy k Azure Media Services | Microsoft Docs"
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
ms.openlocfilehash: 48f3924d44a084d61c1d38002cd5098094001acb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="frequently-asked-questions"></a><span data-ttu-id="2d72b-103">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2d72b-103">Frequently asked questions</span></span>

<span data-ttu-id="2d72b-104">Tento článek řeší nejčastější dotazy aktivováno komunit uživatelů Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="2d72b-104">This article addresses frequently asked questions raised by the Azure Media Services (AMS) user community.</span></span>

## <a name="general-ams-faqs"></a><span data-ttu-id="2d72b-105">Nejčastější dotazy k AMS obecné</span><span class="sxs-lookup"><span data-stu-id="2d72b-105">General AMS FAQs</span></span>
<span data-ttu-id="2d72b-106">Otázka: jak můžete škálovat indexování?</span><span class="sxs-lookup"><span data-stu-id="2d72b-106">Q: How do you scale indexing?</span></span>

<span data-ttu-id="2d72b-107">Odpověď: vyhrazené jednotky jsou stejné pro úlohy kódování a indexování.</span><span class="sxs-lookup"><span data-stu-id="2d72b-107">A: The reserved units are the same for Encoding and Indexing tasks.</span></span> <span data-ttu-id="2d72b-108">Postupujte podle pokynů na [postup škálování jednotek rezervovaných pro kódování](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2d72b-108">Follow instructions on [How to Scale Encoding Reserved Units](media-services-scale-media-processing-overview.md).</span></span> <span data-ttu-id="2d72b-109">**Poznámka:** Indexer výkonu není ovlivněn vyhrazený typ jednotky.</span><span class="sxs-lookup"><span data-stu-id="2d72b-109">**Note** that Indexer performance is not affected by Reserved Unit Type.</span></span>

<span data-ttu-id="2d72b-110">Otázka: I nahrán, kódování a publikování videa.</span><span class="sxs-lookup"><span data-stu-id="2d72b-110">Q: I uploaded, encoded, and published a video.</span></span> <span data-ttu-id="2d72b-111">Co by být z důvodu video nehraje při pokusu Streamovat ho?</span><span class="sxs-lookup"><span data-stu-id="2d72b-111">What would be the reason the video does not play when I try to stream it?</span></span>

<span data-ttu-id="2d72b-112">Odpověď: jeden z nejčastějších důvodů je koncový bod streamování, ze kterého se pokoušíte přehrávání v nemáte **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="2d72b-112">A: One of the most common reasons is you do not have the streaming endpoint from which you are trying to playback in the **Running** state.</span></span>  

<span data-ttu-id="2d72b-113">Otázka: je možné dělat skládání na živý datový proud?</span><span class="sxs-lookup"><span data-stu-id="2d72b-113">Q: Can I do compositing on a live stream?</span></span>

<span data-ttu-id="2d72b-114">Odpověď: skládání na živé datové proudy není aktuálně nabízena v Azure Media Services, budete muset předem vytvořit ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2d72b-114">A: Compositing on live streams is currently not offered in Azure Media Services, so you would need to pre-compose on your computer.</span></span>

<span data-ttu-id="2d72b-115">Otázka: je možné použít Azure CDN s živým streamováním?</span><span class="sxs-lookup"><span data-stu-id="2d72b-115">Q: Can I use Azure CDN with Live Streaming?</span></span>

<span data-ttu-id="2d72b-116">Odpověď: Media Services podporuje integraci se službou Azure CDN (Další informace najdete v tématu [jak spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)).</span><span class="sxs-lookup"><span data-stu-id="2d72b-116">A: Media Services supports integration with Azure CDN (for more information, see [How to Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)).</span></span>  <span data-ttu-id="2d72b-117">Můžete použít živé streamování s CDN.</span><span class="sxs-lookup"><span data-stu-id="2d72b-117">You can use Live streaming with CDN.</span></span> <span data-ttu-id="2d72b-118">Azure Media Services poskytuje technologie Smooth Streaming, MPEG-DASH v HLS výstupy.</span><span class="sxs-lookup"><span data-stu-id="2d72b-118">Azure Media Services provides Smooth Streaming, HLS and MPEG-DASH outputs.</span></span> <span data-ttu-id="2d72b-119">Všechny tyto formáty prostřednictvím protokolu HTTP pro přenos dat a získejte výhody ukládání do mezipaměti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="2d72b-119">All these formats use HTTP for transferring data and get benefits of HTTP caching.</span></span> <span data-ttu-id="2d72b-120">V živé streamování skutečné video nebo zvuk dat je rozdělen na fragmenty a tento jednotlivé fragmenty získat uložené v mezipaměti v CDN.</span><span class="sxs-lookup"><span data-stu-id="2d72b-120">In live streaming actual video/audio data is divided to fragments and this individual fragments get cached in CDN.</span></span> <span data-ttu-id="2d72b-121">Pouze data je nutné aktualizovat jsou manifestu data.</span><span class="sxs-lookup"><span data-stu-id="2d72b-121">Only data needs to be refreshed is the manifest data.</span></span> <span data-ttu-id="2d72b-122">CDN se pravidelně aktualizuje manifestu data.</span><span class="sxs-lookup"><span data-stu-id="2d72b-122">CDN periodically refreshes manifest data.</span></span>

<span data-ttu-id="2d72b-123">Otázka: nemá Azure Media services podporuje ukládání Image?</span><span class="sxs-lookup"><span data-stu-id="2d72b-123">Q: Does Azure Media services support storing images?</span></span>

<span data-ttu-id="2d72b-124">Odpověď: Pokud hledáte pouze ukládání obrázků JPEG nebo PNG, byste měli mít v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2d72b-124">A: If you are just looking to store JPEG or PNG images, you should keep those in Azure Blob Storage.</span></span> <span data-ttu-id="2d72b-125">Neexistuje žádné výhody je vložíte do vašeho účtu Media Services, pokud chcete zachovat je přidružený k Video nebo zvuk prostředky.</span><span class="sxs-lookup"><span data-stu-id="2d72b-125">There is no benefit to putting them in your Media Services account unless you want to keep them associated with your Video or Audio Assets.</span></span> <span data-ttu-id="2d72b-126">Nebo pokud bude možná potřeba použít Image jako překryvy v kodér videa. Media Encoder Standard podporuje překrytí bitové kopie nad videa, a co obsahuje JPEG a PNG jako podporovaný vstupní formáty.</span><span class="sxs-lookup"><span data-stu-id="2d72b-126">Or if you might have a need to use the images as overlays in the video encoder.Media Encoder Standard supports overlaying images on top of videos, and that is what it lists JPEG and PNG as supported input formats.</span></span> <span data-ttu-id="2d72b-127">Další informace najdete v tématu [vytváření překryvy](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="2d72b-127">For more information, see [Creating Overlays](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

<span data-ttu-id="2d72b-128">Otázka: zkopírování prostředků z jednoho účtu Media Services do jiného.</span><span class="sxs-lookup"><span data-stu-id="2d72b-128">Q: How can I copy assets from one Media Services account to another.</span></span>

<span data-ttu-id="2d72b-129">Odpověď: pro kopírování prostředků z jednoho účtu Media Services do jiného pomocí rozhraní .NET, použijte [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) rozšíření metoda dostupná ve [rozšíření Azure Media Services .NET SDK](https://github.com/Azure/azure-sdk-for-media-services-extensions/) úložiště.</span><span class="sxs-lookup"><span data-stu-id="2d72b-129">A: To copy assets from one Media Services account to another using .NET, use [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extension method available in the [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) repository.</span></span> <span data-ttu-id="2d72b-130">Další informace najdete v tématu [to](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) fórum pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="2d72b-130">For more information, see [this](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread.</span></span>

<span data-ttu-id="2d72b-131">Otázka: co jsou podporovanými znaky pro pojmenovávání souborů při práci s AMS?</span><span class="sxs-lookup"><span data-stu-id="2d72b-131">Q: What are the supported characters for naming files when working with AMS?</span></span>

<span data-ttu-id="2d72b-132">Odpověď: Media Services použije hodnotu vlastnosti IAssetFile.Name při sestavování adresy URL pro streamování obsah (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech.</span><span class="sxs-lookup"><span data-stu-id="2d72b-132">A: Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="2d72b-133">Hodnota **název** vlastnost nemůže mít žádné z následujících [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="2d72b-133">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="2d72b-134">Navíc může existovat pouze jedna '. "</span><span class="sxs-lookup"><span data-stu-id="2d72b-134">Also, there can only be one ‘.’</span></span> <span data-ttu-id="2d72b-135">pro příponu názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="2d72b-135">for the file name extension.</span></span>

<span data-ttu-id="2d72b-136">Otázka: jak se připojit pomocí REST?</span><span class="sxs-lookup"><span data-stu-id="2d72b-136">Q: How to connect using REST?</span></span>

<span data-ttu-id="2d72b-137">Odpověď: informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="2d72b-137">A: For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> <span data-ttu-id="2d72b-138">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="2d72b-138">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="2d72b-139">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="2d72b-139">You must make subsequent calls to the new URI.</span></span> 

<span data-ttu-id="2d72b-140">Otázka: jak můžete během procesu kódování otočit video.</span><span class="sxs-lookup"><span data-stu-id="2d72b-140">Q: How can I rotate a video during the encoding process.</span></span>

<span data-ttu-id="2d72b-141">Odpověď: na [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) podporuje oběh podle úhly 90, 180 nebo 270.</span><span class="sxs-lookup"><span data-stu-id="2d72b-141">A: The [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 90/180/270.</span></span> <span data-ttu-id="2d72b-142">Výchozí chování je "Auto", kde ji pokusí zjistit otočení metadata v souboru MP4 nebo MOV příchozí a kompenzovat ho.</span><span class="sxs-lookup"><span data-stu-id="2d72b-142">The default behavior is "Auto", where it tries to detect the rotation metadata in the incoming MP4/MOV file and compensate for it.</span></span> <span data-ttu-id="2d72b-143">Patří **zdroje** element pro jedno z přednastavení json definované [sem](media-services-mes-presets-overview.md):</span><span class="sxs-lookup"><span data-stu-id="2d72b-143">Include the following **Sources** element to one of the json presets defined [here](media-services-mes-presets-overview.md):</span></span>

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


## <a name="media-services-learning-paths"></a><span data-ttu-id="2d72b-144">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="2d72b-144">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2d72b-145">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="2d72b-145">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
