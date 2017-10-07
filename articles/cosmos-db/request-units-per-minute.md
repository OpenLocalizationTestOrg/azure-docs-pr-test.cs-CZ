---
title: "Azure CosmosDB: Požadované jednotky za minutu (RU/m) | Microsoft Docs"
description: "Zjistěte, jak tooreduce náklady s využitím požadované jednotky za minutu."
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
ms.openlocfilehash: fcc3a92b9788750a2bfba361c3a9cebdb56eee05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a><span data-ttu-id="96903-103">Jednotek žádosti za minutu v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="96903-103">Request units per minute in Azure Cosmos DB</span></span>

<span data-ttu-id="96903-104">Azure Cosmos DB je navrženou toohelp dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s růstem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="96903-104">Azure Cosmos DB is designed toohelp you achieve a fast, predictable performance and scale seamlessly along with your application’s growth.</span></span> <span data-ttu-id="96903-105">Můžete zřídit propustnosti na kontejner Cosmos DB v obou, za sekundu a členitostí za minutu (RU/m).</span><span class="sxs-lookup"><span data-stu-id="96903-105">You can provision throughput on a Cosmos DB container at both, per-second and at per-minute (RU/m) granularities.</span></span> <span data-ttu-id="96903-106">Hello zřízené propustnosti na za minutu členitost je použité toomanage neočekávané špičky v hello úlohy, ke kterým dochází v rozlišením za sekundu.</span><span class="sxs-lookup"><span data-stu-id="96903-106">hello provisioned throughput at per-minute granularity is used toomanage unexpected spikes in hello workload occurring at a per-second granularity.</span></span> 

<span data-ttu-id="96903-107">Tento článek obsahuje přehled toho, jak funguje hello zřizování jednotek žádosti za minutu (RU/m).</span><span class="sxs-lookup"><span data-stu-id="96903-107">This article provides an overview of how hello provisioning of Request Unit per Minute (RU/m) works.</span></span> <span data-ttu-id="96903-108">cílem Hello nezapomeňte se zřizováním RU/m je tooprovide předvídatelný výkon kolem nepředvídatelným potřebám (obzvláště pokud potřebujete toorun analytics nad provozních dat) a spiky úlohy.</span><span class="sxs-lookup"><span data-stu-id="96903-108">hello goal in mind with provisioning of RU/m is tooprovide a predictable performance around unpredictable needs (especially if you need toorun analytics on top of your operational data) and spiky workloads.</span></span> <span data-ttu-id="96903-109">Chceme toohave další hello propustnost, které zřizování, můžete rychle škálování se jistotu využívat naše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="96903-109">We want toohave our customers consume more hello throughput they provision so they can scale quickly with peace of mind.</span></span>

<span data-ttu-id="96903-110">Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="96903-110">After reading this article, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="96903-111">Funkce jednotek žádosti za minutu</span><span class="sxs-lookup"><span data-stu-id="96903-111">How does a Request Unit per Minute work?</span></span>
* <span data-ttu-id="96903-112">Jaký je rozdíl hello jednotek žádosti za minutu a jednotek žádosti za sekundu?</span><span class="sxs-lookup"><span data-stu-id="96903-112">What is hello difference between Request Unit per Minute and Request Unit per Second?</span></span>
* <span data-ttu-id="96903-113">Jak tooprovision RU/m?</span><span class="sxs-lookup"><span data-stu-id="96903-113">How tooprovision RU/m?</span></span>
* <span data-ttu-id="96903-114">V části scénář, který je nutné zvážit zřizování jednotek žádosti za minutu?</span><span class="sxs-lookup"><span data-stu-id="96903-114">Under which scenario shall I consider provisioning Request Unit per Minute?</span></span>
* <span data-ttu-id="96903-115">Jak toouse hello portálu metriky toooptimize Moje nákladů a výkonu?</span><span class="sxs-lookup"><span data-stu-id="96903-115">How toouse hello portal metrics toooptimize my cost and performance?</span></span>
* <span data-ttu-id="96903-116">Zadejte, jaký typ požadavku můžete využívat rozpočtem RU/m?</span><span class="sxs-lookup"><span data-stu-id="96903-116">Define which type of request can consume your RU/m budget?</span></span>

