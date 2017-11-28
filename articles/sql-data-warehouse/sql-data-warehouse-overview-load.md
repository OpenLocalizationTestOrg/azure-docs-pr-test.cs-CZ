---
title: aaaLoad data do Azure SQL Data Warehouse | Microsoft Docs
description: "Další běžné scénáře hello dat načítání do SQL Data Warehouse. Patří sem pomocí PolyBase, úložiště objektů blob v Azure, plochých souborů a přesouvání disku. Můžete také použít nástroje třetích stran."
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
ms.openlocfilehash: d1a5063f484e9bd95f854e27a4baed512148aad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="961e9-105">Načtení dat do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="961e9-105">Load data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="961e9-106">Přehled možností hello scénáře a doporučení pro načítání dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="961e9-106">A summary of hello scenario options and recommendations for loading data into SQL Data Warehouse.</span></span>

<span data-ttu-id="961e9-107">načítání dat Hello nejtěžší část obvykle připravuje hello data pro zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-107">hello hardest part of loading data is usually preparing hello data for hello load.</span></span> <span data-ttu-id="961e9-108">Azure zjednodušuje načítání pomocí úložiště objektů blob v Azure jako běžné úložiště dat pro mnoho hello služeb, a pomocí Azure Data Factory hello tooorchestrate komunikačních a datových přesun mezi službami Azure.</span><span class="sxs-lookup"><span data-stu-id="961e9-108">Azure simplifies loading by using Azure blob storage as a common data store for many of hello services, and using Azure Data Factory tooorchestrate communication and data movement between hello Azure services.</span></span> <span data-ttu-id="961e9-109">Tyto procesy jsou integrované s technologií PolyBase, který používá masivně paralelní zpracování dat (MPP) tooload paralelně z Azure blob storage do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="961e9-109">These processes are integrated with PolyBase technology which uses massively parallel processing (MPP) tooload data in parallel from Azure blob storage into SQL Data Warehouse.</span></span> 

<span data-ttu-id="961e9-110">Podrobné pokyny, které načítají ukázkové databáze, najdete v části [načíst ukázkové databáze][Load sample databases].</span><span class="sxs-lookup"><span data-stu-id="961e9-110">For tutorials that load sample databases, see [Load sample databases][Load sample databases].</span></span>

## <a name="load-from-azure-blob-storage"></a><span data-ttu-id="961e9-111">Načtení z Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="961e9-111">Load from Azure blob storage</span></span>
<span data-ttu-id="961e9-112">Hello nejrychlejší způsob, jak tooimport dat do SQL Data Warehouse je toouse PolyBase tooload dat z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="961e9-112">hello fastest way tooimport data into SQL Data Warehouse is toouse PolyBase tooload data from Azure blob storage.</span></span> <span data-ttu-id="961e9-113">PolyBase používá SQL Data Warehouse na masivně paralelní zpracování (MPP) návrhu tooload dat paralelně z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="961e9-113">PolyBase uses SQL Data Warehouse's massively parallel processing (MPP) design tooload data in parallel from Azure blob storage.</span></span> <span data-ttu-id="961e9-114">toouse PolyBase, můžete použít příkazy T-SQL nebo kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="961e9-114">toouse PolyBase, you can use T-SQL commands or an Azure Data Factory pipeline.</span></span>

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="961e9-115">1. Použijte PolyBase a T-SQL</span><span class="sxs-lookup"><span data-stu-id="961e9-115">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="961e9-116">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="961e9-116">Summary of loading process:</span></span>

1. <span data-ttu-id="961e9-117">Přesunout úložiště objektů blob tooAzure dat nebo Azure Data Lake Store a uloží jej v textových souborů.</span><span class="sxs-lookup"><span data-stu-id="961e9-117">Move your data tooAzure blob storage or Azure Data Lake Store and store it in text files.</span></span>
2. <span data-ttu-id="961e9-118">Nakonfigurujte externí objekty v SQL Data Warehouse toodefine hello umístění a formát dat hello</span><span class="sxs-lookup"><span data-stu-id="961e9-118">Configure external objects in SQL Data Warehouse toodefine hello location and format of hello data</span></span>
3. <span data-ttu-id="961e9-119">Paralelní spuštění T-SQL příkazu tooload hello data do nové tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="961e9-119">Run a T-SQL command tooload hello data in parallel into a new database table.</span></span>

