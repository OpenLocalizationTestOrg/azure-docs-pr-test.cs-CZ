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
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a>Kurz: Začínáme s jazykem U-SQL pomocí R rozšíření

Hello následující příklad znázorňuje hello základní kroky pro nasazování kódu jazyka R:
* Použití hello `REFERENCE ASSEMBLY` příkaz tooenable R rozšíření pro hello skript U-SQL.
* Použití` REDUCE` operace toopartition hello vstupní data na klíč.
* rozšíření Hello R U-SQL zahrnují předdefinované reduktorem (`Extension.R.Reducer`) spouští kódu jazyka R se v jednotlivých reduktorem toohello vrchol přiřazen. 
* Použití vyhrazených s názvem datových rámců názvem `inputFromUSQL` a `outputToUSQL `v uvedeném pořadí toopass data mezi U-SQL a R. vstup a výstup se vyřešilo DataFrame identifikátor názvy (to znamená, uživatelé není možné změnit názvy těchto předdefinovaných vstupu a výstupu DataFrame identifikátory).

## <a name="embedding-r-code-in-hello-u-sql-script"></a>Vložení kódu jazyka R v hello skript U-SQL

Skript U-SQL můžete vloženého hello R kódu s použitím parametru příkazu hello hello `Extension.R.Reducer`. Můžete například deklarovat hello skript jazyka R jako řetězec proměnné a předáváme jako parametr toohello reduktorem.


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

## <a name="keep-hello-r-code-in-a-separate-file-and-reference-it--hello-u-sql-script"></a>Zachovat hello R kódu v samostatném souboru a odkazovat na hello U-SQL skriptů

Hello následující příklad ukazuje použití složitější. V takovém případě hello R kód nasazen jako prostředek, který je hello skript U-SQL.

Uložte tento kód R jako samostatný soubor.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

Pomocí skriptu R toodeploy skript U-SQL s hello příkaz nasazení prostředků.

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

## <a name="how-r-integrates-with-u-sql"></a>Jak R integruje s jazykem U-SQL

### <a name="datatypes"></a>Datové typy
* Řetězec a číselné sloupce z U-SQL se převedou jako-mezi R DataFrame a jazykem U-SQL [podporované typy: `double`, `string`, `bool`, `integer`, `byte`].
* Hello `Factor` datový typ není podporován v U-SQL.
* `byte[]`musí být serializován jako kódováním base64 `string`.
* U-SQL řetězce může být převedený toofactors v kódu jazyka R, jakmile U-SQL vytvořit vstupní dataframe R nebo nastavení parametrem reduktorem hello `stringsAsFactors: true`.

### <a name="schemas"></a>Schémata
* Datové sady U-SQL nemůže mít duplicitní názvy sloupců.
* Názvy sloupců datových sad U-SQL musí být řetězce.
* Názvy sloupců musí být hello stejné skripty U-SQL a R.
* Sloupce jen pro čtení nemůže být součástí dataframe výstup hello. Protože sloupce jen pro čtení jsou automaticky vložit zpět v tabulce U-SQL hello, pokud je součástí výstupního schématu UDO.

### <a name="functional-limitations"></a>Funkční omezení
* Hello modul R nelze vytvořit instanci dvakrát v hello stejný postup. 
* U-SQL v současné době nepodporuje kombinační UDO pro použití oddílů modelů generována pomocí reduktorem UDO předpovědi. Uživatelé můžou deklarovat hello rozdělena na oddíly modely jako prostředek a používat je ve své skriptu R (viz ukázkový kód `ExtR_PredictUsingLMRawStringReducer.usql`)

### <a name="r-versions"></a>Verze R
Je podporován pouze R 3.2.2.

### <a name="standard-r-modules"></a>Standardní moduly R

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

### <a name="input-and-output-size-limitations"></a>Vstup a výstup omezení velikosti
Všechny vrcholy má omezené množství paměti přidělené tooit. Protože hello vstupní a výstupní DataFrames musí existovat v paměti v kódu hello R, hello celková velikost hello vstupní a výstupní nemůže překročit 500 MB.

### <a name="sample-code"></a>Ukázkový kód
Další ukázkový kód je k dispozici v účtu Data Lake Store, po instalaci rozšíření hello Advanced Analytics U-SQL. Hello cesta pro další ukázkový kód je: `<your_account_address>/usqlext/samples/R`. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>Nasazení vlastní R modulů s jazykem U-SQL

Nejprve vytvořit vlastní modul R a zip ho a pak nahrajte hello ZIP R vlastní modul souboru tooyour ADL úložiště. V příkladu hello jsme odešlete magittr_1.5.zip toohello kořenového účtu ADLS hello výchozí pro hello ADLA účet, který používáme. Jakmile nahrajete hello modulu tooADL úložiště, deklarujte ji jako použijte toomake nasazení prostředků je k dispozici v skript U-SQL a volání `install.packages` tooinstall ho.

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

## <a name="next-steps"></a>Další kroky
* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Pro úlohy Azure Data Lake Analytics pomocí U-SQL okno funkce](data-lake-analytics-use-window-functions.md)
