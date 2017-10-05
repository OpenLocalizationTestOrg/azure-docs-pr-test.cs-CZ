---
title: "Jak používat Azure Search z aplikace .NET | Microsoft Docs"
description: "Jak používat Azure Search z aplikace .NET"
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
ms.openlocfilehash: 552a7ab193e12d2e72da494166d774e974c85d47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-search-from-a-net-application"></a><span data-ttu-id="1d392-103">Jak používat Azure Search z aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="1d392-103">How to use Azure Search from a .NET Application</span></span>
<span data-ttu-id="1d392-104">Tento článek je návod, které vám pomůžou s spuštěná [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="1d392-104">This article is a walkthrough to get you up and running with the [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span> <span data-ttu-id="1d392-105">Můžete implementovat bohaté vyhledáváním do vaší aplikace pomocí Azure Search .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="1d392-105">You can use the .NET SDK to implement a rich search experience in your application using Azure Search.</span></span>

## <a name="whats-in-the-azure-search-sdk"></a><span data-ttu-id="1d392-106">Novinky ve službě Azure vyhledávání SDK</span><span class="sxs-lookup"><span data-stu-id="1d392-106">What's in the Azure Search SDK</span></span>
<span data-ttu-id="1d392-107">Sada SDK se skládá z Klientská knihovna `Microsoft.Azure.Search`.</span><span class="sxs-lookup"><span data-stu-id="1d392-107">The SDK consists of a client library, `Microsoft.Azure.Search`.</span></span> <span data-ttu-id="1d392-108">Umožňuje spravovat indexy, indexery a zdroje dat a také nahrát a správě dokumentů a spouštět dotazy, aniž by museli řešit podrobnosti protokolu HTTP a JSON.</span><span class="sxs-lookup"><span data-stu-id="1d392-108">It enables you to manage your indexes, data sources, and indexers, as well as upload and manage documents, and execute queries, all without having to deal with the details of HTTP and JSON.</span></span>

<span data-ttu-id="1d392-109">Definuje klientské knihovny tříd jako `Index`, `Field`, a `Document`, stejně jako operací, jako `Indexes.Create` a `Documents.Search` na `SearchServiceClient` a `SearchIndexClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="1d392-109">The client library defines classes like `Index`, `Field`, and `Document`, as well as operations like `Indexes.Create` and `Documents.Search` on the `SearchServiceClient` and `SearchIndexClient` classes.</span></span> <span data-ttu-id="1d392-110">Tyto třídy jsou uspořádány do následujících oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="1d392-110">These classes are organized into the following namespaces:</span></span>

* [<span data-ttu-id="1d392-111">Microsoft.Azure.Search</span><span class="sxs-lookup"><span data-stu-id="1d392-111">Microsoft.Azure.Search</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [<span data-ttu-id="1d392-112">Microsoft.Azure.Search.Models</span><span class="sxs-lookup"><span data-stu-id="1d392-112">Microsoft.Azure.Search.Models</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

<span data-ttu-id="1d392-113">Aktuální verze rozhraní .NET SDK služby Azure Search je nyní všeobecně dostupná.</span><span class="sxs-lookup"><span data-stu-id="1d392-113">The current version of the Azure Search .NET SDK is now generally available.</span></span> <span data-ttu-id="1d392-114">Pokud chcete poskytnout zpětnou vazbu, abychom mohli začlenit v příští verzi, navštivte prosím naše [zpětné vazby stránky](https://feedback.azure.com/forums/263029-azure-search/).</span><span class="sxs-lookup"><span data-stu-id="1d392-114">If you would like to provide feedback for us to incorporate in the next version, please visit our [feedback page](https://feedback.azure.com/forums/263029-azure-search/).</span></span>

<span data-ttu-id="1d392-115">.NET SDK podporuje verzi `2016-09-01` z [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="1d392-115">The .NET SDK supports version `2016-09-01` of the [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span> <span data-ttu-id="1d392-116">Tato verze teď zahrnuje podporu pro vlastní analyzátorů a objektů Blob v Azure a Azure Table podpora indexeru.</span><span class="sxs-lookup"><span data-stu-id="1d392-116">This version now includes support for custom analyzers and Azure Blob and Azure Table indexer support.</span></span> <span data-ttu-id="1d392-117">Zobrazit náhled funkce, které jsou *není* jsou součástí této verze, jako je podpora pro indexování soubory JSON a sdíleného svazku clusteru v [preview](search-api-2015-02-28-preview.md) a dostupný prostřednictvím starší [verze 2.0 preview .NET SDK](https://aka.ms/search-sdk-preview).</span><span class="sxs-lookup"><span data-stu-id="1d392-117">Preview features that are *not* part of this version, such as support for indexing JSON and CSV files, are in [preview](search-api-2015-02-28-preview.md) and available via the older [2.0-preview version of the .NET SDK](https://aka.ms/search-sdk-preview).</span></span>

<span data-ttu-id="1d392-118">Tato sada SDK nepodporuje [operace správy](https://docs.microsoft.com/rest/api/searchmanagement/) jako je například vytváření a škálování služby vyhledávání a správa klíče rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d392-118">This SDK does not support [Management Operations](https://docs.microsoft.com/rest/api/searchmanagement/) such as creating and scaling Search services and managing API keys.</span></span> <span data-ttu-id="1d392-119">Pokud potřebujete ke správě prostředků vyhledávání z aplikace .NET, můžete použít [SDK služby Azure Search .NET správu](https://aka.ms/search-mgmt-sdk).</span><span class="sxs-lookup"><span data-stu-id="1d392-119">If you need to manage your Search resources from a .NET application, you can use the [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).</span></span>

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a><span data-ttu-id="1d392-120">Upgrade na nejnovější verzi sady SDK</span><span class="sxs-lookup"><span data-stu-id="1d392-120">Upgrading to the latest version of the SDK</span></span>
<span data-ttu-id="1d392-121">Pokud už používáte starší verzi .NET SDK služby Azure Search a chcete upgradovat na novou verzi všeobecně dostupná, [v tomto článku](search-dotnet-sdk-migration.md) vysvětluje, jak.</span><span class="sxs-lookup"><span data-stu-id="1d392-121">If you're already using an older version of the Azure Search .NET SDK and you'd like to upgrade to the new generally available version, [this article](search-dotnet-sdk-migration.md) explains how.</span></span>

## <a name="requirements-for-the-sdk"></a><span data-ttu-id="1d392-122">Požadavky pro sadu SDK</span><span class="sxs-lookup"><span data-stu-id="1d392-122">Requirements for the SDK</span></span>
1. <span data-ttu-id="1d392-123">Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1d392-123">Visual Studio 2017.</span></span>
2. <span data-ttu-id="1d392-124">Vlastní službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1d392-124">Your own Azure Search service.</span></span> <span data-ttu-id="1d392-125">Chcete-li použít sadu SDK, budete potřebovat název služby a jeden či více klíčů rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d392-125">In order to use the SDK, you will need the name of your service and one or more API keys.</span></span> <span data-ttu-id="1d392-126">[Vytvoření služby v portálu](search-create-service-portal.md) vám pomohou dokončit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="1d392-126">[Create a service in the portal](search-create-service-portal.md) will help you through these steps.</span></span>
3. <span data-ttu-id="1d392-127">Stáhněte si sadu Azure Search .NET SDK [balíček NuGet](http://www.nuget.org/packages/Microsoft.Azure.Search) pomocí "Správa balíčků NuGet" v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1d392-127">Download the Azure Search .NET SDK [NuGet package](http://www.nuget.org/packages/Microsoft.Azure.Search) by using "Manage NuGet Packages" in Visual Studio.</span></span> <span data-ttu-id="1d392-128">Právě vyhledejte název balíčku `Microsoft.Azure.Search` na NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="1d392-128">Just search for the package name `Microsoft.Azure.Search` on NuGet.org.</span></span>

<span data-ttu-id="1d392-129">.NET SDK služby Azure Search podporuje aplikace cílené na rozhraní .NET Framework 4.6 a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d392-129">The Azure Search .NET SDK supports applications targeting the .NET Framework 4.6 and .NET Core.</span></span>

## <a name="core-scenarios"></a><span data-ttu-id="1d392-130">Základní scénáře</span><span class="sxs-lookup"><span data-stu-id="1d392-130">Core scenarios</span></span>
<span data-ttu-id="1d392-131">Existuje několik věcí, které musíte udělat v aplikaci vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="1d392-131">There are several things you'll need to do in your search application.</span></span> <span data-ttu-id="1d392-132">V tomto kurzu nabídneme tyto základní scénáře:</span><span class="sxs-lookup"><span data-stu-id="1d392-132">In this tutorial, we'll cover these core scenarios:</span></span>

* <span data-ttu-id="1d392-133">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="1d392-133">Creating an index</span></span>
* <span data-ttu-id="1d392-134">Naplňování indexu s dokumenty</span><span class="sxs-lookup"><span data-stu-id="1d392-134">Populating the index with documents</span></span>
* <span data-ttu-id="1d392-135">Hledání dokumentů pomocí fulltextové vyhledávání a filtry</span><span class="sxs-lookup"><span data-stu-id="1d392-135">Searching for documents using full-text search and filters</span></span>

<span data-ttu-id="1d392-136">Ukázkový kód, který následuje znázorňuje všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="1d392-136">The sample code that follows illustrates each of these.</span></span> <span data-ttu-id="1d392-137">Nebojte se, že pomocí fragmenty kódu ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d392-137">Feel free to use the code snippets in your own application.</span></span>

### <a name="overview"></a><span data-ttu-id="1d392-138">Přehled</span><span class="sxs-lookup"><span data-stu-id="1d392-138">Overview</span></span>
<span data-ttu-id="1d392-139">Vytvoří novou ukázkovou aplikaci jsme budete seznamovat index s názvem "hotels", naplní s několika dokumenty a potom provede některé vyhledávací dotazy.</span><span class="sxs-lookup"><span data-stu-id="1d392-139">The sample application we'll be exploring creates a new index named "hotels", populates it with a few documents, then executes some search queries.</span></span> <span data-ttu-id="1d392-140">Tady je hlavní programu, zobrazující celkové toku:</span><span class="sxs-lookup"><span data-stu-id="1d392-140">Here is the main program, showing the overall flow:</span></span>

```csharp
// This sample shows how to delete, create, upload documents and query an index
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

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> <span data-ttu-id="1d392-141">Úplný zdrojový kód ukázkové aplikace použité v tomto názorném postupu najdete na [Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="1d392-141">You can find the full source code of the sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>
> 
>

<span data-ttu-id="1d392-142">Projdeme tento krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="1d392-142">We'll walk through this step by step.</span></span> <span data-ttu-id="1d392-143">Nejprve musíme vytvořit nový `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="1d392-143">First we need to create a new `SearchServiceClient`.</span></span> <span data-ttu-id="1d392-144">Tento objekt umožňuje spravovat indexy.</span><span class="sxs-lookup"><span data-stu-id="1d392-144">This object allows you to manage indexes.</span></span> <span data-ttu-id="1d392-145">Chcete-li jeden vytvořit, je třeba zadat název vaší služby Azure Search, jakož i klíčem rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="1d392-145">In order to construct one, you need to provide your Azure Search service name as well as an admin API key.</span></span> <span data-ttu-id="1d392-146">Můžete zadat tyto informace `appsettings.json` soubor [ukázkové aplikace](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="1d392-146">You can enter this information in the `appsettings.json` file of the [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

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
> <span data-ttu-id="1d392-147">Pokud jste zadali nesprávné klávesy (například klíč dotazů kde nebyla nutná klíč správce), `SearchServiceClient` vyvolá výjimku `CloudException` s chybou zpráva "Zakázán" poprvé, jako například volání metody operace, `Indexes.Create`.</span><span class="sxs-lookup"><span data-stu-id="1d392-147">If you provide an incorrect key (for example, a query key where an admin key was required), the `SearchServiceClient` will throw a `CloudException` with the error message "Forbidden" the first time you call an operation method on it, such as `Indexes.Create`.</span></span> <span data-ttu-id="1d392-148">V takovém případě vám zkontrolujte naše klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d392-148">If this happens to you, double-check our API key.</span></span>
> 
> 

<span data-ttu-id="1d392-149">Další několika řádků volat metody pro vytvoření indexu s názvem "hotels", nejprve odstraňování Pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="1d392-149">The next few lines call methods to create an index named "hotels", deleting it first if it already exists.</span></span> <span data-ttu-id="1d392-150">Jsme provede tyto metody trochu později.</span><span class="sxs-lookup"><span data-stu-id="1d392-150">We will walk through these methods a little later.</span></span>

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

<span data-ttu-id="1d392-151">V dalším kroku index musí být naplněny.</span><span class="sxs-lookup"><span data-stu-id="1d392-151">Next, the index needs to be populated.</span></span> <span data-ttu-id="1d392-152">Chcete-li to provést, je nutné zadat `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="1d392-152">To do this, we will need a `SearchIndexClient`.</span></span> <span data-ttu-id="1d392-153">Existují dva způsoby, jak získat jeden: vytváření ho nebo volání `Indexes.GetClient` na `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="1d392-153">There are two ways to obtain one: by constructing it, or by calling `Indexes.GetClient` on the `SearchServiceClient`.</span></span> <span data-ttu-id="1d392-154">Používáme k tomu ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="1d392-154">We use the latter for convenience.</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="1d392-155">V typické vyhledávací aplikaci se o správu a naplňování indexu stará samostatná komponenta volaná dotazy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="1d392-155">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="1d392-156">`Indexes.GetClient` je vhodné pro naplňování indexu, protože díky tomu není nutné poskytovat další `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="1d392-156">`Indexes.GetClient` is convenient for populating an index because it saves you the trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="1d392-157">Dělá to pomocí předání klíče správce, který jste použili pro vytvoření `SearchServiceClient`, službě `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="1d392-157">It does this by passing the admin key that you used to create the `SearchServiceClient` to the new `SearchIndexClient`.</span></span> <span data-ttu-id="1d392-158">V části aplikace, který provádí dotazy, je nicméně lepší vytvořit `SearchIndexClient` přímo, abyste mohli předávat klíč dotazů místo klíče správce.</span><span class="sxs-lookup"><span data-stu-id="1d392-158">However, in the part of your application that executes queries, it is better to create the `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="1d392-159">To je konzistentní s Princip nejnižších nutných oprávnění a pomůže vám to lépe zabezpečit vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d392-159">This is consistent with the principle of least privilege and will help to make your application more secure.</span></span> <span data-ttu-id="1d392-160">Můžete najít další informace o klíčích a správce dotazu [zde](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span><span class="sxs-lookup"><span data-stu-id="1d392-160">You can find out more about admin keys and query keys [here](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span></span>
> 
> 

<span data-ttu-id="1d392-161">Teď, když máme `SearchIndexClient`, naplníme index.</span><span class="sxs-lookup"><span data-stu-id="1d392-161">Now that we have a `SearchIndexClient`, we can populate the index.</span></span> <span data-ttu-id="1d392-162">K tomu je potřeba další metodou, vám ukážeme později.</span><span class="sxs-lookup"><span data-stu-id="1d392-162">This is done by another method that we will walk through later.</span></span>

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

<span data-ttu-id="1d392-163">Nakonec provést několik vyhledávací dotazy a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="1d392-163">Finally, we execute a few search queries and display the results.</span></span> <span data-ttu-id="1d392-164">Tentokrát použijeme jiné `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="1d392-164">This time we use a different `SearchIndexClient`:</span></span>

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

<span data-ttu-id="1d392-165">Jsme bude trvat bližší pohled na `RunQueries` metoda později.</span><span class="sxs-lookup"><span data-stu-id="1d392-165">We will take a closer look at the `RunQueries` method later.</span></span> <span data-ttu-id="1d392-166">Zde je kód k vytvoření nové `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="1d392-166">Here is the code to create the new `SearchIndexClient`:</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="1d392-167">Tentokrát použijeme klíč dotazů vzhledem k tomu, že jsme není nutné oprávnění k zápisu do indexu.</span><span class="sxs-lookup"><span data-stu-id="1d392-167">This time we use a query key since we do not need write access to the index.</span></span> <span data-ttu-id="1d392-168">Můžete zadat tyto informace `appsettings.json` soubor [ukázkové aplikace](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="1d392-168">You can enter this information in the `appsettings.json` file of the [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

<span data-ttu-id="1d392-169">Pokud spustíte tuto aplikaci s platný název služby a klíče rozhraní API, výstup by měl vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="1d392-169">If you run this application with a valid service name and API keys, the output should look like this:</span></span>

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents to be indexed...
    
    Search the entire index for the term 'budget' and return only the hotelName field:
    
    Name: Roach Motel
    
    Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:
    
    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river
    
    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:
    
    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00
    
    Search the entire index for the term 'motel':
    
    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
    
    Complete.  Press any key to end application...

<span data-ttu-id="1d392-170">Úplný zdrojový kód aplikace je k dispozici na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1d392-170">The full source code of the application is provided at the end of this article.</span></span>

<span data-ttu-id="1d392-171">V dalším kroku jsme bude trvat bližší pohled na každou z metod volá `Main`.</span><span class="sxs-lookup"><span data-stu-id="1d392-171">Next, we will take a closer look at each of the methods called by `Main`.</span></span>

### <a name="creating-an-index"></a><span data-ttu-id="1d392-172">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="1d392-172">Creating an index</span></span>
<span data-ttu-id="1d392-173">Po vytvoření `SearchServiceClient`, je další věcí `Main` nemá se odstranit index "hotels", pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="1d392-173">After creating a `SearchServiceClient`, the next thing `Main` does is delete the "hotels" index if it already exists.</span></span> <span data-ttu-id="1d392-174">Která se provádí následující metodu:</span><span class="sxs-lookup"><span data-stu-id="1d392-174">That is done by the following method:</span></span>

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

<span data-ttu-id="1d392-175">Tato metoda používá v dané `SearchServiceClient` zkontrolujte, zda existuje index a pokud ano, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="1d392-175">This method uses the given `SearchServiceClient` to check if the index exists, and if so, delete it.</span></span>

> [!NOTE]
> <span data-ttu-id="1d392-176">Příklad kódu v tomto článku používá pro jednoduchost synchronní metody sady Azure Search .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="1d392-176">The example code in this article uses the synchronous methods of the Azure Search .NET SDK for simplicity.</span></span> <span data-ttu-id="1d392-177">Doporučujeme ve vlastních aplikacích použít asynchronní metody, aby aplikace byly škálovatelné a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="1d392-177">We recommend that you use the asynchronous methods in your own applications to keep them scalable and responsive.</span></span> <span data-ttu-id="1d392-178">Například v metodě výše můžete použít `ExistsAsync` a `DeleteAsync` místo `Exists` a `Delete`.</span><span class="sxs-lookup"><span data-stu-id="1d392-178">For example, in the method above you could use `ExistsAsync` and `DeleteAsync` instead of `Exists` and `Delete`.</span></span>
> 
> 

<span data-ttu-id="1d392-179">Dále `Main` vytvoří nový index "hotels" voláním této metody:</span><span class="sxs-lookup"><span data-stu-id="1d392-179">Next, `Main` creates a new "hotels" index by calling this method:</span></span>

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

<span data-ttu-id="1d392-180">Tato metoda vytvoří novou `Index` objekt seznam `Field` objekty, které definuje schéma nového indexu.</span><span class="sxs-lookup"><span data-stu-id="1d392-180">This method creates a new `Index` object with a list of `Field` objects that defines the schema of the new index.</span></span> <span data-ttu-id="1d392-181">Každé pole má název, datový typ a několika atributů, které definují chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="1d392-181">Each field has a name, data type, and several attributes that define its search behavior.</span></span> <span data-ttu-id="1d392-182">`FieldBuilder` Třídy pomocí reflexe vytvoří seznam `Field` objekty pro index prověřením veřejné vlastnosti a atributů v dané `Hotel` třída modelu.</span><span class="sxs-lookup"><span data-stu-id="1d392-182">The `FieldBuilder` class uses reflection to create a list of `Field` objects for the index by examining the public properties and attributes of the given `Hotel` model class.</span></span> <span data-ttu-id="1d392-183">Provedeme bližší pohled na `Hotel` později na třídu.</span><span class="sxs-lookup"><span data-stu-id="1d392-183">We'll take a closer look at the `Hotel` class later on.</span></span>

> [!NOTE]
> <span data-ttu-id="1d392-184">Vždy můžete vytvořit seznam `Field` objekty přímo místo použití `FieldBuilder` v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="1d392-184">You can always create the list of `Field` objects directly instead of using `FieldBuilder` if needed.</span></span> <span data-ttu-id="1d392-185">Například nemusíte chtít použijete třídu modelu, nebo budete muset použít existující třídu modelu, který nechcete upravit přidáním atributy.</span><span class="sxs-lookup"><span data-stu-id="1d392-185">For example, you may not want to use a model class or you may need to use an existing model class that you don't want to modify by adding attributes.</span></span>
>
> 

<span data-ttu-id="1d392-186">Kromě polí můžete také přidat vyhodnocování profily, trochu nebo CORS možnosti indexu (tyto byly vynechány z ukázky jako stručný výtah).</span><span class="sxs-lookup"><span data-stu-id="1d392-186">In addition to fields, you can also add scoring profiles, suggesters, or CORS options to the Index (these are omitted from the sample for brevity).</span></span> <span data-ttu-id="1d392-187">Můžete najít další informace o objektu indexu a v jejích částí [referenční informace sady SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), a v [referenční dokumentace rozhraní API REST Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="1d392-187">You can find more information about the Index object and its constituent parts in the [SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), as well as in the [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

### <a name="populating-the-index"></a><span data-ttu-id="1d392-188">Naplňování indexu</span><span class="sxs-lookup"><span data-stu-id="1d392-188">Populating the index</span></span>
<span data-ttu-id="1d392-189">Na další krok v `Main` je naplňte index nově vytvořený.</span><span class="sxs-lookup"><span data-stu-id="1d392-189">The next step in `Main` is to populate the newly-created index.</span></span> <span data-ttu-id="1d392-190">To se provádí v následující metodu:</span><span class="sxs-lookup"><span data-stu-id="1d392-190">This is done in the following method:</span></span>

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
            Description = "Close to town hall and the river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

<span data-ttu-id="1d392-191">Tato metoda obsahuje čtyři části.</span><span class="sxs-lookup"><span data-stu-id="1d392-191">This method has four parts.</span></span> <span data-ttu-id="1d392-192">První vytvoří pole `Hotel` objekty, které bude sloužit jako naše vstupní data na nahrát do indexu.</span><span class="sxs-lookup"><span data-stu-id="1d392-192">The first creates an array of `Hotel` objects that will serve as our input data to upload to the index.</span></span> <span data-ttu-id="1d392-193">Tato data jsou pevně pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="1d392-193">This data is hard-coded for simplicity.</span></span> <span data-ttu-id="1d392-194">Ve vaší vlastní aplikaci bude vaše data pravděpodobně pocházet z externího zdroje dat například do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="1d392-194">In your own application, your data will likely come from an external data source such as a SQL database.</span></span>

<span data-ttu-id="1d392-195">Druhá část vytvoří `IndexBatch` obsahující dokumenty.</span><span class="sxs-lookup"><span data-stu-id="1d392-195">The second part creates an `IndexBatch` containing the documents.</span></span> <span data-ttu-id="1d392-196">Zadejte operace, kterou chcete použít k dávky v době vytvoření, v takovém případě voláním `IndexBatch.Upload`.</span><span class="sxs-lookup"><span data-stu-id="1d392-196">You specify the operation you want to apply to the batch at the time you create it, in this case by calling `IndexBatch.Upload`.</span></span> <span data-ttu-id="1d392-197">Dávky je pak odeslat do indexu Azure Search pomocí `Documents.Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="1d392-197">The batch is then uploaded to the Azure Search index by the `Documents.Index` method.</span></span>

> [!NOTE]
> <span data-ttu-id="1d392-198">V tomto příkladu jsme právě nahráváte dokumenty.</span><span class="sxs-lookup"><span data-stu-id="1d392-198">In this example, we are just uploading documents.</span></span> <span data-ttu-id="1d392-199">Pokud jste chtěli sloučit změny do stávajících dokumentů nebo odstranění dokumentů, dávky můžete vytvořit voláním `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, nebo `IndexBatch.Delete` místo.</span><span class="sxs-lookup"><span data-stu-id="1d392-199">If you wanted to merge changes into existing documents or delete documents, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete` instead.</span></span> <span data-ttu-id="1d392-200">Také je možné kombinovat různé operace v jedné dávkové voláním `IndexBatch.New`, která vezme kolekci `IndexAction` objekty, z nichž každý říká službě Azure Search při provádění konkrétní operace v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1d392-200">You can also mix different operations in a single batch by calling `IndexBatch.New`, which takes a collection of `IndexAction` objects, each of which tells Azure Search to perform a particular operation on a document.</span></span> <span data-ttu-id="1d392-201">Můžete vytvořit každý `IndexAction` s vlastní operaci voláním odpovídající metoda `IndexAction.Merge`, `IndexAction.Upload`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="1d392-201">You can create each `IndexAction` with its own operation by calling the corresponding method such as `IndexAction.Merge`, `IndexAction.Upload`, and so on.</span></span>
> 
> 

<span data-ttu-id="1d392-202">Třetí součást tato metoda je blok catch, která zpracovává s případem důležitých chyb pro indexování.</span><span class="sxs-lookup"><span data-stu-id="1d392-202">The third part of this method is a catch block that handles an important error case for indexing.</span></span> <span data-ttu-id="1d392-203">Pokud služba Azure Search při indexování některých dokumentů v dávce selže, `Documents.Index` vyvolá výjimku `IndexBatchException`.</span><span class="sxs-lookup"><span data-stu-id="1d392-203">If your Azure Search service fails to index some of the documents in the batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="1d392-204">To může nastat v případě indexování dokumentů, zatímco je služba velmi zatížená.</span><span class="sxs-lookup"><span data-stu-id="1d392-204">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="1d392-205">**Důrazně doporučujeme v kódu explicitně zpracovávat tento případ.**</span><span class="sxs-lookup"><span data-stu-id="1d392-205">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="1d392-206">Indexování dokumentů, které selhaly, můžete odložit a poté zkusit znovu, nebo v závislosti na požadavcích vaší aplikace na konzistenci dat provést něco jiného.</span><span class="sxs-lookup"><span data-stu-id="1d392-206">You can delay and then retry indexing the documents that failed, or you can log and continue like the sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

> [!NOTE]
> <span data-ttu-id="1d392-207">Můžete použít `FindFailedActionsToRetry` metoda vytvořit novou dávku obsahující pouze akce, které se nezdařilo předchozí volání `Index`.</span><span class="sxs-lookup"><span data-stu-id="1d392-207">You can use the `FindFailedActionsToRetry` method to construct a new batch containing only the actions that failed in a previous call to `Index`.</span></span> <span data-ttu-id="1d392-208">Metoda je popsána [sem](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) a není diskuzi o způsobu jeho použití správně [na StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span><span class="sxs-lookup"><span data-stu-id="1d392-208">The method is documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) and there is a discussion of how to properly use it [on StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span></span>
>
>

<span data-ttu-id="1d392-209">Nakonec `UploadDocuments` na 2 sekundy odloží metoda.</span><span class="sxs-lookup"><span data-stu-id="1d392-209">Finally, the `UploadDocuments` method delays for two seconds.</span></span> <span data-ttu-id="1d392-210">Indexování ve službě Azure Search probíhá asynchronně, takže ukázková aplikace musí chvíli počkat a ujistit se, že jsou dokumenty dostupné pro vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="1d392-210">Indexing happens asynchronously in your Azure Search service, so the sample application needs to wait a short time to ensure that the documents are available for searching.</span></span> <span data-ttu-id="1d392-211">Tato odložení se obvykle používají pouze v ukázkových aplikacích a při testech.</span><span class="sxs-lookup"><span data-stu-id="1d392-211">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

#### <a name="how-the-net-sdk-handles-documents"></a><span data-ttu-id="1d392-212">Jak .NET SDK zpracovává dokumenty</span><span class="sxs-lookup"><span data-stu-id="1d392-212">How the .NET SDK handles documents</span></span>
<span data-ttu-id="1d392-213">Možná vás zajímá, jak může .NET SDK služby Azure Search odesílat do indexu instance uživatelsky definované třídy, jako třeba `Hotel`.</span><span class="sxs-lookup"><span data-stu-id="1d392-213">You may be wondering how the Azure Search .NET SDK is able to upload instances of a user-defined class like `Hotel` to the index.</span></span> <span data-ttu-id="1d392-214">Pro odpověď na tuto otázku se podívejme se na `Hotel` třídy:</span><span class="sxs-lookup"><span data-stu-id="1d392-214">To help answer that question, let's look at the `Hotel` class:</span></span>

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
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

<span data-ttu-id="1d392-215">Nejprve si všimněte, že každá veřejná vlastnost třídy `Hotel` odpovídá poli v definici indexu s jedním zásadním rozdílem: Název každého pole začíná malým písmenem („CamelCase“), zatímco název každé veřejné vlastnosti třídy `Hotel` začíná velkým písmenem („PascalCase“).</span><span class="sxs-lookup"><span data-stu-id="1d392-215">The first thing to notice is that each public property of `Hotel` corresponds to a field in the index definition, but with one crucial difference: The name of each field starts with a lower-case letter ("camel case"), while the name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="1d392-216">To je běžný scénář v .NET aplikacích provádějících datové vazby, kde je cílové schéma mimo kontrolu vývojáře aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d392-216">This is a common scenario in .NET applications that perform data-binding where the target schema is outside the control of the application developer.</span></span> <span data-ttu-id="1d392-217">Místo porušování směrnic pojmenování .NET psaním názvů vlastností ve stylu CamelCase můžete pomocí atributu `[SerializePropertyNamesAsCamelCase]` říct sadě SDK, aby mapovala názvy vlastností na CamelCase automaticky.</span><span class="sxs-lookup"><span data-stu-id="1d392-217">Rather than having to violate the .NET naming guidelines by making property names camel-case, you can tell the SDK to map the property names to camel-case automatically with the `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="1d392-218">.NET SDK služby Azure Search používá k serializaci a deserializaci vlastních objektů modelu do a z JSON knihovnu [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="1d392-218">The Azure Search .NET SDK uses the [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library to serialize and deserialize your custom model objects to and from JSON.</span></span> <span data-ttu-id="1d392-219">V případě potřeby lze serializaci přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="1d392-219">You can customize this serialization if needed.</span></span> <span data-ttu-id="1d392-220">Další podrobnosti najdete v tématu [vlastní serializace s JSON.NET](#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="1d392-220">For more details, see [Custom Serialization with JSON.NET](#JsonDotNet).</span></span>
> 
> 

<span data-ttu-id="1d392-221">Druhý si všimněte, jako jsou atributy, `IsFilterable`, `IsSearchable`, `Key`, a `Analyzer` , uspořádání každé veřejné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1d392-221">The second thing to notice are the attributes such as `IsFilterable`, `IsSearchable`, `Key`, and `Analyzer` that decorate each public property.</span></span> <span data-ttu-id="1d392-222">Tyto atributy rozvržení přímo na [odpovídající atributy indexu Azure Search](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span><span class="sxs-lookup"><span data-stu-id="1d392-222">These attributes map directly to the [corresponding attributes of the Azure Search index](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span></span> <span data-ttu-id="1d392-223">`FieldBuilder` Třída tyto používá k vytvoření definice pole pro index.</span><span class="sxs-lookup"><span data-stu-id="1d392-223">The `FieldBuilder` class uses these to construct field definitions for the index.</span></span>

<span data-ttu-id="1d392-224">Třetí důležité o `Hotel` třídy jsou datové typy veřejných vlastností.</span><span class="sxs-lookup"><span data-stu-id="1d392-224">The third important thing about the `Hotel` class are the data types of the public properties.</span></span> <span data-ttu-id="1d392-225">.NET typy těchto vlastností se mapují na odpovídající typy polí v definici indexu.</span><span class="sxs-lookup"><span data-stu-id="1d392-225">The .NET types of  these properties map to their equivalent field types in the index definition.</span></span> <span data-ttu-id="1d392-226">Například řetězcová vlastnost `Category` se mapuje na pole `category`, které je typu `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="1d392-226">For example, the `Category` string property maps to the `category` field, which is of type `Edm.String`.</span></span> <span data-ttu-id="1d392-227">Podobná mapování typu probíhají mezi `bool?` a `Edm.Boolean`, `DateTimeOffset?` a `Edm.DateTimeOffset` atd. Konkrétní pravidla pro mapování typu jsou popsaná u metody `Documents.Get` v tématu [Reference k sadě .NET SDK služby Azure Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="1d392-227">There are similar type mappings between `bool?` and `Edm.Boolean`, `DateTimeOffset?` and `Edm.DateTimeOffset`, etc. The specific rules for the type mapping are documented with the `Documents.Get` method in the [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span> <span data-ttu-id="1d392-228">`FieldBuilder` Třída má na starosti toto mapování pro vás, ale stále může být užitečné rozumět v případě, že potřebujete vyřešit jakékoliv problémy serializace.</span><span class="sxs-lookup"><span data-stu-id="1d392-228">The `FieldBuilder` class takes care of this mapping for you, but it can still be helpful to understand in case you need to troubleshoot any serialization issues.</span></span>

<span data-ttu-id="1d392-229">Tato možnost používat vlastní třídy jako dokumenty funguje v obou směrech; Můžete také načíst výsledky vyhledávání a nechat sadu SDK automaticky deserializovala do typu podle vaší volby, jak vidíte v další části.</span><span class="sxs-lookup"><span data-stu-id="1d392-229">This ability to use your own classes as documents works in both directions; You can also retrieve search results and have the SDK automatically deserialize them to a type of your choice, as we will see in the next section.</span></span>

> [!NOTE]
> <span data-ttu-id="1d392-230">.NET SDK služby Azure Search také podporuje dynamicky typované dokumenty pomocí třídy `Document`, která zajišťuje mapování klíč-hodnota názvů polí na hodnoty polí.</span><span class="sxs-lookup"><span data-stu-id="1d392-230">The Azure Search .NET SDK also supports dynamically-typed documents using the `Document` class, which is a key/value mapping of field names to field values.</span></span> <span data-ttu-id="1d392-231">To je užitečné v situacích, kdy v době navrhování neznáte schéma indexu nebo kde by vázání na konkrétní třídy modelu bylo nepraktické.</span><span class="sxs-lookup"><span data-stu-id="1d392-231">This is useful in scenarios where you don't know the index schema at design-time, or where it would be inconvenient to bind to specific model classes.</span></span> <span data-ttu-id="1d392-232">Všechny metody v sadě SDK, které pracují s dokumenty, mají přetížení, které pracují se třídou `Document`, ale i přetížení silně závislá na typu, která přebírají parametr obecného typu.</span><span class="sxs-lookup"><span data-stu-id="1d392-232">All the methods in the SDK that deal with documents have overloads that work with the `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="1d392-233">Pouze ty druhé se používají v ukázkovém kódu v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1d392-233">Only the latter are used in the sample code in this tutorial.</span></span> <span data-ttu-id="1d392-234">`Document` Třída dědí z `Dictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="1d392-234">The `Document` class inherits from `Dictionary<string, object>`.</span></span> <span data-ttu-id="1d392-235">Můžete najít další podrobnosti o [zde](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span><span class="sxs-lookup"><span data-stu-id="1d392-235">You can find other details [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span></span>
> 
> 

<span data-ttu-id="1d392-236">**Proč byste měli používat datové typy s možnou hodnotou null**</span><span class="sxs-lookup"><span data-stu-id="1d392-236">**Why you should use nullable data types**</span></span>

<span data-ttu-id="1d392-237">Při navrhování vlastních tříd modelu pro mapování na index Azure Search doporučujeme deklarovat vlastnosti typů hodnot, jako jsou `bool` nebo `int`, s možnou hodnotou null (například `bool?` místo `bool`).</span><span class="sxs-lookup"><span data-stu-id="1d392-237">When designing your own model classes to map to an Azure Search index, we recommend declaring properties of value types such as `bool` and `int` to be nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="1d392-238">Pokud použijete vlastnost se zakázanou hodnotou null, musíte **zajistit**, aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="1d392-238">If you use a non-nullable property, you have to **guarantee** that no documents in your index contain a null value for the corresponding field.</span></span> <span data-ttu-id="1d392-239">Sada SDK, ani služba Azure Search vám s tím nepomůže.</span><span class="sxs-lookup"><span data-stu-id="1d392-239">Neither the SDK nor the Azure Search service will help you to enforce this.</span></span>

<span data-ttu-id="1d392-240">Nejedná se pouze o hypotetický problém: představte si situaci, kdy přidáte nové pole do stávajícího indexu typu `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="1d392-240">This is not just a hypothetical concern: Imagine a scenario where you add a new field to an existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="1d392-241">Po aktualizaci definice indexu budou mít všechny dokumenty pro toto nové pole hodnotu null (protože všechny typy jsou ve službě Azure Search s možností null).</span><span class="sxs-lookup"><span data-stu-id="1d392-241">After updating the index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="1d392-242">Pokud pak použijete třídu modelu s vlastností `int` se zakázanou hodnotou null, při pokusu o načtení dokumentů dojde k vyvolání podobné výjimky `JsonSerializationException`:</span><span class="sxs-lookup"><span data-stu-id="1d392-242">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying to retrieve documents:</span></span>

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="1d392-243">Z tohoto důvodu doporučujeme jako osvědčený postup používat ve třídách modelu typy s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="1d392-243">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a><span data-ttu-id="1d392-244">Vlastní serializace s JSON.NET</span><span class="sxs-lookup"><span data-stu-id="1d392-244">Custom Serialization with JSON.NET</span></span>
<span data-ttu-id="1d392-245">Sada SDK používá technologii JSON.NET pro serializaci a deserializaci dokumenty.</span><span class="sxs-lookup"><span data-stu-id="1d392-245">The SDK uses JSON.NET for serializing and deserializing documents.</span></span> <span data-ttu-id="1d392-246">Můžete upravit serializace a deserializace v případě potřeby tak, že definujete vlastní `JsonConverter` nebo `IContractResolver` (najdete v článku [JSON.NET dokumentace](http://www.newtonsoft.com/json/help/html/Introduction.htm) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="1d392-246">You can customize serialization and deserialization if needed by defining your own `JsonConverter` or `IContractResolver` (see the [JSON.NET documentation](http://www.newtonsoft.com/json/help/html/Introduction.htm) for more details).</span></span> <span data-ttu-id="1d392-247">To může být užitečné, pokud chcete přizpůsobit existující třídy modelu z vaší aplikace pro použití se službou Azure Search a jiné pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="1d392-247">This can be useful when you want to adapt an existing model class from your application for use with Azure Search, and other more advanced scenarios.</span></span> <span data-ttu-id="1d392-248">S vlastní serializace můžete například:</span><span class="sxs-lookup"><span data-stu-id="1d392-248">For example, with custom serialization you can:</span></span>

* <span data-ttu-id="1d392-249">Zahrnout nebo vyloučit některé vlastnosti vaší třídy modelu z uložené jako pole dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1d392-249">Include or exclude certain properties of your model class from being stored as document fields.</span></span>
* <span data-ttu-id="1d392-250">Mapování mezi názvy vlastností v kódu a názvy polí v indexu.</span><span class="sxs-lookup"><span data-stu-id="1d392-250">Map between property names in your code and field names in your index.</span></span>
* <span data-ttu-id="1d392-251">Vytvoření vlastních atributů, které lze použít pro mapování vlastností dokumentu polí.</span><span class="sxs-lookup"><span data-stu-id="1d392-251">Create custom attributes that can be used for mapping properties to document fields.</span></span>

<span data-ttu-id="1d392-252">Můžete najít příklady implementace vlastní serializace při testování částí pro Azure Search .NET SDK na Githubu.</span><span class="sxs-lookup"><span data-stu-id="1d392-252">You can find examples of implementing custom serialization in the unit tests for the Azure Search .NET SDK on GitHub.</span></span> <span data-ttu-id="1d392-253">Je to dobrý výchozí bod [tato složka](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span><span class="sxs-lookup"><span data-stu-id="1d392-253">A good starting point is [this folder](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span></span> <span data-ttu-id="1d392-254">Obsahuje třídy, které používají vlastní serializace testy.</span><span class="sxs-lookup"><span data-stu-id="1d392-254">It contains classes that are used by the custom serialization tests.</span></span>

### <a name="searching-for-documents-in-the-index"></a><span data-ttu-id="1d392-255">Hledání dokumentů v indexu</span><span class="sxs-lookup"><span data-stu-id="1d392-255">Searching for documents in the index</span></span>
<span data-ttu-id="1d392-256">Poslední krok v ukázkové aplikace je k vyhledání některých dokumentů v indexu.</span><span class="sxs-lookup"><span data-stu-id="1d392-256">The last step in the sample application is to search for some documents in the index.</span></span> <span data-ttu-id="1d392-257">Následující metoda provádí toto:</span><span class="sxs-lookup"><span data-stu-id="1d392-257">The following method does this:</span></span>

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "hotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("budget", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
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

    Console.WriteLine("Search the entire index for the term 'motel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);
}
```

<span data-ttu-id="1d392-258">Pokaždé, když se provede dotaz, tato metoda první vytvoří novou `SearchParameters` objektu.</span><span class="sxs-lookup"><span data-stu-id="1d392-258">Each time it executes a query, this method first creates a new `SearchParameters` object.</span></span> <span data-ttu-id="1d392-259">To slouží k určení dalších možností pro dotaz například třídění, filtrování, stránkování a používání omezujících vlastností.</span><span class="sxs-lookup"><span data-stu-id="1d392-259">This is used to specify additional options for the query such as sorting, filtering, paging, and faceting.</span></span> <span data-ttu-id="1d392-260">Tato metoda jsme nastavení `Filter`, `Select`, `OrderBy`, a `Top` vlastnost pro různé dotazy.</span><span class="sxs-lookup"><span data-stu-id="1d392-260">In this method, we're setting the `Filter`, `Select`, `OrderBy`, and `Top` property for different queries.</span></span> <span data-ttu-id="1d392-261">Všechny `SearchParameters` jsou popsané vlastnosti [zde](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span><span class="sxs-lookup"><span data-stu-id="1d392-261">All the `SearchParameters` properties are documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span></span>

<span data-ttu-id="1d392-262">Dalším krokem je ve skutečnosti provést vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="1d392-262">The next step is to actually execute the search query.</span></span> <span data-ttu-id="1d392-263">To se provádí pomocí `Documents.Search` metoda.</span><span class="sxs-lookup"><span data-stu-id="1d392-263">This is done using the `Documents.Search` method.</span></span> <span data-ttu-id="1d392-264">Pro každý dotaz jsme předat hledaný text, který chcete použít jako řetězec (nebo `"*"` Pokud neexistuje žádný hledaný text), plus parametry vyhledávání vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="1d392-264">For each query, we pass the search text to use as a string (or `"*"` if there is no search text), plus the search parameters created earlier.</span></span> <span data-ttu-id="1d392-265">Můžeme také určit `Hotel` jako parametr typu pro `Documents.Search`, která sděluje sady SDK k deserializaci dokumenty ve výsledcích hledání do objektů typu `Hotel`.</span><span class="sxs-lookup"><span data-stu-id="1d392-265">We also specify `Hotel` as the type parameter for `Documents.Search`, which tells the SDK to deserialize documents in the search results into objects of type `Hotel`.</span></span>

> [!NOTE]
> <span data-ttu-id="1d392-266">Můžete najít další informace o syntaxe výrazu dotazu vyhledávání [zde](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="1d392-266">You can find more information about the search query expression syntax [here](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span></span>
> 
> 

<span data-ttu-id="1d392-267">Nakonec po každém dotazu tato metoda iteruje všechny shody ve výsledcích hledání, tisk každý dokument do konzoly:</span><span class="sxs-lookup"><span data-stu-id="1d392-267">Finally, after each query this method iterates through all the matches in the search results, printing each document to the console:</span></span>

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

<span data-ttu-id="1d392-268">Podívejme bližší pohled na každý dotaz na naopak.</span><span class="sxs-lookup"><span data-stu-id="1d392-268">Let's take a closer look at each of the queries in turn.</span></span> <span data-ttu-id="1d392-269">Tady je kód provést první dotaz:</span><span class="sxs-lookup"><span data-stu-id="1d392-269">Here is the code to execute the first query:</span></span>

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

<span data-ttu-id="1d392-270">V takovém případě jsme se hledající hotely, které odpovídají slovo "rozpočet", a chcete získat zpět pouze hotelů názvy, podle specifikace `Select` parametr.</span><span class="sxs-lookup"><span data-stu-id="1d392-270">In this case, we're searching for hotels that match the word "budget", and we want to get back only the hotel names, as specified by the `Select` parameter.</span></span> <span data-ttu-id="1d392-271">Zde jsou výsledky:</span><span class="sxs-lookup"><span data-stu-id="1d392-271">Here are the results:</span></span>

    Name: Roach Motel

<span data-ttu-id="1d392-272">V dalším kroku chceme nalezení hotelů s noční počet menší než 150 USD a vrátí pouze hotelů ID a popis:</span><span class="sxs-lookup"><span data-stu-id="1d392-272">Next, we want to find the hotels with a nightly rate of less than $150, and return only the hotel ID and description:</span></span>

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

<span data-ttu-id="1d392-273">Tento dotaz používá OData `$filter` výrazu `baseRate lt 150`, chcete-li filtrovat dokumenty v indexu.</span><span class="sxs-lookup"><span data-stu-id="1d392-273">This query uses an OData `$filter` expression, `baseRate lt 150`, to filter the documents in the index.</span></span> <span data-ttu-id="1d392-274">Můžete najít další informace o syntaxi OData, který podporuje Azure Search [zde](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="1d392-274">You can find out more about the OData syntax that Azure Search supports [here](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span></span>

<span data-ttu-id="1d392-275">Zde jsou výsledky dotazu:</span><span class="sxs-lookup"><span data-stu-id="1d392-275">Here are the results of the query:</span></span>

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

<span data-ttu-id="1d392-276">V dalším kroku chceme nalezení hotelů nejvyšší dva, které mají byl naposledy renovovanou a zobrazit název hotelů a datum posledního obnova.</span><span class="sxs-lookup"><span data-stu-id="1d392-276">Next, we want to find the top two hotels that have been most recently renovated, and show the hotel name and last renovation date.</span></span> <span data-ttu-id="1d392-277">Zde je kód:</span><span class="sxs-lookup"><span data-stu-id="1d392-277">Here is the code:</span></span> 

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

<span data-ttu-id="1d392-278">V takovém případě znovu používáme syntaxe OData k určení `OrderBy` parametr jako `lastRenovationDate desc`.</span><span class="sxs-lookup"><span data-stu-id="1d392-278">In this case, we again use OData syntax to specify the `OrderBy` parameter as `lastRenovationDate desc`.</span></span> <span data-ttu-id="1d392-279">Můžeme také nastavit `Top` 2 zajistit nám pouze získat horních dvou dokumenty.</span><span class="sxs-lookup"><span data-stu-id="1d392-279">We also set `Top` to 2 to ensure we only get the top two documents.</span></span> <span data-ttu-id="1d392-280">Před, jsme nastavit jako `Select` zadat pole, která má být vrácen.</span><span class="sxs-lookup"><span data-stu-id="1d392-280">As before, we set `Select` to specify which fields should be returned.</span></span>

<span data-ttu-id="1d392-281">Zde jsou výsledky:</span><span class="sxs-lookup"><span data-stu-id="1d392-281">Here are the results:</span></span>

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

<span data-ttu-id="1d392-282">Nakonec chcete najít všechny hotels, které odpovídají slovo "motel":</span><span class="sxs-lookup"><span data-stu-id="1d392-282">Finally, we want to find all hotels that match the word "motel":</span></span>

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

<span data-ttu-id="1d392-283">A tady jsou výsledky, které zahrnují všechna pole, protože jsme nezadali `Select` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="1d392-283">And here are the results, which include all fields since we did not specify the `Select` property:</span></span>

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

<span data-ttu-id="1d392-284">Dokončení tohoto kroku kurzu, ale není tady zastavit.</span><span class="sxs-lookup"><span data-stu-id="1d392-284">This step completes the tutorial, but don't stop here.</span></span> <span data-ttu-id="1d392-285">**Další kroky** poskytuje další zdroje dalších informací o službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1d392-285">**Next steps** provides additional resources for learning more about Azure Search.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d392-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d392-286">Next steps</span></span>
* <span data-ttu-id="1d392-287">Projděte si referenční materiály pro [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) a [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="1d392-287">Browse the references for the [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
* <span data-ttu-id="1d392-288">Prohloubit vašich znalostí prostřednictvím [videa a jiné ukázky a výukové programy](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="1d392-288">Deepen your knowledge through [videos and other samples and tutorials](search-video-demo-tutorial-list.md).</span></span>
* <span data-ttu-id="1d392-289">Zkontrolujte [konvence vytváření názvů](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) další pravidla pro pojmenovávání různé objekty.</span><span class="sxs-lookup"><span data-stu-id="1d392-289">Review [naming conventions](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) to learn the rules for naming various objects.</span></span>
* <span data-ttu-id="1d392-290">Zkontrolujte [podporované datové typy](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1d392-290">Review [supported data types](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.</span></span>
