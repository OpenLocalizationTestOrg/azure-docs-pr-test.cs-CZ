---
title: aaaLoad dat z SQL serveru do Azure SQL Data Warehouse (bcp) | Microsoft Docs
description: "Pro malou velikost dat využívá bcp tooexport data z tooflat soubory systému SQL Server a importovat data hello přímo do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: a03b5403d123e8814ae73a7cce8e6851c8b264a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a><span data-ttu-id="3d777-103">Načtení dat z SQL Serveru do Azure SQL Data Warehouse (ploché soubory)</span><span class="sxs-lookup"><span data-stu-id="3d777-103">Load data from SQL Server into Azure SQL Data Warehouse (flat files)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3d777-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="3d777-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="3d777-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="3d777-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="3d777-106">bcp</span><span class="sxs-lookup"><span data-stu-id="3d777-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="3d777-107">Pro malé datové sady můžete použít hello bcp nástroj příkazového řádku tooexport data z SQL serveru a pak můžete načíst přímo tooAzure SQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="3d777-107">For small data sets, you can use hello bcp command-line utility tooexport data from SQL Server and then load it directly tooAzure SQL Data Warehouse.</span></span>

<span data-ttu-id="3d777-108">V tomto kurzu použijete nástroj bcp k následujícím operacím:</span><span class="sxs-lookup"><span data-stu-id="3d777-108">In this tutorial, you will use bcp to:</span></span>

* <span data-ttu-id="3d777-109">Export tabulky z ze serveru SQL Server pomocí hello příkazu bcp out (nebo vytvoření jednoduchého ukázkového souboru)</span><span class="sxs-lookup"><span data-stu-id="3d777-109">Export a table from from SQL Server by using hello bcp out command (or create a simple sample file)</span></span>
* <span data-ttu-id="3d777-110">Importovat hello tabulky z plochého souboru tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="3d777-110">Import hello table from a flat file tooSQL Data Warehouse.</span></span>
* <span data-ttu-id="3d777-111">Vytvoření statistiky pro hello načíst data.</span><span class="sxs-lookup"><span data-stu-id="3d777-111">Create statistics on hello loaded data.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="3d777-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3d777-112">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="3d777-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3d777-113">Prerequisites</span></span>
<span data-ttu-id="3d777-114">toostep prostřednictvím tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3d777-114">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="3d777-115">Databázi SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3d777-115">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="3d777-116">Hello bcp nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="3d777-116">hello bcp command-line utility installed</span></span>
* <span data-ttu-id="3d777-117">Hello sqlcmd nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="3d777-117">hello sqlcmd command-line utility installed</span></span>

<span data-ttu-id="3d777-118">Hello nástroje bcp a sqlcmd si můžete stáhnout z hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="3d777-118">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="3d777-119">Data ve formátu ASCII nebo UTF-16</span><span class="sxs-lookup"><span data-stu-id="3d777-119">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="3d777-120">Pokud se tento kurz používáte svoje vlastní data, musí vaše data toouse hello ASCII nebo UTF-16 kódování, protože bcp nepodporuje kódování UTF-8.</span><span class="sxs-lookup"><span data-stu-id="3d777-120">If you are trying this tutorial with your own data, your data needs toouse hello ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

<span data-ttu-id="3d777-121">PolyBase podporuje UTF-8, ale zatím nepodporuje UTF-16.</span><span class="sxs-lookup"><span data-stu-id="3d777-121">PolyBase supports UTF-8 but doesn't yet support UTF-16.</span></span> <span data-ttu-id="3d777-122">Všimněte si, že pokud chcete, aby toocombine bcp pomocí funkce PolyBase budete potřebovat data hello tootransform tooUTF-8 po vyexportování z SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="3d777-122">Note that if you want toocombine bcp with PolyBase you will need tootransform hello data tooUTF-8 after it is exported from SQL Server.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="3d777-123">1. Vytvoření cílové tabulky</span><span class="sxs-lookup"><span data-stu-id="3d777-123">1. Create a destination table</span></span>
<span data-ttu-id="3d777-124">Definujte tabulky v SQL Data Warehouse, které se bude hello cílové tabulky pro zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-124">Define a table in SQL Data Warehouse that will be hello destination table for hello load.</span></span> <span data-ttu-id="3d777-125">Hello sloupců v tabulce hello musí odpovídat toohello dat v jednotlivých řádcích vašeho datového souboru.</span><span class="sxs-lookup"><span data-stu-id="3d777-125">hello columns in hello table must correspond toohello data in each row of your data file.</span></span>

