---
title: Filtry aaaCreating s Azure Media Services .NET SDK
description: "Toto téma popisuje, jak filtry toocreate abyste vašeho klienta mohli používat určité části toostream datového proudu. Služba Media Services vytvoří dynamické manifesty tooachieve selektivní streamování."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 16d9497d48ab1d3f841dd97efb0f66016a2435c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a>Vytváření filtrů pomocí sady Azure Media Services .NET SDK
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Od verze 2.11, Media Services umožňuje toodefine filtry pro vaše prostředky. Tyto filtry jsou pravidla na straně serveru, které vám umožní vašim zákazníkům toochoose toodo věci jako: přehrávání pouze část videa (namísto přehrávání hello celý video), nebo zadejte pouze podmnožinu interpretace audia a videa, vašeho zákazníka zařízení dokáže zpracovat ( Ne všechny interpretace hello které jsou přidruženy hello asset). Tento filtrování vaše prostředky je dosaženo pomocí **dynamické Manifest**ů, které jsou vytvořené při vašeho zákazníka žádost toostream video podle zadané filtry.

Podrobnější informace najdete v související toofilters a dynamické Manifest [dynamické manifesty přehled](media-services-dynamic-manifest-overview.md).

Toto téma ukazuje, jak toouse toocreate sady Media Services .NET SDK, aktualizovat a odstraňovat filtry. 

Poznámka: Pokud aktualizujete filtr, může to trvat až minut too2 pravidla hello toorefresh koncový bod streamování. Pokud obsah hello zpracování pomocí tohoto filtru (a uložené v mezipaměti v proxy servery a CDN mezipaměti), aktualizace tento filtr může způsobit selhání přehrávač. Je vhodné tooclear hello mezipaměti po aktualizaci hello filtru. Pokud tato možnost není možné, zvažte použití jiný filtr. 

## <a name="types-used-toocreate-filters"></a>Použít filtry toocreate typy
Hello následující typy se používají při vytváření filtrů: 

* **IStreamingFilter**.  Tento typ je založen na hello následující rozhraní REST API [filtru](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Tento typ je založen na hello následující rozhraní REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Tento typ je založen na hello následující rozhraní REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** a **IFilterTrackPropertyCondition**. Tyto typy jsou založené na hello následující rozhraní REST API [FilterTrackSelect a FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Vytvoření, aktualizace nebo pro čtení nebo odstranění globálních filtrů
Hello následující kód ukazuje, jak toouse .NET toocreate, aktualizovat, přečtěte si a odstranit asset filtry.

    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


## <a name="createupdatereaddelete-asset-filters"></a>Vytvoření, aktualizace nebo pro čtení nebo odstranění asset filtry
Hello následující kód ukazuje, jak toouse .NET toocreate, aktualizovat, přečtěte si a odstranit asset filtry.

    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();




## <a name="build-streaming-urls-that-use-filters"></a>Vytvoření adresy URL, které používají filtry pro streamování
Informace o tom, jak toopublish a poskytnout vaše prostředky, najdete v části [doručování obsahu tooCustomers přehled](media-services-deliver-content-overview.md).

Hello následující příklady ukazují, jak tooadd filtry tooyour adresy URL pro streamování.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Technologie Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Přehled dynamické manifestů](media-services-dynamic-manifest-overview.md)

