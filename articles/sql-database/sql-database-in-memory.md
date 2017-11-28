---
title: "technologie SQL databázi v paměti aaaAzure | Microsoft Docs"
description: "Azure SQL Database v paměti technologie výrazně zlepšit hello výkon transakcí a analýzy zatížení. Zjistěte, jak tootake výhod těchto technologií."
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
ms.openlocfilehash: 1bacd7297b2f9b018853088eabf2a2ee66a9cb43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a><span data-ttu-id="0bc83-104">Optimalizace výkonu pomocí technologie v paměti v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="0bc83-104">Optimize performance by using In-Memory technologies in SQL Database</span></span>

<span data-ttu-id="0bc83-105">Použití technologií v paměti ve službě Azure SQL Database, můžete dosáhnout zlepšení výkonu pomocí různých úloh: transakcí (online zpracování transakcí (OLTP)), analytics (online analytického zpracování (OLAP)) a ve smíšeném (hybridní transakce/analytického zpracování (HTAP)).</span><span class="sxs-lookup"><span data-stu-id="0bc83-105">By using In-Memory technologies in Azure SQL Database, you can achieve performance improvements with various workloads: transactional (online transactional processing (OLTP)), analytics (online analytical processing (OLAP)), and mixed (hybrid transaction/analytical processing (HTAP)).</span></span> <span data-ttu-id="0bc83-106">Z důvodu hello efektivnější dotazů a zpracování transakcí, v paměti technologie vám také pomoci tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="0bc83-106">Because of hello more efficient query and transaction processing, In-Memory technologies also help you tooreduce cost.</span></span> <span data-ttu-id="0bc83-107">Obvykle není nutné tooupgrade hello cenová úroveň zvýšení výkonu tooachieve hello databáze.</span><span class="sxs-lookup"><span data-stu-id="0bc83-107">You typically don't need tooupgrade hello pricing tier of hello database tooachieve performance gains.</span></span> <span data-ttu-id="0bc83-108">V některých případech i je možné snížit cenovou úroveň, při vylepšení výkonu s technologiemi v paměti se přesto zobrazuje hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-108">In some cases, you might even be able reduce hello pricing tier, while still seeing performance improvements with In-Memory technologies.</span></span>

<span data-ttu-id="0bc83-109">Zde jsou dva příklady, jak OLTP v paměti pomohl toosignificantly zlepšení výkonu:</span><span class="sxs-lookup"><span data-stu-id="0bc83-109">Here are two examples of how In-Memory OLTP helped toosignificantly improve performance:</span></span>