## <a name="provisioning-request-units-per-minute-rum"></a><span data-ttu-id="96903-117">Zřizování jednotek žádosti za minutu (RU/m)</span><span class="sxs-lookup"><span data-stu-id="96903-117">Provisioning request units per minute (RU/m)</span></span>

<span data-ttu-id="96903-118">Pokud zřizujete Azure Cosmos DB v druhé členitosti hello (RU/s), získáte hello záruka, že vaši žádost o úspěšná na nízkou latencí, pokud vaše propustnost nebyla překročena hello kapacity poskytnutém v rámci této sekundu.</span><span class="sxs-lookup"><span data-stu-id="96903-118">When you provision Azure Cosmos DB at hello second granularity (RU/s), you get hello guarantee that your request succeeds at a low latency if your throughput has not exceeded hello capacity provisioned within that second.</span></span> <span data-ttu-id="96903-119">S RU/m členitosti hello je na minutu hello s hello záruka, že neproběhne v rámci této minutu.</span><span class="sxs-lookup"><span data-stu-id="96903-119">With RU/m, hello granularity is at hello minute with hello guarantee that your request succeeds within that minute.</span></span> <span data-ttu-id="96903-120">Porovnání toobursting systémy jsme Ujistěte se, že je předvídatelný výkon hello získáte a můžete naplánovat na něm.</span><span class="sxs-lookup"><span data-stu-id="96903-120">Compared toobursting systems, we make sure that hello performance you get is predictable and you can plan on it.</span></span>

<span data-ttu-id="96903-121">způsob Hello za minutu zřizování funguje je jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="96903-121">hello way per minute provisioning works is simple:</span></span>

