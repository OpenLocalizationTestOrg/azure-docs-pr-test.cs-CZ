---
title: aaaUse bcp tooload dat do SQL Data Warehouse | Microsoft Docs
description: "Zjistěte, co je bcp a jak toouse pro scénáře datových skladů."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 09a2980585097644924c71899f9e74fb32fbc26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-bcp"></a><span data-ttu-id="e155f-103">Načtení dat pomocí bcp</span><span class="sxs-lookup"><span data-stu-id="e155f-103">Load data with bcp</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e155f-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="e155f-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)  
> * [<span data-ttu-id="e155f-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="e155f-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [<span data-ttu-id="e155f-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="e155f-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [<span data-ttu-id="e155f-107">BCP</span><span class="sxs-lookup"><span data-stu-id="e155f-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="e155f-108">**[BCP] [ bcp]**  je nástroj příkazového řádku pro hromadné zatížení, který vám umožní toocopy data mezi SQL serverem, datové soubory a SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e155f-108">**[bcp][bcp]** is a command-line bulk load utility that allows you toocopy data between SQL Server, data files, and SQL Data Warehouse.</span></span> <span data-ttu-id="e155f-109">Pomocí bcp tooimport velkého počtu řádků do tabulek SQL Data Warehouse nebo tooexport data z tabulek SQL serveru do datových souborů.</span><span class="sxs-lookup"><span data-stu-id="e155f-109">Use bcp tooimport large numbers of rows into SQL Data Warehouse tables or tooexport data from SQL Server tables into data files.</span></span> <span data-ttu-id="e155f-110">S výjimkou při použití s možností queryout hello BCP žádné znalosti jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="e155f-110">Except when used with hello queryout option, bcp requires no knowledge of Transact-SQL.</span></span>

<span data-ttu-id="e155f-111">je BCP rychlý a snadný způsob toomove menší sady dat do a z databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e155f-111">bcp is a quick and easy way toomove smaller data sets into and out of a SQL Data Warehouse database.</span></span> <span data-ttu-id="e155f-112">Hello přesné množství dat, který se doporučuje tooload/extrahovat přes bcp, bude záviset na vašem síťovém připojení toohello datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="e155f-112">hello exact amount of data that is recommended tooload/extract via bcp will depend on you network connection toohello Azure data center.</span></span>  <span data-ttu-id="e155f-113">Obecně platí, že pomocí bcp můžete snadno načítat a extrahovat tabulky dimenzí, nedoporučuje se ale používat pro načítání nebo extrahování velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="e155f-113">Generally, dimension tables can be loaded and extracted readily with bcp, however, bcp is not recommended for loading or extracting large volumes of data.</span></span>  <span data-ttu-id="e155f-114">Polybase je hello doporučuje nástroj pro načítání a extrahování velkých objemů dat, jako tomu je také vhodnější pro využívání architektuře massively parallel processing hello služby SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e155f-114">Polybase is hello recommended tool for loading and extracting large volumes of data as it does a better job leveraging hello massively parallel processing architecture of SQL Data Warehouse.</span></span>

<span data-ttu-id="e155f-115">Pomocí nástroje bcp můžete:</span><span class="sxs-lookup"><span data-stu-id="e155f-115">With bcp you can:</span></span>

* <span data-ttu-id="e155f-116">Pomocí jednoduchého nástroje příkazového řádku tooload dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e155f-116">Use a simple command-line utility tooload data into SQL Data Warehouse.</span></span>
* <span data-ttu-id="e155f-117">Pomocí jednoduchého nástroje příkazového řádku tooextract dat z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e155f-117">Use a simple command-line utility tooextract data from SQL Data Warehouse.</span></span>

<span data-ttu-id="e155f-118">V tomto kurzu se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="e155f-118">This tutorial will show you how to:</span></span>

* <span data-ttu-id="e155f-119">Import dat do tabulky pomocí příkazu hello bcp</span><span class="sxs-lookup"><span data-stu-id="e155f-119">Import data into a table using hello bcp in command</span></span>
* <span data-ttu-id="e155f-120">Exportovat data z tabulky pomocí hello příkazu bcp out</span><span class="sxs-lookup"><span data-stu-id="e155f-120">Export data from a table uisng hello bcp out command</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="e155f-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e155f-121">Prerequisites</span></span>
<span data-ttu-id="e155f-122">toostep prostřednictvím tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="e155f-122">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="e155f-123">Databázi SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e155f-123">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="e155f-124">nainstalovaný nástroj příkazového řádku bcp Hello</span><span class="sxs-lookup"><span data-stu-id="e155f-124">hello bcp command line utility installed</span></span>
* <span data-ttu-id="e155f-125">Hello nainstalovaný nástroj příkazového řádku SQLCMD</span><span class="sxs-lookup"><span data-stu-id="e155f-125">hello SQLCMD command line utility installed</span></span>

> [!NOTE]
> <span data-ttu-id="e155f-126">Hello nástroje bcp a sqlcmd si můžete stáhnout z hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="e155f-126">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a><span data-ttu-id="e155f-127">Import dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e155f-127">Import data into SQL Data Warehouse</span></span>
<span data-ttu-id="e155f-128">V tomto kurzu vytvoříte tabulku v Azure SQL Data Warehouse a importovat data do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e155f-128">In this tutorial, you will create a table in Azure SQL Data Warehouse and import data into hello table.</span></span>

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a><span data-ttu-id="e155f-129">Krok 1: Vytvoření tabulky v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e155f-129">Step 1: Create a table in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="e155f-130">Z příkazového řádku použijte hello toorun sqlcmd následující dotaz toocreate tabulku ve vaší instanci:</span><span class="sxs-lookup"><span data-stu-id="e155f-130">From a command prompt, use sqlcmd toorun hello following query toocreate a table on your instance:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

> [!NOTE]
> <span data-ttu-id="e155f-131">V tématu [tabulky přehled] [ Table Overview] nebo [syntaxe CREATE TABLE] [ CREATE TABLE syntax] Další informace o vytváření tabulek v SQL Data Warehouse a hello  Možnosti dostupné v klauzuli WITH hello.</span><span class="sxs-lookup"><span data-stu-id="e155f-131">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse and hello  options available in hello WITH clause.</span></span>
> 
> 

### <a name="step-2-create-a-source-data-file"></a><span data-ttu-id="e155f-132">Krok 2: Vytvoření zdrojového datového souboru</span><span class="sxs-lookup"><span data-stu-id="e155f-132">Step 2: Create a source data file</span></span>
<span data-ttu-id="e155f-133">Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového textového souboru a potom uložte tento soubor tooyour místního dočasného adresáře C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="e155f-133">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> <span data-ttu-id="e155f-134">Je důležité tooremember této bcp.exe nepodporuje kódování souborů UTF-8 hello.</span><span class="sxs-lookup"><span data-stu-id="e155f-134">It is important tooremember that bcp.exe does not support hello UTF-8 file encoding.</span></span> <span data-ttu-id="e155f-135">Při použití nástroje bcp.exe prosím použijte soubory ASCII nebo UTF-16.</span><span class="sxs-lookup"><span data-stu-id="e155f-135">Please use ASCII files or UTF-16 encoded files when using bcp.exe.</span></span>
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a><span data-ttu-id="e155f-136">Krok 3: Připojení a import dat hello</span><span class="sxs-lookup"><span data-stu-id="e155f-136">Step 3: Connect and import hello data</span></span>
<span data-ttu-id="e155f-137">Při použití nástroje bcp můžete připojit a import dat hello pomocí následující příkazu nahraďte hello hodnoty podle potřeby hello:</span><span class="sxs-lookup"><span data-stu-id="e155f-137">Using bcp, you can connect and import hello data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="e155f-138">Můžete ověřit hello byla data načtena spuštěním následujícího dotazu pomocí sqlcmd hello:</span><span class="sxs-lookup"><span data-stu-id="e155f-138">You can verify hello data was loaded by running hello following query using sqlcmd:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="e155f-139">To by měla vrátit hello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="e155f-139">This should return hello following results:</span></span>

| <span data-ttu-id="e155f-140">DateId</span><span class="sxs-lookup"><span data-stu-id="e155f-140">DateId</span></span> | <span data-ttu-id="e155f-141">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="e155f-141">CalendarQuarter</span></span> | <span data-ttu-id="e155f-142">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="e155f-142">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e155f-143">20150101</span><span class="sxs-lookup"><span data-stu-id="e155f-143">20150101</span></span> |<span data-ttu-id="e155f-144">1</span><span class="sxs-lookup"><span data-stu-id="e155f-144">1</span></span> |<span data-ttu-id="e155f-145">3</span><span class="sxs-lookup"><span data-stu-id="e155f-145">3</span></span> |
| <span data-ttu-id="e155f-146">20150201</span><span class="sxs-lookup"><span data-stu-id="e155f-146">20150201</span></span> |<span data-ttu-id="e155f-147">1</span><span class="sxs-lookup"><span data-stu-id="e155f-147">1</span></span> |<span data-ttu-id="e155f-148">3</span><span class="sxs-lookup"><span data-stu-id="e155f-148">3</span></span> |
| <span data-ttu-id="e155f-149">20150301</span><span class="sxs-lookup"><span data-stu-id="e155f-149">20150301</span></span> |<span data-ttu-id="e155f-150">1</span><span class="sxs-lookup"><span data-stu-id="e155f-150">1</span></span> |<span data-ttu-id="e155f-151">3</span><span class="sxs-lookup"><span data-stu-id="e155f-151">3</span></span> |
| <span data-ttu-id="e155f-152">20150401</span><span class="sxs-lookup"><span data-stu-id="e155f-152">20150401</span></span> |<span data-ttu-id="e155f-153">2</span><span class="sxs-lookup"><span data-stu-id="e155f-153">2</span></span> |<span data-ttu-id="e155f-154">4</span><span class="sxs-lookup"><span data-stu-id="e155f-154">4</span></span> |
| <span data-ttu-id="e155f-155">20150501</span><span class="sxs-lookup"><span data-stu-id="e155f-155">20150501</span></span> |<span data-ttu-id="e155f-156">2</span><span class="sxs-lookup"><span data-stu-id="e155f-156">2</span></span> |<span data-ttu-id="e155f-157">4</span><span class="sxs-lookup"><span data-stu-id="e155f-157">4</span></span> |
| <span data-ttu-id="e155f-158">20150601</span><span class="sxs-lookup"><span data-stu-id="e155f-158">20150601</span></span> |<span data-ttu-id="e155f-159">2</span><span class="sxs-lookup"><span data-stu-id="e155f-159">2</span></span> |<span data-ttu-id="e155f-160">4</span><span class="sxs-lookup"><span data-stu-id="e155f-160">4</span></span> |
| <span data-ttu-id="e155f-161">20150701</span><span class="sxs-lookup"><span data-stu-id="e155f-161">20150701</span></span> |<span data-ttu-id="e155f-162">3</span><span class="sxs-lookup"><span data-stu-id="e155f-162">3</span></span> |<span data-ttu-id="e155f-163">1</span><span class="sxs-lookup"><span data-stu-id="e155f-163">1</span></span> |
| <span data-ttu-id="e155f-164">20150801</span><span class="sxs-lookup"><span data-stu-id="e155f-164">20150801</span></span> |<span data-ttu-id="e155f-165">3</span><span class="sxs-lookup"><span data-stu-id="e155f-165">3</span></span> |<span data-ttu-id="e155f-166">1</span><span class="sxs-lookup"><span data-stu-id="e155f-166">1</span></span> |
| <span data-ttu-id="e155f-167">20150801</span><span class="sxs-lookup"><span data-stu-id="e155f-167">20150801</span></span> |<span data-ttu-id="e155f-168">3</span><span class="sxs-lookup"><span data-stu-id="e155f-168">3</span></span> |<span data-ttu-id="e155f-169">1</span><span class="sxs-lookup"><span data-stu-id="e155f-169">1</span></span> |
| <span data-ttu-id="e155f-170">20151001</span><span class="sxs-lookup"><span data-stu-id="e155f-170">20151001</span></span> |<span data-ttu-id="e155f-171">4</span><span class="sxs-lookup"><span data-stu-id="e155f-171">4</span></span> |<span data-ttu-id="e155f-172">2</span><span class="sxs-lookup"><span data-stu-id="e155f-172">2</span></span> |
| <span data-ttu-id="e155f-173">20151101</span><span class="sxs-lookup"><span data-stu-id="e155f-173">20151101</span></span> |<span data-ttu-id="e155f-174">4</span><span class="sxs-lookup"><span data-stu-id="e155f-174">4</span></span> |<span data-ttu-id="e155f-175">2</span><span class="sxs-lookup"><span data-stu-id="e155f-175">2</span></span> |
| <span data-ttu-id="e155f-176">20151201</span><span class="sxs-lookup"><span data-stu-id="e155f-176">20151201</span></span> |<span data-ttu-id="e155f-177">4</span><span class="sxs-lookup"><span data-stu-id="e155f-177">4</span></span> |<span data-ttu-id="e155f-178">2</span><span class="sxs-lookup"><span data-stu-id="e155f-178">2</span></span> |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="e155f-179">Krok 4: Vytvoření statistiky pro vaše nově načtená data</span><span class="sxs-lookup"><span data-stu-id="e155f-179">Step 4: Create Statistics on your newly loaded data</span></span>
<span data-ttu-id="e155f-180">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="e155f-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span> <span data-ttu-id="e155f-181">V pořadí tooget hello nejlepší výkon ze své dotazy je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením hello nebo dojít k významné změny v datech hello.</span><span class="sxs-lookup"><span data-stu-id="e155f-181">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span> <span data-ttu-id="e155f-182">Podrobné vysvětlení statistiky najdete v tématu hello [statistiky] [ Statistics] tématu ve skupině témat věnovaných vývoji hello.</span><span class="sxs-lookup"><span data-stu-id="e155f-182">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span> <span data-ttu-id="e155f-183">Níže je zběžný příklad jak načíst toocreate statistiku hello sestavily v tomto příkladu</span><span class="sxs-lookup"><span data-stu-id="e155f-183">Below is a quick example of how toocreate statistics on hello tabled loaded in this example</span></span>

<span data-ttu-id="e155f-184">Spusťte následující příkazy CREATE STATISTICS z na příkazovém řádku sqlcmd hello:</span><span class="sxs-lookup"><span data-stu-id="e155f-184">Execute hello following CREATE STATISTICS statements from a sqlcmd prompt:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a><span data-ttu-id="e155f-185">Export dat z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e155f-185">Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="e155f-186">V tomto kurzu vytvoříte z tabulky v SQL Data Warehouse datový soubor.</span><span class="sxs-lookup"><span data-stu-id="e155f-186">In this tutorial, you will create a data file from a table in SQL Data Warehouse.</span></span> <span data-ttu-id="e155f-187">Bude exportu hello data, která jsme vytvořili výše tooa nového datového souboru s názvem DimDate2_export.txt.</span><span class="sxs-lookup"><span data-stu-id="e155f-187">We will export hello data we created above tooa new data file called DimDate2_export.txt.</span></span>

### <a name="step-1-export-hello-data"></a><span data-ttu-id="e155f-188">Krok 1: Export dat hello</span><span class="sxs-lookup"><span data-stu-id="e155f-188">Step 1: Export hello data</span></span>
<span data-ttu-id="e155f-189">Hello nástroj bcp můžete připojit a vyexportovat data pomocí následující příkazu nahraďte hello hodnoty podle potřeby hello:</span><span class="sxs-lookup"><span data-stu-id="e155f-189">Using hello bcp utility, you can connect and export data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="e155f-190">Můžete ověřit hello data vyexportovala správně otevřením hello nový soubor.</span><span class="sxs-lookup"><span data-stu-id="e155f-190">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="e155f-191">Hello data v souboru hello shodovat následujícím textem hello:</span><span class="sxs-lookup"><span data-stu-id="e155f-191">hello data in hello file should match hello text below:</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> <span data-ttu-id="e155f-192">Z důvodu toohello povaze distribuovaných systémů nemusí být pořadí dat hello hello stejné napříč databázemi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e155f-192">Due toohello nature of distributed systems, hello data order may not be hello same across SQL Data Warehouse databases.</span></span> <span data-ttu-id="e155f-193">Další možností je toouse hello **queryout** funkce bcp toowrite dotazu extrahovat místo vyexportování celé tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e155f-193">Another option is toouse hello **queryout** function of bcp toowrite a query extract rather than export hello entire table.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="e155f-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e155f-194">Next steps</span></span>
<span data-ttu-id="e155f-195">Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][Load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="e155f-195">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="e155f-196">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="e155f-196">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
