---
title: soubor aaaLoad data ze souboru CSV do Azure SQL Database (bcp) | Microsoft Docs
description: "Pro malou velikost dat využívá bcp tooimport data do Azure SQL Database."
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
ms.openlocfilehash: 9350e459aa844223820fbbd849a830cf0354d4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a><span data-ttu-id="79d5a-103">Načtení dat ze souboru CSV do Azure SQL Database (ploché soubory)</span><span class="sxs-lookup"><span data-stu-id="79d5a-103">Load data from CSV into Azure SQL Database (flat files)</span></span>
<span data-ttu-id="79d5a-104">Můžete data tooimport nástroj příkazového řádku bcp hello ze souboru CSV do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="79d5a-104">You can use hello bcp command-line utility tooimport data from a CSV file into Azure SQL Database.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="79d5a-105">Než začnete</span><span class="sxs-lookup"><span data-stu-id="79d5a-105">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="79d5a-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79d5a-106">Prerequisites</span></span>
<span data-ttu-id="79d5a-107">toocomplete hello kroky v tomto článku, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="79d5a-107">toocomplete hello steps in this article, you need:</span></span>

* <span data-ttu-id="79d5a-108">Logický server a databáze Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="79d5a-108">An Azure SQL Database logical server and database</span></span>
* <span data-ttu-id="79d5a-109">Hello bcp nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="79d5a-109">hello bcp command-line utility installed</span></span>
* <span data-ttu-id="79d5a-110">Hello sqlcmd nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="79d5a-110">hello sqlcmd command-line utility installed</span></span>

<span data-ttu-id="79d5a-111">Hello nástroje bcp a sqlcmd si můžete stáhnout z hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="79d5a-111">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="79d5a-112">Data ve formátu ASCII nebo UTF-16</span><span class="sxs-lookup"><span data-stu-id="79d5a-112">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="79d5a-113">Pokud se tento kurz používáte svoje vlastní data, musí vaše data toouse hello ASCII nebo UTF-16 kódování, protože bcp nepodporuje kódování UTF-8.</span><span class="sxs-lookup"><span data-stu-id="79d5a-113">If you are trying this tutorial with your own data, your data needs toouse hello ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="79d5a-114">1. Vytvoření cílové tabulky</span><span class="sxs-lookup"><span data-stu-id="79d5a-114">1. Create a destination table</span></span>
<span data-ttu-id="79d5a-115">Definujte tabulku v databázi SQL jako hello cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="79d5a-115">Define a table in SQL Database as hello destination table.</span></span> <span data-ttu-id="79d5a-116">Hello sloupců v tabulce hello musí odpovídat toohello dat v jednotlivých řádcích vašeho datového souboru.</span><span class="sxs-lookup"><span data-stu-id="79d5a-116">hello columns in hello table must correspond toohello data in each row of your data file.</span></span>

<span data-ttu-id="79d5a-117">toocreate tabulku, otevřete příkazový řádek a pomocí sqlcmd.exe toorun hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="79d5a-117">toocreate a table, open a command prompt and use sqlcmd.exe toorun hello following command:</span></span>

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


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="79d5a-118">2. Vytvoření zdrojového datového souboru</span><span class="sxs-lookup"><span data-stu-id="79d5a-118">2. Create a source data file</span></span>
<span data-ttu-id="79d5a-119">Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového textového souboru a potom uložte tento soubor tooyour místního dočasného adresáře C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="79d5a-119">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="79d5a-120">Tato data jsou ve formátu ASCII.</span><span class="sxs-lookup"><span data-stu-id="79d5a-120">This data is in ASCII format.</span></span>

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

<span data-ttu-id="79d5a-121">(Volitelné) tooexport svoje vlastní data z databáze systému SQL Server, otevřete příkazový řádek a spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="79d5a-121">(Optional) tooexport your own data from a SQL Server database, open a command prompt and run hello following command.</span></span> <span data-ttu-id="79d5a-122">TableName, ServerName, DatabaseName, Username a Password nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="79d5a-122">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-hello-data"></a><span data-ttu-id="79d5a-123">3. Načtení dat hello</span><span class="sxs-lookup"><span data-stu-id="79d5a-123">3. Load hello data</span></span>
<span data-ttu-id="79d5a-124">tooload hello data, otevřete příkazový řádek a spusťte následující příkaz, nahraďte hello hodnoty pro název serveru, název databáze, uživatelské jméno a heslo s informacemi o sobě hello.</span><span class="sxs-lookup"><span data-stu-id="79d5a-124">tooload hello data, open a command prompt and run hello following command, replacing hello values for Server Name, Database name, Username, and Password with your own information.</span></span>

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

<span data-ttu-id="79d5a-125">Použijte tento příkaz tooverify hello data načetla správně.</span><span class="sxs-lookup"><span data-stu-id="79d5a-125">Use this command tooverify hello data was loaded properly</span></span>

