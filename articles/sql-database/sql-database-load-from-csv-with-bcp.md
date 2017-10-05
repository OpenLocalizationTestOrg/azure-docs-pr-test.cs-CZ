---
title: "Načtení dat ze souboru CSV do Azure SQL Database (bcp) | Microsoft Docs"
description: "Pro malá množství dat se k importu dat do databáze SQL Azure používá bcp."
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 84bebab7763bb21f73880a6c8b367a62b0c137d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a><span data-ttu-id="2363d-103">Načtení dat ze souboru CSV do Azure SQL Database (ploché soubory)</span><span class="sxs-lookup"><span data-stu-id="2363d-103">Load data from CSV into Azure SQL Database (flat files)</span></span>
<span data-ttu-id="2363d-104">Nástroj příkazového řádku bcp můžete použít k importu dat ze souboru CSV do databáze Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2363d-104">You can use the bcp command-line utility to import data from a CSV file into Azure SQL Database.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2363d-105">Než začnete</span><span class="sxs-lookup"><span data-stu-id="2363d-105">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="2363d-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2363d-106">Prerequisites</span></span>
<span data-ttu-id="2363d-107">Chcete-li provést kroky v tomto článku, je třeba:</span><span class="sxs-lookup"><span data-stu-id="2363d-107">To complete the steps in this article, you need:</span></span>

* <span data-ttu-id="2363d-108">Logický server a databáze Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2363d-108">An Azure SQL Database logical server and database</span></span>
* <span data-ttu-id="2363d-109">Nainstalovaný nástroj příkazového řádku bcp</span><span class="sxs-lookup"><span data-stu-id="2363d-109">The bcp command-line utility installed</span></span>
* <span data-ttu-id="2363d-110">Nainstalovaný nástroj příkazového řádku sqlcmd</span><span class="sxs-lookup"><span data-stu-id="2363d-110">The sqlcmd command-line utility installed</span></span>

