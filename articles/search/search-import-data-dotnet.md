---
title: "AAA \"nahrání dat (.NET - Azure Search) | Microsoft Docs\""
description: "Zjistěte, jak hello tooupload data tooan index Azure Search pomocí .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: 
ms.assetid: 0e0e7e7b-7178-4c26-95c6-2fd1e8015aca
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/13/2017
ms.author: brjohnst
ms.openlocfilehash: 78ddbefb522884d1f61cb275c25c091487aee639
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-net-sdk"></a>Nahrávání dat tooAzure vyhledávání pomocí .NET SDK hello
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Tento článek vám ukáže, jak toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport data do indexu Azure Search.

Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md). Tento článek také předpokládá, že jste již vytvořili `SearchServiceClient` objektu, jak je znázorněno v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md#CreateSearchServiceClient).

> [!NOTE]
> Ukázkový kód v tomto článku je napsán v jazyce C#. Hello úplný zdrojový kód najdete [na Githubu](http://aka.ms/search-dotnet-howto). Můžete si také přečíst o hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) pro podrobnější procházení prostřednictvím hello ukázek kódu.

V pořadí toopush dokumentů do indexu pomocí .NET SDK hello budete muset:

1. Vytvoření `SearchIndexClient` indexu vyhledávání tooyour tooconnect objektu.
2. Vytvoření `IndexBatch` obsahující dokumenty toobe hello přidat, upravit nebo odstranit.
3. Volání hello `Documents.Index` metodu vaše `SearchIndexClient` toosend hello `IndexBatch` tooyour indexu vyhledávání.

