---
title: "aaaLoad ukázková data do SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 3459c42f3aae51c27fd35db7874faf99e1e577e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a><span data-ttu-id="db5c1-103">Načtení ukázkových dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="db5c1-103">Load sample data into SQL Data Warehouse</span></span>
<span data-ttu-id="db5c1-104">Postupujte podle těchto tooload jednoduchých kroků a ukázkové společnosti Adventure Works hello dotaz do databáze.</span><span class="sxs-lookup"><span data-stu-id="db5c1-104">Follow these simple steps tooload and query hello Adventure Works Sample database.</span></span> <span data-ttu-id="db5c1-105">Tyto skripty se nejprve pomocí sqlcmd toorun SQL, která bude vytvářet tabulky a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="db5c1-105">These scripts first use sqlcmd toorun SQL which will create tables and views.</span></span> <span data-ttu-id="db5c1-106">Po vytvoření tabulky hello skripty bcp tooload data používat.</span><span class="sxs-lookup"><span data-stu-id="db5c1-106">Once tables have been created, hello scripts will use bcp tooload data.</span></span>  <span data-ttu-id="db5c1-107">Pokud ještě nemáte sqlcmd a bcp nainstalována, použijte tyto odkazy příliš[nainstalovat bcp] [ install bcp] a příliš[nainstalovat sqlcmd][install sqlcmd].</span><span class="sxs-lookup"><span data-stu-id="db5c1-107">If you don't already have sqlcmd and bcp installed, follow these links too[install bcp][install bcp] and too[install sqlcmd][install sqlcmd].</span></span>

## <a name="load-sample-data"></a><span data-ttu-id="db5c1-108">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="db5c1-108">Load sample data</span></span>
1. <span data-ttu-id="db5c1-109">Stáhnout hello [společnosti Adventure Works ukázkové skripty pro SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] soubor zip.</span><span class="sxs-lookup"><span data-stu-id="db5c1-109">Download hello [Adventure Works Sample Scripts for SQL Data Warehouse][Adventure Works Sample Scripts for SQL Data Warehouse] zip file.</span></span>
2. <span data-ttu-id="db5c1-110">Extrahujte soubory hello z adresáře tooa zip staženého v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="db5c1-110">Extract hello files from downloaded zip tooa directory on your local machine.</span></span>
3. <span data-ttu-id="db5c1-111">Upravte hello extrahovat soubor aw_create.bat a nastavte následující proměnné nalezen hello horní části souboru hello hello.</span><span class="sxs-lookup"><span data-stu-id="db5c1-111">Edit hello extracted file aw_create.bat and set hello following variables found at hello top of hello file.</span></span>  <span data-ttu-id="db5c1-112">Být jisti tooleave žádné mezery mezi hello "=" a parametr hello.</span><span class="sxs-lookup"><span data-stu-id="db5c1-112">Be sure tooleave no whitespace between hello "=" and hello parameter.</span></span>  <span data-ttu-id="db5c1-113">Níže jsou příklady, jak může vypadat provedené úpravy.</span><span class="sxs-lookup"><span data-stu-id="db5c1-113">Below are examples of how your edits might look.</span></span>
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. <span data-ttu-id="db5c1-114">Do příkazového řádku systému Windows spusťte aw_create.bat hello upravit.</span><span class="sxs-lookup"><span data-stu-id="db5c1-114">From a Windows cmd prompt, run hello edited aw_create.bat.</span></span>  <span data-ttu-id="db5c1-115">Ujistěte se, že jste v hello adresáře, kam jste uložili vašeho upravenou verzi aw_create.bat.</span><span class="sxs-lookup"><span data-stu-id="db5c1-115">Be sure you are in hello directory where you saved your edited version of aw_create.bat.</span></span>
   <span data-ttu-id="db5c1-116">Tento skript bude...</span><span class="sxs-lookup"><span data-stu-id="db5c1-116">This script will...</span></span>
   
   * <span data-ttu-id="db5c1-117">Vyřaďte společnosti Adventure Works tabulky ani zobrazení, které již existují ve vaší databázi</span><span class="sxs-lookup"><span data-stu-id="db5c1-117">Drop any Adventure Works tables or views that already exist in your database</span></span>
   * <span data-ttu-id="db5c1-118">Vytvoření hello společnosti Adventure Works tabulek a zobrazení</span><span class="sxs-lookup"><span data-stu-id="db5c1-118">Create hello Adventure Works tables and views</span></span>
   * <span data-ttu-id="db5c1-119">Načíst každá tabulka společnosti Adventure Works pomocí bcp</span><span class="sxs-lookup"><span data-stu-id="db5c1-119">Load each Adventure Works table using bcp</span></span>
   * <span data-ttu-id="db5c1-120">Ověření hello počty řádků pro každou tabulku společnosti Adventure Works</span><span class="sxs-lookup"><span data-stu-id="db5c1-120">Validate hello row counts for each Adventure Works table</span></span>
   * <span data-ttu-id="db5c1-121">Shromáždit statistiku pro každý sloupec pro každou tabulku společnosti Adventure Works</span><span class="sxs-lookup"><span data-stu-id="db5c1-121">Collect statistics on every column for each Adventure Works table</span></span>

