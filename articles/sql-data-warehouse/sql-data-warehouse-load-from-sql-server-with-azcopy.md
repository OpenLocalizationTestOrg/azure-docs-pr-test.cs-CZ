---
title: aaaLoad dat z SQL serveru do Azure SQL Data Warehouse (PolyBase) | Microsoft Docs
description: "Využívá bcp tooexport data z tooflat soubory systému SQL Server, úložiště objektů blob tooAzure data tooimport AZCopy a PolyBase tooingest hello data do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 89e2a91bc97642e9fc18545cb802b42d8dc4ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a><span data-ttu-id="6cb06-103">Načtení dat z SQL Serveru do Azure SQL Data Warehouse (AZCopy)</span><span class="sxs-lookup"><span data-stu-id="6cb06-103">Load data from SQL Server into Azure SQL Data Warehouse (AZCopy)</span></span>
<span data-ttu-id="6cb06-104">Pomocí bcp a AZCopy nástroje příkazového řádku tooload dat z úložiště objektů blob tooAzure systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6cb06-104">Use bcp and AZCopy command-line utilities tooload data from SQL Server tooAzure blob storage.</span></span> <span data-ttu-id="6cb06-105">Potom pomocí funkce PolyBase nebo Azure Data Factory tooload hello data do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6cb06-105">Then use PolyBase or Azure Data Factory tooload hello data into Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6cb06-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6cb06-106">Prerequisites</span></span>
<span data-ttu-id="6cb06-107">toostep prostřednictvím tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="6cb06-107">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="6cb06-108">Databázi SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6cb06-108">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="6cb06-109">nainstalovaný nástroj příkazového řádku bcp Hello</span><span class="sxs-lookup"><span data-stu-id="6cb06-109">hello bcp command line utility installed</span></span>
* <span data-ttu-id="6cb06-110">Hello nainstalovaný nástroj příkazového řádku SQLCMD</span><span class="sxs-lookup"><span data-stu-id="6cb06-110">hello SQLCMD command line utility installed</span></span>

> [!NOTE]
> <span data-ttu-id="6cb06-111">Hello nástroje bcp a sqlcmd si můžete stáhnout z hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="6cb06-111">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a><span data-ttu-id="6cb06-112">Import dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6cb06-112">Import data into SQL Data Warehouse</span></span>
<span data-ttu-id="6cb06-113">V tomto kurzu vytvoříte tabulku v Azure SQL Data Warehouse a importovat data do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="6cb06-113">In this tutorial, you will create a table in Azure SQL Data Warehouse and import data into hello table.</span></span>

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a><span data-ttu-id="6cb06-114">Krok 1: Vytvoření tabulky v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6cb06-114">Step 1: Create a table in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="6cb06-115">Z příkazového řádku použijte hello toorun sqlcmd následující dotaz toocreate tabulku ve vaší instanci:</span><span class="sxs-lookup"><span data-stu-id="6cb06-115">From a command prompt, use sqlcmd toorun hello following query toocreate a table on your instance:</span></span>

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
> <span data-ttu-id="6cb06-116">V tématu [tabulky přehled] [ Table Overview] nebo [syntaxe CREATE TABLE] [ CREATE TABLE syntax] Další informace o vytváření tabulek v SQL Data Warehouse a hello  Možnosti dostupné v klauzuli WITH hello.</span><span class="sxs-lookup"><span data-stu-id="6cb06-116">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse and hello  options available in hello WITH clause.</span></span>
> 
> 

### <a name="step-2-create-a-source-data-file"></a><span data-ttu-id="6cb06-117">Krok 2: Vytvoření zdrojového datového souboru</span><span class="sxs-lookup"><span data-stu-id="6cb06-117">Step 2: Create a source data file</span></span>
<span data-ttu-id="6cb06-118">Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového textového souboru a potom uložte tento soubor tooyour místního dočasného adresáře C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="6cb06-118">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span>

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
> <span data-ttu-id="6cb06-119">Je důležité tooremember této bcp.exe nepodporuje kódování souborů UTF-8 hello.</span><span class="sxs-lookup"><span data-stu-id="6cb06-119">It is important tooremember that bcp.exe does not support hello UTF-8 file encoding.</span></span> <span data-ttu-id="6cb06-120">Při použití nástroje bcp.exe prosím použijte soubory ASCII nebo UTF-16.</span><span class="sxs-lookup"><span data-stu-id="6cb06-120">Please use ASCII files or UTF-16 encoded files when using bcp.exe.</span></span>
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a><span data-ttu-id="6cb06-121">Krok 3: Připojení a import dat hello</span><span class="sxs-lookup"><span data-stu-id="6cb06-121">Step 3: Connect and import hello data</span></span>
<span data-ttu-id="6cb06-122">Při použití nástroje bcp můžete připojit a import dat hello pomocí následující příkazu nahraďte hello hodnoty podle potřeby hello:</span><span class="sxs-lookup"><span data-stu-id="6cb06-122">Using bcp, you can connect and import hello data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="6cb06-123">Můžete ověřit hello byla data načtena spuštěním následujícího dotazu pomocí sqlcmd hello:</span><span class="sxs-lookup"><span data-stu-id="6cb06-123">You can verify hello data was loaded by running hello following query using sqlcmd:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="6cb06-124">To by měla vrátit hello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="6cb06-124">This should return hello following results:</span></span>

