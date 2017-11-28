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
# <a name="upload-data-tooazure-search-using-hello-net-sdk"></a><span data-ttu-id="aaaca-103">Nahrávání dat tooAzure vyhledávání pomocí .NET SDK hello</span><span class="sxs-lookup"><span data-stu-id="aaaca-103">Upload data tooAzure Search using hello .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aaaca-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="aaaca-104">Overview</span></span>](search-what-is-data-import.md)
> * [<span data-ttu-id="aaaca-105">.NET</span><span class="sxs-lookup"><span data-stu-id="aaaca-105">.NET</span></span>](search-import-data-dotnet.md)
> * [<span data-ttu-id="aaaca-106">REST</span><span class="sxs-lookup"><span data-stu-id="aaaca-106">REST</span></span>](search-import-data-rest-api.md)
> 
> 

<span data-ttu-id="aaaca-107">Tento článek vám ukáže, jak toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport data do indexu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="aaaca-107">This article will show you how toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport data into an Azure Search index.</span></span>

<span data-ttu-id="aaaca-108">Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md).</span><span class="sxs-lookup"><span data-stu-id="aaaca-108">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md).</span></span> <span data-ttu-id="aaaca-109">Tento článek také předpokládá, že jste již vytvořili `SearchServiceClient` objektu, jak je znázorněno v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md#CreateSearchServiceClient).</span><span class="sxs-lookup"><span data-stu-id="aaaca-109">This article also assumes that you have already created a `SearchServiceClient` object, as shown in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient).</span></span>

> [!NOTE]
> <span data-ttu-id="aaaca-110">Ukázkový kód v tomto článku je napsán v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="aaaca-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="aaaca-111">Hello úplný zdrojový kód najdete [na Githubu](http://aka.ms/search-dotnet-howto).</span><span class="sxs-lookup"><span data-stu-id="aaaca-111">You can find hello full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="aaaca-112">Můžete si také přečíst o hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) pro podrobnější procházení prostřednictvím hello ukázek kódu.</span><span class="sxs-lookup"><span data-stu-id="aaaca-112">You can also read about hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of hello sample code.</span></span>

<span data-ttu-id="aaaca-113">V pořadí toopush dokumentů do indexu pomocí .NET SDK hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="aaaca-113">In order toopush documents into your index using hello .NET SDK, you will need to:</span></span>

1. <span data-ttu-id="aaaca-114">Vytvoření `SearchIndexClient` indexu vyhledávání tooyour tooconnect objektu.</span><span class="sxs-lookup"><span data-stu-id="aaaca-114">Create a `SearchIndexClient` object tooconnect tooyour search index.</span></span>
2. <span data-ttu-id="aaaca-115">Vytvoření `IndexBatch` obsahující dokumenty toobe hello přidat, upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="aaaca-115">Create an `IndexBatch` containing hello documents toobe added, modified, or deleted.</span></span>
3. <span data-ttu-id="aaaca-116">Volání hello `Documents.Index` metodu vaše `SearchIndexClient` toosend hello `IndexBatch` tooyour indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="aaaca-116">Call hello `Documents.Index` method of your `SearchIndexClient` toosend hello `IndexBatch` tooyour search index.</span></span>

