---
title: "aaaHow toouse Fiddler tooevaluate a testování rozhraní REST API Azure Search | Microsoft Docs"
description: "Použijte aplikaci Fiddler s dostupností Azure Search bezkódovou tooverifying a vyzkoušení hello rozhraní REST API."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
ms.openlocfilehash: 2912e1180717d7b40a1e4f7f7f00daf2cc254f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-fiddler-tooevaluate-and-test-azure-search-rest-apis"></a>Použijte aplikaci Fiddler tooevaluate a testování rozhraní REST API Azure Search
> [!div class="op_single_selector"]
>
> * [Přehled](search-query-overview.md)
> * [Průzkumník služby Search](search-explorer.md)
> * [Fiddler](search-fiddler.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Tento článek vysvětluje, jak toouse aplikaci Fiddler, je dostupná [zdarma ke stažení na webu Telerik](http://www.telerik.com/fiddler), tooissue HTTP požadavků tooand zobrazení odpovědí pomocí hello REST API služby Azure Search, bez nutnosti toowrite žádný kód. Azure Search je plně spravovaná, hostovaná cloudová vyhledávací služba v Microsoft Azure, která je snadno programovatelná prostřednictvím rozhraní .NET a REST API. Hello služby Azure Search jsou popsaná rozhraní REST API na [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

V hello následující kroky budete vytvoření indexu, nahrávat dokumenty, dotazování hello indexu a systému hello dotazu informace o službě.

Tyto kroky toocomplete, budete potřebovat službu Azure Search a `api-key`. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) pokyny, jak tooget spuštěna.

## <a name="create-an-index"></a>Vytvoření indexu
1. Spusťte aplikaci Fiddler. Na hello **soubor** nabídce vypnout **zachycování provozu** toohide irelevantní aktivitu protokolu HTTP, je nesouvisejícími toohello aktuální úlohy.
2. Na hello **autora** kartě zformulujete požadavek, který vypadá jako hello následující snímek obrazovky.

      ![][1]
3. Vyberte **PUT**.
4. Zadejte adresu URL, která určuje adresu URL služby hello atributy požadavku a hello api-version. Několik tookeep ukazatele na paměti:

   * Jako předpona hello použijte protokol HTTPS.
   * Atribut žádosti je „/indexes/hotels“. Tato hodnota informuje vyhledávání toocreate index s názvem "hotely".
   * Verze rozhraní API se zadává malými písmeny jako „?api-version=2016-09-01“. Verze rozhraní API jsou důležité, protože služba Azure Search pravidelně nasazuje aktualizace. Ve výjimečných případech mohou aktualizace služby zavést narušující změny toohello rozhraní API. Z tohoto důvodu služba Azure Search v každé žádosti vyžaduje verzi rozhraní API, abyste měli plně pod kontrolou, která verze se použije.

     Hello úplnou adresu URL by měla vypadat podobně jako toohello následující ukázka.

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. Zadejte hlavičku požadavku hello, nahraďte hello hostitele a klíč api-key hodnoty, které jsou platné pro vaši službu.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. V textu žádosti vložte pole hello, která tvoří definici indexu hello.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }
7. Klikněte na tlačítko **Spustit**.

Během pár sekund měli byste vidět odpověď HTTP 201 v seznamu relace hello, naznačuje hello index úspěšně vytvořil.

Pokud se zobrazí kód HTTP 504, ověřte, zda že Hello URL určený protokol HTTPS. Pokud se zobrazí kód HTTP 400 nebo 404, zkontrolujte žádost hello textu tooverify došlo k chybám způsobené kopírováním a vkládáním. HTTP 403 obvykle znamená potíže s hello klíč api-key (neplatný klíč nebo syntaxe problém s jak je zadán hello klíč api-key).

## <a name="load-documents"></a>Nahrání dokumentů
Na hello **autora** kartě dokumentů toopost požadavek bude vypadat jako následující hello. text Hello hello žádosti obsahuje data hello hledání pro 4 hotely.

   ![][2]

