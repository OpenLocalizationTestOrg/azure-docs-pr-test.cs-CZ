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
# <a name="creating-filters-with-azure-media-services-net-sdk"></a><span data-ttu-id="c5a6f-104">Vytváření filtrů pomocí sady Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c5a6f-104">Creating Filters with Azure Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c5a6f-105">.NET</span><span class="sxs-lookup"><span data-stu-id="c5a6f-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="c5a6f-106">REST</span><span class="sxs-lookup"><span data-stu-id="c5a6f-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="c5a6f-107">Od verze 2.11, Media Services umožňuje toodefine filtry pro vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-107">Starting with 2.11 release, Media Services enables you toodefine filters for your assets.</span></span> <span data-ttu-id="c5a6f-108">Tyto filtry jsou pravidla na straně serveru, které vám umožní vašim zákazníkům toochoose toodo věci jako: přehrávání pouze část videa (namísto přehrávání hello celý video), nebo zadejte pouze podmnožinu interpretace audia a videa, vašeho zákazníka zařízení dokáže zpracovat ( Ne všechny interpretace hello které jsou přidruženy hello asset).</span><span class="sxs-lookup"><span data-stu-id="c5a6f-108">These filters are server side rules that will allow your customers toochoose toodo things like: playback only a section of a video (instead of playing hello whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all hello renditions that are associated with hello asset).</span></span> <span data-ttu-id="c5a6f-109">Tento filtrování vaše prostředky je dosaženo pomocí **dynamické Manifest**ů, které jsou vytvořené při vašeho zákazníka žádost toostream video podle zadané filtry.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-109">This filtering of your assets is achieved through **Dynamic Manifest**s that are created upon your customer's request toostream a video based on specified filter(s).</span></span>

<span data-ttu-id="c5a6f-110">Podrobnější informace najdete v související toofilters a dynamické Manifest [dynamické manifesty přehled](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c5a6f-110">For more detailed information related toofilters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="c5a6f-111">Toto téma ukazuje, jak toouse toocreate sady Media Services .NET SDK, aktualizovat a odstraňovat filtry.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-111">This topic shows how toouse Media Services .NET SDK toocreate, update, and delete filters.</span></span> 

<span data-ttu-id="c5a6f-112">Poznámka: Pokud aktualizujete filtr, může to trvat až minut too2 pravidla hello toorefresh koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-112">Note if you update a filter, it can take up too2 minutes for streaming endpoint toorefresh hello rules.</span></span> <span data-ttu-id="c5a6f-113">Pokud obsah hello zpracování pomocí tohoto filtru (a uložené v mezipaměti v proxy servery a CDN mezipaměti), aktualizace tento filtr může způsobit selhání přehrávač.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-113">If hello content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="c5a6f-114">Je vhodné tooclear hello mezipaměti po aktualizaci hello filtru.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-114">It is recommend tooclear hello cache after updating hello filter.</span></span> <span data-ttu-id="c5a6f-115">Pokud tato možnost není možné, zvažte použití jiný filtr.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-115">If this option is not possible, consider using a different filter.</span></span> 

## <a name="types-used-toocreate-filters"></a><span data-ttu-id="c5a6f-116">Použít filtry toocreate typy</span><span class="sxs-lookup"><span data-stu-id="c5a6f-116">Types used toocreate filters</span></span>
<span data-ttu-id="c5a6f-117">Hello následující typy se používají při vytváření filtrů:</span><span class="sxs-lookup"><span data-stu-id="c5a6f-117">hello following types are used when creating filters:</span></span> 

* <span data-ttu-id="c5a6f-118">**IStreamingFilter**.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-118">**IStreamingFilter**.</span></span>  <span data-ttu-id="c5a6f-119">Tento typ je založen na hello následující rozhraní REST API [filtru](https://docs.microsoft.com/rest/api/media/operations/filter)</span><span class="sxs-lookup"><span data-stu-id="c5a6f-119">This type is based on hello following REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)</span></span>
* <span data-ttu-id="c5a6f-120">**IStreamingAssetFilter**.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-120">**IStreamingAssetFilter**.</span></span> <span data-ttu-id="c5a6f-121">Tento typ je založen na hello následující rozhraní REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span><span class="sxs-lookup"><span data-stu-id="c5a6f-121">This type is based on hello following REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span></span>
* <span data-ttu-id="c5a6f-122">**PresentationTimeRange**.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-122">**PresentationTimeRange**.</span></span> <span data-ttu-id="c5a6f-123">Tento typ je založen na hello následující rozhraní REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span><span class="sxs-lookup"><span data-stu-id="c5a6f-123">This type is based on hello following REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span></span>
* <span data-ttu-id="c5a6f-124">**FilterTrackSelectStatement** a **IFilterTrackPropertyCondition**.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-124">**FilterTrackSelectStatement** and **IFilterTrackPropertyCondition**.</span></span> <span data-ttu-id="c5a6f-125">Tyto typy jsou založené na hello následující rozhraní REST API [FilterTrackSelect a FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span><span class="sxs-lookup"><span data-stu-id="c5a6f-125">These types are based on hello following REST APIs [FilterTrackSelect and FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span></span>

## <a name="createupdatereaddelete-global-filters"></a><span data-ttu-id="c5a6f-126">Vytvoření, aktualizace nebo pro čtení nebo odstranění globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="c5a6f-126">Create/Update/Read/Delete global filters</span></span>
<span data-ttu-id="c5a6f-127">Hello následující kód ukazuje, jak toouse .NET toocreate, aktualizovat, přečtěte si a odstranit asset filtry.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-127">hello following code shows how toouse .NET toocreate, update,read, and delete asset filters.</span></span>

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


## <a name="createupdatereaddelete-asset-filters"></a><span data-ttu-id="c5a6f-128">Vytvoření, aktualizace nebo pro čtení nebo odstranění asset filtry</span><span class="sxs-lookup"><span data-stu-id="c5a6f-128">Create/Update/Read/Delete asset filters</span></span>
<span data-ttu-id="c5a6f-129">Hello následující kód ukazuje, jak toouse .NET toocreate, aktualizovat, přečtěte si a odstranit asset filtry.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-129">hello following code shows how toouse .NET toocreate, update,read, and delete asset filters.</span></span>

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




## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="c5a6f-130">Vytvoření adresy URL, které používají filtry pro streamování</span><span class="sxs-lookup"><span data-stu-id="c5a6f-130">Build streaming URLs that use filters</span></span>
<span data-ttu-id="c5a6f-131">Informace o tom, jak toopublish a poskytnout vaše prostředky, najdete v části [doručování obsahu tooCustomers přehled](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c5a6f-131">For information on how toopublish and deliver your assets, see [Delivering Content tooCustomers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="c5a6f-132">Hello následující příklady ukazují, jak tooadd filtry tooyour adresy URL pro streamování.</span><span class="sxs-lookup"><span data-stu-id="c5a6f-132">hello following examples show how tooadd filters tooyour streaming URLs.</span></span>

<span data-ttu-id="c5a6f-133">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="c5a6f-133">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="c5a6f-134">**Apple HTTP Live Streaming (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="c5a6f-134">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="c5a6f-135">**Apple HTTP Live Streaming (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="c5a6f-135">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="c5a6f-136">**Technologie Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="c5a6f-136">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a><span data-ttu-id="c5a6f-137">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="c5a6f-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c5a6f-138">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="c5a6f-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="c5a6f-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="c5a6f-139">See Also</span></span>
[<span data-ttu-id="c5a6f-140">Přehled dynamické manifestů</span><span class="sxs-lookup"><span data-stu-id="c5a6f-140">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)

