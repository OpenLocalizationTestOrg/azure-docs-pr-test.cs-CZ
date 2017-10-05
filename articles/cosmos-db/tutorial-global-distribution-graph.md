---
title: "Globální distribuční kurz pro Azure Cosmos DB pro rozhraní Graph API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní Graph API."
services: cosmos-db
keywords: "globální distribuční, grafu, gremlin"
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: 3c8794fe33c2ff5aa79559ea2c323cf8d92b426a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a><span data-ttu-id="cd86a-104">Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="cd86a-104">How to setup Azure Cosmos DB global distribution using the Graph API</span></span>

<span data-ttu-id="cd86a-105">V tomto článku jsme ukazují, jak pomocí portálu Azure do instalačního programu globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní Graph API (preview).</span><span class="sxs-lookup"><span data-stu-id="cd86a-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the Graph API (preview).</span></span>

<span data-ttu-id="cd86a-106">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd86a-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="cd86a-107">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cd86a-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="cd86a-108">Nakonfigurujte globální distribuční pomocí [rozhraní Graph API](graph-introduction.md) (preview)</span><span class="sxs-lookup"><span data-stu-id="cd86a-108">Configure global distribution using the [Graph APIs](graph-introduction.md) (preview)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a><span data-ttu-id="cd86a-109">Připojování k upřednostňovaná oblast pomocí rozhraní Graph API pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="cd86a-109">Connecting to a preferred region using the Graph API using the .NET SDK</span></span>

<span data-ttu-id="cd86a-110">Rozhraní Graph API je k dispozici jako rozšíření knihovny nad DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="cd86a-110">The Graph API is exposed as an extension library on top of the DocumentDB SDK.</span></span>

<span data-ttu-id="cd86a-111">Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu.</span><span class="sxs-lookup"><span data-stu-id="cd86a-111">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="cd86a-112">Tento krok můžete provést nastavením zásad pro připojení.</span><span class="sxs-lookup"><span data-stu-id="cd86a-112">This can be done by setting the connection policy.</span></span> <span data-ttu-id="cd86a-113">Na základě konfigurace účtu Azure Cosmos DB, aktuální místní dostupnosti a seznamu předvoleb zadán, bude vybrána optimální koncový bod SDK k provedení operace zápisu a operace čtení.</span><span class="sxs-lookup"><span data-stu-id="cd86a-113">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the SDK to perform write and read operations.</span></span>

<span data-ttu-id="cd86a-114">Tento seznam předvoleb je zadána při inicializaci připojení pomocí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="cd86a-114">This preference list is specified when initializing a connection using the SDKs.</span></span> <span data-ttu-id="cd86a-115">Sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.</span><span class="sxs-lookup"><span data-stu-id="cd86a-115">The SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

* <span data-ttu-id="cd86a-116">**Zapíše**: Sada SDK bude automaticky posílat zápis všech zápisů na aktuální oblasti.</span><span class="sxs-lookup"><span data-stu-id="cd86a-116">**Writes**: The SDK will automatically send all writes to the current write region.</span></span>
* <span data-ttu-id="cd86a-117">**Přečte**: všechny operace čtení odešle první oblasti k dispozici v seznamu PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="cd86a-117">**Reads**: All reads will be sent to the first available region in the PreferredLocations list.</span></span> <span data-ttu-id="cd86a-118">Pokud se požadavek nezdaří, klient se nezdaří dolů v seznamu další oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="cd86a-118">If the request fails, the client will fail down the list to the next region, and so on.</span></span> <span data-ttu-id="cd86a-119">Sady SDK se pouze pokusí číst z oblastí, zadaný v PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="cd86a-119">The SDKs will only attempt to read from the regions specified in PreferredLocations.</span></span> <span data-ttu-id="cd86a-120">Ano například pokud Cosmos DB účet je k dispozici v tři oblasti, ale klient pouze určuje pro PreferredLocations dvou oblastí bez zápisu, pak žádné čtení se zpracuje mimo oblast zápisu, i v případě převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="cd86a-120">So, for example, if the Cosmos DB account is available in three regions, but the client only specifies two of the non-write regions for PreferredLocations, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="cd86a-121">Aplikace můžete ověřit aktuální koncový bod zápisu a čtení koncový bod vybrali SDK kontrolou dvě vlastnosti WriteEndpoint a ReadEndpoint dostupné ve verzi sady SDK 1.8 a výše.</span><span class="sxs-lookup"><span data-stu-id="cd86a-121">The application can verify the current write endpoint and read endpoint chosen by the SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span> <span data-ttu-id="cd86a-122">Pokud není nastavena vlastnost PreferredLocations, bude z oblasti aktuální zápisu zpracovat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="cd86a-122">If the PreferredLocations property is not set, all requests will be served from the current write region.</span></span>

### <a name="using-the-sdk"></a><span data-ttu-id="cd86a-123">Pomocí sady SDK</span><span class="sxs-lookup"><span data-stu-id="cd86a-123">Using the SDK</span></span>

<span data-ttu-id="cd86a-124">Například v sadě SDK .NET `ConnectionPolicy` parametr pro `DocumentClient` konstruktor má vlastnost s názvem `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="cd86a-124">For example, in the .NET SDK, the `ConnectionPolicy` parameter for the `DocumentClient` constructor has a property called `PreferredLocations`.</span></span> <span data-ttu-id="cd86a-125">Tuto vlastnost lze nastavit na seznam názvů oblast.</span><span class="sxs-lookup"><span data-stu-id="cd86a-125">This property can be set to a list of region names.</span></span> <span data-ttu-id="cd86a-126">Zobrazení názvů pro [oblasti Azure] [ regions] lze zadat jako součást `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="cd86a-126">The display names for [Azure Regions][regions] can be specified as part of `PreferredLocations`.</span></span>

> [!NOTE]
> <span data-ttu-id="cd86a-127">Adresy URL pro koncové body by se neměla považovat jako dlohotrvající konstanty.</span><span class="sxs-lookup"><span data-stu-id="cd86a-127">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="cd86a-128">Služba může aktualizovat tyto v libovolném bodě.</span><span class="sxs-lookup"><span data-stu-id="cd86a-128">The service may update these at any point.</span></span> <span data-ttu-id="cd86a-129">Sada SDK zpracovává tuto změnu automaticky.</span><span class="sxs-lookup"><span data-stu-id="cd86a-129">The SDK handles this change automatically.</span></span>
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

<span data-ttu-id="cd86a-130">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cd86a-130">That's it, that completes this tutorial.</span></span> <span data-ttu-id="cd86a-131">Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="cd86a-131">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="cd86a-132">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="cd86a-132">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd86a-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd86a-133">Next steps</span></span>

<span data-ttu-id="cd86a-134">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="cd86a-134">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cd86a-135">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cd86a-135">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="cd86a-136">Nakonfigurujte globální distribuční pomocí rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="cd86a-136">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="cd86a-137">Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cd86a-137">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cd86a-138">Vývoj místně pomocí emulátoru</span><span class="sxs-lookup"><span data-stu-id="cd86a-138">Develop locally with the emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