<!-- 5. Schedule and run a loading job. --> 

<span data-ttu-id="961e9-120">Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="961e9-120">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

### <a name="2-use-azure-data-factory"></a><span data-ttu-id="961e9-121">2. Použití Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="961e9-121">2. Use Azure Data Factory</span></span>
<span data-ttu-id="961e9-122">Pro jednodušší způsob toouse PolyBase můžete vytvořit kanál Azure Data Factory, který používá PolyBase tooload dat z Azure blob storage do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="961e9-122">For a simpler way toouse PolyBase, you can create an Azure Data Factory pipeline that uses PolyBase tooload data from Azure blob storage into SQL Data Warehouse.</span></span> <span data-ttu-id="961e9-123">To je rychlé tooconfigure, protože nepotřebujete toodefine hello T-SQL objekty.</span><span class="sxs-lookup"><span data-stu-id="961e9-123">This is fast tooconfigure since you don't need toodefine hello T-SQL objects.</span></span> <span data-ttu-id="961e9-124">Pokud potřebujete tooquery hello externích dat bez provedení importu, použijte T-SQL.</span><span class="sxs-lookup"><span data-stu-id="961e9-124">If you need tooquery hello external data without importing it, use T-SQL.</span></span> 

<span data-ttu-id="961e9-125">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="961e9-125">Summary of loading process:</span></span>

1. <span data-ttu-id="961e9-126">Přesunout úložiště objektů blob tooAzure dat a uloží jej v textových souborů.</span><span class="sxs-lookup"><span data-stu-id="961e9-126">Move your data tooAzure blob storage and store it in text files.</span></span> <span data-ttu-id="961e9-127">Azure Data Factory aktuálně nepodporuje ADLS připojení pomocí funkce PolyBase).</span><span class="sxs-lookup"><span data-stu-id="961e9-127">Azure Data Factory does not currently support ADLS connectivity with PolyBase).</span></span>
2. <span data-ttu-id="961e9-128">Vytvoření Azure Data Factory kanálu tooingest hello data.</span><span class="sxs-lookup"><span data-stu-id="961e9-128">Create an Azure Data Factory pipeline tooingest hello data.</span></span> <span data-ttu-id="961e9-129">Použijte možnost PolyBase hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-129">Use hello PolyBase option.</span></span>
4. <span data-ttu-id="961e9-130">Naplánování a spuštění kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-130">Schedule and run hello pipeline.</span></span>

<span data-ttu-id="961e9-131">Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].</span><span class="sxs-lookup"><span data-stu-id="961e9-131">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].</span></span>

## <a name="load-from-sql-server"></a><span data-ttu-id="961e9-132">Načtení z SQL serveru</span><span class="sxs-lookup"><span data-stu-id="961e9-132">Load from SQL Server</span></span>
<span data-ttu-id="961e9-133">tooload data z tooSQL systému SQL Server datového skladu můžete Integration Services (SSIS), přeneste plochých souborů nebo dodávat tooMicrosoft disky.</span><span class="sxs-lookup"><span data-stu-id="961e9-133">tooload data from SQL Server tooSQL Data Warehouse you can use Integration Services (SSIS), transfer flat files, or ship disks tooMicrosoft.</span></span> <span data-ttu-id="961e9-134">Číst na toosee souhrn hello různých načítání tootutorials procesy a odkazy.</span><span class="sxs-lookup"><span data-stu-id="961e9-134">Read on toosee a summary of hello different loading processes and links tootutorials.</span></span>

