---
title: "aaaSynonyms náhled kurzu ve službě Azure Search | Microsoft Docs"
description: "Přidáte hello synonyma preview funkce tooan index ve službě Azure Search."
services: search
manager: jhubbard
documentationcenter: 
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 03/31/2017
ms.author: heidist
ms.openlocfilehash: 055c1cbafb945823a3dc4da0c522db236b1d192c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="synonym-preview-c-tutorial-for-azure-search"></a>Kurz synonym (Preview) v jazyce C# pro Azure Search

Synonyma odpovídající podle podmínek považují za vstupní termín sémanticky ekvivalentní toohello rozbalit dotazu. Například můžete "auto" toomatch dokumenty obsahující termíny hello "automobilu" nebo "vehicle".

Ve službě Azure Search se synonyma definují v *mapě synonym*, pomocí *pravidel mapování*, která přidružují ekvivalentní termíny. Můžete vytvořit více mapami synonymum, odešlete je jako index k dispozici tooany celé služby prostředků a pak odkazovat které jeden toouse na úrovni pole hello. V době dotazů kromě toosearching indexu Azure Search nepodporuje vyhledávání v mapě synonymum, pokud je zadaná pro pole použitého v dotazu hello.

> [!NOTE]
> Hello synonyma funkce je aktuálně ve verzi preview a podporována v pouze hello nejnovější verzi preview rozhraní API a verze sady SDK (api-version = 2016-09-01-Preview, verze SDK 4.x-preview). Podpora webu Azure Portal se v současnosti neposkytuje. Na rozhraní API ve verzi Preview se nevztahuje žádná smlouva SLA a funkce se také mohou změnit, proto je nedoporučujeme používat v produkčních aplikacích.

## <a name="prerequisites"></a>Požadavky

Kurz požadavky hello následující:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Služba Azure Search](search-create-service-portal.md)
* [Verze Preview knihovny Microsoft.Azure.Search .NET](https://aka.ms/search-sdk-preview)
* [Jak toouse Azure vyhledávání z aplikace .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Přehled

Před a po dotazy ukazují hello hodnotu synonyma. V tomto kurzu používáme ukázkovou aplikaci, která spustí dotazy a vrátí výsledky v ukázkovém indexu. Ukázková aplikace Hello vytvoří malé index s názvem "hotels" naplněný dva dokumenty. aplikace Hello provede vyhledávací dotazy pomocí podmínky a slovní spojení, která nejsou uvedena v indexu hello, povolí funkci synonyma hello, pak problémy hello stejné vyhledávání znovu. Hello uvedený níže ukazuje hello celkové toku.

```csharp
  static void Main(string[] args)
  {
      SearchServiceClient serviceClient = CreateSearchServiceClient();

      Console.WriteLine("{0}", "Cleaning up resources...\n");
      CleanupResources(serviceClient);

      Console.WriteLine("{0}", "Creating index...\n");
      CreateHotelsIndex(serviceClient);

      ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

      Console.WriteLine("{0}", "Uploading documents...\n");
      UploadDocuments(indexClient);

      ISearchIndexClient indexClientForQueries = CreateSearchIndexClient();

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Adding synonyms...\n");
      UploadSynonyms(serviceClient);
      EnableSynonymsInHotelsIndex(serviceClient);
      Thread.Sleep(10000); // Wait for hello changes toopropagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key tooend application...\n");

      Console.ReadKey();
  }
```
Hello toocreate kroky a naplnit index ukázka hello jsou vysvětlené v [jak toouse Azure vyhledávání z aplikace .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

## <a name="before-queries"></a>Dotazy „před“

V `RunQueriesWithNonExistentTermsInIndex` jsme vydali vyhledávací dotazy pro „five star“, „internet“ a „economy AND hotel“.
```csharp
Console.WriteLine("Search hello entire index for hello phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
Ani jeden z hello dva indexované dokumenty obsahují hello podmínky, proto jsme hello následující výstup z hello nejprve `RunQueriesWithNonExistentTermsInIndex`.
~~~
Search hello entire index for hello phrase "five star":

no document matched

Search hello entire index for hello term 'internet':

no document matched

Search hello entire index for hello terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a>Povolení synonym

Povolení synonyma je dvoustupňový proces. Nám nejdřív definovat a nahrát synonymum pravidla a pak nakonfigurujte toouse pole je. proces Hello popsané v `UploadSynonyms` a `EnableSynonymsInHotelsIndex`.

1. Přidáte mapu synonymum tooyour vyhledávací službu. V `UploadSynonyms`, jsme definovat čtyři pravidla v mapě naše synonymum, desc-synonymmap' a nahrát toohello služby.
```csharp
    var synonymMap = new SynonymMap()
    {
        Name = "desc-synonymmap",
        Format = "solr",
        Synonyms = "hotel, motel\n
                    internet,wifi\n
                    five star=>luxury\n
                    economy,inexpensive=>budget"
    };

    serviceClient.SynonymMaps.CreateOrUpdate(synonymMap);
```
Mapu synonymum musí odpovídat standard s otevřeným zdrojem toohello `solr` formátu. Formát Hello je vysvětleno v [synonyma ve službě Azure Search](search-synonyms.md) části hello `Apache Solr synonym format`.

2. Konfigurovat prohledatelná pole toouse hello synonymum mapu v definici indexu hello. V `EnableSynonymsInHotelsIndex`, povolíte synonyma na dvě pole `category` a `tags` podle nastavení hello `synonymMaps` název vlastnosti toohello hello nově nahrán synonymum mapy.
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
Po přidání mapy synonym není potřeba znovu sestavovat index. Můžete přidat službu tooyour mapy synonymum a potom změnit stávající definice pole v jakékoli index toouse hello nové synonymum mapování. Přidání nové atributy Hello nemá žádný vliv na dostupnost index. Hello totéž platí i v zakázání synonyma pro pole. Jednoduše můžete nastavit hello `synonymMaps` vlastnost tooan prázdný seznam.
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a>Dotazy „po“

Po nahrání hello synonymum mapy a hello index je aktualizovaný toouse hello synonymum mapy, hello druhý `RunQueriesWithNonExistentTermsInIndex` volání výstupy hello následující:

~~~
Search hello entire index for hello phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
Hello první dotaz najde hello dokumentu z pravidla hello `five star=>luxury`. druhý dotaz Hello rozšíří hello vyhledávání pomocí `internet,wifi` a hello třetí pomocí obou `hotel, motel` a `economy,inexpensive=>budget` v hledání dokumentů hello odpovídá.

Přidání synonyma zcela změní hello vyhledáváním. V tomto kurzu hello původní dotazů se nezdařilo smysluplný výsledky tooreturn i v případě, že byly relevantní hello dokumenty v indexu. Povolením synonyma jsme můžete rozbalit indexu tooinclude podmínkami společné použití, s daty toounderlying žádné změny v indexu hello.

## <a name="sample-application-source-code"></a>Zdrojový kód ukázkové aplikace
Můžete najít hello úplný zdrojový kód vzorové aplikace hello použitý v této ukázce na [Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).

## <a name="next-steps"></a>Další kroky

* Zkontrolujte [jak synonyma toouse ve službě Azure Search](search-synonyms.md)
* Projděte si [dokumentaci k rozhraní REST API pro synonyma](https://aka.ms/rgm6rq).
* Procházet hello odkazy pro hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) a [REST API](https://docs.microsoft.com/rest/api/searchservice/).
