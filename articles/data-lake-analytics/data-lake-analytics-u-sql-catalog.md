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
# <a name="get-started-with-the-u-sql-catalog"></a>Začínáme s katalogem U-SQL

## <a name="create-a-tvf"></a>Vytvoření TVF

V předchozích skript U-SQL opakované použití EXTRAKCE číst ze stejné zdrojového souboru. S U-SQL funkce vracející tabulku (TVF) může zapouzdřit data pro budoucí využití.  

Tento skript vytvoří TVF, názvem `Searchlog()` ve výchozí databázi a schéma:

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

Tento skript je ukázkou, jak používat funkci TVF, která byla definována v předchozí skript:

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

## <a name="create-views"></a>Vytvoření zobrazení

Pokud máte jeden dotaz výrazu, místo TVF můžete zobrazení U-SQL pro zapouzdření tento výraz.

Tento skript vytvoří zobrazení s názvem `SearchlogView` ve výchozí databázi a schéma:

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

Následující skript demonstruje použití definované zobrazení:

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

## <a name="create-tables"></a>Vytváření tabulek
Jako s tabulkami relační databáze pomocí U-SQL můžete vytvořit tabulku s předdefinovaným schématem nebo vytvořit tabulku, která odvodí, že schéma z dotazu, který naplní tabulky (také označované jako CREATE TABLE AS SELECT nebo funkce CTAS).

Vytvořte databázi a dvě tabulky pomocí následující skript:

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

## <a name="query-tables"></a>Dotazu na tabulky
Můžete dotazovat tabulkami, jako jsou ty, vytvořené v předchozí skript stejným způsobem dotazování datové soubory. Místo vytvoření pomocí EXTRAKCE sadu řádků, teď můžete odkazovat na název tabulky.

Čtení z tabulek, upravte transformace skript, který jste použili dříve:

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
 >S výběrem nelze v současné době spustit v tabulce ve skriptu stejný jako ten, na které jste vytvořili v tabulce.

## <a name="next-steps"></a>Další kroky
* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
