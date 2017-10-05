---
title: "Upgrade .NET SDK služby Azure Search verze 1.1 | Microsoft Docs"
description: "Upgrade .NET SDK služby Azure Search verze 1.1"
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
ms.openlocfilehash: 9782454e3bfc697b63cde8aa28a14be0c393c36b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Upgrade na Azure Search .NET SDK verze 3
Pokud používáte verzi 2.0 preview nebo starší z [Azure Search .NET SDK](https://aka.ms/search-sdk), tento článek vám pomůže při upgradu aplikace k používání verze 3.

Další obecné návod sady SDK, včetně příkladů, najdete v tématu [jak používat Azure Search z aplikace .NET](search-howto-dotnet-sdk.md).

Verze 3 .NET SDK služby Azure Search obsahuje některé změny z předchozích verzí. Tyto jsou většinou dílčí, takže měnili kód měli vyžadovat jen minimální úsilí. V tématu [kroky pro upgrade](#UpgradeSteps) pokyny o tom, jak upravit svůj kód k použití nové verze sady SDK.

> [!NOTE]
> Pokud používáte verzi 1.0.2-preview nebo starší, byste měli upgradovat na verzi 1.1 první a pak upgradovat na verze 3. V tématu [příloha: kroky pro upgrade na verzi 1.1](#UpgradeStepsV1) pokyny.
>
> Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně nejnovější. Můžete použít verzi, když je už nejnovějšího, ale doporučujeme migraci kód a použít nejnovější verzi. Při použití rozhraní REST API, musíte zadat verze rozhraní API v každé žádosti o prostřednictvím parametr api-version. Když pomocí sady .NET SDK, určuje verzi sady SDK používáte odpovídající verzi rozhraní REST API. Pokud používáte starší SDK, můžete spustit tento kód se žádné změny, i v případě, že je služba upgradovat pro podporu na novější verzi rozhraní API.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Co je nového v verze 3
Verze 3 .NET SDK služby Azure Search cílí na nejnovější verzi obecně k dispozici REST API Azure Search, konkrétně 2016-09-01. Díky tomu je možné použít mnoha nových funkcí služby Azure Search z aplikace .NET, včetně následujících:

* [Vlastní analyzátory](https://aka.ms/customanalyzers)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) a [Azure Table Storage](search-howto-indexing-azure-tables.md) podpora indexeru
* Přizpůsobení indexeru prostřednictvím [pole mapování](search-indexer-field-mappings.md)
* Značky etag binárním rozsáhlým podporu pro povolení bezpečné souběžných aktualizací definic index, indexery a zdroje dat
* Podporu pro vytváření indexu pole definice deklarativně architekturu třídě modelu a použitím nové `FieldBuilder` třídy.
* Podpora pro .NET Core a přenosné profil .NET 111

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Kroky pro upgrade
Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí konzoly Správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.

Jakmile NuGet stáhl nové balíčky a jejich závislosti, znovu sestavte projekt. V závislosti na tom, jak je strukturovaná kódu se může znovu sestavit úspěšně. Pokud ano, jste připravení přejít!

Pokud vaše sestavení selže, měli byste vidět chyby sestavení takto:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Dalším krokem je odstranění této chyby sestavení. V tématu [nejnovější změny ve verzi 3](#ListOfChanges) podrobné informace o co způsobí, že chyba a jeho řešení.

Může se zobrazit další sestavení upozornění související s zastaralé metody nebo vlastnosti. Upozornění bude obsahovat pokyny o tom, jak používat místo zastaralé funkce. Například, pokud vaše aplikace používá `IndexingParameters.Base64EncodeKeys` vlastnost, měli byste obdržet upozornění, která uvádí, že`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Jakmile vyřešili všechny chyby sestavení, vám do vaší aplikace využívat výhod nových funkcí, nechcete-li provádět změny. Nové funkce v sadě SDK jsou podrobně popsané na [co je nového ve verzi 3](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Nejnovější změny ve verze 3
Existuje malý počet nejnovější změny ve verzi 3, která může vyžadovat kód změní kromě nové sestavení aplikace.

### <a name="indexesgetclient-return-type"></a>Návratový typ Indexes.GetClient
`Indexes.GetClient` Má nové návratový typ. metoda. Dříve, vrátí `SearchIndexClient`, ale to byl změněn na `ISearchIndexClient` v verze 2.0-preview a že se změny přenesou do verze 3. To je podpora zákazníků, které chcete model `GetClient` metodu pro testování částí vrácením imitované implementace `ISearchIndexClient`.

#### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>AnalyzerName, datový typ a další jsou už implicitně převést na řetězce
Existuje mnoho typů v Azure Search .NET SDK, které pocházejí z `ExtensibleEnum`. Dříve byly všechny tyto typy implicitně převést na typ `string`. Ale byly zjištěny chyby ve `Object.Equals` implementace pro tyto třídy a oprava chyb vyžaduje zakázání této implicitní převod. Explicitní převod na `string` je stále povolený.

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

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

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

Může se zobrazit chyby sestavení související s metody nebo vlastnosti, které byly označeny jako zastaralé ve verzi 2.0 preview a následně odebrat verze 3. Pokud narazíte na takové chyby, zde je způsob jejich řešení:

- Pokud používáte tento konstruktor: `ScoringParameter(string name, string value)`, použijte místo toho tato:`ScoringParameter(string name, IEnumerable<string> values)`
- Pokud jste používali `ScoringParameter.Value` vlastnosti, použijte `ScoringParameter.Values` vlastnost nebo `ToString` metoda místo.
- Pokud jste používali `SearchRequestOptions.RequestId` vlastnosti, použijte `ClientRequestId` vlastnost místo.

### <a name="removed-preview-features"></a>Funkce odebrané preview

Pokud upgradujete z verze 2.0-preview verze 3, mějte na paměti, JSON a CSV analýza podporu pro objekt Blob indexery byl odebrán vzhledem k tomu, že tyto funkce jsou stále ve verzi preview. Konkrétně tyto metody `IndexingParametersExtensions` třídy byly odebrány:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Pokud vaše aplikace obsahuje pevné závislost na těchto funkcích, nebudete moci upgradovat na verzi 3 .NET SDK služby Azure Search. Můžete nadále používat verze 2.0-preview. Ale prosím mějte na paměti, **nedoporučujeme používání náhled sady SDK v produkční aplikace**. Funkce Preview jsou pouze zkušební verze a může změnit.

## <a name="conclusion"></a>Závěr
Pokud potřebujete další podrobnosti o použití .NET SDK služby Azure Search, najdete v našich nedávno aktualizovaného [postupy](search-howto-dotnet-sdk.md).

Uvítáme vaše názory týkající se sady SDK. Pokud narazíte na potíže, neváhejte nám požádat o pomoc na [fórum Azure Search MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Pokud narazíte na chyby, můžete soubor problém v [úložiště Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues). Ujistěte se, zda jste předpony název vašeho problému s "Search SDK:".

Děkujeme za používání služby Azure Search.

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-to-upgrade-to-version-11"></a>Dodatek: Kroky pro upgrade na verzi 1.1
> [!NOTE]
> Tato část se týká pouze uživatelé 1.0.2-preview verze .NET SDK služby Azure Search a starší.
> 
> 

Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí konzoly Správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.

Jakmile NuGet stáhl nové balíčky a jejich závislosti, znovu sestavte projekt.

Pokud jste používali dříve verze 1.0.0-preview, 1.0.1-preview nebo 1.0.2-preview, sestavení uspěli a jste připravení přejít!

Pokud jste dříve používali verze 0.13.0-preview nebo starší, měli byste vidět sestavení chyby takto:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Dalším krokem je opravte chyby sestavení po jednom. Většina bude vyžadovat změny některé názvy třídy a metody, které byly přejmenovány v sadě SDK. [Seznam nejnovější změny ve verzi 1.1](#ListOfChangesV1) obsahuje seznam tyto změny názvu.

Pokud používáte vlastní třídy pro modelování dokumentů a tyto třídy mají vlastnosti použití hodnot Null primitivní typy (například `int` nebo `bool` v jazyce C#), je oprava chyb v 1.1 verzi sady SDK, které byste měli vědět. V tématu [opravy chyb ve verzi 1.1](#BugFixesV1) další podrobnosti.

Nakonec po vyřešili všechny chyby sestavení, vám provádět změny do vaší aplikace využívat výhod nových funkcí, nechcete-li.

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a>Seznam nejnovější změny ve verze 1.1
V následujícím seznamu je seřazené podle pravděpodobnost, že změna ovlivní kódu aplikace.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch a IndexAction změny
`IndexBatch.Create`byl přejmenován na `IndexBatch.New` a již `params` argument. Můžete použít `IndexBatch.New` pro balíků, které kombinovat různé typy akcí (sloučení, odstranění atd.). Kromě toho existují nové statické metody pro vytvoření dávky kde všechny akce jsou stejné: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`.

`IndexAction`již má veřejné konstruktory a jeho vlastnosti jsou nyní neměnné. Byste měli použít nové statické metody pro vytvoření akce pro jiné účely: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`. `IndexAction.Create`byla odebrána. Pokud jste použili přetížení, které přijímá pouze dokumentu, nezapomeňte použít `Upload` místo.

##### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Pokud chcete, můžete ho zjednodušit další k tomuto:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException změny
`IndexBatchException.IndexResponse` Vlastnost byla přejmenována na `IndexingResults`, a je její typ `IList<IndexingResult>`.

##### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a>Operace změny – metoda
Každé operace v rozhraní .NET SDK služby Azure Search je zpřístupněná jako sadu přetížení metody pro synchronní a asynchronní volající. Podpisů a řešení z těchto přetížení metody se změnilo v verze 1.1.

Například "Získat Index statistika" operaci ve starší verze sady SDK zveřejněné tyto podpisy:

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

Metoda podpisy pro stejnou operaci v verze 1.1 bude vypadat takto:

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

Od verze 1.1, .NET SDK služby Azure Search slouží k uspořádání operaci metody jinak:

* Volitelné parametry jsou nyní modelován jako výchozí parametry spíš než přetížení další metody. To někdy výrazně snižuje počet přetížení metody.
* Rozšiřující metody teď skrýt spoustu nadbytečné podrobnosti HTTP volající. Například starší verze sady SDK vrátil objekt odpověď se stavovým kódem HTTP, které často nebyla musíte zkontrolovat, protože operace metody throw `CloudException` pro stavový kód, který označuje chybu. Nové metody rozšíření právě vrátí objekty modelu, ukládání, můžete problém toho, že je rozbalení v kódu.
* Naopak základní rozhraní nyní zveřejněte metody, které získáte větší kontrolu na úrovni HTTP pokud ho potřebujete. Nyní můžete předat do vlastní hlavičky protokolu HTTP, které mají být zahrnuty do požadavků a nové `AzureOperationResponse<T>` návratový typ poskytuje přímý přístup k `HttpRequestMessage` a `HttpResponseMessage` pro operaci. `AzureOperationResponse`je definována v `Microsoft.Rest.Azure` obor názvů a nahradí `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters změny
Novou třídu s názvem `ScoringParameter` byla přidána do na nejnovější SDK, aby bylo snazší poskytuje parametry pro vyhodnocování profily ve vyhledávací dotaz. Dříve `ScoringProfiles` vlastnost `SearchParameters` třída byl zadán jako `IList<string>`; Teď je zadán jako `IList<ScoringParameter>`.

##### <a name="example"></a>Příklad
Pokud váš kód vypadá takto:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Můžete ji změnit k tomuto a opravte případné chyby sestavení: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Změny modelu – třída
Z důvodu změn podpis popsaných v [operaci metoda změny](#OperationMethodChanges), mnoho tříd v `Microsoft.Azure.Search.Models` bylo přejmenováno nebo odebrat obor názvů. Například:

* `IndexDefinitionResponse`nahradila`AzureOperationResponse<Index>`
* Přejmenování `DocumentSearchResponse` na `DocumentSearchResult`
* Přejmenování `IndexResult` na `IndexingResult`
* `Documents.Count()`nyní vrátí `long` s počtem dokumentů místo`DocumentCountResponse`
* Přejmenování `IndexGetStatisticsResponse` na `IndexGetStatisticsResult`
* Přejmenování `IndexListResponse` na `IndexListResult`

To Shrneme, `OperationResponse`-odvozených tříd, které existovaly pouze zabalit byly odebrány objekt modelu. Zbývající třídy předtím jejich přípona se změnila z `Response` k `Result`.

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

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

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
Další změny, které mohou ovlivnit váš kód je už implementaci třídy odpovědi, které uložení kolekce `IEnumerable<T>`. Vlastnost kolekce místo toho můžete přistupovat přímo. Například, pokud kód vypadá takto:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a>Zvláštním případem pro webové aplikace
Pokud máte webové aplikace, který serializuje `DocumentSearchResponse` přímo do prohlížeče odeslat výsledky hledání, budete muset upravit svůj kód nebo výsledky nebude správně serializovat. Například, pokud kód vypadá takto:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Můžete ji změnit získáním `.Results` vlastnost vyhledávání odpovědi opravit vykreslování výsledků vyhledávání:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Bude mít a hledat v takových případech ve vašem kódu sami. **Kompilátor nebude varovat** protože `JsonResult.Data` je typu `object`.

#### <a name="cloudexception-changes"></a>CloudException změny
`CloudException` Třída přesunula z `Hyak.Common` názvů `Microsoft.Rest.Azure` oboru názvů. Také jeho `Error` vlastnost byla přejmenována na `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Změny SearchServiceClient a SearchIndexClient
Typ `Credentials` vlastnost bylo změněno z `SearchCredentials` k její základní třída `ServiceClientCredentials`. Pokud budete potřebovat pro přístup `SearchCredentials` z `SearchIndexClient` nebo `SearchServiceClient`, použijte prosím nový `SearchCredentials` vlastnost.

Starší verze sady SDK `SearchServiceClient` a `SearchIndexClient` měl konstruktory, které trvalo `HttpClient` parametr. Tyto nahradil konstruktory, které provést `HttpClientHandler` a pole `DelegatingHandler` objekty. To usnadňuje instalaci vlastní obslužné rutiny předběžné zpracování požadavků HTTP, v případě potřeby.

Nakonec konstruktory, které trvalo `Uri` a `SearchCredentials` změnily. Například pokud máte kód, který vypadá takto:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Také Upozorňujeme, že typ parametru přihlašovací údaje se změnila na `ServiceClientCredentials`. To se pravděpodobně ovlivnit kódu od `SearchCredentials` je odvozený od `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Předání ID žádosti
Starší verze sady SDK, můžete nastavit ID požadavku na `SearchServiceClient` nebo `SearchIndexClient` a by být součástí každého požadavku rozhraní REST API. To je užitečné pro odstraňování potíží s služby search, pokud potřebujete obraťte se na podporu. Je však užitečnější nastavit požadavek jedinečné ID pro všechny operace, a nikoli používat stejné ID pro všechny operace. Z tohoto důvodu `SetClientRequestId` metody `SearchServiceClient` a `SearchIndexClient` byly odebrány. Místo toho můžete předat ID žádosti do jednotlivých metod operace prostřednictvím nepovinný `SearchRequestOptions` parametr.

> [!NOTE]
> V budoucí verzi sady SDK přidáme nové mechanismus pro ID požadavku globální nastavení na straně klienta objektů, který je konzistentní s přístupem používá jiné sady Azure SDK.
> 
> 

#### <a name="example"></a>Příklad
Pokud máte kód, který vypadá takto:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Můžete ji změnit k tomuto a opravte případné chyby sestavení:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Změny názvu rozhraní
Názvy rozhraní skupin operaci byla změněna být konzistentní s jejich názvy vlastností, odpovídající:

* Typ `ISearchServiceClient.Indexes` byla přejmenována z `IIndexOperations` k `IIndexesOperations`.
* Typ `ISearchServiceClient.Indexers` byla přejmenována z `IIndexerOperations` k `IIndexersOperations`.
* Typ `ISearchServiceClient.DataSources` byla přejmenována z `IDataSourceOperations` k `IDataSourcesOperations`.
* Typ `ISearchIndexClient.Documents` byla přejmenována z `IDocumentOperations` k `IDocumentsOperations`.

Tuto změnu je nepravděpodobné, že pokud jste vytvořili mocks tato rozhraní pro testovací účely ovlivní kódu.

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a>Opravy chyb v verze 1.1
Ve starších verzích týkající se serializace třídy vlastní modelu Azure Search .NET SDK se chyby. Chybě mohlo dojít, pokud jste vytvořili třídu vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null.

#### <a name="steps-to-reproduce"></a>Kroky pro reprodukci
Vytvořte třídu, vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null. Například přidejte veřejné `UnitCount` vlastnost typu `int` místo `int?`.

Pokud indexu dokument s výchozí hodnotou tohoto typu (například 0 pro `int`), bude toto pole hodnotu null ve službě Azure Search. Pokud následně vyhledávání pro tento dokument `Search` volání vyvolá výjimku `JsonSerializationException` nesouhlasících, kterou nelze převést `null` k `int`.

Navíc filtry nemusí fungovat podle očekávání, protože hodnota null byla zapsána do indexu místo určená hodnota.

#### <a name="fix-details"></a>Opravte podrobnosti
Vyřešili jsme tento problém v verze 1.1 sady SDK. Nyní, pokud máte třídu modelu takto:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

a nastavíte `IntValue` na hodnotu 0, tuto hodnotu je nyní správně serializovanou jako 0 v drátové síti a uloží jako 0 v indexu. Zaokrouhlí vypínání také funguje podle očekávání.

Neexistuje jeden potenciální problém s tímto přístupem znát: Pokud použijete typ modelu s hodnotou Null vlastností, budete muset **zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hodnotu null. Sada SDK, ani REST API služby Azure Search vám pomůže vynutit.

Nejedná se pouze o hypotetický problém: představte si situaci, kdy přidáte nové pole do stávajícího indexu typu `Edm.Int32`. Po aktualizaci definice indexu budou mít všechny dokumenty pro toto nové pole hodnotu null (protože všechny typy jsou ve službě Azure Search s možností null). Pokud pak použijete třídu modelu s vlastností `int` se zakázanou hodnotou null, při pokusu o načtení dokumentů dojde k vyvolání podobné výjimky `JsonSerializationException`:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Z tohoto důvodu doporučujeme jako osvědčený postup použijte typy s možnou hodnotou Null ve třídách modelu.

Další podrobnosti o této chyby a opravu, najdete v tématu [potíže na Githubu](https://github.com/Azure/azure-sdk-for-net/issues/1063).

