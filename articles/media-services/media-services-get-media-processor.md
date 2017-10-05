---
title: "Postup vytvoření procesor médií pomocí Azure Media Services SDK pro .NET | Microsoft Docs"
description: "Naučte se vytvořit komponentu procesoru média zakódovat, převést formát, šifrování nebo dešifrování obsahu médií pro službu Azure Media Services. Ukázky kódu jsou napsané v jazyce C# a pomocí sady Media Services SDK pro .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: c2cbe41b71afa8acc184f9d7f4cfe94686de783e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-get-a-media-processor-instance"></a><span data-ttu-id="70b8d-104">Postupy: získání Instance procesoru média</span><span class="sxs-lookup"><span data-stu-id="70b8d-104">How to: Get a Media Processor Instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="70b8d-105">.NET</span><span class="sxs-lookup"><span data-stu-id="70b8d-105">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="70b8d-106">REST</span><span class="sxs-lookup"><span data-stu-id="70b8d-106">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="70b8d-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="70b8d-107">Overview</span></span>
<span data-ttu-id="70b8d-108">Ve službě Media Services, kterou procesor médií je komponenta, která zpracovává zpracování specifické pro úlohy, jako je například kódování formátu převodu, šifrování nebo dešifrování mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="70b8d-108">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="70b8d-109">Obvykle vytvoříte procesor médií při vytváření úlohy kódování, šifrování nebo převést formát mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="70b8d-109">You typically create a media processor when you are creating a task to encode, encrypt, or convert the format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="70b8d-110">Procesory médií Azure</span><span class="sxs-lookup"><span data-stu-id="70b8d-110">Azure media processors</span></span> 

<span data-ttu-id="70b8d-111">Následující téma obsahuje seznam procesory médií:</span><span class="sxs-lookup"><span data-stu-id="70b8d-111">The following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="70b8d-112">Procesory médií z kódování</span><span class="sxs-lookup"><span data-stu-id="70b8d-112">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="70b8d-113">Procesory médií Analytics</span><span class="sxs-lookup"><span data-stu-id="70b8d-113">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a><span data-ttu-id="70b8d-114">Získat procesor médií</span><span class="sxs-lookup"><span data-stu-id="70b8d-114">Get Media Processor</span></span>

<span data-ttu-id="70b8d-115">Následující metodu ukazuje, jak získat instance procesoru média.</span><span class="sxs-lookup"><span data-stu-id="70b8d-115">The following method shows how to get a media processor instance.</span></span> <span data-ttu-id="70b8d-116">Příklad kódu předpokládá použití proměnné úroveň modulu s názvem **_kontextu** tak, aby odkazovaly kontextu serveru, jak je popsáno v části [postupy: připojení k Media Services prostřednictvím kódu programu](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="70b8d-116">The code example assumes the use of a module-level variable named **_context** to reference the server context as described in the section [How to: Connect to Media Services Programmatically](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="70b8d-117">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="70b8d-117">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="70b8d-118">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="70b8d-118">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="70b8d-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70b8d-119">Next Steps</span></span>
<span data-ttu-id="70b8d-120">Teď, když víte, jak získat instance procesoru média, přejděte na [postup kódovat Asset](media-services-dotnet-encode-with-media-encoder-standard.md) téma, které vám ukáže, jak používat Media Encoder Standard kódování assetu.</span><span class="sxs-lookup"><span data-stu-id="70b8d-120">Now that you know how to get a media processor instance, go to the [How to Encode an Asset](media-services-dotnet-encode-with-media-encoder-standard.md) topic which will show you how to use the Media Encoder Standard to encode an asset.</span></span>