## <a name="query-sample-data"></a><span data-ttu-id="db5c1-122">Dotaz na ukázková data</span><span class="sxs-lookup"><span data-stu-id="db5c1-122">Query sample data</span></span>
<span data-ttu-id="db5c1-123">Jakmile jste načíst ukázková data do SQL Data Warehouse, můžete rychle spustit pár dotazů.</span><span class="sxs-lookup"><span data-stu-id="db5c1-123">Once you've loaded some sample data into your SQL Data Warehouse, you can quickly run a few queries.</span></span>  <span data-ttu-id="db5c1-124">toorun dotazu, připojení databáze společnosti Adventure Works tooyour nově vytvořený v Azure SQL DW pomocí sady Visual Studio a rozšíření SSDT, jak je popsáno v hello [dotazu pomocí sady Visual Studio] [ query with Visual Studio] dokumentu.</span><span class="sxs-lookup"><span data-stu-id="db5c1-124">toorun a query, connect tooyour newly created Adventure Works database in Azure SQL DW using Visual Studio and SSDT, as described in hello [query with Visual Studio][query with Visual Studio] document.</span></span>

<span data-ttu-id="db5c1-125">Příklad použití jednoduchých vyberte příkaz tooget všechny údaje hello hello zaměstnanců:</span><span class="sxs-lookup"><span data-stu-id="db5c1-125">Example of simple select statement tooget all hello info of hello employees:</span></span>

```sql
SELECT * FROM DimEmployee;
```

<span data-ttu-id="db5c1-126">Příklad komplexnější dotaz pomocí konstruktory, jako jsou toolook GROUP BY v hello celkovou velikost pro všechny prodeje na každý den:</span><span class="sxs-lookup"><span data-stu-id="db5c1-126">Example of a more complex query using constructs such as GROUP BY toolook at hello total amount for all sales on each day:</span></span>

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="db5c1-127">Příklad výběru se toofilter klauzule WHERE se objednávky z do určitého data:</span><span class="sxs-lookup"><span data-stu-id="db5c1-127">Example of a SELECT with a WHERE clause toofilter out orders from before a certain date:</span></span>

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="db5c1-128">SQL Data Warehouse podporuje téměř všechny konstrukce T-SQL, které systém SQL Server podporuje.</span><span class="sxs-lookup"><span data-stu-id="db5c1-128">SQL Data Warehouse supports almost all T-SQL constructs which SQL Server supports.</span></span>  <span data-ttu-id="db5c1-129">Případné rozdíly jsou popsány v našem [migrace kódu] [ migrate code] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="db5c1-129">Any differences are documented in our [migrate code][migrate code] documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db5c1-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db5c1-130">Next steps</span></span>
<span data-ttu-id="db5c1-131">Teď, když využijete prvního tootry několik dotazů s ukázkovými daty, podívejte se jak příliš[vyvíjet][develop], [načíst][load], nebo [ migrace] [ migrate] tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="db5c1-131">Now that you've had a chance tootry some queries with sample data, check out how too[develop][develop], [load][load], or [migrate][migrate] tooSQL Data Warehouse.</span></span>

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
