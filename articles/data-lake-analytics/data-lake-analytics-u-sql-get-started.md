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
# <a name="get-started-with-u-sql"></a><span data-ttu-id="1a828-103">Začínáme s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="1a828-103">Get started with U-SQL</span></span>
<span data-ttu-id="1a828-104">U-SQL je jazyk, který kombinuje deklarativní SQL s imperativní C# toolet při zpracování dat v jakémkoli měřítku.</span><span class="sxs-lookup"><span data-stu-id="1a828-104">U-SQL is a language that combines declarative SQL with imperative C# toolet you process data at any scale.</span></span> <span data-ttu-id="1a828-105">Prostřednictvím hello škálovatelné a distribuovaných dotazů funkce U-SQL, které můžete efektivně analyzovat data napříč relační úložiště, jako je například Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1a828-105">Through hello scalable, distributed-query capability of U-SQL, you can efficiently analyze data across relational stores such as Azure SQL Database.</span></span> <span data-ttu-id="1a828-106">Pomocí U-SQL můžete zpracovat nestrukturovaných dat tak, že použití schématu na čtení a vložení vlastní logiky a funkcí UDF.</span><span class="sxs-lookup"><span data-stu-id="1a828-106">With U-SQL, you can process unstructured data by applying schema on read and inserting custom logic and UDFs.</span></span> <span data-ttu-id="1a828-107">Navíc U-SQL zahrnuje rozšíření, která poskytuje jemně odstupňovanou kontrolu nad jak tooexecute ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="1a828-107">Additionally, U-SQL includes extensibility that gives you fine-grained control over how tooexecute at scale.</span></span> 

## <a name="learning-resources"></a><span data-ttu-id="1a828-108">Studijní materiály</span><span class="sxs-lookup"><span data-stu-id="1a828-108">Learning resources</span></span>

