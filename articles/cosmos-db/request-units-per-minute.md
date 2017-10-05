---
title: "Azure CosmosDB: Požadované jednotky za minutu (RU/m) | Microsoft Docs"
description: "Zjistěte, jak snížit náklady na s využitím jednotek žádosti za minutu."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 72d60d5656ad664e42a848fc9b372cb09e49b888
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a><span data-ttu-id="9d673-103">Jednotek žádosti za minutu v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9d673-103">Request units per minute in Azure Cosmos DB</span></span>

<span data-ttu-id="9d673-104">Azure Cosmos DB slouží k vám pomohou dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s růstem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d673-104">Azure Cosmos DB is designed to help you achieve a fast, predictable performance and scale seamlessly along with your application’s growth.</span></span> <span data-ttu-id="9d673-105">Můžete zřídit propustnosti na kontejner Cosmos DB v obou, za sekundu a členitostí za minutu (RU/m).</span><span class="sxs-lookup"><span data-stu-id="9d673-105">You can provision throughput on a Cosmos DB container at both, per-second and at per-minute (RU/m) granularities.</span></span> <span data-ttu-id="9d673-106">Zřízená propustnost v přírůstcích po jedné minutě slouží k zvládání nečekaných nárůstů úloh, které nastávají v přírůstcích po jedné sekundě.</span><span class="sxs-lookup"><span data-stu-id="9d673-106">The provisioned throughput at per-minute granularity is used to manage unexpected spikes in the workload occurring at a per-second granularity.</span></span> 

<span data-ttu-id="9d673-107">Tento článek obsahuje přehled toho, jak funguje zřizování jednotek žádosti za minutu (RU/m).</span><span class="sxs-lookup"><span data-stu-id="9d673-107">This article provides an overview of how the provisioning of Request Unit per Minute (RU/m) works.</span></span> <span data-ttu-id="9d673-108">Cílem v paměti se zřizováním RU/m je zajistit předvídatelný výkon kolem nepředvídatelným potřebám (obzvláště pokud potřebujete spustit analytics nad provozních dat) a spiky úlohy.</span><span class="sxs-lookup"><span data-stu-id="9d673-108">The goal in mind with provisioning of RU/m is to provide a predictable performance around unpredictable needs (especially if you need to run analytics on top of your operational data) and spiky workloads.</span></span> <span data-ttu-id="9d673-109">Chceme mít naše zákazníky využívat další, které zřizování, můžete rychle škálování se jistotu propustnost.</span><span class="sxs-lookup"><span data-stu-id="9d673-109">We want to have our customers consume more the throughput they provision so they can scale quickly with peace of mind.</span></span>

<span data-ttu-id="9d673-110">Po přečtení tohoto článku, budete moct odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="9d673-110">After reading this article, you will be able to answer the following questions:</span></span>

* <span data-ttu-id="9d673-111">Funkce jednotek žádosti za minutu</span><span class="sxs-lookup"><span data-stu-id="9d673-111">How does a Request Unit per Minute work?</span></span>
* <span data-ttu-id="9d673-112">Jaký je rozdíl mezi jednotek žádosti za minutu a jednotek žádosti za sekundu?</span><span class="sxs-lookup"><span data-stu-id="9d673-112">What is the difference between Request Unit per Minute and Request Unit per Second?</span></span>
* <span data-ttu-id="9d673-113">Jak zřídit RU/m?</span><span class="sxs-lookup"><span data-stu-id="9d673-113">How to provision RU/m?</span></span>
* <span data-ttu-id="9d673-114">V části scénář, který je nutné zvážit zřizování jednotek žádosti za minutu?</span><span class="sxs-lookup"><span data-stu-id="9d673-114">Under which scenario shall I consider provisioning Request Unit per Minute?</span></span>
* <span data-ttu-id="9d673-115">Postup použití portálu metrik s cílem optimalizovat Moje nákladů a výkonu?</span><span class="sxs-lookup"><span data-stu-id="9d673-115">How to use the portal metrics to optimize my cost and performance?</span></span>
* <span data-ttu-id="9d673-116">Zadejte, jaký typ požadavku můžete využívat rozpočtem RU/m?</span><span class="sxs-lookup"><span data-stu-id="9d673-116">Define which type of request can consume your RU/m budget?</span></span>

