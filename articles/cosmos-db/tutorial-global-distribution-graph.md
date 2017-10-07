---
title: "kurz globální distribuční databáze Cosmos aaaAzure pro rozhraní Graph API | Microsoft Docs"
description: "Zjistěte, jak pomocí globální distribuční databázi Cosmos Azure toosetup hello rozhraní Graph API."
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
ms.openlocfilehash: 1629a31e12a18079f63e07c4909862b36b5f4c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-graph-api"></a><span data-ttu-id="50ae7-104">Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní Graph API hello</span><span class="sxs-lookup"><span data-stu-id="50ae7-104">How toosetup Azure Cosmos DB global distribution using hello Graph API</span></span>

<span data-ttu-id="50ae7-105">V tomto článku ukážeme, jak toouse hello globální distribuční databázi Cosmos Azure Azure portálu toosetup a potom se připojte pomocí hello rozhraní Graph API (preview).</span><span class="sxs-lookup"><span data-stu-id="50ae7-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello Graph API (preview).</span></span>

<span data-ttu-id="50ae7-106">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="50ae7-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="50ae7-107">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="50ae7-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="50ae7-108">Nakonfigurujte globální distribuci pomocí hello [rozhraní Graph API](graph-introduction.md) (preview)</span><span class="sxs-lookup"><span data-stu-id="50ae7-108">Configure global distribution using hello [Graph APIs](graph-introduction.md) (preview)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-graph-api-using-hello-net-sdk"></a><span data-ttu-id="50ae7-109">Připojení tooa upřednostňovaná oblast pomocí hello rozhraní Graph API pomocí hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="50ae7-109">Connecting tooa preferred region using hello Graph API using hello .NET SDK</span></span>

<span data-ttu-id="50ae7-110">Hello rozhraní Graph API je k dispozici jako rozšíření knihovny nad hello DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="50ae7-110">hello Graph API is exposed as an extension library on top of hello DocumentDB SDK.</span></span>

