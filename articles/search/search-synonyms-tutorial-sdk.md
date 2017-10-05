---
title: "Kurz k synonymům ve verzi Preview ve službě Azure Search | Dokumentace Microsoftu"
description: "Přidání funkce synonym ve verzi Preview do indexu ve službě Azure Search."
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
ms.openlocfilehash: 014959ed471f796d2184f0f8ff10d15cdc8a2ec6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="synonym-preview-c-tutorial-for-azure-search"></a><span data-ttu-id="f89c0-103">Kurz synonym (Preview) v jazyce C# pro Azure Search</span><span class="sxs-lookup"><span data-stu-id="f89c0-103">Synonym (preview) C# tutorial for Azure Search</span></span>

<span data-ttu-id="f89c0-104">Synonyma rozšiřují dotazy hledáním shody s termíny, které jsou považované za sémantické ekvivalenty vstupního výrazu.</span><span class="sxs-lookup"><span data-stu-id="f89c0-104">Synonyms expand a query by matching on terms considered semantically equivalent to the input term.</span></span> <span data-ttu-id="f89c0-105">Chcete třeba, aby položce auto odpovídaly i dokumenty obsahující termíny automobil a vozidlo.</span><span class="sxs-lookup"><span data-stu-id="f89c0-105">For example, you might want "car" to match documents containing the terms "automobile" or "vehicle".</span></span>

<span data-ttu-id="f89c0-106">Ve službě Azure Search se synonyma definují v *mapě synonym*, pomocí *pravidel mapování*, která přidružují ekvivalentní termíny.</span><span class="sxs-lookup"><span data-stu-id="f89c0-106">In Azure Search, synonyms are defined in a *synonym map*, through *mapping rules* that associate equivalent terms.</span></span> <span data-ttu-id="f89c0-107">Můžete vytvořit několik map synonym, zveřejnit je jako prostředky na úrovni služby dostupné pro všechny indexy a potom určit, který se má použít na úrovni pole.</span><span class="sxs-lookup"><span data-stu-id="f89c0-107">You can create multiple synonym maps, post them as a service-wide resource available to any index, and then reference which one to use at the field level.</span></span> <span data-ttu-id="f89c0-108">V době zpracování dotazu služba Azure Search nejenom prohledá index, ale pokud některé z polí využitých v tomto dotazu má mapu synonym, prohledá i tuto mapu.</span><span class="sxs-lookup"><span data-stu-id="f89c0-108">At query time, in addition to searching an index, Azure Search does a lookup in a synonym map, if one is specified on fields used in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="f89c0-109">Funkce synonym je aktuálně ve verzi Preview a je podporovaná jenom v nejnovějších verzích Preview rozhraní API a sady SDK(api-version=2016-09-01-Preview, SDK verze 4.x-preview).</span><span class="sxs-lookup"><span data-stu-id="f89c0-109">The synonyms feature is currently in preview and only supported in the latest preview API and SDK versions (api-version=2016-09-01-Preview, SDK version 4.x-preview).</span></span> <span data-ttu-id="f89c0-110">Podpora webu Azure Portal se v současnosti neposkytuje.</span><span class="sxs-lookup"><span data-stu-id="f89c0-110">There is no Azure portal support at this time.</span></span> <span data-ttu-id="f89c0-111">Na rozhraní API ve verzi Preview se nevztahuje žádná smlouva SLA a funkce se také mohou změnit, proto je nedoporučujeme používat v produkčních aplikacích.</span><span class="sxs-lookup"><span data-stu-id="f89c0-111">Preview APIs are not under SLA and preview features may change, so we do not recommend using them in production applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f89c0-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f89c0-112">Prerequisites</span></span>

<span data-ttu-id="f89c0-113">Požadavky kurzu zahrnují tyto položky:</span><span class="sxs-lookup"><span data-stu-id="f89c0-113">Tutorial requirements include the following:</span></span>

