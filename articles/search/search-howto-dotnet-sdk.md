---
title: aaaHow toouse Azure Search z aplikace .NET | Microsoft Docs
description: "Jak toouse Azure vyhledávání z aplikace .NET"
services: search
documentationcenter: 
author: brjohnstmsft
manager: jlembicz
editor: 
ms.assetid: 93653341-c05f-4cfd-be45-bb877f964fcb
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 8e13fbe5549547d65941b856ce5a90611261388f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-search-from-a-net-application"></a>Jak toouse Azure vyhledávání z aplikace .NET
Tento článek je tooget návod vám fungovaly s hello [Azure Search .NET SDK](https://aka.ms/search-sdk). Můžete použít hello .NET SDK tooimplement s formátováním možností vyhledávání do vaší aplikace pomocí Azure Search.

## <a name="whats-in-hello-azure-search-sdk"></a>Co je hello vyhledávání systému Azure SDK
Hello SDK se skládá z Klientská knihovna `Microsoft.Azure.Search`. Ji umožňuje vám toomanage vaše indexy, zdroje dat a indexery, a také nahrát a správě dokumentů a spouštět dotazy, aniž by bylo toodeal s podrobnostmi hello HTTP a JSON.

Definuje Hello klientské knihovny tříd jako `Index`, `Field`, a `Document`, stejně jako operací, jako `Indexes.Create` a `Documents.Search` na hello `SearchServiceClient` a `SearchIndexClient` třídy. Tyto třídy jsou uspořádány do hello následující obory názvů:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

aktuální verze Hello hello Azure Search .NET SDK je nyní všeobecně dostupná. Pokud chcete zpětnou vazbu tooprovide nám tooincorporate v další verzi hello, navštivte naše [zpětné vazby stránky](https://feedback.azure.com/forums/263029-azure-search/).

Hello .NET SDK podporuje verzi `2016-09-01` z hello [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/). Tato verze teď zahrnuje podporu pro vlastní analyzátorů a objektů Blob v Azure a Azure Table podpora indexeru. Zobrazit náhled funkce, které jsou *není* jsou součástí této verze, jako je podpora pro indexování soubory JSON a sdíleného svazku clusteru v [preview](search-api-2015-02-28-preview.md) a dostupný prostřednictvím hello starší [verze 2.0 preview hello .NET SDK ](https://aka.ms/search-sdk-preview).

Tato sada SDK nepodporuje [operace správy](https://docs.microsoft.com/rest/api/searchmanagement/) jako je například vytváření a škálování služby vyhledávání a správa klíče rozhraní API. Pokud potřebujete toomanage vašich vyhledávání prostředků z aplikace .NET, můžete použít hello [SDK služby Azure Search .NET správu](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-toohello-latest-version-of-hello-sdk"></a>Upgrade toohello nejnovější verzi hello SDK
Pokud už používáte starší verzi hello .NET SDK služby Azure Search a chcete tooupgrade toohello nová všeobecně dostupná verze, [v tomto článku](search-dotnet-sdk-migration.md) vysvětluje, jak.

## <a name="requirements-for-hello-sdk"></a>Požadavky pro hello SDK
1. Visual Studio 2017.
2. Vlastní službu Azure Search. V pořadí toouse hello SDK budete potřebovat hello název vaší služby a jeden či více klíčů rozhraní API. [Vytvoření služby portálu hello](search-create-service-portal.md) vám pomohou dokončit tyto kroky.
3. Stáhnout hello Azure Search .NET SDK [balíček NuGet](http://www.nuget.org/packages/Microsoft.Azure.Search) pomocí "Správa balíčků NuGet" v sadě Visual Studio. Právě vyhledejte název balíčku hello `Microsoft.Azure.Search` na NuGet.org.

Hello Azure Search .NET SDK podporuje aplikace cílené na hello rozhraní .NET Framework 4.6 a .NET Core.

## <a name="core-scenarios"></a>Základní scénáře
Existuje několik věcí, které budete potřebovat toodo v vyhledávací aplikaci. V tomto kurzu nabídneme tyto základní scénáře:

* Vytvoření indexu
* Naplňování indexu hello s dokumenty
* Hledání dokumentů pomocí fulltextové vyhledávání a filtry

Hello ukázkový kód, který následuje znázorňuje všechny z nich. Zaregistrované fragmenty kódu hello volné toouse ve vaší vlastní aplikaci.

### <a name="overview"></a>Přehled
Hello ukázkovou aplikaci jsme budete seznamovat vytvoří novou index s názvem "hotels", naplní s několika dokumenty a potom provede některé vyhledávací dotazy. Tady je hlavní programu hello zobrazující celkové toku hello:

```csharp
// This sample shows how toodelete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key tooend application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> Můžete najít hello úplný zdrojový kód vzorové aplikace hello použitý v této ukázce na [Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).
> 
>

Projdeme tento krok po kroku. Nejdřív potřebujeme toocreate nový `SearchServiceClient`. Tento objekt umožňuje toomanage indexy. Pořadí tooconstruct jeden je nutné tooprovide název vaší služby Azure Search, jakož i klíčem rozhraní API pro správu. Tyto informace můžete zadat v hello `appsettings.json` souboru hello [ukázkové aplikace](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> Pokud jste zadali nesprávné klávesy (například klíč dotazů kde nebyla nutná klíč správce), hello `SearchServiceClient` vyvolá výjimku `CloudException` s hello chybová zpráva "Zakázané" hello poprvé zavoláte metodu operace, jako například `Indexes.Create`. V takovém případě tooyou zkontrolujte naše klíč rozhraní API.
> 
> 

Hello další několik řádků volání metody toocreate index s názvem "hotels", nejprve odstraňování Pokud již existuje. Jsme provede tyto metody trochu později.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

V dalším kroku hello index musí toobe naplněno. toodo, je nutné zadat `SearchIndexClient`. Existují dva způsoby tooobtain jednu: vytváření ho nebo volání `Indexes.GetClient` na hello `SearchServiceClient`. Používáme hello pozdější ke zvýšení pohodlí.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> V typické vyhledávací aplikaci se o správu a naplňování indexu stará samostatná komponenta volaná dotazy vyhledávání. `Indexes.GetClient`je vhodné pro naplňování indexu, protože díky tomu můžete hello poskytovat další `SearchCredentials`. Dělá to pomocí předání klíče správce hello této můžete použít toocreate hello `SearchServiceClient` toohello nové `SearchIndexClient`. V rámci hello aplikace, který provádí dotazy, je však lepší hello toocreate `SearchIndexClient` přímo tak, že abyste mohli předávat klíč dotazů místo klíče správce. To je konzistentní s hello Princip nejnižších nutných oprávnění a pomůže toomake bezpečnější aplikace. Můžete najít další informace o klíčích a správce dotazu [zde](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Teď, když máme `SearchIndexClient`, naplníme hello index. K tomu je potřeba další metodou, vám ukážeme později.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Nakonec provést několik vyhledávací dotazy a zobrazit výsledky hello. Tentokrát použijeme jiné `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Jsme bude trvat bližší pohled na hello `RunQueries` metoda později. Zde je hello kód toocreate hello nového `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Tentokrát použijeme klíč dotazů vzhledem k tomu, že jsme nepotřebují přístup pro zápis toohello index. Tyto informace můžete zadat v hello `appsettings.json` souboru hello [ukázkové aplikace](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Pokud spustíte tuto aplikaci s platný název služby a klíče rozhraní API, hello výstup by měl vypadat například takto:

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents toobe indexed...
    
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
    
    Complete.  Press any key tooend application...

Úplný zdrojový kód Hello hello aplikace je k dispozici na konci hello tohoto článku.

V dalším kroku jsme bude trvat bližší pohled na každou z metod hello volá `Main`.

### <a name="creating-an-index"></a>Vytvoření indexu
Po vytvoření `SearchServiceClient`, další věc hello `Main` nemá je index odstranění hello "hotels", pokud již existuje. Která se provádí hello následující metodu:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Tato metoda používá hello zadané `SearchServiceClient` toocheck Pokud hello indexu existuje a pokud ano, odstraňte jej.

> [!NOTE]
> Hello ukázkový kód v tomto článku používá pro jednoduchost synchronní metody hello hello Azure Search .NET SDK. Doporučujeme použít hello asynchronních metod v tookeep vlastní aplikace je škálovatelné a dobře reagovaly. Například v hello metoda výše můžete použít `ExistsAsync` a `DeleteAsync` místo `Exists` a `Delete`.
> 
> 

Dále `Main` vytvoří nový index "hotels" voláním této metody:

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>()
    };

    serviceClient.Indexes.Create(definition);
}
```

Tato metoda vytvoří novou `Index` objekt seznam `Field` objekty, které definuje schéma nového indexu hello hello. Každé pole má název, datový typ a několika atributů, které definují chování vyhledávání. Hello `FieldBuilder` třída používá reflexe toocreate seznam `Field` objekty pro index hello prověřením hello veřejné vlastnosti a atributy hello zadané `Hotel` třída modelu. Provedeme bližší pohled na hello `Hotel` později na třídu.

> [!NOTE]
> Vždy můžete vytvořit seznam hello `Field` objekty přímo místo použití `FieldBuilder` v případě potřeby. Například chcete nemusí toouse třídu modelu, nebo může být nutné toouse existující třídy modelu, že nechcete, aby toomodify přidáním atributy.
>
> 

Kromě toho toofields, můžete také přidat vyhodnocování profily, trochu nebo toohello CORS možnosti indexu (tyto byly vynechány z ukázkové hello jako stručný výtah). Další informace o objektu hello Index a jeho složky můžete najít v hello [referenční informace sady SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), i jako v hello [referenční dokumentace rozhraní API REST Azure Search](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-hello-index"></a>Naplňování indexu hello
Hello další krok v `Main` je toopopulate hello nově vytvořený index. To se provádí v hello následující metodu:

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
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
        },
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
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close tootown hall and hello river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

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
}
```

Tato metoda obsahuje čtyři části. Hello nejprve vytvoří pole `Hotel` objekty, které bude sloužit jako index toohello tooupload vstupní data. Tato data jsou pevně pro jednoduchost. Ve vaší vlastní aplikaci bude vaše data pravděpodobně pocházet z externího zdroje dat například do databáze SQL.

vytvoří druhou částí Hello `IndexBatch` obsahující dokumenty hello. Zadejte chcete tooapply toohello batch v době hello vytvoříte, v takovém případě voláním operace hello `IndexBatch.Upload`. Hello batch je pak nahrané toohello Azure Search indexování hello `Documents.Index` metoda.

> [!NOTE]
> V tomto příkladu jsme právě nahráváte dokumenty. Pokud byste chtěli toomerge mění ve stávajících dokumentech nebo odstranění dokumentů, dávky můžete vytvořit voláním `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, nebo `IndexBatch.Delete` místo. Také je možné kombinovat různé operace v jedné dávkové voláním `IndexBatch.New`, která vezme kolekci `IndexAction` objekty, z nichž každý informuje Azure Search tooperform konkrétní operace v dokumentu. Můžete vytvořit každý `IndexAction` s vlastní operaci voláním hello odpovídající metoda `IndexAction.Merge`, `IndexAction.Upload`a tak dále.
> 
> 

Hello třetí součást tato metoda je blok catch, která zpracovává s případem důležitých chyb pro indexování. Pokud služba Azure Search selže tooindex některé hello dokumentů v dávce hello `IndexBatchException` vyvolá výjimku `Documents.Index`. To může nastat v případě indexování dokumentů, zatímco je služba velmi zatížená. **Důrazně doporučujeme v kódu explicitně zpracovávat tento případ.** Můžete odložit a poté opakujte indexování hello dokumentů, které selhaly, nebo můžete protokolu a pokračovat jako hello znovu, nebo to můžete provést něco jiného v závislosti na požadavcích konzistence dat vaší aplikace.

> [!NOTE]
> Můžete použít hello `FindFailedActionsToRetry` tooconstruct metoda nové dávky obsahující pouze hello akce, které se nezdařila v předchozích volání příliš`Index`. Metoda Hello je popsána [sem](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) a není diskuzi o tom, jak tooproperly použít [na StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Nakonec hello `UploadDocuments` na 2 sekundy odloží metoda. Indexování probíhá asynchronně služby Azure Search, takže ukázková aplikace hello musí toowait krátkou dobu tooensure, že jsou k dispozici pro vyhledávání dokumentů hello. Tato odložení se obvykle používají pouze v ukázkových aplikacích a při testech.

#### <a name="how-hello-net-sdk-handles-documents"></a>Jak hello .NET SDK zpracovává dokumenty
Asi vás zajímá, jak hello Azure Search .NET SDK je možné tooupload instance uživatelsky definované třídy, jako je `Hotel` toohello index. toohelp odpověď na tuto otázku, podíváme se na hello `Hotel` třídy:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// hello SerializePropertyNamesAsCamelCase attribute is defined in hello Azure Search .NET SDK.
// It ensures that Pascal-case property names in hello model class are mapped toocamel-case
// field names in hello index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
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
}
```

Hello nejprve thing toonotice je, že každá veřejná vlastnost třídy `Hotel` odpovídá tooa pole v definici indexu hello, ale s jedním zásadním rozdílem: název hello každého pole začíná malým písmenem ("camelCase"), zatímco název každé veřejné hello Vlastnost `Hotel` začíná velké písmeno ("pascalcase"). Toto je běžný scénář v .NET aplikacích provádějících datové vazby, kde je hello cílové schéma mimo hello kontrolu nad vývojář aplikace hello. Místo tooviolate hello směrnic pojmenování .NET ve vlastnosti názvy camelCase toho se dá zjistit hello SDK toomap hello vlastnost názvy toocamel případ automaticky s hello `[SerializePropertyNamesAsCamelCase]` atribut.

> [!NOTE]
> Hello .NET SDK služby Azure Search používá hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) tooserialize knihovny a deserializaci vaše vlastní model tooand objekty z formátu JSON. V případě potřeby lze serializaci přizpůsobit. Další podrobnosti najdete v tématu [vlastní serializace s JSON.NET](#JsonDotNet).
> 
> 

Hello druhý toonotice co jsou hello atributy, jako `IsFilterable`, `IsSearchable`, `Key`, a `Analyzer` , uspořádání každé veřejné vlastnosti. Tyto atributy přímo propojit toohello [odpovídající atributy indexu Azure Search hello](https://docs.microsoft.com/rest/api/searchservice/create-index#request). Hello `FieldBuilder` třída používá tyto definice pole tooconstruct pro hello index.

Hello třetí důležité hello `Hotel` třídy jsou hello datové typy veřejných vlastností hello. Hello .NET typy těchto vlastností mapy tootheir odpovídající typy polí v definici indexu hello. Například hello `Category` vlastnosti řetězce mapuje toohello `category` pole, které je typu `Edm.String`. Jsou podobná mapování typu mezi `bool?` a `Edm.Boolean`, `DateTimeOffset?` a `Edm.DateTimeOffset`, atd. hello konkrétní pravidla pro hello mapování typu jsou popsaná u hello `Documents.Get` metoda v hello [Azure Search .NET SDK referenční dokumentace](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_). Hello `FieldBuilder` třída má na starosti toto mapování pro vás, ale stále může být užitečné toounderstand v případě, že potřebujete tootroubleshoot problémy serializace.

Tato možnost toouse vaše vlastní třídy jako dokumenty funguje v obou směrech; Můžete také načíst výsledky vyhledávání a nechat hello SDK automaticky deserializovala tooa typu, protože jsme se zobrazí v další části hello.

> [!NOTE]
> Hello .NET SDK služby Azure Search také podporuje dynamicky typované dokumenty pomocí hello `Document` třída, která zajišťuje mapování klíč hodnota hodnot toofield názvy polí. To je užitečné v případech, pokud neznáte schéma indexu hello v době návrhu, nebo pokud je třídy modelu toospecific nepohodlná toobind. Všechny metody hello v hello SDK, které pracují s dokumenty, mají přetížení, které pracují s hello `Document` třídy, jakož i přetížení silně typované, která přebírají parametr obecného typu. Pouze hello pozdější se používají v hello ukázkový kód v tomto kurzu. Hello `Document` třída dědí z `Dictionary<string, object>`. Můžete najít další podrobnosti o [zde](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).
> 
> 

**Proč byste měli používat datové typy s možnou hodnotou null**

Při navrhování indexu Azure Search toomap tooan vlastní model třídy, doporučujeme deklarovat vlastnosti typů hodnot, jako `bool` a `int` toobe s možnou hodnotou Null (například `bool?` místo `bool`). Pokud použijete vlastnost se zakázanou hodnotou Null, máte příliš**zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hello hodnotu null. Hello SDK ani hello služby Azure Search vám pomůže tooenforce vám to.

Nejedná se pouze o hypotetický problém: Představte si situaci, kdy přidáte nový pole tooan existující index typu `Edm.Int32`. Po aktualizaci definice indexu hello, budou všechny dokumenty mít pro toto nové pole hodnotu null (protože jsou všechny typy s možnou hodnotou Null ve službě Azure Search). Pokud pak použijete třídu modelu s hodnotou Null `int` vlastnosti pro toto pole, zobrazí se `JsonSerializationException` při pokusu o tooretrieve dokumenty podobné výjimky:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Z tohoto důvodu doporučujeme jako osvědčený postup používat ve třídách modelu typy s možnou hodnotou null.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>Vlastní serializace s JSON.NET
Hello SDK používá technologii JSON.NET pro serializaci a deserializaci dokumenty. Můžete přizpůsobit serializace a deserializace v případě potřeby tak, že definujete vlastní `JsonConverter` nebo `IContractResolver` (viz hello [JSON.NET dokumentace](http://www.newtonsoft.com/json/help/html/Introduction.htm) podrobnosti). To může být užitečné, pokud chcete tooadapt existující třídy modelu z vaší aplikace pro použití se službou Azure Search a jiné pokročilejší scénáře. S vlastní serializace můžete například:

* Zahrnout nebo vyloučit některé vlastnosti vaší třídy modelu z uložené jako pole dokumentu.
* Mapování mezi názvy vlastností v kódu a názvy polí v indexu.
* Vytvoření vlastních atributů, které lze použít pro mapování polí toodocument vlastnosti.

Můžete najít příklady implementace vlastní serializace v hello testy částí pro hello Azure Search .NET SDK na Githubu. Je to dobrý výchozí bod [tato složka](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Obsahuje třídy, které jsou používány hello vlastní serializace testy.

### <a name="searching-for-documents-in-hello-index"></a>Hledání dokumentů v indexu hello
posledním krokem Hello hello ukázkové aplikace je toosearch u některých dokumentů v indexu hello. Následující metoda Hello provede toto:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
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
}
```

Pokaždé, když se provede dotaz, tato metoda první vytvoří novou `SearchParameters` objektu. Toto je použité toospecify další možnosti pro dotaz hello například třídění, filtrování, stránkování a používání omezujících vlastností. Tato metoda jsme se nastavení hello `Filter`, `Select`, `OrderBy`, a `Top` vlastnost pro různé dotazy. Všechny hello `SearchParameters` jsou popsané vlastnosti [zde](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

dalším krokem Hello je tooactually provést hello vyhledávací dotaz. To se provádí pomocí hello `Documents.Search` metoda. Pro každý dotaz jsme předat hello hledání textu toouse jako řetězec (nebo `"*"` Pokud neexistuje žádný hledaný text), plus hello vyhledávání parametry vytvořili dříve. Můžeme také určit `Hotel` jako parametr typu hello pro `Documents.Search`, která sděluje hello SDK toodeserialize dokumenty ve výsledcích hledání hello do objektů typu `Hotel`.

> [!NOTE]
> Můžete najít další informace o syntaxe výrazu dotazu vyhledávání hello [zde](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Nakonec po každém dotazu tato metoda iteruje všechny hello shody v hello výsledky hledání, tisk každé konzole toohello dokumentu:

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

Podívejme zase bližší pohled na každý dotaz hello. Tady je hello kód tooexecute hello první dotaz:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

V takovém případě jsme se hledající hotely, které odpovídají hello slovo "rozpočet", a chceme tooget zpět pouze hello hotelů názvy, podle specifikace hello `Select` parametr. Zde jsou výsledky hello:

    Name: Roach Motel

V dalším kroku jsme má toofind hello hotels s noční počet menší než 150 USD a vrátí pouze hello hotelů ID a popis:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Tento dotaz používá OData `$filter` výrazu `baseRate lt 150`, toofilter hello dokumenty v indexu hello. Můžete najít další informace o hello syntaxe OData, který podporuje Azure Search [zde](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Zde jsou hello výsledky dotazu hello:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close tootown hall and hello river

V dalším kroku chceme toofind hello nejvyšší dvě hotels které mají byl naposledy renovovanou a zobrazit název hotelů hello a datum posledního obnova. Tady je kód hello: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

V takovém případě znovu používáme OData syntaxe toospecify hello `OrderBy` parametr jako `lastRenovationDate desc`. Můžeme také nastavit `Top` too2 tooensure se nám pouze získat hello horních dvou dokumenty. Před, jsme nastavit jako `Select` toospecify pole, která má být vrácen.

Zde jsou výsledky hello:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Nakonec chcete toofind všechny hotels, které odpovídají hello slovo "motel":

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

A tady jsou hello výsledky, které zahrnují všechna pole, protože jsme nezadali hello `Select` vlastnost:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Dokončení tohoto kroku kurzu hello, ale není tady zastavit. **Další kroky** poskytuje další zdroje dalších informací o službě Azure Search.

## <a name="next-steps"></a>Další kroky
* Procházet hello odkazy pro hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) a [REST API](https://docs.microsoft.com/rest/api/searchservice/).
* Prohloubit vašich znalostí prostřednictvím [videa a jiné ukázky a výukové programy](search-video-demo-tutorial-list.md).
* Zkontrolujte [konvence vytváření názvů](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) toolearn hello pravidla pro pojmenovávání různé objekty.
* Zkontrolujte [podporované datové typy](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) ve službě Azure Search.
