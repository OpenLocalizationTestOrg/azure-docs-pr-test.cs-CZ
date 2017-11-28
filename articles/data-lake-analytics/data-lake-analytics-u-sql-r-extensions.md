---
title: "aaaExtend U-SQL skriptů s R v Azure Data Lake Analytics | Microsoft Docs"
description: "Zjistěte, jak kód toorun R na skriptů U-SQL"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: 24affd4963a08d30a7111b49af388e9c1268430e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a><span data-ttu-id="1cf66-103">Kurz: Začínáme s jazykem U-SQL pomocí R rozšíření</span><span class="sxs-lookup"><span data-stu-id="1cf66-103">Tutorial: Get started with extending U-SQL with R</span></span>

<span data-ttu-id="1cf66-104">Hello následující příklad znázorňuje hello základní kroky pro nasazování kódu jazyka R:</span><span class="sxs-lookup"><span data-stu-id="1cf66-104">hello following example illustrates hello basic steps for deploying R code:</span></span>
* <span data-ttu-id="1cf66-105">Použití hello `REFERENCE ASSEMBLY` příkaz tooenable R rozšíření pro hello skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1cf66-105">Use hello `REFERENCE ASSEMBLY` statement tooenable R extensions for hello U-SQL Script.</span></span>
* <span data-ttu-id="1cf66-106">Použití` REDUCE` operace toopartition hello vstupní data na klíč.</span><span class="sxs-lookup"><span data-stu-id="1cf66-106">Use the` REDUCE` operation toopartition hello input data on a key.</span></span>
* <span data-ttu-id="1cf66-107">rozšíření Hello R U-SQL zahrnují předdefinované reduktorem (`Extension.R.Reducer`) spouští kódu jazyka R se v jednotlivých reduktorem toohello vrchol přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1cf66-107">hello R extensions for U-SQL include a built-in reducer (`Extension.R.Reducer`) that runs R code on each vertex assigned toohello reducer.</span></span> 
* <span data-ttu-id="1cf66-108">Použití vyhrazených s názvem datových rámců názvem `inputFromUSQL` a `outputToUSQL `v uvedeném pořadí toopass data mezi U-SQL a R. vstup a výstup se vyřešilo DataFrame identifikátor názvy (to znamená, uživatelé není možné změnit názvy těchto předdefinovaných vstupu a výstupu DataFrame identifikátory).</span><span class="sxs-lookup"><span data-stu-id="1cf66-108">Usage of dedicated named data frames called `inputFromUSQL` and `outputToUSQL `respectively toopass data between U-SQL and R. Input and output DataFrame identifier names are fixed (that is, users cannot change these predefined names of input and output DataFrame identifiers).</span></span>

## <a name="embedding-r-code-in-hello-u-sql-script"></a><span data-ttu-id="1cf66-109">Vložení kódu jazyka R v hello skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="1cf66-109">Embedding R code in hello U-SQL script</span></span>

<span data-ttu-id="1cf66-110">Skript U-SQL můžete vloženého hello R kódu s použitím parametru příkazu hello hello `Extension.R.Reducer`.</span><span class="sxs-lookup"><span data-stu-id="1cf66-110">You can inline hello R code your U-SQL script by using hello command parameter of hello `Extension.R.Reducer`.</span></span> <span data-ttu-id="1cf66-111">Můžete například deklarovat hello skript jazyka R jako řetězec proměnné a předáváme jako parametr toohello reduktorem.</span><span class="sxs-lookup"><span data-stu-id="1cf66-111">For example, you can declare hello R script as a string variable and pass it as a parameter toohello Reducer.</span></span>


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that hello column names are hello same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-hello-r-code-in-a-separate-file-and-reference-it--hello-u-sql-script"></a><span data-ttu-id="1cf66-112">Zachovat hello R kódu v samostatném souboru a odkazovat na hello U-SQL skriptů</span><span class="sxs-lookup"><span data-stu-id="1cf66-112">Keep hello R code in a separate file and reference it  hello U-SQL script</span></span>

<span data-ttu-id="1cf66-113">Hello následující příklad ukazuje použití složitější.</span><span class="sxs-lookup"><span data-stu-id="1cf66-113">hello following example illustrates a more complex usage.</span></span> <span data-ttu-id="1cf66-114">V takovém případě hello R kód nasazen jako prostředek, který je hello skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1cf66-114">In this case, hello R code is deployed as a RESOURCE that is hello U-SQL script.</span></span>

