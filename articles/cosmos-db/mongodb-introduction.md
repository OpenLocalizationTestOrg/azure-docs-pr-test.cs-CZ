---
title: "Úvod do Azure Cosmos DB: rozhraní API pro MongoDB | Microsoft Docs"
description: "Zjistěte, jak můžete používat Azure Cosmos databázi k ukládání a dotaz ohromné objemy dokumentů JSON s nízkou latencí pomocí Oblíbené MongoDB rozhraní API operačních systémů."
keywords: Co je MongoDB
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: anhoh
ms.openlocfilehash: 4dbf91a3c1d6a287d7337647f9e059566c7ddbe5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a><span data-ttu-id="79744-104">Úvod do Azure Cosmos DB: rozhraní API pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="79744-104">Introduction to Azure Cosmos DB: API for MongoDB</span></span>

<span data-ttu-id="79744-105">[Azure Cosmos DB](../cosmos-db/introduction.md) je globálně distribuovaná databázová služba Microsoftu s více modely pro klíčové aplikace.</span><span class="sxs-lookup"><span data-stu-id="79744-105">[Azure Cosmos DB](../cosmos-db/introduction.md) is Microsoft's globally distributed, multi-model database service for mission-critical applications.</span></span> <span data-ttu-id="79744-106">Azure Cosmos DB poskytuje [globální distribuci na klíč](distribute-data-globally.md), [elastické škálování propustnosti a úložiště](partition-data.md) po celém světě, latence v řádu milisekund na 99. percentilu, [pět jasně definovaných úrovní konzistence](consistency-levels.md) a zaručenou vysokou dostupnost. To vše je podloženo [nejlepšími smlouvami SLA v oboru](https://azure.microsoft.com/support/legal/sla/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="79744-106">Azure Cosmos DB provides [turn-key global distribution](distribute-data-globally.md), [elastic scaling of throughput and storage](partition-data.md) worldwide, single-digit millisecond latencies at the 99th percentile, [five well-defined consistency levels](consistency-levels.md), and guaranteed high availability, all backed by [industry-leading SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/).</span></span> <span data-ttu-id="79744-107">Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), aniž by vyžadovala zapojení správy schémat a indexů.</span><span class="sxs-lookup"><span data-stu-id="79744-107">Azure Cosmos DB [automatically indexes data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) without requiring you to deal with schema and index management.</span></span> <span data-ttu-id="79744-108">Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat.</span><span class="sxs-lookup"><span data-stu-id="79744-108">It is multi-model and supports document, key-value, graph, and columnar data models.</span></span> 

