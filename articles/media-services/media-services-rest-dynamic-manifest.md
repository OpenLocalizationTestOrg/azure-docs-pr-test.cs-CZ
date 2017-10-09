---
title: Filtry aaaCreating s Azure Media Services REST API | Microsoft Docs
description: "Toto téma popisuje, jak filtry toocreate abyste vašeho klienta mohli používat určité části toostream datového proudu. Služba Media Services vytvoří dynamické manifesty tooachieve selektivní streamování."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f7d23daf-7cd2-49c7-a195-ab902912ab3c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: d0b5af3b193b35f22ac70887963c2f0a06b60bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a><span data-ttu-id="e9564-104">Vytváření filtrů s Azure Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="e9564-104">Creating Filters with Azure Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e9564-105">.NET</span><span class="sxs-lookup"><span data-stu-id="e9564-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="e9564-106">REST</span><span class="sxs-lookup"><span data-stu-id="e9564-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="e9564-107">Od verze 2.11, Media Services umožňuje toodefine filtry pro vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="e9564-107">Starting with 2.11 release, Media Services enables you toodefine filters for your assets.</span></span> <span data-ttu-id="e9564-108">Tyto filtry jsou pravidla na straně serveru, které vám umožní vašim zákazníkům toochoose toodo věci jako: přehrávání pouze část videa (namísto přehrávání hello celý video), nebo zadejte pouze podmnožinu interpretace audia a videa, vašeho zákazníka zařízení dokáže zpracovat ( Ne všechny interpretace hello které jsou přidruženy hello asset).</span><span class="sxs-lookup"><span data-stu-id="e9564-108">These filters are server side rules that will allow your customers toochoose toodo things like: playback only a section of a video (instead of playing hello whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all hello renditions that are associated with hello asset).</span></span> <span data-ttu-id="e9564-109">Tento filtrování vaše prostředky bude archivován prostřednictvím **dynamické Manifest**ů, které jsou vytvořené při vašeho zákazníka žádost toostream video podle zadané filtry.</span><span class="sxs-lookup"><span data-stu-id="e9564-109">This filtering of your assets is archived through **Dynamic Manifest**s that are created upon your customer's request toostream a video based on specified filter(s).</span></span>

