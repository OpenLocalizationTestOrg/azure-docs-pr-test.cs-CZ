---
title: "Vytváření filtrů pomocí Azure Media Services REST API | Microsoft Docs"
description: "Toto téma popisuje, jak vytvářet filtry, takže vašeho klienta můžete použít datový proud určité části datového proudu. Služba Media Services vytvoří dynamické manifesty k dosažení této selektivní streamování."
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
ms.openlocfilehash: 76d2721138668d9f0a908af3fa42840309b068ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a><span data-ttu-id="4ce92-104">Vytváření filtrů s Azure Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="4ce92-104">Creating Filters with Azure Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4ce92-105">.NET</span><span class="sxs-lookup"><span data-stu-id="4ce92-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="4ce92-106">REST</span><span class="sxs-lookup"><span data-stu-id="4ce92-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="4ce92-107">Od verze 2.11, Media Services umožňuje definovat filtry pro vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="4ce92-107">Starting with 2.11 release, Media Services enables you to define filters for your assets.</span></span> <span data-ttu-id="4ce92-108">Tyto filtry jsou pravidla na straně serveru, které vám umožní vašim zákazníkům, kde můžete provádět například následující akce: přehrávání pouze část videa (namísto přehrávání celou video), nebo zadejte pouze podmnožinu interpretace audia a videa, které může zařízení vašich zákazníků (místo toho zpracovat všechny interpretací, jsou přidružený asset).</span><span class="sxs-lookup"><span data-stu-id="4ce92-108">These filters are server side rules that will allow your customers to choose to do things like: playback only a section of a video (instead of playing the whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all the renditions that are associated with the asset).</span></span> <span data-ttu-id="4ce92-109">Tento filtrování vaše prostředky bude archivován prostřednictvím **dynamické Manifest**ů, které jsou vytvořené na žádost zákazníka Streamovat videa podle zadané filtry.</span><span class="sxs-lookup"><span data-stu-id="4ce92-109">This filtering of your assets is archived through **Dynamic Manifest**s that are created upon your customer's request to stream a video based on specified filter(s).</span></span>