<span data-ttu-id="50ae7-111">V pořadí tootake výhody [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat hello seřazené předvoleb seznam oblastí toobe použít tooperform dokumentu operace.</span><span class="sxs-lookup"><span data-stu-id="50ae7-111">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="50ae7-112">Tento krok můžete provést nastavením zásad pro připojení hello.</span><span class="sxs-lookup"><span data-stu-id="50ae7-112">This can be done by setting hello connection policy.</span></span> <span data-ttu-id="50ae7-113">V závislosti na konfiguraci účtu Azure Cosmos DB hello aktuální místní dostupnosti a seznamu předvoleb hello zadat, hello většina optimální koncový bod se zvolí hello SDK tooperform zápisu a operace čtení.</span><span class="sxs-lookup"><span data-stu-id="50ae7-113">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello SDK tooperform write and read operations.</span></span>

<span data-ttu-id="50ae7-114">Tento seznam předvoleb je zadána při inicializaci připojení pomocí sady SDK hello.</span><span class="sxs-lookup"><span data-stu-id="50ae7-114">This preference list is specified when initializing a connection using hello SDKs.</span></span> <span data-ttu-id="50ae7-115">Hello sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.</span><span class="sxs-lookup"><span data-stu-id="50ae7-115">hello SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

* <span data-ttu-id="50ae7-116">**Zapíše**: hello SDK automaticky odesílat všechny zapíše toohello aktuální zápisu oblasti.</span><span class="sxs-lookup"><span data-stu-id="50ae7-116">**Writes**: hello SDK will automatically send all writes toohello current write region.</span></span>
* <span data-ttu-id="50ae7-117">**Přečte**: všechny operace čtení zašle toohello první dostupné oblasti v seznamu PreferredLocations hello.</span><span class="sxs-lookup"><span data-stu-id="50ae7-117">**Reads**: All reads will be sent toohello first available region in hello PreferredLocations list.</span></span> <span data-ttu-id="50ae7-118">Pokud hello požadavek selže, hello klienta se nezdaří dolů hello seznamu toohello další oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="50ae7-118">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span> <span data-ttu-id="50ae7-119">Hello sady SDK se pouze pokusí tooread z oblastí hello zadaný v PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="50ae7-119">hello SDKs will only attempt tooread from hello regions specified in PreferredLocations.</span></span> <span data-ttu-id="50ae7-120">Ano například pokud hello Cosmos DB účtu je k dispozici v tři oblasti, ale hello klienta pouze určuje pro PreferredLocations dvou oblastí bez zápisu hello, pak žádný čtení se zpracuje mimo oblast hello zápisu, i v případě hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="50ae7-120">So, for example, if hello Cosmos DB account is available in three regions, but hello client only specifies two of hello non-write regions for PreferredLocations, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="50ae7-121">aplikace Hello můžete ověřte hello aktuální koncový bod zápisu a čtení koncový bod zvolí hello SDK pomocí kontrola dvě vlastnosti WriteEndpoint a ReadEndpoint, k dispozici ve verzi sady SDK 1.8 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="50ae7-121">hello application can verify hello current write endpoint and read endpoint chosen by hello SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span> <span data-ttu-id="50ae7-122">Pokud není nastavena vlastnost PreferredLocations hello, bude z aktuální oblasti zápisu hello zpracovat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="50ae7-122">If hello PreferredLocations property is not set, all requests will be served from hello current write region.</span></span>

### <a name="using-hello-sdk"></a><span data-ttu-id="50ae7-123">Pomocí hello SDK</span><span class="sxs-lookup"><span data-stu-id="50ae7-123">Using hello SDK</span></span>

<span data-ttu-id="50ae7-124">Například v hello .NET SDK, hello `ConnectionPolicy` parametr pro hello `DocumentClient` konstruktor má vlastnost s názvem `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="50ae7-124">For example, in hello .NET SDK, hello `ConnectionPolicy` parameter for hello `DocumentClient` constructor has a property called `PreferredLocations`.</span></span> <span data-ttu-id="50ae7-125">Tuto vlastnost lze nastavit tooa seznam názvů oblast.</span><span class="sxs-lookup"><span data-stu-id="50ae7-125">This property can be set tooa list of region names.</span></span> <span data-ttu-id="50ae7-126">Hello zobrazované názvy pro [oblasti Azure] [ regions] lze zadat jako součást `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="50ae7-126">hello display names for [Azure Regions][regions] can be specified as part of `PreferredLocations`.</span></span>

> [!NOTE]
> <span data-ttu-id="50ae7-127">Hello adresy URL pro koncové body hello by se neměla považovat jako dlohotrvající konstanty.</span><span class="sxs-lookup"><span data-stu-id="50ae7-127">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="50ae7-128">Služba Hello může aktualizovat tyto v libovolném bodě.</span><span class="sxs-lookup"><span data-stu-id="50ae7-128">hello service may update these at any point.</span></span> <span data-ttu-id="50ae7-129">Tato změna zpracovává Hello SDK automaticky.</span><span class="sxs-lookup"><span data-stu-id="50ae7-129">hello SDK handles this change automatically.</span></span>
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

// connect tooAzure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

<span data-ttu-id="50ae7-130">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="50ae7-130">That's it, that completes this tutorial.</span></span> <span data-ttu-id="50ae7-131">Další informace jak toomanage hello konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="50ae7-131">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="50ae7-132">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="50ae7-132">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="50ae7-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50ae7-133">Next steps</span></span>

<span data-ttu-id="50ae7-134">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="50ae7-134">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="50ae7-135">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="50ae7-135">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="50ae7-136">Nakonfigurujte globální distribuci pomocí hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="50ae7-136">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="50ae7-137">Nyní můžete přejít toohello další kurz toolearn jak hello toodevelop místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="50ae7-137">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="50ae7-138">Vývoj místně pomocí emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="50ae7-138">Develop locally with hello emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