* [<span data-ttu-id="f89c0-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f89c0-114">Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="f89c0-115">Služba Azure Search</span><span class="sxs-lookup"><span data-stu-id="f89c0-115">Azure Search service</span></span>](search-create-service-portal.md)
* [<span data-ttu-id="f89c0-116">Verze Preview knihovny Microsoft.Azure.Search .NET</span><span class="sxs-lookup"><span data-stu-id="f89c0-116">Preview version of Microsoft.Azure.Search .NET library</span></span>](https://aka.ms/search-sdk-preview)
* [<span data-ttu-id="f89c0-117">Jak používat Azure Search z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="f89c0-117">How to use Azure Search from a .NET Application</span></span>](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a><span data-ttu-id="f89c0-118">Přehled</span><span class="sxs-lookup"><span data-stu-id="f89c0-118">Overview</span></span>

<span data-ttu-id="f89c0-119">Dotazy před a po ukazují význam použití synonym.</span><span class="sxs-lookup"><span data-stu-id="f89c0-119">Before-and-after queries demonstrate the value of synonyms.</span></span> <span data-ttu-id="f89c0-120">V tomto kurzu používáme ukázkovou aplikaci, která spustí dotazy a vrátí výsledky v ukázkovém indexu.</span><span class="sxs-lookup"><span data-stu-id="f89c0-120">In this tutorial, we use a sample application that executes queries and returns results on a sample index.</span></span> <span data-ttu-id="f89c0-121">Ukázková aplikace vytvoří malý index s názvem hotels naplněný dvěma dokumenty.</span><span class="sxs-lookup"><span data-stu-id="f89c0-121">The sample application creates a small index named "hotels" populated with two documents.</span></span> <span data-ttu-id="f89c0-122">Aplikace spustí vyhledávací dotazy s využitím termínů a frází, které nejsou v indexu uvedené, povolí funkci synonym a potom znovu provede stejné hledání.</span><span class="sxs-lookup"><span data-stu-id="f89c0-122">The application executes search queries using terms and phrases that do not appear in the index, enables the synonyms feature, then issues the same searches again.</span></span> <span data-ttu-id="f89c0-123">Uvedený kód ukazuje celkový tok.</span><span class="sxs-lookup"><span data-stu-id="f89c0-123">The code below demonstrates the overall flow.</span></span>

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
      Thread.Sleep(10000); // Wait for the changes to propagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");

      Console.ReadKey();
  }
```
<span data-ttu-id="f89c0-124">Postup vytvoření a naplnění ukázkového indexu jsou vysvětlené v tématu [Jak používat Azure Search z aplikace .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span><span class="sxs-lookup"><span data-stu-id="f89c0-124">The steps to create and populate the sample index are explained in [How to use Azure Search from a .NET Application](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span></span>

## <a name="before-queries"></a><span data-ttu-id="f89c0-125">Dotazy „před“</span><span class="sxs-lookup"><span data-stu-id="f89c0-125">"Before" queries</span></span>

<span data-ttu-id="f89c0-126">V `RunQueriesWithNonExistentTermsInIndex` jsme vydali vyhledávací dotazy pro „five star“, „internet“ a „economy AND hotel“.</span><span class="sxs-lookup"><span data-stu-id="f89c0-126">In `RunQueriesWithNonExistentTermsInIndex`, we issue search queries with "five star", "internet", and "economy AND hotel".</span></span>
```csharp
Console.WriteLine("Search the entire index for the phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
<span data-ttu-id="f89c0-127">Ani jeden z indexovaných dokumentů tyto termíny neobsahuje, proto z prvního volání `RunQueriesWithNonExistentTermsInIndex` dostáváme tento výstup.</span><span class="sxs-lookup"><span data-stu-id="f89c0-127">Neither of the two indexed documents contain the terms, so we get the following output from the first `RunQueriesWithNonExistentTermsInIndex`.</span></span>
~~~
Search the entire index for the phrase "five star":

no document matched

Search the entire index for the term 'internet':

no document matched

Search the entire index for the terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a><span data-ttu-id="f89c0-128">Povolení synonym</span><span class="sxs-lookup"><span data-stu-id="f89c0-128">Enable synonyms</span></span>

<span data-ttu-id="f89c0-129">Povolení synonyma je dvoustupňový proces.</span><span class="sxs-lookup"><span data-stu-id="f89c0-129">Enabling synonyms is a two-step process.</span></span> <span data-ttu-id="f89c0-130">Nejdřív nadefinujeme a nahrajeme pravidla synonym a potom nakonfigurujeme pole pro jejich použití.</span><span class="sxs-lookup"><span data-stu-id="f89c0-130">We first define and upload synonym rules and then configure fields to use them.</span></span> <span data-ttu-id="f89c0-131">Proces je popsaný v `UploadSynonyms` a `EnableSynonymsInHotelsIndex`.</span><span class="sxs-lookup"><span data-stu-id="f89c0-131">The process is outlined in `UploadSynonyms` and `EnableSynonymsInHotelsIndex`.</span></span>

1. <span data-ttu-id="f89c0-132">Přidejte mapu synonym k vaší vyhledávací službě.</span><span class="sxs-lookup"><span data-stu-id="f89c0-132">Add a synonym map to your search service.</span></span> <span data-ttu-id="f89c0-133">V `UploadSynonyms` definujeme čtyři pravidla v mapě synonym desc-synonymmap a nahrajeme je do služby.</span><span class="sxs-lookup"><span data-stu-id="f89c0-133">In `UploadSynonyms`, we define four rules in our synonym map 'desc-synonymmap' and upload to the service.</span></span>
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
<span data-ttu-id="f89c0-134">Mapa synonym musí odpovídat opensourcovému standardnímu formátu `solr`.</span><span class="sxs-lookup"><span data-stu-id="f89c0-134">A synonym map must conform to the open source standard `solr` format.</span></span> <span data-ttu-id="f89c0-135">Tento formát je vysvětlený v tématu [Synonyma ve službě Azure Search](search-synonyms.md) v části `Apache Solr synonym format`.</span><span class="sxs-lookup"><span data-stu-id="f89c0-135">The format is explained in [Synonyms in Azure Search](search-synonyms.md) under the section `Apache Solr synonym format`.</span></span>

2. <span data-ttu-id="f89c0-136">Nakonfigurujte prohledávatelná pole tak, aby používala mapu synonym v definici indexu.</span><span class="sxs-lookup"><span data-stu-id="f89c0-136">Configure searchable fields to use the synonym map in the index definition.</span></span> <span data-ttu-id="f89c0-137">V `EnableSynonymsInHotelsIndex` povolíme synonyma u dvou polí `category` a `tags` nastavením vlastnosti `synonymMaps` na název nově nahrané mapy synonym.</span><span class="sxs-lookup"><span data-stu-id="f89c0-137">In `EnableSynonymsInHotelsIndex`, we enable synonyms on two fields `category` and `tags` by setting the `synonymMaps` property to the name of the newly uploaded synonym map.</span></span>
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
<span data-ttu-id="f89c0-138">Po přidání mapy synonym není potřeba znovu sestavovat index.</span><span class="sxs-lookup"><span data-stu-id="f89c0-138">When you add a synonym map, index rebuilds are not required.</span></span> <span data-ttu-id="f89c0-139">Mapu synonymum můžete přidat ke službě a potom změnit stávající definice pole v libovolném indexu tak, aby tuto novou mapu synonym používala.</span><span class="sxs-lookup"><span data-stu-id="f89c0-139">You can add a synonym map to your service, and then amend existing field definitions in any index to use the new synonym map.</span></span> <span data-ttu-id="f89c0-140">Přidání nových atributů nemá žádný vliv na dostupnost indexu.</span><span class="sxs-lookup"><span data-stu-id="f89c0-140">The addition of new attributes has no impact on index availability.</span></span> <span data-ttu-id="f89c0-141">To samé platí i pro zákaz synonym u pole.</span><span class="sxs-lookup"><span data-stu-id="f89c0-141">The same applies in disabling synonyms for a field.</span></span> <span data-ttu-id="f89c0-142">Stačí nastavit vlastnost `synonymMaps` na prázdný seznam.</span><span class="sxs-lookup"><span data-stu-id="f89c0-142">You can simply set the `synonymMaps` property to an empty list.</span></span>
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a><span data-ttu-id="f89c0-143">Dotazy „po“</span><span class="sxs-lookup"><span data-stu-id="f89c0-143">"After" queries</span></span>

<span data-ttu-id="f89c0-144">Po nahrání mapy synonym a aktualizaci indexu druhé volání `RunQueriesWithNonExistentTermsInIndex` vrátí tento výstup:</span><span class="sxs-lookup"><span data-stu-id="f89c0-144">After the synonym map is uploaded and the index is updated to use the synonym map, the second `RunQueriesWithNonExistentTermsInIndex` call outputs the following:</span></span>

~~~
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
<span data-ttu-id="f89c0-145">První dotaz najde dokument z pravidla `five star=>luxury`.</span><span class="sxs-lookup"><span data-stu-id="f89c0-145">The first query finds the document from the rule `five star=>luxury`.</span></span> <span data-ttu-id="f89c0-146">Druhý dotaz rozšíří vyhledávání pomocí `internet,wifi` a třetí použije k vyhledání shody v dokumentech `hotel, motel` i `economy,inexpensive=>budget`.</span><span class="sxs-lookup"><span data-stu-id="f89c0-146">The second query expands the search using `internet,wifi` and the third using both `hotel, motel` and `economy,inexpensive=>budget` in finding the documents they matched.</span></span>

<span data-ttu-id="f89c0-147">Přidání synonym úplně mění možnosti vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f89c0-147">Adding synonyms completely changes the search experience.</span></span> <span data-ttu-id="f89c0-148">Původním dotazům v tomto kurzu se nepodařilo vrátit smysluplné výsledky, i když dokumenty v indexu byly relevantní.</span><span class="sxs-lookup"><span data-stu-id="f89c0-148">In this tutorial, the original queries failed to return meaningful results even though the documents in our index were relevant.</span></span> <span data-ttu-id="f89c0-149">Povolením synonym můžeme rozšířit index tak, aby zahrnoval běžně používané termíny, a nemusíme přitom nijak upravovat podkladová data v indexu.</span><span class="sxs-lookup"><span data-stu-id="f89c0-149">By enabling synonyms, we can expand an index to include terms in common use, with no changes to underlying data in the index.</span></span>

## <a name="sample-application-source-code"></a><span data-ttu-id="f89c0-150">Zdrojový kód ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="f89c0-150">Sample application source code</span></span>
<span data-ttu-id="f89c0-151">Úplný zdrojový kód ukázkové aplikace použité v tomto názorném postupu najdete na [Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span><span class="sxs-lookup"><span data-stu-id="f89c0-151">You can find the full source code of the sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f89c0-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f89c0-152">Next steps</span></span>

* <span data-ttu-id="f89c0-153">Projděte si téma věnované [použití synonym ve službě Azure Search](search-synonyms.md).</span><span class="sxs-lookup"><span data-stu-id="f89c0-153">Review [How to use synonyms in Azure Search](search-synonyms.md)</span></span>
* <span data-ttu-id="f89c0-154">Projděte si [dokumentaci k rozhraní REST API pro synonyma](https://aka.ms/rgm6rq).</span><span class="sxs-lookup"><span data-stu-id="f89c0-154">Review [Synonyms REST API documentation](https://aka.ms/rgm6rq)</span></span>
* <span data-ttu-id="f89c0-155">Projděte si referenční materiály pro [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) a [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="f89c0-155">Browse the references for the [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
