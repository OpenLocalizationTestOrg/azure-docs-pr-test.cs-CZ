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
# <a name="creating-filters-with-azure-media-services-rest-api"></a>Vytváření filtrů s Azure Media Services REST API
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Od verze 2.11, Media Services umožňuje toodefine filtry pro vaše prostředky. Tyto filtry jsou pravidla na straně serveru, které vám umožní vašim zákazníkům toochoose toodo věci jako: přehrávání pouze část videa (namísto přehrávání hello celý video), nebo zadejte pouze podmnožinu interpretace audia a videa, vašeho zákazníka zařízení dokáže zpracovat ( Ne všechny interpretace hello které jsou přidruženy hello asset). Tento filtrování vaše prostředky bude archivován prostřednictvím **dynamické Manifest**ů, které jsou vytvořené při vašeho zákazníka žádost toostream video podle zadané filtry.

Podrobnější informace najdete v související toofilters a dynamické Manifest [dynamické manifesty přehled](media-services-dynamic-manifest-overview.md).

Toto téma ukazuje, jak toouse toocreate rozhraní REST API, aktualizovat a odstraňovat filtry. 

## <a name="types-used-toocreate-filters"></a>Použít filtry toocreate typy
Hello následující typy se používají při vytváření filtrů:  

* [Filtr](https://docs.microsoft.com/rest/api/media/operations/filter)
* [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [FilterTrackSelect a FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

>Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP. Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).

## <a name="connect-toomedia-services"></a>Připojení služby tooMedia

Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání toohello nový identifikátor URI.

## <a name="create-filters"></a>Vytváření filtrů
### <a name="create-global-filters"></a>Vytvoření globálních filtrů
toocreate globální filtr, použijte následující požadavky HTTP hello:  

#### <a name="http-request"></a>Požadavek HTTP
Hlavičky požadavku

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

Text žádosti 

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




#### <a name="http-response"></a>Odpověď HTTP
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a>Vytvořit místní AssetFilters
toocreate místní AssetFilter, použijte následující požadavky HTTP hello:  

#### <a name="http-request"></a>Požadavek HTTP
Hlavičky požadavku

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

Text žádosti 

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

#### <a name="http-response"></a>Odpověď HTTP
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a>Seznam filtrů
### <a name="get-all-global-filters-in-hello-ams-account"></a>Získání všech globální **filtru**s v účtu hello AMS
toolist filtrů, použijte následující požadavky HTTP hello: 

#### <a name="http-request"></a>Požadavek HTTP
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a>Získat **AssetFilter**s přidružený prostředek
#### <a name="http-request"></a>Požadavek HTTP
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a>Získat **AssetFilter** na základě jeho ID.
#### <a name="http-request"></a>Požadavek HTTP
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a>Aktualizace filtrů
Použití opravy, PUT nebo SLOUČENÍ tooupdate filtr s novými hodnotami vlastností.  Další informace týkající se těchto operací najdete v tématu [opravy, PUT, SLOUČENÍ](http://msdn.microsoft.com/library/dd541276.aspx).

Pokud aktualizujete filtr, může trvat až minut too2 pravidla hello toorefresh koncový bod streamování. Pokud obsah hello zpracování pomocí tohoto filtru (a uložené v mezipaměti v proxy servery a CDN mezipaměti), aktualizace tento filtr může způsobit selhání přehrávač. Je vhodné tooclear hello mezipaměti po aktualizaci hello filtru. Pokud tato možnost není možné, zvažte použití jiný filtr.  

### <a name="update-global-filters"></a>Aktualizace globálních filtrů
tooupdate globální filtr, použijte následující požadavky HTTP hello: 

#### <a name="http-request"></a>Požadavek HTTP
Hlavičky požadavku: 

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

Text žádosti: 

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

### <a name="update-local-assetfilters"></a>Aktualizovat místní AssetFilters
tooupdate místní filtr, použijte následující požadavky HTTP hello: 

#### <a name="http-request"></a>Požadavek HTTP
Hlavičky požadavku: 

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

Text žádosti: 

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


## <a name="delete-filters"></a>Odstranit filtry
### <a name="delete-global-filters"></a>Odstranit globálních filtrů
toodelete globální filtr, použijte následující požadavky HTTP hello:

#### <a name="http-request"></a>Požadavek HTTP
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a>Odstranit místní AssetFilters
toodelete místní AssetFilter, použijte následující požadavky HTTP hello:

#### <a name="http-request"></a>Požadavek HTTP
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

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