## <a name="create-an-instance-of-hello-searchindexclient-class"></a><span data-ttu-id="aaaca-117">Vytvoření instance třídy SearchIndexClient hello</span><span class="sxs-lookup"><span data-stu-id="aaaca-117">Create an instance of hello SearchIndexClient class</span></span>
<span data-ttu-id="aaaca-118">hello tooimport data do indexu pomocí .NET SDK služby Azure Search, budete potřebovat toocreate instanci hello `SearchIndexClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="aaaca-118">tooimport data into your index using hello Azure Search .NET SDK, you will need toocreate an instance of hello `SearchIndexClient` class.</span></span> <span data-ttu-id="aaaca-119">Můžete vytvořit tato instance sami, ale snadnější Pokud již máte `SearchServiceClient` instance toocall jeho `Indexes.GetClient` metoda.</span><span class="sxs-lookup"><span data-stu-id="aaaca-119">You can construct this instance yourself, but it's easier if you already have a `SearchServiceClient` instance toocall its `Indexes.GetClient` method.</span></span> <span data-ttu-id="aaaca-120">Například, zde je jak lze získat `SearchIndexClient` pro hello index s názvem "hotels" z `SearchServiceClient` s názvem `serviceClient`:</span><span class="sxs-lookup"><span data-stu-id="aaaca-120">For example, here is how you would obtain a `SearchIndexClient` for hello index named "hotels" from a `SearchServiceClient` named `serviceClient`:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="aaaca-121">V typické vyhledávací aplikaci se o správu a naplňování indexu stará samostatná komponenta volaná dotazy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="aaaca-121">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="aaaca-122">`Indexes.GetClient`je vhodné pro naplňování indexu, protože díky tomu můžete hello poskytovat další `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-122">`Indexes.GetClient` is convenient for populating an index because it saves you hello trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="aaaca-123">Dělá to pomocí předání klíče správce hello této můžete použít toocreate hello `SearchServiceClient` toohello nové `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-123">It does this by passing hello admin key that you used toocreate hello `SearchServiceClient` toohello new `SearchIndexClient`.</span></span> <span data-ttu-id="aaaca-124">V rámci hello aplikace, který provádí dotazy, je však lepší hello toocreate `SearchIndexClient` přímo tak, že abyste mohli předávat klíč dotazů místo klíče správce.</span><span class="sxs-lookup"><span data-stu-id="aaaca-124">However, in hello part of your application that executes queries, it is better toocreate hello `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="aaaca-125">To je konzistentní s hello [Princip nejnižších nutných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) a pomůže toomake bezpečnější aplikace.</span><span class="sxs-lookup"><span data-stu-id="aaaca-125">This is consistent with hello [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) and will help toomake your application more secure.</span></span> <span data-ttu-id="aaaca-126">Můžete najít další informace o klíčích a správce dotazu v hello [referenční dokumentace rozhraní API REST Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="aaaca-126">You can find out more about admin keys and query keys in hello [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
> 
> 

<span data-ttu-id="aaaca-127">`SearchIndexClient` obsahuje vlastnost `Documents`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-127">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="aaaca-128">Tato vlastnost poskytuje všechny metody hello tooadd, musíte změnit, odstranit nebo dotazování dokumentů v indexu.</span><span class="sxs-lookup"><span data-stu-id="aaaca-128">This property provides all hello methods you need tooadd, modify, delete, or query documents in your index.</span></span>

## <a name="decide-which-indexing-action-toouse"></a><span data-ttu-id="aaaca-129">Rozhodněte, které indexování toouse akce</span><span class="sxs-lookup"><span data-stu-id="aaaca-129">Decide which indexing action toouse</span></span>
<span data-ttu-id="aaaca-130">tooimport dat pomocí .NET SDK hello, budete potřebovat toopackage dat do `IndexBatch` objektu.</span><span class="sxs-lookup"><span data-stu-id="aaaca-130">tooimport data using hello .NET SDK, you will need toopackage up your data into an `IndexBatch` object.</span></span> <span data-ttu-id="aaaca-131">`IndexBatch` Zapouzdřuje kolekci `IndexAction` objekty, z nichž každý obsahuje dokument a vlastnost, která říká službě Azure Search tooperform jaké akce na tomto dokumentu (odeslání, sloučení, odstranění atd.).</span><span class="sxs-lookup"><span data-stu-id="aaaca-131">An `IndexBatch` encapsulates a collection of `IndexAction` objects, each of which contains a document and a property that tells Azure Search what action tooperform on that document (upload, merge, delete, etc).</span></span> <span data-ttu-id="aaaca-132">Podle toho, která hello níže akce, který zvolíte musí být objekt pro každý dokument obsahovat pouze určitá pole:</span><span class="sxs-lookup"><span data-stu-id="aaaca-132">Depending on which of hello below actions you choose, only certain fields must be included for each document:</span></span>

| <span data-ttu-id="aaaca-133">Akce</span><span class="sxs-lookup"><span data-stu-id="aaaca-133">Action</span></span> | <span data-ttu-id="aaaca-134">Popis</span><span class="sxs-lookup"><span data-stu-id="aaaca-134">Description</span></span> | <span data-ttu-id="aaaca-135">Potřebná pole pro každý dokument</span><span class="sxs-lookup"><span data-stu-id="aaaca-135">Necessary fields for each document</span></span> | <span data-ttu-id="aaaca-136">Poznámky</span><span class="sxs-lookup"><span data-stu-id="aaaca-136">Notes</span></span> |
| --- | --- | --- | --- |
| `Upload` |<span data-ttu-id="aaaca-137">`Upload` Akci je podobný tooan "upsert", kde hello dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="aaaca-137">An `Upload` action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> |<span data-ttu-id="aaaca-138">klíč a další pole chcete toodefine</span><span class="sxs-lookup"><span data-stu-id="aaaca-138">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="aaaca-139">Pokud aktualizujete nebo nahrazujete stávající dokument, bude každé pole, které není určený v požadavku hello mít nastavené příliš`null`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-139">When updating/replacing an existing document, any field that is not specified in hello request will have its field set too`null`.</span></span> <span data-ttu-id="aaaca-140">K tomu dojde i v případě, že bylo hello pole dříve nastavené tooa jinou hodnotu než null.</span><span class="sxs-lookup"><span data-stu-id="aaaca-140">This occurs even when hello field was previously set tooa non-null value.</span></span> |
| `Merge` |<span data-ttu-id="aaaca-141">Aktualizace stávající dokumentů s hello zadaná pole.</span><span class="sxs-lookup"><span data-stu-id="aaaca-141">Updates an existing document with hello specified fields.</span></span> <span data-ttu-id="aaaca-142">Pokud hello dokument v indexu hello neexistuje, sloučení hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="aaaca-142">If hello document does not exist in hello index, hello merge will fail.</span></span> |<span data-ttu-id="aaaca-143">klíč a další pole chcete toodefine</span><span class="sxs-lookup"><span data-stu-id="aaaca-143">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="aaaca-144">Každé pole zadané ve sloučení nahradí stávající pole hello v dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-144">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="aaaca-145">To zahrnuje i pole typu `DataType.Collection(DataType.String)`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-145">This includes fields of type `DataType.Collection(DataType.String)`.</span></span> <span data-ttu-id="aaaca-146">Například pokud hello dokument obsahuje pole `tags` s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro `tags`, hello konečná hodnota hello `tags` pole bude `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-146">For example, if hello document contains a field `tags` with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for `tags`, hello final value of hello `tags` field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="aaaca-147">Hodnota nebude `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-147">It will not be `["budget", "economy", "pool"]`.</span></span> |
| `MergeOrUpload` |<span data-ttu-id="aaaca-148">Tato akce se chová jako `Merge` Pokud dokument s hello zadaný klíč již existuje v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-148">This action behaves like `Merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="aaaca-149">Pokud hello dokument neexistuje, chová se jako `Upload` s novým dokumentem.</span><span class="sxs-lookup"><span data-stu-id="aaaca-149">If hello document does not exist, it behaves like `Upload` with a new document.</span></span> |<span data-ttu-id="aaaca-150">klíč a další pole chcete toodefine</span><span class="sxs-lookup"><span data-stu-id="aaaca-150">key, plus any other fields you wish toodefine</span></span> |- |
| `Delete` |<span data-ttu-id="aaaca-151">Odebere zadaný dokument hello hello index.</span><span class="sxs-lookup"><span data-stu-id="aaaca-151">Removes hello specified document from hello index.</span></span> |<span data-ttu-id="aaaca-152">pouze klíč</span><span class="sxs-lookup"><span data-stu-id="aaaca-152">key only</span></span> |<span data-ttu-id="aaaca-153">Všechna zadaná pole kromě pole klíče hello budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="aaaca-153">Any fields you specify other than hello key field will be ignored.</span></span> <span data-ttu-id="aaaca-154">Pokud chcete tooremove z dokumentu jednotlivá pole, použijte `Merge` místo a jednoduše nastavte pole hello explicitně toonull.</span><span class="sxs-lookup"><span data-stu-id="aaaca-154">If you want tooremove an individual field from a document, use `Merge` instead and simply set hello field explicitly toonull.</span></span> |

