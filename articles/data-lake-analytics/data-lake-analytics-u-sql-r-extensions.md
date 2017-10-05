---
title: "Rozšíření skriptů U-SQL pomocí R v Azure Data Lake Analytics | Microsoft Docs"
description: "Informace o spouštění kódu jazyka R v skriptů U-SQL"
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
ms.openlocfilehash: d479af515566f497d9611e75426f6acb8f8276d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a><span data-ttu-id="900c4-103">Kurz: Začínáme s jazykem U-SQL pomocí R rozšíření</span><span class="sxs-lookup"><span data-stu-id="900c4-103">Tutorial: Get started with extending U-SQL with R</span></span>

<span data-ttu-id="900c4-104">Následující příklad ukazuje základní kroky pro nasazování kódu jazyka R:</span><span class="sxs-lookup"><span data-stu-id="900c4-104">The following example illustrates the basic steps for deploying R code:</span></span>
* <span data-ttu-id="900c4-105">Použití `REFERENCE ASSEMBLY` příkaz umožňující rozšíření R skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="900c4-105">Use the `REFERENCE ASSEMBLY` statement to enable R extensions for the U-SQL Script.</span></span>
* <span data-ttu-id="900c4-106">Použití` REDUCE` operace vstupní data pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="900c4-106">Use the` REDUCE` operation to partition the input data on a key.</span></span>
* <span data-ttu-id="900c4-107">Rozšíření R U-SQL zahrnují předdefinované reduktorem (`Extension.R.Reducer`) spouští kódu jazyka R se v jednotlivých vrchol přiřazené reduktorem.</span><span class="sxs-lookup"><span data-stu-id="900c4-107">The R extensions for U-SQL include a built-in reducer (`Extension.R.Reducer`) that runs R code on each vertex assigned to the reducer.</span></span> 
* <span data-ttu-id="900c4-108">Použití vyhrazených s názvem datových rámců názvem `inputFromUSQL` a `outputToUSQL `v uvedeném pořadí k předávání dat mezi U-SQL a R. vstup a výstup DataFrame opravě názvy identifikátorů (to znamená, uživatelé není možné změnit názvy těchto předdefinovaných vstupu a výstupu DataFrame identifikátory).</span><span class="sxs-lookup"><span data-stu-id="900c4-108">Usage of dedicated named data frames called `inputFromUSQL` and `outputToUSQL `respectively to pass data between U-SQL and R. Input and output DataFrame identifier names are fixed (that is, users cannot change these predefined names of input and output DataFrame identifiers).</span></span>

## <a name="embedding-r-code-in-the-u-sql-script"></a><span data-ttu-id="900c4-109">Vložení kódu jazyka R ve skriptu U-SQL</span><span class="sxs-lookup"><span data-stu-id="900c4-109">Embedding R code in the U-SQL script</span></span>

<span data-ttu-id="900c4-110">Můžete vložený kód R skript U-SQL pomocí parametru příkazového `Extension.R.Reducer`.</span><span class="sxs-lookup"><span data-stu-id="900c4-110">You can inline the R code your U-SQL script by using the command parameter of the `Extension.R.Reducer`.</span></span> <span data-ttu-id="900c4-111">Můžete například deklarovat R skript jako řetězec proměnné a předejte ji jako parametr reduktorem.</span><span class="sxs-lookup"><span data-stu-id="900c4-111">For example, you can declare the R script as a string variable and pass it as a parameter to the Reducer.</span></span>


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a><span data-ttu-id="900c4-112">Zachovat kód R v samostatném souboru a odkazovat na skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="900c4-112">Keep the R code in a separate file and reference it  the U-SQL script</span></span>

<span data-ttu-id="900c4-113">Následující příklad ukazuje použití složitější.</span><span class="sxs-lookup"><span data-stu-id="900c4-113">The following example illustrates a more complex usage.</span></span> <span data-ttu-id="900c4-114">V tomto případě kód R je nasazený jako prostředek, který je skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="900c4-114">In this case, the R code is deployed as a RESOURCE that is the U-SQL script.</span></span>

<span data-ttu-id="900c4-115">Uložte tento kód R jako samostatný soubor.</span><span class="sxs-lookup"><span data-stu-id="900c4-115">Save this R code as a separate file.</span></span>

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

