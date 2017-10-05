---
title: "Načtení dat z SQL serveru do Azure SQL Data Warehouse (bcp) | Microsoft Docs"
description: "Pro malou velikost dat využívá bcp k exportu data z SQL Serveru do plochých souborů a k importu dat přímo do Azure SQL Data Warehouse."
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
ms.openlocfilehash: dae7b5f7456f4ec0daf60d55f9c38b780896ff83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a><span data-ttu-id="01843-103">Načtení dat z SQL Serveru do Azure SQL Data Warehouse (ploché soubory)</span><span class="sxs-lookup"><span data-stu-id="01843-103">Load data from SQL Server into Azure SQL Data Warehouse (flat files)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="01843-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="01843-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="01843-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="01843-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="01843-106">bcp</span><span class="sxs-lookup"><span data-stu-id="01843-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="01843-107">Pro malé datové sady můžete použít nástroj příkazového řádku bcp k exportu dat z SQL Serveru, které pak můžete načíst přímo do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01843-107">For small data sets, you can use the bcp command-line utility to export data from SQL Server and then load it directly to Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="01843-108">V tomto kurzu použijete nástroj bcp k následujícím operacím:</span><span class="sxs-lookup"><span data-stu-id="01843-108">In this tutorial, you will use bcp to:</span></span>

* <span data-ttu-id="01843-109">Export tabulky z SQL Serveru pomocí příkazu bcp out (nebo vytvoření jednoduchého ukázkového souboru)</span><span class="sxs-lookup"><span data-stu-id="01843-109">Export a table from from SQL Server by using the bcp out command (or create a simple sample file)</span></span>
* <span data-ttu-id="01843-110">Import tabulky z plochého souboru do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01843-110">Import the table from a flat file to SQL Data Warehouse.</span></span>
* <span data-ttu-id="01843-111">Vytvoření statistiky pro načtená data</span><span class="sxs-lookup"><span data-stu-id="01843-111">Create statistics on the loaded data.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="01843-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="01843-112">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="01843-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01843-113">Prerequisites</span></span>
<span data-ttu-id="01843-114">Pro jednotlivé kroky v tomto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="01843-114">To step through this tutorial, you need:</span></span>

* <span data-ttu-id="01843-115">Databázi SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01843-115">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="01843-116">Nainstalovaný nástroj příkazového řádku bcp</span><span class="sxs-lookup"><span data-stu-id="01843-116">The bcp command-line utility installed</span></span>
* <span data-ttu-id="01843-117">Nainstalovaný nástroj příkazového řádku sqlcmd</span><span class="sxs-lookup"><span data-stu-id="01843-117">The sqlcmd command-line utility installed</span></span>

