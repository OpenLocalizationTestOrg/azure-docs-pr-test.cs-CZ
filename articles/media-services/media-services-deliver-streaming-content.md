---
title: "Publikovat obsah Azure Media Services pomocí rozhraní .NET | Microsoft Docs"
description: "Naučte se vytvořit lokátor, který je použit k vytvoření adresy URL streamování. Ukázky kódu jsou napsané v jazyce C# a pomocí sady Media Services SDK pro .NET."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 2bcb012eef84faa7c1e13ed22e88e45e4300ed54
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="publish-azure-media-services-content-using-net"></a><span data-ttu-id="6fcc4-104">Publikovat obsah Azure Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="6fcc4-104">Publish Azure Media Services content using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6fcc4-105">REST</span><span class="sxs-lookup"><span data-stu-id="6fcc4-105">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> * [<span data-ttu-id="6fcc4-106">.NET</span><span class="sxs-lookup"><span data-stu-id="6fcc4-106">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="6fcc4-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6fcc4-107">Portal</span></span>](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="6fcc4-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="6fcc4-108">Overview</span></span>
<span data-ttu-id="6fcc4-109">Můžete datového proudu s adaptivní přenosovou rychlostí sady souborů MP4 vytvořením Lokátor streamování OnDemand a vytvoření adresy URL streamování.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-109">You can stream an adaptive bitrate MP4 set by creating an OnDemand streaming locator and building a streaming URL.</span></span> <span data-ttu-id="6fcc4-110">[Kódování prostředek](media-services-encode-asset.md) téma ukazuje, jak ke kódování do sady souborů MP4 adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-110">The [encoding an asset](media-services-encode-asset.md) topic shows how to encode into an adaptive bitrate MP4 set.</span></span> 

> [!NOTE]
> <span data-ttu-id="6fcc4-111">Pokud váš obsah je zašifrován, konfigurace zásad doručení assetu (jak je popsáno v [to](media-services-dotnet-configure-asset-delivery-policy.md) tématu) před vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-111">If your content is encrypted, configure asset delivery policy (as described in [this](media-services-dotnet-configure-asset-delivery-policy.md) topic) before creating a locator.</span></span> 
> 
> 

<span data-ttu-id="6fcc4-112">Lokátor streamování OnDemand. můžete také použít k vytvoření adresy URL, které odkazují na soubory MP4, které lze progresivně stáhnout.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-112">You can also use an OnDemand streaming locator to build URLs that point to MP4 files that can be progressively downloaded.</span></span>  

<span data-ttu-id="6fcc4-113">Toto téma ukazuje, jak vytvořit k publikování asset a vytvoření Smooth, MPEG DASH a adresy URL streamování HLS Lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-113">This topic shows how to create an OnDemand streaming locator to publish your asset and build a Smooth, MPEG DASH, and HLS streaming URLs.</span></span> <span data-ttu-id="6fcc4-114">Také ukazuje aktivní pro vytvoření adres URL progresivního stahování.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-114">It also shows hot to build progressive download URLs.</span></span> 

## <a name="create-an-ondemand-streaming-locator"></a><span data-ttu-id="6fcc4-115">Vytvořit lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-115">Create an OnDemand streaming locator</span></span>
<span data-ttu-id="6fcc4-116">Pokud chcete vytvořit lokátor streamování OnDemand a získání adres URL, musíte udělat následující věci:</span><span class="sxs-lookup"><span data-stu-id="6fcc4-116">To create the OnDemand streaming locator and get URLs, you need to do the following things:</span></span>

