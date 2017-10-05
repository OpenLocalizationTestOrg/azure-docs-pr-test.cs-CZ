---
title: "Načtení dat do Azure SQL Data Warehouse | Microsoft Docs"
description: "Další běžné scénáře pro data načítání do SQL Data Warehouse. Patří sem pomocí PolyBase, úložiště objektů blob v Azure, plochých souborů a přesouvání disku. Můžete také použít nástroje třetích stran."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c4199a387f5cdbd477a5e348e48ba8e8b5900075
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="4b90a-105">Načtení dat do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4b90a-105">Load data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="4b90a-106">Přehled možností scénáře a doporučení pro načítání dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4b90a-106">A summary of the scenario options and recommendations for loading data into SQL Data Warehouse.</span></span>

<span data-ttu-id="4b90a-107">Nejtěžší součástí načítání dat je obvykle Příprava zatížení data.</span><span class="sxs-lookup"><span data-stu-id="4b90a-107">The hardest part of loading data is usually preparing the data for the load.</span></span> <span data-ttu-id="4b90a-108">Azure zjednodušuje načítání pomocí úložiště objektů blob v Azure jako běžné úložiště dat pro mnoho služeb, a pomocí Azure Data Factory k orchestraci přesouvání komunikace a dat mezi službami Azure.</span><span class="sxs-lookup"><span data-stu-id="4b90a-108">Azure simplifies loading by using Azure blob storage as a common data store for many of the services, and using Azure Data Factory to orchestrate communication and data movement between the Azure services.</span></span> <span data-ttu-id="4b90a-109">Tyto procesy jsou integrované s technologií PolyBase, který používá (MPP) massively parallel processing načíst data paralelně z Azure blob storage do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4b90a-109">These processes are integrated with PolyBase technology which uses massively parallel processing (MPP) to load data in parallel from Azure blob storage into SQL Data Warehouse.</span></span> 

<span data-ttu-id="4b90a-110">Podrobné pokyny, které načítají ukázkové databáze, najdete v části [načíst ukázkové databáze][Load sample databases].</span><span class="sxs-lookup"><span data-stu-id="4b90a-110">For tutorials that load sample databases, see [Load sample databases][Load sample databases].</span></span>

## <a name="load-from-azure-blob-storage"></a><span data-ttu-id="4b90a-111">Načtení z Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="4b90a-111">Load from Azure blob storage</span></span>
<span data-ttu-id="4b90a-112">Nejrychlejší způsob, jak importovat data do SQL Data Warehouse je pomocí funkce PolyBase načteme dat z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="4b90a-112">The fastest way to import data into SQL Data Warehouse is to use PolyBase to load data from Azure blob storage.</span></span> <span data-ttu-id="4b90a-113">PolyBase používá SQL Data Warehouse (MPP) massively parallel processing návrhu načíst data paralelně z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="4b90a-113">PolyBase uses SQL Data Warehouse's massively parallel processing (MPP) design to load data in parallel from Azure blob storage.</span></span> <span data-ttu-id="4b90a-114">Pokud chcete používat funkce PolyBase, můžete použít příkazy T-SQL nebo kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4b90a-114">To use PolyBase, you can use T-SQL commands or an Azure Data Factory pipeline.</span></span>

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="4b90a-115">1. Použijte PolyBase a T-SQL</span><span class="sxs-lookup"><span data-stu-id="4b90a-115">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="4b90a-116">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="4b90a-116">Summary of loading process:</span></span>

1. <span data-ttu-id="4b90a-117">Přesun dat do úložiště objektů blob v Azure nebo Azure Data Lake Store a uloží jej v textových souborů.</span><span class="sxs-lookup"><span data-stu-id="4b90a-117">Move your data to Azure blob storage or Azure Data Lake Store and store it in text files.</span></span>
2. <span data-ttu-id="4b90a-118">Nakonfigurujte externí objekty v SQL Data Warehouse zadat umístění a formát dat</span><span class="sxs-lookup"><span data-stu-id="4b90a-118">Configure external objects in SQL Data Warehouse to define the location and format of the data</span></span>
3. <span data-ttu-id="4b90a-119">Spusťte příkaz T-SQL chcete načíst data souběžně do nové tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="4b90a-119">Run a T-SQL command to load the data in parallel into a new database table.</span></span>