<span data-ttu-id="01843-118">Nástroje bcp a sqlcmd si můžete stáhnout z webu [Stažení softwaru společnosti Microsoft][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="01843-118">You can download the bcp and sqlcmd utilities from the [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="01843-119">Data ve formátu ASCII nebo UTF-16</span><span class="sxs-lookup"><span data-stu-id="01843-119">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="01843-120">Pokud pro tento kurz používáte svoje vlastní data, musí vaše data používat kódování ASCII nebo UTF-16, protože bcp nepodporuje kódování UTF-8.</span><span class="sxs-lookup"><span data-stu-id="01843-120">If you are trying this tutorial with your own data, your data needs to use the ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

<span data-ttu-id="01843-121">PolyBase podporuje UTF-8, ale zatím nepodporuje UTF-16.</span><span class="sxs-lookup"><span data-stu-id="01843-121">PolyBase supports UTF-8 but doesn't yet support UTF-16.</span></span> <span data-ttu-id="01843-122">Je potřeba upozornit na to, že pokud chcete kombinovat používání nástroje s funkcí PolyBase, budete muset data po vyexportování z SQL Serveru převést na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="01843-122">Note that if you want to combine bcp with PolyBase you will need to transform the data to UTF-8 after it is exported from SQL Server.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="01843-123">1. Vytvoření cílové tabulky</span><span class="sxs-lookup"><span data-stu-id="01843-123">1. Create a destination table</span></span>
<span data-ttu-id="01843-124">Definujte v SQL Data Warehouse tabulku, která bude cílovou tabulkou pro načtení.</span><span class="sxs-lookup"><span data-stu-id="01843-124">Define a table in SQL Data Warehouse that will be the destination table for the load.</span></span> <span data-ttu-id="01843-125">Sloupce v tabulce musí odpovídat datům v jednotlivých řádcích vašeho datového souboru.</span><span class="sxs-lookup"><span data-stu-id="01843-125">The columns in the table must correspond to the data in each row of your data file.</span></span>

<span data-ttu-id="01843-126">Pokud chcete vytvořit tabulku, otevřete okno příkazového řádku a pomocí sqlcmd.exe spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="01843-126">To create a table, open a command prompt and use sqlcmd.exe to run the following command:</span></span>

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


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="01843-127">2. Vytvoření zdrojového datového souboru</span><span class="sxs-lookup"><span data-stu-id="01843-127">2. Create a source data file</span></span>
<span data-ttu-id="01843-128">Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového textového souboru. Pak tento soubor uložte do místního dočasného adresáře C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="01843-128">Open Notepad and copy the following lines of data into a new text file and then save this file to your local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="01843-129">Tato data jsou ve formátu ASCII.</span><span class="sxs-lookup"><span data-stu-id="01843-129">This data is in ASCII format.</span></span>

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

<span data-ttu-id="01843-130">(Volitelné) Pokud chcete z databáze SQL Serveru vyexportovat svoje vlastní data, otevřete příkazový řádek a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="01843-130">(Optional) To export your own data from a SQL Server database, open a command prompt and run the following command.</span></span> <span data-ttu-id="01843-131">TableName, ServerName, DatabaseName, Username a Password nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="01843-131">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a><span data-ttu-id="01843-132">3. Načtení dat</span><span class="sxs-lookup"><span data-stu-id="01843-132">3. Load the data</span></span>
<span data-ttu-id="01843-133">Pokud chcete načíst data, otevřete příkazový řádek a spusťte následující příkaz, přičemž hodnoty parametrů Server Name (Název serveru), Database name (Název databáze), Username (Uživatelské jméno) a Password (Heslo) nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="01843-133">To load the data, open a command prompt and run the following command, replacing the values for Server Name, Database name, Username, and Password with your own information.</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="01843-134">Pomocí tohoto příkazu ověřte, že se data načetla správně.</span><span class="sxs-lookup"><span data-stu-id="01843-134">Use this command to verify the data was loaded properly</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="01843-135">Výsledky by měly vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="01843-135">The results should look like this:</span></span>

| <span data-ttu-id="01843-136">DateId</span><span class="sxs-lookup"><span data-stu-id="01843-136">DateId</span></span> | <span data-ttu-id="01843-137">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="01843-137">CalendarQuarter</span></span> | <span data-ttu-id="01843-138">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="01843-138">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="01843-139">20150101</span><span class="sxs-lookup"><span data-stu-id="01843-139">20150101</span></span> |<span data-ttu-id="01843-140">1</span><span class="sxs-lookup"><span data-stu-id="01843-140">1</span></span> |<span data-ttu-id="01843-141">3</span><span class="sxs-lookup"><span data-stu-id="01843-141">3</span></span> |
| <span data-ttu-id="01843-142">20150201</span><span class="sxs-lookup"><span data-stu-id="01843-142">20150201</span></span> |<span data-ttu-id="01843-143">1</span><span class="sxs-lookup"><span data-stu-id="01843-143">1</span></span> |<span data-ttu-id="01843-144">3</span><span class="sxs-lookup"><span data-stu-id="01843-144">3</span></span> |
| <span data-ttu-id="01843-145">20150301</span><span class="sxs-lookup"><span data-stu-id="01843-145">20150301</span></span> |<span data-ttu-id="01843-146">1</span><span class="sxs-lookup"><span data-stu-id="01843-146">1</span></span> |<span data-ttu-id="01843-147">3</span><span class="sxs-lookup"><span data-stu-id="01843-147">3</span></span> |
| <span data-ttu-id="01843-148">20150401</span><span class="sxs-lookup"><span data-stu-id="01843-148">20150401</span></span> |<span data-ttu-id="01843-149">2</span><span class="sxs-lookup"><span data-stu-id="01843-149">2</span></span> |<span data-ttu-id="01843-150">4</span><span class="sxs-lookup"><span data-stu-id="01843-150">4</span></span> |
| <span data-ttu-id="01843-151">20150501</span><span class="sxs-lookup"><span data-stu-id="01843-151">20150501</span></span> |<span data-ttu-id="01843-152">2</span><span class="sxs-lookup"><span data-stu-id="01843-152">2</span></span> |<span data-ttu-id="01843-153">4</span><span class="sxs-lookup"><span data-stu-id="01843-153">4</span></span> |
| <span data-ttu-id="01843-154">20150601</span><span class="sxs-lookup"><span data-stu-id="01843-154">20150601</span></span> |<span data-ttu-id="01843-155">2</span><span class="sxs-lookup"><span data-stu-id="01843-155">2</span></span> |<span data-ttu-id="01843-156">4</span><span class="sxs-lookup"><span data-stu-id="01843-156">4</span></span> |
| <span data-ttu-id="01843-157">20150701</span><span class="sxs-lookup"><span data-stu-id="01843-157">20150701</span></span> |<span data-ttu-id="01843-158">3</span><span class="sxs-lookup"><span data-stu-id="01843-158">3</span></span> |<span data-ttu-id="01843-159">1</span><span class="sxs-lookup"><span data-stu-id="01843-159">1</span></span> |
| <span data-ttu-id="01843-160">20150801</span><span class="sxs-lookup"><span data-stu-id="01843-160">20150801</span></span> |<span data-ttu-id="01843-161">3</span><span class="sxs-lookup"><span data-stu-id="01843-161">3</span></span> |<span data-ttu-id="01843-162">1</span><span class="sxs-lookup"><span data-stu-id="01843-162">1</span></span> |
| <span data-ttu-id="01843-163">20150801</span><span class="sxs-lookup"><span data-stu-id="01843-163">20150801</span></span> |<span data-ttu-id="01843-164">3</span><span class="sxs-lookup"><span data-stu-id="01843-164">3</span></span> |<span data-ttu-id="01843-165">1</span><span class="sxs-lookup"><span data-stu-id="01843-165">1</span></span> |
| <span data-ttu-id="01843-166">20151001</span><span class="sxs-lookup"><span data-stu-id="01843-166">20151001</span></span> |<span data-ttu-id="01843-167">4</span><span class="sxs-lookup"><span data-stu-id="01843-167">4</span></span> |<span data-ttu-id="01843-168">2</span><span class="sxs-lookup"><span data-stu-id="01843-168">2</span></span> |
| <span data-ttu-id="01843-169">20151101</span><span class="sxs-lookup"><span data-stu-id="01843-169">20151101</span></span> |<span data-ttu-id="01843-170">4</span><span class="sxs-lookup"><span data-stu-id="01843-170">4</span></span> |<span data-ttu-id="01843-171">2</span><span class="sxs-lookup"><span data-stu-id="01843-171">2</span></span> |
| <span data-ttu-id="01843-172">20151201</span><span class="sxs-lookup"><span data-stu-id="01843-172">20151201</span></span> |<span data-ttu-id="01843-173">4</span><span class="sxs-lookup"><span data-stu-id="01843-173">4</span></span> |<span data-ttu-id="01843-174">2</span><span class="sxs-lookup"><span data-stu-id="01843-174">2</span></span> |

## <a name="4-create-statistics"></a><span data-ttu-id="01843-175">4. Vytvoření statistiky</span><span class="sxs-lookup"><span data-stu-id="01843-175">4. Create statistics</span></span>
<span data-ttu-id="01843-176">SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="01843-176">SQL Data Warehouse does not yet support auto-create or auto-update statistics.</span></span> <span data-ttu-id="01843-177">Aby vám dotazy vracely co nejlepší výsledky, je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením nebo kdykoli, kdy v datech dojde k zásadnějším změnám.</span><span class="sxs-lookup"><span data-stu-id="01843-177">To get the best query performance, it's important to create statistics on all columns of all tables after the first load or after any substantial changes occur in the data.</span></span> <span data-ttu-id="01843-178">Podrobné vysvětlení statistiky najdete v tématu [statistiky][Statistics].</span><span class="sxs-lookup"><span data-stu-id="01843-178">For a detailed explanation of statistics, see [Statistics][Statistics].</span></span> 

<span data-ttu-id="01843-179">Spuštěním následujícího příkazu vytvořte statistiku pro nově načtenou tabulku.</span><span class="sxs-lookup"><span data-stu-id="01843-179">Run the following command to create statistics on your newly loaded table.</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a><span data-ttu-id="01843-180">5. Export dat z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01843-180">5. Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="01843-181">Cvičně si můžete data, která jste právě načetli, vyexportovat zpět z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01843-181">For fun, you can export the data that you just loaded back out of SQL Data Warehouse.</span></span>  <span data-ttu-id="01843-182">Příkaz pro export je úplně stejný jako příkaz pro export z SQL Serveru.</span><span class="sxs-lookup"><span data-stu-id="01843-182">The command to export is exactly the same as exporting from SQL Server.</span></span>

<span data-ttu-id="01843-183">Rozdíl je ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="01843-183">However, there is a difference in the results.</span></span> <span data-ttu-id="01843-184">Vzhledem k tomu, že jsou data v rámci SQL Data Warehouse uložená v distribuovaných umístěních, při exportu dat zapíše data do výstupního souboru každý uzel Výpočty.</span><span class="sxs-lookup"><span data-stu-id="01843-184">Since the data is stored in distributed locations within SQL Data Warehouse, when you export data each Compute node writes it data to the output file.</span></span> <span data-ttu-id="01843-185">Pořadí dat ve výstupním souboru bude s velkou pravděpodobností jiné než pořadí dat ve vstupním souboru.</span><span class="sxs-lookup"><span data-stu-id="01843-185">The order of the data in the output file is likely to be different than the order of the data in the input file.</span></span>

### <a name="export-a-table-and-compare-exported-results"></a><span data-ttu-id="01843-186">Export tabulky a porovnání vyexportovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="01843-186">Export a table and compare exported results</span></span>
<span data-ttu-id="01843-187">Pokud si budete chtít vyexportovaná data zobrazit, otevřete okno příkazového řádku a spustíte tento příkaz pomocí svých vlastních parametrů.</span><span class="sxs-lookup"><span data-stu-id="01843-187">To see the exported data, open a command prompt and run this command using your own parameters.</span></span> <span data-ttu-id="01843-188">ServerName je název vašeho logického SQL serveru Azure.</span><span class="sxs-lookup"><span data-stu-id="01843-188">ServerName is the name of your Azure logical SQL Server.</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="01843-189">To, že se data vyexportovala správně, můžete ověřit tak, že nový soubor otevřete.</span><span class="sxs-lookup"><span data-stu-id="01843-189">You can verify the data was exported correctly by opening the new file.</span></span> <span data-ttu-id="01843-190">Data v souboru by se měla shodovat s následujícím textem, budou ale pravděpodobně uvedená v jiném pořadí:</span><span class="sxs-lookup"><span data-stu-id="01843-190">The data in the file should match the text below, but will likely be sorted in a different order:</span></span>

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

### <a name="export-the-results-of-a-query"></a><span data-ttu-id="01843-191">Export výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="01843-191">Export the results of a query</span></span>
<span data-ttu-id="01843-192">Místo vyexportování celé tabulky můžete také pomocí funkce **queryout** nástroje bcp vyexportovat jenom výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="01843-192">You can use the **queryout** function of bcp to export the results of a query instead of exporting the entire table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="01843-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01843-193">Next steps</span></span>
<span data-ttu-id="01843-194">Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][Load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="01843-194">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="01843-195">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="01843-195">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="01843-196">V tématu [tabulky přehled] [ Table Overview] nebo [syntaxe CREATE TABLE] [ CREATE TABLE syntax] Další informace o vytváření tabulek v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01843-196">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse.</span></span>

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
