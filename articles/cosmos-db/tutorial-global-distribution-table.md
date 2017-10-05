---
title: "Globální distribuční kurz pro Azure Cosmos DB pro tabulku API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API tabulky."
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
ms.openlocfilehash: 63c9e530a4982e2e6e478fea56e015fc77851e1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a><span data-ttu-id="fcb48-104">Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro tabulky</span><span class="sxs-lookup"><span data-stu-id="fcb48-104">How to setup Azure Cosmos DB global distribution using the Table API</span></span>

<span data-ttu-id="fcb48-105">V tomto článku jsme ukazují, jak pomocí portálu Azure do instalačního programu globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní API pro tabulku (preview).</span><span class="sxs-lookup"><span data-stu-id="fcb48-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the Table API (preview).</span></span>

<span data-ttu-id="fcb48-106">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="fcb48-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="fcb48-107">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fcb48-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="fcb48-108">Nakonfigurovat globální distribuční [tabulky rozhraní API](table-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="fcb48-108">Configure global distribution using the [Table API](table-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a><span data-ttu-id="fcb48-109">Připojování k upřednostňovaná oblast pomocí rozhraní API pro tabulky</span><span class="sxs-lookup"><span data-stu-id="fcb48-109">Connecting to a preferred region using the Table API</span></span>

<span data-ttu-id="fcb48-110">Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fcb48-110">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="fcb48-111">To můžete provést nastavením `TablePreferredLocations` hodnota konfigurace v souboru config aplikace sada SDK úložiště Azure ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="fcb48-111">This can be done by setting the `TablePreferredLocations` configuration value in the app config for the preview Azure Storage SDK.</span></span> <span data-ttu-id="fcb48-112">Na základě konfigurace účtu Azure Cosmos DB, aktuální místní dostupnosti a seznamu předvoleb zadán, bude vybrána optimální koncový bod pomocí sady SDK úložiště Azure a provedení operace zápisu operace čtení.</span><span class="sxs-lookup"><span data-stu-id="fcb48-112">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the Azure Storage SDK to perform write and read operations.</span></span>

<span data-ttu-id="fcb48-113">`TablePreferredLocations` By měla obsahovat čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení.</span><span class="sxs-lookup"><span data-stu-id="fcb48-113">The `TablePreferredLocations` should contain a comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="fcb48-114">Každá instance klienta můžete určit podmnožinu těchto oblastí v upřednostňovaném pořadí pro čtení s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="fcb48-114">Each client instance can specify a subset of these regions in the preferred order for low latency reads.</span></span> <span data-ttu-id="fcb48-115">Oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`.</span><span class="sxs-lookup"><span data-stu-id="fcb48-115">The regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span>

<span data-ttu-id="fcb48-116">Všechny operace čtení odešle první dostupné oblasti v `TablePreferredLocations` seznamu.</span><span class="sxs-lookup"><span data-stu-id="fcb48-116">All reads will be sent to the first available region in the `TablePreferredLocations` list.</span></span> <span data-ttu-id="fcb48-117">Pokud se požadavek nezdaří, klient se nezdaří dolů v seznamu další oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="fcb48-117">If the request fails, the client will fail down the list to the next region, and so on.</span></span>

<span data-ttu-id="fcb48-118">Sada SDK bude pouze pokus o čtení z oblastí, zadaný v `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="fcb48-118">The SDK will only attempt to read from the regions specified in `TablePreferredLocations`.</span></span> <span data-ttu-id="fcb48-119">Ano, například pokud databázový účet je k dispozici v tři oblasti, ale klient určuje pouze dva oblastí bez zápisu pro `TablePreferredLocations`, pak se zpracuje žádné čtení z oblasti zápisu i v případě převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fcb48-119">So, for example, if the Database Account is available in three regions, but the client only specifies two of the non-write regions for `TablePreferredLocations`, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="fcb48-120">Sada SDK automaticky odesílat všechny zápisy na aktuální zápisu oblast.</span><span class="sxs-lookup"><span data-stu-id="fcb48-120">The SDK will automatically send all writes to the current write region.</span></span>

<span data-ttu-id="fcb48-121">Pokud `TablePreferredLocations` není nastavena vlastnost, z aktuální oblasti zápisu se zpracuje všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="fcb48-121">If the `TablePreferredLocations` property is not set, all requests will be served from the current write region.</span></span>

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

<span data-ttu-id="fcb48-122">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fcb48-122">That's it, that completes this tutorial.</span></span> <span data-ttu-id="fcb48-123">Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="fcb48-123">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="fcb48-124">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="fcb48-124">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcb48-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fcb48-125">Next steps</span></span>

<span data-ttu-id="fcb48-126">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="fcb48-126">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fcb48-127">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fcb48-127">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="fcb48-128">Nakonfigurujte globální distribuční pomocí rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="fcb48-128">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="fcb48-129">Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fcb48-129">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fcb48-130">Vývoj místně pomocí emulátoru</span><span class="sxs-lookup"><span data-stu-id="fcb48-130">Develop locally with the emulator</span></span>](local-emulator.md)