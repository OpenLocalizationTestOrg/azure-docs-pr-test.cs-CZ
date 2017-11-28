---
title: "obsah aaaPublish Azure Media Services pomocí rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak toocreate Lokátor, který je použité toobuild adresu URL pro streamování. Ukázky kódu jsou napsané v jazyce C# a použít hello sady Media Services SDK pro .NET."
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
ms.openlocfilehash: c941cd93c252a96e66546cce2793bb426afac059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-azure-media-services-content-using-net"></a><span data-ttu-id="1e843-104">Publikovat obsah Azure Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1e843-104">Publish Azure Media Services content using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e843-105">REST</span><span class="sxs-lookup"><span data-stu-id="1e843-105">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> * [<span data-ttu-id="1e843-106">.NET</span><span class="sxs-lookup"><span data-stu-id="1e843-106">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="1e843-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1e843-107">Portal</span></span>](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="1e843-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="1e843-108">Overview</span></span>
<span data-ttu-id="1e843-109">Můžete datového proudu s adaptivní přenosovou rychlostí sady souborů MP4 vytvořením Lokátor streamování OnDemand a vytvoření adresy URL streamování.</span><span class="sxs-lookup"><span data-stu-id="1e843-109">You can stream an adaptive bitrate MP4 set by creating an OnDemand streaming locator and building a streaming URL.</span></span> <span data-ttu-id="1e843-110">Hello [kódování prostředek](media-services-encode-asset.md) téma ukazuje, jak nastavit tooencode do MP4 adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="1e843-110">hello [encoding an asset](media-services-encode-asset.md) topic shows how tooencode into an adaptive bitrate MP4 set.</span></span> 

> [!NOTE]
> <span data-ttu-id="1e843-111">Pokud váš obsah je zašifrován, konfigurace zásad doručení assetu (jak je popsáno v [to](media-services-dotnet-configure-asset-delivery-policy.md) tématu) před vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="1e843-111">If your content is encrypted, configure asset delivery policy (as described in [this](media-services-dotnet-configure-asset-delivery-policy.md) topic) before creating a locator.</span></span> 
> 
> 

<span data-ttu-id="1e843-112">Můžete také použít toobuild Lokátor tento bod tooMP4 soubory, které lze progresivně stáhnout – adresy URL pro streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="1e843-112">You can also use an OnDemand streaming locator toobuild URLs that point tooMP4 files that can be progressively downloaded.</span></span>  

<span data-ttu-id="1e843-113">Toto téma ukazuje, jak toocreate asset a sestavení Smooth, MPEG DASH a adresy URL streamování HLS streamování toopublish Lokátor OnDemand.</span><span class="sxs-lookup"><span data-stu-id="1e843-113">This topic shows how toocreate an OnDemand streaming locator toopublish your asset and build a Smooth, MPEG DASH, and HLS streaming URLs.</span></span> <span data-ttu-id="1e843-114">Také ukazuje aktivní toobuild adresa URL progresivního stahování.</span><span class="sxs-lookup"><span data-stu-id="1e843-114">It also shows hot toobuild progressive download URLs.</span></span> 

## <a name="create-an-ondemand-streaming-locator"></a><span data-ttu-id="1e843-115">Vytvořit lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="1e843-115">Create an OnDemand streaming locator</span></span>
<span data-ttu-id="1e843-116">toocreate hello Lokátor streamování OnDemand a získání adres URL, musíte toodo hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="1e843-116">toocreate hello OnDemand streaming locator and get URLs, you need toodo hello following things:</span></span>

1. <span data-ttu-id="1e843-117">Pokud obsah hello je zašifrován, definujte zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="1e843-117">If hello content is encrypted, define an access policy.</span></span>
2. <span data-ttu-id="1e843-118">Vytvořte Lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="1e843-118">Create an OnDemand streaming locator.</span></span>
3. <span data-ttu-id="1e843-119">Pokud máte v plánu toostream, získáte hello vysílání datového proudu souboru manifestu (.ism) v hello asset.</span><span class="sxs-lookup"><span data-stu-id="1e843-119">If you plan toostream, get hello streaming manifest file (.ism) in hello asset.</span></span> 
   
   <span data-ttu-id="1e843-120">Pokud máte v plánu tooprogressively stahování, získáte hello názvy souborů MP4 v hello asset.</span><span class="sxs-lookup"><span data-stu-id="1e843-120">If you plan tooprogressively download, get hello names of MP4 files in hello asset.</span></span>  
