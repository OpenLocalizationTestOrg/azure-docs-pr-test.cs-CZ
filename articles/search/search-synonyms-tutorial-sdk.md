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
# <a name="synonym-preview-c-tutorial-for-azure-search"></a><span data-ttu-id="30b91-103">Kurz synonym (Preview) v jazyce C# pro Azure Search</span><span class="sxs-lookup"><span data-stu-id="30b91-103">Synonym (preview) C# tutorial for Azure Search</span></span>

<span data-ttu-id="30b91-104">Synonyma odpovídající podle podmínek považují za vstupní termín sémanticky ekvivalentní toohello rozbalit dotazu.</span><span class="sxs-lookup"><span data-stu-id="30b91-104">Synonyms expand a query by matching on terms considered semantically equivalent toohello input term.</span></span> <span data-ttu-id="30b91-105">Například můžete "auto" toomatch dokumenty obsahující termíny hello "automobilu" nebo "vehicle".</span><span class="sxs-lookup"><span data-stu-id="30b91-105">For example, you might want "car" toomatch documents containing hello terms "automobile" or "vehicle".</span></span>

<span data-ttu-id="30b91-106">Ve službě Azure Search se synonyma definují v *mapě synonym*, pomocí *pravidel mapování*, která přidružují ekvivalentní termíny.</span><span class="sxs-lookup"><span data-stu-id="30b91-106">In Azure Search, synonyms are defined in a *synonym map*, through *mapping rules* that associate equivalent terms.</span></span> <span data-ttu-id="30b91-107">Můžete vytvořit více mapami synonymum, odešlete je jako index k dispozici tooany celé služby prostředků a pak odkazovat které jeden toouse na úrovni pole hello.</span><span class="sxs-lookup"><span data-stu-id="30b91-107">You can create multiple synonym maps, post them as a service-wide resource available tooany index, and then reference which one toouse at hello field level.</span></span> <span data-ttu-id="30b91-108">V době dotazů kromě toosearching indexu Azure Search nepodporuje vyhledávání v mapě synonymum, pokud je zadaná pro pole použitého v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="30b91-108">At query time, in addition toosearching an index, Azure Search does a lookup in a synonym map, if one is specified on fields used in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="30b91-109">Hello synonyma funkce je aktuálně ve verzi preview a podporována v pouze hello nejnovější verzi preview rozhraní API a verze sady SDK (api-version = 2016-09-01-Preview, verze SDK 4.x-preview).</span><span class="sxs-lookup"><span data-stu-id="30b91-109">hello synonyms feature is currently in preview and only supported in hello latest preview API and SDK versions (api-version=2016-09-01-Preview, SDK version 4.x-preview).</span></span> <span data-ttu-id="30b91-110">Podpora webu Azure Portal se v současnosti neposkytuje.</span><span class="sxs-lookup"><span data-stu-id="30b91-110">There is no Azure portal support at this time.</span></span> <span data-ttu-id="30b91-111">Na rozhraní API ve verzi Preview se nevztahuje žádná smlouva SLA a funkce se také mohou změnit, proto je nedoporučujeme používat v produkčních aplikacích.</span><span class="sxs-lookup"><span data-stu-id="30b91-111">Preview APIs are not under SLA and preview features may change, so we do not recommend using them in production applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30b91-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30b91-112">Prerequisites</span></span>

<span data-ttu-id="30b91-113">Kurz požadavky hello následující:</span><span class="sxs-lookup"><span data-stu-id="30b91-113">Tutorial requirements include hello following:</span></span>

