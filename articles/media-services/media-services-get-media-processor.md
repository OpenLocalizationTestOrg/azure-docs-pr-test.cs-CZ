---
title: "aaaHow tooCreate pomocí procesoru media hello Azure Media Services SDK pro rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak toocreate tooencode komponenty procesoru média převést formát, šifrování nebo dešifrování obsahu médií pro službu Azure Media Services. Ukázky kódu jsou napsané v jazyce C# a použít hello sady Media Services SDK pro .NET."
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
ms.openlocfilehash: f133565cc1321d366013f17302adc8bc7585b251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-get-a-media-processor-instance"></a><span data-ttu-id="4e945-104">Postupy: získání Instance procesoru média</span><span class="sxs-lookup"><span data-stu-id="4e945-104">How to: Get a Media Processor Instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e945-105">.NET</span><span class="sxs-lookup"><span data-stu-id="4e945-105">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="4e945-106">REST</span><span class="sxs-lookup"><span data-stu-id="4e945-106">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="4e945-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="4e945-107">Overview</span></span>
<span data-ttu-id="4e945-108">Ve službě Media Services, kterou procesor médií je komponenta, která zpracovává zpracování specifické pro úlohy, jako je například kódování formátu převodu, šifrování nebo dešifrování mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="4e945-108">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="4e945-109">Obvykle vytváření procesor médií při vytváření úloh tooencode, šifrování nebo převést formát hello mediálního obsahu.</span><span class="sxs-lookup"><span data-stu-id="4e945-109">You typically create a media processor when you are creating a task tooencode, encrypt, or convert hello format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="4e945-110">Procesory médií Azure</span><span class="sxs-lookup"><span data-stu-id="4e945-110">Azure media processors</span></span> 

<span data-ttu-id="4e945-111">Hello následující téma obsahuje seznam procesory médií:</span><span class="sxs-lookup"><span data-stu-id="4e945-111">hello following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="4e945-112">Procesory médií z kódování</span><span class="sxs-lookup"><span data-stu-id="4e945-112">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="4e945-113">Procesory médií Analytics</span><span class="sxs-lookup"><span data-stu-id="4e945-113">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a><span data-ttu-id="4e945-114">Získat procesor médií</span><span class="sxs-lookup"><span data-stu-id="4e945-114">Get Media Processor</span></span>

<span data-ttu-id="4e945-115">Hello následující metoda ukazuje, jak tooget instance procesoru média.</span><span class="sxs-lookup"><span data-stu-id="4e945-115">hello following method shows how tooget a media processor instance.</span></span> <span data-ttu-id="4e945-116">Hello příklad kódu předpokládá použití hello úroveň modulu proměnné s názvem **_kontextu** tooreference hello kontextu serveru jak je popsáno v části hello [postupy: připojení služby prostřednictvím kódu programu tooMedia](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="4e945-116">hello code example assumes hello use of a module-level variable named **_context** tooreference hello server context as described in hello section [How to: Connect tooMedia Services Programmatically](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="4e945-117">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="4e945-117">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4e945-118">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="4e945-118">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="4e945-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e945-119">Next Steps</span></span>
<span data-ttu-id="4e945-120">Teď, když víte, jak tooget instance procesoru média, přejděte toohello [jak tooEncode prostředek](media-services-dotnet-encode-with-media-encoder-standard.md) téma, které vám ukáže, jak toouse hello Media Encoder Standard tooencode prostředek.</span><span class="sxs-lookup"><span data-stu-id="4e945-120">Now that you know how tooget a media processor instance, go toohello [How tooEncode an Asset](media-services-dotnet-encode-with-media-encoder-standard.md) topic which will show you how toouse hello Media Encoder Standard tooencode an asset.</span></span>