<span data-ttu-id="aaaca-155">Můžete určit, jaká opatření se mají toouse pomocí různých statických metod hello hello `IndexBatch` a `IndexAction` třídy, jak je znázorněno v další části hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-155">You can specify what action you want toouse with hello various static methods of hello `IndexBatch` and `IndexAction` classes, as shown in hello next section.</span></span>

## <a name="construct-your-indexbatch"></a><span data-ttu-id="aaaca-156">Vytvoření třídy IndexBatch</span><span class="sxs-lookup"><span data-stu-id="aaaca-156">Construct your IndexBatch</span></span>
<span data-ttu-id="aaaca-157">Teď, když víte, jaké akce tooperform na dokumentech, jste hello připravené tooconstruct `IndexBatch`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-157">Now that you know which actions tooperform on your documents, you are ready tooconstruct hello `IndexBatch`.</span></span> <span data-ttu-id="aaaca-158">Příklad dole ukazuje jak Hello toocreate dávky s různými akcemi.</span><span class="sxs-lookup"><span data-stu-id="aaaca-158">hello example below shows how toocreate a batch with a few different actions.</span></span> <span data-ttu-id="aaaca-159">Všimněte si, že naše Ukázka používá vlastní třídu s názvem `Hotel` který mapuje tooa dokument v indexu "hotels" hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-159">Note that our example uses a custom class called `Hotel` that maps tooa document in hello "hotels" index.</span></span>

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