* [<span data-ttu-id="30b91-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30b91-114">Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="30b91-115">Služba Azure Search</span><span class="sxs-lookup"><span data-stu-id="30b91-115">Azure Search service</span></span>](search-create-service-portal.md)
* [<span data-ttu-id="30b91-116">Verze Preview knihovny Microsoft.Azure.Search .NET</span><span class="sxs-lookup"><span data-stu-id="30b91-116">Preview version of Microsoft.Azure.Search .NET library</span></span>](https://aka.ms/search-sdk-preview)
* [<span data-ttu-id="30b91-117">Jak toouse Azure vyhledávání z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="30b91-117">How toouse Azure Search from a .NET Application</span></span>](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a><span data-ttu-id="30b91-118">Přehled</span><span class="sxs-lookup"><span data-stu-id="30b91-118">Overview</span></span>

<span data-ttu-id="30b91-119">Před a po dotazy ukazují hello hodnotu synonyma.</span><span class="sxs-lookup"><span data-stu-id="30b91-119">Before-and-after queries demonstrate hello value of synonyms.</span></span> <span data-ttu-id="30b91-120">V tomto kurzu používáme ukázkovou aplikaci, která spustí dotazy a vrátí výsledky v ukázkovém indexu.</span><span class="sxs-lookup"><span data-stu-id="30b91-120">In this tutorial, we use a sample application that executes queries and returns results on a sample index.</span></span> <span data-ttu-id="30b91-121">Ukázková aplikace Hello vytvoří malé index s názvem "hotels" naplněný dva dokumenty.</span><span class="sxs-lookup"><span data-stu-id="30b91-121">hello sample application creates a small index named "hotels" populated with two documents.</span></span> <span data-ttu-id="30b91-122">aplikace Hello provede vyhledávací dotazy pomocí podmínky a slovní spojení, která nejsou uvedena v indexu hello, povolí funkci synonyma hello, pak problémy hello stejné vyhledávání znovu.</span><span class="sxs-lookup"><span data-stu-id="30b91-122">hello application executes search queries using terms and phrases that do not appear in hello index, enables hello synonyms feature, then issues hello same searches again.</span></span> <span data-ttu-id="30b91-123">Hello uvedený níže ukazuje hello celkové toku.</span><span class="sxs-lookup"><span data-stu-id="30b91-123">hello code below demonstrates hello overall flow.</span></span>

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
<span data-ttu-id="30b91-124">Hello toocreate kroky a naplnit index ukázka hello jsou vysvětlené v [jak toouse Azure vyhledávání z aplikace .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span><span class="sxs-lookup"><span data-stu-id="30b91-124">hello steps toocreate and populate hello sample index are explained in [How toouse Azure Search from a .NET Application](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span></span>

## <a name="before-queries"></a><span data-ttu-id="30b91-125">Dotazy „před“</span><span class="sxs-lookup"><span data-stu-id="30b91-125">"Before" queries</span></span>

<span data-ttu-id="30b91-126">V `RunQueriesWithNonExistentTermsInIndex` jsme vydali vyhledávací dotazy pro „five star“, „internet“ a „economy AND hotel“.</span><span class="sxs-lookup"><span data-stu-id="30b91-126">In `RunQueriesWithNonExistentTermsInIndex`, we issue search queries with "five star", "internet", and "economy AND hotel".</span></span>
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
<span data-ttu-id="30b91-127">Ani jeden z hello dva indexované dokumenty obsahují hello podmínky, proto jsme hello následující výstup z hello nejprve `RunQueriesWithNonExistentTermsInIndex`.</span><span class="sxs-lookup"><span data-stu-id="30b91-127">Neither of hello two indexed documents contain hello terms, so we get hello following output from hello first `RunQueriesWithNonExistentTermsInIndex`.</span></span>
~~~
Search hello entire index for hello phrase "five star":

no document matched

Search hello entire index for hello term 'internet':

no document matched

Search hello entire index for hello terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a><span data-ttu-id="30b91-128">Povolení synonym</span><span class="sxs-lookup"><span data-stu-id="30b91-128">Enable synonyms</span></span>

<span data-ttu-id="30b91-129">Povolení synonyma je dvoustupňový proces.</span><span class="sxs-lookup"><span data-stu-id="30b91-129">Enabling synonyms is a two-step process.</span></span> <span data-ttu-id="30b91-130">Nám nejdřív definovat a nahrát synonymum pravidla a pak nakonfigurujte toouse pole je.</span><span class="sxs-lookup"><span data-stu-id="30b91-130">We first define and upload synonym rules and then configure fields toouse them.</span></span> <span data-ttu-id="30b91-131">proces Hello popsané v `UploadSynonyms` a `EnableSynonymsInHotelsIndex`.</span><span class="sxs-lookup"><span data-stu-id="30b91-131">hello process is outlined in `UploadSynonyms` and `EnableSynonymsInHotelsIndex`.</span></span>

1. <span data-ttu-id="30b91-132">Přidáte mapu synonymum tooyour vyhledávací službu.</span><span class="sxs-lookup"><span data-stu-id="30b91-132">Add a synonym map tooyour search service.</span></span> <span data-ttu-id="30b91-133">V `UploadSynonyms`, jsme definovat čtyři pravidla v mapě naše synonymum, desc-synonymmap' a nahrát toohello služby.</span><span class="sxs-lookup"><span data-stu-id="30b91-133">In `UploadSynonyms`, we define four rules in our synonym map 'desc-synonymmap' and upload toohello service.</span></span>
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
<span data-ttu-id="30b91-134">Mapu synonymum musí odpovídat standard s otevřeným zdrojem toohello `solr` formátu.</span><span class="sxs-lookup"><span data-stu-id="30b91-134">A synonym map must conform toohello open source standard `solr` format.</span></span> <span data-ttu-id="30b91-135">Formát Hello je vysvětleno v [synonyma ve službě Azure Search](search-synonyms.md) části hello `Apache Solr synonym format`.</span><span class="sxs-lookup"><span data-stu-id="30b91-135">hello format is explained in [Synonyms in Azure Search](search-synonyms.md) under hello section `Apache Solr synonym format`.</span></span>

2. <span data-ttu-id="30b91-136">Konfigurovat prohledatelná pole toouse hello synonymum mapu v definici indexu hello.</span><span class="sxs-lookup"><span data-stu-id="30b91-136">Configure searchable fields toouse hello synonym map in hello index definition.</span></span> <span data-ttu-id="30b91-137">V `EnableSynonymsInHotelsIndex`, povolíte synonyma na dvě pole `category` a `tags` podle nastavení hello `synonymMaps` název vlastnosti toohello hello nově nahrán synonymum mapy.</span><span class="sxs-lookup"><span data-stu-id="30b91-137">In `EnableSynonymsInHotelsIndex`, we enable synonyms on two fields `category` and `tags` by setting hello `synonymMaps` property toohello name of hello newly uploaded synonym map.</span></span>
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
<span data-ttu-id="30b91-138">Po přidání mapy synonym není potřeba znovu sestavovat index.</span><span class="sxs-lookup"><span data-stu-id="30b91-138">When you add a synonym map, index rebuilds are not required.</span></span> <span data-ttu-id="30b91-139">Můžete přidat službu tooyour mapy synonymum a potom změnit stávající definice pole v jakékoli index toouse hello nové synonymum mapování.</span><span class="sxs-lookup"><span data-stu-id="30b91-139">You can add a synonym map tooyour service, and then amend existing field definitions in any index toouse hello new synonym map.</span></span> <span data-ttu-id="30b91-140">Přidání nové atributy Hello nemá žádný vliv na dostupnost index.</span><span class="sxs-lookup"><span data-stu-id="30b91-140">hello addition of new attributes has no impact on index availability.</span></span> <span data-ttu-id="30b91-141">Hello totéž platí i v zakázání synonyma pro pole.</span><span class="sxs-lookup"><span data-stu-id="30b91-141">hello same applies in disabling synonyms for a field.</span></span> <span data-ttu-id="30b91-142">Jednoduše můžete nastavit hello `synonymMaps` vlastnost tooan prázdný seznam.</span><span class="sxs-lookup"><span data-stu-id="30b91-142">You can simply set hello `synonymMaps` property tooan empty list.</span></span>
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a><span data-ttu-id="30b91-143">Dotazy „po“</span><span class="sxs-lookup"><span data-stu-id="30b91-143">"After" queries</span></span>

<span data-ttu-id="30b91-144">Po nahrání hello synonymum mapy a hello index je aktualizovaný toouse hello synonymum mapy, hello druhý `RunQueriesWithNonExistentTermsInIndex` volání výstupy hello následující:</span><span class="sxs-lookup"><span data-stu-id="30b91-144">After hello synonym map is uploaded and hello index is updated toouse hello synonym map, hello second `RunQueriesWithNonExistentTermsInIndex` call outputs hello following:</span></span>

~~~
Search hello entire index for hello phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
<span data-ttu-id="30b91-145">Hello první dotaz najde hello dokumentu z pravidla hello `five star=>luxury`.</span><span class="sxs-lookup"><span data-stu-id="30b91-145">hello first query finds hello document from hello rule `five star=>luxury`.</span></span> <span data-ttu-id="30b91-146">druhý dotaz Hello rozšíří hello vyhledávání pomocí `internet,wifi` a hello třetí pomocí obou `hotel, motel` a `economy,inexpensive=>budget` v hledání dokumentů hello odpovídá.</span><span class="sxs-lookup"><span data-stu-id="30b91-146">hello second query expands hello search using `internet,wifi` and hello third using both `hotel, motel` and `economy,inexpensive=>budget` in finding hello documents they matched.</span></span>

<span data-ttu-id="30b91-147">Přidání synonyma zcela změní hello vyhledáváním.</span><span class="sxs-lookup"><span data-stu-id="30b91-147">Adding synonyms completely changes hello search experience.</span></span> <span data-ttu-id="30b91-148">V tomto kurzu hello původní dotazů se nezdařilo smysluplný výsledky tooreturn i v případě, že byly relevantní hello dokumenty v indexu.</span><span class="sxs-lookup"><span data-stu-id="30b91-148">In this tutorial, hello original queries failed tooreturn meaningful results even though hello documents in our index were relevant.</span></span> <span data-ttu-id="30b91-149">Povolením synonyma jsme můžete rozbalit indexu tooinclude podmínkami společné použití, s daty toounderlying žádné změny v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="30b91-149">By enabling synonyms, we can expand an index tooinclude terms in common use, with no changes toounderlying data in hello index.</span></span>

## <a name="sample-application-source-code"></a><span data-ttu-id="30b91-150">Zdrojový kód ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="30b91-150">Sample application source code</span></span>
<span data-ttu-id="30b91-151">Můžete najít hello úplný zdrojový kód vzorové aplikace hello použitý v této ukázce na [Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span><span class="sxs-lookup"><span data-stu-id="30b91-151">You can find hello full source code of hello sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span></span>

## <a name="next-steps"></a><span data-ttu-id="30b91-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30b91-152">Next steps</span></span>

* <span data-ttu-id="30b91-153">Zkontrolujte [jak synonyma toouse ve službě Azure Search](search-synonyms.md)</span><span class="sxs-lookup"><span data-stu-id="30b91-153">Review [How toouse synonyms in Azure Search](search-synonyms.md)</span></span>
* <span data-ttu-id="30b91-154">Projděte si [dokumentaci k rozhraní REST API pro synonyma](https://aka.ms/rgm6rq).</span><span class="sxs-lookup"><span data-stu-id="30b91-154">Review [Synonyms REST API documentation](https://aka.ms/rgm6rq)</span></span>
* <span data-ttu-id="30b91-155">Procházet hello odkazy pro hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) a [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="30b91-155">Browse hello references for hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