<span data-ttu-id="4ce92-110">Podrobné informace týkající se filtrů a dynamické Manifest, najdete v části [dynamické manifesty přehled](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4ce92-110">For more detailed information related to filters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="4ce92-111">Toto téma ukazuje, jak používat rozhraní REST API vytvářet, aktualizovat a odstraňovat filtry.</span><span class="sxs-lookup"><span data-stu-id="4ce92-111">This topic shows how to use REST APIs to create, update, and delete filters.</span></span> 

## <a name="types-used-to-create-filters"></a><span data-ttu-id="4ce92-112">Typy používané pro vytvoření filtrů</span><span class="sxs-lookup"><span data-stu-id="4ce92-112">Types used to create filters</span></span>
<span data-ttu-id="4ce92-113">Následující typy se používají při vytváření filtrů:</span><span class="sxs-lookup"><span data-stu-id="4ce92-113">The following types are used when creating filters:</span></span>  

* [<span data-ttu-id="4ce92-114">Filtr</span><span class="sxs-lookup"><span data-stu-id="4ce92-114">Filter</span></span>](https://docs.microsoft.com/rest/api/media/operations/filter)
* [<span data-ttu-id="4ce92-115">AssetFilter</span><span class="sxs-lookup"><span data-stu-id="4ce92-115">AssetFilter</span></span>](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [<span data-ttu-id="4ce92-116">PresentationTimeRange</span><span class="sxs-lookup"><span data-stu-id="4ce92-116">PresentationTimeRange</span></span>](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [<span data-ttu-id="4ce92-117">FilterTrackSelect a FilterTrackPropertyCondition</span><span class="sxs-lookup"><span data-stu-id="4ce92-117">FilterTrackSelect and FilterTrackPropertyCondition</span></span>](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

><span data-ttu-id="4ce92-118">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="4ce92-118">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="4ce92-119">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="4ce92-119">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="4ce92-120">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="4ce92-120">Connect to Media Services</span></span>

<span data-ttu-id="4ce92-121">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="4ce92-121">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="4ce92-122">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="4ce92-122">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="4ce92-123">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="4ce92-123">You must make subsequent calls to the new URI.</span></span>

## <a name="create-filters"></a><span data-ttu-id="4ce92-124">Vytváření filtrů</span><span class="sxs-lookup"><span data-stu-id="4ce92-124">Create filters</span></span>
### <a name="create-global-filters"></a><span data-ttu-id="4ce92-125">Vytvoření globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="4ce92-125">Create global Filters</span></span>
<span data-ttu-id="4ce92-126">K vytvoření globálních filtrů, použijte následující požadavky HTTP:</span><span class="sxs-lookup"><span data-stu-id="4ce92-126">To create a global Filter, use the following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="4ce92-127">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-127">HTTP Request</span></span>
<span data-ttu-id="4ce92-128">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="4ce92-128">Request Headers</span></span>

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

<span data-ttu-id="4ce92-129">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="4ce92-129">Request body</span></span> 

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




#### <a name="http-response"></a><span data-ttu-id="4ce92-130">Odpověď HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-130">HTTP Response</span></span>
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a><span data-ttu-id="4ce92-131">Vytvořit místní AssetFilters</span><span class="sxs-lookup"><span data-stu-id="4ce92-131">Create local AssetFilters</span></span>
<span data-ttu-id="4ce92-132">Pokud chcete vytvořit místní AssetFilter, použijte následující požadavky HTTP:</span><span class="sxs-lookup"><span data-stu-id="4ce92-132">To create a local AssetFilter, use the following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="4ce92-133">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-133">HTTP Request</span></span>
<span data-ttu-id="4ce92-134">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="4ce92-134">Request Headers</span></span>

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

<span data-ttu-id="4ce92-135">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="4ce92-135">Request body</span></span> 

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

#### <a name="http-response"></a><span data-ttu-id="4ce92-136">Odpověď HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-136">HTTP Response</span></span>
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a><span data-ttu-id="4ce92-137">Seznam filtrů</span><span class="sxs-lookup"><span data-stu-id="4ce92-137">List filters</span></span>
### <a name="get-all-global-filters-in-the-ams-account"></a><span data-ttu-id="4ce92-138">Získání všech globální **filtru**s v účtu AMS</span><span class="sxs-lookup"><span data-stu-id="4ce92-138">Get all global **Filter**s in the AMS account</span></span>
<span data-ttu-id="4ce92-139">Chcete-li seznam filtrů, použijte následující HTTP požadavků:</span><span class="sxs-lookup"><span data-stu-id="4ce92-139">To list filters, use the following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="4ce92-140">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-140">HTTP Request</span></span>
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a><span data-ttu-id="4ce92-141">Získat **AssetFilter**s přidružený prostředek</span><span class="sxs-lookup"><span data-stu-id="4ce92-141">Get **AssetFilter**s associated with an asset</span></span>
#### <a name="http-request"></a><span data-ttu-id="4ce92-142">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-142">HTTP Request</span></span>
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a><span data-ttu-id="4ce92-143">Získat **AssetFilter** na základě jeho ID.</span><span class="sxs-lookup"><span data-stu-id="4ce92-143">Get an **AssetFilter** based on its Id</span></span>
#### <a name="http-request"></a><span data-ttu-id="4ce92-144">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-144">HTTP Request</span></span>
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a><span data-ttu-id="4ce92-145">Aktualizace filtrů</span><span class="sxs-lookup"><span data-stu-id="4ce92-145">Update filters</span></span>
<span data-ttu-id="4ce92-146">Použití opravy, PUT nebo SLOUČENÍ aktualizovat filtr s hodnotami novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4ce92-146">Use PATCH, PUT or MERGE to update a filter with new property values.</span></span>  <span data-ttu-id="4ce92-147">Další informace týkající se těchto operací najdete v tématu [opravy, PUT, SLOUČENÍ](http://msdn.microsoft.com/library/dd541276.aspx).</span><span class="sxs-lookup"><span data-stu-id="4ce92-147">For more information about these operations, see [PATCH, PUT, MERGE](http://msdn.microsoft.com/library/dd541276.aspx).</span></span>

<span data-ttu-id="4ce92-148">Pokud aktualizujete filtr, může trvat až 2 minuty koncový bod streamování aktualizovat pravidla.</span><span class="sxs-lookup"><span data-stu-id="4ce92-148">If you update a filter, it can take up to 2 minutes for streaming endpoint to refresh the rules.</span></span> <span data-ttu-id="4ce92-149">Pokud obsah zpracování pomocí tohoto filtru (a uložené v mezipaměti v proxy servery a CDN mezipaměti), aktualizace tento filtr může způsobit selhání přehrávač.</span><span class="sxs-lookup"><span data-stu-id="4ce92-149">If the content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="4ce92-150">Je doporučujeme vymazání mezipaměti po aktualizaci filtru.</span><span class="sxs-lookup"><span data-stu-id="4ce92-150">It is recommend to clear the cache after updating the filter.</span></span> <span data-ttu-id="4ce92-151">Pokud tato možnost není možné, zvažte použití jiný filtr.</span><span class="sxs-lookup"><span data-stu-id="4ce92-151">If this option is not possible, consider using a different filter.</span></span>  

### <a name="update-global-filters"></a><span data-ttu-id="4ce92-152">Aktualizace globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="4ce92-152">Update global Filters</span></span>
<span data-ttu-id="4ce92-153">Chcete-li aktualizovat globální filtr, použijte následující požadavky HTTP:</span><span class="sxs-lookup"><span data-stu-id="4ce92-153">To update a global filter, use the following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="4ce92-154">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-154">HTTP Request</span></span>
<span data-ttu-id="4ce92-155">Hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="4ce92-155">Request headers:</span></span> 

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

<span data-ttu-id="4ce92-156">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="4ce92-156">Request body:</span></span> 

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

### <a name="update-local-assetfilters"></a><span data-ttu-id="4ce92-157">Aktualizovat místní AssetFilters</span><span class="sxs-lookup"><span data-stu-id="4ce92-157">Update local AssetFilters</span></span>
<span data-ttu-id="4ce92-158">Pokud chcete aktualizovat místní filtr, použijte následující požadavky HTTP:</span><span class="sxs-lookup"><span data-stu-id="4ce92-158">To update a local filter, use the following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="4ce92-159">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-159">HTTP Request</span></span>
<span data-ttu-id="4ce92-160">Hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="4ce92-160">Request headers:</span></span> 

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

<span data-ttu-id="4ce92-161">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="4ce92-161">Request body:</span></span> 

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


## <a name="delete-filters"></a><span data-ttu-id="4ce92-162">Odstranit filtry</span><span class="sxs-lookup"><span data-stu-id="4ce92-162">Delete filters</span></span>
### <a name="delete-global-filters"></a><span data-ttu-id="4ce92-163">Odstranit globálních filtrů</span><span class="sxs-lookup"><span data-stu-id="4ce92-163">Delete global Filters</span></span>
<span data-ttu-id="4ce92-164">Chcete-li odstranit globální filtr, použijte následující požadavky HTTP:</span><span class="sxs-lookup"><span data-stu-id="4ce92-164">To delete a global Filter, use the following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="4ce92-165">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-165">HTTP Request</span></span>
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a><span data-ttu-id="4ce92-166">Odstranit místní AssetFilters</span><span class="sxs-lookup"><span data-stu-id="4ce92-166">Delete local AssetFilters</span></span>
<span data-ttu-id="4ce92-167">Chcete-li odstranit místní AssetFilter, použijte následující požadavky HTTP:</span><span class="sxs-lookup"><span data-stu-id="4ce92-167">To delete a local AssetFilter, use the following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="4ce92-168">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="4ce92-168">HTTP Request</span></span>
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="4ce92-169">Vytvoření adresy URL, které používají filtry pro streamování</span><span class="sxs-lookup"><span data-stu-id="4ce92-169">Build streaming URLs that use filters</span></span>
<span data-ttu-id="4ce92-170">Informace o tom, jak publikovat a poskytovat vaše prostředky najdete v tématu [doručování obsahu zákazníkům přehled](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4ce92-170">For information on how to publish and deliver your assets, see [Delivering Content to Customers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="4ce92-171">Následující příklady ukazují, jak přidat filtry k adresám URL streamování.</span><span class="sxs-lookup"><span data-stu-id="4ce92-171">The following examples show how to add filters to your streaming URLs.</span></span>

<span data-ttu-id="4ce92-172">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="4ce92-172">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="4ce92-173">**Apple HTTP Live Streaming (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="4ce92-173">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="4ce92-174">**Apple HTTP Live Streaming (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="4ce92-174">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="4ce92-175">**Technologie Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="4ce92-175">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a><span data-ttu-id="4ce92-176">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="4ce92-176">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4ce92-177">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="4ce92-177">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="4ce92-178">Viz také</span><span class="sxs-lookup"><span data-stu-id="4ce92-178">See Also</span></span>
[<span data-ttu-id="4ce92-179">Přehled dynamické manifestů</span><span class="sxs-lookup"><span data-stu-id="4ce92-179">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)