<span data-ttu-id="1cf66-115">Uložte tento kód R jako samostatný soubor.</span><span class="sxs-lookup"><span data-stu-id="1cf66-115">Save this R code as a separate file.</span></span>

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

<span data-ttu-id="1cf66-116">Pomocí skriptu R toodeploy skript U-SQL s hello příkaz nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="1cf66-116">Use a U-SQL script toodeploy that R script with hello DEPLOY RESOURCE statement.</span></span>

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput too@OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a><span data-ttu-id="1cf66-117">Jak R integruje s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="1cf66-117">How R Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="1cf66-118">Datové typy</span><span class="sxs-lookup"><span data-stu-id="1cf66-118">Datatypes</span></span>
* <span data-ttu-id="1cf66-119">Řetězec a číselné sloupce z U-SQL se převedou jako-mezi R DataFrame a jazykem U-SQL [podporované typy: `double`, `string`, `bool`, `integer`, `byte`].</span><span class="sxs-lookup"><span data-stu-id="1cf66-119">String and numeric columns from U-SQL are converted as-is between R DataFrame and U-SQL [supported types: `double`, `string`, `bool`, `integer`, `byte`].</span></span>
* <span data-ttu-id="1cf66-120">Hello `Factor` datový typ není podporován v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1cf66-120">hello `Factor` datatype is not supported in U-SQL.</span></span>
* <span data-ttu-id="1cf66-121">`byte[]`musí být serializován jako kódováním base64 `string`.</span><span class="sxs-lookup"><span data-stu-id="1cf66-121">`byte[]` must be serialized as a base64-encoded `string`.</span></span>
* <span data-ttu-id="1cf66-122">U-SQL řetězce může být převedený toofactors v kódu jazyka R, jakmile U-SQL vytvořit vstupní dataframe R nebo nastavení parametrem reduktorem hello `stringsAsFactors: true`.</span><span class="sxs-lookup"><span data-stu-id="1cf66-122">U-SQL strings can be converted toofactors in R code, once U-SQL create R input dataframe or by setting hello reducer parameter `stringsAsFactors: true`.</span></span>

### <a name="schemas"></a><span data-ttu-id="1cf66-123">Schémata</span><span class="sxs-lookup"><span data-stu-id="1cf66-123">Schemas</span></span>
* <span data-ttu-id="1cf66-124">Datové sady U-SQL nemůže mít duplicitní názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="1cf66-124">U-SQL datasets cannot have duplicate column names.</span></span>
* <span data-ttu-id="1cf66-125">Názvy sloupců datových sad U-SQL musí být řetězce.</span><span class="sxs-lookup"><span data-stu-id="1cf66-125">U-SQL datasets column names must be strings.</span></span>
* <span data-ttu-id="1cf66-126">Názvy sloupců musí být hello stejné skripty U-SQL a R.</span><span class="sxs-lookup"><span data-stu-id="1cf66-126">Column names must be hello same in U-SQL and R scripts.</span></span>
* <span data-ttu-id="1cf66-127">Sloupce jen pro čtení nemůže být součástí dataframe výstup hello.</span><span class="sxs-lookup"><span data-stu-id="1cf66-127">Readonly column cannot be part of hello output dataframe.</span></span> <span data-ttu-id="1cf66-128">Protože sloupce jen pro čtení jsou automaticky vložit zpět v tabulce U-SQL hello, pokud je součástí výstupního schématu UDO.</span><span class="sxs-lookup"><span data-stu-id="1cf66-128">Because readonly columns are automatically injected back in hello U-SQL table if it is a part of output schema of UDO.</span></span>

### <a name="functional-limitations"></a><span data-ttu-id="1cf66-129">Funkční omezení</span><span class="sxs-lookup"><span data-stu-id="1cf66-129">Functional limitations</span></span>
* <span data-ttu-id="1cf66-130">Hello modul R nelze vytvořit instanci dvakrát v hello stejný postup.</span><span class="sxs-lookup"><span data-stu-id="1cf66-130">hello R Engine can't be instantiated twice in hello same process.</span></span> 
* <span data-ttu-id="1cf66-131">U-SQL v současné době nepodporuje kombinační UDO pro použití oddílů modelů generována pomocí reduktorem UDO předpovědi.</span><span class="sxs-lookup"><span data-stu-id="1cf66-131">Currently, U-SQL does not support Combiner UDOs for prediction using partitioned models generated using Reducer UDOs.</span></span> <span data-ttu-id="1cf66-132">Uživatelé můžou deklarovat hello rozdělena na oddíly modely jako prostředek a používat je ve své skriptu R (viz ukázkový kód `ExtR_PredictUsingLMRawStringReducer.usql`)</span><span class="sxs-lookup"><span data-stu-id="1cf66-132">Users can declare hello partitioned models as resource and use them in their R Script (see sample code `ExtR_PredictUsingLMRawStringReducer.usql`)</span></span>

