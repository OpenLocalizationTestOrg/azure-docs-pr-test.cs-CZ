---
title: "Přehled aaaMedia služby operace REST API | Microsoft Docs"
description: "Přehled Media Services REST API"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b54f4d9123486d6cae832c9817688b0f3da5b401
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-operations-rest-api-overview"></a>Přehled rozhraní REST API operations Media Services
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Hello **Media Services operace REST** rozhraní API slouží k vytvoření úlohy, prostředky, zásady přístupu a další operace na objekty v účtu služby Media Service. Další informace najdete v tématu [Media Services operace REST API odkaz](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Microsoft Azure Media Services je služba, která přijímá požadavky HTTP na základě OData a zpátky v podrobné JSON nebo atom + pub může reagovat. Protože Media Services vyhovuje pokynů pro návrh tooAzure, je sada požadované hlavičky HTTP, které každý klient musí použít při připojení služby tooMedia, jakož i sadu volitelné hlavičky, které lze použít. Hello následující části popisují hello hlavičky a příkazy HTTP, můžete použít při vytváření žádostí o a příjem odpovědí ze služby Media Services.

Toto téma poskytuje přehled o toouse REST v2 pomocí služby Media Services.

## <a name="considerations"></a>Požadavky

Hello následující aspekty použít při používání REST.

* Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky too1000 výsledky dotazu. Budete potřebovat toouse **přeskočit** a **trvat** (.NET) nebo **horní** (REST), jak je popsáno v [v tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [toto rozhraní API REST Příklad](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Při použití formátu JSON a zadání toouse hello **__metadata** – klíčové slovo v hello požadavku (například tooreferences propojeného objektu), je nutné nastavit hello **přijmout** záhlaví příliš[JSON podrobný formát ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (viz následující ukázka hello). OData nerozumí hello **__metadata** vlastnost hello požadavku, pokud ji nastavíte tooverbose.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standardní hlavičky požadavku HTTP podporovaných službou Media Services
Pro každé volání provedete ve službě Media Services, je sada požadované hlavičky musí zahrnovat ve vaší žádosti a také sadu volitelné záhlaví může být vhodné tooinclude. Následující tabulka seznamy hello Hello požadované hlavičky:

| Záhlaví | Typ | Hodnota |
| --- | --- | --- |
| Autorizace |Nosiče |Nosiče je, že hello pouze přijata mechanismus ověřování. Hodnota Hello musí také obsahovat hello přístupový token poskytovaný službou ACS. |
| x-ms-version |Decimal |2.11 |
| DataServiceVersion |Decimal |3.0 |
| MaxDataServiceVersion |Decimal |3.0 |

> [!NOTE]
> Protože služba Media Services použije OData tooexpose své základní úložiště asset metadat prostřednictvím rozhraní API REST, hello DataServiceVersion a MaxDataServiceVersion záhlaví by měl být součástí každá žádost; ale pokud tomu tak není, pak aktuálně Media Services předpokládá, že hodnota DataServiceVersion hello používá je 3.0.
> 
> 

Následující Hello je sada volitelné hlavičky:

| Záhlaví | Typ | Hodnota |
| --- | --- | --- |
| Datum |RFC 1123 datum |Časové razítko požadavku hello |
| Přijmout |Typ obsahu |Hello požadovaný typ obsahu pro odpověď hello například hello následující:<p> -application/json; odata = verbose<p> -application/atom + xml<p> Odpovědí může mít jiný typ obsahu, jako je například načtení objektu blob, kde úspěšné odpovědi bude obsahovat datový proud blob hello jako datovou část hello. |
| Přijměte kódování |GZIP, deflate |GZIP a DEFLATE kódování, v případě potřeby. Poznámka: Pro velké prostředky Media Services může ignorovat tuto hlavičku a vrátit nekomprimované data. |
| Přijměte jazyk |"en", "es" a tak dále. |Určuje hello upřednostňovaný jazyk pro odpověď hello. |
| Accept-Charset |Znaková sada typu like "Znakové sady UTF-8" |Výchozí hodnota je UTF-8. |
| Metoda HTTP X- |Metoda HTTP |Umožňuje klientům nebo brány firewall, které nepodporují metody HTTP, třeba PUT nebo odstranění toouse tyto metody tunelovým propojením prostřednictvím volání GET. |
| Content-Type |Typ obsahu |Typ obsahu hello textu žádosti PUT nebo POST požadavků. |
| Client-request-id |Řetězec |Volající definované hodnotu, která identifikuje hello danou žádost. -Li zadána, tato hodnota bude obsažena v uvítací zprávu odpovědi jako žádost způsob toomap hello. <p><p>**Důležité upozornění**<p>Hodnoty musí být limitován 2096b (2 kB). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Hlavičky standardních odpovědí HTTP podporovaných službou Media Services
Hello následuje sadu hlaviček, které mohou být vráceny tooyou v závislosti na hello prostředek, který se požaduje a hello určený tooperform akce.

| Záhlaví | Typ | Hodnota |
| --- | --- | --- |
| id žádosti |Řetězec |Jedinečný identifikátor pro aktuální operaci hello service generuje. |
| Client-request-id |Řetězec |Identifikátor určeného hello volající v původní žádost hello, pokud je k dispozici. |
| Datum |RFC 1123 datum |Datum Hello zpracování tohoto požadavku hello. |
| Content-Type |Je to různé. |Typ obsahu Hello hello textu odpovědi. |
| Kódování obsahu |Je to různé. |Gzip a deflate podle potřeby. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Standardní příkazy HTTP podporovaných službou Media Services
Následující Hello je úplný seznam příkazů HTTP, které lze použít při vytváření HTTP požadavků:

| Příkaz | Popis |
| --- | --- |
| GET |Vrátí hello aktuální hodnotu objektu. |
| POST |Vytvoří objekt na základě dat hello zadaná nebo odešle příkaz. |
| PUT |Nahradí objekt nebo vytvoří objekt s názvem (v případě potřeby). |
| ODSTRANIT |Odstraní objekt. |
| SLOUČENÍ |Aktualizuje existující objekt s názvem vlastnosti změny. |
| HEAD |Vrátí metadata objektu pro GET odpověď. |

## <a name="discover-media-services-model"></a>Zjistit modelu Media Services
toomake Media Services entity intuitivnější hello $metadata operace můžete použít. Umožňuje tooretrieve všechny typy entit platný, vlastností entity, přidružení, funkce, akce a tak dále. Hello následující příklad ukazuje, jak tooconstruct hello URI: https://media.windows.net/API/$ metadat.

By měl připojit "? api-version=2.x" toohello konec hello identifikátor URI, chcete-li tooview hello metadata v prohlížeči, nebo nezahrnujte záhlaví x-ms-version hello ve vaší žádosti.

## <a name="connect-toomedia-services"></a>Připojení služby tooMedia

Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání toohello nový identifikátor URI.

## <a name="next-steps"></a>Další kroky

tooaccess rozhraní API pro AMS se zbytkem, najdete v části [používání Azure AD authentication tooaccess hello rozhraní API služby Azure Media Services se zbytkem](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