1. <span data-ttu-id="6fcc4-117">Pokud je obsah šifrovat, definujte zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-117">If the content is encrypted, define an access policy.</span></span>
2. <span data-ttu-id="6fcc4-118">Vytvořte Lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-118">Create an OnDemand streaming locator.</span></span>
3. <span data-ttu-id="6fcc4-119">Pokud máte v plánu k vysílání datového proudu, získáte streamování souboru manifestu (.ism) v prostředku.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-119">If you plan to stream, get the streaming manifest file (.ism) in the asset.</span></span> 
   
   <span data-ttu-id="6fcc4-120">Pokud budete chtít progresivně stahovat, získáte názvy soubory MP4 v prostředku.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-120">If you plan to progressively download, get the names of MP4 files in the asset.</span></span>  
4. <span data-ttu-id="6fcc4-121">Sestavení adresy URL k souboru manifestu nebo soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-121">Build URLs to the manifest file or MP4 files.</span></span> 


>[!NOTE]
><span data-ttu-id="6fcc4-122">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="6fcc4-122">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="6fcc4-123">Stejné ID zásady použijte, pokud vždy používají stejné dny / přístupová oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-123">Use the same policy ID if you are always using the same days / access permissions.</span></span> <span data-ttu-id="6fcc4-124">Například zásady pro lokátory, které jsou určeny k zůstat na místě po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="6fcc4-124">For example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="6fcc4-125">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-125">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

### <a name="use-media-services-net-sdk"></a><span data-ttu-id="6fcc4-126">Používání Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="6fcc4-126">Use Media Services .NET SDK</span></span>
<span data-ttu-id="6fcc4-127">Vytvoření datových proudů adres URL</span><span class="sxs-lookup"><span data-stu-id="6fcc4-127">Build Streaming URLs</span></span> 

    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

<span data-ttu-id="6fcc4-128">Výstupy:</span><span class="sxs-lookup"><span data-stu-id="6fcc4-128">The outputs:</span></span>

    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> <span data-ttu-id="6fcc4-129">Můžete také Streamovat obsah pomocí připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-129">You can also stream your content over an SSL connection.</span></span> <span data-ttu-id="6fcc4-130">Chcete-li provést tento postup, ujistěte se, spustíte adresy URL streamování s protokolem HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-130">To do this approach, make sure your streaming URLs start with HTTPS.</span></span> <span data-ttu-id="6fcc4-131">V současné době nepodporuje AMS SSL s vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-131">Currently, AMS doesn’t support SSL with custom domains.</span></span>
> 
> 

<span data-ttu-id="6fcc4-132">Vytvoření adres URL progresivního stahování</span><span class="sxs-lookup"><span data-stu-id="6fcc4-132">Build progressive download URLs</span></span> 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

<span data-ttu-id="6fcc4-133">Výstupy:</span><span class="sxs-lookup"><span data-stu-id="6fcc4-133">The outputs:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a><span data-ttu-id="6fcc4-134">Pomocí rozšíření Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="6fcc4-134">Use Media Services .NET SDK Extensions</span></span>
<span data-ttu-id="6fcc4-135">Následující kód volá metody rozšíření .NET SDK, které vytvořit Lokátor a generovat technologie Smooth Streaming, HLS a adres URL pro MPEG-DASH pro adaptivní streamování.</span><span class="sxs-lookup"><span data-stu-id="6fcc4-135">The following code calls .NET SDK extensions methods that create a locator and generate the Smooth Streaming, HLS, and MPEG-DASH URLs for adaptive streaming.</span></span>

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


## <a name="media-services-learning-paths"></a><span data-ttu-id="6fcc4-136">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="6fcc4-136">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6fcc4-137">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="6fcc4-137">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6fcc4-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fcc4-138">Next steps</span></span>
* [<span data-ttu-id="6fcc4-139">Stáhnout prostředky</span><span class="sxs-lookup"><span data-stu-id="6fcc4-139">Download assets</span></span>](media-services-deliver-asset-download.md)
* [<span data-ttu-id="6fcc4-140">Konfigurace zásad doručení assetu</span><span class="sxs-lookup"><span data-stu-id="6fcc4-140">Configure asset delivery policy</span></span>](media-services-dotnet-configure-asset-delivery-policy.md)