<span data-ttu-id="900c4-116">K nasazení tohoto skriptu jazyka R s příkazem nasazení prostředků pomocí skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="900c4-116">Use a U-SQL script to deploy that R script with the DEPLOY RESOURCE statement.</span></span>

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
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a><span data-ttu-id="900c4-117">Jak R integruje s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="900c4-117">How R Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="900c4-118">Datové typy</span><span class="sxs-lookup"><span data-stu-id="900c4-118">Datatypes</span></span>
* <span data-ttu-id="900c4-119">Řetězec a číselné sloupce z U-SQL se převedou jako-mezi R DataFrame a jazykem U-SQL [podporované typy: `double`, `string`, `bool`, `integer`, `byte`].</span><span class="sxs-lookup"><span data-stu-id="900c4-119">String and numeric columns from U-SQL are converted as-is between R DataFrame and U-SQL [supported types: `double`, `string`, `bool`, `integer`, `byte`].</span></span>
* <span data-ttu-id="900c4-120">`Factor` Datový typ není podporován v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="900c4-120">The `Factor` datatype is not supported in U-SQL.</span></span>
* <span data-ttu-id="900c4-121">`byte[]`musí být serializován jako kódováním base64 `string`.</span><span class="sxs-lookup"><span data-stu-id="900c4-121">`byte[]` must be serialized as a base64-encoded `string`.</span></span>
* <span data-ttu-id="900c4-122">Řetězce U-SQL můžete převést na faktory v kódu jazyka R, jakmile U-SQL vytvořit vstupní dataframe R nebo nastavením parametru reduktorem `stringsAsFactors: true`.</span><span class="sxs-lookup"><span data-stu-id="900c4-122">U-SQL strings can be converted to factors in R code, once U-SQL create R input dataframe or by setting the reducer parameter `stringsAsFactors: true`.</span></span>

### <a name="schemas"></a><span data-ttu-id="900c4-123">Schémata</span><span class="sxs-lookup"><span data-stu-id="900c4-123">Schemas</span></span>
* <span data-ttu-id="900c4-124">Datové sady U-SQL nemůže mít duplicitní názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="900c4-124">U-SQL datasets cannot have duplicate column names.</span></span>
* <span data-ttu-id="900c4-125">Názvy sloupců datových sad U-SQL musí být řetězce.</span><span class="sxs-lookup"><span data-stu-id="900c4-125">U-SQL datasets column names must be strings.</span></span>
* <span data-ttu-id="900c4-126">Názvy sloupců musí být stejná U-SQL a skripty R.</span><span class="sxs-lookup"><span data-stu-id="900c4-126">Column names must be the same in U-SQL and R scripts.</span></span>
* <span data-ttu-id="900c4-127">Sloupce jen pro čtení nemůže být součástí dataframe výstup.</span><span class="sxs-lookup"><span data-stu-id="900c4-127">Readonly column cannot be part of the output dataframe.</span></span> <span data-ttu-id="900c4-128">Protože sloupce jen pro čtení jsou automaticky vložit zpět v tabulce U-SQL, pokud je součástí výstupního schématu UDO.</span><span class="sxs-lookup"><span data-stu-id="900c4-128">Because readonly columns are automatically injected back in the U-SQL table if it is a part of output schema of UDO.</span></span>

### <a name="functional-limitations"></a><span data-ttu-id="900c4-129">Funkční omezení</span><span class="sxs-lookup"><span data-stu-id="900c4-129">Functional limitations</span></span>
* <span data-ttu-id="900c4-130">Modul R nelze vytvořit instanci dvakrát v rámci jednoho procesu.</span><span class="sxs-lookup"><span data-stu-id="900c4-130">The R Engine can't be instantiated twice in the same process.</span></span> 
* <span data-ttu-id="900c4-131">U-SQL v současné době nepodporuje kombinační UDO pro použití oddílů modelů generována pomocí reduktorem UDO předpovědi.</span><span class="sxs-lookup"><span data-stu-id="900c4-131">Currently, U-SQL does not support Combiner UDOs for prediction using partitioned models generated using Reducer UDOs.</span></span> <span data-ttu-id="900c4-132">Uživatelé můžou deklarovat oddílů modely jako prostředek a používat je ve své skriptu R (viz ukázkový kód `ExtR_PredictUsingLMRawStringReducer.usql`)</span><span class="sxs-lookup"><span data-stu-id="900c4-132">Users can declare the partitioned models as resource and use them in their R Script (see sample code `ExtR_PredictUsingLMRawStringReducer.usql`)</span></span>