<!-- 5. Schedule and run a loading job. --> 

<span data-ttu-id="4b90a-120">Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="4b90a-120">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span></span>

### <a name="2-use-azure-data-factory"></a><span data-ttu-id="4b90a-121">2. Použití Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4b90a-121">2. Use Azure Data Factory</span></span>
<span data-ttu-id="4b90a-122">Pro jednodušší způsob, jak pomocí funkce PolyBase můžete vytvořit kanál Azure Data Factory, využívající funkci PolyBase k načtení dat z Azure blob storage do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4b90a-122">For a simpler way to use PolyBase, you can create an Azure Data Factory pipeline that uses PolyBase to load data from Azure blob storage into SQL Data Warehouse.</span></span> <span data-ttu-id="4b90a-123">Toto je rychlé konfigurace vzhledem k tomu, že nemusíte definovat objekty, T-SQL.</span><span class="sxs-lookup"><span data-stu-id="4b90a-123">This is fast to configure since you don't need to define the T-SQL objects.</span></span> <span data-ttu-id="4b90a-124">Pokud potřebujete dotazu na externí data bez provedení importu, použijte T-SQL.</span><span class="sxs-lookup"><span data-stu-id="4b90a-124">If you need to query the external data without importing it, use T-SQL.</span></span> 

<span data-ttu-id="4b90a-125">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="4b90a-125">Summary of loading process:</span></span>

1. <span data-ttu-id="4b90a-126">Přesunutí dat do Azure blob storage a uloží jej v textových souborů.</span><span class="sxs-lookup"><span data-stu-id="4b90a-126">Move your data to Azure blob storage and store it in text files.</span></span> <span data-ttu-id="4b90a-127">Azure Data Factory aktuálně nepodporuje ADLS připojení pomocí funkce PolyBase).</span><span class="sxs-lookup"><span data-stu-id="4b90a-127">Azure Data Factory does not currently support ADLS connectivity with PolyBase).</span></span>
2. <span data-ttu-id="4b90a-128">Vytvořte kanál Azure Data Factory k ingestování data.</span><span class="sxs-lookup"><span data-stu-id="4b90a-128">Create an Azure Data Factory pipeline to ingest the data.</span></span> <span data-ttu-id="4b90a-129">Použijte možnost PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4b90a-129">Use the PolyBase option.</span></span>
4. <span data-ttu-id="4b90a-130">Naplánování a spuštění kanálu.</span><span class="sxs-lookup"><span data-stu-id="4b90a-130">Schedule and run the pipeline.</span></span>

<span data-ttu-id="4b90a-131">Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].</span><span class="sxs-lookup"><span data-stu-id="4b90a-131">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].</span></span>

## <a name="load-from-sql-server"></a><span data-ttu-id="4b90a-132">Načtení z SQL serveru</span><span class="sxs-lookup"><span data-stu-id="4b90a-132">Load from SQL Server</span></span>
<span data-ttu-id="4b90a-133">Načtení dat z SQL serveru do SQL Data Warehouse můžete Integration Services (SSIS) a přenos plochých souborů nebo disky lodě společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4b90a-133">To load data from SQL Server to SQL Data Warehouse you can use Integration Services (SSIS), transfer flat files, or ship disks to Microsoft.</span></span> <span data-ttu-id="4b90a-134">Pro čtení k najdete souhrn různými načítání procesy a odkazy na výukové programy.</span><span class="sxs-lookup"><span data-stu-id="4b90a-134">Read on to see a summary of the different loading processes and links to tutorials.</span></span>

<span data-ttu-id="4b90a-135">Plánování migrace úplná data z SQL serveru do SQL Data Warehouse, najdete v článku [Přehled migrace][Migration overview].</span><span class="sxs-lookup"><span data-stu-id="4b90a-135">To plan a full data migration from SQL Server to SQL Data Warehouse, see the [Migration overview][Migration overview].</span></span> 

