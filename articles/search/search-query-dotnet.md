---
title: "AAA \"dotazování indexu (rozhraní .NET API - Azure Search) | Microsoft Docs\""
description: "Sestavení vyhledávacího dotazu ve službě Azure search a pomocí vyhledávání parametrů toofilter a řazení výsledků vyhledávání."
services: search
manager: jhubbard
documentationcenter: 
author: brjohnstmsft
ms.assetid: 12c3efba-ea99-4187-9d2d-f63b5ec7040d
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/19/2017
ms.author: brjohnst
ms.openlocfilehash: 8b3ba1cd1270aad038fb48d9053fcff35d243e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-net-sdk"></a>Dotazování indexu Azure Search pomocí .NET SDK hello
> [!div class="op_single_selector"]
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Tento článek vám ukáže, jak tooquery indexu pomocí hello [Azure Search .NET SDK](https://aka.ms/search-sdk).

Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md) a ten by měl být [naplněný daty](search-what-is-data-import.md).

> [!NOTE]
> Ukázkový kód v tomto článku je napsán v jazyce C#. Hello úplný zdrojový kód najdete [na Githubu](http://aka.ms/search-dotnet-howto). Můžete si také přečíst o hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) pro podrobnější procházení prostřednictvím hello ukázek kódu.

## <a name="identify-your-azure-search-services-query-api-key"></a>Zjistěte klíč api-key správce služby Azure Search
Teď, když jste vytvořili index Azure Search, jste skoro hotový tooissue dotazy pomocí .NET SDK hello. Nejprve budete potřebovat tooobtain mezi hello dotazu klíče api Key vytvořených pro hello vyhledávací službu, kterou jste zřídili. Hello .NET SDK odešle tento klíč rozhraní api služby tooyour každý požadavek. Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.

1. toofind vaší služby klíče api Key přihlášením toohello [portálu Azure](https://portal.azure.com/)
2. Okno služby Azure Search přejděte tooyour
3. Klikněte na hello ikonu klíčů."

Vaše služba bude mít *klíče správce* a *klíče dotazů*.

* Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat.. Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.
* Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.

Pro účely hello dotazování indexu můžete použít jeden z klíčů dotazů. Pro dotazy lze také použít klíče správce, ale byste měli používat klíče dotazů v kódu aplikace, což lépe odpovídá hello [Princip nejnižších nutných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="create-an-instance-of-hello-searchindexclient-class"></a>Vytvoření instance třídy SearchIndexClient hello
tooissue dotazy s hello .NET SDK služby Azure Search, budete potřebovat toocreate instanci hello `SearchIndexClient` třídy. Tato třída obsahuje několik konstruktorů. Dobrý den, ten, který chcete přebírá název vaší vyhledávací služby, název indexu a `SearchCredentials` jako parametry. `SearchCredentials` zabalí váš klíč api-key.

Hello následující kód vytvoří novou `SearchIndexClient` pro index "hotels" hello (vytvořené v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md)) pomocí hodnot pro název vyhledávací služby hello a rozhraní api klíče, které jsou uložené v souboru config aplikace hello soubor (`appsettings.json` v případě hello hello [ukázkové aplikace](http://aka.ms/search-dotnet-howto)):

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient` obsahuje vlastnost `Documents`. Tato vlastnost poskytuje že všechny metody, které potřebujete indexů Azure Search tooquery hello.

## <a name="query-your-index"></a>Dotázání indexu
Hledání s hello .NET SDK je jednoduché, volání hello `Documents.Search` metoda na vaše `SearchIndexClient`. Tato metoda přijímá několik parametrů včetně textu hello vyhledávání, spolu s `SearchParameters` objekt, který lze použít toofurther upravit hello dotazu.

#### <a name="types-of-queries"></a>Typy dotazů
Hello dva hlavní [typy dotazů](search-query-overview.md#types-of-queries) použijete jsou `search` a `filter`. Dotaz `search` vyhledává jeden nebo více výrazů ve všech *prohledávatelných* polích v indexu. Dotaz `filter` vyhodnocuje logický výraz na všech *filtrovatelných* polích v indexu.

Vyhledávání i filtrování se provádí pomocí hello `Documents.Search` metoda. Vyhledávací dotaz lze předat ve hello `searchText` parametr, zatímco výraz filtru lze předat v hello `Filter` vlastnost hello `SearchParameters` třídy. toofilter bez vyhledávání, stačí předat `"*"` pro hello `searchText` parametr. toosearch bez filtrování, ponechte hello `Filter` vlastnost nenastavenou nebo nepředávejte `SearchParameters` instance vůbec.

#### <a name="example-queries"></a>Ukázky dotazů
Hello následující vzorový kód ukazuje několik různých způsobů tooquery hello "hotels" indexu definované v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md#DefineIndex). Všimněte si, že hello dokumenty vrácené ve výsledcích hledání hello jsou instancemi třídy hello `Hotel` třída, která byla definovaná v [hello Import dat do služby Azure Search pomocí .NET SDK](search-import-data-dotnet.md#HotelClass). Hello ukázkový kód využívá `WriteDocuments` metoda toooutput hello vyhledávání výsledky toohello konzoly. Tato metoda je popsaná v další části hello.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search hello entire index for hello term 'budget' and return only hello hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter toohello index toofind hotels cheaper than $150 per night, ");
Console.WriteLine("and return hello hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search hello entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take hello top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a>Zpracování výsledků vyhledávání
Hello `Documents.Search` metoda vrátí `DocumentSearchResult` objekt, který obsahuje výsledky dotazu hello hello. Příklad Hello v předchozí části hello používá metodu s názvem `WriteDocuments` toooutput hello vyhledávání výsledky toohello konzoly:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Tady je co najít hello výsledky jako pro dotazy hello v předchozí části hello, v případě index hello "hotels" naplněný hello vzorovými daty v [hello Import dat do služby Azure Search pomocí .NET SDK](search-import-data-dotnet.md):

```
Search hello entire index for hello term 'budget' and return only hello hotelName field:

Name: Roach Motel

Apply a filter toohello index toofind hotels cheaper than $150 per night, and return hello hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close tootown hall and hello river

Search hello entire index, order by a specific field (lastRenovationDate) in descending order, take hello top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search hello entire index for hello term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

Hello výše uvedený ukázkový kód využívá výsledky hledání toooutput hello konzoly. Stejně tak budete potřebovat toodisplay výsledků vyhledávání ve vaší vlastní aplikaci. V tématu [této ukázce na Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) příklad jak výsledky hledání toorender založené na ASP.NET MVC webové aplikace.