1. Vyberte **POST**.
2. Zadejte adresu URL, která začíná HTTPS a dál obsahuje adresu URL služby a „/indexes/<'indexname'>/docs/index?api-version=2016-09-01“. Hello úplnou adresu URL by měla vypadat podobně jako toohello následující ukázka.

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. Hlavička žádosti by měla být hello stejné jako předtím. Nezapomeňte nahradit hello hostitele a klíč api-key s hodnotami, které jsou platné pro vaši službu.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Hello text žádosti obsahuje čtyři dokumenty toobe přidané toohello hotels index.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be hello one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }
5. Klikněte na tlačítko **Spustit**.

Během pár sekund měli byste vidět odpověď HTTP 200 v seznamu relace hello. To znamená hello dokumenty úspěšně vytvořily. Pokud se zobrazí kód 207, nejméně jeden dokument se nepovedlo tooupload. Pokud se zobrazí kód 404, máte chyby syntaxe v záhlaví hello nebo textu hello požadavku.

## <a name="query-hello-index"></a>Dotazování indexu hello
Teď, když jsou index a dokumenty nahrané, můžete na ně vydávat dotazy.  Na hello **autora** kartě **získat** příkaz, který dotazuje službu bude vypadat podobně jako toohello následující snímek obrazovky.

   ![][3]

1. Vyberte **GET**.
2. Zadejte adresu URL, která začíná HTTPS a dál obsahuje adresu URL služby, „/indexes/<'indexname'>/docs?“ a nakonec parametry dotazu. Jako příklad použijte hello následující adresu URL, nahraďte název hostitele ukázka hello ten, který je platný pro vaši službu.

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   Tento dotaz hledá hello výraz "motel" a načte kategorie faset pro hodnocení.
3. Hlavička žádosti by měla být hello stejné jako předtím. Nezapomeňte nahradit hello hostitele a klíč api-key s hodnotami, které jsou platné pro vaši službu.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

Kód odpovědi Hello by měla být 200 a hello odpověď by měla vypadat podobně jako toohello následující snímek obrazovky.

   ![][4]

Hello následující příklad dotazu je převzatý z hello [operace prohledání indexu (rozhraní API Azure Search)](http://msdn.microsoft.com/library/dn798927.aspx) na webu MSDN. Řadu hello příklad dotazy v tomto tématu obsahuje mezery, které nejsou v aplikaci Fiddler povolené. Nahraďte každou mezeru znakem + před vkládání v hello řetězec dotazu před pokusem o hello dotazu v aplikaci Fiddler.

**Před nahrazením mezer:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

**Po nahrazení mezer znakem +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-hello-system"></a>Dotaz hello systému
Můžete taky zadat dotaz hello systému tooget počty a úložiště využívání dokumentu. Na hello **autora** , požadavek bude vypadat podobně jako následující toohello a hello odpověď vrátí počet hello počet dokumentů a využité místo.

 ![][5]

1. Vyberte **GET**.
2. Zadejte adresu URL, která obsahuje adresu URL služby a potom „/indexes/hotels/stats?api-version=2016-09-01“:

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. Zadejte hlavičku požadavku hello, nahraďte hello hostitele a klíč api-key hodnoty, které jsou platné pro vaši službu.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Text žádosti hello ponechte prázdné.
5. Klikněte na tlačítko **Spustit**. Měli byste vidět stavový kód HTTP 200 v seznamu relace hello. Vyberte hello položku publikovanou k vašemu příkazu.
6. Klikněte na tlačítko hello **inspektoři** , klikněte na hello **hlavičky** kartu a pak vyberte hello formátu JSON. Měli byste vidět hello dokumentu počet a velikost úložiště (v KB).

## <a name="next-steps"></a>Další kroky
V tématu [Správa služby Search v Azure](search-manage.md) toomanaging žádný kód přístup a používání služby Azure Search.

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