<span data-ttu-id="aaaca-160">V tomto případě používáme `Upload`, `MergeOrUpload`, a `Delete` jako akce hledání podle specifikace hello metody volat u hello `IndexAction` třídy.</span><span class="sxs-lookup"><span data-stu-id="aaaca-160">In this case, we are using `Upload`, `MergeOrUpload`, and `Delete` as our search actions, as specified by hello methods called on hello `IndexAction` class.</span></span>

<span data-ttu-id="aaaca-161">Předpokládejme, že je ukázkový index „hotels“ již naplněný řadou dokumentů.</span><span class="sxs-lookup"><span data-stu-id="aaaca-161">Assume that this example "hotels" index is already populated with a number of documents.</span></span> <span data-ttu-id="aaaca-162">Všimněte si, jak nebylo nutné toospecify všechny hello možná pole dokumentu při použití `MergeOrUpload` a jak jsme zadali pouze klíč dokumentu hello (`HotelId`) při použití `Delete`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-162">Note how we did not have toospecify all hello possible document fields when using `MergeOrUpload` and how we only specified hello document key (`HotelId`) when using `Delete`.</span></span>

<span data-ttu-id="aaaca-163">Také Upozorňujeme, že může obsahovat jenom too1000 dokumentů v jedné žádosti indexování.</span><span class="sxs-lookup"><span data-stu-id="aaaca-163">Also, note that you can only include up too1000 documents in a single indexing request.</span></span>

