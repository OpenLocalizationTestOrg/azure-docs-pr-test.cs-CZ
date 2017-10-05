---
title: "Vytváření filtrů pomocí sady Azure Media Services .NET SDK"
description: "Toto téma popisuje, jak vytvářet filtry, takže vašeho klienta můžete použít datový proud určité části datového proudu. Služba Media Services vytvoří dynamické manifesty k dosažení této selektivní streamování."
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
ms.openlocfilehash: 6c43473b86c14679ace558de478bd95f41d476da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a><span data-ttu-id="993ab-104">Vytváření filtrů pomocí sady Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="993ab-104">Creating Filters with Azure Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="993ab-105">.NET</span><span class="sxs-lookup"><span data-stu-id="993ab-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="993ab-106">REST</span><span class="sxs-lookup"><span data-stu-id="993ab-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="993ab-107">Od verze 2.11, Media Services umožňuje definovat filtry pro vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="993ab-107">Starting with 2.11 release, Media Services enables you to define filters for your assets.</span></span> <span data-ttu-id="993ab-108">Tyto filtry jsou pravidla na straně serveru, které vám umožní vašim zákazníkům, kde můžete provádět například následující akce: přehrávání pouze část videa (namísto přehrávání celou video), nebo zadejte pouze podmnožinu interpretace audia a videa, které může zařízení vašich zákazníků (místo toho zpracovat všechny interpretací, jsou přidružený asset).</span><span class="sxs-lookup"><span data-stu-id="993ab-108">These filters are server side rules that will allow your customers to choose to do things like: playback only a section of a video (instead of playing the whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all the renditions that are associated with the asset).</span></span> <span data-ttu-id="993ab-109">Tento filtrování vaše prostředky je dosaženo pomocí **dynamické Manifest**ů, které jsou vytvořené na žádost zákazníka Streamovat videa podle zadané filtry.</span><span class="sxs-lookup"><span data-stu-id="993ab-109">This filtering of your assets is achieved through **Dynamic Manifest**s that are created upon your customer's request to stream a video based on specified filter(s).</span></span>

