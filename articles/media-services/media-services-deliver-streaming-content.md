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
# <a name="publish-azure-media-services-content-using-net"></a>Publikovat obsah Azure Media Services pomocí rozhraní .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [Azure Portal](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a>Přehled
Můžete datového proudu s adaptivní přenosovou rychlostí sady souborů MP4 vytvořením Lokátor streamování OnDemand a vytvoření adresy URL streamování. Hello [kódování prostředek](media-services-encode-asset.md) téma ukazuje, jak nastavit tooencode do MP4 adaptivní přenosovou rychlostí. 

> [!NOTE]
> Pokud váš obsah je zašifrován, konfigurace zásad doručení assetu (jak je popsáno v [to](media-services-dotnet-configure-asset-delivery-policy.md) tématu) před vytvořením lokátoru. 
> 
> 

Můžete také použít toobuild Lokátor tento bod tooMP4 soubory, které lze progresivně stáhnout – adresy URL pro streamování OnDemand.  

Toto téma ukazuje, jak toocreate asset a sestavení Smooth, MPEG DASH a adresy URL streamování HLS streamování toopublish Lokátor OnDemand. Také ukazuje aktivní toobuild adresa URL progresivního stahování. 

## <a name="create-an-ondemand-streaming-locator"></a>Vytvořit lokátor streamování OnDemand.
toocreate hello Lokátor streamování OnDemand a získání adres URL, musíte toodo hello následující věci:

1. Pokud obsah hello je zašifrován, definujte zásady přístupu.
2. Vytvořte Lokátor streamování OnDemand.
3. Pokud máte v plánu toostream, získáte hello vysílání datového proudu souboru manifestu (.ism) v hello asset. 
   
   Pokud máte v plánu tooprogressively stahování, získáte hello názvy souborů MP4 v hello asset.  
4. Vytvoření souboru manifestu toohello adresy URL nebo soubory MP4. 


>[!NOTE]
>Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Použití hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění. Například zásady pro lokátory, které jsou určené tooremain zavedené po dlouhou dobu (bez odeslání zásady). Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.

### <a name="use-media-services-net-sdk"></a>Používání Media Services .NET SDK
Vytvoření datových proudů adres URL 

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

výstupy Hello:

    URL toomanifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL toomanifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL toomanifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> Můžete také Streamovat obsah pomocí připojení SSL. toodo to přístupu, ujistěte se, spustíte adresy URL streamování s protokolem HTTPS. V současné době nepodporuje AMS SSL s vlastní domény.
> 
> 

Vytvoření adres URL progresivního stahování 

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

výstupy Hello:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a>Pomocí rozšíření Media Services .NET SDK
Hello následující kód volá .NET SDK rozšíření metody, které vytvořit Lokátor a generovat hello technologie Smooth Streaming, HLS a adresy URL pro MPEG-DASH pro adaptivní streamování.

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


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Další kroky
* [Stáhnout prostředky](media-services-deliver-asset-download.md)
* [Konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md)

