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
# <a name="how-to-get-a-media-processor-instance"></a>Postupy: získání Instance procesoru média
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Přehled
Ve službě Media Services, kterou procesor médií je komponenta, která zpracovává zpracování specifické pro úlohy, jako je například kódování formátu převodu, šifrování nebo dešifrování mediální obsah. Obvykle vytváření procesor médií při vytváření úloh tooencode, šifrování nebo převést formát hello mediálního obsahu.

## <a name="azure-media-processors"></a>Procesory médií Azure 

Hello následující téma obsahuje seznam procesory médií:

* [Procesory médií z kódování](scenarios-and-availability.md#encoding-media-processors)
* [Procesory médií Analytics](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Získat procesor médií

Hello následující metoda ukazuje, jak tooget instance procesoru média. Hello příklad kódu předpokládá použití hello úroveň modulu proměnné s názvem **_kontextu** tooreference hello kontextu serveru jak je popsáno v části hello [postupy: připojení služby prostřednictvím kódu programu tooMedia](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Další kroky
Teď, když víte, jak tooget instance procesoru média, přejděte toohello [jak tooEncode prostředek](media-services-dotnet-encode-with-media-encoder-standard.md) téma, které vám ukáže, jak toouse hello Media Encoder Standard tooencode prostředek.