<span data-ttu-id="3d777-126">toocreate tabulku, otevřete příkazový řádek a pomocí sqlcmd.exe toorun hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3d777-126">toocreate a table, open a command prompt and use sqlcmd.exe toorun hello following command:</span></span>

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


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="3d777-127">2. Vytvoření zdrojového datového souboru</span><span class="sxs-lookup"><span data-stu-id="3d777-127">2. Create a source data file</span></span>
<span data-ttu-id="3d777-128">Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového textového souboru a potom uložte tento soubor tooyour místního dočasného adresáře C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="3d777-128">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="3d777-129">Tato data jsou ve formátu ASCII.</span><span class="sxs-lookup"><span data-stu-id="3d777-129">This data is in ASCII format.</span></span>

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

<span data-ttu-id="3d777-130">(Volitelné) tooexport svoje vlastní data z databáze systému SQL Server, otevřete příkazový řádek a spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-130">(Optional) tooexport your own data from a SQL Server database, open a command prompt and run hello following command.</span></span> <span data-ttu-id="3d777-131">TableName, ServerName, DatabaseName, Username a Password nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="3d777-131">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-hello-data"></a><span data-ttu-id="3d777-132">3. Načtení dat hello</span><span class="sxs-lookup"><span data-stu-id="3d777-132">3. Load hello data</span></span>
<span data-ttu-id="3d777-133">tooload hello data, otevřete příkazový řádek a spusťte následující příkaz, nahraďte hello hodnoty pro název serveru, název databáze, uživatelské jméno a heslo s informacemi o sobě hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-133">tooload hello data, open a command prompt and run hello following command, replacing hello values for Server Name, Database name, Username, and Password with your own information.</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="3d777-134">Použijte tento příkaz tooverify hello data načetla správně.</span><span class="sxs-lookup"><span data-stu-id="3d777-134">Use this command tooverify hello data was loaded properly</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="3d777-135">výsledky Hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="3d777-135">hello results should look like this:</span></span>