### <a name="r-versions"></a><span data-ttu-id="1cf66-133">Verze R</span><span class="sxs-lookup"><span data-stu-id="1cf66-133">R Versions</span></span>
<span data-ttu-id="1cf66-134">Je podporován pouze R 3.2.2.</span><span class="sxs-lookup"><span data-stu-id="1cf66-134">Only R 3.2.2 is supported.</span></span>

### <a name="standard-r-modules"></a><span data-ttu-id="1cf66-135">Standardní moduly R</span><span class="sxs-lookup"><span data-stu-id="1cf66-135">Standard R modules</span></span>

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="1cf66-136">Vstup a výstup omezení velikosti</span><span class="sxs-lookup"><span data-stu-id="1cf66-136">Input and Output size limitations</span></span>
<span data-ttu-id="1cf66-137">Všechny vrcholy má omezené množství paměti přidělené tooit.</span><span class="sxs-lookup"><span data-stu-id="1cf66-137">Every vertex has a limited amount of memory assigned tooit.</span></span> <span data-ttu-id="1cf66-138">Protože hello vstupní a výstupní DataFrames musí existovat v paměti v kódu hello R, hello celková velikost hello vstupní a výstupní nemůže překročit 500 MB.</span><span class="sxs-lookup"><span data-stu-id="1cf66-138">Because hello input and output DataFrames must exist in memory in hello R code, hello total size for hello input and output cannot exceed 500 MB.</span></span>

### <a name="sample-code"></a><span data-ttu-id="1cf66-139">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="1cf66-139">Sample Code</span></span>
<span data-ttu-id="1cf66-140">Další ukázkový kód je k dispozici v účtu Data Lake Store, po instalaci rozšíření hello Advanced Analytics U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1cf66-140">More sample code is available in your Data Lake Store account after you install hello U-SQL Advanced Analytics extensions.</span></span> <span data-ttu-id="1cf66-141">Hello cesta pro další ukázkový kód je: `<your_account_address>/usqlext/samples/R`.</span><span class="sxs-lookup"><span data-stu-id="1cf66-141">hello path for more sample code is: `<your_account_address>/usqlext/samples/R`.</span></span> 

## <a name="deploying-custom-r-modules-with-u-sql"></a><span data-ttu-id="1cf66-142">Nasazení vlastní R modulů s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="1cf66-142">Deploying Custom R modules with U-SQL</span></span>

<span data-ttu-id="1cf66-143">Nejprve vytvořit vlastní modul R a zip ho a pak nahrajte hello ZIP R vlastní modul souboru tooyour ADL úložiště.</span><span class="sxs-lookup"><span data-stu-id="1cf66-143">First, create an R custom module and zip it and then upload hello zipped R custom module file tooyour ADL store.</span></span> <span data-ttu-id="1cf66-144">V příkladu hello jsme odešlete magittr_1.5.zip toohello kořenového účtu ADLS hello výchozí pro hello ADLA účet, který používáme.</span><span class="sxs-lookup"><span data-stu-id="1cf66-144">In hello example, we will upload magittr_1.5.zip toohello root of hello default ADLS account for hello ADLA account we are using.</span></span> <span data-ttu-id="1cf66-145">Jakmile nahrajete hello modulu tooADL úložiště, deklarujte ji jako použijte toomake nasazení prostředků je k dispozici v skript U-SQL a volání `install.packages` tooinstall ho.</span><span class="sxs-lookup"><span data-stu-id="1cf66-145">Once you upload hello module tooADL store, declare it as use DEPLOY RESOURCE toomake it available in your U-SQL script and call `install.packages` tooinstall it.</span></span>

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script toorun
    DECLARE @myRScript = @"
    # install hello magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load hello magrittr package,
    require(magrittr),
    # demonstrate use of hello magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput too@OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a><span data-ttu-id="1cf66-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1cf66-146">Next Steps</span></span>
* [<span data-ttu-id="1cf66-147">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="1cf66-147">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="1cf66-148">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1cf66-148">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="1cf66-149">Pro úlohy Azure Data Lake Analytics pomocí U-SQL okno funkce</span><span class="sxs-lookup"><span data-stu-id="1cf66-149">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)