| <span data-ttu-id="6cb06-125">DateId</span><span class="sxs-lookup"><span data-stu-id="6cb06-125">DateId</span></span> | <span data-ttu-id="6cb06-126">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="6cb06-126">CalendarQuarter</span></span> | <span data-ttu-id="6cb06-127">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="6cb06-127">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cb06-128">20150101</span><span class="sxs-lookup"><span data-stu-id="6cb06-128">20150101</span></span> |<span data-ttu-id="6cb06-129">1</span><span class="sxs-lookup"><span data-stu-id="6cb06-129">1</span></span> |<span data-ttu-id="6cb06-130">3</span><span class="sxs-lookup"><span data-stu-id="6cb06-130">3</span></span> |
| <span data-ttu-id="6cb06-131">20150201</span><span class="sxs-lookup"><span data-stu-id="6cb06-131">20150201</span></span> |<span data-ttu-id="6cb06-132">1</span><span class="sxs-lookup"><span data-stu-id="6cb06-132">1</span></span> |<span data-ttu-id="6cb06-133">3</span><span class="sxs-lookup"><span data-stu-id="6cb06-133">3</span></span> |
| <span data-ttu-id="6cb06-134">20150301</span><span class="sxs-lookup"><span data-stu-id="6cb06-134">20150301</span></span> |<span data-ttu-id="6cb06-135">1</span><span class="sxs-lookup"><span data-stu-id="6cb06-135">1</span></span> |<span data-ttu-id="6cb06-136">3</span><span class="sxs-lookup"><span data-stu-id="6cb06-136">3</span></span> |
| <span data-ttu-id="6cb06-137">20150401</span><span class="sxs-lookup"><span data-stu-id="6cb06-137">20150401</span></span> |<span data-ttu-id="6cb06-138">2</span><span class="sxs-lookup"><span data-stu-id="6cb06-138">2</span></span> |<span data-ttu-id="6cb06-139">4</span><span class="sxs-lookup"><span data-stu-id="6cb06-139">4</span></span> |
| <span data-ttu-id="6cb06-140">20150501</span><span class="sxs-lookup"><span data-stu-id="6cb06-140">20150501</span></span> |<span data-ttu-id="6cb06-141">2</span><span class="sxs-lookup"><span data-stu-id="6cb06-141">2</span></span> |<span data-ttu-id="6cb06-142">4</span><span class="sxs-lookup"><span data-stu-id="6cb06-142">4</span></span> |
| <span data-ttu-id="6cb06-143">20150601</span><span class="sxs-lookup"><span data-stu-id="6cb06-143">20150601</span></span> |<span data-ttu-id="6cb06-144">2</span><span class="sxs-lookup"><span data-stu-id="6cb06-144">2</span></span> |<span data-ttu-id="6cb06-145">4</span><span class="sxs-lookup"><span data-stu-id="6cb06-145">4</span></span> |
| <span data-ttu-id="6cb06-146">20150701</span><span class="sxs-lookup"><span data-stu-id="6cb06-146">20150701</span></span> |<span data-ttu-id="6cb06-147">3</span><span class="sxs-lookup"><span data-stu-id="6cb06-147">3</span></span> |<span data-ttu-id="6cb06-148">1</span><span class="sxs-lookup"><span data-stu-id="6cb06-148">1</span></span> |
| <span data-ttu-id="6cb06-149">20150801</span><span class="sxs-lookup"><span data-stu-id="6cb06-149">20150801</span></span> |<span data-ttu-id="6cb06-150">3</span><span class="sxs-lookup"><span data-stu-id="6cb06-150">3</span></span> |<span data-ttu-id="6cb06-151">1</span><span class="sxs-lookup"><span data-stu-id="6cb06-151">1</span></span> |
| <span data-ttu-id="6cb06-152">20150801</span><span class="sxs-lookup"><span data-stu-id="6cb06-152">20150801</span></span> |<span data-ttu-id="6cb06-153">3</span><span class="sxs-lookup"><span data-stu-id="6cb06-153">3</span></span> |<span data-ttu-id="6cb06-154">1</span><span class="sxs-lookup"><span data-stu-id="6cb06-154">1</span></span> |
| <span data-ttu-id="6cb06-155">20151001</span><span class="sxs-lookup"><span data-stu-id="6cb06-155">20151001</span></span> |<span data-ttu-id="6cb06-156">4</span><span class="sxs-lookup"><span data-stu-id="6cb06-156">4</span></span> |<span data-ttu-id="6cb06-157">2</span><span class="sxs-lookup"><span data-stu-id="6cb06-157">2</span></span> |
| <span data-ttu-id="6cb06-158">20151101</span><span class="sxs-lookup"><span data-stu-id="6cb06-158">20151101</span></span> |<span data-ttu-id="6cb06-159">4</span><span class="sxs-lookup"><span data-stu-id="6cb06-159">4</span></span> |<span data-ttu-id="6cb06-160">2</span><span class="sxs-lookup"><span data-stu-id="6cb06-160">2</span></span> |
| <span data-ttu-id="6cb06-161">20151201</span><span class="sxs-lookup"><span data-stu-id="6cb06-161">20151201</span></span> |<span data-ttu-id="6cb06-162">4</span><span class="sxs-lookup"><span data-stu-id="6cb06-162">4</span></span> |<span data-ttu-id="6cb06-163">2</span><span class="sxs-lookup"><span data-stu-id="6cb06-163">2</span></span> |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="6cb06-164">Krok 4: Vytvoření statistiky pro vaše nově načtená data</span><span class="sxs-lookup"><span data-stu-id="6cb06-164">Step 4: Create Statistics on your newly loaded data</span></span>
<span data-ttu-id="6cb06-165">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="6cb06-165">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span> <span data-ttu-id="6cb06-166">V pořadí tooget hello nejlepší výkon ze své dotazy je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením hello nebo dojít k významné změny v datech hello.</span><span class="sxs-lookup"><span data-stu-id="6cb06-166">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span> <span data-ttu-id="6cb06-167">Podrobné vysvětlení statistiky najdete v tématu hello [statistiky] [ Statistics] tématu ve skupině témat věnovaných vývoji hello.</span><span class="sxs-lookup"><span data-stu-id="6cb06-167">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span> <span data-ttu-id="6cb06-168">Níže je zběžný příklad jak načíst toocreate statistiku hello sestavily v tomto příkladu</span><span class="sxs-lookup"><span data-stu-id="6cb06-168">Below is a quick example of how toocreate statistics on hello tabled loaded in this example</span></span>