* <span data-ttu-id="1a828-109">Hello [U-SQL kurzu](http://aka.ms/usqltutorial) poskytuje návod s asistencí pro většinu jazyka hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1a828-109">hello [U-SQL Tutorial](http://aka.ms/usqltutorial) provides a guided walkthrough of most of hello U-SQL language.</span></span> <span data-ttu-id="1a828-110">Tento dokument se doporučuje čtení pro všechny vývojáře, kteří požadují toolearn U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1a828-110">This document is recommended reading for all developers wanting toolearn U-SQL.</span></span>
* <span data-ttu-id="1a828-111">Podrobné informace o hello **syntaxe jazyka U-SQL**, najdete v části hello [referenční příručka jazyka U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="1a828-111">For detailed information about hello **U-SQL language syntax**, see hello [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span></span>
* <span data-ttu-id="1a828-112">toounderstand hello **filosofie návrhu U-SQL**, najdete v příspěvku blogu Visual Studio hello [představení U-SQL – A jazyk, který usnadňuje Big zpracování dat](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="1a828-112">toounderstand hello **U-SQL design philosophy**, see hello Visual Studio blog post [Introducing U-SQL – A Language that makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a828-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1a828-113">Prerequisites</span></span>

<span data-ttu-id="1a828-114">Než přejdete prostřednictvím hello U-SQL ukázky v tomto dokumentu, přečtěte si a dokončete [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1a828-114">Before you go through hello U-SQL samples in this document, read and complete [Tutorial: Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="1a828-115">Tento kurz vysvětluje hello mechanismů U-SQL pomocí nástrojů Azure Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a828-115">That tutorial explains hello mechanics of using U-SQL with Azure Data Lake Tools for Visual Studio.</span></span>

## <a name="your-first-u-sql-script"></a><span data-ttu-id="1a828-116">Váš první skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="1a828-116">Your first U-SQL script</span></span>

<span data-ttu-id="1a828-117">Hello následující skript U-SQL je snadná a umožňují nám prozkoumat mnoho jazyk aspekty hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1a828-117">hello following U-SQL script is simple and lets us explore many aspects hello U-SQL language.</span></span>

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

<span data-ttu-id="1a828-118">Tento skript nemá žádné kroky transformace.</span><span class="sxs-lookup"><span data-stu-id="1a828-118">This script doesn't have any transformation steps.</span></span> <span data-ttu-id="1a828-119">Přečte z hello zdrojového souboru s názvem `SearchLog.tsv`, schematizes ho a zapíše hello řádků zpět do souboru s názvem SearchLog-first-u – sql.csv.</span><span class="sxs-lookup"><span data-stu-id="1a828-119">It reads from hello source file called `SearchLog.tsv`, schematizes it, and writes hello rowset back into a file called SearchLog-first-u-sql.csv.</span></span>

<span data-ttu-id="1a828-120">Všimněte si hello otazník další toohello typ dat ve hello `Duration` pole.</span><span class="sxs-lookup"><span data-stu-id="1a828-120">Notice hello question mark next toohello data type in hello `Duration` field.</span></span> <span data-ttu-id="1a828-121">Znamená to, že hello `Duration` pole může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="1a828-121">It means that hello `Duration` field could be null.</span></span>

### <a name="key-concepts"></a><span data-ttu-id="1a828-122">Klíčové koncepty</span><span class="sxs-lookup"><span data-stu-id="1a828-122">Key concepts</span></span>
* <span data-ttu-id="1a828-123">**Sada řádků proměnné**: tooa proměnné lze přiřadit každý výraz dotazu, který vytvoří sadu řádků.</span><span class="sxs-lookup"><span data-stu-id="1a828-123">**Rowset variables**: Each query expression that produces a rowset can be assigned tooa variable.</span></span> <span data-ttu-id="1a828-124">U-SQL odpovídá vzoru pro pojmenovávání proměnné hello T-SQL (`@searchlog`, například) ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="1a828-124">U-SQL follows hello T-SQL variable naming pattern (`@searchlog`, for example) in hello script.</span></span>
* <span data-ttu-id="1a828-125">Hello **EXTRAHOVAT** – klíčové slovo čte data ze souboru a definuje hello schématu pro čtení.</span><span class="sxs-lookup"><span data-stu-id="1a828-125">hello **EXTRACT** keyword reads data from a file and defines hello schema on read.</span></span> <span data-ttu-id="1a828-126">`Extractors.Tsv`je integrované Extraktor U-SQL pro soubory karta oddělených hodnot.</span><span class="sxs-lookup"><span data-stu-id="1a828-126">`Extractors.Tsv` is a built-in U-SQL extractor for tab-separated-value files.</span></span> <span data-ttu-id="1a828-127">Můžete vyvíjet vlastní extraktory.</span><span class="sxs-lookup"><span data-stu-id="1a828-127">You can develop custom extractors.</span></span>
* <span data-ttu-id="1a828-128">Hello **výstup** zapisuje data ze souboru tooa sady řádků.</span><span class="sxs-lookup"><span data-stu-id="1a828-128">hello **OUTPUT** writes data from a rowset tooa file.</span></span> <span data-ttu-id="1a828-129">`Outputters.Csv()`je integrované toocreate outputter U-SQL soubor oddělených čárkou.</span><span class="sxs-lookup"><span data-stu-id="1a828-129">`Outputters.Csv()` is a built-in U-SQL outputter toocreate a comma-separated-value file.</span></span> <span data-ttu-id="1a828-130">Můžete vyvíjet vlastní outputters.</span><span class="sxs-lookup"><span data-stu-id="1a828-130">You can develop custom outputters.</span></span>

### <a name="file-paths"></a><span data-ttu-id="1a828-131">Cesty k souborům</span><span class="sxs-lookup"><span data-stu-id="1a828-131">File paths</span></span>

<span data-ttu-id="1a828-132">Hello EXTRAKCE a výstup příkazy pomocí cesty k souborům.</span><span class="sxs-lookup"><span data-stu-id="1a828-132">hello EXTRACT and OUTPUT statements use file paths.</span></span> <span data-ttu-id="1a828-133">Cesty k souboru může být absolutní nebo relativní:</span><span class="sxs-lookup"><span data-stu-id="1a828-133">File paths can be absolute or relative:</span></span>

<span data-ttu-id="1a828-134">Tato následující absolutní cestu k souboru odkazuje tooa souborů v Data Lake Store s názvem `mystore`:</span><span class="sxs-lookup"><span data-stu-id="1a828-134">This following absolute file path refers tooa file in a Data Lake Store named `mystore`:</span></span>

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

<span data-ttu-id="1a828-135">Následující cesta k souboru začíná `"/"`.</span><span class="sxs-lookup"><span data-stu-id="1a828-135">This following file path starts with `"/"`.</span></span> <span data-ttu-id="1a828-136">Odkazuje tooa souboru v hello výchozího účtu Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="1a828-136">It refers tooa file in hello default Data Lake Store account:</span></span>

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a><span data-ttu-id="1a828-137">Použijte skalární proměnné</span><span class="sxs-lookup"><span data-stu-id="1a828-137">Use scalar variables</span></span>

<span data-ttu-id="1a828-138">Můžete použít skalární proměnné jako dobře toomake váš skript údržby jednodušší.</span><span class="sxs-lookup"><span data-stu-id="1a828-138">You can use scalar variables as well toomake your script maintenance easier.</span></span> <span data-ttu-id="1a828-139">skript U-SQL předchozí Hello lze zapsat také jako:</span><span class="sxs-lookup"><span data-stu-id="1a828-139">hello previous U-SQL script can also be written as:</span></span>

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

## <a name="transform-rowsets"></a><span data-ttu-id="1a828-140">Transformace sady řádků</span><span class="sxs-lookup"><span data-stu-id="1a828-140">Transform rowsets</span></span>

<span data-ttu-id="1a828-141">Použití **vyberte** tootransform sady řádků:</span><span class="sxs-lookup"><span data-stu-id="1a828-141">Use **SELECT** tootransform rowsets:</span></span>

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

<span data-ttu-id="1a828-142">Hello používá klauzule WHERE [C# logický výraz](https://msdn.microsoft.com/library/6a71f45d.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a828-142">hello WHERE clause uses a [C# Boolean expression](https://msdn.microsoft.com/library/6a71f45d.aspx).</span></span> <span data-ttu-id="1a828-143">Hello C# výrazu jazyka toodo můžete použít vlastní výrazy a funkce.</span><span class="sxs-lookup"><span data-stu-id="1a828-143">You can use hello C# expression language toodo your own expressions and functions.</span></span> <span data-ttu-id="1a828-144">Můžete provést i složitější filtrování je kombinací s logické spojky (operátoru and) a disjunctions (ORs).</span><span class="sxs-lookup"><span data-stu-id="1a828-144">You can even perform more complex filtering by combining them with logical conjunctions (ANDs) and disjunctions (ORs).</span></span>

<span data-ttu-id="1a828-145">Hello následující skript používá metoda DateTime.Parse() hello a spojení.</span><span class="sxs-lookup"><span data-stu-id="1a828-145">hello following script uses hello DateTime.Parse() method and a conjunction.</span></span>

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
 ><span data-ttu-id="1a828-146">druhý dotaz Hello pracuje na výsledek hello hello první sadu řádků, které vytvoří složené hello dva filtry.</span><span class="sxs-lookup"><span data-stu-id="1a828-146">hello second query is operating on hello result of hello first rowset, which creates a composite of hello two filters.</span></span> <span data-ttu-id="1a828-147">Můžete také znovu použít název proměnné a lexikálně oborem názvů hello.</span><span class="sxs-lookup"><span data-stu-id="1a828-147">You can also reuse a variable name, and hello names are scoped lexically.</span></span>

## <a name="aggregate-rowsets"></a><span data-ttu-id="1a828-148">Agregační sady řádků</span><span class="sxs-lookup"><span data-stu-id="1a828-148">Aggregate rowsets</span></span>
<span data-ttu-id="1a828-149">U-SQL poskytuje hello známé ORDER BY, GROUP BY a agregace.</span><span class="sxs-lookup"><span data-stu-id="1a828-149">U-SQL gives you hello familiar ORDER BY, GROUP BY, and aggregations.</span></span>

<span data-ttu-id="1a828-150">Hello následující dotaz najde celková doba trvání hello každou oblast a potom zobrazí hello horní pět doby v pořadí.</span><span class="sxs-lookup"><span data-stu-id="1a828-150">hello following query finds hello total duration per region, and then displays hello top five durations in order.</span></span>

<span data-ttu-id="1a828-151">Sady řádků U-SQL nezachováte jejich pořadí pro další dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="1a828-151">U-SQL rowsets do not preserve their order for hello next query.</span></span> <span data-ttu-id="1a828-152">Proto tooorder na výstup, budete potřebovat tooadd Order toohello výstupu příkazu:</span><span class="sxs-lookup"><span data-stu-id="1a828-152">Thus, tooorder an output, you need tooadd ORDER BY toohello OUTPUT statement:</span></span>

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

<span data-ttu-id="1a828-153">klauzule ORDER BY v U-SQL Hello vyžaduje pomocí klauzule načítání hello ve výrazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="1a828-153">hello U-SQL ORDER BY clause requires using hello FETCH clause in a SELECT expression.</span></span>

<span data-ttu-id="1a828-154">klauzule U-SQL NUTNOSTI Hello může být toogroups výstup hello použité toorestrict, které splňují podmínku HAVING hello:</span><span class="sxs-lookup"><span data-stu-id="1a828-154">hello U-SQL HAVING clause can be used toorestrict hello output toogroups that satisfy hello HAVING condition:</span></span>

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

<span data-ttu-id="1a828-155">Agregace pokročilé scénáře naleznete hello hello U-SQL referenční dokumentaci pro [agregovat, analytické a odkazovat na funkce](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span><span class="sxs-lookup"><span data-stu-id="1a828-155">For advanced aggregation scenarios, see hello hello U-SQL reference documentation for [aggregate, analytic, and reference functions](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a828-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a828-156">Next steps</span></span>
* [<span data-ttu-id="1a828-157">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="1a828-157">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="1a828-158">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a828-158">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