| <span data-ttu-id="3d777-136">DateId</span><span class="sxs-lookup"><span data-stu-id="3d777-136">DateId</span></span> | <span data-ttu-id="3d777-137">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="3d777-137">CalendarQuarter</span></span> | <span data-ttu-id="3d777-138">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="3d777-138">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3d777-139">20150101</span><span class="sxs-lookup"><span data-stu-id="3d777-139">20150101</span></span> |<span data-ttu-id="3d777-140">1</span><span class="sxs-lookup"><span data-stu-id="3d777-140">1</span></span> |<span data-ttu-id="3d777-141">3</span><span class="sxs-lookup"><span data-stu-id="3d777-141">3</span></span> |
| <span data-ttu-id="3d777-142">20150201</span><span class="sxs-lookup"><span data-stu-id="3d777-142">20150201</span></span> |<span data-ttu-id="3d777-143">1</span><span class="sxs-lookup"><span data-stu-id="3d777-143">1</span></span> |<span data-ttu-id="3d777-144">3</span><span class="sxs-lookup"><span data-stu-id="3d777-144">3</span></span> |
| <span data-ttu-id="3d777-145">20150301</span><span class="sxs-lookup"><span data-stu-id="3d777-145">20150301</span></span> |<span data-ttu-id="3d777-146">1</span><span class="sxs-lookup"><span data-stu-id="3d777-146">1</span></span> |<span data-ttu-id="3d777-147">3</span><span class="sxs-lookup"><span data-stu-id="3d777-147">3</span></span> |
| <span data-ttu-id="3d777-148">20150401</span><span class="sxs-lookup"><span data-stu-id="3d777-148">20150401</span></span> |<span data-ttu-id="3d777-149">2</span><span class="sxs-lookup"><span data-stu-id="3d777-149">2</span></span> |<span data-ttu-id="3d777-150">4</span><span class="sxs-lookup"><span data-stu-id="3d777-150">4</span></span> |
| <span data-ttu-id="3d777-151">20150501</span><span class="sxs-lookup"><span data-stu-id="3d777-151">20150501</span></span> |<span data-ttu-id="3d777-152">2</span><span class="sxs-lookup"><span data-stu-id="3d777-152">2</span></span> |<span data-ttu-id="3d777-153">4</span><span class="sxs-lookup"><span data-stu-id="3d777-153">4</span></span> |
| <span data-ttu-id="3d777-154">20150601</span><span class="sxs-lookup"><span data-stu-id="3d777-154">20150601</span></span> |<span data-ttu-id="3d777-155">2</span><span class="sxs-lookup"><span data-stu-id="3d777-155">2</span></span> |<span data-ttu-id="3d777-156">4</span><span class="sxs-lookup"><span data-stu-id="3d777-156">4</span></span> |
| <span data-ttu-id="3d777-157">20150701</span><span class="sxs-lookup"><span data-stu-id="3d777-157">20150701</span></span> |<span data-ttu-id="3d777-158">3</span><span class="sxs-lookup"><span data-stu-id="3d777-158">3</span></span> |<span data-ttu-id="3d777-159">1</span><span class="sxs-lookup"><span data-stu-id="3d777-159">1</span></span> |
| <span data-ttu-id="3d777-160">20150801</span><span class="sxs-lookup"><span data-stu-id="3d777-160">20150801</span></span> |<span data-ttu-id="3d777-161">3</span><span class="sxs-lookup"><span data-stu-id="3d777-161">3</span></span> |<span data-ttu-id="3d777-162">1</span><span class="sxs-lookup"><span data-stu-id="3d777-162">1</span></span> |
| <span data-ttu-id="3d777-163">20150801</span><span class="sxs-lookup"><span data-stu-id="3d777-163">20150801</span></span> |<span data-ttu-id="3d777-164">3</span><span class="sxs-lookup"><span data-stu-id="3d777-164">3</span></span> |<span data-ttu-id="3d777-165">1</span><span class="sxs-lookup"><span data-stu-id="3d777-165">1</span></span> |
| <span data-ttu-id="3d777-166">20151001</span><span class="sxs-lookup"><span data-stu-id="3d777-166">20151001</span></span> |<span data-ttu-id="3d777-167">4</span><span class="sxs-lookup"><span data-stu-id="3d777-167">4</span></span> |<span data-ttu-id="3d777-168">2</span><span class="sxs-lookup"><span data-stu-id="3d777-168">2</span></span> |
| <span data-ttu-id="3d777-169">20151101</span><span class="sxs-lookup"><span data-stu-id="3d777-169">20151101</span></span> |<span data-ttu-id="3d777-170">4</span><span class="sxs-lookup"><span data-stu-id="3d777-170">4</span></span> |<span data-ttu-id="3d777-171">2</span><span class="sxs-lookup"><span data-stu-id="3d777-171">2</span></span> |
| <span data-ttu-id="3d777-172">20151201</span><span class="sxs-lookup"><span data-stu-id="3d777-172">20151201</span></span> |<span data-ttu-id="3d777-173">4</span><span class="sxs-lookup"><span data-stu-id="3d777-173">4</span></span> |<span data-ttu-id="3d777-174">2</span><span class="sxs-lookup"><span data-stu-id="3d777-174">2</span></span> |