<span data-ttu-id="6cb06-169">Spusťte následující příkazy CREATE STATISTICS z na příkazovém řádku sqlcmd hello:</span><span class="sxs-lookup"><span data-stu-id="6cb06-169">Execute hello following CREATE STATISTICS statements from a sqlcmd prompt:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a><span data-ttu-id="6cb06-170">Export dat z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6cb06-170">Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="6cb06-171">V tomto kurzu vytvoříte z tabulky v SQL Data Warehouse datový soubor.</span><span class="sxs-lookup"><span data-stu-id="6cb06-171">In this tutorial, you will create a data file from a table in SQL Data Warehouse.</span></span> <span data-ttu-id="6cb06-172">Bude exportu hello data, která jsme vytvořili výše tooa nového datového souboru s názvem DimDate2_export.txt.</span><span class="sxs-lookup"><span data-stu-id="6cb06-172">We will export hello data we created above tooa new data file called DimDate2_export.txt.</span></span>

### <a name="step-1-export-hello-data"></a><span data-ttu-id="6cb06-173">Krok 1: Export dat hello</span><span class="sxs-lookup"><span data-stu-id="6cb06-173">Step 1: Export hello data</span></span>
<span data-ttu-id="6cb06-174">Hello nástroj bcp můžete připojit a vyexportovat data pomocí následující příkazu nahraďte hello hodnoty podle potřeby hello:</span><span class="sxs-lookup"><span data-stu-id="6cb06-174">Using hello bcp utility, you can connect and export data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="6cb06-175">Můžete ověřit hello data vyexportovala správně otevřením hello nový soubor.</span><span class="sxs-lookup"><span data-stu-id="6cb06-175">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="6cb06-176">Hello data v souboru hello shodovat následujícím textem hello:</span><span class="sxs-lookup"><span data-stu-id="6cb06-176">hello data in hello file should match hello text below:</span></span>

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
> <span data-ttu-id="6cb06-177">Z důvodu toohello povaze distribuovaných systémů nemusí být pořadí dat hello hello stejné napříč databázemi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6cb06-177">Due toohello nature of distributed systems, hello data order may not be hello same across SQL Data Warehouse databases.</span></span> <span data-ttu-id="6cb06-178">Další možností je toouse hello **queryout** funkce bcp toowrite dotazu extrahovat místo vyexportování celé tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="6cb06-178">Another option is toouse hello **queryout** function of bcp toowrite a query extract rather than export hello entire table.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6cb06-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cb06-179">Next steps</span></span>
<span data-ttu-id="6cb06-180">Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][Load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="6cb06-180">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="6cb06-181">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="6cb06-181">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
