---
title: "Začínáme s jazykem U-SQL | Microsoft Docs"
description: "Naučte se základy jazyka U-SQL."
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
ms.openlocfilehash: 38c4e1b9bd24ef0b8a81f6154620f3f98d3b5ac1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-u-sql"></a><span data-ttu-id="56d94-103">Začínáme s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="56d94-103">Get started with U-SQL</span></span>
<span data-ttu-id="56d94-104">U-SQL je jazyk, který kombinuje deklarativní SQL s imperativní jazyka C# k umožňují zpracování dat v jakémkoli měřítku.</span><span class="sxs-lookup"><span data-stu-id="56d94-104">U-SQL is a language that combines declarative SQL with imperative C# to let you process data at any scale.</span></span> <span data-ttu-id="56d94-105">Prostřednictvím funkce škálovatelné a distribuovaných dotazů U-SQL můžete efektivně analyzovat data napříč relační úložiště, jako je například Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56d94-105">Through the scalable, distributed-query capability of U-SQL, you can efficiently analyze data across relational stores such as Azure SQL Database.</span></span> <span data-ttu-id="56d94-106">Pomocí U-SQL můžete zpracovat nestrukturovaných dat tak, že použití schématu na čtení a vložení vlastní logiky a funkcí UDF.</span><span class="sxs-lookup"><span data-stu-id="56d94-106">With U-SQL, you can process unstructured data by applying schema on read and inserting custom logic and UDFs.</span></span> <span data-ttu-id="56d94-107">Navíc U-SQL zahrnuje rozšíření, která poskytuje jemně odstupňovanou kontrolu nad postup provést ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="56d94-107">Additionally, U-SQL includes extensibility that gives you fine-grained control over how to execute at scale.</span></span> 

## <a name="learning-resources"></a><span data-ttu-id="56d94-108">Studijní materiály</span><span class="sxs-lookup"><span data-stu-id="56d94-108">Learning resources</span></span>