4. <span data-ttu-id="1e843-121">Vytvoření souboru manifestu toohello adresy URL nebo soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="1e843-121">Build URLs toohello manifest file or MP4 files.</span></span> 


>[!NOTE]
><span data-ttu-id="1e843-122">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="1e843-122">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="1e843-123">Použití hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1e843-123">Use hello same policy ID if you are always using hello same days / access permissions.</span></span> <span data-ttu-id="1e843-124">Například zásady pro lokátory, které jsou určené tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="1e843-124">For example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="1e843-125">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="1e843-125">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

### <a name="use-media-services-net-sdk"></a><span data-ttu-id="1e843-126">Používání Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="1e843-126">Use Media Services .NET SDK</span></span>
<span data-ttu-id="1e843-127">Vytvoření datových proudů adres URL</span><span class="sxs-lookup"><span data-stu-id="1e843-127">Build Streaming URLs</span></span> 

    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator toohello streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on hello locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference toohello streaming manifest file from hello  
        // collection of files in hello asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL toohello manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL toomanifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL toomanifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL toomanifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

<span data-ttu-id="1e843-128">výstupy Hello:</span><span class="sxs-lookup"><span data-stu-id="1e843-128">hello outputs:</span></span>

    URL toomanifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL toomanifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL toomanifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> <span data-ttu-id="1e843-129">Můžete také Streamovat obsah pomocí připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="1e843-129">You can also stream your content over an SSL connection.</span></span> <span data-ttu-id="1e843-130">toodo to přístupu, ujistěte se, spustíte adresy URL streamování s protokolem HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1e843-130">toodo this approach, make sure your streaming URLs start with HTTPS.</span></span> <span data-ttu-id="1e843-131">V současné době nepodporuje AMS SSL s vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="1e843-131">Currently, AMS doesn’t support SSL with custom domains.</span></span>
> 
> 

<span data-ttu-id="1e843-132">Vytvoření adres URL progresivního stahování</span><span class="sxs-lookup"><span data-stu-id="1e843-132">Build progressive download URLs</span></span> 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator toohello asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on hello locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL toohello MP4 files. Use this tooprogressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

<span data-ttu-id="1e843-133">výstupy Hello:</span><span class="sxs-lookup"><span data-stu-id="1e843-133">hello outputs:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a><span data-ttu-id="1e843-134">Pomocí rozšíření Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="1e843-134">Use Media Services .NET SDK Extensions</span></span>
<span data-ttu-id="1e843-135">Hello následující kód volá .NET SDK rozšíření metody, které vytvořit Lokátor a generovat hello technologie Smooth Streaming, HLS a adresy URL pro MPEG-DASH pro adaptivní streamování.</span><span class="sxs-lookup"><span data-stu-id="1e843-135">hello following code calls .NET SDK extensions methods that create a locator and generate hello Smooth Streaming, HLS, and MPEG-DASH URLs for adaptive streaming.</span></span>

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get hello streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


## <a name="media-services-learning-paths"></a><span data-ttu-id="1e843-136">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="1e843-136">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1e843-137">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="1e843-137">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="1e843-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e843-138">Next steps</span></span>
* [<span data-ttu-id="1e843-139">Stáhnout prostředky</span><span class="sxs-lookup"><span data-stu-id="1e843-139">Download assets</span></span>](media-services-deliver-asset-download.md)
* [<span data-ttu-id="1e843-140">Konfigurace zásad doručení assetu</span><span class="sxs-lookup"><span data-stu-id="1e843-140">Configure asset delivery policy</span></span>](media-services-dotnet-configure-asset-delivery-policy.md)

