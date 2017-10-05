---
title: "Azure SQL Database v paměti technologie | Microsoft Docs"
description: "Azure SQL Database v paměti technologie výrazně zlepšit výkon transakcí a analýzy úlohy. Zjistěte, jak chcete využít výhod těchto technologií."
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jodebrui
ms.openlocfilehash: 4cb45551c486263f26947e5684d54b4f2ecc7410
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a><span data-ttu-id="39ad1-104">Optimalizace výkonu pomocí technologie v paměti v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="39ad1-104">Optimize performance by using In-Memory technologies in SQL Database</span></span>

<span data-ttu-id="39ad1-105">Použití technologií v paměti ve službě Azure SQL Database, můžete dosáhnout zlepšení výkonu pomocí různých úloh: transakcí (online zpracování transakcí (OLTP)), analytics (online analytického zpracování (OLAP)) a ve smíšeném (hybridní transakce/analytického zpracování (HTAP)).</span><span class="sxs-lookup"><span data-stu-id="39ad1-105">By using In-Memory technologies in Azure SQL Database, you can achieve performance improvements with various workloads: transactional (online transactional processing (OLTP)), analytics (online analytical processing (OLAP)), and mixed (hybrid transaction/analytical processing (HTAP)).</span></span> <span data-ttu-id="39ad1-106">Z důvodu efektivnější dotazu a zpracování transakcí v paměti technologie také pomoci snížit náklady.</span><span class="sxs-lookup"><span data-stu-id="39ad1-106">Because of the more efficient query and transaction processing, In-Memory technologies also help you to reduce cost.</span></span> <span data-ttu-id="39ad1-107">Obvykle nemusíte upgradovat cenová úroveň databáze k dosáhnout zvýšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="39ad1-107">You typically don't need to upgrade the pricing tier of the database to achieve performance gains.</span></span> <span data-ttu-id="39ad1-108">V některých případech i je možné snížit cenovou úroveň, při vylepšení výkonu s technologiemi v paměti se přesto zobrazuje.</span><span class="sxs-lookup"><span data-stu-id="39ad1-108">In some cases, you might even be able reduce the pricing tier, while still seeing performance improvements with In-Memory technologies.</span></span>

<span data-ttu-id="39ad1-109">Zde jsou dva příklady, jak článek OLTP v paměti pomůže výrazně zlepšit výkon:</span><span class="sxs-lookup"><span data-stu-id="39ad1-109">Here are two examples of how In-Memory OLTP helped to significantly improve performance:</span></span>

