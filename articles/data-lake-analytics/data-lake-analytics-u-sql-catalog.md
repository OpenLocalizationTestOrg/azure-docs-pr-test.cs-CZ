---
title: "Začínáme s jazykem U-SQL catalog | Microsoft Docs"
description: "Další informace o použití katalogu U-SQL pro sdílení kódu a data."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: edmaca
ms.openlocfilehash: 08364c6c7bea53807844e3b1cc327dc3742e0487
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-u-sql-catalog"></a><span data-ttu-id="f5d45-103">Začínáme s katalogem U-SQL</span><span class="sxs-lookup"><span data-stu-id="f5d45-103">Get started with the U-SQL Catalog</span></span>

## <a name="create-a-tvf"></a><span data-ttu-id="f5d45-104">Vytvoření TVF</span><span class="sxs-lookup"><span data-stu-id="f5d45-104">Create a TVF</span></span>

<span data-ttu-id="f5d45-105">V předchozích skript U-SQL opakované použití EXTRAKCE číst ze stejné zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="f5d45-105">In the previous U-SQL script, you repeated the use of EXTRACT to read from the same source file.</span></span> <span data-ttu-id="f5d45-106">S U-SQL funkce vracející tabulku (TVF) může zapouzdřit data pro budoucí využití.</span><span class="sxs-lookup"><span data-stu-id="f5d45-106">With the U-SQL table-valued function (TVF), you can encapsulate the data for future reuse.</span></span>  

<span data-ttu-id="f5d45-107">Tento skript vytvoří TVF, názvem `Searchlog()` ve výchozí databázi a schéma:</span><span class="sxs-lookup"><span data-stu-id="f5d45-107">The following script creates a TVF called `Searchlog()` in the default database and schema:</span></span>

```
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
RETURN;
END;
```

<span data-ttu-id="f5d45-108">Tento skript je ukázkou, jak používat funkci TVF, která byla definována v předchozí skript:</span><span class="sxs-lookup"><span data-stu-id="f5d45-108">The following script shows you how to use the TVF that was defined in the previous script:</span></span>

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/SerachLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a><span data-ttu-id="f5d45-109">Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="f5d45-109">Create views</span></span>

<span data-ttu-id="f5d45-110">Pokud máte jeden dotaz výrazu, místo TVF můžete zobrazení U-SQL pro zapouzdření tento výraz.</span><span class="sxs-lookup"><span data-stu-id="f5d45-110">If you have a single query expression, instead of a TVF you can use a U-SQL VIEW to encapsulate that expression.</span></span>

<span data-ttu-id="f5d45-111">Tento skript vytvoří zobrazení s názvem `SearchlogView` ve výchozí databázi a schéma:</span><span class="sxs-lookup"><span data-stu-id="f5d45-111">The following script creates a view called `SearchlogView` in the default database and schema:</span></span>

```
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

<span data-ttu-id="f5d45-112">Následující skript demonstruje použití definované zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f5d45-112">The following script demonstrates the use of the defined view:</span></span>

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a><span data-ttu-id="f5d45-113">Vytváření tabulek</span><span class="sxs-lookup"><span data-stu-id="f5d45-113">Create tables</span></span>
<span data-ttu-id="f5d45-114">Jako s tabulkami relační databáze pomocí U-SQL můžete vytvořit tabulku s předdefinovaným schématem nebo vytvořit tabulku, která odvodí, že schéma z dotazu, který naplní tabulky (také označované jako CREATE TABLE AS SELECT nebo funkce CTAS).</span><span class="sxs-lookup"><span data-stu-id="f5d45-114">As with relational database tables, with U-SQL you can create a table with a predefined schema or create a table that infers the schema from the query that populates the table (also known as CREATE TABLE AS SELECT or CTAS).</span></span>

<span data-ttu-id="f5d45-115">Vytvořte databázi a dvě tabulky pomocí následující skript:</span><span class="sxs-lookup"><span data-stu-id="f5d45-115">Create a database and two tables by using the following script:</span></span>

```
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a><span data-ttu-id="f5d45-116">Dotazu na tabulky</span><span class="sxs-lookup"><span data-stu-id="f5d45-116">Query tables</span></span>
<span data-ttu-id="f5d45-117">Můžete dotazovat tabulkami, jako jsou ty, vytvořené v předchozí skript stejným způsobem dotazování datové soubory.</span><span class="sxs-lookup"><span data-stu-id="f5d45-117">You can query tables, such as those created in the previous script, in the same way that you query the data files.</span></span> <span data-ttu-id="f5d45-118">Místo vytvoření pomocí EXTRAKCE sadu řádků, teď můžete odkazovat na název tabulky.</span><span class="sxs-lookup"><span data-stu-id="f5d45-118">Instead of creating a rowset by using EXTRACT, you now can refer to the table name.</span></span>

<span data-ttu-id="f5d45-119">Čtení z tabulek, upravte transformace skript, který jste použili dříve:</span><span class="sxs-lookup"><span data-stu-id="f5d45-119">To read from the tables, modify the transform script that you used previously:</span></span>

```
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    TO "/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 ><span data-ttu-id="f5d45-120">S výběrem nelze v současné době spustit v tabulce ve skriptu stejný jako ten, na které jste vytvořili v tabulce.</span><span class="sxs-lookup"><span data-stu-id="f5d45-120">Currently, you cannot run a SELECT on a table in the same script as the one where you created the table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5d45-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5d45-121">Next Steps</span></span>
* [<span data-ttu-id="f5d45-122">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f5d45-122">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="f5d45-123">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5d45-123">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="f5d45-124">Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5d45-124">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