> [!NOTE]
> <span data-ttu-id="aaaca-164">V tomto příkladu používáme různé akce toodifferent dokumenty.</span><span class="sxs-lookup"><span data-stu-id="aaaca-164">In this example, we are applying different actions toodifferent documents.</span></span> <span data-ttu-id="aaaca-165">Pokud byste chtěli tooperform hello stejné akce na všech dokumentech v dávce hello místo volání `IndexBatch.New`, můžete použít jiné statické metody třídy hello `IndexBatch`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-165">If you wanted tooperform hello same actions across all documents in hello batch, instead of calling `IndexBatch.New`, you could use hello other static methods of `IndexBatch`.</span></span> <span data-ttu-id="aaaca-166">Dávky můžete vytvořit například voláním `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` nebo `IndexBatch.Delete`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-166">For example, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete`.</span></span> <span data-ttu-id="aaaca-167">Tyto metody přijímají místo objektů `IndexAction` kolekci dokumentů (v této ukázce objekty typu `Hotel`).</span><span class="sxs-lookup"><span data-stu-id="aaaca-167">These methods take a collection of documents (objects of type `Hotel` in this example) instead of `IndexAction` objects.</span></span>
> 
> 

## <a name="import-data-toohello-index"></a><span data-ttu-id="aaaca-168">Import dat toohello indexu</span><span class="sxs-lookup"><span data-stu-id="aaaca-168">Import data toohello index</span></span>
<span data-ttu-id="aaaca-169">Teď, když jste inicializovali `IndexBatch` objekt, můžete ho odeslat toohello index voláním `Documents.Index` na vaše `SearchIndexClient` objektu.</span><span class="sxs-lookup"><span data-stu-id="aaaca-169">Now that you have an initialized `IndexBatch` object, you can send it toohello index by calling `Documents.Index` on your `SearchIndexClient` object.</span></span> <span data-ttu-id="aaaca-170">Následující příklad ukazuje, jak Hello toocall `Index`, spolu s některými dalšími kroky, které budete potřebovat tooperform:</span><span class="sxs-lookup"><span data-stu-id="aaaca-170">hello following example shows how toocall `Index`, as well as some extra steps you will need tooperform:</span></span>

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

<span data-ttu-id="aaaca-171">Poznámka: hello `try` / `catch` které obaluje volání toohello hello `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="aaaca-171">Note hello `try`/`catch` surrounding hello call toohello `Index` method.</span></span> <span data-ttu-id="aaaca-172">Hello blok catch ošetřuje s případem důležitých chyb pro indexování.</span><span class="sxs-lookup"><span data-stu-id="aaaca-172">hello catch block handles an important error case for indexing.</span></span> <span data-ttu-id="aaaca-173">Pokud služba Azure Search selže tooindex některé hello dokumentů v dávce hello `IndexBatchException` vyvolá výjimku `Documents.Index`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-173">If your Azure Search service fails tooindex some of hello documents in hello batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="aaaca-174">To může nastat v případě indexování dokumentů, zatímco je služba velmi zatížená.</span><span class="sxs-lookup"><span data-stu-id="aaaca-174">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="aaaca-175">**Důrazně doporučujeme v kódu explicitně zpracovávat tento případ.**</span><span class="sxs-lookup"><span data-stu-id="aaaca-175">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="aaaca-176">Můžete odložit a poté opakujte indexování hello dokumentů, které selhaly, nebo můžete protokolu a pokračovat jako hello znovu, nebo to můžete provést něco jiného v závislosti na požadavcích konzistence dat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="aaaca-176">You can delay and then retry indexing hello documents that failed, or you can log and continue like hello sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

<span data-ttu-id="aaaca-177">Nakonec hello kód v příkladu hello výše na 2 sekundy odloží.</span><span class="sxs-lookup"><span data-stu-id="aaaca-177">Finally, hello code in hello example above delays for two seconds.</span></span> <span data-ttu-id="aaaca-178">Indexování probíhá asynchronně služby Azure Search, takže ukázková aplikace hello musí toowait krátkou dobu tooensure, že jsou k dispozici pro vyhledávání dokumentů hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-178">Indexing happens asynchronously in your Azure Search service, so hello sample application needs toowait a short time tooensure that hello documents are available for searching.</span></span> <span data-ttu-id="aaaca-179">Tato odložení se obvykle používají pouze v ukázkových aplikacích a při testech.</span><span class="sxs-lookup"><span data-stu-id="aaaca-179">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

<a name="HotelClass"></a>

### <a name="how-hello-net-sdk-handles-documents"></a><span data-ttu-id="aaaca-180">Jak hello .NET SDK zpracovává dokumenty</span><span class="sxs-lookup"><span data-stu-id="aaaca-180">How hello .NET SDK handles documents</span></span>
<span data-ttu-id="aaaca-181">Asi vás zajímá, jak hello Azure Search .NET SDK je možné tooupload instance uživatelsky definované třídy, jako je `Hotel` toohello index.</span><span class="sxs-lookup"><span data-stu-id="aaaca-181">You may be wondering how hello Azure Search .NET SDK is able tooupload instances of a user-defined class like `Hotel` toohello index.</span></span> <span data-ttu-id="aaaca-182">toohelp odpověď na tuto otázku, podíváme se na hello `Hotel` třídy, která mapuje toohello schéma indexu definované v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md#DefineIndex):</span><span class="sxs-lookup"><span data-stu-id="aaaca-182">toohelp answer that question, let's look at hello `Hotel` class, which maps toohello index schema defined in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#DefineIndex):</span></span>

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