<span data-ttu-id="e9564-110">Podrobnější informace najdete v související toofilters a dynamické Manifest [dynamické manifesty přehled](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e9564-110">For more detailed information related toofilters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="e9564-111">Toto téma ukazuje, jak toouse toocreate rozhraní REST API, aktualizovat a odstraňovat filtry.</span><span class="sxs-lookup"><span data-stu-id="e9564-111">This topic shows how toouse REST APIs toocreate, update, and delete filters.</span></span> 

## <a name="types-used-toocreate-filters"></a><span data-ttu-id="e9564-112">Použít filtry toocreate typy</span><span class="sxs-lookup"><span data-stu-id="e9564-112">Types used toocreate filters</span></span>
<span data-ttu-id="e9564-113">Hello následující typy se používají při vytváření filtrů:</span><span class="sxs-lookup"><span data-stu-id="e9564-113">hello following types are used when creating filters:</span></span>  

* [<span data-ttu-id="e9564-114">Filtr</span><span class="sxs-lookup"><span data-stu-id="e9564-114">Filter</span></span>](https://docs.microsoft.com/rest/api/media/operations/filter)
* [<span data-ttu-id="e9564-115">AssetFilter</span><span class="sxs-lookup"><span data-stu-id="e9564-115">AssetFilter</span></span>](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [<span data-ttu-id="e9564-116">PresentationTimeRange</span><span class="sxs-lookup"><span data-stu-id="e9564-116">PresentationTimeRange</span></span>](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [<span data-ttu-id="e9564-117">FilterTrackSelect a FilterTrackPropertyCondition</span><span class="sxs-lookup"><span data-stu-id="e9564-117">FilterTrackSelect and FilterTrackPropertyCondition</span></span>](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

><span data-ttu-id="e9564-118">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="e9564-118">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="e9564-119">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="e9564-119">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="e9564-120">Připojení služby tooMedia</span><span class="sxs-lookup"><span data-stu-id="e9564-120">Connect tooMedia Services</span></span>

<span data-ttu-id="e9564-121">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="e9564-121">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="e9564-122">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="e9564-122">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="e9564-123">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="e9564-123">You must make subsequent calls toohello new URI.</span></span>

## <a name="create-filters"></a><span data-ttu-id="e9564-124">Vytváření filtrů</span><span class="sxs-lookup"><span data-stu-id="e9564-124">Create filters</span></span>
### <a name="create-global-filters"></a><span data-ttu-id="e9564-125">Vytvoření globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="e9564-125">Create global Filters</span></span>
<span data-ttu-id="e9564-126">toocreate globální filtr, použijte následující požadavky HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="e9564-126">toocreate a global Filter, use hello following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="e9564-127">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-127">HTTP Request</span></span>
<span data-ttu-id="e9564-128">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="e9564-128">Request Headers</span></span>

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host:media.windows.net 

<span data-ttu-id="e9564-129">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="e9564-129">Request body</span></span> 

    {  
       "Name":"GlobalFilter",
       "PresentationTimeRange":{  
          "StartTimestamp":"0",
          "EndTimestamp":"9223372036854775807",
          "PresentationWindowDuration":"12000000000",
          "LiveBackoffDuration":"0",
          "Timescale":"10000000"
       },
       "Tracks":[  
          {  
             "PropertyConditions":
                  [  
                {  
                   "Property":"Type",
                   "Value":"audio",
                   "Operator":"Equal"
                },
                {  
                   "Property":"Bitrate",
                   "Value":"0-2147483647",
                   "Operator":"Equal"
                }
             ]
          }
       ]
    }




#### <a name="http-response"></a><span data-ttu-id="e9564-130">Odpověď HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-130">HTTP Response</span></span>
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a><span data-ttu-id="e9564-131">Vytvořit místní AssetFilters</span><span class="sxs-lookup"><span data-stu-id="e9564-131">Create local AssetFilters</span></span>
<span data-ttu-id="e9564-132">toocreate místní AssetFilter, použijte následující požadavky HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="e9564-132">toocreate a local AssetFilter, use hello following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="e9564-133">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-133">HTTP Request</span></span>
<span data-ttu-id="e9564-134">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="e9564-134">Request Headers</span></span>

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net  

<span data-ttu-id="e9564-135">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="e9564-135">Request body</span></span> 

    {   
       "Name":"AssetFilter", 
       "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
       "PresentationTimeRange":{   
          "StartTimestamp":"0", 
          "EndTimestamp":"9223372036854775807", 
          "PresentationWindowDuration":"12000000000", 
          "LiveBackoffDuration":"0", 
          "Timescale":"10000000" 
       }, 
       "Tracks":[   
          {   
             "PropertyConditions": 
                  [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

#### <a name="http-response"></a><span data-ttu-id="e9564-136">Odpověď HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-136">HTTP Response</span></span>
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a><span data-ttu-id="e9564-137">Seznam filtrů</span><span class="sxs-lookup"><span data-stu-id="e9564-137">List filters</span></span>
### <a name="get-all-global-filters-in-hello-ams-account"></a><span data-ttu-id="e9564-138">Získání všech globální **filtru**s v účtu hello AMS</span><span class="sxs-lookup"><span data-stu-id="e9564-138">Get all global **Filter**s in hello AMS account</span></span>
<span data-ttu-id="e9564-139">toolist filtrů, použijte následující požadavky HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="e9564-139">toolist filters, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="e9564-140">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-140">HTTP Request</span></span>
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a><span data-ttu-id="e9564-141">Získat **AssetFilter**s přidružený prostředek</span><span class="sxs-lookup"><span data-stu-id="e9564-141">Get **AssetFilter**s associated with an asset</span></span>
#### <a name="http-request"></a><span data-ttu-id="e9564-142">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-142">HTTP Request</span></span>
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a><span data-ttu-id="e9564-143">Získat **AssetFilter** na základě jeho ID.</span><span class="sxs-lookup"><span data-stu-id="e9564-143">Get an **AssetFilter** based on its Id</span></span>
#### <a name="http-request"></a><span data-ttu-id="e9564-144">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-144">HTTP Request</span></span>
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a><span data-ttu-id="e9564-145">Aktualizace filtrů</span><span class="sxs-lookup"><span data-stu-id="e9564-145">Update filters</span></span>
<span data-ttu-id="e9564-146">Použití opravy, PUT nebo SLOUČENÍ tooupdate filtr s novými hodnotami vlastností.</span><span class="sxs-lookup"><span data-stu-id="e9564-146">Use PATCH, PUT or MERGE tooupdate a filter with new property values.</span></span>  <span data-ttu-id="e9564-147">Další informace týkající se těchto operací najdete v tématu [opravy, PUT, SLOUČENÍ](http://msdn.microsoft.com/library/dd541276.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9564-147">For more information about these operations, see [PATCH, PUT, MERGE](http://msdn.microsoft.com/library/dd541276.aspx).</span></span>

<span data-ttu-id="e9564-148">Pokud aktualizujete filtr, může trvat až minut too2 pravidla hello toorefresh koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="e9564-148">If you update a filter, it can take up too2 minutes for streaming endpoint toorefresh hello rules.</span></span> <span data-ttu-id="e9564-149">Pokud obsah hello zpracování pomocí tohoto filtru (a uložené v mezipaměti v proxy servery a CDN mezipaměti), aktualizace tento filtr může způsobit selhání přehrávač.</span><span class="sxs-lookup"><span data-stu-id="e9564-149">If hello content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="e9564-150">Je vhodné tooclear hello mezipaměti po aktualizaci hello filtru.</span><span class="sxs-lookup"><span data-stu-id="e9564-150">It is recommend tooclear hello cache after updating hello filter.</span></span> <span data-ttu-id="e9564-151">Pokud tato možnost není možné, zvažte použití jiný filtr.</span><span class="sxs-lookup"><span data-stu-id="e9564-151">If this option is not possible, consider using a different filter.</span></span>  

### <a name="update-global-filters"></a><span data-ttu-id="e9564-152">Aktualizace globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="e9564-152">Update global Filters</span></span>
<span data-ttu-id="e9564-153">tooupdate globální filtr, použijte následující požadavky HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="e9564-153">tooupdate a global filter, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="e9564-154">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-154">HTTP Request</span></span>
<span data-ttu-id="e9564-155">Hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="e9564-155">Request headers:</span></span> 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    Content-Length: 384

<span data-ttu-id="e9564-156">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="e9564-156">Request body:</span></span> 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

### <a name="update-local-assetfilters"></a><span data-ttu-id="e9564-157">Aktualizovat místní AssetFilters</span><span class="sxs-lookup"><span data-stu-id="e9564-157">Update local AssetFilters</span></span>
<span data-ttu-id="e9564-158">tooupdate místní filtr, použijte následující požadavky HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="e9564-158">tooupdate a local filter, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="e9564-159">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-159">HTTP Request</span></span>
<span data-ttu-id="e9564-160">Hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="e9564-160">Request headers:</span></span> 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

<span data-ttu-id="e9564-161">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="e9564-161">Request body:</span></span> 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 


## <a name="delete-filters"></a><span data-ttu-id="e9564-162">Odstranit filtry</span><span class="sxs-lookup"><span data-stu-id="e9564-162">Delete filters</span></span>
### <a name="delete-global-filters"></a><span data-ttu-id="e9564-163">Odstranit globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="e9564-163">Delete global Filters</span></span>
<span data-ttu-id="e9564-164">toodelete globální filtr, použijte následující požadavky HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="e9564-164">toodelete a global Filter, use hello following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="e9564-165">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-165">HTTP Request</span></span>
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a><span data-ttu-id="e9564-166">Odstranit místní AssetFilters</span><span class="sxs-lookup"><span data-stu-id="e9564-166">Delete local AssetFilters</span></span>
<span data-ttu-id="e9564-167">toodelete místní AssetFilter, použijte následující požadavky HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="e9564-167">toodelete a local AssetFilter, use hello following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="e9564-168">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e9564-168">HTTP Request</span></span>
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="e9564-169">Vytvoření adresy URL, které používají filtry pro streamování</span><span class="sxs-lookup"><span data-stu-id="e9564-169">Build streaming URLs that use filters</span></span>
<span data-ttu-id="e9564-170">Informace o tom, jak toopublish a poskytnout vaše prostředky, najdete v části [doručování obsahu tooCustomers přehled](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e9564-170">For information on how toopublish and deliver your assets, see [Delivering Content tooCustomers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="e9564-171">Hello následující příklady ukazují, jak tooadd filtry tooyour adresy URL pro streamování.</span><span class="sxs-lookup"><span data-stu-id="e9564-171">hello following examples show how tooadd filters tooyour streaming URLs.</span></span>

<span data-ttu-id="e9564-172">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="e9564-172">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="e9564-173">**Apple HTTP Live Streaming (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="e9564-173">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="e9564-174">**Apple HTTP Live Streaming (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="e9564-174">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="e9564-175">**Technologie Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="e9564-175">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a><span data-ttu-id="e9564-176">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="e9564-176">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e9564-177">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="e9564-177">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="e9564-178">Viz také</span><span class="sxs-lookup"><span data-stu-id="e9564-178">See Also</span></span>
[<span data-ttu-id="e9564-179">Přehled dynamické manifestů</span><span class="sxs-lookup"><span data-stu-id="e9564-179">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)