* <span data-ttu-id="56d94-109">[U-SQL kurzu](http://aka.ms/usqltutorial) poskytuje návod s asistencí pro většinu jazykem U-SQL.</span><span class="sxs-lookup"><span data-stu-id="56d94-109">The [U-SQL Tutorial](http://aka.ms/usqltutorial) provides a guided walkthrough of most of the U-SQL language.</span></span> <span data-ttu-id="56d94-110">Tento dokument se doporučuje čtení pro všechny vývojáře, kteří požadují další U-SQL.</span><span class="sxs-lookup"><span data-stu-id="56d94-110">This document is recommended reading for all developers wanting to learn U-SQL.</span></span>
* <span data-ttu-id="56d94-111">Podrobné informace o **syntaxe jazyka U-SQL**, najdete v článku [referenční příručka jazyka U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="56d94-111">For detailed information about the **U-SQL language syntax**, see the [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span></span>
* <span data-ttu-id="56d94-112">Zjistit, **filosofie návrhu U-SQL**, naleznete v příspěvku blogu Visual Studio [představení U-SQL – A jazyk, který usnadňuje Big zpracování dat](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="56d94-112">To understand the **U-SQL design philosophy**, see the Visual Studio blog post [Introducing U-SQL – A Language that makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56d94-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="56d94-113">Prerequisites</span></span>

<span data-ttu-id="56d94-114">Než přejdete pomocí U-SQL ukázky v tomto dokumentu, přečtěte si a dokončete [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="56d94-114">Before you go through the U-SQL samples in this document, read and complete [Tutorial: Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="56d94-115">Tento kurz vysvětluje mechanismů U-SQL pomocí nástrojů Azure Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56d94-115">That tutorial explains the mechanics of using U-SQL with Azure Data Lake Tools for Visual Studio.</span></span>

## <a name="your-first-u-sql-script"></a><span data-ttu-id="56d94-116">Váš první skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="56d94-116">Your first U-SQL script</span></span>

<span data-ttu-id="56d94-117">Následující skript U-SQL je snadná a umožňují nám prozkoumat mnoho aspektů jazykem U-SQL.</span><span class="sxs-lookup"><span data-stu-id="56d94-117">The following U-SQL script is simple and lets us explore many aspects the U-SQL language.</span></span>

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
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="56d94-118">Tento skript nemá žádné kroky transformace.</span><span class="sxs-lookup"><span data-stu-id="56d94-118">This script doesn't have any transformation steps.</span></span> <span data-ttu-id="56d94-119">Kromě toho přečte ze zdrojového souboru s názvem `SearchLog.tsv`, schematizes ho a zapíše sadu řádků zpět do souboru s názvem SearchLog-first-u – sql.csv.</span><span class="sxs-lookup"><span data-stu-id="56d94-119">It reads from the source file called `SearchLog.tsv`, schematizes it, and writes the rowset back into a file called SearchLog-first-u-sql.csv.</span></span>

<span data-ttu-id="56d94-120">Všimněte si znaky otazníku vedle data, zadejte `Duration` pole.</span><span class="sxs-lookup"><span data-stu-id="56d94-120">Notice the question mark next to the data type in the `Duration` field.</span></span> <span data-ttu-id="56d94-121">Znamená to, že `Duration` pole může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="56d94-121">It means that the `Duration` field could be null.</span></span>

### <a name="key-concepts"></a><span data-ttu-id="56d94-122">Klíčové koncepty</span><span class="sxs-lookup"><span data-stu-id="56d94-122">Key concepts</span></span>
* <span data-ttu-id="56d94-123">**Sada řádků proměnné**: každý výraz dotazu, který vytvoří sadu řádků lze přiřadit k proměnné.</span><span class="sxs-lookup"><span data-stu-id="56d94-123">**Rowset variables**: Each query expression that produces a rowset can be assigned to a variable.</span></span> <span data-ttu-id="56d94-124">U-SQL se následující T-SQL proměnné pojmenování (`@searchlog`, například) ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="56d94-124">U-SQL follows the T-SQL variable naming pattern (`@searchlog`, for example) in the script.</span></span>
* <span data-ttu-id="56d94-125">**EXTRAHOVAT** – klíčové slovo čte data ze souboru a definuje schéma pro čtení.</span><span class="sxs-lookup"><span data-stu-id="56d94-125">The **EXTRACT** keyword reads data from a file and defines the schema on read.</span></span> <span data-ttu-id="56d94-126">`Extractors.Tsv`je integrované Extraktor U-SQL pro soubory karta oddělených hodnot.</span><span class="sxs-lookup"><span data-stu-id="56d94-126">`Extractors.Tsv` is a built-in U-SQL extractor for tab-separated-value files.</span></span> <span data-ttu-id="56d94-127">Můžete vyvíjet vlastní extraktory.</span><span class="sxs-lookup"><span data-stu-id="56d94-127">You can develop custom extractors.</span></span>
* <span data-ttu-id="56d94-128">**Výstup** zapisuje data ze sady řádků do souboru.</span><span class="sxs-lookup"><span data-stu-id="56d94-128">The **OUTPUT** writes data from a rowset to a file.</span></span> <span data-ttu-id="56d94-129">`Outputters.Csv()`je integrované outputter U-SQL vytvořte soubor oddělených čárkou.</span><span class="sxs-lookup"><span data-stu-id="56d94-129">`Outputters.Csv()` is a built-in U-SQL outputter to create a comma-separated-value file.</span></span> <span data-ttu-id="56d94-130">Můžete vyvíjet vlastní outputters.</span><span class="sxs-lookup"><span data-stu-id="56d94-130">You can develop custom outputters.</span></span>

### <a name="file-paths"></a><span data-ttu-id="56d94-131">Cesty k souborům</span><span class="sxs-lookup"><span data-stu-id="56d94-131">File paths</span></span>

<span data-ttu-id="56d94-132">EXTRAKCE a výstup příkazy pomocí cesty k souborům.</span><span class="sxs-lookup"><span data-stu-id="56d94-132">The EXTRACT and OUTPUT statements use file paths.</span></span> <span data-ttu-id="56d94-133">Cesty k souboru může být absolutní nebo relativní:</span><span class="sxs-lookup"><span data-stu-id="56d94-133">File paths can be absolute or relative:</span></span>

<span data-ttu-id="56d94-134">Tato následující absolutní cestu k souboru odkazuje na soubor v Data Lake Store s názvem `mystore`:</span><span class="sxs-lookup"><span data-stu-id="56d94-134">This following absolute file path refers to a file in a Data Lake Store named `mystore`:</span></span>

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

<span data-ttu-id="56d94-135">Následující cesta k souboru začíná `"/"`.</span><span class="sxs-lookup"><span data-stu-id="56d94-135">This following file path starts with `"/"`.</span></span> <span data-ttu-id="56d94-136">Odkazuje na soubor v výchozího účtu Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="56d94-136">It refers to a file in the default Data Lake Store account:</span></span>

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a><span data-ttu-id="56d94-137">Použijte skalární proměnné</span><span class="sxs-lookup"><span data-stu-id="56d94-137">Use scalar variables</span></span>

<span data-ttu-id="56d94-138">Skalární proměnné můžete použít také v zájmu snazší údržby vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="56d94-138">You can use scalar variables as well to make your script maintenance easier.</span></span> <span data-ttu-id="56d94-139">Předchozí skript U-SQL můžete zapsat také jako:</span><span class="sxs-lookup"><span data-stu-id="56d94-139">The previous U-SQL script can also be written as:</span></span>

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
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a><span data-ttu-id="56d94-140">Transformace sady řádků</span><span class="sxs-lookup"><span data-stu-id="56d94-140">Transform rowsets</span></span>

<span data-ttu-id="56d94-141">Použití **vyberte** k transformaci sady řádků:</span><span class="sxs-lookup"><span data-stu-id="56d94-141">Use **SELECT** to transform rowsets:</span></span>

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
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

<span data-ttu-id="56d94-142">Klauzule WHERE používá [C# logický výraz](https://msdn.microsoft.com/library/6a71f45d.aspx).</span><span class="sxs-lookup"><span data-stu-id="56d94-142">The WHERE clause uses a [C# Boolean expression](https://msdn.microsoft.com/library/6a71f45d.aspx).</span></span> <span data-ttu-id="56d94-143">Výraz jazyka C# můžete provést vlastní výrazy a funkce.</span><span class="sxs-lookup"><span data-stu-id="56d94-143">You can use the C# expression language to do your own expressions and functions.</span></span> <span data-ttu-id="56d94-144">Můžete provést i složitější filtrování je kombinací s logické spojky (operátoru and) a disjunctions (ORs).</span><span class="sxs-lookup"><span data-stu-id="56d94-144">You can even perform more complex filtering by combining them with logical conjunctions (ANDs) and disjunctions (ORs).</span></span>

<span data-ttu-id="56d94-145">Následující skript používá metodu DateTime.Parse() a spojení.</span><span class="sxs-lookup"><span data-stu-id="56d94-145">The following script uses the DateTime.Parse() method and a conjunction.</span></span>

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
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 ><span data-ttu-id="56d94-146">Druhý dotaz pracuje na výsledek první sadu řádků, které vytvoří složené dva filtry.</span><span class="sxs-lookup"><span data-stu-id="56d94-146">The second query is operating on the result of the first rowset, which creates a composite of the two filters.</span></span> <span data-ttu-id="56d94-147">Můžete také znovu použít název proměnné a lexikálně oborem názvů.</span><span class="sxs-lookup"><span data-stu-id="56d94-147">You can also reuse a variable name, and the names are scoped lexically.</span></span>

## <a name="aggregate-rowsets"></a><span data-ttu-id="56d94-148">Agregační sady řádků</span><span class="sxs-lookup"><span data-stu-id="56d94-148">Aggregate rowsets</span></span>
<span data-ttu-id="56d94-149">U-SQL vám poskytuje známé ORDER BY, GROUP BY a agregace.</span><span class="sxs-lookup"><span data-stu-id="56d94-149">U-SQL gives you the familiar ORDER BY, GROUP BY, and aggregations.</span></span>

<span data-ttu-id="56d94-150">Následující dotaz najde celkovou dobu trvání každou oblast a potom zobrazí horní pět doby v pořadí.</span><span class="sxs-lookup"><span data-stu-id="56d94-150">The following query finds the total duration per region, and then displays the top five durations in order.</span></span>

<span data-ttu-id="56d94-151">Sady řádků U-SQL nezachováte jejich pořadí pro další dotaz.</span><span class="sxs-lookup"><span data-stu-id="56d94-151">U-SQL rowsets do not preserve their order for the next query.</span></span> <span data-ttu-id="56d94-152">Proto pořadí výstup, musíte přidat ORDER BY výstup příkazu:</span><span class="sxs-lookup"><span data-stu-id="56d94-152">Thus, to order an output, you need to add ORDER BY to the OUTPUT statement:</span></span>

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
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="56d94-153">V klauzuli ORDER BY v U-SQL vyžaduje použití klauzuli FETCH ve výrazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="56d94-153">The U-SQL ORDER BY clause requires using the FETCH clause in a SELECT expression.</span></span>

<span data-ttu-id="56d94-154">V klauzuli NUTNOSTI U-SQL umožňuje omezit výstup do skupin, které splňují podmínku HAVING:</span><span class="sxs-lookup"><span data-stu-id="56d94-154">The U-SQL HAVING clause can be used to restrict the output to groups that satisfy the HAVING condition:</span></span>

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
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="56d94-155">Agregace pokročilé scénáře, najdete v dokumentaci odkaz na U-SQL pro [agregovat, analytické a odkazovat na funkce](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span><span class="sxs-lookup"><span data-stu-id="56d94-155">For advanced aggregation scenarios, see the The U-SQL reference documentation for [aggregate, analytic, and reference functions](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="56d94-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56d94-156">Next steps</span></span>
* [<span data-ttu-id="56d94-157">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="56d94-157">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="56d94-158">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56d94-158">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