<span data-ttu-id="961e9-135">tooplan úplná data migrace ze systému SQL Server tooSQL datového skladu, najdete v části hello [Přehled migrace][Migration overview].</span><span class="sxs-lookup"><span data-stu-id="961e9-135">tooplan a full data migration from SQL Server tooSQL Data Warehouse, see hello [Migration overview][Migration overview].</span></span> 

### <a name="use-integration-services-ssis"></a><span data-ttu-id="961e9-136">Použití integrační služby (SSIS)</span><span class="sxs-lookup"><span data-stu-id="961e9-136">Use Integration Services (SSIS)</span></span>
<span data-ttu-id="961e9-137">Pokud už používáte tooload balíčky Integration Services (SSIS) do systému SQL Server, můžete je aktualizovat balíčky toouse systému SQL Server jako zdroj hello a SQL Data Warehouse jako cíl hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-137">If you are already using Integration Services (SSIS) packages tooload into SQL Server, you can update your packages toouse SQL Server as hello source and SQL Data Warehouse as hello destination.</span></span> <span data-ttu-id="961e9-138">To je rychlý a snadný toodo, a je vhodný, pokud se nepokoušíte toomigrate vaše načítání zpracování dat toouse již v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-138">This is quick and easy toodo, and is a good choice if you are not trying toomigrate your loading process toouse data already in hello cloud.</span></span> <span data-ttu-id="961e9-139">kompromis Hello je hello zatížení bude nižší než pomocí PolyBase, protože tento SSIS neprovádí hello zatížení paralelně.</span><span class="sxs-lookup"><span data-stu-id="961e9-139">hello tradeoff is hello load will be slower than using PolyBase because this SSIS does not perform hello load in parallel.</span></span>

<span data-ttu-id="961e9-140">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="961e9-140">Summary of loading process:</span></span>

1. <span data-ttu-id="961e9-141">Upraveno integrační služby balíček toopoint toohello instanci serveru SQL pro zdroj hello a databázi SQL Data Warehouse hello pro cíl hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-141">Revise your Integration Services package toopoint toohello SQL Server instance for hello source and hello SQL Data Warehouse database for hello destination.</span></span>
2. <span data-ttu-id="961e9-142">Vaše tooSQL schématu datového skladu, migrujte, pokud ještě není existuje.</span><span class="sxs-lookup"><span data-stu-id="961e9-142">Migrate your schema tooSQL Data Warehouse, if it is not there already.</span></span>
3. <span data-ttu-id="961e9-143">Mapování hello změn ve vašich balíčcích používají pouze datové typy hello, které jsou podporovány nástrojem SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="961e9-143">Change hello mapping in your packages use only hello data types that are supported by SQL Data Warehouse.</span></span>
4. <span data-ttu-id="961e9-144">Naplánování a spuštění balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-144">Schedule and run hello package.</span></span>

<span data-ttu-id="961e9-145">Podívejte se kurz [načtení dat z tooAzure systému SQL Server datového skladu SSIS (SQL)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].</span><span class="sxs-lookup"><span data-stu-id="961e9-145">For a tutorial, see [Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].</span></span>

### <a name="use-azcopy-recommended-for--10-tb-data"></a><span data-ttu-id="961e9-146">Pomocí nástroje AZCopy (doporučeno pro < 10 TB dat)</span><span class="sxs-lookup"><span data-stu-id="961e9-146">Use AZCopy (recommended for < 10 TB data)</span></span>
<span data-ttu-id="961e9-147">Pokud má velikost dat < 10 TB, můžete exportovat hello data z tooflat soubory systému SQL Server, zkopírovat úložiště objektů blob tooAzure hello soubory a potom pomocí funkce PolyBase tooload hello dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="961e9-147">If your data size is < 10 TB, you can export hello data from SQL Server tooflat files, copy hello files tooAzure blob storage, and then use PolyBase tooload hello data into SQL Data Warehouse</span></span>

<span data-ttu-id="961e9-148">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="961e9-148">Summary of loading process:</span></span>