- <span data-ttu-id="39ad1-110">Pomocí OLTP v paměti, [kvora obchodními řešeními bylo možné dvakrát jejich zatížení při současném zvyšování Dtu 70 %](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span><span class="sxs-lookup"><span data-stu-id="39ad1-110">By using In-Memory OLTP, [Quorum Business Solutions was able to double their workload while improving DTUs by 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span></span>
    - <span data-ttu-id="39ad1-111">DTU znamená *jednotek propustnosti databáze*, a obsahuje měření spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="39ad1-111">DTU means *database throughput unit*, and it includes a mesurement of resource consumption.</span></span>
- <span data-ttu-id="39ad1-112">Toto video ukazuje výrazné zlepšení spotřeby prostředků s ukázky pracovního vytížení: [OLTP v paměti v Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span><span class="sxs-lookup"><span data-stu-id="39ad1-112">The following video demonstrates significant improvement in resource consumption with a sample workload: [In-Memory OLTP in Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span></span>
    - <span data-ttu-id="39ad1-113">Další podrobnosti naleznete v příspěvku blogu: [OLTP v paměti v příspěvku blogu databáze SQL Azure](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span><span class="sxs-lookup"><span data-stu-id="39ad1-113">For more details, see the blog post: [In-Memory OLTP in Azure SQL Database Blog Post](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span></span>

<span data-ttu-id="39ad1-114">Technologie v paměti jsou k dispozici v všechny databáze v úrovni Premium, včetně databází v elastické fondy Premium.</span><span class="sxs-lookup"><span data-stu-id="39ad1-114">In-Memory technologies are available in all databases in the Premium tier, including databases in Premium elastic pools.</span></span>

<span data-ttu-id="39ad1-115">Následující video vysvětluje potenciální zvýšení výkonu s technologiemi v paměti ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="39ad1-115">The following video explains potential performance gains with In-Memory technologies in Azure SQL Database.</span></span> <span data-ttu-id="39ad1-116">Pamatujte, že zvýšení výkonu, který se zobrazí vždy závisí na mnoha faktorech, včetně povahu pracovního vytížení a data, vzor přístupu databáze a tak dále.</span><span class="sxs-lookup"><span data-stu-id="39ad1-116">Remember that the performance gain that you see always depends on many factors, including the nature of the workload and data, access pattern of the database, and so on.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

<span data-ttu-id="39ad1-117">Azure SQL Database má následující technologie v paměti:</span><span class="sxs-lookup"><span data-stu-id="39ad1-117">Azure SQL Database has the following In-Memory technologies:</span></span>

- <span data-ttu-id="39ad1-118">*OLTP v paměti* zvyšuje propustnost a snižuje latence pro zpracování transakcí.</span><span class="sxs-lookup"><span data-stu-id="39ad1-118">*In-Memory OLTP* increases throughput and reduces latency for transaction processing.</span></span> <span data-ttu-id="39ad1-119">Scénáře využívající OLTP v paměti jsou: transakce vysokou propustností zpracování například obchodní a hraní, přijímání dat ze zařízení IoT, ukládání do mezipaměti, načtení dat a dočasné tabulky a scénáře proměnné tabulky nebo události.</span><span class="sxs-lookup"><span data-stu-id="39ad1-119">Scenarios that benefit from In-Memory OLTP are: high-throughput transaction processing such as trading and gaming, data ingestion from events or IoT devices, caching, data load, and temporary table and table variable scenarios.</span></span>
- <span data-ttu-id="39ad1-120">*Clusterované indexy columnstore* redukuje vašeho úložiště (až 10 x) a zlepšení výkonu pro dotazy analýz a generování sestav.</span><span class="sxs-lookup"><span data-stu-id="39ad1-120">*Clustered columnstore indexes* reduce your storage footprint (up to 10 times) and improve performance for reporting and analytics queries.</span></span> <span data-ttu-id="39ad1-121">Můžete ji pomocí tabulky faktů v datová Tržiště nevejde se do databáze více dat a zlepšíte výkon.</span><span class="sxs-lookup"><span data-stu-id="39ad1-121">You can use it with fact tables in your data marts to fit more data in your database and improve performance.</span></span> <span data-ttu-id="39ad1-122">Navíc můžete ho s historických dat v provozní databázi nástroje k archivaci a moct dotaz až 10 x další data.</span><span class="sxs-lookup"><span data-stu-id="39ad1-122">Also, you can use it with historical data in your operational database to archive and be able to query up to 10 times more data.</span></span>
- <span data-ttu-id="39ad1-123">*Neclusterovaných indexů columnstore* pro HTAP umožňují v reálném čase proniknout do vaší firmy prostřednictvím dotazování provozní databáze přímo, aniž by bylo nutné spustit nákladné extrakce, transformace a načítání (ETL) proces a vyčkejte pro datový sklad vyplnit.</span><span class="sxs-lookup"><span data-stu-id="39ad1-123">*Nonclustered columnstore indexes* for HTAP help you to gain real-time insights into your business through querying the operational database directly, without the need to run an expensive extract, transform, and load (ETL) process and wait for the data warehouse to be populated.</span></span> <span data-ttu-id="39ad1-124">Neclusterovaných indexů columnstore povolí velmi rychlé spuštění analytické dotazy na databáze OLTP navíc snižuje dopad na provozní úlohy.</span><span class="sxs-lookup"><span data-stu-id="39ad1-124">Nonclustered columnstore indexes allow very fast execution of analytics queries on the OLTP database, while reducing the impact on the operational workload.</span></span>
- <span data-ttu-id="39ad1-125">Můžete taky nechat kombinace paměťově optimalizované tabulky s indexem columnstore.</span><span class="sxs-lookup"><span data-stu-id="39ad1-125">You can also have the combination of a memory-optimized table with a columnstore index.</span></span> <span data-ttu-id="39ad1-126">Tato kombinace umožňuje provádět zpracování velmi rychlé transakcí a *souběžně* velmi rychle spustit analytické dotazy na stejná data.</span><span class="sxs-lookup"><span data-stu-id="39ad1-126">This combination enables you to perform very fast transaction processing, and to *concurrently* run analytics queries very quickly on the same data.</span></span>

<span data-ttu-id="39ad1-127">Indexy columnstore a OLTP v paměti se součástí produktu SQL Server od 2012 a 2014, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="39ad1-127">Both columnstore indexes and In-Memory OLTP have been part of the SQL Server product since 2012 and 2014, respectively.</span></span> <span data-ttu-id="39ad1-128">Azure SQL Database a SQL Server sdílet stejné implementaci technologií v paměti.</span><span class="sxs-lookup"><span data-stu-id="39ad1-128">Azure SQL Database and SQL Server share the same implementation of In-Memory technologies.</span></span> <span data-ttu-id="39ad1-129">Do budoucna, nové funkce pro tyto technologie jsou vydávány v Azure SQL Database nejprve před jejich vydání v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="39ad1-129">Going forward, new capabilities for these technologies are released in Azure SQL Database first, before they are released in SQL Server.</span></span>

<span data-ttu-id="39ad1-130">Toto téma popisuje aspekty OLTP v paměti a columnstore indexy, které jsou specifické pro Azure SQL Database a obsahuje také ukázku:</span><span class="sxs-lookup"><span data-stu-id="39ad1-130">This topic describes aspects of In-Memory OLTP and columnstore indexes that are specific to Azure SQL Database and also includes samples:</span></span>
- <span data-ttu-id="39ad1-131">Dopad tyto technologie uvidíte na omezení velikosti úložiště a data.</span><span class="sxs-lookup"><span data-stu-id="39ad1-131">You'll see the impact of these technologies on storage and data size limits.</span></span>
- <span data-ttu-id="39ad1-132">Uvidíte, jak spravovat přesouvání databází, které používají tyto technologie mezi různé cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="39ad1-132">You'll see how to manage the movement of databases that use these technologies between the different pricing tiers.</span></span>
- <span data-ttu-id="39ad1-133">Zobrazí se dvou vzorcích, které ilustrují použití OLTP v paměti, jakož i indexy columnstore ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="39ad1-133">You'll see two samples that illustrate the use of In-Memory OLTP, as well as columnstore indexes in Azure SQL Database.</span></span>

<span data-ttu-id="39ad1-134">Najdete v následujících materiálech Další informace.</span><span class="sxs-lookup"><span data-stu-id="39ad1-134">See the following resources for more information.</span></span>

<span data-ttu-id="39ad1-135">Podrobné informace o technologiích:</span><span class="sxs-lookup"><span data-stu-id="39ad1-135">In-depth information about the technologies:</span></span>

- <span data-ttu-id="39ad1-136">[Přehled OLTP v paměti a scénáře použití](https://msdn.microsoft.com/library/mt774593.aspx) (obsahuje odkazy na informace o začít a Zákaznické případové studie)</span><span class="sxs-lookup"><span data-stu-id="39ad1-136">[In-Memory OLTP Overview and Usage Scenarios](https://msdn.microsoft.com/library/mt774593.aspx) (includes references to customer case studies and information to get started)</span></span>
- [<span data-ttu-id="39ad1-137">Dokumentace pro OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="39ad1-137">Documentation for In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
- [<span data-ttu-id="39ad1-138">Průvodce indexy Columnstore</span><span class="sxs-lookup"><span data-stu-id="39ad1-138">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
- <span data-ttu-id="39ad1-139">Hybridní transakcí nebo analytického zpracování (HTAP), také známé jako [provozní analýzu v reálném čase](https://msdn.microsoft.com/library/dn817827.aspx)</span><span class="sxs-lookup"><span data-stu-id="39ad1-139">Hybrid transactional/analytical processing (HTAP), also known as [real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)</span></span>

<span data-ttu-id="39ad1-140">Rychlý úvod do na OLTP v paměti: [rychlý Start 1: technologie OLTP v paměti pro rychlejší T-SQL výkonu](http://msdn.microsoft.com/library/mt694156.aspx) (jiný článek vám pomohou začít)</span><span class="sxs-lookup"><span data-stu-id="39ad1-140">A quick primer on In-Memory OLTP: [Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance](http://msdn.microsoft.com/library/mt694156.aspx) (another article to help you get started)</span></span>

<span data-ttu-id="39ad1-141">Podrobný videa o technologiích:</span><span class="sxs-lookup"><span data-stu-id="39ad1-141">In-depth videos about the technologies:</span></span>

- <span data-ttu-id="39ad1-142">[OLTP v paměti ve službě Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (která obsahuje ukázku výkonnostních výhod a postup reprodukování těchto výsledků sami)</span><span class="sxs-lookup"><span data-stu-id="39ad1-142">[In-Memory OLTP in Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (which contains a demo of performance benefits and steps to reproduce these results yourself)</span></span>
- [<span data-ttu-id="39ad1-143">Videa OLTP v paměti: Co je a v případě nebo postupy pro použití</span><span class="sxs-lookup"><span data-stu-id="39ad1-143">In-Memory OLTP Videos: What it is and When/How to use it</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [<span data-ttu-id="39ad1-144">Columnstore Index: Analýzy v paměti videa z webu Ignite 2016</span><span class="sxs-lookup"><span data-stu-id="39ad1-144">Columnstore Index: In-Memory Analytics Videos from Ignite 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a><span data-ttu-id="39ad1-145">Velikost úložiště a dat.</span><span class="sxs-lookup"><span data-stu-id="39ad1-145">Storage and data size</span></span>

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a><span data-ttu-id="39ad1-146">Limitu velikosti a úložiště dat pro OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="39ad1-146">Data size and storage cap for In-Memory OLTP</span></span>

<span data-ttu-id="39ad1-147">OLTP v paměti obsahuje paměťově optimalizované tabulky, které se používají k ukládání dat uživatele.</span><span class="sxs-lookup"><span data-stu-id="39ad1-147">In-Memory OLTP includes memory-optimized tables, which are used for storing user data.</span></span> <span data-ttu-id="39ad1-148">Tyto tabulky musí nevejdou se do paměti.</span><span class="sxs-lookup"><span data-stu-id="39ad1-148">These tables are required to fit in memory.</span></span> <span data-ttu-id="39ad1-149">Vzhledem k tomu, že budete spravovat paměti přímo ve službě SQL Database, máme koncept kvótu pro data uživatelů.</span><span class="sxs-lookup"><span data-stu-id="39ad1-149">Because you manage memory directly in the SQL Database service, we have the  concept of a quota for user data.</span></span> <span data-ttu-id="39ad1-150">Toto je vhodné se označuje jako *OLTP v paměti úložiště*.</span><span class="sxs-lookup"><span data-stu-id="39ad1-150">This idea is referred to as *In-Memory OLTP storage*.</span></span>

<span data-ttu-id="39ad1-151">Každý podporovaný samostatná databáze cenová úroveň a každý elastického fondu cenová úroveň zahrnuje množství OLTP v paměti úložiště.</span><span class="sxs-lookup"><span data-stu-id="39ad1-151">Each supported standalone database pricing tier and each elastic pool pricing tier includes a certain amount of In-Memory OLTP storage.</span></span> <span data-ttu-id="39ad1-152">V době psaní získat gigabajt úložiště pro každý 125 jednotky transakcí databáze (Dtu) nebo jednotky transakcí elastické databáze (Edtu).</span><span class="sxs-lookup"><span data-stu-id="39ad1-152">At the time of writing, you get a gigabyte of storage for every 125 database transaction units (DTUs) or elastic database transaction units (eDTUs).</span></span>

<span data-ttu-id="39ad1-153">[Úrovně služeb SQL Database](sql-database-service-tiers.md) článek má oficiální seznam úložiště OLTP v paměti, která je k dispozici pro všechny podporované samostatná databáze a elastického fondu cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="39ad1-153">The [SQL Database service tiers](sql-database-service-tiers.md) article has the official list of the In-Memory OLTP storage that is available for each supported standalone database and elastic pool pricing tier.</span></span>

<span data-ttu-id="39ad1-154">Následující položky započítávat vašeho úložiště cap OLTP v paměti:</span><span class="sxs-lookup"><span data-stu-id="39ad1-154">The following items count toward your In-Memory OLTP storage cap:</span></span>

- <span data-ttu-id="39ad1-155">Řádky dat aktivního uživatele v paměťově optimalizované tabulky a proměnné tabulek.</span><span class="sxs-lookup"><span data-stu-id="39ad1-155">Active user data rows in memory-optimized tables and table variables.</span></span> <span data-ttu-id="39ad1-156">Všimněte si, že nemáte směrem k krytky počet staré verze řádku.</span><span class="sxs-lookup"><span data-stu-id="39ad1-156">Note that old row versions don't count toward the cap.</span></span>
- <span data-ttu-id="39ad1-157">Indexy v paměťově optimalizovaných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="39ad1-157">Indexes on memory-optimized tables.</span></span>
- <span data-ttu-id="39ad1-158">Provozní režie operací ALTER TABLE.</span><span class="sxs-lookup"><span data-stu-id="39ad1-158">Operational overhead of ALTER TABLE operations.</span></span>

<span data-ttu-id="39ad1-159">Jestli jste nedosáhli krytky, obdržíte chybu na více systémů kvóty a jste již nebude možné vložit nebo aktualizovat data.</span><span class="sxs-lookup"><span data-stu-id="39ad1-159">If you hit the cap, you receive an out-of-quota error, and you are no longer able to insert or update data.</span></span> <span data-ttu-id="39ad1-160">Pro zmírnění této chybě, odstranit data nebo zvyšte cenová úroveň databáze nebo fond.</span><span class="sxs-lookup"><span data-stu-id="39ad1-160">To mitigate this error, delete data or increase the pricing tier of the database or pool.</span></span>

<span data-ttu-id="39ad1-161">Podrobnosti o sledování využití úložiště OLTP v paměti a konfiguraci výstrah při téměř zásahu krytky najdete v tématu [monitorování v paměti úložiště](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="39ad1-161">For details about monitoring In-Memory OLTP storage utilization and configuring alerts when you almost hit the cap, see [Monitor In-Memory storage](sql-database-in-memory-oltp-monitoring.md).</span></span>

#### <a name="about-elastic-pools"></a><span data-ttu-id="39ad1-162">O elastické fondy</span><span class="sxs-lookup"><span data-stu-id="39ad1-162">About elastic pools</span></span>

<span data-ttu-id="39ad1-163">S elastické fondy úložiště OLTP v paměti je sdílet všechny databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="39ad1-163">With elastic pools, the In-Memory OLTP storage is shared across all databases in the pool.</span></span> <span data-ttu-id="39ad1-164">Proto využití v jedné databáze může potenciálně ovlivnit jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="39ad1-164">Therefore, the usage in one database can potentially affect other databases.</span></span> <span data-ttu-id="39ad1-165">Jsou dvě jejich zmírnění pro toto:</span><span class="sxs-lookup"><span data-stu-id="39ad1-165">Two mitigations for this are:</span></span>

- <span data-ttu-id="39ad1-166">Nakonfigurujte maximální-počet jednotek eDTU pro databáze, která je nižší než počet eDTU pro fond jako celek.</span><span class="sxs-lookup"><span data-stu-id="39ad1-166">Configure a Max-eDTU for databases that is lower than the eDTU count for the pool as a whole.</span></span> <span data-ttu-id="39ad1-167">Tento maximální caps využití úložiště OLTP v paměti, všechny databáze ve fondu, velikost, která odpovídá počet eDTU.</span><span class="sxs-lookup"><span data-stu-id="39ad1-167">This maximum caps the In-Memory OLTP storage utilization, in any database in the pool, to the size that corresponds to the eDTU count.</span></span>
- <span data-ttu-id="39ad1-168">Nakonfigurujte Min eDTU, která je větší než 0.</span><span class="sxs-lookup"><span data-stu-id="39ad1-168">Configure a Min-eDTU that is greater than 0.</span></span> <span data-ttu-id="39ad1-169">Toto minimum zaručuje, že každá databáze ve fondu má velikost úložiště k dispozici OLTP v paměti, která odpovídá nakonfigurované jednotek eDTU Min.</span><span class="sxs-lookup"><span data-stu-id="39ad1-169">This minimum guarantees that each database in the pool has the amount of available In-Memory OLTP storage that corresponds to the configured Min-eDTU.</span></span>

### <a name="data-size-and-storage-for-columnstore-indexes"></a><span data-ttu-id="39ad1-170">Velikost dat a úložiště pro indexy columnstore</span><span class="sxs-lookup"><span data-stu-id="39ad1-170">Data size and storage for columnstore indexes</span></span>

<span data-ttu-id="39ad1-171">Indexy Columnstore nejsou povinné, aby se vešla do paměti.</span><span class="sxs-lookup"><span data-stu-id="39ad1-171">Columnstore indexes aren't required to fit in memory.</span></span> <span data-ttu-id="39ad1-172">Proto je krytky pouze na velikost indexy maximální celkovou velikost databáze, která je popsána v [úrovně služeb SQL Database](sql-database-service-tiers.md) článku.</span><span class="sxs-lookup"><span data-stu-id="39ad1-172">Therefore, the only cap on the size of the indexes is the maximum overall database size, which is documented in the [SQL Database service tiers](sql-database-service-tiers.md) article.</span></span>

<span data-ttu-id="39ad1-173">Při použití Clusterované indexy columnstore sloupcovém komprese se používá pro základní tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="39ad1-173">When you use clustered columnstore indexes, columnar compression is used for the base table storage.</span></span> <span data-ttu-id="39ad1-174">Tato komprese může výrazně snížit nároky na úložiště dat uživatele, což znamená, že můžete začlenit další data v databázi.</span><span class="sxs-lookup"><span data-stu-id="39ad1-174">This compression can significantly reduce the storage footprint of your user data, which means that you can fit more data in the database.</span></span> <span data-ttu-id="39ad1-175">A komprese může být zvýšena další s [sloupcovém archivace komprese](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span><span class="sxs-lookup"><span data-stu-id="39ad1-175">And the compression can be further increased with [columnar archival compression](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span></span> <span data-ttu-id="39ad1-176">Komprese, můžete dosáhnout závisí na povaze data, ale 10krát komprese není.</span><span class="sxs-lookup"><span data-stu-id="39ad1-176">The amount of compression that you can achieve depends on the nature of the data, but 10 times the compression is not uncommon.</span></span>

<span data-ttu-id="39ad1-177">Například pokud máte databázi s maximální velikostí 1 terabajt (TB) a dosáhnout 10krát komprese pomocí indexy columnstore, můžete začlenit celkem 10 TB dat uživatele v databázi.</span><span class="sxs-lookup"><span data-stu-id="39ad1-177">For example, if you have a database with a maximum size of 1 terabyte (TB) and you achieve 10 times the compression by using columnstore indexes, you can fit a total of 10 TB of user data in the database.</span></span>

<span data-ttu-id="39ad1-178">Při použití neclusterovaných indexů columnstore základní tabulka je pořád uložená ve formátu tradiční rowstore.</span><span class="sxs-lookup"><span data-stu-id="39ad1-178">When you use nonclustered columnstore indexes, the base table is still stored in the traditional rowstore format.</span></span> <span data-ttu-id="39ad1-179">Proto nejsou úspory úložiště stejnou velikost jako s Clusterované indexy columnstore.</span><span class="sxs-lookup"><span data-stu-id="39ad1-179">Therefore, the storage savings aren't as big as with clustered columnstore indexes.</span></span> <span data-ttu-id="39ad1-180">Pokud chcete nahradit počet tradiční neclusterované indexy s indexem columnstore jeden, uvidíte stále celkové úspory v nároky na úložiště pro tabulku.</span><span class="sxs-lookup"><span data-stu-id="39ad1-180">However, if you're replacing a number of traditional nonclustered indexes with a single columnstore index, you can still see an overall savings in the storage footprint for the table.</span></span>

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a><span data-ttu-id="39ad1-181">Přesunutí databází, které používají technologie v paměti mezi cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="39ad1-181">Moving databases that use In-Memory technologies between pricing tiers</span></span>

<span data-ttu-id="39ad1-182">Nejsou nikdy žádné nekompatibility nebo jiné problémy při upgradu na vyšší cenová úroveň, například z standardní, Premium.</span><span class="sxs-lookup"><span data-stu-id="39ad1-182">There are never any incompatibilities or other problems when you upgrade to a higher pricing tier, such as from Standard to Premium.</span></span> <span data-ttu-id="39ad1-183">K dispozici funkce a prostředky pouze zvýšit.</span><span class="sxs-lookup"><span data-stu-id="39ad1-183">The available functionality and resources only increase.</span></span>

<span data-ttu-id="39ad1-184">Ale přechod na starší verzi cenové úrovně může mít negativní vliv na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="39ad1-184">But downgrading the pricing tier can negatively impact your database.</span></span> <span data-ttu-id="39ad1-185">Dopad je obzvláště zřejmá při downgradovat z úrovně Premium standardní nebo základní Pokud databáze obsahuje objekty OLTP v paměti.</span><span class="sxs-lookup"><span data-stu-id="39ad1-185">The impact is especially apparent when you downgrade from Premium to Standard or Basic when your database contains In-Memory OLTP objects.</span></span> <span data-ttu-id="39ad1-186">Paměťově optimalizované tabulky a indexy columnstore jsou k dispozici po downgrade (i v případě, že zůstanou viditelné).</span><span class="sxs-lookup"><span data-stu-id="39ad1-186">Memory-optimized tables, and columnstore indexes, are unavailable after the downgrade (even if they remain visible).</span></span> <span data-ttu-id="39ad1-187">Stejné aspekty platí při snížení cenová úroveň fondu elastické databáze, nebo přesunutí databáze s technologiemi v paměti do Standard a Basic elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="39ad1-187">The same considerations apply when you're lowering the pricing tier of an elastic pool, or moving a database with In-Memory technologies, into a Standard or Basic elastic pool.</span></span>

### <a name="in-memory-oltp"></a><span data-ttu-id="39ad1-188">OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="39ad1-188">In-Memory OLTP</span></span>

<span data-ttu-id="39ad1-189">*Přechod na starší verzi na Basic nebo Standard*: OLTP v paměti není podporována v databázích v úroveň Standard nebo Basic.</span><span class="sxs-lookup"><span data-stu-id="39ad1-189">*Downgrading to Basic/Standard*: In-Memory OLTP isn't supported in databases in the Standard or Basic tier.</span></span> <span data-ttu-id="39ad1-190">Kromě toho není možné přesunout databáze, která obsahuje všechny objekty OLTP v paměti na úroveň Standard nebo Basic.</span><span class="sxs-lookup"><span data-stu-id="39ad1-190">In addition, it isn't possible to move a database that has any In-Memory OLTP objects to the Standard or Basic tier.</span></span>

<span data-ttu-id="39ad1-191">Předtím, než jste starší verzi databáze, která se standardní a základní, odeberte všechny paměťově optimalizované tabulky a typů tabulek, jakož i všechny nativně Kompilované moduly T-SQL.</span><span class="sxs-lookup"><span data-stu-id="39ad1-191">Before you downgrade the database to Standard/Basic, remove all memory-optimized tables and table types, as well as all natively compiled T-SQL modules.</span></span>

<span data-ttu-id="39ad1-192">Není programový způsob, jak pochopit, jestli na danou databázi podporuje OLTP v paměti.</span><span class="sxs-lookup"><span data-stu-id="39ad1-192">There is a programmatic way to understand whether a given database supports In-Memory OLTP.</span></span> <span data-ttu-id="39ad1-193">Můžete spustit následující dotaz jazyka Transact-SQL:</span><span class="sxs-lookup"><span data-stu-id="39ad1-193">You can execute the following Transact-SQL query:</span></span>

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

<span data-ttu-id="39ad1-194">Pokud dotaz vrátí **1**, OLTP v paměti je podporováno v této databázi.</span><span class="sxs-lookup"><span data-stu-id="39ad1-194">If the query returns **1**, In-Memory OLTP is supported in this database.</span></span>


<span data-ttu-id="39ad1-195">*Přechod na starší verzi nižší úrovně Premium*: Data v paměťově optimalizovaných tabulkách musí být přizpůsobena úložiště OLTP v paměti, které souvisí s cenová úroveň databáze nebo není k dispozici elastický fond.</span><span class="sxs-lookup"><span data-stu-id="39ad1-195">*Downgrading to a lower Premium tier*: Data in memory-optimized tables must fit within the In-Memory OLTP storage that is associated with the pricing tier of the database or is available in the elastic pool.</span></span> <span data-ttu-id="39ad1-196">Pokud se pokusíte snížit cenovou úroveň, nebo přesunout databáze ve fondu, který nemá dostatek dostupné úložiště OLTP v paměti se operace nezdaří.</span><span class="sxs-lookup"><span data-stu-id="39ad1-196">If you try to lower the pricing tier or move the database into a pool that doesn't have enough available In-Memory OLTP storage, the operation fails.</span></span>

### <a name="columnstore-indexes"></a><span data-ttu-id="39ad1-197">Indexy Columnstore</span><span class="sxs-lookup"><span data-stu-id="39ad1-197">Columnstore indexes</span></span>

<span data-ttu-id="39ad1-198">*Přechod na starší verzi Basic nebo Standard*: indexy Columnstore jsou podporovány pouze na cenová úroveň Premium a ne na úrovních Standard nebo Basic.</span><span class="sxs-lookup"><span data-stu-id="39ad1-198">*Downgrading to Basic or Standard*: Columnstore indexes are supported only on the Premium pricing tier, and not on the Standard or Basic tiers.</span></span> <span data-ttu-id="39ad1-199">Když jste se downgradovat databázi Standard a Basic, stane indexu columnstore není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="39ad1-199">When you downgrade your database to Standard or Basic, your columnstore index becomes unavailable.</span></span> <span data-ttu-id="39ad1-200">Systém udržuje indexu columnstore, ale nikdy využívá index.</span><span class="sxs-lookup"><span data-stu-id="39ad1-200">The system maintains your columnstore index, but it never leverages the index.</span></span> <span data-ttu-id="39ad1-201">Pokud později upgradujete zpět na Premium, je okamžitě připraven znovu využít indexu columnstore.</span><span class="sxs-lookup"><span data-stu-id="39ad1-201">If you later upgrade back to Premium, your columnstore index is immediately ready to be leveraged again.</span></span>

<span data-ttu-id="39ad1-202">Pokud máte **clusterové** columnstore index, celé tabulky nedostupný po vrstvy přechod na starší verzi.</span><span class="sxs-lookup"><span data-stu-id="39ad1-202">If you have a **clustered** columnstore index, the whole table becomes unavailable after tier downgrade.</span></span> <span data-ttu-id="39ad1-203">Proto doporučujeme vyřaďte všechny *clusterové* indexy columnstore před downgradovat databáze nižší než úroveň Premium.</span><span class="sxs-lookup"><span data-stu-id="39ad1-203">Therefore we recommend that you drop all *clustered* columnstore indexes before you downgrade your database below the Premium tier.</span></span>

<span data-ttu-id="39ad1-204">*Přechod na starší verzi nižší úrovně Premium*: Tento přechod na starší verzi úspěšná, pokud odpovídá celé databáze v rámci maximální velikost pro cíl cenová úroveň, nebo dostupné úložiště v elastickém fondu.</span><span class="sxs-lookup"><span data-stu-id="39ad1-204">*Downgrading to a lower Premium tier*: This downgrade succeeds if the whole database fits within the maximum database size for the target pricing tier, or within the available storage in the elastic pool.</span></span> <span data-ttu-id="39ad1-205">Neexistuje žádný konkrétní vliv z indexů columnstore.</span><span class="sxs-lookup"><span data-stu-id="39ad1-205">There is no specific impact from the columnstore indexes.</span></span>


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a><span data-ttu-id="39ad1-206">1. Instalace ukázkové OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="39ad1-206">1. Install the In-Memory OLTP sample</span></span>

<span data-ttu-id="39ad1-207">Ukázkové databáze AdventureWorksLT můžete vytvořit pomocí několika kliknutí v [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="39ad1-207">You can create the AdventureWorksLT sample database with a few clicks in the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="39ad1-208">Pak kroků v této části popisují, jak můžete rozšířit vaše databáze AdventureWorksLT s objekty OLTP v paměti a předvedení výkonnostních výhod.</span><span class="sxs-lookup"><span data-stu-id="39ad1-208">Then, the steps in this section explain how you can enrich your AdventureWorksLT database with In-Memory OLTP objects and demonstrate performance benefits.</span></span>

<span data-ttu-id="39ad1-209">Více zneužívající vlastností prohlížeče, ale vizuálně výkonu ukázku pro OLTP v paměti najdete v části:</span><span class="sxs-lookup"><span data-stu-id="39ad1-209">For a more simplistic, but more visually appealing performance demo for In-Memory OLTP, see:</span></span>

- <span data-ttu-id="39ad1-210">Verze: [v – paměť oltp-demo-verze 1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span><span class="sxs-lookup"><span data-stu-id="39ad1-210">Release: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span></span>
- <span data-ttu-id="39ad1-211">Zdrojový kód: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span><span class="sxs-lookup"><span data-stu-id="39ad1-211">Source code: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span></span>

#### <a name="installation-steps"></a><span data-ttu-id="39ad1-212">Postup instalace</span><span class="sxs-lookup"><span data-stu-id="39ad1-212">Installation steps</span></span>

1. <span data-ttu-id="39ad1-213">V [portál Azure](https://portal.azure.com/), vytvořte na serveru databáze Premium.</span><span class="sxs-lookup"><span data-stu-id="39ad1-213">In the [Azure portal](https://portal.azure.com/), create a Premium database on a server.</span></span> <span data-ttu-id="39ad1-214">Nastavte **zdroj** ukázkové databáze AdventureWorksLT.</span><span class="sxs-lookup"><span data-stu-id="39ad1-214">Set the **Source** to the AdventureWorksLT sample database.</span></span> <span data-ttu-id="39ad1-215">Podrobné pokyny najdete v tématu [vytvořit svoji první databázi Azure SQL](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="39ad1-215">For detailed instructions, see [Create your first Azure SQL database](sql-database-get-started-portal.md).</span></span>

2. <span data-ttu-id="39ad1-216">Připojení k databázi s SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="39ad1-216">Connect to the database with SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

3. <span data-ttu-id="39ad1-217">Kopírování [OLTP v paměti jazyka Transact-SQL skriptu](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) do schránky.</span><span class="sxs-lookup"><span data-stu-id="39ad1-217">Copy the [In-Memory OLTP Transact-SQL script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) to your clipboard.</span></span> <span data-ttu-id="39ad1-218">Skriptu T-SQL vytváří objekty nezbytné v paměti ukázkové databáze AdventureWorksLT, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="39ad1-218">The T-SQL script creates the necessary In-Memory objects in the AdventureWorksLT sample database that you created in step 1.</span></span>

4. <span data-ttu-id="39ad1-219">Vložit do aplikace SSMS skriptu T-SQL a poté spusťte tento skript.</span><span class="sxs-lookup"><span data-stu-id="39ad1-219">Paste the T-SQL script into SSMS, and then execute the script.</span></span> <span data-ttu-id="39ad1-220">`MEMORY_OPTIMIZED = ON` Příkazy CREATE TABLE klauzule jsou zásadní.</span><span class="sxs-lookup"><span data-stu-id="39ad1-220">The `MEMORY_OPTIMIZED = ON` clause CREATE TABLE statements are crucial.</span></span> <span data-ttu-id="39ad1-221">Například:</span><span class="sxs-lookup"><span data-stu-id="39ad1-221">For example:</span></span>


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a><span data-ttu-id="39ad1-222">Chyba 40536</span><span class="sxs-lookup"><span data-stu-id="39ad1-222">Error 40536</span></span>


<span data-ttu-id="39ad1-223">Pokud se zobrazí chyba 40536 při spuštění skriptu T-SQL, spusťte následující skript T-SQL, chcete-li ověřit, jestli podporuje databázi v paměti:</span><span class="sxs-lookup"><span data-stu-id="39ad1-223">If you get error 40536 when you run the T-SQL script, run the following T-SQL script to verify whether the database supports In-Memory:</span></span>


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


<span data-ttu-id="39ad1-224">Důsledkem **0** znamená, že v paměti není podporován, a **1** znamená, že je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="39ad1-224">A result of **0** means that In-Memory isn't supported, and **1** means that it is supported.</span></span> <span data-ttu-id="39ad1-225">A diagnostikovat problém, zkontrolujte, zda databáze na úroveň služeb Premium.</span><span class="sxs-lookup"><span data-stu-id="39ad1-225">To diagnose the problem, ensure that the database is at the Premium service tier.</span></span>


#### <a name="about-the-created-memory-optimized-items"></a><span data-ttu-id="39ad1-226">O vytvořených položek paměťově optimalizované</span><span class="sxs-lookup"><span data-stu-id="39ad1-226">About the created memory-optimized items</span></span>

<span data-ttu-id="39ad1-227">**Tabulky**: Ukázka obsahuje paměťově optimalizované tabulky:</span><span class="sxs-lookup"><span data-stu-id="39ad1-227">**Tables**: The sample contains the following memory-optimized tables:</span></span>

- <span data-ttu-id="39ad1-228">SalesLT.Product_inmem</span><span class="sxs-lookup"><span data-stu-id="39ad1-228">SalesLT.Product_inmem</span></span>
- <span data-ttu-id="39ad1-229">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="39ad1-229">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="39ad1-230">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="39ad1-230">SalesLT.SalesOrderDetail_inmem</span></span>
- <span data-ttu-id="39ad1-231">Demo.DemoSalesOrderHeaderSeed</span><span class="sxs-lookup"><span data-stu-id="39ad1-231">Demo.DemoSalesOrderHeaderSeed</span></span>
- <span data-ttu-id="39ad1-232">Demo.DemoSalesOrderDetailSeed</span><span class="sxs-lookup"><span data-stu-id="39ad1-232">Demo.DemoSalesOrderDetailSeed</span></span>


<span data-ttu-id="39ad1-233">Paměťově optimalizované tabulky prostřednictvím si můžete prohlédnout **Průzkumník objektů** v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="39ad1-233">You can inspect memory-optimized tables through the **Object Explorer** in SSMS.</span></span> <span data-ttu-id="39ad1-234">Klikněte pravým tlačítkem na **tabulky** > **filtru** > **nastavení filtru** > **je paměťově optimalizovaná**.</span><span class="sxs-lookup"><span data-stu-id="39ad1-234">Right-click **Tables** > **Filter** > **Filter Settings** > **Is Memory Optimized**.</span></span> <span data-ttu-id="39ad1-235">Hodnota se rovná 1.</span><span class="sxs-lookup"><span data-stu-id="39ad1-235">The value equals 1.</span></span>


<span data-ttu-id="39ad1-236">Nebo můžete dát dotaz na zobrazení katalogu, jako například:</span><span class="sxs-lookup"><span data-stu-id="39ad1-236">Or you can query the catalog views, such as:</span></span>


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


<span data-ttu-id="39ad1-237">**Nativně kompilované uložené procedury**: SalesLT.usp_InsertSalesOrder_inmem si můžete prohlédnout pomocí zobrazení katalogu dotazu:</span><span class="sxs-lookup"><span data-stu-id="39ad1-237">**Natively compiled stored procedure**: You can inspect SalesLT.usp_InsertSalesOrder_inmem through a catalog view query:</span></span>


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a><span data-ttu-id="39ad1-238">Spuštění ukázky pracovního vytížení OLTP</span><span class="sxs-lookup"><span data-stu-id="39ad1-238">Run the sample OLTP workload</span></span>

<span data-ttu-id="39ad1-239">Jediným rozdílem mezi následující dva *uložené procedury* je, že první postup používá verzích paměťově optimalizované tabulky, zatímco druhý postup používá regulární tabulky na disku:</span><span class="sxs-lookup"><span data-stu-id="39ad1-239">The only difference between the following two *stored procedures* is that the first procedure uses memory-optimized versions of the tables, while the second procedure uses the regular on-disk tables:</span></span>

- <span data-ttu-id="39ad1-240">SalesLT**.** usp_InsertSalesOrder**_inmem**</span><span class="sxs-lookup"><span data-stu-id="39ad1-240">SalesLT**.**usp_InsertSalesOrder**_inmem**</span></span>
- <span data-ttu-id="39ad1-241">SalesLT**.** usp_InsertSalesOrder**_ondisk**</span><span class="sxs-lookup"><span data-stu-id="39ad1-241">SalesLT**.**usp_InsertSalesOrder**_ondisk**</span></span>


<span data-ttu-id="39ad1-242">V této části najdete postup používání užitečný v **ostress.exe** nástroj provést dvě uložené procedury na stressful úrovních.</span><span class="sxs-lookup"><span data-stu-id="39ad1-242">In this section, you see how to use the handy **ostress.exe** utility to execute the two stored procedures at stressful levels.</span></span> <span data-ttu-id="39ad1-243">Jak dlouho trvá pro spustí dvě přízvuk dokončíte, můžete porovnat.</span><span class="sxs-lookup"><span data-stu-id="39ad1-243">You can compare how long it takes for the two stress runs to finish.</span></span>


<span data-ttu-id="39ad1-244">Když spustíte ostress.exe, doporučujeme předat hodnoty parametrů, které jsou určené pro obě z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="39ad1-244">When you run ostress.exe, we recommend that you pass parameter values designed for both of the following:</span></span>

- <span data-ttu-id="39ad1-245">Spustit velký počet souběžných připojení pomocí - n100.</span><span class="sxs-lookup"><span data-stu-id="39ad1-245">Run a large number of concurrent connections, by using -n100.</span></span>
- <span data-ttu-id="39ad1-246">Pomocí mít každý připojení smyčku stovky dobu, pomocí - r500.</span><span class="sxs-lookup"><span data-stu-id="39ad1-246">Have each connection loop hundreds of times, by using -r500.</span></span>


<span data-ttu-id="39ad1-247">Ale můžete chtít začít s mnohem menšími hodnoty jako - n10 a - r 50 zajistit, že vše funguje.</span><span class="sxs-lookup"><span data-stu-id="39ad1-247">However, you might want to start with much smaller values like -n10 and -r50 to ensure that everything is working.</span></span>


### <a name="script-for-ostressexe"></a><span data-ttu-id="39ad1-248">Skript pro ostress.exe</span><span class="sxs-lookup"><span data-stu-id="39ad1-248">Script for ostress.exe</span></span>


<span data-ttu-id="39ad1-249">V této části zobrazí skriptu T-SQL, vložené v našem ostress.exe příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="39ad1-249">This section displays the T-SQL script that is embedded in our ostress.exe command line.</span></span> <span data-ttu-id="39ad1-250">Tento skript využívá položky, které byly vytvořeny pomocí skriptu T-SQL, který jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="39ad1-250">The script uses items that were created by the T-SQL script that you installed earlier.</span></span>


<span data-ttu-id="39ad1-251">Následující skript vloží ukázka prodejní objednávky s pěti položek řádku do těchto paměťově optimalizované *tabulky*:</span><span class="sxs-lookup"><span data-stu-id="39ad1-251">The following script inserts a sample sales order with five line items into the following memory-optimized *tables*:</span></span>

- <span data-ttu-id="39ad1-252">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="39ad1-252">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="39ad1-253">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="39ad1-253">SalesLT.SalesOrderDetail_inmem</span></span>


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


<span data-ttu-id="39ad1-254">Chcete-li *_ondisk* verzi předchozí skriptu T-SQL pro ostress.exe, měli byste nahradit oba výskyty *_inmem* substring s *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="39ad1-254">To make the *_ondisk* version of the preceding T-SQL script for ostress.exe, you would replace both occurrences of the *_inmem* substring with *_ondisk*.</span></span> <span data-ttu-id="39ad1-255">Tyto náhrady ovlivnit názvy tabulek a uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="39ad1-255">These replacements affect the names of tables and stored procedures.</span></span>


### <a name="install-rml-utilities-and-ostress"></a><span data-ttu-id="39ad1-256">Instalace nástrojů RML a ostress</span><span class="sxs-lookup"><span data-stu-id="39ad1-256">Install RML utilities and ostress</span></span>


<span data-ttu-id="39ad1-257">V ideálním případě by plánujete spouštět ostress.exe na virtuální počítač Azure (VM).</span><span class="sxs-lookup"><span data-stu-id="39ad1-257">Ideally, you would plan to run ostress.exe on an Azure virtual machine (VM).</span></span> <span data-ttu-id="39ad1-258">Měli byste vytvořit [virtuálního počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) ve stejné Azure geografické oblasti, kde se nachází databáze AdventureWorksLT.</span><span class="sxs-lookup"><span data-stu-id="39ad1-258">You would create an [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) in the same Azure geographic region where your AdventureWorksLT database resides.</span></span> <span data-ttu-id="39ad1-259">Ale ostress.exe na svém přenosném počítači může spouštět místo.</span><span class="sxs-lookup"><span data-stu-id="39ad1-259">But you can run ostress.exe on your laptop instead.</span></span>


<span data-ttu-id="39ad1-260">Ve virtuálním počítači nebo na ať hostitele, můžete zvolit, nainstalujte nástroje opětovného přehrání Markup Language (RML).</span><span class="sxs-lookup"><span data-stu-id="39ad1-260">On the VM, or on whatever host you choose, install the Replay Markup Language (RML) utilities.</span></span> <span data-ttu-id="39ad1-261">Nástroje zahrnují ostress.exe.</span><span class="sxs-lookup"><span data-stu-id="39ad1-261">The utilities include ostress.exe.</span></span>

<span data-ttu-id="39ad1-262">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="39ad1-262">For more information, see:</span></span>
- <span data-ttu-id="39ad1-263">Ostress.exe diskuse ve [ukázkové databáze pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="39ad1-263">The ostress.exe discussion in [Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="39ad1-264">[Ukázkové databáze pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="39ad1-264">[Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="39ad1-265">[Blogu pro instalaci ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span><span class="sxs-lookup"><span data-stu-id="39ad1-265">The [blog for installing ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span></span>



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a><span data-ttu-id="39ad1-266">Spustit *_inmem* nejprve vystavila zátěži pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="39ad1-266">Run the *_inmem* stress workload first</span></span>


<span data-ttu-id="39ad1-267">Můžete použít *RML Cmd výzva* okno spustíte naše ostress.exe příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="39ad1-267">You can use an *RML Cmd Prompt* window to run our ostress.exe command line.</span></span> <span data-ttu-id="39ad1-268">Parametry příkazového řádku přímé ostress na:</span><span class="sxs-lookup"><span data-stu-id="39ad1-268">The command-line parameters direct ostress to:</span></span>

- <span data-ttu-id="39ad1-269">Připojení 100 běželo (-n100).</span><span class="sxs-lookup"><span data-stu-id="39ad1-269">Run 100 connections concurrently (-n100).</span></span>
- <span data-ttu-id="39ad1-270">Každé připojení 50 času spuštění skriptu T-SQL (-r 50).</span><span class="sxs-lookup"><span data-stu-id="39ad1-270">Have each connection run the T-SQL script 50 times (-r50).</span></span>


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


<span data-ttu-id="39ad1-271">Spuštění předchozího ostress.exe příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="39ad1-271">To run the preceding ostress.exe command line:</span></span>


1. <span data-ttu-id="39ad1-272">Obnovení dat obsah databáze spuštěním následujícího příkazu v aplikaci SSMS odstranit všechna data, která byla vložená všech předchozích spuštění:</span><span class="sxs-lookup"><span data-stu-id="39ad1-272">Reset the database data content by running the following command in SSMS, to delete all the data that was inserted by any previous runs:</span></span>

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. <span data-ttu-id="39ad1-273">Text předchozí příkazového řádku ostress.exe kopírovat do schránky.</span><span class="sxs-lookup"><span data-stu-id="39ad1-273">Copy the text of the preceding ostress.exe command line to your clipboard.</span></span>

3. <span data-ttu-id="39ad1-274">Nahraďte `<placeholders>` pro parametry -S - U -P -d s správné skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="39ad1-274">Replace the `<placeholders>` for the parameters -S -U -P -d with the correct real values.</span></span>

4. <span data-ttu-id="39ad1-275">V okně RML Cmd spusťte do upravená příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="39ad1-275">Run your edited command line in an RML Cmd window.</span></span>


#### <a name="result-is-a-duration"></a><span data-ttu-id="39ad1-276">Výsledkem je, doba trvání</span><span class="sxs-lookup"><span data-stu-id="39ad1-276">Result is a duration</span></span>


<span data-ttu-id="39ad1-277">Po dokončení ostress.exe zapíše spuštění doba trvání jako jeho poslední řádek výstupu v okně RML Cmd.</span><span class="sxs-lookup"><span data-stu-id="39ad1-277">When ostress.exe finishes, it writes the run duration as its final line of output in the RML Cmd window.</span></span> <span data-ttu-id="39ad1-278">Například kratší testovací běh už bylo asi 1,5 minuty:</span><span class="sxs-lookup"><span data-stu-id="39ad1-278">For example, a shorter test run lasted about 1.5 minutes:</span></span>

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a><span data-ttu-id="39ad1-279">Resetovat, upravit pro *_ondisk*, pak znovu spusťte</span><span class="sxs-lookup"><span data-stu-id="39ad1-279">Reset, edit for *_ondisk*, then rerun</span></span>


<span data-ttu-id="39ad1-280">Až budete mít výsledek z *_inmem* spustit, proveďte následující kroky pro *_ondisk* spustit:</span><span class="sxs-lookup"><span data-stu-id="39ad1-280">After you have the result from the *_inmem* run, perform the following steps for the *_ondisk* run:</span></span>


1. <span data-ttu-id="39ad1-281">Obnovit databázi v SSMS pro odstranění všech dat, který byl vložen podle předchozího spuštění spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ad1-281">Reset the database by running the following command in SSMS to delete all the data that was inserted by the previous run:</span></span>
```
EXECUTE Demo.usp_DemoReset;
```

2. <span data-ttu-id="39ad1-282">Upravit ostress.exe příkazového řádku k nahrazení všech *_inmem* s *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="39ad1-282">Edit the ostress.exe command line to replace all *_inmem* with *_ondisk*.</span></span>

3. <span data-ttu-id="39ad1-283">Spusťte ostress.exe podruhé a zaznamenat výsledek doba trvání.</span><span class="sxs-lookup"><span data-stu-id="39ad1-283">Rerun ostress.exe for the second time, and capture the duration result.</span></span>

4. <span data-ttu-id="39ad1-284">Znovu obnovte databázi (pro odstranění jeho zodpovědné, může být velký objem dat test).</span><span class="sxs-lookup"><span data-stu-id="39ad1-284">Again, reset the database (for responsibly deleting what can be a large amount of test data).</span></span>


#### <a name="expected-comparison-results"></a><span data-ttu-id="39ad1-285">Očekávaný porovnání výsledků</span><span class="sxs-lookup"><span data-stu-id="39ad1-285">Expected comparison results</span></span>

<span data-ttu-id="39ad1-286">Naše testy v paměti ukázaly, že výkonu vylepšené podle **devětkrát** pro tento zneužívající vlastností prohlížeče zatížení s ostress systémem virtuálního počítače Azure ve stejné oblasti Azure jako databáze.</span><span class="sxs-lookup"><span data-stu-id="39ad1-286">Our In-Memory tests have shown that performance improved by **nine times** for this simplistic workload, with ostress running on an Azure VM in the same Azure region as the database.</span></span>

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a><span data-ttu-id="39ad1-287">2. Instalace ukázkové analýzy v paměti</span><span class="sxs-lookup"><span data-stu-id="39ad1-287">2. Install the In-Memory Analytics sample</span></span>


<span data-ttu-id="39ad1-288">V této části při porovnání výsledků vstupně-výstupní operace a statistiky, když používáte index columnstore a index tradiční b stromu.</span><span class="sxs-lookup"><span data-stu-id="39ad1-288">In this section, you compare the IO and statistics results when you're using a columnstore index versus a traditional b-tree index.</span></span>


<span data-ttu-id="39ad1-289">Pro analýzu v reálném čase na úloh s online zpracováním je často nejvhodnější použít neclusterovaný index columnstore.</span><span class="sxs-lookup"><span data-stu-id="39ad1-289">For real-time analytics on an OLTP workload, it's often best to use a nonclustered columnstore index.</span></span> <span data-ttu-id="39ad1-290">Podrobnosti najdete v tématu [popsané indexy Columnstore](http://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="39ad1-290">For details, see [Columnstore Indexes Described](http://msdn.microsoft.com/library/gg492088.aspx).</span></span>



### <a name="prepare-the-columnstore-analytics-test"></a><span data-ttu-id="39ad1-291">Příprava testovacího analytics columnstore</span><span class="sxs-lookup"><span data-stu-id="39ad1-291">Prepare the columnstore analytics test</span></span>


1. <span data-ttu-id="39ad1-292">Použití portálu Azure vytvořit novou databázi AdventureWorksLT od vzorku.</span><span class="sxs-lookup"><span data-stu-id="39ad1-292">Use the Azure portal to create a fresh AdventureWorksLT database from the sample.</span></span>
 - <span data-ttu-id="39ad1-293">Použijte tento přesný název.</span><span class="sxs-lookup"><span data-stu-id="39ad1-293">Use that exact name.</span></span>
 - <span data-ttu-id="39ad1-294">Vyberte všechny úroveň služeb Premium.</span><span class="sxs-lookup"><span data-stu-id="39ad1-294">Choose any Premium service tier.</span></span>

2. <span data-ttu-id="39ad1-295">Kopírování [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) do schránky.</span><span class="sxs-lookup"><span data-stu-id="39ad1-295">Copy the [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) to your clipboard.</span></span>
 - <span data-ttu-id="39ad1-296">Skriptu T-SQL vytváří objekty nezbytné v paměti ukázkové databáze AdventureWorksLT, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="39ad1-296">The T-SQL script creates the necessary In-Memory objects in the AdventureWorksLT sample database that you created in step 1.</span></span>
 - <span data-ttu-id="39ad1-297">Tento skript vytvoří tabulce dimenzí a dvě tabulky faktů.</span><span class="sxs-lookup"><span data-stu-id="39ad1-297">The script creates the Dimension table and two fact tables.</span></span> <span data-ttu-id="39ad1-298">Tabulky faktů se naplní 3.5 milionu řádků.</span><span class="sxs-lookup"><span data-stu-id="39ad1-298">The fact tables are populated with 3.5 million rows each.</span></span>
 - <span data-ttu-id="39ad1-299">Skript může trvat 15 minut.</span><span class="sxs-lookup"><span data-stu-id="39ad1-299">The script might take 15 minutes to complete.</span></span>

3. <span data-ttu-id="39ad1-300">Vložit do aplikace SSMS skriptu T-SQL a poté spusťte tento skript.</span><span class="sxs-lookup"><span data-stu-id="39ad1-300">Paste the T-SQL script into SSMS, and then execute the script.</span></span> <span data-ttu-id="39ad1-301">**COLUMNSTORE** – klíčové slovo v **CREATE INDEX** údajů je velmi důležitý, stejně jako na:</span><span class="sxs-lookup"><span data-stu-id="39ad1-301">The **COLUMNSTORE** keyword in the **CREATE INDEX** statement is crucial, as in:</span></span><br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. <span data-ttu-id="39ad1-302">Nastavit úroveň kompatibility 130 AdventureWorksLT:</span><span class="sxs-lookup"><span data-stu-id="39ad1-302">Set AdventureWorksLT to compatibility level 130:</span></span><br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    <span data-ttu-id="39ad1-303">Úroveň 130 přímo nesouvisí s funkcí v paměti.</span><span class="sxs-lookup"><span data-stu-id="39ad1-303">Level 130 is not directly related to In-Memory features.</span></span> <span data-ttu-id="39ad1-304">Ale úroveň 130 obecně poskytuje vyšší výkon dotazu než 120.</span><span class="sxs-lookup"><span data-stu-id="39ad1-304">But level 130 generally provides faster query performance than 120.</span></span>


#### <a name="key-tables-and-columnstore-indexes"></a><span data-ttu-id="39ad1-305">Klíče tabulky a indexy columnstore</span><span class="sxs-lookup"><span data-stu-id="39ad1-305">Key tables and columnstore indexes</span></span>


- <span data-ttu-id="39ad1-306">dbo. FactResellerSalesXL_CCI je tabulka, která má clusterovaný index columnstore, která obsahuje rozšířené komprese na *data* úroveň.</span><span class="sxs-lookup"><span data-stu-id="39ad1-306">dbo.FactResellerSalesXL_CCI is a table that has a clustered columnstore index, which has advanced compression at the *data* level.</span></span>

- <span data-ttu-id="39ad1-307">dbo. FactResellerSalesXL_PageCompressed je tabulku, která má ekvivalentní regulární clusterovaný index, který se komprimují jenom na *stránky* úroveň.</span><span class="sxs-lookup"><span data-stu-id="39ad1-307">dbo.FactResellerSalesXL_PageCompressed is a table that has an equivalent regular clustered index, which is compressed only at the *page* level.</span></span>


#### <a name="key-queries-to-compare-the-columnstore-index"></a><span data-ttu-id="39ad1-308">Klíče dotazů k porovnání columnstore index</span><span class="sxs-lookup"><span data-stu-id="39ad1-308">Key queries to compare the columnstore index</span></span>


<span data-ttu-id="39ad1-309">Existují [několik typů dotazu T-SQL, které můžete spustit](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) zobrazíte vylepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="39ad1-309">There are [several T-SQL query types that you can run](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) to see performance improvements.</span></span> <span data-ttu-id="39ad1-310">V kroku 2 ve skriptu T-SQL věnujte pozornost tento pár dotazů.</span><span class="sxs-lookup"><span data-stu-id="39ad1-310">In step 2 in the T-SQL script, pay attention to this pair of queries.</span></span> <span data-ttu-id="39ad1-311">Liší se pouze na jednom řádku:</span><span class="sxs-lookup"><span data-stu-id="39ad1-311">They differ only on one line:</span></span>


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


<span data-ttu-id="39ad1-312">Clusterovaný index columnstore probíhá FactResellerSalesXL\_KÚS tabulky.</span><span class="sxs-lookup"><span data-stu-id="39ad1-312">A clustered columnstore index is in the FactResellerSalesXL\_CCI table.</span></span>

<span data-ttu-id="39ad1-313">Následující výpis skriptu T-SQL vytiskne statistiky pro vstupně-výstupní operace a čas pro dotaz každé tabulky.</span><span class="sxs-lookup"><span data-stu-id="39ad1-313">The following T-SQL script excerpt prints statistics for IO and TIME for the query of each table.</span></span>


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

<span data-ttu-id="39ad1-314">V databázi s P2 cenovou úroveň bude pravděpodobně přibližně devětkrát zvýšení výkonu pro tento dotaz pomocí clusterovaný index columnstore v porovnání s tradiční index.</span><span class="sxs-lookup"><span data-stu-id="39ad1-314">In a database with the P2 pricing tier, you can expect about nine times the performance gain for this query by using the clustered columnstore index compared with the traditional index.</span></span> <span data-ttu-id="39ad1-315">S P15 bude pravděpodobně přibližně 57 časy zvýšení výkonu pomocí columnstore index.</span><span class="sxs-lookup"><span data-stu-id="39ad1-315">With P15, you can expect about 57 times the performance gain by using the columnstore index.</span></span>



## <a name="next-steps"></a><span data-ttu-id="39ad1-316">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39ad1-316">Next steps</span></span>

- [<span data-ttu-id="39ad1-317">Rychlý Start 1: Technologie OLTP v paměti pro dosažení vyššího výkonu T-SQL</span><span class="sxs-lookup"><span data-stu-id="39ad1-317">Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance</span></span>](http://msdn.microsoft.com/library/mt694156.aspx)

- [<span data-ttu-id="39ad1-318">Použití OLTP v paměti v aplikaci existující Azure SQL</span><span class="sxs-lookup"><span data-stu-id="39ad1-318">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

- <span data-ttu-id="39ad1-319">[Monitorování OLTP v paměti úložiště](sql-database-in-memory-oltp-monitoring.md) pro OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="39ad1-319">[Monitor In-Memory OLTP storage](sql-database-in-memory-oltp-monitoring.md) for In-Memory OLTP</span></span>


## <a name="additional-resources"></a><span data-ttu-id="39ad1-320">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="39ad1-320">Additional resources</span></span>

#### <a name="deeper-information"></a><span data-ttu-id="39ad1-321">Podrobnější informace.</span><span class="sxs-lookup"><span data-stu-id="39ad1-321">Deeper information</span></span>

- [<span data-ttu-id="39ad1-322">Zjistěte, jak kvora zdvojnásobí klíče databáze zatížení při snížení DTU 70 % s OLTP v paměti v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="39ad1-322">Learn how Quorum doubles key database’s workload while lowering DTU by 70% with In-Memory OLTP in SQL Database</span></span>](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [<span data-ttu-id="39ad1-323">OLTP v paměti v příspěvku na blogu databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="39ad1-323">In-Memory OLTP in Azure SQL Database Blog Post</span></span>](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [<span data-ttu-id="39ad1-324">Další informace o OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="39ad1-324">Learn about In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="39ad1-325">Další informace o indexy columnstore</span><span class="sxs-lookup"><span data-stu-id="39ad1-325">Learn about columnstore indexes</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)

- [<span data-ttu-id="39ad1-326">Další informace o provozní analýzu v reálném čase</span><span class="sxs-lookup"><span data-stu-id="39ad1-326">Learn about real-time operational analytics</span></span>](http://msdn.microsoft.com/library/dn817827.aspx)

- <span data-ttu-id="39ad1-327">V tématu [vzory běžné úlohy a posouzení migrace](http://msdn.microsoft.com/library/dn673538.aspx) (které popisuje vzory úlohy, kde OLTP v paměti běžně nabízí výrazné zvýšení výkonu)</span><span class="sxs-lookup"><span data-stu-id="39ad1-327">See [Common Workload Patterns and Migration Considerations](http://msdn.microsoft.com/library/dn673538.aspx) (which describes workload patterns where In-Memory OLTP commonly provides significant performance gains)</span></span>

#### <a name="application-design"></a><span data-ttu-id="39ad1-328">Návrh aplikace</span><span class="sxs-lookup"><span data-stu-id="39ad1-328">Application design</span></span>

- [<span data-ttu-id="39ad1-329">Paměť OLTP (v paměti optimalizace)</span><span class="sxs-lookup"><span data-stu-id="39ad1-329">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="39ad1-330">Použití OLTP v paměti v aplikaci existující Azure SQL</span><span class="sxs-lookup"><span data-stu-id="39ad1-330">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a><span data-ttu-id="39ad1-331">Nástroje</span><span class="sxs-lookup"><span data-stu-id="39ad1-331">Tools</span></span>

- [<span data-ttu-id="39ad1-332">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="39ad1-332">Azure portal</span></span>](https://portal.azure.com/)

- [<span data-ttu-id="39ad1-333">SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="39ad1-333">SQL Server Management Studio (SSMS)</span></span>](https://msdn.microsoft.com/library/mt238290.aspx)

- [<span data-ttu-id="39ad1-334">SQL Server Data Tools (SSDT)</span><span class="sxs-lookup"><span data-stu-id="39ad1-334">SQL Server Data Tools (SSDT)</span></span>](http://msdn.microsoft.com/library/mt204009.aspx)