![Azure Cosmos DB: MongoDB rozhraní API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

<span data-ttu-id="79744-110">Cosmos DB databází lze použít jako úložiště dat pro aplikace napsané pro [MongoDB](https://docs.mongodb.com/manual/introduction/).</span><span class="sxs-lookup"><span data-stu-id="79744-110">Cosmos DB databases can be used as the data store for apps written for [MongoDB](https://docs.mongodb.com/manual/introduction/).</span></span> <span data-ttu-id="79744-111">To znamená, že pomocí stávající [ovladače](https://docs.mongodb.org/ecosystem/drivers/), vaše aplikace napsané pro MongoDB teď můžete komunikovat s Cosmos DB a používat Cosmos DB databáze místo databáze MongoDB.</span><span class="sxs-lookup"><span data-stu-id="79744-111">This means that by using existing [drivers](https://docs.mongodb.org/ecosystem/drivers/), your application written for MongoDB can now communicate with Cosmos DB and use Cosmos DB databases instead of MongoDB databases.</span></span> <span data-ttu-id="79744-112">V mnoha případech můžete přepínat pomocí MongoDB do databáze Cosmos jednoduše změnou připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="79744-112">In many cases, you can switch from using MongoDB to Cosmos DB by simply changing a connection string.</span></span> <span data-ttu-id="79744-113">Pomocí této funkce lze snadno vytvářet a spouštět aplikace databázi MongoDB ve službě Azure cloud s globální distribuční databázi Cosmos Azure a [komplexní špičkové SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), nadále používat známé dovednosti a nástroje pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="79744-113">Using this functionality, you can easily build and run MongoDB database applications in the Azure cloud with Azure Cosmos DB's global distribution and [comprehensive industry leading SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db), while continuing to use familiar skills and tools for MongoDB.</span></span>


## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a><span data-ttu-id="79744-114">Co je výhodou používání Azure Cosmos DB pro MongoDB aplikace?</span><span class="sxs-lookup"><span data-stu-id="79744-114">What is the benefit of using Azure Cosmos DB for MongoDB applications?</span></span>

<span data-ttu-id="79744-115">**Elasticky škálovatelná propustnost a úložiště:** snadno škálovat nahoru nebo dolů databázi MongoDB splňovala potřeby vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="79744-115">**Elastically scalable throughput and storage:** Easily scale up or down your MongoDB database to meet your application needs.</span></span> <span data-ttu-id="79744-116">Data se ukládají na discích SSD (solid-state drive), které nabízí nízkou a předvídatelnou latenci.</span><span class="sxs-lookup"><span data-stu-id="79744-116">Your data is stored on solid state disks (SSD) for low predictable latencies.</span></span> <span data-ttu-id="79744-117">Cosmos DB podporuje MongoDB kolekce, které je možné škálovat na prakticky neomezené velikosti úložiště a zřízené propustnosti.</span><span class="sxs-lookup"><span data-stu-id="79744-117">Cosmos DB supports MongoDB collections that can scale to virtually unlimited storage sizes and provisioned throughput.</span></span> <span data-ttu-id="79744-118">Je možné Elasticky škálovat Cosmos DB s předvídatelným výkonem bezproblémově růstem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="79744-118">You can elastically scale Cosmos DB with predictable performance seamlessly as your application grows.</span></span> 

<span data-ttu-id="79744-119">**Replikace více oblast:** Cosmos DB transparentně replikuje data do všech oblastí, které jste spojené s vaším účtem MongoDB umožňuje vyvíjet aplikace, které vyžadují globální přístup k datům při současném poskytování kompromisy mezi konzistence, dostupnosti a výkonu, všechny s odpovídající záruky.</span><span class="sxs-lookup"><span data-stu-id="79744-119">**Multi-region replication:** Cosmos DB transparently replicates your data to all regions you've associated with your MongoDB account, enabling you to develop applications that require global access to data while providing tradeoffs between consistency, availability and performance, all with corresponding guarantees.</span></span> <span data-ttu-id="79744-120">Cosmos DB poskytuje transparentní regionální převzetí služeb při selhání s více funkci rozhraní API a možnost Elasticky škálovat propustnost a úložiště po celém světě.</span><span class="sxs-lookup"><span data-stu-id="79744-120">Cosmos DB provides transparent regional failover with multi-homing APIs, and the ability to elastically scale throughput and storage across the globe.</span></span> <span data-ttu-id="79744-121">Další informace v [distribuci dat globálně](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="79744-121">Learn more in [Distribute data globally](distribute-data-globally.md).</span></span>

<span data-ttu-id="79744-122">**Kompatibilita MongoDB**: můžete použít existující MongoDB znalosti, kód aplikace a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="79744-122">**MongoDB compatibility**: You can use your existing MongoDB expertise, application code, and tooling.</span></span> <span data-ttu-id="79744-123">Můžete vyvíjet aplikace, které používají MongoDB a nasadit je do produkčního prostředí pomocí plně spravovaná globálně distribuované služby Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="79744-123">You can develop applications using MongoDB and deploy them to production using the fully managed globally distributed Cosmos DB service.</span></span>

<span data-ttu-id="79744-124">**Žádný server správy**: Nemáte ke správě a škálovat vaše databáze MongoDB.</span><span class="sxs-lookup"><span data-stu-id="79744-124">**No server management**: You don't have to manage and scale your MongoDB databases.</span></span> <span data-ttu-id="79744-125">Cosmos DB je plně spravovaná služba, což znamená, že nemusíte spravovat všechny infrastruktury nebo virtuální počítače sami.</span><span class="sxs-lookup"><span data-stu-id="79744-125">Cosmos DB is a fully managed service, which means you do not have to manage any infrastructure or Virtual Machines yourself.</span></span> <span data-ttu-id="79744-126">Je k dispozici v 30 + cosmos DB [oblasti Azure](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="79744-126">Cosmos DB is available in 30+ [Azure Regions](https://azure.microsoft.com/regions/services/).</span></span>

<span data-ttu-id="79744-127">**Přizpůsobitelné úrovně konzistence:** Pro dosažení optimálního poměru mezi konzistencí a výkonem si můžete vybrat z pěti jasně definovaných úrovní konzistence.</span><span class="sxs-lookup"><span data-stu-id="79744-127">**Tunable consistency levels:** Select from five well defined consistency levels to achieve optimal trade-off between consistency and performance.</span></span> <span data-ttu-id="79744-128">Pro dotazy a operace čtení Cosmos DB nabízí pět úrovně konzistence: silnou, s ohraničenou odolností, založenou relace, konzistentní Předpona a případnou.</span><span class="sxs-lookup"><span data-stu-id="79744-128">For queries and read operations, Cosmos DB offers five distinct consistency levels: strong, bounded-staleness, session, consistent prefix, and eventual.</span></span> <span data-ttu-id="79744-129">Tyto podrobné, dobře definované úrovně konzistence umožňují zvolit vhodný poměr mezi konzistencí, dostupností a latencí.</span><span class="sxs-lookup"><span data-stu-id="79744-129">These granular, well-defined consistency levels allow you to make sound trade-offs between consistency, availability, and latency.</span></span> <span data-ttu-id="79744-130">Další informace najdete v tématu popisujícím [využití úrovní konzistence pro maximalizaci dostupnosti a výkonu](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="79744-130">Learn more in [Using consistency levels to maximize availability and performance](consistency-levels.md).</span></span>

<span data-ttu-id="79744-131">**Automatické indexování**: ve výchozím nastavení, Cosmos DB automaticky indexuje všechny vlastnosti v rámci dokumenty ve vaší MongoDB databáze a nemá očekávat nebo nevyžaduje žádné schéma nebo vytváření sekundárních indexů.</span><span class="sxs-lookup"><span data-stu-id="79744-131">**Automatic indexing**: By default, Cosmos DB automatically indexes all the properties within documents in your MongoDB database and does not expect or require any schema or creation of secondary indices.</span></span>

<span data-ttu-id="79744-132">**Podnikové úrovni** -Azure Cosmos DB podporuje více místní repliky k poskytování 99,99 % dostupnost a ochranu dat při krátkodobém místní a regionální selhání.</span><span class="sxs-lookup"><span data-stu-id="79744-132">**Enterprise grade** - Azure Cosmos DB supports multiple local replicas to deliver 99.99% availability and data protection in the face of local and regional failures.</span></span> <span data-ttu-id="79744-133">Azure Cosmos DB má podnikové úrovni [dodržování předpisů certifikace](https://www.microsoft.com/trustcenter) a funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="79744-133">Azure Cosmos DB has enterprise grade [compliance certifications](https://www.microsoft.com/trustcenter) and security features.</span></span> 

<span data-ttu-id="79744-134">Další informace v této Azure videa s Scott Hanselman a Azure Cosmos DB hlavní inženýrství manažer Kirill Gavrylyuk pátek.</span><span class="sxs-lookup"><span data-stu-id="79744-134">Learn more in this Azure Friday video with Scott Hanselman and Azure Cosmos DB Principal Engineering Manager, Kirill Gavrylyuk.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-to-get-started"></a><span data-ttu-id="79744-135">Jak začít</span><span class="sxs-lookup"><span data-stu-id="79744-135">How to get started</span></span>

<span data-ttu-id="79744-136">Postupujte podle quickstarts MongoDB vytvořit účet Cosmos DB a migrovat stávající aplikace Mongo DB pomocí Cosmos DB nebo vytvořit nový:</span><span class="sxs-lookup"><span data-stu-id="79744-136">Follow the MongoDB quickstarts to create a Cosmos DB account and migrate your existing Mongo DB application to use Cosmos DB, or build a new one:</span></span>

* <span data-ttu-id="79744-137">[Migrovat stávající webovou aplikaci Node.js MongoDB](create-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="79744-137">[Migrate an existing Node.js MongoDB web app](create-mongodb-nodejs.md).</span></span>
* [<span data-ttu-id="79744-138">Vytvoření webové aplikace MongoDB API pomocí rozhraní .NET a portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79744-138">Build a MongoDB API web app with .NET and the Azure portal</span></span>](create-mongodb-dotnet.md)
* [<span data-ttu-id="79744-139">Sestavení rozhraní API MongoDB konzolovou aplikaci Java a portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79744-139">Build a MongoDB API console app with Java and the Azure portal</span></span>](create-mongodb-java.md)

## <a name="next-steps"></a><span data-ttu-id="79744-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79744-140">Next steps</span></span>

<span data-ttu-id="79744-141">Informace o rozhraní API MongoDB Azure Cosmos DB je integrována do dokumentace se celkově Azure Cosmos DB, ale tady jsou na několik věcí, které vám pomůžou začít:</span><span class="sxs-lookup"><span data-stu-id="79744-141">Information about Azure Cosmos DB's MongoDB API is integrated into the overall Azure Cosmos DB documentation, but here are a few pointers to get you started:</span></span>

* <span data-ttu-id="79744-142">Postupujte podle [připojit k účtu MongoDB](connect-mongodb-account.md) kurzu se dozvíte, jak získat informace o účtu připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="79744-142">Follow the [Connect to a MongoDB account](connect-mongodb-account.md) tutorial to learn how to get your account connection string information.</span></span>
* <span data-ttu-id="79744-143">Postupujte podle [MongoChef použití s Azure Cosmos DB](mongodb-mongochef.md) kurzu se dozvíte, jak vytvořit připojení mezi Azure Cosmos DB databáze a MongoDB aplikace v MongoChef.</span><span class="sxs-lookup"><span data-stu-id="79744-143">Follow the [Use MongoChef with Azure Cosmos DB](mongodb-mongochef.md) tutorial to learn how to create a connection between your Azure Cosmos DB database and MongoDB app in MongoChef.</span></span>
* <span data-ttu-id="79744-144">Postupujte podle [migrace dat do databáze Cosmos Azure s protokolem podporu pro MongoDB](mongodb-migrate.md) kurzu pro import dat do rozhraní API pro databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="79744-144">Follow the [Migrate data to Azure Cosmos DB with protocol support for MongoDB](mongodb-migrate.md) tutorial to import your data to an API for MongoDB database.</span></span>
* <span data-ttu-id="79744-145">Připojit k rozhraní API pro použití účtu MongoDB [Robomongo](mongodb-robomongo.md).</span><span class="sxs-lookup"><span data-stu-id="79744-145">Connect to an API for MongoDB account using [Robomongo](mongodb-robomongo.md).</span></span>
* <span data-ttu-id="79744-146">Zjistěte, kolik RUs jsou vaše operace pomocí [GetLastRequestStatistics příkaz a metriku portálu Azure](request-units.md#GetLastRequestStatistics).</span><span class="sxs-lookup"><span data-stu-id="79744-146">Learn how many RUs your operations are using with the [GetLastRequestStatistics command and the Azure portal metrics](request-units.md#GetLastRequestStatistics).</span></span>
* <span data-ttu-id="79744-147">Zjistěte, jak [konfigurace pro čtení předvolby pro globálně distribuované aplikace](../cosmos-db/tutorial-global-distribution-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="79744-147">Learn how to [configure read preferences for globally distributed apps](../cosmos-db/tutorial-global-distribution-mongodb.md).</span></span>