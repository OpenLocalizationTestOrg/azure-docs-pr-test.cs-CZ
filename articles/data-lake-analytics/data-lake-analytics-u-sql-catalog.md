---
title: "Začínáme s jazykem U-SQL hello catalog | Microsoft Docs"
description: "Zjistěte, jak toouse hello U-SQL katalogu tooshare kód a data."
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
ms.openlocfilehash: 559bb7a3879031eb290a3e82946d7bf42ac9f553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-u-sql-catalog"></a><span data-ttu-id="b5595-103">Začínáme s hello katalogu U-SQL</span><span class="sxs-lookup"><span data-stu-id="b5595-103">Get started with hello U-SQL Catalog</span></span>

## <a name="create-a-tvf"></a><span data-ttu-id="b5595-104">Vytvoření TVF</span><span class="sxs-lookup"><span data-stu-id="b5595-104">Create a TVF</span></span>

<span data-ttu-id="b5595-105">V hello předchozí skript U-SQL, opakované použití hello EXTRAKCE tooread z hello stejný zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="b5595-105">In hello previous U-SQL script, you repeated hello use of EXTRACT tooread from hello same source file.</span></span> <span data-ttu-id="b5595-106">Pomocí funkce vracející tabulku hello U-SQL (TVF) může zapouzdřit hello data pro budoucí využití.</span><span class="sxs-lookup"><span data-stu-id="b5595-106">With hello U-SQL table-valued function (TVF), you can encapsulate hello data for future reuse.</span></span>  

<span data-ttu-id="b5595-107">Hello následující skript vytvoří TVF, názvem `Searchlog()` v hello výchozí databázi a schéma:</span><span class="sxs-lookup"><span data-stu-id="b5595-107">hello following script creates a TVF called `Searchlog()` in hello default database and schema:</span></span>

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

<span data-ttu-id="b5595-108">Následující skript Hello ukazuje, jak toouse hello TVF, která byla definována v předchozí skript hello:</span><span class="sxs-lookup"><span data-stu-id="b5595-108">hello following script shows you how toouse hello TVF that was defined in hello previous script:</span></span>

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    too"/output/SerachLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a><span data-ttu-id="b5595-109">Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="b5595-109">Create views</span></span>

<span data-ttu-id="b5595-110">Pokud máte jeden dotaz výrazu, místo TVF můžete U-SQL zobrazení tooencapsulate tento výraz.</span><span class="sxs-lookup"><span data-stu-id="b5595-110">If you have a single query expression, instead of a TVF you can use a U-SQL VIEW tooencapsulate that expression.</span></span>

<span data-ttu-id="b5595-111">Hello následující skript vytvoří zobrazení s názvem `SearchlogView` v hello výchozí databázi a schéma:</span><span class="sxs-lookup"><span data-stu-id="b5595-111">hello following script creates a view called `SearchlogView` in hello default database and schema:</span></span>

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

<span data-ttu-id="b5595-112">Následující skript Hello ukazuje použití hello hello definované zobrazení:</span><span class="sxs-lookup"><span data-stu-id="b5595-112">hello following script demonstrates hello use of hello defined view:</span></span>

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    too"/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a><span data-ttu-id="b5595-113">Vytváření tabulek</span><span class="sxs-lookup"><span data-stu-id="b5595-113">Create tables</span></span>
<span data-ttu-id="b5595-114">Jako s tabulkami relační databáze pomocí U-SQL můžete vytvořit tabulku s předdefinovaným schématem nebo vytvořit tabulku, která odvodí hello schématu z hello dotazu, který naplní hello tabulku (také označované jako CREATE TABLE AS SELECT nebo funkce CTAS).</span><span class="sxs-lookup"><span data-stu-id="b5595-114">As with relational database tables, with U-SQL you can create a table with a predefined schema or create a table that infers hello schema from hello query that populates hello table (also known as CREATE TABLE AS SELECT or CTAS).</span></span>

<span data-ttu-id="b5595-115">Vytvořte databázi a dvě tabulky pomocí hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="b5595-115">Create a database and two tables by using hello following script:</span></span>

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

## <a name="query-tables"></a><span data-ttu-id="b5595-116">Dotazu na tabulky</span><span class="sxs-lookup"><span data-stu-id="b5595-116">Query tables</span></span>
<span data-ttu-id="b5595-117">Můžete zadat dotaz tabulkami, jako jsou ty, vytvořené v předchozí skript hello v hello stejným způsobem jako dotazování hello datových souborů.</span><span class="sxs-lookup"><span data-stu-id="b5595-117">You can query tables, such as those created in hello previous script, in hello same way that you query hello data files.</span></span> <span data-ttu-id="b5595-118">Místo vytváření sadu řádků s využitím EXTRAKCE, teď může odkazovat toohello název tabulky.</span><span class="sxs-lookup"><span data-stu-id="b5595-118">Instead of creating a rowset by using EXTRACT, you now can refer toohello table name.</span></span>

<span data-ttu-id="b5595-119">tooread z tabulek hello upravte hello transformace skript, který jste použili dříve:</span><span class="sxs-lookup"><span data-stu-id="b5595-119">tooread from hello tables, modify hello transform script that you used previously:</span></span>

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
    too"/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 ><span data-ttu-id="b5595-120">S výběrem nelze v současné době spustit v tabulce v hello stejný skript jako jeden hello kde jste vytvořili tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="b5595-120">Currently, you cannot run a SELECT on a table in hello same script as hello one where you created hello table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5595-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5595-121">Next Steps</span></span>
* [<span data-ttu-id="b5595-122">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b5595-122">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="b5595-123">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5595-123">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="b5595-124">Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b5595-124">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