## <a name="create-an-instance-of-hello-searchindexclient-class"></a>Vytvoření instance třídy SearchIndexClient hello
hello tooimport data do indexu pomocí .NET SDK služby Azure Search, budete potřebovat toocreate instanci hello `SearchIndexClient` třídy. Můžete vytvořit tato instance sami, ale snadnější Pokud již máte `SearchServiceClient` instance toocall jeho `Indexes.GetClient` metoda. Například, zde je jak lze získat `SearchIndexClient` pro hello index s názvem "hotels" z `SearchServiceClient` s názvem `serviceClient`:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> V typické vyhledávací aplikaci se o správu a naplňování indexu stará samostatná komponenta volaná dotazy vyhledávání. `Indexes.GetClient`je vhodné pro naplňování indexu, protože díky tomu můžete hello poskytovat další `SearchCredentials`. Dělá to pomocí předání klíče správce hello této můžete použít toocreate hello `SearchServiceClient` toohello nové `SearchIndexClient`. V rámci hello aplikace, který provádí dotazy, je však lepší hello toocreate `SearchIndexClient` přímo tak, že abyste mohli předávat klíč dotazů místo klíče správce. To je konzistentní s hello [Princip nejnižších nutných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) a pomůže toomake bezpečnější aplikace. Můžete najít další informace o klíčích a správce dotazu v hello [referenční dokumentace rozhraní API REST Azure Search](https://docs.microsoft.com/rest/api/searchservice/).
> 
> 

`SearchIndexClient` obsahuje vlastnost `Documents`. Tato vlastnost poskytuje všechny metody hello tooadd, musíte změnit, odstranit nebo dotazování dokumentů v indexu.

## <a name="decide-which-indexing-action-toouse"></a>Rozhodněte, které indexování toouse akce
tooimport dat pomocí .NET SDK hello, budete potřebovat toopackage dat do `IndexBatch` objektu. `IndexBatch` Zapouzdřuje kolekci `IndexAction` objekty, z nichž každý obsahuje dokument a vlastnost, která říká službě Azure Search tooperform jaké akce na tomto dokumentu (odeslání, sloučení, odstranění atd.). Podle toho, která hello níže akce, který zvolíte musí být objekt pro každý dokument obsahovat pouze určitá pole:

| Akce | Popis | Potřebná pole pro každý dokument | Poznámky |
| --- | --- | --- | --- |
| `Upload` |`Upload` Akci je podobný tooan "upsert", kde hello dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje. |klíč a další pole chcete toodefine |Pokud aktualizujete nebo nahrazujete stávající dokument, bude každé pole, které není určený v požadavku hello mít nastavené příliš`null`. K tomu dojde i v případě, že bylo hello pole dříve nastavené tooa jinou hodnotu než null. |
| `Merge` |Aktualizace stávající dokumentů s hello zadaná pole. Pokud hello dokument v indexu hello neexistuje, sloučení hello se nezdaří. |klíč a další pole chcete toodefine |Každé pole zadané ve sloučení nahradí stávající pole hello v dokumentu hello. To zahrnuje i pole typu `DataType.Collection(DataType.String)`. Například pokud hello dokument obsahuje pole `tags` s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro `tags`, hello konečná hodnota hello `tags` pole bude `["economy", "pool"]`. Hodnota nebude `["budget", "economy", "pool"]`. |
| `MergeOrUpload` |Tato akce se chová jako `Merge` Pokud dokument s hello zadaný klíč již existuje v indexu hello. Pokud hello dokument neexistuje, chová se jako `Upload` s novým dokumentem. |klíč a další pole chcete toodefine |- |
| `Delete` |Odebere zadaný dokument hello hello index. |pouze klíč |Všechna zadaná pole kromě pole klíče hello budou ignorovány. Pokud chcete tooremove z dokumentu jednotlivá pole, použijte `Merge` místo a jednoduše nastavte pole hello explicitně toonull. |

Můžete určit, jaká opatření se mají toouse pomocí různých statických metod hello hello `IndexBatch` a `IndexAction` třídy, jak je znázorněno v další části hello.

## <a name="construct-your-indexbatch"></a>Vytvoření třídy IndexBatch
Teď, když víte, jaké akce tooperform na dokumentech, jste hello připravené tooconstruct `IndexBatch`. Příklad dole ukazuje jak Hello toocreate dávky s různými akcemi. Všimněte si, že naše Ukázka používá vlastní třídu s názvem `Hotel` který mapuje tooa dokument v indexu "hotels" hello.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close tootown hall and hello river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

V tomto případě používáme `Upload`, `MergeOrUpload`, a `Delete` jako akce hledání podle specifikace hello metody volat u hello `IndexAction` třídy.

Předpokládejme, že je ukázkový index „hotels“ již naplněný řadou dokumentů. Všimněte si, jak nebylo nutné toospecify všechny hello možná pole dokumentu při použití `MergeOrUpload` a jak jsme zadali pouze klíč dokumentu hello (`HotelId`) při použití `Delete`.

Také Upozorňujeme, že může obsahovat jenom too1000 dokumentů v jedné žádosti indexování.

> [!NOTE]
> V tomto příkladu používáme různé akce toodifferent dokumenty. Pokud byste chtěli tooperform hello stejné akce na všech dokumentech v dávce hello místo volání `IndexBatch.New`, můžete použít jiné statické metody třídy hello `IndexBatch`. Dávky můžete vytvořit například voláním `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` nebo `IndexBatch.Delete`. Tyto metody přijímají místo objektů `IndexAction` kolekci dokumentů (v této ukázce objekty typu `Hotel`).
> 
> 

## <a name="import-data-toohello-index"></a>Import dat toohello indexu
Teď, když jste inicializovali `IndexBatch` objekt, můžete ho odeslat toohello index voláním `Documents.Index` na vaše `SearchIndexClient` objektu. Následující příklad ukazuje, jak Hello toocall `Index`, spolu s některými dalšími kroky, které budete potřebovat tooperform:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of hello documents in
    // hello batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log hello failed document keys and continue.
    Console.WriteLine(
        "Failed tooindex some of hello documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents toobe indexed...\n");
Thread.Sleep(2000);
```

Poznámka: hello `try` / `catch` které obaluje volání toohello hello `Index` metoda. Hello blok catch ošetřuje s případem důležitých chyb pro indexování. Pokud služba Azure Search selže tooindex některé hello dokumentů v dávce hello `IndexBatchException` vyvolá výjimku `Documents.Index`. To může nastat v případě indexování dokumentů, zatímco je služba velmi zatížená. **Důrazně doporučujeme v kódu explicitně zpracovávat tento případ.** Můžete odložit a poté opakujte indexování hello dokumentů, které selhaly, nebo můžete protokolu a pokračovat jako hello znovu, nebo to můžete provést něco jiného v závislosti na požadavcích konzistence dat vaší aplikace.

Nakonec hello kód v příkladu hello výše na 2 sekundy odloží. Indexování probíhá asynchronně služby Azure Search, takže ukázková aplikace hello musí toowait krátkou dobu tooensure, že jsou k dispozici pro vyhledávání dokumentů hello. Tato odložení se obvykle používají pouze v ukázkových aplikacích a při testech.

<a name="HotelClass"></a>

### <a name="how-hello-net-sdk-handles-documents"></a>Jak hello .NET SDK zpracovává dokumenty
Asi vás zajímá, jak hello Azure Search .NET SDK je možné tooupload instance uživatelsky definované třídy, jako je `Hotel` toohello index. toohelp odpověď na tuto otázku, podíváme se na hello `Hotel` třídy, která mapuje toohello schéma indexu definované v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md#DefineIndex):

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Hello nejprve thing toonotice je, že každá veřejná vlastnost třídy `Hotel` odpovídá tooa pole v definici indexu hello, ale s jedním zásadním rozdílem: název hello každého pole začíná malým písmenem ("camelCase"), zatímco název každé veřejné hello Vlastnost `Hotel` začíná velké písmeno ("pascalcase"). Toto je běžný scénář v .NET aplikacích provádějících datové vazby, kde je hello cílové schéma mimo hello kontrolu nad vývojář aplikace hello. Místo tooviolate hello směrnic pojmenování .NET ve vlastnosti názvy camelCase toho se dá zjistit hello SDK toomap hello vlastnost názvy toocamel případ automaticky s hello `[SerializePropertyNamesAsCamelCase]` atribut.

> [!NOTE]
> Hello .NET SDK služby Azure Search používá hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) tooserialize knihovny a deserializaci vaše vlastní model tooand objekty z formátu JSON. V případě potřeby lze serializaci přizpůsobit. Další podrobnosti najdete v části [Vlastní serializace pomocí technologie JSON.NET](search-howto-dotnet-sdk.md#JsonDotNet). Příkladem toho je použití hello hello `[JsonProperty]` atribut hello `DescriptionFr` vlastnost ve výše hello ukázkovém kódu.
> 
> 

Hello důležité hello `Hotel` třídy jsou hello datové typy veřejných vlastností hello. Hello .NET typy těchto vlastností mapy tootheir odpovídající typy polí v definici indexu hello. Například hello `Category` vlastnosti řetězce mapuje toohello `category` pole, které je typu `DataType.String`. Podobná mapování typu probíhají mezi `bool?` a `DataType.Boolean`, `DateTimeOffset?` a `DataType.DateTimeOffset` atd. Hello konkrétní pravidla pro hello mapování typu jsou popsaná u hello `Documents.Get` metoda v hello [Azure Search .NET SDK odkaz](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).

Tato možnost toouse vaše vlastní třídy jako dokumenty funguje v obou směrech; Můžete také načíst výsledky vyhledávání a nechat hello SDK automaticky deserializovala tooa typu, jak je znázorněno v hello [následující článek](search-query-dotnet.md).

> [!NOTE]
> Hello .NET SDK služby Azure Search také podporuje dynamicky typované dokumenty pomocí hello `Document` třída, která zajišťuje mapování klíč hodnota hodnot toofield názvy polí. To je užitečné v případech, pokud neznáte schéma indexu hello v době návrhu, nebo pokud je třídy modelu toospecific nepohodlná toobind. Všechny metody hello v hello SDK, které pracují s dokumenty, mají přetížení, které pracují s hello `Document` třídy, jakož i přetížení silně typované, která přebírají parametr obecného typu. Pouze hello pozdější se používají v hello ukázkový kód v tomto článku.
> 
> 

**Proč byste měli používat datové typy s možnou hodnotou null**

Při navrhování indexu Azure Search toomap tooan vlastní model třídy, doporučujeme deklarovat vlastnosti typů hodnot, jako `bool` a `int` toobe s možnou hodnotou Null (například `bool?` místo `bool`). Pokud použijete vlastnost se zakázanou hodnotou Null, máte příliš**zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hello hodnotu null. Hello SDK ani hello služby Azure Search vám pomůže tooenforce vám to.

Nejedná se pouze o hypotetický problém: Představte si situaci, kdy přidáte nový pole tooan existující index typu `DataType.Int32`. Po aktualizaci definice indexu hello, budou všechny dokumenty mít pro toto nové pole hodnotu null (protože jsou všechny typy s možnou hodnotou Null ve službě Azure Search). Pokud pak použijete třídu modelu s hodnotou Null `int` vlastnosti pro toto pole, zobrazí se `JsonSerializationException` při pokusu o tooretrieve dokumenty podobné výjimky:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Z tohoto důvodu doporučujeme jako osvědčený postup používat ve třídách modelu typy s možnou hodnotou null.

## <a name="next-steps"></a>Další kroky
Po naplnění indexu Azure Search, bude připravená toostart vystavování toosearch dotazy pro dokumenty. Podrobnosti naleznete v tématu [Dotazování indexu Azure Search](search-query-overview.md).