* <span data-ttu-id="96903-122">RU/m se fakturuje každou hodinu a přidání tooRU/s.</span><span class="sxs-lookup"><span data-stu-id="96903-122">RU/m is billed hourly and in addition tooRU/s.</span></span> <span data-ttu-id="96903-123">Další podrobnosti naleznete na adrese Azure Cosmos DB [stránce s cenami](https://aka.ms/acdbpricing).</span><span class="sxs-lookup"><span data-stu-id="96903-123">For more details, please visit Azure Cosmos DB [pricing page](https://aka.ms/acdbpricing).</span></span>
* <span data-ttu-id="96903-124">RU/m se dá nastavit na úrovni kolekce.</span><span class="sxs-lookup"><span data-stu-id="96903-124">RU/m can be enabled at collection level.</span></span> <span data-ttu-id="96903-125">To můžete udělat pomocí hello sady SDK (Node.js, Java nebo .net) nebo prostřednictvím portálu hello (také obsahovat úlohy MongoDB rozhraní API)</span><span class="sxs-lookup"><span data-stu-id="96903-125">That can be done through hello SDKs (Node.js, Java, or .Net) or through hello portal (also include MongoDB API workloads)</span></span>
* <span data-ttu-id="96903-126">Pokud je povoleno RU/m, pro každých 100 RU/s zřízený, získáte 1000 RU/m zřízený (hello poměr je 10 x)</span><span class="sxs-lookup"><span data-stu-id="96903-126">When RU/m is enabled, for every 100 RU/s provisioned, you also get 1,000 RU/m provisioned (hello ratio is 10x)</span></span>
* <span data-ttu-id="96903-127">V dané druhý, jednotka žádosti využívá vaší RU/m zřizování pouze v případě, že jste překročili vaší za druhé zřizování v rámci této sekundu</span><span class="sxs-lookup"><span data-stu-id="96903-127">At a given second, a request unit consumes your RU/m provisioning only if you have exceeded your per second provisioning within that second</span></span>
* <span data-ttu-id="96903-128">Jednou hello končí dobu 60 sekund (UTC), je opakovaného plnění hello za minutu zřizování</span><span class="sxs-lookup"><span data-stu-id="96903-128">Once hello 60-second period (UTC) ends, hello per minute provisioning is refilled</span></span>
* <span data-ttu-id="96903-129">RU/m lze povolit pouze pro kolekce s maximální zřizování 5 000 RU/s na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="96903-129">RU/m can be enabled only for collections with a maximum provisioning of 5,000 RU/s per partition.</span></span> <span data-ttu-id="96903-130">Pokud velikost potřeb propustnost a mají vysokou úroveň zřizování na oddíl, zobrazí se zpráva s upozorněním</span><span class="sxs-lookup"><span data-stu-id="96903-130">If you scale your throughput needs and have such a high level of provisioning per partition, you will get a warning message</span></span>

<span data-ttu-id="96903-131">Níže je konkrétní příklad, ve kterém můžete zřídit zákazník 10kRU/s s 100kRU/m, ukládání 73 % náklady proti zřizování ve špičce (na 50kRU za sekundu) prostřednictvím dobou 90 sekundu v kolekci, která má 10 000 RU/s a 100 000 RU/m zřízené:</span><span class="sxs-lookup"><span data-stu-id="96903-131">Below is a concrete example, in which a customer can provision 10kRU/s with 100kRU/m, saving 73% in cost against provisioning for peak (at 50kRU/sec) through a 90-second period on a collection that has 10,000 RU/s and 100,000 RU/m provisioned:</span></span>

* <span data-ttu-id="96903-132">1 sekundu: hello RU/m rozpočtu je nastavený na 100 000</span><span class="sxs-lookup"><span data-stu-id="96903-132">1st second: hello RU/m budget is set at 100,000</span></span>
* <span data-ttu-id="96903-133">3. sekundu: Při této druhé hello využívání jednotek žádosti byl 11,010 RUs, 1,010 RUs výše zřizování hello RU/s.</span><span class="sxs-lookup"><span data-stu-id="96903-133">3rd second: During that second hello consumption of Request Unit was 11,010 RUs, 1,010 RUs above hello RU/s provisioning.</span></span> <span data-ttu-id="96903-134">Proto jsou 1,010 RUs odečtena z rozpočtu RU/m hello.</span><span class="sxs-lookup"><span data-stu-id="96903-134">Therefore, 1,010 RUs are deducted from hello RU/m budget.</span></span> <span data-ttu-id="96903-135">98,990 RUs jsou k dispozici pro hello další 57 sekund v rozpočtu RU/m hello</span><span class="sxs-lookup"><span data-stu-id="96903-135">98,990 RUs are available for hello next 57 seconds in hello RU/m budget</span></span>
* <span data-ttu-id="96903-136">29 sekundu: během druhé, došlo k velké Špička (> 4 x vyšší než zřizování za sekundu) a hello spotřeby jednotek žádosti byl 46,920 RUs.</span><span class="sxs-lookup"><span data-stu-id="96903-136">29th second: During that second, a large spike happened (>4x higher than provisioning per second) and hello consumption of Request Unit was 46,920 RUs.</span></span> <span data-ttu-id="96903-137">Snižuje 36,920 RUs hello RU/m rozpočtu z 92,323 too55 RUs (28th sekundu), 403 RUs (29 sekundu)</span><span class="sxs-lookup"><span data-stu-id="96903-137">36,920 RUs are deducted from hello RU/m budget that dropped from 92,323 RUs (28th second) too55,403 RUs (29th second)</span></span>
* <span data-ttu-id="96903-138">61st sekundu: RU/m nároky se nastaví zpět too100 000 RUs.</span><span class="sxs-lookup"><span data-stu-id="96903-138">61st second: RU/m budget is set back too100,000 RUs.</span></span>
 
![Graf zobrazující hello spotřeby a zřizování Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a><span data-ttu-id="96903-140">Určení požadavku jednotka kapacity pomocí RU/m</span><span class="sxs-lookup"><span data-stu-id="96903-140">Specifying request unit capacity with RU/m</span></span>

<span data-ttu-id="96903-141">Když vytváříte kolekci Azure Cosmos DB, můžete zadat číslo hello jednotek žádosti za sekundu (RU za sekundu), kterou chcete vyhrazené pro kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="96903-141">When creating an Azure Cosmos DB collection, you specify hello number of request units per second (RU per second) you want reserved for hello collection.</span></span> <span data-ttu-id="96903-142">Můžete také rozhodnout, pokud chcete, aby tooadd RU za minutu.</span><span class="sxs-lookup"><span data-stu-id="96903-142">You can also decide if you want tooadd RU per minute.</span></span> <span data-ttu-id="96903-143">To lze provést prostřednictvím hello portál nebo hello SDK.</span><span class="sxs-lookup"><span data-stu-id="96903-143">This can be done through hello Portal or hello SDK.</span></span> 

### <a name="through-hello-portal"></a><span data-ttu-id="96903-144">Prostřednictvím hello portálu</span><span class="sxs-lookup"><span data-stu-id="96903-144">Through hello Portal</span></span>

<span data-ttu-id="96903-145">Povolení nebo zakázání RU za minutu jednoduše vyžaduje kliknutí při zřizování kolekce.</span><span class="sxs-lookup"><span data-stu-id="96903-145">Enabling or disabling RU per minute simply requires a click when provisioning a collection.</span></span> 

 ![Snímek obrazovky zobrazující jak tooset RU/m v hello portálu Azure](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-hello-sdk"></a><span data-ttu-id="96903-147">Prostřednictvím hello SDK</span><span class="sxs-lookup"><span data-stu-id="96903-147">Through hello SDK</span></span>
<span data-ttu-id="96903-148">První to je důležité toonote, RU/m je dostupná jenom pro následující sady SDK hello:</span><span class="sxs-lookup"><span data-stu-id="96903-148">First, this is important toonote that RU/m is only available for hello following SDKs:</span></span>

* <span data-ttu-id="96903-149">Rozhraní .net 1.14.0</span><span class="sxs-lookup"><span data-stu-id="96903-149">.Net 1.14.0</span></span>
* <span data-ttu-id="96903-150">Java 1.11.0</span><span class="sxs-lookup"><span data-stu-id="96903-150">Java 1.11.0</span></span>
* <span data-ttu-id="96903-151">Node.js 1.12.0</span><span class="sxs-lookup"><span data-stu-id="96903-151">Node.js 1.12.0</span></span>
* <span data-ttu-id="96903-152">Python 2.2.0</span><span class="sxs-lookup"><span data-stu-id="96903-152">Python 2.2.0</span></span>

<span data-ttu-id="96903-153">Zde je fragment kódu pro vytvoření kolekce s 3 000 jednotek žádosti za jednotek žádosti druhý a 30 000 za minutu pomocí hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="96903-153">Here is a code snippet for creating a collection with 3,000 request units per second and 30,000 request units per minute using hello .NET SDK:</span></span>

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set hello throughput too3,000 request units per second which will give you 30,000 request units per minute as hello RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

<span data-ttu-id="96903-154">Zde je fragment kódu pro změnu hello propustnost kolekce too5, hello 000 jednotek žádosti za sekundu bez zřizování RU za minutu pomocí .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="96903-154">Here is a code snippet for changing hello throughput of a collection too5,000 request units per second without provisioning RU per minute using hello .NET SDK:</span></span>

```csharp
// Get hello current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput too5000 request units per second without RU/m enabled (hello last parameter tooOfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a><span data-ttu-id="96903-155">Dobré podle scénáře</span><span class="sxs-lookup"><span data-stu-id="96903-155">Good fit scenarios</span></span>

<span data-ttu-id="96903-156">V této části poskytujeme přehled scénářů, které jsou vhodné pro povolení jednotek žádosti za minutu.</span><span class="sxs-lookup"><span data-stu-id="96903-156">In this section, we provide an overview of scenarios that are a good fit for enabling request units per minute.</span></span>

<span data-ttu-id="96903-157">**Prostředí pro vývoj/testování:** vhodné.</span><span class="sxs-lookup"><span data-stu-id="96903-157">**Dev/Test environment:** Good fit.</span></span> <span data-ttu-id="96903-158">Během fáze vývoj hello Pokud testujete aplikace s různé úlohy, RU/m nabízejí flexibilitu hello v této fázi.</span><span class="sxs-lookup"><span data-stu-id="96903-158">During hello development stage, if you are testing your application with different workloads, RU/m can provide hello flexibility at this stage.</span></span> <span data-ttu-id="96903-159">Při hello [emulátoru](local-emulator.md) je skvělý nástroj volné tootest Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="96903-159">While hello [emulator](local-emulator.md) is a great free tool tootest Azure Cosmos DB.</span></span> <span data-ttu-id="96903-160">Ale pokud chcete toostart v cloudovém prostředí, budete mít flexibilitu s RU/m pro vašim požadavkům na výkon ad hoc.</span><span class="sxs-lookup"><span data-stu-id="96903-160">However if you want toostart in a cloud environment, you will have a great flexibility with RU/m for your adhoc performance needs.</span></span> <span data-ttu-id="96903-161">Můžete se věnovat více času, méně plně soustředit na první požadavkům na výkon.</span><span class="sxs-lookup"><span data-stu-id="96903-161">You will spend more time developing, less worrying about performance needs at first.</span></span> <span data-ttu-id="96903-162">Doporučujeme počínaje hello minimální zřizování RU/s a povolit RU/m.</span><span class="sxs-lookup"><span data-stu-id="96903-162">We recommend starting with hello minimum RU/s provisioning and enable RU/m.</span></span>

<span data-ttu-id="96903-163">**Musí nepředvídatelným, spiky, minutu členitost:** dobré nevejde – úspory: 25 75 %.</span><span class="sxs-lookup"><span data-stu-id="96903-163">**Unpredictable, spiky, minute granularity needs:** Good fit – Savings: 25-75%.</span></span> <span data-ttu-id="96903-164">Jsme viděli velké zlepšování z RU/m a většině produkčních scénářích se do této skupiny.</span><span class="sxs-lookup"><span data-stu-id="96903-164">We have seen large improvement from RU/m and most production scenarios are into that group.</span></span> <span data-ttu-id="96903-165">Pokud máte IoT pracovního vytížení, který má Špička několikrát za minutu, pokud máte dotazy spuštěnými při systému provede hromadného vložení na hello stejný čas, budete potřebovat víc kapacity pro handeling spiky potřebuje.</span><span class="sxs-lookup"><span data-stu-id="96903-165">If you have an IoT workload that has spike a few times in a minute, if you have queries running when your system makes mass insert at hello same time, you will need extra capacity for handeling spiky needs.</span></span> <span data-ttu-id="96903-166">Doporučujeme, abyste optimalizace potřeb prostředků s použitím náš přístup krok za krokem níže.</span><span class="sxs-lookup"><span data-stu-id="96903-166">We recommend optimizing your resource needs by applying our step by step approach below.</span></span>

 ![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 <span data-ttu-id="96903-168">*Obrázek - RU spotřeba srovnávacího testu*</span><span class="sxs-lookup"><span data-stu-id="96903-168">*Figure - RU consumption benchmark*</span></span>

<span data-ttu-id="96903-169">**Jistotu:** dobré nevejde – úspory: 10-20 %.</span><span class="sxs-lookup"><span data-stu-id="96903-169">**Peace of mind:** Good fit – Savings: 10-20%.</span></span> <span data-ttu-id="96903-170">V některých případech stačí chcete jistotu toohave a nestarat se o potenciální vrcholů a omezení.</span><span class="sxs-lookup"><span data-stu-id="96903-170">Sometimes, you just want toohave peace of mind and not worry about potential peaks and throttling.</span></span> <span data-ttu-id="96903-171">Tato funkce je pro vás ideální hello jeden.</span><span class="sxs-lookup"><span data-stu-id="96903-171">This feature is hello right one for you.</span></span> <span data-ttu-id="96903-172">V takovém případě doporučujeme povolení RU/m a mírně nižší vaší za druhé zřizování.</span><span class="sxs-lookup"><span data-stu-id="96903-172">In that case, we recommend enabling RU/m and slightly lower your per second provisioning.</span></span> <span data-ttu-id="96903-173">Tento případ se liší od hello výše jako nebude zkoušet toooptimize intenzivně zřizování.</span><span class="sxs-lookup"><span data-stu-id="96903-173">This case is different from hello above as you will not try toooptimize aggressively your provisioning.</span></span> <span data-ttu-id="96903-174">To je více přistupovat "Nula omezení", který se v.</span><span class="sxs-lookup"><span data-stu-id="96903-174">This is more of a “Zero Throttling” mindset you are in.</span></span>

<span data-ttu-id="96903-175">Kritické operace s potřebami ad hoc: doporučujeme někdy tooonly umožní kritické operace přístupu RU/m rozpočtu, proto nepodporuje hello nároky využívat ad hoc nebo méně důležité operace.</span><span class="sxs-lookup"><span data-stu-id="96903-175">Critical operations with adhoc needs: We sometimes recommend tooonly let critical operations access RU/m budget so hello budget doesn’t get consume by adhoc or less important operations.</span></span> <span data-ttu-id="96903-176">Který může být snadno definováno v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="96903-176">That can be easily defined in hello section below.</span></span>

## <a name="using-hello-portal-metrics-toooptimize-cost-and-performance"></a><span data-ttu-id="96903-177">Pomocí hello portálu metriky toooptimize nákladů a výkonu</span><span class="sxs-lookup"><span data-stu-id="96903-177">Using hello portal metrics toooptimize cost and performance</span></span>

<span data-ttu-id="96903-178">**V následujících týdnech hello budeme vyvíjet další obsah hello kolem monitorování toooptimize RUs minutu spotřebu, musí vaše propustnost.**</span><span class="sxs-lookup"><span data-stu-id="96903-178">**In hello coming weeks, we will further develop hello content around monitoring RUs minute consumption toooptimize your throughput needs.**</span></span>

<span data-ttu-id="96903-179">Prostřednictvím portálu metriky hello můžete zobrazit, kolik regulární RU sekund využívat versus RU minut.</span><span class="sxs-lookup"><span data-stu-id="96903-179">Through hello portal metrics, you can see how much of regular RU seconds you consume versus RU minutes.</span></span> <span data-ttu-id="96903-180">Monitorování tyto metriky by měly pomoci při optimalizaci vašeho zřizování.</span><span class="sxs-lookup"><span data-stu-id="96903-180">Monitoring these metrics should help you optimize your provisioning.</span></span> 

<span data-ttu-id="96903-181">Krok za krokem přístup návod, jak se doporučuje toouse využít tooyour RU/m.</span><span class="sxs-lookup"><span data-stu-id="96903-181">We recommend a step by step approach on how toouse RU/m tooyour advantage.</span></span> <span data-ttu-id="96903-182">Při každém kroku byste měli mít přehled o využívání hello RU představující úplný cyklus úlohy (to může být hodiny, dny, nebo i týdny) a přehledné na využití hello můžete zřídit.</span><span class="sxs-lookup"><span data-stu-id="96903-182">For each step, you should have an overview of hello RU consumption representing a full cycle of your workload (it could be hours, days, or even weeks) and get insights on hello utilization of what you provision.</span></span>

<span data-ttu-id="96903-183">Princip Hello za tento přístup je toomake vaší propustnost zřizování jako zavřete jako možné tooa zřizování bod, který odpovídá kritériím výkonu níže.</span><span class="sxs-lookup"><span data-stu-id="96903-183">hello principle behind this approach is toomake your throughput provisioning as close as possible tooa provisioning point that matches your performance criteria below.</span></span> 

![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
<span data-ttu-id="96903-185">toounderstand hello optimální zřizování bodu pro úlohy, je třeba toounderstand:</span><span class="sxs-lookup"><span data-stu-id="96903-185">toounderstand hello optimal provisioning point for your workload, you need toounderstand:</span></span>

* <span data-ttu-id="96903-186">Spotřeba vzory: žádné, nepravidelným nebo dlouhodobě špičky?</span><span class="sxs-lookup"><span data-stu-id="96903-186">Consumption patterns: no, infrequent or sustained spikes?</span></span> <span data-ttu-id="96903-187">(2 x průměr) malá, střední a velké (> 10 x průměr) špičky?</span><span class="sxs-lookup"><span data-stu-id="96903-187">Small (2x average), medium, or large (>10x average) spikes?</span></span>
* <span data-ttu-id="96903-188">Procenta omezenému požadavků: udělat si projděte Pokud máte chvilku omezování?</span><span class="sxs-lookup"><span data-stu-id="96903-188">Percent of throttled requests: do you feel comfortable if you have a bit of throttling?</span></span> <span data-ttu-id="96903-189">Pokud ano, jak moc?</span><span class="sxs-lookup"><span data-stu-id="96903-189">If so, by how much?</span></span> 

<span data-ttu-id="96903-190">Po zjištění, jaké jsou vaše cíle, bude možné tooget blíže toohello optimální zřizování.</span><span class="sxs-lookup"><span data-stu-id="96903-190">Once you have identified what your goals are, you will be able tooget closer toohello optimal provisioning.</span></span>

<span data-ttu-id="96903-191">tooassist, chceme tooprovide celkové pokyny na tom, jak vaše zřizování toooptimize podle vaší spotřeby RU/m.</span><span class="sxs-lookup"><span data-stu-id="96903-191">tooassist you, we want tooprovide an overall guidance on how toooptimize your provisioning based on your RU/m consumption.</span></span> <span data-ttu-id="96903-192">Tyto pokyny netýká tooall druh úloh, ale je založena na hello privátní Preview verzi znalostní báze.</span><span class="sxs-lookup"><span data-stu-id="96903-192">This guidance doesn’t apply tooall kind of workloads but is based on hello private preview knowledge.</span></span> <span data-ttu-id="96903-193">Může se nám změnit tyto standardní hodnoty jako jsme Další informace:</span><span class="sxs-lookup"><span data-stu-id="96903-193">We might change such baselines as we learn more:</span></span>

|<span data-ttu-id="96903-194">% Využití RU/m</span><span class="sxs-lookup"><span data-stu-id="96903-194">RU/m % utilization</span></span>|<span data-ttu-id="96903-195">Úroveň využití RU/m</span><span class="sxs-lookup"><span data-stu-id="96903-195">Degree of utilization of RU/m</span></span>|<span data-ttu-id="96903-196">Doporučené akce pro zřizování</span><span class="sxs-lookup"><span data-stu-id="96903-196">Recommended actions for provisioning</span></span>|
|---|---|---|
|<span data-ttu-id="96903-197">0-1%</span><span class="sxs-lookup"><span data-stu-id="96903-197">0-1%</span></span>|<span data-ttu-id="96903-198">V části využití</span><span class="sxs-lookup"><span data-stu-id="96903-198">Under utilization</span></span>|<span data-ttu-id="96903-199">Nižší další RU/m tooconsume RU/s</span><span class="sxs-lookup"><span data-stu-id="96903-199">Lower RU/s tooconsume more RU/m</span></span>|
|<span data-ttu-id="96903-200">1-10%</span><span class="sxs-lookup"><span data-stu-id="96903-200">1-10%</span></span>|<span data-ttu-id="96903-201">Použití v pořádku</span><span class="sxs-lookup"><span data-stu-id="96903-201">Healthy use</span></span>|<span data-ttu-id="96903-202">Zachovat hello stejné zřizování úroveň</span><span class="sxs-lookup"><span data-stu-id="96903-202">Keep hello same provisioning level</span></span>|
|<span data-ttu-id="96903-203">Více než 10 %</span><span class="sxs-lookup"><span data-stu-id="96903-203">Above 10%</span></span>|<span data-ttu-id="96903-204">Přes využití</span><span class="sxs-lookup"><span data-stu-id="96903-204">Over utilization</span></span>|<span data-ttu-id="96903-205">Zvýšit toorely RU/s menší na RU/m</span><span class="sxs-lookup"><span data-stu-id="96903-205">Increase RU/s toorely less on RU/m</span></span>|

## <a name="select-which-operations-can-consume-hello-rum-budget"></a><span data-ttu-id="96903-206">Vyberte operací, které můžou využívat nároky RU/m hello</span><span class="sxs-lookup"><span data-stu-id="96903-206">Select which operations can consume hello RU/m budget</span></span>

<span data-ttu-id="96903-207">Na úrovni žádost vám může také povolit nebo zakázat RU/m nároky tooserve hello požadavek bez ohledu na typ operace.</span><span class="sxs-lookup"><span data-stu-id="96903-207">At request level, you can also enable/disable RU/m budget tooserve hello request irrespective of operation type.</span></span> <span data-ttu-id="96903-208">Pokud obsazením regulární zřízené nároky RUs za sekundu a hello žádost nemůže využívat hello RU/m rozpočtu, budou omezeny tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="96903-208">If regular provisioned RUs/sec budget is consumed and hello request cannot consume hello RU/m budget, this request will be throttled.</span></span> <span data-ttu-id="96903-209">Ve výchozím nastavení každá žádost obsloužených RU/m rozpočtu, pokud je aktivovaná nároky propustnost RU/m.</span><span class="sxs-lookup"><span data-stu-id="96903-209">By default, any request is served by RU/m budget if RU/m throughput budget is activated.</span></span> 

<span data-ttu-id="96903-210">Zde je fragment kódu pro zakázání RU/m nároky pomocí hello DocumentDB rozhraní API pro operace CRUD a dotazu.</span><span class="sxs-lookup"><span data-stu-id="96903-210">Here is a code snippet for disabling RU/m budget using hello DocumentDB API for CRUD and query operations.</span></span>

```csharp
// In order toodisable any CRUD request for RU/m, set DisableRUPerMinuteUsage tootrue in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order toodisable any query request for RU/m, set DisableRUPerMinuteOnRequest tootrue in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a><span data-ttu-id="96903-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96903-211">Next steps</span></span>

<span data-ttu-id="96903-212">V tomto článku jsme popsali, jak rozdělení funguje v Azure Cosmos DB, jak můžete vytvořit dělené kolekce a jak můžete vybrat vhodným klíčem oddílu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96903-212">In this article, we've described how partitioning works in Azure Cosmos DB, how you can create partitioned collections, and how you can pick a good partition key for your application.</span></span>

* <span data-ttu-id="96903-213">Proveďte škálování a výkon testování pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="96903-213">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="96903-214">V tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md) pro ukázku.</span><span class="sxs-lookup"><span data-stu-id="96903-214">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="96903-215">Začínáme s hello kódování [sady SDK](documentdb-sdk-dotnet.md) nebo hello [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="96903-215">Get started coding with hello [SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/).</span></span>
* <span data-ttu-id="96903-216">Další informace o [zřízené propustnosti](request-units.md) v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="96903-216">Learn about [provisioned throughput](request-units.md) in Azure Cosmos DB</span></span> 

