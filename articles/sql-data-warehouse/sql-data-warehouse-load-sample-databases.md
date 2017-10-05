---
title: "Načtení ukázkových dat do SQL Data Warehouse | Microsoft Docs"
description: "Načtení ukázkových dat do SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1e0df958a2f18fe1e988168918e5cfd293f84e64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a><span data-ttu-id="bf6a7-103">Načtení ukázkových dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="bf6a7-103">Load sample data into SQL Data Warehouse</span></span>
<span data-ttu-id="bf6a7-104">Postupujte podle těchto jednoduchých kroků se načíst a dotaz na databázi společnosti Adventure Works ukázka.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-104">Follow these simple steps to load and query the Adventure Works Sample database.</span></span> <span data-ttu-id="bf6a7-105">Tyto skripty nejprve pomocí sqlcmd spustit SQL, která bude vytvářet tabulky a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-105">These scripts first use sqlcmd to run SQL which will create tables and views.</span></span> <span data-ttu-id="bf6a7-106">Po vytvoření tabulky skripty pomocí bcp načíst data.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-106">Once tables have been created, the scripts will use bcp to load data.</span></span>  <span data-ttu-id="bf6a7-107">Pokud ještě nemáte sqlcmd a bcp nainstalovaný, postupujte podle následujících odkazech na [nainstalovat bcp] [ install bcp] a [nainstalovat sqlcmd][install sqlcmd].</span><span class="sxs-lookup"><span data-stu-id="bf6a7-107">If you don't already have sqlcmd and bcp installed, follow these links to [install bcp][install bcp] and to [install sqlcmd][install sqlcmd].</span></span>

## <a name="load-sample-data"></a><span data-ttu-id="bf6a7-108">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="bf6a7-108">Load sample data</span></span>
1. <span data-ttu-id="bf6a7-109">Stažení [společnosti Adventure Works ukázkové skripty pro SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] soubor zip.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-109">Download the [Adventure Works Sample Scripts for SQL Data Warehouse][Adventure Works Sample Scripts for SQL Data Warehouse] zip file.</span></span>
2. <span data-ttu-id="bf6a7-110">Extrahujte soubory ze staženého zip do adresáře na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-110">Extract the files from downloaded zip to a directory on your local machine.</span></span>
3. <span data-ttu-id="bf6a7-111">Upravte aw_create.bat extrahovaných souborů a nastavte následující proměnné v horní části souboru nalezen.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-111">Edit the extracted file aw_create.bat and set the following variables found at the top of the file.</span></span>  <span data-ttu-id="bf6a7-112">Ponechejte žádné mezery mezi "=" a parametr.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-112">Be sure to leave no whitespace between the "=" and the parameter.</span></span>  <span data-ttu-id="bf6a7-113">Níže jsou příklady, jak může vypadat provedené úpravy.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-113">Below are examples of how your edits might look.</span></span>
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. <span data-ttu-id="bf6a7-114">Do příkazového řádku systému Windows spusťte upravená aw_create.bat.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-114">From a Windows cmd prompt, run the edited aw_create.bat.</span></span>  <span data-ttu-id="bf6a7-115">Ujistěte se, že jsou v adresáři, kam jste uložili vašeho upravenou verzi aw_create.bat.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-115">Be sure you are in the directory where you saved your edited version of aw_create.bat.</span></span>
   <span data-ttu-id="bf6a7-116">Tento skript bude...</span><span class="sxs-lookup"><span data-stu-id="bf6a7-116">This script will...</span></span>
   
   * <span data-ttu-id="bf6a7-117">Vyřaďte společnosti Adventure Works tabulky ani zobrazení, které již existují ve vaší databázi</span><span class="sxs-lookup"><span data-stu-id="bf6a7-117">Drop any Adventure Works tables or views that already exist in your database</span></span>
   * <span data-ttu-id="bf6a7-118">Vytvoření společnosti Adventure Works tabulky a zobrazení</span><span class="sxs-lookup"><span data-stu-id="bf6a7-118">Create the Adventure Works tables and views</span></span>
   * <span data-ttu-id="bf6a7-119">Načíst každá tabulka společnosti Adventure Works pomocí bcp</span><span class="sxs-lookup"><span data-stu-id="bf6a7-119">Load each Adventure Works table using bcp</span></span>
   * <span data-ttu-id="bf6a7-120">Ověření počtu řádků pro každou tabulku společnosti Adventure Works</span><span class="sxs-lookup"><span data-stu-id="bf6a7-120">Validate the row counts for each Adventure Works table</span></span>
   * <span data-ttu-id="bf6a7-121">Shromáždit statistiku pro každý sloupec pro každou tabulku společnosti Adventure Works</span><span class="sxs-lookup"><span data-stu-id="bf6a7-121">Collect statistics on every column for each Adventure Works table</span></span>

## <a name="query-sample-data"></a><span data-ttu-id="bf6a7-122">Dotaz na ukázková data</span><span class="sxs-lookup"><span data-stu-id="bf6a7-122">Query sample data</span></span>
<span data-ttu-id="bf6a7-123">Jakmile jste načíst ukázková data do SQL Data Warehouse, můžete rychle spustit pár dotazů.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-123">Once you've loaded some sample data into your SQL Data Warehouse, you can quickly run a few queries.</span></span>  <span data-ttu-id="bf6a7-124">Ke spuštění dotazu, připojit k nově vytvořenou databázi společnosti Adventure Works v Azure SQL DW pomocí sady Visual Studio a rozšíření SSDT, jak je popsáno v [dotazu pomocí sady Visual Studio] [ query with Visual Studio] dokumentu.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-124">To run a query, connect to your newly created Adventure Works database in Azure SQL DW using Visual Studio and SSDT, as described in the [query with Visual Studio][query with Visual Studio] document.</span></span>

<span data-ttu-id="bf6a7-125">Příklad jednoduchého příkazu select se získat informace o zaměstnanci:</span><span class="sxs-lookup"><span data-stu-id="bf6a7-125">Example of simple select statement to get all the info of the employees:</span></span>

```sql
SELECT * FROM DimEmployee;
```

<span data-ttu-id="bf6a7-126">Příklad komplexnější dotaz pomocí konstruktory, jako jsou GROUP BY se podívat na celkovou velikost pro všechny prodeje na každý den:</span><span class="sxs-lookup"><span data-stu-id="bf6a7-126">Example of a more complex query using constructs such as GROUP BY to look at the total amount for all sales on each day:</span></span>

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="bf6a7-127">Příklad SELECT s klauzulí WHERE filtrovat objednávky z do určitého data:</span><span class="sxs-lookup"><span data-stu-id="bf6a7-127">Example of a SELECT with a WHERE clause to filter out orders from before a certain date:</span></span>

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="bf6a7-128">SQL Data Warehouse podporuje téměř všechny konstrukce T-SQL, které systém SQL Server podporuje.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-128">SQL Data Warehouse supports almost all T-SQL constructs which SQL Server supports.</span></span>  <span data-ttu-id="bf6a7-129">Případné rozdíly jsou popsány v našem [migrace kódu] [ migrate code] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-129">Any differences are documented in our [migrate code][migrate code] documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf6a7-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf6a7-130">Next steps</span></span>
<span data-ttu-id="bf6a7-131">Teď, když vám, že možnost vyzkoušet několik dotazů s ukázkovými daty, podívejte se na postup [vyvíjet][develop], [načíst][load], nebo [ migrace] [ migrate] do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bf6a7-131">Now that you've had a chance to try some queries with sample data, check out how to [develop][develop], [load][load], or [migrate][migrate] to SQL Data Warehouse.</span></span>

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
