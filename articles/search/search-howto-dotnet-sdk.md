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
# <a name="how-toouse-azure-search-from-a-net-application"></a><span data-ttu-id="c6a6f-103">Jak toouse Azure vyhledávání z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="c6a6f-103">How toouse Azure Search from a .NET Application</span></span>
<span data-ttu-id="c6a6f-104">Tento článek je tooget návod vám fungovaly s hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-104">This article is a walkthrough tooget you up and running with hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span> <span data-ttu-id="c6a6f-105">Můžete použít hello .NET SDK tooimplement s formátováním možností vyhledávání do vaší aplikace pomocí Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-105">You can use hello .NET SDK tooimplement a rich search experience in your application using Azure Search.</span></span>

## <a name="whats-in-hello-azure-search-sdk"></a><span data-ttu-id="c6a6f-106">Co je hello vyhledávání systému Azure SDK</span><span class="sxs-lookup"><span data-stu-id="c6a6f-106">What's in hello Azure Search SDK</span></span>
<span data-ttu-id="c6a6f-107">Hello SDK se skládá z Klientská knihovna `Microsoft.Azure.Search`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-107">hello SDK consists of a client library, `Microsoft.Azure.Search`.</span></span> <span data-ttu-id="c6a6f-108">Ji umožňuje vám toomanage vaše indexy, zdroje dat a indexery, a také nahrát a správě dokumentů a spouštět dotazy, aniž by bylo toodeal s podrobnostmi hello HTTP a JSON.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-108">It enables you toomanage your indexes, data sources, and indexers, as well as upload and manage documents, and execute queries, all without having toodeal with hello details of HTTP and JSON.</span></span>

