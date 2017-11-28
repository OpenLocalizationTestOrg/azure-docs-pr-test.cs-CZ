---
title: "Jak tooget instance procesor médií pomocí REST aaa | Microsoft Docs"
description: "Zjistěte, jak toocreate tooencode komponenty procesoru média převést formát, šifrování nebo dešifrování obsahu médií pro službu Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 9f423648ab73c90405c64895ce0f5b6a457862e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-a-media-processor-instance"></a><span data-ttu-id="ccb54-103">Jak tooget instance procesoru média</span><span class="sxs-lookup"><span data-stu-id="ccb54-103">How tooget a Media Processor instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ccb54-104">.NET</span><span class="sxs-lookup"><span data-stu-id="ccb54-104">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="ccb54-105">REST</span><span class="sxs-lookup"><span data-stu-id="ccb54-105">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="ccb54-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="ccb54-106">Overview</span></span>
<span data-ttu-id="ccb54-107">Ve službě Media Services, kterou procesor médií je komponenta, která zpracovává zpracování specifické pro úlohy, jako je například kódování formátu převodu, šifrování nebo dešifrování mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="ccb54-107">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="ccb54-108">Obvykle vytváření procesor médií při vytváření úloh tooencode, šifrování nebo převést formát hello mediálního obsahu.</span><span class="sxs-lookup"><span data-stu-id="ccb54-108">You typically create a media processor when you are creating a task tooencode, encrypt, or convert hello format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="ccb54-109">Procesory médií Azure</span><span class="sxs-lookup"><span data-stu-id="ccb54-109">Azure media processors</span></span> 

<span data-ttu-id="ccb54-110">Hello následující téma obsahuje seznam procesory médií:</span><span class="sxs-lookup"><span data-stu-id="ccb54-110">hello following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="ccb54-111">Procesory médií z kódování</span><span class="sxs-lookup"><span data-stu-id="ccb54-111">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="ccb54-112">Procesory médií Analytics</span><span class="sxs-lookup"><span data-stu-id="ccb54-112">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
><span data-ttu-id="ccb54-113">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="ccb54-113">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="ccb54-114">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="ccb54-114">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="ccb54-115">Připojení služby tooMedia</span><span class="sxs-lookup"><span data-stu-id="ccb54-115">Connect tooMedia Services</span></span>

<span data-ttu-id="ccb54-116">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="ccb54-116">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="ccb54-117">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="ccb54-117">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="ccb54-118">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ccb54-118">You must make subsequent calls toohello new URI.</span></span>

## <a name="get-a-media-processor"></a><span data-ttu-id="ccb54-119">Získat procesor médií</span><span class="sxs-lookup"><span data-stu-id="ccb54-119">Get a media processor</span></span>

<span data-ttu-id="ccb54-120">Následující volání REST Hello ukazuje, jak tooget procesor médií instanci podle názvu (v tomto případě **Media Encoder Standard**).</span><span class="sxs-lookup"><span data-stu-id="ccb54-120">hello following REST call shows how tooget a media processor instance by name (in this case, **Media Encoder Standard**).</span></span> 

<span data-ttu-id="ccb54-121">Žádost:</span><span class="sxs-lookup"><span data-stu-id="ccb54-121">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="ccb54-122">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="ccb54-122">Response:</span></span>

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="ccb54-123">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="ccb54-123">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ccb54-124">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ccb54-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="ccb54-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccb54-125">Next Steps</span></span>
<span data-ttu-id="ccb54-126">Teď, když víte, jak tooget instance procesoru média, přejděte toohello [jak tooEncode prostředek](media-services-rest-get-started.md) téma, které vám ukáže, jak toouse hello Media Encoder Standard tooencode prostředek.</span><span class="sxs-lookup"><span data-stu-id="ccb54-126">Now that you know how tooget a media processor instance, go toohello [How tooEncode an Asset](media-services-rest-get-started.md) topic which will show you how toouse hello Media Encoder Standard tooencode an asset.</span></span>