```bcp
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="79d5a-126">výsledky Hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="79d5a-126">hello results should look like this:</span></span>

| <span data-ttu-id="79d5a-127">DateId</span><span class="sxs-lookup"><span data-stu-id="79d5a-127">DateId</span></span> | <span data-ttu-id="79d5a-128">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="79d5a-128">CalendarQuarter</span></span> | <span data-ttu-id="79d5a-129">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="79d5a-129">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="79d5a-130">20150101</span><span class="sxs-lookup"><span data-stu-id="79d5a-130">20150101</span></span> |<span data-ttu-id="79d5a-131">1</span><span class="sxs-lookup"><span data-stu-id="79d5a-131">1</span></span> |<span data-ttu-id="79d5a-132">3</span><span class="sxs-lookup"><span data-stu-id="79d5a-132">3</span></span> |
| <span data-ttu-id="79d5a-133">20150201</span><span class="sxs-lookup"><span data-stu-id="79d5a-133">20150201</span></span> |<span data-ttu-id="79d5a-134">1</span><span class="sxs-lookup"><span data-stu-id="79d5a-134">1</span></span> |<span data-ttu-id="79d5a-135">3</span><span class="sxs-lookup"><span data-stu-id="79d5a-135">3</span></span> |
| <span data-ttu-id="79d5a-136">20150301</span><span class="sxs-lookup"><span data-stu-id="79d5a-136">20150301</span></span> |<span data-ttu-id="79d5a-137">1</span><span class="sxs-lookup"><span data-stu-id="79d5a-137">1</span></span> |<span data-ttu-id="79d5a-138">3</span><span class="sxs-lookup"><span data-stu-id="79d5a-138">3</span></span> |
| <span data-ttu-id="79d5a-139">20150401</span><span class="sxs-lookup"><span data-stu-id="79d5a-139">20150401</span></span> |<span data-ttu-id="79d5a-140">2</span><span class="sxs-lookup"><span data-stu-id="79d5a-140">2</span></span> |<span data-ttu-id="79d5a-141">4</span><span class="sxs-lookup"><span data-stu-id="79d5a-141">4</span></span> |
| <span data-ttu-id="79d5a-142">20150501</span><span class="sxs-lookup"><span data-stu-id="79d5a-142">20150501</span></span> |<span data-ttu-id="79d5a-143">2</span><span class="sxs-lookup"><span data-stu-id="79d5a-143">2</span></span> |<span data-ttu-id="79d5a-144">4</span><span class="sxs-lookup"><span data-stu-id="79d5a-144">4</span></span> |
| <span data-ttu-id="79d5a-145">20150601</span><span class="sxs-lookup"><span data-stu-id="79d5a-145">20150601</span></span> |<span data-ttu-id="79d5a-146">2</span><span class="sxs-lookup"><span data-stu-id="79d5a-146">2</span></span> |<span data-ttu-id="79d5a-147">4</span><span class="sxs-lookup"><span data-stu-id="79d5a-147">4</span></span> |
| <span data-ttu-id="79d5a-148">20150701</span><span class="sxs-lookup"><span data-stu-id="79d5a-148">20150701</span></span> |<span data-ttu-id="79d5a-149">3</span><span class="sxs-lookup"><span data-stu-id="79d5a-149">3</span></span> |<span data-ttu-id="79d5a-150">1</span><span class="sxs-lookup"><span data-stu-id="79d5a-150">1</span></span> |
| <span data-ttu-id="79d5a-151">20150801</span><span class="sxs-lookup"><span data-stu-id="79d5a-151">20150801</span></span> |<span data-ttu-id="79d5a-152">3</span><span class="sxs-lookup"><span data-stu-id="79d5a-152">3</span></span> |<span data-ttu-id="79d5a-153">1</span><span class="sxs-lookup"><span data-stu-id="79d5a-153">1</span></span> |
| <span data-ttu-id="79d5a-154">20150801</span><span class="sxs-lookup"><span data-stu-id="79d5a-154">20150801</span></span> |<span data-ttu-id="79d5a-155">3</span><span class="sxs-lookup"><span data-stu-id="79d5a-155">3</span></span> |<span data-ttu-id="79d5a-156">1</span><span class="sxs-lookup"><span data-stu-id="79d5a-156">1</span></span> |
| <span data-ttu-id="79d5a-157">20151001</span><span class="sxs-lookup"><span data-stu-id="79d5a-157">20151001</span></span> |<span data-ttu-id="79d5a-158">4</span><span class="sxs-lookup"><span data-stu-id="79d5a-158">4</span></span> |<span data-ttu-id="79d5a-159">2</span><span class="sxs-lookup"><span data-stu-id="79d5a-159">2</span></span> |
| <span data-ttu-id="79d5a-160">20151101</span><span class="sxs-lookup"><span data-stu-id="79d5a-160">20151101</span></span> |<span data-ttu-id="79d5a-161">4</span><span class="sxs-lookup"><span data-stu-id="79d5a-161">4</span></span> |<span data-ttu-id="79d5a-162">2</span><span class="sxs-lookup"><span data-stu-id="79d5a-162">2</span></span> |
| <span data-ttu-id="79d5a-163">20151201</span><span class="sxs-lookup"><span data-stu-id="79d5a-163">20151201</span></span> |<span data-ttu-id="79d5a-164">4</span><span class="sxs-lookup"><span data-stu-id="79d5a-164">4</span></span> |<span data-ttu-id="79d5a-165">2</span><span class="sxs-lookup"><span data-stu-id="79d5a-165">2</span></span> |

## <a name="next-steps"></a><span data-ttu-id="79d5a-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79d5a-166">Next steps</span></span>
<span data-ttu-id="79d5a-167">toomigrate databázi systému SQL Server najdete v části [migrace databáze SQL serveru](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="79d5a-167">toomigrate a SQL Server database, see [SQL Server database migration](sql-database-cloud-migrate.md).</span></span>

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
