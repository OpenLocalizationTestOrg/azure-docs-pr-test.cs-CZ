---
title: "AAA \"dotazování indexu (rozhraní REST - API Azure Search) | Microsoft Docs\""
description: "Sestavení vyhledávacího dotazu ve službě Azure search a pomocí vyhledávání parametrů toofilter a řazení výsledků vyhledávání."
services: search
documentationcenter: 
manager: jhubbard
author: ashmaka
ms.assetid: 8b3ca890-2f5f-44b6-a140-6cb676fc2c9c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/12/2017
ms.author: ashmaka
ms.openlocfilehash: 2f12238b8f4b045f536489cfc8766fb68307bbe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-rest-api"></a>Dotazování indexu Azure Search pomocí REST API hello
> [!div class="op_single_selector"]
>
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Tento článek ukazuje, jak tooquery indexu pomocí hello [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/).

Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md) a ten by měl být [naplněný daty](search-what-is-data-import.md). Rozšiřující informace najdete v tématu popisujícím [způsob fungování fulltextového vyhledávání ve službě Azure Search](search-lucene-query-architecture.md).

## <a name="identify-your-azure-search-services-query-api-key"></a>Zjistěte klíč api-key správce služby Azure Search
Klíčovou součástí každé operace vyhledávání na hello REST API služby Azure Search je hello *klíč api-key* který byl vygenerován pro službu hello jste zřídili. Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.

1. toofind klíče služby api Key, se můžete přihlásit toohello [portálu Azure](https://portal.azure.com/)
2. Okno služby Azure Search přejděte tooyour
3. Klikněte na ikonu hello "klíčů.

Vaše služba má *klíče správce* a *klíče dotazů*.

* Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat.. Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.
* Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.

Pro účely hello dotazování indexu můžete použít jeden z klíčů dotazů. Pro dotazy lze také použít klíče správce, ale byste měli používat klíče dotazů v kódu aplikace, což lépe odpovídá hello [Princip nejnižších nutných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="formulate-your-query"></a>Formulování dotazu
Existují dva způsoby příliš[vyhledávání v indexu pomocí REST API hello](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Jedním ze způsobů je tooissue požadavek HTTP POST, kde jsou definovány parametry dotazu v objektu JSON v textu žádosti hello. Hello jiný způsob je tooissue požadavek HTTP GET, kde jsou definovány parametry dotazu v adrese URL žádosti hello. Metoda POST má [mírnější omezení](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) na hello velikosti parametrů dotazu než metoda GET. Z tohoto důvodu doporučujeme používat metodu POST, pokud pro vás neplatí zvláštní podmínky, kdy by bylo pohodlnější použití metody GET.

U metody POST i GET budete potřebovat tooprovide vaše *název služby*, *název indexu*a hello správné *verze rozhraní API* (aktuální verze rozhraní API hello je `2016-09-01` v době hello publikování tohoto dokumentu) v hello adresa URL požadavku. U metody GET hello *řetězec dotazu* v hello je konec hello adresu URL, kde zadáte parametry dotazu hello. Formát adresy URL hello jsou uvedeny níže:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

Hello formátu pro POST je hello stejné, ale pouze api-version hello parametrů řetězce dotazu.

#### <a name="example-queries"></a>Ukázky dotazů
Zde naleznete několik ukázky dotazů na index s názvem „hotels“. Dotazy jsou ukázané ve formátech GET i POST.

Vyhledejte hello výraz "budget" hello celý index a vrátí pouze hello `hotelName` pole:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

Použít filtru toohello index toofind hotelů levnějších než 150 dolarů za noc a vrátit hello `hotelId` a `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Hledání hello celý index, řadit podle určitého pole (`lastRenovationDate`) v sestupném pořadí, vzít první dva výsledky hello a zobrazit pouze `hotelName` a `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="submit-your-http-request"></a>Odeslání požadavku HTTP
Nyní, když jste formulovali dotaz jako součást URL požadavku HTTP (pro metodu GET) nebo textu požadavku HTTP (pro metodu POST), můžete definovat hlavičky požadavku a odeslat dotaz.

#### <a name="request-and-request-headers"></a>Požadavek a hlavičky požadavku
Musíte definovat dvě hlavičky požadavku pro metodu GET, nebo tři hlavičky pro metodu POST.

1. Hello `api-key` záhlaví musí být nastaven klíč dotazu toohello jste získali v kroku I výše. Můžete také použít klíč správce jako hello `api-key` záhlaví, ale doporučuje se používat klíče dotazů, protože výhradně uděluje oprávnění jen pro čtení tooindexes a dokumenty.
2. Hello `Accept` záhlaví musí být nastaven příliš`application/json`.
3. U metody POST, hello `Content-Type` záhlaví měli nastavit také příliš`application/json`.

Níže najdete metody GET protokolu HTTP žádosti toosearch hello "hotels" indexu pomocí REST API služby Azure Search, s jednoduchým dotazem, který vyhledá hello výraz "motel" hello:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

Tady je hello stejný vzorový dotaz, tentokrát pomocí HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Po úspěšném požadavku dotazu bude mít za následek stavový kód `200 OK` a hello výsledky vyhledávání se vrátí jako JSON v textu odpovědi hello. Tady je co hello výsledky pro hello výše dotazu vypadají, za předpokladu, že je index "hello"hotels"naplněný hello vzorovými daty v [hello Import dat do služby Azure Search pomocí REST API](search-import-data-rest-api.md) (Všimněte si, že hello JSON pro přehlednost zformátován).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

toolearn víc, navštivte část "Odpověď" hello [vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Další informace o stavových kódech HTTP, které se mohou vrátit v případě selhání, naleznete v tématu [Stavové kódy HTTP (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).