### <a name="use-integration-services-ssis"></a><span data-ttu-id="4b90a-136">Použití integrační služby (SSIS)</span><span class="sxs-lookup"><span data-stu-id="4b90a-136">Use Integration Services (SSIS)</span></span>
<span data-ttu-id="4b90a-137">Pokud už používáte balíčky Integration Services (SSIS) do systému SQL Server, můžete je aktualizovat balíčky serveru použít SQL Server jako zdroj a SQL Data Warehouse jako cíl.</span><span class="sxs-lookup"><span data-stu-id="4b90a-137">If you are already using Integration Services (SSIS) packages to load into SQL Server, you can update your packages to use SQL Server as the source and SQL Data Warehouse as the destination.</span></span> <span data-ttu-id="4b90a-138">To je rychlé a snadné provést, a je vhodný, pokud se snažíte provést migraci váš proces načítání již používat data v cloudu.</span><span class="sxs-lookup"><span data-stu-id="4b90a-138">This is quick and easy to do, and is a good choice if you are not trying to migrate your loading process to use data already in the cloud.</span></span> <span data-ttu-id="4b90a-139">Kompromis je že zatížení bude nižší než pomocí PolyBase, protože tento SSIS neprovádí zatížení paralelně.</span><span class="sxs-lookup"><span data-stu-id="4b90a-139">The tradeoff is the load will be slower than using PolyBase because this SSIS does not perform the load in parallel.</span></span>

<span data-ttu-id="4b90a-140">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="4b90a-140">Summary of loading process:</span></span>

1. <span data-ttu-id="4b90a-141">Zkontrolovat, jestli vaše balíčku integračních služeb tak, aby odkazoval na instanci serveru SQL pro zdroj a databázi SQL Data Warehouse pro cíl.</span><span class="sxs-lookup"><span data-stu-id="4b90a-141">Revise your Integration Services package to point to the SQL Server instance for the source and the SQL Data Warehouse database for the destination.</span></span>
2. <span data-ttu-id="4b90a-142">Migrujte schéma do SQL Data Warehouse, pokud ještě není existuje.</span><span class="sxs-lookup"><span data-stu-id="4b90a-142">Migrate your schema to SQL Data Warehouse, if it is not there already.</span></span>
3. <span data-ttu-id="4b90a-143">Změňte mapování ve vašich balíčcích použít pouze typy dat, které jsou podporovány nástrojem SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4b90a-143">Change the mapping in your packages use only the data types that are supported by SQL Data Warehouse.</span></span>
4. <span data-ttu-id="4b90a-144">Naplánování a spuštění balíčku.</span><span class="sxs-lookup"><span data-stu-id="4b90a-144">Schedule and run the package.</span></span>