<span data-ttu-id="aaaca-183">Hello nejprve thing toonotice je, že každá veřejná vlastnost třídy `Hotel` odpovídá tooa pole v definici indexu hello, ale s jedním zásadním rozdílem: název hello každého pole začíná malým písmenem ("camelCase"), zatímco název každé veřejné hello Vlastnost `Hotel` začíná velké písmeno ("pascalcase").</span><span class="sxs-lookup"><span data-stu-id="aaaca-183">hello first thing toonotice is that each public property of `Hotel` corresponds tooa field in hello index definition, but with one crucial difference: hello name of each field starts with a lower-case letter ("camel case"), while hello name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="aaaca-184">Toto je běžný scénář v .NET aplikacích provádějících datové vazby, kde je hello cílové schéma mimo hello kontrolu nad vývojář aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-184">This is a common scenario in .NET applications that perform data-binding where hello target schema is outside hello control of hello application developer.</span></span> <span data-ttu-id="aaaca-185">Místo tooviolate hello směrnic pojmenování .NET ve vlastnosti názvy camelCase toho se dá zjistit hello SDK toomap hello vlastnost názvy toocamel případ automaticky s hello `[SerializePropertyNamesAsCamelCase]` atribut.</span><span class="sxs-lookup"><span data-stu-id="aaaca-185">Rather than having tooviolate hello .NET naming guidelines by making property names camel-case, you can tell hello SDK toomap hello property names toocamel-case automatically with hello `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="aaaca-186">Hello .NET SDK služby Azure Search používá hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) tooserialize knihovny a deserializaci vaše vlastní model tooand objekty z formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="aaaca-186">hello Azure Search .NET SDK uses hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library tooserialize and deserialize your custom model objects tooand from JSON.</span></span> <span data-ttu-id="aaaca-187">V případě potřeby lze serializaci přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="aaaca-187">You can customize this serialization if needed.</span></span> <span data-ttu-id="aaaca-188">Další podrobnosti najdete v části [Vlastní serializace pomocí technologie JSON.NET](search-howto-dotnet-sdk.md#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="aaaca-188">You can find more details in [Custom Serialization with JSON.NET](search-howto-dotnet-sdk.md#JsonDotNet).</span></span> <span data-ttu-id="aaaca-189">Příkladem toho je použití hello hello `[JsonProperty]` atribut hello `DescriptionFr` vlastnost ve výše hello ukázkovém kódu.</span><span class="sxs-lookup"><span data-stu-id="aaaca-189">One example of this is hello use of hello `[JsonProperty]` attribute on hello `DescriptionFr` property in hello sample code above.</span></span>
> 
> 