<span data-ttu-id="c6a6f-109">Definuje Hello klientské knihovny tříd jako `Index`, `Field`, a `Document`, stejně jako operací, jako `Indexes.Create` a `Documents.Search` na hello `SearchServiceClient` a `SearchIndexClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-109">hello client library defines classes like `Index`, `Field`, and `Document`, as well as operations like `Indexes.Create` and `Documents.Search` on hello `SearchServiceClient` and `SearchIndexClient` classes.</span></span> <span data-ttu-id="c6a6f-110">Tyto třídy jsou uspořádány do hello následující obory názvů:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-110">These classes are organized into hello following namespaces:</span></span>

* [<span data-ttu-id="c6a6f-111">Microsoft.Azure.Search</span><span class="sxs-lookup"><span data-stu-id="c6a6f-111">Microsoft.Azure.Search</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [<span data-ttu-id="c6a6f-112">Microsoft.Azure.Search.Models</span><span class="sxs-lookup"><span data-stu-id="c6a6f-112">Microsoft.Azure.Search.Models</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

<span data-ttu-id="c6a6f-113">aktuální verze Hello hello Azure Search .NET SDK je nyní všeobecně dostupná.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-113">hello current version of hello Azure Search .NET SDK is now generally available.</span></span> <span data-ttu-id="c6a6f-114">Pokud chcete zpětnou vazbu tooprovide nám tooincorporate v další verzi hello, navštivte naše [zpětné vazby stránky](https://feedback.azure.com/forums/263029-azure-search/).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-114">If you would like tooprovide feedback for us tooincorporate in hello next version, please visit our [feedback page](https://feedback.azure.com/forums/263029-azure-search/).</span></span>

<span data-ttu-id="c6a6f-115">Hello .NET SDK podporuje verzi `2016-09-01` z hello [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-115">hello .NET SDK supports version `2016-09-01` of hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span> <span data-ttu-id="c6a6f-116">Tato verze teď zahrnuje podporu pro vlastní analyzátorů a objektů Blob v Azure a Azure Table podpora indexeru.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-116">This version now includes support for custom analyzers and Azure Blob and Azure Table indexer support.</span></span> <span data-ttu-id="c6a6f-117">Zobrazit náhled funkce, které jsou *není* jsou součástí této verze, jako je podpora pro indexování soubory JSON a sdíleného svazku clusteru v [preview](search-api-2015-02-28-preview.md) a dostupný prostřednictvím hello starší [verze 2.0 preview hello .NET SDK ](https://aka.ms/search-sdk-preview).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-117">Preview features that are *not* part of this version, such as support for indexing JSON and CSV files, are in [preview](search-api-2015-02-28-preview.md) and available via hello older [2.0-preview version of hello .NET SDK](https://aka.ms/search-sdk-preview).</span></span>

<span data-ttu-id="c6a6f-118">Tato sada SDK nepodporuje [operace správy](https://docs.microsoft.com/rest/api/searchmanagement/) jako je například vytváření a škálování služby vyhledávání a správa klíče rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-118">This SDK does not support [Management Operations](https://docs.microsoft.com/rest/api/searchmanagement/) such as creating and scaling Search services and managing API keys.</span></span> <span data-ttu-id="c6a6f-119">Pokud potřebujete toomanage vašich vyhledávání prostředků z aplikace .NET, můžete použít hello [SDK služby Azure Search .NET správu](https://aka.ms/search-mgmt-sdk).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-119">If you need toomanage your Search resources from a .NET application, you can use hello [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).</span></span>

## <a name="upgrading-toohello-latest-version-of-hello-sdk"></a><span data-ttu-id="c6a6f-120">Upgrade toohello nejnovější verzi hello SDK</span><span class="sxs-lookup"><span data-stu-id="c6a6f-120">Upgrading toohello latest version of hello SDK</span></span>
<span data-ttu-id="c6a6f-121">Pokud už používáte starší verzi hello .NET SDK služby Azure Search a chcete tooupgrade toohello nová všeobecně dostupná verze, [v tomto článku](search-dotnet-sdk-migration.md) vysvětluje, jak.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-121">If you're already using an older version of hello Azure Search .NET SDK and you'd like tooupgrade toohello new generally available version, [this article](search-dotnet-sdk-migration.md) explains how.</span></span>

## <a name="requirements-for-hello-sdk"></a><span data-ttu-id="c6a6f-122">Požadavky pro hello SDK</span><span class="sxs-lookup"><span data-stu-id="c6a6f-122">Requirements for hello SDK</span></span>
1. <span data-ttu-id="c6a6f-123">Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-123">Visual Studio 2017.</span></span>
2. <span data-ttu-id="c6a6f-124">Vlastní službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-124">Your own Azure Search service.</span></span> <span data-ttu-id="c6a6f-125">V pořadí toouse hello SDK budete potřebovat hello název vaší služby a jeden či více klíčů rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-125">In order toouse hello SDK, you will need hello name of your service and one or more API keys.</span></span> <span data-ttu-id="c6a6f-126">[Vytvoření služby portálu hello](search-create-service-portal.md) vám pomohou dokončit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-126">[Create a service in hello portal](search-create-service-portal.md) will help you through these steps.</span></span>
3. <span data-ttu-id="c6a6f-127">Stáhnout hello Azure Search .NET SDK [balíček NuGet](http://www.nuget.org/packages/Microsoft.Azure.Search) pomocí "Správa balíčků NuGet" v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-127">Download hello Azure Search .NET SDK [NuGet package](http://www.nuget.org/packages/Microsoft.Azure.Search) by using "Manage NuGet Packages" in Visual Studio.</span></span> <span data-ttu-id="c6a6f-128">Právě vyhledejte název balíčku hello `Microsoft.Azure.Search` na NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-128">Just search for hello package name `Microsoft.Azure.Search` on NuGet.org.</span></span>

<span data-ttu-id="c6a6f-129">Hello Azure Search .NET SDK podporuje aplikace cílené na hello rozhraní .NET Framework 4.6 a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-129">hello Azure Search .NET SDK supports applications targeting hello .NET Framework 4.6 and .NET Core.</span></span>

## <a name="core-scenarios"></a><span data-ttu-id="c6a6f-130">Základní scénáře</span><span class="sxs-lookup"><span data-stu-id="c6a6f-130">Core scenarios</span></span>
<span data-ttu-id="c6a6f-131">Existuje několik věcí, které budete potřebovat toodo v vyhledávací aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-131">There are several things you'll need toodo in your search application.</span></span> <span data-ttu-id="c6a6f-132">V tomto kurzu nabídneme tyto základní scénáře:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-132">In this tutorial, we'll cover these core scenarios:</span></span>

* <span data-ttu-id="c6a6f-133">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="c6a6f-133">Creating an index</span></span>
* <span data-ttu-id="c6a6f-134">Naplňování indexu hello s dokumenty</span><span class="sxs-lookup"><span data-stu-id="c6a6f-134">Populating hello index with documents</span></span>
* <span data-ttu-id="c6a6f-135">Hledání dokumentů pomocí fulltextové vyhledávání a filtry</span><span class="sxs-lookup"><span data-stu-id="c6a6f-135">Searching for documents using full-text search and filters</span></span>

<span data-ttu-id="c6a6f-136">Hello ukázkový kód, který následuje znázorňuje všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-136">hello sample code that follows illustrates each of these.</span></span> <span data-ttu-id="c6a6f-137">Zaregistrované fragmenty kódu hello volné toouse ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-137">Feel free toouse hello code snippets in your own application.</span></span>

### <a name="overview"></a><span data-ttu-id="c6a6f-138">Přehled</span><span class="sxs-lookup"><span data-stu-id="c6a6f-138">Overview</span></span>
<span data-ttu-id="c6a6f-139">Hello ukázkovou aplikaci jsme budete seznamovat vytvoří novou index s názvem "hotels", naplní s několika dokumenty a potom provede některé vyhledávací dotazy.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-139">hello sample application we'll be exploring creates a new index named "hotels", populates it with a few documents, then executes some search queries.</span></span> <span data-ttu-id="c6a6f-140">Tady je hlavní programu hello zobrazující celkové toku hello:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-140">Here is hello main program, showing hello overall flow:</span></span>

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
> <span data-ttu-id="c6a6f-141">Můžete najít hello úplný zdrojový kód vzorové aplikace hello použitý v této ukázce na [Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-141">You can find hello full source code of hello sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>
> 
>

<span data-ttu-id="c6a6f-142">Projdeme tento krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-142">We'll walk through this step by step.</span></span> <span data-ttu-id="c6a6f-143">Nejdřív potřebujeme toocreate nový `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-143">First we need toocreate a new `SearchServiceClient`.</span></span> <span data-ttu-id="c6a6f-144">Tento objekt umožňuje toomanage indexy.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-144">This object allows you toomanage indexes.</span></span> <span data-ttu-id="c6a6f-145">Pořadí tooconstruct jeden je nutné tooprovide název vaší služby Azure Search, jakož i klíčem rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-145">In order tooconstruct one, you need tooprovide your Azure Search service name as well as an admin API key.</span></span> <span data-ttu-id="c6a6f-146">Tyto informace můžete zadat v hello `appsettings.json` souboru hello [ukázkové aplikace](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-146">You can enter this information in hello `appsettings.json` file of hello [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

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
> <span data-ttu-id="c6a6f-147">Pokud jste zadali nesprávné klávesy (například klíč dotazů kde nebyla nutná klíč správce), hello `SearchServiceClient` vyvolá výjimku `CloudException` s hello chybová zpráva "Zakázané" hello poprvé zavoláte metodu operace, jako například `Indexes.Create`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-147">If you provide an incorrect key (for example, a query key where an admin key was required), hello `SearchServiceClient` will throw a `CloudException` with hello error message "Forbidden" hello first time you call an operation method on it, such as `Indexes.Create`.</span></span> <span data-ttu-id="c6a6f-148">V takovém případě tooyou zkontrolujte naše klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-148">If this happens tooyou, double-check our API key.</span></span>
> 
> 

<span data-ttu-id="c6a6f-149">Hello další několik řádků volání metody toocreate index s názvem "hotels", nejprve odstraňování Pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-149">hello next few lines call methods toocreate an index named "hotels", deleting it first if it already exists.</span></span> <span data-ttu-id="c6a6f-150">Jsme provede tyto metody trochu později.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-150">We will walk through these methods a little later.</span></span>

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

<span data-ttu-id="c6a6f-151">V dalším kroku hello index musí toobe naplněno.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-151">Next, hello index needs toobe populated.</span></span> <span data-ttu-id="c6a6f-152">toodo, je nutné zadat `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-152">toodo this, we will need a `SearchIndexClient`.</span></span> <span data-ttu-id="c6a6f-153">Existují dva způsoby tooobtain jednu: vytváření ho nebo volání `Indexes.GetClient` na hello `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-153">There are two ways tooobtain one: by constructing it, or by calling `Indexes.GetClient` on hello `SearchServiceClient`.</span></span> <span data-ttu-id="c6a6f-154">Používáme hello pozdější ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-154">We use hello latter for convenience.</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="c6a6f-155">V typické vyhledávací aplikaci se o správu a naplňování indexu stará samostatná komponenta volaná dotazy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-155">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="c6a6f-156">`Indexes.GetClient`je vhodné pro naplňování indexu, protože díky tomu můžete hello poskytovat další `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-156">`Indexes.GetClient` is convenient for populating an index because it saves you hello trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="c6a6f-157">Dělá to pomocí předání klíče správce hello této můžete použít toocreate hello `SearchServiceClient` toohello nové `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-157">It does this by passing hello admin key that you used toocreate hello `SearchServiceClient` toohello new `SearchIndexClient`.</span></span> <span data-ttu-id="c6a6f-158">V rámci hello aplikace, který provádí dotazy, je však lepší hello toocreate `SearchIndexClient` přímo tak, že abyste mohli předávat klíč dotazů místo klíče správce.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-158">However, in hello part of your application that executes queries, it is better toocreate hello `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="c6a6f-159">To je konzistentní s hello Princip nejnižších nutných oprávnění a pomůže toomake bezpečnější aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-159">This is consistent with hello principle of least privilege and will help toomake your application more secure.</span></span> <span data-ttu-id="c6a6f-160">Můžete najít další informace o klíčích a správce dotazu [zde](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-160">You can find out more about admin keys and query keys [here](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span></span>
> 
> 

<span data-ttu-id="c6a6f-161">Teď, když máme `SearchIndexClient`, naplníme hello index.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-161">Now that we have a `SearchIndexClient`, we can populate hello index.</span></span> <span data-ttu-id="c6a6f-162">K tomu je potřeba další metodou, vám ukážeme později.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-162">This is done by another method that we will walk through later.</span></span>

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

<span data-ttu-id="c6a6f-163">Nakonec provést několik vyhledávací dotazy a zobrazit výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-163">Finally, we execute a few search queries and display hello results.</span></span> <span data-ttu-id="c6a6f-164">Tentokrát použijeme jiné `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-164">This time we use a different `SearchIndexClient`:</span></span>

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

<span data-ttu-id="c6a6f-165">Jsme bude trvat bližší pohled na hello `RunQueries` metoda později.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-165">We will take a closer look at hello `RunQueries` method later.</span></span> <span data-ttu-id="c6a6f-166">Zde je hello kód toocreate hello nového `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-166">Here is hello code toocreate hello new `SearchIndexClient`:</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="c6a6f-167">Tentokrát použijeme klíč dotazů vzhledem k tomu, že jsme nepotřebují přístup pro zápis toohello index.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-167">This time we use a query key since we do not need write access toohello index.</span></span> <span data-ttu-id="c6a6f-168">Tyto informace můžete zadat v hello `appsettings.json` souboru hello [ukázkové aplikace](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-168">You can enter this information in hello `appsettings.json` file of hello [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

<span data-ttu-id="c6a6f-169">Pokud spustíte tuto aplikaci s platný název služby a klíče rozhraní API, hello výstup by měl vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-169">If you run this application with a valid service name and API keys, hello output should look like this:</span></span>

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

<span data-ttu-id="c6a6f-170">Úplný zdrojový kód Hello hello aplikace je k dispozici na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-170">hello full source code of hello application is provided at hello end of this article.</span></span>

<span data-ttu-id="c6a6f-171">V dalším kroku jsme bude trvat bližší pohled na každou z metod hello volá `Main`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-171">Next, we will take a closer look at each of hello methods called by `Main`.</span></span>

### <a name="creating-an-index"></a><span data-ttu-id="c6a6f-172">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="c6a6f-172">Creating an index</span></span>
<span data-ttu-id="c6a6f-173">Po vytvoření `SearchServiceClient`, další věc hello `Main` nemá je index odstranění hello "hotels", pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-173">After creating a `SearchServiceClient`, hello next thing `Main` does is delete hello "hotels" index if it already exists.</span></span> <span data-ttu-id="c6a6f-174">Která se provádí hello následující metodu:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-174">That is done by hello following method:</span></span>

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

<span data-ttu-id="c6a6f-175">Tato metoda používá hello zadané `SearchServiceClient` toocheck Pokud hello indexu existuje a pokud ano, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-175">This method uses hello given `SearchServiceClient` toocheck if hello index exists, and if so, delete it.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a6f-176">Hello ukázkový kód v tomto článku používá pro jednoduchost synchronní metody hello hello Azure Search .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-176">hello example code in this article uses hello synchronous methods of hello Azure Search .NET SDK for simplicity.</span></span> <span data-ttu-id="c6a6f-177">Doporučujeme použít hello asynchronních metod v tookeep vlastní aplikace je škálovatelné a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-177">We recommend that you use hello asynchronous methods in your own applications tookeep them scalable and responsive.</span></span> <span data-ttu-id="c6a6f-178">Například v hello metoda výše můžete použít `ExistsAsync` a `DeleteAsync` místo `Exists` a `Delete`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-178">For example, in hello method above you could use `ExistsAsync` and `DeleteAsync` instead of `Exists` and `Delete`.</span></span>
> 
> 

<span data-ttu-id="c6a6f-179">Dále `Main` vytvoří nový index "hotels" voláním této metody:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-179">Next, `Main` creates a new "hotels" index by calling this method:</span></span>

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

<span data-ttu-id="c6a6f-180">Tato metoda vytvoří novou `Index` objekt seznam `Field` objekty, které definuje schéma nového indexu hello hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-180">This method creates a new `Index` object with a list of `Field` objects that defines hello schema of hello new index.</span></span> <span data-ttu-id="c6a6f-181">Každé pole má název, datový typ a několika atributů, které definují chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-181">Each field has a name, data type, and several attributes that define its search behavior.</span></span> <span data-ttu-id="c6a6f-182">Hello `FieldBuilder` třída používá reflexe toocreate seznam `Field` objekty pro index hello prověřením hello veřejné vlastnosti a atributy hello zadané `Hotel` třída modelu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-182">hello `FieldBuilder` class uses reflection toocreate a list of `Field` objects for hello index by examining hello public properties and attributes of hello given `Hotel` model class.</span></span> <span data-ttu-id="c6a6f-183">Provedeme bližší pohled na hello `Hotel` později na třídu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-183">We'll take a closer look at hello `Hotel` class later on.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a6f-184">Vždy můžete vytvořit seznam hello `Field` objekty přímo místo použití `FieldBuilder` v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-184">You can always create hello list of `Field` objects directly instead of using `FieldBuilder` if needed.</span></span> <span data-ttu-id="c6a6f-185">Například chcete nemusí toouse třídu modelu, nebo může být nutné toouse existující třídy modelu, že nechcete, aby toomodify přidáním atributy.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-185">For example, you may not want toouse a model class or you may need toouse an existing model class that you don't want toomodify by adding attributes.</span></span>
>
> 

<span data-ttu-id="c6a6f-186">Kromě toho toofields, můžete také přidat vyhodnocování profily, trochu nebo toohello CORS možnosti indexu (tyto byly vynechány z ukázkové hello jako stručný výtah).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-186">In addition toofields, you can also add scoring profiles, suggesters, or CORS options toohello Index (these are omitted from hello sample for brevity).</span></span> <span data-ttu-id="c6a6f-187">Další informace o objektu hello Index a jeho složky můžete najít v hello [referenční informace sady SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), i jako v hello [referenční dokumentace rozhraní API REST Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-187">You can find more information about hello Index object and its constituent parts in hello [SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), as well as in hello [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

### <a name="populating-hello-index"></a><span data-ttu-id="c6a6f-188">Naplňování indexu hello</span><span class="sxs-lookup"><span data-stu-id="c6a6f-188">Populating hello index</span></span>
<span data-ttu-id="c6a6f-189">Hello další krok v `Main` je toopopulate hello nově vytvořený index.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-189">hello next step in `Main` is toopopulate hello newly-created index.</span></span> <span data-ttu-id="c6a6f-190">To se provádí v hello následující metodu:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-190">This is done in hello following method:</span></span>

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

<span data-ttu-id="c6a6f-191">Tato metoda obsahuje čtyři části.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-191">This method has four parts.</span></span> <span data-ttu-id="c6a6f-192">Hello nejprve vytvoří pole `Hotel` objekty, které bude sloužit jako index toohello tooupload vstupní data.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-192">hello first creates an array of `Hotel` objects that will serve as our input data tooupload toohello index.</span></span> <span data-ttu-id="c6a6f-193">Tato data jsou pevně pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-193">This data is hard-coded for simplicity.</span></span> <span data-ttu-id="c6a6f-194">Ve vaší vlastní aplikaci bude vaše data pravděpodobně pocházet z externího zdroje dat například do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-194">In your own application, your data will likely come from an external data source such as a SQL database.</span></span>

<span data-ttu-id="c6a6f-195">vytvoří druhou částí Hello `IndexBatch` obsahující dokumenty hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-195">hello second part creates an `IndexBatch` containing hello documents.</span></span> <span data-ttu-id="c6a6f-196">Zadejte chcete tooapply toohello batch v době hello vytvoříte, v takovém případě voláním operace hello `IndexBatch.Upload`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-196">You specify hello operation you want tooapply toohello batch at hello time you create it, in this case by calling `IndexBatch.Upload`.</span></span> <span data-ttu-id="c6a6f-197">Hello batch je pak nahrané toohello Azure Search indexování hello `Documents.Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-197">hello batch is then uploaded toohello Azure Search index by hello `Documents.Index` method.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a6f-198">V tomto příkladu jsme právě nahráváte dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-198">In this example, we are just uploading documents.</span></span> <span data-ttu-id="c6a6f-199">Pokud byste chtěli toomerge mění ve stávajících dokumentech nebo odstranění dokumentů, dávky můžete vytvořit voláním `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, nebo `IndexBatch.Delete` místo.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-199">If you wanted toomerge changes into existing documents or delete documents, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete` instead.</span></span> <span data-ttu-id="c6a6f-200">Také je možné kombinovat různé operace v jedné dávkové voláním `IndexBatch.New`, která vezme kolekci `IndexAction` objekty, z nichž každý informuje Azure Search tooperform konkrétní operace v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-200">You can also mix different operations in a single batch by calling `IndexBatch.New`, which takes a collection of `IndexAction` objects, each of which tells Azure Search tooperform a particular operation on a document.</span></span> <span data-ttu-id="c6a6f-201">Můžete vytvořit každý `IndexAction` s vlastní operaci voláním hello odpovídající metoda `IndexAction.Merge`, `IndexAction.Upload`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-201">You can create each `IndexAction` with its own operation by calling hello corresponding method such as `IndexAction.Merge`, `IndexAction.Upload`, and so on.</span></span>
> 
> 

<span data-ttu-id="c6a6f-202">Hello třetí součást tato metoda je blok catch, která zpracovává s případem důležitých chyb pro indexování.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-202">hello third part of this method is a catch block that handles an important error case for indexing.</span></span> <span data-ttu-id="c6a6f-203">Pokud služba Azure Search selže tooindex některé hello dokumentů v dávce hello `IndexBatchException` vyvolá výjimku `Documents.Index`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-203">If your Azure Search service fails tooindex some of hello documents in hello batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="c6a6f-204">To může nastat v případě indexování dokumentů, zatímco je služba velmi zatížená.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-204">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="c6a6f-205">**Důrazně doporučujeme v kódu explicitně zpracovávat tento případ.**</span><span class="sxs-lookup"><span data-stu-id="c6a6f-205">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="c6a6f-206">Můžete odložit a poté opakujte indexování hello dokumentů, které selhaly, nebo můžete protokolu a pokračovat jako hello znovu, nebo to můžete provést něco jiného v závislosti na požadavcích konzistence dat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-206">You can delay and then retry indexing hello documents that failed, or you can log and continue like hello sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a6f-207">Můžete použít hello `FindFailedActionsToRetry` tooconstruct metoda nové dávky obsahující pouze hello akce, které se nezdařila v předchozích volání příliš`Index`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-207">You can use hello `FindFailedActionsToRetry` method tooconstruct a new batch containing only hello actions that failed in a previous call too`Index`.</span></span> <span data-ttu-id="c6a6f-208">Metoda Hello je popsána [sem](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) a není diskuzi o tom, jak tooproperly použít [na StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-208">hello method is documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) and there is a discussion of how tooproperly use it [on StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span></span>
>
>

<span data-ttu-id="c6a6f-209">Nakonec hello `UploadDocuments` na 2 sekundy odloží metoda.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-209">Finally, hello `UploadDocuments` method delays for two seconds.</span></span> <span data-ttu-id="c6a6f-210">Indexování probíhá asynchronně služby Azure Search, takže ukázková aplikace hello musí toowait krátkou dobu tooensure, že jsou k dispozici pro vyhledávání dokumentů hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-210">Indexing happens asynchronously in your Azure Search service, so hello sample application needs toowait a short time tooensure that hello documents are available for searching.</span></span> <span data-ttu-id="c6a6f-211">Tato odložení se obvykle používají pouze v ukázkových aplikacích a při testech.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-211">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

#### <a name="how-hello-net-sdk-handles-documents"></a><span data-ttu-id="c6a6f-212">Jak hello .NET SDK zpracovává dokumenty</span><span class="sxs-lookup"><span data-stu-id="c6a6f-212">How hello .NET SDK handles documents</span></span>
<span data-ttu-id="c6a6f-213">Asi vás zajímá, jak hello Azure Search .NET SDK je možné tooupload instance uživatelsky definované třídy, jako je `Hotel` toohello index.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-213">You may be wondering how hello Azure Search .NET SDK is able tooupload instances of a user-defined class like `Hotel` toohello index.</span></span> <span data-ttu-id="c6a6f-214">toohelp odpověď na tuto otázku, podíváme se na hello `Hotel` třídy:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-214">toohelp answer that question, let's look at hello `Hotel` class:</span></span>

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

<span data-ttu-id="c6a6f-215">Hello nejprve thing toonotice je, že každá veřejná vlastnost třídy `Hotel` odpovídá tooa pole v definici indexu hello, ale s jedním zásadním rozdílem: název hello každého pole začíná malým písmenem ("camelCase"), zatímco název každé veřejné hello Vlastnost `Hotel` začíná velké písmeno ("pascalcase").</span><span class="sxs-lookup"><span data-stu-id="c6a6f-215">hello first thing toonotice is that each public property of `Hotel` corresponds tooa field in hello index definition, but with one crucial difference: hello name of each field starts with a lower-case letter ("camel case"), while hello name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="c6a6f-216">Toto je běžný scénář v .NET aplikacích provádějících datové vazby, kde je hello cílové schéma mimo hello kontrolu nad vývojář aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-216">This is a common scenario in .NET applications that perform data-binding where hello target schema is outside hello control of hello application developer.</span></span> <span data-ttu-id="c6a6f-217">Místo tooviolate hello směrnic pojmenování .NET ve vlastnosti názvy camelCase toho se dá zjistit hello SDK toomap hello vlastnost názvy toocamel případ automaticky s hello `[SerializePropertyNamesAsCamelCase]` atribut.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-217">Rather than having tooviolate hello .NET naming guidelines by making property names camel-case, you can tell hello SDK toomap hello property names toocamel-case automatically with hello `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a6f-218">Hello .NET SDK služby Azure Search používá hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) tooserialize knihovny a deserializaci vaše vlastní model tooand objekty z formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-218">hello Azure Search .NET SDK uses hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library tooserialize and deserialize your custom model objects tooand from JSON.</span></span> <span data-ttu-id="c6a6f-219">V případě potřeby lze serializaci přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-219">You can customize this serialization if needed.</span></span> <span data-ttu-id="c6a6f-220">Další podrobnosti najdete v tématu [vlastní serializace s JSON.NET](#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-220">For more details, see [Custom Serialization with JSON.NET](#JsonDotNet).</span></span>
> 
> 

<span data-ttu-id="c6a6f-221">Hello druhý toonotice co jsou hello atributy, jako `IsFilterable`, `IsSearchable`, `Key`, a `Analyzer` , uspořádání každé veřejné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-221">hello second thing toonotice are hello attributes such as `IsFilterable`, `IsSearchable`, `Key`, and `Analyzer` that decorate each public property.</span></span> <span data-ttu-id="c6a6f-222">Tyto atributy přímo propojit toohello [odpovídající atributy indexu Azure Search hello](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-222">These attributes map directly toohello [corresponding attributes of hello Azure Search index](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span></span> <span data-ttu-id="c6a6f-223">Hello `FieldBuilder` třída používá tyto definice pole tooconstruct pro hello index.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-223">hello `FieldBuilder` class uses these tooconstruct field definitions for hello index.</span></span>

<span data-ttu-id="c6a6f-224">Hello třetí důležité hello `Hotel` třídy jsou hello datové typy veřejných vlastností hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-224">hello third important thing about hello `Hotel` class are hello data types of hello public properties.</span></span> <span data-ttu-id="c6a6f-225">Hello .NET typy těchto vlastností mapy tootheir odpovídající typy polí v definici indexu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-225">hello .NET types of  these properties map tootheir equivalent field types in hello index definition.</span></span> <span data-ttu-id="c6a6f-226">Například hello `Category` vlastnosti řetězce mapuje toohello `category` pole, které je typu `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-226">For example, hello `Category` string property maps toohello `category` field, which is of type `Edm.String`.</span></span> <span data-ttu-id="c6a6f-227">Jsou podobná mapování typu mezi `bool?` a `Edm.Boolean`, `DateTimeOffset?` a `Edm.DateTimeOffset`, atd. hello konkrétní pravidla pro hello mapování typu jsou popsaná u hello `Documents.Get` metoda v hello [Azure Search .NET SDK referenční dokumentace](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-227">There are similar type mappings between `bool?` and `Edm.Boolean`, `DateTimeOffset?` and `Edm.DateTimeOffset`, etc. hello specific rules for hello type mapping are documented with hello `Documents.Get` method in hello [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span> <span data-ttu-id="c6a6f-228">Hello `FieldBuilder` třída má na starosti toto mapování pro vás, ale stále může být užitečné toounderstand v případě, že potřebujete tootroubleshoot problémy serializace.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-228">hello `FieldBuilder` class takes care of this mapping for you, but it can still be helpful toounderstand in case you need tootroubleshoot any serialization issues.</span></span>

<span data-ttu-id="c6a6f-229">Tato možnost toouse vaše vlastní třídy jako dokumenty funguje v obou směrech; Můžete také načíst výsledky vyhledávání a nechat hello SDK automaticky deserializovala tooa typu, protože jsme se zobrazí v další části hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-229">This ability toouse your own classes as documents works in both directions; You can also retrieve search results and have hello SDK automatically deserialize them tooa type of your choice, as we will see in hello next section.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a6f-230">Hello .NET SDK služby Azure Search také podporuje dynamicky typované dokumenty pomocí hello `Document` třída, která zajišťuje mapování klíč hodnota hodnot toofield názvy polí.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-230">hello Azure Search .NET SDK also supports dynamically-typed documents using hello `Document` class, which is a key/value mapping of field names toofield values.</span></span> <span data-ttu-id="c6a6f-231">To je užitečné v případech, pokud neznáte schéma indexu hello v době návrhu, nebo pokud je třídy modelu toospecific nepohodlná toobind.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-231">This is useful in scenarios where you don't know hello index schema at design-time, or where it would be inconvenient toobind toospecific model classes.</span></span> <span data-ttu-id="c6a6f-232">Všechny metody hello v hello SDK, které pracují s dokumenty, mají přetížení, které pracují s hello `Document` třídy, jakož i přetížení silně typované, která přebírají parametr obecného typu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-232">All hello methods in hello SDK that deal with documents have overloads that work with hello `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="c6a6f-233">Pouze hello pozdější se používají v hello ukázkový kód v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-233">Only hello latter are used in hello sample code in this tutorial.</span></span> <span data-ttu-id="c6a6f-234">Hello `Document` třída dědí z `Dictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-234">hello `Document` class inherits from `Dictionary<string, object>`.</span></span> <span data-ttu-id="c6a6f-235">Můžete najít další podrobnosti o [zde](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-235">You can find other details [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span></span>
> 
> 

<span data-ttu-id="c6a6f-236">**Proč byste měli používat datové typy s možnou hodnotou null**</span><span class="sxs-lookup"><span data-stu-id="c6a6f-236">**Why you should use nullable data types**</span></span>

<span data-ttu-id="c6a6f-237">Při navrhování indexu Azure Search toomap tooan vlastní model třídy, doporučujeme deklarovat vlastnosti typů hodnot, jako `bool` a `int` toobe s možnou hodnotou Null (například `bool?` místo `bool`).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-237">When designing your own model classes toomap tooan Azure Search index, we recommend declaring properties of value types such as `bool` and `int` toobe nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="c6a6f-238">Pokud použijete vlastnost se zakázanou hodnotou Null, máte příliš**zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hello hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-238">If you use a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="c6a6f-239">Hello SDK ani hello služby Azure Search vám pomůže tooenforce vám to.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-239">Neither hello SDK nor hello Azure Search service will help you tooenforce this.</span></span>

<span data-ttu-id="c6a6f-240">Nejedná se pouze o hypotetický problém: Představte si situaci, kdy přidáte nový pole tooan existující index typu `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-240">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="c6a6f-241">Po aktualizaci definice indexu hello, budou všechny dokumenty mít pro toto nové pole hodnotu null (protože jsou všechny typy s možnou hodnotou Null ve službě Azure Search).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-241">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="c6a6f-242">Pokud pak použijete třídu modelu s hodnotou Null `int` vlastnosti pro toto pole, zobrazí se `JsonSerializationException` při pokusu o tooretrieve dokumenty podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-242">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="c6a6f-243">Z tohoto důvodu doporučujeme jako osvědčený postup používat ve třídách modelu typy s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-243">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a><span data-ttu-id="c6a6f-244">Vlastní serializace s JSON.NET</span><span class="sxs-lookup"><span data-stu-id="c6a6f-244">Custom Serialization with JSON.NET</span></span>
<span data-ttu-id="c6a6f-245">Hello SDK používá technologii JSON.NET pro serializaci a deserializaci dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-245">hello SDK uses JSON.NET for serializing and deserializing documents.</span></span> <span data-ttu-id="c6a6f-246">Můžete přizpůsobit serializace a deserializace v případě potřeby tak, že definujete vlastní `JsonConverter` nebo `IContractResolver` (viz hello [JSON.NET dokumentace](http://www.newtonsoft.com/json/help/html/Introduction.htm) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-246">You can customize serialization and deserialization if needed by defining your own `JsonConverter` or `IContractResolver` (see hello [JSON.NET documentation](http://www.newtonsoft.com/json/help/html/Introduction.htm) for more details).</span></span> <span data-ttu-id="c6a6f-247">To může být užitečné, pokud chcete tooadapt existující třídy modelu z vaší aplikace pro použití se službou Azure Search a jiné pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-247">This can be useful when you want tooadapt an existing model class from your application for use with Azure Search, and other more advanced scenarios.</span></span> <span data-ttu-id="c6a6f-248">S vlastní serializace můžete například:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-248">For example, with custom serialization you can:</span></span>

* <span data-ttu-id="c6a6f-249">Zahrnout nebo vyloučit některé vlastnosti vaší třídy modelu z uložené jako pole dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-249">Include or exclude certain properties of your model class from being stored as document fields.</span></span>
* <span data-ttu-id="c6a6f-250">Mapování mezi názvy vlastností v kódu a názvy polí v indexu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-250">Map between property names in your code and field names in your index.</span></span>
* <span data-ttu-id="c6a6f-251">Vytvoření vlastních atributů, které lze použít pro mapování polí toodocument vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-251">Create custom attributes that can be used for mapping properties toodocument fields.</span></span>

<span data-ttu-id="c6a6f-252">Můžete najít příklady implementace vlastní serializace v hello testy částí pro hello Azure Search .NET SDK na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-252">You can find examples of implementing custom serialization in hello unit tests for hello Azure Search .NET SDK on GitHub.</span></span> <span data-ttu-id="c6a6f-253">Je to dobrý výchozí bod [tato složka](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-253">A good starting point is [this folder](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span></span> <span data-ttu-id="c6a6f-254">Obsahuje třídy, které jsou používány hello vlastní serializace testy.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-254">It contains classes that are used by hello custom serialization tests.</span></span>

### <a name="searching-for-documents-in-hello-index"></a><span data-ttu-id="c6a6f-255">Hledání dokumentů v indexu hello</span><span class="sxs-lookup"><span data-stu-id="c6a6f-255">Searching for documents in hello index</span></span>
<span data-ttu-id="c6a6f-256">posledním krokem Hello hello ukázkové aplikace je toosearch u některých dokumentů v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-256">hello last step in hello sample application is toosearch for some documents in hello index.</span></span> <span data-ttu-id="c6a6f-257">Následující metoda Hello provede toto:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-257">hello following method does this:</span></span>

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

<span data-ttu-id="c6a6f-258">Pokaždé, když se provede dotaz, tato metoda první vytvoří novou `SearchParameters` objektu.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-258">Each time it executes a query, this method first creates a new `SearchParameters` object.</span></span> <span data-ttu-id="c6a6f-259">Toto je použité toospecify další možnosti pro dotaz hello například třídění, filtrování, stránkování a používání omezujících vlastností.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-259">This is used toospecify additional options for hello query such as sorting, filtering, paging, and faceting.</span></span> <span data-ttu-id="c6a6f-260">Tato metoda jsme se nastavení hello `Filter`, `Select`, `OrderBy`, a `Top` vlastnost pro různé dotazy.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-260">In this method, we're setting hello `Filter`, `Select`, `OrderBy`, and `Top` property for different queries.</span></span> <span data-ttu-id="c6a6f-261">Všechny hello `SearchParameters` jsou popsané vlastnosti [zde](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-261">All hello `SearchParameters` properties are documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span></span>

<span data-ttu-id="c6a6f-262">dalším krokem Hello je tooactually provést hello vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-262">hello next step is tooactually execute hello search query.</span></span> <span data-ttu-id="c6a6f-263">To se provádí pomocí hello `Documents.Search` metoda.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-263">This is done using hello `Documents.Search` method.</span></span> <span data-ttu-id="c6a6f-264">Pro každý dotaz jsme předat hello hledání textu toouse jako řetězec (nebo `"*"` Pokud neexistuje žádný hledaný text), plus hello vyhledávání parametry vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-264">For each query, we pass hello search text toouse as a string (or `"*"` if there is no search text), plus hello search parameters created earlier.</span></span> <span data-ttu-id="c6a6f-265">Můžeme také určit `Hotel` jako parametr typu hello pro `Documents.Search`, která sděluje hello SDK toodeserialize dokumenty ve výsledcích hledání hello do objektů typu `Hotel`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-265">We also specify `Hotel` as hello type parameter for `Documents.Search`, which tells hello SDK toodeserialize documents in hello search results into objects of type `Hotel`.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a6f-266">Můžete najít další informace o syntaxe výrazu dotazu vyhledávání hello [zde](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-266">You can find more information about hello search query expression syntax [here](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span></span>
> 
> 

<span data-ttu-id="c6a6f-267">Nakonec po každém dotazu tato metoda iteruje všechny hello shody v hello výsledky hledání, tisk každé konzole toohello dokumentu:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-267">Finally, after each query this method iterates through all hello matches in hello search results, printing each document toohello console:</span></span>

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

<span data-ttu-id="c6a6f-268">Podívejme zase bližší pohled na každý dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-268">Let's take a closer look at each of hello queries in turn.</span></span> <span data-ttu-id="c6a6f-269">Tady je hello kód tooexecute hello první dotaz:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-269">Here is hello code tooexecute hello first query:</span></span>

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

<span data-ttu-id="c6a6f-270">V takovém případě jsme se hledající hotely, které odpovídají hello slovo "rozpočet", a chceme tooget zpět pouze hello hotelů názvy, podle specifikace hello `Select` parametr.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-270">In this case, we're searching for hotels that match hello word "budget", and we want tooget back only hello hotel names, as specified by hello `Select` parameter.</span></span> <span data-ttu-id="c6a6f-271">Zde jsou výsledky hello:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-271">Here are hello results:</span></span>

    Name: Roach Motel

<span data-ttu-id="c6a6f-272">V dalším kroku jsme má toofind hello hotels s noční počet menší než 150 USD a vrátí pouze hello hotelů ID a popis:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-272">Next, we want toofind hello hotels with a nightly rate of less than $150, and return only hello hotel ID and description:</span></span>

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

<span data-ttu-id="c6a6f-273">Tento dotaz používá OData `$filter` výrazu `baseRate lt 150`, toofilter hello dokumenty v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-273">This query uses an OData `$filter` expression, `baseRate lt 150`, toofilter hello documents in hello index.</span></span> <span data-ttu-id="c6a6f-274">Můžete najít další informace o hello syntaxe OData, který podporuje Azure Search [zde](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-274">You can find out more about hello OData syntax that Azure Search supports [here](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span></span>

<span data-ttu-id="c6a6f-275">Zde jsou hello výsledky dotazu hello:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-275">Here are hello results of hello query:</span></span>

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close tootown hall and hello river

<span data-ttu-id="c6a6f-276">V dalším kroku chceme toofind hello nejvyšší dvě hotels které mají byl naposledy renovovanou a zobrazit název hotelů hello a datum posledního obnova.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-276">Next, we want toofind hello top two hotels that have been most recently renovated, and show hello hotel name and last renovation date.</span></span> <span data-ttu-id="c6a6f-277">Tady je kód hello:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-277">Here is hello code:</span></span> 

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

<span data-ttu-id="c6a6f-278">V takovém případě znovu používáme OData syntaxe toospecify hello `OrderBy` parametr jako `lastRenovationDate desc`.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-278">In this case, we again use OData syntax toospecify hello `OrderBy` parameter as `lastRenovationDate desc`.</span></span> <span data-ttu-id="c6a6f-279">Můžeme také nastavit `Top` too2 tooensure se nám pouze získat hello horních dvou dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-279">We also set `Top` too2 tooensure we only get hello top two documents.</span></span> <span data-ttu-id="c6a6f-280">Před, jsme nastavit jako `Select` toospecify pole, která má být vrácen.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-280">As before, we set `Select` toospecify which fields should be returned.</span></span>

<span data-ttu-id="c6a6f-281">Zde jsou výsledky hello:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-281">Here are hello results:</span></span>

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

<span data-ttu-id="c6a6f-282">Nakonec chcete toofind všechny hotels, které odpovídají hello slovo "motel":</span><span class="sxs-lookup"><span data-stu-id="c6a6f-282">Finally, we want toofind all hotels that match hello word "motel":</span></span>

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

<span data-ttu-id="c6a6f-283">A tady jsou hello výsledky, které zahrnují všechna pole, protože jsme nezadali hello `Select` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c6a6f-283">And here are hello results, which include all fields since we did not specify hello `Select` property:</span></span>

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

<span data-ttu-id="c6a6f-284">Dokončení tohoto kroku kurzu hello, ale není tady zastavit.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-284">This step completes hello tutorial, but don't stop here.</span></span> <span data-ttu-id="c6a6f-285">**Další kroky** poskytuje další zdroje dalších informací o službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-285">**Next steps** provides additional resources for learning more about Azure Search.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6a6f-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6a6f-286">Next steps</span></span>
* <span data-ttu-id="c6a6f-287">Procházet hello odkazy pro hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) a [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-287">Browse hello references for hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
* <span data-ttu-id="c6a6f-288">Prohloubit vašich znalostí prostřednictvím [videa a jiné ukázky a výukové programy](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="c6a6f-288">Deepen your knowledge through [videos and other samples and tutorials](search-video-demo-tutorial-list.md).</span></span>
* <span data-ttu-id="c6a6f-289">Zkontrolujte [konvence vytváření názvů](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) toolearn hello pravidla pro pojmenovávání různé objekty.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-289">Review [naming conventions](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) toolearn hello rules for naming various objects.</span></span>
* <span data-ttu-id="c6a6f-290">Zkontrolujte [podporované datové typy](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c6a6f-290">Review [supported data types](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.</span></span>