<span data-ttu-id="4b90a-145">Podívejte se kurz [načtení dat z SQL serveru Azure SQL Data Warehouse (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].</span><span class="sxs-lookup"><span data-stu-id="4b90a-145">For a tutorial, see [Load data from SQL Server to Azure SQL Data Warehouse (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].</span></span>

### <a name="use-azcopy-recommended-for--10-tb-data"></a><span data-ttu-id="4b90a-146">Pomocí nástroje AZCopy (doporučeno pro < 10 TB dat)</span><span class="sxs-lookup"><span data-stu-id="4b90a-146">Use AZCopy (recommended for < 10 TB data)</span></span>
<span data-ttu-id="4b90a-147">Pokud má velikost dat < 10 TB, může exportu dat z SQL serveru do plochých souborů, zkopírujte soubory do Azure blob storage a pak pomocí funkce PolyBase načíst data do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4b90a-147">If your data size is < 10 TB, you can export the data from SQL Server to flat files, copy the files to Azure blob storage, and then use PolyBase to load the data into SQL Data Warehouse</span></span>

<span data-ttu-id="4b90a-148">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="4b90a-148">Summary of loading process:</span></span>

1. <span data-ttu-id="4b90a-149">Pomocí nástroje příkazového řádku bcp k exportu dat z SQL serveru do plochých souborů.</span><span class="sxs-lookup"><span data-stu-id="4b90a-149">Use the bcp command-line utility to export data from SQL Server to flat files.</span></span>
2. <span data-ttu-id="4b90a-150">Pomocí nástroje příkazového řádku AZCopy ke zkopírování dat z plochých souborů do úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4b90a-150">Use the AZCopy command-line utility to copy data from flat files to Azure blob storage.</span></span>
3. <span data-ttu-id="4b90a-151">Pomocí funkce PolyBase načteme do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4b90a-151">Use PolyBase to load into SQL Data Warehouse.</span></span>

<span data-ttu-id="4b90a-152">Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="4b90a-152">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span></span>

### <a name="use-bcp"></a><span data-ttu-id="4b90a-153">Pomocí bcp</span><span class="sxs-lookup"><span data-stu-id="4b90a-153">Use bcp</span></span>
<span data-ttu-id="4b90a-154">Pokud máte malé množství dat můžete načíst přímo do Azure SQL Data Warehouse bcp.</span><span class="sxs-lookup"><span data-stu-id="4b90a-154">If you have a small amount of data you can use bcp to load directly into Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="4b90a-155">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="4b90a-155">Summary of loading process:</span></span>

1. <span data-ttu-id="4b90a-156">Pomocí nástroje příkazového řádku bcp k exportu dat z SQL serveru do plochých souborů.</span><span class="sxs-lookup"><span data-stu-id="4b90a-156">Use the bcp command-line utility to export data from SQL Server to flat files.</span></span>
2. <span data-ttu-id="4b90a-157">Pomocí bcp k načtení dat z plochých souborů přímo do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4b90a-157">Use bcp to load data from flat files directly to SQL Data Warehouse.</span></span>

<span data-ttu-id="4b90a-158">Podívejte se kurz [načtení dat z SQL serveru do Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].</span><span class="sxs-lookup"><span data-stu-id="4b90a-158">For a tutorial, see [Load data from SQL Server to Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].</span></span>

### <a name="use-importexport-recommended-for--10-tb-data"></a><span data-ttu-id="4b90a-159">Použití importu a exportu (doporučeno pro > 10 TB dat)</span><span class="sxs-lookup"><span data-stu-id="4b90a-159">Use Import/Export (recommended for > 10 TB data)</span></span>
<span data-ttu-id="4b90a-160">Pokud vaše velikost dat je > 10 TB a chcete ho přesunout do Azure, doporučujeme používat naše disku přesouvání služby [importu a exportu][Import/Export].</span><span class="sxs-lookup"><span data-stu-id="4b90a-160">If your data size is > 10 TB and you want to move it to Azure, we recommend that you use our disk shipping service [Import/Export][Import/Export].</span></span> 

<span data-ttu-id="4b90a-161">Souhrn procesu načtení</span><span class="sxs-lookup"><span data-stu-id="4b90a-161">Summary of loading process</span></span>

1. <span data-ttu-id="4b90a-162">Pomocí nástroje příkazového řádku bcp k exportu dat z SQL serveru do plochých souborů na přenositelné disky.</span><span class="sxs-lookup"><span data-stu-id="4b90a-162">Use the bcp command-line utility to export data from SQL Server to flat files on transferrable disks.</span></span>
2. <span data-ttu-id="4b90a-163">Dodávat disky společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4b90a-163">Ship the disks to Microsoft.</span></span>
3. <span data-ttu-id="4b90a-164">Microsoft načte data do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4b90a-164">Microsoft loads the data into SQL Data Warehouse</span></span>

## <a name="load-from-hdinsight"></a><span data-ttu-id="4b90a-165">Načtení z prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="4b90a-165">Load from HDInsight</span></span>
<span data-ttu-id="4b90a-166">SQL Data Warehouse podporuje načítání dat z HDInsight prostřednictvím PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4b90a-166">SQL Data Warehouse supports loading data from HDInsight via PolyBase.</span></span> <span data-ttu-id="4b90a-167">Proces je stejný jako načítání dat z Azure Blob Storage – pro připojení k HDInsight k načtení dat pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4b90a-167">The process is the same as loading data from Azure Blob Storage - using PolyBase to connect to HDInsight to load data.</span></span> 

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="4b90a-168">1. Použijte PolyBase a T-SQL</span><span class="sxs-lookup"><span data-stu-id="4b90a-168">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="4b90a-169">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="4b90a-169">Summary of loading process:</span></span>

1. <span data-ttu-id="4b90a-170">Přesunutí dat do HDInsight a uloží jej v textových souborů ORC nebo Parquet formátu.</span><span class="sxs-lookup"><span data-stu-id="4b90a-170">Move your data to HDInsight and store it in text files, ORC or Parquet format.</span></span>
2. <span data-ttu-id="4b90a-171">Nakonfigurujte externí objekty v SQL Data Warehouse zadat umístění a formát data.</span><span class="sxs-lookup"><span data-stu-id="4b90a-171">Configure external objects in SQL Data Warehouse to define the location and format of the data.</span></span>
3. <span data-ttu-id="4b90a-172">Spusťte příkaz T-SQL chcete načíst data souběžně do nové tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="4b90a-172">Run a T-SQL command to load the data in parallel into a new database table.</span></span>

<span data-ttu-id="4b90a-173">Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="4b90a-173">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span></span>

## <a name="recommendations"></a><span data-ttu-id="4b90a-174">Doporučení</span><span class="sxs-lookup"><span data-stu-id="4b90a-174">Recommendations</span></span>
<span data-ttu-id="4b90a-175">Mnoho našich partnerů má načítání řešení.</span><span class="sxs-lookup"><span data-stu-id="4b90a-175">Many of our partners have loading solutions.</span></span> <span data-ttu-id="4b90a-176">Další informace, podívejte se do seznamu naše [partnery poskytujícími řešení][solution partners].</span><span class="sxs-lookup"><span data-stu-id="4b90a-176">To find out more, see a list of our [solution partners][solution partners].</span></span> 

<span data-ttu-id="4b90a-177">Pokud vaše data pochází z nerelační zdroje a chcete načíst do SQL Data Warehouse je nutné jej před načtením transformovat do řádků a sloupců.</span><span class="sxs-lookup"><span data-stu-id="4b90a-177">If your data is coming from a non-relational source and you want to load it into SQL Data Warehouse you will need to transform it into rows and columns before you load it.</span></span> <span data-ttu-id="4b90a-178">Transformovaná data nemusí být uložená v databázi, mohou být uloženy v textových souborů.</span><span class="sxs-lookup"><span data-stu-id="4b90a-178">The transformed data doesn't need to be stored in a database, it can be stored in text files.</span></span>

<span data-ttu-id="4b90a-179">Vytvoření statistiky pro nově načtená data.</span><span class="sxs-lookup"><span data-stu-id="4b90a-179">Create statistics on newly loaded data.</span></span> <span data-ttu-id="4b90a-180">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="4b90a-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="4b90a-181">Chcete-li získat nejlepší výkon ze své dotazy, je důležité vytvořit statistiku pro všechny sloupce všech tabulek po prvním načtením nebo dojít k významné změny v datech.</span><span class="sxs-lookup"><span data-stu-id="4b90a-181">In order to get the best performance from your queries, it's important to create statistics on all columns of all tables after the first load or any substantial changes occur in the data.</span></span>  <span data-ttu-id="4b90a-182">Podrobnosti najdete v tématu [statistiky][Statistics].</span><span class="sxs-lookup"><span data-stu-id="4b90a-182">For details, see [Statistics][Statistics].</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b90a-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b90a-183">Next steps</span></span>
<span data-ttu-id="4b90a-184">Další tipy pro vývoj, najdete v článku [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="4b90a-184">For more development tips, see the [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage to SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server to Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server to Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