## <a name="provisioning-request-units-per-minute-rum"></a><span data-ttu-id="9d673-117">Zřizování jednotek žádosti za minutu (RU/m)</span><span class="sxs-lookup"><span data-stu-id="9d673-117">Provisioning request units per minute (RU/m)</span></span>

<span data-ttu-id="9d673-118">Pokud zřizujete Azure Cosmos DB v druhé členitost (RU/s), získáte záruka, že vaši žádost o úspěšná na nízkou latencí, pokud vaše propustnost nebyla překročena kapacitu poskytnutém v rámci této sekundu.</span><span class="sxs-lookup"><span data-stu-id="9d673-118">When you provision Azure Cosmos DB at the second granularity (RU/s), you get the guarantee that your request succeeds at a low latency if your throughput has not exceeded the capacity provisioned within that second.</span></span> <span data-ttu-id="9d673-119">RU/m členitost je na chvíli se zárukou, který neproběhne v rámci této minutu.</span><span class="sxs-lookup"><span data-stu-id="9d673-119">With RU/m, the granularity is at the minute with the guarantee that your request succeeds within that minute.</span></span> <span data-ttu-id="9d673-120">Porovnání s bursting systémy, zajišťujeme, že, výkon, které máte je předvídatelný a můžete naplánovat na něm.</span><span class="sxs-lookup"><span data-stu-id="9d673-120">Compared to bursting systems, we make sure that the performance you get is predictable and you can plan on it.</span></span>

<span data-ttu-id="9d673-121">Způsob, jakým za minutu zřizování funguje je jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="9d673-121">The way per minute provisioning works is simple:</span></span>