<span data-ttu-id="aaaca-190">Hello důležité hello `Hotel` třídy jsou hello datové typy veřejných vlastností hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-190">hello second important thing about hello `Hotel` class are hello data types of hello public properties.</span></span> <span data-ttu-id="aaaca-191">Hello .NET typy těchto vlastností mapy tootheir odpovídající typy polí v definici indexu hello.</span><span class="sxs-lookup"><span data-stu-id="aaaca-191">hello .NET types of these properties map tootheir equivalent field types in hello index definition.</span></span> <span data-ttu-id="aaaca-192">Například hello `Category` vlastnosti řetězce mapuje toohello `category` pole, které je typu `DataType.String`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-192">For example, hello `Category` string property maps toohello `category` field, which is of type `DataType.String`.</span></span> <span data-ttu-id="aaaca-193">Podobná mapování typu probíhají mezi `bool?` a `DataType.Boolean`, `DateTimeOffset?` a `DataType.DateTimeOffset` atd.</span><span class="sxs-lookup"><span data-stu-id="aaaca-193">There are similar type mappings between `bool?` and `DataType.Boolean`, `DateTimeOffset?` and `DataType.DateTimeOffset`, and so forth.</span></span> <span data-ttu-id="aaaca-194">Hello konkrétní pravidla pro hello mapování typu jsou popsaná u hello `Documents.Get` metoda v hello [Azure Search .NET SDK odkaz](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="aaaca-194">hello specific rules for hello type mapping are documented with hello `Documents.Get` method in hello [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="aaaca-195">Tato možnost toouse vaše vlastní třídy jako dokumenty funguje v obou směrech; Můžete také načíst výsledky vyhledávání a nechat hello SDK automaticky deserializovala tooa typu, jak je znázorněno v hello [následující článek](search-query-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="aaaca-195">This ability toouse your own classes as documents works in both directions; You can also retrieve search results and have hello SDK automatically deserialize them tooa type of your choice, as shown in hello [next article](search-query-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="aaaca-196">Hello .NET SDK služby Azure Search také podporuje dynamicky typované dokumenty pomocí hello `Document` třída, která zajišťuje mapování klíč hodnota hodnot toofield názvy polí.</span><span class="sxs-lookup"><span data-stu-id="aaaca-196">hello Azure Search .NET SDK also supports dynamically-typed documents using hello `Document` class, which is a key/value mapping of field names toofield values.</span></span> <span data-ttu-id="aaaca-197">To je užitečné v případech, pokud neznáte schéma indexu hello v době návrhu, nebo pokud je třídy modelu toospecific nepohodlná toobind.</span><span class="sxs-lookup"><span data-stu-id="aaaca-197">This is useful in scenarios where you don't know hello index schema at design-time, or where it would be inconvenient toobind toospecific model classes.</span></span> <span data-ttu-id="aaaca-198">Všechny metody hello v hello SDK, které pracují s dokumenty, mají přetížení, které pracují s hello `Document` třídy, jakož i přetížení silně typované, která přebírají parametr obecného typu.</span><span class="sxs-lookup"><span data-stu-id="aaaca-198">All hello methods in hello SDK that deal with documents have overloads that work with hello `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="aaaca-199">Pouze hello pozdější se používají v hello ukázkový kód v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="aaaca-199">Only hello latter are used in hello sample code in this article.</span></span>
> 
> 

<span data-ttu-id="aaaca-200">**Proč byste měli používat datové typy s možnou hodnotou null**</span><span class="sxs-lookup"><span data-stu-id="aaaca-200">**Why you should use nullable data types**</span></span>

<span data-ttu-id="aaaca-201">Při navrhování indexu Azure Search toomap tooan vlastní model třídy, doporučujeme deklarovat vlastnosti typů hodnot, jako `bool` a `int` toobe s možnou hodnotou Null (například `bool?` místo `bool`).</span><span class="sxs-lookup"><span data-stu-id="aaaca-201">When designing your own model classes toomap tooan Azure Search index, we recommend declaring properties of value types such as `bool` and `int` toobe nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="aaaca-202">Pokud použijete vlastnost se zakázanou hodnotou Null, máte příliš**zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hello hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="aaaca-202">If you use a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="aaaca-203">Hello SDK ani hello služby Azure Search vám pomůže tooenforce vám to.</span><span class="sxs-lookup"><span data-stu-id="aaaca-203">Neither hello SDK nor hello Azure Search service will help you tooenforce this.</span></span>

<span data-ttu-id="aaaca-204">Nejedná se pouze o hypotetický problém: Představte si situaci, kdy přidáte nový pole tooan existující index typu `DataType.Int32`.</span><span class="sxs-lookup"><span data-stu-id="aaaca-204">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `DataType.Int32`.</span></span> <span data-ttu-id="aaaca-205">Po aktualizaci definice indexu hello, budou všechny dokumenty mít pro toto nové pole hodnotu null (protože jsou všechny typy s možnou hodnotou Null ve službě Azure Search).</span><span class="sxs-lookup"><span data-stu-id="aaaca-205">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="aaaca-206">Pokud pak použijete třídu modelu s hodnotou Null `int` vlastnosti pro toto pole, zobrazí se `JsonSerializationException` při pokusu o tooretrieve dokumenty podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="aaaca-206">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="aaaca-207">Z tohoto důvodu doporučujeme jako osvědčený postup používat ve třídách modelu typy s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="aaaca-207">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aaaca-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aaaca-208">Next steps</span></span>
<span data-ttu-id="aaaca-209">Po naplnění indexu Azure Search, bude připravená toostart vystavování toosearch dotazy pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="aaaca-209">After populating your Azure Search index, you will be ready toostart issuing queries toosearch for documents.</span></span> <span data-ttu-id="aaaca-210">Podrobnosti naleznete v tématu [Dotazování indexu Azure Search](search-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aaaca-210">See [Query Your Azure Search Index](search-query-overview.md) for details.</span></span>

