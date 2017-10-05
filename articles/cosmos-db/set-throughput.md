---
title: "Zřízení propustnost pro Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak nastavit zřízená propustnost pro containsers, kolekce, grafy a tabulky Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: d541bb19ba7e5ecb44c9fe91b1e232d4d9c2170e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a><span data-ttu-id="5a987-103">Nastavit propustnost pro Azure Cosmos DB kontejnery</span><span class="sxs-lookup"><span data-stu-id="5a987-103">Set throughput for Azure Cosmos DB containers</span></span>

<span data-ttu-id="5a987-104">Propustnost lze nastavit pro Azure Cosmos DB kontejnerů na portálu Azure nebo pomocí sady SDK klienta.</span><span class="sxs-lookup"><span data-stu-id="5a987-104">You can set throughput for your Azure Cosmos DB containers in the Azure portal or by using the client SDKs.</span></span> 

<span data-ttu-id="5a987-105">Následující tabulka uvádí k dispozici pro kontejnery propustnost:</span><span class="sxs-lookup"><span data-stu-id="5a987-105">The following table lists the throughput available for containers:</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><span data-ttu-id="5a987-106"><strong>Jeden oddíl kontejneru</strong></span><span class="sxs-lookup"><span data-stu-id="5a987-106"><strong>Single Partition Container</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="5a987-107"><strong>Oddílů kontejneru</strong></span><span class="sxs-lookup"><span data-stu-id="5a987-107"><strong>Partitioned Container</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="5a987-108">Minimální propustnost</span><span class="sxs-lookup"><span data-stu-id="5a987-108">Minimum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="5a987-109">400 jednotek žádosti za sekundu</span><span class="sxs-lookup"><span data-stu-id="5a987-109">400 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="5a987-110">2 500 jednotek žádosti za sekundu</span><span class="sxs-lookup"><span data-stu-id="5a987-110">2,500 request units per second</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="5a987-111">Maximální propustnost</span><span class="sxs-lookup"><span data-stu-id="5a987-111">Maximum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="5a987-112">10 000 jednotek žádosti za sekundu</span><span class="sxs-lookup"><span data-stu-id="5a987-112">10,000 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="5a987-113">Unlimited</span><span class="sxs-lookup"><span data-stu-id="5a987-113">Unlimited</span></span></p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a><span data-ttu-id="5a987-114">Chcete-li nastavit propustnost pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5a987-114">To set the throughput by using the Azure portal</span></span>

1. <span data-ttu-id="5a987-115">V novém okně, otevřete [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5a987-115">In a new window, open the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="5a987-116">Na levém panelu klikněte na tlačítko **Azure Cosmos DB**, nebo klikněte na tlačítko **více služeb** v dolní části, posuňte se k **databáze**a potom klikněte na **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="5a987-116">On the left bar, click **Azure Cosmos DB**, or click **More Services** at the bottom, then scroll to **Databases**, and then click **Azure Cosmos DB**.</span></span>
3. <span data-ttu-id="5a987-117">Vyberte svůj účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5a987-117">Select your Cosmos DB account.</span></span>
4. <span data-ttu-id="5a987-118">V novém okně klikněte na **Data Explorer (Preview)** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="5a987-118">In the new window, click **Data Explorer (Preview)** in the navigation menu.</span></span>
5. <span data-ttu-id="5a987-119">V novém okně rozbalte databázi a kontejneru a pak klikněte na tlačítko **škálování & nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5a987-119">In the new window, expand your database and container and then click **Scale & Settings**.</span></span>
6. <span data-ttu-id="5a987-120">V novém okně zadejte novou hodnotu propustnost v **propustnost** pole a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5a987-120">In the new window, type the new throughput value in the **Throughput** box, and then click **Save**.</span></span>

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-documentdb-api-for-net"></a><span data-ttu-id="5a987-121">Chcete-li nastavit propustnost pomocí DocumentDB rozhraní API pro .NET</span><span class="sxs-lookup"><span data-stu-id="5a987-121">To set the throughput by using the DocumentDB API for .NET</span></span>

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a><span data-ttu-id="5a987-122">Propustnost – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="5a987-122">Throughput FAQ</span></span>

<span data-ttu-id="5a987-123">**Můžete nastavit Moje propustnosti na méně než 400 RU/s**</span><span class="sxs-lookup"><span data-stu-id="5a987-123">**Can I set my throughput to less than 400 RU/s?**</span></span>

<span data-ttu-id="5a987-124">400 RU/s je k dispozici na kolekce tvořené jedním oddílem Cosmos DB (2 500 RU/s je minimální pro dělené kolekce) minimální propustnost.</span><span class="sxs-lookup"><span data-stu-id="5a987-124">400 RU/s is the minimum throughput available on Cosmos DB single partition collections (2500 RU/s is the minimum for partitioned collections).</span></span> <span data-ttu-id="5a987-125">Žádost jednotky jsou nastaveny v intervalech 100 RU/s, ale propustnost nelze nastavit na 100 RU/s nebo jakoukoli hodnotu menší než 400 RU/s.</span><span class="sxs-lookup"><span data-stu-id="5a987-125">Request units are set in 100 RU/s intervals, but throughput cannot be set to 100 RU/s or any value smaller than 400 RU/s.</span></span> <span data-ttu-id="5a987-126">Pokud hledáte nákladově efektivní metodu pro vývoj a testování Cosmos DB, můžete použít bezplatnou [emulátoru DB Cosmos Azure](local-emulator.md), které můžete nasadit místně bez nákladů.</span><span class="sxs-lookup"><span data-stu-id="5a987-126">If you're looking for a cost effective method to develop and test Cosmos DB, you can use the free [Azure Cosmos DB Emulator](local-emulator.md), which you can deploy locally at no cost.</span></span> 

<span data-ttu-id="5a987-127">**Jak lze nastavit pomocí rozhraní API MongoDB througput?**</span><span class="sxs-lookup"><span data-stu-id="5a987-127">**How do I set througput using the MongoDB API?**</span></span>

<span data-ttu-id="5a987-128">Nedojde k rozšíření rozhraní API MongoDB nastavit propustnost.</span><span class="sxs-lookup"><span data-stu-id="5a987-128">There's no MongoDB API extension to set throughput.</span></span> <span data-ttu-id="5a987-129">Doporučuje se používat rozhraní API DocumentDB, jak je znázorněno v [nastavení propustnost pomocí DocumentDB rozhraní API pro .NET](#set-throughput-sdk).</span><span class="sxs-lookup"><span data-stu-id="5a987-129">The recommendation is to use the DocumentDB API, as shown in [To set the throughput by using the DocumentDB API for .NET](#set-throughput-sdk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a987-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a987-130">Next steps</span></span>

<span data-ttu-id="5a987-131">Další informace o zřizování a probíhající planetu měřítku s Cosmos DB, najdete v části [dělení a škálování s Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="5a987-131">To learn more about provisioning and going planet-scale with Cosmos DB, see [Partitioning and scaling with Cosmos DB](partition-data.md).</span></span>