1. <span data-ttu-id="961e9-149">Použijte data tooexport nástroj příkazového řádku bcp hello z tooflat soubory systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="961e9-149">Use hello bcp command-line utility tooexport data from SQL Server tooflat files.</span></span>
2. <span data-ttu-id="961e9-150">Použijte hello AZCopy nástroj příkazového řádku toocopy dat z plochých souborů tooAzure blob storage.</span><span class="sxs-lookup"><span data-stu-id="961e9-150">Use hello AZCopy command-line utility toocopy data from flat files tooAzure blob storage.</span></span>
3. <span data-ttu-id="961e9-151">Použijte PolyBase tooload do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="961e9-151">Use PolyBase tooload into SQL Data Warehouse.</span></span>

<span data-ttu-id="961e9-152">Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="961e9-152">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

### <a name="use-bcp"></a><span data-ttu-id="961e9-153">Pomocí bcp</span><span class="sxs-lookup"><span data-stu-id="961e9-153">Use bcp</span></span>
<span data-ttu-id="961e9-154">Pokud máte malé množství dat můžete bcp tooload přímo do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="961e9-154">If you have a small amount of data you can use bcp tooload directly into Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="961e9-155">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="961e9-155">Summary of loading process:</span></span>

1. <span data-ttu-id="961e9-156">Použijte data tooexport nástroj příkazového řádku bcp hello z tooflat soubory systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="961e9-156">Use hello bcp command-line utility tooexport data from SQL Server tooflat files.</span></span>
2. <span data-ttu-id="961e9-157">Použijte bcp tooload data od ploché soubory přímo tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="961e9-157">Use bcp tooload data from flat files directly tooSQL Data Warehouse.</span></span>

