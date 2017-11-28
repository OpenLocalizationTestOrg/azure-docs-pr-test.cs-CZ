---
title: "aaaOperationalize vytvořené Spark modelů strojového učení | Microsoft Docs"
description: "Jak tooload a skóre učení modely uložené v Azure Blob Storage (WASB) s Python."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 626305a2-0abf-4642-afb0-dad0f6bd24e9
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: c5fadcb13257b94dcb28a522be454f6e03dfa991
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="operationalize-spark-built-machine-learning-models"></a><span data-ttu-id="9e0db-103">Zprovoznit learning modely vytvořené Spark počítače</span><span class="sxs-lookup"><span data-stu-id="9e0db-103">Operationalize Spark-built machine learning models</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="9e0db-104">Toto téma ukazuje, jak toooperationalize modelu uloženého strojového učení (ML) používá Python na HDInsight Spark clusterů.</span><span class="sxs-lookup"><span data-stu-id="9e0db-104">This topic shows how toooperationalize a saved machine learning model (ML) using Python on HDInsight Spark clusters.</span></span> <span data-ttu-id="9e0db-105">Popisuje, jak tooload počítač learning modely, které byly vytvořené pomocí Spark MLlib a uložené v Azure Blob Storage (WASB) a jak tooscore je s datové sady, které byly uloženy také v WASB.</span><span class="sxs-lookup"><span data-stu-id="9e0db-105">It describes how tooload machine learning models that have been built using Spark MLlib and stored in Azure Blob Storage (WASB), and how tooscore them with datasets that have also been stored in WASB.</span></span> <span data-ttu-id="9e0db-106">Zobrazuje jak toopre proces hello vstupní data, pomocí funkce transformace hello indexování a kódování funkce v sadě nástrojů MLlib hello a jak toocreate s popiskem bodu datový objekt, který lze použít jako vstup pro vyhodnocování s modely hello ML.</span><span class="sxs-lookup"><span data-stu-id="9e0db-106">It shows how toopre-process hello input data, transform features using hello indexing and encoding functions in hello MLlib toolkit, and how toocreate a labeled point data object that can be used as input for scoring with hello ML models.</span></span> <span data-ttu-id="9e0db-107">Hello modely použité pro vyhodnocování zahrnují lineární regrese, Logistic Regression, náhodné modely doménové struktury a modely stromu přechodu zvyšovat skóre.</span><span class="sxs-lookup"><span data-stu-id="9e0db-107">hello models used for scoring include Linear Regression, Logistic Regression, Random Forest Models, and Gradient Boosting Tree Models.</span></span>

## <a name="spark-clusters-and-jupyter-notebooks"></a><span data-ttu-id="9e0db-108">Clustery Spark a poznámkové bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="9e0db-108">Spark clusters and Jupyter notebooks</span></span>
<span data-ttu-id="9e0db-109">Postup instalace a toooperationalize kód hello ML model jsou uvedené v tomto názorném postupu pro používání clusteru služby HDInsight Spark 1.6, jakož i cluster Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="9e0db-109">Setup steps and hello code toooperationalize an ML model are provided in this walkthrough for using an HDInsight Spark 1.6 cluster as well as a Spark 2.0 cluster.</span></span> <span data-ttu-id="9e0db-110">Hello kód pro tyto postupy jsou také uvedené v poznámkové bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="9e0db-110">hello code for these procedures is also provided in Jupyter notebooks.</span></span>