<span data-ttu-id="993ab-110">Podrobné informace týkající se filtrů a dynamické Manifest, najdete v části [dynamické manifesty přehled](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="993ab-110">For more detailed information related to filters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="993ab-111">Toto téma ukazuje, jak vytvářet, aktualizovat a odstraňovat filtry pomocí sady Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="993ab-111">This topic shows how to use Media Services .NET SDK to create, update, and delete filters.</span></span> 

<span data-ttu-id="993ab-112">Poznámka: Pokud aktualizujete filtr, může trvat až 2 minuty pro koncový bod k aktualizaci pravidla streamování.</span><span class="sxs-lookup"><span data-stu-id="993ab-112">Note if you update a filter, it can take up to 2 minutes for streaming endpoint to refresh the rules.</span></span> <span data-ttu-id="993ab-113">Pokud obsah zpracování pomocí tohoto filtru (a uložené v mezipaměti v proxy servery a CDN mezipaměti), aktualizace tento filtr může způsobit selhání přehrávač.</span><span class="sxs-lookup"><span data-stu-id="993ab-113">If the content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="993ab-114">Je doporučujeme vymazání mezipaměti po aktualizaci filtru.</span><span class="sxs-lookup"><span data-stu-id="993ab-114">It is recommend to clear the cache after updating the filter.</span></span> <span data-ttu-id="993ab-115">Pokud tato možnost není možné, zvažte použití jiný filtr.</span><span class="sxs-lookup"><span data-stu-id="993ab-115">If this option is not possible, consider using a different filter.</span></span> 

## <a name="types-used-to-create-filters"></a><span data-ttu-id="993ab-116">Typy používané pro vytvoření filtrů</span><span class="sxs-lookup"><span data-stu-id="993ab-116">Types used to create filters</span></span>
<span data-ttu-id="993ab-117">Následující typy se používají při vytváření filtrů:</span><span class="sxs-lookup"><span data-stu-id="993ab-117">The following types are used when creating filters:</span></span> 

* <span data-ttu-id="993ab-118">**IStreamingFilter**.</span><span class="sxs-lookup"><span data-stu-id="993ab-118">**IStreamingFilter**.</span></span>  <span data-ttu-id="993ab-119">Tento typ je založena na následující volání rozhraní REST API [filtru](https://docs.microsoft.com/rest/api/media/operations/filter)</span><span class="sxs-lookup"><span data-stu-id="993ab-119">This type is based on the following REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)</span></span>
* <span data-ttu-id="993ab-120">**IStreamingAssetFilter**.</span><span class="sxs-lookup"><span data-stu-id="993ab-120">**IStreamingAssetFilter**.</span></span> <span data-ttu-id="993ab-121">Tento typ je založena na následující volání rozhraní REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span><span class="sxs-lookup"><span data-stu-id="993ab-121">This type is based on the following REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span></span>
* <span data-ttu-id="993ab-122">**PresentationTimeRange**.</span><span class="sxs-lookup"><span data-stu-id="993ab-122">**PresentationTimeRange**.</span></span> <span data-ttu-id="993ab-123">Tento typ je založena na následující volání rozhraní REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span><span class="sxs-lookup"><span data-stu-id="993ab-123">This type is based on the following REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span></span>
* <span data-ttu-id="993ab-124">**FilterTrackSelectStatement** a **IFilterTrackPropertyCondition**.</span><span class="sxs-lookup"><span data-stu-id="993ab-124">**FilterTrackSelectStatement** and **IFilterTrackPropertyCondition**.</span></span> <span data-ttu-id="993ab-125">Tyto typy jsou založené na následující rozhraní API REST [FilterTrackSelect a FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span><span class="sxs-lookup"><span data-stu-id="993ab-125">These types are based on the following REST APIs [FilterTrackSelect and FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span></span>

## <a name="createupdatereaddelete-global-filters"></a><span data-ttu-id="993ab-126">Vytvoření, aktualizace nebo pro čtení nebo odstranění globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="993ab-126">Create/Update/Read/Delete global filters</span></span>
<span data-ttu-id="993ab-127">Následující kód ukazuje, jak pomocí rozhraní .NET k vytváření, aktualizaci, přečtěte si a odstraňte asset filtry.</span><span class="sxs-lookup"><span data-stu-id="993ab-127">The following code shows how to use .NET to create, update,read, and delete asset filters.</span></span>

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


## <a name="createupdatereaddelete-asset-filters"></a><span data-ttu-id="993ab-128">Vytvoření, aktualizace nebo pro čtení nebo odstranění asset filtry</span><span class="sxs-lookup"><span data-stu-id="993ab-128">Create/Update/Read/Delete asset filters</span></span>
<span data-ttu-id="993ab-129">Následující kód ukazuje, jak pomocí rozhraní .NET k vytváření, aktualizaci, přečtěte si a odstraňte asset filtry.</span><span class="sxs-lookup"><span data-stu-id="993ab-129">The following code shows how to use .NET to create, update,read, and delete asset filters.</span></span>

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




## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="993ab-130">Vytvoření adresy URL, které používají filtry pro streamování</span><span class="sxs-lookup"><span data-stu-id="993ab-130">Build streaming URLs that use filters</span></span>
<span data-ttu-id="993ab-131">Informace o tom, jak publikovat a poskytovat vaše prostředky najdete v tématu [doručování obsahu zákazníkům přehled](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="993ab-131">For information on how to publish and deliver your assets, see [Delivering Content to Customers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="993ab-132">Následující příklady ukazují, jak přidat filtry k adresám URL streamování.</span><span class="sxs-lookup"><span data-stu-id="993ab-132">The following examples show how to add filters to your streaming URLs.</span></span>

<span data-ttu-id="993ab-133">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="993ab-133">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="993ab-134">**Apple HTTP Live Streaming (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="993ab-134">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="993ab-135">**Apple HTTP Live Streaming (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="993ab-135">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="993ab-136">**Technologie Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="993ab-136">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a><span data-ttu-id="993ab-137">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="993ab-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="993ab-138">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="993ab-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="993ab-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="993ab-139">See Also</span></span>
[<span data-ttu-id="993ab-140">Přehled dynamické manifestů</span><span class="sxs-lookup"><span data-stu-id="993ab-140">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)