- <span data-ttu-id="0bc83-110">Pomocí OLTP v paměti, [kvora obchodními řešeními bylo možné toodouble jejich zatížení při současném zvyšování Dtu 70 %](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span><span class="sxs-lookup"><span data-stu-id="0bc83-110">By using In-Memory OLTP, [Quorum Business Solutions was able toodouble their workload while improving DTUs by 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span></span>
    - <span data-ttu-id="0bc83-111">DTU znamená *jednotek propustnosti databáze*, a obsahuje měření spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="0bc83-111">DTU means *database throughput unit*, and it includes a mesurement of resource consumption.</span></span>
- <span data-ttu-id="0bc83-112">Hello toto video ukazuje výrazné zlepšení spotřeby prostředků s ukázky pracovního vytížení: [OLTP v paměti v Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span><span class="sxs-lookup"><span data-stu-id="0bc83-112">hello following video demonstrates significant improvement in resource consumption with a sample workload: [In-Memory OLTP in Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span></span>
    - <span data-ttu-id="0bc83-113">Další podrobnosti najdete v tématu hello blogu: [OLTP v paměti v příspěvku blogu databáze SQL Azure](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span><span class="sxs-lookup"><span data-stu-id="0bc83-113">For more details, see hello blog post: [In-Memory OLTP in Azure SQL Database Blog Post](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span></span>

<span data-ttu-id="0bc83-114">Technologie v paměti jsou k dispozici v všechny databáze v úrovni Premium hello, včetně databází v elastické fondy Premium.</span><span class="sxs-lookup"><span data-stu-id="0bc83-114">In-Memory technologies are available in all databases in hello Premium tier, including databases in Premium elastic pools.</span></span>

<span data-ttu-id="0bc83-115">Hello následující video vysvětluje potenciální zvýšení výkonu s technologiemi v paměti ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="0bc83-115">hello following video explains potential performance gains with In-Memory technologies in Azure SQL Database.</span></span> <span data-ttu-id="0bc83-116">Pamatujte, že hello výkonnější, který se zobrazí vždy závisí na mnoha faktorech, včetně hello povaha hello zatížení a data, vzor přístupu hello databáze a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0bc83-116">Remember that hello performance gain that you see always depends on many factors, including hello nature of hello workload and data, access pattern of hello database, and so on.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

<span data-ttu-id="0bc83-117">Azure SQL Database má hello následující technologie v paměti:</span><span class="sxs-lookup"><span data-stu-id="0bc83-117">Azure SQL Database has hello following In-Memory technologies:</span></span>

- <span data-ttu-id="0bc83-118">*OLTP v paměti* zvyšuje propustnost a snižuje latence pro zpracování transakcí.</span><span class="sxs-lookup"><span data-stu-id="0bc83-118">*In-Memory OLTP* increases throughput and reduces latency for transaction processing.</span></span> <span data-ttu-id="0bc83-119">Scénáře využívající OLTP v paměti jsou: transakce vysokou propustností zpracování například obchodní a hraní, přijímání dat ze zařízení IoT, ukládání do mezipaměti, načtení dat a dočasné tabulky a scénáře proměnné tabulky nebo události.</span><span class="sxs-lookup"><span data-stu-id="0bc83-119">Scenarios that benefit from In-Memory OLTP are: high-throughput transaction processing such as trading and gaming, data ingestion from events or IoT devices, caching, data load, and temporary table and table variable scenarios.</span></span>
- <span data-ttu-id="0bc83-120">*Clusterované indexy columnstore* redukuje vašeho úložiště (až too10 časy) a zlepšení výkonu pro dotazy analýz a generování sestav.</span><span class="sxs-lookup"><span data-stu-id="0bc83-120">*Clustered columnstore indexes* reduce your storage footprint (up too10 times) and improve performance for reporting and analytics queries.</span></span> <span data-ttu-id="0bc83-121">Můžete ho použít s tabulky faktů vaše datové Tržiště toofit další data v databázi a zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="0bc83-121">You can use it with fact tables in your data marts toofit more data in your database and improve performance.</span></span> <span data-ttu-id="0bc83-122">Můžete také pomocí historických dat ve vaší tooarchive provozní databáze a být schopný tooquery too10 časů další data.</span><span class="sxs-lookup"><span data-stu-id="0bc83-122">Also, you can use it with historical data in your operational database tooarchive and be able tooquery up too10 times more data.</span></span>
- <span data-ttu-id="0bc83-123">*Neclusterovaných indexů columnstore* nápovědu HTAP můžete toogain v reálném čase přehled o svém podnikání prostřednictvím dotazování hello provozní databáze přímo, bez nutnosti toorun hello nákladné extrakce, transformace a načítání (ETL) proces a čekání pro hello datového skladu toobe naplněno.</span><span class="sxs-lookup"><span data-stu-id="0bc83-123">*Nonclustered columnstore indexes* for HTAP help you toogain real-time insights into your business through querying hello operational database directly, without hello need toorun an expensive extract, transform, and load (ETL) process and wait for hello data warehouse toobe populated.</span></span> <span data-ttu-id="0bc83-124">Neclusterovaných indexů columnstore povolí velmi rychlé spuštění analytické dotazy na databáze OLTP hello, a současně hello dopad na hello provozní úlohy.</span><span class="sxs-lookup"><span data-stu-id="0bc83-124">Nonclustered columnstore indexes allow very fast execution of analytics queries on hello OLTP database, while reducing hello impact on hello operational workload.</span></span>
- <span data-ttu-id="0bc83-125">Můžete taky nechat hello kombinace paměťově optimalizované tabulky s indexem columnstore.</span><span class="sxs-lookup"><span data-stu-id="0bc83-125">You can also have hello combination of a memory-optimized table with a columnstore index.</span></span> <span data-ttu-id="0bc83-126">Tato kombinace umožňuje velmi rychlé transakce tooperform zpracování a příliš*souběžně* spuštění analytics dotazuje velmi rychle na hello stejná data.</span><span class="sxs-lookup"><span data-stu-id="0bc83-126">This combination enables you tooperform very fast transaction processing, and too*concurrently* run analytics queries very quickly on hello same data.</span></span>

<span data-ttu-id="0bc83-127">Indexy columnstore a OLTP v paměti se součástí produktu SQL Server hello od 2012 a 2014, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0bc83-127">Both columnstore indexes and In-Memory OLTP have been part of hello SQL Server product since 2012 and 2014, respectively.</span></span> <span data-ttu-id="0bc83-128">Azure SQL Database a SQL Server hello sdílet stejné implementaci technologií v paměti.</span><span class="sxs-lookup"><span data-stu-id="0bc83-128">Azure SQL Database and SQL Server share hello same implementation of In-Memory technologies.</span></span> <span data-ttu-id="0bc83-129">Do budoucna, nové funkce pro tyto technologie jsou vydávány v Azure SQL Database nejprve před jejich vydání v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0bc83-129">Going forward, new capabilities for these technologies are released in Azure SQL Database first, before they are released in SQL Server.</span></span>

<span data-ttu-id="0bc83-130">Toto téma popisuje aspekty OLTP v paměti a columnstore indexy, které jsou specifické tooAzure databáze SQL a také zahrnuje ukázky:</span><span class="sxs-lookup"><span data-stu-id="0bc83-130">This topic describes aspects of In-Memory OLTP and columnstore indexes that are specific tooAzure SQL Database and also includes samples:</span></span>
- <span data-ttu-id="0bc83-131">Uvidíte hello dopad těchto technologií na omezení velikosti úložiště a data.</span><span class="sxs-lookup"><span data-stu-id="0bc83-131">You'll see hello impact of these technologies on storage and data size limits.</span></span>
- <span data-ttu-id="0bc83-132">Uvidíte, jak toomanage hello pohybů databází, které používají tyto technologie mezi hello jiných cenových úrovní.</span><span class="sxs-lookup"><span data-stu-id="0bc83-132">You'll see how toomanage hello movement of databases that use these technologies between hello different pricing tiers.</span></span>
- <span data-ttu-id="0bc83-133">Zobrazí se dvou vzorcích, které ilustrují použití hello OLTP v paměti, jakož i indexy columnstore ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="0bc83-133">You'll see two samples that illustrate hello use of In-Memory OLTP, as well as columnstore indexes in Azure SQL Database.</span></span>

<span data-ttu-id="0bc83-134">V tématu hello následující prostředky pro další informace.</span><span class="sxs-lookup"><span data-stu-id="0bc83-134">See hello following resources for more information.</span></span>

<span data-ttu-id="0bc83-135">Podrobné informace o technologiích hello:</span><span class="sxs-lookup"><span data-stu-id="0bc83-135">In-depth information about hello technologies:</span></span>

- <span data-ttu-id="0bc83-136">[Přehled OLTP v paměti a scénáře použití](https://msdn.microsoft.com/library/mt774593.aspx) (zahrnuje odkazy toocustomer případové studie a informace o tooget spuštění)</span><span class="sxs-lookup"><span data-stu-id="0bc83-136">[In-Memory OLTP Overview and Usage Scenarios](https://msdn.microsoft.com/library/mt774593.aspx) (includes references toocustomer case studies and information tooget started)</span></span>
- [<span data-ttu-id="0bc83-137">Dokumentace pro OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="0bc83-137">Documentation for In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
- [<span data-ttu-id="0bc83-138">Průvodce indexy Columnstore</span><span class="sxs-lookup"><span data-stu-id="0bc83-138">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
- <span data-ttu-id="0bc83-139">Hybridní transakcí nebo analytického zpracování (HTAP), také známé jako [provozní analýzu v reálném čase](https://msdn.microsoft.com/library/dn817827.aspx)</span><span class="sxs-lookup"><span data-stu-id="0bc83-139">Hybrid transactional/analytical processing (HTAP), also known as [real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)</span></span>

<span data-ttu-id="0bc83-140">Rychlý úvod do na OLTP v paměti: [rychlý Start 1: technologie OLTP v paměti pro rychlejší T-SQL výkonu](http://msdn.microsoft.com/library/mt694156.aspx) (jiný článek toohelp začnete)</span><span class="sxs-lookup"><span data-stu-id="0bc83-140">A quick primer on In-Memory OLTP: [Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance](http://msdn.microsoft.com/library/mt694156.aspx) (another article toohelp you get started)</span></span>

<span data-ttu-id="0bc83-141">Podrobný videa o technologiích hello:</span><span class="sxs-lookup"><span data-stu-id="0bc83-141">In-depth videos about hello technologies:</span></span>

- <span data-ttu-id="0bc83-142">[OLTP v paměti ve službě Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (která obsahuje ukázku výkonu výhody a kroky tooreproduce těchto výsledků sami)</span><span class="sxs-lookup"><span data-stu-id="0bc83-142">[In-Memory OLTP in Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (which contains a demo of performance benefits and steps tooreproduce these results yourself)</span></span>
- [<span data-ttu-id="0bc83-143">Videa OLTP v paměti: Co je a když/jak toouse ho</span><span class="sxs-lookup"><span data-stu-id="0bc83-143">In-Memory OLTP Videos: What it is and When/How toouse it</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [<span data-ttu-id="0bc83-144">Columnstore Index: Analýzy v paměti videa z webu Ignite 2016</span><span class="sxs-lookup"><span data-stu-id="0bc83-144">Columnstore Index: In-Memory Analytics Videos from Ignite 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a><span data-ttu-id="0bc83-145">Velikost úložiště a dat.</span><span class="sxs-lookup"><span data-stu-id="0bc83-145">Storage and data size</span></span>

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a><span data-ttu-id="0bc83-146">Limitu velikosti a úložiště dat pro OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="0bc83-146">Data size and storage cap for In-Memory OLTP</span></span>

<span data-ttu-id="0bc83-147">OLTP v paměti obsahuje paměťově optimalizované tabulky, které se používají k ukládání dat uživatele.</span><span class="sxs-lookup"><span data-stu-id="0bc83-147">In-Memory OLTP includes memory-optimized tables, which are used for storing user data.</span></span> <span data-ttu-id="0bc83-148">Tyhle tabulky jsou požadované toofit v paměti.</span><span class="sxs-lookup"><span data-stu-id="0bc83-148">These tables are required toofit in memory.</span></span> <span data-ttu-id="0bc83-149">Vzhledem k tomu, že budete spravovat přímo paměti hello služba SQL Database, máme hello konceptu kvótu pro data uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0bc83-149">Because you manage memory directly in hello SQL Database service, we have hello  concept of a quota for user data.</span></span> <span data-ttu-id="0bc83-150">Tento nápad je odkazované tooas *OLTP v paměti úložiště*.</span><span class="sxs-lookup"><span data-stu-id="0bc83-150">This idea is referred tooas *In-Memory OLTP storage*.</span></span>

<span data-ttu-id="0bc83-151">Každý podporovaný samostatná databáze cenová úroveň a každý elastického fondu cenová úroveň zahrnuje množství OLTP v paměti úložiště.</span><span class="sxs-lookup"><span data-stu-id="0bc83-151">Each supported standalone database pricing tier and each elastic pool pricing tier includes a certain amount of In-Memory OLTP storage.</span></span> <span data-ttu-id="0bc83-152">V době psaní textu hello zobrazí gigabajt úložiště pro každý 125 jednotky transakcí databáze (Dtu) nebo elastické databáze jednotky transakcí (Edtu).</span><span class="sxs-lookup"><span data-stu-id="0bc83-152">At hello time of writing, you get a gigabyte of storage for every 125 database transaction units (DTUs) or elastic database transaction units (eDTUs).</span></span>

<span data-ttu-id="0bc83-153">Hello [úrovně služeb SQL Database](sql-database-service-tiers.md) článek obsahuje seznam oficiální hello úložiště hello OLTP v paměti, která je k dispozici pro všechny podporované samostatná databáze a elastického fondu cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="0bc83-153">hello [SQL Database service tiers](sql-database-service-tiers.md) article has hello official list of hello In-Memory OLTP storage that is available for each supported standalone database and elastic pool pricing tier.</span></span>

<span data-ttu-id="0bc83-154">Hello následující počet položek směrem k vaší úložiště cap OLTP v paměti:</span><span class="sxs-lookup"><span data-stu-id="0bc83-154">hello following items count toward your In-Memory OLTP storage cap:</span></span>

- <span data-ttu-id="0bc83-155">Řádky dat aktivního uživatele v paměťově optimalizované tabulky a proměnné tabulek.</span><span class="sxs-lookup"><span data-stu-id="0bc83-155">Active user data rows in memory-optimized tables and table variables.</span></span> <span data-ttu-id="0bc83-156">Všimněte si, že nemáte staré verze řádku započítávat hello zakončení.</span><span class="sxs-lookup"><span data-stu-id="0bc83-156">Note that old row versions don't count toward hello cap.</span></span>
- <span data-ttu-id="0bc83-157">Indexy v paměťově optimalizovaných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="0bc83-157">Indexes on memory-optimized tables.</span></span>
- <span data-ttu-id="0bc83-158">Provozní režie operací ALTER TABLE.</span><span class="sxs-lookup"><span data-stu-id="0bc83-158">Operational overhead of ALTER TABLE operations.</span></span>

<span data-ttu-id="0bc83-159">Při dosažení limitu hello, obdržíte chybu na více systémů kvóty a již není možné data tooinsert nebo aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0bc83-159">If you hit hello cap, you receive an out-of-quota error, and you are no longer able tooinsert or update data.</span></span> <span data-ttu-id="0bc83-160">toomitigate odstranit data této chybě, nebo zvyšte hello cenová úroveň databáze hello nebo fond.</span><span class="sxs-lookup"><span data-stu-id="0bc83-160">toomitigate this error, delete data or increase hello pricing tier of hello database or pool.</span></span>

<span data-ttu-id="0bc83-161">Podrobnosti o monitorování využití úložiště OLTP v paměti a konfiguraci výstrahy, když téměř dosáhl limitu hello najdete v tématu [monitorování v paměti úložiště](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="0bc83-161">For details about monitoring In-Memory OLTP storage utilization and configuring alerts when you almost hit hello cap, see [Monitor In-Memory storage](sql-database-in-memory-oltp-monitoring.md).</span></span>

#### <a name="about-elastic-pools"></a><span data-ttu-id="0bc83-162">O elastické fondy</span><span class="sxs-lookup"><span data-stu-id="0bc83-162">About elastic pools</span></span>

<span data-ttu-id="0bc83-163">S elastické fondy hello OLTP v paměti úložiště sdílet všechny databáze ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-163">With elastic pools, hello In-Memory OLTP storage is shared across all databases in hello pool.</span></span> <span data-ttu-id="0bc83-164">Proto hello využití v jedné databáze může potenciálně ovlivnit jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="0bc83-164">Therefore, hello usage in one database can potentially affect other databases.</span></span> <span data-ttu-id="0bc83-165">Jsou dvě jejich zmírnění pro toto:</span><span class="sxs-lookup"><span data-stu-id="0bc83-165">Two mitigations for this are:</span></span>

- <span data-ttu-id="0bc83-166">Nakonfigurujte maximální-počet jednotek eDTU pro databáze, která je nižší než počet hello eDTU pro fond hello jako celek.</span><span class="sxs-lookup"><span data-stu-id="0bc83-166">Configure a Max-eDTU for databases that is lower than hello eDTU count for hello pool as a whole.</span></span> <span data-ttu-id="0bc83-167">Tento maximální caps hello využití úložiště OLTP v paměti, všechny databáze ve fondu hello, toohello velikost, která odpovídá počet eDTU toohello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-167">This maximum caps hello In-Memory OLTP storage utilization, in any database in hello pool, toohello size that corresponds toohello eDTU count.</span></span>
- <span data-ttu-id="0bc83-168">Nakonfigurujte Min eDTU, která je větší než 0.</span><span class="sxs-lookup"><span data-stu-id="0bc83-168">Configure a Min-eDTU that is greater than 0.</span></span> <span data-ttu-id="0bc83-169">Tato minimální záruky, zda má každý databáze ve fondu hello hello množství dostupného úložiště OLTP v paměti, která odpovídá toohello nakonfigurovat Min eDTU.</span><span class="sxs-lookup"><span data-stu-id="0bc83-169">This minimum guarantees that each database in hello pool has hello amount of available In-Memory OLTP storage that corresponds toohello configured Min-eDTU.</span></span>

### <a name="data-size-and-storage-for-columnstore-indexes"></a><span data-ttu-id="0bc83-170">Velikost dat a úložiště pro indexy columnstore</span><span class="sxs-lookup"><span data-stu-id="0bc83-170">Data size and storage for columnstore indexes</span></span>

<span data-ttu-id="0bc83-171">Indexy Columnstore nejsou požadované toofit v paměti.</span><span class="sxs-lookup"><span data-stu-id="0bc83-171">Columnstore indexes aren't required toofit in memory.</span></span> <span data-ttu-id="0bc83-172">Proto hello pouze zakončení na velikosti hello hello indexů je hello maximální celkovou velikost databáze, která je popsána v hello [úrovně služeb SQL Database](sql-database-service-tiers.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0bc83-172">Therefore, hello only cap on hello size of hello indexes is hello maximum overall database size, which is documented in hello [SQL Database service tiers](sql-database-service-tiers.md) article.</span></span>

<span data-ttu-id="0bc83-173">Při použití Clusterované indexy columnstore sloupcovém komprese se používá pro hello základní tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="0bc83-173">When you use clustered columnstore indexes, columnar compression is used for hello base table storage.</span></span> <span data-ttu-id="0bc83-174">Tato komprese může výrazně redukuje hello úložiště uživatelských dat, což znamená, že můžete začlenit další data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-174">This compression can significantly reduce hello storage footprint of your user data, which means that you can fit more data in hello database.</span></span> <span data-ttu-id="0bc83-175">A komprese hello může být zvýšena další s [sloupcovém archivace komprese](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span><span class="sxs-lookup"><span data-stu-id="0bc83-175">And hello compression can be further increased with [columnar archival compression](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span></span> <span data-ttu-id="0bc83-176">Hello množství komprese, můžete dosáhnout závisí na povaze hello hello dat, ale 10krát hello komprese není.</span><span class="sxs-lookup"><span data-stu-id="0bc83-176">hello amount of compression that you can achieve depends on hello nature of hello data, but 10 times hello compression is not uncommon.</span></span>

<span data-ttu-id="0bc83-177">Například pokud máte databázi s maximální velikostí 1 terabajt (TB) a dosáhnout 10krát hello komprese pomocí indexy columnstore, můžete začlenit celkem 10 TB dat uživatele v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-177">For example, if you have a database with a maximum size of 1 terabyte (TB) and you achieve 10 times hello compression by using columnstore indexes, you can fit a total of 10 TB of user data in hello database.</span></span>

<span data-ttu-id="0bc83-178">Při použití neclusterovaných indexů columnstore hello základní tabulka je pořád uložená ve formátu tradiční rowstore hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-178">When you use nonclustered columnstore indexes, hello base table is still stored in hello traditional rowstore format.</span></span> <span data-ttu-id="0bc83-179">Proto nejsou úspory úložiště hello stejnou velikost jako s Clusterované indexy columnstore.</span><span class="sxs-lookup"><span data-stu-id="0bc83-179">Therefore, hello storage savings aren't as big as with clustered columnstore indexes.</span></span> <span data-ttu-id="0bc83-180">Pokud chcete nahradit počet tradiční neclusterované indexy s indexem columnstore jeden, uvidíte stále celkové úspory v hello nároků úložiště pro tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-180">However, if you're replacing a number of traditional nonclustered indexes with a single columnstore index, you can still see an overall savings in hello storage footprint for hello table.</span></span>

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a><span data-ttu-id="0bc83-181">Přesunutí databází, které používají technologie v paměti mezi cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="0bc83-181">Moving databases that use In-Memory technologies between pricing tiers</span></span>

<span data-ttu-id="0bc83-182">Nejsou nikdy žádné nekompatibility nebo jiné problémy při upgradu tooa vyšší cenové úrovně, jako třeba ze standardní tooPremium.</span><span class="sxs-lookup"><span data-stu-id="0bc83-182">There are never any incompatibilities or other problems when you upgrade tooa higher pricing tier, such as from Standard tooPremium.</span></span> <span data-ttu-id="0bc83-183">k dispozici funkce Hello a prostředky pouze zvýšit.</span><span class="sxs-lookup"><span data-stu-id="0bc83-183">hello available functionality and resources only increase.</span></span>

<span data-ttu-id="0bc83-184">Ale přechod na starší verzi hello cenová úroveň může mít negativní vliv na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="0bc83-184">But downgrading hello pricing tier can negatively impact your database.</span></span> <span data-ttu-id="0bc83-185">dopad Hello je obzvláště zřejmá při downgradovat z Premium tooStandard nebo Basic, když databáze obsahuje objekty OLTP v paměti.</span><span class="sxs-lookup"><span data-stu-id="0bc83-185">hello impact is especially apparent when you downgrade from Premium tooStandard or Basic when your database contains In-Memory OLTP objects.</span></span> <span data-ttu-id="0bc83-186">Paměťově optimalizované tabulky a indexy columnstore jsou k dispozici po hello přechod na starší verzi (i v případě, že zůstanou viditelné).</span><span class="sxs-lookup"><span data-stu-id="0bc83-186">Memory-optimized tables, and columnstore indexes, are unavailable after hello downgrade (even if they remain visible).</span></span> <span data-ttu-id="0bc83-187">Dobrý den, platí stejné aspekty při jste snížení hello cenová úroveň fondu elastické databáze nebo přesunutí databáze s technologiemi v paměti do Standard a Basic elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="0bc83-187">hello same considerations apply when you're lowering hello pricing tier of an elastic pool, or moving a database with In-Memory technologies, into a Standard or Basic elastic pool.</span></span>

### <a name="in-memory-oltp"></a><span data-ttu-id="0bc83-188">OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="0bc83-188">In-Memory OLTP</span></span>

<span data-ttu-id="0bc83-189">*Přechod na starší verzi tooBasic a standardní*: OLTP v paměti není podporována v databázích v hello úroveň Standard nebo Basic.</span><span class="sxs-lookup"><span data-stu-id="0bc83-189">*Downgrading tooBasic/Standard*: In-Memory OLTP isn't supported in databases in hello Standard or Basic tier.</span></span> <span data-ttu-id="0bc83-190">Kromě toho není možné toomove databáze, která obsahuje všechny OLTP v paměti objekty toohello úroveň Standard nebo Basic.</span><span class="sxs-lookup"><span data-stu-id="0bc83-190">In addition, it isn't possible toomove a database that has any In-Memory OLTP objects toohello Standard or Basic tier.</span></span>

<span data-ttu-id="0bc83-191">Předtím, než jste starší verzi databáze hello tooStandard a základní, odeberte všechny paměťově optimalizované tabulky a typů tabulek, jakož i všechny nativně Kompilované moduly T-SQL.</span><span class="sxs-lookup"><span data-stu-id="0bc83-191">Before you downgrade hello database tooStandard/Basic, remove all memory-optimized tables and table types, as well as all natively compiled T-SQL modules.</span></span>

<span data-ttu-id="0bc83-192">Není toounderstand programový způsob, jestli na danou databázi podporuje OLTP v paměti.</span><span class="sxs-lookup"><span data-stu-id="0bc83-192">There is a programmatic way toounderstand whether a given database supports In-Memory OLTP.</span></span> <span data-ttu-id="0bc83-193">Můžete provést hello následující jazyka Transact-SQL:</span><span class="sxs-lookup"><span data-stu-id="0bc83-193">You can execute hello following Transact-SQL query:</span></span>

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

<span data-ttu-id="0bc83-194">Pokud hello dotaz vrátí **1**, OLTP v paměti je podporováno v této databázi.</span><span class="sxs-lookup"><span data-stu-id="0bc83-194">If hello query returns **1**, In-Memory OLTP is supported in this database.</span></span>


<span data-ttu-id="0bc83-195">*Přechod na starší verzi nižší úroveň Premium tooa*: Data v paměťově optimalizovaných tabulkách musí být přizpůsobena úložiště hello OLTP v paměti, které souvisí s hello cenová úroveň databáze hello nebo není k dispozici hello elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="0bc83-195">*Downgrading tooa lower Premium tier*: Data in memory-optimized tables must fit within hello In-Memory OLTP storage that is associated with hello pricing tier of hello database or is available in hello elastic pool.</span></span> <span data-ttu-id="0bc83-196">Pokud zkuste toolower hello cenová úroveň nebo přesunutí hello databáze ve fondu, který nemá dostatek dostupného úložiště OLTP v paměti, hello operace selže.</span><span class="sxs-lookup"><span data-stu-id="0bc83-196">If you try toolower hello pricing tier or move hello database into a pool that doesn't have enough available In-Memory OLTP storage, hello operation fails.</span></span>

### <a name="columnstore-indexes"></a><span data-ttu-id="0bc83-197">Indexy Columnstore</span><span class="sxs-lookup"><span data-stu-id="0bc83-197">Columnstore indexes</span></span>

<span data-ttu-id="0bc83-198">*Přechod na starší verzi Standard nebo tooBasic*: Columnstore indexy jsou podporovány pouze na cenová úroveň Premium hello a ne v hello Standard nebo Basic vrstev.</span><span class="sxs-lookup"><span data-stu-id="0bc83-198">*Downgrading tooBasic or Standard*: Columnstore indexes are supported only on hello Premium pricing tier, and not on hello Standard or Basic tiers.</span></span> <span data-ttu-id="0bc83-199">Když jste starší verzi databáze tooStandard nebo Basic, stane se indexu columnstore není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0bc83-199">When you downgrade your database tooStandard or Basic, your columnstore index becomes unavailable.</span></span> <span data-ttu-id="0bc83-200">Hello systém uchovává indexu columnstore, ale nikdy využívá hello index.</span><span class="sxs-lookup"><span data-stu-id="0bc83-200">hello system maintains your columnstore index, but it never leverages hello index.</span></span> <span data-ttu-id="0bc83-201">Pokud později upgradujete tooPremium zpět, je indexu columnstore okamžitě připraven toobe využít znovu.</span><span class="sxs-lookup"><span data-stu-id="0bc83-201">If you later upgrade back tooPremium, your columnstore index is immediately ready toobe leveraged again.</span></span>

<span data-ttu-id="0bc83-202">Pokud máte **clusterové** columnstore index, celé tabulky hello nedostupný po vrstvy přechod na starší verzi.</span><span class="sxs-lookup"><span data-stu-id="0bc83-202">If you have a **clustered** columnstore index, hello whole table becomes unavailable after tier downgrade.</span></span> <span data-ttu-id="0bc83-203">Proto doporučujeme vyřaďte všechny *clusterové* indexy columnstore před downgradovat databáze nižší než úroveň Premium hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-203">Therefore we recommend that you drop all *clustered* columnstore indexes before you downgrade your database below hello Premium tier.</span></span>

<span data-ttu-id="0bc83-204">*Přechod na starší verzi nižší úroveň Premium tooa*: Tento přechod na starší verzi úspěšná, pokud odpovídá hello celé databáze v rámci hello maximální velikost databáze pro cíl hello cenovou úroveň, nebo dostupné úložiště hello v hello elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="0bc83-204">*Downgrading tooa lower Premium tier*: This downgrade succeeds if hello whole database fits within hello maximum database size for hello target pricing tier, or within hello available storage in hello elastic pool.</span></span> <span data-ttu-id="0bc83-205">Neexistuje žádná konkrétní vlivu indexy columnstore hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-205">There is no specific impact from hello columnstore indexes.</span></span>


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-hello-in-memory-oltp-sample"></a><span data-ttu-id="0bc83-206">1. Instalace ukázkové hello OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="0bc83-206">1. Install hello In-Memory OLTP sample</span></span>

<span data-ttu-id="0bc83-207">Ukázkové databáze AdventureWorksLT hello můžete vytvořit pomocí několika kliknutí v hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0bc83-207">You can create hello AdventureWorksLT sample database with a few clicks in hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="0bc83-208">Potom hello kroky v této části popisují, jak můžete rozšířit vaše databáze AdventureWorksLT s objekty OLTP v paměti a předvedení výkonnostních výhod.</span><span class="sxs-lookup"><span data-stu-id="0bc83-208">Then, hello steps in this section explain how you can enrich your AdventureWorksLT database with In-Memory OLTP objects and demonstrate performance benefits.</span></span>

<span data-ttu-id="0bc83-209">Více zneužívající vlastností prohlížeče, ale vizuálně výkonu ukázku pro OLTP v paměti najdete v části:</span><span class="sxs-lookup"><span data-stu-id="0bc83-209">For a more simplistic, but more visually appealing performance demo for In-Memory OLTP, see:</span></span>

- <span data-ttu-id="0bc83-210">Verze: [v – paměť oltp-demo-verze 1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span><span class="sxs-lookup"><span data-stu-id="0bc83-210">Release: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span></span>
- <span data-ttu-id="0bc83-211">Zdrojový kód: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span><span class="sxs-lookup"><span data-stu-id="0bc83-211">Source code: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span></span>

#### <a name="installation-steps"></a><span data-ttu-id="0bc83-212">Postup instalace</span><span class="sxs-lookup"><span data-stu-id="0bc83-212">Installation steps</span></span>

1. <span data-ttu-id="0bc83-213">V hello [portál Azure](https://portal.azure.com/), vytvořte na serveru databáze Premium.</span><span class="sxs-lookup"><span data-stu-id="0bc83-213">In hello [Azure portal](https://portal.azure.com/), create a Premium database on a server.</span></span> <span data-ttu-id="0bc83-214">Sada hello **zdroj** ukázkové databáze AdventureWorksLT toohello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-214">Set hello **Source** toohello AdventureWorksLT sample database.</span></span> <span data-ttu-id="0bc83-215">Podrobné pokyny najdete v tématu [vytvořit svoji první databázi Azure SQL](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0bc83-215">For detailed instructions, see [Create your first Azure SQL database](sql-database-get-started-portal.md).</span></span>

2. <span data-ttu-id="0bc83-216">Připojit databáze toohello s SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="0bc83-216">Connect toohello database with SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

3. <span data-ttu-id="0bc83-217">Kopírování hello [OLTP v paměti jazyka Transact-SQL skriptu](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour schránky.</span><span class="sxs-lookup"><span data-stu-id="0bc83-217">Copy hello [In-Memory OLTP Transact-SQL script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour clipboard.</span></span> <span data-ttu-id="0bc83-218">Hello skriptu T-SQL vytvoří hello objekty nezbytné v paměti v ukázkové databáze AdventureWorksLT hello, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="0bc83-218">hello T-SQL script creates hello necessary In-Memory objects in hello AdventureWorksLT sample database that you created in step 1.</span></span>

4. <span data-ttu-id="0bc83-219">Vložit do aplikace SSMS hello skriptu T-SQL a pak spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-219">Paste hello T-SQL script into SSMS, and then execute hello script.</span></span> <span data-ttu-id="0bc83-220">Hello `MEMORY_OPTIMIZED = ON` příkazy CREATE TABLE klauzule jsou zásadní.</span><span class="sxs-lookup"><span data-stu-id="0bc83-220">hello `MEMORY_OPTIMIZED = ON` clause CREATE TABLE statements are crucial.</span></span> <span data-ttu-id="0bc83-221">Například:</span><span class="sxs-lookup"><span data-stu-id="0bc83-221">For example:</span></span>


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a><span data-ttu-id="0bc83-222">Chyba 40536</span><span class="sxs-lookup"><span data-stu-id="0bc83-222">Error 40536</span></span>


<span data-ttu-id="0bc83-223">Pokud se zobrazí chyba 40536 při spuštění skriptu hello T-SQL, spusťte hello následující tooverify skriptu T-SQL, zda text hello databáze podporuje v paměti:</span><span class="sxs-lookup"><span data-stu-id="0bc83-223">If you get error 40536 when you run hello T-SQL script, run hello following T-SQL script tooverify whether hello database supports In-Memory:</span></span>


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


<span data-ttu-id="0bc83-224">Důsledkem **0** znamená, že v paměti není podporován, a **1** znamená, že je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="0bc83-224">A result of **0** means that In-Memory isn't supported, and **1** means that it is supported.</span></span> <span data-ttu-id="0bc83-225">toodiagnose hello problém, ujistěte se, že hello je databáze na úroveň služeb Premium hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-225">toodiagnose hello problem, ensure that hello database is at hello Premium service tier.</span></span>


#### <a name="about-hello-created-memory-optimized-items"></a><span data-ttu-id="0bc83-226">O hello vytvořili paměťově optimalizované položky</span><span class="sxs-lookup"><span data-stu-id="0bc83-226">About hello created memory-optimized items</span></span>

<span data-ttu-id="0bc83-227">**Tabulky**: Ukázka hello obsahuje hello následující paměťově optimalizované tabulky:</span><span class="sxs-lookup"><span data-stu-id="0bc83-227">**Tables**: hello sample contains hello following memory-optimized tables:</span></span>

- <span data-ttu-id="0bc83-228">SalesLT.Product_inmem</span><span class="sxs-lookup"><span data-stu-id="0bc83-228">SalesLT.Product_inmem</span></span>
- <span data-ttu-id="0bc83-229">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="0bc83-229">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="0bc83-230">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="0bc83-230">SalesLT.SalesOrderDetail_inmem</span></span>
- <span data-ttu-id="0bc83-231">Demo.DemoSalesOrderHeaderSeed</span><span class="sxs-lookup"><span data-stu-id="0bc83-231">Demo.DemoSalesOrderHeaderSeed</span></span>
- <span data-ttu-id="0bc83-232">Demo.DemoSalesOrderDetailSeed</span><span class="sxs-lookup"><span data-stu-id="0bc83-232">Demo.DemoSalesOrderDetailSeed</span></span>


<span data-ttu-id="0bc83-233">Si můžete prohlédnout paměťově optimalizované tabulky prostřednictvím hello **Průzkumník objektů** v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="0bc83-233">You can inspect memory-optimized tables through hello **Object Explorer** in SSMS.</span></span> <span data-ttu-id="0bc83-234">Klikněte pravým tlačítkem na **tabulky** > **filtru** > **nastavení filtru** > **je paměťově optimalizovaná**.</span><span class="sxs-lookup"><span data-stu-id="0bc83-234">Right-click **Tables** > **Filter** > **Filter Settings** > **Is Memory Optimized**.</span></span> <span data-ttu-id="0bc83-235">Hello hodnota se rovná 1.</span><span class="sxs-lookup"><span data-stu-id="0bc83-235">hello value equals 1.</span></span>


<span data-ttu-id="0bc83-236">Nebo můžete dát dotaz na zobrazení katalogu hello, jako například:</span><span class="sxs-lookup"><span data-stu-id="0bc83-236">Or you can query hello catalog views, such as:</span></span>


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


<span data-ttu-id="0bc83-237">**Nativně kompilované uložené procedury**: SalesLT.usp_InsertSalesOrder_inmem si můžete prohlédnout pomocí zobrazení katalogu dotazu:</span><span class="sxs-lookup"><span data-stu-id="0bc83-237">**Natively compiled stored procedure**: You can inspect SalesLT.usp_InsertSalesOrder_inmem through a catalog view query:</span></span>


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-hello-sample-oltp-workload"></a><span data-ttu-id="0bc83-238">Spustit hello ukázky OLTP pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="0bc83-238">Run hello sample OLTP workload</span></span>

<span data-ttu-id="0bc83-239">Hello jenom rozdíl mezi hello následující dva *uložené procedury* je první postup hello používá verzích paměťově optimalizované tabulky hello při hello druhý postup používá hello regulární tabulky na disku:</span><span class="sxs-lookup"><span data-stu-id="0bc83-239">hello only difference between hello following two *stored procedures* is that hello first procedure uses memory-optimized versions of hello tables, while hello second procedure uses hello regular on-disk tables:</span></span>

- <span data-ttu-id="0bc83-240">SalesLT**.** usp_InsertSalesOrder**_inmem**</span><span class="sxs-lookup"><span data-stu-id="0bc83-240">SalesLT**.**usp_InsertSalesOrder**_inmem**</span></span>
- <span data-ttu-id="0bc83-241">SalesLT**.** usp_InsertSalesOrder**_ondisk**</span><span class="sxs-lookup"><span data-stu-id="0bc83-241">SalesLT**.**usp_InsertSalesOrder**_ondisk**</span></span>


<span data-ttu-id="0bc83-242">V této části, uvidíte, jak hello užitečný toouse **ostress.exe** tooexecute nástroj hello dvě uložené procedury na stressful úrovních.</span><span class="sxs-lookup"><span data-stu-id="0bc83-242">In this section, you see how toouse hello handy **ostress.exe** utility tooexecute hello two stored procedures at stressful levels.</span></span> <span data-ttu-id="0bc83-243">Jak dlouho trvá pro hello dva přízvuk spustí toofinish, můžete porovnat.</span><span class="sxs-lookup"><span data-stu-id="0bc83-243">You can compare how long it takes for hello two stress runs toofinish.</span></span>


<span data-ttu-id="0bc83-244">Když spustíte ostress.exe, doporučujeme předat hodnoty parametrů, které jsou určené pro obě hello následující:</span><span class="sxs-lookup"><span data-stu-id="0bc83-244">When you run ostress.exe, we recommend that you pass parameter values designed for both of hello following:</span></span>

- <span data-ttu-id="0bc83-245">Spustit velký počet souběžných připojení pomocí - n100.</span><span class="sxs-lookup"><span data-stu-id="0bc83-245">Run a large number of concurrent connections, by using -n100.</span></span>
- <span data-ttu-id="0bc83-246">Pomocí mít každý připojení smyčku stovky dobu, pomocí - r500.</span><span class="sxs-lookup"><span data-stu-id="0bc83-246">Have each connection loop hundreds of times, by using -r500.</span></span>


<span data-ttu-id="0bc83-247">Můžete ale chtít toostart s mnohem menšími hodnoty jako - n10 a - r 50 tooensure, který vše funguje.</span><span class="sxs-lookup"><span data-stu-id="0bc83-247">However, you might want toostart with much smaller values like -n10 and -r50 tooensure that everything is working.</span></span>


### <a name="script-for-ostressexe"></a><span data-ttu-id="0bc83-248">Skript pro ostress.exe</span><span class="sxs-lookup"><span data-stu-id="0bc83-248">Script for ostress.exe</span></span>


<span data-ttu-id="0bc83-249">V této části zobrazí skript hello T-SQL, který je vložen v našem ostress.exe příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0bc83-249">This section displays hello T-SQL script that is embedded in our ostress.exe command line.</span></span> <span data-ttu-id="0bc83-250">skript Hello používá položky, které byly vytvořeny hello skriptu T-SQL, který jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="0bc83-250">hello script uses items that were created by hello T-SQL script that you installed earlier.</span></span>


<span data-ttu-id="0bc83-251">Hello následující skript vloží ukázka prodejní objednávky s pěti položek řádku do následující hello paměťově optimalizované *tabulky*:</span><span class="sxs-lookup"><span data-stu-id="0bc83-251">hello following script inserts a sample sales order with five line items into hello following memory-optimized *tables*:</span></span>

- <span data-ttu-id="0bc83-252">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="0bc83-252">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="0bc83-253">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="0bc83-253">SalesLT.SalesOrderDetail_inmem</span></span>


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


<span data-ttu-id="0bc83-254">toomake hello *_ondisk* verzi hello předchozí skriptu T-SQL pro ostress.exe, měli byste nahradit oba výskyty hello *_inmem* substring s *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="0bc83-254">toomake hello *_ondisk* version of hello preceding T-SQL script for ostress.exe, you would replace both occurrences of hello *_inmem* substring with *_ondisk*.</span></span> <span data-ttu-id="0bc83-255">Tyto náhrady ovlivnit hello názvy tabulek a uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="0bc83-255">These replacements affect hello names of tables and stored procedures.</span></span>


### <a name="install-rml-utilities-and-ostress"></a><span data-ttu-id="0bc83-256">Instalace nástrojů RML a ostress</span><span class="sxs-lookup"><span data-stu-id="0bc83-256">Install RML utilities and ostress</span></span>


<span data-ttu-id="0bc83-257">V ideálním případě by plánování toorun ostress.exe na virtuální počítač Azure (VM).</span><span class="sxs-lookup"><span data-stu-id="0bc83-257">Ideally, you would plan toorun ostress.exe on an Azure virtual machine (VM).</span></span> <span data-ttu-id="0bc83-258">Měli byste vytvořit [virtuálního počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) v hello stejné Azure geografické oblasti, kde se nachází databáze AdventureWorksLT.</span><span class="sxs-lookup"><span data-stu-id="0bc83-258">You would create an [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) in hello same Azure geographic region where your AdventureWorksLT database resides.</span></span> <span data-ttu-id="0bc83-259">Ale ostress.exe na svém přenosném počítači může spouštět místo.</span><span class="sxs-lookup"><span data-stu-id="0bc83-259">But you can run ostress.exe on your laptop instead.</span></span>


<span data-ttu-id="0bc83-260">Na hello virtuálního počítače nebo na ať hostitele, můžete zvolit, nainstalujte nástroje hello opětovného přehrání Markup Language (RML).</span><span class="sxs-lookup"><span data-stu-id="0bc83-260">On hello VM, or on whatever host you choose, install hello Replay Markup Language (RML) utilities.</span></span> <span data-ttu-id="0bc83-261">Hello nástroje zahrnují ostress.exe.</span><span class="sxs-lookup"><span data-stu-id="0bc83-261">hello utilities include ostress.exe.</span></span>

<span data-ttu-id="0bc83-262">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="0bc83-262">For more information, see:</span></span>
- <span data-ttu-id="0bc83-263">Hello ostress.exe diskuse ve [ukázkové databáze pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="0bc83-263">hello ostress.exe discussion in [Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="0bc83-264">[Ukázkové databáze pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="0bc83-264">[Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="0bc83-265">Hello [blogu pro instalaci ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span><span class="sxs-lookup"><span data-stu-id="0bc83-265">hello [blog for installing ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span></span>



<!--
dn511655.aspx is for SQL 2014,
[Extensions tooAdventureWorks tooDemonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-hello-inmem-stress-workload-first"></a><span data-ttu-id="0bc83-266">Spustit hello *_inmem* nejprve vystavila zátěži pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="0bc83-266">Run hello *_inmem* stress workload first</span></span>


<span data-ttu-id="0bc83-267">Můžete použít *RML Cmd výzva* okno toorun naše ostress.exe příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0bc83-267">You can use an *RML Cmd Prompt* window toorun our ostress.exe command line.</span></span> <span data-ttu-id="0bc83-268">Parametry příkazového řádku Hello přímé ostress na:</span><span class="sxs-lookup"><span data-stu-id="0bc83-268">hello command-line parameters direct ostress to:</span></span>

- <span data-ttu-id="0bc83-269">Připojení 100 běželo (-n100).</span><span class="sxs-lookup"><span data-stu-id="0bc83-269">Run 100 connections concurrently (-n100).</span></span>
- <span data-ttu-id="0bc83-270">Každé připojení 50 času spuštění skriptu T-SQL hello (-r 50).</span><span class="sxs-lookup"><span data-stu-id="0bc83-270">Have each connection run hello T-SQL script 50 times (-r50).</span></span>


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


<span data-ttu-id="0bc83-271">hello toorun předcházející ostress.exe příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="0bc83-271">toorun hello preceding ostress.exe command line:</span></span>


1. <span data-ttu-id="0bc83-272">Resetování hello databáze data obsah tak, že spustíte následující příkaz v aplikaci SSMS, toodelete hello všechna hello data, která byla vložená všech předchozích spuštění:</span><span class="sxs-lookup"><span data-stu-id="0bc83-272">Reset hello database data content by running hello following command in SSMS, toodelete all hello data that was inserted by any previous runs:</span></span>

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. <span data-ttu-id="0bc83-273">Zkopírujte text hello hello předcházející schránky tooyour ostress.exe příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0bc83-273">Copy hello text of hello preceding ostress.exe command line tooyour clipboard.</span></span>

3. <span data-ttu-id="0bc83-274">Nahraďte hello `<placeholders>` pro hello parametry -S - U -P -d s hello opravte skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0bc83-274">Replace hello `<placeholders>` for hello parameters -S -U -P -d with hello correct real values.</span></span>

4. <span data-ttu-id="0bc83-275">V okně RML Cmd spusťte do upravená příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0bc83-275">Run your edited command line in an RML Cmd window.</span></span>


#### <a name="result-is-a-duration"></a><span data-ttu-id="0bc83-276">Výsledkem je, doba trvání</span><span class="sxs-lookup"><span data-stu-id="0bc83-276">Result is a duration</span></span>


<span data-ttu-id="0bc83-277">Po dokončení ostress.exe zapíše hello spustit trvání jako jeho poslední řádek výstupu v okně RML Cmd hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-277">When ostress.exe finishes, it writes hello run duration as its final line of output in hello RML Cmd window.</span></span> <span data-ttu-id="0bc83-278">Například kratší testovací běh už bylo asi 1,5 minuty:</span><span class="sxs-lookup"><span data-stu-id="0bc83-278">For example, a shorter test run lasted about 1.5 minutes:</span></span>

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a><span data-ttu-id="0bc83-279">Resetovat, upravit pro *_ondisk*, pak znovu spusťte</span><span class="sxs-lookup"><span data-stu-id="0bc83-279">Reset, edit for *_ondisk*, then rerun</span></span>


<span data-ttu-id="0bc83-280">Až budete mít hello výsledek z hello *_inmem* spustit, proveďte následující kroky pro hello hello *_ondisk* spustit:</span><span class="sxs-lookup"><span data-stu-id="0bc83-280">After you have hello result from hello *_inmem* run, perform hello following steps for hello *_ondisk* run:</span></span>


1. <span data-ttu-id="0bc83-281">Obnovení databáze hello tak, že spustíte následující příkaz v aplikaci SSMS toodelete hello všechna hello data, která byla vložená hello předchozí spustit:</span><span class="sxs-lookup"><span data-stu-id="0bc83-281">Reset hello database by running hello following command in SSMS toodelete all hello data that was inserted by hello previous run:</span></span>
```
EXECUTE Demo.usp_DemoReset;
```

2. <span data-ttu-id="0bc83-282">Upravit hello ostress.exe příkazového řádku tooreplace všechny *_inmem* s *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="0bc83-282">Edit hello ostress.exe command line tooreplace all *_inmem* with *_ondisk*.</span></span>

3. <span data-ttu-id="0bc83-283">Znovu spustit ostress.exe pro hello ještě jednou a zachycení hello trvání výsledek.</span><span class="sxs-lookup"><span data-stu-id="0bc83-283">Rerun ostress.exe for hello second time, and capture hello duration result.</span></span>

4. <span data-ttu-id="0bc83-284">Znovu obnovte databáze hello (pro odstranění jeho zodpovědné, může být velký objem dat test).</span><span class="sxs-lookup"><span data-stu-id="0bc83-284">Again, reset hello database (for responsibly deleting what can be a large amount of test data).</span></span>


#### <a name="expected-comparison-results"></a><span data-ttu-id="0bc83-285">Očekávaný porovnání výsledků</span><span class="sxs-lookup"><span data-stu-id="0bc83-285">Expected comparison results</span></span>

<span data-ttu-id="0bc83-286">Naše testy v paměti ukázaly, že výkonu vylepšené podle **devětkrát** pro tuto úlohu zneužívající vlastností prohlížeče s ostress systémem virtuálního počítače Azure v hello stejné oblasti Azure jako hello databáze.</span><span class="sxs-lookup"><span data-stu-id="0bc83-286">Our In-Memory tests have shown that performance improved by **nine times** for this simplistic workload, with ostress running on an Azure VM in hello same Azure region as hello database.</span></span>

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-hello-in-memory-analytics-sample"></a><span data-ttu-id="0bc83-287">2. Instalace ukázkové hello analýzy v paměti</span><span class="sxs-lookup"><span data-stu-id="0bc83-287">2. Install hello In-Memory Analytics sample</span></span>


<span data-ttu-id="0bc83-288">V této části můžete porovnat hello vstupně-výstupní operace a statistiky výsledky při použití index columnstore a index tradiční b stromu.</span><span class="sxs-lookup"><span data-stu-id="0bc83-288">In this section, you compare hello IO and statistics results when you're using a columnstore index versus a traditional b-tree index.</span></span>


<span data-ttu-id="0bc83-289">Pro analýzu v reálném čase na úloh s online zpracováním je často nejlepším toouse neclusterovaný index columnstore.</span><span class="sxs-lookup"><span data-stu-id="0bc83-289">For real-time analytics on an OLTP workload, it's often best toouse a nonclustered columnstore index.</span></span> <span data-ttu-id="0bc83-290">Podrobnosti najdete v tématu [popsané indexy Columnstore](http://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="0bc83-290">For details, see [Columnstore Indexes Described](http://msdn.microsoft.com/library/gg492088.aspx).</span></span>



### <a name="prepare-hello-columnstore-analytics-test"></a><span data-ttu-id="0bc83-291">Příprava hello columnstore analytics testu</span><span class="sxs-lookup"><span data-stu-id="0bc83-291">Prepare hello columnstore analytics test</span></span>


1. <span data-ttu-id="0bc83-292">Pomocí portálu Azure toocreate čerstvé databáze AdventureWorksLT z ukázkové hello hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-292">Use hello Azure portal toocreate a fresh AdventureWorksLT database from hello sample.</span></span>
 - <span data-ttu-id="0bc83-293">Použijte tento přesný název.</span><span class="sxs-lookup"><span data-stu-id="0bc83-293">Use that exact name.</span></span>
 - <span data-ttu-id="0bc83-294">Vyberte všechny úroveň služeb Premium.</span><span class="sxs-lookup"><span data-stu-id="0bc83-294">Choose any Premium service tier.</span></span>

2. <span data-ttu-id="0bc83-295">Kopírování hello [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour schránky.</span><span class="sxs-lookup"><span data-stu-id="0bc83-295">Copy hello [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour clipboard.</span></span>
 - <span data-ttu-id="0bc83-296">Hello skriptu T-SQL vytvoří hello objekty nezbytné v paměti v ukázkové databáze AdventureWorksLT hello, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="0bc83-296">hello T-SQL script creates hello necessary In-Memory objects in hello AdventureWorksLT sample database that you created in step 1.</span></span>
 - <span data-ttu-id="0bc83-297">skript Hello vytvoří hello tabulce dimenzí a dvě tabulky faktů.</span><span class="sxs-lookup"><span data-stu-id="0bc83-297">hello script creates hello Dimension table and two fact tables.</span></span> <span data-ttu-id="0bc83-298">tabulky faktů Hello se naplní 3.5 milionu řádků.</span><span class="sxs-lookup"><span data-stu-id="0bc83-298">hello fact tables are populated with 3.5 million rows each.</span></span>
 - <span data-ttu-id="0bc83-299">skript Hello může trvat 15 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="0bc83-299">hello script might take 15 minutes toocomplete.</span></span>

3. <span data-ttu-id="0bc83-300">Vložit do aplikace SSMS hello skriptu T-SQL a pak spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-300">Paste hello T-SQL script into SSMS, and then execute hello script.</span></span> <span data-ttu-id="0bc83-301">Hello **COLUMNSTORE** – klíčové slovo v hello **CREATE INDEX** údajů je velmi důležitý, stejně jako na:</span><span class="sxs-lookup"><span data-stu-id="0bc83-301">hello **COLUMNSTORE** keyword in hello **CREATE INDEX** statement is crucial, as in:</span></span><br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. <span data-ttu-id="0bc83-302">Nastavení úrovně toocompatibility AdventureWorksLT 130:</span><span class="sxs-lookup"><span data-stu-id="0bc83-302">Set AdventureWorksLT toocompatibility level 130:</span></span><br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    <span data-ttu-id="0bc83-303">Úroveň 130 není funkce přímo související tooIn paměti.</span><span class="sxs-lookup"><span data-stu-id="0bc83-303">Level 130 is not directly related tooIn-Memory features.</span></span> <span data-ttu-id="0bc83-304">Ale úroveň 130 obecně poskytuje vyšší výkon dotazu než 120.</span><span class="sxs-lookup"><span data-stu-id="0bc83-304">But level 130 generally provides faster query performance than 120.</span></span>


#### <a name="key-tables-and-columnstore-indexes"></a><span data-ttu-id="0bc83-305">Klíče tabulky a indexy columnstore</span><span class="sxs-lookup"><span data-stu-id="0bc83-305">Key tables and columnstore indexes</span></span>


- <span data-ttu-id="0bc83-306">dbo. Tabulka, která má clusterovaný index columnstore, která obsahuje rozšířené komprese na hello je FactResellerSalesXL_CCI *data* úroveň.</span><span class="sxs-lookup"><span data-stu-id="0bc83-306">dbo.FactResellerSalesXL_CCI is a table that has a clustered columnstore index, which has advanced compression at hello *data* level.</span></span>

- <span data-ttu-id="0bc83-307">dbo. Tabulka, která má ekvivalentní regulární clusterovaný index, který se komprimují jenom na hello je FactResellerSalesXL_PageCompressed *stránky* úroveň.</span><span class="sxs-lookup"><span data-stu-id="0bc83-307">dbo.FactResellerSalesXL_PageCompressed is a table that has an equivalent regular clustered index, which is compressed only at hello *page* level.</span></span>


#### <a name="key-queries-toocompare-hello-columnstore-index"></a><span data-ttu-id="0bc83-308">Index columnstore hello toocompare klíče dotazů</span><span class="sxs-lookup"><span data-stu-id="0bc83-308">Key queries toocompare hello columnstore index</span></span>


<span data-ttu-id="0bc83-309">Existují [několik typů dotazu T-SQL, které můžete spustit](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee vylepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="0bc83-309">There are [several T-SQL query types that you can run](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee performance improvements.</span></span> <span data-ttu-id="0bc83-310">V kroku 2 hello skriptu T-SQL věnujte pozornost toothis pár dotazů.</span><span class="sxs-lookup"><span data-stu-id="0bc83-310">In step 2 in hello T-SQL script, pay attention toothis pair of queries.</span></span> <span data-ttu-id="0bc83-311">Liší se pouze na jednom řádku:</span><span class="sxs-lookup"><span data-stu-id="0bc83-311">They differ only on one line:</span></span>


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


<span data-ttu-id="0bc83-312">Clusterovaný index columnstore je v hello FactResellerSalesXL\_KÚS tabulky.</span><span class="sxs-lookup"><span data-stu-id="0bc83-312">A clustered columnstore index is in hello FactResellerSalesXL\_CCI table.</span></span>

<span data-ttu-id="0bc83-313">Hello následující výňatek ze skriptu T-SQL vytiskne statistiky pro vstupně-výstupní operace a čas pro dotaz hello každé tabulky.</span><span class="sxs-lookup"><span data-stu-id="0bc83-313">hello following T-SQL script excerpt prints statistics for IO and TIME for hello query of each table.</span></span>


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order toosee Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins hello Fact Table with dimension tables
-- Note this query will run on hello Page Compressed table, Note down hello time
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


-- This is hello same Prior query on a table with a clustered columnstore index CCI
-- hello comparison numbers are even more dramatic hello larger hello table is (this is an 11 million row table only)
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

<span data-ttu-id="0bc83-314">V databázi s hello P2 cenovou úroveň bude pravděpodobně přibližně devětkrát hello výkonnější pro tento dotaz pomocí hello clusterovaný index columnstore v porovnání s tradiční index hello.</span><span class="sxs-lookup"><span data-stu-id="0bc83-314">In a database with hello P2 pricing tier, you can expect about nine times hello performance gain for this query by using hello clustered columnstore index compared with hello traditional index.</span></span> <span data-ttu-id="0bc83-315">S P15 můžete očekávat o 57 časy hello výkonnější pomocí hello columnstore index.</span><span class="sxs-lookup"><span data-stu-id="0bc83-315">With P15, you can expect about 57 times hello performance gain by using hello columnstore index.</span></span>



## <a name="next-steps"></a><span data-ttu-id="0bc83-316">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0bc83-316">Next steps</span></span>

- [<span data-ttu-id="0bc83-317">Rychlý Start 1: Technologie OLTP v paměti pro dosažení vyššího výkonu T-SQL</span><span class="sxs-lookup"><span data-stu-id="0bc83-317">Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance</span></span>](http://msdn.microsoft.com/library/mt694156.aspx)

- [<span data-ttu-id="0bc83-318">Použití OLTP v paměti v aplikaci existující Azure SQL</span><span class="sxs-lookup"><span data-stu-id="0bc83-318">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

- <span data-ttu-id="0bc83-319">[Monitorování OLTP v paměti úložiště](sql-database-in-memory-oltp-monitoring.md) pro OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="0bc83-319">[Monitor In-Memory OLTP storage](sql-database-in-memory-oltp-monitoring.md) for In-Memory OLTP</span></span>


## <a name="additional-resources"></a><span data-ttu-id="0bc83-320">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0bc83-320">Additional resources</span></span>

#### <a name="deeper-information"></a><span data-ttu-id="0bc83-321">Podrobnější informace.</span><span class="sxs-lookup"><span data-stu-id="0bc83-321">Deeper information</span></span>

- [<span data-ttu-id="0bc83-322">Zjistěte, jak kvora zdvojnásobí klíče databáze zatížení při snížení DTU 70 % s OLTP v paměti v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="0bc83-322">Learn how Quorum doubles key database’s workload while lowering DTU by 70% with In-Memory OLTP in SQL Database</span></span>](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [<span data-ttu-id="0bc83-323">OLTP v paměti v příspěvku na blogu databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="0bc83-323">In-Memory OLTP in Azure SQL Database Blog Post</span></span>](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [<span data-ttu-id="0bc83-324">Další informace o OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="0bc83-324">Learn about In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="0bc83-325">Další informace o indexy columnstore</span><span class="sxs-lookup"><span data-stu-id="0bc83-325">Learn about columnstore indexes</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)

- [<span data-ttu-id="0bc83-326">Další informace o provozní analýzu v reálném čase</span><span class="sxs-lookup"><span data-stu-id="0bc83-326">Learn about real-time operational analytics</span></span>](http://msdn.microsoft.com/library/dn817827.aspx)

- <span data-ttu-id="0bc83-327">V tématu [vzory běžné úlohy a posouzení migrace](http://msdn.microsoft.com/library/dn673538.aspx) (které popisuje vzory úlohy, kde OLTP v paměti běžně nabízí výrazné zvýšení výkonu)</span><span class="sxs-lookup"><span data-stu-id="0bc83-327">See [Common Workload Patterns and Migration Considerations](http://msdn.microsoft.com/library/dn673538.aspx) (which describes workload patterns where In-Memory OLTP commonly provides significant performance gains)</span></span>

#### <a name="application-design"></a><span data-ttu-id="0bc83-328">Návrh aplikace</span><span class="sxs-lookup"><span data-stu-id="0bc83-328">Application design</span></span>

- [<span data-ttu-id="0bc83-329">Paměť OLTP (v paměti optimalizace)</span><span class="sxs-lookup"><span data-stu-id="0bc83-329">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="0bc83-330">Použití OLTP v paměti v aplikaci existující Azure SQL</span><span class="sxs-lookup"><span data-stu-id="0bc83-330">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a><span data-ttu-id="0bc83-331">Nástroje</span><span class="sxs-lookup"><span data-stu-id="0bc83-331">Tools</span></span>

- [<span data-ttu-id="0bc83-332">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0bc83-332">Azure portal</span></span>](https://portal.azure.com/)

- [<span data-ttu-id="0bc83-333">SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="0bc83-333">SQL Server Management Studio (SSMS)</span></span>](https://msdn.microsoft.com/library/mt238290.aspx)

- [<span data-ttu-id="0bc83-334">SQL Server Data Tools (SSDT)</span><span class="sxs-lookup"><span data-stu-id="0bc83-334">SQL Server Data Tools (SSDT)</span></span>](http://msdn.microsoft.com/library/mt204009.aspx)