### <a name="notebook-for-spark-16"></a><span data-ttu-id="9e0db-111">Poznámkový blok pro Spark 1.6</span><span class="sxs-lookup"><span data-stu-id="9e0db-111">Notebook for Spark 1.6</span></span>
<span data-ttu-id="9e0db-112">Hello [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Poznámkový blok Jupyter ukazuje, jak toooperationalize uložené model používá Python v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="9e0db-112">hello [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter notebook shows how toooperationalize a saved model using Python on HDInsight clusters.</span></span> 

### <a name="notebook-for-spark-20"></a><span data-ttu-id="9e0db-113">Poznámkový blok pro Spark 2.0</span><span class="sxs-lookup"><span data-stu-id="9e0db-113">Notebook for Spark 2.0</span></span>
<span data-ttu-id="9e0db-114">hello toomodify Poznámkový blok Jupyter pro toouse Spark 1.6 pomocí clusteru služby HDInsight Spark 2.0 nahradit soubor kód Python hello s [tento soubor](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span><span class="sxs-lookup"><span data-stu-id="9e0db-114">toomodify hello Jupyter notebook for Spark 1.6 toouse with an HDInsight Spark 2.0 cluster, replace hello Python code file with [this file](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span></span> <span data-ttu-id="9e0db-115">Tento kód ukazuje, jak tooconsume hello modely vytvořené v Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="9e0db-115">This code shows how tooconsume hello models created in Spark 2.0.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="9e0db-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e0db-116">Prerequisites</span></span>

1. <span data-ttu-id="9e0db-117">Budete potřebovat účet Azure a Spark 1.6 (nebo Spark 2.0) cluster HDInsight toocomplete tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="9e0db-117">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster toocomplete this walkthrough.</span></span> <span data-ttu-id="9e0db-118">V tématu hello [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny toosatisfy tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="9e0db-118">See hello [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how toosatisfy these requirements.</span></span> <span data-ttu-id="9e0db-119">Toto téma obsahuje také popis hello NYC 2013 taxíkem dat použít se zde a pokyny, jak tooexecute kód z poznámkového bloku Jupyter v clusteru Spark hello.</span><span class="sxs-lookup"><span data-stu-id="9e0db-119">That topic also contains a description of hello NYC 2013 Taxi data used here and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster.</span></span> 
2. <span data-ttu-id="9e0db-120">Musíte taky vytvořit hello modely machine learning toobe skóre pro magnitudu zde projdete hello [zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) téma je určené pro cluster hello Spark 1.6 nebo poznámkových bloků hello Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="9e0db-120">You must also create hello machine learning models toobe scored here by working through hello [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic for hello Spark 1.6 cluster or hello Spark 2.0 notebooks.</span></span> 
3. <span data-ttu-id="9e0db-121">poznámkových bloků Hello Spark 2.0 používat další datové sady pro hello klasifikace úkol hello dobře známé letecká společnost na čas odeslání datové sady z 2011 a 2012.</span><span class="sxs-lookup"><span data-stu-id="9e0db-121">hello Spark 2.0 notebooks use an additional data set for hello classification task, hello well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="9e0db-122">Popis toothem hello poznámkových bloků a odkazy jsou uvedeny v hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) hello Githubu úložiště, které je obsahují.</span><span class="sxs-lookup"><span data-stu-id="9e0db-122">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="9e0db-123">Kromě toho hello kód sem a v poznámkových bloků hello propojené je obecný a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="9e0db-123">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="9e0db-124">Pokud nepoužíváte HDInsight Spark, může být kroky instalace a správy clusteru hello mírně lišit od co se zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="9e0db-124">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="9e0db-125">Instalační program: umístění úložiště, knihovny a hello přednastavení Spark kontextu</span><span class="sxs-lookup"><span data-stu-id="9e0db-125">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="9e0db-126">Spark je možné tooan tooread a zápisu objektů Blob Azure Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="9e0db-126">Spark is able tooread and write tooan Azure Storage Blob (WASB).</span></span> <span data-ttu-id="9e0db-127">Takže existující data v ní uloženy může zpracovat pomocí Spark a hello výsledky uložené v znovu WASB.</span><span class="sxs-lookup"><span data-stu-id="9e0db-127">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="9e0db-128">modely toosave či soubory v WASB, musí cesta hello toobe zadán správně.</span><span class="sxs-lookup"><span data-stu-id="9e0db-128">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="9e0db-129">Hello clusteru Spark toohello výchozí kontejner připojené můžete odkazovat pomocí cesty počínaje: *"wasb / /"*.</span><span class="sxs-lookup"><span data-stu-id="9e0db-129">hello default container attached toohello Spark cluster can be referenced using a path beginning with: *"wasb//"*.</span></span> <span data-ttu-id="9e0db-130">Hello následující ukázka kódu určuje hello umístění toobe hello dat pro čtení a uložena hello cesta pro model výstup hello directory toowhich hello modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e0db-130">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved.</span></span> 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="9e0db-131">Nastavení cesty adresáře pro umístění úložiště v WASB</span><span class="sxs-lookup"><span data-stu-id="9e0db-131">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="9e0db-132">Modely se ukládají do: "wasb: / / / uživatel/remoteuser/NYCTaxi/modely".</span><span class="sxs-lookup"><span data-stu-id="9e0db-132">Models are saved in: "wasb:///user/remoteuser/NYCTaxi/Models".</span></span> <span data-ttu-id="9e0db-133">Pokud tato cesta není správně nastavena, modely nenačtou pro vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="9e0db-133">If this path is not set properly, models are not loaded for scoring.</span></span>

<span data-ttu-id="9e0db-134">Hello scored výsledky byly uloženy v: "wasb: / / / uživatel/remoteuser/NYCTaxi/ScoredResults".</span><span class="sxs-lookup"><span data-stu-id="9e0db-134">hello scored results have been saved in: "wasb:///user/remoteuser/NYCTaxi/ScoredResults".</span></span> <span data-ttu-id="9e0db-135">Pokud cesta toofolder hello není v pořádku, výsledky nejsou uloženy v této složce.</span><span class="sxs-lookup"><span data-stu-id="9e0db-135">If hello path toofolder is incorrect, results are not saved in that folder.</span></span>   

> [!NOTE]
> <span data-ttu-id="9e0db-136">umístění souboru cest Hello lze kopírovat a vložit do zástupné symboly hello tento kód hello výstup hello poslední buňku hello **machine-learning-data-science-spark-data-exploration-modeling.ipynb** poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="9e0db-136">hello file path locations can be copied and pasted into hello placeholders in this code from hello output of hello last cell of hello **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebook.</span></span>   
> 
> 

<span data-ttu-id="9e0db-137">Tady je cesty adresářů tooset hello kódu:</span><span class="sxs-lookup"><span data-stu-id="9e0db-137">Here is hello code tooset directory paths:</span></span> 

    # LOCATION OF DATA tooBE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE hello LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE hello LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR hello MODELS tooBE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="9e0db-138">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-138">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-139">DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)</span><span class="sxs-lookup"><span data-stu-id="9e0db-139">datetime.datetime(2016, 4, 25, 23, 56, 19, 229403)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="9e0db-140">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="9e0db-140">Import libraries</span></span>
<span data-ttu-id="9e0db-141">Nastavit kontext spark a importovat potřebné knihovny s hello následující kód</span><span class="sxs-lookup"><span data-stu-id="9e0db-141">Set spark context and import necessary libraries with hello following code</span></span>

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="9e0db-142">Předvolby kontextu Spark a Magic PySpark</span><span class="sxs-lookup"><span data-stu-id="9e0db-142">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="9e0db-143">Hello jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu.</span><span class="sxs-lookup"><span data-stu-id="9e0db-143">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="9e0db-144">Proto není nutné tooset kontexty Spark nebo Hive hello explicitně před zahájením práce s hello aplikací, které vyvíjíte.</span><span class="sxs-lookup"><span data-stu-id="9e0db-144">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="9e0db-145">Tyto jsou dostupné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9e0db-145">These are available for you by default.</span></span> <span data-ttu-id="9e0db-146">Tyto kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="9e0db-146">These contexts are:</span></span>

* <span data-ttu-id="9e0db-147">sc - pro Spark</span><span class="sxs-lookup"><span data-stu-id="9e0db-147">sc - for Spark</span></span> 
* <span data-ttu-id="9e0db-148">sqlContext - pro Hive</span><span class="sxs-lookup"><span data-stu-id="9e0db-148">sqlContext - for Hive</span></span>

<span data-ttu-id="9e0db-149">Hello jádra PySpark poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%.</span><span class="sxs-lookup"><span data-stu-id="9e0db-149">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="9e0db-150">Existují dva takové příkazy, které se používají v tyto ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="9e0db-150">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="9e0db-151">**%% místní** zadat, že je kód hello v dalších řádcích spuštěn místně.</span><span class="sxs-lookup"><span data-stu-id="9e0db-151">**%%local** Specified that hello code in subsequent lines is executed locally.</span></span> <span data-ttu-id="9e0db-152">Kód musí být platný kód Python.</span><span class="sxs-lookup"><span data-stu-id="9e0db-152">Code must be valid Python code.</span></span>
* <span data-ttu-id="9e0db-153">**%% sql -o<variable name>**</span><span class="sxs-lookup"><span data-stu-id="9e0db-153">**%%sql -o <variable name>**</span></span> 
* <span data-ttu-id="9e0db-154">Provede dotaz Hive proti hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="9e0db-154">Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="9e0db-155">Pokud není předán parametr -o hello hello výsledek dotazu hello je uchován v hello %% lokální kontext Python jako Pandas dataframe.</span><span class="sxs-lookup"><span data-stu-id="9e0db-155">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas dataframe.</span></span>

<span data-ttu-id="9e0db-156">Pro další informace o hello jádra pro poznámkové bloky Jupyter a hello předdefinované "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="9e0db-156">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a><span data-ttu-id="9e0db-157">Načítání dat a vytvořte rámeček vyčištěnými dat</span><span class="sxs-lookup"><span data-stu-id="9e0db-157">Ingest data and create a cleaned data frame</span></span>
<span data-ttu-id="9e0db-158">Tato část obsahuje hello kód pro řadu úloh vyžaduje tooingest hello data toobe vypočítat skóre.</span><span class="sxs-lookup"><span data-stu-id="9e0db-158">This section contains hello code for a series of tasks required tooingest hello data toobe scored.</span></span> <span data-ttu-id="9e0db-159">Čtení v připojené k ukázce 0,1 % hello taxíkem služební cestě a tarif souboru (uložený jako soubor TSV), formát hello dat a poté vytvoří vyčištění dat rámce.</span><span class="sxs-lookup"><span data-stu-id="9e0db-159">Read in a joined 0.1% sample of hello taxi trip and fare file (stored as a .tsv file), format hello data, and then creates a clean data frame.</span></span>

<span data-ttu-id="9e0db-160">Hello taxíkem služební cestě a tarif soubory byly spojené na základě na hello postup uvedený v: [hello proces vědecké účely Team dat v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="9e0db-160">hello taxi trip and fare files were joined based on hello procedure provided in the: [hello Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) topic.</span></span>

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="9e0db-161">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-161">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-162">Doba trvání tooexecute nad buňku: 46.37 sekund</span><span class="sxs-lookup"><span data-stu-id="9e0db-162">Time taken tooexecute above cell: 46.37 seconds</span></span>

## <a name="prepare-data-for-scoring-in-spark"></a><span data-ttu-id="9e0db-163">Příprava dat pro vyhodnocování v Spark</span><span class="sxs-lookup"><span data-stu-id="9e0db-163">Prepare data for scoring in Spark</span></span>
<span data-ttu-id="9e0db-164">Tato část popisuje, jak tooindex, kódovat a škálování kategorií funkce tooprepare je pro použití v algoritmů učení MLlib pod dohledem pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="9e0db-164">This section shows how tooindex, encode, and scale categorical features tooprepare them for use in MLlib supervised learning algorithms for classification and regression.</span></span>

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a><span data-ttu-id="9e0db-165">Funkce transformace: index a kódování kategorií funkce pro vstup do modely pro vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="9e0db-165">Feature transformation: index and encode categorical features for input into models for scoring</span></span>
<span data-ttu-id="9e0db-166">Tato část uvádí, jak tooindex kategorizovaná data pomocí `StringIndexer` a kódování funkcí s `OneHotEncoder` vstup na modely hello.</span><span class="sxs-lookup"><span data-stu-id="9e0db-166">This section shows how tooindex categorical data using a `StringIndexer` and encode features with `OneHotEncoder` input into hello models.</span></span>

<span data-ttu-id="9e0db-167">Hello [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kóduje sloupec řetězce popisky tooa sloupce popisek indexů.</span><span class="sxs-lookup"><span data-stu-id="9e0db-167">hello [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) encodes a string column of labels tooa column of label indices.</span></span> <span data-ttu-id="9e0db-168">Hello indexy, které jsou seřazené podle četnosti popisek.</span><span class="sxs-lookup"><span data-stu-id="9e0db-168">hello indices are ordered by label frequencies.</span></span> 

<span data-ttu-id="9e0db-169">Hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mapuje sloupec popisek indexy tooa sloupce binární vektorů s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9e0db-169">hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="9e0db-170">Toto kódování umožňuje algoritmy, které očekávají průběžné cenná funkce, jako je logistic regression funkce toocategorical toobe použít.</span><span class="sxs-lookup"><span data-stu-id="9e0db-170">This encoding allows algorithms that expect continuous valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()

    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="9e0db-171">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-171">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-172">Doba trvání tooexecute nad buňku: 5.37 sekund</span><span class="sxs-lookup"><span data-stu-id="9e0db-172">Time taken tooexecute above cell: 5.37 seconds</span></span>

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a><span data-ttu-id="9e0db-173">Vytvoření RDD objektů s poli funkce pro vstup na modely</span><span class="sxs-lookup"><span data-stu-id="9e0db-173">Create RDD objects with feature arrays for input into models</span></span>
<span data-ttu-id="9e0db-174">Tato část obsahuje kód, který popisuje, jak tooindex kategorií textová data jako RDD objektů a jeden horkou zakódovat je tedy možné ji použít tootrain a testování MLlib logistic regression a na základě stromu modely.</span><span class="sxs-lookup"><span data-stu-id="9e0db-174">This section contains code that shows how tooindex categorical text data as an RDD object and one-hot encode it so it can be used tootrain and test MLlib logistic regression and tree-based models.</span></span> <span data-ttu-id="9e0db-175">Hello indexované data jsou uložena v [odolné distribuované datovou sadu (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objekty.</span><span class="sxs-lookup"><span data-stu-id="9e0db-175">hello indexed data is stored in [Resilient Distributed Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objects.</span></span> <span data-ttu-id="9e0db-176">Jedná se o základní abstrakce hello v Spark.</span><span class="sxs-lookup"><span data-stu-id="9e0db-176">These are hello basic abstraction in Spark.</span></span> <span data-ttu-id="9e0db-177">Objekt RDD představuje kolekci neměnné, oddílů elementů, které lze provozovat na paralelně s Spark.</span><span class="sxs-lookup"><span data-stu-id="9e0db-177">An RDD object represents an immutable, partitioned collection of elements that can be operated on in parallel with Spark.</span></span>

<span data-ttu-id="9e0db-178">Také obsahuje kód, který ukazuje, jak tooscale dat pomocí hello `StandardScalar` poskytované MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD), oblíbených algoritmus pro trénování širokou škálu modely machine learning.</span><span class="sxs-lookup"><span data-stu-id="9e0db-178">It also contains code that shows how tooscale data with hello `StandardScalar` provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of machine learning models.</span></span> <span data-ttu-id="9e0db-179">Hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) je použité tooscale hello funkce toounit odchylky.</span><span class="sxs-lookup"><span data-stu-id="9e0db-179">hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) is used tooscale hello features toounit variance.</span></span> <span data-ttu-id="9e0db-180">Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle hello.</span><span class="sxs-lookup"><span data-stu-id="9e0db-180">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> 

    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)

    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)

    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)

    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="9e0db-181">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-181">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-182">Doba trvání tooexecute nad buňku: 11.72 sekund</span><span class="sxs-lookup"><span data-stu-id="9e0db-182">Time taken tooexecute above cell: 11.72 seconds</span></span>

## <a name="score-with-hello-logistic-regression-model-and-save-output-tooblob"></a><span data-ttu-id="9e0db-183">Stanovení skóre s hello Logistic regresní Model a uložte tooblob výstup</span><span class="sxs-lookup"><span data-stu-id="9e0db-183">Score with hello Logistic Regression Model and save output tooblob</span></span>
<span data-ttu-id="9e0db-184">Hello kód v této části popisuje, jak úložiště objektů blob tooload Logistic regresní Model, který byl uložen v Azure a použít na cestě taxíkem toopredict, jestli je tip placené, skóre s standardní klasifikace metriky a potom uložte a vykreslení tooblob výsledky hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e0db-184">hello code in this section shows how tooload a Logistic Regression Model that has been saved in Azure blob storage and use it toopredict whether or not a tip is paid on a taxi trip, score it with standard classification metrics, and then save and plot hello results tooblob storage.</span></span> <span data-ttu-id="9e0db-185">Hello skóre pro magnitudu výsledky jsou uloženy v RDD objekty.</span><span class="sxs-lookup"><span data-stu-id="9e0db-185">hello scored results are stored in RDD objects.</span></span> 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) tooBLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="9e0db-186">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-186">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-187">Doba trvání tooexecute nad buňku: 19.22 sekund</span><span class="sxs-lookup"><span data-stu-id="9e0db-187">Time taken tooexecute above cell: 19.22 seconds</span></span>

## <a name="score-a-linear-regression-model"></a><span data-ttu-id="9e0db-188">Určení skóre modelu lineární regrese</span><span class="sxs-lookup"><span data-stu-id="9e0db-188">Score a Linear Regression Model</span></span>
<span data-ttu-id="9e0db-189">Použili jsme [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain placené model lineární regrese pomocí Stochastického přechodu klesání (SGD) pro optimalizaci toopredict hello množství tip.</span><span class="sxs-lookup"><span data-stu-id="9e0db-189">We used [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain a linear regression model using Stochastic Gradient Descent (SGD) for optimization toopredict hello amount of tip paid.</span></span> 

<span data-ttu-id="9e0db-190">Hello kód v této části ukazuje, jak tooload Model lineární regrese z Azure blob storage, stanovení skóre pomocí škálovat proměnných a potom uložte hello výsledky zpět toohello objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9e0db-190">hello code in this section shows how tooload a Linear Regression Model from Azure blob storage, score using scaled variables, and then save hello results back toohello blob.</span></span>

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel

    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="9e0db-191">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-191">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-192">Doba trvání tooexecute nad buňku: 16.63 sekund</span><span class="sxs-lookup"><span data-stu-id="9e0db-192">Time taken tooexecute above cell: 16.63 seconds</span></span>

## <a name="score-classification-and-regression-random-forest-models"></a><span data-ttu-id="9e0db-193">Stanovení skóre klasifikace a regrese náhodných modely doménové struktury</span><span class="sxs-lookup"><span data-stu-id="9e0db-193">Score classification and regression Random Forest Models</span></span>
<span data-ttu-id="9e0db-194">Hello kód v této části ukazuje, jak uložit tooload hello klasifikace a regrese náhodných doménové struktury modely uloží do úložiště objektů blob v Azure, stanovení skóre výkonu s standardní třídění a regrese míry a potom uložte hello výsledky zpět tooblob úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e0db-194">hello code in this section shows how tooload hello saved classification and regression Random Forest Models saved in Azure blob storage, score their performance with standard classifier and regression measures, and then save hello results back tooblob storage.</span></span>

<span data-ttu-id="9e0db-195">[Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="9e0db-195">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="9e0db-196">Že kombinují mnoho rozhodovací stromy tooreduce hello riziko overfitting.</span><span class="sxs-lookup"><span data-stu-id="9e0db-196">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="9e0db-197">Náhodné doménových struktur dokáže zpracovat kategorií funkce rozšířit nastavení toohello více třídami klasifikace, nevyžadují funkce škálování a jsou možné toocapture nelineárností a funkce, interakce.</span><span class="sxs-lookup"><span data-stu-id="9e0db-197">Random forests can handle categorical features, extend toohello multiclass classification setting, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="9e0db-198">Náhodné doménových strukturách jsou jedním z hello těch nejúspěšnějších modelů strojového učení pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="9e0db-198">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>

<span data-ttu-id="9e0db-199">[Spark.mllib](http://spark.apache.org/mllib/) podporuje náhodných doménové struktury pro více třídami a binární klasifikaci a pro regresní pomocí funkce nepřetržitý a kategorií.</span><span class="sxs-lookup"><span data-stu-id="9e0db-199">[spark.mllib](http://spark.apache.org/mllib/) supports random forests for binary and multiclass classification and for regression, using both continuous and categorical features.</span></span> 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="9e0db-200">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-200">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-201">Doba trvání tooexecute nad buňku: 31.07 sekund</span><span class="sxs-lookup"><span data-stu-id="9e0db-201">Time taken tooexecute above cell: 31.07 seconds</span></span>

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a><span data-ttu-id="9e0db-202">Stanovení skóre klasifikace a regrese přechodu zvyšovat skóre stromu modely</span><span class="sxs-lookup"><span data-stu-id="9e0db-202">Score classification and regression Gradient Boosting Tree Models</span></span>
<span data-ttu-id="9e0db-203">Hello kód v této části ukazuje, jak tooload klasifikace a regrese přechodu zvyšovat skóre stromu modely z Azure blob storage, stanovení skóre výkonu s standardní třídění a regrese míry a potom uložte hello výsledky zpět tooblob úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e0db-203">hello code in this section shows how tooload classification and regression Gradient Boosting Tree Models from Azure blob storage, score their performance with standard classifier and regression measures, and then save hello results back tooblob storage.</span></span> 

<span data-ttu-id="9e0db-204">**Spark.mllib** podporuje GBTs pro binární klasifikaci a pro regresní pomocí funkce nepřetržitý a kategorií.</span><span class="sxs-lookup"><span data-stu-id="9e0db-204">**spark.mllib** supports GBTs for binary classification and for regression, using both continuous and categorical features.</span></span> 

<span data-ttu-id="9e0db-205">[Přechodu zvyšovat skóre stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="9e0db-205">[Gradient Boosting Trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="9e0db-206">GBTs train rozhodovací stromy interaktivně toominimize funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="9e0db-206">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="9e0db-207">GBTs může zpracovat kategorií funkce, nevyžadují funkce škálování a jsou možné toocapture nelineárností a funkce, interakce.</span><span class="sxs-lookup"><span data-stu-id="9e0db-207">GBTs can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="9e0db-208">Můžete také používají v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="9e0db-208">They can also be used in a multiclass-classification setting.</span></span>

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB

    #LOAD AND SCORE hello MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="9e0db-209">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-209">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-210">Doba trvání tooexecute nad buňku: 14.6 sekund</span><span class="sxs-lookup"><span data-stu-id="9e0db-210">Time taken tooexecute above cell: 14.6 seconds</span></span>

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a><span data-ttu-id="9e0db-211">Vyčištění objektů z paměti a tisk skóre pro magnitudu umístění souborů</span><span class="sxs-lookup"><span data-stu-id="9e0db-211">Clean up objects from memory and print scored file locations</span></span>
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH tooSCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


<span data-ttu-id="9e0db-212">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="9e0db-212">**OUTPUT:**</span></span>

<span data-ttu-id="9e0db-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span><span class="sxs-lookup"><span data-stu-id="9e0db-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span></span>

<span data-ttu-id="9e0db-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span><span class="sxs-lookup"><span data-stu-id="9e0db-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span></span>

<span data-ttu-id="9e0db-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span><span class="sxs-lookup"><span data-stu-id="9e0db-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span></span>

<span data-ttu-id="9e0db-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span><span class="sxs-lookup"><span data-stu-id="9e0db-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span></span>

<span data-ttu-id="9e0db-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span><span class="sxs-lookup"><span data-stu-id="9e0db-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span></span>

<span data-ttu-id="9e0db-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span><span class="sxs-lookup"><span data-stu-id="9e0db-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span></span>

## <a name="consume-spark-models-through-a-web-interface"></a><span data-ttu-id="9e0db-219">Využívat modely Spark pomocí webového rozhraní</span><span class="sxs-lookup"><span data-stu-id="9e0db-219">Consume Spark Models through a web interface</span></span>
<span data-ttu-id="9e0db-220">Spark poskytuje mechanismus tooremotely odeslání dávkových úloh nebo interaktivních dotazů prostřednictvím REST rozhraní s komponenty s názvem Livy.</span><span class="sxs-lookup"><span data-stu-id="9e0db-220">Spark provides a mechanism tooremotely submit batch jobs or interactive queries through a REST interface with a component called Livy.</span></span> <span data-ttu-id="9e0db-221">Livy je povoleno ve výchozím nastavení v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="9e0db-221">Livy is enabled by default on your HDInsight Spark cluster.</span></span> <span data-ttu-id="9e0db-222">Další informace o Livy najdete v tématu: [úlohy odeslání Spark vzdáleně pomocí Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="9e0db-222">For more information on Livy, see: [Submit Spark jobs remotely using Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span></span> 

<span data-ttu-id="9e0db-223">Můžete použít Livy tooremotely odeslat úlohu, která dávky skóre soubor, který je uložený v objektu blob Azure a pak zapíše hello výsledky tooanother objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9e0db-223">You can use Livy tooremotely submit a job that batch scores a file that is stored in an Azure blob and then writes hello results tooanother blob.</span></span> <span data-ttu-id="9e0db-224">toodo, můžete nahrávat na server hello skript v jazyce Python z</span><span class="sxs-lookup"><span data-stu-id="9e0db-224">toodo this, you upload hello Python script from</span></span>  
<span data-ttu-id="9e0db-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob clusteru Spark hello.</span><span class="sxs-lookup"><span data-stu-id="9e0db-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob of hello Spark cluster.</span></span> <span data-ttu-id="9e0db-226">Můžete použít nástroje, jako je **Microsoft Azure Storage Explorer** nebo **AzCopy** toocopy hello skriptu toohello clusteru blob.</span><span class="sxs-lookup"><span data-stu-id="9e0db-226">You can use a tool like **Microsoft Azure Storage Explorer** or **AzCopy** toocopy hello script toohello cluster blob.</span></span> <span data-ttu-id="9e0db-227">V našem případě jsme příliš nahrán hello skriptu***wasb:///example/python/ConsumeGBNYCReg.py***.</span><span class="sxs-lookup"><span data-stu-id="9e0db-227">In our case we uploaded hello script too***wasb:///example/python/ConsumeGBNYCReg.py***.</span></span>   

> [!NOTE]
> <span data-ttu-id="9e0db-228">Hello přístupové klíče, které můžete potřebovat naleznete na portálu hello pro účet úložiště hello spojené s clusterem Spark hello.</span><span class="sxs-lookup"><span data-stu-id="9e0db-228">hello access keys that you need can be found on hello portal for hello storage account associated with hello Spark cluster.</span></span> 
> 
> 

<span data-ttu-id="9e0db-229">Po nahrání toothis umístění tento skript se spouští v rámci clusteru Spark hello v distribuované kontextu.</span><span class="sxs-lookup"><span data-stu-id="9e0db-229">Once uploaded toothis location, this script runs within hello Spark cluster in a distributed context.</span></span> <span data-ttu-id="9e0db-230">Načítá hello modelu a spouští předpovědi na vstupní soubory na základě modelu hello.</span><span class="sxs-lookup"><span data-stu-id="9e0db-230">It loads hello model and runs predictions on input files based on hello model.</span></span>  

<span data-ttu-id="9e0db-231">Tento skript můžete spustit vzdáleně pomocí jednoduchého požadavku HTTPS nebo REST na Livy.</span><span class="sxs-lookup"><span data-stu-id="9e0db-231">You can invoke this script remotely by making a simple HTTPS/REST request on Livy.</span></span>  <span data-ttu-id="9e0db-232">Zde je vzdáleně curl příkaz tooconstruct hello HTTP žádost tooinvoke hello skript v jazyce Python.</span><span class="sxs-lookup"><span data-stu-id="9e0db-232">Here is a curl command tooconstruct hello HTTP request tooinvoke hello Python script remotely.</span></span> <span data-ttu-id="9e0db-233">Nahraďte CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME hello odpovídající hodnoty pro váš cluster Spark.</span><span class="sxs-lookup"><span data-stu-id="9e0db-233">Replace CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME with hello appropriate values for your Spark cluster.</span></span>

    # CURL COMMAND tooINVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

<span data-ttu-id="9e0db-234">Můžete použít jakýkoli jazyk u hello vzdálený systém tooinvoke hello Spark úlohy pomocí Livy tím, že jednoduché volání HTTPS se základním ověřováním.</span><span class="sxs-lookup"><span data-stu-id="9e0db-234">You can use any language on hello remote system tooinvoke hello Spark job through Livy by making a simple HTTPS call with Basic Authentication.</span></span>   

> [!NOTE]
> <span data-ttu-id="9e0db-235">Je vhodné toouse hello Python požadavky knihovny při provádění této volání protokolu HTTP, ale není momentálně nainstalována ve výchozím nastavení v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9e0db-235">It would be convenient toouse hello Python Requests library when making this HTTP call, but it is not currently installed by default in Azure Functions.</span></span> <span data-ttu-id="9e0db-236">Aby se místo toho používat starší knihovny HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e0db-236">So older HTTP libraries are used instead.</span></span>   
> 
> 

<span data-ttu-id="9e0db-237">Zde je kód Python hello hello HTTP volání:</span><span class="sxs-lookup"><span data-stu-id="9e0db-237">Here is hello Python code for hello HTTP call:</span></span>

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF hello REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY hello PYTHON SCRIPT tooRUN ON hello SPARK CLUSTER
    # IN hello FILE PARAMETER OF hello JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


<span data-ttu-id="9e0db-238">Můžete také přidat tento kód Python příliš[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger odeslání úlohy Spark, která skóre objekt blob podle různých událostí jako časovače, vytvoření nebo aktualizace objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9e0db-238">You can also add this Python code too[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger a Spark job submission that scores a blob based on various events like a timer, creation, or update of a blob.</span></span> 

<span data-ttu-id="9e0db-239">Pokud dáváte přednost volné klientského prostředí kódu, použijte hello [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark dávkového vyhodnocování definováním akce HTTP na hello **logiku aplikace Návrhář** a nastavení jeho parametry.</span><span class="sxs-lookup"><span data-stu-id="9e0db-239">If you prefer a code free client experience, use hello [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark batch scoring by defining an HTTP action on hello **Logic Apps Designer** and setting its parameters.</span></span> 

* <span data-ttu-id="9e0db-240">Z portálu Azure vytvořit novou aplikaci logiky výběrem **+ nový** -> **Web + mobilní** -> **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="9e0db-240">From Azure portal, create a new Logic App by selecting **+New** -> **Web + Mobile** -> **Logic App**.</span></span> 
* <span data-ttu-id="9e0db-241">toobring až hello **logiku aplikace Návrhář**, zadejte název hello hello aplikace logiky a plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9e0db-241">toobring up hello **Logic Apps Designer**, enter hello name of hello Logic App and App Service Plan.</span></span>
* <span data-ttu-id="9e0db-242">Vyberte akci HTTP a zadejte parametry hello znázorňuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="9e0db-242">Select an HTTP action and enter hello parameters shown in hello following figure:</span></span>

![Návrhář aplikace logiky](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a><span data-ttu-id="9e0db-244">Co dále?</span><span class="sxs-lookup"><span data-stu-id="9e0db-244">What's next?</span></span>
<span data-ttu-id="9e0db-245">**Křížové ověření a hyperparameter (vymetání) komínů**: najdete v části [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí sweeping křížové ověření a technologie hyper parametr.</span><span class="sxs-lookup"><span data-stu-id="9e0db-245">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping.</span></span>

