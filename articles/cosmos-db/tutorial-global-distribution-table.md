---
title: "kurz globální distribuční aaaAzure Cosmos DB pro rozhraní API tabulky | Microsoft Docs"
description: "Zjistěte, jak pomocí globální distribuční databázi Cosmos Azure toosetup hello tabulky rozhraní API."
services: cosmos-db
keywords: "globální distribuční tabulky"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: d2a995e09c37f9449856aef2ab707e95eb8a540c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-table-api"></a><span data-ttu-id="8f44f-104">Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní API tabulky hello</span><span class="sxs-lookup"><span data-stu-id="8f44f-104">How toosetup Azure Cosmos DB global distribution using hello Table API</span></span>

<span data-ttu-id="8f44f-105">V tomto článku ukážeme, jak toouse hello globální distribuční databázi Cosmos Azure Azure portálu toosetup a potom se připojte pomocí hello tabulky rozhraní API (preview).</span><span class="sxs-lookup"><span data-stu-id="8f44f-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello Table API (preview).</span></span>

<span data-ttu-id="8f44f-106">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="8f44f-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="8f44f-107">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8f44f-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="8f44f-108">Nakonfigurujte globální distribuci pomocí hello [tabulky rozhraní API](table-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="8f44f-108">Configure global distribution using hello [Table API](table-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-table-api"></a><span data-ttu-id="8f44f-109">Připojení tooa upřednostňovaná oblast pomocí hello tabulky rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8f44f-109">Connecting tooa preferred region using hello Table API</span></span>

<span data-ttu-id="8f44f-110">V pořadí tootake výhody [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat hello seřazené předvoleb seznam oblastí toobe použít tooperform dokumentu operace.</span><span class="sxs-lookup"><span data-stu-id="8f44f-110">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="8f44f-111">To můžete provést nastavení hello `TablePreferredLocations` hodnota konfigurace v konfiguraci aplikace hello pro hello preview SDK úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8f44f-111">This can be done by setting hello `TablePreferredLocations` configuration value in hello app config for hello preview Azure Storage SDK.</span></span> <span data-ttu-id="8f44f-112">V závislosti na konfiguraci účtu Azure Cosmos DB hello aktuální místní dostupnosti a hello předvoleb seznamu zadán, hello většina optimální koncového bodu, bude použita volba hello sada SDK úložiště Azure tooperform zápis a operace čtení.</span><span class="sxs-lookup"><span data-stu-id="8f44f-112">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello Azure Storage SDK tooperform write and read operations.</span></span>

<span data-ttu-id="8f44f-113">Hello `TablePreferredLocations` by měla obsahovat čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8f44f-113">hello `TablePreferredLocations` should contain a comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="8f44f-114">Každá instance klienta můžete určit podmnožinu těchto oblastí v hello upřednostňované pořadí pro čtení s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="8f44f-114">Each client instance can specify a subset of these regions in hello preferred order for low latency reads.</span></span> <span data-ttu-id="8f44f-115">Hello oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`.</span><span class="sxs-lookup"><span data-stu-id="8f44f-115">hello regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span>

<span data-ttu-id="8f44f-116">Všechny operace čtení budou odeslány toohello první dostupné oblasti hello `TablePreferredLocations` seznamu.</span><span class="sxs-lookup"><span data-stu-id="8f44f-116">All reads will be sent toohello first available region in hello `TablePreferredLocations` list.</span></span> <span data-ttu-id="8f44f-117">Pokud hello požadavek selže, hello klienta se nezdaří dolů hello seznamu toohello další oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8f44f-117">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span>

<span data-ttu-id="8f44f-118">Hello SDK se pouze pokusí tooread ze zadané v oblasti hello `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="8f44f-118">hello SDK will only attempt tooread from hello regions specified in `TablePreferredLocations`.</span></span> <span data-ttu-id="8f44f-119">Ano, například pokud hello databázového účtu je k dispozici v tři oblasti, ale hello klienta určuje pouze dva z hello oblasti bez zápisu pro `TablePreferredLocations`, pak žádný čtení se zpracuje mimo oblast hello zápisu, i v případě hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8f44f-119">So, for example, if hello Database Account is available in three regions, but hello client only specifies two of hello non-write regions for `TablePreferredLocations`, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="8f44f-120">Hello SDK automaticky odesílat všechny zápisy toohello aktuální zápisu oblasti.</span><span class="sxs-lookup"><span data-stu-id="8f44f-120">hello SDK will automatically send all writes toohello current write region.</span></span>

<span data-ttu-id="8f44f-121">Pokud hello `TablePreferredLocations` není nastavena vlastnost, z aktuální oblasti zápisu hello se zpracuje všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="8f44f-121">If hello `TablePreferredLocations` property is not set, all requests will be served from hello current write region.</span></span>

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

<span data-ttu-id="8f44f-122">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8f44f-122">That's it, that completes this tutorial.</span></span> <span data-ttu-id="8f44f-123">Další informace jak toomanage hello konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="8f44f-123">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="8f44f-124">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="8f44f-124">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f44f-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f44f-125">Next steps</span></span>

<span data-ttu-id="8f44f-126">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="8f44f-126">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8f44f-127">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8f44f-127">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="8f44f-128">Nakonfigurujte globální distribuci pomocí hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8f44f-128">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="8f44f-129">Nyní můžete přejít toohello další kurz toolearn jak hello toodevelop místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8f44f-129">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8f44f-130">Vývoj místně pomocí emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="8f44f-130">Develop locally with hello emulator</span></span>](local-emulator.md)
