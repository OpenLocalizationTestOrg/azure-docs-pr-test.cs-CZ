---
title: "Začínáme s jazykem U-SQL | Microsoft Docs"
description: "Přečtěte si základy hello hello jazykem U-SQL."
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
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 5edee0e0d85211e84b3d47895c53d71f0a19f083
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-u-sql"></a>Začínáme s jazykem U-SQL
U-SQL je jazyk, který kombinuje deklarativní SQL s imperativní C# toolet při zpracování dat v jakémkoli měřítku. Prostřednictvím hello škálovatelné a distribuovaných dotazů funkce U-SQL, které můžete efektivně analyzovat data napříč relační úložiště, jako je například Azure SQL Database. Pomocí U-SQL můžete zpracovat nestrukturovaných dat tak, že použití schématu na čtení a vložení vlastní logiky a funkcí UDF. Navíc U-SQL zahrnuje rozšíření, která poskytuje jemně odstupňovanou kontrolu nad jak tooexecute ve velkém měřítku. 

## <a name="learning-resources"></a>Studijní materiály

* Hello [U-SQL kurzu](http://aka.ms/usqltutorial) poskytuje návod s asistencí pro většinu jazyka hello U-SQL. Tento dokument se doporučuje čtení pro všechny vývojáře, kteří požadují toolearn U-SQL.
* Podrobné informace o hello **syntaxe jazyka U-SQL**, najdete v části hello [referenční příručka jazyka U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).
* toounderstand hello **filosofie návrhu U-SQL**, najdete v příspěvku blogu Visual Studio hello [představení U-SQL – A jazyk, který usnadňuje Big zpracování dat](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Požadavky

Než přejdete prostřednictvím hello U-SQL ukázky v tomto dokumentu, přečtěte si a dokončete [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Tento kurz vysvětluje hello mechanismů U-SQL pomocí nástrojů Azure Data Lake pro Visual Studio.

## <a name="your-first-u-sql-script"></a>Váš první skript U-SQL

Hello následující skript U-SQL je snadná a umožňují nám prozkoumat mnoho jazyk aspekty hello U-SQL.

```
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

OUTPUT @searchlog   
    too"/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

Tento skript nemá žádné kroky transformace. Přečte z hello zdrojového souboru s názvem `SearchLog.tsv`, schematizes ho a zapíše hello řádků zpět do souboru s názvem SearchLog-first-u – sql.csv.

Všimněte si hello otazník další toohello typ dat ve hello `Duration` pole. Znamená to, že hello `Duration` pole může mít hodnotu null.

### <a name="key-concepts"></a>Klíčové koncepty
* **Sada řádků proměnné**: tooa proměnné lze přiřadit každý výraz dotazu, který vytvoří sadu řádků. U-SQL odpovídá vzoru pro pojmenovávání proměnné hello T-SQL (`@searchlog`, například) ve skriptu hello.
* Hello **EXTRAHOVAT** – klíčové slovo čte data ze souboru a definuje hello schématu pro čtení. `Extractors.Tsv`je integrované Extraktor U-SQL pro soubory karta oddělených hodnot. Můžete vyvíjet vlastní extraktory.
* Hello **výstup** zapisuje data ze souboru tooa sady řádků. `Outputters.Csv()`je integrované toocreate outputter U-SQL soubor oddělených čárkou. Můžete vyvíjet vlastní outputters.

### <a name="file-paths"></a>Cesty k souborům

Hello EXTRAKCE a výstup příkazy pomocí cesty k souborům. Cesty k souboru může být absolutní nebo relativní:

Tato následující absolutní cestu k souboru odkazuje tooa souborů v Data Lake Store s názvem `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Následující cesta k souboru začíná `"/"`. Odkazuje tooa souboru v hello výchozího účtu Data Lake Store:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Použijte skalární proměnné

Můžete použít skalární proměnné jako dobře toomake váš skript údržby jednodušší. skript U-SQL předchozí Hello lze zapsat také jako:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        too@out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Transformace sady řádků

Použití **vyberte** tootransform sady řádků:

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

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        too"/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Hello používá klauzule WHERE [C# logický výraz](https://msdn.microsoft.com/library/6a71f45d.aspx). Hello C# výrazu jazyka toodo můžete použít vlastní výrazy a funkce. Můžete provést i složitější filtrování je kombinací s logické spojky (operátoru and) a disjunctions (ORs).

Hello následující skript používá metoda DateTime.Parse() hello a spojení.

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

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        too"/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 >druhý dotaz Hello pracuje na výsledek hello hello první sadu řádků, které vytvoří složené hello dva filtry. Můžete také znovu použít název proměnné a lexikálně oborem názvů hello.

## <a name="aggregate-rowsets"></a>Agregační sady řádků
U-SQL poskytuje hello známé ORDER BY, GROUP BY a agregace.

Hello následující dotaz najde celková doba trvání hello každou oblast a potom zobrazí hello horní pět doby v pořadí.

Sady řádků U-SQL nezachováte jejich pořadí pro další dotaz hello. Proto tooorder na výstup, budete potřebovat tooadd Order toohello výstupu příkazu:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

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

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @rs1
        too@out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        too@out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

klauzule ORDER BY v U-SQL Hello vyžaduje pomocí klauzule načítání hello ve výrazu SELECT.

klauzule U-SQL NUTNOSTI Hello může být toogroups výstup hello použité toorestrict, které splňují podmínku HAVING hello:

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

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
        GROUP BY Region
        HAVING SUM(Duration) > 200;

    OUTPUT @res
        too"/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Agregace pokročilé scénáře naleznete hello hello U-SQL referenční dokumentaci pro [agregovat, analytické a odkazovat na funkce](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)

## <a name="next-steps"></a>Další kroky
* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