<span data-ttu-id="961e9-158">Podívejte se kurz [načtení dat z SQL serveru tooAzure SQL Data Warehouse (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].</span><span class="sxs-lookup"><span data-stu-id="961e9-158">For a tutorial, see [Load data from SQL Server tooAzure SQL Data Warehouse (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].</span></span>

### <a name="use-importexport-recommended-for--10-tb-data"></a><span data-ttu-id="961e9-159">Použití importu a exportu (doporučeno pro > 10 TB dat)</span><span class="sxs-lookup"><span data-stu-id="961e9-159">Use Import/Export (recommended for > 10 TB data)</span></span>
<span data-ttu-id="961e9-160">Pokud je vaše velikost dat > 10 TB a chcete toomove ho tooAzure, doporučujeme používat naše disku přesouvání služby [importu a exportu][Import/Export].</span><span class="sxs-lookup"><span data-stu-id="961e9-160">If your data size is > 10 TB and you want toomove it tooAzure, we recommend that you use our disk shipping service [Import/Export][Import/Export].</span></span> 

<span data-ttu-id="961e9-161">Souhrn procesu načtení</span><span class="sxs-lookup"><span data-stu-id="961e9-161">Summary of loading process</span></span>

1. <span data-ttu-id="961e9-162">Použijte data tooexport nástroj příkazového řádku bcp hello z tooflat soubory systému SQL Server na přenositelné disky.</span><span class="sxs-lookup"><span data-stu-id="961e9-162">Use hello bcp command-line utility tooexport data from SQL Server tooflat files on transferrable disks.</span></span>
2. <span data-ttu-id="961e9-163">Dodávat tooMicrosoft disky hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-163">Ship hello disks tooMicrosoft.</span></span>
3. <span data-ttu-id="961e9-164">Microsoft načte hello data do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="961e9-164">Microsoft loads hello data into SQL Data Warehouse</span></span>

## <a name="load-from-hdinsight"></a><span data-ttu-id="961e9-165">Načtení z prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="961e9-165">Load from HDInsight</span></span>
<span data-ttu-id="961e9-166">SQL Data Warehouse podporuje načítání dat z HDInsight prostřednictvím PolyBase.</span><span class="sxs-lookup"><span data-stu-id="961e9-166">SQL Data Warehouse supports loading data from HDInsight via PolyBase.</span></span> <span data-ttu-id="961e9-167">proces Hello je hello stejné jako načítání dat z Azure Blob Storage – pomocí PolyBase tooconnect tooHDInsight tooload data.</span><span class="sxs-lookup"><span data-stu-id="961e9-167">hello process is hello same as loading data from Azure Blob Storage - using PolyBase tooconnect tooHDInsight tooload data.</span></span> 

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="961e9-168">1. Použijte PolyBase a T-SQL</span><span class="sxs-lookup"><span data-stu-id="961e9-168">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="961e9-169">Souhrn procesu načtení:</span><span class="sxs-lookup"><span data-stu-id="961e9-169">Summary of loading process:</span></span>

1. <span data-ttu-id="961e9-170">Přesunout tooHDInsight vaše data a ukládá je v textových souborů ORC nebo Parquet formátu.</span><span class="sxs-lookup"><span data-stu-id="961e9-170">Move your data tooHDInsight and store it in text files, ORC or Parquet format.</span></span>
2. <span data-ttu-id="961e9-171">Nakonfigurujte externí objekty v SQL Data Warehouse toodefine hello umístění a formát dat hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-171">Configure external objects in SQL Data Warehouse toodefine hello location and format of hello data.</span></span>
3. <span data-ttu-id="961e9-172">Paralelní spuštění T-SQL příkazu tooload hello data do nové tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="961e9-172">Run a T-SQL command tooload hello data in parallel into a new database table.</span></span>

<span data-ttu-id="961e9-173">Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="961e9-173">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

## <a name="recommendations"></a><span data-ttu-id="961e9-174">Doporučení</span><span class="sxs-lookup"><span data-stu-id="961e9-174">Recommendations</span></span>
<span data-ttu-id="961e9-175">Mnoho našich partnerů má načítání řešení.</span><span class="sxs-lookup"><span data-stu-id="961e9-175">Many of our partners have loading solutions.</span></span> <span data-ttu-id="961e9-176">toofind informace, podívejte se do seznamu naše [partnery poskytujícími řešení][solution partners].</span><span class="sxs-lookup"><span data-stu-id="961e9-176">toofind out more, see a list of our [solution partners][solution partners].</span></span> 

<span data-ttu-id="961e9-177">Pokud vaše data pochází z nerelační zdroje a chcete tooload do SQL datového skladu můžete potřebovat tootransform do řádků a sloupců jej před načtením.</span><span class="sxs-lookup"><span data-stu-id="961e9-177">If your data is coming from a non-relational source and you want tooload it into SQL Data Warehouse you will need tootransform it into rows and columns before you load it.</span></span> <span data-ttu-id="961e9-178">Hello transformovat data nepotřebuje toobe uložená v databázi, mohou být uloženy v textových souborů.</span><span class="sxs-lookup"><span data-stu-id="961e9-178">hello transformed data doesn't need toobe stored in a database, it can be stored in text files.</span></span>

<span data-ttu-id="961e9-179">Vytvoření statistiky pro nově načtená data.</span><span class="sxs-lookup"><span data-stu-id="961e9-179">Create statistics on newly loaded data.</span></span> <span data-ttu-id="961e9-180">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="961e9-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="961e9-181">V pořadí tooget hello nejlepší výkon ze své dotazy je důležité nejdřív načíst toocreate statistiky pro všechny sloupce všech tabulek po hello nebo dojít k významné změny v datech hello.</span><span class="sxs-lookup"><span data-stu-id="961e9-181">In order tooget hello best performance from your queries, it's important toocreate statistics on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="961e9-182">Podrobnosti najdete v tématu [statistiky][Statistics].</span><span class="sxs-lookup"><span data-stu-id="961e9-182">For details, see [Statistics][Statistics].</span></span>

## <a name="next-steps"></a><span data-ttu-id="961e9-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="961e9-183">Next steps</span></span>
<span data-ttu-id="961e9-184">Další tipy pro vývoj, najdete v části hello [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="961e9-184">For more development tips, see hello [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server tooAzure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server tooAzure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