## <a name="4-create-statistics"></a><span data-ttu-id="3d777-175">4. Vytvoření statistiky</span><span class="sxs-lookup"><span data-stu-id="3d777-175">4. Create statistics</span></span>
<span data-ttu-id="3d777-176">SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="3d777-176">SQL Data Warehouse does not yet support auto-create or auto-update statistics.</span></span> <span data-ttu-id="3d777-177">tooget hello nejlepší výkon dotazů, je důležité toocreate statistiky pro všechny sloupce všech tabulek po prvním načtením hello nebo po dojít k významné změny v datech hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-177">tooget hello best query performance, it's important toocreate statistics on all columns of all tables after hello first load or after any substantial changes occur in hello data.</span></span> <span data-ttu-id="3d777-178">Podrobné vysvětlení statistiky najdete v tématu [statistiky][Statistics].</span><span class="sxs-lookup"><span data-stu-id="3d777-178">For a detailed explanation of statistics, see [Statistics][Statistics].</span></span> 

<span data-ttu-id="3d777-179">Spusťte následující příkaz toocreate statistiky na nově načtenou tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-179">Run hello following command toocreate statistics on your newly loaded table.</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a><span data-ttu-id="3d777-180">5. Export dat z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3d777-180">5. Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="3d777-181">Cvičně si můžete vyexportovat hello dat, který jste právě načetli zpět z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3d777-181">For fun, you can export hello data that you just loaded back out of SQL Data Warehouse.</span></span>  <span data-ttu-id="3d777-182">příkaz tooexport Hello je přesně hello stejné jako Export z SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="3d777-182">hello command tooexport is exactly hello same as exporting from SQL Server.</span></span>

<span data-ttu-id="3d777-183">Je však rozdíl ve výsledcích hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-183">However, there is a difference in hello results.</span></span> <span data-ttu-id="3d777-184">Vzhledem k tomu, že hello data je uložená v distribuovaných umístěních v rámci SQL Data Warehouse, při exportu dat zapíše ho každý výpočetní uzel data toohello výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="3d777-184">Since hello data is stored in distributed locations within SQL Data Warehouse, when you export data each Compute node writes it data toohello output file.</span></span> <span data-ttu-id="3d777-185">pořadí Hello hello dat v hello výstupní soubor je pravděpodobně toobe liší od pořadí hello hello dat ve vstupním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-185">hello order of hello data in hello output file is likely toobe different than hello order of hello data in hello input file.</span></span>

### <a name="export-a-table-and-compare-exported-results"></a><span data-ttu-id="3d777-186">Export tabulky a porovnání vyexportovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="3d777-186">Export a table and compare exported results</span></span>
<span data-ttu-id="3d777-187">toosee hello exportovaná data, otevřete příkazový řádek a spusťte tento příkaz pomocí svých vlastních parametrů.</span><span class="sxs-lookup"><span data-stu-id="3d777-187">toosee hello exported data, open a command prompt and run this command using your own parameters.</span></span> <span data-ttu-id="3d777-188">ServerName je název hello Azure logického SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="3d777-188">ServerName is hello name of your Azure logical SQL Server.</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="3d777-189">Můžete ověřit hello data vyexportovala správně otevřením hello nový soubor.</span><span class="sxs-lookup"><span data-stu-id="3d777-189">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="3d777-190">Hello data v souboru hello shodovat následujícím textem hello, ale bude pravděpodobně uvedená v jiném pořadí:</span><span class="sxs-lookup"><span data-stu-id="3d777-190">hello data in hello file should match hello text below, but will likely be sorted in a different order:</span></span>

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

### <a name="export-hello-results-of-a-query"></a><span data-ttu-id="3d777-191">Export hello výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="3d777-191">Export hello results of a query</span></span>
<span data-ttu-id="3d777-192">Můžete použít hello **queryout** funkce bcp tooexport hello výsledků dotazu místo vyexportování celé tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="3d777-192">You can use hello **queryout** function of bcp tooexport hello results of a query instead of exporting hello entire table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3d777-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d777-193">Next steps</span></span>
<span data-ttu-id="3d777-194">Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][Load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="3d777-194">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="3d777-195">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="3d777-195">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="3d777-196">V tématu [tabulky přehled] [ Table Overview] nebo [syntaxe CREATE TABLE] [ CREATE TABLE syntax] Další informace o vytváření tabulek v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3d777-196">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse.</span></span>

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