* <span data-ttu-id="9d673-122">RU/m se fakturuje každou hodinu a kromě RU/s.</span><span class="sxs-lookup"><span data-stu-id="9d673-122">RU/m is billed hourly and in addition to RU/s.</span></span> <span data-ttu-id="9d673-123">Další podrobnosti naleznete na adrese Azure Cosmos DB [stránce s cenami](https://aka.ms/acdbpricing).</span><span class="sxs-lookup"><span data-stu-id="9d673-123">For more details, please visit Azure Cosmos DB [pricing page](https://aka.ms/acdbpricing).</span></span>
* <span data-ttu-id="9d673-124">RU/m se dá nastavit na úrovni kolekce.</span><span class="sxs-lookup"><span data-stu-id="9d673-124">RU/m can be enabled at collection level.</span></span> <span data-ttu-id="9d673-125">To můžete udělat pomocí sady SDK (Node.js, Java nebo .net) nebo prostřednictvím portálu (také obsahovat úlohy MongoDB rozhraní API)</span><span class="sxs-lookup"><span data-stu-id="9d673-125">That can be done through the SDKs (Node.js, Java, or .Net) or through the portal (also include MongoDB API workloads)</span></span>
* <span data-ttu-id="9d673-126">Pokud je povoleno RU/m, pro každých 100 RU/s zřízený, získáte 1000 RU/m zřízený (poměr je 10 x)</span><span class="sxs-lookup"><span data-stu-id="9d673-126">When RU/m is enabled, for every 100 RU/s provisioned, you also get 1,000 RU/m provisioned (the ratio is 10x)</span></span>
* <span data-ttu-id="9d673-127">V dané druhý, jednotka žádosti využívá vaší RU/m zřizování pouze v případě, že jste překročili vaší za druhé zřizování v rámci této sekundu</span><span class="sxs-lookup"><span data-stu-id="9d673-127">At a given second, a request unit consumes your RU/m provisioning only if you have exceeded your per second provisioning within that second</span></span>
* <span data-ttu-id="9d673-128">Po skončení doby 60 sekund (UTC), za minutu zřizování je opakovaného plnění</span><span class="sxs-lookup"><span data-stu-id="9d673-128">Once the 60-second period (UTC) ends, the per minute provisioning is refilled</span></span>
* <span data-ttu-id="9d673-129">RU/m lze povolit pouze pro kolekce s maximální zřizování 5 000 RU/s na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="9d673-129">RU/m can be enabled only for collections with a maximum provisioning of 5,000 RU/s per partition.</span></span> <span data-ttu-id="9d673-130">Pokud velikost potřeb propustnost a mají vysokou úroveň zřizování na oddíl, zobrazí se zpráva s upozorněním</span><span class="sxs-lookup"><span data-stu-id="9d673-130">If you scale your throughput needs and have such a high level of provisioning per partition, you will get a warning message</span></span>

<span data-ttu-id="9d673-131">Níže je konkrétní příklad, ve kterém můžete zřídit zákazník 10kRU/s s 100kRU/m, ukládání 73 % náklady proti zřizování ve špičce (na 50kRU za sekundu) prostřednictvím dobou 90 sekundu v kolekci, která má 10 000 RU/s a 100 000 RU/m zřízené:</span><span class="sxs-lookup"><span data-stu-id="9d673-131">Below is a concrete example, in which a customer can provision 10kRU/s with 100kRU/m, saving 73% in cost against provisioning for peak (at 50kRU/sec) through a 90-second period on a collection that has 10,000 RU/s and 100,000 RU/m provisioned:</span></span>

* <span data-ttu-id="9d673-132">1 sekundu: RU/m rozpočtu je nastavený na 100 000</span><span class="sxs-lookup"><span data-stu-id="9d673-132">1st second: The RU/m budget is set at 100,000</span></span>
* <span data-ttu-id="9d673-133">3. sekundu: Při tomto sekundu spotřeby jednotek žádosti byl 11,010 RUs, 1,010 RUs výše zřizování RU/s.</span><span class="sxs-lookup"><span data-stu-id="9d673-133">3rd second: During that second the consumption of Request Unit was 11,010 RUs, 1,010 RUs above the RU/s provisioning.</span></span> <span data-ttu-id="9d673-134">Proto jsou 1,010 RUs odečtena z rozpočtu RU/m.</span><span class="sxs-lookup"><span data-stu-id="9d673-134">Therefore, 1,010 RUs are deducted from the RU/m budget.</span></span> <span data-ttu-id="9d673-135">98,990 RUs jsou k dispozici pro další 57 sekund v rozpočtu RU/m</span><span class="sxs-lookup"><span data-stu-id="9d673-135">98,990 RUs are available for the next 57 seconds in the RU/m budget</span></span>
* <span data-ttu-id="9d673-136">29 sekundu: během druhé, došlo k velké Špička (> 4 x vyšší než zřizování za sekundu) a spotřeby jednotek žádosti byl 46,920 RUs.</span><span class="sxs-lookup"><span data-stu-id="9d673-136">29th second: During that second, a large spike happened (>4x higher than provisioning per second) and the consumption of Request Unit was 46,920 RUs.</span></span> <span data-ttu-id="9d673-137">36,920 RUs jsou odečtena z rozpočtu RU/m vyřadit z 92,323 RUs (28th sekundu) k 55,403 RUs (29 sekundu)</span><span class="sxs-lookup"><span data-stu-id="9d673-137">36,920 RUs are deducted from the RU/m budget that dropped from 92,323 RUs (28th second) to 55,403 RUs (29th second)</span></span>
* <span data-ttu-id="9d673-138">61st sekundu: RU/m nároky se nastaví zpátky na 100 000 RUs.</span><span class="sxs-lookup"><span data-stu-id="9d673-138">61st second: RU/m budget is set back to 100,000 RUs.</span></span>
 
![Graf zobrazující spotřeby a zřizování Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a><span data-ttu-id="9d673-140">Určení požadavku jednotka kapacity pomocí RU/m</span><span class="sxs-lookup"><span data-stu-id="9d673-140">Specifying request unit capacity with RU/m</span></span>

<span data-ttu-id="9d673-141">Při vytváření kolekci Azure Cosmos DB, je třeba zadat počet jednotek žádosti za sekundu (RU za sekundu), kterou chcete vyhrazené pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="9d673-141">When creating an Azure Cosmos DB collection, you specify the number of request units per second (RU per second) you want reserved for the collection.</span></span> <span data-ttu-id="9d673-142">Můžete také rozhodnout, pokud chcete přidat RU za minutu.</span><span class="sxs-lookup"><span data-stu-id="9d673-142">You can also decide if you want to add RU per minute.</span></span> <span data-ttu-id="9d673-143">To lze provést prostřednictvím portálu nebo sady SDK.</span><span class="sxs-lookup"><span data-stu-id="9d673-143">This can be done through the Portal or the SDK.</span></span> 

### <a name="through-the-portal"></a><span data-ttu-id="9d673-144">Prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="9d673-144">Through the Portal</span></span>

<span data-ttu-id="9d673-145">Povolení nebo zakázání RU za minutu jednoduše vyžaduje kliknutí při zřizování kolekce.</span><span class="sxs-lookup"><span data-stu-id="9d673-145">Enabling or disabling RU per minute simply requires a click when provisioning a collection.</span></span> 

 ![Snímek obrazovky ukazující, jak nastavit RU/m na portálu Azure](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-the-sdk"></a><span data-ttu-id="9d673-147">Prostřednictvím sady SDK</span><span class="sxs-lookup"><span data-stu-id="9d673-147">Through the SDK</span></span>
<span data-ttu-id="9d673-148">První to je důležité si uvědomit, že RU/m je dostupná jenom pro následující sady SDK:</span><span class="sxs-lookup"><span data-stu-id="9d673-148">First, this is important to note that RU/m is only available for the following SDKs:</span></span>

* <span data-ttu-id="9d673-149">Rozhraní .net 1.14.0</span><span class="sxs-lookup"><span data-stu-id="9d673-149">.Net 1.14.0</span></span>
* <span data-ttu-id="9d673-150">Java 1.11.0</span><span class="sxs-lookup"><span data-stu-id="9d673-150">Java 1.11.0</span></span>
* <span data-ttu-id="9d673-151">Node.js 1.12.0</span><span class="sxs-lookup"><span data-stu-id="9d673-151">Node.js 1.12.0</span></span>
* <span data-ttu-id="9d673-152">Python 2.2.0</span><span class="sxs-lookup"><span data-stu-id="9d673-152">Python 2.2.0</span></span>

<span data-ttu-id="9d673-153">Zde je fragment kódu pro vytvoření kolekce s 3 000 jednotek žádosti za jednotek žádosti druhý a 30 000 za minutu pomocí sady .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="9d673-153">Here is a code snippet for creating a collection with 3,000 request units per second and 30,000 request units per minute using the .NET SDK:</span></span>

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set the throughput to 3,000 request units per second which will give you 30,000 request units per minute as the RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

<span data-ttu-id="9d673-154">Zde je fragment kódu pro změnu propustnost kolekce do 5 000 jednotek žádosti za sekundu bez zřizování RU za minutu pomocí sady .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="9d673-154">Here is a code snippet for changing the throughput of a collection to 5,000 request units per second without provisioning RU per minute using the .NET SDK:</span></span>

```csharp
// Get the current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to 5000 request units per second without RU/m enabled (the last parameter to OfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a><span data-ttu-id="9d673-155">Dobré podle scénáře</span><span class="sxs-lookup"><span data-stu-id="9d673-155">Good fit scenarios</span></span>

<span data-ttu-id="9d673-156">V této části poskytujeme přehled scénářů, které jsou vhodné pro povolení jednotek žádosti za minutu.</span><span class="sxs-lookup"><span data-stu-id="9d673-156">In this section, we provide an overview of scenarios that are a good fit for enabling request units per minute.</span></span>

<span data-ttu-id="9d673-157">**Prostředí pro vývoj/testování:** vhodné.</span><span class="sxs-lookup"><span data-stu-id="9d673-157">**Dev/Test environment:** Good fit.</span></span> <span data-ttu-id="9d673-158">Během fáze vývoje Pokud testujete aplikace s různé úlohy, RU/m nabízejí flexibilitu v této fázi.</span><span class="sxs-lookup"><span data-stu-id="9d673-158">During the development stage, if you are testing your application with different workloads, RU/m can provide the flexibility at this stage.</span></span> <span data-ttu-id="9d673-159">Když [emulátoru](local-emulator.md) je skvělým bezplatný nástroj k testování Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9d673-159">While the [emulator](local-emulator.md) is a great free tool to test Azure Cosmos DB.</span></span> <span data-ttu-id="9d673-160">Ale pokud chcete spustit v cloudovém prostředí, budete mít flexibilitu s RU/m pro vašim požadavkům na výkon ad hoc.</span><span class="sxs-lookup"><span data-stu-id="9d673-160">However if you want to start in a cloud environment, you will have a great flexibility with RU/m for your adhoc performance needs.</span></span> <span data-ttu-id="9d673-161">Můžete se věnovat více času, méně plně soustředit na první požadavkům na výkon.</span><span class="sxs-lookup"><span data-stu-id="9d673-161">You will spend more time developing, less worrying about performance needs at first.</span></span> <span data-ttu-id="9d673-162">Doporučujeme od minimální zřizování RU/s a povolit RU/m.</span><span class="sxs-lookup"><span data-stu-id="9d673-162">We recommend starting with the minimum RU/s provisioning and enable RU/m.</span></span>

<span data-ttu-id="9d673-163">**Musí nepředvídatelným, spiky, minutu členitost:** dobré nevejde – úspory: 25 75 %.</span><span class="sxs-lookup"><span data-stu-id="9d673-163">**Unpredictable, spiky, minute granularity needs:** Good fit – Savings: 25-75%.</span></span> <span data-ttu-id="9d673-164">Jsme viděli velké zlepšování z RU/m a většině produkčních scénářích se do této skupiny.</span><span class="sxs-lookup"><span data-stu-id="9d673-164">We have seen large improvement from RU/m and most production scenarios are into that group.</span></span> <span data-ttu-id="9d673-165">Pokud máte IoT pracovního vytížení, který má Špička několikrát za minutu, pokud máte dotazy spuštěn, když se váš systém díky hromadného vložení ve stejnou dobu, budete potřebovat víc kapacity pro potřeby spiky handeling.</span><span class="sxs-lookup"><span data-stu-id="9d673-165">If you have an IoT workload that has spike a few times in a minute, if you have queries running when your system makes mass insert at the same time, you will need extra capacity for handeling spiky needs.</span></span> <span data-ttu-id="9d673-166">Doporučujeme, abyste optimalizace potřeb prostředků s použitím náš přístup krok za krokem níže.</span><span class="sxs-lookup"><span data-stu-id="9d673-166">We recommend optimizing your resource needs by applying our step by step approach below.</span></span>

 ![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 <span data-ttu-id="9d673-168">*Obrázek - RU spotřeba srovnávacího testu*</span><span class="sxs-lookup"><span data-stu-id="9d673-168">*Figure - RU consumption benchmark*</span></span>

<span data-ttu-id="9d673-169">**Jistotu:** dobré nevejde – úspory: 10-20 %.</span><span class="sxs-lookup"><span data-stu-id="9d673-169">**Peace of mind:** Good fit – Savings: 10-20%.</span></span> <span data-ttu-id="9d673-170">V některých případech jenom chcete mít jistotu a nestarat se o potenciální vrcholů a omezení.</span><span class="sxs-lookup"><span data-stu-id="9d673-170">Sometimes, you just want to have peace of mind and not worry about potential peaks and throttling.</span></span> <span data-ttu-id="9d673-171">Tato funkce je ten správný za vás.</span><span class="sxs-lookup"><span data-stu-id="9d673-171">This feature is the right one for you.</span></span> <span data-ttu-id="9d673-172">V takovém případě doporučujeme povolení RU/m a mírně nižší vaší za druhé zřizování.</span><span class="sxs-lookup"><span data-stu-id="9d673-172">In that case, we recommend enabling RU/m and slightly lower your per second provisioning.</span></span> <span data-ttu-id="9d673-173">Tento případ se liší od výše jako nebude zkoušet za účelem optimalizace intenzivně zřizování.</span><span class="sxs-lookup"><span data-stu-id="9d673-173">This case is different from the above as you will not try to optimize aggressively your provisioning.</span></span> <span data-ttu-id="9d673-174">To je více přistupovat "Nula omezení", který se v.</span><span class="sxs-lookup"><span data-stu-id="9d673-174">This is more of a “Zero Throttling” mindset you are in.</span></span>

<span data-ttu-id="9d673-175">Kritické operace s potřebami ad hoc: doporučujeme někdy umožníte pouze kritické operace přístupu RU/m rozpočtu, není-li získat rozpočtu využívat ad hoc nebo méně důležité operace.</span><span class="sxs-lookup"><span data-stu-id="9d673-175">Critical operations with adhoc needs: We sometimes recommend to only let critical operations access RU/m budget so the budget doesn’t get consume by adhoc or less important operations.</span></span> <span data-ttu-id="9d673-176">Který může být snadno definován v následující části.</span><span class="sxs-lookup"><span data-stu-id="9d673-176">That can be easily defined in the section below.</span></span>

## <a name="using-the-portal-metrics-to-optimize-cost-and-performance"></a><span data-ttu-id="9d673-177">Pomocí portálu metriky za účelem optimalizace nákladů a výkonu</span><span class="sxs-lookup"><span data-stu-id="9d673-177">Using the portal metrics to optimize cost and performance</span></span>

<span data-ttu-id="9d673-178">**V následujících týdnech jsme další vyvíjet obsah kolem monitorování RUs minutu spotřeby za účelem optimalizace potřeb propustnost.**</span><span class="sxs-lookup"><span data-stu-id="9d673-178">**In the coming weeks, we will further develop the content around monitoring RUs minute consumption to optimize your throughput needs.**</span></span>

<span data-ttu-id="9d673-179">Prostřednictvím portálu metriky můžete zobrazit, kolik regulární RU sekund využívat versus RU minut.</span><span class="sxs-lookup"><span data-stu-id="9d673-179">Through the portal metrics, you can see how much of regular RU seconds you consume versus RU minutes.</span></span> <span data-ttu-id="9d673-180">Monitorování tyto metriky by měly pomoci při optimalizaci vašeho zřizování.</span><span class="sxs-lookup"><span data-stu-id="9d673-180">Monitoring these metrics should help you optimize your provisioning.</span></span> 

<span data-ttu-id="9d673-181">Doporučujeme, abyste krok za krokem přístup k použití RU/m využít.</span><span class="sxs-lookup"><span data-stu-id="9d673-181">We recommend a step by step approach on how to use RU/m to your advantage.</span></span> <span data-ttu-id="9d673-182">Při každém kroku byste měli mít přehled o využívání RU představující úplný cyklus úlohy (to může být hodiny, dny, nebo i týdny) a získat informace o využití těchto můžete zřídit.</span><span class="sxs-lookup"><span data-stu-id="9d673-182">For each step, you should have an overview of the RU consumption representing a full cycle of your workload (it could be hours, days, or even weeks) and get insights on the utilization of what you provision.</span></span>

<span data-ttu-id="9d673-183">Princip za tento přístup je vaše propustnost zřizování co nejblíže k zřizování bodu, který odpovídá kritériím výkonu níže.</span><span class="sxs-lookup"><span data-stu-id="9d673-183">The principle behind this approach is to make your throughput provisioning as close as possible to a provisioning point that matches your performance criteria below.</span></span> 

![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
<span data-ttu-id="9d673-185">Pokud chcete pochopit optimální zřizování bod pro úlohy, je potřeba pochopit:</span><span class="sxs-lookup"><span data-stu-id="9d673-185">To understand the optimal provisioning point for your workload, you need to understand:</span></span>

* <span data-ttu-id="9d673-186">Spotřeba vzory: žádné, nepravidelným nebo dlouhodobě špičky?</span><span class="sxs-lookup"><span data-stu-id="9d673-186">Consumption patterns: no, infrequent or sustained spikes?</span></span> <span data-ttu-id="9d673-187">(2 x průměr) malá, střední a velké (> 10 x průměr) špičky?</span><span class="sxs-lookup"><span data-stu-id="9d673-187">Small (2x average), medium, or large (>10x average) spikes?</span></span>
* <span data-ttu-id="9d673-188">Procenta omezenému požadavků: udělat si projděte Pokud máte chvilku omezování?</span><span class="sxs-lookup"><span data-stu-id="9d673-188">Percent of throttled requests: do you feel comfortable if you have a bit of throttling?</span></span> <span data-ttu-id="9d673-189">Pokud ano, jak moc?</span><span class="sxs-lookup"><span data-stu-id="9d673-189">If so, by how much?</span></span> 

<span data-ttu-id="9d673-190">Po zjištění, jaké jsou vaše cíle, bude možné získat blíže optimální zřizování.</span><span class="sxs-lookup"><span data-stu-id="9d673-190">Once you have identified what your goals are, you will be able to get closer to the optimal provisioning.</span></span>

<span data-ttu-id="9d673-191">K usnadnění, chceme poskytovat celkové pokyny o tom, jak optimalizovat vaše zřizování na základě vaší spotřeby RU/m.</span><span class="sxs-lookup"><span data-stu-id="9d673-191">To assist you, we want to provide an overall guidance on how to optimize your provisioning based on your RU/m consumption.</span></span> <span data-ttu-id="9d673-192">V tomto návodu se nevztahuje na všechny druh úloh, ale je založena na znalosti privátní Preview verzi.</span><span class="sxs-lookup"><span data-stu-id="9d673-192">This guidance doesn’t apply to all kind of workloads but is based on the private preview knowledge.</span></span> <span data-ttu-id="9d673-193">Může se nám změnit tyto standardní hodnoty jako jsme Další informace:</span><span class="sxs-lookup"><span data-stu-id="9d673-193">We might change such baselines as we learn more:</span></span>

|<span data-ttu-id="9d673-194">% Využití RU/m</span><span class="sxs-lookup"><span data-stu-id="9d673-194">RU/m % utilization</span></span>|<span data-ttu-id="9d673-195">Úroveň využití RU/m</span><span class="sxs-lookup"><span data-stu-id="9d673-195">Degree of utilization of RU/m</span></span>|<span data-ttu-id="9d673-196">Doporučené akce pro zřizování</span><span class="sxs-lookup"><span data-stu-id="9d673-196">Recommended actions for provisioning</span></span>|
|---|---|---|
|<span data-ttu-id="9d673-197">0-1%</span><span class="sxs-lookup"><span data-stu-id="9d673-197">0-1%</span></span>|<span data-ttu-id="9d673-198">V části využití</span><span class="sxs-lookup"><span data-stu-id="9d673-198">Under utilization</span></span>|<span data-ttu-id="9d673-199">Nižší využívat další RU/m. RU/s</span><span class="sxs-lookup"><span data-stu-id="9d673-199">Lower RU/s to consume more RU/m</span></span>|
|<span data-ttu-id="9d673-200">1-10%</span><span class="sxs-lookup"><span data-stu-id="9d673-200">1-10%</span></span>|<span data-ttu-id="9d673-201">Použití v pořádku</span><span class="sxs-lookup"><span data-stu-id="9d673-201">Healthy use</span></span>|<span data-ttu-id="9d673-202">Zachovat na stejné úrovni zřizování</span><span class="sxs-lookup"><span data-stu-id="9d673-202">Keep the same provisioning level</span></span>|
|<span data-ttu-id="9d673-203">Více než 10 %</span><span class="sxs-lookup"><span data-stu-id="9d673-203">Above 10%</span></span>|<span data-ttu-id="9d673-204">Přes využití</span><span class="sxs-lookup"><span data-stu-id="9d673-204">Over utilization</span></span>|<span data-ttu-id="9d673-205">Zvýšit spolehnout menší na RU/m. RU/s</span><span class="sxs-lookup"><span data-stu-id="9d673-205">Increase RU/s to rely less on RU/m</span></span>|

## <a name="select-which-operations-can-consume-the-rum-budget"></a><span data-ttu-id="9d673-206">Vyberte operací, které můžou využívat nároky RU/m</span><span class="sxs-lookup"><span data-stu-id="9d673-206">Select which operations can consume the RU/m budget</span></span>

<span data-ttu-id="9d673-207">Na žádost o úrovni můžete můžete také povolit nebo zakázat RU/m nároky na požadavek bez ohledu na typ operaci vyřídit.</span><span class="sxs-lookup"><span data-stu-id="9d673-207">At request level, you can also enable/disable RU/m budget to serve the request irrespective of operation type.</span></span> <span data-ttu-id="9d673-208">Pokud se využívá regulární zřízené nároky RUs za sekundu a požadavek nelze využívat nároky RU/m, budou omezeny tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="9d673-208">If regular provisioned RUs/sec budget is consumed and the request cannot consume the RU/m budget, this request will be throttled.</span></span> <span data-ttu-id="9d673-209">Ve výchozím nastavení každá žádost obsloužených RU/m rozpočtu, pokud je aktivovaná nároky propustnost RU/m.</span><span class="sxs-lookup"><span data-stu-id="9d673-209">By default, any request is served by RU/m budget if RU/m throughput budget is activated.</span></span> 

<span data-ttu-id="9d673-210">Zde je fragment kódu pro zakázání RU/m nároky pomocí DocumentDB rozhraní API pro operace CRUD a dotazu.</span><span class="sxs-lookup"><span data-stu-id="9d673-210">Here is a code snippet for disabling RU/m budget using the DocumentDB API for CRUD and query operations.</span></span>

```csharp
// In order to disable any CRUD request for RU/m, set DisableRUPerMinuteUsage to true in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order to disable any query request for RU/m, set DisableRUPerMinuteOnRequest to true in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a><span data-ttu-id="9d673-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d673-211">Next steps</span></span>

<span data-ttu-id="9d673-212">V tomto článku jsme popsali, jak rozdělení funguje v Azure Cosmos DB, jak můžete vytvořit dělené kolekce a jak můžete vybrat vhodným klíčem oddílu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d673-212">In this article, we've described how partitioning works in Azure Cosmos DB, how you can create partitioned collections, and how you can pick a good partition key for your application.</span></span>

* <span data-ttu-id="9d673-213">Proveďte škálování a výkon testování pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9d673-213">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="9d673-214">V tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md) pro ukázku.</span><span class="sxs-lookup"><span data-stu-id="9d673-214">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="9d673-215">Začínáme s kódování [sady SDK](documentdb-sdk-dotnet.md) nebo [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="9d673-215">Get started coding with the [SDKs](documentdb-sdk-dotnet.md) or the [REST API](/rest/api/documentdb/).</span></span>
* <span data-ttu-id="9d673-216">Další informace o [zřízené propustnosti](request-units.md) v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9d673-216">Learn about [provisioned throughput](request-units.md) in Azure Cosmos DB</span></span> 