<span data-ttu-id="2363d-111">Nástroje bcp a sqlcmd si můžete stáhnout z webu [Stažení softwaru společnosti Microsoft][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="2363d-111">You can download the bcp and sqlcmd utilities from the [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="2363d-112">Data ve formátu ASCII nebo UTF-16</span><span class="sxs-lookup"><span data-stu-id="2363d-112">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="2363d-113">Pokud pro tento kurz používáte svoje vlastní data, musí vaše data používat kódování ASCII nebo UTF-16, protože bcp nepodporuje kódování UTF-8.</span><span class="sxs-lookup"><span data-stu-id="2363d-113">If you are trying this tutorial with your own data, your data needs to use the ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="2363d-114">1. Vytvoření cílové tabulky</span><span class="sxs-lookup"><span data-stu-id="2363d-114">1. Create a destination table</span></span>
<span data-ttu-id="2363d-115">Definujte tabulku ve službě SQL Database jako cílovou tabulku.</span><span class="sxs-lookup"><span data-stu-id="2363d-115">Define a table in SQL Database as the destination table.</span></span> <span data-ttu-id="2363d-116">Sloupce v tabulce musí odpovídat datům v jednotlivých řádcích vašeho datového souboru.</span><span class="sxs-lookup"><span data-stu-id="2363d-116">The columns in the table must correspond to the data in each row of your data file.</span></span>

<span data-ttu-id="2363d-117">Pokud chcete vytvořit tabulku, otevřete okno příkazového řádku a pomocí sqlcmd.exe spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2363d-117">To create a table, open a command prompt and use sqlcmd.exe to run the following command:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="2363d-118">2. Vytvoření zdrojového datového souboru</span><span class="sxs-lookup"><span data-stu-id="2363d-118">2. Create a source data file</span></span>
<span data-ttu-id="2363d-119">Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového textového souboru. Pak tento soubor uložte do místního dočasného adresáře C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="2363d-119">Open Notepad and copy the following lines of data into a new text file and then save this file to your local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="2363d-120">Tato data jsou ve formátu ASCII.</span><span class="sxs-lookup"><span data-stu-id="2363d-120">This data is in ASCII format.</span></span>

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

<span data-ttu-id="2363d-121">(Volitelné) Pokud chcete z databáze SQL Serveru vyexportovat svoje vlastní data, otevřete příkazový řádek a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="2363d-121">(Optional) To export your own data from a SQL Server database, open a command prompt and run the following command.</span></span> <span data-ttu-id="2363d-122">TableName, ServerName, DatabaseName, Username a Password nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="2363d-122">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-the-data"></a><span data-ttu-id="2363d-123">3. Načtení dat</span><span class="sxs-lookup"><span data-stu-id="2363d-123">3. Load the data</span></span>
<span data-ttu-id="2363d-124">Pokud chcete načíst data, otevřete příkazový řádek a spusťte následující příkaz, přičemž hodnoty parametrů Server Name (Název serveru), Database name (Název databáze), Username (Uživatelské jméno) a Password (Heslo) nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="2363d-124">To load the data, open a command prompt and run the following command, replacing the values for Server Name, Database name, Username, and Password with your own information.</span></span>

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

<span data-ttu-id="2363d-125">Pomocí tohoto příkazu ověřte, že se data načetla správně.</span><span class="sxs-lookup"><span data-stu-id="2363d-125">Use this command to verify the data was loaded properly</span></span>

```bcp
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="2363d-126">Výsledky by měly vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2363d-126">The results should look like this:</span></span>

| <span data-ttu-id="2363d-127">DateId</span><span class="sxs-lookup"><span data-stu-id="2363d-127">DateId</span></span> | <span data-ttu-id="2363d-128">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="2363d-128">CalendarQuarter</span></span> | <span data-ttu-id="2363d-129">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="2363d-129">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2363d-130">20150101</span><span class="sxs-lookup"><span data-stu-id="2363d-130">20150101</span></span> |<span data-ttu-id="2363d-131">1</span><span class="sxs-lookup"><span data-stu-id="2363d-131">1</span></span> |<span data-ttu-id="2363d-132">3</span><span class="sxs-lookup"><span data-stu-id="2363d-132">3</span></span> |
| <span data-ttu-id="2363d-133">20150201</span><span class="sxs-lookup"><span data-stu-id="2363d-133">20150201</span></span> |<span data-ttu-id="2363d-134">1</span><span class="sxs-lookup"><span data-stu-id="2363d-134">1</span></span> |<span data-ttu-id="2363d-135">3</span><span class="sxs-lookup"><span data-stu-id="2363d-135">3</span></span> |
| <span data-ttu-id="2363d-136">20150301</span><span class="sxs-lookup"><span data-stu-id="2363d-136">20150301</span></span> |<span data-ttu-id="2363d-137">1</span><span class="sxs-lookup"><span data-stu-id="2363d-137">1</span></span> |<span data-ttu-id="2363d-138">3</span><span class="sxs-lookup"><span data-stu-id="2363d-138">3</span></span> |
| <span data-ttu-id="2363d-139">20150401</span><span class="sxs-lookup"><span data-stu-id="2363d-139">20150401</span></span> |<span data-ttu-id="2363d-140">2</span><span class="sxs-lookup"><span data-stu-id="2363d-140">2</span></span> |<span data-ttu-id="2363d-141">4</span><span class="sxs-lookup"><span data-stu-id="2363d-141">4</span></span> |
| <span data-ttu-id="2363d-142">20150501</span><span class="sxs-lookup"><span data-stu-id="2363d-142">20150501</span></span> |<span data-ttu-id="2363d-143">2</span><span class="sxs-lookup"><span data-stu-id="2363d-143">2</span></span> |<span data-ttu-id="2363d-144">4</span><span class="sxs-lookup"><span data-stu-id="2363d-144">4</span></span> |
| <span data-ttu-id="2363d-145">20150601</span><span class="sxs-lookup"><span data-stu-id="2363d-145">20150601</span></span> |<span data-ttu-id="2363d-146">2</span><span class="sxs-lookup"><span data-stu-id="2363d-146">2</span></span> |<span data-ttu-id="2363d-147">4</span><span class="sxs-lookup"><span data-stu-id="2363d-147">4</span></span> |
| <span data-ttu-id="2363d-148">20150701</span><span class="sxs-lookup"><span data-stu-id="2363d-148">20150701</span></span> |<span data-ttu-id="2363d-149">3</span><span class="sxs-lookup"><span data-stu-id="2363d-149">3</span></span> |<span data-ttu-id="2363d-150">1</span><span class="sxs-lookup"><span data-stu-id="2363d-150">1</span></span> |
| <span data-ttu-id="2363d-151">20150801</span><span class="sxs-lookup"><span data-stu-id="2363d-151">20150801</span></span> |<span data-ttu-id="2363d-152">3</span><span class="sxs-lookup"><span data-stu-id="2363d-152">3</span></span> |<span data-ttu-id="2363d-153">1</span><span class="sxs-lookup"><span data-stu-id="2363d-153">1</span></span> |
| <span data-ttu-id="2363d-154">20150801</span><span class="sxs-lookup"><span data-stu-id="2363d-154">20150801</span></span> |<span data-ttu-id="2363d-155">3</span><span class="sxs-lookup"><span data-stu-id="2363d-155">3</span></span> |<span data-ttu-id="2363d-156">1</span><span class="sxs-lookup"><span data-stu-id="2363d-156">1</span></span> |
| <span data-ttu-id="2363d-157">20151001</span><span class="sxs-lookup"><span data-stu-id="2363d-157">20151001</span></span> |<span data-ttu-id="2363d-158">4</span><span class="sxs-lookup"><span data-stu-id="2363d-158">4</span></span> |<span data-ttu-id="2363d-159">2</span><span class="sxs-lookup"><span data-stu-id="2363d-159">2</span></span> |
| <span data-ttu-id="2363d-160">20151101</span><span class="sxs-lookup"><span data-stu-id="2363d-160">20151101</span></span> |<span data-ttu-id="2363d-161">4</span><span class="sxs-lookup"><span data-stu-id="2363d-161">4</span></span> |<span data-ttu-id="2363d-162">2</span><span class="sxs-lookup"><span data-stu-id="2363d-162">2</span></span> |
| <span data-ttu-id="2363d-163">20151201</span><span class="sxs-lookup"><span data-stu-id="2363d-163">20151201</span></span> |<span data-ttu-id="2363d-164">4</span><span class="sxs-lookup"><span data-stu-id="2363d-164">4</span></span> |<span data-ttu-id="2363d-165">2</span><span class="sxs-lookup"><span data-stu-id="2363d-165">2</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2363d-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2363d-166">Next steps</span></span>
<span data-ttu-id="2363d-167">Postup migrace databáze serveru SQL Server naleznete v části [Migrace databáze serveru SQL Server](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="2363d-167">To migrate a SQL Server database, see [SQL Server database migration](sql-database-cloud-migrate.md).</span></span>

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
