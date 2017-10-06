---
title: aaaUpgrading toohello Azure Search .NET SDK verze 1.1 | Microsoft Docs
description: Upgrade toohello Azure Search .NET SDK verze 1.1
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 66f89958-a320-4a24-87f9-69315848909f
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 291ae5731546e47b3c22c721d3552a79bdea80c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-net-sdk-version-3"></a>Upgrade toohello Azure Search .NET SDK verze 3
Pokud používáte verzi 2.0 preview nebo starší z hello [Azure Search .NET SDK](https://aka.ms/search-sdk), tento článek vám pomůže při upgradu vaší aplikace toouse verze 3.

Další obecné návod hello SDK včetně příkladů, najdete v části [jak toouse Azure vyhledávání z aplikace .NET](search-howto-dotnet-sdk.md).

Verze 3 hello Azure Search .NET SDK obsahuje některé změny z předchozích verzí. Tyto jsou většinou dílčí, takže měnili kód měli vyžadovat jen minimální úsilí. V tématu [tooupgrade kroky](#UpgradeSteps) pokyny toochange váš kód toouse hello nové verze sady SDK.

> [!NOTE]
> Pokud používáte verzi 1.0.2-preview nebo starší, doporučujeme nejprve upgradovat tooversion 1.1 a pak upgradovat tooversion 3. V tématu [příloha: kroky tooupgrade tooversion 1.1](#UpgradeStepsV1) pokyny.
>
> Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně hello nejnovějšího. Toouse verze můžete pokračovat v případě, že je už hello nejnovějšího, ale doporučujeme migraci nejnovější verze vašeho kódu toouse hello. Při použití hello REST API, musíte zadat hello verze rozhraní API v každé žádosti o prostřednictvím parametr api-version hello. Při použití hello .NET SDK, určuje hello verzi hello SDK používáte hello odpovídající verzi systému hello REST API. Pokud používáte starší SDK, můžete dál toorun tento kód se žádné změny i v případě, že je služba hello upgradovaný toosupport novější rozhraní API verze.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Co je nového v verze 3
Verze 3 hello Azure Search .NET SDK cíle hello nejnovější obecně dostupná verze hello REST API služby Azure Search, konkrétně 2016-09-01. Díky tomu je možné toouse mnoha nových funkcí služby Azure Search z aplikace .NET, včetně hello následující:

* [Vlastní analyzátory](https://aka.ms/customanalyzers)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) a [Azure Table Storage](search-howto-indexing-azure-tables.md) podpora indexeru
* Přizpůsobení indexeru prostřednictvím [pole mapování](search-indexer-field-mappings.md)
* Značky etag binárním rozsáhlým podporují tooenable bezpečné souběžných aktualizaci indexu definice, indexery a zdroje dat.
* Podporu pro vytváření indexu pole definice deklarativně architekturu třídě modelu a použitím hello nové `FieldBuilder` třídy.
* Podpora pro .NET Core a přenosné profil .NET 111

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a>Kroky tooupgrade
Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí buď hello Konzola správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.

Jakmile NuGet stáhl hello nové balíčky a jejich závislosti, znovu sestavte projekt. V závislosti na tom, jak je strukturovaná kódu se může znovu sestavit úspěšně. Pokud ano, chystáte připravené toogo!

Pokud vaše sestavení selže, měli byste vidět chyby sestavení jako hello následující:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' too'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

dalším krokem Hello je toofix této chyby sestavení. V tématu [nejnovější změny ve verzi 3](#ListOfChanges) podrobnosti o co způsobí, že chyba hello a jak toofix ho.

Může se zobrazit další sestavení tooobsolete metody nebo vlastnosti související s upozorněními. Hello upozornění bude obsahovat pokyny, které toouse místo z hello zastaralé funkce. Například, pokud vaše aplikace používá hello `IndexingParameters.Base64EncodeKeys` vlastnost, měli byste obdržet upozornění, která uvádí, že`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Jakmile jste vyřešili všechny chyby sestavení, můžete provádět změny tooyour aplikace tootake využívat nové funkce nechcete-li. Nové funkce v hello SDK jsou podrobně popsané na [co je nového ve verzi 3](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Nejnovější změny ve verze 3
Existuje malý počet nejnovější změny ve verzi 3, které mohou vyžadovat kód změní kromě toorebuilding vaší aplikace.

### <a name="indexesgetclient-return-type"></a>Návratový typ Indexes.GetClient
Hello `Indexes.GetClient` má nové návratový typ. metoda. Dříve, vrátí `SearchIndexClient`, ale to byl změněn příliš`ISearchIndexClient` v verze 2.0-preview a že se změny přenesou tooversion 3. Toto je toosupport zákazníků, které chcete toomock hello `GetClient` metodu pro testování částí vrácením imitované implementace `ISearchIndexClient`.

#### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Můžete ji změnit toothis toofix žádné chyby sestavení:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-toostrings"></a>AnalyzerName, datový typ a další jsou už implicitně převést toostrings
Existuje mnoho typů v hello Azure Search .NET SDK, které pocházejí z `ExtensibleEnum`. Dříve byly tyto typy všechny implicitně převést tootype `string`. Ale byly zjištěny chyby v hello `Object.Equals` implementace pro tyto třídy a uchycení hello chyb vyžaduje zakázání této implicitní převod. Explicitní převod příliš`string` je stále povolený.

#### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

Můžete ji změnit toothis toofix žádné chyby sestavení:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Odebrat zastaralé členy

Může se zobrazit vlastnosti, které byly označeny jako zastaralé v verze 2.0-preview a následně odstraněny ve verzi 3 nebo toomethods související chyby sestavení. Pokud narazíte na takové chyby, zde je způsob tooresolve je:

- Pokud používáte tento konstruktor: `ScoringParameter(string name, string value)`, použijte místo toho tato:`ScoringParameter(string name, IEnumerable<string> values)`
- Pokud jste používali hello `ScoringParameter.Value` vlastnost, použijte hello `ScoringParameter.Values` vlastnost nebo hello `ToString` metoda místo.
- Pokud jste používali hello `SearchRequestOptions.RequestId` vlastnost, použijte hello `ClientRequestId` vlastnost místo.

### <a name="removed-preview-features"></a>Funkce odebrané preview

Pokud provádíte upgrade z verze 2.0 preview tooversion 3, mějte na paměti, JSON a CSV analýza podporu pro objekt Blob indexery byl odebrán vzhledem k tomu, že tyto funkce jsou stále ve verzi preview. Konkrétně hello následující metody hello `IndexingParametersExtensions` třídy byly odebrány:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Pokud vaše aplikace obsahuje pevné závislost na těchto funkcích, nebudete moct tooupgrade tooversion 3 z hello Azure Search .NET SDK. Můžete pokračovat v toouse verze 2.0-preview. Ale prosím mějte na paměti, **nedoporučujeme používání náhled sady SDK v produkční aplikace**. Funkce Preview jsou pouze zkušební verze a může změnit.

## <a name="conclusion"></a>Závěr
Pokud potřebujete další podrobnosti o použití hello .NET SDK služby Azure Search, najdete v našich nedávno aktualizovaného [postupy](search-howto-dotnet-sdk.md).

Uvítáme vaše názory na hello SDK. Pokud narazíte na potíže, myslíte, že volné tooask nám nápovědu k hello [fórum Azure Search MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Pokud narazíte na chyby, můžete soubor problém v hello [úložiště Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues). Ujistěte se, že tooprefix název vašeho problému s "Search SDK:".

Děkujeme za používání služby Azure Search.

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-tooupgrade-tooversion-11"></a>Dodatek: Kroky tooupgrade tooversion 1.1
> [!NOTE]
> Tato část se týká pouze toousers hello Azure Search .NET SDK verze 1.0.2-preview a starší.
> 
> 

Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí buď hello Konzola správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.

Jakmile NuGet stáhl hello nové balíčky a jejich závislosti, znovu sestavte projekt.

Pokud jste dříve pomocí sestavení hello verze 1.0.0-preview, 1.0.1-preview nebo 1.0.2-preview, uspěli a vy budete připravené toogo!

Pokud jste dříve používali verze 0.13.0-preview nebo starší, měli byste vidět chyby jako hello následující sestavení:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: hello type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

dalším krokem Hello je chyby sestavení hello toofix po jednom. Většina bude vyžadovat změna názvů některé třídy a metody, které byly přejmenovány v hello SDK. [Seznam nejnovější změny ve verzi 1.1](#ListOfChangesV1) obsahuje seznam tyto změny názvu.

Pokud používáte vlastní třídy toomodel dokumentů a tyto třídy mají vlastnosti použití hodnot Null primitivní typy (například `int` nebo `bool` v jazyce C#), je oprava chyb v hello 1.1 verzi hello SDK, které byste měli vědět. V tématu [opravy chyb ve verzi 1.1](#BugFixesV1) další podrobnosti.

Nakonec po vyřešili všechny chyby sestavení, můžete provádět změny tooyour aplikace tootake využívat nové funkce nechcete-li.

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a>Seznam nejnovější změny ve verze 1.1
Hello následujícím seznamu je seřazené podle hello pravděpodobnost, že změna hello ovlivní kódu aplikace.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch a IndexAction změny
`IndexBatch.Create`byl přejmenován příliš`IndexBatch.New` a již `params` argument. Můžete použít `IndexBatch.New` pro balíků, které kombinovat různé typy akcí (sloučení, odstranění atd.). Kromě toho existují nové statické metody pro vytvoření dávky kde jsou všechny akce hello hello stejné: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`.

`IndexAction`již má veřejné konstruktory a jeho vlastnosti jsou nyní neměnné. Používejte hello nové statické metody pro vytvoření akce pro jiné účely: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`. `IndexAction.Create`byla odebrána. Pokud jste použili hello přetížení, které přijímá pouze dokumentu, ujistěte se, že toouse `Upload` místo.

##### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Můžete ji změnit toothis toofix žádné chyby sestavení:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Pokud chcete, můžete další zjednodušit ho toothis:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException změny
Hello `IndexBatchException.IndexResponse` vlastnost byla přejmenována příliš`IndexingResults`, a je její typ `IList<IndexingResult>`.

##### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Můžete ji změnit toothis toofix žádné chyby sestavení:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a>Operace změny – metoda
Jednotlivých operací ve hello Azure Search .NET SDK je zpřístupněná jako sadu přetížení metody pro synchronní a asynchronní volající. Hello podpisů a řešení z těchto přetížení metody se změnilo v verze 1.1.

Například hello "Získat Index statistika" operace ve starších verzích hello SDK zveřejněné tyto podpisy:

V `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

V `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

podpisy Hello metoda pro hello stejné operace v vypadají verze 1.1:

V `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

V `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Od verze 1.1, hello .NET SDK služby Azure Search slouží k uspořádání operaci metody jinak:

* Volitelné parametry jsou nyní modelován jako výchozí parametry spíš než přetížení další metody. To někdy výrazně snižuje počet hello přetížení metody.
* metody rozšíření Hello teď skrýt spoustu hello nadbytečné podrobnosti HTTP z volající hello. Například starší verze sady SDK vrátil objekt odpověď se stavovým kódem HTTP, které můžete často hello nemám potřebu toocheck, protože operaci metody throw `CloudException` pro stavový kód, který označuje chybu. Dobrý den nové objekty vrácena model metody rozšíření, ukládání můžete hello potíže při použití toounwrap je ve vašem kódu.
* Naopak hello základní rozhraní nyní vystavit metody, které získáte větší kontrolu na úrovni hello HTTP pokud ho potřebujete. Nyní můžete předat ve vlastní toobe hlavičky protokolu HTTP součástí požadavků a hello nové `AzureOperationResponse<T>` vrátí typ poskytuje přímý přístup toohello `HttpRequestMessage` a `HttpResponseMessage` pro operaci hello. `AzureOperationResponse`je definována v hello `Microsoft.Rest.Azure` obor názvů a nahradí `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters změny
Novou třídu s názvem `ScoringParameter` byl v hello nejnovější SDK toomake jeho přidání jednodušší tooprovide parametry tooscoring profily ve vyhledávací dotaz. Dříve hello `ScoringProfiles` vlastnost hello `SearchParameters` třída byl zadán jako `IList<string>`; Teď je zadán jako `IList<ScoringParameter>`.

##### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Můžete ji změnit toothis toofix žádné chyby sestavení: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Změny modelu – třída
Z důvodu změn podpis toohello popsaných v [operaci metoda změny](#OperationMethodChanges), mnoho tříd v hello `Microsoft.Azure.Search.Models` bylo přejmenováno nebo odebrat obor názvů. Například:

* `IndexDefinitionResponse`nahradila`AzureOperationResponse<Index>`
* `DocumentSearchResponse`byl přejmenován příliš`DocumentSearchResult`
* `IndexResult`byl přejmenován příliš`IndexingResult`
* `Documents.Count()`nyní vrátí `long` s počtem dokumentů hello místo`DocumentCountResponse`
* `IndexGetStatisticsResponse`byl přejmenován příliš`IndexGetStatisticsResult`
* `IndexListResponse`byl přejmenován příliš`IndexListResult`

toosummarize, `OperationResponse`-odvozených tříd, které existovaly pouze toowrap objekt modelu se odebraly. Hello zbývající třídy předtím jejich přípona se změnila z `Response` příliš`Result`.

##### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Můžete ji změnit toothis toofix žádné chyby sestavení:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Odpověď třídy a rozhraní IEnumerable
Další změny, které mohou ovlivnit váš kód je už implementaci třídy odpovědi, které uložení kolekce `IEnumerable<T>`. Vlastnost kolekce hello místo toho můžete přistupovat přímo. Například, pokud kód vypadá takto:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Můžete ji změnit toothis toofix žádné chyby sestavení:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a>Zvláštním případem pro webové aplikace
Pokud máte webové aplikace, který serializuje `DocumentSearchResponse` přímo výsledky hledání toosend toohello prohlížeče, budete potřebovat toochange kódu nebo hello výsledky nebude správně serializovat. Například, pokud kód vypadá takto:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Můžete ji změnit získáním hello `.Results` vlastnost vykreslování výsledků vyhledávání odpovědi toofix na hello vyhledávání:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Budete mít toolook takových případech ve vašem kódu sami. **hello kompilátoru nebude varovat** protože `JsonResult.Data` je typu `object`.

#### <a name="cloudexception-changes"></a>CloudException změny
Hello `CloudException` třída přesunula z hello `Hyak.Common` toohello obor názvů `Microsoft.Rest.Azure` oboru názvů. Také jeho `Error` vlastnost byla přejmenována příliš`Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Změny SearchServiceClient a SearchIndexClient
Hello typ hello `Credentials` vlastnost bylo změněno z `SearchCredentials` tooits základní třídy, `ServiceClientCredentials`. Pokud potřebujete tooaccess hello `SearchCredentials` z `SearchIndexClient` nebo `SearchServiceClient`, použijte nové hello `SearchCredentials` vlastnost.

Ve starších verzích hello SDK `SearchServiceClient` a `SearchIndexClient` měl konstruktory, které trvalo `HttpClient` parametr. Tyto nahradil konstruktory, které provést `HttpClientHandler` a pole `DelegatingHandler` objekty. Díky tomu je snazší tooinstall vlastní obslužné rutiny toopre zpracování požadavků HTTP v případě potřeby.

Nakonec hello konstruktory, které trvalo `Uri` a `SearchCredentials` změnily. Například pokud máte kód, který vypadá takto:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Můžete ji změnit toothis toofix žádné chyby sestavení:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Všimněte si, že typ hello hello pověření parametr se změní také příliš`ServiceClientCredentials`. Toto je pravděpodobně tooaffect kódu od `SearchCredentials` je odvozený od `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Předání ID žádosti
Ve starších verzích hello SDK, můžete nastavit ID požadavku na hello `SearchServiceClient` nebo `SearchIndexClient` a by být součástí každé toohello požadavku REST API. To je užitečné pro odstraňování potíží s služby search, pokud potřebujete podporu toocontact. Je však tooset užitečnější požadavku Jedinečný ID pro každou operaci místo toouse hello stejným ID pro všechny operace. Z tohoto důvodu hello `SetClientRequestId` metody `SearchServiceClient` a `SearchIndexClient` byly odebrány. Místo toho můžete předat metodu požadavku tooeach ID operace prostřednictvím hello volitelné `SearchRequestOptions` parametr.

> [!NOTE]
> V budoucí verzi hello SDK přidáme nové mechanismus pro ID požadavku globální nastavení na klientovi hello objektů, který je konzistentní s přístupem hello používá jiné sady Azure SDK.
> 
> 

#### <a name="example"></a>Příklad
Pokud máte kód, který vypadá takto:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Můžete ji změnit toothis toofix žádné chyby sestavení:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Změny názvu rozhraní
Hello operaci skupiny rozhraní názvy mají všechny změněné toobe konzistentní s jejich odpovídající názvy vlastností:

* Hello typ `ISearchServiceClient.Indexes` byla přejmenována z `IIndexOperations` příliš`IIndexesOperations`.
* Hello typ `ISearchServiceClient.Indexers` byla přejmenována z `IIndexerOperations` příliš`IIndexersOperations`.
* Hello typ `ISearchServiceClient.DataSources` byla přejmenována z `IDataSourceOperations` příliš`IDataSourcesOperations`.
* Hello typ `ISearchIndexClient.Documents` byla přejmenována z `IDocumentOperations` příliš`IDocumentsOperations`.

Tuto změnu je pravděpodobně tooaffect kódu, pokud jste vytvořili mocks tato rozhraní pro účely testování.

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a>Opravy chyb v verze 1.1
Starší verze hello Azure Search .NET SDK týkající se tooserialization třídy vlastní model obsahoval chyby. Hello chyb mohlo dojít, pokud jste vytvořili třídu vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null.

#### <a name="steps-tooreproduce"></a>Kroky tooreproduce
Vytvořte třídu, vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null. Například přidejte veřejné `UnitCount` vlastnost typu `int` místo `int?`.

Pokud indexu dokument s hello výchozí hodnota tohoto typu (například 0 pro `int`), hello pole bude mít hodnotu null ve službě Azure Search. Pokud následně vyhledávání pro tento dokument hello `Search` volání vyvolá výjimku `JsonSerializationException` nesouhlasících, kterou nelze převést `null` příliš`int`.

Navíc filtry nemusí fungovat podle očekávání vzhledem k tomu, že null byla zapsána toohello index namísto hello určené hodnoty.

#### <a name="fix-details"></a>Opravte podrobnosti
Vyřešili jsme tento problém v verze 1.1 hello SDK. Nyní, pokud máte třídu modelu takto:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

a nastavíte `IntValue` too0, že hodnota je nyní správně serializovanou jako 0 na hello přenosu a uložené jako 0 v indexu hello. Zaokrouhlí vypínání také funguje podle očekávání.

Je vědoma s tímto přístupem jeden potenciální problém toobe: Pokud chcete použít typ modelu s hodnotou Null vlastností, můžete mít příliš**zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hello hodnotu null. Hello SDK ani hello REST API služby Azure Search vám pomůže tooenforce vám to.

Nejedná se pouze o hypotetický problém: Představte si situaci, kdy přidáte nový pole tooan existující index typu `Edm.Int32`. Po aktualizaci definice indexu hello, budou všechny dokumenty mít pro toto nové pole hodnotu null (protože jsou všechny typy s možnou hodnotou Null ve službě Azure Search). Pokud pak použijete třídu modelu s hodnotou Null `int` vlastnosti pro toto pole, zobrazí se `JsonSerializationException` při pokusu o tooretrieve dokumenty podobné výjimky:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Z tohoto důvodu doporučujeme jako osvědčený postup použijte typy s možnou hodnotou Null ve třídách modelu.

Další informace o tato oprava chyb a hello, najdete v tématu [potíže na Githubu](https://github.com/Azure/azure-sdk-for-net/issues/1063).