### <a name="r-versions"></a><span data-ttu-id="900c4-133">Verze R</span><span class="sxs-lookup"><span data-stu-id="900c4-133">R Versions</span></span>
<span data-ttu-id="900c4-134">Je podporován pouze R 3.2.2.</span><span class="sxs-lookup"><span data-stu-id="900c4-134">Only R 3.2.2 is supported.</span></span>

### <a name="standard-r-modules"></a><span data-ttu-id="900c4-135">Standardní moduly R</span><span class="sxs-lookup"><span data-stu-id="900c4-135">Standard R modules</span></span>

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

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="900c4-136">Vstup a výstup omezení velikosti</span><span class="sxs-lookup"><span data-stu-id="900c4-136">Input and Output size limitations</span></span>
<span data-ttu-id="900c4-137">Všechny vrcholy má omezené množství paměti přidělené k němu.</span><span class="sxs-lookup"><span data-stu-id="900c4-137">Every vertex has a limited amount of memory assigned to it.</span></span> <span data-ttu-id="900c4-138">Vzhledem k tomu, že vstupní a výstupní DataFrames musí existovat v paměti v kódu jazyka R, celková velikost vstupní a výstupní nemůže překročit 500 MB.</span><span class="sxs-lookup"><span data-stu-id="900c4-138">Because the input and output DataFrames must exist in memory in the R code, the total size for the input and output cannot exceed 500 MB.</span></span>

### <a name="sample-code"></a><span data-ttu-id="900c4-139">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="900c4-139">Sample Code</span></span>
<span data-ttu-id="900c4-140">Další ukázkový kód je k dispozici v účtu Data Lake Store, po instalaci rozšíření Advanced Analytics U-SQL.</span><span class="sxs-lookup"><span data-stu-id="900c4-140">More sample code is available in your Data Lake Store account after you install the U-SQL Advanced Analytics extensions.</span></span> <span data-ttu-id="900c4-141">Cesta pro další ukázkový kód je: `<your_account_address>/usqlext/samples/R`.</span><span class="sxs-lookup"><span data-stu-id="900c4-141">The path for more sample code is: `<your_account_address>/usqlext/samples/R`.</span></span> 

## <a name="deploying-custom-r-modules-with-u-sql"></a><span data-ttu-id="900c4-142">Nasazení vlastní R modulů s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="900c4-142">Deploying Custom R modules with U-SQL</span></span>

<span data-ttu-id="900c4-143">Nejprve vytvořit vlastní modul R a zip ho a pak nahrajte soubor ZIP vlastní modul R ADL úložiště.</span><span class="sxs-lookup"><span data-stu-id="900c4-143">First, create an R custom module and zip it and then upload the zipped R custom module file to your ADL store.</span></span> <span data-ttu-id="900c4-144">V příkladu jsme odešlete magittr_1.5.zip do kořenového adresáře výchozího účtu ADLS ADLA účtu, který používáme.</span><span class="sxs-lookup"><span data-stu-id="900c4-144">In the example, we will upload magittr_1.5.zip to the root of the default ADLS account for the ADLA account we are using.</span></span> <span data-ttu-id="900c4-145">Jakmile nahrajete do úložiště ADL modul, deklarujte ji jako použijte nasazení prostředků a zpřístupněte ji ve skriptu U-SQL a volání `install.packages` k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="900c4-145">Once you upload the module to ADL store, declare it as use DEPLOY RESOURCE to make it available in your U-SQL script and call `install.packages` to install it.</span></span>

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
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

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a><span data-ttu-id="900c4-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="900c4-146">Next Steps</span></span>
* [<span data-ttu-id="900c4-147">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="900c4-147">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="900c4-148">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="900c4-148">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="900c4-149">Pro úlohy Azure Data Lake Analytics pomocí U-SQL okno funkce</span><span class="sxs-lookup"><span data-stu-id="900c4-149">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)
